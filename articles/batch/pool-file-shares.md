---
title: Azure dosya paylaşımı için Azure Batch havuzlarında | Microsoft Docs
description: Azure batch'te bir Linux veya Windows havuzdaki işlem düğümlerinden bir Azure dosya paylaşımını bağlayabilmeniz nasıl.
services: batch
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 05/24/2018
ms.author: lahugh
ms.custom: ''
ms.openlocfilehash: 914bc11736b08dab6b334307dc188b5d153c7331
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341313"
---
# <a name="use-an-azure-file-share-with-a-batch-pool"></a>Bir Azure dosya paylaşımı ile bir Batch havuzu kullanma

[Azure dosyaları](../storage/files/storage-files-introduction.md) tam olarak yönetilen sunucu ileti bloğu (SMB) protokolü aracılığıyla erişilebilir bulut dosya paylaşımları sunar. Bu makalede, bağlama ve bir Azure dosya paylaşımı üzerinde işlem düğümleri havuzu kullanmak için bilgi ve kod örnekleri sağlar. Kod örnekleri Batch .NET ve Python SDK'ları kullanın, ancak diğer Batch SDK'ları ve araçları kullanarak benzer işlemleri gerçekleştirebilirsiniz.

Batch, veri okuma ve yazma için Azure depolama BLOB'ları kullanmak için yerel API desteği sağlar. Ancak, bazı durumlarda, işlem düğümleri havuzu bir Azure dosya paylaşımı erişim isteyebilirsiniz. Örneğin, sahip olduğunuz bir SMB dosya paylaşımında bağlı eski bir iş yükü veya görevlerinizi Paylaşılan verilere erişmek veya paylaşılan çıkış üretmesi gerekir. 

## <a name="considerations-for-use-with-batch"></a>Batch ile kullanmak için dikkat edilmesi gerekenler

* Paralel Görevler görece az sayıda çalışan havuzları varsa, bir Azure dosya paylaşımı kullanmayı düşünün. Gözden geçirme [performans ve ölçeklendirme hedeflerini](../storage/files/storage-files-scale-targets.md) Azure dosyaları (Bu bir Azure depolama hesabını kullanır) kullanılması gerekip gerekmediğini, verilen beklenen havuz boyutunu ve varlık dosyalarının sayısını belirlemek için. 

