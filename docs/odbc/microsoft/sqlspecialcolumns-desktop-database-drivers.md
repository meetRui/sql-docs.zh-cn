---
title: "SQLSpecialColumns （桌面数据库驱动程序） |Microsoft 文档"
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
- SQLSpecialColumns function [ODBC], Desktop Database Drivers
ms.assetid: 3de66fdf-053b-4354-979d-e76a5a5e975f
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: jhubbard
ms.translationtype: MT
ms.sourcegitcommit: f7e6274d77a9cdd4de6cbcaef559ca99f77b3608
ms.openlocfilehash: 6ddd78f90927378fd0a7f8dbd78a5c66abb01f4a
ms.contentlocale: zh-cn
ms.lasthandoff: 09/09/2017

---
# <a name="sqlspecialcolumns-desktop-database-drivers"></a>SQLSpecialColumns （桌面数据库驱动程序）
唯一索引将返回 （如果存在） SQL_BEST_ROWID 标志*fColType*。 将 SQL_ROWVER 标志不返回任何结果集。  
  
 所有行 Id 都具有 SQL_SCOPE_CURROW 的作用域。  
  
 模式匹配为不支持*szTableQualifier*或*szTableName*自变量。