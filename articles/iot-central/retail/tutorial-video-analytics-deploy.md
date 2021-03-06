---
title: Comment déployer le modèle d’application d’analytique vidéo pour la détection d’objets et de mouvements Azure IoT Central
description: Ce guide résume les étapes de déploiement d’une application Azure IoT Central à l’aide du modèle d’application d’analytique vidéo pour la détection d’objets et de mouvements.
services: iot-central
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: how-to
ms.author: nandab
author: KishorIoT
ms.date: 07/31/2020
ms.openlocfilehash: d661df57e4409c1d7fe196c7f136965191421bd4
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88719566"
---
# <a name="how-to-deploy-an-iot-central-application-using-the-video-analytics---object-and-motion-detection-application-template"></a>Comment déployer une application IoT Central à l’aide du modèle d’application d’analytique vidéo pour la détection d’objets et de mouvements

Pour obtenir une vue d’ensemble des composants d’application clés d’*analytique vidéo pour la détection d’objets et de mouvements*, consultez l’[architecture de l’application d’analytique vidéo pour la détection d’objets et de mouvements](architecture-video-analytics.md).

## <a name="deploy-the-application"></a>Déployer l’application

Procédez comme suit pour déployer une application IoT Central à l’aide du modèle d’application d’analytique vidéo :

1. Effectuez le tutoriel [Créer une application d’analytique vidéo dans Azure IoT Central](tutorial-video-analytics-create-app.md) pour :
    - Créer un compte Azure Media Services.
    - Créer l’application IoT Central à partir du modèle d’application d’analytique vidéo pour la détection d’objets et de mouvements.
    - Configurer un appareil de passerelle dans l’application IoT Central. La passerelle permet aux caméras de se connecter à l’application.

1. Effectuez le tutoriel [Créer une instance IoT Edge pour l’analytique vidéo (machine virtuelle Linux)](tutorial-video-analytics-iot-edge-vm.md) pour :
    - Créer une machine virtuelle Azure sur laquelle est installé le runtime Azure IoT Edge. Préparer l’installation IoT Edge pour héberger le module d’analytique vidéo.
    - Connecter l’appareil IoT Edge à votre application IoT Central.

1. Effectuez le tutoriel [Superviser et gérer une application d’analytique vidéo](tutorial-video-analytics-manage.md) pour :
    - Ajouter des caméras de détection d’objet et de mouvement à la passerelle dans votre application IoT Central.
    - Démarrer le traitement de la caméra.
    - Installer un lecteur multimédia local pour afficher la vidéo capturée dans AMS.
    - Afficher la vidéo capturée qui montre les objets détectés.
    - Nettoyer.

## <a name="next-steps"></a>Étapes suivantes

Maintenant que vous disposez d’une vue d’ensemble des étapes de déploiement et d’utilisation du modèle d’application d’analytique vidéo, consultez [Créer une application d’analytique vidéo dans Azure IoT Central](tutorial-video-analytics-create-app.md) pour commencer.
