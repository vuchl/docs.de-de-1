---
title: Grundlagen der Vererbung
ms.date: 07/20/2015
helpviewer_keywords:
- derived classes [Visual Basic], inheritance
- MyClass keyword [Visual Basic], using
- MyBase keyword [Visual Basic], using
- Inherits statement [Visual Basic], inheritance
- overriding, Overridable keyword
- MustInherit keyword [Visual Basic], using
- Overrides keyword [Visual Basic], using
- inheritance
- MustInherit classes [Visual Basic]
- MustOverride keyword [Visual Basic], using
- classes [Visual Basic], derived
- NotInheritable keyword [Visual Basic], using
- base classes [Visual Basic], extending properties and methods [Visual Basic]
- NotOverridable keyword [Visual Basic], using
- base classes [Visual Basic], inheritance
- abstract classes [Visual Basic], inheritance
- overriding, Overrides keyword
ms.assetid: dfc8deba-f5b3-4d1d-a937-7cb826446fc5
ms.openlocfilehash: 89fcf2a14d8938d536aa72628218242811baa1a2
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350825"
---
# <a name="inheritance-basics-visual-basic"></a>Grundlagen der Vererbung (Visual Basic)

Die `Inherits`-Anweisung wird verwendet, um eine neue Klasse, die als *abgeleitete Klasse*bezeichnet wird, auf Grundlage einer vorhandenen Klasse, die als *Basisklasse*bezeichnet wird, zu deklarieren. Abgeleitete Klassen erben die Eigenschaften, Methoden, Ereignisse, Felder und Konstanten, die in der Basisklasse definiert sind, und können Sie erweitern. Im folgenden Abschnitt werden einige der Vererbungs Regeln und die Modifizierer beschrieben, die Sie verwenden können, um die Art und Weise zu ändern, in der Klassen geerbt oder geerbt werden:

- Standardmäßig sind alle Klassen vererbbar, sofern Sie nicht mit dem `NotInheritable`-Schlüsselwort gekennzeichnet sind. Klassen können von anderen Klassen in Ihrem Projekt oder von Klassen in anderen Assemblys erben, auf die Ihr Projekt verweist.

- Im Unterschied zu Sprachen, die mehrere Vererbung zulassen, Visual Basic nur die einfache Vererbung in Klassen zulässt. Das heißt, dass abgeleitete Klassen nur eine Basisklasse haben können. Obwohl die mehrfache Vererbung in Klassen nicht zulässig ist, können Klassen mehrere Schnittstellen implementieren, die die gleichen enden effektiv erreichen können.

- Um zu verhindern, dass eingeschränkte Elemente in einer Basisklasse verfügbar gemacht werden, muss der Zugriffstyp einer abgeleiteten Klasse gleich oder restriktiver sein als die Basisklasse. Beispielsweise kann eine `Public` Klasse keine `Friend` oder eine `Private` Klasse erben, und eine `Friend` Klasse kann keine `Private` Klasse erben.

## <a name="inheritance-modifiers"></a>Vererbungs Modifizierer

Visual Basic führt die folgenden Klassen-und Modifizierer auf Klassenebene ein, um Vererbung zu unterstützen:

- `Inherits`-Anweisung – gibt die Basisklasse an.

- `NotInheritable` Modifizierer – verhindert, dass Programmierer die Klasse als Basisklasse verwenden.

