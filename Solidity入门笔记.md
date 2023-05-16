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

### 映射Mapping

在映射中，人们可以通过键（`Key`）来查询对应的值（`Value`），比如：通过一个人的`id`来查询他的钱包地址。

声明映射的格式为`mapping(_KeyType => _ValueType)`，其中`_KeyType`和`_ValueType`分别是`Key`和`Value`的变量类型。例子：

```solidity
    mapping(uint => address) public idToAddress; // id映射到地址
    mapping(address => address) public swapPair; // 币对的映射，地址到地址
```



> 跟Java的map一样



### 映射的规则

- **规则1**：映射的`_KeyType`只能选择`solidity`默认的类型，比如`uint`，`address`等，不能用自定义的结构体。而`_ValueType`可以使用自定义的类型。下面这个例子会报错，因为`_KeyType`使用了我们自定义的结构体：

```solidity
    // 我们定义一个结构体 Struct
    struct Student{
        uint256 id;
        uint256 score; 
    }
     mapping(Student => uint) public testVar;
```



- **规则2**：映射的存储位置必须是`storage`，因此可以用于合约的状态变量，函数中的`storage`变量，和library函数的参数（见[例子](https://github.com/ethereum/solidity/issues/4635)）。不能用于`public`函数的参数或返回结果中，因为`mapping`记录的是一种关系 (key - value pair)。
- **规则3**：如果映射声明为`public`，那么`solidity`会自动给你创建一个`getter`函数，可以通过`Key`来查询对应的`Value`。
- **规则4**：给映射新增的键值对的语法为`_Var[_Key] = _Value`，其中`_Var`是映射变量名，`_Key`和`_Value`对应新增的键值对。例子：

```solidity
    function writeMap (uint _Key, address _Value) public{
        idToAddress[_Key] = _Value;
    }
```



> 回头多多复习这部分内容



### 映射的原理

- **原理1**: 映射不储存任何键（`Key`）的资讯，也没有length的资讯。
- **原理2**: 映射使用`keccak256(key)`当成offset存取value。
- **原理3**: 因为Ethereum会定义所有未使用的空间为0，所以未赋值（`Value`）的键（`Key`）初始值都是0。



> 完全不理解。。。



下面是一个示例来说明映射的原理：

```solidity
pragma solidity ^0.8.0;

contract MappingExample {
    mapping(address => uint256) public balances;

    function updateBalance(uint256 newBalance) external {
        balances[msg.sender] = newBalance;
    }
}
```

在上述示例中，我们定义了一个名为 `balances` 的映射，将地址 (`address`) 映射到无符号整数 (`uint256`) 类型的余额。通过函数 `updateBalance`，我们可以更新映射中特定地址的余额。



假设我们执行以下操作：

```solidity
MappingExample contractInstance = new MappingExample();
contractInstance.updateBalance(100);
```

上述操作将更新 `balances` 映射中调用者的地址 (`msg.sender`) 对应的余额为 100。在映射中，`msg.sender` 将被哈希转换为对应的存储位置，然后将值 `100` 存储在该位置上。

之后，我们可以通过访问 `balances[msg.sender]` 来检索和使用该地址的余额值。

请注意，映射在 Solidity 中是一种非常常用的数据结构，用于存储和管理各种状态信息，如用户账户余额、数据索引等。它提供了高效的键值存储和检索功能，使得访问和更新数据变得简单和快速。



### 手写demo

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract Mapping{

    mapping(uint => address) public idToAddress;
    mapping(address => address) public  swapPair; 

    function writeMap(uint _key, address _value) public {
        idToAddress[_key] = _value;
    }
}
```





## 08.变量初始值

在`solidity`中，声明但没赋值的变量都有它的初始值或默认值。这一讲，我们将介绍常用变量的初始值。



### 值类型初始值

- `boolean`: `false`

- `string`: `""`

- `int`: `0`

- `uint`: `0`

- `enum`: 枚举中的第一个元素

- `address`: `0x0000000000000000000000000000000000000000` (或 `address(0)`)

- ```
  function
  ```

  - `internal`: 空白方程
  - `external`: 空白方程

可以用`public`变量的`getter`函数验证上面写的初始值是否正确：

```solidity
    bool public _bool; // false
    string public _string; // ""
    int public _int; // 0
    uint public _uint; // 0
    address public _address; // 0x0000000000000000000000000000000000000000

    enum ActionSet { Buy, Hold, Sell}
    ActionSet public _enum; // 第一个元素 0

    function fi() internal{} // internal空白方程 
    function fe() external{} // external空白方程 
```



> 有印象即可



### 引用类型初始值

- 映射`mapping`: 所有元素都为其默认值的`mapping`
- 结构体`struct`: 所有成员设为其默认值的结构体
- 数组`array`
  - 动态数组: `[]`
  - 静态数组（定长）: 所有成员设为其默认值的静态数组

可以用`public`变量的`getter`函数验证上面写的初始值是否正确：

```solidity
    // Reference Types
    uint[8] public _staticArray; // 所有成员设为其默认值的静态数组[0,0,0,0,0,0,0,0]
    uint[] public _dynamicArray; // `[]`
    mapping(uint => address) public _mapping; // 所有元素都为其默认值的mapping
    // 所有成员设为其默认值的结构体 0, 0
    struct Student{
        uint256 id;
        uint256 score; 
    }
    Student public student;
```



> 有印象即可





### delete操作符

`delete a`会让变量`a`的值变为初始值。

```solidity
    // delete操作符
    bool public _bool2 = true; 
    function d() external {
        delete _bool2; // delete 会让_bool2变为默认值，false
    }
```



### 测试

![image-20230516140707419](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230516140707419.png)

bytes1的初始值为0x00，是十进制数0的十六进制表示，这是因为bytes1是一种无符号整数，可以存储0到255之间的值，当bytes1变量被声明但未初始化时，默认会赋值为0x00。 其他选项不正确。0是整数，而不是bytes1。1大于可以存储在bytes1中的最大值。0x0不是有效的十六进制数。



为什么默认值不是0而是0x00？

在 Solidity 中，字节类型（例如 `bytes1`）的默认初始值是 0x00，而不是十进制的 0。这是因为 Solidity 在处理字节数据时，通常会使用十六进制表示法，其中每个字节由两个十六进制位表示。

使用十六进制的 `0x00` 来表示字节的默认初始值具有一些优势：

1. 一致性：字节数据在处理和表示时通常使用十六进制表示法，因此使用十六进制的 `0x00` 作为默认值能够保持一致性。
2. 易读性：对于熟悉十六进制表示法的开发者来说，`0x00` 更直观地表示一个字节所有位都为零。
3. 数据格式：十六进制的 `0x00` 表示一个字节的所有位都为零，符合数据格式的规范和约定。

虽然在某些编程语言中，字节类型的默认初始值可能是十进制的 0，但在 Solidity 中，采用十六进制的 `0x00` 作为字节类型的默认初始值更符合 Solidity 的设计和数据表示习惯。







![image-20230516141126261](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230516141126261.png)

调用函数 `d()` 后，将返回一个空字符串。

在 Solidity 中，字符串类型（`string`）是一个动态大小的数据类型，存储在内存中。当你执行 `delete` 操作时，它会将字符串的内容清空，使其成为空字符串。

因此，在函数 `d()` 中，当你执行 `delete _string;` 之后，`_string` 变量的值将变为空字符串。然后，你尝试返回 `_string`，它将返回一个空字符串作为结果。



![image-20230516140909156](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230516140909156.png)

在 Solidity 中，对于未显式赋值的映射（mapping）类型，其值将被初始化为对应值类型的默认值。

对于 `_balances` 这个映射类型，值类型是 `uint256`，它的默认值是 0。

因此，对于未记录的用户，它们在 `_balances` 中的值将是 0。





## 09.常数和不变量

这一讲，我们介绍`solidity`中两个关键字，`constant`（常量）和`immutable`（不变量）。状态变量声明这个两个关键字之后，不能在合约后更改数值；并且还可以节省`gas`。另外，只有数值变量可以声明`constant`和`immutable`；`string`和`bytes`可以声明为`constant`，但不能为`immutable`。



> - 数值变量可以声明constant和immutable
> - string和bytes仅constant





### constant and immutable

The following table summarizes the key differences between constant and immutable variables:

| Feature                          | Constant                                      | Immutable                                        |
| :------------------------------- | :-------------------------------------------- | :----------------------------------------------- |
| Can be assigned a value          | At the time of declaration                    | At the time of declaration or in the constructor |
| Can be changed after declaration | No                                            | No                                               |
| Typical use cases                | Storing values that are known at compile time | Storing values that should not be changed        |



`constant` 和 `immutable` 都是 Solidity 中用于声明不可修改的变量的关键字，它们有一些共同点和区别。

共同点：

1. 不可修改：无论是 `constant` 还是 `immutable` 声明的变量都是不可修改的，一旦赋值后就无法再修改其值。

区别：

1. 声明位置：`constant` 可以用于函数内部、函数参数和库中，而 `immutable` 只能用于合约级别。
2. 编译时计算：`constant` 声明的变量的值在编译时确定并被固定，而 `immutable` 声明的变量的值可以在部署合约时计算，因此 `immutable` 变量可以基于其他的不可变变量进行计算，而 `constant` 变量的值必须在编译时已知。
3. 存储位置：`constant` 变量的值在编译时被内联到字节码中，因此在执行时不会占用存储空间，而 `immutable` 变量的值需要在合约存储中分配空间并被存储。



>  需要注意的是，`constant` 关键字在 Solidity 0.6.0 版本中已被废弃，推荐使用 `immutable` 关键字来声明不可修改的变量。





## 10.控制流

### 控制流



- if-else
- for循环
- while
- do-while
- 三元运算符





> 为什么要用solidity写插入排序算法？





## 11.构造函数和修饰器



### 构造函数

构造函数（`constructor`）是一种特殊的函数，每个合约可以定义一个，并在部署合约的时候自动运行一次。它可以用来初始化合约的一些参数，例如初始化合约的`owner`地址：

```solidity
   address owner; // 定义owner变量

   // 构造函数
   constructor() {
      owner = msg.sender; // 在部署合约的时候，将owner设置为部署者的地址
   }
```



> 跟Java的构造函数一样。





### 修饰器

修饰器（`modifier`）是`solidity`特有的语法，类似于面向对象编程中的`decorator`，声明函数拥有的特性，并减少代码冗余。它就像钢铁侠的智能盔甲，穿上它的函数会带有某些特定的行为。`modifier`的主要使用场景是运行函数前的检查，例如地址，变量，余额等。







### demo

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract Owner{
    // 定义owner变量
    address public _owner;

    // 构造函数
    constructor(){
        // 在部署合约时，将owner设置为合约部署者
        _owner = msg.sender;
    }

    // 定义modifier
    modifier onlyOwner{
        /*
        要求只有合约的拥有者（_owner）可以执行被修饰的函数。
        如果当前调用者（msg.sender）不是合约的拥有者，则会抛出异常，导致函数执行中止。
        */
        require(msg.sender == _owner);

        //修饰器中的 _; 表示函数执行体，表示在权限验证通过后，继续执行被修饰的函数。
        _;
    }

   function changeOwner(address _newOwner) external onlyOwner{
      _owner = _newOwner; // 只有owner地址运行这个函数，并改变owner
   }

}
```

上述代码是一个名为 `Owner` 的合约，它具有以下功能：

1. 在合约部署时，通过构造函数将 `_owner` 设置为合约部署者的地址。这样，部署合约的账户就成为了合约的拥有者。
2. 定义了一个名为 `onlyOwner` 的修饰器（modifier），用于限制只有合约的拥有者才能执行被修饰的函数。
3. 在 `changeOwner` 函数中使用了 `onlyOwner` 修饰器。这意味着只有合约的拥有者才能调用 `changeOwner` 函数，并且通过该函数可以更改合约的拥有者地址。

通过上述代码，可以实现合约的拥有者管理机制。只有拥有者账户可以执行带有 `onlyOwner` 修饰器的函数，例如 `changeOwner` 函数，从而确保只有拥有者才能更改合约的拥有者。

合约的拥有者权限控制是一种常见的安全机制，可用于限制对敏感操作的访问，增加合约的安全性。



## 12.事件



### 概述

`Solidity`中的事件（`event`）是`EVM`上日志的抽象，它具有两个特点：

- 响应：应用程序（[`ether.js`](https://learnblockchain.cn/docs/ethers.js/api-contract.html#id18)）可以通过`RPC`接口订阅和监听这些事件，并在前端做响应。
- 经济：事件是`EVM`上比较经济的存储数据的方式，每个大概消耗2,000 `gas`；相比之下，链上存储一个新变量至少需要20,000 `gas`。





### demo

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract Event{
    // 定义_balances映射变量，记录每个地址的持币数量
    mapping(address => uint256) public _balances;

    // 定义Transfer event，记录transfer交易的转账地址，接收地址和转账数量
    event Transfer(address indexed from, address indexed to, uint256 value); 


    // 定义_transfer函数，执行转账逻辑
    function _transfer(address from,address to,uint256 amount) external {

        _balances[from] = 10000000; // 给转账地址一些初始代币
        _balances[from] -=  amount; // from地址减去转账数量
        _balances[to] += amount; // to地址加上转账数量

        // 释放事件
        emit Transfer(from, to, amount);
    }
}
```

以上代码是一个简单的 Solidity 合约，它实现了转账功能，并使用事件（event）来记录转账交易的相关信息。

代码主要包含以下部分：

1. 定义了一个名为 `_balances` 的映射变量，用于记录每个地址的持币数量。通过 `mapping(address => uint256)`，可以根据地址快速查找对应的持币数量。
2. 定义了一个名为 `Transfer` 的事件。该事件包含三个参数：`from`、`to`、`value`，分别表示转账发起地址、接收地址和转账数量。通过 `event Transfer(address indexed from, address indexed to, uint256 value)`，定义了事件的结构。
3. 定义了一个名为 `_transfer` 的函数，用于执行转账操作。该函数接受三个参数：`from`（转账发起地址）、`to`（接收地址）和 `amount`（转账数量）。在函数内部，首先给转账发起地址设置一些初始代币数量，然后更新转账发起地址和接收地址的持币数量。最后，通过 `emit Transfer(from, to, amount)` 语句触发 `Transfer` 事件，记录转账交易的信息。

该合约通过事件将转账交易的信息记录在区块链的日志中，方便其他用户和应用程序查询和分析转账操作。通过监听 `Transfer` 事件，外部应用程序可以实时获取转账交易的发生情况，并执行相应的操作，例如更新用户界面或触发其他合约函数。事件的使用提高了合约的可追溯性和与外部应用程序的交互性。



 `event Transfer(address indexed from, address indexed to, uint256 value); `

- `address indexed from` 是一个参数，它表示转移发起者的地址。`indexed` 关键字用于指定该参数可以被索引，以便在日志查询中进行快速搜索。
- `address indexed to` 是一个参数，它表示接收者的地址。同样，`indexed` 关键字用于指定该参数可以被索引。
- `uint256 value` 是一个参数，它表示转移的数值。





### 测试



![image-20230516161224372](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230516161224372.png)



![image-20230516161113284](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230516161113284.png)

不完全正确。在 Solidity 中，`indexed` 关键字只能用于修饰事件（event）的参数，并且只能修饰 `address` 或基本数据类型（`bool`、`uint`、`int` 等）的参数。

使用 `indexed` 关键字修饰事件参数可以使参数成为事件的索引参数，这意味着在以太坊区块链上存储事件日志时，可以更高效地按照该参数进行检索和过滤。

对于其他情况，如合约函数的参数或局部变量，不能使用 `indexed` 关键字进行修饰。`indexed` 关键字只在事件定义中具有特殊意义。





## 13.继承

继承是面向对象编程很重要的组成部分，可以显著减少重复代码。如果把合约看作是对象的话，`solidity`也是面向对象的编程，也支持继承。



### 规则

- `virtual`: 父合约中的函数，如果希望子合约重写，需要加上`virtual`关键字。
- `override`：子合约重写了父合约中的函数，需要加上`override`关键字。



> 继承
>
> - 父-virtual
> - 子-virtual + override





### 简单继承

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract Yeye{
    event Log(string msg);

    // 定义3个function: hip(), pop(), man()，Log值为Yeye。
    function hip() public virtual{
        emit Log("Yeye");
    }

    function pop() public virtual{
        emit Log("Yeye");
    }

    function yeye() public virtual {
        emit Log("Yeye");
    }
}


contract Baba is Yeye{
    // 继承两个function: hip()和pop()，输出改为Baba。
    function hip() public virtual override{
        emit Log("Baba");
    }

    function pop() public virtual override{
        emit Log("Baba");
    }

    function baba() public virtual{
        emit Log("Baba");
    }
}
```

![image-20230516162513233](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230516162513233.png)

上述代码定义了两个合约 `Yeye` 和 `Baba`，其中 `Baba` 继承自 `Yeye`。

`Yeye` 合约中定义了一个事件 `Log`，用于记录日志消息。

`Yeye` 合约中还定义了三个函数：`hip()`、`pop()` 和 `yeye()`，它们都触发 `Log` 事件，并输出 "Yeye"。

`Baba` 合约继承自 `Yeye`，并覆盖了两个函数 `hip()` 和 `pop()`。在 `Baba` 合约中，这两个函数被重写为输出 "Baba"，同时保留了继承自 `Yeye` 的功能。

此外，`Baba` 合约还定义了一个新的函数 `baba()`，它触发 `Log` 事件，并输出 "Baba"。

综上所述，通过合约的继承和函数重写，`Baba` 合约在 `Yeye` 合约的基础上添加了自己的功能，并修改了部分函数的输出。



### 多继承

`solidity`的合约可以继承多个合约。规则：

继承时要按辈分最高到最低的顺序排。比如我们写一个`Erzi`合约，继承`Yeye`合约和`Baba`合约，那么就要写成`contract Erzi is Yeye, Baba`，而不能写成`contract Erzi is Baba, Yeye`，不然就会报错。 如果某一个函数在多个继承的合约里都存在，比如例子中的`hip()`和`pop()`，在子合约里必须重写，不然会报错。 重写在多个父合约中都重名的函数时，`override`关键字后面要加上所有父合约名字，例如`override(Yeye, Baba)`。 例子：

```solidity
contract Erzi is Yeye, Baba{
    // 继承两个function: hip()和pop()，输出值为Erzi。
    function hip() public virtual override(Yeye, Baba){
        emit Log("Erzi");
    }

    function pop() public virtual override(Yeye, Baba) {
        emit Log("Erzi");
    }
```



我们可以看到，`Erzi`合约里面重写了`hip()`和`pop()`两个函数，将输出改为`”Erzi”`，并且还分别从`Yeye`和`Baba`合约继承了`yeye()`和`baba()`两个函数。



### 修饰器的继承

`Solidity`中的修饰器（`Modifier`）同样可以继承，用法与函数继承类似，在相应的地方加`virtual`和`override`关键字即可。

```solidity
contract Base1 {
    modifier exactDividedBy2And3(uint _a) virtual {
        require(_a % 2 == 0 && _a % 3 == 0);
        _;
    }
}

contract Identifier is Base1 {

    //计算一个数分别被2除和被3除的值，但是传入的参数必须是2和3的倍数
    function getExactDividedBy2And3(uint _dividend) public exactDividedBy2And3(_dividend) pure returns(uint, uint) {
        return getExactDividedBy2And3WithoutModifier(_dividend);
    }

    //计算一个数分别被2除和被3除的值
    function getExactDividedBy2And3WithoutModifier(uint _dividend) public pure returns(uint, uint){
        uint div2 = _dividend / 2;
        uint div3 = _dividend / 3;
        return (div2, div3);
    }
}
```



`Identifier`合约可以直接在代码中使用父合约中的`exactDividedBy2And3`修饰器，也可以利用`override`关键字重写修饰器：

```solidity
    modifier exactDividedBy2And3(uint _a) override {
        _;
        require(_a % 2 == 0 && _a % 3 == 0);
    }
```



### 构造函数的继承

构造函数在继承中的行为略有不同。让我们通过一个示例来说明。

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

contract A {
    string public name;

    constructor(string memory _name) {
        name = _name;
    }
}

contract B is A {
    uint public age;

    constructor(string memory _name, uint _age) A(_name) {
        age = _age;
    }
}

contract C is B {
    address public addressC;

    constructor(string memory _name, uint _age, address _addressC) B(_name, _age) {
        addressC = _addressC;
    }
}
```

在上面的示例中，我们有三个合约：`A`、`B` 和 `C`。合约 `B` 继承了合约 `A`，合约 `C` 继承了合约 `B`。

在构造函数中，我们通过调用父合约的构造函数来初始化继承的状态变量。在合约 `B` 中，我们调用了 `A` 的构造函数，并传递了 `_name` 参数。在合约 `C` 中，我们调用了 `B` 的构造函数，并传递了 `_name` 和 `_age` 参数。

这样，当我们部署合约 `C` 时，会依次执行 `A`、`B` 和 `C` 的构造函数。构造函数的参数会按照继承链的顺序依次传递。

例如，如果我们部署合约 `C` 并传入参数 "Alice"、25 和 `0x1234567890ABCDEF`，那么合约 `C` 的构造函数会调用合约 `B` 的构造函数，并传递 "Alice" 和 25 作为参数。然后合约 `B` 的构造函数又会调用合约 `A` 的构造函数，并传递 "Alice" 作为参数。最后，合约 `C` 的构造函数会初始化自身的状态变量 `addressC`。

继承中的构造函数调用顺序遵循从最基类到最派生类的顺序，确保所有继承链上的构造函数都被正确调用并初始化。



### 调用父合约的函数

子合约有两种方式调用父合约的函数，直接调用和利用`super`关键字。

1. 直接调用：子合约可以直接用`父合约名.函数名()`的方式来调用父合约函数，例如`Yeye.pop()`。

```solidity
    function callParent() public{
        Yeye.pop();
    }
```



1. `super`关键字：子合约可以利用`super.函数名()`来调用最近的父合约函数。`solidity`继承关系按声明时从右到左的顺序是：`contract Erzi is Yeye, Baba`，那么`Baba`是最近的父合约，`super.pop()`将调用`Baba.pop()`而不是`Yeye.pop()`：

```solidity
    function callParentSuper() public{
        // 将调用最近的父合约函数，Baba.pop()
        super.pop();
    }
```



## 14.抽象合约和接口



### 抽象合约

如果一个智能合约里至少有一个未实现的函数，即某个函数缺少主体`{}`中的内容，则必须将该合约标为`abstract`，不然编译会报错；另外，未实现的函数需要加`virtual`，以便子合约重写。拿我们之前的[插入排序合约](https://github.com/AmazingAng/WTFSolidity/tree/main/07_InsertionSort)为例，如果我们还没想好具体怎么实现插入排序函数，那么可以把合约标为`abstract`，之后让别人补写上。

```solidity
abstract contract InsertionSort{
    function insertionSort(uint[] memory a) public pure virtual returns(uint[] memory);
}
```



> 就是Java中的抽象类



### 接口

接口类似于抽象合约，但它不实现任何功能。接口的规则：

1. 不能包含状态变量
2. 不能包含构造函数
3. 不能继承除接口外的其他合约
4. 所有函数都必须是external且不能有函数体
5. 继承接口的合约必须实现接口定义的所有功能

虽然接口不实现任何功能，但它非常重要。接口是智能合约的骨架，定义了合约的功能以及如何触发它们：如果智能合约实现了某种接口（比如`ERC20`或`ERC721`），其他Dapps和智能合约就知道如何与它交互。因为接口提供了两个重要的信息：

1. 合约里每个函数的`bytes4`选择器，以及基于它们的函数签名`函数名(每个参数类型）`。
2. 接口id（更多信息见[EIP165](https://eips.ethereum.org/EIPS/eip-165)）



> 跟Java中的接口差不多



另外，接口与合约`ABI`（Application Binary Interface）等价，可以相互转换：编译接口可以得到合约的`ABI`，利用[abi-to-sol工具](https://gnidan.github.io/abi-to-sol/)也可以将`ABI json`文件转换为`接口sol`文件。

我们以`ERC721`接口合约`IERC721`为例，它定义了3个`event`和9个`function`，所有`ERC721`标准的NFT都实现了这些函数。我们可以看到，接口和常规合约的区别在于每个函数都以`;`代替函数体`{ }`结尾。

```solidity
interface IERC721 is IERC165 {
    event Transfer(address indexed from, address indexed to, uint256 indexed tokenId);
    event Approval(address indexed owner, address indexed approved, uint256 indexed tokenId);
    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
    
    function balanceOf(address owner) external view returns (uint256 balance);

    function ownerOf(uint256 tokenId) external view returns (address owner);

    function safeTransferFrom(address from, address to, uint256 tokenId) external;

    function transferFrom(address from, address to, uint256 tokenId) external;

    function approve(address to, uint256 tokenId) external;

    function getApproved(uint256 tokenId) external view returns (address operator);

    function setApprovalForAll(address operator, bool _approved) external;

    function isApprovedForAll(address owner, address operator) external view returns (bool);

    function safeTransferFrom( address from, address to, uint256 tokenId, bytes calldata data) external;
}
```



**IERC721事件**

`IERC721`包含3个事件，其中`Transfer`和`Approval`事件在`ERC20`中也有。

- `Transfer`事件：在转账时被释放，记录代币的发出地址`from`，接收地址`to`和`tokenid`。
- `Approval`事件：在授权时释放，记录授权地址`owner`，被授权地址`approved`和`tokenid`。
- `ApprovalForAll`事件：在批量授权时释放，记录批量授权的发出地址`owner`，被授权地址`operator`和授权与否的`approved`。



**IERC721函数**

- `balanceOf`：返回某地址的NFT持有量`balance`。
- `ownerOf`：返回某`tokenId`的主人`owner`。
- `transferFrom`：普通转账，参数为转出地址`from`，接收地址`to`和`tokenId`。
- `safeTransferFrom`：安全转账（如果接收方是合约地址，会要求实现`ERC721Receiver`接口）。参数为转出地址`from`，接收地址`to`和`tokenId`。
- `approve`：授权另一个地址使用你的NFT。参数为被授权地址`approve`和`tokenId`。
- `getApproved`：查询`tokenId`被批准给了哪个地址。
- `setApprovalForAll`：将自己持有的该系列NFT批量授权给某个地址`operator`。
- `isApprovedForAll`：查询某地址的NFT是否批量授权给了另一个`operator`地址。
- `safeTransferFrom`：安全转账的重载函数，参数里面包含了`data`。





**什么时候使用接口？**

如果我们知道一个合约实现了`IERC721`接口，我们不需要知道它具体代码实现，就可以与它交互。

无聊猿`BAYC`属于`ERC721`代币，实现了`IERC721`接口的功能。我们不需要知道它的源代码，只需知道它的合约地址，用`IERC721`接口就可以与它交互，比如用`balanceOf()`来查询某个地址的`BAYC`余额，用`safeTransferFrom()`来转账`BAYC`。



```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract interactBAYC{
    // 利用BAYC地址创建接口合约变量（ETH主网）
    IERC721 BAYC = IERC721(0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D);

    // 通过接口调用BAYC的balanceOf()查询持仓量
    function balanceOfBAYC(address owner) external view returns (uint256 balance){
        return BAYC.balanceOf(owner);
    }

    // 通过接口调用BAYC的safeTransferFrom()安全转账
    function safeTransferFromBAYC(address from, address to, uint256 tokenId) external{
        BAYC.safeTransferFrom(from, to, tokenId);
    }
}
```





### 测试

![image-20230516170001861](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230516170001861.png)

被标记为`abstract`的合约不能被部署。

一个合约如果被标记为`abstract`，意味着它是一个抽象合约，其中包含未实现的函数或接口。抽象合约提供了一个接口或基类，用于定义其他合约的行为和结构，但它本身不能被实例化或部署。

如果一个合约被标记为`abstract`，那么它通常作为一个基类或接口，用于派生其他合约，并要求子合约实现其中的未实现函数或接口。只有实现了所有未实现函数的子合约才能被部署和实例化。

因此，被标记为`abstract`的合约本身不能被部署，它的目的是为其他合约提供一种约定或接口。





![image-20230516170802366](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230516170802366.png)

在这个代码中，我们定义了一个名为`approveAzuki`的函数，它接受两个参数`to`和`id`，并且声明为`external`，表示可以从外部进行调用。在函数体内，我们调用了`Azuki`合约的`approve`函数，并将`to`和`id`作为参数传递给它。注意，这里没有使用`view`和`returns`修饰符，因为`approve`函数没有返回值。

请注意，以上代码是一个示例，具体的实现可能需要根据实际情况进行适当的调整。



为什么不加`view`？

`view`修饰符表示函数只读取状态而不修改它。在你描述的情况下，调用`approve`函数会修改合约的状态，因为它会将某个NFT的许可权授予另一个地址。因此，你不应该在`approveAzuki`函数上使用`view`修饰符，而应该将其声明为普通的`external`函数。

如果你在`approveAzuki`函数上加上`view`修饰符，编译器将会报错，因为这样的声明与函数实际的行为不一致。





## 15.异常

`solidity`三种抛出异常的方法：`error`，`require`和`assert`，并比较三种方法的`gas`消耗。



### error

`error`是`solidity 0.8版本`新加的内容，方便且高效（省`gas`）地向用户解释操作失败的原因。人们可以在`contract`之外定义异常。下面，我们定义一个`TransferNotOwner`异常，当用户不是代币`owner`的时候尝试转账，会抛出错误：

```solidity
error TransferNotOwner(); // 自定义error
```



在执行当中，`error`必须搭配`revert`（回退）命令使用。

```solidity
    function transferOwner1(uint256 tokenId, address newOwner) public {
        if(_owners[tokenId] != msg.sender){
            revert TransferNotOwner();
        }
        _owners[tokenId] = newOwner;
    }
```



我们定义了一个`transferOwner1()`函数，它会检查代币的`owner`是不是发起人，如果不是，就会抛出`TransferNotOwner`异常；如果是的话，就会转账。



### Require

`require`命令是`solidity 0.8版本`之前抛出异常的常用方法，目前很多主流合约仍然还在使用它。它很好用，唯一的缺点就是`gas`随着描述异常的字符串长度增加，比`error`命令要高。使用方法：`require(检查条件，"异常的描述")`，当检查条件不成立的时候，就会抛出异常。

我们用`require`命令重写一下上面的`transferOwner`函数：

```solidity
    function transferOwner2(uint256 tokenId, address newOwner) public {
        require(_owners[tokenId] == msg.sender, "Transfer Not Owner");
        _owners[tokenId] = newOwner;
    }
```



### Assert

`assert`命令一般用于程序员写程序`debug`，因为它不能解释抛出异常的原因（比`require`少个字符串）。它的用法很简单，`assert(检查条件）`，当检查条件不成立的时候，就会抛出异常。

我们用`assert`命令重写一下上面的`transferOwner`函数：

```solidity
    function transferOwner3(uint256 tokenId, address newOwner) public {
        assert(_owners[tokenId] == msg.sender);
        _owners[tokenId] = newOwner;
    }
```





### 三种方法的gas比较

我们比较一下三种抛出异常的`gas`消耗，通过remix控制台的Debug按钮，能查到每次函数调用的`gas`消耗分别如下：

1. **`error`方法`gas`消耗**：24445
2. **`require`方法`gas`消耗**：24743
3. **`assert`方法`gas`消耗**：24446

我们可以看到，`error`方法`gas`最少，其次是`assert`，`require`方法消耗`gas`最多！因此，`error`既可以告知用户抛出异常的原因，又能省`gas`，大家要多用！（注意，由于部署测试时间的不同，每个函数的`gas`消耗会有所不同，但是比较结果会是一致的。）



### 总结

这一讲，我们介绍`solidity`三种抛出异常的方法：`error`，`require`和`assert`，并比较了三种方法的`gas`消耗。结论：`error`既可以告知用户抛出异常的原因，又能省`gas`。
