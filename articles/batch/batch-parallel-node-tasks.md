---
title: İşlem kaynaklarını verimli bir şekilde - kullanmak için paralel olarak Azure Batch görevleri çalıştırmak | Microsoft Docs
description: Azure Batch havuzundaki her düğümde daha az işlem düğümlerini ve çalışan eş zamanlı görevleri kullanarak verimliliği ve maliyetleri artırın
services: batch
documentationcenter: .net
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: 538a067c-1f6e-44eb-a92b-8d51c33d3e1a
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5106bbbb073908af7e7e8f045fa6fb60e8a306f4
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="run-tasks-concurrently-to-maximize-usage-of-batch-compute-nodes"></a>İşlem düğümleri eş zamanlı olarak toplu kullanımını en üst düzeye çıkarmak için görevleri Çalıştır 

Birden fazla görev aynı anda Azure Batch havuzundaki her işlem düğümünde çalıştırarak, havuzdaki düğümlerin daha az sayıda kaynak kullanımı en üst düzeye çıkarabilirsiniz. Bazı iş yükleri için bu kısa iş süreleri ve daha düşük maliyetli neden olabilir.

Bir düğümün kaynakların tümünü tek bir görev ayrılması bazı senaryolarda yararlı olsa da, bazı durumlarda bu kaynakları paylaşmak birden çok görev izin vererek yararlanabilirsiniz:

* **Veri aktarımı en aza** görevleri olduğunda veri paylaşamaz. Bu senaryoda, paylaşılan veri düğümlerinin daha küçük bir sayıya kopyalayarak ve her bir düğümde Paralel Görevler yürütülürken veri aktarımı ücretlerine önemli ölçüde azaltabilir. Bu özellikle, her düğüme kopyalanacak veri coğrafi bölgeler arasında aktarılması geçerlidir.
* **Bellek kullanımını en üst düzeye çıkarma** görevleri bellek, ancak yalnızca dönemlerde kısa süre ve yürütme sırasında değişken zamanlarda büyük bir miktarını ne zaman gerektirir. Bu tür ani verimli bir şekilde işlemek için daha fazla belleğe sahip daha az, ancak daha büyük, işlem düğümleri tercih edebilirsiniz. Bu düğümler her düğümde paralel olarak çalışan birden çok görev gerekir, ancak farklı zamanlarda her görev düğümleri çoktur bellek yararlanmak.
* **Düğüm sayı sınırları Azaltıcı** düğümler arası iletişimin olduğunda bir havuzu içinde gerekli. Şu anda, düğümler arası iletişim için yapılandırılan havuzları 50 işlem düğümlerine sınırlıdır. Bu tür bir havuzdaki her düğüme görevleri paralel olarak yürütmek mümkün ise, görevlerin daha fazla sayıda eşzamanlı olarak çalıştırılabilir.
* **Bir şirket içi işlem kümesi çoğaltma**, önce bir bilgi işlem ortamı için Azure taşıdığınızda gibi. Geçerli şirket içi çözümünüzü işlem düğümü başına birden çok görev yürütülürse, düğüm görevleri daha yakından yapılandırmayı yansıtmak üzere en fazla sayısını artırabilirsiniz.

## <a name="example-scenario"></a>Örnek senaryo
Paralel görev yürütme yararları göstermek için örnek olarak, diyelim görev uygulamanız CPU ve bellek gereksinimlerini sahip olacağı şekilde [standart\_D1](../cloud-services/cloud-services-sizes-specs.md) düğümleri yeterli. Ancak, bu düğümler 1.000 gerekli sürede işini bitirmesi için gereklidir.

Standart kullanmak yerine\_1 CPU çekirdeği olan D1 düğümleri kullanabilir [standart\_D14](../cloud-services/cloud-services-sizes-specs.md) 16 çekirdeğe sahip ve paralel görev yürütme etkinleştiren düğümleri. Bu nedenle, *16 kez daha az düğümü* 63 gerekli olacak yalnızca--1.000 düğümleri yerine kullanılabilir. Büyük uygulama dosyaları veya başvuru verileri her düğüm için gerekirse, yalnızca 63 düğümlerine kopyalanan verileri bu yana ek olarak, iş süresi ve verimlilik yeniden geliştirildi.

## <a name="enable-parallel-task-execution"></a>Paralel görev yürütme etkinleştir
Paralel görev yürütme için işlem düğümleri havuzu düzeyinde yapılandırın. Batch .NET kitaplığı ile ayarlamak [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] bir havuz oluşturduğunuzda özelliği. Batch REST API'si kullanıyorsanız ayarlamak [maxTasksPerNode] [ rest_addpool] havuzu oluşturma sırasında istek gövdesindeki öğesi.

Azure toplu işlem düğümü başına en fazla görevleri en çok dört kez ayarlamanıza olanak verir (4 x) düğüm çekirdeği sayısı. Havuzun düğümleri ile yapılandırılmışsa, örneğin, "Büyük" (dört çekirdek), ardından boyut `maxTasksPerNode` 16 olarak ayarlanabilir. Her düğümü boyutları için çekirdek sayısı hakkında daha fazla bilgi için bkz: [Cloud Services boyutları](../cloud-services/cloud-services-sizes-specs.md). Hizmet sınırları hakkında daha fazla bilgi için bkz: [Azure Batch hizmeti için kotalar ve sınırlar](batch-quota-limit.md).

> [!TIP]
> Dikkate aldığınızdan emin olun `maxTasksPerNode` değeri, yapısı oluştururken bir [otomatik ölçeklendirme formülü] [ enable_autoscaling] havuzunuz için. Örneğin, veren bir formül `$RunningTasks` önemli ölçüde görevleri düğümü başına bir artış tarafından etkilenebilir. Bkz: [ölçek işlem düğümlerini Azure Batch havuzunda otomatik olarak](batch-automatic-scaling.md) daha fazla bilgi için.
>
>

