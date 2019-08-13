---
title: Arbeiten mit Daten in ASP.NET Core-Apps
description: Entwerfen moderner Webanwendungen mit ASP.NET Core und Azure | Arbeiten mit Daten in ASP.NET Core-Apps
author: ardalis
ms.author: wiwagn
ms.date: 01/30/2019
ms.openlocfilehash: 9f765acce89bec1fd73e9c43a6e7d75d78be785d
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68672817"
---
# <a name="working-with-data-in-aspnet-core-apps"></a><span data-ttu-id="6d046-103">Arbeiten mit Daten in ASP.NET Core-Apps</span><span class="sxs-lookup"><span data-stu-id="6d046-103">Working with Data in ASP.NET Core Apps</span></span>

> <span data-ttu-id="6d046-104">„Daten sind ein wertvolles Gut und werden länger erhalten bleiben als die Systeme.“</span><span class="sxs-lookup"><span data-stu-id="6d046-104">"Data is a precious thing and will last longer than the systems themselves."</span></span>
>
> <span data-ttu-id="6d046-105">Tim Berners-Lee</span><span class="sxs-lookup"><span data-stu-id="6d046-105">Tim Berners-Lee</span></span>

<span data-ttu-id="6d046-106">Der Datenzugriff stellt in beinahe allen Softwareanwendungen einen wichtigen Bestandteil dar.</span><span class="sxs-lookup"><span data-stu-id="6d046-106">Data access is an important part of almost any software application.</span></span> <span data-ttu-id="6d046-107">ASP.NET Core unterstützt verschiedene Datenzugriffsoptionen, einschließlich Entity Framework Core und Entity Framework 6, sowie alle .NET-Frameworks für den Datenzugriff.</span><span class="sxs-lookup"><span data-stu-id="6d046-107">ASP.NET Core supports a variety of data access options, including Entity Framework Core (and Entity Framework 6 as well), and can work with any .NET data access framework.</span></span> <span data-ttu-id="6d046-108">Welches Framework für den Datenzugriff verwendet werden soll, hängt von den Anforderungen der jeweiligen App ab.</span><span class="sxs-lookup"><span data-stu-id="6d046-108">The choice of which data access framework to use depends on the application's needs.</span></span> <span data-ttu-id="6d046-109">Wenn Sie die Auswahlmöglichkeiten aus ApplicationCore und Benutzeroberflächenprojekten zusammenfassen und Informationen zur Implementierung in der Infrastruktur kapseln, können sie loser gekoppelte, testbare Software erstellen.</span><span class="sxs-lookup"><span data-stu-id="6d046-109">Abstracting these choices from the ApplicationCore and UI projects, and encapsulating implementation details in Infrastructure, helps to produce loosely coupled, testable software.</span></span>

## <a name="entity-framework-core-for-relational-databases"></a><span data-ttu-id="6d046-110">Entity Framework Core (für relationale Datenbanken)</span><span class="sxs-lookup"><span data-stu-id="6d046-110">Entity Framework Core (for relational databases)</span></span>

<span data-ttu-id="6d046-111">Wenn Sie eine neue ASP.NET Core-Anwendung programmieren, die mit relationalen Datenbanken zusammenarbeiten muss, wird die Verwendung von Entity Framework Core (EF Core) empfohlen, damit die Anwendung auf ihre Daten zugreifen kann.</span><span class="sxs-lookup"><span data-stu-id="6d046-111">If you're writing a new ASP.NET Core application that needs to work with relational data, then Entity Framework Core (EF Core) is the recommended way for your application to access its data.</span></span> <span data-ttu-id="6d046-112">EF Core ist eine objektrelationale Zuordnung (Object-Relational Mapper, O/RM), die es .NET-Entwicklern ermöglicht, Objekte aus Datenquellen zu entnehmen oder diesen hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="6d046-112">EF Core is an object-relational mapper (O/RM) that enables .NET developers to persist objects to and from a data source.</span></span> <span data-ttu-id="6d046-113">In EF Core ist der Großteil des Datenzugriffscodes, den Entwickler in der Regel schreiben müssen, nicht mehr erforderlich.</span><span class="sxs-lookup"><span data-stu-id="6d046-113">It eliminates the need for most of the data access code developers would typically need to write.</span></span> <span data-ttu-id="6d046-114">Ähnlich wie ASP.NET Core wurde auch EF Core von Grund auf neu erstellt, um modulare plattformübergreifende Anwendungen zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="6d046-114">Like ASP.NET Core, EF Core has been rewritten from the ground up to support modular cross-platform applications.</span></span> <span data-ttu-id="6d046-115">Sie fügen den Dienst als NuGet-Paket zu Ihrer Anwendung hinzu, konfigurieren dieses beim Start und fordern es wenn nötig über Dependency Injection an.</span><span class="sxs-lookup"><span data-stu-id="6d046-115">You add it to your application as a NuGet package, configure it in Startup, and request it through dependency injection wherever you need it.</span></span>

<span data-ttu-id="6d046-116">Wenn Sie EF Core mit einer SQL Server-Datenbank verwenden möchten, führen Sie den folgenden Dotnet-CLI-Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="6d046-116">To use EF Core with a SQL Server database, run the following dotnet CLI command:</span></span>

<span data-ttu-id="6d046-117">dotnet add package Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="6d046-117">dotnet add package Microsoft.EntityFrameworkCore.SqlServer</span></span>

<span data-ttu-id="6d046-118">Führen Sie den folgenden Befehl zum Testen aus, wenn Sie Unterstützung für eine InMemory-Datenquelle hinzufügen möchten:</span><span class="sxs-lookup"><span data-stu-id="6d046-118">To add support for an InMemory data source, for testing:</span></span>

<span data-ttu-id="6d046-119">dotnet add package Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="6d046-119">dotnet add package Microsoft.EntityFrameworkCore.InMemory</span></span>

### <a name="the-dbcontext"></a><span data-ttu-id="6d046-120">DbContext</span><span class="sxs-lookup"><span data-stu-id="6d046-120">The DbContext</span></span>

<span data-ttu-id="6d046-121">Sie benötigen zum Arbeiten mit EF Core eine Unterklasse von <xref:Microsoft.EntityFrameworkCore.DbContext>.</span><span class="sxs-lookup"><span data-stu-id="6d046-121">To work with EF Core, you need a subclass of <xref:Microsoft.EntityFrameworkCore.DbContext>.</span></span> <span data-ttu-id="6d046-122">Diese Klasse enthält Eigenschaften, die Auflistungen der Entitäten darstellt, mit denen Ihre Anwendung arbeiten soll.</span><span class="sxs-lookup"><span data-stu-id="6d046-122">This class holds properties representing collections of the entities your application will work with.</span></span> <span data-ttu-id="6d046-123">Das eShopOnWeb-Beispiel umfasst CatalogContext mit Auflistungen für Elemente, Marken und Typen:</span><span class="sxs-lookup"><span data-stu-id="6d046-123">The eShopOnWeb sample includes a CatalogContext with collections for items, brands, and types:</span></span>

```csharp
public class CatalogContext : DbContext
{
    public CatalogContext(DbContextOptions<CatalogContext> options) : base(options)
    {

    }

    public DbSet<CatalogItem> CatalogItems { get; set; }

    public DbSet<CatalogBrand> CatalogBrands { get; set; }

    public DbSet<CatalogType> CatalogTypes { get; set; }
}
```

<span data-ttu-id="6d046-124">DbContext muss über einen Konstruktor verfügen, der DbContextOptions akzeptiert und dieses Argument an den Basiskonstruktor „DbContext“ übergibt.</span><span class="sxs-lookup"><span data-stu-id="6d046-124">Your DbContext must have a constructor that accepts DbContextOptions and pass this argument to the base DbContext constructor.</span></span> <span data-ttu-id="6d046-125">Beachten Sie, dass Sie nur eine DbContextOptions-Instanz übergeben können, wenn Sie nur über ein DbContext-Element in Ihrer Anwendung verfügen. Wenn es allerdings mehrere Elemente sind, müssen Sie den generischen DbContextOptions\<T>-Typ verwenden, der Ihren DbContext-Typ als generischen Parameter übergibt.</span><span class="sxs-lookup"><span data-stu-id="6d046-125">Note that if you have only one DbContext in your application, you can pass an instance of DbContextOptions, but if you have more than one you must use the generic DbContextOptions\<T> type, passing in your DbContext type as the generic parameter.</span></span>

### <a name="configuring-ef-core"></a><span data-ttu-id="6d046-126">Konfigurieren von EF Core</span><span class="sxs-lookup"><span data-stu-id="6d046-126">Configuring EF Core</span></span>

<span data-ttu-id="6d046-127">In Ihrer ASP.NET Core-Anwendung konfigurieren Sie in der Regel EF Core in Ihrer ConfigureServices-Methode.</span><span class="sxs-lookup"><span data-stu-id="6d046-127">In your ASP.NET Core application, you'll typically configure EF Core in your ConfigureServices method.</span></span> <span data-ttu-id="6d046-128">EF Core verwendet ein DbContextOptionsBuilder-Element, das mehrere nützliche Erweiterungsmethoden unterstützt, um seine Konfiguration zu optimieren.</span><span class="sxs-lookup"><span data-stu-id="6d046-128">EF Core uses a DbContextOptionsBuilder, which supports several helpful extension methods to streamline its configuration.</span></span> <span data-ttu-id="6d046-129">Zum Konfigurieren von CatalogContext zur Verwendung einer SQL Server-Datenbank mit einer in der Konfiguration definierten Verbindungszeichenfolge sollten Sie den folgenden ConfigureServices-Code verwenden:</span><span class="sxs-lookup"><span data-stu-id="6d046-129">To configure CatalogContext to use a SQL Server database with a connection string defined in Configuration, you would add the following code to ConfigureServices:</span></span>

```csharp
services.AddDbContext<CatalogContext>(options => options.UseSqlServer (Configuration.GetConnectionString("DefaultConnection")));
```

<span data-ttu-id="6d046-130">Zur Verwendung in der In-Memory-Datenbank:</span><span class="sxs-lookup"><span data-stu-id="6d046-130">To use the in-memory database:</span></span>

```csharp
services.AddDbContext<CatalogContext>(options =>
    options.UseInMemoryDatabase());
```

<span data-ttu-id="6d046-131">Nachdem Sie EF Core installiert, einen untergeordneten DbContext-Typ erstellt und diesen in ConfigureServices konfiguriert haben, können Sie EF Core verwenden.</span><span class="sxs-lookup"><span data-stu-id="6d046-131">Once you have installed EF Core, created a DbContext child type, and configured it in ConfigureServices, you are ready to use EF Core.</span></span> <span data-ttu-id="6d046-132">Sie können in jedem Dienst, der eine Instanz Ihres DbContext-Typs benötigt, diese anfordern und damit beginnen, mit Ihren gespeicherten Entitäten unter Verwendung von LINQ so zu arbeiten, als würden diese sich nur in einer Auflistung befinden.</span><span class="sxs-lookup"><span data-stu-id="6d046-132">You can request an instance of your DbContext type in any service that needs it, and start working with your persisted entities using LINQ as if they were simply in a collection.</span></span> <span data-ttu-id="6d046-133">EF Core übersetzt Ihre LINQ-Ausdrücke dann in SQL-Abfragen, um Ihre Daten zu speichern und abzurufen.</span><span class="sxs-lookup"><span data-stu-id="6d046-133">EF Core does the work of translating your LINQ expressions into SQL queries to store and retrieve your data.</span></span>

