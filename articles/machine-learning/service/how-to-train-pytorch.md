---
title: Azure Machine Learning ile PyTorch modellerini eğitin
description: Tek düğümlü ve dağıtılmış eğitimi PyTorch modelleri ile PyTorch estimator çalıştırmayı öğrenin
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: e569b63f676fb750bcbab88dda6cda39156d41f5
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46977046"
---
# <a name="how-to-train-pytorch-models"></a>PyTorch modelleri eğitme

PyTorch kullanarak derin sinir ağı (DNN) eğitimi, Azure Machine Learning, bir özel Estimator PyTorch sınıfı sağlar. Azure SDK'ın PyTorch Estimator kolayca Azure işlem hem tek düğümlü hem de dağıtılmış çalıştırmalar için PyTorch eğitim işleri göndermenizi sağlar.

## <a name="single-node-training"></a>Tek düğümlü eğitim
PyTorch Estimator eğitimlerle kullanmaya benzer [Estimator temel](how-to-train-ml-models.md), bu nedenle öncelikle yapılır makaleyi okuyun ve orada tanıtılan kavramları anladığınızdan emin olun.
  
PyTorch işi çalıştırmak için örneği bir `PyTorch` nesne. Oluşturmuş olmanız, [hedef işlem](how-to-set-up-training-targets.md#batch) nesne `compute_target` ve [veri deposu](how-to-access-data.md) nesne `ds`.

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
* `source_directory`: Tüm eğitim işine yönelik gerekli kodunuzu içeren yerel bir dizin. Bu klasörü yerel makinenizden uzak bilgisayarda kopyalanır
* `script_params`: Eğitim betiğinizi komut satırı bağımsız değişkenleri belirtme bir sözlük `entry_script`, < komut satırı bağımsız değişkeni, değer > biçiminde çiftleri
* `compute_target`: Eğitim betiğinizi, bu durumda çalışır uzak işlem bir [Batch AI](how-to-set-up-training-targets.md#batch) küme
* `entry_script`: Dosya yolunu (göreli `source_directory`) eğitim betiğin uzak işlem üzerinde çalıştırılacak. Bu dosya ve, bağımlı herhangi bir ek dosyaları bu klasörde bulunmalıdır
* `conda_packages`: Python paketlerini eğitim betiğinizi gerekli conda aracılığıyla yüklenecek listesi.
Oluşturucu adlı başka bir parametreye sahip `pip_packages` gereken herhangi bir pip paketleri için kullanabileceğiniz
* `use_gpu`: Bu bayrağı ayarlanmış `True` eğitim GPU yararlanmak için. Varsayılan olarak `False`

Eğitim için kullanılan kapsayıcı PyTorch estimator kullandığından, varsayılan PyTorch paket ve CPU ve gpu üzerinde eğitim için gereken ilgili bağımlılıklar içerir.

Ardından, PyTorch işi gönder:
```Python
run = exp.submit(pt_est)
```

## <a name="distributed-training"></a>Dağıtılmış eğitimi
PyTorch Estimator Azure VM'lerin CPU ve GPU kümeleri arasında uygun ölçekte Modellerinizi eğitmek sağlar. Azure Machine Learning tüm altyapı ve bu iş yüklerinin ölçeğini gerçekleştirmek için gereken düzenleme arka planda yönetecek karşın dağıtılmış PyTorch eğitimi birkaç API çağrısı ile kolayca çalıştırabilirsiniz.

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

Yukarıdaki kod, aşağıdaki yeni parametreleri PyTorch oluşturucuya kullanıma sunar:
* `node_count`:, Eğitim işine yönelik kullanmak için düğüm sayısı. Varsayılan olarak bu bağımsız değişken `1`
* `process_count_per_node`: Her bir düğümde çalıştırılacak işlemleri (veya "çalışanları") sayı. Varsayılan olarak bu bağımsız değişken `1`
* `distributed_backend`: Başlatmak için arka uç MPI Estimator sunan eğitim dağıtılmış. Bu bağımsız değişken varsayılan olarak `None`. Paralel veya dağıtılmış eğitimini gerçekleştirmek istiyorsanız (örneğin `node_count`> 1 veya `process_count_per_node`> 1 veya her ikisi) MPI (ve Horovod) sahip, `distributed_backend='mpi'`. Azure Machine Learning tarafından kullanılan MPI uygulamasıdır [açık MPI](https://www.open-mpi.org/).

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
Tek düğümlü PyTorch eğitim hakkında bir öğretici için bkz:
* `training/01.train-tune-deploy-pytorch/01.train-tune-deploy-pytorch.ipynb`

Dağıtılmış PyTorch Horovod ile temel bir öğretici için bkz:
* `training/02.distributed-pytorch-with-horovod/02.distributed-pytorch-with-horovod.ipynb`

Bu not defterlerini alın:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar
* [İzleme ölçümlerini eğitim sırasında çalıştırın](how-to-track-experiments.md)
* [Hiperparametreleri ayarlama](how-to-tune-hyperparameters.md)
* [Eğitilen model dağıtma](how-to-deploy-and-where.md)
