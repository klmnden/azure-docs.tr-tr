---
title: Görevleri durumunu - Azure Batch tarafından sayım tarafından bir işin ilerleme durumunu izlemek | Microsoft Docs
description: Bir iş için görevleri saymak için görev sayar alma işlemi çağırarak bir işin ilerleme durumunu izleyin. Etkin, çalışan ve tamamlanan görevler ve başarılı veya başarısız olduğunu görevler tarafından sayımı elde edebilirsiniz.
services: batch
author: dlepow
manager: jeconnoc
ms.service: batch
ms.topic: article
ms.date: 08/02/2017
ms.author: danlep
ms.openlocfilehash: bc112ed5b481560362962d6b550d336de6b3d9b4
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
ms.locfileid: "30312652"
---
# <a name="count-tasks-by-state-to-monitor-a-jobs-progress-preview"></a>Görevler bir işin ilerleme durumunu (Önizleme) izlemek için duruma göre Say

Azure toplu işlem görevlerinin çalıştırdığı bir işin ilerleme durumunu izlemek için etkili bir yol sağlar. Çağırabilirsiniz [alma görev sayar] [ rest_get_task_counts] bir etkin, çalışıyor veya tamamlanmış durumda kaç görevlerdir ve kaç tane sahip öğrenin işlemi başarılı veya başarısız oldu. Her durumda görev sayısı sayım tarafından daha kolay işin ilerleme durumunu bir kullanıcıya görüntüleyebilir, ya da beklenmeyen gecikmeler veya iş etkileyebilecek hataları algılar.

> [!IMPORTANT]
> Görev sayar alma işlemi şu anda önizlemede değil ve henüz Azure kamu, Azure Çin ve Azure Almanya kullanılabilir değil. 
>
>

## <a name="how-tasks-are-counted"></a>Görevlerin nasıl sayılır

Görev sayar alma işlemi aşağıdaki gibi görevleri durumuna göre sayar:

- Bir görev olarak sayılır **etkin** zaman sıraya alınan ve yapabileceksiniz Çalıştır, ancak şu anda bir işlem düğümünde atanmadı. Bir görev olarak da sayılır **etkin** henüz tamamlanmadı üst göreve bağımlı olması durumunda. Görev bağımlılıkları ile ilgili daha fazla bilgi için bkz: [diğer görevlere bağlı görevleri çalıştırmak için görev bağımlılıkları oluşturmak](batch-task-dependencies.md). 
- Bir görev olarak sayılır **çalıştıran** ne zaman bir işlem düğümüne atanmış, ancak henüz tamamlanmadı. Bir görev olarak sayılır **çalıştıran** durumuna olduğunda ya da `preparing` veya `running`belirtildiği gibi [bir görev hakkında bilgi alma] [ rest_get_task] işlemi.
- Bir görev olarak sayılır **tamamlandı** olduğu zaman artık çalıştırmak uygun. Bir görev olarak sayılan **tamamlandı** sahip genellikle başarıyla tamamlandı ya da veya başarısız sona erdi ve ayrıca, yeniden deneme sınırını aştı. 

