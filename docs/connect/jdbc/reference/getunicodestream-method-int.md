---
title: getUnicodeStream 方法 (int) |Microsoft 文档
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
- SQLServerResultSet.getUnicodeStream (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 0de79b65-a25e-4028-9cc2-7ac02340115b
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 8034a89bde03c3994dc660ed7cfde390ff970357
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2018
---
# <a name="getunicodestream-method-int"></a>getUnicodeStream 方法 (int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索此当前行中指定的列索引的值[SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)为 Unicode 字符流的对象。  
  
> [!NOTE]  
>  此方法在 JDBC 规范中已不推荐使用，调用它将导致引发“尚未实现”异常。 相反，应使用[getCharacterStream](../../../connect/jdbc/reference/getcharacterstream-method-sqlserverresultset.md)方法。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.io.InputStream getUnicodeStream(int columnIndex)  
```  
  
#### <a name="parameters"></a>Parameters  
 columnIndex  
  
 指示列索引的 int。  
  
## <a name="return-value"></a>返回值  
 一个 InputStream 对象中。  
  
## <a name="exceptions"></a>异常  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>注释  
 由 java.sql.ResultSet 接口中的 getUnicodeString 方法指定此 getUnicodeString 方法。  
  
## <a name="see-also"></a>另请参阅  
 [getUnicodeStream 方法&#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getunicodestream-method-sqlserverresultset.md)   
 [SQLServerResultSet 成员](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet 类](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
