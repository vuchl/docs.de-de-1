---
ms.openlocfilehash: de40d16dbb5e7a7a49ae0988342b3eb75bc078c5
ms.sourcegitcommit: d55e14eb63588830c0ba1ea95a24ce6c57ef8c8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67804415"
---
### <a name="currentculture-and-currentuiculture-flow-across-tasks"></a>CurrentCulture und CurrentUICulture werden über mehrere Aufgaben übertragen

|   |   |
|---|---|
|Details|Ab .NET Framework 4.6 werden <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=name> und <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=name> im <xref:System.Threading.ExecutionContext?displayProperty=name>-Objekt des Threads gespeichert, das über mehrere asynchrone Vorgänge übertragen wird. Das heißt, dass Änderungen an <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=name> oder <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=name> sich auf Aufgaben auswirken, die später asynchron ausgeführt werden. Dieses Verhalten weicht von demjenigen früherer .NET Framework-Versionen ab (die <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=name> und <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=name> in allen asynchronen Aufgaben zurückgesetzt haben).|
|Vorschlag|Apps die von dieser Änderung betroffen sind, können diese möglicherweise umgehen, indem die gewünschte <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=name> oder <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=name> als erster Vorgang in einer asynchronen Aufgabe festgelegt wird. Alternativ können Sie sich für das alte Verhalten (keine Weitergabe von <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=name>/<xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=name>) entscheiden, indem Sie die folgende Kompatibilitätsoption festlegen:<pre><code class="lang-csharp">AppContext.SetSwitch(&quot;Switch.System.Globalization.NoAsyncCurrentCulture&quot;, true);&#13;&#10;</code></pre>Dieses Problem wurde von WPF in .NET Framework 4.6.2 behoben. Es wurde ebenfalls in .NET Framework 4.6 und 4.6.1 durch [KB 3139549](https://support.microsoft.com/kb/3139549) behoben. Apps mit der Zielplattform .NET Framework 4.6 oder höher erhalten automatisch das richtige Verhalten in WPF-Anwendungen (<xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=name>/<xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=name>), das über Dispatcher-Vorgänge hinweg beibehalten werden würde.|
|Bereich|Gering|
|Version|4.6|
|Typ|Neuzuweisung|
|Betroffene APIs|<ul><li><xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=nameWithType></li><li><xref:System.Threading.Thread.CurrentCulture?displayProperty=nameWithType></li><li><xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=nameWithType></li><li><xref:System.Threading.Thread.CurrentUICulture?displayProperty=nameWithType></li></ul>|

