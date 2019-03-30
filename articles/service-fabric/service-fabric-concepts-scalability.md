---
title: Service Fabric hizmetlerinin ölçeklenebilirliği | Microsoft Docs
description: Service Fabric Hizmetleri ölçeklendirme açıklar
services: service-fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: ''
ms.assetid: ed324f23-242f-47b7-af1a-e55c839e7d5d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 14a7389fe562b5f3206b81411d2224257051c636
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666656"
---
# <a name="scaling-in-service-fabric"></a>Service Fabric'te ölçeklendirme
Azure Service Fabric, hizmetler, bölümler ve çoğaltmalar bir küme düğümlerine yöneterek ölçeklenebilir uygulamalar oluşturmanızı kolaylaştırır. Aynı donanımda çok sayıda iş yükü çalıştıran en fazla kaynak kullanımını mümkün kılar, ancak aynı zamanda iş yüklerinizin ölçeğini nasıl seçim açısından esneklik sağlar. Bu kanal 9 videosunda, ölçeklenebilir bir mikro hizmet uygulamaları nasıl oluşturabileceğinizi açıklar:

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T116/player]

Service Fabric'te ölçeklendirme birkaç farklı şekilde gerçekleştirilir:

1. Oluşturma veya durum bilgisi olmayan hizmet örnekleri kaldırarak ölçeklendirme
2. Oluşturma veya yeni kaldırarak ölçeklendirme Hizmetleri adlı
3. Uygulama örnekleri adlı oluşturmak veya yeni kaldırarak ölçeklendirme
4. Bölümlenmiş hizmetlerini kullanarak ölçeklendirme
5. Ölçeklendirme ekleyerek ve kümeden düğüm kaldırma 
6. Küme Kaynak Yöneticisi ölçümleri kullanarak ölçeklendirme

## <a name="scaling-by-creating-or-removing-stateless-service-instances"></a>Oluşturma veya durum bilgisi olmayan hizmet örnekleri kaldırarak ölçeklendirme
Service Fabric içinde ölçeklendirme en kolay yollarından biri, durum bilgisi olmayan hizmetler ile çalışır. Durum bilgisi olmayan hizmet oluşturduğunuzda tanımlamak için imkanına sahip bir `InstanceCount`. `InstanceCount` hizmeti başladığında hizmetin kod çalışan kaç kopyasının oluşturulan tanımlar. Örneğin, kümede 100 düğüm olduğunu varsayalım. Ayrıca bir hizmet ile oluşturduğunuz diyelim ki bir `InstanceCount` 10. Çalışma zamanı sırasında bu 10 çalışan kod kopyaları tüm çok meşgul olabilir (veya yeterince meşgul bulunamadı). Bu iş yükü ölçeği bir örnek sayısını değiştirmek için yoludur. Örneğin, bazı izleme veya yönetim kod parçasını 50'ye veya olup iş yükü ölçeğini daraltma veya genişletme yüke bağlı gerektiğinde bağlı olarak 5, var olan örnek sayısını değiştirebilirsiniz. 

C# İÇİN:

```csharp
StatelessServiceUpdateDescription updateDescription = new StatelessServiceUpdateDescription(); 
updateDescription.InstanceCount = 50;
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/app/service"), updateDescription);
```

PowerShell:

```posh
Update-ServiceFabricService -Stateless -ServiceName $serviceName -InstanceCount 50
```
### <a name="using-dynamic-instance-count"></a>Kullanarak dinamik örnek sayısı
Özel durum bilgisi olmayan hizmetler için Service Fabric örnek sayısını değiştirmek için otomatik bir yöntem sunar. Bu hizmet kullanılabilir düğüm sayısı ile dinamik olarak ölçeklendirmenize olanak sağlar. Bu davranış geri çevirmek için örnek sayısı ayarlanacak yoludur = -1. Instancecount = -1'dir "Çalıştır bu durum bilgisi olmayan hizmet her düğümde." ifadesini içeren bir Service Fabric için bir yönerge Düğüm sayısı değişirse, Service Fabric hizmetinin geçerli tüm düğümler üzerinde çalıştığından emin olduktan eşleştirmek için örneği sayısı otomatik olarak değiştirir. 

C# İÇİN:

```csharp
StatelessServiceDescription serviceDescription = new StatelessServiceDescription();
//Set other service properties necessary for creation....
serviceDescription.InstanceCount = -1;
await fc.ServiceManager.CreateServiceAsync(serviceDescription);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName -Stateless -PartitionSchemeSingleton -InstanceCount "-1"
```

## <a name="scaling-by-creating-or-removing-new-named-services"></a>Oluşturma veya yeni kaldırarak ölçeklendirme Hizmetleri adlı
Belirli bir örneği bir hizmet türünün adlandırılmış hizmet örneğinde olduğu (bkz [Service Fabric uygulama yaşam döngüsü](service-fabric-application-lifecycle.md)) içinde kümedeki bazı Adlandırılmış uygulama örneği. 

Yeni bir adlandırılmış hizmet örnekleri oluşturulan (kaldırılamaz fazla veya az meşgul duruma hizmet olarak veya). Bu, genellikle yükü azaltmak için var olan hizmetleri sağlayan, daha fazla hizmet örnekleri arasında yaymak istekleri sağlar. Service Fabric küme kaynak yöneticisi hizmetleri Hizmetleri oluştururken, dağıtılmış bir biçimde kümedeki yerleştirir. Tam kararları yönetilir [ölçümleri](service-fabric-cluster-resource-manager-metrics.md) kümenin ve diğer yerleştirme kuralları. Hizmetleri, birkaç farklı şekilde oluşturulabilir, ancak en yaygın birisi çağırma gibi yönetim eylemleri aracılığıyla olan [ `New-ServiceFabricService` ](https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricservice?view=azureservicefabricps), ya da kod arama [ `CreateServiceAsync` ](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync?view=azure-dotnet). `CreateServiceAsync` bile gelen kümede çalışan diğer hizmetlere içinde çağrılabilir.

Oluşturma Hizmetleri dinamik olarak, her türlü içinde kullanılabilir ve bir ortak desendir. Örneğin, belirli bir iş akışını temsil eden bir durum bilgisi olan hizmet göz önünde bulundurun. Bu hizmet için gösterilecek iş temsil eden çağrılar yapacaksınız ve bu hizmet, bu iş akışı ve kayıt ilerleme adımlarına yürütülecek geçiyor. 

Nasıl bu belirli hizmet ölçek hale getirir? Hizmeti bazı formunda, çok kiracılı ve çağrılarını kabul ve adımlar için birçok farklı örnekleri aynı iş akışının tamamını aynı anda kazandırın. Artık tüm farklı aşamaları ve farklı müşterilerden birçok farklı aynı iş akışı örneği hakkında endişelenmeye gerek vardır ancak, kod beri daha karmaşık yapabilirsiniz. Ayrıca, aynı anda birden çok iş akışı işleme ölçek sorunu çözdü değil. Belirli bir noktada bu hizmet, belirli bir makinede sığdırmak için çok fazla kaynak tüketir olmasıdır. Bu düzen için yerleşik olmayan birçok hizmet de ilk başta nedeniyle bazı devralınmış bir performans sorunu veya yavaşlama zorluk kodlarında karşılaşırsınız. Bu tür sorunları hizmet eş zamanlı iş akışları, izleme sayısı büyük aldığında de çalışmamasına neden.  

İzlemek istediğiniz iş akışının farklı her örneği için bu hizmeti örneği oluşturmak için kullanılan bir çözümdür. Bu harika bir desendir ve durum bilgisi olan veya hizmeti olup olmadığını çalışır. Bu düzen çalışmak bir "iş yükü Yöneticisi hizmeti" davranan genellikle başka bir hizmet yoktur. Bu hizmetin işi isteklerini almak için ve diğer hizmetlere isteklere yönlendirmek için silinir. Yöneticisi ileti aldığında, iş yükü Hizmeti'nin bir örneğini dinamik olarak oluşturabilir ve ardından bu hizmetlere isteklerinde geçirin. Belirtilen iş akışı hizmeti, iş tamamlandığında Yöneticisi hizmeti geri çağırmaları de alabilir. Yöneticisi, bu geri aramalarda aldığında, iş akışı hizmeti örneğini silin veya çağrı beklenmiyordu tutulacaksa. 

