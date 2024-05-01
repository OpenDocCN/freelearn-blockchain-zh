# 构建企业级智能合约

到目前为止，我们一直在使用浏览器 Solidity 来编写和编译 Solidity 代码。 我们使用 web3.js 来测试我们的合同。 我们也可以使用 Solidity 在线 IDE 进行测试。 因为我们只编译了一个小合同，而且导入的内容很少，所以这似乎没问题。 当您开始构建大型和复杂的智能合约时，您将开始遇到使用当前流程进行编译和测试的问题。 在本章中，我们将学习有关 truffle 的知识，它使得构建企业级 DApps 变得容易，通过构建一个另类币。 除了比特币之外的所有加密货币都被称为另类币。

在本章中，我们将涵盖以下主题：

+   什么是 `ethereumjs-testrpc` 节点，以及如何使用它？

+   事件主题是什么？

+   使用 `truffle-contract` 包处理合同。

+   安装 truffle 并探索 truffle 命令行工具和配置文件

+   使用 truffle 编译、部署和测试 Solidity 代码

+   通过 NPM 和 EthPM 进行包管理

+   使用 truffle 控制台和编写外部脚本

+   使用 truffle 构建 DApp 的客户端

# 探索 ethereumjs-testrpc

`ethereumjs-testrpc` 是一个基于 Node.js 的以太坊节点，用于测试和开发。 它模拟完整节点行为，使以太坊应用程序的开发速度加快。 它还包括所有流行的 RPC 函数和功能（例如事件），并且可以以确定性的方式运行，以使开发变得轻松愉快。

它是用 JavaScript 编写的，并作为一个 `npm` 包分发。 在撰写本文时，最新版本的 `ethereumjs-testrpc` 是 3.0.3，至少需要 Node.js 版本 6.9.1 才能正常运行。

它将所有内容保存在内存中； 因此，每次节点重新启动时，它都会丢失先前的状态。

# 安装和使用

有三种方法可以使用 `ethereumjs-testrpc` 模拟以太坊节点。 每种方法都有自己的用例。 让我们来探索一下。

# testrpc 命令行应用程序

testrpc 命令可用于模拟以太坊节点。 要安装此命令行应用程序，您需要全局安装 `ethereumjs-testrpc`：

```
npm install -g ethereumjs-testrpc

```

这里是可以提供的各种选项：

+   `-a` 或 `--accounts`：这指定了启动时要生成的帐户数。

+   `-b` 或 `--blocktime`：这指定了自动挖矿的秒数。 默认值为 0，没有自动挖矿。

+   `-d` 或 `--deterministic`：每次运行节点时，它将生成 10 个确定性地址； 也就是说，当您提供此标志时，每次都会生成相同的地址集。 这个选项还可以根据预定义的助记词生成确定性地址。

+   `-n` 或 `--secure`：默认情况下锁定可用帐户。 当此选项在不带 `--unlock` 选项的情况下使用时，将不会创建 HD 钱包。

+   `-m` 或 `--mnemonic`：使用特定的 HD 钱包助记词来生成初始地址。

+   `-p`或`--port`：要监听的端口号。默认为 8545。

+   `-h`或`--hostname`：要监听的主机名。默认为 Node 的`server.listen()`默认值。

+   `-s`或`--seed`：生成 HD 钱包助记词所需的任意数据。

+   `-g`或`--gasPrice`：使用自定义的 gas 价格（默认为 1）。如果在向节点发送交易时未提供 gas 价格，则将使用此 gas 价格。

+   `-l`或`--gasLimit`：使用自定义的 gas 限制（默认为 0x47E7C4）。如果在向节点发送交易时未提供 gas 限制，则将使用此 gas 限制。

+   `-f`或`--fork`：这是从另一个当前运行的以太坊节点在给定块中分叉。输入应该是其他客户端的 HTTP 位置和端口；例如，`http://localhost:8545`。可以选择指定要从中分叉的块，使用@标记：`http://localhost:8545@1599200`。

+   `--debug`：输出用于调试的虚拟机操作码。

+   `--account`：此选项用于导入账户。它可以任意多次指定`--account=...`，传递任意的私钥和它们关联的余额来生成初始地址。在使用`--account`时，不会为你创建一个 HD 钱包。

+   `-u`或`--unlock`：指定`--unlock ...`任意次数，传递要解锁的特定账户的地址或账户索引。当与`--secure`同时使用时，`--unlock`将覆盖指定帐户的锁定状态：`testrpc --secure --unlock "0x1234..." --unlock "0xabcd..."`。还可以通过编号指定解锁账户：`testrpc --secure -u 0 -u 1`。该功能也可以用于冒充帐户和解锁您原本无法访问的地址。在与`--fork`功能一起使用时，可以使用`testrpc`以任何区块链上的地址进行交易，这在测试和动态分析中非常有用。

+   `--networkId`：用于指定此节点所属的网络 ID。

请注意，私钥长度为 64 个字符，必须输入为带有 0x 前缀的十六进制字符串。余额可以输入为整数，也可以输入为以太币金额的 0x 前缀十六进制值。

# 将`ethereumjs-testrpc`用作 web3 提供者或作为 HTTP 服务器

你可以像这样将`ethereumjs-testrpc`用作`web3`提供者：

```
var TestRPC = require("ethereumjs-testrpc"); 
web3.setProvider(TestRPC.provider());

```

你可以像这样将`ethereumjs-testrpc`用作通用 HTTP 服务器：

```
var TestRPC = require("ethereumjs-testrpc"); 
var server = TestRPC.server(); 
server.listen(port, function(err, blockchain) {});

```

`provider()`和`server()`都接受一个允许您指定`ethereumjs-testrpc`行为的对象。此参数是可选的。可用选项如下：

+   `accounts`：值是对象的数组。每个对象都应该有一个带有十六进制值的余额键。也可以指定 secretKey 键，它表示帐户的私钥。如果没有 secretKey，地址将根据给定的余额自动生成。如果指定，密钥用于确定帐户的地址。

+   `debug`: 输出用于调试的 VM 操作码。

+   `logger`: 值是实现 `log()` 函数的对象。

+   `mnemonic`: 使用特定的 HD 钱包助记词生成初始地址。

+   `port`: 作为服务器运行时要监听的端口号。

+   `seed`: 生成 HD 钱包助记词所需的任意数据。

+   `total_accounts`: 启动时要生成的帐户数。

+   `fork`: 与前述 `--fork` 选项相同。

+   `network_id`: 与 `--networkId` 选项相同。用于指定此节点所属的网络 ID。

+   `time`: 第一个区块应该开始的日期。与 `evm_increaseTime` 方法一起使用此功能来测试“依赖于时间”的代码。

+   `locked`: 指定默认情况下是否锁定帐户。

+   `unlocked_accounts`: 一个指定应解锁哪些帐户的地址或地址索引的数组。

# 可用的 RPC 方法

这是使用 `ethereumjs-testrpc` 提供的 RPC 方法列表：

+   `eth_accounts`

+   `eth_blockNumber`

+   `eth_call`

+   `eth_coinbase`

+   `eth_compileSolidity`

+   `eth_estimateGas`

+   `eth_gasPrice`

+   `eth_getBalance`

+   `eth_getBlockByNumber`

+   `eth_getBlockByHash`

+   `eth_getBlockTransactionCountByHash`

+   `eth_getBlockTransactionCountByNumber`

+   `eth_getCode (仅支持区块号“latest”)`

+   `eth_getCompilers`

+   `eth_getFilterChanges`

+   `eth_getFilterLogs`

+   `eth_getLogs`

+   `eth_getStorageAt`

+   `eth_getTransactionByHash`

+   `eth_getTransactionByBlockHashAndIndex`

+   `eth_getTransactionByBlockNumberAndIndex`

+   `eth_getTransactionCount`

+   `eth_getTransactionReceipt`

+   `eth_hashrate`

+   `eth_mining`

+   `eth_newBlockFilter`

+   `eth_newFilter (包括日志/事件过滤器)`

+   `eth_sendTransaction`

+   `eth_sendRawTransaction`

+   `eth_sign`

+   `eth_syncing`

+   `eth_uninstallFilter`

+   `net_listening`

+   `net_peerCount`

+   `net_version`

+   `miner_start`

+   `miner_stop`

+   `rpc_modules`

+   `web3_clientVersion`

+   `web3_sha3`

还有一些特殊的非标准方法未包含在原始 RPC 规范中：

+   `evm_snapshot`: 在当前区块中快照区块链的状态。不带参数。返回创建的快照的整数 ID。

+   `evm_revert`: 将区块链状态恢复到先前的快照。接受一个参数，即要恢复到的快照 ID。如果未传递快照 ID，则将恢复到最新的快照。返回 true。

+   `evm_increaseTime`: 向前跳转时间。带一个参数，即要增加的秒数。返回秒数的总时间调整。

+   `evm_mine`: 强制挖掘一个区块。不带参数。独立于挖矿是否启动或停止而挖掘一个区块。

# 事件主题是什么？

主题是用于索引事件的值。您不能没有主题搜索事件。每当调用事件时，都会生成一个默认主题，该主题被视为事件的第一个主题。一个事件最多可以有四个主题。主题始终以相同的顺序生成。您可以使用一个或多个主题搜索事件。

第一个主题是事件的签名。剩下的三个主题是索引参数的值。如果索引参数是`string`、`bytes`或`array`，那么其 keccak-256 哈希就是主题。

让我们举一个示例来理解主题。假设有这样一个事件：

```
event ping(string indexed a, int indexed b, uint256 indexed c, string d, int e); 

//invocation of event 
ping("Random String", 12, 23, "Random String", 45);

```

这里生成了四个主题。它们分别是：

