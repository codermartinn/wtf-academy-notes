

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



# 22. Call

## Call

`call` 是`address`类型的低级成员函数，它用来与其他合约交互。它的返回值为`(bool, data)`，分别对应`call`是否成功以及目标函数的返回值。

- `call`是`solidity`官方推荐的通过触发`fallback`或`receive`函数发送`ETH`的方法。
- 不推荐用`call`来调用另一个合约，因为当你调用不安全合约的函数时，你就把主动权交给了它。推荐的方法仍是声明合约变量后调用函数，见[第21讲：调用其他合约](https://github.com/AmazingAng/WTFSolidity/tree/main/21_CallContract)
- 当我们不知道对方合约的源代码或`ABI`，就没法生成合约变量；这时，我们仍可以通过`call`调用对方合约的函数。





`call` 方法的一般语法如下：

```solidity
(bool success, bytes memory data) = address(target).call{value: amount}(functionSignature);
```

其中：
- `target` 是要调用的合约地址。
- `value` 是一个可选项，表示要附带的以太币金额。
- `functionSignature` 是要调用的函数的选择器（函数的哈希值）。

`call` 方法会返回一个布尔值 `success`，表示调用是否成功，以及一个 `bytes` 类型的 `data`，表示返回的数据。

需要注意的是，使用 `call` 进行合约交互存在一些重要的安全注意事项，如避免重入攻击和正确处理返回值等。在使用 `call` 进行合约交互时，建议仔细考虑安全性，并进行适当的检查和处理。

另外，Solidity还提供了其他更高级的方法来与其他合约进行交互，例如使用接口进行类型转换和使用 `external` 关键字来调用其他合约的函数。这些方法提供了更方便和安全的合约交互方式，推荐在大多数情况下使用它们。



## demo

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

contract OtherContract {
    uint256 private _x = 0; // 状态变量x
    // 收到eth事件，记录amount和gas
    event Log(uint amount, uint gas);

    fallback() external payable{}

    // 返回合约ETH余额
    function getBalance() view public returns(uint) {
        return address(this).balance;
    }

    // 可以调整状态变量_x的函数，并且可以往合约转ETH (payable)
    function setX(uint256 x) external payable{
        _x = x;
        // 如果转入ETH，则释放Log事件
        if(msg.value > 0){
            emit Log(msg.value, gasleft());
        }
    }

    // 读取x
    function getX() external view returns(uint x){
        x = _x;
    }
}

contract Call{
    // 定义Response事件，输出call返回的结果success和data
    event Response(bool success, bytes data);

    function callSetX(address payable _addr, uint256 x) public payable {
        // call setX()，同时可以发送ETH
        (bool success, bytes memory data) = _addr.call{value: msg.value}(
            abi.encodeWithSignature("setX(uint256)", x)
        );

        emit Response(success, data); //释放事件
    }

    function callGetX(address _addr) external returns(uint256){
        // call getX()
        (bool success, bytes memory data) = _addr.call(
            abi.encodeWithSignature("getX()")
        );

        emit Response(success, data); //释放事件
        return abi.decode(data, (uint256));
    }

    function callNonExist(address _addr) external{
        // call getX()
        (bool success, bytes memory data) = _addr.call(
            abi.encodeWithSignature("foo(uint256)")
        );

        emit Response(success, data); //释放事件
    }
}
```





## 测试

![image-20230518114336981](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230518114336981.png)

这个退款合约存在一个潜在的漏洞，可以被利用进行重入攻击。

在函数 `withdraw` 中，合约首先检查调用者在 `shares` 映射中的份额，然后使用 `call.value` 向调用者发送以太币。然而，在调用 `call` 方法时，被调用的函数可能会重新进入（re-enter）合约的 `withdraw` 函数，从而导致意外或恶意的操作。

攻击者可以通过构造一个恶意合约来利用这个漏洞。当攻击者调用 `withdraw` 函数时，他们的恶意合约可以在同一个交易中调用 `withdraw` 函数，重复执行退款并接收以太币，从而导致合约中的以太币被重复提取。

为了修复这个漏洞，可以使用一种称为"状态变量优先模式"（state variable state machine pattern）的方法。具体而言，在 `withdraw` 函数中添加一个标记来跟踪每个地址是否正在进行退款，并在进行退款前进行检查，以防止重入攻击。

以下是修复漏洞的修改版本：

```solidity
pragma solidity >=0.6.2 <0.9.0;

contract Fund {
    mapping(address => uint) shares;
    mapping(address => bool) private isWithdrawn;

    function withdraw() public {
        require(!isWithdrawn[msg.sender], "Already withdrawn");
        uint amount = shares[msg.sender];
        require(amount > 0, "No funds to withdraw");
        isWithdrawn[msg.sender] = true;
        shares[msg.sender] = 0;
        payable(msg.sender).transfer(amount);
    }
}
```

在修复后的合约中，添加了一个名为 `isWithdrawn` 的映射，用于跟踪每个地址是否已经进行了退款。在进行退款之前，首先检查 `isWithdrawn` 映射来确保地址没有重复进行退款操作。此外，使用 `transfer` 函数来替代 `call.value`，以避免重入攻击。

修复后的合约使用状态变量优先模式来确保每个地址只能进行一次退款操作，从而增加了安全性。









# 23. Delegatecall

## delegatecall

`delegatecall`与`call`类似，是`solidity`中地址类型的低级成员函数。`delegate`中是委托/代表的意思，那么`delegatecall`委托了什么？

当用户`A`通过合约`B`来`call`合约`C`的时候，执行的是合约`C`的函数，`语境`(`Context`，可以理解为包含变量和状态的环境)也是合约`C`的：`msg.sender`是`B`的地址，并且如果函数改变一些状态变量，产生的效果会作用于合约`C`的变量上。

![call的语境](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/VgMR533pA8WYtE5Lr65mQ.png)

而当用户`A`通过合约`B`来`delegatecall`合约`C`的时候，执行的是合约`C`的函数，但是`语境`仍是合约`B`的：`msg.sender`是`A`的地址，并且如果函数改变一些状态变量，产生的效果会作用于合约`B`的变量上。

![delegatecall的语境](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/JucQiWVixdlmJl6zHjCSI.png)

大家可以这样理解：一个`富商`把它的资产（`状态变量`）都交给一个`VC`代理（`目标合约`的函数）来打理。执行的是`VC`的函数，但是改变的是`富商`的状态。

`delegatecall`语法和`call`类似，也是：

```solidity
目标合约地址.delegatecall(二进制编码);
```



使用 `delegatecall` 可以将执行流程和状态传递给另一个合约，并在该合约中执行函数。与普通的函数调用不同，`delegatecall` 不会创建新的执行环境，而是在当前合约的上下文中执行目标合约的函数。

`delegatecall` 的语法如下：
```solidity
(bool success, bytes memory returnData) = address.delegatecall(bytes4(keccak256(bytes("functionName(arguments)"))));
```

使用 `delegatecall` 时需要注意以下几点：
- 调用的合约必须是已经存在并部署在链上的合约。
- 调用的函数必须是目标合约中存在的函数，否则会导致调用失败。
- 在 `delegatecall` 中，目标合约的代码会在当前合约的上下文中执行，包括存储状态和当前合约的地址。
- 调用结果以布尔值 `success` 和返回数据 `returnData` 的形式返回，`success` 表示调用是否成功，`returnData` 是目标合约函数的返回值。

需要注意的是，由于 `delegatecall` 在当前合约的上下文中执行目标合约的代码，因此需要谨慎使用，确保对目标合约的调用是安全的，并避免对当前合约的状态造成不可预料的影响。



## 什么情况下会用到delegatecall?

`delegatecall` 通常在以下情况下会被使用：

1. 升级合约：当需要升级合约逻辑时，可以使用 `delegatecall` 将执行流程转移到新的合约中，而不改变合约地址和存储状态。这样可以实现合约的升级，同时保留之前合约的存储数据。
2. 库合约调用：有时候，合约可能需要使用外部的库合约来提供某些功能。使用 `delegatecall` 可以在当前合约的上下文中调用库合约的函数，将库函数的代码执行在当前合约的上下文中。这样可以实现在当前合约中直接调用库合约的功能，而无需在当前合约中部署和维护重复的代码。
3. 状态共享：当需要多个合约之间共享状态时，可以使用 `delegatecall` 将执行流程转移到一个共享状态的合约中。这样多个合约可以共享同一个存储状态，实现状态共享和协作。

需要注意的是，使用 `delegatecall` 需要谨慎，并且确保对目标合约的调用是安全的。由于 `delegatecall` 允许在当前合约的上下文中执行目标合约的代码，因此目标合约的代码可以修改当前合约的状态。因此，在使用 `delegatecall` 时需要确保目标合约的代码是可信的，并且没有安全漏洞。



代理合约（`Proxy Contract`）：将智能合约的存储合约和逻辑合约分开：代理合约（`Proxy Contract`）存储所有相关的变量，并且保存逻辑合约的地址；所有函数存在逻辑合约（`Logic Contract`）里，通过`delegatecall`执行。

当升级时，只需要将代理合约指向新的逻辑合约即可。



## demo

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

// delegatecall和call类似，都是低级函数
// call: B call C, 语境为 C (msg.sender = B, C中的状态变量受影响)
// delegatecall: B delegatecall C, 语境为B (msg.sender = A, B中的状态变量受影响)
// 注意B和C的数据存储布局必须相同！变量类型、声明的前后顺序要相同，不然会搞砸合约。

// 被调用的合约C
contract C {
    uint public num;
    address public sender;

    function setVars(uint _num) public payable {
        num = _num;
        sender = msg.sender;
    }
}

// 发起delegatecall的合约B
contract B {
    uint public num;
    address public sender;

    // 通过call来调用C的setVars()函数，将改变合约C里的状态变量
    function callSetVars(address _addr, uint _num) external payable{
        (bool success, bytes memory data) = _addr.call(abi.encodeWithSignature("setVars(uint256)", _num));
    }

     // 通过delegatecall来调用C的setVars()函数，将改变合约B里的状态变量
    function delegatecallSetVars(address _addr, uint _num) external payable{
        (bool success, bytes memory data) = _addr.delegatecall(abi.encodeWithSignature("setVars(uint256)", _num));
    }
}
```

上述代码包含两个合约：合约 C 和合约 B。

合约 C 是被调用的合约，其中包含一个公共变量 `num` 和一个公共变量 `sender`。合约 C 中定义了一个名为 `setVars` 的函数，该函数接受一个参数 `_num`，将其赋值给 `num` 变量，并将调用者的地址存储在 `sender` 变量中。

合约 B 是发起 `delegatecall` 的合约。合约 B 中也包含一个公共变量 `num` 和一个公共变量 `sender`。合约 B 中定义了两个函数：

1. `callSetVars` 函数通过使用 `call` 调用合约 C 的 `setVars` 函数，将合约 C 中的状态变量进行更改。这里使用了 `abi.encodeWithSignature` 来生成函数调用的 ABI 编码。

2. `delegatecallSetVars` 函数通过使用 `delegatecall` 调用合约 C 的 `setVars` 函数，将合约 B 中的状态变量进行更改。与 `callSetVars` 类似，也使用了 `abi.encodeWithSignature` 生成函数调用的 ABI 编码。

需要注意的是，`delegatecall` 将调用合约 C 的函数在合约 B 的上下文中执行，所以在执行过程中，合约 C 中的代码认为调用者是合约 B。因此，合约 C 中的 `msg.sender` 将是合约 A（即合约 B 的调用者）的地址，而不是合约 B 自身的地址。

由于 `delegatecall` 将执行上下文切换到被调用合约，所以合约 B 中的状态变量也会受到影响。因此，在调用 `delegatecallSetVars` 函数后，合约 B 的 `num` 和 `sender` 变量将被修改为合约 C 中的相应值。

需要注意的是，在使用 `delegatecall` 时要非常小心，确保被调用的合约是可信的，并且了解其代码逻辑和预期行为。否则，可能导致意外的状态修改和安全漏洞。



![image-20230518143920344](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230518143920344.png)



## 测试

![image-20230518144540413](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230518144540413.png)



![image-20230518145311314](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230518145311314.png)





# 24.在合约中创建新合约

在以太坊链上，用户（外部账户，`EOA`）可以创建智能合约，智能合约同样也可以创建新的智能合约。去中心化交易所`uniswap`就是利用工厂合约（`Factory`）创建了无数个币对合约（`Pair`）。这一讲，我会用简化版的`uniswap`讲如何通过合约创建合约。



## `create`和`create2`

有两种方法可以在合约中创建新合约，`create`和`create2`，这里我们讲`create`，下一讲会介绍`create2`。

`create`的用法很简单，就是`new`一个合约，并传入新合约构造函数所需的参数：

```solidity
Contract x = new Contract{value: _value}(params)
```

其中`Contract`是要创建的合约名，`x`是合约对象（地址），如果构造函数是`payable`，可以创建时转入`_value`数量的`ETH`，`params`是新合约构造函数的参数。



## 极简Uniswap

`Uniswap V2`[核心合约](https://github.com/Uniswap/v2-core/tree/master/contracts)中包含两个合约：

1. UniswapV2Pair: 币对合约，用于管理币对地址、流动性、买卖。
2. UniswapV2Factory: 工厂合约，用于创建新的币对，并管理币对地址。



> 工厂生产袜子是吧？！



下面我们用`create`方法实现一个极简版的`Uniswap`：`Pair`币对合约负责管理币对地址，`PairFactory`工厂合约用于创建新的币对，并管理币对地址。



### `Pair`合约



```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

contract Pair{
    address public factory; // 工厂合约地址
    address public token0; // 代币1
    address public token1; // 代币2

    constructor() payable {
        factory = msg.sender;
    }

    // called once by the factory at time of deployment
    function initialize(address _token0, address _token1) external {
        require(msg.sender == factory, 'UniswapV2: FORBIDDEN'); // sufficient check
        token0 = _token0;
        token1 = _token1;
    }
}
```



`Pair`合约很简单，包含3个状态变量：`factory`，`token0`和`token1`。

构造函数`constructor`在部署时将`factory`赋值为工厂合约地址。`initialize`函数会在`Pair`合约创建的时候被工厂合约调用一次，将`token0`和`token1`更新为币对中两种代币的地址。

> **提问**：为什么`uniswap`不在`constructor`中将`token0`和`token1`地址更新好？
>
> **答**：因为`uniswap`使用的是`create2`创建合约，限制构造函数不能有参数。当使用`create`时，`Pair`合约允许构造函数有参数，可以在`constructor`中将`token0`和`token1`地址更新好。









### `PairFactory`

```solidity
contract PairFactory{
    mapping(address => mapping(address => address)) public getPair; // 通过两个代币地址查Pair地址
    address[] public allPairs; // 保存所有Pair地址

    function createPair(address tokenA, address tokenB) external returns (address pairAddr) {
        // 创建新合约
        Pair pair = new Pair(); 
        // 调用新合约的initialize方法
        pair.initialize(tokenA, tokenB);
        // 更新地址map
        pairAddr = address(pair);
        allPairs.push(pairAddr);
        getPair[tokenA][tokenB] = pairAddr;
        getPair[tokenB][tokenA] = pairAddr;
    }
}
```



- 如何理解solidity中的：` mapping(address => mapping(address => address)) public getPair; ` 



在 Solidity 中，`mapping` 是一种关联数据结构，类似于 Java 中的 `Map` 或者键值对。它用于存储键值对的映射关系。

在给定的代码中，`getPair` 是一个 `mapping` 类型的公共变量。它是一个二维映射，其中第一个键类型是 `address`，第二个键类型也是 `address`，值类型是 `address`。可以将其理解为一个三级关系，类似于 Java 中的嵌套 `Map`。

在 Solidity 中，`mapping` 类型的变量可以通过键来存储和访问对应的值。在这个例子中，`getPair` 可以通过两个地址作为键来查找和存储对应的地址值。这个 `mapping` 可以用于存储交易对的信息，例如通过两个代币地址（`address`）作为键来查找对应的交易对地址（`address`）。

示例用法：
- 存储交易对地址：`getPair[token0][token1] = pairAddress;`
- 获取交易对地址：`pairAddress = getPair[token0][token1];`

需要注意的是，`mapping` 类型的变量在 Solidity 中只能通过已知的键值来进行访问和修改，不能像数组那样遍历。



### code



```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

contract Pair{
    address public factory; // 工厂合约地址
    address public token0; // 代币1
    address public token1; // 代币2

    constructor() payable {
        factory = msg.sender;
    }

    // called once by the factory at time of deployment
    function initialize(address _token0, address _token1) external {
        require(msg.sender == factory, 'UniswapV2: FORBIDDEN'); // sufficient check
        token0 = _token0;
        token1 = _token1;
    }
}

contract PairFactory{
    mapping(address => mapping(address => address)) public getPair; // 通过两个代币地址查Pair地址
    address[] public allPairs; // 保存所有Pair地址

    function createPair(address tokenA, address tokenB) external returns (address pairAddr) {
        // 创建新合约
        Pair pair = new Pair(); 
        // 调用新合约的initialize方法
        pair.initialize(tokenA, tokenB);
        // 更新地址map
        pairAddr = address(pair);
        allPairs.push(pairAddr);
        getPair[tokenA][tokenB] = pairAddr;
        getPair[tokenB][tokenA] = pairAddr;
    }
}
```

上面的代码展示了两个合约：`Pair` 和 `PairFactory`。

`Pair` 合约是一个简单的交易对合约，用于存储两个代币的地址。它包含了三个公共变量：`factory`（工厂合约地址）、`token0`（代币1地址）和`token1`（代币2地址）。构造函数初始化了`factory`变量为合约部署者的地址。`initialize` 函数用于设置代币地址，并要求调用者必须是工厂合约。

`PairFactory` 合约是一个工厂合约，用于创建和管理交易对。它包含了两个重要的数据结构。首先，`getPair` 是一个双重映射，用于通过两个代币地址查找对应的交易对地址。其类型为 `mapping(address => mapping(address => address))`，即通过两个地址索引到一个地址。其目的是为了快速检索和查找已创建的交易对。其次，`allPairs` 是一个动态数组，用于保存所有创建的交易对的地址。

`PairFactory` 合约中的 `createPair` 函数用于创建一个新的交易对。它首先实例化了 `Pair` 合约，并调用 `initialize` 函数来设置代币地址。然后，更新 `getPair` 映射，将两个代币地址映射到新创建的交易对地址。最后，将交易对地址添加到 `allPairs` 数组中。

通过 `PairFactory` 合约，可以方便地创建和管理多个交易对，并且可以通过代币地址快速查找对应的交易对地址。



## 测试

![image-20230518152654131](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230518152654131.png)

![image-20230518152741341](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230518152741341.png)



- Solidity中创建新合约的关键字是：`new`

- `Contract x = new Contract{value: _value}(params)`，表达式中value代表：**当前合约发送给新创建合约的ETH**

- `Contract x = new Contract{value: _value}(params)`，表达式中params代表：**新合约的构造函数的参数**

- 1个工厂合约PairFactory创建Pair合约的最大数量一般由**PairFactory合约逻辑**决定

- 示例中Pair合约创建时的msg.sender是：**工厂合约PairFactory**



# 25. Create2

## CREATE2

`CREATE2` 操作码使我们在智能合约部署在以太坊网络之前就能预测合约的地址。`Uniswap`创建`Pair`合约用的就是`CREATE2`而不是`CREATE`。



### CREATE如何计算地址

智能合约可以由其他合约和普通账户利用`CREATE`操作码创建。 在这两种情况下，新合约的地址都以相同的方式计算：创建者的地址(通常为部署的钱包地址或者合约地址)和`nonce`(该地址发送交易的总数,对于合约账户是创建的合约总数,每创建一个合约nonce+1))的哈希。

```text
新地址 = hash(创建者地址, nonce)
```



创建者地址不会变，但`nonce`可能会随时间而改变，因此用`CREATE`创建的合约地址不好预测。

### CREATE2如何计算地址

`CREATE2`的目的是为了让合约地址独立于未来的事件。不管未来区块链上发生了什么，你都可以把合约部署在事先计算好的地址上。用`CREATE2`创建的合约地址由4个部分决定：

- `0xFF`：一个常数，避免和`CREATE`冲突
- 创建者地址
- `salt`（盐）：一个创建者给定的数值
- 待部署合约的字节码（`bytecode`）

```text
新地址 = hash("0xFF",创建者地址, salt, bytecode)
```



`CREATE2` 确保，如果创建者使用 `CREATE2` 和提供的 `salt` 部署给定的合约`bytecode`，它将存储在 `新地址` 中。

## 如何使用`CREATE2`

`CREATE2`的用法和之前讲的`Create`类似，同样是`new`一个合约，并传入新合约构造函数所需的参数，只不过要多传一个`salt`参数：

```text
Contract x = new Contract{salt: _salt, value: _value}(params)
```

其中`Contract`是要创建的合约名，`x`是合约对象（地址），`_salt`是指定的盐；如果构造函数是`payable`，可以创建时转入`_value`数量的`ETH`，`params`是新合约构造函数的参数。



## demo



```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

contract Pair{
    address public factory; // 工厂合约地址
    address public token0; // 代币1
    address public token1; // 代币2

    constructor() payable {
        factory = msg.sender;
    }

    // called once by the factory at time of deployment
    function initialize(address _token0, address _token1) external {
        require(msg.sender == factory, 'UniswapV2: FORBIDDEN'); // sufficient check
        token0 = _token0;
        token1 = _token1;
    }
}

contract PairFactory2{
        mapping(address => mapping(address => address)) public getPair; // 通过两个代币地址查Pair地址
        address[] public allPairs; // 保存所有Pair地址

        function createPair2(address tokenA, address tokenB) external returns (address pairAddr) {
            require(tokenA != tokenB, 'IDENTICAL_ADDRESSES'); //避免tokenA和tokenB相同产生的冲突
            // 计算用tokenA和tokenB地址计算salt
            (address token0, address token1) = tokenA < tokenB ? (tokenA, tokenB) : (tokenB, tokenA); //将tokenA和tokenB按大小排序
            bytes32 salt = keccak256(abi.encodePacked(token0, token1));
            // 用create2部署新合约
            Pair pair = new Pair{salt: salt}(); 
            // 调用新合约的initialize方法
            pair.initialize(tokenA, tokenB);
            // 更新地址map
            pairAddr = address(pair);
            allPairs.push(pairAddr);
            getPair[tokenA][tokenB] = pairAddr;
            getPair[tokenB][tokenA] = pairAddr;
        }

        // 提前计算pair合约地址
        function calculateAddr(address tokenA, address tokenB) public view returns(address predictedAddress){
            require(tokenA != tokenB, 'IDENTICAL_ADDRESSES'); //避免tokenA和tokenB相同产生的冲突
            // 计算用tokenA和tokenB地址计算salt
            (address token0, address token1) = tokenA < tokenB ? (tokenA, tokenB) : (tokenB, tokenA); //将tokenA和tokenB按大小排序
            bytes32 salt = keccak256(abi.encodePacked(token0, token1));
            // 计算合约地址方法 hash()
            predictedAddress = address(uint160(uint(keccak256(abi.encodePacked(
                bytes1(0xff),
                address(this),
                salt,
                keccak256(type(Pair).creationCode)
            )))));
        }
}
```



## 测试

- Create2与Create的不同之处在于，Create2可以让合约地址独立于未来的事件。
- Create2创建的合约地址由4部分决定
- Create2的用法相比于Create，在new一个合约并传入构造函数所需参数时，需要多传入一个创建者给定的数值：salt
- 利用Create2创建合约的代码中，参数salt等于一对代币地址的哈希



## create2的实际应用场景

1. 交易所为新用户预留创建钱包合约地址。
2. 由 `CREATE2` 驱动的 `factory` 合约，在`uniswapV2`中交易对的创建是在 `Factory`中调用`create2`完成。这样做的好处是: 它可以得到一个确定的`pair`地址, 使得 `Router`中就可以通过 `(tokenA, tokenB)` 计算出`pair`地址, 不再需要执行一次 `Factory.getPair(tokenA, tokenB)` 的跨合约调用。

## 总结

这一讲，我们介绍了`CREATE2`操作码的原理，使用方法，并用它完成了极简版的`Uniswap`并提前计算币对合约地址。`CREATE2`让我们可以在部署合约前确定它的合约地址，这也是 一些`layer2`项目的基础。



# 26. 删除合约



## `selfdestruct`

`selfdestruct`命令可以用来删除智能合约，并将该合约剩余`ETH`转到指定地址。`selfdestruct`是为了应对合约出错的极端情况而设计的。它最早被命名为`suicide`（自杀），但是这个词太敏感。为了保护抑郁的程序员，改名为`selfdestruct`。

### 如何使用`selfdestruct`

`selfdestruct`使用起来非常简单：

```solidity
selfdestruct(_addr)；
```



其中`_addr`是接收合约中剩余`ETH`的地址。

### 例子

```solidity
contract DeleteContract {

    uint public value = 10;

    constructor() payable {}

    receive() external payable {}

    function deleteContract() external {
        // 调用selfdestruct销毁合约，并把剩余的ETH转给msg.sender
        selfdestruct(payable(msg.sender));
    }

    function getBalance() external view returns(uint balance){
        balance = address(this).balance;
    }
}
```



在`DeleteContract`合约中，我们写了一个`public`状态变量`value`，两个函数：`getBalance()`用于获取合约`ETH`余额，`deleteContract()`用于自毁合约，并把`ETH`转入给发起人。

部署好合约后，我们向`DeleteContract`合约转入1 `ETH`。这时，`getBalance()`会返回1 `ETH`，`value`变量是10。

当我们调用`deleteContract()`函数，合约将自毁，所有变量都清空，此时`value`变为默认值`0`，`getBalance()`也返回空值。

### 注意事项

1. 对外提供合约销毁接口时，最好设置为只有合约所有者可以调用，可以使用函数修饰符`onlyOwner`进行函数声明。
2. 当合约被销毁后与智能合约的交互也能成功，并且返回0。
3. 当合约中有`selfdestruct`功能时常常会带来安全问题和信任问题，合约中的Selfdestruct功能会为攻击者打开攻击向量(例如使用`selfdestruct`向一个合约频繁转入token进行攻击，这将大大节省了GAS的费用，虽然很少人这么做)，此外，此功能还会降低用户对合约的信心。





# 27. ABI编码解码

[27. ABI编码解码 | WTF Academy](https://www.wtf.academy/solidity-advanced/ABIEncode/)



## 测试

![image-20230518172849843](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230518172849843.png)

`abi.encodePacked()`函数用于将给定的参数按顺序紧密打包为字节序列，而不添加任何编码元数据。它返回一个字节数组，其中包含按顺序拼接的参数值。

尽管`abi.encodePacked()`可以将参数打包为字节序列，但由于缺乏编码元数据，它并不适合直接用作调用智能合约的数据。在调用合约时，通常需要遵循特定的ABI编码规范，以确保数据按照正确的格式进行传递和解析。

使用`abi.encodePacked()`返回的字节数组作为调用数据可能会导致以下问题：

1. 参数解析错误：调用合约时，合约函数的参数可能包含不同的数据类型和编码要求。如果直接使用`abi.encodePacked()`返回的字节数组，可能无法正确解析和识别参数的类型和值。
2. 数据对齐问题：合约函数的参数通常需要按照特定的规则进行对齐，以确保数据在字节序列中的位置正确。使用`abi.encodePacked()`返回的字节数组可能无法满足正确的数据对齐要求。

因此，为了确保正确的数据传递和解析，应使用适当的ABI编码方法，如`abi.encode()`或`abi.encodeWithSignature()`来生成调用智能合约所需的数据。这些方法将提供符合ABI规范的编码结果，包含必要的编码元数据，以确保数据按照正确的格式进行传递和解析。



# 28.Hash

[28. Hash | WTF Academy](https://www.wtf.academy/solidity-advanced/Hash)



# 29. 函数选择器Selector

[29. 选择器 | WTF Academy](https://www.wtf.academy/solidity-advanced/Selector/)

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

contract Selector{
    // event 返回msg.data
    event Log(bytes data);

    // 输入参数 to: 0x2c44b726ADF1963cA47Af88B284C06f30380fC78
    function mint(address /*to*/) external{
        emit Log(msg.data);
    } 

    // 输出selector
    // "mint(address)"： 0x6a627842
    function mintSelector() external pure returns(bytes4 mSelector){
        return bytes4(keccak256("mint(address)"));
    }

    // 使用selector来调用函数
    function callWithSignature() external returns(bool, bytes memory){
        // 只需要利用`abi.encodeWithSelector`将`mint`函数的`selector`和参数打包编码
        (bool success, bytes memory data) = address(this).call(abi.encodeWithSelector(0x6a627842, 0x2c44b726ADF1963cA47Af88B284C06f30380fC78));
        return(success, data);
    }
}
```



## 测试

函数选择器有几个字节？4



![image-20230518192037740](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230518192037740.png)



![image-20230518192144598](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230518192144598.png)





# 30. Try Catch



## try-catch

在`solidity`中，`try-catch`只能被用于`external`函数或创建合约时`constructor`（被视为`external`函数）的调用。基本语法如下：

```solidity
        try externalContract.f() {
            // call成功的情况下 运行一些代码
        } catch {
            // call失败的情况下 运行一些代码
        }
```



其中`externalContract.f()`是某个外部合约的函数调用，`try`模块在调用成功的情况下运行，而`catch`模块则在调用失败时运行。

同样可以使用`this.f()`来替代`externalContract.f()`，`this.f()`也被视作为外部调用，但不可在构造函数中使用，因为此时合约还未创建。

如果调用的函数有返回值，那么必须在`try`之后声明`returns(returnType val)`，并且在`try`模块中可以使用返回的变量；如果是创建合约，那么返回值是新创建的合约变量。

```solidity
        try externalContract.f() returns(returnType val){
            // call成功的情况下 运行一些代码
        } catch {
            // call失败的情况下 运行一些代码
        }
```



另外，`catch`模块支持捕获特殊的异常原因：

```solidity
        try externalContract.f() returns(returnType){
            // call成功的情况下 运行一些代码
        } catch Error(string memory reason) {
            // 捕获失败的 revert() 和 require()
        } catch (bytes memory reason) {
            // 捕获失败的 assert()
        }
```



## 测试

- try-catch可以捕获的异常：revert()、require()、assert()
- revert()、require()、assert() 的异常返回值类型都是 bytes
- try-catch捕获到异常后不会使try-catch所在的方法调用失败
- try代码块内的revert不会被catch本身捕获



