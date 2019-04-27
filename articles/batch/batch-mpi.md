---
title: MPI uygulamaları - Azure Batch çalıştırmak için çok örnekli görevleri kullanma | Microsoft Docs
description: Azure Batch hizmetinde çok örnekli görev türü kullanarak ileti geçirme arabirimi (MPI) uygulamaları çalıştırma hakkında bilgi edinin.
services: batch
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: 83e34bd7-a027-4b1b-8314-759384719327
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.date: 03/13/2019
ms.author: lahugh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7fe75dabe098cf98f0c3c04d592a32d6a44cebf8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60775323"
---
# <a name="use-multi-instance-tasks-to-run-message-passing-interface-mpi-applications-in-batch"></a>Batch'de ileti geçirme arabirimi (MPI) uygulamalarını çalıştırmak için çok örnekli görevleri kullanma

Çok örnekli görevler, bir Azure Batch görevi aynı anda birden çok işlem düğümleri üzerinde çalışmak olanak tanır. Bu görevler, yüksek performanslı bilgi işlem senaryoları gibi Batch ileti geçirme arabirimi (MPI) uygulamalarında etkinleştirin. Bu makalede, çok örnekli görevleri kullanma yürütülecek öğrenin [Batch .NET] [ api_net] kitaplığı.

> [!NOTE]
> Bu makaledeki örnekler, Batch .NET, MS-MPI odaklanın ve Windows işlem düğümleri, ancak burada tartışılan çok örnekli görev kavramları, diğer platformlar ve teknolojiler (Python ve örnek için Linux düğümleri üzerinde Intel MPI) için geçerlidir.
>
>

## <a name="multi-instance-task-overview"></a>Çok örnekli görev genel bakış
Batch içinde normalde her görev, bir tek işlem düğümde--birden çok görev bir iş göndermek ve Batch hizmeti her görevi bir düğümde yürütülmek zamanlar. Bir görevin yapılandırarak ancak **çok örnekli ayarlar**, bunun yerine bir birincil görev ve ardından birden çok düğümde yürütülen birkaç alt görevler oluşturmak için Batch söyleyin.

![Çok örnekli görev genel bakış][1]

Bir görev çok örnekli ayarlar bir iş gönderdiğinizde, Batch çok örnekli görevler benzersiz birkaç adım gerçekleştirir:

1. Batch hizmeti bir oluşturur **birincil** ve birkaç **görevleri** çok örnekli ayarlara göre. Görevleri (tüm alt birincil) toplam sayısı sayısı ile eşleşen **örnekleri** (işlem düğümleri) çok örnekli ayarlar belirtin.
2. Batch işlem düğümleri birini atar **ana**ve ana yürütmek için birincil görevi zamanlar. Bu, çok örnekli görev, bir alt düğüm başına ayrılmış işlem düğümleri kalanını yürütmek için görevleri zamanlar.
3. Birincil ve tüm alt görevlerin indirmek **ortak kaynak dosyaları** çok örnekli ayarlar belirtin.
4. Sonra ortak kaynak dosyaları yüklenmiş, birincil ve alt görevlere yürütün **koordinasyon komut** çok örnekli ayarlar belirtin. Koordinasyon komutu, genellikle görevin yürütmek için ilgili düğümleri hazırlamak için kullanılır. Bu arka plan hizmetleri başlatılıyor içerebilir (gibi [Microsoft MPI][msmpi_msdn]'s `smpd.exe`) ve düğümler arası iletileri işlemeye hazır olduğu doğrulanıyor.
5. Birincil görevi yürüten **uygulama komutu** ana düğüm üzerinde *sonra* koordinasyon komutu başarıyla birincil ve tüm alt görevler tarafından tamamlandı. Uygulama komutu, çok örnekli görev komut satırı ve yalnızca birincil görev tarafından yürütülür. İçinde bir [MS MPI][msmpi_msdn]-çözüm, temel burada kullanarak MPI etkinleştirilmiş uygulamanın yürütme budur `mpiexec.exe`.

> [!NOTE]
> İşlevsel olarak farklı olsa da, "çok örnekli görev" gibi bir benzersiz görev türü değil [StartTask] [ net_starttask] veya [JobPreparationTask] [ net_jobprep]. Çok örnekli görev yalnızca standart bir Batch görevinde olduğu ([CloudTask] [ net_task] Batch. NET'te), çok örnekli ayarlar yapılandırıldı. Bu makalede, bu diyoruz **çok örnekli görev**.
>
>

## <a name="requirements-for-multi-instance-tasks"></a>Çok örnekli görevler için gereksinimleri
Çok örnekli görevler gerektiren bir havuzla **etkin düğümler arası iletişimin**ile **eşzamanlı görev yürütme devre dışı**. Eşzamanlı görev yürütme devre dışı bırakmak için ayarlanmış [CloudPool.MaxTasksPerComputeNode](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool) özelliği 1.

> [!NOTE]
> Batch [sınırları](batch-quota-limit.md#pool-size-limits) düğümler arası iletişimin etkin olan bir havuz boyutu.


Bu kod parçacığı, Batch .NET kitaplığını kullanarak çok örnekli görevleri için bir havuz oluşturma işlemi gösterilmektedir.

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicatedComputeNodes: 3
        virtualMachineSize: "standard_d1_v2",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.MaxTasksPerComputeNode = 1;
```

> [!NOTE]
> Düğümler arası iletişimin devre dışı veya ile bir havuzda çok örnekli görev çalıştırmayı denerseniz bir *maxTasksPerNode* 1'den büyük değer, görev hiçbir zaman zamanlanmış--süresiz olarak "etkin" durumda kalır. 


### <a name="use-a-starttask-to-install-mpi"></a>MPI yüklemek için StartTask kullanın
Bir çok örnekli görev MPI uygulamalarını çalıştırmak için ilk (MS-MPI veya örneğin Intel MPI) MPI uygulama havuzundaki işlem düğümlerinde yüklemeniz gerekir. Bunu kullanmak için iyi bir zamandır bir [StartTask][net_starttask], her bir düğüm bir havuza katıldığında veya yeniden yürütür. Bu kod parçacığı MS MPI kurulum paketi olarak belirten bir StartTask oluşturur bir [kaynak dosyası][net_resourcefile]. Başlangıç görevinin komut satırı kaynak dosyasındaki düğüme İndirildikten sonra yürütülür. Bu durumda, komut satırında, MS-MPI'in bir katılımsız yüklemesi gerçekleştirir.

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
Seçeneğini belirlediğinizde bir [RDMA özellikli boyutu](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Batch havuzunuzdaki işlem düğümlerinin A9 gibi MPI uygulamanızı Azure'nın yüksek performanslı, düşük gecikme süreli doğrudan uzak bellek erişimi (RDMA) ağ yararlanabilirsiniz.

Aşağıdaki makalelerde "RDMA özellikli" belirtilen boyutları arayın:

* **CloudServiceConfiguration** havuzları

  * [Cloud Services boyutları](../cloud-services/cloud-services-sizes-specs.md) (yalnızca Windows)
* **VirtualMachineConfiguration** havuzları

  * [Azure'da sanal makine boyutları](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux)
  * [Azure'da sanal makine boyutları](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows)

> [!NOTE]
> RDMA yararlanmak [Linux işlem düğümlerini](batch-linux-nodes.md), kullanmalısınız **Intel MPI** düğümlerinde. 
>

## <a name="create-a-multi-instance-task-with-batch-net"></a>Batch .NET ile çok örnekli görev oluşturma
MPI paket yükleme ve havuzu gereksinimleri ele aldığımız, çok örnekli görev oluşturalım. Bu kod parçacığında, bir standart oluştururuz [CloudTask][net_task], yapılandırın, sonra kendi [MultiInstanceSettings] [ net_multiinstance_prop] özelliği. Daha önce bahsedildiği gibi çok örnekli görev farklı görev türü, ancak çok örnekli ayarlar ile yapılandırılmış bir standart toplu görev değil.

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
Çok örnekli ayarlar için bir görev oluşturduğunuzda, görev yürütülecek olan işlem düğümü sayısını belirtin. Görev bir iş gönderdiğinizde, Batch hizmeti bir oluşturur **birincil** görev ve yeterli **görevleri** belirttiğiniz düğüm sayısını birlikte eşleşen.

Bu görevler için 0 aralığında bir tamsayı kimliği atanır *numberOfInstances* - 1. Görev Kimliği 0 ile birincil bir görevdir ve diğer tüm kimlikleri görevleridir. Örneğin, bir görev için aşağıdaki çok örnekli ayarlar oluşturursanız, birincil görevin kimliği 0 olması ve alt görevlerin kimlikleri 1-9 gerekir.

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a>Ana düğüm
Çok örnekli görev gönderdiğinizde, Batch hizmeti işlem düğümlerinden biri olarak "Yönetici" düğümü belirler ve ana düğüm üzerinde yürütmek için birincil görevi zamanlar. Alt görevler için çok örnekli görev ayrılan düğümler kalanını yürütmek için zamanlanır.

## <a name="coordination-command"></a>Koordinasyon komutu
**Koordinasyon komut** alt görevlerin ve birincil yürütülür.

Koordinasyon komut çağırmayı engelliyor--toplu düzenleme komut için tüm görevleri başarıyla verdi kadar uygulama komutu yürütülmez. Koordinasyon komutu bu nedenle tüm gerekli arka plan hizmetleri başlatın, kullanıma hazır olduğundan emin olun ve sonra çıkın. Örneğin, bu düzenleme komut 7 SMPD hizmet düğümde başlatılır. MS-MPI sürümünü kullanan bir çözüm için kapanır:

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

Kullanımına dikkat edin `start` bu koordinasyon komutu. Bu gereklidir çünkü `smpd.exe` uygulama hemen sonra yürütmeye döndürmez. Kullanmadan [Başlat] [ cmd_start] komutu, bu düzenleme komut değil döndürür ve bu nedenle çalışmasını uygulama komut engellenebilir.

## <a name="application-command"></a>Uygulama komutu
Koordinasyon komutu yürütmeden birincil görev ve tüm görevleri tamamladıktan sonra çok örnekli görev komut satırı birincil görev tarafından yürütülen *yalnızca*. Bu diyoruz **uygulama komutu** koordinasyon komuttan ayırmak için.

MS-MPI uygulamaları için uygulama komutu MPI özellikli uygulamanızla yürütmek için kullanın. `mpiexec.exe`. Örneğin, MS-MPI sürüm 7 kullanarak bir çözüm için bir uygulama komutu şu şekildedir:

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

> [!NOTE]
> Çünkü MS-MPI's `mpiexec.exe` kullanan `CCP_NODES` değişken varsayılan olarak (bkz [ortam değişkenlerini](#environment-variables)) Yukarıdaki örnek uygulama komut satırı dahil değildir.
>
>

## <a name="environment-variables"></a>Ortam değişkenleri
Batch oluşturur birkaç [ortam değişkenlerini] [ msdn_env_var] belirli bir çok örnekli görev için ayrılmış işlem düğümleri üzerinde çok örnekli görevler için. Bunları programları ve betikleri gibi işbirliği ve uygulama komut satırları bu ortam değişkenleri başvurabilirsiniz.

Aşağıdaki ortam değişkenlerini, çok örnekli görevler tarafından kullanılmak üzere Batch hizmeti tarafından oluşturulur:

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

Bunlar tam ayrıntıları ve diğer toplu işlem düğümü ortam değişkenleri, bunların içeriğini ve görünürlük de dahil olmak üzere işlem bkz [işlem düğümü ortam değişkenleri][msdn_env_var].

> [!TIP]
> Batch Linux MPI kod örneği birkaç bu ortam değişkenlerinin nasıl kullanılabileceğini örneği içerir. [Koordinasyon cmd] [ coord_cmd_example] ortak uygulama Bash betiğini indirir ve giriş dosyaları Azure Storage'dan, ana düğüm ağ dosya sistemi (NFS) paylaşımında sağlar ve diğer düğümleri yapılandırır çok örnekli görev için NFS istemcisi olarak ayrılmış.
>
>

## <a name="resource-files"></a>Kaynak dosyaları
Kaynak dosyaları için çok örnekli görevler dikkate alınması gereken iki kümesi vardır: **ortak kaynak dosyaları** , *tüm* görevler indirir (hem birincil hem de ve alt görevler) ve **kaynakdosyaları** çok örnekli görev için kendisini, belirtilen *yalnızca birincil* görev indirmeler.

Bir veya daha fazla belirtebilirsiniz **ortak kaynak dosyaları** bir görev için çok örnekli ayarlar. Bu ortak kaynak dosyaları karşıdan yüklenir [Azure depolama](../storage/common/storage-introduction.md) her düğümün içine **görev paylaşılan dizine** birincil ve tüm alt görevler tarafından. Kullanarak, uygulama ve işbirliği komut satırlarından görev paylaşılan dizine erişebilir `AZ_BATCH_TASK_SHARED_DIR` ortam değişkeni. `AZ_BATCH_TASK_SHARED_DIR` Yolu çok örnekli görev için ayrılan tüm düğümlerde özdeş ise, bu nedenle, birincil ve tüm alt görevler arasında koordinasyon tek komut paylaşabilirsiniz. Batch "Uzaktan erişim anlamda dizini paylaşmaz", ancak bağlama kullanın veya ortam değişkenlerini ipucu daha önce belirtildiği gibi noktası paylaşın.

Çok örnekli görev için belirttiğiniz kaynak dosyaları, görevin çalışma dizinine yüklenir `AZ_BATCH_TASK_WORKING_DIR`, varsayılan olarak. , Ortak kaynak dosyaları aksine belirtildiği gibi yalnızca birincil görev çok örnekli görev kendisi için belirtilen kaynak dosyalarını indirir.

> [!IMPORTANT]
> Her zaman ortam değişkenlerini kullanma `AZ_BATCH_TASK_SHARED_DIR` ve `AZ_BATCH_TASK_WORKING_DIR` , komut satırlarında bu dizinler başvurmak için. Yollarını el ile oluşturmak çalışmayın.
>
>

## <a name="task-lifetime"></a>Görev ömrü
Birincil görev ömrünü, tüm çok örnekli görev ömrünü denetler. Birincil çıktığında tüm görevleri sonlandırılır. Birincil çıkış kodunu, görevin çıkış kodu ve bu nedenle başarısı veya başarısızlığı yeniden deneme amaçlı görev belirlemek için kullanılır.

Herhangi bir alt görevlerin başarısız olursa sıfır olmayan dönüş kodu ile çıkmak gibi tüm çok örnekli görev başarısız olur. Çok örnekli görev sonra sonlandırıldı ve, yeniden deneme sınırına kadar yeniden denenir.

Çok örnekli Görev sildiğinizde, birincil ve tüm alt görevler de Batch hizmeti tarafından silinir. Tüm dizinler eklemeli ve işlem düğümlerinden, standart bir görev olduğu gibi dosyalar silinir.

[TaskConstraints] [ net_taskconstraints] bir çok örnekli görev için gibi [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime] [ net_taskconstraint_maxwallclock], ve [RetentionTime] [ net_taskconstraint_retention] bunlar standart bir görev için birincil ve tüm alt görevleri uygulamak gibi özellikleri dikkate alınır. Ancak, değiştirirseniz [RetentionTime] [ net_taskconstraint_retention] özelliği çok örnekli görev bu değişikliği projeye ekledikten sonra yalnızca birincil göreve uygulanır. Tüm alt görevlerin özgün kullanmaya devam [RetentionTime][net_taskconstraint_retention].

Yeni görev bir çok örnekli görev bir parçası ise bir işlem düğümünün son kullanılan görevler listesi bir alt görevin kimliği yansıtır.

## <a name="obtain-information-about-subtasks"></a>Alt görevler hakkında bilgi edinin
Batch .NET kitaplığını kullanarak görevleri hakkında bilgi edinmek için çağrı [CloudTask.ListSubtasks] [ net_task_listsubtasks] yöntemi. Bu yöntem tüm görevleri hakkında bilgi ve görevler yürütülen işlem düğüm hakkında bilgi döndürür. Bu bilgileri, her bir alt ait kök dizini, Havuz kimliği, geçerli durumunda, çıkış kodu ve diğer belirleyebilirsiniz. Bu bilgi ile birlikte kullanabileceğiniz [PoolOperations.GetNodeFile] [ poolops_getnodefile] alt görevin'ın dosyalarını almak için yöntemi. Bu yöntem birincil görevin (kimliği 0) bilgi döndürmediğine dikkat edin.

> [!NOTE]
> Aksi belirtilmediği sürece, Batch .NET yöntemleri çalışan birden çok örnek üzerinde [CloudTask] [ net_task] kendisini uygulamak *yalnızca* birincil görev. Örneğin, çağırdığınızda [CloudTask.ListNodeFiles] [ net_task_listnodefiles] bir çok örnekli görev yönteminde, yalnızca birincil görevin dosyalarının döndürülür.
>
>

Aşağıdaki kod parçacığı, istek üzerinde yürütülen düğümlerden dosya içerikleri yanı sıra alt bilgi edinmek gösterilmektedir.

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
[MultiInstanceTasks] [ github_mpi] github'daki kod örneği, bir çok örnekli görev çalıştırmak amacıyla kullanmak üzere nasıl gösterir bir [MS MPI] [ msmpi_msdn] uygulaması Batch işlem düğümleri. Bağlantısındaki [hazırlık](#preparation) ve [yürütme](#execution) örneği çalıştırmak için.

### <a name="preparation"></a>Hazırlık
1. İlk iki adımını izleyin [nasıl derlenip çalıştırılacağı basit bir MS-MPI program][msmpi_howto]. Bu, aşağıdaki adımı önkoşullarını karşılar.
2. Derleme bir *yayın* sürümünü [MPIHelloWorld] [ helloworld_proj] örnek MPI programı. Çok örnekli görev tarafından işlem düğümlerinde çalıştırılacak programı budur.
3. İçeren bir ZIP dosyası oluşturma `MPIHelloWorld.exe` (hangi derlediğiniz 2. adım) ve `MSMpiSetup.exe` (indirmiş 1. adım). Sonraki adımda bir uygulama paketi olarak bu ZIP dosyasını karşıya yükleyelim.
4. Kullanım [Azure portalında] [ portal] toplu oluşturmak için [uygulama](batch-application-packages.md) "MPIHelloWorld" olarak adlandırılan ve sürümü olarak "1.0" önceki adımda oluşturulan zip dosyası belirtin Uygulama paketi. Bkz: [karşıya yüklemek ve uygulamaları yönetmek](batch-application-packages.md#upload-and-manage-applications) daha fazla bilgi için.

> [!TIP]
> Derleme bir *yayın* sürümünü `MPIHelloWorld.exe` herhangi bir ek bağımlılıklar eklemek zorunda kalmazsınız (örneğin, `msvcp140d.dll` veya `vcruntime140d.dll`) uygulama paketinizdeki.
>
>

### <a name="execution"></a>Yürütme
1. İndirme [azure-batch-samples] [ github_samples_zip] github'dan.
2. MultiInstanceTasks açın **çözüm** Visual Studio 2017'de. `MultiInstanceTasks.sln` Çözüm dosyası bulunur:

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`
3. Batch ve Storage hesabı kimlik bilgilerinizi girin `AccountSettings.settings` içinde **Microsoft.Azure.Batch.Samples.Common** proje.
4. **Derleme ve çalıştırma** MultiInstanceTasks çözüm MPI yürütmek için örnek bir Batch havuzundaki işlem düğümlerinde uygulama.
5. *İsteğe bağlı*: Kullanım [Azure portalında] [ portal] veya [Batch Gezgini] [ batch_labs] örnek havuzu, iş ve görev incelemek için ("MultiInstanceSamplePool"," MultiInstanceSampleJob","MultiInstanceSampleTask") önce kaynakları silin.

> [!TIP]
> İndirebileceğiniz [Visual Studio Community] [ visual_studio] ücretsiz Visual Studio yoksa.
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
* Microsoft HPC ve Azure Batch ekibi blog anlatılmaktadır [MPI desteklemek için Linux Azure Batch][blog_mpi_linux]ve kullanma hakkında bilgiler yer almaktadır [OpenFOAM] [ openfoam] Batch ile. Python kod örnekleri bulabilirsiniz [OpenFOAM örneği github'daki][github_mpi].
* Bilgi nasıl [Linux işlem düğümü havuzlarını oluşturma](batch-linux-nodes.md) Azure Batch MPI çözümlerinizi kullanmak için.

[helloworld_proj]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_labs]: https://azure.github.io/BatchExplorer/
[blog_mpi_linux]: https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/
[cmd_start]: https://technet.microsoft.com/library/cc770297.aspx
[coord_cmd_example]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/mpi/data/linux/openfoam/coordination-cmd
[github_mpi]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[msdn_env_var]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[msmpi_msdn]: https://msdn.microsoft.com/library/bb524831.aspx
[msmpi_sdk]: https://go.microsoft.com/FWLink/p/?LinkID=389556
[msmpi_howto]: https://blogs.technet.com/b/windowshpc/archive/2015/02/02/how-to-compile-and-run-a-simple-ms-mpi-program.aspx
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
