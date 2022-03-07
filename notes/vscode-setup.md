##### vscode 如何让打开的文件不消失

```json
"workbench.editor.enablePreview": false
```

##### vscode文件控制字体大小

```json
"editor.mouseWheelZoom": true,
```

vscode鼠标右键打开在浏览器

```json
使用 open in browser 插件
```

vscode鼠标右键打开在server上

```json
使用 live Server 插件
```

##### 使用react的插件

```
react---> ES7 React/Redux/React-Native/JS snippets
```

##### 设置缩进空格数

```
tab size
```



##### 插件列表

```
HTML CSS Support
在编写样式表的时候，自动补全功能大大缩减了编写时间

Auto Close Tag
自动添加HTML / XML关闭标签

HTML Snippets
代码自动填充

HTML Boilerplate
提供生成标准HTML样板代码

Image Preview
鼠标移到路径里显示图像预览

intelliSense for CSS class names in HTML
项目中 css 文件里的名称智能提示在 html 中

JavaScript (ES6) code snippets
es6代码片段

Path Intellisense
路径自动补全

Vetur
Vue 语法高亮显示, 语法错误检查, 代码自动补全

Color Info
这个便捷的插件，将为你提供你在 CSS 中使用颜色的相关信息。你只需在颜色上悬停光标，就可以预览色块中色彩模型的（HEX、 RGB、HSL 和 CMYK）相关信息了

Beautify
格式化代码

Guides
代码的标签对齐线

jQuery Code Snippets
jq代码段提示

IntelliSense for CSS class names in HTML
智能地给你已定义CSS类名的提示

VSCode Great Icons
小图标区分不同的文件类型

Bracket Pair Colorizer
给括号加上不同的颜色，便于区分不同的区块
```

#### vscode setting json

```json
{
  "editor.renderControlCharacters": true,
  "window.zoomLevel": 4,
  "workbench.sideBar.location": "left",
  "editor.minimap.enabled": false,
  "open-in-browser.default": "chrome",
  "editor.tabSize": 2,
  "workbench.activityBar.visible": true,
  "editor.snippetSuggestions": "top",
  // "editor.tabCompletion": "on",
  "workbench.startupEditor": "newUntitledFile",
  "emmet.triggerExpansionOnTab": true,
  "files.associations": {
    "*.tpl": "html",
    "*.xtpl": "html",
    "*.vue": "vue",
    "*.wxml": "html",
    "*.wxss": "less",
    "*.wpy": "vue",
    "*.mpx": "vue",
    "*.cjson": "jsonc",
    "*.wxs": "javascript",
    "*.js": "javascript"
  },
  "editor.tokenColorCustomizations": {
    // "comments": "#FF9900",
    "comments": "#FF9977"
  },
  "extensions.ignoreRecommendations": true,
  "vetur.format.defaultFormatter.html": "js-beautify-html",
  "git.ignoreMissingGitWarning": true,
  "window.menuBarVisibility": "default",
  // 是否开启编译 scss对应的souceMap文件
  "liveSassCompile.settings.generateMap": true,
  "explorer.confirmDelete": false,
  "problems.autoReveal": false,
  "explorer.confirmDragAndDrop": false,
  "files.exclude": {
    "**/.git": true,
    "**/.svn": true,
    "**/.hg": true,
    "**/CVS": true,
    "**/.DS_Store": true,
    // 隐藏node包
    "node_modules/": true,
    "dist": true
  },
  "liveServer.settings.donotShowInfoMsg": true,
  "breadcrumbs.enabled": true,
  "git.enabled": false,
  "workbench.statusBar.feedback.visible": false,
  "editor.fontSize": 13,
  "workbench.editor.highlightModifiedTabs": true,
  "workbench.iconTheme": "material-icon-theme",
  "javascript.updateImportsOnFileMove.enabled": "never",
  // "vetur.format.defaultFormatter.scss": "prettier",
  // "vetur.validation.style": false
  "terminal.integrated.rendererType": "dom",
  "[html]": {
    "editor.defaultFormatter": "vscode.html-language-features"
  },
  "[json]": {
    "editor.defaultFormatter": "vscode.json-language-features"
  },
  "[javascript]": {
    "editor.defaultFormatter": "vscode.typescript-language-features"
  },
  "less.compile": {
    // "outExt": ".wxss"
  },
  "emmet.includeLanguages": {
    "wxml": "html"
  },
  "minapp-vscode.disableAutoConfig": true,
  "[javascriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
},
"[jsonc]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
},
"workbench.colorTheme": "Monokai",
"php.suggest.basic": false,
"php.validate.enable": false
}
```

