---
title: Windows Communication Foundation 和 WCF 数据服务
ms.date: 11/04/2016
ms.topic: conceptual
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- services, WCF Data
- WCF services, binding to
- WCF services, asynchronous service methods
- service references [Visual Studio]
- WCF Data Services
- asynchronous calls
- service references [Visual Studio], type sharing
- endpoints [WCF]
- asynchronous service methods
- service references [Visual Studio] endpoints
- WCF services, type sharing
- Windows Communication Foundation, in Visual Studio
- services, WCF
- WCF service, Visual Studio
- data services, WCF
- services, in Visual Studio
- data binding [Visual Studio], WCF
- service endpoints [Visual Studio]
- service references [Visual Studio], asynchronous calls
- services, Windows Communication Foundation
- type sharing in WCF services
- WCF services, endpoints
- service method, called asynchronously[Visual Studio]
ms.assetid: d56f12cb-e139-4fec-b3e4-488383356642
author: ghogen
ms.author: ghogen
manager: jillfra
ms.workload:
- data-storage
ms.openlocfilehash: abcfde777223ada130e06ab7766319e1d982258c
ms.sourcegitcommit: d233ca00ad45e50cf62cca0d0b95dc69f0a87ad6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/01/2020
ms.locfileid: "75585933"
---
# <a name="windows-communication-foundation-services-and-wcf-data-services-in-visual-studio"></a>Visual Studio 中的 Windows Communication Foundation 服务和 WCF 数据服务

Visual Studio 提供了用于处理 Windows Communication Foundation （WCF）和 WCF 数据服务的工具，这是用于创建分布式应用程序的 Microsoft 技术。 本主题介绍 Visual Studio 透视中的服务。 有关完整文档，请参阅[WCF 数据服务 4.5](/dotnet/framework/data/wcf/index)。

## <a name="what-is-wcf"></a>什么是 WCF？

Windows Communication Foundation （WCF）是一个统一的框架，用于创建安全、可靠、事务处理和可互操作的分布式应用程序。 它取代了较旧的进程间通信技术，如 .ASMX web 服务、.NET 远程处理、企业服务（DCOM）和 MSMQ。 WCF 将所有这些技术的功能汇集在一个统一的编程模型下。 这简化了开发分布式应用程序的体验。

### <a name="what-are-wcf-data-services"></a>WCF 数据服务

