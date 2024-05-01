# 第五章：暴露网络资产和交易

如果您已经走到这一步，恭喜您！您已经建立了区块链应用程序的核心以及直接读取、更重要的是操纵您网络的记录系统的智能合约。但是，您还没有完成。正如您所能想象的那样，合约是一段敏感的代码，必须受到不当使用或篡改的保护。

要开发一个健壮且安全的应用程序，安全地发布给业务用户，您必须将智能合约与一层或多层保护包装起来，并将其设计为客户端可以通过适当的保障远程访问的服务。此外，希望共享分类帐和智能合约的各方可能具有独特和特定的业务逻辑需求，这些需求只有他们需要实现，其他人不需要。因此，运行一个智能合约的区块链应用程序可能最终会为不同的利益相关者提供不同的视图和功能。

在本章中，您将首先学习如何从头开始构建一个完整的区块链应用程序，使用我们的贸易应用程序作为指南和示例。稍后，您将了解到设计这个应用程序所需考虑的各种因素，以及如何将该应用程序与现有系统和流程集成。

本章将涵盖以下主题：

+   构建完整的应用程序

+   将应用程序与现有系统和流程集成

# 构建完整的应用程序

在本节中，您将学习如何围绕核心智能合约构建一个完整的应用程序，该应用程序可以被已经加入一起形成网络的商业实体方便地使用。我们将从回顾 Hyperledger Fabric 交易流程开始，以提醒读者用户（或客户端）的角度，一个区块链应用程序是如何做的（以及如何做的）。通过代码示例，我们将向您展示如何根据商业实体的需求构建、设计和组织网络，创建适当的配置，并完成从开始到结束的区块链交易的不同阶段。在此过程结束时，读者将了解如何设计一个 Fabric 应用程序，并通过一个简单的 web 界面展示其功能。在本章开始时，我们唯一需要拥有的资产是合约或链代码，它是使用 Go 编程（请参阅 *第四章*，*使用 Golang 设计数据和交易模型*）手工开发的。

在本章的后端，我们将引导经验丰富的企业开发人员深入了解更高级的主题，例如服务设计模式、可靠性和其他常见的工程问题。尽管这些问题适用于每个分布式应用程序，但我们将讨论基于区块链的应用程序的特殊需求和问题。

# Hyperledger Fabric 应用的性质

在之前的章节中，我们看到 Hyperledger Fabric 可以被视为一个分布式事务处理系统，具有分阶段的操作流水线，这些操作最终可能导致由网络对等方维护的共享复制分类帐的状态发生变化。对于开发者来说，区块链应用程序是一组过程，通过这些过程，用户可以向智能合约提交事务，或者从智能合约中读取状态。在底层，开发者必须将用户请求引导到事务流水线的不同阶段，并提取结果以在流程结束时提供反馈。基本上，无论合约是手工实现的（参见 *第四章*，*使用 Golang 设计数据和事务模型*）还是使用 Hyperledger Composer 实现的（参见 *第六章*，*业务网络*），应用程序开发者的工作都是在智能合约周围实现一个或多个包装层。

以智能合约（或资产实体模型）为核心开发的应用可以被视为具有一组视图或服务 API 的事务处理数据库应用程序。然而，开发人员必须牢记每个 Hyperledger Fabric 事务都是异步的，也就是说，事务的结果在提交的通信会话中不会立即可用。这是因为，正如我们在之前的章节中所看到的，一个事务必须通过共识被网络中的对等方集体批准。因此，共识可能需要花费不确定的时间，并且事务结果的通信被设计为发布/订阅机制。以下图表从开发者的角度说明了区块链应用程序和事务流水线：

![](img/89fa0e47-d4c1-475e-9c6f-1d256edffb9b.png)

图 5.1：区块链应用程序创建和运行中的阶段

在接下来的部分中，将详细描述图中提到的操作，并将其映射到特定的 Fabric 机制。

# 应用程序和事务阶段

创建应用程序的第一步是实例化区块链，或者说共享分类帐本身。在 Fabric 的术语中，区块链的一个实例被称为通道，因此区块链应用程序的第一步是创建一个通道，并使用通道的创世区块引导网络排序服务。

下一步是对等网络的初始化，所有被选中运行应用程序的对等节点必须加入通道，这个过程允许每个对等方维护一个分类帐的副本，该副本被初始化为一个空白的键值存储。加入通道的每个对等方都将拥有分类帐提交特权，并可以参与八卦协议以与其他对等方同步分类帐状态。

在对等网络创建完成后，将在该网络上安装智能合约。在此之前连接到通道的一部分对等方将被选中来运行智能合约；换句话说，它们将拥有背书特权。合约代码将部署到这些对等方并构建以进行后续操作。如你所知，到这一步，合约在 Fabric 术语中被称为链码，本章的其余部分将使用这个术语。

一旦链码被安装在背书对等方上，将根据嵌入其中的逻辑进行初始化（参见*第四章*，*使用 Golang 设计数据和交易模型*，以获取示例）。

到这一点为止，除非在前面的一个或多个步骤中出现了问题，否则应用程序已经启动并运行。现在，可以将交易发送到链码，以更新分类帐的状态（调用）或读取分类帐状态（查询）在应用程序的生命周期内。

应用可能随着时间的推移而发生变化或演化，需要执行一些未在*图 5.1：区块链应用创建和运行阶段*中捕获的特殊操作。这些将在*第九章*中描述，*区块链网络中的生活*。

在标题为<q>构建应用</q>及其后面的部分，我们将展示如何围绕*第四章*中开发的链码构建贸易应用，使用适当的代码和说明。

# 应用模型和架构

编写 Fabric 应用程序的过程始于链码，但最终开发人员必须就终端用户或软件代理如何与该链码进行接口设计做出明智的决策。链码的资产以及运行该链码的区块链网络的操作应该如何暴露给用户是一个需要仔细处理的问题。如果这些功能暴露出去而没有限制，特别是涉及区块链引导和配置的功能，可能会造成严重的损害。链码本身的正确运行不仅依赖于其内部逻辑，还依赖于在其上构建的适当访问控制。正如我们在前一节中看到的，设置应用程序并为其准备使用是一个复杂的过程。此外，分类帐更新事务的异步性质需要在链码和用户之间设置一个仲裁层。为了让用户专注于影响应用程序而不是网络模块的细节，所有这些复杂性都应该尽可能隐藏起来。因此，一个三层架构作为 Fabric 应用程序的标准已经成为标准，如下图所示：

![](img/f51a2b7c-1eaf-409a-a9c4-be83b96bb12a.png)

图 5.2 Hyperledger Fabric 应用程序的典型三层架构

在最底层是直接在共享账本上运行的智能合约，可以使用一个或多个链码单元编写。这些链码在网络对等体上运行，为调用和查询提供服务 API，并发布事务结果的事件通知，以及通道上发生的配置更改。

在中间层中，存在着协调区块链应用程序各个阶段的功能（参见*图 5.1：区块链应用程序创建和运行的各个阶段*）。Hyperledger Fabric 提供了一个 SDK（目前在`Node.js`和 Java 中都可用）来执行诸如通道创建和加入、用户注册和注册、以及链码操作等功能。此外，SDK 提供了机制来订阅来自网络的事务和配置相关事件。根据应用程序的需求，可以维护一个离链数据库以方便使用，或者作为分类帐状态的缓存。

最顶层是一个面向用户的应用程序，导出一个服务 API，主要由特定于应用程序的功能组成，尽管管理员操作（如频道和链码操作）也可能暴露给系统管理员。通常，提供用户界面以便使用，尽管如果用户是软件代理，一个明确定义的 API 可能就足够了。我们简单地称这一层为应用程序，因为这是最终用户（或代理）将看到的内容。此外，考虑到任何区块链应用程序和网络都是多样参与者的聚合体，这一层通常将由为不同参与者量身定制的多个应用程序堆栈组成。

这种架构不应该被定死；它的目的纯粹是为开发人员提供一个指导方针。根据应用程序的复杂性，层的数量和垂直方面（或不同的应用程序）可能会有所不同。对于一个非常简单的应用程序，具有少量功能的开发人员甚至可以选择将中间件和应用程序层压缩到一起。然而更普遍的是，这种解耦使不同的能力集可以暴露给不同的网络参与者。例如，在我们的交易用例中，监管机构和出口商会以不同的方式查看区块链并有不同的需求，因此建立为它们构建不同服务集比强行将所有功能适配成一个单片应用程序并具有统一界面更有用。但是这两个应用程序都应该隐藏网络操作的复杂性，比如频道的创建和加入，或特权操作，例如将链码安装到对等体上，这样从一个共同的中间件层中可以受益。

用户直接互动的应用程序层可以设计出许多选择和复杂性，我们将在本章的后半部分深入探讨这些内容。不过，首先，我们将描述如何实现 Fabric 应用程序的关键部分，重点放在基本要素上。出于指导目的，我们的最顶层将是一个简单的 Web 服务器，提供*RESTful*服务 API。

这种架构背后的思想和推动它的原则独立于基础的区块链技术。要在与 Hyperledger Fabric 不同的区块链平台上实现相同的应用程序，只需重新实现智能合约和一些中间件的部分。应用程序的其余部分可以保持不变，最终用户不会注意到任何差异。

# 构建应用程序

现在我们不仅了解了设计分层 Fabric 应用程序的方法，还了解了其背后的哲学，我们可以深入实施了。在前两章中，我们讨论了如何实现和测试最底层，或者说链代码。因此，我们可以假设读者现在已经准备好添加中间件和应用层，这正是我们将在以下章节中演示的内容。

测试中间件和应用程序代码的先决条件是运行网络。在继续下一节之前，请确保我们在*第三章*中配置和启动的示例四组织网络仍在运行中。

# 中间件 - 封装和驱动链代码

以下图示了在《应用与交易阶段》部分讨论的交易阶段，并在*图 5.1：区块链应用创建和运行阶段*中加以说明，以 Fabric 术语和使用 Fabric 术语来表示：

![](img/3111cf77-037e-44bd-b5fd-8f6b70d31d27.png)

图 5.3：区块链应用创建和运行阶段

