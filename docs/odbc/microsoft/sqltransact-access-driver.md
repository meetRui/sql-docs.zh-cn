---
title: "SQLTransact （Access 驱动程序） |Microsoft 文档"
ms.custom: 
ms.date: 01/19/2017
ms.prod: sql-non-specified
ms.reviewer: 
ms.suite: 
ms.technology:
- drivers
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- Access driver [ODBC], SQLTransact
- SQLTransact function [ODBC], Access Driver
ms.assetid: 892b79c7-9e20-4d1f-bc60-d4b25694ca25
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: jhubbard
ms.translationtype: MT
ms.sourcegitcommit: f7e6274d77a9cdd4de6cbcaef559ca99f77b3608
ms.openlocfilehash: 657d0324132b279300e2a61151c790f066e4a3f5
ms.contentlocale: zh-cn
ms.lasthandoff: 09/09/2017

---
# <a name="sqltransact-access-driver"></a>SQLTransact （Access 驱动程序）
> [!NOTE]  
>  本主题提供访问特定于驱动程序信息。 有关此函数的常规信息，请参阅下的相应主题[ODBC API 参考](../../odbc/reference/syntax/odbc-api-reference.md)。  
  
 当使用 Microsoft Access 驱动程序时，为支持 SQL_COMMIT 和 SQL_ROLLBACK *fType*对的调用中的自变量**SQLTransact**。  
  
 如果在提交过程中发生故障，受影响的数据库，则可以修复使用修复数据库选项，在 Microsoft Access 驱动程序设置中，或通过使用中的 REPAIR_DB 关键字**SQLConfigDataSource**函数。