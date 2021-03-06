---
title: Analyser et résoudre les problèmes de déconnexion d’Azure IoT Hub
description: Apprendre à analyser et résoudre les erreurs courantes en matière de connectivité des appareils pour Azure IoT Hub
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/30/2020
ms.author: jlian
ms.custom:
- mqtt
- 'Role: Cloud Development'
- 'Role: IoT Device'
- 'Role: Technical Support'
ms.openlocfilehash: b22573fc98d4647c2b05763d9b67c37f6f023f98
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87322718"
---
# <a name="monitor-diagnose-and-troubleshoot-disconnects-with-azure-iot-hub"></a>Analyser et résoudre les problèmes de déconnexion avec Azure IoT Hub

Les problèmes de connectivité des appareils IoT sont parfois difficiles à résoudre, car il existe de nombreux points de défaillance possibles. La logique d’application, les réseaux physiques, les protocoles, le matériel, IoT Hub et d'autres services cloud sont tous susceptibles d’occasionner des problèmes. La possibilité de détecter et d’identifier la source d’un problème est essentielle. Cela étant, une solution IoT à grande échelle peut impliquer des milliers d’appareils et il n'est pas pratique de vérifier chaque appareil individuellement. Pour vous aider à détecter, diagnostiquer et résoudre ces problèmes à grande échelle, utilisez les fonctionnalités de supervision offertes par IoT Hub via Azure Monitor. Ces fonctionnalités se limitant à ce que IoT Hub peut observer, nous vous recommandons également de suivre les meilleures pratiques de supervision pour vos appareils et autres services Azure.

## <a name="get-alerts-and-error-logs"></a>Obtenir des alertes et des journaux d’activité d’erreurs

Utilisez Azure Monitor pour recevoir des alertes et créer des journaux d’activité en cas de déconnexion des appareils.

### <a name="turn-on-diagnostic-logs"></a>Activer les journaux de diagnostic

Pour enregistrer les événements et les erreurs de connexion d’appareil, activez les diagnostics pour IoT Hub. Nous vous recommandons d’activer ces journaux le plus tôt possible, car si les journaux de diagnostic ne sont pas activés, en cas de déconnexion d'appareils, vous ne disposez d’aucune information pour résoudre le problème.

