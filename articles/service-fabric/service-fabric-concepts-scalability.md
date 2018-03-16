---
title: "Service Fabric Hizmetleri ölçeklenebilirliğini | Microsoft Docs"
description: "Service Fabric Hizmetleri ölçeklendirmek açıklar"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: ed324f23-242f-47b7-af1a-e55c839e7d5d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: aeda1184610398c0445238ea2e7ccbea866ed418
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="scaling-in-service-fabric"></a>Service Fabric ölçeklendirme
Azure Service Fabric, hizmetler, bölümler ve çoğaltmalar bir küme düğümlerinde yöneterek ölçeklenebilir uygulamalar oluşturmanızı kolaylaştırır. Aynı donanımda birçok iş yükü çalıştıran en fazla kaynak kullanımı sağlar, ancak aynı zamanda, iş yüklerinin ölçeğini seçiminizi açısından esneklik sağlar. Bu Channel 9 video ölçeklenebilir mikro uygulamaları nasıl oluşturabileceğiniz açıklanmaktadır:

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T116/player]

Service Fabric ölçeklendirme birkaç farklı şekilde gerçekleştirilir:

1. Ölçek oluşturma veya durum bilgisiz hizmet örnekleri kaldırma
2. Hizmetleri adlı ölçek oluşturma veya yeni kaldırma
3. Uygulama örnekleri adlı ölçek oluşturma veya yeni kaldırma
4. Bölümlenmiş hizmetlerini kullanarak ölçeklendirme
5. Ekleme ve küme düğümleri kaldırarak ölçeklendirme 
6. Küme Kaynak Yöneticisi'ni ölçümleri kullanarak ölçeklendirme

## <a name="scaling-by-creating-or-removing-stateless-service-instances"></a>Ölçek oluşturma veya durum bilgisiz hizmet örnekleri kaldırma
Service Fabric içinde ölçeklendirmek için en kolay yollarından biri, durum bilgisiz Hizmetleri ile çalışır. Durum bilgisi olmayan bir hizmet oluşturduğunuzda tanımlamak için bir fırsat alma bir `InstanceCount`. `InstanceCount` hizmeti başladığında hizmetin kod kaç tane çalışan kopyalarını oluşturulan tanımlar. Örneğin, kümedeki 100 düğümleri olduğunu düşünelim. Ayrıca bir hizmet ile oluşturduğunuz düşünelim bir `InstanceCount` 10. Çalışma zamanı sırasında bu kodu 10 çalışan kopyalarını tüm meşgul hale gelebilir (veya yetecek kadar meşgul bulunamadı). İş yükü ölçeklendirmek için bir örneklerinin sayısını değiştirmek için yoludur. Örneğin, bazı izleme veya yönetim kod parçası varolan sayısının 50'ye veya olup iş yükü veya ölçeklendirmek yüküne göre gereken bağlı olarak 5 değiştirebilirsiniz. 

C# ' TA:

```csharp
StatelessServiceUpdateDescription updateDescription = new StatelessServiceUpdateDescription(); 
updateDescription.InstanceCount = 50;
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/app/service"), updateDescription);
```

PowerShell:

```posh
Update-ServiceFabricService -Stateless -ServiceName $serviceName -InstanceCount 50
```
### <a name="using-dynamic-instance-count"></a>Dinamik örnek sayısı kullanma
Özel durum bilgisi olmayan hizmetler için Service Fabric örnek sayısı değiştirmek için otomatik bir yol sağlar. Bu, dinamik olarak kullanılabilir düğüm sayısını ölçeklendirmek hizmet sağlar. Bu davranış kabul etmek için örnek sayısı ayarlanacak yoludur = -1. Instancecount = -1'dir "Çalıştır bu durum bilgisi olmayan hizmetin her düğümde." diyen bir Service Fabric bir yönerge Düğüm sayısı değişirse, Service Fabric hizmetinin geçerli tüm düğümler üzerinde çalıştığından emin olduktan eşleştirmek için örnek sayısı otomatik olarak değişir. 

C# ' TA:

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

## <a name="scaling-by-creating-or-removing-new-named-services"></a>Hizmetleri adlı ölçek oluşturma veya yeni kaldırma
Bir adlandırılmış hizmet örneği bir hizmet türünün belirli bir örneği olan (bkz [Service Fabric uygulama yaşam döngüsü](service-fabric-application-lifecycle.md)) kümedeki bazı Adlandırılmış uygulama örneği içinde. 

