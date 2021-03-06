---
title: IDiaSymbol：： get_machineType |Microsoft Docs
ms.date: 11/04/2016
ms.topic: conceptual
dev_langs:
- C++
helpviewer_keywords:
- IDiaSymbol::get_machineType method
ms.assetid: 30870b10-6f32-45c6-a0d7-020dea707710
author: mikejo5000
ms.author: mikejo
manager: jillfra
ms.workload:
- multiple
ms.openlocfilehash: 5ea9f5762e265b2892a906060a430db03f7b7d67
ms.sourcegitcommit: 5f6ad1cefbcd3d531ce587ad30e684684f4c4d44
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2019
ms.locfileid: "72739835"
---
# <a name="idiasymbolget_machinetype"></a>IDiaSymbol::get_machineType
检索目标 CPU 的类型。

## <a name="syntax"></a>语法

```C++
HRESULT get_machineType ( 
   DWORD* pRetVal
);
```

#### <a name="parameters"></a>参数
 `pRetVal`

弄从指定目标 CPU 类型的[IMAGE_FILE_MACHINE_ 常量](/windows/desktop/SysInfo/image-file-machine-constants)中返回一个值。

## <a name="return-value"></a>返回值
 如果成功，将返回 `S_OK`;否则，将返回 `S_FALSE` 或错误代码。

> [!NOTE]
> @No__t_0 的返回值意味着该属性对符号不可用。

## <a name="see-also"></a>请参阅
- [IMAGE_FILE_MACHINE_ 常量](/windows/desktop/SysInfo/image-file-machine-constants) 
- [IDiaSymbol](../../debugger/debug-interface-access/idiasymbol.md)