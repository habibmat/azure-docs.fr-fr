---
title: Restrictions des URI de redirection (URL de réponse) | Azure
titleSuffix: Microsoft identity platform
description: Description des restrictions et limitations relatives au format des URI de redirection (URL de réponse) appliquées par la plateforme d’identité Microsoft.
author: SureshJa
ms.author: sureshja
manager: CelesteDG
ms.date: 08/07/2020
ms.topic: conceptual
ms.subservice: develop
ms.custom: aaddev
ms.service: active-directory
ms.reviewer: lenalepa, manrath
ms.openlocfilehash: bd6f88db2b55a5f0f445659e4b5ef609d3e146e9
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90030308"
---
# <a name="redirect-uri-reply-url-restrictions-and-limitations"></a>Limitations et restrictions des URI de redirection (URL de réponse)

Un URI de redirection, ou URL de réponse, correspond à l’emplacement vers lequel le serveur d’autorisation envoie l’utilisateur une fois l’application autorisée et un code d’autorisation ou un jeton d’accès attribué. Le serveur d’autorisation envoie le code ou le jeton à l’URI de redirection. Il est donc important que vous inscriviez l’emplacement qui convient dans le cadre du processus d’inscription de l’application.

 Les restrictions suivantes s’appliquent aux URI de redirection :

* L’URI de redirection doit commencer par le schéma `https`.

* L’URI de redirection respecte la casse. Sa casse doit correspondre à celle du chemin d’URL de votre application en cours d’exécution. Par exemple, si votre application comprend `.../abc/response-oidc` dans son chemin d’accès, ne spécifiez pas `.../ABC/response-oidc` dans l’URI de redirection. Étant donné que le navigateur web considère que les chemins respectent la casse, les cookies associés à `.../abc/response-oidc` peuvent être exclus s’ils sont redirigés vers l’URL `.../ABC/response-oidc` qui ne correspond pas à la casse.

## <a name="maximum-number-of-redirect-uris"></a>Nombre maximal d'URI de redirection

Ce tableau indique le nombre maximal d’URI de redirection que vous pouvez ajouter à une inscription d’application dans la plateforme d’identité Microsoft.

| Comptes en cours de connexion | Nombre maximal d'URI de redirection | Description |
|--------------------------|---------------------------------|-------------|
| Comptes professionnels et scolaires Microsoft d'un locataire Azure Active Directory (Azure AD) d'une organisation | 256 | Dans le manifeste de l'application, le champ `signInAudience` est défini sur *AzureADMyOrg* ou *AzureADMultipleOrgs* |
| Comptes personnels ou professionnels et scolaires Microsoft | 100 | Dans le manifeste de l'application, le champ `signInAudience` est défini sur *AzureADandPersonalMicrosoftAccount* |

## <a name="maximum-uri-length"></a>Longueur maximale d’URI

Vous pouvez utiliser un maximum de 256 caractères pour chaque URI de redirection que vous ajoutez à une inscription d’application.

## <a name="supported-schemes"></a>Schémas pris en charge

Le modèle d’application Azure Active Directory (Azure AD) prend actuellement en charge les schémas HTTP et HTTPS pour les applications qui connectent des comptes professionnels ou scolaires dans tout locataire Azure AD de l’organisation. Ces types de comptes sont spécifiés par les valeurs `AzureADMyOrg` et `AzureADMultipleOrgs` dans le champ `signInAudience` du manifeste de l’application. Pour les applications qui connectent des comptes Microsoft personnels (MSA) *et* des comptes professionnels et scolaires (c’est-à-dire que `signInAudience` est défini sur `AzureADandPersonalMicrosoftAccount`), seul le schéma HTTPS est autorisé.

