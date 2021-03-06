---
title: 属性对象 (ADOX) |Microsoft 文档
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
apitype: COM
helpviewer_keywords:
- Property object [ADOX]
ms.assetid: 6a56def6-dbe6-4ccc-a491-8d076889f019
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 80af9304927f526422650f63264f06539cb14b29
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2018
---
# <a name="property-object-adox"></a>属性对象 (ADOX)
表示 ADOX 对象的特征。  
  
## <a name="remarks"></a>注释  
 ADOX 对象具有两种类型的属性： 内置和动态。  
  
 内置属性是立即可供任何新对象，使用 MyObject.Property 语法使用这些属性。 它们不会为属性对象的对象会显示[属性集合](../../../ado/reference/ado-api/properties-collection-ado.md)，因此，尽管您可以更改其值，但不能修改其特征。  
  
 动态属性定义的基础数据提供程序，并显示在相应的 ADOX 对象的属性集合。  动态属性可以引用只能通过使用 MyObject.Properties(0) 或 MyObject.Properties("Name") 语法的集合。  
  
 无法删除这两种属性。  
  
 动态属性对象具有自己的四个内置属性：  
  
 [名称](../../../ado/reference/ado-api/name-property-ado.md)属性是一个字符串，标识的属性。  
  
 [类型](../../../ado/reference/ado-api/type-property-ado.md)属性是一个整数，指定的属性数据类型。  
  
 [值](../../../ado/reference/ado-api/value-property-ado.md)属性是一个包含属性设置的变量。 值为属性对象的默认属性。  
  
 [属性](../../../ado/reference/ado-api/attributes-property-ado.md)属性是一个长值，指示特征的特定于所提供的属性。  
  
 本部分包含以下主题。  
  
-   [ADOX 属性对象属性、方法和事件](../../../ado/reference/adox-api/adox-property-object-properties-methods-and-events.md)
