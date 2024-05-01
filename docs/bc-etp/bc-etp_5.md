# 第五章：建立互操作性区块链

有各种许可和公共区块链代表不同的资产、信息和业务流程。使用这些网络的主要需求之一是使它们能够彼此通信，这是一个需要克服的主要挑战。如果只有一个区块链能够统治它们所有，那将是很棒的，但绝对不会发生，因为只有一个区块链无法在安全性、隐私性、效率性、灵活性、平台复杂性、开发人员易用性等方面取得胜利。未来的区块链将是数个公共和许可区块链之间的互操作。本章中，我们将探讨如何实现多个夸梦网络之间的互操作性。

在本章中，我们将涵盖以下主题：

+   理解区块链互操作性及相关的各种流行项目

+   研究互操作性区块链可以实现的用例

+   鉴于可以用于实现区块链互操作性的各种技术和模式

+   建立代表**联邦硬币**的互操作性区块链网络

# 理解区块链互操作性

互操作性的区块链是可以彼此通信的区块链。每个区块链可以读取另一个区块链的状态。很多时候，你可能想要启用你的智能合约与中心化或其他去中心化的应用交互，当我们谈论 DApp 之间的互操作性时，我们在谈论与区块链之间的互操作性有所不同。

使以太坊智能合约能够检查文件是否存在于 IPFS 中，这就实现了 DApp 之间的互操作性，而使以太坊智能合约能够获取比特币账户余额则实现了区块链之间的互操作性。

让以太坊智能合约调用 REST API 以调用中心化应用被认为是在以太坊和 WWW 之间实现互操作性。但在本章中，我们将了解区块链之间的互操作性，特别是夸梦网络之间的互操作性。

目前正在开发的各种流行项目旨在采用去中心化机制实现区块链之间的互操作性。各种流行项目，如**Cosmos**、**Polkadot**、**Interledger**、**Block Collider**等，正在积极开发，以使区块链互操作化去中心化和易于实现，但在本章中我们不会涵盖这些项目，因为它们的目标是将互操作性带给公共区块链。然而，我们将学习创建互操作性区块链所使用的策略，正在被这些项目使用。

# 互操作性的区块链可以实现什么？

在进一步研究如何实现区块链互操作性之前，我们需要知道互操作性区块链可以实现哪些事情。显然，互操作性区块链有许多用例，但我们将重点关注互操作性区块链主要旨在实现的用例。它可以实现以下一个或多个用例：

+   **可移植资产**：在不同区块链之间来回转移资产。这也被称为**一对一挂钩**或**双向挂钩**。

+   **支付对支付和支付对交付**：技术上称为**原子交换**。当两个用户交换存在于两个不同区块链中的资产时，需要一个保证，即要么两个转账都发生，要么都不发生。例如，如果一个区块链持有数字化美元，另一个区块链持有数字化欧元，那么用户应该能够原子地交换这些资产。

+   **获取信息并对事件做出反应**：一个区块链能够读取另一个区块链上存在的信息，或者对另一个区块链上发生的交易做出反应。例如，一个区块链代表租赁合同，另一个代表锁定的安全押金，因此当第一个区块链中的租赁合同到期时，第二个区块链应自动释放安全押金。

# 实现区块链互操作性的策略

让我们看看您可以实现前述一个或多个用例的各种策略。我们将学习可以在 Quorum 中实施的策略，但不是所有公共或其他区块链都可用的策略。我们还将看一些如何实现这些策略的示例。

# 单一托管者

实现互操作性的最简单方法是通过一个集中式第三方，使区块链能够相互通信。基本上，您需要信任这个第三方。

然而，像**Oraclize**这样的集中式互操作性项目已解决了信任问题。Oraclize 使您的以太坊智能合约能够与万维网进行通信；也就是说，它使您能够进行 REST API 调用，获取比特币账户余额，检查 IPFS 中的文件状态等等。Oraclize 还为智能合约提供了证明，证明结果没有被篡改；因此，Oraclize 解决了信任问题，但单点故障仍然存在。Oraclize 适用于权限和公共以太坊。Oraclize 的主要成就是为以太坊智能合约提供了进行 REST API 调用的能力，但它的目的从未是提供区块链之间的互操作性，因此它不提供此功能。

