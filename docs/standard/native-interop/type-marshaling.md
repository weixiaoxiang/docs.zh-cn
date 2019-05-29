---
title: 类型封送 - .NET
description: 了解 .NET 如何将类型封送到本机表示形式。
author: jkoritzinsky
ms.author: jekoritz
ms.date: 01/18/2019
ms.openlocfilehash: cb18a7607a3d99907401543b4d37995a956a3920
ms.sourcegitcommit: ca2ca60e6f5ea327f164be7ce26d9599e0f85fe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/06/2019
ms.locfileid: "65065960"
---
# <a name="type-marshaling"></a><span data-ttu-id="2bf7d-103">类型封送</span><span class="sxs-lookup"><span data-stu-id="2bf7d-103">Type marshaling</span></span>

<span data-ttu-id="2bf7d-104">**封送**是当类型需要在托管代码和本机代码之间切换时转换类型的过程。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-104">**Marshaling** is the process of transforming types when they need to cross between managed and native code.</span></span>

<span data-ttu-id="2bf7d-105">需要封送的原因在于托管代码与非托管代码中的类型并不相同。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-105">Marshaling is needed because the types in the managed and unmanaged code are different.</span></span> <span data-ttu-id="2bf7d-106">例如，在托管代码中，可指定 `String`。但在非托管环境中，字符串类型可以是 Unicode（“宽型”）、非 Unicode、null 结尾、ASCII 等。默认情况下，P/Invoke 子系统会尝试基于默认行为执行正确的操作，如本文中所述。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-106">In managed code, for instance, you have a `String`, while in the unmanaged world strings can be Unicode ("wide"), non-Unicode, null-terminated, ASCII, etc. By default, the P/Invoke subsystem tries to do the right thing based on the default behavior, described on this article.</span></span> <span data-ttu-id="2bf7d-107">但是，如果需要额外的控制，可以使用 [MarshalAs](xref:System.Runtime.InteropServices.MarshalAsAttribute) 属性指定要在非托管端上使用的预期类型。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-107">However, for those situations where you need extra control, you can employ the [MarshalAs](xref:System.Runtime.InteropServices.MarshalAsAttribute) attribute to specify what is the expected type on the unmanaged side.</span></span> <span data-ttu-id="2bf7d-108">例如，如果想要将字符串作为以 null 结尾的 ANSI 字符串发送，则可以执行如下操作：</span><span class="sxs-lookup"><span data-stu-id="2bf7d-108">For instance, if you want the string to be sent as a null-terminated ANSI string, you could do it like this:</span></span>

```csharp
[DllImport("somenativelibrary.dll")]
static extern int MethodA([MarshalAs(UnmanagedType.LPStr)] string parameter);
```

## <a name="default-rules-for-marshaling-common-types"></a><span data-ttu-id="2bf7d-109">封送通用类型的默认规则</span><span class="sxs-lookup"><span data-stu-id="2bf7d-109">Default rules for marshaling common types</span></span>

<span data-ttu-id="2bf7d-110">通常，运行时会尝试在封送时执行“正确的操作”，从而最大限地减少用户的工作。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-110">Generally, the runtime tries to do the "right thing" when marshaling to require the least amount of work from you.</span></span> <span data-ttu-id="2bf7d-111">以下表格介绍了每种类型在用于参数或字段时的默认封送方式。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-111">The following tables describe how each type is marshaled by default when used in a parameter or field.</span></span> <span data-ttu-id="2bf7d-112">C99/C++11 固定宽度的整数和字符类型用于确保下表适用于所有平台。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-112">The C99/C++11 fixed-width integer and character types are used to ensure that the following table is correct for all platforms.</span></span> <span data-ttu-id="2bf7d-113">可以使用与这些类型具有相同的对齐和大小要求的任何本机类型。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-113">You can use any native type that has the same alignment and size requirements as these types.</span></span>

<span data-ttu-id="2bf7d-114">第一个表介绍了其封送与 P/Invoke 和字段封送相同的各种类型的映射。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-114">This first table describes the mappings for various types for whom the marshaling is the same for both P/Invoke and field marshaling.</span></span>