Yeni adlı hizmet örneği oluşturulan (daha az veya meşgul hale Hizmetleri olarak veya kaldırılabilir). Bu, genellikle yükü azaltmak için mevcut hizmetleri sağlayan daha fazla hizmet örnekleri arasında yaymak isteklere izin verir. Hizmetleri oluştururken Service Fabric kümesi kaynak yöneticisi hizmetleri küme dağıtılmış bir şekilde yerleştirir. Tam kararları tarafından yönetilir [ölçümleri](service-fabric-cluster-resource-manager-metrics.md) kümenin ve diğer yerleştirme kuralları. Hizmetleri birkaç farklı şekilde oluşturulabilir, ancak en yaygın birisi çağırma gibi yönetim eylemleri yoluyla olan [ `New-ServiceFabricService` ](https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricservice?view=azureservicefabricps), veya kod arama [ `CreateServiceAsync` ](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync?view=azure-dotnet). `CreateServiceAsync` bile gelen kümede çalışan diğer hizmetler içinde çağrılabilir.

Oluşturma Hizmetleri dinamik olarak her türlü içinde kullanılabilir ve ortak bir deseni. Örneğin, belirli bir iş akışı temsil eden bir durum bilgisi olan hizmet göz önünde bulundurun. İş temsil eden çağrıları bu hizmete görünmesini olacak ve bu iş akışı ve kayıt ilerleme adımlarına yürütmek için bu hizmet geçiyor. 

Nasıl bu belirli bir hizmet ölçek yapmak? Hizmet bazı formunda, çok kiracılı çağrılarını kabul ve birçok farklı örnekleri aynı iş akışı için adımları kapalı tümünü bir defada atarlar. Artık tüm farklı aşamalar ve farklı müşterilere ait, birçok farklı aynı iş akışı örnekleri hakkında endişelenmeye gerek mevcuttur ancak, kod itibaren daha karmaşık yapabilirsiniz. Ayrıca, aynı anda birden çok iş akışı işleme ölçek sorunu çözmek değil. Belirli bir noktada bu hizmetin belirli bir makinede sığması için çok fazla kaynak tüketir olmasıdır. Birçok hizmet için bu deseni yerleşik değil de ilk başta bazı devralınmış performans sorunu veya yavaşlama nedeniyle zorluk kendi kodunda karşılaşırsınız. Bu tür sorunları izleme mı eşzamanlı iş sayısı büyük aldığında de çalışmıyor hizmetinin neden.  

İzlemek istediğiniz iş akışı farklı her örneği için bu hizmet örneği oluşturmak için kullanılan bir çözümdür. Bu harika bir desen ve durum bilgisiz veya durum bilgisi olan hizmet olup olmadığını çalışır. Bu desen çalışmak "İş yükü Yöneticisi hizmeti" davranır genellikle başka bir hizmet yok. Bu hizmetin iş istekleri almaya ve bu istekleri diğer hizmetlere yönlendirmek için ' dir. Yöneticisi ileti aldığında, iş yükü Hizmeti'nin bir örneğini dinamik olarak oluşturabilir ve ardından isteklerinde Bu hizmetlere geçirebilirsiniz. Belirtilen iş akışı hizmeti işini tamamladığında Yöneticisi hizmeti de geri aramalar alabilir. Yöneticisi bu geri aramalar aldığında, iş akışı hizmeti örneğini silin veya daha fazla çağrıları beklenen, bırakın. 

