# 第三章：撰写智能合约

在前一章中，我们学习了以太坊区块链的工作原理以及 PoW 共识协议如何保证其安全性。现在是时候开始撰写智能合约了，因为我们已经对以太坊的工作原理有了很好的把握。有各种各样的语言可以编写以太坊智能合约，但 Solidity 是最流行的。在本章中，我们将学习 Solidity 编程语言。最终，我们将构建一个用于在特定时间证明存在性、完整性和所有权的 DApp，即一个可以证明某个文件在特定时间与特定所有者在一起的 DApp。

在本章中，我们将涵盖以下主题：

+   Solidity 源文件的布局

+   了解 Solidity 数据类型

+   合约的特殊变量和函数

+   控制结构

+   合约的结构和特征

+   编译和部署合约

# Solidity 源文件

使用`.sol`扩展名指示 Solidity 源文件。与任何其他编程语言一样，Solidity 有各种版本。在撰写本书时，最新版本是 0.4.2。

在源文件中，你可以使用`pragma Solidity`指令提及编写代码所需的编译器版本。

例如，看一下以下示例：

```
pragma Solidity ⁰.4.2; 

```

现在，源文件将无法在早于版本 0.4.2 的编译器上编译，并且也无法在从版本 0.5.0 开始的编译器上工作（使用 `^` 添加了第二个条件）。版本在 0.4.2 到 0.5.0 之间的编译器最可能包含 bug 修复而不是任何破坏性更改。

可以为编译器版本指定更复杂的规则；该表达式遵循 npm 使用的规则。

# 智能合约的结构

合约类似于类。合约包含状态变量、函数、函数修饰符、事件、结构和枚举。合约还支持继承。继承是通过在编译时复制代码来实现的。智能合约还支持多态。

让我们看一个智能合约的示例，以了解其外观：

```
contract Sample 
{ 
    //state variables 
    uint256 data; 
    address owner; 

    //event definition 
    event logData(uint256 dataToLog);  

    //function modifier 
    modifier onlyOwner() { 
        if (msg.sender != owner) throw; 
        _; 
    } 

    //constructor 
    function Sample(uint256 initData, address initOwner){ 
        data = initData; 
        owner = initOwner; 
    } 

    //functions 
    function getData() returns (uint256 returnedData){ 
        return data; 
    } 

    function setData(uint256 newData) onlyOwner{ 
        logData(newData); 
        data = newData; 
    } 
} 

```

以下是前述代码的工作原理：

+   首先，我们使用`contract`关键字声明了一个合约。

+   然后，我们声明了两个状态变量；`data`保存了一些数据，而`owner`保存了合约部署者的以太坊钱包地址，也就是合约部署的地址。

+   然后，我们定义了一个事件。事件用于通知客户端有关某事的信息。每当`data`发生更改时，我们将触发此事件。所有事件都保存在区块链中。

+   然后，我们定义了一个函数修饰符。修饰符用于在执行函数之前自动检查条件。在这里，修饰符检查合约的所有者是否调用了函数。如果没有，则会引发异常。

+   然后，我们有了合约构造函数。在部署合约时，会调用构造函数。构造函数用于初始化状态变量。

+   然后，我们定义了两种方法。第一种方法是获取`data`状态变量的值，第二种方法是改变`data`的值。

在深入了解智能合约的特性之前，让我们先学习一些与 Solidity 相关的其他重要内容。 然后我们将回到合约。

# 数据位置

所有你之前学过的编程语言都是将它们的变量存储在内存中。而在 Solidity 中，变量根据上下文的不同，会被存储在内存和文件系统中。

根据上下文的不同，总是存在一个默认的位置。但是对于字符串、数组和结构等复杂数据类型，可以通过在类型后添加`storage`或`memory`来覆盖默认位置。函数参数（包括返回参数）的默认位置是内存，局部变量的默认位置是存储，当然状态变量的位置强制为存储。

数据位置很重要，因为它们会改变赋值的行为：

+   存储变量与内存变量之间的赋值总是创建独立的副本。但是，从一个存储在内存中的复杂类型赋值给另一个存储在内存中的复杂类型并不会创建副本。

+   对状态变量的赋值（即使来自其他状态变量）总是创建独立的副本。

+   你不能将存储在内存中的复杂类型赋值给本地存储变量。

+   在将状态变量赋值给本地存储变量时，本地存储变量指向状态变量；也就是说，本地存储变量成为了指针。

# 有哪些不同的数据类型？

Solidity 是一种静态类型语言；变量所持有的数据类型需要预先定义。默认情况下，所有变量的位都被赋值为 0。在 Solidity 中，变量是函数作用域的；也就是说，在函数内部声明的任何变量都将对整个函数的作用域有效，无论它在何处声明。

现在让我们来看看 Solidity 提供的各种数据类型：

