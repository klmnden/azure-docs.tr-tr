---
title: Ölçümleri kullanarak Azure Service Fabric uygulama yük yönetme | Microsoft Docs
description: Yapılandırma ve hizmet kaynak tüketimi yönetmek için Service Fabric'te ölçümleri kullanma hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: ''
ms.assetid: 0d622ea6-a7c7-4bef-886b-06e6b85a97fb
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 1a61de6b0b6f73e112dd69108272ded3a67497e8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60516756"
---
# <a name="managing-resource-consumption-and-load-in-service-fabric-with-metrics"></a>Kaynak tüketimine ve Service Fabric yük ölçümlerle yönetme
*Ölçümleri* , hizmetleri çok değer verdiğiniz ve, kümedeki düğümler tarafından sağlanan kaynaklar. Bir ölçüm iyileştirmek veya hizmetlerinizin performansını izlemek için yönetmek istediğiniz herhangi bir şey: Örneğin, hizmetiniz aşırı yüklü olmadığını bilmek bellek tüketimi izlemek. Başka bir olup hizmet bellek az daha iyi performans elde etmek için sınırlı olduğu başka bir yere taşıyabilirsiniz şekil için kullanılır.

Bellek, Disk ve CPU kullanımı gibi şeyleri ölçümleri bir örnektir. Bu fiziksel ölçüm, fiziksel kaynakların yönetilmesi gereken düğüme karşılık gelen kaynakları ölçümleridir. Ölçümleri de olabilir (ve yaygın olarak olan) mantıksal ölçümleri. "MyWorkQueueDepth" veya "MessagesToProcess" veya "TotalRecords" gibi şeyleri mantıksal ölçümleridir. Mantıksal ölçümleri, uygulama tanımlı ve dolaylı olarak bazı fiziksel kaynak tüketimi karşılık gelir. Hizmet başına temelinde sabit ölçü ve rapor fiziksel kaynak tüketimi olabileceğinden mantıksal ölçümleri yaygındır. Service Fabric, bazı varsayılan ölçümleri neden sağlar ölçme ve raporlama fiziksel ölçümlerinizi karmaşıklığı de olur.

## <a name="default-metrics"></a>Varsayılan ölçümleri
Yazmaya ve hizmetinizi dağıtmaya başlamak istediğinizi düşünelim. Bu noktada hangi fiziksel veya mantıksal kaynaklarını kullandığı bilmiyorum. Bir sakınca yoktur! Başka bir ölçüm belirtildiğinde, Service Fabric Küme Kaynak Yöneticisi bazı varsayılan ölçümleri kullanır. Bunlar:

  - PrimaryCount - düğüm birincil çoğaltma sayısı 
  - ReplicaCount - düğüm üzerindeki toplam durum bilgisi olan çoğaltmalarının sayısı
  - Count - tüm hizmet nesneleri (durum bilgisiz ve durum bilgisi olan) düğüm sayısı

| Ölçüm | Durum bilgisi olmayan bir örneğini yükleme | Durum bilgisi olan ikincil yük | Durum bilgisi olan birincil yük | Ağırlık |
| --- | --- | --- | --- | --- |
| PrimaryCount |0 |0 |1 |Yüksek |
| ReplicaCount |0 |1\. |1 |Orta |
| Sayı |1 |1\. |1 |Düşük |


Temel iş yükleri için makul bir küme içindeki iş dağıtımını varsayılan ölçümleri sağlar. Aşağıdaki örnekte, iki hizmet oluşturmak ve Dengeleme için varsayılan ölçümleri dayanan ne olacağını görelim. Bir hedef çoğaltma kümesi boyutu üç ve üç bölümleri olan bir durum bilgisi olan hizmet ilk hizmetidir. İkinci bir bölüm ve üç örnek sayısı ile durum bilgisi olmayan hizmet hizmetidir.

Elde edecekleriniz aşağıda verilmiştir:

<center>

