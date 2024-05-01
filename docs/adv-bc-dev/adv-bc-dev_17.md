# 构建联盟区块链

联盟（通常由多个参与者组成，如银行、电子商务网站、政府实体、医院等）可以利用区块链技术解决许多问题，使事情变得更快更便宜。尽管他们弄清楚了区块链如何帮助他们，以太坊的区块链实现并不特别适合所有情况。尽管还有其他区块链实现（例如 Hyperledger）专门为联盟进行构建，但在本书中我们将看到如何使用以太坊来构建联盟区块链。基本上，我们将使用 parity 来构建联盟区块链。虽然还有其他替代方案，如摩根大通的 quorum，我们将使用 parity，因为在撰写本书时，它已存在了一段时间，并且许多企业已经在使用它，而其他替代方案尚未被任何企业使用。但是对于您的需求，parity 可能并不是最好的解决方案；因此，在决定使用哪种解决方案之前，请调查所有其他解决方案。

在本章中，我们将涵盖以下主题：

+   为什么以太坊不适用于联盟区块链？

+   什么是 parity 节点，它有哪些特性？

+   什么是权威证明共识协议，并且 parity 支持哪些 PoA 类型？

+   Aura 共识协议是如何工作的？

+   下载和安装 parity

+   使用 parity 构建联盟区块链

# 联盟区块链是什么？

要了解什么是联盟区块链，或者换句话说，联盟需要什么样的区块链实现，我们来看一个例子。银行希望构建一个区块链，使资金转账变得更简单、更快捷、更便宜。在这种情况下，他们需要以下内容：

1.  **速度**：他们需要一个可以在近实时内确认交易的区块链网络。目前，以太坊区块链网络的区块时间为12秒，客户通常要等待几分钟才能确认一笔交易。

1.  **许可**：他们希望区块链是有许可的。许可本身意味着不同的事情。例如：许可可以包括获得加入网络的许可，可以包括获得创建区块的许可，也可以包括获得发送特定交易的许可等等。

1.  **安全**：对于私有网络来说，工作量证明（PoW）并不安全，因为参与者数量有限；因此，产生的哈希算力不足以使其安全。因此，需要一种可以保持区块链安全和不可变性的共识协议。

1.  **隐私**：尽管网络是私有的，在网络本身仍然需要隐私。有两种隐私：

1.  **身份隐私**：身份隐私是使身份无法追踪的行为。我们之前看到的获得身份隐私的解决方案是使用多个以太坊账户地址。但是，如果使用多个以太坊账户，则智能合约将无法通过所有权验证，因为无法知道这些账户是否实际上属于同一用户。

1.  **数据隐私**：有时，我们不希望数据对网络中的所有节点可见，而只对特定节点可见。

总的来说，在本章中，我们将学习如何解决以太坊中的这些问题。

# 什么是权威证明共识？

PoA是一种区块链共识机制，共识是通过参考验证器列表（当它们与物理实体相关联时称为权威）来实现的。验证器是一组被允许参与共识的账户/节点；它们验证交易和区块。

与PoW或PoS不同，没有涉及挖矿机制。有各种类型的PoA协议，它们的差异取决于它们的实际工作方式。Hyperledger和Ripple都是基于PoA的。Hyperledger基于PBFT，而Ripple使用迭代过程。

# Parity简介

Parity是从头开始为正确性/可验证性、模块化、低资源占用和高性能而编写的以太坊节点。它是用Rust编程语言编写的，这是一种具有效率重点的混合命令式/OO/函数式语言。它由Parity Technologies专业开发。在撰写本书时，parity的最新版本是1.7.0，我们将使用此版本。我们将学习构建联盟区块链所需的所有内容。要深入了解parity，您可以参考官方文档。

它比go-ethereum具有更多功能，如web3 dapp浏览器，更高级的账户管理等。但是使它特殊的是它支持**权威证明**（**PoA**）以及PoW。Parity目前支持Aura和Tendermint PoA协议。未来，它可能支持更多的PoA协议。目前，Parity建议使用Aura而不是Tendermint，因为Tendermint仍在开发中。

对于权限区块链来说，Aura比PoW是一个更好的解决方案，因为它拥有更好的区块时间，并且在私有网络中提供了更好的安全性。

