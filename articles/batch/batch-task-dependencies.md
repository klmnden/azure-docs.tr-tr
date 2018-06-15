---
title: Diğer görevler - Azure Batch tamamlanma dayalı görevleri çalıştırmak için görev bağımlılıkları kullanın | Microsoft Docs
description: MapReduce stili ve benzer büyük veri işleme için diğer görevlerin bağımlı görevler oluşturma Azure Batch iş yükleri.
services: batch
documentationcenter: .net
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: b8d12db5-ca30-4c7d-993a-a05af9257210
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ba85e075c39251b0b3d7c4b8bc3f8d53a1afadf7
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
ms.locfileid: "30316826"
---
# <a name="create-task-dependencies-to-run-tasks-that-depend-on-other-tasks"></a>Diğer görevlere bağlı görevleri çalıştırmak için görev bağımlılıkları oluşturma

Yalnızca bir üst görev tamamlandıktan sonra bir görevi veya görev kümesini çalıştırmak için görev bağımlılıkları tanımlayabilirsiniz. Görev bağımlılıkları yararlı olduğu bazı senaryolar şunlardır:

* MapReduce stili iş yüklerini bulutta.
* İşleri, veri işleme görevlerini yönlendirilmiş Çevrimsiz grafik (DAG) ifade edilebilir.
* Sonraki görev başlamadan önce her görevin nerede tamamlamalısınız ön işleme ve sonrası işleme işlemleri.
* Aşağı Akış görevleri Yukarı Akış görevleri Çıkışta bağımlı diğer işler.

Toplu görev bağımlılıkları ile bir veya daha fazla üst görevleri tamamlandıktan sonra işlem düğümlerinde yürütülmesi için zamanlanmış görevler oluşturabilirsiniz. Örneğin, ayrı, paralel görevler içeren bir 3B film her karesini işleyen bir iş oluşturabilirsiniz. En son görev--"Birleştirme görev"--birleştirmeler işlenmiş çerçeve tam film yalnızca tüm çerçeveler başarıyla işlenip işlenmediğini sonra.