Bu tür bir yönetici Gelişmiş sürümleri bile yönettiği Hizmetleri havuzları oluşturabilirsiniz. Havuzu, özel olarak yeni bir istek geldiğinde, hizmetin dönmesi bekleyin zorunda olmadığı sağlamaya yardımcı olur. Bunun yerine, Yöneticisi yalnızca havuzundan şu anda meşgul olmadığından bir iş akışı hizmeti seçin veya rastgele yol. İstek çalışmaya başlar yeni bir hizmet için beklenecek olduğunu olasılığını olduğundan Hizmetleri kullanılabilir bir havuzu tutma yeni işleme isteklerinin daha hızlı hale getirir. Yeni hizmetler oluşturmak için hızlı, ancak ücretsiz veya anlık. Havuz hizmet önce beklenecek istek zamanı en aza yardımcı olur. Yanıt süreleri en önemli olduğunda bu Yöneticisi ve havuz deseni genellikle görürsünüz. İstek sıraya alma ve arka planda hizmeti oluşturma ve _ardından_ geçirme da popüler manager desen gibi oluşturuyor ve bazı iş miktarını bu hizmet şu anda izleme üzerinde tabanlı hizmetler siliniyor bekleyen . 

## <a name="scaling-by-creating-or-removing-new-named-application-instances"></a>Uygulama örnekleri adlı oluşturmak veya yeni kaldırarak ölçeklendirme
Oluşturma ve tüm uygulama örnekleri siliniyor oluşturma ve silme Hizmetleri desenini benzerdir. Bu düzen için küme içinde diğer hizmetlerden alma onu görmeye istekleri ve bilgilere göre karar vermeden bazı Yöneticisi hizmeti yok. 