* Azure dosya paylaşımları [maliyet açısından verimli](https://azure.microsoft.com/pricing/details/storage/files/) ve yapılandırılabilir verilerle başka bir bölgeye çoğaltma şekilde Global yedekli. 

* Bir şirket içi bilgisayardan eşzamanlı olarak bir Azure dosya paylaşımı bağlayabilir.

* Ayrıca bkz. genel [planlama konuları](../storage/files/storage-files-planning.md) için Azure dosya paylaşımları.


## <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma

[Dosya paylaşımı oluşturma](../storage/files/storage-how-to-create-file-share.md) Batch hesabınıza bağlı bir depolama hesabı veya ayrı bir depolama hesabı.

## <a name="mount-a-share-on-a-windows-pool"></a>Bir Windows havuzu paylaşımında bağlama

Bu bölümdeki adımları sağlar ve bir Windows düğümleri havuzu üzerinde bağlamak ve bir Azure dosyası kullanmak için kod örnekleri paylaşın. Ek bilgiler için bkz. [belgeleri](../storage/files/storage-how-to-use-files-windows.md) Linux'tan bir Azure dosya paylaşımını Windows üzerinde. 

Toplu işlemde her zaman bir Windows düğümde görev çalıştırılır ve paylaşımı bağlamak gerekir. Şu anda, Windows düğümlerindeki görevler arasında ağ bağlantısı kalıcı hale getirmek mümkün değildir.

Örneğin, bir `net use` her görev komut satırının bir parçası dosya paylaşımını bağlamak için komutu. Dosya paylaşımını bağlayabilmeniz için aşağıdaki kimlik bilgilerini gerekir:

* **Kullanıcı adı**: AZURE\\\<storageaccountname\>, örneğin, AZURE\\*mystorageaccountname*
* **Parola**: < StorageAccountKeyWhichEnds içinde == > Örneğin, *XXXXXXXXXXXXXXXXXXXXX ==*

Aşağıdaki komutu bir dosya paylaşımı bağlar *myfileshare* depolama hesabındaki *mystorageaccountname* olarak *S:* sürücü:

```
net use S: \\mystorageaccountname.file.core.windows.net\myfileshare /user:AZURE\mystorageaccountname XXXXXXXXXXXXXXXXXXXXX==
```

Kolaylık olması için buradaki örnekler kimlik bilgilerini doğrudan metin geçirin. Uygulamada, ortam değişkenleri, sertifikaları veya Azure anahtar kasası gibi bir çözüm kullanarak kimlik bilgilerinin yönetimiyle öneririz.

Bağlama işlemi basitleştirmek için isteğe bağlı olarak düğümlerinde kimlik bilgilerini kalıcı hale getirin. Ardından, kimlik bilgileri olmadan paylaşımı bağlayabilir. Aşağıdaki iki adımı uygulayın:

1. Çalıştırma `cmdkey` havuz yapılandırmasına bir başlangıç görevi kullanarak komut satırı yardımcı programı. Bu, her bir düğümde Windows kimlik bilgilerini devam ettirir. Başlangıç görevi komut satırı benzer:

   ```
   cmd /c "cmdkey /add:mystorageaccountname.file.core.windows.net /user:AZURE\mystorageaccountname /pass:XXXXXXXXXXXXXXXXXXXXX=="

   ```

2. Her görevi kullanılarak bir parçası olarak her bir düğümde paylaşımını `net use`. Örneğin, aşağıdaki görev komut satırı dosya paylaşımı olarak bağlar *S:* sürücü. Bu, bir komut veya paylaşım başvuran bir betik tarafından takip. Önbelleğe alınmış kimlik bilgilerini çağrısında kullanılan `net use`. Bu adım, görev başlatma tüm senaryolar için uygun olmayan havuzunda kullanılan görevler için aynı kullanıcı kimliğini kullanarak varsayar.

   ```
   cmd /c "net use S: \\mystorageaccountname.file.core.windows.net\myfileshare" 
   ```

### <a name="c-example"></a>C# örneği
Aşağıdaki C# örnek, bir başlangıç görevi kullanarak bir Windows havuzu şirket kimlik bilgilerini kalıcı hale getirmek nasıl gösterir. Depolama dosya hizmeti adı ve depolama kimlik bilgileri, tanımlı sabitler geçirilir. Burada, başlangıç görevinin havuzu kapsamlı bir standart (yönetici olmayan) otomatik kullanıcı hesabı altında çalışır.

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

Kimlik bilgilerini depolama sonrasında, görev komut satırları paylaşımını ve Paylaşım başvuru okuma veya yazma işlemleri için kullanın. Temel bir örnek olarak, aşağıdaki kod parçacığı görev komut satırında kullanan `dir` Dosya paylaşımındaki dosyaları Listele komutu. Aynı kullanarak her iş görevi çalıştırıldığından emin olun [kullanıcı kimliğini](batch-user-accounts.md) başlangıç görevi, havuzda çalıştırmak için kullanılır. 

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

## <a name="mount-a-share-on-a-linux-pool"></a>Linux havuzu paylaşımında bağlama

Azure dosya paylaşımlarını kullanarak Linux dağıtımları bağlanabilir [CIFS çekirdek istemci](https://wiki.samba.org/index.php/LinuxCIFS). Aşağıdaki örnekte, Ubuntu 16.04 LTS işlem düğümlerinin havuzunda bir dosya paylaşımını bağlama gösterilmektedir. Farklı bir Linux dağıtımı kullanıyorsanız genel adımlar benzerdir, ancak dağıtım için uygun paket yöneticisini kullanın. Ayrıntılar ve ek örnekler için bkz. [Linux ile Azure dosyaları kullan](../storage/files/storage-how-to-use-files-linux.md).

İlk olarak, bir yönetici kullanıcı kimliğini altında yükleyin `cifs-utils` paketini ve bağlama noktası oluştur (örneğin, */mnt/MyAzureFileShare*), yerel dosya sistemi. Bir bağlama noktası için bir klasör dosya sisteminde herhangi bir yere oluşturulabilir, ancak işbu sözleşmenin oluşturmak için ortak bir kuraldır `/mnt` klasör. Bir bağlama noktası doğrudan oluşturmamayı mutlaka `/mnt` (Ubuntu üzerinde) veya `/mnt/resource` (üzerinde diğer dağıtımları).

```
apt-get update && apt-get install cifs-utils && sudo mkdir -p /mnt/MyAzureFileShare
```

Ardından çalıştırın `mount` bu kimlik bilgilerini girerek dosya paylaşımını bağlayabilmeniz için komut:

* **Kullanıcı adı**: \<storageaccountname\>, örneğin, *mystorageaccountname*
* **Parola**: < StorageAccountKeyWhichEnds içinde == > Örneğin, *XXXXXXXXXXXXXXXXXXXXX ==*

Aşağıdaki komutu bir dosya paylaşımı bağlar *myfileshare* depolama hesabındaki *mystorageaccountname* adresindeki */mnt/MyAzureFileShare*: 

```
mount -t cifs //mystorageaccountname.file.core.windows.net/myfileshare /mnt/MyAzureFileShare -o vers=3.0,username=mystorageaccountname,password=XXXXXXXXXXXXXXXXXXXXX==,dir_mode=0777,file_mode=0777,serverino && ls /mnt/MyAzureFileShare
```

Kolaylık olması için buradaki örnekler kimlik bilgilerini doğrudan metin geçirin. Uygulamada, ortam değişkenleri, sertifikaları veya Azure anahtar kasası gibi bir çözüm kullanarak kimlik bilgilerinin yönetimiyle öneririz.

Bir Linux havuzu üzerinde tek bir başlangıç görevinde bu adımların tümü birleştirebilir veya bir betikte çalıştırabilirsiniz. Başlangıç görevi, havuzda bir yönetici kullanıcı olarak çalıştırın. Paylaşım başvuran havuzda başka bir görev çalıştırmadan önce başarıyla tamamlanması için beklenecek başlangıç göreviniz ayarlayın.

### <a name="python-example"></a>Python örnek

Aşağıdaki örnek Python görev başlatma ve paylaşımı bağlamak için bir Ubuntu havuzunu yapılandırmak nasıl gösterir. Bağlama noktası, dosya paylaşımı uç noktası ve depolama kimlik bilgileri, tanımlı sabitler geçirilir. Başlangıç görevi, havuzu kapsamlı yönetici otomatik kullanıcı hesabı altında çalışır.

```python
pool = batch.models.PoolAddParameter(
    id=pool_id,
    virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
        image_reference=batchmodels.ImageReference(
            publisher="Canonical",
            offer="UbuntuServer",
            sku="16.04.0-LTS",
            version="latest"),
        node_agent_sku_id="batch.node.ubuntu 16.04"),
    vm_size=_POOL_VM_SIZE,
    target_dedicated_nodes=_POOL_NODE_COUNT,
    start_task=batchmodels.StartTask(
        command_line="/bin/bash -c \"apt-get update && apt-get install cifs-utils && mkdir -p {} && mount -t cifs {} {} -o vers=3.0,username={},password={},dir_mode=0777,file_mode=0777,serverino\"".format(
            _COMPUTE_NODE_MOUNT_POINT, _STORAGE_ACCOUNT_SHARE_ENDPOINT, _COMPUTE_NODE_MOUNT_POINT, _STORAGE_ACCOUNT_NAME, _STORAGE_ACCOUNT_KEY),
        wait_for_success=True,
        user_identity=batchmodels.UserIdentity(
            auto_user=batchmodels.AutoUserSpecification(
                scope=batchmodels.AutoUserScope.pool,
                elevation_level=batchmodels.ElevationLevel.admin)),
    )
)
batch_service_client.pool.add(pool)
```

Paylaşımı bağlayarak ve bir iş tanımlama sonra paylaşım, görev komut satırlarında kullanın. Örneğin, aşağıdaki temel komutu kullanan `ls` Dosya paylaşımındaki dosyaları listeleme.

```python
...
task = batch.models.TaskAddParameter(
    id='mytask',
    command_line="/bin/bash -c \"ls {}\"".format(_COMPUTE_NODE_MOUNT_POINT))

batch_service_client.task.add(job_id, task)
```


## <a name="next-steps"></a>Sonraki adımlar

* Diğer seçenekleri okuyup Batch'de veri yazmak, bkz [Batch özelliğine genel bakış](batch-api-basics.md) ve [iş ve görev çıktılarını kalıcı hale getirme](batch-task-output.md).

* Ayrıca bkz: [Batch Shipyard](https://github.com/Azure/batch-shipyard) içeren araç seti [Shipyard tarifleri](https://github.com/Azure/batch-shipyard/tree/master/recipes) Batch kapsayıcı iş yükleri için dosya sistemlerine dağıtılacak.