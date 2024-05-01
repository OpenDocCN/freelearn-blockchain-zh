# 前言

区块链和**物联网（IoT）**已被证明是目前最受欢迎的技术，并且只是开始应用它们的曲线。多家大公司的首要任务之一是整合区块链和物联网，其中一些公司已经开始在几个项目中使用其实施、解决方案和倡议。

这本书将帮助您使用最佳实践开发区块链和物联网解决方案。

# 本书适用对象

本书适用于负责物联网基础设施安全机制的任何人，以及希望在 IBM Cloud 平台上开发使用区块链和物联网解决方案的 IT 专业人员。需要对物联网有基本的了解。

# 本书涵盖内容

第一章，*了解物联网并在 IBM Watson IoT 平台上开发设备*，帮助您了解物联网如何改变游戏规则，哪些行业可以利用这项技术，如何开始进入物联网世界以及 IBM IoT 平台提供哪些功能以及在创建物联网解决方案时如何利用这些功能。

第二章，*创建您的第一个物联网解决方案*，帮助您使用平台和树莓派创建您的第一个端到端物联网解决方案，以锻炼您的技能。您将创建一个自动浇水的花园系统，该系统使用平台来确保植物得到充足的水分。

第三章，*解释区块链技术并与 Hyperledger 合作*，介绍了区块链并帮助您理解它是如何与分类帐一起工作的，以记录提供已知身份的许可网络的交易历史。

第四章，*创建您自己的区块链网络*，帮助您使用 Hyperledger Composer 创建您自己的区块链网络，并探讨如何创建资产、交易功能、访问控制和查询定义。

第五章，*解决食品安全 - 围绕区块链构建*，帮助您设计和实施解决方案以解决物流问题。您将了解到，如何使用物联网和区块链解决方案可以确保从农场开始并在一个人的盘子上结束的食品链可以在整个旅程中安全跟踪使用这些技术，并且为了在未来几年中获得许多国家的遵从而应用这种做法。

第六章，*设计解决方案架构*，帮助您从食品安全运输业务问题出发设计解决方案架构，并定义使用区块链支持分布式分类帐网络和物联网设备的技术解决方案的要求，并且定义用于支持跟踪过程的平台。

第七章，*创建您的区块链和物联网解决方案*，向您展示了如何创建一个区块链和物联网集成解决方案来解决食品安全运输问题。您将通过编码和测试前一章中设计的组件来获得使用区块链和物联网平台的实践经验。

第八章，*物联网、区块链和工业 4.0*，帮助您了解行业趋势以及可以从物联网和区块链解决方案中创建或派生出的新业务模式，以及关于这些技术的市场和技术趋势。

第九章，*开发区块链和物联网解决方案的最佳实践*，帮助您理解之前的项目经验和场景，并查看设计和开发区块链和物联网解决方案的最佳实践和经验教训。

# 为了最大限度地利用本书的内容

我们期望您熟悉一种编程语言，并具有为任何嵌入式平台（如树莓派、Arduino、ESP8266 或英特尔 Edison）开发任何解决方案的一些经验。我们将主要使用 Node.js 和 Hyperledger Composer 建模语言。欢迎初学者级别的 JavaScript 技能。

# 下载示例代码文件

您可以从[www.packt.com](http://www.packt.com)账户下载本书的示例代码文件。如果您在其他地方购买了本书，您可以访问[www.packt.com/support](http://www.packt.com/support)并注册，文件将直接通过电子邮件发送给您。

您可以按照以下步骤下载代码文件：

1.  在[www.packt.com](http://www.packt.com)登录或注册。

1.  选择"SUPPORT"标签。

1.  点击"Code Downloads & Errata"。

1.  在搜索框中输入书名并按照屏幕上的说明操作。

下载文件后，请确保使用最新版本的解压或提取文件夹：

+   用于 Windows 的 WinRAR/7-Zip

+   用于 Mac 的 Zipeg/iZip/UnRarX

+   用于 Linux 的 7-Zip/PeaZip

本书的代码包也托管在 GitHub 上，网址为[`github.com/PacktPublishing/Hands-On-IoT-Solutions-with-Blockchain`](https://github.com/PacktPublishing/Hands-On-IoT-Solutions-with-Blockchain)。如果代码有更新，将在现有的 GitHub 存储库上进行更新。

我们还提供来自丰富图书和视频目录的其他代码包，可在**[`github.com/PacktPublishing/`](https://github.com/PacktPublishing/)**获取。快去看看吧！

# 下载彩色图像

我们还提供了一个 PDF 文件，其中包含本书中使用的截图/图表的彩色图像。您可以从这里下载：[`www.packtpub.com/sites/default/files/downloads/9781789132243_ColorImages.pdf`](https://www.packtpub.com/sites/default/files/downloads/9781789132243_ColorImages.pdf)。

# 使用的约定

本书中使用了许多文本约定。

`CodeInText`：指示文本中的代码字词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 句柄。以下是一个示例：“接下来，打开您喜欢的 IDE，创建一个新的 Node.js 项目，并安装`ibmiotf`依赖包。”

代码块设置如下：

```
{
  "org": "<your iot org id>",
  "id": "<any application name>",
  "auth-key": "<application authentication key>",
  "auth-token": "<application authentication token>"
}
```

当我们希望引起您对代码块的特定部分的注意时，相关的行或项将以粗体显示：

```
"successRedirect": “<redirection URL. will be overwritten by the property 'json: true'>”,
"failureRedirect": "/?success=false",
"session": true,
```

任何命令行输入或输出均以以下格式书写：

```
$ npm start
> sample-device@1.0.0 start /sample-device
```

**粗体**：指示一个新术语、一个重要词或您在屏幕上看到的单词。例如，菜单或对话框中的单词在文本中出现如下。以下是一个示例：“从设置步骤中创建的 IoT 平台服务中，在菜单中选择设备，然后选择添加设备*。”

警告或重要说明会显示为此格式。

提示和技巧显示为此格式。

# 联系我们

我们非常欢迎读者的反馈意见。

**一般反馈**：如果您对本书的任何方面有疑问，请在邮件主题中提及书名，并通过电子邮件联系我们，地址为`customercare@packtpub.com`。

**勘误表**：尽管我们已经尽一切努力确保内容的准确性，但错误是难免的。如果您在本书中发现了错误，我们将不胜感激地接受您的报告。请访问[www.packt.com/submit-errata](http://www.packt.com/submit-errata)，选择您的书籍，点击勘误提交表单链接，并输入详细信息。

**盗版**：如果您在互联网上发现我们作品的任何形式的非法副本，请向我们提供位置地址或网站名称，我们将不胜感激。请通过链接`copyright@packt.com`与我们联系。

**如果您有兴趣成为作者**：如果您有专业知识，并且有兴趣撰写或为一本书做出贡献，请访问[authors.packtpub.com](http://authors.packtpub.com/)。

# 评论

请留下评论。一旦您阅读并使用了本书，为什么不在购买书籍的网站上留下评论呢？潜在的读者可以通过您的客观意见来做出购买决定，我们在 Packt 可以了解您对我们产品的看法，我们的作者可以看到您对他们书籍的反馈。谢谢！

有关 Packt 的更多信息，请访问[packt.com](http://www.packt.com/)。