+   `0xb62a11697c0f56e93f3957c088d492b505b9edd7fb6e7872a93b41cdb2020644`：这是第一个主题。它是使用`web3.sha3("ping(string,int256,uint256,string,int256)")`生成的。在这里，您可以看到所有类型都是规范形式的。

+   `0x30ee7c926ebaf578d95b278d78bc0cde445887b0638870a26dcab901ba21d3f2`：这是第二个主题。它是使用`web3.sha3("随机字符串")`生成的。

+   第三和第四个主题分别为`0x000000000000000000000000000000000000000000000000000000000000000c`和`0x0000000000000000000000000000000000000000000000000000000000000017`，也就是值的十六进制表示。它们分别使用`EthJS.Util.bufferToHex(EthJS.Util.setLengthLeft(12, 32))`和`EthJS.Util.bufferToHex(EthJS.Util.setLengthLeft(23, 32))`计算得出。

在内部，您的以太坊节点将使用主题构建索引，以便您可以轻松找到基于签名和索引值的事件。

假设您想获取前述事件的事件调用，其中第一个参数是`随机字符串`，第三个参数是`23`或`78`；那么，您可以通过以下方式使用`web3.eth.getFilter`找到它们：

```
var filter = web3.eth.filter({ 
   fromBlock: 0, 
   toBlock: "latest", 
   address: "0x853cdcb4af7a6995808308b08bb78a74de1ef899", 
   topics: ["0xb62a11697c0f56e93f3957c088d492b505b9edd7fb6e7872a93b41cdb2020644", "0x30ee7c926ebaf578d95b278d78bc0cde445887b0638870a26dcab901ba21d3f2", null, [EthJS.Util.bufferToHex(EthJS.Util.setLengthLeft(23, 32)), EthJS.Util.bufferToHex(EthJS.Util.setLengthLeft(78, 32))]]            
}); 

filter.get(function(error, result){ 
 if (!error) 
   console.log(result); 
});

```

因此，我们在这里要求节点返回所有已被`0x853cdcb4af7a6995808308b08bb78a74de1ef899`合约地址触发的来自区块链的事件，其第一个主题为`0xb62a11697c0f56e93f3957c088d492b505b9edd7fb6e7872a93b41cdb2020644`，第二个主题为`0x30ee7c926ebaf578d95b278d78bc0cde445887b0638870a26dcab901ba21d3f2`，第三个主题为`0x0000000000000000000000000000000000000000000000000000000000000017`或`0x000000000000000000000000000000000000000000000000000000000000004e`。

在上述代码中，请注意`topics`数组值的顺序。顺序很重要。

# 开始使用 truffle-contract

在学习 truffle 之前，学习`truffle-contract`非常重要，因为`truffle-contract`与 truffle 紧密集成。 truffle 测试、与 truffle 中的合约交互的代码、部署代码等都是使用`truffle-contract`编写的。

`truffle-contract` API 是一个 JavaScript 和 Node.js 库，它使得与以太坊智能合约的交互变得容易。到目前为止，我们一直在使用 web3.js 来部署和调用智能合约函数，这很好，但`truffle-contract`旨在使与以太坊智能合约的交互变得更加容易。下面是一些`truffle-contract`的特点，这使得它在处理智能合约方面比 web3.js 更优秀：

+   同步事务以更好地控制流程（即，直到您保证它们已经被挖掘，事务才会完成）。

+   基于 Promise 的 API。不再有回调地狱。与 ES6 和 async/await 很好地配合使用。

+   交易的默认值，例如来自 `address` 或 `gas`。

+   返回每个同步交易的日志、交易收据和交易哈希。

在我们开始使用 `truffle-contract` 之前，你需要知道它不允许我们使用存储在以太坊节点之外的账户来签署交易；也就是说，它没有类似于 `sendRawTransaction` 的功能。`truffle-contract` API 假设你的 DApp 的每个用户都有自己的以太坊节点运行，并且他们的账户存储在该节点中。实际上，这就是 DApp 应该工作的方式，因为如果每个 DApp 的客户端都开始让用户创建和管理账户，那么用户将需要管理这么多账户，并且对于开发人员来说，每次构建客户端都要为每个客户端开发钱包管理器都是痛苦的。现在，问题是客户端如何知道用户在哪里存储账户以及以什么格式？因此，出于可移植性的考虑，建议你假设用户的账户存储在他们的个人节点中，并且为了管理账户，他们使用类似以太坊钱包应用的东西。由于以太坊节点中存储的账户由以太坊节点本身签名，因此不再需要 `sendRawTransaction`。每个用户都需要有自己的节点，并且不能共享节点，因为当一个账户被解锁时，任何人都可以使用它，这将使用户能够窃取他人的以太币并从他人的账户中进行交易。

如果你使用的应用需要你托管自己的节点并在其中管理账户，请确保不允许所有人对该节点进行 JSON-RPC 调用；相反，只有本地应用程序才能进行调用。另外，请确保不要将账户长时间保持解锁状态，并在不再需要账户时立即将其锁定。

如果你的应用程序需要创建和签署原始交易的功能，那么你可以使用 `truffle-contract` 来开发和测试智能合约，而在你的应用程序中，你可以像之前一样与合约交互。

# 安装和引入 truffle-contract

在撰写本文时，最新版本的 `truffle-contract` API 是 1.1.10。在引入 `truffle-contract` 之前，你需要先引入 web3.js，因为你需要创建一个提供程序来与 `truffle-contract` API 一起工作，这样 `truffle-contract` 将在内部使用提供程序来进行 JSON-RPC 调用。

要在 Node.js 应用中安装 `truffle-contract`，只需在应用目录中运行以下命令：

```
npm install truffle-contract

```

然后使用以下代码进行引入：

```
var TruffleContract = require("truffle-contract");

```

