---
title: Ölçümleri kullanarak Azure mikro hizmet yük yönetme | Microsoft Docs
description: Yapılandırma ve hizmet kaynak tüketimi yönetmek için Service Fabric ölçümleri kullanma hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: 0d622ea6-a7c7-4bef-886b-06e6b85a97fb
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 7457a820d9179248eab976ceec64f6b7a4a38563
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34643347"
---
# <a name="managing-resource-consumption-and-load-in-service-fabric-with-metrics"></a>Kaynak tüketimini ve Service Fabric yük ölçümlerle yönetme
*Ölçümleri* , hizmetleri çok değer verdiğiniz ve hangi kümedeki düğümlerin tarafından sağlanan kaynaklardır. Bir ölçü artırmak veya hizmetlerinizi performansını izlemek için yönetmek istediğiniz herhangi bir şey sayısıdır. Örneğin, hizmetiniz aşırı yüklü olmadığını bilmek bellek tüketimi izlemek. Başka bir olup hizmet bellek az daha iyi performans elde için kısıtlı olduğu başka bir yerde dışarı Taşı bulmak için kullanılır.

Bellek, Disk ve CPU kullanımı gibi şeyleri ölçümler gösterilebilir. Bu ölçümler fiziksel, yönetilmesi gereken düğüm üzerindeki fiziksel kaynaklara karşılık kaynakları ölçümleridir. Ölçümleri de olabilir (ve genellikle olan) mantıksal ölçümleri. Mantıksal ölçümleri "MyWorkQueueDepth" veya "MessagesToProcess" veya "TotalRecords" gibi noktalardır. Mantıksal ölçümleri uygulama tanımlı ve dolaylı olarak bazı fiziksel kaynak tüketimini karşılık gelir. Bir hizmeti başına temelinde sabit fiziksel kaynakları ölçü ve rapor tüketimi için olabileceğinden mantıksal ölçümleri yaygındır. Ölçme ve fiziksel ölçümlerinizi raporlama karmaşıklığını ayrıca neden Service Fabric bazı varsayılan ölçümleri sağlar olur.

## <a name="default-metrics"></a>Varsayılan ölçümleri
Yazma ve hizmet dağıtma başlamak istiyorsanız varsayalım. Bu noktada içereceği tükettiği hangi fiziksel veya mantıksal kaynakları bilinmiyor. Uygundur! Diğer bir ölçümleri belirtildiğinde Service Fabric kümesi Kaynak Yöneticisi bazı varsayılan ölçümleri kullanır. Bunlar:

  - PrimaryCount - düğümde birincil çoğaltmaların sayısı 
  - ReplicaCount - düğümü üzerindeki toplam durum bilgisi olan çoğaltmaların sayısı
  - Sayısı - tüm hizmet nesnelerde (durum bilgisiz ve durum bilgisi olan) düğüm sayısı

| Ölçüm | Durum bilgisiz örneği yükleme | Durum bilgisi olan ikincil yükleme | Durum bilgisi olan birincil yükleme | Ağırlık |
| --- | --- | --- | --- | --- |
| PrimaryCount |0 |0 |1 |0 |
| ReplicaCount |0 |1 |1 |0 |
| Sayı |1 |1 |1 |0 |


Temel iş yükleri için çalışma kümesindeki makul bir dağıtımını varsayılan ölçümleri sağlar. Aşağıdaki örnekte, iki hizmet oluşturmak ve Dengeleme için varsayılan ölçümleri Bel ne olur görelim. Üç bölümleri olan bir durum bilgisi olan hizmet ilk hizmetidir ve bir hedef çoğaltma üç boyutunu ayarlayın. İkinci hizmeti, bir bölüm ve bir örnek sayısını üç ile durumsuz bir hizmettir.

İşte, alırsınız:

<center>
![Varsayılan ölçümlerle küme düzeni][Image1]
</center>

Dikkat edilecek bazı noktalar:
  - Durum bilgisi olan hizmet için birincil çoğaltmalara birkaç düğümleri arasında dağıtılır
  - Farklı düğümlerde aynı bölüme çoğaltmaları olan
  - Ana ve ikincil toplam sayısı kümedeki dağıtılır.
  - Hizmet nesneleri toplam sayısı her düğümde eşit olarak ayrılır

