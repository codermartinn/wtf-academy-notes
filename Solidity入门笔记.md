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







### 测试

![image-20230513094412092](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230513094412092.png)

编写EVM的好像是golang。







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

跟Java中的布尔值是一样的。



### 整型

Solidity支持多种整型类型，包括有符号和无符号整型。这些整型类型的区别在于它们能够表示的数字范围不同。

有符号整型类型包括int8、int16、int32、int64、int128、int256等，其中int表示有符号整型。这些类型表示的数字范围从-2的n-1次方到2的n-1次方-1（n表示整型类型的位数），例如：int8表示8位有符号整数，可以表示的范围是-128到127。

无符号整型类型包括uint8、uint16、uint32、uint64、uint128、uint256等，其中uint表示无符号整型。这些类型表示的数字范围从0到2的n次方-1，例如：uint8表示8位无符号整数，可以表示的范围是0到255。

需要注意的是，整型类型的位数必须是8的倍数，且最大值为256位。在Solidity中，整型类型的默认值为0。

除了这些基本整型类型，Solidity还支持固定点数类型（Fixed Point），可以表示带小数点的数字，例如：fixed256x18可以表示一个带18位小数的256位固定点数。

在Solidity中，整型类型通常用于表示数字或者存储状态变量。需要根据实际情况选择合适的整型类型，以避免浪费存储空间或者溢出的问题。



```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract HelloWorld{
    // 整型
    int public _int = -1; // 整数，包括负数
    uint public _uint = 1; // 正整数
    uint256 public _number = 20220330; // 256位正整数

    // 整数运算
    uint256 public _number1 = _number + 1; // +，-，*，/     20220331
    uint256 public _number2 = 2**2; // 指数   4
    uint256 public _number3 = 7 % 2; // 取余数  1
    bool public _numberbool = _number2 > _number3; // 比大小   true
}
```



### 地址类型

在Solidity中，地址类型（Address）用于表示以太坊账户的地址。地址类型的长度为20字节，通常以16进制字符串的形式表示。地址类型是固定大小的，无法进行类型转换。

有普通的地址和可以转账`ETH`的地址（`payable`）。`payable`的地址拥有`balance`和`transfer()`两个成员，方便查询`ETH`余额以及转账。

代码：

```solidity
    // 地址
    address public _address = 0x7A58c0Be72BE218B41C608b7Fe7C5bB630736C71;
    address payable public _address1 = payable(_address); // payable address，可以转账、查余额
    // 地址类型的成员
    uint256 public balance = _address1.balance; // balance of address
```

简单来说：就是有`payable`修饰过的地址类型，可以调用`balance()`和`transfer()`



### 定长字节数组

在Solidity中，定长字节数组（Fixed-size byte array）是一种固定长度的字节数组类型，它的长度在声明时就被确定了，不能被修改。定长字节数组类型由关键字“bytes”和长度组成，例如“bytes32”表示一个长度为32字节的定长字节数组。

与动态字节数组不同，定长字节数组是紧凑的，不需要额外的长度信息，因此更加高效。同时，由于长度固定，定长字节数组的访问速度也更快，更加适合用于存储固定长度的数据。



代码：

```solidity
// 固定长度的字节数组
bytes32 public _byte32 = "MiniSolidity"; 
bytes1 public _byte = _byte32[0]; 
```



`MiniSolidity`变量以字节的方式存储进变量`_byte32`，转换成`16进制`为：`0x4d696e69536f6c69646974790000000000000000000000000000000000000000`

`_byte`变量存储`_byte32`的第一个字节，为`0x4d`。



**注意**：取第1个字节是取二进制的8位，毕竟存储在内存中还是二进制的。因此，当我们取出字节数组中的某一个字节时，需要将其看作二进制数进行处理，而不是十六进制数。



### 枚举 enum

枚举（`enum`）是`solidity`中用户定义的数据类型。它主要用于为`uint`分配名称，使程序易于阅读和维护。它与`C语言`中的`enum`类似，使用名称来代替从`0`开始的`uint`：

```solidity
    // 用enum将uint 0， 1， 2表示为Buy, Hold, Sell
    enum ActionSet { Buy, Hold, Sell }
    // 创建enum变量 action
    ActionSet action = ActionSet.Buy;
```



