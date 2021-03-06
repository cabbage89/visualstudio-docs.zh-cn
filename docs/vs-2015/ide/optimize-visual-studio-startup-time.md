---
title: 优化启动时间 |Microsoft Docs
ms.date: 11/15/2016
ms.topic: conceptual
helpviewer_keywords:
- startup time [Visual Studio]
- optimizing startup time [Visual Studio]
- speed up start time [Visual Studio]
ms.assetid: d1508121-8499-4084-8eb5-fa89fa7b17d3
caps.latest.revision: 5
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: 0f00bbc7741768852b5928b249dc7035bc440992
ms.sourcegitcommit: a8e8f4bd5d508da34bbe9f2d4d9fa94da0539de0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2019
ms.locfileid: "72670405"
---
# <a name="optimize-visual-studio-startup-time"></a>优化 Visual Studio 启动时间
[!INCLUDE[vs2017banner](../includes/vs2017banner.md)]

理想情况下，Visual Studio 应总是尽可能快地启动。 但是，Visual Studio 扩展和开启工具窗口会对启动时间产生负面影响，因为在启动时会自动加载它们。 通过**管理 Visual Studio 性能**窗口，不仅可以查看哪些扩展和功能影响 Visual Studio 的启动时间，还可以确定它们的加载行为，以便更好地控制 Visual Studio 的启动速度。

## <a name="control-startup-behavior"></a>控制启动行为

为了避免延长启动时间，Visual Studio 2017 和更高版本将在启动过程中使用按需加载方法避免加载扩展。 这意味着不会在 Visual Studio 启动后立即打开扩展，而是在启动后根据需要以异步方式打开。 此外，由于在之前的 Visual Studio 会话中保持工具窗口的打开状态会使启动时间变慢，因此 Visual Studio 以更智能的方式打开工具窗口，从而避免影响启动时间。

如果 Visual Studio 检测到启动速度较慢，则会弹出一条消息，提示你导致速度变慢的扩展或工具窗口。 此消息还提供了**管理 Visual Studio 性能**对话框的链接，此对话框中列出了影响启动性能的扩展和工具窗口。 通过此对话框可以更改扩展和工具窗口的设置以提高启动性能。

![管理 Visual Studio 性能-弹出窗口](../ide/media/vside-perfdialog-popup.PNG "管理 Visual Studio 性能-弹出窗口")

**管理 Visual Studio 性能**对话框有两个类别：**扩展**和**工具窗口**。

### <a name="control-extensions"></a>控制扩展
如果某个扩展使 Visual Studio 的启动变慢，那么选择它的一个扩展类型时，此扩展将显示在**管理 Visual Studio 性能**对话框中。 如果对启动时间的影响（在“影响”部分下面列出）很高，令人无法接受，则可以选择“禁用”按钮，以便在启动时始终禁用此扩展。 可以使用扩展管理器或“管理 Visual Studio 性能”对话框重新启用扩展，以用于以后的会话。

![管理 Visual Studio 性能-扩展](../ide/media/vside-perfdialog-extensions.PNG "管理 Visual Studio 性能-扩展")

除了启动扩展，你也可以禁用加载解决方案或当用户键入时加载的扩展。 只需选择场景以查看关联的扩展列表。

### <a name="control-tool-windows"></a>控制工具窗口
如果工具窗口使 Visual Studio 启动变慢，可以选择保留其默认行为（对启动速度没有任何帮助），或者选择以下两种行为之一来替代其默认行为：

- **启动时不显示窗口：** 如果选择此选项，则打开 Visual Studio 时指定的工具窗口将始终关闭，即使它在上一个会话中为打开状态。 可以从菜单打开工具窗口。
- **启动时自动隐藏窗口：** 如果工具窗口在上一个会话中保留打开状态，那么选择此选项将在启动时折叠工具窗口组，以避免初始化工具窗口。 如果你经常使用工具窗口，那么这是一个不错的选择。因为工具窗口仍然可用，但不会再对 Visual Studio 启动时间产生负面影响。

![管理 Visual Studio 性能-工具窗口](../ide/media/vside-perfdialog-toolwindows.PNG "管理 Visual Studio 性能-工具窗口")

如果你以后改变主意，可以在**管理 Visual Studio 性能**对话框中还原任何选项。 若要打开“管理 Visual Studio 性能”对话框，请在菜单栏上选择“帮助”->“管理 Visual Studio 性能”。