# 理解Aura的工作原理

让我们从高层次了解Aura的工作原理。Aura要求在每个节点中都指定相同的验证器列表。这是一组参与共识的账户地址。一个节点可能是验证节点，也可能不是。即使是验证节点，也需要有这个列表，以便它自己能达成共识。

如果验证者列表将永远保持不变，那么可以将此列表作为静态列表包含在创世文件中，或者将其提供在智能合约中，以便可以动态更新并使每个节点都知道它。在智能合约中，您可以配置有关谁可以添加新验证者的各种策略。

区块时间可以在创世文件中进行配置。由你决定区块时间。在私有网络中，3秒的区块时间效果很好。在 Aura 中，每三秒钟选择一个验证者，这个验证者负责创建、验证、签名和广播该区块。我们不需要太了解实际的选择算法，因为它不会影响我们的 dapp 开发。但这是计算下一个验证者的公式，`(UNIX_TIMESTAMP / BLOCK_TIME % NUMBER_OF_TOTAL_VALIDATORS)`。选择算法足够智能，给每个人平等的机会。当其他节点接收到一个区块时，它们会检查它是否来自下一个有效的验证者；如果不是，则会拒绝它。与 PoW 不同，当验证者创建一个区块时，它不会受到以太币的奖励。在 Aura 中，决定在没有交易时是否生成空块是由我们决定的。

你一定会想知道，如果由于某些原因，下一个验证节点无法创建和广播下一个区块会发生什么。为了理解这一点，让我们举个例子：假设 A 是第五个区块的验证者，而 B 是第六个区块的验证者。假设区块时间为五秒。如果 A 未能广播一个区块，那么当 B 的轮到时，它将在五秒后广播一个区块。所以实际上没什么严重的事情发生。区块时间戳将揭示这些细节。

你可能也会想知道网络是否有可能出现多个不同的区块链，就像 PoW 中当两个矿工同时挖矿时会发生的情况一样。是的，这种情况可能会发生很多次。让我们举个例子，了解一种可能发生这种情况的方式以及网络如何自动解决这个问题。假设有五个验证者：A、B、C、D 和 E。区块时间为五秒。假设首先选择了 A，它广播了一个区块，但由于某种原因该区块未能到达 D 和 E；因此他们会认为 A 没有广播区块。现在假设选择算法选择了 B 来生成下一个区块；那么 B 将在 A 的区块之上生成下一个区块，并广播给所有节点。D 和 E 将拒绝它，因为前一个区块的哈希值将不匹配。由此，D 和 E 将形成一个不同的链，而 A、B 和 C 将形成另一个不同的链。A、B 和 C 将拒绝来自 D 和 E 的区块，而 D 和 E 将拒绝来自 A、B 和 C 的区块。节点之间将这个问题解决为 A、B 和 C 所持有的区块链比 D 和 E 所持有的区块链更准确；因此 D 和 E 将用 A、B 和 C 所持有的区块链替换他们的版本。这两个版本的区块链将有不同的准确性得分，而第一个区块链的得分将比第二个区块链的得分更高。当 B 广播它的区块时，它还将提供其区块链的得分，由于其得分更高，D 和 E 将用 B 的区块链替换了他们的区块链。这就是冲突如何解决的。区块链的链分数是使用 `(U128_max * BLOCK_NUMBER_OF_LATEST_BLOCK - (UNIX_TIMESTAMP_OF_LATEST_BLOCK / BLOCK_TIME))` 计算的。首先按长度对链进行评分（区块越多，得分越高）。对于长度相等的链，选择最后一个区块更旧的链。

