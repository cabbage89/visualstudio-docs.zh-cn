---
title: 调试器中的表达式 |微软文档
ms.date: 03/02/2020
ms.topic: conceptual
f1_keywords:
- vs.debug.expressions
helpviewer_keywords:
- expressions [debugger]
- debugging [Visual Studio], expressions
- expression evaluation, debugger evaluator
- native expression evaluation
- expression evaluators
- debugger, evaluating expressions
- debugging [Visual Studio], expression evaluation
- debugging [Visual Studio], variable evaluation
ms.assetid: 70f9b531-44c7-4d77-980d-5eddbf2bff41
author: mikejo5000
ms.author: mikejo
manager: jillfra
ms.workload:
- multiple
ms.openlocfilehash: 4ab66f288ad8442b6f2b5aab3499e2c1f3857632
ms.sourcegitcommit: 95f26af1da51d4c83ae78adcb7372b32364d8a2b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79301133"
---
# <a name="expressions-in-the-visual-studio-debugger"></a>可视化工作室调试器中的表达式
Visual Studio 调试器包括表达式计算器，当您在 **“快速监视”** 对话框、 **“监视”** 窗口或 **“即时”** 窗口中输入表达式时，这些计算器可以对其进行计算。 这些表达式计算器还可以在 **“断点”** 窗口和调试器中的许多其他位置使用。

以下各节介绍 Visual Studio 支持的语言的表达式评估限制。

## <a name="f-expressions-are-not-supported"></a>不支持 F# 表达式。
无法识别 F# 表达式。 如果正在调试 F# 代码，你需要在向调试器窗口或对话框中输入表达式之前，将表达式转换为 C# 语法。 当把表达式从 F# 转换到 C# 时，请务必记住 C# 使用 `==` 运算符来测试相等性，而 F# 使用单个 `=`。

## <a name="c-expressions"></a>C++ 表达式
有关将上下文运算符用于 C++ 中的表达式的信息，请参阅 [上下文运算符 (C++)](../debugger/context-operator-cpp.md)。

### <a name="unsupported-expressions-in-c"></a>不支持的 C++ 表达式

#### <a name="constructors-destructors-and-conversions"></a>构造函数、析构函数和转换
不能显式或隐式地为对象调用构造函数或析构函数。 例如，以下表达式显式调用构造函数并生成错误消息：

```C++
my_date( 2, 3, 1985 )
```

如果转换的目标是类，则不能调用转换函数。 这种转换涉及对象的构造。 例如，如果 `myFraction` 是 `CFraction`的实例，后者定义了转换函数运算符 `FixedPoint`，则以下表达式会导致错误：

```C++
(FixedPoint)myFraction
```

不能调用新运算符或删除运算符。 例如，下面的表达式不受支持：

```C++
new Date(2,3,1985)
```

#### <a name="preprocessor-macros"></a>预处理器宏
调试器中不支持预处理器宏。 例如，如果常量 `VALUE` 被声明为： `#define VALUE 3`，则不能使用 **监视** 窗口中的 `VALUE` 。 若要避免此限制，只要有可能就应将 `#define`替换为枚举和函数。

### <a name="using-namespace-declarations"></a>使用命名空间声明
不能使用 `using namespace` 声明。  若要访问一个类型名称或当前命名空间之外的变量，必须使用完全限定的名称。

### <a name="anonymous-namespaces"></a>匿名命名空间
不支持匿名命名空间。 如果你有以下代码，则不能将 `test` 添加到监视窗口：

```C++
namespace mars
{
    namespace
    {
        int test = 0;
    }
}
int main()
{
    // Adding a watch on test does not work.
    mars::test++;
    return 0;
}

```

### <a name="using-debugger-intrinsic-functions-to-maintain-state"></a><a name="BKMK_Using_debugger_intrinisic_functions_to_maintain_state"></a> 使用调试器内部函数来维护状态
利用调试器内部函数，您可以调用表达式中的某些 C/C++ 函数而不更改应用程序的状态。

调试器内部函数：

- 一定是安全的：执行调试器内部函数将不会中断将调试的进程。

- 在任何表达式中都是被允许的，甚至在不允许副作用和函数计算的方案中也是如此。

