## Dart 基础

![Dart product logo](https://dart.dev/assets/img/logo_lockup_dart_horizontal.png)

Dart是一种面向对象、类定义、垃圾回收的编程语言，由Google开发并用于构建移动、桌面、服务器和Web应用。Flutter框架就是使用Dart语言编写的。Dart的语法类似于C和Java，因此对于熟悉这些语言的开发者来说很容易上手。下面是一些Dart语言的基本概念和特性。

1. 变量和类型：

    * 变量使用`var`关键字声明，类型可以是静态（如`int`、`String`、`bool`等）或动态（`dynamic`）。
    * Dart支持类型推断，因此在声明变量时可以省略类型。
    * 使用`final`和`const`关键字定义不可变变量。
2. 控制流程：

    * `if`、`else if`和`else`用于条件判断。
    * `for`和`while`用于循环，`break`和`continue`用于控制循环流程。
    * `switch`和`case`用于多条件判断。
3. 函数：

    * 使用`void`关键字定义没有返回值的函数，其他返回类型直接使用类型名。
    * 支持匿名函数（lambda表达式）和箭头函数（单行函数）。
    * 支持可选参数（位置可选参数和命名可选参数）和默认参数值。
4. 面向对象编程：

    * 使用`class`关键字定义类，`extends`关键字用于继承。
    * 支持抽象类（`abstract`关键字）和接口。
    * 构造函数可以有多个（命名构造函数），可以使用初始化列表。
    * 支持getter和setter方法，用于属性访问和修改。
5. 异步编程：

    * 使用`Future`和`async`/`await`进行异步编程。
    * 使用`Stream`处理异步数据流。
6. 错误处理：

    * 使用`try`、`catch`和`finally`处理异常。
    * 使用`throw`抛出异常，自定义异常类可以继承自`Exception`。
7. 集合：

    * Dart中的集合有`List`（列表）、`Set`（集合）和`Map`（映射）。
    * 支持集合字面量和构造函数创建集合。
    * 支持集合操作，如添加、删除、查找和修改元素。
8. 导入和库：

    * 使用`import`关键字导入库，支持导入核心库、外部库和自定义库。
    * 使用`library`、`part`和`part of`关键字构建模块化代码。

了解这些基本概念和特性后，你就可以开始使用Dart语言进行Flutter开发了。要深入了解Dart语言，请参阅官方文档
