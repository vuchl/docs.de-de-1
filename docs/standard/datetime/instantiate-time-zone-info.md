---
title: 'Vorgehensweise: Instanziieren eines TimeZoneInfo-Objekts'
ms.date: 04/10/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- instantiating time zone objects
- time zone objects [.NET Framework], instantiation
ms.assetid: 8cb620e5-c6a6-4267-a52e-beeb73cd1a34
ms.openlocfilehash: 5975270dd6a166bb625278a97f4c60851cbe7942
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73122282"
---
# <a name="how-to-instantiate-a-timezoneinfo-object"></a>Vorgehensweise: Instanziieren eines TimeZoneInfo-Objekts

Die gängigste Methode zum Instanziieren eines <xref:System.TimeZoneInfo> -Objekts ist es, Informationen darüber aus der Registrierung abzurufen. In diesem Thema wird erläutert, wie ein <xref:System.TimeZoneInfo> -Objekt aus der Registrierung des lokalen Systems instanziiert wird.

### <a name="to-instantiate-a-timezoneinfo-object"></a>So instanziieren Sie ein TimeZoneInfo-Objekt

1. Deklarieren Sie ein <xref:System.TimeZoneInfo> -Objekt.

2. Rufen Sie die `static` (`Shared` in Visual Basic) <xref:System.TimeZoneInfo.FindSystemTimeZoneById%2A?displayProperty=nameWithType> -Methode auf.

3. Verarbeiten Sie alle Ausnahmen, die von der Methode ausgelöst werden, insbesondere die <xref:System.TimeZoneNotFoundException> -Ausnahme, die ausgelöst wird, wenn die Zeitzone nicht in der Registrierung definiert ist.

## <a name="example"></a>Beispiel

Der folgende Code ruft ein <xref:System.TimeZoneInfo> -Objekt ab, das die Zeitzone "Eastern Standard Time" darstellt und die der Ortszeit entsprechende "Eastern Standard Time" anzeigt.

[!code-csharp[System.TimeZone2.Concepts#5](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#5)]
[!code-vb[System.TimeZone2.Concepts#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#5)]

Der einzige Parameter der Methode, <xref:System.TimeZoneInfo.FindSystemTimeZoneById%2A?displayProperty=nameWithType> , ist der Bezeichner der Zeitzone, die abgerufen werden soll; er entspricht der <xref:System.TimeZoneInfo.Id%2A?displayProperty=nameWithType> -Eigenschaft des Objekts. Der Zeitzonenbezeichner ist ein Schlüsselfeld, das die Zeitzone eindeutig identifiziert. Während die meisten Schlüssel relativ kurz sind, ist der Zeitzonenbezeichner vergleichsweise lang. In den meisten Fällen entspricht der dazugehörige Wert der <xref:System.TimeZoneInfo.StandardName%2A> -Eigenschaft eines <xref:System.TimeZoneInfo> -Objekts, das zum Bereitstellen des Namens der Standardzeit für die Zeitzone verwendet wird. Es gibt jedoch auch Ausnahmen. Die beste Möglichkeit, um sicherzustellen, dass Sie einen gültigen Bezeichner angeben, besteht darin, die auf Ihrem System verfügbaren Zeitzonen aufzulisten und die Bezeichner der darin enthaltenen Zeitzonen zur Kenntnis zu nehmen. Eine Veranschaulichung finden Sie unter [How to: Enumerate time zones present on a computer](../../../docs/standard/datetime/enumerate-time-zones.md). Das Thema [Suchen der auf einem lokalen System definierten Zeitzonen](../../../docs/standard/datetime/finding-the-time-zones-on-local-system.md) enthält auch eine Liste ausgewählter Zeitzonenbezeichner.

Wenn die Zeitzone gefunden wird, gibt die Methode das zugehörige <xref:System.TimeZoneInfo> -Objekt zurück. Wenn die Zeitzone nicht gefunden wird, löst die Methode eine <xref:System.TimeZoneNotFoundException>aus. Wenn die Zeitzone gefunden wird, aber die zugehörigen Daten beschädigt oder unvollständig sind, löst die Methode eine <xref:System.InvalidTimeZoneException>aus.

Wenn Ihre Anwendung eine Zeitzone benötigt, die vorhanden sein muss, sollten Sie zuerst die <xref:System.TimeZoneInfo.FindSystemTimeZoneById%2A> -Methode aufrufen, um die Zeitzonendaten aus der Registrierung abzurufen. Wenn der Methodenaufruf fehlschlägt, sollte der Ausnahmehandler dann eine neue Instanz der Zeitzone erstellen oder diese durch Deserialisieren eines serialisierten <xref:System.TimeZoneInfo> -Objekts neu erstellen. Ein Beispiel finden [Sie unter Gewusst wie: Wiederherstellen von Zeitzonen aus einer eingebetteten Ressource](../../../docs/standard/datetime/restore-time-zones-from-an-embedded-resource.md) .

## <a name="see-also"></a>Siehe auch

- [Datumsangaben, Uhrzeiten und Zeitzonen](../../../docs/standard/datetime/index.md)
- [Suchen der in einem lokalen System definierten Zeitzonen](../../../docs/standard/datetime/finding-the-time-zones-on-local-system.md)
- [Vorgehensweise: Zugreifen auf die vordefinierte UTC und lokale Zeitzonenobjekte](../../../docs/standard/datetime/access-utc-and-local.md)
