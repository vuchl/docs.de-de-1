---
ms.openlocfilehash: 2a65caedea2af65796267aa145e275ebff814bf8
ms.sourcegitcommit: 2e95559d957a1a942e490c5fd916df04b39d73a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72394464"
---
### <a name="signalr-usesignalr-and-useconnections-methods-marked-obsolete"></a><span data-ttu-id="2437d-101">SignalR: Methoden UseSignalR und UseConnections wurden als veraltet markiert.</span><span class="sxs-lookup"><span data-stu-id="2437d-101">SignalR: UseSignalR and UseConnections methods marked obsolete</span></span>

<span data-ttu-id="2437d-102">Die Methoden `UseConnections` und `UseSignalR` sowie die Klassen `ConnectionsRouteBuilder` und `HubRouteBuilder` wurden in ASP.NET Core 3.0 als veraltet markiert.</span><span class="sxs-lookup"><span data-stu-id="2437d-102">The methods `UseConnections` and `UseSignalR` and the classes `ConnectionsRouteBuilder` and `HubRouteBuilder` are marked as obsolete in ASP.NET Core 3.0.</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="2437d-103">Eingeführt in Version</span><span class="sxs-lookup"><span data-stu-id="2437d-103">Version introduced</span></span>

<span data-ttu-id="2437d-104">3.0</span><span class="sxs-lookup"><span data-stu-id="2437d-104">3.0</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="2437d-105">Altes Verhalten</span><span class="sxs-lookup"><span data-stu-id="2437d-105">Old behavior</span></span>

<span data-ttu-id="2437d-106">Das Hubrouting in SignalR wurde mithilfe von `UseSignalR` oder `UseConnections` konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="2437d-106">SignalR hub routing was configured using `UseSignalR` or `UseConnections`.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2437d-107">Neues Verhalten</span><span class="sxs-lookup"><span data-stu-id="2437d-107">New behavior</span></span>

<span data-ttu-id="2437d-108">Das alte Verfahren zum Konfigurieren des Routings wurde als veraltet markiert und durch das Endpunktrouting ersetzt.</span><span class="sxs-lookup"><span data-stu-id="2437d-108">The old way of configuring routing has been obsoleted and replaced with endpoint routing.</span></span>

#### <a name="reason-for-change"></a><span data-ttu-id="2437d-109">Grund für die Änderung</span><span class="sxs-lookup"><span data-stu-id="2437d-109">Reason for change</span></span>

<span data-ttu-id="2437d-110">Die Middleware wurden auf das neue System mit Endpunktrouting umgestellt.</span><span class="sxs-lookup"><span data-stu-id="2437d-110">Middleware is being moved to the new endpoint routing system.</span></span> <span data-ttu-id="2437d-111">Das alte Verfahren zum Hinzufügen von Middleware ist veraltet.</span><span class="sxs-lookup"><span data-stu-id="2437d-111">The old way of adding middleware is being obsoleted.</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="2437d-112">Empfohlene Maßnahme</span><span class="sxs-lookup"><span data-stu-id="2437d-112">Recommended action</span></span>

<span data-ttu-id="2437d-113">Ersetzen Sie `UseSignalR` durch `UseEndpoints`:</span><span class="sxs-lookup"><span data-stu-id="2437d-113">Replace `UseSignalR` with `UseEndpoints`:</span></span>

<span data-ttu-id="2437d-114">**Alter Code:**</span><span class="sxs-lookup"><span data-stu-id="2437d-114">**Old code:**</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<SomeHub>("/path");
});
```

<span data-ttu-id="2437d-115">**Neuer Code:**</span><span class="sxs-lookup"><span data-stu-id="2437d-115">**New code:**</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<SomeHub>("/path");
});
```

#### <a name="category"></a><span data-ttu-id="2437d-116">Kategorie</span><span class="sxs-lookup"><span data-stu-id="2437d-116">Category</span></span>

<span data-ttu-id="2437d-117">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2437d-117">ASP.NET Core</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="2437d-118">Betroffene APIs</span><span class="sxs-lookup"><span data-stu-id="2437d-118">Affected APIs</span></span>

- <xref:Microsoft.AspNetCore.Builder.ConnectionsAppBuilderExtensions.UseConnections(Microsoft.AspNetCore.Builder.IApplicationBuilder,System.Action{Microsoft.AspNetCore.Http.Connections.ConnectionsRouteBuilder})?displayProperty=fullName>
- <xref:Microsoft.AspNetCore.Builder.SignalRAppBuilderExtensions.UseSignalR(Microsoft.AspNetCore.Builder.IApplicationBuilder,System.Action{Microsoft.AspNetCore.SignalR.HubRouteBuilder})?displayProperty=fullName>
- <xref:Microsoft.AspNetCore.Http.Connections.ConnectionsRouteBuilder?displayProperty=fullName>
- <xref:Microsoft.AspNetCore.SignalR.HubRouteBuilder?displayProperty=fullName>

<!-- 

#### Affected APIs

- `M:Microsoft.AspNetCore.Builder.ConnectionsAppBuilderExtensions.UseConnections(Microsoft.AspNetCore.Builder.IApplicationBuilder,System.Action{Microsoft.AspNetCore.Http.Connections.ConnectionsRouteBuilder})`
- `M:Microsoft.AspNetCore.Builder.SignalRAppBuilderExtensions.UseSignalR(Microsoft.AspNetCore.Builder.IApplicationBuilder,System.Action{Microsoft.AspNetCore.SignalR.HubRouteBuilder})`
- `T:Microsoft.AspNetCore.Http.Connections.ConnectionsRouteBuilder`
- `T:Microsoft.AspNetCore.SignalR.HubRouteBuilder`

-->