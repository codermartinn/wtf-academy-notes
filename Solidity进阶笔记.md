

# 16.函数重载

## 重载

`solidity`中允许函数进行重载（`overloading`），即名字相同但输入参数类型不同的函数可以同时存在，他们被视为不同的函数。注意，`solidity`不允许修饰器（`modifier`）重载。



> 就是同一个函数名，不同入参



### 函数重载

举个例子，我们可以定义两个都叫`saySomething()`的函数，一个没有任何参数，输出`"Nothing"`；另一个接收一个`string`参数，输出这个`string`。

```solidity
function saySomething() public pure returns(string memory){
    return("Nothing");
}

function saySomething(string memory something) public pure returns(string memory){
    return(something);
}
```

最终重载函数在经过编译器编译后，由于不同的参数类型，都变成了不同的函数选择器（selector）。



### 实参匹配（Argument Matching）

在调用重载函数时，会把输入的实际参数和函数参数的变量类型做匹配。 如果出现多个匹配的重载函数，则会报错。下面这个例子有两个叫`f()`的函数，一个参数为`uint8`，另一个为`uint256`：

```solidity
    function f(uint8 _in) public pure returns (uint8 out) {
        out = _in;
    }

    function f(uint256 _in) public pure returns (uint256 out) {
        out = _in;
    }
```

我们调用`f(50)`，因为`50`既可以被转换为`uint8`，也可以被转换为`uint256`，因此会报错。



> 入参在两个范围内，都多个函数匹配是不行的



## 总结

这一讲，我们介绍了`solidity`中函数重载的基本用法：名字相同但输入参数类型不同的函数可以同时存在，他们被视为不同的函数。





# 17.库合约



## 库函数

库函数是一种特殊的合约，为了提升`solidity`代码的复用性和减少`gas`而存在。库合约一般都是一些好用的函数合集（`库函数`），由大神或者项目方创作，咱们站在巨人的肩膀上，会用就行了。



他和普通合约主要有以下几点不同：

1. 不能存在状态变量
2. 不能够继承或被继承
3. 不能接收以太币
4. 不可以被销毁



> 就跟Java提供的lib一样



## String库合约

`String库合约`是将`uint256`类型转换为相应的`string`类型的代码库，样例代码如下：



### 如何使用库合约

我们用String库函数的toHexString()来演示两种使用库合约中函数的办法。



**1. 利用using for指令**

指令`using A for B;`可用于附加库函数（从库 A）到任何类型（B）。添加完指令后，库`A`中的函数会自动添加为`B`类型变量的成员，可以直接调用。注意：在调用的时候，这个变量会被当作第一个参数传递给函数：

```solidity
    // 利用using for指令
    using Strings for uint256;
    function getString1(uint256 _number) public pure returns(string memory){
        // 库函数会自动添加为uint256型变量的成员
        return _number.toHexString();
    }
```



> 给原来的类型添加了功能



**2. 通过库合约名称调用库函数**

```solidity
    // 直接通过库合约名调用
    function getString2(uint256 _number) public pure returns(string memory){
        return Strings.toHexString(_number);
    }
```



> Java熟悉的方式





# 18.Import

`solidity`支持利用`import`关键字导入其他源代码中的合约，让开发更加模块化。



## `import`用法

- 通过源文件相对位置导入，例子：

```text
文件结构
├── Import.sol
└── Yeye.sol

// 通过文件相对位置import
import './Yeye.sol';
```



- 通过源文件网址导入网上的合约，例子：

```text
// 通过网址引用
import 'https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/Address.sol';
```



- 通过`npm`的目录导入，例子：

```solidity
import '@openzeppelin/contracts/access/Ownable.sol';
```



- 通过`全局符号`导入特定的合约，例子：

```solidity
import {Yeye} from './Yeye.sol';
```



- 引用(`import`)在代码中的位置为：在声明版本号之后，在其余代码之前。





## 测试

![image-20230517114516937](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230517114516937.png)

Solidity中的`import`语句的作用是导入其他合约中的全局符号。

