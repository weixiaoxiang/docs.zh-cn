---
title: ICLRDebuggingLibraryProvider 接口
ms.date: 03/30/2017
api_name:
- ICLRDebuggingLibraryProvider
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICLRDebuggingLibraryProvider
helpviewer_keywords:
- ICLRDebuggingLibraryProvider interface [.NET Framework debugging]
ms.assetid: 67739617-6add-41a9-9de5-a3200c3109ce
topic_type:
- apiref
ms.openlocfilehash: 77c2011ce677d1bd2823d47740782f48151b408a
ms.sourcegitcommit: d9c7ac5d06735a01c1fafe34efe9486734841a72
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/06/2020
ms.locfileid: "82860278"
---
# <a name="iclrdebugginglibraryprovider-interface"></a>ICLRDebuggingLibraryProvider 接口
包括[ProvideLibrary 方法](iclrdebugginglibraryprovider-providelibrary-method.md)方法，该方法可获取一个库提供程序回调接口，该接口允许根据需要定位和加载特定于公共语言运行时版本的调试库。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[ProvideLibrary 方法](iclrdebugginglibraryprovider-providelibrary-method.md)|允许调试器提供可用于加载调试库的模块的句柄。|  
  
## <a name="requirements"></a>要求  
 **平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
- [调试](index.md)
