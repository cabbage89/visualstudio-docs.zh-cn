---
title: '[Content_types]xml 文件的结构 |微软文档'
ms.date: 11/04/2016
ms.topic: conceptual
helpviewer_keywords:
- content_types
- content types
- opc
- vsix
ms.assetid: 9c399598-b9fa-4da7-84b5-defbf82e9335
author: acangialosi
ms.author: anthc
manager: jillfra
ms.workload:
- vssdk
ms.openlocfilehash: 2761e012d32516265e61c8001491e3c605372ff5
ms.sourcegitcommit: 16a4a5da4a4fd795b46a0869ca2152f2d36e6db2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2020
ms.locfileid: "80699023"
---
# <a name="the-structure-of-the-content_typesxml-file"></a>[Content_types].xml 文件的结构
包含有关 VSIX 包中内容类型的信息。 Visual Studio 使用 [Content_Types]xml 文件来安装包，但它不会自行安装该文件。

> [!NOTE]
> 尽管本主题仅适用于 VSIX 包中使用的 [Content_Type]xml 文件，但 [Content_Types]xml 文件类型是*开放打包约定 （OPC）* 标准的一部分。 有关详细信息，请参阅 OPC：在 MSDN 网站上[打包数据的新标准](https://msdn.microsoft.com/magazine/cc163372.aspx)。

## <a name="attributes-and-elements"></a>特性和元素
 以下各节介绍根元素及其属性和子元素。

### <a name="root-element"></a>Root 元素

|元素|描述|
|-------------|-----------------|
|`Types`|包含枚举 VSIX 包中的文件类型的子元素。|

### <a name="attributes"></a>特性

|特性|描述|
|---------------|-----------------|
|`Xmlns`|（必需。用于此 [Content_Types]xml 文件的架构的位置。|

### <a name="attribute-name-attribute"></a>[属性名称]属性

| 值 | 说明 |
| - | - |
| `http://schemas.openformats.org/package/2006/content-types` | 内容类型架构的位置。 |

### <a name="child-elements"></a>子元素
 `Types` 元素可包含任意数量的 `Default` 元素。

|元素|描述|
|-------------|-----------------|
|`Default`|描述 VSIX 包中的内容类型。 包中的每个文件类型都必须有自己的`Default`元素。|

### <a name="attributes"></a>特性

|特性|描述|
|---------------|-----------------|
|`Extension`|VSIX 包中文件的文件名扩展名。|
|`ContentType`|描述与文件名扩展名关联的内容类型。|

### <a name="attribute-name-attribute"></a>[属性名称]属性
 Visual Studio 可识别关联`ContentType``Extension`类型的以下值。

|分机|ContentType|
|---------------|-----------------|
|txt|text/plain|
|普格德夫|text/plain|
|xml|text/xml|
|vsixmanifest|text/xml|
|htm 或 html|text/html|
|Rtf|应用程序/rtf|
|pdf|应用程序/pdf|
|GIF|image/gif|
|jpg 或 jpeg|图像/jpg|
|tiff|image/tiff|
|vsix|应用程序/zip|
|zip|应用程序/zip|
|dll|application/octet-stream|
|所有其他文件类型|application/octet-stream|

## <a name="example"></a>示例

### <a name="description"></a>描述
 以下 [Content_Types]xml 文件描述了典型的 VSIX 包。

### <a name="code"></a>代码

```
<?xml version="1.0" encoding="utf-8" ?>
<Types xmlns="http://schemas.openxmlformats.org/package/2006/content-types">
    <Default Extension="vsixmanifest" ContentType="text/xml" />
    <Default Extension="dll" ContentType="application/octet-stream" />
    <Default Extension="png" ContentType="application/octet-stream" />
    <Default Extension="txt" ContentType="text/plain" />
    <Default Extension="pkgdef" ContentType="text/plain" />
</Types>
```

## <a name="see-also"></a>请参阅
- [VSIX 包的剖析](../extensibility/anatomy-of-a-vsix-package.md)
- [VSIX 扩展架构 1.0 参考](https://msdn.microsoft.com/library/76e410ec-b1fb-4652-ac98-4a4c52e09a2b)
- [OPC：打包数据的新标准](https://msdn.microsoft.com/magazine/cc163372.aspx)
