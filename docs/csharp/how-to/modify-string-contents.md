---
title: 'Vorgehensweise: Ändern von Zeichenfolgeninhalten – C#-Leitfaden'
ms.date: 02/26/2018
helpviewer_keywords:
- strings [C#], modifying
ms.openlocfilehash: 539e313173d46c2c92399cefe94207c8beed03b4
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73973258"
---
# <a name="how-to-modify-string-contents-in-c"></a>Vorgehensweise: Ändern von Zeichenfolgeninhalten in C\#

In diesem Artikel werden verschiedene Methoden zum Erzeugen einer `string` durch Modifizieren einer vorhandenen `string` erläutert. Alle diese gezeigten Methoden geben das Ergebnis der Modifizierung als neues `string`-Objekt zurück. Um dies deutlich zu machen, wird das Ergebnis in den Beispielen als neue Variable gespeichert. Dann können Sie sich sowohl die ursprüngliche `string` als auch die `string` ansehen, die durch die Modifizierung beim Ausführen jedes Beispiels entsteht.

[!INCLUDE[interactive-note](~/includes/csharp-interactive-note.md)]

In diesem Artikel werden mehrere Methoden aufgezeigt. Sie können vorhandenen Text ersetzen. Sie können nach Mustern suchen und übereinstimmenden Text durch anderen Text ersetzen. Sie können Zeichenfolgen als Zeichensequenzen behandeln. Sie können zudem Hilfsmethoden verwenden, die Leerräume entfernen. Sie sollten sich für die Methoden entscheiden, die am besten zu Ihrem Szenario passen.

## <a name="replace-text"></a>Ersetzen von Text

Der folgende Code erstellt eine neue Zeichenfolge, indem er vorhandenen Text ersetzt.

[!code-csharp-interactive[replace creates a new string](../../../samples/snippets/csharp/how-to/strings/ModifyStrings.cs#1)]

Der vorherige Code veranschaulicht diese *unveränderliche* Eigenschaft von Zeichenfolgen. Im vorherigen Beispiel können Sie sehen, dass die ursprüngliche Zeichenfolge `source` nicht modifiziert wird. Die Methode <xref:System.String.Replace%2A?displayProperty=nameWithType> erstellt eine neue `string`, die die Modifizierungen enthält.

Die Methode <xref:System.String.Replace%2A> kann entweder Zeichenfolgen oder einzelne Zeichen ersetzen. In beiden Fällen wird jedes Vorkommen des gesuchten Texts ersetzt.  In folgendem Beispiel werden alle „ “-Zeichen durch \_ ersetzt:

[!code-csharp-interactive[replace characters](../../../samples/snippets/csharp/how-to/strings/ModifyStrings.cs#2)]

Die Quellzeichenfolge bleibt unverändert, und es wird eine neue Zeichenfolge mit der Ersetzung zurückgegeben.

## <a name="trim-white-space"></a>Entfernen von Leerräumen

Sie können die Methoden <xref:System.String.Trim%2A?displayProperty=nameWithType>, <xref:System.String.TrimStart%2A?displayProperty=nameWithType> und <xref:System.String.TrimEnd%2A?displayProperty=nameWithType> verwenden, um führende oder nachfolgende Leerräume zu entfernen.  Im folgenden Code ist ein Beispiel für jede Methode dargestellt. Die Quellzeichenfolge bleibt unverändert. Diese Methoden geben eine neue Zeichenfolge mit modifiziertem Inhalt zurück.

[!code-csharp-interactive[trim white space](../../../samples/snippets/csharp/how-to/strings/ModifyStrings.cs#3)]

## <a name="remove-text"></a>Entfernen von Text

Mit der Methode <xref:System.String.Remove%2A?displayProperty=nameWithType> können Sie Text aus einer Zeichenfolge entfernen. Diese Methode entfernt mehrere Zeichen ab einem spezifischen Index. In folgendem Beispiel wird gezeigt, wie Sie <xref:System.String.IndexOf%2A?displayProperty=nameWithType> gefolgt von <xref:System.String.Remove%2A> verwenden können, um Text aus einer Zeichenfolge zu entfernen:

[!code-csharp-interactive[remove text](../../../samples/snippets/csharp/how-to/strings/ModifyStrings.cs#4)]

## <a name="replace-matching-patterns"></a>Ersetzen von übereinstimmenden Mustern

Sie können [reguläre Ausdrücke](../../standard/base-types/regular-expressions.md) verwenden, um Text, der mit Mustern übereinstimmt, durch neuen Text zu ersetzen, der auch durch ein Muster definiert werden kann. In folgendem Beispiel wird die Klasse <xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType> verwendet, um ein Muster in einer Quellzeichenfolge zu finden und dieses durch Text mit korrekter Großschreibung zu ersetzen. Die Methode <xref:System.Text.RegularExpressions.Regex.Replace(System.String,System.String,System.Text.RegularExpressions.MatchEvaluator,System.Text.RegularExpressions.RegexOptions)?displayProperty=nameWithType> akzeptiert eine Funktion, die die Logik der Ersetzung als eines ihrer Argumente bereitstellt. In diesem Beispiel ist diese Funktion `LocalReplaceMatchCase` eine **lokale Funktion**, die in der Beispielmethode deklariert wird. `LocalReplaceMatchCase` verwendet die <xref:System.Text.StringBuilder?displayProperty=nameWithType>-Klasse zum Erstellen der Ersatzzeichenfolge mit korrekter Großschreibung.

Reguläre Ausdrücke sind besonders beim Suchen und Ersetzen von Text nützlich, der einem bestimmten Muster folgt, und nicht so sehr bei bekanntem Text. Weitere Informationen finden Sie unter [Vorgehensweise: Durchsuchen von Zeichenfolgen](search-strings.md). Das Suchmuster „the\s“ sucht nach dem Wort „the“ gefolgt von einem Leerzeichen. Der Teil des Musters stellt sicher, das es nicht „there“ als Übereinstimmung in der Quellzeichenfolge ansieht. Weitere Informationen zur Sprache für reguläre Ausdrücke finden Sie unter [Sprachelemente für reguläre Ausdrücke – Kurzübersicht](../../standard/base-types/regular-expression-language-quick-reference.md).

[!code-csharp-interactive[replace creates a new string](../../../samples/snippets/csharp/how-to/strings/ModifyStrings.cs#5)]

Die Methode <xref:System.Text.StringBuilder.ToString%2A?displayProperty=nameWithType> gibt eine unveränderliche Zeichenfolge zurück, die den Inhalt im <xref:System.Text.StringBuilder>-Objekt enthält.

## <a name="modifying-individual-characters"></a>Modifizieren einzelner Zeichen

Sie können ein Zeichenarray aus einer Zeichenfolge erzeugen, den Inhalt des Arrays modifizieren und dann eine neue Zeichenfolge aus dem modifizierten Inhalt des Arrays erstellen.

In folgendem Beispiel wird gezeigt, wie Sie mehrere Zeichen in einer Zeichenfolge ersetzen. Zunächst wird die Methode <xref:System.String.ToCharArray?displayProperty=nameWithName> verwendet, um ein Zeichenarray zu erstellen. Die Methode <xref:System.String.IndexOf%2A> wird verwendet, um den Startindex des Worts „fox“ zu finden. Die folgenden drei Zeichen werden durch ein anderes Wort ersetzt. Zum Schluss wird eine neue Zeichenfolge aus dem aktualisierten Zeichenarray erstellt.

[!code-csharp-interactive[replace creates a new string](../../../samples/snippets/csharp/how-to/strings/ModifyStrings.cs#6)]

## <a name="unsafe-modifications-to-string"></a>Unsichere Modifizierung einer Zeichenfolge

Mit **unsicherem Code** können Sie eine Zeichenfolge „direkt“ modifizieren, nachdem sie erstellt wurde. Unsicherer Code umgeht viele der Features von .NET, die dazu gedacht sind, bestimmte Fehler in Code zu minimieren. Sie müssen unsicheren Code verwenden, um eine Zeichenfolge direkt zu modifizieren, weil die Zeichenfolgenklasse als **unveränderlicher** Typ konzipiert ist. Sobald Sie erstellt wurde, kann ihr Wert nicht verändert werden. Unsicherer Code umgeht diese Eigenschaft, indem er auf den Speicher zugreift und diesen modifiziert, der von einer `string` verwendet wird, ohne normale `string`-Methoden zu verwenden.
Das folgende Beispiel können Sie verwenden, falls Sie eine Zeichenfolge direkt mit unsicherem Code modifizieren möchte. Dies kommt jedoch sehr selten vor. Im folgenden Beispiel wird die Verwendung des Schlüsselworts `fixed` veranschaulicht. Das Schlüsselwort `fixed` verhindert, dass der Garbage Collector (GC) das Zeichenfolgenobjekt im Speicher verschiebt, während der Code mit einem unsicheren Zeiger auf den Speicher zugreift. Darüber hinaus wird ein möglicher Nebeneffekt unsicherer Vorgänge auf Zeichenfolgen gezeigt, die daraus entstehen, dass der C#-Compiler Zeichenfolgen intern speichert (hält). Im Allgemeinen sollten Sie dieses Verfahren nicht verwenden, es sei denn, es ist unbedingt notwendig. Weitere Informationen erhalten Sie in den Artikeln zu [unsafe](../language-reference/keywords/unsafe.md) und [fixed](../language-reference/keywords/fixed-statement.md). Die API-Referenz für <xref:System.String.Intern%2A> enthält Informationen zum Internalisieren von Zeichenfolgen.

[!code-csharp[unsafe ways to create a new string](../../../samples/snippets/csharp/how-to/strings/ModifyStrings.cs#7)]

Sie können diese Beispiele ausprobieren, indem Sie sich den Code in unserem [GitHub-Repository](https://github.com/dotnet/samples/tree/master/snippets/csharp/how-to/strings) ansehen. Alternativ dazu können Sie die Beispiele [als ZIP-Datei](https://github.com/dotnet/samples/raw/master/snippets/csharp/how-to/strings.zip) herunterladen.

## <a name="see-also"></a>Siehe auch

- [Reguläre Ausdrücke von .NET Framework](../../standard/base-types/regular-expressions.md)
- [Sprachelemente für reguläre Ausdrücke – Kurzübersicht](../../standard/base-types/regular-expression-language-quick-reference.md)
