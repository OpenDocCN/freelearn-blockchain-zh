# 编写智能合约

在上一章中，我们了解了 Quorum 的工作原理以及各种共识协议是如何保护它的。现在我们了解了 Quorum 的工作原理，让我们继续编写智能合约。Quorum 智能合约可以使用许多语言编写；最流行的是**Solidity**。在本章中，我们将学习 Solidity，并构建一个企业可以用来数字签署文件的 DApp。

在本章中，我们将涵盖以下主题：

+   Solidity 源文件的布局

+   理解 Solidity 数据类型

+   特殊变量和合约函数

+   控制结构

+   合约的结构和特性

+   编译和部署合约

本章与作者之前的书籍*项目区块链*中的章节相同。这不是第二版的书籍，它被用来向读者解释基本概念。

# Solidity 源文件

Solidity 源文件的识别方法是通过 `.sol` 扩展名。它有各种版本，就像通常的编程语言一样。在撰写本书时，最新版本是 `0.4.17`。

在源文件中，您可以使用 `pragma Solidity` 指令来指定编写代码的编译器版本。例如：

```
pragma Solidity ^0.4.17;
```

需要注意的是，源文件不会在早于 `0.4.17` 或晚于 `0.5.0`（此第二个条件使用 `^` 添加）的编译器版本下编译。编译器版本在 `0.4.17` 和 `0.5.0` 之间的情况最有可能包含 bug 修复，并且不太可能破坏任何内容。

我们可以为编译器版本指定更复杂的规则；表达式遵循 `npm` 使用的规则。

# 智能合约的结构

A 类似于一个类。它可以有函数、修改器、状态变量、事件、结构体和枚举。合约也支持继承。您可以通过在编译时复制代码来实现继承。智能合约也可以是多态的。

以下是一个智能合约的示例：

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

让我们看看上述代码如何工作：

+   首先，我们使用 `contract` 关键字声明了一个合约。

+   接下来，我们声明了两个状态变量：`data` 保存一些数据；`owner` 保存了他们的以太坊钱包的地址，也就是合约部署的地址。状态变量构成智能合约的状态，并存储在智能合约的存储中。智能合约的存储位于数据库中。

+   然后，我们定义了事件。事件用于客户端通知。我们的事件将在数据更改时触发。所有事件都保留在区块链中。

+   接下来，我们定义了一个修改器函数。修改器在执行函数之前自动检查条件。我们的修改器检查合约所有者是否是调用函数的人。如果不是，则会抛出异常。

+   在此之后，我们有了合约构造函数。它在部署合约时调用。构造函数用于初始化状态变量。

+   最后，我们定义了两种方法。第一种方法获取数据状态变量的值，第二种方法更改数据值。

在更深入研究智能合约功能之前，我们必须学习与 Solidity 相关的一些重要事项。之后，我们将回到合约。

# Solidity 中的数据位置

与其他编程语言不同，Solidity 的变量根据上下文存储在内存和数据库中。

总是有一个默认位置，但可以通过附加 storage 或 memory 来覆盖复杂类型的数据，例如字符串、数组和结构体。Memory 是函数参数（包括 `return` 参数）的默认值，而 storage 适用于局部和状态变量（显然）。

数据位置很重要，因为它们会改变赋值的行为：

+   在存储变量和内存变量之间的赋值中，始终会创建独立的副本。但是，从一个内存存储的复杂类型赋值给另一个内存存储的复杂类型时，不会创建副本。

+   对状态变量进行赋值时，始终会创建独立的副本（即使来自其他状态变量）。

+   存储在内存中的复杂类型不能赋值给局部存储变量。

+   如果状态变量赋给局部存储变量，那么局部存储变量将指向状态变量；基本上，局部存储变量充当指针。

# 不同类型的数据

Solidity 是一种静态类型语言；变量持有的数据类型需要预定义。所有变量的位默认都被赋值为零。在 Solidity 中，变量是在函数范围内生效；也就是说，无论在函数的任何地方声明的变量都将在整个函数范围内生效。

Solidity 提供了以下数据类型：

+   最简单的数据类型是 `bool`。它可以存储 `true` 或 `false`。