![Varsayılan ölçümlerle küme düzeni][Image1]
</center>

Dikkat edilecek bazı noktalar:
  - Durum bilgisi olan hizmet için birincil çoğaltmalara birden fazla düğümde dağıtılan
  - Farklı düğümlere aynı bölüm çoğaltmaları olan
  - Kümede dağıtılan seçimlerine ve ikincil veritabanı toplam sayısı
  - Hizmet nesneleri toplam sayısı her düğümde eşit oranda ayrılır

İyi!

Varsayılan ölçümleri harika bir başlangıç çalışır. Bununla birlikte, varsayılan ölçümleri yalnızca, şimdiye sahip olacaktır. Örneğin: Bölümleme, Düzen olduğunu olasılığını nedir sonuçları tüm bölümleri tarafından mükemmel bile kullanımı çekilen? Ne zaman içinde belirli bir hizmetin yük sabit almayı ve hatta tıpkı birden çok bölümde şu anda?

Yalnızca varsayılan Ölçümleriyle çalıştırabilir. Ancak, genellikle Bunun yapılması, küme kullanımı alt ve istediğiniz birden fazla eşit olduğunu gösterir. Varsayılan ölçümleri uyumlu olmayan ve her şeyi eşdeğer olduğunu varsayın olmasıdır. Örneğin, meşgul bir birincil ve iki yer almayan bir PrimaryCount Ölçümde "1" katkıda bulunur. En kötü durumda, yalnızca varsayılan ölçümleri kullanarak da overscheduled düğümleri kaynaklanan performans sorunlarına neden olabilir. En iyi kümenizi alma ve performans sorunlarını önleme ilgileniyorsanız, özel Ölçümler ve dinamik yük raporlama kullanmanız gerekir.

## <a name="custom-metrics"></a>Özel ölçümler
Hizmet oluştururken ölçümleri adlı-service-örnek başına temelinde yapılandırılır.

Herhangi bir ölçümü açıkladığı bazı özellikler vardır: bir ad ve Ağırlık varsayılan yük.

* Ölçüm adı: Ölçüm adı. Ölçüm adı, Resource Manager'ın açısından kümesindeki ölçüm için benzersiz bir tanımlayıcıdır.
* Ağırlık: Ölçüm ağırlık, bu hizmet için diğer ölçümlere göre ne kadar önemli Bu ölçüm olduğunu tanımlar.
* Varsayılan yükleme: Varsayılan yük, hizmet durum bilgisi olan veya bağlı olarak farklı şekilde temsil edilir.
  * Durum bilgisi olmayan hizmetler için her bir ölçüm DefaultLoad adlı tek bir özelliğe sahip.
  * Durum bilgisi olan hizmetler için tanımladığınız:
    * PrimaryDefaultLoad: Birincil olduğunda bu ölçüm varsayılan süre bu hizmeti kullanır.
    * SecondaryDefaultLoad: İkincil olduğunda bu ölçüm varsayılan süre bu hizmeti kullanır.

> [!NOTE]
> Özel ölçümler tanımlayın ve istediğiniz _ayrıca_ varsayılan ölçümleri kullanmak için yapmanız _açıkça_ varsayılan ölçümleri yedekleme ve ağırlıkları ve değerleri tanımlamak için ekleyin. Bu durum, varsayılan Ölçümler ve özel ölçümleriniz arasındaki ilişkiyi tanımlamanız gerekir çünkü. Örneğin, belki de daha fazla birincil dağıtım ConnectionCount veya WorkQueueDepth verdiğiniz. Varsayılan olarak PrimaryCount ölçüm ağırlığı yüksek olduğundan, öncelikli emin olmak için diğer ölçümlerinizi eklediğinizde, Orta azaltmak istediğiniz.
>

