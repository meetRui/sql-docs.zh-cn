---
title: Members 元素 (XMLA) |Microsoft 文档
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 598873b186106e39221b446492828f323642c438
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/10/2018
---
# <a name="members-element-xmla"></a>Members 元素 (XMLA)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../../includes/ssas-appliesto-sqlas-aas.md)]
  包含一套[成员](../../../analysis-services/xmla/xml-elements-properties/member-element-xmla.md)包含由容器的父元素[CrossProduct](../../../analysis-services/xmla/xml-elements-properties/crossproduct-element-xmla.md)元素。  
  
## <a name="syntax"></a>语法  
  
```xml  
  
<CrossProduct>  
   <Members Hierarchy="string">  
      <Member>...</Member>  
   </Members>  
   ...  
</CrossProduct>  
```  
  
## <a name="element-characteristics"></a>元素特征  
  
|特征|说明|  
|--------------------|-----------------|  
|数据类型和长度|无|  
|默认值|无|  
|基数|0-n：可多次出现的可选元素。|  
  
## <a name="element-relationships"></a>元素关系  
  
|关系|元素|  
|------------------|-------------|  
|父元素|[CrossProduct](../../../analysis-services/xmla/xml-elements-properties/crossproduct-element-xmla.md)|  
|子元素|[成员](../../../analysis-services/xmla/xml-elements-properties/member-element-xmla.md)|  
  
## <a name="attributes"></a>属性  
  
|Attribute|Description|  
|---------------|-----------------|  
|层次结构|所需**字符串**属性。 向其成员包含的层次结构的名称**成员**元素属于。|  
  
## <a name="remarks"></a>注释  
 当客户端应用程序设置**AxisFormat**属性*ClusterFormat*，每个轴上的成员被划分到其中的每个群集都表示之间有序集的叉积的群集从每个层次结构的成员。 每个**轴**元素包含一个或多个**CrossProduct**元素。 每个**CrossProduct**元素包含**成员**在轴上的每个层次结构的元素。 **成员**元素，反过来，包含一个**成员**跨产品中包含指定层次结构的每个成员的元素。  
  
## <a name="example"></a>示例  
 下面的示例演示的结构**成员**元素时客户端指定*ClusterFormat*为**AxisFormat**给定的 XMLA 属性轴的以下成员：  
  
||||||  
|-|-|-|-|-|  
|**时间**层次结构|1999|1999|2000|2001|  
|**类别**层次结构|Actual|Budget|Budget|Budget|  
|Clusters|分类 1|分类 1|分类 1|Cluster 2|  
  
```  
<Axes>  
   <Axis name="Axis0">  
      <CrossProduct Size = "4">  
         <Members Hierarchy="Time">  
            <Member>  
               <UName>[Time].[1999]</UName>  
               ...  
            </Member>  
            <Member>  
               <UName>[Time].[2000]</UName>  
               ...  
            </Member>  
         </Members>  
         <Members Hierarchy="Category">  
            <Member>  
               <UName>[Scenario].[Actual]</UName>  
               ...  
            </Member>  
            <Member>  
               <UName>[Scenario].[Budget]</UName>  
               ...  
            </Member>  
         </Members>  
      </CrossProduct>  
      <CrossProduct Size = "1">  
         <Members Hierarchy="Time">  
            <Member>  
               <UName>[Time].[2001]</UName>  
               ...  
            </Member>  
         </Members>  
         <Members Hierarchy="Category">  
            <Member>  
               <UName>[Scenario].[Budget]</UName>  
               ...  
            </Member>  
         </Members>  
      </CrossProduct>  
   </Axis>  
   ...  
</Axes>  
```  
  
## <a name="see-also"></a>另请参阅  
 [属性 & #40;XMLA & #41;](../../../analysis-services/xmla/xml-elements-properties/xml-elements-properties.md)  
  
  
