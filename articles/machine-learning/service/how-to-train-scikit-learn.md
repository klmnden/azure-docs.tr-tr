---
title: Eğitim ve scikit kaydetme-modelleri bilgi edinin
titleSuffix: Azure Machine Learning service
description: Bu makalede eğitmek ve bir scikit kaydettirmek gösterilmektedir-bilgi modeli Azure Machine Learning hizmetini kullanarak.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.date: 06/30/2019
ms.custom: seodec18
ms.openlocfilehash: c9e983f7981c1155964617694d2cce86aba741b7
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840015"
---
# <a name="train-and-register-scikit-learn-models-at-scale-with-azure-machine-learning-service"></a>Eğitim ve Scikit-öğrenme modellerini ölçeğe uygun Azure Machine Learning hizmeti ile kaydetme

Bu makalede eğitme ve Azure Machine Learning hizmetini kullanarak Scikit-bilgi modeli kaydetmeyi gösterilmektedir. Popüler kullanan [Iris veri kümesini](https://archive.ics.uci.edu/ml/datasets/iris) süsen Çiçeği özel görüntülerle sınıflandırmak için [scikit-bilgi](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py) sınıfı.

Scikit-öğrenme, machine learning için yaygın olarak kullanılan bir açık kaynaklı bilgi işlem altyapısıdır. Azure Machine Learning hizmeti ile açık kaynaklı eğitim işleri elastik bulut bilgi işlem kaynaklarını kullanarak hızlı bir şekilde ölçeklendirebilirsiniz. Ayrıca izleyebilirsiniz eğitim çalıştırmaları, sürüm modelleri, modelleri ve daha fazlasını dağıtın.

Azure Machine Learning hizmeti, sıfırdan Scikit-öğrenme modelden geliştiriyor ister mevcut bir model buluta getirdiğiniz, üretime hazır modelleri oluşturmanıza yardımcı olabilir.

## <a name="prerequisites"></a>Önkoşullar

Bu kod, bu ortamların birini çalıştırın:
 - Azure Machine Learning not defteri VM - herhangi bir indirme veya yükleme gerekli

    - Tamamlamak [bulut tabanlı bir not defteri hızlı](quickstart-run-cloud-notebook.md) adanmış notebook sunucusu önceden yüklenmiş SDK'sı ve örnek depoyu oluşturmak için.
    - Bu dizine giderek notebook sunucusu örnekleri klasöründe, tamamlanan ve genişletilmiş bir not defteri Bul: **Yardım-How-to-kullanın-azureml > Eğitim > train-hyperparameter-tune-deploy-with-sklearn** klasör.

 - Kendi Jupyter Notebook sunucusu

    - [Azure Machine için Python SDK'sı Learning yükleme](setup-create-workspace.md#sdk)
    - [Çalışma alanı yapılandırma dosyası oluşturma](setup-create-workspace.md#write-a-configuration-file)
    - [Örnek betik dosyasını indirin](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/train-hyperparameter-tune-deploy-with-sklearn) `train_iris.py`
    - Ayrıca bir tamamlanmış bulabilirsiniz [Jupyter Not Defteri sürüm](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-hyperparameter-tune-deploy-with-keras/train-hyperparameter-tune-deploy-with-sklearn.ipynb) GitHub örnekleri sayfasında bu kılavuzun. Not Defteri, akıllı hiper parametre ayarlama ve en iyi modeli tarafından birincil ölçümleri alma kapsayan genişletilmiş bir bölüm içerir.

## <a name="set-up-the-experiment"></a>Deneme ayarlama

Bu bölümde, gerekli python paketlerini yükleme, bir çalışma alanı başlatma, deneme oluşturma ve eğitim verilerini ve eğitim betikleriniz karşıya eğitim denemesini ayarlar.

### <a name="import-packages"></a>Paketleri içeri aktarma

İlk olarak, gerekli Python kitaplıkları içeri aktarın.

```Python
import os
import urllib
import shutil
import azureml

from azureml.core import Experiment
from azureml.core import Workspace, Run

from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException
```

### <a name="initialize-a-workspace"></a>Bir çalışma alanını Başlat

[Azure Machine Learning hizmeti çalışma alanında](concept-workspace.md) hizmeti için en üst düzey bir kaynaktır. Oluşturduğunuz tüm yapıları ile çalışma için merkezi bir yerde sağlar. Python SDK'da çalışma yapıları oluşturarak erişebileceğiniz bir [ `workspace` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) nesne.

Bir çalışma alanı nesne oluşturmak `config.json` 'te dosya oluşturulduğunda [Önkoşullar bölümüne](#prerequisites).

```Python
ws = Workspace.from_config()
```

### <a name="create-an-experiment"></a>Deneme oluşturma

Bir deney ve eğitim betiklerinizi tutmak için bir klasör oluşturun. Bu örnekte, "Iris sklearn" adlı bir deneme oluşturun.

```Python
project_folder = './sklearn-iris'
os.makedirs(project_folder, exist_ok=True)

exp = Experiment(workspace=ws, name='sklearn-iris')
```

### <a name="upload-dataset-and-scripts"></a>Veri kümesi ve komut dosyaları karşıya yükle

[Veri deposu](how-to-access-data.md) burada veri depolanabilir ve bağlama ya da işlem hedefi için verileri kopyalayarak erişilen bir yerdir. Her çalışma alanı bir varsayılan veri deposu sağlar. Eğitim sırasında kolayca erişilebilir olacak şekilde eğitim betikleri ve verileri veri deposuna yükleyin.

1. Verileriniz için bir dizin oluşturun.

    ```Python
    os.makedirs('./data/iris', exist_ok=True)
    ```

1. Iris veri kümesini, varsayılan veri deposuna yükleyin.

    ```Python
    ds = ws.get_default_datastore()
    ds.upload(src_dir='./data/iris', target_path='iris', overwrite=True, show_progress=True)
    ```

1. Karşıya yükleme Scikit-öğrenme eğitim betiğini `train_iris.py`.

    ```Python
    shutil.copy('./train_iris.py', project_folder)
    ```

## <a name="create-or-get-a-compute-target"></a>Oluşturun veya bir işlem hedefine alın

Scikit-öğrenme işinizi çalıştırmak işlem hedefi oluşturmak. Scikit öğrenin yalnızca destekleyen tek düğüm, CPU bilgi işlem.

Aşağıdaki kod, uzak eğitim işlem kaynağınızın yönetilen Azure Machine Learning işlem (AmlCompute) oluşturur. Oluşturma, AmlCompute yaklaşık 5 dakika sürer. Bu ada sahip AmlCompute çalışma alanınızda zaten varsa, bu kod oluşturma işlemini atlar.

```Python
cluster_name = "cpu-cluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2', 
                                                           max_nodes=4)

    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

İşlem hedefleri hakkında daha fazla bilgi için bkz. [işlem hedefi nedir](concept-compute-target.md) makalesi.

## <a name="create-a-scikit-learn-estimator"></a>Scikit-öğrenme tahmin aracı oluşturma

[Scikit-öğrenme tahmin](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) Scikit-öğrenme başlatma basit bir yol sağlayan işlem hedefi üzerinde eğitim işi. Aracılığıyla uygulanır [ `SKLearn` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py) tek düğümlü CPU eğitim desteklemek için kullanılan sınıf.

Eğitim betiğinizi ek pip ya da conda paketleri çalıştırmak için gerekiyorsa, paketlerin adlarını aracılığıyla geçirerek elde edilen bir docker görüntüsü üzerinde yüklü olabilir `pip_packages` ve `conda_packages` bağımsız değişkenler.

```Python
from azureml.train.sklearn import SKLearn

script_params = {
    '--kernel': 'linear',
    '--penalty': 1.0,
}

estimator = SKLearn(source_directory=project_folder, 
                    script_params=script_params,
                    compute_target=compute_target,
                    entry_script='train_iris.py'
                    pip_packages=['joblib']
                   )
```

## <a name="submit-a-run"></a>Bir farklı çalıştır gönderin

[Nesnesini çalıştırmak](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) işi devam ederken ve tamamlandıktan sonra çalıştırma geçmişini arabirim sağlar.

```Python
run = experiment.submit(estimator)
run.wait_for_completion(show_output=True)
```

Çalıştırma yürütülür gibi aşağıdaki aşamalarının geçer:

- **Hazırlama**: Bir docker görüntüsü TensorFlow estimator göre oluşturulur. Görüntü çalışma alanınızın container registry'ye yüklendi ve sonraki çalıştırmalar için önbelleğe alınır. Günlükler için çalıştırma geçmişi de aktarılır ve ilerleme durumunu izlemek için görüntülenebilir.

- **Ölçeklendirme**: Daha fazla düğüm çalıştırma şu anda mevcut olandan yürütmek için Batch AI kümesi gerektiriyorsa, Ölçek kümesi çalışır.

- **Çalışan**: Betik klasöründeki tüm betikler işlem hedefine yüklenen, veri depoları bağlı veya kopyalanır ve entry_script yürütülür. Stdout çıkışları ve. / Günlükler klasöründe için çalıştırma geçmişi aktarılır ve çalıştırmasını izlemek için kullanılabilir.

- **İşleme sonrası**: . / Çalışma klasörüne kopyalanır üzerinden için çalıştırma geçmişi çıkarır.

## <a name="save-and-register-the-model"></a>Kaydet ve modeli kaydedin

Modeli eğittiğimize sonra Kaydet ve çalışma alanınıza kaydedin. Model kaydı sağlar, depolama ve sürüm Modellerinizi basitleştirmek için çalışma alanınızdaki [model yönetimi ve dağıtım](concept-model-management-and-deployment.md).

Eğitim komut dosyanıza, train_iris.py, model kaydetmek için aşağıdaki kodu ekleyin. 

``` Python
import joblib

joblib.dump(svm_model_linear, 'model.joblib')
```

Aşağıdaki kod ile çalışma alanınıza modeli kaydedin.

```Python
model = run.register_model(model_name='sklearn-iris', model_path='model.joblib')
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, eğitim ve Azure Machine Learning hizmetinde bir Scikit-öğrenme modelin kayıtlı.

* Model dağıtma konusunda bilgi almak için devam etmek için sunduğumuz [model dağıtım](how-to-deploy-and-where.md) makalesi.

* [Hiperparametreleri ayarlama](how-to-tune-hyperparameters.md)

* [İzleme ölçümlerini eğitim sırasında çalıştırın](how-to-track-experiments.md)