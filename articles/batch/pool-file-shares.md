---
title: Azure dosya paylaşımı için Azure Batch havuzları | Microsoft Docs
description: Bir Azure dosya paylaşımının Azure Batch bir Linux veya Windows havuzdaki işlem düğümlerinden nasıl.
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 05/24/2018
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: 88d7c0d033d7b517a396df27468de8be7ae20be9
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34811755"
---
# <a name="use-an-azure-file-share-with-a-batch-pool"></a>Azure dosya paylaşımının Batch havuzuyla kullanın

[Azure dosyaları](../storage/files/storage-files-introduction.md) tam olarak yönetilen dosya paylaşımları, sunucu ileti bloğu (SMB) protokolü aracılığıyla erişilebilir bulutta sunar. Bu makalede bağlanması ve Azure dosya paylaşımının havuz işlem düğümlerinde kullanmak için bilgi ve kod örnekleri sağlar. Kod örnekleri Batch .NET ve Python SDK'ları kullanın, ancak diğer toplu SDK'lar ve araçlar kullanılarak benzer işlemler gerçekleştirebilirsiniz.

Batch, Azure Storage bloblarında kullanarak veri okumak veya yazmak için yerel API desteği sağlar. Ancak, bazı durumlarda, havuzu işlem düğümleri bir Azure dosya paylaşımına erişmek isteyebilirsiniz. Örneğin, bir SMB dosya paylaşımında bağlıdır eski bir iş yükü sahip veya paylaşılan veri erişim veya paylaşılan bir çıktı oluşturmak için görevlerinizi olması gerekir. 

## <a name="considerations-for-use-with-batch"></a>Batch ile kullanmak için dikkat edilecek noktalar

* Paralel Görevler göreli olarak az sayıda çalıştırmak havuzları varsa, Azure dosya paylaşımının kullanmayı düşünün. Gözden geçirme [performansı ve ölçeği hedefleri](../storage/files/storage-files-scale-targets.md) Azure (Azure Storage hesabı kullanır) dosyaları kullanılması gerekip gerekmediğini, verilen beklenen havuz boyutu ve varlık dosyaları sayısını belirlemek için. 

