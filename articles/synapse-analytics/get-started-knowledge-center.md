---
title: 'Tutoriel : Commencer à explorer le centre des connaissances Synapse'
description: Dans ce tutoriel, découvrez comment utiliser le centre des connaissances Synapse.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 09/15/2020
ms.openlocfilehash: c01d1bcb682a5f711dcba3cc7b32ef69b2642ef6
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90900761"
---
# <a name="explore-the-synapse-knowledge-center"></a>Explorer le centre des connaissances Synapse

Dans ce tutoriel, découvrez comment utiliser le centre des connaissances Synapse Studio.

## <a name="getting-to-the-knowledge-center"></a>Accès au centre des connaissances

Il existe deux façons d’accéder au centre des connaissances dans Synapse Studio :

  1. Dans le hub Accueil, sous Liens utiles, cliquez sur le premier lien appelé **Centre des connaissances**.
  2. Dans la barre de menu située en haut, cliquez sur **?** , puis sur **Centre des connaissances**.

Choisissez l’une des méthodes et ouvrez le **centre des connaissances**.

## <a name="overview"></a>Vue d’ensemble

Le **centre des connaissances** vous permet d’effectuer trois opérations :
* **Use samples immediately (Utiliser des exemples immédiatement)** . Cette option est optimisée pour vous permettre de voir les analytiques en action aussi rapidement que possible. Si vous souhaitez avoir un exemple rapide du fonctionnement de Synapse, choisissez cette option.
* **Browser available sample (Parcourir les exemples disponibles)** . Cette option vous permet de lier des exemples de jeux de données et d’ajouter des exemples de code sous la forme de scripts SQL, de notebooks et de pipelines.
* **Tour Synapse studio (Visiter Synapse Studio)** . Cette option vous guide dans une brève présentation des composants de base de Synapse Studio. Elle est utile si vous n’avez jamais utilisé Synapse Studio.

## <a name="exploring-blob-storage-with-sql-on-demand"></a>Exploration du stockage blob avec SQL à la demande

1. Dans le **centre des connaissances**, cliquez sur **Use samples immediately (Utiliser des exemples immédiatement)**
1. Sélectionnez **Query data with SQL (Interroger des données avec SQL)** . 
1. Cliquez sur **Use samples immediately (Utiliser des exemples immédiatement)** .
1. Un script SQL est alors créé.
1. Faites défiler jusqu’à la première requête (lignes 28 à 32) et sélectionnez le texte de la requête.
1. Cliquez sur Exécuter. Le texte que vous avez sélectionné est exécuté.

## <a name="loading-more-nyc-taxi-data"></a>Chargement d’autres données sur les taxis new-yorkais

1. Dans le **centre des connaissances**, cliquez sur **Browse available samples (Parcourir les exemples disponibles)** . 
1. Sélectionnez l’onglet **SQL scripts (Scripts SQL)** en haut.
1. Sélectionnez **Load the New York Taxicab dataset (Charger le jeu de données New York Taxicab)** .
1. Sous **Inputs (Entrées)** , choisissez **Select an existing pool (Sélectionner un pool existant)** et sélectionnez **SQLDB1**.
1. Cliquez sur **Open Script (Ouvrir le script)** .
1. Un nouveau script SQL s’affiche.
1. Cliquez sur **Exécuter**.
1. Cette opération crée plusieurs tables pour toutes les données de taxi de New York et les charge à l’aide de la commande de copie T-SQL COPY.

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Analyser avec un pool SQL](get-started-analyze-sql-pool.md)
