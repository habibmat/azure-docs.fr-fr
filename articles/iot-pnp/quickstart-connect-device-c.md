---
title: Connecter l’exemple de code d’appareil C IoT Plug-and-Play en préversion à IoT Hub | Microsoft Docs
description: Créez et exécutez l’exemple de code pour appareil IoT Plug-and-Play en préversion sur Linux ou Windows qui se connecte à un hub IoT. Utilisez l’outil Azure IoT Explorer pour afficher les informations envoyées par l’appareil au hub.
author: ericmitt
ms.author: ericmitt
ms.date: 07/14/2020
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: afe7396ebdada97b9311d0afe903f40757084586
ms.sourcegitcommit: ac5cbef0706d9910a76e4c0841fdac3ef8ed2e82
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89426110"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-preview-device-application-running-on-linux-or-windows-to-iot-hub-c"></a>Démarrage rapide : Connecter un exemple d’application d’appareil IoT Plug-and-Play en préversion sur Linux ou Windows à IoT Hub (C)

[!INCLUDE [iot-pnp-quickstarts-device-selector.md](../../includes/iot-pnp-quickstarts-device-selector.md)]

Ce guide de démarrage rapide vous montre comment créer un exemple d’application d’appareil IoT Plug-and-Play, comment le connecter à votre hub IoT et comment utiliser l’outil Explorateur Azure IoT pour afficher les données de télémétrie qu’il envoie. L’exemple d’application est écrit en C et est inclus dans le Azure IoT device SDK pour C. Un générateur de solutions peut utiliser l’outil Explorateur Azure IoT pour comprendre les fonctionnalités d’un appareil IoT Plug-and-Play sans avoir à afficher de code d’appareil.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Prérequis

Vous pouvez exécuter ce guide de démarrage rapide sur Linux ou Windows. Dans ce guide de démarrage rapide, les commandes de l’interpréteur de commandes suivent la convention Linux pour les séparateurs de chemin « `/` » ; si vous suivez ce guide sur Windows, veillez à remplacer ces séparateurs par « `\` ».

Les prérequis diffèrent selon le système d’exploitation :

### <a name="linux"></a>Linux

Ce guide de démarrage rapide suppose que vous utilisez Ubuntu Linux. Les étapes de ce guide de démarrage rapide ont été testées à l’aide d’Ubuntu 18.04.

Pour suivre ce guide de démarrage rapide sur Linux, vous devez installer les logiciels suivants sur votre environnement Linux :

Installez **GCC**, **Git**, **cmake** et toutes les dépendances nécessaires à l’aide de la commande `apt-get` :

```sh
sudo apt-get update
sudo apt-get install -y git cmake build-essential curl libcurl4-openssl-dev libssl-dev uuid-dev
```

Vérifiez que la version de `cmake` est supérieure à **2.8.12** et que la version de **GCC** est supérieure à **4.4.7**.

```sh
cmake --version
gcc --version
```

### <a name="windows"></a>Windows

Pour suivre ce guide de démarrage rapide sur Windows, vous devez installer les logiciels suivants sur votre environnement Windows :

* [Visual Studio (Community, Professional ou Enterprise)](https://visualstudio.microsoft.com/downloads/) : veillez à inclure la charge de travail **Développement Desktop en C++** quand vous [installez](https://docs.microsoft.com/cpp/build/vscpp-step-0-installation?view=vs-2019) Visual Studio.
* [Git](https://git-scm.com/download/).
* [CMake](https://cmake.org/download/).

### <a name="azure-iot-explorer"></a>Explorateur Azure IoT

Pour interagir avec l’exemple d’appareil dans la deuxième partie de ce guide de démarrage rapide, vous utilisez l’outil **Explorateur Azure IoT**. [Téléchargez et installez la dernière version de l’Explorateur Azure IoT](./howto-use-iot-explorer.md) pour votre système d’exploitation.

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Exécutez la commande suivante pour obtenir la _chaîne de connexion IoT Hub_ pour votre hub. Prenez note de cette chaîne de connexion, car vous l’utiliserez plus loin dans ce guide de démarrage rapide :

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

> [!TIP]
> Vous pouvez également utiliser l’outil Explorateur Azure IoT pour rechercher la chaîne de connexion du hub IoT.

Exécutez la commande suivante pour obtenir la _chaîne de connexion_ à l’appareil que vous avez ajouté au hub. Prenez note de cette chaîne de connexion, car vous l’utiliserez plus loin dans ce guide de démarrage rapide :

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name <YourIoTHubName> --device-id <YourDeviceID> --output table
```

[!INCLUDE [iot-pnp-download-models.md](../../includes/iot-pnp-download-models.md)]

## <a name="download-the-code"></a>Téléchargement du code

Dans ce guide de démarrage rapide, vous préparez un environnement de développement pour cloner et générer le kit Azure IoT Hub Device C SDK.

