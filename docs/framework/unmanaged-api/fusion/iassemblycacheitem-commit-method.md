---
title: IAssemblyCacheItem::Commit-Methode
ms.date: 03/30/2017
api_name:
- IAssemblyCacheItem.Commit
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IAssemblyCacheItem::Commit
helpviewer_keywords:
- IAssemblyCacheItem::Commit method [.NET Framework fusion]
- Commit method, IAssemblyCacheItem interface [.NET Framework fusion]
ms.assetid: c2321f17-f46f-4815-ae41-b28678753613
topic_type:
- apiref
ms.openlocfilehash: 5de522c00da76e7c01369c706cb7f9e2bdad4b3b
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73134518"
---
# <a name="iassemblycacheitemcommit-method"></a>IAssemblyCacheItem::Commit-Methode
Führt einen Commit für den zwischengespeicherten Assemblyverweis zum  
  
## <a name="syntax"></a>Syntax  
  
```cpp  
HRESULT Commit (  
    [in] DWORD dwFlags,   
    [out, optional] ULONG *pulDisposition  
);  
```  
  
## <a name="parameters"></a>Parameter  
 `dwFlags`  
 in In Fusion. idl definierte Flags.  
  
 `pulDisposition`  
 [out, optional] Ein-Wert, der das Ergebnis des Vorgangs angibt.  
  
## <a name="requirements"></a>Anforderungen  
 **Plattformen:** Informationen finden Sie unter [Systemanforderungen](../../get-started/system-requirements.md).  
  
 **Header:** Fusion. h  
  
 **.NET Framework-Versionen:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Siehe auch

- [IAssemblyCacheItem-Schnittstelle](iassemblycacheitem-interface.md)