* Azure dosya paylaşımları olan [ekonomik](https://azure.microsoft.com/pricing/details/storage/files/) ve yapılandırılabilir verilerle başka bir bölgeye çoğaltma şekilde genel gereksiz. 

* Bir şirket içi bilgisayardan eşzamanlı olarak bir Azure dosya paylaşımı bağlayabilir.

* Ayrıca bkz. genel [planlama konuları](../storage/files/storage-files-planning.md) için Azure dosya paylaşımları.


## <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma

[Bir dosya paylaşımı oluşturmak](../storage/files/storage-how-to-create-file-share.md) Batch hesabınıza bağlı bir depolama hesabı ya da ayrı bir depolama hesabı.

## <a name="mount-a-share-on-a-windows-pool"></a>Bir Windows havuzunda paylaşımını bağlama

Bu bölümdeki adımları sağlar ve Windows düğümlerinin havuzunu üzerinde bağlamak ve bir Azure dosyası kullanmak için kod örnekleri paylaşın. Ek bilgiler için bkz: [belgelerine](../storage/files/storage-how-to-use-files-windows.md) Windows dosya paylaşımını Azure bağlanması için. 

Toplu işlemde her zaman Windows düğümüne bir görevi çalıştırmayı ve paylaşımı bağlamak gerekir. Şu anda Windows düğümlerinde görevler arasında ağ bağlantısı devam ettirmek olası değil.

Örneğin, bir `net use` her görev komut satırının parçası olarak dosya paylaşımını bağlamak için komutu. Dosya paylaşımını bağlamak için aşağıdaki kimlik bilgileri gereklidir:

* **Kullanıcı adı**: AZURE\\\<storageaccountname\>, örneğin, AZURE\\*mystorageaccountname*
* **Parola**: < StorageAccountKeyWhichEnds içinde == >, örneğin, *XXXXXXXXXXXXXXXXXXXXX ==*

Aşağıdaki komutu bir dosya paylaşımı bağladığı *myfileshare* depolama hesabındaki *mystorageaccountname* olarak *S:* sürücü:

```
net use S: \\mystorageaccountname.file.core.windows.net\myfileshare /user:AZURE\mystorageaccountname XXXXXXXXXXXXXXXXXXXXX==
```

Kolaylık olması için burada örnekler kimlik bilgilerini doğrudan metin olarak geçirin. Uygulamada, ortam değişkenleri, sertifikalar veya Azure anahtar kasası gibi bir çözüm kullanarak kimlik bilgilerini yönetme öneririz.

Bağlama işlemi kolaylaştırmak için isteğe bağlı olarak düğümler üzerinde kimlik bilgilerini kalıcı olmasını sağlar. Ardından, kimlik bilgileri olmadan paylaşımı bağlayabilir. Aşağıdaki iki adımı gerçekleştirin:

1. Çalıştırma `cmdkey` havuzu yapılandırmasında bir başlangıç görevi kullanarak komut satırı yardımcı programı. Bu kimlik bilgileri her bir Windows düğümde devam ettirir. Başlangıç görevi komut satırı benzer:

  ```
  cmd /c "cmdkey /add:mystorageaccountname.file.core.windows.net /user:AZURE\mystorageaccountname /pass:XXXXXXXXXXXXXXXXXXXXX=="

  ```

2. Her görev kullanan bir parçası olarak her bir düğümde paylaşımını bağlama `net use`. Örneğin, aşağıdaki görev komut satırı dosya paylaşımı olarak bağlar *S:* sürücü. Bu bir komut veya paylaşım başvuruda bulunan komut dosyası tarafından izlenmesi. Önbelleğe alınmış kimlik bilgilerini çağrısında kullanılan `net use`. Bu adım, başlangıç görevi tüm senaryolar için uygun değil havuzunda kullanılan görevler için aynı kullanıcı kimliğini kullandığınızı varsayar.

  ```
  cmd /c "net use S: \\mystorageaccountname.file.core.windows.net\myfileshare" 
  ```

### <a name="c-example"></a>C# örnek
Aşağıdaki C# örnek bir başlangıç görevi kullanarak bir Windows havuzunda kimlik kalıcı gösterilmektedir. Depolama dosya hizmet adı ve depolama kimlik tanımlı sabitler olarak geçirilir. Burada, başlangıç görevi havuzu kapsamlı bir standart (yönetici olmayan) otomatik-kullanıcı hesabı altında çalışır.

```csharp
...
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: PoolId,
    targetDedicatedComputeNodes: PoolNodeCount,
    virtualMachineSize: PoolVMSize,
    virtualMachineConfiguration: virtualMachineConfiguration);

// Start task to store credentials to mount file share
string startTaskCommandLine = String.Format("cmd /c \"cmdkey /add:{0} /user:AZURE\\{1} /pass:{2}\"", StorageFileService, StorageAccountName, StorageAccountKey);

pool.StartTask = new StartTask
{
    CommandLine = startTaskCommandLine,
    UserIdentity = new UserIdentity(new AutoUserSpecification(
        elevationLevel: ElevationLevel.NonAdmin, 
        scope: AutoUserScope.Pool))
};

pool.Commit();
```

Bu kimlik bilgilerini depolama sonra görev komut satırları ve paylaşımı bağlamak ve Paylaşım okuma başvuru ya da yazma işlemleri için kullanın. Temel bir örnek olarak, aşağıdaki kod parçacığında görev komut satırında kullanan `dir` dosya paylaşımında dosyaları listeleme komutu. Aynı kullanarak her iş görevi çalıştırıldığından emin olun [kullanıcı kimliği](batch-user-accounts.md) havuzda başlangıç görevi çalıştırmak için kullanılır. 

```csharp
...
string taskId = "myTask";
string taskCommandLine = String.Format("cmd /c \"net use {0} {1} & dir {2}\"", ShareMountPoint, StorageFileShare, ShareMountPoint);

CloudTask task = new CloudTask(taskId, taskCommandLine);
task.UserIdentity = new UserIdentity(new AutoUserSpecification(
    elevationLevel: ElevationLevel.NonAdmin,
    scope: AutoUserScope.Pool));
tasks.Add(task);
```

## <a name="mount-a-share-on-a-linux-pool"></a>Bir Linux havuzunda paylaşımını bağlama

Azure dosya paylaşımları kullanarak Linux dağıtımları içinde takılı [CIFS çekirdek istemci](https://wiki.samba.org/index.php/LinuxCIFS). Aşağıdaki örnek, Ubuntu 16.04 LTS işlem düğümlerinin havuzunda bir dosya paylaşımını bağlama gösterilmektedir. Başka bir Linux dağıtım kullanırsanız, genel adımlar benzerdir, ancak dağıtım için uygun paket yöneticisini kullanın. Ayrıntılar ve ek örnekler için bkz: [Azure dosyaları'nı Linux ile kullanma](../storage/files/storage-how-to-use-files-linux.md).

İlk olarak, bir yönetici kullanıcı kimliği altında yüklemek `cifs-utils` paketini ve bağlama noktası oluşturun (örneğin, */mnt/MyAzureFileShare*) yerel dosya sistemi içinde. Bir bağlama noktası için bir klasör dosya sisteminde herhangi bir yerden oluşturulabilir, ancak bu altında oluşturmak için ortak bir kuraldır `/mnt` klasör. Bir bağlama noktası doğrudan oluşturmamayı özen `/mnt` (üzerinde Ubuntu) veya `/mnt/resource` (üzerindeki diğer dağıtımları).

```
apt-get update && apt-get install cifs-utils && sudo mkdir -p /mnt/MyAzureFileShare
```

Daha sonra çalıştırın `mount` bu kimlik bilgilerini sağlayan dosya paylaşımını bağlamak için komutu:

* **Kullanıcı adı**: \<storageaccountname\>, örneğin, *mystorageaccountname*
* **Parola**: < StorageAccountKeyWhichEnds içinde == >, örneğin, *XXXXXXXXXXXXXXXXXXXXX ==*

Aşağıdaki komutu bir dosya paylaşımı bağladığı *myfileshare* depolama hesabındaki *mystorageaccountname* adresindeki */mnt/MyAzureFileShare*: 

```
mount -t cifs //mystorageaccountname.file.core.windows.net/myfileshare /mnt/MyAzureFileShare -o vers=3.0,username=mystorageaccountname,password=XXXXXXXXXXXXXXXXXXXXX==,dir_mode=0777,file_mode=0777,serverino && ls /mnt/MyAzureFileShare
```

Kolaylık olması için burada örnekler kimlik bilgilerini doğrudan metin olarak geçirin. Uygulamada, ortam değişkenleri, sertifikalar veya Azure anahtar kasası gibi bir çözüm kullanarak kimlik bilgilerini yönetme öneririz.

Bir Linux havuzunda tek bir başlangıç görevi bu adımların tümü birleştirebilir veya bir komut dosyasını çalıştırın. Başlangıç görevi havuzu bir yönetici kullanıcı olarak çalıştırın. Paylaşım başvuran havuzunda başka görevler çalıştırmadan önce başarıyla tamamlanması için beklenecek başlangıç göreviniz ayarlayın.

### <a name="python-example"></a>Python örneği

Aşağıdaki Python örnek bir başlangıç görevi ve paylaşımı bağlamak için bir Ubuntu havuzunu yapılandırmak nasıl gösterir. Bağlama noktası, dosya paylaşımı uç noktası ve depolama kimlik tanımlı sabitler olarak geçirilir. Başlangıç görevi havuzu kapsamlı yönetici otomatik kullanıcı hesabı altında çalışır.

```python
pool = batch.models.PoolAddParameter(
    id=pool_id,
    virtual_machine_configuration = batchmodels.VirtualMachineConfiguration(
        image_reference = batchmodels.ImageReference(
            publisher="Canonical",
            offer="UbuntuServer",
            sku="16.04.0-LTS",
            version="latest"),
        node_agent_sku_id = "batch.node.ubuntu 16.04"),
    vm_size=_POOL_VM_SIZE,
    target_dedicated_nodes=_POOL_NODE_COUNT,
    start_task=batchmodels.StartTask(
        command_line="/bin/bash -c \"apt-get update && apt-get install cifs-utils && mkdir -p {} && mount -t cifs {} {} -o vers=3.0,username={},password={},dir_mode=0777,file_mode=0777,serverino\"".format(_COMPUTE_NODE_MOUNT_POINT, _STORAGE_ACCOUNT_SHARE_ENDPOINT, _COMPUTE_NODE_MOUNT_POINT, _STORAGE_ACCOUNT_NAME, _STORAGE_ACCOUNT_KEY),
        wait_for_success=True,
        user_identity=batchmodels.UserIdentity(
            auto_user=batchmodels.AutoUserSpecification(
                scope=batchmodels.AutoUserScope.pool,
                elevation_level=batchmodels.ElevationLevel.admin)),
    )
)
batch_service_client.pool.add(pool)
```

Paylaşım bağlanması ve bir iş tanımlama sonra paylaşım, görev komut satırları kullanın. Örneğin, aşağıdaki temel komutunu kullanan `ls` dosya paylaşımında dosyaları listeleme.

```python
...
task = batch.models.TaskAddParameter(
    id='mytask',
    command_line="/bin/bash -c \"ls {}\"".format(_COMPUTE_NODE_MOUNT_POINT))

batch_service_client.task.add(job_id, task)
```


## <a name="next-steps"></a>Sonraki adımlar

* Diğer Seçenekler okuyun ve toplu veri yazmak, bkz: [Batch özelliklerine genel bakış](batch-api-basics.md) ve [iş ve Görev çıkış kalıcı](batch-task-output.md).

* Ayrıca bkz. [toplu Shipyard](https://github.com/Azure/batch-shipyard) içeren araç seti [Shipyard tarif](https://github.com/Azure/batch-shipyard/tree/master/recipes) toplu kapsayıcı iş yükleri için dosya sistemlerini dağıtmak için.