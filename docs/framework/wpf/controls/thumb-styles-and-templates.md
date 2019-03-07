---
title: Thumb-Stile und-Vorlagen
ms.date: 03/30/2017
helpviewer_keywords:
- states [WPF], Thumb
- styles [WPF], Thumb
- templates [WPF], Thumb
- Thumb [WPF], styles and templates
- ControlTemplate [WPF], Thumb
- parts [WPF], Thumb
ms.assetid: 86a49235-62d9-414e-923e-53126e3f930a
ms.openlocfilehash: b7fc595f0c592d42f118c6b5542edf93716c2fca
ms.sourcegitcommit: 5137208fa414d9ca3c58cdfd2155ac81bc89e917
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57509569"
---
# <a name="thumb-styles-and-templates"></a><span data-ttu-id="3c3ac-102">Thumb-Stile und-Vorlagen</span><span class="sxs-lookup"><span data-stu-id="3c3ac-102">Thumb Styles and Templates</span></span>

<span data-ttu-id="3c3ac-103">In diesem Thema wird beschrieben, die Stile und Vorlagen für die <xref:System.Windows.Controls.Primitives.Thumb> Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-103">This topic describes the styles and templates for the <xref:System.Windows.Controls.Primitives.Thumb> control.</span></span> <span data-ttu-id="3c3ac-104">Sie können den Standardwert ändern <xref:System.Windows.Controls.ControlTemplate> auf dem Steuerelement eine unverwechselbare Darstellung verleihen.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-104">You can modify the default <xref:System.Windows.Controls.ControlTemplate> to give the control a unique appearance.</span></span> <span data-ttu-id="3c3ac-105">Weitere Informationen finden Sie unter [Anpassen der Darstellung eines vorhandenen Steuerelements durch Erstellen einer ControlTemplate](customizing-the-appearance-of-an-existing-control.md).</span><span class="sxs-lookup"><span data-stu-id="3c3ac-105">For more information, see [Customizing the Appearance of an Existing Control by Creating a ControlTemplate](customizing-the-appearance-of-an-existing-control.md).</span></span>

## <a name="thumb-parts"></a><span data-ttu-id="3c3ac-106">Thumb-Teile</span><span class="sxs-lookup"><span data-stu-id="3c3ac-106">Thumb Parts</span></span>

<span data-ttu-id="3c3ac-107">Die <xref:System.Windows.Controls.Primitives.Thumb> Steuerelement enthält keine benannten Teile.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-107">The <xref:System.Windows.Controls.Primitives.Thumb> control does not have any named parts.</span></span>

## <a name="thumb-states"></a><span data-ttu-id="3c3ac-108">Thumb-Zustände</span><span class="sxs-lookup"><span data-stu-id="3c3ac-108">Thumb States</span></span>

<span data-ttu-id="3c3ac-109">Die folgende Tabelle enthält die visuellen Zustände für die <xref:System.Windows.Controls.Primitives.Thumb> Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-109">The following table lists the visual states for the <xref:System.Windows.Controls.Primitives.Thumb> control.</span></span>

|<span data-ttu-id="3c3ac-110">VisualState-Name</span><span class="sxs-lookup"><span data-stu-id="3c3ac-110">VisualState Name</span></span>|<span data-ttu-id="3c3ac-111">VisualStateGroup-Name</span><span class="sxs-lookup"><span data-stu-id="3c3ac-111">VisualStateGroup Name</span></span>|<span data-ttu-id="3c3ac-112">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="3c3ac-112">Description</span></span>|
|-|-|-|
|<span data-ttu-id="3c3ac-113">Normal</span><span class="sxs-lookup"><span data-stu-id="3c3ac-113">Normal</span></span>|<span data-ttu-id="3c3ac-114">CommonStates</span><span class="sxs-lookup"><span data-stu-id="3c3ac-114">CommonStates</span></span>|<span data-ttu-id="3c3ac-115">Der Standardzustand</span><span class="sxs-lookup"><span data-stu-id="3c3ac-115">The default state.</span></span>|
|<span data-ttu-id="3c3ac-116">MouseOver</span><span class="sxs-lookup"><span data-stu-id="3c3ac-116">MouseOver</span></span>|<span data-ttu-id="3c3ac-117">CommonStates</span><span class="sxs-lookup"><span data-stu-id="3c3ac-117">CommonStates</span></span>|<span data-ttu-id="3c3ac-118">Der Mauszeiger ist über dem Steuerelement positioniert.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-118">The mouse pointer is positioned over the control.</span></span>|
|<span data-ttu-id="3c3ac-119">Gedrückt</span><span class="sxs-lookup"><span data-stu-id="3c3ac-119">Pressed</span></span>|<span data-ttu-id="3c3ac-120">CommonStates</span><span class="sxs-lookup"><span data-stu-id="3c3ac-120">CommonStates</span></span>|<span data-ttu-id="3c3ac-121">Das Steuerelement wird gedrückt.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-121">The control is pressed.</span></span>|
|<span data-ttu-id="3c3ac-122">Deaktiviert</span><span class="sxs-lookup"><span data-stu-id="3c3ac-122">Disabled</span></span>|<span data-ttu-id="3c3ac-123">CommonStates</span><span class="sxs-lookup"><span data-stu-id="3c3ac-123">CommonStates</span></span>|<span data-ttu-id="3c3ac-124">Das Steuerelement ist deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-124">The control is disabled.</span></span>|
|<span data-ttu-id="3c3ac-125">Focused</span><span class="sxs-lookup"><span data-stu-id="3c3ac-125">Focused</span></span>|<span data-ttu-id="3c3ac-126">FocusStates</span><span class="sxs-lookup"><span data-stu-id="3c3ac-126">FocusStates</span></span>|<span data-ttu-id="3c3ac-127">Der Fokus liegt auf dem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-127">The control has focus.</span></span>|
|<span data-ttu-id="3c3ac-128">Ohne Fokus</span><span class="sxs-lookup"><span data-stu-id="3c3ac-128">Unfocused</span></span>|<span data-ttu-id="3c3ac-129">FocusStates</span><span class="sxs-lookup"><span data-stu-id="3c3ac-129">FocusStates</span></span>|<span data-ttu-id="3c3ac-130">Der Fokus liegt nicht auf dem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-130">The control does not have focus.</span></span>|
|<span data-ttu-id="3c3ac-131">Gültig</span><span class="sxs-lookup"><span data-stu-id="3c3ac-131">Valid</span></span>|<span data-ttu-id="3c3ac-132">ValidationStates</span><span class="sxs-lookup"><span data-stu-id="3c3ac-132">ValidationStates</span></span>|<span data-ttu-id="3c3ac-133">Das Steuerelement verwendet die <xref:System.Windows.Controls.Validation> Klasse und die <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> angefügte Eigenschaft `false`.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-133">The control uses the <xref:System.Windows.Controls.Validation> class and the <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> attached property is `false`.</span></span>|
|<span data-ttu-id="3c3ac-134">InvalidFocused</span><span class="sxs-lookup"><span data-stu-id="3c3ac-134">InvalidFocused</span></span>|<span data-ttu-id="3c3ac-135">ValidationStates</span><span class="sxs-lookup"><span data-stu-id="3c3ac-135">ValidationStates</span></span>|<span data-ttu-id="3c3ac-136">Die <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> angefügte Eigenschaft `true` hat das Steuerelement den Fokus besitzt.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-136">The <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> attached property is `true` has the control has focus.</span></span>|
|<span data-ttu-id="3c3ac-137">InvalidUnfocused</span><span class="sxs-lookup"><span data-stu-id="3c3ac-137">InvalidUnfocused</span></span>|<span data-ttu-id="3c3ac-138">ValidationStates</span><span class="sxs-lookup"><span data-stu-id="3c3ac-138">ValidationStates</span></span>|<span data-ttu-id="3c3ac-139">Die <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> angefügte Eigenschaft `true` hat das Steuerelement keinen Fokus besitzt.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-139">The <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> attached property is `true` has the control does not have focus.</span></span>|