Bu tür Yöneticisi'nin Gelişmiş sürümleri bile yönettiği Hizmetleri havuzları oluşturabilirsiniz. Havuzda yeni bir istek geldiğinde, hizmetinin dönmesi beklenecek sahip olmadığı sağlamaya yardımcı olur. Bunun yerine, yönetici yalnızca havuzundan şu anda kullanımda olmayan bir iş akışı hizmeti seçin veya rastgele rota. İstek çalışmaya başlar yeni bir hizmet için beklenecek sahip olasılığını olduğundan kullanılabilir hizmetler havuzu tutma işleme yeni isteklerinin daha hızlı hale getirir. Yeni hizmetler oluşturma, hızlı, ancak boş veya anlık bağlıdır. Havuzu hizmet önce beklenecek istek sahip süreyi en aza indirmenize yardımcı olur. Yanıt süreleri en önemli olduğunda bu Yöneticisi ve havuz deseni genellikle görürsünüz. İsteğin sıraya alma ve arka planda hizmet oluşturma ve _sonra_ geçirme de popüler Yöneticisi düzeni oluşturuyor gibi ve bazı iş miktarını bu hizmet şu anda izleme üzerinde temel Hizmetleri siliniyor sahip bekleyen . 

## <a name="scaling-by-creating-or-removing-new-named-application-instances"></a>Uygulama örnekleri adlı ölçek oluşturma veya yeni kaldırma
Oluşturma ve silme tüm uygulama örnekleri oluşturma ve Hizmetleri silme desenini benzer. Bu model için küme içindeki diğer hizmetlerden alma bunu görmesini istekleri ve bilgileri temelinde kararı vermeden bazı manager hizmeti yok. 

Ne zaman yeni bir adlandırılmış uygulama örneği oluşturma yeni bir adlandırılmış hizmet örneği zaten mevcut olan bazı uygulamada oluşturmak yerine kullanılsın mı? Bazı durumlar vardır:

  * Yeni uygulama, kod bazı belirli kimlik veya güvenlik ayarları altında çalıştırmak için gereken bir müşteri için örneğidir.
    * Service Fabric belirli kimlikleri altında çalıştırmak için farklı bir kod paketleri tanımlanmasına olanak sağlar. Farklı kimlik altında aynı kodu paket başlatmak için farklı uygulama durumlarda gerçekleşmesi etkinleştirmeleri gerekir. Dağıtılan mevcut bir müşterinin iş yükleri sahip olduğu bir durum düşünün. İzleme ve uzak veritabanları gibi başka kaynaklar veya diğer sistemler kendi erişimi denetlemek için bu belirli bir kimlik altında çalışıyor olabilir. Yeni bir müşteri kaydolduğunda, bu durumda, büyük olasılıkla kodlarını (işlem alanı) aynı bağlamda etkinleştirmek istediğiniz yok. Olabilir, ancak bu, belirli bir kimliğe bağlamında yapması hizmet kodunuzun zorlaştırır. Daha fazla güvenlik, yalıtım ve kimlik yönetimi kodu genellikle olması gerekir. Hizmet örneklerinin aynı uygulama örneği içinde farklı kullanmak yerine adlandırılmış ve bu nedenle aynı alanı işlemi, farklı adlı Service Fabric uygulaması örnekleri kullanabilirsiniz. Bu, farklı kimlik bağlamları tanımlamak kolaylaştırır.
  * Yeni Uygulama örneğinin aynı zamanda yapılandırmasının bir araç olarak görev yapar
    * Varsayılan olarak, tüm uygulama örneğini içindeki belirli bir hizmet türü adlandırılmış hizmet örneklerinin aynı işlem içinde belirli bir düğümde çalıştırın. Ne bu her hizmet örneği farklı bir şekilde yapılandırabilirsiniz ancak, bunun kadar karmaşık olmasıdır anlamına gelir. Hizmetler için bir yapılandırma paketi içinde kendi config aramak için kullandıkları bazı belirteci olması gerekir. Genellikle bu yalnızca hizmetin adıdır. Bu düzgün çalışır, ancak bu uygulama örneği içinde tek tek adlandırılmış hizmet örnekleri adlarına yapılandırma couples. Bu karmaşık ve yapılandırma normalde uygulama örneği belirli değerleri içeren bir tasarım zamanı yapısı olduğundan yönetmek zor olabilir. Daha fazla hizmet her zaman oluşturma, yapılandırma paketleri içindeki bilgileri değiştirmek veya yeni hizmetler kendi özel bilgiler arayabilir yenilerini dağıtmak için daha fazla uygulama yükseltmelerini anlamına gelir. Genellikle, yepyeni bir adlandırılmış uygulama örneği oluşturmak daha kolay olur. Ardından hangi hizmetlerin gerekli bir yapılandırmadır ayarlamak için uygulama parametreleri kullanabilirsiniz. Bu şekilde tüm, içinde oluşturulan Hizmetleri adlandırılmış uygulama örneği belirli yapılandırma ayarları devralabilirsiniz. Örneğin, sırlar ya da müşteri kaynak sınırları, başına her müşteri için özelleştirmeleri ve ayarları olan tek yapılandırma dosyası sahip olmak yerine yerine farklı uygulama örneği bu ayarlarla her müşteri için olurdu geçersiz kılındı. 
  * Yeni Uygulama yükseltme sınır olarak görev yapar
    * Service Fabric içinde farklı adlandırılmış uygulama örnekleri yükseltme için sınır olarak hizmet. Bir adlandırılmış uygulama örneği yükseltmesini başka bir adlandırılmış uygulama örneğini çalıştıran kod etkilemez. Farklı uygulamaları, aynı düğümlerde aynı kodu farklı sürümlerini çalıştıran sona erer. Yeni kod veya başka bir hizmet olarak aynı yükseltmeleri izleyip izlemeyeceğini seçebileceğinden ölçeklendirme bir kararı gerektiğinde bu bir etken olabilir. Örneğin, bir çağrı oluşturma ve Hizmetleri dinamik olarak silme belirli bir müşterinin iş yükleri ölçekleme için sorumlu Yöneticisi hizmetine ulaşan söyleyin. Bu durumda ancak, ilişkili bir iş yükü için çağrıdır bir _yeni_ müşteri. Müşterilerin çoğu birbirinden yalnızca güvenlik ve yapılandırma nedeniyle yalıtılmış, daha önce listelenen gibi ancak çünkü yazılım belirli sürümlerini çalıştıran ve ne zaman yükseltme seçme açısından daha fazla esneklik sağlar. Ayrıca yeni bir uygulama örneği oluşturun ve herhangi bir yükseltme touch hizmetlerinizi miktarda hizmete var. yalnızca daha fazla bölüm oluşturmak. Ayrı uygulama örnekleri uygulama yükseltme yaparken büyük ayrıntı düzeyi sağlamak ve ayrıca A etkinleştirin / B testi ve mavi/yeşil dağıtımları. 
  * Mevcut uygulama örneği dolu
    * Service Fabric içinde [uygulama kapasite](service-fabric-cluster-resource-manager-application-groups.md) belirli uygulama örnekleri için kullanılabilir kaynakları miktarını denetlemek için kullanabileceğiniz bir kavramdır. Örneğin, belirli bir hizmeti ölçeklendirmek için oluşturulan başka bir örneğine sahip olması gerektiğini karar verebilirsiniz. Ancak, bu uygulama örneği belirli bir ölçüm için kapasite dışındadır. Bu belirli müşteri ya da iş yükü daha fazla kaynak hala verilmelidir, ardından bu uygulama için var olan kapasitesini artırmak ya da yeni bir uygulama oluşturun. 