具体来说，通过`import`语句，可以将其他合约中定义的合约、结构体、枚举、库、接口等全局符号导入到当前合约中，以便在当前合约中使用这些符号。



什么是全局符号？

在Solidity中，全局符号是指在合约中定义的可以在整个合约范围内被访问的标识符，包括合约、结构体、枚举、库、接口等。

具体来说，全局符号是在合约的顶层范围内定义的标识符，可以被合约中的其他函数、结构体、事件等元素访问和使用。这些全局符号可以在合约中的任何地方被引用，提供了代码组织和模块化的能力。

举例来说，以下是几种常见的全局符号：

- 合约：在合约中使用`contract`关键字定义的代码单元。
- 结构体：使用`struct`关键字定义的自定义数据结构。
- 枚举：使用`enum`关键字定义的一组命名值。
- 函数：在合约中使用`function`关键字定义的可执行代码块。
- 事件：使用`event`关键字定义的用于记录和发布信息的通知机制。
- 接口：使用`interface`关键字定义的抽象合约，描述了其他合约应该实现的函数接口。

这些全局符号的定义范围通常是整个合约，因此可以在合约的任何地方进行访问和使用。









![image-20230517114135841](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230517114135841.png)

正确答案是：以上都可以。

通过使用`import`关键字导入文件时，可以选择性地指定要导入的全局符号，包括合约、纯函数和结构体类型等。这使得在导入文件时可以只选择性地引入所需的符号，而不必导入整个文件的所有内容。





![image-20230517115130780](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230517115130780.png)





# 19.接收ETH receive和fallback

 

`Solidity`支持两种特殊的回调函数，`receive()`和`fallback()`，他们主要在两种情况下被使用：

1. 接收ETH
2. 处理合约中不存在的函数调用（代理合约proxy contract）



## 接收ETH函数 receive

`receive()`只用于处理接收`ETH`。一个合约最多有一个`receive()`函数，声明方式与一般函数不一样，不需要`function`关键字：`receive() external payable { ... }`。`receive()`函数不能有任何的参数，不能返回任何值，必须包含`external`和`payable`。

当合约接收ETH的时候，`receive()`会被触发。`receive()`最好不要执行太多的逻辑因为如果别人用`send`和`transfer`方法发送`ETH`的话，`gas`会限制在`2300`，`receive()`太复杂可能会触发`Out of Gas`报错；如果用`call`就可以自定义`gas`执行更复杂的逻辑（这三种发送ETH的方法我们之后会讲到）。

我们可以在`receive()`里发送一个`event`，例如：

```solidity
    // 定义事件
    event Received(address Sender, uint Value);
    // 接收ETH时释放Received事件
    receive() external payable {
        emit Received(msg.sender, msg.value);
    }
```



有些恶意合约，会在`receive()` 函数（老版本的话，就是 `fallback()` 函数）嵌入恶意消耗`gas`的内容或者使得执行故意失败的代码，导致一些包含退款和转账逻辑的合约不能正常工作，因此写包含退款等逻辑的合约时候，一定要注意这种情况。

## 回退函数 fallback

`fallback()`函数会在调用合约不存在的函数时被触发。可用于接收ETH，也可以用于代理合约`proxy contract`。`fallback()`声明时不需要`function`关键字，必须由`external`修饰，一般也会用`payable`修饰，用于接收ETH:`fallback() external payable { ... }`。

我们定义一个`fallback()`函数，被触发时候会释放`fallbackCalled`事件，并输出`msg.sender`，`msg.value`和`msg.data`:

```solidity
    // fallback
    fallback() external payable{
        emit fallbackCalled(msg.sender, msg.value, msg.data);
    }
```



## receive和fallback的区别

`receive`和`fallback`都能够用于接收`ETH`，他们触发的规则如下：

```text
触发fallback() 还是 receive()?
           接收ETH
              |
         msg.data是空？
            /  \
          是    否
          /      \
receive()存在?   fallback()
        / \
       是  否
      /     \
receive()   fallback()
```



简单来说，合约接收`ETH`时，`msg.data`为空且存在`receive()`时，会触发`receive()`；`msg.data`不为空或不存在`receive()`时，会触发`fallback()`，此时`fallback()`必须为`payable`。

