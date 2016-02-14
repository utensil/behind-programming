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

体例结构
--------

编程规范的编辑体例，应该像GoF在著名的《设计模式》中一样，构筑自洽完备的表述结构，就像代码的组织要遵循良好的结构一样。

反对《Google C++ Style Guide》的“Definition/Pros/Cons/Decision”的体例，更像一份探讨而不是规范，留下了很多模糊的空间，没有保持逻辑的自洽，更像一个折衷产物和半成品。《LLVM Coding Standards》的体例更像散文。《C++ Coding Standards》的“Item title/Summary/Examples/Exceptions/References”不够完备。

本编程规范的编辑体例与组织结构，基本与C++之父Bjarne Stroustrup最新尚在Github上协作撰写的、面向C++1z的《C++ Core Guidelines》一致，从其他编程规范中，也吸取了部分元素。

编程规范由许多条编程规则组成，相关联的规则，组成小节。

每个规则具备如下要素：

* __编号__（Reference Number）：为了方便在代码review中引用。采用字母缩写与数字序号相结合的方式。同一小节内的字母缩写一致，与该节内容的单词所含的字母相关。优先采用单个的大写字母，其次考虑首字母大写、其余字母小写的缩写。
* __规则标题__（Rule）：言简意赅的规则描述，采用格言文风。其表述只需让初读者能猜出大致与什么相关，以及让已阅者朗朗上口，易于回想其含义即可。
* __阐述__（Elaboration）：对规则的含义进行一定的展开描述。一般是先阐述规则的内涵，再通过列表的形式来展开规则的外延。
* __因由__(Reasons/Rationales)：解释这样做的好处、不这样做的坏处，以及该规则为什么可以入选这个“最小集”。自由桀骜的程序员不会盲从权威的规定，但愿意遵循合理的游戏规则。
* __样例__(Examples)：包括正面和反面的例子，可以适度包括对其后果的演示。抽象的描述是难以让人理解领会的。样例能够帮助人在提炼共性中加深理解。
* __备选方案__（Alternatives）：适用于禁令式的规则，给人一条出路。被禁止的做法原来想解决的问题，总得有个被鼓励/允许的做法来解决。演示怎样做才能规避被禁止的做法的弊端，而不引入新的弊端。
* __例外__（Exceptions）：规则往往有例外，如果不将有限的、可以接受的例外纳入规则，规则就会被无穷的、不可接受的例外所吞噬。
* __贯彻手段__(Enforcement)：如何机械式地在代码自查/检视中发现这些问题？如何使用自动化工具（如静态代码检查工具扫描、强制）？
* __参考__（See alsos）：参考相关的规则。
* __探讨__（discussion）：先贤及其他编程规范，对该规则的阐述与讨论，包括一些反对该规则的理由。

每个规则可复制如下这段模板后开始撰写，可适当移除不必要的要素：

```
### X.1 规则标题

...阐述直接开始...

__因由__

__样例__

__备选方案__

__例外__

__贯彻手段__

__参考__

__探讨__
```

理念（Philosophy）
-----------------

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

TODO 此处应该引用一些规则，或展开阐述一些具体原则。

具体例子包含：

* 一致性：
* 向下兼容性：

__探讨__


### P.2 品质的生活

生活，即接地气，不脱离实践、架空、学院派，不陷入盲目的理想主义，不会教条主义地适用某个牛逼的原则。

品质，即品味与质量，能够拒绝不好的味道，能够追求更优的质量，更高的标准，更佳的实践。

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

__探讨__

### P.3 留空

TODO 待写成更多具体规则后，提炼补充。

初步看，这里应该类似：以人为本，让计算机的特性为人（需求方、开发人员、测试人员、运维人员）服务。

原则
--------

TODO 后继寻求合理方案替代/废弃本节

### 一致性

### 向下兼容性

### 名正言顺

https://en.wikipedia.org/wiki/Naming_convention_(programming)

http://unbug.github.io/codelf/

### 详略得当

代码格式化（Format）
------------------

### F.0 克制重新格式化代码的冲动

对已有的代码基，尽量模仿其格式和风格，哪怕只是一个文件、一个类、一个方法的局部，而不要轻易因为个人喜好，重新格式化。

__因由__

* 在没有取得对于格式的统一认识之前，每个人都因个人喜好重新格式化代码，会形成代码格式的冲突和混乱。
* 随意地重新格式化代码，会使得代码review时无法清晰看出有意义的修改点，从而导致隐藏bug。

__例外__

团队内取得共识，有计划地基于成文的格式规范，集中进行代码的重新格式化，自然是可以的。但注意：

* 格式化的修改需要单独commit，并在commit log中加以说明。
* 千万不能在一次commit中混杂有意义的修改和纯粹的格式化。
* 也不要在一系列的commit中穿插有意义的修改和纯粹的格式化，而应分别集中做有意义的修改，和纯粹的格式化。

__贯彻手段__

团队应在代码基形成初期就对代码格式达成统一共识，集中将代码格式统一，并提供统一工具在每次commit前自动格式化，确保后继无需再为格式操心。

__参考__

__探讨__

http://llvm.org/docs/CodingStandards.html#introduction :

> ...we explicitly do not want patches that do large-scale reformating of existing code. On the other hand, it is reasonable to rename the methods of a class if you’re about to change it in some other way. Just do the reformating as a separate commit from the functionality change.

### F.1 注释

