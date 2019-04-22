---
title: Otomatik ML uzak işlem hedefleri
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti ile veri bilimi sanal makinesi (DSVM) uzak işlem hedefi üzerinde otomatik makine öğrenimini kullanarak model oluşturmayı öğrenin
services: machine-learning
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 6f2d71abeacee531b21a8276f621367dd39a39d9
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58891676"
---
# <a name="train-models-with-automated-machine-learning-in-the-cloud"></a>Bulutta otomatik machine learning ile modellerini eğitin

Azure Machine Learning'de yönettiğiniz işlem kaynaklarının farklı türlerde modelinizi eğitin. İşlem hedefi, yerel bir bilgisayar veya bulutta bir bilgisayar olabilir.

Kolayca artırın veya ek işlem hedefleri ekleyerek machine learning denemenizi ölçeklendirin. Bilgi işlem Ubuntu tabanlı veri bilimi sanal makinesi (DSVM) veya Azure Machine Learning işlem hedef seçenekleri içerir. Microsoft Azure bulutunda veri bilimi yapmak için özel olarak oluşturulmuş özel bir VM görüntüsü dsvm'dir. Bu, çok sayıda popüler veri bilimi ve önceden yüklenmiş ve önceden yapılandırılmış diğer araçları vardır.  

Bu makalede, otomatik ML DSVM'nin kullanarak model oluşturma konusunda bilgi edinin.

## <a name="how-does-remote-differ-from-local"></a>Uzaktan yerel bilgisayardan farkı nedir?

Öğreticiyi "[otomatik machine learning ile bir sınıflandırma modeli eğitme](tutorial-auto-train-models.md)" yerel bilgisayarda otomatik ML ile modeli eğitmek için nasıl kullanılacağını size öğretir.  Yerel olarak da eğitimindeki iş akışı uzak hedefleri için de geçerlidir. Ancak, uzak işlem ile otomatik ML deneme yinelemelerini zaman uyumsuz olarak yürütülür. Bu işlevsellik, belirli bir yinelemeye iptal etme, yürütme durumunu izlemek veya diğer Jupyter not defteri hücrelerde üzerinde çalışmaya devam sağlar. Uzaktan eğitmek için öncelikle bir Azure DSVM gibi uzak işlem hedefi oluşturun.  Ardından uzak kaynak yapılandırın ve kodunuzu var. gönderin.

Bu makalede, bir uzak DSVM'nin otomatik ML deneme çalıştırmak için gereken ek adımlar gösterilmektedir.  Çalışma alanı nesnesi `ws`, öğreticinin buraya kod kullanılır.

```python
ws = Workspace.from_config()
```

## <a name="create-resource"></a>Kaynak Oluştur

Çalışma alanınızda DSVM oluşturma (`ws`) zaten mevcut değilse. DSVM daha önce oluşturulmuş olsa bile, bu kod oluşturma işlemini atlar ve mevcut kaynak ayrıntıya yükler `dsvm_compute` nesne.  

**Tahmini Süre**: VM'nin oluşturulması yaklaşık 5 dakika sürer.

```python
from azureml.core.compute import DsvmCompute

dsvm_name = 'mydsvm' #Name your DSVM
try:
    dsvm_compute = DsvmCompute(ws, dsvm_name)
    print('found existing dsvm.')
except:
    print('creating new dsvm.')
    # Below is using a VM of SKU Standard_D2_v2 which is 2 core machine. You can check Azure virtual machines documentation for additional SKUs of VMs.
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

Bu kod, bir kullanıcı adı veya parola, sağlanan DSVM için oluşturmaz. Doğrudan VM'ye bağlanmak istiyorsanız, Git [Azure portalında](https://portal.azure.com) kimlik bilgilerini oluşturmak için.  

### <a name="attach-existing-linux-dsvm"></a>Var olan bir Linux DSVM'sini ekleme

Bu gibi durumlarda, var olan bir Linux DSVM'sini da işlem hedefi olarak ekleyebilirsiniz. Bu örnekte, mevcut bir DSVM kullanır, ancak yeni bir kaynak oluşturmaz.

> [!NOTE]
>
> Aşağıdaki kod [RemoteCompute](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.remote.remotecompute?view=azure-ml-py) hedef, işlem hedefi olarak varolan bir VM'yi eklemek için sınıf.
> [DsvmCompute](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.dsvmcompute?view=azure-ml-py) sınıfı kullanımdan kaldırılacaktır gelecek sürümlerde bu tasarım deseni ile değiştiriliyor.

Önceden var olan bir Linux DSVM'sini işlem hedefi oluşturmak için aşağıdaki kodu çalıştırın.

```python
from azureml.core.compute import ComputeTarget, RemoteCompute 

attach_config = RemoteCompute.attach_configuration(username='<username>',
                                                   address='<ip_address_or_fqdn>',
                                                   ssh_port=22,
                                                   private_key_file='./.ssh/id_rsa')
