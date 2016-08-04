C++编码规范
=========================

目的
--------

本规范尝试：

* 博采众家C++编程规范之长，将其中对一个团队的编程水平和质量影响最大的规范，形成一个适度的最小集，并以文字重新表述出来
* 将一些未被采选但依然有益的规则（主要是仅针对不常用的语言特性或库的），以综述类规则的形式索引起来备用

意在确定一个具备如下优势的C++编程规范：

* 学习成本低（太多的规则给人过大的心智负担，以至于直接泡汤）
* 易于达成共识（不会浪费太多时间在争论上，最多弃用其中几条，或采取规则的变种）
* 便于施行（很容易在代码检视制度中贯彻，或采用静态代码检查工具扫描、强制）

但本编程规范是带有强烈个人理念的、有态度的编程规范，无意适应、取悦众口。

对于仅关心具体编程规范的读者，可以直接跳过“体例结构”、“理念”两节，从“代码格式化”开始阅读。

体例结构
--------

编程规范的编辑体例，应该像GoF在著名的《设计模式》中一样，构筑自洽完备的表述结构，就像代码的组织要遵循良好的结构一样。

《Google C++ Style Guide》的“Definition/Pros/Cons/Decision”的体例，更像一份讨论权衡而不是规范，留下了很多模糊的空间，没有保持逻辑的自洽，更像一个折衷产物和半成品，且部分条款具备误导性（参考[《Why Google Style Guide for C++ is a deal-breaker》](https://www.linkedin.com/pulse/20140503193653-3046051-why-google-style-guide-for-c-is-a-deal-breaker)）。《LLVM Coding Standards》的体例更像散文。《C++ Coding Standards》的“Item title/Summary/Examples/Exceptions/References”不够完备。《高质量C++/C 编程指南》采用的规则条文式体例，每条规则都干净专注，有其可取之处，但本规范希望将相关的一系列规则融合成一体。

本编程规范的编辑体例与组织结构，基本与C++之父Bjarne Stroustrup最新尚在Github上协作撰写的、面向C++14的《C++ Core Guidelines》(下文简称`CCG`)一致，从其他编程规范中，也吸取了部分元素。

编程规范由许多条编程规则组成，相关联的规则，组成小节。

每个规则具备如下要素：

* __编号__（Reference Number）：为了方便在代码review中引用。采用字母缩写与数字序号相结合的方式。同一小节内的字母缩写一致，与该节内容的单词所含的字母相关。优先采用单个大写字母或多个单词的手字母大写缩写，其次考虑一个单词内首字母大写、再取1-2个字母小写的缩写。
* __规则标题__（Rule）：言简意赅的规则描述，采用格言文风。其表述只需让初读者能猜出大致与什么相关，以及让已阅者朗朗上口，易于回想其含义即可。
* __阐述__（Elaboration）：对规则的含义进行一定的展开描述。一般是先阐述规则的内涵，再通过列表的形式来展开规则的外延。
* __例外__（Exceptions）：规则往往有例外，如果不将有限的、可以接受的例外纳入规则，规则就会被无穷的、不可接受的例外所吞噬。
* __因由__(Reasons/Rationales)：解释这样做的好处、不这样做的坏处，以及该规则为什么可以入选这个“最小集”。自由桀骜的程序员不会盲从权威的规定，但愿意遵循合理的游戏规则。
* __样例__(Examples)：包括正面和反面的例子，可以适度包括对其后果的演示。抽象的描述是难以让人理解领会的。样例能够帮助人在提炼共性中加深理解。
* __备选方案__（Alternatives）：适用于禁令式的规则，给人一条出路。被禁止的做法原来想解决的问题，总得有个被鼓励/允许的做法来解决。演示怎样做才能规避被禁止的做法的弊端，而不引入新的弊端。
* __贯彻手段__(Enforcement)：如何机械式地在代码自查/检视中发现这些问题？如何使用自动化工具（如静态代码检查工具扫描、强制）？
* __参考__（See alsos）：参考相关的规则，或者先贤及其他编程规范，对该规则的阐述与讨论，包括一些反对该规则的理由。

每个规则可复制如下这段模板后开始撰写，可适当移除不必要的要素（如例外、备选方案及之后的要素）：

```
### X.1 规则标题

...阐述直接开始...

__例外__

__因由__

__样例__

__备选方案__

__贯彻手段__

__参考__
```

用语规范:

> TODO 待规范用语后在此说明，如：

* 术语：
  - 函数/方法
  - 定义文件/源文件
  - 团队/组织
* 列表中句号的使用

理念（Philosophy）
-----------------

### P.0 以人为本

让计算机的特性为人服务。

计算机的特性，包括硬件（如CPU二级缓存）、语言（如lambda）、库（如boost）的特性，包括正面的便利之处与反面的约束之处。

人包括：

* 需求方：尽一切可能，不让实现或者计算机特性，影响到目标的达成，寻求合理的方案利用正面的特性，克制反面的约束，除非需求自身不合理，或影响软件的长期维护。
* 开发人员：代码应具备较强的可读性，便于后来者理解（进行白盒分析）和维护（进行调整修改）。
* 测试人员：代码应通过合理的模块化组织，提升可测试性（黑盒测试）。即使不采取TDD（测试驱动开发），也应持续添加自动化测试用例，至少在发现bug时。
* 运维人员（后台系统）/技术支持人员（软件）：代码的运行，应留有便于调整（如修改参数）和观察（如定位问题）的口子，就像汽车应该提供仪表的观察界面和驾驶的操作界面。

__因由__

我们是人类。

__样例__

反面：

* 不熟悉语言特性与潜在的坑，能解决问题的特性不用，能绕过的坑掉进去
* 使用生僻的语言特性，代码让人难以一目了然
* 变量命名晦涩随意
* 因为语法没有规定格式，就不重视代码的格式，只要程序能跑就好
* 意大利面条般、几百万光年外都耦合的代码
* 写死的参数
* 对规范（比如本规范）厌烦，从而不能充分利用编码规范提升自己的生产力、效率和水平。

### P.1 演进的自洽

自洽，即自己与自己不矛盾，有清晰一致的内在逻辑性（如因果性、规则优先序列等），没有潜规则、陷阱和过多的例外。

演进，即演化、进步，是一种动态的视野与格局。演进的自洽不是僵化的千篇一律的硬性规定。它因其清晰一致的内在逻辑性，具备不断演化、完善补充的生命力，同时，也具备对新规则和新理念的包容性，只要具备协调一致的可能。

虽然两者对立统一，但演进以自洽为前提，有其内在优先序列。

这个理念在团队管理场景的另外一个表述，是“遵循并构建规约”，展开如下：

* 团队的规范与原则，入乡随俗，予以遵循并主动尝试理解和了解其中的合理性
* 非必要不制造规范和原则的例外，而不是小事都可以含糊随意，造成破窗效应，千里之堤毁于蚁穴
* 充分理解现状和存在的先例后，为空白区域构建新的规范，或对现有规范加以归纳或优化
* 对团队讨论形成的最终结论，予以执行而不会阳奉阴违

__因由__

* 理性的人做事情，总有一定道理可循。讲一次道理容易，讲一辈子道理不容易，关键在于道理之间的自洽。
* 自洽的规则，易于建立直观理解，符合直觉，能降低理解和遵循规则的心智负担。
* 支离破碎、东一榔头西一棒子的规则，不成体系，只是半成品，容易在自身冲突抵消和例外侵蚀下分崩离析，难以建立长久权威。
* 公说公有理婆说婆有理，谁应被采纳？更能纳入现有自洽的规则体系内的，应被采纳。破而后立，不应是常态。

__样例__

* D.1 保持一致性
* D.2 保持向下兼容性

### P.2 品质的生活

生活，即接地气，不脱离实践、架空、学院派，不陷入盲目的理想主义，不会教条主义地适用某个牛逼的原则。

品质，即品味与质量，能够拒绝坏味道，能够追求更优的质量，更高的标准，更佳的实践。

应持续追求，以合理的代价，在合适的时机，将合用的业界良好实践，运用到实际的编程活动中，贴合实际又不乏预见性地解决编程活动中的痛点。

虽然两者对立统一，但品质以生活为前提，有其内在优先序列。

__因由__

* 许多编程从业者的编码时间不长，但却从各种书籍里，或者网上的博客里，接受过许多金科玉律般的编程理念，在实际编码中，容易纠结于这些规则（很多规则之间，应用起来，容易相互冲突，但没有明确的优先级序列来指引），或者追求牛逼或优雅，脱离了实际的需求，所以需要优先强调接地气。
* 编程活动往往承受巨大进度压力（上线前或出问题后），容易在实践中渐渐远离那些初识编程时学到的原则，放弃趋近，甚至逐渐认为那些都中看不中用，直至深深陷入救火的恶性循环，代码质量和团队水平一路堕落。

__样例__

不接地气的坏例子：

* 早熟的性能优化
* 追求适应一切需求弹性、每次极小改动的通用化万能设计

不追求品质的坏例子：

* 单词拼写错误
* 代码粘贴拷贝
* 手工重复测试
* 忽视代码规范

品质的良好例子：

遵循UNIX编程哲学，详见[《The Art of Unix Programming》之《Basics of the Unix Philosophy》](http://homepage.cs.uri.edu/~thenry/resources/unix_art/ch01s06.html)

> * Rule of Modularity: Write simple parts connected by clean interfaces.
> * Rule of Clarity: Clarity is better than cleverness.
> * Rule of Composition: Design programs to be connected to other programs.
> * Rule of Separation: Separate policy from mechanism; separate interfaces from engines.
> * Rule of Simplicity: Design for simplicity; add complexity only where you must.
> * Rule of Parsimony: Write a big program only when it is clear by demonstration that nothing else will do.
> * Rule of Transparency: Design for visibility to make inspection and debugging easier.
> * Rule of Robustness: Robustness is the child of transparency and simplicity.
> * Rule of Representation: Fold knowledge into data so program logic can be stupid and robust.
> * Rule of Least Surprise: In interface design, always do the least surprising thing.
> * Rule of Silence: When a program has nothing surprising to say, it should say nothing.
> * Rule of Repair: When you must fail, fail noisily and as soon as possible.
> * Rule of Economy: Programmer time is expensive; conserve it in preference to machine time.
> * Rule of Generation: Avoid hand-hacking; write programs to write programs when you can.
> * Rule of Optimization: Prototype before polishing. Get it working before you optimize it.
> * Rule of Diversity: Distrust all claims for “one true way”.
> * Rule of Extensibility: Design for the future, because it will be here sooner than you think.

遵循面向对象的[S.O.L.I.D原则][solid]：

> * S – Single-responsiblity principle
> * O – Open-closed principle
> * L – Liskov substitution principle
> * I – Interface segregation principle
> * D – Dependency Inversion Principle

[solid]: https://en.wikipedia.org/wiki/SOLID_(object-oriented_design)

设计原则（Design Principles）
---------------------------------

### D.1 保持一致性

* 技术选型、代码风格等与所属模块保持一致性，团队内模块间保持一致性。
* 尽可能使用和改善团队内公共的基础设施，而不是自写一套个性化的实现，除非有一系列与基础设施在定位、侧重点、方案取舍等方面存在难以融入却又有必要的差异、定制。
* 不要违背这两点给他人制造惊喜与陷阱。

### D.2 保持向下兼容性

* 保持向下兼容性，无故不删除函数或修改同名函数语义。
* 要进行破坏兼容性修改时，应经过团队讨论，并只在Major版本时进行，不在Minor和Patch版本进行，参考[《Semantic Versioning》](http://semver.org/)。
* 应维护模块的changelog，包括新增的功能与修复的bug，参考[《Keep a CHANGELOG》](http://keepachangelog.com/)。底线是，在版本合入主干时，以写changelog的认真态度，写commit message，参考[《How to Write a Git Commit Message》](http://chris.beams.io/posts/git-commit/)和[《The bluejava Git Commit Message Format Guide》](https://github.com/bluejava/git-commit-guide)。更好的建议是用单独的文件来维护changelog。

### D.3 有损服务，柔性可用

* 通过接受并运营牺牲，来提升系统整体支撑能力，确保关键功能的可用性。
* 在设计阶段就考虑，当系统不能提供完美体验时，设定多级主动体验降级，来为用户提供更好的体验过渡。

### D.4 API设计意识与SLA服务意识

* 把每一个模块、子模块、类、函数，都当做API来精心雕琢设计，为使用者提供便利。
* 把每一个子功能，都当做一项服务、一个产品，给出相应的服务标准（SLA）的承诺，并以之为目标导向。

__API__

* Application：应用、场景、需求、用例
* Programmable：组合、套餐、解决方案、时序图
* Interface：
  - 提供风格一致的界面
  - 入参/出参
  - 初始化/前置条件/后置条件
  - 行为
  - 避免副作用，或明确以文档/注释说明
  - 异常结果的表达方式（返回值与异常）
  - 授权/来源/分层/作用域

__SLA__

* Service：
  - 特性（定性）
    + 功能
    + 安全
    + 可用性（例如：120分钟；99.97%）
    + 性能
      - 吞吐量
      - 时耗
      - 总量
      - 超频弹性
    + 需求响应速度
      - 设计灵活性
      - 研发效率
      - 资源调度能力
    + 成本
  - 对象（QoS）
* Level：
  - 量化
  - 分级
  - 交付时间
* Agreement：
  - 正常：
    + 用户端：Expectation
    + 服务端：Goal
  - 一次灾难或部分二次灾难
    + 用户端：Acceptance
    + 服务端：Bottom Line

__参考__

* [How to Design a Good API and Why it Matters](http://static.googleusercontent.com/media/research.google.com/zh-CN//pubs/archive/32713.pdf)
* [The Little Manual of API Design](http://people.mpi-inf.mpg.de/~jblanche/api-design.pdf)
* [Principles of API Design by Charlie Garrod & Jonathan Aldrich](https://www.cs.cmu.edu/~charlie/courses/15-214/2014-fall/slides/17-api-design.pdf)
* [API Design Principles in Qt](https://wiki.qt.io/API_Design_Principles)

### D.5 测试驱动开发

* 自测通过自动化单元测试进行，而非手工架设依赖的环境并手工触发测试。
* 通过构造测试用例，来思考代码的异常路径的逻辑。
* 考虑代码的可测试性，内部代码也组织成小小的API。
* 新增的工具函数和关键业务逻辑必须编写配套的自动化测试用例（如Java使用JUnit，C++使用Google Test），前者相当于单元测试，后者相当于粗略的集成测试。
* 应持续提升自动化测试覆盖率。
* 对依赖的外部系统，必须写能够模拟正常和异常返回的桩，利于联调测试开展。
* 可以通过[夹具(Fixture)](https://en.wikipedia.org/wiki/Test_fixture#Software)来提升编写自动化测试用例的效率。

### D.6 代码中的文档

* 写文档（详见F.1）
* 读文档
  * 对业已存在的文档尽可能进行阅读理解与自我重新组织。
  * 避免出现“不写文档我不知道、写了文档我还是不知道”的局面。
* 不依赖文档
  * 在没有文档或文档不准确的时候，通过阅读自动化测试用例以及分析理解代码意图来把握代码。
  * 不要想当然地以文档、函数的命名或者自己的预期来判断代码的实际功能、内部逻辑与副作用。
  * 避免出现“别人写的代码太晦涩了，我怎么想得到这里会有这么一个弯弯绕绕，出了错也不是我的责任”这类思想上的借口。
* 建立文档索引：
  * 文档越积越多，势必变得难以找到需要的文档；文档也逐步失去时效性，充满了过时的信息；恶化到极致，对同样的系统，有不同时期写成的多个版本的文档，每个互不了解，各自提供部分信息和部分误导。
  * 因此，应为文档建立索引（而不仅是提供搜索），从而让同一领域的文档，有固定的集散地，相互呼应、关联，共同提供有效和最新的信息。

代码格式化（Format）
------------------

### F.0 克制重新格式化代码的冲动

对已有的代码基，尽量模仿其格式和风格（哪怕只是一个文件、一个类、一个函数的局部风格），而不要轻易因为个人喜好，重新格式化。

__因由__

* 在没有取得对于格式的统一认识之前，每个人都因个人喜好重新格式化代码，会形成代码格式的冲突和混乱。
* 随意地重新格式化代码，会使得代码review时无法清晰看出有意义的修改点，从而导致隐藏bug。

__例外__

有两种情况下进行代码的重新格式化，是可以的：

* 本来就要对某个局部进行大的逻辑重构，而该局部目前的代码风格不符合团队成文的格式规范。
* 团队内取得共识，有计划地基于成文的格式规范，集中进行代码的重新格式化。

但注意：

* 格式化的修改需要单独commit，并在commit log中加以说明。
* 严禁在一次commit中混杂有意义的修改和纯粹的格式化。
* 不宜在一系列的commit中穿插有意义的修改和纯粹的格式化，而应分别集中做有意义的修改，和纯粹的格式化。

__贯彻手段__

团队应在代码基形成初期就对代码格式达成统一共识，集中将代码格式统一，并提供统一工具在每次commit前自动格式化，确保后继无需再为格式操心。

一个优秀的代码格式化工具是[Astyle](http://astyle.sourceforge.net/astyle.html)，本规范的格式可以用如下选项运行Astyle：

```
astyle --recursive --indent=spaces=2 *.cpp *.h
```

根据在F.5中对花括号换行风格的选择，还可以增加不同的`--style`选项，详见F.5。

还可以增加如下选项使之符合本规范风格：

* `--indent-col1-comments` / `-Y`：将注释和下一行代码对齐。
* `--break-blocks` / `-f`： 'if', 'for', 'while'等代码块前后增加1个空行。
* `--pad-oper` / `-p`：二元操作符两侧增加1个空格。
* `--unpad-paren` / `-U`：确保括号两侧无空格。
* `--align-pointer=type --align-reference=type` / `-k1 -W1`：确保`*`和`&`靠近类型而不是变量名。
* `--add-brackets` / `-j`：确保'if', 'for', 'while'等代码块如果只有1条语句，依然加花括号。
* `--remove-comment-prefix` / `-xp`：去掉多行注释每行的`*`前缀。
* `--max-code-length=80 --break-after-logical` / `-xC80 -xL`：在逻辑操作符、逗号、括号、分号或空格处断开，使得每行长度小于80。

其他值得注意的地方：

* 实际格式化之前，请使用`--dry-run`测试效果。
* 对于外部代码或其他不希望格式化的代码，请使用`--exclude=`选项排除
* 可以考虑将部分规则自动化，每次commit均执行，而另外部分规则，工具自动提示修改的地方，但人工确认才生效。

__参考__

[LLVM Coding Standards：Introduction](http://llvm.org/docs/CodingStandards.html#introduction):

> ...we explicitly do not want patches that do large-scale reformating of existing code. On the other hand, it is reasonable to rename the methods of a class if you’re about to change it in some other way. Just do the reformating as a separate commit from the functionality change.

TODO: 待补充或替换为：[ClangFormat](http://clang.llvm.org/docs/ClangFormatStyleOptions.html)

TODO：`.astylerc`的使用

```
# 缩进：2个空格，禁用Tab
--indent=spaces=2
# 将注释和下一行代码对齐。
--indent-col1-comments 
# 'if', 'for', 'while'等代码块前后增加1个空行。
--break-blocks
# 二元操作符两侧增加1个空格。
--pad-oper
# 确保括号两侧无空格。
--unpad-paren
# 确保`*`和`&`靠近类型而不是变量名。
--align-pointer=type --align-reference=type
# 确保'if', 'for', 'while'等代码块如果只有1条语句，依然加花括号。
--add-brackets
# 去掉多行注释每行的`*`前缀。
--remove-comment-prefix
# 在逻辑操作符、逗号、括号、分号或空格处断开，使得每行长度小于80。
--max-code-length=80 --break-after-logical
```

### F.1 注释

注释贵精不贵多：

* 注释传递意图(why)，而非做法（how）。
* 写具备可读性的代码来减少（易于因实现变化而过时的）注释。
* 修改代码时，应注意检查相关注释是否依然有效，并进行更新或删减。
* 内外部API，如公有函数、公有成员等，必须采用Doxygen格式注释。
* 会被多处使用的实体，如类的静态变量、成员变量、常量、枚举等，必须采用Doxygen格式注释。
* 不应养成将许多历史代码长期以注释的形态保留的坏习惯。

注释格式应清晰可读：

* 类、函数、成员变量等实体，采用Doxygen格式的`/** ... */`，在实体上方；对于部分字段短小的结构的成员变量等实体，允许在成员同一行的后半段用`//`注释说明，Doxygen也支持这种格式。
* 函数体内的一行或一小段代码，采用`//`，在代码段上方，反对在代码段后端的写法。
* 注释应适度进行局部的对齐。

__例外__

* 可以在长段代码块（如类定义、namespace等，特别是有多重嵌套的）尾部的花括号右侧，以`//`注释说明对应的名称。
* 在演示性的代码中，允许在行末使用`//`说明其结果与优劣，本规范的样例，使用该格式。

__因由__

* 过少的注释，他人难以维护，自己几个月后也容易忘记了代码意图与考虑。
* 过多的注释，难以跟随实现变化，也难以坚持。
* 格式混乱的注释，影响阅读观感和代码品质。
* 按照Doxygen格式写的注释，自身整齐清晰，也能自动生成文档，反向促进注释的维护。

__样例__


```cpp

  /**
    关于该类的一行简短描述。
    
    关于该类的详细描述，如：职责。
   */
  class ClassName
  {
  public:

    /**
      关于该函数的一行简短描述。
      
      关于该函数的详细描述，如：参数、行为和异常处理。

      详见D.4

      @param foo 参数foo的意义、用法、取值限制等。

      @returns 返回值的说明。如果显然，可忽略。 
     */
    virtual int anExampleMethod(int foo);

    /**
      对于废弃的成员函数，应标注:

      @deprecated

      并说明原因和替代它的函数。

      @see anExampleMethod

      可以适当标注一些TODO事项，按如下格式：

      TODO 在下一个major版本移除该方法。
    */
    void aDeprecatedMethod();

  private:
    /**
      关于该成员变量的描述
    */
    int m_anExampleDataMember;

  }; // class ClassName

```

__参考__

[Documenting NuPIC with Doxygen: Documenting conventions](https://github.com/numenta/nupic/wiki/Documenting-NuPIC-with-Doxygen#documenting-conventions)

### F.2 `#include`

`#include`天生有序：

* `#include`应出现在代码的最前面，中间不应穿插其他代码，除了条件编译和头文件保护符。
* `#include`应遵循如下顺序：
  - 本定义文件对应的头文件（仅适用于定义文件，详见SF.2）
  - C标准库的头文件
  - C++标准库的头文件
  - 操作系统/第三方库的头文件
  - 本项目的头文件

最小依赖原则：

* `#include`应只包含最小必要的头文件。
* `.h`文件中，能用前向声明解决的，就不要`#include`。

正确区分使用`<>`和`""`：

* 用 `#include <filename.h>` 格式来引用标准库的头文件（编译器将从标准库目录开始搜索）。
* 用`#include "filename.h"` 格式来引用非标准库的头文件（编译器将从用户的工作目录开始搜索）。

__因由__

* 包含顺序：消除隐性依赖
* 最小依赖：提升编译速度
* `<>`和`""`：消除隐性路径问题

__样例__

`foo/server/fooserver.cpp`:

```cpp
#include "foo/server/fooserver.h"

#include <sys/types.h>
#include <unistd.h>

#include <hash_map>
#include <vector>

#include "base/basictypes.h"
#include "base/commandlineflags.h"
#include "foo/server/bar.h"
```

__参考__

* [Google C++ Style Guide: Names and Order of Includes](http://google.github.io/styleguide/cppguide.html#Names_and_Order_of_Includes)
* [LLVM Coding Standards: `#include` Style](http://llvm.org/docs/CodingStandards.html#include-style)

### F.3 行宽

80字节。

__因由__

* 这是最广泛采用的标准，少量增加（如90）并无意义，只会带来不一致。
* 虽然现在宽屏已普及，但需要考虑代码对比时，新老文件左右并列显示时依然能完整显示一行。
* 短的行宽，能够自然抑制3层以上的深度嵌套。

__参考__

### F.4 缩进

* 一致地使用4个或2个空格，禁用Tab。
* 有层次结构的地方，均应正确缩进，但namespace自身不缩进，也不增加其内部结构的缩进。

__因由__

主要是为了代码一致的可读性。

* 禁用Tab，尤其禁用Tab和空格混用，不禁用的后果是各种编辑器一打开，不同人写的代码就乱套了。
* 建议不采用浪费空间的4、8个空格，而是紧凑的2个空格，控制行宽。本规范其他地方的样例均采用2个空格。
* 缩进可以清晰传递层次结构信息，错误的缩进会误导、扭曲语义。
* namespace如果缩进，会带来过深的缩进，所以规避之。

__样例__

```cpp
namespace outer {

namespace inner {           // 良好的风格：namespace自身不缩进


void foo() {                // 良好的风格：不因在namespace内而增加缩进深度
  ...
}

}  // namespace inner       // 良好的风格：在尾部的花括号注释说明对应的namespace

}  // namespace outer
```

__参考__

关于namespace：

* [Google C++ Style Guide: Namespace Formatting](http://google.github.io/styleguide/cppguide.html#Namespace_Formatting)
* [LLVM Coding Standards: Namespace Indentation](http://llvm.org/docs/CodingStandards.html#namespace-indentation)

### F.5 括号

花括号：

* 严禁无花括号的`if`/`else`/`for`/`while`等语句。
* 一致地使用花括号换行风格，详见贯彻手段。建议选择其中的“K & R”风格或“break”风格。

圆括号：

* 当操作符优先级可能有疑问时（尤其是算术类、逻辑类、其他类等2种以上操作符混合时），使用圆括号明确表达优先级。

__样例__

```cpp
if(a & flag != 0) {}   // 不良的例子：未正确理解优先级，代码运行不符合预期

if((a & flag) != 0) {} // 良好的例子：通过括号确立优先级，确保正确性和可读性

if(aCondition) doSomething(); // 不良的例子：无花括号的`if`语句

/*
  良好的例子：只有一行，也要有花括号。
*/
if(aCondition)
{
  doSomething();
}
```

__贯彻手段__

通过Astyle（见F.0）的`--style`选项。几个主要的选择：

`--style=attach`：下称“attach”风格。

开花括号向前跟随，闭花括号后若有类似`else`等语句则向后跟随，均间隔1个空格。Java社区主流。例如：

```cpp
int Foo(bool isBar) {
  if(isBar) {
    bar();
    return 1;
  } else {
    return 0;
  }
}
```

`--style=break`：下称“break”风格。

开、闭花括号都在单独一行。C++社区较常见。例如：

```cpp
int Foo(bool isBar)
{
  if(isBar)
  {
    bar();
    return 1;
  }
  else
  {
    return 0;
  }
}
```

`--style=kr`：即“K & R”风格，R代表的Dennis Ritchie是C之父，K & R是《The C Programming Language》的作者。

仅对于命名空间、类和函数定义，采用“break”风格；其他均采用“attach”风格。

`--style=stroustrup`：即“Stroustrup”风格，他是C++之父，也是《The C++ Programming Language》的作者。

仅对于函数定义，采用“break”风格；其他均采用“attach”风格。

__参考__

圆括号：

* [CCG ES.41: If in doubt about operator precedence, parenthesize](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#es41-if-in-doubt-about-operator-precedence-parenthesize)

### F.6 换行与空行

合理使用换行：

* 一行代码只做一件事情，如只定义一个变量，或只写一条语句，或开启一个逻辑块（如函数头、for、if等），便于阅读代码和添加注释。
* 长表达式要在低优先级操作符处拆分成新行，操作符放在新行之首（以便突出操作符），并进行适当的缩进，使排版整齐，语句可读。

一致而适度地使用空行，划分逻辑单元，增强代码可读性：

* 每个独立实体（类、函数）前后应有空行。
* 函数体内逻辑关系密切的代码间不加空行，逻辑段落间应有空行。

__样例__

良好的风格：

```cpp
int aVariable = 123;

if(aCondition)
{
  doSomethingWith(aVariable);
}

// 打开冰箱门
openFridge();
// 塞进大象
putInElephant();
// 关闭冰箱门
closeFridge();

if((veryLongVariable1 >= veryLongVariable2)
  && (veryLongVariable3 >= veryLongVariable4)
  && (veryLongVariable5 >= veryLongVariable6))
{
  doSomething();
}
```

__参考__

*《高质量C++/C 编程指南》规则2-2-1、2-2-2、2-5-2

### F.7 空格

一致而适度地使用空格，突出关键内容，增强代码可读性。详见样例。

__因由__

* 如果在满足语法的基础上不加任何空格，代码就显得过于密集，难以清晰抓住关键点。
* 如果过度使用空格，就像一段话全都加重黑体，反而更加没有重点。
* 如果空格使用在一个文件内甚至一行内不一致，充满随意性，缺乏对称性，阅读起来就十分别扭，其代码传递的是一种不修边幅、得过且过的气息，不能传递认真谨慎、精益求精的精神。

__样例__

函数、关键字与参数：

```cpp
/**
  良好的风格：

  * 函数名后不留空格。
  * 左右括号的内外都紧挨，不留空格。
  * 逗号、分行向前紧跟，不留空格。
  * 逗号后留空格。

  因由：

  * 函数名的定义和调用，都应在视觉上将函数名和被操作的参数关联起来，空格则在括号的基础上进一步制造了距离，所以函数名后、左右括号的内外都不应添加空格。
  * 逗号视觉上不应该突出，所以不能是两边空格；没有空格过于紧凑不利于阅读；逗号靠近后面参数会造成后一个参数以逗号开头的奇怪感觉；综上所以逗号更适宜跟随前面一个参数。
*/
void fun(int x, int y, int z); // 良好的风格

/**
  不良的风格：

  * 函数名后有多余的空格，造成函数名和参数之间的割裂感。
  * 第一个逗号后无空格，没有合理分隔不同的参数。
  * 第二个逗号前有多余空格，两边的距离把（不重要的）逗号突出出来了。
*/
void fun (int x,int y , int z);

/**
  良好的风格：

  * for、if等关键词后面不建议加空格（存在争议，有人推荐加）
  * 如果‘;’不是一行的结束符号，其后要留空格，遵循和函数参数里逗号类似的规则。
*/
for(i = 0; i < 10; ++i)

/**
  不良的风格：太少的空格
*/
for(i=0;i<10;i++)
```

操作符相关：

```cpp
if(year >= 2000)       // 良好的风格：二元操作符前后加空格
if((a>=b) && (c<=d))   // 良好的风格：突出关键的二元操作符，保持整体紧凑
if(year>=2000)          // 不良的风格：二元操作符前后无空格，过于密集
if(year >=2000)         // 糟糕的风格：没有一致地使用空格，非常别扭

int *x = &y;            // 良好的风格：一元操作符前后不应加空格
int * x = & y;          // 不良的风格：空格割裂了一元操作符和被操作对象之间的关系

/**
  象“［］”、“.”、“->”这类操作符前后不加空格。
*/
array[5] = 0; // 不要写成 array [ 5 ] = 0;
a.method(); // 不要写成 a . method();
b->method(); // 不要写成 b -> method();
```

注释：

```cpp
// 注释      // 良好的风格： `//`后空一格
int x = 1;

//注释       // 不良的风格： `//`后紧跟注释
int x = 1;

/**
  注释       // 良好的风格： `/**`当行无注释，下一行缩进一级（即2个空格）后，开始注释。
*/
```

__参考__

[CCG NL.15: Use spaces sparingly](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#nl15-use-spaces-sparingly)

讨论：是否应该用空格来突出`if`等条件判断场景下的`!`？因为`!`非常贴近后面表达式，非常容易看漏。

结论：可以考虑，但可能更应该通过语法高亮和字体等方式来做。

源文件代码组织(Source File)
--------------------------

### SF.1 头文件（Header File）

头文件是脸面。

* 头文件的内容，包括自身的`#include`，应被头文件保护符(header guard, i.e. `ifndef/define/define`）所环绕，防止文件被重复包含，具体格式见样例。
* 头文件应更少暴露实现细节，如使用[“Pimpl”惯用法](https://en.wikipedia.org/wiki/Opaque_pointer)或[前向声明（Forward Declaration）](https://zh.wikipedia.org/zh-cn/前向声明)。
* 头文件应视为对外API或对内API，使用Doxygen格式进行注释，自动生成文档，详见F.1一节。
* 一致地使用`.h`或`.hpp`作为C++头文件的文件名后缀。建议使用最普遍的`.h`。
* 注意考虑F.2中提到的关于`#include`的规则。

__样例__

头文件保护符，遵循`项目或总命名空间_关键路径_文件名_H_`的格式（注意最后一个下划线），并在`#endif`处用注释这是保护符的结尾：

```cpp
#ifndef FOO_BAR_BAZ_H_
#define FOO_BAR_BAZ_H_

...

#endif  // FOO_BAR_BAZ_H_
```

__参考__

* [Google C++ Style Guide: The `#define` Guard](http://google.github.io/styleguide/cppguide.html#The__define_Guard)
* [CCG: Source files](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#sf-source-files)

### SF.2 定义文件（Definition File）

定义文件是实现。

* 定义文件应包含定义其接口的头文件，参见[CCG SF.5](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#sf5-a-cpp-file-must-include-the-h-files-that-defines-its-interface)
* 将大多数不必要的`#include`和声明都移到定义文件里来。
* 反对使用文件作用域变量，其初始化时机存在不确定性，带来潜在顺序依赖。（TODO 找到依据文献）
* 一致地使用`.cpp`、`.cxx`或`.cc`作为C++定义文件的文件名后缀。建议使用最普遍的`.cpp`。
* 注意考虑F.2中提到的关于`#include`的规则。
* 如果一个软件的头文件数目比较多（如超过十个），通常应将头文件和定义文件分别保存于不同的目录，以便于维护。头文件一般放在`include`目录，定义文件一般放在`src`或`source`目录。

__参考__

[CCG: Source files](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#sf-source-files)

命名(Naming)
-----------------

### N.0 命名一般规范

命名应“名正言顺”，对被命名者，进行易于理解、结构规范、具体而简洁地描述：

* 易于理解：
  - 命名应使用英文名，不应使用汉语拼音。
  - 对于常见的业务对象命名，应该在设计阶段就给出一致的英文名，避免在代码的不同地方以近义词进行不一致的命名。
  - 命名不允许出现单词拼写错误和语义错误，可以查词典，也可以在[codelf](http://unbug.github.io/codelf/)搜索建议的命名，以及参考开源代码中实际使用的命名。
* 结构规范：
  - 类型、变量、常量等实体性质的命名，一般可采用“形容词/限定词-名词”的结构。
  - 函数等动作性质的命名，一般可采取“动词-名词”的结构。
  - 布尔类型的变量或函数，应使用“is/has/can/should”等前缀，与其后的“形容词/名词/动词”搭配，表达其语义，反对以`b`作为前缀。
* 具体：
  - 命名中应避免模糊抽象随意，如`data2`、`object`。
  - 替代旧函数`fun()`的新函数命名，不应将新函数命名为`funNew()`或`newFun()`，而应将老的命名为`deprecatedFun()`，新函数直接用`fun()`；如果不方便做到，那么新函数命名应充分表达出与老函数的语义区别。
* 简洁：
  - 命名应使用1-5个英文单词，每个单词宜小于12个字母。
  - 但不要为了简短，就任意缩减字母或采用并非众所周知的缩写。
* 无歧义：
  - 命名应不易产生误会、歧义或者具备和实际含义相左的字面意思。
  - 反对为了命名的不重复，而选择区别不明显的近义词表达有本质区别的两样事物，如：用`fail`和`error`表达业务错误和系统错误的区别，容易导致使用者记混用错。
  - 反对用一个名字表达多重意思，如：一个类里多个成员函数都用`add`前缀，表示的却是统计累加、队列添加、记录添加等不同意思。
  - 反对用多个名字表达一个意思，如在代码里不同地方，以不同的名字称呼同一个事物，从而导致阅读者和使用者揣测其微妙区别。

__例外__

作用域非常小，而含义因周围代码而较明显时，可使用短小命名。

参见[CCG ES.7: Keep common and local names short, and keep uncommon and nonlocal names longer](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#es7-keep-common-and-local-names-short-and-keep-uncommon-and-nonlocal-names-longer)

__样例__

```cpp
/*
  良好的风格
*/
class Animal;

Animal goodDog;

goodDog.run();
goodDog.eatFood();

bool isRunning = goodDog.isRunning();

/*
  不良的风格：使用拼音
*/
class dongWu;

/*
  不良的风格：冗长
*/
int aVeryLongAndComplicatedAndRedundantName;

/*
  不良的风格：拼写错误
*/
Field filed;

/*
  良好的风格，简炼，更易读：

  * 模板类型使用惯见的`T`
  * 循环使用惯见的`i`、`j`、`k`等
*/
template<typename T>    // good
void print(ostream& os, const vector<T>& v)
{
  for(int i = 0; i < v.end(); ++i)
  {
    os << v[i] << '\n';
  }
}

/*
  不良的风格，简单的代码变得冗长：
*/
template<typename ElementType>
void print(ostream& target_stream, const vector<ElementType>& current_vector)
{
    for (int current_element_index = 0;
            current_element_index < current_vector.end();
            ++current_element_index
    )
    target_stream << current_vector[i] << '\n';
}
```

__参考__

[Wikipedia的Naming Convention(programming)条目][wiki_naming_convention]

[wiki_naming_convention]: https://en.wikipedia.org/wiki/Naming_convention_(programming)

### N.1 文件名

* 一个模块内，如果文件比较多，可以通过目录对文件进行分组，建议1层，不要超过2层。
* 目录名全部小写，单词间用下划线`_`相连，一般最好只有1个单词。
* 文件名全部小写，单词间用下划线`_`相连。
* 文件名应具体而非抽象，可以通过文件名了解该文件的功能，但不宜太长。

__样例__

目录名：

* 良好：`base`、`admin`
* 不良：`Base`、`some_module`

文件名：

* 良好：`http_server_logs.h`
* 不良：`logs.h`、`a_handsome_smart_and_great_guy.h`

### N.2 命名空间

* 原则上，一个组织/一个子系统应该有自己的顶级命名空间，下设以模块、功能的次级命名空间，建议1层，不要超过2层。
* 命名空间的命名规则同文件。
* 如果使用了目录划分文件，同一目录下的文件的命名空间应一致，要么直接用顶级命名空间，要么次级命名空间和目录名一致。

### N.3 类型命名

* 类型命名应采用`CamelCase`的格式，即每个单词首字母大写，其他字母小写，单词间直接相连。
* 可以考虑使用前缀：
  * 常用的有：类（C，代表class）、结构（S，代表struct）、枚举（E，代表enum）；
  * 不太常用的有：类型定义（T，代表typedef）、联合（U，代表union）。

__样例__

* 良好：`Animal`、`DigitalCamera`、`CWindow`
* 不良：`animal`、`digital_camera`

### N.4 常量命名

* 常量包括`const`定义的常量、枚举值和宏定义的常量。
* 常量命名应采用`UPPER_CASE_UNDERSCORE`的格式，即所有字母大写，单词间用下划线`_`相连。
* 同一组常量（如一个枚举里的多个枚举值），应有统一前缀。

### N.5 变量命名

* 变量命名应采用`camelCase`的格式，即第一个单词首字母小写，其余单词首字母大写，其他字母小写，单词间直接相连。
* 坚定一致地使用作用域前缀：
  - 成员变量，应采用`m_`前缀。
  - 全局变量，应采用`g_`前缀（但应规避全局变量）。
  - 静态变量，应该用`s_`前缀。
  - 具备上述多个作用域的，按照`sgm`的顺序（符合英语语序）进行结合。
* 对于在一定语境内反复出现的业务对象，坚持使用N.0中提到的“形容词/限定词-名词”结构。

存在争议项：

* 指针前缀
  - 指针，应该使用`p`前缀，代表pointer。
  - 函数指针，应该使用`pfn`前缀，代表function pointer。
* 可选、适度使用易理解的、简化版的匈牙利命名法，如：
  - 整数（`n`或`i`，代表integer，宜用前者）
  - C风格字符串（`sz`，代表string ends with zero）
  - C++风格字符串（`str`，代表string）
* 共享内存变量，可采用`gg_`前缀。
* 结构类型的变量，可采用`st`前缀。
* 对于在一定语境内反复出现的基础对象，使用易于领会的类型缩写前缀，比如：可以用`vec/set/map/hash`等前缀，代表`std::vector/std::set/std::map/std::unordered_map`等STL容器类型）。

### N.6 函数命名

* 函数命名应采用如下两种格式之一：
  - `CamelCase`的格式，即每个单词的首字母都__大写__，其他字母小写，单词间直接相连。
  - `camelCase`的格式，即第一个单词首字母__小写__，其余单词首字母大写，其他字母小写，单词间直接相连。
* 对于操作类的函数，坚持使用N.0中提到的“动词-名词”结构。
* 对于存取和访问类的函数，应分别使用`set`和`get`前缀，其中`get`函数应声明为`const`。
* 对于返回布尔值的函数，应使用N.0中提到的“is/has/can/should”前缀。

基础编码（Basics）
-----------------------------

### B.0 防御性编程

一段代码应通过校验确保前置条件满足，才执行主体逻辑，并校验后置条件满足，尤其是对于一个函数、一个类、一个模块而言。

代码的假设与依赖，应该通过上述校验来清晰表达出来，不应隐含地引入，或只在注释中说明。

__因由__

不能因为如下理由而不坚持防御性编程：

1. 该条件（恒等式）显然/必然成立
2. 过早考虑性能因素
3. 觉得此类检查/保障在调用者和被调用者处重复，从而多余

因为：

1. 需求的变化会威胁恒等式的成立，甚至迫使我们寻找新的恒等式；技术的错误（如内存覆盖）可能从意想不到的角度破坏其成立。
2. 在这段代码的性能问题成为问题之前，其可维护性问题、运行异常定位就会先频频成为问题。而且，[类型检查](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#p4-ideally-a-program-should-be-statically-type-safe)、静态断言等[编译期检查机制](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#p5-prefer-compile-time-checking-to-run-time-checking)，可以在不影响运行性能的情况下，防止很多条件的违反。而运行时检查，可以先遵循[CCG P.6](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#p6-what-cannot-be-checked-at-compile-time-should-be-checkable-at-run-time)和[CCG P.7](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#p7-catch-run-time-errors-early)，待真正需要优化性能时，总是有办法可以优化的。
3. 多余的问题可以从3个方面考虑：
  - 被调用者应被视为一个自足的、会自我保护的主体来看待，它可能被很多并不做检查的调用者调用。
  - 调用者通过检查前一步的后置条件来保护被调用者，被调用者通过检查本步骤的前置条件保护自己，这是一种分工。
  - 一个系统真正出现引起损失的问题，往往是多个环节都存在缺失。为了一个系统的健壮和对疏失具备一定的抵抗力，适度的冗余是必要的。

__样例__

```cpp

void handle(const InputType& in)
{
  // 检查输入参数
  checkInput(in);

  // 检查前置条件，state代指某个状态
  checkPreCondition(state);

  // 执行操作
  OutputType out = op(state);

  // 检查后置条件
  checkPostCondition(state);

  return out;
}

```

### B.1 合理使用配置

首先反对将代码运行依赖的一些参数写死在代码里，以至于每次调整都必须修改代码。应有一种配置的意识，始终为各种参数提供修改配置即可调整的可能。

配置应提供方便的调整手段：

* 配置文件应考虑热加载。
* 部分可能经常在运行时调整的参数（如限流阈值），应提供接口方式热修改（如发个管理命令UDP包）。与此同时，应通过合理方案确保程序重启后依然生效。

通过“约定优先于配置”的思路，避免配置的无限膨胀和误配：

* 提供合理的、符合直觉并具备一致性的默认值，减少配置的数量和冗余。
* 通过尽可能少的配置项决定配置参数的组合，避免相互耦合却需要分别配置的配置项。
* 有默认约定不是不要配置，要在默认值的基础上通过配置预留深度定制的空间。
* 配置项名称应包含单位（如秒、毫秒），配置项应有注释。
* 同一项目的配置项命名应采取一致的大小写规则，如`ALL_CAPS`、`under_score`等，不建议采取大小写混用的格式，因为容易记错。

### B.2 严禁使用幻数

幻数（Magic Number）又称魔数，指代码中出现的含义模糊、依据缺失的裸数字。

程序中使用的每一个数字，应该以枚举或常量的方式定义，通过名称清晰地揭示其含义与单位，通过表达式揭示起推算过程，通过注释进一步加以说明，并在局部相对集中的地方定义（比如一个子模块里有一个文件定义子模块公共常量，整个项目也有一个文件定义项目公共常量）。

__因由__

从写代码者的角度：

每到一个需要确定1个数字的场景，都需要决定该数字的取值，从而带来取值的随意性，没有统一的推导依据。

没有依据导致无法应对未考虑到的极端情况，没有一致的依据导致多个幻数间可能只有最短的板生效，其他都带来危险（如溢出）或损失（如截断）。

从读代码者的角度：

难以理解一段代码的意图，更难以确认其正确性和适用范围。

从修改代码者的角度：

需要修改这个幻数的取值时，难以找到和这个幻数同等含义和用场的所有幻数一起修改，极易改漏。

__样例__

不良的风格：

```cpp
// TODO 补充一段从实际代码基中提取出来的较多依据不一的幻数的代码段。
```

良好的风格：

```cpp
/**
  默认HTTP连接超时，单位：毫秒。
*/
const int DEFAULT_HTTP_CONNECT_TIMEOUT_IN_MS = 200;

/**
  每天的秒数。
*/
const long SECONDS_PER_DAY = 24 * 60 * 60;

/**
  每个key-value对中的key的大小限制
*/
const size_t KEY_SIZE = 128;

/**
  每个key-value对中的value的大小限制
*/
const size_t VALUE_SIZE = 512;

/**
  对value做UrlEncode的缓冲区大小

  依据：最坏的情况下，全部需要encode，1个字节变成'%'+2个字节的HEX值，膨胀系数为3
*/
const size_t ENCODED_VALUE_SIZE = VALUE_SIZE * 3;

/**
  一个报文的大小限制：10kB
*/
const size_t PACKET_SIZE = 10 * 1024;

```

### B.3 用RAII管理资源的申请与释放

[RAII（Resource Acquisition Is Initialization）](https://en.wikipedia.org/wiki/Resource_Acquisition_Is_Initialization)是C++防止资源泄漏的一种惯用法（Idiom）。具体而言，RAII使用一个对象，在其构造时获取资源，在对象生命期控制对资源的访问使之始终保持有效，最后在对象析构的时候释放资源。

资源包括内存、锁、文件句柄、Socket句柄、数据库结果等。

严禁在业务代码中出现`new`/`delete`对、`lock`/`unlock`对、`open`/`close`对等，应将其用合适的资源管理类封装在内，使得业务代码无需确保其成对出现。

__因由__

一般而言，一段代码会有多个出口，比如：

* 某个条件满足，提前return
* 某个条件满足，循环提前break
* 某种异常情况，抛出exception
* 等等等等

或者申请多个资源。

无论是1个资源多个出口，还是多个资源1个出口，或是多个资源多个出口，确保每个资源在每个出口处得到正确的释放，十分困难。纵然初次写代码时考虑周全了，也难以在后继修改中确保不遗漏。

对于栈上分配的对象，C++保证无论以何种方式退出当前代码块时，都会调用该对象的析构函数，这就提供了一种减轻编程者心智负担、对代码健壮性加以保障的机制，应对这种机制，加以善用。

__样例__

不良的风格：

```cpp

bool doSomething()
{
  Object pObject = new Object();

  int result = step1(*pObject);

  if(result != 0)
  {
    delete pObject;
    pObject = NULL;
    return false;
  }

  // 内部可能抛出exception
  step2(pObject->aPublicMember);

  delete pObject;
  pObject = NULL;

  return true;
}

```

良好的风格：

```cpp
class ObjectGuard
{
  public:
    ObjectGuard(Object* obj): m_obj(obj) {}

    ~ObjectGuard()
    {
      delete m_obj;
      m_obj = NULL;
    }

    // 此处应考虑拷贝构造函数等因素，此处略

    Object& operator * ()
    {
      return *m_obj;
    }

    Object* operator -> ()
    {
      return m_obj;
    }

  private:
    Object * m_obj;
};


bool doSomething()
{
  ObjectGuard pObject(new Object());

  int result = step1(*pObject);

  if(result != 0)
  {
    return false;
  }

  // 内部可能抛出exception
  step2(pObject->aPublicMember);

  return true;
}
```

更好的方式应该是使用C++11标准库的[`unique_ptr<T>`](http://en.cppreference.com/w/cpp/memory/unique_ptr)或[`shared_ptr<T>`](http://en.cppreference.com/w/cpp/memory/shared_ptr)来管理内存，[`lock_guard<std::mutex>`](http://en.cppreference.com/w/cpp/thread/lock_guard)等管理锁。

__参考__

* [CCG R.1: Manage resources automatically using resource handles and RAII (Resource Acquisition Is Initialization)](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#r1-manage-resources-automatically-using-resource-handles-and-raii-resource-acquisition-is-initialization)
* [CCG R.20: Use `unique_ptr` or `shared_ptr` to represent ownership](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#r20-use-unique_ptr-or-shared_ptr-to-represent-ownership)
* [CCG R.21: Prefer `unique_ptr` over `shared_ptr` unless you need to share ownership](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#r21-prefer-unique_ptr-over-shared_ptr-unless-you-need-to-share-ownership)

### B.4 防止不必要的范围扩大

* 严禁全局变量，考虑更小的作用域，或者单例模式能否满足需求。
* 避免污染全局命名空间：减少`using namespace xxx`的使用。头文件中禁用，定义文件中优先考虑直接在用时加命名空间前缀，其次考虑使用`using xxx::yyy`。
* 局部变量的作用域应尽可能小。
* 成员变量和成员函数，应尽可能减少可访问它的范围。

### B.5 变量应有清晰而唯一的用途

* 仅在即将初始化和使用时，声明变量。
* 不应保留未使用的变量。
* 不应该把1个变量用作多个用途。

__因由__

> Perfection is achieved, not when there is nothing more to add, but when there is nothing left to take away. —— Antoine de Saint-Exupery

上面的“take away”不仅代表“去除”，也包含“合并”。

远离初始化和使用的变量声明的坏处：

* 代码可读性下降。
* 容易引发误用。

未使用的变量的坏处：

* 可能它本来是有用的，但用错了别的变量，导致它被漏用。如果养成习惯留着未使用的变量，就难以发现漏用的变量。
* 它可能与别的变量形似，从而被误用。

多用途的变量的坏处：

* 代码可读性下降：多个用途，容易让人难以理解代码意图，尤其是无关用途。
* 状态残留：同一个变量前一次被使用时的状态，可能因为一些原因未清理干净，从而影响到后一次使用。
* 资源泄露：指针、句柄类的变量，在复用时，可能导致原来指向的资源未正确释放。

__样例__

```cpp
/*
  不良的风格：

  * 没有立刻初始化。
  * 一行声明多个变量，违反F.6
*/
int i, j, k;
// 100行无关代码
i = 3;
// 100行无关代码
j = 4;
// 100行无关代码
k = i + j;

/*
  不良的风格：代码声明后没有使用。
*/
int neverUsed;

/*
  不良的风格：一个变量用作多个用途。
*/
ResultType result = step1();
// ...判断result有效并使用的...
result = step2();
```

__参考__

* CCG ES.20: Always initialize an object
* CCG ES.21: Don't introduce a variable (or constant) before you need to use it
* CCG ES.22: Don't declare a variable until you have a value to initialize it with
* CCG ES.26: Don't use a variable for two unrelated purposes

### B.6 规避已知编码问题

不使用可能带来如下编码安全问题的高危函数，使用其更安全的替代者：

* 缓冲区溢出
* 类型安全
* Shell安全
* SQL注入、XSS等元字符转义型漏洞
* CSRF等跨站型漏洞
* 线程不安全
* 参数校验缺失
* 权限校验缺失
* ……

通过学习吸取前人教训，不重蹈编程反模式的覆辙。

__因由__

已经广为人知的坑，自己应该知道并规避，不要因为惰怠而付出血泪的代价。

__参考__

团队应维护自己的高危函数列表，收集时可以参考如下列表：

* [MSDN: Security Development Lifecycle (SDL) Banned Function Calls](https://msdn.microsoft.com/en-us/library/bb288454.aspx)
* [IBM developerWorks: 使您的软件运行起来: 防止缓冲区溢出](https://www.ibm.com/developerworks/cn/security/buffer-defend/)
* Apple Developer Secure Coding Guide：
  * [Types of Security Vulnerabilities](https://developer.apple.com/library/ios/documentation/Security/Conceptual/SecureCodingGuide/Articles/TypesSecVuln.html)
  * [Security Development Checklists](https://developer.apple.com/library/prerelease/ios/documentation/Security/Conceptual/SecureCodingGuide/SecurityDevelopmentChecklists/SecurityDevelopmentChecklists.html)

反模式：

* [Common Weakness Enumeration (CWE)](https://cwe.mitre.org/data/index.html)
* [What Errors Are Included in the Top 25 Software Errors?](http://www.sans.org/top25-software-errors/)
* [Introduction to Software Engineering/Architecture/Anti-Patterns](https://en.wikibooks.org/wiki/Introduction_to_Software_Engineering/Architecture/Anti-Patterns)
* [Wikipedia: Software Engineering Anti-pattern](https://en.wikipedia.org/wiki/Anti-pattern#Software_engineering)
* [9 Anti-Patterns Every Programmer Should Be Aware Of](http://sahandsaba.com/nine-anti-patterns-every-programmer-should-be-aware-of-with-examples.html)
* [Software Development AntiPatterns](https://sourcemaking.com/antipatterns/software-development-antipatterns)

### B.7 重视警告

应重视编译器和静态/动态代码检查的警告，及时修正，逐步趋近0容忍。

反对因为缺乏代码质量相关知识，而误认为告警很多是误报或者不是什么大问题。

静态/动态代码检查工具列表：

* [Cppcheck](http://cppcheck.sourceforge.net/)
* [Clang Static Analyzer](http://clang-analyzer.llvm.org/)
* [Valgrind](http://valgrind.org/)

函数体代码组织（Function Body）
-----------------------------

### FB.0 函数应中心明确，语义清晰，详略得当，条理分明

优美的代码就像一首诗。或者至少，像一篇文章。

* 中心明确：一个函数应该具备高内聚，即围绕一个中心展开，其中心思想，应该能在7-10个字内说清楚，而且不带连词。
* 语义清晰：代码中应综合运用常量/枚举名、变量/参数名、（被本函数调用的）函数名甚至宏名，将操作对象、操作动作等类似主谓宾的重点要素突出出来，而不是充满底层细节。
* 详略得当：大致遵循代码重要程度和代码“面积”成正比的原则，避免大量底层细节铺开来形成多行冗长的代码，却让抽象程度更高、更贴近语义和意图、代表着关键步骤环节的代码，被淹没其中。
* 条理分明：步骤间按符合时间、空间或逻辑的顺序和关系进行组织；步骤间中间结果的承转传递，清晰合理。

__贯彻手段__

先写注释规划代码的关键步骤，再写代码。

### FB.1 函数参数

基本要素：

* 定义函数时，参数顺序为：输入参数在前，输出参数在后。
* 输入参数对非基本类型应使用`const`引用。
* 输出参数应使用非`const`引用，并与返回值合理搭配，或去掉返回值。
* 返回值对非基本类型应优先使用`const`引用，谨慎使用传值或非`const`引用。
* 不修改对象成员变量的函数，必须标注为`const`。
* 禁止带throw (XXXException)的异常声明。原因：如果函数内抛的异常与指定的异常类型不符，程序会crash，并带来debug的困难。

参数组织：

* 一般而言，第1个参数应与函数名的动词，形成“动-宾”结构。
* 多个参数应按关联度分组，同组的参数放在一起。
* 避免非常长的参数列表，比如大于10个的参数基本可以确定不合理，此时应将同组的参数合并为一个结构体或类传入。
* 参数名避免出现意义相同或相似的命名，要能从参数名区分出各个参数的区别和用途。
* 谨慎使用`bool`类型的参数，而应优先考虑枚举，获得更清晰的语义和可扩展性，除非：
  - 的确是二元互斥关系
  - `bool`类型的参数是这个函数的唯一参数
  - 函数名能够表达清楚怎样是`true`

### FB.2 函数复杂度

倾向于编写短小、凝练的函数，便于他人阅读和修改代码。

如果函数超过40行，说明这个函数很可能存在潜在设计问题，上下文之间存在隐含的依赖关系，容易产生难以发现的bug。

但也注意，如果过度追求很短的函数，可能带来很深的函数调用层次，同样也会不利于他人阅读和修改代码。所以要寻找深度和长度之间平衡点。

### FB.3 主流程函数

建议在一个子功能的最外层的主流程函数，采取如下方式：

里面只有顺序调用多个步骤函数，每个函数没有入参也没有出参。通过成员变量来传递中间状态和结果。

### FB.4 写周全的`if/else if/else`

对于任何需要通过`if`判断的代码，应：

* 通过`if`/`else if`将所有可能出现的具体情况枚举出来，相应处理。
* 通过`else`表达未预料的、不应出现的或默认的情况，相应处理。

注意：

* 每一个分支都应该加以处理，如果实在无需处理或理应忽略，也应注释说明，不能留空。
* `else`分支，可考虑报错、抛异常、中断循环、上报统计、发送告警、打日志并忽略、默认逻辑等处置方式。
* 避免使用有“且”、“或”的条件。如果要使用，需要十分仔细地分析其他的`else if`分支有没有不小心漏数了一些情况。

反对：

* 多层嵌套无`else`甚至无`else if`的裸`if`块。
* `if`了一个具体情况，`else`了其他所有。

__例外__

允许不写周全的`if/else if/else`的情况：

* 专用于异常情况或终止条件处理的短路`if`块。
* 专用于可选步骤的`if`块，应通过注释标明“可选步骤”这个语义。该例外不可滥用。

__样例__

```cpp
/*
  良好的风格：

  * 枚举每种情况，对未能枚举的情况，给出了针对性处理。
  * 被比较的常量在==的前方，防止误将==写成=
*/
if(CONDITION1 == value)
{
  // 处理CONDITION1
}
else if(CONDITION2 == value)
{
  // 处理CONDITION2
}
else
{
  // 未预料的、不应出现的或默认的情况
  throw XXXException("出错的信息");
}

/*
  良好的风格：专用于异常情况或终止条件处理的短路`if`块
*/
while(/* 控制主循环条件 */)
{
  if(/* 循环结束的另一条件 */)
  {
    break;
  }
  // ...
}

int fun(int input)
{
  if(input <= 0)
  {
    throw XXXException("input必须大于0");
  }

  int ret = input + 1;

  return ret;
}

/*
  良好的风格： 专用于可选步骤的`if`块，通过注释标明“可选步骤”这个语义。
*/
void fun()
{
  mandatoryStep1();

  mandatoryStep2();

  // 可选：仅当XXX时
  if(condition)
  {
    optionalStep();
  }

  mandatoryStep3();
}

/*
  不良的风格：

  * 多层裸`if`
  * 被比较的常量在==的后方，容易误将==写成=而难以察觉
*/
if(value1 == CONST1)
{
  // 某些动作1
  if(value2 == CONST2)
  {
    // 某些动作2
    if(value3 == CONST3)
    {
      // 某些动作3
    }
  }
}

/*
  不良的风格：情况枚举时使用了复杂的组合条件
*/
if(CONST1 == value1 && CONST2 == value2)
{
  // 某些动作1
}
else if(CONST1 == value1 && CONST3 == value2)
{
  // 某些动作2
}
else
{
  // 默认动作
}
```

### FB.5 返回值风格的异常处理

任何时候都要检查函数的返回值。如果因为一些原因，要对返回值进行忽略，应在代码上显式体现这个意图。

### FB.6 C++异常风格的异常处理

要善用异常，不要如[Google C++ Style Guide： Exceptions](http://google.github.io/styleguide/cppguide.html#Exceptions)那般恐惧异常。

在有限的集中的位置捕获所有异常：

* `try...catch`块在一个函数体内的范围应尽可能大，将整个主逻辑全部包含，在末端对所有异常统一处理，不要一步一异常。
* 上一条有个例外：`for/while`循环体，宜在每一次循环内捕获并处理异常，从而尽可能不让循环中止，除非本来就希望异常能中断循环。
* 不要每层函数都处理异常。最里层的函数，一般都可以直接抛异常。中间层次的函数，一般不处理所调用函数的异常，统一交给外层的函数来处理。
* 最外层一定要有兜底的异常捕获，避免不必要的程序终止。
* 如果是写公共库，那么公共库要定义自己的异常类型（更基础宽泛的语义），不要直接使用调用方侧的异常类型（更具体的语义）。调用方在异常穿越库与应用的边界时，要做异常类型的转换，并为其提供更多的语境信息。

写异常安全的代码：

* 捕获异常时应写至少3个`catch`块：业务自定义异常、`std::exception`、`...`。
* 捕获异常时应用const引用。
* 不要将整数、字符串等基础类型作为异常抛出。
* 任何时候都要假设代码可能异常被中断，包括被异常、`break`、`return`等，熟练使用RAII技术做好资源管理，防止资源泄漏，见B.3 。
* 严禁使用带throw (XXXException)的异常声明，见FB.1 。

__样例__

```cpp
/*
  良好的风格：
*/
void inner()
{
  if(conditionXXX)
  {
    throw XYZException(/* 错误码 */123456, /* 错误信息 */"某某条件不满足");
  }
}

void middle()
{
  inner();
}

void outter()
{
  try
  {
    middle();
  }
  catch(const XYZException & e)
  {
    // e.getErrorCode()
    // e.getErrorInfo()
  }
  catch(const std::exception & e)
  {
    // e.what();
  }
  catch(...)
  {
    // 打印日志、统计或告警
  }
}

var cycle()
{
  while(condition)
  {
    try
    {
      middle();
    }
    catch(const XYZException & e)
    {
      // e.getErrorCode()
      // e.getErrorInfo()
    }
    catch(const std::exception & e)
    {
      // e.what();
    }
    catch(...)
    {
      // 打印日志、统计或告警
    }
  }
}

/*
  不良的风格：
*/
void fun()
{
  try
  {
    step1()
  }
  catch(const XXXException & e)
  {
    // ...
  }

  try
  {
    step2()
  }
  catch(const YYYException & e)
  {
    // ...
  }
}
```

类体代码组织
----------------

* 构造函数初始化成员
* 拷贝构造函数等全或无
* 虚析构函数
* `public`成员在前
* `protected`成员函数，`private`成员变量

类组织
----------------

* 轻类
* 尽一切可能避免继承，优先使用组合

语言特性使用（Language Feature）
------------------------------

### L.1 C语言特性使用

* 克制对宏的使用：
  - CCG ES.30: Don't use macros for program text manipulation
  - CCG ES.31: Don't use macros for constants or "functions"
  - CCG ES.33: If you must use macros, give them unique names

### L.2 C++98/03语言特性使用

* 善用模版基础特性辅助类型推导和函数式泛型编程

__参考__

《Extended STL》

### L.3 C++11/14语言特性使用

* [Elements of Modern C++ Style](http://herbsutter.com/elements-of-modern-c-style/)
* [GotW #91 Solution: Smart Pointer Parameters](http://herbsutter.com/2013/06/05/gotw-91-solution-smart-pointer-parameters/)
* [Guru of the Week(Revised for C++14)](http://herbsutter.com/gotw/)
* [C++11/14 References](https://github.com/numenta/nupic/wiki/C--11-14-References)

标准库与第三方库使用
------------------

第三方库可以从这里寻找备选者：[Awesome C/C++](https://github.com/fffaraz/awesome-cpp)

团队定制规则
--------------

> 注：本节单独留空，团队可在此添加自己定制的规则。

__因由__

* 团队必然有自己的规则偏好，如果和上面的规则（称之为“通用规则”）冲突或有补充，可以在此：
  - 废弃相应通用规则
  - 指定通用规则中给出的几个备选规范中的某个
  - 新写自己的定制规则
* 不建议直接修改/删除通用规则部分，因为将来通用规则部分如果更新增补，则不易整合进来。

__参考__

> 注：这里可以给出团队所在组织的内部参考文献。

参考文献
-----------

* [Google C++ Style Guide](http://google.github.io/styleguide/cppguide.html)
* [LLVM Coding Standards](http://llvm.org/docs/CodingStandards.html)
* [C++ Coding Standards](http://www.gotw.ca/publications/c++cs.htm)
* [C++ Core Guidelines](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md)
* [Chromium’s style guide](http://www.chromium.org/developers/coding-style)
* [Mozilla’s style guide](https://developer.mozilla.org/en-US/docs/Developer_Guide/Coding_Style)
* [WebKit’s style guide](http://www.webkit.org/coding/coding-style.html)
* [Joint Strike Fighter Air Vehicle C++ Coding Standards](http://www.stroustrup.com/JSF-AV-rules.pdf)
* [POCO C++ Libraries Coding Style Guide](http://www.appinf.com/download/CppCodingStyleGuide.pdf)
* [Blink Code Style](http://www.chromium.org/blink/coding-style)
* [NuPIC C++ Coding Guide](https://github.com/numenta/nupic/wiki/C-Coding-Guide)
* [林锐的《高质量C++/C 编程指南》](http://www.chinastor.org/upload/2014-04/14040815326461.pdf)
* TODO: 华为C++编码规范
* [Principles Wiki](http://principles-wiki.net/principles:start)
* [Common Weakness Enumeration](https://cwe.mitre.org/index.html)