| <span data-ttu-id="2bf7d-115">.NET 类型</span><span class="sxs-lookup"><span data-stu-id="2bf7d-115">.NET Type</span></span> | <span data-ttu-id="2bf7d-116">本机类型</span><span class="sxs-lookup"><span data-stu-id="2bf7d-116">Native Type</span></span>  |
|-----------|-------------------------|
| `byte`    | `uint8_t`               |
| `sbyte`   | `int8_t`                |
| `short`   | `int16_t`               |
| `ushort`  | `uint16_t`              |
| `int`     | `int32_t`               |
| `uint`    | `uint32_t`              |
| `long`    | `int64_t`               |
| `ulong`   | `uint64_t`              |
| `char`    | <span data-ttu-id="2bf7d-117">`char` 或 `char16_t` 依赖于 P/Invoke 或结构的 `CharSet`。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-117">Either `char` or `char16_t` depending on the `CharSet` of the P/Invoke or structure.</span></span> <span data-ttu-id="2bf7d-118">请参阅[字符集文档](charset.md)。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-118">See the [charset documentation](charset.md).</span></span> |
| `string`  | <span data-ttu-id="2bf7d-119">`char*` 或 `char16_t*` 依赖于 P/Invoke 或结构的 `CharSet`。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-119">Either `char*` or `char16_t*` depending on the `CharSet` of the P/Invoke or structure.</span></span> <span data-ttu-id="2bf7d-120">请参阅[字符集文档](charset.md)。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-120">See the [charset documentation](charset.md).</span></span> |
| `System.IntPtr` | `intptr_t`        |
| `System.UIntPtr` | `uintptr_t`      |
| <span data-ttu-id="2bf7d-121">.NET 指针类型（例如，</span><span class="sxs-lookup"><span data-stu-id="2bf7d-121">.NET Pointer types (ex.</span></span> <span data-ttu-id="2bf7d-122">`void*`)</span><span class="sxs-lookup"><span data-stu-id="2bf7d-122">`void*`)</span></span>  | `void*` |
| <span data-ttu-id="2bf7d-123">从 `System.Runtime.InteropServices.SafeHandle` 派生的类型</span><span class="sxs-lookup"><span data-stu-id="2bf7d-123">Type derived from `System.Runtime.InteropServices.SafeHandle`</span></span> | `void*` |
| <span data-ttu-id="2bf7d-124">从 `System.Runtime.InteropServices.CriticalHandle` 派生的类型</span><span class="sxs-lookup"><span data-stu-id="2bf7d-124">Type derived from `System.Runtime.InteropServices.CriticalHandle`</span></span> | `void*`          |
| `bool`    | <span data-ttu-id="2bf7d-125">Win32 `BOOL` 类型</span><span class="sxs-lookup"><span data-stu-id="2bf7d-125">Win32 `BOOL` type</span></span>       |
| `decimal` | <span data-ttu-id="2bf7d-126">COM `DECIMAL` 结构</span><span class="sxs-lookup"><span data-stu-id="2bf7d-126">COM `DECIMAL` struct</span></span> |
| <span data-ttu-id="2bf7d-127">.NET 委托</span><span class="sxs-lookup"><span data-stu-id="2bf7d-127">.NET Delegate</span></span> | <span data-ttu-id="2bf7d-128">本机函数指针</span><span class="sxs-lookup"><span data-stu-id="2bf7d-128">Native function pointer</span></span> |
| `System.DateTime` | <span data-ttu-id="2bf7d-129">Win32 `DATE` 类型</span><span class="sxs-lookup"><span data-stu-id="2bf7d-129">Win32 `DATE` type</span></span> |
| `System.Guid` | <span data-ttu-id="2bf7d-130">Win32 `GUID` 类型</span><span class="sxs-lookup"><span data-stu-id="2bf7d-130">Win32 `GUID` type</span></span> |

<span data-ttu-id="2bf7d-131">如果作为参数或结构进行封送，则部分封送类别具有不同的默认设置。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-131">A few categories of marshaling have different defaults if you're marshaling as a parameter or structure.</span></span>

| <span data-ttu-id="2bf7d-132">.NET 类型</span><span class="sxs-lookup"><span data-stu-id="2bf7d-132">.NET Type</span></span> | <span data-ttu-id="2bf7d-133">本机类型（参数）</span><span class="sxs-lookup"><span data-stu-id="2bf7d-133">Native Type (Parameter)</span></span> | <span data-ttu-id="2bf7d-134">本机类型（字段）</span><span class="sxs-lookup"><span data-stu-id="2bf7d-134">Native Type (Field)</span></span> |
|-----------|-------------------------|---------------------|
| <span data-ttu-id="2bf7d-135">.NET 数组</span><span class="sxs-lookup"><span data-stu-id="2bf7d-135">.NET array</span></span> | <span data-ttu-id="2bf7d-136">指向数组元素的本机表示形式的数组开头的指针。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-136">A pointer to the start of an array of native representations of the array elements.</span></span> | <span data-ttu-id="2bf7d-137">不允许不带 `[MarshalAs]` 属性</span><span class="sxs-lookup"><span data-stu-id="2bf7d-137">Not allowed without a `[MarshalAs]` attribute</span></span>|
| <span data-ttu-id="2bf7d-138">`LayoutKind` 为 `Sequential` 或 `Explicit` 的类</span><span class="sxs-lookup"><span data-stu-id="2bf7d-138">A class with a `LayoutKind` of `Sequential` or `Explicit`</span></span> | <span data-ttu-id="2bf7d-139">指向类的本机表示形式的指针</span><span class="sxs-lookup"><span data-stu-id="2bf7d-139">A pointer to the native representation of the class</span></span> | <span data-ttu-id="2bf7d-140">类的本机表示形式</span><span class="sxs-lookup"><span data-stu-id="2bf7d-140">The native representation of the class</span></span> |

