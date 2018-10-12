---
title: Bewährte Methoden zum Schreiben von Komponententests
description: Informationen zu den bewährten Methoden zum Schreiben von Komponententests, die die Qualität und Stabilität von Code gewährleisten
author: jpreese
ms.author: wiwagn
ms.date: 07/28/2018
ms.openlocfilehash: 69fe0cae141d1ed1e1281eecd78bf03e6e8be961
ms.sourcegitcommit: 2eceb05f1a5bb261291a1f6a91c5153727ac1c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2018
ms.locfileid: "43527732"
---
# <a name="unit-testing-best-practices"></a><span data-ttu-id="28e8b-103">Bewährte Methoden für Komponententests</span><span class="sxs-lookup"><span data-stu-id="28e8b-103">Unit testing best practices</span></span>

<span data-ttu-id="28e8b-104">Von [John Reese](http://reesespieces.io) mit besonderem Dank an [Roy Osherove](http://osherove.com/)</span><span class="sxs-lookup"><span data-stu-id="28e8b-104">By [John Reese](http://reesespieces.io) with special thanks to [Roy Osherove](http://osherove.com/)</span></span>

<span data-ttu-id="28e8b-105">Komponententests bringen zahlreiche Vorteile mit sich: Sie helfen bei der Regression, stellen Dokumentation bereit und unterstützen Sie beim Entwerfen guten Codes.</span><span class="sxs-lookup"><span data-stu-id="28e8b-105">There are numerous benefits to writing unit tests; they help with regression, provide documentation, and facilitate good design.</span></span> <span data-ttu-id="28e8b-106">Allerdings können Komponententest, die schwierig zu lesen und fehleranfällig sind, zu einer unübersichtlichen Codebasis führen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-106">However, hard to read and brittle unit tests can wreak havoc on your code base.</span></span>

<span data-ttu-id="28e8b-107">In diesem Leitfaden lernen Sie die bewährten Methoden beim Schreiben von Komponententests kennen, mit denen Sie Ihre Tests resilient und leicht verständlich gestalten können.</span><span class="sxs-lookup"><span data-stu-id="28e8b-107">In this guide, you'll learn some best practices when writing unit tests to keep your tests resilient and easy to understand.</span></span>

## <a name="why-unit-test"></a><span data-ttu-id="28e8b-108">Weshalb sollte ich Komponententests durchführen?</span><span class="sxs-lookup"><span data-stu-id="28e8b-108">Why unit test?</span></span>

### <a name="less-time-performing-functional-tests"></a><span data-ttu-id="28e8b-109">Weniger Zeitaufwand für das Ausführen von Funktionstests</span><span class="sxs-lookup"><span data-stu-id="28e8b-109">Less time performing functional tests</span></span>
<span data-ttu-id="28e8b-110">Funktionstests sind kostspielig.</span><span class="sxs-lookup"><span data-stu-id="28e8b-110">Functional tests are expensive.</span></span> <span data-ttu-id="28e8b-111">Für Funktionstests muss normalerweise die Anwendung geöffnet werden, und Sie (oder eine andere Person) müssen mehrere Schritte ausführen, um das erwartete Verhalten zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-111">They typically involve opening up the application and performing a series of steps that you (or someone else), must follow in order to validate the expected behavior.</span></span> <span data-ttu-id="28e8b-112">Es kann durchaus sein, dass der Tester diese Schritte nicht kennt, was bedeutet, dass sich dieser an eine Person wenden muss, die sich in dem Bereich auskennt, um den Test auszuführen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-112">These steps may not always be known to the tester, which means they will have to reach out to someone more knowledgeable in the area in order to carry out the test.</span></span> <span data-ttu-id="28e8b-113">Der Testvorgang selbst kann nur ein paar Sekunden für banale Änderungen oder Minuten für größere Änderungen in Anspruch nehmen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-113">Testing itself could take seconds for trivial changes, or minutes for larger changes.</span></span> <span data-ttu-id="28e8b-114">Abschließend muss der ganze Vorgang für jede Änderung, die Sie am System vornehmen, wiederholt werden.</span><span class="sxs-lookup"><span data-stu-id="28e8b-114">Lastly, this process must be repeated for every change that you make in the system.</span></span>

<span data-ttu-id="28e8b-115">Im Vergleich dazu benötigen Komponententests nur Millisekunden. Außerdem können Sie durch Klicken auf eine Schaltfläche ausgeführt werden und erfordern nicht unbedingt tiefgreifende Kenntnisse zum System.</span><span class="sxs-lookup"><span data-stu-id="28e8b-115">Unit tests, on the other hand, take milliseconds, can be run at the press of a button and do not necessarily require any knowledge of the system at large.</span></span> <span data-ttu-id="28e8b-116">Ob der Test erfolgreich ist oder fehlschlägt, hängt vom Testlauf und nicht von der einzelnen Person ab, die den Test ausführt.</span><span class="sxs-lookup"><span data-stu-id="28e8b-116">Whether or not the test passes or fails is up to the test runner, not the individual.</span></span>

### <a name="protection-against-regression"></a><span data-ttu-id="28e8b-117">Schutz vor Regression</span><span class="sxs-lookup"><span data-stu-id="28e8b-117">Protection against regression</span></span>
<span data-ttu-id="28e8b-118">Regressionsfehler sind Fehler, die auftreten, wenn eine Änderung an der Anwendung vorgenommen wird.</span><span class="sxs-lookup"><span data-stu-id="28e8b-118">Regression defects are defects that are introduced when a change is made to the application.</span></span> <span data-ttu-id="28e8b-119">Tester testen meist nicht nur ihr neues Feature sondern auch die Features, die zuvor schon vorhanden waren. So können sie überprüfen, ob die zuvor implementierten Features noch immer wie erwartet funktionieren.</span><span class="sxs-lookup"><span data-stu-id="28e8b-119">It is common for testers to not only test their new feature but also features that existed beforehand in order to verify that previously implemented features still function as expected.</span></span>

<span data-ttu-id="28e8b-120">Durch Komponententests ist es möglich, die gesamte Testsammlung nach jedem Build oder sogar nach jeder Änderung an einer Codezeile erneut durchzuführen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-120">With unit testing, it's possible to rerun your entire suite of tests after every build or even after you change a line of code.</span></span> <span data-ttu-id="28e8b-121">So können Sie sicherstellen, dass Ihr neuer Code die vorhandene Funktionalität nicht beeinträchtigt.</span><span class="sxs-lookup"><span data-stu-id="28e8b-121">Giving you confidence that your new code does not break existing functionality.</span></span>

### <a name="executable-documentation"></a><span data-ttu-id="28e8b-122">Dokumentation zur Ausführbarkeit</span><span class="sxs-lookup"><span data-stu-id="28e8b-122">Executable documentation</span></span>
<span data-ttu-id="28e8b-123">Es ist nicht immer offensichtlich, was genau eine bestimmte Methode tut oder wie sie sich bei einer bestimmten Eingabe verhält.</span><span class="sxs-lookup"><span data-stu-id="28e8b-123">It may not always be obvious what a particular method does or how it behaves given a certain input.</span></span> <span data-ttu-id="28e8b-124">Sie fragen sich vielleicht: Wie verhält sich diese Methode, wenn ich ihr eine leere Zeichenfolge übergebe?</span><span class="sxs-lookup"><span data-stu-id="28e8b-124">You may ask yourself: How does this method behave if I pass it a blank string?</span></span> <span data-ttu-id="28e8b-125">Oder was geschieht, wenn ich NULL übergebe?</span><span class="sxs-lookup"><span data-stu-id="28e8b-125">Null?</span></span>

<span data-ttu-id="28e8b-126">Wenn Sie über eine Sammlung von sinnvoll benannten Komponententests verfügen, sollte jeder Test die erwartete Ausgabe für eine gegebene Eingabe eindeutig erklären können.</span><span class="sxs-lookup"><span data-stu-id="28e8b-126">When you have a suite of well-named unit tests, each test should be able to clearly explain the expected output for a given input.</span></span> <span data-ttu-id="28e8b-127">Zusätzlich sollte er überprüfen, ob sie tatsächlich funktioniert.</span><span class="sxs-lookup"><span data-stu-id="28e8b-127">In addition, it should be able to verify that it actually works.</span></span>

### <a name="less-coupled-code"></a><span data-ttu-id="28e8b-128">Weniger gekoppelter Code</span><span class="sxs-lookup"><span data-stu-id="28e8b-128">Less coupled code</span></span>
<span data-ttu-id="28e8b-129">Wenn Code eng gekoppelt ist, kann es schwierig sein, Komponententests dafür auszuführen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-129">When code is tightly coupled, it can be difficult to unit test.</span></span> <span data-ttu-id="28e8b-130">Ohne das Erstellen von Komponententests für den von Ihnen geschriebenen Code kann die Kopplung weniger offensichtlich sein.</span><span class="sxs-lookup"><span data-stu-id="28e8b-130">Without creating unit tests for the code that you're writing, coupling may be less apparent.</span></span>

<span data-ttu-id="28e8b-131">Wenn Sie Tests für Ihren Code schreiben, wird dieser automatisch entkoppelt, da es ansonsten schwieriger wäre, diesen zu testen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-131">Writing tests for your code will naturally decouple your code, because it would be more difficult to test otherwise.</span></span>

## <a name="characteristics-of-a-good-unit-test"></a><span data-ttu-id="28e8b-132">Merkmale eines guten Komponententests</span><span class="sxs-lookup"><span data-stu-id="28e8b-132">Characteristics of a good unit test</span></span>
- <span data-ttu-id="28e8b-133">**Schnelligkeit**.</span><span class="sxs-lookup"><span data-stu-id="28e8b-133">**Fast**.</span></span> <span data-ttu-id="28e8b-134">Es ist nicht ungewöhnlich, dass ausgereifte Projekte Tausende von Komponententests enthalten.</span><span class="sxs-lookup"><span data-stu-id="28e8b-134">It is not uncommon for mature projects to have thousands of unit tests.</span></span> <span data-ttu-id="28e8b-135">Die Ausführung von Komponententests sollte sehr wenig Zeit in Anspruch nehmen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-135">Unit tests should take very little time to run.</span></span> <span data-ttu-id="28e8b-136">Sie sollten innerhalb von Millisekunden abgeschlossen sein.</span><span class="sxs-lookup"><span data-stu-id="28e8b-136">Milliseconds.</span></span>
- <span data-ttu-id="28e8b-137">**Unabhängigkeit**.</span><span class="sxs-lookup"><span data-stu-id="28e8b-137">**Isolated**.</span></span> <span data-ttu-id="28e8b-138">Komponententest sind eigenständig, sie können isoliert ausgeführt werden und verfügen über keine Abhängigkeiten auf externen Faktoren, wie etwa einem Dateisystem oder einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="28e8b-138">Unit tests are standalone, can be run in isolation, and have no dependencies on any outside factors such as a file system or database.</span></span>
- <span data-ttu-id="28e8b-139">**Wiederholbarkeit**.</span><span class="sxs-lookup"><span data-stu-id="28e8b-139">**Repeatable**.</span></span> <span data-ttu-id="28e8b-140">Das Ausführen eines Komponententests sollte immer zu den gleichen Ergebnissen führen. Wenn Sie zwischen den Ausführungen nichts ändern, sollte sich das Ergebnis auch nicht ändern.</span><span class="sxs-lookup"><span data-stu-id="28e8b-140">Running a unit test should be consistent with its results, that is, it always returns the same result if you do not change anything in between runs.</span></span>
- <span data-ttu-id="28e8b-141">**Selbstprüfung**.</span><span class="sxs-lookup"><span data-stu-id="28e8b-141">**Self-Checking**.</span></span> <span data-ttu-id="28e8b-142">Der Test sollte in der Lage sein, automatisch und ohne menschliches Eingreifen zu erkennen, ob er erfolgreich war oder fehlgeschlagen ist.</span><span class="sxs-lookup"><span data-stu-id="28e8b-142">The test should be able to automatically detect if it passed or failed without any human interaction.</span></span>
- <span data-ttu-id="28e8b-143">**Verhältnismäßigkeit**.</span><span class="sxs-lookup"><span data-stu-id="28e8b-143">**Timely**.</span></span> <span data-ttu-id="28e8b-144">Ein Komponententest darf verglichen mit dem zu testenden Code nicht unverhältnismäßig lang für das Schreiben benötigen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-144">A unit test should not take a disproportionally long time to write compared to the code being tested.</span></span> <span data-ttu-id="28e8b-145">Wenn Sie bemerken, dass der Codetest im Vergleich zum Schreiben des Codes viel Zeit in Anspruch nimmt, erwägen Sie einen Entwurf, der besser getestet werden kann.</span><span class="sxs-lookup"><span data-stu-id="28e8b-145">If you find testing the code taking a large amount of time compared to writing the code, consider a design that is more testable.</span></span>

## <a name="lets-speak-the-same-language"></a><span data-ttu-id="28e8b-146">Begriffsklärung</span><span class="sxs-lookup"><span data-stu-id="28e8b-146">Let's speak the same language</span></span>
<span data-ttu-id="28e8b-147">Der Begriff *Mock* ( Deutsch „Pseudo“) wird leider im Testkontext häufig falsch verwendet.</span><span class="sxs-lookup"><span data-stu-id="28e8b-147">The term *mock* is unfortunately very misused when talking about testing.</span></span> <span data-ttu-id="28e8b-148">Im Folgenden werden die häufigsten Arten von unechten Objekten, sogenannten *Fakes*, beim Schreiben von Komponententests definiert:</span><span class="sxs-lookup"><span data-stu-id="28e8b-148">The following defines the most common types of *fakes* when writing unit tests:</span></span>

<span data-ttu-id="28e8b-149">*Fake*: Fake ist ein generischer Begriff, der sowohl für Stubs als auch für Pseudoobjekte („Mocks“) verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="28e8b-149">*Fake* - A fake is a generic term which can be used to describe either a stub or a mock object.</span></span> <span data-ttu-id="28e8b-150">Ob es sich um einen Stub oder einen Mock handelt, hängt vom Kontext ab.</span><span class="sxs-lookup"><span data-stu-id="28e8b-150">Whether it is a stub or a mock depends on the context in which it's used.</span></span> <span data-ttu-id="28e8b-151">Das bedeutet, dass ein Fake entweder ein Stub oder ein Mock sein kann.</span><span class="sxs-lookup"><span data-stu-id="28e8b-151">So in other words, a fake can be a stub or a mock.</span></span>

<span data-ttu-id="28e8b-152">*Mock*: Ein Pseudoobjekt ist ein unechtes Objekt im System, das entscheidet, ob ein Komponententest erfolgreich war oder nicht.</span><span class="sxs-lookup"><span data-stu-id="28e8b-152">*Mock* - A mock object is a fake object in the system that decides whether or not a unit test has passed or failed.</span></span> <span data-ttu-id="28e8b-153">Ein Mock ist zunächst ein Fake, bis eine Assert-Anweisung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="28e8b-153">A mock starts out as a Fake until it is asserted against.</span></span>

<span data-ttu-id="28e8b-154">*Stub*: Ein Stub ist ein anpassbarer Platzhalter für eine vorhandene Abhängigkeit (oder einen Projektmitarbeiter) im System.</span><span class="sxs-lookup"><span data-stu-id="28e8b-154">*Stub* - A stub is a controllable replacement for an existing dependency (or collaborator) in the system.</span></span> <span data-ttu-id="28e8b-155">Wenn Sie einen Stub verwenden, können Sie Ihren Code testen, ohne sich direkt um die Abhängigkeit kümmern zu müssen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-155">By using a stub, you can test your code without dealing with the dependency directly.</span></span> <span data-ttu-id="28e8b-156">Standardmäßig ist ein Fake immer zunächst ein Stub.</span><span class="sxs-lookup"><span data-stu-id="28e8b-156">By default, a fake starts out as a stub.</span></span>

<span data-ttu-id="28e8b-157">Betrachten Sie den folgenden Codeausschnitt:</span><span class="sxs-lookup"><span data-stu-id="28e8b-157">Consider the following code snippet:</span></span>

```csharp
var mockOrder = new MockOrder();
var purchase = new Purchase(mockOrder);

purchase.ValidateOrders();

Assert.True(purchase.CanBeShipped);
```

<span data-ttu-id="28e8b-158">Dies wäre ein Beispiel eines Stubs, der als Mock bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="28e8b-158">This would be an example of stub being referred to as a mock.</span></span> <span data-ttu-id="28e8b-159">In diesem Fall handelt es sich aber um einen Stub.</span><span class="sxs-lookup"><span data-stu-id="28e8b-159">In this case, it is a stub.</span></span> <span data-ttu-id="28e8b-160">Sie übergeben „Order“ nur, um `Purchase` instanziieren zu können (das System, das getestet wird).</span><span class="sxs-lookup"><span data-stu-id="28e8b-160">You're just passing in the Order as a means to be able to instantiate `Purchase` (the system under test).</span></span> <span data-ttu-id="28e8b-161">Die Bezeichnung `MockOrder` ist ebenfalls irreführend, da auch hier Order keinen Mock darstellt.</span><span class="sxs-lookup"><span data-stu-id="28e8b-161">The name `MockOrder` is also very misleading because again, the order is not a mock.</span></span>

<span data-ttu-id="28e8b-162">Dies ist ein besserer Ansatz:</span><span class="sxs-lookup"><span data-stu-id="28e8b-162">A better approach would be</span></span>

```csharp
var stubOrder = new FakeOrder();
var purchase = new Purchase(stubOrder);

purchase.ValidateOrders();

Assert.True(purchase.CanBeShipped);
```

<span data-ttu-id="28e8b-163">Indem die Klasse in `FakeOrder` umbenannt wird, wird sie generischer und kann so als Mock oder Stub verwendet werden,</span><span class="sxs-lookup"><span data-stu-id="28e8b-163">By renaming the class to `FakeOrder`, you've made the class a lot more generic, the class can be used as a mock or a stub.</span></span> <span data-ttu-id="28e8b-164">je nachdem, was für die Testsituation besser geeignet ist.</span><span class="sxs-lookup"><span data-stu-id="28e8b-164">Whichever is better for the test case.</span></span> <span data-ttu-id="28e8b-165">Im Beispiel oben wird `FakeOrder` als Stub verwendet.</span><span class="sxs-lookup"><span data-stu-id="28e8b-165">In the above example, `FakeOrder` is used as a stub.</span></span> <span data-ttu-id="28e8b-166">Sie verwenden `FakeOrder` nicht in der Assert-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="28e8b-166">You're not using the `FakeOrder` in any shape or form during the assert.</span></span> <span data-ttu-id="28e8b-167">`FakeOrder` wurde nur an die `Purchase`-Klasse übergeben, um die Anforderungen des Konstruktors zu erfüllen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-167">`FakeOrder` was just passed into the `Purchase` class to satisfy the requirements of the constructor.</span></span>

<span data-ttu-id="28e8b-168">Um „Purchase“ als Mock verwenden zu können, können Sie z.B. wie folgt vorgehen:</span><span class="sxs-lookup"><span data-stu-id="28e8b-168">To use it as a Mock, you could do something like this</span></span>

```csharp
var mockOrder = new FakeOrder();
var purchase = new Purchase(mockOrder);

purchase.ValidateOrders();

Assert.True(mockOrder.Validated);
```

<span data-ttu-id="28e8b-169">In diesem Fall überprüfen Sie eine Eigenschaft des Fakes (d.h. führen eine Assert-Anweisung dafür aus). Im Codeausschnitt oben ist `mockOrder` also ein Mock.</span><span class="sxs-lookup"><span data-stu-id="28e8b-169">In this case, you are checking a property on the Fake (asserting against it), so in the above code snippet, the `mockOrder` is a Mock.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="28e8b-170">Es ist wichtig, dass die Terminologie richtig ist.</span><span class="sxs-lookup"><span data-stu-id="28e8b-170">It's important to get this terminology correct.</span></span> <span data-ttu-id="28e8b-171">Wenn Sie Ihre Stubs als „Mocks“ bezeichnen, können andere Entwickler Ihre Absicht falsch verstehen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-171">If you call your stubs "mocks", other developers are going to make false assumptions about your intent.</span></span>

<span data-ttu-id="28e8b-172">Der Hauptunterschied zwischen Mocks und Stubs ist also der folgende: Mocks unterscheiden sich im Grunde genommen kaum von Stubs, für diese führen Sie aber eine Assert-Anweisung aus, und für einen Stub nicht.</span><span class="sxs-lookup"><span data-stu-id="28e8b-172">The main thing to remember about mocks versus stubs is that mocks are just like stubs, but you assert against the mock object, whereas you do not assert against a stub.</span></span>

## <a name="best-practices"></a><span data-ttu-id="28e8b-173">Bewährte Methoden</span><span class="sxs-lookup"><span data-stu-id="28e8b-173">Best practices</span></span>

### <a name="naming-your-tests"></a><span data-ttu-id="28e8b-174">Benennen Ihrer Tests</span><span class="sxs-lookup"><span data-stu-id="28e8b-174">Naming your tests</span></span>
<span data-ttu-id="28e8b-175">Der Name Ihres Tests sollte aus drei Teilen bestehen:</span><span class="sxs-lookup"><span data-stu-id="28e8b-175">The name of your test should consist of three parts:</span></span>
- <span data-ttu-id="28e8b-176">aus dem Namen der Methode, die getestet wird</span><span class="sxs-lookup"><span data-stu-id="28e8b-176">The name of the method being tested.</span></span>
- <span data-ttu-id="28e8b-177">aus den Bedingungen, unter denen getestet wird</span><span class="sxs-lookup"><span data-stu-id="28e8b-177">The scenario under which it's being tested.</span></span>
- <span data-ttu-id="28e8b-178">aus dem erwarteten Verhalten, wenn das Szenario aufgerufen wird</span><span class="sxs-lookup"><span data-stu-id="28e8b-178">The expected behavior when the scenario is invoked.</span></span>

#### <a name="why"></a><span data-ttu-id="28e8b-179">Warum?</span><span class="sxs-lookup"><span data-stu-id="28e8b-179">Why?</span></span>
- <span data-ttu-id="28e8b-180">Benennungsstandards sind wichtig, da diese die Absicht des Tests explizit ausdrücken.</span><span class="sxs-lookup"><span data-stu-id="28e8b-180">Naming standards are important because they explicitly express the intent of the test.</span></span>

<span data-ttu-id="28e8b-181">Tests sind nicht nur dazu da sicherzustellen, dass Ihr Code funktioniert, sie bieten auch Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="28e8b-181">Tests are more than just making sure your code works, they also provide documentation.</span></span> <span data-ttu-id="28e8b-182">Sie sollten anhand der Komponententestsammlung in der Lage sein, das Verhalten Ihres Codes abzuleiten, auch wenn Sie nicht den Code selbst ansehen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-182">Just by looking at the suite of unit tests, you should be able to infer the behavior of your code without even looking at the code itself.</span></span> <span data-ttu-id="28e8b-183">Wenn Tests fehlschlagen, können Sie zusätzlich genau erkennen, welche Szenarios nicht Ihren Erwartungen entsprechen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-183">Additionally, when tests fail, you can see exactly which scenarios do not meet your expectations.</span></span>

#### <a name="bad"></a><span data-ttu-id="28e8b-184">Nicht empfohlen:</span><span class="sxs-lookup"><span data-stu-id="28e8b-184">Bad:</span></span>
[!code-csharp[BeforeNaming](../../../samples/csharp/unit-testing-best-practices/before/StringCalculatorTests.cs#BeforeNaming)]

#### <a name="better"></a><span data-ttu-id="28e8b-185">Empfohlen:</span><span class="sxs-lookup"><span data-stu-id="28e8b-185">Better:</span></span>
[!code-csharp[AfterNamingAndMinimallyPassing](../../../samples/csharp/unit-testing-best-practices/after/StringCalculatorTests.cs#AfterNamingAndMinimallyPassing)]

### <a name="arranging-your-tests"></a><span data-ttu-id="28e8b-186">Anordnen Ihrer Tests</span><span class="sxs-lookup"><span data-stu-id="28e8b-186">Arranging your tests</span></span>
<span data-ttu-id="28e8b-187">**Arrange, Act, Assert** ist ein häufiges Muster beim Ausführen von Komponententests.</span><span class="sxs-lookup"><span data-stu-id="28e8b-187">**Arrange, Act, Assert** is a common pattern when unit testing.</span></span> <span data-ttu-id="28e8b-188">Dieses Muster besteht aus drei Hauptteilen:</span><span class="sxs-lookup"><span data-stu-id="28e8b-188">As the name implies, it consists of three main actions:</span></span>
- <span data-ttu-id="28e8b-189">*Arrange*: Ordnen Sie Ihrer Objekte an, und erstellen und planen Sie diese nach Bedarf.</span><span class="sxs-lookup"><span data-stu-id="28e8b-189">*Arrange* your objects, creating and setting them up as necessary.</span></span>
- <span data-ttu-id="28e8b-190">*Act*: Führen Sie eine Aktion für ein Objekt durch.</span><span class="sxs-lookup"><span data-stu-id="28e8b-190">*Act* on an object.</span></span>
- <span data-ttu-id="28e8b-191">*Assert*: Überprüfen Sie die ordnungsgemäße Funktionsweise.</span><span class="sxs-lookup"><span data-stu-id="28e8b-191">*Assert* that something is as expected.</span></span>

#### <a name="why"></a><span data-ttu-id="28e8b-192">Warum?</span><span class="sxs-lookup"><span data-stu-id="28e8b-192">Why?</span></span>
- <span data-ttu-id="28e8b-193">Durch diese Methode wird ganz klar unterteilt, was unter den Schritten *arrange* und *assert* getestet wird.</span><span class="sxs-lookup"><span data-stu-id="28e8b-193">Clearly separates what is being tested from the *arrange* and *assert* steps.</span></span>
- <span data-ttu-id="28e8b-194">Es ist weniger wahrscheinlich, dass Überprüfungen mit dem „act“-Code vermischt werden.</span><span class="sxs-lookup"><span data-stu-id="28e8b-194">Less chance to intermix assertions with "Act" code.</span></span>

<span data-ttu-id="28e8b-195">Die Lesbarkeit ist einer der wichtigsten Punkte beim Schreiben eines Tests.</span><span class="sxs-lookup"><span data-stu-id="28e8b-195">Readability is one of the most important aspects when writing a test.</span></span> <span data-ttu-id="28e8b-196">Durch die Aufteilung innerhalb des Tests werden die Abhängigkeiten deutlich, die beim Codeaufruf, der Art und Weise des Codeaufrufs und bei der Überprüfung entstehen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-196">Separating each of these actions within the test clearly highlight the dependencies required to call your code, how your code is being called, and what you are trying to assert.</span></span> <span data-ttu-id="28e8b-197">Einige Schritte können zwar möglicherweise kombiniert werden, wodurch die Größe Ihres Tests verringert wird, jedoch liegt das Hauptaugenmerk darauf, den Test besser lesbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-197">While it may be possible to combine some steps and reduce the size of your test, the primary goal is to make the test as readable as possible.</span></span>

#### <a name="bad"></a><span data-ttu-id="28e8b-198">Nicht empfohlen:</span><span class="sxs-lookup"><span data-stu-id="28e8b-198">Bad:</span></span>
[!code-csharp[BeforeArranging](../../../samples/csharp/unit-testing-best-practices/before/StringCalculatorTests.cs#BeforeArranging)]

#### <a name="better"></a><span data-ttu-id="28e8b-199">Empfohlen:</span><span class="sxs-lookup"><span data-stu-id="28e8b-199">Better:</span></span>
[!code-csharp[AfterArranging](../../../samples/csharp/unit-testing-best-practices/after/StringCalculatorTests.cs#AfterArranging)]

### <a name="write-minimally-passing-tests"></a><span data-ttu-id="28e8b-200">Schreiben von minimal erfolgreichen Tests</span><span class="sxs-lookup"><span data-stu-id="28e8b-200">Write minimally passing tests</span></span>
<span data-ttu-id="28e8b-201">Die in einem Komponententest verwendete Eingabe soll so einfach wie möglich gehalten werden, damit das Verhalten, das Sie gerade testen, überprüft werden kann.</span><span class="sxs-lookup"><span data-stu-id="28e8b-201">The input to be used in a unit test should be the simplest possible in order to verify the behavior that you are currently testing.</span></span>

#### <a name="why"></a><span data-ttu-id="28e8b-202">Warum?</span><span class="sxs-lookup"><span data-stu-id="28e8b-202">Why?</span></span>
- <span data-ttu-id="28e8b-203">Tests funktionieren auch nach Änderungen an der Codebasis wahrscheinlicher weiter.</span><span class="sxs-lookup"><span data-stu-id="28e8b-203">Tests become more resilient to future changes in the codebase.</span></span>
- <span data-ttu-id="28e8b-204">Das Testverhalten steht im Vordergrund, nicht die Implementierung.</span><span class="sxs-lookup"><span data-stu-id="28e8b-204">Closer to testing behavior over implementation.</span></span>

<span data-ttu-id="28e8b-205">Es ist wahrscheinlicher, dass Tests, die mehr Informationen enthalten, als für das Bestehen erforderlich sind, Fehler verursachen. Dadurch kann die Absicht des Tests weniger eindeutig sein.</span><span class="sxs-lookup"><span data-stu-id="28e8b-205">Tests that include more information than required to pass the test have a higher chance of introducing errors into the test and can make the intent of the test less clear.</span></span> <span data-ttu-id="28e8b-206">Wenn Sie Test schreiben, konzentrieren Sie sich auf das Verhalten.</span><span class="sxs-lookup"><span data-stu-id="28e8b-206">When writing tests you want to focus on the behavior.</span></span> <span data-ttu-id="28e8b-207">Die Festlegung von zusätzlichen Eigenschaften für Modelle oder die Verwendung von Werten, die nicht 0 (null) sind, wenn es nicht nötig ist, lenkt nur vom gewünschten Ergebnis ab.</span><span class="sxs-lookup"><span data-stu-id="28e8b-207">Setting extra properties on models or using non-zero values when not required, only detracts from what you are trying to prove.</span></span>

#### <a name="bad"></a><span data-ttu-id="28e8b-208">Nicht empfohlen:</span><span class="sxs-lookup"><span data-stu-id="28e8b-208">Bad:</span></span>
[!code-csharp[BeforeMinimallyPassing](../../../samples/csharp/unit-testing-best-practices/before/StringCalculatorTests.cs#BeforeMinimallyPassing)]

#### <a name="better"></a><span data-ttu-id="28e8b-209">Empfohlen:</span><span class="sxs-lookup"><span data-stu-id="28e8b-209">Better:</span></span>
[!code-csharp[AfterNamingAndMinimallyPassing](../../../samples/csharp/unit-testing-best-practices/after/StringCalculatorTests.cs#AfterNamingAndMinimallyPassing)]

### <a name="avoid-magic-strings"></a><span data-ttu-id="28e8b-210">Vermeiden „magischer“ Zeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="28e8b-210">Avoid magic strings</span></span>
<span data-ttu-id="28e8b-211">Das Benennen von Variablen in Komponententests ist genauso wichtig, wenn nicht noch wichtiger, wie das Benennen von Variablen im Produktionscode.</span><span class="sxs-lookup"><span data-stu-id="28e8b-211">Naming variables in unit tests is as important, if not more important, than naming variables in production code.</span></span> <span data-ttu-id="28e8b-212">Komponententests sollten keine „magischen“ Zeichenfolgen enthalten.</span><span class="sxs-lookup"><span data-stu-id="28e8b-212">Unit tests should not contain magic strings.</span></span>

#### <a name="why"></a><span data-ttu-id="28e8b-213">Warum?</span><span class="sxs-lookup"><span data-stu-id="28e8b-213">Why?</span></span>
- <span data-ttu-id="28e8b-214">Durch magische Zeichenfolgen sieht es der Leser des Tests womöglich nicht als notwendig an, den Produktionscode zu überprüfen, um herauszufinden, was den Wert besonders macht.</span><span class="sxs-lookup"><span data-stu-id="28e8b-214">Prevents the need for the reader of the test to inspect the production code in order to figure out what makes the value special.</span></span>
- <span data-ttu-id="28e8b-215">Sie stellen explizit dar, was Sie *beweisen* möchten und nicht, was Sie versuchen zu *erreichen*.</span><span class="sxs-lookup"><span data-stu-id="28e8b-215">Explicitly shows what you're trying to *prove* rather than trying to *accomplish*.</span></span>

<span data-ttu-id="28e8b-216">Magische Zeichenfolgen können den Leser Ihrer Tests verwirren.</span><span class="sxs-lookup"><span data-stu-id="28e8b-216">Magic strings can cause confusion to the reader of your tests.</span></span> <span data-ttu-id="28e8b-217">Wenn eine Zeichenfolge nicht wie gewohnt aussieht, fragt sich der Leser womöglich, warum ein bestimmter Wert für einen Parameter oder Rückgabewert ausgewählt wurde.</span><span class="sxs-lookup"><span data-stu-id="28e8b-217">If a string looks out of the ordinary, they may wonder why a certain value was chosen for a parameter or return value.</span></span> <span data-ttu-id="28e8b-218">Dadurch sieht sich der Leser möglicherweise die Implementierungsdetails genauer an und konzentriert sich so nicht mehr auf den Test.</span><span class="sxs-lookup"><span data-stu-id="28e8b-218">This may lead them to take a closer look at the implementation details, rather than focus on the test.</span></span>

> [!TIP] 
> <span data-ttu-id="28e8b-219">Gehen Sie deshalb so genau wie möglich auf die Absicht ein, wenn Sie Tests schreiben.</span><span class="sxs-lookup"><span data-stu-id="28e8b-219">When writing tests, you should aim to express as much intent as possible.</span></span> <span data-ttu-id="28e8b-220">Im Fall von magischen Zeichenfolgen ist es ein guter Ansatz, diese Werte Kontanten zuzuweisen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-220">In the case of magic strings, a good approach is to assign these values to constants.</span></span>

#### <a name="bad"></a><span data-ttu-id="28e8b-221">Nicht empfohlen:</span><span class="sxs-lookup"><span data-stu-id="28e8b-221">Bad:</span></span>
[!code-csharp[BeforeMagicString](../../../samples/csharp/unit-testing-best-practices/before/StringCalculatorTests.cs#BeforeMagicString)]

#### <a name="better"></a><span data-ttu-id="28e8b-222">Empfohlen:</span><span class="sxs-lookup"><span data-stu-id="28e8b-222">Better:</span></span>
[!code-csharp[AfterMagicString](../../../samples/csharp/unit-testing-best-practices/after/StringCalculatorTests.cs#AfterMagicString)]

### <a name="avoid-logic-in-tests"></a><span data-ttu-id="28e8b-223">Vermeiden von Logik in Tests</span><span class="sxs-lookup"><span data-stu-id="28e8b-223">Avoid logic in tests</span></span>
<span data-ttu-id="28e8b-224">Wenn Sie Ihre Komponententests schreiben, vermeiden Sie manuelle Zeichenfolgenverkettungen und logische Bedingungen, z.B. `if`, `while`, `for`, `switch` usw.</span><span class="sxs-lookup"><span data-stu-id="28e8b-224">When writing your unit tests avoid manual string concatenation and logical conditions such as `if`, `while`, `for`, `switch`, etc.</span></span>

#### <a name="why"></a><span data-ttu-id="28e8b-225">Warum?</span><span class="sxs-lookup"><span data-stu-id="28e8b-225">Why?</span></span>
- <span data-ttu-id="28e8b-226">Es ist weniger wahrscheinlich, dass dies zu einem Fehler innerhalb Ihrer Tests führt.</span><span class="sxs-lookup"><span data-stu-id="28e8b-226">Less chance to introduce a bug inside of your tests.</span></span>
- <span data-ttu-id="28e8b-227">Konzentrieren Sie sich auf das Endergebnis und weniger auf die Implementierungsdetails.</span><span class="sxs-lookup"><span data-stu-id="28e8b-227">Focus on the end result, rather than implementation details.</span></span>

<span data-ttu-id="28e8b-228">Wenn Sie Logik in Ihre Testsammlung einfügen, erhöht sich die Wahrscheinlichkeit, dass dies zu einem Fehler führt.</span><span class="sxs-lookup"><span data-stu-id="28e8b-228">When you introduce logic into your test suite, the chance of introducing a bug into it increases dramatically.</span></span> <span data-ttu-id="28e8b-229">Ihre Testsammlung sollte auf keinen Fall einen Fehler enthalten.</span><span class="sxs-lookup"><span data-stu-id="28e8b-229">The last place that you want to find a bug is within your test suite.</span></span> <span data-ttu-id="28e8b-230">Sie müssen sich wirklich sicher sein, dass Ihre Tests funktionieren, andernfalls können Sie sich nicht auf diese verlassen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-230">You should have a high level of confidence that your tests work, otherwise, you will not trust them.</span></span> <span data-ttu-id="28e8b-231">Tests, auf die sie sich nicht verlassen können, haben keinen Nutzen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-231">Tests that you do not trust, do not provide any value.</span></span> <span data-ttu-id="28e8b-232">Wenn ein Test fehlschlägt, sollten Sie merken, dass etwas mit Ihrem Code tatsächlich nicht stimmt, und dass dies nicht ignoriert werden kann.</span><span class="sxs-lookup"><span data-stu-id="28e8b-232">When a test fails, you want to have a sense that something is actually wrong with your code and that it cannot be ignored.</span></span>

> [!TIP]
> <span data-ttu-id="28e8b-233">Wenn Logik in Ihrem Test unvermeidlich scheint, dann erwägen Sie, den Test in zwei oder mehr Tests aufzuteilen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-233">If logic in your test seems unavoidable, consider splitting the test up into two or more different tests.</span></span>

#### <a name="bad"></a><span data-ttu-id="28e8b-234">Nicht empfohlen:</span><span class="sxs-lookup"><span data-stu-id="28e8b-234">Bad:</span></span>
[!code-csharp[LogicInTests](../../../samples/csharp/unit-testing-best-practices/before/StringCalculatorTests.cs#LogicInTests)]

#### <a name="better"></a><span data-ttu-id="28e8b-235">Empfohlen:</span><span class="sxs-lookup"><span data-stu-id="28e8b-235">Better:</span></span>
[!code-csharp[AfterTestLogic](../../../samples/csharp/unit-testing-best-practices/after/StringCalculatorTests.cs#AfterTestLogic)]

### <a name="prefer-helper-methods-to-setup-and-teardown"></a><span data-ttu-id="28e8b-236">Hilfsmethoden statt SetUp und TearDown</span><span class="sxs-lookup"><span data-stu-id="28e8b-236">Prefer helper methods to setup and teardown</span></span>
<span data-ttu-id="28e8b-237">Wenn Sie für Ihre Test ein ähnliches Objekt oder einen ähnlichen Zustand benötigen, nutzen Sie anstatt der „Setup“- und „Teardown“-Attribute (falls vorhanden) lieber eine Hilfsmethode.</span><span class="sxs-lookup"><span data-stu-id="28e8b-237">If you require a similar object or state for your tests, prefer a helper method than leveraging Setup and Teardown attributes if they exist.</span></span>

#### <a name="why"></a><span data-ttu-id="28e8b-238">Warum?</span><span class="sxs-lookup"><span data-stu-id="28e8b-238">Why?</span></span>
- <span data-ttu-id="28e8b-239">Weniger Verwirrung beim Lesen der Tests, da der gesamte Code innerhalb der einzelnen Tests sichtbar ist.</span><span class="sxs-lookup"><span data-stu-id="28e8b-239">Less confusion when reading the tests since all of the code is visible from within each test.</span></span>
- <span data-ttu-id="28e8b-240">Für den spezifischen Test ist das Risiko geringer, zu viel oder zu wenig einzurichten.</span><span class="sxs-lookup"><span data-stu-id="28e8b-240">Less chance of setting up too much or too little for the given test.</span></span>
- <span data-ttu-id="28e8b-241">Geringeres Risiko, den Zustand zwischen Tests freizugeben, was zu unnötigen Abhängigkeiten zwischen diesen führen würde.</span><span class="sxs-lookup"><span data-stu-id="28e8b-241">Less chance of sharing state between tests which creates unwanted dependencies between them.</span></span>

<span data-ttu-id="28e8b-242">In Komponententestframeworks wird `Setup` vor jedem Komponententest innerhalb Ihrer Testsammlung aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-242">In unit testing frameworks, `Setup` is called before each and every unit test within your test suite.</span></span> <span data-ttu-id="28e8b-243">Während einige diese Methode als nützlich betrachten, führt sie letztendlich doch zu überfrachteten und schwer zu lesenden Tests.</span><span class="sxs-lookup"><span data-stu-id="28e8b-243">While some may see this as a useful tool, it generally ends up leading to bloated and hard to read tests.</span></span> <span data-ttu-id="28e8b-244">Jeder Test hat in der Regel unterschiedliche Anforderungen, damit er ordnungsgemäß funktioniert.</span><span class="sxs-lookup"><span data-stu-id="28e8b-244">Each test will generally have different requirements in order to get the test up and running.</span></span> <span data-ttu-id="28e8b-245">Leider werden Sie mit `Setup` gezwungen, exakt dieselben Anforderungen für jeden Test zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="28e8b-245">Unfortunately, `Setup` forces you to use the exact same requirements for each test.</span></span>

> [!NOTE] 
> <span data-ttu-id="28e8b-246">Ab xUnit Version 2.x sind SetUp und TearDown nicht mehr verfügbar.</span><span class="sxs-lookup"><span data-stu-id="28e8b-246">xUnit has removed both SetUp and TearDown as of version 2.x</span></span>

#### <a name="bad"></a><span data-ttu-id="28e8b-247">Nicht empfohlen:</span><span class="sxs-lookup"><span data-stu-id="28e8b-247">Bad:</span></span>
[!code-csharp[BeforeSetup](../../../samples/csharp/unit-testing-best-practices/before/StringCalculatorTests.cs#BeforeSetup)]

```csharp
// more tests...
```

[!code-csharp[BeforeHelperMethod](../../../samples/csharp/unit-testing-best-practices/before/StringCalculatorTests.cs#BeforeHelperMethod)]

#### <a name="better"></a><span data-ttu-id="28e8b-248">Empfohlen:</span><span class="sxs-lookup"><span data-stu-id="28e8b-248">Better:</span></span>
[!code-csharp[AfterHelperMethod](../../../samples/csharp/unit-testing-best-practices/after/StringCalculatorTests.cs#AfterHelperMethod)]

```csharp
// more tests...
```

[!code-csharp[AfterSetup](../../../samples/csharp/unit-testing-best-practices/after/StringCalculatorTests.cs#AfterSetup)]

### <a name="avoid-multiple-asserts"></a><span data-ttu-id="28e8b-249">Vermeiden mehrerer Assert-Anweisungen</span><span class="sxs-lookup"><span data-stu-id="28e8b-249">Avoid multiple asserts</span></span>
<span data-ttu-id="28e8b-250">Wenn Sie Ihre Tests schreiben, versuchen Sie, nur eine Assert-Anweisung pro Test einzuschließen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-250">When writing your tests, try to only include one Assert per test.</span></span> <span data-ttu-id="28e8b-251">Allgemeine Ansätze zur Verwendung von nur einer Assert-Anweisung sind die folgenden:</span><span class="sxs-lookup"><span data-stu-id="28e8b-251">Common approaches to using only one assert include:</span></span>
- <span data-ttu-id="28e8b-252">Erstellen eines separaten Tests für jede Assert-Anweisung</span><span class="sxs-lookup"><span data-stu-id="28e8b-252">Create a separate test for each assert.</span></span>
- <span data-ttu-id="28e8b-253">Verwenden parametrisierter Tests</span><span class="sxs-lookup"><span data-stu-id="28e8b-253">Use parameterized tests.</span></span>

#### <a name="why"></a><span data-ttu-id="28e8b-254">Warum?</span><span class="sxs-lookup"><span data-stu-id="28e8b-254">Why?</span></span>
- <span data-ttu-id="28e8b-255">Wenn eine Assert-Anweisung fehlschlägt, werden die darauffolgenden Assert-Vorgänge nicht ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="28e8b-255">If one Assert fails, the subsequent Asserts will not be evaluated.</span></span>
- <span data-ttu-id="28e8b-256">Es wird sichergestellt, dass Sie nicht mehrere Fälle in Ihren Tests überprüfen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-256">Ensures you are not asserting multiple cases in your tests.</span></span>
- <span data-ttu-id="28e8b-257">Sie erhalten einen Überblick, warum Ihre Tests fehlschlagen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-257">Gives you the entire picture as to why your tests are failing.</span></span> 

<span data-ttu-id="28e8b-258">Wenn mehrere Assert-Anweisungen in einer Testsituation verwendet werden, kann nicht garantiert werden, dass alle Assert-Anweisungen ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="28e8b-258">When introducing multiple asserts into a test case, it is not guaranteed that all of the asserts will be executed.</span></span> <span data-ttu-id="28e8b-259">In den meisten Komponententestframeworks wird automatisch angenommen, dass die nachfolgenden Tests fehlschlagen, sobald eine Überprüfung in einem Komponententest fehlschlägt.</span><span class="sxs-lookup"><span data-stu-id="28e8b-259">In most unit testing frameworks, once an assertion fails in a unit test, the proceeding tests are automatically considered to be failing.</span></span> <span data-ttu-id="28e8b-260">Dies kann verwirrend sein, da eine Funktion, die eigentlich funktioniert, als fehlgeschlagen angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="28e8b-260">This can be confusing as functionality that is actually working, will be shown as failing.</span></span>

> [!NOTE]
> <span data-ttu-id="28e8b-261">Eine Ausnahme bestätigt die Regel: das Überprüfen eines Objekts</span><span class="sxs-lookup"><span data-stu-id="28e8b-261">A common exception to this rule is when asserting against an object.</span></span> <span data-ttu-id="28e8b-262">In diesem Fall sind mehrere Assert-Anweisungen für jede Eigenschaft kein Problem, denn damit wird sichergestellt, dass sich das Objekt in dem Zustand befindet, den Sie von ihm erwarten.</span><span class="sxs-lookup"><span data-stu-id="28e8b-262">In this case, it is generally acceptable to have multiple asserts against each property to ensure the object is in the state that you expect it to be in.</span></span>

#### <a name="bad"></a><span data-ttu-id="28e8b-263">Nicht empfohlen:</span><span class="sxs-lookup"><span data-stu-id="28e8b-263">Bad:</span></span>
[!code-csharp[BeforeMultipleAsserts](../../../samples/csharp/unit-testing-best-practices/before/StringCalculatorTests.cs#BeforeMultipleAsserts)]

#### <a name="better"></a><span data-ttu-id="28e8b-264">Empfohlen:</span><span class="sxs-lookup"><span data-stu-id="28e8b-264">Better:</span></span>
[!code-csharp[AfterMultipleAsserts](../../../samples/csharp/unit-testing-best-practices/after/StringCalculatorTests.cs#AfterMultipleAsserts)]

### <a name="validate-private-methods-by-unit-testing-public-methods"></a><span data-ttu-id="28e8b-265">Überprüfen privater Methoden durch Komponententest von öffentlichen Methoden</span><span class="sxs-lookup"><span data-stu-id="28e8b-265">Validate private methods by unit testing public methods</span></span>
<span data-ttu-id="28e8b-266">In den meisten Fällen sollte es nicht nötig sein, eine private Methode zu testen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-266">In most cases, there should not be a need to test a private method.</span></span> <span data-ttu-id="28e8b-267">Private Methoden sind ein Implementierungsdetail.</span><span class="sxs-lookup"><span data-stu-id="28e8b-267">Private methods are an implementation detail.</span></span> <span data-ttu-id="28e8b-268">Sie können sich diese wie folgt vorstellen: Private Methoden sind nie isoliert vorhanden.</span><span class="sxs-lookup"><span data-stu-id="28e8b-268">You can think of it this way: private methods never exist in isolation.</span></span> <span data-ttu-id="28e8b-269">Irgendwann wird eine öffentliche Methode die private Methode als Teil ihrer Implementierung aufrufen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-269">At some point, there is going to be a public facing method that calls the private method as part of its implementation.</span></span> <span data-ttu-id="28e8b-270">Sie sollten sich über das Endergebnis der öffentlichen Methode Gedanken machen, die die private abruft.</span><span class="sxs-lookup"><span data-stu-id="28e8b-270">What you should care about is the end result of the public method that calls into the private one.</span></span> 

<span data-ttu-id="28e8b-271">Betrachten Sie folgenden Fall:</span><span class="sxs-lookup"><span data-stu-id="28e8b-271">Consider the following case</span></span>

```csharp
public string ParseLogLine(string input)
{
    var sanitizedInput = trimInput(input);
    return sanitizedInput;
}

private string trimInput(string input)
{
    return input.Trim();
}
```

<span data-ttu-id="28e8b-272">Möglicherweise versuchen Sie zunächst, einen Test für `trimInput` zu schreiben, da Sie sicherstellen möchten, dass die Methode wie erwartet funktioniert.</span><span class="sxs-lookup"><span data-stu-id="28e8b-272">Your first reaction may be to start writing a test for `trimInput` because you want to make sure that the method is working as expected.</span></span> <span data-ttu-id="28e8b-273">Es ist jedoch auch möglich, dass `sanitizedInput` von `ParseLogLine` auf unerwartete Weise manipuliert wird, wodurch das Rendern eines Tests für `trimInput` nutzlos ist.</span><span class="sxs-lookup"><span data-stu-id="28e8b-273">However, it is entirely possible that `ParseLogLine` manipulates `sanitizedInput` in such a way that you do not expect, rendering a test against `trimInput` useless.</span></span> 

<span data-ttu-id="28e8b-274">Der echte Test sollte für die öffentliche Methode, `ParseLogLine`, ausgeführt werden, denn darum sollten Sie sich letztendlich kümmern.</span><span class="sxs-lookup"><span data-stu-id="28e8b-274">The real test should be done against the public facing method `ParseLogLine` because that is what you should ultimately care about.</span></span> 

```csharp
public void ParseLogLine_ByDefault_ReturnsTrimmedResult()
{
    var parser = new Parser();

    var result = parser.ParseLogLine(" a ");

    Assert.Equals("a", result);
}
```

<span data-ttu-id="28e8b-275">Wenn Sie mit diesem Wissen eine private Methode erkennen, suchen Sie nach der öffentlichen Methode, und schreiben Sie Ihre Tests für diese.</span><span class="sxs-lookup"><span data-stu-id="28e8b-275">With this viewpoint, if you see a private method, find the public method and write your tests against that method.</span></span> <span data-ttu-id="28e8b-276">Nur weil eine private Methode die erwarteten Ergebnisse liefert, bedeutet das nicht, dass das System, das letztendlich die private Methode aufruft, das Ergebnis richtig verwendet.</span><span class="sxs-lookup"><span data-stu-id="28e8b-276">Just because a private method returns the expected result, does not mean the system that eventually calls the private method uses the result correctly.</span></span>

### <a name="stub-static-references"></a><span data-ttu-id="28e8b-277">Statische Verweise für Stub</span><span class="sxs-lookup"><span data-stu-id="28e8b-277">Stub static references</span></span>
<span data-ttu-id="28e8b-278">Ein Komponententest muss das zu testende System vollständig steuern können.</span><span class="sxs-lookup"><span data-stu-id="28e8b-278">One of the principles of a unit test is that it must have full control of the system under test.</span></span> <span data-ttu-id="28e8b-279">Wenn der Produktionscode Aufrufe von statischen Verweisen (z.B. `DateTime.Now`) enthält, kann dies zu Problemen führen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-279">This can be problematic when production code includes calls to static references (e.g. `DateTime.Now`).</span></span> <span data-ttu-id="28e8b-280">Betrachten Sie folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="28e8b-280">Consider the following code</span></span>

```csharp
public int GetDiscountedPrice(int price)
{
    if(DateTime.Now == DayOfWeek.Tuesday) 
    {
        return price / 2;
    }
    else 
    {
        return price;
    }
}
```

<span data-ttu-id="28e8b-281">Wie kann für diesen Code am besten ein Komponententest ausgeführt werden?</span><span class="sxs-lookup"><span data-stu-id="28e8b-281">How can this code possibly be unit tested?</span></span> <span data-ttu-id="28e8b-282">Sie können beispielsweise folgenden Ansatz ausprobieren:</span><span class="sxs-lookup"><span data-stu-id="28e8b-282">You may try an approach such as</span></span>

```csharp
public void GetDiscountedPrice_ByDefault_ReturnsFullPrice()
{
    var priceCalculator = new PriceCalculator();

    var actual = priceCalculator.GetDiscountedPrice(2);

    Assert.Equals(2, actual)
}

public void GetDiscountedPrice_OnTuesday_ReturnsHalfPrice()
{
    var priceCalculator = new PriceCalculator();

    var actual = priceCalculator.GetDiscountedPrice(2);

    Assert.Equals(1, actual);
}
```

<span data-ttu-id="28e8b-283">Sie werden wahrscheinlich schnell merken, dass mit Ihren Tests leider einiges nicht stimmt.</span><span class="sxs-lookup"><span data-stu-id="28e8b-283">Unfortunately, you will quickly realize that there are a couple problems with your tests.</span></span> 

- <span data-ttu-id="28e8b-284">Wenn die Testsammlung an einem Dienstag ausgeführt wird, ist der zweite Test erfolgreich, der erste schlägt jedoch fehl.</span><span class="sxs-lookup"><span data-stu-id="28e8b-284">If the test suite is run on a Tuesday, the second test will pass, but the first test will fail.</span></span>
- <span data-ttu-id="28e8b-285">Wenn die Testsammlung an einem anderen Tag ausgeführt wird, ist der erste Test erfolgreich, der zweite schlägt jedoch fehl.</span><span class="sxs-lookup"><span data-stu-id="28e8b-285">If the test suite is run on any other day, the first test will pass, but the second test will fail.</span></span>

<span data-ttu-id="28e8b-286">Sie müssen zur Lösung dieser Probleme eine *Nahtstelle* in Ihren Produktionscode einfügen.</span><span class="sxs-lookup"><span data-stu-id="28e8b-286">To solve these problems, you'll need to introduce a *seam* into your production code.</span></span> <span data-ttu-id="28e8b-287">Eine Methode ist das Umschließen des zu steuernden Codes mit einer Schnittstelle. Der Produktionscode soll dann von dieser Schnittstelle abhängig sein.</span><span class="sxs-lookup"><span data-stu-id="28e8b-287">One approach is to wrap the code that you need to control in an interface and have the production code depend on that interface.</span></span>

```csharp
public interface IDateTimeProvider
{
    DayOfWeek DayOfWeek();
}

public bool GetDiscountedPrice(int price, IDateTimeProvider dateTimeProvider)
{
    if(dateTimeProvider.DayOfWeek() == DayOfWeek.Tuesday) 
    {
        return price / 2;
    }
    else 
    {
        return price;
    }
}
```

<span data-ttu-id="28e8b-288">Ihre Testsammlung sieht nun so aus:</span><span class="sxs-lookup"><span data-stu-id="28e8b-288">Your test suite now becomes</span></span>

```csharp
public void GetDiscountedPrice_ByDefault_ReturnsFullPrice()
{
    var priceCalculator = new PriceCalculator();
    var dateTimeProviderStub = new Mock<IDateTimeProvider>();
    dateTimeProviderStub.Setup(dtp => dtp.DayOfWeek()).Returns(DayOfWeek.Monday);

    var actual = priceCalculator.GetDiscountedPrice(2, dateTimeProviderStub);

    Assert.Equals(2, actual);
}

public void GetDiscountedPrice_OnTuesday_ReturnsHalfPrice()
{
    var priceCalculator = new PriceCalculator();
    var dateTimeProviderStub = new Mock<IDateTimeProvider>();
    dateTimeProviderStub.Setup(dtp => dtp.DayOfWeek()).Returns(DayOfWeek.Tuesday);

    var actual = priceCalculator.GetDiscountedPrice(2, dateTimeProviderStub);

    Assert.Equals(1, actual);
}
```

<span data-ttu-id="28e8b-289">Die Testsammlung hat nun volle Kontrolle über `DateTime.Now` und kann für jeden Wert einen Stub ausführen, wenn die Methode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="28e8b-289">Now the test suite has full control over `DateTime.Now` and can stub any value when calling into the method.</span></span>