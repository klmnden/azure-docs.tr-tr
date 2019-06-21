---
title: PyTorch modelleri eğitme ve kaydetme
titleSuffix: Azure Machine Learning service
description: Bu makalede eğitme ve Azure Machine Learning hizmetini kullanarak bir PyTorch modeli kaydetmeyi gösterilmektedir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: peterlu
ms.date: 06/18/2019
ms.custom: seodec18
ms.openlocfilehash: fc80fcde8de3fb2d6dd6f59804f6019b76aa8727
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295588"
---
# <a name="train-and-register-pytorch-models-at-scale-with-azure-machine-learning-service"></a>Eğitim ve uygun ölçekte PyTorch modeller Azure Machine Learning hizmeti ile kaydetme

Bu makalede eğitme ve Azure Machine Learning hizmetini kullanarak bir PyTorch modeli kaydetmeyi gösterilmektedir. Dayanır [öğretici PyTorch'ın aktarımlı](https://pytorch.org/tutorials/beginner/transfer_learning_tutorial.html) , derin sinir ağı (DNN) sınıflandırıcı karıncaların ve arı görüntüleri oluşturur.

[PyTorch](https://pytorch.org/) , derin sinir ağı (DNN) oluşturmak için yaygın olarak kullanılan bir açık kaynaklı bilgi işlem altyapısıdır. Azure Machine Learning hizmeti ile açık kaynaklı eğitim işleri elastik bulut bilgi işlem kaynaklarını kullanarak hızlı bir şekilde ölçeklendirebilirsiniz. Ayrıca izleyebilirsiniz eğitim çalıştırmaları, sürüm modelleri, modelleri ve daha fazlasını dağıtın.

Azure Machine Learning hizmeti, sıfırdan PyTorch modelden geliştiriyor ister mevcut bir model buluta getirdiğiniz, üretime hazır modelleri oluşturmanıza yardımcı olabilir.

## <a name="prerequisites"></a>Önkoşullar

Bu kod, bu ortamların birini çalıştırın:

 - Azure Machine Learning not defteri VM - herhangi bir indirme veya yükleme gerekli

    - Tamamlamak [bulut tabanlı bir not defteri hızlı](quickstart-run-cloud-notebook.md) adanmış notebook sunucusu önceden yüklenmiş SDK'sı ve örnek depoyu oluşturmak için.
    - Bu dizine giderek notebook sunucusu örnekleri klasöründe, tamamlanan ve genişletilmiş bir not defteri Bul: **Yardım-How-to-kullanın-azureml > Eğitim ile derin öğrenme > train-hyperparameter-tune-deploy-with-pytorch** klasör. 
 
 - Kendi Jupyter Notebook sunucusu

    - [Azure Machine için Python SDK'sı Learning yükleme](setup-create-workspace.md#sdk)
    - [Çalışma alanı yapılandırma dosyası oluşturma](setup-create-workspace.md#write-a-configuration-file)
    - [Örnek komut dosyalarını indirme](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch) `pytorch_train.py`
     
    Ayrıca bir tamamlanmış bulabilirsiniz [Jupyter Not Defteri sürüm](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch/train-hyperparameter-tune-deploy-with-pytorch.ipynb) GitHub örnekleri sayfasında bu kılavuzun. Not Defteri, akıllı hiper parametre ayarı, model dağıtımı ve not defteri pencere öğeleri kapsayan genişletilmiş bölümler içerir.

## <a name="set-up-the-experiment"></a>Deneme ayarlama

Bu bölümde, gerekli python paketlerini yükleme, bir çalışma alanı başlatma, deneme oluşturma ve eğitim verilerini ve eğitim betikleriniz karşıya eğitim denemesini ayarlar.

### <a name="import-packages"></a>Paketleri içeri aktarma

İlk olarak, gerekli Python kitaplıkları içeri aktarın.

```Python
import os
import shutil

from azureml.core.workspace import Workspace
from azureml.core import Experiment

from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException
from azureml.train.dnn import PyTorch
```

### <a name="initialize-a-workspace"></a>Bir çalışma alanını Başlat

[Azure Machine Learning hizmeti çalışma alanında](concept-workspace.md) hizmeti için en üst düzey bir kaynaktır. Oluşturduğunuz tüm yapıları ile çalışma için merkezi bir yerde sağlar. Python SDK'da çalışma yapıları oluşturarak erişebileceğiniz bir [ `workspace` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) nesne.

Bir çalışma alanı nesne oluşturmak `config.json` 'te dosya oluşturulduğunda [Önkoşullar bölümüne](#prerequisites).

```Python
ws = Workspace.from_config()
```

### <a name="create-an-experiment"></a>Deneme oluşturma

Bir deney ve eğitim betiklerinizi tutmak için bir klasör oluşturun. Bu örnekte, "hymenoptera pytorch" adlı bir deneme oluşturun.

```Python
project_folder = './pytorch-hymenoptera'
os.makedirs(project_folder, exist_ok=True)

experiment_name = 'pytorch-hymenoptera'
experiment = Experiment(ws, name=experiment_name)
```

### <a name="get-the-data"></a>Verileri alma

Veri kümesi, yaklaşık 120 eğitim görüntülerin her karıncaların ve arı, her sınıf için 75 doğrulama görüntülerle oluşur. Hymenoptera karıncaların ve arı içeren böcekler sırasıdır. İndirin ve eğitim betiğimizi bir parçası olarak dataset ayıklayın `pytorch_train.py`.

### <a name="prepare-training-scripts"></a>Eğitim betikleriniz hazırlama

Bu öğreticide, bir eğitim betiğini `pytorch_train.py`, zaten sağlanır. Uygulamada olduğu gibi tüm özel eğitim betiği alıp ve Azure Machine Learning hizmeti ile çalıştırın.

Pytorch eğitim betiğini yükleme `pytorch_train.py`.

```Python
shutil.copy('pytorch_train.py', project_folder)
```

Ancak, Azure Machine Learning hizmeti izleme ve ölçümler özellikleri kullanmak istiyorsanız, bir eğitim betiğinizi içinde küçük miktar kod eklemeniz gerekir. İzleme ölçümlerini örnekleri bulunabilir `pytorch_train.py`.

## <a name="create-a-compute-target"></a>İşlem hedefi oluşturmak

PyTorch işinizi çalıştırmak işlem hedefi oluşturmak. Bu örnekte, bir GPU özellikli Azure Machine Learning işlem kümesini oluşturun.

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

## <a name="create-a-pytorch-estimator"></a>PyTorch tahmin oluşturma

[PyTorch estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py) işlem hedefi üzerinde bir PyTorch eğitim işini başlatmak için basit bir yol sağlar.

PyTorch estimator genel uygulanır [ `estimator` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) herhangi bir çerçeveyi desteklemek için kullanılan sınıf. Genel tahmin Aracı'nı kullanarak modellerin eğitimi hakkında daha fazla bilgi için bkz. [tahmin Aracı'nı kullanarak Azure Machine Learning modellerini eğitin](how-to-train-ml-models.md)

Eğitim betiğinizi ek pip ya da conda paketleri çalıştırmak için gerekiyorsa, paketlerin adlarını aracılığıyla geçirerek elde edilen bir docker görüntüsü üzerinde yüklü olabilir `pip_packages` ve `conda_packages` bağımsız değişkenler.

```Python
script_params = {
    '--num_epochs': 30,
    '--output_dir': './outputs'
}

estimator = PyTorch(source_directory=project_folder, 
                    script_params=script_params,
                    compute_target=compute_target,
                    entry_script='pytorch_train.py',
                    use_gpu=True,
                    pip_packages=['pillow==5.4.1'])
```

## <a name="submit-a-run"></a>Bir farklı çalıştır gönderin

[Nesnesini çalıştırmak](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) işi devam ederken ve tamamlandıktan sonra çalıştırma geçmişini arabirim sağlar.

```Python
run = experiment.submit(estimator)
run.wait_for_completion(show_output=True)
```

Çalıştırma yürütülür gibi aşağıdaki aşamalarının geçer:

- **Hazırlama**: Bir docker görüntüsü PyTorch estimator göre oluşturulur. Görüntü çalışma alanınızın container registry'ye yüklendi ve sonraki çalıştırmalar için önbelleğe alınır. Günlükler için çalıştırma geçmişi de aktarılır ve ilerleme durumunu izlemek için görüntülenebilir.

- **Ölçeklendirme**: Daha fazla düğüm çalıştırma şu anda mevcut olandan yürütmek için Batch AI kümesi gerektiriyorsa, Ölçek kümesi çalışır.

- **Çalışan**: Betik klasöründeki tüm betikler işlem hedefine yüklenen, veri depoları bağlı veya kopyalanır ve entry_script yürütülür. Stdout çıkışları ve. / Günlükler klasöründe için çalıştırma geçmişi aktarılır ve çalıştırmasını izlemek için kullanılabilir.

- **İşleme sonrası**: . / Çalışma klasörüne kopyalanır üzerinden için çalıştırma geçmişi çıkarır.

## <a name="register-or-download-a-model"></a>Kaydolun veya bir model indirin

Modeli eğittiğimize sonra çalışma alanınıza kaydedebilirsiniz. Model kaydı sağlar, depolama ve sürüm Modellerinizi basitleştirmek için çalışma alanınızdaki [model yönetimi ve dağıtım](concept-model-management-and-deployment.md).

```Python
model = run.register_model(model_name='pt-dnn', model_path='outputs/')
```

Ayrıca, çalışma nesnesini kullanarak model yerel bir kopyasını indirebilirsiniz. Eğitim betiğinde `pytorch_train.py`, bir PyTorch kaydetme nesne modeli (işlem hedefine yerel) yerel bir klasöre devam ederse. Çalışma nesnesinin bir kopyasını indirmek için kullanabilirsiniz.

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

[ `PyTorch` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py) Estimator ayrıca CPU ve GPU kümeleri arasında dağıtılmış eğitimi destekler. Dağıtılmış PyTorch işleri kolayca çalıştırabilir ve düzenleme için Azure Machine Learning hizmeti yönetir.

### <a name="horovod"></a>Horovod
[Horovod](https://github.com/uber/horovod) bir açık kaynaklı, dağıtılmış eğitimi Uber tarafından geliştirilen için tüm framework azaltın. Dağıtılmış GPU PyTorch işleri kolay bir yolunu sunar.

Horovod kullanmak için belirtin bir [ `MpiConfiguration` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.mpiconfiguration?view=azure-ml-py) nesnesi `distributed_training` PyTorch oluşturucuda parametresi. Bu parametre, eğitim betiğinizde kullanabilmeniz için Horovod kitaplığı yüklendiğini sağlar.


```Python
from azureml.train.dnn import PyTorch

estimator= PyTorch(source_directory=project_folder,
                      compute_target=compute_target,
                      script_params=script_params,
                      entry_script='script.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_training=MpiConfiguration(),
                      framework_version='1.13',
                      use_gpu=True)
```
Eğitim betiğinizde alabilmeniz Horovod ve bağımlılıklarını sizin için yüklenecek `train.py` gibi:

```Python
import torch
import horovod
```
## <a name="export-to-onnx"></a>ONNX için dışarı aktarma

Çıkarım ile en iyi duruma getirme [ONNX çalışma zamanı](concept-onnx.md), eğitilen PyTorch modelinizi ONNX biçimine dönüştürün. Çıkarım veya Puanlama modeli, dağıtılan model için tahmin, üretim veri çubuğunda en yaygın olarak kullanıldığı aşamasıdır. Bkz: [öğretici](https://github.com/onnx/tutorials/blob/master/tutorials/PytorchOnnxExport.ipynb) örneği.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, eğitim ve Azure Machine Learning hizmetinde bir PyTorch modelin kayıtlı. Model dağıtımı makalemizi açın devam model dağıtma hakkında bilgi edinin.

> [!div class="nextstepaction"]
> [Nasıl ve nerede modelleri dağıtma](how-to-deploy-and-where.md)
* [İzleme ölçümlerini eğitim sırasında çalıştırın](how-to-track-experiments.md)
* [Hiperparametreleri ayarlama](how-to-tune-hyperparameters.md)
* [Eğitilen model dağıtma](how-to-deploy-and-where.md)