- 在无法进行正则函数调用的方案（例如，调试小型转储）中是可行的。

  此外，可利用调试器内部函数更方便地计算表达式。 例如， `strncmp(str, "asd")` 比 `str[0] == 'a' && str[1] == 's' && str[2] == 'd'`更易于写入断点条件中。 )

|区域|内部函数|
|----------|-------------------------|
|**字符串长度**|[斯特伦， wcslen](https://docs.microsoft.com/cpp/c-runtime-library/reference/strlen-wcslen-mbslen-mbslen-l-mbstrlen-mbstrlen-l)，[斯特伦， wcsnlen](https://docs.microsoft.com/cpp/c-runtime-library/reference/strnlen-strnlen-s)|
|**字符串比较**|[斯特康普， wcscmp，](https://docs.microsoft.com/cpp/c-runtime-library/reference/strcmp-wcscmp-mbscmp) [stricmp， wcsicmp，](https://docs.microsoft.com/cpp/c-runtime-library/reference/stricmp-wcsicmp) [_stricmp， _strcmpi， _wcsicmp， _wcscmpi，](https://docs.microsoft.com/cpp/c-runtime-library/reference/stricmp-wcsicmp-mbsicmp-stricmp-l-wcsicmp-l-mbsicmp-l)[斯特兰普， wcsncmp，](https://docs.microsoft.com/cpp/c-runtime-library/reference/strncmp-wcsncmp-mbsncmp-mbsncmp-l)[斯特尼普， wcsnicmp，](https://docs.microsoft.com/cpp/c-runtime-library/reference/strnicmp-wcsnicmp) [_strnicmp， _wcsnicmp](https://docs.microsoft.com/cpp/c-runtime-library/reference/strnicmp-wcsnicmp-mbsnicmp-strnicmp-l-wcsnicmp-l-mbsnicmp-l)|
|**字符串搜索**|[斯特赫， wcschr](https://docs.microsoft.com/cpp/c-runtime-library/reference/strchr-wcschr-mbschr-mbschr-l)， [memchr， wmemchr，](https://docs.microsoft.com/cpp/c-runtime-library/reference/memchr-wmemchr)[斯特斯特， wcsstr](https://docs.microsoft.com/cpp/c-runtime-library/reference/strstr-wcsstr-mbsstr-mbsstr-l)|
|**Win32**|[副解码代理](https://docs.microsoft.com/windows/win32/api/combaseapi/nf-combaseapi-codecodeproxy)，[解码指针](https://docs.microsoft.com/previous-versions/bb432242%28v%3dvs.85%29)，[获取错误](https://docs.microsoft.com/windows/win32/api/errhandlingapi/nf-errhandlingapi-getlasterror)， [TlsGetValue](https://docs.microsoft.com/windows/win32/api/processthreadsapi/nf-processthreadsapi-tlsgetvalue)|
|**视窗 8**|[RoInspect 捕获的堆栈回溯](https://docs.microsoft.com/windows/win32/api/roerrorapi/nf-roerrorapi-roinspectcapturedstackbacktrace)， [Windows 比较弦乐 ，](https://docs.microsoft.com/windows/win32/api/winstring/nf-winstring-windowscomparestringordinal) [WindowsGetStringLen](https://docs.microsoft.com/windows/win32/api/winstring/nf-winstring-windowsgetstringlen)， [WindowsGetStringrawBuffer](https://docs.microsoft.com/windows/win32/api/winstring/nf-winstring-windowsgetstringrawbuffer)<br /><br /> 这些函数要求将调试的进程运行于 Windows 8 上。 调试从 Windows 8 设备生成的转储文件还要求 Visual Studio 计算机运行的是 Windows 8。 但是，如果远程调试 Windows 8 设备，则 Visual Studio 计算机可运行 Windows 7。|
|**杂项**|__log2 // 返回指定整数的日志基础 2，四舍五入到最接近的下整数。<br /><br />__findNonNull、解码HString、解码温特限制异常、动态广播、动态成员查找、GetEnvBlock 长度<br /><br />Stdext_HashMap_Int_OperatorBracket_idx，Std_UnorderedMap_Int_OperatorBracket_idx<br /><br />ConcurrencyArray_OperatorBracket_idx // 并发：：数组<>：：运算符[索引<>]和运算符（索引<>）<br /><br />ConcurrencyArray_OperatorBracket_int // 并发：数组<>：运算符（int，int，...）<br /><br />ConcurrencyArray_OperatorBracket_tidx // 并发：：数组<>：：运算符[tiled_index<>]和运算符（tiled_index<>）<br /><br />ConcurrencyArrayView_OperatorBracket_idx // 并发：array_view<>：：运算符[索引<>]和运算符（索引<>）<br /><br />ConcurrencyArrayView_OperatorBracket_int // 并发：array_view<>：运算符（int，int，...）<br /><br />ConcurrencyArrayView_OperatorBracket_tidx // 并发：：array_view<>：：操作员[tiled_index<>]和操作员（tiled_index<>）<br /><br />TreeTraverse_Init // 初始化新树遍历<br /><br />TreeTraverse_Next // 返回树中的节点<br /><br />TreeTraverse_Skip // 跳过挂起树遍历中的节点"|

## <a name="ccli---unsupported-expressions"></a>C++/CLI - 不支持的表达式

- 不支持包含指针的强制转换或用户定义的强制转换。

- 不支持对象比较和赋值。

- 不支持重载运算符和重载函数。

- 不支持装箱和取消装箱。

- 不支持`Sizeof` 运算符。

## <a name="c---unsupported-expressions"></a>C# - 不支持的表达式

### <a name="dynamic-objects"></a>动态对象
你可以使用静态类型化为动态的调试器表达式中的变量。 在"监视"<xref:System.Dynamic.IDynamicMetaObjectProvider>窗口中计算实现的对象时，将添加动态视图节点。 该动态视图节点显示对象成员，但不允许编辑成员的值。

不支持动态对象的下列功能：

- 复合运算符 `+=`、 `-=`、 `%=`、 `/=`和 `*=`

- 许多强制转换，包括数值强制转换和类型参数强制转换

- 带两个以上的参数的方法调用

- 带两个以上的参数的属性 getter

- 带参数的属性 setter

- 分配给索引器

- 布尔运算符 `&&` 和 `||`

### <a name="anonymous-methods"></a>匿名方法
不支持创建新的匿名方法。

## <a name="visual-basic---unsupported-expressions"></a>Visual Basic - 不支持的表达式

### <a name="dynamic-objects"></a>动态对象
你可以使用静态类型化为动态的调试器表达式中的变量。 在“监视”窗口中计算实现 <xref:System.Dynamic.IDynamicMetaObjectProvider> 的对象时，会添加一个“动态视图”节点。 该动态视图节点显示对象成员，但不允许编辑成员的值。

不支持动态对象的下列功能：

- 复合运算符 `+=`、 `-=`、 `%=`、 `/=`和 `*=`

- 许多强制转换，包括数值强制转换和类型参数强制转换

- 带两个以上的参数的方法调用

- 带两个以上的参数的属性 getter

- 带参数的属性 setter

- 分配给索引器

- 布尔运算符 `&&` 和 `||`

### <a name="local-constants"></a>局部常量
不支持局部常量。

### <a name="import-aliases"></a>导入别名
不支持导入别名。

### <a name="variable-declarations"></a>变量声明
不能在调试器窗口中声明显式的新变量。 但是，可以在“即时” **** 窗口中分配一个新的隐式变量。 这些隐式变量的范围限于调试会话并且无法在调试器外对其进行访问。 例如，语句 `o = 5` 隐式创建一个新变量 `o` ，并向其赋予值 5。 这样的隐式变量为 **对象** 类型，除非调试器可以推断该类型。

### <a name="unsupported-keywords"></a>不受支持的关键字

- `AddressOf`

- `End`

- `Error`

- `Exit`

- `Goto`

- `On Error`

- `Resume`

- `Return`

- `Select/Case`

- `Stop`

- `SyncLock`

- `Throw`

- `Try/Catch/Finally`

- `With`

- 命名空间或模块级的关键字，如 `End Sub` 或 `Module`。

## <a name="see-also"></a>另请参阅
- [C++ 中的格式说明符](../debugger/format-specifiers-in-cpp.md)
- [上下文运算符 (C++)](../debugger/context-operator-cpp.md)
- [格式规格在C#](../debugger/format-specifiers-in-csharp.md)
- [伪变量](../debugger/pseudovariables.md)