如果信任对你不是问题的话，那么单一托管策略绝对是你应该考虑的一件事，因为这支持我们先前讨论的所有三种用例。 您可以轻松编写一个集中式应用程序，以响应一个区块链上的事件并在另一个区块链上调用操作。 在得到许可的区块链中，通常有监管机构或权威机构可以选择承载该集中式应用程序。 单一托管方应发送使用预先编程为信任的区块链签名的交易。

# 多重签名联盟

在区块链之间实现互操作性的更好选择是由一组公证人（或权威）控制多重签名，其中大多数公证人必须批准某项操作才能进行。 这种设置比拥有单一托管方更好，但仍然集中控制。 要实现真正的去中心化，应谨慎选择公证人，至少具有以下特性：

+   公证人的数量不应低—例如，至少 10 个。

+   公证人的数量不应太高—例如，少于 30 个，这样用户可以验证公证人的真实性和诚实度。

+   公证人应分布在不同的法律司法管辖区和国家，以防止国家攻击、胁迫和审查。

+   为了防止自然灾害发生时基础设施的故障，公证人应该在地理上分布。

+   公证人应是有名望的。

+   公证人不应受（或依赖于）较低数量的实体控制。 例如，公证人不能是同一银行的不同分支。

+   公证人应通过物理和逻辑保护以及所需的安全程序实现和保持指定级别的安全性。

# 侧链或中继

侧链是一种在一个区块链内部的系统，可以验证和读取其他区块链中的事件和/或状态。 中继是一种更直接的促进互操作性的方法，而不是依赖于可信中介向另一个区块链提供信息，区块链有效地承担了这一任务。 目前，使用**Merkle 树**来实现侧链系统。

通用的方法如下。假设在区块链 *B* 上执行的智能合约想要了解区块链 *A* 上是否发生了某个特定事件，或者在区块链 *A* 的状态中的某个特定对象在某个特定时间是否包含了某个值。我们可以在区块链 *B* 上创建一个合约，该合约接受区块链 *A* 的其中一个区块头，并使用区块链 *A* 共识算法的标准验证过程来验证这个区块头——在 IBFT 中，这将涉及验证超过 75% 的验证者签名已经签署了区块头。一旦中继验证了区块头已经被确定，中继就可以验证任何所需的交易或账户/状态条目，通过对梅克尔树的单个分支进行区块头验证。

所谓的 **轻客户端验证** 技术的应用对中继而言是理想的，因为区块链在资源上基本上是受限的。事实上，在同一时间，内部机制无法完全验证区块链 *A* 并使内部机制完全验证区块链 *B*，因为数学上简单的原因是两个盒子无法同时包含彼此：*A* 需要重新运行重新运行 *A* 的那部分部分，包括重新运行 *B* 的那部分 *A*，等等。然而，通过轻客户端验证，一个协议，其中区块链 *A* 包含区块链 *B* 的小片段，并且区块链 *B* 包含区块链 *A* 的小片段，这些片段是按需拉取的，是完全可行的。在区块链 *B* 上的一个中继的智能合约想要验证区块链 *A* 上的特定交易、事件或状态信息时，就像传统的轻客户端一样，会验证区块链 *A* 的加密哈希树的一个分支，然后通过区块头验证这个分支的根是否在内部，如果这两个检查通过，它会接受该交易、事件或状态信息是正确的。

注意，由于区块链是完全自包含的环境，并且没有自然访问外部世界的能力，因此链 *A* 的相关数据需要由用户输入到链 *B* 中；但是，因为数据在密码学意义上是自验证的，输入这些信息的用户无需受信任。

# 哈希锁定

哈希锁定是一种实现资产的原子交换的技术。它不需要任何中介。哈希锁定的工作原理如下：

+   假设在两个不同的区块链中有两个名为 *A* 和 *B* 的资产。资产 *A* 的所有者是 *X*，资产 *B* 的所有者是 *Y*。

