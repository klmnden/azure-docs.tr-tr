---
title: İşlem kaynaklarını verimli bir şekilde - kullanmak için paralel olarak Azure Batch görevlerini çalıştırmak | Microsoft Docs
description: Azure Batch havuzundaki her düğümde daha az işlem düğümlerini ve eş zamanlı çalışan görevleri kullanarak verimliliği ve maliyetleri düşüren
services: batch
documentationcenter: .net
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: 538a067c-1f6e-44eb-a92b-8d51c33d3e1a
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/17/2019
ms.author: lahugh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 79b45bd423ed6715cdb7cc7c0e079c150eefede5
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63763704"
---
# <a name="run-tasks-concurrently-to-maximize-usage-of-batch-compute-nodes"></a>İşlem düğümleri Batch kullanımını eşzamanlı olarak en üst düzeye çıkarmak için görevleri Çalıştır 

Birden fazla görevin eşzamanlı olarak, Azure Batch havuzundaki her işlem düğümünde çalıştırmak, kaynak kullanımını daha küçük bir havuzdaki düğümlerin sayısı en üst düzeye çıkarabilirsiniz. Bazı iş yükleri için bu kısa iş süreleri ve daha düşük maliyetli neden olabilir.

Bir düğümün kaynakların tümünü tek bir görev için ayrılmış bazı senaryolarda yararlı olsa da bu kaynakları paylaşmak birden çok görev izin vermesini çeşitli durumlarda yararlanabilirsiniz:

* **Veri aktarımını en aza** görevleri olduğunda veri paylaşabilir. Bu senaryoda, paylaşılan veri düğümler küçük bir sayıya kopyalayarak ve her bir düğümde Paralel Görevler yürütülürken veri aktarımı ücretlerini önemli ölçüde azaltabilir. Bu özellikle, her düğüme kopyalanacak verileri coğrafi bölgeler arasında aktarılması gereken geçerlidir.
* **Bellek kullanımını en üst düzeye** görevleri büyük miktarda bellek, ancak yalnızca kısa süreli süreyi ve yürütme sırasında değişken zamanlarda sırasında ne zaman gerektirir. Bu tür ani verimli bir şekilde işlemek için daha fazla bellek ile daha az sayıda ancak daha büyük işlem düğümleri kullanabilirsiniz. Bu düğümü birden çok görevi paralel olarak her bir düğümde çalışan olur, ancak farklı zamanlarda her görev düğümlerinin çoktur bellek yararlanmak.
* **Düğüm sayısı sınırları Azaltıcı** düğümler arası iletişimin olduğunda bir havuz gerekli. Şu anda, 50 işlem düğümlerine düğümler arası iletişim için yapılandırılmış havuzlarında sınırlıdır. Böyle bir havuzdaki her düğüme görevleri paralel olarak yürütmek için ise çok büyük sayıda görev aynı anda çalıştırılabilir.
* **Bir şirket içi işlem kümesi çoğaltma**, ilk işlem ortamı Azure'a taşıdığınızda gibi. İşlem düğümü başına birden çok görev geçerli şirket içi çözümünüzü yürütür, düğüm görevleri daha yakından söz konusu yapılandırmayı yansıtmak üzere en fazla sayısını artırabilirsiniz.

## <a name="example-scenario"></a>Örnek senaryo
Paralel görev yürütmeye avantajlarını göstermek için örnek olarak, varsayalım görev uygulamanızın CPU ve bellek gereksinimleri karşıladığından emin olacak şekilde [standart\_D1](../cloud-services/cloud-services-sizes-specs.md) düğümlerdir yeterli. Ancak, bu düğümler 1.000 işi gereken sürede tamamlamak için gerekli.

Standart kullanmak yerine\_1 CPU çekirdeği D1 düğüm, kullanabilir [standart\_D14](../cloud-services/cloud-services-sizes-specs.md) 16 çekirdeğe sahip ve paralel görev yürütmeye etkinleştirme düğümleri. Bu nedenle, *16 kat daha az düğümü* 63 gerekli yalnızca--1.000 düğümler yerine kullanılabilir. Büyük uygulama dosyaları veya başvuru verileriyle her düğüm için gerekli olduğunda, veriler yalnızca 63 düğümlerine kopyaladığınızdan ek olarak, iş süresi ve verimlilik yeniden geliştirildi.