+   最简单的数据类型是 `bool`。它可以存储 `true` 或 `false`。

+   `uint8`、`uint16`、`uint24` ... `uint256` 用于分别存储 8 位、16 位、24 位 ... 256 位的无符号整数。同样，`int8`、`int16` ... `int256` 分别用于存储 8 位、16 位 ... 256 位的有符号整数。`uint` 和 `int` 是 `uint256` 和 `int256` 的别名。类似于 `uint` 和 `int`，`ufixed` 和 `fixed` 用于表示小数。`ufixed0x8`、`ufixed0x16` ... `ufixed0x256` 用于分别存储 8 位、16 位 ... 256 位的无符号小数。同样，`fixed0x8`、`fixed0x16` ... `fixed0x256` 用于分别存储 8 位、16 位 ... 256 位有符号小数。如果一个数字需要多于 256 位的存储空间，那么就使用 256 位的数据类型，于此情况下将存储该数字的近似值。

+   `address` 用于通过分配十六进制文字来存储最多 20 个字节的值。它用于存储以太坊地址。`address` 类型公开两个属性：`balance` 和 `send`。`balance` 用于检查地址的余额，`send` 用于向地址转移以太币。send 方法接受需要转移的 wei 数量，并根据转移是否成功返回 true 或 false。wei 从调用 `send` 方法的合同中扣除。你可以在 Solidity 中使用 `0x` 前缀为变量分配十六进制编码的值。

# 数组

Solidity 支持通用数组和字节数组。它支持固定大小和动态数组。它还支持多维数组。

`bytes1`、`bytes2`、`bytes3`、...、`bytes32` 是字节数组的类型。`byte` 是 `bytes1` 的别名。

这里是展示通用数组语法的示例：

```
contract sample{ 
    //dynamic size array 
    //wherever an array literal is seen a new array is created. If the array literal is in state than it's stored in storage and if it's found inside function than its stored in memory 
    //Here myArray stores [0, 0] array. The type of [0, 0] is decided based on its values.  
    //Therefore you cannot assign an empty array literal. 
    int[] myArray = [0, 0]; 

    function sample(uint index, int value){ 

        //index of an array should be uint256 type 
        myArray[index] = value; 

        //myArray2 holds pointer to myArray 
        int[] myArray2 = myArray; 

        //a fixed size array in memory 
        //here we are forced to use uint24 because 99999 is the max value and 24 bits is the max size required to hold it.  
        //This restriction is applied to literals in memory because memory is expensive. As [1, 2, 99999] is of type uint24 therefore myArray3 also has to be the same type to store pointer to it. 
        uint24[3] memory myArray3 = [1, 2, 99999]; //array literal 

        //throws exception while compiling as myArray4 cannot be assigned to complex type stored in memory 
        uint8[2] myArray4 = [1, 2]; 
    } 
} 

```

以下是关于数组的一些重要事项：

+   数组还有一个 `length` 属性，用于查找数组的长度。你也可以为 length 属性分配一个值来改变数组的大小。但是，在内存中无法调整数组的大小，也不能调整非动态数组的大小。

+   如果尝试访问动态数组的未设置索引，则会抛出异常。

请记住，数组、结构体和映射都不能作为函数的参数，也不能作为函数的返回值。

# 字符串

在 Solidity 中，有两种创建字符串的方法：使用 `bytes` 和 `string`。`bytes` 用于创建原始字符串，而 `string` 用于创建 UTF-8 字符串。字符串的长度始终是动态的。

这里是展示字符串语法的示例：

```
contract sample{ 
    //wherever a string literal is seen a new string is created. If the string literal is in state than it's stored in storage and if it's found inside function than its stored in memory 
    //Here myString stores "" string.  
    string myString = "";  //string literal 
    bytes myRawString; 

    function sample(string initString, bytes rawStringInit){ 
        myString = initString; 

        //myString2 holds a pointer to myString 
        string myString2 = myString; 

        //myString3 is a string in memory  
        string memory myString3 = "ABCDE"; 

        //here the length and content changes 
        myString3 = "XYZ"; 

        myRawString = rawStringInit; 

        //incrementing the length of myRawString 
        myRawString.length++; 

        //throws exception while compiling 
        string myString4 = "Example"; 

        //throws exception while compiling 
        string myString5 = initString; 
    } 
} 

```

# 结构体

Solidity 也支持结构体。以下是展示结构体语法的示例：

```
contract sample{ 
    struct myStruct { 
        bool myBool; 
        string myString; 
    } 

    myStruct s1; 

    //wherever a struct method is seen a new struct is created. If the struct method is in state than it's stored in storage and if it's found inside function than its stored in memory 
    myStruct s2 = myStruct(true, ""); //struct method syntax 

    function sample(bool initBool, string initString){ 

        //create a instance of struct 
        s1 = myStruct(initBool, initString); 

        //myStruct(initBool, initString) creates a instance in memory 
        myStruct memory s3 = myStruct(initBool, initString); 
    } 
} 

```

