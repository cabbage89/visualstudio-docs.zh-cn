---
title: 调试多线程应用程序
description: 使用 Visual Studio 中的 "线程" 窗口和 "调试位置" 工具栏进行调试
ms.date: 02/14/2020
ms.topic: conceptual
dev_langs:
- CSharp
- VB
- FSharp
- C++
helpviewer_keywords:
- multithreaded debugging, tutorial
- tutorials, multithreaded debugging
ms.assetid: adfbe002-3d7b-42a9-b42a-5ac0903dfc25
author: mikejo5000
ms.author: mikejo
manager: jillfra
ms.workload:
- multiple
ms.openlocfilehash: eb7b7850d8d7582110152d248683f89981933215
ms.sourcegitcommit: 6ef52c2030b37ea7a64fddb32f050ecfb77dd918
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/17/2020
ms.locfileid: "77416368"
---
# <a name="walkthrough-debug-a-multithreaded-app-using-the-threads-window-c-visual-basic-c"></a>演练：使用 "线程" 窗口调试多线程应用C#（Visual Basic、 C++）

多个 Visual Studio 用户界面元素可帮助调试多线程应用。 本文介绍 "代码编辑器" 窗口、"**调试位置**" 工具栏和 "**线程**" 窗口中的多线程调试功能。 有关用于调试多线程应用程序的其他工具的信息，请参阅[开始调试多线程应用程序](../debugger/get-started-debugging-multithreaded-apps.md)。

完成本教程只需几分钟的时间，熟悉你就会熟悉调试多线程应用程序的基础知识。

## <a name="create-a-multithreaded-app-project"></a>创建一个多线程应用项目

创建以下要在本教程中使用的多线程应用项目：

1. 打开 Visual Studio 并创建一个新项目。

   ::: moniker range=">=vs-2019"

   如果开始窗口未打开，请选择“文件” **“开始窗口”** >。

   在“开始”窗口上，选择“创建新项目”。

   在“创建新项目”窗口的搜索框中输入或键入“控制台”。 接下来， **C#** 从**C++** "语言" 列表中选择或，然后从平台列表中选择 " **Windows** "。 

   应用语言和平台筛选器后，选择 "**控制台应用（.net Core）** "，或者对于C++"**控制台应用**模板"，选择 "**下一步**"。

   > [!NOTE]
   > 如果看不到正确的模板，请转到 "**工具**" > **获取工具和功能 ...** "，这将打开 Visual Studio 安装程序。 选择“.NET 桌面开发”或“使用 C++ 的桌面开发”工作负载，然后选择“修改”。

   在 "**配置新项目**" 窗口中，在 "**项目名称**" 框中键入或输入*MyThreadWalkthroughApp* 。 然后，选择“创建”。

   ::: moniker-end
   ::: moniker range="vs-2017"
   从顶部菜单栏中选择“文件” > “新建” > “项目”。 在 "**新建项目**" 对话框的左窗格中，选择以下项：

   - 对于C#应用程序，在 **" C#视觉对象**" 下选择 " **Windows 桌面**"，然后在中间窗格中选择 "**控制台应用（.NET Framework）** "。
   - 对于C++应用程序，在 **" C++视觉对象**" 下，选择 " **windows 桌面**"，然后选择 " **windows 控制台应用程序**"。

   如果看不到**控制台应用（.Net Core）** 或C++**控制台应用**项目模板，请转到 "**工具**" > **获取工具和功能 ...** "，这将打开 Visual Studio 安装程序。 选择“.NET 桌面开发”或“使用 C++ 的桌面开发”工作负载，然后选择“修改”。

   然后，键入名称（例如*MyThreadWalkthroughApp* ），然后单击 **"确定"** 。

   选择“确定”。
   ::: moniker-end

   新的控制台项目随即显示。 创建该项目后，将显示源文件。 根据所选语言，源文件名称可能是 Program.cs、MyThreadWalkthroughApp.cpp 或 Module1.vb。

1. 将源文件中的代码替换为C# [开始调试多线程应用程序](../debugger/get-started-debugging-multithreaded-apps.md)中的或C++示例代码。

1. 选择“文件” **“全部保存”。**  > 

## <a name="start-debugging"></a>“启动调试”

1. 在源代码中查找以下行：

   ```csharp
   Thread.Sleep(3000);
   Console.WriteLine();
   ```

   ```C++
   Thread::Sleep(3000);
   Console.WriteLine();
   ```

1. 在 `Console.WriteLine();` 行上设置断点，方法是单击左侧的滚动条线，或选择线条并按**F9**。

   断点在代码行旁边的左侧滚动条中显示为红色圆圈。