若要在浏览器中使用 `truffle-contract`，你可以在 [`github.com/trufflesuite/truffle-contract`](https://github.com/trufflesuite/truffle-contract) 仓库的 dist 目录中找到浏览器分发版。

在 HTML 中，你可以这样排队：

```
<script type="text/javascript" src="img/truffle-contract.min.js"></script>

```

现在你将拥有一个全局变量 `TruffleContract`。

# 建立一个测试环境

在开始学习关于`truffle-contract` API 之前，我们需要建立一个测试环境，这将帮助我们在学习过程中测试我们的代码。

首先，通过仅运行`testrpc --networkId 10`命令来运行代表网络 ID 10 的`ethereumjs-testrpc`节点。我们随机选择了网络 ID 10 用于开发目的，但您可以自由选择其他网络 ID。只需确保它不是 1，因为主网始终用于实时应用程序，而不用于开发和测试目的。

然后，创建一个 HTML 文件并将此代码放入其中：

```
<!doctype html> 
<html> 
   <body> 
         <script type="text/javascript" src="img/web3.min.js"></script> 
         <script type="text/javascript" src="img/>            contract.min.js"></script> 
         <script type="text/javascript"> 
               //place your code here 
         </script> 
   </body> 
</html>

```

下载`web3.min.js` 和 `truffle-contract.min.js`。您可以在[`github.com/trufflesuite/truffle-contract/tree/master/dist`](https://github.com/trufflesuite/truffle-contract/tree/master/dist)找到`truffle-contract`的浏览器构建。

# truffle-contract API

现在让我们来探索`truffle-contract`的 API。基本上，`truffle-contract`有两个 API，即合同抽象 API 和合同实例 API。合同抽象 API 表示关于合同（或库）的各种信息，例如其 ABI；未链接的字节码；如果合同已部署，则其在各个以太坊网络中的地址；如果已部署，则其依赖的库在各个以太坊网络中的地址；以及合同的事件。抽象 API 是为所有合同抽象存在的一组函数。合同实例表示特定网络中部署的合同。实例 API 是向合同实例提供的 API。它是根据您的`Solidity`源文件中可用的函数动态创建的。针对特定合同的合同实例是通过表示相同合同的合同抽象创建的。

# 合同抽象 API

合同抽象 API 是使`truffle-contract`与 web3.js 相比非常特殊的东西。这里是它的特殊之处：

+   它将根据连接的网络自动获取默认值，如库地址、合同地址等，因此每次更改网络时都不必编辑源代码。

+   您可以选择只在某些网络中监听某些事件。

+   它使在运行时轻松将库链接到合同的字节码。一旦您探索了如何使用 API，您将发现还有其他几个好处。

在讨论如何创建合同抽象及其方法之前，让我们编写一个样本合同，该合同抽象将代表。这是样本合同：

```
pragma Solidity ⁰.4.0; 

import "github.com/pipermerriam/ethereum-string-utils/contracts/StringLib.sol"; 

contract Sample 
{ 
    using StringLib for *; 

    event ping(string status); 

    function Sample() 
    { 
        uint a = 23; 
        bytes32 b = a.uintToBytes(); 

        bytes32 c = "12"; 
        uint d = c.bytesToUInt(); 

        ping("Conversion Done"); 
    } 
}

```

该合同将`uint`转换为`bytes32`，并使用`StringLib`库将`bytes32`转换为`uint`。`StringLib`在主网络上的地址为`0xcca8353a18e7ab7b3d094ee1f9ddc91bdf2ca6a4`，但在其他网络上，我们需要部署以测试合同。在继续之前，请使用浏览器 Solidity 编译它，因为您将需要 ABI 和字节码。

现在让我们创建一个代表`Sample`合同和`StringLib`库的合同抽象。这是代码，请将其放入`HTML`文件中：

```
var provider = new Web3.providers.HttpProvider("http://localhost:8545"); 
var web3 = new Web3(provider); 

var SampleContract = TruffleContract({ 
   abi: [{"inputs":[],"payable":false,"type":"constructor"},{"anonymous":false,"inputs":[{"indexed":false,"name":"status","type":"string"}],"name":"ping","type":"event"}], 
   unlinked_binary: "6060604052341561000c57fe5b5b6000600060006000601793508373__StringLib__6394e8767d90916000604051602001526040518263ffffffff167c01000000000000000000000000000000000000000000000000000000000281526004018082815260200191505060206040518083038186803b151561008b57fe5b60325a03f4151561009857fe5b5050506040518051905092507f31320000000000000000000000000000000000000000000000000000000000009150816000191673__StringLib__6381a33a6f90916000604051602001526040518263ffffffff167c010000000000000000000000000000000000000000000000000000000002815260040180826000191660001916815260200191505060206040518083038186803b151561014557fe5b60325a03f4151561015257fe5b5050506040518051905090507f3adb191b3dee3c3ccbe8c657275f608902f13e3a020028b12c0d825510439e5660405180806020018281038252600f8152602001807f436f6e76657273696f6e20446f6e65000000000000000000000000000000000081525060200191505060405180910390a15b505050505b6033806101da6000396000f30060606040525bfe00a165627a7a7230582056ebda5c1e4ba935e5ad61a271ce8d59c95e0e4bca4ad20e7f07d804801e95c60029", 
   networks: { 
         1: { 
         links: { 
       "StringLib": "0xcca8353a18e7ab7b3d094ee1f9ddc91bdf2ca6a4" 
               }, 
        events: { 
                  "0x3adb191b3dee3c3ccbe8c657275f608902f13e3a020028b12c0d825510439e56": { 
                "anonymous": false, 
                "inputs": [ 
                         { 
                          "indexed": false, 
                          "name": "status", 
                          "type": "string" 
                         } 
                           ], 
                           "name": "ping", 
                           "type": "event" 
                     } 
               } 
         }, 
         10: { 
               events: { 
                  "0x3adb191b3dee3c3ccbe8c657275f608902f13e3a020028b12c0d825510439e56": { 
                           "anonymous": false, 
                           "inputs": [ 
                                 { 
                                       "indexed": false, 
                                       "name": "status", 
                                       "type": "string" 
                                 } 
                           ], 
                           "name": "ping", 
                           "type": "event" 
                     } 
               } 
         } 
   }, 
   contract_name: "SampleContract", 
}); 

SampleContract.setProvider(provider); 
SampleContract.detectNetwork(); 

SampleContract.defaults({ 
   from: web3.eth.accounts[0], 
   gas: "900000", 
   gasPrice: web3.eth.gasPrice, 
}) 

var StringLib = TruffleContract({ 
   abi: [{"constant":true,"inputs":[{"name":"v","type":"bytes32"}],"name":"bytesToUInt","outputs":[{"name":"ret","type":"uint256"}],"payable":false,"type":"function"},{"constant":true,"inputs":[{"name":"v","type":"uint256"}],"name":"uintToBytes","outputs":[{"name":"ret","type":"bytes32"}],"payable":false,"type":"function"}], 
   unlinked_binary: "6060604052341561000c57fe5b5b6102178061001c6000396000f30060606040526000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806381a33a6f1461004657806394e8767d14610076575bfe5b6100606004808035600019169060200190919050506100aa565b6040518082815260200191505060405180910390f35b61008c6004808035906020019091905050610140565b60405180826000191660001916815260200191505060405180910390f35b6000600060006000600102846000191614156100c557610000565b600090505b60208110156101355760ff81601f0360080260020a85600190048115156100ed57fe5b0416915060008214156100ff57610135565b603082108061010e5750603982115b1561011857610000565b5b600a8302925060308203830192505b80806001019150506100ca565b8292505b5050919050565b60006000821415610173577f300000000000000000000000000000000000000000000000000000000000000090506101e2565b5b60008211156101e157610100816001900481151561018e57fe5b0460010290507f01000000000000000000000000000000000000000000000000000000000000006030600a848115156101c357fe5b06010260010281179050600a828115156101d957fe5b049150610174565b5b8090505b9190505600a165627a7a72305820d2897c98df4e1a3a71aefc5c486aed29c47c80cfe77e38328ef5f4cb5efcf2f10029", 
   networks: { 
         1: { 
               address: "0xcca8353a18e7ab7b3d094ee1f9ddc91bdf2ca6a4" 
         } 
   }, 
   contract_name: "StringLib", 
}) 

StringLib.setProvider(provider); 
StringLib.detectNetwork(); 

StringLib.defaults({ 
   from: web3.eth.accounts[0], 
   gas: "900000", 
   gasPrice: web3.eth.gasPrice, 
})

```

以下是前述代码的工作原理：

1.  首先，我们创建一个提供者。使用此提供者，`truffle-contract`将与节点通信。

1.  然后，我们为`Sample`合同创建一个合同抽象。要创建合同抽象，我们使用`TruffleContract`函数。此函数接受一个包含有关合同的各种信息的对象。此对象可以称为工件对象。`abi`和`unlinked_binary`属性是强制的。对象的其他属性是可选的。`abi`属性指向合同的 ABI，而`unlinked_binary`属性指向合同的未链接二进制代码。

1.  然后，我们有一个网络属性，指示各种网络中有关合同的信息。在这里，我们说在网络 ID 1 中，`StringLib`依赖项部署在`0xcca8353a18e7ab7b3d094ee1f9ddc91bdf2ca6a4`地址，因此在网络 1 中部署`Sample`合同时，它会自动链接它。在网络对象下，我们还可以放置一个`address`属性，指示合同已部署到此网络并且这是合同地址。在`networks`对象中还有一个`events`对象，指定我们感兴趣捕获的合同事件。`events`对象的键是事件的主题，而`values`是事件的 ABI。

1.  然后，我们通过传递新的提供者实例调用`SampleContract`对象的`setProvider`方法。这是传递提供者的一种方式，以便`truffle-contract`可以与节点通信。`truffle-contract` API 不提供全局设置提供者的方法；相反，您需要为每个合同抽象设置提供者。这是一个功能，使我们可以轻松连接和处理多个网络。

1.  然后，我们调用`SampleContract`对象的`detectNetwork`方法。这是设置合同抽象当前表示的网络 ID 的方法；也就是说，在合同抽象的所有操作中，使用映射到此网络 ID 的值。此方法将自动检测我们的节点连接到哪个网络 ID，并将其自动设置。如果您想要手动设置网络 ID 或在运行时更改它，则可以使用`SampleContract.setNetwork(network_id)`。如果更改网络 ID，则确保提供者也指向相同网络的节点，因为否则`truffle-contract`将无法将网络 ID 与正确的链接、地址和事件进行映射。

1.  然后，我们为`SampleContract`进行了交易的默认值设置。此方法获取并可选地设置交易默认值。如果没有传递任何参数调用，它将简单地返回表示当前默认值的对象。如果传递了对象，则将设置新的默认值。

1.  我们对`StringLib`库执行了相同的操作，以便为其创建一个合约抽象。

# 创建合约实例

合约实例表示特定网络中部署的合约。使用合约抽象实例，我们需要创建合约实例。创建合约实例有三种方法：

+   `SampleContract.new([arg1, arg2, ...], [tx params])`: 此函数接受您的合约所需的任何构造函数参数，并将合约的新实例部署到合约抽象设置要使用的网络中。最后一个参数是可选的，您可以使用它来传递事务参数，包括事务来自地址、燃气限制和燃气价格。此函数返回一个承诺，当交易被挖掘时，它会解析为新部署地址处的合约抽象实例。此方法不会对合约抽象所代表的构件对象进行任何更改。在使用此方法之前，请确保它能找到字节码所依赖的库的地址，以供设置要使用的网络。

+   `SampleContract.at(address)`: 此函数创建一个新的合约抽象实例，表示传入地址处的合约。它返回一个“可被解析的”对象（出于向后兼容性，尚未实际承诺）。在确保代码存在于指定地址所在的网络之后，它会解析为一个合约抽象实例。

+   `SampleContract.deployed()`: 这就像`at()`一样，但地址是从构件对象中检索的。像`at()`一样，`deployed()`是可解析的，并且将解析为代表部署合约的合约实例，以确保代码存在于该位置，并且地址存在于设置为使用的网络上。

让我们部署并获取`Sample`合约的合约实例。在`网络 ID 10`中，我们需要使用`new()`首先部署`StringLib`库，然后将`StringLib`库的部署地址添加到`StringLib`抽象中，将`StringLib`抽象链接到`SampleContract`抽象，然后使用`new()`部署`Sample`合约以获取`Sample`合约的实例。但在`网络 ID 1`中，我们只需要部署`SampleContract`并获取其实例，因为我们已经在那里部署了`StringLib`。这是执行所有这些操作的代码：

```
web3.version.getNetwork(function(err, network_id) { 
   if(network_id == 1) 
   { 
         var SampleContract_Instance = null; 

         SampleContract.new().then(function(instance){ 
               SampleContract.networks[SampleContract.network_id]
                  ["address"] = instance.address; 
               SampleContract_Instance = instance; 
         }) 
   } 
   else if(network_id == 10) 
   { 
         var StringLib_Instance = null; 
         var SampleContract_Instance = null; 

         StringLib.new().then(function(instance){ 
               StringLib_Instance = instance; 
         }).then(function(){ 
               StringLib.networks[StringLib.network_id] = {}; 
               StringLib.networks[StringLib.network_id]["address"] = 
                 StringLib_Instance.address; 
               SampleContract.link(StringLib); 
         }).then(function(result){ 
               return SampleContract.new(); 
         }).then(function(instance){ 
               SampleContract.networks[SampleContract.network_id]
                 ["address"] = instance.address; 
               SampleContract_Instance = instance; 
         }) 
   } 
});

```

这是上述代码的工作原理：

1.  首先，我们检测网络 ID。如果网络 ID 是`10`，那么我们部署合约和库，如果网络 ID 是`10`，那么我们只部署合约。

1.  在 `网络 ID 10`，我们部署了 `StringLib` 合同并获得了它的合同实例。

1.  然后，我们更新 `StringLib` 抽象，以便它知道当前网络中合同的地址。更新抽象的界面类似于直接更新 artifacts 对象。如果你连接到网络 ID 1，那么它将覆盖已设置的 `StringLib` 地址。

1.  然后，我们将部署的 `StringLib` 链接到 `SampleContract` 抽象中。链接将更新链接并将库的事件复制到该抽象当前网络所代表的 `SampleContract`。库可以被多次链接，并且会覆盖其先前的链接。

1.  我们将 `SampleContract` 部署到当前网络。

1.  我们更新 `SampleContract` 抽象以存储在其代表的当前网络中合同的地址，以便以后可以使用 `deployed()` 获取实例。

1.  在网络 ID 1 的情况下，我们只部署 `SampleContract` 就可以了。

1.  现在你只需简单地更改连接的网络并重新启动你的应用程序，你的应用程序就会相应地运行。例如，在开发者的机器上，应用程序将连接到开发网络，而在生产服务器上，它将连接到主网络。显然，你可能不希望每次运行前述文件时都部署合同，因此在部署合同后，你可以实际上更新 artifacts 对象并在代码中检查合同是否已部署。如果尚未部署，则只需部署。而不是手动更新 artifacts 对象，你可以将 artifacts 存储在数据库中或文件中，并编写代码在合同部署完成后自动更新它们。

# 合同实例 API

每个合同实例都是基于 Solidity 合同源不同，并且 API 是动态创建的。以下是合同实例的各种 API：

+   `allEvents`：这是合同实例的一个函数，它接受一个回调，每当合同在当前网络 ID 下触发符合事件签名的事件时就会调用。你也可以使用`特定事件名`的函数来捕捉特定事件，而不是全部事件。在前述合同中，要捕捉 ping 事件，你可以使用 `SampleContract_Instance.ping(function(e, r){})`。

+   `send`：此函数用于向合同发送以太币。它接受两个参数；即第一个参数是要传输的 wei 数量，第二个参数是一个可选对象，可用于设置交易的 `from`，指示以太币是从哪个地址发送的。此调用返回一个 promise，并且当其被挖掘时，该 promise 解析为交易的详细信息。

+   我们可以使用 `SampleContract.functionName()` 或 `SampleContract.functionName.call()` 调用合约的任何方法。前者发送交易，而后者仅在 EVM 上调用方法，并且更改不是持久的。这两种方法都返回一个 promise。在第一种情况下，promise 解析为交易的结果，即一个包含交易哈希、日志和交易收据的对象。而在第二种情况下，它解析为方法 `call` 的返回值。这两种方法都接受函数参数和一个可选的最后一个参数，该参数是一个对象，用于设置交易的 `from`、`gas`、`value` 等。

# Truffle 简介

Truffle 是一个开发环境（提供命令行工具来编译、部署、测试和构建）、框架（提供各种包来编写测试、部署代码、构建客户端等）和资产管道（发布包并使用其他人发布的包）来构建基于以太坊的 DApps。

# 安装 Truffle

Truffle 可在 OS X、Linux 和 Windows 上运行。Truffle 要求你安装 Node.js 版本 5.0+。在撰写本文时，Truffle 的最新稳定版本是 3.1.2，我们将使用此版本。要安装 Truffle，只需运行以下命令：

```
npm install -g truffle

```

在继续之前，请确保你正在运行 network ID 为 10 的 testrpc。原因与前面讨论的相同。

# Truffle 初始化

首先，你需要为你的应用创建一个目录。将目录命名为`altcoin`。在`altcoin`目录内，运行以下命令来初始化你的项目：

```
truffle init

```

完成后，你将得到一个项目结构，其中包括以下项目：

+   contracts：Truffle 期望找到 `Solidity` 合约的目录。

+   migrations：包含合约部署代码的文件目录。

+   `test`：用于测试智能合约的测试文件位置。

+   `truffle.js`：主要的 Truffle 配置文件。

默认情况下，`truffle init` 会为你提供一组示例合约（`MetaCoin` 和 `ConvertLib`），它们就像是建立在以太坊之上的一个简单的代币。

这是 MetaCoin 智能合约的源代码，仅供参考：

```
pragma Solidity ⁰.4.4; 

import "./ConvertLib.sol"; 

contract MetaCoin { 
   mapping (address => uint) balances; 

   event Transfer(address indexed _from, address indexed _to, uint256 _value); 

   function MetaCoin() { 
         balances[tx.origin] = 10000; 
   } 

   function sendCoin(address receiver, uint amount) returns(bool sufficient) { 
         if (balances[msg.sender] < amount) return false; 
         balances[msg.sender] -= amount; 
         balances[receiver] += amount; 
         Transfer(msg.sender, receiver, amount); 
         return true; 
   } 

   function getBalanceInEth(address addr) returns(uint){ 
         return ConvertLib.convert(getBalance(addr),2); 
   } 

   function getBalance(address addr) returns(uint) { 
         return balances[addr]; 
   } 
}

```

`MetaCoin` 将 10 k metacoins 分配给部署合约的账户地址。10 k 是存在的比特币的总量。现在这个用户可以使用 `sendCoin()` 函数将这些 metacoins 发送给任何人。你可以随时使用 `getBalance()` 来查找你的账户余额。假设一个 metacoin 等于两个以太币，你可以使用 `getBalanceInEth()` 获取以太币的余额。

`ConvertLib` 库用于计算以太中 `metacoins` 的价值。为此，它提供了 `convert()` 方法。

# 合约编译

在 Truffle 中编译合约会生成带有 `abi` 和 `unlinked_binary` 设置的构建对象。要编译，请运行以下命令：

```
truffle compile 

```

Truffle 只会编译自上次编译以来发生变化的合约，以避免不必要的编译。如果你想要覆盖这种行为，可以在前面的命令中加上`--all`选项。

你可以在`build/contracts`目录中找到这些构建文件。你可以根据需要自由编辑这些文件。这些文件在运行`compile`和`migrate`命令时会被修改。

在编译之前，以下是需要注意的几件事：

+   Truffle 期望你的合约文件定义与其文件名完全匹配的合约。例如，如果你有一个名为`MyContract.sol`的文件，那么合约文件中应该存在其中一个：`contract MyContract{}`或`library myContract{}`。

+   文件名匹配区分大小写，这意味着如果你的文件名没有大写，那么你的合约名也不应该有大写。

+   你可以使用 Solidity 的`import`命令声明合约依赖关系。Truffle 将按照正确的顺序编译合约，并在必要时自动链接库。依赖关系必须相对于当前 Solidity 文件指定，并以`./`或`../`开头。

Truffle 版本 3.1.2 使用的编译器版本是 0.4.8。目前 Truffle 不支持更改编译器版本，因此是固定的。

# 配置文件

`truffle.js`文件是用于配置项目的 JavaScript 文件。该文件可以执行任何必要的代码来创建项目的配置。它必须导出一个代表你的项目配置的对象。以下是文件的默认内容：

```
module.exports = { 
  networks: { 
    development: { 
      host: "localhost", 
      port: 8545, 
      network_id: "*" // Match any network id 
    } 
  } 
};

```

此对象可以包含各种属性。但最基本的一个是`networks`。`networks`属性指定了哪些网络可用于部署，以及在与每个网络交互时的特定交易参数（例如`gasPrice`、`from`、`gas`等）。默认的`gasPrice`是 100,000,000,000，`gas`是 4712388，`from`是以太坊客户端中的第一个可用合约。

你可以指定任意数量的网络。请编辑配置文件为以下内容：

```
module.exports = { 
  networks: { 
    development: { 
      host: "localhost", 
      port: 8545, 
      network_id: "10" 
    }, 
    live: { 
         host: "localhost", 
      port: 8545, 
      network_id: "1" 
    } 
  } 
};

```

在上面的代码中，我们定义了两个名为`development`和`live`的网络。

当在 Windows 上使用命令提示符时，默认的配置文件名称可能会与`truffle`可执行文件发生冲突。如果是这种情况，我们建议您使用 Windows PowerShell 或 Git BASH，因为这些 shell 不会出现此冲突。或者，您可以将配置文件重命名为`truffle-config.js`以避免此冲突。

# 部署合约

即使是最小的项目也会与至少两个区块链进行交互：一个是开发者机器上的，比如 EthereumJS TestRPC，另一个代表着开发者最终将部署其应用程序的网络（例如主 Ethereum 网络或私有联盟网络）。

因为网络在运行时由合约抽象自动检测，这意味着您只需要部署一次您的应用或前端。当您的应用运行时，运行的以太坊客户端将确定使用哪些 artifacts，并且这将使您的应用非常灵活。

包含将合约部署到以太坊网络的代码的 JavaScript 文件称为迁移。这些文件负责分阶段执行部署任务，并且根据您的部署需求会随着时间而变化。随着项目的发展，您将创建新的迁移脚本以在区块链上进一步发展。先前运行的迁移历史记录在区块链上通过特殊的 `Migrations` 合约记录。如果您已经看过 `contracts` 和 `build/contracts` 目录的内容，那么您会注意到 `Migrations` 合约的存在。该合约应始终存在，并且除非您知道自己在做什么，否则不应触摸它。

# 迁移文件

在迁移目录中，您会注意到文件名以数字为前缀；也就是说，您会找到 `1_initial_migration.js` 和 `2_deploy_contracts.js` 文件。要记录迁移是否成功运行，需要带有编号前缀。

`Migrations` 合约（存储在 `last_completed_migration` 中）存储着对应于 `migrations` 文件夹中找到的最后一个应用的迁移脚本的编号。`Migrations` 合约总是首先部署。编号约定为 `x_script_name.js`，其中 x 从 1 开始。你的应用合约通常从编号为 2 的脚本开始。

因此，由于 `Migrations` 合约存储了应用的最后部署脚本的编号，truffle 将不会再次运行这些脚本。另一方面，未来，您的应用可能需要部署修改过的或新的合约。为了实现这一点，您会创建一个增加编号的新脚本，描述需要发生的步骤。然后，再次运行一次后，它们将不会再次运行。

# 编写迁移

在迁移文件的开头，我们通过 `artifacts.require()` 方法告诉 truffle 我们想要与之交互的合约。这个方法类似于 Node 的 `require`，但在我们的情况下，它具体返回一个合约抽象，我们可以在部署脚本的其余部分中使用它。

所有迁移必须通过 `module.exports` 语法导出一个函数。每个迁移导出的函数应将一个 `deployer` 对象作为其第一个参数。该对象通过提供清晰的 API 来部署智能合约，并执行一些部署的更普通的职责，例如保存部署后的 artifacts 文件以供以后使用，链接库等，来协助部署。`deployer` 对象是您进行部署任务分阶段的主要接口。

这里是 deployer 对象的方法。所有方法都是同步的：

+   `deployer.deploy(contractAbstraction, args..., options)`: 部署由合约抽象对象指定的特定合约，可选择传入构造函数参数。对于单例合约非常有用，这样您的 DApp 只存在一个该合约的实例。这将在部署后设置合约的地址（也就是 artifacts 文件中的`address`属性将等于新部署的地址），并覆盖存储的任何先前地址。您还可以选择传递一个合约数组或数组的数组，以加速多个合约的部署。此外，最后一个参数是一个可选对象，它可以包含一个单一键，即`overwrite`。如果`overwrite`设置为`false`，则如果已经部署了合约，则部署器将不会部署此合约。此方法返回一个 promise。

+   `deployer.link(library, destinations)`: 将已部署的库链接到一个或多个合约。`destinations`参数可以是单个合约抽象或多个合约抽象的数组。如果目标中的任何合约不依赖于被链接的库，则部署器将忽略该合约。此方法返回一个 promise。

+   `deployer.then(function(){})`: 用于运行任意的部署步骤。在迁移期间调用特定合约函数以添加、编辑和重新组织合约数据时使用它。在回调函数内部，您将使用合约抽象 API 部署和链接合约。

可以根据要部署到的网络条件性地运行部署步骤。为了有条件地分阶段部署步骤，编写迁移以便它们接受名为`network`的第二个参数。一个示例用例可以是，许多流行的库已经部署到了主网络；因此，当使用这些网络时，我们不会再次部署库，而只会链接它们。以下是一个代码示例：

```
module.exports = function(deployer, network) { 
  if (network != "live") { 
   // Perform a different step otherwise. 
  } else { 
    // Do something specific to the network named "live". 
  } 
}

```

在项目中，您会发现两个迁移文件，即`1_initial_migration.js`和`2_deploy_contracts.js`。第一个文件不应该被编辑，除非您知道自己在做什么。您可以自由处理另一个文件。以下是`2_deploy_contracts.js`文件的代码：

```
var ConvertLib = artifacts.require("./ConvertLib.sol"); 
var MetaCoin = artifacts.require("./MetaCoin.sol"); 

module.exports = function(deployer) { 
  deployer.deploy(ConvertLib); 
  deployer.link(ConvertLib, MetaCoin); 
  deployer.deploy(MetaCoin); 
};

```

这里，我们首先为`CovertLib`库和`MetaCoin`合约创建了抽象。无论使用哪个网络，我们都会部署`ConvertLib`库，然后将库链接到`MetaCoin`网络，最后部署`MetaCoin`网络。

要运行迁移，也就是部署合约，请运行以下命令：

```
truffle migrate --network development

```

在这里，我们告诉 truffle 在开发网络上运行迁移。如果我们不提供`--network`选项，则默认使用名称为`development`的网络。

在运行前述命令后，您会注意到 truffle 将自动更新 artifacts 文件中的`ConvertLib`库和`MetaCoin`合约地址，并更新链接。

以下是你可以提供给 migrate 子命令的一些其他重要选项：

+   `--reset`：从头开始运行所有迁移，而不是从上次完成的迁移开始运行。

+   -`f number`：从特定迁移运行合同。

你可以随时使用 `truffle networks` 命令在各种网络中找到项目的合同和库的地址。

# 单元测试合同

单元测试是测试应用程序的一种类型。这是一个过程，其中应用程序的最小可测试部分（称为单元）被单独和独立地检查是否正确运行。单元测试可以手动进行，但通常是自动化的。

Truffle 默认提供了一个单元测试框架来自动化测试你的合同。它在运行测试文件时提供了一个干净的环境；也就是说，truffle 会在每个测试文件开始时重新运行所有迁移，以确保你有一套新鲜的合同进行测试。

Truffle 让你以两种不同的方式编写简单易管理的测试：

+   在 JavaScript 中，从应用客户端执行你的合同

+   在 Solidity 中，从其他合同执行你的合同

两种测试方式都有各自的优点和缺点。我们将学习两种编写测试的方式。

所有测试文件都应位于 `./test` 目录中。Truffle 只会运行具有以下文件扩展名的测试文件：`.js`、`.es`、`.es6`、`.jsx` 和 `.sol`。所有其他文件都会被忽略。

在运行自动化测试时，`ethereumjs-testrpc` 比其他客户端快得多。此外，`testrpc` 包含 truffle 利用的特殊功能，可将测试运行时间加速近 90%。作为一般的工作流程，我们建议你在正常开发和测试期间使用 `testrpc`，然后在准备部署到实时或生产网络时，再对 go-ethereum 或其他官方以太坊客户端运行一次测试。

# 使用 JavaScript 编写测试

Truffle 的 JavaScript 测试框架是基于 mocha 构建的。Mocha 是一个用于编写测试的 JavaScript 框架，而 chai 是一个断言库。

测试框架用于组织和执行测试，而断言库提供了验证事物是否正确的工具。断言库使得测试代码变得更容易，这样你就不必执行成千上万的 if 语句。大多数测试框架不包含断言库，并允许用户插入他们想要使用的库。

在继续之前，你需要学习如何使用 mocha 和 chai 编写测试。要学习 mocha，请访问 [`mochajs.org/`](https://mochajs.org/)，要学习 chai，请访问 [`chaijs.com/`](http://chaijs.com/)。

你的测试应该存在于 `./test` 目录中，并且它们应该以 `.js` 扩展名结尾。

合约抽象是从 JavaScript 实现合约交互的基础。因为 truffle 无法检测到你在测试中需要与哪些合约交互，所以你需要显式地请求这些合约。你可以通过使用`artifacts.require()`方法来实现。因此，在测试文件中应该首先为你想要测试的合约创建抽象。

然后，应该编写实际的测试。结构上，你的测试应该与 mocha 的大部分测试保持基本一致。测试文件应该包含 mocha 可识别为自动化测试的代码。使 truffle 测试与 mocha 不同的是`contract()`函数：此函数的工作方式与`describe()`完全相同，只是它会告诉 truffle 运行所有迁移。`contract()`函数的工作方式如下：

+   在运行每个`contract()`函数之前，你的合约都会被重新部署到运行中的以太坊节点上，因此其中的测试会以清洁的合约状态运行。

+   `contract()`函数提供了由你的以太坊节点提供的一系列帐户，你可以用它们来编写测试。

由于 truffle 在内部使用 mocha，所以当 truffle 功能不必要时，仍然可以使用`describe()`来运行普通的 mocha 测试。

下面是 truffle 生成的用于测试`MetaCoin`合约的默认测试代码。你可以在`metacoin.js`文件中找到这段代码：

```
// Specifically request an abstraction for MetaCoin.sol 
var MetaCoin = artifacts.require("./MetaCoin.sol"); 

contract('MetaCoin', function(accounts) { 
  it("should put 10000 MetaCoin in the first account", function() { 
    return MetaCoin.deployed().then(function(instance) { 
      return instance.getBalance.call(accounts[0]); 
    }).then(function(balance) { 
      assert.equal(balance.valueOf(), 10000, "10000 wasn't in the first account"); 
    }); 
  }); 
  it("should send coin correctly", function() { 
    var meta; 

    // Get initial balances of first and second account. 
    var account_one = accounts[0]; 
    var account_two = accounts[1]; 

    var account_one_starting_balance; 
    var account_two_starting_balance; 
    var account_one_ending_balance; 
    var account_two_ending_balance; 

    var amount = 10; 

    return MetaCoin.deployed().then(function(instance) { 
      meta = instance; 
      return meta.getBalance.call(account_one); 
    }).then(function(balance) { 
      account_one_starting_balance = balance.toNumber(); 
      return meta.getBalance.call(account_two); 
    }).then(function(balance) { 
      account_two_starting_balance = balance.toNumber(); 
      return meta.sendCoin(account_two, amount, {from: account_one}); 
    }).then(function() { 
      return meta.getBalance.call(account_one); 
    }).then(function(balance) { 
      account_one_ending_balance = balance.toNumber(); 
      return meta.getBalance.call(account_two); 
    }).then(function(balance) { 
      account_two_ending_balance = balance.toNumber(); 

      assert.equal(account_one_ending_balance, account_one_starting_balance - amount, "Amount wasn't correctly taken from the sender"); 
      assert.equal(account_two_ending_balance, account_two_starting_balance + amount, "Amount wasn't correctly sent to the receiver"); 
    }); 
  }); 
});

```

在上述代码中，你可以看到所有合约的交互代码都是使用`truffle-contract`库编写的。这段代码很容易理解。

最后，truffle 为你提供了对 mocha 配置的访问，以便你可以更改 mocha 的行为。mocha 的配置放在`truffle.js`文件导出的对象的`mocha`属性下。例如，看一下这个：

```
mocha: { 
  useColors: true 
}

```

# 在 Solidity 中编写测试

Solidity 测试代码放在`.sol`文件中。在使用 Solidity 编写测试之前，请注意以下事项：

+   Solidity 测试不应该继承任何合约。这样可以使你的测试尽可能简洁，并完全控制你编写的合约。

+   Truffle 为你提供了默认的断言库，但你可以随时更改此库以满足你的需求。

+   你应该能够针对任何以太坊客户端运行你的 Solidity 测试。

要学习如何在 Solidity 中编写测试，让我们来探索 truffle 生成的默认 Solidity 测试代码。这是代码，可以在`TestMetacoin.sol`文件中找到：

```
pragma Solidity ⁰.4.2; 

import "truffle/Assert.sol"; 
import "truffle/DeployedAddresses.sol"; 
import "../contracts/MetaCoin.sol"; 

contract TestMetacoin { 

  function testInitialBalanceUsingDeployedContract() { 
    MetaCoin meta = MetaCoin(DeployedAddresses.MetaCoin()); 

    uint expected = 10000; 

    Assert.equal(meta.getBalance(tx.origin), expected, "Owner should have 10000 MetaCoin initially"); 
  } 

  function testInitialBalanceWithNewMetaCoin() { 
    MetaCoin meta = new MetaCoin(); 

    uint expected = 10000; 

    Assert.equal(meta.getBalance(tx.origin), expected, "Owner should have 10000 MetaCoin initially"); 
  } 

}

```

以下是上述代码的工作原理：

+   诸如`Assert.equal()`之类的断言函数由`truffle/Assert.sol`库提供。这是默认的断言库；但是，只要该库触发正确的断言事件，您就可以包含自己的断言库，以便与 truffle 的测试运行器松散集成。断言函数触发事件，这些事件被 truffle 捕获，并显示信息。这是 truffle 中 Solidity 断言库的架构。您可以在`Assert.sol`中找到所有可用的断言函数（[`github.com/ConsenSys/truffle/blob/beta/lib/testing/Assert.sol`](https://github.com/ConsenSys/truffle/blob/beta/lib/testing/Assert.sol)）。

+   在导入路径中，`truffle/Assert.sol`，`truffle`是包名。我们稍后将详细了解包。

+   您部署的合约地址（即作为迁移的一部分部署的合约）可通过`truffle/DeployedAddresses.sol`库获得。这由 truffle 提供，并在运行每个测试套件之前重新编译和重新链接。此库提供了所有已部署合约的函数，形式为`DeployedAddresses.<contract name>()`。然后，这将返回一个地址，您可以使用该地址访问该合约。

+   要使用部署的合约，您必须将合约代码导入测试套件。请注意，上述示例中的`import "../contracts/MetaCoin.sol";`。此导入是相对于测试合约的，测试合约存在于`./test`目录中，它会超出测试目录以找到`MetaCoin`合约。然后，它使用该合约将地址转换为`MetaCoin`类型。

+   所有测试合约都必须以大写字母`Test`开头。这将其与测试辅助工具和项目合约（即受测试约束的合约）区分开来，让测试运行器知道哪些合约代表测试套件。

+   像测试合约名称一样，所有测试函数都必须以小写字母`test`开头。每个测试函数都按照其在测试文件中的出现顺序作为单个事务执行（例如您的 JavaScript 测试）。由`truffle/Assert.sol`提供的断言函数触发测试运行器评估以确定测试结果的事件。断言函数返回一个表示断言结果的布尔值，您可以使用它来提前返回测试以防止执行错误（即`testrpc`将暴露的错误）。

+   你提供了许多测试钩子，如下例所示。这些钩子是`beforeAll`、`beforeEach`、`afterAll`和`afterEach`，与你的 JavaScript 测试中 mocha 提供的相同钩子。你可以使用这些钩子在每个测试之前和之后或在每个套件运行之前和之后执行设置和拆卸操作。像测试函数一样，每个钩子都作为单个事务执行。注意，一些复杂的测试需要执行大量设置可能会超出单个事务的燃气限制；你可以通过创建带有不同后缀的多个钩子来避免这种限制，如下例所示：

```
import "truffle/Assert.sol"; 

contract TestHooks { 
  uint someValue; 

  function beforeEach() { 
    someValue = 5; 
  } 

  function beforeEachAgain() { 
    someValue += 1; 
  } 

  function testSomeValueIsSix() { 
    uint expected = 6; 

    Assert.equal(someValue, expected, "someValue should have been 6"); 
  } 
}

```

+   这个测试合约还显示了你的`test`函数和`hook`函数都共享相同的合约状态。你可以在测试之前设置合约数据，在测试期间使用该数据，并在准备下一个测试之前重置它。注意，就像你的 JavaScript 测试一样，你的下一个测试函数将从上一个运行的测试函数的状态继续。

Truffle 没有直接的方法来测试你的合约是否应该抛出异常（也就是说，对于使用 throw 来表示预期错误的合约）。但是有一个笨拙的解决方案，你可以在这里找到：[`truffleframework.com/tutorials/testing-for-throws-in-Solidity-tests`](http://truffleframework.com/tutorials/testing-for-throws-in-solidity-tests)。

# 如何向测试合约发送以太币

要向你的 Solidity 测试合约发送以太币，它应该有一个名为`initialBalance`的返回`uint`的公共函数。这可以直接写成一个函数或一个公共变量。当你的测试合约部署到网络上时，truffle 会从你的测试账户向你的测试合约发送那个金额的以太币。然后，你的测试合约可以使用那个以太币在你的测试合约中脚本以太交互。注意，`initialBalance`是可选的，不是必需的。例如，看看下面的代码：

```
import "truffle/Assert.sol"; 
import "truffle/DeployedAddresses.sol"; 
import "../contracts/MyContract.sol"; 

contract TestContract { 
  // Truffle will send the TestContract one Ether after deploying the contract. 
  public uint initialBalance = 1 ether; 

  function testInitialBalanceUsingDeployedContract() { 
    MyContract myContract = MyContract(DeployedAddresses.MyContract()); 

    // perform an action which sends value to myContract, then assert. 
    myContract.send(...); 
  } 

  function () { 
    // This will NOT be executed when Ether is sent. o/ 
  } 
}

```

Truffle 以一种不执行回退函数的方式向你的测试合约发送以太币，所以你仍然可以在你的 Solidity 测试中使用回退函数进行高级测试用例。

# 运行测试

要运行你的测试脚本，只需运行这个命令：

```
truffle test

```

或者，你可以指定要运行的特定文件的路径。例如，看看这个：

```
truffle test ./path/to/test/file.js

```

# 包管理

一个 truffle 包是智能合约和它们的构件的集合。一个包可以依赖于零个或多个包，也就是说，你可以使用包的智能合约和构件。当在你自己的项目中使用包时，重要的是要注意有两个地方你将使用包的合约和构件：在你的项目的合约中和在你的项目的 JavaScript 代码中（迁移和测试）。

使用 truffle 创建的项目默认具有特定的布局，使它们可以被用作包。`truffle`包中最重要的目录如下：

+   `/contracts`

+   `/build/contracts`（由 truffle 创建）

第一个目录是你的合约目录，包括原始的 Solidity 合约。第二个目录是`/build/contracts`目录，其中保存着以`.json`文件形式的构建产物。

Truffle 支持两种包构建方式：`npm`和`ethpm`包。你必须知道什么是`npm`包，但让我们来看看什么是`ethpm`包。`Ethpm`是以太坊的包注册表。你可以在[`www.ethpm.com/`](https://www.ethpm.com/)找到所有的`ethpm`包。它遵循 ERC190 ([`github.com/ethereum/EIPs/issues/190`](https://github.com/ethereum/EIPs/issues/190)) 规范来发布和消费智能合约包。

# 通过 NPM 进行包管理

Truffle 默认带有 npm 集成，并且知道项目中的`node_modules`目录（如果存在）。这意味着你可以通过 npm 使用和分发合约或库，使你的代码对其他人可用，其他人的代码对你可用。你的项目中也可以有一个`package.json`文件。你可以在项目中简单地安装任何`npm`包，并在任何 JavaScript 文件中导入它，但只有包含前面提到的两个目录的情况下，它才会被称为 truffle 包。在 truffle 项目中安装`npm`包与在任何`Node.js`应用程序中安装`npm`包相同。

# 通过 EthPM 进行包管理

当安装 EthPM 包时，如果不存在，将创建一个`installed_contracts`目录。该目录可以类似于`node_modules`目录进行处理。

通过 EthPM 安装包几乎与通过 NPM 安装包一样简单。你只需运行以下命令：

```
truffle install <package name>

```

您还可以安装特定版本的包：

```
truffle install <package name>@<version>

```

像 NPM 一样，EthPM 版本遵循语义版本规范。你的项目也可以定义一个`ethpm.json`文件，它类似于 npm 包的`package.json`。要安装`ethpm.json`文件中列出的所有依赖项，请运行以下命令：

```
truffle install

```

一个示例的`ethpm.json`文件如下所示：

```
{ 
  "package_name": "adder", 
  "version": "0.0.3", 
  "description": "Simple contract to add two numbers", 
  "authors": [ 
    "Tim Coulter <tim.coulter@consensys.net>" 
  ], 
  "keywords": [ 
    "ethereum", 
    "addition" 
  ], 
  "dependencies": { 
    "owned": "⁰.0.1" 
  }, 
  "license": "MIT" 
}

```

创建和发布一个 truffle 的`npm`包与创建任何其他`npm`包的过程相同。要了解如何创建和发布一个`ethpm`包，请访问[`truffleframework.com/docs/getting_started/packages-ethpm#publishing-your-own-package`](http://truffleframework.com/docs/getting_started/packages-ethpm#publishing-your-own-package)。无论您将包发布为`npm`包还是`ethpm`包，都需要运行`truffle networks --clean`命令。运行此命令时，它会删除配置文件中仅匹配`*`通配符的所有网络 ID 的构件。这是因为这些地址对于消费此包的其他项目来说是无效的，因为这些网络很可能是私有的，因为它们仅用于开发目的。除非你知道你在做什么，否则不应省略此命令。它将无法删除作为常量列出的私有网络的任何构件，因此您需要手动删除它们。

# 在你的合同内使用包的合同

要在您的合同内使用包的合同，只需像 Solidity 的`import`语句一样简单。当您的`import`路径不是明确相对或绝对时，它表示您正在寻找特定命名包中的文件。考虑使用`example-truffle-library`（[`github.com/ConsenSys/example-truffle-library`](https://github.com/ConsenSys/example-truffle-library)）的示例：

```
import "example-truffle-library/contracts/SimpleNameRegistry.sol";

```

由于路径没有以`./`开头，truffle 知道要在您的项目的`node_modules`或`installed_contracts`目录中查找`example-truffle-library`文件夹。从那里，它解析路径以提供您请求的合同。

# 在 JavaScript 代码中使用包的构件

要在 JavaScript 代码中与包的构件进行交互，您只需要求该包的`.json`文件，然后使用`truffle-contract`将它们转换为可用的抽象：

```
var contract = require("truffle-contract"); 
var data = require("example-truffle-library/build/contracts/SimpleNameRegistry.json"); 
var SimpleNameRegistry = contract(data);

```

# 访问 Solidity 中包的合同部署地址

有时候，你可能希望你的合同与包先前部署的合同进行交互。由于部署的地址存在于包的`.json`文件中，Solidity 代码不能直接读取这些文件的内容。因此，使 Solidity 代码访问`.json`文件中的地址的流程是通过在 Solidity 代码中定义函数来设置依赖合同地址，并在合同部署后使用 JavaScript 调用这些函数来设置依赖合同地址。

所以你可以像这样定义你的合同代码：

```
import "example-truffle-library/contracts/SimpleNameRegistry.sol"; 

contract MyContract { 
  SimpleNameRegistry registry; 
  address public owner; 

  function MyContract { 
    owner = msg.sender; 
  } 

  // Simple example that uses the deployed registry from the package. 
  function getModule(bytes32 name) returns (address) { 
    return registry.names(name); 
  } 

  // Set the registry if you're the owner. 
  function setRegistry(address addr) { 
    if (msg.sender != owner) throw; 

    registry = SimpleNameRegistry(addr); 
  } 
}

```

这是您的迁移应该看起来像的：

```
var SimpleNameRegistry = artifacts.require("example-truffle-library/contracts/SimpleNameRegistry.sol"); 

module.exports = function(deployer) { 
  // Deploy our contract, then set the address of the registry. 
  deployer.deploy(MyContract).then(function() { 
    return MyContract.deployed(); 
  }).then(function(deployed) { 
    return deployed.setRegistry(SimpleNameRegistry.address); 
  }); 
};

```

# 使用 truffle 的控制台

有时，为了测试和调试目的或手动执行交易，与您的合同交互是很好的。Truffle 为您提供了通过交互式控制台轻松执行此操作的方式，您的合同可用并且可以立即使用。

要打开控制台，请运行此命令：

```
truffle console

```

控制台基于您的项目配置连接到以太坊节点。前述命令还接受`--network`选项，以指定要连接的特定节点。

以下是控制台的特点：

+   您可以在控制台中运行该命令。例如，您可以在控制台中键入`migrate --reset`，它将被解释为在控制台外部运行`truffle migrate --reset`的方式一样。

+   所有编译的合约都可供使用并准备就绪。

+   在每个命令（如`migrate --reset`）之后，您的合约都将重新提供，因此您可以立即开始使用新分配的地址和二进制文件。

+   `web3`对象已经可用，并设置为连接到您的以太坊节点。

+   所有返回 Promise 的命令都将自动解析并打印结果，无需对简单命令使用`.then()`。例如，您可以编写如下代码：`MyContract.at("0xabcd...").getValue.call();`

# 在 truffle 的上下文中运行外部脚本

通常，您可能希望运行与您的合约交互的外部脚本。Truffle 提供了一种简单的方法来实现这一点，根据您所需的网络引导您的合约，并根据项目配置自动连接到您的以太坊节点。

要运行外部脚本，请运行此命令：

```
truffle exec <path/to/file.js>

```

为了正确运行外部脚本，truffle 期望它们导出一个接受单个参数作为回调函数的函数。只要在此脚本中调用回调函数，您可以随意执行任何操作。回调函数接受一个错误作为其唯一参数。如果提供了错误，执行将停止，并且进程将返回一个非零退出代码。

外部脚本必须遵循以下结构：

```
module.exports = function(callback) { 
  // perform actions 
  callback(); 
}

```

# Truffle 的构建流水线

现在您已经知道如何使用 truffle 编译、部署和测试智能合约，是时候为我们的代币构建一个客户端了。在介绍如何使用 truffle 构建客户端之前，您需要知道它不允许我们使用存储在以太坊节点外部的账户进行交易签名；也就是说，它没有类似于`sendRawTransaction`的东西，原因与`truffle-contract`相同。

使用 truffle 构建客户端首先意味着在客户端源代码中集成 truffle 的构件，然后准备客户端源代码以进行部署。

要构建客户端，您需要运行此命令：

```
truffle build

```

当运行此命令时，truffle 将通过检查项目配置文件中的`build`属性来确定如何构建客户端。

# 运行外部命令

可以使用命令行工具构建客户端。当`build`属性为字符串时，truffle 会假定我们要运行一个命令来构建客户端，因此会将字符串作为命令运行。该命令会获得充足的环境变量以与 truffle 集成。

您可以通过类似的配置代码让 truffle 运行命令行工具来构建客户端：

```
module.exports = { 
  // This will run the &grave;webpack&grave; command on each build. 
  // 
  // The following environment variables will be set when running the command: 
  // WORKING_DIRECTORY: root location of the project 
  // BUILD_DESTINATION_DIRECTORY: expected destination of built assets 
  // BUILD_CONTRACTS_DIRECTORY: root location of your build contract files (.sol.js) 
  // 
  build: "webpack" 
}

```

# 运行自定义函数

JavaScript 函数可用于构建客户端。当 `build` 属性是一个函数时，松露将在我们想要构建客户端时运行该函数。该函数提供了有关项目如何与松露集成的大量信息。

你可以让松露运行一个函数来构建客户端，使用类似的配置代码：

```
module.exports = { 
  build: function(options, callback) { 
     // Do something when a build is required. &grave;options&grave; contains these values: 
     // 
     // working_directory: root location of the project 
     // contracts_directory: root directory of .sol files 
     // destination_directory: directory where truffle expects the built assets (important for &grave;truffle serve&grave;) 
  } 
}

```

你也可以创建一个对象，其中包含一个像这里的 `build` 方法。这对于想要发布一个构建客户端的包的人来说是很好的。

# 松露的默认构建器

松露提供了 `truffle-default-builder npm` 包，被称为松露的默认构建器。该构建器导出一个对象，其中有一个与之前提到的方法完全相同的构建方法。

默认构建器可用于为你的 DApp 构建 web 客户端，其服务器仅提供静态文件，并且所有功能都在前端。

在我们深入了解如何使用默认构建器之前，首先使用以下命令安装它：

```
npm install truffle-default-builder --save

```

现在将你的配置文件更改为这样：

```
var DefaultBuilder = require("truffle-default-builder"); 

module.exports = { 
  networks: { 
    development: { 
      host: "localhost", 
      port: 8545, 
      network_id: "10" 
    }, 
    live: { 
         host: "localhost", 
      port: 8545, 
      network_id: "1" 
    } 
  }, 
  build: new DefaultBuilder({ 
    "index.html": "index.html", 
    "app.js": [ 
      "javascripts/index.js" 
    ], 
    "bootstrap.min.css": "stylesheets/bootstrap.min.css" 
  }) 
};

```

默认构建器让你完全控制你想要组织客户端文件和文件夹的方式。

这个配置描述了 `targets`（左侧）的文件、文件夹和构成 `targets` 内容的文件数组（右侧）。每个目标将通过处理右侧的文件根据它们的文件扩展名进行生成，将结果连接在一起，然后将结果文件（目标）保存到构建目标中。在这里，右侧指定了一个字符串而不是一个数组，如果需要，该文件将被处理，然后直接复制过去。如果字符串以 `"/"` 结尾，它将被解释为一个目录，目录将被直接复制过去而不进行进一步处理。右侧指定的所有路径都是相对于 `app/` 目录的。

你可以随时更改此配置和目录结构。例如，你不需要 `javascripts` 和 `stylesheets` 目录，但请确保你相应地编辑你的配置。

如果你希望默认的构建器将松露集成到你的 web 应用程序的前端，请确保你有一个名为 `app.js` 的构建目标，该默认构建器可以将代码追加到其中。它不会将松露与任何其他文件名集成。

这里是默认构建器的特点：

+   自动导入你的编译合同构件、部署的合同信息和以太坊节点配置到客户端源代码中

+   包括建议的依赖项，包括 web3 和 `truffle-contract`

+   编译 `ES6` 和 `JSX` 文件

+   编译`SASS`文件

+   最小化 `asset` 文件

你可以使用 `truffle watch` 命令，它会监视 `contracts` 目录、`app` 目录和配置文件的更改。当有更改时，它会重新编译合约并生成新的构建文件，然后重新构建客户端。但它不会运行迁移和测试。

# 构建一个客户端

现在让我们为我们的 DApp 编写一个客户端，并使用 truffle 的默认构建器构建它。首先，根据我们设置的先前配置创建文件和目录：创建一个 `app` 目录，在其中创建一个 `index.html` 文件和两个名为 `javascripts` 和 `styelsheets` 的目录。在 `javascripts` 目录中，创建一个名为 `index.js` 的文件，在 `stylesheets` 目录中，下载并放置 Bootstrap 4 的 CSS 文件。你可以在 [`v4-alpha.getbootstrap.com/getting-started/download/#bootstrap-css-and-js`](https://v4-alpha.getbootstrap.com/getting-started/download/#bootstrap-css-and-js) 找到它。

在 `index.html` 文件中，放置以下代码：

```
<!doctype html> 
<html> 
   <head> 
         <link rel="stylesheet" type="text/css" href="bootstrap.min.css"> 
   </head> 
   <body> 
   <div class="container"> 
         <div class="row"> 
         <div class="col-md-6"> 
         <br> 
         <h2>Send Metacoins</h2> 
         <hr> 
         <form id="sendForm"> 
         <div class="form-group"> 
               <label for="fromAddress">Select Account Address</label> 
         <select class="form-control" id="fromAddress"> 
         </select> 
         </div> 
         <div class="form-group"> 
         <label for="amount">How much metacoin do you want to send?
         </label> 
         <input type="text" class="form-control" id="amount"> 
         </div> 
         <div class="form-group"> 
         <label for="toAddress">Enter the address to which you want to 
           send matacoins</label> 
         <input type="text" class="form-control" id="toAddress"  
             placeholder="Prefixed with 0x"> 
         </div> 
         <button type="submit" class="btn btn-primary">Submit</button> 
         </form> 
         </div> 
         <div class="col-md-6"> 
         <br> 
         <h2>Find Balance</h2> 
         <hr> 
         <form id="findBalanceForm"> 
         <div class="form-group"> 
               <label for="address">Select Account Address</label> 
               <select class="form-control" id="address"> 
               </select> 
         </div> 
         <button type="submit" class="btn btn-primary">Check 
            Balance</button> 
         </form> 
         </div> 
         </div> 
         </div> 
         <script type="text/javascript" src="img/app.js"></script> 
   </body> 
</html>

<!doctype html> 
 <html> 
     <head> 
         <link rel="stylesheet" type="text/css" href="bootstrap.min.css"> 
     </head> 
     <body> 
         <div class="container"> 
             <div class="row"> 
                 <div class="col-md-6"> 
                     <br> 
                     <h2>Send Metacoins</h2> 
                     <hr> 
                     <form id="sendForm"> 
                         <div class="form-group"> 
                             <label for="fromAddress">Select Account Address</label> 
                             <select class="form-control" id="fromAddress"> 
                             </select> 
                           </div> 
                           <div class="form-group"> 
                             <label for="amount">How much metacoin you want to send?</label> 
                             <input type="text" class="form-control" id="amount"> 
                           </div> 
                           <div class="form-group"> 
                             <label for="toAddress">Enter the address to which you want to send matacoins</label> 
                             <input type="text" class="form-control" id="toAddress" placeholder="Prefixed with 0x"> 
                           </div> 
                           <button type="submit" class="btn btn-primary">Submit</button> 
                     </form> 
                 </div> 
                 <div class="col-md-6"> 
                     <br> 
                     <h2>Find Balance</h2> 
                     <hr> 
                     <form id="findBalanceForm"> 
                         <div class="form-group"> 
                             <label for="address">Select Account Address</label> 
                             <select class="form-control" id="address"> 
                             </select> 
                           </div> 
                           <button type="submit" class="btn btn-primary">Check Balance</button> 
                     </form> 
                 </div> 
             </div> 
         </div> 
         <script type="text/javascript" src="img/app.js"></script> 
     </body> 
 </html>

```

在上述代码中，我们加载了 `bootstrap.min.css` 和 `app.js` 文件。我们有两个表单：一个用于向不同账户发送 metacoins，另一个用于检查账户的 metacoins 余额。在第一个表单中，用户必须选择一个账户，然后输入要发送的 metacoin 金额和要发送到的地址。而在第二个表单中，用户只需选择其想要检查 metacoin 余额的地址。

在 `index.js` 文件中，放置以下代码：

```
window.addEventListener("load", function(){ 
   var accounts = web3.eth.accounts; 

   var html = ""; 

   for(var count = 0; count < accounts.length; count++) 
   { 
         html = html + "<option>" + accounts[count] + "</option>"; 
   } 

   document.getElementById("fromAddress").innerHTML = html; 
   document.getElementById("address").innerHTML = html; 

   MetaCoin.detectNetwork(); 
}) 

document.getElementById("sendForm").addEventListener("submit", function(e){ 
   e.preventDefault(); 

   MetaCoin.deployed().then(function(instance){ 
         return instance.sendCoin(document.getElementById("toAddress").value, document.getElementById("amount").value, { 
               from: document.getElementById("fromAddress").options[document.getElementById("fromAddress").selectedIndex].value 
         }); 
   }).then(function(result){ 
         alert("Transaction mined successfully. Txn Hash: " + result.tx); 
   }).catch(function(e){ 
         alert("An error occured"); 
   }) 
}) 

document.getElementById("findBalanceForm").addEventListener("submit", function(e){ 
   e.preventDefault(); 

   MetaCoin.deployed().then(function(instance){ 
         return instance.getBalance.call(document.getElementById("address").value); 
   }).then(function(result){ 
         console.log(result); 
         alert("Balance is: " + result.toString() + " metacoins"); 
   }).catch(function(e){ 
         alert("An error occured"); 
   }) 
})

```

以下是代码的工作原理：

1.  `truffle-default-builder` 使构件对象可在 `__contracts__` 全局对象下使用。

1.  它还通过将变量名设置为合约名，为所有可用的合约提供了合约抽象作为全局变量。

1.  它还通过已设置提供程序为 web3 对象提供了支持。它还为合约抽象设置了提供程序。它使得 web3 对象连接到名为 `development` 的网络，如果不存在，则默认值为 `http://localhost:8545`。

1.  在上述代码中，首先，我们等待页面加载完成，一旦加载完成，我们就检索连接节点中的账户列表，并在两个表单中显示它们。我们还调用 `MetaCoin` 抽象的 `detectNetwork()` 方法。

1.  然后，我们为两个表单都设置了 `submit` 事件处理程序。它们都按预期执行其操作，并在弹出窗口中显示结果。

1.  当第一个表单提交时，我们获取 `MetaCoin` 合约的部署实例，并使用正确的参数调用 `sendCoin` 方法。

1.  当第二个表单提交时，我们通过调用 EVM 中的 `getBalance` 方法而不是广播交易来检索所选账户的余额。

现在继续运行 truffle build 命令，你会注意到 truffle 将在 `build` 目录中创建 `index.html`、`app.js` 和 `bootstrap.min.css` 文件，并在其中放置客户端的最终部署代码。

# Truffle 的服务器

Truffle 带有内置的 Web 服务器。这个 Web 服务器只是以适当的 MIME 类型设置提供`build`目录中的文件。除此之外，它没有配置为执行任何其他操作。

要运行 Web 服务器，请运行此命令：

```
truffle serve

```

默认情况下，服务器在端口号 8080 上运行。但你可以使用`-p`选项指定不同的端口号。

类似于 truffle watch，这个 Web 服务器还监视`contracts`目录、`app`目录和配置文件中的更改。当有更改时，它重新编译合约并生成新的工件文件，然后重建客户端。但它不会运行迁移和测试。

由于 truffle-default-builder 将最终可部署的代码放在构建目录中，你只需运行`truffle serve`即可通过 Web 提供文件。

让我们测试我们的 Web 客户端。访问`http://localhost:8080`，你会看到这个截图：

![](img/image_08_001.png)

所选框中的帐户地址对你而言会有所不同。现在在部署合约时，合约将所有的 metacoins 分配给部署合约的地址；所以在这里，第一个帐户将有 10,000 个 metacoins 的余额。现在从第一个帐户发送五个 metacoins 到第二个帐户，然后点击提交。你会看到类似以下截图的屏幕：

![](img/image_08_002.png)

现在通过在第二个表单的选择框中选择第二个帐户然后点击“检查余额”按钮来检查第二个帐户的余额。你会看到类似以下截图的屏幕：

![](img/image_08_003.png)

# 摘要

在本章中，我们深入了解了如何使用 truffle 构建 DApps 及其相应的客户端。我们看到 truffle 如何简化编写、编译、部署和测试 DApps 的过程。我们还看到了使用`truffle-contract`在客户端之间轻松切换网络而无需触及源代码的简便之处。现在，你已经准备好开始使用 truffle 构建企业级 DApps 了。

在下一章中，我们将使用 truffle 和以太坊闹钟 DApp 构建一个分散式闹钟应用程序，该应用程序在时间上为你支付报酬。

只需用冒号`":"`替换`i.e.`。
