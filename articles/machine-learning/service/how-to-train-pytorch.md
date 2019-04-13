---
title: PyTorch ile eğitme modelleri
titleSuffix: Azure Machine Learning service
description: Tek düğümlü ve dağıtılmış eğitimi PyTorch modelleri ile PyTorch estimator çalıştırmayı öğrenin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 9ae7795381f036bb819ce24554d8cea94ceb5552
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59548560"
---
# <a name="train-pytorch-models-with-azure-machine-learning-service"></a>Azure Machine Learning hizmeti ile PyTorch modellerini eğitin

PyTorch kullanarak derin sinir ağı (DNN) eğitim için özel bir Azure Machine Learning sağlar [PyTorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py) sınıfının `Estimator`. Azure SDK'ın `PyTorch` estimator kolayca Azure işlem hem tek düğümlü hem de dağıtılmış çalıştırmalar için PyTorch eğitim işleri göndermenizi sağlar.

## <a name="single-node-training"></a>Tek düğümlü eğitim
İle eğitim `PyTorch` estimator kullanmaya benzer [temel `Estimator` ](how-to-train-ml-models.md), bu nedenle öncelikle yapılır makaleyi okuyun ve orada tanıtılan kavramları anladığınızdan emin olun.
  
PyTorch işi çalıştırmak için örneği bir `PyTorch` nesne. Oluşturmuş olmanız, [hedef işlem](how-to-set-up-training-targets.md#amlcompute) nesne `compute_target` ve [veri deposu](how-to-access-data.md) nesne `ds`.

```Python
from azureml.train.dnn import PyTorch

script_params = {
    '--data_dir': ds
}

pt_est = PyTorch(source_directory='./my-pytorch-proj',
                 script_params=script_params,
                 compute_target=compute_target,
                 entry_script='train.py',
                 use_gpu=True)
```

Burada, PyTorch oluşturucuya aşağıdaki parametreleri belirtin:

Parametre | Açıklama
--|--
`source_directory` |  Eğitim işine yönelik gerekli kodunuzun tamamını içeren yerel dizin. Bu klasörü yerel makinenizden uzak bilgisayarda kopyalanır
`script_params` |  Eğitim betiğinizi komut satırı bağımsız değişkenleri belirtme sözlük `entry_script`, < komut satırı bağımsız değişkeni, değer > biçiminde çiftleri.  Ayrıntılı bir bayrak belirlemek için `script_params`, kullanın `<command-line argument, "">`.
`compute_target` |  Eğitim betiğinizi, bu örnekte, bir Azure Machine Learning işlem çalıştıracak uzak işlem hedefine ([AmlCompute](how-to-set-up-training-targets.md#amlcompute)) kümesi
`entry_script` |  FilePath (göreli `source_directory`) eğitim betiğin uzak işlem üzerinde çalıştırılacak. Bu dosya ve, bağımlı herhangi bir ek dosyaları bu klasörde bulunmalıdır
`conda_packages` |  Eğitim betiğinizi gerekli conda aracılığıyla yüklenecek Python paketleri listesi. Oluşturucu adlı başka bir parametreye sahip `pip_packages` gereken herhangi bir pip paketleri için kullanabileceğiniz
`use_gpu` |  Bu bayrağı ayarlanmış `True` eğitim GPU yararlanmak için. Varsayılan olarak `False`

Kullanmakta olduğunuz beri `PyTorch` tahmin, eğitim için kullanılan kapsayıcı PyTorch paket ve CPU ve gpu üzerinde eğitim için gereken ilgili bağımlılıkları içerecektir.

Ardından, PyTorch işi gönder:
```Python
run = exp.submit(pt_est)
```

## <a name="distributed-training"></a>Dağıtılmış eğitimi
`PyTorch` Tahmin aracı da Azure Vm'lerini CPU ve GPU kümeleri arasında uygun ölçekte Modellerinizi eğitmek olanak sağlar. Azure Machine Learning tüm altyapı ve bu iş yüklerinin ölçeğini gerçekleştirmek için gereken düzenleme arka planda yönetecek karşın dağıtılmış PyTorch eğitimi birkaç API çağrısı ile kolayca çalıştırabilirsiniz.

Azure Machine Learning şu anda dağıtılmış eğitimi MPI tabanlı PyTorch Horovod framework kullanarak, destekler.

### <a name="horovod"></a>Horovod
[Horovod](https://github.com/uber/horovod) için Dağıtılmış eğitimi Uber tarafından geliştirilen bir açık kaynak halka-allreduce çerçevesidir.

Dağıtılmış PyTorch Horovod çerçevesini kullanarak çalıştırmak için PyTorch nesnesini şu şekilde oluşturun:

```Python
from azureml.train.dnn import PyTorch

pt_est = PyTorch(source_directory='./my-pytorch-project',
                 script_params={},
                 compute_target=compute_target,
                 entry_script='train.py',
                 node_count=2,
                 process_count_per_node=1,
                 distributed_backend='mpi',
                 use_gpu=True)
```

Bu kod, aşağıdaki yeni parametreleri PyTorch oluşturucuya kullanıma sunar:

Parametre | Açıklama | Varsayılan
--|--|--
`node_count` |  Eğitim işine yönelik kullanmak için düğüm sayısı. | `1`
`process_count_per_node` |  Her bir düğümde çalıştırılacak işlemleri (veya "çalışanları") sayısı. | `1`
`distributed_backend` |  Başlatmak için arka uç MPI Estimator sunan eğitim dağıtılmış.  Paralel veya dağıtılmış eğitimini yürütmek için (örneğin `node_count`> 1 veya `process_count_per_node`> 1 veya her ikisi) MPI (ve Horovod) sahip, `distributed_backend='mpi'`. Azure Machine Learning tarafından kullanılan MPI uygulamasıdır [açık MPI](https://www.open-mpi.org/). | `None`

Yukarıdaki örnekte, bir çalışan düğümü başına iki arkadaşlarınızla dağıtılmış eğitimi çalıştırılır.

Bu eğitim betiğinizde yalnızca içeri aktarılabilmesini sağlamak Horovod ve bağımlılıklarını sizin için yüklenecek `train.py` gibi:
```Python
import torch
import horovod
```

Son olarak, dağıtılmış PyTorch iş gönderme:
```Python
run = exp.submit(pt_est)
```

## <a name="examples"></a>Örnekler

Dağıtılmış derin öğrenme dizüstü bilgisayarlar için bkz:
* [How-to-use-azureml/Training-With-DEEP-Learning](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar
* [İzleme ölçümlerini eğitim sırasında çalıştırın](how-to-track-experiments.md)
* [Hiperparametreleri ayarlama](how-to-tune-hyperparameters.md)
* [Eğitilen model dağıtma](how-to-deploy-and-where.md)
