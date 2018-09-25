---
title: Azure Machine Learning ile TensorFlow modellerini eğitin
description: Tek düğümlü ve dağıtılmış eğitimi TensorFlow modelleri ile TensorFlow estimator çalıştırmayı öğrenin
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: ba43593e90b78aaa0083faf4f8162a7663c0ad47
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46974230"
---
# <a name="how-to-train-tensorflow-models"></a>TensorFlow modelleri eğitme

TensorFlow kullanarak derin sinir ağı (DNN) eğitimi, Azure Machine Learning, bir özel Estimator TensorFlow sınıfı sağlar. Azure SDK'ın TensorFlow Estimator (ile conflated değil için [ `tf.estimator.Estimator` ](https://www.tensorflow.org/api_docs/python/tf/estimator/Estimator) sınıfı) kolayca Azure işlem çalışır hem tek düğümlü hem de dağıtılmış TensorFlow eğitim işleri göndermenizi sağlar.

## <a name="single-node-training"></a>Tek düğümlü eğitim
TensorFlow Estimator eğitimlerle kullanmaya benzer [Estimator temel](how-to-train-ml-models.md), bu nedenle öncelikle yapılır makaleyi okuyun ve orada tanıtılan kavramları anladığınızdan emin olun.
  
TensorFlow işi çalıştırmak için örneği bir `TensorFlow` nesne. Oluşturmuş olmanız, [hedef işlem](how-to-set-up-training-targets.md#batch) nesne `compute_target`.

```Python
from azureml.train.dnn import TensorFlow

script_params = {
    '--batch-size': 50,
    '--learning-rate': 0.01,
}

tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params=script_params,
                    compute_target=compute_target,
                    entry_script='train.py',
                    conda_packages=['scikit-learn'],
                    use_gpu=True)
```

Burada, TensorFlow oluşturucuya aşağıdaki parametreleri belirtin:
* `source_directory`: Tüm eğitim işine yönelik gerekli kodunuzu içeren yerel bir dizin. Bu klasörü yerel makinenizden uzak bilgisayarda kopyalanır
* `script_params`: Eğitim betiğinizi komut satırı bağımsız değişkenleri belirtme bir sözlük `entry_script`, < komut satırı bağımsız değişkeni, değer > biçiminde çiftleri
* `compute_target`: Eğitim betiğinizi, bu durumda çalışır uzak işlem bir [Batch AI](how-to-set-up-training-targets.md#batch) küme
* `entry_script`: Dosya yolunu (göreli `source_directory`) eğitim betiğin uzak işlem üzerinde çalıştırılacak. Bu dosya ve, bağımlı herhangi bir ek dosyaları bu klasörde bulunmalıdır
* `conda_packages`: Python paketlerini eğitim betiğinizi gerekli conda aracılığıyla yüklenecek listesi. Bu durumda eğitim betiği kullanır `sklearn` verileri yüklemek için bu nedenle yüklenmesi için bu paketi belirtin.  
Oluşturucu adlı başka bir parametreye sahip `pip_packages` gereken herhangi bir pip paketleri için kullanabileceğiniz
* `use_gpu`: Bu bayrağı ayarlanmış `True` eğitim GPU yararlanmak için. Varsayılan olarak `False`.

Eğitim için kullanılan kapsayıcı TensorFlow estimator kullandığından, varsayılan TensorFlow paket ve CPU ve gpu üzerinde eğitim için gereken ilgili bağımlılıklar içerir.

Ardından, TensorFlow işi gönder:
```Python
run = exp.submit(tf_est)
```

## <a name="distributed-training"></a>Dağıtılmış eğitimi
TensorFlow Estimator Azure VM'lerin CPU ve GPU kümeleri arasında uygun ölçekte Modellerinizi eğitmek sağlar. Azure Machine Learning tüm altyapı ve bu iş yüklerinin ölçeğini gerçekleştirmek için gereken düzenleme arka planda yönetecek karşın dağıtılmış TensorFlow eğitimi birkaç API çağrısı ile kolayca çalıştırabilirsiniz.

Azure Machine Learning TensorFlow, dağıtılmış Eğitim'in iki yöntemi destekler:
1. MPI tabanlı kullanarak eğitim dağıtılmış [Horovod](https://github.com/uber/horovod) framework
2. yerel [dağıtılmış TensorFlow](https://www.tensorflow.org/deploy/distributed) aracılığıyla parametresi sunucu yöntemi

### <a name="horovod"></a>Horovod
[Horovod](https://github.com/uber/horovod) için Dağıtılmış eğitimi Uber tarafından geliştirilen bir açık kaynak halka-allreduce çerçevesidir.

Dağıtılmış TensorFlow Horovod çerçevesini kullanarak çalıştırmak için TensorFlow nesnesini şu şekilde oluşturun:

```Python
from azureml.train.dnn import TensorFlow

tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params={},
                    compute_target=compute_target,
                    entry_script='train.py',
                    node_count=2,
                    process_count_per_node=1,
                    distributed_backend='mpi',
                    use_gpu=True)
```

Yukarıdaki kod, aşağıdaki yeni parametreleri TensorFlow oluşturucuya kullanıma sunar:
* `node_count`:, Eğitim işine yönelik kullanmak için düğüm sayısı. Varsayılan olarak bu bağımsız değişken `1`
* `process_count_per_node`: Her bir düğümde çalıştırılacak işlemleri (veya "çalışanları") sayı. Varsayılan olarak bu bağımsız değişken `1`
* `distributed_backend`: Başlatmak için arka uç MPI Estimator sunan eğitim dağıtılmış. Bu bağımsız değişken varsayılan olarak `None`. Paralel veya dağıtılmış eğitimini gerçekleştirmek istiyorsanız (örneğin `node_count`> 1 veya `process_count_per_node`> 1 veya her ikisi) MPI (ve Horovod) sahip, `distributed_backend='mpi'`. Azure Machine Learning tarafından kullanılan MPI uygulamasıdır [açık MPI](https://www.open-mpi.org/).

Yukarıdaki örnekte, bir çalışan düğümü başına iki arkadaşlarınızla dağıtılmış eğitimi çalıştırılır.

Bu eğitim betiğinizde yalnızca içeri aktarılabilmesini sağlamak Horovod ve bağımlılıklarını sizin için yüklenecek `train.py` gibi:
```Python
import tensorflow as tf
import horovod
```

Son olarak, TensorFlow işi gönder:
```Python
run = exp.submit(tf_est)
```

### <a name="parameter-server"></a>Parametre sunucusu
Ayrıca çalıştırabileceğiniz [yerel dağıtılmış TensorFlow](https://www.tensorflow.org/deploy/distributed), parametre sunucu modeli kullanır. Bu yöntemde, parametre sunucularının ve çalışan bir küme genelinde eğitin. Parametre sunucuları gradyanlar toplama sırasında çalışan gradyanlar eğitim sırasında hesaplayın.

TensorFlow nesnesi oluşturun:
```Python
from azureml.train.dnn import TensorFlow

tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params={},
                    compute_target=compute_target,
                    entry_script='train.py',
                    node_count=2,
                    worker_count=2,
                    parameter_server_count=1,
                    distributed_backend='ps',
                    use_gpu=True)
```

Aşağıdaki parametreleri yukarıdaki kodda TensorFlow oluşturucuya dikkat edin:
* `worker_count`: Çalışan sayısı. Varsayılan olarak bu bağımsız değişken `1`
* `parameter_server_count`: Parametresi sunucu sayısı. Varsayılan olarak bu bağımsız değişken `1`
* `distributed_backend`: Eğitim için kullanılacak arka uç dağıtılmış. Bu bağımsız değişken varsayılan olarak `None`. Parametre sunucusu aracılığıyla dağıtılmış eğitimi yapmak için ayarlamanız gerekir `distributed_backend='ps'`

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

Kullanıyorsanız, TensorFlow, üst düzey [ `tf.estimator` ](https://www.tensorflow.org/api_docs/python/tf/estimator) API, TensorFlow ayrıştırma bu `TF_CONFIG` değişkeni ve derleme küme için özel. 

Eğitim için bunun yerine TensorFlow'ın alt düzey core API kullanıyorsanız, ayrıştırma gerek `TF_CONFIG` değişkeni ve derleme `tf.train.ClusterSpec` kendiniz eğitim kodunuzda. İçinde [Bu örnek](https://aka.ms/aml-notebook-tf-ps), bu nedenle de yaptığınız **eğitim betiğinizi** gibi:

```Python
import os, json
import tensorflow as tf

tf_config = os.environ.get('TF_CONFIG')
if not tf_config or tf_config == "":
    raise ValueError("TF_CONFIG not found.")
tf_config_json = json.loads(tf_config)
cluster_spec = tf.train.ClusterSpec(cluster)

```

Bir kez eğitim betiğinizi yazma bitirdikten ve TensorFlow nesnesi oluşturulurken, eğitim işinizi gönderebilirsiniz:
```Python
run = exp.submit(tf_est)
```

## <a name="examples"></a>Örnekler
Tek düğümlü TensorFlow eğitimi hakkında bir öğretici için bkz:
* `training/03.train-tune-deploy-tensorflow/03.train-tune-deploy-tensorflow.ipynb`

Dağıtılmış TensorFlow Horovod ile temel bir öğretici için bkz:
* `training/04.distributed-tensorflow-with-horovod/04.distributed-tensorflow-with-horovod.ipynb`

Yerel dağıtılmış TensorFlow hakkında bir öğretici için bkz:
* `training/05.distributed-tensorflow-with-parameter-server/05.distributed-tensorflow-with-parameter-server.ipynb`

Bu not defterlerini alın:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar
* [İzleme ölçümlerini eğitim sırasında çalıştırın](how-to-track-experiments.md)
* [Hiperparametreleri ayarlama](how-to-tune-hyperparameters.md)
* [Eğitilen model dağıtma](how-to-deploy-and-where.md)
