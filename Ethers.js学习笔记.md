

# 01 HelloVitalik

这一讲，介绍`ethers.js`库，javascript在线编辑器`playcode`，并且我们会写第一个程序`HelloVitalik`：查询V神的`ETH`余额，并输出在`console`中。



## ethers.js简述

ethers.js是一个用于与以太坊区块链交互的JavaScript库。它提供了一组易于使用的工具和API，使开发者能够构建以太坊智能合约、发送交易、与区块链进行交互并管理以太坊钱包。

ethers.js的主要功能包括：

1. Web3 Provider：ethers.js可以连接到以太坊网络的Web3提供者，如MetaMask、Infura等，以便与以太坊网络进行交互。
2. 钱包管理：ethers.js提供了创建、导入和管理以太坊钱包的功能。它支持生成助记词、私钥和Keystore文件，并能够签名交易和消息。
3. 智能合约开发：ethers.js提供了用于编译、部署和与以太坊智能合约进行交互的工具。它支持Solidity智能合约语言，并提供了类型安全的合约接口。
4. 交易处理：ethers.js允许创建和发送以太坊交易，并提供了对交易状态、确认数和事件的监听。
5. 事件监听：ethers.js可以监听以太坊网络上的事件，如合约事件、区块事件和日志事件。
6. 以太坊标准：ethers.js实现了以太坊的标准规范，包括EIP-20（ERC-20代币标准）、EIP-721（ERC-721非同质化代币标准）等，以便开发者能够方便地与这些标准进行交互。

ethers.js具有良好的文档和社区支持，提供了广泛的示例和教程，使开发者能够轻松地开始构建基于以太坊的应用程序。它是以太坊开发者常用的工具之一，并在以太坊生态系统中得到广泛应用。



与更早出现的`web3.js`相比，它有以下优点：

1. 代码更加紧凑：`ethers.js`大小为116.5 kB，而`web3.js`为590.6 kB。
2. 更加安全：`Web3.js`认为用户会在本地部署以太坊节点，私钥和网络连接状态由这个节点管理（实际并不是这样）；`ethers.js`中，`Provider`提供器类管理网络连接状态，`Wallet`钱包类管理密钥，安全且灵活。
3. 原生支持`ENS`。



##  开发工具



**VScode**

