---
title: 游标库操作 |Microsoft 文档
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
- ODBC cursor library [ODBC], backward compatibility
- compatibility [ODBC], cursor library
- upgrading applications [ODBC], cursor library
- application upgrades [ODBC], cursor library
- backward compatibility [ODBC], cursor library
- cursor library [ODBC], backward compatibility
ms.assetid: 04d514b1-dc4d-4b84-bf35-60f4657ef1f6
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 60d0d5e9cb136b586fad675ad48a4b84ce353bc7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2018
---
# <a name="cursor-library-operations"></a>游标库操作
> [!IMPORTANT]  
>  将 Windows 的未来版本中删除该功能。 避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 Microsoft 建议使用驱动程序的游标功能。  
  
 如果应用程序使用 ODBC 2 *.x*驱动程序，可以对 ODBC 3 的调用。*x*游标库应用程序可能能够使用 ODBC 3。*x* ODBC 2 不支持的功能 *.x*驱动程序。 应注意如何使用这些功能，但是应用程序编写器。 ODBC 3 的使用。*x*游标库不会使 ODBC 2 *.x*到 ODBC 3 的驱动程序。*x*驱动程序。
