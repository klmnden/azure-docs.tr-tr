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
ms.openlocfilehash: cc86a18b0db67bf968006c42f5791e1ad7a093f0
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51016710"
---
# <a name="commonly-asked-service-fabric-questions"></a>Sık sorulan sorular Service Fabric

Service Fabric neler yapabileceğinizi ve nasıl kullanılmalıdır hakkında pek çok sık sorulan soruların vardır. Bu belgede bu ortak sorularını ve yanıtlarını çoğunu kapsar.

## <a name="cluster-setup-and-management"></a>Küme kurulumu ve Yönetimi

### <a name="how-do-i-roll-back-my-service-fabric-cluster-certificate"></a>Nasıl Service Fabric kümesi Sertifikamı geri alma?

Geri alma, Service Fabric Küme çekirdeğini değişiklik yapmadan önce sistem durumu hata algılama uygulamanıza yükseltme gerektirir; Kaydettiğim değişiklikleri yalnızca ileri alınabilir. Yükseltme mühendisi ait Müşteri Destek Hizmetleri aracılığıyla gerekebilir kümenizi, kurtarılır izlenmeyen bir sertifika değişiklik sunulan durumunda.  [Service Fabric'in uygulama yükseltmesi](https://review.docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade?branch=master) geçerlidir [uygulama yükseltme parametreleri](https://review.docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-parameters?branch=master), ve sıfır kapalı kalma süresi yükseltme promise sunar.  İzlenen modu önerilen uygulamamızı yükseltme, otomatik güncelleştirme etki alanlarını ilerlemeyi durum denetimleri otomatik olarak varsayılan hizmet güncelleştirme geçen, sıralı geri başarısız temel alır.
 
Kümenizi hala Klasik Resource Manager şablonunuzu sertifika parmak izi özelliği yararlanarak, onu önerilir [ortak adı için sertifika parmak izi değişikliği kümeden](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-change-cert-thumbprint-to-cn), modern gizli dizileri yararlanmak için Yönetim Özellikleri.

### <a name="can-i-create-a-cluster-that-spans-multiple-azure-regions-or-my-own-datacenters"></a>Birden fazla Azure bölgesini veya kendi veri merkezlerini yayılan bir küme oluşturabilir miyim?

Evet. 

Service Fabric küme teknolojisini temel ağ bağlantısı birbirine sahip oldukları sürece dünyanın her yerde çalışan makineler birleştirmek için kullanılabilir. Ancak, derleme ve böyle bir kümeyi çalıştıran karmaşık olabilir.

Bu senaryoda ilgileniyorsanız, kişi ya da almak için öneriyoruz aracılığıyla [Service Fabric Github sorunlar listesinde](https://github.com/azure/service-fabric-issues) veya ek yönergeler almak için destek temsilciniz aracılığıyla. Service Fabric ekibi açıklığa, yönergeler ve öneriler bu senaryo için çalışmaktadır. 

Dikkate alınması gereken bazı noktalar şunlardır: 

1. Azure'da Service Fabric küme kaynağı Bugün, kümenin oluşturulmuş sanal makine ölçek kümeleri gibi bölgesel. Başka bir deyişle, bölgesel bir hata olması durumunda Azure Resource Manager veya Azure portalı ile küme yönetme olanağı kaybedebilir. Kümenin çalışmaya devam ve doğrudan etkileşim olacaktır olsa bile bu durum ortaya çıkabilir. Ayrıca, Azure bugün bölgede kullanılabilir tek bir sanal ağınız varsa olanağı sunmaz. Azure'da çok bölgeli küme ya da gerekir yani [VM ölçek kümesindeki her VM için genel IP adresleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-networking.md#public-ipv4-per-virtual-machine) veya [Azure VPN Gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md). Bu ağ seçenekleri maliyetler, performans, farklı etkiler ve bazı derece uygulama tasarımı için bu nedenle dikkatli analiz ve planlama gereklidir böyle bir ortam ayakta önce.
2. Bakım, yönetimi, ve bu makinelerin izleme dönüşebilir karmaşık, özellikle arasında dağıtılmış olduğunda _türleri_ ortamların, farklı bulut sağlayıcıları arasında veya şirket içi kaynaklar ile Azure arasında. Yükseltme, izleme, yönetim ve tanılama hem küme hem de uygulamalar için böyle bir ortamda üretim iş yükleri çalıştırmadan önce anlaşıldığından emin olmak için dikkatli olunması gerekir. Ardından, Azure'da veya kendi veri merkezlerinin içinde bu sorunları çözmek deneyimi zaten varsa, büyük olasılıkla aynı bu çözümleri oluşturmak veya Service Fabric kümenizi çalıştırdığınızda uygulanabilir. 

### <a name="do-service-fabric-nodes-automatically-receive-os-updates"></a>Service Fabric düğümleri otomatik olarak işletim sistemi güncelleştirmeleri alacak mıyım?

Kullanabileceğiniz [sanal makine ölçek kümesi otomatik işletim sistemi görüntüsü güncelleştirme](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade) bugün genel kullanıma açık özellik.

Azure'da çalıştırma kümeler için sahibiz [uygulama sağlanan](service-fabric-patch-orchestration-application.md) , Service Fabric düğümleri altındaki işletim sistemlerine yama yapma.

### <a name="can-i-use-large-virtual-machine-scale-sets-in-my-sf-cluster"></a>Büyük sanal makine ölçek kümeleri SF kümem kullanabilir miyim? 

**Kısa yanıt** : Hayır 

**Uzun yanıt** - büyük sanal makine ölçek kümeleri, bir sanal makine ölçek sağlar ancak ölçek kümesi en fazla 1000 VM örneklerine, yerleştirme grubuna (PGs) kullanarak bunu yapar. Hata etki alanları (FD) ve yükseltme etki alanları (UD) yalnızca bir yerleştirme grubu Service fabric kullanımlarını içinde FD ve Ud'ler hizmet çoğaltmaları/hizmet örneklerinin yerleştirme kararları vermek için tutarlı değil. FD ve Ud'ler bir yerleştirme grubu içinde yalnızca karşılaştırılabilir olduğundan, BT tarafından kullanılamaz. VM1, pg1'in bir FD topolojisi varsa, örneğin, = 0 ve VM9 PG2 içinde'bir FD topolojisi = 4, onu gelmez VM1 ve VM2 olan iki farklı donanım raflar, bu nedenle SF FD değerleri bu durumda yerleştirme kararları vermek için kullanamazsınız.

Büyük sanal makine ölçek kümeleri ile ilgili diğer sorunlar şu anda vardır, yük düzeyi 4 olmaması gibi Dengeleme desteği. İçin başvurmak [ayrıntıları büyük ölçek kümeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md)



### <a name="what-is-the-minimum-size-of-a-service-fabric-cluster-why-cant-it-be-smaller"></a>Bir Service Fabric kümesinin minimum boyutu nedir? Neden daha küçük olamaz?

Üretim iş yükleri çalıştıran bir Service Fabric kümesi için desteklenen minimum boyut beş düğüm ' dir. Geliştirme/test senaryoları için üç düğümlü küme destekliyoruz.

Service Fabric kümesine adlandırma hizmeti ve Yük Devretme Yöneticisi gibi bir sistem durum bilgisi olan hizmetler kümesi çalıştığı için bu alt sınır yok. Bu hizmetler, hangi hizmetlerin kümeye ve bunlar şu anda barındırıldığı, dağıtılan hangi izleme üzerinde güçlü tutarlılık bağlıdır. Bu güçlü tutarlılık sırayla alma yeteneğini bağlıdır bir *çekirdek* herhangi belirli bir güncelleştirme durumu için Hizmetleri, bir çekirdek belirli bir hizmete yönelik çoğaltmalar (N/2 + 1) katı çoğunu temsil ettiği için.

Bu bilgileri, bazı olası küme yapılandırmalarını inceleyelim:

**Bir düğüm**: herhangi bir nedenle anlamına gelir kaybı tüm küme için bu seçeneği yüksek kullanılabilirlik tek düğüm kaybı beri sağlamaz.

**İki düğüm**: iki düğümde dağıtılmış bir hizmet için bir çekirdek (N = 2) 2 (2/2 + 1 = 2). Tek bir çoğaltma kaybolursa, bir çekirdek oluşturmak mümkün değildir. Hizmet yükseltme gerçekleştirme geçici olarak bir çoğaltma sürüyor gerektirdiğinden, kullanışlı bir yapılandırma değil.

**Üç düğüm**: (N = 3) üç düğüm ile bir çekirdek oluşturmak için gereksinim yine de iki düğüm olması (3/2 + 1 = 2). Bu, tek bir düğüm kaybeder ve yine de yeterli çoğunluğu sürdürmek anlamına gelir.

Güvenli bir şekilde yükseltme gerçekleştirmek ve tek tek düğüm hatalara çünkü aynı anda sürece üç düğümlü küme yapılandırması dev/test için desteklenir. Beş düğüm gerekli olacak şekilde üretim iş yükleri için bu tür bir eş zamanlı hatasına dayanıklı olması gerekir.

### <a name="can-i-turn-off-my-cluster-at-nightweekends-to-save-costs"></a>Kümem gece/maliyet tasarrufu için hafta sonları kapatabilir miyim?

Genellikle yapamazsınız. Service Fabric, sanal makine farklı bir ana bilgisayara taşınırsa, verileri ile taşımaz, yani, yerel, kısa ömürlü disklerde durumunu depolar. Diğer düğümler tarafından yeni düğümün güncel duruma getirildikten normal işleminde, bir sorun değildir. Ancak, tüm düğümler durdurun ve daha sonra yeniden başlatmak, düğümlerin en yeni ana bilgisayar ve sistem kurtaramadı markasını baz alarak başlangıç önemli bir olasılık yoktur.

Dağıtmadan önce uygulamanızı test etme için kümeleri oluşturmak istiyorsanız, bu kümeleri dinamik olarak bir parçası olarak oluşturmanızı öneririz, [sürekli tümleştirme/sürekli dağıtım işlem hattı](service-fabric-tutorial-deploy-app-with-cicd-vsts.md).


### <a name="how-do-i-upgrade-my-operating-system-for-example-from-windows-server-2012-to-windows-server-2016"></a>İşletim sistemimin (örneğin Windows Server 2012'den Windows Server 2016) nasıl yükseltebilirim?

Geliştirilmiş bir deneyim üzerinde bugün çalışıyoruz ancak yükseltme için sorumlu olursunuz. Küme sanal makinelerde işletim sistemi görüntüsü yükseltmelisiniz birer birer VM. 

### <a name="can-i-encrypt-attached-data-disks-in-a-cluster-node-type-virtual-machine-scale-set"></a>Ben bir küme düğümü türü (sanal makine ölçek kümesi) bağlı veri diskleri şifreleyebilir mi?
Evet.  Daha fazla bilgi için [bağlı veri diskleri ile küme oluşturma](../virtual-machine-scale-sets/virtual-machine-scale-sets-attached-disks.md#create-a-service-fabric-cluster-with-attached-data-disks), [şifrelemek diskler (PowerShell)](../virtual-machine-scale-sets/virtual-machine-scale-sets-encrypt-disks-ps.md), ve [şifrelemek diskler (CLI)](../virtual-machine-scale-sets/virtual-machine-scale-sets-encrypt-disks-cli.md).

### <a name="can-i-use-low-priority-vms-in-a-cluster-node-type-virtual-machine-scale-set"></a>Düşük öncelikli VM'ler bir küme düğümü türü (sanal makine ölçek kümesi) kullanabilir miyim?
Hayır. Düşük öncelikli VM'ler desteklenmez. 

### <a name="what-are-the-directories-and-processes-that-i-need-to-exclude-when-running-an-anti-virus-program-in-my-cluster"></a>Dizinleri ve virüsten koruma programı kümem içinde çalışırken hariç gereken işlemler nelerdir?

| **Virüsten koruma hariç tutulan dizinler** |
| --- |
| Program Files\Microsoft Service Fabric |
| FabricDataRoot (küme yapılandırmasından) |
| FabricLogRoot (küme yapılandırmasından) |

| **Virüsten koruma dışlanan işlemler** |
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
 
### <a name="how-can-my-application-authenticate-to-keyvault-to-get-secrets"></a>Anahtar kasası için gizli dizileri almak üzere uygulamamı nasıl kimlik doğrulaması yapabilir?
Uygulamanız için KeyVault kimlik doğrulaması için kimlik bilgilerini almak için yol şunlardır:

A. Derleme/paketleme işlemi sırasında uygulamalarınızı, bir sertifika SF uygulamanızın veri pakete çekme ve için KeyVault kimlik doğrulaması için bunu kullanın.
B. MSI etkin konak sanal makine ölçek kümesi için bir basit PowerShell almak için SetupEntryPoint SF uygulamanız için geliştirebilirsiniz [MSI uç noktasından bir erişim belirteci](https://docs.microsoft.com/azure/active-directory/managed-service-identity/how-to-use-vm-token), ardından [KeyVault, gizli dizilerini alma](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Get-AzureKeyVaultSecret?view=azurermps-6.5.0)

## <a name="application-design"></a>Uygulama tasarımı

### <a name="whats-the-best-way-to-query-data-across-partitions-of-a-reliable-collection"></a>Güvenilir bir koleksiyonun bölümler arasında veri en iyi yolu nedir?

Güvenilir koleksiyonlar olan genellikle [bölümlenmiş](service-fabric-concepts-partitioning.md) ölçek genişletme için daha yüksek performans ve aktarım hızı sağlamak. Belirli bir hizmetin durumunu onlarca veya yüzlerce makine yaymak, anlamına gelir. Bu tam bir veri kümesi üzerinde işlemler gerçekleştirmek üzere birkaç seçeneğiniz vardır:

- Gerekli verileri çekmek için başka bir hizmetin tüm bölümleri sorgular bir hizmet oluşturursunuz.
- Başka bir hizmetin tüm bölümleri data alabilen bir hizmet oluşturursunuz.
- Düzenli aralıklarla veri her hizmetinden bir dış depoya gönderin. Bu yaklaşım, gerçekleştiriyorsunuz sorguları çekirdek İş mantığınızın bir parçası değilse uygundur.


### <a name="whats-the-best-way-to-query-data-across-my-actors"></a>My aktörler arasında veri en iyi yolu nedir?

Aktör, durum ve işlem, bağımsız bir birim için çalışma zamanında aktör durumunun geniş sorguları gerçekleştirmek için önerilmez olacak şekilde tasarlanmıştır. Aktör durumunun tam kümesi arasında sorgu için bir gereksinimi varsa, ya da dikkate almanız gerekir:

- Hizmet bölüm sayısı için aktör sayısından tüm veri toplamak üzere istekleri ağ sayısı, durum bilgisi olan reliable services özelliğiyle aktör hizmetlerinizi değiştiriliyor.
- Düzenli aralıklarla durumlarını daha kolay bir şekilde sorgulamak için bir dış mağazaya göndermek, aktör tasarlama. Yukarıdaki bu yaklaşım yalnızca gerçekleştirmekte sorguları, çalışma zamanı davranışı için gerekli değilse uygulanabilir gibidir.

### <a name="how-much-data-can-i-store-in-a-reliable-collection"></a>Ne kadar veri miyim güvenilir koleksiyonda depolayabilir miyim?

Kümedeki sahip makinelerin sayısı ve bu makinelerin kullanılabilir bellek miktarı, depolamanın miktarını yalnızca sınırlı şekilde güvenilir hizmetler genellikle bölümlenir.

Örnek olarak, bir güvenilir koleksiyon 100 bölüm ve 3 çoğaltma, bir hizmet olarak ortalama boyutu 1 KB'a nesneler depolanıyor olduğunu varsayın. Şimdi, 16 gb bellek makine başına 10 makine kümesiyle olduğunu varsayalım. Kolaylık olması için ve Pasif, olduğunu varsayar, bu küme için kullanılabilir makine başına 10 gb ve 100 gb bırakarak 6 gb işletim sistemi ve Sistem Hizmetleri, Service Fabric çalışma zamanı ve hizmetlerinizi kullanır.

Her bir nesne olması gereken göz önünde bulundurarak, depolanan üç (birincil ve iki çoğaltmalar), yaklaşık 35 milyon nesneler için yeterli bellek koleksiyonunuzda tam kapasitede çalışırken gerekir. Ancak, bir hata etki alanı ve yaklaşık 1/3 kapasiteye temsil eder ve yaklaşık 23 milyon azaltmak bir yükseltme etki eşzamanlı kaybına karşı dayanıklı olması önerilir.

Bu hesaplama ayrıca varsaydığını unutmayın:

- Bölümler arasında veri dağılımını kabaca Tekdüzen veya küme kaynak yöneticisi için yükleme ölçümleri raporlama. Varsayılan olarak, Service Fabric Bakiye yineleme sayısına göre yükler. Önceki örnekte, kümedeki her düğümde, 10 birincil çoğaltmalara ve 20 ikincil çoğaltmaları koyabilirsiniz. İyi bölümler arasında eşit olarak dağıtılmış yük için işe yarar. Yük bile değilse, kaynak yöneticisi daha küçük çoğaltmaları birlikte paketi ve daha büyük çoğaltmalar tek bir düğüm üzerinde daha fazla bellek kullanmasına izin ver yük bildirilmesi gerekir.

- Söz konusu güvenilir hizmet kümeyi yalnızca bir depolama durumda olduğunu. Birden çok hizmeti bir kümeye dağıtabilirsiniz olduğundan, kaynakları oluşturduğunu olmasına gerek her çalıştırın ve durumunu yönetmek gerekir.

- Küme büyüyen küçülterek veya yok. Daha fazla makine eklerseniz, Service Fabric çoğaltmalarınızın tek bir çoğaltma makineler yayılamaz bu yana makine sayısı, hizmet bölüm sayısı değerini geçiyor kadar ek kapasite yararlanmak için yeniden dengelemeniz. Aksine, makineleri kaldırarak ve kümenin boyutunu azaltın, çoğaltmalarınızın daha sıkı bir şekilde paketlenir ve daha az genel kapasiteye sahip.

### <a name="how-much-data-can-i-store-in-an-actor"></a>Ne kadar veri ben bir oyuncu depolayabilir miyim?

Reliable services gibi ile bir aktör hizmetinde depoladığınız veri miktarı yalnızca kullanılabilir bellek ve toplam disk alanı, kümenizdeki düğümler arasında sınırlıdır. Az miktarda durumu ve ilişkili iş mantığı kapsülleyen kullanıldığında ancak, tek tek en etkili birer aktördür. Genel kural olarak, tek bir aktör kilobayt cinsinden ölçülen durumu olmalıdır.

## <a name="other-questions"></a>Diğer sorular

### <a name="how-does-service-fabric-relate-to-containers"></a>Service Fabric kapsayıcıları için nasıl ilişkilidir?

Tutarlı bir şekilde tüm ortamlarda çalıştırabilirsiniz ve yalıtılmış bir şekilde tek bir makinede çalışabilen kapsayıcıları paket Hizmetleri ve onların bağımlılıkları için basit bir yol sunar. Service Fabric dağıtma ve yönetme gibi hizmetler için bir yol sunar [bir kapsayıcıda paketlenmiş Hizmetleri](service-fabric-containers-overview.md).

### <a name="are-you-planning-to-open-source-service-fabric"></a>Açık kaynak Service Fabric'e planlıyor musunuz?

Service Fabric açık kaynaklı bölümlerini sahibiz ([güvenilir hizmetler framework](https://github.com/Azure/service-fabric-services-and-actors-dotnet), [reliable actors framework](https://github.com/Azure/service-fabric-services-and-actors-dotnet), [ASP.NET Core tümleştirme kitaplıkları](https://github.com/Azure/service-fabric-aspnetcore), [ Service Fabric Explorer](https://github.com/Azure/service-fabric-explorer), ve [Service Fabric CLI](https://github.com/Azure/service-fabric-cli)) github'da ve bu projelerde topluluk katkılarına kabul edin. 

Biz [kısa süre önce duyurulan](https://blogs.msdn.microsoft.com/azureservicefabric/2018/03/14/service-fabric-is-going-open-source/) biz açık kaynaklı Service Fabric çalışma zamanını planlayın. Bu noktada sahibiz [Service Fabric deponuzu](https://github.com/Microsoft/service-fabric/) kadar GitHub ile Linux üzerinde derleyip test etmeye araçları, depoyu kopyalama, Service Fabric Linux için derleme, temel testleri çalıştırabilir, açık sorun ve çekme istekleri gönderme anlamına gelir. Yapı ortamı üzerinden de birlikte eksiksiz bir CI ortamı geçirilen Windows almak için çalışıyoruz.

İzleyin [Service Fabric blog](https://blogs.msdn.microsoft.com/azureservicefabric/) Duyurusu daha fazla ayrıntı için.

## <a name="next-steps"></a>Sonraki adımlar

- [Service Fabric kavramları ve en iyi uygulamalar hakkında bilgi edinin](https://mva.microsoft.com/en-us/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965)
