---
title: Utiliser Studio pour déployer des modèles entraînés dans le concepteur
titleSuffix: Azure Machine Learning
description: Utilisez Azure Machine Learning Studio pour déployer des modèles entraînés dans le concepteur.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: keli19
author: likebupt
ms.reviewer: peterlu
ms.date: 09/04/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: 95b41723d3cb398caad3a0cf388b7810deda78dc
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90930075"
---
# <a name="use-the-studio-to-deploy-models-trained-in-the-designer"></a>Utiliser Studio pour déployer des modèles entraînés dans le concepteur

Dans cet article, découvrez comment déployer un modèle entraîné à partir du concepteur en tant que point de terminaison en temps réel dans Azure Machine Learning Studio.

Le déploiement dans Studio se compose des étapes suivantes :

1. Inscrivez le modèle entraîné.
1. Téléchargez le script d’entrée et le fichier de dépendances Conda du modèle.
1. Déployez le modèle sur la cible de calcul.

Vous pouvez également déployer des modèles directement dans le concepteur pour ignorer les étapes d’inscription de modèle et de téléchargement de fichier. Cela peut être utile pour un déploiement rapide. Pour plus d’informations, consultez [Déployer un modèle avec le concepteur](tutorial-designer-automobile-price-deploy.md).

Les modèles entraînés dans le concepteur peuvent également être déployés par le biais du SDK ou de l’interface de ligne de commande (CLI). Pour plus d’informations, consultez [Déployer votre modèle existant avec Azure Machine Learning](how-to-deploy-existing-model.md).

## <a name="prerequisites"></a>Prérequis

* [Un espace de travail Azure Machine Learning](how-to-manage-workspace.md)

* Pipeline d’entraînement complet contenant un [module Entraîner un modèle](./algorithm-module-reference/train-model.md)

## <a name="register-the-model"></a>Inscrire le modèle

Une fois le pipeline d’entraînement terminé, inscrivez le modèle entraîné auprès de votre espace de travail Azure Machine Learning pour accéder au modèle dans d’autres projets.

1. Sélectionnez le module [Entraîner le modèle](./algorithm-module-reference/train-model.md).
1. Sélectionnez l’onglet **Sorties + journaux** dans le volet de droite.
1. Sélectionnez l’icône **Inscrire le modèle** ![Capture d’écran de l’icône d’engrenage](./media/how-to-deploy-model-designer/register-model-icon.png).

    ![Capture d’écran du volet droit du module Entraîner le modèle](./media/how-to-deploy-model-designer/train-model-right-pane.png)

1. Entrez le nom de votre modèle, puis sélectionnez **Enregistrer**.

Lorsque vous avez inscrit votre modèle, vous pouvez le retrouver dans la page de ressources **Modèles** dans Studio.
    
![Capture d’écran du modèle inscrit dans la page de ressources Modèles](./media/how-to-deploy-model-designer/models-asset-page.png)


## <a name="download-the-entry-script-file-and-conda-dependencies-file"></a>Télécharger le fichier de script d’entrée et le fichier de dépendances Conda

Pour déployer un modèle dans Azure Machine Learning Studio, vous avez besoin des fichiers suivants :

- **Fichier de script d’entrée** : charge le modèle entraîné, traite les données d’entrée des requêtes, effectue des inférences en temps réel et retourne le résultat. Le concepteur génère automatiquement un fichier de script d’entrée `score.py` lors de l’exécution du module **Entraîner le modèle**.

- **Fichier de dépendances Conda** : spécifie les packages PIP et Conda dont dépend votre service web. Le concepteur crée automatiquement un fichier `conda_env.yaml` lors de l’exécution du module **Entraîner le modèle**.

Vous pouvez télécharger ces deux fichiers dans le volet droit du module **Entraîner le modèle** :

1. Sélectionnez le module **Entraîner le modèle**.
1. Sous l’onglet **Sorties + journaux**, sélectionnez le dossier `trained_model_outputs`.
1. Téléchargez le fichier `conda_env.yaml` et le fichier `score.py`.

    ![Capture d’écran des fichiers téléchargés pour le déploiement dans le volet droit](./media/how-to-deploy-model-designer/download-artifacts-in-right-pane.png)

Vous pouvez également télécharger les fichiers à partir de la page de ressources **Modèles** une fois que vous avez inscrit votre modèle :

1. Accédez à la page de ressources **Modèles**.
1. Sélectionnez le modèle que vous souhaitez déployer.
1. Sélectionnez l’onglet **Artefacts**.
1. Sélectionnez le dossier `trained_model_outputs` .
1. Téléchargez le fichier `conda_env.yaml` et le fichier `score.py`.  

    ![Capture d’écran des fichiers téléchargés pour le déploiement dans la page de détails du modèle](./media/how-to-deploy-model-designer/download-artifacts-in-models-page.png)

