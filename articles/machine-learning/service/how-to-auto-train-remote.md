---
title: Otomatik machine learning - Azure Machine Learning hizmeti için uzak işlem hedeflerini ayarlama
description: Bu makalede, Azure Machine Learning hizmeti ile veri bilimi sanal makinesi (DSVM) uzak işlem hedefi üzerinde otomatik makine öğrenimini kullanarak modelleri oluşturmak açıklanmaktadır
services: machine-learning
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 80a227b57c8df157890337f0e207519c71ae5bd6
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47034629"
---
# <a name="train-models-with-automated-machine-learning-in-the-cloud"></a>Bulutta otomatik machine learning ile modellerini eğitin

Azure Machine Learning'de modelinizi yönettiğiniz işlem kaynakları farklı türleri hakkında eğitebilirsiniz. İşlem hedefi, yerel bir bilgisayar veya bulutta bir bilgisayar olabilir. 

Kolayca artırın veya machine learning denemenizi Ubuntu tabanlı veri bilimi sanal makinesi (DSVM) veya Azure Batch AI gibi ek işlem hedefleri ekleyerek ölçeklendirin. Microsoft Azure bulutunda veri bilimi yapmak için özel olarak oluşturulmuş özel bir VM görüntüsü dsvm'dir. Bu, çok sayıda popüler veri bilimi ve önceden yüklenmiş ve önceden yapılandırılmış diğer araçları vardır.  

Bu makalede, ML otomatik DSVM'nin kullanarak model oluşturma öğrenin.  

## <a name="how-does-remote-differ-from-local"></a>Uzaktan yerel bilgisayardan farkı nedir?

Öğreticiyi "[otomatik ML ile bir sınıflandırma modeli eğitme](tutorial-auto-train-models.md)" yerel bilgisayarda otomatik ML ile modeli eğitmek için nasıl kullanılacağını size öğretir.  Yerel olarak da eğitimindeki iş akışı uzak hedefleri için de geçerlidir. Uzak işlem ile otomatik ML deneme yinelemelerini zaman uyumsuz olarak yürütülür. Bu belirli bir yinelemeye iptal sağlar, yürütme durumunu izleyin, Jupyter not defteri diğer hücreler üzerinde çalışmaya devam eder. Uzaktan eğitmek için ilk Azure DSVM gibi uzak işlem hedefi oluşturmak, yapılandırmak ve oradaki koda gönderin.

Bu makalede, bir uzak DSVM'nin otomatik ML çalıştırmak için gereken ek adımlar denemeniz gösterilmektedir.  Çalışma alanı nesnesi `ws`, öğreticinin buraya kod kullanılır.

```python
ws = Workspace.from_config()
```

## <a name="create-resource"></a>Kaynak Oluştur

Çalışma alanınızda DSVM oluşturma (`ws`) zaten yoksa. DSVM daha önce oluşturulmuş olsa bile, bu kod oluşturma işlemini atlar ve mevcut kaynak ayrıntıya yükler `dsvm_compute` nesne.  

**Tahmini Süre**: sanal makinenin oluşturulması yaklaşık 5 dakika sürer.

```python
from azureml.core.compute import DsvmCompute

dsvm_name = 'mydsvm' #Name your DSVM
try:
    dsvm_compute = DsvmCompute(ws, dsvm_name)
    print('found existing dsvm.')
except:
    print('creating new dsvm.')
    dsvm_config = DsvmCompute.provisioning_configuration(vm_size = "Standard_D2_v2")
    dsvm_compute = DsvmCompute.create(ws, name = dsvm_name, provisioning_configuration = dsvm_config)
    dsvm_compute.wait_for_completion(show_output = True)
```

Artık `dsvm_compute` uzak işlem hedefi olarak nesnesi.

DSVM adı kısıtlamaları şunlardır:
+ 64 karakterden kısa olmalıdır.  
+ Aşağıdaki karakterlerden herhangi birini içeremez: `\` ~! @ # $ % ^ & * () = + _ [] {} \\ \\ |;: \' \\", < > /?. `