İyi!

Varsayılan ölçümleri harika bir başlangıç çalışır. Ancak, varsayılan ölçümleri yalnızca, o ana kadarki yürütecek. Örneğin: sonuçları mükemmel bile kullanımı tüm bölümleri tarafından çekilen bölümlendirme, Düzen olduğunu olasılığını nedir? Ne zaman içinde belirli bir hizmeti yük sabit şansınızdır ve hatta tıpkı arasında birden çok bölüm şimdi?

Yalnızca varsayılan Ölçümleriyle çalıştırabilir. Ancak, genellikle böylece küme kullanımınızı alt ve istediğiniz daha fazla düzensiz anlamına gelir. Varsayılan ölçümleri Uyarlamalı değil ve her şeyi eşdeğer olduğunu varsayın olmasıdır. Örneğin, meşgul bir birincil ve iki olmayan bir "1" PrimaryCount ölçüm için katkıda. Kötü durumda, yalnızca varsayılan ölçümleri kullanarak da performans sorunları kaynaklanan overscheduled düğümleri neden olabilir. En iyi kümenizi alma ve performans sorunlarını önleme düşünüyorsanız, özel Ölçümler ve dinamik yük raporlama kullanmanız gerekir.

## <a name="custom-metrics"></a>Özel ölçümler
Hizmet oluşturulurken ölçümleri adlı-service-örnek başına temelinde yapılandırılır.

Herhangi bir ölçümü açıkladığı bazı özelliklere sahiptir: bir ad, bir ağırlık ve varsayılan yükleme.

* Ölçüm adı: Ölçüm adı. Ölçüm adı Resource Manager'ın açısından kümedeki ölçüm için benzersiz bir tanımlayıcı değil.
* Ağırlık: Bu hizmet için diğer ölçümleri göre ne kadar önemli Bu ölçüm olduğunu ölçüm ağırlığı tanımlar.
* Varsayılan yükleme: Varsayılan Yük hizmet durum bilgisiz veya durum bilgisi olan bağlı olarak farklı şekilde gösterilir.
  * Durum bilgisi olmayan hizmetler için her ölçümü DefaultLoad adlı tek bir özelliğe sahip.
  * Durum bilgisi olan hizmetler için tanımladığınız:
    * PrimaryDefaultLoad: Bu ölçüm, varsayılan miktarını birincil olduğunda bu hizmeti kullanır
    * SecondaryDefaultLoad: Bu ölçüm, varsayılan miktarını ikincil olduğunda bu hizmeti kullanır

> [!NOTE]
> Özel ölçümlerini tanımlayın ve yapmak istiyorsanız _de_ varsayılan ölçümleri kullanın, yapmanız _açıkça_ varsayılan ölçümleri geri ve ağırlıklarını ve değerleri tanımlamak için ekleyin. Bu durum, varsayılan ölçümleri ve özel ölçümlerinizi arasındaki ilişkiyi tanımlamanız gerekir çünkü. Örneğin, belki de birincil dağıtım'den fazla ConnectionCount veya WorkQueueDepth ilgilendiğiniz. Varsayılan olarak PrimaryCount ölçüm ağırlığı yüksek olduğundan öncelik kazanır emin olmak için diğer ölçümlerinizi eklediğinizde, Orta azaltmak istiyor.
>

### <a name="defining-metrics-for-your-service---an-example"></a>Ölçümleri hizmetinizin - örneği tanımlama
Aşağıdaki yapılandırma istediğinizi varsayalım:

  - "ConnectionCount" adlı bir ölçüm hizmetinizi raporları
  - Ayrıca varsayılan ölçümleri kullanmak istediğiniz 
  - Bazı ölçüler yaptıktan ve normal olarak bu hizmet birincil çoğaltmasını "ConnectionCount" 20 birimler aldığını biliyorsanız
  - İkincil kopya "ConnectionCount" 5 birimleri kullanın
  - "ConnectionCount" Bu belirli bir hizmet performansını yönetme bakımından en önemli ölçüm olduğunu biliyor
  - Hala dengeli birincil çoğaltmalara istiyor. Birincil çoğaltmalara Dengeleme genellikle iyi bir fikir ne olursa olsun içindir. Bu, bunun yanı sıra birincil çoğaltmalara çoğunu etkileyen bazı düğüm veya hata etki alanı kaybı önlemeye yardımcı olur. 
  - Aksi takdirde, varsayılan ölçümleri ince

