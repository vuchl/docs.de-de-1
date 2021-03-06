---
title: 'Vorgehensweise: Festlegen der Höhe eines Fensters einer Seite'
ms.date: 03/30/2017
helpviewer_keywords:
- windows [WPF], setting height from a page
- pages [WPF], setting window height from
- height of window [WPF], setting from a page
ms.assetid: 4e4488ff-ab5c-4ee9-81a4-e1addb55c5cc
ms.openlocfilehash: c1041af88241011b51c96d7b61423344a32b25ca
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69940798"
---
# <a name="how-to-set-the-height-of-a-window-from-a-page"></a>Vorgehensweise: Festlegen der Höhe eines Fensters einer Seite
In diesem Beispiel wird veranschaulicht, wie die Höhe des Fensters von einem <xref:System.Windows.Controls.Page>festgelegt wird.  
  
## <a name="example"></a>Beispiel  
 Ein <xref:System.Windows.Controls.Page> kann die Höhe des Host Fensters durch Festlegen <xref:System.Windows.Controls.Page.WindowHeight%2A>von festlegen. Diese Eigenschaft ermöglicht es <xref:System.Windows.Controls.Page> dem nicht, explizite Informationen über den Typ des Fensters zu haben, in dem es gehostet wird.  
  
> [!NOTE]
> Um die Höhe eines Fensters mit <xref:System.Windows.Controls.Page.WindowHeight%2A>festzulegen, muss ein <xref:System.Windows.Controls.Page> untergeordnetes Element eines Fensters sein.  
  
 [!code-xaml[HOWTONavigationSnippets#SetPageWindowHeightXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/HOWTONavigationSnippets/CSharp/SetWindowHeightPage.xaml#setpagewindowheightxaml)]
