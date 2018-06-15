---
title: Microsoft Azure Service Fabric hakkında sık sorulan sorular | Microsoft Docs
description: Service Fabric ve yanıtlarını hakkında sık sorulan sorular
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: ''
ms.assetid: 5a179703-ff0c-4b8e-98cd-377253295d12
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: chackdan
ms.openlocfilehash: a40432aa1d9a466706b4a3ebbcbd56cd8e5b768e
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34205571"
---
# <a name="commonly-asked-service-fabric-questions"></a>Sık sorulan soruları Service Fabric

Service Fabric neler yapabileceğinizi ve nasıl kullanılacağını hakkında çok sık sorulan sorular bulunmaktadır. Bu belgede bu ortak sorularını ve yanıtlarını çoğunu kapsar.

## <a name="cluster-setup-and-management"></a>Küme kurulumu ve Yönetimi

### <a name="how-do-i-rollback-my-service-fabric-cluster-certificate"></a>Nasıl yedeklerim geri alma my Service Fabric kümesi sertifika?

Geri alma uygulamanıza yükseltme, Service Fabric Küme çekirdeğini değişikliği gerçekleştirmeden önce sistem durumu hatası algılama gerektirir; yapılan değişiklikleri yalnızca ileri alınabilir. Yükseltme mühendisi ait Müşteri Destek Hizmetleri ile kümeyi kurtarmak için gerekli bir izlenmeyen önemli sertifika değişiklik sunulan durumunda olabilir.  [Service Fabric'ın uygulama yükseltme](https://review.docs.microsoft.com/en-us/azure/service-fabric/service-fabric-application-upgrade?branch=master) geçerlidir [uygulama yükseltme parametreleri](https://review.docs.microsoft.com/en-us/azure/service-fabric/service-fabric-application-upgrade-parameters?branch=master), ve sıfır kapalı kalma süresi yükseltme promise sunar.  İzlenen modu önerilen uygulamamız yükseltme, otomatik güncelleştirme etki alanları ilerlemeyi otomatik olarak varsayılan hizmet güncelleştirmek geçen, çalışırken geri başarısız durumu denetimleri sırasında dayanır.
 
Kümenizi hala önerilir, Resource Manager şablonu Klasik sertifika parmak izi özelliğinde yararlanarak varsa, [sertifika parmak izi değişiklik kümeden ortak adı için](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-change-cert-thumbprint-to-cn), modern gizli yararlanmak için Yönetim Özellikleri.

### <a name="can-i-create-a-cluster-that-spans-multiple-azure-regions-or-my-own-datacenters"></a>Birden çok Azure bölgeleri veya kendi veri merkezlerini yayılan bir küme oluşturabilir miyim?

Evet. 

Service Fabric kümeleme teknolojisine çekirdek birbirleri ile ağ bağlantısına sahip oldukları sürece dünyanın her yerden çalışan makineler birleştirmek için kullanılabilir. Ancak, oluşturma ve böyle bir kümeyi çalıştırma karmaşık olabilir.

Bu senaryoda ilgileniyorsanız, kişi ya da almak için öneririz aracılığıyla [Service Fabric Github sorunlar listesine](https://github.com/azure/service-fabric-issues) veya ek yönergeler almak için destek temsilcisine aracılığıyla. Service Fabric takım açıklığa, yönergeler ve bu senaryo için öneriler sağlamak üzere çalışmaktadır. 

Dikkate alınması gereken bazı noktalar şunlardır: 

1. Azure Service Fabric kümesi kaynak Bugün, sanal makine ölçek kümesi yerleşik olan ayarlar olduğunuz bölgesel aynıdır. Bu, bölgesel bir arıza olması durumunda Azure Resource Manager veya Azure Portalı aracılığıyla küme yönetme olanağı kaybedebilir anlamına gelir. Kümenin çalışmaya devam eder ve doğrudan ile etkileşim olacaktır olsa bile bu durum oluşabilir. Ayrıca, Azure bölgeler arasında kullanılabilir tek bir sanal ağ sahip olma yeteneği bugün sağlamaz. Bu Azure bölgeli kümede ya da gerektirdiği anlamına gelir. [VM ölçek kümesindeki her bir VM için genel IP adresleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-networking.md#public-ipv4-per-virtual-machine) veya [Azure VPN ağ geçitleri](../vpn-gateway/vpn-gateway-about-vpngateways.md). Bu ağ farklı etkileri maliyetler, performans, seçeneğiniz ve bazı derece uygulama tasarımı için bu nedenle dikkatli analiz ve planlama gereklidir yeri gibi bir ortam kurma önce.
2. Bakım, yönetim, ve bu makinelerin izleme dönüşebilir karmaşık, özellikle arasında dağıtılmış olduğunda _türleri_ gibi farklı bulut sağlayıcıları arasında veya şirket içi kaynakları ile Azure arasında ortamlarının. Yükseltme, izleme, yönetim ve tanılama küme ve uygulamalar için bu tür bir ortamda üretim iş yükleri çalıştırmadan önce anlaşılır emin olmak için dikkatli olunması gerekir. Azure veya kendi veri merkezleri içinde bu sorunları çözmek deneyimi zaten varsa, daha sonra bu çıkışı oluşturma veya Service Fabric kümesi çalıştırdığınızda bu aynı çözümler uygulanabilir olasıdır. 

### <a name="do-service-fabric-nodes-automatically-receive-os-updates"></a>Service Fabric düğümlerin işletim sistemi güncelleştirmelerini otomatik olarak alıyorsunuz?

Bugün değil, ancak bu ayrıca Azure teslim amaçlayan bir ortak isteği.

Bu arada, sahibiz [bir uygulama sağlanan](service-fabric-patch-orchestration-application.md) Service Fabric düğümleriniz altında işletim sistemlerini düzeltme eki ve güncel kalmasını.

Sınama ile işletim sistemi güncelleştirmelerini, bunlar genellikle geçici kullanılabilirlik kaybına neden olur makinenin yeniden başlatılması gerekir ' dir. Service Fabric diğer düğümlere otomatik olarak bu hizmetlerin trafiğini yönlendirir kendisi tarafından bir sorun değildir. Ancak, işletim sistemi güncelleştirmelerini kümede düzenlenir değil, aynı anda birçok düğümleri Git aşağı olası yoktur. Bu tür eşzamanlı yeniden başlatmalar veya bir hizmet için tam kullanılabilirlik kaybına neden olabilir (durum bilgisi olan bir hizmeti için) belirli bir bölüm için en az.

Gelecekte, tamamen otomatik ve güncelleştirme etki alanları arasında eşgüdümlü bir işletim sistemi güncelleştirme ilkesi desteği kullanılabilirlik yeniden başlatmalar ve diğer beklenmeyen hataları rağmen korunduğundan emin olduktan planlıyoruz.

### <a name="can-i-use-large-virtual-machine-scale-sets-in-my-sf-cluster"></a>Büyük sanal makine ölçek kümeleri my BT kümede kullanabilir miyim? 

**Kısa yanıt** - Hayır 

**Uzun yanıt** - büyük sanal makine ölçek kümesi bir sanal makine ölçeklendirmenizi izin verse de kümesi fazla 1000 VM örnekleri ölçeklendirme, yerleştirme grupları (PGs) kullanarak bunu yapar. Yalnızca hata etki alanlarını (FDs) ve yükseltme etki alanları (UDs) yerleştirme Grup Service fabric kullandığı içinde FDs ve UDs, hizmet çoğaltmaları/hizmet örnekleri yerleştirme kararları almak için tutarlı değil. FDs ve UDs yerleştirme grup içinde yalnızca karşılaştırılabilir olduğundan, BT tarafından kullanılamaz. PG1 VM1 FD topolojisini varsa, örneğin, = 0 ve PG2 VM9 FD topolojisini = 4, onu gelmez VM1 ve VM2 olan iki farklı donanım rafları, dolayısıyla BT FD değerleri bu durumda yerleştirme kararları için kullanamazsınız.

Diğer sorunlar var. büyük sanal makine ölçek kümeleri ile şu anda olmaması, düzey 4 yükleme gibi karşı desteği İçin başvurmak [büyük ayrıntıları ölçekleme kümeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md)



### <a name="what-is-the-minimum-size-of-a-service-fabric-cluster-why-cant-it-be-smaller"></a>Service Fabric kümesi minimum boyutu nedir? Neden küçük olamaz?

Minimum desteklenen üretim iş yükleri çalıştıran bir Service Fabric kümesi için beş düğümü boyutudur. Geliştirme ve test senaryoları için üç düğümlü kümeler destekliyoruz.

Service Fabric kümesi adlandırma hizmeti ve Yük Devretme Yöneticisi dahil olmak üzere, durum bilgisi olan sistem hizmetleri kümesi çalıştığından bu zamanlayıcısındaki mevcut. Bu hizmetler, hangi hizmetlerin kümeye ve bunlar şu anda barındırıldığı, dağıtılan hangi izleme üzerinde güçlü tutarlılık bağlıdır. Bu güçlü tutarlılık sırayla alma yeteneğini bağlıdır bir *çekirdek* bu durum verilen herhangi bir güncelleştirme hizmetleri, bir çekirdek (N/2 + 1) çoğaltmalarını, belirli bir hizmeti katı çoğunu temsil ettiği için.

Bu arka plan ile bazı olası küme yapılandırmaları inceleyelim:

**Bir düğüm**: herhangi bir nedenle kümenin tamamının kaybı anlamına gelir bu seçenek yüksek kullanılabilirlik tek düğümlü kaybı itibaren sağlamaz.

**İki düğüm**: iki düğüm arasında dağıtılan bir hizmet için çekirdek (N = 2) 2'dir (2/2 + 1 = 2). Tek bir çoğaltma kaybolursa, bir çekirdek oluşturmak mümkün değildir. Hizmet yükseltme gerçekleştirme geçici olarak bir çoğaltma sürüyor gerektirdiğinden, bu kullanışlı bir yapılandırma değildir.

**Üç düğümü**: (N = 3) üç düğümü ile bir çekirdek oluşturmak için hala iki düğüm gereksinimdir (3/2 + 1 = 2). Bu, tek bir düğümünü kaybeder ve hala çekirdeğini korumak anlamına gelir.

Güvenli bir şekilde yükseltme yapmak ve tek tek düğüm hatalarını varlığını sürdürmesi için aynı anda durum mu olduğu sürece üç düğüm Küme yapılandırması geliştirme ve test için desteklenir. Beş düğümü gerekli; bu nedenle üretim iş yükleri için bu tür bir eşzamanlı hatalarına karşı dayanıklı olması gerekir.

### <a name="can-i-turn-off-my-cluster-at-nightweekends-to-save-costs"></a>My küme gece/maliyet tasarrufu için hafta sonları kapatabilir miyim?

Genellikle yapamazsınız. Service Fabric, sanal makineyi farklı bir ana bilgisayara taşınırsa, verileri ile taşımaz, yani, yerel, kısa ömürlü disklerde durumu depolar. Diğer düğümleri tarafından yeni düğümü güncel hale getirilene gibi normal işleminde, bir sorun değildir. Ancak, tüm düğümler durdurur ve daha sonra yeniden düğümlerin en yeni ana bilgisayar ve yapma kurtaramaz sistem üzerinde başlatmak önemli bir olasılığı yoktur.

Dağıtmadan önce uygulamanızı test etmek için kümeleri oluşturmak istiyorsanız, bu kümeleri dinamik olarak parçası olarak oluşturmanızı öneririz, [sürekli tümleştirme/sürekli dağıtım ardışık düzen](service-fabric-tutorial-deploy-app-with-cicd-vsts.md).


### <a name="how-do-i-upgrade-my-operating-system-for-example-from-windows-server-2012-to-windows-server-2016"></a>My işletim sisteminden (örneğin Windows Server 2012 için Windows Server 2016) nasıl yükseltme?

Gelişmiş bir deneyim üzerinde bugün çalışıyoruz ancak yükseltme için siz sorumlusunuz. Küme sanal makinelerin işletim sistemi görüntüsü yükseltmelisiniz birer birer VM. 

### <a name="can-i-encrypt-attached-data-disks-in-a-cluster-node-type-virtual-machine-scale-set"></a>Bir küme düğümü türü (sanal makine ölçek kümesi) bağlı veri diskleri şifreleyebilir mi?
Evet.  Daha fazla bilgi için bkz: [eklenen veri disklerini ile küme oluşturma](../virtual-machine-scale-sets/virtual-machine-scale-sets-attached-disks.md#create-a-service-fabric-cluster-with-attached-data-disks), [şifrelemek diskler (PowerShell)](../virtual-machine-scale-sets/virtual-machine-scale-sets-encrypt-disks-ps.md), ve [şifrelemek diskleri (CLI)](../virtual-machine-scale-sets/virtual-machine-scale-sets-encrypt-disks-cli.md).

### <a name="what-are-the-directories-and-processes-that-i-need-to-exclude-when-running-an-anti-virus-program-in-my-cluster"></a>Dizinleri ve virüs koruma programı my kümede çalışırken hariç gereken işlemleri nelerdir?

| **Virüsten koruma dışlanan dizinler** |
| --- |
| Program Files\Microsoft Service Fabric |
| FabricDataRoot (küme yapılandırmasından) |
| FabricLogRoot (küme yapılandırmasından) |

| **Virüsten koruma hariç tutulan işlemler** |
| --- |
| Fabric.exe |
| FabricHost.exe |
| FabricInstallerService.exe |
| FabricSetup.exe |
| FabricDeployer.exe |
| ImageBuilder.exe |
| FabricGateway.exe |
| FabricDCA.exe |
| FabricFAS.exe |
| FabricUOS.exe |
| FabricRM.exe |
| FileStoreService.exe |
 
## <a name="application-design"></a>Uygulama tasarımı

### <a name="whats-the-best-way-to-query-data-across-partitions-of-a-reliable-collection"></a>Güvenilir bir koleksiyonun bölümler sorgu veri en iyi yolu nedir?

Güvenilir koleksiyonlarıdır genellikle [bölümlenmiş](service-fabric-concepts-partitioning.md) daha yüksek performans ve verimlilik için ölçek genişletme etkinleştirmek için. Belirli bir hizmeti durumunun onlarca veya makineler yüzlerce yayılan, anlamına gelir. Bu tam veri kümesi üzerinde işlemler gerçekleştirmek üzere, birkaç seçeneğiniz vardır:

- Tüm bölümler gerekli verileri çıkarmak için başka bir hizmet, sorgular bir hizmet oluşturun.
- Başka bir hizmetin tüm bölümler veri aldığınız bir hizmet oluşturun.
- Düzenli aralıklarla veri her hizmetinden bir dış depoya iletin. Bu yaklaşım, yalnızca, gerçekleştiriyorsunuz sorguları çekirdek iş mantığınızı parçası değilse uygundur.


### <a name="whats-the-best-way-to-query-data-across-my-actors"></a>My aktörler arasında verileri Sorgulama en iyi yolu nedir?

Aktör çalışma zamanında aktör durumunun geniş sorguları gerçekleştirmek için önerilmez şekilde durumu ve işlem, bağımsız bir birim olacak şekilde tasarlanmıştır. Aktör durumunun tam kümesi boyunca sorgu için bir gereksiniminiz varsa, ya da dikkate almanız:

- Böylece hizmetinizi bölüm sayısı için aktörler sayısından tüm verileri toplamak için istekleri ağ sayısı aktör hizmetlerinizi durum bilgisi olan güvenilir hizmetler ile değiştirin.
- Düzenli aralıklarla daha kolay sorgulama için bir dış depolama durumlarına göndermek için aktörler tasarlama. Yukarıdaki bu yaklaşım yalnızca gerçekleştirmekte sorguları kullanarak çalışma zamanı davranışını gerekli değilse uygulanabilir gibidir.

### <a name="how-much-data-can-i-store-in-a-reliable-collection"></a>Ne kadar veri ı güvenilir bir koleksiyonda depolayabilir miyim?

Kümedeki sahip makine sayısı ve bu makineleri kullanılabilir bellek miktarına göre depolayabileceğiniz tutar yalnızca sınırlı şekilde güvenilir hizmetler genellikle bölümlenir.

Bir örnek olarak, güvenilir bir koleksiyon 100 bölümleri ve 3 çoğaltmalar, bir hizmet olarak 1 kb boyutunda ortalama nesneleri depolamak olduğunu varsayalım. Şimdi makine başına bellek 16 gb ile 10 makine kümesi olduğunu varsayalım. Basitlik için ve koruyucu, varsayımında 6 küme için makine başına kullanılabilir 10 gb ya da 100 gb bırakarak gb, işletim sistemi ve Sistem Hizmetleri, Service Fabric çalışma zamanını ve hizmetlerinizi tüketir.

Her bir nesne olmalıdır göz önünde bulundurarak, depolanan üç (bir birincil ve iki çoğaltma), yaklaşık 35 milyon nesneler için yeterli bellek koleksiyonunuzda tam kapasitede çalışırken olurdu. Ancak, bir hata etki alanı ve yaklaşık 1/3 kapasiteye temsil eder ve kabaca 23 milyon azaltmak bir yükseltme etki eşzamanlı kaybı dayanıklı olması önerilir.

Bu hesaplama ayrıca varsaydığını unutmayın:

- Dağıtım bölümleri arasında veri kabaca Tekdüzen veya küme kaynak yöneticisi için yük ölçümleri raporlama. Varsayılan olarak, Service Fabric Bakiye yineleme sayısına göre yükler. Önceki örnekte, kümedeki her düğümde, 10 birincil çoğaltma ve 20 ikincil çoğaltmaları koyabilirsiniz. İyi bölümleri arasında eşit olarak dağıtılmış yük için işe yarar. Yük bile değilse, böylece Kaynak Yöneticisi'ni küçük çoğaltmaları birlikte paketi ve tek tek bir düğüm üzerinde daha fazla bellek tüketmesine büyük çoğaltmaları izin yük bildirilmesi gerekir.

- Söz konusu güvenilir hizmetin küme depolama yalnızca bir durumda olduğunu. Bir kümeye birden çok hizmet dağıtabilirsiniz olduğundan, kaynakları dikkatli olmanız gerekir her çalıştırın ve durumunu yönetmek gerekir.

- Küme büyüyen küçültme veya olduğunu. Daha fazla makine eklerseniz, Service Fabric tek tek bir çoğaltma makineler yayılamaz bu yana makine sayısı hizmetinizi bölüm sayısı değerini geçiyor kadar ek kapasite yararlanmak için çoğaltmalar yeniden dengelemeniz. Bunun aksine, makineler kaldırarak küme boyutunu azaltın, çoğaltmalarınızı daha sıkı bir şekilde paketlenmiş ve daha az genel kapasiteye sahip.

### <a name="how-much-data-can-i-store-in-an-actor"></a>Ne kadar veri t bir aktör depolayabilir miyim?

Güvenilir hizmetleriyle gibi bir aktör hizmetinde depoladığınız veri miktarı yalnızca kullanılabilir bellek ve toplam disk alanı, kümedeki düğümler arasında sınırlanır. Ancak, durumu ve ilişkili iş mantığı az miktarda kapsüllemek için kullanıldığında tek tek aktörler en etkili. Genel kural olarak, tek bir aktör kilobayt cinsinden ölçülen durumu olması gerekir.

## <a name="other-questions"></a>Diğer sorular

### <a name="how-does-service-fabric-relate-to-containers"></a>Service Fabric kapsayıcılara nasıl ilişkilidir?

Tutarlı bir şekilde tüm ortamlarda çalıştırın ve tek bir makinede yalıtılmış bir şekilde işleyebilir şekilde kapsayıcıları paketi hizmetlerine ve bağımlılıklarını basit bir yol sunar. Service Fabric dağıtma ve yönetme gibi hizmetler için bir yol sunar [bir kapsayıcıda paketlenmiş Hizmetleri](service-fabric-containers-overview.md).

### <a name="are-you-planning-to-open-source-service-fabric"></a>Açık kaynak Service Fabric planlıyor musunuz?

Service Fabric açık kaynaklıdır bölümlerini sahibiz ([güvenilir hizmetler framework](https://github.com/Azure/service-fabric-services-and-actors-dotnet), [güvenilir aktörler framework](https://github.com/Azure/service-fabric-services-and-actors-dotnet), [ASP.NET Core tümleştirme kitaplıkları](https://github.com/Azure/service-fabric-aspnetcore), [ Service Fabric Explorer](https://github.com/Azure/service-fabric-explorer), ve [Service Fabric CLI](https://github.com/Azure/service-fabric-cli)) github'da ve topluluk katkılarına bu projelerine kabul edin. 

Biz [son duyurdu](https://blogs.msdn.microsoft.com/azureservicefabric/2018/03/14/service-fabric-is-going-open-source/) biz açık kaynaklı Service Fabric çalışma zamanını planlayın. Bu noktada sahibiz [Service Fabric depodaki](https://github.com/Microsoft/service-fabric/) kadar Linux github'da yapı ve test araçları, hangi depoyu kopyalama, Service Fabric Linux için derleme, temel testleri çalıştırmak, açabilir sorunları ve çekme istekleri gönderme anlamına gelir. Katı derleme ortamı üzerinden de yanı sıra eksiksiz bir CI ortamı geçirilen Windows almak için çalışıyoruz.

İzleyin [Service Fabric blog](https://blogs.msdn.microsoft.com/azureservicefabric/) duyurdu gibi daha fazla ayrıntı için.

## <a name="next-steps"></a>Sonraki adımlar

- [Service Fabric kavramları ve en iyi uygulamalar hakkında bilgi edinin](https://mva.microsoft.com/en-us/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965)