Görev sayar alma işlemi de kaç görevlerin başarılı veya başarısız olduğunu bildirir. Toplu göreve başarılı olup kontrol ederek belirler **sonuç** [executionInfo] özelliği [https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task#executionInfo] özelliği:

    - Bir görev olarak sayılır **başarılı** görev yürütme sonuç ise `success`.
    - Bir görev olarak sayılır **başarısız** görev yürütme sonuç ise `failure`.

Görev durumları hakkında daha fazla bilgi için bkz: [bir görev hakkında bilgi alma][rest_get_task].

Aşağıdaki .NET kod örneği, görev sayıları durumuna göre almak gösterilmektedir: 

```csharp
var taskCounts = await batchClient.JobOperations.GetTaskCountsAsync("job-1");

Console.WriteLine("Task count in active state: {0}", taskCounts.Active);
Console.WriteLine("Task count in preparing or running state: {0}", taskCounts.Running);
Console.WriteLine("Task count in completed state: {0}", taskCounts.Completed);
Console.WriteLine("Succeeded task count: {0}", taskCounts.Succeeded);
Console.WriteLine("Failed task count: {0}", taskCounts.Failed);
Console.WriteLine("ValidationStatus: {0}", taskCounts.ValidationStatus);
```

> [!NOTE]
> Bir işi için görev sayılarını elde etmek için geri KALANI için benzer bir desen ve diğer desteklenen dilleri kullanabilirsiniz. 
> 
> 

## <a name="consistency-checking-for-task-counts"></a>Tutarlılık denetimini görev sayılarını

Batch hizmeti, birden çok zaman uyumsuz bir Dağıtılmış Sistem parçalarını veri toplamayı tarafından görev sayıları toplar. Görev sayıları doğru, tutarlılık gerçekleştirerek toplu durumu sayıları ek doğrulama sağlayan emin olmak için birden çok sistem bileşenlerinin karşı denetler. Vardır sürece daha azını 200.000 görevleri işinde toplu bu tutarlılık denetimleri gerçekleştirir. Tutarlılık denetimi hatalar bulur olası olayında toplu tutarlılık denetimi sonuçlarına dayalı görevleri sayar alma işleminin sonucu düzeltir. Tutarlılık denetimi Al görevi sayar işlemi kullanan müşteriler kendi çözüm için gereken doğru bilgileri aldığından emin olmak için fazladan bir ölçüsüdür.

**ValidationStatus** yanıt özelliği, toplu tutarlılık denetimi gerçekleştirip gerçekleştirmediğini belirtir. Toplu iş durumu sistemde tutulan gerçek durumları düşülür denetlemek değil kurabiliyorsa sonra **validationStatus** özelliği ayarlanmış `unvalidated`. Birden çok 200.000 görevleri, iş içeriyorsa, performans nedenleriyle toplu tutarlılık denetimi gerçekleştirmez böylece **validationStatus** özellik kümesine `unvalidated` bu durumda. Çok sınırlı veri kaybı son derece düşüktür ancak görev sayısı bu durumda, mutlaka yanlış değildir. 

Bir görev durumu değiştiğinde, toplama ardışık düzen değişiklik birkaç saniye içinde işler. Görev sayar alma işlemi, bu süre içinde güncelleştirilmiş görev sayıları yansıtır. Görev durumundaki bir değişikliği toplama ardışık düzen isabetsizliği varsa, ancak, ardından bu değişikliği kadar sonraki doğrulama geçişini kayıtlı değil. Bu süre boyunca görev sayıları eksik olayı nedeniyle biraz yanlış olabilir, ancak sonraki doğrulama geçişte düzeltilir.

## <a name="best-practices-for-counting-a-jobs-tasks"></a>Bir iş görevlerinin saymak için en iyi uygulamalar

Görev sayar alma işlemi çağırmadan durumuna göre bir iş görevlerinin temel sayısını döndürmek için en etkili yoldur. Batch hizmeti sürümü 2017 06 01.5.1 kullanıyorsanız, yazma ya da kodunuzu almak görev sayar kullanmak için güncelleştirme öneririz.

Görev sayar alma işlemi 2017 06 01.5.1'den önceki toplu işlem hizmeti sürümlerinde kullanılamaz. Hizmeti daha eski bir sürümü kullanıyorsanız, bir liste sorgusu işteki görevler yerine saymak için kullanın. Daha fazla bilgi için bkz: [listelemek için Create sorguları toplu kaynaklarını verimli bir şekilde](batch-efficient-list-queries.md).

## <a name="next-steps"></a>Sonraki adımlar

* Batch hizmeti kavramları ve özellikler hakkında daha fazla bilgi edinmek için bkz. [Batch özelliklerine genel bakışı](batch-api-basics.md). Makale havuzlar, işlem düğümleri, işler ve görevler gibi birincil Batch kaynaklarını ele ve hizmetin özelliklerine genel bir bakış sağlar.
* [Batch .NET istemci kitaplığı](batch-dotnet-get-started.md) veya [Python](batch-python-tutorial.md) kullanarak Batch özellikli bir uygulama geliştirmenin temellerini öğrenin. Bu Tanıtım makaleler birden çok işlem düğümlerinde iş yükünü yürütmek için Batch hizmetini kullanan çalışan bir uygulama size kılavuzluk eder.


[rest_get_task_counts]: https://docs.microsoft.com/rest/api/batchservice/get-the-task-counts-for-a-job
[rest_get_task]: https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task
[rest_list_tasks]: https://docs.microsoft.com/rest/api/batchservice/list-the-tasks-associated-with-a-job
