---
title: "EXP (Transact SQL) |Microsoft 文档"
ms.custom: 
ms.date: 03/06/2017
ms.prod: sql-non-specified
ms.reviewer: 
ms.suite: 
ms.technology:
- database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
f1_keywords:
- EXP_TSQL
- EXP
dev_langs:
- TSQL
helpviewer_keywords:
- exponential functions
- EXP function
ms.assetid: 5a9b8c52-6fb6-4e33-8b02-a878785b2f51
caps.latest.revision: 35
author: BYHAM
ms.author: rickbyh
manager: jhubbard
ms.translationtype: MT
ms.sourcegitcommit: 876522142756bca05416a1afff3cf10467f4c7f1
ms.openlocfilehash: c50757291a8f1fc3d58d8a9a131c4a24aa79a008
ms.contentlocale: zh-cn
ms.lasthandoff: 09/01/2017

---
# <a name="exp-transact-sql"></a>EXP (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all_md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  返回指定的指数值**float**表达式。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
-- Syntax for SQL Server, Azure SQL Database, Azure SQL Data Warehouse, Parallel Data Warehouse  
  
EXP ( float_expression )  
```  
  
## <a name="arguments"></a>参数  
 *float_expression*  
 是[表达式](../../t-sql/language-elements/expressions-transact-sql.md)类型的**float**或可以隐式转换为的类型**float**。  
  
## <a name="return-types"></a>返回类型  
 **float**  
  
## <a name="remarks"></a>注释  
 常量**e** (2.718281 …) 是自然对数的底。  
  
 一个数字的指数是常量**e**数的幂。 例如，EXP(1.0) = e^1.0 = 2.71828182845905，而 EXP(10) = e^10 = 22026.4657948067。  
  
 指数的自然对数的数字是数字本身： EXP (日志 (*n*)) =  *n* 。 并且的数字的指数的自然对数是该数字本身： 日志 (EXP (*n*)) =  *n* 。  
  
## <a name="examples"></a>示例  
  
### <a name="a-finding-the-exponent-of-a-number"></a>A. 查找数字的指数  
 以下示例声明一个变量，并返回指定变量 (`10`) 的指数值，并附有文字说明。  
  
```  
DECLARE @var float  
SET @var = 10  
SELECT 'The EXP of the variable is: ' + CONVERT(varchar,EXP(@var))  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----------------------------------------------------------  
The EXP of the variable is: 22026.5  
(1 row(s) affected)  
```  
  
### <a name="b-finding-exponentials-and-natural-logarithms"></a>B. 查找指数和自然对数  
 以下示例返回 `20` 的自然对数的指数值，以及 `20` 的指数的自然对数。 由于这两个函数彼此互为反函数，所以两种情况下的返回值都是 `20`。  
  
```  
SELECT EXP( LOG(20)), LOG( EXP(20))  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
---------------------- ----------------------  
20                     20  
  
(1 row(s) affected)  
```  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]和[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-finding-the-exponent-of-a-number"></a>C. 查找数字的指数  
 下面的示例返回指定的值的指数值 (`10`)。  
  
```  
SELECT EXP(10);  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----------  
22026.4657948067  
```  
  
### <a name="d-finding-exponential-values-and-natural-logarithms"></a>D. 查找指数值和自然对数  
 以下示例返回 `20` 的自然对数的指数值，以及 `20` 的指数的自然对数。 由于这两个函数彼此互为反函数，所以两种情况下的返回值都是 `20`。  
  
```  
SELECT EXP( LOG(20)), LOG( EXP(20));  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
-------------- -----------------  
20                  20  
```  
  
## <a name="see-also"></a>另请参阅  
 [数学函数 &#40;Transact SQL &#41;](../../t-sql/functions/mathematical-functions-transact-sql.md)   
 [日志 &#40;Transact SQL &#41;](../../t-sql/functions/log-transact-sql.md)   
 [LOG10 &#40;Transact SQL &#41;](../../t-sql/functions/log10-transact-sql.md)  
  
  

