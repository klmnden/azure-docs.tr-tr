---
title: Azure Hızlı Başlangıcı - Batch AI ile CNTK eğitimi - Python | Microsoft Docs
description: Python SDK’sını kullanarak Batch AI ile CNTK eğitim işini çalıştırmayı hızlı şekilde öğrenin
services: batch-ai
documentationcenter: na
author: lliimsft
manager: Vaman.Bedekar
editor: tysonn
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: Python
ms.topic: quickstart
ms.date: 10/06/2017
ms.author: lili
ms.openlocfilehash: f535c9adf4926f29ae9cade6382debedab73937d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="run-a-cntk-training-job-using-the-azure-python-sdk"></a>Azure Python SDK’sını kullanarak CNTK eğitim işini çalıştırma

Bu hızlı başlangıçta, Batch AI hizmeti kullanılarak Microsoft Cognitive Toolkit (CNTK) eğitim işini çalıştırmak için Azure Python SDK’sının kullanılması açıklanmaktadır. 

Bu örnekte, tek düğümlü GPU kümesinde konvolüsyonel sinir ağını (CNN) eğitmek için el yazısı görüntülerin MNIST veritabanını kullanacaksınız. 

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

* Azure Python SDK’sı - [Yükleme yönergelerine](/python/azure/python-sdk-azure-install) bakın

* Azure depolama hesabı - Bkz. [Azure depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md)

