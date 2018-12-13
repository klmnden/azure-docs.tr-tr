---
title: Oluşturun, çalıştırın ve ML işlem hatlarını izleyin
titleSuffix: Azure Machine Learning service
description: Oluşturun ve Python için Azure Machine Learning SDK'sı ile işlem hattı öğrenme bir makine çalıştırın.  İşlem hatları oluşturmak ve bu veri hazırlama, model eğitiminin, model dağıtımı ve çıkarım gibi Birleştir birlikte machine learning (ML) aşamaları iş akışları yönetmek için kullanılır.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.author: sanpil
author: sanpil
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 8478b6760921f4641cd214b1ff19cae9757b6d7e
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53269054"
---
# <a name="create-and-run-a-machine-learning-pipeline-using-azure-machine-learning-sdk"></a>Oluşturma ve Azure Machine Learning SDK'sını kullanarak bir makine öğrenimi işlem hattı çalıştırma

Bu makalede, oluşturmak, yayımlamak, çalıştırmak ve izlemek öğrenin bir [makine öğrenimi işlem hattı](concept-ml-pipelines.md) kullanarak [Azure Machine Learning SDK'sı](https://aka.ms/aml-sdk).  Bu komut zincirleri oluşturmak ve çeşitli makine öğrenme aşamaları birleştirmek iş akışlarını yönetme yardımcı olur. Veri hazırlama ve modeli eğitimi gibi bir Ardışık düzenin her aşaması, bir veya daha fazla adım ekleyebilirsiniz.

Oluşturduğunuz işlem hattı, Azure Machine Learning hizmeti üyelerine görünür [çalışma](how-to-manage-workspace.md). 

İşlem hatları, hesaplama ve bu işlem hattı çalıştırmasıyla ilişkili Ara ve son veri depolama için uzak işlem hedefleri kullanın.  İşlem hatları okuyabilir ve yazma veri ve desteklenen [Azure depolama](https://docs.microsoft.com/azure/storage/) konumları.

>[!Note]
>Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](http://aka.ms/AMLFree) bugün.

## <a name="prerequisites"></a>Önkoşullar

* [Geliştirme ortamınızı yapılandırma](how-to-configure-environment.md) Azure Machine Learning SDK'sını yüklemek için.

* Oluşturma bir [Azure Machine Learning çalışma alanı](how-to-configure-environment.md#workspace) tüm işlem hattı kaynakları tutmak için. 

 ```python
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

* Yapılandırma bir `DataReference` kendini veya bir veri deposunda erişilebilir veriler işaret edecek şekilde nesne.

* Ayarlanan [hedefleri işlem](concept-azure-machine-learning-architecture.md#compute-target) işlem hattı adımlarınızı çalıştırılacağı.

### <a name="set-up-a-datastore"></a>Bir veri deposu ayarlayın
Bir veri deposuna erişmek işlem hattının verileri depolar.  Her bir çalışma alanı bir varsayılan veri deposu sahiptir. Ek veri depoları kaydedebilirsiniz. 

Çalışma Alanınızı oluştururken bir [Azure dosya depolama](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) ve [blob depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) varsayılan çalışma alanına eklenir.  Azure dosya depolama için bir çalışma alanı için "varsayılan datastore" olsa da, blob depolama bir veri deposu olarak kullanabilirsiniz.  Daha fazla bilgi edinin [Azure Depolama Seçenekleri](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks). 

```python
# Default datastore (Azure file storage)
def_data_store = ws.get_default_datastore() 

# The above call is equivalent to this 
def_data_store = Datastore(ws, "workspacefilestore")

# Get blob storage associated with the workspace
def_blob_store = Datastore(ws, "workspaceblobstore")
```

Bunlar işlem hatlarınızı erişilebilir olması için veri deposu, veri dosyaları veya dizinleri yükleyin.  Bu örnek, blob depolama veri deposu sürümü kullanır:

```python
def_blob_store.upload_files(
    ["./data/20news.pkl"],
    target_path="20newsgroups", 
    overwrite=True)
```

Bir işlem hattı, bir veya daha fazla adımdan oluşur.  Bir adım, bir işlem hedefte çalıştırma bir birimdir.  Adımlar veri kaynaklarını ve "Ara" veri üretir. Bir adım, veri modeli, modeli ve bağımlı dosyaları ile bir dizin veya geçici veri oluşturabilirsiniz.  Bu veriler daha sonra ardışık düzende sonraki diğer adımları için kullanılabilir.

### <a name="configure-data-reference"></a>Veri başvurusu yapılandırma

Başvurulan veri kaynağı bir işlem hattı, bir adım bir giriş olarak oluşturduğunuz. Bir veri kaynağında bir işlem hattı tarafından temsil edilen bir [DataReference](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference) nesne. `DataReference` Nesne işaret kendini veya bir veri deposundan erişilebilir veriler.

```python
blob_input_data = DataReference(
    datastore=def_blob_store,
    data_reference_name="test_data",
    path_on_datastore="20newsgroups/20news.pkl")
```

Ara veriler (veya bir adımın çıkış) tarafından temsil edilen bir [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py) nesne. `output_data1` bir adımın çıktı olarak bir oluşturulur ve bir veya daha fazla gelecekteki adımları girdisi olarak kullanılır.  `PipelineData` adımlar arasında veri bağımlılığı tanıtır ve işlem hattı, bir örtük yürütme sırası oluşturur.

```python
output_data1 = PipelineData(
    "output_data1",
    datastore=def_blob_store,
    output_name="output_data1")
```

### <a name="set-up-compute"></a>Hesaplamayı ayarlayın

Azure Machine Learning işlem (ya da işlem hedefi) makine ya da machine learning işlem hattı, hesaplama adımları gerçekleştirecektir kümeleri ifade eder. Örneğin, bir Azure Machine Learning işlem adımlarınızı çalıştırmak için oluşturabilirsiniz.

```python
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

## <a name="construct-your-pipeline-steps"></a>İşlem hattı adımlarınızı oluşturun

Bir işlem hattı adımı tanımlamak hazırsınız. Azure Machine Learning SDK üzerinden sunulan birçok yerleşik adım vardır. Bu adımlar temel bir en iyi bir `PythonScriptStep` , belirtilen işlem hedefte Python betiğini yürütür.

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

>[!NOTE]
>Adımları tanımlayın veya derleme işlem hattı hiçbir dosya veya veri Azure Machine Learning hizmeti ile yüklenir.

```python
# list of steps to run
compareModels = [trainStep, extractStep, compareStep]

# Build the pipeline
pipeline1 = Pipeline(workspace=ws, steps=[compareModels])
```

## <a name="submit-the-pipeline"></a>İşlem hattı gönderin

İşlem hattı gönderdiğinizde, bağımlılıkları her adım ve Azure Machine Learning hizmeti için kaynak dizini karşıya yüklenmiş olarak belirtilen klasörün bir anlık görüntü için denetlenir.  Kaynak dizin belirtilirse, geçerli yerel dizine yüklenir.

```python
# Submit the pipeline to be run
pipeline_run1 = Experiment(ws, 'Compare_Models_Exp').submit(pipeline1)
```

İlk kez bir işlem hattı çalıştırdığınızda:

* Proje anlık görüntüsü işlem hedef çalışma alanı ile ilişkili blob depolama alanından indirilir.
* İşlem hattı her adımda karşılık gelen bir docker görüntüsü oluşturulur.
* Her adım için docker görüntüsünü, kapsayıcı kayıt defterinden işlem hedefine indirilir.
* Varsa bir `DataReference` nesne veri deposuna bağlanan bir adımda belirtilir. Bağlama desteklenmiyorsa, veri işlem hedefine bunun yerine kopyalanır.
* Adım adım tanımında belirtilen işlem hedefini çalıştırır. 
* Yapılar günlüklerine, stdout ve stderr, ölçümleri ve adımı tarafından belirtilen çıkış gibi oluşturulur. Bu yapılar ardından karşıya ve kullanıcının varsayılan veri deposunda saklanır.

![bir deney bir işlem hattı çalıştırma](./media/how-to-create-your-first-pipeline/run_an_experiment_as_a_pipeline.png)

## <a name="publish-a-pipeline"></a>Bir işlem hattı yayımlama

Bir işlem hattı çalıştırma için farklı girişlerle daha sonra yeniden yayımlayabilirsiniz. REST uç noktası parametreler kabul etmek için önce yayımlanmış bir işlem hattı için işlem hattı yayımlanmadan önce parametreli gerekir. 

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

Yayımlanan tüm işlem hatları, işlem hattının çalıştırma Python olmayan istemciler gibi dış sistemlerden çağrılacak REST uç noktası vardır. Bu uç nokta "yönetilen yinelenebilirliği" toplu Puanlama ve yeniden eğitme senaryoları için bir yol sağlar.

Önceki işlem hattının çalıştırma başlatmak için bir Azure Active Directory kimlik doğrulama üst bilgisi belirteç açıklandığı ihtiyacınız [AzureCliAuthentication sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.azurecliauthentication?view=azure-ml-py)

```python
response = requests.post(published_pipeline1.endpoint, 
    headers=aad_token, 
    json={"ExperimentName": "My_Pipeline",
        "ParameterAssignments": {"pipeline_arg": 20}})
```
## <a name="view-results"></a>Sonuçları görüntüleme

Tüm işlem hatlarınızı ve çalıştırma ayrıntıları listesine bakın:
1. [Azure Portal](https://portal.azure.com/) oturum açın.  

1. [Çalışma alanınızı görüntülemek](how-to-manage-workspace.md#view-a-workspace) işlem hatları listesinde bulunamadı.
 ![machine learning işlem hatlarını listesi](./media/how-to-create-your-first-pipeline/list_of_pipelines.png)
 
1. Çalıştırma sonuçları görmek için belirli bir işlem hattını seçin.

## <a name="next-steps"></a>Sonraki adımlar
- Kullanım [github'da bu Jupyter not defterlerini](https://aka.ms/aml-pipeline-readme) machine learning işlem hatlarını daha fazla araştırmak için.
- SDK başvurusu Yardım konularını okuyun [azureml işlem hatları çekirdek](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) paket ve [azureml işlem hatları adımları](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py) paket.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]
