---
title: 'Vorgehensweise: Konfigurieren eines lokalen Ausstellers'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, federation
- federation
ms.assetid: 15263371-514e-4ea6-90fb-14b4939154cd
ms.openlocfilehash: 0028a0522447588ee0fb183b5b2f93d334a7b2b2
ms.sourcegitcommit: a97ecb94437362b21fffc5eb3c38b6c0b4368999
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68972071"
---
# <a name="how-to-configure-a-local-issuer"></a>Vorgehensweise: Konfigurieren eines lokalen Ausstellers

In diesem Thema wird beschrieben, wie Sie einen Client für die Verwendung eines lokalen Ausstellers für ausgestellte Token konfigurieren.

Wenn ein Client mit einem Verbunddienst kommuniziert, wird vom Dienst die Adresse des Sicherheitstokendiensts angegeben, der das Token ausgeben soll, das vom Client für die Authentifizierung am Verbunddienst verwendet werden soll. In bestimmten Situationen kann der Client für die Verwendung eines *lokalen Ausstellers*konfiguriert werden.

Windows Communication Foundation (WCF) verwendet einen lokalen Aussteller in Fällen, in denen die Aussteller Adresse einer Verbund Bindung oder `http://schemas.microsoft.com/2005/12/ServiceModel/Addressing/Anonymous` `null`ist. In diesem Fall müssen Sie <xref:System.ServiceModel.Description.ClientCredentials> mit der Adresse des lokalen Ausstellers und der für die Kommunikation mit diesem Aussteller zu verwendenden Bindung konfigurieren.

> [!NOTE]
> Wenn die <xref:System.ServiceModel.Description.ClientCredentials.SupportInteractive%2A> -Eigenschaft `ClientCredentials` der-Klasse auf `true`festgelegt ist, wird keine lokale Aussteller Adresse angegeben [ \<, und die vom WSFederationHttpBinding->](../../../../docs/framework/configure-apps/file-schema/wcf/wsfederationhttpbinding.md) oder einer anderen Verbund Bindung angegebene Aussteller Adresse ist `http://schemas.xmlsoap.org/ws/2005/05/identity/issuer/self` ,`http://schemas.microsoft.com/2005/12/ServiceModel/Addressing/Anonymous`oder ist`null`, dann wird der Windows CardSpace-Aussteller verwendet.

## <a name="to-configure-the-local-issuer-in-code"></a>So konfigurieren Sie den lokalen Aussteller im Code

1. Erstellen Sie eine Variable vom Typ <xref:System.ServiceModel.Security.IssuedTokenClientCredential>.