你可以在 [https://github.com/paritytech/parity/wiki/Aura](https://github.com/paritytech/parity/wiki/Aura) 深入了解 Aura。

# 运行 Parity

Parity 需要 Rust 版本 1.16.0 来构建。建议通过 rustup 安装 Rust。

# 安装 rust

如果你还没有安装 rustup，你可以这样安装。

# Linux

在基于 Linux 的操作系统上，运行此命令：

```
curl https://sh.rustup.rs -sSf | sh
```

Parity 还需要安装 `gcc`、`g++`、`libssl-dev`/`openssl`、`libudev-dev` 和 `pkg-config` 软件包。

# OS X

在 OS X 上，运行此命令：

```
curl https://sh.rustup.rs -sSf | sh
```

Parity 还需要 clang。Clang 随 Xcode 命令行工具提供，也可以使用 Homebrew 安装。

# Windows

确保你已安装了带有 C++ 支持的 Visual Studio 2015。接下来，从 [https://static.rust-lang.org/rustup/dist/x86_64-pc-windows-msvc/rustup-init.exe](https://static.rust-lang.org/rustup/dist/x86_64-pc-windows-msvc/rustup-init.exe) 下载并运行 rustup 安装程序，启动 "VS2015 x64 Native Tools Command Prompt"，并使用以下命令安装和设置 `msvc` 工具链：

```
rustup default stable-x86_64-pc-windows-msvc
```

# 下载、安装和运行 Parity

现在，一旦您在操作系统上安装了rust，您就可以运行这个简单的一行命令来安装parity：

```
cargo install --git https://github.com/paritytech/parity.git parity
```

要检查Parity是否安装，请运行此命令：

```
parity --help
```

如果Parity安装成功，那么您将看到子命令和选项列表。

# 创建私有网络

现在是时候建立我们的财团区块链了。我们将创建两个用Aura连接的验证节点。我们将把它们都设置在同一台计算机上。

# 创建帐户

首先，打开两个shell窗口。第一个是给第一个验证者，第二个是给第二个验证者。第一个节点将包含两个帐户，第二个节点将包含一个帐户。第一个节点的第二个帐户将被分配一些初始以太，以便网络将拥有一些以太。

在第一个shell中，运行此命令两次：

```
parity account new -d ./validator0 
```

两次都会要求您输入密码。现在只需为两个帐户输入相同的密码。

在第二个shell中，只运行此命令一次：

```
parity account new  -d ./validator1 
```

就像以前一样，输入密码。

# 创建规范文件

每个网络节点共享一个通用规范文件。该文件告诉节点有关创世块，验证者是谁，等等的信息。我们将创建一个智能合约，其中包含验证者列表。有两种类型的验证者合同：非报告合同和报告合同。我们只需要提供一个。

区别在于，非报告合同仅返回验证者列表，而报告合同可以对良性（良性不端行为可能只是从指定验证者那里收不到一个块）和恶意不端行为（恶意不端行为将释放两个不同的块以同一步骤为基础）采取行动。

非报告合同应至少具有此接口：

```
{"constant":true,"inputs":[],"name":"getValidators","outputs":[{"name":"","type":"address[]"}],"payable":false,"type":"function"}
```

`getValidators`函数将在每个块中调用，以确定当前列表。然后，切换规则由实现该方法的合同确定。

报告合同应至少具有此接口：

```
[ 
    {"constant":true,"inputs":[],"name":"getValidators","outputs":[{"name":"","type":"address[]"}],"payable":false,"type":"function"}, 
    {"constant":false,"inputs":[{"name":"validator","type":"address"}],"name":"reportMalicious","outputs":[],"payable":false,"type":"function"}, 
    {"constant":false,"inputs":[{"name":"validator","type":"address"}],"name":"reportBenign","outputs":[],"payable":false,"type":"function"} 
]
```

当有良性或恶意行为时，共识引擎分别调用`reportBenign`和`reportMalicious`函数。

让我们创建一个报告合同。以下是一个基本示例：

```
contract ReportingContract { 
   address[] public validators = [0x831647ec69be4ca44ea4bd1b9909debfbaaef55c, 0x12a6bda0d5f58538167b2efce5519e316863f9fd]; 
   mapping(address => uint) indices; 
   address public disliked; 

   function ReportingContract() { 
       for (uint i = 0; i < validators.length; i++) { 
           indices[validators[i]] = i; 
       } 
   } 

   // Called on every block to update node validator list. 
    function getValidators() constant returns (address[]) { 
      return validators; 
   } 

   // Expand the list of validators. 
   function addValidator(address validator) { 
      validators.push(validator); 
   } 

   // Remove a validator from the list. 
   function reportMalicious(address validator) { 
      validators[indices[validator]] = validators[validators.length-1]; 
      delete indices[validator]; 
      delete validators[validators.length-1]; 
      validators.length--; 
   } 

   function reportBenign(address validator) { 
       disliked = validator; 
   } 
}
```

这段代码是不言而喻的。确保在验证人员数组中用第一个验证节点的地址替换地址，因为我们将使用这些地址进行验证。现在使用您感觉舒服的方式编译上述合同。

现在让我们创建规范文件。创建一个名为`spec.json`的文件，并将以下代码放入其中：

```
{ 
    "name": "ethereum", 
    "engine": { 
        "authorityRound": { 
            "params": { 
               "gasLimitBoundDivisor": "0x400", 
               "stepDuration": "5", 
               "validators" : { 
               "contract": "0x0000000000000000000000000000000000000005" 
                } 
            } 
        } 
    }, 
    "params": { 
        "maximumExtraDataSize": "0x20", 
        "minGasLimit": "0x1388", 
        "networkID" : "0x2323" 
    }, 
    "genesis": { 
      "seal": { 
       "authorityRound": { 
         "step": "0x0", 
          "signature": "0x00000000000000000000000000000000000000000
             000000000000000000000000000000000000000000000000000000
                00000000000000000000000000000000000" 
            } 
        }, 
        "difficulty": "0x20000", 
        "gasLimit": "0x5B8D80" 
    }, 
    "accounts": { 
        "0x0000000000000000000000000000000000000001": { "balance": "1", "builtin": { "name": "ecrecover", "pricing": { "linear": { "base": 3000, "word": 0 } } } }, 
        "0x0000000000000000000000000000000000000002": { "balance": "1", "builtin": { "name": "sha256", "pricing": { "linear": { "base": 60, "word": 12 } } } }, 
        "0x0000000000000000000000000000000000000003": { "balance": "1", "builtin": { "name": "ripemd160", "pricing": { "linear": { "base": 600, "word": 120 } } } }, 
        "0x0000000000000000000000000000000000000004": { "balance": "1", "builtin": { "name": "identity", "pricing": { "linear": { "base": 15, "word": 3 } } } }, 
        "0x0000000000000000000000000000000000000005": { "balance": "1", "constructor" : "0x606060405260406040519081016040528073831647" }, 
        "0x004ec07d2329997267Ec62b4166639513386F32E": { "balance": "10000000000000000000000" } 
    } 
}
```

以下是上述文件的工作方式：

+   `engine` 属性用于设置共识协议和协议特定的参数。在这里，引擎是 `authorityRound`，即 aura。`gasLimitBoundDivisor` 决定了燃料限制的调整，具有常见的 `ethereum` 值。在 `validators` 属性中，我们有一个 `contract` 属性，即报告合同的地址。`stepDuration` 是以秒为单位的区块时间。

+   在 `params` 属性中，只有网络 ID 有关紧要关头；其他参数对于所有链都是标准的。

+   `genesis` 具有一些标准值用于 `authorityRound` 共识。

+   `accounts` 用于列出网络中存在的初始帐户和合同。前四个是标准的以太坊内置合同；这些应包括以使用 Solidity 合同编写语言。第五个是报告合同。确保您在 `constructor` 参数中用您的字节码替换字节码。最后一个帐户是验证器 1 shell 中生成的第二个帐户。它用于向网络提供以太币。将此地址替换为您自己的地址。

在我们进一步进行之前，请创建另一个名为 `node.pwds` 的文件。在该文件中，放置您创建的帐户的密码。验证者将使用此文件解锁帐户以签署区块。

# 启动节点

现在我们已经准备好启动我们的验证节点了。在第一个 shell 中，运行以下命令来启动第一个验证节点：

```
parity  --chain spec.json -d ./validator0 --force-sealing --engine-signer "0x831647ec69be4ca44ea4bd1b9909debfbaaef55c" --port 30300 --jsonrpc-port 8540 --ui-port 8180 --dapps-port 8080 --ws-port 8546 --jsonrpc-apis web3,eth,net,personal,parity,parity_set,traces,rpc,parity_accounts --password "node.pwds"
```

以下是前述命令的工作方式：

+   `--chain` 用于指定规范文件的路径。

+   `-d` 用于指定数据目录。

+   `--force-sealing` 确保即使没有交易也会产生区块。

+   `--engine-signer` 用于指定节点将使用的地址来签署区块，即验证者的地址。如果可能存在恶意验证者，则建议使用 `--force-sealing`；这将确保正确的链是最长的。确保您将地址更改为您生成的地址，即此 shell 上生成的第一个地址。

+   `--password` 用于指定密码文件。

在第二个 shell 中，运行以下命令来启动第二个验证节点：

```
parity  --chain spec.json -d ./validator1 --force-sealing --engine-signer "0x12a6bda0d5f58538167b2efce5519e316863f9fd" --port 30301 --jsonrpc-port 8541 --ui-port 8181 --dapps-port 8081 --ws-port 8547 --jsonrpc-apis web3,eth,net,personal,parity,parity_set,traces,rpc,parity_accounts --password "/Users/narayanprusty/Desktop/node.pwds" 
```

在这里，请确保将地址更改为您生成的地址，即在此 shell 上生成的地址。

# 连接节点

最后，我们需要连接两个节点。打开一个新的 shell 窗口并运行以下命令以查找连接到第二个节点的 URL：

```
curl --data '{"jsonrpc":"2.0","method":"parity_enode","params":[],"id":0}' -H "Content-Type: application/json" -X POST localhost:8541
```

您将获得这种类型的输出：

```
{"jsonrpc":"2.0","result":"enode://7bac3c8cf914903904a408ecd71635966331990c5c9f7c7a291b531d5912ac3b52e8b174994b93cab1bf14118c2f24a16f75c49e83b93e0864eb099996ec1af9@[::0.0.1.0]:30301","id":0}
```

现在运行以下命令，将 `enode` URL 中的编码 URL 和 IP 地址替换为 127.0.0.1：

```
curl --data '{"jsonrpc":"2.0","method":"parity_addReservedPeer","params":["enode://7ba..."],"id":0}' -H "Content-Type: application/json" -X POST localhost:8540
```

您应该获得以下输出：

```
{"jsonrpc":"2.0","result":true,"id":0}
```

节点应在控制台中指示 0/1/25 个对等体，这意味着它们彼此连接。这是一个参考图像：

![](img/d9e5ad8a-6bc4-418b-95d3-990a6838cb89.png)

# 许可和隐私

我们看到 parity 如何解决速度和安全性问题。目前，parity 没有提供任何与权限和隐私相关的特定内容。让我们看看如何在 parity 中实现这一点：

1.  **权限控制**：Parity 网络可以通过配置每个节点服务器以仅允许来自特定 IP 地址的连接来实现权限控制。即使 IP 地址没有被阻止，要连接到网络中的一个节点，新节点也需要一个之前我们看到的 enode 地址，这是无法猜测的。因此，默认情况下，存在基本的保护。但是没有强制执行此措施。网络中的每个节点都必须在其端口注意此事。通过智能合约可以对谁可以创建区块和谁不能进行类似的权限控制。最后，节点可以发送什么样的交易目前无法配置。

1.  **身份隐私**：有一种技术可以通过仍然启用所有权检查来实现身份隐私。在设置所有权时，所有者需要指定一个非确定性的非对称加密的公钥。每当它想要通过所有权检查时，它将提供常规文本的加密形式，合约将解密该文本并查看帐户是否为所有者。合约应确保不会检查相同的加密数据两次。

1.  **数据隐私**：如果你只是使用区块链来存储数据，你可以使用对称加密来加密数据并与希望查看数据的人分享密钥。但是无法对加密数据进行操作。如果需要对输入数据进行操作并仍然保护隐私，那么各方必须完全设置一个不同的区块链网络。

# 摘要

在本章中，我们学习了如何使用奇偶校验和光环运行以及一些实现奇偶校验和隐私的技术。现在，你至少可以自信地为使用区块链构建联合体构建一个概念验证。现在，你可以继续探索其他解决方案，例如 Hyperledger 1.0 和 Quorum 用于构建联合体区块链。目前，以太坊正式致力于使其更适合联合体；因此，请密切关注各种区块链信息来源，了解市场上的任何新动态。
