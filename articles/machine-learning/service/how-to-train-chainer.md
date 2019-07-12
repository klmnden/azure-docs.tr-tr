---
title: Bağlayıcı modelleri eğitme ve kaydetme
titleSuffix: Azure Machine Learning service
description: Bu makalede eğitme ve Azure Machine Learning hizmetini kullanarak bir bağlayıcı modeli kaydetmeyi gösterilmektedir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.reviewer: sdgilley
ms.date: 06/15/2019
ms.openlocfilehash: 8ecefccbdf5f02652e935858b6ae8fb4cdfde640
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840030"
---
# <a name="train-and-register-chainer-models-at-scale-with-azure-machine-learning-service"></a>Eğitim ve uygun ölçekte Chainer modeller Azure Machine Learning hizmeti ile kaydetme

Bu makalede eğitme ve Azure Machine Learning hizmetini kullanarak bir bağlayıcı modeli kaydetmeyi gösterilmektedir. Popüler kullanan [MNIST dataset](http://yann.lecun.com/exdb/mnist/) kullanılarak oluşturulan bir derin sinir ağı (DNN) kullanarak el ile yazılmış rakam sınıflandırmak için [Chainer Python Kitaplığı](https://Chainer.org) üstünde çalışan [numpy](https://www.numpy.org/).

Bağlayıcı, üst düzey bir sinir ağı geliştirmeyi kolaylaştıran diğer popüler DNN altyapıları üstte çalıştırabilen API ' dir. Azure Machine Learning hizmeti ile eğitim işleri elastik bulut bilgi işlem kaynaklarını kullanarak hızlı bir şekilde ölçeklendirebilirsiniz. Ayrıca izleyebilirsiniz eğitim çalıştırmaları, sürüm modelleri, modelleri ve daha fazlasını dağıtın.

Azure Machine Learning hizmeti, sıfırdan Chainer modelden geliştiriyor ister mevcut bir model buluta getirdiğiniz, üretime hazır modelleri oluşturmanıza yardımcı olabilir.

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree) bugün.

## <a name="prerequisites"></a>Önkoşullar

Bu kod, bu ortamların birini çalıştırın:

- Azure Machine Learning not defteri VM - herhangi bir indirme veya yükleme gerekli

    - Tamamlamak [bulut tabanlı bir not defteri hızlı](quickstart-run-cloud-notebook.md) adanmış notebook sunucusu önceden yüklenmiş SDK'sı ve örnek depoyu oluşturmak için.
    - Not Defteri sunucusu örnekleri klasöründe, tamamlanmış bir not defteri ve dosya Bul **how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-chainer** klasör.  Not Defteri, akıllı hiper parametre ayarı, model dağıtımı ve not defteri pencere öğeleri kapsayan genişletilmiş bölümler içerir.