## <a name="scaling-at-the-partition-level"></a>Bölüm düzeyde ölçekleme
Service Fabric bölümleme destekler. Bölümleme hizmet her biri bağımsız olarak çalışır birkaç mantıksal ve fiziksel bölümlere, ayırır. Bu durum bilgisi olan hizmetleriyle yararlıdır, tüm çağrıları işlemek ve durumu tümünün aynı anda işlemek hiç kimse çoğaltmalarının ayarlamak beri sahiptir. [Genel bakış bölümleme](service-fabric-concepts-partitioning.md) desteklenen bölümleme şeması türleri hakkında bilgi sağlar. Her bölüm kopyalarını hizmetin yük dağıtımı ve bir bütün olarak hizmet hiçbiri veya herhangi bir bölümü tek hata noktası olduğundan emin olduktan bir kümedeki düğümleri arasında yayılır. 

Düşük anahtarı 0, 99 yüksek anahtarı ve 4 bölüm sayısı ile ranged bölümleme düzeni kullanan bir hizmet göz önünde bulundurun. Üç düğümlü bir küme içinde hizmet aşağıda gösterildiği gibi her bir düğümde kaynakları paylaşan dört yinelemelerle düzenlendiği:

<center>
![Üç düğümü olan bir bölüm düzeni](./media/service-fabric-concepts-scalability/layout-three-nodes.png)
</center>

