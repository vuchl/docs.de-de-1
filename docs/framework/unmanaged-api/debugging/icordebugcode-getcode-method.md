---
title: ICorDebugCode::GetCode-Methode
ms.date: 03/30/2017
api_name:
- ICorDebugCode.GetCode
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugCode::GetCode
helpviewer_keywords:
- ICorDebugCode::GetCode method [.NET Framework debugging]
- GetCode method, ICorDebugCode interface [.NET Framework debugging]
ms.assetid: 7137e3d1-1dad-48d8-8c37-16ac816534d3
topic_type:
- apiref
ms.openlocfilehash: db24228de7e8c98fd97f890b1e408515172299b3
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73125673"
---
# <a name="icordebugcodegetcode-method"></a>ICorDebugCode::GetCode-Methode
Ruft den gesamten für die Disassembly formatierten Code für die angegebene Funktion ab. Diese Methode ist in der .NET Framework Version 2,0 veraltet. Verwenden Sie stattdessen [ICorDebugCode2:: getcodechunert](../../../../docs/framework/unmanaged-api/debugging/icordebugcode2-getcodechunks-method.md) .  
  
## <a name="syntax"></a>Syntax  
  
```cpp  
HRESULT GetCode (  
    [in] ULONG32     startOffset,   
    [in] ULONG32     endOffset,  
    [in] ULONG32     cBufferAlloc,  
    [out, size_is(cBufferAlloc),  
        length_is(*pcBufferSize)] BYTE buffer[],  
    [out] ULONG32    *pcBufferSize  
);  
```  
  
## <a name="parameters"></a>Parameter  
 `startOffset`  
 in Der Offset des Anfangs der Funktion.  
  
 `endOffset`  
 in Der Offset des Endes der Funktion.  
  
 `cBufferAlloc`  
 in Die Größe des `buffer` Arrays, in das der Code zurückgegeben wird.  
  
 `buffer`  
 vorgenommen Das Array, in das der Code zurückgegeben wird.  
  
 `pcBufferSize`  
 vorgenommen Die Anzahl der zurückgegebenen Bytes.  
  
## <a name="remarks"></a>Hinweise  
 Wenn der Code der Funktion in mehrere Blöcke aufgeteilt wurde, werden diese in der Reihenfolge der Vergrößerung des systemeigenen Offsets verkettet. Anweisungs Grenzen werden nicht geprüft.  
  
## <a name="requirements"></a>Anforderungen  
 **Plattformen:** Informationen finden Sie unter [Systemanforderungen](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Header:** CorDebug.idl, CorDebug.h  
  
 **Bibliothek:** CorGuids.lib  
  
 **.NET Framework Versionen:** 1,1, 1,0  
  
## <a name="see-also"></a>Siehe auch

- [GetCodeChunks-Methode](../../../../docs/framework/unmanaged-api/debugging/icordebugcode2-getcodechunks-method.md)