compute_target = ComputeTarget.attach(workspace=ws,
                                      name='attached-vm',
                                      attach_configuration=attach_config)

compute_target.wait_for_completion(show_output=True)
```

Artık `compute_target` uzak işlem hedefi olarak nesnesi.

## <a name="access-data-using-getdata-file"></a>Verilere get_data dosyası kullanma

Eğitim verilerinizi uzak bir kaynağa erişim sağlar. Uzak işlem üzerinde çalışan otomatik makine öğrenimi denemeleri için verilerin kullanarak getirilmesi gerekir. bir `get_data()` işlevi.  

Erişim sağlamak için yapmanız gerekir:
+ Get_data.py içeren dosyayı oluşturma bir `get_data()` işlevi 
+ Bu dosyanın mutlak bir yol olarak erişilebilir bir dizine yerleştirin 

Bir blob depolama veya yerel disk get_data.py dosyasındaki verileri okumak için kod yalıtabilirsiniz. Aşağıdaki kod örneğinde, veriler sklearn öğesini paketten gelir.

>[!Warning]
>Uzak işlem kullandığınız sonra kullanmalısınız `get_data()` veri Bağlantılarınızdaki gerçekleştirildiği. Veri Dönüşümleri için ek kitaplıklar get_data() bir parçası olarak yüklemeniz gerekiyorsa, izlenmesi için ek adımlar vardır. Başvurmak [otomatik ml dataprep örnek not defteri](https://aka.ms/aml-auto-ml-data-prep ) Ayrıntılar için.


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

## <a name="configure-experiment"></a>Deneme yapılandırma

Ayarlarını belirtin `AutoMLConfig`.  (Bkz: bir [parametrelerin tam listesi](how-to-configure-auto-train.md#configure-experiment) ve olası değerleri.)

Ayarlarında `run_configuration` ayarlanır `run_config` DSVM yapılandırması ve ayarları içeren bir nesne.  

```python
from azureml.train.automl import AutoMLConfig
import time
import logging

automl_settings = {
    "name": "AutoML_Demo_Experiment_{0}".format(time.time()),
    "iteration_timeout_minutes": 10,
    "iterations": 20,
    "n_cross_validations": 5,
    "primary_metric": 'AUC_weighted',
    "preprocess": False,
    "max_concurrent_iterations": 10,
    "verbosity": logging.INFO
}

automl_config = AutoMLConfig(task='classification',
                             debug_log='automl_errors.log',
                             path=project_folder,
                             compute_target = dsvm_compute,
                             data_script=project_folder + "/get_data.py",
                             **automl_settings,
                            )
```

### <a name="enable-model-explanations"></a>Model açıklamalarını etkinleştir

İsteğe bağlı `model_explainability` parametresinde `AutoMLConfig` Oluşturucusu. Ayrıca, bir doğrulama dataframe nesnesi bir parametre olarak geçirilmelidir `X_valid` modeli explainability özelliği kullanmak için.

```python
automl_config = AutoMLConfig(task='classification',
                             debug_log='automl_errors.log',
                             path=project_folder,
                             compute_target = dsvm_compute,
                             data_script=project_folder + "/get_data.py",
                             **automl_settings,
                             model_explainability=True,
                             X_valid = X_test
                            )