Service Fabric düğümlerin sayısını artırmak için mevcut çoğaltmaları bazıları var. taşınır. Örneğin, şimdi deyin dört ve çoğaltmaları düğümleri artışları sayısı yeniden dağıtılabilir. Şimdi hizmet artık her düğümde her farklı bölümlere ait çalışan üç çoğaltmaları sahiptir. Bu, yeni düğümü soğuk değil olduğundan daha iyi kaynak kullanımı sağlar. Genellikle, her hizmet için kullanılabilir kaynakları içerdiğinden da performansı artırır.

<center>
![Dört düğüm ile bir bölüm düzeni](./media/service-fabric-concepts-scalability/layout-four-nodes.png)
</center>

## <a name="scaling-by-using-the-service-fabric-cluster-resource-manager-and-metrics"></a>Service Fabric kümesi Kaynak Yöneticisi ve ölçümleri kullanarak ölçeklendirme
[Ölçümleri](service-fabric-cluster-resource-manager-metrics.md) nasıl Hizmetleri Service Fabric kendi kaynak tüketimi express şunlardır. Ölçümleri kullanarak küme Kaynak Yöneticisi'ni yeniden düzenlemeniz ve küme düzenini en iyi duruma getirme olanağı sağlar. Örneğin, olabilir kümedeki kaynaklar Eskinin, ancak bunlar şu anda iş yapmakta olduğunuz Hizmetleri ayrılabilir değil. Ölçümleri kullanarak hizmetleri kullanılabilir kaynaklara erişimi olduğundan emin olun kümeye yeniden düzenlemek için Küme Kaynak Yöneticisi'ni sağlar. 


## <a name="scaling-by-adding-and-removing-nodes-from-the-cluster"></a>Ekleme ve küme düğümleri kaldırarak ölçeklendirme 
Service Fabric ile ölçekleme için başka bir küme boyutunu değiştirmek için bir seçenektir. Küme boyutunu değiştirme, düğüm ekleme veya bir veya daha fazla düğüm türleri için kümedeki kaldırma anlamına gelir. Örneğin, tüm küme düğümlerinin etkin olduğu bir durum düşünün. Bu kümenin kaynakları neredeyse tüm tüketilen anlamına gelir. Bu durumda, daha fazla düğüm kümeye ekleme ölçeklendirmek için en iyi yoludur. Kümeye yeni düğümler katılma sonra Service Fabric kümesi Resource Manager Hizmetleri, var olan düğümler üzerinde daha az toplam yük kaynaklanan taşır. Örnek sayısı ile durum bilgisi olmayan hizmetler için = -1, daha fazla hizmet örnekleri otomatik olarak oluşturulur. Bu yeni düğümler varolan düğümlerden taşımak bazı çağrıları sağlar. 

Ekleme ve kaldırma küme düğümlerine Service Fabric Azure Resource Manager PowerShell modülü yoluyla gerçekleştirilebilir.

```posh
Add-AzureRmServiceFabricNode -ResourceGroupName $resourceGroupName -Name $clusterResourceName -NodeType $nodeTypeName  -NumberOfNodesToAdd 5 
Remove-AzureRmServiceFabricNode -ResourceGroupName $resourceGroupName -Name $clusterResourceName -NodeType $nodeTypeName -NumberOfNodesToRemove 5
```

## <a name="putting-it-all-together"></a>Tüm bir araya getirme
Burada tartışılan ve bir örnek konuşun tüm fikirleri atalım. Aşağıdaki hizmet göz önünde bulundurun: adlarına bulunduran bir adres defteri görevi gören bir hizmet oluşturmak ve kişi bilgilerini çalışıyorsunuz. 