* 注释传递意图(why)，而非做法（how）。
* 内外部API，如公有方法、公有成员等，必须采用Doxygen格式注释。
* 会被多处使用的实体，如类的静态变量、成员变量、常量、枚举等，必须采用Doxygen格式注释。
* 类、方法、成员变量等实体，采用Doxygen格式的`/** ... */`，在实体上方。
* 方法体内的一行或一小段代码，采用`//`，在代码段上方，反对在代码段后端的写法。

__因由__

TODO 待重新表述

* 代码中的文档不是脱离于代码，而是为了更好地维护代码，要与代码一起更新，过时的文档比没有文档危害更大
* 注释与文档不仅仅是写给别人看，也是写给几个月后忘记了代码逻辑的自己看
* 用类似Javadoc或Doxygen的格式为各个方法在注释中撰写API文档
* 代码实现中，通过变量和方法的命名与代码结构的组织来增强可读性，通过注释来补充说明意图

__样例__

TODO 待翻译；待纳入TODO、@deprecated等

```

  /** One line class description for Doxygen.
   *
   * A detailed description follows the one line description.
   * Detailed description should include responsibility (what
   * does this class do?).
   *
   */
  class ClassName
  {
  public:

    /**
     * Brief description of method for Doxygen.
     *
     * More detailed description includes explanation of error checking.
     */
    virtual void anExampleMethod();

    /**
     * Brief description of method for Doxygen.
     *
     * More detailed description includes explanation of parameters
     * and error checking.
     *
     * @param foo Description of parameter foo including valid
     * range and (any) error checking.
     *
     * @retval Description of return value of this method.
     */
    virtual int anotherMethod(int foo);

  protected:

  private:
    /** An example private data member. */
    int m_AnExampleDataMember;

    /**
     * Default ctor, copy ctor and assignment operator
     * forbidden by default
     */
    ClassName();
    ClassName(const ClassName &);
    ClassName& operator=(const ClassName&);

  }; // end class ClassName

```

__备选方案__

__例外__

__贯彻手段__

__参考__

https://github.com/numenta/nupic/wiki/Documenting-NuPIC-with-Doxygen#documenting-conventions

__探讨__

### F.2 `#include`

* `#include`应只包含最小必要的头文件。
* `.h`文件中，能用前向声明解决的，就不要`#include`。
* 用 `#include <filename.h>` 格式来引用标准库的头文件（编译器将从
标准库目录开始搜索）。
* 用`#include "filename.h"` 格式来引用非标准库的头文件（编译器将
从用户的工作目录开始搜索）。
* `#include`应遵循如下顺序：
  - 自己的头文件（仅使用`.cpp`文件）
  - 本项目的头文件
  - 操作系统/第三方库的头文件
  - C/C++标准库的头文件

__因由__

* 最小必要：提升编译速度
* 包含顺序：消除隐性依赖

__样例__

__备选方案__

__例外__

__贯彻手段__

__参考__

__探讨__

http://llvm.org/docs/CodingStandards.html#include-style

### F.3 行宽

80字节。

__因由__

* 这是最广泛采用的标准，少量增加（如90）并无意义，只会带来不一致。
* 虽然现在宽屏已普及，但需要考虑代码对比时，新老文件左右并列显示时依然能完整显示一行。
* 短的行宽，能够自然抑制深度嵌套。

__样例__

__备选方案__

__例外__

__贯彻手段__

__参考__

__探讨__

### F.4 缩进

* 2个空格，禁用Tab。
* 有层次结构的地方，均应正确缩进，但namespace一律不缩进

__因由__

主要是为了代码一致的可读性。

* 禁用Tab，尤其禁用Tab和空格混用，不禁用的后果是各种编辑器一打开，不同人写的代码就乱套了；
* 不采用浪费空间的4个空格，而是紧凑的2个空格，控制行宽。
* 缩进可以清晰传递层次结构信息，错误的缩进会误导、扭曲语义。
* namespace如果缩进，会带来过深的缩进，所以规避之。

__样例__


```
namespace {

void foo() {  // Correct.  No extra indentation within namespace.
  ...
}

}  // namespace
```

__探讨__

关于namespace：

* http://google.github.io/styleguide/cppguide.html#Namespace_Formatting
* http://llvm.org/docs/CodingStandards.html#namespace-indentation

### F.5 括号

* 严禁无花括号的if/else语句。
* 一致地使用两种花括号换行风格之一。

__样例__

花括号换行风格1（Java社区主流）：

```
if(xxx) {
  
} else {
  
}
```

花括号换行风格2（C++社区较多见）：

```
if(xxx)
{

}
else
{

}
```

### F.6 空格

文件代码组织(File)
-------------------

### Fi.1 头文件

http://google.github.io/styleguide/cppguide.html#The__define_Guard

### Fi.2 源文件

方法体代码组织
----------------

类组织
----------------

* 尽一切可能避免继承，优先使用组合
* 

if-else
----------------

语言特性使用
------------------

标准库与第三方库使用
------------------

参考文献
-----------

* [Google C++ Style Guide](http://google.github.io/styleguide/cppguide.html)
* [LLVM Coding Standards](http://llvm.org/docs/CodingStandards.html)
* [C++ Coding Standards](http://www.gotw.ca/publications/c++cs.htm)
* [C++ Core Guidelines](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md)
* [NuPIC C++ Coding Guide](https://github.com/numenta/nupic/wiki/C-Coding-Guide)
* [林锐的“高质量C++/C 编程指南”](http://www.chinastor.org/upload/2014-04/14040815326461.pdf)