## <a name="distribution-of-tasks"></a>Dağıtım görevleri
Bir havuzdaki işlem düğümleri görevleri aynı anda çalıştırılabildiği zaman nasıl görevleri havuzdaki düğümlerin dağıtılmasını istediğinizi belirtmek önemlidir.

Kullanarak [CloudPool.TaskSchedulingPolicy] [ task_schedule] özelliği, görevler ("yayılmak") havuzdaki tüm düğümlere eşit olarak atanmalıdır belirtebilirsiniz. Veya görevler için ("Ara") havuzundaki başka bir düğüme atanır önce mümkün olduğunca gibi birçok görevi her düğüme atanmalıdır belirtebilirsiniz.

Bu özellik nasıl değerlidir örnek olarak, havuzu göz önünde bulundurun [standart\_D14](../cloud-services/cloud-services-sizes-specs.md) ile yapılandırılmış düğümlerin (yukarıdaki örnekte) bir [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] 16 değeri. Varsa [CloudPool.TaskSchedulingPolicy] [ task_schedule] ile yapılandırılmış bir [ComputeNodeFillType] [ fill_type] , *paketi*, her düğümün tüm 16 çekirdeğe kullanımını en üst düzeye ve izin bir [otomatik ölçeklendirmeyi havuzu](batch-automatic-scaling.md) kullanılmayan düğümler (düğüm atanmış herhangi bir görevi olmadan) havuzundan ayıklanacağını. Bu kaynak kullanımını en aza indirir ve para kaydeder.

## <a name="batch-net-example"></a>Batch .NET örnek
Bu [Batch .NET] [ api_net] API kod parçacığını en fazla düğüm başına dört görevler dört büyük düğümleri içeren bir havuz oluşturma isteği gösterir. Bir görev her düğüm havuzundaki başka bir düğüme görev atamadan önce görevlerle doldurur ilke zamanlama belirtir. Batch .NET API'si kullanarak havuzları ekleme ile ilgili daha fazla bilgi için bkz: [BatchClient.PoolOperations.CreatePool][poolcreate_net].

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicatedComputeNodes: 4
        virtualMachineSize: "large",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

pool.MaxTasksPerComputeNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

## <a name="batch-rest-example"></a>Batch REST örneği
Bu [Batch REST] [ api_rest] API parçacığı en fazla düğüm başına dört görevleri iki büyük düğümleri içeren bir havuz oluşturma isteği gösterir. REST API kullanarak havuzları ekleme ile ilgili daha fazla bilgi için bkz: [bir havuz için bir hesap eklemek][rest_addpool].

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
> Ayarlayabileceğiniz `maxTasksPerNode` öğesi ve [MaxTasksPerComputeNode] [ maxtasks_net] havuzu oluşturma zamanında yalnızca özelliği. Bir havuzu zaten oluşturulduktan sonra bunlar değiştirilemez.
>
>

## <a name="code-sample"></a>Kod örneği
[ParallelNodeTasks] [ parallel_tasks_sample] GitHub projede kullanımını göstermektedir [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] özelliği.

Bu C# konsol uygulaması kullanan [Batch .NET] [ api_net] kitaplığı ile bir veya daha fazla işlem düğümleri havuzu oluşturun. Bu görevler yapılandırılabilir bir dizi değişken yük benzetimini yapmak için bu düğümlerde yürütür. Uygulamadan çıktı, her görevin hangi düğümlerin yürütülen belirtir. Uygulama, ayrıca iş parametrelerini ve süresi bir özetini sağlar. İki farklı çalıştırmalarını örnek uygulamasının çıktısı Özet kısmı aşağıda yer almaktadır.

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

Örnek uygulamayı ilk kez yürütülmesi havuzunu ve her düğüm, bir görevin varsayılan ayar olarak tek bir düğüm ile iş süresi boyunca 30 dakikadır gösterir.

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

İkinci iş süresi önemli bir düşüş örnek gösterir çalıştırın. Paralel görev yürütme süresi yaklaşık bir çeyrekte işi tamamlamak sağlayan düğüm başına dört görevlerle havuzu yapılandırılmış olmasıdır.

> [!NOTE]
> Yukarıdaki özetleri iş süreleri havuzu oluşturma zamanı içermez. İşlem düğümleri bundan daha önce oluşturulmuş havuzlarına gönderilen her yukarıdaki işleri *boşta* gönderme zaman durum.
>
>

## <a name="next-steps"></a>Sonraki adımlar
### <a name="batchlabs-heat-map"></a>BatchLabs ısı Haritası
[BatchLabs][batch_labs], Azure Batch uygulamalarıyla ilgili oluşturma, hata ayıklama ve izleme işlemlerini gerçekleştirmenize yardımcı olan ücretsiz, gelişmiş özelliklere sahip ve tek başına kullanılan bir istemci aracıdır. BatchLabs içeren bir *ısı Haritası* görev yürütme görselleştirme sağlayan özelliktir. Ne zaman yürütme [ParallelTasks] [ parallel_tasks_sample] örnek uygulama, her bir düğümde Paralel Görevler yürütülmesi kolayca görselleştirmek için ısı Haritası özelliğini kullanabilirsiniz.


[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_labs]: https://azure.github.io/BatchLabs/
[cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[enable_autoscaling]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[fill_type]: https://msdn.microsoft.com/library/microsoft.azure.batch.common.computenodefilltype.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[rest_addpool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[task_schedule]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudpool.taskschedulingpolicy.aspx

