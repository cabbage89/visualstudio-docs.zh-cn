---
title: IDebugEngine启动2：：可以终止过程 |微软文档
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
- IDebugEngineLaunch2::CanTerminateProcess
helpviewer_keywords:
- IDebugEngineLaunch2::CanTerminateProcess
ms.assetid: 7973454d-c957-4123-a0ee-80ebcdbbd2d1
author: acangialosi
ms.author: anthc
manager: jillfra
ms.workload:
- vssdk
dev_langs:
- CPP
- CSharp
ms.openlocfilehash: 91c68e0a0e314015c1f2e6df2a96243c6ce854e7
ms.sourcegitcommit: 16a4a5da4a4fd795b46a0869ca2152f2d36e6db2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2020
ms.locfileid: "80730559"
---
# <a name="idebugenginelaunch2canterminateprocess"></a>IDebugEngineLaunch2::CanTerminateProcess
确定是否可以终止进程。

## <a name="syntax"></a>语法

```cpp
HRESULT CanTerminateProcess ( 
   IDebugProcess2* pProcess
);
```

```csharp
int CanTerminateProcess ( 
   IDebugProcess2 pProcess
);
```

## <a name="parameters"></a>参数
`pProcess`\
[在]表示要终止的进程的[IDebugProcess2](../../../extensibility/debugger/reference/idebugprocess2.md)对象。

## <a name="return-value"></a>返回值
 如果成功，返回`S_OK`;否则返回错误代码。 例如`S_FALSE`，如果引擎无法终止进程，则返回该进程，因为访问被拒绝。

## <a name="remarks"></a>备注
 如果此方法返回`S_OK`，则可以调用[终止进程](../../../extensibility/debugger/reference/idebugenginelaunch2-terminateprocess.md)方法来实际终止进程。

## <a name="see-also"></a>请参阅
- [IDebugEngineLaunch2](../../../extensibility/debugger/reference/idebugenginelaunch2.md)
- [IDebugProcess2](../../../extensibility/debugger/reference/idebugprocess2.md)
- [TerminateProcess](../../../extensibility/debugger/reference/idebugenginelaunch2-terminateprocess.md)
