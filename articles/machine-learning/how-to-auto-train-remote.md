---
title: Automatisierte ML-Remotecomputeziele
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie Modelle mit automatisiertem maschinellem Lernen auf einem Azure Machine Learning-Remotecomputeziel mit Azure Machine Learning erstellen.
services: machine-learning
author: cartacioS
ms.author: sacartac
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.workload: data-services
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: afdfe6cd660466a0ff262d5823be0a5104946d6a
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/28/2019
ms.locfileid: "75535650"
---
# <a name="train-models-with-automated-machine-learning-in-the-cloud"></a>Trainieren von Modellen mit automatisiertem maschinellem Lernen in der Cloud

[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In Azure Machine Learning trainieren Sie Ihr Modell mit verschiedenen, von Ihnen verwalteten Arten von Computeressourcen. Beim Computeziel kann es sich um einen lokalen Computer oder eine Ressource in der Cloud handeln.

Sie können Ihr Machine Learning-Experiment bequem durch Hinzufügen zusätzlicher Computeziele wie Azure Machine Learning Compute (AmlCompute) vertikal oder horizontal hochskalieren. AmlCompute ist eine verwaltete Compute-Infrastruktur, mit der Sie ganz einfach ein Computeziel mit einem oder mehreren Knoten erstellen können.

In diesem Artikel erfahren Sie, wie Sie ein Modell mit automatisiertem Machine Learning mit AmlCompute erstellen.

## <a name="how-does-remote-differ-from-local"></a>Remote und lokal – wo liegen die Unterschiede?

Im Tutorial [Train a classification model with automated machine learning (Trainieren eines Klassifizierungsmodells mit automatisiertem maschinellen Lernen)](tutorial-auto-train-models.md) erfahren Sie, wie Sie einen lokalen Computer zum Trainieren von Modellen mit automatisiertem maschinellen Lernen verwenden. Der Workflow zum lokalen Trainieren trifft ebenfalls auf Remoteziele zu. Bei Remotecomputezielen werden jedoch die Experimentiterationen mit automatisiertem maschinellem Lernen asynchron ausgeführt. Diese Funktion ermöglicht es, eine bestimmte Iteration abzubrechen, den Ausführungsstatus anzuzeigen oder im Jupyter Notebook an anderen Zellen weiterzuarbeiten. Damit Sie Ihr Modell remote trainieren können, müssen Sie zunächst ein Remotecomputeziel wie AmlCompute erstellen. Konfigurieren Sie dann die Remoteressource, und übermitteln Sie dort Ihren Code.

Dieser Artikel zeigt die weiteren erforderlichen Schritte, um ein Experiment mit automatisiertem Machine Learning auf einem AmlCompute-Remoteziel auszuführen. Das Arbeitsbereichsobjekt `ws` aus dem Tutorial wird hier im gesamten Code verwendet.

```python
ws = Workspace.from_config()
```

## <a name="create-resource"></a>Erstellen von Ressourcen

Erstellen Sie das AmlCompute-Ziel in Ihrem Arbeitsbereich (`ws`), wenn es noch nicht vorhanden ist.

**Geschätzter Zeitaufwand**: Ein Erstellen des AmlCompute-Ziels dauert etwa fünf Minuten.

```python
from azureml.core.compute import AmlCompute
from azureml.core.compute import ComputeTarget

amlcompute_cluster_name = "automlcl"  # Name your cluster
provisioning_config = AmlCompute.provisioning_configuration(vm_size="STANDARD_D2_V2",
                                                            # for GPU, use "STANDARD_NC6"
                                                            # vm_priority = 'lowpriority', # optional
                                                            max_nodes=6)
compute_target = ComputeTarget.create(
    ws, amlcompute_cluster_name, provisioning_config)

# Can poll for a minimum number of nodes and for a specific timeout.
# If no min_node_count is provided, it will use the scale settings for the cluster.
compute_target.wait_for_completion(
    show_output=True, min_node_count=None, timeout_in_minutes=20)
```

Sie können das `compute_target`-Objekt jetzt als Remotecomputeziel verwenden.

Zu den Einschränkungen für einen Clusternamen gehören:
+ Muss kürzer sein als 64 Zeichen.
+ Darf keines der folgenden Zeichen enthalten: `\` ~! @ # $ % ^ & * ( ) = + _ [ ] { } \\\\ | ; : \' \\" , < > / ?.`

## <a name="access-data-using-tabulardataset-function"></a>Zugreifen auf Daten mit der TabularDataset-Funktion

Definierte training_data als `TabularDataset` und die Bezeichnung, die in der AutoMLConfig-Datei an Automated ML übergeben werden. `from_delimited_files` legt `infer_column_types` standardmäßig auf „true“ fest, wodurch der Spaltentypen automatisch abgeleitet wird. 

Wenn Sie die Spaltentypen manuell festlegen möchten, können Sie das `set_column_types`-Argument so festlegen, dass der Typ der einzelnen Spalten manuell festgelegt wird. Im folgenden Codebeispiel stammen die Daten aus dem sklearn-Paket.

```python
from sklearn import datasets
from azureml.core.dataset import Dataset
from scipy import sparse
import numpy as np
import pandas as pd
import os

# Create a project_folder if it doesn't exist
if not os.path.isdir('data'):
    os.mkdir('data')
    
if not os.path.exists(project_folder):
    os.makedirs(project_folder)

X = pd.DataFrame(data_train.data[100:,:])
y = pd.DataFrame(data_train.target[100:])

# merge X and y
label = "digit"
X[label] = y

training_data = X

training_data.to_csv('data/digits.csv')
ds = ws.get_default_datastore()
ds.upload(src_dir='./data', target_path='digitsdata', overwrite=True, show_progress=True)

training_data = Dataset.Tabular.from_delimited_files(path=ds.path('digitsdata/digits.csv'))
```

## <a name="configure-experiment"></a>Konfigurieren des Experiments
Legen Sie die Einstellungen für `AutoMLConfig` fest.  (Siehe dazu eine [vollständige Liste der Parameter](how-to-configure-auto-train.md#configure-experiment) und ihrer möglichen Werte.)

```python
from azureml.train.automl import AutoMLConfig
import time
import logging

automl_settings = {
    "name": "AutoML_Demo_Experiment_{0}".format(time.time()),
    "experiment_timeout_minutes" : 20,
    "enable_early_stopping" : True,
    "iteration_timeout_minutes": 10,
    "n_cross_validations": 5,
    "primary_metric": 'AUC_weighted',
    "max_concurrent_iterations": 10,
}

automl_config = AutoMLConfig(task='classification',
                             debug_log='automl_errors.log',
                             path=project_folder,
                             compute_target=compute_target,
                             training_data=training_data,
                             label_column_name=label,
                             **automl_settings,
                             )
```

## <a name="submit-training-experiment"></a>Übermitteln des Trainingsexperiments

Übermitteln Sie nun die Konfiguration, um automatisch den Algorithmus und die Hyperparameter festzulegen und das Modell zu trainieren.

```python
from azureml.core.experiment import Experiment
experiment = Experiment(ws, 'automl_remote')
remote_run = experiment.submit(automl_config, show_output=True)
```

Es wird eine Ausgabe angezeigt, die in etwa wie im folgenden Beispiel aussieht:

    Running on remote compute: mydsvmParent Run ID: AutoML_015ffe76-c331-406d-9bfd-0fd42d8ab7f6
    ***********************************************************************************************
    ITERATION: The iteration being evaluated.
    PIPELINE:  A summary description of the pipeline being evaluated.
    DURATION: Time taken for the current iteration.
    METRIC: The result of computing score on the fitted pipeline.
    BEST: The best observed score thus far.
    ***********************************************************************************************

     ITERATION     PIPELINE                               DURATION                METRIC      BEST
             2      Standardize SGD classifier            0:02:36                  0.954     0.954
             7      Normalizer DT                         0:02:22                  0.161     0.954
             0      Scale MaxAbs 1 extra trees            0:02:45                  0.936     0.954
             4      Robust Scaler SGD classifier          0:02:24                  0.867     0.954
             1      Normalizer kNN                        0:02:44                  0.984     0.984
             9      Normalizer extra trees                0:03:15                  0.834     0.984
             5      Robust Scaler DT                      0:02:18                  0.736     0.984
             8      Standardize kNN                       0:02:05                  0.981     0.984
             6      Standardize SVM                       0:02:18                  0.984     0.984
            10      Scale MaxAbs 1 DT                     0:02:18                  0.077     0.984
            11      Standardize SGD classifier            0:02:24                  0.863     0.984
             3      Standardize gradient boosting         0:03:03                  0.971     0.984
            12      Robust Scaler logistic regression     0:02:32                  0.955     0.984
            14      Scale MaxAbs 1 SVM                    0:02:15                  0.989     0.989
            13      Scale MaxAbs 1 gradient boosting      0:02:15                  0.971     0.989
            15      Robust Scaler kNN                     0:02:28                  0.904     0.989
            17      Standardize kNN                       0:02:22                  0.974     0.989
            16      Scale 0/1 gradient boosting           0:02:18                  0.968     0.989
            18      Scale 0/1 extra trees                 0:02:18                  0.828     0.989
            19      Robust Scaler kNN                     0:02:32                  0.983     0.989


## <a name="explore-results"></a>Überprüfen der Ergebnisse

Sie können das gleiche [Jupyter-Widget](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py) wie in diesem [Trainings-Tutorial](tutorial-auto-train-models.md#explore-the-results) verwenden, um ein Diagramm und eine Tabelle mit Ergebnissen anzuzeigen.

```python
from azureml.widgets import RunDetails
RunDetails(remote_run).show()
```

Hier ist ein statisches Bild des Widgets.  Klicken Sie im Notebook auf eine beliebige Zeile in der Tabelle, um die Ausführungseigenschaften und Ausgabeprotokolle für diese Ausführung anzuzeigen.   Sie können auch die Dropdownliste über dem Diagramm verwenden, um ein Diagramm der einzelnen verfügbaren Metriken für jede Iteration anzuzeigen.

![Widget-Tabelle](./media/how-to-auto-train-remote/table.png)
![Widget-Plot](./media/how-to-auto-train-remote/plot.png)

Das Widget zeigt eine URL an, die Sie zum Anzeigen und Überprüfen der Details der einzelnen Ausführungen verwenden können.  

Wenn Sie nicht in einem Jupyter-Notebook arbeiten, können Sie die URL aus der Ausführung selbst anzeigen.

```
remote_run.get_portal_url()
```

Die gleichen Informationen sind in Ihrem Arbeitsbereich verfügbar.  Weitere Informationen zu diesen Ergebnissen finden Sie unter [Grundlegendes zu den Ergebnissen des automatisierten maschinellen Lernens](how-to-understand-automated-ml.md).

## <a name="example"></a>Beispiel

Das folgende [Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/regression/auto-ml-regression.ipynb) veranschaulicht die Konzepte in diesem Artikel.

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, [wie Sie die Einstellungen für das automatische Training konfigurieren](how-to-configure-auto-train.md).
* Weitere Informationen zum Aktivieren von Modellinterpretierbarkeitsmerkmalen in automatisierten ML-Experimenten finden Sie im Artikel zur [Vorgehensweise](how-to-machine-learning-interpretability-automl.md).