<span data-ttu-id="2bf7d-141">下表包含仅适用于 Windows 的默认封送规则。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-141">The following table includes the default marshaling rules that are Windows-only.</span></span> <span data-ttu-id="2bf7d-142">在非 Windows 平台上，无法封送这些类型。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-142">On non-Windows platforms, you cannot marshal these types.</span></span>

| <span data-ttu-id="2bf7d-143">.NET 类型</span><span class="sxs-lookup"><span data-stu-id="2bf7d-143">.NET Type</span></span> | <span data-ttu-id="2bf7d-144">本机类型（参数）</span><span class="sxs-lookup"><span data-stu-id="2bf7d-144">Native Type (Parameter)</span></span> | <span data-ttu-id="2bf7d-145">本机类型（字段）</span><span class="sxs-lookup"><span data-stu-id="2bf7d-145">Native Type (Field)</span></span> |
|-----------|-------------------------|---------------------|
| `object`  | `VARIANT`               | `IUnknown*`         |
| `System.Array` | <span data-ttu-id="2bf7d-146">COM 接口</span><span class="sxs-lookup"><span data-stu-id="2bf7d-146">COM interface</span></span> | <span data-ttu-id="2bf7d-147">不允许不带 `[MarshalAs]` 属性</span><span class="sxs-lookup"><span data-stu-id="2bf7d-147">Not allowed without a `[MarshalAs]` attribute</span></span> |
| `System.ArgIterator` | `va_list` | <span data-ttu-id="2bf7d-148">不允许</span><span class="sxs-lookup"><span data-stu-id="2bf7d-148">Not allowed</span></span> |
| `System.Collections.IEnumerator` | `IEnumVARIANT*` | <span data-ttu-id="2bf7d-149">不允许</span><span class="sxs-lookup"><span data-stu-id="2bf7d-149">Not allowed</span></span> |
| `System.Collections.IEnumerable` | `IDispatch*` | <span data-ttu-id="2bf7d-150">不允许</span><span class="sxs-lookup"><span data-stu-id="2bf7d-150">Not allowed</span></span> |
| `System.DateTimeOffset` | <span data-ttu-id="2bf7d-151">`int64_t` 表示自 1601 年 1 月 1 日午夜以来的时钟周期数</span><span class="sxs-lookup"><span data-stu-id="2bf7d-151">`int64_t` representing the number of ticks since midnight on January 1, 1601</span></span> || <span data-ttu-id="2bf7d-152">`int64_t` 表示自 1601 年 1 月 1 日午夜以来的时钟周期数</span><span class="sxs-lookup"><span data-stu-id="2bf7d-152">`int64_t` representing the number of ticks since midnight on January 1, 1601</span></span> |

<span data-ttu-id="2bf7d-153">某些类型只能作为参数封送，而不能作为字段封送。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-153">Some types can only be marshaled as parameters and not as fields.</span></span> <span data-ttu-id="2bf7d-154">下表列出了这些类型：</span><span class="sxs-lookup"><span data-stu-id="2bf7d-154">These types are listed in the following table:</span></span>

| <span data-ttu-id="2bf7d-155">.NET 类型</span><span class="sxs-lookup"><span data-stu-id="2bf7d-155">.NET Type</span></span> | <span data-ttu-id="2bf7d-156">本机类型（仅参数）</span><span class="sxs-lookup"><span data-stu-id="2bf7d-156">Native Type (Parameter Only)</span></span> |
|-----------|------------------------------|
| `System.Text.StringBuilder` | <span data-ttu-id="2bf7d-157">`char*` 或 `char16_t*` 依赖于 P/Invoke 的 `CharSet`。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-157">Either `char*` or `char16_t*` depending on the `CharSet` of the P/Invoke.</span></span>  <span data-ttu-id="2bf7d-158">请参阅[字符集文档](charset.md)。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-158">See the [charset documentation](charset.md).</span></span> |
| `System.ArgIterator` | <span data-ttu-id="2bf7d-159">`va_list`（仅在 Windows x86/x64/arm64 上）</span><span class="sxs-lookup"><span data-stu-id="2bf7d-159">`va_list` (on Windows x86/x64/arm64 only)</span></span> |
| `System.Runtime.InteropServices.ArrayWithOffset` | `void*` |
| `System.Runtime.InteropServices.HandleRef` | `void*` |

