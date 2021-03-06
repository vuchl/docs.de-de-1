---
title: Erste Schritte mit Azure Blob Storage mit F#
description: Speichern Sie unstrukturierte Daten in der Cloud mit Azure BLOB Storage.
author: sylvanc
ms.date: 09/20/2016
ms.openlocfilehash: 90ec0d63b11ad00c53a1740211e9a6509582e863
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2020
ms.locfileid: "75935502"
---
# <a name="get-started-with-azure-blob-storage-using-f"></a>Einstieg in Azure BLOB Storage mit F\#

Azure Blob Storage ist ein Dienst, der unstrukturierte Daten in der Cloud als Objekte/Blobs speichert. Blob Storage kann beliebige Typen von Text oder Binärdaten speichern, z.B. Dokumente, Mediendateien oder Installationsprogramme für Anwendungen. Blob Storage wird auch als Objektspeicher bezeichnet.

In diesem Artikel erfahren Sie, wie Sie häufige Aufgaben mit BLOB Storage ausführen. Die Beispiele wurden mit F# mit der Azure-Speicherclientbibliothek für .NET. Die behandelten Aufgaben umfassen das Hochladen, auflisten, herunterladen und Löschen von BLOB.

Eine konzeptionelle Übersicht über BLOB Storage finden Sie [im .net-Handbuch für BLOB Storage](/azure/storage/storage-dotnet-how-to-use-blobs).

## <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Handbuch verwenden zu können, müssen Sie zunächst [ein Azure Storage-Konto erstellen](/azure/storage/storage-create-storage-account). Sie benötigen auch ihren Speicherzugriffs Schlüssel für dieses Konto.

## <a name="create-an-f-script-and-start-f-interactive"></a>Erstellen Sie einen F#-Skript, und starten F# Interactive

Die Beispiele in diesem Artikel können entweder in einer F# Anwendung oder in einem F# Skript verwendet werden. Um ein F# Skript zu erstellen, erstellen Sie eine Datei mit der `.fsx`-Erweiterung, z. b F# . `blobs.fsx`, in Ihrer Entwicklungsumgebung.

Verwenden Sie als nächstes einen [Paket-Manager](package-management.md) , wie z. b. [Paket](https://fsprojects.github.io/Paket/) oder [nuget](https://www.nuget.org/) , um die `WindowsAzure.Storage` zu installieren, und `Microsoft.WindowsAzure.ConfigurationManager` Pakete und Verweise `WindowsAzure.Storage.dll` und `Microsoft.WindowsAzure.Configuration.dll` in Ihrem Skript mithilfe einer `#r` Direktive.

### <a name="add-namespace-declarations"></a>Hinzufügen von Namespacedeklarationen

Fügen Sie am Anfang der Datei `blobs.fsx` die folgenden `open`-Anweisungen ein:

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L1-L5)]

### <a name="get-your-connection-string"></a>Abrufen der Verbindungszeichenfolge

Für dieses Tutorial benötigen Sie eine Azure Storage Verbindungs Zeichenfolge. Weitere Informationen zu Verbindungs Zeichenfolgen finden Sie unter [Konfigurieren von Speicher Verbindungs](/azure/storage/storage-configure-connection-string)Zeichenfolgen.

Für das Tutorial geben Sie Ihre Verbindungs Zeichenfolge wie folgt in Ihr Skript ein:

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L11-L11)]

Dies wird jedoch nicht für echte Projekte **empfohlen** . Ihr Speicherkontoschlüssel ähnelt dem Stammkennwort für das Speicherkonto. Achten Sie darauf, den Speicherkontoschlüssel immer gut zu schützen. Geben Sie ihn nicht an andere Benutzer weiter, vermeiden Sie das Hartcodieren, und speichern Sie ihn nicht in einer Klartextdatei, auf die andere Benutzer zugreifen können. Sie können Ihren Schlüssel mithilfe des Azure-Portals neu generieren, wenn Sie der Meinung sind, dass er möglicherweise kompromittiert wurde

