---
title: SqlTypes 和数据集
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 9172c20a-9876-4b3b-9c97-1963c02b1993
ms.openlocfilehash: dea5a2017479443cb747d31e253c1c83585ddd09
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/07/2019
ms.locfileid: "70791497"
---
# <a name="sqltypes-and-the-dataset"></a>SqlTypes 和数据集
ADO.NET 2.0 通过 `DataSet` 命名空间引入了对 <xref:System.Data.SqlTypes> 的增强类型支持。 <xref:System.Data.SqlTypes> 中的类型旨在提供具有与 SQL Server 数据库中的数据类型相同的语义和精度的数据类型。 <xref:System.Data.SqlTypes> 中的每个数据类型在 SQL Server 中都具有等效的数据类型，并具有相同的基础数据表示形式。  
  
 在使用 SQL Server 数据类型时，在 <xref:System.Data.SqlTypes> 中直接使用 <xref:System.Data.DataSet> 具有多个优点。 <xref:System.Data.SqlTypes> 支持与 SQL Server 本机数据类型相同的语义。 通过在 <xref:System.Data.SqlTypes> 的定义中指定 <xref:System.Data.DataColumn> 之一，可避免在将 decimal 或 numeric 数据类型转换为公共语言运行库 (CLR) 数据类型之一时可能造成的精度损失。  
  
## <a name="example"></a>示例  
 以下示例创建 <xref:System.Data.DataTable> 对象，并使用 <xref:System.Data.DataColumn> 而非 CLR 类型来显式定义 <xref:System.Data.SqlTypes> 数据类型。 代码将用 SQL Server 中的 AdventureWorks 数据库的 Sales.SalesOrderDetail 表中的数据填充 <xref:System.Data.DataTable>。 控制台窗口中显示的输出显示每个列的数据类型以及从 SQL Server 检索的值。  
  
 [!code-csharp[DataWorks SqlTypes.GetTypeAW#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlTypes.GetTypeAW/CS/source.cs#1)]
 [!code-vb[DataWorks SqlTypes.GetTypeAW#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlTypes.GetTypeAW/VB/source.vb#1)]  
  
## <a name="see-also"></a>请参阅

- [SQL Server 数据类型映射](../sql-server-data-type-mappings.md)
- [配置参数和参数数据类型](../configuring-parameters-and-parameter-data-types.md)
- [ADO.NET 概述](../ado-net-overview.md)
