---
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/09/2018
ms.author: cherylmc
ms.openlocfilehash: 622a2f69c2e7b82f2df10d6120ee2dd466961915
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77211392"
---
Les anciennes références SKU de passerelle VPN sont :

* Par défaut (De base)
* standard
* HighPerformance

La passerelle VPN n’utilise pas la référence de la passerelle UltraPerformance. Pour plus d’informations sur la référence UltraPerformance, consultez la documentation [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md).

Lorsque vous travaillez avec les anciennes références SKU, tenez compte des points suivants :

* Si vous souhaitez utiliser un type de VPN basé sur uns stratégie, vous devez utiliser la référence SKU De base. Les VPN basés sur une stratégie (précédemment appelés « routage statique ») ne sont pas pris en charge sur une autre SKU.
* Le protocole BGP n’est pas pris en charge pour la référence SKU De base.
* Les configurations de passerelle VPN et ExpressRoute coexistants ne sont pas prises en charge par la référence SKU De base.
* Les connexions VPN S2S en mode actif-actif avec des passerelles peuvent uniquement être configurées sur la référence SKU HighPerformance.