2. Legen Sie die Variable der Instanz fest, die von der <xref:System.ServiceModel.Description.ClientCredentials.IssuedToken%2A>-Eigenschaft der `ClientCredentials`-Klasse zurückgegeben wird. Diese Instanz wird von der <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A>-Eigenschaft des Clients (geerbt von <xref:System.ServiceModel.ClientBase%601>) oder der <xref:System.ServiceModel.ChannelFactory.Credentials%2A>-Eigenschaft der <xref:System.ServiceModel.ChannelFactory> zurückgegeben:

     [!code-csharp[c_CreateSTS#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#9)]
     [!code-vb[c_CreateSTS#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#9)]

3. Legen Sie die <xref:System.ServiceModel.Security.IssuedTokenClientCredential.LocalIssuerAddress%2A>-Eigenschaft auf eine neue Instanz der <xref:System.ServiceModel.EndpointAddress> fest, wobei die Adresse des lokalen Ausstellers als Argument für den Konstruktor verwendet wird.

     [!code-csharp[c_CreateSTS#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#10)]
     [!code-vb[c_CreateSTS#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#10)]

     Sie können auch eine neue <xref:System.Uri>-Instanz als Argument für den Konstruktor erstellen.

     [!code-csharp[c_CreateSTS#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#11)]
     [!code-vb[c_CreateSTS#11](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#11)]

     Der `addressHeaders` -Parameter ist ein Array <xref:System.ServiceModel.Channels.AddressHeader> von-Instanzen, wie gezeigt.

     [!code-csharp[c_CreateSTS#12](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#12)]
     [!code-vb[c_CreateSTS#12](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#12)]

4. Legen Sie die Bindung für den lokalen Aussteller mithilfe <xref:System.ServiceModel.Security.IssuedTokenClientCredential.LocalIssuerBinding%2A> der-Eigenschaft fest.

     [!code-csharp[c_CreateSTS#13](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#13)]
     [!code-vb[c_CreateSTS#13](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#13)]

5. Optional. Fügen Sie die konfigurierten Endpunktverhalten für den lokalen Aussteller hinzu, indem Sie solche Verhalten der Auflistung hinzufügen, die von der <xref:System.ServiceModel.Security.IssuedTokenClientCredential.LocalIssuerChannelBehaviors%2A>-Eigenschaft zurückgegeben wird.

     [!code-csharp[c_CreateSTS#14](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#14)]
     [!code-vb[c_CreateSTS#14](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#14)]

## <a name="to-configure-the-local-issuer-in-configuration"></a>So konfigurieren Sie den lokalen Aussteller in der Konfiguration

1. Erstellen Sie ein [ \<localIssuer >](../../../../docs/framework/configure-apps/file-schema/wcf/localissuer.md) -Element als unter [ \<geordnetes Element des IssuedToken->](../../../../docs/framework/configure-apps/file-schema/wcf/issuedtoken.md) Elements, das selbst ein untergeordnetes Element des [ \<Clientanmelde->](../../../../docs/framework/configure-apps/file-schema/wcf/clientcredentials.md) -Elements in einem Endpunkt Verhalten ist.

2. Legen Sie das `address`-Attribut auf die Adresse des lokalen Ausstellers fest, der Tokenanforderungen annimmt.

3. Legen Sie das `binding`-Attribut und das `bindingConfiguration`-Attribut auf Werte fest, mit denen die für die Kommunikation mit dem lokalen Ausstellerendpunkt geeignete Bindung referenziert wird.

4. Optional. Legen Sie das [ \<Identity >](../../../../docs/framework/configure-apps/file-schema/wcf/identity.md) -Element als untergeordnetes Element`localIssuer`des < >-Elements fest, und geben Sie Identitätsinformationen für den lokalen Aussteller an.

5. Optional. Legen Sie die [ \<Header >](../../../../docs/framework/configure-apps/file-schema/wcf/headers.md) -Elements als untergeordnetes Element`localIssuer`des < >-Elements fest, und geben Sie zusätzliche Header an, die erforderlich sind, um den lokalen Aussteller ordnungsgemäß zu adressieren.

## <a name="net-framework-security"></a>.NET Framework-Sicherheit

Beachten Sie, dass beim Angeben einer Ausstelleradresse und einer Ausstellerbindung für eine bestimmte Bindung nicht der lokale Aussteller für Endpunkte verwendet wird, die diese Bindung verwenden. Clients, die immer den lokalen Aussteller verwenden möchten, sollten sicherstellen, dass keine solche Bindung verwendet wird oder dass die Bindung so geändert wird, dass die Ausstelleradresse `null` lautet.

## <a name="see-also"></a>Siehe auch

- [Vorgehensweise: Konfigurieren von Anmelde Informationen für eine Verbunddienst](../../../../docs/framework/wcf/feature-details/how-to-configure-credentials-on-a-federation-service.md)
- [Vorgehensweise: Erstellen eines Verbund Clients](../../../../docs/framework/wcf/feature-details/how-to-create-a-federated-client.md)
- [Vorgehensweise: Erstellen einer WSFederationHttpBinding](../../../../docs/framework/wcf/feature-details/how-to-create-a-wsfederationhttpbinding.md)