Fabric 对等方、排序器和 CA（或 MSP）使用 gRPC（[`grpc.io/`](https://grpc.io/)）进行通信，以及由对等方生成的用于运行链代码的进程（该进程实际上是一个 Docker 容器）。该进程导出一个实现 JSON RPC 2.0 规范（[`www.jsonrpc.org/specification`](http://www.jsonrpc.org/specification)）的服务端点，用于通道和链代码操作。我们可以编写一个直接使用服务规范与链代码通信的包装应用程序，但然后我们将不得不编写逻辑来解析和解释负载。由于 Fabric 平台及其规范可能会在未来发生变化，因此这不一定是编写应用程序的最佳和最可维护的方式，特别是对于生产目的而言。幸运的是，Hyperledger Fabric 提供了两种不同的方式来运行链代码操作，同时隐藏接口规范和通信协议的细节：

+   **命令行界面**（**CLI**）：Fabric 提供了可以从终端运行的命令，用于执行*图 5.3：区块链应用创建和运行阶段*中指示的各种操作。运行这些命令的工具是`peer`，它在下载 Fabric 源代码并构建它（使用`make`或只是`make peer`）时生成。可以使用不同的开关与此命令一起使用以执行不同的通道和链代码操作，在本节中您将看到一些示例。

+   **软件开发工具包**（**SDK**）：Hyperledger 提供了一个工具包和一组库，用于轻松开发多种语言（如 Node.js、Java、Go 和 Python）的应用程序，以包装通道和链码操作。这些 SDK 还提供了与 MSPs 或 Fabric CA 实例进行交互的功能。

尽管 CLI 工具可用于测试和演示目的，但它们不足以用于应用程序开发。SDK 库除了前面提到的功能外，还提供了订阅来自网络的事件的能力，传达关于需要驱动应用程序逻辑的状态变化的信息。我们将使用 Node.js SDK 演示如何构建我们的中间件和更高层次的应用程序。读者可以使用其他 SDK 之一来构建自己选择的其他语言中的等效应用程序。

# 工具和依赖项的安装

我们将展示如何在我们的中间件中构建的函数可以在代码库的中间件文件夹中找到。

# 创建和运行中间件的先决条件

阅读者应该熟悉 Node.js/JavaScript 编程（尤其是`Promise`模式）以及 Node.js 和`npm`工具的用法：

1.  安装 Node.js ([`nodejs.org/en/download/`](https://nodejs.org/en/download/)) 和 `npm` ([`www.npmjs.com/get-npm`](https://www.npmjs.com/get-npm))。

1.  安装 `fabric-client` 和 `fabric-ca-client npm` 库：

    +   您可以从`npm`注册表中安装这些包，可以手动运行 `npm install <package-name>` 或者通过在 `package.json` 文件中设置名称和版本。例如，中间件文件夹中的 `package.json` 在依赖项部分包含以下条目：

        +   `fabric-ca-client`: ¹.1.0

        +   `fabric-client`: ¹.1.0

1.  这告诉`npm`安装这两个包的 1.1.0 版本：

    +   或者，您可以克隆 Fabric SDK node ([`github.com/hyperledger/fabric-sdk-node/`](https://github.com/hyperledger/fabric-sdk-node/)) 源代码库，并按如下方式本地导入这两个库：

        +   在 `fabric-client` 和 `fabric-ca-client` 文件夹中运行 `npm install` 

        +   安装这些包作为依赖项，可以手动在中间件/`package.json`文件中指定路径，也可以使用`npm`链接命令将符号链接添加到中间件/`node_modules`中的包中

在接下来的章节中，我们将使用 `fabric-client` 库执行涉及对等体和订购者的通道和链码操作，以及 `fabric-ca-client` 库执行涉及 CA（或 MSP）的用户注册和注册操作。

# 依赖项的安装

在中间件文件夹中运行 `npm install` 来安装 `package.json` 中指定的包（库及其依赖项）。您应该会看到这些包下载到 `node_modules` 文件夹中。

安装依赖项和配置中间件以进行常规操作的更干净的方法是使用 `Makefile` 进行自动构建。您只需在 `middleware` 文件夹中运行 `make`；有关设置和构建您的开发和测试环境的更多详细信息，请参阅 *第八章* *区块链网络中的灵活性*。

# 创建和运行中间件

我们现在将编写函数来执行和编排图 5.3 中所示的阶段：*区块链应用程序的创建和操作中的各个阶段*。但首先，我们将概述必须设置的各种配置参数，以使应用程序按预期工作。

# 网络配置

编写中间件的第一步是收集所有必要的配置信息，以识别并连接到我们在上一节中创建和启动的网络的各个元素。在 JavaScript 中编写代码时，将这些配置表示为 JSON 格式是很有用的。在我们的示例代码中，`config.json` 文件用于此目的。此文件包含一个网络的描述，其属性包含在 trade-network 对象中。此对象的每个属性描述了网络中每个唯一组织的配置，除了一个称为 orderer 的属性，它只是指向订购者节点。（注意：这对于我们的简单网络足够了，只包含一个订购者节点。）让我们通过以 `Exporterorg` 属性为例来检查每个组织描述中必须指定的内容：

```
"exporterorg": {
  "name": "peerExporterOrg",
  "mspid": "ExporterOrgMSP",
  "ca": {
    "url": "https://localhost:7054",
    "name": "ca-exporterorg"
  },
  "peer1": {
    "requests": "grpcs://localhost:7051",
    "events": "grpcs://localhost:7053",
    "server-hostname": "peer0.exporterorg.trade.com",
    "tls_cacerts": "../network/crypto-config/peerOrganizations/exporterorg.trade.com/peers/peer0.exporterorg.trade.com/msp/tlscacerts/tlsca.exporterorg.trade.com-cert.pem"
  }
},
```

`mspid` 值必须与在 `network/configtx.yaml` 中指定的值相匹配，以使我们的中间件与为网络创建的通道工件和加密材料兼容。 CA 的名称和端口信息必须与在 `network/docker-compose-e2e.yaml` 中指定的内容相匹配。由于每个组织只有一个对等体，我们为方便起见将其命名为对等体，尽管可以轻松地为多对等体组织设置定义不同的模式。请注意，对等体导出对等体请求和事件订阅的服务，端口与 `network/base/docker-compose-base.yaml` 中公开的端口相匹配。`server-hostname` 也必须与 `configtx.yaml` 和 docker-compose 配置中指定的内容相匹配。由于我们的网络元素使用 TLS 连接，因此还必须在此指定对等体的 TLS 证书路径。

最后，如果您将前面的模式片段与其他组织的配置进行比较，您将注意到列出的端口与 docker-compose 配置中公开的端口完全匹配。例如，出口商、进口商、承运商和监管机构组织中的对等体分别在端口 `7051`、`8051`、`9051` 和 `10051` 上监听请求。 URL 中的主机名仅指向 localhost，因为这是我们所有网络元素容器运行的地方。

# 支持政策

下一步是为我们的链码制定一个认可策略，该策略将在实例化期间提交到分类帐。该认可策略规定了需要多少个角色和组织的对等方认可分类帐承诺事务（或调用）。在示例代码中，`constants.js`列出了不同的认可策略，其中包含我们中间件使用的各种设置和关键字。我们将使用的是`ALL_FOUR_ORG_MEMBERS`：

```
var FOUR_ORG_MEMBERS_AND_ADMIN = [
  { role: { name: 'member', mspId: 'ExporterOrgMSP' } },
  { role: { name: 'member', mspId: 'ImporterOrgMSP' } },
  { role: { name: 'member', mspId: 'CarrierOrgMSP' } },
  { role: { name: 'member', mspId: 'RegulatorOrgMSP' } },
  { role: { name: 'admin', mspId: 'TradeOrdererMSP' } }
];
var ALL_FOUR_ORG_MEMBERS = {
  identities: FOUR_ORG_MEMBERS_AND_ADMIN,
  policy: {
    '4-of': [{ 'signed-by': 0 }, { 'signed-by': 1 }, { 'signed-by': 2 }, { 'signed-by': 3 }]
  }
};
```

主体列表在策略的 identities 属性中指定，并且引用了四个对等组织的成员（或普通）用户，以及订购者组织的管理员用户。此处的策略属性声明，需要从每个四个对等组织的成员那里获得认可；总共需要四个签名。

在`constants.js`中，变量`TRANSACTION_ENDORSEMENT_POLICY`默认设置为`ALL_FOUR_ORG_MEMBERS`，将在本节后面用于配置通道认可策略。

# 用户记录

对于通道状态和各自组织的用户密钥和证书，我们将使用基于文件的存储，如`clientUtils.js`中所指定的那样：

```
var Constants = require('./constants.js');
var tempdir = Constants.tempdir;
module.exports.KVS = path.join(tempdir, 'hfc-test-kvs');
module.exports.storePathForOrg = function(org) {
  return module.exports.KVS + '_' + org;
};
```

在`constants.js`中，`tempdir`被初始化如下：

```
var tempdir = "../network/client-certs";
```

或者，您还可以使用`os.tmpdir()`函数将存储位置设置为位于操作系统临时文件夹中的子文件夹（比如`<folder-name>`）。在典型的 Linux 系统上，此存储位置将默认为`/tmp/<folder-name>/`，并且将在那里为每个组织创建文件夹。在我们运行各种操作时，我们会看到这些文件夹被生成，并且文件被添加到其中。

# 客户端注册和注册

尽管可以使用`cryptogen`工具静态创建组织用户的加密材料，但是我们必须在中间件中构建能力，动态创建用户身份和凭证，并使这些用户登录到网络以提交交易和查询分类帐状态。这些操作需要特权访问的用户（或管理员）的介入，在`fabric-ca-server`启动时必须创建他们。默认情况下，管理用户被赋予 ID`admin`和密码`adminpw`，这是我们在本节练习中将要使用的。我们创建和启动的网络使用这些默认值，读者可以在`fabric-ca-server`和`network/docker-compose-e2e.yaml`的开始命令中进行修改（以下是`exporter-ca`部分的内容）。

```
fabric-ca-server start --ca.certfile /etc/hyperledger/fabric-ca-server-config/ca.exporterorg.trade.com-cert.pem --ca.keyfile /etc/hyperledger/fabric-ca-server-config/cc58284b6af2c33812cfaef9e40b8c911dbbefb83ca2e7564e8fbf5e7039c22e_sk -b admin:adminpw -d
```

通过管理员创建用户的步骤如下：

1.  从本地存储加载管理员用户凭据

1.  如果凭据不存在，则管理员需要注册或登录到 Fabric CA 服务器，并获取他们的凭据（私钥和注册证书）

1.  由管理用户注册另一个具有给定 ID 的用户，并指定角色和 affiliations 与 Fabric CA 服务器

1.  使用注册时返回的秘钥，enroll 新用户并获得该用户的凭据

1.  将凭据保存到本地存储

可以在`clientUtils.js`中找到相关示例代码，以下代码片段大多来自`getUserMember`函数，该函数使用管理员凭据，需要用户必须 enrolled 的组织名称/ID。还需要传递一个客户端的 handle（`fabric-client`的实例，或一个客户端对象）给该函数：

```
var cryptoSuite = client.getCryptoSuite();
if (!cryptoSuite) {
  cryptoSuite = Client.newCryptoSuite();
  if (userOrg) {
    cryptoSuite.setCryptoKeyStore(Client.newCryptoKeyStore({path: module.exports.storePathForOrg(ORGS[userOrg].name)}));
    client.setCryptoSuite(cryptoSuite);
  }
}
```

上述代码将客户端句柄与本地存储（按组织进行分区）关联，以存储管理员和临时创建的其他用户的凭据：

```
var member = new User(adminUser);
member.setCryptoSuite(cryptoSuite);
```

该代码确保管理员用户句柄将与我们的存储关联：

```
var copService = require('fabric-ca-client/lib/FabricCAClientImpl.js');
var caUrl = ORGS[userOrg].ca.url;
var cop = new copService(caUrl, tlsOptions, ORGS[userOrg].ca.name, cryptoSuite);
return cop.enroll({
  enrollmentID: adminUser,
  enrollmentSecret: adminPassword
}).then((enrollment) => {
  console.log('Successfully enrolled admin user');
  return member.setEnrollment(enrollment.key, enrollment.certificate, ORGS[userOrg].mspid);
})
```

在这里，我们使用`fabric-ca-client`库连接到与给定组织关联的`fabric-ca-server`实例（其 URL 可以从我们的`config.json`获得；例如，出口商组织的`caUrl`将是`https://localhost:7054`）。enroll 函数允许管理员使用 MSP 登录，并获取 enrollment 密钥和证书。

现在我们已经获取了管理员用户的句柄，形式上是 member 对象，我们可以使用它来为用户 ID（用户名表示）enroll 一个新用户，如下所示：

```
var enrollUser = new User(username);
return cop.register({
  enrollmentID: username,
  role: 'client',
  affiliation: 'org1.department1'
}, member).then((userSecret) => {
  userPassword = userSecret;
  return cop.enroll({
    enrollmentID: username,
    enrollmentSecret: userSecret
  });
}).then((enrollment) => {
  return enrollUser.setEnrollment(enrollment.key, enrollment.certificate, ORGS[userOrg].mspid);
}).then(() => {
  return client.setUserContext(enrollUser, false);
}).then(() => {
  return client.saveUserToStateStore();
})
```

在注册过程中，我们可以指定用户的角色，上面的代码中是客户端，允许用户名提交调用和查询到链码。此处指定的 affiliation 是组织内的一个细分，它在 Fabric CA 服务器的配置中指定（[`hyperledger-fabric-ca.readthedocs.io/en/latest/serverconfig.html`](http://hyperledger-fabric-ca.readthedocs.io/en/latest/serverconfig.html)）（更新此配置留给读者作为练习；在这里，我们将使用默认的 affiliation）。使用返回的秘钥，用户名现在已经与服务器 enrolled，并且其密钥和 enrollment 证书被保存。

调用`client.setUserContext`将该用户与客户端句柄关联，`client.saveUserToStateStore`将用户的凭据保存到我们在文件系统上的本地存储。

与获取管理员用户句柄相似的函数还有`getAdmin`和`getMember`，也在`clientUtils.js`中定义。前者检索使用`cryptogen`创建的管理员用户凭据，而后者动态创建一个新的`admin` member。

# 创建一个通道

要创建我们的交易通道，我们首先需要实例化一个`fabric-client`实例，并使用`config.json`中的配置获得一个到 orderer 的 handle（参见`create-channel.js`中的`createChannel`函数）：

```
var client = new Client();
var orderer = client.newOrderer(
  ORGS.orderer.url,
  {
    'pem': caroots,
    'ssl-target-name-override': ORGS.orderer['server-hostname']
  }
);
```

我们使用基于文件的键值存储来保存分类帐的世界状态，如下所示（读者可以尝试其他类型的存储，比如使用 `CouchDB`，使用 `CouchDBKeyValueStore.js`）：

```
utils.setConfigSetting('key-value-store', 'fabric-client/lib/impl/FileKeyValueStore.js');
```

接下来，我们必须为订购者注册一个管理员用户（使用前一部分讨论的机制）。成功注册后，必须提取使用 `configtxgen` 工具创建的通道配置（参见 `network/channel-artifacts/channel.tx`）。此配置文件的路径在 `constants.js` 中设置：

```
let envelope_bytes = fs.readFileSync(path.join(__dirname, Constants.networkLocation, Constants.channelConfig));
config = client.extractChannelConfig(envelope_bytes);
```

现在，我们需要为我们的四个组织中的每个管理员用户注册。这四个管理员以及订购者管理员必须对通道配置进行签名，签名如下收集：

```
ClientUtils.getSubmitter(client, true /*get the org admin*/, org)
.then((admin) => {
  var signature = client.signChannelConfig(config);
  signatures.push(signature);
});
```

`getSubmitter` 函数定义在 `clientUtils.js` 中，它是将给定组织的成员（普通成员或管理员）与客户端对象关联的间接方式。换句话说，它将客户端对象与用户的*签名身份*（凭证和 MSP 标识）关联起来。在底层，`getSubmitter` 使用了我们在前面章节中描述的 `getAdmin`、`getUserMember` 和 `getMember` 函数。

`getOrderAdminSubmitter` 类似于 `getSubmitter`，返回一个与订购者组织的 `admin` 用户关联的句柄。

最后，我们准备构建一个通道创建请求，并将其提交给订购者：

```
let tx_id = client.newTransactionID();
var request = {
  config: config,
  signatures : signatures,
  name : channel_name,
  orderer : orderer,
  txId : tx_id
};
return client.createChannel(request);
```

实际创建通道可能需要几秒钟，因此应用逻辑在返回成功结果之前应该等待一段时间。`channel_name` 参数在 `clientUtils.js` 中设置为 `tradechannel`，这是我们启动网络时设置的（参见 `network/trade.sh`）。

通道创建步骤涉及使用 `configtxgen` 在本章早些时候创建的创世块来初始化区块链。创世块只是追加到链上的第一个*配置块*。配置块包含对通道的规范以及其中所包含的组织等内容；这样的块不包含链码交易。在*第九章*，*区块链网络中的生活*中，我们将再次处理配置块，讨论如何扩展网络。

现在，我们创建一个通道所需做的就是调用 `createChannel('tradechannel')` 函数并等待结果。这是我们测试代码 `createTradeApp.js` 的第一步，它执行了*图 5.3：区块链应用创建和操作阶段*中所示的基本操作序列：

```
var Constants = require('./constants.js');
var createChannel = require('./create-channel.js');
createChannel.createChannel(Constants.CHANNEL_NAME).then(() => { ...... })
```

我们用于将不同签名标识与一个通用客户端对象关联起来，然后在单个过程中对频道配置进行签名的代码纯粹是为了演示目的。在实际生产应用中，属于不同组织的不同用户的签名标识是私有的，并且必须受到保护；因此，没有将它们汇集到一个共同位置的问题。相反，频道配置必须由不同组织的管理员独立签名，并使用某种带外机制传递以累积签名（并验证签名）。在更新配置时也必须使用类似的机制（见*第九章*，*区块链网络中的生活*）。加入通道和安装链码时也必须遵循独立的、去中心化的程序，尽管我们为了方便演示使用了集中式过程来展示基本机制。

# 加入通道

现在 `tradechannel` 已经创建，我们的四个对等方，每个组织一个，必须加入通道，这一步骤在每个节点上初始化分类帐，并准备对等方在其上运行链码和交易。为此，我们需要重用在前一步骤中创建的客户端句柄，或者使用类似操作序列实例化一个。此外，我们必须实例化一个通道句柄，注册订购方，并获取创世块（在使用通道配置在创建步骤中隐式发送到订购方），如 `join-channel.js` 中 `joinChannel` 函数中的以下代码片段所示：

```
var channel = client.newChannel(channel_name);
channel.addOrderer(
  client.newOrderer(
    ORGS.orderer.url,
    {
      'pem': caroots,
      'ssl-target-name-override': ORGS.orderer['server-hostname']
    }
  )
);
tx_id = client.newTransactionID();
let request = { txId : tx_id };
return channel.getGenesisBlock(request);
```

在前述的 `getGenesisBlock` 调用中，交易 ID 参数是可选的。现在，对于每个组织，我们必须获取一个管理员用户的句柄，然后为属于该组织的对等方提交通道加入请求：

```
return ClientUtils.getSubmitter(client, true /* get peer org admin */, org);
for (let key in ORGS[org])
  if (ORGS[org].hasOwnProperty(key)) {
    if (key.indexOf('peer') === 0) {
      data = fs.readFileSync(path.join(__dirname, ORGS[org][key]['tls_cacerts']));
      targets.push(
        client.newPeer(
          ORGS[org][key].requests,
          {
            pem: Buffer.from(data).toString(),
            'ssl-target-name-override': ORGS[org][key]['server-hostname']
          }
        )
      );
    }
  }
}
tx_id = client.newTransactionID();
let request = {
  targets : targets,
  block : genesis_block,
  txId : tx_id
};
let sendPromise = channel.joinChannel(request, 40000);
```

与通道创建过程一样，`getSubmitter` 函数在提交通道加入请求之前将特定组织的管理员的签名标识与客户端对象关联起来。此请求包含创世块以及该组织中每个对等方的配置（从 `config.json` 中每个组织中包含 `peer` 前缀的属性中加载，如上面的代码所示）。

如上所示，给出了宽裕的等待时间，因为此过程可能需要一段时间才能完成。这个加入过程需要由每个组织的管理员独立执行；因此，在我们的测试脚本 `createTradeApp.js` 中，`processJoinChannel` 主函数依次调用了 4 次 `joinChannel(<org-name>)` 函数：

```
var joinChannel = require('./join-channel.js');
joinChannel.processJoinChannel();
```

在典型的生产网络中，每个组织将独立运行加入过程，但仅针对其对等方。我们在存储库中使用的编排代码（`join-channel.js` 中的 `processJoinChannel`）旨在方便和测试。

# 链码的安装

链码的安装导致将源代码复制到我们选择作为背书人的对等方，并且每个安装都与用户定义的版本相关联。主要函数`installChaincode`在`install-chaincode.js`中实现。该函数依次为每个组织调用`installChaincodeInOrgPeers`函数；后者在给定组织的对等体上安装链码。与频道加入一样，在给定组织创建客户端和频道句柄，为该组织的管理员用户注册，并将该用户与客户端句柄关联。下一步是创建安装建议并将其提交给订购者，如下所示：

```
var request = {
  targets: targets,
  chaincodePath: chaincode_path,
  chaincodeId: Constants.CHAINCODE_ID,
  chaincodeVersion: chaincode_version
};
client.installChaincode(request);
```

目标指的是组织中认可的对等体的配置，从`config.json`加载。`chaincodeId`和`chaincodeVersion`可以由调用者设置（默认值在`constants.js`中分别设置为`tradecc`和`v0`），但`chaincodePath`必须指向包含源代码的位置。在我们的场景中，该位置指的是本地文件系统上的路径：`github.com/trade_workflow`。

在 SDK 内部，安装请求将链码的源代码打包到一种称为`ChaincodeDeploymentSpec`（CDS）的规定格式中（[`github.com/hyperledger/fabric/blob/release-1.1/protos/peer/chaincode.proto`](https://github.com/hyperledger/fabric/blob/release-1.1/protos/peer/chaincode.proto)）。然后对该包进行签名（由与客户端对象关联的组织管理员签名），以创建`SignedChaincodeDeploymentSpec`（[`github.com/hyperledger/fabric/blob/release-1.1/protos/peer/signed_cc_dep_spec.proto`](https://github.com/hyperledger/fabric/blob/release-1.1/protos/peer/signed_cc_dep_spec.proto)），然后将其发送到*生命周期系统链码*（LSCC）进行安装。

上述过程描述了每个 Signed CDS 实例仅具有签发安装请求的客户端相关联的标识的签名的*简单*情况。Fabric 支持更*复杂*的场景，其中 CDS 可以（在带外）传递给不同组织的不同客户端，并在收到安装请求之前由每个客户端签名。鼓励读者使用可用的 API 函数和 Fabric 数据结构尝试此变体（[`hyperledger-fabric.readthedocs.io/en/latest/chaincode4noah.html`](http://hyperledger-fabric.readthedocs.io/en/latest/chaincode4noah.html)）。

安装请求的成功取决于检查每个目标对等体的建议响应，如下所示：

```
if (proposalResponses && proposalResponses[i].response && proposalResponses[i].response.status === 200) {
  one_good = true;
  logger.info('install proposal was good');
}
```

最后，为了在整个网络上进行安装协调，我们调用了`install-chaincode.js`中定义的`installChaincode`函数。为了让`fabric-client`知道从哪里加载`chaincode`源代码，我们暂时将进程中的`GOPATH`设置为指向我们项目中的正确位置，即`chaincode`文件夹：

这仅适用于用 Go 编写的`chaincode`

```
process.env.GOPATH = path.join(__dirname,Constants.chaincodeLocation);
```

为了成功安装，`chaincode` 文件夹必须包含一个名为`src`的子文件夹，在其中，安装提案中发送的`chaincode`路径必须指向实际代码。正如你所看到的，这最终解析为我们代码库中的`chaincode/src/github.com/trade_workflow`，其中确实包含了我们在*第四章*中开发的源代码，*使用 Golang 设计数据和事务模型*。

在我们的`createTradeApp.js`脚本中，我们现在可以简单地调用：

```
var installCC = require('./install-chaincode.js');
installCC.installChaincode(Constants.CHAINCODE_PATH, Constants.CHAINCODE_VERSION);
```

在典型的生产网络中，每个组织将独立运行安装过程（在`installChaincodeInOrgPeers`函数中定义），但仅针对其背书对等体。我们在仓库中使用的编排代码（`install-chaincode.js`中的`installChaincode`）是为了方便和测试而设计的。

# 链码的实例化

现在网络中的背书对等体有了链码后，我们必须在我们的通道上实例化该链码，以确保所有分类账副本都使用正确的数据集（或键值对）初始化。这是我们的智能合约设置的最后一步，然后我们才能将其用于常规操作。实例化是调用 LSCC 来初始化通道上的链码的事务，从而将两者绑定并将前者的状态隔离到后者。

此操作应由授权初始化链码的任何组织中心触发（在我们的示例代码中，我们使用进口商组织的管理员）。同样，这遵循了先前安装部分中描述的简单场景，其中链码包由单个组织管理员签名。

默认的通道实例化策略要求任何通道 MSP 管理员触发该操作，但如果需要，可以在签名的 CDS 结构中设置不同的策略。此外，触发实例化操作的实体还必须在通道上配置为写入者。我们使用`configtxgen`隐含地授予了 4 个组织的管理员写入权限，以创建通道配置的过程。（关于通道配置策略的详细讨论超出了本书的范围。）

实现链码实例化的主要函数在`instantiate-chaincode.js`中作为`instantiateOrUpgradeChaincode`实现。此函数既可用于实例化新部署的链码，也可用于更新已在通道上运行的链码（见*第九章*，*在区块链网络中的生活*）与之前的阶段一样，我们必须创建客户端和通道句柄，并将通道句柄与客户端关联。此外，必须将网络中的所有背书对等方添加到通道中，然后必须使用与通道关联的通道对象来初始化通道的与通道相关联的 MSP（来自四个组织中的每一个）：

```
channel.initialize();
```

这设置了通道以验证证书和签名，例如，来自对等方收到的背书。接下来，我们构建了一个实例化提案并将其提交给通道上的所有背书对等方（从`buildChaincodeProposal`函数的片段）：

```
var tx_id = client.newTransactionID();
var request = {
  chaincodePath: chaincode_path,
  chaincodeId: Constants.CHAINCODE_ID,
  chaincodeVersion: version,
  fcn: funcName,
  args: argList,
  txId: tx_id,
  'endorsement-policy': Constants.TRANSACTION_ENDORSEMENT_POLICY
};
channel.sendInstantiateProposal(request, 300000);
```

链码的路径、ID 和版本必须与安装提案中提供的内容匹配。此外，我们必须提供将发送到链码并执行的函数名和参数列表（在我们的链码中，这将执行 `Init` 函数）。还请注意，提案中包含了我们之前设置的背书策略（`Constants.TRANSACTION_ENDORSEMENT_POLICY`），该策略要求来自四个组织的成员对链码调用进行背书。顺序服务返回的提案响应（每个背书对等方一个）必须像在安装阶段一样进行验证。利用前面`channel.sendInstantiateProposal`调用的结果，我们现在必须构建一个实例化交易请求，并将其提交给顺序服务：

```
var proposalResponses = results[0];
var proposal = results[1];
var request = {
  proposalResponses: proposalResponses,
  proposal: proposal
};
channel.sendTransaction(request);
```

对`channel.sendTransaction`的成功响应将使我们的中间件在实例化成功提交的基础上继续进行。然而，这并不意味着实例化将成功地在共享分类账上提交；为此，我们的代码将必须订阅事件，我们将在本节后面看到如何做到这一点。

我们的脚本`createTradeApp.js`触发链码实例化如下：

```
var instantiateCC = require('./instantiate-chaincode.js');
instantiateCC.instantiateOrUpgradeChaincode(
  Constants.IMPORTER_ORG,
  Constants.CHAINCODE_PATH,
  Constants.CHAINCODE_VERSION,
  'init',
  ['LumberInc', 'LumberBank', '100000', 'WoodenToys', 'ToyBank', '200000', 'UniversalFrieght', 'ForestryDepartment'],
  false
);
```

最后一个参数设置为`false`，表示必须执行实例化而不是升级。第一个参数（`Constants.IMPORTER_ORG`）表示实例化请求必须由进口商组织的成员（在这个上下文中是管理员）提交。

如果实例化成功，链码将在 Docker 容器中构建，每个背书对等方对应一个容器，并部署以代表它们接收请求。如果你运行 `docker ps -a`，你应该会看到除了在启动网络时创建的容器外，还有类似以下的内容：

```
CONTAINER ID    IMAGE    COMMAND    CREATED    STATUS    PORTS    NAMES
b5fb71241f6d     dev-peer0.regulatororg.trade.com-tradecc-v0-cbbb0581fb2b9f86d1fbd159e90f7448b256d2f7cc0e8ee68f90813b59d81bf5    "chaincode -peer.add..."    About a minute ago    Up About a minute        dev-peer0.regulatororg.trade.com-tradecc-v0
077304fc60d8    dev-peer0.importerorg.trade.com-tradecc-v0-49020d3db2f1c0e3c00cf16d623eb1dddf7b649fee2e305c4d2c3eb5603a2a9f    "chaincode -peer.add..."    About a minute ago    Up About a minute        dev-peer0.importerorg.trade.com-tradecc-v0
8793002062d7    dev-peer0.carrierorg.trade.com-tradecc-v0-ec83c1904f90a76404e9218742a0fc3985f74e8961976c1898e0ea9a7a640ed2    "chaincode -peer.add..."    About a minute ago    Up About a minute        dev-peer0.carrierorg.trade.com-tradecc-v0
9e5164bd8da1    dev-peer0.exporterorg.trade.com-tradecc-v0-dc2ed9ea732a90d6c5ffb0cd578dfb614e1ba14c2936b0ae785f30ea0f37da56    "chaincode -peer.add..."    About a minute ago    Up About a minute        dev-peer0.exporterorg.trade.com-tradecc-v0
```

# 调用链码

现在我们已经完成了频道的设置，并为交易安装了链码，我们需要实现函数来执行链码调用。我们的代码在`invoke-chaincode.js`的`invokeChaincode`函数中。

调用`chaincode`的过程与我们进行实例化时相同，代码也类似。调用者必须构建一个包含要调用的`chaincode`函数的事务提议，以及要传递给它的参数。仅提供`chaincode`ID（在我们的实现中为`tradecc`）就足以识别`chaincode`进程以引导请求：

```
tx_id = client.newTransactionID();
var request = {
  chaincodeId : Constants.CHAINCODE_ID,
  fcn: funcName,
  args: argList,
  txId: tx_id,
};
channel.sendTransactionProposal(request);
```

与实例化提议的一个区别是，这个操作通常不需要组织中的管理用户；任何普通成员可能就足够了。这个提议必须发送给足够的背书对等体以收集满足我们的背书策略的正确签名集。这是通过将我们网络中的所有四个对等体添加到频道对象中来完成的（必须与之前的阶段一样创建和初始化频道对象）。一旦提议响应被收集并得到验证，与实例化提议一样的方式，必须构建一个交易请求并发送给订购方：

```
var request = {
  proposalResponses: proposalResponses,
  proposal: proposal
};
channel.sendTransaction(request);
```

我们在`createTradeApp.js`中的测试脚本中调用`invokeChaincode`。我们希望执行的链码函数是`requestTrade`，按时间顺序来说，这是应该由进口商角色的用户调用的第一个函数（请回忆我们在`chaincode`中构建了访问控制逻辑，以确保只有进口商组织的成员可以提交`requestTrade`）：

```
var invokeCC = require('./invoke-chaincode.js');
invokeCC.invokeChaincode(Constants.IMPORTER_ORG, Constants.CHAINCODE_VERSION, 'requestTrade', ['2ks89j9', '50000','Wood for Toys', 'Importer']);
```

最后一个参数（`'Importer'`）只是表明进口商组织中要提交此交易请求的用户的 ID。在代码中，如果用户已经在 CA 进行了注册，那么将加载该用户的凭证，否则将使用`clientUtils.getUserMember`函数注册一个具有该 ID 的新用户。

与实例化案例一样，成功的`channel.sendTransaction`调用只表示订购方接受了交易。只有订阅事件才能告诉我们交易是否成功提交到总账簿。

# 查询链码

查询链码的实现相对简单一些，因为它涉及到整个网络，但只需从客户端到对等体的通信。

客户端和频道句柄应该如同之前的阶段一样创建，但这次，我们将选择来自调用者（或客户端）组织的一个或多个对等体与频道对象关联。然后，我们必须创建一个查询请求（与调用提议请求相同）并将其提交给所选的对等体：

```
var request = {
  chaincodeId : Constants.CHAINCODE_ID,
  fcn: funcName,
  args: argList
};
channel.queryByChaincode(request);
```

在将响应返回给调用者之前，可以收集和比较查询的响应。完整的实现可以在 `query-chaincode.js` 中的 `queryChaincode` 函数中找到。我们通过在 `createTradeApp.js` 脚本中运行 `getTradeStatus` 链码查询来测试此函数：

```
var queryCC = require('./query-chaincode.js');
queryCC.queryChaincode(Constants.EXPORTER_ORG, Constants.CHAINCODE_VERSION, 'getTradeStatus', ['2ks89j9'], 'Exporter');
```

与调用一样，我们指定了一个用户 ID（`‘Exporter’`）和组织：这里我们希望出口商组织的成员检查交易请求的状态。

由于查询是对客户端及其相关节点的本地查询，因此响应会立即返回给客户端，无需订阅（如调用的情况）。

# 完成循环 - 订阅区块链事件

正如我们在前几章中所见，权限区块链上的共享账本承诺需要网络节点之间的共识。Hyperledger Fabric 在其 v1 版本中有一个更独特的过程来提交到账本：交易执行、排序和提交过程都是彼此解耦的，并被构建为管道中的阶段，在其中背书者、排序者和提交者独立执行其任务。因此，在 Fabric 方案中，导致区块提交到账本的任何操作都是异步的。我们在中间件中实现的三个操作都属于这一类别：

+   通道加入

+   链码实例化

+   链码调用

在我们对这些操作的描述中，我们停在成功发送请求到排序者的地方。但是，为了完成操作循环，任何使用我们中间件的应用程序都需要知道请求的最终结果，以推动应用程序逻辑向前发展。幸运的是，Fabric 提供了用于异步操作结果通信的发布/订阅机制。这包括用于提交块、完成交易（成功或否）以及可以由链码定义和发出的自定义事件的事件。在这里，我们将检查区块和交易事件，它们涵盖了我们感兴趣的操作。

Fabric 在 SDK 中通过 `EventHub` 类提供了一个事件订阅机制，相关的订阅方法分别是 `registerBlockEvent`、`registerTxEvent` 和 `registerChaincodeEvent`，可以传递回调函数以在中间件层（或更高层）执行操作，每当事件可用时。

让我们看看如何在我们的中间件代码中捕获成功加入的事件。回到 `join-channel.js` 中的 `joinChannel` 函数，以下代码为给定的节点实例化了一个 `EventHub` 对象，其配置从 `config.json` 中加载。例如，要订阅来自出口商组织唯一节点的事件，我们 `fabric-client` 实例将监听的 URL（在内部）是 `grpcs://localhost:7053`：

```
let eh = client.newEventHub();
eh.setPeerAddr(
  ORGS[org][key].events,
  {
    pem: Buffer.from(data).toString(),
    'ssl-target-name-override': ORGS[org][key]['server-hostname']
  }
);
eh.connect();
eventhubs.push(eh);
```

对于每个块事件，监听器或回调被定义如下：

```
var eventPromises = [];
eventhubs.forEach((eh) => {
  let txPromise = new Promise((resolve, reject) => {
    let handle = setTimeout(reject, 40000);
    eh.registerBlockEvent((block) => {
      clearTimeout(handle);
      if(block.data.data.length === 1) {
        var channel_header = block.data.data[0].payload.header.channel_header;
        if (channel_header.channel_id === channel_name) {
          console.log('The new channel has been successfully joined on peer '+ eh.getPeerAddr());
          resolve();
        }
        else {
          console.log('The new channel has not been succesfully joined');
          reject();
        }
      }
    });
  });
  eventPromises.push(txPromise);
});
```

每当接收到区块事件时，代码会将预期的通道名称（在我们的场景中为`tradechannel`）与从区块中提取的通道名称进行匹配。（区块有效载荷是使用 Fabric 源代码中的标准模式构造的，在`protos`文件夹中可用。理解和玩弄这些格式留给读者作为练习。）我们将在代码中设置超时时间（这里为 40 秒），以防止我们的事件订阅逻辑无限等待并阻塞应用程序。最后，通道加入的结果不仅取决于`channel.joinChannel`调用的成功，还取决于区块事件的可用性，如下所示：

```
let sendPromise = channel.joinChannel(request, 40000);
return Promise.all([sendPromise].concat(eventPromises));
```

对于实例化和调用，我们不是为区块注册回调，而是为特定事务注册回调，这些事务由事务建议创建时设置的 ID 标识。订阅代码可以在`instantiate-chaincode.js`和`invoke-chaincode.js`中找到的`instantiateChaincode`和`invokeChaincode`函数中找到。后者的代码片段说明了事务事件处理的基本工作原理：

```
eh.registerTxEvent(deployId.toString(),
  (tx, code) => {
    eh.unregisterTxEvent(deployId);
    if (code !== 'VALID') {
      console.log('The transaction was invalid, code = ' + code);
      reject();
    } else {
      console.log('The transaction has been committed on peer '+ eh.getPeerAddr());
      resolve();
    }
  }
);
```

传递给回调的参数包括事务句柄和状态码，可以检查链码调用结果是否成功提交到分类帐。一旦收到事件，事件监听器就会被注销以释放系统资源（我们的代码也可能会监听区块事件而不是特定的事务事件，但那样就必须解析区块有效载荷并查找和解释提交的事务的信息）。

# 将所有内容整合在一起

之前描述的步骤序列可以通过适当编码的脚本一次性运行。如前所述，`createTradeApp.js`包含这样一个脚本，该脚本导致创建`tradechannel`，将四个对等体加入该通道，将`trade_workflow`链码安装在所有四个对等体上，并在通道上进行实例化，最终以进口商向出口商创建贸易请求并跟踪查询请求状态而结束。您可以运行以下命令，然后在控制台上查看各种步骤的执行：

```
node createTradeApp.js
```

作为一个练习，也是为了测试中间件库函数和链码，您可以通过开始接受出口商的贸易请求，并最终由进口商向出口商支付完整的货物运输费用来完成`createTradeApp.js`脚本开始的贸易场景。要查看这个操作，请运行以下命令：

```
node runTradeScenarioApp.js
```

# 用户应用程序 - 导出服务和 API

为我们的中间件创建一组函数的练习为我们可以在其上构建的面向用户的应用程序奠定了基础。虽然我们可以以不同的方式设计应用程序，但它应该提供的功能集合将保持不变。在演示为区块链用户构建应用程序的方法之前，我们将讨论这样一个应用程序应该具备的显著特征。

# 应用程序

参考*图 5.2：Hyperledger Fabric 应用程序的典型三层架构*，以及本章的 <q>应用模型和架构</q> 部分的讨论，Hyperledger Fabric 应用程序的不同用户可能需要不同且独特的应用程序。我们的交易场景就是一个例子：代表交易方、银行、承运人和政府部门的用户可能需要我们的应用程序中的不同内容，即使他们共同参与交易网络并认可智能合约操作。

不同组织的管理员必须具备执行的共同操作。这包括从创建通道到实例化链码的各个阶段。因此，如果我们需要为每个网络参与者构建不同的应用程序，我们应该向这些应用程序的每个实例提供这些功能。一旦我们进入应用程序本身，它由链码提供的一组调用和查询功能组成，我们必须为区分留出空间。为交易方及其银行设计的应用程序必须向用户公开交易和信用证操作。然而，并不需要将这些操作暴露给承运人，因此，为后者设计的应用程序可以并且应该限制提供给其的功能，仅限于影响承运人角色的功能，例如创建提单和记录货物位置的功能。

在这里，为了简单起见，我们将所有应用程序汇集成一个，并演示如何使其工作。基于用户角色和要求的多样化留给读者作为一项练习。我们的汇编应用程序将作为一个 Web 服务器实现，松散地连接智能合约和中间件，从最终用户那里发出声音。

# 用户和会话管理

任何面向服务的应用程序的设计都需要确定可以访问应用程序并执行各种操作的用户。对于 Hyperledger Fabric 应用程序，应该特别考虑用户类别之间的区别。每个 Fabric 网络都有一组特权用户（我们一直称之为组织的管理员）和普通成员。这种角色的区分必须反映在面向用户的应用程序的设计中。

应用程序必须具有身份验证层以及会话管理机制，允许已经通过身份验证的用户行使应用程序，其权限受其角色的限制。 在我们的示例应用程序中，我们将使用**JSON Web Tokens**（**JWT**）来实现这一目的。

# 设计 API

在构建我们的应用程序之前，我们必须设计一个服务 API，以涵盖我们中间件暴露的功能。 我们将设计我们的 API 为 `RESTful`，如下所示：

1.  `POST/login`: 注册一个新用户（管理或普通用户）或登录为现有用户

1.  `POST/channel/create`: 创建一个通道

1.  `POST/channel/join`: 将网络对等体加入到此用户会话中创建的通道中

1.  `POST/chaincode/install`: 在对等体上安装 `chaincode`

1.  `POST/chaincode/instantiate`: 在通道上实例化 `chaincode`

1.  `POST/chaincode/:fcn`: 调用传递参数的 `chaincode` 函数 `fcn`（在请求体中）； `fcn` 的示例包括 `requestTrade`，`acceptLC` 等

1.  `GET/chaincode/:fcn`: 查询传递参数的 `chaincode` 函数 `fcn`（在请求体中）； `fcn` 的示例包括 `getTradeStatus`，`getLCStatus` 等

这些 API 函数共同覆盖了 *图 5.3：区块链应用程序创建和操作阶段* 中的事务阶段。

# 创建和启动服务

我们将在 Node.js 中实现一个 express（[`expressjs.com/`](https://expressjs.com/)）web 应用程序，以暴露前述 API。 代码位于我们仓库的应用程序文件夹中，源代码在 `app.js` 中，依赖项在 `package.json` 中定义。 作为运行 web 应用程序的先决条件，必须安装依赖项，可以通过运行 `npm install` 或 `make`（参见*Chapter 8*，*区块链网络中的灵活性*) 在该文件夹中。

以下代码片段显示了如何实例化和运行 *express* 服务器：

```
var express = require('express');
var bodyParser = require('body-parser');
var app = express();
var port = process.env.PORT || 4000;
app.options('*', cors());
app.use(cors());
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({
  extended: false
}));
var server = http.createServer(app).listen(port, function() {});
```

总结一下，启动一个 web 服务器以在端口 `4000` 上监听 HTTP 请求。 配置中间件以启用 CORS，自动解析 JSON 负载并在 POST 请求中形成数据。

我们的 web 服务器通过不安全的 HTTP 监听请求。 在生产应用中，我们将启动一个 HTTPS 服务器，以与客户端进行安全、机密的通信。

现在，让我们看看如何配置各种 express 路由来实现我们的服务 API 功能。

# 用户和会话管理

在执行网络（通道）或链码操作之前，用户必须建立经过身份验证的会话。 我们将实现 `/login` API 函数如下：

1.  为用户创建一个具有 60 秒过期时间的 JWT 令牌

1.  注册或登录用户

1.  如果成功，将令牌返回给客户端

服务器期望提供请求体中的表单数据，以提供注册或登录的用户名和组织名。 管理用户仅由管理员用户名标识。 请求体格式为：

```
username=<username>&orgName=<orgname>[&password=<password>]
```

必须提供密码，但只有当 `<username>` 是 `admin` 时才需要。在这种情况下，中间件将简单地检查提供的密码是否与用于启动组织 MSP 的 `fabric-ca-server` 的密码匹配。正如本章前面提到的，我们的 MSP 管理员密码设置为默认的 `adminpw`。

这是一个天真的实现，但是由于 Web 应用安全不是本教程的重点，这足以展示如何在智能合约和中间件上实现服务器和前端。

JWT 令牌创建和用户注册/登录的代码可以在以下 express 路由中找到，在 `app.js` 中配置：

```
app.post('/login', async function(req, res) { ... });
```

读者可以尝试使用会话管理的其他机制，例如会话 cookie，而不是 JWT 令牌。

我们的 Web 应用现在可以进行测试了。首先，使用 `docker-compose`（或 `trade.sh`）启动 Fabric 网络，就像本章前面所示的那样。

如果您使用 `cryptogen`（或 `trade.sh` 脚本）为组织创建了新的加密密钥和证书，则必须清除中间件用于保存世界状态和用户信息的临时文件夹，否则如果尝试注册在应用程序的上一次运行中使用过的 ID，则可能会看到错误。例如：如果临时文件夹在您的机器上是 `network/client-certs`，您只需从 `network` 文件夹运行 `rm -rf client-certs` 来清除内容。

在不同的终端窗口中，通过运行以下命令启动 Web 应用程序：

```
node app.js
```

在第三个终端窗口中，使用 `curl` 命令向 Web 服务器发送请求，以创建名为 `Jim` 的普通用户在 `importerorg` 组织中（这是在 `middleware/config.json` 中指定的组织名称）：

```
curl -s -X POST http://localhost:4000/login -H "content-type: application/x-www-form-urlencoded" -d 'username=Jim&orgName=importerorg'
```

你应该看到类似以下的输出：

```
{"token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE1MjUwMDU4NTQsInVzZXJuYW1lIjoiSmltIiwib3JnTmFtZSI6ImltcG9ydGVyb3JnIiwiaWF0IjoxNTI1MDAxNzE0fQ.yDX1PyKnpQAFC0mbo1uT1Vxgig0gXN9WNCwgp-1vj2g","success":true,"secret":"LNHaVEXHuwUf","message":"Registration successful"}
```

在中间件中，在这里执行的函数是 `clientUtils.js` 中的 `getUserMember`，这在本章前面已经讨论过。

要在同一组织中创建一个管理用户，请运行：

```
curl -s -X POST http://localhost:4000/login -H "content-type: application/x-www-form-urlencoded" -d 'username=admin&orgName=importerorg&password=adminpw'
```

你应该看到以下输出（管理员用户已经注册，所以这实际上是一个登录调用）：

```
{"token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE1MjUwMDU4OTEsInVzZXJuYW1lIjoiYWRtaW4iLCJvcmdOYW1lIjoiaW1wb3J0ZXJvcmciLCJpYXQiOjE1MjUwMDE3NTF9.BYIEBO_MZzQa52_LW2AKVhLVag9OpSiZsI3cYHI9_oA","success":true,"message":"Login successful"}
```

在中间件中，在这里执行的函数是 `clientUtils.js` 中的 `getMember`。

# 网络管理

正如你在 `app.js` 中所看到的，从通道创建到链码实例化的 API 函数都被实现为 express 路由：

```
app.post('/channel/create', async function(req, res) { ... });
app.post('/channel/join', async function(req, res) { ... });
app.post('/chaincode/install', async function(req, res) { ... });
app.post('/chaincode/instantiate', async function(req, res) { ... });
```

要使用这些路由，最终用户必须以管理员身份登录并使用返回的令牌。根据上一次调用的输出，我们可以如下请求通道创建：

```
curl -s -X POST http://localhost:4000/channel/create -H "authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE1MjUwMDU4OTEsInVzZXJuYW1lIjoiYWRtaW4iLCJvcmdOYW1lIjoiaW1wb3J0ZXJvcmciLCJpYXQiOjE1MjUwMDE3NTF9.BYIEBO_MZzQa52_LW2AKVhLVag9OpSiZsI3cYHI9_oA"
```

注意，授权头的格式是 `Bearer <JWT 令牌值>`。Web 服务器隐式假定通道名称为 `tradechannel`，这在 `middleware/constants.js` 中设置。（如果愿意，您可以扩展服务器 API 以接受请求体中的通道名称。）如果一切顺利，输出应该如下：

```
{"success":true,"message":"Channel created"}
```

管理员可以运行类似的查询以加入通道、安装链码和实例化链码。例如，实例化 API 端点预期的链码路径、链码版本和链码参数列表如下：

```
curl -s -X POST http://localhost:4000/chaincode/instantiate -H "authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE1MjUwMDU4OTEsInVzZXJuYW1lIjoiYWRtaW4iLCJvcmdOYW1lIjoiaW1wb3J0ZXJvcmciLCJpYXQiOjE1MjUwMDE3NTF9.BYIEBO_MZzQa52_LW2AKVhLVag9OpSiZsI3cYHI9_oA" -H "content-type: application/json" -d '{ "ccpath": "github.com/trade_workflow", "ccversion": "v0", "args": ["LumberInc", "LumberBank", "100000", "WoodenToys", "ToyBank", "200000", "UniversalFreight", "ForestryDepartment"] }'
```

如果一切顺利，输出将是：

```
{"success":true,"message":"Chaincode instantiated"}
```

在每个路由的实现中，都会检查用户（由 JWT 令牌标识）是否是管理员用户，如下所示：

```
if (req.username !== 'admin') {
  res.statusCode = 403;
  res.send('Not an admin user: ' + req.username);
  return;
}
```

如果我们要使用 Jim 注册的用户令牌，Web 服务器将向客户端返回`403`错误代码。

# 运行应用程序

一旦由管理员用户初始化了链码，我们的应用程序就开放了。现在，任何普通用户（例如进口商组织中的 Jim）都可以请求链码调用或查询。例如，可以发出交易请求如下：

```
curl -s -X POST http://localhost:4000/chaincode/requestTrade -H "authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE1MjUwMDU4NTQsInVzZXJuYW1lIjoiSmltIiwib3JnTmFtZSI6ImltcG9ydGVyb3JnIiwiaWF0IjoxNTI1MDAxNzE0fQ.yDX1PyKnpQAFC0mbo1uT1Vxgig0gXN9WNCwgp-1vj2g" -H "content-type: application/json" -d '{ "ccversion": "v0", "args": ["2ks89j9", "50000","Wood for Toys"] }'
```

注意，请求体中必须提供链码版本。如果一切顺利，输出将是：

```
{"success":true,"message":"Chaincode invoked"}
```

随后，可以查询交易的状态（再次由`Jim`）：

```
curl -s -X GET http://localhost:4000/chaincode/getTradeStatus -H "authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE1MjUwMDU4NTQsInVzZXJuYW1lIjoiSmltIiwib3JnTmFtZSI6ImltcG9ydGVyb3JnIiwiaWF0IjoxNTI1MDAxNzE0fQ.yDX1PyKnpQAFC0mbo1uT1Vxgig0gXN9WNCwgp-1vj2g" -H "content-type: application/json" -d '{ "ccversion": "v0", "args": ["2ks89j9"] }'
```

现在，输出应包含链码响应：

```
{"success":true,"message":"{\"Status\":\"REQUESTED\"}"}
```

# 用户/客户端交互模式

虽然运行 curl 命令足以测试我们的 Web 应用程序，但向用户公开应用程序的适当方式是通过一个或多个网页，其中包含用户触发这些命令的小部件。

正如我们在中间件实现部分看到的，包括链码调用在内的各种操作都是异步的。在我们的实现中，我们通过使包装函数返回给调用者来遮蔽了这种异步行为，但仅当请求已成功发送到订购者并且已收到并验证了已订阅的事件时才返回。我们还可以选择将此异步行为暴露给 Web 应用程序客户端。使用 Web Sockets ([`developer.mozilla.org/en-US/docs/Web/API/WebSockets_API`](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API))，当事件通知到达与事件中心注册的回调时，向最终用户呈现的 Web 界面内容可以动态更新。

设计良好的 Web 界面超出了本书的范围，读者可以利用其他知识源来构建适合其应用程序的界面。

# 测试中间件和应用程序

我们已经展示了如何使用示例脚本和`curl`命令来执行基于 Node JS 的中间件和应用程序功能。通过观察控制台输出，您可以了解应用程序是否按预期工作。对于生产应用程序，您将需要一种更健壮和可维护的测试方法，以持续评估库函数和 API 端点的正确性。单元测试和集成测试都应该是您评估过程的一部分。这样的测试的实际演示超出了本章的范围，编写单元测试和集成测试留给读者练习。Mocha 是一个功能丰富的 JavaScript 框架，用于异步测试([`mochajs.org/`](https://mochajs.org/))，可以用于此目的。

# 与现有系统和流程的集成

在与客户讨论端到端解决方案时，我们经常解释说，与整体足迹相比，与区块链相关的组件所占比例很小。这仍然是一组非常重要的组件，但它们代表的足迹很小。

本节将重点介绍我们传统系统与 Hyperledger Fabric 和 Composer API 之间的接触点。

我们将探讨我们已经利用的各种集成模式，并查看一些非功能性要求如何影响集成部署。最后，我们将探讨一些在设计集成层时集成者需要牢记的其他考虑因素。

简而言之，在本节中，您将：

+   了解集成层的设计考虑

+   查看集成设计模式

+   探索非功能性要求对集成的影响

# 设计考虑

到目前为止，您已经有了使用 Fabric SDK 的经验，到*第七章*结束时，*一个商业网络示例*，您将体验使用`Composer REST`网关。虽然这些确实是集成的主要工具，但它们是生态系统的一部分，并且企业的业务流程需要进行调整，以确保集成是有意义的。

根据设计考虑，我们将考虑以下几个方面：

+   去中心化的影响

+   流程调整

+   消息关联

+   服务发现

+   身份映射

# 去中心化

曾经有很多尝试标准化 IT 功能和能力，但现实是没有两个组织拥有相同的 IT 景观。即使对于那些选择了相同的 ERP 供应商的组织来说，系统也会被定制以满足组织的流程和需求。

这意味着在规划集成设计时，您应该记住每个组织可能都有自己调用智能合约的方式，可能没有相同的 IT 能力或政策。

例如，通过 Web Socket 公开事件可能对熟悉基于云的技术的组织有意义，但其他组织可能没有这方面的技能，或者其 IT 安全策略可能不允许他们使用该协议。

尽管对一些人来说可能令人惊讶，但请记住，一个网络可以是财富 500 强组织和初创企业的混合体。想象一下供应链行业；您会发现一些几乎没有 IT 基础设施的卡车公司，一直到行业巨头。显然，一刀切可能不适用于所有情况。

话虽如此，从网络的角度来看，您应考虑网络希望为加入组织提供的支持程度。有两种可能的方法：

+   **网络提供集成资产**：这可以采用每个参与者在其自己的基础设施中部署的网关形式。网关对每个人都是标准的，并以一致的方式管理智能合约的调用。

    这可以提供加速入职流程的好处，但需要考虑谁拥有、管理和支持这个 IT 组件。此外，由于信任问题，一些组织可能不希望部署此基础设施。

+   **每个参与者构建自己的集成层**：这种方法的明显缺点是所有参与者都要重新发明轮子，但它减少了通过在每个组织中部署一个公共组件所创建的潜在支持问题。

    对于需要深度系统集成以实现流程优化效益的用例，这也可能是首选方法。

# 流程对齐

集成层将不得不处理两种不同的观点：

+   **组织 IT 系统和业务流程视角**：组织业务流程可能托管在诸如 SAP 等 ERP 中。在这种情况下，当特定的业务事件需要调用智能合约时，可能会通过 SAP 系统的 **业务 API**（**BAPI**）调用来发出。来自 ERP 的 API 调用可能包含各种数据结构，其中一些与区块链网络完全无关。

+   **智能合约视角**：这个视角具有数据表示是应用程序不可知的特点。这意味着网络的所有参与者都将理解正在处理的数据的性质。

它取决于集成层来协调两者，并确保在两个系统中保持正确的交易语义。这可能意味着：

+   **映射**：将数据从一个字段名移动到另一个字段名

+   **转换**：基于输入聚合、拆分或计算新值

+   **交叉引用**：利用参考表将应用程序特定代码映射到网络识别的值

这里的观点是，即使你的网络同意使用第七章中介绍的 Hyperledger Composer REST 网关，《业务网络示例》，每个参与者仍然需要做一些工作，以确保集成适应组织的整体业务流程。

# 消息亲和性

虽然这通常不是一个讨论的问题，但忽视它可能会导致严重的问题，通常会在集成或性能测试过程中出现。

当系统发出一系列相互依赖的交易，并且这些交易在短时间内发出时，我们将其称为消息亲和性。因为每个交易是分开发出的，所以它们可能以与客户端发出时不同的顺序进行处理。

结果可能是不可预测的，就像下面的例子所示。为了更具体，让我们看一个将发出三个单独事务的订单过程，如下图所示：

![](img/05a9b3bf-eb3a-4aec-94ff-da5d84218061.png)

图 5.4：按顺序处理服务请求

由于服务提供者是多线程的，所以处理顺序可能因当时的负载而有所变化。一个潜在的结果如下图所示：

![](img/0c721e3c-759b-45cc-b086-fc5fdba9fa3b.png)

图 5.5：潜在的服务处理结果

如果第一个正在处理的项目顺序错误将被拒绝，因为订单对象还没有被创建。然而，后续的两个对象将成功，并且将系统留在订单记录为单个项目而不是两个项目的状态。

面临的挑战是很难进行故障排除。一个不知情的开发人员可能无法在他/她的开发平台上重现这种行为。

现在，你可能会想，这与区块链和 Hyperledger Fabric 有什么关系？考虑到 Fabric 交易是异步处理的，并且它们根据每个世界状态进行验证，这种情况可能会发生。客户端将发出事务，并且可能会异步收到一条消息，说事务无效，因为它与世界状态不相符。

这里的故事教训是，在设计 API 时，确保它们在完全描述业务事件的粒度级别上。过多的细粒度交易只会导致消息亲和性、增加的延迟和可能出现的问题，就像这里描述的那样。

# 服务发现

在集成到 Hyperledger Fabric 和 Composer 的上下文中，服务发现的概念着重于记录和向调用应用程序暴露来自 Fabric 的工件：CA、对等体和排序器。

正如我们现在所经历的那样，为了让应用程序获得认可的交易并添加到分类账中，需要与这些类型的众多组件进行交互。有一种方法将此信息作为服务配置元素维护将使团队能够快速适应网络的不断发展。

目前，在使用 Fabric SDK 开发客户端应用程序时，开发人员负责管理和应用此服务配置。

Hyperledger Fabric 路线图的一部分是促进这种配置。

依赖于`Composer REST`网关等组件的一个好处是网关提供了服务发现。具体来说，正如您很快会发现的，它提供了一个包含身份信息和连接配置文件的商业卡，其中列出了可用于执行交易的 Hyperledger Fabric 服务。

# 身份映射

身份映射是将个人或组织的身份转换为网络上可识别的身份的过程。

从业务网络视角看待解决方案时，需要识别的身份的粒度是多少？其他组织是否会在乎 ACME 的 Bob 或 Ann 发起了交易？在大多数情况下，答案会是否定的。知道交易是由 ACME 发起的将是足够的。

你可能会想为什么。这直接与信任的概念有关。还记得*第一章*中介绍的概念，*区块链-企业和行业视角*；区块链解决了时间和信任的问题。了解信任问题的根源有助于我们理性地确定应该用于在网络上交易的身份。在大多数情况下，我们的经验是信任问题发生在组织之间。

如果想象一个银行客户通过其银行门户进行交易的用例，客户不会关心后端系统；他们信任他们银行的安全系统。

虽然如此，有些情况下需要进行身份映射：

+   通过组织的集成层进行交易的商业合作伙伴

+   具有不同权限级别的不同部门

+   具有不同角色的用户具有不同的访问权限

在这种情况下，集成层将需要将传入的凭据（API 密钥，用户 ID 和密码，JTW 令牌等）转换为 Hyperledger Fabric 身份。

在使用 Hyperledger `Composer REST`网关时，可以配置它来支持多个用户。服务器利用节点密码框架来管理此身份验证。这提供了支持不同模型（例如用户 ID/密码，JWT 和 OAUTH）的灵活性。

客户端一经验证通过服务器，还有一个额外的步骤，即将 Hyperledger Composer 商业卡加载到服务器的用户存储库中。客户端与服务器之间需要有隐式信任，因为商业卡包含私钥。

# 集成设计模式

现在我们将看一些我们在行业中见过的可行集成模式。这个列表并不是详尽无遗的，考虑到我们仍处于 Hyperledger Fabric 和 Composer 解决方案的早期阶段，我们预计随着人们和组织对技术变得更加熟悉，将会出现新的模式。

# 企业系统集成

在这个类别中，我们考虑任何组织在加入网络之前存在的现有系统。因此，这些系统有着自己的概念和范式，我们将需要一种形式的抽象来协调这两个世界。

# 与现有记录系统集成

以下是一张图来说明将区块链网络集成到现有记录系统中的过程：

![](img/ace45f87-b12c-4ea7-9b2d-0011fd97732d.png)

图 5.6：将区块链网络集成到现有记录系统中

大多数寻求加入业务网络的大型企业最终将目标定在集成其记录系统，以确保他们从实时透明的交易分配中受益。在这种情况下，我们之前提到的流程对齐将非常重要。

如前图所示，该方法将利用适配器模式来充当两个世界之间的数据映射器。适配器将采用企业系统应用程序协议和数据结构来接收交易请求。可选地，它还可以利用现有基础设施，如消息服务，来传播分类帐事件。

这里需要注意的重要事项是，这种类型的集成将针对一个组织，很少有可能重用。

作为此模式的一种变体，一些组织将适配器分解为两部分：

+   **REST 网关**：暴露与 Fabric 智能合约对齐的 REST 接口

+   **集成总线**：映射字段并连接企业系统

虽然在这种变体中，重用率更高，但相同的考虑只是向下移动了一层。

# 与运营数据存储集成

这里有一张图，展示了将区块链网络集成到运营数据存储中的过程：

![](img/b7b1f220-6a7b-4734-9cf6-bd325cf463d7.png)

图 5.7：将区块链网络集成到运营数据存储中

通常，组织正在寻找方法对其分类帐中的信息进行分析。然而，对组织的对等方发出多个/大型查询将仅影响系统的在线性能。通常，在企业系统设计中公认的方法是将数据移动到运行数据存储中。然后可以轻松查询数据。可以通过使用不同的数据来源使数据进行丰富处理，从而创建数据的附加视图。

在这种模式下，事件监听器订阅了 Fabric 组织的事件。因此，它可以接收组织有权访问的所有通道的交易。如果数据的完整性保护很重要，事件监听器可以计算每条记录的哈希值，并将其与记录一起存储。

你会注意到该模式还考虑了一个名为`syncAll`的函数，它将允许事件监听器与世界状态的最新视图重新同步数据存储。请记住，这个`syncAll`函数的实现需要谨慎进行，并且很可能需要支持结果集的分页。

# 微服务和事件驱动架构

下图说明了区块链应用的微服务和事件驱动架构：

![](img/93b7c097-cabb-4d7f-9633-abcff03632ab.png)

图 5.8：区块链应用的微服务和事件驱动架构

我们将此模式标记为微服务和事件驱动，因为这是这些类型架构中最常见的模式。然而，该模式的特殊之处来自于网关。这样的系统不会执行任何数据映射；它将利用共同的通信协议（HTTP）和数据格式（通常为 JSON，但也可以是 XML）。此外，服务已经设计为理解所进行交易数据的语义也是一个期望。事件也通过相同的协议和数据格式传播。

再次强调，微服务应用往往是较新的应用程序，并且会从更细粒度的接口受益。因此，它们 tend to evolve more quickly and be in a position to adapt and adhere to the transactions from the network. Similarly, event-driven applications will benefit from their low coupling to the other components of the system, and so are good candidates for this pattern.

# 考虑可靠性、可用性和可维护性

软件或硬件组件的故障对于任何工业应用来说都是不可避免的事实，因此您必须设计您的应用程序能够抵御故障并最大程度地减少停机的可能性。我们将讨论业界广泛使用的三个关键指导方针，以构建和维护系统，并简要审视它们如何适用于使用 Fabric 或 Composer 工具构建的应用程序。

# 可靠性

可靠的系统是在面对故障时确保正确运行的系统，概率很高。这涉及以下几点：

+   系统的持续自我监控

+   组件中故障或损坏的检测

+   修复问题和/或切换到正常工作的组件

虽然行业中已经发展出了各种实践以确保可靠性，但冗余和故障转移通常（甚至普遍）被使用。

在我们在第一部分构建的 Fabric 应用程序的背景下，这具有某些含义。请回想一下，Fabric 有许多不同的组件，它们必须协同工作（虽然是以松散耦合的方式）以确保成功运行。排序服务是其中一个关键组件，如果它失败了，将会完全停滞事务流水线。因此，当构建生产版本的比如说我们的交易应用程序时，您必须确保排序器具有足够的冗余性。在实践中，如果您的排序器是一个 Kafka 集群，这意味着必须确保有足够的 Kafka 节点（broker）来接管应付一个或多个失败的情况。

同样，对于背书和承诺的对等体的可靠性对于确保交易完整性至关重要。虽然区块链作为共享复制账本被设计为在对等体故障时具有一定的健壮性，但其易受攻击的程度可能因应用程序而异。如果一个背书对等体失败，并且其签名是满足交易背书政策所必需的，那么就无法创建交易请求。如果一个背书对等体行为不端，并且产生了错误的执行结果，那么交易将无法提交。在任何一种情况下，系统的吞吐量都会降低或降为零。为了防止这种情况发生，您应确保在每个组织内建有足够的冗余，特别是对于满足背书政策的关键对等体。以下图示了一种可能的机制，通过该机制，交易提案将被提供给多个对等体，并且使用多数规则丢弃缺席或错误的响应：

![](img/48ae0c90-7c06-4f14-aa18-4ca441fb8a2e.png)

图 5.9 可靠交易背书的冗余对等体

从系统获得的可靠性水平取决于投入到监控和故障转移的资源量。例如，前述图中的五个对等体足以应对两个对等体的失败，但现在这需要比我们在示例网络中使用的对等体多四个对等体。为了确定并确保您的网络产生预期的可靠性水平，您需要在一段时间内对您的完整系统进行集成测试。

# 可用性

可用性标准与可靠性密切相关，但更多关乎确保系统的高概率运行时间，或作为推论，最小化系统停机的概率。与可靠性一样，检测故障节点并确保充足的故障转移是确保您的应用仍将正常运行的关键。确定所需的可用性水平，分配足够数量的冗余和自我纠正组件资源，并在生产环境中进行测试，是确保您的应用能够获得期望性能的必要条件。

# 可维护性

**可维护性**或**易维护性**是指您可以在不影响整个系统的情况下更换或升级系统部件的便捷程度。

考虑这样一种情况：您必须在一个或多个订购服务节点上升级操作系统，或者需要在一个组织内替换一个有故障的节点。与可靠性或可用性一样，拥有冗余（或并行）资源，可以让应用操作无缝切换至这些资源，是处理工业规模系统中这类情况的方法。所谓的蓝绿部署是用于这个目的的一种流行机制之一。简而言之，您有两个平行环境（比如，对于订购服务），一个称为蓝色，一个称为绿色，其中蓝色环境正在接收实时流量。您可以升级绿色机器上的操作系统，充分测试它们，然后切换流量从蓝色到绿色。此时，绿色正在提供服务，您可以以同样的方式升级蓝色。

在具有松散耦合组件的区块链应用中，建议为每个组件（订购者、节点和 MSP）都提供蓝色和绿色环境，并分阶段进行升级和测试，或者一次只升级一个组件集群，以减小出错的机会。

# 摘要

构建一个完整的区块链应用是一个雄心勃勃且具有挑战性的项目，不仅因为它需要各种技能——系统、网络、安全和 web 应用开发等，而且因为它需要跨越多个安全域的多个组织共同进行开发、测试和部署。

在本章中，我们从一个简单的智能合约开始，并以一个准备好驱动贸易场景并在一个防篡改、共享、复制账本中存储记录的四节点区块链网络结束。在此过程中，我们学会了如何设计组织结构并配置 Fabric 网络。我们学会了如何构建一个通道，或者说一个 Fabric 区块链的实例，让网络中的节点加入该通道，并在该通道上安装和实例化一个智能合约，使用 Fabric SDK。我们学会了如何通过 Web 应用程序向最终用户公开我们网络和智能合约的功能，公开服务 API。我们还学会了超级账本 Fabric 交易流水线的工作原理，以及区块提交操作的异步性质如何影响端到端应用程序的实现。

在本章的后半部分，我们学习了可以用来构建行业规模的区块链应用程序的各种设计模式和最佳实践。我们还了解了在将这些应用程序与现有系统和流程集成时需要考虑的因素。最后，我们探讨了运行操作性 Fabric 网络的性能方面，并学习了 CAP 定理以及 Fabric 如何在分布式环境中实现数据一致性。

超级账本平台和工具毫无疑问会随着时间的推移而发展，以满足行业和开发者的需求，但我们在应用构建过程中描述的架构和方法论，以及设计和集成模式，应该会长期作为教育指南。

我们迄今为止的旅程已经带领我们到了超级账本 Fabric 框架的基础。我们已经使用链码并使用 Fabric SDK API 集成了一个应用程序。这些都是必不可少的技能。

在接下来的两章中，我们将探讨一种不同的建模和实现业务网络的方法。
