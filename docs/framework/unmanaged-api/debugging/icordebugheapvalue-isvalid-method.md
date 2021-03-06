---
title: ICorDebugHeapValue::IsValid-Methode
ms.date: 03/30/2017
api_name:
- ICorDebugHeapValue.IsValid
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugHeapValue::IsValid
helpviewer_keywords:
- IsValid method [.NET Framework debugging]
- ICorDebugHeapValue::IsValid method [.NET Framework debugging]
ms.assetid: 68e20e62-203d-46d8-bb91-8d3c61cfacc3
topic_type:
- apiref
ms.openlocfilehash: 7edf0065fa7eb39dada167a682f2b634a438f1f3
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73138400"
---
# <a name="icordebugheapvalueisvalid-method"></a>ICorDebugHeapValue::IsValid-Methode
Ruft einen Wert ab, der angibt, ob das von diesem ICorDebugHeapValue dargestellte Objekt gültig ist.  
  
 Diese Methode ist in der .NET Framework Version 2,0 veraltet.  
  
## <a name="syntax"></a>Syntax  
  
```cpp  
HRESULT IsValid (  
    [out] BOOL    *pbValid  
);  
```  
  
## <a name="parameters"></a>Parameter  
 `pbValid`  
 vorgenommen Ein Zeiger auf einen booleschen Wert, der angibt, ob dieser Wert im Heap gültig ist.  
  
## <a name="remarks"></a>Hinweise  
 Der Wert ist ungültig, wenn er vom Garbage Collector freigegeben wurde.  
  
 Diese Methode ist veraltet. In den .NET Framework 2,0 sind alle Werte gültig, bis [icordbugcontroller:: Continue](../../../../docs/framework/unmanaged-api/debugging/icordebugcontroller-continue-method.md) aufgerufen wird. zu diesem Zeitpunkt werden die Werte für ungültig erklärt.  
  
## <a name="requirements"></a>Anforderungen  
 **Plattformen:** Informationen finden Sie unter [Systemanforderungen](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Header:** CorDebug.idl, CorDebug.h  
  
 **Bibliothek:** CorGuids.lib  
  
 **.NET Framework-Versionen:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