## <a name="enable-parallel-task-execution"></a>Paralel görev yürütmeye etkinleştir
Paralel görev yürütmeye için işlem düğümleri havuzu düzeyinde yapılandırdığınız. Batch .NET kitaplığı ile ayarlanmış [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] bir havuz oluşturduğunuzda özelliği. Batch REST API'SİNİN kullanıyorsanız [maxTasksPerNode] [ rest_addpool] havuz oluşturma sırasında istek gövdesindeki öğesi.

Azure Batch görevleri (4 x) en fazla düğüm başına ayarlamanıza olanak sağlayan çekirdek düğüm sayısı. Havuz düğümleri ile yapılandırılmışsa, örneğin, "Büyük" (dört çekirdek), ardından boyut `maxTasksPerNode` 16 olarak ayarlanabilir. Ancak, düğüm kaç çekirdek bağımsız olarak, düğüm başına 256 karakterden daha fazla görev sahip olamaz. Her düğümü boyutları için çekirdek sayısı hakkında daha fazla bilgi için bkz: [Cloud Services boyutları](../cloud-services/cloud-services-sizes-specs.md). Hizmet sınırları hakkında daha fazla bilgi için bkz. [Azure Batch hizmeti için kotalar ve sınırlar](batch-quota-limit.md).

> [!TIP]
> Hesaba katması mutlaka `maxTasksPerNode` değeri oluşturduğunuzda, bir [otomatik ölçeklendirme formülü] [ enable_autoscaling] havuzunuz için. Örneğin, veren bir formül `$RunningTasks` önemli ölçüde artış düğüm başına görev tarafından etkilenebilir. Bkz: [işlem düğümleri Azure Batch havuzunda otomatik olarak](batch-automatic-scaling.md) daha fazla bilgi için.
>
>

## <a name="distribution-of-tasks"></a>Dağıtım görevleri
Bir havuzdaki işlem düğümlerine görevleri eşzamanlı olarak yürütebilir, havuzdaki düğümler arasında dağıtılması için görevleri nasıl istediğinizi belirtmek önemlidir.

Kullanarak [CloudPool.TaskSchedulingPolicy] [ task_schedule] özelliği görevleri ("yayılma") havuzdaki tüm düğümlere eşit olarak atanmasını belirtebilirsiniz. Veya görevleri ("Paket") havuzdaki başka bir düğüme atanır önce mümkün olduğunca çok görevleri her düğüme atanmalıdır belirtebilirsiniz.

Bu özelliği nasıl değerlidir ilişkin bir örnek olarak, havuzu göz önünde bulundurun [standart\_D14](../cloud-services/cloud-services-sizes-specs.md) düğümleri (yukarıdaki örnekte) ile yapılandırılmış bir [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] değer 16. Varsa [CloudPool.TaskSchedulingPolicy] [ task_schedule] ile yapılandırılmış bir [ComputeNodeFillType] [ fill_type] , *paketi*, her düğümün tüm 16 Çekirdek kullanımını en üst düzeye çıkarmak ve izin bir [otomatik ölçeklendirme havuzu](batch-automatic-scaling.md) kullanılmayan düğümler (düğüm atanan tüm görevleri olmadan) havuzundan ayıklamak üzere. Bu, kaynak kullanımını en aza indirir ve masraflarından tasarruf edebilirsiniz.

## <a name="batch-net-example"></a>Batch .NET örnek
Bu [Batch .NET] [ api_net] API kod parçacığı, düğüm başına dört görev en fazla dört düğüm içeren bir havuz oluşturma isteği göstermektedir. Bu, bir görev her düğüm görevleri havuzdaki başka bir düğüme atamadan önce görevlerle doldurur ilke zamanlama belirtir. Batch .NET API'si kullanılarak havuzları ekleme ile ilgili daha fazla bilgi için bkz: [BatchClient.PoolOperations.CreatePool][poolcreate_net].

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicatedComputeNodes: 4
        virtualMachineSize: "standard_d1_v2",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));

