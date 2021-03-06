---
title: Concepts relatifs aux conversations dans Azure Communication Services
titleSuffix: An Azure Communication Services concept document
description: Découvrez les concepts relatifs aux conversations dans Communication Services.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: af07894fcbfae386849d32492be9d2718a3adcc3
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90944372"
---
# <a name="chat-concepts"></a>Concepts relatifs aux conversations

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]

Les bibliothèques de client pour les conversations dans Azure Communication Services peuvent être utilisées pour ajouter la fonctionnalité de conversation textuelle en temps réel à vos applications. Cette page récapitule les principaux concepts et fonctionnalités relatifs aux conversations.

Pour en savoir plus sur les langages et les fonctionnalités de la bibliothèque de client, consultez [Présentation de la bibliothèque de client pour les conversations dans Communication Services](./sdk-features.md).

## <a name="chat-overview"></a>Présentation de la fonctionnalité de conversation 

Les conversations ont lieu dans des fils de conversation. Un fil de conversation peut contenir de nombreux messages et utilisateurs. Chaque message appartient à un seul fil et un utilisateur peut faire partie d’un ou de plusieurs fils. 

Les utilisateurs d’un fil de conversation en sont des « membres ». Un fil de conversation peut comprendre jusqu’à 250 membres. Seuls les membres du fil peuvent envoyer et recevoir des messages, ainsi qu’ajouter ou supprimer des membres dans le fil de conversation. La taille maximale autorisée pour un message est d’environ 28 Ko. Communication Services stocke l’historique des conversations jusqu’à ce que vous exécutiez une opération de suppression sur le fil de conversation. Vous pouvez récupérer tous les messages d’un fil de conversation à l’aide de l’opération `List/Get Messages`.

Pour les fils de conversation comprenant plus de 20 membres, les confirmations de lecture et l’indicateur de saisie sont désactivés. 

## <a name="chat-architecture"></a>Architecture des conversations

L’architecture des conversations comporte deux parties principales : 1) un service approuvé et 2) une application cliente.

:::image type="content" source="../../media/chat-architecture.png" alt-text="Diagramme montrant l’architecture des conversations dans Communication Services":::

 - **Service approuvé :** Pour gérer correctement une session de conversation, vous avez besoin d’un service qui vous permette de vous connecter à Communication Services à l’aide de votre chaîne de connexion de ressource. Ce service est chargé de créer des fils de conversation, de gérer les appartenances aux fils et de fournir des jetons d’accès aux utilisateurs. Pour plus d’informations sur l’obtention des jetons d’accès, consultez notre guide de démarrage rapide sur les [jetons d’accès](../../quickstarts/access-tokens.md).

 - **Application cliente :**  L’application cliente se connecte à votre service approuvé et reçoit les jetons d’accès qui sont utilisés pour se connecter directement à Communication Services. Une fois cette connexion établie, votre application cliente peut envoyer et recevoir des messages.
    
## <a name="message-types"></a>Types de messages

La conversation Communication Services partage les messages générés par l’utilisateur, ainsi que les messages générés par le système, qui sont appelés **activités de fil**. Les activités de fil sont générées lors de la mise à jour d’un fil de conversation. Lorsque vous appelez `List Messages` ou `Get Messages` sur un fil de conversation, le résultat contient les messages texte générés par l’utilisateur, ainsi que les messages système, classés par ordre chronologique. Cela vous permet de savoir à quel moment un membre a été ajouté ou supprimé, ainsi que la date de mise à jour du sujet du fil de conversation. Les types de messages pris en charge sont les suivants :  

 - `Text`: Message réel composé et envoyé par l’utilisateur dans le cadre d’une conversation 
 - `ThreadActivity/AddMember`: Message système qui indique qu’un ou plusieurs membres ont été ajoutés au fil de conversation Par exemple :

```xml

<addmember>
    <eventtime>1598478187549</eventtime>
    <initiator>8:acs:57b9bac9-df6c-4d39-a73b-26e944adf6ea_0e59221d-0c1d-46ae-9544-c963ce56c10b</initiator>
    <detailedinitiatorinfo>
        <friendlyName>User 1</friendlyName>
    </detailedinitiatorinfo>
    <rosterVersion>1598478184564</rosterVersion>
    <target>8:acs:57b9bac9-df6c-4d39-a73b-26e944adf6ea_0e59221d-0c1d-46ae-9544-c963ce56c10b</target>
    <detailedtargetinfo>
        <id>8:acs:57b9bac9-df6c-4d39-a73b-26e944adf6ea_0e59221d-0c1d-46ae-9544-c963ce56c10b</id>
        <friendlyName>User 1</friendlyName>
    </detailedtargetinfo>
    <target>8:acs:57b9bac9-df6c-4d39-a73b-26e944adf6ea_8540c0de-899f-5cce-acb5-3ec493af3800</target>
    <detailedtargetinfo>
        <id>8:acs:57b9bac9-df6c-4d39-a73b-26e944adf6ea_8540c0de-899f-5cce-acb5-3ec493af3800</id>
        <friendlyName>User 2</friendlyName>
    </detailedtargetinfo>
</addmember>

```  