### <a name="defining-metrics-for-your-service---an-example"></a>Hizmetinizin - örnek ölçümler tanımlama
Aşağıdaki yapılandırmayı istediğiniz varsayalım:

  - Hizmetinizi "ConnectionCount" adlı bir ölçüm raporları
  - Ayrıca varsayılan ölçümleri kullanmak istiyorsunuz 
  - Bazı ölçümler yaptıktan ve bu normalde bu hizmeti birincil çoğaltması "ConnectionCount", 20 birimler aldığını biliyor
  - İkincil veritabanı 5 biriminin "ConnectionCount" kullanın.
  - "ConnectionCount" Bu belirli hizmet performansını yönetme açısından en önemli ölçüm olduğunu bildiğiniz
  - Yine de birincil çoğaltmalara dengeli istiyor. Birincil çoğaltmalara Dengeleme, genellikle iyi bir fikir ne olursa olsun olur. Bu, bunun yanı sıra birincil çoğaltmalara çoğunu etkilemesini bazı düğüm ya da hata etki alanı kaybı önlemeye yardımcı olur. 
  - Aksi takdirde, varsayılan ölçümleri bir sakınca yoktur

Bu ölçüm bir yapılandırma ile bir hizmet oluşturmak için yazdığınız kod aşağıdaki gibidir:

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
> Yukarıdaki örneklerde ve bu belgenin geri kalan yönetme ölçümleri tek başına adlı hizmet temelinde açıklanmaktadır. Hizmetlerinizi hizmetine ölçümlerini tanımlamak da mümkündür _türü_ düzeyi. Bu hizmet bildirimlerinizi belirterek gerçekleştirilir. Tür düzeyindeki ölçümleri tanımlama, çeşitli nedenlerle önerilmez. İlk neden ölçüm adları sık ortama özgü değildir. Yerinde kesin bir sözleşme olmadığı sürece, bir ortamda "Çekirdek" ölçüm "MiliCores" veya "Çekirdek" bazılarında olmadığından emin olamaz. Her ortam için yeni bildirimler oluşturmak için ihtiyacınız bildiriminizdeki ölçümlerinizi tanımlanmışsa. Bu genellikle bir yaygınlaşmasının için yönetim zorlukları açabilir, küçük farklılıklar dışında farklı bildirimler için yol açar.  
>
> Ölçüm yükleri adlı-service-örnek başına temelinde yaygın olarak atanır. Örneğin, yalnızca hafifçe kullanmak için plan CustomerA için Hizmeti'nin bir örneğini oluşturduğunuz varsayalım. Ayrıca daha büyük bir iş yükü olan CustomerB için oluşturduğunuz başka varsayalım. Bu durumda, muhtemelen varsayılan yükleri için bu hizmetlere yönelik ince istiyorsunuz. Ölçümlerin vardır ve bu senaryoyu desteklemek bildirimler ve size tanımlanan yükleri istiyorsanız her müşteri için farklı uygulama ve hizmet türleri gerektirir. Hizmet oluşturma sırasında tanımlanan değerleri, belirli varsayılan değerleri ayarlamak için kullanabilirsiniz, böylece bildiriminde tanımlanan ayarları geçersiz kılar. Ancak, hizmet ile gerçekte çalıştırılan eşleşen değil için bildirimleri bildirilen değerler, bunu neden olur. Bu, karışıklığa neden olabilir. 
>

Bir anımsatıcı olarak: yalnızca varsayılan ölçümleri kullanmak istiyorsanız, ölçümleri koleksiyonu hiç dokunma veya özel bir şey hizmetinizi oluştururken yapmanız gerekmez. Tanımlandığında, hiçbir diğer varsayılan ölçümlere otomatik olarak kullanılan. 

Şimdi daha ayrıntılı bir şekilde bu ayarların her biri aracılığıyla dönelim ve onu etkileyen davranışı hakkında konuşun.