- `MustInherit` Modifizierer – gibt an, dass die Klasse nur als Basisklasse verwendet werden soll. Instanzen von `MustInherit` Klassen können nicht direkt erstellt werden. Sie können nur als Basisklassen Instanzen einer abgeleiteten Klasse erstellt werden. (Andere Programmiersprachen, wie z C++ . C#b. und, verwenden den Begriff *abstrakte Klasse* , um eine solche Klasse zu beschreiben.)

## <a name="overriding-properties-and-methods-in-derived-classes"></a>Überschreiben von Eigenschaften und Methoden in abgeleiteten Klassen

Standardmäßig erbt eine abgeleitete Klasse Eigenschaften und Methoden von ihrer Basisklasse. Wenn eine geerbte Eigenschaft oder Methode in der abgeleiteten Klasse anders Verhalten muss, kann Sie *überschrieben*werden. Das heißt, Sie können eine neue Implementierung der-Methode in der abgeleiteten Klasse definieren. Mit folgenden Modifizierern steuern Sie das Überschreiben von Eigenschaften und Methoden:

- `Overridable` – ermöglicht, dass eine Eigenschaft oder Methode in einer Klasse in einer abgeleiteten Klasse überschrieben wird.

- `Overrides` – überschreibt eine `Overridable` Eigenschaft oder-Methode, die in der Basisklasse definiert ist.

- `NotOverridable` – verhindert, dass eine Eigenschaft oder Methode in einer erbenden Klasse überschrieben wird. Standardmäßig werden `Public` Methoden `NotOverridable`.

- `MustOverride` – erfordert, dass eine abgeleitete Klasse die-Eigenschaft oder die-Methode überschreibt. Wenn das `MustOverride`-Schlüsselwort verwendet wird, besteht die Methoden Definition nur aus der `Sub`-, `Function`-oder `Property`-Anweisung. Es sind keine anderen Anweisungen zulässig, und es ist keine `End Sub`-oder `End Function`-Anweisung vorhanden. `MustOverride` Methoden müssen in `MustInherit` Klassen deklariert werden.

Angenommen, Sie möchten Klassen definieren, um die Gehaltsabrechnung zu behandeln. Sie können eine generische `Payroll` Klasse definieren, die eine `RunPayroll` Methode enthält, die die Gehaltsabrechnung für eine typische Woche berechnet. Anschließend können Sie `Payroll` als Basisklasse für eine spezialisiertere `BonusPayroll` Klasse verwenden, die bei der Verteilung von Mitarbeiter-Boni verwendet werden kann.

Die `BonusPayroll` Klasse kann die `PayEmployee` Methode erben und überschreiben, die in der Basis `Payroll` Klasse definiert ist.

Im folgenden Beispiel wird eine Basisklasse, `Payroll,` und eine abgeleitete Klasse `BonusPayroll`definiert, die eine geerbte Methode, `PayEmployee`, überschreibt. Eine Prozedur, die `RunPayroll`, erstellt und übergibt dann ein `Payroll`-Objekt und ein `BonusPayroll`-Objekt an eine Funktion, `Pay`, die die `PayEmployee`-Methode beider-Objekte ausführt.

[!code-vb[VbVbalrOOP#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#28)]

## <a name="the-mybase-keyword"></a>Das MyBase-Schlüsselwort

Das `MyBase`-Schlüsselwort verhält sich wie eine Objekt Variable, die auf die Basisklasse der aktuellen Instanz einer-Klasse verweist. `MyBase` wird häufig verwendet, um auf Basisklassenmember zuzugreifen, die von einer abgeleiteten Klasse überschrieben oder überschattet werden. Insbesondere wird `MyBase.New` verwendet, um explizit einen Basisklassenkonstruktor aus einem abgeleiteten Klassenkonstruktor aufzurufen.

Angenommen, Sie entwerfen eine abgeleitete Klasse, die eine Methode überschreibt, die von der Basisklasse geerbt wurde. Die überschriebene-Methode kann die-Methode in der Basisklasse aufrufen und den Rückgabewert ändern, wie im folgenden Code Fragment gezeigt:

[!code-vb[VbVbalrOOP#109](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#109)]

In der folgenden Liste werden die Einschränkungen bei der Verwendung von `MyBase`beschrieben:

- `MyBase` verweist auf die unmittelbare Basisklasse und die geerbten Member. Sie kann nicht verwendet werden, um auf `Private` Member in der-Klasse zuzugreifen.

- `MyBase` ist ein Schlüsselwort, kein reales Objekt. `MyBase` kann keiner Variablen zugewiesen, an Prozeduren oder bei einem `Is` Vergleich verwendet werden.

- Die Methode, die `MyBase` qualifiziert, muss nicht in der unmittelbaren Basisklasse definiert werden. Sie kann stattdessen in einer indirekt geerbten Basisklasse definiert werden. Damit ein durch `MyBase` qualifizierter Verweis ordnungsgemäß kompiliert werden kann, muss eine Basisklasse eine Methode enthalten, die mit dem Namen und den Typen von Parametern übereinstimmt, die im-Befehl angezeigt werden.

- Sie können `MyBase` nicht verwenden, um `MustOverride` Basisklassen Methoden aufzurufen.

- `MyBase` können nicht verwendet werden, um sich selbst zu qualifizieren. Daher ist der folgende Code nicht gültig:

  `MyBase.MyBase.BtnOK_Click()`

- `MyBase` kann nicht in Modulen verwendet werden.

- `MyBase` kann nicht verwendet werden, um auf Basisklassenmember zuzugreifen, die als `Friend` gekennzeichnet sind, wenn sich die Basisklasse in einer anderen Assembly befindet.

Weitere Informationen und ein weiteres Beispiel finden Sie unter Gewusst [wie: Zugreifen auf eine Variable, die von einer abgeleiteten Klasse ausgeblendet](../../../../visual-basic/programming-guide/language-features/declared-elements/how-to-access-a-variable-hidden-by-a-derived-class.md)wird.

## <a name="the-myclass-keyword"></a>Das Schlüsselwort "MyClass"

Das `MyClass` Schlüsselwort verhält sich wie eine Objekt Variable, die auf die aktuelle Instanz einer Klasse verweist, die ursprünglich implementiert wurde. `MyClass` ähnelt `Me`, aber jeder Methoden-und Eigenschafts Rückruf für `MyClass` wird so behandelt, als ob die Methode oder Eigenschaft " [nodeverridable](../../../../visual-basic/language-reference/modifiers/notoverridable.md)" wäre. Daher wird die-Methode oder-Eigenschaft nicht durch das Überschreiben in einer abgeleiteten Klasse beeinträchtigt.

- `MyClass` ist ein Schlüsselwort, kein reales Objekt. `MyClass` kann keiner Variablen zugewiesen, an Prozeduren oder bei einem `Is` Vergleich verwendet werden.

- `MyClass` verweist auf die enthaltende Klasse und die geerbten Member.

- `MyClass` können als Qualifizierer für `Shared` Member verwendet werden.

- `MyClass` kann nicht innerhalb einer `Shared`-Methode verwendet werden, kann jedoch innerhalb einer Instanzmethode verwendet werden, um auf einen freigegebenen Member einer Klasse zuzugreifen.

- `MyClass` können nicht in Standardmodulen verwendet werden.

- `MyClass` kann verwendet werden, um eine Methode zu qualifizieren, die in einer Basisklasse definiert ist und die über keine Implementierung der in dieser Klasse bereitgestellten Methode verfügt. Ein solcher Verweis hat dieselbe Bedeutung wie `MyBase.`*Methode*.

Im folgenden Beispiel werden `Me` und `MyClass`verglichen.

```vb
Class baseClass
    Public Overridable Sub testMethod()
        MsgBox("Base class string")
    End Sub
    Public Sub useMe()
        ' The following call uses the calling class's method, even if
        ' that method is an override.
        Me.testMethod()
    End Sub
    Public Sub useMyClass()
        ' The following call uses this instance's method and not any
        ' override.
        MyClass.testMethod()
    End Sub
End Class
Class derivedClass : Inherits baseClass
    Public Overrides Sub testMethod()
        MsgBox("Derived class string")
    End Sub
End Class
Class testClasses
    Sub startHere()
        Dim testObj As derivedClass = New derivedClass()
        ' The following call displays "Derived class string".
        testObj.useMe()
        ' The following call displays "Base class string".
        testObj.useMyClass()
    End Sub
End Class
```

Obwohl `derivedClass` `testMethod`überschreibt, hebt das `MyClass`-Schlüsselwort in `useMyClass` die Auswirkungen der Überschreibung auf, und der Compiler löst den-Befehl in die Basisklassen Version von `testMethod`auf.

## <a name="see-also"></a>Siehe auch

- [Inherits-Anweisung](../../../../visual-basic/language-reference/statements/inherits-statement.md)
- [Me, My, MyBase und MyClass](../../../../visual-basic/programming-guide/program-structure/me-my-mybase-and-myclass.md)