+   如果他们两个都想交换这些资产，那么首先，*X* 必须生成一个秘密值 *S*，并计算秘密值的哈希 *H*。之后，*X* 与 *Y* 共享 *H*。

+   现在，*X* 锁定资产 *A*，声明如果 *Y* 在 *N* 秒内揭示 *H* 的 *S*，则资产的所有权将转移到 *Y*；否则，资产将在 *N* 秒后解锁。

+   接下来，*Y* 锁定资产 *B*，声明如果 *X* 在 *N/2* 秒内揭示 *H* 的 *S*，则资产的所有权将转移给 *X*；否则，资产将在 *N/2* 秒后解锁。

+   所以现在，资产 *A* 和 *B* 在分别 *N* 秒和 *N/2* 秒后被锁定。现在，在 *N/2* 秒内，*X* 向 *B* 的区块链揭示 *S* 以主张资产的所有权。现在，*Y* 有同等的时间来了解 *S* 并向区块链 *A* 揭示 *S* 以主张所有权。

*X* 得到与 *Y* 给予的时间的一半的原因是只有 *X* 知道密码，而 *Y* 锁定资金后，*X* 可以等到 *N* 秒快结束时主张资产，这将不给 *Y* 足够的时间来主张他们的资金。因此，*X* 可以成功地窃取资产 *A* 和 *B*。为了避免这种情况，我们给予 *Y* *N* 秒和 *X* *N/2* 秒，以便 *Y* 有与 *X* 相同的时间来主张资产。

这种技术的缺点是，如果 *X* 在 *N/2* 秒和 *N* 秒之间将 *S* 揭示给区块链 *B*，那么 *X* 将无法主张对 *B* 的所有权，但 *Y* 将了解 *S* 并有时间主张对 *A* 的所有权。然而，这将是 *X* 的错，可以避免。

# 创建 FedCoin

FedCoin 是由中央银行发行的数字货币，与其法定货币一比一进行对冲。使用区块链数字化法定货币有几个好处，例如可以实现便捷的跨境支付，节省了对账工作等。

让我们在两个不同的区块链网络上构建一些数字化的印度卢比和美元。然后，让我们创建一些原子交换合约，以实现这些货币在银行之间的原子交换。这个用例需要您创建两个不同的 Quorum 网络，使用 IBFT 共识。在每个网络中有一个权威，即中央银行，以及 *N* 个同行，即其他银行。因此，您可以假设在第一个网络中，**美联储系统**（**FRS**）是权威，**美国银行**（**BOA**）和 ICICI 银行是同行。同样，在第二个网络中，**印度储备银行**（**RBI**）是权威，BOA 和 ICICI 银行是同行。

您现在不必构建此网络，因为在构建和测试智能合约时，您只能使用具有四个以太坊账户地址的一个节点。这足以模拟整个场景。

# 用于数字化法定货币的智能合约

这里是一个基本的智能合约，用于在区块链上创建数字化美元。这个智能合约允许我们发行和转移数字化货币：

```
pragma solidity ⁰.4.19;

contract USD {

    mapping (address => uint) balances;
    mapping (address => mapping (address => uint)) allowed;
    address owner;

    function USD() {
        owner = msg.sender;
    }

    function issueUSD(address to, uint amount) {
        if(msg.sender == owner) {
            balances[to] += amount;
        } 
    }

    function transferUSD(address to, uint amount) {
        if(balances[msg.sender] >= amount) {
            balances[msg.sender] -= amount;
            balances[to] += amount;
        }
    }

    function getUSDBalance(address account) view returns 
      (uint balance) {
        return balances[account];
    }

    function approve(address spender, uint amount) {
        allowed[spender][msg.sender] = amount;
    }

    function transferUSDFrom(address from, address to, uint amount) {
        if(allowed[msg.sender][from] >= amount && balances[from]
          >= amount) {
            allowed[msg.sender][from] -= amount;
            balances[from] -= amount;
            balances[to] += amount;
        }
    }
}
```

这是上述代码的工作原理：