+   `uint8`、`uint16`、`uint24`，一直到 `uint256` 用于存储 8 位、16 位、24 位，一直到 256 位的无符号整数。同样地，`int8`、`int16` 一直到 `int256` 用于存储 8 位、16 位，一直到 256 位的有符号整数。`uint` 和 `int` 是 `uint256` 和 `int256` 的别名。`ufixed` 和 `fixed`代表分数。`ufixed0x8`、`ufixed0x16`，一直到 `ufixed0x256` 用于存储 8 位、16 位，一直到 256 位的无符号分数。类似地，`fixed0x8`、`fixed0x16`，一直到 `fixed0x256` 用于存储 8 位、16 位，一直到 256 位的有符号分数。如果我们有一个需要超过 256 位的数字，那么将使用 256 位数据类型，此时将存储数字的近似值。

+   地址（Address）用于存储最多 20 字节的值，通过分配十六进制字面量。它用于存储以太坊地址。您可以在 Solidity 中使用 `0x` 前缀，将十六进制编码的值赋给变量。

# 数组

Solidity 支持通用和字节数组，固定大小和动态数组，以及多维数组。

`bytes1`、`bytes2`、`bytes3`，一直到 `bytes32` 都是字节数组的类型。我们将使用字节表示 `bytes1`。

这是一些通用数组语法的示例：

```
 contract sample{
//dynamic size array
//wherever an array literal is seen a new array is created. If the //array literal is in state, then it's stored in storage and if it's //found inside function, then its stored in memory
//Here myArray stores [0, 0] array. The type of [0, 0] is decided based //on its values.
//Therefore, you cannot assign an empty array literal.
     int[] myArray = [0, 0];

    function sample(uint index, int value){
         //index of an array should be uint256 type
         myArray[index] = value;

         //myArray2 holds pointer to myArray
         int[] myArray2 = myArray;
//a fixed size array in memory
//here we are forced to use uint24 because 99999 is the max value and //24 bits is the max size required to hold it.
//This restriction is applied to literals in memory because memory is //expensive. As [1, 2, 99999] is of type uint24, myArray3 also has to //be the same type to store pointer to it.
         uint24[3] memory myArray3 = [1, 2, 99999]; //array literal

//throws exception while compiling as myArray4 cannot be assigned to //complex type stored in memory
         uint8[2] myArray4 = [1, 2];
     }
}
```

以下是您应该了解的一些关于数组的重要事项：

+   数组还具有 `length` 属性，可用于查找数组的长度。还可以将 `value` 分配给 `length` 属性以更改数组大小。但是，内存中的数组或非动态数组无法调整大小。

+   如果尝试访问动态数组的未设置的 `index`，则会引发异常。

# 字符串

在 Solidity 中，可以通过两种方式创建字符串：使用 `bytes` 和 `string`。`bytes` 用于创建原始字符串，而 `string` 用于创建 UTF-8 字符串。字符串的长度始终是动态的。

以下是显示 `string` 语法的示例：

