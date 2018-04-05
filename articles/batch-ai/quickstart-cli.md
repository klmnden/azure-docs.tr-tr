---
title: Azure Hızlı Başlangıcı - Batch AI ile CNTK eğitimi - Azure CLI | Microsoft Docs
description: Azure CLI’yi kullanarak Batch AI ile CNTK eğitim işini çalıştırmayı hızlı şekilde öğrenin
services: batch-ai
documentationcenter: na
author: AlexanderYukhanov
manager: Vaman.Bedekar
editor: tysonn
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: CLI
ms.topic: quickstart
ms.date: 10/06/2017
ms.author: Alexander.Yukhanov
ms.openlocfilehash: 82e3885021a2f2309dfed456d472e7027b8d6cf2
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="run-a-cntk-training-job-using-the-azure-cli"></a>Azure CLI kullanarak CNTK eğitim işini çalıştırma

Bu hızlı başlangıçta, Batch AI hizmeti kullanılarak Microsoft Cognitive Toolkit (CNTK) eğitim işini çalıştırmak için Azure komut satırı arabiriminin (CLI) kullanılması açıklanmaktadır. Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır.

Bu örnekte, Batch AI tarafından yönetilen tek düğümlü GPU kümesinde konvolüsyonel sinir ağını (CNN) eğitmek için el yazısı görüntülerin MNIST veritabanını kullanacaksınız. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu hızlı başlangıç için en son Azure CLI sürümünü kullanmanız gerekir. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

Azure Cloud Shell veya Azure CLI kullanılarak aboneliğiniz için Batch AI kaynak sağlayıcılarının da bir defa kaydedilmesi gerekir. Sağlayıcı kaydı 15 dakikaya kadar sürebilir.

```azurecli
az provider register -n Microsoft.BatchAI
az provider register -n Microsoft.Batch
```


## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Batch AI kümeleri ve işleri Azure kaynaklarıdır ve bir Azure kaynak grubuna yerleştirilmelidir.

