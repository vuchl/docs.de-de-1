---
title: Bundles von cloudbasierten Anwendungen
description: Architektur von Cloud Native .net-apps für Azure | Native Cloud-Anwendungs Bündel
ms.date: 06/30/2019
ms.openlocfilehash: 0c67035af08d3c337ff027f3742e1ce8a83f8d0f
ms.sourcegitcommit: 55f438d4d00a34b9aca9eedaac3f85590bb11565
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "73840742"
---
# <a name="cloud-native-application-bundles"></a>Bundles von cloudbasierten Anwendungen

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

Eine Schlüsseleigenschaft von Cloud-native Anwendungen ist, dass Sie die Eigenschaften der Cloud nutzen, um die Entwicklung zu beschleunigen. Dies bedeutet häufig, dass eine vollständige Anwendung verschiedene Arten von Technologien verwendet. Anwendungen können in docker-Containern ausgeliefert werden. einige Dienste können Azure Functions verwenden, während andere Teile direkt auf virtuellen Computern ausgeführt werden können, die auf großen Metal-Servern mit Hardware-GPU-Beschleunigung zugeordnet sind. Es sind keine zwei cloudbasierten Anwendungen identisch, weshalb es schwierig ist, einen einzigen Mechanismus für den Versand bereitzustellen.

Die Docker-Container können auf Kubernetes mithilfe eines Helm-Diagramms für die Bereitstellung ausgeführt werden. Die Azure Functions können mithilfe von TERRAFORM-Vorlagen zugeordnet werden. Schließlich können die virtuellen Computer mithilfe von TERRAFORM zugeordnet, aber mithilfe von ansible erstellt werden. Dabei handelt es sich um eine ganze Reihe von Technologien, und es gibt keine Möglichkeit, Sie in einem angemessenen Paket zu verpacken. Bis jetzt.

Native Cloud-Anwendungspakete (cnabs) sind ein gemeinsamer Aufwand für eine Reihe von communityorientierten Unternehmen wie Microsoft, docker und hashicorp, um eine Spezifikation für das Verpacken von Paketen verteilter Anwendungen zu entwickeln.

Der Aufwand wurde im Dezember 2018 angekündigt. es gibt also noch eine Menge Arbeit, um den Aufwand für die größere Community offenzulegen. Es gibt jedoch bereits eine [offene Spezifikation](https://github.com/deislabs/cnab-spec) und eine Referenz Implementierung, die als [Duffle](https://duffle.sh/)bezeichnet wird. Dieses Tool, das in go geschrieben wurde, ist ein gemeinsamer Aufwand zwischen docker und Microsoft.

Die cnabs können unterschiedliche Arten von Installationstechnologien enthalten. Dadurch können z. b. Helm-Diagramme, TERRAFORM-Vorlagen und ansible-Playbooks im selben Paket nebeneinander vorhanden sein. Nachdem die Pakete erstellt wurden, sind Sie eigenständig und portabel. Sie können über einen USB-Stick installiert werden.  Die Pakete werden kryptografisch signiert, um sicherzustellen, dass Sie von der Partei stammen, die Sie beanspruchen.

Der Kern von cnab ist eine Datei mit dem Namen `bundle.json`. Mit dieser Datei werden die Inhalte des Bündels definiert, also "TERRAFORM" oder "Images" oder etwas anderes. In Abbildung 11-9 wird ein cnab-Element definiert, das eine TERRAFORM aufruft. Beachten Sie jedoch, dass es tatsächlich ein Aufruf Bild definiert, das zum Aufrufen von TERRAFORM verwendet wird. Beim Verpacken wird die Docker-Datei, die sich im *cnab* -Verzeichnis befindet, in ein docker-Image integriert, das im Paket enthalten ist. Wenn TERRAFORM innerhalb eines docker-Containers im Paket installiert ist, bedeutet dies, dass Benutzer nicht auf Ihrem Computer TERRAFORM installieren müssen, um die Bündelung auszuführen.

```json
{
    "name": "terraform",
    "version": "0.1.0",
    "schemaVersion": "v1.0.0-WD",
    "parameters": {
        "backend": {
            "type": "boolean",
            "defaultValue": false,
            "destination": {
                "env": "TF_VAR_backend"
            }
        }
    },
    "invocationImages": [
        {
        "imageType": "docker",
        "image": "cnab/terraform:latest"
        }
    ],
    "credentials": {
        "tenant_id": {
            "env": "TF_VAR_tenant_id"
        },
        "client_id": {
            "env": "TF_VAR_client_id"
        },
        "client_secret": {
            "env": "TF_VAR_client_secret"
        },
        "subscription_id": {
            "env": "TF_VAR_subscription_id"
        },
        "ssh_authorized_key": {
            "env": "TF_VAR_ssh_authorized_key"
        }
    },
    "actions": {
        "status": {
            "modifies": true
        }
    }
}
```

**Abbildung 11-13** : ein Beispiel für eine TERRAFORM-Datei

Der `bundle.json` definiert auch eine Reihe von Parametern, die in TERRAFORM übermittelt werden. Die Parametrisierung des Pakets ermöglicht die Installation in einer Vielzahl unterschiedlicher Umgebungen.

Das cnab-Format ist auch flexibel, sodass es für jede beliebige Cloud verwendet werden kann. Sie kann sogar für lokale Lösungen wie [openstack](https://www.openstack.org/)verwendet werden.

## <a name="devops-decisions"></a>Devops-Entscheidungen

Heutzutage gibt es viele großartige Tools im devops-Raum und sogar noch mehr fantastische Bücher und Beiträge zum Erfolg. Ein bevorzugtes Buch für den Einstieg in die devops-Reise ist [das Phoenix-Projekt](https://www.oreilly.com/library/view/the-phoenix-project/9781457191350/), das die Transformation eines fiktiven Unternehmens von noops zu devops befolgt. Ein wichtiger Punkt ist, dass devops bei der Bereitstellung komplexer, nativer Cloud-Anwendungen nicht mehr "schön ist". Dies ist eine Anforderung und sollte am Anfang jedes Projekts geplant und erstellt werden.

>[!div class="step-by-step"]
>[Vorheriges](infrastructure-as-code.md)