```
contract sample {
//wherever a string literal is seen, a new string is created. If the //string literal is in state, then it's stored in storage and if it's //found inside function, then its stored in memory
     //Here myString stores "" string.
     string myString = ""; //string literal
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

Solidity 结构体。以下是 `struct` 语法的示例：

```
contract sample{
     struct myStruct {
         bool myBool;
         string myString;
     }

     myStruct s1;

    //wherever a struct method is seen, a new struct is created. If 
    //the struct method is in state, then it's stored in storage
    //and if it's found inside function, then its stored in memory
     myStruct s2 = myStruct(true, ""); //struct method syntax

     function sample(bool initBool, string initString){
         //create an instance of struct
         s1 = myStruct(initBool, initString);

        //myStruct(initBool, initString) creates an instance in memory
         myStruct memory s3 = myStruct(initBool, initString);
     }
}
```

# 枚举

Solidity 枚举。以下是 `enum` 语法的示例：

```
contract sample {
     //The integer type which can hold all enum values and is the
     //smallest is chosen to hold enum values
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

**哈希表** 是一种映射数据类型。由于映射只能存在于存储中，因此它们被声明为状态变量。您可以将映射看作具有 `key` 和 `value` 对的数据结构。`key` 实际上不会被存储；相反，将 `key` 的 **keccak256** 哈希用于查找 `value`。映射没有长度，并且无法分配给另一个映射。

以下是创建和使用 `mapping` 的示例：

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

# delete 运算符

`delete` 运算符可以应用于任何变量以将其重置为其默认值。默认值是所有位都分配为零。

如果我们对动态数组应用 `delete`，它将删除所有元素并使长度变为零。如果我们对静态数组应用 `delete`，它的所有索引都会被重置。我们也可以对特定的索引应用 `delete`，以重置它们。

然而，如果您将 `delete` 应用于映射类型，则不会发生任何事情。但是，如果您将 `delete` 应用于映射的 `key`，则与 `key` 关联的值将被删除。

让我们看看 `delete` 运算符的工作原理，如下所示：

```
contract sample { 

    struct Struct { 
        mapping (int => int) myMap; 
        int myNumber; 
    } 

    int[] myArray; 
    Struct myStruct; 

    function sample(int key, int value, int number, int[] array) { 

        //maps cannot be assigned so while constructing struct we
        // ignore the maps 
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

除了数组、字符串、结构体、枚举和映射之外的所有内容都被称为 **基本类型**。

如果我们对不同类型的操作数应用运算符，编译器会尝试将其中一个操作数隐式转换为另一个操作数的类型。一般来说，如果在语义上有意义且不会丢失信息，那么值类型之间的隐式转换是可能的：`uint8` 可转换为 `uint16`，`int128` 可转换为 `int256`，但 `int8` 无法转换为 `uint256`（因为 `uint256` 无法容纳，例如，`-1`）。此外，无符号整数可以转换为相同或更大尺寸的字节，但反之则不行。任何可以转换为 `uint160` 的类型也可以转换为地址。

Solidity还支持显式转换。如果编译器不允许两种数据类型之间的隐式转换，您可以选择显式转换。我们建议避免显式转换，因为它可能会给您带来意外的结果。

显式转换的以下示例，如下所示：

```
uint32 a = 0x12345678; 
uint16 b = uint16(a); // b will be 0x5678 now 
```

在这里，我们将`uint32`类型显式转换为`uint16`，即将大类型转换为小类型；因此，高阶位被截断。

# 使用var

Solidity提供了`var`关键字来声明变量。在这种情况下，变量的类型是动态决定的，取决于分配给它的第一个值。一旦分配了一个值，类型就固定了；如果您给它分配另一种类型，它将导致类型转换。

让我们看看`var`的工作原理，如下所示：

```
int256 x = 12;

//y type is int256 
var y = x;

uint256 z= 9;

//exception because implicit conversion not possible 
y = z;
```

请注意，在定义数组和映射时不能使用`var`。它也不能用于定义函数参数和状态变量。

# 控制结构

Solidity支持`if...else`，`do...while`，`for`，`break`，`continue`，`return`和`?:`控制结构。

这里有一个结构：

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

# 使用new操作符创建合同

合同可以使用`new`关键字创建新的合同。必须了解被创建合同的完整代码。

让我们演示一下，如下所示：

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

在某些情况下，异常会自动抛出。您可以使用`assert()`，`revert()`和`require()`来抛出手动异常。异常会停止并撤销当前正在执行的调用（即，对状态和余额的所有更改都将被撤消）。在Solidity中，尚不可能捕获异常。

以下三行是Solidity中抛出异常的不同方式：

```
if(x != y) { revert(); }

//In assert() and require(), the conditional statement is an inversion //to "if" block's condition, switching the comparison operator != to ==
assert(x == y);
require(x == y);
```

`assert()`将消耗所有gas，而`require()`和`revert()`将退还剩余的gas。

Solidity不支持返回异常的原因，但预计会很快支持。您可以访问[https://github.com/ethereum/solidity/issues/1686](https://github.com/ethereum/solidity/issues/1686)问题进行更新。然后，您将能够编写`revert("Something bad happened")`和`require(condition, "Something bad happened")`。

# 外部函数调用

Solidity有两种类型的函数调用：内部和外部。内部函数调用是指函数调用同一合同中的另一个函数。外部函数调用是指函数调用另一个合同的函数。

以下是一个示例：

```
contract sample1 
{ 
    int a; 

    //"payable" is a built-in modifier 
    //This modifier is required if another contract is sending 
    // Ether while calling the method 
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

        //hello() is internal call whereas this.hello() is 
        external call 
        this.hello(); 

        //pointing a contract that's already deployed 
        sample1 s2 = sample1(addressOfContract); 

        s2.makePayment(112); 

    } 
}
```

使用`this`关键字进行的调用称为外部调用。函数内部的`this`关键字表示当前合同实例。

# 合同的特性

现在是深入研究合同的时候了。让我们从一些新特性开始，然后我们将更深入地了解我们已经看到的特性。

# 可见性

状态变量或函数的可见性定义了谁可以看到它。可见性有四种类型：`external`，`public`，`internal`和`private`。

默认情况下，函数的可见性为`public`，状态变量的可见性为`internal`。让我们看看这些可见性函数意味着什么：

+   `external`：外部函数只能从其他合约或通过交易调用。例如，我们无法在内部调用一个`f`外部函数：`f()`将不起作用，但`this.f()`会起作用。我们也不能将`external`可见性应用于状态变量。

+   `public`：公共函数和状态变量可以以各种方式访问。编译器生成的访问器函数都是`public`状态变量。我们不能创建自己的访问器。实际上，它只生成**getter**，而不是**setter**。

+   `internal`：内部函数和状态变量只能在内部访问，即在当前合约和继承它的合约中。我们不能使用`this`来访问它。

+   `private`：私有函数和状态变量与内部函数类似，只是不能被继承合约访问。

这是一个代码示例，用于演示可见性和访问器：

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

我们已经看到了函数修饰符是什么，并且我们编写了一个基本版本。现在让我们详细看一下它。

修饰符由子合约继承，并且它们也可以被子合约覆盖。可以通过在空格分隔的列表中指定它们来向函数应用多个修饰符，并且它们将按顺序进行评估。您还可以向修饰符传递参数。

在修饰符内部，下一个修饰符主体或函数主体，以后出现的，被插入到`_;`出现的位置。

让我们看一个函数修饰符的复杂代码示例，如下所示：

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

这是`myFunction()`的执行方式：

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

在这里，当你调用`myFunction`方法时，它将返回`0`。但之后，当你尝试访问状态变量`a`时，你将得到`8`。

`return` 在修饰符或函数体中立即离开整个函数，返回值被分配给需要的变量。

在函数的情况下，`return`后的代码在调用方的代码执行完成后执行。而在修饰符的情况下，在前一个修饰符中的`_;`后的代码在调用方的代码执行完成后执行。在上述示例中，第五、六和七行永远不会执行。在第四行之后，执行直接从第八到第十行开始。

修饰符内部的`return`不能与任何值关联。它总是返回零位。

# 回退函数

**回退函数**是合约可以拥有的唯一无名称的函数。此函数不能有参数，也不能返回任何内容。如果没有其他函数匹配给定的函数标识符，则在调用合约时执行该函数。

这个函数也会在合约在没有任何函数调用的情况下接收以太币时执行；也就是说，交易将以太币发送到合约并不调用任何方法。在这样的情况下，通常只有很少的气体可用于函数调用（精确地说是 2,300 气体），因此将回退函数尽可能地廉价是很重要的。

当合约收到以太币但没有定义回退函数时，它们会抛出异常，从而将以太币退回。因此，如果你希望你的合约接收以太坊，你必须实现一个回退函数。

下面是一个回退函数的例子：

```
contract sample 
{ 
    function() payable 
    { 
        //Note how much Ether has been sent and by whom 
    } 
} 
```

# 继承

Solidity 支持通过复制代码来实现多重继承，包括多态性。即使一个合约从多个其他合约继承，区块链上也只会创建一个合约。此外，父合约的代码始终会被复制到最终的合约中。

让我们来回顾一个继承的例子：

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
//Note that sample1 is also a parent of sample2; yet there is only a
// single instance of sample1 
contract sample4 is sample1, sample2 
{ 
    function a(){} 

    function c(){ 

        //this executes the "a" method of sample3 contract 
        a(); 

        //this executes the "a" method of sample1 contract 
        sample1.a(); 

        //calls sample2.b() because it is last in the parent 
        contracts list and therefore it overrides sample1.b() 
        b(); 
    } 
} 

//If a constructor takes an argument, it needs to be provided at 
//the constructor of the child contract. 
//In Solidity, child constructor does not call parent constructor,
// instead parent is initialized and copied to child 
contract sample5 is sample3(122) 
{ 

} 
```