[az group create](/cli/azure/group#az_group_create) komutuyla bir kaynak grubu oluşturun.

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur. Bu kaynak grubunu ve konumunu varsayılan olarak ayarlamak için [az configure](/cli/azure/reference-index#az_configure) komutunu kullanır.

```azurecli
az group create --name myResourceGroup --location eastus

az configure --defaults group=myResourceGroup

az configure --defaults location=eastus
```

>[!NOTE]
>`az` komutu için varsayılan değerleri ayarlama, isteğe bağlı bir adımdır. Varsayılan değerleri ayarlamamayı seçebilirsiniz. Varsayılan değerleri ayarlamayı seçerseniz, öğreticiyi tamamladıktan sonra varsayılan ayarları kaldırmanız gerekir. Aşağıdaki komutları kullanarak varsayılan ayarları kaldırın:
>
>```azurecli
>az configure --defaults group=''
>
>az configure --defaults location=''
>```
>

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Bu hızlı başlangıç, eğitim işine yönelik verileri ve betikleri barındırmak için bir Azure depolama hesabı kullanır. [az storage account create](/cli/azure/storage/account#az_storage_account_create) komutuyla bir depolama hesabı oluşturun.

```azurecli
az storage account create --name mystorageaccount --sku Standard_LRS
```

>[!NOTE]
>Her depolama hesabının benzersiz bir adı olmalıdır. Önceki `az` komutunda ve bu öğreticide yer alan diğer benzer komutlarda, `mystorageaccount` ayarının değerini, depolama hesabınızın adıyla değiştirin.

## <a name="prepare-azure-file-share"></a>Azure dosya paylaşımını hazırlama

Gösterim amacıyla, bu hızlı başlangıçta öğrenme işine yönelik eğitim verilerini ve betikleri barındırmak için bir Azure dosya paylaşımı kullanılmaktadır.

1. [az storage share create](/cli/azure/storage/share#az_storage_share_create) komutunu kullanarak *batchaiquickstart* adlı bir dosya paylaşımı oluşturun.

  ```azurecli
  az storage share create --account-name mystorageaccount --name batchaiquickstart
  ```
2. [az storage directory create](/cli/azure/storage/directory#az_storage_directory_create) komutunu kullanarak *mnistcntksample* adlı paylaşımda bir dizin oluşturun.

  ```azurecli
  az storage directory create --share-name batchaiquickstart  --name mnistcntksample
  ```

3. [Örnek paketi](https://batchaisamples.blob.core.windows.net/samples/BatchAIQuickStart.zip?st=2017-09-29T18%3A29%3A00Z&se=2099-12-31T08%3A00%3A00Z&sp=rl&sv=2016-05-31&sr=b&sig=hrAZfbZC%2BQ%2FKccFQZ7OC4b%2FXSzCF5Myi4Cj%2BW3sVZDo%3D) indirip sıkıştırmasını açın. [az storage file upload](/cli/azure/storage/file#az_storage_file_upload) komutunu kullanarak içerikleri dizine yükleyin:

  ```azurecli
  az storage file upload --share-name batchaiquickstart --source Train-28x28_cntk_text.txt --path mnistcntksample

  az storage file upload --share-name batchaiquickstart --source Test-28x28_cntk_text.txt --path mnistcntksample

  az storage file upload --share-name batchaiquickstart --source ConvNet_MNIST.py --path mnistcntksample
  ```


## <a name="create-gpu-cluster"></a>GPU kümesi oluşturma
Tek bir GPU VM düğümünden oluşan bir Batch AI kümesi oluşturmak için [az batchai cluster create](/cli/azure/batchai/cluster#az_batchai_cluster_create) komutunu kullanın. Bu örnekte sanal makine, varsayılan Ubuntu LTS görüntüsünü çalıştırır. Ek eğitim çerçevelerini destekleyen Microsoft Ayrıntılı Öğrenme Sanal Makinesi’ni çalıştırmak için bunun yerine `image UbuntuDSVM` komutunu belirtin. NC6 boyutu tek bir NVIDIA K80 GPU içerir. Dosya paylaşımını *azurefileshare* adlı bir klasöre bağlayın. GPU işlem düğümünde bu klasörün tam yolu $AZ_BATCHAI_MOUNT_ROOT/azurefileshare şeklindedir.


```azurecli
az batchai cluster create --name mycluster --vm-size STANDARD_NC6 --image UbuntuLTS --min 1 --max 1 --storage-account-name mystorageaccount --afs-name batchaiquickstart --afs-mount-path azurefileshare --user-name <admin_username> --password <admin_password>
```


Küme oluşturulduktan sonra çıktı aşağıdakilere benzer:

```azurecli
{
  "allocationState": "resizing",
  "allocationStateTransitionTime": "2017-10-05T02:09:03.194000+00:00",
  "creationTime": "2017-10-05T02:09:01.998000+00:00",
  "currentNodeCount": 0,
  "errors": null,
  "id": "/subscriptions/10d0b7c6-9243-4713-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.BatchAI/clusters/mycluster",
  "location": "eastus",
  "name": "mycluster",
  "nodeSetup": {
    "mountVolumes": {
      "azureBlobFileSystems": null,
      "azureFileShares": [
        {
          "accountName": "batchaisamples",
          "azureFileUrl": "https://batchaisamples.file.core.windows.net/batchaiquickstart",
          "credentialsInfo": {
            "accountKey": null,
            "accountKeySecretUrl": null
          },
          "directoryMode": "0777",
          "fileMode": "0777",
          "relativeMountPath": "azurefileshare"
        }
      ],
      "fileServers": null,
      "unmanagedFileSystems": null
    },
    "setupTask": null
  },
  "nodeStateCounts": {
    "idleNodeCount": 0,
    "leavingNodeCount": 0,
    "preparingNodeCount": 0,
    "runningNodeCount": 0,
    "unusableNodeCount": 0
  },
  "provisioningState": "succeeded",
  "provisioningStateTransitionTime": "2017-10-05T02:09:02.857000+00:00",
  "resourceGroup": "myresourcegroup",
  "scaleSettings": {
    "autoScale": null,
    "manual": {
      "nodeDeallocationOption": "requeue",
      "targetNodeCount": 1
    }
  },
  "subnet": {
    "id": null
  },
  "tags": null,
  "type": "Microsoft.BatchAI/Clusters",
  "userAccountSettings": {
    "adminUserName": "demoUser",
    "adminUserPassword": null,
    "adminUserSshPublicKey": null
  },
  "virtualMachineConfiguration": {
    "imageReference": {
      "offer": "UbuntuServer",
      "publisher": "Canonical",
      "sku": "16.04-LTS",
      "version": "latest"
    }
  },
  "vmPriority": "dedicated",
  "vmSize": "STANDARD_NC6"
```
## <a name="get-cluster-status"></a>Küme durumunu alma

Küme durumunun genel bakışını almak için [az batchai cluster list](/cli/azure/batchai/cluster#az_batchai_cluster_list) komutunu çalıştırın:

```azurecli
az batchai cluster list -o table
```

Çıktı aşağıdakine benzer:

```azurecli
Name        Resource Group    VM Size        State     Idle    Running    Preparing    Unusable    Leaving
---------   ----------------  -------------  -------   ------  ---------  -----------  ----------  --------
mycluster   myresourcegroup   STANDARD_NC6   steady    1       0          0            0            0
```

Daha fazla ayrıntı için [az batchai cluster show](/cli/azure/batchai/cluster#az_batchai_cluster_show) komutunu çalıştırın. Küme oluşturulduktan sonra gösterilen tüm küme özelliklerini döndürür.

Düğümler ayrıldığında ve hazırlık işlemini tamamladığında küme hazırdır (`nodeStateCounts` özniteliğine bakın). Bir sorun oluştuysa `errors` özniteliği, hata açıklamasını içerir.

## <a name="create-training-job"></a>Eğitim çalışması oluşturma

Küme hazır olduktan sonra öğrenme işini yapılandırın ve gönderin.

1. İş oluşturma için job.json adlı bir JSON şablon dosyası oluşturun:

  ```JSON
  {
    "properties": {
        "stdOutErrPathPrefix": "$AZ_BATCHAI_MOUNT_ROOT/azurefileshare",
       "inputDirectories": [{
            "id": "SAMPLE",
            "path": "$AZ_BATCHAI_MOUNT_ROOT/azurefileshare/mnistcntksample"
        }],
        "outputDirectories": [{
            "id": "MODEL",
            "pathPrefix": "$AZ_BATCHAI_MOUNT_ROOT/azurefileshare",
            "pathSuffix": "model",
            "type": "custom"
        }],
        "containerSettings": {
            "imageSourceRegistry": {
                "image": "microsoft/cntk:2.1-gpu-python3.5-cuda8.0-cudnn6.0"
            }
        },
        "nodeCount": 1,
        "cntkSettings": {
            "pythonScriptFilePath": "$AZ_BATCHAI_INPUT_SAMPLE/ConvNet_MNIST.py",
            "commandLineArgs": "$AZ_BATCHAI_INPUT_SAMPLE $AZ_BATCHAI_OUTPUT_MODEL"
        }
    }
  }
  ```
2. [az batchai job create](/cli/azure/batchai/job#az_batchai_job_create) komutuyla kümede çalıştırılacak *myjob* adlı bir iş oluşturun:

  ```azurecli
  az batchai job create --name myjob --cluster-name mycluster --config job.json
  ```

Çıktı aşağıdakine benzer:

```azurecli
{
  "caffeSettings": null,
  "chainerSettings": null,
  "cluster": {
    "id": "/subscriptions/10d0b7c6-9243-4713-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.BatchAI/clusters/mycluster",
    "resourceGroup": "myresourcegroup"
  },
  "cntkSettings": {
    "commandLineArgs": "$AZ_BATCHAI_INPUT_SAMPLE $AZ_BATCHAI_OUTPUT_MODEL",
    "configFilePath": null,
    "languageType": "Python",
    "processCount": 1,
    "pythonInterpreterPath": null,
    "pythonScriptFilePath": "$AZ_BATCHAI_INPUT_SAMPLE/ConvNet_MNIST.py"
  },
  "constraints": {
    "maxTaskRetryCount": null,
    "maxWallClockTime": "7 days, 0:00:00"
  },
  "containerSettings": {
    "imageSourceRegistry": {
      "credentials": null,
      "image": "microsoft/cntk:2.1-gpu-python3.5-cuda8.0-cudnn6.0",
      "serverUrl": null
    }
  },
  "creationTime": "2017-10-05T06:41:42.163000+00:00",
  "customToolkitSettings": null,
  "environmentVariables": null,
  "executionInfo": {
    "endTime": null,
    "errors": null,
    "exitCode": null,
    "lastRetryTime": null,
    "retryCount": null,
    "startTime": "2017-10-05T06:41:44.392000+00:00"
  },
  "executionState": "running",
  "executionStateTransitionTime": "2017-10-05T06:41:44.953000+00:00",
  "experimentName": null,
  "id": "/subscriptions/10d0b7c6-9243-4713-xxxx-xxxxxxxxxxxx/resourceGroups/demo/providers/Microsoft.BatchAI/jobs/myjob",
  "inputDirectories": [
    {
      "id": "SAMPLE",
      "path": "$AZ_BATCHAI_MOUNT_ROOT/azurefileshare/mnistcntksample"
    }
  ],
  "jobPreparation": null,
  "location": null,
  "name": "cntk_job",
  "nodeCount": 1,
  "outputDirectories": [
    {
      "createNew": true,
      "id": "MODEL",
      "pathPrefix": "$AZ_BATCHAI_MOUNT_ROOT/azurefileshare",
      "pathSuffix": "model",
      "type": "Custom"
    }
  ],
  "priority": 0,
  "provisioningState": "succeeded",
  "provisioningStateTransitionTime": "2017-10-05T06:41:44.238000+00:00",
  "resourceGroup": "demo",
  "stdOutErrPathPrefix": "$AZ_BATCHAI_MOUNT_ROOT/azurefileshare",
  "tags": null,
  "tensorFlowSettings": null,
  "toolType": "CNTK",
  "type": "Microsoft.BatchAI/Jobs"
}
```

## <a name="monitor-job"></a>İşi izleme

İş durumunun genel bakışını almak için [az batchai job list](/cli/azure/batchai/job#az_batchai_job_list) komutunu kullanın:

```azurecli
az batchai job list -o table
```

Çıktı aşağıdakine benzer:

```azurecli
Name        Resource Group    Cluster    Cluster RG      Nodes  State    Exit code
----------  ----------------  ---------  --------------- -----  -------  -----------
myjob       myresourcegroup   mycluster  myresourcegroup 1      running

```

Daha fazla ayrıntı için [az batchai job show](/cli/azure/batchai/job#az_batchai_job_show) komutunu çalıştırın.

`executionState`, işin geçerli yürütme durumunu içerir:

* `queued`: iş, küme düğümlerinin kullanılabilir olmasını bekliyor
* `running`: iş çalışıyor
*   `succeeded` (veya `failed`): iş tamamlandı ve `executionInfo`, sonuçla ilgili ayrıntıları içeriyor


## <a name="list-stdout-and-stderr-output"></a>stdout ve stderr çıktısını listeleme
stdout ve stderr günlük dosyalarının bağlantılarını listelemek için [az batchai job list-files](/cli/azure/batchai/job#az_batchai_job_list_files) komutunu kullanın:

```azurecli
az batchai job list-files --name myjob --output-directory-id stdouterr
```

Çıktı aşağıdakine benzer:

```azurecli
[
  {
    "contentLength": 733,
    "downloadUrl": "https://batchaisamples.file.core.windows.net/batchaiquickstart/10d0b7c6-9243-4713-91a9-2730375d3a1b/demo/jobs/cntk_job/stderr.txt?sv=2016-05-31&sr=f&sig=Rh%2BuTg9C1yQxm7NfA9YWiKb%2B5FRKqWmEXiGNRDeFMd8%3D&se=2017-10-05T07%3A44%3A38Z&sp=rl",
    "lastModified": "2017-10-05T06:44:38+00:00",
    "name": "stderr.txt"
  },
  {
    "contentLength": 300,
    "downloadUrl": "https://batchaisamples.file.core.windows.net/batchaiquickstart/10d0b7c6-9243-4713-91a9-2730375d3a1b/demo/jobs/cntk_job/stdout.txt?sv=2016-05-31&sr=f&sig=jMhJfQOGry9jr4Hh3YyUFpW5Uaxnp38bhVWNrTTWMtk%3D&se=2017-10-05T07%3A44%3A38Z&sp=rl",
    "lastModified": "2017-10-05T06:44:29+00:00",
    "name": "stdout.txt"
  }
]
```


## <a name="observe-output"></a>Çıktıyı inceleme

İş yürütülürken bir işin çıktı dosyalarını akışa veya kuyruğa alabilirsiniz. Aşağıdaki örnekte, stderr.txt günlüğünü akışa almak için [az batchai job stream-file](/cli/azure/batchai/job#az_batchai_job_stream_file) komutu kullanılmaktadır:

```azurecli
az batchai job stream-file --job-name myjob --output-directory-id stdouterr --name stderr.txt
```

Çıktı aşağıdakine benzer olacaktır. [Ctrl]-[C] tuşlarına basarak çıktıyı kesintiye uğratın.

```azurecli
…
Finished Epoch[2 of 40]: [Training] loss = 0.104846 * 60000, metric = 3.00% * 60000 3.849s (15588.5 samples/s);
Finished Epoch[3 of 40]: [Training] loss = 0.077043 * 60000, metric = 2.23% * 60000 3.902s (15376.7 samples/s);
Finished Epoch[4 of 40]: [Training] loss = 0.063050 * 60000, metric = 1.82% * 60000 3.811s (15743.9 samples/s);
…

```

## <a name="delete-resources"></a>Kaynakları silme

İşi silmek için [az batchai job delete](/cli/azure/batchai/job#az_batchai_job_delete) komutunu kullanın:

```azurecli
az batchai job delete --name myjob
```
Kümeyi silmek için [az batchai cluster delete](/cli/azure/batchai/cluster#az_batchai_cluster_delete) komutunu kullanın:

```azurecli
az batchai cluster delete --name mycluster
```

Bu hızlı başlangıç için oluşturduğunuz kaynak grubunu silmek için `az group delete` komutunu kullanın:

```azurecli
az group delete --name myResourceGroup
```

## <a name="restore-azure-cli-20-default-settings"></a>Azure CLI 2.0 varsayılan ayarlarını geri yükleme

Konum ve kaynak grubu için önceden yapılandırılan varsayılan ayarları kaldırın:

```azurecli
az configure --defaults group=''

az configure --defaults location=''
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure CLI’yi kullanarak bir Batch AI kümesinde CNTK eğitim işinin nasıl çalıştırılacağını öğrendiniz. Farklı araç takımları ile Batch AI uygulamasını kullanma hakkında daha fazla bilgi edinmek için [eğitim tariflerine](https://github.com/Azure/BatchAI) bakın.

Batch AI uygulamasını yönetme için Azure CLI 2.0’ı kullanma hakkında daha fazla bilgi edinmek için [github belgelerine](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md) bakın.