请注意，函数参数不能是结构体，函数也不能返回结构体。

# 枚举

Solidity 也支持枚举。以下是展示枚举语法的示例：

```
contract sample { 

    //The integer type which can hold all enum values and is the smallest is chosen to hold enum values 
    enum OS { Windows, Linux, OSX, UNIX } 

    OS choice; 

    function sample(OS chosen){ 
        choice = chosen; 
    } 

    function setLinuxOS(){ 
        choice = OS.Linux; 
    } 

    function getChoice() returns (OS chosenOS){ 
        return choice; 
    } 
} 

```

# 映射

映射数据类型是哈希表。映射只能存在于存储中，不能存在于内存中。因此，它们仅被声明为状态变量。映射可以被看作由键/值对组成。键实际上不存储；相反，使用键的 keccak256 哈希来查找值。映射没有长度。映射不能赋值给另一个映射。

这里是创建和使用映射的示例：

```
contract sample{ 
    mapping (int => string) myMap; 

    function sample(int key, string value){ 
        myMap[key] = value; 

        //myMap2 is a reference to myMap 
        mapping (int => string) myMap2 = myMap; 
    } 
} 

```

请记住，如果尝试访问未设置的键，则会返回所有 0 位。

# delete 操作符

`delete` 操作符可以应用于任何变量，将其重置为默认值。默认值是所有位分配为 0。

如果我们对动态数组应用 `delete`，那么它将删除所有元素，长度变为 0。如果对静态数组应用 `delete`，则所有索引将被重置。你也可以对特定索引应用 `delete`，在这种情况下，索引将被重置。

如果将`delete`应用于映射类型，则不会发生任何事情。但是如果将`delete`应用于映射的键，则与键关联的值将被删除。

下面是一个演示`delete`运算符的示例：

```
contract sample { 

    struct Struct { 
        mapping (int => int) myMap; 
        int myNumber; 
    } 

    int[] myArray; 
    Struct myStruct;  

    function sample(int key, int value, int number, int[] array) { 

        //maps cannot be assigned so while constructing struct we ignore the maps 
        myStruct = Struct(number); 

        //here set the map key/value 
        myStruct.myMap[key] = value; 

        myArray = array; 
    } 

    function reset(){ 

        //myArray length is now 0 
        delete myArray; 

        //myNumber is now 0 and myMap remains as it is 
        delete myStruct; 
    } 

    function deleteKey(int key){ 

        //here we are deleting the key 
        delete myStruct.myMap[key]; 
    } 

} 

```

# 基本类型之间的转换

除了数组、字符串、结构、枚举和映射之外，其他一切皆为基本类型。

如果将运算符应用于不同类型，编译器会尝试将其中一个操作数隐式转换为另一个的类型。总的来说，如果语义上有意义且没有丢失信息，那么值类型之间的隐式转换是可能的：`uint8`可以转换为`uint16`，`int128`可以转换为`int256`，但`int8`无法转换为`uint256`（因为`uint256`不能容纳，例如，-1）。此外，无符号整数可以转换为相同或更大尺寸的字节，但反之则不行。任何可转换为`uint160`的类型也可以转换为`address`。

Solidity 还支持明确的转换。因此，如果编译器不允许两种数据类型之间的隐式转换，那么您可以进行显式转换。建议尽量避免显式转换，因为它可能会给您带来意外的结果。

让我们来看一个明确转换的例子：

```
uint32 a = 0x12345678; 
uint16 b = uint16(a); // b will be 0x5678 now 

```

这里我们明确地将`uint32`类型转换为`uint16`，也就是将一个大类型转换为一个小类型；因此，高阶位被截断。

# 使用 var

Solidity 提供`var`关键字来声明变量。在这种情况下，变量的类型是动态确定的，取决于分配给它的第一个值。一旦分配了一个值，类型就是固定的，因此如果您将另一个类型分配给它，就会引起类型转换。

下面是一个示例来演示`var`：

```
int256 x = 12; 

//y type is int256 
var y = x; 

uint256 z= 9; 

//exception because implicit conversion not possible 
y = z; 

```

请记住，在定义数组和映射时，不能使用`var`。也不能用它定义函数参数和状态变量。

# 控制结构

Solidity 支持`if`、`else`、`while`、`for`、`break`、`continue`、`return`、`? :`控制结构。

下面是一个演示控制结构的例子：

```
contract sample{ 
    int a = 12; 
    int[] b; 

    function sample() 
    { 
        //"==" throws exception for complex types  
        if(a == 12) 
        { 
        } 
        else if(a == 34) 
        { 
        } 
        else 
        { 
        } 

        var temp = 10; 

        while(temp < 20) 
        { 
            if(temp == 17) 
            { 
                break; 
            } 
            else 
            { 
                continue; 
            } 

            temp++; 
        } 

        for(var iii = 0; iii < b.length; iii++) 
        { 

        } 
    } 
} 

```