## <a name="thumb-controltemplate-example"></a><span data-ttu-id="3c3ac-140">Thumb-ControlTemplate-Beispiel</span><span class="sxs-lookup"><span data-stu-id="3c3ac-140">Thumb ControlTemplate Example</span></span>

<span data-ttu-id="3c3ac-141">Das folgende Beispiel zeigt, wie Sie definieren eine <xref:System.Windows.Controls.ControlTemplate> für die <xref:System.Windows.Controls.Primitives.Thumb> Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-141">The following example shows how to define a <xref:System.Windows.Controls.ControlTemplate> for the <xref:System.Windows.Controls.Primitives.Thumb> control.</span></span>

[!code-xaml[ControlTemplateExamples#Thumb](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlTemplateExamples/CS/resources/slider.xaml#thumb)]

<span data-ttu-id="3c3ac-142">Im vorhergehenden Beispiel wird mindestens eine der folgenden Ressourcen verwendet.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-142">The preceding example uses one or more of the following resources.</span></span>

[!code-xaml[ControlTemplateExamples#Resources](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlTemplateExamples/CS/resources/shared.xaml#resources)]

<span data-ttu-id="3c3ac-143">Das vollständige Beispiel finden Sie unter [Beispiel zum Formatieren mit ControlTemplates](https://github.com/Microsoft/WPF-Samples/tree/master/Styles%20&%20Templates/IntroToStylingAndTemplating).</span><span class="sxs-lookup"><span data-stu-id="3c3ac-143">For the complete sample, see [Styling with ControlTemplates Sample](https://github.com/Microsoft/WPF-Samples/tree/master/Styles%20&%20Templates/IntroToStylingAndTemplating).</span></span>

## <a name="see-also"></a><span data-ttu-id="3c3ac-144">Siehe auch</span><span class="sxs-lookup"><span data-stu-id="3c3ac-144">See also</span></span>

- <xref:System.Windows.FrameworkElement.Style%2A>
- <xref:System.Windows.Controls.ControlTemplate>
- [<span data-ttu-id="3c3ac-145">Steuerelementformate und -vorlagen</span><span class="sxs-lookup"><span data-stu-id="3c3ac-145">Control Styles and Templates</span></span>](control-styles-and-templates.md)
- [<span data-ttu-id="3c3ac-146">Anpassung von Steuerelementen</span><span class="sxs-lookup"><span data-stu-id="3c3ac-146">Control Customization</span></span>](control-customization.md)
- [<span data-ttu-id="3c3ac-147">Erstellen von Formaten und Vorlagen</span><span class="sxs-lookup"><span data-stu-id="3c3ac-147">Styling and Templating</span></span>](styling-and-templating.md)
- [<span data-ttu-id="3c3ac-148">Anpassen der Darstellung eines vorhandenen Steuerelements durch Erstellen einer ControlTemplate</span><span class="sxs-lookup"><span data-stu-id="3c3ac-148">Customizing the Appearance of an Existing Control by Creating a ControlTemplate</span></span>](customizing-the-appearance-of-an-existing-control.md)