```

## <a name="submit-training-experiment"></a>Eğitim denemesini gönderin

Algoritma, Hiper parametre otomatik olarak seçmek için yapılandırma şimdi gönderin ve modeli eğitme.

```python
from azureml.core.experiment import Experiment
experiment=Experiment(ws, 'automl_remote')
remote_run = experiment.submit(automl_config, show_output=True)
```

Aşağıdaki örneğe benzer bir çıktı görürsünüz:

    Running on remote compute: mydsvmParent Run ID: AutoML_015ffe76-c331-406d-9bfd-0fd42d8ab7f6
    ***********************************************************************************************
    ITERATION: The iteration being evaluated.
    PIPELINE:  A summary description of the pipeline being evaluated.
    DURATION: Time taken for the current iteration.
    METRIC: The result of computing score on the fitted pipeline.
    BEST: The best observed score thus far.
    ***********************************************************************************************
    
     ITERATION     PIPELINE                               DURATION                METRIC      BEST
             2      Standardize SGD classifier            0:02:36                  0.954     0.954
             7      Normalizer DT                         0:02:22                  0.161     0.954
             0      Scale MaxAbs 1 extra trees            0:02:45                  0.936     0.954
             4      Robust Scaler SGD classifier          0:02:24                  0.867     0.954
             1      Normalizer kNN                        0:02:44                  0.984     0.984
             9      Normalizer extra trees                0:03:15                  0.834     0.984
             5      Robust Scaler DT                      0:02:18                  0.736     0.984
             8      Standardize kNN                       0:02:05                  0.981     0.984
             6      Standardize SVM                       0:02:18                  0.984     0.984
            10      Scale MaxAbs 1 DT                     0:02:18                  0.077     0.984
            11      Standardize SGD classifier            0:02:24                  0.863     0.984
             3      Standardize gradient boosting         0:03:03                  0.971     0.984
            12      Robust Scaler logistic regression     0:02:32                  0.955     0.984
            14      Scale MaxAbs 1 SVM                    0:02:15                  0.989     0.989
            13      Scale MaxAbs 1 gradient boosting      0:02:15                  0.971     0.989
            15      Robust Scaler kNN                     0:02:28                  0.904     0.989
            17      Standardize kNN                       0:02:22                  0.974     0.989
            16      Scale 0/1 gradient boosting           0:02:18                  0.968     0.989
            18      Scale 0/1 extra trees                 0:02:18                  0.828     0.989
            19      Robust Scaler kNN                     0:02:32                  0.983     0.989


## <a name="explore-results"></a>Sonuçları keşfedin

Aynı Jupyter pencere öğesi bir olarak kullanabileceğiniz [öğreticisini](tutorial-auto-train-models.md#explore-the-results) bir grafik ve tablo sonuçları görmek için.

```python
from azureml.widgets import RunDetails
RunDetails(remote_run).show()
```
Burada pencere öğesinin statik bir görüntüsü yer alır.  Not Defteri çalıştırma özellikleri görmek ve günlükleri çalıştırılan için çıkış tablodaki herhangi bir satırdaki tıklayabilirsiniz.   Grafiğin üstünde açılan, her yineleme için her ölçüm grafiğini görüntülemek için de kullanabilirsiniz.

![pencere öğesi tablosu](./media/how-to-auto-train-remote/table.png)
![pencere öğesi çizimi](./media/how-to-auto-train-remote/plot.png)

Pencere öğesi görebilir ve çalıştırma ayrıntıları tek keşfetmek için kullanabileceğiniz bir URL görüntülenir.
 
### <a name="view-logs"></a>Günlükleri görüntüleme

Günlükleri altında DSVM bulmak `/tmp/azureml_run/{iterationid}/azureml-logs`.

## <a name="best-model-explanation"></a>En iyi modeli açıklaması

Model açıklaması verileri alınırken arka ucunda çalışan ne, konusunda saydamlık artırmak için modelleri hakkında ayrıntılı bilgi sağlar. Bu örnekte yalnızca en iyi uygun model için model açıklamaları çalıştırın. İşlem hattındaki tüm modeller için çalıştırırsanız, önemli çalışma zamanında neden olur. Model açıklaması bilgileri içerir:

* shap_values: Şekil lib tarafından oluşturulan açıklama bilgileri
* expected_values: Beklenen değer X_train verilerin ayarlamak için uygulanan modeli.
* overall_summary: Azalan düzende sıralanmış model düzeyi özelliği önem değerleri
* overall: Özellik adları overall_summary olduğu gibi aynı sırada sıralandı
* per_class_summary: Azalan düzende sıralanmış sınıf düzeyi özelliği önem değerleri. Yalnızca sınıflandırma çalışması için kullanılabilir
* per_class: Özellik adları per_class_summary olduğu gibi aynı sırada sıralanır. Yalnızca sınıflandırma çalışması için kullanılabilir

En iyi işlem hattı yinelemelerinizi seçmek için aşağıdaki kodu kullanın. `get_output` Yöntemi, en iyi çalıştırmanın ve ekrana sığdırılmış modeli son çağırma sığdırmak için döndürür.

```python
best_run, fitted_model = remote_run.get_output()
```

İçeri aktarma `retrieve_model_explanation` işlevini ve en iyi modeli üzerinde çalıştırın.

```python
from azureml.train.automl.automlexplainer import retrieve_model_explanation

shap_values, expected_values, overall_summary, overall_imp, per_class_summary, per_class_imp = \
    retrieve_model_explanation(best_run)
```

Sonuçları yazdırma `best_run` görüntülemek istediğiniz açıklama değişkenleri.

```python
print(overall_summary)
print(overall_imp)
print(per_class_summary)
print(per_class_imp)
```

Yazdırma `best_run` açıklama Özet değişkenleri sonuçları aşağıdaki çıktıda.

![Model explainability konsol çıktısı](./media/how-to-auto-train-remote/expl-print.png)

Çalışma alanınız içinde özellik önem UI pencere öğesi ve bunun yanı sıra Azure Portal'da web UI aracılığıyla da görselleştirebilirsiniz.

![Model explainability kullanıcı Arabirimi](./media/how-to-auto-train-remote/model-exp.png)

## <a name="example"></a>Örnek

[How-to-use-azureml/automated-machine-learning/remote-execution/auto-ml-remote-execution.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/remote-execution/auto-ml-remote-execution.ipynb) Not Defteri, bu makaledeki kavramları göstermektedir. 

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bilgi [otomatik eğitim ayarlarını yapılandırma](how-to-configure-auto-train.md).
