---
title: Oluşturun, çalıştırın ve ML işlem hatlarını izleyin
titleSuffix: Azure Machine Learning service
description: Oluşturun ve Python için Azure Machine Learning SDK'sı ile işlem hattı öğrenme bir makine çalıştırın. İşlem hatları oluşturmak ve iş akışları, Birleştir birlikte machine learning (ML) aşamalarını yönetmek için kullanın. Veri hazırlama, model eğitiminin, model dağıtımı ve çıkarım bu aşamaları içerir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.author: sanpil
author: sanpil
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 3ec3e915c26abf38653d1bddfe0a5ba44d5e6de1
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64914886"
---
# <a name="create-and-run-a-machine-learning-pipeline-by-using-azure-machine-learning-sdk"></a>Oluşturma ve Azure Machine Learning SDK'sını kullanarak bir makine öğrenimi işlem hattı çalıştırma

Bu makalede, oluşturmak, yayımlamak, çalıştırmak ve izlemek öğrenin bir [makine öğrenimi işlem hattı](concept-ml-pipelines.md) kullanarak [Azure Machine Learning SDK'sı](https://aka.ms/aml-sdk).  Bu komut zincirleri oluşturmak ve çeşitli makine öğrenme aşamaları birleştirmek iş akışlarını yönetme yardımcı olur. Veri hazırlama ve modeli eğitimi gibi bir Ardışık düzenin her aşaması, bir veya daha fazla adım ekleyebilirsiniz.

Oluşturduğunuz işlem hattı, Azure Machine Learning hizmeti üyelerine görünür [çalışma](how-to-manage-workspace.md). 

İşlem hatları, hesaplama ve bu işlem hattı çalıştırmasıyla ilişkili Ara ve son veri depolama için uzak işlem hedefleri kullanın. İşlem hatları okuyabilir ve yazma veri ve desteklenen [Azure depolama](https://docs.microsoft.com/azure/storage/) konumları.

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree).

## <a name="prerequisites"></a>Önkoşullar

* [Geliştirme ortamınızı yapılandırma](how-to-configure-environment.md) Azure Machine Learning SDK'sını yüklemek için.

* Oluşturma bir [Azure Machine Learning çalışma alanı](how-to-configure-environment.md#workspace) tüm işlem hattı kaynakları tutmak için. 

  ```python
  from azureml.core import Workspace
  
  ws = Workspace.create(
     name = '<workspace-name>',
     subscription_id = '<subscription-id>',
     resource_group = '<resource-group>',
     location = '<workspace_region>',
     exist_ok = True)
  ```

## <a name="set-up-machine-learning-resources"></a>Machine learning kaynaklarını ayarlama

Bir işlem hattını çalıştırmak için gereken kaynakları oluşturun:

* İşlem hattı adımları gerekli verilere erişmek için kullanılan bir veri deposunu ayarlayın.

* Yapılandırma bir `DataReference` kendini ya da bir veri deposu, erişilebilir veri işaret edecek şekilde nesne.

* Ayarlanan [hedefleri işlem](concept-azure-machine-learning-architecture.md#compute-target) işlem hattı adımlarınızı çalıştırılacağı.

### <a name="set-up-a-datastore"></a>Bir veri deposu ayarlayın
Bir veri deposuna erişmek işlem hattının verileri depolar. Her bir çalışma alanı bir varsayılan veri deposu sahiptir. Ek veri depoları kaydedebilirsiniz. 

Çalışma Alanınızı oluştururken [Azure dosyaları](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) ve [Azure Blob Depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) varsayılan çalışma alanına eklenir. Azure dosyaları, bir çalışma alanı için varsayılan veri deposu olduğu halde Blob depolamayı bir veri deposu olarak kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure dosyaları, Azure Blobları veya Azure diskleri ne zaman kullanılacağını belirleme](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks). 

```python
# Default datastore (Azure file storage)
def_data_store = ws.get_default_datastore() 

# The above call is equivalent to this 
def_data_store = Datastore(ws, "workspacefilestore")

# Get blob storage associated with the workspace
def_blob_store = Datastore(ws, "workspaceblobstore")
```

Bunlar işlem hatlarınızı erişilebilir olması için veri deposu, veri dosyaları veya dizinleri yükleyin. Bu örnek, Blob Depolama veri deposu sürümü kullanır:

```python
def_blob_store.upload_files(
    ["./data/20news.pkl"],
    target_path="20newsgroups", 
    overwrite=True)
```

Bir işlem hattı, bir veya daha fazla adımdan oluşur. Bir adım, bir işlem hedefte çalıştırma bir birimdir. Adımlar veri kaynaklarını ve "Ara" veri üretir. Bir adım, veri modeli, modeli ve bağımlı dosyaları ile bir dizin veya geçici veri oluşturabilirsiniz. Bu veriler daha sonra ardışık düzende sonraki diğer adımları için kullanılabilir.

### <a name="configure-data-reference"></a>Veri başvurusu yapılandırma

Başvurulan veri kaynağı bir işlem hattı, bir adım bir giriş olarak oluşturduğunuz. Bir veri kaynağında bir işlem hattı tarafından temsil edilen bir [DataReference](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference) nesne. `DataReference` Nesne işaret kendini veya bir veri deposundan erişilebilir veriler.

```python
blob_input_data = DataReference(
    datastore=def_blob_store,
    data_reference_name="test_data",
    path_on_datastore="20newsgroups/20news.pkl")
```

Ara veriler (veya bir adımın çıkış) tarafından temsil edilen bir [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py) nesne. `output_data1` bir adımın çıktı olarak bir oluşturulur ve bir veya daha fazla gelecekteki adımları girdisi olarak kullanılır. `PipelineData` adımlar arasında veri bağımlılığı tanıtır ve işlem hattı, bir örtük yürütme sırası oluşturur.

```python
output_data1 = PipelineData(
    "output_data1",
    datastore=def_blob_store,
    output_name="output_data1")
```

## <a name="set-up-compute-target"></a>İşlem Hedefi ' ayarlayın

Azure Machine Learning içinde terimi __işlem__ (veya __hedef işlem__) makineleri ya da machine learning işlem hattı, hesaplama adımları kümeleri anlamına gelir.   Bkz: [model eğitim hedefleri işlem](how-to-set-up-training-targets.md) işlem hedefleri ve nasıl oluşturup bunları çalışma alanınıza eklemek tam bir listesi için.  İşlem hedefi ekleme ve oluşturma işlemi modeli veya bir işlem hattı adım çalıştırılıyor bağımsız olarak aynıdır. Oluşturma ve işlem hedef ekleme sonra kullanın `ComputeTarget` nesnesine, [işlem hattı adım](#steps).

> [!IMPORTANT]
> İşlem hedefleri üzerinde yönetim işlemlerini gerçekleştirme desteklenmiyor gelen içinde uzak işler. Machine learning işlem hatlarını uzak iş olarak gönderilen olduğundan, işlem hattı içindeki işlem hedeflerden yönetim işlemlerini kullanmayın.

Oluşturma ve işlem hedeflerine yönelik ekleme örnekleri aşağıda verilmiştir:

* Azure Machine Learning işlem
* Azure Databricks 
* Azure Data Lake Analytics

### <a name="azure-machine-learning-compute"></a>Azure Machine Learning işlem

Adımlarınız çalıştırmak için bir Azure Machine Learning işlem oluşturabilirsiniz.

```python
from azureml.core.compute import ComputeTarget, AmlCompute

compute_name = "aml-compute"
 if compute_name in ws.compute_targets:
    compute_target = ws.compute_targets[compute_name]
    if compute_target and type(compute_target) is AmlCompute:
        print('Found compute target: ' + compute_name)
else:
    print('Creating a new compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(vm_size = vm_size, # NC6 is GPU-enabled
                                                                min_nodes = 1, 
                                                                max_nodes = 4)
     # create the compute target
    compute_target = ComputeTarget.create(ws, compute_name, provisioning_config)
    
    # Can poll for a minimum number of nodes and for a specific timeout. 
    # If no min node count is provided it will use the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
     # For a more detailed view of current cluster status, use the 'status' property    
    print(compute_target.status.serialize())
```

### <a id="databricks"></a>Azure Databricks

Azure Databricks, Azure bulutta Apache Spark tabanlı bir ortam olan. Bir Azure Machine Learning işlem hattı ile işlem hedefi olarak kullanılabilir.

Kullanmadan önce bir Azure Databricks çalışma alanı oluşturun. Bu kaynak oluşturmak için bkz [Azure Databricks'te Spark işini çalıştırma](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal) belge.

Azure Databricks işlem hedefi olarak eklemek için aşağıdaki bilgileri sağlayın:

* __Databricks işlem adı__: Bu işlem kaynağına atamak istediğiniz adı.
* __Databricks çalışma alanı adı__: Azure Databricks çalışma alanı adı.
* __Databricks erişim belirteci__: Azure Databricks için kimliğini doğrulamak için kullanılan erişim belirteci. Bir erişim belirteci oluşturmak için bkz: [kimlik doğrulaması](https://docs.azuredatabricks.net/api/latest/authentication.html) belge.

Aşağıdaki kod, Azure Databricks, Azure Machine Learning SDK'sı ile bir işlem hedefi olarak eklemek gösterilmektedir:

```python
import os
from azureml.core.compute import ComputeTarget, DatabricksCompute
from azureml.exceptions import ComputeTargetException

databricks_compute_name = os.environ.get("AML_DATABRICKS_COMPUTE_NAME", "<databricks_compute_name>")
databricks_workspace_name = os.environ.get("AML_DATABRICKS_WORKSPACE", "<databricks_workspace_name>")
databricks_resource_group = os.environ.get("AML_DATABRICKS_RESOURCE_GROUP", "<databricks_resource_group>")
databricks_access_token = os.environ.get("AML_DATABRICKS_ACCESS_TOKEN", "<databricks_access_token>")

try:
    databricks_compute = ComputeTarget(workspace=ws, name=databricks_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('databricks_compute_name {}'.format(databricks_compute_name))
    print('databricks_workspace_name {}'.format(databricks_workspace_name))
    print('databricks_access_token {}'.format(databricks_access_token))

    # Create attach config
    attach_config = DatabricksCompute.attach_configuration(resource_group = databricks_resource_group,
                                                           workspace_name = databricks_workspace_name,
                                                           access_token = databricks_access_token)
    databricks_compute = ComputeTarget.attach(
             ws,
             databricks_compute_name,
             attach_config
         )
    
    databricks_compute.wait_for_completion(True)
```
### <a id="adla"></a>Azure Data Lake Analytics'i

Azure Data Lake Analytics, Azure bulutta büyük veri analiz platformudur. Bir Azure Machine Learning işlem hattı ile işlem hedefi olarak kullanılabilir.

Kullanmadan önce bir Azure Data Lake Analytics hesabı oluşturun. Bu kaynak oluşturmak için bkz [Azure Data Lake Analytics ile çalışmaya başlama](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-portal) belge.

Data Lake Analytics işlem hedefi olarak eklemek için Azure Machine Learning SDK'yı kullanın ve aşağıdaki bilgileri sağlayın:

* __İşlem adı__: Bu işlem kaynağına atamak istediğiniz adı.
* __Kaynak grubu__: Data Lake Analytics hesabını içeren kaynak grubu.
* __Hesap adı__: Data Lake Analytics hesap adı.

Aşağıdaki kod, Data Lake Analytics işlem hedefi olarak eklemek gösterilmektedir:

```python
import os
from azureml.core.compute import ComputeTarget, AdlaCompute
from azureml.exceptions import ComputeTargetException


adla_compute_name = os.environ.get("AML_ADLA_COMPUTE_NAME", "<adla_compute_name>")
adla_resource_group = os.environ.get("AML_ADLA_RESOURCE_GROUP", "<adla_resource_group>")
adla_account_name = os.environ.get("AML_ADLA_ACCOUNT_NAME", "<adla_account_name>")

try:
    adla_compute = ComputeTarget(workspace=ws, name=adla_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('adla_compute_name {}'.format(adla_compute_name))
    print('adla_resource_id {}'.format(adla_resource_group))
    print('adla_account_name {}'.format(adla_account_name))
    # create attach config
    attach_config = AdlaCompute.attach_configuration(resource_group = adla_resource_group,
                                                     account_name = adla_account_name)
    # Attach ADLA
    adla_compute = ComputeTarget.attach(
             ws,
             adla_compute_name,
             attach_config
         )
    
    adla_compute.wait_for_completion(True)
```

> [!TIP]
> Azure Machine Learning işlem hatlarını, yalnızca varsayılan Data Lake Analytics hesabı veri deposunda depolanan verilerle çalışabilirsiniz. Veri gerekirse çalışma varsayılandan farklı bir depoda, kullanabilirsiniz bir [ `DataTransferStep` ](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) eğitim önce verileri kopyalamak için.

## <a id="steps"></a>İşlem hattı adımlarınızı oluşturun

Oluşturma ve bir işlem hedefine çalışma alanınıza eklemek sonra bir işlem hattı adımı tanımlamak hazır olursunuz. Azure Machine Learning SDK üzerinden sunulan birçok yerleşik adım vardır. Bu adımlar temel bir en iyi bir [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py), belirtilen işlem hedefte bir Python betiği çalıştırır:

```python
trainStep = PythonScriptStep(
    script_name="train.py",
    arguments=["--input", blob_input_data, "--output", processed_data1],
    inputs=[blob_input_data],
    outputs=[processed_data1],
    compute_target=compute_target,
    source_directory=project_folder
)
```

Adımlarınız tanımladıktan sonra bazılarını veya tümünü bu adımları kullanarak işlem hattını oluşturun.

> [!NOTE]
> Adımları tanımlayın veya derleme işlem hattı yok dosyasını veya verileri Azure Machine Learning hizmeti ile yüklenir.

```python
# list of steps to run
compareModels = [trainStep, extractStep, compareStep]

# Build the pipeline
pipeline1 = Pipeline(workspace=ws, steps=[compareModels])
```

Aşağıdaki örnek, daha önce oluşturduğunuz Azure Databricks işlem hedefini kullanır: 

```python
dbStep = DatabricksStep(
    name="databricksmodule",
    inputs=[step_1_input],
    outputs=[step_1_output],
    num_workers=1,
    notebook_path=notebook_path,
    notebook_params={'myparam': 'testparam'},
    run_name='demo run name',
    compute_target=databricks_compute,
    allow_reuse=False
)
# List of steps to run
steps = [dbStep]

# Build the pipeline
pipeline1 = Pipeline(workspace=ws, steps=steps)
```

Daha fazla bilgi için [azure işlem hattı adımları paket](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py) ve [sınıfı'işlem hattı](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline%28class%29?view=azure-ml-py) başvuru.

## <a name="submit-the-pipeline"></a>İşlem hattı gönderin

İşlem hattı gönderdiğinizde, Azure Machine Learning hizmeti her adım için bağımlılıkları denetler ve belirtilen kaynak dizini anlık görüntüsünü yükler. Kaynak dizin belirtilirse, geçerli yerel dizine yüklenir. Anlık görüntü, ayrıca çalışma alanınızda denemeler bir parçası olarak depolanır.

> [!IMPORTANT]
> Anlık görüntüde bulunan dosyaların önlemek için oluşturma bir [.gitignore](https://git-scm.com/docs/gitignore) veya `.amlignore` dosya dizin ve dosyaları ekleyin. `.amlignore` Dosyası aynı sözdizimini kullanır ve olarak desenleri [.gitignore](https://git-scm.com/docs/gitignore) dosya. Her iki dosya varsa, `.amlignore` dosya önceliklidir.
>
> Daha fazla bilgi için [anlık görüntüleri](concept-azure-machine-learning-architecture.md#snapshot).

```python
# Submit the pipeline to be run
pipeline_run1 = Experiment(ws, 'Compare_Models_Exp').submit(pipeline1)
pipeline_run1.wait_for_completion()
```

İlk işlem hattı, Azure Machine Learning çalıştırıldığında:

* Proje anlık görüntü, çalışma alanı ile ilişkili Blob depolamadan işlem hedefine indirir.
* İşlem hattı her adımda karşılık gelen bir Docker görüntüsü oluşturur.
* Her adım için docker görüntüsünü, kapsayıcı kayıt defterinden işlem hedefine indirir.
* Veri deposu, bağlar bir `DataReference` nesnesi bir adımda belirtildi. Bağlama desteklenmiyorsa, veri işlem hedefine bunun yerine kopyalanır.
* Adım adım tanımında belirtilen işlem hedefini çalıştırır. 
* Günlüklerine, stdout ve stderr, ölçümleri ve adımı tarafından belirtilen çıkış gibi yapılarını oluşturur. Bu yapılar ardından karşıya ve kullanıcının varsayılan veri deposunda saklanır.

![Bir deney bir işlem hattı çalıştırma diyagramı](./media/how-to-create-your-first-pipeline/run_an_experiment_as_a_pipeline.png)

Daha fazla bilgi için [deneme sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py) başvuru.

## <a name="publish-a-pipeline"></a>Bir işlem hattı yayımlama

Bir işlem hattı çalıştırma için farklı girişlerle daha sonra yeniden yayımlayabilirsiniz. REST uç noktası parametreler kabul etmek için önce yayımlanmış bir Ardışık düzenin için yayımlamadan önce işlem hattı parametreleştirin gerekir. 

1. Bir işlem hattı parametresi oluşturmak için bir [PipelineParameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.pipelineparameter?view=azure-ml-py) varsayılan değeri olan nesne.

   ```python
   pipeline_param = PipelineParameter(
     name="pipeline_arg", 
     default_value=10)
   ```

2. Bu ekleme `PipelineParameter` gibi herhangi bir işlem hattı'ndaki adımları bir parametre olarak nesne:

   ```python
   compareStep = PythonScriptStep(
     script_name="compare.py",
     arguments=["--comp_data1", comp_data1, "--comp_data2", comp_data2, "--output_data", out_data3, "--param1", pipeline_param],
     inputs=[ comp_data1, comp_data2],
     outputs=[out_data3],    
     target=compute_target, 
     source_directory=project_folder)
   ```

3. Çağrıldığında bir parametre kabul eder Bu işlem hattı yayımlayın.

   ```python
   published_pipeline1 = pipeline1.publish(
       name="My_Published_Pipeline", 
       description="My Published Pipeline Description")
   ```

## <a name="run-a-published-pipeline"></a>Yayımlanan bir işlem hattı çalıştırma

Tüm yayınlanan işlem hatları bir REST uç noktası vardır. Bu uç nokta, işlem hattının çalıştırma Python olmayan istemciler gibi dış sistemlerden çağırır. Bu uç nokta "Puanlama ve senaryoları yeniden eğitme batch'de yinelenebilirliği yönetilen" sağlar.

Önceki işlem hattının çalıştırma başlatmak için bir Azure Active Directory kimlik doğrulama üst bilgisi belirteç açıklandığı ihtiyacınız [AzureCliAuthentication sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.azurecliauthentication?view=azure-ml-py) ya da daha fazla bilgi edinin [Azure makinede kimlik doğrulaması Öğrenme](https://aka.ms/pl-restep-auth) dizüstü bilgisayar.

```python
response = requests.post(published_pipeline1.endpoint, 
    headers=aad_token, 
    json={"ExperimentName": "My_Pipeline",
        "ParameterAssignments": {"pipeline_arg": 20}})
```

## <a name="view-results"></a>Sonuçları görüntüleme

Tüm işlem hatlarınızı ve çalıştırma ayrıntıları listesine bakın:
1. [Azure Portal](https://portal.azure.com/) oturum açın.  

1. [Çalışma alanınızı görüntülemek](how-to-manage-workspace.md#view) işlem hatları listesinde bulunamadı.
 ![machine learning işlem hatlarını listesi](./media/how-to-create-your-first-pipeline/list_of_pipelines.png)
 
1. Çalıştırma sonuçları görmek için belirli bir işlem hattını seçin.

## <a name="caching--reuse"></a>Önbelleğe alma ve yeniden kullanma  

En iyi duruma getirmek ve işlem hatlarınızı davranışını özelleştirmek için önbelleğe alma etrafında birkaç şeyi yapmak ve yeniden kullanabilirsiniz. Örneğin, şunlar için seçim yapabilirsiniz:
+ **Çıkış adımdan varsayılan kullanılmasını devre dışı kapatma** ayarlayarak `allow_reuse=False` sırasında [adım tanımı](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py)
+ **Betik karma genişletmek**, mutlak bir yol veya başka dosya ve dizinleri kullanma kaynak_dizin göreli yolları da eklemek için `hash_paths=['<file or directory']` 
+ **Çıkış anahtarınızın yeniden oluşturulması çalıştırmada tüm adımlar için zorlama** ile `pipeline_run = exp.submit(pipeline, regenerate_outputs=False)`

Varsayılan olarak adımı yeniden kullanımı etkindir ve ana komut dosyası yalnızca karma. Bunu, belirli bir adım için komut dosyası aynı kalırsa (`script_name`, giriş ve parametreleri), bir önceki adımdan çıktısı yeniden, proje için işlem gönderilmeyen ve sonuçları önceki çalıştırmaya bunun yerine bir sonraki adıma hemen kullanılabilir .  

```python
step = PythonScriptStep(name="Hello World", 
                        script_name="hello_world.py",  
                        compute_target=aml_compute,  
                        source_directory= source_directory, 
                        allow_reuse=False, 
                        hash_paths=['hello_world.ipynb']) 
```
 

## <a name="next-steps"></a>Sonraki adımlar
- Kullanım [github'da bu Jupyter not defterlerini](https://aka.ms/aml-pipeline-readme) machine learning işlem hatlarını daha fazla araştırmak için.
- SDK başvurusu Yardım konularını okuyun [azureml işlem hatları çekirdek](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) paket ve [azureml işlem hatları adımları](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py) paket.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]
