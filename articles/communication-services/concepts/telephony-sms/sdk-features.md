---
title: Vue d’ensemble de la bibliothèque cliente SMS pour Azure Communication Services
titleSuffix: An Azure Communication Services concept document
description: Fournit une vue d’ensemble de la bibliothèque cliente SMS et de ses offres.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 2d81749e7023bdbf5353e5c8da633674ea8e8ce9
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90944234"
---
# <a name="sms-client-library-overview"></a>Vue d’ensemble de la bibliothèque cliente SMS

[!INCLUDE [Private Preview Notice](../../includes/private-preview-include.md)]

Les bibliothèques clientes SMS d’Azure Communication Services peuvent être utilisées pour ajouter la messagerie SMS à vos applications.

## <a name="sms-client-library-capabilities"></a>Fonctionnalités de la bibliothèque cliente SMS

La liste suivante présente l’ensemble des fonctionnalités actuellement disponibles dans nos bibliothèques clientes.

| Groupe de fonctionnalités | Fonctionnalité                                                                            | JS  | Java | .NET | Python |
| ----------------- | ------------------------------------------------------------------------------------- | --- | ---- | ---- | ------ |
| Fonctionnalités principales | Envoyer et recevoir des messages SMS </br> *Emojis Unicode pris en charge*                        | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Recevoir des rapports de remise pour les messages envoyés                                            | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Tous les jeux de caractères (prise en charge de la langue/Unicode)                                         | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Prise en charge des messages longs (jusqu’à 2 048 caractères)                                           | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Concaténation automatique des messages longs                                                   | ✔️   | ✔️    | ✔️    | ✔️      |
| Événements            | Utiliser Event Grid pour configurer des webhooks afin de recevoir des messages entrants et des rapports de remise | ✔️   | ✔️    | ✔️    | ✔️      |
| Numéro de téléphone      | Numéros gratuits                                                                     | ✔️   | ✔️    | ✔️    | ✔️      |
| Réglementaire        | Gestion des désactivations                                                                      | ✔️   | ✔️    | ✔️    | ✔️      |
| Surveillance        | Surveiller l’utilisation des messages envoyés et reçus                                          | ✔️   | ✔️    | ✔️    | ✔️      |
| Appels RTPC      | Ajouter des fonctionnalités d’appel RTPC à votre numéro de téléphone compatible SMS                    | ✔️   | ✔️    | ✔️    | ✔️      |

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [ l’envoi de SMS](../../quickstarts/telephony-sms/send.md)

Les documents suivants peuvent vous intéresser :

- Se familiariser avec les [concepts SMS](../telephony-sms/concepts.md) généraux
- Obtenir un [numéro de téléphone](../../quickstarts/telephony-sms/get-phone-number.md) compatible SMS
- [Planifier votre solution SMS](../telephony-sms/plan-solution.md)