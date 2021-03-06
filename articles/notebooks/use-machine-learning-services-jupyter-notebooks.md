---
title: Verwenden von Azure Machine Learning in Azure Notebooks
description: Eine Übersicht der Beispielnotebooks für Azure Machine Learning, die Sie mit Azure Notebooks verwenden können.
ms.topic: article
ms.date: 12/04/2018
ms.openlocfilehash: e3c4fbdf35bf7ea1f4dddbceb9d7235c67ed13a4
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2019
ms.locfileid: "74277453"
---
# <a name="use-azure-machine-learning-in-a-notebook"></a>Verwenden von Azure Machine Learning in einem Notebook

Azure Notebooks ist mit der erforderlichen Umgebung für das Arbeiten mit [Azure Machine Learning](/azure/machine-learning/service/) vorkonfiguriert. Sie können einfach ein Beispielprojekt in Ihr Notebooks-Konto klonen, um eine Vielzahl von Machine Learning-Szenarien zu untersuchen.

## <a name="clone-the-sample-into-your-account"></a>Klonen des Beispiels in Ihr Konto

1. Melden Sie sich bei [Azure Notebooks](https://notebooks.azure.com/) an.
1. Wählen Sie **Meine Projekte** aus, um zum Projektdashboard zu wechseln.
1. Wählen Sie die Schaltfläche **Upload GitHub Repo** (GitHub-Repository hochladen, den Pfeil nach oben) aus, um das Popupfenster **Upload GitHub Repository** (GitHub-Repository hochladen) zu öffnen.
1. Geben Sie im Popupfenster in **GitHub-Repository** den Text `Azure/MachineLearningNotebooks` ein, geben Sie in **Projektname** einen Namen für das Projekt an, z. B. „Azure Machine Learning“, geben Sie in **Projekt-ID** einen Bezeichner ein, deaktivieren Sie auf Wunsch **Öffentlich**, und wählen Sie dann **Importieren** aus.

    ![Importieren des Azure Machine Learning Notebook-Beispiels in Ihr Notebooks-Konto](media/azureml-import-project.png)

1. Nach einer oder zwei Minuten navigiert Azure Notebooks automatisch zum Dashboard des neuen Projekts.

## <a name="run-a-sample-notebook"></a>Ausführen eines Beispielnotebook

1. Wählen Sie **00 - configuration.ipynb** aus, um den Konfigurationsabschnitt des Notebooks zu starten, und befolgen Sie seine Anweisungen, um einen Azure Machine Learning-Arbeitsbereich zu erstellen.

    - Da Azure Notebooks bereits die erforderlichen Python-Pakete enthält, können Sie einfach den Codeausschnitt in Schritt 2 der Voraussetzungen ausführen, um die Azure ML SDK-Version zu überprüfen.

1. Wählen Sie nach dem Abschluss der Konfiguration **01.getting-started** aus, um den Ordner zu öffnen, der dreizehn verschiedene Beispielnotebooks enthält, die allesamt selbsterklärend sind.

## <a name="next-steps"></a>Nächste Schritte

Die Dokumentation zu Azure Machine Learning enthält eine Vielzahl weiterer Ressourcen, um Sie durch die Arbeit mit Machine Learning innerhalb von Notebooks zu führen:

- [Schnellstart: Verwenden von Python für die ersten Schritte mit Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/quickstart-create-workspace-with-python)
- [Tutorial 1: Trainieren eines Bildklassifizierungsmodells mit Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/tutorial-train-models-with-aml)
- [Tutorial 2: Bereitstellen eines Bildklassifizierungsmodells in Azure Container Instances (ACI)](https://docs.microsoft.com/azure/machine-learning/service/tutorial-deploy-models-with-aml)
- [Tutorial: Vorhersagen von Preisen für Taxifahrten mit automatisiertem maschinellem Lernen](https://docs.microsoft.com/azure/machine-learning/service/tutorial-auto-train-models)

Lesen Sie außerdem die Dokumentation zum [Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).