Bei echten Anwendungen ist die beste Möglichkeit, Ihre Speicher Verbindungs Zeichenfolge beizubehalten, in einer Konfigurationsdatei. Wenn Sie die Verbindungs Zeichenfolge aus einer Konfigurationsdatei abrufen möchten, können Sie Folgendes tun:

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L13-L15)]

Die Verwendung von Azure Configuration Manager ist optional. Sie können auch eine API verwenden, z. b. den `ConfigurationManager` Typ der .NET Framework.

### <a name="parse-the-connection-string"></a>Analysieren der Verbindungszeichenfolge

Um die Verbindungs Zeichenfolge zu analysieren, verwenden Sie Folgendes:

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L21-L22)]

Dadurch wird eine `CloudStorageAccount`zurückgegeben.

### <a name="create-some-local-dummy-data"></a>Erstellen von lokalen Dummydaten

Bevor Sie beginnen, erstellen Sie im Verzeichnis des Skripts einige lokale Dummydaten. Später laden Sie diese Daten hoch.

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L28-L30)]

### <a name="create-the-blob-service-client"></a>Erstellen des Blob-Dienstclients

Der `CloudBlobClient`-Typ ermöglicht das Abrufen von Containern und BLOBs, die in BLOB Storage gespeichert sind. Hier sehen Sie eine Möglichkeit zum Erstellen des Dienstclients:

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L36-L36)]

Jetzt können Sie Code schreiben, der Daten aus dem Blobspeicher liest und Daten in den Blobspeicher schreibt.

## <a name="create-a-container"></a>Erstellen eines Containers

Dieses Beispiel zeigt, wie Sie einen Container erstellen, falls er nicht bereits vorhanden ist.

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L42-L46)]

Standardmäßig ist der neue Container privat. Das bedeutet, Sie müssen Ihren Speicherzugriffsschlüssel angeben, um Blobs aus diesem Container herunterzuladen. Wenn die Dateien im Container für alle verfügbar sein sollen, können Sie den Container mithilfe des folgenden Codes öffentlich machen:

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L48-L49)]

Jede Person im Internet kann Blobs in einem öffentlichen Container anzeigen, Sie können sie jedoch nur bearbeiten oder löschen, wenn Sie über den entsprechenden Kontozugriffsschlüssel oder eine SAS (Shared Access Signature) verfügen.

## <a name="upload-a-blob-into-a-container"></a>Hochladen eines Blobs in einen Container

Azure Blob Storage unterstützt Block- und Seitenblobs. In den meisten Fällen ist ein blockblob der empfohlene Typ, der verwendet werden soll.

Rufen Sie einen Containerverweis ab und verwenden Sie diesen zum Abrufen eines Blockblobverweises, um eine Datei in einen Blockblob hochzuladen. Sobald Sie über einen BLOB-Verweis verfügen, können Sie jeden Datenstrom in diesen hochladen, indem Sie die `UploadFromFile`-Methode aufrufen. Dieser Vorgang erstellt das BLOB, wenn es nicht bereits vorhanden ist, oder überschreibt es, falls es vorhanden ist.

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L55-L59)]

## <a name="list-the-blobs-in-a-container"></a>Auflisten der Blobs in einem Container

Um die Blobs in einem Container aufzuführen, müssen Sie zuerst einen Containerverweis abrufen. Anschließend können Sie mit der `ListBlobs`-Methode des Containers die darin enthaltenen blobverzeichnisse und/oder Verzeichnisse abrufen. Um auf den umfangreichen Satz von Eigenschaften und Methoden für ein zurück gegebenes `IListBlobItem`zuzugreifen, müssen Sie es in ein `CloudBlockBlob`, `CloudPageBlob`oder `CloudBlobDirectory` Objekt umwandeln. Wenn der Typ unbekannt ist, können Sie eine Typenüberprüfung durchführen, um zu bestimmen, in welchen Typ die Umwandlung erfolgen soll. Der folgende Code zeigt, wie Sie die URI der einzelnen Elemente im `mydata` -Container abrufen und ausgeben:

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L67-L80)]