它可以显式的和`uint`相互转换，并会检查转换的正整数是否在枚举的长度内，不然会报错：

```solidity
    // enum可以和uint显式的转换
    function enumToUint() external view returns(uint){
        return uint(action);
    }
```

`enum`的一个比较冷门的变量，几乎没什么人用。



看看就好。





### 测试

![image-20230513092622999](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230513092622999.png)

这题不太懂，先记住吧。



![image-20230513091955559](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230513091955559.png)

计算机原理功底太差了。


二进制            8bit    00-11

十六进制        4bit    0000-1111    



1字节可以存储8bit，而十六进制用4bit足以，故1字节可以存储2个十六进制。

bytes4是4字节，故具有8个十六进制。





## 03.函数类型



### 函数的理解

solidity官方文档里把函数归到数值类型，但我觉得差别很大，所以单独分一类。我们先看一下solidity中函数的形式：

```solidity
    function <function name> (<parameter types>) {internal|external|public|private} [pure|view|payable] [returns (<return types>)]
```



看着些复杂，咱们从前往后一个一个看（方括号中的是可写可不写的关键字）：

1. `function`：声明函数时的固定用法，想写函数，就要以function关键字开头。
2. `<function name>`：函数名。
3. `(<parameter types>)`：圆括号里写函数的参数，也就是要输入到函数的变量类型和名字。
4. `{internal|external|public|private}`：函数可见性说明符，一共4种。没标明函数类型的，默认`internal`。
   - `public`: 内部外部均可见。(也可用于修饰状态变量，public变量会自动生成 `getter`函数，用于查询数值).
   - `private`: 只能从本合约内部访问，继承的合约也不能用（也可用于修饰状态变量）。
   - `external`: 只能从合约外部访问（但是可以用`this.f()`来调用，`f`是函数名）
   - `internal`: 只能从合约内部访问，继承的合约可以用（也可用于修饰状态变量）。
5. `[pure|view|payable]`：决定函数权限/功能的关键字。`payable`（可支付的）很好理解，带着它的函数，运行的时候可以给合约转入`ETH`。`pure`和`view`的介绍见下一节。
6. `[returns ()]`：函数返回的变量类型和名称。



> 直接记，没别的。





### 区分关键字pure和view



共同点：

- 不改变合约中的状态变量，不需要支付gas费



不同点：

- pure不能read和write合约中的状态变量
- view能read但不能write合约中的状态变量
- 默认可以既可以read也可以write合约中的状态变量



### 代码



#### pure and view

定义一个add()函数，每次调用，每次给number + 1。

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract FunctionTypes{
    uint256 public number = 5;
    
    // 默认
    function add() external {
        number = number + 1;
    }
}
```



如果给`add()`函数添加`pure`关键字，就会报错。

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract FunctionTypes{
    uint256 public number = 5;
    
    // 默认;添加pure
    function add() external pure {
        number = number + 1;
    }
}
```

![image-20230513100817318](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230513100817318.png)



代码会报错，因为 `add()` 函数被声明为 `pure` 函数，这意味着它不能访问或修改合约状态变量，包括 `number` 变量。因此，尝试在 `add()` 函数中修改 `number` 变量会导致编译错误。要解决此错误，可以将 `pure` 关键字从 `add()` 函数声明中删除，这将允许函数访问和修改合约状态变量。





那`pure`函数能做些什么？举个例子，可以给函数传递一个参数 `_number`，然后让他返回 `_number+1`。

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract FunctionTypes{
    uint256 public number = 5;
    
    // 默认
    function add() external  {
        number = number + 1;
    }

    // pure
    function addPure(uint256 _number) external pure returns (uint256 new_number) {
        new_number = _number + 1;
    }
}
```

![image-20230513101147517](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230513101147517.png)





```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract FunctionTypes{
    uint256 public number = 5;
    
    // 默认
    function add() external view  {
        number = number + 1;
    }

    // pure
    function addPure(uint256 _number) external pure returns (uint256 new_number) {
        new_number = _number + 1;
    }
}
```

如果`add()`包含`view`，比如`function add() view external`，也会报错。因为`view`能读取，但不能够改写状态变量。

![image-20230513101333266](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230513101333266.png)





让他不改写`number`，而是返回一个新的变量。

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract FunctionTypes{
    uint256 public number = 5;
    
    // 默认
    function add() external  {
        number = number + 1;
    }

    // pure
    function addPure(uint256 _number) external pure returns (uint256 new_number) {
        new_number = _number + 1;
    }

    // view
    function addView() external view returns (uint256 new_number) {
        new_number = number + 1;
    }
}
```



