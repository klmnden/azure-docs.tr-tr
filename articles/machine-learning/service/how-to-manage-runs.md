---
title: Başlatmak, izlemek ve python'da eğitim çalıştırmaları iptal etme
titleSuffix: Azure Machine Learning service
description: Başlat, durumu, etiketi olarak ve makine öğrenimi denemelerini düzenleme hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: roastala
author: rastala
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 04/05/2019
ms.openlocfilehash: 82df2258116ce55fa440b67ec0a66b106d0d72c7
ms.sourcegitcommit: 6e32f493eb32f93f71d425497752e84763070fad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2019
ms.locfileid: "59471503"
---
# <a name="start-monitor-and-cancel-training-runs-in-python"></a>Başlatmak, izlemek ve python'da eğitim çalıştırmaları iptal etme

[Python için Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) izlemek, düzenlemek ve eğitim ve deneme çalıştırmalarınızın yönetmek için çeşitli yöntemler sağlar.

Bu nasıl yapılır örnekleri aşağıdaki görevleri gösterir:

* [Performans İzleyicisi](#monitor)
* [İptal etme veya çalıştırma başarısız](#cancel)
* [Alt çalıştırmaları oluştur](#children)
* [Etiketi ve çalıştırmaları bulma](#tag)

## <a name="prerequisites"></a>Önkoşullar

Aşağıdakiler gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree) bugün.

* Bir Azure Machine Learning hizmeti çalışma alanı. Bkz: [bir Azure Machine Learning hizmeti çalışma alanı oluşturma](setup-create-workspace.md).

* Yüklü Python için Azure Machine Learning SDK'sını (sürüm 1.0.21 veya üzeri). SDK'sının en son sürüme güncelleştirmek veya yüklemek için Git [yükleme/güncelleştirmesi SDK'sı](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) sayfası.

    Azure Machine Learning SDK'sının sürümünüzü denetlemek için aşağıdaki kodu kullanın.

    ```Python
    print(azureml.core.VERSION)
    ```

<a name="monitor"></a>

## <a name="start-a-run-and-set-its-status"></a>Çalıştırma başlatılması ve durumunu ayarla

İçeri aktararak, deneme ayarlama ayarlamak [çalışma](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py), [deneme](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py), [çalıştırma](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py), ve [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) sınıflardan[azureml.core](https://docs.microsoft.com/python/api/azureml-core/azureml.core?view=azure-ml-py) paket.

```Python
import azureml.core
from azureml.core import Workspace, Experiment, Run
from azureml.core import ScriptRunConfig

ws = Workspace.from_config()
exp = Experiment(workspace=ws, name="explore-runs")
```

Bir farklı çalıştır ve kendi günlüğe kaydetme işlemine başlamak [ `start_logging()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment(class)?view=azure-ml-py#start-logging--args----kwargs-) yöntemi.

```Python
notebook_run = exp.start_logging()

notebook_run.log(name="message", value="Hello from run!")
```

Olan çalıştırma durumunu alın [ `get_status()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-status--).

```Python
print(notebook_run.get_status())
```

Çalıştırma kullanımı hakkındaki ek ayrıntıları almak için [ `get_details()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#get-details--).

```Python
notebook_run.get_details()
```

Çalıştırmanız başarıyla tamamlandığında kullanın [ `complete()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#complete--set-status-true-) tamamlandı olarak işaretlemek için yöntemi.

```Python
notebook_run.complete()
print(notebook_run.get_status())
```

Python'un kullanabilirsiniz `with...as` deseni. Çalıştırma kapsamı dışında olduğunda bu bağlamda çalıştırma otomatik olarak kendisini tamamlanmış olarak işaretler. Bu şekilde el ile çalıştırma tamamlandı olarak işaretlemek gerekmez.

```Python
with exp.start_logging() as notebook_run:
    notebook_run.log(name="message", value="Hello from run!")
    print("Is it still running?",notebook_run.get_status())

print("Has it completed?",notebook_run.get_status())
```

<a name="cancel"></a>

## <a name="cancel-or-fail-runs"></a>İptal etme veya çalıştırma başarısız

 Bir hata olduğuna dikkat edin veya çalıştırma gibi görünüyor son kullanmak için biraz zaman [ `cancel()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#cancel--) tamamlanmadan önce çalıştırmasını Durdur ve iptal edildi olarak işaretlemek için yöntem.

```Python
run_config = ScriptRunConfig(source_directory='.', script='hello_with_delay.py')

local_script_run = exp.submit(run_config)
print("Did the run start?",local_script_run.get_status())

local_script_run.cancel()
print("Did the run cancel?",local_script_run.get_status())
```

Çalıştırmanız tamamlandıktan, ancak gibi bir hata içeren, yanlış bir eğitim betiğini kullanıldı, kullanabilirsiniz [ `fail()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#fail-error-details-none---set-status-true-) yöntemi başarısız olarak işaretlenecek.

```Python
local_script_run = exp.submit(run_config)
local_script_run.fail()

print(local_script_run.get_status())
```

<a name="children"></a>

## <a name="create-child-runs"></a>Alt çalıştırmaları oluştur

Alt ilgili çalıştırma gibi yineleme ayarlama farklı hiper parametre için gruplamak için çalıştırmaları oluşturun.

Bu kod örneği hello_with_children.py betik çalıştırma bir gönderilen kullanarak içinde beş alt çalıştırmalardan toplu oluşturmak için kullanır. [ `child_run()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#child-run-name-none--run-id-none--outputs-none-) yöntemi.

```Python
!more hello_with_children.py
run_config = ScriptRunConfig(source_directory='.', script='hello_with_children.py')

local_script_run = exp.submit(run_config)
local_script_run.wait_for_completion(show_output=True)
print(local_script_run.get_status())

with exp.start_logging() as parent_run:
    for c,count in enumerate(range(5)):
        with parent_run.child_run() as child:
            child.log(name="Hello from child run", value=c)
```

> [!NOTE]
> Bunlar kapsam dışına taşımak gibi alt otomatik olarak tam çalışır.

Tek tek alt çalıştırmaları da başlatabilirsiniz, ancak her oluşturma bir ağ çağrısında sonuçlandığından çalıştırmaları toplu gönderme daha verimli değildir.

Kullanım [ `get_children()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-children-recursive-false--tags-none--properties-none--type-none--status-none---rehydrate-runs-true-) alt sorgu yöntemi, belirli bir üst öğesi çalıştırır.

```Python
list(parent_run.get_children())
```

<a name="tag"></a>

## <a name="tag-and-find-runs"></a>Etiketi ve çalıştırmaları bulma

Azure Machine Learning hizmetinde düzenlemek ve çalıştırmalarınızı önemli bilgiler için sorgu yardımcı olmak için özelliklerini ve etiketlerini kullanabilirsiniz.

### <a name="add-properties-and-tags"></a>Özellikler ve etiketler ekleme

Kullanım [ `add_properties()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#add-properties-properties-) çalışmalarınız için aranabilir meta veriler eklemek için. Örneğin, aşağıdaki kodu Çalıştır "Yazar" özelliği ekler.

```Python
local_script_run.add_properties({"author":"azureml-user"})
print(local_script_run.get_properties())
```

Özellikleri Denetim amacıyla kalıcı bir kayıt olarak faydalı olan, sabittir. Aşağıdaki kod örneği, zaten "azureml-kullanıcı" "Yazar" özelliği olarak önceki kodda ekledik olduğundan hata neden olur.

```Python
try:
    local_script_run.add_properties({"author":"different-user"})
except Exception as e:
    print(e)
```

Etiketler, ancak değiştirilebilir. Kullanım [ `tag()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#tag-key--value-none-) denemenizin Tüketiciler için arama yapılabilen ve anlamlı bilgileri eklemek için.

```Python
local_script_run.tag("quality", "great run")
print(local_script_run.get_tags())

local_script_run.tag("quality", "fantastic run")
print(local_script_run.get_tags())
```

Ayrıca, bir basit dize etiket ekleyebilirsiniz. Etiket sözlük değeri ile görünür `None`.

```Python
local_script_run.tag("worth another look")
print(local_script_run.get_tags())
```

### <a name="query-properties-and-tags"></a>Sorgu özellikleri ve etiketler

Çalıştırmaları sorgulayabilirsiniz belirli özelliklerini ve etiketlerini eşleşen bir denemeyi içinde.

```Python
list(exp.get_runs(properties={"author":"azureml-user"},tags={"quality":"fantastic run"}))
list(exp.get_runs(properties={"author":"azureml-user"},tags="worth another look"))
```

## <a name="example-notebooks"></a>Örnek Not Defterleri

Aşağıdaki not defterleri, bu makaledeki kavramları göstermektedir:

* API'leri günlüğe kaydetme hakkında daha fazla bilgi için bkz: [günlük API not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/logging-api/logging-api.ipynb).

* Azure Machine Learning SDK ile yönetme hakkında ek bilgi çalıştırmaları için bkz [çalıştırmaları not defteri yönetme](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/manage-runs).

## <a name="next-steps"></a>Sonraki adımlar

* Denemelerinizi için ölçümleri günlüğe kaydetme hakkında bilgi edinmek için [oturum ölçümleri eğitim çalıştırmaları sırasında](https://docs.microsoft.com/azure/machine-learning/service/how-to-track-experiment) makalesi.