<span data-ttu-id="6d046-134">Sie können wie in Abbildung 8–1 dargestellt die Abfragen abrufen, die EF Core ausführt, indem Sie eine Protokollierung konfigurieren und sicherstellen, dass diese mindestens auf „Information“ festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="6d046-134">You can see the queries EF Core is executing by configuring a logger and ensuring its level is set to at least Information, as shown in Figure 8-1.</span></span>

![](./media/image8-1.png)

<span data-ttu-id="6d046-135">Abbildung 8–1: Protokollieren von EF Core-Abfragen an die Konsole</span><span class="sxs-lookup"><span data-stu-id="6d046-135">Figure 8-1 Logging EF Core queries to the console</span></span>

### <a name="fetching-and-storing-data"></a><span data-ttu-id="6d046-136">Abrufen und Speichern von Daten</span><span class="sxs-lookup"><span data-stu-id="6d046-136">Fetching and storing Data</span></span>

<span data-ttu-id="6d046-137">Greifen Sie zum Abrufen von Daten aus EF Core auf eine passende Eigenschaft zu, und verwenden Sie LINQ, um das Ergebnis zu filtern.</span><span class="sxs-lookup"><span data-stu-id="6d046-137">To retrieve data from EF Core, you access the appropriate property and use LINQ to filter the result.</span></span> <span data-ttu-id="6d046-138">Außerdem können Sie LINQ verwenden, um eine Projektion durchzuführen, indem Sie das Ergebnis von einem Typ in einen anderen umwandeln.</span><span class="sxs-lookup"><span data-stu-id="6d046-138">You can also use LINQ to perform projection, transforming the result from one type to another.</span></span> <span data-ttu-id="6d046-139">Über das folgende Beispiel werden CatalogBrands-Elemente abgerufen, die nach Namen sortiert und anhand der Eigenschaft „Enabled“ (Aktiviert) sortiert sind und auf einen SelectListItem-Typ projiziert werden:</span><span class="sxs-lookup"><span data-stu-id="6d046-139">The following example would retrieve CatalogBrands, ordered by name, filtered by their Enabled property, and projected onto a SelectListItem type:</span></span>

```csharp
var brandItems = await _context.CatalogBrands
    .Where(b => b.Enabled)
    .OrderBy(b => b.Name)
    .Select(b => new SelectListItem {
        Value = b.Id, Text = b.Name })
    .ToListAsync();
```

<span data-ttu-id="6d046-140">Im obenstehenden Beispiel ist es wichtig, dass der Aufruf zu ToListAsync hinzugefügt wird, um die Abfrage ohne Umschweife auszuführen.</span><span class="sxs-lookup"><span data-stu-id="6d046-140">It's important in the above example to add the call to ToListAsync in order to execute the query immediately.</span></span> <span data-ttu-id="6d046-141">Andernfalls weist die Anweisung ein IQueryable\<SelectListItem-Element brandItems-Elementen zu, die erst ausgeführt werden, nachdem dieses aufgelistet wurde.</span><span class="sxs-lookup"><span data-stu-id="6d046-141">Otherwise, the statement will assign an IQueryable\<SelectListItem> to brandItems, which will not be executed until it is enumerated.</span></span> <span data-ttu-id="6d046-142">Das Zurückgeben von IQueryable-Ergebnissen aus Methoden hat sowohl Vorteile als auch Nachteile.</span><span class="sxs-lookup"><span data-stu-id="6d046-142">There are pros and cons to returning IQueryable results from methods.</span></span> <span data-ttu-id="6d046-143">Auf der einen Seite kann die Abfrage, die EF Core erstellt, dadurch weiter verändert werden. Auf der anderen Seite können dadurch auch Fehler entstehen, die nur zur Laufzeit entstehen, wenn zu der Abfrage Vorgänge hinzugefügt werden, die EF Core nicht übersetzen kann.</span><span class="sxs-lookup"><span data-stu-id="6d046-143">It allows the query EF Core will construct to be further modified, but can also result in errors that only occur at runtime, if operations are added to the query that EF Core cannot translate.</span></span> <span data-ttu-id="6d046-144">In der Regel ist es sicherer, Filter an die Methode zu übergeben, die den Datenzugriff durchführt, und eine In-Memory-Auflistung (z.B. List\<T>) als Ergebnis zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="6d046-144">It's generally safer to pass any filters into the method performing the data access, and return back an in-memory collection (for example, List\<T>) as the result.</span></span>

<span data-ttu-id="6d046-145">EF Core verfolgt Änderungen an Entitäten nach, die aus dem Persistenzspeicher abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="6d046-145">EF Core tracks changes on entities it fetches from persistence.</span></span> <span data-ttu-id="6d046-146">Wenn Sie Änderungen an einer nachverfolgten Entität speichern möchten, rufen Sie einfach die SaveChanges-Methode für das DbContext-Element ab, und stellen Sie dabei sicher, dass es sich um die DbContext-Instanz handelt, die auch verwendet wurde, um die Entität abzurufen.</span><span class="sxs-lookup"><span data-stu-id="6d046-146">To save changes to a tracked entity, you just call the SaveChanges method on the DbContext, making sure it's the same DbContext instance that was used to fetch the entity.</span></span> <span data-ttu-id="6d046-147">Das Hinzufügen und Entfernen von Entitäten geschieht direkt über die entsprechende DbSet-Eigenschaft, während gleichzeitig SaveChanges aufgerufen wird, um die Datenbankbefehle auszuführen.</span><span class="sxs-lookup"><span data-stu-id="6d046-147">Adding and removing entities is directly done on the appropriate DbSet property, again with a call to SaveChanges to execute the database commands.</span></span> <span data-ttu-id="6d046-148">Im folgenden Beispiel wird dargestellt, wie Sie Entitäten zum Persistenzspeicher hinzufügen, diese darin aktualisieren und aus ihm entfernen.</span><span class="sxs-lookup"><span data-stu-id="6d046-148">The following example demonstrates adding, updating, and removing entities from persistence.</span></span>

```csharp
// create
var newBrand = new CatalogBrand() { Brand = "Acme" };
_context.Add(newBrand);
await _context.SaveChangesAsync();

// read and update
var existingBrand = _context.CatalogBrands.Find(1);
existingBrand.Brand = "Updated Brand";
await _context.SaveChangesAsync();

// read and delete (alternate Find syntax)
var brandToDelete = _context.Find<CatalogBrand>(2);
_context.CatalogBrands.Remove(brandToDelete);
await _context.SaveChangesAsync();
```

<span data-ttu-id="6d046-149">EF Core unterstützt sowohl synchrone als auch asynchrone Methoden zum Abrufen und Speichern.</span><span class="sxs-lookup"><span data-stu-id="6d046-149">EF Core supports both synchronous and async methods for fetching and saving.</span></span> <span data-ttu-id="6d046-150">In Webanwendungen wird empfohlen, das Async/Await-Muster mit der Async-Methode zu verwenden, damit keine Webserverthreads blockiert werden, während darauf gewartet wird, dass Datenzugriffsvorgänge abgeschlossen werden.</span><span class="sxs-lookup"><span data-stu-id="6d046-150">In web applications, it's recommended to use the async/await pattern with the async methods, so that web server threads are not blocked while waiting for data access operations to complete.</span></span>

### <a name="fetching-related-data"></a><span data-ttu-id="6d046-151">Abrufen zugehöriger Daten</span><span class="sxs-lookup"><span data-stu-id="6d046-151">Fetching related data</span></span>

<span data-ttu-id="6d046-152">Wenn EF Core Entitäten abruft, werden alle Eigenschaften aufgefüllt, die direkt mit dieser Entität in der Datenbank gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="6d046-152">When EF Core retrieves entities, it populates all of the properties that are stored directly with that entity in the database.</span></span> <span data-ttu-id="6d046-153">Navigationseigenschaften wie Listen mit verknüpften Entitäten werden nicht aufgefüllt, und ihr Wert ist möglicherweise auf NULL festgelegt.</span><span class="sxs-lookup"><span data-stu-id="6d046-153">Navigation properties, such as lists of related entities, are not populated and may have their value set to null.</span></span> <span data-ttu-id="6d046-154">Dadurch wird sichergestellt, dass EF Core nicht mehr Daten abruft als nötig. Dies ist besonders wichtig für Webanwendungen, die schnell Anforderungen verarbeiten und auf effiziente Weise Antworten zurückgeben müssen.</span><span class="sxs-lookup"><span data-stu-id="6d046-154">This ensures EF Core is not fetching more data than is needed, which is especially important for web applications, which must quickly process requests and return responses in an efficient manner.</span></span> <span data-ttu-id="6d046-155">Geben Sie wie im Folgenden dargestellt unter Verwendung der Erweiterungsmethode „Include“ bei der Abfrage die Eigenschaft an, um über _Eager Loading_ Beziehungen zu einer Entität hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="6d046-155">To include relationships with an entity using _eager loading_, you specify the property using the Include extension method on the query, as shown:</span></span>

```csharp
// .Include requires using Microsoft.EntityFrameworkCore
var brandsWithItems = await _context.CatalogBrands
    .Include(b => b.Items)
    .ToListAsync();
```

<span data-ttu-id="6d046-156">Sie können unter Verwendung von ThenInclude mehrere Beziehungen sowie untergeordnete Beziehungen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="6d046-156">You can include multiple relationships, and you can also include sub-relationships using ThenInclude.</span></span> <span data-ttu-id="6d046-157">EF Core führt dann eine einzelne Abfrage aus, um die daraus entstehenden Entitäten abzurufen.</span><span class="sxs-lookup"><span data-stu-id="6d046-157">EF Core will execute a single query to retrieve the resulting set of entities.</span></span> <span data-ttu-id="6d046-158">Alternativ können Sie die Navigationseigenschaften von Navigationseigenschaften einfügen, indem Sie eine durch Trennzeichen (.) getrennte Zeichenfolge wie im Folgenden dargestellt an die Erweiterungsmethode `.Include()` übergeben:</span><span class="sxs-lookup"><span data-stu-id="6d046-158">Alternately you can include navigation properties of navigation properties by passing a '.'-separated string to the `.Include()` extension method, like so:</span></span>

```csharp
    .Include(“Items.Products”)
```

<span data-ttu-id="6d046-159">Zusätzlich zum Kapseln der Filterlogik kann eine Spezifikation die Form der zurückzugebenden Daten angeben, einschließlich der aufzufüllenden Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="6d046-159">In addition to encapsulating filtering logic, a specification can specify the shape of the data to be returned, including which properties to populate.</span></span> <span data-ttu-id="6d046-160">Das Beispiel „eShopOnWeb“ enthält mehrere Spezifikationen, die das Kapseln von Eager Loading-Informationen in der Spezifikation veranschaulichen.</span><span class="sxs-lookup"><span data-stu-id="6d046-160">The eShopOnWeb sample includes several specifications that demonstrate encapsulating eager loading information within the specification.</span></span> <span data-ttu-id="6d046-161">Hier sehen Sie, wie die Spezifikation als Teil einer Abfrage verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="6d046-161">You can see how the specification is used as part of a query here:</span></span>

```csharp
// Includes all expression-based includes
query = specification.Includes.Aggregate(query,
            (current, include) => current.Include(include));

// Include any string-based include statements
query = specification.IncludeStrings.Aggregate(query,
            (current, include) => current.Include(include));
```

<span data-ttu-id="6d046-162">Sie können auch das _explizite Laden_ verwenden, um verknüpfte Daten zu laden.</span><span class="sxs-lookup"><span data-stu-id="6d046-162">Another option for loading related data is to use _explicit loading_.</span></span> <span data-ttu-id="6d046-163">Beim expliziten Laden können Sie zusätzliche Daten in eine Entität laden, die bereits abgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="6d046-163">Explicit loading allows you to load additional data into an entity that has already been retrieved.</span></span> <span data-ttu-id="6d046-164">Da dies eine separate Anforderung an die Datenbank umfasst, wird dies nicht für Webanwendungen empfohlen, die die Anzahl von Datenbankroundtrips pro Anforderung reduzieren sollen.</span><span class="sxs-lookup"><span data-stu-id="6d046-164">Since this involves a separate request to the database, it's not recommended for web applications, which should minimize the number of database round trips made per request.</span></span>