![image-20230513101707309](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230513101707309.png)





#### internal and external



```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract FunctionTypes{
    uint256 public number = 5;
    
    // internal
    function minus() internal {
        number = number - 1;
    }
}
```

这段代码是一个名为`minus`的函数，其访问修饰符为`internal`，在Solidity中，`internal`关键字表示该函数只能在当前合约内部或者其派生合约内部被调用。这里的`minus`函数将`number`的值减1，由于该函数没有返回值，因此使用关键字`void`来表示。

另外需要注意的是，由于该函数是一个内部函数，因此不能被直接从合约的外部调用。如果需要从合约的外部调用该函数，可以在合约中添加一个公共函数，然后在公共函数中调用内部函数。



定义一个`internal`的`minus()`函数，每次调用使得`number`变量减1。由于是`internal`，只能由合约内部调用，而外部不能。因此，我们必须再定义一个`external`的`minusCall()`函数，来间接调用内部的`minus()`。

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract FunctionTypes{
    uint256 public number = 5;
    
    // internal
    function minus() internal {
        number = number - 1;
    }

    // external；合约内的函数可以调用内部函数
    function minusCall() external {
        minus();
    }
}
```

![image-20230513102632708](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230513102632708.png)



#### payable

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract FunctionTypes{

    // payable
    function queryBalance() external payable returns(uint256 balance){
        balance = address(this).balance;
    }
}
```

上面的代码定义了一个名为 `queryBalance` 的函数，它可以查询智能合约当前的ETH余额。具体来说，该函数使用了 `payable` 关键字，表示该函数可以接收以太币，同时使用 `returns` 关键字指定了返回类型为 `uint256`，表示查询到的余额。

函数内部使用了 `address(this).balance` 来获取智能合约的当前余额，并将其赋值给 `balance` 变量。最终，当函数执行完成后，该函数会返回智能合约当前的ETH余额。

在以太坊上，智能合约的余额可以被其他账户转入或转出，因此这个函数可以让其他人查询智能合约的余额，以便进行相应的交易或操作。

![image-20230513103758999](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230513103758999.png)



### 测试

![image-20230513104549462](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230513104549462.png)



> pure。因为既不查看合约中的状态变量也不修改合约中的状态变量



![image-20230513104640244](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230513104640244.png)



> 关键字 public 适合在下划线处填写，因为该函数可以被任何人调用，以查询某个地址的 USDT 余额。





## 04.函数输出 return



### 返回值 return和returns

`Solidity`有两个关键字与函数输出相关：`return`和`returns`，他们的区别在于：

- `returns`加在函数名后面，用于声明返回的变量类型及变量名；
- `return`用于函数主体中，返回指定的变量。

```solidity
// 返回多个变量
function returnMultiple() public pure returns(uint256, bool, uint256[3] memory){
	return(1, true, [uint256(1),2,5]);
}
```



上面这段代码中，我们声明了`returnMultiple()`函数将有多个输出：`returns(uint256, bool, uint256[3] memory)`，接着我们在函数主体中用`return(1, true, [uint256(1),2,5])`确定了返回值。



> 记住即可



### 命名式返回

我们可以在`returns`中标明返回变量的名称，这样`solidity`会自动给这些变量初始化，并且自动返回这些函数的值，不需要加`return`。

```solidity
// 命名式返回
function returnNamed() public pure returns(uint256 _number, bool _bool, uint256[3] memory _array){
    _number = 2;
    _bool = false; 
    _array = [uint256(3),2,1];
}
```

在上面的代码中，我们用`returns(uint256 _number, bool _bool, uint256[3] memory _array)`声明了返回变量类型以及变量名。这样，我们在主体中只需要给变量`_number`，`_bool`和`_array`赋值就可以自动返回了。

当然，你也可以在命名式返回中用`return`来返回变量：

