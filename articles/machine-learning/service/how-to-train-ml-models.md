---
title: Tahmin sınıfı kullanarak ML modelleri eğitme
titleSuffix: Azure Machine Learning service
description: Tek düğümlü ve dağıtılmış eğitim, öğrenme ve derin öğrenme modellerini Azure Machine Learning Hizmetleri Estimator sınıfını kullanarak geleneksel makinenin yapmayı öğrenin
ms.author: minxia
author: mx-iao
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: e7157b9546d1f9ca40bab35d9e643c38051db04e
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53100761"
---
# <a name="train-models-with-azure-machine-learning"></a>Azure Machine Learning ile eğitme modelleri

Eğitim makine öğrenimi modellerini, özellikle derin sinir ağı, genellikle bir saat ve işlem yoğunluklu görevdir. Eğitim betiğinizi yazma ve yerel makinenizde verilerin küçük bir alt kümesi üzerinde çalışan bitirdiğinizde, büyük olasılıkla, iş yükü ölçeklemek istersiniz.

Eğitim kolaylaştırmak için üst düzey bir soyutlamadır, kullanıcıların Azure ekosistemindeki modellerini kolayca eğitin olanak sağlar sınıfını estimator Azure Machine Learning Python SDK'sı sağlar. Oluşturma ve kullanma bir `Estimator` bağlı uzak işlem, çalıştırmak istediğiniz çalıştırma veya dağıtılmış bir tek düğümlü eğitim GPU küme genelinde olup herhangi bir eğitim kod göndermek için nesne. PyTorch ve TensorFlow işler için Azure Machine Learning Ayrıca kendi özel sağlar `PyTorch` ve `TensorFlow` estimators bu çerçeveler kullanmayı kolaylaştırmak için.

## <a name="train-with-an-estimator"></a>Bir tahmin ile eğitme

