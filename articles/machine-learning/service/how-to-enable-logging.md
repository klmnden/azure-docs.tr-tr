---
title: Azure Machine Learning hizmetinde günlüğe kaydetmeyi etkinleştir
titleSuffix: Azure Machine Learning service
description: SDK'sı özgü işlevini kullanarak yanı sıra Python günlük paketi hem varsayılan kullanarak Azure Machine Learning hizmetinde günlüğü etkinleştirme hakkında bilgi edinin.
ms.author: trbye
author: trevorbye
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: trbye
ms.date: 02/20/2019
ms.openlocfilehash: ce510168e2aa92758a3468fa55ff7b2a8d39b547
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58360276"
---
# <a name="enable-logging-in-azure-machine-learning-service"></a>Azure Machine Learning hizmetinde günlüğe kaydetmeyi etkinleştir

Azure Machine Learning Python SDK'sı, hem yerel günlüğe kaydetme ve çalışma alanınıza Portalı'nda oturum SDK özgü işlevini kullanarak yanı sıra varsayılan Python günlüğü paketini kullanarak günlüğe kaydetme etkinleştirmenize izin verir. Günlükleri, geliştiricilerin uygulama durumuyla ilgili gerçek zamanlı bilgi sağlamak ve bir hata veya uyarı Tanılama ile yardımcı olabilir. Bu makalede, aşağıdaki alanlarda günlüğü etkinleştirme farklı yollarını öğrenin:

> [!div class="checklist"]
> * Eğitim modelleri ve işlem hedefleri
> * Görüntü oluşturma
> * Dağıtılan modellerinde
> * Python `logging` ayarları

[Bir Azure Machine Learning hizmeti çalışma alanı oluşturma](setup-create-workspace.md). Kullanım [Kılavuzu](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) SDK daha fazla bilgi için.

## <a name="training-models-and-compute-target-logging"></a>Eğitim modelleri ve hedef günlük işlem

Model eğitimi işlemi sırasında günlük kaydını etkinleştirmek için birden çok yolu vardır ve sık karşılaşılan tasarım desenleri gösterilen örneklerden konacaktır. Kolayca çalıştırma ile ilgili veriler bulutta çalışma alanınızı kullanarak oturum açabilirsiniz `start_logging` işlevini `Experiment` sınıfı.

```python
from azureml.core import Experiment

exp = Experiment(workspace=ws, name='test_experiment')
run = exp.start_logging()
run.log("test-val", 10)
```

Başvuru belgelerine bakın [çalıştırma](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py) ek günlükler işlevler için sınıf.

Uygulama durumu yerel olarak günlüğe eğitim ilerleme sırasında etkinleştirmek için `show_output` parametresi. Ayrıntılı günlük kaydını etkinleştirme, eğitim işlem yanı sıra herhangi bir uzak kaynaklar hakkında bilgi ayrıntılarını görmek veya hedef işlem olanak tanır. Denemeyi gönderme sırasında günlük kaydını etkinleştirmek için aşağıdaki kodu kullanın.

```python
from azureml.core import Experiment

experiment = Experiment(ws, experiment_name)
run = experiment.submit(config=run_config_object, show_output=True)
```

Parametrenin aynısını kullanabilirsiniz `wait_for_completion` elde edilen çalıştırmaya işlevi.

```python
run.wait_for_completion(show_output=True)
```

SDK varsayılan python bir günlük paketi belirli senaryolar için eğitim kullanarak da destekler. Aşağıdaki örnek bir günlük tutma düzeyini sağlayan `INFO` içinde bir `AutoMLConfig` nesne.

```python
from azureml.train.automl import AutoMLConfig
import logging

automated_ml_config = AutoMLConfig(task = 'regression',
                                   verbosity=logging.INFO,
                                   X=your_training_features,
                                   y=your_training_labels,
                                   iterations=30,
                                   iteration_timeout_minutes=5,
                                   primary_metric="spearman_correlation")
```

Ayrıca `show_output` kalıcı işlem hedefi oluşturulurken parametre. Parametreyi belirtin `wait_for_completion` işlem hedef oluşturma sırasında günlük kaydını etkinleştirmek için işlevi.

```python
from azureml.core.compute import ComputeTarget

compute_target = ComputeTarget.attach(workspace=ws, name="example", attach_configuration=config)
compute.wait_for_completion(show_output=True)
```

## <a name="logging-during-image-creation"></a>Görüntü oluşturma işlemi sırasında günlük kaydı

Görüntü oluşturma sırasında günlük kaydını etkinleştirme derleme işlemi sırasında hataları görmenizi sağlayacak. Ayarlama `show_output` üzerinde param `wait_for_deployment()` işlevi.

```python
from azureml.core.webservice import Webservice

service = Webservice.deploy_from_image(deployment_config=your_config,
                                            image=image,
                                            name="example-image",
                                            workspace=ws)

service.wait_for_deployment(show_output=True)
```

## <a name="logging-for-deployed-models"></a>Dağıtılan modellerinde günlüğe kaydetme

Daha önceden dağıtılan web hizmeti günlüklerini almak için hizmeti yüklemek ve kullanmak `get_logs()` işlevi. Günlükleri, dağıtım sırasında oluşan hataları hakkında ayrıntılı bilgiler içeriyor olabilir.

```python
from azureml.core.webservice import Webservice

# load existing web service
service = Webservice(name="service-name", workspace=ws)
logs = service.get_logs()
```

Ayrıca özel oturum yığın izlemeler web hizmetiniz için izleme istek/yanıt süreleri, hata oranları ve özel durumları tanır Application Insights'ı etkinleştirerek. Çağrı `update()` Application Insights'ı etkinleştirmek için mevcut bir web hizmetini işlevi.

```python
service.update(enable_app_insights=True)
```

Bkz: [yapılır](how-to-enable-app-insights.md#enable-and-disable-in-the-portal) Azure portalında Application Insights ile çalışma konusunda daha fazla bilgi için.

## <a name="python-native-logging-settings"></a>Python yerel günlük ayarları

Belirli günlükleri SDK'sında hata ayıklama için günlük tutma düzeyini ayarlamaya yönlendiren bir hata içeriyor olabilir. Günlüğe kaydetme düzeyini ayarlamak için komut dosyanıza aşağıdaki kodu ekleyin.

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```