Bir grup Ölçekle ilgili sorular sahip Önden sağ: kaç kullanıcının sahip olacak? Her kullanıcı kaç kişi depolayacak mı? Bu her şeyi anlamaya çalışırken zaman hizmetinizi ilk kez yukarı duran zordur. Belirli bir bölüm sayısı olan tek bir statik hizmetle giderek varsayalım. Yanlış bölüm sayısı çekme sonuçlarıyla ölçek sorunları daha sonra olmasına neden olabilir. Benzer şekilde, tüm bilgileri değil olabilir sağ sayısı çekme olsa bile, gerekir. Örneğin, size ayrıca Önden, küme boyutu hem düğümlerin sayısını ve boyutlarının açısından karar vermelisiniz. Bu, genellikle bir hizmet yaşam süresi boyunca tüketen gittiği kaç kaynak tahmin etmek zordur. Ayrıca hizmet gerçekte gördüğü trafiği düzeni önceden bilmeniz zor olabilir. Örneğin, olabilir kişileri ekleyin ve bunların yalnızca ilk şey sabah kaldırın veya belki de onu eşit olarak gün süresince dağıtılır. Oturumunuzu kapatıp dinamik olarak ölçeklendirmek için gerekebilecek bunu temel alarak. Olabilir ve ölçeklendirmek ihtiyacınız olacak, ancak her iki durumda da, büyük olasılıkla, hizmetiniz tarafından kaynak tüketimini değiştirmeye tepki vermek gereksinim duyacağınızı tahmin etmek bilgi edinebilirsiniz. Bu, var olan kaynakların kullanımını yeniden düzenleme yeterli olmadığı durumlarda daha fazla kaynak sağlamak için küme boyutunu değiştirme içerebilir. 

Ancak neden bile tüm kullanıcılar için tek bir bölüm düzeni seçin çalıştığınızda? Neden kendiniz bir hizmet ve bir statik küme sınırlamak? Gerçek genellikle daha dinamik bir durumdur. 

İçin ölçek oluştururken, aşağıdaki dinamik düzeni göz önünde bulundurun. Durumunuza uyarlamanız gerekebilir:

1. Bölümleme düzeni herkes Önden için çekme gibi çalışılırken yerine, "Yöneticisi hizmeti" oluşturun.
2. Bunlar hizmetiniz için kaydolduğunuzda, müşteri bilgileri aramak için İş Yöneticisi hizmeti ediyor. Bu bilgiye bağlı olarak Yöneticisi hizmeti oluşturup örneği, _gerçek_ kişi depolama hizmeti _yalnızca o müşteri için_. Belirli yapılandırma, yalıtım veya yükseltmeler ihtiyacınız varsa, ayrıca bu müşteri için uygulama örneğini dönmeye karar verebilirsiniz. 

Bu dinamik oluşturma birçok avantaj Desen:

  - Önden tüm kullanıcılar için doğru bölüm sayısı tahmin veya tüm kendi başına sonsuz ölçeklenebilir tek bir hizmet oluşturmak çalıştığınız değil. 
  - Farklı kullanıcılar aynı bölüm sayısı, çoğaltma sayısı, kısıtlamalarından, ölçümleri, varsayılan yükler, hizmet adlarını, dns ayarlarını veya herhangi bir hizmet veya uygulama düzeyinde belirtilen diğer özelliklerin olması gerekmez. 
  - Ek veri kesimleme kazanır. Her bir müşteri hizmeti, kendi kopyasına sahip
    - Her müşteri hizmeti farklı şekilde yapılandırılmış ve bölümleri veya çoğaltmaları gerekli kendi beklenen ölçekte dayalı olarak daha az veya daha fazla veya daha az kaynakları verildi.
      - Örneğin, "Altın" katmanını - Ücretli müşteri söyleyin bunlar daha fazla çoğaltmaları veya daha fazla bölüm sayısı ve büyük olasılıkla kaynakları ölçümleri ve uygulama kapasiteleri aracılığıyla kendi hizmetlerine ayrılmış alabilirsiniz.
      - Veya "Küçük" gerekli kişi sayısını belirten bilgi oldu sağlanan söyleyin - yalnızca birkaç bölümleri elde etmesi veya hatta diğer müşteriler paylaşılan hizmet havuzuyla içine konmuş.
  - Müşterilerin gösterilmesi beklerken bir dizi hizmet örneği veya çoğaltmaları çalıştırdığınız değil
  - Bir müşteri hiç ayrılırsa, ardından hizmetinizden bilgilerini kaldırarak bu hizmeti veya oluşturulduğu uygulamayı silmek Yöneticisi sahip olarak kadar basittir.

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Service Fabric hizmetlerin kullanılabilirliğini](service-fabric-availability-services.md)
* [Service Fabric Hizmetleri bölümlendirme](service-fabric-concepts-partitioning.md)
