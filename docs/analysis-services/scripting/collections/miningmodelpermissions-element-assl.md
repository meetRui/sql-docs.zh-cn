---
title: MiningModelPermissions 元素 (ASSL) |Microsoft 文档
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: assl
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 560e9a8aab34aef2ff5d60b844f8a7fa4a089c7a
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/10/2018
---
# <a name="miningmodelpermissions-element-assl"></a>MiningModelPermissions 元素 (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  包含的有关权限的集合[MiningModel](../../../analysis-services/scripting/objects/miningmodel-element-assl.md)元素。  
  
## <a name="syntax"></a>语法  
  
```xml  
  
<MiningModel>  
   ...  
   <MiningModelPermissions>  
      <MiningModelPermission xsi:type="Permission">...</MiningModelPermission>  
   </MiningModelPermissions>  
   ...  
</MiningModel>  
```  
  
## <a name="element-characteristics"></a>元素特征  
  
|特征|说明|  
|--------------------|-----------------|  
|数据类型和长度|无|  
|默认值|无|  
|基数|0-1：可出现一次且仅出现一次的可选元素。|  
  
## <a name="element-relationships"></a>元素关系  
  
|关系|元素|  
|------------------|-------------|  
|父元素|[MiningModel](../../../analysis-services/scripting/objects/miningmodel-element-assl.md)|  
|子元素|[MiningModelPermission](../../../analysis-services/scripting/objects/miningmodelpermission-element-assl.md)类型的[权限](../../../analysis-services/scripting/data-type/permission-data-type-assl.md)|  
  
## <a name="remarks"></a>注释  
 分析管理对象 (AMO) 对象模型中的相应元素是<xref:Microsoft.AnalysisServices.MiningModelPermissionCollection>。  
  
## <a name="see-also"></a>另请参阅  
 [权限数据类型 & #40;ASSL & #41;](../../../analysis-services/scripting/data-type/permission-data-type-assl.md)   
 [集合 & #40;ASSL & #41;](../../../analysis-services/scripting/collections/collections-assl.md)  
  
  