# 超级关键字

`super` 关键字用于引用最终继承链中的下一个合约。以下是一个例子，帮助你更好地理解：

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

关于 `sample6` 合约的最终继承链是 `sample6`, `sample5`, `sample4`, `sample2`, `sample3`, `sample1`。继承链以最派生的合约开始，以最不派生的合约结束。

# 抽象合约

抽象合约是仅包含函数原型而不包含实现的合约。它们无法被编译（即使它们包含了已实现的函数和未实现的函数）。如果一个合约继承自一个抽象合约并且没有通过覆盖实现所有未实现的函数，那么它本身也会变成抽象的。

提供抽象合约的原因是为了让编译器知道接口。这对于引用已部署的合约并调用其函数是有用的。

让我们通过下面的例子来演示：

```
contract sample1 
{ 
    function a() returns (int b); 
} 

contract sample2 
{ 
    function myFunc() 
    { 
        sample1 s = 
          sample1(0xd5f9d8d94886e70b06e474c3fb14fd43e2f23970); 

        //without abstract contract this wouldn't have compiled 
        s.a(); 
    } 
}
```

# 库

库与合约类似，但它们只在特定地址部署一次，它们的代码被各种合约重复使用。这意味着如果库函数被调用，它们的代码会在调用合约的上下文中执行；因此，`this` 指向调用合约，并且特别允许访问调用合约的存储。由于库是一个孤立的源代码片段，它只能在显式提供它们的情况下访问调用合约的状态变量（否则它无法命名它们）。

