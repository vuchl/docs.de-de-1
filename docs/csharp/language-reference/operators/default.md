---
title: 'default-Operator: C#-Referenz'
ms.custom: seodec18
description: Der default-Operator dient zum Erzeugen des Standardwerts eines Typs.
ms.date: 08/01/2019
helpviewer_keywords:
- default keyword [C#]
ms.openlocfilehash: 5623cb9dc3790b5bb99635c41cb3f122f4c71d8e
ms.sourcegitcommit: bbfcc913c275885381820be28f61efcf8e83eecc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/05/2019
ms.locfileid: "68796939"
---
# <a name="default-operator-c-reference"></a>default-Operator (C#-Referenz)

Der `default`-Operator dient zum Erzeugen des [Standardwerts](../keywords/default-values-table.md) eines Typs. Das Argument für den `default`-Operator muss der Name eines Typs oder ein Typparameter sein.

Im folgenden Beispiel wird die Verwendung des `default`-Operators veranschaulicht:

[!code-csharp-interactive[default of T](~/samples/csharp/language-reference/operators/DefaultOperator.cs#WithOperand)]

Außerdem verwenden Sie das Schlüsselwort `default` in der [`switch`-Anweisung](../keywords/switch.md) als standardmäßige case-Bezeichnung.

## <a name="default-literal"></a>Standardliteral

Ab C# 7.1 können Sie das `default`-Literal verwenden, um den Standardwert eines Typs zu erzeugen, wenn der Compiler den Ausdruckstyp ableiten kann. Der `default`-Literalausdruck erzeugt den gleichen Wert wie der `default(T)`-Ausdruck, wobei `T` der abgeleitete Typ ist. Sie können das `default`-Literal in den folgenden Fälle verwenden:

- Bei der Zuweisung oder Initialisierung einer Variablen.
- Bei der Deklaration des Standardwerts eines optionalen Methodenparameters.
- Bei einem Methodenaufruf zum Bereitstellen eines Argumentwerts.
- In einer `return`-Anweisung oder als Ausdruck in einem Ausdruckskörpermember.

Im folgenden Beispiel wird die Verwendung des `default`-Literals veranschaulicht:

[!code-csharp-interactive[default literal](~/samples/csharp/language-reference/operators/DefaultOperator.cs#DefaultLiteral)]

## <a name="c-language-specification"></a>C#-Sprachspezifikation

Weitere Informationen finden Sie im Abschnitt [Ausdrücke mit Standardwert](~/_csharplang/spec/expressions.md#default-value-expressions) der [C#-Sprachspezifikation](~/_csharplang/spec/introduction.md).

Weitere Informationen zum `default`-Literal finden Sie unter [Hinweis zum Featurevorschlag](~/_csharplang/proposals/csharp-7.1/target-typed-default.md).

## <a name="see-also"></a>Siehe auch

- [C#-Referenz](../index.md)
- [C#-Operatoren](index.md)
- [Tabelle für Standardwerte](../keywords/default-values-table.md)