`receive()`和`payable fallback()`均不存在的时候，向合约**直接**发送`ETH`将会报错（你仍可以通过带有`payable`的函数向合约发送`ETH`）。



> - 是否有msg.data
>   - 有——> fallback()
>   - 无
>     - receive()函数存不存在？
>       - 不存在——>fallback()
>       - 存在——>receive()





## 测试

![image-20230517144816911](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230517144816911.png)

![image-20230517144826659](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230517144826659.png)

![image-20230517144840532](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230517144840532.png)



![image-20230517144848427](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230517144848427.png)





# 20. 发送ETH

`Solidity`有三种方法向其他合约发送`ETH`，他们是：`transfer()`，`send()`和`call()`，其中`call()`是被鼓励的用法。



## 接收ETH合约

我们先部署一个接收`ETH`合约`ReceiveETH`。`ReceiveETH`合约里有一个事件`Log`，记录收到的`ETH`数量和`gas`剩余。还有两个函数，一个是`receive()`函数，收到`ETH`被触发，并发送`Log`事件；另一个是查询合约`ETH`余额的`getBalance()`函数。



```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract ReceiveETH {
    // 收到ETH事件，记录amount和gas
    event Log(uint amount, uint gas);

    // recive()方法，接收ETH时触发
    receive() external payable {
        emit Log(msg.value, gasleft());
    }

    // 返回合约ETH余额
    function getBalance() view public returns(uint){
        return address(this).balance;
    }
}
```



## 发送ETH合约

我们将实现三种方法向`ReceiveETH`合约发送`ETH`。首先，先在发送ETH合约`SendETH`中实现`payable`的`构造函数`和`receive()`，让我们能够在部署时和部署后向合约转账。





### call

- 用法是`接收方地址.call{value: 发送ETH数额}("")`。
- `call()`没有`gas`限制，可以支持对方合约`fallback()`或`receive()`函数实现复杂逻辑。
- `call()`如果转账失败，不会`revert`。
- `call()`的返回值是`(bool, data)`，其中`bool`代表着转账成功或失败，需要额外代码处理一下。



```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

contract ReceiveETH {
    // 收到ETH事件，记录amount和gas
    event Log(uint amount, uint gas);

    // recive()方法，接收ETH时触发
    receive() external payable {
        emit Log(msg.value, gasleft());
    }

    // 返回合约ETH余额
    function getBalance() view public returns(uint){
        return address(this).balance;
    }
}

error CallFailed();

contract SendETH {
    // 构造函数，payable使得部署的时候可以转eth进去
    constructor() payable{}

    // receive方法，接收eth时被触发
    receive() external payable{}

    // call()发送ETH
    function callETH(address payable _to, uint256 amount) external payable{
    
        // 处理下call的返回值，如果失败，revert交易并发送error
        (bool success,) = _to.call{value:amount}("");

        // 失败
        if(!success){
            revert CallFailed();
        }
    }
}
```



### transfer

- 用法是`接收方地址.transfer(发送ETH数额)`。
- `transfer()`的`gas`限制是`2300`，足够用于转账，但对方合约的`fallback()`或`receive()`函数不能实现太复杂的逻辑。
- `transfer()`如果转账失败，会自动`revert`（回滚交易）。

代码样例，注意里面的`_to`填`ReceiveETH`合约的地址，`amount`是`ETH`转账金额：



```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

contract ReceiveETH {
    // 收到ETH事件，记录amount和gas
    event Log(uint amount, uint gas);

    // recive()方法，接收ETH时触发
    receive() external payable {
        emit Log(msg.value, gasleft());
    }

    // 返回合约ETH余额
    function getBalance() view public returns(uint){
        return address(this).balance;
    }
}



contract SendETH {
    // 构造函数，payable使得部署的时候可以转eth进去
    constructor() payable{}

    // receive方法，接收eth时被触发
    receive() external payable{}

    // 用transfer()发送ETH
    function transferETH(address payable _to, uint256 amount) external payable{
        _to.transfer(amount);
    }
}
```



