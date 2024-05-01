# 深入区块链 - 拥有证明

在本章中，我们将通过创建一个拥有证明应用程序来介绍区块链的更广泛应用。在本章中，我们将讨论区块链内智能合约的概念，以便实施此应用程序。由于我们已经在本书的前几章介绍了区块链的概念，本章主要将关注智能合约的高级细节，即拥有证明和去中心化应用程序的创建。

在本章中，我们将重点关注以下主题：

+   创建拥有证明应用程序

+   区块链内智能合约的概念

+   如何选择智能合约平台

+   探索 NEO 区块链平台

+   在 NEO 区块链中创建去中心化应用程序（拥有证明）

+   探索以太坊区块链平台

+   在以太坊区块链中创建去中心化应用程序（拥有证明）

在资产的世界中，如果你想要声明和证明拥有它们，就必须跟踪每一个资产。但是资产是由世界各地的不同实体创建的，并且没有一个单一的协议来管理资产，因为每个实体都有自己的资产管理系统。例如，如果爱丽丝在一个城市拥有一所房子和一辆车，并且她想把房子和车都卖给鲍勃，因为她打算搬出这个城市，她必须通过不同的程序将所有权转让给鲍勃 - 她必须处理房地产登记处的房屋，交通部门的汽车。

此外，她还任命了一名律师，因为这个程序相当复杂。只有在处理注册办公室、律师和公证员之后，她才能最终将车和房屋的所有权转让给鲍勃。注册和管理不同资产的协议意味着爱丽丝必须与不同的实体打交道来执行一个简单的任务。

当前的资产管理系统需要来自某些受信任的权威的批准。受信任的权威参与的主要原因是资产存在于一个无信任的社会中。不同的实体创建自己的一套处理资产的程序。其中一些实体可能正在使用过时的技术，使得用户不得不处理一些传统的程序，这样很难使用。

本章提出的拥有证明解决方案将使用区块链来构建一个去中心化的应用程序，以缓解集中式资产管理系统所面临的所有问题。我们将利用数字身份、资产和智能合约来借助区块链技术创建一个完全去中心化的资产管理系统。

# 数字资产和身份

数字资产是以数字格式存在的可编程资产。这些资产可以拥有自己的价值（数字代币），或者可以虚拟代表现有的实物资产（车辆所有权）。数字资产自数字时代开始就已经被使用，但直到现在它们一直存在于管理集中的环境中。区块链的发明使数字资产得以存在于去中心化网络中，无需信任的中介来注册或交易资产。去除中介意味着用户在交易资产时无需支付任何额外费用。

数字身份对于处理资产所有权时至关重要。它代表了以数字格式存在的任何个人或组织的身份。数字身份基于**公钥基础设施**（**PKI**），为用户提供准确的身份管理。与容易伪造的传统身份文件不同，数字身份要求用户通过数字签名进行身份验证以证明其身份。这个系统通常使用安全的密钥基础设施，不容易被破坏。

在前几章我们讨论了索取数字资产的问题；你会记得我们讲解了为用户创建身份，用户将能够使用秘密密钥来索取资产。类似的方法将被用来在我们将用于构建本章应用程序的平台中创建和管理用户的数字身份。

# 所有权证明

世界上的每一项资产都归某个实体所有。由于部分所有权记录缺失或现有记录数据的模糊性，资产的部分所有权可能无法证明。尽管所有权是通过数字方式或其他方式由实体证明的，在大多数情况下，所有权信息在所有系统中并不一致。通过保持数字记录来证明所有权是最佳解决方案，数字资产和身份在其中扮演了重要角色。

数字资产与数字身份一起为主张对任何商品的所有权提供了一种方便的方式，因为资产与用户的身份一起在数字上注册。每当用户需要验证和证明资产的所有权时，他们可以提供他们的身份详细信息以及他们正在试图索赔的资产。用户经常需要通过提供一些秘密信息或使用秘密信息进行身份验证来验证他们的身份。身份验证过程取决于建立资产管理系统的第三方。在我们之前的示例中，Alice 想要将她的房子和车卖给 Bob，她需要向土地登记处和交通部门提供身份信息，然后可以通过与他们系统上的记录进行比较来进行验证。这种所有权证明系统的缺点是没有在不同组织之间保持适当的协议。这就是为什么身份管理在每个系统中都不安全的原因，以及为什么组织仍然使用传统系统，例如用户身份的硬拷贝，来验证身份而没有适当的身份验证机制，这很容易被不良行为者利用。

完全安全的所有权证明系统可以通过使用数字身份来创建，该身份使用强身份验证来证明用户的身份。大多数实现此所有权证明模型的现有系统都是集中式的，这需要对集中式机构的信任。尽管这种出处模型解决了问题，但它需要用户完全信任第三方组织来证明和验证所有权。使用区块链创建分散的所有权证明系统是唯一已知的解决方案，可以解决有关资产管理和所有权证明的所有问题。由于其不可变性和可追溯性，区块链是资产管理的最合适技术，因为一旦某个资产的某些信息被附加到区块链上，就无法撤销。可追溯性使得验证任何交易变得容易，并且如果隐私是一个问题，还允许使用专用区块链限制交易。

尽管在分散网络中可以使用区块链实现所有权证明，但在某些情况下，交易参与者之间可能存在一些复杂的协议。这些协议是通过创建合同而形成的。一个称为**智能合约**的概念用于在分散网络中执行此操作。我们将使用此功能来创建分散的所有权证明应用程序。

