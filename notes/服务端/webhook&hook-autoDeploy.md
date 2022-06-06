### webhook

##### 原理

webhook的作用是在每次代码仓库出现更改的时候发送一个post请求到指定的地址，可以通过这个请求来完成持续集成自动化部署

##### 操作步骤

1. 查看云服务器的公网ip，用于之后 webhook 发送请求的地址
2. 创建一个项目提交到github
3. 在github的setting里面添加一个webhook，url指向云服务器公网ip里面的某个目录（这个目录放着webhook脚本）
4. 服务器上安装docker，node，git，pm2相关环境
5. 编写一个webhook脚本（包含webhook监控脚本和sh脚本部署指令）提交到github
6. 把webhook脚本服务上传到服务器，放在github-webhook设置的目录下
7. 服务器把webhook服务跑起来监控github代码提交后发送的构建请求
8. 测试自动化部署效果



### webhook具体流程

> 基础环境安装



首先给云服务器安装docker
根据 [docker 官网](https://link.juejin.cn/?target=https%3A%2F%2Fdocs.docker.com%2Fengine%2Finstall%2Fcentos%2F) 的安装教程，运行以下命令

```shell
# Step 1: 安装必要的一些系统工具
sudo yum install -y yum-utils
# Step 2: 添加软件源信息，使用阿里云镜像
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# Step 3: 安装 docker-ce
sudo yum install docker-ce docker-ce-cli containerd.io
# Step 4: 开启 docker服务
sudo systemctl start docker
# Step 5: 运行 hello-world 项目
sudo docker run hello-world

```



进入docker，安装git node nginx等环境

```shell
yum install git
```

这里用 nvm 来管理 node 版本

```shell
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
```

接着需要将 nvm 作为环境变量

```shell
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

```

通过 nvm 安装最新版 node

```shell
nvm install node
```

node 安装完成后，还需要安装 `pm2`，它能使你的 js 脚本能在云服务器的后台运行

PM2是node进程管理工具，可以利用它来简化很多node应用管理的繁琐任务，如性能监控、自动重启、负载均衡等，而且使用非常简单。

```shell
npm i pm2 -g
```



> 搭建服务器脚本用于监听webhook实现自动化部署 这里使用node



监听脚本流程：

1. 创建一个脚本项目，上传到服务器

2. 新建一个脚本文件用于监听webhook

3. 新建一个sh脚本文件，用于编写部署指令

4. 用pm2运行脚本文件，实现监控服务

   ```shell
   pm2 start index.js
   ```

   

5. 提交代码进行测试
6. 注意: webhooks中需要填写一个secret,这个密钥需要于后面编写的webHook脚本的指定的密钥相同，否则之后进行签名验证的时候会出现错误



开始编写webhook服务接口，应对github的webhooks请求

```js
let http = require("http")
let crypto = require("crypto")
let spawn = require("child_process")
let sendMail = require("./sendMail")
const SECRET = "chenSir123"

// 计算数字签名
function sig(body){
    return `sha1=` + crypto.createHmac("sha1", SECRET).update(body).digest('hex');
}

let server = http.createServer(function(req, res){
    if(req.method == "POST" && req.url == "/webhook"){
        let buffers = [];
        req.on("data", function(buffer){
            buffers.push(buffer)
        })
        req.on('end', function(buffer){
            let body = Buffer.concat(buffers)
            // 解析webhook的请求
            let event = req.headers['X-github-event'];
            // github请求到来的时候，要传递一个body，还会带一个signature过来，需要验证签名是否正确
            let signature = req.headers['x-hub-signature'];
            if(signature !== sig(body)){
                return res.end('Not Allowed');
            }
            res.setHeader('Content-Type', 'application/json');
            res.end(JSON.stringify({ok: true}))
            if(event === "push"){
                let payload = JSON.parse(body);
                // 开启子进程执行对应仓库的sh脚本
                let child = spawn('sh', [`./${payload.repository.name}.sh`]);
                let buffers = [];
                child.stdout.on("data", function(buffer){
                    buffers.push(buffer)
                });
                child.stdout.on("end", function(buffer){
                    let log = Buffer.concat(buffers).toString();
                    // 发送邮件通知
                    sendMail(`
                        <h1>部署日期: ${new Date()}</h1>
                        <h2>部署人: ${payload.pusher.name}</h2>
                        <h2>部署邮箱: ${payload.pusher.email}</h2>
                        <h2>提交信息: ${payload.head_commit && payload.head_commit}</h2>
                        <h2>部署日志: ${log.replace("\r\n", "<br />")}</h2>
                    `)
                })
            }
        })
    }else{
        res.end("Not Found")
    }
})

server.listen(4000, () => {
    console.log("webhook 启动在4000端口")
})

```

编写邮件发送代码:

```js
// 编写邮件发送代码:
const nodemailer = require("nodemailer")
let transporter = nodemailer.createTransport({
    host: "smtp.qq.com",
    port: 465,
    secure: true,
    auth: {
        user: "1084983891@qq.com",
        pass: "这里是邮箱的授权码"
    }
})


function sendMail(message){
    let mailOptions = {
        from: "1084983891@qq.com",
        to: "1084983891@qq.com",
        subject: "部署通知",
        html: message
    }
    transporter.sendMail(mailOptions, (err, info) => {
        if(err){
            return console.log(err)
        }
        console.log('Message sent: %s', info.messageId)
    })
}

module.exports = sendMail
```



sh脚本

```shell
#!/bin/bash
cd '/usr/projects/vue-front'
echo '先清除老代码'
git reset --hard origin/master
git clean -f
echo '拉去新代码'
git pull origin master
echo '安装node_modules'
npm install
echo '编译代码'
npm run build
echo '开始执行构建'
docker build -t vue-front:1.0 .
echo '停止旧容器&&删除旧容器'
docker stop vue-front-container
docker rm vue-front-container
echo '启动新容器'
docker container run -p 80:80 --name vue-front-container -d vue-front:1.0


```



Dockerfile

```shell
FROM nginx
LABEL name='yyx-vue-front'
LABEL version='0.0.1'
COPY ./dist /usr/share/nginx/html
COPY .vue.conf /etc/nginx/default.d
EXPOSE 80

```

nginx.conf

```shell
server  {
    listen 80;
    server_name 139.198.174.89;
    location / {
        root /usr/share/nginx/html;
        index index.html index.htm;
        try_files $uri $uri/ /index.html;
    }
    location /api {
        proxy_pass http://139.198.174.89:3000;
    }
}

```



最后一步，执行webhook脚本

```shell
#进入服务器，git clone xxx
#cd xxx
#node webhook.js (不想看日志的推荐用pm2守护进程运行)
pm2 start index.js/pm2 start index.js --watch
```