Sie können auch blobnamen mit Pfadinformationen benennen. Dadurch entsteht eine virtuelle Verzeichnisstruktur, die Sie wie ein herkömmliches Dateisystem organisieren und durchlaufen können. Beachten Sie, dass nur die Verzeichnisstruktur virtuell ist – die einzigen Ressourcen, die im Blob-Speicher verfügbar sind, sind Container und Blobs. Die Speicher Client Bibliothek bietet jedoch ein `CloudBlobDirectory` Objekt, das auf ein virtuelles Verzeichnis verweist und den Prozess der Arbeit mit BLOB, die auf diese Weise organisiert sind, vereinfacht.

Betrachten Sie z. B. den folgenden Satz von Blockblobs in einem Container mit dem Namen `photos`:

*photo1.jpg*\
*2015/architecture/description.txt*\
*2015/Architecture/photo3. jpg*\
*2015/architecture/photo4.jpg*\
*2016/Architecture/photo5. jpg* -\
*2016/architecture/photo6.jpg*\
*2016/architecture/description.txt*\
*2016/photo7.jpg*\

Wenn Sie `ListBlobs` für einen Container (wie im obigen Beispiel) aufzurufen, wird eine hierarchische Auflistung zurückgegeben. Wenn Sie sowohl `CloudBlobDirectory`-als auch `CloudBlockBlob`-Objekte enthält, die jeweils die Verzeichnisse und blobdateien im Container darstellen, sieht die resultierende Ausgabe in etwa wie folgt aus:

```console
Directory: https://<accountname>.blob.core.windows.net/photos/2015/
Directory: https://<accountname>.blob.core.windows.net/photos/2016/
Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg
```

Optional können Sie den `UseFlatBlobListing`-Parameter der `ListBlobs`-Methode auf `true`festlegen. In diesem Fall wird jedes BLOB im Container als `CloudBlockBlob` Objekt zurückgegeben. Der `ListBlobs` zum Zurückgeben einer flachen Auflistung sieht wie folgt aus:

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L82-L89)]

und abhängig vom aktuellen Inhalt Ihres Containers sehen die Ergebnisse wie folgt aus:

```console
Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2015/architecture/description.txt
Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2015/architecture/photo3.jpg
Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2015/architecture/photo4.jpg
Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2016/architecture/description.txt
Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2016/architecture/photo5.jpg
Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2016/architecture/photo6.jpg
Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2016/photo7.jpg
Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg
```

## <a name="download-blobs"></a>Herunterladen von Blobs

Um BLOBs herunterzuladen, rufen Sie zuerst einen blobverweis ab, und rufen Sie dann die `DownloadToStream`-Methode auf. Im folgenden Beispiel wird die `DownloadToStream`-Methode verwendet, um den BLOB-Inhalt in ein Datenstrom Objekt zu übertragen, das Sie dann in einer lokalen Datei speichern können.

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L95-L101)]

Sie können auch die `DownloadToStream`-Methode verwenden, um den Inhalt eines Blobs als Text Zeichenfolge herunterzuladen.

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L103-L106)]

## <a name="delete-blobs"></a>Löschen von Blobs

Um ein BLOB zu löschen, müssen Sie zuerst einen blobverweis abrufen und dann die `Delete`-Methode darauf anwenden.

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L112-L116)]

## <a name="list-blobs-in-pages-asynchronously"></a>Asynchrones Auflisten von Blobs auf Seiten

Wenn Sie eine große Anzahl von Blobs auflisten oder die Anzahl der Ergebnisse steuern möchten, die in einem Auflistungsvorgang zurückgegeben werden, können Sie Blobs auf Ergebnisseiten auflisten. In diesem Beispiel wird gezeigt, wie Sie Ergebnisse auf Seiten asynchron zurückgeben, sodass die Ausführung nicht durch einen großen Ergebnissatz blockiert wird.

Dieses Beispiel zeigt eine einfache BLOB-Auflistung. Sie können jedoch auch eine hierarchische Auflistung ausführen, indem Sie den `useFlatBlobListing`-Parameter der `ListBlobsSegmentedAsync`-Methode auf `false`festlegen.

