---
ms.openlocfilehash: 43e9c1c2f03daedf4d56152da5672b89399a3c69
ms.sourcegitcommit: d55e14eb63588830c0ba1ea95a24ce6c57ef8c8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67804677"
---
### <a name="vbnet-no-longer-supports-partial-namespace-qualification-for-systemwindows-apis"></a>VB.NET unterstützt die Angabe partieller Namespaces für System.Windows-APIs nicht mehr

|   |   |
|---|---|
|Details|Ab .NET Framework 4.5.2 können VB.NET-Projekte keine System.Windows-APIs mit teilweise qualifizierten Namespaces angeben. Der Verweis auf <code>Windows.Forms.DialogResult</code> führt z. B. zu einem Fehler. Stattdessen muss der Code auf den vollqualifizierten Namen (<xref:System.Windows.Forms.DialogResult>) verweisen oder den bestimmten Namespace importieren und dann einfach auf <xref:System.Windows.Forms.DialogResult?displayProperty=name> verweisen.|
|Vorschlag|Der Code sollte aktualisiert werden, um auf <code>System.Windows</code>-APIs mit einfachen Namen (und den entsprechenden Namespace importieren) oder mit vollqualifizierten Namen zu verweisen.|
|Bereich|Gering|
|Version|4.5.2|
|Typ|Neuzuweisung|

