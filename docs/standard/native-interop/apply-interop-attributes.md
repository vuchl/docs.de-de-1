---
title: Anwenden von Interop-Attributen
ms.date: 03/30/2017
helpviewer_keywords:
- design-time attributes
- .NET Framework, exposing components to COM
- attributes [.NET Framework], design-time functionality
- conversion-tool attributes
- attributes [.NET Framework], interop-specific
- attributes [.NET Framework], conversion-tool
- interoperation with unmanaged code, applying attributes
- interoperation with unmanaged code, exposing .NET Framework components
- COM interop, exposing COM components
- COM interop, applying attributes
ms.assetid: b6014613-641c-4912-9e2f-83a99210a037
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 27d1a8cc80db9e17000880c006ac1d7c1bd12fec
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68631494"
---
# <a name="applying-interop-attributes"></a><span data-ttu-id="cf1f3-102">Anwenden von Interop-Attributen</span><span class="sxs-lookup"><span data-stu-id="cf1f3-102">Applying Interop Attributes</span></span>
<span data-ttu-id="cf1f3-103">Der <xref:System.Runtime.InteropServices>-Namespace stellt drei Kategorien von Interop-spezifischen Attributen zur Verfügung: Attribute, die Sie zur Entwurfszeit anwenden, Attribute, die von COM-Interop-Tools und -APIs während des Konvertierungsprozesses angewendet werden und Attribute, die entweder von Ihnen oder von COM-Interop angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-103">The <xref:System.Runtime.InteropServices> namespace provides three categories of interop-specific attributes: those applied by you at design time, those applied by COM interop tools and APIs during the conversion process, and those applied either by you or COM interop.</span></span>  
  
 <span data-ttu-id="cf1f3-104">Wenn Sie mit dem Anwenden von Attributen auf verwalteten Code nicht vertraut sind, finden Sie weitere Informationen unter [Erweitern von Metadaten mithilfe von Attributen](../../../docs/standard/attributes/index.md).</span><span class="sxs-lookup"><span data-stu-id="cf1f3-104">If you are unfamiliar with the task of applying attributes to managed code, see [Extending Metadata Using Attributes](../../../docs/standard/attributes/index.md).</span></span> <span data-ttu-id="cf1f3-105">Wie andere benutzerdefinierte Attribute können Sie die Interop-spezifischen Attribute auf Typen, Methoden, Eigenschaften, Parameter, Felder und andere Elemente anwenden.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-105">Like other custom attributes, you can apply interop-specific attributes to types, methods, properties, parameters, fields, and other members.</span></span>  
  
## <a name="design-time-attributes"></a><span data-ttu-id="cf1f3-106">Entwurfszeitattribute</span><span class="sxs-lookup"><span data-stu-id="cf1f3-106">Design-Time Attributes</span></span>  
 <span data-ttu-id="cf1f3-107">Sie können das Ergebnis des Konvertierungsvorgangs von COM-Interop-Tools und -APIs mithilfe von Entwurfszeitattributen anpassen.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-107">You can adjust the outcome of the conversion process performed by COM interop tools and APIs by using design-time attributes.</span></span> <span data-ttu-id="cf1f3-108">Die folgende Tabelle beschreibt die Attribute, die Sie auf dem verwalteten Quellcode anwenden können.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-108">The following table describes the attributes that you can apply to your managed source code.</span></span> <span data-ttu-id="cf1f3-109">COM-Interop-Tools wenden möglicherweise auch die in dieser Tabelle beschriebenen Attribute an.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-109">COM interop tools, on occasion, might also apply the attributes described in this table.</span></span>  
  