Pour ajouter des URI de redirection avec un schéma HTTP aux inscriptions d’applications qui se connectent à des comptes professionnels ou scolaires, vous devez utiliser l’éditeur de manifeste de l’application dans [Inscriptions d’applications](https://go.microsoft.com/fwlink/?linkid=2083908) dans le Portail Azure. Toutefois, bien qu’il soit possible de définir un URI de redirection basé sur HTTP à l’aide de l’éditeur de manifeste, nous recommandons *fortement* d’utiliser le schéma HTTPS pour vos URI de redirection.

## <a name="localhost-exceptions"></a>Exceptions de Localhost

Selon le document [RFC 8252 : sections 8.3](https://tools.ietf.org/html/rfc8252#section-8.3) et [7.3](https://tools.ietf.org/html/rfc8252#section-7.3), les URI de redirection « loopback » ou « localhost » comportent deux aspects particuliers à prendre en considération :

1. `http` Les schémas d’URI sont acceptables car la redirection ne quitte jamais l’appareil. Par conséquent, ces deux éléments sont acceptables :
    - `http://127.0.0.1/myApp`
    - `https://127.0.0.1/myApp`
1. En raison des plages de ports éphémères souvent nécessaires aux applications natives, le composant de port (par exemple `:5001` ou `:443`) est ignoré pour permettre une correspondance avec un URI de redirection. En conséquence, tous ces éléments sont considérés comme équivalents :
    - `http://127.0.0.1/MyApp`
    - `http://127.0.0.1:1234/MyApp`
    - `http://127.0.0.1:5000/MyApp`
    - `http://127.0.0.1:8080/MyApp`

Du point de vue du développement, cela signifie plusieurs choses :

* N’inscrivez pas plusieurs URI de redirection quand seul le port diffère. Le serveur de connexion en choisit un arbitrairement et utilise le comportement associé à cet URI de redirection (par exemple, s’il s’agit d’une redirection de type `web`, `native` ou `spa`).
* Si vous devez inscrire plusieurs URI de redirection sur localhost pour tester différents flux pendant le développement, différenciez-les à l’aide du composant *path* de l’URI. Par exemple, `http://127.0.0.1/MyWebApp` ne correspond pas à `http://127.0.0.1/MyNativeApp`.
* D’après la documentation d’aide des RFC, vous ne devez pas utiliser `localhost` dans l’URI de redirection. À la place, utilisez l’adresse IP réelle de bouclage, `127.0.0.1`. Cela empêche votre application d’être bloquée par des pare-feu mal configurés ou des interfaces réseau renommées.

    Pour utiliser le schéma `http` avec l’adresse de bouclage (127.0.0.1) au lieu de localhost, vous devez modifier le [manifeste d’application](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest#replyurls-attribute). 

    L’adresse de bouclage IPv6 (`[::1]`) n’est pas prise en charge actuellement.

## <a name="restrictions-on-wildcards-in-redirect-uris"></a>Restrictions concernant les caractères génériques des URI de redirection

Les URI avec caractères génériques comme `https://*.contoso.com` peuvent paraître pratiques, mais doivent être évités en raison des implications en matière de sécurité. Selon la spécification OAuth 2.0 ([section 3.1.2 de RFC 6749](https://tools.ietf.org/html/rfc6749#section-3.1.2)), un URI de point de terminaison de redirection doit être un URI absolu.

Les URI avec caractères génériques ne sont actuellement pas pris en charge dans les inscriptions d’applications configurées pour se connecter à des comptes Microsoft personnels et à des comptes professionnels ou scolaires. Les URI avec caractères génériques sont autorisés, mais uniquement pour les applications configurées pour se connecter à des comptes professionnels ou scolaires dans un locataire Azure AD d’une organisation.

Pour ajouter des URI de redirection avec des caractères génériques aux inscriptions d’applications qui se connectent à des comptes professionnels ou scolaires, vous devez utiliser l’éditeur de manifeste de l’application dans [Inscriptions d’applications](https://go.microsoft.com/fwlink/?linkid=2083908) dans le Portail Azure. Bien qu’il soit possible de définir un URI de redirection avec un caractère générique à l’aide de l’éditeur de manifeste, nous vous recommandons *fortement* de respecter la [section 3.1.2 de la RFC 6749](https://tools.ietf.org/html/rfc6749#section-3.1.2) et d’utiliser uniquement des URI absolus.

Si votre scénario implique plus d’URI de redirection que la limite maximale autorisée, envisagez les [approches suivantes](#use-a-state-parameter) au lieu d’ajouter un URI de redirection avec caractères génériques.

### <a name="use-a-state-parameter"></a>Utiliser un paramètre d’état

Si vous avez plusieurs sous-domaines et que votre scénario implique que vous redirigiez des utilisateurs, en cas d’authentification réussie, vers la même page que celle où ils ont été démarrés, l’utilisation d’un paramètre d’état peut être utile.

Dans cette approche :

1. Créez un URI de redirection « partagé » par l’application pour traiter les jetons de sécurité que vous recevez du point de terminaison d’autorisation.
1. Votre application peut envoyer des paramètres qui lui sont spécifiques (URL de sous-domaine avec origine de l’utilisateur ou informations sur une marque, par exemple) dans le paramètre d’état. Lorsque vous utilisez un paramètre d’état, protégez-vous contre la falsification de requête intersites (CSRF, Cross Site Request Forgery) comme spécifié dans la [section 10.12 du document RFC 6749](https://tools.ietf.org/html/rfc6749#section-10.12)).
1. Les paramètres spécifiques à l’application incluent toutes les informations requises pour permettre à l'application d'offrir une bonne expérience à l’utilisateur, à savoir, concevoir l'état d'application approprié. Le point de terminaison d'autorisation Azure AD supprime le code HTML du paramètre d'état. Aussi, assurez-vous que vous ne transmettez pas de contenu HTML dans ce paramètre.
1. Quand Azure AD envoie une réponse à l'URI de redirection « partagé », il renvoie le paramètre d'état à l'application.
1. L'application peut ensuite utiliser la valeur du paramètre d'état pour déterminer l'URL vers laquelle envoyer l'utilisateur. Veillez à valider la protection CSRF.

> [!WARNING]
> Cette approche permet à un client compromis de modifier les paramètres supplémentaires envoyés dans le paramètre d'état, redirigeant ainsi l'utilisateur vers une URL différente, ce qui correspond à la [menace de redirecteur ouvert](https://tools.ietf.org/html/rfc6819#section-4.2.4) décrite dans le document RFC 6819. Par conséquent, le client doit protéger ces paramètres en chiffrant l’état ou en le vérifiant par un autre moyen, tel que la validation du nom de domaine dans l’URI de redirection par rapport au jeton.

## <a name="next-steps"></a>Étapes suivantes

En savoir plus sur le [Manifeste de l’application](reference-app-manifest.md) de l’inscription d’application.
