# 前言

区块链正在迅速增长，并改变着商业的运作方式。领先的组织已经在探索区块链的可能性。通过本书，你将学会如何构建端到端的企业级**去中心化应用程序**（**DApps**）并在组织中扩展它们以满足公司的需求。

本书将帮助你了解什么是 DApps 以及区块链生态系统的运作方式，并通过一些实际的例子来说明。这是一本全面的端到端书籍，涵盖了区块链的各个方面，例如它对于企业和开发人员的应用。它将帮助你了解流程，以便你可以将其纳入到你自己的企业中。你将学会如何使用 J.P.摩根的 Quorum 构建基于区块链的应用程序。你还将介绍如何编写能够在企业区块链解决方案中通信的应用程序。你将学会编写无需审查和第三方干预即可运行的智能合约。

一旦你对区块链有了很好的理解，并学习了关于 Quorum 的一切，你就会开始构建真实世界的区块链应用，应用于支付和货币转移、医疗保健、云计算、供应链管理等领域。

# 本书适合谁

本书适合创新者、数字化变革者和想要使用区块链技术构建端到端 DApps 的区块链开发人员。如果你想要在企业范围内扩展你现有的区块链系统，你也会发现这本书很有用。它为你提供了解决企业实际问题所需的实用方法，结合了理论和实践的方法。

# 如何充分利用本书

你必须具备 JavaScript 和 Python 编程语言的使用经验。

你必须之前开发过分布式网络应用。

你必须了解基本的加密概念，如签名、加密和哈希。

# 下载示例代码文件

你可以从你在[www.packt.com](http://www.packt.com)的账户中下载本书的示例代码文件。如果你在其他地方购买了这本书，你可以访问[www.packt.com/support](http://www.packt.com/support)并注册，将文件直接发送到你的邮箱。

你可以通过以下步骤下载代码文件：

1.  在[www.packt.com](http://www.packt.com/)上登录或注册。

1.  选择“支持”选项卡。

1.  点击“代码下载 & 勘误”。

1.  在搜索框中输入书名并按照屏幕上的指示操作。

下载文件后，请确保使用最新版本的解压缩软件解压缩文件夹：

+   适用于 Windows 的 WinRAR/7-Zip

+   适用于 Mac 的 Zipeg/iZip/UnRarX

+   适用于 Linux 的 7-Zip/PeaZip

本书的代码包也托管在 GitHub 上，网址为[`github.com/PacktPublishing/Blockchain-for-Enterprise`](https://github.com/PacktPublishing/Blockchain-for-Enterprise)。 如果代码有更新，它将在现有的 GitHub 仓库中更新。

我们还有来自我们丰富的图书和视频目录的其他代码包可供使用，位于**[`github.com/PacktPublishing/`](https://github.com/PacktPublishing/)**。快来看看吧！

# 使用的约定

在本书中使用了许多文本约定。

`CodeInText`：表示文本中的代码词，数据库表名，文件夹名，文件名，文件扩展名，路径名，虚拟 URL，用户输入和 Twitter 句柄。例如："在使用`raft.addPeer`添加节点时，将出现此 Raft ID。"

代码块设置如下：

```
url = "http://127.0.0.1:9002/"
port = 9002
storage = "dir:./cnode_data/cnode2/"
socket = "./cnode_data/cnode1/constellation_node2.ipc"
othernodes = ["http://127.0.0.1:9001/"]
publickeys = ["./cnode2.pub"]
privatekeys = ["./cnode2.key"]
tls = "off"
```

任何命令行输入或输出均如下所示：

```
git clone https://github.com/jpmorganchase/quorum.git
cd quorum
make all
```

**粗体**：表示一个新术语，一个重要词或者你在屏幕上看到的词。例如，菜单中的词或对话框中的词在文本中显示为这样。示例："现在选择一个文件，输入所有者的名字，然后点击 提交。 ."

警告或重要说明会出现在这里。

提示和技巧会出现在这里。