pool.MaxTasksPerComputeNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

## <a name="batch-rest-example"></a>Batch REST örneği
Bu [Batch REST] [ api_rest] API kod parçacığı, düğüm başına dört görev en fazla iki büyük düğümleri içeren bir havuz oluşturmak için bir istek gösterir. REST API kullanarak havuzları ekleme ile ilgili daha fazla bilgi için bkz: [hesaba havuz ekleme][rest_addpool].

```json
{
  "odata.metadata":"https://myaccount.myregion.batch.azure.com/$metadata#pools/@Element",
  "id":"mypool",
  "vmSize":"large",
  "cloudServiceConfiguration": {
    "osFamily":"4",
    "targetOSVersion":"*",
  }
  "targetDedicatedComputeNodes":2,
  "maxTasksPerNode":4,
  "enableInterNodeCommunication":true,
}
```

> [!NOTE]
> Ayarlayabileceğiniz `maxTasksPerNode` öğesi ve [MaxTasksPerComputeNode] [ maxtasks_net] havuzu oluşturma zamanında yalnızca özellik. Bunlar zaten bir havuzu oluşturulduktan sonra değiştirilemez.
>
>

## <a name="code-sample"></a>Kod örneği
[ParallelNodeTasks] [ parallel_tasks_sample] GitHub projede kullanımını göstermektedir [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] özelliği.

Bu C# konsol uygulaması kullanan [Batch .NET] [ api_net] kitaplığı ile bir veya daha fazla işlem düğümleri havuzu oluşturmak için. Bu görevler yapılandırılabilir sayıda değişken yükünün benzetimini yapmak için bu düğümlerde yürütür. Uygulamadan çıktı, her görevin çalıştığı düğümleri yürütülen belirtir. Uygulama ayrıca iş parametresi ve süresi bir özetini sağlar. İki farklı çalışma örnek uygulamasının çıktısı Özet bölümü altında görünür.

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

Örnek uygulamayı ilk kez yürütülmesi, ile havuz ve varsayılan ayarı, bir görev, düğüm başına tek bir düğüm üzerinde 30 dakika iş süresi olduğunu gösterir.

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

İkinci örnek gösterir iş süresi önemli ölçüde bir düşüş çalıştırın. Havuzu neredeyse zaman Çeyrek içinde işi tamamlamak paralel görev yürütmeye izin veren, düğüm başına dört görev ile yapılandırılmış olmasıdır.

> [!NOTE]
> İş süreleri yukarıdaki özetleri içinde havuz oluşturma zamanı içermez. Yukarıdaki işlerinin her biri olan işlem düğümleri, daha önce oluşturulan havuzlar için gönderilen *boşta* gönderme zaman durum.
>
>

## <a name="next-steps"></a>Sonraki adımlar
### <a name="batch-explorer-heat-map"></a>Batch Explorer ısı Haritası
[Batch Explorer] [ batch_labs] oluşturma, hata ayıklama ve izleme Azure Batch uygulamalarıyla yardımcı olmak için bir ücretsiz ve zengin özellikli tek başına istemci aracıdır. Batch Gezgini içeren bir *ısı Haritası* görselleştirme görev yürütme sağlayan özelliktir. Ne zaman yürütme [ParallelTasks] [ parallel_tasks_sample] örnek uygulama, her bir düğümde Paralel Görevler yürütülmesini kolayca görselleştirmek için ısı Haritası özelliğini kullanabilirsiniz.


[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_labs]: https://azure.github.io/BatchExplorer/
[cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[enable_autoscaling]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[fill_type]: https://msdn.microsoft.com/library/microsoft.azure.batch.common.computenodefilltype.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[rest_addpool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[task_schedule]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudpool.taskschedulingpolicy.aspx

