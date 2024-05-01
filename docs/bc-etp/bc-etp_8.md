# 为银行建立支付解决方案

如今，有许多由银行和其他金融科技公司开发的应用程序和服务，让我们可以发送和接受付款。但是我们还没有一个应用程序能够使发送和接收资金像发送和接收短信一样简单。虽然比特币和其他加密货币使得全球范围内的付款变得非常简单，但由于波动性和监管问题，它们目前无法成为主流。在本章中，我们将建立一个 P2P 支付系统，使发送和接收银行间支付变得非常容易，并且在银行之间的结算和清算几乎实时和简单。在构建解决方案的同时，我们还将学习各种银行和金融概念。

在本章中，我们将学习以下内容：

+   银行间国内和国际电子转账的清算和结算方式

+   **全球银行间金融电信协会**（**SWIFT**）系统及银行间国际汇款的工作原理

+   如何在区块链上数字化法定货币以及它解决的问题

+   如何在 Quorum 中实现网络权限管理

+   如何构建使用手机号码转账的解决方案

# 支付系统概述

在本章中，我们将建立一个可集成在手机银行应用程序中的支付解决方案。这个解决方案将允许客户使用手机号码发送付款。只需使用手机号码就可以向世界上任何人发送付款将是发送付款的最友好方式。

我们的解决方案将使用数字化法定货币来进行银行间转账的结算和清算。为了理解为什么我们选择使用数字化法定货币作为结算媒介，让我们先了解一下银行间转账的结算和清算方式及其问题。

# 银行间转账的结算与清算

让我们首先了解国内银行间转账的工作原理。每个国家的中央银行都有一种或多种不同类型的集中式电子资金转移系统。例如，印度的**即时付款服务**（**IMPS**），美国的**自动清算机构**（**ACH**），加拿大的**电子资金转账**（**EFT**）。这些系统被各国银行用来向彼此发送消息，以促进向其客户的资金转移。只有消息被转移，而不是真正的资金。最终的结算通过结算账户进行。每家银行在中央银行都持有一个结算账户，当有转账消息时，资金要么在这些账户中存入，要么支出。为了更清楚地理解这一点，让我们看一个例子。

假设银行*A*在中央银行有一个结算账户，其中存入了$50,000。同样，假设银行*B*在中央银行有一个结算账户，其中包含$100,000。现在，假设*X*是银行*A*的客户，*Y*是银行*B*的客户。当*X*想向*Y*发送$100 时，银行*A*通过资金转移系统向银行*B*发送一条消息，指示已从*X*的账户中扣除$100，并将$100 存入银行*B*的*Y*账户中。收到该消息后，银行*B*立即将*Y*的账户存入$100。为了结算这笔款项，中央银行从银行*A*的结算账户中扣除$100，因此新余额为$49,900，并将$100 存入银行*B*的结算账户，因此新余额为$100,100。

中央银行通常在特定时间每天进行最终结算。消息传输几乎是实时的。只要涉及的两家银行都信任中央银行，这个过程就能正常运作。

让我们看看国际银行间转账是如何运作的。在这种情况下，涉及两个不同国家的两家银行，国际支付的方式与国内转账不同。在国际支付的情况下，银行使用 SWIFT 系统发送消息。SWIFT 是一种金融机构用于安全传输信息和指令的消息网络，通过标准化的代码系统进行传输。SWIFT 为每个金融组织分配一个唯一的代码，代码有 8 个或 11 个字符。该代码通常称为**银行识别代码**（**BIC**）、SWIFT 代码、SWIFT ID 或 ISO 9362 代码。