# 使用`new`运算符创建合同

合同可以使用`new`关键字创建一个新的合同。必须知道正在创建的合同的完整代码。

下面是一个演示的例子：

```
contract sample1 
{ 
    int a; 

    function assign(int b) 
    { 
        a = b; 
    } 
} 

contract sample2{ 
    function sample2() 
    { 
        sample1 s = new sample1(); 
        s.assign(12); 
    } 
} 

```

# 异常

在一些情况下，异常会自动抛出。您可以使用`throw`手动抛出异常。异常的效果是当前执行的调用被停止和回滚（也就是说，对状态和余额的所有更改都被撤销）。无法捕捉异常：

```
contract sample 
{ 
    function myFunction() 
    { 
        throw; 
    } 
} 

```

# 外部函数调用

在 Solidity 中有两种函数调用：内部和外部函数调用。内部函数调用是指一个函数调用同一合同中的另一个函数。

外部函数调用是指一个函数调用另一个合同中的函数。让我们看一个例子：

```
contract sample1 
{ 
    int a; 

    //"payable" is a built-in modifier 
    //This modifier is required if another contract is sending Ether while calling the method 
    function sample1(int b) payable 
    { 
        a = b; 
    } 

    function assign(int c)  
    { 
        a = c; 
    } 

    function makePayment(int d) payable 
    { 
        a = d; 
    } 
} 

contract sample2{ 

    function hello() 
    { 
    } 

    function sample2(address addressOfContract) 
    { 
        //send 12 wei while creating contract instance 
        sample1 s = (new sample1).value(12)(23); 

        s.makePayment(22); 

        //sending Ether also 
        s.makePayment.value(45)(12); 

        //specifying the amount of gas to use 
        s.makePayment.gas(895)(12); 

        //sending Ether and also specifying gas 
        s.makePayment.value(4).gas(900)(12); 

        //hello() is internal call whereas this.hello() is external call 
        this.hello(); 

        //pointing a contract that's already deployed 
        sample1 s2 = sample1(addressOfContract); 

        s2.makePayment(112); 

    } 
} 

```

使用关键字`this`进行的调用称为外部调用。函数内部的`this`关键字代表当前合约实例。

# 合约的特性

现在是时候深入了解合约了。我们将看一些新功能，并且深入了解我们已经看过的功能。

# 可见性

状态变量或函数的可见性定义了谁可以看到它。函数和状态变量有四种可见性：`external`、`public`、`internal`和`private`。

默认情况下，函数的可见性是`public`，状态变量的可见性是`internal`。让我们看看每个可见性函数的含义：

+   `external`：外部函数只能从其他合约或通过交易调用。外部函数`f`不能在内部调用；也就是说，`f()`不起作用，但是`this.f()`可以。你不能将`external`可见性应用于状态变量。

+   `public`：公共函数和状态变量可以以所有可能的方式访问。编译器生成的访问器函数都是公共状态变量。你不能创建自己的访问器。实际上，它只生成 getter，不生成 setter。

+   `internal`：内部函数和状态变量只能从内部访问，也就是说，只能从当前合约和继承它的合约中访问。你不能使用`this`来访问它。

+   `private`：私有函数和状态变量与内部函数类似，但不能被继承的合约访问。

这里有一个代码示例来演示可见性和访问器：

```
contract sample1 
{ 
    int public b = 78; 
    int internal c = 90; 

    function sample1() 
    { 
        //external access 
        this.a(); 

        //compiler error 
        a(); 

        //internal access 
        b = 21; 

        //external access 
        this.b; 

        //external access 
        this.b(); 

        //compiler error 
        this.b(8); 

        //compiler error 
        this.c(); 

        //internal access 
        c = 9; 
    } 

    function a() external  
    { 

    } 
} 

contract sample2 
{ 
    int internal d = 9; 
    int private e = 90; 
} 

//sample3 inherits sample2 
contract sample3 is sample2 
{ 
    sample1 s; 

    function sample3() 
    { 
        s = new sample1(); 

        //external access 
        s.a(); 

        //external access 
        var f = s.b; 

        //compiler error as accessor cannot used to assign a value 
        s.b = 18; 

        //compiler error 
        s.c(); 

        //internal access 
        d = 8; 

        //compiler error 
        e = 7; 
    } 
} 

```

# 函数修饰符

我们之前看到了什么是函数修饰符，并且写了一个基本的函数修饰符。现在让我们深入了解修饰符。

修饰符会被子合约继承，并且子合约可以覆盖它们。可以通过在空格分隔的列表中指定它们来将多个修饰符应用于函数，并按顺序评估它们。你也可以给修饰符传递参数。