Im Beispiel wird eine asynchrone Methode mit einem `async`-Block definiert. Das ``let!``-Schlüsselwort hält die Ausführung der Beispiel Methode an, bis die Auflistungs Aufgabe abgeschlossen ist.

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L122-L160)]

Diese asynchrone Routine kann nun wie folgt verwendet werden. Zunächst laden Sie einige Dummydaten hoch (mithilfe der lokalen Datei, die Sie zuvor in diesem Tutorial erstellt haben).

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L162-L166)]

Aufrufen Sie nun die-Routine. Sie verwenden `Async.RunSynchronously`, um die Ausführung des asynchronen Vorgangs zu erzwingen.

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L168-L168)]

## <a name="writing-to-an-append-blob"></a>Beschreiben eines Anfügeblobs

Anfügeblobs sind für Anfügevorgäng wie die Protokollierung optimiert. Ein Anfügeblob besteht wie ein Blockblob aus Blöcken. Allerdings ist es bei einem Anfügeblob so, dass ein neuer Block immer ans Ende des Blobs angefügt wird. Das Aktualisieren oder Löschen eines vorhandenen Blocks ist in einem Anfügeblob nicht möglich. Anders als bei Blockblobs sind die Block-IDs sind für Anfügeblobs nicht verfügbar.

In einem Anfügeblob kann jeder Block unterschiedlich groß sein, bis maximal 4 MB. Insgesamt können bis zu 50.000 Blöcke enthalten sein. Die maximale Größe eines Anfügeblobs ist deshalb etwas mehr als 195 GB (4 MB X 50.000 Blöcke).

Im folgenden Beispiel wird ein neues anfügeblob erstellt und Daten an das BLOB angehängt, wobei ein einfacher Protokollierungs Vorgang simuliert wird.

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L174-L203)]

