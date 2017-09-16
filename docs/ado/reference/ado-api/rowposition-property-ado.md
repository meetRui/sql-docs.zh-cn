---
title: "RowPosition 属性 (ADO) |Microsoft 文档"
ms.prod: sql-non-specified
ms.technology:
- drivers
ms.custom: 
ms.date: 01/19/2017
ms.reviewer: 
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: article
apitype: COM
f1_keywords:
- ADORecordConstruction::put_RowPosition
- ADORecordConstruction::PutRowPosition
- ADORecordConstruction::GetRowPosition
- ADORecordConstruction::RowPosition
- ADORecordConstruction::get_RowPosition
helpviewer_keywords:
- RowPosition property [ADO]
ms.assetid: 9d068fed-39bf-4842-afc3-686a2af2145d
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: jhubbard
ms.translationtype: MT
ms.sourcegitcommit: f7e6274d77a9cdd4de6cbcaef559ca99f77b3608
ms.openlocfilehash: 00206531bf8154d509f79e7229b1a2da319fa5fa
ms.contentlocale: zh-cn
ms.lasthandoff: 09/09/2017

---
# <a name="rowposition-property-ado"></a>RowPosition 属性 (ADO)
获取或设置 OLE DB **RowPosition**对象从/上**ADORecordsetConstruction**对象。 当你使用**put_RowPosition**设置**RowPosition**对象，生成**记录集**对象使用**RowPosition**对象传递给确定当前行。  
  
 读/写。  
  
## <a name="syntax"></a>语法  
  
```  
HRESULT get_RowPosition([out, retval] IUnknown** ppRowPos);  
HRESULT put_RowPosition([in] IUnknown* pRowPos);  
```  
  
## <a name="parameters"></a>Parameters  
 *ppRowPos*  
 指向 OLE DB **RowPosition**对象。  
  
 *PRowPos*  
 OLE DB **RowPosition**对象。  
  
## <a name="return-values"></a>返回值  
 此属性方法返回的标准的 HRESULT 值，包括，则为 S_OK 和 E_FAIL。  
  
## <a name="remarks"></a>注释  
 设置此属性时，如果**行集**对象上**RowPosition**对象与不同**行集**对象上**记录集**对象，前者将覆盖后者。 相同的行为将应用于当前**章**的**RowPosition**以及。  
  
## <a name="applies-to"></a>适用范围  
 [ADORecordsetConstruction 接口](../../../ado/reference/ado-api/adorecordsetconstruction-interface.md)