在修饰符内部，下一个修饰符体或函数体，无论哪个先出现，都会插入到`_;`出现的地方。

让我们看一个复杂的函数修饰符的代码示例：

```
contract sample 
{ 
    int a = 90; 

    modifier myModifier1(int b) { 
        int c = b; 
        _; 
        c = a; 
        a = 8; 
    } 

    modifier myModifier2 { 
        int c = a; 
        _; 
    } 

    modifier myModifier3 { 
        a = 96; 
        return; 
        _; 
        a = 99; 
    } 

    modifier myModifier4 { 
        int c = a; 
        _; 
    } 

    function myFunction() myModifier1(a) myModifier2 myModifier3 returns (int d) 
    { 
        a = 1; 
        return a; 
    } 
} 

```

这是如何执行`myFunction()`的：

```
int c = b; 
    int c = a; 
        a = 96; 
        return; 
            int c = a; 
                a = 1; 
                return a; 
        a = 99; 
c = a; 
a = 8; 

```

在这里，当你调用`myFunction`方法时，它将返回`0`。但在此之后，当你尝试访问状态变量`a`时，你将得到`8`。

在修饰符或函数体中的`return`会立即退出整个函数，并且返回值被分配给它需要的任何变量。

对于函数而言，在`return`之后的代码在调用者代码执行完成后执行。对于修饰符而言，在上一个修饰符的`_;`后的代码在调用者代码执行完成后执行。在前面的示例中，第 5、6 和 7 行永远不会被执行。在第 4 行之后，执行从第 8 行到第 10 行开始。

修饰符内部的`return`不能与值关联。它总是返回 0 位。

# 回退函数

一个合约可以有一个未命名的函数，称为`fallback`函数。这个函数不能有参数，也不能返回任何东西。如果没有其他函数匹配给定的函数标识符，它会在调用合约时执行。

当合约在没有任何函数调用的情况下接收以太时，也会执行这个函数；也就是说，交易向合约发送以太，并且不调用任何方法。在这样的情况下，通常很少有 gas 可用于函数调用（确切地说，只有 2,300 gas），因此很重要要尽量让 fallback 函数尽可能便宜。

接收以太但没有定义 fallback 函数的合约会抛出异常，将以太发送回去。因此，如果你希望你的合约接收以太，你必须实现一个 fallback 函数。

这里是一个 fallback 函数的例子：

```
contract sample 
{ 
    function() payable  
    { 
        //keep a note of how much Ether has been sent by whom            
    }     
} 

```

# 继承

Solidity 支持通过复制代码实现多重继承，包括多态性。即使一个合约从多个其他合约继承，区块链上只会创建一个合约；父合约的代码总是复制到最终合约中。

下面是一个用来示范继承的例子：

```
contract sample1 
{ 
    function a(){} 

    function b(){} 
} 

//sample2 inherits sample1 
contract sample2 is sample1 
{ 
    function b(){} 
} 

contract sample3 
{ 
    function sample3(int b) 
    { 

    } 
} 

//sample4 inherits from sample1 and sample2 
//Note that sample1 is also parent of sample2, yet there is only a single instance of sample1  
contract sample4 is sample1, sample2 
{ 
    function a(){} 

    function c(){ 

        //this executes the "a" method of sample3 contract 
        a(); 

        //this executes the 'a" method of sample1 contract 
        sample1.a(); 

        //calls sample2.b() because it's in last in the parent contracts list and therefore it overrides sample1.b() 
        b(); 
    } 
} 

//If a constructor takes an argument, it needs to be provided at the constructor of the child contract. 
//In Solidity child constructor doesn't call parent constructor instead parent is initialized and copied to child 
contract sample5 is sample3(122) 
{ 

} 

```

# super 关键字

`super`关键字用于引用继承链中的下一个合约。让我们通过一个例子来理解这一点：

```
contract sample1 
{ 
} 

contract sample2 
{ 
} 

contract sample3 is sample2 
{ 
} 

contract sample4 is sample2 
{ 
} 

contract sample5 is sample4 
{ 
    function myFunc() 
    { 
    } 
} 

contract sample6 is sample1, sample2, sample3, sample5 
{ 
    function myFunc() 
    { 
        //sample5.myFunc() 
        super.myFunc(); 
    } 
} 

```

关于`sample6`合约的最终继承链是`sample6`、`sample5`、`sample4`、`sample2`、`sample3`、`sample1`。继承链从最派生的合约开始，以最少派生的合约结束。

# 抽象合约

只包含函数原型而非实现的合约称为抽象合约。这样的合约不能被编译（即使它们包含了实现的函数和未实现的函数）。如果一个合约继承自一个抽象合约并且没有通过覆盖实现所有未实现的函数，那它本身就是抽象的。

这些抽象合约只是用来让编译器知道接口。当你引用已部署合约并调用它的函数时，这是有用的。

下面是一个用来示范这一点的例子：