* Azure Active Directory hizmet sorumlusu kimlik bilgileri - Bkz. [CLI ile hizmet sorumlusu oluşturma](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

* Azure Cloud Shell veya Azure CLI kullanarak aboneliğiniz için Batch AI kaynak sağlayıcılarını kaydedin. Sağlayıcı kaydı 15 dakikaya kadar sürebilir.

  ```azurecli
  az provider register -n Microsoft.BatchAI
  az provider register -n Microsoft.Batch
  ```


## <a name="configure-credentials"></a>Kimlik bilgilerini yapılandırma
Yerine kendi değerlerinizi koyarak Python betiğinizde bu parametreleri oluşturun:

```Python
# credentials used for authentication
client_id = 'my_aad_client_id'
secret = 'my_aad_secret_key'
token_uri = 'my_aad_token_uri'
subscription_id = 'my_subscription_id'

# credentials used for storage
storage_account_name = 'my_storage_account_name'
storage_account_key = 'my_storage_account_key'

# specify the credentials used to remote login your GPU node
admin_user_name = 'my_admin_user_name'
admin_user_password = 'my_admin_user_password'
```

En iyi uygulama olarak tüm kimlik bilgileri, bu örnekte gösterilmeyen ayrı bir yapılandırma dosyasında depolanmalıdır. Bir yapılandırma dosyasını uygulamak için [tarifler](https://github.com/Azure/BatchAI/tree/master/recipes) bölümüne bakın. 

## <a name="authenticate-with-batch-ai"></a>Batch AI ile kimlik doğrulaması

Azure Batch AI uygulamasına erişebilmek için Azure Active Directory kullanarak oturum açmanız gerekir. Aşağıda, hizmet sorumlusu kimlik bilgilerinizi ve abonelik kimliğinizi kullanarak hizmette kimlik doğrulaması yapmak (`BatchAIManagementClient` nesnesi oluşturmak) için kod yer almaktadır:

```Python
from azure.common.credentials import ServicePrincipalCredentials
import azure.mgmt.batchai as batchai
import azure.mgmt.batchai.models as models

creds = ServicePrincipalCredentials(
        client_id=client_id, secret=secret, token_uri=token_uri)

batchai_client = batchai.BatchAIManagementClient(credentials=creds,
                                         subscription_id=subscription_id
)
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Batch AI kümeleri ve işleri Azure kaynaklarıdır ve bir Azure kaynak grubuna yerleştirilmelidir. Aşağıdaki kod parçacığı bir kaynak grubu oluşturur:

```Python
from azure.mgmt.resource import ResourceManagementClient

resource_group_name = 'myresourcegroup'

resource_management_client = ResourceManagementClient(
        credentials=creds, subscription_id=subscription_id)

resource = resource_management_client.resource_groups.create_or_update(
        resource_group_name, {'location': 'eastus'})
```


## <a name="prepare-azure-file-share"></a>Azure dosya paylaşımını hazırlama
Gösterim amacıyla, bu hızlı başlangıçta öğrenme işine yönelik eğitim verilerini ve betikleri barındırmak için bir Azure dosya paylaşımı kullanılmaktadır. 

1. *batchaiquickstart* adlı bir dosya paylaşımı oluşturun.

  ```Python
  from azure.storage.file import FileService 
 
  azure_file_share_name = 'batchaiquickstart' 
 
  service = FileService(storage_account_name, storage_account_key) 
 
  service.create_share(azure_file_share_name, fail_on_exist=False)
  ``` 
 
2. *mnistcntksample* adlı paylaşımda bir dizin oluşturun 

  ```Python
  mnist_dataset_directory = 'mnistcntksample' 
 
  service.create_directory(azure_file_share_name, mnist_dataset_directory, fail_on_exist=False) 
  ```
3. [Örnek paketi](https://batchaisamples.blob.core.windows.net/samples/BatchAIQuickStart.zip?st=2017-09-29T18%3A29%3A00Z&se=2099-12-31T08%3A00%3A00Z&sp=rl&sv=2016-05-31&sr=b&sig=hrAZfbZC%2BQ%2FKccFQZ7OC4b%2FXSzCF5Myi4Cj%2BW3sVZDo%3D) indirip sıkıştırmasını açın. İçerikleri, Python betiğini yürüteceğiniz dizine yükleyin.

  ```Python
  for f in ['Train-28x28_cntk_text.txt', 'Test-28x28_cntk_text.txt', 'ConvNet_MNIST.py']:     
     service.create_file_from_path(
             azure_file_share_name, mnist_dataset_directory, f, f) 
  ```

## <a name="create-gpu-cluster"></a>GPU kümesi oluşturma

Bir Batch AI kümesi oluşturun. Bu örnekte küme, tek bir STANDARD_NC6 VM düğümünden oluşur. Bu sanal makine tek bir NVIDIA K80 GPU içerir. Dosya paylaşımını *azurefileshare* adlı bir klasöre bağlayın. GPU işlem düğümünde bu klasörün tam yolu $AZ_BATCHAI_MOUNT_ROOT/azurefileshare şeklindedir.

```Python
cluster_name = 'mycluster'

relative_mount_point = 'azurefileshare' 
 
parameters = models.ClusterCreateParameters(
    # Location where the cluster will physically be deployed
    location='eastus', 
 
    # VM size. Use NC or NV series for GPU
    vm_size='STANDARD_NC6', 
 
    # Configure the ssh users
    user_account_settings=models.UserAccountSettings(
         admin_user_name=admin_user_name,
         admin_user_password=admin_user_password), 
 
    # Number of VMs in the cluster
    scale_settings=models.ScaleSettings(
         manual=models.ManualScaleSettings(target_node_count=1)
     ), 
 
    # Configure each node in the cluster
    node_setup=models.NodeSetup( 
 
        # Mount shared volumes to the host
         mount_volumes=models.MountVolumes(
             azure_file_shares=[
                 models.AzureFileShareReference(
                     account_name=storage_account_name,
                     credentials=models.AzureStorageCredentialsInfo(
         account_key=storage_account_key),
         azure_file_url='https://{0}.file.core.windows.net/{1}'.format(
               storage_account_name, azure_file_share_name),
                  relative_mount_path = relative_mount_point)],
         ), 
    ), 
) 
batchai_client.clusters.create(resource_group_name, cluster_name, parameters).result() 
```

## <a name="get-cluster-status"></a>Küme durumunu alma

Aşağıdaki komutu kullanarak küme durumunu izleyin: 

```Python
cluster = batchai_client.clusters.get(resource_group_name, cluster_name)
print('Cluster state: {0} Target: {1}; Allocated: {2}; Idle: {3}; '
      'Unusable: {4}; Running: {5}; Preparing: {6}; leaving: {7}'.format(
    cluster.allocation_state,
    cluster.scale_settings.manual.target_node_count,
    cluster.current_node_count,
    cluster.node_state_counts.idle_node_count,
    cluster.node_state_counts.unusable_node_count,
    cluster.node_state_counts.running_node_count,
    cluster.node_state_counts.preparing_node_count,
    cluster.node_state_counts.leaving_node_count)) 
```

Önceki kod, aşağıda örnekleri verilen temel küme ayırma bilgilerini yazdırır:

```Shell
Cluster state: AllocationState.steady Target: 1; Allocated: 1; Idle: 0; Unusable: 0; Running: 0; Preparing: 1; Leaving 0
 
```  

Düğümler ayrıldığında ve hazırlık işlemini tamamladığında küme hazırdır (`nodeStateCounts` özniteliğine bakın). Bir sorun oluştuysa `errors` özniteliği, hata açıklamasını içerir.

## <a name="create-training-job"></a>Eğitim çalışması oluşturma

Küme hazır olduktan sonra öğrenme işini yapılandırın ve gönderin. 

```Python
job_name = 'myjob' 
 
parameters = models.job_create_parameters.JobCreateParameters( 
 
     # Location where the job will run
     # Ideally this should be co-located with the cluster.
     location='eastus', 
 
     # The cluster this job will run on
     cluster=models.ResourceId(cluster.id), 
 
     # The number of VMs in the cluster to use
     node_count=1, 
 
     # Override the path where the std out and std err files will be written to.
     # In this case we will write these out to an Azure Files share
     std_out_err_path_prefix='$AZ_BATCHAI_MOUNT_ROOT/{0}'.format(relative_mount_point), 
 
     input_directories=[models.InputDirectory(
         id='SAMPLE',
         path='$AZ_BATCHAI_MOUNT_ROOT/{0}/{1}'.format(relative_mount_point, mnist_dataset_directory))], 
 
     # Specify directories where files will get written to 
     output_directories=[models.OutputDirectory(
        id='MODEL',
        path_prefix='$AZ_BATCHAI_MOUNT_ROOT/{0}'.format(relative_mount_point),
        path_suffix="Models")], 
 
     # Container configuration
     container_settings=models.ContainerSettings(
        models.ImageSourceRegistry(image='microsoft/cntk:2.1-gpu-python3.5-cuda8.0-cudnn6.0')), 
 
     # Toolkit specific settings
     cntk_settings = models.CNTKsettings(
        python_script_file_path='$AZ_BATCHAI_INPUT_SAMPLE/ConvNet_MNIST.py',
        command_line_args='$AZ_BATCHAI_INPUT_SAMPLE $AZ_BATCHAI_OUTPUT_MODEL')
 ) 
 
# Create the job 
batchai_client.jobs.create(resource_group_name, job_name, parameters).result() 
```

## <a name="monitor-job"></a>İşi izleme
Aşağıdaki komutu kullanarak işin durumunu denetleyebilirsiniz: 
 
```Python
job = batchai_client.jobs.get(resource_group_name, job_name) 
 
print('Job state: {0} '.format(job.execution_state.name))
```

Çıktı şuna benzer olacaktır: `Job state: running`.

`executionState`, işin geçerli yürütme durumunu içerir:
* `queued`: iş, küme düğümlerinin kullanılabilir olmasını bekliyor
* `running`: iş çalışıyor
* `succeeded` (veya `failed`): iş tamamlandı ve `executionInfo`, sonuçla ilgili ayrıntıları içeriyor
 
## <a name="list-stdout-and-stderr-output"></a>stdout ve stderr çıktısını listeleme
stdout ve stderr günlük dosyalarının bağlantılarını listelemek için aşağıdaki komutu kullanın:

```Python
files = batchai_client.jobs.list_output_files(resource_group_name, job_name, models.JobsListOutputFilesOptions("stdouterr")) 
 
for file in list(files):
     print('file: {0}, download url: {1}'.format(file.name, file.download_url)) 
```
## <a name="delete-resources"></a>Kaynakları silme

İşi silmek için aşağıdaki komutu kullanın:
```Python
batchai_client.jobs.delete(resource_group_name, job_name) 
```

Kümeyi silmek için aşağıdaki komutu kullanın:
```Python
batchai_client.clusters.delete(resource_group_name, cluster_name)
```
## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure Python SDK’sını kullanarak bir Batch AI kümesinde CNTK eğitim işinin nasıl çalıştırılacağını öğrendiniz. Farklı araç takımları ile Batch AI uygulamasını kullanma hakkında daha fazla bilgi edinmek için [eğitim tariflerine](https://github.com/Azure/BatchAI) bakın.
