---
title: COR_PRF_FUNCTION_ARGUMENT_INFO-Struktur
ms.date: 03/30/2017
api_name:
- COR_PRF_FUNCTION_ARGUMENT_INFO
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_FUNCTION_ARGUMENT_INFO
helpviewer_keywords:
- COR_PRF_FUNCTION_ARGUMENT_INFO structure [.NET Framework profiling]
ms.assetid: 07cf3bab-e193-4991-8205-3f41cf2d67b3
topic_type:
- apiref
ms.openlocfilehash: 2b01acbd617b13a64ef3dca6c8661f1e6bb067ac
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2019
ms.locfileid: "74447382"
---
# <a name="cor_prf_function_argument_info-structure"></a>COR_PRF_FUNCTION_ARGUMENT_INFO-Struktur
Stellt die Argumente einer Funktion dar, in Reihenfolge von links nach rechts.  
  
## <a name="syntax"></a>Syntax  
  
```cpp  
typedef struct _COR_PRF_FUNCTION_ARGUMENT_INFO {  
    ULONG numRanges;  
    ULONG totalArgumentSize;  
    COR_PRF_FUNCTION_ARGUMENT_RANGE ranges[1];  
} COR_PRF_FUNCTION_ARGUMENT_INFO;  
```  
  
## <a name="members"></a>Member  
  
|Member|Beschreibung|  
|------------|-----------------|  
|`numRanges`|Die Anzahl der Argument Blöcke. Das heißt, dieser Wert ist die Anzahl der [COR_PRF_FUNCTION_ARGUMENT_RANGE](../../../../docs/framework/unmanaged-api/profiling/cor-prf-function-argument-range-structure.md) Strukturen im `ranges` Array.|  
|`totalArgumentSize`|Die Gesamtgröße aller Argumente. Mit anderen Worten: dieser Wert ist die Summe der Argument Längen.|  
|`ranges`|Ein Array von `COR_PRF_FUNCTION_ARGUMENT_RANGE`-Strukturen, von denen jedes einen Block von Funktions Argumenten darstellt.|  
  
## <a name="remarks"></a>Hinweise  
 Eine Funktion kann viele Argumente aufweisen. Diese Argumente werden möglicherweise nicht im Arbeitsspeicher zusammenhängend gespeichert. Möglicherweise verfügen Sie über einen Block von drei Argumenten an einem Ort, einen Block mit zwei Argumenten an einer anderen Stelle und einen abschließenden Block eines Arguments an einer anderen Stelle. Diese Argumente sind alle für die gleiche Funktion. Sie werden nur an unterschiedlichen Stellen gespeichert.  
  
 Die `COR_PRF_FUNCTION_ARGUMENT_INFO`-Struktur stellt alle Argumente einer einzelnen Funktion dar. Dabei wird ein Array verwendet, um auf alle Blöcke von Funktions Argumenten zu verweisen. Für eine einzelne Funktion verfügen Sie also über eine einzelne `COR_PRF_FUNCTION_ARGUMENT_INFO` Struktur, die auf mehrere `COR_PRF_FUNCTION_ARGUMENT_RANGE` Strukturen verweist, die jeweils auf ein oder mehrere Funktionsargumente verweisen.  
  
 Argumente, die in Registern gespeichert werden, werden in den Arbeitsspeicher übertragen, um die Strukturen zu erstellen.  
  
## <a name="requirements"></a>Voraussetzungen  
 **Plattformen:** Informationen finden Sie unter [Systemanforderungen](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Header:** Corprof. idl  
  
 **Bibliothek:** CorGuids.lib  
  
 **.NET Framework-Versionen:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Siehe auch

- [Profilerstellungsstrukturen](../../../../docs/framework/unmanaged-api/profiling/profiling-structures.md)
