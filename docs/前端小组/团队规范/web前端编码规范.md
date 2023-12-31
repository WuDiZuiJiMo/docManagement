## web前端编码规范

## 总述

### 为什么需要规范化标准
> 前端作为应用与用户交互的“脸面”功能，对其要求越来越高；同时前端软件开发通常需要多人协同，不同的开发者有不同的编码习惯和喜好，不同的习惯和喜好会增加项目维护成本，所以每个项目或团队需要明确统一的标准。
### 哪里需要规范化标准
> 开发过程中人为编写的一切。如代码、文档、纪要甚至是提交日志，其中代码的标准化规范尤为重要，因为代码的质量关于项目的质量以及项目后期的维护成本。
### 实施规范化的方法
> 本文重点给出代码的规范性说明，并提供对应的 Lint 文件，实现部分规则校验。
### 本文组织结构
> 本文先给出整体规约描述，随后以此给出针对 HTML&CSS，Javascript 以及 Vue 规约的描述。
### 规约分级
本文给出的规约根据重要程度给出如下三个等级：
| 约束等级 | 约束效力 | 强制性 |
| :---------------------------------: | :------------------------------: | :------------------------------------------: |
| 【强制】 | 违反该项将被认为代码存在严重缺陷 | 前端程序团队必须遵守 |
| 【推荐】 | 违反该项将被认为代码存在轻微缺陷 | 根据具体产品特性的不同，选择性地遵守 |
| 【参考】 | 违反该项可被认为代码存在优化空间 | 从产品持续优化及人员技能提升的角度，参考使用 |


## 整体原则