1. 选择 "**调试**" > "**开始调试**"，或按**F5**。

   应用程序在调试模式下启动，并在断点处暂停。

1. 在中断模式下，通过选择 "**调试**" > **Windows** > **线程**打开 "**线程**" 窗口。 你必须在调试会话中才能打开或查看**线程**和其他调试窗口。

## <a name="examine-thread-markers"></a>检查线程标记

1. 在源代码中，找到 `Console.WriteLine();` 行。

   1. 在 "**线程**" 窗口中右键单击，然后选择菜单中的 "**在源**中![显示线程"](../debugger/media/dbg-multithreaded-show-threads.png "ThreadMarker") 。

   源代码行旁边的装订线现在显示一个*线程标记*图标![线程标记](../debugger/media/dbg-thread-marker.png "线程标记")。 线程标记指示线程在此位置停止。 如果该位置有多个已停止的线程，则会显示![多个线程](../debugger/media/dbg-multithreaded-show-threads.png "多个线程")图标。

1. 将指针悬停在线程标记上。 显示数据提示，并显示已停止的线程或线程的名称和线程 ID 号。 线程名称可能 `<No Name>`。

   >[!TIP]
   >为了帮助识别不需要的线程，您可以在 "**线程**" 窗口中重命名它们。 右键单击该线程，然后选择 "**重命名**"。

1. 右键单击源代码中的线程标记可查看快捷菜单上的可用选项。

## <a name="flag-and-unflag-threads"></a>标记线程和取消标记线程

您可以标记线程以跟踪您要特别注意的线程。

在源代码编辑器或 "**线程**" 窗口中标记和取消标记线程。 从 "**调试位置**" 或 "**线程**" 窗口工具栏中选择是仅显示标记的线程还是显示所有线程。 从任何位置进行的选择将影响所有位置。

### <a name="flag-and-unflag-threads-in-source-code"></a>在源代码中标记和取消标记线程

1. 通过选择 "**视图**" > **工具栏** > **调试位置**，打开 "**调试位置**" 工具栏。 还可以在工具栏区域中右键单击，然后选择 "**调试位置**"。

1. "**调试位置**" 工具栏有三个字段： "**进程**"、"**线程**" 和 "**堆栈帧**"。 下拉**线程**列表，并记下有多少线程。 在**线程**列表中，当前正在执行的线程由 **>** 符号标记。

1. 在源代码窗口中，将鼠标悬停在滚动条中的一个线程标记图标上，并在数据提示中选择标志图标（或一个空标志图标）。 标志图标变为红色。

   您还可以右键单击线程标记图标，指向 "**标志**"，然后从快捷菜单中选择要标记的线程。

