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
ms.openlocfilehash: 3601ea412790c991892a0c05210d2551810287b8
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33869028"
---
# <a name="run-a-cntk-training-job-using-the-azure-cli"></a>Azure CLI kullanarak CNTK eğitim işini çalıştırma

Azure CLI 2.0’ı kullanarak Batch AI kaynakları oluşturup yönetebilir, Batch AI dosya sunucuları ile kümelerini oluşturabilir/silebilir ve eğitim işlerini gönderebilir/sonlandırabilir/silebilir/izleyebilirsiniz.

Bu hızlı başlangıçta Microsoft Cognitive Toolkit kullanarak bir GPU kümesi oluşturma ve eğitim işi çalıştırma işlemi gösterilmektedir.

[ConvNet_MNIST.py](https://github.com/Azure/BatchAI/blob/master/recipes/CNTK/CNTK-GPU-Python/CNTK-GPU-Python.ipynb) adlı eğitim betiği, Batch AI GitHub sayfasında bulunabilir. Bu betik, MNIST el yazısı sayı veritabanındaki kıvrımlı sinir ağını eğitir.

Resmi CNTK örneği, komut satırı bağımsız değişkenleri aracılığıyla eğitim veri kümesinin konumunu ve çıkış dizini konumunu kabul edecek şekilde değiştirilmiştir.

## <a name="quickstart-overview"></a>Hızlı Başlangıca Genel Bakış

* `nc6` adıyla tek düğümlü bir GPU kümesi (VM boyutu `Standard_NC6`) oluşturun;
* İş girdi ve çıktısını depolamak için yeni bir depolama hesabı oluşturun;
* İş çıktısını ve eğitim betiklerini depolamak için `logs` ve `scripts` adlı iki klasörle bir Azure Dosya Paylaşımı oluşturun;
* Eğitim verilerini depolamak için bir Azure Blob Kapsayıcısı `data` oluşturun;
* Eğitim betiğini ve eğitim verilerini, oluşturulan dosya paylaşımına ve kapsayıcıya dağıtın;
* Azure Dosya Paylaşımı ve Azure Blob Kapsayıcısı’nı kümenin düğümüne bağlayacak şekilde işi yapılandırın ve bunları `$AZ_BATCHAI_JOB_MOUNT_ROOT/logs`, `$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts` ve `$AZ_BATCHAI_JOB_MOUNT_ROOT/data` üzerinde normal dosya sistemi olarak kullanılabilir hale getirin.
`AZ_BATCHAI_JOB_MOUNT_ROOT`, iş için Batch AI tarafından ayarlanmış bir ortam değişkenidir.
* Standart çıktısını akışla aktararak işin yürütülmesini izleyin;
* İş tamamlandıktan sonra çıktısını ve oluşturulan modelleri inceleyin;
* Sonunda, ayrılan tüm kaynakları silin.

# <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
* Sürüm 2.0.31 veya üzeri ile Azure CLI 2.0’a erişin. [Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview)’de mevcut olan Azure CLI 2.0’ı kullanabilir veya [bu yönergeleri](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) izleyerek yerel olarak yükleyebilirsiniz.

# <a name="cloud-shell-only"></a>Yalnızca Cloud Shell

Cloud Shell kullanıyorsanız, giriş dizininizde boş alan olmadığı için çalışma dizininizi `/usr/$USER/clouddrive` olarak değiştirin:

```azurecli
cd /usr/$USER/clouddrive
```

# <a name="create-a-resource-group"></a>Kaynak Grubu oluşturma

Azure kaynak grubu, Azure kaynaklarını dağıtıp yönetmeye yönelik bir mantıksal kapsayıcıdır. Aşağıdaki komut, Doğu ABD konumunda ```batchai.quickstart``` adlı yeni bir kaynak grubu oluşturur:

```azurecli
az group create -n batchai.quickstart -l eastus
```

# <a name="create-gpu-cluster"></a>GPU kümesi oluşturma

Aşağıdaki komut, işletim sistemi görüntüsü olarak Ubuntu DSVM kullanarak tek düğümlü bir GPU kümesi (VM boyutu Standard_NC6) oluşturur:

```azurecli
az batchai cluster create -n nc6 -g batchai.quickstart -s Standard_NC6 -i UbuntuDSVM -t 1 --generate-ssh-keys
```

Ubuntu DSVM, herhangi bir eğitim işini docker kapsayıcılarında çalıştırmanıza ve en popüler derin öğrenme çerçevelerini doğrudan VM üzerinde çalıştırmanıza olanak tanır.

`--generate-ssh-keys` seçeneği, henüz sahip değilseniz Azure CLI’ya özel ve genel ssh anahtarları oluşturmasını söyler. Geçerli kullanıcı adını ve oluşturulan ssh anahtarını kullanarak küme düğümlerine erişebilirsiniz.

Cloud Shell kullanıyorsanız ~/.ssh klasörünü kalıcı bir depolama alanına yedeklemeyi unutmayın.

Örnek çıktı:
```json
{
  "allocationState": "steady",
  "allocationStateTransitionTime": "2018-04-11T21:17:26.345000+00:00",
  "creationTime": "2018-04-11T20:12:10.758000+00:00",
  "currentNodeCount": 0,
  "errors": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/batchai.quickstart/providers/Microsoft.BatchAI/clusters/nc6",
  "location": "eastus",
  "name": "nc6",
  "nodeSetup": null,
  "nodeStateCounts": {
    "additionalProperties": {},
    "idleNodeCount": 0,
    "leavingNodeCount": 0,
    "preparingNodeCount": 0,
    "runningNodeCount": 0,
    "unusableNodeCount": 0
  },
  "provisioningState": "succeeded",
  "provisioningStateTransitionTime": "2018-04-11T20:12:11.445000+00:00",
  "resourceGroup": "batchai.quickstart",
  "scaleSettings": {
    "additionalProperties": {},
    "autoScale": null,
    "manual": {
      "nodeDeallocationOption": "requeue",
      "targetNodeCount": 1
    }
  },
  "subnet": null,
  "tags": null,
  "type": "Microsoft.BatchAI/Clusters",
  "userAccountSettings": {
    "additionalProperties": {},
    "adminUserName": "alex",
    "adminUserPassword": null,
    "adminUserSshPublicKey": "<YOUR SSH PUBLIC KEY HERE>"
  },
  "virtualMachineConfiguration": {
    "additionalProperties": {},
    "imageReference": {
      "additionalProperties": {},
      "offer": "linux-data-science-vm-ubuntu",
      "publisher": "microsoft-ads",
      "sku": "linuxdsvmubuntu",
      "version": "latest",
      "virtualMachineImageId": null
    }
  },
  "vmPriority": "dedicated",
  "vmSize": "STANDARD_NC6"
}
```

# <a name="create-a-storage-account"></a>Depolama Hesabı oluşturma

Aşağıdaki komut, batchai.repices kaynak grubu ile aynı bölgede yeni bir depolama hesabı oluşturur. Komutu benzer bir depolama hesabı adı ile güncelleştirin.

```azurecli
az storage account create -n <storage account name> --sku Standard_LRS -g batchai.quickstart
```

Seçili depolama hesabı adı kullanılabilir durumda değilse, yukarıdaki komut karşılık gelen hatayı bildirir. Bu durumda, başka bir ad seçip yeniden deneyin.

# <a name="data-deployment"></a>Veri Dağıtımı

## <a name="download-the-training-script-and-training-data"></a>Eğitim Betiği ve Eğitim Verilerini İndirin

* Önceden işlenmiş MNIST Veritabanını [bu konumdan](https://batchaisamples.blob.core.windows.net/samples/mnist_dataset.zip?st=2017-09-29T18%3A29%3A00Z&se=2099-12-31T08%3A00%3A00Z&sp=rl&sv=2016-05-31&sr=c&sig=PmhL%2BYnYAyNTZr1DM2JySvrI12e%2F4wZNIwCtf7TRI%2BM%3D) indirip geçerli klasöre ayıklayın.

GNU/Linux veya Cloud Shell için:

```azurecli
wget "https://batchaisamples.blob.core.windows.net/samples/mnist_dataset.zip?st=2017-09-29T18%3A29%3A00Z&se=2099-12-31T08%3A00%3A00Z&sp=rl&sv=2016-05-31&sr=c&sig=PmhL%2BYnYAyNTZr1DM2JySvrI12e%2F4wZNIwCtf7TRI%2BM%3D" -O mnist_dataset.zip
unzip mnist_dataset.zip
```

GNU/Linux dağıtımınızda yoksa `unzip` yüklemeniz gerekebileceğini unutmayın.

* [ConvNet_MNIST.py](https://raw.githubusercontent.com/Azure/BatchAI/master/recipes/CNTK/CNTK-GPU-Python/ConvNet_MNIST.py) örnek betiğini geçerli klasöre indirin:

GNU/Linux veya Cloud Shell için:

```azurecli
wget https://raw.githubusercontent.com/Azure/BatchAI/master/recipes/CNTK/CNTK-GPU-Python/ConvNet_MNIST.py
```

## <a name="create-azure-file-share-and-deploy-the-training-script"></a>Azure Dosya Paylaşımı Oluşturma ve Eğitim Betiğini Dağıtma

Aşağıdaki komutlar `scripts` ve `logs` adlı Azure Dosya Paylaşımlarını oluşturur ve eğitim betiğini `scripts` paylaşımının içindeki `cntk` klasörüne kopyalar:

```azurecli
az storage share create -n scripts --account-name <storage account name>
az storage share create -n logs --account-name <storage account name>
az storage directory create -n cntk -s scripts --account-name <storage account name>
az storage file upload -s scripts --source ConvNet_MNIST.py --path cntk --account-name <storage account name> 
```

## <a name="create-a-blob-container-and-deploy-training-data"></a>Blob Kapsayıcısı Oluşturma ve Eğitim Verilerini Dağıtma

Aşağıdaki komutlar `data` adlı Azure Blob Kapsayıcısını oluşturur ve eğitim verilerini `mnist_cntk` klasörüne kopyalar:
```azurecli
az storage container create -n data --account-name <storage account name>
az storage blob upload-batch -s . --pattern '*28x28_cntk*' --destination data --destination-path mnist_cntk --account-name <storage account name>
```

# <a name="submit-training-job"></a>Eğitim İşi Gönderme

## <a name="prepare-job-configuration-file"></a>İş Yapılandırma Dosyası Hazırlama

Aşağıdaki içeriğe sahip `job.json` adlı bir eğitim iş yapılandırma dosyası oluşturun:
```json
{
    "$schema": "https://raw.githubusercontent.com/Azure/BatchAI/master/schemas/2018-03-01/cntk.json",
    "properties": {
        "nodeCount": 1,
        "cntkSettings": {
            "pythonScriptFilePath": "$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts/cntk/ConvNet_MNIST.py",
            "commandLineArgs": "$AZ_BATCHAI_JOB_MOUNT_ROOT/data/mnist_cntk $AZ_BATCHAI_OUTPUT_MODEL"
        },
        "stdOutErrPathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/logs",
        "outputDirectories": [{
            "id": "MODEL",
            "pathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/logs"
        }],
        "mountVolumes": {
            "azureFileShares": [
                {
                    "azureFileUrl": "https://<AZURE_BATCHAI_STORAGE_ACCOUNT>.file.core.windows.net/logs",
                    "relativeMountPath": "logs"
                },
                {
                    "azureFileUrl": "https://<AZURE_BATCHAI_STORAGE_ACCOUNT>.file.core.windows.net/scripts",
                    "relativeMountPath": "scripts"
                }
            ],
            "azureBlobFileSystems": [
                {
                    "accountName": "<AZURE_BATCHAI_STORAGE_ACCOUNT>",
                    "containerName": "data",
                    "relativeMountPath": "data"
                }
            ]
        }
    }
}
```

Bu yapılandırma dosyasını şunları belirtir:

* `nodeCount` - İşin gerektirdiği düğüm sayısı (bu hızlı başlangıç için 1);
* `cntkSettings` - eğitim betiğinin ve komut satırı bağımsız değişkenlerinin yolunu belirtir. Komut satırı bağımsız değişkenleri, eğitim verilerinin yolunu ve oluşturulan modelleri depolamak için hedef yolu içerir. `AZ_BATCHAI_OUTPUT_MODEL`, çıkış dizisini yapılandırmasına göre Batch AI tarafından ayarlanan bir ortam değişkenidir (aşağıya bakın);
* `stdOutErrPathPrefix` - Batch AI hizmetinin, iş çıktısını ve günlüklerini içeren dizinler oluşturacağı yol;
* `outputDirectories` - Batch AI tarafından oluşturulacak çıkış dizinleri koleksiyonu. Her dizin için, Batch AI `AZ_BATCHAI_OUTPUT_<id>` adlı bir ortam değişkeni oluşturur. Burada `<id>`, dizin tanımlayıcısının adıdır;
* `mountVolumes` - işin yürütülmesi sırasında bağlanacak dosya sistemlerinin listesi. Dosya sistemleri `AZ_BATCHAI_JOB_MOUNT_ROOT/<relativeMountPath>` altında bağlanır. `AZ_BATCHAI_JOB_MOUNT_ROOT`, Batch AI tarafından ayarlanmış bir ortam değişkenidir;
* `<AZURE_BATCHAI_STORAGE_ACCOUNT>`, işin gönderilmesi sırasında depolama hesabı adının --storage-account-name parametresi veya bilgisayarınızdaki `AZURE_BATCHAI_STORAGE_ACCOUNT` ortam değişkeni aracılığıyla belirtileceğini söyler.

## <a name="submit-the-job"></a>İş Gönderme

Küme üzerinde işi göndermek için aşağıdaki komutu kullanın:

```azurecli
az batchai job create -n cntk_python_1 -r nc6 -g batchai.quickstart -c job.json --storage-account-name <storage account name>
```

Örnek çıktı:
```
{
  "additionalProperties": {},
  "caffeSettings": null,
  "chainerSettings": null,
  "cluster": {
    "additionalProperties": {},
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/batchai.quickstart/providers/Microsoft.BatchAI/clusters/nc6",
    "resourceGroup": "batchai.quickstart"
  },
  "cntkSettings": {
    "additionalProperties": {},
    "commandLineArgs": "$AZ_BATCHAI_JOB_MOUNT_ROOT/data/mnist_cntk $AZ_BATCHAI_OUTPUT_MODEL",
    "configFilePath": null,
    "languageType": "Python",
    "processCount": 1,
    "pythonInterpreterPath": null,
    "pythonScriptFilePath": "$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts/cntk/ConvNet_MNIST.py"
  },
  "constraints": {
    "additionalProperties": {},
    "maxWallClockTime": "7 days, 0:00:00"
  },
  "containerSettings": null,
  "creationTime": "2018-04-11T21:48:10.303000+00:00",
  "customToolkitSettings": null,
  "environmentVariables": null,
  "executionInfo": null,
  "executionState": "queued",
  "executionStateTransitionTime": "2018-04-11T21:48:10.303000+00:00",
  "experimentName": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/batchai.quickstart/providers/Microsoft.BatchAI/jobs/cntk_python_1",
  "inputDirectories": null,
  "jobOutputDirectoryPathSegment": "00000000-0000-0000-0000-000000000000/batchai.quickstart/jobs/cntk_python_1/b9576bae-e878-4fb2-9390-2e962356b5b1",
  "jobPreparation": null,
  "location": null,
  "mountVolumes": {
    "additionalProperties": {},
    "azureBlobFileSystems": [
      {
        "accountName": "<YOU STORAGE ACCOUNT NAME>",
        "additionalProperties": {},
        "containerName": "data",
        "credentials": {
          "accountKey": null,
          "accountKeySecretReference": null,
          "additionalProperties": {}
        },
        "mountOptions": null,
        "relativeMountPath": "data"
      }
    ],
    "azureFileShares": [
      {
        "accountName": "<YOU STORAGE ACCOUNT NAME>,
        "additionalProperties": {},
        "azureFileUrl": "https://<YOU STORAGE ACCOUNT NAME>.file.core.windows.net/logs",
        "credentials": {
          "accountKey": null,
          "accountKeySecretReference": null,
          "additionalProperties": {}
        },
        "directoryMode": "0777",
        "fileMode": "0777",
        "relativeMountPath": "logs"
      },
      {
        "accountName": "<YOU STORAGE ACCOUNT NAME>",
        "additionalProperties": {},
        "azureFileUrl": "https://<YOU STORAGE ACCOUNT NAME>.file.core.windows.net/scripts",
        "credentials": {
          "accountKey": null,
          "accountKeySecretReference": null,
          "additionalProperties": {}
        },
        "directoryMode": "0777",
        "fileMode": "0777",
        "relativeMountPath": "scripts"
      }
    ],
    "fileServers": null,
    "unmanagedFileSystems": null
  },
  "name": "cntk_python_1",
  "nodeCount": 1,
  "outputDirectories": [
    {
      "additionalProperties": {},
      "createNew": true,
      "id": "MODEL",
      "pathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/logs",
      "pathSuffix": null,
      "type": "custom"
    }
  ],
  "priority": 0,
  "provisioningState": "succeeded",
  "provisioningStateTransitionTime": "2018-04-11T21:48:11.577000+00:00",
  "pyTorchSettings": null,
  "resourceGroup": "batchai.quickstart",
  "secrets": null,
  "stdOutErrPathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/logs",
  "tags": null,
  "tensorFlowSettings": null,
  "toolType": "cntk",
  "type": "Microsoft.BatchAI/Jobs"
}
```

# <a name="monitor-job-execution"></a>İş Yürütmeyi İzleme

Eğitim betiği, standart çıkış dizininin içindeki `stderr.txt` dosyasında eğitim ilerleme durumunu bildirir. Aşağıdaki komutu kullanarak ilerleme durumunu izleyebilirsiniz:

```azurecli
az batchai job file stream -n cntk_python_1 -g batchai.quickstart -f stderr.txt
```

Örnek çıktı:
```
File found with URL "https://<YOU STORAGE ACCOUNT>.file.core.windows.net/logs/00000000-0000-0000-0000-000000000000/batchai.quickstart/jobs/cntk_python_1/<JOB's UUID>/stdouterr/stderr.txt?sv=2016-05-31&sr=f&sig=n86JK9YowV%2BPQ%2BkBzmqr0eud%2FlpRB%2FVu%2FFlcKZx192k%3D&se=2018-04-11T23%3A05%3A54Z&sp=rl". Start streaming
Selected GPU[0] Tesla K80 as the process wide default device.
-------------------------------------------------------------------
Build info:

        Built time: Jan 31 2018 15:03:41
        Last modified date: Tue Jan 30 03:26:13 2018
        Build type: release
        Build target: GPU
        With 1bit-SGD: no
        With ASGD: yes
        Math lib: mkl
        CUDA version: 9.0.0
        CUDNN version: 7.0.4
        Build Branch: HEAD
        Build SHA1: a70455c7abe76596853f8e6a77a4d6de1e3ba76e
        MPI distribution: Open MPI
        MPI version: 1.10.7
-------------------------------------------------------------------
Training 98778 parameters in 10 parameter tensors.

Learning rate per 1 samples: 0.001
Momentum per 1 samples: 0.0
Finished Epoch[1 of 40]: [Training] loss = 0.405960 * 60000, metric = 13.01% * 60000 21.741s (2759.8 samples/s);
Finished Epoch[2 of 40]: [Training] loss = 0.106030 * 60000, metric = 3.09% * 60000 3.638s (16492.6 samples/s);
Finished Epoch[3 of 40]: [Training] loss = 0.078542 * 60000, metric = 2.32% * 60000 3.477s (17256.3 samples/s);
...
Final Results: Minibatch[1-11]: errs = 0.54% * 10000
```

İş tamamlandığında (başarılı veya başarısız) akış durdurulur.

# <a name="inspect-generated-model-files"></a>Oluşturulan Model Dosyalarını İnceleme

İş, oluşturulan model dosyalarını `id` özniteliği `MODEL` değerine eşit olacak şekilde çıkış dizininde depolar. Aşağıdaki komutu kullanarak model dosyalarını listeleyebilir ve indirme URL’lerini alabilirsiniz:

```azurecli
az batchai job file list -n cntk_python_1 -g batchai.quickstart -d MODEL
```

Örnek çıktı:
```
[
  {
    "additionalProperties": {},
    "contentLength": 409456,
    "downloadUrl": "https://<YOUR STORAGE ACCOUNT>.file.core.windows.net/...",
    "isDirectory": false,
    "lastModified": "2018-04-11T22:05:51+00:00",
    "name": "ConvNet_MNIST_0.dnn"
  },
  {
    "additionalProperties": {},
    "contentLength": 409456,
    "downloadUrl": "https://<YOUR STORAGE ACCOUNT>.file.core.windows.net/...",
    "isDirectory": false,
    "lastModified": "2018-04-11T22:05:55+00:00",
    "name": "ConvNet_MNIST_1.dnn"
  },
...

```

Alternatif olarak, oluşturulan dosyaları incelemek için Portalı veya Azure Depolama Gezgini’ni kullanabilirsiniz. Çıktıyı farklı işlerden ayırt etmek için Batch AI her bir işe ait benzersiz bir klasör yapısı oluşturur. Çıktıyı içeren klasörün yolunu, gönderilen işin `jobOutputDirectoryPathSegment` özniteliğini kullanarak bulabilirsiniz:

```azurecli
az batchai job show -n cntk_python_1 -g batchai.quickstart --query jobOutputDirectoryPathSegment
```

Örnek çıktı:
```
"00000000-0000-0000-0000-000000000000/batchai.quickstart/jobs/cntk_python_1/<JOB's UUID>"
```

# <a name="delete-resources"></a>Kaynakları Silme

Kaynak grubunu ve tüm ayrılmış kaynakları aşağıdaki komutla silin:

```azurecli
az batchai group delete -n batchai.quickstart -y
```
