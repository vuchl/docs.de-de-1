---
title: Hinzufügen von Inhalt in einem Textfeld mithilfe von Benutzeroberflächenautomatisierung
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- adding content to text boxes
- text boxes, adding content
- UI Automation, adding content to text boxes
ms.assetid: 8bdd1a73-1ecb-4a05-a891-a7827ebb767f
ms.openlocfilehash: 71385690c01d261b4ff1b495674bab377232e81d
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2019
ms.locfileid: "74447250"
---
# <a name="add-content-to-a-text-box-using-ui-automation"></a>Hinzufügen von Inhalt in einem Textfeld mithilfe von Benutzeroberflächenautomatisierung
> [!NOTE]
> Diese Dokumentation ist für .NET Framework-Entwickler vorgesehen, die die verwalteten [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]-Klassen verwenden möchten, die im <xref:System.Windows.Automation>-Namespace definiert sind. Aktuelle Informationen zur [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]finden Sie auf der Seite zur [Windows-Automatisierungs-API: UI-Automatisierung](/windows/win32/winauto/entry-uiauto-win32).  
  
 Dieses Thema enthält Beispielcode, in dem veranschaulicht wird, wie [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] verwendet wird, um Text in ein einzeilige Textfeld einzufügen. Eine alternative Methode wird für mehrzeilige und Rich-Text-Steuerelemente bereitgestellt, in denen [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] nicht anwendbar ist. Zu Vergleichszwecken veranschaulicht das Beispiel auch, wie Win32-Methoden verwendet werden, um die gleichen Ergebnisse zu erzielen.  
  
## <a name="example"></a>Beispiel  
 Im folgenden Beispiel wird eine Sequenz von Text Steuerelementen in einer Zielanwendung durchlaufen. Jedes Text Steuerelement wird getestet, um zu überprüfen, ob ein <xref:System.Windows.Automation.ValuePattern> Objekt mithilfe der <xref:System.Windows.Automation.AutomationElement.TryGetCurrentPattern%2A>-Methode aus ihm abgerufen werden kann. Wenn das Text Steuerelement <xref:System.Windows.Automation.ValuePattern>unterstützt, wird die <xref:System.Windows.Automation.ValuePattern.SetValue%2A>-Methode verwendet, um eine benutzerdefinierte Zeichenfolge in das Text Steuerelement einzufügen. Andernfalls wird die <xref:System.Windows.Forms.SendKeys.SendWait%2A?displayProperty=nameWithType>-Methode verwendet.  
  
 [!code-csharp[InsertText#InsertText](../../../samples/snippets/csharp/VS_Snippets_Wpf/InsertText/CSharp/Window1.xaml.cs#inserttext)]
 [!code-vb[InsertText#InsertText](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/InsertText/VisualBasic/Window1.xaml.vb#inserttext)]  
  
## <a name="see-also"></a>Siehe auch

- [TextPattern-Textbeispiel einfügen](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms771478(v=vs.90))
