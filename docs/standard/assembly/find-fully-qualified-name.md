---
title: 'Vorgehensweise: Ermitteln des vollqualifizierten Namens einer Assembly'
ms.date: 08/20/2019
helpviewer_keywords:
- names [.NET Framework], fully qualified type names
- names [.NET Framework], assemblies
- assemblies [.NET Framework], names
ms.assetid: 009dae23-e1f6-4a64-9a9a-32e4c34802b0
author: rpetrusha
ms.author: ronpet
dev_langs:
- csharp
- vb
- cpp
ms.openlocfilehash: 4fc670adc80a6f4ce7b36074185dcd3bb85fbc67
ms.sourcegitcommit: 005980b14629dfc193ff6cdc040800bc75e0a5a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2019
ms.locfileid: "70991314"
---
# <a name="how-to-find-an-assemblys-fully-qualified-name"></a><span data-ttu-id="cb3aa-102">Vorgehensweise: Ermitteln des vollqualifizierten Namens einer Assembly</span><span class="sxs-lookup"><span data-stu-id="cb3aa-102">How to: Find an assembly's fully qualified name</span></span>

<span data-ttu-id="cb3aa-103">Verwenden Sie das Tool für den globalen Assemblycache ([Gacutil.exe](../../framework/tools/gacutil-exe-gac-tool.md)), um den vollqualifizierten Namen einer .NET Framework-Assembly im globalen Assemblycache zu ermitteln.</span><span class="sxs-lookup"><span data-stu-id="cb3aa-103">To discover the fully qualified name of a .NET Framework assembly in the global assembly cache, use the Global Assembly Cache tool ([Gacutil.exe](../../framework/tools/gacutil-exe-gac-tool.md)).</span></span> <span data-ttu-id="cb3aa-104">Weitere Informationen finden Sie unter [How to: Vorgehensweise: Anzeigen der Inhalte des globalen Assemblycaches](../../framework/app-domains/how-to-view-the-contents-of-the-gac.md).</span><span class="sxs-lookup"><span data-stu-id="cb3aa-104">See [How to: View the contents of the global assembly cache](../../framework/app-domains/how-to-view-the-contents-of-the-gac.md).</span></span>

<span data-ttu-id="cb3aa-105">Für .NET Core- und .NET Framework-Assemblys, die sich nicht im globalen Assemblycache befinden, können Sie den vollqualifizierten Assemblynamen auf verschiedene Weisen abrufen:</span><span class="sxs-lookup"><span data-stu-id="cb3aa-105">For .NET Core assemblies, and for .NET Framework assemblies that aren't in the global assembly cache, you can get the fully qualified assembly name in a number of ways:</span></span>

- <span data-ttu-id="cb3aa-106">Sie können Code verwenden, um die Informationen in der Konsole oder in einer Variable auszugeben, oder Sie können die [Ildasm.exe (IL-Disassembler)](../../framework/tools/ildasm-exe-il-disassembler.md) verwenden, um die Assemblymetadaten zu überprüfen, die den vollqualifizierten Namen enthalten.</span><span class="sxs-lookup"><span data-stu-id="cb3aa-106">You can use code to output the information to the console or to a variable, or you can use the [Ildasm.exe (IL Disassembler)](../../framework/tools/ildasm-exe-il-disassembler.md) to examine the assembly's metadata, which contains the fully qualified name.</span></span>

- <span data-ttu-id="cb3aa-107">Wenn die Assembly bereits von der Anwendung geladen wird, können Sie den Wert der <xref:System.Reflection.Assembly.FullName%2A?displayProperty=nameWithType>-Eigenschaft abrufen, um den vollqualifizierten Namen abzurufen.</span><span class="sxs-lookup"><span data-stu-id="cb3aa-107">If the assembly is already loaded by the application, you can retrieve the value of the <xref:System.Reflection.Assembly.FullName%2A?displayProperty=nameWithType> property to get the fully qualified name.</span></span> <span data-ttu-id="cb3aa-108">Sie können die <xref:System.Type.Assembly>-Eigenschaft einer in dieser Assembly definierten <xref:System.Type>-Klasse verwenden, um einen Verweis auf das <xref:System.Reflection.Assembly>-Objekt abzurufen.</span><span class="sxs-lookup"><span data-stu-id="cb3aa-108">You can use the <xref:System.Type.Assembly> property of a <xref:System.Type> defined in that assembly to retrieve a reference to the <xref:System.Reflection.Assembly> object.</span></span> <span data-ttu-id="cb3aa-109">Dies wird im Beispiel veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="cb3aa-109">The example provides an illustration.</span></span>

