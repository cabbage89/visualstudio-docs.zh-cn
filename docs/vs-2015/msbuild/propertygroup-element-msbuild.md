---
title: PropertyGroup 元素 (MSBuild) | Microsoft Docs
ms.custom: ''
ms.date: 2018-06-30
ms.prod: visual-studio-dev14
ms.reviewer: ''
ms.suite: ''
ms.technology:
- vs-ide-sdk
ms.tgt_pltfrm: ''
ms.topic: article
f1_keywords:
- http://schemas.microsoft.com/developer/msbuild/2003#PropertyGroup
dev_langs:
- VB
- CSharp
- C++
- jsharp
helpviewer_keywords:
- <PropertyGroup> element [MSBuild]
- PropertyGroup element [MSBuild]
ms.assetid: ff1e6c68-b9a1-4263-a1ce-dc3b829a64d4
caps.latest.revision: 24
author: mikejo5000
ms.author: mikejo
manager: ghogen
ms.openlocfilehash: a89a11de811eda28abc1ef8463ae29cbd81825a1
ms.sourcegitcommit: 55f7ce2d5d2e458e35c45787f1935b237ee5c9f8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/22/2018
ms.locfileid: "47477765"
---
# <a name="propertygroup-element-msbuild"></a>PropertyGroup 元素 (MSBuild)
[!INCLUDE[vs2017banner](../includes/vs2017banner.md)]

本主题的最新版本，请参阅[PropertyGroup 元素 (MSBuild)](https://docs.microsoft.com/visualstudio/msbuild/propertygroup-element-msbuild)。  
  
  
包含一组用户定义的 [Property](../msbuild/property-element-msbuild.md) 元素。 [!INCLUDE[vstecmsbuild](../includes/vstecmsbuild-md.md)] 项目中使用的每个 `Property` 元素必须是 `PropertyGroup` 元素的子元素。  
  
 \<Project>  
 \<PropertyGroup>  
  
## <a name="syntax"></a>语法  
  
```  
<PropertyGroup Condition="'String A' == 'String B'">  
    <Property1>...</Property1>  
    <Property2>...</Property2>  
</PropertyGroup>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  
 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|特性|描述|  
|---------------|-----------------|  
|条件|可选特性。<br /><br /> 要计算的条件。 有关详细信息，请参阅[条件](../msbuild/msbuild-conditions.md)。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[Property](../msbuild/property-element-msbuild.md)|可选元素。<br /><br /> 用户定义的属性名称，其中包含属性值。 `PropertyGroup` 元素中可能有零个或零个以上的 *Property* 元素。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[Project](../msbuild/project-element-msbuild.md)|[!INCLUDE[vstecmsbuild](../includes/vstecmsbuild-md.md)] 项目文件必需的根元素。|  
  
## <a name="example"></a>示例  
 以下代码示例演示如何基于条件设置属性。 在此示例中，如果 `CompileConfig` 属性的值为 `DEBUG`，则会设置 `PropertyGroup` 元素内部的 `Optimization`、`Obfuscate` 和 `OutputPath` 属性。  
  
```  
<PropertyGroup Condition="'$(CompileConfig)' == 'DEBUG'" >  
    <Optimization>false</Optimization>  
    <Obfuscate>false</Obfuscate>  
    <OutputPath>$(OutputPath)\debug</OutputPath>  
</PropertyGroup>  
```  
  
## <a name="see-also"></a>请参阅  
 [项目文件架构参考](../msbuild/msbuild-project-file-schema-reference.md)  
 [MSBuild 属性](msbuild-properties1.md)


