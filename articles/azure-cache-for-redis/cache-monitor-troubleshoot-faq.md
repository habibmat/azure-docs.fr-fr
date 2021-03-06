---
title: FAQ sur l’analyse et la résolution des problèmes pour Azure Cache pour Redis
description: Découvrez les réponses aux questions fréquentes qui vous aideront à analyser et à résoudre les problèmes d’Azure Cache pour Redis
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 08/06/2020
ms.openlocfilehash: 2149d069ce9cb0b636e461d19963f413b162bc9d
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88010757"
---
# <a name="azure-cache-for-redis-monitoring-and-troubleshooting-faqs"></a>FAQ sur l’analyse et la résolution des problèmes pour Azure Cache pour Redis
Cet article fournit des réponses aux questions fréquentes sur l’analyse et la résolution des problèmes pour Azure Cache pour Redis.

## <a name="common-questions-and-answers"></a>Questions et réponses courantes
Cette section aborde les FAQ suivantes :

* [Comment surveiller l’intégrité et les performances de mon cache ?](#how-do-i-monitor-the-health-and-performance-of-my-cache)
* [Pourquoi est-ce que je reçois des erreurs d’expiration du délai ?](#why-am-i-seeing-timeouts)
* [Pourquoi mon client a-t-il été déconnecté du cache ?](#why-was-my-client-disconnected-from-the-cache)

### <a name="how-do-i-monitor-the-health-and-performance-of-my-cache"></a>Comment surveiller l’intégrité et les performances de mon cache ?
Les instances du Cache Microsoft Azure pour Redis peuvent être surveillées dans le [portail Azure](https://portal.azure.com). Vous pouvez afficher les mesures, épingler des graphiques de mesure au Tableau d’accueil, personnaliser la plage de date et d’heure des graphiques de surveillance, ajouter et supprimer des mesures dans les graphiques et définir des alertes lorsque certaines conditions sont remplies. Pour plus d’informations, voir [Surveillance du Cache Azure pour Redis](cache-how-to-monitor.md).

Le **menu Ressource** du Cache Azure pour Redis contient également plusieurs outils permettant de surveiller et dépanner vos caches.

* **Diagnostiquer et résoudre les problèmes** fournit des informations sur les problèmes courants et les stratégies de résolutions associées.
* **Resource Health** surveille vos ressources et vous indique si elles s’exécutent comme prévu. Pour plus d’informations sur le service Azure Resource Health, consultez [Vue d’ensemble d’Azure Resource Health](../resource-health/resource-health-overview.md).
* **Nouvelle demande de support** fournit des options pour ouvrir une demande de support pour votre cache.

Ces outils vous permettent de surveiller l’intégrité de vos instances de cache Azure pour Redis et vous aident à gérer vos applications de mise en cache. Pour plus d’informations, consultez la section « Paramètres de support et dépannage » de l’article [Configuration de Cache Azure pour Redis](cache-configure.md).

### <a name="why-am-i-seeing-timeouts"></a>Pourquoi est-ce que je reçois des erreurs d’expiration du délai ?
L’expiration du délai se produit dans le client que vous utilisez pour communiquer avec Redis. Lorsqu’une commande est envoyée au serveur Redis, elle est placée dans la file d’attente, puis le serveur Redis la récupère et l’exécute. Toutefois, le client peut expirer pendant ce processus. Si c’est le cas, une exception est levée côté appelant. Pour plus d’informations sur la résolution des problèmes de délai d’attente, consultez [Résolution des problèmes côté client](cache-troubleshoot-client.md) et [Exceptions au délai d’expiration de StackExchange.Redis](cache-troubleshoot-timeouts.md#stackexchangeredis-timeout-exceptions).

### <a name="why-was-my-client-disconnected-from-the-cache"></a>Pourquoi mon client a-t-il été déconnecté du cache ?
Voici quelques raisons pour lesquelles la déconnexion du cache peut se produire :

* Causes côté client
  * L’application cliente a été redéployée.
  * L’application cliente a effectué une opération de mise à l’échelle.
    * Dans le cas de Cloud Services ou de Web Apps, cela peut être dû à la mise à l’échelle automatique.
  * La couche réseau côté client a été modifiée.
  * Des erreurs temporaires se sont produites dans le client ou dans les nœuds du réseau entre le client et le serveur.
  * Les limites de seuil de bande passante ont été atteintes.
  * Les opérations côté processeur ont pris trop de temps.
* Causes côté serveur
  * Dans l’offre de cache Standard, le service Cache Azure pour Redis a lancé un basculement du nœud principal vers le nœud de réplica.
  * Azure appliquait un correctif à l’instance où le cache était déployé
    * Il peut s’agir de mises à jour du serveur Redis ou d’une maintenance générale de la machine virtuelle.


## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur la supervision et le dépannage de vos instances du Cache Azure pour Redis, consultez [Surveillance du Cache Azure pour Redis](cache-how-to-monitor.md) et les différents guides de dépannage.

En savoir plus sur d’autres [FAQ sur Azure Cache pour Redis](cache-faq.md).