要了解更多关于 SWIFT 的信息，请访问[`www.investopedia.com/articles/personal-finance/050515/how-swift-system-works.asp`](https://www.investopedia.com/articles/personal-finance/050515/how-swift-system-works.asp)。

在这种情况下，两家银行不是与特定的中央银行拥有结算账户，而是彼此之间拥有结算账户。为了进一步理解这一点，让我们举个例子。假设银行*A*是一家美国银行，银行*B*是一家印度银行。*X*是银行*A*的客户，*Y*是银行*B*的客户。为了让*X*能向*Y*转账，反之亦然，银行*A*和银行*B*彼此之间持有结算账户。因此，银行*A*可能在银行*B*开设一个账户，其中存入了₹300,000，而银行*B*也会在银行*A*开设一个结算账户，其中存入了$100,000。现在，当*X*向*Y*发送价值$100 的付款时，银行*B*将从其管理的银行*A*的结算账户中扣除₹6909.50（本书编写时的汇率为 1 美元=69.10 印度卢比）。*X*的账户将被扣除$100，*Y*的账户将被存入₹6909.50。

通常需要五到七天才能反映在*Y*的账户中。这是由于许多必要的流程、检查和问题，比如以下内容：

+   由于外汇汇款促成了洗钱行业，银行必须进行某种背景检查，以确保您使用的资金不是来自非法来源。

+   汇款人的银行在其系统中搜索，查看是否与接收人账户所在的银行直接合作（持有结算账户）。通常情况下，这种情况不太可能发生。因此，汇款人的银行与他们有合作关系的银行联系，同时也知道接收方银行也是他们的合作伙伴。因此，这基本上形成了一个链条。有时，这个链条可能会在中间延伸到三四家银行，这取决于您所在国家的银行基础设施和经济开放程度。

+   大多数情况下，这些 SWIFT 消息只由汇款人银行的一个专门从事外汇汇款的分支机构发送。此外，汇款人分支机构会花费更多时间将消息发送到主分支机构，然后再由那里继续进行。

+   在许多国家，与法规和合规相关的支票仍需手动处理，从而增加了转账完成时间。

**存放款项账户（Nostro）**是银行**A**用来指代由银行**B**持有的我们的账户的术语。**您账户（Vostro）**是银行 B 使用的术语，其中存放着银行 A 的资金。

# 数字化法定货币

我们看到了银行间转账是如何运作的。对于国内转账，中央银行必须负责管理和更新结算账户，而对于国际转账，各银行必须努力更新结算账户。在国际转账的情况下，还存在其他问题，例如需要更多的对账工作，因为没有可信第三方，还有通过多个中介银行进行支付的路由。

区块链使银行能够通过提供数字化法定货币的能力直接将资金转移到世界上的任何其他银行；通过提供单一真相源，它减少了大量对账工作。

让我们来看看数字化法定货币在区块链上的过程和流程：

+   只有中央银行有权在区块链上发行各自的数字化法定货币。

+   我们可以为每种法定货币建立一个单独的网络，而不是使用单一网络分配流量并增加可扩展性。

+   要将法定货币转换为数字化形式，银行必须将法定货币存入中央银行的现金保管账户。中央银行将在区块链上向各银行发行同等金额的数字化法定货币。

+   银行可以随时通过在区块链上销毁数字化法定货币来将数字化法定货币转换回纸币。

+   为了实现匿名性，银行可以使用多个地址。因此，其他银行将难以预测谁拥有多少数字化法定货币。

# 使用手机号作为身份证明

我们的支付应用将基于使用手机号码作为接收方的身份。让我们看看使用区块链将手机号码作为支付标识符的整个过程：

+   区块链将充当与银行代码相关联的手机号码的共享和受保护的存储。

+   每个 ISD 代码将有自己的网络。这是出于可扩展性的考量。

+   每个手机号码可以与一个或多个银行相关联。如果有多个银行，则发送方可以选择要发送支付的银行账户。

+   接收方要使用手机号码接收支付，必须通过接收银行的手机银行应用在区块链上注册手机号码。

+   如果接收方的银行账户被暂停，应更新区块链上的状态，以通知其他人不要接受该手机号的支付。

# 构建网络

在我们继续编写智能合约之前，让我们为+1 ISD 代码的美元货币创建 Quorum 网络。我们将确保这些网络是经过许可的，并使用节点 ID 进行保护。

到目前为止，对于我们在本书中创建的所有网络，我们已经假设它们是使用白名单 IP 进行保护的。但是夸罗姆提供了一种方式来对节点 ID 进行白名单设置。你可以将相同的实践应用于本书中构建的其他网络。手机号码不应泄露到网络之外，因此重要性在于不惜一切代价保护网络。

# Quorum 中的网络许可

网络许可是通过在节点启动期间将`--permissioned`标志作为命令行参数添加到各个节点级别启用的。当添加该标志时，节点将在节点的数据目录文件夹中寻找名为`permissioned-nodes.json`的文件。

`permissioned-nodes.json`文件包含了该特定节点将接受来自和向外部连接的节点标识符（`enode://nodeID@ip:port`）的列表。

如果设置了`--permissioned`标志，但`permissioned-nodes.json`文件为空或根本不存在于节点的数据目录文件夹中，则节点将启动，但它既不会连接到任何其他节点，也不会接受来自其他节点的任何传入连接请求。

例如，在我们的案例中，我们需要至少三个节点网络，即 A 银行，B 银行和中央银行。假设 A 银行的节点 ID 是`480cd6ab5c7910af0e413e17135d494d9a6b74c9d67692b0611e4eefea1cd082adbdaa4c22467c583fb881e30fda415f0f84cfea7ddd7df45e1e7499ad3c680c`，B 银行的节点 ID 是`60998b26d4a1ecbb29eff66c428c73f02e2b8a2936c4bbb46581ef59b2678b7023d300a31b899a7d82cae3cbb6f394de80d07820e0689b505c99920803d5029a`以及中央银行的节点 ID 是`e03f30b25c1739d203dd85e2dcc0ad79d53fa776034074134ec2bf128e609a0521f35ed341edd12e43e436f08620ea68d39c05f63281772b4cce15b21d27941e`。

因此，Bank A 节点上的`permissioned-nodes.json`文件将包含以下内容：

```
[
  "enode://60998b26d4a1ecbb29eff66c428c73f02e2b8a2936c4bbb46581ef59b2678b7023d300a31b899a7d82cae3cbb6f394de80d07820e0689b505c99920803d5029a@[::]:23001?discport=0",
  "enode://e03f30b25c1739d203dd85e2dcc0ad79d53fa776034074134ec2bf128e609a0521f35ed341edd12e43e436f08620ea68d39c05f63281772b4cce15b21d27941e@[::]:23002?discport=0"
]
```

类似地，银行*B*将白名单中的银行*A*和央行，而央行将白名单中的银行*A*和银行*B*。

`permissioned-nodes.json`文件的任何添加都将在后续的传入/传出请求时动态地被服务器接收。节点不需要重新启动以使更改生效，但是从`permissioned-nodes.json`文件中删除现有连接的节点不会立即断开这些现有连接的节点。但是，如果出于任何原因断开了连接，并且从已断开的节点 ID 发出了后续的连接请求，那么该请求将作为该新请求的一部分被拒绝。

# 构建 DApp

让我们编写智能合约，将法定货币数字化并存储与银行账户关联的手机号。这是用于数字化法定货币的智能合约：

```
pragma solidity ⁰.4.18;

contract USD {
    address centralBank;

    mapping (address => uint256) balances;
    uint256 totalDestroyed;
    uint256 totalIssued;

    event usdIssued(uint256 amount, address to);
    event usdDestroyed(uint256 amount, address from);
    event usdTransferred(uint256 amount, address from, address to,
      string description);

    function USD() {
        centralBank = msg.sender;
    }

    function issueUSD(uint256 amount, address to) {
        if(msg.sender == centralBank) {
            balances[to] += amount; 
            totalIssued += amount;
            usdIssued(amount, to);
        }
    }

    function destroyUSD(uint256 amount) {
        balances[msg.sender] -= amount;
        totalDestroyed += amount;
        usdDestroyed(amount, msg.sender);
    }

    function transferUSD(uint256 amount, address to, string 
      description) {
        if(balances[msg.sender] >= amount) {
            balances[msg.sender] -= amount;
            balances[to] += amount;
            usdTransferred(amount, msg.sender, to, description);
        }
    }

    function getBalance(address account) returns (uint256 balance) {
        return balances[account];
    }

    function getTotal() returns (uint256 totalDestroyed, uint256 
      totalIssued) {
        return (totalDestroyed, totalIssued);
    }
}
```

这是上述代码的工作原理：

+   我们假设央行部署了智能合约。

+   我们有方法来发行、转移和销毁`USD`。这些方法都是不言自明的。转移方法还有一个描述，可以包含诸如交易目的或接收客户详细信息等信息。

+   我们有方法来检索账户的余额，以及发行和销毁的总`USD`。

以下是保存手机号及其对应银行的智能合约：

```
pragma solidity ⁰.4.18;

contract MobileNumbers {
    address centralBank;

    struct BankDetails {
        string name;
        bool authorization;
    }

    mapping (address => BankDetails) banks;
    mapping (uint256 => address[]) mobileNumbers;

    event bankAdded(address bankAddress, string bankName);
    event bankRemoved(address bankAddress);
    event mobileNumberAdded(address bankAddress, uint256 mobileNumber);

    function MobileNumbers() {
        centralBank = msg.sender; 
    }

    function addBank(address bank, string bankName) {
        if(centralBank == msg.sender) {
            banks[bank] = BankDetails(bankName, true);
            bankAdded(bank, bankName);
        }
    }

    function removeBank(address bank) {
        if(centralBank == msg.sender) {
            banks[bank].authorization = false;
            bankRemoved(bank);
        } 
    }

    function getBankDetails(address bank) view returns (string 
      bankName, bool authorization) {
        return (banks[bank].name, banks[bank].authorization);
    }

    function addMobileNumber(uint256 mobileNumber) {
        if(banks[msg.sender].authorization == true) {
            for(uint256 count = 0; count < 
              mobileNumbers[mobileNumber].length; count++) {
                if(mobileNumbers[mobileNumber][count] == msg.sender) {
                    return;
                }
            }

            mobileNumbers[mobileNumber].push(msg.sender);
            mobileNumberAdded(msg.sender, mobileNumber);
        }
    }

    function removeMobileNumber(uint256 mobileNumber) {
        if(banks[msg.sender].authorization == true) {
            for(uint256 count = 0; count < 
              mobileNumbers[mobileNumber].length; count++) {
                if(mobileNumbers[mobileNumber][count] == msg.sender) {
                    delete mobileNumbers[mobileNumber][count];

                    //fill the gap caused by delete
                    for (uint i = count; i < 
                      mobileNumbers[mobileNumber].length - 1; i++){
                        mobileNumbers[mobileNumber][i] = 
                          mobileNumbers[mobileNumber][i+1];
                    }
                    mobileNumbers[mobileNumber].length--;

                    break;
                }
            }
        }
    }

    function getMobileNumberBanks(uint256 mobileNumber) view returns
     (address[] banks) {
        return mobileNumbers[mobileNumber];
    }
}
```

以下是上述代码的工作原理：

+   我们假设央行部署了该合约。

+   然后央行可以添加或删除银行到网络中。每家银行在区块链上都有一个账户。我们不能使用任何账户编写手机号，因为这将让银行进行欺诈——即使他们没有拥有与手机号相关联的账户，他们仍然会添加它并且不会被发现。预定义的账户将实现审计，以便银行不会接受他们不持有的手机号账户的付款。

+   然后我们有一个添加手机号的方法。每个手机号都与用户拥有账户的很多银行相关联。在发送付款时，付款人可以选择其中一个账户。

+   然后我们有一个方法，银行可以使用它将自己从手机号中移除。当银行账户被暂停或关闭时，这是有用的。

+   最后，我们有一个函数可以获取与手机号关联的银行账户列表。

现在让我们看看用户支付和结算的整个流程：

+   假设*X*在银行*A*有一个账户，*Y*在银行*B*有一个账户。两家银行都在两个网络上注册，并且两家银行在`USD`网络上有足够的`USD`。

+   为了使用手机号接收付款，*Y*必须使用银行*B*的手机号在`MobileNumbers`网络上注册其手机号。银行*B*将调用`addMobileNumber`方法在网络上注册*Y*的银行账户。

+   要让 *X* 向 *Y* 发送付款，*X* 必须在银行 *A* 的手机银行应用程序中输入 *Y* 的手机号码。之后，银行 *A* 将调用 `getMobileNumberBanks` 方法以获取 *Y* 拥有账户的银行列表。银行 *B* 必然会被列出，所以 *X* 可以选择它并点击“发送付款”按钮。

+   一旦点击“发送付款”按钮，银行 *A* 将调用 `transferUSD` 方法，并在说明中提供 *Y* 的手机号码，表示要将资金存入的银行账户。`transferUSD` 中的 `to` 地址将是由 `getMobileNumberBanks` 方法返回的地址。

# 摘要

在本章中，我们学习了一些银行业的基本概念，以及银行间转账是如何结算和清算的。我们还了解了 SWIFT 以及它的工作原理。然后我们深入了解了 Quorum 中的高级网络许可，并学习了 `--permissioned` 标志。

最后，我们建立了一种新型的资金转移系统，使用数字化的法定货币和客户识别的手机号进行支付结算。我们将整个解决方案构建在区块链上，这样可以最大程度地减少调节工作量，并解决了以前无法解决的许多问题。