```solidity
// 命名式返回，依然支持return
function returnNamed2() public pure returns(uint256 _number, bool _bool, uint256[3] memory _array){
	return(1, true, [uint256(1),2,5]);
}
```





> 记住即可





### 解构式赋值

`solidity`使用解构式赋值的规则，支持读取函数的全部或部分返回值。

 

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract Return{
    // 命名式返回
    function returnNamed() public pure returns (uint256 _number, bool _bool ,uint256[3] memory _array){
        return(1, true, [uint256(1),2,5]);
    }

    // 解构式赋值

    //solidity使用解构式赋值的规则，支持读取函数的全部或部分返回值。
    function readReturn() public pure{
        // 读取所有返回值
        uint256 _number;
        bool _bool;
        uint256[3] memory _array;
        (_number, _bool, _array) = returnNamed();

        // 读取部分返回值
        bool _bool2;
        (, _bool2, ) = returnNamed();
    }
}
```



### 测试

- 命名式返回，returns中有类型和变量名



## 05.变量数据存储和作用域



### Solidity中的引用类型

**引用类型(Reference Type)**：包括数组（`array`），结构体（`struct`）和映射（`mapping`），这类变量占空间大，赋值时候直接传递地址（类似指针）。



> - 占用内存大
> - 类似指针





### 数据位置

solidity数据存储位置有三类：`storage`，`memory`和`calldata`。不同存储位置的`gas`成本不同。`storage`类型的数据存在链上，类似计算机的硬盘，消耗`gas`多；`memory`和`calldata`类型的临时存在内存里，消耗`gas`少。大致用法：

1. `storage`：合约里的状态变量默认都是`storage`，存储在链上。
2. `memory`：函数里的参数和临时变量一般用`memory`，存储在内存中，不上链。
3. `calldata`：和`memory`类似，存储在内存中，不上链。与`memory`的不同点在于`calldata`变量不能修改（`immutable`），一般用于函数的参数。例子：

```solidity
function fCalldata(uint[] calldata _x) public pure returns(uint[] calldata){
    //参数为calldata数组，不能被修改
    // _x[0] = 0 //这样修改会报错
    return(_x);
}
```

这是一个公共函数，名为 `fCalldata`，它接受一个 `uint` 类型的动态数组 `_x` 作为参数，并且使用 `pure` 修饰符标记该函数不会修改合约状态，因此它可以免费调用。

此函数使用 `calldata` 关键字标记 `_x` 参数，表示该参数的数据存储在事务调用的输入数据中，而不是存储在合约状态中。这意味着 `_x` 数组不能被修改，因为所有传递给函数的参数都是只读的。

函数返回一个 `uint` 类型的动态数组，使用 `calldata` 关键字标记该返回值，表示该数组的数据将存储在事务调用的输出数据中。

在函数主体中，我们不能修改传递给函数的 `_x` 数组，否则会导致编译错误。最后，函数将未修改的 `_x` 数组作为返回值返回。



> calldata当做一个标记，被标记的变量就是不可变的。类似java中final



#### 数据位置和赋值规则

在不同存储类型相互赋值时候，有时会产生独立的副本（修改新变量不会影响原变量），有时会产生引用（修改新变量会影响原变量）。规则如下：

- 1.`storage`（合约的状态变量）赋值给本地`storage`（函数里的）时候，会创建引用，改变新变量会影响原变量。例子：

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract DataStorage{
    uint[] x = [1,2,3];

    function fStorage() public {
        // 声明一个storage标记的变量xStorage，指向x，修改xStorage也会影响到x
        uint[] storage xStorage = x;
        xStorage[0] = 100;
    }
}
```

![image-20230515114435391](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230515114435391.png)

上面的代码定义了一个状态变量 `x`，其为一个 `uint` 类型的数组，包含 `[1,2,3]` 这三个元素。函数 `fStorage()` 声明了一个 `storage` 变量 `xStorage`，其指向状态变量 `x`，因为 `storage` 变量和状态变量指向同一块区域，所以修改 `xStorage` 的值也会影响到状态变量 `x`。在函数中，`xStorage` 的第一个元素被修改为 100。由于 `xStorage` 和 `x` 指向同一块区域，所以状态变量 `x` 中的第一个元素也会被修改为 100。最后，函数没有返回任何值。



