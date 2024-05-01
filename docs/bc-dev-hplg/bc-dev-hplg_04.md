# 用 Golang 设计数据和交易模型

在超级账本 Fabric 中，链代码是由开发人员编写的智能合约的一种形式。链代码实现了区块链网络的利益相关者所达成一致的业务逻辑。该功能对客户端应用程序开放，以便它们调用，前提是它们具有正确的权限。

链代码作为一个独立的进程在自己的容器中运行，与 Fabric 网络的其他组件隔离开来。背书节点负责管理链代码和交易调用的生命周期。作为对客户端调用的回应，链代码查询和更新分类账，并生成交易提案。

在本章中，我们将学习如何用 Go 语言开发链代码，并实现场景的智能合约业务逻辑。最后，我们将探讨开发完全功能的链代码所必需的关键概念和库。

在接下来的章节中，我们将探索与概念相关的代码片段，你可以在以下地址找到链代码的完整实现：

[https://github.com/HyperledgerHandsOn/trade-finance-logistics/tree/master/chaincode/src/github.com/trade_workflow_v1](https://github.com/HyperledgerHandsOn/trade-finance-logistics/tree/master/chaincode/src/github.com/trade_workflow_v1)

请注意，这也可以在我们在上一章中创建的本地 git 克隆中找到。我们有两个版本的链代码，一个在`trade_workflow`文件夹中，另一个在`trade_workflow_v1`文件夹中。我们需要两个版本来演示稍后升级，在[*第 9 章*](72e0e4f7-a8e3-49e6-935f-2c478d0ac891.xhtml)，*区块链网络的生活*中。在本章中，我们使用`v1`版本来演示如何用 Go 编写链代码。

在本章中，我们将讨论以下主题：

+   创建链代码

+   访问控制

+   实现链代码函数

+   测试链代码

+   链代码设计主题

+   记录输出

# 启动链代码开发

在我们可以开始编写链代码之前，我们首先需要启动我们的开发环境。

设置开发环境的步骤已在[*第 3 章*](5a4b5cba-356c-4997-b816-0676a2c503c2.xhtml)，*使用商业情景设定舞台*中解释过。然而，我们现在继续启动开发模式下的 Fabric 网络。这种模式允许我们控制如何构建和运行链代码。我们将使用此网络在开发环境中运行我们的链代码。

下面是我们如何以开发模式启动 Fabric 网络的步骤：

```
$ cd $GOPATH/src/trade-finance-logistics/network
$ ./trade.sh up -d true  
```

如果在启动网络时遇到任何错误，可能是由一些残留的 Docker 容器引起的。

你可以通过停止网络使用`./trade.sh down -d true`，然后运行以下命令解决这个问题：`./trade.sh clean -d true`。

`-d` true 选项告诉我们的脚本在开发网络上采取行动。

我们的开发网络现在在五个Docker容器中运行。网络由单个订购者、在`devmode`中运行的单个对等体、一个链码容器、一个CA容器和一个CLI容器组成。CLI容器在启动时创建了一个名为`tradechannel`的区块链通道。我们将使用CLI与链码交互。

随时查看日志目录中的日志消息。它列出了网络启动期间执行的组件和函数。我们将保持终端开放，因为一旦安装并调用了链码，我们会在这里收到进一步的日志消息。

# 编译和运行链码

克隆源代码已经使用Go vendoring包含了所有的依赖关系。考虑到这一点，我们现在可以开始构建代码，并使用以下步骤运行链码：

1.  **编译链码**：在一个新的终端中，连接到链码容器并使用以下命令构建链码：

```
$ docker exec -it chaincode bash 
$ cd trade_workflow_v1 
$ go build 
```

1.  使用以下命令运行链码：

```
$ CORE_PEER_ADDRESS=peer:7052 CORE_CHAINCODE_ID_NAME=tw:0 ./trade_workflow_v1  
```

我们现在有一个连接到对等体的运行中的链码。这里的日志消息表明链码已经启动并正在运行。您还可以在网络终端中检查日志消息，其中列出了对等体上与链码的连接。

# 安装和实例化链码

在我们启动通道之前，现在需要在通道上安装链码，这将调用`Init`方法：

1.  **安装链码**：在一个新的终端中，连接到CLI容器并按如下方式安装名称为`tw`的链码：

```
$ docker exec -it cli bash 
$ peer chaincode install -p chaincodedev/chaincode/trade_workflow_v1 -n tw -v 0
```

1.  现在，实例化以下`链码`：

```
$ peer chaincode instantiate -n tw -v 0 -c '{"Args":["init","LumberInc","LumberBank","100000","WoodenToys","ToyBank","200000","UniversalFreight","ForestryDepartment"]}' -C tradechannel 
```

CLI连接的终端现在包含了与链码交互的日志消息列表。`链码`终端显示了来自`链码`方法调用的消息，而网络终端显示了对等体和订购者之间通信的消息。

# 调用链码

现在我们有一个正在运行的链码，我们可以开始调用一些函数。我们的链码有几种创建和检索资产的方法。目前，我们只会调用其中两个；第一个是创建一个新的交易协议，第二个是从分类账中检索它。要做到这一点，请完成以下步骤：

1.  使用以下命令在分类账上放置具有唯一ID`trade-12`的新交易协议：

```
$ peer chaincode invoke -n tw -c '{"Args":["requestTrade", "trade-12", "50000", "Wood for Toys"]}' -C tradechannel
```

1.  使用以下命令从分类账中检索ID为`trade-12`的交易协议：

```
$ peer chaincode invoke -n tw -c '{"Args":["getTradeStatus", "trade-12"]}' -C tradechannel
```

现在，我们在`devmode`中有一个正在运行的网络，我们已成功测试了我们的链码。在接下来的部分，我们将学习如何从头开始创建和测试链码。

开发模式

在生产环境中，链码的生命周期由对等体管理。当我们需要在开发环境中重复修改和测试链码时，我们可以使用`devmode`，它允许开发人员控制链码的生命周期。此外，`devmode`将`stdout`和`stderr`标准文件重定向到终端；这些在生产环境中是禁用的。

要使用 `devmode`，对等方必须连接到其他网络组件，就像在生产环境中一样，并且以参数 `peer-chaincodedev=true` 启动。然后，链代码将单独启动并配置为连接到对等方。在开发过程中，可以从终端重复编译、启动、调用和停止链代码。

在接下来的部分中，我们将使用启用了 `devmode` 的网络。

# 创建链代码

现在我们已经准备好开始实现我们的链代码了，我们将使用 Go 语言编程。有几个提供对 Go 的支持的 IDE 可用。其中一些较好的 IDE 包括 Atom、Visual Studio Code 等等。无论您选择的环境如何，都可以与我们的示例一起使用。

# 链代码接口

每个链代码都必须实现 `Chaincode interface`，其方法是在接收到交易提案后调用的。SHIM 包中定义的 `Chaincode interface` 如下所示：

```
type Chaincode interface { 
    Init(stub ChaincodeStubInterface) pb.Response 
    Invoke(stub ChaincodeStubInterface) pb.Response 
} 
```

正如你所看到的，`Chaincode` 类型定义了两个函数：`Init` 和 `Invoke`。

这两个函数都有一个参数，类型为 `ChaincodeStubInterface`，名为 `stub`。

`stub` 参数是我们在实现链代码功能时将使用的主要对象，因为它提供了访问和修改账本、获取调用参数等功能。

另外，SHIM 包提供了其他类型和函数来构建链代码；你可以在以下链接检查整个包：[https://godoc.org/github.com/hyperledger/fabric/core/chaincode/shim.](https://godoc.org/github.com/hyperledger/fabric/core/chaincode/shim)

# 设置链代码文件

现在让我们设置 `chaincode` 文件。

我们将使用从 GitHub 克隆的文件夹结构进行工作。链代码文件位于以下文件夹中：

```
$GOPATH/src/trade-finance-logistics/chaincode/src/github.com/trade_workflow_v1
```

您可以按照步骤检查文件夹中的代码文件，也可以创建一个新文件夹并按描述创建代码文件。

1.  首先，我们需要创建 `chaincode` 文件

在您喜欢的编辑器中，创建一个名为 `tradeWorkflow.go` 的文件，并包含以下包和导入语句：

```
package main

import (
    "fmt"
    "errors"
    "strconv"
    "strings"
    "encoding/json"
    "github.com/hyperledger/fabric/core/chaincode/shim"
    "github.com/hyperledger/fabric/core/chaincode/lib/cid"
    pb "github.com/hyperledger/fabric/protos/peer"
)
```

在前面的代码片段中，我们可以看到第 4 至 8 行导入了 Go 语言系统包，第 9 至 11 行导入了 `shim`、`cid` 和 `pb` Fabric 包。`pb` 包提供了对 peer `protobuf` 类型的定义，而 `cid` 则提供了访问控制函数。在访问控制部分，我们将更详细地了解 CID。

1.  现在我们需要定义 `Chaincode` 类型。让我们添加将实现链代码函数的 `TradeWorkflowChaincode` 类型，如下所示：

```
type TradeWorkflowChaincode struct {
    testMode bool
}
```

注意第 2 行中的 `testMode` 字段。我们将在测试期间使用此字段来绕过访问控制检查。

1.  `TradeWorkflowChaincode` 是实现 `shim.Chaincode` 接口所需的。必须实现接口的方法，以使 `TradeWorkflowChaincode` 成为 `shim` 包中 `Chaincode` 类型的有效实现。

1.  `Init` 方法在将链码安装到区块链网络后调用。每个认可节点只会执行一次，部署其自己的链码实例。此方法可用于初始化、引导和设置链码。`Init` 方法的默认实现如下代码片段所示。请注意，第 3 行中的方法向标准输出写入一行以报告其调用。第 4 行中，方法返回了对 `shim` 函数调用的结果。成功的参数值为 `nil` 表示成功执行并返回空结果，如下所示：

```
// TradeWorkflowChaincode implementation
func (t *TradeWorkflowChaincode) Init(stub SHIM.ChaincodeStubInterface)         pb.Response {
    fmt.Println("Initializing Trade Workflow")
    return shim.Success(nil)
}
```

调用链码方法必须返回 `pb.Response` 对象的一个实例。以下代码片段列出了两个来自 SHIM 包的辅助函数，用于创建响应对象。以下函数将响应序列化为 gRPC protobuf 消息：

```
// Creates a Response object with the Success status and with argument of a 'payload' to return
// if there is no value to return, the argument 'payload' should be set to 'nil'
func shim.Success(payload []byte)

// creates a Response object with the Error status and with an argument of a message of the error
func shim.Error(msg string)
```

1.  现在是移步到调用参数的时候了。在这里，该方法将使用 `stub.GetFunctionAndParameters` 函数检索调用的参数，并验证是否提供了预期数量的参数。`Init` 方法希望收到零个参数，因此将账本保持为原样。当 `Init` 函数被调用时，这是因为链码在账本上升级到新版本。当链码首次安装时，它希望收到包含参与者详情的八个参数，这些将被记录为初始状态。如果提供了不正确数量的参数，该方法将返回错误。验证参数的代码块如下：

```
_, args := stub.GetFunctionAndParameters()
var err error

// Upgrade Mode 1: leave ledger state as it was
if len(args) == 0 {
  return shim.Success(nil)
}

// Upgrade mode 2: change all the names and account balances
if len(args) != 8 {
 err = errors.New(fmt.Sprintf("Incorrect number of arguments. Expecting 8: {" +
             "Exporter, " +
             "Exporter's Bank, " +
             "Exporter's Account Balance, " +
             "Importer, " +
             "Importer's Bank, " +
             "Importer's Account Balance, " +
             "Carrier, " +
             "Regulatory Authority" +
             "}. Found %d", len(args)))
  return shim.Error(err.Error())
}
```

正如我们在上述代码片段中所见，当提供了包含参与者姓名和角色的预期参数数量时，该方法会验证和将参数转换为正确的数据类型，并将它们作为初始状态记录到账本中。

在下面的代码片段中，在第 2 和第 7 行，该方法将参数转换为整数。如果转换失败，它返回错误。在第 14 行，通过字符串常量构造了一个字符串数组。这里，我们引用了在 `chaincode` 文件夹中的 `constants.go` 文件中定义的词法常量，这些常量代表了初始值将被记录到账本中的键。最后，在第 16 行，对于每个常量，一个记录（资产）被写入到账本中。`stub.PutState` 函数记录了一个键值对到账本中。

请注意，账本上的数据以字节数组形式存储；我们想要存储在账本上的任何数据都必须首先转换为字节数组，正如您在以下代码片段中所见：

```
// Type checks
_, err = strconv.Atoi(string(args[2]))
if err != nil {
    fmt.Printf("Exporter's account balance must be an integer. Found %s\n", args[2])
    return shim.Error(err.Error())
}
_, err = strconv.Atoi(string(args[5]))
if err != nil {
    fmt.Printf("Importer's account balance must be an integer. Found %s\n", args[5])
    return shim.Error(err.Error())
}

// Map participant identities to their roles on the ledger
roleKeys := []string{ expKey, ebKey, expBalKey, impKey, ibKey, impBalKey, carKey, raKey }
for i, roleKey := range roleKeys {
    err = stub.PutState(roleKey, []byte(args[i]))
    if err != nil {
        fmt.Errorf("Error recording key %s: %s\n", roleKey, err.Error())
```

```
        return shim.Error(err.Error())
    }
}
```

# 调用方法

每当查询或修改区块链的状态时，都会调用`Invoke`方法。

所有对账本上持有的资产的**创建**、**读取**、**更新**和**删除**（**CRUD**）操作都由`Invoke`方法封装。

当事务由调用客户端创建时，将调用此方法。当查询状态时（即，检索一个或多个资产但不修改账本的状态时），上下文事务将在客户端接收到`Invoke`响应后被丢弃。一旦账本已被修改，修改将被记录到事务中。在接收到要记录在账本上的事务的响应后，客户端将提交该事务给一个排序服务。下面的代码片段显示了一个空的`Invoke`方法：

```
func (t *TradeWorkflowChaincode) Invoke(stub shim.ChaincodeStubInterface) pb.Response {
    fmt.Println("TradeWorkflow Invoke")
}
```

通常，链码的实现将包含多个查询和修改函数。如果这些函数非常简单，可以直接在`Invoke`方法的主体中实现。然而，更加优雅的解决方案是独立实现每个函数，然后从`Invoke`方法中调用它们。

SHIM API提供了几个函数来检索`Invoke`方法的调用参数。这些列在以下代码片段中。开发人员可以选择参数的含义和顺序；但是，通常情况下，`Invoke`方法的第一个参数是函数的名称，其后的参数是该函数的参数。

```
// Returns the first argument as the function name and the rest of the arguments as parameters in a string array.
// The client must pass only arguments of the type string.
func GetFunctionAndParameters() (string, []string)

// Returns all arguments as a single string array.
// The client must pass only arguments of the type string.
func GetStringArgs() []string

// Returns the arguments as an array of byte arrays.
func GetArgs() [][]byte

// Returns the arguments as a single byte array.
func GetArgsSlice() ([]byte, error)
```

在下面的代码片段中，第1行使用`stub.GetFunctionAndParameters`函数检索调用的参数。从第3行开始，一系列`if`条件将执行与参数一起传递到请求的函数（`requestTrade`、`acceptTrade`等）。每个这些函数都独立实现其功能。如果请求了一个不存在的函数，该方法将返回一个错误，指示请求的函数不存在，如第18行所示：

```
    function, args := stub.GetFunctionAndParameters()

    if function == "requestTrade" {
        // Importer requests a trade
        return t.requestTrade(stub, creatorOrg, creatorCertIssuer, args)
    } else if function == "acceptTrade" {
        // Exporter accepts a trade
        return t.acceptTrade(stub, creatorOrg, creatorCertIssuer, args)
    } else if function == "requestLC" {
        // Importer requests an L/C
        return t.requestLC(stub, creatorOrg, creatorCertIssuer, args)
    } else if function == "issueLC" {
        // Importer's Bank issues an L/C
        return t.issueLC(stub, creatorOrg, creatorCertIssuer, args)
    } else if function == "acceptLC" {
  ...

  return shim.Error("Invalid invoke function name")
```

如您所见，`Invoke`方法是放置任何需要用于提取和验证将由请求的函数使用的参数的共享代码的合适位置。在下一节中，我们将看看访问控制机制，并将一些共享访问控制代码放入`Invoke`方法中。

# 访问控制

在深入讨论`Chaincode`函数的实现之前，我们需要首先定义我们的访问控制机制。

安全且受权限限制的区块链的关键特点是访问控制。在Fabric中，**成员服务提供商**（**MSP**）在启用访问控制方面发挥了关键作用。Fabric网络的每个组织都可以有一个或多个MSP提供者。MSP实现为**证书颁发机构**（**Fabric CA**）。有关Fabric CA的更多信息，包括其文档，请访问：[https://hyperledger-fabric-ca.readthedocs.io/.](https://hyperledger-fabric-ca.readthedocs.io/)

Fabric CA为网络用户颁发**注册证书**（**ecerts**）。ecert代表用户的身份，并在用户提交给Fabric时用作签名交易。在调用交易之前，用户必须首先从Fabric CA注册并获得ecert。

Fabric支持一种**基于属性的访问控制**（**ABAC**）机制，链码可以使用该机制控制对其功能和数据的访问。ABAC允许链码根据与用户身份关联的属性做出访问控制决策。拥有ecert的用户也可以访问一系列附加属性（即，名称/值对）。

在调用期间，链码将提取属性并做出访问控制决策。我们将在即将到来的章节中更深入地了解ABAC机制。

# ABAC

在接下来的步骤中，我们将向您展示如何注册用户并创建具有属性的ecert。然后我们将在链码中检索用户身份和属性，以验证访问控制。然后我们将把这个功能集成到我们的教程链码中。

首先，我们必须使用Fabric CA注册一个新用户。在注册过程中，我们必须定义生成ecert后将使用的属性。通过运行命令`fabric-ca-client register`来注册用户。访问控制属性可以使用后缀`:ecert`添加。

# 注册用户

这些步骤仅供参考，无法执行。更多信息可以参考GitHub存储库[https://github.com/HyperledgerHandsOn/trade-finance-logistics/blob/master/chaincode/abac.md](https://github.com/HyperledgerHandsOn/trade-finance-logistics/blob/master/chaincode/abac.md)

现在让我们注册一个具有自定义属性名为`importer`和值为`true`的用户。请注意，属性的值可以是任何类型，并不仅限于布尔值，如下段所示：

```
fabric-ca-client register --id.name user1 --id.secret pwd1 --id.type user --id.affiliation ImporterOrgMSP --id.attrs 'importer=true:ecert'
```

前面的片段显示了注册具有属性`importer=true`的用户时的命令行。请注意，`id.secret`的值和其他参数取决于Fabric CA配置。

上述命令还可以一次定义多个默认属性，例如：`--id.attrs`和`importer=true:ecert,email=user1@gmail.com`。

以下表格包含用户注册期间使用的默认属性：

| **属性名称** | **命令行参数** | **属性值** |
| --- | --- | --- |
| hf.EnrollmentID | (automatic) | 身份的注册ID |
| hf.Type  | id.type  | 身份的类型 |
| hf.Affiliation  | id.affiliation  | 身份的从属关系 |

如果在ecert中需要任何先前的属性，则必须首先在用户注册命令中定义它们。例如，以下命令注册`user1`，其属性为`hf.Affiliation=ImporterOrgMSP`，该属性将默认复制到ecert中：

```
fabric-ca-client register --id.name user1 --id.secret pwd1 --id.type user --id.affiliation ImporterOrgMSP --id.attrs 'importer=true:ecert,hf.Affiliation=ImporterOrgMSP:ecert'
```

# 注册用户

在这里，我们将注册用户并创建ecert。`enrollment.attrs`定义了从用户注册中复制到ecert的属性。后缀opt定义了从注册中复制的这些属性中的哪些是可选的。如果一个或多个非可选属性在用户注册时未定义，则注册将失败。以下命令将注册一个带有属性`importer`的用户：

```
fabric-ca-client enroll -u http://user1:pwd1@localhost:7054 --enrollment.attrs "importer,email:opt"
```

# 在链码中检索用户身份和属性

在此步骤中，我们将在执行链码期间检索用户的身份。链码可用的ABAC功能由**客户端身份链码**（**CID**）库提供。

提交给链码的每个交易建议都携带着发起者的ecert – 提交交易的用户。链码通过导入CID库并调用带有参数`ChaincodeStubInterface`的库函数来访问ecert，即在`Init`和`Invoke`方法中都收到的参数`stub`。

链码可以使用证书来提取有关调用者的信息，包括：

+   调用者的ID

+   发出调用者证书的**成员服务提供商（MSP）**的唯一ID

+   证书的标准属性，如其域名、电子邮件等

+   存储在证书中与客户端身份相关的ecert属性

CID库提供的函数如下所示：

```
// Returns the ID associated with the invoking identity. 
// This ID is unique within the MSP (Fabric CA) which issued the identity, however, it is not guaranteed to be unique across all MSPs of the network. 
func GetID() (string, error) 

// Returns the unique ID of the MSP associated with the identity that submitted the transaction. 
// The combination of the MSPID and of the identity ID are guaranteed to be unique across the network. 
func GetMSPID() (string, error) 

// Returns the value of the ecert attribute named `attrName`. 
// If the ecert has the attribute, the `found` returns true and the `value` returns the value of the attribute. 
// If the ecert does not have the attribute, `found` returns false and `value` returns empty string. 
func GetAttributeValue(attrName string) (value string, found bool, err error) 

// The function verifies that the ecert has the attribute named `attrName` and that the attribute value equals to `attrValue`. 
// The function returns nil if there is a match, else, it returns error. 
func AssertAttributeValue(attrName, attrValue string) error 

// Returns the X509 identity certificate. 
// The certificate is an instance of a type Certificate from the library "crypto/x509". 
func GetX509Certificate() (*x509.Certificate, error)  
```

在以下的代码块中，我们定义了一个名为`getTxCreatorInfo`的函数，该函数获取调用者的基本身份信息。首先，我们必须导入CID和x509库，如第3和第4行所示。第13行检索到唯一的MSPID，第19行获取了X509证书。然后在第24行，我们检索证书的`CommonName`，其中包含网络中Fabric CA的唯一字符串。这两个属性由该函数返回，并在后续的访问控制验证中使用，如以下片段所示：

```
import ( 
   "fmt" 
   "github.com/hyperledger/fabric/core/chaincode/shim" 
   "github.com/hyperledger/fabric/core/chaincode/lib/cid" 
   "crypto/x509" 
) 

func getTxCreatorInfo(stub shim.ChaincodeStubInterface) (string, string, error) { 
   var mspid string 
   var err error 
   var cert *x509.Certificate 

   mspid, err = cid.GetMSPID(stub) 
   if err != nil { 
         fmt.Printf("Error getting MSP identity: %sn", err.Error()) 
         return "", "", err 
   } 

   cert, err = cid.GetX509Certificate(stub) 
   if err != nil { 
         fmt.Printf("Error getting client certificate: %sn", err.Error()) 
         return "", "", err 
   } 

   return mspid, cert.Issuer.CommonName, nil 
}
```

现在，我们需要在链码中定义和实现简单的访问控制策略。链码的每个函数只能由特定组织的成员调用；因此，每个链码函数都将验证调用者是否是所需组织的成员。例如，函数`requestTrade`只能由`Importer`组织的成员调用。在下面的代码片段中，函数`authenticateImporterOrg`验证调用者是否是`ImporterOrgMSP`的成员。然后，将从`requestTrade`函数调用此函数以执行访问控制。

```
func authenticateExportingEntityOrg(mspID string, certCN string) bool {
    return (mspID == "ExportingEntityOrgMSP") && (certCN == "ca.exportingentityorg.trade.com")
}
func authenticateExporterOrg(mspID string, certCN string) bool {
return (mspID == "ExporterOrgMSP") && (certCN == "ca.exporterorg.trade.com")
}
func authenticateImporterOrg(mspID string, certCN string) bool {
    return (mspID == "ImporterOrgMSP") && (certCN == "ca.importerorg.trade.com")
}
func authenticateCarrierOrg(mspID string, certCN string) bool {
    return (mspID == "CarrierOrgMSP") && (certCN == "ca.carrierorg.trade.com")
}
func authenticateRegulatorOrg(mspID string, certCN string) bool {
    return (mspID == "RegulatorOrgMSP") && (certCN == "ca.regulatororg.trade.com")
}
```

下面的代码片段显示了访问控制验证的调用，该验证仅授予`ImporterOrgMSP`成员访问权限。该函数使用从`getTxCreatorInfo`函数获取的参数进行调用。

```
creatorOrg, creatorCertIssuer, err = getTxCreatorInfo(stub)
if !authenticateImporterOrg(creatorOrg, creatorCertIssuer) {
    return shim.Error("Caller not a member of Importer Org. Access denied.")
}
```

现在，我们需要将身份验证函数放入一个单独的文件`accessControlUtils.go`中，该文件位于与主`tradeWorkflow.go`文件相同的目录中。此文件将在编译期间自动导入到主`chaincode`文件中，因此我们可以引用其中定义的函数。

# 实现链码函数

到此为止，我们现在拥有链码的基本构建模块。我们有`Init`方法，用于初始化链码，以及`Invoke`方法，用于接收来自客户端和访问控制机制的请求。现在，我们需要定义链码的功能。

根据我们的场景，以下表格总结了记录和检索数据以及提供智能合约业务逻辑所需的函数列表。这些表格还定义了组织成员的访问控制定义，以便调用相应的函数。

以下表格说明了链码修改函数，即如何在分类账上记录交易：

| **函数名称** | **调用权限** | **描述** |
| --- | --- | --- |
| `requestTrade` | 进口商 | 请求贸易协议 |
| `acceptTrade` | 出口商 | 接受贸易协议 |
| `requestLC` | 进口商 | 请求信用证 |
| `issueLC` | 进口商 | 发行信用证 |
| `acceptLC` | 出口商 | 接受信用证 |
| `requestEL` | 出口商 | 请求出口许可证 |
| `issueEL` | 监管机构 | 发行出口许可证 |
| `prepareShipment` | 出口商 | 准备装运 |
| `acceptShipmentAndIssueBL` | 承运人 | 接受装运并发行提单 |
| `requestPayment` | 出口商 | 请求支付 |
| `makePayment` | 进口商 | 进行支付 |
| `updateShipmentLocation` | 承运人 | 更新装运位置 |

以下表格说明了链码查询函数，即从分类账中检索数据所需的函数：

| **函数名称** | **调用权限** | **描述** |
| --- | --- | --- |
| `getTradeStatus` | 出口商/出口实体/进口商 | 获取贸易协议的当前状态 |
| `getLCStatus` | 出口商/出口实体/进口商 | 获取信用证的当前状态 |
| `getELStatus` | 出口实体/监管机构 | 获取出口许可证的当前状态 |
| `getShipmentLocation` | 出口商/出口实体/进口商/承运人 | 获取货物当前位置 |
| `getBillOfLading` | 出口商/出口实体/进口商 | 获取提货单 |
| `getAccountBalance` | 出口商/出口实体/进口商 | 获取给定参与者的当前账户余额 |

# 定义链码资产

现在我们要定义资产的结构，这些资产将记录在账本上。在 Go 语言中，资产被定义为具有属性名和类型列表的结构类型。定义还需要包含 JSON 属性名，这些属性名将用于将资产序列化为 JSON 对象。在下面的片段中，您将看到我们应用程序中四个资产的定义。请注意，结构体的属性可以封装其他结构体，从而允许创建多级树。

```
type TradeAgreement struct { 
   Amount                    int               `json:"amount"` 
   DescriptionOfGoods        string            `json:"descriptionOfGoods"` 
   Status                    string            `json:"status"` 
   Payment                   int               `json:"payment"` 
} 

type LetterOfCredit struct { 
   Id                        string            `json:"id"` 
   ExpirationDate            string            `json:"expirationDate"` 
   Beneficiary               string            `json:"beneficiary"` 
   Amount                    int               `json:"amount"` 
   Documents                 []string          `json:"documents"` 
   Status                    string            `json:"status"` 
} 

type ExportLicense struct { 
   Id                        string            `json:"id"` 
   ExpirationDate            string            `json:"expirationDate"` 
   Exporter                  string            `json:"exporter"` 
   Carrier                   string            `json:"carrier"` 
   DescriptionOfGoods        string            `json:"descriptionOfGoods"` 
   Approver                  string            `json:"approver"` 
   Status                    string            `json:"status"` 
} 

type BillOfLading struct { 
   Id                        string            `json:"id"` 
   ExpirationDate            string            `json:"expirationDate"` 
   Exporter                  string            `json:"exporter"` 
   Carrier                   string            `json:"carrier"` 
   DescriptionOfGoods        string            `json:"descriptionOfGoods"` 
   Amount                    int               `json:"amount"` 
   Beneficiary               string            `json:"beneficiary"` 
   SourcePort                string            `json:"sourcePort"` 
   DestinationPort           string            `json:"destinationPort"` 
}  
```

# 编写链码函数

在本节中，我们将实现之前查看过的链码函数。为了实现链码函数，我们将使用三个 SHIM API 函数，这些函数将从 Worldstate 中读取资产并记录更改。正如我们已经学到的那样，这些函数的读取和写入分别记录到`ReadSet`和`WriteSet`中，而这些更改并不会立即影响账本的状态。只有在交易通过验证并被提交到账本之后，更改才会生效。

以下片段显示了一系列资产 API 函数：

```
// Returns the value of the `key` from the Worldstate. 
// If the key does not exist in the Worldstate the function returns (nil, nil). 
// The function does not read data from the WriteSet and hence uncommitted values modified by PutState are not returned. 
func GetState(key string) ([]byte, error) 

// Records the specified `key` and `value` into the WriteSet. 
// The function does not affect the ledger until the transaction is committed into the ledger. 
func PutState(key string, value []byte) error 

// Marks the the specified `key` as deleted in the WriteSet. 
// The key will be marked as deleted and removed from Worldstate once the transaction is committed into the ledger. 
func DelState(key string) error
```

# 创建资产

现在我们可以实现我们的第一个链码函数了，接下来我们将实现一个`requestTrade`函数，它将创建一个新的交易协议，状态为`REQUESTED`，然后记录该协议到账本上。

函数的实现如下所示。正如您将看到的，第 9 行我们验证调用者是否是`ImporterOrg`的成员，并且具有调用该函数的权限。从第 13 行到第 21 行，我们验证并提取参数。在第 23 行，我们创建一个以接收到的参数初始化的新的`TradeAgreement`实例。正如我们之前学到的，账本以字节数组的形式存储值。因此，在第 24 行我们将`TradeAgreement`序列化为 JSON 并转换为字节数组。在第 32 行，我们创建一个唯一的键，我们将存储`TradeAgreement`。最后，在第 37 行，我们使用键和序列化的`TradeAgreement`以及函数`PutState`将值存储到`WriteSet`中。

以下片段说明了`requestTrade`函数：

```
func (t *TradeWorkflowChaincode) requestTrade(stub shim.ChaincodeStubInterface, creatorOrg string, creatorCertIssuer string, args []string) pb.Response { 
   var tradeKey string 
   var tradeAgreement *TradeAgreement 
   var tradeAgreementBytes []byte 
   var amount int 
   var err error 

   // Access control: Only an Importer Org member can invoke this transaction 
   if !t.testMode && !authenticateImporterOrg(creatorOrg, creatorCertIssuer) { 
         return shim.Error("Caller not a member of Importer Org. Access denied.") 
   } 

   if len(args) != 3 { 
         err = errors.New(fmt.Sprintf("Incorrect number of arguments. Expecting 3: {ID, Amount, Description of Goods}. Found %d", len(args))) 
         return shim.Error(err.Error()) 
   } 

   amount, err = strconv.Atoi(string(args[1])) 
   if err != nil { 
         return shim.Error(err.Error()) 
   } 

   tradeAgreement = &TradeAgreement{amount, args[2], REQUESTED, 0} 
   tradeAgreementBytes, err = json.Marshal(tradeAgreement) 
   if err != nil { 
         return shim.Error("Error marshaling trade agreement structure") 
   } 

   // Write the state to the ledger 
   tradeKey, err = getTradeKey(stub, args[0]) 
   if err != nil { 
         return shim.Error(err.Error()) 
   } 
   err = stub.PutState(tradeKey, tradeAgreementBytes) 
   if err != nil { 
         return shim.Error(err.Error()) 
   } 
   fmt.Printf("Trade %s request recorded", args[0]) 

   return shim.Success(nil) 
}  
```

# 读取和修改资产

当我们实现了创建交易协议的函数之后，我们需要实现一个函数来接受交易协议。该函数将检索协议，将其状态修改为`ACCEPTED`，并将其放回到账本上。

此函数的实现如下所示。在代码中，我们构造了我们想要检索的贸易协议的唯一的复合键。在第 22 行，我们使用`GetState`函数检索值。在第 33 行，我们将字节数组反序列化为`TradeAgreement`结构的实例。在第 41 行，我们修改状态，使其为`ACCEPTED`；最后，在第 47 行，我们将更新后的值存储在分类账上，如下所示：

```
func (t *TradeWorkflowChaincode) acceptTrade(stub shim.ChaincodeStubInterface, creatorOrg string, creatorCertIssuer string, args []string) pb.Response { 
   var tradeKey string 
   var tradeAgreement *TradeAgreement 
   var tradeAgreementBytes []byte 
   var err error 

   // Access control: Only an Exporting Entity Org member can invoke this transaction 
   if !t.testMode && !authenticateExportingEntityOrg(creatorOrg, creatorCertIssuer) { 
         return shim.Error("Caller not a member of Exporting Entity Org. Access denied.") 
   } 

   if len(args) != 1 { 
         err = errors.New(fmt.Sprintf("Incorrect number of arguments. Expecting 1: {ID}. Found %d", len(args))) 
         return shim.Error(err.Error()) 
   } 

   // Get the state from the ledger 
   tradeKey, err = getTradeKey(stub, args[0]) 
   if err != nil { 
         return shim.Error(err.Error()) 
   } 
   tradeAgreementBytes, err = stub.GetState(tradeKey) 
   if err != nil { 
         return shim.Error(err.Error()) 
   } 

   if len(tradeAgreementBytes) == 0 { 
         err = errors.New(fmt.Sprintf("No record found for trade ID %s", args[0])) 
         return shim.Error(err.Error()) 
   } 

   // Unmarshal the JSON 
   err = json.Unmarshal(tradeAgreementBytes, &tradeAgreement) 
   if err != nil { 
         return shim.Error(err.Error()) 
   } 

   if tradeAgreement.Status == ACCEPTED { 
         fmt.Printf("Trade %s already accepted", args[0]) 
   } else { 
         tradeAgreement.Status = ACCEPTED 
         tradeAgreementBytes, err = json.Marshal(tradeAgreement) 
         if err != nil { 
               return shim.Error("Error marshaling trade agreement structure") 
         } 
         // Write the state to the ledger 
         err = stub.PutState(tradeKey, tradeAgreementBytes) 
         if err != nil { 
               return shim.Error(err.Error()) 
         } 
   } 
   fmt.Printf("Trade %s acceptance recordedn", args[0]) 

   return shim.Success(nil) 
}  
```

# 主函数

最后但并非最不重要的是，我们将添加`main`函数：Go 程序的初始点。当链码的实例部署在 peer 上时，会执行`main`函数以启动链码。

在下面片段的第 2 行中，实例化了链码。 函数`shim.Start`在第 4 行启动了链码，并向 peer 注册了链码，如下所示：

```
func main() { 
   twc := new(TradeWorkflowChaincode) 
   twc.testMode = false 
   err := shim.Start(twc) 
   if err != nil { 
         fmt.Printf("Error starting Trade Workflow chaincode: %s", err) 
   } 
} 
```

# 测试链码

现在我们可以为我们的链码函数编写单元测试，我们将使用内置的自动化 Go 测试框架。有关更多信息和文档，请访问 Go 的官方网站：[https://golang.org/pkg/testing/](https://golang.org/pkg/testing/)

框架自动寻找并执行以下签名的函数：

```
 func TestFname(*testing.T)
```

函数名`Fname`是一个任意的名称，必须以大写字母开头。

请注意，包含单元测试的测试套件文件必须以后缀`_test.go`结束；因此，我们的测试套件文件将被命名为`tradeWorkflow_test.go`，并放置在与我们的`chaincode`文件相同的目录中。`test`函数的第一个参数是类型为`T`的，它提供了用于管理测试状态并支持格式化测试日志的函数。测试的输出被写入标准输出，可以在终端中检查。

# SHIM 模拟

SHIM 包提供了一个全面的模拟模型，可用于测试链码。在我们的单元测试中，我们将使用`MockStub`类型，它为单元测试链码提供了`ChaincodeStubInterface`的实现。

# 测试`Init`方法

首先，我们需要定义一个函数，用于调用`Init`方法。该函数将接收对`MockStub`的引用，以及一个传递给`Init`方法的参数数组。在以下代码的第 2 行，使用接收到的参数调用链码函数`Init`，然后在第 3 行进行验证。

以下代码片段演示了`Init`方法的调用：

```
 func checkInit(t *testing.T, stub *shim.MockStub, args [][]byte) { 
   res := stub.MockInit("1", args) 
   if res.Status != shim.OK { 
         fmt.Println("Init failed", string(res.Message)) 
         t.FailNow() 
   } 
} 
```

我们将定义一个函数，用于准备`Init`函数参数的默认值数组，如下所示：

```
func getInitArguments() [][]byte { 
   return [][]byte{[]byte("init"), 
               []byte("LumberInc"), 
               []byte("LumberBank"), 
               []byte("100000"), 
               []byte("WoodenToys"), 
               []byte("ToyBank"), 
               []byte("200000"),
```

```
               []byte("UniversalFreight"), 
               []byte("ForestryDepartment")} 
} 
```

现在我们将定义`Init`函数的测试，如下所示。测试首先创建链码的一个实例，然后将模式设置为测试，最后为链码创建一个新的`MockStub`。在第 7 行，调用`checkInit`函数并执行`Init`函数。最后，从第 9 行开始，我们将验证分类账的状态，如下所示：

```
func TestTradeWorkflow_Init(t *testing.T) { 
   scc := new(TradeWorkflowChaincode) 
   scc.testMode = true 
   stub := shim.NewMockStub("Trade Workflow", scc) 

   // Init 
   checkInit(t, stub, getInitArguments()) 

   checkState(t, stub, "Exporter", EXPORTER) 
   checkState(t, stub, "ExportersBank", EXPBANK) 
   checkState(t, stub, "ExportersAccountBalance", strconv.Itoa(EXPBALANCE)) 
   checkState(t, stub, "Importer", IMPORTER) 
   checkState(t, stub, "ImportersBank", IMPBANK) 
   checkState(t, stub, "ImportersAccountBalance", strconv.Itoa(IMPBALANCE)) 
   checkState(t, stub, "Carrier", CARRIER) 
   checkState(t, stub, "RegulatoryAuthority", REGAUTH) 
}
```

接下来，我们将通过`checkState`函数验证每个键的状态是否符合预期，如以下代码块所示：

```
func checkState(t *testing.T, stub *shim.MockStub, name string, value string) { 
  bytes := stub.State[name] 
  if bytes == nil { 
    fmt.Println("State", name, "failed to get value") 
    t.FailNow() 
  } 
  if string(bytes) != value {
    fmt.Println("State value", name, "was", string(bytes), "and not", value, "as expected")
    t.FailNow()
  }
} 
```

# 测试调用方法

现在是定义`Invoke`函数的测试的时候了。在以下代码块的第7行，调用`checkInit`来初始化总账，然后在第13行调用`checkInvoke`，调用`requestTrade`函数。`requestTrade`函数创建一个新的贸易资产并将其存储在总账上。在第15和16行创建并序列化一个新的`TradeAgreement`，然后在第17行计算一个新的复合键。最后，在第18行，验证键的状态是否与序列化的值相匹配。

此外，正如前面所述，我们的链码包含一系列函数，这些函数一起定义了贸易工作流程。我们将在测试中将这些函数的调用链接成一个序列，以验证整个工作流程。整个函数的代码可以在位于`chaincode`文件夹中的测试文件中找到。

```
func TestTradeWorkflow_Agreement(t *testing.T) { 
   scc := new(TradeWorkflowChaincode) 
   scc.testMode = true 
   stub := shim.NewMockStub("Trade Workflow", scc) 

   // Init 
   checkInit(t, stub, getInitArguments()) 

   // Invoke 'requestTrade' 
   tradeID := "2ks89j9" 
   amount := 50000 
   descGoods := "Wood for Toys" 
   checkInvoke(t, stub, [][]byte{[]byte("requestTrade"), []byte(tradeID), []byte(strconv.Itoa(amount)), []byte(descGoods)}) 

   tradeAgreement := &TradeAgreement{amount, descGoods, REQUESTED, 0} 
   tradeAgreementBytes, _ := json.Marshal(tradeAgreement) 
   tradeKey, _ := stub.CreateCompositeKey("Trade", []string{tradeID}) 
   checkState(t, stub, tradeKey, string(tradeAgreementBytes)) 
   ... 
}
```

下面的代码段显示了`checkInvoke`函数。

```
func checkInvoke(t *testing.T, stub *shim.MockStub, args [][]byte) { 
   res := stub.MockInvoke("1", args) 
   if res.Status != shim.OK { 
         fmt.Println("Invoke", args, "failed", string(res.Message)) 
         t.FailNow() 
   } 
}
```

# 运行测试

现在我们准备好运行我们的测试了！`go test`命令将执行在`tradeWorkflow_test.go`文件中找到的所有测试。该文件包含一系列测试，验证了我们工作流中定义的函数。

现在让我们使用以下命令在终端中运行测试：

```
$ cd $GOPATH/src/trade-finance-logistics/chaincode/src/github.com/trade_workflow_v1 
$ go test 
```

前面的命令应该生成以下输出：

```
Initializing Trade Workflow 
Exporter: LumberInc 
Exporter's Bank: LumberBank 
Exporter's Account Balance: 100000 
Importer: WoodenToys 
Importer's Bank: ToyBank 
Importer's Account Balance: 200000 
Carrier: UniversalFreight 
Regulatory Authority: ForestryDepartment 
... 
Amount paid thus far for trade 2ks89j9 = 25000; total required = 50000 
Payment request for trade 2ks89j9 recorded 
TradeWorkflow Invoke 
TradeWorkflow Invoke 
Query Response:{"Balance":"150000"} 
TradeWorkflow Invoke 
Query Response:{"Balance":"150000"} 
PASS 
ok       trade-finance-logistics/chaincode/src/github.com/trade_workflow_v1      0.036s 
```

# 链码设计主题

# 复合键

我们经常需要在总帐上存储一个类型的多个实例，比如多个贸易协议、信用证等等。在这种情况下，这些实例的键通常将由多个属性的组合构造而成，例如`"Trade" + ID, yielding ["Trade1","Trade2", ...]`。实例的键可以在代码中自定义，或者在SHIM中提供API函数来构造实例的复合键（换句话说，基于几个属性的唯一键）。这些函数简化了复合键的构造。复合键可以像普通字符串键一样使用`PutState()`和`GetState()`函数来记录和检索值。

以下代码段显示了一系列创建和使用复合键的函数：

```
// The function creates a key by combining the attributes into a single string. 
// The arguments must be valid utf8 strings and must not contain U+0000 (nil byte) and U+10FFFF charactres. 
func CreateCompositeKey(objectType string, attributes []string) (string, error) 

// The function splits the compositeKey into attributes from which the key was formed. 
// This function is useful for extracting attributes from keys returned by range queries. 
func SplitCompositeKey(compositeKey string) (string, []string, error) 
```

在下面的代码段中，我们可以看到一个名为`getTradeKey`的函数，它通过将关键字`Trade`与贸易的ID组合构造了一个唯一的复合键：

```
func getTradeKey(stub shim.ChaincodeStubInterface, tradeID string) (string, error) { 
   tradeKey, err := stub.CreateCompositeKey("Trade", []string{tradeID}) 
   if err != nil { 
         return "", err 
   } else { 
         return tradeKey, nil 
   } 
}
```

在更复杂的情况下，键可以由多个属性构造。复合键还允许您根据键的组件在范围查询中搜索资产。我们将在接下来的部分更详细地探讨搜索。

# 范围查询

除了使用唯一键检索资产之外，SHIM还提供API函数来根据范围条件检索一系列资产。此外，可以对复合键进行建模，以便查询多个键的组件。

范围函数返回与查询条件匹配的一组键的迭代器（`StateQueryIteratorInterface`）。 返回的键按字典顺序排列。 迭代器必须通过调用`Close()`函数关闭。 此外，当复合键具有多个属性时，范围查询函数`GetStateByPartialCompositeKey()`可用于搜索匹配部分属性的键。

例如，由`TradeId`和`PaymentId`组成的支付密钥可以在与特定`TradeId`相关联的所有支付中进行搜索，如下片段所示：

```
 // Returns an iterator over all keys between the startKey (inclusive) and endKey (exclusive). 
// To query from start or end of the range, the startKey and endKey can be an empty. 
func GetStateByRange(startKey, endKey string) (StateQueryIteratorInterface, error) 

// Returns an iterator over all composite keys whose prefix matches the given partial composite key. 
// Same rules as for arguments of CreateCompositeKey function apply. 
func GetStateByPartialCompositeKey(objectType string, keys []string) (StateQueryIteratorInterface, error) 
```

我们还可以使用以下查询搜索ID范围在1-100之间的所有贸易协议：

```
startKey, err = getTradeKey(stub, "1") 
endKey, err = getTradeKey(stub, "100") 

keysIterator, err := stub.GetStateByRange(startKey, endKey) 
if err != nil { 
    return shim.Error(fmt.Printf("Error accessing state: %s", err)) 
} 

defer keysIterator.Close() 

var keys []string 
for keysIterator.HasNext() { 
    key, _, err := keysIterator.Next() 
    if err != nil { 
        return shim.Error(fmt.Printf("keys operation failed. Error accessing state: %s", err)) 
    } 
    keys = append(keys, key) 
}
```

# 状态查询和CouchDB

默认情况下，Fabric使用LevelDB作为Worldstate的存储。 Fabric还提供了配置对等方将Worldstate存储在CouchDB中的选项。 当资产以JSON文档的形式存储时，CouchDB允许您根据资产状态执行复杂的查询。

查询采用本机CouchDB声明性JSON查询语法格式化。 此语法的当前版本可在以下链接找到：[http://docs.couchdb.org/en/2.1.1/api/database/find.html.](http://docs.couchdb.org/en/2.1.1/api/database/find.html)

Fabric将查询转发到CouchDB并返回一个迭代器（`StateQueryIteratorInterface()`），该迭代器可用于迭代结果集。 基于状态的查询函数的声明如下所示：

```
func GetQueryResult(query string) (StateQueryIteratorInterface, error)
```

在下面的代码片段中，我们可以看到一个基于状态的查询，用于所有状态为`ACCEPTED`且收到的付款超过1000的贸易协议。 然后执行查询，并将找到的文档写入终端，如下所示：

```
// CouchDB query definition
queryString :=
`{
    "selector": {
            "status": "ACCEPTED"
            "payment": {
                    "$gt": 1000
            }
    }
}`

fmt.Printf("queryString:\n%s\n", queryString)

// Invoke query
resultsIterator, err := stub.GetQueryResult(queryString)
if err != nil {
    return nil, err
}
defer resultsIterator.Close()

var buffer bytes.Buffer
buffer.WriteString("[")

// Iterate through all returned assets
bArrayMemberAlreadyWritten := false
for resultsIterator.HasNext() {
    queryResponse, err := resultsIterator.Next()
    if err != nil {
        return nil, err
    }
    if bArrayMemberAlreadyWritten == true {
        buffer.WriteString(",")
    }
    buffer.WriteString("{\"Key\":")
    buffer.WriteString("\"")
    buffer.WriteString(queryResponse.Key)
    buffer.WriteString("\"")

    buffer.WriteString(", \"Record\":")
    buffer.WriteString(string(queryResponse.Value))
    buffer.WriteString("}")
    bArrayMemberAlreadyWritten = true
}
buffer.WriteString("]")

fmt.Printf("queryResult:\n%s\n", buffer.String())
```

请注意，与键的查询不同，对状态的查询不会记录到交易的`ReadSet`中。 因此，交易的验证实际上无法验证在执行和提交交易之间Worldstate的更改。 因此，链码设计必须考虑到这一点； 如果查询基于预期的调用序列，那么无效的交易可能会出现。

# 索引

在大型数据集上执行查询是一项计算复杂的任务。 Fabric提供了在CouchDB托管的Worldstate上定义索引以提高效率的机制。 请注意，索引也是查询中排序操作所必需的。

索引在一个扩展名为`*.json`的单独文件中以JSON格式定义。 格式的完整定义可在以下链接找到：[http://docs.couchdb.org/en/2.1.1/api/database/find.html#db-index](http://docs.couchdb.org/en/2.1.1/api/database/find.html#db-index)。

以下代码片段说明了一个与我们之前查看的贸易协议查询匹配的索引：

```
 { 
  "index": { 
    "fields": [ 
      "status", 
      "payment" 
    ] 
  }, 
  "name": "index_sp", 
  "type": "json" 
}  
```

在这里，索引文件被放置到文件夹`/META-INF/statedb/couchdb/indexes`中。在编译期间，索引与链码一起打包。在对等体上安装和实例化链码后，索引会自动部署到世界状态并用于查询。

# 读集（ReadSet）和写集（WriteSet）

当接收到来自客户端的事务调用消息时，背书节点会执行一个事务。执行会在对等节点的世界状态上下文中调用链码，并将其数据的所有读取和写入记录到`ReadSet`和`WriteSet`中。

交易的`WriteSet`包含了在链码执行期间被修改的键值对列表。当修改键的值时（即记录新键值对或使用新值更新现有键），`WriteSet`会包含更新后的键值对。

当键被删除时，`WriteSet`会包含具有标记键为已删除的属性的键。如果在链码执行期间多次修改单个键，则`WriteSet`会包含最新修改的值。

交易的`ReadSet`包含了在链码执行期间访问的键及其版本的列表。键的版本号由区块号和区块内事务号的组合派生而来。这种设计使得数据的高效搜索和处理成为可能。交易的另一部分包含了关于范围查询及其结果的信息。请记住，当链码读取键的值时，会返回账本中最新提交的值。

如果链码执行期间引入的修改被存储在`WriteSet`中，当链码读取在执行期间被修改的键时，会返回已提交而非修改后的值。因此，如果后续需要修改后的值，则必须实现链码以保留并使用正确的值。

一个交易的`ReadSet`和`WriteSet`的示例如下：

```
{
  "rwset": {
    "reads": [
      {
        "key": "key1",
        "version": {
          "block_num": {
            "low": 9546,
            "high": 0,
            "unsigned": true
          },
          "tx_num": {
            "low": 0,
            "high": 0,
            "unsigned": true
          }
        }
      }
    ],
    "range_queries_info": [],
    "writes": [
      {
        "key": "key1",
        "is_delete": false,
        "value": "value1"
      },
      {
        "key": "key2",
        "is_delete": true
      }
    ]
  }
}
```

# 多版本并发控制

Fabric使用**多版本并发控制**（**MVCC**）机制来确保账本的一致性并防止双重支付。双重支付攻击旨在通过引入使用或多次修改同一资源的事务来利用系统中的缺陷，比如在加密货币网络中多次花费同一枚硬币。键碰撞是另一种可能发生的问题类型，它可能会在并行客户端提交的事务处理中尝试同时修改相同的键/值对。

此外，由于Fabric的去中心化架构，事务执行顺序可以在不同的Fabric组件（包括背书者，排序者和提交者）上有不同的排序和提交方式，从而在交易计算和提交之间引入延迟，其中可能发生键冲突。去中心化还使网络容易受到客户端故意或无意地修改交易顺序的潜在问题和攻击的影响。

为确保一致性，像数据库这样的计算机系统通常使用锁定机制。然而，在Fabric中无法使用这种集中式方法。同时，也值得注意的是，锁定有时可能会导致性能下降。

为了应对这一点，Fabric使用了存储在总账簿上的键的版本系统。版本系统的目标是确保交易按照不引入不一致性的顺序被排序和提交到总账簿中。当在提交对等方收到一个块时，会验证块中的每笔交易。该算法会检查`ReadSet`中的键及其版本；如果`ReadSet`中每个键的版本与世界状态中相同键的版本，或同一块中之前的交易的版本相匹配，则交易被视为有效。换句话说，该算法验证执行交易期间从世界状态读取的任何数据是否已发生更改。

如果一个事务包含范围查询，这些查询也将得到验证。对于每个范围查询，算法会检查在链码执行期间执行查询的结果是否与之前完全相同，或者是否发生了任何修改。

未通过此验证的交易将在总账簿中标记为无效，并且它们所引入的更改不会映射到世界状态中。需要注意的是，由于总账簿是不可修改的，这些交易会保留在总账簿上。

如果交易通过了验证，`WriteSet`将被映射到世界状态。交易修改的每个键都会在世界状态中设置为`WriteSet`中指定的新值，并且该键在世界状态中的版本会设置为从交易中导出的版本。通过这种方式，任何重复支出等不一致性将被防止。同时，在可能发生键冲突的情况下，链码设计必须考虑MVCC的行为。针对键冲突和MVCC，存在多种公认的解决策略，如分割资产、使用多个键、事务排队等。

# 日志输出

日志记录是系统代码的重要组成部分，它使得可以分析和检测运行时问题。

Fabric中的日志记录是基于标准的Go日志包`github.com/op/go-logging`。日志机制提供基于严重性的日志控制和消息的漂亮打印装饰。日志级别按严重性递减的顺序定义，如下所示：

```
CRITICAL | ERROR | WARNING | NOTICE | INFO | DEBUG 
```

所有组件生成的日志消息都会组合并写入标准错误文件（`stderr`）。可以通过对对等体和模块的配置以及chaincode的代码来控制日志记录。

# 配置

对等体日志的默认配置设置为级别INFO，但可以通过以下方式控制此级别：

1.  命令行选项日志级别。此选项覆盖默认配置，如下所示：

```
peer node start --logging-level=error  
```

请注意，通过命令行选项可以配置任何模块或chaincode，如下所示：

```
 peer node start --logging-level=chaincode=error:main=info
```

1.  默认的日志级别也可以通过`environment`变量`CORE_LOGGING_LEVEL`来定义，默认配置如下所示：

```
peer0.org1.example.com:
    environment:
        - CORE_LOGGING_LEVEL=error
```

1.  `core.yml`文件中的一个配置属性，定义了网络的配置，也可以与以下代码一起使用：

```
logging:
    level: info
```

1.  `core.yml` 文件还允许您配置特定模块的日志级别，例如`chaincode`模块或消息格式，如下节选所示：

```
 chaincode: 
   logging: 
         level:  error 
         shim:   warning  
```

关于各种配置选项的详细信息包含在`core.yml`文件的注释中。

# 日志API

SHIM包提供了API，供chaincode创建和管理日志对象。这些对象生成的日志与对等体日志集成。

chaincode可以创建和使用任意数量的日志对象。每个日志对象必须有一个唯一的名称，用于在输出中添加日志记录的前缀并区分不同日志对象和SHIM的记录。（请记住，日志对象名称SHIM API是保留的，不应在chaincode中使用。）每个日志对象都设置了一个日志严重级别，在该级别下记录日志将被发送到输出中。具有严重级别`CRITICAL`的日志记录始终出现在输出中。以下节选列出了在chaincode中创建和管理日志对象的API函数。

```
// Creates a new logging object. 
func NewLogger(name string) *ChaincodeLogger 

// Converts a case-insensitive string representing a logging level into an element of LoggingLevel enumeration type. 
// This function is used to convert constants of standard GO logging levels (i.e. CRITICAL, ERROR, WARNING, NOTICE, INFO or DEBUG) into the shim's enumeration LoggingLevel type (i.e. LogDebug, LogInfo, LogNotice, LogWarning, LogError, LogCritical). 
func LogLevel(levelString string) (LoggingLevel, error) 

// Sets the logging level of the logging object. 
func (c *ChaincodeLogger) SetLevel(level LoggingLevel) 

// Returns true if the logging object will generate logs at the given level. 
func (c *ChaincodeLogger) IsEnabledFor(level LoggingLevel) bool 
```

日志对象`ChaincodeLogger`提供了每个严重级别的日志记录函数。以下是`ChaincodeLogger`的函数列表。

```
func (c *ChaincodeLogger) Debug(args ...interface{}) 
func (c *ChaincodeLogger) Debugf(format string, args ...interface{}) 
func (c *ChaincodeLogger) Info(args ...interface{}) 
func (c *ChaincodeLogger) Infof(format string, args ...interface{}) 
func (c *ChaincodeLogger) Notice(args ...interface{}) 
func (c *ChaincodeLogger) Noticef(format string, args ...interface{}) 
func (c *ChaincodeLogger) Warning(args ...interface{}) 
func (c *ChaincodeLogger) Warningf(format string, args ...interface{}) 
func (c *ChaincodeLogger) Error(args ...interface{}) 
func (c *ChaincodeLogger) Errorf(format string, args ...interface{}) 
func (c *ChaincodeLogger) Critical(args ...interface{}) 
func (c *ChaincodeLogger) Criticalf(format string, args ...interface{}) 
```

记录的默认格式由SHIM的配置定义，该配置在输入参数的打印表示之间添加空格。对于每个严重级别，日志对象还提供了一个带有后缀`f`的附加函数，这些函数允许您使用参数`format`来控制输出的格式。

由日志对象生成的输出模板如下：

```
[timestamp] [logger name] [severity level] printed arguments 
```

所有日志对象和SHIM的输出都会合并并发送到标准错误（`stderr`）中。

以下代码块说明了如何创建和使用日志对象的示例：

```
var logger = shim.NewLogger("tradeWorkflow") 
logger.SetLevel(shim.LogDebug) 

_, args := stub.GetFunctionAndParameters() 
logger.Debugf("Function: %s(%s)", "requestTrade", strings.Join(args, ",")) 

if !authenticateImporterOrg(creatorOrg, creatorCertIssuer) { 
   logger.Info("Caller not a member of Importer Org. Access denied:", creatorOrg, creatorCertIssuer) 
} 
```

# SHIM日志级别

链码还可以通过使用 API 函数 `SetLoggingLevel` 直接控制其 SHIM 的日志记录严重级别，如下所示：

```
logLevel, _ := shim.LogLevel(os.Getenv("TW_SHIM_LOGGING_LEVEL"))
shim.SetLoggingLevel(logLevel)
```

# Stdout 和 stderr

除了由 SHIM API 提供并与对等节点集成的日志记录机制外，在开发阶段，链码可以使用标准输出文件。链码作为一个独立的进程执行，因此可以使用标准输出（`stdout`）和标准错误（`stderr`）文件来使用标准的 Go 打印函数记录输出（例如，`fmt.Printf(...)` 和 `os.Stdout`）。默认情况下，在 `Dev` 模式下启动链码进程时，标准输出可用。

在生产环境中，当链码进程由对等节点管理时，出于安全原因，标准输出被禁用。当需要时，可以通过设置对等节点的配置变量 `CORE_VM_DOCKER_ATTACHSTDOUT` 来启用它。然后，链码的输出将与对等节点的输出合并。请注意，这些输出仅应用于调试目的，不应在生产环境中启用。

以下片段说明了其他 SHIM API 函数：

```
peer0.org1.example.com: 
   environment: 
         - CORE_VM_DOCKER_ATTACHSTDOUT=true 
```

列表 4.1：在 `docker-compose` 文件中启用对等节点上链码标准输出文件。

# 其他 SHIM API 函数

本节我们提供了剩余的适用于链码的 SHIM API 函数的概述。

```
 // Returns an unique Id of the transaction proposal. 
func GetTxID() string 

// Returns an Id of the channel the transaction proposal was sent to. 
func GetChannelID() string 

// Calls an Invoke function on a specified chaincode, in the context of the current transaction. 
// If the invoked chaincode is on the same channel, the ReadSet and WriteSet will be added into the same transaction. 
// If the invoked chaincode is on a different channel, the invocation can be used only as a query. 
func InvokeChaincode(chaincodeName string, args [][]byte, channel string) pb.Response 

// Returns a list of historical states, timestamps and transactions ids. 
func GetHistoryForKey(key string) (HistoryQueryIteratorInterface, error) 

// Returns the identity of the user submitting the transaction proposal. 
func GetCreator() ([]byte, error) 

// Returns a map of fields containing cryptographic material which may be used to implement custom privacy layer in the chaincode. 
func GetTransient() (map[string][]byte, error) 

// Returns data which can be used to enforce a link between application data and the transaction proposal. 
func GetBinding() ([]byte, error) 

// Returns data produced by peer decorators which modified the chaincode input. 
func GetDecorations() map[string][]byte 

// Returns data elements of a transaction proposal. 
func GetSignedProposal() (*pb.SignedProposal, error) 

// Returns a timestamp of the transaction creation by the client. The timestamp is consistent across all endorsers. 
func GetTxTimestamp() (*timestamp.Timestamp, error) 

// Sets an event attached to the transaction proposal response. This event will be be included in the block and ledger. 
func SetEvent(name string, payload []byte) error  
```

# 总结

设计和实现一个功能良好的链码是一项复杂的软件工程任务，它需要对 Fabric 架构、API 函数以及 GO 语言的知识，以及对业务需求的正确实现。

在本章中，我们逐步学习了如何启动适用于链码实现和测试的开发模式的区块链网络，以及如何使用 CLI 部署和调用链码。然后我们学习了如何实现我们场景的链码。我们通过 `Init` 和 `Invoke` 函数探索了链码如何接收来自客户端的请求，探索了访问控制机制以及开发人员可用于实现链码功能的各种 API。

最后，我们学习了如何测试链码以及如何将日志功能集成到代码中。为了为下一章做好准备，现在应该使用 `./trade.sh` down `-d` true 停止网络。