> [!NOTE]
> Le fichier `score.py` fournit quasiment les mêmes fonctionnalités que les modules **Noter le modèle**. Toutefois, certains modules tels que [Score SVD Recommender](./algorithm-module-reference/score-svd-recommender.md), [Score Wide and Deep Recommender](./algorithm-module-reference/score-wide-and-deep-recommender.md) et [Score Vowpal Wabbit Model](./algorithm-module-reference/score-vowpal-wabbit-model.md) ont des paramètres pour différents modes de notation. Vous pouvez également modifier ces paramètres dans le script d’entrée.
>
>Pour plus d’informations sur la définition des paramètres du fichier `score.py`, consultez la section [Configurer le script d’entrée](#configure-the-entry-script).

## <a name="deploy-the-model"></a>Déployer le modèle

Une fois que vous avez téléchargé les fichiers nécessaires, vous êtes prêt à déployer le modèle.

1. Dans la page de ressources **Modèles**, sélectionnez le modèle inscrit.
1. Sélectionnez le bouton **Déployer**.
1. Dans le menu Configuration, entrez les informations suivantes :

    - Entrez le nom du point de terminaison.
    - Sélectionnez cette option pour déployer le modèle sur [Azure Kubernetes Service](how-to-deploy-azure-kubernetes-service.md) ou [Azure Container Instance](how-to-deploy-azure-container-instance.md).
    - Chargez `score.py` comme **fichier de script d’entrée**.
    - Chargez `conda_env.yml` comme fichier de dépendances **Conda**. 

    >[!TIP]
    > Dans le paramètre **Avancé**, vous pouvez définir la capacité de l’UC et de la mémoire et d’autres paramètres pour le déploiement. Ces paramètres sont importants pour certains modèles, tels que les modèles PyTorch, qui consomment une quantité considérable de mémoire (environ 4 Go).

1. Sélectionnez **Déployer** pour déployer votre modèle en tant que point de terminaison en temps réel.

    ![Capture d’écran de la page Déployer un modèle dans la page de ressources Modèles](./media/how-to-deploy-model-designer/deploy-model.png)

## <a name="consume-the-real-time-endpoint"></a>Consommer le point de terminaison en temps réel

Une fois le déploiement correctement effectué, vous pouvez trouver le point de terminaison en temps réel dans la page de ressources **Points de terminaison**. Dans cette page, vous trouverez un point de terminaison REST, que les clients peuvent utiliser pour envoyer des demandes au point de terminaison en temps réel. 

> [!NOTE]
> Le concepteur génère également un exemple de fichier JSON de données à des fins de test. Vous pouvez télécharger `_samples.json` dans le dossier **trained_model_outputs**.

Utilisez l’exemple de code suivant pour utiliser un point de terminaison en temps réel.

```python

import json
from pathlib import Path
from azureml.core.workspace import Workspace, Webservice
 
service_name = 'YOUR_SERVICE_NAME'
ws = Workspace.get(
    name='WORKSPACE_NAME',
    subscription_id='SUBSCRIPTION_ID',
    resource_group='RESOURCEGROUP_NAME'
)
service = Webservice(ws, service_name)
sample_file_path = '_samples.json'
 
with open(sample_file_path, 'r') as f:
    sample_data = json.load(f)
score_result = service.run(json.dumps(sample_data))
print(f'Inference result = {score_result}')
```

## <a name="configure-the-entry-script"></a>Configurer le script d’entrée

Certains modules du concepteur tels que [Score SVD Recommender](./algorithm-module-reference/score-svd-recommender.md), [Score Wide and Deep Recommender](./algorithm-module-reference/score-wide-and-deep-recommender.md) et [Score Vowpal Wabbit Model](./algorithm-module-reference/score-vowpal-wabbit-model.md) ont des paramètres pour différents modes de notation. Dans cette section, découvrez comment mettre à jour ces paramètres dans le fichier de script d’entrée.

L’exemple suivant met à jour le comportement par défaut d’un modèle entraîné un modèle **Wide & Deep recommender**. Par défaut, le fichier `score.py` indique au service web de prédire les évaluations entre les utilisateurs et les éléments. 

Vous pouvez modifier le fichier de script d’entrée pour créer des recommandations d’élément et retourner les éléments recommandés en modifiant le paramètre `recommender_prediction_kind`.


```python
import os
import json
from pathlib import Path
from collections import defaultdict
from azureml.studio.core.io.model_directory import ModelDirectory
from azureml.designer.modules.recommendation.dnn.wide_and_deep.score. \
    score_wide_and_deep_recommender import ScoreWideAndDeepRecommenderModule
from azureml.designer.serving.dagengine.utils import decode_nan
from azureml.designer.serving.dagengine.converter import create_dfd_from_dict

model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), 'trained_model_outputs')
schema_file_path = Path(model_path) / '_schema.json'
with open(schema_file_path) as fp:
    schema_data = json.load(fp)


def init():
    global model
    model = ModelDirectory.load(load_from_dir=model_path)


def run(data):
    data = json.loads(data)
    input_entry = defaultdict(list)
    for row in data:
        for key, val in row.items():
            input_entry[key].append(decode_nan(val))

    data_frame_directory = create_dfd_from_dict(input_entry, schema_data)

    # The parameter names can be inferred from Score Wide and Deep Recommender module parameters:
    # convert the letters to lower cases and replace whitespaces to underscores.
    score_params = dict(
        trained_wide_and_deep_recommendation_model=model,
        dataset_to_score=data_frame_directory,
        training_data=None,
        user_features=None,
        item_features=None,
        ################### Note #################
        # Set 'Recommender prediction kind' parameter to enable item recommendation model
        recommender_prediction_kind='Item Recommendation',
        recommended_item_selection='From All Items',
        maximum_number_of_items_to_recommend_to_a_user=5,
        whether_to_return_the_predicted_ratings_of_the_items_along_with_the_labels='True')
    result_dfd, = ScoreWideAndDeepRecommenderModule().run(**score_params)
    result_df = result_dfd.data
    return json.dumps(result_df.to_dict("list"))
```

Pour les modèles **Wide & Deep recommender** et **Vowpal Wabbit**, vous pouvez configurer le paramètre de mode de notation à l’aide des méthodes suivantes :

- Les noms de paramètres sont les combinaisons de minuscules et de traits de soulignement composant les noms de paramètres pour [Score Vowpal Wabbit Model](./algorithm-module-reference/score-vowpal-wabbit-model.md) et [Score Wide and Deep Recommender](./algorithm-module-reference/score-wide-and-deep-recommender.md).
- Les valeurs de paramètre de type de mode sont des chaînes constituées des noms d’options correspondants. Dans le **type de prédiction du générateur de recommandations** dans les codes ci-dessus comme exemple, la valeur peut être `'Rating Prediction'` ou `'Item Recommendation'`. Les autres valeurs ne sont pas autorisées.

Pour le modèle entraîné **SVD recommender**, les noms et valeurs de paramètres peuvent être moins évidents et vous pouvez consulter les tableaux ci-dessous pour décider comment définir des paramètres.

| Nom du paramètre dans le modèle [Score SVD Recommender](./algorithm-module-reference/score-svd-recommender.md)                           | Nom de paramètre dans le fichier de script d’entrée |
| ------------------------------------------------------------ | --------------------------------------- |
| Type de prédiction du générateur de recommandations                                  | prediction_kind                         |
| Sélection d'élément recommandé                                   | recommended_item_selection              |
| Taille minimale du pool de recommandation pour un seul utilisateur    | min_recommendation_pool_size            |
| Nombre maximal d'éléments à recommander à un utilisateur               | max_recommended_item_count              |
| Indique s’il faut retourner les évaluations prédites des éléments avec les étiquettes | return_ratings                          |

Le code suivant vous montre comment définir les paramètres d’un générateur de recommandations SVD, qui utilise les six paramètres pour recommander des éléments classés avec des évaluations prédites jointes.

```python
score_params = dict(
        learner=model,
        test_data=DataTable.from_dfd(data_frame_directory),
        training_data=None,
        # RecommenderPredictionKind has 2 members, 'RatingPrediction' and 'ItemRecommendation'. You
        # can specify prediction_kind parameter with one of them.
        prediction_kind=RecommenderPredictionKind.ItemRecommendation,
        # RecommendedItemSelection has 3 members, 'FromAllItems', 'FromRatedItems', 'FromUndatedItems'.
        # You can specify recommended_item_selection parameter with one of them.
        recommended_item_selection=RecommendedItemSelection.FromRatedItems,
        min_recommendation_pool_size=1,
        max_recommended_item_count=3,
        return_ratings=True,
    )
```


## <a name="next-steps"></a>Étapes suivantes

* [Entraîner un modèle dans le concepteur](tutorial-designer-automobile-price-train-score.md)
* [Résoudre des problèmes d’échec de déploiement](how-to-troubleshoot-deployment.md)
* [Déployer dans Azure Kubernetes Service](how-to-deploy-azure-kubernetes-service.md)
* [Créer des applications clientes pour utiliser des services web](how-to-consume-web-service.md)
* [Mettre à jour un service web](how-to-deploy-update-web-service.md)
