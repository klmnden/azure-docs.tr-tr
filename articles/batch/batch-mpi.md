---
title: Azure Batch MPI uygulamaları - çalıştırmak için çok örnekli görevleri kullanma | Microsoft Docs
description: Çok örnekli görev türü kullanarak Azure Batch'de ileti geçirme arabirimi (MPI) uygulamaları çalıştırma hakkında bilgi edinin.
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: 83e34bd7-a027-4b1b-8314-759384719327
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.date: 5/22/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0fb5ea21c6403369cbcb60df58c0f70a57a61d4e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="use-multi-instance-tasks-to-run-message-passing-interface-mpi-applications-in-batch"></a>Batch'de ileti geçirme arabirimi (MPI) uygulamalarını çalıştırmak için çok örnekli görevleri kullanma

Çok örnekli görevler, bir Azure Batch görev birden çok işlem düğümlerinde aynı anda çalışmasına olanak tanır. Bu görevler, ileti geçirme arabirimi (MPI) uygulamalarını toplu gibi senaryoları yüksek performans sağlar. Bu makalede, kullanarak çok örnekli görevleri çalıştırma hakkında bilgi edinin [Batch .NET] [ api_net] kitaplığı.

> [!NOTE]
> Batch .NET, MS-MPI, bu makaledeki örneklerde odaklanmanıza ve Windows işlem düğümleri olsa da, burada açıklanan çok örnekli görev kavramlar, diğer platformlar ve teknolojiler (Python ve Linux düğümleri, örneğin üzerinde Intel MPI) için geçerlidir.
>
>

## <a name="multi-instance-task-overview"></a>Çok örnekli görev genel bakış
Toplu işlemde her normalde bir görevdir tek işlem düğümü üzerinde--yürütülen bir iş birden çok görevler gönderebilir ve toplu işlem hizmetinin her görev bir düğümde yürütülmek zamanlar. Ancak, bir görevin yapılandırma tarafından **çok örnekli ayarları**, bunun yerine bir birincil görev ve sonra birden çok düğümde yürütülen çeşitli görevleri oluşturmak üzere toplu söyleyin.

![Çok örnekli görev genel bakış][1]

Bir iş çok örnekli ayarlarla bir görev gönderdiğinizde, toplu iş çok örnekli görevleri benzersiz çeşitli adımları gerçekleştirir:

1. Batch hizmeti bir oluşturur **birincil** ve birkaç **görevleri** çok örnekli ayarlara göre. Görevler (tüm alt birincil) toplam sayısı sayısıyla eşleştiğini **örnekleri** (işlem düğümleri) çok örnekli ayarlarında belirtin.
2. Toplu işlem düğümleri olarak birini atar **ana**ve ana yürütmek için birincil görev zamanlar. Çok örnekli görev, bir alt düğüm başına ayrılan işlem düğümleri geri kalanı yürütmek için alt görevler zamanlar.
3. Birincil ve tüm alt görevler herhangi indirme **ortak kaynak dosyaları** çok örnekli ayarlarında belirtin.
4. Sonra ortak kaynak dosyaları yüklenmiş, birincil ve alt görevleri yürütme **koordinasyon komutu** çok örnekli ayarlarında belirtin. Düzenleme komutu genellikle görev yürütmek için düğümleri hazırlamak için kullanılır. Bu arka plan Hizmetleri başlatma içerebilir (gibi [Microsoft MPI][msmpi_msdn]'s `smpd.exe`) ve düğümlerin düğümler arası iletileri işlemek hazır olduğunu doğrulama.
5. Birincil görevi yürütür **uygulama komutu** ana düğüm üzerinde *sonra* koordinasyon komutu başarıyla birincil ve tüm alt görevler tarafından tamamlandı. Uygulama komutu çok örnekli görev komut satırı ve yalnızca birincil görev tarafından yürütülen. İçinde bir [MS MPI][msmpi_msdn]-tabanlı çözümün, burada kullanarak MPI özellikli uygulamanızı yürütme budur `mpiexec.exe`.

> [!NOTE]
> İşlevsel olarak ayrı olsa da, "çok örnekli görev" gibi benzersiz görev türü değil [StartTask] [ net_starttask] veya [JobPreparationTask][net_jobprep]. Çok örnekli görev yalnızca standart bir Batch görevinde olduğu ([CloudTask] [ net_task] Batch .NET içinde), çok örnekli ayarları yapılandırılır. Bu makalede, biz bu başvurmak **çok örnekli görev**.
>
>

## <a name="requirements-for-multi-instance-tasks"></a>Çok örnekli görevler için gereksinimleri
Çok örnekli görevleri gerektiren bir havuzla **etkin düğümler arası iletişim**ile **eşzamanlı görev yürütme devre dışı**. Eşzamanlı görev yürütme devre dışı bırakmak için ayarlanmış [CloudPool.MaxTasksPerComputeNode](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool#Microsoft_Azure_Batch_CloudPool_MaxTasksPerComputeNode) özelliği 1.

> [!NOTE]
> Toplu [sınırları](batch-quota-limit.md#other-limits) düğümler arası iletişimin etkin olan bir havuz boyutu.


Bu kod parçacığını bir havuz Batch .NET kitaplığını kullanarak çok örnekli görevleri için oluşturulacağını gösterir.

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicatedComputeNodes: 3
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.MaxTasksPerComputeNode = 1;
```

> [!NOTE]
> Düğümler arası iletişim devre dışı veya ile bir havuzda çok örnekli görev çalıştırmayı denerseniz bir *maxTasksPerNode* 1'den büyük değer, görev hiçbir zaman zamanlanmış--süresiz olarak "etkin" durumda kalır. 
>
> Çok örnekli görevler yalnızca 14 Aralık 2015 tarihinden sonra oluşturulan havuzlarında düğümlerinde yürütebilir.
>
>

### <a name="use-a-starttask-to-install-mpi"></a>MPI yüklemek için StartTask kullanın
Çok örnekli görev MPI uygulamaları çalıştırmak için önce havuzundaki işlem düğümlerinde MPI uygulaması (MS-MPI veya örneğin Intel MPI) yüklemeniz gerekir. Bu kullanmak için iyi bir zamandır bir [StartTask][net_starttask], her bir düğümün bir havuzuna katılır veya yeniden yürütür. Bu kod parçacığını MS MPI kurulum paketi olarak belirten bir StartTask oluşturur bir [kaynak dosyası][net_resourcefile]. Başlangıç görevinin komut satırı kaynak dosyası düğüme İndirildikten sonra yürütülür. Bu durumda, komut satırı katılımsız yükleme MS MPI işlemi gerçekleştirir.

```csharp
// Create a StartTask for the pool which we use for installing MS-MPI on
// the nodes as they join the pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin)),
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit the fully configured pool to the Batch service to actually create
// the pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a>Doğrudan uzak bellek erişimi (RDMA)
Seçeneğini belirlediğinizde bir [RDMA özellikli boyutu](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) , Batch havuzundaki işlem düğümleri için A9 gibi MPI uygulamanızı Azure'nın yüksek performans, düşük gecikme süreli doğrudan uzak bellek erişimi (RDMA) ağ avantajından yararlanabilirsiniz.

Aşağıdaki makalelerde "RDMA özellikli" belirtilen boyutlarını arayın:

* **CloudServiceConfiguration** havuzları

  * [Cloud Services boyutları](../cloud-services/cloud-services-sizes-specs.md) (yalnızca Windows)
* **VirtualMachineConfiguration** havuzları

  * [Azure sanal makineler için Boyutlar](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux)
  * [Azure sanal makineler için Boyutlar](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows)

> [!NOTE]
> RDMA yararlanmak [Linux işlem düğümlerini](batch-linux-nodes.md), kullanmalısınız **Intel MPI** düğümler üzerinde. 
>

## <a name="create-a-multi-instance-task-with-batch-net"></a>Çok örnekli görev Batch .NET ile oluşturma
Biz MPI paket yükleme ve havuzu gereksinimlerini ele, çok örnekli görev oluşturalım. Bu parçacığında, bir standart oluşturuyoruz [CloudTask][net_task], ardından yapılandırma kendi [MultiInstanceSettings] [ net_multiinstance_prop] özelliği. Daha önce belirtildiği gibi çok örnekli görev farklı görev türü, ancak çok örnekli ayarlarla yapılandırılan standart bir Batch görevinde değil.

```csharp
// Create the multi-instance task. Its command line is the "application command"
// and will be executed *only* by the primary, and only after the primary and
// subtasks execute the CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure the task's MultiInstanceSettings. The CoordinationCommandLine will be executed by
// the primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit the task to the job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on the nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a>Birincil görevi ve alt görevler
Bir görev için çok örnekli ayarları oluşturduğunuzda, görev yürütmek üzere olduğunuz işlem düğümü sayısını belirtin. Bir iş görevi gönderdiğinizde, Batch hizmeti bir oluşturur **birincil** görev ve yeterli **görevleri** belirttiğiniz düğüm sayısını birlikte eşleşmesi.

Bu görevler için 0 aralığında bir tamsayı kimliği atanır *numberOfInstances* - 1. Kimliği 0 olan görev birincil bir görevdir ve diğer tüm kimliklerini görevleridir. Örneğin, bir görev için aşağıdaki çok örnekli ayarları oluşturursanız, birincil görev 0 bir kimliğe sahip ve alt görevler kimlikleri 1 ile 9 arasında olması gerekir.

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a>Ana düğüm
Çok örnekli görev gönderdiğinizde, Batch hizmeti işlem düğümlerinden biri olarak "Yönetici" düğümü atar ve ana düğümde yürütmek için birincil görev zamanlar. Alt görevler için çok örnekli görev ayrılan düğümler kalanı yürütmek üzere zamanlanır.

## <a name="coordination-command"></a>Düzenleme komutu
**Koordinasyon komutu** birincil tarafından yürütülen ve alt görevlerin.

Düzenleme komutu çağırma engelleme--toplu düzenleme komutu için tüm görevleri başarıyla verdi kadar uygulama komutu yürütülmez. Düzenleme komutu bu nedenle tüm gerekli arka plan hizmetleri başlatmak, kullanıma hazır olduğunu doğrulayın ve sonra çıkın. Örneğin, bu düzenleme komut 7 düğümde SMPD hizmeti başlatılır MS MPI sürümünü kullanan bir çözüm için çıkar:

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

Kullanımına dikkat edin `start` bu koordinasyon komutu. Bu gereklidir çünkü `smpd.exe` uygulama hemen yürütme sonrasında döndürmez. Kullanmadan [Başlat] [ cmd_start] komutu, bu düzenleme komut değil döndürür ve bu nedenle uygulama komutu çalışmasını engeller.

## <a name="application-command"></a>Uygulama komutu
Düzenleme komutu yürütülürken birincil görev ve tüm görevleri tamamladıktan sonra çok örnekli görevin komut satırı birincil görev tarafından yürütülen *yalnızca*. Bu diyoruz **uygulama komutu** koordinasyon komuttan ayırt etmek için.

MS-MPI uygulamaları için uygulama MPI etkin uygulamanızla yürütmek için komutunu `mpiexec.exe`. Örneğin, MS-MPI sürüm 7 kullanarak bir çözüm için bir uygulama komutu şöyledir:

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

> [!NOTE]
> Çünkü MS-MPI's `mpiexec.exe` kullanan `CCP_NODES` değişken varsayılan olarak (bkz [ortam değişkenleri](#environment-variables)), yukarıdaki örnek uygulama komut satırı dışlar.
>
>

## <a name="environment-variables"></a>Ortam değişkenleri
Toplu oluşturur birkaç [ortam değişkenleri] [ msdn_env_var] çok örnekli görevler için çok örnekli görev ayrılan işlem düğümlerinde özgüdür. Komut dosyaları ve bunların yürütme programları gibi düzenleme ve uygulama komut satırları bu ortam değişkenleri başvuruda bulunabilir.

Aşağıdaki ortam değişkenleri çok örnekli görevler tarafından kullanılmak üzere Batch hizmeti tarafından oluşturulur:

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

Düğüm ortam değişkenleri, görünürlük ve içeriği de dahil olmak üzere bunlar üzerinde tam Ayrıntılar ve diğer toplu işlem için bkz: [işlem düğümü ortam değişkenleri][msdn_env_var].

> [!TIP]
> Toplu Linux MPI kod örneği, bu ortam değişkenleri çeşitli nasıl kullanılabileceğini örneği içerir. [Koordinasyon cmd] [ coord_cmd_example] Bash betiğini indirir ortak uygulama ve giriş dosyaları Azure Storage'dan, ana düğüm ağ dosya sistemi (NFS) paylaşımında etkinleştirir ve NFS istemcisi olarak çok örnekli görev ayrılan diğer düğümlere yapılandırır.
>
>

## <a name="resource-files"></a>Kaynak dosyaları
Kaynak dosyaları için çok örnekli görevler dikkate alınması gereken iki kümesi vardır: **ortak kaynak dosyaları** , *tüm* görevleri indirin (hem birincil hem de ve alt görevler) ve **kaynak dosyaları** çok örnekli görev için kendisini, belirtilen *yalnızca birincil* görev yüklemeleri.

Bir veya daha fazla belirtebilirsiniz **ortak kaynak dosyaları** bir görev için çok örnekli ayarlarında. Bu ortak kaynak dosyaları karşıdan yüklenir [Azure Storage](../storage/common/storage-introduction.md) her düğümün içine **görev paylaşılan dizine** birincil ve tüm alt görevler. Kullanarak uygulama ve düzenleme komut satırlarından görev paylaşılan dizine erişebilir `AZ_BATCH_TASK_SHARED_DIR` ortam değişkeni. `AZ_BATCH_TASK_SHARED_DIR` Yolu için çok örnekli görev ayrılan her düğümde aynı, böylece bir tek koordinasyon komutu birincil ve tüm alt görevler arasında paylaşabilirsiniz. Toplu "bir uzaktan erişim fikir dizininde paylaşmaz", ancak bağlama kullanın ya da ortam değişkenleri ipucu önceki bölümünde belirtildiği gibi noktası paylaşın.

Çok örnekli görev için belirttiğiniz kaynak dosyaları, görevin çalışma dizinine yüklenir `AZ_BATCH_TASK_WORKING_DIR`, varsayılan olarak. , Sık kullanılan kaynak dosyaları aksine belirtildiği gibi yalnızca birincil görev çok örnekli görev kendisi için belirtilen kaynak dosyaları indirir.

> [!IMPORTANT]
> Her zaman ortam değişkenlerini kullanma `AZ_BATCH_TASK_SHARED_DIR` ve `AZ_BATCH_TASK_WORKING_DIR` bu dizinleri, komut satırında başvurmak için. Yollarını el ile oluşturmak çalışmayın.
>
>

## <a name="task-lifetime"></a>Görev yaşam süresi
Birincil görev ömrü tüm çok örnekli görev ömrü denetler. Birincil çıktığında tüm görevleri sonlandırılır. Birincil çıkış kodu, görevin çıkış kodu ve bu nedenle başarı veya başarısızlık görev yeniden deneme amaçlı belirlemek için kullanılır.

Herhangi bir alt görevler başarısız olursa, sıfır olmayan dönüş koduyla çıkma Örneğin, tüm çok örnekli görev başarısız olur. Çok örnekli görev sonra sona erdi ve, yeniden deneme sınırına kadar denenecek.

Çok örnekli Görev sildiğinizde, birincil ve tüm alt görevler de Batch hizmeti tarafından silinir. Tüm dizinleri eklemeli ve dosyalarına yalnızca standart bir görev için bilgi işlem düğümleri silinir.

[TaskConstraints] [ net_taskconstraints] için çok örnekli görev gibi [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime] [ net_taskconstraint_maxwallclock], ve [RetentionTime] [ net_taskconstraint_retention] bunlar için standart bir görev ve birincil ve tüm alt görevleri uygulamak gibi özelliklerini dikkate alınır. Ancak, değiştirirseniz [RetentionTime] [ net_taskconstraint_retention] özelliği bu değişiklik iş için çok örnekli görev ekledikten sonra yalnızca birincil göreve uygulanır. Tüm alt görevlerin özgün kullanmaya devam [RetentionTime][net_taskconstraint_retention].

Yeni görev çok örnekli görev parçası ise, bir işlem düğümün son kullanılan görevler listesi alt görev kimliği yansıtır.

## <a name="obtain-information-about-subtasks"></a>Alt görevler hakkında bilgi edinin
Batch .NET kitaplığını kullanarak görevleri hakkında bilgi edinmek için arama [CloudTask.ListSubtasks] [ net_task_listsubtasks] yöntemi. Bu yöntem, tüm alt görevler hakkında bilgi ve görevler yürütülen işlem düğümü hakkında bilgi döndürür. Bu bilgilerden, her alt ait kök dizini, havuz kimliğini, geçerli durumu, çıkış kodu ve daha fazla belirleyebilirsiniz. Bu bilgi ile birlikte kullanabileceğiniz [PoolOperations.GetNodeFile] [ poolops_getnodefile] alt görevi'nin dosyaları elde etmek için yöntemi. Bu yöntem (kimliği 0) birincil görev için bilgi döndürmüyor unutmayın.

> [!NOTE]
> Aksi belirtilmediği sürece, Batch .NET yöntemleri, çalışan çok örneğinde [CloudTask] [ net_task] kendisini uygulamak *yalnızca* birincil görev. Örneğin, size çağırdığınızda [CloudTask.ListNodeFiles] [ net_task_listnodefiles] çok örnekli görev yöntemi, yalnızca birincil görev dosyaları döndürülür.
>
>

Aşağıdaki kod parçacığını gibi alt bilgi elde üzerinde yürütülen düğümlerden dosya içeriklerini isteği gösterilmektedir.

```csharp
// Obtain the job and the multi-instance task from the Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain the list of subtasks for the task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over the subtasks and print their stdout and stderr
// output if the subtask has completed
await subtasks.ForEachAsync(async (subtask) =>
{
    Console.WriteLine("subtask: {0}", subtask.Id);
    Console.WriteLine("exit code: {0}", subtask.ExitCode);

    if (subtask.State == SubtaskState.Completed)
    {
        ComputeNode node =
            await batchClient.PoolOperations.GetComputeNodeAsync(subtask.ComputeNodeInformation.PoolId,
                                                                 subtask.ComputeNodeInformation.ComputeNodeId);

        NodeFile stdOutFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardOutFileName);
        NodeFile stdErrFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardErrorFileName);
        stdOut = await stdOutFile.ReadAsStringAsync();
        stdErr = await stdErrFile.ReadAsStringAsync();

        Console.WriteLine("node: {0}:", node.Id);
        Console.WriteLine("stdout.txt: {0}", stdOut);
        Console.WriteLine("stderr.txt: {0}", stdErr);
    }
    else
    {
        Console.WriteLine("\tSubtask {0} is in state {1}", subtask.Id, subtask.State);
    }
});
```

## <a name="code-sample"></a>Kod örneği
[MultiInstanceTasks] [ github_mpi] kodu örneği github'daki çok örnekli görev çalıştırmak için nasıl kullanılacağını gösteren bir [MS MPI] [ msmpi_msdn] uygulama batch işlem düğümlerinde. Adımları [hazırlık](#preparation) ve [yürütme](#execution) örneği çalıştırmak için.

### <a name="preparation"></a>Hazırlama
1. İlk iki adımları [derlemek ve basit bir MS-MPI program çalıştırmak nasıl][msmpi_howto]. Aşağıdaki adımı önkoşulları karşılar.
2. Derleme bir *sürüm* sürümü [MPIHelloWorld] [ helloworld_proj] örnek MPI programı. Bu işlem düğümlerinde çok örnekli görev tarafından çalıştırılacak programıdır.
3. İçeren bir zip dosyası oluşturma `MPIHelloWorld.exe` (hangi, 2. adım yerleşik) ve `MSMpiSetup.exe` (hangi indirdiğiniz 1. adım). Sonraki adımda bir uygulama paketi olarak bu zip dosyasını yükleyeceksiniz.
4. Kullanım [Azure portal] [ portal] toplu oluşturmak için [uygulama](batch-application-packages.md) "MPIHelloWorld" olarak adlandırılan ve önceki adımda oluşturduğunuz uygulama paketi "1.0" sürümü olarak zip dosyası belirtin. Bkz: [karşıya yükleyin ve uygulamalarını yönetin](batch-application-packages.md#upload-and-manage-applications) daha fazla bilgi için.

> [!TIP]
> Derleme bir *sürüm* sürümü `MPIHelloWorld.exe` böylece ek bağımlılıkları içerecek şekilde yoksa (örneğin, `msvcp140d.dll` veya `vcruntime140d.dll`) uygulama paketi.
>
>

### <a name="execution"></a>Yürütme
1. Karşıdan [azure-batch-samples] [ github_samples_zip] github'dan.
2. MultiInstanceTasks açmak **çözüm** Visual Studio 2015'te ya da daha yeni. `MultiInstanceTasks.sln` Çözüm dosyasını bulunur:

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`
3. Batch ve Storage hesabı kimlik bilgilerinizi girin `AccountSettings.settings` içinde **öğesini kullanıma alın** projesi.
4. **Derleme ve çalıştırma** MultiInstanceTasks çözüm MPI yürütmek için örnek bir Batch havuzundaki işlem düğümlerinde uygulama.
5. *İsteğe bağlı*: kullanım [Azure portal] [ portal] veya [BatchLabs] [ batch_labs] örnek havuz, iş ve görev incelemek için (" MultiInstanceSamplePool","MultiInstanceSampleJob","MultiInstanceSampleTask") kaynakları silmeden önce.

> [!TIP]
> İndirebilirsiniz [Visual Studio Community] [ visual_studio] ücretsiz Visual Studio yoksa.
>
>

Çıktı `MultiInstanceTasks.exe` aşağıdakine benzer:

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] to job [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks to complete...

---- Subtask information ----
subtask: 1
        exit code: 0
        node: tvm-1219235766_3-20161017t162002z
        stdout.txt:
        stderr.txt:
subtask: 2
        exit code: 0
        node: tvm-1219235766_2-20161017t162002z
        stdout.txt:
        stderr.txt:

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Sonraki adımlar
* Microsoft HPC & Azure Batch ekip blogu anlatılmaktadır [MPI desteklemek için Azure batch Linux][blog_mpi_linux]ve kullanma hakkında bilgi içerir [OpenFOAM] [ openfoam] toplu ile. Python kodu örnekleri bulabilirsiniz [OpenFOAM örneği github'daki][github_mpi].
* Bilgi nasıl [Linux işlem düğümleri havuzları oluşturma](batch-linux-nodes.md) Azure Batch MPI çözümlerinizi kullanmak için.

[helloworld_proj]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_labs]: https://azure.github.io/BatchLabs/
[blog_mpi_linux]: https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/
[cmd_start]: https://technet.microsoft.com/library/cc770297.aspx
[coord_cmd_example]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/mpi/data/linux/openfoam/coordination-cmd
[github_mpi]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[msdn_env_var]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[msmpi_msdn]: https://msdn.microsoft.com/library/bb524831.aspx
[msmpi_sdk]: http://go.microsoft.com/FWLink/p/?LinkID=389556
[msmpi_howto]: http://blogs.technet.com/b/windowshpc/archive/2015/02/02/how-to-compile-and-run-a-simple-ms-mpi-program.aspx
[openfoam]: http://www.openfoam.com/
[visual_studio]: https://www.visualstudio.com/vs/community/

[net_jobprep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_multiinstance_class]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_multiinstance_prop]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.multiinstancesettings.aspx
[net_multiinsance_commonresfiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.commonresourcefiles.aspx
[net_multiinstance_coordcmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.coordinationcommandline.aspx
[net_multiinstance_numinstances]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.numberofinstances.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_cmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.commandline.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_taskconstraints]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.aspx
[net_taskconstraint_maxretry]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxtaskretrycount.aspx
[net_taskconstraint_maxwallclock]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxwallclocktime.aspx
[net_taskconstraint_retention]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.retentiontime.aspx
[net_task_listsubtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listsubtasks.aspx
[net_task_listnodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[poolops_getnodefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getnodefile.aspx

[portal]: https://portal.azure.com
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx

[1]: ./media/batch-mpi/batch_mpi_01.png "Çok örnekli genel bakış"
