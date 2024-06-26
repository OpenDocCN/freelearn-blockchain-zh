# 第四章：区块链背后的密码学和机制

区块链的使用依赖于密码学。数值密码学可以被视为一种近代发明，过去的密码依赖于交换单词和字母。正如我们将看到的，现代密码学是一种非常强大的工具，用于保护通信，并且，对于我们的主题非常重要的是，确定数字签名的来源和数字资产的真实性。

本章将涵盖以下主题：

+   安全原则

+   历史视角 - 古典密码学

+   密码签名

+   散列

# 安全原则

密码学保护了信息安全的三个原则，可以通过记忆设备**中央情报局**（**CIA**）来记忆：

+   **保密性**：确保信息与适当的方当共享，并且敏感信息（例如，医疗信息，某些财务数据）仅在适当方的同意下共享。

+   **完整性**：确保只有授权方可以更改数据，并且（根据应用程序的情况）所做的更改不会威胁数据的准确性或真实性。这一原则可以说是与区块链最相关的，尤其是公共区块链。

+   **可用性**：确保授权用户（例如，持有代币的用户）在需要或想要时可以使用数据或资源。区块链的分布式和去中心化特性极大地有助于实现这一点。

与区块链和加密货币的相关性立即显而易见：例如，如果区块链没有提供完整性，那么用户试图花费的资金或代币是否存在将无法确定。对于区块链的典型应用，其中链可能持有房地产或证券的产权，数据完整性确实非常重要。在本章中，我们将讨论这些原则与区块链的相关性以及如何通过加密来保证诸如完整性等内容。

# 历史视角 - 古典密码学

*密码学*是指用于保护信息或通信的任何方法或技术，特别是指用于安全通信方法和协议的研究。过去，密码学是指加密，这是一种指代用于编码信息的技术。

在其最基本的形式中，加密可以采用替换密码的形式，即根据双方预先共享的代码，对消息中的字母或单词进行替换。经典示例是凯撒密码，其中个别字母根据它们在字母表中的位置索引，并向前移动给定数量的字符。例如，字母*A*可能会变成字母*N*，密钥为 13。

这种具体形式的凯撒密码被称为 **ROT****13**，它可能是唯一继续定期使用的替换密码——它为用户提供了一种轻松可逆的方式来隐藏粗言秽语或静态网站上的谜题解答（当然，同样的功能也可以非常简单地用 JavaScript 实现）。

这个非常简单的例子介绍了两个重要概念。第一个是算法，它是对具有可预测、确定性结果的特定计算的正式描述。取消息中的每个字符，并在字母表中向前移动 *n* 个位置。第二个是密钥：在这个算法中 *n* 是 13。在这种情况下，密钥是一个预共享的秘密，一种双方（或更多）已经同意的代码，但正如我们将看到的，这并不是唯一的密钥类型。

# 密码学类型

密码学主要分为对称加密和非对称加密。对称加密是指密钥是预共享或协商的加密方式。AES、DES 和 Blowfish 是对称加密中使用的算法的示例。

# 对称密码学

大多数精通计算机的用户都熟悉 WEP、WPA 或 WPA2，这些是 Wi-Fi 连接中使用的安全协议。这些协议存在的目的是防止无线连接上传输的数据被截取和篡改（或者换句话说，为无线用户提供保密性和完整性）。现在的路由器通常都会在上面打印无线密码，这是一个非常直接的预共享密钥的例子。

对称加密中使用的算法通常非常快速，并且与非对称加密相比，生成新密钥（或使用它加密/解密数据）所需的计算能力相对较少。

# 非对称（公钥）密码学

非对称密码学（也称为公钥密码学）采用两个密钥：一个公钥，可以广泛共享，一个私钥，保持秘密。公钥用于加密传输到私钥持有者的数据。然后使用私钥进行解密。

公钥密码学的发展使得诸如电子商务网上银行等事物得以发展，并补充了经济的很大一部分。它使电子邮件具有一定的保密性，并使财务报表可通过网络门户获得。它还使电子税务申报成为可能，并使我们能够信任地与也许是完全陌生的人分享我们最私密的秘密——你可能会说它使整个世界更加紧密地联系在一起。

