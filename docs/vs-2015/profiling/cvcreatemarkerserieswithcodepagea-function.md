---
title: CvCreateMarkerSeriesWithCodePageA 函数 | Microsoft Docs
ms.custom: ''
ms.date: 2018-06-30
ms.prod: visual-studio-dev14
ms.reviewer: ''
ms.suite: ''
ms.technology:
- vs-ide-debug
ms.tgt_pltfrm: ''
ms.topic: article
f1_keywords:
- cvmakers/CvCreateMarkerSeriesWithCodePageA
helpviewer_keywords:
- CvCreateMarkerSeriesWithCodePageA method
ms.assetid: 3d7ed601-2222-4be9-a557-f217db008753
caps.latest.revision: 8
author: mikejo5000
ms.author: mikejo
manager: ghogen
ms.openlocfilehash: 3716cb41da056590a7978e49f4672d25b1bbdaae
ms.sourcegitcommit: 55f7ce2d5d2e458e35c45787f1935b237ee5c9f8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/22/2018
ms.locfileid: "47478883"
---
# <a name="cvcreatemarkerserieswithcodepagea-function"></a>CvCreateMarkerSeriesWithCodePageA 函数
[!INCLUDE[vs2017banner](../includes/vs2017banner.md)]

本主题的最新版本，请参阅[CvCreateMarkerSeriesWithCodePageA 函数](https://docs.microsoft.com/visualstudio/profiling/cvcreatemarkerserieswithcodepagea-function)。  
  
为给定提供程序和指定代码页创建标记系列。 此函数可用于为标记 API ANSI 函数写出的文本显式指定代码页。 设置代码页对不同计算机使用不同的区域设置或语言来捕获然后分析跟踪非常有用。 默认情况下使用 GetACP() 函数返回的代码页。  
  
## <a name="syntax"></a>语法  
  
```  
HRESULT CvCreateMarkerSeriesWithCodePageA(  
   _In_ PCV_PROVIDER pProvider,  
   _In_ LPCSTR pSeriesName,  
   _In_ UINT nTextCodePage,  
   _Out_ PCV_MARKERSERIES* ppMarkerSeries  
);  
```  
  
#### <a name="parameters"></a>参数  
 `pProvider`  
 先前由 CvInitProvider 初始化的提供程序对象。 不能为 NULL。  
  
 `pSeriesName`  
 标记系列名称。 不能为 NULL，但允许空字符串。  
  
 `nTextCodePage`  
 有效的代码页。  
  
 `ppMarkerSeries`  
 输出变量的地址，用于存储标记系列上下文。 不能为 NULL。  
  
## <a name="return-value"></a>返回值  
 成功创建标记系列时返回 S_OK，出现任何错误时返回错误代码。 使用 SUCCEEDED/FAILED 宏检查错误条件。  
  
## <a name="requirements"></a>要求  
 **标头：** cvmarkers.h  
  
## <a name="see-also"></a>请参阅  
 [C++ 库参考](../profiling/cpp-library-reference.md)