```
contract sample1 
{ 
    function a() returns (int b); 
} 

contract sample2 
{ 
    function myFunc() 
    { 
        sample1 s = sample1(0xd5f9d8d94886e70b06e474c3fb14fd43e2f23970); 

        //without abstract contract this wouldn't have compiled 
        s.a(); 
    } 
} 

```

# 库

库与合约类似，但它们的目的是在特定地址只部署一次，并且它们的代码被各种合约重复使用。这意味着如果库函数被调用，它们的代码将在调用合约的上下文中执行；也就是说，`this`指向调用合约，特别是可以访问来自调用合约的存储。由于库是一个隔离的源代码片段，它只能访问调用合约的状态变量，如果它们被显式提供的话（否则就没有办法命名它们）。

库不能有状态变量；它们不支持继承，也不能接收以太。库可以包含结构和枚举。

一旦 Solidity 库部署到区块链上，任何人都可以使用它，假设您知道它的地址并且拥有源代码（仅具有原型或完整实现）。 Solidity 编译器需要源代码，以便它可以确保您正在尝试访问的方法确实存在于库中。

让我们来看一个例子：

```
library math 
{ 
    function addInt(int a, int b) returns (int c) 
    { 
        return a + b; 
    } 
} 

contract sample 
{ 
    function data() returns (int d) 
    { 
        return math.addInt(1, 2); 
    } 
} 

```

我们不能在合同源代码中添加库的地址；相反，在编译时需要将库地址提供给编译器。

库有许多用例。库的两个主要用例如下：

+   如果你有许多具有一些共同代码的合同，那么你可以将该共同代码部署为一个库。这样做可以节省 gas，因为 gas 取决于合同的大小。因此，我们可以将库视为使用它的合同的基本合同。使用基本合同而不是库来分割公共代码不会节省 gas，因为在 Solidity 中，继承是通过复制代码实现的。由于库被认为是基本合同的原因，库中具有内部可见性的函数会被复制到使用它的合同中；否则，具有库内部可见性的函数无法被使用库的合同调用，因为需要进行外部调用，并且具有内部可见性的函数无法使用外部调用调用。此外，库中的结构体和枚举将被复制到使用库的合同中。

+   库可用于向数据类型添加成员函数。

如果库只包含内部函数和/或结构/枚举，则库不需要部署，因为库中的所有内容都会被复制到使用它的合同中。

# 使用 `for`

`using A for B;` 指令可以用于将库函数（从库 `A` 到任何类型 `B`）附加到类型 `B`。这些函数将以调用它们的对象作为第一个参数。

使用 `A for *;` 的效果是将库 `A` 中的函数附加到所有类型上。

以下是一个演示 `for` 的示例：

```
library math 
{ 
    struct myStruct1 { 
        int a; 
    } 

    struct myStruct2 { 
        int a; 
    } 

    //Here we have to make 's' location storage so that we get a reference.  
    //Otherwise addInt will end up accessing/modifying a different instance of myStruct1 than the one on which its invoked 
    function addInt(myStruct1 storage s, int b) returns (int c) 
    { 
        return s.a + b; 
    } 

    function subInt(myStruct2 storage s, int b) returns (int c) 
    { 
        return s.a + b; 
    } 
} 

contract sample 
{ 
    //"*" attaches the functions to all the structs 
    using math for *; 
    math.myStruct1 s1; 
    math.myStruct2 s2; 

    function sample() 
    { 
        s1 = math.myStruct1(9); 
        s2 = math.myStruct2(9); 

        s1.addInt(2); 

        //compiler error as the first parameter of addInt is of type myStruct1 so addInt is not attached to myStruct2 
        s2.addInt(1); 
    } 
} 

```

# 返回多个值

Solidity 允许函数返回多个值。以下是一个演示这一点的示例：

```
contract sample 
{ 
    function a() returns (int a, string c) 
    { 
        return (1, "ss"); 
    } 

    function b() 
    { 
        int A; 
        string memory B; 

        //A is 1 and B is "ss" 
        (A, B) = a(); 

        //A is 1 
        (A,) = a(); 

        //B is "ss" 
        (, B) = a(); 
    } 
} 

```

# 导入其他 Solidity 源文件

Solidity 允许源文件导入其他源文件。以下是一个示例以演示这一点：

```
//This statement imports all global symbols from "filename" (and symbols imported there) into the current global scope. "filename" can be a absolute or relative path. It can only be a HTTP URL 
import "filename"; 

//creates a new global symbol symbolName whose members are all the global symbols from "filename". 
import * as symbolName from "filename"; 

//creates new global symbols alias and symbol2 which reference symbol1 and symbol2 from "filename", respectively. 
import {symbol1 as alias, symbol2} from "filename"; 

//this is equivalent to import * as symbolName from "filename";. 
import "filename" as symbolName; 

```

# 全局可用变量

