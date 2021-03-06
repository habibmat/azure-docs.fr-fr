---
title: Métriques pour Azure NetApp Files | Microsoft Docs
description: Azure NetApp Files fournit des métriques sur le stockage alloué, l’utilisation réelle du stockage, les IOPS du volume et la latence. Utilisez ces métriques pour comprendre l’utilisation et les performances.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/10/2020
ms.author: b-juche
ms.openlocfilehash: 1690a844ff700a2975be8e972fd90ba71eeb937c
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90707779"
---
# <a name="metrics-for-azure-netapp-files"></a>Métriques pour Azure NetApp Files

Azure NetApp Files fournit des métriques sur le stockage alloué, l’utilisation réelle du stockage, les IOPS du volume et la latence. En analysant ces métriques, vous en saurez plus sur le mode d'utilisation et les performances des volumes de vos comptes NetApp.  

## <a name="usage-metrics-for-capacity-pools"></a><a name="capacity_pools"></a>Métriques d'utilisation des pools de capacités

- *Taille allouée au pool*   
    Taille provisionnée du pool.

- *Pool alloué à la taille du volume*  
    Quota total des volumes (en Gio) d’un pool de capacités donné (autrement dit, total des tailles provisionnées pour les volumes du pool de capacités).  
    Il s’agit de la taille que vous avez sélectionnée lors de la création des volumes.  

- *Taille du pool consommée*  
    Espace logique total (en Gio) utilisé sur l’ensemble des volumes d’un pool de capacités.  

- *Taille totale des instantanés du pool*    
    La somme de la taille des instantanés de tous les volumes du pool.

## <a name="usage-metrics-for-volumes"></a><a name="volumes"></a>Métriques d'utilisation des volumes

<!--
- *Volume Quota Size*    
    The quota size (GiB) the volume is provisioned with.   
    This size is the size you selected during capacity pool creation. 
-->
-  *Taille du volume consommée*  
    Espace logique total utilisé dans un volume (en Gio).  
    Cette taille inclut l'espace logique utilisé par les captures instantanées et les systèmes de fichiers actifs.  
- *Taille du cliché instantané de volume*   
   Espace logique incrémentiel utilisé par les clichés instantanés d’un volume.  

## <a name="performance-metrics-for-volumes"></a>Métriques de performances des volumes

- *Latence de lecture moyenne*   
    Temps moyen des lectures du volume (en millisecondes).
- *Latence d’écriture moyenne*   
    Temps moyen des écritures du volume (en millisecondes).
- *E/S par seconde en lecture*   
    Nombre de lectures sur le volume par seconde.
- *E/S par seconde en écriture*   
    Nombre d’écritures sur le volume par seconde.

## <a name="volume-replication-metrics"></a><a name="replication"></a>Métriques de réplication de volume

- *L’état de la réplication de volume est-il sain*   
    Condition de la relation de réplication. 

- *La réplication de volume est-elle en cours de transfert*    
    L’état de la réplication de volume est-il « En cours de transfert ». 
 
- *Décalage de la réplication de volume*   
    Durée, en secondes, du retard des données présentes sur le miroir par rapport à la source 

- *Durée du dernier transfert de réplication de volume*   
    Durée, en secondes, nécessaire au dernier transfert 

- *Taille du dernier transfert de réplication de volume*    
    Nombre total d’octets transférés dans le cadre du dernier transfert 

- *Progression de la réplication de volume*    
    Le volume total de données transférées pour l’opération de transfert en cours. 

- *Transfert total de la réplication de volume*   
    Les octets cumulés transférés pour la relation. 

## <a name="next-steps"></a>Étapes suivantes

* [Comprendre la hiérarchie de stockage d’Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
* [Configurer un pool de capacité](azure-netapp-files-set-up-capacity-pool.md)
* [Créer un volume pour Azure NetApp Files](azure-netapp-files-create-volumes.md)