库可以包含结构体和枚举，但它们不能有状态变量。它们不支持继承，也不能接收以太币。

一旦 Solidity 库被部署到区块链上，任何人都可以使用它，只要知道它的地址并且有源代码（只有原型或完整实现）。Solidity 编译器需要源代码，以便确保正在访问的方法确实存在于库中。

下面是一个示例：

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

库的地址无法添加到合约源代码中。我们需要在编译期间向编译器提供库地址。

库有许多用例。两个主要用例如下：

+   如果你有几个合约具有一些公共代码，你可以将该公共代码作为库部署。这将节省燃气，这也取决于合约的大小。因此，我们可以将库视为使用它的合约的基础合约。使用基础合约而不是库来拆分公共代码将不会节省燃气，因为 Solidity 中的继承是通过复制代码实现的。因为库被认为是基础合约，所以库中具有内部可见性的函数会被复制到使用它的合约中。否则，具有库内部可见性的函数无法被使用库的合约调用，因为需要进行外部调用。具有内部可见性的函数无法使用外部调用调用。此外，库中的结构和枚举会被复制到使用库的合约中。

+   库可以用来为数据类型添加成员函数。

仅包含内部函数和/或结构/枚举的库不需要部署，因为库中的所有内容都会复制到使用它的合约中。

# 使用 `for`

`using A for B;` 指令可用于将库函数（来自库 `A`）附加到任何类型 `B` 上。这些函数将以调用它们的对象作为第一个参数。

`using A for *;` 的效果是将库 `A` 的函数附加到所有类型上。

下面是一个演示 `for` 的示例：

```
library math 
{ 
    struct myStruct1 { 
        int a; 
    } 

    struct myStruct2 { 
        int a; 
    } 

    //Here we have to make 's' location storage so that we 
    //get a reference. 
    //Otherwise addInt will end up accessing/modifying a 
    //different instance of myStruct1 than the one on which its invoked 
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

        //compiler error as the first parameter of addInt is
        //of type myStruct1 so addInt is not attached to myStruct2 
        s2.addInt(1); 
    } 
} 
```

