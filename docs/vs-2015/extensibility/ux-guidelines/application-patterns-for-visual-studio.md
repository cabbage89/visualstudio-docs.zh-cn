---
title: 应用程序模式
ms.date: 11/15/2016
ms.prod: visual-studio-dev14
ms.technology: vs-ide-sdk
ms.topic: conceptual
ms.assetid: 8ed68602-4e28-46fe-b39f-f41979b308a2
caps.latest.revision: 8
ms.author: gregvanl
manager: jillfra
ms.openlocfilehash: cc14aadfafb16fcae571ab66e5811ea465cb55a9
ms.sourcegitcommit: 95f26af1da51d4c83ae78adcb7372b32364d8a2b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79301307"
---
# <a name="application-patterns-for-visual-studio"></a>Visual Studio 的应用程序模式
[!INCLUDE[vs2017banner](../../includes/vs2017banner.md)]

## <a name="window-interactions"></a><a name="BKMK_WindowInteractions"></a>窗互

### <a name="overview"></a>概述
 Visual Studio 中使用的两种主要窗口类型是文档编辑器和工具窗口。 很少（但可能）是大型无模式对话框。 尽管这些在 shell 中都是无模式的，但他们的模式却截然不同。 本主题介绍文档窗口、工具窗口和无模式对话框之间的区别。 模式对话框模式在[对话框](../../extensibility/ux-guidelines/application-patterns-for-visual-studio.md#BKMK_Dialogs)中介绍。

### <a name="comparing-window-usage-patterns"></a>比较窗口使用模式
 **文档窗口**几乎总是显示在文档中。 这为文档编辑器提供了一个"中心阶段"来安排周围的补充工具窗口。

 **工具窗口**通常显示为一个单独的、更小的窗口（可以可见、隐藏或自动隐藏）折叠在 IDE 的边缘。 但是，有时通过取消选中窗口中**的窗口/停靠**属性，它们显示在文档中。 这会导致更多的房地产，但也一个共同的设计决策：当尝试集成到 Visual Studio 时，您必须决定您的功能是显示工具窗口还是文档窗口。

 在可视化工作室中不鼓励**无模式对话框**。 在很大程度上，它们（顾名思义）是浮动的工具窗口，应该这样实现。 如果停靠在外壳侧面的正常工具窗口的大小过于限制，则允许进行无模式对话框。 如果用户可能会将对话框移动到辅助监视器，则也允许它们。

 仔细考虑您需要哪种容器类型。 下表中提供了 UI 设计的常见使用模式注意事项。

||文档窗口|工具窗口|无模式对话框|
|-|---------------------|-----------------|---------------------|
|**位置**|始终放置在文档中，并且不停靠在 IDE 的边缘周围。 它可以"拉下"，以便它与主壳分开浮动。|通常，选项卡停靠在 IDE 的边缘，但可以自定义为浮动、自动隐藏（未固定）或停靠在文档中。|与 IDE 分开的大型浮动窗口。|
|**提交模型**|*延迟提交*<br /><br /> 为了将数据保存在文档中，用户必须发出"文件/保存、保存为"或"全部保存"命令。 文档窗口中的数据的概念是"已删除"，然后提交到其中一个保存命令。 关闭文档窗口时，所有内容都将保存到磁盘或丢失。|*立即提交*<br /><br /> 没有保存模型。 对于帮助编辑文件的检查器工具窗口，必须在活动编辑器或设计器中打开该文件，并且编辑器或设计器拥有保存。|*延迟或立即提交*<br /><br /> 通常，大型无模式对话框需要操作来提交更改，并允许执行"取消"操作，该操作回滚在对话框会话中所做的任何更改。  这将无模式对话框与该工具窗口中的工具窗口区分开来，该工具窗口中始终具有即时提交模型。|
|**可见性**|*打开/创建（文件）和关闭*<br /><br /> 打开文档窗口是通过打开现有文档或使用模板创建新文档来完成的。 没有"打开\<特定编辑器>"命令。|*隐藏和显示*<br /><br /> 可以隐藏或显示单实例工具窗口。 无论在视图中还是隐藏中，工具窗口中的内容和状态都保持不变。 可以关闭和隐藏多实例工具窗口。 关闭多实例工具窗口时，将丢弃工具窗口中的内容和状态。|*从命令启动*<br /><br /> 对话框从基于任务的命令启动。|
|**Instances**|*多实例*<br /><br /> 多个编辑器可以同时打开并编辑不同的文件，而某些编辑器还允许在多个编辑器中打开同一文件（使用 **"窗口>新窗口**"命令）。<br /><br /> 单个编辑器可能同时编辑一个或多个文件（项目设计器）。|*单实例或多实例*<br /><br /> 内容更改以反映上下文（如属性浏览器中）或将焦点/上下文推送到其他窗口（任务列表、解决方案资源管理器）。<br /><br /> 单实例和多实例工具窗口都应与活动文档窗口关联，除非有令人信服的理由不这样做。|*单实例*|
|**例子**|**文本编辑器**，如代码编辑器<br /><br /> **设计曲面**，如窗体设计器或建模曲面<br /><br /> **控件布局类似于对话框**，如清单设计器|**解决方案资源管理器**提供解决方案中包含的解决方案和项目<br /><br /> **服务器资源管理器**提供用户选择在窗口中打开的服务器和数据连接的分层视图。 从数据库层次结构（如查询）打开对象将打开文档窗口，并允许用户编辑查询。<br /><br /> **属性浏览器**显示在文档窗口或其他工具窗口中选择的对象的属性。 这些属性在分层网格视图或类似对话的复杂控件中显示，并允许用户设置这些属性的值。||

## <a name="tool-windows"></a><a name="BKMK_ToolWindows"></a>工具窗口

### <a name="overview"></a>概述
 工具窗口支持在文档窗口中发生的用户工作。 它们可用于显示表示 Visual Studio 提供并可以操作的基本根对象的层次结构。

 在 IDE 中考虑新工具窗口时，作者应：

- 使用适合任务的现有工具窗口，而不是创建具有类似功能的新工具窗口。 仅当新工具窗口提供无法集成到类似窗口中的显著不同的"工具"或功能，或者通过将现有窗口转换为枢轴中心，才应创建新工具窗口。

- 如果需要，请使用工具窗口顶部的标准命令栏。

- 与控件演示文稿和键盘导航的其他工具窗口中已有的模式保持一致。

- 与其他工具窗口中的控制演示文稿保持一致。

- 如果可能，特定于文档的工具窗口应自动可见，以便它们仅在激活父文档时才出现。

- 确保其窗口内容可通过键盘（支持箭头键）导航。

#### <a name="tool-window-states"></a>工具窗口状态
 Visual Studio 工具窗口具有不同的状态，其中一些状态是用户激活的（如自动隐藏功能）。 其他状态（如自动可见）允许工具窗口显示在正确的上下文中，并在不需要时隐藏。 共有五个工具窗口状态。

- **停靠/固定**工具窗口可以附加到文档区域的四个边中的任何一个。 图钉图标将显示在工具窗口标题栏中。 工具窗口可以沿壳体和其他工具窗口的边缘水平或垂直停靠，也可以与选项卡链接。

- **自动隐藏**的工具窗口将取消固定。 窗口可以滑出视线，在文档区域的边缘留下一个选项卡（带有工具窗口的名称及其图标）。 当用户悬停在选项卡上时，工具窗口会滑出。

- 当启动另一块 UI（如编辑器）或获得焦点时，自动**可见**工具窗口会自动显示。

- **浮动**工具窗口悬停在 IDE 外部。 这对于多监视器配置很有用。

- **选项卡式文档**工具窗口可以停靠在文档中。 这对于大型工具窗口（如对象浏览器）非常有用，这些窗口需要比停靠到框架允许的边缘更多的空间。

  ![Visual Studio 中的“工具”窗口状态](../../extensibility/ux-guidelines/media/0702-01-toolwindowstates.png "0702-01_ToolWindowStates")

  **Visual Studio 中的“工具”窗口状态**

#### <a name="single-instance-and-multi-instance"></a>单实例和多实例
 工具窗口是单实例或多实例。 某些单实例工具窗口可能与活动文档窗口相关联，而多实例工具窗口可能未关联。 多实例工具窗口通过创建窗口的新实例来响应窗口/新窗口命令。 下图演示了当窗口实例处于活动状态时启用"新建窗口"命令的工具窗口：

 ![Visual Studio 中启用“工具”窗口的命令](../../extensibility/ux-guidelines/media/0702-02-toolwindowenablingcommand.png "0702-02_ToolWindowEnablingCommand")

 **当窗口实例处于活动状态时启用"新窗口"命令的工具窗口**

 可以隐藏或显示单实例工具窗口，而多实例工具窗口可以关闭和隐藏。 所有工具窗口都可以停靠、选项卡链接、浮动或设置为多文档接口 （MDI） 子窗口（类似于文档窗口）。 所有工具窗口都应响应"窗口"菜单中的相应窗口管理命令：

 ![Visual Studio 中的窗口管理命令](../../extensibility/ux-guidelines/media/0702-03-windowmanagementcontrols.png "0702-03_WindowManagementControls")

 **可视化工作室窗口菜单中的窗口管理命令**

#### <a name="document-specific-tool-windows"></a>特定于文档的工具窗口
 某些工具窗口旨在根据给定的文档类型进行更改。 这些窗口不断更新以反映适用于 IDE 中活动文档窗口的功能。

 其内容更改以反映所选编辑器的工具窗口的示例包括"工具箱"和"文档大纲"。 当编辑器具有不向窗口提供上下文的焦点时，这些窗口将显示水印。

#### <a name="navigable-list-tool-windows"></a>导航列表工具窗口
 某些工具窗口显示用户可以与之交互的可导航项的列表。 在这种类型的窗口中，应始终对列表中的当前项进行反馈，即使窗口处于非活动状态也是如此。 列表应响应**GoToNextLocation**和**GoTotovlocation 命令**，同时更改窗口中当前选定的项

 可导航列表工具窗口的示例包括"解决方案资源管理器"和"查找结果"窗口。

### <a name="tool-window-types"></a>工具窗口类型

#### <a name="common-tool-windows-and-their-functions"></a>常用工具窗口及其功能

|类型|工具窗口|函数|
|----------|-----------------|--------------|
|**Hierarchy**|解决方案资源管理器|显示项目、杂项文件和解决方案项中包含的文档列表的分层树。 项目中项的显示由拥有项目类型的包定义（例如，基于引用、基于目录或混合模式的类型）。|
|**Hierarchy**|类视图|工作文档集中的类和各种元素的分层树，独立于文件本身。|
|**Hierarchy**|服务器资源管理器|显示解决方案中的所有服务器和数据连接的分层树。|
|**Hierarchy**|文档大纲|活动文档的层次结构。|
|**网 格**|属性|显示所选对象的属性列表以及用于编辑这些属性的值选取器的网格。|
|**网 格**|任务列表|允许用户创建/编辑/删除任务和注释的网格。|
|**内容**|帮助|一个窗口，允许用户访问各种方法来获取帮助，从"我如何？ 视频到MSDN论坛。|
|**内容**|动态帮助|一个工具窗口，用于显示帮助适用于当前选择的主题的链接。|
|**内容**|对象浏览器|两列框架集，在左窗格中包含分层对象组件的列表，在右列中包含对象的属性和方法。|
|**对话框**|查找，高级查找|允许用户在解决方案中的各种文件中查找或查找和替换的对话框。|
|**其他**|工具箱|用于存储将拖放到设计表面的元素的工具窗口，为所有设计器提供一致的拖动源。|
|**其他**|起始页|用户访问 Visual Studio 的门户，可以访问开发人员新闻、Visual Studio 帮助和最近项目的源。 用户还可以通过将 StartPage.xaml 文件从"通用 7_IDE_StartPages"Visual Studio 程序文件目录复制到 Visual Studio 文档目录中的 StartPages 文件夹，然后手动编辑 XAML，或手动编辑 XAML，然后创建自定义起始页在 Visual Studio 或其他代码编辑器中打开它。|
|**调试器：** 一组特定于调试任务和监视活动的窗口|自动||
|**调试器：** 一组特定于调试任务和监视活动的窗口|即时||
|**调试器：** 一组特定于调试任务和监视活动的窗口|输出|只要您要声明文本事件或状态，即可使用输出窗口。|
|**调试器：** 一组特定于调试任务和监视活动的窗口|内存||
|**调试器：** 一组特定于调试任务和监视活动的窗口|断点||
|**调试器：** 一组特定于调试任务和监视活动的窗口|正在运行||
|**调试器：** 一组特定于调试任务和监视活动的窗口|文档||
|**调试器：** 一组特定于调试任务和监视活动的窗口|调用堆栈||
|**调试器：** 一组特定于调试任务和监视活动的窗口|局部变量||
|**调试器：** 一组特定于调试任务和监视活动的窗口|手表||
|**调试器：** 一组特定于调试任务和监视活动的窗口|反汇编||
|**调试器：** 一组特定于调试任务和监视活动的窗口|寄存器||
|**调试器：** 一组特定于调试任务和监视活动的窗口|线程数||

## <a name="document-editor-conventions"></a><a name="BKMK_DocumentEditorConventions"></a>文档编辑器约定

### <a name="document-interactions"></a>文档交互
 "文档井"是 IDE 中最大的空间，用户通常集中注意力以完成其任务，并由补充工具窗口协助。 文档编辑器表示用户在 Visual Studio 中打开和保存的基本工作单元。 它们保留了与解决方案资源管理器或其他活动层次结构窗口相关联的强大选择感。 用户应该能够指向其中一个层次结构窗口，并知道文档的包含位置及其与解决方案、项目或 Visual Studio 包提供的另一个根对象的关系。

 文档编辑需要一致的用户体验。 要允许用户专注于手头的任务，而不是窗口管理和查找命令，请选择最适合编辑该文档类型的用户任务的文档视图策略。

#### <a name="common-interactions-for-the-document-well"></a>文档的常见交互

- 在常见的 **"新文件和****打开文件**"体验中保持一致的交互模型。

- 打开文档窗口时，更新相关窗口和菜单中的相关功能。

- 菜单命令被适当地集成到常见菜单中，如 **"编辑**"、"**格式**"和"**视图"** 菜单。 如果大量专用命令可用，则可以创建新菜单，仅当文档具有焦点时才可见。

- 嵌入工具栏可以放置在编辑器的顶部。 这比在编辑器外部显示单独的工具栏更可取。

- 始终在解决方案资源管理器或类似的活动层次结构窗口中维护选择。

- 双击解决方案资源管理器中的文档应执行与**Open**相同的操作。

- 如果在文档类型上可以使用多个编辑器，则用户应该能够通过右键单击文件并从快捷菜单中选择 **"打开可用**"对话框，使用 **"打开"** 对话框覆盖或重置给定文档类型的默认操作。

- 不要在文档中构建向导。

### <a name="user-expectations-for-specific-document-types"></a>用户对特定文档类型的期望
 有几种不同的基本类型的文档编辑器，每个类型都有一组与相同类型的其他交互一致。

- **基于文本的编辑器：** 代码编辑器、日志文件

- **设计表面：** WPF 窗体设计器、Windows 窗体

- **对话样式编辑器：** 清单设计器，项目属性

- **模型设计器：** 工作流设计器、代码映射、体系结构图、进度

  还有几种非编辑器类型很好地使用文档。 虽然它们不自行编辑文档，但它们确实需要遵循文档窗口的标准交互。

- **报告：** IntelliTrace 报告、Hyper-V 报告、探查器报告

- **仪表板：** 诊断中心

#### <a name="text-based-editors"></a>基于文本的编辑器

- 文档参与预览选项卡模型，允许在不打开文档的情况下预览文档。

- 文档的结构可以在配套工具窗口中表示，例如文档大纲。

- IntelliSense（如果适用）将与其他代码编辑器保持一致。

- 弹出窗口或辅助 UI 遵循现有类似 UI（如 CodeLens）的类似样式和模式。

- 有关文档状态的消息将在文档顶部的信息栏控件或状态栏中显示。

- 用户必须能够使用**工具>选项**页面、共享的字体和颜色页面或特定于编辑器的页面自定义字体和颜色的外观。

#### <a name="design-surfaces"></a>设计表面

- 空设计器应表面有一个水印，指示如何开始。

- 视图切换机制将遵循现有模式，例如双击以打开代码编辑器，或在文档窗口中允许与两个窗格交互的选项卡。

- 除非需要高度特定的工具窗口，否则应通过工具箱向设计图面添加元素。

- 曲面上的项目将遵循一致的选择模型。

- 嵌入工具栏仅包含特定于文档的命令，而不是常见的命令，如 **"保存**"。

#### <a name="dialog-style-editors"></a>对话样式编辑器

- 控件布局应遵循正常的对话框布局约定。

- 编辑器中的选项卡不应与文档选项卡的外观匹配，它们应与两个允许的内部选项卡样式之一匹配。

- 用户必须能够仅使用键盘与控件进行交互;通过激活编辑器并通过控件进行 Tab 键，或者使用标准助记符。

- 设计器应使用通用的"保存"模型。 不应在曲面上放置整体"保存"或"提交"按钮，尽管其他按钮可能合适。

#### <a name="model-designers"></a>模型设计师

- 空设计器应表面有一个水印，指示如何开始。

- 应通过工具箱向设计图面添加元素。

- 曲面上的项目将遵循一致的选择模型。

- 嵌入工具栏仅包含特定于文档的命令，而不是常见的命令，如 **"保存**"。

- 图例可能出现在曲面上，指示性标记或水印。

- 用户必须能够使用**工具>选项**页面、共享的字体和颜色页面或特定于编辑器的页面自定义字体/颜色的外观。

#### <a name="reports"></a>报表

- 报表通常仅供参考，并且不参与"保存"模型。 但是，它们可能包括交互，例如指向其他相关信息的链接或展开和折叠的部分。

- 表面上的大多数命令应该是超链接，而不是按钮。

- 布局应包括标题并遵循标准报表布局准则。

#### <a name="dashboards"></a>仪表板

- 仪表板本身没有交互模型，而是作为提供各种其他工具的一种手段。

- 他们不参与"保存"模型。

- 用户必须能够仅使用键盘与控件进行交互，既通过激活编辑器并通过控件进行选项卡，要么使用标准助记符。

## <a name="dialogs"></a><a name="BKMK_Dialogs"></a>对话 框

### <a name="introduction"></a>介绍
 可视化工作室中的对话框通常应支持用户工作的一个独立单元，然后被驳回。

 如果您确定需要一个对话框，则有三个选项，按首选项顺序排列：

1. 将您的功能集成到 Visual Studio 中的共享对话框之一中。

2. 使用现有类似对话框中找到的模式创建自己的对话框。

3. 按照交互和布局指南创建新对话框。

   本主题介绍如何在 Visual Studio 工作流中选择正确的对话框模式以及对话框设计的通用约定。

### <a name="themes"></a>主题
 可视化工作室中的对话框遵循以下两种基本样式之一：

#### <a name="standard-unthemed"></a>标准（无主题）
 大多数对话框是标准实用程序对话框，应取消主题。 不要重新模板通用控件或尝试创建样式化的"现代"按钮或控件。 控件和镶边外观遵循[对话框的标准 Windows 桌面交互准则](https://msdn.microsoft.com/library/windows/desktop/dn742499\(v=vs.85\).aspx)。

#### <a name="themed"></a>主题
 专业"签名"对话框可能是主题对话框。 主题对话框具有独特的外观，其中也有一些与样式关联的特殊交互模式。 仅当对话框满足以下要求时，它才主题：

- 该对话框是一种常见体验，经常或由许多用户（例如"**新项目**"对话框）经常看到和使用。

- 该对话框包含突出的产品品牌元素（例如，"**帐户设置"** 对话框）。

- 该对话框显示为包含其他主题对话框（例如"**添加已连接服务"** 对话框）的大流的组成部分。

- 该对话框是体验的重要组成部分，在推广或区分产品版本方面发挥着战略作用。

  创建主题对话框时，请使用适当的环境颜色并遵循正确的布局和交互模式。 （请参阅[视觉工作室的布局](../../extensibility/ux-guidelines/layout-for-visual-studio.md)）

### <a name="dialog-design"></a>对话框设计
 精心设计的对话框考虑了以下元素：

- 受支持的用户任务

- 对话框文本样式、语言和术语

- 控件选择和 UI 约定

- 可视化布局规范和控制对齐

- 键盘访问

#### <a name="content-organization"></a>内容组织
 请考虑这些基本类型的对话框之间的差异：

- [简单对话框](../../extensibility/ux-guidelines/application-patterns-for-visual-studio.md#BKMK_SimpleDialogs)在单个模式窗口中显示控件。 演示文稿可能包括复杂控制模式的变化，包括字段选取器或图标栏。

- [分层对话框](../../extensibility/ux-guidelines/application-patterns-for-visual-studio.md#BKMK_LayeredDialogs)用于充分利用屏幕空间，当一个 UI 部分包含多个控件组时。 对话框的分组通过选项卡控件、导航列表控件或按钮进行"分层"，以便用户可以选择在任何给定时刻要查看的分组。

- [向导](../../extensibility/ux-guidelines/application-patterns-for-visual-studio.md#BKMK_Wizards)可用于指导用户完成任务的逻辑步骤序列。 顺序面板中提供了一系列选择，有时引入不同的工作流（"分支"），具体取决于上一个面板中所做的选择。

#### <a name="simple-dialogs"></a><a name="BKMK_SimpleDialogs"></a>简单对话框
 简单对话框是在单个模式窗口中表示控件。 此演示文稿可能包括复杂控制模式（如字段选取器）的变体。 对于简单对话框，请遵循标准常规布局以及复杂控件分组所需的任何特定布局。

 ![Visual Studio 中的简单对话框](../../extensibility/ux-guidelines/media/0704-01-createstrongnamekey.png "0704-01_CreateStrongNameKey")

 **创建强名称键是可视化工作室中简单对话框的示例。**

#### <a name="layered-dialogs"></a><a name="BKMK_LayeredDialogs"></a>分层对话框
 分层对话框包括选项卡、仪表板和嵌入的树。 当一个 UI 中提供了多个控件组时，它们用于最大化房地产。 分组是分层的，以便用户可以选择在任意时间要查看的分组。

 在最直接的情况下，分组之间切换的机制是选项卡控件。 有多种替代方法可供选择。 有关如何选择最合适的样式，请参阅优先级和分层。

 **"工具>选项"** 对话框是使用嵌入树分层对话框的示例：

 ![Visual Studio 中的分层对话框](../../extensibility/ux-guidelines/media/0704-02-toolsoptions.png "0704-02_ToolsOptions")

 **工具>选项是可视化工作室中分层对话框的示例。**

#### <a name="wizards"></a><a name="BKMK_Wizards"></a>向导
 向导可用于指导用户完成任务时的逻辑步骤序列。 顺序面板中提供了一系列选项，用户必须继续执行每个步骤，然后才能继续执行下一步。 一旦有足够的默认值可用，将启用 **"完成"** 按钮。

 模式向导用于：

- 包含分支，根据用户选择提供不同的路径

- 包含步骤之间的依赖项，其中后续步骤依赖于上一步中的用户输入

- 足够复杂，应使用 UI 来解释每个步骤中提供的选项和可能的结果

- 是事务性的，要求在提交任何更改之前完成一组步骤

### <a name="common-conventions"></a>共同公约
 要实现对话框的最佳设计和功能，请遵循这些约定，包括对话框大小、位置、标准、控制配置和对齐、UI 文本、标题栏、控制按钮和访问键。

 有关特定于布局的指南，请参阅[可视化工作室的布局](../../extensibility/ux-guidelines/layout-for-visual-studio.md)。

#### <a name="size"></a>大小
 对话框应适合至少 1024x768 屏幕分辨率，初始对话框大小不应超过 900x700 像素。 对话框可以调整大小，但这不是要求。

 有两个可调整大小的对话框的建议：

1. 最小大小是为对话框定义的，该对话框将针对控件集进行优化，而不进行剪切，并进行调整以适应合理的本地化增长。

2. 用户缩放的大小在会话和会话中仍然存在。 例如，如果用户将对话框缩放为 150%，则随后启动对话框将显示为 150%。

#### <a name="position"></a>位置
 对话必须在首次启动时显示在 IDE 中居中。 对于不可调整大小的对话框，不需要保留对话框的最后一个位置，因此它将以后续启动为中心。 对于可调整大小的对话框，应在后续启动时保留大小。 对于模态的可调整大小的对话框，不需要保留该位置。 在 IDE 中显示它们居中可防止当用户的显示配置发生更改时，对话框在不可预测或不可用的位置显示的可能性。 对于可以重新定位的无模式对话框，应在后续启动时保持用户的位置，因为该对话框可能经常用作较大工作流的组成部分。

 当对话框必须生成其他对话框时，最上面的对话框应从父对话框向右和向下级联，以便用户清楚显示它们已导航到新位置。

#### <a name="modality"></a>形态
 模态意味着用户在继续之前必须完成或取消对话框。 由于模态对话框阻止用户与环境的其他部分交互，因此功能的任务流应尽可能谨慎地使用它们。 当需要模式操作时，Visual Studio 具有许多共享对话框，您可以将功能集成到其中。 如果必须创建新对话框，请遵循具有类似功能的现有对话框的交互模式。

 当用户需要同时执行两个活动（如在编写新代码时**查找**和**替换**）时，对话框应不模式，以便用户可以轻松地在它们之间切换。 Visual Studio 通常使用工具窗口执行此类支持编辑器的链接任务。

#### <a name="control-configuration"></a>控制配置
 与在 Visual Studio 中完成相同操作的现有控制配置保持一致。

#### <a name="title-bars"></a>标题栏

- 标题栏中的文本必须反映启动该命令的命令的名称。

- 对话框标题栏中不应使用任何图标。 在系统需要的情况下，请使用 Visual Studio 徽标。

- 对话框不应具有最小化或最大化按钮。

- 标题栏中的帮助按钮已被弃用。 不要将它们添加到新对话框中。 当它们确实存在时，它们应启动一个与任务在概念上相关的帮助主题。

  ![Visual Studio 的标题栏规范](../../extensibility/ux-guidelines/media/0704-03-titlebarspecs.png "0704-03_TitleBarSpecs")

  **可视化工作室对话框中标题栏的准则规范。**

#### <a name="control-buttons"></a>控制按钮
 通常 **，"确定**/**取消**/**帮助"** 按钮应水平排列在对话框的右下角。 如果对话框底部有几个其他按钮，这些按钮会对控件按钮造成视觉混淆，则允许备用垂直堆栈。

 ![Visual Studio 中的控件按钮配置](../../extensibility/ux-guidelines/media/0704-04-controlbuttonconfig.png "0704-04_ControlButtonConfig")

 **可视化工作室对话框中控制按钮的可接受配置**

 该对话框必须包含默认控件按钮。 要确定用作默认值的最佳命令，请从以下选项中进行选择（按优先级顺序列出）：

- 选择最安全、最安全的命令作为默认值。 这意味着选择最有可能防止数据丢失和避免意外系统访问的命令。

- 如果数据丢失和安全性不是因素，则根据便利性选择默认命令。 当对话框支持频繁或重复的任务时，将最有可能的命令作为默认值将改进用户的工作流。

  避免为默认命令选择永久破坏性操作。 如果存在此类命令，请选择更安全的命令作为默认值。

#### <a name="access-keys"></a>访问密钥
 请勿将访问键用于 **"确定**/**取消**/**帮助"** 按钮。 默认情况下，这些按钮映射到快捷键：

|按钮名称|键盘快捷键|
|-----------------|-----------------------|
|OK|Enter|
|取消|Esc|
|帮助|F1|

#### <a name="imagery"></a>图像
 在对话框中谨慎使用图像。 请勿在对话框中使用大图标只是为了占用空间。 仅当图像是向用户传达消息的重要部分（如警告图标或状态动画）时，才使用图像。

### <a name="prioritizing-and-layering"></a><a name="BKMK_PrioritizingAndLayering"></a>优先级和分层

#### <a name="prioritizing-your-ui"></a>确定 UI 的优先级
 可能需要将某些 UI 元素放在首位，并将更高级的行为和选项（包括模糊的命令）放入对话框中。 通过为其腾出空间，并在显示对话框时，在 UI 中默认使用带有文本标签的 UI 中使其可见，从而将常用功能置于最前沿。

#### <a name="layering-your-ui"></a>分层 UI
 如果确定需要对话框，但要向用户显示的相关功能超出了可在简单对话框中显示的功能，则需要对 UI 进行分层。 Visual Studio 最常用的分层方法是选项卡、走廊或仪表板。 在某些情况下，可以扩展和折叠的区域可能合适。 视觉工作室通常不建议使用自适应 UI。

 通过类似选项卡的控件对 UI 进行分层的不同方法有优点和缺点。 查看下面的列表，以确保您选择适合您的情况的分层技术。

##### <a name="tabbing"></a>Tab 键次序

|开关机构|优势和适当使用|缺点和不当使用|
|-------------------------|------------------------------------|-----------------------------------------|
|Tab 控件|逻辑上将对话框页分组到相关集中<br /><br /> 对于对话框中相关控件的五个（或适合在对话框中一行中的选项卡数）少于五个（或选项卡数）非常有用<br /><br /> 选项卡标签必须简短：一个或两个单词，可以轻松地识别内容<br /><br /> 通用系统对话框样式<br /><br /> 示例：**文件资源管理器>项属性**|制作描述性短标签可能很困难<br /><br /> 通常不会在一个对话框中缩放超过五个选项卡<br /><br /> 如果一行的选项卡太多，则不合适;使用替代分层技术<br /><br /> 不可扩展|
|边栏导航|简单的切换设备，可以容纳比选项卡更多的类别<br /><br /> 类别的平面列表（无层次结构）<br /><br /> 可扩展<br /><br /> 示例：**自定义... >添加命令**|如果水平空间少于三组，则不能很好地使用水平空间<br /><br /> 任务可能更适合下拉|
|树控件|允许无限类别<br /><br /> 允许对类别进行分组和/或层次结构<br /><br /> 可扩展<br /><br /> 示例：**工具>选项**|严重嵌套层次结构可能会导致过度的水平滚动<br /><br /> 视觉工作室拥有过多的树景|
|向导|通过指导完成基于任务的连续步骤，帮助完成任务。 向导表示高级任务，各个面板表示完成整个任务所需的子任务。<br /><br /> 当任务跨越 Ui 边界时很有用，例如用户必须使用多个编辑器和工具窗口来完成任务<br /><br /> 当任务需要分支时有用<br /><br /> 当任务包含步骤之间的依赖项时很有用<br /><br /> 当一个决策分叉的几个类似任务可以在一个对话框中显示，以减少不同类似对话框的数量时非常有用。|不适合不需要顺序工作流的任何任务<br /><br /> 用户可以因向导过多而不知所措和困惑<br /><br /> 巫师有固有的有限的屏幕房地产|

##### <a name="hallways-or-dashboards"></a>走廊或仪表板
 "导航"和"仪表板"是用作其他对话框和窗口的启动点的对话框或面板。 精心设计的"hallway"立即只显示最常见的选项、命令和设置，使用户能够轻松完成常见任务。 与实际走廊提供门道以访问其后面的房间一样，此处不太常见的 UI 被收集到单独的"房间"（通常是其他对话框）中，这些功能可以从主走廊访问。

 或者，提供单个集合中的所有可用功能而不是将不太常见的功能重构到单独位置的 UI 只是一个仪表板。

 ![Outlook 中的“过道”概念](../../extensibility/ux-guidelines/media/0704-08-hallway.png "0704-08_Hallway")

 **用于在 Outlook 中公开其他 UI 的走廊概念**

##### <a name="adaptive-ui"></a>自适应 UI
 根据使用情况或用户自报体验显示或隐藏 UI 是显示必要 UI 而隐藏其他部分的另一种方法。 Visual Studio 中不建议这样做，因为用于决定何时显示或隐藏 UI 的算法可能比较棘手，对于某些情况，规则总是是错误的。

## <a name="projects"></a><a name="BKMK_Projects"></a>项目

### <a name="projects-in-the-solution-explorer"></a>解决方案资源管理器中的项目
 大多数项目都分类为基于引用、基于目录或混合的项目。 解决方案资源管理器中同时支持所有三种类型的项目。 处理项目的用户体验的根位于此窗口中。 尽管不同的项目节点是引用、目录或混合模式类型项目，但有一个常见的交互模式，在分到特定于项目的用户模式之前，应作为起点应用。

 项目应始终：

- 支持添加项目文件夹以组织项目内容的能力

- 维护项目持久性的一致模型

  项目还应保持一致的交互模型：：

- 删除项目项

- 保存文档

- 项目属性编辑

- 在备用视图中编辑项目

- 拖放操作

### <a name="drag-and-drop-interaction-model"></a>拖放交互模型
 项目通常将其分类为基于引用（只能保留对存储中的项目项的引用）、基于目录（只能保留物理存储在项目层次结构中的项目项）或混合（能够保留引用）或实物）。 IDE 同时适应**解决方案资源管理器**中所有三种类型的项目。

 从拖放的角度来看，以下特征应应用于**解决方案资源管理器**中的每种类型的项目：

- **基于参考的项目：** 关键是项目正在拖动对存储中的项的引用。 当基于引用的项目充当移动操作的源时，它应仅从项目中删除对该项目的引用。 该项目实际上不应从硬盘驱动器中删除。 当基于引用的项目充当移动（或复制）操作的目标时，它应该添加对原始源项的引用，而不创建项的私有副本。

- **基于目录的项目：** 从拖放的角度来看，项目是围绕物理项而不是引用进行拖动。 当基于目录的项目充当移动操作的源时，它最终应该从硬盘驱动器中删除物理项，并从项目中删除它。 当基于目录的项目充当移动（或复制）操作的目标时，它应在其目标位置创建源项的副本。

- **混合目标项目：** 从拖放的角度来看，这种类型的项目的行为基于要拖动的项的性质（对存储中的项或项本身的引用）。 上面描述了引用和物理项的正确行为。

  如果**解决方案资源管理器**中只有一种类型的项目，则拖放操作将非常简单。 由于每个项目系统都能够定义自己的拖放行为，因此应遵循某些准则（基于 Windows 资源管理器拖放行为），以确保可预测的用户体验：

- **解决方案资源管理器**中未修改的拖动操作（当 Ctrl 和 Shift 键均未保持关闭时）应会导致移动操作。

- 换档拖动操作还应导致移动操作。

- Ctrl 拖动操作应会导致复制操作。

- 基于引用的混合项目系统支持向源项添加链接（或引用）的概念。 当这些项目是拖放操作的目标（当**Ctrl + Shift**被关闭时），它应导致对要添加到项目的项的引用

  并非所有拖放操作都是基于引用、基于目录和混合项目的组合的明智之举。 特别是，假装允许在基于目录的源项目和基于引用的目标项目之间进行移动操作是有问题的，因为基于源目录的项目必须在完成移动后删除源项。 然后，基于目标引用的项目最终将引用已删除的项目。

  假装允许在这些类型的项目之间进行复制操作也是误导性的，因为基于目标引用的项目不应创建源项的独立副本。 同样，不应允许 Ctrl 和 Shift 拖动到基于目录的目标项目，因为基于目录的项目无法保留引用。 在不支持拖放操作的情况下，IDE 应禁止放置，并向用户显示无放置光标（如下指针表中所示）。

  要正确实现拖放行为，拖动的源项目需要将其性质（例如，它是基于引用还是基于目录？ 此信息由源提供的剪贴板格式指示。 作为拖动（或剪贴板复制操作）的来源，项目应分别提供**CF_VSREFPROJECTITEM**S 或**CF_VSSTGPROJECTITEMS，** 具体取决于项目是基于引用还是基于目录。 这两种格式具有相同的数据内容，这与 Windows **CF_HDROP**格式类似，只不过字符串列表（而不是文件名）是**Projref**字符串的双**NULL**终止列表（从**IVsSolution 返回：getProjrefItem**或 **：：getProjrefOfProject（** 视情况而定）。

  作为放置（或剪贴板粘贴操作）的目标，项目应同时接受**CF_VSREFPROJECTITEMS**和**CF_VSSTGPROJECTITEMS，** 但拖放操作的确切处理因目标项目和源项目的性质而异。 源项目通过提供**CF_VSREFPROJECTITEMS**还是**CF_VSSTGPROJECTITEMS**来声明其性质。 丢弃的目标了解其自身性质，因此有足够的信息来决定是否应执行移动、复制或链接。 用户还可以通过按 Ctrl、Shift 或"移动"键来修改应执行的拖放操作。 放置目标必须正确指示在其**DragEnter**和**DragOver**方法中提前执行哪些操作，这一点很重要。 **解决方案资源管理器**自动知道源项目和目标项目是否相同。

  特别不支持跨 Visual Studio 实例（例如，从 devenv.exe 的一个实例拖动项目项）。 **解决方案资源管理器**还直接禁用此功能。

  用户应始终能够通过选择项目、将其拖动到目标位置以及观察在删除项目之前显示以下鼠标指针中出现哪种效果来确定拖放操作的效果：

|鼠标指针|Command|说明|
|-------------------|-------------|-----------------|
|![鼠标“不放下”图标](../../extensibility/ux-guidelines/media/0706-01-mousenodrop.png "0706-01_MouseNoDrop")|无掉落|无法将物料删除到指定位置。|
|![鼠标“复制”图标](../../extensibility/ux-guidelines/media/0706-02-mousecopy.png "0706-02_MouseCopy")|复制|项目将复制到目标位置。|
|![鼠标“移动”图标](../../extensibility/ux-guidelines/media/0706-03-mousemove.png "0706-03_MouseMove")|移动|项目将移动到目标位置。|
|![鼠标“添加引用”图标](../../extensibility/ux-guidelines/media/0706-04-mouseaddref.png "0706-04_MouseAddRef")|添加引用|对所选项的引用将添加到目标位置。|

#### <a name="reference-based-projects"></a>基于参考的项目
 下表汇总了应根据源项和为基于引用的目标项目按下的修改器键的性质执行的拖放操作：：

|||源项：参考/链接|源项：物理项或文件系统 （CF_HDROP）|
|-|-|----------------------------------|-------------------------------------------------------------|
|无修饰符|操作|移动|链接|
|无修饰符|目标|添加对原始项目的引用|添加对原始项目的引用|
|无修饰符|源|删除对原始项目的引用|保留原始项目|
|无修饰符|结果|**DROPEFFECT_MOVE**返回，因为操作来自 **：:Drop，** 项目保留在存储中的原始位置|**DROPEFFECT_LINK**返回，因为操作来自 **：:Drop，** 项目保留在存储中的原始位置|
|换档+拖动|操作|移动|无掉落|
|换档+拖动|目标|添加对原始项目的引用|无掉落|
|换档+拖动|源|删除对原始项目的引用|无掉落|
|换档+拖动|结果|**DROPEFFECT_MOVE**返回，因为操作来自 **：:Drop，** 项目保留在存储中的原始位置|无掉落|
|Ctrl_拖动|操作|复制|无掉落|
|Ctrl_拖动|目标|添加对原始项目的引用|无掉落|
|Ctrl_拖动|源|保留对原始项目的引用|无掉落|
|Ctrl_拖动|结果|**DROPEFFECT_COPY**返回，因为操作来自 **：:Drop**和项目保留在存储中的原始位置|无掉落|
|Ctrl_Shift_拖动|操作|链接|链接|
|Ctrl_Shift_拖动|目标|添加对原始项目的引用|添加对原始项目的引用|
|Ctrl_Shift_拖动|源|保留对原始项目的引用|保留原始项目|
|Ctrl_Shift_拖动|结果|**DROPEFFECT_LINK**返回，因为操作来自 **：:Drop，** 项目保留在存储中的原始位置|**DROPEFFECT_LINK**返回，因为操作来自 **：:Drop，** 项目保留在存储中的原始位置|
|Ctrl_Shift_拖动|注意|与 Windows 资源管理器中快捷方式的拖放行为相同。||
|剪切/粘贴|操作|移动|链接|
|剪切/粘贴|目标|添加对原始项目的引用|添加对原始项目的引用|
|剪切/粘贴|源|保留对原始项目的引用|保留原始项目|
|剪切/粘贴|结果|项目保留在存储中的原始位置|项目保留在存储中的原始位置|
|复制/粘贴|操作|复制|链接|
|复制/粘贴|源|添加对原始项目的引用|添加对原始项目的引用|
|复制/粘贴|结果|保留对原始项目的引用|保留原始项目|
|复制/粘贴|操作|项目保留在存储中的原始位置|项目保留在存储中的原始位置|

#### <a name="directory-based-projects"></a>基于目录的项目
 下表汇总了应根据源项和为基于目录的目标项目按下的修改器键的性质执行的拖放操作：：

|||源项：参考/链接|源项：物理项或文件系统 （CF_HDROP）|
|-|-|----------------------------------|-------------------------------------------------------------|
|无修饰符|操作|移动|移动|
|无修饰符|目标|将项目复制到目标位置|将项目复制到目标位置|
|无修饰符|源|删除对原始项目的引用|删除对原始项目的引用|
|无修饰符|结果|**DROPEFFECT_ MOVE**返回，因为操作来自 **：:Drop，** 项目保留在存储中的原始位置|**DROPEFFECT_ MOVE**返回，因为操作来自 **：:Drop，** 项目保留在存储中的原始位置|
|换档+拖动|操作|移动|移动|
|换档+拖动|目标|将项目复制到目标位置|将项目复制到目标位置|
|换档+拖动|源|删除对原始项目的引用|从原始位置删除项目|
|换档+拖动|结果|**DROPEFFECT_ MOVE**返回，因为操作来自 **：:Drop，** 项目保留在存储中的原始位置|**DROPEFFECT_ MOVE**返回，因为操作来自 **：:Drop，** 项目保留在存储中的原始位置|
|Ctrl_拖动|操作|复制|复制|
|Ctrl_拖动|目标|将项目复制到目标位置|将项目复制到目标位置|
|Ctrl_拖动|源|保留对原始项目的引用|保留对原始项目的引用|
|Ctrl_拖动|结果|**DROPEFFECT_ COPY**返回，因为操作来自 **：:Drop，** 项目保留在存储中的原始位置|**DROPEFFECT_ COPY**返回，因为操作来自 **：:Drop，** 项目保留在存储中的原始位置|
|Ctrl_Shift_拖动||无掉落|无掉落|
|剪切/粘贴|操作|移动|移动|
|剪切/粘贴|目标|将项目复制到目标位置|将项目复制到目标位置|
|剪切/粘贴|源|删除对原始项目的引用|从原始位置删除项目|
|剪切/粘贴|结果|项目保留在存储中的原始位置|项目从存储中的原始位置中删除|
|复制/粘贴|操作|复制|复制|
|复制/粘贴|目标|添加对原始项目的引用|将项目复制到目标位置|
|复制/粘贴|源|保留原始项目|保留原始项目|
|复制/粘贴|结果|项目保留在存储中的原始位置|项目保留在原始位置的存储|

#### <a name="mixed-target-projects"></a>混合目标项目
 下表汇总了应根据源项和为混合目标项目按下的修改器键的性质执行的拖放操作（以及剪切/复制/粘贴）操作：

|||源项：参考/链接|源项：物理项或文件系统 （CF_HDROP）|
|-|-|----------------------------------|-------------------------------------------------------------|
|无修饰符|操作|移动|移动|
|无修饰符|目标|添加对原始项目的引用|将项目复制到目标位置|
|无修饰符|源|删除对原始项目的引用|删除对原始项目的引用|
|无修饰符|结果|**DROPEFFECT_ MOVE**返回，因为操作来自 **：:Drop，** 项目保留在存储中的原始位置|**DROPEFFECT_ MOVE**返回为**操作，从 ：:Drop**中删除项目从存储中的原始位置中删除|
|换档+拖动|操作|移动|移动|
|换档+拖动|目标|添加对原始项目的引用|将项目复制到目标位置|
|换档+拖动|源|删除对原始项目的引用|从原始位置删除项目|
|换档+拖动|结果|**DROPEFFECT_ MOVE**返回，因为操作来自 **：:Drop，** 项目保留在存储中的原始位置|**DROPEFFECT_ MOVE**返回为**操作，从 ：:Drop**中删除项目从存储中的原始位置中删除|
|Ctrl_拖动|操作|复制|复制|
|Ctrl_拖动|目标|添加对原始项目的引用|将项目复制到目标位置|
|Ctrl_拖动|源|保留对原始项目的引用|保留原始项目|
|Ctrl_拖动|结果|**DROPEFFECT_ COPY**返回，因为操作来自 **：:Drop，** 项目保留在存储中的原始位置|**DROPEFFECT_ COPY**返回，因为操作来自 **：:Drop，** 项目保留在存储中的原始位置|
|Ctrl_Shift_拖动|操作|链接|链接|
|Ctrl_Shift_拖动|目标|添加对原始项目的引用|添加对原始源项的引用|
|Ctrl_Shift_拖动|源|保留对原始项目的引用|保留原始项目|
|Ctrl_Shift_拖动|结果|**DROPEFFECT_ LINK**返回，因为操作来自 **：:Drop**和项目保留在存储中的原始位置|**DROPEFFECT_ LINK**返回，因为操作来自 **：:Drop**和项目保留在存储中的原始位置|
|剪切/粘贴|操作|移动|移动|
|剪切/粘贴|目标|将项目复制到目标位置|将项目复制到目标位置|
|剪切/粘贴|源|删除对原始项目的引用|从原始位置删除项目|
|剪切/粘贴|结果|项目保留在存储中的原始位置|项目从存储中的原始位置中删除|
|复制/粘贴|操作|复制|复制|
|复制/粘贴|目标|添加对原始项目的引用|将项目复制到目标位置|
|复制/粘贴|源|保留原始项目|保留原始项目|
|复制/粘贴|结果|项目保留在存储中的原始位置|项目保留在存储中的原始位置|

 在**解决方案资源管理器**中实现拖动时，应考虑这些详细信息：

- 为多种选择方案设计。

- 文件名（完整路径）必须在整个目标项目中是唯一的，否则不应允许删除。

- 文件夹名称必须是唯一的（不区分大小写）的级别。

- 在拖动时打开或关闭的文件之间存在行为差异（上述方案未提及）。

- 顶级文件的行为与文件夹中的文件略有不同。

  需要注意的另一个问题是如何处理具有打开设计器或编辑器的项目的移动操作。 预期行为如下（这适用于所有项目类型）：

1. 如果打开的编辑器/设计器没有任何未保存的更改，则应静默关闭编辑器/设计器窗口。

2. 如果打开的编辑器/设计器确实有未保存的更改，则拖动源应等待放置，然后要求用户在关闭窗口之前保存未提交的更改，提示如下所示：

   ```
   ==========================================================
        One or more open documents have unsaved changes.
   Do you want to save uncommitted changes before proceeding?
                     [Yes]  [No]  [Cancel]
   ==========================================================
   ```

   这使用户有机会在目标复制之前保存正在进行的工作。 添加了新方法**IVs 层次结构DropDataSource2：：已添加 OnToonNotify**以启用此处理。

   然后，目标将复制项目在存储中的状态（如果用户选择 **"否**"，则不包括编辑器中未保存的更改）。 目标完成复制后（在**IV层次结构DropDataSource：:Drop**中），源将有机会完成移动操作的删除部分（在**IVs 层次结构DropDataSource中：onDropNotify）。**

   任何未保存更改的编辑器都应保留打开状态。 对于具有未保存更改的文档，这意味着将执行移动操作的副本部分，但删除部分将中止。 在用户选择 **"否"** 的多个选择方案中，不应关闭或删除具有未保存更改的文档，但应关闭和删除那些没有未保存更改的文档。
