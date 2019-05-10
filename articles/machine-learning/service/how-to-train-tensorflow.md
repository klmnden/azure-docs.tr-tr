---
title: TensorFlow & Keras modellerini eğitin
titleSuffix: Azure Machine Learning service
description: Tek düğümlü ve dağıtılmış eğitimi TensorFlow ve Keras modelleri ile TensorFlow ve Keras estimators çalıştırmayı öğrenin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 05/06/2019
ms.custom: seodec18
ms.openlocfilehash: 0d5751ab96dc6b44229e2b18b832a570930058ca
ms.sourcegitcommit: 4891f404c1816ebd247467a12d7789b9a38cee7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65442357"
---
# <a name="train-tensorflow-and-keras-models-with-azure-machine-learning-service"></a>Azure Machine Learning hizmeti ile TensorFlow ve Keras modellerini eğitin

Azure işlemi üzerinde kolayca TensorFlow eğitim işleri çalıştırabilirsiniz kullanarak [ `TensorFlow` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) Azure Machine Learning SDK'da estimator sınıfı. `TensorFlow` Estimator TensorFlow özellikli bir kapsayıcı için derin sinir ağı (DNN) eğitim işinizi çalıştırmak için Azure Machine Learning hizmetine yönlendirir.

`TensorFlow` Estimator ayrıca sağlar bir Soyutlama Katmanı üzerinden kolayca farklı işlem hedefleri parametreli çalışır, eğitim betikleriniz değiştirmeden yapılandırabilirsiniz, yani yürütme.

## <a name="get-started"></a>başlarken

Bu yana `TensorFlow` estimator sınıfı için temel benzer [ `Estimator` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py), ilk okuma öneririz [tahmin aracı ile ilgili nasıl yapılır makalesi](how-to-train-ml-models.md) ıpam'da kavramları anlamak için.

