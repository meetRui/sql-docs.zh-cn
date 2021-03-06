---
title: MoveRecordOptionsEnum |Microsoft 文档
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
f1_keywords:
- MoveRecordOptionsEnum
helpviewer_keywords:
- MoveRecordOptionsEnum enumeration [ADO]
ms.assetid: f53c2ce4-1021-4a45-92b8-775e8bebad99
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 18508d9103b0be9a0dae3cff013ca2732a00035c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2018
---
# <a name="moverecordoptionsenum"></a>MoveRecordOptionsEnum
指定的行为[记录](../../../ado/reference/ado-api/record-object-ado.md)对象[移动记录](../../../ado/reference/ado-api/moverecord-method-ado.md)方法。  
  
|常量|“值”|Description|  
|--------------|-----------|-----------------|  
|**adMoveUnspecified**|-1|默认值。 执行默认移动操作： 如果目标文件或目录已存在，并且该操作将更新超文本链接，该操作将失败。|  
|**adMoveOverWrite**|1|将覆盖目标文件或目录，即使它已存在。|  
|**adMoveDontUpdateLinks**|2|修改的默认行为**移动记录**方法不需要更新的源的超文本链接**记录**。 默认行为取决于提供程序的功能。 如果该提供程序是支持移动操作更新这些链接。 如果提供程序不能解决链接，或者如果未指定此值，移动成功甚至当链接具有未得到解决。|  
|**adMoveAllowEmulation**|4|请求的提供程序尝试模拟 （使用下载、 上载和删除操作） 移动。 如果尝试将移动**记录**失败，因为位于不同服务器上的目标 URL 或由与源不同的提供程序提供服务，这可能会导致增加的延迟或数据丢失，由于不同的提供程序功能时提供程序之间的移动资源。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 这些常量没有 ADO/WFC 等效项。  
  
## <a name="applies-to"></a>适用范围  
 [MoveRecord 方法 (ADO)](../../../ado/reference/ado-api/moverecord-method-ado.md)