<span data-ttu-id="6d046-165">Beim _verzögerten Laden_ handelt es sich um ein Feature, das automatisch verknüpfte Daten lädt, wenn die Anwendung auf dieses verweist.</span><span class="sxs-lookup"><span data-stu-id="6d046-165">_Lazy loading_ is a feature that automatically loads related data as it is referenced by the application.</span></span> <span data-ttu-id="6d046-166">EF Core Version 2.1 unterstützt verzögertes Laden.</span><span class="sxs-lookup"><span data-stu-id="6d046-166">EF Core has added support for lazy loading in version 2.1.</span></span> <span data-ttu-id="6d046-167">Verzögertes Laden ist standardmäßig deaktiviert und erfordert die Installation von `Microsoft.EntityFrameworkCore.Proxies`.</span><span class="sxs-lookup"><span data-stu-id="6d046-167">Lazy loading is not enabled by default and requires installing the `Microsoft.EntityFrameworkCore.Proxies`.</span></span> <span data-ttu-id="6d046-168">Ähnlich wie das explizite Laden sollte das verzögerte Laden in der Regel für Webanwendungen deaktiviert sein, da dessen Verwendung zu zusätzlichen Datenbankabfragen in jeder Webanforderung führt.</span><span class="sxs-lookup"><span data-stu-id="6d046-168">As with explicit loading, lazy loading should typically be disabled for web applications, since its use will result in additional database queries being made within each web request.</span></span> <span data-ttu-id="6d046-169">Der vom verzögerten Laden verursachte zeitliche Mehraufwand wird zur Entwicklungszeit oft nicht beachtet, wenn die Wartezeit kurz ist und die für die Tests verwendeten Datasets klein sind.</span><span class="sxs-lookup"><span data-stu-id="6d046-169">Unfortunately, the overhead incurred by lazy loading often goes unnoticed at development time, when latency is small and often the data sets used for testing are small.</span></span> <span data-ttu-id="6d046-170">Allerdings können die zusätzlichen Datenbankanforderungen oft zu schlechter Leistung bei Webanwendungen führen, die intensiven Gebrauch vom verzögerten Laden machen, da in der Produktion mehr Benutzer sowie Daten vorhanden sind und höhere Wartezeiten auftreten.</span><span class="sxs-lookup"><span data-stu-id="6d046-170">However, in production, with more users, more data, and more latency, the additional database requests can often result in poor performance for web applications that make heavy use of lazy loading.</span></span>