# 返回多个值

Solidity 允许函数返回多个值。让我们演示一下，如下所示：

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

Solidity 允许一个源文件导入其他源文件。下面是一个示例来演示这一点：

```
//This statement imports all global symbols from "filename" (and //symbols imported therein) to the current global scope. "filename" can //be an absolute or relative path. It can only be an HTTP URL 
//import "filename"; 

//creates a new global symbol symbolName whose members are all the //global symbols from "filename". 
import * as symbolName from "filename"; 

//creates new global symbols alias and symbol2 which reference symbol1 //and symbol2 from "filename", respectively. 
import {symbol1 as alias, symbol2} from "filename"; 

//this is equivalent to import * as symbolName from "filename";. 
import "filename" as symbolName; 
```

# 全局可用变量

有一些特殊的变量和函数总是全局存在。我们将在接下来的章节中讨论它们。

# 区块和交易属性

区块和交易属性如下所示：

+   `block.blockhash(uint blockNumber) returns (bytes32)`: 给定区块的哈希仅适用于最近的 256 个区块。

+   `block.coinbase (address)`: 当前区块的矿工地址。

+   `block.difficulty (uint)`: 当前区块的难度。

+   `block.gaslimit (uint)`: 当前区块的燃气限制。它定义了整个区块中所有交易允许消耗的最大燃气量。其目的是保持区块传播和处理时间低，从而使网络足够去中心化。矿工有权将当前区块的燃气限制设置为上一个区块燃气限制的~0.0975%（1/1,024），因此结果燃气限制应该是矿工偏好的中位数。

+   `block.number (uint)`: 当前区块的编号。

+   `block.timestamp (uint)`: 当前区块的时间戳。

+   `msg.data (字节)`: 完整的调用数据包含了交易调用的函数及其参数。

+   `msg.gas (uint)`: 剩余的 gas。

+   `msg.sender (地址)`: 消息的发送者（当前调用）。

+   `msg.sig (bytes4)`: 调用数据的前四个字节（函数标识符）。

+   `msg.value (uint)`: 随消息发送的 wei 的数量。

+   `now (uint)`: 当前区块的时间戳（别名为 block.timestamp）。

+   `tx.gasprice (uint)`: 交易的 gas 价格。

+   `tx.origin (地址)`: 交易的发送者（完整的调用链）。

# 地址类型相关的变量

地址类型相关的变量如下：

+   `<address>.balance (uint256)`: 地址中 wei 的余额。

+   `<address>.send(uint256 金额) returns (bool)`: 将指定金额的 wei 发送到`地址`；失败时返回`false`。即使执行失败，当前合同也不会因异常而停止。

+   `<address>.transfer(uint256 金额)`: 向地址发送 wei。如果执行耗尽 gas 或失败，则以太转账将被撤销，并且当前合同将因异常而停止。

# 合同相关的变量

合同相关的变量如下：

+   `this`: 当前合同，可以显式转换为地址类型

+   `selfdestruct(address 收款人)`: 销毁当前合同，并将其资金发送到指定地址。

# 以太单位

字面数字可以附加`wei`、`finney`、`szabo`或`ether`后缀，以在以太币的子单位之间进行转换，其中以太币货币数字没有后缀被假定为 wei。例如，`2 Ether == 2000 finney`计算结果为`true`。

# 存在性、完整性和所有权证明合同

如今，企业正在使用电子签名解决方案签署协议。然而，这些文件的详细信息存储在可以轻松更改的数据库中，因此不能用于审计目的。区块链可以通过将区块链集成为这些电子签名系统的解决方案来解决此问题。

让我们编写一个 Solidity 合同，可以证明文件所有权而不泄露实际文件。它可以证明文件在特定时间存在，并检查文件的完整性。

企业可以使用此解决方案在区块链上存储其协议的哈希。这样做的好处是可以证明协议的日期/时间、协议的实际条款等。  

