---
title: 基于文件的驱动程序诊断示例 |Microsoft 文档
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
- file-based driver diagnostic [ODBC]
- diagnostic information [ODBC], examples
- error messages [ODBC], diagnostic messages
ms.assetid: 0575fccd-4641-478d-a3cc-5a764e35bae2
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d58072bebac57eca8976064b85a25999475a9586
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2018
---
# <a name="file-based-driver-diagnostic-example"></a>基于文件的驱动程序诊断示例
基于文件的驱动程序可以充当同时 ODBC 驱动程序和数据源。 因此可以生成错误和警告同时作为组件中的 ODBC 连接和作为数据源。 它也是接口与驱动程序管理器中的组件，因为它格式，并返回自变量**SQLGetDiagRec**。  
  
 例如，如果 dBASE Microsoft® 驱动程序无法分配足够的内存，它可能返回中的以下值**SQLGetDiagRec**:  
  
```  
SQLSTATE:         "HY001"  
Native Error:      42052  
Diagnostic Msg:   "[Microsoft][ODBC dBASE Driver]Unable to allocate sufficient memory."  
```  
  
 因为此错误不与数据源相关的该驱动程序只添加前缀到诊断消息供应商 ([Microsoft]) 和驱动程序 ([ODBC dBASE 驱动程序])。  
  
 如果该驱动程序找不到文件 Employee.dbf，它可能会返回中的以下值**SQLGetDiagRec**:  
  
```  
SQLSTATE:         "42S02"  
Native Error:      -1305  
Diagnostic Msg:   "[Microsoft][ODBC dBASE Driver][dBASE]No such table or object"  
```  
  
 因为此错误与数据源相关的该驱动程序会添加数据源 ([dBASE]) 的文件格式作为前缀到诊断消息中。 因为该驱动程序也是 interfaced 与数据源的组件，它为供应商 ([Microsoft]) 和驱动程序 ([ODBC dBASE 驱动程序]) 中添加前缀。
