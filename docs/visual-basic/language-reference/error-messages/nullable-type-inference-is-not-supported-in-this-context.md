---
title: 该上下文中不支持可以为 null 的类型推理
ms.date: 07/20/2015
f1_keywords:
- vbc36629
- bc36629
helpviewer_keywords:
- BC36629
ms.assetid: 0a1e2dbc-d9a4-433d-9306-c5540782b81d
ms.openlocfilehash: 52e5391fbcf30a4dada4d64a0e810c900ea85806
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409382"
---
# <a name="nullable-type-inference-is-not-supported-in-this-context"></a>该上下文中不支持可以为 null 的类型推理
值类型和结构可以声明为 null。  
  
```vb  
Dim a? As Integer  
Dim b As Integer?  
```  
  
 但是，不能将可为 null 的声明与类型推理结合使用。 下面的示例将导致此错误。  
  
```vb  
' Not valid.  
' Dim c? = 10  
' Dim d? = a  
```  
  
 **错误 ID：** BC36629  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
- 使用 `As` 子句将变量声明为可以为 null 的值类型。  
  
## <a name="see-also"></a>另请参阅

- [可以为 null 的值类型](../../programming-guide/language-features/data-types/nullable-value-types.md)
- [局部类型推理](../../programming-guide/language-features/variables/local-type-inference.md)
