---
title: ML modelleri ile estimators eğitin
titleSuffix: Azure Machine Learning service
description: Tek düğümlü ve dağıtılmış eğitim, öğrenme ve derin öğrenme modellerini Azure Machine Learning Hizmetleri Estimator sınıfını kullanarak geleneksel makinenin yapmayı öğrenin
ms.author: minxia
author: mx-iao
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.date: 04/19/2019
ms.custom: seodec18
ms.openlocfilehash: 98f7dc2e295c0c994db9a0189814b0ef2a19b758
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66153608"
---
# <a name="train-models-with-azure-machine-learning-using-estimator"></a>Azure Machine Learning kullanarak tahmin modellerini eğitin

Azure Machine Learning ile kolayca eğitim betiğinizi gönderebilirsiniz [çeşitli hedefleri işlem](how-to-set-up-training-targets.md#compute-targets-for-training)kullanarak [RunConfiguration nesne](how-to-set-up-training-targets.md#whats-a-run-configuration) ve [ScriptRunConfig nesne](how-to-set-up-training-targets.md#submit). Bu desen çok fazla esneklik ve en fazla denetim sağlar.

Kolaylaştırmak için derin öğrenme modeli Eğitimi, Azure Machine Learning Python SDK'sı bir alternatif üst düzey Özet, kolayca yapısı çalıştırma yapılandırmaları kullanıcıların sağlar sınıfını tahmin sağlar. Oluşturun ve genel bir [Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py) seçtiğiniz herhangi bir öğrenme çerçeveyi kullanarak eğitim betiğini göndermek için (gibi scikit-bilgi edinin) yerel makinenize, azure'da tek bir VM veya bir GPU olup herhangi bir işlem hedef üzerinde çalıştırmak istediğiniz azure'da küme. PyTorch, TensorFlow ve bağlayıcı görevleri için Azure Machine Learning ayrıca ilgili sağlar [PyTorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py), [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) ve [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) estimators bu kullanmayı kolaylaştırmak için çerçeveler.

## <a name="train-with-an-estimator"></a>Bir tahmin ile eğitme

Oluşturduktan sonra [çalışma](concept-workspace.md) ve ayarlayın, [geliştirme ortamı](how-to-configure-environment.md), Azure Machine Learning modeli, aşağıdaki adımları içerir:  
1. Oluşturma bir [uzak işlem hedef](how-to-set-up-training-targets.md) (Ayrıca kullanabileceğiniz yerel bilgisayar işlem hedefi olarak not)
2. Karşıya yükleme, [eğitim verilerini](how-to-access-data.md) için veri deposu (isteğe bağlı)
3. Oluşturma, [eğitim betiği](tutorial-train-models-with-aml.md#create-a-training-script)
4. Oluşturma bir `Estimator` nesnesi
5. Çalışma alanı altında deneme nesneye estimator gönderin

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
`script_params`| Eğitim betiğinizi komut satırı bağımsız değişkenleri belirtme sözlük `entry_script`, < komut satırı bağımsız değişkeni, değer > biçiminde çiftleri. Ayrıntılı bir bayrak belirlemek için `script_params`, kullanın `<command-line argument, "">`.
`compute_target`| Eğitim betiğinizi, bu örnekte, bir Azure Machine Learning işlem çalıştıracak uzak işlem hedefine ([AmlCompute](how-to-set-up-training-targets.md#amlcompute)) kümesi. (Lütfen unutmayın AmlCompute küme yaygın olarak kullanılan hedef olsa da hedef türleri Azure Vm'leri ya da yerel bilgisayar gibi diğer işlem seçmek mümkündür.)
`entry_script`| FilePath (göreli `source_directory`) eğitim betiğin uzak işlem üzerinde çalıştırılacak. Bu dosya ve, bağımlı herhangi bir ek dosyaları bu klasörde bulunmalıdır
`conda_packages`| Eğitim betiğinizi gerekli conda aracılığıyla yüklenecek Python paketleri listesi.  

Oluşturucu adlı başka bir parametreye sahip `pip_packages` , gerekli herhangi bir pip paketleri kullanın

Oluşturduğunuza göre `Estimator` nesne, bir çağrı ile uzak işlem üzerinde çalıştırılacak eğitim işini gönderme `submit` işlevini, [deneme](concept-azure-machine-learning-architecture.md#experiment) nesne `experiment`. 

```Python
run = experiment.submit(sk_est)
print(run.get_portal_url())
```

> [!IMPORTANT]
> **Özel klasörler** iki klasör *çıkarır* ve *günlükleri*, Azure Machine Learning tarafından özel olarak değerlendirilmesi alırsınız. Klasörleri için dosyaları yazdığınızda eğitim sırasında *çıkarır* ve *günlükleri* kök dizine göre olan (`./outputs` ve `./logs`sırasıyla), dosyaları otomatik olarak ayarlanır çalıştırmanız tamamlandıktan sonra onlara yönelik erişimi olması için çalıştırma geçmişi karşıya yükleyin.
>
> Oluşturmak için (örneğin, model dosyaları, kontrol noktaları, veri dosyalarını veya çizilen görüntüsü) eğitim sırasında yapıtları için bu yazma `./outputs` klasör.
>
> Eğitim için Çalıştır'nden herhangi bir günlük benzer şekilde, yazabileceğiniz `./logs` klasör. Azure Machine Learning'ın kullanmaya [TensorBoard tümleştirme](https://aka.ms/aml-notebook-tb) bu klasöre TensorBoard günlüklerinizi aldığınızdan emin olun. Çalıştırma sürerken TensorBoard başlatın ve bu günlüklerin akışını mümkün olacaktır.  Daha sonra ayrıca günlükleri, önceki çalıştırmaları birini geri yüklenmesi mümkün olacaktır.
>
> Örneğin, yazılan bir dosyayı indirmek için *çıkarır* klasör yerel makinenize sonra uzak eğitim çalıştırın: `run.download_file(name='outputs/my_output_file', output_file_path='my_destination_path')`

### <a name="distributed-training-and-custom-docker-images"></a>Dağıtılmış eğitimi ve özel Docker görüntüleri

Sizin gerçekleştirdiğiniz ile iki ek eğitim senaryosu vardır `Estimator`:
* Özel Docker görüntüsü kullanma
* Çok düğümlü bir küme üzerinde dağıtılmış eğitimi

Aşağıdaki kod, Keras modeli için Dağıtılmış eğitimi yürütmek gösterilmektedir. Ayrıca, varsayılan Azure Machine Learning görüntüleri kullanmak yerine, bir özel docker görüntüsünü Docker hub'dan belirtir `continuumio/miniconda` eğitim.

Oluşturmuş olmanız, [hedef işlem](how-to-set-up-training-targets.md#amlcompute) nesne `compute_target`. Tahmin aracı gibi oluşturun:

```Python
from azureml.train.estimator import Estimator

estimator = Estimator(source_directory='./my-keras-proj',
                      compute_target=compute_target,
                      entry_script='train.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_backend='mpi',     
                      conda_packages=['tensorflow', 'keras'],
                      custom_docker_base_image='continuumio/miniconda')
```

Yukarıdaki kod için aşağıdaki yeni parametreleri sunan `Estimator` Oluşturucusu:

Parametre | Açıklama | Varsayılan
--|--|--
`custom_docker_base_image`| Kullanmak istediğiniz resmin adı. Yalnızca genel docker depolardaki (Bu durum Docker hub'daki) görüntüleri sağlar. Özel docker deposundan görüntü kullanmak için oluşturucunun kullanın `environment_definition` parametresi yerine. [Örnek](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/how-to-use-estimator/how-to-use-estimator.ipynb). | `None`
`node_count`| Eğitim işine yönelik kullanmak için düğüm sayısı. | `1`
`process_count_per_node`| Her bir düğümde çalıştırılacak işlemleri (veya "çalışanları") sayısı. Bu durumda, kullandığınız `2` GPU'ları her düğümde kullanılabilir.| `1`
`distributed_backend`| Başlatmak için arka uç MPI Estimator sunan eğitim dağıtılmış.  Paralel veya dağıtılmış eğitimini yürütmek için (örneğin `node_count`> 1 veya `process_count_per_node`> 1 veya her ikisi) ayarlayın `distributed_backend='mpi'`. AML tarafından kullanılan MPI uygulamasıdır [açık MPI](https://www.open-mpi.org/).| `None`

Son olarak, eğitim işini gönder:
```Python
run = experiment.submit(estimator)
print(run.get_portal_url())
```

## <a name="github-tracking-and-integration"></a>GitHub izleme ve tümleştirme

Kaynak dizini yerel bir Git deposu olduğu çalıştırma eğitim başlattığınızda, depo bilgilerini çalıştırma geçmişinde depolanır. Örneğin, geçerli işleme kimliği depo için geçmiş bir parçası olarak günlüğe kaydedilir.

## <a name="examples"></a>Örnekler
Tahmin deseni temelleri gösteren bir not defteri için bkz:
* [How-to-use-azureml/Training-With-DEEP-Learning/How-to-use-estimator](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/how-to-use-estimator/how-to-use-estimator.ipynb)

Bir scikit eğitir bir not defteri için-bilgi modeli tahmin Aracı'nı kullanarak, bkz:
* [öğreticiler/img-sınıflandırma-bölüm 1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb)

Derin öğrenme framework belirli estimators kullanarak modellerin eğitimi dizüstü bilgisayarlar için bkz:
* [How-to-use-azureml/Training-With-DEEP-Learning](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [İzleme ölçümlerini eğitim sırasında çalıştırın](how-to-track-experiments.md)
* [PyTorch modellerini eğitin](how-to-train-pytorch.md)
* [TensorFlow modellerini eğitin](how-to-train-tensorflow.md)
* [Hiperparametreleri ayarlama](how-to-tune-hyperparameters.md)
* [Eğitilen model dağıtma](how-to-deploy-and-where.md)