[<span data-ttu-id="6d046-171">Avoid Lazy Loading Entities in Web Applications (Vermeiden von verzögertem Laden von Entitäten in Webanwendungen)</span><span class="sxs-lookup"><span data-stu-id="6d046-171">Avoid Lazy Loading Entities in Web Applications</span></span>](https://ardalis.com/avoid-lazy-loading-entities-in-asp-net-applications)

### <a name="encapsulating-data"></a><span data-ttu-id="6d046-172">Kapseln von Daten</span><span class="sxs-lookup"><span data-stu-id="6d046-172">Encapsulating data</span></span>

<span data-ttu-id="6d046-173">EF Core enthält mehrere Features, mit denen Sie den Zustand des Modells richtig kapseln können.</span><span class="sxs-lookup"><span data-stu-id="6d046-173">EF Core supports several features that allow your model to properly encapsulate its state.</span></span> <span data-ttu-id="6d046-174">Ein häufiges Problem im Zusammenhang mit Domänenmodellen ist, dass sie Navigationseigenschaften für Sammlungen als öffentlich zugängliche Listentypen zur Verfügung stellen.</span><span class="sxs-lookup"><span data-stu-id="6d046-174">A common problem in domain models is that they expose collection navigation properties as publicly accessible list types.</span></span> <span data-ttu-id="6d046-175">Dadurch können alle Mitarbeiter die Inhalte dieser Auflistungstypen ändern. Dabei können möglicherweise wichtige Geschäftsregeln umgangen werden, die im Zusammenhang mit der Sammlung stehen, wodurch das Objekt in einem ungültigen Zustand hinterlassen wird.</span><span class="sxs-lookup"><span data-stu-id="6d046-175">This allows any collaborator to manipulate the contents of these collection types, which may bypass important business rules related to the collection, possibly leaving the object in an invalid state.</span></span> <span data-ttu-id="6d046-176">Zur Lösung dieses Problems können Sie lediglich schreibgeschützten Zugriff auf verwandte Sammlungen auf gewähren und explizit Methoden zur Verfügung stellen, über die Clients Änderungen vornehmen können. Dieser Vorgang wird im folgenden Beispiel veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="6d046-176">The solution to this is to expose read-only access to related collections, and explicitly provide methods defining ways in which clients can manipulate them, as in this example:</span></span>

```csharp
public class Basket : BaseEntity
{
    public string BuyerId { get; set; }
    private readonly List<BasketItem> _items = new List<BasketItem>();
    public IReadOnlyCollection<BasketItem> Items => _items.AsReadOnly();

    public void AddItem(int catalogItemId, decimal unitPrice, int quantity = 1)
    {
        if (!Items.Any(i => i.CatalogItemId == catalogItemId))
        {
            _items.Add(new BasketItem()
            {
                CatalogItemId = catalogItemId,
                Quantity = quantity,
                UnitPrice = unitPrice
            });
            return;
        }
        var existingItem = Items.FirstOrDefault(i => i.CatalogItemId == catalogItemId);
        existingItem.Quantity += quantity;
    }
}
```

<span data-ttu-id="6d046-177">Beachten Sie, dass bei diesem Entitätstyp keine öffentliche `List`- oder `ICollection`-Eigenschaft sondern ein `IReadOnlyCollection`-Typ verfügbar gemacht wird, der den zugrunde liegenden Listentyp einschließt.</span><span class="sxs-lookup"><span data-stu-id="6d046-177">Note that this entity type doesn’t expose a public `List` or `ICollection` property, but instead exposes an `IReadOnlyCollection` type that wraps the underlying List type.</span></span> <span data-ttu-id="6d046-178">Wenn Sie dieses Muster nutzen, können Sie festlegen, dass Entity Framework Core das Unterstützungsfeld folgendermaßen verwendet:</span><span class="sxs-lookup"><span data-stu-id="6d046-178">When using this pattern, you can indicate to Entity Framework Core to use the backing field like so:</span></span>

```csharp
private void ConfigureBasket(EntityTypeBuilder<Basket> builder)
{
    var navigation = builder.Metadata.FindNavigation(nameof(Basket.Items));

    navigation.SetPropertyAccessMode(PropertyAccessMode.Field);
}
```

<span data-ttu-id="6d046-179">Sie können Ihr Domänenmodell auch verbessern, indem Sie die Wertobjekte für Typen verwenden, die keine Identitäten besitzen oder sich nur durch ihre Eigenschaften unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="6d046-179">Another way in which you can improve your domain model is through the use of value objects for types that lack identity and are only distinguished by their properties.</span></span> <span data-ttu-id="6d046-180">Durch das Verwenden solcher Typen als Eigenschaften Ihrer Entitäten können Sie sicherstellen, dass die Logik für das entsprechende Wertobjekt spezifisch ist. So kann duplizierte Logik in mehreren Entitäten, die das gleiche Konzept verwenden, vermieden werden.</span><span class="sxs-lookup"><span data-stu-id="6d046-180">Using such types as properties of your entities can help keep logic specific to the value object where it belongs, and can avoid duplicate logic between multiple entities that use the same concept.</span></span> <span data-ttu-id="6d046-181">In Entity Framework Core können Sie Wertobjekte in der gleichen Tabelle wie die besitzende Entität speichern, indem Sie den Typ folgendermaßen als nicht eigenständige Entität konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="6d046-181">In Entity Framework Core, you can persist value objects in the same table as their owning entity by configuring the type as an owned entity, like so:</span></span>

```csharp
private void ConfigureOrder(EntityTypeBuilder<Order> builder)
{
    builder.OwnsOne(o => o.ShipToAddress);
}
```

<span data-ttu-id="6d046-182">In diesem Beispiel weist die `ShipToAddress`-Eigenschaft den Typ `Address` auf.</span><span class="sxs-lookup"><span data-stu-id="6d046-182">In this example, the `ShipToAddress` property is of type `Address`.</span></span> <span data-ttu-id="6d046-183">`Address` ist ein Wertobjekt mit mehreren Eigenschaften wie `Street` und `City`.</span><span class="sxs-lookup"><span data-stu-id="6d046-183">`Address` is a value object with several properties such as `Street` and `City`.</span></span> <span data-ttu-id="6d046-184">EF Core ordnet das `Order`-Objekt zu seiner Tabelle zu, die eine Spalte pro `Address`-Eigenschaft enthält. Dabei wird jedem Spaltennamen der Name der Eigenschaft vorangestellt.</span><span class="sxs-lookup"><span data-stu-id="6d046-184">EF Core maps the `Order` object to its table with one column per `Address` property, prefixing each column name with the name of the property.</span></span> <span data-ttu-id="6d046-185">In diesem Beispiel würde die Tabelle `Order` Spalten wie `ShipToAddress_Street` und `ShipToAddress_City` enthalten.</span><span class="sxs-lookup"><span data-stu-id="6d046-185">In this example, the `Order` table would include columns such as `ShipToAddress_Street` and `ShipToAddress_City`.</span></span>

[<span data-ttu-id="6d046-186">Ab EF Core 2.2 werden Sammlungen von nicht eigenständigen Entitäten unterstützt</span><span class="sxs-lookup"><span data-stu-id="6d046-186">EF Core 2.2 introduces support for collections of owned entities</span></span>](https://docs.microsoft.com/ef/core/what-is-new/ef-core-2.2#collections-of-owned-entities)

### <a name="resilient-connections"></a><span data-ttu-id="6d046-187">Robuste Verbindungen</span><span class="sxs-lookup"><span data-stu-id="6d046-187">Resilient connections</span></span>

<span data-ttu-id="6d046-188">Es kann sein, dass externe Ressourcen wie SQL-Datenbanken zeitweise nicht verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="6d046-188">External resources like SQL databases may occasionally be unavailable.</span></span> <span data-ttu-id="6d046-189">Wenn es zu einem temporären Ausfall kommen sollte, können Anwendungen die Wiederholungslogik verwenden, damit keine Ausnahmen ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="6d046-189">In cases of temporary unavailability, applications can use retry logic to avoid raising an exception.</span></span> <span data-ttu-id="6d046-190">Diese Technik wird als _Verbindungsresilienz_ bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6d046-190">This technique is commonly referred to as _connection resiliency_.</span></span> <span data-ttu-id="6d046-191">Sie können Ihre eigene Technik für [Wiederholungen mit exponentiellem Backoff verwenden](https://docs.microsoft.com/azure/architecture/patterns/retry), indem Sie so viele Wiederholungen durchführen, bis die maximale Anzahl von möglichen Wiederholungen erreicht ist, und dabei die Wartezeit zwischen den einzelnen Wiederholungen immer weiter ausdehnen.</span><span class="sxs-lookup"><span data-stu-id="6d046-191">You can implement your [own retry with exponential backoff](https://docs.microsoft.com/azure/architecture/patterns/retry) technique by attempting to retry with an exponentially increasing wait time, until a maximum retry count has been reached.</span></span> <span data-ttu-id="6d046-192">Diese Technik berücksichtigt den Umstand, dass Cloudressourcen zeitweise nicht verfügbar sein können, wodurch einige Anforderungen fehlschlagen.</span><span class="sxs-lookup"><span data-stu-id="6d046-192">This technique embraces the fact that cloud resources might intermittently be unavailable for short periods of time, resulting in failure of some requests.</span></span>

<span data-ttu-id="6d046-193">Entity Framework Core bietet bereits interne Datenbankverbindungsresilienz und Wiederholungslogik für Azure SQL DB.</span><span class="sxs-lookup"><span data-stu-id="6d046-193">For Azure SQL DB, Entity Framework Core already provides internal database connection resiliency and retry logic.</span></span> <span data-ttu-id="6d046-194">Jedoch müssen Sie die Entity Framework-Ausführungsstrategie für jede DbContext-Verbindung aktivieren, wenn Sie robuste EF Core-Verbindungen erzielen wollen.</span><span class="sxs-lookup"><span data-stu-id="6d046-194">But you need to enable the Entity Framework execution strategy for each DbContext connection if you want to have resilient EF Core connections.</span></span>

<span data-ttu-id="6d046-195">Zum Beispiel aktiviert der folgende Code auf der EF Core-Verbindungsebene robuste SQL-Verbindungen, die wiederholt werden, wenn die Verbindung fehlschlägt.</span><span class="sxs-lookup"><span data-stu-id="6d046-195">For instance, the following code at the EF Core connection level enables resilient SQL connections that are retried if the connection fails.</span></span>

```csharp
// Startup.cs from any ASP.NET Core Web API
public class Startup
{
    public IServiceProvider ConfigureServices(IServiceCollection services)
    {
        //...
        services.AddDbContext<OrderingContext>(options =>
        {
            options.UseSqlServer(Configuration["ConnectionString"],
            sqlServerOptionsAction: sqlOptions =>
        {
            sqlOptions.EnableRetryOnFailure(
            maxRetryCount: 5,
            maxRetryDelay: TimeSpan.FromSeconds(30),
            errorNumbersToAdd: null);
        });
    });
}
//...
```

#### <a name="execution-strategies-and-explicit-transactions-using-begintransaction-and-multiple-dbcontexts"></a><span data-ttu-id="6d046-196">Ausführungsstrategien und explizite Transaktionen mit „BeginTransaction“ und mehreren DbContext-Objekten</span><span class="sxs-lookup"><span data-stu-id="6d046-196">Execution strategies and explicit transactions using BeginTransaction and multiple DbContexts</span></span>

<span data-ttu-id="6d046-197">Wenn Wiederholungen in EF Core-Verbindungen aktiviert sind, wird jeder Vorgang, den Sie mit EF Core durchführen, zu einem individuell wiederholbaren Vorgang.</span><span class="sxs-lookup"><span data-stu-id="6d046-197">When retries are enabled in EF Core connections, each operation you perform using EF Core becomes its own retriable operation.</span></span> <span data-ttu-id="6d046-198">Jede Abfrage und jeder Aufruf von „SaveChanges“ wird als eine Einheit wiederholt, wenn ein vorübergehender Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="6d046-198">Each query and each call to SaveChanges will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="6d046-199">Wenn Ihr Code jedoch eine Transaktion mit „BeginTransaction“ ausführt, definieren Sie Ihre eigene Gruppe von Vorgängen, die als Einheit behandelt werden müssen. Alles innerhalb dieser Transaktion wird zurückgesetzt, wenn ein Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="6d046-199">However, if your code initiates a transaction using BeginTransaction, you are defining your own group of operations that need to be treated as a unit; everything inside the transaction has to be rolled back if a failure occurs.</span></span> <span data-ttu-id="6d046-200">Eine Ausnahme wie die Folgende wird angezeigt, wenn Sie versuchen, diese Transaktion auszuführen, wenn Sie die EF-Ausführungsstrategie verwenden (Wiederholungsrichtlinie) und der Transaktion mehrere SaveChanges-Elemente von mehreren DbContext-Objekten hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="6d046-200">You will see an exception like the following if you attempt to execute that transaction when using an EF execution strategy (retry policy) and you include several SaveChanges from multiple DbContexts in it.</span></span>

<span data-ttu-id="6d046-201">System.InvalidOperationException: Die konfigurierte Ausführungsstrategie SqlServerRetryingExecutionStrategy unterstützt keine vom Benutzer initiierten Transaktionen.</span><span class="sxs-lookup"><span data-stu-id="6d046-201">System.InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="6d046-202">Verwenden Sie die Ausführungsstrategie, die von „DbContext.Database.CreateExecutionStrategy()“ zurückgegeben wird, um alle Vorgänge in der Transaktion als wiederholbare Einheit auszuführen.</span><span class="sxs-lookup"><span data-stu-id="6d046-202">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="6d046-203">Die Lösung ist, die EF-Ausführungsstrategie mit einem Delegaten manuell aufzurufen, der alle Komponenten darstellt, die ausgeführt werden müssen.</span><span class="sxs-lookup"><span data-stu-id="6d046-203">The solution is to manually invoke the EF execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="6d046-204">Die Ausführungsstrategie ruft den Delegaten erneut auf, wenn ein vorübergehender Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="6d046-204">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span> <span data-ttu-id="6d046-205">Im folgenden Codebeispiel wird die Implementierung dieses Ansatzes veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="6d046-205">The following code shows how to implement this approach:</span></span>

```csharp
// Use of an EF Core resiliency strategy when using multiple DbContexts
// within an explicit transaction
// See:
// https://docs.microsoft.com/ef/core/miscellaneous/connection-resiliency
var strategy = _catalogContext.Database.CreateExecutionStrategy();
await strategy.ExecuteAsync(async () =>
{
    // Achieving atomicity between original Catalog database operation and the
    // IntegrationEventLog thanks to a local transaction
    using (var transaction = _catalogContext.Database.BeginTransaction())
    {
        _catalogContext.CatalogItems.Update(catalogItem);
        await _catalogContext.SaveChangesAsync();

        // Save to EventLog only if product price changed
        if (raiseProductPriceChangedEvent)
        await _integrationEventLogService.SaveEventAsync(priceChangedEvent);
        transaction.Commit();
    }
});
```

<span data-ttu-id="6d046-206">Das erste DbContext-Objekt ist \_catalogContext und das zweite befindet sich im Objekt \_integrationEventLogService.</span><span class="sxs-lookup"><span data-stu-id="6d046-206">The first DbContext is the \_catalogContext and the second DbContext is within the \_integrationEventLogService object.</span></span> <span data-ttu-id="6d046-207">Zum Schluss wird die Commitaktion unter Verwendung einer EF-Ausführungsstrategie für mehrere DbContext-Objekte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="6d046-207">Finally, the Commit action would be performed multiple DbContexts and using an EF Execution Strategy.</span></span>

> ### <a name="references--entity-framework-core"></a><span data-ttu-id="6d046-208">Ressourcen: Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="6d046-208">References – Entity Framework Core</span></span>
>
> - <span data-ttu-id="6d046-209">**EF Core-Dokumentation**</span><span class="sxs-lookup"><span data-stu-id="6d046-209">**EF Core Docs**</span></span>  
>   <https://docs.microsoft.com/ef/>
> - <span data-ttu-id="6d046-210">**EF Core: Laden zugehöriger Daten**</span><span class="sxs-lookup"><span data-stu-id="6d046-210">**EF Core: Related Data**</span></span>  
>   <https://docs.microsoft.com/ef/core/querying/related-data>
> - <span data-ttu-id="6d046-211">**Avoid Lazy Loading Entities in ASPNET Applications (Vermeiden von verzögertem Laden von Entitäten in ASP.NET-Anwendungen)**</span><span class="sxs-lookup"><span data-stu-id="6d046-211">**Avoid Lazy Loading Entities in ASPNET Applications**</span></span>  
>   <https://ardalis.com/avoid-lazy-loading-entities-in-asp-net-applications>

## <a name="ef-core-or-micro-orm"></a><span data-ttu-id="6d046-212">EF Core oder Mikro-ORM?</span><span class="sxs-lookup"><span data-stu-id="6d046-212">EF Core or micro-ORM?</span></span>

<span data-ttu-id="6d046-213">EF Core eignet sich gut zum Verwalten der Persistenz und kapselt vor allem die von Anwendungsentwicklern bereitgestellten Datenbankinformationen. Es gibt hierfür jedoch auch andere Möglichkeiten.</span><span class="sxs-lookup"><span data-stu-id="6d046-213">While EF Core is a great choice for managing persistence, and for the most part encapsulates database details from application developers, it is not the only choice.</span></span> <span data-ttu-id="6d046-214">[Dapper](https://github.com/StackExchange/Dapper) ist z.B. eine Open-Source-Alternative, die häufig verwendet wird. Dabei handelt es sich um eine Mikro-ORM.</span><span class="sxs-lookup"><span data-stu-id="6d046-214">Another popular open source alternative is [Dapper](https://github.com/StackExchange/Dapper), a so-called micro-ORM.</span></span> <span data-ttu-id="6d046-215">Bei einer Mikro-ORM handelt es sich um ein Tool mit allen Features, die zum Zuordnen von Objekten zu Datenstrukturen benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="6d046-215">A micro-ORM is a lightweight, less full-featured tool for mapping objects to data structures.</span></span> <span data-ttu-id="6d046-216">Bei Dapper hat man sich vor allem auf das Thema Leistung konzentriert, anstatt die zugrunde liegenden Abfragen, die verwendet werden, um Daten abzurufen und zu aktualisieren, vollständig zu kapseln.</span><span class="sxs-lookup"><span data-stu-id="6d046-216">In the case of Dapper, its design goals focus on performance, rather than fully encapsulating the underlying queries it uses to retrieve and update data.</span></span> <span data-ttu-id="6d046-217">Da Dapper SQL nicht vom Entwickler abstrahiert, kann dieser sich mehr an der Hardware orientierten und genau die Abfragen schreiben, die er für einen bestimmten Vorgang zum Zugreifen auf Daten verwenden möchte.</span><span class="sxs-lookup"><span data-stu-id="6d046-217">Because it doesn't abstract SQL from the developer, Dapper is "closer to the metal" and lets developers write the exact queries they want to use for a given data access operation.</span></span>

<span data-ttu-id="6d046-218">EF Core enthält zwei wichtige Features, durch die sich dieses Tool zwar von Dapper unterscheidet, die aber gleichzeitig den Leistungsaufwand erhöhen.</span><span class="sxs-lookup"><span data-stu-id="6d046-218">EF Core has two significant features it provides which separate it from Dapper but also add to its performance overhead.</span></span> <span data-ttu-id="6d046-219">Das eine Feature ist dafür zuständig, LINQ-Ausdrücke in SQL zu übersetzen.</span><span class="sxs-lookup"><span data-stu-id="6d046-219">The first is translation from LINQ expressions into SQL.</span></span> <span data-ttu-id="6d046-220">Diese Übersetzungen werden zwar zwischengespeichert, aber es ist trotzdem sehr aufwändig, wenn sie das erste Mal erstellt werden müssen.</span><span class="sxs-lookup"><span data-stu-id="6d046-220">These translations are cached, but even so there is overhead in performing them the first time.</span></span> <span data-ttu-id="6d046-221">Das andere Feature ist dafür zuständig, Änderungen an Entitäten nachzuverfolgen, damit effiziente Updateanweisungen generiert werden können.</span><span class="sxs-lookup"><span data-stu-id="6d046-221">The second is change tracking on entities (so that efficient update statements can be generated).</span></span> <span data-ttu-id="6d046-222">Dieses Verhalten kann für bestimmte Abfragen mit der AsNotTracking-Erweiterung deaktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="6d046-222">This behavior can be turned off for specific queries by using the AsNotTracking extension.</span></span> <span data-ttu-id="6d046-223">Außerdem generiert EF Core SQL-Abfragen, die sehr effizient sind und kein zu hohes Maß an Leistung erfordern. Wenn Sie zudem die Abfragen, die ausgeführt werden sollen, genau steuern möchten, können Sie auch benutzerdefinierte SQL-Elemente hinzufügen oder eine gespeicherte Prozedur mit EF Core ausführen.</span><span class="sxs-lookup"><span data-stu-id="6d046-223">EF Core also generates SQL queries that usually are very efficient and in any case perfectly acceptable from a performance standpoint, but if you need fine control over the precise query to be executed, you can pass in custom SQL (or execute a stored procedure) using EF Core, too.</span></span> <span data-ttu-id="6d046-224">In diesem Szenario ist Dapper im Hinblick auf die Leistung zwar nützlicher als EF Core, aber der Unterschied ist nur gering.</span><span class="sxs-lookup"><span data-stu-id="6d046-224">In this case, Dapper still outperforms EF Core, but only slightly.</span></span> <span data-ttu-id="6d046-225">Julie Lerman führt in Ihrem MSDN-Artikel mit dem Titel [Datenpunkte – Dapper, Entity Framework und Hybrid-Apps](https://msdn.microsoft.com/magazine/mt703432.aspx) vom Mai 2016 einige Leistungsdaten auf.</span><span class="sxs-lookup"><span data-stu-id="6d046-225">Julie Lerman presents some performance data in her May 2016 MSDN article [Dapper, Entity Framework, and Hybrid Apps](https://msdn.microsoft.com/magazine/mt703432.aspx).</span></span> <span data-ttu-id="6d046-226">Außerdem finden Sie auf der [Dapper-Website](https://github.com/StackExchange/Dapper) zusätzliche Vergleichsdaten zur Leistung für verschiedene Datenzugriffsmethoden.</span><span class="sxs-lookup"><span data-stu-id="6d046-226">Additional performance benchmark data for a variety of data access methods can be found on [the Dapper site](https://github.com/StackExchange/Dapper).</span></span>

<span data-ttu-id="6d046-227">Wenn Sie sehen möchten, wie sich die Dapper-Syntax von der EF Core-Syntax unterscheidet, können Sie auf zwei unterschiedliche Versionen einer Methode zurückgreifen, die eine Liste von Elementen abruft:</span><span class="sxs-lookup"><span data-stu-id="6d046-227">To see how the syntax for Dapper varies from EF Core, consider these two versions of the same method for retrieving a list of items:</span></span>

```csharp
// EF Core
private readonly CatalogContext _context;
public async Task<IEnumerable<CatalogType>> GetCatalogTypes()
{
    return await _context.CatalogTypes.ToListAsync();
}

// Dapper
private readonly SqlConnection _conn;
public async Task<IEnumerable<CatalogType>> GetCatalogTypesWithDapper()
{
    return await _conn.QueryAsync<CatalogType>("SELECT * FROM CatalogType");
}
```

<span data-ttu-id="6d046-228">Wenn Sie komplexere Objektgraphen mit Dapper erstellen möchten, müssen Sie die jeweiligen Abfragen selbst erstellen. Im Gegensatz dazu müssen Sie in EF Core nur ein Include-Element hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="6d046-228">If you need to build more complex object graphs with Dapper, you need to write the associated queries yourself (as opposed to adding an Include as you would in EF Core).</span></span> <span data-ttu-id="6d046-229">Sie können zum Erstellen dieser Graphen verschiedene Syntaxmöglichkeiten verwenden, z.B. ein sogenanntes Multi Mapping-Feature, über das Sie mehreren zugeordneten Objekten einzelne Zeilen zuordnen können.</span><span class="sxs-lookup"><span data-stu-id="6d046-229">This is supported through a variety of syntaxes, including a feature called Multi Mapping that lets you map individual rows to multiple mapped objects.</span></span> <span data-ttu-id="6d046-230">Angenommen, Sie verfügen über eine Post-Klasse mit einer Owner-Eigenschaft des User-Typs. Dann gibt das folgende SQL-Element alle notwendigen Daten zurück:</span><span class="sxs-lookup"><span data-stu-id="6d046-230">For example, given a class Post with a property Owner of type User, the following SQL would return all of the necessary data:</span></span>

```sql
select * from #Posts p
left join #Users u on u.Id = p.OwnerId
Order by p.Id
```

<span data-ttu-id="6d046-231">Jede zurückgegebene Zeile umfasst sowohl User- als auch Post-Daten.</span><span class="sxs-lookup"><span data-stu-id="6d046-231">Each returned row includes both User and Post data.</span></span> <span data-ttu-id="6d046-232">Da die User-Daten über die Owner-Eigenschaft an die Post-Daten angefügt werden sollten, wird die folgende Funktion verwendet:</span><span class="sxs-lookup"><span data-stu-id="6d046-232">Since the User data should be attached to the Post data via its Owner property, the following function is used:</span></span>

```csharp
(post, user) => { post.Owner = user; return post; }
```

<span data-ttu-id="6d046-233">Im Folgenden wird die vollständig Codeliste dargestellt, über die eine Auflistung von Post-Daten mit der jeweiligen Owner-Eigenschaft, die mit den zugewiesenen User-Daten aufgefüllt ist, zurückgegeben werden kann:</span><span class="sxs-lookup"><span data-stu-id="6d046-233">The full code listing to return a collection of posts with their Owner property populated with the associated user data would be:</span></span>

```csharp
var sql = @"select * from #Posts p
left join #Users u on u.Id = p.OwnerId
Order by p.Id";
var data = connection.Query<Post, User, Post>(sql,
(post, user) => { post.Owner = user; return post;});
```

<span data-ttu-id="6d046-234">Da Entwickler im Zusammenhang mit Dapper weniger Kapselungen vornehmen müssen, ist es erforderlich, dass diese wissen, wie ihre Daten gespeichert werden, wie sie diese effizient abfragen können und wie sie mehr Code schreiben können, um diese Daten abzurufen.</span><span class="sxs-lookup"><span data-stu-id="6d046-234">Because it offers less encapsulation, Dapper requires developers know more about how their data is stored, how to query it efficiently, and write more code to fetch it.</span></span> <span data-ttu-id="6d046-235">Wenn das Modell verändert wird, darf nicht bloß eine neue Migration erstellt werden (ein anderes EF Core-Feature) und/oder Informationen einem Ort in DbContext zugeordnet werden. Stattdessen muss jede Abfrage, die betroffen ist, aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="6d046-235">When the model changes, instead of simply creating a new migration (another EF Core feature), and/or updating mapping information in one place in a DbContext, every query that is impacted must be updated.</span></span> <span data-ttu-id="6d046-236">Für diese Abfragen gibt es keine Garantien hinsichtlich der Kompilierzeit. Das kann zur Laufzeit zu Unterbrechungen führen, wenn Änderungen am Modell oder an der Datenbank vorgenommen werden, sodass es schwieriger wird, Fehler direkt zu ermitteln.</span><span class="sxs-lookup"><span data-stu-id="6d046-236">These queries have no compile time guarantees, so they may break at runtime in response to changes to the model or database, making errors more difficult to detect quickly.</span></span> <span data-ttu-id="6d046-237">Nichtsdestotrotz bietet Dapper eine schnelle Leistung.</span><span class="sxs-lookup"><span data-stu-id="6d046-237">In exchange for these tradeoffs, Dapper offers extremely fast performance.</span></span>

<span data-ttu-id="6d046-238">Sowohl für die meisten Anwendungen als auch für die meisten Bestandteile von Anwendungen bietet EF Core eine akzeptable Leistung.</span><span class="sxs-lookup"><span data-stu-id="6d046-238">For most applications, and most parts of almost all applications, EF Core offers acceptable performance.</span></span> <span data-ttu-id="6d046-239">Somit sind die Vorteile hinsichtlich der Leistung für Entwickler größer als die durch den Leistungsaufwand entstehenden Nachteile.</span><span class="sxs-lookup"><span data-stu-id="6d046-239">Thus, its developer productivity benefits are likely to outweigh its performance overhead.</span></span> <span data-ttu-id="6d046-240">Bei Abfragen, die von Zwischenspeicherungen profitieren können, beansprucht die eigentliche Abfrage nur wenig Zeit, wodurch kleine Unterschiede hinsichtlich der Abfrageleistung nur in der Theorie einen Unterschied machen.</span><span class="sxs-lookup"><span data-stu-id="6d046-240">For queries that can benefit from caching, the actual query may only be executed a tiny percentage of the time, making relatively small query performance differences moot.</span></span>

## <a name="sql-or-nosql"></a><span data-ttu-id="6d046-241">SQL und NoSQL im Vergleich</span><span class="sxs-lookup"><span data-stu-id="6d046-241">SQL or NoSQL</span></span>

<span data-ttu-id="6d046-242">In der Regel werden vorzugsweise relationale Datenbanken wie SQL Server als beständige Datenspeicher verwendet. Es gibt jedoch auch noch andere Möglichkeiten.</span><span class="sxs-lookup"><span data-stu-id="6d046-242">Traditionally, relational databases like SQL Server have dominated the marketplace for persistent data storage, but they are not the only solution available.</span></span> <span data-ttu-id="6d046-243">Beispielsweise bieten NoSQL-Datenbanken wie [MongoDB](https://www.mongodb.com/what-is-mongodb) eine andere Möglichkeit zum Speichern von Objekten.</span><span class="sxs-lookup"><span data-stu-id="6d046-243">NoSQL databases like [MongoDB](https://www.mongodb.com/what-is-mongodb) offer a different approach to storing objects.</span></span> <span data-ttu-id="6d046-244">Sie müssen nicht unbedingt Tabellen oder Zeilen Objekten zuordnen, sondern können auch den vollständigen Objektgraphen serialisieren und das Ergebnis speichern.</span><span class="sxs-lookup"><span data-stu-id="6d046-244">Rather than mapping objects to tables and rows, another option is to serialize the entire object graph, and store the result.</span></span> <span data-ttu-id="6d046-245">Dieser Ansatz ist, zumindest auf den ersten Blick, einfacher anzuwenden und hat positive Auswirkungen auf die Leistung.</span><span class="sxs-lookup"><span data-stu-id="6d046-245">The benefits of this approach, at least initially, are simplicity and performance.</span></span> <span data-ttu-id="6d046-246">Natürlich ist es einfacher, ein serialisiertes Objekt mit einem Schlüssel zu speichern, als das Objekt in mehrere Tabellen zu zerlegen, die in Beziehung miteinander stehen und aktualisiert werden müssen. Dabei kann es nämlich sein, dass Änderungen an den einzelnen Zeilen vorgenommen wurden, nachdem das Objekt zum letzten Mal aus der Datenbank abgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="6d046-246">It's certainly simpler to store a single serialized object with a key than to decompose the object into many tables with relationships and update and rows that may have changed since the object was last retrieved from the database.</span></span> <span data-ttu-id="6d046-247">Gleichzeitig ist das Abrufen und Deserialisieren eines einzelnen Objekts aus einem schlüsselbasiertem Speicher viel einfacher und schneller als bei komplizierten Verknüpfungen oder mehreren Datenbankabfragen, die erforderlich sind, um dieses Objekt vollständig aus einer relationalen Datenbank herzustellen.</span><span class="sxs-lookup"><span data-stu-id="6d046-247">Likewise, fetching and deserializing a single object from a key-based store is typically much faster and easier than complex joins or multiple database queries required to fully compose the same object from a relational database.</span></span> <span data-ttu-id="6d046-248">Im Zusammenhang mit NoSQL-Datenbanken gibt es keine Sperren, Transaktionen oder festgelegte Schemata, wodurch die Skalierung für mehrere Computer, die sehr große Datenbanken unterstützen, einfacher wird.</span><span class="sxs-lookup"><span data-stu-id="6d046-248">The lack of locks or transactions or a fixed schema also makes NoSQL databases very amenable to scaling across many machines, supporting very large datasets.</span></span>

<span data-ttu-id="6d046-249">Trotzdem haben NoSQL-Datenbanken auch Nachteile.</span><span class="sxs-lookup"><span data-stu-id="6d046-249">On the other hand, NoSQL databases (as they are typically called) have their drawbacks.</span></span> <span data-ttu-id="6d046-250">Relationale Datenbanken verwenden das Prinzip der Normalisierung, um Konsistenz zu erzwingen und zu vermeiden, dass Daten dupliziert werden.</span><span class="sxs-lookup"><span data-stu-id="6d046-250">Relational databases use normalization to enforce consistency and avoid duplication of data.</span></span> <span data-ttu-id="6d046-251">Dadurch wird die Gesamtgröße der Datenbank reduziert und sichergestellt, dass Updates von freigegebenen Daten unmittelbar nach deren Freigabe auch für die gesamte Datenbank verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="6d046-251">This reduces the total size of the database and ensures that updates to shared data are available immediately throughout the database.</span></span> <span data-ttu-id="6d046-252">In einer relationalen Datenbank kann beispielsweise in einer Adresstabelle per ID auf eine Landestabelle verwiesen werden. Wird später der Name eines Landes/einer Region geändert, steht diese Aktualisierung auch für die Adressdatensätze zur Verfügung, ohne dass diese selbst aktualisiert werden müssen.</span><span class="sxs-lookup"><span data-stu-id="6d046-252">In a relational database, an Address table might reference a Country table by ID, such that if the name of a country/region were changed, the address records would benefit from the update without themselves having to be updated.</span></span> <span data-ttu-id="6d046-253">Dann kann es hingegen sein, dass die Adresse und das zugehörige Land in einer NoSQL-Datenbank als Bestandteile vieler gespeicherter Objekte serialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="6d046-253">However, in a NoSQL database, Address and its associated Country might be serialized as part of many stored objects.</span></span> <span data-ttu-id="6d046-254">Nach der Aktualisierung des Namens eines Landes/einer Region müssten alle entsprechenden Objekte aktualisiert werden (nicht nur eine einzelne Zeile).</span><span class="sxs-lookup"><span data-stu-id="6d046-254">An update to a country/region name would require all such objects to be updated, rather than a single row.</span></span> <span data-ttu-id="6d046-255">Relationale Datenbanken können auch die relationale Integrität gewährleisten, indem sie Regeln wie Fremdschlüssel erzwingen.</span><span class="sxs-lookup"><span data-stu-id="6d046-255">Relational databases can also ensure relational integrity by enforcing rules like foreign keys.</span></span> <span data-ttu-id="6d046-256">NoSQL-Datenbanken bieten solche Einschränkungen für ihre Daten in der Regel nicht.</span><span class="sxs-lookup"><span data-stu-id="6d046-256">NoSQL databases typically do not offer such constraints on their data.</span></span>

<span data-ttu-id="6d046-257">Außerdem muss im Zusammenhang mit NoSQL-Datenbanken der komplizierte Aspekt der Versionsverwaltung beachtet werden.</span><span class="sxs-lookup"><span data-stu-id="6d046-257">Another complexity NoSQL databases must deal with is versioning.</span></span> <span data-ttu-id="6d046-258">Wenn sich die Eigenschaften eines Objekts ändern, kann es sein, dass dieses nicht aus früheren gespeicherten Versionen deserialisiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="6d046-258">When an object's properties change, it may not be able to be deserialized from past versions that were stored.</span></span> <span data-ttu-id="6d046-259">Daher müssen alle vorhandenen Objekte, die über eine serialisierte (Vorgänger-)Version des Objekts verfügen, aktualisiert werden, damit sie dem neuen Schema entsprechen.</span><span class="sxs-lookup"><span data-stu-id="6d046-259">Thus, all existing objects that have a serialized (previous) version of the object must be updated to conform to its new schema.</span></span> <span data-ttu-id="6d046-260">Dieses Konzept unterscheidet sich von der Vorgehensweise bei relationalen Datenbanken, denn in diesem Zusammenhang erfordern Schemaänderungen häufig die Aktualisierung von Skripts oder das Zuordnen von Updates.</span><span class="sxs-lookup"><span data-stu-id="6d046-260">This is not conceptually different from a relational database, where schema changes sometimes require update scripts or mapping updates.</span></span> <span data-ttu-id="6d046-261">Die Anzahl von Einträgen, die geändert werden müssen, ist jedoch beim NoSQL-Ansatz häufig viel höher, da mehr Daten dupliziert werden.</span><span class="sxs-lookup"><span data-stu-id="6d046-261">However, the number of entries that must be modified is often much greater in the NoSQL approach, because there is more duplication of data.</span></span>

<span data-ttu-id="6d046-262">In NoSQL-Datenbanken ist es möglich, mehrere Versionen von Objekten zu speichern. Dies wird von relationalen Datenbanken mit festem Schema in der Regel nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="6d046-262">It's possible in NoSQL databases to store multiple versions of objects, something fixed schema relational databases typically do not support.</span></span> <span data-ttu-id="6d046-263">In diesem Fall muss Ihr Anwendungscode erfassen, ob Vorgängerversionen von Objekten vorhanden sind. Dies macht das Konzept zusätzlich komplexer.</span><span class="sxs-lookup"><span data-stu-id="6d046-263">However, in this case your application code will need to account for the existence of previous versions of objects, adding additional complexity.</span></span>

<span data-ttu-id="6d046-264">NoSQL-Datenbanken erzwingen in der Regel nicht das [ACID](https://en.wikipedia.org/wiki/ACID)-Prinzip, weshalb sie im Hinblick auf die Leistung und Skalierbarkeit einen Vorteil gegenüber relationalen Datenbanken aufweisen.</span><span class="sxs-lookup"><span data-stu-id="6d046-264">NoSQL databases typically do not enforce [ACID](https://en.wikipedia.org/wiki/ACID), which means they have both performance and scalability benefits over relational databases.</span></span> <span data-ttu-id="6d046-265">Sie eignen sich besonders gut für extrem große Datasets und Objekte, die sich nicht zum Speichern in genormten Tabellenstrukturen eignen.</span><span class="sxs-lookup"><span data-stu-id="6d046-265">They're well-suited to extremely large datasets and objects that are not well-suited to storage in normalized table structures.</span></span> <span data-ttu-id="6d046-266">Sie müssen sich aber nicht zwischen relationalen Datenbanken und NoSQL-Datenbanken entscheiden: Sie können sogar innerhalb einer Anwendung für jeden einzelnen Bestandteil entscheiden, welche Art von Datenbank sich besser eignet.</span><span class="sxs-lookup"><span data-stu-id="6d046-266">There is no reason why a single application cannot take advantage of both relational and NoSQL databases, using each where it is best suited.</span></span>

## <a name="azure-documentdb"></a><span data-ttu-id="6d046-267">Azure DocumentDB</span><span class="sxs-lookup"><span data-stu-id="6d046-267">Azure DocumentDB</span></span>

<span data-ttu-id="6d046-268">Azure DocumentDB ist ein vollständig verwalteter Dienst für NoSQL-Datenbanken, der einen cloudbasierten schemalosen Datenspeicher umfasst.</span><span class="sxs-lookup"><span data-stu-id="6d046-268">Azure DocumentDB is a fully managed NoSQL database service that offers cloud-based schema-free data storage.</span></span> <span data-ttu-id="6d046-269">DocumentDB ist auf schnelle und vorhersagbare Leistung, Hochverfügbarkeit, elastische Skalierung und globale Verteilung ausgerichtet.</span><span class="sxs-lookup"><span data-stu-id="6d046-269">DocumentDB is built for fast and predictable performance, high availability, elastic scaling, and global distribution.</span></span> <span data-ttu-id="6d046-270">Obwohl es sich um einen Dienst für NoSQL-Datenbanken handelt, können Entwickler aufwändige und vertraute SQL-Abfragefunktionen für JSON-Daten verwenden.</span><span class="sxs-lookup"><span data-stu-id="6d046-270">Despite being a NoSQL database, developers can use rich and familiar SQL query capabilities on JSON data.</span></span> <span data-ttu-id="6d046-271">Alle Ressourcen in DocumentDB werden als JSON-Dokumente gespeichert.</span><span class="sxs-lookup"><span data-stu-id="6d046-271">All resources in DocumentDB are stored as JSON documents.</span></span> <span data-ttu-id="6d046-272">Ressourcen werden als _Elemente_, bei denen es sich um Dokumente mit Metadaten handelt, und als _Feeds_ verwaltet, bei denen es sich um Auflistungen von Elementen handelt.</span><span class="sxs-lookup"><span data-stu-id="6d046-272">Resources are managed as _items_, which are documents containing metadata, and _feeds_, which are collections of items.</span></span> <span data-ttu-id="6d046-273">In Abbildung 8–2 wird die Beziehung zwischen verschiedenen DocumentDB-Ressourcen dargestellt.</span><span class="sxs-lookup"><span data-stu-id="6d046-273">Figure 8-2 shows the relationship between different DocumentDB resources.</span></span>

![Die hierarchische Beziehung zwischen Ressourcen in DocumentDB; eine NoSQL-Datenbank im JSON-Format](./media/image8-2.png)

<span data-ttu-id="6d046-275">**Abbildung 8–2.**</span><span class="sxs-lookup"><span data-stu-id="6d046-275">**Figure 8-2.**</span></span> <span data-ttu-id="6d046-276">Ressourcenorganisation in DocumentDB</span><span class="sxs-lookup"><span data-stu-id="6d046-276">DocumentDB resource organization.</span></span>

<span data-ttu-id="6d046-277">Bei der DocumentDB-Abfragesprache handelt es sich um eine einfache, aber leistungsstarke Schnittstelle zum Abfragen von JSON-Dokumenten.</span><span class="sxs-lookup"><span data-stu-id="6d046-277">The DocumentDB query language is a simple yet powerful interface for querying JSON documents.</span></span> <span data-ttu-id="6d046-278">Diese Sprache unterstützt einen Teil der durch das American National Standards Institute (ANSI) festgelegten Grammatik und umfasst eine ausführliche Integration von JavaScript-Objekten, Arrays, Objektkonstruktionen und Funktionsaufrufen.</span><span class="sxs-lookup"><span data-stu-id="6d046-278">The language supports a subset of ANSI SQL grammar and adds deep integration of JavaScript object, arrays, object construction, and function invocation.</span></span>

<span data-ttu-id="6d046-279">**Ressourcen: DocumentDB**</span><span class="sxs-lookup"><span data-stu-id="6d046-279">**References – DocumentDB**</span></span>

- <span data-ttu-id="6d046-280">Einführung in DocumentDB</span><span class="sxs-lookup"><span data-stu-id="6d046-280">DocumentDB Introduction</span></span>  
  <https://docs.microsoft.com/azure/documentdb/documentdb-introduction>

## <a name="other-persistence-options"></a><span data-ttu-id="6d046-281">Andere Persistenzoptionen</span><span class="sxs-lookup"><span data-stu-id="6d046-281">Other persistence options</span></span>

<span data-ttu-id="6d046-282">Neben relationalen Speicheroptionen und NoSQL-Optionen können ASP.NET Core-Anwendungen auch Azure Storage verwenden, um verschiedene Datenformate und Dateien auf cloudbasierte, skalierbare Weise zu speichern.</span><span class="sxs-lookup"><span data-stu-id="6d046-282">In addition to relational and NoSQL storage options, ASP.NET Core applications can use Azure Storage to store a variety of data formats and files in a cloud-based, scalable fashion.</span></span> <span data-ttu-id="6d046-283">Azure Storage ist im großen Umfang skalierbar, d.h., Sie können zunächst kleine Mengen von Daten speichern und zentral hochskalieren, und den Umfang dann auf mehrere Hundert Terrabyte ausweiten, falls dies die Anwendung erfordert.</span><span class="sxs-lookup"><span data-stu-id="6d046-283">Azure Storage is massively scalable, so you can start out storing small amounts of data and scale up to storing hundreds or terabytes if your application requires it.</span></span> <span data-ttu-id="6d046-284">Azure Storage unterstützt vier verschiedene Arten von Daten:</span><span class="sxs-lookup"><span data-stu-id="6d046-284">Azure Storage supports four kinds of data:</span></span>

- <span data-ttu-id="6d046-285">Blob Storage für unstrukturierten Text oder als Binärspeicher (auch als Objektspeicher bezeichnet)</span><span class="sxs-lookup"><span data-stu-id="6d046-285">Blob Storage for unstructured text or binary storage, also referred to as object storage.</span></span>

- <span data-ttu-id="6d046-286">Table Storage für strukturierte Datasets, auf die über Zeilenschlüssel zugegriffen werden kann</span><span class="sxs-lookup"><span data-stu-id="6d046-286">Table Storage for structured datasets, accessible via row keys.</span></span>

- <span data-ttu-id="6d046-287">Queue Storage für zuverlässiges warteschlangenbasiertes Messaging</span><span class="sxs-lookup"><span data-stu-id="6d046-287">Queue Storage for reliable queue-based messaging.</span></span>

- <span data-ttu-id="6d046-288">File Storage für den Zugriff auf freigegebene Dateien zwischen virtuellen Azure-Computern und lokalen Anwendungen</span><span class="sxs-lookup"><span data-stu-id="6d046-288">File Storage for shared file access between Azure virtual machines and on-premises applications.</span></span>

<span data-ttu-id="6d046-289">**Ressourcen: Azure Storage**</span><span class="sxs-lookup"><span data-stu-id="6d046-289">**References – Azure Storage**</span></span>

- <span data-ttu-id="6d046-290">Einführung in Azure Storage</span><span class="sxs-lookup"><span data-stu-id="6d046-290">Azure Storage Introduction</span></span>  
  <https://docs.microsoft.com/azure/storage/storage-introduction>

## <a name="caching"></a><span data-ttu-id="6d046-291">Zwischenspeicherung</span><span class="sxs-lookup"><span data-stu-id="6d046-291">Caching</span></span>

<span data-ttu-id="6d046-292">In Webanwendungen sollte jede Webanforderung so schnell wie möglich abgeschlossen werden.</span><span class="sxs-lookup"><span data-stu-id="6d046-292">In web applications, each web request should be completed in the shortest time possible.</span></span> <span data-ttu-id="6d046-293">Um dies zu erreichen, können Sie z.B. die Anzahl der externen Aufrufe beschränken, die der Server ausführen muss, um eine Anforderung abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="6d046-293">One way to achieve this is to limit the number of external calls the server must make to complete the request.</span></span> <span data-ttu-id="6d046-294">Das Zwischenspeichern umfasst das Speichern einer Kopie von Daten auf dem Server oder in einem anderen Datenspeicher, der leichter abgefragt werden kann als die Datenquelle.</span><span class="sxs-lookup"><span data-stu-id="6d046-294">Caching involves storing a copy of data on the server (or another data store that is more easily queried than the source of the data).</span></span> <span data-ttu-id="6d046-295">Webanwendungen und insbesondere traditionelle Webanwendungen, bei denen es sich nicht um Single-Page-Webanwendung handelt, müssen mit jeder Anforderung die gesamte Benutzeroberfläche erstellen.</span><span class="sxs-lookup"><span data-stu-id="6d046-295">Web applications, and especially non-SPA traditional web applications, need to build the entire user interface with every request.</span></span> <span data-ttu-id="6d046-296">Darum müssen häufig dieselben Datenbankabfragen wiederholt von einer Benutzeranforderung zur nächsten durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="6d046-296">This frequently involves making many of the same database queries repeatedly from one user request to the next.</span></span> <span data-ttu-id="6d046-297">In den meisten Fällen ändern sich diese Daten nur selten, sodass es keinen Grund gibt, sie ständig aus der Datenbank abzufragen.</span><span class="sxs-lookup"><span data-stu-id="6d046-297">In most cases, this data changes rarely, so there is little reason to constantly request it from the database.</span></span> <span data-ttu-id="6d046-298">ASP.NET Core unterstützt das Zwischenspeichern von Antworten, ganzen Seiten sowie Daten und somit ein präziseres Zwischenspeicherungsverhalten.</span><span class="sxs-lookup"><span data-stu-id="6d046-298">ASP.NET Core supports response caching, for caching entire pages, and data caching, which supports more granular caching behavior.</span></span>

<span data-ttu-id="6d046-299">Wenn Sie die Zwischenspeicherung implementieren, müssen Sie das Prinzip „Separation of Concerns“ im Hinterkopf behalten.</span><span class="sxs-lookup"><span data-stu-id="6d046-299">When implementing caching, it's important to keep in mind separation of concerns.</span></span> <span data-ttu-id="6d046-300">Fügen Sie möglichst keine Logik für die Zwischenspeicherung in Ihre Logik für den Datenzugriff oder Ihre Benutzeroberfläche ein.</span><span class="sxs-lookup"><span data-stu-id="6d046-300">Avoid implementing caching logic in your data access logic, or in your user interface.</span></span> <span data-ttu-id="6d046-301">Kapseln Sie die Zwischenspeicherung stattdessen in ihren eigenen Klassen, und verwenden Sie die Konfiguration, um ihr Verhalten zu steuern.</span><span class="sxs-lookup"><span data-stu-id="6d046-301">Instead, encapsulate caching in its own classes, and use configuration to manage its behavior.</span></span> <span data-ttu-id="6d046-302">Dies entspricht dem Offen/Geschlossen-Prinzip und dem Prinzip der einzigen Verantwortung (Single Responsibility) und vereinfacht die Verwaltung der Verwendungsweise der Zwischenspeicherung in Ihrer Anwendung, während diese immer umfangreicher wird.</span><span class="sxs-lookup"><span data-stu-id="6d046-302">This follows the Open/Closed and Single Responsibility principles, and will make it easier for you to manage how you use caching in your application as it grows.</span></span>

### <a name="aspnet-core-response-caching"></a><span data-ttu-id="6d046-303">Zwischenspeichern von Antworten mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6d046-303">ASP.NET Core response caching</span></span>

<span data-ttu-id="6d046-304">ASP.NET Core unterstützt zwei Ebenen des Zwischenspeicherns von Antworten.</span><span class="sxs-lookup"><span data-stu-id="6d046-304">ASP.NET Core supports two levels of response caching.</span></span> <span data-ttu-id="6d046-305">Auf der ersten Ebene werden zwar keine Elemente auf dem Server zwischengespeichert, aber es werden HTTP-Header hinzugefügt, die Clients und Proxyserver anweisen, Antworten zwischenzuspeichern.</span><span class="sxs-lookup"><span data-stu-id="6d046-305">The first level does not cache anything on the server, but adds HTTP headers that instruct clients and proxy servers to cache responses.</span></span> <span data-ttu-id="6d046-306">Dies wird implementiert, indem das ResponseCache-Attribut individuellen Controllern oder Aktionen hinzugefügt wird:</span><span class="sxs-lookup"><span data-stu-id="6d046-306">This is implemented by adding the ResponseCache attribute to individual controllers or actions:</span></span>

```csharp
    [ResponseCache(Duration = 60)]
    public IActionResult Contact()
    { }

    ViewData["Message"] = "Your contact page.";
    return View();
}
```

<span data-ttu-id="6d046-307">Im vorherigen Beispiel wird der folgende Header der Antwort hinzugefügt, der Clients dazu auffordert, das Ergebnis bis zu 60 Sekunden lang zwischenzuspeichern.</span><span class="sxs-lookup"><span data-stu-id="6d046-307">The previous example will result in the following header being added to the response, instructing clients to cache the result for up to 60 seconds.</span></span>

<span data-ttu-id="6d046-308">Cache-Control: public,max-age=60</span><span class="sxs-lookup"><span data-stu-id="6d046-308">Cache-Control: public,max-age=60</span></span>

<span data-ttu-id="6d046-309">Damit die serverseitige Zwischenspeicherung im Arbeitsspeicher der Anwendung hinzugefügt werden kann, müssen Sie auf das NuGet-Paket „Microsoft.AspNetCore.ResponseCaching“ verweisen, und dann die Antworten zwischenspeichernde Middleware hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="6d046-309">In order to add server-side in-memory caching to the application, you must reference the Microsoft.AspNetCore.ResponseCaching NuGet package, and then add the Response Caching middleware.</span></span> <span data-ttu-id="6d046-310">Diese Middleware wird beim Start sowohl in „ConfigureServices“ als auch in „Configure“ konfiguriert:</span><span class="sxs-lookup"><span data-stu-id="6d046-310">This middleware is configured in both ConfigureServices and Configure in Startup:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCaching();
}

public void Configure(IApplicationBuilder app)
{
    app.UseResponseCaching();
}
```

<span data-ttu-id="6d046-311">Die Antworten zwischenspeichernde Middleware speichert automatisch auf der Grundlage von einigen Bedingungen Antworten zwischen, die Sie anpassen können.</span><span class="sxs-lookup"><span data-stu-id="6d046-311">The Response Caching Middleware will automatically cache responses based on a set of conditions, which you can customize.</span></span> <span data-ttu-id="6d046-312">Standardmäßig werden nur „200 – OK“-Antworten zwischengespeichert, die über die Methoden GET oder HEAD angefordert werden.</span><span class="sxs-lookup"><span data-stu-id="6d046-312">By default, only 200 (OK) responses requested via GET or HEAD methods are cached.</span></span> <span data-ttu-id="6d046-313">Außerdem müssen Anforderungen über eine Antwort mit Cache-Control und öffentlichen Headers verfügen und können keine Header für das Authorization- oder Set-Cookie umfassen.</span><span class="sxs-lookup"><span data-stu-id="6d046-313">In addition, requests must have a response with a Cache-Control: public header, and cannot include headers for Authorization or Set-Cookie.</span></span> <span data-ttu-id="6d046-314">Weitere Informationen finden Sie in einer [vollständigen Liste von Zwischenspeicherungsbedingungen, die von der Antworten zwischenspeichernden Middleware verwendet werden](/aspnet/core/performance/caching/middleware#conditions-for-caching).</span><span class="sxs-lookup"><span data-stu-id="6d046-314">See a [complete list of the caching conditions used by the response caching middleware](/aspnet/core/performance/caching/middleware#conditions-for-caching).</span></span>

### <a name="data-caching"></a><span data-ttu-id="6d046-315">Zwischenspeichern von Daten</span><span class="sxs-lookup"><span data-stu-id="6d046-315">Data caching</span></span>

<span data-ttu-id="6d046-316">Anstelle des oder neben dem Zwischenspeichern vollständiger Webantworten können Sie auch die Ergebnisse einzelner Datenabfragen zwischenspeichern.</span><span class="sxs-lookup"><span data-stu-id="6d046-316">Rather than (or in addition to) caching full web responses, you can cache the results of individual data queries.</span></span> <span data-ttu-id="6d046-317">Dafür können Sie speicherinternes Caching auf dem Webserver oder einen [verteilten Cache verwenden](/aspnet/core/performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="6d046-317">For this, you can use in memory caching on the web server, or use [a distributed cache](/aspnet/core/performance/caching/distributed).</span></span> <span data-ttu-id="6d046-318">In diesem Abschnitt erfahren Sie, wie Sie das speicherinterne Caching implementieren.</span><span class="sxs-lookup"><span data-stu-id="6d046-318">This section will demonstrate how to implement in memory caching.</span></span>

<span data-ttu-id="6d046-319">Sie fügen in ConfigureServices Unterstützung für speicherinternes oder verteiltes Zwischenspeichern hinzu:</span><span class="sxs-lookup"><span data-stu-id="6d046-319">You add support for memory (or distributed) caching in ConfigureServices:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMemoryCache();
    services.AddMvc();
}
```

<span data-ttu-id="6d046-320">Achten Sie darauf, dass auch das NuGet-Paket „Microsoft.Extensions.Caching.Memory“ hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="6d046-320">Be sure to add the Microsoft.Extensions.Caching.Memory NuGet package as well.</span></span>

<span data-ttu-id="6d046-321">Sobald Sie den Dienst hinzugefügt haben, fordern Sie über Dependency Injection IMemoryCache an, wenn Sie auf den Cache zugreifen müssen.</span><span class="sxs-lookup"><span data-stu-id="6d046-321">Once you've added the service, you request IMemoryCache via dependency injection wherever you need to access the cache.</span></span> <span data-ttu-id="6d046-322">In diesem Beispiel verwendet CachedCatalogService das Entwurfsmuster „Proxy“ (oder „Decorator“), indem eine alternative Implementierung von ICatalogService bereitgestellt wird, die den Zugriff auf die zugrunde liegende CatalogService-Implementierung steuert (oder Verhalten zu dieser hinzufügt).</span><span class="sxs-lookup"><span data-stu-id="6d046-322">In this example, the CachedCatalogService is using the Proxy (or Decorator) design pattern, by providing an alternative implementation of ICatalogService that controls access to (or adds behavior to) the underlying CatalogService implementation.</span></span>

```csharp
public class CachedCatalogService : ICatalogService
{
    private readonly IMemoryCache _cache;
    private readonly CatalogService _catalogService;
    private static readonly string _brandsKey = "brands";
    private static readonly string _typesKey = "types";
    private static readonly string _itemsKeyTemplate = "items-{0}-{1}-{2}-{3}";
    private static readonly TimeSpan _defaultCacheDuration = TimeSpan.FromSeconds(30);
    public CachedCatalogService(IMemoryCache cache,
    CatalogService catalogService)
    {
        _cache = cache;
        _catalogService = catalogService;
    }

    public async Task<IEnumerable<SelectListItem>> GetBrands()
    {
        return await _cache.GetOrCreateAsync(_brandsKey, async entry =>
        {
            entry.SlidingExpiration = _defaultCacheDuration;
            return await _catalogService.GetBrands();
        });
    }

    public async Task<Catalog> GetCatalogItems(int pageIndex, int itemsPage, int? brandID, int? typeId)
    {
        string cacheKey = String.Format(_itemsKeyTemplate, pageIndex, itemsPage, brandID, typeId);
        return await _cache.GetOrCreateAsync(cacheKey, async entry =>
        {
            entry.SlidingExpiration = _defaultCacheDuration;
            return await _catalogService.GetCatalogItems(pageIndex, itemsPage, brandID, typeId);
        });
    }

    public async Task<IEnumerable<SelectListItem>> GetTypes()
    {
        return await _cache.GetOrCreateAsync(_typesKey, async entry =>
        {
            entry.SlidingExpiration = _defaultCacheDuration;
            return await _catalogService.GetTypes();
        });
    }
}
```

<span data-ttu-id="6d046-323">Wenn Sie die Anwendung so konfigurieren möchten, dass zwar die zwischengespeicherte Version des Diensts verwendet wird, der Dienst aber immer noch die Instanz von CatalogService abrufen kann, die er in seinem Konstruktor benötigt, können Sie Folgendes zu ConfigureServices hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="6d046-323">To configure the application to use the cached version of the service, but still allow the service to get the instance of CatalogService it needs in its constructor, you would add the following in ConfigureServices:</span></span>

```csharp
services.AddMemoryCache();
services.AddScoped<ICatalogService, CachedCatalogService>();
services.AddScoped<CatalogService>();
```

<span data-ttu-id="6d046-324">Wenn dies funktioniert, sendet die Datenbank anstatt bei jeder Anforderung nur einmal pro Minute einen Aufruf, um die Katalogdaten abzurufen.</span><span class="sxs-lookup"><span data-stu-id="6d046-324">With this in place, the database calls to fetch the catalog data will only be made once per minute, rather than on every request.</span></span> <span data-ttu-id="6d046-325">Je nachdem, wie viel Datenverkehr an die Website gesendet wird, kann dies deutliche Auswirkungen auf die Anzahl von Abfragen, die an die Datenbank gesendet werden, und die durchschnittliche Seitenladezeit der Startseite haben, die zu diesem Zeitpunkt von allen drei Abfragen abhängig ist, die von diesem Dienst zur Verfügung gestellt werden.</span><span class="sxs-lookup"><span data-stu-id="6d046-325">Depending on the traffic to the site, this can have a very significant impact on the number of queries made to the database, and the average page load time for the home page that currently depends on all three of the queries exposed by this service.</span></span>

<span data-ttu-id="6d046-326">Wenn Sie allerdings das Caching implementieren, kann dies dazu führen, dass _veraltete Daten_ verwendet werden. Damit sind Daten gemeint, die in der Quelle verändert wurden, von denen aber eine veraltete Version im Cache erhalten bleibt.</span><span class="sxs-lookup"><span data-stu-id="6d046-326">An issue that arises when caching is implemented is _stale data_ – that is, data that has changed at the source but an out of date version remains in the cache.</span></span> <span data-ttu-id="6d046-327">Dieses Problem können Sie umgehen, wenn Sie eine kurze Cachedauer verwenden, da es für häufig verwendete Anwendungen nur wenig hilfreich ist, diese Dauer auszuweiten.</span><span class="sxs-lookup"><span data-stu-id="6d046-327">A simple way to mitigate this issue is to use small cache durations, since for a busy application there is limited additional benefit to extending the length data is cached.</span></span> <span data-ttu-id="6d046-328">Angenommen, Sie verfügen z.B. über eine Seite, die eine einzelne Datenbankabfrage sendet und zehnmal pro Sekunde abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="6d046-328">For example, consider a page that makes a single database query, and is requested 10 times per second.</span></span> <span data-ttu-id="6d046-329">Wenn diese Seite für eine Minute zwischengespeichert wird, hat dies zur Folge, dass die Anzahl der Datenbankabfragen pro Minute von 600 auf 1, also um 99,8 %, reduziert wird.</span><span class="sxs-lookup"><span data-stu-id="6d046-329">If this page is cached for one minute, it will result in the number of database queries made per minute to drop from 600 to 1, a reduction of 99.8%.</span></span> <span data-ttu-id="6d046-330">Wenn die Cachedauer stattdessen auf eine Stunde festgelegt werden würde, würde die Anzahl der Abfragen insgesamt um 99,997 % reduziert werden. Dann kann es jedoch sein, dass die zwischengespeicherten Daten viel häufiger veralten.</span><span class="sxs-lookup"><span data-stu-id="6d046-330">If instead the cache duration were made one hour, the overall reduction would be 99.997%, but now the likelihood and potential age of stale data are both increased dramatically.</span></span>

<span data-ttu-id="6d046-331">Stattdessen können Sie proaktiv Cacheeinträge entfernen, wenn die darin enthaltenen Daten aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="6d046-331">Another approach is to proactively remove cache entries when the data they contain is updated.</span></span> <span data-ttu-id="6d046-332">Jeder einzelne Eintrag kann entfernt werden, wenn dessen Schlüssel bekannt ist:</span><span class="sxs-lookup"><span data-stu-id="6d046-332">Any individual entry can be removed if its key is known:</span></span>

```csharp
_cache.Remove(cacheKey);
```

<span data-ttu-id="6d046-333">Wenn in Ihrer Anwendung Funktionen zum Aktualisieren von zwischengespeicherten Einträgen enthalten sind, können Sie die jeweiligen Cacheeinträge aus dem Code entfernen, der die Updates ausführt.</span><span class="sxs-lookup"><span data-stu-id="6d046-333">If your application exposes functionality for updating entries that it caches, you can remove the corresponding cache entries in your code that performs the updates.</span></span> <span data-ttu-id="6d046-334">Es kann manchmal sein, dass es viele verschiedene Einträge gibt, die von verschiedenen Daten abhängig sind.</span><span class="sxs-lookup"><span data-stu-id="6d046-334">Sometimes there may be many different entries that depend on a particular set of data.</span></span> <span data-ttu-id="6d046-335">In diesem Fall kann es hilfreich sein, mithilfe von CancellationChangeToken Abhängigkeiten zwischen Cacheeinträgen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="6d046-335">In that case, it can be useful to create dependencies between cache entries, by using a CancellationChangeToken.</span></span> <span data-ttu-id="6d046-336">Mit einem CancellationChangeToken-Element können Sie festlegen, dass mehrere Cacheeinträge gleichzeitig ablaufen, indem Sie das Token entfernen.</span><span class="sxs-lookup"><span data-stu-id="6d046-336">With a CancellationChangeToken, you can expire multiple cache entries at once by cancelling the token.</span></span>

```csharp
// configure CancellationToken and add entry to cache
var cts = new CancellationTokenSource();
_cache.Set("cts", cts);
_cache.Set(cacheKey,
itemToCache,
new CancellationChangeToken(cts.Token));

// elsewhere, expire the cache by cancelling the token\
_cache.Get<CancellationTokenSource>("cts").Cancel();
```

<span data-ttu-id="6d046-337">Die Zwischenspeicherung kann die Leistung von Webseiten drastisch verbessern, die immer wieder die gleichen Werte von der Datenbank anfordern.</span><span class="sxs-lookup"><span data-stu-id="6d046-337">Caching can dramatically improve the performance of web pages that repeatedly request the same values from the database.</span></span> <span data-ttu-id="6d046-338">Stellen Sie sicher, dass Sie den Datenzugriff und die Seitenleistung messen, bevor Sie die Zwischenspeicherung hinzufügen. Wenden Sie die Zwischenspeicherung nur für notwendige Verbesserungen an.</span><span class="sxs-lookup"><span data-stu-id="6d046-338">Be sure to measure data access and page performance before applying caching, and only apply caching where you see a need for improvement.</span></span> <span data-ttu-id="6d046-339">Das Zwischenspeichern verbraucht Arbeitsspeicherressourcen des Webservers, und die Anwendung wird komplexer, weshalb es wichtig ist, dass Sie diese Art von Optimierung nicht voreilig durchführen.</span><span class="sxs-lookup"><span data-stu-id="6d046-339">Caching consumes web server memory resources and increases the complexity of the application, so it’s important you don’t prematurely optimize using this technique.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="6d046-340">[Zurück](develop-asp-net-core-mvc-apps.md)
>[Weiter](test-asp-net-core-mvc-apps.md)</span><span class="sxs-lookup"><span data-stu-id="6d046-340">[Previous](develop-asp-net-core-mvc-apps.md)
[Next](test-asp-net-core-mvc-apps.md)</span></span>