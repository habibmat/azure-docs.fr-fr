---
title: Connecter vos machines non-Azure à Azure Security Center
description: Découvrez comment connecter vos machines non-Azure à Security Center
author: memildin
ms.author: memildin
ms.date: 9/12/2020
ms.topic: how-to
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 2602df2e8a2699914ee32138a8aeba31d7f58cdb
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90930018"
---
#  <a name="connect-your-non-azure-machines-to-security-center"></a>Connecter vos machines non-Azure à Security Center

Security Center peut surveiller l’état de sécurité de vos ordinateurs autres qu’Azure, mais vous devez d’abord intégrer ces ressources. Vous pouvez ajouter des ordinateurs non-Azure à partir de la page **Mise en route** ou de la page **Inventaire**, comme décrit ci-dessous.

## <a name="add-non-azure-computers"></a>Ajouter des ordinateurs non-Azure 

1. Dans le menu de Security Center, ouvrez la page **Mise en route**.
1. Sélectionnez l’onglet **Prise en main**.

    :::image type="content" source="./media/security-center-onboarding/onboarding-get-started-tab.png" alt-text="Onglet Prise en main de la page Mise en route" lightbox="./media/security-center-onboarding/onboarding-get-started-tab.png":::

1. Sous **Ajouter des serveurs non-Azure**, sélectionnez **Configurer** .

    > [!TIP]
    > Vous pouvez également ajouter des ordinateurs à partir de la page **Inventaire**, en cliquant sur le bouton **Ajouter des serveurs non-Azure**.

    Une liste de vos espaces de travail Log Analytics apparaît. Elle comprend, le cas échéant, l’espace de travail par défaut créé pour vous par Security Center à l’activation de l’approvisionnement automatique. Sélectionnez cet espace de travail ou un autre espace de travail à utiliser.

    Vous pouvez ajouter des ordinateurs à un espace de travail existant ou créer un espace de travail. 

1. Vous pouvez également créer un espace de travail en sélectionnant **Créer un espace de travail**.

