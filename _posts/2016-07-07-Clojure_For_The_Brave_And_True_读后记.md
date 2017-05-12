---
title: Clojure For The Brave And True 读后记
layout: post
tag: 读后记
---

最近在线读过 [Clojure For The Brave And True][CFBT] 一书。该书的内容比较丰富，插图和文笔使我联想到另一部在线书 [Learn You A Haskell For Great Good][LYHFGG]。文字幽默，观点清晰，部分例子略显黑暗（勇敢者的Clojure书）而且对于不是英文母语的读者略显困难，但不影响观点的理解。

作者不假设读者有 Java 基础，并在一开始避免强调 Clojure 与 Java 高下比较，试图留给读者以 Clojure 解决问题的方法，而不是单纯 Clojure 知识介绍。全书分为两部分：基础部分和高阶部分，共十三个章节，外加两个附录（关于 Leiningen 和 Boot 项目管理工具）。

* 基础部分

  - 从 REPL 交互和 Emacs 使用开始讲起，系统介绍了 Clojure 的语法、数据结构、函数（解构、匿名函数和闭包）等。
  - 用单独的两个章节描述了 Clojure 中抽象数据结构 Sequence 和 Collection，并与 OOP 对比解释纯函数和不可变数据结构编程范式的优点。
  - 解释了变量（identifier）定义与 Namespace 的概念，并穿插有 Leiningen 相关使用方法
  - 宏，说明为什么 Clojure（或Lisp）与众不同，引导读者构造宏并展开，反复强调 Symbol 与 var 的关系，并证明宏不特殊、强大、要慎用

* 高阶部分

  - 并行编程，从并行编程几个基本问题出发，分三章分别介绍了
    - Clojure 基本异步处理方法：`future`, `delay` 和 `promise` 三个函数，能将求值过程的三个阶段定义、执行、取结果拆分开来
	- Clojure 对事务的解决方案STM：`atom`, `ref`，并提到了动态绑定的原理、使用、风险
	- Clojure 中模仿 Google Go 并发模型提出的 `core.async` 异步通信模型解决方案
  - Clojure 多态与类抽象，涉及 Multimethod，Protocol 和 Record



读过后，留下最深印象的有这么一些观点和思考

* OOP 解决问题方法过于呆板，导致有大量的专用操作方法，并不能达到通用；而 Clojure 强调的对数据的抽象使得更多时候有大量通用函数可用。两者并无高下，但个人认为后者可能更能调动人的思考和对对象的抽象，而非浪费时间在大量的查手册（ *It is better to have 100 functions operate on one data structure than 10 functions on 10 data structure*）
* Clojure 的函数重载和多态比 Java 支持更丰富，更通用
* Clojure 继承 Lisp 语法，语法即数据，及求值模型使得语法易扩展，并独特。但并不能因为语法独特而放松对设计的把握
* Clojure 中抽象数据 Sequence 和 Collection 并不特别，使之特别的在于不可变与纯函数的结合
* 纯函数是好习惯，但不能因之而放弃变化，因为编程最终需要的还是变化和副效应
* 宏很危险，有很大的坑，学习宏有助于提高对求值过程的理解
* Clojure 提供的并发工具和框架能够简化并发开发，但不能替代流程设计
* 不要试图在 Java 和 Clojure 比出高下，两者有不同的思考和设计。但要明白两者的差异。( *Clojure is a bit like a utopian commnity plunked down in the middle of a dystopian country* )
* Clojure 中多态抽象很有用，特别是 Protocol + Record 方式
* 学习 Clojure 不仅在于其库和语法，而在于解决具体问题时的思考方法 



[CFBT]: http://www.braveclojure.com/  "Clojure For The Brave And True"
[LYHFGG]: http://learnyouahaskell.com/ "Learn You A Haskell For Great Good"
