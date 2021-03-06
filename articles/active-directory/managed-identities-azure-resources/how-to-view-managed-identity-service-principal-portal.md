---
title: Afficher le principal du service d’une identité managée dans le portail Azure - Azure AD
description: Instructions pas à pas pour afficher le principal du service d’une identité managée dans le portail Azure.
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/29/2018
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.openlocfilehash: c88f3cdc880888162301ace5c896161f669d2ede
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89269197"
---
# <a name="view-the-service-principal-of-a-managed-identity-in-the-azure-portal"></a>Afficher le principal du service d’une identité managée dans le portail Azure

Les identités managées pour ressources Azure fournissent des services Azure avec une identité managée automatiquement dans Azure Active Directory. Vous pouvez utiliser cette identité pour vous authentifier sur n’importe quel service prenant en charge l’authentification Azure AD, sans avoir d’informations d’identification dans votre code. 

Dans cet article, vous allez apprendre à afficher le principal du service d’une identité managée à l’aide du portail Azure.

 > [!NOTE] 
 > Les principaux de service sont des applications d’entreprise. 

## <a name="prerequisites"></a>Conditions préalables requises

- Si vous n’êtes pas familiarisé avec les identités managées pour ressources Azure, consultez la [section Vue d’ensemble](overview.md).
- Si vous n’avez pas encore de compte Azure, [inscrivez-vous pour créer un compte gratuit](https://azure.microsoft.com/free/).
- Activez [l’identité affectée par le système sur une machine virtuelle](./qs-configure-portal-windows-vm.md#system-assigned-managed-identity) ou une [application](../../app-service/overview-managed-identity.md#add-a-system-assigned-identity).

## <a name="view-the-service-principal"></a>Afficher le principal du service

Cette procédure montre comment afficher le principal du service d’une machine virtuelle avec l’identité affectée par le système activée (les mêmes étapes s’appliquent pour une application).

1. Cliquez sur **Azure Active Directory**, puis sur **Applications d’entreprise**.
2. Sous **Type d’application**, choisissez **Toutes les applications**, puis cliquez sur **Appliquer**.
3. Dans la zone du filtre de recherche, tapez le nom de la machine virtuelle ou de l’application sur laquelle l’identité managée est activée ou faites votre choix dans la liste présentée.

   ![Afficher le principal du service d’identité managée dans le portail](./media/how-to-view-managed-identity-service-principal-portal/view-managed-identity-service-principal-portal.png)

## <a name="next-steps"></a>Étapes suivantes

[Identités managées pour les ressources Azure](./overview.md)