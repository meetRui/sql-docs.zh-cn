---
title: getInt 方法 (int) |Microsoft 文档
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
apiname:
- SQLServerCallableStatement.getInt (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: c86792bb-096e-4c58-8b9e-74491ccf83a6
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 296e226f282a5480c35dfb61337487aded3f059c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2018
---
# <a name="getint-method-int"></a>getInt 方法 (int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索与指定参数的值**int** java 编程语言提供的参数索引。  
  
## <a name="syntax"></a>语法  
  
```  
  
public int getInt(int index)  
```  
  
#### <a name="parameters"></a>Parameters  
 *索引*  
  
 指示参数索引的 int。  
  
## <a name="return-value"></a>返回值  
 **Int**值。  
  
## <a name="exceptions"></a>异常  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>注释  
 由 java.sql.CallableStatement 接口中的 getInt 方法指定此 getInt 方法。  
  
 仅在支持此方法[!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)]可以安全地返回如 int、 smallint、 tinyint 和位的整数值的数据类型。 在任何其他数据类型上使用此方法会引发异常。  
  
## <a name="see-also"></a>另请参阅  
 [getInt 方法&#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getint-method-sqlservercallablestatement.md)   
 [SQLServerCallableStatement 成员](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [SQLServerCallableStatement 类](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