Ouvrez une invite de commandes dans le répertoire de votre choix. Exécutez la commande suivante pour cloner le dépôt GitHub [Azure IoT C SDKs and Libraries](https://github.com/Azure/azure-iot-sdk-c) à cet emplacement :

```cmd\bash
git clone https://github.com/Azure/azure-iot-sdk-c.git
cd azure-iot-sdk-c
git submodule update --init
```

Attendez-vous à ce que cette opération prenne plusieurs minutes.

## <a name="build-the-code"></a>Générer le code

Vous utilisez le kit de développement logiciel (SDK) de l’appareil pour créer l’exemple de code inclus :

1. Créez un sous-répertoire _cmake_ dans le dossier racine du SDK d’appareil, puis accédez à ce dossier :

    ```cmd\bash
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

1. Exécutez les commandes suivantes pour générer le SDK et les exemples :

    ```cmd\bash
    cmake ..
    cmake --build .
    ```

> [!TIP]
> Sur Windows, vous pouvez ouvrir la solution générée par la commande `cmake` dans Visual Studio 2019. Ouvrez le fichier projet *azure_iot_sdks.sln* dans le répertoire _cmake_ et définissez le projet **pnp_simple_thermostat** comme projet de démarrage dans la solution. Vous pouvez maintenant générer l’exemple dans Visual Studio et l’exécuter en mode débogage.

## <a name="run-the-device-sample"></a>Exécuter l’exemple d’appareil

Pour exécuter l’exemple d’application dans le SDK qui simule un appareil IoT Plug-and-Play envoyant des données de télémétrie à votre hub IoT :

Créez deux variables d’environnement pour configurer l’exemple afin qu’il utilise une chaîne de connexion pour se connecter à votre hub IoT :

- **IOTHUB_DEVICE_SECURITY_TYPE** avec la valeur `"connectionString"`
- **IOTHUB_DEVICE_CONNECTION_STRING** pour stocker la chaîne de connexion à l’appareil que vous avez notée précédemment.

Dans le dossier _cmake_, accédez au dossier qui contient le fichier exécutable et exécutez-le :

```bash
# Bash
cd iothub_client/samples/pnp/pnp_simple_thermostat/
./pnp_simple_thermostat
```

```cmd
REM Windows
cd iothub_client\samples\pnp\pnp_simple_thermostat\Debug
.\pnp_simple_thermostat.exe
```

> [!TIP]
> Pour effectuer le suivi de l’exécution du code dans Visual Studio sur Windows, ajoutez un point d’arrêt à la fonction `main` dans le fichier _pnp_simple_thermostat.c_.

L’appareil, désormais prêt à recevoir des commandes et des mises à jour de propriétés, a commencé à envoyer des données de télémétrie au hub. Laissez l’exemple s’exécuter pendant que vous effectuez les étapes suivantes.

## <a name="use-azure-iot-explorer-to-validate-the-code"></a>Utiliser l’Explorateur Azure IoT pour valider le code

Une fois l’exemple de client d’appareil démarré, utilisez l’outil Explorateur Azure IoT pour vérifier qu’il fonctionne.

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

## <a name="review-the-code"></a>Vérifier le code

Cet exemple implémente un appareil à thermostat IoT Plug-and-Play simple. Le modèle implémenté par cet exemple n’utilise pas de [composants](concepts-components.md) IoT Plug-and-Play. Le [fichier de modèle DTDL pour l’appareil à thermostat](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/Thermostat.json) définit la télémétrie, les propriétés et les commandes implémentées par l’appareil.

Le code de l’appareil utilise la fonction standard pour se connecter à votre hub IoT :

```c
deviceHandle = IoTHubDeviceClient_CreateFromConnectionString(connectionString, MQTT_Protocol)
```

L’appareil envoie l’ID du modèle DTDL qu’il implémente dans la demande de connexion. Un appareil qui envoie un ID de modèle est un appareil IoT Plug-and-Play :

```c
static const char g_ModelId[] = "dtmi:com:example:Thermostat;1";

...

IoTHubDeviceClient_SetOption(deviceHandle, OPTION_MODEL_ID, modelId)
```

Le code qui met à jour les propriétés, gère les commandes et envoie les données de télémétrie est identique au code d’un appareil qui n’utilise pas les conventions IoT Plug-and-Play.

Le code utilise la bibliothèque Parson pour analyser les objets JSON dans les charges utiles envoyées à partir de votre hub IoT :

```c
// JSON parser
#include "parson.h"
```

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Étapes suivantes

Dans ce démarrage rapide, vous avez appris à connecter un appareil IoT Plug-and-Play à un hub IoT. Pour en savoir plus sur la création d’une solution qui interagit avec vos appareils IoT Plug-and-Play, consultez :

> [!div class="nextstepaction"]
> [Guide pratique pour Se connecter à un appareil et interagir avec celui-ci](howto-develop-solution.md)
