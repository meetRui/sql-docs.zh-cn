---
title: 延隔时间 (DMX) |Microsoft 文档
ms.custom: ''
ms.date: 03/02/2016
ms.prod: analysis-services
ms.prod_service: analysis-services
ms.component: data-mining
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- LAG
dev_langs:
- DMX
helpviewer_keywords:
- Lag function
ms.assetid: 2da6df1a-5506-4871-a0f0-83292f1df41e
caps.latest.revision: 31
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: 929924bc7ef571a64bc4d7f6c26d20dece081821
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2018
---
# <a name="lag-dmx"></a>Lag (DMX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  返回当前事例的日期与定型集的最近日期之间的时间段。  
  
## <a name="syntax"></a>语法  
  
```  
  
Lag()  
```  
  
## <a name="return-type"></a>返回类型  
 整型类型的标量值。  
  
## <a name="remarks"></a>注释  
 如果**延隔时间**函数用于 KEY TIME 列所在的位置在嵌套表中的模型，该函数必须位于嵌套 select 语句。  
  
## <a name="examples"></a>示例  
 下面的示例返回用于模型定型的数据过去 12 个月内的用例。  
  
```  
SELECT * FROM [Forecasting].CASES  
WHERE Lag() < 12  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据挖掘扩展插件&#40;DMX&#41;函数引用](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [函数&#40;DMX&#41;](../dmx/functions-dmx.md)   
 [常规预测函数&#40;DMX&#41;](../dmx/general-prediction-functions-dmx.md)  
  
  
