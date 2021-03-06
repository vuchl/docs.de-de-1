---
title: Die Bereichsvariable <variable> verbirgt eine Variable in einem einschließenden Block, eine zuvor definierte Bereichsvariable oder eine implizit im Abfrageausdruck deklarierte Variable
ms.date: 07/20/2015
f1_keywords:
- bc36633
- vbc36633
helpviewer_keywords:
- BC36633
ms.assetid: 5d5470e4-3de5-49c2-8831-1087625f4a77
ms.openlocfilehash: 5e071970eec70828841c686e89aa673d38ff9918
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2019
ms.locfileid: "64661622"
---
# <a name="range-variable-variable-hides-a-variable-in-an-enclosing-block-a-previously-defined-range-variable-or-an-implicitly-declared-variable-in-a-query-expression"></a>Range-Variable \<Variable > verbirgt eine Variable in einem einschließenden Block, eine zuvor definierte Bereichsvariable oder eine implizit deklarierte Variable in einem Abfrageausdruck
Ein Bereich Variablenname, der im angegebenen ein `Select`, `From`, `Aggregate`, oder `Let` Klausel, um Duplikate der Name einer Bereichsvariablen, die bereits zuvor angegeben werden, in der Abfrage oder den Namen einer Variablen, die von der Abfrage implizit deklariert ist z. B. ein Feldname oder der Name der Aggregatfunktion.  
  
 **Fehler-ID:** BC36633  
  
## <a name="to-correct-this-error"></a>So beheben Sie diesen Fehler  
  
- Stellen Sie sicher, dass alle Bereichsvariablen in einem bestimmten Abfragebereich eindeutige Namen aufweisen. Sie können eine Abfrage einschließen, in Klammern einschließen, um sicherzustellen, dass geschachtelte Abfragen einen eindeutigen Bereich verfügen.  
  
## <a name="see-also"></a>Siehe auch

- [Einführung in LINQ in Visual Basic](../../../visual-basic/programming-guide/language-features/linq/introduction-to-linq.md)
- [From-Klausel](../../../visual-basic/language-reference/queries/from-clause.md)
- [Let-Klausel](../../../visual-basic/language-reference/queries/let-clause.md)
- [Aggregate-Klausel](../../../visual-basic/language-reference/queries/aggregate-clause.md)
- [Select-Klausel](../../../visual-basic/language-reference/queries/select-clause.md)