- `ThreadActivity/DeleteMember`: Message système qui indique qu’un membre a été supprimé du fil de conversation Par exemple :

```xml

<deletemember>
    <eventtime>1598478187642</eventtime>
    <initiator>8:acs:57b9bac9-df6c-4d39-a73b-26e944adf6ea_0e59221d-0c1d-46ae-9544-c963ce56c10b</initiator>
    <detailedinitiatorinfo>
        <friendlyName>User 1</friendlyName>
    </detailedinitiatorinfo>
    <rosterVersion>1598478184564</rosterVersion>
    <target>8:acs:57b9bac9-df6c-4d39-a73b-26e944adf6ea_8540c0de-899f-5cce-acb5-3ec493af3800</target>
    <detailedtargetinfo>
        <id>8:acs:57b9bac9-df6c-4d39-a73b-26e944adf6ea_8540c0de-899f-5cce-acb5-3ec493af3800</id>
        <friendlyName>User 2</friendlyName>
    </detailedtargetinfo>
</deletemember>

```

- `ThreadActivity/TopicUpdate`: Message système qui indique que le sujet a été mis à jour Par exemple :

```xml

<topicupdate>
    <eventtime>1598477591811</eventtime>
    <initiator>8:acs:57b9bac9-df6c-4d39-a73b-26e944adf6ea_0e59221d-0c1d-46ae-9544-c963ce56c10b</initiator>
    <value>New topic</value>
</topicupdate>

```

## <a name="real-time-signaling"></a>Signalisation en temps réel 

La bibliothèque de client pour les conversations dans JavaScript comprend la fonctionnalité de signalisation en temps réel. Cela permet aux clients d’écouter des mises à jour en temps réel et des messages entrants dans un thread de conversation sans avoir à interroger les API. Les événements disponibles sont les suivants :

 - `ChatMessageReceived` - Quand un nouveau message est envoyé à un thread de conversation dont l’utilisateur est membre. Cet événement n’est pas envoyé pour les messages système générés automatiquement, comme nous l’avons vu dans la rubrique précédente.  
 - `ChatMessageEdited` - Quand un message est modifié dans un thread de conversation dont l’utilisateur est membre. 
 - `ChatMessageDeleted` - Quand un message est supprimé dans un thread de conversation dont l’utilisateur est membre. 
 - `TypingIndicatorReceived` - Quand un autre membre est en train de taper un message dans le thread de conversation dont l’utilisateur est membre. 
 - `ReadReceiptReceived` - Quand un autre membre a lu le message envoyé par l’utilisateur dans un thread de conversation. 

## <a name="chat-events"></a>Événements de conversation 

La signalisation en temps réel permet à vos utilisateurs de discuter en temps réel. Vos services peuvent utiliser Azure Event Grid pour s’abonner à des événements liés à une conversation. Pour plus d’informations, consultez [Concepts relatifs à la gestion des événements](../event-handling.md).

## <a name="using-cognitive-services-with-chat-client-library-to-enable-intelligent-features"></a>Utilisation de Cognitive Services avec la bibliothèque de client des conversations pour activer les fonctionnalités intelligentes

Vous pouvez utiliser les [API Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/) avec la bibliothèque de client des conversations pour ajouter des fonctionnalités intelligentes à vos applications. Par exemple, vous pouvez :

- Permettre aux utilisateurs de discuter les uns avec les autres dans différentes langues 
- Aider un agent de support à hiérarchiser les tickets en détectant un sentiment négatif dans les problèmes client
- Analyser les messages entrants à des fins de détection des clés et de reconnaissance d’entité, et demander des informations pertinentes à l’utilisateur dans votre application en fonction du contenu du message

Pour ce faire, vous pouvez faire en sorte que votre service approuvé agisse en tant que membre d’un fil de conversation. Supposons que vous souhaitiez activer la fonctionnalité de traduction. Ce service est chargé d’écouter les messages échangés par d’autres membres [1], d’appeler les API cognitives pour traduire le contenu vers la langue souhaitée [2, 3] et d’envoyer le résultat traduit sous la forme d’un message dans le fil de conversation [4]. 

De cette façon, l’historique des messages contient à la fois les messages d’origine et les messages traduits. Dans l’application cliente, vous pouvez ajouter une logique pour afficher le message d’origine ou le message traduit. Pour savoir comment utiliser les API cognitives dans le but de traduire du texte en différentes langues, consultez [ce guide de démarrage rapide](https://docs.microsoft.com/azure/cognitive-services/translator/quickstart-translate). 

:::image type="content" source="../media/chat/cognitive-services.png" alt-text="Diagramme montrant Cognitive Services qui interagit avec Communication Services":::

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Bien démarrer avec les conversations](../../quickstarts/chat/get-started.md)

Les documents suivants peuvent vous intéresser :

- Familiarisez-vous avec la [bibliothèque de client des conversations](sdk-features.md)