Bu ölçüm yapılandırmaya sahip bir hizmet oluşturmak için yazma kod aşağıdaki gibidir:

Kod:

```csharp
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
StatefulServiceLoadMetricDescription connectionMetric = new StatefulServiceLoadMetricDescription();
connectionMetric.Name = "ConnectionCount";
connectionMetric.PrimaryDefaultLoad = 20;
connectionMetric.SecondaryDefaultLoad = 5;
connectionMetric.Weight = ServiceLoadMetricWeight.High;

StatefulServiceLoadMetricDescription primaryCountMetric = new StatefulServiceLoadMetricDescription();
primaryCountMetric.Name = "PrimaryCount";
primaryCountMetric.PrimaryDefaultLoad = 1;
primaryCountMetric.SecondaryDefaultLoad = 0;
primaryCountMetric.Weight = ServiceLoadMetricWeight.Medium;

StatefulServiceLoadMetricDescription replicaCountMetric = new StatefulServiceLoadMetricDescription();
replicaCountMetric.Name = "ReplicaCount";
replicaCountMetric.PrimaryDefaultLoad = 1;
replicaCountMetric.SecondaryDefaultLoad = 1;
replicaCountMetric.Weight = ServiceLoadMetricWeight.Low;

StatefulServiceLoadMetricDescription totalCountMetric = new StatefulServiceLoadMetricDescription();
totalCountMetric.Name = "Count";
totalCountMetric.PrimaryDefaultLoad = 1;
totalCountMetric.SecondaryDefaultLoad = 1;
totalCountMetric.Weight = ServiceLoadMetricWeight.Low;

serviceDescription.Metrics.Add(connectionMetric);
serviceDescription.Metrics.Add(primaryCountMetric);
serviceDescription.Metrics.Add(replicaCountMetric);
serviceDescription.Metrics.Add(totalCountMetric);

await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("ConnectionCount,High,20,5”,"PrimaryCount,Medium,1,0”,"ReplicaCount,Low,1,1”,"Count,Low,1,1”)
```

> [!NOTE]
> Yukarıdaki örneklerde ve bu belgenin kalan yönetme ölçümleri başına adlı-hizmet temelinde açıklanmaktadır. Ölçümleri konuşması hizmetleriniz için tanımlamak mümkündür _türü_ düzeyi. Bu hizmet bildirimlerinizi belirterek gerçekleştirilir. Türü düzeyindeki ölçümlerini tanımlayan çeşitli nedenlerle önerilmez. Ölçüm adları sık ortama özgü ilk nedenidir. Yerinde kesin bir sözleşme sürece, bir ortamda ölçüm "Çekirdek" "MiliCores" veya "Çekirdek" bazılarında olup olmadığından emin olamaz. Ölçümlerinizi, bildiriminde tanımlanmışsa, yeni bildirimleri ortamı başına oluşturmanız gerekir. Bu genellikle sağlamakta yönetim zorluklar küçük farklılıklar dışında farklı bildirimleri çoğalmasıdır neden olmaktadır.  
>
> Ölçüm yükleri adlı-service-örnek başına temelinde yaygın olarak atanır. Örneğin, yalnızca hafifçe kullanmayı planlıyor CustomerA Hizmeti'nin bir örneğini oluşturmak varsayalım. Ayrıca daha büyük bir iş yükü olan CustomerB için oluşturduğunuz başka varsayalım. Bu durumda, büyük olasılıkla bu hizmetlerin varsayılan yükleri ince ayar istersiniz. Ölçümleri varsa ve bu senaryoyu desteklemek bildirimler ve tanımlanan yükleri istediğiniz her müşteri için farklı bir uygulama ve hizmet türleri gerektirir. Hizmet oluşturma sırasında tanımlanan değerleri, belirli varsayılan değerleri ayarlamak için kullanabilir şekilde bildiriminde tanımlanan geçersiz kılar. Ancak, hizmeti ile gerçekte çalışır eşleştiğinden değil için bildirimlerinde bildirilen değerler, bunu neden olur. Bu karışıklığa neden olabilir. 
>

