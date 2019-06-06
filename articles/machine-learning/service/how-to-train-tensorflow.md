---
title: TensorFlow modelleri eğitme ve kaydetme
titleSuffix: Azure Machine Learning service
description: Bu makalede eğitme ve Azure Machine Learning hizmetini kullanarak bir TensorFlow modeli kaydetmeyi gösterilmektedir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.date: 05/28/2019
ms.custom: seodec18
ms.openlocfilehash: 4a6f9734a7b2b59035efcbb0f4e2d75f47e053be
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66515607"
---
# <a name="train-and-register-tensorflow-models-at-scale-with-azure-machine-learning-service"></a>Eğitim ve uygun ölçekte TensorFlow modeller Azure Machine Learning hizmeti ile kaydetme

Bu makalede eğitme ve Azure Machine Learning hizmetini kullanarak bir TensorFlow modeli kaydetmeyi gösterilmektedir. Popüler kullanacağız [MNIST dataset](http://yann.lecun.com/exdb/mnist/) kullanılarak oluşturulan bir derin sinir ağı kullanarak resimlerdeki el yazısı basamak sınıflandırmak için [TensorFlow Python Kitaplığı](https://www.tensorflow.org/overview).

Azure Machine Learning hizmeti ile hızlı bir şekilde elastik bulut bilgi işlem kaynakları kullanarak, açık kaynaklı eğitim işleri ölçeklendirmek mümkün olacaktır. Ayrıca mümkün olacaktır İzle eğitim çalıştırmaları, sürüm modelleri, modelleri ve daha fazlasını dağıtın.

Sıfırdan TensorFlow modelden geliştiriyor ister mevcut bir model buluta getirdiğiniz, üretime hazır modeller Azure Machine Learning hizmeti ile de oluşturabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- Yükleme [Azure Machine için Python SDK'sı Learning](setup-create-workspace.md#sdk). İsteğe bağlı: oluşturma bir `config.json` yapılandırma dosyası.
- İndirme [örnek komut dosyalarını](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow) `mnist-tf.py` ve `utils.py`

Ayrıca bir tamamlanmış bulabilirsiniz [Jupyter Not Defteri sürüm](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow/train-hyperparameter-tune-deploy-with-tensorflow.ipynb) Github örnekleri sayfamızda bu kılavuzun. Not Defteri, akıllı hiper parametre ayarı kapsayan genişletilmiş bölüm ve model dağıtımı içerir.

## <a name="set-up-the-experiment"></a>Deneme ayarlama

Bu bölümde, gerekli python paketlerini yükleme, bir çalışma alanı başlatma, deneme oluşturma ve eğitim verilerini ve eğitim betikleriniz Python SDK'yı kullanarak karşıya eğitim denemesini ayarlar.

### <a name="import-packages"></a>Paketleri içeri aktarma

İlk olarak biz gerekli Python kitaplıkları içeri aktarma gerekecektir.

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

İsteğe bağlı bir adım tamamladıysanız [Önkoşullar bölümüne](#prerequisites), kullanabileceğiniz `Workspace.from_config()` hızla yapılandırma dosyasında depolanan ayrıntılarından bir çalışma alanı nesnesi oluşturmak için.

```Python
ws = Workspace.from_config()
```

Bir çalışma alanı açıkça oluşturabilirsiniz:

```Python
ws = Workspace.create(name='<workspace-name>',
                      subscription_id='<azure-subscription-id>',
                      resource_group='<choose-a-resource-group>',
                      create_resource_group=True,
                      location='<select-location>' # For example: 'eastus2'
                      )
```

### <a name="create-an-experiment"></a>Deneme oluşturma

Bir deney ve eğitim betiklerinizi tutmak için bir klasör oluşturun. Bu örnekte, "tf-mnıst" adlı bir deneme oluşturun.

```Python
script_folder = './tf-mnist'
os.makedirs(script_folder, exist_ok=True)

exp = Experiment(workspace=ws, name='tf-mnist')
```

### <a name="upload-dataset-and-scripts"></a>Veri kümesi ve komut dosyaları karşıya yükle

[Veri deposu](how-to-access-data.md) burada veri depolanabilir ve bağlama ya da işlem hedefi için verileri kopyalayarak erişilen bir yerdir. Her çalışma alanı bir varsayılan veri deposu sağlar. Eğitim sırasında kolayca erişilebilir emin ediyoruz verilerimizi ve eğitim betikleriniz yükleyeceksiniz.

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

TensorFlow işinizi çalıştırmak işlem hedefi oluşturmak. Bu örnekte, bir GPU özellikli Azure Machine Learning işlem kümesi oluştururuz. Kullanılabilir eğitim listesi için hedef işlem, bkz: [bu makalede](how-to-set-up-training-targets.md#compute-targets-for-training)

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

## <a name="create-a-tensorflow-estimator"></a>TensorFlow tahmin oluşturma

[TensorFlow estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) işlem hedefi üzerinde bir TensorFlow eğitimi işini başlatmak için basit bir yol sağlar. TensorFlow yüklü olan bir docker görüntüsü oluşturun.

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

Çalıştırma yürütülür gibi aşağıdaki aşamalarının verilmektedir:

- **Hazırlama**: Bir docker görüntüsü TensorFlow estimator göre oluşturulur. Görüntü çalışma alanınızın container registry'ye yüklendi ve sonraki çalıştırmalar için önbelleğe alınır. Günlükler için çalıştırma geçmişi de aktarılır ve ilerleme durumunu izlemek için görüntülenebilir.

- **Ölçeklendirme**: Kümeye daha fazla düğüm çalıştırma şu anda mevcut olandan yürütmek için Batch AI kümesi gerektiriyorsa, ölçeği dener.

- **Çalışan**: Betik klasöründeki tüm betikler işlem hedefine yüklenen, veri depoları bağlı veya kopyalanır ve entry_script yürütülür. Stdout çıkışları ve. / Günlükler klasöründe için çalıştırma geçmişi aktarılır ve çalıştırmasını izlemek için kullanılabilir.

- **İşleme sonrası**: . / Çalışma klasörüne kopyalanır üzerinden için çalıştırma geçmişi çıkarır.

## <a name="register-or-download-a-model"></a>Kaydolun veya bir model indirin

Modeli eğittiğimize sonra çalışma alanınıza kaydedebilirsiniz. Model kaydı sağlar, depolama ve sürüm Modellerinizi basitleştirmek için çalışma alanınızdaki [model yönetimi ve dağıtım](concept-model-management-and-deployment.md).

```Python
model = run.register_model(model_name='tf-dnn-mnist', model_path='outputs/model')
```

Ayrıca, çalışma nesnesini kullanarak model yerel bir kopyasını indirebilirsiniz. Eğitim betiğinde `mnist-tf.py`, TensorFlow koruyucu nesne modeli (işlem hedefine yerel) yerel bir klasöre devam ettirir. Bir kopyasını indirmek için çalışma nesnesi kullanabiliriz.

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

Horovod kullanılacağını belirtin `mpi` için `distributed_training` TensorFlow estimator oluşturucuda parametresi. Horovod eğitim betiğinizde kullanabilmeniz için yüklenir.

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

Parametre sunucusu yönteminin kullanılacağını belirtin `ps` için `distributed_training` TensorFlow estimator oluşturucuda parametresi.

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
                      distributed_backend=distributed_training,
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

TensorFlow'ın yüksek düzeyde için [ `tf.estimator` ](https://www.tensorflow.org/api_docs/python/tf/estimator) API, TensorFlow ayrıştırma bu `TF_CONFIG` değişkeni ve derleme küme için özel.

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

Bu makalede, eğitim ve Azure Machine Learning hizmetinde bir TensorFlow modelin kayıtlı. Model dağıtımı makalemizi açın devam model dağıtma hakkında bilgi edinin.

> [!div class="nextstepaction"]
> [Nasıl ve nerede modelleri dağıtma](how-to-deploy-and-where.md)