分散式所有权证明应用的最佳示例之一是**Everledger**，它为钻石市场的供应链构建了一个所有权证明模型（[`diamonds.everledger.io`](https://diamonds.everledger.io)）。Everledger 提供了一个全球数字区块链分类账，以跟踪资产的所有权历史。它试图防止钻石行业的欺诈行为，据估计，每年达数十亿美元。

# 智能合约

合同是在各方之间建立以强制执行协议并确保参与者不能以后否认协议的协议。智能合约是一种协议，允许合同以自动执行的方式进行验证和执行。简单来说，它在合同条件满足时执行由各方达成的合同，而无需任何人的干预。智能合约这个术语是由密码学家尼克·萨博（Nick Szabo）在 1994 年提出的。尽管智能合约在 20 世纪 90 年代初概念化，用于自动执行传统合同，但直到比特币底层区块链技术的采用之后才在公共网络中得以实现。

正是拜占庭容错共识算法使得在分散式公共网络中执行智能合约成为可能。许多现有的区块链平台支持**图灵完备**编程语言，这使得创建构建智能合约所需的逻辑变得更容易。

如果一个编程语言可以用来模拟图灵机，那么它被认为是图灵完备的。比特币的脚本语言被有意地设计成图灵不完备，以尽可能简化比特币交易。

由于智能合约是在区块链中创建和部署的，它们将受益于区块链提供的所有功能。一旦合同被各方接受和部署，由于其不可变性，区块链中存储的任何合同都无法被任何人篡改。除此之外，在区块链中部署智能合约会提供完全的透明度，因为任何人都可以随时验证合同的存在。

智能合约还具有其他几个额外的好处：

+   **更快的部署和执行**：以传统方式准备合同将需要用户花费数小时的时间准备文件并加工处理。智能合约只是一组自动化这些任务的指令，消除了许多不必要的步骤。

+   **成本效率的部署和执行**：在区块链上创建和执行智能合约比使用传统合同更便宜，传统合同需要中介参与才能处理。

+   **安全管理**：区块链中创建的所有合同都得到了安全管理。这是区块链的固有特性。

+   **复制证明**：由于网络中的去中心化公共账本，每个合约都驻留在网络的每个节点上，提供多重备份。在区块链网络上不可能丢失合约。

+   **准确执行**：智能合约以一组指令创建，在区块链网络中的每个节点上一致执行。这确保智能合约始终准确运行。由于区块链的拜占庭容错特性，网络将忽略合约的任何错误执行。

# 选择智能合约平台

智能合约是自动执行的合约，可以通过支持在其交易中执行基本脚本的任何区块链应用部署。大多数区块链平台支持领域特定语言。我们已经了解了比特币交易中使用的语言，称为**Script**，这是一种功能有限的基于堆栈的语言。尽管 Script 是一种图灵不完全的语言，但只有少数选项可以用于创建复杂交易。它可以创建多重签名交易，付款通道和原子跨链交易。此外，比特币可以创建一个带有锁定时间的交易。可以创建交易，但在某段时间内锁定，以防创建者希望在锁定时间到期之前使交易无效。尽管比特币的 Script 语言提供了足够的灵活性来创建复杂交易，但不适合创建复杂合约。

自那时以来，已经创建了许多区块链平台，这些平台提供使用自己的领域特定语言的高级脚本功能，例如以太坊的**Solidity**和卡尔达诺的**Plutus**。此外，大多数平台都有自己的运行时环境，编译后的智能合约在其中执行。运行时环境类似于通用编程语言（如 Java）中使用的环境。智能合约将在虚拟机上运行，类似于**Java 虚拟机**（**JVM**）。区块链虚拟机提供一种在公共网络中执行不受信任代码的方式。这些虚拟机还可以提供对抗攻击（如**拒绝服务**（**DoS**）攻击）的安全性，这是在执行不受信任代码的系统中必不可少的功能。

提供这些服务的一些区块链平台如下：

+   **EOS**：一种智能合约平台和去中心化操作系统，旨在通过每秒进行数百万次交易来解决区块链的可扩展性问题。

+   **以太坊**：这是最著名的智能合约平台。它在其区块链上实现了一个几乎图灵完备的语言。它使用一个名为 Solidity 的领域特定语言，在**以太坊虚拟机**（**EVM**）上编译和执行。

+   **Hyperledger Fabric**：这是由 Linux Foundation 主办的 Hyperledger 项目下的一个许可区块链项目。它允许执行称为**链码**的智能合约。它还允许共识机制作为组件插入。

+   **NEO**：这是一个区块链平台，允许智能合约用几种通用编程语言编写，例如 C＃、Python 和 JavaScript。我们将在下一节详细介绍 NEO 区块链。

+   **NXT**：这是一个公共区块链平台，执行了有限的智能合约模板选择。它并没有太多的空间来创建复杂的合约。

选择创建智能合约的平台取决于需要构建的应用类型、所需的性能、智能合约语言以及许多其他因素。在选择平台之前考虑所有的要求是很重要的。就我们的所有权证明应用而言，它需要一个能处理数字资产和数字身份的平台。

NEO 区块链提供了处理数字身份和资产的便利方式。除此之外，智能合约可以用通用编程语言编写，以构建去中心化应用程序。我们将用 Python 编程语言创建我们的智能合约，这样我们就不需要掌握任何额外的编程语言。以太坊是另一个平台，可以便捷地实现各种用例。我们将在这两个平台上实现我们的所有权证明用例，因为它们是最常用的区块链平台，同时对这两个平台的介绍将为去中心化应用程序开发打下坚实的基础。

在接下来的章节中，我们将深入探讨 NEO 和以太坊区块链平台，以及所有权证明的实现。

# NEO 区块链

NEO 是一个区块链平台和加密货币，能够促进数字资产和智能合约的创建和管理。NEO 项目最初于 2014 年以 AntShares 的名义发布，直到 2017 年 6 月更名为 NEO。所有的开发资源都是由创始人达·宏飞的区块链解决方案公司 Onchain 提供的。NEO 项目的主要目的是在分布式网络中实现**智能经济**，借助数字资产、数字身份和智能合约。

该平台使用两种类型的代币，称为 NEO 和 GAS。与比特币不同，NEO 代币是不可分割的，这意味着 NEO 的最小单位为 1. 持有 NEO 代币可以在共识机制中行使投票权，该机制在*共识算法*部分有解释。持有 NEO 代币会产生一种名为 GAS 的新代币，用于支付交易费。GAS 代币就像燃料一样，如果你想在区块链上部署和执行任何智能合约，就必不可少。创世区块中发行了 1 亿个 NEO 代币。但相应的 1 亿个 GAS 代币将在大约 22 年内逐渐发行。

# NEO 区块链的构建块

NEO 区块链使用了去中心化网络的两个重要元素，以创建去中心化应用程序，因此构建了智能经济。这些元素是数字资产和数字身份，它们是任何区块链应用程序的基石。这些概念，连同智能合约，在之前的章节中已经提到，对于任何所有权证明应用程序的创建是至关重要的。

NEO 提供了在去中心化 NEO 区块链网络中创建和管理数字资产的便利方式。NEO 提供了两种不同类型的资产：

+   全局资产

+   合约资产

全局资产记录在系统中，并且所有客户端和智能合约都能识别。NEO 和 GAS 代币是全局资产。合约资产与特定合约绑定，不能被其他合约识别。只有某些兼容的客户端才能访问合约资产。基于 NEP-5 的资产就是合约资产的一个例子。

NEP-5 是 NEO 指定的用于创建加密代币的标准。该标准帮助开发人员在构建与代币相关的应用程序时保持模板。NEP-5 代币类似于以太坊中使用的 ERC-20 代币。我们将在第十二章中实施使用 NEP-5 代币的用例，*区块链使用案例*。

NEO 利用数字身份在区块链中处理物理资产和数字资产之间的连接。NEO 实现了**X.509**公钥证书颁发标准来创建数字身份。借助区块链，NEO 可以取代**在线证书状态协议**（**OCSP**）来管理和记录 X.509 **证书吊销列表**（**CRL**）。

# NEO 技术

NEO 提供了几种功能来作为可扩展的区块链平台运行。下面讨论了 NEO 中使用的一些技术。

# 共识算法

NEO 使用**委托拜占庭容错**（**dBFT**），这是一种改进的拜占庭容错共识算法。它是一种允许区块链参与者通过代理投票达成共识的机制。一组特殊的节点，称为记账人，通过投票达成共识以在网络中生成新区块。这些记账人是由 NEO 代币持有人通过投票选举产生的。dBFT 算法具有*f =* ⌊ *(n-1) / 3* ⌋ of *n*节点的容错能力，即约 33%的节点。一旦生成和确认了区块和交易，几乎不可能撤销它们。 

在 NEO 中生成一个区块大约需要 15 到 20 秒，而且它的吞吐量为每秒 1000 笔交易，与基于工作量证明的实现相比，这是非常高的。由于其高的交易吞吐量，NEO 的应用可以轻松扩展。

# NEO 智能合约

智能合约是 NEO 区块链突出特点之一。在 NEO 区块链系统中编写智能合约相对于其他智能合约平台来说是一个相当简单的过程，主要是因为它支持大量通用目的语言，可用于创建智能合约。与以太坊不同，NEO 不需要特定领域的语言来创建和执行智能合约。

NEO 拥有一个轻量级的虚拟机，类似于 Java 中的 JVM。NEO 的虚拟机按顺序执行智能合约指令。NEO 虚拟机只执行由 NEO 编译器编译的指令。NEO 计划支持 C＃，Java，C，C ++，Go，JavaScript，Python 和 Ruby 等语言的编译器；尽管并非所有语言都已实现，但正在进行开发以支持大多数语言。除了这些编译器，NEO 目前还支持 Java 和 C＃的 IDE 插件。这有助于开发人员在不改变开发生态系统的情况下创建智能合约。

# 其他 NEO 项目

NEO 是一个不断发展的社区，其路线图中有许多项目；你会在以下列表中找到最受欢迎的一些样本：

+   **NeoX**：NEO 的这一特性将允许不同链之间的资产交换。它将提供原子资产交换协议，以确保交易要么完全处理，要么完全拒绝。即使是不兼容的区块链，只要提供一些基本的智能合约功能，也可以通过 NeoX 进行通信。由于 NeoX 将帮助实现跨链协作，一个智能合约可以在两条链上执行操作。

+   **NeoFS**：这是一种将使用 **分布式哈希表** (**DHT**) 技术的分布式存储机制。 每个文档将以其内容的摘要索引。 大型文档被分割成块并分布在区块链节点之间。 NeoFS 计划为存储需要更高可靠性的文档的节点提供代币奖励。 NeoFS 节点可用于存储 NEO 区块链的旧块数据，以减轻完整节点的负载。

+   **NeoQS**：NEO 计划解决量子计算对加密算法的挑战。 NeoQS 计划开发量子安全的加密机制。

尽管所有这些项目都在开发中，但 NEO 的路线图看起来非常有前景。 NeoFS 和 NeoQS 将于 2018 年第三季度提供研究更新，而 NeoX 预计将在 2018 年最后一个季度进行初步测试。

# NEO 节点

与任何其他区块链平台一样，NEO 也有保存完整区块链历史记录的节点，称为完整节点。 这些完整节点构成了网络的骨干，并且它们使用 P2P 协议进行通信。

# 入门指南

NEO 支持两种变体的完整节点，一种具有图形用户界面，另一种仅支持命令行。 名为 **NEO-GUI** 的图形用户界面变体提供了用户所需的所有功能。 **NEO-CLI** 针对想要使用基本钱包功能和 API 的开发人员。 使用任何一个变体都非常简单。 我们将主要处理 NEO-CLI，因为它更适合开发人员。

# 配置完整节点

原始的 NEO-CLI 实现是用 C# 编写的，源代码可以在 [`github.com/neo-project/neo-cli`](https://github.com/neo-project/neo-cli) 找到。 这个实现需要一个已安装 .NET Core 的用户节点来运行编译的二进制文件。 该存储库说明了如何在不同环境中设置 .NET Core。 源代码中的一个 **动态链接库** (**DLL**) 文件需要执行才能运行 NEO-CLI。 一旦在您的系统上安装了 .NET Core，以下命令就会启动 NEO-CLI 完整节点进程：

```
$ dotnet neo-cli.dll 
```

完整节点使用三个不同的端口进行 JSON-RPC (`10332`)、通过 TCP 进行 P2P (`10333`) 和通过 WebSocket 进行 P2P (`10334`)。 除此之外，JSON-RPC 还有一个 HTTPS 版本 (`10331`)。 NEO 对测试网和私有网使用不同的端口集。 它使用初始的 "1" 替换为 "2" 用于测试网和 "3" 用于私有网。 在本章中，我们将主要处理私有网节点，因为我们将创建自己的网络来部署应用程序。

可以通过运行以下带有 `/rpc` 标志的命令来暴露节点的 JSON-RPC 接口：

```
$ dotnet neo-cli.dll /rpc 
```

NEO-CLI 打开一个交互式界面，用户可以在其中执行所有区块链节点和钱包操作：

```
NEO-CLI Version: 2.7.4.0 
neo> 
```

在执行任何管理钱包或节点的命令之前，用户必须创建一个钱包或打开一个现有的钱包。在 NEO-CLI shell 中，以下命令创建并打开一个钱包：

```
create wallet neo_wallet.db3 
password: *** 
address: ASxUka4WqmEkD2mJtGy37J9NeuTe8bTtYF 
pubkey: 0374c66e892d7a8cbbbd4c8bd5b7b71ec83819a90c2327d7057b1234072291b5d8 

open wallet neo_wallet.db3 
```

用户需要提供一个密码来保护钱包，每次解锁钱包都会使用这个密码。用户可以在打开钱包后执行任何钱包、交易或区块操作。

您可以参考 [`docs.neo.org/en-us/node/cli/cli.html`](http://docs.neo.org/en-us/node/cli/cli.html) 查看 NEO-CLI 文档中的所有命令。

NEO-CLI 支持在区块链中测试、构建和部署智能合约，但目前不支持用多种编程语言编写智能合约。NEO-CLI 只提供 .NET 和 Java 的编译器。我们需要第三方编译器来在任何其他编程语言中创建智能合约。**neo-python** 就是这样一个项目。它由 City of Zion 组织支持，该组织由一群积极贡献的开发人员组成。我们将在本章中使用 neo-python 项目来构建我们的应用程序。

# 设置 neo-python 环境

neo-python 项目提供了一个 NEO 节点和一个 SDK，使开发人员可以使用 Python 在 NEO 区块链上创建、测试、部署和执行智能合约。该项目支持你在钱包和区块链节点中管理资产所需的所有功能。该项目旨在完全移植 NEO-CLI 实现。

要设置 neo-python，您需要安装 Python 3.6 解释器。neo-python 可以安装在任何平台上，尽管需要执行一些特定于平台的步骤。在安装 neo-python 之前，需要安装 `leveldb` 和 `openssl` 库。

从快速入门到构建复杂智能合约，neo-python 的完整文档可以在 [`neo-python.readthedocs.io`](https://neo-python.readthedocs.io) 找到。

neo-python 软件包可以像任何其他 Python 软件包一样从 **PyPI** 安装，也可以从源代码库安装。安装完成后，可以使用 `np-prompt` 命令启动 neo-python 的交互式 shell。其 shell 界面如下所示，与 NEO-CLI 类似：

```
$ np-prompt 
NEO cli. Type 'help' to get started 
neo> 
```

neo-python 也可以通过从 [`github.com/CityOfZion/neo-python`](https://github.com/CityOfZion/neo-python) 克隆存储库并以开发模式安装 neo-python 软件包来安装。然后可以使用 `np-prompt` 命令启动 neo-python shell，或者简单地运行 Python 脚本 `neo/bin/prompt.py`。

在打开钱包后，可以像在 NEO-CLI 上一样执行任何命令。

虽然在 NEO-CLI 上执行的所有操作在 neo-python 中也可以执行，但有些命令语法与 NEO-CLI 命令不同。在 neo-python shell 中键入 `help` 列出所有命令。

# 设置节点的 JSON-RPC 接口

如在设置节点时的前一节所指定的，NEO 节点充当 JSON-RPC 服务器，以便可以使用 RPC 接口进行通信。可以通过在 NEO-CLI 中添加 `/rpc` 标志来实例化 JSON-RPC 服务器，如前所述。您需要启动一个不同的进程来在 neo-python 中创建 RPC 服务器：

```
$ np-api-server --port-rpc 10332 
```

就像比特币的 JSON-RPC 接口一样，NEO 为其每个 API 提供了一个 RPC 终端点：

```
$ curl -X POST http://localhost:10332 -H 'Content-Type: 
 application/json' -d '{ "jsonrpc": "2.0", "id": 5, "method": 
 "getversion", "params": [] }' 

{"jsonrpc": "2.0", "id": 5, "result": {"port": 8080, "nonce": 
 1439440988, "useragent": "/NEO-PYTHON:0.6.6/"}} 
```

大多数前端应用程序使用 JSON-RPC 与去中心化应用程序和区块链本身通信。

**neon-js** 由 City of Zion 维护，提供了使用 NEO 节点公开的 RPC 接口与区块链通信的 JavaScript 库。

# NEO 网络

NEO 使用网络协议来建立连接并与每个节点双工通信。网络中的节点根据其责任被分类为两种类型：验证节点（记账节点）和普通对等节点。对等节点在它们被验证后帮助广播区块和未确认的交易，而记账节点生成新的区块。NEO 遵循与比特币类似的网络协议，以启动连接并在对等节点之间交换区块。

当我们通过 NEO-CLI 或 neo-python 启动 NEO shell 时，节点将加入默认配置中指定的网络。neo-python 节点可以属于主网、测试网或私有网络。如果启动时未指定网络，则 NEO 节点将加入测试网络。以下命令将节点启动在私有网络中，该私有网络协议配置文件位于 `neo/data/` 中：

```
$ np-prompt -p 
```

我们将在本章中使用私有网络执行所有 neo-python 操作。可以通过显式指定网络配置文件并使用 `-c` 标志来启动节点。

# 测试网络

NEO 测试网络类似于主网。在测试网络中，用户可以开发、部署和执行程序。用户可以使用没有实际价值的测试代币，而不是花费真实的 GAS 和 NEO 代币。在测试网上执行的每个其他操作与在主网上执行的操作相同。因此，这是开发人员在部署到主网之前测试应用程序的理想环境。由于测试网是一个来自世界各地的参与者的活跃网络，NEO 和 GAS 代币的供应是有限的，当您加入网络时，将不会提供任何代币，就像在主网上一样。由于部署智能合约需要至少 500 GAS，用户可以从其他测试网用户那里获取这些 GAS，或者在 [`neo.org/Testnet/Create`](https://neo.org/Testnet/Create) 申请。用户在拥有足够 GAS 后可以将智能合约部署到网络上。

# 私有网络

私有网络是一组 NEO 节点，它们自行实现区块链状态一致性。这些 NEO 节点与主网或测试网的公共节点完全隔离。私有网络非常适合在组织内部创建区块链网络。

私有 NEO 网络需要至少四个节点才能达成共识。私有 NEO 网络可以部署在局域网中，并且还可以通过创建虚拟机在单个设备上部署多个节点。即使私有网络的节点也需要 GAS 来在私有区块链中创建和部署智能合约。私有节点可以通过从所有共识节点创建多方签名地址来提取网络中的所有 NEO 和 GAS 代币。然后可以将 NEO 和 GAS 从联系地址转移到普通地址。

通过部署托管在 [`hub.docker.com/r/cityofzion/neo-privatenet`](https://hub.docker.com/r/cityofzion/neo-privatenet) 的即插即用 Docker 镜像，可以创建具有有限共识节点的小型私有网络。此 Docker 镜像部署了四个 NEO 验证节点，并预领取了所有 1 亿 NEO 和 16,600 GAS 代币。加入网络的任何用户都可以使用包含所有代币的钱包部署智能合约。只需几个命令即可启动私有网络：

```
$ docker pull cityofzion/neo-privatenet
$ docker run --rm -d --name neo-privatenet -p 20333-20336:20333-
 20336/tcp -p 30333-30336:30333-30336/tcp cityofzion/neo-privatenet 
```

Docker 镜像将创建一个包含四个节点的容器，同时公开 P2P（20333-20336）端口和 RPC 端口（30333-30336）。

在相同网络中的任何 neo-python 节点都可以将这四个节点添加为其种子节点，并开始同步私有网络区块链。然后用户可以使用包含所有网络代币的钱包创建交易和部署智能合约。

# NEO 交易

NEO 网络上的每个节点都可以在 NEO 区块链中创建交易并执行操作。节点必须打开钱包才能创建交易并进行广播：

```
open wallet neo_wallet.db3
wallet 
```

neo-python 中的 `wallet` 命令为您提供有关已打开钱包的完整信息：

```
{ 
  "path": "neo_wallet.db3", 
  "addresses": [ 
    { 
      "version": 0, 
      "script_hash": "AK2nJJpJr6o664CWJKi1QRXjqeic2zRp8y", 
      "frozen": false, 
      "votes": [], 
      "balances": { 
        "0xc56f33fc6ecfcd0c225c4ab356fee59390af8560be0e930faebe74
 a6daff7c9b": "100000000.0", 
        "0x602c79718b16e442de58778e148d0b1084e3b2dffd5de6b7b16cee
 7969282de7": "16024.0" 
      }, 
      "is_watch_only": false 
    } 
  ], 
  "height": 20066, 
  "percent_synced": 100, 
  "synced_balances": [ 
    "[NEO]: 100000000.0 ", 
    "[NEOGas]: 16024.0 " 
  ], 
  "public_keys": [ 
    { 
      "Address": "AK2nJJpJr6o664CWJKi1QRXjqeic2zRp8y", 
      "Public Key": "031a6c6fbbdf02ca351745fa86b9ba5
 a9452d785ac4f7fc2b7548ca2a46c4fcf4a" 
    } 
  ], 
  "tokens": [], 
  "claims": { 
    "available": "143992.0", 
    "unavailable": "480.0" 
  } 
} 
```

钱包在每笔交易验证后维护更新的详细信息。`addresses` 字段包含钱包持有的所有密钥的详细信息。已打开的钱包仅有一个密钥，其公共地址为 `AK2nJJpJr6o664CWJKi1QRXjqeic2zRp8y`。`addresses` 字段内的 `balances` 字段显示节点当前拥有的 NEO 和 GAS 代币。在 `balances` 字段中，`0xc56f33fc6ecfcd0c225c4ab356fee59390af8560be0e930faebe74a6daff7c9b` 表示 NEO 代币的交易 ID，而 `0x602c79718b16e442de58778e148d0b1084e3b2dffd5de6b7b16cee7969282de7` 是 GAS 代币的交易 ID。NEO 将这些用作 NEO 和 GAS 代币的标准 ID。

与比特币节点不同，NEO 节点可以创建多种不同类型的交易来支持区块链中执行的所有操作。以下表格描述了可以在 NEO 网络中创建的不同类型的交易：

| **名称** | **描述** |
| --- | --- |
| `MinerTransaction` | 分配字节费用 |
| `IssueTransaction` | 发行资产 |
| `ClaimTransaction` | 领取 NEO 币 |
| `EnrollmentTransaction` | 注册为验证者 |
| `VotingTransaction` | 为验证者投票 |
| `RegisterTransaction` | 资产注册 |
| `ContractTransaction` | 合约交易 |
| `AgencyTransaction` | 订单交易 |

表格 7.1\. NEO 中的交易类型

每种类型的交易都将具有专用字段来存储有关交易的更多信息。例如，`MinerTransaction` 有一个额外的字段来存储 `nonce`，它是一个随机数。

# 资产转移

节点可以使用交易对 NEO 资产执行操作。交易由节点使用其在钱包中的私钥创建。一旦打开钱包，用户就可以对资产执行任何操作。neo-python 中的 `send` 命令通过接收资产 ID、接收地址和金额来转移资产。`send` 命令创建一个交易并将其中继到网络，以便将其包含在区块链中：

```
send NEO AZ81H31DMWzbSnFDLFkzh9vHwaDLayV7fU 100 
[Password]> *** 
Relayed Tx: 53b72dbce63a28a01432c1ddcc82aed8c28fb1fa338cab812c979
 d56cc8e4410 
```

创建的交易是一种 `ContractTransaction`，详细信息显示它包含 `vout` 和 `vin` 字段，其功能类似于比特币交易中的字段。`vin` 字段指向被引用其未花费输出的交易，而 `vout` 由新创建的未花费输出组成。第一个输出是用户交易的金额，第二个是输出的变化。与比特币交易不同，验证脚本在一个单独的 script 字段下找到：

```
{ 
  "txid": "0x53b72dbce63a28a01432c1ddcc82aed8c28fb1fa338cab812
 c979d56cc8e4410", 
  "type": "ContractTransaction", 
  "version": 0, 
  "attributes": [ 
    { 
      "usage": 32, 
      "data": "23ba2703c53263e8d6e522dc32203339dcd8eee9" 
    } 
  ], 
  "vout": [ 
    { 
      "n": 0, 
      "asset": "0xc56f33fc6ecfcd0c225c4ab356fee59390af8560be0e930
 faebe74a6daff7c9b", 
      "value": "100", 
      "address": "AZ81H31DMWzbSnFDLFkzh9vHwaDLayV7fU" 
    }, 
    { 
      "n": 1, 
      "asset": "0xc56f33fc6ecfcd0c225c4ab356fee59390af8560be0e930
 faebe74a6daff7c9b", 
      "value": "99999900", 
      "address": "AK2nJJpJr6o664CWJKi1QRXjqeic2zRp8y" 
    } 
  ], 
  "vin": [ 
    { 
      "txid": "2b8907db07ebbc3ea2244162ff3d696e7b80874d3ddc3f1fc52
 e427d91cd91c3", 
      "vout": 0 
    } 
  ], 
  "sys_fee": "0", 
  "net_fee": "0", 
  "scripts": [ 
    { 
      "invocation": "40f6b2e5c2ca932a536284136c254119096813ee35
 d494c939d9e26a7b6247f0801284a34c39e0194c35d1db68bf54fa1de2852
 b86182d86a673a206dcf64c6f04", 
      "verification": "21031a6c6fbbdf02ca351745fa86b9ba5a9452d785
 ac4f7fc2b7548ca2a46c4fcf4aac" 
    } 
  ], 
  "height": 20594, 
  "unspents": [ 
    { 
      "n": 0, 
      "asset": "0xc56f33fc6ecfcd0c225c4ab356fee59390af8560be0e930
 faebe74a6daff7c9b", 
      "value": "100", 
      "address": "AZ81H31DMWzbSnFDLFkzh9vHwaDLayV7fU" 
    }, 
    { 
      "n": 1, 
      "asset": "0xc56f33fc6ecfcd0c225c4ab356fee59390af8560be0e930
 faebe74a6daff7c9b", 
      "value": "99999900", 
      "address": "AK2nJJpJr6o664CWJKi1QRXjqeic2zRp8y" 
    } 
  ] 
} 
```

# 创建一个去中心化应用程序

现在我们已经了解了 NEO 平台的一些基本功能，我们准备使用 NEO 区块链创建我们的第一个去中心化应用程序。智能合约是使用 NEO 创建去中心化应用程序的基础。在创建去中心化所有权证明应用程序之前，我们将通过创建一个 hello world 应用程序来熟悉智能合约。

# 基本智能合约

首先，我们将创建一个简单的 Python 脚本，返回一个连接的字符串来向用户问候：

```
from boa.builtins import concat 

def main(name): 
  return concat("Hello ", name) 
```

合约脚本使用 `boa` 提供的 `concat` 方法来连接两个字符串。每个智能合约都应该有一个名为 `main` 的函数，这将是入口点。智能合约需要编译成字节码，可以在 NeoVM 中执行。合约可以通过 neo-python shell 使用 neo-boa 编译器编译，如下所示：

```
build hello.py test 07 07 False False Alice 
```

`build` 命令带有一个 `test` 参数，用于测试样本结果。`test` 标志之后的代码表示参数和返回类型的数据类型。前面的代码规定了合约函数接受一个字符串参数并返回一个字符串值。下表列出了所有可用的数据类型及其代码：

| **数据类型** | **代码** |
| --- | --- |
| Signature | `0x00` |
| 布尔值 | `0x01` |
| Integer | `0x02` |
| Hash160 | `0x03` |
| Hash256 | `0x04` |
| 字节数组 | `0x05` |
| 公钥 | `0x06` |
| 字符串 | `0x07` |
| 数组 | `0x10` |
| 互操作接口 | `0xf0` |
| 空 | `0xff` |

表 7.2。合约参数使用的数据类型

在构建过程中，跟随数据类型后面的第一个布尔值规定合约是否需要本地存储，第二个布尔值指示合约是否动态调用其他智能合约，其地址仅在执行期间才知道。 在构建过程中，合约的任何输入都遵循这些参数。测试调用将显示结果，以及调用合约所需的 GAS。 结果显示输出是 `string` 类型，以及它的值。 最重要的是，`build` 调用通过创建 AVM 文件将合约指令生成为字节码，并存储在同一目录中：

```
Calling hello.py with arguments ['Alice'] 
Test deploy invoke successful 
Used total of 19 operations 
Result [{'type': 'String', 'value': 'Hello Alice'}] 
Invoke TX gas cost: 0.0001 
```

生成的 AVM 文件需要导入到 NeoVM，然后中继到区块链网络。以下的 `import` 调用执行合约导入。 `import` 命令接受类似于构建过程中指定的参数：

```
import contract hello.avm 07 07 False False Alice 
```

用户需要在创建和中继到网络之前输入智能合约的详细信息：

```
Please fill out the following contract details: 
[Contract Name] > Hello World 
[Contract Version] > 1.0.0 
[Contract Author] > Alice 
[Contract Email] > alice@neotest.com 
[Contract Description] > Basic smart contract 
Creating smart contract.... 
{ 
  "hash": "0x6ed9fabe179b236ca7c22deb72a02bdf65b57b84", 
  "script": 
 "54c56b6a00527ac40648656c6c6f206a00c37e6c75665ec56b6a00527ac46a515
 27ac46a51c36a00c3946a52527ac46a52c3c56a53527ac4006a54527ac46a00c36
 a55527ac461616a00c36a51c39f6433006a54c36a55c3936a56527ac46a56c36a5
 3c36a54c37bc46a54c351936a54527ac46a55c36a54c3936a00527ac462c8ff616
 1616a53c36c7566", 
  "parameters": "07", 
  "returntype": "07" 
} 
Used 100.0 Gas 
```

一旦创建智能合约，将生成脚本哈希以及合约脚本。脚本哈希代表合约，并且可以由网络中的所有人用来调用智能合约。

交易中使用的 GAS 取决于智能合约操作类型和智能合约中使用的系统调用。 创建智能合约的成本为 100 GAS 加上系统调用的额外费用。 如果智能合约需要存储空间，则需要额外花费 400 GAS。 我们之前的智能合约部署只使用了 100 GAS 来创建智能合约，因为没有需要本地存储的东西。

neo-python 提供了一个 `testinvoke` 命令，可以用于在区块链上已部署的合约哈希上进行测试。除非用户接受，否则 `testinvoke` 调用不会被中继到网络。它只接受合约的脚本哈希和其参数：

```
testinvoke 0x6ed9fabe179b236ca7c22deb72a02bdf65b57b84 Alice 
```

一旦节点更新其本地区块链以包含之前创建的中继合约，就可以执行 `testinvoke` 调用。以下是输出：

```
Test invoke successful 
Total operations: 19 
Results ['48656c6c6f20416c696365'] 
Invoke TX GAS cost: 0.0 
Invoke TX fee: 0.0001 
```

`testinvoke` 从区块链中调用合约，并以十六进制字符串数组的形式返回计算结果。十六进制结果 `'48656c6c6f20416c696365'` 转换为 "Hello Alice"，这是期望的输出。

# 所有权证明应用

我们之前已经详细讨论了在本章早期在分散应用中创建所有权证明应用的好处；现在我们将继续创建一个使用 NEO 智能合约来执行资产管理的所有权证明应用，以在分散网络中执行。

我们创建了一个“存在证明”应用程序来证明第六章中文档的存在，*深入区块链 - 存在证明*。在本节中，我们将创建一个资产管理系统来注册和证明本节中文档的所有权。目标是创建以下资产管理功能：

+   资产注册

+   资产查询

+   资产移除

+   资产转移

我们将在智能合约中实现所有功能以证明文档的所有权。

# 创建智能合约

使用 Python 创建的智能合约包含`main`函数作为入口点。此函数接受两个参数。第一个参数接受操作类型，所有附加参数都传递给第二个参数的列表。`operation`参数接受`register`、`query`、`delete`和`transfer`，以便执行资产管理功能。

智能合约的`main`函数解析`operation`参数，并调用智能合约中的相应函数以对资产执行操作。`main`函数解析`args`参数，并将列表的第一项分配给`asset_id`，其他项分配给`owner`：

```
from boa.interop.Neo.Runtime import Log, Notify 
from boa.interop.Neo.Storage import Get, Put, GetContext 
from boa.interop.Neo.Runtime import GetTrigger,CheckWitness 
from boa.builtins import concat 

def main(operation, args): 
  nargs = len(args) 
  if nargs == 0: 
    print("No asset id supplied") 
    return 0 

  if operation == 'query': 
    asset_id = args[0] 
    return query_asset(asset_id) 

  elif operation == 'delete': 
    asset_id = args[0] 
    return delete_asset(asset_id) 

  elif operation == 'register': 
    if nargs < 2: 
      print("required arguments: [asset_id] [owner]") 
      return 0  
    asset_id = args[0] 
    owner = args[1] 
    return register_asset(asset_id, owner) 

  elif operation == 'transfer': 
    if nargs < 2: 
      print("required arguments: [asset_id] [to_address]") 
      return 0 
    asset_id = args[0] 
    to_address = args[1] 
    return transfer_asset(asset_id, to_address) 
```

`register_asset`函数接受`asset_id`和所有者地址，并在区块链中创建所有权条目。`CheckWitness`是一个 NEO 运行时功能检查，检查所有者地址是否与调用合同的用户地址匹配。如果要注册的资产所有者与调用合同的用户不同，则合同返回`False`。合同通过调用 NEO 存储库的`Get`方法来验证是否已经注册了`asset_id`。最后，通过使用`Put`存储方法将资产 ID 和所有者详细信息存储在键/值对中，将资产注册给所有者：

```
def register_asset(asset_id, owner): 
  msg = concat("RegisterAsset: ", asset_id) 
  Notify(msg) 

  if not CheckWitness(owner): 
    Notify("Owner argument is not the same as the sender") 
    return False 

  context = GetContext() 
  exists = Get(context, asset_id) 
  if exists: 
    Notify("Asset is already registered") 
    return False 

  Put(context, asset_id, owner) 
  return True 
```

NEO 通过在区块链中存储数据的方式提供存储功能，采用键/值对。智能合约必须在部署时指定脚本是否需要合约存储空间。在部署期间使用存储会额外消耗 GAS。

`query_asset`函数查询本地存储以检查用户是否已经注册了资产。如果找到资产，则返回所有者地址：

```
def query_asset(asset_id): 
  msg = concat("QueryAsset: ", asset_id) 
  Notify(msg) 

  context = GetContext() 
  owner = Get(context, asset_id) 
  if not owner: 
    Notify("Asset is not yet registered") 
    return False 

  Notify(owner) 
  return owner 
```

以下功能需要`asset_id`和资产接收者地址以便转移资产。作为第一步，它通过使用`Get`方法检查存储来验证资产的存在。然后检查资产所有者是否与调用者相同。合同还验证了接收者地址是否是有效地址。最后，使用`Put`存储方法更新资产的新所有者：

```
def transfer_asset(asset_id, to_address): 
  msg = concat("TransferAsset: ", asset_id) 
  Notify(msg) 

  context = GetContext() 
  owner = Get(context, asset_id) 
  if not owner: 
    Notify("Asset is not yet registered") 
    return False 

  if not CheckWitness(owner): 
    Notify("Sender is not the owner, cannot transfer") 
    return False 

  if not len(to_address) != 34: 
    Notify("Invalid new owner address. Must be exactly 34 
 characters") 
    return False 

  Put(context, asset_id, to_address) 
  return True 
```

`delete_asset` 方法实现与 `transfer_asset` 类似的功能，不同之处在于它删除资产而不是更新它。`Delete` 函数调用用于从存储中删除存储的键值对:

```
def delete_asset(asset_id): 
  msg = concat("DeleteAsset: ", asset_id) 
  Notify(msg) 

  context = GetContext() 
  owner = Get(context, asset_id) 
  if not owner: 
    Notify("Asset is not yet registered") 
    return False 

  if not CheckWitness(owner): 
    Notify("Sender is not the owner, cannot transfer") 
    return False 

  Delete(context, asset_id) 
  return True 
```

现在，我们已经实现了资产管理的所有基本功能，我们将在下一部分中使用 neo-python shell 执行合约。

# 执行智能合约

智能合约的执行步骤与我们部署基本智能合约的之前部分类似。唯一的区别是提供给合约的参数和相应的返回数据不同。

正如前面提到的，我们将创建一个所有权证明应用来跟踪文件。每个文件可以通过其摘要唯一标识。我们将使用文件的 SHA256 哈希值作为资产 ID。让我们考虑一个具有以下内容的文件。这些文件通常存储有一个额外的看不见的换行符:

```
This document was created by Alice. 
```

以下摘要表示文件的 SHA256 哈希值:

```
f572f8ce40bf97b56bad1c6f8d62552b8b066039a9835f294ea4826629278df3 
```

让我们使用哈希值作为资产 ID 来唯一标识每个文件。合同在 neo-python shell 中使用以下命令构建:

```
build poo.py test 0710 05 True False query 
 ["f572f8ce40bf97b56bad1c6f8d62552b8b066039a9835f294ea4826629278
 df3"] 

Test deploy invoke successful 
Used total of 113 operations 
Result [{'type': 'ByteArray', 'value': ''}] 
Invoke TX gas cost: 0.0001 
```

`build` 过程以 `0710` 作为参数类型，表示它以一个字符串 (`07`) 和一个数组 (`10`) 作为参数。`05` 表示它具有`字节数组`返回类型。

成功构建后，可以使用创建的 AVM 文件部署合约:

```
import poo.avm 0710 05 True False 
{ 
  "hash": "0x60a7ed582c6885addf1f9bec7e413d01abe54f1a", 
  "script": "....", 
  "parameters": "0710", 
  "returntype": "05" 
} 
Used 500.0 Gas 
```

由于智能合约需要本地存储，交易需要额外的 400 GAS。总共消耗的 GAS 将为 `500`，如前面的代码块所示。

一旦合约交易包含在区块链中并在本地区块链中同步，合约就可以执行。让我们使用 `testinvoke` 命令来测试我们创建的智能合约。

让我们使用先前提到的相同的 SHA256 值作为资产 ID 注册文件。注册操作调用一个参数列表，包括哈希值和调用智能合约的地址:

```
testinvoke 0x60a7ed582c6885addf1f9bec7e413d01abe54f1a register 
 ["f572f8ce40bf97b56bad1c6f8d62552b8b066039a9835f294ea4826629278
 df3", "AK2nJJpJr6o664CWJKi1QRXjqeic2zRp8y"] 
```

一旦交易被调用并传递到网络上，其他操作就可以在资产上执行。现在让我们通过调用 `transfer` 操作并在列表中指定接收者地址来将文件所有权转让给新所有者：

```
testinvoke 0x60a7ed582c6885addf1f9bec7e413d01abe54f1a transfer 
 ["f572f8ce40bf97b56bad1c6f8d62552b8b066039a9835f294ea4826629278
 df3", "AZ81H31DMWzbSnFDLFkzh9vHwaDLayV7fU"] 
```

可以随时通过调用查询操作并传递资产 ID 来验证文件所有权：

```
testinvoke 0x60a7ed582c6885addf1f9bec7e413d01abe54f1a query 
 ["f572f8ce40bf97b56bad1c6f8d62552b8b066039a9835f294ea4826629278
 df3"] 

Test invoke successful 
Total operations: 118 
Results ['415a3831483331444d577a62536e46444c466b7a683976487761444
 c617956376655'] 
Invoke TX GAS cost: 0.0 
Invoke TX fee: 0.0001 
```

查询返回一个具有十六进制字符串的字节数组。十六进制结果表示地址 `AZ81H31DMWzbSnFDLFkzh9vHwaDLayV7fU`。由于文件的所有权已转移给新所有者，结果显示文件的更新所有者。

我们已经完成了创建智能合约的过程，以演示一个跟踪文档所有权的系统。一旦智能合约在区块链上部署，它将永远留在那里。用户只需处理智能合约的调用。NEO 节点的 RPC 接口提供了一种方便的方式来与区块链进行通信。现在，我们将看看如何通过创建一个接口来方便地与区块链进行通信。

# 应用程序的界面

NEO 社区创建了一些 JavaScript 库，用于与 NEO 区块链进行交互。我们将使用一个名为 neon-js 的流行库([`github.com/CityOfZion/neon-js`](https://github.com/CityOfZion/neon-js))，该库得到了 City of Zion 社区的支持。

以下脚本创建了一个接口，用于查询我们所有权证明应用程序中资产的所有者：

```
queryAsset(assetID) {
  const props = { 
    scriptHash: '60a7ed582c6885addf1f9bec7e413d01abe54f1a', 
    operation: 'query', 
    args: [assetID.hexEncode()]
  }; 
  const Script = Neon.create.script(props); 

  rpc.Query.invokeScript(Script).execute('http://localhost:30333')
 .then((res) => { 
    return res.result.stack[0].value.hexDecode() 
  }); 
} 
```

使用 neon-js 库创建的接口使用`Neon.create.script`方法构建了智能合约脚本。然后，它使用 RPC 接口调用智能合约脚本。之后，`queryAsset`返回拥有文档资产的用户地址。

创建与区块链智能合约的接口是构建完整去中心化应用程序的关键部分。该接口还为与区块链节点通信提供了便捷的方式，从而增强了用户体验。

# 以太坊区块链

以太坊是一个公共区块链，由 Vitalik Buterin 于 2013 年末提出，并于 2015 年向公众发布。以太坊是最初创建的区块链平台之一，旨在帮助程序员使用智能合约开发和部署去中心化应用程序。以太坊拥有丰富的框架和库，用于开发、测试和部署应用程序。本节我们将介绍使用以太坊平台开发和部署所有权证明应用程序的开发和部署。有关以太坊生态系统的更多详细信息，请参阅第八章，*区块链项目*。

# 以太坊节点

与比特币和 NEO 类似，不同语言中存在多种客户端软件实现，可用作以太坊完整节点。以太坊客户端实现可以在 Java、JavaScript、Python、Go 等多种语言中找到。以太坊的 Golang 实现称为**Go Ethereum**或**Geth**，是其中最受欢迎的。

# 入门

在深入应用开发之前，设置节点是必不可少的一步。尽管任何以太坊客户端都可以用来设置完整节点，我们将设置 Geth 客户端以与公共区块链同步并进行交互。与 NEO 区块链类似，Geth 客户端可以连接到主网、测试网或私有网络。

# 设置节点

我们将设置一个 Geth 客户端，用于同步整个区块链交易。它还提供了 JSON-RPC 接口，以调用客户端软件支持的任何方法。JSON-RPC 接口可用于执行多个操作，包括部署和调用智能合约。

可以通过在大多数平台上找到的软件包来构建或安装 Geth 客户端。不同平台的安装说明可以在 [`github.com/ethereum/go-ethereum/wiki/Building-Ethereum`](https://github.com/ethereum/go-ethereum/wiki/Building-Ethereum) 找到。

Geth 提供了一个命令行界面，可用于启动节点。一旦安装了所有依赖项的 Geth，就可以启动它以将本地区块链数据与公共区块链同步。可以通过提供几个参数来配置 Geth 客户端，用于链、交易池、性能调优、账户、网络、矿工等。以下命令使用了几个参数来实例化以太坊节点：`rpc`（启用 RPC 服务器）、`rpcapi`（通过 RPC 接口访问的 API 列表）、`cache`（用于内部缓存的内存分配）、`rpcport`（RPC 服务器端口）和 `rpcaddr`（RPC 服务器地址）：

```
$ geth --rpc --rpcapi db,eth,net,web3,personal --cache=2048 
 --rpcport 8545 --rpcaddr 127.0.0.1 
```

实例化的以太坊节点将尝试通过连接到主网节点来同步区块链。或者，可以配置 Geth 来连接到以太坊测试网络（**Rinkeby**、**Kovan** 或 **Ropsten**）。以下命令实例化了一个带有 Rinkeby 测试网络区块链的节点：

```
$ geth -rinkeby 
```

一组 Geth 客户端也可以形成自己的私有以太坊网络，而不是连接到现有的主网或测试网。

使用 Geth 客户端设置私有网络的说明可以在这里找到：[`github.com/ethereum/go-ethereum/wiki/Setting-up-private-network-or-local-cluster`](https://github.com/ethereum/go-ethereum/wiki/Setting-up-private-network-or-local-cluster)。

在本章中，我们将使用**Truffle**套件框架中提供的工具来创建一个私有区块链，该工具称为 **Ganache CLI**，这是一个 JavaScript 包，可以使用 node 包管理器进行安装。在执行以下命令之前，请确保系统中安装了 `node` 和 `npm`：

```
$ npm install -g ganache-cli 
```

可以通过启动 Ganache CLI 来实例化私有区块链。可以通过指定几个参数来配置 Ganache CLI，也可以在没有参数的情况下启动，如下所示的命令：

```
$ ganache-cli 
```

成功启动将创建一个私有区块链，并加载了一些以太币的账户，可用于支付任何创建的交易的交易费用。 Ganache CLI 还将创建一个客户端应用程序，默认情况下将侦听端口 8545。我们将使用私有区块链和运行在端口 8545 上的应用程序，在接下来的章节中部署和查询智能合约。

# 设置开发环境

现在我们已经实例化了一个带有私有区块链的本地节点，我们将设置开发环境以便轻松创建和部署去中心化应用程序。由于我们需要创建一个完全成熟的去中心化应用程序，我们需要使用 JavaScript 等脚本语言与以太坊区块链进行通信。以太坊提供了一个称为 **web3.js** 的 JavaScript 库，其中包含与以太坊区块链交互的 API。

web3.js 使用 RPC 调用与暴露 RPC 接口（应用程序端口 8545）的以太坊节点进行通信。因此，web3.js 可以调用以太坊节点提供的任何方法。以下代码片段可在节点终端上执行：

```
Web3 = require('web3') 
web3 = new Web3(new 
 Web3.providers.HttpProvider("http://localhost:8545")); 
web3.eth.getBlockNumber(console.log); 
```

这段代码将创建一个 `web3` 实例，然后将本地节点添加为提供程序。当在 web 浏览器中执行 web3.js 应用程序时，`web3` 对象可以通过 MetaMask 等桥接注入。在构建所有权证明应用程序时，我们将使用 MetaMask。

尽管 `web3.js` 提供了与区块链交互所需的所有方法，但它没有设置完整的开发环境。这可以通过称为 Truffle 的以太坊开发框架来实现。Truffle 提供了一个完整的开发环境，以及方便的智能合约测试、部署和迁移。可以使用节点包管理器安装 Truffle 框架：

```
$ npm install -g truffle 
```

可以在空目录中初始化一个 Truffle 项目，以构建应用程序开发所需的初始文件：

```
$ truffle init 
```

这将创建三个目录 - `contracts`（智能合约）、`migrations`（部署脚本）和 `test`（测试脚本）以及一个配置文件，`truffle.js`。我们需要向 `truffle.js` 文件添加以下配置，以将创建的 Truffle 项目指向我们的私有区块链：

```
module.exports = { 
  networks: { 
    development: { 
      host: '127.0.0.1', 
      port: 8545, 
      network_id: '*' } 
  } 
}; 
```

可以从 Truffle 项目目录启动交互式 Truffle 控制台：

```
$ truffle console 
```

`web3` 对象将在 Truffle 控制台中已经被实例化，并且可以使用这个对象访问任何 web3 API。我们将在下一节使用 Truffle 控制台查询部署的合约。

# 创建去中心化应用程序

由于我们已经设置好了开发环境，我们现在可以在以太坊平台上创建我们的第一个去中心化应用程序。我们将通过创建并部署一个 hello world 应用程序来熟悉以太坊智能合约。

# 基本智能合约

以太坊使用一种称为 Solidity 的特定领域语言来编写智能合约的逻辑。Solidity 是一种高级编程语言，可以编译生成字节码，然后在 EVM 上执行。

Solidity 是一种由 Gavin Wood 最初提出的静态类型编程语言。它被设计为与 ECMAScript 语法类似，以便它可以被 Web 开发人员社区轻松适应。有关 Solidity 编程语言的更多细节可以在[`solidity.readthedocs.io`](https://solidity.readthedocs.io)找到。

我们将使用 Solidity 编程语言创建一个简单的 hello world 智能合约：

```
pragma solidity ⁰.4.23; 

contract Hello { 

  function greetUser(bytes user) view public returns (bytes) { 
    return abi.encodePacked("Hello ", user); 

  } 
} 
```

Solidity 脚本的第一行是版本`pragma`，用于指示 solidity 程序的版本。前面的脚本不应该在版本早于 0.4.23 的 Solidity 编译器上编译。每个合约被定义为一个类，类名与文件名相同。所有函数都被定义在合约内部。一个构造函数也可以被创建，其名称与合同的名称相同。`greetUser`函数接受一个`bytes`类型的字符串并返回一个`bytes`字符串。函数同时具有公共可见性，意味着它可以从任何地方调用。`greetUser`函数将串联并返回两个字符串。

我们将使用 Truffle 框架来部署和调用智能合约。我们需要通过在`migrations`文件夹内包括以下代码片段的 JavaScript 文件（`2_deploy_contracts.js`）或通过更新现有的`1_initial_migration.js`文件将智能合约文件指向 Truffle 框架：

```
var Hello = artifacts.require("./Hello.sol"); 
module.exports = function(deployer) { 
  deployer.deploy(Hello); 
}; 
```

可以使用以下 Truffle 命令编译智能合约：

```
$ truffle compile 
```

它将在`build/contracts`文件夹中生成一个名为**应用二进制接口**（**ABI**）的接口文件。生成的 ABI 文件将是 JSON 格式的，并且它提供了在以太坊生态系统中与合约交互的接口。

然后通过迁移将合约部署到区块链上：

```
$ truffe migrate 
Deploying Hello... 
  ... 0x4d85f83c2ffcf1405eb7b610e0f34c99f42b4189f11fbb5ffb782b6eb4d96316 
  Hello: 0x149cd2285f8b8a72a5f8b7286aceb94fb54c1aee 
```

部署的合约将生成一个合约地址，可用于与之交互。我们将使用 Truffle 控制台与合约交互：

```
$ truffle console 
truffle(development)> 
Hello.deployed().then((instance) => instance.greetUser("Alice")); 
```

当在 Truffle 控制台上执行前面的代码片段以使用`Alice`作为参数调用合约的`greetUser`函数时，合约将返回`0x48656c6c6f20416c696365`，这是一个代表"Hello Alice"的十六进制字符串。

# 所有权证明应用

我们将创建一个具有资产管理功能的应用程序，用于注册、查询、移除和转移资产。

# 创建智能合约

应用程序的逻辑类似于 NEO 智能合约中使用的逻辑。但由于虚拟机提供的功能，用于存储资产信息的数据结构不同。以太坊合约与 NEO 合约之间的一个主要区别是，以太坊合约中的函数可以直接通过 ABI 的帮助进行调用。

`ProofOfOwnership`智能合约使用映射数据结构以键值对的形式存储资产所有权信息。类型为`bytes32`的资产信息映射到资产所有者的以太坊地址：

```
pragma solidity ⁰.4.23; 

contract ProofOfOwnership { 
  mapping (bytes32 => address) public assetOwners; 
```

`registerAsset`函数将调用合约的用户地址与资产 ID 映射到映射数据结构中：

```
  function registerAsset(bytes32 asset) public { 
    if (address(assetOwners[asset]) == address(0))
      {
        assetOwners[asset] = msg.sender;
      }
  } 
```

资产所有者可以通过映射数据结构中存储的信息检索：

```
  function queryAsset(bytes32 asset) view public returns (address) 
  { 
    return assetOwners[asset]; 
  } 
```

`transferAsset`函数在验证资产当前所有者与调用合约的用户相同时，将所有权转移到新地址：

```
  function transferAsset(bytes32 asset, address owner) public { 
    if (assetOwners[asset] == msg.sender) 
    { 
      assetOwners[asset] = owner; 
    } 
  } 
```

当资产所有者调用合约时，`deleteAsset`函数将为资产分配一个空地址：

```
  function deleteAsset(bytes32 asset) public { 
    if (assetOwners[asset] == msg.sender) 
    { 
      assetOwners[asset] = 
 0x0000000000000000000000000000000000000000; 
    } 
  } 
} 
```

与以前的智能合约部署类似，应在新的 JavaScript 文件（`2_deploy_contracts.js`）中创建以下配置，放在`migrations`文件夹内，或者应更新现有的`1_initial_migration.js`文件：

```
var ProofOfOwnership= artifacts.require("./ProofOfOwnership.sol"); 
module.exports = function(deployer) { 
  deployer.deploy(ProofOfOwnership); 
}; 
```

智能合约随后可以使用 Truffle 框架进行编译和迁移，就像前面的例子一样：

```
$ truffle migrate 
Deploying ProofOfOwnership... 
  ... 0x0902a793d20a3846935fffa9558fb8a2f59f74edd2ef811189c9dcdd0a0aedcc 
  ProofOfOwnership: 0xc58b4e456b840ca924ddc1c971932febec717e95 
```

# 执行智能合约

让我们考虑在 NEO 应用程序中以前使用的跟踪文档所有权的相同例子。我们将使用相同的文件，并且具有以下内容作为资产：

```
This document was created by Alice. 
```

我们将使用 md5（32 个字符）哈希算法来计算该文件的资产 ID，而不是使用 SHA256（64 个字符）。这是因为我们合约中映射数据结构的键（`bytes32`）只能接受 32 个字符。以下是 md5 算法生成的文件的 32 个字符或 128 位哈希值：

```
c9f50a3bdd2efccb7e34fbd8b42e9675 
```

一旦所有权智能合约部署到区块链上，就可以在 Truffle 控制台中调用。让我们使用`registerAsset`函数注册资产：

```
$ truffle console 
truffle(development)> 
ProofOfOwnership.deployed().then((instance)=>
 instance.registerAsset("c9f50a3bdd2efccb7e34fbd8b42e9675", 
 {from: "0xebe41ec4c574fde7a1d13d333d17267ca93df491"})); 
```

如果调用节点有多个帐户，`from`地址可以包含在函数调用中，以标识调用合约的用户。由于`registerAsset`函数执行写操作，它在执行期间需要 GAS。一旦创建交易，将显示使用的总 GAS。

`transferAsset`将资产 ID 与新所有者地址作为参数：

```
ProofOfOwnership.deployed().then((instance)=>
 instance.transferAsset("c9f50a3bdd2efccb7e34fbd8b42e9675", 
 "0xfda013eecad647a2593aacbb3c18445f051d0f52", 
 {from: "0xebe41ec4c574fde7a1d13d333d17267ca93df491"})); 
```

我们可以随时查询资产，检查文档的所有者：

```
ProofOfOwnership.deployed().then((instance)=>
 instance.queryAsset("c9f50a3bdd2efccb7e34fbd8b42e9675")); 
```

如果查询返回`0xfda013eecad647a2593aacbb3c18445f051d0f52`作为当前所有者，则我们成功执行了`transferAsset`函数。

# 应用程序的接口

通过将前端应用程序与以太坊区块链集成，可以创建一个完整的分散式应用程序。我们可以利用`web3.js`库提供的 API 与区块链网络交互。我们已经用 Truffle 开发环境部署和调用了智能合约。在本节中，我们将利用 Truffle 库与合约通信。

以下代码可以在任何 JavaScript 运行环境中执行，例如 Node.js。请参考本书的 GitHub 仓库，查找使用 React 库的实现。

`ProofOfOwnership.json` 是在合同编译期间创建的 ABI 文件。这个 ABI 对于与智能合同通信是必不可少的：

```
import Web3 from 'web3'; 
import { default as contract } from 'truffle-contract'; 
import contract_artifacts from 
 './contracts/ProofOfOwnership.json'; 
```

如果代码在安装了 MetaMask 等桥接应用程序的浏览器中执行，web3 对象将与提供程序一起注入到浏览器中。web3 对象也可以通过将提供程序设置为本地或任何其他远程 RPC 服务器节点来创建。

MetaMask 浏览器插件可以从 [`metamask.io`](https://metamask.io) 安装。安装插件后，必须创建一个帐户。用户必须将 MetaMask 指向用于开发的私有区块链。由私有区块链（通过 Ganache CLI）创建的帐户可以导入到 MetaMask 钱包中，以便用于支付交易 GAS。

```
const web3 = window.web3; 
if (typeof web3 !== 'undefined') 
{ 
  this.web3 = new Web3(web3.currentProvider); 
  this.user_address = this.web3.eth.accounts[0] 
} 
else 
{ 
  this.web3 = new Web3
 (new Web3.providers.HttpProvider("http://localhost:8545")); 
} 
```

可以从导入的 ABI 创建合同实例。然后可以使用此合同实例调用任何合同函数：

```
this.poo = contract(contract_artifacts); 
this.poo.setProvider(this.web3.currentProvider); 
```

以下函数将通过将 `assetID` 作为参数传递来调用合同的 `registerAsset` 函数。当该函数从浏览器执行时，MetaMask 将弹出一个窗口，询问是否确认交易。在确认交易后，合同函数将被执行：

```
registerAsset(assetID) 
{ 
  try { 
    let user_address = this.user_address; 
    this.poo.deployed().then(function(contractInstance) { 

      contractInstance.registerAsset(assetID, {gas: 1400000, from: user_address}).then(function(c) { 
        console.log(c.toLocaleString()); 
      }); 
    }); 
  } 
  catch (err) { 
    console.log(err); 
  } 
} 
```

类似地，查询函数将调用智能合同的 `queryAsset` 函数。由于 `queryAsset` 不会写入区块链，MetaMask 将不会创建新的交易：

```
  queryAsset(assetID) 
  { 
    try { 
      let user_address = this.user_address; 
      this.poo.deployed().then(function(contractInstance) { 

        contractInstance.queryAsset
 (assetID, {gas: 1400000, from: user_address}).then(function(c) { 
          console.log(c.toLocaleString()); 
        }); 
      }); 
    } 
    catch (err) { 
      console.log(err); 
    } 
  } 
```

所有其他的产权证明应用功能都可以用类似的方式实现。参考 GitHub 仓库（[`github.com/PacktPublishing/Foundations-of-Blockchain`](https://github.com/PacktPublishing/Foundations-of-Blockchain)）获取应用程序的完整前端实现。

现在我们已经使用 NEO 和以太坊区块链平台实现了产权证明应用程序，我们有足够的信息来在这些平台上构建其他应用程序。由于我们还比较了两个平台的功能，我们可以根据需求决定最适合实现任何用例的平台。

# 总结

在本章中，我们深入探讨了创建和使用智能合同，以及使用 NEO 和以太坊平台构建去中心化应用程序。在创建 NEO 和以太坊区块链的基础之上，我们创建了一个证明资产所有权的系统。

本章希望通过向您介绍智能合同来激励您开发去中心化应用程序。在下一章中，我们将通过探索来自不同领域的项目，探索区块链技术的实际应用。