Weitere Informationen zu den Unterschieden zwischen den drei Arten von Blobs finden Sie unter [Grundlegendes zu Blockblobs, Seitenblobs und Anfügeblobs](https://msdn.microsoft.com/library/azure/ee691964.aspx) .

## <a name="concurrent-access"></a>Paralleler Zugriff

Um den parallelen Zugriff auf eine Blob von mehreren Clients oder mehreren Prozessinstanzen zu unterstützen, können Sie **ETags** oder **Leases** verwenden.

- **Etag** – bietet eine Möglichkeit, um zu ermitteln, dass das Blob oder der Container durch einen anderen Prozess geändert wurde.

- **Lease** – bietet eine Möglichkeit zum Abrufen eines exklusiven, erneuerbaren Lese- oder Schreibzugriffs auf ein Blob für einen Zeitraum.

Weitere Informationen finden Sie unter [Verwalten der Parallelität in Microsoft Azure Storage](https://azure.microsoft.com/blog/managing-concurrency-in-microsoft-azure-storage-2/).

## <a name="naming-containers"></a>Benennen von Containern

Jeder Blob im Azure-Speicher muss sich in einem Container befinden. Der Container ist Teil des Blob-Namens. Beispiel: `mydata` ist der Name des Containers in diesen Beispiel-Blob-URIs:

- https://storagesample.blob.core.windows.net/mydata/blob1.txt
- https://storagesample.blob.core.windows.net/mydata/photos/myphoto.jpg

Ein Containername muss ein gültiger DNS-Name sein und den folgenden Benennungsregeln entsprechen:

1. Containernamen müssen mit einem Buchstaben oder einer Zahl beginnen und dürfen nur Buchstaben, Zahlen und Bindestriche (-) enthalten.
1. Jedem Bindestrich (-) muss unmittelbar ein Buchstabe oder eine Zahl vorangehen und folgen von einem Buchstaben oder einer Zahl; zudem dürfen nicht mehrere Bindestriche direkt aufeinander folgen.
1. Der Containername darf ausschließlich Kleinbuchstaben enthalten.
1. Containernamen müssen zwischen 3 und 63 Zeichen lang sein.

Beachten Sie, dass der Name eines Containers immer aus Kleinbuchstaben bestehen muss. Wenn der Containername einen Großbuchstaben enthält oder anderweitig gegen die Benennungsregeln für Container verstößt, wird möglicherweise der Fehlercode 400 (Ungültige Anforderung) zurückgegeben.

## <a name="managing-security-for-blobs"></a>Verwalten der Sicherheit für Blobs

Standardmäßig sichert Azure Storage Ihre Daten, indem der Zugriff auf den Kontobesitzer beschränkt ist, der im Besitz der Kontozugriffsschlüssel ist. Wenn Sie Blobdaten in Ihrem Speicherkonto freigeben müssen, ist es wichtig, dass die Sicherheit Ihrer Kontozugriffsschlüssel dadurch nicht beeinträchtigt wird. Darüber hinaus können Sie Blobdaten verschlüsseln, um sicherzustellen, dass sie bei der Übertragung zu Azure Storage sicher sind.

### <a name="controlling-access-to-blob-data"></a>Steuern des Zugriffs auf Blobdaten

Standardmäßig können nur Sie als Speicherkontobesitzer auf die Blobdaten in Ihrem Speicherkonto zugreifen. Die Authentifizierung von Anforderungen an Blob Storage erfordert standardmäßig den Kontozugriffsschlüssel. Möglicherweise möchten Sie jedoch bestimmte BLOB-Daten anderen Benutzern zur Verfügung stellen.

### <a name="encrypting-blob-data"></a>Verschlüsseln von Blobdaten

Azure Storage unterstützt das Verschlüsseln von BLOB-Daten sowohl auf dem Client als auch auf dem Server.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun mit den Grundlagen von Blobspeichern vertraut gemacht haben, lesen Sie die folgenden Artikel, um mehr zu erfahren.

### <a name="tools"></a>Tools

- [ F# Azurestoragetypeprovider](https://fsprojects.github.io/AzureStorageTypeProvider/) -\
Ein F# Typanbieter, der zum Durchsuchen von BLOB-, Tabellen-und Warteschlangen Azure Storage Assets und einfachen Anwenden von CRUD-Vorgängen verwendet werden kann.

- [FSharp.Azure.Storage](https://github.com/fsprojects/FSharp.Azure.Storage)\
Eine F# API für die Verwendung Microsoft Azure Table Storage Dienstanbieter

- [Microsoft Azure Storage-Explorer (Mase)](/azure/vs-azure-tools-storage-manage-with-storage-explorer)\
Eine kostenlose eigenständige APP von Microsoft, die es Ihnen ermöglicht, mit Azure Storage-Daten unter Windows, OS X und Linux visuell zu arbeiten.

### <a name="blob-storage-reference"></a>Blob Storage-Referenz

- [Azure Storage-APIs für .NET](/dotnet/api/overview/azure/storage)
- [Referenz zur REST-API von Azure Storage-Diensten](/rest/api/storageservices/Azure-Storage-Services-REST-API-Reference)

### <a name="related-guides"></a>Verwandte Leitfäden

- [Die ersten Schritte mit Azure BLOB Storage inC#](https://azure.microsoft.com/resources/samples/storage-blob-dotnet-getting-started/)
- [Übertragen von Daten mit dem Befehlszeilen-Hilfsprogramm azcopy unter Windows](/azure/storage/common/storage-use-azcopy)
- [Übertragen von Daten mit dem Befehlszeilenprogramm azcopy unter Linux](/azure/storage/common/storage-use-azcopy-linux)
- [Konfigurieren von Azure Storage-Verbindungszeichenfolgen](/azure/storage/common/storage-configure-connection-string)
- [Azure Storage-Teamblog](https://docs.microsoft.com/archive/blogs/windowsazurestorage/)
- [Schnellstart: Verwenden von .net zum Erstellen eines BLOBs im Objektspeicher](/azure/storage/blobs/storage-quickstart-blobs-dotnet)
