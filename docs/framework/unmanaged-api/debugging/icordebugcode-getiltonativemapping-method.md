---
title: ICorDebugCode::GetILToNativeMapping-Methode
ms.date: 03/30/2017
api_name:
- ICorDebugCode.GetILToNativeMapping
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugCode::GetILToNativeMapping
helpviewer_keywords:
- GetILToNativeMapping method, ICorDebugCode interface [.NET Framework debugging]
- ICorDebugCode::GetILToNativeMapping method [.NET Framework debugging]
ms.assetid: a8ecd8c8-9627-4356-9c6f-bd05e24637c0
topic_type:
- apiref
ms.openlocfilehash: 011da6aacbf4c40420329952f47b1fabdfc2c1a3
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73125635"
---
# <a name="icordebugcodegetiltonativemapping-method"></a>ICorDebugCode::GetILToNativeMapping-Methode
Ruft ein Array von "COR_DEBUG_IL_TO_NATIVE_MAP"-Instanzen ab, die Zuordnungen von MSIL-Offsets (Microsoft Intermediate Language) zu nativen Offsets darstellen.  
  
## <a name="syntax"></a>Syntax  
  
```cpp  
HRESULT GetILToNativeMapping (  
    [in]  ULONG32    cMap,  
    [out] ULONG32    *pcMap,  
    [out, size_is(cMap), length_is(*pcMap)]  
        COR_DEBUG_IL_TO_NATIVE_MAP map[]  
);  
```  
  
## <a name="parameters"></a>Parameter  
 `cMap`  
 [in] Die Größe des `map`-Arrays.  
  
 `pcMap`  
 vorgenommen Ein Zeiger auf die tatsächliche Anzahl der Elemente, die im `map` Array zurückgegeben werden.  
  
 `map`  
 vorgenommen Ein Array von `COR_DEBUG_IL_TO_NATIVE_MAP`-Strukturen, von denen jedes eine Zuordnung von einem MSIL-Offset zu einem systemeigenen Offset darstellt.  
  
 Es gibt keine Reihenfolge für das Array von Elementen, die zurückgegeben werden.  
  
## <a name="remarks"></a>Hinweise  
 Die `GetILToNativeMapping`-Methode gibt nur dann sinnvolle Ergebnisse zurück, wenn diese "ICorDebugCode"-Instanz systemeigenen Code darstellt, der Just-in-time (JIT) aus MSIL-Code kompiliert hat.  
  
## <a name="requirements"></a>Anforderungen  
 **Plattformen:** Informationen finden Sie unter [Systemanforderungen](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Header:** CorDebug.idl, CorDebug.h  
  
 **Bibliothek:** CorGuids.lib  
  
 **.NET Framework-Versionen:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Siehe auch

- [ICorDebugCode-Schnittstelle](../../../../docs/framework/unmanaged-api/debugging/icordebugcode-interface1.md)