<span data-ttu-id="2bf7d-160">如果这些默认设置不执行所需的操作，则可以自定义参数的封送方式。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-160">If these defaults don't do exactly what you want, you can customize how parameters are marshaled.</span></span> <span data-ttu-id="2bf7d-161">[参数封送](customize-parameter-marshaling.md)一文介绍自定义不同参数类型的封送方式的具体步骤。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-161">The [parameter marshaling](customize-parameter-marshaling.md) article walks you through how to customize how different parameter types are marshaled.</span></span>

## <a name="marshaling-classes-and-structs"></a><span data-ttu-id="2bf7d-162">封送类和结构</span><span class="sxs-lookup"><span data-stu-id="2bf7d-162">Marshaling classes and structs</span></span>

<span data-ttu-id="2bf7d-163">有关类型封送的另一个问题是如何将结构传入非托管方法。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-163">Another aspect of type marshaling is how to pass in a struct to an unmanaged method.</span></span> <span data-ttu-id="2bf7d-164">例如，某些非托管方法需要使用结构作为参数。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-164">For instance, some of the unmanaged methods require a struct as a parameter.</span></span> <span data-ttu-id="2bf7d-165">在这种情况下，需要在环境的托管部分中创建相应的结构或类，以便将它用作参数。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-165">In these cases, you need to create a corresponding struct or a class in managed part of the world to use it as a parameter.</span></span> <span data-ttu-id="2bf7d-166">不过，仅仅是定义类并不足够，还需要告知封送处理程序如何将类中的字段映射到非托管结构。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-166">However, just defining the class isn't enough, you also need to instruct the marshaler how to map fields in the class to the unmanaged struct.</span></span> <span data-ttu-id="2bf7d-167">在此处，`StructLayout` 属性将会很有用。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-167">Here the `StructLayout` attribute becomes useful.</span></span>

```csharp
[DllImport("kernel32.dll")]
static extern void GetSystemTime(SystemTime systemTime);

[StructLayout(LayoutKind.Sequential)]
class SystemTime {
    public ushort Year;
    public ushort Month;
    public ushort DayOfWeek;
    public ushort Day;
    public ushort Hour;
    public ushort Minute;
    public ushort Second;
    public ushort Milsecond;
}

public static void Main(string[] args) {
    SystemTime st = new SystemTime();
    GetSystemTime(st);
    Console.WriteLine(st.Year);
}
```

<span data-ttu-id="2bf7d-168">前面的代码演示了如何调用 `GetSystemTime()` 函数的简单示例。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-168">The previous code shows a simple example of calling into `GetSystemTime()` function.</span></span> <span data-ttu-id="2bf7d-169">值得注意的部分是第 4 行。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-169">The interesting bit is on line 4.</span></span> <span data-ttu-id="2bf7d-170">该属性指定应按顺序将类的字段映射到另一端（非托管端）上的结构。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-170">The attribute specifies that the fields of the class should be mapped sequentially to the struct on the other (unmanaged) side.</span></span> <span data-ttu-id="2bf7d-171">这意味着，字段的命名并不重要，唯一重要的是字段顺序，因为这种顺序需对应于非托管结构，如下面的示例所示：</span><span class="sxs-lookup"><span data-stu-id="2bf7d-171">This means that the naming of the fields isn't important, only their order is important, as it needs to correspond to the unmanaged struct, shown in the following example:</span></span>

```c
typedef struct _SYSTEMTIME {
  WORD wYear;
  WORD wMonth;
  WORD wDayOfWeek;
  WORD wDay;
  WORD wHour;
  WORD wMinute;
  WORD wSecond;
  WORD wMilliseconds;
} SYSTEMTIME, *PSYSTEMTIME*;
```

<span data-ttu-id="2bf7d-172">有时，默认结构封送不执行所需的操作。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-172">Sometimes the default marshaling for your structure doesn't do what you need.</span></span> <span data-ttu-id="2bf7d-173">[自定义结构封送](./customize-struct-marshaling.md)一文介绍如何自定义结构的封送方式。</span><span class="sxs-lookup"><span data-stu-id="2bf7d-173">The [Customizing structure marshaling](./customize-struct-marshaling.md) article teaches you how to customize how your structure is marshaled.</span></span>