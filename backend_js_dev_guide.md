后端Javascript开发指引
=====================

后端Js平台选择
--------------

后端Js开发，一般依托性能强劲、应用广泛的[V8引擎][v8]。而用法又有两种：

* 使用库包丰富、生态成熟、社区活跃的[node.js][node]
* 直接[嵌入V8引擎][embedv8]

前者的指引书籍非常多，本指引关注后者，其中许多内容也适用前者，或者本就从node.js社区而来。

[embedv8]: https://developers.google.com/v8/embed

语言代际选择
-------------

Javascript（下文简称Js）是一门入门极易却又有丰富内涵的语言，其正式的名称是ECMAScript。许多人熟悉的Javascript，不过是它发展早期的语言特性与标准库，大致相当于ECMAScript 4。IE 8之前的IE浏览器，只支持到这个水平。现代浏览器，如Chrome、Firefox等，都对[ECMAScript 5](http://yanhaijing.com/es5/)有较佳的支持。而2015年标准化的ECMAScript 6（又称ECMAScript 2015，下文简称ES6），为Javascript增加了一系列如虎添翼的语言特性与标准库，使得它焕发出全新的活力。

ES6之于Js，正如C++ 11之于C++、Java 8之于Java，解决它们在长期的工程实践中发现的坑与瓶颈之余，让语言跟上最新的编程思想（其实也不算新出现，应该是新成熟、普及）。

不像C++ 11、Java 8在实际工作中普及缓慢，一日千里的前端开发社区，甚至在ES 6标准的起草阶段，就借助于tranpiler(FIXME)与shim/pollyfill技术，使用[Babel](https://babeljs.io/)将ES6预编译成ES5，迅速地在实际工作中使用。而在ES 6这个标准出现之前，社区也流行过类Ruby/Python的CoffeeScript，微软也搞出了TypeScript，更不提[其他的各种转译成Js的语言](TODO)，社区已经适应这种不拘一格用语言的方式快速提高生产力的节奏。想要对比上面提到的几门主流Js语言，请见[TypeScript vs. CoffeeScript vs. ES6](http://www.slideshare.net/NeilGreen1/type-script-vs-coffeescript-vs-es6) 

结论：

使用 [ES6](http://es6-features.org/)来写后端Js。

* 英文资料参考[Overview of ECMAScript 6 features](https://github.com/lukehoban/es6features)
* 中文资料参考[《ECMAScript 6入门》](http://es6.ruanyifeng.com/)

工作流
----------

什么是工作流（Workflow）呢？这里不是指工作流引擎，而是指DRY(不甘于自我重复)的程序员，为提高自己的生产力，而搭建的日常编程任务自动化环境。工作流能够在代码修改后自动被触发，或者在人工指定任务时，做相应的整套处理，如：编译、转换、合并、minify、跑自动化测试用例、启动代码调试环境等等。过去，程序员使用GUI界面的IDE来达成类似的目的；现在，node.js社区使用命令行界面来做到。

这里先给出工作流的选型结论，具体怎么使用，又有什么效果，则在下面每一点里展开。

* 使用[Webpack](https://webpack.github.io/)，而非[Browserify](http://browserify.org/)
* 如果够用，使用`npm`自己的[极简工作流支持](http://blog.keithcirkel.co.uk/how-to-use-npm-as-a-build-tool/)，原因参考：
  - [Why we should stop using Grunt & Gulp](http://blog.keithcirkel.co.uk/why-we-should-stop-using-grunt/)
  - [Choose: Grunt, Gulp, or npm?](https://ponyfoo.com/articles/choose-grunt-gulp-or-npm)
* 如果不够，则使用[Gulp](https://github.com/alferov/awesome-gulp)加以补充
* 不使用[GruntJS](http://gruntjs.com/)

预编译
-----------

C++/Java等是编译型语言，要先编译成机器码或虚拟机字节码，然后才能运行。Js不是编译型语言，.......，怎么也需要预编译？

Js预编译的各环节与效果如下：

###  模块导入

程序员都希望将大块的逻辑，拆分成模块，组织在多个文件里，这样有利于代码的可维护性，也不会使得编辑器因为打开一个巨大的源文件卡顿。Js怎么做到这件事情呢？

在node.js或者其它CommonJS环境下，js可以分成很多小模块，用`module.exports`h或ES6的`export`对模块的函数或变量进行导出，并在需要用到该模块时，用`require`或者ES6的`import`导入使用。在浏览器里，或者嵌入的v8引擎里，虽然也可以多次载入多个Js，但较繁琐且增加IO次数，从而不可能拆细到可维护的粒度。

所以势必需要一种合理的方式，在开发时用细粒度的多个小Js文件，在运行时用粗粒度的少数几个或者1个大Js文件。

一种简陋的方式，就是虽然代码按照模块分开在多个文件中写，但都是全局的函数/变量，从而可以将这所有的Js文件简单地合并后来运行。

这样做的缺点是，在写代码的时候，依然要考虑整个代码基之间潜在的相互耦合或影响，使得模块/文件并不能真正提供一个相对独立的、易于掌控的小单元。

而Browsify、Webpack这样的工作流，就能让人像在node.js里面一样进行模块化开发，并预编译成少数的几个Js文件来运行。

### 语言转换

浏览器目前还不能原生支持ES6的运行，所以需要将ES6转换成ES5来在浏览器中运行。最新的V8的确原生支持ES6的绝大多数特性，但如果生产上嵌入的是老版本的V8引擎，或者我们希望用上最新的语言特性（如ES2016），也是必然需要相应的语言转换步骤的。

### minify与uglify

巨大的包含注释、空行、长变量与函数名的Js文件，传输的效率低，编译的速度也较慢。所以，前端往往会对Js做minify。

静态代码检查
-----------

自动化测试
-----------

代码调试
-----------

编辑器
----------
