---
title: IXCLRDataMethodDefinition::EnumInstance-Methode
ms.date: 01/16/2019
api.name:
- IXCLRDataMethodDefinition::EnumInstance Method
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- IXCLRDataMethodDefinition::EnumInstance Method
helpviewer.keywords:
- IXCLRDataMethodDefinition::EnumInstance Method [.NET Framework debugging]
topic_type:
- apiref
author: cshung
ms.author: andrewau
ms.openlocfilehash: 7ad1a9957e9bffd7b28aa241723dedba1d11f4cd
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67775872"
---
# <a name="ixclrdatamethoddefinitionenuminstance-method"></a>IXCLRDataMethodDefinition::EnumInstance-Methode

Listet die Instanzen dieser Definition der Methode.

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="syntax"></a>Syntax

```cpp
HRESULT EnumInstance(
    [in, out] CLRDATA_ENUM         *handle,
    [out] IXCLRDataMethodInstance **instance
);
```

## <a name="parameters"></a>Parameter

`handle`\
[in, out] Ein Handle für das Auflisten der Instanzen.

`instance`\
[out] Die aufgelisteten Instanz.

## <a name="remarks"></a>Hinweise

Die angegebene Methode ist Teil der `IXCLRDataMethodDefinition` Schnittstelle, und mit dem vierten Steckplatz der virtuellen Methodentabelle entspricht.

## <a name="requirements"></a>Anforderungen

**Plattformen:** Weitere Informationen finden Sie unter [Systemanforderungen](../../../../docs/framework/get-started/system-requirements.md).  
**Header:** Keiner  
**Bibliothek:** Keiner  
**.NET Framework-Versionen:** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]  

## <a name="see-also"></a>Siehe auch

- [CLRDataSourceType-Enumeration](../../../../docs/framework/unmanaged-api/debugging/clrdatasourcetype-enumeration.md)
- [Debuggen](../../../../docs/framework/unmanaged-api/debugging/index.md)
- [IXCLRDataMethodDefinition-Schnittstelle](../../../../docs/framework/unmanaged-api/debugging/ixclrdatamethoddefinition-interface.md)
- [IXCLRDataMethodInstance-Schnittstelle](../../../../docs/framework/unmanaged-api/debugging/ixclrdatamethodinstance-interface.md)
