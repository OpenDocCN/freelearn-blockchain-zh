# 序言

区块链是一个去中心化的账本，它维护着一个持续增长的数据记录列表，受到篡改和修订的保护。每个用户都可以连接到网络，向其发送新交易，验证交易，并创建新的区块。

本书将教会您什么是区块链，它如何维护数据完整性，以及如何使用以太坊创建真实世界的区块链项目。通过有趣的真实世界项目，您将学会如何编写智能合约，这些合约完全按照程序编写，没有欺诈、审查或第三方干预的机会，并构建端到端的区块链应用程序。您将学习加密货币中的密码学概念、以太安全、挖矿、智能合约和 Solidity 等概念。

区块链是比特币的主要技术创新，它作为比特币交易的公共账本。

# 本书涵盖的内容

[第 1 章](97a50ded-ba94-43ee-93c2-ce781e8a98a7.xhtml)，*理解去中心化应用*，将解释什么是 DApps，并概述它们的工作原理。

[第 2 章](fd10eeec-3c1a-494b-9628-acc065d69b92.xhtml)，*理解以太坊的工作原理*，解释了以太坊的工作原理。

[第 3 章](13cd4015-d978-4d9c-b99f-a68aa76d4437.xhtml)，*编写智能合约*，展示了如何编写智能合约，以及如何使用 geth 的交互式控制台使用 web3.js 部署和广播交易。

[第 4 章](878a3321-e436-4110-a109-c4dea94642f2.xhtml)，*使用 web3.js 入门*，介绍了 web3js 及如何导入、连接 geth，并解释如何在 Node.js 或客户端 JavaScript 中使用它。

[第 5 章](93ac152a-df95-4449-a109-d965d9ba319f.xhtml)，*构建钱包服务*，解释了如何构建一个钱包服务，用户可以轻松创建和管理以太坊钱包，甚至是离线的。我们将专门使用 LightWallet 库来实现这一点。

[第 6 章](97d4a675-402b-4ca3-86fe-aacd748d73e3.xhtml)，*构建智能合约部署平台*，展示了如何使用 web3.js 编译智能合约，并使用 web3.js 和 EthereumJS 部署它。

[第 7 章](27bfa4df-5d7a-47e0-b93f-cbb9418b1fba.xhtml)，*构建一个投注应用*，解释了如何使用 Oraclize 从以太坊智能合约发出 HTTP 请求，以访问来自万维网的数据。我们还将学习如何访问存储在 IPFS 中的文件，使用字符串库处理字符串等等。

[第 8 章](faf4c014-9863-4e43-9631-9536741c81a5.xhtml)，*构建企业级智能合约*，解释了如何使用 Truffle 来轻松构建企业级 DApps。我们将通过构建一种替代货币来学习 Truffle。

[第 9 章](c3d7a01e-0aec-4dfb-9e95-8231111bcf71.xhtml)，*构建联盟链*，我们将讨论联盟链。

# 本书所需内容

您需要 Windows 7 SP1+、8、10 或 Mac OS X 10.8+。

# 本书适合对象

本书适用于现在想要使用区块链和以太坊创建防篡改数据（和交易）应用程序的 JavaScript 开发人员。对加密货币、支撑其逻辑和数据库的人将发现本书非常有用。

# 约定

在本书中，您将找到许多文本样式，用以区分不同类型的信息。以下是一些这些样式的示例以及它们的含义解释。

文本中的代码词，数据库表名，文件夹名，文件名，文件扩展名，路径名，虚拟 URL，用户输入和 Twitter 句柄显示如下：“然后，在 `Final` 目录中使用 `node app.js` 命令运行应用。”

代码块设置如下：

```
var solc = require("solc"); 
var input = "contract x { function g() {} }"; 
var output = solc.compile(input, 1); // 1 activates the optimizer  
for (var contractName in output.contracts) { 
    // logging code and ABI  
    console.log(contractName + ": " + output.contracts[contractName].bytecode); 
    console.log(contractName + "; " + JSON.parse(output.contracts[contractName].interface)); 
}

```

任何命令行输入或输出均如下所示：

```
    npm install -g solc

```