1. Dans la liste des espaces de travail, sélectionnez **Ajouter des serveurs** pour l’espace de travail approprié.
    La page **Gestion des agents** apparaît.

    À partir de là, choisissez la procédure appropriée ci-dessous en fonction du type de machine que vous intégrez :

    - [Intégrer vos machines virtuelles Azure Stack](#onboard-your-azure-stack-vms)
    - [Intégrer vos machines Linux](#onboard-your-linux-machines)
    - [Intégrer vos machines Windows](#onboard-your-windows-machines)


### <a name="onboard-your-azure-stack-vms"></a>Intégrer vos machines virtuelles Azure Stack
Vous avez besoin des informations de la page **Gestion des agents** pour ajouter des machines virtuelles Azure Stack et pour configurer l’extension de machine virtuelle **Azure Monitor, Update and Configuration Management** sur les machines virtuelles s’exécutant sur votre Azure Stack.
1. Dans la page **Gestion des agents**, copiez **ID de l’espace de travail** et **Clé primaire** dans le Bloc-notes.
1. Connectez-vous à votre portail **Azure Stack** et ouvrez la page **Machines virtuelles**.
1. Sélectionnez la machine virtuelle que vous souhaitez protéger avec Security Center.
    >[!TIP]
    > Pour plus d’informations sur la façon de créer une machine virtuelle sur Azure Stack, consultez [ce guide de démarrage rapide pour les machines virtuelles Windows](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-quick-windows-portal) ou [ce guide de démarrage rapide pour les machines virtuelles Linux](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-quick-linux-portal).
1. Sélectionnez **Extensions**. La liste des extensions de machine virtuelle installées sur cette machine virtuelle s’affiche.
1. Sélectionnez l’onglet **Ajouter**. Le menu **Nouvelle ressource** affiche la liste des extensions de machine virtuelle disponibles.
1. Sélectionnez l’extension **Azure Monitor, Update and Configuration Management**, puis sélectionnez **Créer**. La page de configuration **Installer l’extension** s’ouvre.
    >[!NOTE]
    > Si l’extension **Azure Monitor, Update and Configuration Management** n’est pas listée dans votre place de marché, contactez votre opérateur Azure Stack pour qu’elle soit disponible.
1. Dans la page de configuration **Installer l’extension**, collez l’**ID de l’espace de travail** et la **Clé de l’espace de travail (clé primaire)** que vous avez copiés dans le Bloc-notes au cours de l’étape précédente.
1. Sélectionnez **OK** pour achever la configuration. L’état de l’extension indique **Approvisionnement réussi**. Il peut s’écouler une heure avant que la machine virtuelle n’apparaisse dans Security Center.


### <a name="onboard-your-linux-machines"></a>Intégrer vos machines Linux
Pour ajouter des machines Linux, vous avez besoin de la commande WGET de la page **Gestion des agents**.
1. Depuis la page **Gestion des agents**, copiez la commande **WGET** dans le Bloc-notes. Enregistrez ce fichier dans un emplacement accessible à partir de votre ordinateur Linux.
1. Sur votre ordinateur Linux, ouvrez le fichier avec la commande WGET. Sélectionnez tout le contenu, copiez-le, puis collez-le dans une console de terminal.
1. Une fois l’installation terminée, vous pouvez vérifier que *omsagent* est installé en exécutant la commande *pgrep*. La commande retourne l’ID de processus (PID) *omsagent*.
    Les journaux de l’agent se trouvent à l’emplacement suivant : */var/opt/microsoft/omsagent/\<workspace id>/log/* . 30 minutes peuvent s’écouler avant que la nouvelle machine Linux n’apparaisse dans Security Center.


### <a name="onboard-your-windows-machines"></a>Intégrer vos machines Windows
Vous avez besoin des informations de la page **Gestion des agents** pour ajouter des machines Windows et télécharger le fichier d’agent approprié (32/64 bits).
1. Cliquez sur le lien **Télécharger l’Agent Windows** correspondant au type de processeur de votre ordinateur pour télécharger le fichier d’installation.
1. Dans la page **Gestion des agents**, copiez **ID de l’espace de travail** et **Clé primaire** dans le Bloc-notes.
1. Copiez le fichier d’installation téléchargé sur l’ordinateur cible, puis exécutez-le.
1. Suivez les instructions de l’Assistant d’installation (**Suivant**, **J’accepte**, **Suivant**, **Suivant**).
    1. Dans la page **Azure Log Analytics**, collez les valeurs **ID de l’espace de travail** et **Clé de l’espace de travail (clé primaire)** que vous avez copiées dans le Bloc-notes.
    1. Si l’ordinateur doit rendre compte à un espace de travail Log Analytics dans le cloud Azure Government, sélectionnez **Azure US Government** dans la liste déroulante **Cloud Azure**.
    1. Si l’ordinateur a besoin de communiquer avec le service Log Analytics par le biais d’un serveur proxy, sélectionnez **Avancé**, puis indiquez l’URL et le numéro de port du serveur proxy.
    1. Une fois que vous avez entré tous les paramètres de configuration, sélectionnez **Suivant**.
    1. Sur la page **Prêt à installer**, passez en revue les paramètres à appliquer, puis sélectionnez **Installer**.
    1. Sur la page **Configuration effectuée**, sélectionnez **Terminer**.

Quand vous avez terminé, l’**agent Log Analytics** apparaît dans le **Panneau de configuration**. Vous pouvez vérifier votre configuration et vous assurer que l’agent est connecté.

Pour plus d’informations sur l’installation et la configuration de l’agent, consultez la page [Connecter des machines Windows](../azure-monitor/platform/agent-windows.md#install-agent-using-setup-wizard).


## <a name="verifying"></a>Vérification
Félicitations ! Vous pouvez maintenant afficher vos machines Azure et non-Azure au même endroit. Ouvrez la [page d’inventaire des ressources](asset-inventory.md) et filtrez les types de ressources appropriés. Ces deux icônes distinguent les différents types :

  ![icon1](./media/quick-onboard-linux-computer/security-center-monitoring-icon1.png) Machine non-Azure

  ![icon2](./media/quick-onboard-linux-computer/security-center-monitoring-icon2.png) Azure VM


## <a name="next-steps"></a>Étapes suivantes

Cette page vous a montré comment ajouter vos machines non-Azure à Azure Security Center. Pour surveiller leur état, utilisez les outils d’inventaire, comme expliqué à la page suivante :

- [Explorez et gérez vos ressources à l’aide des outils d’inventaire et de gestion des ressources](asset-inventory.md)