# 31.ERC20



## ERC20

 `ERC20`是以太坊上的代币标准，来自2015年11月V神参与的[`EIP20`](https://eips.ethereum.org/EIPS/eip-20)。它实现了代币转账的基本逻辑：

- 账户余额
- 转账
- 授权转账
- 代币总供给
- 代币信息（可选）：名称，代号，小数位数





## IERC20

`IERC20`是`ERC20`代币标准的接口合约，规定了`ERC20`代币需要实现的函数和事件。 之所以需要定义接口，是因为有了规范后，就存在所有的`ERC20`代币都通用的函数名称，输入参数，输出参数。 在接口函数中，只需要定义函数名称，输入参数，输出参数，并不关心函数内部如何实现。 由此，函数就分为内部和外部两个内容，一个重点是实现，另一个是对外接口，约定共同数据。 这就是为什么需要`ERC20.sol`和`IERC20.sol`两个文件实现一个合约。

> IERC20
>
> - 对外
>
> ERC20
>
> - 函数如何实现





### event

`IERC20`定义了`2`个事件：`Transfer`事件和`Approval`事件，分别在转账和授权时被释放

```solidity
    /**
     * @dev 释放条件：当 `value` 单位的货币从账户 (`from`) 转账到另一账户 (`to`)时.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @dev 释放条件：当 `value` 单位的货币从账户 (`owner`) 授权给另一账户 (`spender`)时.
     */
    event Approval(address indexed owner, address indexed spender, uint256 value);
```



> `Transfer()`比较好理解，但是`Approval()`比较难理解



### 函数

以下是IERC20接口中常见的函数：

1. `totalSupply()`: 返回代币的总供应量。
2. `balanceOf(address account)`: 返回指定账户地址持有的代币余额。
3. `transfer(address recipient, uint256 amount)`: 将指定数量的代币从调用者账户转移到接收者账户。
4. `allowance(address owner, address spender)`: 返回允许spender从owner账户转移的代币数量。
5. `approve(address spender, uint256 amount)`: 授权spender能够从调用者账户转移指定数量的代币。
6. `transferFrom(address sender, address recipient, uint256 amount)`: 从发送者账户转移指定数量的代币到接收者账户，需要经过调用者账户预先授权。

除了这些基本的函数，IERC20接口还定义了一些事件，例如`Transfer`和`Approval`，用于记录代币的转账和授权操作。

通过实现IERC20接口，一个智能合约可以提供代币的基本功能，例如转账、余额查询和授权等，使得代币合约可以与其他合约和应用进行交互，实现代币的使用和流通。在以太坊上，许多代币合约都遵循了IERC20接口标准，这样就能够保证它们之间的互操作性和兼容性。



**通过形象化的例子来说明什么是`Approval()`函数**



当一个持有代币的账户（称为所有者）希望授权另一个账户（称为支出者）可以从其账户中转移一定数量的代币时，可以使用`approve()`函数。

假设有一个名为`Token`的代币合约，账户A持有100个Token。账户A希望允许账户B从其账户中转移10个Token。以下是授权的步骤：

1. 账户A调用`approve()`函数，向代币合约传递账户B的地址和要授权的Token数量。代码示例：`Token.approve(B, 10)`。
2. 代币合约接收到授权请求后，记录下账户A授权账户B可以转移10个Token。
3. 现在，账户B可以调用`transferFrom()`函数来从账户A转移10个Token到其他账户。代码示例：`Token.transferFrom(A, C, 10)`。

通过这种方式，账户A可以授权其他账户在其名义下转移一定数量的代币。这在某些场景下非常有用，例如允许某个智能合约代表用户执行特定的操作或转移代币。授权的限额可以在授权时指定，确保授权不会超出预期的数量。







## 实现ERC20

要实现ERC20标准的代币合约，你可以按照以下步骤进行：

1. 创建一个Solidity合约，并指定Solidity的版本。

```solidity
pragma solidity ^0.8.0;

contract MyToken {
    // 代币的名称
    string public name;
    // 代币的符号（缩写）
    string public symbol;
    // 代币的小数位数
    uint8 public decimals;
    // 代币的总供应量
    uint256 public totalSupply;
    
    // 使用mapping存储账户地址和对应的代币余额
    mapping(address => uint256) public balanceOf;
    // 使用mapping存储账户地址之间的授权额度
    mapping(address => mapping(address => uint256)) public allowance;

    // 构造函数，在部署合约时执行
    constructor(string memory _name, string memory _symbol, uint8 _decimals, uint256 _totalSupply) {
        name = _name;
        symbol = _symbol;
        decimals = _decimals;
        totalSupply = _totalSupply;
        
        // 将总供应量分配给合约创建者
        balanceOf[msg.sender] = _totalSupply;
    }
    
    // 转账函数
    function transfer(address _to, uint256 _value) public returns (bool success) {
        require(balanceOf[msg.sender] >= _value, "Insufficient balance"); // 检查发送者账户余额是否足够
        
        balanceOf[msg.sender] -= _value; // 扣除发送者账户余额
        balanceOf[_to] += _value; // 增加接收者账户余额
        
        emit Transfer(msg.sender, _to, _value); // 触发Transfer事件
        
        return true; // 转账成功
    }
    
    // 授权函数
    function approve(address _spender, uint256 _value) public returns (bool success) {
        allowance[msg.sender][_spender] = _value; // 设置发送者授权额度
        
        emit Approval(msg.sender, _spender, _value); // 触发Approval事件
        
        return true; // 授权成功
    }
    
    // 代币转账函数，需要事先进行授权
    function transferFrom(address _from, address _to, uint256 _value) public returns (bool success) {
        require(balanceOf[_from] >= _value, "Insufficient balance"); // 检查发送者账户余额是否足够
        require(allowance[_from][msg.sender] >= _value, "Insufficient allowance"); // 检查授权额度是否足够
        
        balanceOf[_from] -= _value; // 扣除发送者账户余额
        balanceOf[_to] += _value; // 增加接收者账户余额
        allowance[_from][msg.sender] -= _value; // 减少发送者授权额度
        
        emit Transfer(_from, _to, _value); // 触发Transfer事件
        
        return true; // 转账成功
    }

    // Transfer事件
    event Transfer(address indexed _from, address indexed _to, uint256 _value);

    // Approval事件
    event Approval(address indexed _owner, address indexed _spender, uint256 _value);
}
```

2. 在构造函数中设置代币的名称、符号、小数位数和总供应量，并将总供应量分配给合约创建者。

3. 使用mapping数据结构存储账户地址和对应的代币余额。`balanceOf`用于记录每个账户的代币余额。

4. 使用mapping数据结构存储账户地址之间的授权额度。`allowance`用于记录授权的额度，其中`allowance[owner][spender]`表示`owner`账户授权给`spender`账户的额度。

5. 实现转账函数`transfer()`，用于实现账户之间的代币转移。

6. 实现授权函数`approve()`，用于授权其他账户可以从发送者账户中转移一定数量的代币。

7. 实现代币转账函数`transferFrom()`，该函数需要事先进行授权，用于实现从一个账户转移代币到另一个账户。

8. 声明`Transfer`和`Approval`事件，用于在转账和授权操作时触发事件。

以上是一个简单的ERC20代币合约的实现。你可以根据自己的需求进行扩展和修改。




## demo

### IERC20

```solidity
// SPDX-License-Identifier: MIT
// WTF Solidity by 0xAA

pragma solidity ^0.8.4;

/**
 * @dev ERC20 接口合约.
 */
interface IERC20 {
    /**
     * @dev 释放条件：当 `value` 单位的货币从账户 (`from`) 转账到另一账户 (`to`)时.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @dev 释放条件：当 `value` 单位的货币从账户 (`owner`) 授权给另一账户 (`spender`)时.
     */
    event Approval(address indexed owner, address indexed spender, uint256 value);

    /**
     * @dev 返回代币总供给.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev 返回账户`account`所持有的代币数.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev 转账 `amount` 单位代币，从调用者账户到另一账户 `to`.
     *
     * 如果成功，返回 `true`.
     *
     * 释放 {Transfer} 事件.
     */
    function transfer(address to, uint256 amount) external returns (bool);

    /**
     * @dev 返回`owner`账户授权给`spender`账户的额度，默认为0。
     *
     * 当{approve} 或 {transferFrom} 被调用时，`allowance`会改变.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev 调用者账户给`spender`账户授权 `amount`数量代币。
     *
     * 如果成功，返回 `true`.
     *
     * 释放 {Approval} 事件.
     */
    function approve(address spender, uint256 amount) external returns (bool);

    /**
     * @dev 通过授权机制，从`from`账户向`to`账户转账`amount`数量代币。转账的部分会从调用者的`allowance`中扣除。
     *
     * 如果成功，返回 `true`.
     *
     * 释放 {Transfer} 事件.
     */
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}
```

### ERC20

```solidity
// SPDX-License-Identifier: MIT
// WTF Solidity by 0xAA

pragma solidity ^0.8.4;

import "./IERC20.sol";

contract ERC20 is IERC20 {
    // override 修饰public 会重写继承父合约的getter函数
    // 使用mapping存储账户地址和对应的代币余额
    mapping(address => uint256) public override balanceOf; 

    // 使用mapping存储账户地址之间的授权额度
    mapping( address => mapping(address => uint256)) public override allowance;

    // 代币总供量
    uint256 public override totalSupply;

    string public name;
    string public symbol;

    uint8 public decimals  = 18;

    // @dev 在合约部署的时候实现合约名称和符号
    constructor(string memory _name, string memory _symbol){
        name = _name;
        symbol = _symbol;
    }

    // @dev 实现`transfer`函数，代币转账逻辑
    function transfer(address recipient, uint256 amount) external override returns(bool) {
        balanceOf[msg.sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    // @dev 实现 `approve` 函数, 代币授权逻辑
    function approve(address spender, uint256 amount) external returns (bool){
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    // @dev 实现`transferFrom`函数，代币授权转账逻辑
    function transferFrom(address from,address to,uint256 amount) external returns (bool){
        allowance[from][to] -= amount;
        balanceOf[from] -= amount;
        balanceOf[to] += amount;
        emit Transfer(from, to, amount);
        return true;
    }

    // @dev 铸造代币，从 `0` 地址转账给 调用者地址
    function mint(uint amount) external {
        balanceOf[msg.sender] += amount;
        totalSupply += amount;
        emit Transfer(address(0), msg.sender, amount);
    }

    // @dev 销毁代币，从 调用者地址 转账给  `0` 地址
    function burn(uint amount) external {
        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;
        emit Transfer(msg.sender, address(0), amount);
    }
}
```



# 32. 代币水龙头



## 什么是水龙头

ERC20水龙头（ERC20 Faucet）是一个提供免费ERC20代币的服务或工具。它允许用户获取一定数量的ERC20代币，以便进行测试、学习或其他目的。

通常，ERC20水龙头会要求用户提供一个有效的以太坊地址，并在验证后向该地址发送一定数量的ERC20代币。这样，用户就可以使用这些代币进行各种操作，例如测试智能合约、交互与DApp，或者了解ERC20代币的功能和使用方式。

ERC20水龙头在以太坊生态系统中非常有用，特别是对于开发者、研究人员和新手用户来说。它们为用户提供了免费获取代币的机会，而不需要购买或交换真实的货币。这对于学习和探索以太坊网络和ERC20代币的特性非常有帮助。



## ERC20水龙头合约

```solidity
// SPDX-License-Identifier: MIT
// WTF Solidity by 0xAA

pragma solidity ^0.8.4;

import "./IERC20.sol";

contract ERC20 is IERC20 {
    // override 修饰public 会重写继承父合约的getter函数
    // 使用mapping存储账户地址和对应的代币余额
    mapping(address => uint256) public override balanceOf; 

    // 使用mapping存储账户地址之间的授权额度
    mapping( address => mapping(address => uint256)) public override allowance;

    // 代币总供量
    uint256 public override totalSupply;

    string public name;
    string public symbol;

    uint8 public decimals  = 18;

    // @dev 在合约部署的时候实现合约名称和符号
    constructor(string memory _name, string memory _symbol){
        name = _name;
        symbol = _symbol;
    }

    // @dev 实现`transfer`函数，代币转账逻辑
    function transfer(address recipient, uint256 amount) external override returns(bool) {
        balanceOf[msg.sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    // @dev 实现 `approve` 函数, 代币授权逻辑
    function approve(address spender, uint256 amount) external returns (bool){
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    // @dev 实现`transferFrom`函数，代币授权转账逻辑
    function transferFrom(address from,address to,uint256 amount) external returns (bool){
        allowance[from][to] -= amount;
        balanceOf[from] -= amount;
        balanceOf[to] += amount;
        emit Transfer(from, to, amount);
        return true;
    }

    // @dev 铸造代币，从 `0` 地址转账给 调用者地址
    function mint(uint amount) external {
        balanceOf[msg.sender] += amount;
        totalSupply += amount;
        emit Transfer(address(0), msg.sender, amount);
    }

    // @dev 销毁代币，从 调用者地址 转账给  `0` 地址
    function burn(uint amount) external {
        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;
        emit Transfer(msg.sender, address(0), amount);
    }
}

// ERC20代币的水龙头合约
contract Faucet{
    uint256 public amountAllowed = 100; // 每次领 100单位代币
    address public tokenContract;   // token合约地址
    mapping(address => bool) public requestedAddress;   // 记录领取过代币的地址

    // SendToken事件,记录每次领取代币的地址和数量
    event SendToken(address indexed Receiver, uint256 indexed Amount);

    // 部署时设定ERC2代币合约
    constructor(address _tokenContract) {
        tokenContract = _tokenContract; // set token contract
    }

    // 用户领取代币函数
    function requestTokens() external {
        require(!requestedAddress[msg.sender], "Can't Request Multiple Times!"); // 每个地址只能领一次

        IERC20 token = IERC20(tokenContract); // 创建IERC20合约对象
        require(token.balanceOf(address(this)) >= amountAllowed, "Faucet Empty!"); // 水龙头空了

        token.transfer(msg.sender, amountAllowed); // 发送token
        requestedAddress[msg.sender] = true; // 记录领取地址 
        
        emit SendToken(msg.sender, amountAllowed); // 释放SendToken事件
    }

}
```

为什么领取水龙头的代码要单独一个`contract`?

在给定的代码中，领取水龙头的代码被放在了单独的`Faucet`合约中。这是因为水龙头合约有自己独立的功能和逻辑，它并不是 ERC20 代币合约本身的一部分。

通过将领取水龙头的功能放在单独的合约中，可以更好地组织代码并提供清晰的模块化结构。这种设计使得代币合约与水龙头合约相互独立，方便对它们进行单独的部署、升级和维护。

在水龙头合约中，`Faucet`合约通过构造函数接收 ERC20 代币合约的地址，并将其保存在 `tokenContract` 变量中。然后，`requestTokens` 函数允许用户从水龙头合约领取指定数量的代币。这样，用户可以通过调用水龙头合约的 `requestTokens` 函数来获取代币，而不需要直接与 ERC20 代币合约进行交互。

总而言之，将领取水龙头的代码放在单独的合约中有助于代码的模块化和可维护性，同时使代币合约和水龙头合约分开管理和操作。





# 33. 发送空投

## 空投 Airdrop

因为每次接收空投的用户很多，项目方不可能一笔一笔的转账。利用智能合约批量发放`ERC20`代币，可以显著提高空投效率。





### 向多个地址转账ERC20代币

```solidity
    function multiTransferToken(
        address _token,
        address[] calldata _addresses,
        uint256[] calldata _amounts
        ) external {
        // 检查：_addresses和_amounts数组的长度相等
        require(_addresses.length == _amounts.length, "Lengths of Addresses and Amounts NOT EQUAL");
        IERC20 token = IERC20(_token); // 声明IERC合约变量
        uint _amountSum = getSum(_amounts); // 计算空投代币总量
        // 检查：授权代币数量 > 空投代币总量
        require(token.allowance(msg.sender, address(this)) > _amountSum, "Need Approve ERC20 token");
        

        // for循环，利用transferFrom函数发送空投
        for (uint256 i; i < _addresses.length; i++) {
            token.transferFrom(msg.sender, _addresses[i], _amounts[i]);
        }
    }
```



- `token.allowance(msg.sender, address(this)) `中`address(this)`是什么意思？

在给定的代码中，`address(this)` 表示当前合约的地址。它用作 `token.allowance` 函数的第二个参数。

`address(this)` 是 Solidity 中的一种特殊语法，用于引用当前合约的地址。在该上下文中，`address(this)` 指的是正在执行的合约的地址。

在代码中的上下文中，`token.allowance(msg.sender, address(this))` 的作用是检查当前合约在代币合约中被调用者（`msg.sender`）授权的代币数量是否足够进行空投操作。它通过 `token.allowance` 函数查询代币合约中的授权额度，第一个参数是授权的所有者（`msg.sender`），第二个参数是当前合约的地址。

通过使用 `address(this)`，可以确保在空投操作中，代币合约检查的是当前合约的授权额度，而不是其他地址的授权额度。



- 代码为什么用`calldata`?

在给定的代码中，`calldata` 关键字用于修饰函数参数。

`calldata` 关键字表示函数参数是通过调用方传递给合约的不可变数据。它是一种特殊的数据位置，用于指示函数参数在函数执行期间只能被读取，而不能被修改。

在给定的代码中，函数 `multiTransferToken` 的参数 `_addresses` 和 `_amounts` 被声明为 `calldata` 类型。这是因为在该函数中，这些参数只需要被读取，而不需要在函数执行期间被修改。

使用 `calldata` 关键字可以带来一些优势：

- 节省 gas 成本：由于函数参数是不可变的，Solidity 编译器可以优化函数调用，减少 gas 消耗。
- 显式指定意图：通过将参数声明为 `calldata`，可以清楚地表达参数在函数中的使用方式，即只读取而不修改。

总之，使用 `calldata` 关键字对于只读取函数参数的情况是合适的，它可以提高代码的可读性和效率。





### 向多个地址转账ETH

```solidity
    /// 向多个地址转账ETH
    function multiTransferETH(
        address payable[] calldata _addresses,
        uint256[] calldata _amounts
    ) public payable {
        // 检查：_addresses和_amounts数组的长度相等
        require(_addresses.length == _amounts.length, "Lengths of Addresses and Amounts NOT EQUAL");

        uint _amountSum = getSum(_amounts); // 计算空投ETH总量
        // 检查转入ETH等于空投总量
        require(msg.value == _amountSum, "Transfer amount error");
        
        // for循环，利用transfer函数发送ETH
        for (uint256 i = 0; i < _addresses.length; i++) {
            _addresses[i].transfer(_amounts[i]);
        }
    }
```





```solidity
// SPDX-License-Identifier: MIT
// WTF Solidity by 0xAA

pragma solidity ^0.8.4;

import "./IERC20.sol";

contract ERC20 is IERC20 {
    // override 修饰public 会重写继承父合约的getter函数
    // 使用mapping存储账户地址和对应的代币余额
    mapping(address => uint256) public override balanceOf; 

    // 使用mapping存储账户地址之间的授权额度
    mapping( address => mapping(address => uint256)) public override allowance;

    // 代币总供量
    uint256 public override totalSupply;

    string public name;
    string public symbol;

    uint8 public decimals  = 18;

    // @dev 在合约部署的时候实现合约名称和符号
    constructor(string memory _name, string memory _symbol){
        name = _name;
        symbol = _symbol;
    }

    // @dev 实现`transfer`函数，代币转账逻辑
    function transfer(address recipient, uint256 amount) external override returns(bool) {
        balanceOf[msg.sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    // @dev 实现 `approve` 函数, 代币授权逻辑
    function approve(address spender, uint256 amount) external returns (bool){
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    // @dev 实现`transferFrom`函数，代币授权转账逻辑
    function transferFrom(address from,address to,uint256 amount) external returns (bool){
        allowance[from][to] -= amount;
        balanceOf[from] -= amount;
        balanceOf[to] += amount;
        emit Transfer(from, to, amount);
        return true;
    }

    // @dev 铸造代币，从 `0` 地址转账给 调用者地址
    function mint(uint amount) external {
        balanceOf[msg.sender] += amount;
        totalSupply += amount;
        emit Transfer(address(0), msg.sender, amount);
    }

    // @dev 销毁代币，从 调用者地址 转账给  `0` 地址
    function burn(uint amount) external {
        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;
        emit Transfer(msg.sender, address(0), amount);
    }
}

/// @notice 向多个地址转账ERC20代币
contract Airdrop {

    function multiTransferToken(
        address _token,
        address[] calldata _addresses,
        uint256[] calldata _amounts
        ) external {
        // 检查：_addresses和_amounts数组的长度相等
        require(_addresses.length == _amounts.length, "Lengths of Addresses and Amounts NOT EQUAL");
        
        
        IERC20 token = IERC20(_token); // 声明IERC合约变量
        uint _amountSum = getSum(_amounts); // 计算空投代币总量
        // 检查：(当前调用者授予当前合约的数量)授权代币数量 > 空投代币总量
        require(token.allowance(msg.sender, address(this)) > _amountSum, "Need Approve ERC20 token");
        

        // for循环，利用transferFrom函数发送空投
        for (uint256 i; i < _addresses.length; i++) {
            token.transferFrom(msg.sender, _addresses[i], _amounts[i]);
        }
    }


    /// 向多个地址转账ETH
    function multiTransferETH(
        address payable[] calldata _addresses,
        uint256[] calldata _amounts
    ) public payable {
        // 检查：_addresses和_amounts数组的长度相等
        require(_addresses.length == _amounts.length, "Lengths of Addresses and Amounts NOT EQUAL");

        uint _amountSum = getSum(_amounts); // 计算空投ETH总量
        // 检查转入ETH等于空投总量
        require(msg.value == _amountSum, "Transfer amount error");
        
        // for循环，利用transfer函数发送ETH
        for (uint256 i = 0; i < _addresses.length; i++) {
            _addresses[i].transfer(_amounts[i]);
        }
    }



     // 数组求和函数
    function getSum(uint256[] calldata _arr) public pure returns(uint sum){
        for(uint i = 0; i < _arr.length; i++)
            sum = sum + _arr[i];
    }
}
```













# 34. ERC721

`BTC`和`ETH`这类代币都属于同质化代币，矿工挖出的第`1`枚`BTC`与第`10000`枚`BTC`并没有不同，是等价的。但世界中很多物品是不同质的，其中包括房产、古董、虚拟艺术品等等，这类物品无法用同质化代币抽象。因此，[以太坊EIP721](https://eips.ethereum.org/EIPS/eip-721)提出了`ERC721`标准，来抽象非同质化的物品。这一讲，我们将介绍`ERC721`标准，并基于它发行一款`NFT`。



## EIP与ERC

EIP（Ethereum Improvement Proposal）和 ERC（Ethereum Request for Comments）是两个在以太坊生态系统中广泛使用的标准。

1. EIP（Ethereum Improvement Proposal）：
   - EIP是以太坊网络的改进提案标准。
   - EIP用于提出关于以太坊协议、功能、标准和改进的技术提案。
   - EIP由以太坊社区的开发者和利益相关者创建、提交和讨论，并通过社区共识达成最终决策。
   - EIP提案经过审核、讨论和接受后，可以成为以太坊协议的一部分，实现对以太坊网络的升级和改进。
2. ERC（Ethereum Request for Comments）：
   - ERC是以太坊网络中代币标准的提案和规范。
   - ERC提案用于定义和描述以太坊上的代币合约接口和功能。
   - ERC提案是由以太坊社区的开发者和利益相关者创建和提出的。
   - ERC提案经过讨论、审核和接受后，可以成为以太坊上新的代币标准，并被其他合约开发者使用和实现。

总结： EIP和ERC都是以太坊社区的标准化提案，但它们的关注点和应用领域略有不同。EIP是针对整个以太坊协议和生态系统的改进提案，而ERC是关于代币合约接口和标准的提案。EIP用于协议的升级和改进，而ERC用于代币合约的设计和实现。



## ERC165

通过ERC165标准，智能合约可以声明它支持的接口，供其他合约检查。

简单的说，ERC165就是检查一个智能合约是不是支持了ERC721，ERC1155的接口。



## IERC721

IERC721 是以太坊上的非同质代币标准接口，定义了非同质代币的基本功能和操作，包括代币的所有权转移、元数据定义、授权机制和事件通知等。通过遵循 IERC721 接口，开发者可以创建和操作独特的数字资产，如艺术品、游戏道具和房地产等。这为数字资产领域提供了标准化和互操作性，促进了数字资产的发展和应用。



```solidity
/**
 * @dev ERC721标准接口.
 */
interface IERC721 is IERC165 {
    event Transfer(address indexed from, address indexed to, uint256 indexed tokenId);
    event Approval(address indexed owner, address indexed approved, uint256 indexed tokenId);
    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);

    function balanceOf(address owner) external view returns (uint256 balance);

    function ownerOf(uint256 tokenId) external view returns (address owner);

    function safeTransferFrom(
        address from,
        address to,
        uint256 tokenId,
        bytes calldata data
    ) external;

    function safeTransferFrom(
        address from,
        address to,
        uint256 tokenId
    ) external;

    function transferFrom(
        address from,
        address to,
        uint256 tokenId
    ) external;

    function approve(address to, uint256 tokenId) external;

    function setApprovalForAll(address operator, bool _approved) external;

    function getApproved(uint256 tokenId) external view returns (address operator);

    function isApprovedForAll(address owner, address operator) external view returns (bool);
}
```



### IERC721事件

`IERC721`包含3个事件，其中`Transfer`和`Approval`事件在`ERC20`中也有。

- `Transfer`事件：在转账时被释放，记录代币的发出地址`from`，接收地址`to`和`tokenid`。
  - 该事件具有以下参数：
    - `from`：代币的原始拥有者地址。
    - `to`：代币的新拥有者地址。
    - `tokenId`：代币的唯一标识符。
- `Approval`事件：在授权时释放，记录授权地址`owner`，被授权地址`approved`和`tokenid`。
  - 该事件具有以下参数：
    - `owner`：代币的拥有者地址。
    - `approved`：被授权地址。
    - `tokenId`：代币的唯一标识符。
- `ApprovalForAll`事件：在批量授权时释放，记录批量授权的发出地址`owner`，被授权地址`operator`和授权与否的`approved`。
  - 该事件具有以下参数：
    - `owner`：拥有者地址，设置操作员权限的地址。
    - `operator`：操作员地址，被授权执行代币操作的地址。
    - `approved`：表示操作员权限的状态，`true` 表示已授权，`false` 表示已撤销。



### IERC721函数

IERC721 接口定义了一组函数，用于实现 ERC721 非同质化代币标准。以下是一些常见的函数：

1. `balanceOf(address _owner) external view returns (uint256)`: 返回指定地址 `_owner` 拥有的代币数量。

2. `ownerOf(uint256 _tokenId) external view returns (address)`: 返回代币 ID 为 `_tokenId` 的拥有者地址。

3. `safeTransferFrom(address _from, address _to, uint256 _tokenId, bytes data) external`: 将代币 ID 为 `_tokenId` 的代币从 `_from` 地址转移给 `_to` 地址。函数会触发 `Transfer` 事件，并根据接收者地址是否是合约，执行不同的安全操作。

4. `safeTransferFrom(address _from, address _to, uint256 _tokenId) external`: 类似于上述函数，但不包含任何额外的数据。

5. `transferFrom(address _from, address _to, uint256 _tokenId) external`: 将代币 ID 为 `_tokenId` 的代币从 `_from` 地址转移给 `_to` 地址。该函数要求调用者必须具有 `_from` 地址的代币所有权或相应的操作权限。

6. `approve(address _approved, uint256 _tokenId) external`: 授权给地址 `_approved` 操作代币 ID 为 `_tokenId` 的代币。授权可以是临时的或永久的。

7. `setApprovalForAll(address _operator, bool _approved) external`: 授权或撤销地址 `_operator` 对当前地址所有代币的操作权限。参数 `_approved` 用于表示操作员权限的状态。

8. `getApproved(uint256 _tokenId) external view returns (address)`: 返回被授权操作代币 ID 为 `_tokenId` 的地址。

9. `isApprovedForAll(address _owner, address _operator) external view returns (bool)`: 检查地址 `_operator` 是否被授权操作地址 `_owner` 的所有代币。

以上是部分常用的函数，用于管理 ERC721 代币的所有权转移、授权和查询。通过这些函数，ERC721 实现了代币的独特性和不可互换性，使得每个代币都具有唯一的标识和所有者。



## IERC721Receiver

如果一个合约没有实现`ERC721`的相关函数，转入的`NFT`就进了黑洞，永远转不出来了。为了防止误转账，`ERC721`实现了`safeTransferFrom()`安全转账函数，目标合约必须实现了`IERC721Receiver`接口才能接收`ERC721`代币，不然会`revert`。`IERC721Receiver`接口只包含一个`onERC721Received()`函数。



IERC721Receiver 是一个接口，定义了用于接收 ERC721 代币的合约应实现的函数。它包含了以下函数：

1. `onERC721Received(address _operator, address _from, uint256 _tokenId, bytes _data) external returns (bytes4)`: 该函数在接收 ERC721 代币时被调用。它返回一个 4 字节的魔法值，以确认合约是否正确实现了该接口。函数的参数包括 `_operator`，表示执行代币转移的操作者地址；`_from`，表示代币的原始所有者地址；`_tokenId`，表示被转移的代币 ID；`_data`，表示可选的附加数据。返回值是一个 4 字节的函数选择器，即 `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`。

IERC721Receiver 接口的目的是为了确保合约在接收 ERC721 代币时具备相应的处理能力。实现了该接口的合约可以接收 ERC721 代币，并在接收成功后执行自定义的逻辑，例如更新合约中的数据、触发事件等。

一个合约要被其他合约接收 ERC721 代币，必须实现 IERC721Receiver 接口并正确实现 `onERC721Received` 函数。在代币转移时，如果目标地址是一个合约，并且该合约实现了 IERC721Receiver 接口，那么 `safeTransferFrom` 和 `transferFrom` 函数将调用目标合约的 `onERC721Received` 函数，以确保代币的安全转移和正确处理。如果合约没有正确实现该接口，代币转移将失败。

通过 IERC721Receiver 接口，ERC721 代币标准提供了一种规范化的方法来与其他合约进行交互，并确保代币在转移过程中的安全性和正确性。



`ERC721`利用`_checkOnERC721Received`来确保目标合约实现了`onERC721Received()`函数（返回`onERC721Received`的`selector`）：

```solidity
    function _checkOnERC721Received(
        address from,
        address to,
        uint tokenId,
        bytes memory _data
    ) private returns (bool) {
        if (to.isContract()) {
            return
                IERC721Receiver(to).onERC721Received(
                    msg.sender,
                    from,
                    tokenId,
                    _data
                ) == IERC721Receiver.onERC721Received.selector;
        } else {
            return true;
        }
    }
```

以上代码是一个内部函数 `_checkOnERC721Received`，用于检查目标地址是否是一个合约，并调用该合约的 `onERC721Received` 函数来处理 ERC721 代币的接收。

代码逻辑如下：

1. 首先判断目标地址 `to` 是否是一个合约，通过调用 `isContract()` 函数来判断。如果 `to` 是一个合约，则执行以下步骤。
2. 调用 `to` 合约的 `onERC721Received` 函数，并传递参数 `msg.sender`（即当前合约的地址），`from`（表示代币的原始所有者地址），`tokenId`（表示被转移的代币 ID），以及 `_data`（可选的附加数据）。
3. 将返回值与 `IERC721Receiver.onERC721Received.selector` 进行比较，判断是否返回的是预期的函数选择器。`IERC721Receiver.onERC721Received.selector` 是函数 `onERC721Received` 的函数选择器，用于确认目标合约是否正确实现了 `onERC721Received` 函数。
4. 如果返回的值与预期的函数选择器相等，表示目标合约正确实现了 `onERC721Received` 函数，返回 `true`。
5. 如果目标地址 `to` 不是一个合约，直接返回 `true`，表示无需进行特殊处理。

该函数的目的是确保代币的安全转移和正确处理。当代币被转移到一个合约地址时，通过调用该合约的 `onERC721Received` 函数，可以触发合约内部的特定逻辑，例如更新数据、触发事件等。通过比较返回的函数选择器，可以验证合约是否正确实现了接收代币的函数，并避免代币转移到无法处理的合约地址。如果目标地址不是合约地址，则无需进行特殊处理，直接返回 `true`。



为什么：`如果目标地址不是合约地址，则无需进行特殊处理，直接返回 true。`

如果目标地址不是一个合约地址，即目标地址是一个常规的外部账户地址，那么在代币转移过程中无需执行特殊的逻辑或回调函数。通常，ERC721 代币的接收合约的存在是为了在代币转移时触发特定的操作或记录相关的数据。因此，如果目标地址是一个常规的外部账户地址，转移代币不需要调用任何合约函数，直接完成转移即可。在这种情况下，函数 `_checkOnERC721Received` 可以直接返回 `true`，表示代币转移成功，无需进行特殊处理。





## IERC721Metadata

IERC721Metadata 接口定义了以下函数：

- `function name() external view returns (string memory)`: 获取代币的名称。
- `function symbol() external view returns (string memory)`: 获取代币的符号。
- `function tokenURI(uint256 tokenId) external view returns (string memory)`: 获取指定 tokenId 代币的元数据 URI。URI 是一个标识代币元数据的唯一资源标识符，通常是一个 URL 地址。

这些函数允许用户和应用程序查询代币的元数据信息，例如显示代币的名称和符号，获取代币的详细描述或者访问代币的图像或其他相关资源。





## ERC721主合约

### IERC165

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

/**
 * @dev ERC165标准接口, 详见
 * https://eips.ethereum.org/EIPS/eip-165[EIP].
 *
 * 合约可以声明支持的接口，供其他合约检查
 *
 */
interface IERC165 {
    /**
     * @dev 接口 IERC165 声明了一个函数 supportsInterface，该函数用于检查合约是否实现了指定的接口。
     *     函数接受一个 bytes4 类型的参数 interfaceId，表示要查询的接口标识符。
     *     如果合约实现了查询的 interfaceId，则函数返回 true，否则返回 false。
     * 规则详见：https://eips.ethereum.org/EIPS/eip-165#how-interfaces-are-identified[EIP section]
     *
     */
    function supportsInterface(bytes4 interfaceId) external view returns (bool);
}
```

上述代码定义了一个接口 `IERC165`，该接口遵循 ERC165 标准。ERC165 是一个用于合约接口识别的标准，它定义了一种方法来检查合约是否实现了特定的接口。

接口 `IERC165` 声明了一个函数 `supportsInterface`，该函数用于检查合约是否实现了指定的接口。函数接受一个 `bytes4` 类型的参数 `interfaceId`，表示要查询的接口标识符。如果合约实现了查询的 `interfaceId`，则函数返回 `true`，否则返回 `false`。





### IERC721

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

import "./IERC165.sol";

/**
 * @dev ERC721标准接口.
 */
interface IERC721 is IERC165 {
    event Transfer(address indexed from, address indexed to, uint256 indexed tokenId);
    event Approval(address indexed owner, address indexed approved, uint256 indexed tokenId);
    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);

    function balanceOf(address owner) external view returns (uint256 balance);

    function ownerOf(uint256 tokenId) external view returns (address owner);

    function safeTransferFrom(
        address from,
        address to,
        uint256 tokenId,
        bytes calldata data
    ) external;

    function safeTransferFrom(
        address from,
        address to,
        uint256 tokenId
    ) external;

    function transferFrom(
        address from,
        address to,
        uint256 tokenId
    ) external;

    function approve(address to, uint256 tokenId) external;

    function setApprovalForAll(address operator, bool _approved) external;

    function getApproved(uint256 tokenId) external view returns (address operator);

    function isApprovedForAll(address owner, address operator) external view returns (bool);
}
```

接口 `IERC721` 继承自 `IERC165` 接口，因此该接口包含了 `supportsInterface` 函数用于检查合约是否实现了指定的接口。





当一个接口继承自另一个接口时，它会继承父接口中定义的所有函数和事件。在这种情况下，`IERC721` 接口继承自 `IERC165` 接口，因此它会继承 `IERC165` 中定义的所有函数，包括 `supportsInterface` 函数。

`supportsInterface` 函数是 ERC165 标准定义的函数，用于检查合约是否实现了指定的接口。它接受一个 `bytes4` 类型的参数 `interfaceId`，表示要检查的接口标识符。如果合约实现了指定的接口，`supportsInterface` 函数会返回 `true`，否则返回 `false`。

通过继承自 `IERC165` 接口，`IERC721` 接口也具有了 `supportsInterface` 函数。这意味着任何实现了 `IERC721` 接口的合约，都必须实现 `supportsInterface` 函数，并能够通过传递不同的接口标识符来检查合约是否实现了其他接口。

在代码中，声明 `IERC721` 接口继承自 `IERC165` 接口的作用是确保实现了 `IERC721` 接口的合约具备 `supportsInterface` 函数，以便其他合约可以使用该函数来检查合约是否支持 ERC721 标准。



### IERC721Metadata

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC721Metadata {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function tokenURI(uint256 tokenId) external view returns (string memory);
}
```



### IERC721Receiver

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// ERC721接收者接口：合约必须实现这个接口来通过安全转账接收ERC721
interface IERC721Receiver {
    function onERC721Received(
        address operator,
        address from,
        uint tokenId,
        bytes calldata data
    ) external returns (bytes4);
}
```



### Address.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.1;

// Address库
library Address {
    // 利用extcodesize判断一个地址是否为合约地址
    function isContract(address account) internal view returns (bool) {
        uint size;
        assembly {
            size := extcodesize(account)
        }
        return size > 0;
    }
}
```





### String.sol

```solidity
// SPDX-License-Identifier: MIT
// OpenZeppelin Contracts (last updated v4.7.0) (utils/Strings.sol)

pragma solidity ^0.8.4;

/**
 * @dev String operations.
 */
library Strings {
    bytes16 private constant _HEX_SYMBOLS = "0123456789abcdef";
    uint8 private constant _ADDRESS_LENGTH = 20;

    /**
     * @dev Converts a `uint256` to its ASCII `string` decimal representation.
     */
    function toString(uint256 value) internal pure returns (string memory) {
        // Inspired by OraclizeAPI's implementation - MIT licence
        // https://github.com/oraclize/ethereum-api/blob/b42146b063c7d6ee1358846c198246239e9360e8/oraclizeAPI_0.4.25.sol

        if (value == 0) {
            return "0";
        }
        uint256 temp = value;
        uint256 digits;
        while (temp != 0) {
            digits++;
            temp /= 10;
        }
        bytes memory buffer = new bytes(digits);
        while (value != 0) {
            digits -= 1;
            buffer[digits] = bytes1(uint8(48 + uint256(value % 10)));
            value /= 10;
        }
        return string(buffer);
    }

    /**
     * @dev Converts a `uint256` to its ASCII `string` hexadecimal representation.
     */
    function toHexString(uint256 value) internal pure returns (string memory) {
        if (value == 0) {
            return "0x00";
        }
        uint256 temp = value;
        uint256 length = 0;
        while (temp != 0) {
            length++;
            temp >>= 8;
        }
        return toHexString(value, length);
    }

    /**
     * @dev Converts a `uint256` to its ASCII `string` hexadecimal representation with fixed length.
     */
    function toHexString(uint256 value, uint256 length) internal pure returns (string memory) {
        bytes memory buffer = new bytes(2 * length + 2);
        buffer[0] = "0";
        buffer[1] = "x";
        for (uint256 i = 2 * length + 1; i > 1; --i) {
            buffer[i] = _HEX_SYMBOLS[value & 0xf];
            value >>= 4;
        }
        require(value == 0, "Strings: hex length insufficient");
        return string(buffer);
    }

    /**
     * @dev Converts an `address` with fixed length of 20 bytes to its not checksummed ASCII `string` hexadecimal representation.
     */
    function toHexString(address addr) internal pure returns (string memory) {
        return toHexString(uint256(uint160(addr)), _ADDRESS_LENGTH);
    }
}
```

上述代码是一个名为 `Strings` 的库，提供了一些字符串操作函数。它包含以下几个函数：

1. `toString(uint256 value) internal pure returns (string memory)`: 将一个 `uint256` 类型的整数转换为十进制表示的 ASCII 字符串。该函数会将整数按照十进制形式转换为字符串。
2. `toHexString(uint256 value) internal pure returns (string memory)`: 将一个 `uint256` 类型的整数转换为十六进制表示的 ASCII 字符串。该函数会将整数按照十六进制形式转换为字符串，并在结果前添加 `"0x"` 前缀。
3. `toHexString(uint256 value, uint256 length) internal pure returns (string memory)`: 将一个 `uint256` 类型的整数转换为指定长度的十六进制表示的 ASCII 字符串。该函数会将整数按照十六进制形式转换为字符串，并在结果前添加 `"0x"` 前缀。同时，如果指定的长度不足，会在结果前补零。
4. `toHexString(address addr) internal pure returns (string memory)`: 将一个 `address` 类型的地址转换为不带校验和的 ASCII 格式的十六进制字符串。该函数会将地址转换为长度为 40 的十六进制字符串，并在结果前添加 `"0x"` 前缀。

这些函数可以在智能合约中使用，用于将整数、地址等数据类型转换为字符串形式，方便进行输出和处理。



### ERC721.sol

```solidity
// SPDX-License-Identifier: MIT
// by 0xAA
pragma solidity ^0.8.4;

import "./IERC165.sol";
import "./IERC721.sol";
import "./IERC721Receiver.sol";
import "./IERC721Metadata.sol";
import "./Address.sol";
import "./String.sol";

contract ERC721 is IERC721, IERC721Metadata{
    using Address for address; // 使用Address库，用isContract来判断地址是否为合约
    using Strings for uint256; // 使用String库，

    // Token名称
    string public override name;
    // Token代号
    string public override symbol;
    // tokenId 到 owner address 的持有人映射
    mapping(uint => address) private _owners;
    // address 到 持仓数量 的持仓量映射
    mapping(address => uint) private _balances;
    // tokenID 到 授权地址 的授权映射
    mapping(uint => address) private _tokenApprovals;
    //  owner地址。到operator地址 的批量授权映射
    mapping(address => mapping(address => bool)) private _operatorApprovals;

    /**
     * 构造函数，初始化`name` 和`symbol` .
     */
    constructor(string memory name_, string memory symbol_) {
        name = name_;
        symbol = symbol_;
    }

    // 实现IERC165接口supportsInterface
    function supportsInterface(bytes4 interfaceId)
        external
        pure
        override
        returns (bool)
    {
        return
            interfaceId == type(IERC721).interfaceId ||
            interfaceId == type(IERC165).interfaceId ||
            interfaceId == type(IERC721Metadata).interfaceId;
    }

    // 实现IERC721的balanceOf，利用_balances变量查询owner地址的balance。
    function balanceOf(address owner) external view override returns (uint) {
        require(owner != address(0), "owner = zero address");
        return _balances[owner];
    }

    // 实现IERC721的ownerOf，利用_owners变量查询tokenId的owner。
    function ownerOf(uint tokenId) public view override returns (address owner) {
        owner = _owners[tokenId];
        require(owner != address(0), "token doesn't exist");
    }

    // 实现IERC721的isApprovedForAll，利用_operatorApprovals变量查询owner地址是否将所持NFT批量授权给了operator地址。
    function isApprovedForAll(address owner, address operator)
        external
        view
        override
        returns (bool)
    {
        return _operatorApprovals[owner][operator];
    }

    // 实现IERC721的setApprovalForAll，将持有代币全部授权给operator地址。调用_setApprovalForAll函数。
    function setApprovalForAll(address operator, bool approved) external override {
        _operatorApprovals[msg.sender][operator] = approved;
        emit ApprovalForAll(msg.sender, operator, approved);
    }

    // 实现IERC721的getApproved，利用_tokenApprovals变量查询tokenId的授权地址。
    function getApproved(uint tokenId) external view override returns (address) {
        require(_owners[tokenId] != address(0), "token doesn't exist");
        return _tokenApprovals[tokenId];
    }
     
    // 授权函数。通过调整_tokenApprovals来，授权 to 地址操作 tokenId，同时释放Approval事件。
    function _approve(
        address owner,
        address to,
        uint tokenId
    ) private {
        _tokenApprovals[tokenId] = to;
        emit Approval(owner, to, tokenId);
    }

    // 实现IERC721的approve，将tokenId授权给 to 地址。条件：to不是owner，且msg.sender是owner或授权地址。调用_approve函数。
    function approve(address to, uint tokenId) external override {
        address owner = _owners[tokenId];
        require(
            msg.sender == owner || _operatorApprovals[owner][msg.sender],
            "not owner nor approved for all"
        );
        _approve(owner, to, tokenId);
    }

    // 查询 spender地址是否可以使用tokenId（需要是owner或被授权地址）
    function _isApprovedOrOwner(
        address owner,
        address spender,
        uint tokenId
    ) private view returns (bool) {
        return (spender == owner ||
            _tokenApprovals[tokenId] == spender ||
            _operatorApprovals[owner][spender]);
    }

    /*
     * 转账函数。通过调整_balances和_owner变量将 tokenId 从 from 转账给 to，同时释放Transfer事件。
     * 条件:
     * 1. tokenId 被 from 拥有
     * 2. to 不是0地址
     */
    function _transfer(
        address owner,
        address from,
        address to,
        uint tokenId
    ) private {
        require(from == owner, "not owner");
        require(to != address(0), "transfer to the zero address");

        _approve(owner, address(0), tokenId);

        _balances[from] -= 1;
        _balances[to] += 1;
        _owners[tokenId] = to;

        emit Transfer(from, to, tokenId);
    }
    
    // 实现IERC721的transferFrom，非安全转账，不建议使用。调用_transfer函数
    function transferFrom(
        address from,
        address to,
        uint tokenId
    ) external override {
        address owner = ownerOf(tokenId);
        require(
            _isApprovedOrOwner(owner, msg.sender, tokenId),
            "not owner nor approved"
        );
        _transfer(owner, from, to, tokenId);
    }

    /**
     * 安全转账，安全地将 tokenId 代币从 from 转移到 to，会检查合约接收者是否了解 ERC721 协议，以防止代币被永久锁定。调用了_transfer函数和_checkOnERC721Received函数。条件：
     * from 不能是0地址.
     * to 不能是0地址.
     * tokenId 代币必须存在，并且被 from拥有.
     * 如果 to 是智能合约, 他必须支持 IERC721Receiver-onERC721Received.
     */
    function _safeTransfer(
        address owner,
        address from,
        address to,
        uint tokenId,
        bytes memory _data
    ) private {
        _transfer(owner, from, to, tokenId);
        require(_checkOnERC721Received(from, to, tokenId, _data), "not ERC721Receiver");
    }

    /**
     * 实现IERC721的safeTransferFrom，安全转账，调用了_safeTransfer函数。
     */
    function safeTransferFrom(
        address from,
        address to,
        uint tokenId,
        bytes memory _data
    ) public override {
        address owner = ownerOf(tokenId);
        require(
            _isApprovedOrOwner(owner, msg.sender, tokenId),
            "not owner nor approved"
        );
        _safeTransfer(owner, from, to, tokenId, _data);
    }

    // safeTransferFrom重载函数
    function safeTransferFrom(
        address from,
        address to,
        uint tokenId
    ) external override {
        safeTransferFrom(from, to, tokenId, "");
    }

    /** 
     * 铸造函数。通过调整_balances和_owners变量来铸造tokenId并转账给 to，同时释放Transfer事件。铸造函数。通过调整_balances和_owners变量来铸造tokenId并转账给 to，同时释放Transfer事件。
     * 这个mint函数所有人都能调用，实际使用需要开发人员重写，加上一些条件。
     * 条件:
     * 1. tokenId尚不存在。
     * 2. to不是0地址.
     */
    function _mint(address to, uint tokenId) internal virtual {
        require(to != address(0), "mint to zero address");
        require(_owners[tokenId] == address(0), "token already minted");

        _balances[to] += 1;
        _owners[tokenId] = to;

        emit Transfer(address(0), to, tokenId);
    }

    // 销毁函数，通过调整_balances和_owners变量来销毁tokenId，同时释放Transfer事件。条件：tokenId存在。
    function _burn(uint tokenId) internal virtual {
        address owner = ownerOf(tokenId);
        require(msg.sender == owner, "not owner of token");

        _approve(owner, address(0), tokenId);

        _balances[owner] -= 1;
        delete _owners[tokenId];

        emit Transfer(owner, address(0), tokenId);
    }

    // _checkOnERC721Received：函数，用于在 to 为合约的时候调用IERC721Receiver-onERC721Received, 以防 tokenId 被不小心转入黑洞。
    function _checkOnERC721Received(
        address from,
        address to,
        uint tokenId,
        bytes memory _data
    ) private returns (bool) {
        if (to.isContract()) {
            return
                IERC721Receiver(to).onERC721Received(
                    msg.sender,
                    from,
                    tokenId,
                    _data
                ) == IERC721Receiver.onERC721Received.selector;
        } else {
            return true;
        }
    }

    /**
     * 实现IERC721Metadata的tokenURI函数，查询metadata。
     */
    function tokenURI(uint256 tokenId) public view virtual override returns (string memory) {
        require(_owners[tokenId] != address(0), "Token Not Exist");

        string memory baseURI = _baseURI();
        return bytes(baseURI).length > 0 ? string(abi.encodePacked(baseURI, tokenId.toString())) : "";
    }

    /**
     * 计算{tokenURI}的BaseURI，tokenURI就是把baseURI和tokenId拼接在一起，需要开发重写。
     * BAYC的baseURI为ipfs://QmeSjSinHpPnmXmspMjwiXyN6zS4E9zccariGR3jxcaWtq/ 
     */
    function _baseURI() internal view virtual returns (string memory) {
        return "";
    }
}
```



这段代码实现了一个 ERC721 标准的非同质代币合约。ERC721 是以太坊上用于创建和管理非同质代币（NFT）的标准接口。

下面是这段代码的主要功能和结构：

- 引入了多个接口文件，包括 IERC165、IERC721、IERC721Receiver 和 IERC721Metadata。这些接口定义了 ERC721 标准的各种函数和事件。
- 使用了 Solidity 中的库 `Address` 和 `Strings`，用于提供一些地址和字符串相关的辅助函数。
- 定义了合约 `ERC721`，实现了 `IERC721` 和 `IERC721Metadata` 接口。
- 包含了代币名称 `name` 和代号 `symbol` 的公共状态变量。
- 使用了私有映射变量 `_owners`、`_balances`、`_tokenApprovals` 和 `_operatorApprovals`，用于跟踪代币的所有权、持仓量和授权信息。
- 实现了 `supportsInterface` 函数，用于判断合约是否支持特定接口。
- 实现了 `balanceOf` 函数，用于查询给定地址的代币余额。
- 实现了 `ownerOf` 函数，用于查询给定 tokenId 的代币所有者。
- 实现了 `isApprovedForAll` 函数，用于查询给定地址是否被授权管理某个地址的所有代币。
- 实现了 `setApprovalForAll` 函数，用于将所有代币授权给某个地址。
- 实现了 `getApproved` 函数，用于查询给定 tokenId 的授权地址。
- 实现了 `approve` 函数，用于将某个 tokenId 授权给某个地址。
- 实现了 `transferFrom` 函数，用于非安全地将 tokenId 从一个地址转移到另一个地址。
- 实现了 `_safeTransfer` 函数，用于安全地将 tokenId 从一个地址转移到另一个地址，并检查接收地址是否支持 ERC721 接口。
- 实现了 `safeTransferFrom` 函数，调用了 `_safeTransfer` 函数。
- 实现了 `_mint` 函数，用于铸造新的代币并将其转移给指定地址。
- 实现了 `_burn` 函数，用于销毁指定 tokenId 的代币。
- 实现了 `_checkOnERC721Received` 函数，用于检查接收地址是否支持接收 ERC721 代币。
- 实现了 `tokenURI` 函数，用于获取指定 tokenId 的元数据 URI。
- 实现了 `_baseURI` 函数，用于计算基本的元数据 URI。

这段代码提供了一个基本的 ERC721 实现，开发人员可以在此基础上构建自己的非同质代币合约，并添加额外的逻辑和功能。



### WTFApe.sol

```solidity
// SPDX-License-Identifier: MIT
// by 0xAA
pragma solidity ^0.8.4;

import "./ERC721.sol";

contract WTFApe is ERC721{
    uint public MAX_APES = 10000; // 总量

    // 构造函数
    constructor(string memory name_, string memory symbol_) ERC721(name_, symbol_){
    }

    //BAYC的baseURI为ipfs://QmeSjSinHpPnmXmspMjwiXyN6zS4E9zccariGR3jxcaWtq/ 
    function _baseURI() internal pure override returns (string memory) {
        return "ipfs://QmeSjSinHpPnmXmspMjwiXyN6zS4E9zccariGR3jxcaWtq/";
    }
    
    // 铸造函数
    function mint(address to, uint tokenId) external {
        require(tokenId >= 0 && tokenId < MAX_APES, "tokenId out of range");
        _mint(to, tokenId);
    }
}
```

这段代码是一个名为WTFApe的智能合约，它继承了另一个智能合约ERC721。WTFApe代表了一个虚拟艺术品集合，类似于Bored Ape Yacht Club（BAYC）。

首先，合约定义了一个名为`MAX_APES`的公共变量，表示WTFApe集合中虚拟艺术品的总数量，这里设置为10000。

构造函数`constructor`调用了ERC721合约的构造函数，传递了名称和代号来初始化WTFApe。

`_baseURI`函数是内部函数，重写了ERC721合约中的同名函数。它返回虚拟艺术品的元数据的基本URI（Uniform Resource Identifier），用于构建每个虚拟艺术品的具体URI。在这个例子中，基本URI是固定的，指向一个IPFS地址，具体URI会根据每个虚拟艺术品的tokenId拼接而成。

最后，合约定义了一个名为`mint`的外部函数，用于铸造新的虚拟艺术品。这个函数接受两个参数：`to`表示新虚拟艺术品的接收地址，`tokenId`表示新虚拟艺术品的唯一标识符。在函数内部，它首先检查`tokenId`是否在合法的范围内，然后调用继承自ERC721的`_mint`函数来完成铸造过程。

总之，这段代码实现了一个简化版的虚拟艺术品合约，可以创建和管理虚拟艺术品，以及查询和操作它们的属性和所有权。



# 35. 荷兰拍卖

## 什么是荷兰拍卖

荷兰式拍卖是一种特殊的拍卖形式，也成为“减价拍卖”，是拍卖标的的竞价由高到低依次递减直到第一个竞买人应价时击槌成交的一种拍卖。

荷兰式拍卖的特点价格随着一定的时间间隔，按照事先确定的降价阶梯，由高到低进行递减；所有买受人即买到物品的人都以可以最后的竞价即所有买受人中的最低出价拍板成交。



![image-20230611180858736](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230611180858736.png)



## DutchAuction合约

...





# 36. 默克尔树

![image-20230613161047688](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230613161047688.png)

Merkle树（Merkle Tree），也称为哈希树（Hash Tree）或默克尔梅克尔树，是一种树状数据结构，用于有效地验证和验证大量数据的完整性。它是由计算机科学家Ralph Merkle在1979年提出的。

Merkle树通过对数据的分层哈希运算构建而成，其中每个叶子节点表示数据块的哈希值，而非叶子节点则表示其子节点的哈希值的哈希值。通过递归地应用哈希函数，最终可以得到一个根节点，称为Merkle根。

Merkle树的主要优势在于它可以快速且高效地验证大量数据的完整性。通过比较根节点的哈希值，可以快速确定数据是否被篡改或丢失。当数据量很大时，只需比较根节点的哈希值即可，而不需要比较所有数据块的哈希值。这使得Merkle树在分布式系统、区块链和加密货币等领域得到广泛应用。

Merkle树在区块链中扮演着重要的角色。在比特币和以太坊等区块链系统中，交易数据和状态信息被组织为Merkle树的形式，以便快速验证区块的完整性和有效性。Merkle树还被用于验证分布式存储系统中数据的一致性，例如IPFS（InterPlanetary File System）等。

总之，Merkle树是一种通过哈希运算构建的树状数据结构，用于验证和验证大量数据的完整性。它在分布式系统和区块链等领域发挥着重要作用，提供了高效和安全的数据验证机制。





## 利用MerkleTree实现白名单

```solidity
// SPDX-License-Identifier: MIT
// By 0xAA
pragma solidity ^0.8.4;

import "../34_ERC721/ERC721.sol";


/**
 * 利用Merkle树树验证白名单（生成Merkle树的网页：https://lab.miguelmota.com/merkletreejs/example/）
 * 选上Keccak-256, hashLeaves和sortPairs选项
 * 4个叶子地址：
    [
    "0x5B38Da6a701c568545dCfcB03FcB875f56beddC4", 
    "0xAb8483F64d9C6d1EcF9b849Ae677dD3315835cb2",
    "0x4B20993Bc481177ec7E8f571ceCaE8A9e22C02db",
    "0x78731D3Ca6b7E34aC0F824c42a7cC18A495cabaB"
    ]
 * 第一个地址对应的merkle proof
    [
    "0x999bf57501565dbd2fdcea36efa2b9aef8340a8901e3459f4a4c926275d36cdb",
    "0x4726e4102af77216b09ccd94f40daa10531c87c4d60bba7f3b3faf5ff9f19b3c"
    ]
 * Merkle root: 0xeeefd63003e0e702cb41cd0043015a6e26ddb38073cc6ffeb0ba3e808ba8c097
 */


/**
 * @dev 验证Merkle树的合约.
 *
 * proof可以用JavaScript库生成：
 * https://github.com/miguelmota/merkletreejs[merkletreejs].
 * 注意: hash用keccak256，并且开启pair sorting （排序）.
 * javascript例子见 `https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/test/utils/cryptography/MerkleProof.test.js`.
 */
library MerkleProof {
    /**
     * @dev 当通过`proof`和`leaf`重建出的`root`与给定的`root`相等时，返回`true`，数据有效。
     * 在重建时，叶子节点对和元素对都是排序过的。
     */
    function verify(
        bytes32[] memory proof,
        bytes32 root,
        bytes32 leaf
    ) internal pure returns (bool) {
        return processProof(proof, leaf) == root;
    }

    /**
     * @dev Returns 通过Merkle树用`leaf`和`proof`计算出`root`. 当重建出的`root`和给定的`root`相同时，`proof`才是有效的。
     * 在重建时，叶子节点对和元素对都是排序过的。
     */
    function processProof(bytes32[] memory proof, bytes32 leaf) internal pure returns (bytes32) {
        bytes32 computedHash = leaf;
        for (uint256 i = 0; i < proof.length; i++) {
            computedHash = _hashPair(computedHash, proof[i]);
        }
        return computedHash;
    }

    // Sorted Pair Hash
    function _hashPair(bytes32 a, bytes32 b) private pure returns (bytes32) {
        return a < b ? keccak256(abi.encodePacked(a, b)) : keccak256(abi.encodePacked(b, a));
    }
}

contract MerkleTree is ERC721 {
    bytes32 immutable public root; // Merkle书的根
    mapping(address => bool) public mintedAddress;   // 记录已经mint的地址

    // 构造函数，初始化NFT合集的名称、代号、Merkle树的根
    constructor(string memory name, string memory symbol, bytes32 merkleroot)
    ERC721(name, symbol)
    {
        root = merkleroot;
    }

    // 利用Merkle书验证地址并mint
    function mint(address account, uint256 tokenId, bytes32[] calldata proof)
    external
    {
        require(_verify(_leaf(account), proof), "Invalid merkle proof"); // Merkle检验通过
        require(!mintedAddress[account], "Already minted!"); // 地址没有mint过
        
        mintedAddress[account] = true; // 记录mint过的地址
        _mint(account, tokenId); // mint
    }

    // 计算Merkle书叶子的哈希值
    function _leaf(address account)
    internal pure returns (bytes32)
    {
        return keccak256(abi.encodePacked(account));
    }

    // Merkle树验证，调用MerkleProof库的verify()函数
    function _verify(bytes32 leaf, bytes32[] memory proof)
    internal view returns (bool)
    {
        return MerkleProof.verify(proof, root, leaf);
    }
}
```



这段代码是一个利用Merkle树验证白名单并进行NFT（非同质化代币）的合约。下面是对代码的解释：

1. 合约导入：代码开头导入了ERC721.sol合约，这是一个用于实现ERC721标准的合约，用于创建和管理NFT。

2. MerkleProof库：代码中定义了一个名为MerkleProof的库，用于验证Merkle树的证明。

   - `verify`函数：该函数用于验证给定的Merkle树证明（`proof`）和叶子节点的哈希值（`leaf`）是否能够重建出给定的根哈希值（`root`）。

   - `processProof`函数：该函数通过给定的Merkle树证明（`proof`）和叶子节点的哈希值（`leaf`）计算出重建的根哈希值。

   - `_hashPair`函数：该函数用于计算排序后的两个哈希值的组合哈希。

3. MerkleTree合约：该合约继承自ERC721合约，用于验证Merkle树并进行NFT的创建。

   - `root`变量：存储了Merkle树的根哈希值。

   - `mintedAddress`映射：记录已经进行NFT创建的地址。

   - 构造函数：接受合约名称、代号和Merkle树的根哈希值作为参数，并将根哈希值赋值给`root`变量。

   - `mint`函数：接受一个地址、一个NFT的tokenId和一个Merkle树证明作为参数。函数首先验证给定的Merkle树证明是否有效，然后检查该地址是否已经进行过NFT创建。如果验证通过且地址没有进行过NFT创建，则进行NFT创建，将NFT分配给该地址。

   - `_leaf`函数：接受一个地址作为参数，返回该地址的哈希值。

   - `_verify`函数：接受一个叶子节点的哈希值和一个Merkle树证明作为参数，并使用MerkleProof库中的`verify`函数验证给定的证明是否能够重建出根哈希值。

通过使用该合约，可以验证给定地址是否在白名单中，并进行相应的NFT创建操作。验证过程利用了Merkle树的性质，可以高效地验证大量数据的完整性。



# 37. 数字签名

这一讲，我们将简单的介绍以太坊中的数字签名`ECDSA`，以及如何利用它发放`NFT`白名单。代码中的`ECDSA`库由`OpenZeppelin`的同名库简化而成。







## 签名验证

签名验证是一种用于验证消息或数据完整性和身份认证的方法。在加密学中，签名通常使用公钥密码学中的数字签名算法来生成和验证。

在签名验证过程中，有两个主要的实体：签名者和验证者。签名者使用其私钥对消息进行加密，生成数字签名。验证者使用签名者的公钥和收到的消息及其对应的数字签名来验证签名的有效性。

签名验证的过程如下：

1. 签名者使用其私钥对消息进行哈希操作，然后使用私钥对哈希结果进行加密生成数字签名。
2. 签名者将消息及其对应的数字签名发送给验证者。
3. 验证者使用签名者的公钥对收到的消息进行哈希操作，得到哈希结果。
4. 验证者使用签名者的公钥和数字签名对哈希结果进行解密，得到解密后的值。
5. 验证者将解密后的值与第3步得到的哈希结果进行比较。如果两者相等，则验证通过，否则验证失败。

通过这个过程，验证者可以确认消息的完整性和身份认证，因为只有拥有私钥的签名者才能生成正确的数字签名，而其他人无法伪造签名。
