---
title: Configurer les paramètres de serveur - Portail Azure - Azure Database pour PostgreSQL - Serveur unique
description: Cet article décrit comment configurer les paramètres Postgres dans Azure Database pour PostgreSQL par le biais du portail Azure.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: how-to
ms.date: 02/28/2018
ms.openlocfilehash: a2410328b068be6bc5af358fa72ee20eead4c8ed
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90907467"
---
# <a name="configure-server-parameters-in-azure-database-for-postgresql---single-server-via-the-azure-portal"></a>Configurer les paramètres de serveur dans Azure Database pour PostgreSQL (serveur unique) via le portail Azure 
Vous pouvez lister, afficher et mettre à jour les paramètres de configuration d’un serveur Azure Database pour PostgreSQL en utilisant le portail Azure.

## <a name="prerequisites"></a>Prérequis
Pour parcourir ce guide pratique, vous avez besoin des éléments suivants :
- [Un serveur Azure Database pour PostgreSQL](quickstart-create-server-database-portal.md)

## <a name="viewing-and-editing-parameters"></a>Affichage et modification des paramètres
1. Ouvrez le [portail Azure](https://portal.azure.com).

2. Sélectionnez votre serveur Azure Database pour PostgreSQL.

3. Dans la section **Paramètres**, sélectionnez **Paramètres du serveur**. La page affiche une liste de paramètres avec leurs valeurs et leurs descriptions.
:::image type="content" source="./media/howto-configure-server-parameters-in-portal/3-overview-of-parameters.png" alt-text="Page de vue d’ensemble des paramètres":::

4. Sélectionnez le bouton **déroulant** pour afficher les valeurs possibles des paramètres de type énuméré comme client_min_messages.
:::image type="content" source="./media/howto-configure-server-parameters-in-portal/4-enum-drop-down.png" alt-text="Bouton déroulant Énumérer":::

5. Sélectionnez ou placez le curseur sur le bouton **i** (informations) pour afficher la plage des valeurs possibles des paramètres numériques comme cpu_index_tuple_cost.
:::image type="content" source="./media/howto-configure-server-parameters-in-portal/4-information-button.png" alt-text="Bouton Informations":::

6. Si nécessaire, utilisez la **zone de recherche** pour limiter la recherche à un paramètre spécifique. La recherche s’effectue sur le nom et la description des paramètres.
:::image type="content" source="./media/howto-configure-server-parameters-in-portal/5-search.png" alt-text="Résultats de la recherche":::

7. Modifiez les valeurs de paramètres souhaitées. Toutes les modifications que vous apportez dans une session sont surlignées en violet. Une fois que vous avez modifié les valeurs, vous pouvez sélectionner **Enregistrer**. Vous pouvez également **Abandonner** vos modifications.
:::image type="content" source="./media/howto-configure-server-parameters-in-portal/6-save-and-discard-buttons.png" alt-text="Enregistrer ou annuler les modifications":::

8. Si vous avez enregistré de nouvelles valeurs pour les paramètres, vous pouvez toujours rétablir toutes les valeurs par défaut en sélectionnant **Rétablir toutes les valeurs par défaut**.
:::image type="content" source="./media/howto-configure-server-parameters-in-portal/7-reset-to-default-button.png" alt-text="Rétablir toutes les valeurs par défaut":::

## <a name="next-steps"></a>Étapes suivantes
Vous en saurez plus sur :
- [Vue d’ensemble des paramètres de serveur dans Azure Database pour PostgreSQL](concepts-servers.md)
- [Configuration des paramètres à l’aide d’Azure CLI](howto-configure-server-parameters-using-cli.md)
