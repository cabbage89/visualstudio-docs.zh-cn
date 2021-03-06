---
title: 错误：工作组远程登录失败 |Microsoft Docs
ms.date: 11/04/2016
ms.topic: troubleshooting
f1_keywords:
- vs.debug.error.workgroup_remote_logon_failure
dev_langs:
- CSharp
- VB
- FSharp
- JScript
- C++
helpviewer_keywords:
- logon failure, remote debugging
- remote debugging, logon failure
author: mikejo5000
ms.author: mikejo
manager: jillfra
ms.workload:
- multiple
ms.openlocfilehash: 9d1ee0cfbd021eb7d6a03a791713d187d3c8877c
ms.sourcegitcommit: 5f6ad1cefbcd3d531ce587ad30e684684f4c4d44
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2019
ms.locfileid: "72736262"
---
# <a name="error-workgroup-remote-logon-failure"></a>错误：工作组远程登录失败
此错误显示如下：

 登录失败: 未知的用户名或密码不正确

 **原因**

 当在工作组上的计算机进行调试，并尝试连接到远程计算机时可能发生此错误。 可能的原因包括：

- 远程计算机上没有匹配用户名和密码的帐户。

- 如果 Visual Studio 计算机和远程计算机都在工作组上，远程计算机的默认**本地安全策略**设置可能导致此错误。 **本地安全策略**默认设置为**仅来宾-本地用户以来宾身份验证**。 要在此设置上调试，必须在远程计算机上将设置更改为**经典-本地用户以自己的身份验证**。

> [!NOTE]
> 你必须是管理员才能执行以下任务。

### <a name="to-open-the-local-security-policy-window"></a>打开“本地安全策略”窗口

1. 启动 **secpol.msc** Microsoft 管理控制台管理单元。 在 Windows 搜索、Windows“运行”框中或命令提示符处键入 secpol.msc。

### <a name="to-add-user-rights-assignments"></a>添加用户权限分配

1. 打开“本地安全策略”窗口。

2. 展开“本地策略”文件夹。

3. 单击“用户权限分配”。

4. 在**策略**列中，双击**调试程序**，以在**本地安全策略设置**对话框中查看当前本地组策略分配。

     ![本地安全策略用户权限](../debugger/media/dbg_err_localsecuritypolicy_userrightsdebugprograms.png "DBG_ERR_LocalSecurityPolicy_UserRightsDebugPrograms")

5. 若要添加新用户，请单击“添加用户或组”按钮。

### <a name="to-change-the-sharing-and-security-model"></a>更改“共享和安全模型”

1. 打开“本地安全策略”窗口。

2. 展开“本地策略”文件夹。

3. 单击“安全选项”。

4. 在中**策略**列中，双击**网络访问：本地帐户的共享和安全模型**。

5. 在**网络访问： 本地帐户的共享和安全模型**对话框中，将值更改为**经典-本地用户以自己的身份验证**，然后单击**应用**按钮。

     ![本地安全策略安全选项](../debugger/media/dbg_err_localsecuritypolicy_securityoptions_networkaccess.png "DBG_ERR_LocalSecurityPolicy_SecurityOptions_NetworkAccess")

## <a name="see-also"></a>请参阅
- [远程调试错误和疑难解答](../debugger/remote-debugging-errors-and-troubleshooting.md)
- [Remote Debugging](../debugger/remote-debugging.md)