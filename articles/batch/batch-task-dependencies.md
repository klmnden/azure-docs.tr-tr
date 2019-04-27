---
title: Diğer görevler - Azure Batch öğesinin tamamlanmasına bağlı görevleri çalıştırmak için görev bağımlılıkları kullanın | Microsoft Docs
description: MapReduce stil ve benzer büyük veri işleme için diğer görevlerin tamamlanmasına bağımlı görevler oluşturma iş yüklerini Azure batch'te.
services: batch
documentationcenter: .net
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: b8d12db5-ca30-4c7d-993a-a05af9257210
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: lahugh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ca6918b809a9b4ede3fffb151c7fa5183ae03b47
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60550398"
---
# <a name="create-task-dependencies-to-run-tasks-that-depend-on-other-tasks"></a>Diğer görevlere bağlı görevlerin çalıştırılacak görev bağımlılıklarını oluşturma

Yalnızca bir üst görev tamamlandıktan sonra bir görevi veya görevler kümesini çalıştırmak için görev bağımlılıkları tanımlayabilirsiniz. Görev bağımlılıkları yararlı olduğu bazı senaryolar şunlardır:

* Buluttaki iş yüklerinizi MapReduce stili.
* İşleri, veri işleme görevlerini yönlendirilmiş Çevrimsiz graf (DAG) ifade edilebilir.
* Her görevin nerede sonraki görev başlamadan önce tamamlamanız gerekir ön işleme ve sonrası işleme işlemleri.
* Aşağı Akış görevlerinde çıktı Yukarı Akış görevlerin bağımlı olan diğer işler.

Toplu görev bağımlılıkları ile bir veya daha fazla üst görev tamamlandıktan sonra işlem düğümleri üzerinde yürütülmek zamanlanmış görevleri oluşturabilirsiniz. Örneğin, ayrı, paralel görevler içeren bir 3B film her karesi işleyen bir proje oluşturabilirsiniz. Son görev--"birleştirme görevi"--birleştirmeleri işlenmiş çerçeve tam film yalnızca tüm çerçeveleri başarıyla işlenip sonra.