Bir anımsatıcı olarak: yalnızca varsayılan ölçümleri kullanmak istiyorsanız, ölçümleri toplama hiç dokunma veya hizmetinizi oluşturulurken özel bir şey yapmanız gerekmez. Hiçbir başkalarının tanımlandığında varsayılan ölçümleri otomatik olarak kullanılan. 

Artık daha ayrıntılı bu ayarların her biri aracılığıyla edelim ve onu etkilediğini davranışı hakkında konuşun.

## <a name="load"></a>Yükleme
Ölçümleri tanımlamanın tüm noktası bazı yük göstermektir. *Yük* belirli bir metrik ne kadarının bazı hizmet örneği veya belirtilen bir düğüm üzerindeki çoğaltma tarafından kullanılan olduğu. Yük neredeyse herhangi bir noktada yapılandırılabilir. Örneğin:

  - Bir hizmet oluşturduğunuzda yük tanımlanabilir. Bu adlı _varsayılan yük_.
  - Hizmet oluşturulduktan sonra bir hizmet için varsayılan yükleri de dahil olmak üzere ölçüm bilgileri güncelleştirildi. Bu adlı _bir hizmeti güncelleştirme_. 
  - Belli bir bölüm yükleri bu hizmet için varsayılan değerlerine sıfırlayabilirsiniz. Bu adlı _bölüm yükünü sıfırlama_.
  - Yükleme bildirilen üzerinde bir çalışma zamanı sırasında dinamik olarak hizmet nesnesi temelinde. Bu adlı _yük raporlama_. 
  
Bu stratejileri tümünün aynı hizmet içinde yaşam süresi boyunca kullanılabilir. 

## <a name="default-load"></a>Varsayılan yükleme
*Varsayılan yükleme* bu hizmetin her bir hizmet nesnesi (durum bilgisiz örneği veya durum bilgisi olan çoğaltma) tüketen ölçüsü ne kadarının. Dinamik yük raporu gibi diğer bilgileri alıncaya kadar Küme Kaynak Yöneticisi hizmeti nesnesi yük için bu sayıyı kullanır. Daha basit hizmetler için varsayılan yükleme statik bir tanımıdır. Varsayılan yükleme hiçbir zaman güncelleştirilir ve hizmet yaşam süresi için kullanılır. Varsayılan basit kapasite planlama burada belirli miktarda kaynağı farklı iş yükleri için ayrılmış ve değiştirme senaryoları works harika yükler.

