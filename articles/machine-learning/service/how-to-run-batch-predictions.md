---
title: Büyük veriler üzerinde toplu Öngörüler çalıştırın
titleSuffix: Azure Machine Learning service
description: Batch zaman uyumsuz olarak büyük miktarlarda verileri Azure Machine Learning hizmetini kullanarak tahminlerde bulunmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens, garye
ms.author: jordane
author: jpe316
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: a711b80471da0677c5e2d0dd0ee5e371e5a16f75
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53268658"
---
# <a name="run-batch-predictions-on-large-data-sets-with-azure-machine-learning-service"></a>Azure Machine Learning hizmeti ile büyük veri kümelerinde toplu Öngörüler çalıştırma

Bu makalede, hızlı ve verimli bir şekilde büyük miktarda zaman uyumsuz olarak Azure Machine Learning hizmetini kullanarak veri tahminlerde bulunmayı öğreneceksiniz.

Batch tahmin (veya toplu Puanlama) uygun maliyetli çıkarımı eşsiz aktarım hızı ile zaman uyumsuz uygulamalar için sağlar. Toplu tahmin işlem hatlarını çıkarımı terabaytlarca üretim veri gerçekleştirmek için ölçeklendirilebilir. Batch tahmin, yüksek aktarım hızı, büyük bir veri koleksiyonu için Öngörüler Başlat ve unut etrafında optimize edilmiştir.

>[!NOTE]
> Sisteminiz düşük gecikmeli işleme (bir tek belge veya küçük ayarlamanız belgeleri hızla işlem) gerektiriyorsa, kullanın [gerçek zamanlı Puanlama](how-to-consume-web-service.md) batch tahmin yerine.