WCF 数据服务是开放数据（OData）协议标准的实现。  WCF 数据服务允许你以一组 REST Api 的形式公开表格数据，从而使你能够使用标准 HTTP 谓词（如 GET、POST、PUT 或 DELETE）返回数据。 在服务器端，WCF 数据服务被[ASP.NET Web API](https://dotnet.microsoft.com/apps/aspnet/apis)用于创建新的 OData 服务。 在 Visual Studio 中使用 .NET 应用程序中的 OData 服务时，WCF 数据服务客户端库仍是一个不错的选择（**项目** > **添加服务引用**）。 有关详细信息，请参阅 [WCF Data Services 4.5](/dotnet/framework/data/wcf)。

### <a name="wcf-programming-model"></a>WCF 编程模型

WCF 编程模型基于两个实体之间的通信： WCF 服务和 WCF 客户端。 编程模型封装在 .NET 中的 <xref:System.ServiceModel> 命名空间中。

### <a name="wcf-service"></a>WCF 服务

WCF 服务基于在服务与客户端之间定义协定的接口。 它标记有 <xref:System.ServiceModel.ServiceContractAttribute> 特性，如以下代码所示：

[!code-csharp[WCFWalkthrough#6](../data-tools/codesnippet/CSharp/windows-communication-foundation-services-and-wcf-data-services-in-visual-studio_1.cs)]
[!code-vb[WCFWalkthrough#6](../data-tools/codesnippet/VisualBasic/windows-communication-foundation-services-and-wcf-data-services-in-visual-studio_1.vb)]

通过使用 <xref:System.ServiceModel.OperationContractAttribute> 特性来标记 WCF 服务公开的函数或方法。

[!code-csharp[WCFWalkthrough#1](../data-tools/codesnippet/CSharp/windows-communication-foundation-services-and-wcf-data-services-in-visual-studio_2.cs)]
[!code-vb[WCFWalkthrough#1](../data-tools/codesnippet/VisualBasic/windows-communication-foundation-services-and-wcf-data-services-in-visual-studio_2.vb)]

此外，还可以通过使用 <xref:System.Runtime.Serialization.DataContractAttribute> 特性标记复合类型来公开序列化的数据。 这会在客户端中启用数据绑定。

定义接口及其方法后，它们封装在实现接口的类中。 单个 WCF 服务类可实现多个服务协定。

WCF 服务通过所谓的*终结点*来公开使用。 终结点提供了与服务进行通信的唯一方法;你不能通过直接引用访问服务，就像使用其他类一样。

终结点由地址、绑定和协定组成。 该地址定义了服务所在的位置;这可能是 URL、FTP 地址或网络路径或本地路径。 绑定定义了与服务进行通信的方式。 WCF 绑定提供了用于指定协议（如 HTTP 或 FTP）、安全机制（例如 Windows 身份验证、用户名和密码等）的通用模型。 协定包括 WCF 服务类公开的操作。

单个 WCF 服务可以公开多个终结点。 这使得不同的客户端可以通过不同的方式与同一服务通信。 例如，银行服务可能为员工提供一个终结点，为外部客户提供另一个终结点，每个终结点使用不同的地址、绑定和/或协定。

### <a name="wcf-client"></a>WCF client（WCF 客户端）

WCF 客户端由一个允许应用程序与 WCF 服务进行通信的*代理*和一个与为服务定义的终结点匹配的终结点组成。 代理在*app.config*文件的客户端上生成，并包括有关服务公开的类型和方法的信息。 对于公开多个终结点的服务，客户端可以选择最适合其需要的服务，例如，通过 HTTP 进行通信并使用 Windows 身份验证。

创建 WCF 客户端之后，你可以在代码中引用服务，就像对任何其他对象一样。 例如，若要调用之前所示的 `GetData` 方法，将编写类似于下面的代码：

[!code-csharp[WCFWalkthrough#3](../data-tools/codesnippet/CSharp/windows-communication-foundation-services-and-wcf-data-services-in-visual-studio_3.cs)]
[!code-vb[WCFWalkthrough#3](../data-tools/codesnippet/VisualBasic/windows-communication-foundation-services-and-wcf-data-services-in-visual-studio_3.vb)]

## <a name="wcf-tools-in-visual-studio"></a>Visual Studio 中的 WCF 工具

Visual Studio 提供的工具可帮助你创建 WCF 服务和 WCF 客户端。 有关演示工具的演练，请参阅[演练：在 Windows 窗体中创建简单的 WCF 服务](../data-tools/walkthrough-creating-a-simple-wcf-service-in-windows-forms.md)。

### <a name="create-and-test-wcf-services"></a>创建和测试 WCF 服务

可以使用 WCF Visual Studio 模板作为基础来快速创建自己的服务。 然后，可以使用 WCF 服务自动主机和 WCF 测试客户端来调试和测试服务。 这些工具结合在一起，可以快速方便地进行调试和测试周期，并且无需在早期阶段提交到托管模型。

#### <a name="wcf-templates"></a>WCF 模板

WCF Visual Studio 模板为服务开发提供了一个基本的类结构。 "**添加新项目**" 对话框中提供了若干 WCF 模板。 其中包括 WCF 服务 lLibrary 项目、WCF 服务网站和 WCF 服务项模板。

选择模板时，会为服务协定、服务实现和服务配置添加文件。 已经添加了所有必需的属性，这会创建一个简单的 "Hello World" 类型的服务，您无需编写任何代码。 当然，您需要添加代码来为您的实际服务提供函数和方法，但这些模板提供了基本的基础。

若要了解有关 WCF 模板的详细信息，请参阅[Wcf Visual Studio 模板](/dotnet/framework/wcf/wcf-vs-templates)。

#### <a name="wcf-service-host"></a>WCF 服务主机

当你为 WCF 服务项目启动 Visual Studio 调试器（按**F5**）时，Wcf 服务主机工具会自动启动以在本地托管服务。 WCF 服务主机枚举 WCF 服务项目中的服务，加载项目的配置，并为它找到的每个服务实例化主机。

通过使用 WCF 服务主机，你可以在开发过程中测试 WCF 服务，而无需编写额外的代码或提交到特定的主机。

若要了解有关 WCF 服务主机的详细信息，请参阅[wcf 服务主机（wcfsvchost.exe）](/dotnet/framework/wcf/wcf-service-host-wcfsvchost-exe)。

#### <a name="wcf-test-client"></a>WCF 测试客户端

WCF 测试客户端工具使你可以输入测试参数、将该输入提交到 WCF 服务，以及查看服务发送回的响应。 在将其与 WCF 服务主机组合时，它提供了一种方便的服务测试体验。 在 *% ProgramFiles （x86）% \ Microsoft Visual Studio\2017\Enterprise\Common7\IDE*文件夹中找到该工具。

当您按**F5**调试 WCF 服务项目时，Wcf 测试客户端将打开并显示在配置文件中定义的服务终结点的列表。 你可以测试参数并启动服务，并重复此过程以持续测试和验证你的服务。

若要了解有关 WCF 测试客户端的详细信息，请参阅[wcf 测试客户端（wcftestclient.exe）](/dotnet/framework/wcf/wcf-test-client-wcftestclient-exe)。

### <a name="accessing-wcf-services-in-visual-studio"></a>在 Visual Studio 中访问 WCF 服务

Visual Studio 简化了创建 WCF 客户端的任务，自动生成代理和使用 "**添加服务引用**" 对话框添加的服务的终结点。 所有必需的配置信息都将添加到*app.config*文件中。 大多数情况下，您所要做的只是实例化服务以便使用。

使用 "**添加服务引用**" 对话框可以输入服务的地址，也可以搜索解决方案中定义的服务。 此对话框返回服务和这些服务提供的操作的列表。 它还允许您定义在代码中引用服务时所依据的命名空间。

使用 "**配置服务引用**" 对话框，可以自定义服务的配置。 您可以更改服务的地址，指定访问级别、异步行为和消息协定类型，并配置类型重用。

## <a name="how-to-select-a-service-endpoint"></a>如何：选择服务终结点

某些 Windows Communication Foundation （WCF）服务公开多个终结点，客户端可以通过这些终结点与服务进行通信。 例如，服务可能会公开一个终结点，该终结点使用 HTTP 绑定、用户名和密码安全以及使用 FTP 和 Windows 身份验证的第二个终结点。 从防火墙外部访问服务的应用程序可能会使用第一个终结点，而第二个终结点可以在 intranet 上使用。

在这种情况下，可以将 `endpointConfigurationName` 指定为服务引用的构造函数的参数。

[!INCLUDE[note_settings_general](../data-tools/includes/note_settings_general_md.md)]

### <a name="to-select-a-service-endpoint"></a>选择服务终结点

1. 通过在**解决方案资源管理器**中右键单击项目节点，然后选择 "**添加服务引用**"，添加对 WCF 服务的引用。

2. 在代码编辑器中，添加服务引用的构造函数：

    ```vb
    Dim proxy As New ServiceReference.Service1Client(
    ```

    ```csharp
    ServiceReference.Service1Client proxy = new ServiceReference.Service1Client(
    ```

    > [!NOTE]
    > 将*ServiceReference*替换为服务引用的命名空间，并将*Service1Client*替换为服务的名称。

3. IntelliSense 列表显示，其中包含构造函数的重载。 选择 `endpointConfigurationName As String` 重载。

4. 在重载之后键入 `=` *ConfigurationName*，其中*ConfigurationName*是要使用的终结点的名称。

    > [!NOTE]
    > 如果你不知道可用终结点的名称，可以在*app.config*文件中找到它们。

### <a name="to-find-the-available-endpoints-for-a-wcf-service"></a>查找 WCF 服务的可用终结点

1. 在**解决方案资源管理器**中，右键单击包含服务引用的项目的**app.config**文件，然后单击 "**打开**"。 文件将出现在代码编辑器中。

2. 在文件中搜索 `<Client>` 标记。

3. 在 `<Client>` 标记下搜索以查找以 `<Endpoint>`开头的标记。

     如果服务引用提供多个终结点，则将有两个或多个 `<Endpoint` 标记。

4. 在 `<EndPoint>` 标记中，你将找到 `name="`*SomeService*`"` 参数（其中*SomeService*表示终结点名称）。 这是终结点的名称，可将其传递给服务引用的构造函数的 `endpointConfigurationName As String` 重载。

## <a name="how-to-call-a-service-method-asynchronously"></a>如何：异步调用服务方法

Windows Communication Foundation （WCF）服务中的大多数方法都可以同步或异步调用。 异步调用方法使应用程序能够继续工作，同时在方法通过慢速连接进行操作时调用方法。

默认情况下，将服务引用添加到项目时，会将其配置为同步调用方法。 通过更改 "**配置服务引用**" 对话框中的设置，可以将行为更改为异步调用方法。

> [!NOTE]
> 此选项在每个服务的基础上进行设置。 如果异步调用服务的一种方法，则必须异步调用所有方法。

[!INCLUDE[note_settings_general](../data-tools/includes/note_settings_general_md.md)]

### <a name="to-call-a-service-method-asynchronously"></a>异步调用服务方法

1. 在**解决方案资源管理器**中，选择 "服务引用"。

2. 在 "**项目**" 菜单上，单击 "**配置服务引用**"。

3. 在 "**配置服务引用**" 对话框中，选中 "**生成异步操作**" 复选框。

## <a name="how-to-bind-data-returned-by-a-service"></a>如何：绑定服务返回的数据

您可以将 Windows Communication Foundation （WCF）服务返回的数据绑定到控件，就像您可以将任何其他数据源绑定到控件一样。 当您添加对 WCF 服务的引用时，如果该服务包含返回数据的复合类型，则它们将自动添加到 "**数据源**" 窗口中。

### <a name="to-bind-a-control-to-single-data-field-returned-by-a-wcf-service"></a>将控件绑定到 WCF 服务返回的单个数据字段

1. 在 **“数据”** 菜单上，单击 **“显示数据源”** 。

   随即出现“数据源”窗口。

2. 在 "**数据源**" 窗口中，展开服务引用的节点。 服务显示返回的任何复合类型。

3. 展开某个类型的节点。 显示该类型的数据字段。

4. 选择字段并单击下拉箭头以显示可用于数据类型的控件的列表。

5. 单击要绑定到的控件的类型。

6. 将字段拖到窗体上。 控件添加到窗体中，同时还添加了一个 <xref:System.Windows.Forms.BindingSource> 组件和一个 <xref:System.Windows.Forms.BindingNavigator> 组件。

7. 对于要绑定的任何其他字段，请重复步骤4到6。

### <a name="to-bind-a-control-to-composite-type-returned-by-a-wcf-service"></a>将控件绑定到 WCF 服务返回的复合类型

1. 在 "**数据**" 菜单上，选择 "**显示数据源**"。 随即出现“数据源”窗口。

2. 在 "**数据源**" 窗口中，展开服务引用的节点。 服务显示返回的任何复合类型。

3. 选择某个类型的节点并单击下拉箭头以显示可用选项的列表。

4. 单击 " **DataGridView** " 可在网格中显示数据，或单击 "**详细信息**" 以在单个控件中显示数据。

5. 将节点拖到窗体上。 控件添加到窗体中，同时还添加了一个 <xref:System.Windows.Forms.BindingSource> 组件和一个 <xref:System.Windows.Forms.BindingNavigator> 组件。

## <a name="how-to-configure-a-service-to-reuse-existing-types"></a>如何：将服务配置为重复使用现有类型

将服务引用添加到项目中时，会在本地项目中生成服务中定义的任何类型。 在许多情况下，当服务使用常见的 .NET 类型或在共享库中定义类型时，这会创建重复的类型。

为了避免此问题，默认情况下，将共享引用程序集中的类型。 如果要禁用一个或多个程序集的类型共享，可以在 "**配置服务引用**" 对话框中执行此操作。

### <a name="to-disable-type-sharing-in-a-single-assembly"></a>在单个程序集中禁用类型共享

1. 在**解决方案资源管理器**中，选择 "服务引用"。

2. 在 "**项目**" 菜单上，单击 "**配置服务引用**"。

3. 在 "**配置服务引用**" 对话框中，选择 "**在指定的引用程序集中重用类型**"。

4. 对于要在其中启用类型共享的每个程序集，选中相应的复选框。 若要禁用程序集的类型共享，请清除该复选框。

### <a name="to-disable-type-sharing-in-all-assemblies"></a>禁用所有程序集中的类型共享

1. 在**解决方案资源管理器**中，选择 "服务引用"。

2. 在 "**项目**" 菜单上，单击 "**配置服务引用**"。

3. 在 "**配置服务引用**" 对话框中，清除 "**重新使用引用的程序集中的类型**" 复选框。

## <a name="related-topics"></a>相关主题

| 职务 | 描述 |
| - | - |
| [演练：在 Windows 窗体中创建简单的 WCF 服务](../data-tools/walkthrough-creating-a-simple-wcf-service-in-windows-forms.md) | 提供在 Visual Studio 中创建和使用 WCF 服务的分步演示。 |
| [演练：使用 WPF 和 Entity Framework 创建 WCF Data Service](../data-tools/walkthrough-creating-a-wcf-data-service-with-wpf-and-entity-framework.md) | 提供有关如何在 Visual Studio 中创建和使用 WCF 数据服务的分步演示。 |
| [使用 WCF 开发工具](/dotnet/framework/wcf/using-the-wcf-development-tools) | 介绍如何在 Visual Studio 中创建和测试 WCF 服务。 |
| | [如何：添加、更新或删除 WCF Data Service 引用](../data-tools/how-to-add-update-or-remove-a-wcf-data-service-reference.md) |
| [服务引用疑难解答](../data-tools/troubleshooting-service-references.md) | 显示服务引用可能发生的一些常见错误以及如何防止这些错误。 |
| [调试 WCF 服务](../debugger/debugging-wcf-services.md) | 描述调试 WCF 服务时可能会遇到的常见调试问题和技术。 |
| [演练：创建 N 层数据应用程序](../data-tools/walkthrough-creating-an-n-tier-data-application.md) | 提供有关创建类型化数据集并将 TableAdapter 和数据集代码分离到多个项目中的分步说明。 |
| [“配置服务引用”对话框](../data-tools/configure-service-reference-dialog-box.md) | 介绍 "**配置服务引用**" 对话框的用户界面元素。 |

## <a name="reference"></a>引用

- <xref:System.ServiceModel>
- <xref:System.Data.Services>

## <a name="see-also"></a>另请参阅

- [适用于 NET 的 Visual Studio Data Tools](../data-tools/visual-studio-data-tools-for-dotnet.md)
