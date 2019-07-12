---
title: TensorFlow modelleri eğitme ve kaydetme
titleSuffix: Azure Machine Learning service
description: Bu makalede eğitme ve Azure Machine Learning hizmetini kullanarak bir TensorFlow modeli kaydetmeyi gösterilmektedir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.date: 06/10/2019
ms.custom: seodec18
ms.openlocfilehash: 67263df319063cdf21dadea257dcab05ba0d5f7b
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840006"
---
# <a name="train-and-register-tensorflow-models-at-scale-with-azure-machine-learning-service"></a>Eğitim ve uygun ölçekte TensorFlow modeller Azure Machine Learning hizmeti ile kaydetme

Bu makalede eğitme ve Azure Machine Learning hizmetini kullanarak bir TensorFlow modeli kaydetmeyi gösterilmektedir. Popüler kullanan [MNIST dataset](http://yann.lecun.com/exdb/mnist/) kullanılarak oluşturulan bir derin sinir ağı kullanarak resimlerdeki el yazısı basamak sınıflandırmak için [TensorFlow Python Kitaplığı](https://www.tensorflow.org/overview).

TensorFlow derin sinir ağı (DNN) oluşturmak için yaygın olarak kullanılan bir açık kaynak hesaplama çerçevesidir. Azure Machine Learning hizmeti ile açık kaynaklı eğitim işleri elastik bulut bilgi işlem kaynaklarını kullanarak hızlı bir şekilde ölçeklendirebilirsiniz. Ayrıca izleyebilirsiniz eğitim çalıştırmaları, sürüm modelleri, modelleri ve daha fazlasını dağıtın.

Sıfırdan TensorFlow modelden geliştirirken ya da sizin getirdiğiniz bir [varolan modeli](how-to-deploy-existing-model.md) buluta, Azure Machine Learning hizmeti üretime hazır modelleri oluşturmanıza yardımcı olabilir.

## <a name="prerequisites"></a>Önkoşullar

Bu kod, bu ortamların birini çalıştırın:

 - Azure Machine Learning not defteri VM - herhangi bir indirme veya yükleme gerekli

     - Tamamlamak [bulut tabanlı bir not defteri hızlı](quickstart-run-cloud-notebook.md) adanmış notebook sunucusu önceden yüklenmiş SDK'sı ve örnek depoyu oluşturmak için.
    - Bu dizine giderek notebook sunucusu örnekleri klasöründe, tamamlanan ve genişletilmiş bir not defteri Bul: **Yardım-How-to-kullanın-azureml > Eğitim ile derin öğrenme > train-hyperparameter-tune-deploy-with-tensorflow**klasör. 
 
 - Kendi Jupyter Notebook sunucusu

     - [Azure Machine için Python SDK'sı Learning yükleme](setup-create-workspace.md#sdk)
    - [Çalışma alanı yapılandırma dosyası oluşturma](setup-create-workspace.md#write-a-configuration-file)
    - [Örnek komut dosyalarını indirme](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow) `mnist-tf.py` ve `utils.py`
     
    Ayrıca bir tamamlanmış bulabilirsiniz [Jupyter Not Defteri sürüm](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow/train-hyperparameter-tune-deploy-with-tensorflow.ipynb) GitHub örnekleri sayfasında bu kılavuzun. Not Defteri, akıllı hiper parametre ayarı, model dağıtımı ve not defteri pencere öğeleri kapsayan genişletilmiş bölümler içerir.

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

Bir deney ve eğitim betiklerinizi tutmak için bir klasör oluşturun. Bu örnekte, "tf-mnıst" adlı bir deneme oluşturun.

```Python
script_folder = './tf-mnist'
os.makedirs(script_folder, exist_ok=True)

exp = Experiment(workspace=ws, name='tf-mnist')
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

1. TensorFlow eğitimi betiğini yükleme `tf_mnist.py`ve yardımcı dosyanın `utils.py`.

    ```Python
    shutil.copy('./tf_mnist.py', script_folder)
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

## <a name="create-a-tensorflow-estimator"></a>TensorFlow tahmin oluşturma

[TensorFlow estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) işlem hedefi üzerinde bir TensorFlow eğitimi işini başlatmak için basit bir yol sağlar.

TensorFlow estimator genel uygulanır [ `estimator` ](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) herhangi bir çerçeveyi desteklemek için kullanılan sınıf. Genel tahmin Aracı'nı kullanarak modellerin eğitimi hakkında daha fazla bilgi için bkz. [tahmin Aracı'nı kullanarak Azure Machine Learning modellerini eğitin](how-to-train-ml-models.md)

Eğitim betiğinizi ek pip ya da conda paketleri çalıştırmak için gerekiyorsa, paketlerin adlarını aracılığıyla geçirerek elde edilen bir docker görüntüsü üzerinde yüklü olabilir `pip_packages` ve `conda_packages` bağımsız değişkenler.

```Python
script_params = {
    '--data-folder': ws.get_default_datastore().as_mount(),
    '--batch-size': 50,
    '--first-layer-neurons': 300,
    '--second-layer-neurons': 100,
    '--learning-rate': 0.01
}

est = TensorFlow(source_directory=script_folder,
                 entry_script='tf_mnist.py',
                 script_params=script_params,
                 compute_target=compute_target,
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

## <a name="register-or-download-a-model"></a>Kaydolun veya bir model indirin

Modeli eğittiğimize sonra çalışma alanınıza kaydedebilirsiniz. Model kaydı sağlar, depolama ve sürüm Modellerinizi basitleştirmek için çalışma alanınızdaki [model yönetimi ve dağıtım](concept-model-management-and-deployment.md).

```Python
model = run.register_model(model_name='tf-dnn-mnist', model_path='outputs/model')
```

Ayrıca, çalışma nesnesini kullanarak model yerel bir kopyasını indirebilirsiniz. Eğitim betiğinde `mnist-tf.py`, TensorFlow koruyucu nesne modeli (işlem hedefine yerel) yerel bir klasöre devam ettirir. Çalışma nesnesinin bir kopyasını indirmek için kullanabilirsiniz.

```Python
# Create a model folder in the current directory
os.makedirs('./model', exist_ok=True)

for f in run.get_file_names():
    if f.startswith('outputs/model'):
        output_file_path = os.path.join('./model', f.split('/')[-1])
        print('Downloading from {} to {} ...'.format(f, output_file_path))
        run.download_file(name=f, output_file_path=output_file_path)
```

## <a name="distributed-training"></a>Dağıtılmış eğitimi

[ `TensorFlow` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) Estimator ayrıca CPU ve GPU kümeleri arasında dağıtılmış eğitimi destekler. Dağıtılmış TensorFlow işleri kolayca çalıştırabilir ve düzenleme için Azure Machine Learning hizmeti yönetir.

Azure Machine Learning hizmeti TensorFlow, dağıtılmış Eğitim'in iki yöntemi destekler:

- [MPI tabanlı](https://www.open-mpi.org/) kullanarak eğitim dağıtılmış [Horovod](https://github.com/uber/horovod) framework
- Yerel [dağıtılmış TensorFlow](https://www.tensorflow.org/deploy/distributed) parametresi sunucu yöntemi kullanarak

### <a name="horovod"></a>Horovod

[Horovod](https://github.com/uber/horovod) için Dağıtılmış eğitimi Uber tarafından geliştirilen bir açık kaynak çerçevesidir. Dağıtılmış TensorFlow GPU işleri kolay bir yolunu sunar.

Horovod kullanmak için belirtin bir [ `MpiConfiguration` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.mpiconfiguration?view=azure-ml-py) nesnesi `distributed_training` TensorFlow oluşturucuda parametresi. Bu parametre, eğitim betiğinizde kullanabilmeniz için Horovod kitaplığı yüklendiğini sağlar.

```Python
from azureml.train.dnn import TensorFlow

# Tensorflow constructor
estimator= TensorFlow(source_directory=project_folder,
                      compute_target=compute_target,
                      script_params=script_params,
                      entry_script='script.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_training=MpiConfiguration(),
                      framework_version='1.13',
                      use_gpu=True)
```

### <a name="parameter-server"></a>Parametre sunucusu

Ayrıca çalıştırabileceğiniz [yerel dağıtılmış TensorFlow](https://www.tensorflow.org/deploy/distributed), parametre sunucu modeli kullanır. Bu yöntemde, parametre sunucularının ve çalışan bir küme genelinde eğitin. Parametre sunucuları gradyanlar toplama sırasında çalışan gradyanlar eğitim sırasında hesaplayın.

Parametre sunucusu yöntemi kullanmak için belirtin bir [ `TensorflowConfiguration` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.tensorflowconfiguration?view=azure-ml-py) nesnesi `distributed_training` TensorFlow oluşturucuda parametresi.

```Python
from azureml.train.dnn import TensorFlow

distributed_training = TensorflowConfiguration()
distributed_training.worker_count = 2

# Tensorflow constructor
estimator= TensorFlow(source_directory=project_folder,
                      compute_target=compute_target,
                      script_params=script_params,
                      entry_script='script.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_training=distributed_training,
                      use_gpu=True)

# submit the TensorFlow job
run = exp.submit(tf_est)
```

#### <a name="define-cluster-specifications-in-tfconfig"></a>Küme 'TF_CONFIG' belirtimlerinde tanımlayın

Ayrıca küme için bağlantı noktaları ve ağ adresleri gerekir [ `tf.train.ClusterSpec` ](https://www.tensorflow.org/api_docs/python/tf/train/ClusterSpec), Azure Machine Learning ayarlar `TF_CONFIG` sizin için ortam değişkeni.

`TF_CONFIG` Ortam değişkenidir bir JSON dizesi. Parametre sunucusu için değişkenin bir örnek aşağıda verilmiştir:

```JSON
TF_CONFIG='{
    "cluster": {
        "ps": ["host0:2222", "host1:2222"],
        "worker": ["host2:2222", "host3:2222", "host4:2222"],
    },
    "task": {"type": "ps", "index": 0},
    "environment": "cloud"
}'
```

TensorFlow'ın yüksek düzeyde için [ `tf.estimator` ](https://www.tensorflow.org/api_docs/python/tf/estimator) API, TensorFlow ayrıştırır `TF_CONFIG` değişkeni ve küme için özel yapılar.

TensorFlow'ın alt düzey için çekirdek API'ler eğitim, ayrıştırma `TF_CONFIG` değişkeni ve derleme `tf.train.ClusterSpec` eğitim kodunuzda.

```Python
import os, json
import tensorflow as tf

tf_config = os.environ.get('TF_CONFIG')
if not tf_config or tf_config == "":
    raise ValueError("TF_CONFIG not found.")
tf_config_json = json.loads(tf_config)
cluster_spec = tf.train.ClusterSpec(cluster)

```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, eğitim ve TensorFlow modeli kayıtlı. GPU model dağıtım makalemizi açın devam model GPU özellikli bir kümeye dağıtmayı öğrenin.

[Çıkarım gpu'larla for dağıtma](how-to-deploy-inferencing-gpus.md)
[Tensorboard ile izleme](how-to-monitor-tensorboard.md)