|<span data-ttu-id="cf1f3-110">Attribut</span><span class="sxs-lookup"><span data-stu-id="cf1f3-110">Attribute</span></span>|<span data-ttu-id="cf1f3-111">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="cf1f3-111">Description</span></span>|  
|---------------|-----------------|  
|<xref:System.Runtime.InteropServices.AutomationProxyAttribute>|<span data-ttu-id="cf1f3-112">Gibt an, ob der Typ mithilfe des Automation-Marshallers oder eines benutzerdefinierten Proxys und Stubs gemarshallt werden soll.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-112">Specifies whether the type should be marshaled using the Automation marshaler or a custom proxy and stub.</span></span>|  
|<xref:System.Runtime.InteropServices.ClassInterfaceAttribute>|<span data-ttu-id="cf1f3-113">Steuert den Typ der für eine Klasse generierten Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-113">Controls the type of interface generated for a class.</span></span>|  
|<xref:System.Runtime.InteropServices.CoClassAttribute>|<span data-ttu-id="cf1f3-114">Identifiziert den Klassenbezeichner einer aus einer Typbibliothek importierten Co-Klasse.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-114">Identifies the CLSID of the original coclass imported from a type library.</span></span><br /><br /> <span data-ttu-id="cf1f3-115">Dieses Attribut wird in der Regel von COM-Interop-Tools angewendet.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-115">COM interop tools typically apply this attribute.</span></span>|  
|<xref:System.Runtime.InteropServices.ComImportAttribute>|<span data-ttu-id="cf1f3-116">Gibt an, dass die Definition einer Co-Klasse oder -Schnittstelle aus einer COM-Typbibliothek importiert wurde.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-116">Indicates that a coclass or interface definition was imported from a COM type library.</span></span> <span data-ttu-id="cf1f3-117">Die Common Language Runtime verwendet dieses Flag, um zu wissen, wie der Typ aktiviert und gemarshallt wird.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-117">The runtime uses this flag to know how to activate and marshal the type.</span></span> <span data-ttu-id="cf1f3-118">Dieses Attribut verhindert, dass der Typ zurück in eine Typbibliothek exportiert wird.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-118">This attribute prohibits the type from being exported back to a type library.</span></span><br /><br /> <span data-ttu-id="cf1f3-119">Dieses Attribut wird in der Regel von COM-Interop-Tools angewendet.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-119">COM interop tools typically apply this attribute.</span></span>|  
|<xref:System.Runtime.InteropServices.ComRegisterFunctionAttribute>|<span data-ttu-id="cf1f3-120">Gibt an, dass eine Methode aufgerufen werden soll, wenn die Assembly für die Verwendung von COM registriert ist, damit der vom Benutzer geschriebene Code während der Registrierung ausgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-120">Indicates that a method should be called when the assembly is registered for use from COM, so that user-written code can be executed during the registration process.</span></span>|  
|<xref:System.Runtime.InteropServices.ComSourceInterfacesAttribute>|<span data-ttu-id="cf1f3-121">Identifiziert Schnittstellen, die Ereignisquellen für die Klasse sind.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-121">Identifies interfaces that are sources of events for the class.</span></span><br /><br /> <span data-ttu-id="cf1f3-122">Dieses Attribut wird von COM-Interop-Tools angewendet.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-122">COM interop tools can apply this attribute.</span></span>|  
|<xref:System.Runtime.InteropServices.ComUnregisterFunctionAttribute>|<span data-ttu-id="cf1f3-123">Gibt an, dass eine Methode aufgerufen werden soll, wenn die Assembly nicht in COM registriert ist, damit vom Benutzer erstellter Code während des Prozesses ausgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-123">Indicates that a method should be called when the assembly is unregistered from COM, so that user-written code can execute during the process.</span></span>|  
|<xref:System.Runtime.InteropServices.ComVisibleAttribute>|<span data-ttu-id="cf1f3-124">Macht Typen für COM unsichtbar, wenn der Attributwert **FALSE** ist.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-124">Renders types invisible to COM when the attribute value equals **false**.</span></span> <span data-ttu-id="cf1f3-125">Dieses Attribut kann für eine gesamte Assembly oder einen einzelnen Typ angewendet werden, um die COM-Sichtbarkeit zu steuern.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-125">This attribute can be applied to an individual type or to an entire assembly to control COM visibility.</span></span> <span data-ttu-id="cf1f3-126">Standardmäßig sind alle verwalteten, öffentlichen Typen angezeigt. Das Attribut ist nicht erforderlich, um sie sichtbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-126">By default, all managed, public types are visible; the attribute is not needed to make them visible.</span></span>|  
|<xref:System.Runtime.InteropServices.DispIdAttribute>|<span data-ttu-id="cf1f3-127">Gibt die COM-Dispatch-ID (DISPID) einer Methode oder eines Felds an.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-127">Specifies the COM dispatch identifier (DISPID) of a method or field.</span></span> <span data-ttu-id="cf1f3-128">Dieses Attribut enthält die DISPID für die Methode, das Feld oder die Eigenschaft, die es beschreibt.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-128">This attribute contains the DISPID for the method, field, or property it describes.</span></span><br /><br /> <span data-ttu-id="cf1f3-129">Dieses Attribut wird von COM-Interop-Tools angewendet.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-129">COM interop tools can apply this attribute.</span></span>|  
|<xref:System.Runtime.InteropServices.FieldOffsetAttribute>|<span data-ttu-id="cf1f3-130">Gibt die physische Position jedes Felds innerhalb einer Klasse an, wenn **StructLayoutAttribute** verwendet und **LayoutKind** auf explizit festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-130">Indicates the physical position of each field within a class when used with the **StructLayoutAttribute**, and the **LayoutKind** is set to Explicit.</span></span>|  
|<xref:System.Runtime.InteropServices.GuidAttribute>|<span data-ttu-id="cf1f3-131">Gibt den Globally Unique Identifier (GUID) einer Klasse, Schnittstelle oder einer ganzen Typbibliothek an.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-131">Specifies the globally unique identifier (GUID) of a class, interface, or an entire type library.</span></span> <span data-ttu-id="cf1f3-132">Das Format der Zeichenfolge, die an das Attribut übergeben wird, muss ein zulässiges Konstruktorargument für den Typ **System.Guid** darstellen.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-132">The string passed to the attribute must be a format that is an acceptable constructor argument for the type **System.Guid**.</span></span><br /><br /> <span data-ttu-id="cf1f3-133">Dieses Attribut wird von COM-Interop-Tools angewendet.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-133">COM interop tools can apply this attribute.</span></span>|  
|<xref:System.Runtime.InteropServices.IDispatchImplAttribute>|<span data-ttu-id="cf1f3-134">Gibt an, welche **IDispatch**-Schnittstellenimplementierung die Common Language Runtime verwendet, wenn sie duale Schnittstellen und Disp-Schnittstellen für COM verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-134">Indicates which **IDispatch** interface implementation the common language runtime uses when exposing dual interfaces and dispinterfaces to COM.</span></span>|  
|<xref:System.Runtime.InteropServices.InAttribute>|<span data-ttu-id="cf1f3-135">Gibt an, dass Daten zum Aufrufer gemarshallt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-135">Indicates that data should be marshaled in to the caller.</span></span> <span data-ttu-id="cf1f3-136">Kann zum Zuordnen von Parametern verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-136">Can be used to attribute parameters.</span></span>|  
|<xref:System.Runtime.InteropServices.InterfaceTypeAttribute>|<span data-ttu-id="cf1f3-137">Steuert, wie eine verwaltete Schnittstelle für COM-Clients (Dual, IUnknown-abgeleitet oder nur IDispatch) verfügbar gemacht wird.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-137">Controls how a managed interface is exposed to COM clients (Dual, IUnknown-derived, or IDispatch only).</span></span><br /><br /> <span data-ttu-id="cf1f3-138">Dieses Attribut wird von COM-Interop-Tools angewendet.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-138">COM interop tools can apply this attribute.</span></span>|  
|<xref:System.Runtime.InteropServices.LCIDConversionAttribute>|<span data-ttu-id="cf1f3-139">Gibt an, dass eine nicht verwaltete Methodensignatur einen LCID-Parameter erwartet.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-139">Indicates that an unmanaged method signature expects an LCID parameter.</span></span><br /><br /> <span data-ttu-id="cf1f3-140">Dieses Attribut wird von COM-Interop-Tools angewendet.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-140">COM interop tools can apply this attribute.</span></span>|  
|<xref:System.Runtime.InteropServices.MarshalAsAttribute>|<span data-ttu-id="cf1f3-141">Gibt an, wie die Daten in Feldern oder Parametern zwischen verwaltetem und nicht verwaltetem Code gemarshallt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-141">Indicates how the data in fields or parameters should be marshaled between managed and unmanaged code.</span></span> <span data-ttu-id="cf1f3-142">Das Attribut ist immer optional, da jeder Datentyp standardmäßiges Marshallingverhalten aufweist.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-142">The attribute is always optional because each data type has default marshaling behavior.</span></span><br /><br /> <span data-ttu-id="cf1f3-143">Dieses Attribut wird von COM-Interop-Tools angewendet.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-143">COM interop tools can apply this attribute.</span></span>|  
|<xref:System.Runtime.InteropServices.OptionalAttribute>|<span data-ttu-id="cf1f3-144">Gibt an, dass ein Parameter optional ist.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-144">Indicates that a parameter is optional.</span></span><br /><br /> <span data-ttu-id="cf1f3-145">Dieses Attribut wird von COM-Interop-Tools angewendet.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-145">COM interop tools can apply this attribute.</span></span>|  
|<xref:System.Runtime.InteropServices.OutAttribute>|<span data-ttu-id="cf1f3-146">Gibt an, dass die Daten in einem Feld oder Parameter vom aufgerufenen Objekt zurück an den Aufrufer gemarshallt werden müssen.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-146">Indicates that the data in a field or parameter must be marshaled from a called object back to its caller.</span></span>|  
|<xref:System.Runtime.InteropServices.PreserveSigAttribute>|<span data-ttu-id="cf1f3-147">Unterdrückt das HRESULT oder die Retval-Signaturtransformation, die in der Regel bei Interop-Aufrufen stattfindet.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-147">Suppresses the HRESULT or retval signature transformation that normally takes place during interoperation calls.</span></span> <span data-ttu-id="cf1f3-148">Das Attribut wirkt sich auf Marshalling und das Exportieren der Typbibliothek aus.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-148">The attribute affects marshaling as well as type library exporting.</span></span><br /><br /> <span data-ttu-id="cf1f3-149">Dieses Attribut wird von COM-Interop-Tools angewendet.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-149">COM interop tools can apply this attribute.</span></span>|  
|<xref:System.Runtime.InteropServices.ProgIdAttribute>|<span data-ttu-id="cf1f3-150">Gibt die ProgID der .NET Framework-Klasse an.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-150">Specifies the ProgID of a .NET Framework class.</span></span> <span data-ttu-id="cf1f3-151">Kann zum Zuordnen von Klassen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-151">Can be used to attribute classes.</span></span>|  
|<xref:System.Runtime.InteropServices.StructLayoutAttribute>|<span data-ttu-id="cf1f3-152">Steuert das physische Layout der Felder einer Klasse.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-152">Controls the physical layout of the fields of a class.</span></span><br /><br /> <span data-ttu-id="cf1f3-153">Dieses Attribut wird von COM-Interop-Tools angewendet.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-153">COM interop tools can apply this attribute.</span></span>|  
  