- 2.`storage`赋值给`memory`，会创建独立的复本，修改其中一个不会影响另一个；反之亦然。例子：

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract DataStorage{
    // 状态变量x
    uint[] x = [1,2,3];

    function fStorage() public {
        // 声明一个storage标记的变量xStorage，指向x，修改xStorage也会影响到x
        uint[] storage xStorage = x;
        xStorage[0] = 100;
    }

    function fMemory() public view{
        // //声明一个Memory的变量xMemory，复制x。修改xMemory不会影响x
        uint[] memory xMemory = x;
        xMemory[0] = 666;
        xMemory[1] = 777;
        xMemory[2] = 888;
    }
}
```



![image-20230515115144689](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230515115144689.png)

这段代码声明了一个函数 `fMemory()`，其可见性为 `public`，并且标注为 `view`，因此该函数不会修改合约的状态，也不会消耗任何 gas。

在函数内部，首先声明了一个内存变量 `xMemory`，并将数组 `x` 的值复制到了该变量中。需要注意的是，在 Solidity 中，数组类型默认为 storage 类型，即声明一个数组变量时，默认情况下会在存储器中分配空间来存储数组的元素，而非在内存中。因此，在声明一个内存变量并复制数组 `x` 的值时，需要使用 `memory` 关键字来显式地指定变量类型。

接下来，该函数修改了数组 `xMemory` 的前三个元素的值为 666、777 和 888。由于这些修改只发生在内存中，不会影响存储器中的数组 `x`，因此函数执行完后，数组 `x` 的值仍然为 [1, 2, 3]。



> 小疑问：fStorage 能不能也加`view`?
>
> 答：不能，因为添加 `view` 标记的函数是不允许修改状态变量的。直接报错，无法编译





- 3.`memory`赋值给`memory`，会创建引用，改变新变量会影响原变量。

- 4.其他情况，变量赋值给`storage`，会创建独立的复本，修改其中一个不会影响另一个。



### 变量的作用域

`Solidity`中变量按作用域划分有三种，分别是状态变量（state variable），局部变量（local variable）和全局变量(global variable)



#### 状态变量

状态变量是数据存储在链上的变量，所有合约内函数都可以访问 ，`gas`消耗高。状态变量在合约内、函数外声明：



```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract Variables{
    uint public x = 1;
    uint public y;
    string public z;

    function foo() external {
        // 可以在函数里更改状态变量的值
        x = 5;
        y = 2;
        z = "test~";
    }
}
```

在 Solidity 中，函数的可见性修饰符（visibility modifier）用于指定函数可以被哪些实体调用。`external` 是其中一种可见性修饰符，它表示函数只能被合约外部的地址调用，而不能在合约内部被调用。

在给定的代码中，函数 `foo()` 被标记为 `external`，这意味着该函数只能被合约的外部地址调用，而无法在合约内部调用。这种设置通常用于指定合约的接口函数，提供给其他合约或外部实体进行交互。

需要注意的是，`external` 函数不能在合约内部被直接调用，因此在合约内部无法使用 `this.foo()` 或者 `selfdestruct` 等方式调用函数 `foo()`。



![](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230515144213417.png)



#### 局部变量

局部变量是仅在函数执行过程中有效的变量，函数退出后，变量无效。局部变量的数据存储在内存里，不上链，`gas`低。局部变量在函数内声明：

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract Variables{

    function bar() external pure returns(uint) {
        uint xx = 1;
        uint yy = 3;
        uint zz = xx + yy;
        return(zz);
    }
}
```



#### 全局变量

