---
title: Neues in C# 8.0 – C#-Leitfaden
description: Überblick über die neuen Funktionen von C# 8.0.
ms.date: 09/20/2019
ms.openlocfilehash: 540b95beaf00c17812a3b602602504278be69b0e
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2019
ms.locfileid: "74429389"
---
# <a name="whats-new-in-c-80"></a>Neues in C# 8.0

C# 8.0 fügt der Sprache C# die folgenden Features und Verbesserungen hinzu:

- [Readonly-Member](#readonly-members)
- [Standardschnittstellenmethoden](#default-interface-methods)
- [Verbesserungen am Musterabgleich](#more-patterns-in-more-places):
  - [switch-Ausdrücke](#switch-expressions)
  - [Eigenschaftsmuster](#property-patterns)
  - [Tupelmuster](#tuple-patterns)
  - [Positionsmuster](#positional-patterns)
- [using-Deklarationen](#using-declarations)
- [Statische lokale Funktionen](#static-local-functions)
- [Verwerfbare Referenzstrukturen](#disposable-ref-structs)
- [Nullwerte zulassende Verweistypen](#nullable-reference-types)
- [Asynchrone Streams](#asynchronous-streams)
- [Indizes und Bereiche](#indices-and-ranges)
- [NULL-Coalescing-Zuweisung](#null-coalescing-assignment)
- [Nicht verwaltete konstruierte Typen](#unmanaged-constructed-types)
- [Stackalloc in geschachtelten Ausdrücken](#stackalloc-in-nested-expressions)
- [Erweiterung von interpolierten ausführlichen Zeichenfolgen](#enhancement-of-interpolated-verbatim-strings)

C# 8.0 wird unter **.NET Core 3.x** und **.NET Standard 2.1** unterstützt. Weitere Informationen finden Sie unter [C#-Sprachversionsverwaltung](../language-reference/configure-language-version.md).

Der Rest dieses Artikels beschreibt diese Funktionen kurz. Wenn ausführliche Artikel verfügbar sind, werden Links zu diesen Tutorials und Übersichten bereitgestellt. Sie können sich diese Funktionen in unserer Umgebung mit dem globalen `dotnet try`-Tool näher ansehen:

1. Installieren Sie das globale [dotnet-try](https://github.com/dotnet/try/blob/master/README.md#setup)-Tool.
1. Klonen Sie das [dotnet/try-samples](https://github.com/dotnet/try-samples)-Repository.
1. Legen Sie das aktuelle Verzeichnis auf das Unterverzeichnis *csharp8* für das *try-samples*-Repository fest.
1. Führen Sie aus `dotnet try`.

## <a name="readonly-members"></a>Readonly-Member

Sie können den `readonly`-Modifizierer auf jeden Member einer Struktur anwenden. Damit wird angezeigt, dass der Member den Zustand nicht ändert. Dies ist granularer als das Anwenden des `readonly`-Modifikators auf eine `struct`-Deklaration.  Betrachten Sie folgende veränderliche Struktur:

```csharp
public struct Point
{
    public double X { get; set; }
    public double Y { get; set; }
    public double Distance => Math.Sqrt(X * X + Y * Y);

    public override string ToString() =>
        $"({X}, {Y}) is {Distance} from the origin";
}
```

Wie bei den meisten Strukturen verändert die `ToString()`-Methode den Zustand nicht. Sie könnten dies durch Hinzufügen des `readonly`-Modifikators zur Deklaration von `ToString()` angeben:

```csharp
public readonly override string ToString() =>
    $"({X}, {Y}) is {Distance} from the origin";
```

Die vorhergehende Änderung generiert eine Compilerwarnung, weil `ToString` auf die `Distance`-Eigenschaft zugreift, die nicht als `readonly` markiert ist:

```console
warning CS8656: Call to non-readonly member 'Point.Distance.get' from a 'readonly' member results in an implicit copy of 'this'
```

Der Compiler warnt Sie, wenn er eine Defensivkopie erstellen muss.  Die `Distance`-Eigenschaft verändert nicht den Zustand, sodass Sie diese Warnung aufheben können, indem Sie der Deklaration den `readonly`-Modifizierer hinzufügen:

```csharp
public readonly double Distance => Math.Sqrt(X * X + Y * Y);
```

Beachten Sie, dass der `readonly`-Modifizierer bei einer schreibgeschützten Eigenschaft erforderlich ist. Der Compiler geht nicht davon aus, dass `get`-Zugriffsmethoden den Zustand nicht ändern. Sie müssen `readonly` explizit deklarieren. Automatisch implementierte Eigenschaften sind eine Ausnahme; der Compiler sieht alle automatisch implementierten Getter als schreibgeschützt an, sodass es hier nicht notwendig ist, den `readonly`-Modifizierer zu den `X`- und `Y`-Eigenschaften hinzuzufügen.

Der Compiler erzwingt die Regel, dass `readonly`-Member den Status nicht ändern. Die folgende Methode wird nicht kompiliert, es sei denn, Sie entfernen den `readonly`-Modifizierer:

```csharp
public readonly void Translate(int xOffset, int yOffset)
{
    X += xOffset;
    Y += yOffset;
}
```

Mit diesem Feature können Sie Ihre Designabsicht angeben, damit der Compiler sie erzwingen und Optimierungen basierend auf dieser Absicht vornehmen kann. Erfahren Sie mehr über schreibgeschützte Member in dem Artikel [`readonly`](../language-reference/keywords/readonly.md#readonly-member-examples).

## <a name="default-interface-methods"></a>Standardschnittstellenmethoden

Sie können nun Member zu Schnittstellen hinzufügen und eine Implementierung für diese Member bereitstellen. Dieses Sprachfeature ermöglicht es API-Autoren, in späteren Versionen Methoden zu einer Schnittstelle hinzuzufügen, ohne die Quell- oder Binärkompatibilität mit bestehenden Implementierungen dieser Schnittstelle zu beeinträchtigen. Bestehende Implementierungen *erben* die Standardimplementierung. Dieses Feature ermöglicht zudem die Interaktion zwischen C# und APIs, die auf Android oder Swift abzielen und ähnliche Funktionen unterstützen. Standardschnittstellenmethoden ermöglichen auch Szenarien, die einem „Traits“-Sprachfeature ähneln.

Standardschnittstellenmethoden wirken sich auf viele Szenarien und Sprachelemente aus. Unser erstes Tutorial behandelt [die Aktualisierung einer Schnittstelle mit Standardimplementierungen](../tutorials/default-interface-methods-versions.md). Weitere Tutorials und Referenzaktualisierungen folgen rechtzeitig zur allgemeinen Veröffentlichung.

## <a name="more-patterns-in-more-places"></a>Weitere Muster an mehr Orten

**Musterabgleich** bietet Tools zur Bereitstellung formabhängiger Funktionalität für verwandte, aber unterschiedliche Datentypen. C# 7.0 führte eine Syntax für Typmuster und konstante Muster ein, indem der Ausdruck [`is`](../language-reference/keywords/is.md) und die Anweisung [`switch`](../language-reference/keywords/switch.md) verwendet wurden. Diese Funktionen stellten die ersten vorläufigen Schritte zur Unterstützung von Programmierparadigmen dar, bei denen Daten und Funktionalität getrennt voneinander sind. Da sich die Branche immer mehr auf Mikroservices und andere Cloud-basierte Architekturen konzentriert, werden andere Sprachtools benötigt.

C# 8.0 erweitert dieses Vokabular, sodass Sie mehr Musterausdrücke an mehreren Stellen in Ihrem Code verwenden können. Berücksichtigen Sie diese Funktionen, wenn Ihre Daten und Funktionen getrennt sind. Berücksichtigen Sie den Musterabgleich, wenn Ihre Algorithmen von einer anderen Tatsache als dem Runtimetyp eines Objekts abhängen. Diese Verfahren bieten eine weitere Möglichkeit, Entwürfe auszudrücken.

Zusätzlich zu neuen Mustern an neuen Orten fügt C# 8.0 **rekursive Muster** hinzu. Das Ergebnis eines beliebigen Musterausdrucks ist ein Ausdruck. Ein rekursives Muster ist einfach ein Musterausdruck, der auf die Ausgabe eines anderen Musterausdrucks angewendet wird.

### <a name="switch-expressions"></a>Switch-Ausdrücke

Häufig liefert eine [`switch`](../language-reference/keywords/switch.md)-Anweisung in jedem ihrer `case`-Blöcke einen Wert. Mit **switch-Ausdrücken** können Sie eine präzisere Ausdruckssyntax verwenden. Es gibt weniger repetitive `case`- und `break`-Schlüsselwörter und weniger geschweifte Klammern.  Betrachten Sie als Beispiel die folgende Enumeration, die die Farben des Regenbogens enumeriert:

```csharp
public enum Rainbow
{
    Red,
    Orange,
    Yellow,
    Green,
    Blue,
    Indigo,
    Violet
}
```

Wenn Ihre Anwendung einen `RGBColor`-Typ definiert hat, der aus den Komponenten `R`, `G` und `B` aufgebaut ist, können Sie einen `Rainbow`-Wert in seine RGB-Werte konvertieren, indem Sie die folgende Methode verwenden, die einen Switch-Ausdruck enthält:

```csharp
public static RGBColor FromRainbow(Rainbow colorBand) =>
    colorBand switch
    {
        Rainbow.Red    => new RGBColor(0xFF, 0x00, 0x00),
        Rainbow.Orange => new RGBColor(0xFF, 0x7F, 0x00),
        Rainbow.Yellow => new RGBColor(0xFF, 0xFF, 0x00),
        Rainbow.Green  => new RGBColor(0x00, 0xFF, 0x00),
        Rainbow.Blue   => new RGBColor(0x00, 0x00, 0xFF),
        Rainbow.Indigo => new RGBColor(0x4B, 0x00, 0x82),
        Rainbow.Violet => new RGBColor(0x94, 0x00, 0xD3),
        _              => throw new ArgumentException(message: "invalid enum value", paramName: nameof(colorBand)),
    };
```

Es gibt hier verschiedene Syntaxverbesserungen:

- Die Variable steht vor dem `switch`-Schlüsselwort. Die unterschiedliche Reihenfolge macht es optisch einfach, den switch-Ausdruck von der switch-Anweisung zu unterscheiden.
- Die `case`- und `:`-Elemente werden durch `=>` ersetzt. Es ist präziser und intuitiver.
- Der `default`-Fall wird durch eine `_`-Ausschlussvariable ersetzt.
- Die Textkörper sind Ausdrücke, keine Anweisungen.

Stellen Sie dies mit dem entsprechenden Code unter Verwendung der klassischen `switch`-Anweisung gegenüber:

```csharp
public static RGBColor FromRainbowClassic(Rainbow colorBand)
{
    switch (colorBand)
    {
        case Rainbow.Red:
            return new RGBColor(0xFF, 0x00, 0x00);
        case Rainbow.Orange:
            return new RGBColor(0xFF, 0x7F, 0x00);
        case Rainbow.Yellow:
            return new RGBColor(0xFF, 0xFF, 0x00);
        case Rainbow.Green:
            return new RGBColor(0x00, 0xFF, 0x00);
        case Rainbow.Blue:
            return new RGBColor(0x00, 0x00, 0xFF);
        case Rainbow.Indigo:
            return new RGBColor(0x4B, 0x00, 0x82);
        case Rainbow.Violet:
            return new RGBColor(0x94, 0x00, 0xD3);
        default:
            throw new ArgumentException(message: "invalid enum value", paramName: nameof(colorBand));
    };
}
```

### <a name="property-patterns"></a>Eigenschaftsmuster

Mit dem **Eigenschaftsmuster** können Sie die Eigenschaften des untersuchten Objekts anpassen. Denken Sie an eine eCommerce-Website, die die Umsatzsteuer auf der Grundlage der Adresse des Käufers berechnen muss. Diese Berechnung ist keine Kernaufgabe einer `Address`-Klasse. Sie wird sich im Laufe der Zeit ändern, wahrscheinlich häufiger als Adressformatänderungen. Die Höhe der Umsatzsteuer hängt von der `State`-Eigenschaft der Adresse ab. Die folgende Methode verwendet das Eigenschaftsmuster, um die Umsatzsteuer aus der Adresse und dem Preis zu berechnen:

```csharp
public static decimal ComputeSalesTax(Address location, decimal salePrice) =>
    location switch
    {
        { State: "WA" } => salePrice * 0.06M,
        { State: "MN" } => salePrice * 0.75M,
        { State: "MI" } => salePrice * 0.05M,
        // other cases removed for brevity...
        _ => 0M
    };
```

Ein Musterabgleich erstellt eine präzise Syntax,. um diesen Algorithmus auszudrücken.

### <a name="tuple-patterns"></a>Tupelmuster

Einige Algorithmen sind von mehreren Eingaben abhängig. **Tupelmuster** erlauben Ihnen, auf Grundlage mehrerer Werte, ausgedrückt als ein [Tupel](../tuples.md), zu wechseln („switch“).  Der folgende Code zeigt einen switch-Ausdruck für das Spiel *Stein, Schere, Papier* (Schnick, Schnack, Schnuck):

```csharp
public static string RockPaperScissors(string first, string second)
    => (first, second) switch
    {
        ("rock", "paper") => "rock is covered by paper. Paper wins.",
        ("rock", "scissors") => "rock breaks scissors. Rock wins.",
        ("paper", "rock") => "paper covers rock. Paper wins.",
        ("paper", "scissors") => "paper is cut by scissors. Scissors wins.",
        ("scissors", "rock") => "scissors is broken by rock. Rock wins.",
        ("scissors", "paper") => "scissors cuts paper. Scissors wins.",
        (_, _) => "tie"
    };
```

Die Meldungen zeigen den Gewinner an. Der Fall „Verwerfen“ („case discard“) stellt die drei Kombinationen für Unentschieden oder andere Texteingaben dar.

### <a name="positional-patterns"></a>Positionsmuster

Einige Typen umfassen eine `Deconstruct`-Methode, die ihre Eigenschaften in diskrete Variablen dekonstruiert. Wenn auf eine `Deconstruct`-Methode zugegriffen werden kann, können Sie **Positionsmuster** verwenden, um Eigenschaften des Objekts zu untersuchen, und diese Eigenschaften für ein Muster verwenden.  Beachten Sie die folgende `Point`-Klasse, die eine `Deconstruct`-Methode umfasst, um diskrete Variablen für `X` und `Y` zu erstellen:

```csharp
public class Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int x, int y) => (X, Y) = (x, y);

    public void Deconstruct(out int x, out int y) =>
        (x, y) = (X, Y);
}
```

Beachten Sie zudem die folgende Enumeration, die verschiedene Positionen eines Quadranten darstellt:

```csharp
public enum Quadrant
{
    Unknown,
    Origin,
    One,
    Two,
    Three,
    Four,
    OnBorder
}
```

Die folgende Methode verwendet das **Positionsmuster**, um die Werte von `x` und `y` zu extrahieren. Dann wird mit einer `when`-Klausel der `Quadrant` des Punktes bestimmt:

```csharp
static Quadrant GetQuadrant(Point point) => point switch
{
    (0, 0) => Quadrant.Origin,
    var (x, y) when x > 0 && y > 0 => Quadrant.One,
    var (x, y) when x < 0 && y > 0 => Quadrant.Two,
    var (x, y) when x < 0 && y < 0 => Quadrant.Three,
    var (x, y) when x > 0 && y < 0 => Quadrant.Four,
    var (_, _) => Quadrant.OnBorder,
    _ => Quadrant.Unknown
};
```

Das Ausschlussmuster im vorherigen Switch stimmt überein, wenn entweder `x` oder `y` 0 ist, jedoch nicht beide. Ein switch-Ausdruck muss entweder einen Wert erzeugen oder eine Ausnahme auslösen. Wenn keiner der Fälle übereinstimmt, löst der switch-Ausdruck eine Ausnahme aus. Der Compiler erzeugt für Sie eine Warnung, wenn Sie nicht alle möglichen Fälle in Ihrem Switch-Ausdruck abdecken.

In diesem [erweiterten Tutorial zum Musterabgleich](../tutorials/pattern-matching.md) erhalten Sie weitere Informationen zu Musterabgleichverfahren.

## <a name="using-declarations"></a>Using-Deklarationen

Eine **using-Deklaration** ist eine Variablendeklaration, der das Schlüsselwort `using` vorangestellt ist. Es teilt dem Compiler mit, dass die zu deklarierende Variable am Ende des umschließenden Bereichs angeordnet werden soll. Sehen Sie sich beispielsweise den folgenden Code an, der eine Textdatei schreibt:

```csharp
static int WriteLinesToFile(IEnumerable<string> lines)
{
    using var file = new System.IO.StreamWriter("WriteLines2.txt");
    // Notice how we declare skippedLines after the using statement.
    int skippedLines = 0;
    foreach (string line in lines)
    {
        if (!line.Contains("Second"))
        {
            file.WriteLine(line);
        }
        else
        {
            skippedLines++;
        }
    }
    // Notice how skippedLines is in scope here.
    return skippedLines;
    // file is disposed here
}
```

Im vorhergehenden Beispiel wird die Datei angeordnet, wenn die schließende Klammer für die Methode erreicht ist. Dies ist das Ende des Bereichs, in dem `file` deklariert wird. Der obige Code entspricht dem folgenden Code mit klassischer [using-Anweisung](../language-reference/keywords/using-statement.md):

```csharp
static int WriteLinesToFile(IEnumerable<string> lines)
{
    // We must declare the variable outside of the using block
    // so that it is in scope to be returned.
    int skippedLines = 0;
    using (var file = new System.IO.StreamWriter("WriteLines2.txt"))
    {
        foreach (string line in lines)
        {
            if (!line.Contains("Second"))
            {
                file.WriteLine(line);
            }
            else
            {
                skippedLines++;
            }
        }
    } // file is disposed here
    return skippedLines;
}
```

Im vorhergehenden Beispiel wird die Datei angeordnet, wenn die der `using`-Anweisung zugeordnete schließende Klammer erreicht ist.

In beiden Fällen generiert der Compiler den Aufruf von `Dispose()`. Der Compiler erzeugt einen Fehler, wenn der Ausdruck in der `using`-Anweisung nicht verwerfbar ist.

## <a name="static-local-functions"></a>Statische lokale Funktionen

Sie können nun den `static`-Modifikator zu lokalen Funktionen hinzufügen, um sicherzustellen, dass die lokale Funktion keine Variablen aus dem umschließenden Bereich erfasst (referenziert). Dadurch wird `CS8421` erzeugt: „Eine statische lokale Funktion kann keine Referenz auf \<variable> enthalten.“ 

Betrachten Sie folgenden Code. Die lokale Funktion `LocalFunction` greift auf die Variable `y` zu, die im umschließenden Bereich (die Methode `M`) deklariert ist. Daher kann `LocalFunction` nicht mit dem `static`-Modifikator deklariert werden:

```csharp
int M()
{
    int y;
    LocalFunction();
    return y;

    void LocalFunction() => y = 0;
}
```

Der folgende Code enthält eine statische lokale Funktion. Er kann statisch sein, da er nicht auf Variablen im umschließenden Bereich zugreift:

```csharp
int M()
{
    int y = 5;
    int x = 7;
    return Add(x, y);

    static int Add(int left, int right) => left + right;
}
```

## <a name="disposable-ref-structs"></a>Verwerfbare Referenzstrukturen

Ein mit dem `ref`-Modifizierer deklarierter `struct` darf keine Schnittstellen und damit auch keine <xref:System.IDisposable> implementieren. Aus diesem Grund Aktivieren einer `ref struct` um verworfen werden, muss eine zugängliche `void Dispose()` Methode. Dieses Feature gilt auch für `readonly ref struct`-Deklarationen.

## <a name="nullable-reference-types"></a>Nullwerte zulassende Verweistypen

In einem Nullwerte zulassenden Anmerkungskontext, wird jede Variable eines Referenztyps als **keine Nullwerte zulassender Referenztyp** betrachtet. Wenn Sie angeben möchten, dass eine Variable Null sein kann, müssen Sie den Typnamen mit `?` ergänzen, um die Variable als **keine Nullwerte zulassenden Verweistyp** zu deklarieren.

Wenn der Verweistyp keine Nullwerte zulässt, verwendet der Compiler die Flussanalyse, um sicherzustellen, dass lokale Variablen bei der Deklaration auf einen Nicht-Null-Wert initialisiert werden. Felder müssen während der Erstellung initialisiert werden. Der Compiler erzeugt eine Warnung, wenn die Variable nicht durch einen Aufruf eines der verfügbaren Konstruktoren oder durch einen Initialisierer festgelegt wird. Darüber hinaus kann Verweistypen, die keine Nullwerte zulassen, kein Wert zugewiesen werden, der Null sein könnte.

Verweistypen, die keine Nullwerte zulassen werden nicht überprüft, um sicherzustellen, dass sie nicht mit Null belegt oder initialisiert werden. Der Compiler verwendet jedoch die Flussanalyse, um sicherzustellen, dass jede Variable eines Verweistypen, der Nullwerte zulässt, gegen null geprüft wird, bevor auf sie zugegriffen oder einem nicht Verweistypen zugewiesen wird, der Nullwerte zulässt.

Mehr über die Funktion erfahren Sie in der Übersicht der [Verweistypen, die Nullwerte zulassen](../nullable-references.md). Probieren Sie es selbst in einer neuen Anwendung in diesem [Tutorial für Verweistypen, die Nullwerte zulassen](../tutorials/nullable-reference-types.md) aus. Weitere Informationen über die Schritte zur Migration einer bestehenden Codebasis, um Verweistypen zu nutzen, die Nullwerte zulassen, finden Sie im Tutorial [Migration einer Anwendung zur Verwendung Nullwerte zulassenden Verweistypen](../tutorials/upgrade-to-nullable-references.md).

## <a name="asynchronous-streams"></a>Asynchrone Streams

Ab C# 8.0 können Sie Streams asynchron erstellen und nutzen. Eine Methode, die einen asynchronen Stream zurückgibt, hat drei Eigenschaften:

1. Die Deklaration erfolgte mit dem `async`-Modifikator.
1. Es wird <xref:System.Collections.Generic.IAsyncEnumerable%601> zurückgegeben.
1. Das Verfahren enthält `yield return`-Anweisungen, um aufeinanderfolgende Elemente im asynchronen Stream zurückzugeben.

Die Verwendung eines asynchronen Streams erfordert, dass Sie das Schlüsselwort `await` vor dem Schlüsselwort `foreach` hinzufügen, wenn Sie die Elemente des Streams auflisten. Das Hinzufügen des Schlüsselwortes `await` erfordert, dass die Methode, die den asynchronen Strom enumiert, mit dem Modifikator `async` deklariert wird und einen für eine `async`-Methode zulässigen Typ zurückgibt. In der Regel ist dies die Rückgabe von <xref:System.Threading.Tasks.Task> oder <xref:System.Threading.Tasks.Task%601>. Es kann auch <xref:System.Threading.Tasks.ValueTask> oder <xref:System.Threading.Tasks.ValueTask%601> sein. Eine Methode kann einen asynchronen Stream sowohl verwenden als auch erzeugen, was bedeutet, dass sie <xref:System.Collections.Generic.IAsyncEnumerable%601> zurückgeben würde. Der folgende Code erzeugt eine Sequenz von 0 bis 19 und wartet 100 ms zwischen der Generierung jeder Zahl:

```csharp
public static async System.Collections.Generic.IAsyncEnumerable<int> GenerateSequence()
{
    for (int i = 0; i < 20; i++)
    {
        await Task.Delay(100);
        yield return i;
    }
}
```

Die Enumeration der Sequenz erfolgt mit der `await foreach`-Anweisung:

```csharp
await foreach (var number in GenerateSequence())
{
    Console.WriteLine(number);
}
```

Sie können asynchrone Streams selbst in unserem Tutorial zum [Erstellen und Verwenden von asynchronen Streams](../tutorials/generate-consume-asynchronous-stream.md) ausprobieren.

## <a name="indices-and-ranges"></a>Indizes und Bereiche

Indizes und Bereiche bieten eine prägnante Syntax für den Zugriff auf einzelne Elemente oder Bereiche in einer Sequenz.

Diese Sprachunterstützung basiert auf zwei neuen Typen und zwei neuen Operatoren:

- <xref:System.Index?displayProperty=nameWithType>: Stellt einen Index in einer Sequenz dar.
- Der Index vom Endeoperator `^`, der angibt, dass ein Index relativ zum Ende der Sequenz ist.
- <xref:System.Range?displayProperty=nameWithType>: Stellt einen Unterbereich einer Sequenz dar.
- Der Bereichsoperator `..`, der den Beginn und das Ende eines Bereichs als seine Operanden angibt.

Beginnen wir mit den Regeln für Indizes. Betrachten Sie einen Array `sequence`. Der `0`-Index entspricht `sequence[0]`. Der `^0`-Index entspricht `sequence[sequence.Length]`. Beachten Sie, dass `sequence[^0]` genau wie `sequence[sequence.Length]` eine Ausnahme auslöst. Für eine beliebige Zahl `n` ist der Index `^n` identisch mit `sequence.Length - n`.

Ein Bereich gibt den *Beginn* und das *Ende* eines Bereichs an. Der Beginn des Bereichs ist inklusiv, das Ende des Bereichs ist jedoch exklusiv. Das bedeutet, dass der *Beginn* im Bereich enthalten ist, das *Ende* aber nicht. Der Bereich `[0..^0]` stellt ebenso wie `[0..sequence.Length]` den gesamten Bereich dar.

Schauen wir uns einige Beispiele an. Betrachten Sie das folgende Array, kommentiert mit seinem Index „from the start“ und „from the end“:

```csharp
var words = new string[]
{
                // index from start    index from end
    "The",      // 0                   ^9
    "quick",    // 1                   ^8
    "brown",    // 2                   ^7
    "fox",      // 3                   ^6
    "jumped",   // 4                   ^5
    "over",     // 5                   ^4
    "the",      // 6                   ^3
    "lazy",     // 7                   ^2
    "dog"       // 8                   ^1
};              // 9 (or words.Length) ^0
```

Sie können das letzte Wort mit dem `^1`-Index abrufen:

```csharp
Console.WriteLine($"The last word is {words[^1]}");
// writes "dog"
```

Der folgende Code erzeugt einen Teilbereich mit den Worten „quick“, „brown“ und „fox“. Er enthält `words[1]` bis `words[3]`. Das Element `words[4]` befindet sich nicht im Bereich.

```csharp
var quickBrownFox = words[1..4];
```

Der folgende Code erzeugt einen Teilbereich mit „lazy“ und „dog“. Dazu gehören `words[^2]` und `words[^1]`. Der Endindex `words[^0]` ist nicht enthalten:

```csharp
var lazyDog = words[^2..^0];
```

Die folgenden Beispiele erstellen Bereiche, die am Anfang, am Ende und auf beiden Seiten offen sind:

```csharp
var allWords = words[..]; // contains "The" through "dog".
var firstPhrase = words[..4]; // contains "The" through "fox"
var lastPhrase = words[6..]; // contains "the", "lazy" and "dog"
```

Sie können Bereiche auch als Variablen deklarieren:

```csharp
Range phrase = 1..4;
```

Der Bereich kann dann innerhalb der Zeichen `[` und `]` verwendet werden:

```csharp
var text = words[phrase];
```

Indizes und Bereiche werden nicht nur von Arrays unterstützt. Indizes und Bereiche können auch mit [string](../language-reference/builtin-types/reference-types.md#the-string-type), <xref:System.Span%601> oder <xref:System.ReadOnlySpan%601> verwendet werden. Weitere Informationen finden Sie unter [Typunterstützung für Indizes und Bereiche](../tutorials/ranges-indexes.md#type-support-for-indices-and-ranges).

Weitere Informationen zu Indizes und Bereichen finden Sie im Tutorial zu [Indizes und Bereichen](../tutorials/ranges-indexes.md).

## <a name="null-coalescing-assignment"></a>NULL-Coalescing-Zuweisung

In C# 8.0 wird der NULL-Coalescing-Zuweisungsoperator `??=` eingeführt. Sie können den `??=`-Operator verwenden, um den Wert des rechten Operanden dem linken Operanden nur dann zuzuweisen, wenn die Auswertung des linken Operanden `null` ergibt.

```csharp
List<int> numbers = null;
int? i = null;

numbers ??= new List<int>();
numbers.Add(i ??= 17);
numbers.Add(i ??= 20);

Console.WriteLine(string.Join(" ", numbers));  // output: 17 17
Console.WriteLine(i);  // output: 17
```

Weitere Informationen finden Sie im Artikel zu den Operatoren [?? und ??=](../language-reference/operators/null-coalescing-operator.md).

## <a name="unmanaged-constructed-types"></a>Nicht verwaltete konstruierte Typen

In C# 7.3 und früher darf ein konstruierter Typ (also ein Typ, der mindestens ein Typargument enthält) kein [nicht verwalteter Typ](../language-reference/builtin-types/unmanaged-types.md) sein. Ab C# 8.0 ist ein konstruierter Werttyp nicht verwaltet, wenn er nur Felder von nicht verwalteten Typen enthält.

Ein Beispiel: Ihr Code enthält die folgende Definition des generischen `Coords<T>`-Typs:

```csharp
public struct Coords<T>
{
    public T X;
    public T Y;
}
```

Dann ist der `Coords<int>`-Typ in C# 8.0 und höher ein nicht verwalteter Typ. Wie bei allen nicht verwalteten Typen können Sie einen Zeiger auf eine Variable dieses Typs erstellen oder Instanzen dieses Typs einen [Arbeitsspeicherblock im Stapel zuordnen](../language-reference/operators/stackalloc.md):

```csharp
Span<Coords<int>> coordinates = stackalloc[]
{
    new Coords<int> { X = 0, Y = 0 },
    new Coords<int> { X = 0, Y = 3 },
    new Coords<int> { X = 4, Y = 0 }
};
```

Weitere Informationen finden Sie unter [Nicht verwaltete Typen](../language-reference/builtin-types/unmanaged-types.md).

## <a name="stackalloc-in-nested-expressions"></a>Stackalloc in geschachtelten Ausdrücken

Ab C# 8.0 können Sie, wenn das Ergebnis eines [stackalloc](../language-reference/operators/stackalloc.md)-Ausdrucks vom Typ <xref:System.Span%601?displayProperty=nameWithType> oder <xref:System.ReadOnlySpan%601?displayProperty=nameWithType> ist, den `stackalloc`-Ausdruck in anderen Ausdrücken verwenden:

```csharp
Span<int> numbers = stackalloc[] { 1, 2, 3, 4, 5, 6 };
var ind = numbers.IndexOfAny(stackalloc[] { 2, 4, 6 ,8 });
Console.WriteLine(ind);  // output: 1
```

## <a name="enhancement-of-interpolated-verbatim-strings"></a>Erweiterung von interpolierten ausführlichen Zeichenfolgen

`$`- und `@`-Token in [interpolierten](../language-reference/tokens/interpolated.md) ausführlichen Zeichenfolgen können in beliebiger Reihenfolge vorliegen: sowohl `$@"..."` als auch `@$"..."` sind gültige interpolierte ausführliche Zeichenfolgen. In früheren C#-Versionen musste das Token `$` vor dem Token `@` vorhanden sein.
