# 序言

在过去的十年中，区块链及其相关技术已被用于增加透明度并去除涉及到关键流程中不必要的中间方。类似地，人工智能已被采用来优化过程并以准确且廉价的方式预测结果。人工智能和区块链的结合正在加速企业创新的步伐。预计这两种技术的融合将彻底改变我们今天所知的数字领域的某些方面。本书旨在帮助您理解区块链和人工智能的基本概念，分析它们的用例，并在诸如医疗保健、金融、贸易和供应链管理等各个行业中实施这些技术。本书还指导您使用以太坊、机器学习和 MóiBit 构建应用。

# 本书适用对象

本书适用于区块链和人工智能架构师、开发人员、数据科学家、数据工程师和传道者，他们希望将人工智能的力量引入区块链应用中。如果您想要理解如何在区块链解决方案中实现智能认知洞察的理论和实践用例的完美融合，那么本书正是您所需！需要对涉及到的机器学习和区块链概念有一定的了解。

# 本书内容

第一章，*区块链入门*，帮助您了解区块链的基础知识及各种形式和实现之间的对比。如果您已经熟悉区块链及其应用的基础知识，可以跳过本章，开始阅读第二章，*AI 景观简介*。

第二章，*AI 景观简介*，顾名思义，向您介绍了人工智能的基础知识和历史，并对其一些基本形式和实现进行了对比。如果您已经熟悉人工智能及其应用的基础知识，可以直接跳转至第四章，*AI- and Blockchain-Driven Databases*。

第三章，*AI 和区块链的特定领域应用**,*，涵盖了一些知名的区块链和人工智能应用。

第四章，*AI- and Blockchain-Driven Databases*，是学习如何将区块链与人工智能相连接的关键。我们将介绍并对比传统数据管理工具和去中心化数据库，以及文件系统。

第五章，*利用人工智能增强区块链*，涵盖了一些专属应用，这些应用同时使用人工智能和区块链来解决一些现实世界的挑战。

第六章，*加密货币与人工智能*，探讨了人工智能在加密货币交易中的一些应用。

第七章，*DIApp 的开发生命周期*，向您介绍了 DIApp 设计模式，并概述了涉及的**软件开发生命周期（SDLC）**流程。

第八章，*实现 DIApp*，演示了如何构建一个实时应用程序，利用区块链、人工智能和去中心化数据库来解决真实世界的挑战。

第九章，*区块链与人工智能的未来*，通过建议新的用例分析和应用书中的学习内容来结束本书，以构建您自己的 DIApp。

# 为了充分利用本书

虽然我们不指望您对区块链和人工智能的基础知识有透彻的了解，但熟悉这些技术会很有帮助。此外，本书的学习目标之一是了解如何构建一个结合了区块链和人工智能优势的 DIApp。如果您有兴趣学习如何构建 DIApp，应该熟悉 Solidity 智能合约、机器学习和 Python 的基础知识。

| **书中涵盖的软件/硬件** | **操作系统要求** |
| --- | --- |

| 开发一款可以帮助追踪动物和物体 COVID-19 感染情况的 DIApp 需要以下工具：

+   Python 3.7

+   Node.js 12

+   Firefox 或基于 Chromium 的浏览器并具有互联网访问权限

|

+   macOS Mojave 或更高版本

+   Ubuntu 18.04 LTS 或更高版本

|

**如果您正在使用本书的数字版本，我们建议您自己输入代码或通过 GitHub 存储库访问代码（链接在下一节中提供）。这样做可以帮助您避免与复制粘贴代码相关的潜在错误。**

## 下载示例代码文件

您可以从您在[www.packt.com](http://www.packt.com)的账户中下载本书的示例代码文件。如果您在其他地方购买了本书，您可以访问[www.packtpub.com/support](https://www.packtpub.com/support)并注册，将文件直接发送到您的邮箱。

您可以按照以下步骤下载代码文件：

1.  在[www.packt.com](http://www.packt.com)上登录或注册。

1.  选择支持选项卡。

1.  单击“代码下载”。

1.  在搜索框中输入书名并按照屏幕上的说明操作。

文件下载完成后，请确保使用以下最新版本的软件解压或提取文件夹：

+   WinRAR/7-Zip for Windows

+   Zipeg/iZip/UnRarX for Mac

+   7-Zip/PeaZip for Linux

本书的代码包也托管在 GitHub 上，网址是[`github.com/PacktPublishing/Hands-On-Artificial-Intelligence-for-Blockchain`](https://github.com/PacktPublishing/Hands-On-Artificial-Intelligence-for-Blockchain)。如果代码有更新，将在现有的 GitHub 存储库中更新。

我们还有其他代码包来自我们丰富的书籍和视频目录，可在[`github.com/PacktPublishing/`](https://github.com/PacktPublishing/)获取。请查看！

## 下载彩色图片

我们还提供了一个 PDF 文件，其中包含本书中使用的截图/图表的彩色图像。您可以在此处下载：[`static.packt-cdn.com/downloads/9781838822293_ColorImages.pdf`](https://static.packt-cdn.com/downloads/9781838822293_ColorImages.pdf)。

## 使用的约定

本书中使用了许多文本约定。

`CodeInText`：表示文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟网址、用户输入和 Twitter 句柄。这是一个例子："将下载的 `WebStorm-10*.dmg` 磁盘映像文件挂载为系统中的另一个磁盘。"

代码块设置如下：

```
modifier onlyBy(*address* _account) {
       require(
           *msg*.sender == _account,
           "Sender not authorized to update this mapping!"
       );
       _; // The "_;"! will be replaced by the actual function body when the modifier is used.
   }
```

任何命令行输入或输出都写成如下形式：

```
just run-server
```

**粗体**：表示新术语、重要词或屏幕上看到的词。例如，菜单中或对话框中的词在文本中显示如下。这是一个例子："创建区块的规则和区块的接受由称为 PoW 或 **权益证明**（**PoS**）的共识算法指定。"

警告或重要说明如下所示。

提示和技巧如下所示。
