---
title: 终止和分离 |微软文档
ms.date: 11/04/2016
ms.topic: conceptual
helpviewer_keywords:
- programs, termination events
- debug engines, detaching from programs
ms.assetid: 268c1e51-6363-45d1-964c-1ab99bdfa4f9
author: acangialosi
ms.author: anthc
manager: jillfra
ms.workload:
- vssdk
ms.openlocfilehash: 0b88255d618ce42fa55d878f192d31523ba3f83b
ms.sourcegitcommit: 16a4a5da4a4fd795b46a0869ca2152f2d36e6db2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2020
ms.locfileid: "80712495"
---
# <a name="termination-and-detaching"></a>终止和分离
以下部分介绍正常终止。

## <a name="discussion"></a>讨论区
 [IDebugLoadCompleteEvent2](../../extensibility/debugger/reference/idebugloadcompleteevent2.md)或[IDebugEntryPointEvent2](../../extensibility/debugger/reference/idebugentrypointevent2.md)接口继续后，如果要调试的应用程序中没有断点、异常、运行时错误或无限循环，则正在调试的程序将运行到完成。 此过程是正常的终止。

 您必须发送[IDebugProgram销毁事件2](../../extensibility/debugger/reference/idebugprogramdestroyevent2.md)来实现正常终止。 正常终止需要运行[IDebugProgram销毁事件2：：getExitCode](../../extensibility/debugger/reference/idebugprogramdestroyevent2-getexitcode.md)方法。

## <a name="see-also"></a>请参阅
- [创建自定义调试引擎](../../extensibility/debugger/creating-a-custom-debug-engine.md)
