# 前言

区块链技术被誉为当今最具革命性和颠覆性的创新之一。区块链技术最初是在世界上最流行的数字货币比特币中被识别出来的，但现在已经改变了许多组织的看法，并赋予他们甚至将其用于价值存储和转移的能力。

本书将首先向您介绍常见的网络威胁景观和常见攻击，如恶意软件、钓鱼、内部威胁和 DDoS。接下来的章节将帮助您了解区块链技术、以太坊和 Hyperledger 架构的运作方式，以及它们如何适应网络安全生态系统。这些章节还将帮助您在以太坊区块链和 Hyperledger Fabric 框架上编写您的第一个分布式应用程序。稍后，您将了解安全三重性及其与区块链的适应性。最后一组章节将带您了解网络安全的核心概念，如 DDoS 保护、基于 PKI 的身份验证、双因素认证和 DNS 安全。您将了解区块链在从根本上转变网络安全解决方案方面起到了至关重要的作用。

在书的最后，您将了解区块链在安全案例中的真实部署示例，并了解短期挑战和区块链与网络安全未来的发展。

# 本书适合谁

本书面向网络安全专业人士，或任何与网络安全相关的利益相关者，他们希望了解如何使用区块链来提升基础设施的安全性的下一层次。对区块链的基本理解可能是一个额外的优势。

# 要充分利用本书

硬件要求如下：

+   Ubuntu 16.04

软件要求如下：

+   Linux

+   Node.js

+   Truffle

+   Ganache-CLI

# 下载彩色图片

我们还提供了一个 PDF 文件，其中包含本书中使用的截图/图表的彩色图像。您可以从[`www.packtpub.com/sites/default/files/downloads/HandsOnCybersecuritywithBlockchain_ColorImages.pdf`](https://www.packtpub.com/sites/default/files/downloads/HandsOnCybersecuritywithBlockchain_ColorImages.pdf)下载。

# 使用惯例

在本书中使用了许多文本约定。

`CodeInText`：指示文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 句柄。这是一个例子：“这个文件夹包括我们的智能合约，`TwoFactorAuth.sol`。”

代码块设置如下：

```
forward-zones=bit.=127.0.0.1:5333,dns.=127.0.0.1:5333,eth.=127.0.0.1:5333,p2p.=127.0.0.1:5333
export-etc-hosts=off
allow-from=0.0.0.0/0
local-address=0.0.0.0
local-port=53
```

当我们希望引起您对代码块的特定部分的注意时，相关的行或项目将以粗体显示：

```
$ node registerAdmin.js 
//File Structure Tuna-app/tuna-chaincode.go 
```

任何命令行输入或输出都以以下形式编写：

```
sudo apt-get update
sudo apt-get install git npm
sudo apt-get install nodejs-legacy
```

**粗体**：表示新术语、重要单词或屏幕上看到的单词。例如，菜单中的单词或对话框中的单词会在文本中显示为这样。这是一个例子：“我们需要将环境字段设置为 Web3 Provider 选项。”

警告或重要说明会以这种方式显示。

实用提示和技巧如下所示。