Aşağıdaki adımlarda, oluşturacağınız bir [makine öğrenimi işlem hattı](concept-ml-pipelines.md) bir kullanan bir görüntü işleme modelinizle kaydetmek için ([başlangıcı V3](https://arxiv.org/abs/1512.00567)) ve toplu Puanlama görüntülerinde pretrained modeli kullanın Azure blob hesabınızda kullanılabilir. Puanlama için kullanılan görüntülerin etiketlenmemiş gelen görüntüleri [Imagenet](http://image-net.org/) veri kümesi.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](http://aka.ms/AMLFree) bugün.

- Azure Machine Learning SDK'sını yüklemek için geliştirme ortamınızı yapılandırın. Daha fazla bilgi için [bir geliştirme ortamı yapılandırmak için Azure Machine Learning](how-to-configure-environment.md).

- Tüm işlem hattı kaynakları barındıracak bir Azure Machine Learning çalışma alanı oluşturun. Aşağıdaki kodu kullanın veya daha fazla seçenek için bkz: [çalışma alanı yapılandırma dosyası oluşturma](how-to-configure-environment.md#workspace).

  ```python
  ws = Workspace.create(
     name = '<workspace-name>',
     subscription_id = '<subscription-id>',
     resource_group = '<resource-group>',
     location = '<workspace_region>',
     exist_ok = True)
  ```

## <a name="set-up-machine-learning-resources"></a>Machine learning kaynaklarını ayarlama

Aşağıdaki adımlar, bir işlem hattını çalıştırmak için gereken kaynakları ayarlanır:

- Modeli kullanan, giriş etiketleri ve puanlamak için görüntüleri olan veri deposuna erişim (Bu zaten sizin için ayarlanır).
- Bir veri deposu, çıktılarının depolanması için ayarlayın.
- Önceki veri depoları verilerde işaret edecek şekilde DataReference nesneleri yapılandırın.
- İşlem hattı adımları çalıştıracağınız işlem makineler veya kümeleri ayarlayın.

### <a name="access-the-datastores"></a>Erişim veri depoları

İlk olarak, model, etiketler ve görüntüleri olan veri deposuna erişin.

Adlı bir ortak blob kapsayıcısı kullanacağınız *sampledata* içinde *pipelinedata* görüntüleri Imagenet değerlendirme kümesinden tutan hesabı. Bu ortak kapsayıcı için veri deposu adı *images_datastore*. Bu veri deposu ile çalışma alanınızı kaydedin:

```python
# Public blob container details
account_name = "pipelinedata"
datastore_name="images_datastore"
container_name="sampledata"
 
batchscore_blob = Datastore.register_azure_blob_container(ws,
                      datastore_name=datastore_name,
                      container_name= container_name,
                      account_name=account_name,
                      overwrite=True)
```

Varsayılan veri deposu çıktısı için kullanılacak sonraki, kurulumu.

Çalışma Alanınızı oluştururken bir [Azure dosya deposundan](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) ve [blob depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) varsayılan çalışma alanına eklenir. Azure dosya depolama için bir çalışma alanı için "varsayılan datastore" olsa da, blob depolama bir veri deposu olarak kullanabilirsiniz. Daha fazla bilgi edinin [Azure Depolama Seçenekleri](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks).

```python
def_data_store = ws.get_default_datastore()
```

### <a name="configure-data-references"></a>Veri başvuruları yapılandırın

Artık, işlem hattınızdaki veri işlem hattı adımları girdi olarak başvuru.

Bir veri kaynağında bir işlem hattı tarafından temsil edilen bir [DataReference](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference) nesne. DataReference nesnenin kendini veya bir veri deposundan erişilebilir veriler işaret eder. Girdi görüntülerini, pretrained modeli depolanan directory kullanılan dizin DataReference nesneleri gereksinim etiketleri için dizin ve çıkış dizini.

```python
input_images = DataReference(datastore=batchscore_blob, 
                             data_reference_name="input_images",
                             path_on_datastore="batchscoring/images",
                             mode="download")
                           
model_dir = DataReference(datastore=batchscore_blob, 
                          data_reference_name="input_model",
                          path_on_datastore="batchscoring/models",
                          mode="download")                          
                         
label_dir = DataReference(datastore=batchscore_blob, 
                          data_reference_name="input_labels",
                          path_on_datastore="batchscoring/labels",
                          mode="download")                          
                         
output_dir = PipelineData(name="scores", 
                          datastore=def_data_store, 
                          output_path_on_compute="batchscoring/results")
```

### <a name="set-up-compute-target"></a>İşlem Hedefi ' ayarlayın

Azure Machine Learning işlem (ya da işlem hedefi) makine ya da machine learning işlem hattı, hesaplama adımları gerçekleştirecektir kümeleri ifade eder. Örneğin, oluşturabileceğiniz bir `Azure Machine Learning compute`.

```python
compute_name = "gpucluster"
compute_min_nodes = 0
compute_max_nodes = 4
vm_size = "STANDARD_NC6"

if compute_name in ws.compute_targets:
    compute_target = ws.compute_targets[compute_name]
    if compute_target and type(compute_target) is AmlCompute:
        print('Found compute target. just use it. ' + compute_name)
else:
    print('Creating a new compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(
                     vm_size = vm_size, # NC6 is GPU-enabled
                     vm_priority = 'lowpriority', # optional
                     min_nodes = compute_min_nodes, 
                     max_nodes = compute_max_nodes)

    # create the cluster
    compute_target = ComputeTarget.create(ws, 
                        compute_name, 
                        provisioning_config)
    
    compute_target.wait_for_completion(
                     show_output=True, 
                     min_node_count=None, 
                     timeout_in_minutes=20)
```

## <a name="prepare-the-model"></a>Model hazırlama

Pretrained modeli kullanabilmeniz için model indirin ve çalışma alanınızla kaydetmek gerekir.

### <a name="download-the-pretrained-model"></a>Pretrained modeli indirin.

Kullanan görüntü işleme modelinizle (InceptionV3) indirmesine <http://download.tensorflow.org/models/inception_v3_2016_08_28.tar.gz>. İndirildikten sonra ayıklayın `models` alt.

```python
import os
import tarfile
import urllib.request

model_dir = 'models'
if not os.path.isdir(model_dir):
    os.mkdir(model_dir)

url="http://download.tensorflow.org/models/inception_v3_2016_08_28.tar.gz"
response = urllib.request.urlretrieve(url, "model.tar.gz")
tar = tarfile.open("model.tar.gz", "r:gz")
tar.extractall(model_dir)
```

### <a name="register-the-model"></a>Modeli kaydedin

```python
import shutil
from azureml.core.model import Model

# register downloaded model 
model = Model.register(
        model_path = "models/inception_v3.ckpt",
        model_name = "inception", # This is the name of the registered model
        tags = {'pretrained': "inception"},
        description = "Imagenet trained tensorflow inception",
        workspace = ws)
```

## <a name="write-your-scoring-script"></a>Puanlama betiğinizi yazma

>[!Warning]
>Aşağıdaki kod içinde bulunan, yalnızca bir örnektir [batch_score.py](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline/batch_score.py) tarafından kullanılan [örnek not defteri](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline/pipeline-batch-scoring.ipynb) senaryonuz için Puanlama kendi betiğinizi oluşturmak ihtiyacınız olacak.

`batch_score.py` Betik alır girdi görüntülerini *dataset_path*, kullanan modellerinde *model_dir,* ve çıkaran *sonuçları label.txt* için *output_dir*.

```python
# Snippets from a sample scoring script
# Refer to the accompanying batch-scoring Notebook
# https://github.com/Azure/MachineLearningNotebooks/blob/master/pipeline/pipeline-batch-scoring.ipynb
# for the implementation script

# Get labels
def get_class_label_dict(label_file):
  label = []
  proto_as_ascii_lines = tf.gfile.GFile(label_file).readlines()
  for l in proto_as_ascii_lines:
    label.append(l.rstrip())
  return label

class DataIterator:
  # Definition of the DataIterator here
  
def main(_):
    # Refer to batch-scoring Notebook for implementation.
    label_file_name = os.path.join(args.label_dir, "labels.txt")
    label_dict = get_class_label_dict(label_file_name)
    classes_num = len(label_dict)
    test_feeder = DataIterator(data_dir=args.dataset_path)
    total_size = len(test_feeder.labels)

    # get model from model registry
    model_path = Model.get_model_path(args.model_name)
    with tf.Session() as sess:
        test_images = test_feeder.input_pipeline(batch_size=args.batch_size)
        with slim.arg_scope(inception_v3.inception_v3_arg_scope()):
            input_images = tf.placeholder(tf.float32, [args.batch_size, image_size, image_size, num_channel])
            logits, _ = inception_v3.inception_v3(input_images,
                                                        num_classes=classes_num,
                                                        is_training=False)
            probabilities = tf.argmax(logits, 1)

        sess.run(tf.global_variables_initializer())
        sess.run(tf.local_variables_initializer())
        coord = tf.train.Coordinator()
        threads = tf.train.start_queue_runners(sess=sess, coord=coord)
        saver = tf.train.Saver()
        saver.restore(sess, model_path)
        out_filename = os.path.join(args.output_dir, "result-labels.txt")
            
        # copy the file to artifacts
        shutil.copy(out_filename, "./outputs/")
```

## <a name="build-and-run-the-batch-scoring-pipeline"></a>Derleme ve toplu işlem Puanlama işlem hattı çalıştırma

Derleme işlem hattı, şimdi bir araya getirelim için ihtiyacınız olan her şeye sahip.

### <a name="prepare-the-run-environment"></a>Çalışma ortamı hazırlama

Betiği için conda bağımlılıklarını belirtin. Daha sonra işlem hattı adım oluşturduğunuzda, bu nesne gerekir.

```python
from azureml.core.runconfig import DEFAULT_GPU_IMAGE

cd = CondaDependencies.create(pip_packages=["tensorflow-gpu==1.10.0", "azureml-defaults"])

# Runconfig
amlcompute_run_config = RunConfiguration(conda_dependencies=cd)
amlcompute_run_config.environment.docker.enabled = True
amlcompute_run_config.environment.docker.gpu_support = True
amlcompute_run_config.environment.docker.base_image = DEFAULT_GPU_IMAGE
amlcompute_run_config.environment.spark.precache_packages = False
```

### <a name="specify-the-parameter-for-your-pipeline"></a>İşlem hattınızı parametresi belirtin

Parametresini kullanarak bir işlem hattı oluşturma bir [PipelineParameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.pipelineparameter?view=azure-ml-py) varsayılan değeri olan nesne.

```python
batch_size_param = PipelineParameter(
                    name="param_batch_size", 
                    default_value=20)
```

### <a name="create-the-pipeline-step"></a>İşlem hattı adım oluşturma

Betik, ortamı yapılandırması ve parametreleri kullanarak işlem hattı adım oluşturun. Betik yürütme hedefi olarak zaten çalışma alanınıza bağlı işlem hedefini belirtin. Kullanım [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py) işlem hattı adımı oluşturmak.

```python
inception_model_name = "inception_v3.ckpt"

batch_score_step = PythonScriptStep(
    name="batch_scoring",
    script_name="batch_score.py",
    arguments=["--dataset_path", input_images, 
               "--model_name", "inception",
               "--label_dir", label_dir, 
               "--output_dir", output_dir, 
               "--batch_size", batch_size_param],
    compute_target=compute_target,
    inputs=[input_images, label_dir],
    outputs=[output_dir],
    runconfig=amlcompute_run_config,
    source_directory=scripts_folder
```

### <a name="run-the-pipeline"></a>İşlem hattını çalıştırma

Artık bir işlem hattı çalıştırma ve oluşturulan çıktıyı inceleyin. Çıktı, her giriş görüntüsüne karşılık gelen bir puan sahip olur.

```python
# Run the pipeline
pipeline = Pipeline(workspace=ws, steps=[batch_score_step])
pipeline_run = Experiment(ws, 'batch_scoring').submit(pipeline, pipeline_params={"param_batch_size": 20})

# Wait for the run to finish (this may take several minutes)
pipeline_run.wait_for_completion(show_output=True)

# Download and review the output
step_run = list(pipeline_run.get_children())[0]
step_run.download_file("./outputs/result-labels.txt")

import pandas as pd
df = pd.read_csv("result-labels.txt", delimiter=":", header=None)
df.columns = ["Filename", "Prediction"]
df.head()
```

## <a name="publish-the-pipeline"></a>Yayımlama kanalı

Çalıştırma sonucunu memnun olduğunuzda, farklı giriş değerlerle daha sonra çalıştırabilmeniz için işlem hattı yayımlayın. Bir işlem hattı yayımladığınızda, zaten dahil kullanarak parametre kümesi ile işlem hattı yürütmesini kabul eden bir REST uç noktasını alma [PipelineParameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.pipelineparameter?view=azure-ml-py).

```python
published_pipeline = pipeline_run.publish_pipeline(
    name="Inception_v3_scoring", 
    description="Batch scoring using Inception v3 model", 
    version="1.0")
```

## <a name="rerun-the-pipeline-using-the-rest-endpoint"></a>REST uç noktasını kullanarak işlem hattını yeniden çalıştırın

İşlem hattını yeniden çalıştırmak için bir Azure Active Directory kimlik doğrulama üst bilgisi belirteç açıklandığı ihtiyacınız olacak [AzureCliAuthentication sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.azurecliauthentication?view=azure-ml-py).

```python
from azureml.pipeline.core import PublishedPipeline

rest_endpoint = published_pipeline.endpoint
# specify batch size when running the pipeline
response = requests.post(rest_endpoint, 
        headers=aad_token, 
        json={"ExperimentName": "batch_scoring",
               "ParameterAssignments": {"param_batch_size": 50}})

# Monitor the run
from azureml.pipeline.core.run import PipelineRun
published_pipeline_run = PipelineRun(ws.experiments["batch_scoring"], run_id)

RunDetails(published_pipeline_run).show()
```

## <a name="next-steps"></a>Sonraki adımlar

Bu çalışma için uçtan uca görmek için toplu işlem Puanlama not defterinde deneyin ([how-to-use-azureml/machine-learning-pipelines](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines). 

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