> [!NOTE]
> Kapasite yönetimi ve kümenizdeki düğümlerin kapasiteleri tanımlama hakkında daha fazla bilgi için lütfen bkz [bu makalede](service-fabric-cluster-resource-manager-cluster-description.md#capacity).
> 

Küme Kaynak Yöneticisi'ni kendi ana ve ikincil kopya için farklı varsayılan yük belirtmek durum bilgisi olan hizmetler sağlar. Durum bilgisi olmayan hizmetler yalnızca tüm örnekleri için geçerli bir değer belirtebilirsiniz. Her rol işlerinde farklı türde çoğaltmaları yapmak bu yana durum bilgisi olan hizmetler, varsayılan yükleme birincil ve ikincil çoğaltmaları genellikle farklıdır. Örneğin, ana genellikle okuma ve yazma işlemleri hizmet ve ikincil desteklerken, hesaplama iş yükünü çoğunu işleyecek. Genellikle bir birincil çoğaltma için varsayılan yük ikincil çoğaltmalar için varsayılan yük daha yüksektir. Gerçek sayılar, kendi ölçü bağımlı olmalıdır.

## <a name="dynamic-load"></a>Dinamik yük
Hizmetiniz bir süredir çalışıyor varsayalım. Bazı izleme ile, fark ettim:

1. Bazı bölümleri veya belirli bir hizmeti örnekleri diğerlerinden daha fazla kaynak tüketmesine
2. Bazı hizmetleri zaman içinde değişir yük sahiptir.

Birçok yük dalgalanmaları bu tür neden olabilecek bir şey yoktur. Örneğin, farklı hizmetler veya bölümleri farklı gereksinimlere sahip farklı müşteriler ile ilişkilendirilir. Hizmetten çalışma miktarını gün boyunca değiştiğinden yükü de değişebilir. Bir nedenle bağımsız olarak, genellikle varsayılan kullanabilirsiniz ve tek bir sayı yoktur. Küme dışında çoğu kullanımı almak istiyorsanız bu özellikle doğrudur. Varsayılan yükleme için çekme herhangi bir değer sürenin bir kısmı yanlış. Yanlış varsayılan sonuç kümesi Kaynak Yöneticisi'nde üzerinde veya altında kaynaklarını ayırma yükler. Sonuç olarak, küme dengelenmiş Küme Kaynak Yöneticisi'ni düşündüğü olsa da, üzerinde veya altında kullanılan düğümlerin vardır. Varsayılan yükleri ilk yerleştirme için bazı bilgiler sağlar, ancak gerçek iş yükleri için tam bir öykü olmadıklarını beri hala iyidir. Değişen kaynak gereksinimlerini doğru bir şekilde yakalamak için çalışma zamanı sırasında kendi yük güncelleştirmek her hizmet nesne Küme Kaynak Yöneticisi'ni sağlar. Bu, dinamik yük raporlama çağrılır.

Dinamik yük raporları çoğaltmalar veya kendi ayırma ve bildirilen yükü ölçümleri kendi ömürleri boyunca ayarlamak için örnekler izin verir. Bir hizmet çoğaltma veya soğuk ve herhangi bir iş yapmamanın örneğini genellikle belirli bir metrik düşük miktarda tarafından kullanılan raporlar. Meşgul çoğaltma veya örnek daha kullandıkları raporlar.

Çoğaltma veya örnek başına yük raporlama Küme Kaynağı Yöneticisi'nin kümedeki bireysel hizmet nesneleri reorganıze sağlar. Hizmetleri yeniden düzenleme gereksinim duydukları kaynakları alma sağlamaya yardımcı olur. Meşgul Hizmetleri "diğer çoğaltmaları veya şu anda soğuk veya daha az çalışarak olan örnekleri kaynaklarınızı geri kazanmanız" etkili bir şekilde alın.

Güvenilir hizmetler içinde yükü raporlamak üzere kod dinamik olarak şöyle:

Kod:

```csharp
this.Partition.ReportLoad(new List<LoadMetric> { new LoadMetric("CurrentConnectionCount", 1234), new LoadMetric("metric1", 42) });
```

Bir hizmet oluşturma sırasında tanımlı ölçümleri hiçbirinde bildirebilirsiniz. Service Fabric olmayan bir ölçüm için bir hizmet raporları yükü kullanmak üzere yapılandırılmışsa, bu raporu yok sayar. Geçerli olan diğer ölçümleri aynı anda bildirilen varsa, bu raporları kabul edilir. Servis kodu ölçmek ve bunu bilen tüm ölçümleri rapor etme, ve işleçler servis kodunu değiştirmek zorunda kalmadan kullanmak üzere ölçüm yapılandırma belirtebilirsiniz. 

### <a name="updating-a-services-metric-configuration"></a>Bir hizmetin ölçüm yapılandırmasını güncelleştirme
Ölçüm listesine hizmetle ilişkilendirilmiş ve hizmeti Canlı çalışırken bu ölçümleri özelliklerini dinamik olarak güncelleştirilebilir. Bu deneme ve esneklik sağlar. Bazı örnekler bu yararlı olduğunda şunlardır:

  - belirli bir hizmet için buggy rapora bir ölçüm devre dışı bırakma
  - İstenen davranışı temelinde ölçümleri ağırlıkları yeniden yapılandırma
  - yalnızca kodu zaten dağıtıldığını ve diğer mekanizmalar doğrulanmış sonra yeni bir ölçümü etkinleştirme
  - Gözlemlenen davranışı ve tüketim dayalı bir hizmet için varsayılan yük değiştirme

Ölçüm yapılandırmasını değiştirmek için ana API'leri `FabricClient.ServiceManagementClient.UpdateServiceAsync` C# ve `Update-ServiceFabricService` PowerShell'de. Bu API'leri ile belirttiğiniz ne olursa olsun bilgileri hemen hizmet varolan Ölçüm bilgilerini değiştirir. 

## <a name="mixing-default-load-values-and-dynamic-load-reports"></a>Varsayılan yükleme değerlerini ve dinamik yük raporları karıştırma
Varsayılan yükleme ve dinamik yükleri aynı hizmet için kullanılabilir. Varsayılan yükleme ve dinamik yük raporları bir hizmet kullanan, dinamik raporlar görünmesini kadar varsayılan yük tahmini olarak görür. Küme Kaynak Yöneticisi'ni çalışmak için bir şeyler verdiğinden varsayılan yük iyi olur. Varsayılan yükleme oluşturuldukları sırada hizmet nesneleri iyi konumlara yerleştirmek için Küme Kaynak Yöneticisi'ni sağlar. Varsayılan yükleme bilgi sağlanırsa, hizmetleri yerleşimini etkili bir şekilde rastgele belirlenir. Yük raporları daha sonra geldiğinde ilk rastgele yerleştirme genellikle bir sorun var ve Hizmetleri taşımak Küme Kaynak Yöneticisi'ni sahiptir.

Şimdi önceki örneğimizde alın ve bazı özel ölçümleri ve dinamik yük raporlama eklediğimiz ne olur bakın. Bu örnekte, bir örnek ölçümü "MemoryInMb" kullanırız.

> [!NOTE]
> Bellek için Service Fabric sistem ölçümleri biridir [kaynak yöneten](service-fabric-resource-governance.md), ve kendiniz raporlama genellikle zordur. Aslında, bellek tüketimi bildirmenizi bekliyoruz yoktur; Kullanılan bellek burada yardımcı olarak Küme Kaynağı Yöneticisi'nin özellikleri hakkında bilgi.
>

Şimdi başlangıçta durum bilgisi olan hizmet aşağıdaki komutla oluşturduğumuz olduğunu varsayın:

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("MemoryInMb,High,21,11”,"PrimaryCount,Medium,1,0”,"ReplicaCount,Low,1,1”,"Count,Low,1,1”)
```

Bir anımsatıcı bu söz dizimi ("MetricName MetricWeight, PrimaryDefaultLoad, SecondaryDefaultLoad") değil.

Hangi bir olası küme düzeni gibi görünebilir görelim:

<center>
![Küme dengeli hem varsayılan hem de özel ölçümlerle][Image2]
</center>

Belirtmeye değer olan bazı noktalar:

* Bölüm içindeki ikincil çoğaltmaları her kendi yük olabilir
* Genel ölçümleri dengeli arayın. Bellek için maksimum ve minimum yük arasında 1.75 orandır (N3, düğümdür en yük ile en az N2 ve 28/16 olan 1.75 =).

Biz açıklamak için gereken bazı şeyler vardır:

* Ne 1.75 oranını makul olup olmadığını belirledi? Küme Kaynak Yöneticisi'ni nasıl yeterince iyi olması durumunda veya yapmak için daha fazla iş ise biliyor?
* Ne zaman Dengeleme oluşuyor?
* Ne bellek "Yüksek" ağırlıklı olduğunu anlama geliyor?

## <a name="metric-weights"></a>Ölçüm ağırlığı
Farklı hizmetler arasında aynı ölçümleri izleme önemlidir. Bu genel küme tüketimini izlemek, düğümlere tüketim dengelemek ve düğüm kapasitesi geçmez sağlamak için Küme Kaynak Yöneticisi'ni nelere izin görünümüdür. Bununla birlikte, hizmetleri aynı Ölçüm önemini konusunda farklı görünümleri olabilir. Ayrıca, birçok ölçümleri ve çok sayıda Hizmetleri ile bir kümede mükemmel dengeli çözümleri için tüm ölçümleri var olmayabilir. Küme Kaynak Yöneticisi'ni bu gibi durumlarda nasıl yöneteceğini?

Ölçüm ağırlığı Küme Kaynağı Yöneticisi'nin kusursuz yanıt olduğunda küme dengelemeye yönelik karar verin. Ölçüm ağırlığı belirli hizmetleri farklı şekilde dengelemek için Küme Kaynak Yöneticisi'ni de olanak sağlar. Ölçümleri dört farklı ağırlık düzeyine sahip olabilir: sıfır, düşük, Orta ve yüksek. Ölçüm ağırlık sıfır hiçbir şey veya dengeli değerlendirirken katkıda bulunur. Ancak, kendi yük hala kapasite yönetimi katkıda. Ölçümleri sıfır ağırlık ile hala faydalıdır ve sık hizmet davranışı ve performans izleme işleminin bir parçası olarak kullanılır. [Bu makalede](service-fabric-diagnostics-event-generation-infra.md) izleme ölçümleri kullanımını ve Tanılama, hizmetlerin üzerinde daha fazla bilgi sağlar. 

Gerçek kümedeki farklı ölçüm ağırlığı Küme Kaynak Yöneticisi'ni farklı çözümler oluşturur etkisidir. Ölçüm ağırlığı Küme Kaynak Yöneticisi'ni, belirli ölçümleri diğerlerinden daha önemli söyleyin. Mükemmel bir çözüm olduğunda Küme Kaynak Yöneticisi'ni daha iyi yüksek ağırlıklı ölçümleri dengeleyen çözümleri tercih. Bir hizmeti belirli bir ölçüm düşündüğü varsa bu ölçümün kullanımlarını imbalanced bulabilirsiniz önemli değildir. Bu, hatta bir dağıtım için önemli olan bazı ölçüsünün almak başka bir hizmet sağlar.

Bazı yük raporları ve nasıl farklı ölçümü bir örneğe bakalım kümedeki farklı ayırmaları sonuçlarında ağırlık verir. Bu örnekte, göreli ağırlık ölçümleri geçiş farklı düzenlemeler hizmetleri oluşturmak için Küme Kaynak Yöneticisi'ni neden bakın.

<center>
![Ölçüm ağırlık örnek ve Dengeleme çözümlerini üzerindeki etkisini][Image3]
</center>

Bu örnekte, dört farklı hizmetler, MetricA ve MetricB olmak üzere iki farklı ölçümleri için tüm raporlama farklı değerler vardır. Bir durumda, tüm hizmetler MetricA tanımlamak en önemli sunucudur (ağırlık yüksek =) ve önemli olarak MetricB (ağırlık düşük =). Sonuç olarak, böylece MetricA MetricB daha iyi dengelenmiş Küme Kaynak Yöneticisi'ni Hizmetleri yerleştirir bakın. "Daha iyi dengelenmiş" anlamına gelir MetricA düşük olduğunu MetricB daha düşük bir standart sapma vardır. İkinci durumda, ölçüm ağırlığı ters. Sonuç olarak, Küme Kaynak Yöneticisi'ni A ve B MetricB daha iyi MetricA dengeli olduğu bir ayırma ile gündeme için Hizmetleri değiştirir.

> [!NOTE]
> Ölçüm ağırlığı belirlemek nasıl Küme Kaynak Yöneticisi'ni dengelemeniz, ancak ne zaman Dengeleme olmayacak. Dengeleme hakkında daha fazla bilgi için kullanıma [bu makalede](service-fabric-cluster-resource-manager-balancing.md)
>

### <a name="global-metric-weights"></a>Genel ölçüm ağırlığı
Diyelim ki ServiceA yüksek ağırlık olarak MetricA tanımlar ve düşük veya sıfır ağırlığı ServiceB MetricA için ayarlar. Kullanılan alma sona eriyor gerçek ağırlık nedir?

Her ölçüm için izlenen birden çok ağırlıkları vardır. İlk ağırlık hizmet oluşturulduğunda ölçümü tanımlanandır. Başka ağırlığını otomatik olarak hesaplanır genel ağırlık ' dir. Bu iki ağırlıkları çözümleri Puanlama Küme Kaynak Yöneticisi'ni kullanır. Her iki ağırlıkları dikkate alarak önemlidir. Bu, her hizmetin kendi öncelikleri göre dengelemek ve ayrıca bir bütün olarak doğru olarak ayrılmış sağlamak için Küme Kaynak Yöneticisi'ni sağlar.

Küme Kaynak Yöneticisi'ni hem genel hem de yerel Bakiye hakkında önemli kaydetmedi neler olacağını? İyi genel dengeli ancak, tek tek Hizmetleri için yetersiz kaynak dengesindeki neden çözümleri oluşturmak kolaydır. Aşağıdaki örnekte, şimdi yalnızca varsayılan ölçümleri ile yapılandırılmış bir hizmet bakın ve yalnızca genel Bakiye kabul ne olur bakın:

<center>
![Genel bir yalnızca çözüm etkisi][Image4]
</center>

Üst örnekte, yalnızca genel bakiyesi dayalı bir bütün olarak küme gerçekten dengelenir. Tüm düğümlerin ana ve aynı numara toplam çoğaltmaları sayısının aynı vardır. Bu ayırma gerçek etkisini bakarsanız ancak, bu nedenle iyi değildir: tüm kendi ana aldığından herhangi bir düğümün kaybını belirli bir iş yükünü orantısız, etkiler. Üç ana daire hizmetinin üç farklı bölümler için ilk düğüm başarısız olursa Örneğin, tümü kaybolur. Buna karşılık, bir çoğaltma kaybetmek bölümlerinin üçgen ve Altıgene hizmetler sahiptir. Bu aşağı çoğaltma kurtarmak zorunda dışında bir kesintiye neden olur.

Alt örnekte, Küme Kaynak Yöneticisi'ni her iki genel ve hizmet başına bakiyesi tabanlı çoğaltmalar dağıtılmış. Çözüm puanı hesaplanırken genel çözüm ve tek tek Hizmetleri (yapılandırılabilir) bölümüne ağırlık çoğunu sağlar. Ölçüm için genel bakiyesi her hizmetinden ölçüm ağırlığı ortalaması alınarak hesaplanır. Her hizmetin kendi tanımlı ölçüm ağırlığı göre dengelenir. Bu hizmetleri kendilerini içinde kendi gereksinimlerine göre dengeli sağlar. Sonuç olarak, hata aynı ilk düğüm başarısız olursa tüm hizmetlerin tüm bölümler dağıtılır. Her aynı etkisidir.

## <a name="next-steps"></a>Sonraki adımlar
- Hizmetleri yapılandırma hakkında daha fazla bilgi için [hizmetleri yapılandırma hakkında bilgi edinin](service-fabric-cluster-resource-manager-configure-services.md)(service-fabric-cluster-resource-manager-configure-services.md)
- Birleştirme ölçümleri tanımlama yayılmak yerine düğümlerde yük birleştirmek için bir yoludur. Birleştirme yapılandırma konusunda bilgi edinmek için bkz [bu makalede](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
- Küme Kaynak Yöneticisi'ni yönetir ve yük devretme kümesinde dengeleyen hakkında bilgi almak için makalesine kontrol [Yük Dengeleme](service-fabric-cluster-resource-manager-balancing.md)
- En baştan başlatın ve [bir giriş için Service Fabric kümesi Resource Manager Al](service-fabric-cluster-resource-manager-introduction.md)
- Taşıma maliyeti için Küme Kaynak Yöneticisi'ni, belirli hizmetleri diğerlerinden taşımak daha pahalı sinyal bir yoludur. Taşıma maliyeti hakkında daha fazla bilgi için bkz [bu makalede](service-fabric-cluster-resource-manager-movement-cost.md)

[Image1]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-cluster-layout-with-default-metrics.png
[Image2]:./media/service-fabric-cluster-resource-manager-metrics/Service-Fabric-Resource-Manager-Dynamic-Load-Reports.png
[Image3]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-metric-weights-impact.png
[Image4]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-global-vs-local-balancing.png