有一些特殊的全局存在的变量和函数。它们将在接下来的章节中讨论。

# 区块和交易属性

区块和交易属性如下：

+   `block.blockhash(uint blockNumber) returns (bytes32)`: 给定区块的哈希仅适用于最近的 256 个区块。

+   `block.coinbase (address)`: 当前区块的矿工地址。

+   `block.difficulty (uint)`: 当前区块的难度。

+   `block.gaslimit (uint)`: 当前的区块燃气限制。它定义了整个区块中所有事务允许消耗的最大燃气量。其目的是保持区块传播和处理时间低，从而实现足够分散的网络。矿工有权将当前区块的燃气限制设定为上一个区块燃气限制的 0.0975%（1/1,024），因此得到的燃气限制应该是矿工偏好的中位数。

+   `block.number (uint)`: 当前的区块编号。

+   `block.timestamp (uint)`: 当前区块的时间戳。

+   `msg.data (bytes)`: 完整的调用数据包括了事务调用的函数和其参数。

+   `msg.gas (uint)`: 剩余燃气。

+   `msg.sender (address)`: 消息的发送者（当前调用）。

+   `msg.sig (bytes4)`: 调用数据的前四个字节（函数标识符）。

+   `msg.value (uint)`: 与消息一起发送的 wei 数量。

+   `now (uint)`: 当前区块的时间戳（`block.timestamp` 的别名）。

+   `tx.gasprice (uint)`: 事务的燃气价格。

+   `tx.origin (address)`: 事务的发送者（完整的调用链）。

# 与地址类型相关

与地址类型相关的变量如下:

+   `<address>.balance (uint256)`: 以 wei 为单位的地址余额

+   `<address>.send(uint256 amount) returns (bool)`: 向 `address` 发送指定数量的 wei; 失败时返回 `false`。

# 与合约相关

合约相关的变量如下:

+   `this`: 当前合约，可显式转换为 `address` 类型。

+   `selfdestruct(address recipient)`: 销毁当前合同，将其资金发送到给定地址。

# 以太单位

字面上的数字可以以 `wei`、`finney`、`szabo` 或 `以太` 为后缀，以在以太的子单位间转换，没有后缀的以太货币数被假定是 wei; 例如，`2 Ether == 2000 finney` 评估为 `true`。

# 存在性、完整性和拥有权合约

让我们编写一份 Solidity 合约，它可以证明拥有文件的所有权，而不会显示实际文件。它可以证明文件在特定时间存在，并最终检查文件的完整性。

我们将通过将文件的哈希值和所有者的名字作为对存储来实现拥有权的证明。我们将通过将文件的哈希值和区块时间戳作为对存储来实现文件的存在性证明。最后，存储哈希本身证明了文件的完整性; 也就是说，如果文件被修改，那么它的哈希值将发生变化，合同将无法找到这样的文件，从而证明文件已经被修改。

下面是实现所有这些的智能合约的代码:

```
contract Proof 
{ 
    struct FileDetails 
    { 
        uint timestamp; 
        string owner; 
    } 

    mapping (string => FileDetails) files; 

    event logFileAddedStatus(bool status, uint timestamp, string owner, string fileHash); 

    //this is used to store the owner of file at the block timestamp 
    function set(string owner, string fileHash) 
    { 
        //There is no proper way to check if a key already exists or not therefore we are checking for default value i.e., all bits are 0 
        if(files[fileHash].timestamp == 0) 
        { 
            files[fileHash] = FileDetails(block.timestamp, owner); 

            //we are triggering an event so that the frontend of our app knows that the file's existence and ownership details have been stored 
            logFileAddedStatus(true, block.timestamp, owner, fileHash); 
        } 
        else 
        { 
            //this tells to the frontend that file's existence and ownership details couldn't be stored because the file's details had already been stored earlier 
            logFileAddedStatus(false, block.timestamp, owner, fileHash); 
        } 
    } 

    //this is used to get file information 
    function get(string fileHash) returns (uint timestamp, string owner) 
    { 
        return (files[fileHash].timestamp, files[fileHash].owner); 
    } 
} 

```

# 编译和部署合约