1. Connectez-vous au [portail Azure](https://portal.azure.com).

2. Accédez à votre hub IoT.

3. Sélectionnez **Paramètres de diagnostic**.

4. Sélectionnez **Activer les diagnostics**.

5. Activez la collecte des journaux d’activité **Connexions**.

6. Pour faciliter l’analyse, activez l’option **Envoyer à Log Analytics** ([voir les tarifs](https://azure.microsoft.com/pricing/details/log-analytics/)). Consultez l’exemple sous [Résoudre les erreurs de connectivité](#resolve-connectivity-errors).

   ![Paramètres recommandés](./media/iot-hub-troubleshoot-connectivity/diagnostic-settings-recommendation.png)

Pour plus d’informations, consultez l’article [Surveiller l’intégrité d’Azure IoT Hub et diagnostiquer rapidement les problèmes](iot-hub-monitor-resource-health.md).

### <a name="set-up-alerts-for-device-disconnect-at-scale"></a>Configurer des alertes en cas de déconnexion d'appareils à grande échelle

Pour recevoir des alertes lorsque des appareils se déconnectent, configurez des alertes sur la métrique **Appareils connectés (préversion)** .

1. Connectez-vous au [portail Azure](https://portal.azure.com).

2. Accédez à votre hub IoT.

3. Sélectionnez **Alertes**.

4. Sélectionnez **Nouvelle règle d’alerte**.

5. Sélectionnez **Ajouter une condition**, puis sélectionnez « Appareils connectés (préversion) ».

6. Configurez le seuil et les alertes en suivant les invites.

Pour plus d’informations, consultez [Que sont les alertes dans Microsoft Azure ?](../azure-monitor/platform/alerts-overview.md).

#### <a name="detecting-individual-device-disconnects"></a>Détecter les déconnexions d'appareils individuels

Pour détecter les *déconnexions* par appareil (s'il vous faut savoir qu'une fabrique a été déconnectée, par exemple), [configurez des événements de déconnexion d’appareil avec Event Grid](iot-hub-event-grid.md).

## <a name="resolve-connectivity-errors"></a>Résoudre les erreurs de connectivité

Lorsque vous activez les alertes et les journaux de diagnostic relatifs aux appareils connectés, vous recevez des alertes quand des erreurs se produisent. Cette section décrit comment examiner les problèmes courants lorsque vous recevez une alerte. Pour exécuter la procédure ci-après, vous devez avoir configuré les journaux Azure Monitor pour vos journaux de diagnostic.

1. Connectez-vous au [portail Azure](https://portal.azure.com).

1. Accédez à votre hub IoT.

1. Sélectionnez **Journaux d’activité**.

1. Pour isoler les journaux d’activité d’erreurs de connectivité pour IoT Hub, entrez la requête ci-après, puis sélectionnez **Exécuter** :

    ```kusto
    AzureDiagnostics
    | where ( ResourceType == "IOTHUBS" and Category == "Connections" and Level == "Error")
    ```

1. Si des résultats s’affichent, recherchez les éléments `OperationName`, `ResultType` (code d’erreur) et `ResultDescription` (message d’erreur) pour obtenir plus de détails sur l’erreur.

   ![Exemple de journal des erreurs](./media/iot-hub-troubleshoot-connectivity/diag-logs.png)

1. Suivez les guides de résolution des problèmes pour les erreurs les plus courantes :

    - **[404104 DeviceConnectionClosedRemotely](iot-hub-troubleshoot-error-404104-deviceconnectionclosedremotely.md)**
    - **[401003 IoTHubUnauthorized](iot-hub-troubleshoot-error-401003-iothubunauthorized.md)**
    - **[409002 LinkCreationConflict](iot-hub-troubleshoot-error-409002-linkcreationconflict.md)**
    - **[500001 ServerError](iot-hub-troubleshoot-error-500xxx-internal-errors.md)**
    - **[500008 GenericTimeout](iot-hub-troubleshoot-error-500xxx-internal-errors.md)**

## <a name="i-tried-the-steps-but-they-didnt-work"></a>J’ai suivi les différentes étapes, mais cela n'a pas fonctionné.

Si les étapes ci-dessus n’ont pas permis de résoudre le problème, essayez ce qui suit :

* Si vous avez accès aux appareils problématiques, que ce soit physiquement ou à distance (comme SSH), consultez le [guide de résolution des problèmes côté appareil](https://github.com/Azure/azure-iot-sdk-node/wiki/Troubleshooting-Guide-Devices) pour poursuivre le dépannage.

* Vérifiez que vos appareils présentent l’état **Activé** en accédant au portail Azure > votre hub IoT > Appareils IoT.

* Si votre appareil utilise le protocole MQTT, vérifiez que le port 8883 est ouvert. Pour plus d’informations, consultez [Connexion à IoT Hub (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub).

* Obtenez de l’aide sur [Page de questions Microsoft Q&A sur Azure IoT Hub](https://docs.microsoft.com/answers/topics/azure-iot-hub.html), [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-iot-hub)ou [support Azure](https://azure.microsoft.com/support/options/).

Pour contribuer à améliorer la documentation à l’intention de tous les utilisateurs, laissez un commentaire dans la section des commentaires ci-après si ce guide ne vous a pas aidé.

## <a name="next-steps"></a>Étapes suivantes

* Pour en savoir plus sur la résolution des problèmes temporaires, consultez [Gestion des erreurs temporaires](/azure/architecture/best-practices/transient-faults).

* Pour explorer davantage le SDK Azure IoT et la gestion des nouvelles tentatives, consultez [Guide pratique pour gérer la connectivité et la messagerie fiable à l’aide des kits Azure IoT Hub device SDK](iot-hub-reliability-features-in-sdks.md#connection-and-retry).
