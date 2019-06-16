---
title: Büyük veriler üzerinde toplu Öngörüler çalıştırın
titleSuffix: Azure Machine Learning service
description: Batch zaman uyumsuz olarak büyük miktarlarda verileri Azure Machine Learning hizmetini kullanarak tahminlerde bulunmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens, garye
ms.author: jordane
author: jpe316
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 1e403ac0d2fbe9572a44fb3cde9d25e4df9b3db4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60818514"
---
# <a name="run-batch-predictions-on-large-data-sets-with-azure-machine-learning-service"></a>Azure Machine Learning hizmeti ile büyük veri kümelerinde toplu Öngörüler çalıştırma

Bu makalede, büyük miktarlarda veri çubuğunda zaman uyumsuz olarak Azure Machine Learning hizmetini kullanarak tahminlerde bulunmayı öğreneceksiniz.

Batch tahmin (veya toplu Puanlama) ile eşsiz bir aktarım hızı zaman uyumsuz uygulamalar için uygun maliyetli çıkarımı sağlar. Toplu tahmin işlem hatlarını çıkarımı terabaytlarca üretim veri gerçekleştirmek için ölçeklendirilebilir. Batch tahmin, yüksek aktarım hızı, büyük bir veri koleksiyonu için Başlat ve unut Öngörüler için optimize edilmiştir.

>[!TIP]
> Sisteminiz düşük gecikmeli işleme (tek bir belge veya küçük bir belge kümesini hızlı bir şekilde işlemek için) gerektiriyorsa, kullanın [gerçek zamanlı Puanlama](how-to-consume-web-service.md) batch tahmin yerine.

Aşağıdaki adımlarda, oluşturduğunuz bir [makine öğrenimi işlem hattı](concept-ml-pipelines.md) bir kullanan bir görüntü işleme modelinizle kaydetmek için ([başlangıcı V3](https://arxiv.org/abs/1512.00567)). Ardından Azure Blob Depolama hesabınızda kullanılabilen görüntülerindeki Puanlama toplu pretrained modeli kullanır. Puanlama için kullanılan görüntülerin etiketlenmemiş gelen görüntüleri [Imagenet](http://image-net.org/) veri kümesi.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree).

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

Aşağıdaki adımlar bir işlem hattını çalıştırmak için gereken kaynakları ayarlayın:

- Modeli kullanan, giriş etiketleri ve puanlamak için görüntüleri olan veri deposuna erişim (Bu zaten sizin için ayarlanır).
- Bir veri deposu, çıktılarının depolanması için ayarlayın.
- Yapılandırma `DataReference` önceki veri depoları verilerde işaret edecek şekilde nesneleri.
- İşlem hattı adımları çalıştıracağınız işlem makineler veya kümeleri ayarlayın.

### <a name="access-the-datastores"></a>Erişim veri depoları

İlk olarak, model, etiketler ve görüntüleri olan veri deposuna erişin.

Adlı bir ortak blob kapsayıcısı kullanacağınız *sampledata*, *pipelinedata* görüntüleri Imagenet değerlendirme kümesinden tutan hesabı. Bu ortak kapsayıcı için veri deposu adı *images_datastore*. Bu veri deposu ile çalışma alanınızı kaydedin:

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

Sonraki, varsayılan veri deposu çıktıların kullanacak şekilde ayarlanmış.

Çalışma Alanınızı oluştururken [Azure dosyaları](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) ve [Blob Depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) varsayılan çalışma alanına eklenir. Azure dosyaları, bir çalışma alanı için varsayılan veri deposu olduğu halde Blob depolamayı bir veri deposu olarak kullanabilirsiniz. Daha fazla bilgi için [Azure Depolama Seçenekleri](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks).

```python
def_data_store = ws.get_default_datastore()
```

### <a name="configure-data-references"></a>Veri başvuruları yapılandırın

Artık, işlem hattınızdaki veri işlem hattı adımları girdi olarak başvuru.

Bir veri kaynağında bir işlem hattı tarafından temsil edilen bir [DataReference](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference) nesne.  `DataReference` Kendini ya da bir veri deposu, erişilebilir bir veri nesnesi işaret eder. Gereksinim duyduğunuz `DataReference`  nesneler için girdi görüntülerini, pretrained modeli depolanan dizini kullanılacak dizini etiketleri için dizin ve çıkış dizini.

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

Azure Machine Learning içinde *işlem* (veya *hedef işlem*) makineleri ya da machine learning işlem hattı, hesaplama adımları kümeleri anlamına gelir. Örneğin, oluşturabileceğiniz bir `Azure Machine Learning compute`.

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

Kullanan görüntü işleme modelinizle (InceptionV3) indirmesine <http://download.tensorflow.org/models/inception_v3_2016_08_28.tar.gz>. Ardından ayıklayın `models` alt.

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

Modeli kaydetmeyi nasıl aşağıda verilmiştir:

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
>Aşağıdaki kod içinde bulunan, yalnızca bir örnektir [batch_score.py](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/pipeline-batch-scoring/batch_scoring.py) tarafından kullanılan [örnek not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/pipeline-batch-scoring/pipeline-batch-scoring.ipynb). Senaryonuz için kendi Puanlama betiği oluşturmanız gerekir.

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

Betiği için conda bağımlılıklarını belirtin. İşlem hattı adım oluşturduğunuzda, daha sonra bu nesne gerekir.

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

Bir işlem hattı parametresini kullanarak oluşturma bir [PipelineParameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.pipelineparameter?view=azure-ml-py) varsayılan değeri olan nesne.

```python
batch_size_param = PipelineParameter(
                    name="param_batch_size", 
                    default_value=20)
```

### <a name="create-the-pipeline-step"></a>İşlem hattı adım oluşturma

İşlem hattı adım, betik, ortamı yapılandırması ve parametreleri kullanarak oluşturun. Betik yürütme hedefi olarak zaten çalışma alanınıza bağlı işlem hedefini belirtin. Kullanım [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py) işlem hattı adımı oluşturmak.

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

Artık bir işlem hattı çalıştırma ve oluşturulan çıktıyı inceleyin. Çıktı, her giriş görüntüsüne karşılık gelen bir puanı vardır.

```python
# Run the pipeline
pipeline = Pipeline(workspace=ws, steps=[batch_score_step])
pipeline_run = Experiment(ws, 'batch_scoring').submit(pipeline, pipeline_params={"param_batch_size": 20})

# Wait for the run to finish (this might take several minutes)
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

Çalıştırma sonucunu yaptıktan sonra farklı giriş değerlerle daha sonra çalıştırabilmeniz için işlem hattı yayımlayın. Bir işlem hattı yayımladığınızda, bir REST uç noktasını alın. Bu uç nokta kabul eder, zaten dahil kullanarak parametre kümesi ile işlem hattı yürütmesini [PipelineParameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.pipelineparameter?view=azure-ml-py).

```python
published_pipeline = pipeline_run.publish_pipeline(
    name="Inception_v3_scoring", 
    description="Batch scoring using Inception v3 model", 
    version="1.0")
```

## <a name="rerun-the-pipeline-by-using-the-rest-endpoint"></a>REST uç noktasını kullanarak işlem hattını yeniden çalıştırın

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

Bu çalışma için uçtan uca görmek için toplu işlem Puanlama not defterinde deneyin [GitHub](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines). 

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

