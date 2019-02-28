---
title: Azure Batch AI neler oluyor? | Microsoft Docs
description: Azure Batch AI ve Azure Machine Learning hizmetinin işlem seçeneği için neler olduğu hakkında bilgi edinin.
services: batch-ai
author: garyericson
ms.service: batch-ai
ms.topic: overview
ms.date: 2/14/2019
ms.author: garye
ms.openlocfilehash: fb1114e94c227ce6787532c6059186399d0f57f0
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56961372"
---
# <a name="whats-happening-to-azure-batch-ai"></a>Azure Batch AI neler oluyor?

**Azure Batch AI hizmeti Mart ayında devre dışı bırakılıyor.** Ölçekli eğitim ve puanlama Batch AI yeteneklerini de kullanıma sunulmuştur [Azure Machine Learning hizmeti](../machine-learning/service/overview-what-is-azure-ml.md), hangi genel olarak kullanılabilir hale 4 Aralık 2018'de.

Makine öğrenimi özellikleri birçok diğer yanı sıra, Azure Machine Learning hizmeti, eğitim, dağıtma ve makine öğrenimi modellerini Puanlama için yönetilen bir bulut tabanlı bilgi işlem hedef içerir. Bu işlem hedef adlı [Azure Machine Learning işlem](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute). [Geçiş ve bugün kullanmaya başlayın](#migrate). Azure Machine Learning hizmeti aracılığıyla etkileşim kurabilir, [Python SDK'ları](../machine-learning/service/quickstart-create-workspace-with-python.md), komut satırı arabirimi ve [Azure portalında](../machine-learning/service/quickstart-get-started.md).

## <a name="support-timeline"></a>Destek zaman çizelgesi

| Tarih | Batch AI hizmeti destek ayrıntıları |
| ---- |-----------------|
| Aralık&nbsp;14&#x2c;&nbsp;2018| Önce mevcut Azure Batch AI abonelikler olarak kullanabilirsiniz. Ancak, hiçbir **yeni abonelikler** mümkün olan ve hiçbir yeni yatırım yapılan.|
| March&nbsp;31&#x2c;&nbsp;2019 | Bu tarihten sonra mevcut Batch AI abonelikleri artık çalışmayacak. |

## <a name="how-does-azure-machine-learning-service-compare"></a>Azure Machine Learning hizmeti nasıl arasındaki fark nedir?
Bu eğitim, dağıtma, otomatikleştirin ve tüm bulut sunuyor Geniş ölçekte makine öğrenimi modelleri yönetmek için kullandığınız bir bulut hizmetidir. Bir üst düzey anlayın [bu genel bakışta Azure Machine Learning hizmetinde](../machine-learning/service/overview-what-is-azure-ml.md).
 

Veri hazırlama, eğitim ve deneme ve dağıtım aşaması tipik model geliştirme yaşam döngüsünü kapsar. Machine Learning işlem hatlarını kullanarak bu uçtan uca döngüsü düzenlenmiş.

![Akış Diyagramı](./media/overview-what-happened-batch-ai/lifecycle.png)


[Hizmetinin nasıl çalıştığı ve ana kavramları hakkında daha fazla bilgi](../machine-learning/service/concept-azure-machine-learning-architecture.md). Batch AI mevcut kavramları model eğitim iş akışında kavramlardan bir çoğunu benzerdir. 

Özellikle, bunlar hakkında nasıl almalısınız, eşleme şu şekildedir:
 
|Batch AI hizmeti|  Azure Machine Learning hizmeti|
|:--:|:---:|
|Çalışma alanı|Çalışma alanı|
|Küme|   İşlem türü `AmlCompute`|
|Dosya sunucuları|Veri depoları|
|Denemeler|Denemeler|
|İşler|Çalıştıran (iç içe geçmiş çalıştırmaları izin verir)|
 
Daha fazla şey görselleştirmenize yardımcı olacak aynı tablosunun başka bir görünümü aşağıda verilmiştir:
 
**Batch AI hiyerarşi**
![Akış Diyagramı](./media/overview-what-happened-batch-ai/batchai-heirarchy.png) 
 
**Azure Machine Learning hizmeti hiyerarşisi**
![Akış Diyagramı](./media/overview-what-happened-batch-ai/azure-machine-learning-service-heirarchy.png) 

### <a name="platform-capabilities"></a>Platform özellikleri
Azure Machine Learning hizmeti harika bir uçtan uca eğitim dahil olmak üzere yeni işlevler ilişin -> Azure kaynaklarını yönetmek zorunda kalmadan, yapay ZEKA geliştirme için kullanabileceğiniz dağıtım yığını. Bu tablo iki hizmet arasında eğitim özellik desteği karşılaştırır.

|Özellik|BatchAI hizmeti|Azure Machine Learning hizmeti|
|-------|:-------:|:-------:|
|VM boyutu seçeneği |CPU/GPU    |CPU/GPU. Ayrıca FPGA için çıkarım destekler|
|AI hazır kümesi (sürücüleri, Docker, vb.)|  Evet |Evet|
|Düğüm hazırlığı| Evet|    Hayır|
|İşletim sistemi ailesi seçim   |Kısmi    |Hayır|
|Ayrılmış ve LowPriority VM'ler  |Evet    |Evet|
|Otomatik Ölçeklendirme   |Evet    |Evet (varsayılan)|
|Bekleme süresi için otomatik ölçeklendirme  |Hayır |Evet|
|SSH    |Evet|   Evet|
|Küme düzeyi bağlama |Evet (FileShares, Bloblar, NFS, özel)   |Evet (bağlayın veya kendi veri deposu indirin)|
|Dağıtılmış eğitimi|  Evet |Evet|
|İş yürütme modu|    VM veya kapsayıcı|    Kapsayıcı|
|Özel bir kapsayıcı görüntüsü|    Evet |Evet|
|Herhangi bir araç seti    |Evet    |Evet (çalışma Python betiğini)|
|JobPreparation|    Evet |Henüz değil|
|İş düzeyi bağlama |Evet (FileShares, Bloblar, NFS, özel)   |Evet (FileShares, BLOB'ları)|
|İş izleme     |GetJob|    Çalıştırma geçmişi (daha zengin bilgi, daha fazla ölçüm göndermek için özel çalışma zamanı)|
|İş günlükleri ve dosyaları/modelleri alma |   ListFiles ve depolama API'leri  |Yapı Hizmeti|
 |Tensorboard desteği   |Hayır|    Evet|
|VM ailesi düzeyi kotaları |Evet    |(İlet taşınan Evet önceki kapasite ile)|
 
Yukarıdaki tabloda ek olarak, Azure Machine Learning hizmetinde BatchAI içinde desteklenen geleneksel değil özellikler mevcuttur.

|Özellik|BatchAI hizmeti|Azure Machine Learning hizmeti|
|-------|:-------:|:-------:|
|Ortamı hazırlama    |Hayır |Evet (Conda hazırlamak ve ACR yükleme)|
|Hiper parametre ayarı  |Hayır|    Evet|
|Model yönetimi   |Hayır |Evet|
|Kullanıma hazır hale getirme/dağıtım| Hayır  |ACI ve AKS|
|Veri hazırlama   |Hayır |Evet|
|Hedef işlem    |Azure VM’leri  |Yerel, BatchAI (olarak AmlCompute), DataBricks, HDInsight|
|Otomatik makine öğrenimi |Hayır|    Evet|
|İşlem hatları  |Hayır |Evet|
|Toplu Puanlama  |Evet    |Evet|
|Portal/CLI desteği|    Evet |Evet|


### <a name="programming-interfaces"></a>Programlama arabirimleri

Bu tabloda, çeşitli programlama arabirimlerinde için her bir hizmet sunar.
    
|Özellik|BatchAI hizmeti|Azure Machine Learning hizmeti|
|-------|:-------:|:-------:|
|SDK    |Java, C#, Python, Nodejs   |Python (temel çalıştırma yapılandırma ve tahmin ortak çerçeveleri için temel)|
|CLI    |Evet    |Henüz değil|
|Azure portal   |Evet    |Evet (iş gönderme dışında)|
|REST API   |Evet    |Mikro hizmetler arasında ancak dağıtılmış Evet|




## <a name="why-migrate"></a>Neden geçiş?

Önizleme BatchAI hizmetinden GA'ed Azure Machine Learning hizmeti için yükseltme Estimators ve veri depoları gibi kullanmak daha kolay olan kavramları aracılığıyla daha iyi bir deneyim sunar. Ayrıca Azure hizmet düzeyi SLA'lar genel kullanım ve müşteri desteği garanti eder.

Azure Machine Learning hizmeti, otomatik ML, Hiper parametre ayarı ve ML işlem hatları en büyük ölçekli yapay ZEKA iş yüklerini yararlı olan gibi yeni işlevsellik de getirir. Eğitilen bir modelin ayrı bir hizmet arasında geçiş yapmadan getirebilme imkanı, verilerin hazırlanmasından (Data Prep SDK'sını kullanarak) veri bilimi döngüyü tamamlamak tamamen kullanıma hazır hale getirme ve model izlemek için yardımcı olur.

<a name="migrate"></a>

## <a name="how-do-i-migrate"></a>Nasıl geçiş yaparım?

Bu Geçiş Kılavuzu iki hizmet arasında eşleme komutlarını yardımcı adımları izlemeden önce Azure Machine Learning hizmeti aracılığıyla alışık alma biraz zaman harcadığı öneririz, [belgeleri](../machine-learning/service/overview-what-is-azure-ml.md) dahil [python'da öğretici](../machine-learning/service/tutorial-train-models-with-aml.md).

Uygulamalarınıza kesintilerinden kaçının ve en son özelliklerden yararlanmak için 31 Mart 2019'den önce aşağıdaki adımları uygulayın:

1. Bir Azure Machine Learning hizmeti çalışma alanı oluşturun ve kullanmaya başlayın:
    + [Python tabanlı hızlı başlangıç](../machine-learning/service/quickstart-create-workspace-with-python.md)
    + [Azure portal tabanlı hızlı başlangıç](../machine-learning/service/quickstart-get-started.md)

1. Ayarlanmış bir [Azure Machine Learning işlem](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute) modeli eğitimi için.

1. Azure Machine Learning işlem kullanmak için komut dosyalarını güncelleştirin.


### <a name="sdk-migration"></a>SDK'sı geçiş

Geçerli SDK desteği Azure Machine Learning birkaç Python Sdk'leri hizmetidir. Ana SDK'sı, kabaca iki haftada bir güncelleştirilir ve aşağıdaki komutu kullanarak Pypı ' yüklenebilir:

```python
pip install --upgrade azureml-sdk[notebooks]
```

Ortamınızı ayarlama ve bunları kullanarak SDK'sını yükleyin [hızlı başlangıç yönergeleri](../machine-learning/service/quickstart-create-workspace-with-python.md#install-the-sdk)

İlgili conda ortama işaret eden çekirdeği ile jupyter not defteri açtıktan sonra iki hizmet komutlar nasıl eşleştiği şöyledir:


#### <a name="create-a-workspace"></a>Çalışma alanı oluşturma
Bir yapılandırma dosyasında Azure ML'yi kullanarak BatchAI içinde bir configuration.json kullanarak bir çalışma alanı başlatma kavramına benzer şekilde eşler.

İçin **Batch AI**, bu şekilde yaptınız:

```python
sys.path.append('../../..')
import utilities as utils

cfg = utils.config.Configuration('../../configuration.json')
client = utils.config.create_batchai_client(cfg)

utils.config.create_resource_group(cfg)
_ = client.workspaces.create(cfg.resource_group, cfg.workspace, cfg.location).result()
```

**Azure Machine Learning hizmeti**, deneyin:

```python
from azureml.core.workspace import Workspace

ws = Workspace.from_config()
print('Workspace name: ' + ws.name, 
      'Azure region: ' + ws.location, 
      'Subscription id: ' + ws.subscription_id, 
      'Resource group: ' + ws.resource_group, sep = '\n')
```

Ayrıca, bir çalışma alanı gibi yapılandırma parametreleri belirterek doğrudan oluşturabilirsiniz

```python
from azureml.core import Workspace
# Create the workspace using the specified parameters
ws = Workspace.create(name = workspace_name,
                      subscription_id = subscription_id,
                      resource_group = resource_group, 
                      location = workspace_region,
                      create_resource_group = True,
                      exist_ok = True)
ws.get_details()

# write the details of the workspace to a configuration file to the notebook library
ws.write_config()
```

AML çalışma sınıfı hakkında daha fazla bilgi [SDK başvuru belgeleri](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py).


#### <a name="create-a-compute-cluster"></a>Bir işlem kümesini oluşturun
Azure Machine Learning bazı hizmet ve çalışma alanınıza (örn. eklenebilecek başkaları tarafından yönetilen, birden çok bilgisayar hedefine destekler Bir HDInsight kümesi veya uzak bir VM. Çeşitli hakkında daha fazla bilgiyi [hedefleri işlem](../machine-learning/service/how-to-set-up-training-targets.md). Bir BatchAI oluşturma kavramı Azure ML'de AmlCompute küme oluşturmak için küme haritalarını işlem. Parametreleri BatchAI içinde nasıl geçireceğiniz benzer bir işlem yapılandırmasında Amlcompute oluşturulması alır. Unutmayın, BatchAI varsayılan olarak kapalıdır ise otomatik ölçeklendirmenin AmlCompute kümenizdeki varsayılan olarak açıktır ' dir.

İçin **Batch AI**, bu şekilde yaptınız:

```python
nodes_count = 2
cluster_name = 'nc6'

parameters = models.ClusterCreateParameters(
    vm_size='STANDARD_NC6',
    scale_settings=models.ScaleSettings(
        manual=models.ManualScaleSettings(target_node_count=nodes_count)
    ),
    user_account_settings=models.UserAccountSettings(
        admin_user_name=cfg.admin,
        admin_user_password=cfg.admin_password or None,
        admin_user_ssh_public_key=cfg.admin_ssh_key or None,
    )
)
_ = client.clusters.create(cfg.resource_group, cfg.workspace, cluster_name, parameters).result()
```

İçin **Azure Machine Learning hizmeti**, deneyin:

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
gpu_cluster_name = "nc6"

# Verify that cluster does not exist already
try:
    gpu_cluster = ComputeTarget(workspace=ws, name=gpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6',
                                                           vm_priority='lowpriority',
                                                           min_nodes=1,
                                                           max_nodes=2,
                                                           idle_seconds_before_scaledown='300',
                                                           vnet_resourcegroup_name='<my-resource-group>',
                                                           vnet_name='<my-vnet-name>',
                                                           subnet_name='<my-subnet-name>')
    gpu_cluster = ComputeTarget.create(ws, gpu_cluster_name, compute_config)

gpu_cluster.wait_for_completion(show_output=True)
```

AMLCompute sınıfı hakkında daha fazla bilgi [SDK başvuru belgeleri](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py). Yukarıdaki yapılandırmada yalnızca vm_size ve max_nodes zorunlu olan ve sanal ağlar gibi özelliklerin geri kalanı olan yalnızca Gelişmiş Küme kurulumu için olduğunu unutmayın.


#### <a name="monitoring-status-of-your-cluster"></a>Kümenizi durumunu izleme
Bu aşağıda göreceğiniz gibi Azure ML üzerinde daha basittir.

İçin **Batch AI**, bu şekilde yaptınız:

```python
cluster = client.clusters.get(cfg.resource_group, cfg.workspace, cluster_name)
utils.cluster.print_cluster_status(cluster)
```

İçin **Azure Machine Learning hizmeti**, deneyin:

```python
gpu_cluster.get_status().serialize()
```

#### <a name="getting-reference-to-a-storage-account"></a>Bir depolama hesabı için başvuru alma
Bir veri depolama, blob gibi kavramı veri deposu nesnesi kullanılarak Azure ML'de Basitleştirilmiş. Varsayılan olarak, bir depolama hesabı, Azure ML çalışma alanı oluşturur, ancak kendi depolama çalışma alanı oluşturmanın bir parçası da ekleyebilirsiniz. 

İçin **Batch AI**, bu şekilde yaptınız:

```python
azure_blob_container_name = 'batchaisample'
blob_service = BlockBlobService(cfg.storage_account_name, cfg.storage_account_key)
blob_service.create_container(azure_blob_container_name, fail_on_exist=False)
```

İçin **Azure Machine Learning hizmeti**, deneyin:

```python
ds = ws.get_default_datastore()
print(ds.datastore_type, ds.account_name, ds.container_name)
```

Ek depolama hesapları kaydetme veya başka bir kayıtlı veri deposunda bir başvuru alma hakkında daha fazla bilgi [Azure ML service belgelerini](../machine-learning/service/how-to-access-data.md#access).


#### <a name="downloading-and-uploading-data"></a>Karşıya veri yükleme ve indirme 
Her iki hizmet ile kolayca Yukarıdaki veri deposu başvurusu kullanarak depolama hesabına verileri karşıya yükleyebilirsiniz. Azure ML söz konusu olduğunda işin yapılandırmanızın parçası olarak belirttiğiniz nasıl görürsünüz ancak BatchAI için dosya paylaşımını bir parçası olarak biz eğitim betiğini de dağıtın.

İçin **Batch AI**, bu şekilde yaptınız:

```python
mnist_dataset_directory = 'mnist_dataset'
utils.dataset.download_and_upload_mnist_dataset_to_blob(
    blob_service, azure_blob_container_name, mnist_dataset_directory)

script_directory = 'tensorflow_samples'
script_to_deploy = 'mnist_replica.py'

blob_service.create_blob_from_path(azure_blob_container_name,
                                   script_directory + '/' + script_to_deploy, 
                                   script_to_deploy)
```


İçin **Azure Machine Learning hizmeti**, deneyin:

```python
import os
import urllib
os.makedirs('./data', exist_ok=True)
download_url = 'https://s3.amazonaws.com/img-datasets/mnist.npz'
urllib.request.urlretrieve(download_url, filename='data/mnist.npz')

ds.upload(src_dir='data', target_path='mnist_dataset', overwrite=True, show_progress=True)

path_on_datastore = ' mnist_dataset/mnist.npz' ds_data = ds.path(path_on_datastore) print(ds_data)
```

#### <a name="create-an-experiment"></a>Deneme oluşturma
Yukarıda da belirtildiği gibi Azure ML BatchAI için benzer bir denemeyi kavramı vardır. Her deneme sonra bireysel çalışır, nasıl işleri BatchAI sahibiz için benzer olabilir. Azure ML, tek tek alt çalıştırmalar için çalışan her bir üst hiyerarşisiyle sahip olmanızı sağlar.

İçin **Batch AI**, bu şekilde yaptınız:

```python
experiment_name = 'tensorflow_experiment'
experiment = client.experiments.create(cfg.resource_group, cfg.workspace, experiment_name).result()
```

İçin **Azure Machine Learning hizmeti**, deneyin:

```python
from azureml.core import Experiment

experiment_name = 'tensorflow_experiment'
experiment = Experiment(ws, name=experiment_name)
```


#### <a name="submit-a-job"></a>Bir iş gönderdiniz
Bir deney oluşturduktan sonra bir farklı çalıştır gönderdikten birkaç farklı yöntemle sahip. Bu örnekte, TensorFlow kullanarak bir ayrıntılı öğrenme modeli oluşturmaya çalışıyorsunuz ve bir Azure ML tahmin, bunu yapmak için kullanır. Bir [Estimator](../machine-learning/service/how-to-train-ml-models.md) yalnızca bir sarmalayıcı işlevi çalıştırmaları göndermek kolaylaştırır ve Pytorch ve TensorFlow için şu anda yalnızca desteklenir temel alınan çalışma yapılandırması. Veri depoları kavramını da ne kadar kolay hale göreceksiniz bağlama yollarını belirtmek için 

İçin **Batch AI**, bu şekilde yaptınız:

```python
azure_file_share = 'afs'
azure_blob = 'bfs'
args_fmt = '--job_name={0} --num_gpus=1 --train_steps 10000 --checkpoint_dir=$AZ_BATCHAI_OUTPUT_MODEL --log_dir=$AZ_BATCHAI_OUTPUT_TENSORBOARD --data_dir=$AZ_BATCHAI_INPUT_DATASET --ps_hosts=$AZ_BATCHAI_PS_HOSTS --worker_hosts=$AZ_BATCHAI_WORKER_HOSTS --task_index=$AZ_BATCHAI_TASK_INDEX'

parameters = models.JobCreateParameters(
     cluster=models.ResourceId(id=cluster.id),
     node_count=2,
     input_directories=[
        models.InputDirectory(
            id='SCRIPT',
            path='$AZ_BATCHAI_JOB_MOUNT_ROOT/{0}/{1}'.format(azure_blob, script_directory)),
        models.InputDirectory(
            id='DATASET',
            path='$AZ_BATCHAI_JOB_MOUNT_ROOT/{0}/{1}'.format(azure_blob, mnist_dataset_directory))],
     std_out_err_path_prefix='$AZ_BATCHAI_JOB_MOUNT_ROOT/{0}'.format(azure_file_share),
     output_directories=[
        models.OutputDirectory(
            id='MODEL',
            path_prefix='$AZ_BATCHAI_JOB_MOUNT_ROOT/{0}'.format(azure_file_share),
            path_suffix='Models'),
        models.OutputDirectory(
            id='TENSORBOARD',
            path_prefix='$AZ_BATCHAI_JOB_MOUNT_ROOT/{0}'.format(azure_file_share),
            path_suffix='Logs')
     ],
     mount_volumes=models.MountVolumes(
            azure_file_shares=[
                models.AzureFileShareReference(
                    account_name=cfg.storage_account_name,
                    credentials=models.AzureStorageCredentialsInfo(
                        account_key=cfg.storage_account_key),
                    azure_file_url='https://{0}.file.core.windows.net/{1}'.format(
                        cfg.storage_account_name, azure_file_share_name),
                    relative_mount_path=azure_file_share)
            ],
            azure_blob_file_systems=[
                models.AzureBlobFileSystemReference(
                    account_name=cfg.storage_account_name,
                    credentials=models.AzureStorageCredentialsInfo(
                        account_key=cfg.storage_account_key),
                    container_name=azure_blob_container_name,
                    relative_mount_path=azure_blob)
            ]
        ),
     container_settings=models.ContainerSettings(
         image_source_registry=models.ImageSourceRegistry(image='tensorflow/tensorflow:1.8.0-gpu')),
     tensor_flow_settings=models.TensorFlowSettings(
         parameter_server_count=1,
         worker_count=nodes_count,
         python_script_file_path='$AZ_BATCHAI_INPUT_SCRIPT/'+ script_to_deploy,
         master_command_line_args=args_fmt.format('worker'),
         worker_command_line_args=args_fmt.format('worker'),
         parameter_server_command_line_args=args_fmt.format('ps'),
     )
)
```

İşi gönderiliyor kendisini BatchAI Oluştur işlevdir.

```python
job_name = datetime.utcnow().strftime('tf_%m_%d_%Y_%H%M%S')
job = client.jobs.create(cfg.resource_group, cfg.workspace, experiment_name, job_name, parameters).result()
print('Created Job {0} in Experiment {1}'.format(job.name, experiment.name))
```

Bu eğitim kod parçacığı (yukarıdaki dosya paylaşımına dosyamızı mnist_replica.py dosyası dahil) için tüm bilgiler bulunabilir [BatchAI örnek not defteri github deposunu](https://github.com/Azure/BatchAI/tree/2238607d5a028a0c5e037168aefca7d7bb165d5c/recipes/TensorFlow/TensorFlow-GPU-Distributed).

İçin **Azure Machine Learning hizmeti**, deneyin:

```python
from azureml.train.dnn import TensorFlow

script_params={
    '--num_gpus': 1,
    '--train_steps': 500,
    '--input_data': ds_data.as_mount()

}

estimator = TensorFlow(source_directory=project_folder,
                       compute_target=gpu_cluster,
                       script_params=script_params,
                       entry_script='tf_mnist_replica.py',
                       node_count=2,
                       worker_count=2,
                       parameter_server_count=1,   
                       distributed_backend='ps',
                       use_gpu=True)
```

Bu eğitim kod parçacığı (tf_mnist_replica.py dosyası dahil) için tüm bilgiler bulunabilir [Azure ML örnek not defteri github deposunu](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/distributed-tensorflow-with-parameter-server). Datastore ya da tek düğümler üzerinden bağlanabilir veya eğitim verileri düğümde indirilebilir. Veri deposunda, tahmin başvurma hakkında daha fazla ayrıntı konusu [Azure ML service belgelerini](../machine-learning/service/how-to-access-data.md#access). 

Azure ML çalıştırmasında gönderme gönderme işlevdir.

```python
run = experiment.submit(estimator)
print(run)
```

– Özel eğitim ortamı tanımlamak için özellikle yararlı çalışma bir yapılandırma kullanarak çalıştırmanız, parametrelerini belirtme başka bir yolu yoktur. Bu konuda daha fazla ayrıntı bulabilirsiniz [örnek AmlCompute not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-amlcompute/train-on-amlcompute.ipynb). 

#### <a name="monitor-your-run"></a>Çalıştırma izleyin
Bir farklı çalıştır gönderildikten sonra ya da tamamlamak ya da Azure ML, doğrudan kodunuzdan çağırabilirsiniz masaüstünüzdeki Jupyter pencere öğelerini kullanarak izlemek de bekleyebilirsiniz. Bir çalışma alanındaki çeşitli denemeler döngü herhangi bir önceki çalıştırma bağlamında çekebilirsiniz ve tek tek her deneme içinde çalıştırır.

İçin **Batch AI**, bu şekilde yaptınız:

```python
utils.job.wait_for_job_completion(client, cfg.resource_group, cfg.workspace, 
                                  experiment_name, job_name, cluster_name, 'stdouterr', 'stdout-wk-0.txt')

files = client.jobs.list_output_files(cfg.resource_group, cfg.workspace, experiment_name, job_name,
                                      models.JobsListOutputFilesOptions(outputdirectoryid='stdouterr')) 
for f in list(files):
    print(f.name, f.download_url or 'directory')
```


İçin **Azure Machine Learning hizmeti**, deneyin:

```python
run.wait_for_completion(show_output=True)

from azureml.widgets import RunDetails
RunDetails(run).show()
```

Pencere öğesi, günlükleri gerçek zamanlı olarak aramak için defterinizdeki nasıl yüklenir, bir anlık görüntüsünü şu şekildedir: ![Pencere öğesi izleme diyagramı](./media/overview-what-happened-batch-ai/monitor.png)



#### <a name="editing-a-cluster"></a>Küme düzenleme
Küme silme oldukça basittir. Ayrıca, Azure ML ayrıca düğümlerinin daha yüksek bir sayı için ölçeklendirmek veya kümeyi ölçeklendirme önce boşta bekleme süresini artırmak istiyorsanız, Not defterini içinde bir kümeden güncelleştirmenizi sağlar. Arka uçtaki etkili bir şekilde yeni bir dağıtım gerektirdiğinden, kümenin kendisi, VM boyutunu değiştirmek izin verme.

İçin **Batch AI**, bu şekilde yaptınız:
```python
_ = client.clusters.delete(cfg.resource_group, cfg.workspace, cluster_name)
```

İçin **Azure Machine Learning hizmeti**, deneyin:

```python
gpu_cluster.delete()

gpu_cluster.update(min_nodes=2, max_nodes=4, idle_seconds_before_scaledown=600)
```

## <a name="support"></a>Destek

BatchAI 31 Mart devre dışı bırakmak için planlanmıştır ve desteği ile bir özel durum oluşturularak izin verilenler listesinde olmadığı sürece, hizmete kaydediliyor gelen yeni abonelikler zaten engelliyor.  Adresinden bize ulaşın [Azure Batch AI eğitimi önizlemesi](mailto:AzureBatchAITrainingPreview@service.microsoft.com) ile herhangi bir sorunuz veya Azure Machine Learning hizmeti ile geçiş yaparken geri bildirimde bulunmak istiyorsanız.

Azure Machine Learning hizmeti genel kullanıma sunulan bir hizmettir. Bu, taahhüt SLA'sı ve aralarından seçim yapabileceğiniz çeşitli destek planları ile geldiğini anlamına gelir.

Yalnızca her iki durumda yalnızca temel işlem fiyatı alırız olarak Azure altyapısı BatchAI hizmeti aracılığıyla veya Azure Machine Learning hizmeti ile kullanmak için fiyatlandırma, değiştirilmemelidir. Daha fazla bilgi için [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/details/machine-learning-service/).

Bölgesel kullanılabilirlik iki hizmet arasında görünümünde [Azure portalında](https://azure.microsoft.com/global-infrastructure/services/?products=batch-ai,machine-learning-service&regions=all).


## <a name="next-steps"></a>Sonraki adımlar

+ Okuma [Azure Machine Learning hizmetine genel bakış](../machine-learning/service/overview-what-is-azure-ml.md).

+ [Bir işlem hedefine modeli eğitimi için yapılandırma](../machine-learning/service/how-to-set-up-training-targets.md) Azure Machine Learning hizmeti ile.

+ Gözden geçirme [Azure yol haritası](https://azure.microsoft.com/updates/) diğer Azure hizmet güncelleştirmeleri öğrenin.