以太坊提供了 solc 编译器，它提供了一个命令行界面来编译 `.sol` 文件。访问[`solidity.readthedocs.io/en/develop/installing-solidity.html#binary-packages`](http://solidity.readthedocs.io/en/develop/installing-solidity.html#binary-packages)以找到安装说明，并访问[`Solidity.readthedocs.io/en/develop/using-the-compiler.html`](https://Solidity.readthedocs.io/en/develop/using-the-compiler.html)以找到如何使用的说明。我们不会直接使用 solc 编译器；相反，我们将使用 solcjs 和 Solidity 浏览器。Solcjs 允许我们在 Node.js 中以程序方式编译 Solidity，而浏览器 Solidity 是一个适用于小型合约的 IDE，它提供了编辑器并生成部署合约的代码。

现在，让我们使用以太坊提供的浏览器 Solidity 编译前述合约。在[`Ethereum.github.io/browser-Solidity/`](https://ethereum.github.io/browser-solidity/)了解更多信息。您还可以下载此浏览器 Solidity 源代码并离线使用。访问[`github.com/Ethereum/browser-Solidity/tree/gh-pages`](https://github.com/Ethereum/browser-Solidity/tree/gh-pages)下载。

使用此浏览器 Solidity 的主要优势是它提供了编辑器，并且还生成部署合约的代码。

在编辑器中，复制并粘贴前述合约代码。您将看到它编译并给出了使用 geth 交互式控制台部署它的 web3.js 代码。

您将获得以下输出：

```
var proofContract = web3.eth.contract([{"constant":false,"inputs":[{"name":"fileHash","type":"string"}],"name":"get","outputs":[{"name":"timestamp","type":"uint256"},{"name":"owner","type":"string"}],"payable":false,"type":"function"},{"constant":false,"inputs":[{"name":"owner","type":"string"},{"name":"fileHash","type":"string"}],"name":"set","outputs":[],"payable":false,"type":"function"},{"anonymous":false,"inputs":[{"indexed":false,"name":"status","type":"bool"},{"indexed":false,"name":"timestamp","type":"uint256"},{"indexed":false,"name":"owner","type":"string"},{"indexed":false,"name":"fileHash","type":"string"}],"name":"logFileAddedStatus","type":"event"}]); 
var proof = proofContract.new( 
  { 
    from: web3.eth.accounts[0],  
    data: '60606040526......,  
    gas: 4700000 
  }, function (e, contract){ 
   console.log(e, contract); 
  if (typeof contract.address !== 'undefined') { 
    console.log('Contract mined! address: ' + contract.address + ' transactionHash: ' + contract.transactionHash); 
  } 
}) 

```

`data` 表示 EVM 可理解的合约（字节码）的编译版本。源代码首先转换为操作码，然后操作码转换为字节码。每个操作码都有与之相关的 gas。

`web3.eth.contract` 的第一个参数是 ABI 定义。ABI 定义用于创建交易，因为它包含了所有方法的原型。

现在以开发者模式运行 geth，并启用挖矿。为此，请运行以下命令：

```
geth --dev --mine

```

现在打开另一个命令行窗口，在其中输入以下命令以打开 geth 的交互式 JavaScript 控制台：

```
geth attach

```

这将把 JS 控制台连接到另一个窗口中运行的 geth 实例。

在浏览器 Solidity 的右侧面板中，复制 web3 部署文本区域中的所有内容，并将其粘贴到交互式控制台中。现在按 *Enter*。您将首先获得交易哈希，等待一段时间后，您将在交易被挖掘后获得合约地址。交易哈希是交易的哈希，对于每个交易都是唯一的。每个部署的合约都有一个唯一的合约地址，用于在区块链中标识合约。

合约地址是从其创建者的地址（from 地址）和创建者发送的交易数量（交易 nonce）确定性地计算出来的。这两个参数经过 RLP 编码，然后使用 keccak-256 散列算法进行哈希处理。我们将在后面更多地了解交易 nonce。您可以在 [`github.com/Ethereum/wiki/wiki/RLP`](https://github.com/Ethereum/wiki/wiki/RLP) 了解更多关于 RLP 的信息。

现在让我们存储文件的详细信息并检索它们。

放置此代码以广播交易以存储文件的详细信息：

```
var contract_obj = proofContract.at("0x9220c8ec6489a4298b06c2183cf04fb7e8fbd6d4"); 
contract_obj.set.sendTransaction("Owner Name", "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855", { 
 from: web3.eth.accounts[0], 
}, function(error, transactionHash){ 
  if (!err) 
    console.log(transactionHash); 
}) 

```

在这里，将合约地址替换为您获得的合约地址。`proofContract.at` 方法的第一个参数是合约地址。在这里，我们没有提供 gas，这种情况下，它会自动计算。

现在让我们找到文件的详细信息。按顺序运行此代码以查找文件的详细信息：

```
contract_obj.get.call("e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855"); 

```

您将得到以下输出：

```
[1477591434, "Owner Name"] 

```

调用方法用于在当前状态下使用 EVM 调用合约的方法。它不广播交易。要读取数据，我们不需要广播，因为我们将拥有自己的区块链副本。

在接下来的章节中，我们将更多地了解 web3.js。

# 概要

在本章中，我们学习了 Solidity 编程语言。我们了解了数据位置、数据类型和合约的高级特性。我们还学习了编译和部署智能合约的最快最简单的方法。现在，您应该能够轻松地编写智能合约了。

在下一章中，我们将为智能合约构建一个前端，这将使部署智能合约和运行交易变得更容易。
