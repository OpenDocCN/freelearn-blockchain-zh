# 前言

本书旨在让你深入了解以太坊区块链世界，并让你使用以太坊制作自己的加密货币。在本书中，你将学习各种概念，并直接应用这些知识，同时还将介绍以太坊区块链在未来提供的广泛功能范围。

# 本书适合谁阅读

如果你是一个对区块链如何运作充满热情的人，或者你是一个希望从事加密货币工作或对黑客技术感兴趣的爱好者，那么这本书就是为你准备的。

# 要充分利用这本书

至少需要了解一种面向对象的编程语言。如果你懂一些 JavaScript 就更好了。

我们将大量使用 NPM，并且会回顾区块链的基础知识，但一些先前的基础知识当然是有益的。

# 下载示例代码文件

您可以从 [www.packtpub.com](http://www.packtpub.com) 账户下载本书的示例代码文件。如果您在其他地方购买了本书，您可以访问 [www.packtpub.com/support](http://www.packtpub.com/support) 并注册，文件将直接发送到您的邮箱。

您可以按照以下步骤下载代码文件：

1.  请登录或注册 [www.packtpub.com](http://www.packtpub.com/support)。

1.  选择“支持”选项卡。

1.  点击“代码下载 & 勘误”。

1.  在搜索框中输入书名，然后按照屏幕上的说明操作。

文件下载完成后，请确保使用最新版本的软件解压或提取文件夹：

+   Windows 用户请使用 WinRAR/7-Zip

+   Mac 用户请使用 Zipeg/iZip/UnRarX

+   Linux 用户请使用 7-Zip/PeaZip

本书的代码包也托管在 GitHub 上，网址为 [`github.com/PacktPublishing/Ethereum-Projects-for-Beginners`](https://github.com/PacktPublishing/Ethereum-Projects-for-Beginners)。如果代码有更新，将会更新到现有的 GitHub 代码库上。

我们还有其他代码包来自我们丰富的图书和视频目录，可在 **[`github.com/PacktPublishing/`](https://github.com/PacktPublishing/)** 上查看！快来看看吧！

# 使用的约定

本书中使用了许多文本约定。

`CodeInText`：表示文本中的代码字词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟网址、用户输入和 Twitter 句柄。这里有一个例子：“这可以通过在终端窗口中输入 `ganache-cli` 来完成。”

代码块设置如下：

```
function MetaCoin() public {
    balances[tx.origin] = 10000;
  }
```

任何命令行输入或输出都将按照以下方式显示：

```
C:\Windows\System32\my_project>truffle-cli compile
```

**粗体**：表示一个新术语、一个重要单词或屏幕上显示的单词。例如，菜单或对话框中的单词在文本中显示为这样。这里有一个例子：“在上述提到的本地主机 URL 中输入并点击“保存”。”

警告或重要说明会以这种方式出现。

小贴士和技巧会以这种方式出现。
