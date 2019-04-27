---
title: Uygun maliyetli düşük öncelikli Vm'lere - Azure Batch iş yüklerini çalıştırma | Microsoft Docs
description: Düşük öncelikli VM'ler, Azure Batch iş yüklerinizi maliyetini azaltmak için hazırlamayı öğrenin.
services: batch
author: mscurrell
manager: jeconnoc
ms.assetid: dc6ba151-1718-468a-b455-2da549225ab2
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 03/19/2018
ms.author: markscu
ms.custom: seodec18
ms.openlocfilehash: 17668470be3e997c215aacc4cc2c32c80de2dd81
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60776137"
---
# <a name="use-low-priority-vms-with-batch"></a>Batch ile düşük öncelikli VM’ler kullanma

Düşük öncelikli sanal makineler (VM'ler) Batch iş yükü maliyetini azaltmak için Azure Batch sunar. Düşük öncelikli VM'ler, toplu iş yükleri, büyük bir miktarını etkinleştirerek mümkün işlem gücü çok düşük bir maliyetle için kullanılacak yeni türleri olmasını sağlayın.
 
Düşük öncelikli VM'ler Azure'da kapasiteden yararlanın. Düşük öncelikli VM'ler havuzlarınızdaki belirttiğinizde, Azure Batch, kullanılabilir olduğunda bu fazlalık kullanabilirsiniz.
 
Düşük öncelikli VM'ler kullanma zorunluluğunu getirir, bu sanal makineler ayrılacak kullanılamıyor olabilir veya kullanılabilir kapasiteye bağlı olarak herhangi bir zamanda etkisiz hale getirilebilir ' dir. Bu nedenle, düşük öncelikli VM'ler belirli türdeki iş yükleri için en uygun değildir. Düşük öncelikli VM'ler, batch ve burada işin tamamlanma süresi esnektir ve iş birçok sanal makinelerde dağıtılan zaman uyumsuz işleme iş yükleri için kullanın.
 
Düşük öncelikli VM'ler, özel Vm'leriyle kıyasla önemli ölçüde indirimli fiyatla sunulmaktadır. Fiyatlandırma ayrıntıları için bkz: [Batch fiyatlandırması](https://azure.microsoft.com/pricing/details/batch/).

## <a name="use-cases-for-low-priority-vms"></a>Düşük öncelikli VM'ler için kullanım örnekleri

Düşük öncelikli VM'ler, hangi iş yüklerini kullanabilir ve bunları kullanamazsınız özelliklerini verilen? Genel olarak, işleri paralel görevlere ayrılır veya ölçeği ve birçok sanal dağıtılmış birçok iş uygun, toplu işlem gerçekleştirme iş yüklerinde sunuldu.

-   Azure'daki fazla kapasite kullanımını en üst düzeye çıkarmak için uygun işleri genişletebilir.

-   Bazen VM'lerin kullanılamıyor olabilir veya işlerdeki, hangi işlerin daha az kapasite sonuçlanır ve görev kesinti ve tekrar bölümlerini neden. Bu nedenle işleri çalıştırmak için yapabilecekleri sürede esnek olmalıdır.

-   Daha uzun görevleri işlerle kesildi durumunda etkilenebilir. Ardından, uzun süre çalışan görevlerini uygulama bunlar yürütürken ilerleme durumunu kaydetmek için denetim noktası oluşturma, kesinti etkisini azaltılır. Kesinti etkisini çok daha az olduğu için kısa yürütme süresi olan görevler ile düşük öncelikli VM'ler, en iyi şekilde çalıştığı eğilimindedir.

-   Etkisiz bir VM tüm işi yeniden çalıştırmak zorunda neden olabileceği için birden çok VM yazılımınız uzun süre çalışan MPI işlerini düşük öncelikli VM'ler kullanmak için uygun değildir.

Düşük öncelikli VM'ler kullanma için uygun toplu işleme kullanım örnekleri iyi bazı örnekleri şunlardır:

-   **Geliştirme ve test**: Özellikle de büyük ölçekli çözümleri geliştirdiğinizde, önemli ölçüde tasarruf gerçekleşmiş. Tüm test türleri yararlı olabilir, ancak büyük ölçekli yük testi ve gerileme sınaması harika kullanır.

-   **İsteğe bağlı kapasite ekleme**: Düşük öncelikli VM'ler, normal özel VM'ler - desteklemek için kullanılabilir kullanılabilir olduğunda, işleri ölçeklendirmek ve düşük maliyetli; bu nedenle daha hızlı tamamlayın kullanılabilir olduğunda, ayrılmış sanal taban kullanılabilir durumda kalır.

-   **Esnek iş yürütme süresi**: Esneklik ise sürede işlerin tamamlamanız gereken ve kapasite olası bırakmaları izin; Bununla birlikte düşük öncelikli VM'ler'ın eklenmesiyle işleri sık daha hızlı ve düşük maliyetli çalıştırın.

Düşük öncelikli VM'ler, iş yürütme süresi esneklik bağlı olarak birkaç yolu kullanmak için Batch havuzları yapılandırılabilir:

-   Düşük öncelikli VM'ler, yalnızca bir havuzda kullanılabilir. Bu durumda, Batch, herhangi bir preempted kapasite kullanılabilir olduğunda kurtarır. Yalnızca düşük öncelikli VM'ler kullanıldığından bu yapılandırma, işleri yürütmek için ucuz yoludur.

-   Düşük öncelikli VM'ler, ayrılmış sanal sabit bir taban çizgisi birlikte kullanılabilir. Sabit ayrılmış sanal makine sayısı, her zaman bir işi ilerlediğini tutmak için bazı kapasite olmasını sağlar.

-   Daha ucuz düşük öncelikli VM'ler yalnızca uygun olduğunda kullanılır, böylece adanmış ve düşük öncelikli VM'ler, dinamik karışımını olabilir, ancak tam fiyatlı adanmış VM'ler gerektiğinde ölçeklenir. Bu yapılandırma, en düşük miktarda bir kapasite ilerlediğini işleri tutmak kullanılabilir kalmasını sağlar.

## <a name="batch-support-for-low-priority-vms"></a>Düşük öncelikli VM'ler için Batch desteği

Azure Batch, kullanma ve düşük öncelikli sanal makinelerden yararlanabilir kolaylaştıran çeşitli özellikleri sağlar:

-   Batch havuzları, hem özel VM'ler hem de düşük öncelikli VM'ler içerebilir. Bir havuz oluşturulduğunda veya açık yeniden boyutlandırma işlemi veya otomatik ölçeklendirme kullanarak mevcut bir havuz için herhangi bir zamanda değiştirildi, her sanal makine türü sayısı belirtilebilir. İş ve görev gönderimi havuzdaki VM türleri bakılmaksızın değişmeden kalır. Ayrıca, tamamen düşük öncelikli VM'ler kapasite çalışan işleri tutmak için en düşük eşiğin altına düşmesi durumunda işleri olası, ancak özel VM'ler çalıştırma olarak hesaplı bir şekilde çalıştırmak için kullanılacak bir havuz yapılandırabilirsiniz.

-   Batch havuzları, düşük öncelikli VM'ler hedef sayısı otomatik olarak aranır. Vm'leri etkisiz, Batch kaybedilen kapasitenin yerini ve hedef döndürmek çalışır.

-   Görevler kesintiye, Batch algılar ve otomatik olarak yeniden çalıştırılacak görevleri requeues.

-   Düşük öncelikli VM'ler, ayrılmış sanal makineler için olandan farklı bir ayrı bir vCPU kotası vardır. 
    Düşük öncelikli VM'ler, daha az maliyet olduğundan kota düşük öncelikli VM'ler için özel VM'ler için kota daha yüksektir. Daha fazla bilgi için [Batch Hizmeti kotaları ve sınırları](batch-quota-limit.md#resource-quotas).    

> [!NOTE]
> Düşük öncelikli VM'ler o anda desteklenmez oluşturulan Batch hesapları için [kullanıcı aboneliği modunda](batch-api-basics.md#account).
>

## <a name="create-and-update-pools"></a>Oluşturma ve havuzları güncelleştirme

Bir Batch havuzu (işlem düğümleri olarak da bilinir) hem adanmış hem de düşük öncelikli VM'ler içerebilir. İşlem düğümlerinin hedef sayısıyla hem adanmış hem de düşük öncelikli VM'ler için ayarlayabilirsiniz. Hedef düğüm sayısı, havuzda istediğiniz sanal makinelerin sayısını belirtir.

Örneğin, bir havuzu oluşturmak için 5 hedefi ile Vm'leri Azure bulut hizmeti kullanarak Vm'leri ve 20 düşük öncelikli VM'ler adanmış:

```csharp
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: "cspool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard_D2_v2",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5") // WS 2016
);
```

Bir havuz oluşturmak için 5 hedef Azure sanal makinelerinde (Bu örnekte Linux Vm'leri) kullanarak Vm'leri ve 20 düşük öncelikli VM'ler adanmış:

```csharp
ImageReference imageRef = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "16.04-LTS",
    version: "latest");

// Create the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration("batch.node.ubuntu 16.04", imageRef);

pool = batchClient.PoolOperations.CreatePool(
    poolId: "vmpool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard_D2_v2",
    virtualMachineConfiguration: virtualMachineConfiguration);
```

Hem özel hem de düşük öncelikli VM'ler için geçerli düğüm sayısını alabilirsiniz:

```csharp
int? numDedicated = pool1.CurrentDedicatedComputeNodes;
int? numLowPri = pool1.CurrentLowPriorityComputeNodes;
```

Havuz düğümleri düğüm adanmış veya düşük öncelikli VM olup olmadığını gösteren bir özelliğe sahiptir:

```csharp
bool? isNodeDedicated = poolNode.IsDedicated;
```

Bir havuzdaki bir veya daha fazla düğümleri etkisiz, havuz üzerindeki bir liste düğümleri işlemi hala düğümleri döndürür. Geçerli düşük öncelikli düğümlerin sayısını değişmeden kalır, ancak bu düğümleri kümesine durumlarına sahip **geçersiz kılındı** durumu. Değiştirme Vm'leri bulmak batch çalışır ve başarılı olursa, düğümler üzerinden geçerek **oluşturma** ardından **başlangıç** durumları görev yürütme için kullanılabilir hale gelmeden önce olduğu gibi yeni düğümler.

## <a name="scale-a-pool-containing-low-priority-vms"></a>Düşük öncelikli VM'ler içeren bir havuzu ölçeklendirme

Olarak yalnızca adanmış Vm'lerden oluşan havuzlarıyla Resize yöntemi çağırarak veya otomatik ölçeklendirme kullanarak düşük öncelikli VM'ler içeren bir havuzu ölçeklendirme mümkündür.

Havuz yeniden boyutlandırma işlemi değerini güncelleştiren bir ikinci isteğe bağlı parametresi alan **targetLowPriorityNodes**:

```csharp
pool.Resize(targetDedicatedComputeNodes: 0, targetLowPriorityComputeNodes: 25);
```

Düşük öncelikli VM'ler bir havuzu otomatik ölçeklendirme formülü aşağıdaki şekilde destekler:

-   Get veya hizmet tarafından tanımlanan değişkenin değerini ayarlamak **$TargetLowPriorityNodes**.

-   Hizmet tarafından tanımlanan değişkenin değerini alabilirsiniz **$CurrentLowPriorityNodes**.

-   Hizmet tarafından tanımlanan değişkenin değerini alabilirsiniz **$PreemptedNodeCount**. 
    Bu değişken preempted durumda düğüm sayısını döndürür ve ölçeği artırın veya azaltın, kullanılamayan etkisiz düğüm sayısına bağlı olarak ayrılmış düğüm sayısını sağlar.

## <a name="jobs-and-tasks"></a>İşleri ve görevleri

İşleri ve görevleri için düşük öncelikli düğümler küçük bir ek yapılandırma gerektirir; yalnızca destek aşağıdaki gibidir:

-   Bir işin JobManagerTask özelliği yeni bir özellik olan **AllowLowPriorityNode**. 
    Bu özelliği true olduğunda, iş yöneticisi görevi ya da bir adanmış veya düşük öncelikli düğüm üzerinde zamanlanabilir. Bu özellik false ise, iş yöneticisi görevi yalnızca bir adanmış düğüm için zamanlandı.

-   Bir [ortam değişkeni](batch-compute-node-environment-variables.md) düşük öncelikli ya da ayrılmış bir düğüm üzerinde çalışıp çalışmadığını belirleyebilirsiniz, böylece bir görev uygulamaları için kullanılabilir. AZ_BATCH_NODE_IS_DEDICATED ortam değişkenidir.

## <a name="handling-preemption"></a>Önalım işleme

Vm'leri zaman zaman etkisiz hale getirilebilir; önalım gerçekleştiğinde, Batch şunları yapar:

-   Preempted VM'ler için güncelleştirilmiş durumlarına sahip **geçersiz kılındı**.
-   Görevler bir düğüm etkisiz Vm'lerde çalışıyordu, ardından bu görevleri yeniden kuyruğa ve yeniden çalıştırın.
-   VM etkili bir şekilde, sanal makinede yerel olarak depolanan herhangi bir veri kaybı için önde gelen silindi.
-   Havuzu kullanılabilir olan düşük öncelikli düğümlerin hedef sayısını ulaşmak sürekli olarak çalışır. Yedek kapasite bulunduğunda, düğümlerin kimlikleri korumakla birlikte, üzerinden başlatılır **oluşturma** ve **başlangıç** görev zamanlama için kullanılabilir olmadan önce belirtir.
-   Önalım sayıları, Azure portalında bir ölçü olarak kullanılabilir.

## <a name="metrics"></a>Ölçümler

Yeni ölçümler kullanılabilir [Azure portalında](https://portal.azure.com) düşük öncelikli düğümler için. Bu ölçümler şunlardır:

- Düşük öncelikli düğüm sayısı
- Düşük öncelikli çekirdek sayısı 
- Etkisiz düğüm sayısı

Azure portalında ölçümleri görüntülemek için:

1. Portalda Batch hesabınıza gidin ve Batch hesabınız için ayarları görüntüleyin.
2. Seçin **ölçümleri** gelen **izleme** bölümü.
3. İstediğiniz ölçümleri seçin **kullanılabilir ölçümler** listesi.

![Düşük öncelikli düğümler için ölçümleri](media/batch-low-pri-vms/low-pri-metrics.png)

## <a name="next-steps"></a>Sonraki adımlar

* Batch kullanmaya hazırlanan herkes için gerekli bilgileri içeren [Geliştiriciler için Batch özelliğine genel bakış](batch-api-basics.md) konusunu okuyun. Bu makalede havuzlar, düğümler, işler ve görevler gibi Batch hizmet kaynakları ve Batch uygulamanızı oluştururken kullanabileceğiniz birçok API özelliği hakkında daha ayrıntılı bilgi verilmektedir.
* Batch çözümleri oluşturmak için kullanılabilen [Batch API’leri ve araçları](batch-apis-tools.md) hakkında bilgi alın.