## <a name="conversion-tool-attributes"></a><span data-ttu-id="cf1f3-154">Attribute des Konvertierungstools</span><span class="sxs-lookup"><span data-stu-id="cf1f3-154">Conversion-Tool Attributes</span></span>  
 <span data-ttu-id="cf1f3-155">Die folgende Tabelle beschreibt die Attribute, die COM-Interop-Tools während des Konvertierungsvorgangs anwenden.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-155">The following table describes attributes that COM interop tools apply during the conversion process.</span></span> <span data-ttu-id="cf1f3-156">Diese Attribute werden nicht zur Entwurfszeit angewendet.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-156">You do not apply these attributes at design time.</span></span>  
  
|<span data-ttu-id="cf1f3-157">Attribut</span><span class="sxs-lookup"><span data-stu-id="cf1f3-157">Attribute</span></span>|<span data-ttu-id="cf1f3-158">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="cf1f3-158">Description</span></span>|  
|---------------|-----------------|  
|<xref:System.Runtime.InteropServices.ComAliasNameAttribute>|<span data-ttu-id="cf1f3-159">Gibt den COM-Alias für einen Parameter oder Feldtyp an.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-159">Indicates the COM alias for a parameter or field type.</span></span> <span data-ttu-id="cf1f3-160">Kann verwendet werden, um Parameter und Felder zuzuordnen oder Werte zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-160">Can be used to attribute parameters, fields, or return values.</span></span>|  
|<xref:System.Runtime.InteropServices.ComConversionLossAttribute>|<span data-ttu-id="cf1f3-161">Gibt den Informationsverlust zu einer Klasse oder Schnittstelle an, als diese aus einer Typbibliothek in eine Assembly importiert wurden.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-161">Indicates that information about a class or interface was lost when it was imported from a type library to an assembly.</span></span>|  
|<xref:System.Runtime.InteropServices.ComEventInterfaceAttribute>|<span data-ttu-id="cf1f3-162">Identifiziert die Quellschnittstelle und die Klasse, die die Methoden der Ereignisschnittstelle implementieren.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-162">Identifies the source interface and the class that implements the methods of the event interface.</span></span>|  
|<xref:System.Runtime.InteropServices.ImportedFromTypeLibAttribute>|<span data-ttu-id="cf1f3-163">Gibt an, dass die Assembly ursprünglich aus einer COM-Typbibliothek importiert wurde.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-163">Indicates that the assembly was originally imported from a COM type library.</span></span> <span data-ttu-id="cf1f3-164">Dieses Attribut enthält die Typdefinition für die Bibliothek von der ursprünglichen Typbibliothek.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-164">This attribute contains the type library definition of the original type library.</span></span>|  
|<xref:System.Runtime.InteropServices.TypeLibFuncAttribute>|<span data-ttu-id="cf1f3-165">Enthält die **FUNCFLAGS**, die ursprünglich für diese Funktion aus der COM-Typbibliothek importiert wurden.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-165">Contains the **FUNCFLAGS** that were originally imported for this function from the COM type library.</span></span>|  
|<xref:System.Runtime.InteropServices.TypeLibTypeAttribute>|<span data-ttu-id="cf1f3-166">Enthält die **FUNCFLAGS**, die ursprünglich für diesen Typ aus der COM-Typbibliothek importiert wurden.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-166">Contains the **TYPEFLAGS** that were originally imported for this type from the COM type library.</span></span>|  
|<xref:System.Runtime.InteropServices.TypeLibVarAttribute>|<span data-ttu-id="cf1f3-167">Enthält die **FUNCFLAGS**, die ursprünglich für diese Variable aus der COM-Typbibliothek importiert wurden.</span><span class="sxs-lookup"><span data-stu-id="cf1f3-167">Contains the **VARFLAGS** that were originally imported for this variable from the COM type library.</span></span>|  
  