- <span data-ttu-id="cb3aa-110">Wenn Sie den Dateisystempfad der Assembly kennen, können Sie die <xref:System.Reflection.AssemblyName.GetAssemblyName%2A?displayProperty=nameWithType>-Methoden `static` (C#) oder `Shared` (Visual Basic) aufrufen, um den vollqualifizierten Assemblynamen abzurufen.</span><span class="sxs-lookup"><span data-stu-id="cb3aa-110">If you know the assembly's file system path, you can call the `static` (C#) or `Shared` (Visual Basic) <xref:System.Reflection.AssemblyName.GetAssemblyName%2A?displayProperty=nameWithType> method to get the fully qualified assembly name.</span></span> <span data-ttu-id="cb3aa-111">Im Folgenden finden Sie ein einfaches Beispiel.</span><span class="sxs-lookup"><span data-stu-id="cb3aa-111">The following is a simple example.</span></span>

  ```csharp
  using System;
  using System.Reflection;

  public class Example
  {
     public static void Main()
     {
        Console.WriteLine(AssemblyName.GetAssemblyName(@".\UtilityLibrary.dll"));
     }
  }
  // The example displays output like the following:
  //   UtilityLibrary, Version=1.1.0.0, Culture=neutral, PublicKeyToken=null
  ```

  ```vb
  Imports System.Reflection

  Public Module Example
     Public Sub Main
        Console.WriteLine(AssemblyName.GetAssemblyName(".\UtilityLibrary.dll"))
     End Sub
  End Module
  ' The example displays output like the following:
  '   UtilityLibrary, Version=1.1.0.0, Culture=neutral, PublicKeyToken=null
  ```

- <span data-ttu-id="cb3aa-112">Sie können den [Ildasm.exe (IL-Disassembler)](../../framework/tools/ildasm-exe-il-disassembler.md) verwenden, um die Metadaten der Assembly zu überprüfen, die den vollqualifizierten Namen enthalten.</span><span class="sxs-lookup"><span data-stu-id="cb3aa-112">You can use the [Ildasm.exe (IL Disassembler)](../../framework/tools/ildasm-exe-il-disassembler.md) to examine the assembly's metadata, which contains the fully qualified name.</span></span>

<span data-ttu-id="cb3aa-113">Weitere Informationen zum Festlegen von Assemblyattributen, z. B. Version, Kultur und Assemblyname, finden Sie unter [Festlegen von Assemblyattributen](set-attributes.md).</span><span class="sxs-lookup"><span data-stu-id="cb3aa-113">For more information about setting assembly attributes such as version, culture, and assembly name, see [Set assembly attributes](set-attributes.md).</span></span> <span data-ttu-id="cb3aa-114">Weitere Informationen zum Vergeben eines starken Namens für eine Assembly finden Sie unter [Erstellen und Verwenden von Assemblys mit starken Namen](create-use-strong-named.md).</span><span class="sxs-lookup"><span data-stu-id="cb3aa-114">For more information about giving an assembly a strong name, see [Create and use strong-named assemblies](create-use-strong-named.md).</span></span>

## <a name="example"></a><span data-ttu-id="cb3aa-115">Beispiel</span><span class="sxs-lookup"><span data-stu-id="cb3aa-115">Example</span></span>

<span data-ttu-id="cb3aa-116">Im folgenden Beispiel wird gezeigt, wie Sie den vollqualifizierten Namen einer Assembly, die eine angegebene Klasse enthält, in der Konsole anzeigen können.</span><span class="sxs-lookup"><span data-stu-id="cb3aa-116">The following example shows how to display the fully qualified name of an assembly containing a specified class to the console.</span></span> <span data-ttu-id="cb3aa-117">Im Beispiel wird die <xref:System.Type.Assembly?displayProperty=nameWithType>-Eigenschaft verwendet, um einen Verweis auf eine Assembly mit einem Typ abzurufen, der in der Assembly definiert ist.</span><span class="sxs-lookup"><span data-stu-id="cb3aa-117">It uses the <xref:System.Type.Assembly?displayProperty=nameWithType> property to retrieve a reference to an assembly from a type that's defined in that assembly.</span></span>

```cpp
#using <System.dll>
#using <System.Data.dll>

using namespace System;
using namespace System::Reflection;

ref class asmname
{
public:
    static void Main()
    {
        Type^ t = System::Data::DataSet::typeid;
        String^ s = t->Assembly->FullName->ToString();
        Console::WriteLine("The fully qualified assembly name " +
            "containing the specified class is {0}.", s);
    }
};

int main()
{
    asmname::Main();
}
```

```csharp
using System;
using System.Reflection;

class asmname
{
    public static void Main()
    {
        Type t = typeof(System.Data.DataSet);
        string s = t.Assembly.FullName.ToString();
        Console.WriteLine("The fully qualified assembly name " +
            "containing the specified class is {0}.", s);
    }
}
```

```vb
Imports System
Imports System.Reflection

Class asmname
    Public Shared Sub Main()
        Dim t As Type = GetType(System.Data.DataSet)
        Dim s As String = t.Assembly.FullName.ToString()
        Console.WriteLine("The fully qualified assembly name " +
            "containing the specified class is {0}.", s)
    End Sub
End Class
```

## <a name="see-also"></a><span data-ttu-id="cb3aa-118">Siehe auch</span><span class="sxs-lookup"><span data-stu-id="cb3aa-118">See also</span></span>

- [<span data-ttu-id="cb3aa-119">Assemblynamen</span><span class="sxs-lookup"><span data-stu-id="cb3aa-119">Assembly names</span></span>](names.md)
- [<span data-ttu-id="cb3aa-120">Erstellen von Assemblys</span><span class="sxs-lookup"><span data-stu-id="cb3aa-120">Create assemblies</span></span>](create.md)
- [<span data-ttu-id="cb3aa-121">Erstellen und Verwenden von Assemblys mit starkem Namen</span><span class="sxs-lookup"><span data-stu-id="cb3aa-121">Create and use strong-named assemblies</span></span>](create-use-strong-named.md)
- [<span data-ttu-id="cb3aa-122">Globaler Assemblycache</span><span class="sxs-lookup"><span data-stu-id="cb3aa-122">Global assembly cache</span></span>](../../framework/app-domains/gac.md)
- [<span data-ttu-id="cb3aa-123">So sucht Common Language Runtime nach Assemblys</span><span class="sxs-lookup"><span data-stu-id="cb3aa-123">How the runtime locates assemblies</span></span>](../../framework/deployment/how-the-runtime-locates-assemblies.md)
- [<span data-ttu-id="cb3aa-124">Programmieren mit Assemblys</span><span class="sxs-lookup"><span data-stu-id="cb3aa-124">Program with assemblies</span></span>](program.md)