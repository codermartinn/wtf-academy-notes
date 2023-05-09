# Solidity 入门笔记

## 01.快速入门



### solidity 概述

Solidity是一种面向合约编程语言，它被设计用于在以太坊平台上编写智能合约。它是一种高级语言，类似于JavaScript，并且具有静态类型和编译功能，可以在以太坊虚拟机上执行。

智能合约是一种自动执行的程序，可以在以太坊上创建和执行，它们通常用于处理数字资产和执行交易。Solidity提供了一种在以太坊上创建智能合约的方式，它是以太坊上最受欢迎的编程语言之一。

Solidity具有强大的特性，例如可编程性、透明性、自动化执行和安全性等。它具有许多内置的库和工具，可以轻松地编写和测试智能合约。同时，Solidity还具有广泛的社区支持，以及可靠的文档和教程，使得开发者可以更轻松地开始使用它。



### IDE：REMIX

![image-20230509213243283](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230509213243283.png)



网址：[remix.ethereum.org](https://remix.ethereum.org/)



Remix是一种基于浏览器的Solidity集成开发环境（IDE），它为开发人员提供了一种便捷的方式来编写、部署和测试智能合约。它是一个免费的工具，可以直接在浏览器中访问，无需安装任何软件。

Remix具有许多有用的功能，包括：

1. 代码编辑器：Remix提供了一个代码编辑器，可以帮助开发者编写Solidity智能合约代码。
2. 编译器：Remix集成了Solidity编译器，可以编译智能合约代码，生成字节码。
3. 调试器：Remix提供了一个调试器，可以帮助开发者调试智能合约代码。
4. 部署工具：Remix可以帮助开发者将智能合约部署到以太坊网络中。
5. 测试框架：Remix提供了一个内置的测试框架，可以帮助开发者编写和运行测试用例。
6. 交互式控制台：Remix提供了一个交互式控制台，可以让开发者与智能合约进行交互。

总之，Remix是一个非常有用的工具，可以大大简化Solidity智能合约的开发和测试过程。它是Solidity生态系统中不可或缺的一部分，被广泛使用并受到开发者们的喜爱。





### 用solidity编写HelloWorld

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract HelloWorld{
    string public str = "Hello World!";
}
```

这是一个基本的Solidity智能合约，它包含一个名为"HelloWorld"的合约。下面是每行代码的解释：

1. `// SPDX-License-Identifier: MIT`：这是一个特殊的注释，用于指定合约的许可证类型。
2. `pragma solidity ^0.8.4;`：这是一个编译器指示符，告诉编译器合约应该使用的Solidity版本。"^"符号表示Solidity版本可以在0.8.4及以上，但必须低于0.9.0。
3. `contract HelloWorld{...}`：这是一个Solidity合约的声明，包含了合约的名称和代码。在本例中，合约名为"HelloWorld"。
4. `string public str = "Hello World!";`：这是一个公共状态变量（public state variable），它被声明为字符串类型。"public"关键字表示该变量可以被外部访问，"str"是变量名，"Hello World!"是变量的初始值。

因此，该合约的功能非常简单，它仅仅包含一个名为"str"的公共字符串变量，其值为"Hello World!"。其他的功能，例如函数调用、事件、修饰器等，都没有被包含在此合约中。

![image-20230509214504131](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230509214504131.png)





## 02.数值类型

Solidity支持多种不同的数据类型，包括以下几种主要类型：

1. 布尔类型（Boolean）：包括true和false两个值。
2. 整数类型（Integer）：包括int、uint、int8、uint8等，其中int和uint表示有符号和无符号整数类型，8表示位数。
3. 地址类型（Address）：表示以太坊账户的地址，长度为20个字节。
4. 字节类型（Bytes）：包括bytes和byteN（其中N表示字节长度，范围为1到32）。
5. 字符串类型（String）：表示UTF-8编码的字符串。
6. 固定点数类型（Fixed Point）：包括fixedMxN和ufixedMxN，其中M表示位数，N表示小数位数。
7. 枚举类型（Enum）：表示一组可能的离散值，例如：enum Color { RED, GREEN, BLUE }。
8. 数组类型（Array）：包括动态数组和静态数组，静态数组长度在声明时指定。
9. 结构体类型（Struct）：表示可以包含多个不同类型成员变量的自定义类型。

此外，Solidity还支持映射类型（Mapping）、函数类型（Function）、事件类型（Event）等其他类型。这些数据类型提供了丰富的功能，可以满足智能合约开发中的各种需求。



### 布尔类型



```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract HelloWorld{
    // 布尔类型
    bool public _bool = true;
    
    // 布尔运算
    bool public _bool1 = !_bool; //取非
    bool public _bool2 = _bool && _bool1; //与
    bool public _bool3 = _bool || _bool1; //或
    bool public _bool4 = _bool == _bool1; //相等
    bool public _bool5 = _bool != _bool1; //不相等
}
```