Ne zaman yeni bir adlandırılmış uygulama örneği zaten mevcut olan bazı uygulama yeni bir adlandırılmış hizmet örneklerini oluşturmak yerine kullanılmalıdır? Birkaç durum vardır:

  * Yeni uygulama örneği, kodu belirli bazı kimlik veya güvenlik ayarları altında çalıştırmak için gereken bir müşteri içindir.
    * Service Fabric, özel olarak farklı kod paketleri, belirli bir kimlik altında çalışmasına tanımlanmasına olanak sağlar. Aynı kod paketi farklı kimlikler altında başlatmak için farklı uygulama örneklerinin gerçekleşmesi etkinleştirmeleri gerekir. Dağıtılmış mevcut bir müşterinin iş yüklerini sahip olduğu bir durum düşünün. İzleme ve kendi uzak veritabanları gibi diğer kaynaklar ya da diğer sistemlerle erişimi denetlemek için bu belirli bir kimlik altında çalışıyor olabilir. Yeni bir müşteri kaydolduğunda bu durumda, muhtemelen kodlarını (işlem alanı) aynı bağlamda etkinleştirmek istediğiniz yok. Yapabilirsiniz, ancak bu, belirli bir kimliğe bağlamında yapacak hizmeti kodunuza zorlaştırır. Daha fazla güvenlik, yalıtım ve Kimlik Yönetimi kod genellikle olması gerekir. Hizmet örnekleri aynı uygulama örneğinin içinde farklı kullanmak yerine adlı ve bu nedenle aynı alanı işlem, farklı adlandırılmış bir Service Fabric uygulaması örnekleri kullanabilirsiniz. Bu, farklı kimlik bağlamları tanımlamak kolaylaştırır.
  * Yeni uygulama örneği de yapılandırmasının bir yol hizmet veren
    * Varsayılan olarak, tüm uygulama örneğini içinde belirli bir hizmet türüne adlandırılmış hizmet örneklerinin aynı işlem içinde belirli bir düğümde çalışır. Ne bu her hizmet örneği farklı şekilde yapılandırabilirsiniz; ancak, bunun yapılması kadar karmaşık olduğunu anlamına gelir. Hizmetleri, kendi yapılandırma içinde bir yapılandırma paketini aramak için kullandıkları bazı belirteç olmalıdır. Genellikle bu yalnızca hizmetin adıdır. Bu düzgün çalışır, ancak bu uygulama örneği içinde tek tek adlandırılmış hizmet örnekleri adlarına yapılandırmasını couples. Bu, karmaşık ve yapılandırma normalde bir tasarım zamanı yapıt uygulama örneği belirli değerlere sahip olduğundan yönetmek zor olabilir. Diğer hizmetler oluşturmak, her zaman daha fazla uygulama yükseltmelerini yapılandırma paketleri içinde bilgileri değiştirmek için ya da yeni hizmetleri kendi özel bilgiler bakabilirsiniz yenilerini dağıtılacağı anlamına gelir. Genellikle, yepyeni bir adlandırılmış uygulama örneği oluşturmak çok kolaydır. Ardından uygulama parametreleri ne olursa olsun hizmetler için gerekli bir yapılandırmadır ayarlamak için kullanabilirsiniz. Bu şekilde, içinde oluşturulan hizmetlerin Adlandırılmış uygulama örneği, belirli yapılandırma ayarları devralabilir. Örneğin, ayarları ve özelleştirmelerini gizli dizileri gibi veya her müşteri kaynak sınırları her müşteri için tek bir yapılandırma dosyası olması yerine, bunun yerine farklı uygulama örneği şu ayarlarla her müşteri için gerekir geçersiz kılındı. 
  * Yeni uygulama bir yükseltme sınırı hizmet verir.
    * Service Fabric içinde adlandırılmış farklı uygulama örneklerinin yükseltme için sınır olarak hizmet eder. Yükseltme bir adlandırılmış Uygulama örneğinin başka bir adlandırılmış uygulama örneği çalışıyor kodu etkilemez. Farklı uygulamalar aynı kodun farklı sürümleri aynı düğümler üzerinde çalışan sona erecek. Yeni kod veya başka bir hizmet olarak aynı yükseltmeleri izleyip izlemeyeceğini seçebileceğinden ölçeklendirme karar vermek, ihtiyacınız olduğunda bu bir etken olabilir. Örneğin, bir çağrı oluşturma ve Hizmetleri dinamik olarak silerek belirli bir müşterinin iş yükleri ölçeklendirme için sorumlu Yöneticisi hizmetine ulaşan varsayalım. Bu durumda ancak ilişkili bir iş yükü için çağrıdır bir _yeni_ müşteri. Müşterilerin çoğu yalnızca güvenlik ve yapılandırma nedenlerle birbirinden yalıtılmış, daha önce listelenen gibi ancak çünkü belirli bir yazılımı sürümlerini çalıştıran ve ne zaman yükseltilir seçme açısından daha fazla esneklik sağlar. Ayrıca yeni bir uygulama örneği oluşturur ve herhangi bir yükseltme touch hizmetlerinizi miktarını hizmete var. yalnızca daha fazla bölüm oluşturmak. Ayrı uygulama örnekleri uygulama yükseltme yaparken ayrıntı düzeyi sağlar ve ayrıca bir etkinleştirmek / B testi ve mavi/yeşil dağıtım. 
  * Var olan bir uygulama örneğini dolu
    * Service fabric'te [uygulama kapasitesi](service-fabric-cluster-resource-manager-application-groups.md) belirli uygulama örnekleri için kullanılabilir kaynakların miktarını denetlemek için kullanabileceğiniz bir kavramdır. Örneğin, belirli bir hizmeti ölçeklendirmek için oluşturulan başka bir örneğine sahip olması gerektiğini karar verebilirsiniz. Ancak, bu uygulama örneği için belirli bir ölçüm kapasite dışındadır. Daha fazla kaynak, bu belirli müşteri veya iş yükü yine de verilmelidir, ardından bu uygulama için mevcut kapasiteyi artırmak ya da yeni bir uygulama oluşturun. 

## <a name="scaling-at-the-partition-level"></a>Bölüm düzeyinde ölçeklendirme
Service Fabric bölümleme destekler. Bölümleme hizmet her biri bağımsız olarak çalışır birkaç mantıksal ve fiziksel bölümlere, ayırır. Bu durum bilgisi olan hizmetlerle yararlıdır, tüm çağrıları işlemek ve tüm durumları tek seferde işlemek hiç çoğaltmalarını ayarlandığı sahiptir. [Genel bakış bölümleme](service-fabric-concepts-partitioning.md) desteklenen bölümleme düzenleri türleri hakkında bilgi sağlar. Her bölüm çoğaltmalarını hizmetin yük dağıtımı ve bir bütün olarak hiçbir hizmet veya herhangi bir bölümünün bir tek hata noktası olduğundan emin bir kümedeki düğümler arasında yayılır. 

