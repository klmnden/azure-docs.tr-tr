---
title: Başlatmak, izlemek ve python'da eğitim çalıştırmaları iptal etme
titleSuffix: Azure Machine Learning service
description: Başlat, durumu, etiketi olarak ve makine öğrenimi denemelerinizi düzenleme hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: roastala
author: rastala
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 04/05/2019
ms.openlocfilehash: a67ac07c26063b380bda2b8cb2b6a02677e7f816
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67656196"
---
# <a name="start-monitor-and-cancel-training-runs-in-python"></a>Başlatmak, izlemek ve python'da eğitim çalıştırmaları iptal etme

[Python için Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) ve [Machine Learning CLI](reference-azure-machine-learning-cli.md) izlemek, düzenlemek ve eğitim ve deneme çalıştırmalarınızın yönetmek için çeşitli yöntemler sağlar.

Bu makalede, örnek olarak aşağıdaki görevleri gösterir:

* Performans İzleyicisi.
* İptal etme veya çalıştırma başarısız.
* Alt çalıştırmaları oluşturun.
* Etiket ve çalıştırmaları bulun.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki öğeler gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree) bugün.

* Bir [Azure Machine Learning hizmeti çalışma alanında](setup-create-workspace.md).

* Python için Azure Machine Learning SDK'sını (sürüm 1.0.21 veya üzeri). Yüklemek veya SDK'sının en son sürüme güncelleştirmek için bkz: [yükleme veya SDK'sı güncelleştirme](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

    Azure Machine Learning SDK'sının sürümünüzü denetlemek için aşağıdaki kodu kullanın:

    ```Python
    print(azureml.core.VERSION)
    ```

* [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) ve [CLI uzantısını Azure Machine Learning hizmeti için](reference-azure-machine-learning-cli.md).

## <a name="start-a-run-and-its-logging-process"></a>Bir farklı çalıştır ve kendi günlüğe kaydetme işlemi Başlat

### <a name="using-the-sdk"></a>SDK’yı kullanarak

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

### <a name="using-the-cli"></a>CLI kullanarak

Çalıştırma denemenizi başlatmak için aşağıdaki adımları kullanın:

1. Bir kabuk ya da komut isteminden, Azure aboneliğiniz için kimlik doğrulaması için Azure CLI'yı kullanın:

    ```azurecli-interactive
    az login
    ```

1. Bir çalışma alanı yapılandırması eğitim komut dosyanızı içeren klasöre ekleyin. Değiştirin `myworkspace` ile Azure Machine Learning hizmeti çalışma. Değiştirin `myresourcegroup` çalışma alanınızı içeren Azure kaynak grubu ile:

    ```azurecli-interactive
    az ml folder attach -w myworkspace -g myresourcegroup
    ```

    Bu komut, oluşturur bir `.azureml` örnek runconfig ve conda ortam dosyaları içeren alt. Ayrıca içerdiği bir `config.json` , Azure Machine Learning çalışma alanı ile iletişim kurmak için kullanılan dosya.

    Daha fazla bilgi için [az ml klasör ekleme](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/folder?view=azure-cli-latest#ext-azure-cli-ml-az-ml-folder-attach).

2. Çalıştırma başlatmak için aşağıdaki komutu kullanın. Bu komutu kullanırken runconfig dosyasının adını belirtin (metinden önce \*dosya sisteminizin arıyorsanız .runconfig) karşı - c parametresi.

    ```azurecli-interactive
    az ml run submit-script -c sklearn -e testexperiment train.py
    ```

    > [!TIP]
    > `az ml folder attach` Oluşturduğunuz komutu bir `.azureml` iki örnek runconfig dosyaları içeren alt dizini. 
    >
    > Program aracılığıyla çalıştırma yapılandırma nesnesini oluşturan bir Python komut dosyası varsa, kullanabileceğiniz [RunConfig.save()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py#save-path-none--name-none--separate-environment-yaml-false-) runconfig dosyası olarak kaydedin.
    >
    > Daha fazla örnek runconfig dosyalar için bkz: [ https://github.com/MicrosoftDocs/pipelines-azureml/tree/master/.azureml ](https://github.com/MicrosoftDocs/pipelines-azureml/tree/master/.azureml).

    Daha fazla bilgi için [az ml çalıştırma betiği Gönder](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-script).

## <a name="monitor-the-status-of-a-run"></a>Bir çalıştırma durumunu izleme

### <a name="using-the-sdk"></a>SDK’yı kullanarak

İle bir çalıştırmanın durumunu Al [ `get_status()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-status--) yöntemi.

```Python
print(notebook_run.get_status())
```

Çalıştırma hakkında ek ayrıntıları almak için kullanın [ `get_details()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#get-details--) yöntemi.

```Python
notebook_run.get_details()
```

Çalıştırmanız başarıyla tamamlandığında kullanın [ `complete()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#complete--set-status-true-) tamamlandı olarak işaretlemek için yöntemi.

```Python
notebook_run.complete()
print(notebook_run.get_status())
```

Python'un kullanırsanız `with...as` deseni çalıştırma otomatik olarak işaretlenmesine neden olacak kendisini kapsam dışına çalıştırıldığında tamamlandı olarak. El ile çalıştırma tamamlandı olarak işaretlemek gerekmez.

```Python
with exp.start_logging() as notebook_run:
    notebook_run.log(name="message", value="Hello from run!")
    print("Is it still running?",notebook_run.get_status())

print("Has it completed?",notebook_run.get_status())
```

### <a name="using-the-cli"></a>CLI kullanarak

1. Denemeniz için çalıştırmaları listesini görüntülemek için aşağıdaki komutu kullanın. Değiştirin `experiment` denemenizi adı:

    ```azurecli-interactive
    az ml run list --experiment-name experiment
    ```

    Bu komut, bu deneme için çalıştırma hakkında bilgi listeleyen bir JSON belgesini döndürür.

    Daha fazla bilgi için [az ml denemeler listesi](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/experiment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-experiment-list).

2. Belirli bir çalıştırma hakkında bilgi görüntülemek için aşağıdaki komutu kullanın. Değiştirin `runid` çalıştırma kimliği:

    ```azurecli-interactive
    az ml run show -r runid
    ```

    Bu komut çalıştırma hakkında bilgi listeleyen bir JSON belgesini döndürür.

    Daha fazla bilgi için [az ml çalıştırma show](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-show).

## <a name="cancel-or-fail-runs"></a>İptal etme veya çalıştırma başarısız

Bir hata olduğunu fark ederseniz veya çalıştırma tamamlanması uzun zaman alıyorsa, çalıştırmayı iptal edebilirsiniz.

### <a name="using-the-sdk"></a>SDK’yı kullanarak

SDK'sını kullanarak çalıştırma iptal etmek için kullanın [ `cancel()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#cancel--) yöntemi:

```Python
run_config = ScriptRunConfig(source_directory='.', script='hello_with_delay.py')

local_script_run = exp.submit(run_config)
print("Did the run start?",local_script_run.get_status())

local_script_run.cancel()
print("Did the run cancel?",local_script_run.get_status())
```

Çalıştırmanız tamamlandıktan, ancak bir hata içeriyor (örneğin, yanlış bir eğitim betiğini kullanılan), kullanabileceğiniz [ `fail()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)#fail-error-details-none--error-code-none---set-status-true-) yöntemi başarısız olarak işaretlenecek.

```Python
local_script_run = exp.submit(run_config)
local_script_run.fail()

print(local_script_run.get_status())
```

### <a name="using-the-cli"></a>CLI kullanarak

CLI kullanarak bir çalıştırma iptal etmek için aşağıdaki komutu kullanın. Değiştirin `runid` çalıştırma kimliği

```azurecli-interactive
az ml run cancel -r runid
```

Daha fazla bilgi için [az ml çalıştırma iptal](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-cancel).

## <a name="create-child-runs"></a>Alt çalıştırmaları oluştur

Alt ilgili çalışmaları gibi farklı hiper parametre ayarı yinelemelerini gruplamak için çalıştırmaları oluşturun.

> [!NOTE]
> Alt çalıştırmaları yalnızca SDK'sını kullanarak oluşturulabilir.

Bu kod örneği kullanan `hello_with_children.py` gönderilen çalışması içinde beş alt çalıştırmalardan toplu kullanarak oluşturmak için betik [ `child_run()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#child-run-name-none--run-id-none--outputs-none-) yöntemi:

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
> Kapsam dışına taşınabilecek gibi alt çalışmaları otomatik olarak tamamlandı olarak işaretlenir.

Tek tek alt çalıştırmaları da başlatabilirsiniz, ancak her oluşturma bir ağ çağrısında sonuçlandığından çalıştırmaları toplu gönderme daha verimli değildir.

Belirli bir üst grubun alt çalıştırmalar sorgulamak için aşağıdaki komutu kullanın [ `get_children()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-children-recursive-false--tags-none--properties-none--type-none--status-none---rehydrate-runs-true-) yöntemi.

```Python
list(parent_run.get_children())
```

## <a name="tag-and-find-runs"></a>Etiketi ve çalıştırmaları bulma

Azure Machine Learning hizmetinde düzenlemek ve çalıştırmalarınızı önemli bilgiler için sorgu yardımcı olmak için özelliklerini ve etiketlerini kullanabilirsiniz.

### <a name="add-properties-and-tags"></a>Özellikler ve etiketler ekleme

#### <a name="using-the-sdk"></a>SDK’yı kullanarak

Aranabilir meta veriler çalışmalarınız için eklemek için [ `add_properties()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#add-properties-properties-) yöntemi. Örneğin, aşağıdaki kodu ekler `"author"` özelliği farklı çalıştır:

```Python
local_script_run.add_properties({"author":"azureml-user"})
print(local_script_run.get_properties())
```

Özellikler, denetim amacıyla kalıcı bir kayıt oluşturmak için sabittir. Zaten eklenmiş olduğundan aşağıdaki örnek bir hata sonuçlarında kod `"azureml-user"` olarak `"author"` önceki kodda özellik değeri:

```Python
try:
    local_script_run.add_properties({"author":"different-user"})
except Exception as e:
    print(e)
```

Özelliklerin aksine, etiketler değiştirilebilir. Tüketiciler için arama yapılabilen ve anlamlı bilgileri denemenizi eklemek için [ `tag()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#tag-key--value-none-) yöntemi.

```Python
local_script_run.tag("quality", "great run")
print(local_script_run.get_tags())

local_script_run.tag("quality", "fantastic run")
print(local_script_run.get_tags())
```

Basit dize etiketlerini de ekleyebilirsiniz. Bu etiketler etiket sözlüğünde görüntülendiğinde, bunlar bir değere sahip `None`.

```Python
local_script_run.tag("worth another look")
print(local_script_run.get_tags())
```

#### <a name="using-the-cli"></a>CLI kullanarak

> [!NOTE]
> CLI kullanarak, yalnızca eklemek veya etiketleri güncelleştirin.

Ekleme veya bir etiket güncelleştirmek için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml run update -r runid --add-tag quality='fantastic run'
```

Daha fazla bilgi için [az ml güncelleştirme çalıştırması](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-update).

### <a name="query-properties-and-tags"></a>Sorgu özellikleri ve etiketler

Çalıştırmaları sorgulayabilirsiniz belirli özelliklerini ve etiketlerini eşleşen çalıştırmalarının bir listesini döndürmek için bir deneme içinde.

#### <a name="using-the-sdk"></a>SDK’yı kullanarak

```Python
list(exp.get_runs(properties={"author":"azureml-user"},tags={"quality":"fantastic run"}))
list(exp.get_runs(properties={"author":"azureml-user"},tags="worth another look"))
```

#### <a name="using-the-cli"></a>CLI kullanarak

Azure CLI'yı destekleyen [JMESPath](http://jmespath.org) çalıştırmaları özelliklerini ve etiketlerini göre filtrelemek için kullanılan sorgu. Azure CLI ile JMESPath sorgusu kullanmak için onunla belirtin `--query` parametresi. Aşağıdaki örnekler temel sorgular özelliklerini ve etiketlerini kullanarak göstermektedir:

```azurecli-interactive
# list runs where the author property = 'azureml-user'
az ml run list --experiment-name experiment [?properties.author=='azureml-user']
# list runs where the tag contains a key that starts with 'worth another look'
az ml run list --experiment-name experiment [?tags.keys(@)[?starts_with(@, 'worth another look')]]
# list runs where the author property = 'azureml-user' and the 'quality' tag starts with 'fantastic run'
az ml run list --experiment-name experiment [?properties.author=='azureml-user' && tags.quality=='fantastic run']
```

Azure CLI sonuçları sorgulama hakkında daha fazla bilgi için bkz: [sorgu Azure CLI komut çıktısı](https://docs.microsoft.com/cli/azure/query-azure-cli?view=azure-cli-latest).

## <a name="example-notebooks"></a>Örnek Not Defterleri

Aşağıdaki not defterleri, bu makaledeki kavramları göstermektedir:

* API'leri günlüğü hakkında daha fazla bilgi için bkz. [günlük API not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/logging-api/logging-api.ipynb).

* Azure Machine Learning SDK ile yönetme hakkında daha fazla bilgi çalıştıran için bkz: [çalıştırmaları not defteri yönetme](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/manage-runs).

## <a name="next-steps"></a>Sonraki adımlar

* Denemelerinizi için ölçümleri günlüğe kaydetme hakkında bilgi edinmek için [oturum ölçümleri eğitim çalıştırmaları sırasında](how-to-track-experiments.md).