1. 在 "**调试位置**" 工具栏上，选择 "**仅显示标记的线程**" 图标在 "**线程**" 字段的右侧![显示标记的线程](../debugger/media/dbg-threads-show-flagged.png "显示标记的线程")。 除非标记一个或多个线程，否则图标为灰显。

   只有已标记的线程才会出现在工具栏的 "**线程**" 下拉列表中。 若要再次显示所有线程，请再次选择 "**仅显示标记的线程**" 图标。

   >[!TIP]
   >标记了某些线程后，可以将光标放在代码编辑器中，右键单击，然后选择 "**将标记的线程运行到光标处**"。 请确保选择所有已标记的线程将达到的代码。 将**标记的线程运行到光标处**将暂停选定代码行上的线程，从而可以更轻松地通过[冻结和解冻线程](#bkmk_freeze)控制执行顺序。

1. 若要切换当前正在执行的线程的已标记或未标记状态，请选择 "**仅显示标记的线程**" 按钮左侧的单个标志 "**切换当前线程标记状态**" 工具栏按钮。 标记当前线程对于仅显示标记的线程时查找当前线程非常有用。

1. 若要取消标记线程，请将鼠标悬停在源代码中的线程标记上，并选择红色标记图标以清除它，或右键单击线程标记，然后选择 "取消**标记**"。

### <a name="flag-and-unflag-threads-in-the-threads-window"></a>在 "线程" 窗口中标记和取消标记线程

在 "**线程**" 窗口中，已标记的线程旁边显示红色标志图标，而未标记的线程（如果已显示）具有空图标。

![“线程”窗口](../debugger/media/dbg-threads-window.png "“线程”窗口")

选择标记图标，以根据其当前状态将线程状态更改为标记或未标记。

还可以右键单击行，然后从快捷菜单中选择 "**标记**"、"取消**标记**" 或 "取消**标记所有线程**"。

"**线程**" 窗口工具栏还具有 "**仅显示标记的线程**" 按钮，它是两个标志图标中的 righthand。 它的工作方式与 "**调试位置**" 工具栏上的按钮相同，其中的一个按钮控制两个位置的显示。

### <a name="other-threads-window-features"></a>其他线程窗口功能

在 "**线程**" 窗口中，选择任意列的标头以按该列对线程进行排序。 再次选择以反转排序顺序。 如果显示了所有线程，则选择标志图标列会按标记或未标记的状态对线程进行排序。

"**线程**" 窗口的第二列（没有标头）是**当前线程**列。 此列中的黄色箭头标记当前执行点。

"**位置**" 列显示每个线程在源代码中出现的位置。 选择**Location**项旁边的展开箭头，或将鼠标悬停在该项上，以显示该线程的部分调用堆栈。

>[!TIP]
>有关线程调用堆栈的图形视图，请使用 "[并行堆栈](../debugger/using-the-parallel-stacks-window.md)" 窗口。 若要在调试时打开窗口，请选择 "**调试**"> **Windows** > "**并行堆栈**"。

除了**标记** **、取消标记和**取消**标记所有线程**外，**线程**窗口项的右键单击上下文菜单还具有：

- "**在源中显示线程**" 按钮。
- **十六进制显示**，将 "**线程**" 窗口中的**线程 ID**更改为十进制格式。
- [切换到线程](#switch-to-another-thread)，这会立即将执行切换到该线程。
- **重命名**，使你可以更改线程名称。
- [冻结和解冻](#bkmk_freeze)命令。

## <a name="bkmk_freeze"></a>冻结和解冻线程执行

可以冻结和解冻线程，也可以挂起和恢复线程，以控制线程执行工作的顺序。 冻结和解冻线程可帮助解决并发性问题，如死锁和争用条件。

> [!TIP]
> 若要在不冻结其他线程的情况下跟踪单个线程，这也是一种常见的调试方案，请参阅[开始调试多线程应用程序](../debugger/get-started-debugging-multithreaded-apps.md#bkmk_follow_a_thread)。

**冻结和解冻线程：**

1. 在 "**线程**" 窗口中，右键单击任意线程，然后选择 "**冻结**"。 "**当前线程**" 列中的**暂停**图标指示线程已冻结。

1. 选择 "**线程**" 窗口工具栏中的**列**，然后选择 "**挂起的计数**" 以显示 "挂起的**计数**" 列。 冻结线程的挂起计数值为**1**。

1. 右键单击冻结的线程，然后选择 "**解冻**"。

   **暂停**图标消失，"挂起的**计数**" 值更改为**0**。

## <a name="switch-to-another-thread"></a>切换到另一个线程

尝试切换到另一个线程时，可能会看到**应用程序处于中断模式**窗口。 此窗口告诉您该线程没有当前调试器可以显示的任何代码。 例如，你可能正在调试托管代码，但线程是本机代码。 此窗口提供了解决此问题的建议。

**切换到另一个线程：**

1. 在 "**线程**" 窗口中，记下当前线程 ID （**当前线程**列中带有黄色箭头的线程）。 你需要切换回此线程以继续运行你的应用程序。

1. 右键单击其他线程，然后从上下文菜单中选择 "**切换到线程**"。

1. 观察 "**线程**" 窗口中的黄色箭头位置是否已更改。 原始的当前线程标记也保留为轮廓。

   查看代码源编辑器中的线程标记上的工具提示，以及 "**调试位置**" 工具栏上的 "**线程**" 下拉列表中的列表。 观察当前线程是否也已更改。

1. 在 "**调试位置**" 工具栏上，从 "**线程**" 列表中选择一个不同的线程。 请注意，当前线程还会在其他两个位置发生更改。

1. 在源代码编辑器中，右键单击线程标记，指向 "**切换到线程**"，然后从列表中选择另一个线程。 观察当前线程是否在所有三个位置发生了更改。

在源代码中，通过线程标记，只能切换到在该位置停止的线程。 使用“线程”窗口和“调试位置”工具栏可以切换到任何线程。

你现在已经了解了调试多线程应用程序的基础知识。 您可以使用 "**线程**" 窗口、"**调试位置**" 工具栏中的 "**线程**" 或 "源代码编辑器" 中的 "线程" 标记来观察、标记和取消标记线程，以及冻结和解冻线程。

## <a name="see-also"></a>另请参阅
- [调试多线程应用](../debugger/debug-multithreaded-applications-in-visual-studio.md)
- [如何：在调试时切换到另一个线程](../debugger/how-to-switch-to-another-thread-while-debugging.md)