## <a name="load"></a>Yükleme
Bazı yük temsil etmek için ölçümleri tanımlamanın tüm noktayı belirler. *Yük* olan belirli bir metrik ne kadarının bazı hizmet örneği veya belirli bir düğüm üzerinde çoğaltma tarafından kullanılır. Yük neredeyse herhangi bir noktada yapılandırılabilir. Örneğin:

  - Bir hizmet oluşturduğunuzda yük tanımlanabilir. Bu adlandırılır _varsayılan yük_.
  - Hizmet oluşturulduktan sonra bir hizmet için varsayılan yükleri de dahil olmak üzere ölçüm bilgileri güncelleştirildi. Bu adlandırılır _bir hizmeti güncelleştirme_. 
  - Belirli bir bölüme yükleri aydaki hizmet için varsayılan değerlerine sıfırlayabilirsiniz. Bu adlandırılır _bölüm yük sıfırlama_.
  - Yükleme rapor üzerinde bir hizmet nesnesi olarak çalışma zamanı sırasında dinamik olarak. Bu adlandırılır _yük raporlama_. 
  
Tüm bu stratejiler yaşam süresi boyunca aynı hizmetinde kullanılabilir. 

## <a name="default-load"></a>Varsayılan yükleme
*Varsayılan yük* bu hizmetin her bir hizmet nesnesi (durum bilgisi olmayan örnek ya da durum bilgisi olan çoğaltma) tüketen ölçüsü ne kadarının. Dinamik yük rapor gibi diğer bilgileri alıncaya kadar Küme Kaynak Yöneticisi hizmet nesnesi yükü için bu sayıyı kullanır. Daha basit bir hizmetse, varsayılan yükleme statik bir tanımıdır. Varsayılan yük, hiçbir zaman güncelleştirilir ve hizmet ömrü boyunca kullanılır. Varsayılan basit kapasite planlama burada belirli bir miktarda kaynak farklı iş yükleri için ayrılmış ve değiştirme senaryoları için harika çalışır yükler.

