---
title: Azure Machine Learning hizmeti ile MLflow kullanın
titleSuffix: Azure Machine Learning service
description: Ölçümler ve yapıtları kullanarak Azure Machine Learning hizmetine MLflow kitaplığı oturum öğrenin
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.subservice: core
ms.reviewer: nibaccam
ms.topic: conceptual
ms.date: 06/10/2019
ms.custom: seodec18
ms.openlocfilehash: d0bc4620d0c55d6e94a3d99c39ab405dab2743e5
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67461659"
---
# <a name="use-mlflow-with-azure-machine-learning-service-preview"></a>Azure Machine Learning hizmeti (Önizleme) ile MLflow kullanın

Bu makalede MLflow'ın URI izleme ve günlüğe kaydetme API'si, nasıl kullanılacağını gösterir. toplu olarak da bilinen MLflow izleme, izleme ve deneme ölçümleri ve yapıları oturum için Azure Machine Learning hizmeti ile kendi [Azure Machine Learning Hizmet çalışma](https://docs.microsoft.com/azure/machine-learning/service/concept-azure-machine-learning-architecture#workspace). Zaten MLflow izleme denemelerinizi için kullanıyorsanız, çalışma alanı eğitim ölçümleri ve modelleri depolamak için merkezi, güvenli ve ölçeklenebilir bir konum sağlar.

[MLflow](https://www.mlflow.org) makine öğrenimi denemelerini yaşam döngüsünü yönetmeye yönelik bir açık kaynak kitaplığı. [MLFlow izleme](https://mlflow.org/docs/latest/quickstart.html#using-the-tracking-api) günlükleri ve ölçümleri çalıştırma eğitim izler MLflow bileşenidir ve model yapıtları denemelerinizi yerel olarak çalışan bir sanal makine veya uzak bir işlem kümesi.
![azure machine learning diyagramıyla mlflow](media/how-to-use-mlflow/mlflow-diagram.png)

## <a name="compare-mlflow-and-azure-machine-learning-clients"></a>MLflow ve Azure Machine Learning istemcileri karşılaştırın

 Azure Machine Learning hizmetini ve ilgili işlevi yeteneklerini kullanabileceğiniz farklı istemciler aşağıdaki tabloda özetlenmiştir.

 MLflow izleme ölçüm günlüğe kaydetme ve yapıt yalnızca aksi aracılığıyla kullanılabilir olan depolama işlevlerini sunar [Azure Machine Learning Python SDK'sı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

| | MLflow izleme | Azure Machine Learning <br> Python SDK'sı |  Azure Machine Learning <br> CLI | Azure portal|
|-|-|-|-|-|
| Çalışma alanını yönetme |   | ✓ |  ✓ | ✓  |
| Veri depolarını kullanma  |   | ✓ |  ✓ |    |
| Ölçümleri günlüğe kaydedin      | ✓ | ✓ |    |    |
| Yapıtları karşıya yükleyin | ✓ | ✓ |    |    |
| Ölçümleri görüntüle     | ✓ | ✓ | ✓  | ✓ |
| İşlemi yönetme   |   | ✓ | ✓  | ✓ |
| Modelleri dağıtma    |   | ✓ |   ✓ | ✓ |

## <a name="prerequisites"></a>Önkoşullar

* [MLflow yükleyin.](https://mlflow.org/docs/latest/quickstart.html)
* [Yerel bilgisayarınızda Azure Machine Learning Python SDK'sını yükleyin ve bir Azure Machine Learning çalışma alanı oluşturma](setup-create-workspace.md#sdk). SDK, çalışma alanına erişmeye MLflow için bağlantı sağlar.

## <a name="track-local-runs"></a>Yerel çalıştırmalarını izleme

Yükleme `azureml-contrib-run` MLflow izleme Azure Machine Learning sayesinde denemelerinizi bir Jupyter Not Defteri veya Kod Düzenleyicisi'nde yerel olarak çalıştırma kullanmak üzere paket.

```shell
pip install azureml-contrib-run
```

>[!NOTE]
>Hizmeti geliştirmek için çalışıyoruz azureml.contrib ad alanı sık sık değişir. Bu nedenle, bu ad alanındaki herhangi bir şey önizleme olarak kabul ve tamamen Microsoft tarafından desteklenmiyor.

İçeri aktarma `mlflow` ve [ `Workspace` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py) sınıfları MLflow erişmek için URI izleme ve çalışma alanınızı yapılandırın.

Aşağıdaki kodda, `get_mlflow_tracking_uri()` yöntemi çalışma alanına benzersiz bir izleme URI adresini atar `ws`, ve `set_tracking_uri()` URI bu adrese izleme MLflow işaret eder.

```Python
import mlflow
from azureml.core import Workspace

ws = Workspace.from_config()

mlflow.set_tracking_uri(ws.get_mlflow_tracking_uri())
```

>[!NOTE]
>İzleme URI geçerli bir saat veya daha az. Betiğinizi boşta geçen süre sonra yeniden başlatırsanız, yeni bir URI'ya almak için get_mlflow_tracking_uri API'yı kullanın.

Kümesinin MLflow deney adı ile `set_experiment()` ve çalıştırma eğitime başlayın `start_run()`. Ardından `log_metric()` MLflow günlüğe kaydetme API'si etkinleştirmek ve ölçümleri çalıştırma eğitim oturum başlatmak için.

```Python
experiment_name = "experiment_with_mlflow"
mlflow.set_experiment(experiment_name)

with mlflow.start_run():
    mlflow.log_metric('alpha', 0.03)
```

## <a name="track-remote-runs"></a>Uzak çalıştırmalarını izleme

Uzaktan çalıştırmalar GPU özellikli sağlanan sanal makineler veya Machine Learning işlem kümeleri gibi daha güçlü hesaplar üzerinde Modellerinizi eğitmek olanak tanır. Bkz: [işlem hedeflerine yönelik model eğitiminin ayarlama](how-to-set-up-training-targets.md) farklı işlem seçenekleri hakkında bilgi edinmek için.

İşlem ve eğitim çalıştırma ortamıyla yapılandırma [ `Environment` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) sınıfı. Dahil `mlflow` ve `azure-contrib-run` ortamın paketleri pip [ `CondaDependencies` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py) bölümü. Ardından oluşturmak [ `ScriptRunConfig` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig?view=azure-ml-py) işlem hedefi olarak uzak işlem ile.

```Python

from azureml.core import Environment
from azureml.core.conda_dependencies import CondaDependencies
from azureml.core import ScriptRunConfig

exp = Experiment(workspace = "my_workspace",
                 name = "my_experiment")

mlflow_env = Environment(name="mlflow-env")

cd = CondaDependencies.create(pip_packages = ["mlflow", "azureml-contrib-run"])

mlflow_env.python.conda_dependencies=cd

src = ScriptRunConfig(source_directory="./my_script_location", script="my_training_script.py")

src.run_config.target = "my-remote-compute-compute"
src.run_config.environment = mlflow_env
```

Eğitim betiğinizde alma `mlflow` MLflow API'leri günlüğe kaydetme ve çalıştırma ölçümlerinizi günlüğe kaydetmeyi Başlat.

```Python
import mlflow

with mlflow.start_run():
    mlflow.log_metric("example", 1.23)
```

İşlem ve eğitim çalıştırma yapılandırmasını kullanmak `Experiment.submit("train.py")` çalıştırma göndermek için yöntemi. Bu otomatik olarak URI izleme MLflow ayarlar ve MLflow günlük çalışma alanınıza yönlendirir.

```Python
run = exp.submit(src)
```

## <a name="view-metrics-and-artifacts-in-your-workspace"></a>Ölçümler ve yapıtları çalışma alanınızda görüntüleyin

Çalışma alanınızda tutulduğundan ölçümleri ve yapıtları MLflow oturum [Azure portalında](https://portal.azure.com). Bunları dilediğiniz zaman görüntülemek için çalışma alanınıza gidin ve denemenin ada göre bulur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Oturum ölçümleri ve yapıtları çalışma alanınızda kullanmayı planlamıyorsanız, ayrı ayrı silme özelliği şu anda kullanılamıyor. Bunun yerine, herhangi bir ücret ödememeniz depolama hesabı ve çalışma alanında, içeren kaynak grubunu silin:

1. Azure portalının en sol tarafındaki **Kaynak gruplarını** seçin.

   ![Azure portalında silme](media/how-to-use-mlflow/delete-resources.png)

1. Listeden oluşturduğunuz kaynak grubunu seçin.

1. **Kaynak grubunu sil**'i seçin.

1. Kaynak grubu adı girin. Ardından **Sil**’i seçin.

## <a name="example-notebooks"></a>Örnek Not Defterleri

[Azure ML Not Defterleriyle MLflow](https://aka.ms/azureml-mlflow-examples) bu makaledeki kavramları göstermektedir.

## <a name="next-steps"></a>Sonraki adımlar

* [Bir model dağıtma](how-to-deploy-and-where.md).