可以使用本地`vscode`进行开发。需要安装[Node.js](https://nodejs.org/zh-cn/download/)，然后利用包管理工具`npm`安装`ethers`库：

```shell
npm install --save ethers
```



**playcode**

[playcode](https://playcode.io/)是一个在线编译`javascript`的平台，你不需要配置`Nodejs`就可以运行`.js`文件，非常方便。且要比更知名的`codesandbox`快一百倍。





## demo

```js
import { ethers } from 'ethers';

const provider = ethers.getDefaultProvider();

const main = async () => {
  const balance = await provider.getBalance(`vitalik.eth`);

  console.log(
    `ETH Balance of vitalik: ${ethers.utils.formatEther(balance)} ETH`
  );
};

main();
```



发现vscode启动不了实例代码。



- 清空所有文件
- node.js和npm升级到最新版
- `npm install ethers@6.2.3  `
- package.json添加`module`

![image-20230626194550882](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230626194550882.png)



- hello.js

```js
// 导入ethers包
import { ethers } from "ethers";
// playcode免费版不能安装ethers，用这条命令，需要从网络上import包（把上面这行注释掉）
// import { ethers } from "https://cdnjs.cloudflare.com/ajax/libs/ethers/6.2.3/ethers.js";

// 利用ethers默认的Provider连接以太坊网络
// const provider = new ethers.getDefaultProvider();
const ALCHEMY_MAINNET_URL = 'https://eth-mainnet.g.alchemy.com/v2/oKmOQKbneVkxgHZfibs-iFhIlIAl6HDN';
const provider = new ethers.JsonRpcProvider(ALCHEMY_MAINNET_URL)

const main = async () => {
    // 查询vitalik的ETH余额
    const balance = await provider.getBalance(`vitalik.eth`);
    // 将余额输出在console
    console.log(`ETH Balance of vitalik: ${ethers.formatEther(balance)} ETH`);
}
main()
```



vscode设置中关闭`terminal.integrated.inheritEnv`

切换版本:`nvm use 你的版本`

> https://www.jianshu.com/p/3457344b8709

- node hello.js

ETH Balance of vitalik: 4119.629326576718126891 ETH



### 代码解读

1.导入`ethers`库

```js
import { ethers } from "ethers";
```

这一行代码的作用是导入已经安装好的`ethers`库



2.连接以太坊

在`ethers`中，`Provider`类是一个为以太坊网络连接提供抽象的类，它提供对区块链及其状态的只读访问。我们声明一个`provider`用于连接以太坊网络。`ethers`内置了一些公用`rpc`，方便用户连接以太坊:

```js
const ALCHEMY_MAINNET_URL = 'https://eth-mainnet.g.alchemy.com/v2/oKmOQKbneVkxgHZfibs-iFhIlIAl6HDN';
const provider = new ethers.JsonRpcProvider(ALCHEMY_MAINNET_URL)
```

**注意:**`ethers`内置的`rpc`访问速度有限制，仅测试用，生产环境还是要申请个人`rpc`。



3.声明async函数

由于和区块链交互不是实时的我们需要用到js的`async/await`语法糖。每次和链交互的调用需要用到`await`，再把这些这些用`async`函数包裹起来，最后再调用这个函数。

```javascript
const main = async () => {
    // 查询vitalik的ETH余额
    const balance = await provider.getBalance(`vitalik.eth`);
    // 将余额输出在console
    console.log(`ETH Balance of vitalik: ${ethers.formatEther(balance)} ETH`);
}
```



4.获取v神地址的`ETH`余额

```js
// 查询vitalik的ETH余额
const balance = await provider.getBalance(`vitalik.eth`);
```

可以利用`Provider`类的`getBalance()`函数来查询某个地址的`ETH`余额。由于`ethers`原生支持`ENS`域名，我们不需要知道具体地址，用`ENS`域名`vitalik.eth`就可以查询到以太坊创始人豚林-vitalik的余额。



5.转换单位后在`console`中输出

从链上获取的以太坊余额以`wei`为单位，而`1 ETH = 10^18 wei`。我们打印在`console`之前，需要进行单位转换。`ethers`提供了功能函数`formatEther`，我们可以利用它将`wei`转换为`ETH`。

```javascript
    console.log(`ETH Balance of vitalik: ${ethers.utils.formatEther(balance)} ETH`);
```





# 02 Provider 提供器



## Provider类

`Provider`类是对以太坊网络连接的抽象，为标准以太坊节点功能提供简洁、一致的接口。在`ethers`中，`Provider`不接触用户私钥，只能读取链上信息，不能写入，这一点比`web3.js`要安全。

除了[之前](https://github.com/WTFAcademy/WTFEthers)介绍的默认提供者`defaultProvider`以外，`ethers`中最常用的是`jsonRpcProvider`，可以让用户连接到特定节点服务商的节点。



> 简单来说，Provider类就是一个以太网连接的后的信息中转站，连接上后可以查各种链上信息。



## JsonRpcProvider

`JsonRpcProvider` 是 ethers.js 中的一个提供者（provider）类，用于与以太坊网络进行交互并发送 JSON-RPC 请求。

在以太坊开发中，通过 JSON-RPC 接口可以与以太坊节点进行通信，查询区块链数据、发送交易等操作。`JsonRpcProvider` 封装了与以太坊节点进行通信的逻辑，简化了与以太坊网络交互的过程。

使用 `JsonRpcProvider`，你可以创建一个连接到指定以太坊网络的提供者实例，然后通过调用它的方法来执行各种操作，例如查询余额、获取区块信息、发送交易等。



```js
const balance_ETH = await providerETH.getBalance(`vitalik.eth`);
```

```js
console.log(`Goerli ETH Balance of vitalik: ${ethers.formatEther(balanceGoerli)} ETH`);
```

使用了模板字面量，使用模板字面量而不是常规字符串引号是为了方便插入表达式，并使代码更加清晰易读。





## demo

```js
// 导入ethers包
import { ethers } from "ethers";

// 利用Alchemy的rpc节点连接以太坊网络
// 准备 alchemy API 可以参考https://github.com/AmazingAng/WTFSolidity/blob/main/Topics/Tools/TOOL04_Alchemy/readme.md
// 已打码。。用自己的
const ALCHEMY_MAINNET_URL = 'https://eth-mainnet.g.alchemy.com/v2/Ei6hUIoNaeNXvFmZqTWk3xxxxxoqC';
const ALCHEMY_GOERLI_URL = 'https://eth-goerli.g.alchemy.com/v2/bF_CjR8q5cMYmhBOXUkwjIdQgcxxxp';

// 连接以太坊主网
const providerETH = new ethers.JsonRpcProvider(ALCHEMY_MAINNET_URL);
// 连接以太坊测试网
const providerGOERLI = new ethers.JsonRpcProvider(ALCHEMY_GOERLI_URL);

const main = async () => {
    // 利用Provider读取链上信息

    // 1. 查询vitalik在主网和Goerli测试网的ETH余额
    console.log("1. 查询vitalik在主网和Goerli测试网的ETH余额");
    const balance_ETH = await providerETH.getBalance(`vitalik.eth`);
    const balance_GOERLI = await providerGOERLI.getBalance('vitalik.eth');
    // 将余额输出在console（主网）
    console.log(`ETH Balance of vitalik:${ethers.formatEther(balance_ETH)} ETH`);
    // 将余额输出在console（测试网）
    console.log(`ETH Balance of vitalik:${ethers.formatEther(balance_GOERLI)} ETH`);


    // 2. 查询provider连接到了哪条链
    console.log("\n2. 查询provider连接到了哪条链")
    const network = await providerETH.getNetwork();
    console.log(network.toJSON());

    // 3. 查询区块高度
    console.log("\n3. 查询区块高度");
    const blockNumber = await providerETH.getBlockNumber();
    console.log(blockNumber);

    // 4. 查询 vitalik 钱包历史交易次数
    console.log("\n4. 查询 vitalik 钱包历史交易次数")
    const txCount = await providerETH.getTransactionCount("vitalik.eth");
    console.log(txCount);

    // 5. 查询当前建议的gas设置
    console.log("\n5. 查询当前建议的gas设置")
    const feeData = await providerETH.getFeeData();
    console.log(feeData);

    // 6. 查询区块信息
    console.log("\n6. 查询区块信息")
    const block = await providerETH.getBlock(0);
    console.log(block);

    // 7. 给定合约地址查询合约bytecode，例子用的WETH地址
    console.log("\n7. 给定合约地址查询合约bytecode，例子用的WETH地址")
    const code = await providerETH.getCode("0xc778417e063141139fce010982780140aa0cd5ab");
    console.log(code);
}

main()
```



运行结果：

```
F:\02blockchain\wtf-etherjs>node 02_provider.js
1. 查询vitalik在主网和Goerli测试网的ETH余额
ETH Balance of vitalik:4119.629326576718126891 ETH
ETH Balance of vitalik:6.415755770611693818 ETH


2. 查询provider连接到了哪条链
{ name: 'mainnet', chainId: 1n }

3. 查询区块高度
17569573

4. 查询 vitalik 钱包历史交易次数
1097

5. 查询当前建议的gas设置
FeeData {
  gasPrice: 12685262756n,
  maxFeePerGas: 26281542012n,      
  maxPriorityFeePerGas: 1000000000n
}

6. 查询区块信息
Block {
  provider: JsonRpcProvider {},
  number: 0,
  hash: '0xd4e56740f876aef8c010b86a40d5f56745a118d0906a34e69aec8c0db1cb8fa3',
  timestamp: 0,
  parentHash: '0x0000000000000000000000000000000000000000000000000000000000000000',
  nonce: '0x0000000000000042',
  difficulty: 17179869184n,
  gasLimit: 5000n,
  gasUsed: 0n,
  miner: '0x0000000000000000000000000000000000000000',
  extraData: '0x11bbe8db4e347b4e8c937c1c8370e4b5ed33adb3db69cbdb7a38e1e50b1b82fa',
  baseFeePerGas: null
}

7. 给定合约地址查询合约bytecode，例子用的WETH地址
0x6060604052361561010f5763ffffffff7c010000000000000000000000000000000000000000000000000000000060003504166306fdde038114610111
...
10155b929150505600a165627a7a723058202c338bb3d0c86b4d03a42df2ad24f83dbd41c3d87dbcd59296451a3a0c1683c70029
```





# 03 读取合约信息

`Contract`合约类，并利用它来读取链上的合约信息。



## Contract类

在`ethers`中，`Contract`类是部署在以太坊网络上的合约（`EVM`字节码）的抽象。通过它，开发者可以非常容易的对合约进行读取`call`和交易`transcation`，并可以获得交易的结果和事件。以太坊强大的地方正是合约，所以对于合约的操作要熟练掌握。

> 简单来说，这个类就是用来和合约交互的。





## 只读和可读写Contract

`Contract`对象分为两类，只读和可读写。只读`Contract`只能读取链上合约信息，执行`call`操作，即调用合约中`view`和`pure`的函数，而不能执行交易`transaction`。创建这两种`Contract`变量的方法有所不同：

- 只读`Contract`：参数分别是合约地址，合约`abi`和`provider`变量（只读）。

```javascript
const contract = new ethers.Contract(`address`, `abi`, `provider`);
```



- 可读写`Contract`：参数分别是合约地址，合约`abi`和`signer`变量。`Signer`签名者是`ethers`中的另一个类，用于签名交易，之后我们会讲到。

```javascript
const contract = new ethers.Contract(`address`, `abi`, `signer`);
```

**注意** `ethers`中的`call`指的是只读操作，与`solidity`中的`call`不同。





## 读取合约信息

1. 创建Provider





2. 创建只读Contract实例

创建只读Contract实例需要填入`3`个参数，分别是合约地址，合约`abi`和`provider`变量。合约地址可以在网上查到，`provider`变量上一步我们已经创建了，那么`abi`怎么填？

`ABI` (Application Binary Interface) 是与以太坊智能合约交互的标准，更多内容见[WTF Solidity教程第27讲: ABI编码](https://github.com/AmazingAng/WTFSolidity/blob/main/27_ABIEncode/readme.md)。`ethers`支持两种`abi`填法：

- **方法1.** 直接输入合约`abi`。你可以从`remix`的编译页面中复制，在本地编译合约时生成的`artifact`文件夹的`json`文件中得到，或者从`etherscan`开源合约的代码页面得到。我们用这个方法创建`WETH`的合约实例：

```javascript
// 第1种输入abi的方式: 复制abi全文
// WETH的abi可以在这里复制：https://etherscan.io/token/0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2#code
const abiWETH = '[{"constant":true,"inputs":[],"name":"name","outputs":[{"name":"","type":"string"}],"payable":false,"stateMutability":"view",...太长后面省略...';
const addressWETH = '0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2' // WETH Contract
const contractWETH = new ethers.Contract(addressWETH, abiWETH, provider)
```

![image-20230627175752064](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230627175752064.png)



- **方法2.** 由于`abi`可读性太差，`ethers`创新的引入了`Human-Readable Abi`（人类可读abi）。开发者可以通过`function signature`和`event signature`来写`abi`。我们用这个方法创建稳定币`DAI`的合约实例：

```javascript
// 第2种输入abi的方式：输入程序需要用到的函数，逗号分隔，ethers会自动帮你转换成相应的abi
// 人类可读abi，以ERC20合约为例
const abiERC20 = [
    "function name() view returns (string)",
    "function symbol() view returns (string)",
    "function totalSupply() view returns (uint256)",
    "function balanceOf(address) view returns (uint)",
];
const addressDAI = '0x6B175474E89094C44Da98b954EedeAC495271d0F' // DAI Contract
const contractDAI = new ethers.Contract(addressDAI, abiERC20, provider)
```



> 直接用方法2！！！



3.读取`WETH`和`DAI`的链上信息

可以利用只读`Contract`实例调用合约的`view`和`pure`函数，获取链上信息：

```js
const main = async () => {
    // 1. 读取WETH合约的链上信息（WETH abi）
    const nameWETH = await contractWETH.name()
    const symbolWETH = await contractWETH.symbol()
    const totalSupplyWETH = await contractWETH.totalSupply()
    console.log("\n1. 读取WETH合约信息")
    console.log(`合约地址: ${addressWETH}`)
    console.log(`名称: ${nameWETH}`)
    console.log(`代号: ${symbolWETH}`)
    console.log(`总供给: ${ethers.utils.formatEther(totalSupplyWETH)}`)
    const balanceWETH = await contractWETH.balanceOf('vitalik.eth')
    console.log(`Vitalik持仓: ${ethers.utils.formatEther(balanceWETH)}\n`)

    // 2. 读取DAI合约的链上信息（IERC20接口合约）
    const nameDAI = await contractDAI.name()
    const symbolDAI = await contractDAI.symbol()
    const totalSupplDAI = await contractDAI.totalSupply()
    console.log("\n2. 读取DAI合约信息")
    console.log(`合约地址: ${addressDAI}`)
    console.log(`名称: ${nameDAI}`)
    console.log(`代号: ${symbolDAI}`)
    console.log(`总供给: ${ethers.utils.formatEther(totalSupplDAI)}`)
    const balanceDAI = await contractDAI.balanceOf('vitalik.eth')
    console.log(`Vitalik持仓: ${ethers.utils.formatEther(balanceDAI)}\n`)
}

main()
```



## demo

```js
// 声明只读合约的规则：
// 参数分别为合约地址`address`，合约ABI `abi`，Provider变量`provider`
// const contract = new ethers.Contract(`address`, `abi`, `provider`);

import { ethers } from "ethers";

// 利用Alchemy的rpc节点连接以太坊网络
const ALCHEMY_MAINNET_URL = 'https://eth-mainnet.g.alchemy.com/v2/EixxxxxxxxxxxxxxxxxxxxxxxxxxxoqC';
const provider = new ethers.JsonRpcProvider(ALCHEMY_MAINNET_URL);


// 第1种输入abi的方式: 复制abi全文
// WETH的abi可以在这里复制：https://etherscan.io/token/0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2#code
const abiWETH = '[{"constant":true,"inputs":[],"name":"name","outputs":[{"name":"","type":"string"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"guy","type":"address"},{"name":"wad","type":"uint256"}],"name":"approve","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"totalSupply","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"src","type":"address"},{"name":"dst","type":"address"},{"name":"wad","type":"uint256"}],"name":"transferFrom","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[{"name":"wad","type":"uint256"}],"name":"withdraw","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"decimals","outputs":[{"name":"","type":"uint8"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"","type":"address"}],"name":"balanceOf","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"symbol","outputs":[{"name":"","type":"string"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"dst","type":"address"},{"name":"wad","type":"uint256"}],"name":"transfer","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[],"name":"deposit","outputs":[],"payable":true,"stateMutability":"payable","type":"function"},{"constant":true,"inputs":[{"name":"","type":"address"},{"name":"","type":"address"}],"name":"allowance","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"payable":true,"stateMutability":"payable","type":"fallback"},{"anonymous":false,"inputs":[{"indexed":true,"name":"src","type":"address"},{"indexed":true,"name":"guy","type":"address"},{"indexed":false,"name":"wad","type":"uint256"}],"name":"Approval","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"name":"src","type":"address"},{"indexed":true,"name":"dst","type":"address"},{"indexed":false,"name":"wad","type":"uint256"}],"name":"Transfer","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"name":"dst","type":"address"},{"indexed":false,"name":"wad","type":"uint256"}],"name":"Deposit","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"name":"src","type":"address"},{"indexed":false,"name":"wad","type":"uint256"}],"name":"Withdrawal","type":"event"}]';
const addressWETH = '0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2' // WETH Contract
const contractWETH = new ethers.Contract(addressWETH, abiWETH, provider)


// 第2种输入abi的方式：输入程序需要用到的函数，逗号分隔，ethers会自动帮你转换成相应的abi
// 人类可读abi，以ERC20合约为例
const abiERC20 = [
    "function name() view returns (string)",
    "function symbol() view returns (string)",
    "function totalSupply() view returns (uint256)",
    "function balanceOf(address) view returns (uint)",
]

const addressDAI = '0x6B175474E89094C44Da98b954EedeAC495271d0F' // DAI Contract
const contractDAI = new ethers.Contract(addressDAI, abiERC20, provider)

const main = async () => {
    // 1. 读取WETH合约的链上信息（WETH abi）
    const nameWETH = await contractWETH.name()
    const symbolWETH = await contractWETH.symbol()
    const totalSupplyWETH = await contractWETH.totalSupply()
    console.log("\n1. 读取WETH合约信息")
    console.log(`合约地址: ${addressWETH}`)
    console.log(`名称: ${nameWETH}`)
    console.log(`代号: ${symbolWETH}`)
    console.log(`总供给: ${ethers.formatEther(totalSupplyWETH)}`)
    const balanceWETH = await contractWETH.balanceOf('vitalik.eth')
    console.log(`Vitalik持仓: ${ethers.formatEther(balanceWETH)}\n`)

    // 2. 读取DAI合约的链上信息（IERC20接口合约）
    const nameDAI = await contractDAI.name()
    const symbolDAI = await contractDAI.symbol()
    const totalSupplDAI = await contractDAI.totalSupply()
    console.log("\n2. 读取DAI合约信息")
    console.log(`合约地址: ${addressDAI}`)
    console.log(`名称: ${nameDAI}`)
    console.log(`代号: ${symbolDAI}`)
    console.log(`总供给: ${ethers.formatEther(totalSupplDAI)}`)
    const balanceDAI = await contractDAI.balanceOf('vitalik.eth')
    console.log(`Vitalik持仓: ${ethers.formatEther(balanceDAI)}\n`)
}

main();
```



```
F:\02blockchain\wtf-etherjs>node 03_contract.js

1. 读取WETH合约信息
合约地址: 0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2
名称: Wrapped Ether
代号: WETH
总供给: 3428568.83610725190618094
Vitalik持仓: 26.992040046283229929


2. 读取DAI合约信息
合约地址: 0x6B175474E89094C44Da98b954EedeAC495271d0F
名称: Dai Stablecoin
代号: DAI
总供给: 4423483116.583826703859563292
Vitalik持仓: 27183.125886000881460504
```



# 04 发送ETH

介绍`Signer`签名者类和和它派生的`Wallet`钱包类，并利用它来发送`ETH`。





## Signer签名者类

`Web3.js`认为用户会在本地部署以太坊节点，私钥和网络连接状态由这个节点管理（实际并不是这样）；而在`ethers.js`中，`Provider`提供器类管理网络连接状态，`Signer`签名者类或`Wallet`钱包类管理密钥，安全且灵活。

在`ethers`中，`Signer`签名者类是以太坊账户的抽象，可用于对消息和交易进行签名，并将签名的交易发送到以太坊网络，并更改区块链状态。`Signer`类是抽象类，不能直接实例化，我们需要使用它的子类：`Wallet`钱包类。



## Wallet钱包类

`Wallet`类继承了`Signer`类，并且开发者可以像包含私钥的外部拥有帐户（`EOA`）一样，用它对交易和消息进行签名。

下面我们介绍创建`Wallet`实例的几种办法：

### 方法1：创建随机的wallet对象

我们可以利用`ethers.Wallet.createRandom()`函数创建带有随机私钥的`Wallet`对象。该私钥由加密安全的熵源生成，如果当前环境没有安全的熵源，则会引发错误（没法在在线平台`playcode`使用此方法）。

```javascript
// 创建随机的wallet对象
const wallet1 = new ethers.Wallet.createRandom()
```



### 方法2：用私钥创建wallet对象

我们已知私钥的情况下，可以利用`ethers.Wallet()`函数创建`Wallet`对象。

```javascript
// 利用私钥和provider创建wallet对象
const privateKey = '0x227dbb8586117d55284e26620bc76534dfbd2394be34cf4a09cb775d593b6f2b'
const wallet2 = new ethers.Wallet(privateKey, provider)
```



### 方法3：从助记词创建wallet对象

我们已知助记词的情况下，可以利用`ethers.Wallet.fromMnemonic()`函数创建`Wallet`对象。

```javascript
// 从助记词创建wallet对象
const wallet3 = new ethers.Wallet.fromMnemonic(mnemonic.phrase)
```





### 其他方法：通过JSON文件创建wallet对象

以上三种方法即可满足大部分需求，当然还可以通过`ethers.Wallet.fromEncryptedJson`解密一个`JSON`钱包文件创建钱包实例，`JSON`文件即`keystore`文件，通常来自`Geth`, `Parity`等钱包，通过`Geth`搭建过以太坊节点的个人对`keystore`文件不会陌生。







## 发送ETH

我们可以利用`Wallet`实例来发送`ETH`。首先，我们要构造一个交易请求，在里面声明接收地址`to`和发送的`ETH`数额`value`。交易请求`TransactionRequest`类型可以包含发送方`from`，nonce值`nounce`，请求数据`data`等信息，之后的教程里会更详细介绍。

```javascript
    // 创建交易请求，参数：to为接收地址，value为ETH数额
    const tx = {
        to: address1,
        value: ethers.utils.parseEther("0.001")
    }
```



然后，我们就可以利用`Wallet`类的`sendTransaction`来发送交易，等待交易上链，并获得交易的收据，非常简单。

```javascript
    //发送交易，获得收据
    const receipt = await wallet2.sendTransaction(tx)
    await receipt.wait() // 等待链上确认交易
    console.log(receipt) // 打印交易详情
```





## demo



```js
// 利用Wallet类发送ETH
// 由于playcode不支持ethers.Wallet.createRandom()函数，我们只能用VScode运行这一讲代码
import { ethers } from "ethers";

// 利用Alchemy的rpc节点连接以太坊测试网络
// 准备 alchemy API 可以参考https://github.com/AmazingAng/WTFSolidity/blob/main/Topics/Tools/TOOL04_Alchemy/readme.md 
const ALCHEMY_GOERLI_URL = 'https://eth-goerli.alchemyapi.io/v2/GlaeWuylnNM3uuOo-SAwJxuwTdqHaY5l';
const provider = new ethers.JsonRpcProvider(ALCHEMY_GOERLI_URL);

// 创建随机的wallet对象
const wallet1 = ethers.Wallet.createRandom()
const wallet1WithProvider = wallet1.connect(provider)
const mnemonic = wallet1.mnemonic // 获取助记词

// 利用私钥和provider创建wallet对象
const privateKey = '0x227dbb8586117d55284e26620bc76534dfbd2394be34cf4a09cb775d593b6f2b'
const wallet2 = new ethers.Wallet(privateKey, provider)

// 从助记词创建wallet对象
const wallet3 = ethers.Wallet.fromPhrase(mnemonic.phrase)

const main = async () => {
    // 1. 获取钱包地址
    const address1 = await wallet1.getAddress()
    const address2 = await wallet2.getAddress() 
    const address3 = await wallet3.getAddress() // 获取地址
    console.log(`1. 获取钱包地址`);
    console.log(`钱包1地址: ${address1}`);
    console.log(`钱包2地址: ${address2}`);
    console.log(`钱包3地址: ${address3}`);
    console.log(`钱包1和钱包3的地址是否相同: ${address1 === address3}`);
    
    // 2. 获取助记词
    console.log(`\n2. 获取助记词`);
    console.log(`钱包1助记词: ${wallet1.mnemonic.phrase}`)
    // 注意：从private key生成的钱包没有助记词
    // console.log(wallet2.mnemonic.phrase)

    // 3. 获取私钥
    console.log(`\n3. 获取私钥`);
    console.log(`钱包1私钥: ${wallet1.privateKey}`)
    console.log(`钱包2私钥: ${wallet2.privateKey}`)

    // 4. 获取链上发送交易次数    
    console.log(`\n4. 获取链上交易次数`);
    const txCount1 = await provider.getTransactionCount(wallet1WithProvider)
    const txCount2 = await provider.getTransactionCount(wallet2)
    console.log(`钱包1发送交易次数: ${txCount1}`)
    console.log(`钱包2发送交易次数: ${txCount2}`)

    // 5. 发送ETH
    // 如果这个钱包没goerli测试网ETH了，去水龙头领一些，钱包地址: 0xe16C1623c1AA7D919cd2241d8b36d9E79C1Be2A2
    // 1. chainlink水龙头: https://faucets.chain.link/goerli
    // 2. paradigm水龙头: https://faucet.paradigm.xyz/
    console.log(`\n5. 发送ETH（测试网）`);
    // i. 打印交易前余额
    console.log(`i. 发送前余额`)
    console.log(`钱包1: ${ethers.formatEther(await provider.getBalance(wallet1WithProvider))} ETH`)
    console.log(`钱包2: ${ethers.formatEther(await provider.getBalance(wallet2))} ETH`)
    // ii. 构造交易请求，参数：to为接收地址，value为ETH数额
    const tx = {
        to: address1,
        value: ethers.parseEther("0.001")
    }
    // iii. 发送交易，获得收据
    console.log(`\nii. 等待交易在区块链确认（需要几分钟）`)
    const receipt = await wallet2.sendTransaction(tx)
    await receipt.wait() // 等待链上确认交易
    console.log(receipt) // 打印交易详情
    // iv. 打印交易后余额
    console.log(`\niii. 发送后余额`)
    console.log(`钱包1: ${ethers.formatEther(await provider.getBalance(wallet1WithProvider))} ETH`)
    console.log(`钱包2: ${ethers.formatEther(await provider.getBalance(wallet2))} ETH`)
}

main()
```



# 05 合约交互

介绍如何声明可写的`Contract`合约变量，并利用它与测试网的`WETH`合约交互。



## 创建可写Contract变量

声明可写的`Contract`变量的规则：

```js
const contract = new ethers.Contract(address, abi, signer)
```

其中`address`为合约地址，`abi`是合约的`abi`接口，`signer`是`wallet`对象。注意，这里你需要提供`signer`，而在声明可读合约时你只需要提供`provider`。



也可以利用下面的方法，将可读合约转换为可写合约：

```js
const contract2 = contract.connect(signer)
```





## 合约交互

写入合约信息，需要构建交易，并且支付`gas`。该交易将由整个网络上的每个节点以及矿工验证，并改变区块链状态。

可以用下面方法方法进行合约交互：

```js
// 发送交易
const tx = await contract.METHOD_NAME(args [, overrides])
// 等待链上确认交易
await tx.wait() 
```

其中`METHOD_NAME`为调用的函数名，`args`为函数参数，`[, overrides]`是可以选择传入的数据，包括：

- gasPrice：gas价格
- gasLimit：gas上限
- value：调用时传入的ether（单位是wei）
- nonce：nonce

**注意：** 此方法不能获取合约运行的返回值，如有需要，要使用`Solidity`事件记录，然后利用交易收据去查询。





## 例子：与测试网WETH合约交互

1.创建`provider`，`wallet`变量。

```js
import { ethers } from "ethers";

// 利用Infura的rpc节点连接以太坊网络
const INFURA_ID = '184d4c5ec78243c290d151d3f1a10f1d'
// 连接Rinkeby测试网
const provider = new ethers.providers.JsonRpcProvider(`https://rinkeby.infura.io/v3/${INFURA_ID}`)

// 利用私钥和provider创建wallet对象
const privateKey = '0x227dbb8586117d55284e26620bc76534dfbd2394be34cf4a09cb775d593b6f2b'
const wallet = new ethers.Wallet(privateKey, provider)
```



2.创建可写`WETH`合约变量，在`ABI`中加入了3个我们要调用的函数：

- `balanceOf()`：查询地址的`WETH`余额。
- `deposit()`：将转入合约的`ETH`转为`WETH`。
- `transfer()`：转账。

```js
// WETH的ABI
const abiWETH = [
    "function balanceOf(address) public view returns(uint)",
    "function deposit() public payable",
    "function transfer(address, uint) public returns (bool)",
    "function withdraw(uint) public",
]

// WETH合约地址
const addressWETH = '0xb4fbf271143f4fbf7b91a5ded31805e42b2208d6'

// 声明可写合约
const contract = new ethers.Contract(addressWETH,abiWETH,wallet);
```



3.读取账户`WETH`余额，可以看到余额为

```js
const address = await wallet.getAddress()
// 读取WETH合约的链上信息（WETH abi）
console.log("\n1. 读取WETH余额")
const balanceWETH = await contractWETH.balanceOf(address)
console.log(`存款前WETH持仓: ${ethers.utils.formatEther(balanceWETH)}\n`)
```



4.调用`WETH`合约的`deposit()`函数，将`0.001 ETH`转换为`0.001 WETH`，打印交易详情和余额。

```js
    console.log("\n2. 调用desposit()函数，存入0.001 ETH")
    // 发起交易
    const tx = await contractWETH.deposit({value: ethers.utils.parseEther("0.001")})
    // 等待交易上链
    await tx.wait()
    console.log(`交易详情：`)
    console.log(tx)
    const balanceWETH_deposit = await contractWETH.balanceOf(address)
    console.log(`存款后WETH持仓: ${ethers.utils.formatEther(balanceWETH_deposit)}\n`)
```



5.调用`WETH`合约的`transfer()`函数，给V神转账`0.001 WETH`，并打印余额。

```js
console.log("\n3. 调用transfer()函数，给vitalik转账0.001 WETH")
const tx2 = await contractWETH.transfer("vitalik.eth",ethers.parseEther("0.001"));
await tx2.wait();
const balanceWETH_transfer = await contractWETH.balanceOf(address)
console.log(`转账后WETH持仓: ${ethers.formatEther(balanceWETH_transfer)}\n`)
```



**注意**：观察`deposit()`函数和`balanceOf()`函数，为什么他们的返回值不一样？为什么前者返回一堆数据，而后者只返回确定的值？这是因为对于钱包的余额，它是一个只读操作，读到什么就是什么。而对于一次函数的调用，并不知道数据何时上链，所以只会返回这次交易的信息。总结来说，就是对于非`pure`/`view`函数的调用，会返回交易的信息。如果想知道函数执行过程中合约变量的变化，可以在合约中使用`emit`输出事件，并在返回的`transaction`信息中读取事件信息来获取相应的值。



# 06 部署合约

介绍`ethers.js`中的合约工厂`ContractFactory`类型，并利用它部署合约。







## 部署智能合约

在以太坊上，智能合约的部署是一种特殊的交易：将编译智能合约得到的字节码发送到0地址。如果这个合约的构造函数有参数的话，需要利用`abi.encode`将参数编码为字节码，然后附在在合约字节码的尾部一起发送。对于ABI编码的介绍见WTF Solidity极简教程[第27讲 ABI编码](https://github.com/AmazingAng/WTFSolidity/blob/main/27_ABIEncode/readme.md)。



## 合约工厂

`ethers.js`创造了合约工厂`ContractFactory`类型，方便开发者部署合约。你可以利用合约`abi`，编译得到的字节码`bytecode`和签名者变量`signer`来创建合约工厂实例，为部署合约做准备。

```js
const contractFactory = new ethers.ContractFactory(abi, bytecode, signer);
```



**注意**：如果合约的构造函数有参数，那么在`abi`中必须包含构造函数。

在创建好合约工厂实例之后，可以调用它的`deploy`函数并传入合约构造函数的参数`args`来部署并获得合约实例：

```js
const contract = await contractFactory.deploy(args)
```



可以利用下面两种命令，等待合约部署在链上确认，然后再进行交互。

```js
await contractERC20.deployed()
//或者 await contract.deployTransaction.wait()
```



## 例子：部署ERC20代币合约

1.创建provider和wallet变量。

```js
import { ethers } from "ethers";

// 利用Alchemy的rpc节点连接以太坊网络
// 准备 alchemy API 可以参考https://github.com/AmazingAng/WTFSolidity/blob/main/Topics/Tools/TOOL04_Alchemy/readme.md 
const ALCHEMY_GOERLI_URL = 'https://eth-goerli.alchemyapi.io/v2/GlaeWuylnNM3uuOo-SAwJxuwTdqHaY5l';
const provider = new ethers.JsonRpcProvider(ALCHEMY_GOERLI_URL);

// 利用私钥和provider创建wallet对象
const privateKey = '0x227dbb8586117d55284e26620bc76534dfbd2394be34cf4a09cb775d593b6f2b'
const wallet = new ethers.Wallet(privateKey, provider)
```



2.准备ERC20合约的字节码和ABI。因为ERC20的构造函数含有参数，因此必须把它包含在ABI中。合约的字节码可以从remix的编译面板中点击Bytecode按钮，把它复制下来，其中"object"字段对应的数据就是字节码。如果部署在链上的合约，可以在etherscan的Contract页面的Contract Creation Code中找到。

![image-20230628194213091](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230628194213091.png)



3.创建合约工厂ContractFactory实例。

`const factoryERC20 = new ethers.ContractFactory(abiERC20, bytecodeERC20, wallet);`



4.调用工厂合约的`deploy()`函数并填入构造函数的参数（代币名称和代号），部署`ERC20`代币合约并获得合约实例。你可以利用：

- `contract.address`获取合约地址，
- `contract.deployTransaction`获取部署详情，
- `contractERC20.deployed()`等待合约部署在链上确认。

```js
// 1. 利用contractFactory部署ERC20代币合约
console.log("\n1. 利用contractFactory部署ERC20代币合约")
// 部署合约，填入constructor的参数
const contractERC20 = await factoryERC20.deploy("WTF Token", "WTF")
console.log(`合约地址: ${contractERC20.address}`);
console.log("部署合约的交易详情")
console.log(contractERC20.deployTransaction)
console.log("\n等待合约部署上链")
await contractERC20.deployed()
console.log("合约已上链")
```

这段代码是用来部署一个 ERC20 代币合约，并输出一些部署相关的信息。

1. 首先，通过 `factoryERC20.deploy("QQ TOKEN","QQ")` 调用合约工厂的 `deploy` 方法来部署 ERC20 代币合约。其中 `"QQ TOKEN"` 和 `"QQ"` 是作为构造函数的参数传递给合约。
2. 在部署成功后，`contractERC20` 变量将包含已部署合约的实例。通过 `contractERC20.target` 可以获取合约的地址。
3. 使用 `contractERC20.deploymentTransaction()` 可以获取用于部署合约的交易详情，这包括交易的哈希、发送者、接收者等信息。
4. 代码输出 "等待合约部署上链"，然后通过 `contractERC20.waitForDeployment()` 等待合约部署上链。这个方法会等待合约部署交易被确认，并返回一个已部署合约的实例。
5. 最后，输出 "合约已上链" 表示合约已经成功部署并确认上链。





5.在合约上链后，调用`name()`和`symbol()`函数打印代币名称和代号，然后调用`mint()`函数给自己铸造`10,000`枚代币。

```js
// 打印合约的name()和symbol()，然后调用mint()函数，给自己地址mint 10,000代币
console.log("\n2. 调用mint()函数，给自己地址mint 10,000代币")
console.log(`合约名称: ${await contractERC20.name()}`)
console.log(`合约代号: ${await contractERC20.symbol()}`)
let tx = await contractERC20.mint("10000")
console.log("等待交易上链")
await tx.wait()
console.log(`mint后地址中代币余额: ${await contractERC20.balanceOf(wallet.address)}`)
console.log(`代币总供给: ${await contractERC20.totalSupply()}`)
```



6.调用`transfer()`函数，给V神转账`1,000`枚代币。

```js
// 3. 调用transfer()函数，给V神转账1000代币
console.log("\n3. 调用transfer()函数，给V神转账1,000代币")
tx = await contractERC20.transfer("vitalik.eth", "1000")
console.log("等待交易上链")
await tx.wait()
console.log(`V神钱包中的代币余额: ${await contractERC20.balanceOf("vitalik.eth")}`)
```





# 07 检索事件

介绍如何使用`ethers.js`读取智能合约释放的事件。如果你不了解`Solidity`的事件，可以阅读WTF Solidity极简教程中[第12讲：事件](https://github.com/AmazingAng/WTFSolidity/blob/main/12_Event/readme.md)。



## 事件 Event

智能合约释放出的事件存储于以太坊虚拟机的日志中。日志分为两个主题`topics`和数据`data`部分，其中事件哈希和`indexed`变量存储在`topics`中，作为索引方便以后搜索；没有`indexed`变量存储在`data`中，不能被直接检索，但可以存储更复杂的数据结构。

以ERC20代币中的`Transfer`转账事件为例，在合约中它是这样声明的：

```solidity
event Transfer(address indexed from, address indexed to, uint256 amount);
```



它共记录了3个变量`from`，`to`和`amount`，分别对应代币的发出地址，接收地址和转账数量，其中`from`和`to`前面带有`indexed`关键字。转账时，`Transfer`事件会被记录，可以在`etherscan`中[查到](https://rinkeby.etherscan.io/tx/0x8cf87215b23055896d93004112bbd8ab754f081b4491cb48c37592ca8f8a36c7)。

![image-20230630164209429](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230630164209429.png)



> 转账事件，EVM日志记录 事件的哈希值、from、to



## 检索事件

可以利用`Ethers`中合约类型的`queryFilter()`函数读取合约释放的事件。

```js
const transferEvents = await contract.queryFilter('事件名', 起始区块, 结束区块)
```



`queryFilter()`包含3个参数，分别是事件名（必填），起始区块（选填），和结束区块（选填）。检索结果会以数组的方式返回。

**注意**：要检索的事件必须包含在合约的`abi`中。









## demo

```js
// 检索事件的方法：
// const transferEvents = await contract.queryFilter("事件名", [起始区块高度，结束区块高度])
// 其中起始区块高度和结束区块高度为选填参数。

import { ethers } from "ethers";
// playcode免费版不能安装ethers，用这条命令，需要从网络上import包（把上面这行注释掉）
// import { ethers } from "https://cdn-cors.ethers.io/lib/ethers-5.6.9.esm.min.js";

// 利用Alchemy的rpc节点连接以太坊网络
// 准备 alchemy API 可以参考https://github.com/AmazingAng/WTFSolidity/blob/main/Topics/Tools/TOOL04_Alchemy/readme.md 
const ALCHEMY_GOERLI_URL = 'https://eth-goerli.alchemyapi.io/v2/GlaeWuylnNM3uuOo-SAwJxuwTdqHaY5l';
const provider = new ethers.JsonRpcProvider(ALCHEMY_GOERLI_URL);

// WETH ABI，只包含我们关心的Transfer事件
const abiWETH = [
    "event Transfer(address indexed from, address indexed to, uint amount)"
];

// 测试网WETH地址
const addressWETH = '0xb4fbf271143f4fbf7b91a5ded31805e42b2208d6'
// 声明合约实例
const contract = new ethers.Contract(addressWETH, abiWETH, provider)

const main = async () => {

    // 获取过去10个区块内的Transfer事件
    console.log("\n1. 获取过去10个区块内的Transfer事件，并打印出1个");
    // 得到当前block
    const block = await provider.getBlockNumber()
    console.log(`当前区块高度: ${block}`);
    console.log(`打印事件详情:`);
    const transferEvents = await contract.queryFilter('Transfer', block - 10, block)
    // 打印第1个Transfer事件
    console.log(transferEvents[0])

    // 解析Transfer事件的数据（变量在args中）
    console.log("\n2. 解析事件：")
    const amount = ethers.formatUnits(ethers.getBigInt(transferEvents[0].args["amount"]), "ether");
    console.log(`地址 ${transferEvents[0].args["from"]} 转账${amount} WETH 到地址 ${transferEvents[0].args["to"]}`)
}

main()
```



```js
    // 解析Transfer事件的数据（变量在args中）
    console.log("\n2. 解析事件：")
    const amount = ethers.formatUnits(ethers.getBigInt(transferEvents[0].args["amount"]), "ether");
    console.log(`地址 ${transferEvents[0].args["from"]} 转账${amount} WETH 到地址 ${transferEvents[0].args["to"]}`)
```

这段代码一直报错。。。





# 08 监听合约

介绍如何监听合约，并实现监听USDT合约的`Transfer`事件。





## 监听合约事件

### `contract.on`

在`ethersjs`中，合约对象有一个`contract.on`的监听方法，让我们持续监听合约的事件：

```js
contract.on("eventName", function)
```

`contract.on`有两个参数，一个是要监听的事件名称`"eventName"`，需要包含在合约`abi`中；另一个是我们在事件发生时调用的函数。



### contract.once

合约对象有一个`contract.once`的监听方法，让我们只监听一次合约释放事件，它的参数与`contract.on`一样：

```js
contract.once("eventName", function)
```



## 监听USDT合约



```js
import { ethers } from "ethers";

// 利用Alchemy的rpc节点连接以太坊网络
// 获取Provider
const ALCHEMY_GOERLI_URL = 'https://eth-goerli.g.alchemy.com/v2/bF_CjR8q5cMYmhBOXUkwjIdQgRu2iEup';
const provider = new ethers.JsonRpcProvider(ALCHEMY_GOERLI_URL);



// 测试网USDT地址
const address = '0xb4fbf271143f4fbf7b91a5ded31805e42b2208d6'

// USDT abi
const abiUSDT = [
    "event Transfer(address indexed from, address indexed to, uint256 value)"
];

// 生成USDT合约实例
const contractUSDT = new ethers.Contract(address,abiUSDT,provider);

const main = async () =>{
    // 只监听一次
    console.log("\n1. 利用contract.once()，监听一次Transfer事件");
    contractUSDT.once('Transfer', (from, to, value)=>{
        // 打印结果
        console.log(
            `${from} -> ${to} ${ethers.formatUnits(ethers.getBigInt(value),6)}`
        )
    })

    // 持续监听USDT合约
    console.log("\n2. 利用contract.on()，持续监听Transfer事件");
    contractUSDT.on("Transfer",(from, to, value)=>{
        // 打印结果
        console.log(
            `${from} -> ${to} ${ethers.formatUnits(ethers.getBigInt(value),6)}`
        )
    });
}

main();
```



## 总结

这一讲，介绍了`ethers`中最简单的链上监听功能，`contract.on()`和`contract.once()`。通过上述方法，可以监听指定合约的指定事件。





# 09 事件过滤

在监听合约的基础上，监听的过程中增加过滤器，监听指定地址的转入转出。



## 过滤器

在 ethers.js 中，过滤器（Filter）用于从以太坊区块链中检索和筛选特定的事件和日志记录。过滤器允许你订阅合约事件、查询历史事件，并根据你指定的条件过滤结果。

ethers.js 提供了几种类型的过滤器，包括合约事件过滤器（Event Filter）和日志过滤器（Log Filter）。这些过滤器可以用于监听和查询事件，以及按照一定的条件过滤和检索相关数据。

以下是一些常用的过滤器方法和用法：

1. 合约事件过滤器（Event Filter）：合约事件过滤器用于订阅和监听合约中的特定事件。你可以指定事件名称、合约地址和其他条件来创建过滤器，并监听事件的触发。

   ```javascript
   const contract = new ethers.Contract(contractAddress, abi, provider);
   
   // 创建合约事件过滤器
   const filter = contract.filters.EventName(arg1, arg2, ...);
   
   // 监听事件
   contract.on(filter, (eventArgs) => {
     // 处理事件数据
   });
   ```

2. 日志过滤器（Log Filter）：日志过滤器用于查询和检索符合条件的日志记录。你可以指定日志的主题、地址和其他条件来创建过滤器，并使用 `provider.getLogs` 方法查询相关的日志记录。

   ```javascript
   const filter = {
     address: contractAddress,
     topics: [topic1, topic2, ...],
     fromBlock: startBlock,
     toBlock: endBlock,
   };
   
   // 查询日志记录
   const logs = await provider.getLogs(filter);
   
   // 处理日志记录
   logs.forEach((log) => {
     // 处理日志数据
   });
   ```

过滤器提供了一种强大的机制来监听和检索以太坊网络中的事件和日志记录。你可以根据自己的需求创建和配置过滤器，以便按照特定的条件获取所需的数据。请参考 ethers.js 文档以获取更详细的信息和示例。



当合约创建日志（释放事件）时，它最多可以包含[4]条数据作为索引（`indexed`）。索引数据经过哈希处理并包含在[布隆过滤器](https://en.wikipedia.org/wiki/Bloom_filter)中，这是一种允许有效过滤的数据结构。因此，一个事件过滤器最多包含`4`个主题集，每个主题集是个条件，用于筛选目标事件。规则：

- 如果一个主题集为`null`，则该位置的日志主题不会被过滤，任何值都匹配。
- 如果主题集是单个值，则该位置的日志主题必须与该值匹配。
- 如果主题集是数组，则该位置的日志主题至少与数组中其中一个匹配。



## 构建过滤器

`ethers.js`中的合约类提供了`contract.filters`来简化过滤器的创建：

```js
const filter = contract.filters.EVENT_NAME( ...args ) 
```



其中`EVENT_NAME`为要过滤的事件名，`..args`为主题集/条件。前面的规则有一点抽象，下面举几个例子。

1. 过滤来自`myAddress`地址的`Transfer`事件

   ```js
   contract.filters.Transfer(myAddress)
   ```

   

2. 过滤所有发给 `myAddress`地址的`Transfer`事件

   ```js
   contract.filters.Transfer(null, myAddress)
   ```

   

3. 过滤所有从 `myAddress`发给`otherAddress`的`Transfer`事件

   ```js
   contract.filters.Transfer(myAddress, otherAddress)
   ```

   

4. 过滤所有发给`myAddress`或`otherAddress`的`Transfer`事件

   ```js
   contract.filters.Transfer(null, [ myAddress, otherAddress ])
   ```





## 监听交易所的USDT转账

1.从币安交易所转出USDT的交易

监听USDT合约之前，我们需要先看懂交易日志`Logs`，包括事件的`topics`和`data`。我们先找到一笔从币安交易所转出USDT的交易，然后通过hash在etherscan查它的详情：

交易哈希：[0xab1f7b575600c4517a2e479e46e3af98a95ee84dd3f46824e02ff4618523fff5](https://etherscan.io/tx/0xab1f7b575600c4517a2e479e46e3af98a95ee84dd3f46824e02ff4618523fff5)



![image-20230701181740416](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230701181740416.png)



2.创建`provider`，`abi`，和`USDT`合约变量：

```js
const provider = new ethers.providers.JsonRpcProvider(ALCHEMY_MAINNET_URL);
// 合约地址
const addressUSDT = '0xdac17f958d2ee523a2206206994597c13d831ec7'
// 交易所地址
const accountBinance = '0x28C6c06298d514Db089934071355E5743bf21d60'
// 构建ABI
const abi = [
  "event Transfer(address indexed from, address indexed to, uint value)",
  "function balanceOf(address) public view returns(uint)",
];
// 构建合约对象
const contractUSDT = new ethers.Contract(addressUSDT, abi, provider);
```



3.读取币安热钱包USDT余额。可以看到，当前币安热钱包有8亿多枚USDT

```js
const balanceUSDT = await contractUSDT.balanceOf(accountBinance)
console.log(`USDT余额: ${ethers.utils.formatUnits(ethers.BigNumber.from(balanceUSDT),6)}\n`)
```



4.创建过滤器，监听`USDT`转入币安的事件。

```js
    // 2. 创建过滤器，监听转移USDT进交易所
    console.log("\n2. 创建过滤器，监听USDT转进交易所")
    let filterBinanceIn = contractUSDT.filters.Transfer(null, accountBinance);
    console.log("过滤器详情：")
    console.log(filterBinanceIn);
    contractUSDT.on(filterBinanceIn,(res)=>{
        console.log('---------监听USDT进入交易所--------');
        console.log(
            `${res.args[0]} -> ${res.args[1]} ${ethers.formatUnits(res.args[2],6)}`
        )
    });
```

在给定的代码中，`res`代表监听到的合约事件的参数。当符合过滤器条件的合约事件被触发时，代码中的回调函数将被执行，并将事件参数作为参数传递给回调函数。

在这种情况下，过滤器 `filterBinanceIn` 用于监听 USDT 转账交易中的转入操作，目标地址是 `accountBinance`。当满足条件的转账交易发生时，合约事件被触发，执行回调函数，并将事件参数 `res` 作为参数传递进去。

在回调函数中，你可以根据需要处理事件参数 `res` 的数据。根据代码中的打印语句，`res.args[0]` 表示转账交易的发送方地址，`res.args[1]` 表示接收方地址，`res.args[2]` 表示转账金额。这些参数可以根据实际需求进行处理和使用。

请注意，具体的事件参数和参数顺序可能根据合约定义和事件声明而有所不同。你需要根据合约的事件定义和文档来确定参数的正确顺序和含义。



5.创建过滤器，监听`USDT`转出币安的交易。

```js
 // 3. 创建过滤器，监听交易所转出USDT
    let filterToBinanceOut = contractUSDT.filters.Transfer(accountBinance);
    console.log("\n3. 创建过滤器，监听USDT转出交易所")
    console.log("过滤器详情：")
    console.log(filterToBinanceOut);
    contractUSDT.on(filterToBinanceOut, (res) => {
      console.log('---------监听USDT转出交易所--------');
      console.log(
        `${res.args[0]} -> ${res.args[1]} ${ethers.formatUnits(res.args[2],6)}`
      )
    }
    );
```





## 总结

介绍了事件过滤器，并用它监听了与币安交易所热钱包相关的`USDT`交易。可以用它监听任何你感兴趣的交易，发现`smart money`做了哪些新交易，`NFT`大佬冲了哪些新项目，等等。



# 10 BigNumber和单位转换



## BigNumber

JavaScript整数的安全值（js中最大安全整数为9007199254740991），而以太坊的许多计算都超过这个。因此，有了`BigNumber`来处理任何大小的数。

在`ethers.js`中，大多数需要返回值的操作将返回`BigNumber`，而接受值的参数也会接受它们。



### 创建BigNumber实例

`ethers.BigNumber.from()`函数可以将将string，number，BigNumber等类型转换为BigNumber

**注意**，超过js最大安全整数的数值将不能转换。

```js
import {ethers} from "ethers";

const oneGwei = ethers.BigNumber.from("1000000000"); // 从十进制字符串生成
console.log(oneGwei)
console.log(ethers.BigNumber.from("0x3b9aca00")) // 从hex字符串生成
console.log(ethers.BigNumber.from(1000000000)) // 从数字生成
// 不能从js最大的安全整数之外的数字生成BigNumber，下面代码会报错
// ethers.BigNumber.from(Number.MAX_SAFE_INTEGER);
console.log("js中最大安全整数：", Number.MAX_SAFE_INTEGER)
```

在 ethers.js 版本 5.x 中，`ethers.BigNumber.from()` 函数是可用的。但在升级到 ethers.js 版本 6.x 后，`ethers.BigNumber.from()` 函数已被移除。



使用` ethers.getBigInt()`

```js
// 1. BigNumber
import {ethers} from "ethers";

console.group('\n1. BigNumber类');

const oneGwei = ethers.getBigInt("1000000000"); // 从十进制字符串生成
console.log(oneGwei)
console.log(ethers.getBigInt("0x3b9aca00")) // 从hex字符串生成
console.log(ethers.getBigInt(1000000000)) // 从数字生成
// 不能从js最大的安全整数之外的数字生成BigNumber，下面代码会报错
// ethers.getBigInt(Number.MAX_SAFE_INTEGER);
console.log("js中最大安全整数：", Number.MAX_SAFE_INTEGER)
```



### BigNumber运算

```js
// 运算
console.log("加法：", oneGwei + 1n)
console.log("减法：", oneGwei - 1n)
console.log("乘法：", oneGwei * 2n)
console.log("除法：", oneGwei / 2n)
// 比较
console.log("是否相等：", oneGwei == 1000000000n)
```



## 单位转换

在应用中，我们经常将数值在用户可读的字符串（以`ether`为单位）和机器可读的数值（以`wei`为单位）之间转换。例如，钱包可以为用户界面指定余额（以`ether`为单位）和`gas`价格（以`gwei`为单位），但是在发送交易时，两者都必须转换成以`wei`为单位的数值。`ethers.js`提供了一些功能函数，方便这类转换。

> 都是以wei来计算的！！！



### 格式化：小单位转大单位

 例如将wei转换为ether：`formatUnits(变量, 单位)`：单位填位数（数字）或指定的单位（字符串）



```js
// 2. 格式化：小单位转大单位
// 例如将wei转换为ether：formatUnits(变量, 单位)：单位填位数（数字）或指定的单位（字符串）
console.group('\n2. 格式化：小单位转大单位，formatUnits');
console.log(ethers.formatUnits(oneGwei, 0));
// '1000000000'
console.log(ethers.formatUnits(oneGwei, "gwei"));
// '1.0'
console.log(ethers.formatUnits(oneGwei, 9));
// '1.0'
console.log(ethers.formatUnits(oneGwei, "ether"));
// `0.000000001`
console.log(ethers.formatUnits(1000000000, "gwei"));
// '1.0'
console.log(ethers.formatEther(oneGwei));
// `0.000000001` 等同于formatUnits(value, "ether")
console.groupEnd();
```



###  解析：大单位转小单位

例如将ether转换为wei：`parseUnits(变量, 单位)`,parseUnits默认单位是 ether

```js
console.group('\n3. 解析：大单位转小单位，parseUnits');
console.log(ethers.parseUnits("1.0").toString());
// { BigNumber: "1000000000000000000" }
console.log(ethers.parseUnits("1.0", "ether").toString());
// { BigNumber: "1000000000000000000" }
console.log(ethers.parseUnits("1.0", 18).toString());
// { BigNumber: "1000000000000000000" }
console.log(ethers.parseUnits("1.0", "gwei").toString());
// { BigNumber: "1000000000" }
console.log(ethers.parseUnits("1.0", 9).toString());
// { BigNumber: "1000000000" }
console.log(ethers.parseEther("1.0").toString());
// { BigNumber: "1000000000000000000" } 等同于parseUnits(value, "ether")
console.groupEnd();
```





# 11 CallStatic

介绍合约类的`callStatic`方法，在发送交易之前检查交易是否会失败，节省大量gas。

`callStatic`方法是属于`ethers.Contract`类的编写方法分析，同类的还有`populateTransaction`和`estimateGas`方法。



> 通过模拟交易，来估算本次交易是否成功







## 可能失败的交易

在以太坊上发交易需要付昂贵的`gas`，并且有失败的风险，发送失败的交易并不会把`gas`返还给你。因此，在发送交易前知道哪些交易可能会失败非常重要。

它是怎么做到的呢？这是因为以太坊节点有一个`eth_call`方法，让用户可以模拟一笔交易，并返回可能的交易结果，但不真正在区块链上执行它（交易不上链）。



## callStatic

在 ethers.js 中，`callStatic` 是一个用于执行合约函数的方法，它允许你在本地调用合约函数，而无需发送交易到区块链网络。这个方法主要用于读取合约的状态或执行不修改区块链状态的操作。

使用 `callStatic` 方法时，需要提供合约实例的方法名称和参数。它将模拟执行合约函数，并返回一个 Promise，该 Promise 在执行完成后将返回一个结果。这个结果是合约函数在模拟执行期间返回的值。



在`ethers.js`中你可以利用`contract`对象的`callStatic()`来调用以太坊节点的`eth_call`。如果调用成功，则返回`ture`；如果失败，则报错并返回失败原因。方法：

```js
    const tx = await contract.callStatic.函数名( 参数, {override})
    console.log(`交易会成功吗？：`, tx)
```



- 函数名：为模拟调用的函数名。
- 参数：调用函数的参数。
- {override}：选填，可包含一下参数：
  - `from`：执行时的`msg.sender`，也就是你可以模拟任何一个人的调用，比如V神。
  - `value`：执行时的`msg.value`。
  - `blockTag`：执行时的区块高度。
  - `gasPrice`
  - `gasLimit`
  - `nonce`



在 ethers.js v5 版本中，静态调用合约函数使用的是 `callStatic` 方法，而在 ethers.js v6 版本中，它被重命名为 `callStatic`。

在 ethers.js v5 版本中，调用合约函数的静态方法是 `callStatic`。例如：

```javascript
// ethers.js v5
const result = await contract.callStatic.functionName(...args);
```

然而，在 ethers.js v6 版本中，为了提供更一致的编程体验，该方法被重命名为 `staticCall`。示例如下：

```javascript
// ethers.js v6
const result = await contract.functionName.staticCall(...args);
```

需要注意的是，这只是一个方法名称的变化，ethers.js v6 中的 `staticCall` 方法与 v5 中的 `callStatic` 方法具有相同的功能。重命名是为了提高代码的一致性和易读性。





## 用callStatic模拟DAI转账



1.创建智能合约实例

```js
// contract.函数名.staticCall(参数, {override})
import { ethers } from "ethers";

//准备 alchemy API 可以参考https://github.com/AmazingAng/WTFSolidity/blob/main/Topics/Tools/TOOL04_Alchemy/readme.md
const ALCHEMY_MAINNET_URL = 'https://eth-mainnet.g.alchemy.com/v2/oKmOQKbneVkxgHZfibs-iFhIlIAl6HDN';
const provider = new ethers.JsonRpcProvider(ALCHEMY_MAINNET_URL);

// 利用私钥和provider创建wallet对象
const privateKey = '0x227dbb8586117d55284e26620bc76534dfbd2394be34cf4a09cb775d593b6f2b'
const wallet = new ethers.Wallet(privateKey, provider)

// DAI的ABI
const abiDAI = [
    "function balanceOf(address) public view returns(uint)",
    "function transfer(address, uint) public returns (bool)",
];

// DAI合约地址（主网）
const addressDAI = '0x6B175474E89094C44Da98b954EedeAC495271d0F' // DAI Contract

// 创建DAI合约实例
const contractDAI = new ethers.Contract(addressDAI, abiDAI, provider)
```



2.读取DAI合约的链上信息

```js
    //获取钱包的地址
    const address = await wallet.getAddress()

    // 1. 读取DAI合约的链上信息
    console.log("\n1. 读取测试钱包的DAI余额")
    const balanceDAI = await contractDAI.balanceOf(address)
    const balanceDAIVitalik = await contractDAI.balanceOf("vitalik.eth")
    console.log(`测试钱包 DAI持仓: ${ethers.formatEther(balanceDAI)}\n`)
    console.log(`vitalik DAI持仓: ${ethers.formatEther(balanceDAIVitalik)}\n`)
```



3.用staticCall尝试调用transfer转账10000 DAI，msg.sender为V神，交易将成功

```js
    console.log("\n2.  用staticCall尝试调用transfer转账1 DAI，msg.sender为V神地址")
    // 发起交易
    const tx = await contractDAI.transfer.staticCall("vitalik.eth",
        ethers.parseEther("10000"),
        {from: "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"}
    );
    console.log(`交易会成功吗？：`, tx)
```



4. 用staticCall尝试调用transfer转账10000 DAI，msg.sender为测试钱包地址，交易将失败

```js
    console.log("\n3.  用staticCall尝试调用transfer转账1 DAI，msg.sender为测试钱包地址")
    const tx2 = await contractDAI.transfer.staticCall("vitalik.eth", ethers.parseEther("10000"), {from: address})
    console.log(`交易会成功吗？：`, tx)
```







## 总结

`ethers.js`将`eth_call`封装在`callStatic`方法中，方便开发者模拟交易的结果，并避免发送可能失败的交易。我们利用`callStatic`模拟了V神和测试钱包的转账。当然，这个方法还有更多用处，比如计算土狗币的交易滑点。开动你的想象力，你会将`callStatic`用在什么地方呢？

> 模拟转账？？？



# 12 识别ERC721合约



## ERC721

简单来说就是以太坊上NFT的标准



## ERC165

ERC-165是以太坊上的一种接口标准，用于合约之间的接口兼容性和功能检查。它定义了一种标准的接口查询机制，允许合约在运行时确定其他合约是否实现了特定的接口。

ERC-165标准规定了两个主要的函数：`supportsInterface`和`interfaceId`。

1. `supportsInterface(bytes4 interfaceId) external view returns (bool)`：该函数用于检查合约是否支持特定的接口。它接收一个4字节的接口ID作为参数，并返回一个布尔值，指示合约是否支持该接口。如果合约支持给定的接口，函数返回`true`，否则返回`false`。

2. `interfaceId()`：该函数返回合约实现的接口ID。接口ID是一个4字节的哈希值，用于唯一标识一个接口。合约可以通过重写`interfaceId()`函数来指定自己所实现的接口ID。

ERC-165标准的主要目的是提供一种机制，使合约能够在运行时检查其他合约是否实现了特定的接口。这样一来，合约之间就可以进行接口兼容性检查，从而更好地进行交互和集成。通过使用`supportsInterface`函数，合约可以确定其他合约是否具有所需的功能，并相应地调整其行为。

通过使用ERC-165标准，合约可以实现更灵活和可扩展的交互模式，以及更好地支持模块化和可插拔的设计。它提供了一种标准化的方式来定义和查询接口，从而促进了智能合约之间的互操作性和可组合性。





## 识别ERC721



1.创建智能合约实例

```js
import { ethers } from "ethers";

//准备 alchemy API 可以参考https://github.com/AmazingAng/WTFSolidity/blob/main/Topics/Tools/TOOL04_Alchemy/readme.md
const ALCHEMY_MAINNET_URL = 'https://eth-mainnet.g.alchemy.com/v2/oKmOQKbneVkxgHZfibs-iFhIlIAl6HDN';
const provider = new ethers.JsonRpcProvider(ALCHEMY_MAINNET_URL);

//合约abi
const abiERC721 =[
    "function name() view returns (string)",
    "function symbol() view returns (string)",
    "function supportsInterface(bytes4) public view returns(bool)",
]
// ERC721的合约地址，这里用的BAYC
const addressBAYC = "0xbc4ca0eda7647a8ab7c2061c2e118a18a936f13d"

//创建智能合约实例
const contractERC721 = new ethers.Contract(addressBAYC,abiERC721,provider);
```



2.读取ERC721合约的链上信息

```js
const nameERC721 = await contractERC721.name()
const symbolERC721 = await contractERC721.symbol()
console.log("\n1. 读取ERC721合约信息")
console.log(`合约地址: ${addressBAYC}`)
console.log(`名称: ${nameERC721}`)
console.log(`代号: ${symbolERC721}`)
```



3.利用ERC165的supportsInterface，确定合约是否为ERC721标准



`ERC721`合约中会实现`IERC165`接口合约的`supportsInterface`函数，并且当查询`0x80ac58cd`（`ERC721`接口id）时返回`true`。



可以在对应页面，用`INTERFACE`来查询接口id

![image-20230704185732605](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230704185732605.png)

[Bored Ape Yacht Club: BAYC Token | Address 0xbc4ca0eda7647a8ab7c2061c2e118a18a936f13d | Etherscan](https://etherscan.io/address/0xbc4ca0eda7647a8ab7c2061c2e118a18a936f13d#code)

`bytes4 private constant _INTERFACE_ID_ERC721 = 0x80ac58cd;`

```js
const isERC721 = await contractERC721.supportsInterface(selectorERC721);
console.log("\n2. 利用ERC165的supportsInterface，确定合约是否为ERC721标准")
console.log(`合约是否为ERC721标准: ${isERC721}`)
```





完整代码：

```js
import { ethers } from "ethers";

//准备 alchemy API 可以参考https://github.com/AmazingAng/WTFSolidity/blob/main/Topics/Tools/TOOL04_Alchemy/readme.md
const ALCHEMY_MAINNET_URL = 'https://eth-mainnet.g.alchemy.com/v2/oKmOQKbneVkxgHZfibs-iFhIlIAl6HDN';
const provider = new ethers.JsonRpcProvider(ALCHEMY_MAINNET_URL);

//合约abi
const abiERC721 =[
    "function name() view returns (string)",
    "function symbol() view returns (string)",
    "function supportsInterface(bytes4) public view returns(bool)",
]
// ERC721的合约地址，这里用的BAYC
const addressBAYC = "0xbc4ca0eda7647a8ab7c2061c2e118a18a936f13d"

//创建智能合约实例
const contractERC721 = new ethers.Contract(addressBAYC,abiERC721,provider);


// ERC721接口的ERC165 identifier
const selectorERC721 = "0x80ac58cd"

const main = async ()=>{
    try{
        // 1. 读取ERC721合约的链上信息
        const nameERC721 = await contractERC721.name()
        const symbolERC721 = await contractERC721.symbol()
        console.log("\n1. 读取ERC721合约信息")
        console.log(`合约地址: ${addressBAYC}`)
        console.log(`名称: ${nameERC721}`)
        console.log(`代号: ${symbolERC721}`)

        // 2. 利用ERC165的supportsInterface，确定合约是否为ERC721标准
        const isERC721 = await contractERC721.supportsInterface(selectorERC721);
        console.log("\n2. 利用ERC165的supportsInterface，确定合约是否为ERC721标准")
        console.log(`合约是否为ERC721标准: ${isERC721}`)

    }catch (e) {
        // 如果不是ERC721，则会报错
        console.log(e);
    }
}

main();
```



运行结果：

```js
1. 读取ERC721合约信息
合约地址: 0xbc4ca0eda7647a8ab7c2061c2e118a18a936f13d
名称: BoredApeYachtClub
代号: BAYC

2. 利用ERC165的supportsInterface，确定合约是否为ERC721标准
合约是否为ERC721标准: true
```





## 总结

只要实现了ERC165的接口，都能用上面的方法查是否存在某个接口。

但是，ERC20没实现ERC165的接口，因此不行。





# 13 编码calldata

介绍`ethers.js`中的接口类，并利用它编码`calldata`。



## 合约调用数据

合约调用数据是一个字节数组，用于指定要调用合约的特定函数及其参数。它的结构如下：

```
<函数选择器><参数1><参数2>...
```

- 函数选择器（Function Selector）是一个用于标识函数的唯一标识符。它是函数名称和参数类型的哈希值的前四个字节。通过函数选择器，以太坊可以识别出要调用的是合约中的哪个函数。

- 参数是函数调用的输入数据。每个参数都按照其类型进行编码，并按顺序排列在函数选择器后面。

通过将函数选择器和参数编码为字节数组，我们可以创建合约调用数据，以便在以太坊上执行合约的特定函数。

在上述代码中，`contractWETH.interface.encodeFunctionData` 方法用于生成合约调用数据。它接收函数名称和参数，并将它们编码为字节数组，以便发送给合约进行调用。生成的合约调用数据将包含要调用的函数选择器和参数编码。





## 接口类 Interface

在 ethers.js 中，接口类（Interface class）是一个用于处理智能合约的 ABI（Application Binary Interface）的工具。它允许你根据智能合约的 ABI 定义来解析和编码交易数据、调用函数以及解析事件日志。

接口类提供了以下主要功能：

1. 解析函数调用：接口类可以根据智能合约的 ABI 定义解析函数调用数据。通过提供函数名称和参数值，你可以使用接口类的方法生成正确的函数调用数据。这对于构建和签署交易以及调用智能合约函数非常有用。

2. 解析事件日志：接口类可以解析智能合约的事件日志数据。你可以通过提供事件名称和事件日志数据，使用接口类的方法解析事件的参数值。这对于监听和处理智能合约的事件非常有用。

3. 编码和解码数据：接口类可以根据指定的类型定义编码和解码数据。你可以使用接口类的方法将数据从 JavaScript 对象编码为字节数组，或者将字节数组解码为 JavaScript 对象。这在处理复杂数据类型时非常有用，如结构体、动态数组等。

使用接口类需要提供智能合约的 ABI 定义，它描述了智能合约的函数、事件以及它们的参数类型和顺序。通过加载合约的 ABI 定义并创建接口类的实例，你可以轻松地与智能合约进行交互。

以下是 ethers.js 接口类的基本用法示例：

```javascript
const { ethers } = require('ethers');

// 定义智能合约的 ABI
const abi = [
  // 函数定义...
  // 事件定义...
];

// 创建接口类实例
const contractInterface = new ethers.utils.Interface(abi);

// 解析函数调用
const functionName = 'transfer';
const functionArgs = ['0x0123456789abcdef', 100];
const functionData = contractInterface.encodeFunctionData(functionName, functionArgs);
console.log('函数调用数据:', functionData);

// 解析事件日志
const eventName = 'Transfer';
const eventData = {
  address: '0xabcdef0123456789',
  topics: ['0x0123456789abcdef', '0xabcdef0123456789'],
  data: '0x0123456789abcdef',
};
const eventParsedData = contractInterface.parseLog(eventData);
console.log('事件解析结果:', eventParsedData);

// 编码和解码数据
const dataType = 'uint256';
const value = 100;
const encodedData = contractInterface.encode([dataType], [value]);
console.log('编码后的数据:', encodedData);
const decodedData = contractInterface.decode([dataType], encodedData);
console.log('解码后的数据:', decodedData);
```

请注意，上述示例中的 `abi` 变量是一个包含智能合约的 ABI 定义的数组。你需要替换为你所使用的智能合约的实际 ABI。

总而言之，ethers.js 的接口类提供了

便捷的方法来处理智能合约的 ABI，包括函数调用、事件解析和数据编码/解码等操作。它简化了与智能合约的交互过程，并提供了一致性和类型安全性。





使用接口类的好处包括：

1. 类型安全：通过使用接口类，可以在编译时进行类型检查，确保函数调用的正确性。接口类提供了对合约函数的类型定义，可以避免在调用合约函数时传递错误的参数或返回值类型。
2. 代码提示：接口类可以为合约函数提供代码提示和自动补全功能。在开发环境中，通过接口类可以获得函数名称、参数和返回值的提示，提高开发效率。
3. ABI 解析：接口类可以解析合约的 ABI，根据函数名称和参数生成函数调用数据。这样，开发人员无需手动编写和处理函数调用数据，减少了出错的可能性。
4. 可读性：使用接口类可以使代码更易读和易理解。通过使用接口类中的函数名称，开发人员可以更直观地了解代码的意图和功能。







## 例子：与测试网WETH合约交互

![image-20230704224919523](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20230704224919523.png)



```js
// Interface 接口类
// 利用abi生成
// const interface = ethers.Interface(abi)
// 直接从contract中获取
// const interface2 = contract.interface
import { ethers } from "ethers";

//准备 alchemy API 可以参考https://github.com/AmazingAng/WTFSolidity/blob/main/Topics/Tools/TOOL04_Alchemy/readme.md 
const ALCHEMY_GOERLI_URL = 'https://eth-goerli.alchemyapi.io/v2/GlaeWuylnNM3uuOo-SAwJxuwTdqHaY5l';
const provider = new ethers.JsonRpcProvider(ALCHEMY_GOERLI_URL);

// 利用私钥和provider创建wallet对象
const privateKey = '0x227dbb8586117d55284e26620bc76534dfbd2394be34cf4a09cb775d593b'
const wallet = new ethers.Wallet(privateKey, provider)

// WETH的ABI
const abiWETH = [
    "function balanceOf(address) public view returns(uint)",
    "function deposit() public payable",
];
// WETH合约地址（Goerli测试网）
const addressWETH = '0xb4fbf271143f4fbf7b91a5ded31805e42b2208d6'

// 声明WETH合约
const contractWETH = new ethers.Contract(addressWETH, abiWETH, wallet)

const main = async () => {

    const address = await wallet.getAddress()
    // 1. 读取WETH合约的链上信息（WETH abi）
    console.log("\n1. 读取WETH余额")
    // 编码calldata
    const param1 = contractWETH.interface.encodeFunctionData(
        "balanceOf",
        [address]
      );
    console.log(`编码结果： ${param1}`)
    // 创建交易
    const tx1 = {
        to: addressWETH,
        data: param1
    }
    // 发起交易，可读操作（view/pure）可以用 provider.call(tx)
    const balanceWETH = await provider.call(tx1)
    console.log(`存款前WETH持仓: ${ethers.formatEther(balanceWETH)}\n`)

    //读取钱包内ETH余额
    const balanceETH = await provider.getBalance(wallet)
    // 如果钱包ETH足够
    if(ethers.formatEther(balanceETH) > 0.0015){

        // 2. 调用desposit()函数，将0.001 ETH转为WETH
        console.log("\n2. 调用desposit()函数，存入0.001 ETH")
        // 编码calldata
        const param2 = contractWETH.interface.encodeFunctionData(
            "deposit"          
            );
        console.log(`编码结果： ${param2}`)
        // 创建交易
        const tx2 = {
            to: addressWETH,
            data: param2,
            value: ethers.parseEther("0.001")}
        // 发起交易，写入操作需要 wallet.sendTransaction(tx)
        const receipt1 = await wallet.sendTransaction(tx2)
        // 等待交易上链
        await receipt1.wait()
        console.log(`交易详情：`)
        console.log(receipt1)
        const balanceWETH_deposit = await contractWETH.balanceOf(address)
        console.log(`存款后WETH持仓: ${ethers.formatEther(balanceWETH_deposit)}\n`)

    }else{
        // 如果ETH不足
        console.log("ETH不足，去水龙头领一些Goerli ETH")
        console.log("1. chainlink水龙头: https://faucets.chain.link/goerli")
        console.log("2. paradigm水龙头: https://faucet.paradigm.xyz/")
    }
}

main()
```



上面的代码使用了 ethers.js 中的接口类 `ethers.Interface`。接口类是一个非常有用的工具，它能够根据合约的 ABI（Application Binary Interface）定义生成函数的调用数据，简化了与合约的交互过程。

在上面的代码中，首先声明了一个 `abiWETH` 变量，它包含了 WETH 合约的 ABI，用于描述合约的函数和事件。接下来创建了一个合约对象 `contractWETH`，通过提供合约地址、ABI 和钱包对象，实例化了一个具体的合约实例。

在代码的执行过程中，需要调用合约的函数来与合约进行交互。这时，通过使用接口类的 `encodeFunctionData` 方法，可以根据函数名称和参数生成合约函数的调用数据。例如，在代码中的以下部分：

```js
const param1 = contractWETH.interface.encodeFunctionData("balanceOf", [address]);
```

使用接口类的 `encodeFunctionData` 方法，传入函数名 `"balanceOf"` 和对应的参数 `[address]`，生成了调用合约函数 `"balanceOf"` 的数据。这样就不需要手动编写调用数据，避免了繁琐的编码过程和容易出错的问题。

生成了函数的调用数据之后，可以将该数据作为交易的数据字段（`data`）发送给以太坊网络进行合约函数的调用。例如，在代码中的以下部分：

```js
const tx1 = {
    to: addressWETH,
    data: param1
}
const balanceWETH = await provider.call(tx1);
```

创建了一个交易对象 `tx1`，指定合约地址和生成的调用数据作为数据字段，然后通过 `provider.call()` 方法发起了一个只读的调用操作。这样就能够读取合约的状态或执行只读函数，而无需发送实际的交易。

总而言之，接口类在上面的代码中的作用是根据合约的 ABI 自动生成函数的调用数据，使得与合约的交互过程更加简单、高效和可靠。通过使用接口类，开发者可以避免手动编写调用数据带来的错误和繁琐，并提高代码的可读性和可维护性。





首先，在以下代码中，我们使用接口类的 `encodeFunctionData` 方法生成了调用合约函数 `"deposit"` 的调用数据：

```js
const param2 = contractWETH.interface.encodeFunctionData("deposit");
```

这里没有传递任何参数，因为 `deposit` 函数不需要额外的输入。然后，我们将生成的调用数据作为交易的数据字段（`data`）一并与交易的接收地址（`to`）和发送的 ETH 数量（`value`）一起组成一个交易对象 `tx2`：

```js
const tx2 = {
    to: addressWETH,
    data: param2,
    value: ethers.parseEther("0.001")
};
```

我们将交易发送给钱包对象的 `sendTransaction` 方法，并等待交易被打包和上链：

```js
const receipt1 = await wallet.sendTransaction(tx2);
await receipt1.wait();
```

接收到的交易回执信息被存储在 `receipt1` 变量中，我们打印交易详情：

```js
console.log(`交易详情：`);
console.log(receipt1);
```

最后，我们通过调用 `balanceOf` 函数来获取存款后的 WETH 余额：

```js
const balanceWETH_deposit = await contractWETH.balanceOf(address);
```

这里的 `balanceOf` 函数是用于查询 WETH 合约中指定地址的余额。我们使用 `ethers.formatEther` 方法将余额从原始的 `BigNumber` 格式转换为以太单位的字符串，并打印出来：

```js
console.log(`存款后WETH持仓: ${ethers.formatEther(balanceWETH_deposit)}\n`);
```

总结起来，上述代码段使用接口类的 `encodeFunctionData` 方法生成合约函数 `"deposit"` 的调用数据，并将其与其他交易参数一起组成交易对象。然后，通过钱包对象的 `sendTransaction` 方法发送交易并等待交易被打包和上链。最后，我们查询存款后的 WETH 余额并将其打印出来。



## 总结

先搞清楚接口类`contractWETH.interface.encodeFunctionData`生成的合约调用数据是长什么样。

使用接口类的好处包括：

1. 类型安全：通过使用接口类，可以在编译时进行类型检查，确保函数调用的正确性。接口类提供了对合约函数的类型定义，可以避免在调用合约函数时传递错误的参数或返回值类型。
2. 代码提示：接口类可以为合约函数提供代码提示和自动补全功能。在开发环境中，通过接口类可以获得函数名称、参数和返回值的提示，提高开发效率。
3. ABI 解析：接口类可以解析合约的 ABI，根据函数名称和参数生成函数调用数据。这样，开发人员无需手动编写和处理函数调用数据，减少了出错的可能性。
4. 可读性：使用接口类可以使代码更易读和易理解。通过使用接口类中的函数名称，开发人员可以更直观地了解代码的意图和功能。





# 14 批量生成钱包

介绍HD钱包，并写一个批量生成钱包的脚本。



## HD钱包

HD钱包（Hierarchical Deterministic Wallet，多层确定性钱包）是一种数字钱包 ，通常用于存储比特币和以太坊等加密货币持有者的数字密钥。通过它，用户可以从一个随机种子创建一系列密钥对，更加便利、安全、隐私。要理解HD钱包，我们需要简单了解比特币的BIP32，BIP44，和BIP39。



BIP32、BIP44和BIP39是比特币中的三个重要的钱包标准，它们共同为比特币钱包的派生、备份和恢复提供了标准化的方法。下面是对它们的简单介绍：

1. BIP32（确定性钱包）：BIP32定义了一种确定性钱包的标准，它允许从单个种子派生出一系列的私钥和公钥，而无需为每个私钥生成新的备份。这种确定性钱包的好处在于可以使用一个简单的助记词（seed phrase）来备份和恢复整个钱包，而不必处理多个私钥。

2. BIP39（助记词）：BIP39定义了一种用于生成钱包助记词的标准。助记词是一组由单词组成的短语，可以用于生成加密钱包的种子。这种助记词具有易记和易于备份的特点，可以用作生成确定性钱包的种子。BIP39规定了助记词的长度以及如何计算助记词的校验和。

3. BIP44（多币种钱包）：BIP44定义了一种多币种钱包的标准，它可以从助记词和确定性钱包中派生出多个币种的密钥对。BIP44使用了一种层次化的钱包结构，其中币种、账户、外部/内部链以及地址索引都有固定的规则。这样可以方便地管理和备份多个加密货币的钱包，并保持钱包之间的层次结构一致性。

总结起来，BIP32提供了确定性钱包的标准，允许从单个种子派生多个密钥对；BIP39定义了生成助记词的规范，用于备份和恢复钱包；而BIP44建立在BIP32和BIP39的基础上，为多币种钱包提供了派生规则和层次结构，使得管理多个加密货币的钱包更加方便和一致。这些标准的使用使得比特币钱包的创建、备份和恢复变得更加标准化和可靠。



`BIP44`为`BIP32`的衍生路径提供了一套通用规范，适配比特币、以太坊等多链。这一套规范包含六级，每级之间用"/"分割：

```text
m / purpose' / coin_type' / account' / change / address_index
```

其中：

- m: 固定为"m"
- purpose：固定为"44"
- coin_type：代币类型，比特币主网为0，比特币测试网为1，以太坊主网为60
- account：账户索引，从0开始。
- change：是否为外部链，0为外部链，1为内部链，一般填0.
- address_index：地址索引，从0开始，想生成新地址就把这里改为1，2，3。

举个例子，以太坊的默认衍生路径为`"m/44'/60'/0'/0/0"`。



## 批量生成钱包

```js
import { ethers } from "ethers";

// 1. 创建HD钱包
console.log("\n1. 创建HD钱包")
// 生成随机助记词
const mnemonic = ethers.Mnemonic.entropyToPhrase(ethers.randomBytes(32))
// 创建HD钱包
const hdNode = ethers.HDNodeWallet.fromPhrase(mnemonic)
console.log(hdNode);

// 2. 通过HD钱包派生20个钱包
console.log("\n2. 通过HD钱包派生20个钱包")
const numWallet = 20
// 派生路径：m / purpose' / coin_type' / account' / change / address_index
// 我们只需要切换最后一位address_index，就可以从hdNode派生出新钱包
let basePath = "m/44'/60'/0'/0";
let wallets = [];
for (let i = 0; i < numWallet; i++) {
    let hdNodeNew = hdNode.derivePath(basePath + "/" + i);
    let walletNew = new ethers.Wallet(hdNodeNew.privateKey);
    console.log(`第${i+1}个钱包地址： ${walletNew.address}`)
    wallets.push(walletNew);
}

// 3. 保存钱包（加密json）
console.log("\n3. 保存钱包（加密json）")
const wallet = ethers.Wallet.fromPhrase(mnemonic)
console.log("通过助记词创建钱包：")
console.log(wallet)
// 加密json用的密码，可以更改成别的
const pwd = "password"
const json = await wallet.encrypt(pwd)
console.log("钱包的加密json：")
console.log(json)

// 4. 从加密json读取钱包
const wallet2 = await ethers.Wallet.fromEncryptedJson(json, pwd);
console.log("\n4. 从加密json读取钱包：")
console.log(wallet2)
```





## 总结

介绍了HD钱包（BIP32，BIP44，BIP39），并利用它在`ethers.js`批量生成了20个钱包。