- Kendi Jupyter Notebook sunucusu

    - [Azure Machine için Python SDK'sı Learning yükleme](setup-create-workspace.md#sdk)
    - [Çalışma alanı yapılandırma dosyası oluşturma](setup-create-workspace.md#write-a-configuration-file)
    - Örnek komut dosyasını indirme [chainer_mnist.py](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-chainer/chainer_mnist.py)
     - Ayrıca bir tamamlanmış bulabilirsiniz [Jupyter Not Defteri sürüm](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-chainer/train-hyperparameter-tune-deploy-with-chainer.ipynb) GitHub örnekleri sayfasında bu kılavuzun. Not Defteri, akıllı hiper parametre ayarı, model dağıtımı ve not defteri pencere öğeleri kapsayan genişletilmiş bölümler içerir.

## <a name="set-up-the-experiment"></a>Deneme ayarlama

Bu bölümde, gerekli python paketlerini yükleme, bir çalışma alanı başlatma, deneme oluşturma ve eğitim verilerini ve eğitim betikleriniz karşıya eğitim denemesini ayarlar.

### <a name="import-packages"></a>Paketleri içeri aktarma

İlk olarak, Python kitaplığı ad görüntü sürüm numarasını azureml.core içeri aktarın.

```
# Check core SDK version number
import azureml.core

print("SDK version:", azureml.core.VERSION)
```

### <a name="initialize-a-workspace"></a>Bir çalışma alanını Başlat

[Azure Machine Learning hizmeti çalışma alanında](concept-workspace.md) hizmeti için en üst düzey bir kaynaktır. Oluşturduğunuz tüm yapıları ile çalışma için merkezi bir yerde sağlar. Python SDK'da çalışma yapıları oluşturarak erişebileceğiniz bir [ `workspace` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) nesne.

Bir çalışma alanı nesne oluşturmak `config.json` 'te dosya oluşturulduğunda [Önkoşullar bölümüne](#prerequisites).

```Python
ws = Workspace.from_config()
```

### <a name="create-a-project-directory"></a>Proje dizini oluşturma
Uzak kaynak erişimi için gerekir, yerel makinenizde tüm gerekli kodu içeren bir dizin oluşturun. Bu eğitim betiğini ve eğitim betiğinizi bağımlı başka dosyalar içerir.

```
import os

project_folder = './chainer-mnist'
os.makedirs(project_folder, exist_ok=True)
```

### <a name="prepare-training-script"></a>Eğitim betiği hazırlama

Bu öğreticide, bir eğitim betiğini **chainer_mnist.py** zaten size sağlanır. Uygulamada, olan ve uygulamayı Azure ML ile kodunuzu değiştirmek zorunda kalmadan herhangi özel bir eğitim betiğini alın mümkün olması gerekir.

Azure ML'ın izleme ve ölçümler özellikleri kullanmak için az miktarda bir eğitim betiğinizi içinde Azure ML kod eklemeniz gerekir.  Eğitim betiğini **chainer_mnist.py** bazı ölçümler, Azure ML çalıştırmak için oturum işlemi gösterilmektedir. Bunu yapmak için Azure ML erişim `Run` betik içinde nesne.

Eğitim betiği kopyalayın **chainer_mnist.py** , proje dizinine.

```
import shutil

shutil.copy('chainer_mnist.py', project_folder)
```

### <a name="create-an-experiment"></a>Deneme oluşturma

Bir deney ve eğitim betiklerinizi tutmak için bir klasör oluşturun. Bu örnekte, "bağlayıcı mnıst" adlı bir deneme oluşturun.

```
from azureml.core import Experiment

experiment_name = 'chainer-mnist'
experiment = Experiment(ws, name=experiment_name)
```


## <a name="create-or-get-a-compute-target"></a>Oluşturun veya bir işlem hedefine alın

İhtiyacınız olacak bir [hedef işlem](concept-compute-target.md) modelinizi eğitim için. Bu öğreticide, Azure ML yönetilen bilgi işlem (AmlCompute) için Uzak eğitim işlem kaynağı kullanır.

**Oluşturma, AmlCompute yaklaşık 5 dakika sürer**. Bu ada sahip AmlCompute çalışma alanınızda zaten varsa, bu kod oluşturma işlemini atlar.  

```Python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# choose a name for your cluster
cluster_name = "gpu-cluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target.')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6', 
                                                           max_nodes=4)

    # create the cluster
    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True)

# use get_status() to get a detailed status for the current cluster. 
print(compute_target.get_status().serialize())
```

İşlem hedefleri hakkında daha fazla bilgi için bkz. [işlem hedefi nedir](concept-compute-target.md) makalesi.

## <a name="create-a-chainer-estimator"></a>Bir bağlayıcı tahmin oluşturma

[Chainer estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) işlem hedef Chainer eğitim işleri başlatılıyor, basit bir yol sağlar.

Bağlayıcı estimator genel uygulanır [ `estimator` ](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) herhangi bir çerçeveyi desteklemek için kullanılan sınıf. Genel tahmin Aracı'nı kullanarak modellerin eğitimi hakkında daha fazla bilgi için bkz. [tahmin Aracı'nı kullanarak Azure Machine Learning modellerini eğitin](how-to-train-ml-models.md)

```Python
from azureml.train.dnn import Chainer

script_params = {
    '--epochs': 10,
    '--batchsize': 128,
    '--output_dir': './outputs'
}

estimator = Chainer(source_directory=project_folder, 
                    script_params=script_params,
                    compute_target=compute_target,
                    pip_packages=['numpy', 'pytest'],
                    entry_script='chainer_mnist.py',
                    use_gpu=True)
```

## <a name="submit-a-run"></a>Bir farklı çalıştır gönderin

[Nesnesini çalıştırmak](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) işi devam ederken ve tamamlandıktan sonra çalıştırma geçmişini arabirim sağlar.

```Python
run = exp.submit(est)
run.wait_for_completion(show_output=True)
```

Çalıştırma yürütülür gibi aşağıdaki aşamalarının geçer:

- **Hazırlama**: Bir docker görüntüsü Chainer estimator göre oluşturulur. Görüntü çalışma alanınızın container registry'ye yüklendi ve sonraki çalıştırmalar için önbelleğe alınır. Günlükler için çalıştırma geçmişi de aktarılır ve ilerleme durumunu izlemek için görüntülenebilir.

- **Ölçeklendirme**: Daha fazla düğüm çalıştırma şu anda mevcut olandan yürütmek için Batch AI kümesi gerektiriyorsa, Ölçek kümesi çalışır.

- **Çalışan**: Betik klasöründeki tüm betikler işlem hedefine yüklenen, veri depoları bağlı veya kopyalanır ve entry_script yürütülür. Stdout çıkışları ve. / Günlükler klasöründe için çalıştırma geçmişi aktarılır ve çalıştırmasını izlemek için kullanılabilir.

- **İşleme sonrası**: . / Çalışma klasörüne kopyalanır üzerinden için çalıştırma geçmişi çıkarır.

## <a name="save-and-register-the-model"></a>Kaydet ve modeli kaydedin

Modeli eğittiğimize sonra Kaydet ve çalışma alanınıza kaydedin. Model kaydı sağlar, depolama ve sürüm Modellerinizi basitleştirmek için çalışma alanınızdaki [model yönetimi ve dağıtım](concept-model-management-and-deployment.md).

Eğitim betiğe aşağıdaki kodu ekleyin **chainer_mnist.py**, modeli kaydedin. 

``` Python
    serializers.save_npz(os.path.join(args.output_dir, 'model.npz'), model)
```

Aşağıdaki kod ile çalışma alanınıza modeli kaydedin.

```Python
model = run.register_model(model_name='chainer-dnn-mnist', model_path='outputs/model.npz')
```



## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Machine Learning hizmetinde bir bağlayıcı modelin eğitim. 

* Model dağıtma konusunda bilgi almak için devam etmek için sunduğumuz [model dağıtım](how-to-deploy-and-where.md) makalesi.

* [Hiperparametreleri ayarlama](how-to-tune-hyperparameters.md)

* [İzleme ölçümlerini eğitim sırasında çalıştırın](how-to-track-experiments.md)