### send

- 用法是`接收方地址.send(发送ETH数额)`。
- `send()`的`gas`限制是`2300`，足够用于转账，但对方合约的`fallback()`或`receive()`函数不能实现太复杂的逻辑。
- `send()`如果转账失败，不会`revert`。
- `send()`的返回值是`bool`，代表着转账成功或失败，需要额外代码处理一下。



```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

contract ReceiveETH {
    // 收到ETH事件，记录amount和gas
    event Log(uint amount, uint gas);

    // recive()方法，接收ETH时触发
    receive() external payable {
        emit Log(msg.value, gasleft());
    }

    // 返回合约ETH余额
    function getBalance() view public returns(uint){
        return address(this).balance;
    }
}

error SendFailed();

contract SendETH {
    // 构造函数，payable使得部署的时候可以转eth进去
    constructor() payable{}

    // receive方法，接收eth时被触发
    receive() external payable{}

    // send()发送ETH
    function sendETH(address payable _to, uint256 amount) external payable{
        // 处理下send的返回值，如果失败，revert交易并发送error
        bool success = _to.send(amount);
        if(!success){
            revert SendFailed();
        }
    }
}
```



## 总结

这一讲，我们介绍`solidity`三种发送`ETH`的方法：`transfer`，`send`和`call`。

- `call`没有`gas`限制，最为灵活，是最提倡的方法；
- `transfer`有`2300 gas`限制，但是发送失败会自动`revert`交易，是次优选择；
- `send`有`2300 gas`限制，而且发送失败不会自动`revert`交易，几乎没有人用它。



# 21. 调用其他合约

## 调用已部署合约
开发者写智能合约来调用其他合约，这让以太坊网络上的程序可以复用，从而建立繁荣的生态。很多web3项目依赖于调用其他合约，比如收益农场（yield farming）。这一讲，我们介绍如何在已知合约代码（或接口）和地址情况下调用目标合约的函数。



## 目标合约

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

interface IOtherContract {
    function getBalance() external returns(uint);
    function setX(uint256 x) external  payable;
    function getX() external  view returns(uint x);
}

contract OtherContract is IOtherContract{
    uint256 private _x = 0;
    event Log(uint amount, uint gas);

    function getBalance() external  view override returns(uint){
        return address(this).balance;
    }

    function setX(uint256 x) external override  payable {
        _x = x;
        if(msg.value > 0){
            emit Log(msg.value, gasleft());
        }
    }

    function getX() external  view override  returns(uint x){
        x = _x;
    }
}
```

上面的代码定义了一个Solidity合约，其中包括一个接口（`IOtherContract`）和一个实现该接口的合约（`OtherContract`）。

1. `IOtherContract` 接口定义了三个函数：
   - `getBalance()`：用于获取合约的余额。
   - `setX(uint256 x)`：用于设置一个整数值 `x`，可以接收以太币。
   - `getX()`：用于获取之前设置的整数值 `x`。
2. `OtherContract` 合约实现了 `IOtherContract` 接口，并提供了对应的函数实现。
   - `getBalance()` 函数是对接口函数的实现，用于返回合约的当前余额。
   - `setX(uint256 x)` 函数是对接口函数的实现，用于设置一个整数值 `x`。如果函数被调用时有附带以太币，它会触发一个日志事件 `Log`，记录附带的以太币数量和当前的剩余燃气量。
   - `getX()` 函数是对接口函数的实现，用于返回之前设置的整数值 `x`。

通过这个例子，可以看到接口和实现合约之间的关系。接口定义了一组函数的签名，而实现合约需要提供对应的函数实现。实现合约需要按照接口中定义的函数签名来定义函数，并确保函数修饰符（如 `external`、`payable`、`view`）和返回类型等与接口保持一致。这样，其他合约可以通过接口来调用实现合约中的函数，实现合约通过实现接口中定义的函数来提供功能。



## 调用`OtherContract`合约

### 1.传入合约地址

我们可以在函数里传入目标合约地址，生成目标合约的引用，然后调用目标函数。以调用OtherContract合约的setX函数为例，我们在新合约中写一个callSetX函数，传入已部署好的OtherContract合约地址_Address和setX的参数x：

```solidity
    // 传入合约地址
    function callSetX(address _address, uint256 x) external {
        OtherContract(_address).setX(x);
    }