Yalnızca üst görevin başarıyla tamamlandıktan sonra varsayılan olarak, bağımlı görevler yürütülmek üzere zamanlanır. Varsayılan davranışı geçersiz kılabilir ve üst görev başarısız olduğunda, görevleri çalıştırmak için bir bağımlılık eylemi belirtebilirsiniz. Bkz: [bağımlılık Eylemler](#dependency-actions) ayrıntıları bölümü.  

Diğer görevler birebir veya bire çok ilişkisi bağımlı görevler oluşturabilirsiniz. Burada bir görev tamamlandığında bir görevin kimlikleri belirtilen bir aralıktaki görev grubu, bağımlı bir aralık bağımlılık de oluşturabilirsiniz. Çoktan çoğa ilişki oluşturmak için bu üç temel senaryo birleştirebilirsiniz.

## <a name="task-dependencies-with-batch-net"></a>Görev bağımlılıkları ile Batch .NET
Bu makalede, görev bağımlılıklarını kullanarak yapılandırmak nasıl ele [Batch .NET] [ net_msdn] kitaplığı. İlk nasıl gösteriyoruz için [görev bağımlılığı etkinleştirme](#enable-task-dependencies) , işlerini ve ardından göstermek nasıl [görev bağımlılıklarıyla yapılandırmanın](#create-dependent-tasks). Biz de üst başarısız olursa, bağımlı görevleri çalıştırmak için bir bağımlılık eylem belirleme konusunda açıklanmaktadır. Son olarak, ele [bağımlılık senaryoları](#dependency-scenarios) , Batch destekler.

## <a name="enable-task-dependencies"></a>Görev bağımlılıklarını etkinleştirin
Görev bağımlılıkları Batch uygulamanızda kullanmak için proje, görev bağımlılıklarını kullanacak şekilde yapılandırmanız gerekir. Batch .NET üzerinde etkinleştirmek, [CloudJob] [ net_cloudjob] ayarlayarak onun [UsesTaskDependencies] [ net_usestaskdependencies] özelliğini `true`:

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

Yukarıdaki kod parçacığında, "batchClient" örneğidir [BatchClient] [ net_batchclient] sınıfı.

## <a name="create-dependent-tasks"></a>Bağımlı görevler oluşturun
Bir veya daha fazla üst görev öğesinin tamamlanmasına bağımlı olan bir görev oluşturmak için görev "diğer görevlere bağlı olduğunu" belirtebilirsiniz. Batch .NET içinde yapılandırma [CloudTask][net_cloudtask].[ DependsOn] [ net_dependson] örneği özelliğiyle [TaskDependencies] [ net_taskdependencies] sınıfı:

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

Bu kod parçacığı, görev kimliği ile "Satmanın" bağımlı bir görev oluşturur. "Satmanın" görev "Yağmur" ve "Paz ifadesiyle" görevlere bağlıdır. Görev "satmanın", "Yağmur" ve "Paz ifadesiyle" başarıyla tamamlandığında görevler sonra bir işlem düğümünde çalıştırılacak zamanlanacak.

> [!NOTE]
> Varsayılan olarak, içinde olduğunda başarıyla tamamlanacak bir görev olarak kabul edilir **tamamlandı** durumu ve kendi **çıkış kodu** olduğu `0`. Batch. NET'te, yani bir [CloudTask][net_cloudtask].[ Durum] [ net_taskstate] özelliği değerinin `Completed` ve CloudTask'ın [TaskExecutionInformation][net_taskexecutioninformation].[ ExitCode] [ net_exitcode] özellik değeri `0`. Bunu değiştirmek için bkz. nasıl [bağımlılık Eylemler](#dependency-actions) bölümü.
> 
> 

## <a name="dependency-scenarios"></a>Bağımlılık senaryoları
Azure Batch hizmetinde kullanabileceğiniz üç basit görev bağımlılık senaryo vardır: bire bir, bire çok ve görev kimliği aralığı bağımlılık. Bu dördüncü bir senaryo, çoktan çoğa sağlamak için birleştirilebilir.

| Senaryo&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Örnek |  |
|:---:| --- | --- |
|  [Bire bir](#one-to-one) |*Görevb* bağlıdır *Göreva* <p/> *Görevb* kadar yürütme için zamanlanır değil *Göreva* başarıyla tamamlandı |![Diyagram: bire bir görev bağımlılığı][1] |
|  [-Çok](#one-to-many) |*görevC* hem *görevA* hem de *görevB*’ye bağlıdır <p/> *Görevc* hem kadar yürütme için zamanlanır değil *Göreva* ve *Görevb* başarıyla tamamladınız |![Diyagram: bir-çok görev bağımlılığı][2] |
|  [Görev Kimliği aralığı](#task-id-range) |*Görevd* bir dizi üzerinde bağlıdır <p/> *Görevd* kimlikleri görevlerle kadar yürütme için zamanlanır değil *1* aracılığıyla *10* başarıyla tamamladınız |![Diyagram: Görev Kimliği aralığı bağımlılığı][3] |

> [!TIP]
> Oluşturabileceğiniz **çoktan çoğa** burada görevlere A ve b C, D, E ve F her görevleri bağlı gibi ilişkileri Bu, burada birden çok Yukarı Akış görevi çıkışı üzerinde görevleri bağımlı gibi paralel ön işleme senaryolarda kullanışlıdır.
> 
> Yalnızca üst görevleri başarıyla tamamlandıktan sonra bu bölümdeki örneklerde, bağımlı bir görev çalıştırır. Bu davranış, bağımlı bir görev için varsayılan davranıştır. Bağımlı bir görev, üst görev varsayılan davranışın üzerine bir bağımlılık eylemi belirterek başarısız olduktan sonra çalıştırabilirsiniz. Bkz: [bağımlılık Eylemler](#dependency-actions) ayrıntıları bölümü.

### <a name="one-to-one"></a>Bire bir
Bire bir ilişki bir görev başarıyla tamamlandığında, bir üst göreve bağlıdır. Bağımlılık oluşturmak için bir tek bir görev kimliği sağlayın. [TaskDependencies][net_taskdependencies].[ OnId] [ net_onid] , doldurmanız statik yöntemi [DependsOn] [ net_dependson] özelliği [CloudTask] [ net_cloudtask].

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a>-Çok
Bir-çok ilişkisi içinde bir görev birden çok üst görev olarak tamamlanmasına bağlıdır. Bağımlılık oluşturmak için bir koleksiyon için görev kimliklerinin sağlamak [TaskDependencies][net_taskdependencies].[ OnIds] [ net_onids] , doldurmanız statik yöntemi [DependsOn] [ net_dependson] özelliği [CloudTask] [ net_cloudtask].

```csharp
// 'Rain' and 'Sun' don't depend on any other tasks
new CloudTask("Rain", "cmd.exe /c echo Rain"),
new CloudTask("Sun", "cmd.exe /c echo Sun"),

// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
``` 

### <a name="task-id-range"></a>Görev Kimliği aralığı
Bir dizi üst bağımlılık olarak, bir görev kimlikleri bir aralıkta kalan görevlerin tamamlanmasına bağlıdır.
Bağımlılık oluşturmak için ilk sağlayın ve son aralığı içinde kimlikleri görev [TaskDependencies][net_taskdependencies].[ OnIdRange] [ net_onidrange] , doldurmanız statik yöntemi [DependsOn] [ net_dependson] özelliği [CloudTask] [ net_cloudtask].

> [!IMPORTANT]
> Bağımlılıklarınızı için görev kimliği aralığı kullandığınızda, tamsayı değerlerini temsil eden kimlikleri yalnızca görevlerle aralığına göre seçilir. Bu nedenle aralık `1..10` görevleri seçer `3` ve `7`, ama `5flamingoes`. 
> 
> Başında sıfır olmayan önemli görevleri, tanımlayıcıları dize için aralığı bağımlılıkları değerlendirirken `4`, `04` ve `004` olacak *içinde* aralığı ve tüm görev kabuledilir`4`, tamamlanması birinci bağımlılığını yerine getirecek.
> 
> Aralıktaki her görev, bağımlılık başarıyla tamamlanmasını veya ayarlamak bir bağımlılık eylemi eşlenmiş bir hata ile Tamamlanıyor karşılamalıdır **Satisfy**. Bkz: [bağımlılık Eylemler](#dependency-actions) ayrıntıları bölümü.
>
>

```csharp
// Tasks 1, 2, and 3 don't depend on any other tasks. Because
// we will be using them for a task range dependency, we must
// specify string representations of integers as their ids.
new CloudTask("1", "cmd.exe /c echo 1"),
new CloudTask("2", "cmd.exe /c echo 2"),
new CloudTask("3", "cmd.exe /c echo 3"),

// Task 4 depends on a range of tasks, 1 through 3
new CloudTask("4", "cmd.exe /c echo 4")
{
    // To use a range of tasks, their ids must be integer values.
    // Note that we pass integers as parameters to TaskIdRange,
    // but their ids (above) are string representations of the ids.
    DependsOn = TaskDependencies.OnIdRange(1, 3)
},
```

## <a name="dependency-actions"></a>Bağımlılık eylemleri

Yalnızca bir üst görevi başarıyla tamamlandıktan sonra varsayılan olarak, bağımlı bir görevi veya görevler kümesini çalıştırır. Bazı senaryolarda, üst görev başarısız olsa bile bağımlı görevleri çalıştırmak isteyebilirsiniz. Bir bağımlılık eylemi belirterek varsayılan davranışı geçersiz kılabilirsiniz. Bir bağımlılık eylemi, başarı veya başarısızlık üst görevin göre bağımlı görevi çalıştırmak uygun olup olmadığını belirtir. 

Örneğin, bağımlı bir görev tamamlandığında Yukarı Akış görevi, verilerden bekliyor varsayalım. Yukarı Akış görevi başarısız olursa, bağımlı görev eski verileri kullanarak çalıştırmak mümkün olabilir. Bu durumda, bir bağımlılık eylemi bağımlı görev üst görevin başarısız olmasına rağmen çalıştırmak uygun olduğunu belirtebilirsiniz.

Bir bağımlılık eylemi üst görev için bir çıkış koşulunu temel alır. Çıkış aşağıdaki koşullardan herhangi biri için bir bağımlılık eylemi belirtebilirsiniz; .NET için bkz: [ExitConditions] [ net_exitconditions] ayrıntıları sınıfı:

- Ön işleme hatası olduğunda gerçekleşir.
- Bir dosya karşıya yükleme hatası oluşur. Görev aracılığıyla belirtilen çıkış kodu ile çıkar, **exitCodes** veya **exitCodeRanges**, ve ardından bir dosya karşıya yükleme hatası, çıkış kodu tarafından belirtilen eylemi karşılaştığı önceliklidir.
- Görev tarafından tanımlanan bir çıkış kodu ile çıktığında **ExitCodes** özelliği.
- Görev tarafından belirtilen bir aralıkta kalan bir çıkış kodu ile çıktığında **ExitCodeRanges** özelliği.
- Varsayılan durumda, görev tarafından tanımlanmayan bir çıkış kodu ile çıkılıyorsa **ExitCodes** veya **ExitCodeRanges**, ya da görev ön işleme hatası ile bulunup bulunmadığını ve **PreProcessingError** özelliği ayarlı değil, ya da bir dosyayla görev başarısız olursa karşıya yükleme hatası ve **FileUploadError** özelliği ayarlı değil. 

. NET'te bir bağımlılık eylemi belirtmek için ayarlayın [ExitOptions][net_exitoptions].[ DependencyAction] [ net_dependencyaction] çıkış koşulu özelliği. **DependencyAction** özelliği iki değerden birini alır:

- Ayarı **DependencyAction** özelliğini **Satisfy** bağımlı görev üst görevin ile belirtilen bir hata varsa çalıştırmak uygun olduğunu gösterir.
- Ayarı **DependencyAction** özelliğini **blok** bağımlı görevleri çalıştırmak uygun olmadığını gösterir.

Varsayılan ayarı **DependencyAction** özelliği **Satisfy** çıkış kodu 0, için ve **blok** diğer tüm çıkış koşulları için.

Aşağıdaki kod parçacığı kümeleri **DependencyAction** özelliği için bir üst görev. Bağımlı görev üst görevin ön işleme hatası veya belirtilen hata kodlarını ile çıkılıyorsa engellenir. Üst görev ile başka bir sıfır olmayan hata varsa, bağımlı görevi çalıştırmak uygundur.

```csharp
// Task A is the parent task.
new CloudTask("A", "cmd.exe /c echo A")
{
    // Specify exit conditions for task A and their dependency actions.
    ExitConditions = new ExitConditions
    {
        // If task A exits with a pre-processing error, block any downstream tasks (in this example, task B).
        PreProcessingError = new ExitOptions
        {
            DependencyAction = DependencyAction.Block
        },
        // If task A exits with the specified error codes, block any downstream tasks (in this example, task B).
        ExitCodes = new List<ExitCodeMapping>
        {
            new ExitCodeMapping(10, new ExitOptions() { DependencyAction = DependencyAction.Block }),
            new ExitCodeMapping(20, new ExitOptions() { DependencyAction = DependencyAction.Block })
        },
        // If task A succeeds or fails with any other error, any downstream tasks become eligible to run 
        // (in this example, task B).
        Default = new ExitOptions
        {
            DependencyAction = DependencyAction.Satisfy
        }
    }
},
// Task B depends on task A. Whether it becomes eligible to run depends on how task A exits.
new CloudTask("B", "cmd.exe /c echo B")
{
    DependsOn = TaskDependencies.OnId("A")
},
```

## <a name="code-sample"></a>Kod örneği
[TaskDependencies] [ github_taskdependencies] örnek proje, biridir [Azure Batch kod örnekleri] [ github_samples] GitHub üzerinde. Bu Visual Studio çözüm gösterir:

- Bir proje görev bağımlılığı etkinleştirme
- Diğer görevlere bağlı görevlerin oluşturma
- Nasıl bir işlem düğümleri havuzu üzerinde görevlere yürütün.

## <a name="next-steps"></a>Sonraki adımlar
### <a name="application-deployment"></a>Uygulama dağıtımı
[Uygulama paketleri](batch-application-packages.md) özellik toplu işlemi kolay bir yol sağlar hem de dağıtma ve sürüm görevleriniz üzerinde yürütülen uygulamalarda işlem düğümleri.

### <a name="installing-applications-and-staging-data"></a>Uygulamaları yükleme ve verileri hazırlama
Bkz: [uygulamaları yükleme ve batch veri hazırlama işlem düğümleri] [ forum_post] düğümlerinizi, görevleri çalıştırmak için hazırlama yöntemlerine genel bir bakış için Azure Batch forumunu içinde. Bir Azure Batch ekip üyeleri tarafından yazılan bu uygulamalar, görev girdi verilerini ve diğer dosyaları, işlem düğümlerinizi kopyalamak için çeşitli yollar hakkında iyi öncü postadır.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_taskdependencies]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_dependson]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.dependson.aspx
[net_exitcode]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.exitcode.aspx
[net_exitconditions]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.exitconditions
[net_exitoptions]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.exitoptions
[net_dependencyaction]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.exitoptions
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_onid]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onid.aspx
[net_onids]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onids.aspx
[net_onidrange]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onidrange.aspx
[net_taskexecutioninformation]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_usestaskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.usestaskdependencies.aspx
[net_taskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskdependencies.aspx

[1]: ./media/batch-task-dependency/01_one_to_one.png "Diyagram: bire bir bağımlılık"
[2]: ./media/batch-task-dependency/02_one_to_many.png "Diyagram: bir-çok bağımlılık"
[3]: ./media/batch-task-dependency/03_task_id_range.png "Diyagram: görev kimliği aralığı bağımlılığı"
