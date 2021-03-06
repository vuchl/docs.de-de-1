---
title: Debuggen von globalen statischen Funktionen
ms.date: 03/30/2017
helpviewer_keywords:
- global static functions [.NET Framework debugging]
- debugging global static functions [.NET Framework]
- unmanaged global static functions [.NET Framework], debugging
ms.assetid: efc64414-77c3-48d0-881a-8594ed416aad
ms.openlocfilehash: 965724c1e937fa62f05c33b0dcf8a5c8b9e1b029
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73124314"
---
# <a name="debugging-global-static-functions"></a>Debuggen von globalen statischen Funktionen
In diesem Abschnitt werden die nicht verwalteten globalen statischen Funktionen beschrieben, die die Debug-API verwendet.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
 [_EFN_GetManagedExcepStack-Funktion](../../../../docs/framework/unmanaged-api/debugging/efn-getmanagedexcepstack-function.md)  
 Gibt bei Angabe der Adresse eines verwalteten Ausnahmeobjekts eine Zeichenfolgenversion der enthaltenen Stapelüberwachung zurück.  
  
 [_EFN_GetManagedObjectFieldInfo-Funktion](../../../../docs/framework/unmanaged-api/debugging/efn-getmanagedobjectfieldinfo-function.md)  
 Ruft den Offset vom Beginn eines Objekts zu einem Feld sowie den Wert des Felds mit dem bereitgestellten Objektzeiger und Feldnamen ab.  
  
 [_EFN_GetManagedObjectName-Funktion](../../../../docs/framework/unmanaged-api/debugging/efn-getmanagedobjectname-function.md)  
 Ruft den Namen eines Typs mit dem bereitgestellten verwalteten Objektzeiger ab.  
  
 [_EFN_StackTrace-Funktion](../../../../docs/framework/unmanaged-api/debugging/efn-stacktrace-function.md)  
 Stellt eine Textdarstellung einer verwalteten Stapelüberwachung und ein Array von `CONTEXT`-Datensätzen bereit, einen Datensatz für jeden Übergang zwischen nicht verwaltetem und verwaltetem Code.  
  
 [CLRDataCreateInstance-Funktion](../../../../docs/framework/unmanaged-api/debugging/clrdatacreateinstance-function.md)  
 Wird von den Datenzugriffsdiensten der Common Language Runtime (CLR) aufgerufen, um das angegebene Schnittstellenobjekt für den angegebenen Zielprozess zu erstellen.  
  
 [PFN_CLRDataCreateInstance-Funktionszeiger](../../../../docs/framework/unmanaged-api/debugging/pfn-clrdatacreateinstance-function-pointer.md)  
 Verweist auf eine Funktion, die von den Datenzugriffsdiensten der Common Language Runtime (CLR) aufgerufen wird, um das angegebene Schnittstellenobjekt für den angegebenen Zielprozess zu erstellen.  
  
## <a name="related-sections"></a>Verwandte Abschnitte  
 [Debuggen von Co-Klassen](../../../../docs/framework/unmanaged-api/debugging/debugging-coclasses.md)  
  
 [Debuggen von Schnittstellen](../../../../docs/framework/unmanaged-api/debugging/debugging-interfaces.md)  
  
 [Debuggen von Enumerationen](../../../../docs/framework/unmanaged-api/debugging/debugging-enumerations.md)  
  
 [Debuggen von Strukturen](../../../../docs/framework/unmanaged-api/debugging/debugging-structures.md)
