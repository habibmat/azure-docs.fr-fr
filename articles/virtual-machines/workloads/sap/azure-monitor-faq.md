---
title: FAQ - Azure Monitor pour solutions SAP | Microsoft Docs
description: Dans cet article, découvrez les réponses aux questions fréquentes sur Azure Monitor pour solutions SAP.
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/30/2020
ms.author: radeltch
ms.openlocfilehash: f70aa684a089e0f21cb50932e212b71f77df0e01
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90987242"
---
# <a name="azure-monitor-for-sap-solutions-faq-preview"></a>Questions fréquentes (FAQ) sur Azure Monitor pour solutions SAP (préversion)
## <a name="frequently-asked-questions"></a>Forum aux questions

Cet article offre des réponses aux questions fréquentes sur Azure Monitor pour solutions SAP.  

 - **Dois-je payer pour Azure Monitor pour solutions SAP ?**  
Il n’y a pas de frais de licence pour Azure Monitor pour solutions SAP.  
Toutefois, les clients sont chargés de supporter le coût des composants du groupe de ressources managé.  

 - **Dans quelles régions ce service est-il disponible pour la préversion publique ?**  
Pour la préversion publique, ce service sera disponible dans les régions USA Est 2, USA Ouest 2, USA Est et Europe Ouest.  

 - **Dois-je fournir des autorisations pour permettre le déploiement d’un groupe de ressources managé dans mon abonnement ?**  
Non, des autorisations explicites ne sont pas requises.  

 - **Où se trouve la machine virtuelle du collecteur ?**  
Au moment de déployer une ressource Azure Monitor pour solutions SAP, nous recommandons aux clients de choisir le même réseau virtuel pour la surveillance des ressources que leur serveur SAP HANA. Par conséquent, il est recommandé d’héberger la machine virtuelle du collecteur sur le même réseau virtuel que le serveur SAP HANA. Si les clients utilisent une base de données non HANA, la machine virtuelle du collecteur se trouve dans le même réseau virtuel que la base de données non HANA.  

 - **Quelles sont les versions de HANA prises en charge ?**  
HANA 1.0 SPS 12 (Rev. 120 ou version ultérieure) et HANA 2.0 SPS03 ou version ultérieure  

 - **Quelles sont les configurations de déploiement HANA prises en charge ?**  
Les configurations suivantes sont prises en charge :
   - Nœud unique (scale-up) et nœuds multiples (scale-out)  
   - Un conteneur de base de données unique (HANA 1.0 SPS 12) et des conteneurs de base de données multiples (HANA 1.0 SPS 12 ou HANA 2.0)  
   - Basculement d’hôte automatique (n + 1) et HSR  

 - **Quelles versions de SQL Server sont prises en charge ?**  
SQL Server 2012 SP4 ou ultérieur.  

 - **Quelles sont les configurations de SQL Server prises en charge ?**  
Les configurations suivantes sont prises en charge :
   - Les instances par défaut ou autonomes nommées dans une machine virtuelle  
   - Les instances en cluster ou dans une configuration AlwaysOn lors de l’utilisation du nom virtuel de la ressource de cluster ou du nom d’écouteur AlwaysOn. Actuellement, aucune métrique propre au cluster ou à la configuration AlwaysOn n’est collectée    
   - Actuellement, Azure SQL Database (PaaS) n’est pas pris en charge  

 - **Que se passe-t-il si je supprime accidentellement le groupe de ressources managé ?**  
Le groupe de ressources managé est verrouillé par défaut. Par conséquent, les risques de supprimer accidentellement le groupe de ressources managé par les clients sont minuscules.  
Si un client supprime le groupe de ressources managé, Azure Monitor pour solutions SAP cessera de fonctionner. Le client devra déployer une nouvelle ressource Azure Monitor pour solutions SAP et recommencer.  

 - **De quels rôles ai-je besoin dans mon abonnement Azure pour déployer des ressources Azure Monitor pour solutions SAP ?**  
Le rôle Contributeur.  

 - **Quel est le contrat SLA sur ce produit ?**  
Les préversions sont exclues des contrats de niveau de service. Veuillez lire le contrat de licence complet via l’image Azure Monitor pour solutions SAP de la Place de marché.  

 - **Puis-je surveiller l’intégralité de son paysage avec cette solution ?**  
Vous pouvez actuellement surveiller la base de données HANA, l’infrastructure sous-jacente, le cluster à haute disponibilité et Microsoft SQL Server en préversion publique.  

 - **Ce service remplace-t-il le gestionnaire de solutions SAP ?**  
Non. Les clients peuvent toujours utiliser le gestionnaire de solutions SAP pour l’analyse des processus d’entreprise.  

 - **Quelle est la valeur de ce service par rapport aux solutions traditionnelles comme SAP HANA Cockpit/Studio ?**  
Azure Monitor pour solutions SAP n’est pas spécifique à la base de données HANA. Azure Monitor pour solutions SAP prend également en charge AnyDB.  

## <a name="next-steps"></a>Étapes suivantes

- Créez votre première ressource Azure Monitor pour solutions SAP.