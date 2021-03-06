---
title: LINQ to DataSet
ms.date: 03/30/2017
ms.assetid: 743e3755-3ecb-45a2-8d9b-9ed41f0dcf17
ms.openlocfilehash: 4995512336ee9eb6e33df011757ed533db57e76e
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2019
ms.locfileid: "70783787"
---
# <a name="linq-to-dataset"></a>LINQ to DataSet
Mit LINQ to DataSet wird die Abfrage von Daten, die in einem <xref:System.Data.DataSet> -Objekt zwischengespeichert sind, vereinfacht. Insbesondere LINQ to DataSet die Abfrage vereinfacht, indem Sie Entwicklern das Schreiben von Abfragen aus der Programmiersprache selbst ermöglichen, anstatt eine separate Abfragesprache zu verwenden. Dies ist besonders nützlich für Visual Studio-Entwickler, die nun die Syntax Überprüfung zur Kompilierzeit, statische Typisierung und IntelliSense-Unterstützung nutzen können, die von Visual Studio in Ihren Abfragen bereitgestellt wird.  
  
 LINQ to DataSet können auch verwendet werden, um Daten abzufragen, die aus einer oder mehreren Datenquellen konsolidiert wurden. Dies ermöglicht eine Vielzahl von Szenarios, bei denen Flexibilität bei der Darstellung und Behandlung von Daten erforderlich ist, wie das Abfragen lokal aggregierter Daten und die Zwischenspeicherung auf der mittleren Ebene bei Webanwendungen. Diese Manipulationsmethode wird insbesondere bei der Erstellung von Berichterstellungs-, Analyse- und Business Intelligence-Anwendungen benötigt.  
  
 Die LINQ to DataSet-Funktion wird primär durch die Erweiterungs Methoden in den <xref:System.Data.DataRowExtensions> Klassen <xref:System.Data.DataTableExtensions> und verfügbar gemacht. LINQ to DataSet baut auf auf und verwendet die vorhandene ADO.NET-Architektur und ist nicht dazu gedacht, ADO.net im Anwendungscode zu ersetzen. Vorhandener ADO.NET-Code funktioniert weiterhin in einer LINQ to DataSet Anwendung. Die Beziehung von LINQ to DataSet zu ADO.net und dem Datenspeicher wird im folgenden Diagramm veranschaulicht.  
  
 ![Diagramm, das anzeigt, dass LINQ to DataSet auf dem ADO.NET-Anbieter basiert.](./media/linq-to-dataset/linq-dataset-ado-dotnet-provider.gif)  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
 [Erste Schritte](getting-started-linq-to-dataset.md)  
  
 [Programmierhandbuch](programming-guide-linq-to-dataset.md)  
  
## <a name="reference"></a>Referenz  
 <xref:System.Data.DataTableExtensions>  
  
 <xref:System.Data.DataRowExtensions>  
  
 <xref:System.Data.DataRowComparer>  
  
## <a name="see-also"></a>Siehe auch

- [Language Integrated Query (LINQ) – C#](../../../csharp/programming-guide/concepts/linq/index.md)
- [Language Integrated Query (LINQ) – Visual Basic](../../../visual-basic/programming-guide/concepts/linq/index.md)
- [LINQ und ADO.NET](linq-and-ado-net.md)
- [ADO.NET](index.md)
