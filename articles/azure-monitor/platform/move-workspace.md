---
title: Déplacer un espace de travail Log Analytics dans Azure Monitor | Microsoft Docs
description: Découvrez comment déplacer votre espace de travail Log Analytics vers un autre abonnement ou groupe de ressources.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/13/2019
ms.openlocfilehash: 91094879de1e1762f95d35e22c1ea441e211b99e
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90979695"
---
# <a name="move-a-log-analytics-workspace-to-different-subscription-or-resource-group"></a>Déplacer un espace de travail Log Analytics vers un autre abonnement ou groupe de ressources

Dans cet article, vous découvrirez la procédure permettant de déplacer un espace de travail Log Analytics vers un autre abonnement ou groupe de ressources dans la même région. Vous pouvez en savoir plus sur le déplacement des ressources Azure via le portail Azure, PowerShell, Azure CLI ou l’API REST. sur [Déplacer des ressources vers un nouveau groupe de ressource ou un nouvel abonnement](../../azure-resource-manager/management/move-resource-group-and-subscription.md). 

> [!IMPORTANT]
> Vous ne pouvez pas déplacer un espace de travail vers une autre région.

## <a name="verify-active-directory-tenant"></a>Vérifier le locataire Active Directory
Les abonnements source et de destination de l’espace de travail doivent exister dans le même locataire Azure Active Directory. Utilisez Azure PowerShell pour vérifier que les deux abonnements ont le même ID de locataire.

``` PowerShell
(Get-AzSubscription -SubscriptionName <your-source-subscription>).TenantId
(Get-AzSubscription -SubscriptionName <your-destination-subscription>).TenantId
```

## <a name="workspace-move-considerations"></a>Considérations relatives au déplacement de l’espace de travail
Les solutions gérées qui sont installées dans l’espace de travail seront déplacées à l’aide de l’opération de déplacement de l’espace de travail Log Analytics. Les agents connectés restent connectés et continuent à envoyer des données à l’espace de travail après le déplacement. Étant donné que l’opération de déplacement exige qu’il n’existe aucun service lié à partir de l’espace de travail, les solutions qui s’appuient sur ce lien doivent être supprimées pour permettre le déplacement de l’espace de travail.

Solutions qui doivent être supprimées avant de pouvoir dissocier votre compte Automation :

- Update Management
- Suivi des modifications
- Démarrer/arrêter des machines virtuelles pendant les heures creuses
- Azure Security Center

>[!IMPORTANT]
> **Clients Azure Sentinel :**
> - Une fois déployé dans un espace de travail, Azure Sentinel **ne prend actuellement pas en charge** le déplacement de cet espace de travail vers d’autres groupes de ressources ou abonnements. 
>
>   Si vous avez déjà déplacé l’espace de travail, désactivez toutes les règles actives dans **Analytics**, puis réactivez-les après cinq minutes. Cela doit fonctionner la plupart du temps, mais ce n’est, pour rappel, pas pris en charge et est sous votre entière responsabilité.

### <a name="delete-solutions-in-azure-portal"></a>Supprimer des solutions dans le portail Azure
Procédez comme suit pour supprimer les solutions via le portail Azure :

1. Ouvrez le menu des groupes de ressources dans lesquels des solutions sont installées.
2. Sélectionnez les solutions à supprimer.
3. Cliquez sur **Supprimer les ressources**, puis confirmez la suppression en cliquant sur **Supprimer**.

![Supprimer des solutions](media/move-workspace/delete-solutions.png)

### <a name="delete-using-powershell"></a>Supprimer à l’aide de PowerShell

Pour supprimer les solutions à l’aide de PowerShell, utilisez la cmdlet [Remove-AzResource](/powershell/module/az.resources/remove-azresource?view=azps-2.8.0) comme indiqué dans l’exemple suivant :

``` PowerShell
Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "ChangeTracking(<workspace-name>)" -ResourceGroupName <resource-group-name>
Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Updates(<workspace-name>)" -ResourceGroupName <resource-group-name>
Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Start-Stop-VM(<workspace-name>)" -ResourceGroupName <resource-group-name>
```

### <a name="remove-alert-rules-for-startstop-vms-solution"></a>Supprimer les règles d’alerte pour la solution Start/Stop VMs
Pour supprimer la solution **Start/Stop VMs**, vous devez également supprimer les règles d’alerte qu’elle a créées. Procédez comme suit pour supprimer ces règles via le portail Azure.

1. Ouvrez le menu **Surveiller**, puis sélectionnez **Alertes**.
2. Cliquez sur **Gérer les règles d’alerte**.
3. Sélectionnez les trois règles d’alerte suivantes, puis cliquez sur **Supprimer**.

   - AutoStop_VM_Child
   - ScheduledStartStop_Parent
   - SequencedStartStop_Parent

    ![Supprimer des règles](media/move-workspace/delete-rules.png)

## <a name="unlink-automation-account"></a>Dissocier un compte Automation
Procédez comme suit pour dissocier le compte Automation lié à l’espace de travail via le portail Azure :

1. Ouvrez le menu **Comptes Automation**, puis sélectionnez le compte à supprimer.
2. Accédez à la section **Ressources associées** du menu, puis sélectionnez **Espace de travail lié**. 
3. Sélectionnez **Dissocier l’espace de travail** pour dissocier l’espace de travail de votre compte Automation.

    ![Supprimer le lien de votre espace de travail](media/move-workspace/unlink-workspace.png)

## <a name="move-your-workspace"></a>Déplacer votre espace de travail

### <a name="azure-portal"></a>Portail Azure
Procédez comme suit pour déplacer votre espace de travail via le portail Azure :

1. Ouvrez le menu **Espaces de travail Log Analytics**, puis sélectionnez votre espace de travail.
2. Dans la page **Vue d’ensemble**, cliquez sur le bouton **Modifier** en regard de **Groupe de ressources** ou **Abonnement**.
3. Une nouvelle page comprenant une liste de ressources associées à l’espace de travail s’affiche. Sélectionnez les ressources à déplacer vers le même abonnement de destination et le même groupe de ressources que l’espace de travail. 
4. Sélectionnez un **abonnement** de destination et un **groupe de ressources**. Si vous déplacez l’espace de travail vers un autre groupe de ressources dans le même abonnement, vous ne verrez pas l’option **Abonnement**.
5. Cliquez sur **OK** pour déplacer l’espace de travail et les ressources sélectionnées.

    ![Capture d’écran affichant le volet Vue d’ensemble de l’espace de travail Log Analytics avec les options permettant de modifier le groupe de ressources et le nom de l’abonnement.](media/move-workspace/portal.png)

### <a name="powershell"></a>PowerShell
Pour déplacer votre espace de travail à l’aide de PowerShell, utilisez la cmdlet [Move-AzResource](/powershell/module/AzureRM.Resources/Move-AzureRmResource) comme dans l’exemple suivant :

``` PowerShell
Move-AzResource -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MyResourceGroup01/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace" -DestinationSubscriptionId "00000000-0000-0000-0000-000000000000" -DestinationResourceGroupName "MyResourceGroup02"
```

> [!IMPORTANT]
> Après le déplacement, les solutions supprimées et le lien du compte Automation doivent être reconfigurés pour rétablir l’état précédent de l’espace de travail.


## <a name="next-steps"></a>Étapes suivantes
- Pour obtenir la liste des ressources prenant en charge l’opération de déplacement, consultez [Prise en charge de l’opération de déplacement pour les ressources](../../azure-resource-manager/management/move-support-resources.md).
