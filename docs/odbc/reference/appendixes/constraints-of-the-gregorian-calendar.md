---
title: 约束的公历 |Microsoft 文档
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- data types [ODBC], Gregorian calendar
- Gregorian calendar [ODBC]
ms.assetid: 70667410-c582-4369-8e06-9d98e21cd2bf
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1cea1f4b4dd5b56feee64623bd6ff2355ce4332c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2018
---
# <a name="constraints-of-the-gregorian-calendar"></a>公历的约束
Date 和 datetime 数据类型和尾随字段 interval 数据类型必须符合的公历的约束。 这些约束如下所示：  
  
-   月字段的值必须介于 1 和 12 （含) 之间。  
  
-   日期字段的值必须介于 1 和在每月天数范围中。 在每月天数取决于年和月字段的值，并且可以 28、 29、 30 或 31。 （在每月天数可以还依赖于它是否为闰年。）  
  
-   小时字段的值必须介于 0 和 23，（含) 之间。  
  
-   分钟的字段的值必须介于 0 到 59 （含) 之间。  
  
-   对于 interval 数据类型的尾随秒字段，第二个字段的值必须介于 0 和 59.9 (*n*) (含） 之间，其中*n*是以秒的小数部分精度的数字个数。  
  
-   对于日期时间数据类型的尾随秒字段，第二个字段的值必须介于 0 和 61.9 (*n*) (含） 之间，其中*n*指定"9"的数字个数和的值*n*是秒的小数部分精度。 （的秒的范围允许多达两个闰秒，以便保持 sidereal 时间同步。）
