---
title: Düşük maliyetli düşük öncelikli sanal makinelerin Azure Batch iş yüklerini çalıştırmak | Microsoft Docs
description: Düşük öncelikli sanal makineleri Azure Batch iş yükü maliyetini azaltmak için hazırlamayı öğrenin.
services: batch
author: mscurrell
manager: timlt
ms.assetid: dc6ba151-1718-468a-b455-2da549225ab2
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 03/19/2018
ms.author: markscu
ms.openlocfilehash: 68240e29429b4c6321e8627b62ad65ce7ecb468e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="use-low-priority-vms-with-batch"></a>Düşük öncelikli sanal makineleri Batch ile kullanma

Düşük öncelikli sanal makineleri (VM'ler) Batch iş yükü maliyetini azaltmak için Azure Batch sunar. Düşük öncelikli sanal makineleri yeni toplu iş yükleri, büyük bir miktarını sağlayarak olası çok düşük bir maliyetle için kullanılacak güç işlem türlerini olun.
 
Düşük öncelikli sanal makineleri Azure'da fazlalık kapasite yararlanın. Düşük öncelikli sanal makineleri, havuzlarınızı belirttiğinizde, Azure Batch kullanılabilir olduğunda bu fazlalık kullanabilirsiniz.
 
Düşük öncelikli sanal makineleri kullanma kolaylığını bu VM'lerin ayrılacak kullanılabilir durumda olmayabilir veya kullanılabilir kapasite bağlı olarak herhangi bir zamanda etkisiz değil. Bu nedenle, düşük öncelikli sanal makineleri belirli türde bir iş yükleri için en uygun. Düşük öncelikli sanal makineleri toplu ve burada iş tamamlanma zamanı esnek ve iş üzerinde birçok VM dağıtılmış zaman uyumsuz işleme iş yükleri için kullanın.
 
Düşük öncelikli sanal makineleri özel VM'ler ile kıyasla önemli ölçüde azaltılmış fiyatla sunulur. Fiyatlandırma ayrıntıları için bkz: [Batch fiyatlandırması](https://azure.microsoft.com/pricing/details/batch/).

## <a name="use-cases-for-low-priority-vms"></a>Düşük öncelikli VM'ler için kullanım örnekleri

Düşük öncelikli sanal makineleri, hangi iş yüklerini kullanabilir ve bunları kullanamazsınız özelliklerini verilen? Genel olarak, işleri birçok paralel görevlere bozuk veya çıkışı ölçeği ve üzerinde birçok VM dağıtılan birçok iş iyi bir tercihtir toplu işleme iş yüklerinin olduğunu.

-   Azure fazlalık kapasite kullanımını en üst düzeye çıkarmak için kullanıma uygun işleri ölçeklendirebilirsiniz.

-   Bazen VM'ler kullanılamayabilir veya etkisiz, hangi işleri için sınırlı kapasite sonuçlanır ve görev kesinti ve tekrar bölümlerini neden. Bu nedenle işlerini çalıştırmak için uygulayabileceğiniz zamanında esnek olmalıdır.

-   İşlerini uzun görevlerle kesintiye varsa daha etkilenmiş olabilir. Uzun süre çalışan görevleri bunlar yürütmek olarak ilerleme durumunu kaydetmek için denetim noktası oluşturma uygulamak, kesinti etkisini azalır. Kesinti etkisini çok daha az olduğu için en düşük öncelik VM'ler ile çalışmak için kısa yürütme süreleri görevlerle eğilimlidir.

-   Bir etkisiz VM yeniden çalıştırmak zorunda tüm iş yol açabileceğinden birden çok VM kullanmak uzun süre çalışan MPI işlerini düşük öncelikli sanal makineleri kullanmak için uygun değildir.

Düşük öncelikli sanal makineleri kullanmak için uygun toplu işleme kullanım örnekleri iyi bazı örnekler şunlardır:

-   **Geliştirme ve test**: özel olarak, büyük ölçekli çözümleri geliştirdiğinizde, önemli tasarrufları alırlar. Sınama tüm türleri yararlanabilirsiniz, ancak büyük ölçekli yük test etme ve gerileme sınaması harika kullanır.

-   **İsteğe bağlı kapasite ekleme**: düşük öncelikli sanal makineleri, normal özel VM'ler - tamamlamak için kullanılabilir kullanılabilir olduğunda, işleri ölçeklendirmek ve bu nedenle daha hızlı tamamlamak için daha düşük maliyetli; mevcut değil, özel VM'ler temel kullanılabilir durumda kalır .

-   **Esnek iş yürütme süresi**: varsa zaman işler esnekliğe sahip tamamlamak daha sonra kapasite olası bırakmaları izin; ancak, düşük öncelikli sanal makineleri eklenmesiyle işleri sık daha hızlı ve daha düşük maliyetli bir çalıştırın.

Batch havuzları, iş yürütme süresi esneklik bağlı olarak birkaç şekilde düşük öncelikli sanal makineleri kullanmak üzere yapılandırılabilir:

-   Düşük öncelikli sanal makineleri yalnızca bir havuzda kullanılabilir. Bu durumda, Batch tüm preempted kapasite kullanılabilir olduğunda kurtarır. Düşük öncelikli yalnızca VM'ler kullanılmak üzere bu yapılandırmayı işleri yürütmek için ucuz yoludur.

-   Düşük öncelikli sanal makineleri, sabit bir taban çizgisi özel VM'ler ile birlikte kullanılabilir. Ayrılmış sanal sabit sayıda her zaman bir iş İleri aşamalara tutmak için bazı kapasite olmasını sağlar.

-   Böylece cheaper düşük öncelikli VM'ler kullanılabilir olduğunda yalnızca kullanılan ayrılmış ve düşük öncelikli sanal makineleri, dinamik karışımını olabilir, ancak tam fiyatlı özel VM'ler gerektiğinde ölçeklenir. Bu yapılandırma İleri aşamalara işleri tutmak kullanılabilir kapasite en düşük düzeyde tutar.

## <a name="batch-support-for-low-priority-vms"></a>Düşük öncelikli VM'ler için toplu desteği

Azure toplu işlem kullanmasına ve düşük öncelikli Vm'lerden yararlanmak kolaylaştıran çeşitli özellikleri sağlar:

-   Batch havuzları özel VM'ler ve düşük öncelikli sanal makineleri içerebilir. Bir havuzu oluşturulduğunda veya açık yeniden boyutlandırma işlemi veya Otomatik ölçek kullanarak mevcut bir havuz için herhangi bir zamanda değiştirildi VM türlerinin sayısı belirtilebilir. İş ve görev gönderimi havuzu VM türler bağımsız olarak değişmeden kalabilir. Tamamen düşük öncelikli sanal makineleri kapasite çalışan işleri korumak için en düşük eşiğin altına düşerse işleri mümkün, ancak özel VM'ler yukarı döndürme olarak ailenin çalıştırmak için kullanılacak bir havuzu de yapılandırabilirsiniz.

-   Batch havuzlarının otomatik olarak düşük öncelikli VM'ler hedef sayısını arama. Sanal makineleri etkisiz sonra toplu kayıp kapasite değiştirin ve hedef dönmek çalışır.

-   Görevler kesintiye toplu algılar ve otomatik olarak yeniden çalıştırılacak görevleri requeues.

-   Düşük öncelikli sanal makineleri özel VM'ler için farklı bir ayrı vCPU kota sahip. 
    Düşük öncelikli sanal makineleri daha az maliyet olduğundan kota düşük öncelikli sanal makineleri için özel VM'ler için kota daha yüksektir. Daha fazla bilgi için bkz: [Batch Hizmeti kotaları ve sınırlarına](batch-quota-limit.md#resource-quotas).    

> [!NOTE]
> Düşük öncelikli sanal makineleri şu anda desteklenmemektedir oluşturulan Batch hesaplarıyla ilgili [kullanıcı abonelik modu](batch-api-basics.md#account).
>

## <a name="create-and-update-pools"></a>Oluşturma ve havuzları güncelleştirme

Batch havuzundaki (işlem düğümleri olarak da bilinir) hem adanmış hem de düşük öncelikli sanal makineleri içerebilir. Hem özel hem de düşük öncelikli VM'ler için işlem düğümleri sayısını ayarlayabilirsiniz. Düğümlerin hedef sayısını havuzunda olmasını istediğiniz VM'lerin sayısını belirtir.

Örneğin, bir havuzu oluşturmak için Azure bulut hizmeti VM'ler 5 hedefle kullanarak sanal makineleri ve 20 düşük öncelikli sanal makineleri ayrılmış:

```csharp
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: "cspool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard_D2_v2",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4") // WS 2012 R2
);
```

Bir havuzu oluşturmak için 5'in bir hedef Azure sanal makineleri (Bu durumda Linux VM'ler) kullanarak sanal makineleri ve 20 düşük öncelikli sanal makineleri ayrılmış:

```csharp
ImageReference imageRef = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "16.04.0-LTS",
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

Geçerli düğüm sayısını hem adanmış hem de düşük öncelikli VM'ler için alabilirsiniz:

```csharp
int? numDedicated = pool1.CurrentDedicatedComputeNodes;
int? numLowPri = pool1.CurrentLowPriorityComputeNodes;
```

Havuz düğümleri düğüm adanmış veya düşük öncelikli bir VM olup olmadığını gösteren bir özelliği vardır:

```csharp
bool? isNodeDedicated = poolNode.IsDedicated;
```

Bir veya daha fazla düğüm havuzunda etkisiz, havuzu üzerinde bir liste düğümleri işlemi hala düğümleri döndürür. Düşük öncelikli düğüm geçerli sayısını değişmeden kalır, ancak bu düğümler kümesine durumlarına sahip **geçersiz kılındı** durumu. Toplu deneme değiştirme VM'ler bulmaya ve başarılı olursa, düğümler üzerinden geçerek **oluşturma** ve ardından **başlangıç** durumları görev yürütme için kullanılabilir hale gelmeden önce olduğu gibi yeni düğümler.

## <a name="scale-a-pool-containing-low-priority-vms"></a>Düşük öncelikli sanal makineleri içeren bir havuzu ölçeklendirme

Olarak yalnızca özel VM'ler oluşan havuzlarıyla Resize yöntemini çağırarak veya otomatik ölçeklendirme kullanarak düşük öncelikli sanal makineleri içeren bir havuzu ölçeklendirmek mümkündür.

Havuzu yeniden boyutlandırma işlemi değerini güncelleştirmeleri ikinci bir isteğe bağlı parametresi alan **targetLowPriorityNodes**:

```csharp
pool.Resize(targetDedicatedComputeNodes: 0, targetLowPriorityComputeNodes: 25);
```

Havuz otomatik ölçeklendirme formülü aşağıdaki gibi düşük öncelikli sanal makineleri destekler:

-   Almak veya hizmet tanımlı değişkenin değerini ayarlamak **$TargetLowPriorityNodes**.

-   Hizmet tanımlı değişkenin değerini alabilir **$CurrentLowPriorityNodes**.

-   Hizmet tanımlı değişkenin değerini alabilir **$PreemptedNodeCount**. 
    Bu değişken preempted durumda düğümlerin sayısını döndürür ve yukarı veya aşağı kullanılamaz etkisiz düğüm sayısına bağlı olarak ayrılmış düğüm sayısını ölçeklendirmenizi sağlar.

## <a name="jobs-and-tasks"></a>İşler ve görevler

İşlerini ve görevleri düşük öncelikli düğümleri için çok az ek yapılandırma gerektirmez; yalnızca desteği aşağıdaki gibidir:

-   Yeni bir özellik JobManagerTask özelliğinin bir işin **AllowLowPriorityNode**. 
    Bu özelliği true olduğunda, iş yöneticisi görevi ya da bir adanmış veya düşük öncelikli düğümünde zamanlanabilir. Bu özellik false ise, iş yöneticisi görevi yalnızca adanmış bir düğüme zamanlanır.

-   Bir [ortam değişkeni](batch-compute-node-environment-variables.md) düşük öncelikli veya ayrılmış bir düğümde çalışan olup olmadığını belirleyebilmek görev uygulamaları için kullanılabilir. AZ_BATCH_NODE_IS_DEDICATED ortam değişkenidir.

## <a name="handling-preemption"></a>Önalım işleme

Sanal makineleri bazen etkisiz; önalım olduğunda toplu şunları yapar:

-   Preempted VM'ler için güncelleştirilmiş durumlarına sahip **geçersiz kılındı**.
-   Görevler etkisiz düğümü Vm'lerde çalışıyordu, ardından bu görevleri yeniden kuyruğa ve yeniden çalıştırın.
-   VM etkili bir şekilde silindi VM üzerinde yerel olarak depolanan veri kaybına neden.
-   Havuz sürekli olarak kullanılabilir düşük öncelikli düğümlerin hedef sayısını ulaşmaya çalışır. Yedek kapasite bulunduğunda, düğümlerin kimlikleri tutmak ancak, oluşturulmak yeniden başlatılır **oluşturma** ve **başlangıç** görev zamanlama için kullanılabilir önce belirtir.
-   Önalım sayıları, Azure portalında bir ölçü olarak kullanılabilir.

## <a name="metrics"></a>Ölçümler

Yeni ölçümleri kullanılabilir [Azure portal](https://portal.azure.com) düşük öncelikli düğümleri için. Bu ölçümler şunlardır:

- Düşük öncelikli düğüm sayısı
- Düşük öncelikli çekirdek sayısı 
- Etkisiz düğüm sayısı

Azure Portal'da ölçümleri görüntülemek için:

1. Portalda Batch hesabınıza gidin ve toplu işlem hesabı için ayarları görüntüleyin.
2. Seçin **ölçümleri** gelen **izleme** bölümü.
3. İşlemleriniz ölçümleri seçin **kullanılabilir ölçümler** listesi.

![Düşük öncelikli düğümleri için ölçümleri](media/batch-low-pri-vms/low-pri-metrics.png)

## <a name="next-steps"></a>Sonraki adımlar

* Batch kullanmaya hazırlanan herkes için gerekli bilgileri içeren [Geliştiriciler için Batch özelliğine genel bakış](batch-api-basics.md) konusunu okuyun. Bu makalede havuzlar, düğümler, işler ve görevler gibi Batch hizmet kaynakları ve Batch uygulamanızı oluştururken kullanabileceğiniz birçok API özelliği hakkında daha ayrıntılı bilgi verilmektedir.
* Batch çözümleri oluşturmak için kullanılabilen [Batch API’leri ve araçları](batch-apis-tools.md) hakkında bilgi alın.