Oluşturduktan sonra [çalışma](concept-azure-machine-learning-architecture.md#workspace) ve ayarlayın, [geliştirme ortamı](how-to-configure-environment.md), Azure Machine Learning modeli, aşağıdaki adımları içerir:  
1. Oluşturma bir [uzak işlem hedefi](how-to-set-up-training-targets.md)
2. Karşıya yükleme, [eğitim verilerini](how-to-access-data.md) (isteğe bağlı)
3. Oluşturma, [eğitim betiği](tutorial-train-models-with-aml.md#create-a-training-script)
4. Oluşturma bir `Estimator` nesnesi
5. Eğitim iş gönderme

Bu makalede, adım 4-5 üzerinde odaklanır. Adım 1-3 başvurmak için [model Öğreticisi eğitme](tutorial-train-models-with-aml.md) örneği.

### <a name="single-node-training"></a>Tek düğümlü eğitim

Kullanım bir `Estimator` bir scikit için uzak işlem azure'da çalışan bir tek düğümlü eğitim-model öğrenin. Oluşturmuş olmanız, [hedef işlem](how-to-set-up-training-targets.md#amlcompute) nesne `compute_target` ve [veri deposu](how-to-access-data.md) nesne `ds`.

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

Bu kod parçacığı için aşağıdaki parametreleri belirtir `Estimator` Oluşturucusu.

Parametre | Açıklama
--|--
`source_directory`| Eğitim işine yönelik gerekli kodunuzun tamamını içeren yerel dizin. Bu klasörü yerel makinenizden uzak bilgisayarda kopyalanır 
`script_params`| Eğitim betiğinizi komut satırı bağımsız değişkenleri belirtme sözlük `entry_script`, < komut satırı bağımsız değişkeni, değer > biçiminde çiftleri
`compute_target`| Eğitim betiğinizi, bu örnekte, bir Azure Machine Learning işlem çalıştıracak uzak işlem hedefine ([AmlCompute](how-to-set-up-training-targets.md#amlcompute)) kümesi
`entry_script`| FilePath (göreli `source_directory`) eğitim betiğin uzak işlem üzerinde çalıştırılacak. Bu dosya ve, bağımlı herhangi bir ek dosyaları bu klasörde bulunmalıdır
`conda_packages`| Eğitim betiğinizi gerekli conda aracılığıyla yüklenecek Python paketleri listesi.  
Oluşturucu adlı başka bir parametreye sahip `pip_packages` , gerekli herhangi bir pip paketleri kullanın

Oluşturduğunuza göre `Estimator` nesne, bir çağrı ile uzak işlem üzerinde çalıştırılacak eğitim işini gönderme `submit` işlevini, [deneme](concept-azure-machine-learning-architecture.md#experiment) nesne `experiment`. 

```Python
run = experiment.submit(sk_est)
print(run.get_details().status)
```

> [!IMPORTANT]
> **Özel klasörler** iki klasör *çıkarır* ve *günlükleri*, Azure Machine Learning tarafından özel olarak değerlendirilmesi alırsınız. Klasörleri için dosyaları yazdığınızda eğitim sırasında *çıkarır* ve *günlükleri* kök dizine göre olan (`./outputs` ve `./logs`sırasıyla), dosyaları otomatik olarak ayarlanır çalıştırmanız tamamlandıktan sonra onlara yönelik erişimi olması için çalıştırma geçmişi karşıya yükleyin.
>
> Oluşturmak için (örneğin, model dosyaları, kontrol noktaları, veri dosyalarını veya çizilen görüntüsü) eğitim sırasında yapıtları için bu yazma `./outputs` klasör.
>
> Herhangi bir günlük için çalıştırın eğitim gelen yazma benzer şekilde, `./logs` klasör. Azure Machine Learning'ın kullanmaya [TensorBoard tümleştirme](https://aka.ms/aml-notebook-tb) bu klasöre TensorBoard günlüklerinizi aldığınızdan emin olun. Çalıştırma sürerken TensorBoard başlatın ve bu günlüklerin akışını mümkün olacaktır.  Daha sonra ayrıca günlükleri, önceki çalıştırmaları birini geri yüklenmesi mümkün olacaktır.
>
> Örneğin, yazılan bir dosyayı indirmek için *çıkarır* klasör yerel makinenize sonra uzak eğitim çalıştırın: `run.download_file(name='outputs/my_output_file', output_file_path='my_destination_path')`

### <a name="distributed-training-and-custom-docker-images"></a>Dağıtılmış eğitimi ve özel Docker görüntüleri

Sizin gerçekleştirdiğiniz ile iki ek eğitim senaryosu vardır `Estimator`:
* Özel Docker görüntüsü kullanma
* Çok düğümlü bir küme üzerinde dağıtılmış eğitimi

Aşağıdaki kod, dağıtılmış bir CNTK modeli eğitimi yürütmek gösterilmektedir. Ayrıca, varsayılan Azure Machine Learning görüntüleri kullanmak yerine, bu, kendi özel docker görüntüsü için eğitim kullandığınızı varsayar.

Oluşturmuş olmanız, [hedef işlem](how-to-set-up-training-targets.md#amlcompute) nesne `compute_target`. Tahmin aracı gibi oluşturun:

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

Yukarıdaki kod için aşağıdaki yeni parametreleri sunan `Estimator` Oluşturucusu:

Parametre | Açıklama | Varsayılan
--|--|--
`custom_docker_base_image`| Kullanmak istediğiniz resmin adı. Yalnızca genel docker depolardaki (Bu durum Docker hub'daki) görüntüleri sağlar. Özel docker deposundan görüntü kullanmak için oluşturucunun kullanın `environment_definition` parametresi yerine | `None`
`node_count`| Eğitim işine yönelik kullanmak için düğüm sayısı. | `1`
`process_count_per_node`| Her bir düğümde çalıştırılacak işlemleri (veya "çalışanları") sayısı. Bu durumda, kullandığınız `2` GPU'ları her düğümde kullanılabilir.| `1`
`distributed_backend`| Başlatmak için arka uç MPI Estimator sunan eğitim dağıtılmış.  Paralel veya dağıtılmış eğitimini yürütmek için (örneğin `node_count`> 1 veya `process_count_per_node`> 1 veya her ikisi) ayarlayın `distributed_backend='mpi'`. AML tarafından kullanılan MPI uygulamasıdır [açık MPI](https://www.open-mpi.org/).| `None`

Son olarak, eğitim işini gönder:
```Python
run = experiment.submit(cntk_est)
```

## <a name="examples"></a>Örnekler
Sklearn modeli eğitir bir not defteri için bkz:
* [öğreticiler/img-sınıflandırma-bölüm 1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb)

Dağıtılmış derin öğrenme dizüstü bilgisayarlar için bkz:
* [How-to-use-azureml/Training-With-DEEP-Learning](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [İzleme ölçümlerini eğitim sırasında çalıştırın](how-to-track-experiments.md)
* [PyTorch modellerini eğitin](how-to-train-pytorch.md)
* [TensorFlow modellerini eğitin](how-to-train-tensorflow.md)
* [Hiperparametreleri ayarlama](how-to-tune-hyperparameters.md)
* [Eğitilen model dağıtma](how-to-deploy-and-where.md)