Azure Machine Learning hizmeti ile kullanmaya başlamak için [hızlı başlangıcı tamamlamak](quickstart-run-cloud-notebook.md). İşleminizi tamamladıktan sonra sahip olacaksınız bir [Azure Machine Learning çalışma alanı](concept-azure-machine-learning-architecture.md#workspace) ve tüm müşterilerimize [örnek not defterleri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml) eğitim Dnn'leri TensorFlow ve Keras için dahil olmak üzere.

## <a name="single-node-training"></a>Tek düğümlü eğitim

TensorFlow işi çalıştırmak için örneği bir [ `TensorFlow` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) nesnesi ve bir deney olarak gönderin.

Aşağıdaki kod, TensorFlow estimator örneğini oluşturur ve bir deney gönderir. Eğitim betiğini `train.py` belirtilen betik parametreleri kullanılarak çalıştırılır. İş, bir GPU özellikli üzerinde çalıştırılacak [hedef işlem](how-to-set-up-training-targets.md)ve scikit-olacak bilgi yüklenmesi için bir bağımlılık olarak `train.py`.

```Python
from azureml.train.dnn import TensorFlow

# training script parameters passed as command-line arguments
script_params = {
    '--batch-size': 50,
    '--learning-rate': 0.01,
}

# TensorFlow constructor
tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params=script_params,
                    compute_target=compute_target,
                    entry_script='train.py', # relative path to your TensorFlow job
                    conda_packages=['scikit-learn'],
                    use_gpu=True)

# submit the TensorFlow job
run = exp.submit(tf_est)
```

## <a name="distributed-training"></a>Dağıtılmış eğitimi

[ `TensorFlow` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) Estimator ayrıca CPU ve GPU kümeleri arasında dağıtılmış eğitimi destekler. Dağıtılmış TensorFlow işleri kolayca çalıştırabilir ve Azure Machine Learning hizmeti için düzenleme ve altyapı yönetir.

Azure Machine Learning hizmeti TensorFlow, dağıtılmış Eğitim'in iki yöntemi destekler:

* [MPI tabanlı](https://www.open-mpi.org/) kullanarak eğitim dağıtılmış [Horovod](https://github.com/uber/horovod) framework
* Yerel [dağıtılmış TensorFlow](https://www.tensorflow.org/deploy/distributed) parametresi sunucu yöntemi kullanarak

### <a name="horovod"></a>Horovod

[Horovod](https://github.com/uber/horovod) için Dağıtılmış eğitimi Uber tarafından geliştirilen bir açık kaynak çerçevesidir. Dağıtılmış TensorFlow GPU işleri kolay bir yolunu sunar.

Aşağıdaki örnek, iki düğüm arasında dağıtılmış iki arkadaşlarınızla Horovod kullanarak dağıtılmış eğitim işini çalıştırır.

```Python
from azureml.train.dnn import TensorFlow

# Tensorflow constructor
tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params={},
                    compute_target=compute_target,
                    entry_script='train.py', # relative path to your TensorFlow job
                    node_count=2,
                    process_count_per_node=1,
                    distributed_backend='mpi', # specifies Horovod backend
                    use_gpu=True)

# submit the TensorFlow job
run = exp.submit(tf_est)
```

Eğitim betiğinizde alabilmeniz Horovod ve bağımlılıklarını sizin için yüklenecek.

```Python
import tensorflow as tf
import horovod
```

### <a name="parameter-server"></a>Parametre sunucusu

Ayrıca çalıştırabileceğiniz [yerel dağıtılmış TensorFlow](https://www.tensorflow.org/deploy/distributed), parametre sunucu modeli kullanır. Bu yöntemde, parametre sunucularının ve çalışan bir küme genelinde eğitin. Parametre sunucuları gradyanlar toplama sırasında çalışan gradyanlar eğitim sırasında hesaplayın.

Aşağıdaki örnek, iki düğüm arasında dağıtılmış dört arkadaşlarınızla parametresi sunucu yöntemi kullanarak dağıtılmış eğitim işini çalıştırır.

```Python
from azureml.train.dnn import TensorFlow

# Tensorflow constructor
tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params={},
                    compute_target=compute_target,
                    entry_script='train.py', # relative path to your TensorFlow job
                    node_count=2,
                    worker_count=2,
                    parameter_server_count=1,
                    distributed_backend='ps', # specifies parameter server backend
                    use_gpu=True)

# submit the TensorFlow job
run = exp.submit(tf_est)
```

#### <a name="note-on-tfconfig"></a>Not alın `TF_CONFIG`

Ayrıca küme için bağlantı noktaları ve ağ adresleri gerekir [ `tf.train.ClusterSpec` ](https://www.tensorflow.org/api_docs/python/tf/train/ClusterSpec), Azure Machine Learning ayarlar `TF_CONFIG` sizin için ortam değişkeni.

`TF_CONFIG` Ortam değişkenidir bir JSON dizesi. Parametre sunucusu için değişkenin bir örnek aşağıda verilmiştir:

```
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

TensorFlow'ın alt düzey için çekirdek API'ler eğitim, ayrıştırma `TF_CONFIG` değişkeni ve derleme `tf.train.ClusterSpec` eğitim kodunuzda. İçinde [Bu örnek](https://aka.ms/aml-notebook-tf-ps), bu nedenle de yaptığınız **eğitim betiğinizi** gibi:

```Python
import os, json
import tensorflow as tf

tf_config = os.environ.get('TF_CONFIG')
if not tf_config or tf_config == "":
    raise ValueError("TF_CONFIG not found.")
tf_config_json = json.loads(tf_config)
cluster_spec = tf.train.ClusterSpec(cluster)

```

## <a name="keras-support"></a>Keras desteği

[Keras](https://keras.io/) bir popüler, üst düzey DNN Python TensorFlow, CNTK ve arka uçları olarak Theano destekleyen bir API'dir. TensorFlow arka uç olarak kullanıyorsanız, Keras ekleme dahil olmak üzere olarak basit bir `pip_package` Oluşturucu parametresi.

Aşağıdaki örnek örnekleyen bir [ `TensorFlow` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) tahmin aracı ve bir deney gönderir. Tahmin Keras eğitim betiği çalıştıran `keras_train.py`. İş, bir gpu özellikli üzerinde çalıştırılacak [hedef işlem](how-to-set-up-training-targets.md) Keras pip aracılığıyla bağımlılık olarak yüklü olan.

```Python
from azureml.train.dnn import TensorFlow

keras_est = TensorFlow(source_directory='./my-keras-proj',
                       script_params=script_params,
                       compute_target=compute_target,
                       entry_script='keras_train.py', # relative path to your TensorFlow job
                       pip_packages=['keras'], # add keras through pip
                       use_gpu=True)
```

## <a name="export-to-onnx"></a>ONNX için dışarı aktarma

İle en iyi duruma getirilmiş çıkarım almak için [ONNX çalışma zamanı](concept-onnx.md), eğitilen TensorFlow modelinizi ONNX biçimine dönüştürebilirsiniz. Bkz: [örnek](https://github.com/onnx/tensorflow-onnx/blob/master/examples/call_coverter_via_python.py).

## <a name="examples"></a>Örnekler

Çeşitli çerçeveleri kullanarak hem tek düğümlü hem de dağıtılmış TensorFlow yürütme için çalışma kod örnekleri bulabilirsiniz [GitHub sayfamızdan](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning).

## <a name="next-steps"></a>Sonraki adımlar

* [İzleme ölçümlerini eğitim sırasında çalıştırın](how-to-track-experiments.md)
* [Hiperparametreleri ayarlama](how-to-tune-hyperparameters.md)
* [Eğitilen model dağıtma](how-to-deploy-and-where.md)