```





### 2.传入合约变量

我们可以直接在函数里传入合约的引用，只需要把上面参数的address类型改为目标合约名，比如OtherContract。下面例子实现了调用目标合约的getX()函数。

注意该函数参数OtherContract _Address底层类型仍然是address，生成的ABI中、调用callGetX时传入的参数都是address类型

```solidity
    function callGetX(OtherContract _otherContract) external view returns(uint x){
        x = _otherContract.getX();
    }
```



### 3.创建合约变量

我们可以创建合约变量，然后通过它来调用目标函数。下面例子，我们给变量_otherContract存储了OtherContract合约的引用：

```solidity
    function callGetX2(address _address) external view returns(uint x){
        OtherContract _otherContract = OtherContract(_address);
        x = _otherContract.getX();
    }
```





### 4.调用合约并发送ETH

如果目标合约的函数是payable的，那么我们可以通过调用它来给合约转账：_Name(_Address).f{value: _Value}()，其中_Name是合约名，_Address是合约地址，f是目标函数名，_Value是要转的ETH数额（以wei为单位）。

OtherContract合约的setX函数是payable的，在下面这个例子中我们通过调用setX来往目标合约转账。

```solidity
    function CallSetX2(address _otherContract, uint256 x) payable external {
        OtherContract(_otherContract).setX{value:msg.value}(x);
    }
```

上面的代码定义了一个名为 `CallSetX2` 的函数，用于调用另一个合约的 `setX` 函数，并传递参数 `x`。该函数还接受以太币（ETH）作为支付。

函数签名为 `CallSetX2(address _otherContract, uint256 x) payable external`，表示该函数接受两个参数：一个 `address` 类型的 `_otherContract`，表示另一个合约的地址，和一个 `uint256` 类型的 `x`，表示要传递给 `setX` 函数的参数值。同时，函数标记为 `payable`，表示它可以接收以太币。

在函数内部，通过类型转换将 `_otherContract` 转换为 `OtherContract` 类型的实例，并使用该实例调用 `setX` 函数。为了支付以太币，使用了 `{value: msg.value}` 语法，表示将函数调用中的以太币转发给目标合约。这样，在调用 `setX` 函数时，将传递参数 `x`，同时也将发送的以太币转发给目标合约。

这段代码展示了如何在调用另一个合约函数时同时支付以太币。通过 `{value: msg.value}` 可以指定要支付的以太币数量，并将其转发给目标合约。这对于需要在函数调用中传递以太币的情况非常有用，例如向合约存储数据并支付费用。





```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

interface IOtherContract {
    function getBalance() external returns(uint);
    function setX(uint256 x) external  payable;
    function getX() external  view returns(uint x);
}

contract OtherContract is IOtherContract{
    uint256 private _x = 0;
    event Log(uint amount, uint gas);

    function getBalance() external  view override returns(uint){
        return address(this).balance;
    }

    function setX(uint256 x) external override  payable {
        _x = x;
        if(msg.value > 0){
            emit Log(msg.value, gasleft());
        }
    }

    function getX() external  view override  returns(uint x){
        x = _x;
    }
}

contract CallContract {
    // 传入合约地址
    function callSetX(address _address, uint256 x) external {
        OtherContract(_address).setX(x);
    }

    // 传入合约变量
    function callGetX(OtherContract _otherContract) external view returns(uint x){
        x = _otherContract.getX();
    }

    // 创建合约变量
    function callGetX2(address _address) external view returns(uint x){
        OtherContract _otherContract = OtherContract(_address);
        x = _otherContract.getX();
    }

    // 调用合约并发送ETH
    function CallSetX2(address _otherContract, uint256 x) payable external {
        OtherContract(_otherContract).setX{value:msg.value}(x);
    }
}
```



## 测试

![image-20230517184503131](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230517184503131.png)



>  向合约地址转账的操作