>[!Warning]
>Market satın alma uygunluk hakkında bir ileti ile oluşturulması başarısız olursa:
>    1. [Azure portal](https://portal.azure.com)'a gidin
>    1. Bir DSVM oluşturmaya başlayın 
>    1. Seç "Program aracılığıyla oluşturmak istiyorsanız" programlı oluşturmayı etkinleştirmek için
>    1. VM oluşturmadan Çık
>    1. Yeniden oluşturma kodu


## <a name="access-data-using-get-data-file"></a>Veri dosyası kullanarak erişim veri alma

Eğitim verilerinizi uzak bir kaynağa erişim sağlar. Veriler uzak bir işlem üzerinde çalışan otomatik ML denemeleri için kullanarak getirilmesi gerekir bir `get_data()` işlevi.  

Erişim sağlamak için yapmanız gerekir:
+ Get_data.py içeren dosyayı oluşturma bir `get_data()` işlevi 
* Bu dosyayı betiklerinizi içeren klasörün kök dizinine yerleştirin 

Bir blob depolama veya yerel disk get_data.py dosyasındaki verileri okumak için kod yalıtabilirsiniz. Aşağıdaki kod örneğinde, veriler sklearn öğesini paketten gelir.

>[!Warning]
>Uzak işlem kullandığınız sonra kullanmalısınız `get_data()` veri Bağlantılarınızdaki gerçekleştirilecek.


```python
# Create a project_folder if it doesn't exist
if not os.path.exists(project_folder):
    os.makedirs(project_folder)

#Write the get_data file.
%%writefile $project_folder/get_data.py

from sklearn import datasets
from scipy import sparse
import numpy as np

def get_data():
    
    digits = datasets.load_digits()
    X_digits = digits.data[10:,:]
    y_digits = digits.target[10:]

    return { "X" : X_digits, "y" : y_digits }
```

## <a name="configure-automated-machine-learning-experiment"></a>Otomatik makine öğrenimi denemesi yapılandırma

Ayarlarını belirtin `AutoMLConfig`.  (Bkz: bir [parametrelerin tam listesi]() ve olası değerleri.)

Ayarlarında `run_configuration` ayarlanır `run_config` DSVM yapılandırması ve ayarları içeren bir nesne.  

```python
from azureml.train.automl import AutoMLConfig
import time
import logging

automl_settings = {
    "name": "AutoML_Demo_Experiment_{0}".format(time.time()),
    "max_time_sec": 600,
    "iterations": 20,
    "n_cross_validations": 5,
    "primary_metric": 'AUC_weighted',
    "preprocess": False,
    "concurrent_iterations": 10,
    "verbosity": logging.INFO
}

automl_config = AutoMLConfig(task='classification',
                             debug_log='automl_errors.log',
                             path=project_folder,
                             compute_target = dsvm_compute,
                             data_script=project_folder + "/get_data.py",
                             **automl_settings
                            )
```

## <a name="submit-automated-machine-learning-training-experiment"></a>Otomatik machine learning eğitim denemesini gönderin

Algoritma, Hiper parametre otomatik olarak seçmek için yapılandırma şimdi gönderin ve modeli eğitme. (Bilgi [ayarları hakkında daha fazla bilgi]() için `submit` yöntemi.)

```python
from azureml.core.experiment import Experiment
experiment=Experiment(ws, 'automl_remote')
remote_run = experiment.submit(automl_config, show_output=True)
```
Şuna benzer bir çıktı görürsünüz:

    Running on remote compute: mydsvmParent Run ID: AutoML_015ffe76-c331-406d-9bfd-0fd42d8ab7f6
    ***********************************************************************************************
    ITERATION: The iteration being evaluated.
    PIPELINE:  A summary description of the pipeline being evaluated.
    DURATION: Time taken for the current iteration.
    METRIC: The result of computing score on the fitted pipeline.
    BEST: The best observed score thus far.
    ***********************************************************************************************
    
     ITERATION     PIPELINE                               DURATION                METRIC      BEST
             2      Standardize SGD classifier            0.0                      0.954     0.954
             7      Normalizer DT                         0.0                      0.161     0.954
             0      Scale MaxAbs 1 extra trees            0.0                      0.936     0.954
             4      Robust Scaler SGD classifier          0.0                      0.867     0.954
             1      Normalizer kNN                        0.0                      0.984     0.984
             9      Normalizer extra trees                0.0                      0.834     0.984
             5      Robust Scaler DT                      0.0                      0.736     0.984
             8      Standardize kNN                       0.0                      0.981     0.984
             6      Standardize SVM                       2.2                      0.984     0.984
            10      Scale MaxAbs 1 DT                     0.0                      0.077     0.984
            11      Standardize SGD classifier            0.0                      0.863     0.984
             3      Standardize gradient boosting         5.4                      0.971     0.984
            12      Robust Scaler logistic regression     2.0                      0.955     0.984
            14      Scale MaxAbs 1 SVM                    0.0                      0.989     0.989
            13      Scale MaxAbs 1 gradient boosting      3.4                      0.971     0.989
            15      Robust Scaler kNN                     0.0                      0.904     0.989
            17      Standardize kNN                       0.0                      0.974     0.989
            16      Scale 0/1 gradient boosting           2.8                      0.968     0.989
            18      Scale 0/1 extra trees                 0.0                      0.828     0.989
            19      Robust Scaler kNN                     0.0                      0.983     0.989


## <a name="explore-results"></a>Sonuçları keşfedin

Aynı Jupyter pencere öğesi bir olarak kullanabileceğiniz [öğreticisini](tutorial-auto-train-models.md#explore-the-results) bir grafik ve tablo sonuçları görmek için.

```python
from azureml.train.widgets import RunDetails
RunDetails(remote_run).show()
```
Statik bir resim pencere öğesinin aşağıda verilmiştir.  Not Defteri çalıştırma özellikleri görmek ve günlükleri çalıştırılan için çıkış tablodaki herhangi bir satırdaki tıklayabilirsiniz.   Grafiğin üstünde açılan, her yineleme için her ölçüm grafiğini görüntülemek için de kullanabilirsiniz.

![pencere öğesi tablo](./media/how-to-auto-train-remote/table.png)
![pencere öğesi çizim](./media/how-to-auto-train-remote/plot.png)

Pencere öğesi görebilir ve çalıştırma ayrıntıları tek keşfetmek için kullanabileceğiniz bir URL görüntülenir.
 
### <a name="view-logs"></a>Günlükleri görüntüleme

Günlükleri altında/tmp/azureml_run / {kod incelemesinde} DSVM bulmak / azureml günlükleri.

## <a name="example"></a>Örnek

`automl/03.auto-ml-remote-execution.ipynb` Not Defteri, bu makaledeki kavramları göstermektedir.  Bu not alın:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bilgi [otomatik eğitim ayarlarını yapılandırma](how-to-configure-auto-train.md).
