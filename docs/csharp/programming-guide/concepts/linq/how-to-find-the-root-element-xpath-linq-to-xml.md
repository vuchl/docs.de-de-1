---
title: 'Vorgehensweise: Ermitteln des Stammelements (XPath-LINQ to XML) (C#)'
ms.date: 07/20/2015
ms.assetid: 4fd824e0-4d39-429b-b092-f6a5c046ee6c
ms.openlocfilehash: 1c5526f436b5b9d88ca359ef7e0fc04c5c3cf43c
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75345948"
---
# <a name="how-to-find-the-root-element-xpath-linq-to-xml-c"></a>Vorgehensweise: Ermitteln des Stammelements (XPath-LINQ to XML) (C#)
In diesem Thema wird gezeigt, wie Sie mit XPath und mit [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] das Stammelement ermitteln können.  
  
 Der XPath-Ausdruck lautet:  
  
 `/PurchaseOrders`  
  
## <a name="example"></a>Beispiel  
 Dieses Beispiel sucht nach dem Stammelement.  
  
 In diesem Beispiel wird das folgende XML-Dokument verwendet: [Beispiel-XML-Datei: Mehrere Bestellungen (LINQ to XML)](./sample-xml-file-multiple-purchase-orders-linq-to-xml.md).  
  
```csharp  
XDocument po = XDocument.Load("PurchaseOrders.xml");  
  
// LINQ to XML query  
XElement el1 = po.Root;  
  
// XPath expression  
XElement el2 = po.XPathSelectElement("/PurchaseOrders");  
  
if (el1 == el2)  
    Console.WriteLine("Results are identical");  
else  
    Console.WriteLine("Results differ");  
Console.WriteLine(el1.Name);  
```  
  
 Dieses Beispiel erzeugt die folgende Ausgabe:  
  
```output  
Results are identical  
PurchaseOrders  
```  