> [!NOTE]
> Kapasite yönetimi ve kümedeki düğümlerin kapasiteler tanımlama hakkında daha fazla bilgi için lütfen bkz [bu makalede](service-fabric-cluster-resource-manager-cluster-description.md#capacity).
> 

Küme Kaynak Yöneticisi, ana ve ikincil veritabanı için farklı varsayılan yük belirtmek durum bilgisi olan hizmetler sağlar. Durum bilgisi olmayan hizmetler, yalnızca tüm örnekleri için geçerli bir değer belirtebilirsiniz. Farklı türde iş her rolden yineleme yapmak bu yana durum bilgisi olan hizmetler için varsayılan yükleme için birincil ve ikincil çoğaltmalar genellikle farklıdır. Örneğin, ana genellikle hem okuma hem de yazma işlemleri hizmet ve desteklerken ikincil veritabanı büyük işlem yükünü işleyecek. Genellikle bir birincil çoğaltma için varsayılan yük ikincil çoğaltmalar için varsayılan yük göre olandan yüksektir. Gerçek sayıları kendi Ölçülerinizi bağlı olmalıdır.

## <a name="dynamic-load"></a>Dinamik yük
Bir süredir hizmete çalıştırıyorsunuz varsayalım. Bazı izleme, ettik:

1. Bazı bölümleri veya belirli bir hizmet örneği diğerlerinden daha fazla kaynak tüketmesine
2. Bazı hizmetler, zaman içinde değişen yükü var.

Çok sayıda bu tür yük dalgalanmaları neden olabilecek bir şey yoktur. Örneğin, farklı hizmetlerde veya bölümler farklı müşteriler farklı gereksinimlere sahip ilişkilendirilmiş. İş hizmetten gün boyunca değiştiğinden yük de değiştirebilir. Nedeni ne olursa olsun, genellikle varsayılan için kullanabileceğiniz tek bir sayı yoktur. Küme dışında çoğu kullanımı almak istiyorsanız bu özellikle doğrudur. Varsayılan yükleme için çekme herhangi bir değer sürenin bir kısmı yanlış olur. Yanlış bir varsayılan sonuç kümesi Kaynak Yöneticisi'nde üzerinde veya altında kaynaklarını ayırma yükler. Sonuç olarak, küme dengeli Küme Kaynak Yöneticisi gördüğü olsa bile, üzerinde veya altında kullanılan düğümler var. Varsayılan ilk yerleştirme için bazı bilgiler sağlarlar, ancak bunlar gerçek iş yükleri için eksiksiz bir hikaye değilseniz beri iyi hala yükleniyor. Doğru bir şekilde değişen kaynak gereksinimlerine yakalamak için Küme Kaynak Yöneticisi, kendi yük çalışma zamanı güncelleştirmek her hizmet nesnesi sağlar. Bu, dinamik yük raporlama adı verilir.

Dinamik yük raporları çoğaltmalar veya örnekleri kendi ömrü boyunca kendi ayırma ve bildirilen yükü ölçümlerinin ayarlamaya izin verir. Genellikle hizmet çoğaltması veya soğuk ve herhangi bir iş yapmadan örneği belirli bir metrik düşük miktarda kullanıyordu rapor. Bir meşgul çoğaltma veya örnek, daha fazla kullansalar rapor.

Çoğaltma veya örnek başına yük raporlama, kümedeki bireysel hizmet nesneleri yeniden düzenlemek için Küme Kaynak Yöneticisi sağlar. Hizmetleri yeniden düzenleme gereksinim duydukları kaynakları elde etmenize yardımcı olur. "Diğer çoğaltmalar veya şu anda daha az iş yaparken veya soğuk olan örnekleri kaynaklarınızı geri kazanmanız" meşgul Hizmetleri etkili bir şekilde alın.

Reliable Services içinde kod yükü raporlamak üzere dinamik olarak şuna benzer:

Kod:

```csharp
this.Partition.ReportLoad(new List<LoadMetric> { new LoadMetric("CurrentConnectionCount", 1234), new LoadMetric("metric1", 42) });
```

Bir hizmet oluşturma zamanında tanımlı ölçümleri biriyle bildirebilirsiniz. Service Fabric hizmeti raporları yükü değil bir ölçüm için kullanmak üzere yapılandırılmışsa, bu rapor yok sayar. Geçerli olan, aynı anda bildirilen diğer ölçümleri varsa, bu raporları kabul edilir. Hizmet kodu ölçün ve giden tüm ölçümleri raporu, nasıl ve hizmet kodu değiştirmek zorunda kalmadan kullanılacak ölçüm yapılandırması işleçleri belirtebilirsiniz. 

### <a name="updating-a-services-metric-configuration"></a>Bir hizmetin ölçüm yapılandırması güncelleştiriliyor
Ölçüm listesinde hizmetle ilişkilendirilen ve hizmet canlıyken ölçümleri özelliklerini dinamik olarak güncelleştirilebilir. Bu, deneme ve esneklik sağlar. Bazı örnekler bu ne zaman faydalıdır şunlardır:

  - bir ölçüm belirli bir hizmet için hatalı bir raporla devre dışı bırakma
  - İstenen davranışına göre ölçümleri ağırlıkları yeniden yapılandırma
  - yalnızca kod zaten dağıtıldığını ve başka mekanizmalar doğrulanmış sonra yeni bir ölçüm etkinleştirme
  - Gözlemlenen davranışı ve tüketim tabanlı bir hizmet için varsayılan yük değiştirme

Ölçüm yapılandırmanın değiştirilmesine ilişkin ana API'leri `FabricClient.ServiceManagementClient.UpdateServiceAsync` C# ve `Update-ServiceFabricService` PowerShell'de. Bu API'ler ile belirttiğiniz hangi bilgileri hemen hizmeti için mevcut Ölçüm bilgilerini değiştirir. 

## <a name="mixing-default-load-values-and-dynamic-load-reports"></a>Varsayılan yük değerleri ve dinamik yük raporları karıştırma
Varsayılan yükleme ve dinamik yükleri aynı hizmet için kullanılabilir. Varsayılan yük hem de dinamik yük raporları bir hizmet kullanan, dinamik raporlar görünmesini bekleyin varsayılan yük tahmini olarak görev yapar. Varsayılan yük, Küme Kaynak Yöneticisi ile çalışmak için bir şey sağladığından uygundur. Varsayılan yük oluşturulduğunda hizmet nesneleri iyi konumlara yerleştirmek için Küme Kaynak Yöneticisi sağlar. Varsayılan yük bilgi sağlanırsa, yerleştirme hizmetlerinin etkili bir şekilde rastgele belirlenir. Yük raporlar daha sonra geldiğinde ilk rastgele yerleştirme genellikle yanlıştır ve Hizmetleri taşımak Küme Kaynak Yöneticisi sahiptir.

Şimdi önceki Örneğimizdeki alın ve biz bazı özel Ölçümler ve dinamik yük raporlama eklediğinizde ne olacağına bakalım. Bu örnekte, bir örnek ölçümü "MemoryInMb" kullanırız.

> [!NOTE]
> Bellek için Service Fabric sistem ölçümlerini biridir [kaynak yöneten](service-fabric-resource-governance.md), ve kendiniz raporlama genellikle zordur. Biz aslında bellek tüketimi bildirmenize olanak beklemiyoruz; Kullanılan bellek Küme Kaynak Yöneticisi'nin özellikleri hakkında daha fazla bilgi için yardımcı olarak burada.
>

Şimdi, başlangıçta durum bilgisi olan hizmet aşağıdaki komutla oluşturduğumuz olduğunu varsayın:

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("MemoryInMb,High,21,11”,"PrimaryCount,Medium,1,0”,"ReplicaCount,Low,1,1”,"Count,Low,1,1”)
```

Bir anımsatıcı, bu sözdizimini ("MetricName MetricWeight, PrimaryDefaultLoad, SecondaryDefaultLoad") değil.

Hangi bir olası küme Düzen görünebileceğine bakalım:

<center>

![Hem varsayılan hem de özel ölçümleri ile dengeli küme][Image2]
</center>

Dikkate almaya değer bazı noktalar şunlardır:

* Bölüm içindeki ikincil çoğaltmaları her kendi yük olabilir
* Genel ölçümleri dengeli arayın. Bellek için maksimum ve minimum yük arasındaki oran 1.75 olduğu (düğümüdür N3, en iyi yük ile en az N2 ve 28/16 olan 1.75 =).

Hala açıklamak için gereken bazı şeyler vardır:

* Ne 1.75 oranını makul olup olmadığını belirledi? Küme Kaynak Yöneticisi, bu kadar iyi olursa veya daha fazla iş yapmak için ise nasıl biliyor musunuz?
* Ne zaman Dengeleme gerçekleşir?
* Ne bellek "Yüksek" ağırlıklı olduğunu anlama geliyor?

## <a name="metric-weights"></a>Ölçüm ağırlıkları
Farklı Hizmetleri genelinde aynı ölçümleri izlemek önemlidir. Bu genel görünüm ne kümedeki tüketimi izlemek, tüketim düğümlere dengelemek ve düğümleri kapasite aşımı gerçekleştirilmeyen sağlamak Küme Kaynak Yöneticisi olur. Ancak, hizmetler için aynı Ölçüm önemini farklı görünümleri olabilir. Ayrıca, birçok ölçüm ve çok sayıda Hizmetleri ile bir kümede tüm ölçümler için mükemmel bir şekilde dengeli çözümleri olmayabilir. Küme Kaynak Yöneticisi, bu gibi durumlarda nasıl işleyeceğini?

Ölçüm ağırlıkları mükemmel yanıt olduğunda küme Dengeleme nasıl karar vermek için Küme Kaynak Yöneticisi izin verin. Ölçüm ağırlıkları da belirli hizmetler farklı bir şekilde dengelemek için Küme Kaynak Yöneticisi izin verin. Ölçümler, dört farklı ağırlık düzeyine sahip olabilir: Sıfır, düşük, Orta ve yüksek. Bir ölçüm ağırlık sıfır ile hiçbir şey olmadığını dengeli değerlendirirken katkıda bulunur. Ancak, kendi yük hala kapasite yönetimi katkıda. Sıfır ağırlık Ölçümleriyle hala faydalıdır ve hizmet davranışı ve performans izleme işleminin bir parçası olarak sıklıkla kullanılır. [Bu makalede](service-fabric-diagnostics-event-generation-infra.md) kullanımı izleme ölçümleri ve tanılama hizmetlerinizin daha fazla bilgi sağlar. 

Gerçek farklı ölçüm ağırlıkları kümedeki küme kaynak yöneticisi için farklı çözümler ürettiğini etkisidir. Ölçüm ağırlıkları Küme Kaynak Yöneticisi, belirli ölçümleri diğerlerinden daha önemli olduğunu bildirir. Hiçbir kusursuz bir çözüm olduğunda Küme Kaynak Yöneticisi daha iyi bir üst ağırlıklı ölçümleri dengeleyen çözümleri tercih. Bir hizmeti belirli bir ölçüm gördüğü, bunların kullanılması bu ölçümün imbalanced bulabilirsiniz, önemli değildir. Bu, hatta bir dağıtım için önemli olan bazı ölçümün elde etmek için başka bir hizmet sağlar.

Bazı yük raporları ve farklı ölçüm bir örneğe göz atalım ağırlık verme kümedeki farklı ayırmaları sonuçlanır. Bu örnekte, göreli ağırlık ölçümlerine geçiş hizmetleri farklı düzenlemeleri oluşturmak için Küme Kaynak Yöneticisi neden olduğunu görürüz.

<center>

![Ölçüm ağırlık örnek ve Dengeleme çözümlerinde üzerindeki etkileri][Image3]
</center>

Bu örnekte, dört farklı hizmet, iki farklı ölçüm MetricA ve MetricB için tüm raporlama farklı değerler vardır. Bir durumda MetricA tüm hizmetleri tanımlamakta en önemli olur (ağırlık yüksek =) ve önemsiz olarak MetricB (ağırlık = düşük). Sonuç olarak, böylece MetricA daha iyi MetricB dengeli küme kaynak yöneticisi hizmetleri yerleştirir bakın. "Daha iyi dengelenmiş" anlamına gelir MetricA düşük olduğunu MetricB daha düşük bir standart sapma sahiptir. İkinci durumda, biz ölçüm ağırlıkları tersine çevirir. Sonuç olarak, Küme Kaynak Yöneticisi Hizmetleri A ve B MetricB daha iyi MetricA dengeli olduğu bir ayırma ile görünmesi değiştirir.

> [!NOTE]
> Ölçüm ağırlıkları belirlemek nasıl Küme Kaynak Yöneticisi dengelemeniz, ancak zaman Dengeleme olmayacak. Dengeleme hakkında daha fazla bilgi için kullanıma [bu makalede](service-fabric-cluster-resource-manager-balancing.md)
>

### <a name="global-metric-weights"></a>Genel ölçüm ağırlıkları
Diyelim ki yüksek ağırlık olarak MetricA ServiceA tanımlar ve ServiceB ağırlığı MetricA için düşük veya sıfıra ayarlar. Kullanılan alma sona eriyor gerçek ağırlığı nedir?

Her bir ölçüm için izlenen birden çok ağırlıkları vardır. İlk ağırlık hizmeti oluşturulduğunda ölçümü için tanımlanmış bir bileşendir. Diğer ağırlığı otomatik olarak hesaplanan bir genel ağırlık ' dir. Küme Kaynak Yöneticisi çözümleri Puanlama hem bu ağırlıkları kullanır. Her iki ağırlıkları hesaba katarak çok önemlidir. Bu, her hizmetin kendi önceliklere göre dengelemek ve ayrıca bir bütün olarak doğru şekilde ayrılır emin olmak için Küme Kaynak Yöneticisi sağlar.

Küme Kaynak Yöneticisi hem genel hem de yerel Bakiye hakkında önemli yaramadı durumunda ne olacağını? Genel olarak dengelenir ancak, tek başına hizmetlerinden düşük kaynak bakiyesini neden çözümleri oluşturmak de kolaydır. Aşağıdaki örnekte, şimdi yalnızca varsayılan ölçümleri ile yapılandırılmış bir hizmet bakmak ve yalnızca genel Bakiye edildiği durumlarda ne olacağına bakalım:

<center>

![Genel olarak tek bir çözüm etkisini][Image4]
</center>

Yalnızca genel bakiyeye göre üst örnekte, aslında bir bütün olarak dengelenir. Tüm düğümlerin aynı seçimlerine ve aynı numara toplam çoğaltma sayısı vardır. Bu ayırma gerçek etkisini göz önünde ancak, bu nedenle iyi değildir: tüm alt seçimlerine aldığından herhangi bir düğümün kaybını belirli bir iş yükü orantısız, etkiler. Üç ana daire hizmetinin üç farklı bölümleri için ilk düğüm başarısız olursa, tüm kaybolur. Buna karşılık, bir çoğaltma kaybetmek bölümlerinin üçgen ve Altıgene hizmetlere sahip. Bu, aşağı çoğaltma kurtarmak zorunda dışındaki bir kesintiye neden olur.

Alt örnekte, küme kaynak yöneticisi her iki genel ve hizmet başına bakiyeye tabanlı çoğaltmalar dağıtılmış. Çözümün hesaplarken genel çözüm ve ayrı ayrı hizmetlere (yapılandırılabilir) bir bölümü için ağırlık çoğunu sağlar. Bir ölçüm için genel Bakiye her hizmetinden ölçüm ağırlıkları ortalaması alınarak hesaplanır. Her hizmet kendi tanımlı ölçüm ağırlıklara göre dengelenir. Bu hizmetleri kendilerini içinde kendi gereksinimlerine göre dengelendiğinden sağlar. Sonuç olarak, hata aynı ilk düğüm başarısız olursa tüm hizmetleri tüm bölümler arasında dağıtılır. Her etkisi aynıdır.

## <a name="next-steps"></a>Sonraki adımlar
- Hizmetleri yapılandırma hakkında daha fazla bilgi için [hizmetleri yapılandırma hakkında bilgi edinin](service-fabric-cluster-resource-manager-configure-services.md)(service-fabric-cluster-resource-manager-configure-services.md)
- Birleştirme ölçümleri tanımlama, yaymak yerine düğümlerde yük birleştirmek için bir yoludur. Birleştirme yapılandırma konusunda bilgi için bkz [bu makalede](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
- Küme Kaynak Yöneticisi yönetir ve kümedeki yük dengeleyen hakkında bilgi almak için makalesine göz atın [Yük Dengeleme](service-fabric-cluster-resource-manager-balancing.md)
- En baştan başlatmak ve [için Service Fabric Küme Kaynak Yöneticisi giriş yapın](service-fabric-cluster-resource-manager-introduction.md)
- Taşıma maliyeti belirli hizmetleri taşıma diğerlerine göre daha pahalı küme kaynak yöneticisi için sinyal bir yoludur. Taşıma maliyeti hakkında daha fazla bilgi için bkz [bu makalede](service-fabric-cluster-resource-manager-movement-cost.md)

[Image1]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-cluster-layout-with-default-metrics.png
[Image2]:./media/service-fabric-cluster-resource-manager-metrics/Service-Fabric-Resource-Manager-Dynamic-Load-Reports.png
[Image3]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-metric-weights-impact.png
[Image4]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-global-vs-local-balancing.png
