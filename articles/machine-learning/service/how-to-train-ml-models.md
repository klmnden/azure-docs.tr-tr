---
title: Azure Machine Learning ile makine öğrenimi modellerini eğitin
description: Tek düğümlü ve dağıtılmış eğitim, öğrenme ve derin öğrenme modellerini Azure Machine Learning services ile geleneksel makinenin yapmayı öğrenin
ms.author: minxia
author: mx-iao
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: 50b808b5769f2160d765ecdb431d71856e651b33
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46983591"
---
# <a name="how-to-train-models-with-azure-machine-learning"></a>Azure Machine Learning modeli eğitme

Eğitim makine öğrenimi modellerini, özellikle derin sinir ağı, genellikle bir saat ve işlem yoğunluklu görevdir. Eğitim betiğinizi yazma ve yerel makinenizde verilerin küçük bir alt kümesi üzerinde çalışan bitirdiğinizde, büyük olasılıkla, iş yükü ölçeklemek istersiniz.

Eğitim kolaylaştırmak için üst düzey bir soyutlamadır, kullanıcıların Azure ekosistemindeki modellerini kolayca eğitin olanak sağlar sınıfını Estimator Azure Machine Learning Python SDK'sı sağlar. Oluşturun ve bağlı uzak işlem, çalıştırmak istediğiniz çalıştırma veya dağıtılmış bir tek düğümlü eğitim GPU küme genelinde olup herhangi bir eğitim kod göndermek için bir tahmin nesnesini kullanın. PyTorch ve TensorFlow işler için Azure Machine Learning Ayrıca kendi özel PyTorch sağlar ve bu çerçeveler kolaylaştırır TensorFlow Estimators kullanın.

## <a name="train-with-an-estimator"></a>Bir tahmin ile eğitme

