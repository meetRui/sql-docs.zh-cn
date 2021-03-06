---
title: WriteText 方法 |Microsoft 文档
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- _Stream::raw_WriteText
- _Stream::WriteText
helpviewer_keywords:
- WriteText method [ADO]
ms.assetid: 7a669048-13f4-4574-a2b1-985e089729d5
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a2b12293935df6f9afaf6a1691e2decce3f6c6f9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2018
---
# <a name="writetext-method"></a>WriteText 方法
将一个指定的文本字符串写入[流](../../../ado/reference/ado-api/stream-object-ado.md)对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
Stream.WriteText Data, Options  
```  
  
#### <a name="parameters"></a>Parameters  
 *数据*  
 A**字符串**值，该值包含要写入的字符中的文本。  
  
 *Options*  
 選擇性。 A [StreamWriteEnum](../../../ado/reference/ado-api/streamwriteenum.md)值，该值指定是否必须在指定的字符串的末尾写入的行的分隔符。  
  
## <a name="remarks"></a>注释  
 指定的字符串写入到**流**没有任何干预空格或每个字符串之间的字符的对象。  
  
 当前[位置](../../../ado/reference/ado-api/position-property-ado.md)设置为以下写入的数据的字符。 **WriteText**方法不截断流中的数据的其余部分。 如果要进行截断操作这些字符，调用[SetEOS](../../../ado/reference/ado-api/seteos-method.md)。  
  
 如果您编写过去的当前[EOS](../../../ado/reference/ado-api/eos-property.md)位置，[大小](../../../ado/reference/ado-api/size-property-ado-stream.md)的**流**将增加以包含新的任何字符，和**EOS**将移到新的最后一字节在**流**。  
  
> [!NOTE]
>  **WriteText**与文本流中使用方法 ([类型](../../../ado/reference/ado-api/type-property-ado-stream.md)是**adTypeText**)。 为二进制流 (**类型**是**adTypeBinary**)，使用[编写](../../../ado/reference/ado-api/write-method.md)。  
  
## <a name="applies-to"></a>适用范围  
 [流对象 (ADO)](../../../ado/reference/ado-api/stream-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [Write 方法](../../../ado/reference/ado-api/write-method.md)
