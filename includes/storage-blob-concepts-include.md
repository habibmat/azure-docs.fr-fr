---
title: Fichier Include
description: Fichier Include
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/05/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 7c3438631dca921989309bb8701e113cb5ce3ff2
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89569984"
---
Le stockage Blob Azure est la solution de stockage d’objet de Microsoft pour le cloud. Le stockage Blob est optimisé pour stocker de grandes quantités de données non structurées. Les données non structurées sont des données qui n’obéissent pas à un modèle ou une définition de données en particulier, comme des données texte ou binaires.

## <a name="about-blob-storage"></a>À propos du stockage Blob

Le stockage Blob est conçu pour :

* Mise à disposition d’images ou de documents directement dans un navigateur.
* Stockage de fichiers pour un accès distribué.
* Diffusion en continu de vidéo et d’audio.
* Écriture dans les fichiers journaux.
* Stockage de données pour la sauvegarde et la restauration, la récupération d’urgence et l’archivage.
* Stockage des données pour l’analyse par un service local ou hébergé par Azure.

Les utilisateurs ou les applications clientes peuvent accéder aux objets du stockage Blob via HTTP/HTTPS, où qu’ils se trouvent dans le monde. Les objets du stockage Blob sont accessibles via l’[API REST Stockage Azure](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api), [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.storage), [Azure CLI](https://docs.microsoft.com/cli/azure/storage) ou une bibliothèque de client Stockage Azure. Les bibliothèques clientes sont disponibles pour différents langages, notamment :

* [.NET](/dotnet/api/overview/azure/storage?view=azure-dotnet)
* [Java](https://docs.microsoft.com/java/api/overview/azure/storage)
* [Node.JS](https://azure.github.io/azure-storage-node)
* [Python](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-python)
* [Go](https://github.com/azure/azure-storage-blob-go/)
* [PHP](https://azure.github.io/azure-storage-php/)
* [Ruby](https://azure.github.io/azure-storage-ruby)

## <a name="about-azure-data-lake-storage-gen2"></a>À propos d’Azure Data Lake Storage Gen2

Le stockage Blob prend en charge Azure Data Lake Storage Gen2, solution d’analytique de Big Data d’entreprise de Microsoft pour le cloud. Azure Data Lake Storage Gen2 offre un système de fichiers hiérarchique ainsi que les avantages du stockage Blob, notamment :

* Stockage hiérarchisé à faible coût
* Haute disponibilité
* Cohérence forte
* Fonctionnalités de reprise d’activité

Pour plus d’informations sur Data Lake Storage Gen2, consultez [Présentation d’Azure Data Lake Storage Gen2](../articles/storage/data-lake-storage/introduction.md).