+   首先，我们定义了一个映射来存储每家银行持有的美元数量。每家银行可以有多个地址以实现隐私。这些地址不一定是银行；它们也可以是其他智能合约，因为每个智能合约也有`address`。

+   接下来，我们假设中央银行部署了合约。因此，我们将中央银行定义为发行方，通过将其`address`分配给`owner`。

+   然后，我们定义了一个名为`issueUSD`的函数，中央银行可以利用它向其他银行发行美元。

+   然后，我们定义了另一个名为`transferUSD`的函数，银行可以利用它在彼此之间转移美元。

+   接下来，我们有一个函数用于读取账户的余额。

+   最后，我们有两个重要的函数：`approve`和`transferUSDFrom`。`transferUSDFrom`函数允许合约代表您发送美元。换句话说，您为同一区块链上的其他智能合约提供了管理您资金的 API。`approve`函数用于为智能合约提供管理您资金的批准。在调用`approve`时，您指定了该合约可以管理多少您的资金。

在这里，我们正在使用一个名为`view`的内置修饰符。`view`表示该函数无法修改存储，但将读取存储（因此查看）。`view`函数无法发送或接收以太币。类似地，还有另一个名为`pure`的修饰符，表示返回值只能依赖于输入参数，即它们甚至不能读取存储，也不能发送或接收以太币。您应该使用这些修饰符，因为它们具有多种好处——例如，在生成用于与合约交互的 UI 表单时，Remix IDE 会寻找这些修饰符。

现在在第二个网络中部署一个类似的合约以数字化印度卢比。在前述合约中用`INR`替换`USD`并部署它。应该是这样的：

```
pragma solidity ⁰.4.19;

contract INR {

    mapping (address => uint) balances;
    mapping (address => mapping (address => uint)) allowed;
    address owner;

    function INR() {
        owner = msg.sender;
    }

    function issueINR(address to, uint amount) {
        if(msg.sender == owner) {
            balances[to] += amount;
        } 
    }

    function transferINR(address to, uint amount) {
        if(balances[msg.sender] >= amount) {
            balances[msg.sender] -= amount;
            balances[to] += amount;
        }
    }

    function getINRBalance(address account) view returns 
      (uint balance) {
        return balances[account];
    }

    function approve(address spender, uint amount) {
        allowed[spender][msg.sender] = amount;
    }

    function transferINRFrom(address from, address to, uint amount) {
        if(allowed[msg.sender][from] >= amount && balances[from] 
          >= amount) {
            allowed[msg.sender][from] -= amount;
            balances[from] -= amount;
            balances[to] += amount;
        }
    }
}
```

# 原子交换智能合约

我们已成功数字化了法定货币。现在是实现哈希锁定机制的原子交换智能合约的时候了。我们在每个区块链上都有一个原子交换智能合约部署——也就是说，第一个区块链上的原子交换智能合约将美元锁定一段时间，并期望印度银行（这里是 ICICI 银行）在规定的时间内使用密钥来认领它。同样，第二个区块链上的原子交换合约将在规定的时间内锁定印度卢比，并期望美国银行（这里是 BOA）在规定的时间内使用密钥来认领它。

以下是用于锁定美元的原子交换智能合约：

```
pragma solidity ⁰.4.19;

import "./USD.sol";

contract AtomicSwap_USD {

    struct AtomicTxn {
        address from;
        address to;
        uint lockPeriod;
        uint amount;
    }

    mapping (bytes32 => AtomicTxn) txns;
    USD USDContract;

    event usdLocked(address to, bytes32 hash, uint expiryTime, 
      uint amount);
    event usdUnlocked(bytes32 hash);
    event usdClaimed(string secret, address from, bytes32 hash);

    function AtomicSwap_USD(address usdContractAddress) {
        USDContract = USD(usdContractAddress); 
    }

    function lock(address to, bytes32 hash, uint lockExpiryMinutes,
      uint amount) {
        USDContract.transferUSDFrom(msg.sender, address(this), amount);
        txns[hash] = AtomicTxn(msg.sender, to, block.timestamp + 
         (lockExpiryMinutes * 60), amount);
        usdLocked(to, hash, block.timestamp + (lockExpiryMinutes * 60),
          amount);
    }

    function unlock(bytes32 hash) {
        if(txns[hash].lockPeriod < block.timestamp) {
            USDContract.transferUSD(txns[hash].from, 
              txns[hash].amount);
            usdUnlocked(hash);
        }
    }

    function claim(string secret) {
        bytes32 hash = sha256(secret);
        USDContract.transferUSD(txns[hash].to, txns[hash].amount);
        usdClaimed(secret, txns[hash].from, hash);
    }

    function calculateHash(string secret) returns (bytes32 result) {
        return sha256(secret);
    }
}
```