**新术语** 和 **重要词汇** 以粗体显示。例如，屏幕上看到的词汇，例如菜单或对话框中的词汇，显示在文本中，如下所示：“现在再次选择同一文件，然后单击“获取信息”按钮。”

警告或重要提示会以此框的形式出现。

提示和技巧会以这种方式出现。

# 读者反馈

我们读者的反馈意见一直受欢迎。请告诉我们您对本书的看法——您喜欢或不喜欢的地方。读者的反馈对我们很重要，因为它帮助我们开发出您真正能够充分利用的标题。

要向我们发送一般反馈意见，只需简单发送电子邮件至 `feedback@packtpub.com`，并在消息主题中提及书名。

如果您对某个主题具有专业知识，并且有兴趣编写或为书籍做出贡献，请查看我们的作者指南，网址为 [www.packtpub.com/authors](http://www.packtpub.com/authors)。

# 客户支持

现在您是 Packt 书籍的自豪所有者，我们有很多事情可以帮助您充分利用您的购买。

# 下载示例代码

您可以从您的帐户在 [http://www.packtpub.com](http://www.packtpub.com) 上为本书下载示例代码文件。如果您在其他地方购买了本书，您可以访问 [http://www.packtpub.com/support](http://www.packtpub.com/support) 并注册，文件将直接发送到您的邮箱。

您可以按照以下步骤下载代码文件：

1.  使用您的电子邮件地址和密码登录或注册到我们的网站。

1.  将鼠标指针悬停在顶部的 **SUPPORT** 标签上。

1.  单击“代码下载和勘误”。

1.  在搜索框中输入书名。

1.  选择要下载代码文件的书籍。

1.  从下拉菜单中选择您从何处购买了本书。

1.  单击“代码下载”。

下载文件后，请确保使用最新版本的解压缩或提取文件夹：

+   Windows 的 WinRAR / 7-Zip

+   Mac 的 Zipeg / iZip / UnRarX

+   Linux 的 7-Zip / PeaZip

本书的代码包也托管在 GitHub 上，链接为 [https://github.com/PacktPublishing/Building-Blockchain-Projects](https://github.com/PacktPublishing/Building-Blockchain-Projects)。我们还提供了来自我们丰富书籍和视频目录的其他代码包，可以在 [https://github.com/PacktPublishing/](https://github.com/PacktPublishing/) 查看！请查看！

# 下载本书的彩色图像

我们还为您提供了一个 PDF 文件，其中包含本书中使用的屏幕截图/图表的彩色图像。彩色图像将帮助您更好地理解输出中的变化。您可以从 [https://www.packtpub.com/sites/default/files/downloads/BuildingBlockchainProjects_ColorImages.pdf](https://www.packtpub.com/sites/default/files/downloads/BuildingBlockchainProjects_ColorImages.pdf) 下载此文件。

# 勘误

尽管我们已经尽一切努力确保内容的准确性，但错误确实会发生。如果您在我们的书籍中发现错误——可能是文字或代码方面的错误——我们将不胜感激地向您报告。通过这样做，您可以避免其他读者的挫折，并帮助我们改进本书的后续版本。如果您发现任何勘误，请访问 [http://www.packtpub.com/submit-errata](http://www.packtpub.com/submit-errata)，选择您的书籍，点击“勘误提交表格”链接，然后输入勘误的详细信息。一旦您的勘误经过验证，您的提交将被接受，并且勘误将被上传到我们的网站或添加到该书籍的勘误部分的任何现有勘误列表中。

要查看先前提交的勘误，请访问 [https://www.packtpub.com/books/content/support](https://www.packtpub.com/books/content/support)，然后在搜索框中输入书名。所需信息将显示在勘误部分下。

# 盗版

互联网上侵犯版权的行为一直是所有媒体的持续问题。在 Packt，我们非常重视版权和许可证的保护。如果您在互联网上发现我们作品的任何形式的非法副本，请立即向我们提供位置地址或网站名称，以便我们采取补救措施。

请通过链接至可疑盗版材料的方式联系我们，邮箱地址为`copyright@packtpub.com`。

我们感谢您在保护我们的作者和我们为您提供有价值内容的能力方面的帮助。

# 问题

如果您对本书的任何方面有问题，可以通过邮件联系我们，邮箱地址为`questions@packtpub.com`，我们将尽力解决问题。