全局变量是全局范围工作的变量，都是`solidity`预留关键字。他们可以在函数内不声明直接使用：

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract Variables{

    function global() external view returns(address, uint, bytes memory){
        address sender = msg.sender;
        uint blockNum = block.number;
        bytes memory data = msg.data;
        return(sender, blockNum, data);
    }
}
```







在上面例子里，我们使用了3个常用的全局变量：`msg.sender`, `block.number`和`msg.data`，他们分别代表请求发起地址，当前区块高度，和请求数据。下面是一些常用的全局变量，更完整的列表请看这个[链接](https://learnblockchain.cn/docs/solidity/units-and-global-variables.html#special-variables-and-functions)：

- `blockhash(uint blockNumber)`: (`bytes32`)给定区块的哈希值 – 只适用于256最近区块, 不包含当前区块。
- `block.coinbase`: (`address payable`) 当前区块矿工的地址
- `block.gaslimit`: (`uint`) 当前区块的gaslimit
- `block.number`: (`uint`) 当前区块的number
- `block.timestamp`: (`uint`) 当前区块的时间戳，为unix纪元以来的秒
- `gasleft()`: (`uint256`) 剩余 gas
- `msg.data`: (`bytes calldata`) 完整call data
- `msg.sender`: (`address payable`) 消息发送者 (当前 caller)
- `msg.sig`: (`bytes4`) calldata的前四个字节 (function identifier)
- `msg.value`: (`uint`) 当前交易发送的`wei`值



### 测试

![image-20230515150114128](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230515150114128.png)

为什么选第3个？

题目要求不同类型的引用变量相互赋值。。。





## 06.引用类型



### array

数组（`Array`）是`solidity`常用的一种变量类型，用来存储一组数据（整数，字节，地址等等）。数组分为固定长度数组和可变长度数组两种：



bytes比较特殊，是数组，但是不用加`[]`

**bytes array7;**





#### 创建数组的规则

在solidity里，创建数组有一些规则：

- 对于`memory`修饰的`动态数组`，可以用`new`操作符来创建，但是必须声明长度，并且声明后长度不能改变。例子：

```solidity
    // memory动态数组
    uint[] memory array8 = new uint[](5);
    bytes memory array9 = new bytes(9);
```



```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract Reference{

    function f() public pure {
        g([1, 2, 3]);
    }
    function g(uint[3] memory) public pure {
        // ...
    }
}
```

![image-20230515151358436](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230515151358436.png)

g函数要求的是`uint256`类型而传入的`[1,2,3]`是默认的`uint8`类型，因此要显式对第一个元素进行uint强转。



```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.4.16 <0.9.0;
contract C {
    function f() public pure {
        g([uint(1), 2, 3]);
    }
    function g(uint[3] memory) public pure {
        // ...
    }
}
```



- 如果创建的是动态数组，你需要一个一个元素的赋值。

```solidity
    uint[] memory x = new uint[](3);
    x[0] = 1;
    x[1] = 3;
    x[2] = 4;
```



#### 数组成员

- `length`: 数组有一个包含元素数量的`length`成员，`memory`数组的长度在创建后是固定的。
- `push()`: `动态数组`和`bytes`拥有`push()`成员，可以在数组最后添加一个`0`元素。
- `push(x)`: `动态数组`和`bytes`拥有`push(x)`成员，可以在数组最后添加一个`x`元素。
- `pop()`: `动态数组`和`bytes`拥有`pop()`成员，可以移除数组最后一个元素。



```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract Reference{
    uint[] array4;

    function arrayPush() public returns(uint[] memory){
        uint[2] memory a  = [uint(1),2];
        array4 = a;
        array4.push(3);

        return array4;
    }
}
```

![image-20230515152310219](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230515152310219.png)



### struct

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract Reference{
     // 结构体
    struct Student{
        uint256 id;
        uint256 score; 
    }

    // 初始一个student结构体
    Student student;

    //  给结构体赋值

    // 方法1:在函数中创建一个storage的struct引用
    function initStudent1() external{
        Student storage _student = student; // assign a copy of student
        _student.id = 11;
        _student.score = 100;
    }

    // 方法2:直接引用状态变量的struct
    function initStudent2() external{
        student.id = 1;
        student.score = 80;
    }

}
```



### 测试

![image-20230515153520619](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230515153520619.png)



执行 `initStudent` 方法后，`student.id` 的值为 `300`，`student.score` 的值为 `400`。

在 `initStudent` 方法中，首先给 `student` 的 `id` 和 `score` 分别赋值为 `100` 和 `200`。然后，声明一个指向 `student` 的存储引用 `_student`，并将其值设为 `student`。接下来，修改 `_student` 的 `id` 和 `score` 分别为 `300` 和 `400`。

由于 `_student` 是对 `student` 的存储引用，它们指向同一个存储位置。因此，对 `_student` 的修改也会反映在 `student` 上。所以，最终 `student.id` 的值为 `300`，`student.score` 的值为 `400`。



## 07.映射类型

