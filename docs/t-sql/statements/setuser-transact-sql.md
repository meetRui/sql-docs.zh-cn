---
title: "SETUSER (Transact SQL) |Microsoft 文档"
ms.custom: 
ms.date: 07/26/2017
ms.prod: sql-non-specified
ms.reviewer: 
ms.suite: 
ms.technology:
- database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
f1_keywords:
- SETUSER_TSQL
- SETUSER
dev_langs:
- TSQL
helpviewer_keywords:
- delegation [SQL Server]
- impersonation [SQL Server]
- SETUSER statement
- user impersonation [SQL Server]
ms.assetid: 7acfac5c-9ad6-4226-b874-7add36c4ea43
caps.latest.revision: 27
author: BYHAM
ms.author: rickbyh
manager: jhubbard
ms.translationtype: MT
ms.sourcegitcommit: 876522142756bca05416a1afff3cf10467f4c7f1
ms.openlocfilehash: ab9805dda5f5b2b4199cb40ef3c28af1d5b4379f
ms.contentlocale: zh-cn
ms.lasthandoff: 09/01/2017

---
# <a name="setuser-transact-sql"></a>SETUSER (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx_md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  允许的成员**sysadmin**固定服务器角色或模拟其他用户数据库的所有者。  
  
> [!IMPORTANT]  
>  包含 SETUSER 只是为了保持向后兼容性。 在以后的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本中可能不再支持 SETUSER。 我们建议你使用[EXECUTE AS](../../t-sql/statements/execute-as-transact-sql.md)相反。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
SETUSER [ 'username' [ WITH NORESET ] ]   
```  
  
## <a name="arguments"></a>参数  
  *用户名*   
 当前数据库中被模拟的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户名或 Windows 用户名。 当*用户名*未指定，则重置的系统管理员或数据库所有者模拟用户的原始标识。  
  
 WITH NORESET  
 指定，后续 SETUSER 语句 (具有未指定*用户名*) 应不会重置的用户标识为系统管理员或数据库所有者。  
  
## <a name="remarks"></a>注释  
 成员时可以使用 SETUSER **sysadmin**固定服务器角色或数据库的所有者以采用另一个要测试的其他用户的权限的用户的标识。 中的 db_owner 固定数据库角色的成员身份是不够的。  
  
 仅对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户使用 SETUSER。 不支持 Windows 用户使用 SETUSER。 如果使用 SETUSER 来模拟其他用户的标识，则进行模拟的用户创建的任何对象均由被模拟的用户所有。 例如，如果数据库所有者假定用户的标识**Margaret**并创建一个名为表**订单**、**订单**表归**Margaret**，不是系统管理员。  
  
 SETUSER 一直保持有效，直到发出其他 SETUSER 语句或用 USE 语句更改当前数据库为止。  
  
> [!NOTE]  
>  如果使用了 SETUSER WITH NORESET，数据库所有者或系统管理员必须注销然后重新登录，才能重新建立自己的权限。  
  
## <a name="permissions"></a>Permissions  
 要求的成员身份**sysadmin**固定服务器角色，或者必须是数据库的所有者。 中的成员身份**db_owner**固定的数据库角色不足  
  
## <a name="examples"></a>示例  
 以下示例显示了数据库所有者如何采用其他用户的标识。 用户 `mary` 已创建了一个名为 `computer_types` 的表。 通过使用 SETUSER，数据库所有者可以模拟 `mary` 授予用户 `joe` 访问 `computer_types` 表的权限，然后重置自己的标识。  
  
```  
SETUSER 'mary';  
GO  
GRANT SELECT ON computer_types TO joe;  
GO  
--To revert to the original user  
SETUSER;  
```  
  
## <a name="see-also"></a>另请参阅  
 [DENY (Transact-SQL)](../../t-sql/statements/deny-transact-sql.md)   
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE (Transact-SQL)](../../t-sql/statements/revoke-transact-sql.md)   
 [USE (Transact-SQL)](../../t-sql/language-elements/use-transact-sql.md)  
  
  
