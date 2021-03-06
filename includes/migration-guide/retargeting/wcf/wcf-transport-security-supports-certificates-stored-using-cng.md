---
ms.openlocfilehash: 8c1ca89af289dbcc6c68d5ace3e8a9f32672ec0d
ms.sourcegitcommit: d55e14eb63588830c0ba1ea95a24ce6c57ef8c8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67859299"
---
### <a name="wcf-transport-security-supports-certificates-stored-using-cng"></a>Unterstützung der WCF-Transportsicherheit für Zertifikate, die mithilfe der CNG gespeichert wurden

|   |   |
|---|---|
|Details|Von Apps für die Zielplattform .NET Framework 4.6.2 an unterstützt WCF-Transportsicherheit Zertifikate, die mithilfe der Windows-Kryptographiebibliothek (CNG) gespeichert wurden. Diese Unterstützung ist auf die Verwendung von Zertifikaten mit einem öffentlichen Schlüssel beschränkt, der über einen Exponent mit einer Länge von nicht mehr als 32 Bit verfügt. Wenn eine Anwendung auf .NET Framework 4.6.2 ausgerichtet ist, ist dieses Feature standardmäßig aktiviert. In früheren Versionen von .NET Framework löst der Versuch, X509-Zertifikate mit einem CSG-Schlüsselspeicheranbieter zu verwenden, eine Ausnahme aus.|
|Vorschlag|Apps für die Zielplattform .NET Framework 4.6.1 und früher, die unter .NET Framework 4.6.2 ausgeführt werden, können Unterstützung für CNG-Zertifikate aktivieren, indem sie dem <code>&lt;runtime&gt;</code>-Abschnitt der app.config- oder der web.config-Datei die folgende Zeile hinzufügen:<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.ServiceModel.DisableCngCertificates=false&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>Dies kann mithilfe des folgenden Codes auch programmgesteuert erfolgen:<pre><code class="lang-cs">private const string DisableCngCertificates = @&quot;Switch.System.ServiceModel.DisableCngCertificate&quot;;&#13;&#10;AppContext.SetSwitch(disableCngCertificates, false);&#13;&#10;</code></pre><pre><code class="lang-vb">Const DisableCngCertificates As String = &quot;Switch.System.ServiceModel.DisableCngCertificates&quot;&#13;&#10;AppContext.SetSwitch(disableCngCertificates, False)&#13;&#10;</code></pre>Beachten Sie, dass aufgrund dieser Änderung jeglicher Code zur Ausnahmebehandlung, der von dem Versuch zur Einleitung von sicherer Kommunikation mit einem CNG-Zertifikat abhängt, nicht ausgeführt wird.|
|Bereich|Gering|
|Version|4.6.2|
|Typ|Neuzuweisung|

