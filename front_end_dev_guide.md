前端小队开发指引
===================

前端生态了解
-------------------

> TODO：待寻觅合适的slides，如[That Js Dude](http://thatjsdude.com/)里的[What's New in ES6](http://khan4019.github.io/ES6/#/) 

暂时请参考：

* [2015前端生态发展回顾](https://github.com/kuitos/kuitos.github.io/issues/32)
* [2016 JavaScript 技术栈展望](http://pinggod.com/2016/2016-JavaScript-技术栈展望)
* [一名全栈工程师Node.js之路](http://mp.weixin.qq.com/s?__biz=MzAwMDU1MTE1OQ==&mid=405001493&idx=1&sn=f0ecab9b31bad83fb065ac37bb728245)
* [ES2015介绍](http://pinggod.com/2015/ES2015/)
* [前端知识体系教程](https://github.com/AutumnsWind/Front-end-tutorial)
* [前端组件库](https://github.com/JingwenTian/awesome-frontend)
* [Frontend Development](https://github.com/dypsilon/frontend-dev-bookmarks)
* [Awesome JavaScript](https://github.com/sorrycc/awesome-javascript)
* [Awesome之Front-end Development](https://github.com/sindresorhus/awesome#front-end-development)

前端工作流选型
---------------

* 使用[Webpack](https://webpack.github.io/)，而非[Browserify](http://browserify.org/)
* 如果够用，使用`npm`自己的[极简工作流支持](http://blog.keithcirkel.co.uk/how-to-use-npm-as-a-build-tool/)，原因参考：
  - [Why we should stop using Grunt & Gulp](http://blog.keithcirkel.co.uk/why-we-should-stop-using-grunt/)
  - [Choose: Grunt, Gulp, or npm?](https://ponyfoo.com/articles/choose-grunt-gulp-or-npm)
* 如果不够，则使用[Gulp](https://github.com/alferov/awesome-gulp)加以补充
* 不使用[GruntJS](http://gruntjs.com/)

前端技术选型
----------------

* 不支持IE等古代浏览器
* 字符编码选择[UTF-8](http://htmlpurifier.org/docs/enduser-utf8.html)，而非GBK
* [HTML5](https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/HTML5)
  - 直接使用HTML来写时，用[Emmet(旧称"Zen Coding")](http://docs.emmet.io/)和Snippets来提升录入效率
  - 使用[Jade](http://jade-lang.com/) 模版语言来写时，用[HTML2jade](http://www.html2jade.org/)将HTML片段转换为Jade
* [CSS3](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS3)
  - 使用 [SCSS](http://sass-lang.com/guide) 语法来写
  - 不使用SASS语法、[Less](http://lesscss.org/)，原因参见
    + [Less vs Sass? It’s time to switch to Sass](http://www.zingdesign.com/less-vs-sass-its-time-to-switch-to-sass/)
    + [Why Bootstrap 4 Means Sass Has Won](http://developer.telerik.com/featured/why-bootstrap-4-means-sass-has-won/)
  - 建议过程中只使用SCSS的一个子集（主要是Variables、Nested、Mixins），将来可能考虑基于[PostCSS](https://github.com/postcss/postcss)提供类Scss特性的[PreCSS](https://github.com/jonathantneal/precss)
* JS
  - 使用 [ES6](http://es6-features.org/)（又称之为ES2015） 来写
  - 英文资料参考[Overview of ECMAScript 6 features](https://github.com/lukehoban/es6features)
  - 中文资料参考[《ECMAScript 6入门》](http://es6.ruanyifeng.com/)
  - 不使用：
    + ES4：大部分的IE只支持到这个程度，即很多人理解里的古代JS，语言/库特性缺失太严重
    + [ES5](http://yanhaijing.com/es5/)：大部分现代浏览器只支持到这个程度。ES5为JS打下来扎实的语言/库基础。
    + [TypeScript](http://www.typescriptlang.org/)：微软倒腾出来的ES5的超集，增加了类型支持。
    + [CoffeeScript](http://coffeescript.org/)：Rails社区很火过
    + 想要对比这些语言，请见[TypeScript vs. CoffeeScript vs. ES6](http://www.slideshare.net/NeilGreen1/type-script-vs-coffeescript-vs-es6)
  - 使用[Babel](https://babeljs.io/)将ES6预编译成ES5来在浏览器中运行
* CSS框架选用众所周知的Bootstrap
  - 可以选择其[版本3](http://getbootstrap.com/)，受众最广，生态最完善。
    + [AdminLTE模版](https://almsaeedstudio.com/)
    + [Ace - Responsive Admin Template](https://wrapbootstrap.com/theme/ace-responsive-admin-template-WB0B30DGR)
  - 可以考虑直接选择其[版本4](http://v4-alpha.getbootstrap.com/)，参考:
    + 优点是：前端发展一日千里，版本3很快会失去支持，而且版本4提供了很多新的组件和特性，尤其是[Flexbox](https://github.com/afonsopacifer/awesome-flexbox)相关的特性；缺点是：生态还不完善。
    + [What's New in Bootstrap 4?](http://bootstrap4.guide/)
    + [New Bootstrap 4 alpha](http://blog.getbootstrap.com/2015/12/08/bootstrap-4-alpha-2/)
    + [正在开发切换到Bootstrap 4的AdminLTE模版](https://github.com/almasaeed2010/AdminLTE/milestones/v3.0.0-alpha)
    + [Bootstrap 4 Admin Dashboard Demo](http://www.bootstrapzero.com/bootstrap-template/bootstrap-4-admin-dashboard)
* SASS框架：
  - 选择[Bourbon](http://bourbon.io/)+[Neat](http://neat.bourbon.io/)
  - 不选择[Compass](http://compass-style.org/)，原因主要是它太重了，见[Sass Frameworks: Compass or Bourbon?](http://www.sitepoint.com/compass-or-bourbon-sass-frameworks/)
  - 如无必要，可以不要框架
* 几乎必然包含的JS库是[jQuery](http://jquery.com/)
  - 在移动场景，可以换成其轻量移动版本[Zepto](http://zeptojs.com/)
  - 大部分活跃维护的jQuery Plugin一般都同时兼容jQuery和Zepto
* JS框架选用类似[React](https://github.com/enaqx/awesome-react)但更轻的[Vue.js](http://vuejs.org/) ：
  - Vue.js的丰富资料，见[Awesome Vue.js](https://github.com/vuejs/awesome-vue)
  - Vue.js和其他前端框架的比较，请见[Comparison with Other Frameworks](http://vuejs.org/guide/comparison.html)
  - 使用官方的[脚手架vue-cli](https://github.com/vuejs/vue-cli)并采用[utensil/webpack](https://github.com/utensil/webpack)模版来生成项目结构，包含[官方webpack](https://github.com/vuejs-templates/webpack)模版中的：
    + [Webpack](https://webpack.github.io/) + [vue-loader](http://vuejs.github.io/vue-loader) 支持 `.vue`单文件组件
    + [Babel](https://babeljs.io/)来将ES6转换为ES5
    + [Karma](https://karma-runner.github.io)+[Jasmine](http://jasmine.github.io/)来做单元测试
    + [inject-loader](https://github.com/plasticine/inject-loader)来做[Mock](http://vuejs.github.io/vue-loader/workflow/testing-with-mocks.html)
    + [eslint](http://eslint.org/) 做JS静态代码检查
    + [EditorConfig](http://editorconfig.org/) 来配置代码编辑器
  - 加上[Building Large-Scale Apps(Using Vue.js)](http://vuejs.org/guide/application.html)中推荐的：
    + [vue-router](https://github.com/vuejs/vue-router) 解决路由问题
    + [vue-resource](https://github.com/vuejs/vue-resource) 解决和后端[RESTful服务](https://github.com/marmelab/awesome-rest)通讯的问题
    + [Vuex](https://github.com/vuejs/vuex/) 实现 [Flux](https://facebook.github.io/flux/) ，或者采用兼容 React 和 Vue.js 的 [Redux](https://github.com/rackt/redux/)
  - 再加上：
    + [lodash](https://lodash.com/)：JavaScript 工具库，更好地支持函数式编程(参见[各种编程范式](https://en.wikipedia.org/wiki/Comparison_of_programming_paradigms))，弥补一些JavaScript标准库的缺憾
    + `jade`
    + `sass-loader` + `node-sass`
    + [vue-async-data](https://github.com/vuejs/vue-async-data)
    + [vueboot](https://github.com/Morgul/vueboot)
    + 在进行Ajax请求时，请使用[`fetch` API标准](http://github.github.io/fetch/)，不要使用`jQuery.ajax()`或`XMLHttpRequest`，参见：
      - [Fetch Living Standard](https://fetch.spec.whatwg.org/#fetch-api)
      - [That’s so fetch!](http://jakearchibald.com/2015/thats-so-fetch/)
      - [Using webpack with shims and polyfills](http://mts.io/2015/04/08/webpack-shims-polyfills/)

前端开发规范参考
------------------

> 当你难以确定最佳实践的时候，看看业界是怎么做的

* [前端开发规范手册](https://github.com/Aaaaaashu/Front-End-Style-Guide)
* [前端代码规范 及 最佳实践](http://coderlmn.github.io/code-standards/)
* [豆瓣Javascript代码风格规范](https://docs.google.com/document/pub?id=17ICSeE4Qd04-1U-pphmKCAmfgJGEVjqDellbu4oAiqU)
* [豆瓣CSS开发规范](https://docs.google.com/document/pub?id=17dKkWwdaKyNnkwswihHje2cfoMGqbSJLydTIxqFwlQU)
* [网易HTML规范](http://nec.netease.com/standard/html-structure.html)
* [网易CSS开发规范](http://nec.netease.com/standard/css-sort.html)
* [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript/tree/master/es5)
* [Isobar Front-end Code Standards](http://isobar-idev.github.io/code-standards/)
* [PUP Front end standards](http://www.yellowshoe.com.au/standards/)
* [TMWtech Front-End Dev guidelines](http://tech.tmw.co.uk/code/TMW-frontend-guidelines/)
* [Frontend Guidelines](https://github.com/bendc/frontend-guidelines)
* [Front End Development Guidelines](http://taitems.github.io/Front-End-Development-Guidelines/)
* [Front-end Style Guides](https://24ways.org/2011/front-end-style-guides/)
* [20 Useful Docs and Guides for Front-End Developers](http://www.sitepoint.com/20-docs-guides-front-end-developers/)

编辑器选择
-------------

如果喜欢[Atom](https://atom.io/)，建议参考 [utensil的Atom配置](https://github.com/utensil/config/tree/master/IDE/atom) 。

如果喜欢[Sublime Text 3](http://www.sublimetext.com/3)，并搭配插件：

* [EditorConfig](https://github.com/sindresorhus/editorconfig-sublime)
* [Vue Syntax Highlight](https://github.com/vuejs/vue-syntax-highlight)
* [Emmet](https://github.com/sergeche/emmet-sublime)
* [Jade](https://packagecontrol.io/packages/Jade)
* [Sass](https://packagecontrol.io/search/Sass)
* [Color Highlighter](https://packagecontrol.io/packages/Color%20Highlighter)
* [Java Script Next - ES6 Syntax](https://packagecontrol.io/packages/JavaScriptNext%20-%20ES6%20Syntax)
* [HTML5](https://packagecontrol.io/packages/HTML5)
* [Nodejs](https://packagecontrol.io/packages/Nodejs)
* [Bootstrap 4 - Sublime Plugin](https://github.com/mdegoo/sublime-bootstrap4)
* [jQuery](https://packagecontrol.io/packages/jQuery)
* [Convert To UTF8](https://packagecontrol.io/packages/ConvertToUTF8)
* [Sublime Terminal](https://packagecontrol.io/packages/Terminal)

这些插件都可以通过[Sublime Text的插件/包管理器Package Control](https://packagecontrol.io/)来安装。