以下是前述智能合约的工作原理：

+   部署智能合约时，我们提供了`USD`合约的合约地址，以便它调用其函数来转移资金。

+   `lock`方法用于使用`hash`锁定资金。显然，在调用`lock`方法之前，BOA 必须批准此原子互换合约地址，以便能够访问其一定数量的资金。它接受`hash`并锁定资金一段时间。 `amount`被指定为指示锁定多少美元，此金额应小于等于批准的金额。 `to`地址指定印度银行的地址—即 ICICI 银行。因此，当 ICICI 银行来索取资金时，它们将去到此地址。该函数实际上将资金转移到其合约地址（即`address(this)`）并触发事件，以便 ICICI 银行可以看到资金已被锁定。

+   `unlock`方法可以被 BOA 用来在`hash`过期后解锁资金，如果资金没有被索取。

+   `claim`方法由 ICICI 银行使用秘密索取资金。

+   最后，我们使用`calculateHash`方法来计算秘密的`hash`。

这里，我们以 BOA 和 ICICI 为例来简单解释，但之前的智能合约可以与任意数量的货币和银行配合良好运行。

在前述合同中将`USD`更改为`INR`，以为第二个区块链提供原子交换智能合约。以下是代码的外观：

```
pragma solidity ⁰.4.19;

import "./INR.sol";

contract AtomicSwap_INR {

    struct AtomicTxn {
        address from;
        address to;
        uint lockPeriod;
        uint amount;
    }

    mapping (bytes32 => AtomicTxn) txns;
    INR INRContract;

    event inrLocked(address to, bytes32 hash, uint expiryTime,
      uint amount);
    event inrUnlocked(bytes32 hash);
    event inrClaimed(string secret, address from, bytes32 hash);

    function AtomicSwap_INR(address inrContractAddress) {
        INRContract = INR(inrContractAddress); 
    }

    function lock(address to, bytes32 hash, uint lockExpiryMinutes, 
      uint amount) {
        INRContract.transferINRFrom(msg.sender, address(this), amount);
        txns[hash] = AtomicTxn(msg.sender, to, block.timestamp + 
         (lockExpiryMinutes * 60), amount);
        inrLocked(to, hash, block.timestamp + (lockExpiryMinutes * 60), 
          amount);
    }

    function unlock(bytes32 hash) {
        if(txns[hash].lockPeriod < block.timestamp) {
            INRContract.transferINR(txns[hash].from, 
              txns[hash].amount);
            inrUnlocked(hash);
        }
    }

    function claim(string secret) {
        bytes32 hash = sha256(secret);
        INRContract.transferINR(txns[hash].to, txns[hash].amount);
        inrClaimed(secret, txns[hash].from, hash);
    }

    function calculateHash(string secret) returns (bytes32 result) {
        return sha256(secret);
    }
}
```

# 测试

现在我们已经准备好在两个不同区块链的资产之间进行原子交换的智能合约了。接下来，让我们编写一些 JavaScript 代码来测试前面的合约并进行原子交换。以下代码允许您执行此操作。为了测试和模拟目的，您可以在一个单独的 Quorum 节点上运行以下代码，该节点具有四个帐户：

