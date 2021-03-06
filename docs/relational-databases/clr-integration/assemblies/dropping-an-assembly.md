---
title: 删除程序集 |Microsoft 文档
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: clr
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- removing assemblies
- DROP ASSEMBLY statement
- assemblies [CLR integration], removing
- dropping assemblies
ms.assetid: 03481034-dc91-4488-ab24-ba44243e2690
caps.latest.revision: 16
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 46be0c285631013e3847e7ad7c0e8f03a543ad8e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2018
---
# <a name="dropping-an-assembly"></a>删除程序集
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  使用 CREATE ASSEMBLY 语句在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中注册的程序集可以进行删除（如果不再需要程序集所提供的功能）。 删除程序集时，将从数据库中删除程序集和它的所有关联文件，如调试文件。 若要删除程序集，可按照如下语法使用 DROP ASSEMBLY 语句：  
  
```  
DROP ASSEMBLY MyDotNETAssembly  
```  
  
 DROP ASSEMBLY 不会干扰引用当前正在运行的程序集的任何代码，但是，执行 DROP ASSEMBLY 之后，任何调用程序集代码的尝试都将失败。  
  
 如果程序集被存在于该数据库中的另一个程序集引用，或者它被当前数据库中的公共语言运行时 (CLR) 函数、过程、触发器、用户定义类型 (UDT) 或用户定义聚合 (UDA) 使用，则 DROP ASSEMBLY 返回错误。 首先可使用 DROP AGGREGATE、DROP FUNCTION、DROP PROCEDURE、DROP TRIGGER 和 DROP TYPE 语句删除该程序集中所包含的所有托管数据库对象。  
  
## <a name="removing-a-udt-from-the-database"></a>从数据库中删除 UDT  
 使用 DROP TYPE 语句从当前数据库中删除 UDT。 删除了 UDT 之后，可使用 DROP ASSEMBLY 语句从数据库中删除程序集。  
  
 如果对象依赖于 UDT，则 DROP TYPE 语句将失败，如以下情况：  
  
-   数据库中的表包含使用 UDT 定义的列。  
  
-   在数据库中使用 WITH SCHEMABINDING 子句创建了使用 UDT 变量或参数的函数、存储过程或触发器。  
  
### <a name="finding-udt-dependencies"></a>查找 UDT 依赖关系  
 在执行 DROP TYPE 语句之前，首先必须删除所有依赖对象。 以下[!INCLUDE[tsql](../../../includes/tsql-md.md)]查询找到的所有列和参数，在 UDT **AdventureWorks**数据库。  
  
```  
USE Adventureworks;  
SELECT o.name AS major_name, o.type_desc AS major_type_desc  
     , c.name AS minor_name, c.type_desc AS minor_type_desc  
     , at.assembly_class  
  FROM (  
        SELECT object_id, name, user_type_id, 'SQL_COLUMN' AS type_desc  
          FROM sys.columns  
     UNION ALL  
        SELECT object_id, name, user_type_id, 'SQL_PROCEDURE_PARAMETER'  
          FROM sys.parameters  
     ) AS c  
  JOIN sys.objects AS o  
    ON o.object_id = c.object_id  
  JOIN sys.assembly_types AS at  
    ON at.user_type_id = c.user_type_id;   
```  
  
## <a name="see-also"></a>另请参阅  
 [管理 CLR 集成程序集](../../../relational-databases/clr-integration/assemblies/managing-clr-integration-assemblies.md)   
 [更改程序集](../../../relational-databases/clr-integration/assemblies/altering-an-assembly.md)   
 [创建程序集](../../../relational-databases/clr-integration/assemblies/creating-an-assembly.md)   
 [拖放聚合&#40;Transact SQL&#41;](../../../t-sql/statements/drop-aggregate-transact-sql.md)   
 [DROP FUNCTION (Transact-SQL)](../../../t-sql/statements/drop-function-transact-sql.md)   
 [DROP PROCEDURE & #40;Transact SQL & #41;](../../../t-sql/statements/drop-procedure-transact-sql.md)   
 [DROP TRIGGER (Transact-SQL)](../../../t-sql/statements/drop-trigger-transact-sql.md)   
 [拖放类型&#40;Transact SQL&#41;](../../../t-sql/statements/drop-type-transact-sql.md)  
  
  