Oluşturduktan sonra [çalışma](https://docs.microsoft.com/azure/machine-learning/service/concept-azure-machine-learning-architecture#workspace) ve ayarlayın, [geliştirme ortamı](how-to-configure-environment.md), Azure Machine Learning modeli, aşağıdaki adımları içerir:  
1. [Uzak işlem hedefi oluşturmak](how-to-set-up-training-targets.md)
2. [Eğitim verilerinizi karşıya](how-to-access-data.md) (isteğe bağlı)
3. Eğitim komut dosyanızı oluşturmaya
4. Tahmin nesne oluşturma
5. Eğitim iş gönderme

Bu makalede, adım 4-5 üzerinde odaklanır. 1-3. adımları için bkz. [öğretici](tutorial-train-models-with-aml.md) örneği.

### <a name="single-node-training"></a>Tek düğümlü eğitim

Aşağıdaki kod, bir scikit için uzak işlem azure'da çalışan bir tek düğümlü eğitim kılavuzluk-model öğrenin. Oluşturmuş olmanız, [hedef işlem](how-to-set-up-training-targets.md#batch) nesne `compute_target` ve [veri deposu](how-to-access-data.md) nesne `ds`.

```Python
from azureml.train.estimator import Estimator

script_params = {
    '--data-folder': ds.as_mount(),
    '--regularization': 0.8
}

sk_est = Estimator(source_directory='./my-sklearn-proj',
                   script_params=script_params,
                   compute_target=compute_target,
                   entry_script='train.py',
                   conda_packages=['scikit-learn'])
```

Yukarıdaki kod parçacığında tahmin Oluşturucu aşağıdaki parametreleri belirtir:
* `source_directory`: Tüm eğitim işine yönelik gerekli kodunuzu içeren yerel bir dizin. Bu klasörü yerel makinenizden uzak bilgisayarda kopyalanır 
* `script_params`: Eğitim betiğinizi komut satırı bağımsız değişkenleri belirtme bir sözlük `entry_script`, < komut satırı bağımsız değişkeni, değer > biçiminde çiftleri
* `compute_target`: Eğitim betiğinizi, bu durumda çalışır uzak işlem bir [Batch AI](how-to-set-up-training-targets.md#batch) küme
* `entry_script`: Dosya yolunu (göreli `source_directory`) eğitim betiğin uzak işlem üzerinde çalıştırılacak. Bu dosya ve, bağımlı herhangi bir ek dosyaları bu klasörde bulunmalıdır
* `conda_packages`: Python paketlerini eğitim betiğinizi gerekli conda aracılığıyla yüklenecek listesi.  
Oluşturucu adlı başka bir parametreye sahip `pip_packages` gereken herhangi bir pip paketleri için kullanabileceğiniz

Tahmin nesnenizin oluşturduğunuza göre bir çağrı aracılığıyla uzak bir işlem üzerinde çalıştırılacak eğitim işini gönderebilirsiniz `submit` işlevini, [deneme](concept-azure-machine-learning-architecture.md#experiment) nesne `experiment`. 

```Python
run = experiment.submit(sk_est)
print(run.get_details().status)
```

> [!IMPORTANT]
> **Özel klasörler** iki klasör *çıkarır* ve *günlükleri*, Azure Machine Learning tarafından özel olarak değerlendirilmesi alırsınız. Adlı bir klasöre dosyalar yazarsanız, eğitim sırasında *çıkarır* ve *günlükleri* kök dizine göre olan (`./outputs` ve `./logs`sırasıyla), bu dosyaları alırsınız çalıştırmanız tamamlandıktan sonra onlara yönelik erişimi gerekir. böylece, çalıştırma geçmişi için otomatik olarak karşıya yüklendi. 
>
> Erişmek için (örneğin, model dosyaları, kontrol noktaları, veri dosyalarını veya çizilen görüntüsü) eğitim sırasında oluşturulan yapılar için bu yazma `./outputs` klasör.
>
> Eğitim için Çalıştır'nden herhangi bir günlük benzer şekilde, yazabileceğiniz `./logs` klasör. Azure Machine Learning'ın kullanmaya [TensorBoard tümleştirme](https://aka.ms/aml-notebook-tb) bu klasöre TensorBoard günlüklerinizi aldığınızdan emin olun. Çalıştırma sürerken TensorBoard başlatın ve bu günlüklerin akışını mümkün olacaktır.  Daha sonra ayrıca günlükleri, önceki çalıştırmaları birini geri yüklenmesi mümkün olacaktır.
>
> Örneğin, yazılan bir dosyayı indirmek için *çıkarır* klasör yerel makinenize sonra uzak eğitim çalıştırın: `run.download_file(name='outputs/my_output_file', output_file_path='my_destination_path')`

### <a name="distributed-training-and-custom-docker-images"></a>Dağıtılmış eğitimi ve özel Docker görüntüleri

Tahmin ile çalıştırabileceğiniz iki ek eğitim senaryosu vardır:
1. Özel Docker görüntüsü kullanma
2. Çok düğümlü bir küme üzerinde dağıtılmış eğitimi

Aşağıdaki kod, dağıtılmış bir CNTK modeli eğitimi yürütmek gösterilmektedir. Ayrıca, varsayılan Azure Machine Learning görüntüleri kullanmak yerine, kendi özel docker görüntüsü eğitim için kullanmak istediğiniz olduğunu varsayar.

Oluşturmuş olmanız, [hedef işlem](how-to-set-up-training-targets.md#batch) nesne `compute_target`. Tahmin aracı gibi oluşturabilirsiniz:

```Python
from azureml.train.estimator import Estimator

estimator = Estimator(source_directory='./my-cntk-proj',
                      compute_target=compute_target,
                      entry_script='train.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_backend='mpi',     
                      pip_packages=['cntk==2.5.1'],
                      custom_docker_base_image='microsoft/mmlspark:0.12')
```

Yukarıdaki kod, aşağıdaki yeni parametreleri Estimator oluşturucuya kullanıma sunar:
* `custom_docker_base_image`: Kullanmak istediğiniz görüntünün adı. (Bu durum Docker hub'daki) genel docker depoları, görüntüleri yalnızca sağlayabilir. Özel docker deposundan görüntü kullanmak için oluşturucunun kullanın `environment_definition` parametresi yerine
* `node_count`:, Eğitim işine yönelik kullanmak için düğüm sayısı. Varsayılan olarak bu bağımsız değişken `1`
* `process_count_per_node`: Her bir düğümde çalıştırılacak işlemleri (veya "çalışanları") sayı. Bu durumda, kullandığınız `2` GPU'ları her düğümde kullanılabilir. Varsayılan olarak bu bağımsız değişken `1`
* `distributed_backend`: Başlatmak için arka uç MPI Estimator sunan eğitim dağıtılmış. Bu bağımsız değişken varsayılan olarak `None`. Paralel veya dağıtılmış eğitimini gerçekleştirmek istiyorsanız (örneğin `node_count`> 1 veya `process_count_per_node`> 1 veya her ikisi) ayarlayın `distributed_backend='mpi'`. AML tarafından kullanılan MPI uygulamasıdır [açık MPI](https://www.open-mpi.org/).

Son olarak, eğitim işini gönder:
```Python
run = experiment.submit(cntk_est)
```

## <a name="examples"></a>Örnekler
Sklearn modeli ilişkin bir öğretici için bkz:
* `tutorials/01.train-models.ipynb`

Özel docker kullanarak dağıtılmış CNTK hakkında bir öğretici için bkz:
* `training/06.distributed-cntk-with-custom-docker/06.distributed-cntk-with-custom-docker.ipynb`

Bu not defterlerini alın:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [İzleme ölçümlerini eğitim sırasında çalıştırın](how-to-track-experiments.md)
* [PyTorch modellerini eğitin](how-to-train-pytorch.md)
* [TensorFlow modellerini eğitin](how-to-train-tensorflow.md)
* [Hiperparametreleri ayarlama](how-to-tune-hyperparameters.md)
* [Eğitilen model dağıtma](how-to-deploy-and-where.md)