```
var generateSecret = function () {
    return Math.random().toString(36).substr(2, 9);
};

var web3 = new Web3(new Web3.providers.HttpProvider("http://localhost:8545"));

var RBI_Address = "0x92764a01c43ca175c0d2de145947d6387205c655";
var FRS_Address = "0xbc37e7ba9f099ba8c61532c6fce157072798fe77";
var BOA_Address = "0x104803ea6d8696afa6e7a284a46a1e71553fcf12";
var ICICI_Address = "0x84d2dab0d783dd84c40d04692e303b19fa49bf47";

var usdContract_ABI = /* Put JSON here */;
var usdContract_Bytecode = "0x606..."
var atomicswapUSD_ABI = /* Put JSON here */;
var atomicswapUSD_Bytecode = "0x606..."
var inrContract_ABI = /* Put JSON here */;
var inrContract_Bytecode = "0x606..."
var atomicswapINR_ABI = /* Put JSON here */;
var atomicswapINR_Bytecode = "0x606..."

var usdContract = web3.eth.contract(usdContract_ABI);
var usd = usdContract.new({
  from: FRS_Address, 
   data: usdContract_Bytecode, 
   gas: "4700000"
}, function (e, contract){
  if (typeof contract.address !== 'undefined') {
    var usdContractAddress = contract.address;
    var usdContractInstance = usdContract.at(usdContractAddress)
    var atomicswap_usdContract = web3.eth.contract(atomicswapUSD_ABI);
    var atomicswap_usd = atomicswap_usdContract.new(usdContractAddress, {
        from: FRS_Address, 
        data: atomicswapUSD_Bytecode, 
        gas: "4700000"
    }, function (e, contract){
        if (typeof contract.address !== 'undefined') {
            var atomicSwapUSDAddress = contract.address;
            var atomicSwapUSDContractInstance =
              atomicswap_usdContract.at(atomicSwapUSDAddress);

            var inrContract = web3.eth.contract(inrContract_ABI);

        var inr = inrContract.new({
            from: RBI_Address, 
            data: inrContract_Bytecode, 
            gas: "4700000"
        }, function (e, contract){
            if(typeof contract.address !== 'undefined') {
                var inrContractAddress = contract.address;
                var inrContractInstance = 
                  inrContract.at(inrContractAddress)
            var atomicswap_inrContract =
              web3.eth.contract(atomicswapINR_ABI);
            var atomicswap_inr = atomicswap_inrContract.new(
                inrContractAddress, {
                from: RBI_Address, 
                data: atomicswapINR_Bytecode, 
                gas: '4700000'
            }, function (e, contract){
                if (typeof contract.address !== 'undefined') {
                    var atomicSwapINRAddress = contract.address;
                    var atomicSwapINRContractInstance = 
                      atomicswap_inrContract.at(atomicSwapINRAddress);

                }
            })
            }
        })
        }
    })
  }
})
```

首先，我们部署了`USD`合约，然后通过将`USD`合约的地址作为参数来部署了 USD 的原子交换合约。我们将这些合约部署为 FRS。然后，我们部署了`INR`合约，然后通过将`INR`合约的地址作为参数来部署了 INR 的原子交换合约。我们将这些合约部署为 RBI。

将以下代码放在提到的连续位置：