## <a name="see-also"></a><span data-ttu-id="cf1f3-168">Siehe auch</span><span class="sxs-lookup"><span data-stu-id="cf1f3-168">See also</span></span>

- <xref:System.Runtime.InteropServices>
- [<span data-ttu-id="cf1f3-169">Verfügbarmachen von .NET Framework-Komponenten in COM</span><span class="sxs-lookup"><span data-stu-id="cf1f3-169">Exposing .NET Framework Components to COM</span></span>](../../../docs/framework/interop/exposing-dotnet-components-to-com.md)
- [<span data-ttu-id="cf1f3-170">Attribute</span><span class="sxs-lookup"><span data-stu-id="cf1f3-170">Attributes</span></span>](../../../docs/standard/attributes/index.md)
- [<span data-ttu-id="cf1f3-171">Qualifizieren von .NET-Typen für die Interoperation</span><span class="sxs-lookup"><span data-stu-id="cf1f3-171">Qualifying .NET Types for Interoperation</span></span>](../../../docs/standard/native-interop/qualify-net-types-for-interoperation.md)
- [<span data-ttu-id="cf1f3-172">Verpacken einer .NET Framework-Assembly für COM</span><span class="sxs-lookup"><span data-stu-id="cf1f3-172">Packaging a .NET Framework Assembly for COM</span></span>](../../../docs/framework/interop/packaging-an-assembly-for-com.md)