Düşük bir anahtar 0, 99 yüksek bir anahtarı ve bir bölüm sayısı 4'ün değerleriyle ranged bir bölümleme düzeni kullanan bir hizmet göz önünde bulundurun. Üç düğümlü bir kümede bulunan bir hizmet burada gösterildiği gibi her bir düğümünde kaynaklarını paylaşmak dört Çoğaltmalarla düzenlendiği:

<center>

![Üç düğüm ile bölüm düzeni](./media/service-fabric-concepts-scalability/layout-three-nodes.png)
</center>

Düğümlerin sayısını artırmak için Service Fabric var olan çoğaltmalardan bazıları taşıyacağız. Örneğin, şimdi varsayalım sayısı düğüm arttıkça dört ve çoğaltmaları yeniden. Artık service artık her düğümde her farklı bölümlerine ait çalışan üç çoğaltması yok. Bu, yeni düğümün soğuk değil olduğundan daha iyi kaynak kullanımı sağlar. Genellikle, daha fazla kaynak için kullanılabilir her bir hizmet olarak da performansı artırır.

<center>

![Dört düğüm ile bölüm düzeni](./media/service-fabric-concepts-scalability/layout-four-nodes.png)
</center>

## <a name="scaling-by-using-the-service-fabric-cluster-resource-manager-and-metrics"></a>Service Fabric Küme Kaynak Yöneticisi ve ölçümleri kullanarak ölçeklendirme
[Ölçümleri](service-fabric-cluster-resource-manager-metrics.md) Hizmetleri kaynak tüketimi için Service fabric'e nasıl express olan. Ölçümleri kullanarak küme kaynak yöneticisi yeniden düzenlemek ve küme düzenini en iyi duruma getirmek için bir fırsat sağlar. Örneğin, olabilir kümedeki kaynaklar bolca, ancak bunlar şu anda iş yaptığı hizmetlere ayrılabilir değil. Ölçümleri kullanarak hizmetleri kullanılabilir kaynaklara erişimi olmasını sağlamak için kümeye yeniden düzenlemek için Küme Kaynak Yöneticisi sağlar. 


## <a name="scaling-by-adding-and-removing-nodes-from-the-cluster"></a>Ölçeklendirme ekleyerek ve kümeden düğüm kaldırma 
Başka bir Service Fabric ile ölçeklendirme seçeneği ve kümenin boyutunu değiştirmektir. Kümenin boyutunu değiştirme, ekleme veya kümedeki düğümler bir veya daha fazla düğüm türü için kaldırma anlamına gelir. Örneğin, tüm küme düğümleri etkin olduğu bir durum düşünün. Bu, kümenizin kaynaklarını neredeyse tüm tüketilen olduğu anlamına gelir. Bu durumda, daha fazla düğüm kümeye ekleme ölçeklendirme için en iyi yoludur. Yeni düğümleri Kümeye katılma sonra Service Fabric Küme Kaynak Yöneticisi Hizmetleri, var olan düğümler küçük toplam yük výsledek taşır. Örnek sayısı ile durum bilgisi olmayan hizmetler için = -1, daha fazla hizmet örnekleri otomatik olarak oluşturulur. Bu, yeni düğümleri için var olan düğümleri taşımak için bazı çağrılar sağlar. 

Daha fazla bilgi için [küme ölçeklendirme](service-fabric-cluster-scaling.md).

## <a name="putting-it-all-together"></a>Hepsini bir araya getirme
Burada kısıtlayabildiğinden ve bir örnek konuşmak fikirleri ele alalım. Şu hizmet göz önünde bulundurun: adlarına bulunduran bir adres defteri görevi gören bir hizmet oluşturun ve kişi bilgilerini çalışıyorsunuz. 