由于公钥不需要保密，它允许诸如证书颁发机构和 PGP 密钥服务器之类的东西存在——发布用于加密的密钥，只有持有私钥的人才能解密使用该发布密钥加密的数据。用户甚至可以发布加密文本，这种方法会享有一定程度的匿名性——将加密文本放在新闻组、电子邮件邮件列表或社交媒体上的群组中会使其被众多人接收，而任何窃听者都无法确定预期的接收者。这种方法在区块链世界中也会很有趣——成千上万个节点镜像着一个没有已知接收者的密文，也许永远，无法撤销，而且接收者绝对否认。

公钥密码学比对称密码学的计算成本更高，部分原因是使用的密钥大小巨大。美国国家安全局目前要求商业应用中密钥大小为 3,072 位或更大，这是公钥密码学的主要用途。相比之下，128 位加密通常被认为对大多数加密应用足够了，256 位是美国国家安全局对保密性的标准。

大多数情况下，虽然可以仅使用公钥算法，但公钥密码学最常见的用途是为其余会话协商对称密钥。在大多数实现中，对称密钥不会被传输，因此，如果攻击者夺取了一个或两个私钥，他们将无法访问实际的通信内容。这个特性被称为前向保密性。

有些协议，比如 SSH，用于远程访问计算机，非常积极主动。在一个会话期间，SSH 会定期更改密钥。SSH 还展示了公钥密码学的基本属性——可以将您的公钥放在远程服务器上进行身份验证，而不会有任何固有的保密问题。

如今大多数使用的加密技术都不是不可破解的，只要有极大（或无限）的计算资源。然而，适用于保护需要保密性的数据的算法被认为是计算上不太可能的——也就是说，破解加密所需的计算资源不存在，并且在不久的将来也不会存在。

# 签名

值得注意的是，尽管在对数据进行加密以发送到特定接收者时，使用私钥进行解密，但通常可以反向操作。对于加密签名，使用私钥生成一个可以使用给定用户发布的公钥解密（验证）的签名。这种公钥加密的反转用法允许用户以明文发布消息，并高度确信签署者是写下这条消息的人。这再次引出了完整性的概念——如果由用户的私钥签署，消息（或交易）可以被假定为真实的。通常，在涉及区块链的情况下，当用户希望转移代币时，他们会用钱包的私钥签署交易。然后用户广播该交易。

现在多重签名钱包也相当常见，在这种情况下，交易最常由多个用户签名，然后广播，要么在托管钱包服务的网络界面上，要么在本地客户端中。这在具有分布式团队的软件项目中是一个相当常见的用例。

# 哈希化

与加密概念不同（并且存在于许多加密机制中，例如加密签名和身份验证）的是哈希，它指的是一种确定性算法，用于将数据映射到固定大小的字符串。除了确定性外，加密哈希算法必须具有几个其他特征，本节将介绍这些特征。

正如我们将在接下来的部分看到的那样，哈希函数必须难以反向操作。大多数通过高中代数的读者会记得被因式分解折磨过。乘法是一个容易完成的操作，但是很难反向操作——找到一个大数的公因数需要更多的努力，而不是将该数作为乘法的乘积创建出来。这个简单的例子实际上享有实际应用。适当大的数，它们是两个质数的乘积——被称为**半素数**或（较少见的）**双质数**——在 RSA 中得到了应用，这是一种广泛使用的公钥密码算法。

RSA 是公钥加密中的黄金标准，使得 SSH、SSL 和诸如 PGP 等电子邮件加密系统成为可能。基于这样的操作——单向容易，反向非常困难——才使得加密技术如此强大。

# 雪崩效应

坚固的哈希算法的一个理想特征称为**雪崩效应**。输入的微小变化应导致输出的剧烈变化。例如，使用输出重定向和大多数 Linux 发行版中都有的 GNU `md5sum` 实用程序，以下是三个示例：

```
$ echo "Hills Like White Elephants by Ernest Hemingway" | md5sum
86db7865e5b6b8be7557c5f1c3391d7a -
$ echo "Bills Like White Elephants by Ernest Hemingway" | md5sum
ccba501e321315c265fe2fa9ed00495c -
$ echo "Bills Like White Buffalo by Ernest Hemingway"| md5sum
37b7556b27b12b55303743bf8ba3c612 -
```