```
//Issue USD
usdContractInstance.issueUSD.sendTransaction(BOA_Address, 1000,
  {from: FRS_Address}, function(e, txnHash){

  //Fetch USD Balance
  console.log("Bank of America's USD Balance is : " + 
    usdContractInstance.getUSDBalance.call(BOA_Address).toString())

  //Issue INR
  inrContractInstance.issueINR.sendTransaction(ICICI_Address, 1000,
   {from: RBI_Address}, function(e, txnHash){

    //Fetch INR Balance
    console.log("ICICI Bank's INR Balance is : " + 
      inrContractInstance.getINRBalance.call(ICICI_Address).toString())

    //Generate Secret and Hash
    var secret = generateSecret();
    var hash = atomicSwapUSDContractInstance.calculateHash.call(secret,
      {from: BOA_Address});

    //Give Access to Smart Contract
    usdContractInstance.approve.sendTransaction(atomicSwapUSDAddress,
      1000, {from: BOA_Address}, function(e, txnHash){

      //Give Access to Smart Contract
      inrContractInstance.approve.sendTransaction(atomicSwapINRAddress,
        1000, {from: ICICI_Address}, function(e, txnHash){

        //Lock 1000 USD for 30 min
        atomicSwapUSDContractInstance.lock.sendTransaction(ICICI_Address, hash, 
  30, 1000, {from: BOA_Address, gas: 4712388}, function(e, txnHash){

          //Fetch USD Balance
          console.log("USD Atomic Exchange Smart Contracts holds : " + 
            usdContractInstance.getUSDBalance.call
            (atomicSwapUSDAddress).toString())

          //Lock 1000 INR for 15 min
          atomicSwapINRContractInstance.lock.sendTransaction(BOA_Address,
  hash, 15, 1000, {from: ICICI_Address, gas: 4712388},
  function(e, txnHash){

            //Fetch INR Balance
            console.log("INR Atomic Exchange Smart Contracts holds : "
              + inrContractInstance.getINRBalance.call
              (atomicSwapINRAddress).toString())

            atomicSwapINRContractInstance.claim(secret, {
              from: BOA_Address, gas: 4712388
            }, function(error, txnHash){

              //Fetch INR Balance
              console.log("Bank of America's INR Balance is : " +
                inrContractInstance.getINRBalance.call
                (BOA_Address).toString())

              atomicSwapUSDContractInstance.claim(secret, {
                from: ICICI_Address, gas: 4712388
              }, function(error, txnHash){

                //Fetch USD Balance
                console.log("ICICI Bank's USD Balance is : " +
                  usdContractInstance.getUSDBalance.call
                  (ICICI_Address).toString())
              })

            })

          })
        })
      })

    })
  })

}) 
```

以下是前面的代码如何运行的：

1.  这里，FRS 向 BOA 发行了 USD，然后 RBI 向 ICICI 银行发行了 INR。

1.  然后，BOA 生成了一个秘密。我们使用一个非常基本的函数来生成一个秘密。显然，在实际场景中，您应该使用某种基于硬件的工具来生成这些类型的安全秘密。

1.  接下来，我们计算了秘密的哈希。

1.  BOA 和 ICICI 银行分别授予了 USD 原子交换和 INR 原子交换合约对其资金的访问权限。

1.  BOA 将 USD 锁定在 USD 原子交换合约中，锁定时间为 30 分钟，并声明只有 ICICI 银行可以索取资金。

1.  同样，ICICI 银行将 INR 锁定在 INR 原子交换合约中，锁定时间为 15 分钟，并声明只有 BOA 可以索取资金。

1.  最后，BOA 前去索取 INR。一旦 ICICI 知道了秘密，它立即索取了 USD。

要测试上述合约，首先复制你的以太坊地址，并替换我在之前示例中生成的地址。然后确保在你的节点上解锁所有四个账户。最后，编译合约并填充`ABI`和`Bytecode`变量。

这里，我们使用 Solidity 函数来计算`hash`，但你也可以使用 JavaScript 来计算`hash`。如果你想计算`sha256`哈希值，那么你可以使用任何一个 JavaScript 库，但如果你想像 Solidity 在 JavaScript 中计算`sha3`（也就是`keccak256`）一样，那么你需要使用`web3.utils`库，它提供了一个名为`soliditySha3`的函数。这个函数会以与 Solidity 相同的方式计算给定输入参数的`sha3`。这意味着参数将被`ABI`转换和紧密打包后再进行哈希运算。

# 摘要

在本章中，我们探讨了构建可互操作的区块链的各种选项。总结起来，单一托管方、多签名联邦以及哈希锁定易于实现，而侧链则复杂且需要大量工程工作。很快，我们将拥有内置侧链支持的生产许可区块链平台。

最后，我们通过模拟两家中央和商业银行来实现了哈希锁定。你可以继续尝试构建两个不同的网络，并尝试进行原子交换。

在下一章中，我们将学习如何构建一个用于 Quorum 的区块链服务器。在构建过程中，我们还将学习 DevOps 和云计算的概念。