Hemen bir sürü Ölçekle ilgili sorular sahip Önden: Kaç kullanıcının sahip olacak? Her kullanıcı kaç kişi depolayacak mı? Bu her şeyi anlamaya çalışırken, hizmetiniz için ilk kez ayarlama duran zordur. Belirli bir bölüm sayısı olan tek bir statik hizmetiyle giderek varsayalım. Yanlış bölüm sayısı çekme sonuçlarını ölçek sorunları daha sonra olmasına neden olabilir. Benzer şekilde, tüm bilgileri değil sahip olabileceğiniz doğru sayısı çekme olsa bile, gerekir. Örneğin, ayrıca hem düğüm sayısını ve boyutlarının açısından Önden, kümenin boyutunu karar vermelisiniz. Bu, genellikle hizmet yaşam süresi boyunca kullanmak için gittiği kaç kaynak tahmin etmek zordur. Ayrıca hizmet aslında gördüğü trafik desenini önceden gerektiğini belirlemek zor olabilir. Örneğin, belki de insanların ekleyip ilgili kişilerinin yalnızca ilk şey sabah veya belki de, eşit olarak gün boyunca dağıtılır. Dinamik olarak ölçek genişletme ve daraltma gerekebilir bunu temel alarak. Belki de ölçek genişletme ve daraltma ihtiyacınız olacak, ancak her iki durumda da, büyük olasılıkla hizmetiniz tarafından kaynak tüketimine değişen tepki vermek ihtiyacınız olacak tahmin etmek bilgi edinebilirsiniz. Bu, var olan kaynak kullanımını yeniden düzenleme yeterli olmadığı durumlarda, daha fazla kaynak sağlamak için kümenin boyutunu değiştirme içerebilir. 

Ancak neden tüm kullanıcılar için bir tek bir bölüm düzeni neyle bile deneyin? Neden kendiniz bir hizmet ve statik bir küme için sınırlamak? Gerçek durum, genellikle daha dinamik bir işlemdir. 

Ölçek için oluştururken, aşağıdaki dinamik düzeni göz önünde bulundurun. Kendi durumunuza uyarlamak gerekebilir:

1. Bir bölümleme düzeni herkes Önden için çekme çalışmak yerine, bir "Yöneticisi hizmeti" oluşturun.
2. İş Yöneticisi hizmeti, hizmete kaydolduğunda müşteri bilgilerine aramaktır. Bu bilgilere bağlı olarak Yöneticisi hizmeti oluşturup örneği, _gerçek_ kişi depolama hizmeti _yalnızca o müşteri için_. Belirli yapılandırma, yalıtım veya yükseltmeler ihtiyacınız varsa Ayrıca bu müşteri için bir uygulama örneği dönmeye karar verebilirsiniz. 

Bu dinamik oluşturma, birçok avantaj Desen:

  - Tüm kendi kendine sonsuz ölçeklenebilir tek bir hizmet oluşturun veya önden tüm kullanıcılar için doğru bölüm sayısını tahmin etmeye çalıştığınız değil. 
  - Farklı kullanıcılar aynı bölüm sayısı, yineleme sayısı, yerleştirme kısıtlamaları, ölçümleri, varsayılan yükler, hizmet adlarını, dns ayarlarını veya hizmet veya uygulama düzeyindeki diğer özelliklerden herhangi birini olması gerekmez. 
  - Ek veri Segment kazandığınız. Her bir müşterinin hizmet kendilerine ait kopyasında
    - Her müşteri hizmetleri farklı şekilde yapılandırılabilir ve bölümleri veya çoğaltmaları gerekli kendi beklenen ölçeği temel olarak daha az veya daha fazla veya daha az kaynaklar verilir.
      - Örneğin, ücretli "Altın" katmanı için - müşteri varsayalım, daha fazla çoğaltmalar veya daha fazla bölüm sayısı ve büyük olasılıkla kaynakları adanmış kapasite ölçümleri ve uygulama aracılığıyla hizmetler alabilir.
      - Veya "Küçük" gereken kişilerin sayısını gösteren bilgisini yükleyemedi sağlanan söyleyin - yalnızca birkaç bölümler elde edebileceğiniz veya hatta bir paylaşılan hizmet havuzu diğer müşterilerle içine yerleştirilebilir.
  - Müşterilerin gösterilmesi beklerken bir sürü hizmet örneği veya çoğaltmaları çalıştırmıyorsunuz
  - Bir müşteri hiç olmadığı kadar ayrılırsa, ardından hizmetinizden bilgilerini kaldırarak bu hizmeti veya onu oluşturan uygulamayı silmek manager sahip olacak şekilde kadar kolaydır.

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Service Fabric hizmetlerinin kullanılabilirliği](service-fabric-availability-services.md)
* [Service Fabric hizmetlerini bölümleme](service-fabric-concepts-partitioning.md)