将一个单词改为完全不同的单词与改变一个字母的结果相同：每个哈希都完全不同。在密码哈希的情况下，这是一个非常理想的属性。一个恶意的黑客无法接近它，然后尝试类似密码的排列。然而，我们将在接下来的章节中看到，哈希并不完美。

# 碰撞

理想的哈希函数不会发生**碰撞**。碰撞是两个输入产生相同输出的情况。碰撞会削弱哈希算法，因为可能通过错误的输入获得预期的结果。由于哈希算法用于根证书的数字签名、密码存储和区块链签名，一个具有许多碰撞的哈希函数可能允许恶意黑客从密码哈希中检索密码，从而用于访问其他帐户。一个具有许多碰撞的弱哈希算法可能有助于中间人攻击，使攻击者能够完美地欺骗**安全套接字层**（**SSL**）证书。

MD5，上面示例中使用的算法，被认为不适用于加密哈希。区块链幸运地大部分使用更安全的哈希函数，如 SHA-256 和 RIPEMD-160。

# 哈希一个块

在 PoW 系统中，向区块链添加新条目需要计算哈希值。在比特币中，矿工必须对块中的当前交易计算两个 SHA-256 哈希值，并且其中包括上一个块的哈希值。

对于哈希算法来说，这相当简单。让我们简要重申一下：理想的哈希函数接受预期输入，然后输出唯一的哈希值。它是确定性的。只有一个可能的输出，并且使用不同的输入无法获得该输出（或者在计算上几乎不可能）。这些属性确保了矿工可以处理一个区块，并且每个矿工可以返回相同的结果。正是通过哈希，区块链获得了对其采用和当前流行至关重要的两个属性：分散性和不可变性。

将当前块链接到前一个块和后续块在某种程度上是使区块链成为一个不断增长的交易链表（为其提供不可变性属性）的原因之一，哈希算法的确定性特性使得每个节点都能够毫无问题地获得相同的结果（为其提供了分散性）。

# Hashing 在 PoW 之外

除了工作量证明外，PoS 和 DPoS 也利用哈希，而且在很大程度上是出于相同的目的。已经有大量讨论专门讨论 PoS 是否会取代 PoW 并阻止我们运行数千台计算机执行耗费巨大的碳足迹的繁琐哈希计算。

尽管 PoW 系统的功耗和环境影响相当大，但似乎仍然存在。可以说，其原因是非常简单的经济学原理：矿工有动机通过计算哈希来验证交易，因为他们可以获得新产生的代币中的一部分。对于权益证明或分布式权益证明的更复杂的代币经济方案，通常不经得起检验。

举个例子，股票照片区块链项目的想法——我们称之为 Cannistercoin。用户向股票照片网站贡献照片，作为回报他们会收到代币。这个代币也可用于从网站购买股票照片，并且该代币在交易所上交易。

这样看起来似乎可行，而且是一个完整的市场——Cannistercoin 已经确定了买家和卖家，并且有一种机制来匹配它们，但也许这不是一个功能性的市场。这里的准入壁垒很高：买家可以使用任何普通的股票照片网站，并使用他们的信用卡或银行账户。在这个模式中，买家需要注册一个交易所，并交换加密货币以换取代币。

要真正地去中心化，这种经济模型还缺少一个重要组成部分——这就是激励系统。是什么激励见证者或验证者运行他们的机器并验证交易呢？

你可以给他们一些代币的份额，但为什么他们不会立即出售他们的代币以便收回运行机器的成本呢？可以合理地预期，这种不断的抛售压力会压低许多应用币加密货币的代币价格，这是一件遗憾的事情。在处理能力方面，权益证明系统通常更为优雅（以牺牲更为优雅的经济模型为代价）。

股权证明（或其他机制）很可能仍然会占据世界，但不管怎样，可以肯定地期望加密世界将做大量的哈希计算。

# 总结

区块链和加密货币的世界主要得益于上个世纪密码学的创新。我们已经介绍了密码学的概念和具体的加密操作，尤其是哈希操作，在区块链背后起着巨大作用。

在下一章中，我们将在此基础上介绍比特币，这是第一个（也是最显著的）区块链应用。