### 主流技术栈以及框架选型
#### 模块化标准
模块化的意义：
- 将一个复杂的程序依据一定的规则(规范)封装成几个块(文件), 并进行组合在一起；
- 块的内部数据与实现是私有的, 只是向外部暴露一些接口(方法)与外部其它模块通信；
> CommonJS：
参见 [http://nodejs.cn/api/modules.html](http://nodejs.cn/api/modules.html) CommonJS模块是为 Node.js 打包 JavaScript 代码的原始方式。根据CommonJS规范，一个单独的文件就是一个模块。每一个模块都是一个单独的作用域，也就是说，在该模块内部定义的变量，无法被其他模块读取，除非定义为global对象的属性。输出模块变量的最好方法是使用module.exports对象。典型语法： 
```
var _ = require('lodash');
var object = { 'a': 1, 'b': '2', 'c': 3 };
module.exports = _.omit(object, ['a', 'c']);
```
> ES6 module：
参见 [https://www.runoob.com/w3cnote/es6-module.html](https://www.runoob.com/w3cnote/es6-module.html) .目前主流的前端模块化标准，几乎目前所有的项目都基于ES6 module组织代码.典型语法：
```
// es6
import * as _ from 'lodash';
const object = { 'a': 1, 'b': '2', 'c': 3 };
export default _.omit(object, ['a', 'c']);

// typescript
import _ = require('lodash');
const object: Object = { 'a': 1, 'b': '2', 'c': 3 };
export = _.omit(object, ['a', 'c']);
```
考虑到前端领域 ES6 Module 已经是事实上的模块化标准，并且 Nodejs 端代码也可以通过打包方式将将 ES6 代码打包成 commonjs 代码执行, 本文的规约都基于 ES6，给出描述。

工程化相关规约：
- 【推荐】 后端项目如果采用 ES6 module 方式写源码，建议打包后保留 source map 便于出错后问题的定位。
- 【强制】 前端项目禁止将 source map 发布到生产环境。
- 【推荐】 建议使用 babel7 作为编译器。通过配置.browserslistrc 文件选择支持最低版本的浏览器

#### 主流前端框架
- Angular
Angular 原名 angularJS 诞生于 2009 年，它最大的特点是把后端的一些开发模式移植到前端来实现，如 MVC、依赖注入等。
- React
react 是 Facebook 推出的一个用来构建用户界面的 JavaScript 库。 React 主要用于构建 UI，很多人认为 React 是 MVC 中的 V（视图）。React 拥有较高的性能，代码逻辑非常简单，越来越多的人已开始关注和使用它。
- Vue
Vue.js 是一款流行的 JavaScript 前端框架，一个用于创建用户界面的开源 JavaScript 框架，旨在更好地组织与简化 Web 开发。Vue.js 是一套构建用户界面的渐进式框架。与其他重量级框架不同的是，Vue 采用自底向上增量开发的设计。
【推荐】 公司 IT 基于 VUE 开发的 PC 版本的 XUI 以及移动版本的 XUI For MagicUI 都是基于 Vue2，并将于未来支持 Vue3，所以建议使用 Vue 框架，以便于享受 IT 部门提供的标准化组件的福利。

#### 数据交互框架选型
- 【强制】 Axios 提供了较为完备的接口访问方式，可以同时运行于浏览器端和 Nodejs 端，且具备较好的兼容性（xhr & fetch），禁止前端项目引入 JQ 用于 Ajax;

#### 性能优化
- 【推荐】 对于 C 端页面，尤其是背负转化率指标的页面建议走服务端渲染，提升首屏加载性能。
- 【推荐】 如果条件允许，静态资源部署在 CDN 上。
- 【强制】 通过 nginx 代理的静态资源需要配置缓存策略。
- 【推荐】 图片列表使用懒加载技术，同时图片资源尽量 webp 化，对于支持 webp 的浏览器下发 webp 资源。
- 【推荐】 每次上线前，建议对页面作 lighthouse 综合打分测试，对于得分较低的项，评估后决定是否修复。

### 排版规范
> 【推荐】 推荐统一使用 VSCODE 作为前端工程 IDE，使用 Prettier 插件为文件作统一格式化处理，相比于 ESLint，TSLint，Prettier 可以对 JS 文件以外的文件作格式化：
- JavaScript (including experimental features)
- JSX
- Angular
- Vue
- Flow
- TypeScript
- CSS, Less, and SCSS
- HTML
- Ember/Handlebars
- JSON
- GraphQL
- Markdown, including GFM and MDX
- YAML
```javascript
/*  prettier的推荐配置 */
"prettier.printWidth": 120, // 超过最大值换行
"prettier.tabWidth": 2, // 缩进字节数
"prettier.useTabs": false, // 缩进不使用tab，使用空格
"prettier.semi": true, // 句尾添加分号
"prettier.singleQuote": true, // 使用单引号代替双引号
"prettier.proseWrap": "preserve", // 默认值。因为使用了一些折行敏感型的渲染器（如GitHub comment）而按照markdown文本样式进行折行
"prettier.arrowParens": "avoid", //  (x) => {} 箭头函数参数只有一个时是否要有小括号。avoid：省略括号
"prettier.bracketSpacing": true, // 在对象，数组括号与文字之间加空格 "{ foo: bar }"
"prettier.disableLanguages": ["vue"], // 不格式化vue文件，vue文件的格式化单独设置
"prettier.endOfLine": "auto", // 结尾是 \n \r \n\r auto
"prettier.eslintIntegration": false, //不让prettier使用eslint的代码格式进行校验
"prettier.htmlWhitespaceSensitivity": "ignore",
"prettier.ignorePath": ".prettierignore", // 不使用prettier格式化的文件填写在项目的.prettierignore文件中
"prettier.jsxBracketSameLine": false, // 在jsx中把'>' 是否单独放一行
"prettier.jsxSingleQuote": false, // 在jsx中使用单引号代替双引号
"prettier.parser": "babylon", // 格式化的解析器，默认是babylon
"prettier.requireConfig": false, // Require a 'prettierconfig' to format prettier
"prettier.stylelintIntegration": false, //不让prettier使用stylelint的代码格式进行校验
"prettier.trailingComma": "es5", // 在对象或数组最后一个元素后面是否加逗号（在ES5中加尾逗号）
"prettier.tslintIntegration": false // 不让prettier使用tslint的代码格式进行校验
```


## HTML

### DOCTYPE 声明
【强制】 HTML 文件必须加上 DOCTYPE 声明，并统一使用 HTML5 的文档声明
```html
<!DOCTYPE html>
```

### charset
【强制】 统一使用"UTF-8"编码
```html
<html>
  <head>
     <meta charset="UTF-8">
   </head>
</html>
```

### viewport
【强制】 页面提供给移动设备使用时，需要设置 viewport
```html
<meta name="viewport" content="width=device-width, minimum-scale=1.0, viewport-fit=cover" />
```

### IE 兼容模式
【推荐】 用 <meta> 标签可以指定页面应该用什么版本的 IE 来渲染
```html
<meta http-equiv="X-UA-Compatible" content="IE=Edge,chrome=1">
```

### 资源加载
【推荐】 引入 CSS 和 JavaScript 时无需指定 type。
根据 HTML5 规范，引入 CSS 和 JavaScript 时通常不需要指明 type，因为 text/css 和 text/javascript 分别是他们的默认值。
```html
<!-- bad -->
<link rel="stylesheet" type="text/css" href="example.css">
<script type="text/javascript" src="example.js"></script>

<!-- good -->
<link rel="stylesheet" href="example.css">
<script src="example.js"></script>
```
【参考】 引用的资源路径如果包含域名，则建议使用//和站点始终保持相同协议
```html
<!-- bad -->
<link rel="stylesheet" type="text/css" href="https://static.hihonor.com/example.css">
<script type="text/javascript" src="https://static.hihonor.com/example.js"></script>

<!-- good -->
<link rel="stylesheet" href="//static.hihonor.com/example.css">
<script src="//static.hihonor.com/example.js"></script>
```

### 引用顺序
【强制】 在 head 标签内引入 CSS, 在 body 结束标签前引入 JS。
在 <body></body>中指定外部样式表和嵌入式样式块可能会导致页面的重排和重绘，对页面的渲染造成影响。因此，一般情况下，CSS 应在 <head></head> 标签里引入。
```html
<!-- bad -->
<!DOCTYPE html>
<html>
  <head>
    <script src="example.js"></script>
    <script src="script.js"></script>
  </head>
  <body>
    <style>
      .div-example {
        padding-left: 15px;
      }
    </style>
  </body>
</html>

<!-- good -->
<!DOCTYPE html>
<html>
  <head>
    <style>
      .div-example {
        padding-left: 15px;
      }
    </style>
  </head>
  <body>
    ...
    <script src="path/script.js"></script>
  </body>
</html>
```

### 注释
【强制】 单行注释，需在注释内容和注释符之间需留有一个空格，以增强可读性。
```html
<!-- 单行注释 -->
```
【强制】 多行注释，需在注释内容和注释符之间需留有一个空格，以增强可读性。
多行注释，注释符单独占一行，注释内容 2 个空格缩进。
```html
<!--
  多行注释
  多行注释
-->
```

### 缩进
【强制】嵌套的节点统一使用 2 个空格缩进
```html
<html>
  <head>
    <title>title</title>
  </head>
  <body>
    <h1 class="hello-world">Hello world</h1>
  </body>
</html>
```
缩进可以通过全局 Prettier 控制

### 标签
【强制】 所有具有开始标签和结束标签的元素必须写上起止标签；自闭合标签不需要加上结束标签；
```html
<!-- bad -->
<div>
  <h1>我是h1标题</h1>
  <p>我是一段文字，没有结束标签，浏览器也能正确解析
</div>
<br/>

<!-- good -->
<div>
  <h1>我是h1标题</h1>
  <p>我是一段文字，浏览器能正确解析</p>
</div>
<br>
```

### 大小写
【强制】 HTML 标签名、类名、标签属性和大部分属性值统一使用小写。
```html
<!-- bad -->
<div class="DEMO"></div>
<DIV CLASS="DEMO"></DIV>

<!-- good -->
<div class="demo"></div>
<!-- 优先使用 IE 最新版本和 Chrome Frame -->
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1"/>
```

### 属性
- 【强制】元素属性值使用双引号。
- 【强制】不要为 Boolean 属性添加取值：html5下原生html标签如果设置了该属性则表示为true，如果disable该属性则直接不设置该属性。
- 【强制】属性名称用中划线做分隔符。
```html
<!-- bad -->
<input class='demo' checked='checked'>

<!-- good -->
<input class="demo" checked>

<!-- bad -->
<h1 class="helloWorld">Hello world</h1>

<!-- good -->
<h1 class="hello-world">Hello world</h1>
```
【推荐】 自定义属性的命名：以 data- 为前缀。
```html
<!-- bad -->
<a modal="toggle" href="#">
  example link
</a>

<!-- good -->
<a data-modal="toggle" href="#">
  example link
</a>
```

### 图像
【强制】禁止 img 标签的 src 取值为空。src为空会导致img标签直接访问页面文档造成严重性能问题。
【推荐】建议为重要图片添加 alt 属性，可以提高图片记载失败时的用户体验。
【参考】建议添加 width 和 height。如果图像指定了高度宽度，页面加载时就会保留指定的尺寸。如果没有指定图片的大小，加载页面时有可能会破坏 HTML 页面的整体布局。

### 语义化
【推荐】尽量根据语义使用 HTML 标签。我们应优先选取符合当下所需语义的标签，这既有助于可访问性，也可以在 CSS 加载失败时获得较好的展示效果。
```html
<!-- bad -->
<div class="list">
  <div class="list-item">1</div>
  <div class="list-item">2</div>
  <div class="list-item">3</div>
</div>

<!-- good -->
<ul class="list">
  <li class="list-item">1</li>
  <li class="list-item">2</li>
  <li class="list-item">3</li>
</ul>
```

### 可访问性
【推荐】网页可访问性使网页内容落实"无障碍"，让不同程度或需求的用户可以顺畅的获取网站上的信息。了解更多 HTML 可访问性的知识，可以参考[MDN 的无障碍文档](https://developer.mozilla.org/zh-CN/docs/learn/Accessibility)。


## CSS

### 命名
【强制】 类名使用小写字母，用中划线分割。sass、less 中的变量、函数、混合等采用驼峰式命名。
```css
/* bad */
.mySelector {
  padding-left: 15px;
}
/* good */
.my-selector {
  padding-left: 15px;
}
/* 变量 */
$colorBlack: #000;
/* 函数 */
@function pxToRem($px) {
  padding-left: 15px;
}
/* 混合 */
@mixin centerBlock {
  padding-left: 15px;
}
```

### 缩进
【强制】 使用 2 个空格缩进
```css
/* bad */
.selector {
    padding-left: 15px;
}
/* good */
.selector {
  padding-left: 15px;
}
```

### 注释
【强制】 注释内容和注释符之间留有一个空格
```css
/* bad */
.selector {
  /*comment*/
  /*  comment  */
  padding-left: 15px;
}

/* good */
.selector {
  /* comment */
  /**
   * comment
   */
  padding-left: 15px;
}
```

### 分号
【强制】 每个属性声明末尾都要加上分号
```css
/* bad */
.selector {
  margin-top: 10px;
  padding-left: 10px
}

/* good */
.selector {
  margin-top: 10px;
  padding-left: 10px;
}
```

### 引号
【强制】 统一使用单引号
```css
/* bad */
.selector {
  background: url("logo.png");
}
input[type=text] {
  height: 20px;
}

/* good */
.selector {
  background: url('logo.png');
}
input[type='text'] {
  height: 20px;
}
```

### 空格
【强制】 选择器和{之间有一个空格
```css
/* bad */
.selector{
  padding-left: 15px;
}
/* good */
.selector {
  padding-left: 15px;
}
```
【强制】 属性名和 : 之前无空格，：和属性值之间有一个空格
```css
/* bad */
.selector {
  margin-top : 10px;
  padding-left:15px;
}

/* good */
.selector {
  margin-top: 10px;
  padding-left: 15px;
}
```
【强制】 选择器 >、+、~等前后有一个空格
```css
/* bad */
.selector>.children {
  padding-left: 15px;
}
.selector+.brother {
  padding-left: 15px;
}

/* good */
.selector > .children {
  padding-left: 15px;
}
.selector + .brother {
  padding-left: 15px;
}
```
【强制】 注释内容和注释符之前有一个空格
```css
/* bad */
.selector {
  /*comment*/
  /*comment */
  padding-left: 15px;
}

/* good */
.selector {
  /* comment */
  padding-left: 15px;
}
```

### 换行
【强制】 声明的 { 和 } 应单独成行
```css
/* bad */
.selector {
  padding-left: 15px;}

/* good */
.selector {
  padding-left: 15px;
}
```
【强制】 每个属性声明应单独成行
```css
/* bad */
.selector {
  padding-left: 15px;  margin-left: 10px;
}

/* good */
.selector {
  padding-left: 15px;
  margin-left: 10px;
}
```
【强制】 使用多个选择器时，每个选择器应单独成行
```css
/* bad */
.element, .selector {
  padding-left: 15px;
}

/* good */
.element,
.selector {
  padding-left: 15px;
}
```

### 选择器
- 【推荐】 尽量不要使用 id 选择器。id 选择器有过高的优先级，使得后续很难进行样式覆盖。
- 【推荐】 尽量不用 * 选择器

### 属性
【强制】 颜色 16 进制使用小写字母，尽量简写
```css
/* bad */
.selector {
  background-color: #ABCDEF;
  color: #ffffff;
}

/* good */
.selector {
  background-color: #abcdef;
  color: #fff;
}
```
【强制】 属性多数情况下并不需要设置属性简写中包含的所有值。常见的属性简写包括：margin、padding、font、background、border、border-radius、transition、animation
```css
/* bad */
.selector {
  margin-left: 10px;
  margin-right: 10px;
  margin-top: 10px;
  margin-bottom: 10px;
}

/* good */
.selector {
  margin: 10px;
}
```
【强制】 属性值 0 后面不要加单位
```css
/* bad */
.selector {
  margin-top: 0px;
  font-size: 0em;
}

/* good */
.selector {
  margin-top: 0;
  font-size: 0;
}
```
【强制】 不允许有空的规则
```css
/* bad */
.selector {

}

/* good */
.selector {
  padding: 10px;
}
```
【强制】 同个属性不同前缀的写法需要在垂直方向保持对齐；浏览器私有前缀在前，标准前缀在后。
```css
/* bad */
.selector {
  background: linear-gradient(to bottom, #fff 0, #eee 100%);
  background: -webkit-linear-gradient(top, #fff 0, #eee 100%);
  background: -moz-linear-gradient(top, #fff 0, #eee 100%);
}

/* good */
.selector {
  background: -webkit-linear-gradient(top, #fff 0, #eee 100%);
  background: -moz-linear-gradient(top, #fff 0, #eee 100%);
  background: linear-gradient(to bottom, #fff 0, #eee 100%);
}
```

### 媒体查询
【推荐】 尽量将媒体查询的规则靠近与他们相关的规则；不要将他们一起放到一个独立的样式文件中或者放在文档的最底部。
```css
/* bad */
/* header styles */
/* main styles */
/* footer styles */
@media (…) {
 …
}

/* good */
/* header styles */
@media (…) {
 …
}
```

### 书写顺序
【推荐】 相关联的属性声明最好写成一组，并按如下顺序排序：
1. **定位**：如 position、left、right、top、bottom、z-index
2. **盒模型**：如 display、float、width、height、margin、padding、border
3. **文字排版**：如 font、color、line-height、text-align
4. **外观**：如 background
5. **其他属性**
```css
.declaration-order {
    /* 定位 */
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 100;
  /* 盒模型 */
  display: block;
  float: right;
  width: 100px;
  height: 100px;
  border: 1px solid #e5e5e5;
  /* 排版 */
  font: normal 13px "Helvetica Neue", sans-serif;
  line-height: 1.5;
  color: #333;
  text-align: center;
  /* 外观 */
  background-color: #f5f5f5;
  /* 其他 */
  opacity: 1;
}
```

## 命名

### 命名原则
- 【强制】 源文件编码格式（包括注释）必须是 UTF-8
ESLINT: [unicode-bom](https://eslint.bootcss.com/docs/rules/unicode-bom)
- 【参考】 为方法、变量取一个好名字，提高代码可读性
说明： 好的命名，有如下特征：
1. 能清晰的表达意图：使用完整的描述性的单词，避免使用单个字母、未形成惯例的缩写来命名。
例如 let elapsedTimeInDays 要比 let d 好得多；
2. 必须用英文单词命名，不允许出现中文拼音：因为代码默认是使用英文构词，若突然出现拼音会让人感到混乱，不利于读懂名字的意思。同时拼音不利于外国员工理解；
3. 避免造成误导：有误导的名字比表达不清的名字还要有危害性；
4. 能区分出意思：比如以下名称：product，productInfo，productData，表达的意思都差不多，让人从名称并不能区分出它们各自代表的东西到底有什么不同。因此建议不要在变量后加 info， data，object 等一般意义的词；
5. 不用或少用缩写：小于 15 个字母的一般不用缩写。超过 15 个字母的，可采用以去掉元音字母的方法或者行业内约定俗成的缩写，不要编造不符合惯例的缩写。比如 serviceDataPoint，可以缩写为 svcDataPnt，不可缩写为 SDP。

### 类
【强制】 构造器函数、类采用首字母大写的驼峰命名法
```javascript
//bad
class myClass() {}
const cls = new myClass();
//good
class MyClass() {}
const cls = new myClass();
```
ESLINT: [new-cap](https://eslint.bootcss.com/docs/rules/new-cap)

### 方法
- 【强制】 方法的命名用动词、动宾结构，并遵循驼峰风格
说明： 方法名尽量语义话，强烈建议采用动宾结构
```javascript
//bad
function type() {}
function Finished() {}
function visible() {}
function DRAW() {}
function KeyListener(listener) {}
//good
function getType() {}
function isFinished() {}
function setVisible() {}
function draw() {}
function addKeyListener(listener) {}
```
ESLINT: [camelcase](https://eslint.bootcss.com/docs/rules/camelcase)

- 【强制】【推荐】 为保证可读性，方法名不宜过长或过短
说明： 方法名越长，可读性越差，如果方法名称超过 15 个字母，可采用以去掉元音字母的方法或者以行业内约定俗成的缩写方式缩写方法名。
ESLINT: [id-length](https://eslint.bootcss.com/docs/rules/id-length)

### 类属性
【推荐】 类中的私有的属性和方法名称，建议以下划线 "_" 开头
说明： 以下划线开头或结尾，能够清楚看出该属性和方法只是内部使用的，在修改的时候并不需要担心影响到外部。
```javascript
class Foo {
  constructor() {
    // 私有属性
    this._bar = computeBar();
    // 不是私有属性，可以通过实例访问;
    this.baz = computeBaz();
  }
}
```

### 变量
- 【强制】 变量名遵循驼峰风格
说明： 便于一眼看出函数、属性的意思。
```javascript
//bad
const firstname = 'xiaoming';
//good
const firstName = 'xiaoming';
```
ESLINT: camelcase

- 【强制】 不要用保留字作用键名或者变量名，请用保留字的同义词
```javascript
//bad
const superMan = {
  default: {
    clark: 'kent'
  },
  private: true
};

//good
const superMan = {
  default: {
    clark: 'kent'
  },
  hidden: true
};
```
ESLINT: no-reserved-keys

- 【推荐】 避免使用否定的布尔变量名
说明： 以当使用逻辑非运算符，并出现双重否定时，会出现理解问题
```javascript
//bad
const isNoError = false;
const isNotFound = false;
//good
const isError = false;
const isFound = false;
```

- 【推荐】 变量中的缩写词应始终全部大写或全部小写，文件名应避免大小写，尽量用连字符（防止 windows 中文件名大小写不敏感）
```javascript
//bad
import SmsContainer from './containers/SmsContainer';

const HttpRequests = [
// ...
];
//good
import SMSContainer from './containers/SmsContainer';

const httpRequests = [
// ...
];
```

- 【推荐】 业务开发中变量名尽量不要使用特殊字符
```javascript
//bad
const side$bar = 'a';
//good
const sideBar = 'a';
```

### 常量
- 【推荐】 常量名称全大写单词以下划线分隔
说明： 不要使用魔鬼数字，用有意义的常量代替。不应该取 NUM_FIVE = 5 或 NUM_5 = 5 这样的"魔鬼常量"。魔鬼数字或常量的解决方式：可以考虑使用 API 来进行判断，不要定义数字，比如 size === 0，改成用 isEmpty()。
```javascript
//bad
const hotleGetUrl = 'http://map.baidu.com/detail';
const MAXUSERNUM = 200;
const sL = 'Launcher';

//good
const HOTEL_GET_URL = 'http://map.baidu.com/detail';
const MAX_USER_NUM = 200;
const APPLICATION_NAME = 'Launcher';
```

### 目录
- 【推荐】 普通文件目录，采用小写方式， 以中划线分隔。
```javascript
//bad
Scripts/style_test/Dir-test/baseInfo.vue;
src/components/BigBanner/top-index/....
src/indexPage.vue

//good
scripts/style-test/dir-test/base-info.vue;
src/components/big-banner/top-index/...
src/index-page.vue
```

## 注释

### 注释原则
- 【推荐】 尽量用代码来解释自己
说明： 在写注释前，要思考是否能通过改善代码可读性来达到注释的效果，从而避免写注释。
```javascript
//bad
// 检查员工是否有资格享受全部福利
if ((employee.flags || HOURLY_FLAG) && employee.age > 65) {
  ...
}

//good
//把复杂的条件判断封装成一个名字清晰的方法，达到代码解析自己的目的
if (employee.isFullBenefits()) {
  ...
}
```

- 【推荐】 注释应该是解释代码的意图，而不是描述代码怎么做
说明： 如果不得不写注释，那么就要写出好的注释。好的注释能提供有用、额外的信息，能解释代码为什么要这么写，而不是再描述一遍代码是怎么做的。

- 【推荐】 保证注释与代码一致，避免产生误导
说明： 以注释造成误导，危害性很大，还不如不写。很多误导的产生，并不是有意为之，而是在代码修改的同时没有修改对应的注释造成的。因此，如果写了注释，就要保证注释与代码一致，避免产生误导。如果注释不再有用，必须删除。

### 注释风格
- 【推荐】 一般单行注释用 //，块注释用 /* */，文档注释用 /** */
说明：
1. 单行注释用//，比较方便。
2. 块注释一般采用/* */，在 IDE 支持的情况下，可以用快捷键增加块注释。
3. JsDoc 对注释格式要求采用/***/，语法格式：用法，参数，返回值。
```javascript
//bad
// make() returns a new element
// based on the passed in tag name
//
// @param {String} tag
// @return {Element} element
function make(tag) {
  // ...
  return element;
}

//good
/
*    make() returns a new element
*    based on the passed in tag name
*
*    @param {String} tag
*    @return {Element} element
*/
function make(tag) {
  // ...
  return element;
}
```

- 【推荐】 注释应与其描述代码位置相邻，放在所注释代码上方，并与代码采用同样缩进
```javascript
function getTableData() {
  // 从后台请求表格数据
  getData();
}
```
ESLINT: line-comment-position

- 【推荐】 注释和上面代码块要有空行，// 和注释内容之间要有一个空格
说明： 注释和上面代码块要有空行可读性更强，在//和内容之间加一个空格会让注释看起来清晰美观。
ESLINT: lines-around-comment

- 【推荐】 删除不必要的开发态注释 （TODO/FIXME），以及注释掉的废弃代码
说明： 给代码添加警告注释，标明哪些没有完成或需要审查是可以的。不过在代码交付给客户之前，应该把警告注释删除, 正式交付给客户的或者贡献到社区的代码不应包含 TODO/FIXME 注释。
```javascript
//bad
// TODO: do something
// FIXME: this is not a good idea
```

- 【推荐】 不要用注释记录修改日志
说明： 代码中充斥着大量修改历史信息，使代码难以阅读。现代的配置管理工具能记录每次修改的日志，因此不必将修改日志写在代码中。


## 最佳实践

- 【推荐】 一个目录的子目录和文件数之和不要超过 50
说明： 目录过大会导致可读性和维护性问题。

- 【推荐】 控制文件的长度，最好不要超过 1000 行
说明： 过长的文件往往意味着文件（模块）功能不单一，过于复杂。很多开源项目都表明，文件长度越短，越容易理解和维护。
ESLINT: max-lines

- 【推荐】 每行代码应该少于 120 个字符
说明： 除了一些特殊情况，如：URL、shell 命令、导入模块名，其他任何超出此限制的行都应该被换行。
ESLINT: max-len

- 【推荐】 方法长度不超过 50 行
说明： 短方法易理解、易维护、易扩展、易测试。根据业界经验，方法的长度最好不超过 50 行（即： 尽量让一个方法能在一个电脑屏幕内看完）。
ESLINT: max-lines-per-function

- 【推荐】圈复杂度不超过 20
说明： 圈复杂度数量上表现为覆盖所有代码的独立现行路径条数。为了代码的可读性，圈复杂度建议不要超过 20。
ESLINT: complexity

- 【强制】 块语句的最大可嵌套深度不要超过 4 层
说明： 很多开发者认为如果块语句嵌套深度超过某个值，代码就很难阅读。强制块语句的最大可嵌套深度不超过 4 层来降低代码的复杂性。
```javascript
//bad
function foo() {
  for (;;) { // 嵌套 1 层
    const val = param => { // 嵌套 2 层
      if (true) { // 嵌套 3 层
        if (true) { // 嵌套 4 层
          if (true) { // 嵌套 5 层
          }
        }
      }
    };
  }
}

//good
function foo() {
  for (;;) { // 嵌套 1 层
    const val = param => { // 嵌套 2 层
      if (true) { // 嵌套 3 层
        if (true) { // 嵌套 4 层
        }
      }
    };
  }
}
```
ESLINT: max-depth

- 【强制】 回调函数嵌套的层数不超过 3 层。
说明： 很多 JavaScript 类库是使用回调模式处理异步操作。一个最长见的隐患就是嵌套的回调，代码嵌套层级越深越难以阅读。尽量将异步回调转换成 promise 或者 async await 的方式，提高可读性。
ESLINT: max-nested-callbacks

## 变量定义和声明
- 【强制】 要求使用 const 或 let 而不是 var（ECMAScript 6+）
说明： ES6 允许程序员使用 let 和 const 关键字在块级作用域而非函数作用域下声明变量。块级作用域在很多其他编程语言中很普遍，能帮助程序员避免错误。只读变量用 const 定义, 其它变量用 let 定义。
```javascript
//bad
var number = 1;
var count = 1;


if (isOK) {
  count += 1;
}
//good
const number = 1;


let count = 1;
if (isOK) {
  count += 1
}
```
ESLINT: no-var

- 【推荐】 在需要时候声明变量，并且尽快初始化（ECMAScript 6）
说明：在 ECMAScript 6，let 和 const 是块级作用域而不是函数作用域，不用当心变量提升导致问题。声明局部变量应该接近它们首次使用的点（在合理范围内），以最小化它们的范围，会使逻辑更加清晰、可读性更好。
```javascript
//bad
function (hasName) {
  const name = getName();
  if (!hasName) {
    return false;
  }
  this.setFirstName(name); return true;
}

//good
function (hasName) {
  if (!hasName) {
    return false;
  }
  const name = getName();
  this.setFirstName(name); return true;
}
```

- 【参考】 每行声明一个变量
说明："解构赋值"和"for 循环的标题中"的情况例外，允许一行声明多个变量。
```javascript
//bad
const count = 1, row = 2;
//good
const count = 1;
const row = 2;
//exception
const {a = 1, c= 2} = object;
for (let i = 0, k = 1; i < 4; i++) {
}
```

- 【参考】 禁止连续赋值
说明： 对变量连续赋值可能会导致意想不到的结果，而且难以阅读。
```javascript
//bad
const age = bee = cat = 5;
const foo = bar = 'baz';

//good
const age = 5;
const bee = 5;
const cat = 5;
const foo = 'baz';
const bar = 'baz';
```

- 【强制】 变量不需要用 undefined 初始化
说明： 在 JavaScript 中，声明一个变量但未初始化，变量会自动获得 undefined 作为初始值。因此，初始化变量值为 undefined 是多余的。
```javascript
//bad
let foo = undefined;

//good
let foo;
```
ESLINT: no-undef-init

- 【推荐】使用基本类型的字面量而不是其封装类型
说明： 在不需要使用基本类型的属性和方法的时候，不要使用其封装类型。封装类型的使用有如下的几个注意事项：

封装类型 Boolean 的语义问题： 在用作条件表达式中时，封装类型 Boolean 的语义与 Object 相同，结果都是 true，而不是基本类型的 true 和 false；
```javascript
//bad
const isOk = new Boolean(false);
if (isOk) { // 总是true
  console.log ('hi'); // 输出 'hi'
}

//good
const isOk = false;
if (isOk) {
  console.log (‘hi’);
}
```
Array 的构造函数的语义问题：Array 的构造函数行为与参数传入的个数和值关联，导致存在语义的不确定性；
```javascript
//bad
// 长度为3的数组
const a1 = new Array(x1, x2, x3);

// 如果x1是自然数，会创建一个长度为x1的数组
// 如果x1是一个数字，但是非自然数，会抛异常
// 否则会创建长度为1的数组，值为x1
const a2 = new Array(x1);

// 长度为 0
const a3 = new Array();

//good
const a1 = [x1, x2, x3];
const a2 = [x1];
const a3 = [];
```
Object 类型：鉴于可读性和一致性考虑，建议也使用字面量进行声明。
```javascript
//bad
const obj = new Object();
const obj2 = new Object();
obj2.age = 0;
obj2.bee = 1;
obj2.cat = 2;
obj2 ['strangekey'] = 3;

//good
const obj = {};
const obj2 = {
  age: 0,
  bee: 1,
  cat: 2,
  strangeKey: 3,
};
```
ESLINT: no-new-wrappers

- 【强制】 禁止变量声明覆盖外层作用域的变量
说明： 覆盖是指在同一作用域里，局部变量和全局变量同名。
```javascript
//bad
let bar = 3;
function foo() {
  // bar 覆盖了全局环境中的 bar。这会混淆读者并且在 foo 中不能获取全局变量
  let bar = 10;
}
```
ESLINT: no-shadow

## 方法
### 方法设计
- 【强制】 方法设计应遵循单一职责原则（SRP），一个方法仅完成一个功能
说明： 一个方法应该做一件事，做好这件事，只做这件事。

- 【推荐】 方法设计应遵循单一抽象层次原则（SLAP）
说明： SLAP 原则，是指让一个方法中所有的操作处于相同的抽象层。代码抽象层次的跳跃会破坏了代码的流畅性。
```javascript
//bad
function compute() {
  input();
  const height = 100;  //此处的操作相比上下文是一个“原子操作”，可以单独抽象成方法
  output();
}

//good
function compute() {
  input();
  process();
  output();
}
```

### 参数
- 【强制】 方法的参数个数不宜超过 5 个
说明： 如果参数超过 5 个，则维护的难度很大，建议减少参数个数。如果多个参数同时多次出现在多个方法中，说明这些参数紧密相关，可以将它们封装到一个对象中。
```javascript
//bad
function insertHistory(workfSeq, workfType, recSeq, sortOrder, orderSeq, workfID, region, subSys, orderID, procSystem, platType, neID, telnum, imsi, orderPri, createTime, preStatus, maxProcnum, procNum, rspCode, rspDesc, maxOvertime, operType, curStatus, neRetn, neDesc, procInterval, transid, oprid, prepayType) {
// ...
}

//good
//把参数封装为对象 spWorkfLog
function insertHistory(spWorkfLog) {
// ...
}
```
ESLINT: max-params

- 【推荐】 给函数的参数指定默认值
```javascript
//bad
function handleThings(opts) {
  // 不应该改变函数参数
  // 更加糟糕: 如果参数 opts 是 false 或 空字符串 的话，它就会被设定为一个对象
  opts = opts || {};
  // ...
}

function handleThings(opts) {
  if (opts === undefined) {
   opts = {};
  }
  // ...
}

//good
function handleThings(opts = {}) {
// ...
}
```

- 【强制】 不要把方法的入参当作工作变量/临时变量
说明： 对函数参数中的变量进行赋值可能会误导读者，导致混乱，也会改变 arguments 对象。
1. 每个变量/参数都有自己独特的功用，让一个变量承担多个职责，变量名无法清晰表达其功能，会使程序难以理解。
2. 如果参数是传引用方式的，在方法内对参数的更改，会传递到方法外，可能会造成意外的错误。所以也不建议对参数的属性进行修改。
ESLINT: no-param-reassign

- 【推荐】 总是将默认参数放在最后
说明： 将默认参数放在最后，在调用方法时，若默认参数没有值，就可以不传入这个参数，让调用代码更简单。
```javascript
//bad
function handleThings(opts = {}, name) {
  // ...
}

//good
function handleThings(name, opts = {}) {
  // ...
}
```

- 【强制】 不要使用 arguments，可以选择 rest 语法替代
说明： rest 参数是一个真正的数组，而 arguments 是一个类数组。rest 参数必须是列表中的最后一个参数。
```javascript
//bad
function concatenateAll() {
  const args = Array.prototype.slice.call(arguments);
  return args.join('');
}

//good
function concatenateAll(...args) {
  return args.join('');
}
```
ESLINT: prefer-rest-params

- 【推荐】 实现对外 API 时，应该对外部传入参数的合法性进行判断
说明： 作为对外接口，由于传入的参数是否已经被正确赋值无法确定，为了自身代码的健壮性，在使用前应该先进行合法性判断。
```javascript
//bad
function print(err) {
  addToSysLog(err.msg); // err可能是undefined
}

//good
function print(err) {
  if (err) {
    addToSysLog(err.msg);
  }
}

function print(err) {
  if (err) {
    addToSysLog(err?.msg);  //OptionalChain
  }
}
```

- 【强制】 使用参数的解构
说明： 优先使用参数的解构，而不是通过成员表达式访问属性。函数参数内使用解构时，必须是单一级别的简写属性，不能是嵌套和计算属性。如果解构的对象是可选的，则始终需要将{}指定为默认值。
ESLINT: prefer-destructuring

### 声明与实现
- 【推荐】 function 声明或表达式的一致性
说明： 在 JavaScript 中，有两种方式定义函数：function 声明和 function 表达式。
函数声明和函数表达式的主要区别是：所有的声明都被提升到当前作用域的顶部，这就意味着可以把调用它的语句放在函数声明之前。对于 function 表达式，必须在使用它之前进行定义，否则将会导致错误。项目中应该采用一致的方式来定义函数。

- 【推荐】 用到匿名函数时优先使用箭头函数（或 Function#bind），别保存 this 的引用
说明： 箭头函数提供了更简洁的语法，并且箭头函数中的 this 对象指向是不变的，this 绑定到定义时所在的对象，有更好的代码可读性。而保存 this 引用的方式，容易让开发人员搞混。
ESLINT: prefer-arrow-callback

- 【推荐】 箭头函数的简写
说明： 如果一个箭头函数适合用一行写出，并且只有一个参数，那就把花括号、圆括号和 return 都省略掉。如果不是，那就不要省略。
```javascript
//good
// 省略了那就把花括号、圆括号和return [1, 2, 3].map(num => num * num);

// 有两个参数，不能省略
[1, 2, 3].reduce((total, num) => {
  return total + num;
}, 0);
```

- 【推荐】 要求使用一致的 return 语句
说明： 不像静态类型语言强制要求函数返回一个指定类型的值，JavaScript 允许在一个函数中不同的代码路径返回不同类的值。
JavaScript 中令人感到困惑的一面是：在以下情况下函数返回 undefined ：
1. 在退出之前没有执行 return 语句
2. 执行 return 语句，但没有显式地指定一个值
3. 执行 return undefined
4. 执行 return void，其后跟着一个表达式 (例如，一个函数调用)
5. 执行 return，其后跟着其它等于 undefined 的表达式
在一个函数中，如果任何代码路径显式的返回一个值，但一些代码路径不显式返回一个值，那么这种情况可能是个书写错误，尤其是在一个较大的函数里。
```javascript
//bad
function doSomething(condition) {
  if (condition) {
    return true;
  } else {
    return;
  }
}

function doSomething(condition) {
  if (condition) {
    return true;
  }
}

//good
function doSomething(condition) {
  if (condition) {
    return true;
  } else {
    return false;
  }
}

function doSomething(condition) {
  if (condition) {
    return true;
  }
  return false;
}
```
ESLINT: consistent-return

- 【强制】 函数返回多个值时，应该使用对象解构，而不是数组解构
说明： 对象解构在增加属性或者改变排序时，无需改变调用代码的参数位置。
```javascript
//bad
function processInput() {
  return [left, right, top, bottom];
}

// 需要考虑返回数据的顺序
const [left, , top] = processInput();

//good
function processInput() {
  return { left, right, top, bottom };
}

// 只要选择需要的数据
const { left, right } = processInput();
```

## 类与对象

### 类
- 【强制】 采用 class 关键字定义类，采用 extends 关键字实现继承
说明： 在 ECMAscript 6 中，采用 class 关键字定义类，语法更简洁，更容易理解。避免采用 prototype 关键字。
```javascript
//bad
function Queue(contents = []) {
  this._queue = [...contents];
}

Queue.prototype.pop = function() {
  const value = this._queue[0];
  this._queue.splice(0, 1);
  return value;
}

//good
class Queue {
  constructor(contents = []) {
    this._queue = [...contents];
  }

  pop() {
    let value = this._queue[0];
    this._queue.splice(0, 1);
    return value;
  }
}
```
ESLINT: no-proto
ESLINT: no-prototype-builtins

- 【强制】 在构造函数中禁止在调用 super()之前使用 this 或 super
ESLINT: no-this-before-super

- 【推荐】 在构造函数中设置所有对象的属性（不包括方法）
说明： 在构造函数完成后，不应将属性添加到对象或从对象中删除属性，因为它会阻碍 VM 的优化能力。在构造函数中设置所有的属性，也有利于看代码时，对代码整体功能的理解。如有必要，对于稍后初始化的字段，也应在构造函数中显示设置为 null。

- 【推荐】 静态方法只应在基类本身上调用
说明： 不应该直接在子类上调用父类的静态方法。
```javascript
//bad
class Base { static foo() {} }
class Sub extends Base {}
function callFoo(cls) { cls.foo(); } // 不鼓励：动态的调用静态方法
Sub.foo(); // 错误: 在子类中调用父类的静态方法

//good
Base.foo(); // 在基类本身上调用静态方法
```

- 【参考】 方法可以返回 this 以支持方法的链式调用
```javascript
class Jedi {
  jump() {
    this.jumping = true;
    return this;
  }

  setHeight(height) {
    this.height = height;
    return this;
  }
}
const luke = new Jedi();
luke.jump().setHeight(20); // 支持链式调用
```

### 字符串
- 【推荐】 建议字符串使用单引号
说明： 单引号优于双引号，当你创建一个包含 HTML 代码的字符串时就知道它的好处了。
```javascript
const msg = 'This is some HTML';
```

- 【推荐】 使用模板字符串（`）实现字符串拼接，不要使用字符串的行连续符号
说明：在 ECMAScript 6 中，如果模板字符串跨越多行，或者有变量的拼接，则需要使用模板字符串。
ESLINT: prefer-template

### 数组
- 【推荐】 向数组添加元素时使用 Arrary#push 替代直接赋值
```javascript
const someStack = [];
//bad
someStack[someStack.length] = 'abracadabra';

//good
someStack.push('abracadabra');
```

- 【推荐】 不要在数组上定义或使用非数字属性（length 除外）
说明 ：在 JavaScript 中，数组也是对象，可以往数组上添加属性，但为了处理方便和避免出错，数组只应该用来存储有序（即索引连续）的一组数据。必须要添加属性时，考虑用 Map 或者 Object 替代。

- 【推荐】 数组遍历优先使用 Array 对象方法
说明： 对于数组的遍历处理，应该优先使用 Array 对象方法，如：forEach()、map()、every()、filter()、find()、findIndex()、reduce()、some()。注意：不能使用 for-in 遍历数组。

- 【强制】 不要在 forEach 循环里进行元素的 remove/add 等改变数组长度的操作
说明： 在 forEach 循环中删除、增加元素，会破坏原本的遍历顺序，可能导致意料之外的结果。

- 【强制】 使用数组解构
说明： ECMAScript 6 允许按照一定的模式，从数组和对象中提取值，对变量进行赋值，这被称之为解构(Destructuring)。解构赋值避免了临时变量或对象，给 JavaScript 书写带来了很大的便利性，同时也提高了代码的可读性。
1. 可以在赋值的左侧使用数组字面量来执行解构，最后元素可以是 rest。
2. 数组解构也可以用于函数参数。如果数组参数是可选的，则始终需要将[]指定为默认值，并在左侧提供默认值。
ESLINT: prefer-destructuring

- 【推荐】 使用扩展（spread）运算符 复制数组
说明： 扩展运算符可以通过减少赋值语句的使用，或者减少通过下标访问数组或对象的方式，使代码更加简洁优雅，可读性更佳。
```javascript
//good
// 通过 … 复制数组
const itemsCopy = [...items];

// 通过 … 合并两个数组
const fooBar = [...foo, ...bar];
```

### 对象
- 【推荐】 尽量在一个地方定义对象的所有属性

- 【强制】 在对象字面量中使用方法简写
说明： ECMAScript 6 中，对象字面量被增强了，写法更加简洁与灵活，同时在定义对象的时候能够做的事情更多了，具体表现在：
1. 可以在字面量对象里面定义原型方法。
2. 定义方法可以不用 function 关键字，参见 ES6 类相关语法规则。
```javascript
//bad
return {
  stuff: 'candy', 
  method: function() {
    return this.stuff; // return 'candy'
  },
};

//good
return {
  stuff: 'candy',
  method() { // 可以不用function关键字
    return this.stuff; // return 'candy'
  },
};

return {
  stuff: 'fruit',
  method: () => this.stuff, // return 'fruit'
};
```

- 【推荐】 使用点号来访问对象的属性，只有属性是动态的时候使用 []
说明： 在 JavaScript 中，你可以使用点号 (foo.bar) 或者方括号(foo['bar'])来访问属性。然而，点号通常是首选，因为它更加易读，简洁，也更适于 JavaScript 压缩。
```javascript
const foo = 1;
const bar = 2;
const obj = {
  foo, // 等价foo: foo,
  bar, // 等价bar: bar,
  method() {
    return this.foo + this.bar;
  },
};
```

- 【强制】 getter 和 setter 应该成对出现在对象中
说明： 创建一个对象，其某个属性只有 setter 而没有对应的 getter，这在 Javascript 中是个常犯的错误。没有 getter，你不能读取这个属性，该属性也就不会被用到。
```javascript
//bad
const obj = {
  set name(value) {
    this.nameVal = value;
  }
};

//good
const obj = {
  set name(value) {
    this.nameVal = value;
  },

  get name() {
    return this.nameVal;
  },
};
```
ESLINT: accessor-pairs

- 【强制】【推荐】 需要约束 for-in
说明： 在使用 for-in 遍历对象时，会把从原型链继承来的属性也包括进来。这样会导致意想不到的项出现。因此，应该在 for-in 循环中使用 Object.prototype.hasOwnProperty 来排除不需要的原型属性。也可以使用 Object.keys()替代 for-in 进行遍历。

- 【推荐】 使用扩展运算符 ... 进行对象复制
说明： 对象扩展是在 ECMAScript 6 中引入的，它可能比 Object.assign 执行得更好，也更简单易懂。
ESLINT: prefer-spread

## 运算与表达式
### 条件表达式
- 【强制】 在条件判断中，应该变量在先，字面量在第二的位置
说明： 变量在先，字面量在第二的位置是用更自然的方式去描述对比，可读性更好。并且工具也帮我们解决了使用 = 代替 == 的错误风险（ESLint 将为你捕获这个错误）。
ESLINT: yoda

- 【强制】 判断相等时使用 = 和 ! ，而不是 == 和 !=
说明： JavaScript 中使用双等 做相等判断时会自动做类型转换，如：[] false、[] ![]、3 '03'都是 true，当类型确定时使用全等 === 可以提高效率。
ESLINT: eqeqeq

- 【推荐】 推荐使用简写的条件表达式
说明： 对于条件表达式，例如 if 语句，JavaScript 引擎会强制计算它们的表达式为布尔值，规则如下：
1. 对象 被计算为 true；
2. undefined 被计算为 false；
3. null 被计算为 false；
4. 布尔值 被计算为 布尔的值；
5. 数字 如果是 +0、-0、或 NaN 被计算为 false, 否则为 true；
6. 字符串 如果是空字符串 '' 被计算为 false，否则为 true。注意：字符串应该使用显式比较。

- 【推荐】 简化条件表达式
说明： 复杂的表达式难以理解，会增加软件维护的成本和出错的几率。建议将复杂的表达式语句封装到某个方法里，通过方法名来概括该表达式的含义。
```javascript
//bad
if ((employee.flags || HOURLEY_FLAG) && employee.age > 65 && isPass) {
// ...
}

//good
if (employee.isFullBenefits()) {
  // ...
}
function isFullBenefits (employee) {
  return (employee.flags || HOURLEY_FLAG) && employee.age > 65 && isPass;
}
```

- 【参考】 不要在复杂的条件表达式前加个否定操作符
说明： 在一个复杂条件表达式前面再加一个 !，会使代码非常难以理解。

- 【强制】 禁止使用嵌套的三元表达式
说明： 嵌套的三元表达式使代码更加难以理解。
```javascript
//bad
foo ? baz === qux ? quxx() : foobar() : bar();
//good
if (foo) {
  if (baz === qux) {
     quxx();
  } else {
    foobar();
  }
} else {
  bar();
}
```
ESLINT: no-nested-ternary

- 【推荐】 在混合使用不同的操作符时，采用括号明确运算的优先级
说明： 在混合使用不同的操作符中，使用括号说明优先级，以方便阅读和理解。即使代码优先级正确，也应该考虑到后续阅读或者修改这段代码的其他人员的方便，最好加上括号。

### 控制语句
- 【强制】 每个 switch 语句都包含一个 default 语句，即使它不包含任何代码
说明： 考虑到开发人员可能会忘记定义默认分支而导致程序发生错误，所以明确规定在 switch 语句中强制声明 default 分支。或者也可以在最后一个 case 分支下，使用 // no default 来表明此处不需要 default
分支。注释可以任何形式出现，比如 // No Default。
ESLINT: default-case

- 【强制】 在 switch 语句的每一个有内容的 case 中都放置一条 break 语句
说明： 遗漏 break，可能导致程序误入下一个 case 分支，执行了预期之外的代码，导致异常。而且， 不推荐做出有意不写 break 的设计。

- 【推荐】 case 语句中需要声明词法时, 花括号{}不能省略
说明： 词法声明在整个 switch 语句块中是可见的，但是它只有在运行到它定义的 case 语句时，才会进行初始化操作。为了保证词法声明语句只在当前 case 语句中有效，应该将语句包裹在块中。

- 【推荐】 case 对于 if-else if（有多个 else if）类型的条件判断，最后必须包含一个 else 分支
说明： 多个 else if 条件组合的判断逻辑，往往会出现开发人员忽略的分支，需要设置 else 默认操作(类似中 switch-case 语句要有 default 分支) 。

- 【强制】 不要用逻辑运算符代替控制语句
```javascript
//bad
!isRunning && startRunning();
//good
if (!isRunning) {
  startRunning();
}
```

### 正则表达式
- 【推荐】 禁止正则表达式字面量中出现多个空格
说明： 正则表达式可以很复杂和难以理解，这就是为什么要保持它们尽可能的简单，以避免出现错误。在使用正则表达式时使用了多个空格很容易出错。

- 【推荐】 建议在正则表达式中使用命名捕获组
说明： 随着 ECMAScript 2018 的发布，命名捕获组可以用于正则表达式中，这可以提高正则表达式的可读性。


## 其它
### 模块
- 【推荐】 代码中总是使用 ECMAScript 6 标准的模块(import/export)方式，而不是使用非标准的模块化加载器（NodeJS 代码除外）
说明： 这样可以使代码具备很好的可读性。尽管现在有一些浏览器不支持 ECMAScript 6 的模块方式， 但是可以通过编译器的编译进行兼容。
```javascript
//bad
// 使用了三方的模块化实现
const AirbnbStyleGuide = require('./AirbnbStyleGuide');

//good
// 导入defalut export模块
import AirbnbStyleGuide from './AirbnbStyleGuide';
// 导入指定的export模块
import { es6 } from './AirbnbStyleGuide';
//运行时加载
const AirbnbStyleGuide = await import('./AirbnbStyleGuide');
```

- 【推荐】 不要 import 通配符 *
说明： import * 会导入所有 export 模块，建议只导入 default export 模块。
```javascript
//bad
// 导入所有export模块（不包含default export）
import * as AirbnbStyleGuide from './AirbnbStyleGuide';

//good
// 导入defalut export模块
import AirbnbStyleGuide from './AirbnbStyleGuide';
```

- 【推荐】 不要多次 import 同一个模块
```javascript
//bad
import React from 'react';
import { useState } from 'react';

//good
import React, { useState } from 'react';
```
ESLINT: no-duplicate-imports

- 【推荐】 模块 import 的顺序依次为内置模块（如：Nodejs 模块）、外部模块、内部模块
ESLINT: sort-imports

- 【推荐】 import 不要包括 JavaScript 文件扩展名
说明：编译工具允许您忽略导入源路径中的文件扩展名，例如：编译工具可以将./foo/bar 解析为绝对路径/User/someone/foo/bar.js，因为默认情况下.js 扩展名是自动解析的。根据不同场景，您可以配置更多的自动解析扩展。

- 【强制】 import 不使用绝对路径
- 【强制】 import 的三方件需要加入到 package.json 依赖中
说明：添加三方组件时需要加入 --save 可以自动保存到 package.json 中，三方组件需要固定版本

- 【推荐】 当模块中只有一个 export 时，最好使用默认导出，而不是命名导出
说明： 推荐默认导出的原因是鼓励更多只导出一件事的文件，这有利于可读性和可维护性。

- 【推荐】 对于首屏不需要的模块建议采用动态加载以提升性能。
```javascript
//bad
export function foo() {
}
//good
export default function foo() {
}
```

- 【推荐】 不要从 import 中直接 export
说明： 虽然一行代码简洁明了，但让 import 和 export 各司其职，代码可读性更好。
```javascript
//bad
// 以default模块的方式导出es6
export { es6 as default } from './airbnbStyleGuide';

//good
// 导入指定的export模块
import { es6 } from './AirbnbStyleGuide';

// 以default模块的方式导出es6
export default es6;
```

- 【强制】 需要导出的变量必须是 const 类型或不可变的类型
说明： 可以使用 Object.freeze 防止导出对象被引用后篡改。

### 数值与计算
- 【推荐】 禁止省略小数点前后的 0
说明： 在 JavaScript 中，浮点值会包含一个小数点，没有要求小数点之前或之后必须有一个数字。虽然不是一个语法错误，这种格式的数字使真正的小数和点操作符变的难以区分。由于这个原因，有些人建议应该总是在小数点前面和后面有一个数字，以明确表明是要创建一个小数。
```javascript
//bad
const num = .5;
const num = 2.;
const num = -.7;

//good
const num = 0.5;
const num = 2.0;
const num = -0.7;
```

- 【推荐】 要求调用 isNaN() 检查 NaN
说明： 在 JavaScript 中，NaN 是 Number 类型的一个特殊值。它被用来表示非数值，这里的数值是指在 IEEE 浮点数算术标准中定义的双精度 64 位格式的值。 因为在 JavaScript 中 NaN 独特之处在于它不等于任何值，包括它本身，与 NaN 进行比较的结果是令人困惑：NaN !== NaN or NaN != NaN 的值都是 true。因此，使用 Number.isNaN() 或 全局的 isNaN() 函数来测试一个值是否是 NaN。
ESLINT: use-isnan

### 异常
- 【推荐】 异常的使用
说明：
1. 抛出的异常应该是 Error 错误或 Error 的子类，永远不要抛出字符串或其他对象。构造异常时始终使用 new。
ESLINT: no-throw-literal
2. 自定义异常是在函数中传递附加错误信息的好方法，应在默认异常的错误类型不满足的情况下使用自定义异常。
3. 有时候捕获了异常，什么也不做是正确的，但是应该增加注释说明原因。

- 【推荐】 要求使用 Error 对象作为 Promise 拒绝的原因
说明： 在 Promise 中只传递内置的 Error 对象实例给 reject()函数作为自定义错误，被认为是个很好的实践。Error 对象会自动存储堆栈跟踪，在调试时，通过它可以用来确定错误是从哪里来的。如果 Promise 使用了非 Error 的值作为拒绝原因，那么就很难确定 reject 在哪里产生。
ESLINT: prefer-promise-reject-errors

- 【强制】 禁止在 finally 语句块中出现控制流语句
说明： JavaScript 暂停 try 和 catch 语句块中的控制流语句，直到 finally 语句块执行完毕。所以， 当 return、throw、break 和 continue 出现在 finally 中时，try 和 catch 语句块中的控制流语句将被覆盖，这被认为是意外的行为
ESLINT: [no-unsafe-finally](https://eslint.bootcss.com/docs/rules/no-unsafe-finally)

### 异步
- 【强制】 禁用不必要的 return await
说明： 在 async function 中，return await 很少有用。因为 asyncfunction 的返回值总是封装在 Promise.resolve，return await 实际上并没有做任何事情，只是在 Promise resolve 或 reject 之前增加了额外的时间。唯一有效是，如果 try/catch 语句中使用 return await 来捕获另一个基于 Promise 的函数的错误，则会出现异常。
ESLINT: no-return-await

### 避免
- 【强制】 使用不安全的 api
- 【强制】 禁止使用 eval()
说明：
1. eval()接收一个参数 content，如果 content 不是字符串，则直接返回 content，否则执 content 语句。如果 content 语句执行结果是一个值，则返回此值，否则返回 undefined。这样会让程序比较混乱，导致可读性较差。
2. 当 eval() 里面包含用户输入的话存在安全风险。在不可信的代码里使用 eval()有可能使程序受到不同的注入攻击。建议用其它更佳、更清晰、更安全的方式写你的代码。反序列化操作推荐使用安全的 JSON.parse
ESLINT: no-eval

- 【强制】 禁止使用隐式的 eval()
说明： 在 JavaScript 中避免使用 eval() 被认为是一个很好的实践。有一些其它方式，通过传递一个字符串，并将它解析为 JavaScript 代码，也有类似的问题。首当其冲的就是 setTimeout()、setInterval() 或者 execScript() (仅限 IE 浏览器)，它们都可以接受一个 JavaScript 字符串代码作为第一个参数。
ESLINT: no-implied-eval

- 【强制】 禁止使用 with() {}
说明： 使用 with 让你的代码在语义上变得不清晰. 因为 with 的对象，可能会与局部变量产生冲突， 从而改变你程序原本的用义。
ESLINT: no-with

- 【强制】 不要使用 continue
说明： continue 语句终止当前的循环的此次迭代或带标签的循环，执行循环中的下一个迭代。不正确的使用会降低代码可测性、可读性以及可维护性。应使用结构化的控制语句如 if 来代替。
ESLINT: no-continue

- 【强制】 禁用 console
说明： 在 JavaScript，虽然 console 被设计为在浏览器中执行的，但避免使用 console 的方法被认为是一种最佳实践。这样的消息被认为是用于调试的，因此不适合输出到客户端。通常，在发布到产品之前应该剔除 console 的调用。
ESLINT: no-console

- 【强制】 禁用 alert
说明： JavaScript 的 alert、confirm 和 prompt 被广泛认为是突兀的 U 元素，应该被一个更合适的自定义的 UI 界面代替。此外，alert**经常被用于调试代码，部署到生产环境之前应该删除。
ESLINT: no-alert

- 【强制】 禁用 debugger
说明： debugger 语句用于告诉 JavaScript 执行环境停止执行并在代码的当前位置启动调试器。随着现代调试和开发工具的出现，使用调试器已不是最佳实践。部署到生产环境之前应该删除。
ESLINT: no-debugger

- 【强制】 禁止使用较短的符号实现类型转换
说明： 在 JavaScript 中，有许多不同的方式进行类型转换。其中有些可能难于阅读和理解。

- 【强制】 禁止多余的 return 语句
说明： 如果 return 句是多余的，并且在函数执行过程中不会产生效果。这可能令人困惑，因此最好禁止使用这些多余的语句。
ESLINT: no-useless-return

- 【强制】 不要在 HTML 中写大量 JavaScript 或 CSS 代码
说明： 在 HTML 中写大量 JavaScript 和 CSS 代码，不利于代码重用，并且可读性和维护性较差。除了特殊业务需要（如要求极致高性能），都应该在 html 中通过外部方式引入 JavaScript 代码。

- 【强制】 不要使用非标准的功能
说明： 不要使用非标准功能。这包括已删除的旧功能（例如，WeakMap.clear），尚未标准化的新功能（例如，当前的 TC39 工作草案，任何阶段的提案或提议但尚未完成的 Web 标准），或者仅在某些浏览器中实现的专有功能。禁止使用非标准语言"扩展"（例如某些外部转发器提供的扩展）。请使用当前 ECMA-262 或 WHATWG 标准中定义的功能。 （请注意，针对特定 API 编写的项目， 例如 Chrome 扩展或 Node.js，是可以被使用的）。

- 【强制】 防止内存泄漏
- 【强制】 销毁不再使用的定时器和延时器
说明： 在页面局部刷新时，若没有清理定时器或延时器，每个定时器或延时器对应闭包所引用的 DOM 可能无法释放，导致内存泄漏。

## 安全规范
### 防范 CSRF
1. 说明
> 跨站请求伪造（CSRF）是一种挟制终端用户在当前已登录的 Web 应用程序上执行非本意的操作的攻击方法。攻击者可以迫使用户去执行攻击者预先设置的操作，例如，如果用户登录网络银行去查看其存款余额，他没有退出网络银行系统就去浏览自己喜欢的论坛，如果攻击者在论坛中精心构造了一个恶意的链接并诱使该用户点击了该链接，那么该用户在网络银行账户中的资金就有可能被转移到攻击者指定的账号中。当 CSRF 针对普通用户发动攻击时，将对终端用户的数据和操作指令构成严重的威胁；当受攻击的终端用户具有管理员帐号的时候，CSRF 攻击将危及整个 Web 应用程序。

2. 实施指导
> 以 axios 为例，需要在所有服务请求头加入 X-CSRF-TOKEN，其中 CSRF Token 是在打开 SPA 应用时，从服务后台返回，并存放起来。

### HTML5
- 【强制】 访问 Web SQL database 数据库时，使用参数绑定 SQL 语句。
- 【强制】 禁止将 WebSocket 接收到的数据直接嵌入到 DOM 中或当做代码执行，如果服务器端的响应是 JSON 格式的数据，则使用安全函数 JSON.parse()对其进行处理。
- 【强制】 规则 21.5.2 如果获取用户许可时声明了用户位置信息的使用范围，那么，实际使用不能超出声明的范围。
### 存储安全
- 【强制】 禁止应用在客户端明文保存用户的敏感信息。
- 【推荐】 如果在同一个源（origin）上部署多个应用，则禁止应用使用 localStorage 对象存储数据。
- 【强制】 如果数据仅需要临时存储在客户端，使用会话 cookie 或者 sessionStorage 而不是 localStorage。
- 【推荐】 发送到客户端的源文件中，其注释信息不允许包含敏感信息，包括但不限于物理路径、数据库连接等；为了减少信息泄漏，动态网页应尽量仅使用不发送到客户端的隐藏注释；

ESLINT 规范集
[https://github.com/airbnb/javascript](https://github.com/airbnb/javascript)
airbnb-base

[https://github.com/standard/standard](https://github.com/standard/standard)
Standard

[https://github.com/rwaldron/idiomatic.js/](https://github.com/rwaldron/idiomatic.js/)
Idiomatic

[https://google.github.io/styleguide/jsguide.html](https://google.github.io/styleguide/jsguide.html)
Google

以 airbnb 为例，或者自定义一套 eslint 规范集
npm install eslint-config-airbnb-base
// .eslintrc.js
extends: 'airbnb-base'

## Vue 编码规约
###  编码风格
- 【强制】组件应该是自闭合。eslint：vue/html-self-closing
在单文件组件、字符串模板和 JSX 中没有内容的组件应该是自闭合的，但是在 DOM 模板中永远不要这样子做。
```vue
<!-- BAD -->
<!-- 在单文件组件、字符串模板和 JSX 中 -->
<MyComponent></MyComponent>
<!-- 在 DOM 模板中 -->
<my-component />

<!-- GOOD -->
<!-- 在单文件组件、字符串模板和 JSX 中 -->
<MyComponent />
<!-- 在 DOM 模板中 -->
<my-component></my-component>
```

### 语言特性
#### Props
- 【强制】 Props 的定义应该尽量详细，至少需要指定其类型。eslint: vue/valid-define-props
```javascript
// BAD
defineProps(["status"])

// GOOD
defineProps({
  status: String;
})

// Even better!
defineProps({
  status: {
    type: String,
    required: true,

    validator: value => {
      return [
        'syncing',
        'synced',
        'version-conflict',
        'error'
      ].includes(value)
    }
  }
})

// With TS
defineProps<{
  status: 'syncing' | 'synced' | 'version-conflict' | 'error'
}>()
```

- 【强制】禁止设置无效的 Prop 默认值。eslint：vue/require-valid-default-prop
```javascript
// BAD
defineProps({
  userInfo: Array;
  default: [],
})

defineProps({
  userInfo: Object;
  default: {},
})

// GOOD
defineProps({
  userInfo: Array;
  default: () => [],
})

defineProps({
  userInfo: Object;
  default: () => ({}),
})
```

- 【强制】采用小写连字符命名 prop。eslint：vue/attribute-hyphenation
```vue
<template>
  <!-- BAD -->
  <MyComponent myProp="prop" />
  <!-- GOOD -->
  <MyComponent my-prop="prop" />
</template>
```

- 【推荐】除了 isRequired 的属性, 其余需要指定 default。
```javascript
// BAD
defineProps({
  userInfo: Array;
})

// GOOD
defineProps({
  userInfo: Array;
  default: () => [],
})

defineProps({
  userInfo: Object;
  isRequired: true
})

// With TS
defineProps<{
  userInfo: Record<string, any>;
}>()

withDefaults(defineProps<{
  userInfo?: Record<string, any>;
}>(),{
  userInfo: () => ({})
})
```

- 【推荐】prop 值为 true 时，可以省略它的值。
```vue
<!-- BAD -->
<Foo hidden="true" />
<!-- GOOD -->
<Foo hidden />
```

#### Computed
- 【强制】禁止在计算属性中执行异步操作。eslint: vue/no-async-in-computed-properties
```vue
<!-- BAD -->
<script setup>
  import { computed } from "vue";

  const pro = computed(() => {
    return Promise.all([new Promise((resolve, reject) => {})]);
  });

  const foo1 = computed(async () => {
    return await someFunc();
  });

  const bar = computed(async () => {
    fetch(url).then((response) => {});
  });

  const tim = computed(async () => {
    setTimeout(() => {}, 0);
  });

  const inter = computed(async () => {
    setInterval(() => {}, 0);
  });

  const anim = computed(async () => {
    requestAnimationFrame(() => {});
  });
</script>
```

- 【强制】禁止在计算属性中对属性修改。eslint: vue/no-side-effects-in-computed-properties
在计算属性和函数中引入副作用被认为是一种非常糟糕的做法。它使代码不可预测且难以理解。
```vue
<script setup>
  import { ref, computed } from "vue";

  const firstName = ref("lorem");
  const lastName = ref("Jack");

  // BAD
  const fullName = computed(() => {
    firstName.value = "Jim";
    return `${firstName.value} ${lastName.value}`;
  });
</script>
```

- 【强制】计算属性必须含有返回值。eslint: vue/return-in-computed-property
```vue
<script setup>
  import { ref, computed } from "vue";

  // BAD
  const userInfo = computed(() => {
    if (isVIP) {
      return users.filters((user) => user.vip);
    }
  });

  const foo = computed(() => { });

  // GOOD
  const userInfo = computed(() => {
    if (isVIP.value) {
      return users.value.filters((user) => user.vip);
    } else {
      return users;.value
    }
  });
</script>
```

- 【参考】把复杂计算属性分割为尽可能多的更简单的属性。
简单的计算属性可以使得代码的可读性更高，同时在写单元测试时更简单。
```javascript
// BAD
const price = computed(() => {
  var basePrice = manufactureCost.value / (1 - profitMargin.value);
  return basePrice - basePrice * (discountPercent.value || 0);
});

// GOOD
const basePrice = computed(() => {
  return manufactureCost.value - profitMargin.value;
});

const discount = computed(() => {
  return basePrice.value * (discountPercent.value || 0);
});

const finalPrice = computed(() => {
  return basePrice.value - discount.value;
});
```

#### Watch
- 【强制】禁止异步声明 watch。eslint：vue/no-watch-after-await
```javascript
import { watch } from "vue";
export default {
  async setup() {
    // GOOD
    watch(watchSource, () => {
      // ...
    });

    await doSomething();

    // BAD
    watch(watchSource, () => {
      // ...
    });
  },
};
```

#### 列表渲染
- 【强制】禁止在使用 v-for 未绑定键值, 且推荐不使用索引作为键值。eslint：vue/require-v-for-key
组件列表渲染必须用 key 配合 v-for，以便维护内部组件及其子树的状态，同时尽量避免采用 index 作为键值。Vue 在渲染组件时通过 key 标识哪些项已更改、已添加或者已删除，key 值应该始终稳定，且更够唯一表识元素。如果数组重新排序或将元素添加到数组的开头，可能会更改索引导致不必要的渲染，对性能产生负面影响。

如果数组的顺序可能发生变化，我们不推荐使用索引值作为 key。
```vue
<!-- BAD -->
<ul>
  <li v-for="todo in todos">{{ todo.text }}</li>
</ul>

<!-- GOOD -->
<ul>
  <li v-for="todo in todos" :key="todo.id">{{ todo.text }}</li>
</ul>
```

- 【强制】避免 v-if 与 v-for 同时使用在同一元素上。eslint：vue/no-use-v-if-with-v-for
因为v-for的优先级比v-if的优先级高，所以每次渲染时都会先循环再进行条件判断，二者在一起会特别消耗性能。可以采用两种方式去解决这个问题：

1. 将列表渲染中数据替换为可以返回筛选后的计算属性
2. 添加 template 标签
```vue
<!-- BAD -->
<ul>
  <li v-for="user in users" v-if="user.isActive" :key="user.id">
    {{ user.name }}
  </li>
</ul>

<!-- GOOD -->
<ul>
  <template v-for="user in users" :key="user.id">
    <li v-if="user.isActive">{{ user.name }}</li>
  </template>
</ul>

<!-- GOOD -->
<ul>
  <li v-for="user in users" :key="user.id">{{ user.name }}</li>
</ul>

<script setup>
  const activeUser = computed(() => {
    return users.value.filter((user) => user.isActive);
  });
</script>
```

#### 顺序
- 【强制】元素属性的排序规则。eslint：vue/attributes-order
元素（包括组件）的属性应该有统一的排序，按固定的顺序排序可以增强代码的一致。
1. 定义: is
2. 列表渲染: v-for
3. 条件渲染: v-if, v-else-if, v-else, v-show, v-cloak
4. 渲染方式: v-pre, v-once
5. 全局感知: id
6. 唯一属性: ref, key
7. 双向绑定: v-model
8. 其他属性: v-bind
9. 事件: v-on
10. 内容渲染: v-html, v-text

- 【强制】单文件组件顶层元素的排序规则。eslint：vue/component-tags-order
单文件组件应该总是以  <script>、<template>  和  <style>  标签的顺序保持一致，且  <style>  要放在最后，因为另外两个标签至少要有一个。
```vue
<!-- GOOD -->
<!-- ComponentA.vue -->
<script>
  /* ... */
</script>
<template>...</template>
<style>
  /* ... */
</style>

<!-- ComponentB.vue -->
<script>
  /* ... */
</script>
<template>...</template>
<style>
  /* ... */
</style>
```

- 【推荐】defineEmits 和 defineProps 的排序规则。eslint：vue/define-macros-order
```vue
<script setup>
  // BAD
  defineEmits(/* ... */);
  defineProps(/* ... */);

  // GOOD
  defineProps(/* ... */);
  defineEmits(/* ... */);
</script>
```

- 【推荐】组件名中的单词的排序规则。
组件名称应以最高级别（通常是最通用的）的单词开头，并以描述性修饰词结尾。
```javascript
// BAD
components/
|- ClearSearchButton.vue
|- ExcludeFromSearchInput.vue
|- LaunchOnStartupCheckbox.vue
|- RunSearchButton.vue
|- SearchInput.vue
|- TermsCheckbox.vue

// GOOD
components/
|- SearchButtonClear.vue
|- SearchButtonRun.vue
|- SearchInputQuery.vue
|- SearchInputExcludeGlob.vue
|- SettingsCheckboxTerms.vue
|- SettingsCheckboxLaunchOnStartup.vue
```

#### 样式作用域
- 【推荐】为组件样式添加作用域。
对于应用来说，顶层的 APP 组件和布局组件中的样式是可以全局的，其他所有的单文件组件都应该是有作用域的。
```vue
<!-- BAD -->
<template>
  <button class="btn btn-close">×</button>
</template>

<style>
  .btn-close {
    background-color: red;
  }
</style>

<!-- GOOD -->
<template>
  <button class="button button-close">×</button>
</template>

<!-- Using the `scoped` attribute -->
<style scoped>
  .button {
    border: none;
    border-radius: 2px;
  }

  .button-close {
    background-color: red;
  }
</style>

<!-- GOOD -->
<template>
  <button :class="[$style.button, $style.buttonClose]">×</button>
</template>

<!-- Using the BEM convention -->
<style module>
  .button {
    border: none;
    border-radius: 2px;
  }

  .buttonClose {
    background-color: red;
  }
</style>

<!-- GOOD -->
<template>
  <button class="c-Button c-Button--close">×</button>
</template>

<!-- Using the BEM convention -->
<style>
  .c-Button {
    border: none;
    border-radius: 2px;
  }

  .c-Button--close {
    background-color: red;
  }
</style>
```

- 【推荐】在 scoped 中的优先使用类选择器。
在  scoped  样式中，类选择器比元素选择器更好，因为大量使用元素选择器会导致页面渲染很慢，元素选择器应该避免在  scoped  中出现。

这是因为为了给样式设置作用域，Vue 应用会为元素添加一个独一无二的属性，例如  data-v-f3f3eg9，使得在匹配选择器的元素中，只有带这个属性才会真正生效 (比如 button[data-v-f3f3eg9])。如果使用大量的元素选择器 (比如 button[data-v-f3f3eg9]) 会比类选择器 (比如 .btn-close[data-v-f3f3eg9]) 慢，所以应该尽可能选用类选择器进行样式作用域的控制。
```vue
<!-- BAD -->
<template>
  <button>×</button>
</template>

<style scoped>
  button {
    background-color: red;
  }
</style>

<!-- GOOD -->
<template>
  <button class="btn btn-close">×</button>
</template>

<style scoped>
  .btn-close {
    background-color: red;
  }
</style>
```

#### 指令和组件模板
- 【强制】指令缩写应该统一。eslint：vue/v-slot-style
指令缩写 (用  :  表示  v-bind:、用  @  表示  v-on:  和用  #  表示  v-slot:) 应该要么都用要么都不用。
不好示例：
```vue
<!-- BAD -->
<input v-bind:value="newTodoText" :placeholder="newTodoInstructions" />
<input v-on:input="onInput" @focus="onFocus" />
<template v-slot:header>
  <h1>Here might be a page title</h1>
</template>
<template #footer>
  <p>Here's some contact info</p>
</template>

<!-- GOOD -->
<input :value="newTodoText" :placeholder="newTodoInstructions" />
<input @input="onInput" @focus="onFocus" />
<template #header>
  <h1>Here might be a page title</h1>
</template>
```

- 【推荐】组件模板中使用简单的表达式。
组件模板应该只包含简单的表达式，复杂的表达式则应该重构为计算属性或方法。
```vue
<!-- BAD -->
<template>
  {{ fullName.split(' ').map(function (word) { return word[0].toUpperCase() +
  word.slice(1) }).join(' ') }}
</template>

<!-- GOOD -->
<template>{{ normalizedFullName }}</template>

<script setup>
  import { computed } from "vue";

  const normalizedFullName = computed(() => {
    return fullName.value
      .split(" ")
      .map((word) => {
        return word[0].toUpperCase() + word.slice(1);
      })
      .join(" ");
  });
</script>
```

#### Mixins
- 【推荐】 不再使用 mixins。
对于 Vue 应用，推荐不再使用 mixins，vue3 支持 mixins 主要是为了向后兼容 (一些工具库)。Mixins 引入了隐式依赖，可能导致命名冲突，并导致滚雪球式的复杂度。

大多数使用 mixins 的场景都可以通过组件、工具模块或组合式函数（Vue2 使用 @vue/composition-api）完成。组合式函数可以实现更加简洁高效的逻辑复用，其能力孵化了一些非常棒的社区项目，例如：VueUse。

一个简单的组合式函数示例：
```javascript
// GOOD
// useUser.js 抽离与user相关的逻辑
const useUser = () => {
  const userInfo = ref({});

  const getUserInfo = () => {}; // 获取用户状态
  // ... 等其他逻辑

  return {
    userInfo,
    getUserInfo,
  };
};

// UserHome.vue
// 在单文件组件中直接使用
import { useUser } from "./useUser";

const { userInfo, getUserInfo } = useUser();

getUserInfo();
```

#### 父子组件通信
- 【推荐】优先通过 prop 和 emit 进行父子组件之间的通信。
一个理想的 Vue 应用是 prop 向下传递，emit 向上传递的，遵循这一约定会让你的组件更易于理解，不要为了一时方便 (少写代码) 而牺牲数据流向的简洁性 (易于理解)。

不好示例：
```js
// useDialog.js
import { ref } from "vue";

export const dialogVisible = ref(false);
<!-- DialogHome.vue -->
<template>
  <el-button @click="dialogVisible = true">详情</el-button>
  <DialogDetail /> </template
>;

<script setup>
  import { dialogVisible } from "./useDialog";
</script>

<!-- DialogDetail.vue -->
<template>
  <el-dialog v-model="dialogVisible" @confirm="onConfirm">
    <!-- ... -->
  </el-dialog> </template
>;

<script setup>
  import { dialogVisible } from "./useDialog";

  const onConfirm = () => {
    dialogVisible = false;
  };
</script>
```
上述写法同一个数据的变更存在多个组件，在一些复杂情况下会使得代码难以理解，推荐写法是把数据的变更维护在同一个地方，然后通过 props 和 emits 进行数据的传递。同时这种做法还会使得数据常驻，导致其他不可预知的错误，推荐通过导出函数的方式，然后在单文件组件中通过数据结构进行使用。

推荐示例：
```javascript
// useDialog.js
import { ref } from "vue";

export const useDialog = () => {
  const dialogVisible = ref(false);

  return {
    dialogVisible,
  };
};
<!-- DialogHome.vue -->
<template>
  <el-button @click="dialogVisible = true">详情</el-button>
  <DialogDetail v-model="dialogVisible" /> </template
>;

<script setup>
  import { useDialog } from "./useDialog";

  const { dialogVisible } = useDialog();
</script>

<!-- DialogDetail.vue -->
<template>
  <el-dialog v-model="dialogVisible" @confirm="onConfirm">
    <!-- ... -->
  </el-dialog> </template
>;

<script setup>
  import { computed } from "vue";

  const props = defineProps({
    modelValue: {
      type: Boolean,
      default: false,
    },
  });
  const emits = defineEmits(["update:modelValue"]);

  const dialogVisible = computed({
    get() {
      return props.modelValue;
    },
    set(val) {
      emits("update:modelValue", val);
    },
  });
</script>
```

#### 全局状态
- 【参考】优先通过 Vuex/Pinia 进行全局状态管理。
Vuex 和 Pinia可以通过浏览器的调试工具 Devtools 更好地组织、追踪和调试数据状态，同时像 Pinia 还支持热更新以及服务端渲染。在一些特殊情况下，再考虑通过全局事件总线 EventBus 或者 Provide/Inject 变更全局数据状态。

在使用 Vue3 的单页面应用中，也可以采用响应式 API reactive,例如, export const state = reactive({}) 来共享一个全局状态，但如果应用在服务端渲染，这种做法会使应用暴露出一些安全漏洞，不推荐这样使用。

#### 命名
- 【强制】禁止在一个文件内声明两个组件。eslint：vue/one-component-per-file
只要能够单独构成模块的系统，就把每个组件单独分成文件，好的组件文件分类有助于在需要编辑组件或查阅组件用法时更快地找到它。
```javascript
// BAD
// app.js
const ToDoList = app.component("TodoList", {});
const ToDoItem = app.component("TodoItem", {});

// GOOD
components/
|- TodoList.vue
|- TodoItem.vue
```

- 【强制】组件名应为多个单词。eslint：vue/multi-word-component-names
组件名应该始终是多个单词组成，根组件 <App> 以及 <transition>、<component> 之类的 Vue 内置组件除外，这样可以避免与现有以及未来的 HTML 元素冲突，因为目前所有的 HTML 元素名称都是单个单词。
```javascript
<!-- BAD -->
<Item />
<item></item>

<!-- GOOD -->
<TodoItem />
<todo-item></todo-item>
// BAD
export default { name: 'Detail' }

// GOOD
export default { name: 'TodoDetail' }
```

- 【强制】组件名称大小写规则。eslint：vue/component-definition-name-casing
为了保持代码一致性，组件名应该始终是 PascalCase 的形式，在较为简单的应用中只使用  app.component  进行全局组件注册时，可以使用 kebab-case 字符串。

不好示例：
```javascript
// BAD
app.component('myComponent', {})

import myComponent from './MyComponent.vue'
export default {
  name: 'myComponent',
  // or
  name: 'my-component'
  // ...
}

// GOOD
app.component("MyComponent", {});
app.component("my-component", {});

export default {
  name: "MyComponent",
  // ...
};
```

- 【强制】模板中组件的命名规则。eslint：vue/component-name-in-template-casing
为了保持代码一致性，在单文件组件和字符串模板中，组件名应该始终为 PascalCase，由于 HTML 是大小写不敏感的，在 DOM 模板中必须是 kebab-case 的。推荐统一为 kebab-case 形式。
```vue
<!-- BAD -->
<!-- 在单文件组件和字符串模板中 -->
<mycomponent />
<myComponent />
<!-- 在 DOM 模板中 -->
<MyComponent></MyComponent>

<!-- GOOD -->
<!-- 在单文件组件和字符串模板中 -->
<MyComponent />
<!-- 在 DOM 模板中 -->
<my-component></my-component>
亦或者

<!-- 在所有地方 -->
<my-component></my-component>
```

- 【推荐】组件文件名的大小写规则。
为了保持工程一致性，单文件组件的文件名应该始终为单词大写开头（PascalCase），或者为横线连接（kebab-case）。
```javascript
// BAD
components/
|- mycomponent.vue
components/
|- myComponent.vue

// GOOD
components/
|- MyComponent.vue
components/
|- my-component.vue
```

- 【推荐】基础组件的命名规则。
应用特定样式和约束的基础组件（无逻辑或者无状态的组件）应该全部以一个特定前缀开头，例如Base、APP或者V 等。这样做的几个好处：

当你在编辑器中以字母顺序排序时，你的应用的基础组件会全部列在一起，这样更容易识别。
因为组件名应该始终是多个单词，所以这样做可以避免你在包裹简单组件时随意选择前缀 (比如  MyButton、VueButton)。
```javascript
<!-- BAD  -->
components/
|- MyButton.vue
|- VueTable.vue
|- Icon.vue

<!-- GOOD -->
components/
|- BaseButton.vue
|- BaseTable.vue
|- BaseIcon.vue

|- AppButton.vue
|- AppTable.vue
|- AppIcon.vue

|- VButton.vue
|- VTable.vue
|- VIcon.vue
```

- 【推荐】单例组件的命名规则。
只应该拥有单个活跃实例的组件应该以  The  前缀命名，以示其唯一性。这不意味着该组件只可用于一个单页面，而是每个页面只使用一次，且这些组件永远不接受任何 prop。如果你发现有必要添加 prop，那就表明这实际上是一个可复用的组件，只是目前在每个页面里只使用一次。
```javascript
// BAD
components/
|- Heading.vue
|- MySidebar.vue

// GOOD
components/
|- TheHeading.vue
|- TheSidebar.vue
```

- 【推荐】紧密耦合的组件的命名规则。
与其父组件紧密耦合的子组件应包含父组件名称作为前缀。
```javascript
// BAD
components/
|- TodoList.vue
|- TodoItem.vue
|- TodoButton.vue
|- SearchSidebar.vue
|- NavigationForSearchSidebar.vue

// GOOD
components/
|- TodoList.vue
|- TodoListItem.vue
|- TodoListItemButton.vue
|- SearchSidebar.vue
|- SearchSidebarNavigation.vue
```
也可以通过在其父组件命名的目录中嵌套子组件目录解决这个问题。例如：
```javascript
// NOT GOOD
components/
|- TodoList/
   |- Item/
      |- index.vue
      |- Button.vue
   |- index.vue
```
但是这种方式并不推荐，因为这会导致：
1. 许多文件的名字相同，使得在编辑器中快速切换文件变得困难。
2. 过多嵌套的子目录增加了在编辑器侧边栏中浏览组件所花的时间。

## Eslint 配套工具
上述强制规则可以通过 Eslint 插件 eslint-plugin-vue 进行约束，推荐规则需要开发人员尽量遵守，以此确保团队代码风格统一。

目前，该插件提供了四套规则包（base、essential、strongly-recommended、recommended）。在提高代码可读性和开发体验的基础上，结合社区默认规则，推荐使用 plugin:vue/vue3-recommended (Vue2: plugin:vue/recommended)。项目中，使用*.eslintrc.**文件进行配置：
```javascript
module.exports = {
  extends: [
    // add more generic rulesets here, such as:
    // 'eslint:recommended',
    "plugin:vue/vue3-recommended",
    // 'plugin:vue/recommended' // Use this if you are using Vue.js 2.x.
  ],
  rules: {
    // override/add rules settings here, such as:
    // "vue/component-name-in-template-casing": [
    //   "error",
    //   "kebab-case",
    //   {
    //     registeredComponentsOnly: false,
    //     ignores: ["/^el/"],
    //   },
    // ],
  },
};
```

## 参考资料
1. [Eslint-Plugin-Vue](https://eslint.vuejs.org/)
2. [Vue 编码风格指南](https://vuejs.org/style-guide/)
3. [Vue2 编码风格指南](https://v2.cn.vuejs.org/v2/style-guide)