Yalnızca üst görevi başarıyla tamamlandıktan sonra varsayılan olarak, bağımlı görevler çalıştırılmak üzere zamanlandı. Varsayılan davranışı geçersiz kılabilir ve üst görevi başarısız olduğunda, görevleri çalıştırmak için bir bağımlılık eylemi belirtebilirsiniz. Bkz: [bağımlılık Eylemler](#dependency-actions) ayrıntıları bölümü.  

Diğer görevleri birebir veya bire çok ilişkide bağımlı görevler oluşturabilirsiniz. Burada bir görev görev kimlikleri belirtilen bir aralıkta görevlerin bir grubun bağlı bir aralık bağımlılık de oluşturabilirsiniz. Çok-çok ilişkileri oluşturmak için bu üç temel senaryodan birleştirebilirsiniz.

## <a name="task-dependencies-with-batch-net"></a>Görev bağımlılıkları Batch .NET ile
Bu makalede, görev bağımlılıkları kullanarak yapılandırmak nasıl aşağıdakiler ele [Batch .NET] [ net_msdn] kitaplığı. İlk nasıl gösteriyoruz için [görev bağımlılığı etkinleştirmek](#enable-task-dependencies) , işlerini ve ardından göstermek nasıl [görev bağımlılıkları ile yapılandırma](#create-dependent-tasks). Biz de üst başarısız olursa bağımlı görevleri çalıştırmak için bir bağımlılık eylem belirtmek açıklar. Son olarak, aşağıdakiler ele [bağımlılık senaryolarına](#dependency-scenarios) toplu destekleyen.

## <a name="enable-task-dependencies"></a>Görev bağımlılıkları etkinleştir
Görev bağımlılıkları Batch uygulamanızda kullanmak için ilk görev bağımlılıkları kullanmak için iş yapılandırmanız gerekir. Batch .NET içinde etkinleştirmeden, [CloudJob] [ net_cloudjob] ayarlayarak kendi [UsesTaskDependencies] [ net_usestaskdependencies] özelliğine `true`:

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

Yukarıdaki kod parçacığında, "batchClient" örneğidir [BatchClient] [ net_batchclient] sınıfı.

## <a name="create-dependent-tasks"></a>Bağımlı görevler oluşturma
Bir veya daha fazla üst görevleri tamamlama sonrasında bağlıdır bir görev oluşturmak için görev "diğer görevlere bağlı olduğunu" belirtebilirsiniz. Batch .NET içinde yapılandırma [CloudTask][net_cloudtask].[ DependsOn] [ net_dependson] örneği özelliğiyle [Github_samples] [ net_taskdependencies] sınıfı:

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

Bu kod parçacığını görev kimliği "Çiçekler" bağımlı bir görev oluşturur. "Çiçekler" görev görevlere "Yağmur" ve "Sun" bağlıdır. Görev "çiçekler", "Yağmur" ve "Sun" tamamladınız görevleri sonra işlem düğümü üzerinde çalışacak şekilde zamanlanır.

> [!NOTE]
> İçinde olduğunda başarıyla tamamlanması için bir görev olarak kabul edilir **tamamlandı** durumu ve kendi **çıkış kodu** olan `0`. Batch .NET içinde yani bir [CloudTask][net_cloudtask].[ Durumu] [ net_taskstate] özellik değerinin `Completed` ve CloudTask's [TaskExecutionInformation][net_taskexecutioninformation].[ ExitCode] [ net_exitcode] özellik değeri `0`.
> 
> 

## <a name="dependency-scenarios"></a>Bağımlılık senaryoları
Azure Batch kullanabileceğiniz üç temel görev bağımlılık senaryo vardır: birebir, bir çok ve görev kimliği aralığı bağımlılık. Bunlar çok-bir dördüncü senaryo sağlamak üzere birleştirilebilir.

| Senaryo&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Örnek |  |
|:---:| --- | --- |
|  [Bire bir](#one-to-one) |*Görevb* bağlıdır *Göreva* <p/> *Görevb* kadar yürütülmek zamanlanmaz *Göreva* başarıyla tamamlandı |![Diyagram: bire bir görev bağımlılığı][1] |
|  [Bir çok](#one-to-many) |*görevC* hem *görevA* hem de *görevB*’ye bağlıdır <p/> *Görevc* her ikisi de kadar yürütülmek zamanlanmaz *Göreva* ve *Görevb* başarıyla tamamladınız. |![Diyagram: bir çok görev bağımlılığı][2] |
|  [Görev Kimliği aralığı](#task-id-range) |*Görevd* bir dizi göreve bağlıdır <p/> *Görevd* kimlikleri görevlerle kadar yürütülmek zamanlanmaz *1* aracılığıyla *10* başarıyla tamamladınız. |![Diyagram: Görev kimliği aralığı bağımlılığı][3] |

> [!TIP]
> Oluşturabileceğiniz **çok-** burada görevlere A ve b C D, E ve F her görevleri bağlı gibi ilişkileri Bu, örneğin, paralel birkaç ölçeklendirin önişlem senaryolarında olduğu birden çok Yukarı Akış görevi Çıkışta aşağı akış görevlerinizi bağımlı yararlıdır.
> 
> Yalnızca üst görevleri başarıyla tamamlandıktan sonra bu bölümde örneklerde bağımlı bir görev çalıştırır. Bu davranış, bağımlı bir görev için varsayılan davranıştır. Varsayılan davranışı geçersiz kılmak için bir bağımlılık eylemi belirterek üst görev başarısız olduktan sonra bağımlı görev çalıştırabilirsiniz. Bkz: [bağımlılık Eylemler](#dependency-actions) ayrıntıları bölümü.

### <a name="one-to-one"></a>Bire bir
Bire bir ilişkide bir görev bir üst görev başarılı tamamlanma bağlıdır. Bağımlılık oluşturmak için bir tek görev kimliği sağlayın [Github_samples][net_taskdependencies].[ OnId] [ net_onid] , doldurmak statik yöntemi [DependsOn] [ net_dependson] özelliği [CloudTask][net_cloudtask].

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a>Bir çok
Bir-çok ilişkisi görev birden çok üst görevleri tamamlama sonrasında bağlıdır. Bağımlılık oluşturmak için görev kimlikleri koleksiyonunu sağlamak [Github_samples][net_taskdependencies].[ OnIds] [ net_onids] , doldurmak statik yöntemi [DependsOn] [ net_dependson] özelliği [CloudTask][net_cloudtask].

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
Bir görevin üst görevleri bir dizi bir bağımlılık olarak bağlı olan kimlikleri bir aralıkta bulunan görevlerin.
Bağımlılık oluşturmak için ilk sağlayın ve aralıktaki son kimlikleri görev [Github_samples][net_taskdependencies].[ OnIdRange] [ net_onidrange] , doldurmak statik yöntemi [DependsOn] [ net_dependson] özelliği [CloudTask][net_cloudtask].

> [!IMPORTANT]
> Görev Kimliği aralıkları için kullandığınızda, bağımlılıkları, görev kimlikleri aralığında *gerekir* tamsayı değerleri dize gösterimlerini olabilir.
> 
> Her görev aralığında bağımlılık başarıyla tamamlayarak veya ayarlamak bir bağımlılık eylemi eşlenmiş bir hata ile tamamlayarak getirmelidir **Satisfy**. Bkz: [bağımlılık Eylemler](#dependency-actions) ayrıntıları bölümü.
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

## <a name="dependency-actions"></a>Bağımlılık Eylemler

Yalnızca bir üst görev başarıyla tamamlandıktan sonra varsayılan olarak, bağımlı görev veya dizi görevi çalıştırır. Bazı senaryolarda, üst görev başarısız olsa bile bağımlı görevleri çalıştırmak isteyebilirsiniz. Bir bağımlılık eylemi belirterek varsayılan davranışı geçersiz kılabilirsiniz. Bir bağımlılık eylemi, başarı veya başarısızlık üst görevinin durumuna göre bağımlı görevi çalıştırmak uygun olup olmadığını belirtir. 

Örneğin, bağımlı bir görev tamamlandığında Yukarı Akış görevi, verilerden bekliyor varsayalım. Yukarı Akış görevi başarısız olursa, bağımlı görev eski verileri kullanarak çalışacak şekilde mümkün olabilir. Bu durumda, bir bağımlılık eylemi bağımlı görevin üst göreve hatası rağmen çalıştırmak uygun olduğunu belirtebilirsiniz.

Bir bağımlılık eylemi üst görev için bir çıkış koşulunu temel alır. Aşağıdaki çıkış koşullardan herhangi biri için bir bağımlılık eylemi belirtebilirsiniz; .NET için bkz: [ExitConditions] [ net_exitconditions] sınıfı Ayrıntılar için:

- Ön işleme bir hata oluştuğunda.
- Bir dosyayı karşıya yüklemeyi hata oluşur. Görev aracılığıyla belirtilen çıkış kodu ile çıkar varsa **exitCodes** veya **exitCodeRanges**, ve ardından bir dosyayı karşıya yükleme hatası, çıkış kodu tarafından belirtilen eylemi karşılaştığı önceliklidir.
- Görev çıktığında tarafından tanımlanan bir çıkış kodu ile **ExitCodes** özelliği.
- Görev çıktığında tarafından belirtilen bir aralıkta denk bir çıkış kodu ile **ExitCodeRanges** özelliği.
- Varsayılan durumda, görev tarafından tanımlanmamış bir çıkış kodu ile bulunup bulunmadığını **ExitCodes** veya **ExitCodeRanges**, veya görev önceden işleme bir hata ile bulunup bulunmadığını ve **PreProcessingError** özelliği ayarlı değil, veya görev bir dosyayla başarısız olursa hata karşıya yükleme ve **FileUploadError** özelliği ayarlı değil. 

.NET içinde bir bağımlılık eylemi belirtmek için ayarlayın [ExitOptions][net_exitoptions].[ DependencyAction] [ net_dependencyaction] çıkış koşulu özelliği. **DependencyAction** özelliği iki değerden birini alır:

- Ayarı **DependencyAction** özelliğine **Satisfy** bağımlı görevler belirtilen bir hata ile üst görev bulunup bulunmadığını çalıştırmak uygun olduğunu gösterir.
- Ayarı **DependencyAction** özelliğine **blok** bağımlı görevleri çalıştırmak uygun olmadığını gösterir.

İçin varsayılan ayar **DependencyAction** özelliği **Satisfy** çıkış kodu 0 için ve **blok** diğer tüm çıkış koşulları için.

Aşağıdaki kod parçacığını kümeleri **DependencyAction** özelliği için bir üst görev. Ön işleme hata veya belirtilen hata kodları üst görev bulunup bulunmadığını bağımlı görevi engellendi. Başka bir sıfır olmayan hata ile üst görev bulunup bulunmadığını bağımlı görevi çalıştırmak uygundur.

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
[Github_samples] [ github_taskdependencies] örnek proje biridir [Azure Batch kod örnekleri] [ github_samples] github'da. Bu Visual Studio çözümü gösterir:

- Bir iş üzerinde görev bağımlılığı etkinleştirme
- Diğer görevlere bağlı görevlerin oluşturma
- Bu görevleri bir işlem düğümleri havuzu üzerinde yürütmek nasıl.

## <a name="next-steps"></a>Sonraki adımlar
### <a name="application-deployment"></a>Uygulama dağıtımı
[Uygulama paketleri](batch-application-packages.md) toplu özellik kolay bir yol sağlar hem de dağıtmak ve görevlerinizi yürütmek uygulamaları işlem düğümlerini sürümü.

### <a name="installing-applications-and-staging-data"></a>Uygulama yükleme ve verileri hazırlama
Bkz: [uygulamaların yüklenmesi ve toplu veri hazırlama işlem düğümlerini] [ forum_post] görevleri çalıştırmak için düğümü hazırlama yöntemlerine genel bakış için Azure Batch Forumunda. Azure Batch ekip üyelerinin biri tarafından yazılmış, bu uygulamalar, görev giriş verilerinin ve diğer dosyalar işlem düğümleriniz kopyalamak için çeşitli yollar hakkında iyi öncü postasıdır.

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
[net_dependencyaction]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.exitoptions#Microsoft_Azure_Batch_ExitOptions_DependencyAction
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_onid]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onid.aspx
[net_onids]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onids.aspx
[net_onidrange]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onidrange.aspx
[net_taskexecutioninformation]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_usestaskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.usestaskdependencies.aspx
[net_taskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskdependencies.aspx

[1]: ./media/batch-task-dependency/01_one_to_one.png "Diyagram: bire bir bağımlılık"
[2]: ./media/batch-task-dependency/02_one_to_many.png "Diyagram: bir çok bağımlılık"
[3]: ./media/batch-task-dependency/03_task_id_range.png "Diyagram: görev kimliği aralığı bağımlılığı"