我们将通过将文件的哈希和所有者的名称存储为对来实现所有权的证明。所有者可以是创建协议的企业。另一方面，我们将通过将文件的哈希和区块时间戳存储为对来实现存在性的证明。最后，存储哈希本身证明了文件的完整性。如果文件被修改，其哈希将更改，合同将无法找到文件，从而证明文件已被修改。

我们将使用Quorum的私有交易，因为实体之间签署的协议对它们是私有的，细节不会暴露给其他实体。尽管只有文件的哈希将被暴露，但其他实体知道一个实体签署了多少协议仍然不是一个好主意。

以下是实现所有这些的智能合约代码：

```
contract Proof 
{ 
 struct FileDetails 
 { 
 uint timestamp; 
 string owner; 
 } 

 mapping (string => FileDetails) files; 

 event logFileAddedStatus(bool status, uint timestamp, 
   string owner, string fileHash); 

 //this is used to store the owner of file at the block timestamp 
 function set(string owner, string fileHash) 
 { 
 //There is no proper way to check if a key already exists, 
 //therefore we are checking for default value i.e., all bits are 0 
 if(files[fileHash].timestamp == 0) 
 { 
 files[fileHash] = FileDetails(block.timestamp, owner); 

 //we are triggering an event so that the frontend of our app
 //knows that the file's existence and ownership 
 //details have been stored 
 logFileAddedStatus(true, block.timestamp, owner, fileHash); 
 } 
 else 
 { 
 //this tells the frontend that the file's existence and 
 //ownership details couldn't be stored because the 
 //file's details had already been stored earlier 
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

以太坊提供了solc编译器，该编译器提供了一个命令行界面来编译`.sol`文件。访问[http://solidity.readthedocs.io/en/develop/installing-solidity.html#binary-packages](http://solidity.readthedocs.io/en/develop/installing-solidity.html#binary-packages)获取安装说明，并访问[https://Solidity.readthedocs.io/en/develop/using-the-compiler.html](https://Solidity.readthedocs.io/en/develop/using-the-compiler.html)获取使用说明。我们不会直接使用solc编译器；相反，我们将使用浏览器Solidity。浏览器Solidity是一个适用于小型合约的集成开发环境（IDE）。

现在，让我们使用浏览器Solidity编译上述合约。了解更多信息，请访问[https://Ethereum.github.io/browser-Solidity/](https://ethereum.github.io/browser-solidity/)。您还可以下载用于离线使用的浏览器Solidity源代码：[https://github.com/Ethereum/browser-Solidity/tree/gh-pages](https://github.com/Ethereum/browser-Solidity/tree/gh-pages)。

使用浏览器Solidity的一个主要优势是它提供了一个编辑器，并生成部署合约的代码。

在编辑器中，复制并粘贴上述合约代码。您会看到它编译并给出了使用Geth交互式控制台部署它的web3.js代码。

没有`privateFor`属性时，您将获得以下输出：

```
var proofContract = web3.eth.contract([{"constant":false,"inputs":[{"name":"fileHash","type":"string"}],"name":"get","outputs":[{"name":"timestamp","type":"uint256"},{"name":"owner","type":"string"}],"payable":false,"type":"function"},{"constant":false,"inputs":[{"name":"owner","type":"string"},{"name":"fileHash","type":"string"}],"name":"set","outputs":[],"payable":false,"type":"function"},{"anonymous":false,"inputs":[{"indexed":false,"name":"status","type":"bool"},{"indexed":false,"name":"timestamp","type":"uint256"},{"indexed":false,"name":"owner","type":"string"},{"indexed":false,"name":"fileHash","type":"string"}],"name":"logFileAddedStatus","type":"event"}]); 
var proof = proofContract.new( 
  { 
    from: web3.eth.accounts[0], 
    data: '0x606060.......', 
    gas: 4700000,
  privateFor: ['CGXyBlYOGgU4fZ7n8dVLaTW24p+ZOF8kSiUJkQCUABk=',
    'zumojc44Dge0juFgph4xzqOUyNVw+QNZUaY7wOL0P0o='] 
  }, function (e, contract){ 
   console.log(e, contract); 
  if (typeof contract.address !== 'undefined') { 
    console.log('Contract mined! address: ' + contract.address + '
      transactionHash: ' + contract.transactionHash); 
  } 
}) 
```

`data`表示合约的编译版本（字节码），以太坊虚拟机（EVM）可以理解。源代码首先被转换为操作码，然后转换为字节码。每个操作码都与`gas`相关联。

`web3.eth.contract`的第一个参数是ABI定义。ABI定义包含所有方法的原型，并在创建交易时使用。

现在是部署智能合约的时候了。在进一步操作之前，请确保您启动了我们在上一章中创建的由三个节点组成的raft网络。我们将假设这三个节点来自三个不同的企业。还要确保您已启用constellation，并复制所有constellation成员的公钥。在`privateFor`数组中，用您生成的公钥替换它们。在这里，我将私有智能合约对所有三个网络成员可见。

`privateFor` 仅在发送私有事务时使用。它被分配给一个接收者的 base64 编码的公钥数组。在上述代码中，在 `privateFor` 数组中，我只有两个公钥。这是因为发送者不必将其公钥添加到数组中。如果添加，那么将会引发错误。

在第一个节点的交互式控制台中，使用 `personal.unlockAccount(web3.eth.accounts[0], "", 0)` 来无限期地解锁以太坊账户。

在浏览器 Solidity 的右侧面板中，复制 web3 部署文本区域中的所有内容，然后添加 `privateFor` 并将其粘贴到第一个节点的交互式控制台中。现在按 *Enter* 键。您将首先获得事务哈希，等待一段时间后，事务被挖掘后您将获得合同地址。事务哈希是事务的哈希值，对于每个事务都是唯一的。每个部署的合同都有一个唯一的合同地址，用于在区块链中标识合同。

合同地址是从其创建者的地址（`from` 地址）和创建者发送的事务数量（事务 nonce）确定地计算出来的。这两个值都是 RLP 编码然后使用 keccak256 哈希算法进行哈希。我们将在后面了解更多关于事务 nonce 的内容。您可以在 [https://github.com/Ethereum/wiki/wiki/RLP](https://github.com/Ethereum/wiki/wiki/RLP) 了解更多关于**递归长度前缀**（**RLP**）的信息。

现在让我们存储文件详细信息并检索它们。假设前两个实体已签署协议并希望将文件的详细信息存储在区块链上。将以下代码放置以广播一个事务以存储文件的详细信息：

```
var contract_obj = proofContract.at
  ("0x006c3e992b6e3f52e81560aa3ef6d66e1706b45c"); 
contract_obj.set.sendTransaction("Enterprise 1",
   "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855"
    { 
 from: web3.eth.accounts[0], 
 privateFor: ['CGXyBlYOGgU4fZ7n8dVLaTW24p+ZOF8kSiUJkQCUABk=']
}, function(error, transactionHash){ 
  if (!err) 
    console.log(transactionHash); 
}) 
```

在这里，用您获得的合同地址替换合同地址。`proofContract.at` 方法的第一个参数是合同地址。在这里，我们没有提供 gas，这样会自动计算。最后，由于这是前两个实体之间的协议，第一个实体正在使用第二个实体的公钥发送交易，我们在 `privateFor` 属性中有第二个实体的公钥。

现在运行此代码以查找文件的详细信息：

```
contract_obj.get.call("e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934c
  a495991b7852b855"); 
```

您将获得此输出：

```
[1477591434, "Owner Name"] 

```

调用方法用于在 EVM 上调用合同的方法以及当前状态。它不广播事务。要读取数据，我们不需要广播，因为我们将有自己的区块链副本。

如果在节点 3 中运行上述代码，则不会得到任何细节，因为数据对第三个实体不可见。但第一个和第二个节点可以读取细节。在接下来的章节中，我们将更多地了解 web3.js。

# 摘要

在本章中，我们学习了Solidity编程语言。我们学习了数据位置，数据类型以及合约的高级特性。我们还学习了编译和部署智能合约的最快最简单的方法。现在你应该能够轻松编写智能合约了。

在下一章中，我们将为智能合约构建一个前端，这将使得部署智能合约和运行交易变得容易。
