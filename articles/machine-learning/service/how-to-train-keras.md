---
title: Eğitim ve Keras modelleri üzerinde TensorFlow çalıştıran kaydetme
titleSuffix: Azure Machine Learning service
description: Bu makalede TensorFlow üzerinde çalışan bir Keras modeli eğitmek ve Azure Machine Learning hizmetini kullanarak gösterilmektedir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.reviewer: peterlu
ms.date: 06/07/2019
ms.custom: seodec18
ms.openlocfilehash: 9d405b454d755e0c848e9422c8d4cf6e7c505b68
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840060"
---
# <a name="train-and-register-keras-models-at-scale-with-azure-machine-learning-service"></a>Eğitim ve uygun ölçekte Keras modeller Azure Machine Learning hizmeti ile kaydetme

Bu makalede Azure Machine Learning hizmetini kullanarak nasıl eğitmek ve üzerinde TensorFlow derlenen bir Keras modelini kaydettirmek gösterilmektedir. Popüler kullanan [MNIST dataset](http://yann.lecun.com/exdb/mnist/) kullanılarak oluşturulan bir derin sinir ağı (DNN) kullanarak el ile yazılmış rakam sınıflandırmak için [Keras Python Kitaplığı](https://keras.io) üstünde çalışan [TensorFlow](https://www.tensorflow.org/overview) .

Keras üst düzey bir sinir ağı geliştirmeyi kolaylaştıran üst tarafındaki diğer popüler DNN altyapıları çalıştırabilen API ' dir. Azure Machine Learning hizmeti ile eğitim işleri elastik bulut bilgi işlem kaynaklarını kullanarak hızlı bir şekilde ölçeklendirebilirsiniz. Ayrıca izleyebilirsiniz eğitim çalıştırmaları, sürüm modelleri, modelleri ve daha fazlasını dağıtın.

Azure Machine Learning hizmeti, sıfırdan Keras modelden geliştiriyor ister mevcut bir model buluta getirdiğiniz, üretime hazır modelleri oluşturmanıza yardımcı olabilir.

## <a name="prerequisites"></a>Önkoşullar

Bu kod, bu ortamların birini çalıştırın:

 - Azure Machine Learning not defteri VM - herhangi bir indirme veya yükleme gerekli

     - Tamamlamak [bulut tabanlı bir not defteri hızlı](quickstart-run-cloud-notebook.md) adanmış notebook sunucusu önceden yüklenmiş SDK'sı ve örnek depoyu oluşturmak için.
    - Bu dizine giderek notebook sunucusu örnekleri klasöründe, tamamlanan ve genişletilmiş bir not defteri Bul: **Yardım-How-to-kullanın-azureml > Eğitim ile derin öğrenme > train-hyperparameter-tune-deploy-with-keras** klasör. 
 
 - Kendi Jupyter Notebook sunucusu

     - [Azure Machine için Python SDK'sı Learning yükleme](setup-create-workspace.md#sdk)
    - [Çalışma alanı yapılandırma dosyası oluşturma](setup-create-workspace.md#write-a-configuration-file)
    - [Örnek komut dosyalarını indirme](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras) `mnist-keras.py` ve `utils.py`
     
    Ayrıca bir tamamlanmış bulabilirsiniz [Jupyter Not Defteri sürüm](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras/train-hyperparameter-tune-deploy-with-keras.ipynb) GitHub örnekleri sayfasında bu kılavuzun. Not Defteri, akıllı hiper parametre ayarı, model dağıtımı ve not defteri pencere öğeleri kapsayan genişletilmiş bölümler içerir.

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

Bir deney ve eğitim betiklerinizi tutmak için bir klasör oluşturun. Bu örnekte, "mnıst keras" adlı bir deneme oluşturun.

```Python
script_folder = './keras-mnist'
os.makedirs(script_folder, exist_ok=True)

exp = Experiment(workspace=ws, name='keras-mnist')
```

### <a name="upload-dataset-and-scripts"></a>Veri kümesi ve komut dosyaları karşıya yükle

[Veri deposu](how-to-access-data.md) burada veri depolanabilir ve bağlama ya da işlem hedefi için verileri kopyalayarak erişilen bir yerdir. Her çalışma alanı bir varsayılan veri deposu sağlar. Eğitim sırasında kolayca erişilebilir olacak şekilde eğitim betikleri ve verileri veri deposuna yükleyin.

1. Yerel olarak MNIST veri kümesini indirin.

    ```Python
    os.makedirs('./data/mnist', exist_ok=True)

    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz', filename = './data/mnist/train-images.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz', filename = './data/mnist/train-labels.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz', filename = './data/mnist/test-images.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz', filename = './data/mnist/test-labels.gz')
    ```

1. MNIST veri kümesi, varsayılan veri deposuna yükleyin.

    ```Python
    ds = ws.get_default_datastore()
    ds.upload(src_dir='./data/mnist', target_path='mnist', overwrite=True, show_progress=True)
    ```

1. Keras eğitim betiğini yükleme `keras_mnist.py`ve yardımcı dosyanın `utils.py`.

    ```Python
    shutil.copy('./keras_mnist.py', script_folder)
    shutil.copy('./utils.py', script_folder)
    ```

## <a name="create-a-compute-target"></a>İşlem hedefi oluşturmak

TensorFlow işinizi çalıştırmak işlem hedefi oluşturmak. Bu örnekte, bir GPU özellikli Azure Machine Learning işlem kümesini oluşturun.

```Python
cluster_name = "gpucluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6', 
                                                           max_nodes=4)

    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

İşlem hedefleri hakkında daha fazla bilgi için bkz. [işlem hedefi nedir](concept-compute-target.md) makalesi.

## <a name="create-a-tensorflow-estimator-and-import-keras"></a>TensorFlow estimator ve Keras alın

[TensorFlow estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) işlem hedefte TensorFlow eğitim işleri başlatılıyor, basit bir yol sağlar. Keras TensorFlow üzerinde çalıştığından, TensorFlow tahmin Aracı'nı kullanın ve Keras kitaplığı kullanarak içeri aktarma `pip_packages` bağımsız değişken.

TensorFlow estimator genel uygulanır [ `estimator` ](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) herhangi bir çerçeveyi desteklemek için kullanılan sınıf. Genel tahmin Aracı'nı kullanarak modellerin eğitimi hakkında daha fazla bilgi için bkz. [tahmin Aracı'nı kullanarak Azure Machine Learning modellerini eğitin](how-to-train-ml-models.md)

```Python
script_params = {
    '--data-folder': ds.path('mnist').as_mount(),
    '--batch-size': 50,
    '--first-layer-neurons': 300,
    '--second-layer-neurons': 100,
    '--learning-rate': 0.001
}

est = TensorFlow(source_directory=script_folder,
                 entry_script='keras_mnist.py',
                 script_params=script_params,
                 compute_target=compute_target,
                 pip_packages=['keras', 'matplotlib'],
                 use_gpu=True)
```

## <a name="submit-a-run"></a>Bir farklı çalıştır gönderin

[Nesnesini çalıştırmak](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) işi devam ederken ve tamamlandıktan sonra çalıştırma geçmişini arabirim sağlar.

```Python
run = exp.submit(est)
run.wait_for_completion(show_output=True)
```

Çalıştırma yürütülür gibi aşağıdaki aşamalarının geçer:

- **Hazırlama**: Bir docker görüntüsü TensorFlow estimator göre oluşturulur. Görüntü çalışma alanınızın container registry'ye yüklendi ve sonraki çalıştırmalar için önbelleğe alınır. Günlükler için çalıştırma geçmişi de aktarılır ve ilerleme durumunu izlemek için görüntülenebilir.

- **Ölçeklendirme**: Daha fazla düğüm çalıştırma şu anda mevcut olandan yürütmek için Batch AI kümesi gerektiriyorsa, Ölçek kümesi çalışır.

- **Çalışan**: Betik klasöründeki tüm betikler işlem hedefine yüklenen, veri depoları bağlı veya kopyalanır ve entry_script yürütülür. Stdout çıkışları ve. / Günlükler klasöründe için çalıştırma geçmişi aktarılır ve çalıştırmasını izlemek için kullanılabilir.

- **İşleme sonrası**: . / Çalışma klasörüne kopyalanır üzerinden için çalıştırma geçmişi çıkarır.

## <a name="register-the-model"></a>Modeli kaydedin

Modeli eğittiğimize sonra çalışma alanınıza kaydedebilirsiniz. Model kaydı sağlar, depolama ve sürüm Modellerinizi basitleştirmek için çalışma alanınızdaki [model yönetimi ve dağıtım](concept-model-management-and-deployment.md).

```Python
model = run.register_model(model_name='keras-dnn-mnist', model_path='outputs/model')
```

Modeli yerel bir kopyasını da indirebilirsiniz. Bu, yerel olarak ek model doğrulama iş yapmak için yararlı olabilir. Eğitim betiğinde `mnist-keras.py`, TensorFlow koruyucu nesne modeli (işlem hedefine yerel) yerel bir klasöre devam ettirir. Çalıştırma nesneyi veri deposundan bir kopyasını indirmek için kullanabilirsiniz.

```Python
# Create a model folder in the current directory
os.makedirs('./model', exist_ok=True)

for f in run.get_file_names():
    if f.startswith('outputs/model'):
        output_file_path = os.path.join('./model', f.split('/')[-1])
        print('Downloading from {} to {} ...'.format(f, output_file_path))
        run.download_file(name=f, output_file_path=output_file_path)
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, eğitim ve Azure Machine Learning hizmetinde bir Keras modelin kayıtlı. Model dağıtımı makalemizi açın devam model dağıtma hakkında bilgi edinin.

> [!div class="nextstepaction"]
> [Nasıl ve nerede modelleri dağıtma](how-to-deploy-and-where.md)
