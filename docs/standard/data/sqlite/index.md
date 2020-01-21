---
title: 概述
ms.date: 12/13/2019
description: 有关 Microsoft.Data.Sqlite 的概述
ms.openlocfilehash: a5dc1366cc0ddfcd5501e26bf2a994456bcd5d98
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75353465"
---
# <a name="microsoftdatasqlite-overview"></a><span data-ttu-id="3773e-103">Microsoft.Data.Sqlite 概述</span><span class="sxs-lookup"><span data-stu-id="3773e-103">Microsoft.Data.Sqlite overview</span></span>

<span data-ttu-id="3773e-104">Microsoft.Data.Sqlite 是用于 SQLite 的轻型 [ADO.NET](../../../framework/data/adonet/index.md) 提供程序。</span><span class="sxs-lookup"><span data-stu-id="3773e-104">Microsoft.Data.Sqlite is a lightweight [ADO.NET](../../../framework/data/adonet/index.md) provider for SQLite.</span></span> <span data-ttu-id="3773e-105">用于 SQLite 的 [Entity Framework Core](/ef/core/) 提供程序就是基于此库而构建。</span><span class="sxs-lookup"><span data-stu-id="3773e-105">The [Entity Framework Core](/ef/core/) provider for SQLite is built on top of this library.</span></span> <span data-ttu-id="3773e-106">但它还可以单独使用，也可以与其他数据访问库一起使用。</span><span class="sxs-lookup"><span data-stu-id="3773e-106">However, it can also be used independently or with other data access libraries.</span></span>

## <a name="installation"></a><span data-ttu-id="3773e-107">安装</span><span class="sxs-lookup"><span data-stu-id="3773e-107">Installation</span></span>

<span data-ttu-id="3773e-108">可从 [NuGet](https://www.nuget.org/packages/Microsoft.Data.Sqlite) 获取最新的稳定版本。</span><span class="sxs-lookup"><span data-stu-id="3773e-108">The latest stable version is available on [NuGet](https://www.nuget.org/packages/Microsoft.Data.Sqlite).</span></span>

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3773e-109">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="3773e-109">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet add package Microsoft.Data.Sqlite
```

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3773e-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3773e-110">Visual Studio</span></span>](#tab/visual-studio)

``` PowerShell
Install-Package Microsoft.Data.Sqlite
```

---

## <a name="usage"></a><span data-ttu-id="3773e-111">用法</span><span class="sxs-lookup"><span data-stu-id="3773e-111">Usage</span></span>

<span data-ttu-id="3773e-112">此库实现了用于连接、命令、数据读取器等的常见 ADO.NET 抽象。</span><span class="sxs-lookup"><span data-stu-id="3773e-112">This library implements the common ADO.NET abstractions for connections, commands, data readers, and so on.</span></span>

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/HelloWorldSample/Program.cs?name=snippet_HelloWorld)]

## <a name="see-also"></a><span data-ttu-id="3773e-113">请参阅</span><span class="sxs-lookup"><span data-stu-id="3773e-113">See also</span></span>

* [<span data-ttu-id="3773e-114">连接字符串</span><span class="sxs-lookup"><span data-stu-id="3773e-114">Connection strings</span></span>](connection-strings.md)
* [<span data-ttu-id="3773e-115">API 参考</span><span class="sxs-lookup"><span data-stu-id="3773e-115">API Reference</span></span>](/dotnet/api/?view=msdata-sqlite-3.0.0)
* [<span data-ttu-id="3773e-116">SQL 语法</span><span class="sxs-lookup"><span data-stu-id="3773e-116">SQL Syntax</span></span>](https://www.sqlite.org/lang.html)