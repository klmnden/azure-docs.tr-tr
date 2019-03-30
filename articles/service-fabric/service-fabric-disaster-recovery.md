---
title: Azure Service Fabric olağanüstü durum kurtarma | Microsoft Docs
description: Azure Service Fabric felaketler tüm türleri ile dağıtılacak gerekli özellikleri sunar. Bu makalede ortaya çıkabilecek felaketler ve bunlarla nasıl türlerini açıklar.
services: service-fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 0804095a9e12e91d6b0fa88b626b006b78bdf3a5
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58670821"
---
# <a name="disaster-recovery-in-azure-service-fabric"></a>Azure Service fabric'te olağanüstü durum kurtarma
Yüksek kullanılabilirlik sunmak için kritik bir parçası Hizmetleri tüm farklı türde hataları hayatta kalamaz sağlamaktır. Bu planlanmamış hataları için özellikle önemlidir ve denetiminiz dışında. Bu makalede olağanüstü durumlar olabilir değilse modellenir ve doğru bir şekilde yönetilen bazı yaygın hata modlarını açıklanır. Bu da ele risk azaltma işlemleri ve yine de olağanüstü bir durum oluştuysa gerçekleştirilecek eylemler. Sınırlamak veya aksi halde ortaya ortaya çıkan hataları, planlı kapalı kalma süresi veya veri kaybı riskini ortadan kaldırmak için hedeftir.

## <a name="avoiding-disaster"></a>Olağanüstü durum kaçınma
Service Fabric'in birincil ortamınızı hem hizmetlerinizi yaygın hata türleri olağanüstü olmayan şekilde model yardımcı olmaktır. 

Genel olarak olağanüstü durum/hata senaryoları iki tür vardır:

1. Donanım veya yazılım hatası
2. İşletimsel hataları

### <a name="hardware-and-software-faults"></a>Donanım ve yazılım hataları
Donanım ve yazılım hataları tahmin edilemez. Hataları hayatta kalmak için en kolay yolu, donanım veya yazılım hatası sınırları arasında dağıtılmış hizmet fazla kopyasını çalışıyor. Örneğin, hizmetinize belirli olan yalnızca bir makinede çalışıyorsa, hata, bir makinenin hizmet için bir olağanüstü durum olur. Bu olağanüstü durum önlemenin en basit yolu, hizmetin birden fazla makinede aslında çalışmadığını sağlamaktır. Test ayrıca bir makine hatasını çalışan hizmetin kesintiye değil emin olmak gereklidir. Kapasite planlaması, başka bir değiştirme örneği oluşturulabilir ve kapasite azaltma kalan hizmet aşırı değil sağlar. Aynı düzeni ne hatasını önlemek çalıştığınız bağımsız olarak çalışır. Örneğin. bir SAN hatasını endişeleriniz varsa, birden çok SAN çalıştırın. Bir sunucu rafı kaybı endişeleriniz varsa, birden çok raf arasında çalıştırın. Veri kaybı hakkında endişeleniyoruz, hizmetinizin Azure bölgeleri veya veri merkezleri arasında çalıştırmanız gerekir. 

Dağıtılmış modu bu tür içinde çalışırken hala bazı eşzamanlı hataları, ancak tek tür ve belirli bir türün bile hataları tabi olduğunuz (örn: tek bir VM veya ağ bağlantısı başarısız) otomatik olarak işlenir (ve bu nedenle artık olağanüstü bir "Durum"). Service Fabric kümesi genişletmek için birçok mekanizma sağlar ve düğümleri ve Hizmetleri geri getirme işler başarısız oldu. Service Fabric, bu tür planlanmamış gerçek felaketler oturum açma hatalarından kaçınmak için hizmetlerinizi pek çok örneğini çalıştıran de sağlar.

Neden hataları yayılan kadar büyük bir dağıtımın çalışıyor uygun olmadığı nedeniyle olabilir. Örneğin, daha fazla donanım kaynakları hatası olasılığını göre ödeme yapılacaktır daha beklemeniz gerekebilir. Dağıtılmış uygulamalar ile ilgilenirken, coğrafi uzaklıkları nedenleri arasında kabul edilemez bir gecikmeye ek iletişim atlama veya durum çoğaltma maliyeti olabilir. Burada bu çizgi çizilir her uygulama için farklıdır. Yazılım hataları için özellikle ölçeklendirme çalıştığınız hizmetinde hata olabilir. Bu durumda tüm örneklerinde hata durumu bağıntılı olduğundan kopya olağanüstü durum engellemez.

### <a name="operational-faults"></a>İşletimsel hataları
Hizmetiniz dünya çapındaki çok sayıda fazlalıkları arasında dağıtılmış olsa bile, yine de çok kötü sonuçlar olayları oluşabilir. Örneğin, birisi yanlışlıkla hizmetin dns adı yeniden yapılandırır ya da tamamen siler. Örneğin, durum bilgisi olan bir Service Fabric hizmeti olan ve biri hizmet yanlışlıkla silinirse varsayalım. Diğer bazı risk azaltma olmadığı sürece bu hizmet ve tüm önceki durumunda olduğundan artık kaybedildi. Bu tür bir işletimsel felaketler ("HAY aksi") farklı risk azaltma işlemleri ve adımları normal planlanmayan hatalar daha kurtarma gerektirir. 

Bu tür bir işlem hataları önlemek için en iyi yöntemleri
1. ortama işletimsel erişimi kısıtlama
2. kesin olarak tehlikeli işlemleri denetleme
3. Otomasyon koymak, el ile veya bant dışı değişiklikleri dışında önlemeye ve kabul etme önce gerçek ortama yönelik belirli değişiklikleri doğrulama
4. zararlı işlemler "soft" olduğundan emin olun. Yazılım işlemleri hemen etkili yok veya bazı zaman penceresi içinde geri alınabilir

Service Fabric sağlamak gibi işlemsel hatalar engellemek için bazı mekanizmalar sağlar [rol tabanlı](service-fabric-cluster-security-roles.md) erişim denetimi küme işlemleri için. Ancak operasyonel bu hataların çoğu kuruluş çabaları ve diğer sistemler gerektirir. Service Fabric, işletimsel hataları, özellikle yedekleme geri kalan bazı mekanizma sağlar ve durum bilgisi olan hizmetler için geri yükleyin.

## <a name="managing-failures"></a>Hataları yönetme
Service Fabric neredeyse her zaman hata otomatik yönetim hedefidir. Ancak, bazı türleri hataları işlemek için Hizmetler ek kod olmalıdır. Diğer hata türleri gereken _değil_ otomatik olarak güvenliği ve iş sürekliliği sebeplerden dolayı ele. 

### <a name="handling-single-failures"></a>Tek hatalarını işleme
Tek tek makineler için çok çeşitli nedenlerle başarısız olabilir. Bunlardan bazıları güç kaynakları ve donanım hatalarına ağ gibi donanım nedenler şunlardır. Diğer hatalar yazılımlardır. Bunlar gerçek işletim sistemi ve hizmet hataları içerir. Service Fabric, bu tür hataları, çalışmaları nereye makine ağ sorunları nedeniyle diğer makinelerden yalıtılmış olur dahil olmak üzere otomatik olarak algılar.

Kodunu tek bir kopyasını, herhangi bir nedenle başarısız olursa hizmetin türü ne olursa olsun, bu hizmet için kapalı kalma süresi sonuçları tek bir örneği çalışıyor. 

Herhangi bir tek hata işlemek için yapabileceğiniz en basit şeyi hizmetlerinizi varsayılan olarak birden fazla düğümde çalışır olduğundan emin olmaktır. Durum bilgisi olmayan hizmetler için bu sağlayarak gerçekleştirilebilir bir `InstanceCount` 1'den büyük. Durum bilgisi olan hizmetler için her zaman en az kullanılması önerilir bir `TargetReplicaSetSize` ve `MinReplicaSetSize` en az 3. Daha fazla kopyasını hizmet kodunuzu çalıştıran hizmetinizi herhangi bir tek hata otomatik olarak ele almasını sağlar. 

### <a name="handling-coordinated-failures"></a>Eşgüdümlü işleme hataları
Eşgüdümlü hataları nedeniyle ya da planlanan bir küme veya planlanmamış altyapı hataları ve değişiklikleri veya planlı yazılım değişiklikleri oluşabilir. Service Fabric, hata etki alanları olarak Eşgüdümlü hatalar yaşansa altyapı bölgeleri modeller. Eşgüdümlü yazılım değişiklikleri yaşar alanlarını, yükseltme etki alanları modellenir. Hata ve yükseltme etki alanları hakkında daha fazla bilgi yer [bu belgeyi](service-fabric-cluster-resource-manager-cluster-description.md) küme topolojisi ve tanımını açıklar.

Varsayılan olarak Service Fabric hata ve yükseltme etki alanları hizmetlerinizi çalıştırdığı planlarken göz önünde bulundurur. Varsayılan olarak, Service Fabric, hizmetlerinizi planlanmış veya planlanmamış değişiklikleri görülüyorsa hizmetlerinizi kullanılabilir kalması için birden çok hata ve yükseltme etki alanları arasında çalıştığından emin olmak çalışır. 

Örneğin, hata güç kaynağı raf makinelerin aynı anda başarısız olmasına neden olduğunu varsayalım. Hata etki alanı hatası çok sayıda makineyi kaybı çalışan hizmeti birden çok kopyasını ile başka bir örneği için belirli bir hizmete tek hatası dönüştürür. Hata etki alanlarını yönetme hizmetlerinizin yüksek kullanılabilirliğini sağlamak için önemli olmasının nedenidir. Service Fabric, Azure'da çalıştırırken, hata etki alanları otomatik olarak yönetilir. Diğer ortamlarda oldukları olmayabilir. Şirket içi kendi kümeleri oluşturuyorsanız, eşleme ve hata etki alanı düzeninizi doğru planı emin olun.

Yükseltme etki alanları aynı anda yükseltilmesi için yazılım nerede bulunacağını modellemek için kullanışlıdır. Bu nedenle, yükseltme etki alanları da genellikle yazılım burada planlanmış yükseltme sırasında yapılan sınırlarını tanımlayın. Service Fabric ve hizmetlerinizin yükseltmeleri aynı modelin izleyin. Yükseltmeler, yükseltme etki alanları ve istenmeyen değişiklikleri küme ve hizmetinizi etkilemesini engeller Service Fabric sistem durumu modeli hakkında daha fazla bilgi için bu belgelere bakın:

 - [Uygulama yükseltme](service-fabric-application-upgrade.md)
 - [Uygulama yükseltme Öğreticisi](service-fabric-application-upgrade-tutorial.md)
 - [Service Fabric sistem durumu modeli](service-fabric-health-introduction.md)

Sağlanan küme eşlemesi kullanarak kümenize düzenini görselleştirebilirsiniz [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md):

<center>

![Service Fabric Explorer'da hata etki düğümler paylaştırın][sfx-cluster-map]
</center>

> [!NOTE]
> Hizmet kodu ve durum, pek çok örneğini çalıştıran çalışırken hatasının alanları modelleme hizmetlerinizi emin olmak için yerleştirme kuralları hata ve yükseltme etki alanları arasında çalıştırmanız, ve yerleşik sistem durumu izleme yalnızca **bazı** , normal çalışma sorunlarını ve hataları felaketler oturum açma tutmak için Service Fabric sağladığı özellikleri. 
>

### <a name="handling-simultaneous-hardware-or-software-failures"></a>Eşzamanlı donanım veya yazılım hatalarını işleme
Yukarıda tek hataları hakkında konuştuk. Gördüğünüz gibi durum bilgisiz ve durum bilgisi olan hizmetler için hata ve yükseltme etki alanları arasında çalışan kodu (ve durumu) daha fazla kopyasını tutarak işlemek kolayca uygulanabilir. Birden çok eşzamanlı rastgele hatalara da oluşabilir. Bunlar için gerçek bir olağanüstü müşteri adayı olasılığı daha yüksektir.


### <a name="random-failures-leading-to-service-failures"></a>Hizmet hataları için önde gelen rastgele hatalara
Hizmet olduğunu düşünelim bir `InstanceCount` 5 ve birkaç düğümlerinin çalışmakta olan tüm bu örnekler aynı anda başarısız oldu. Service Fabric, diğer düğümlerde değiştirme örnekleri otomatik olarak oluşturarak yanıt verir. Bu, hizmet istenen örnek sayısına kadar değişiklik oluşturma devam edecektir. Başka bir örnek olarak, bir durum bilgisi olmayan hizmet ile oluştu diyelim ki bir `InstanceCount`-1, geçerli tüm küme düğümleri üzerinde çalıştığı anlamına gelir. Bu örneklere bazıları başarısız olduğunu varsayalım. Bu durumda, Service Fabric hizmeti, istenen durumda değil ve eksik olduğu düğümlerinde örnekleri oluşturmak çalışır fark eder. 

Durum bilgisi olan hizmetler için durum olup hizmet durumu veya kalıcı üzerinde bağlıdır. Ayrıca bir şekline göre değişir çok sayıda çoğaltma hizmeti ve kaç tanesinin başarısız. Bir durum bilgisi olan hizmet için olağanüstü bir durum oluştu ve yönetmeyi izleyen üç aşaması belirleme:

1. Olmuştur, çekirdek kaybı olmadığını belirleme
   - Çekirdek kayıp bir durum bilgisi olan hizmet çoğaltmalarının etkinleştirildiklerinde aşağı aynı zamanda, birincil dahil olmak üzere herhangi bir zamandır.
2. Çekirdek kayıp kalıcı olup olmadığını belirleme
   - Çoğu zaman, hatalar geçicidir. İşlemler başlatılır, düğümlerin yeniden, Vm'leri yeniden, ağ bölümlerinden onarımı. Ancak bazen hatalar kalıcı olur. 
     - Hizmetleri olmadan kalıcı için durum, bir çekirdek ya da daha fazla yineleme sonuçları hata _hemen_ kalıcı çekirdek kaybına. Service Fabric durum bilgisi olan kalıcı olmayan hizmet çekirdek kayıp algıladığında, (olası) veri kaybı bildirerek hemen 3. adıma geçer. Service Fabric, kurtarılır olsa bile kullanıcılar boş olacağından, çoğaltmaları geri gelmesi bekleniyor hiçbir noktası olduğunu bildiğinden etmeden veri kaybı mantıklıdır.
     - Durum bilgisi olan kalıcı hizmetler için bir çekirdek ya da daha fazla çoğaltma hatası çoğaltmalarını geri dönün ve çekirdek geri yüklemek bekleyen başlatmak Service Fabric neden olur. Bu bir hizmet kesintisi için sonuçları _Yazar_ hizmet etkilenen bölüm (veya "çoğaltma kümesi"). Ancak, okuma hala azaltılmış tutarlılık Garantisi ile mümkün olabilir. Devam etmeden (olası) bir veri kaybı olayıdır ve diğer riskleri taşır geri yüklenmesi, çekirdek için Service Fabric bekler varsayılan süreyi sonsuzdur. Varsayılan geçersiz kılma `QuorumLossWaitDuration` değeri mümkündür, ancak önerilmez. Bunun yerine şu anda tüm çalışmalarını aşağı çoğaltmaları geri yüklemek için yapılmalıdır. Bu, yedekleme basılı olan düğümleri getiren ve bunlar yerel kalıcı durumunu depolandığı sürücüleri yeniden bağlamak, sağlama gerektirir. Çekirdek kayıp işlem hatasından kaynaklanır, Service Fabric işlemleri yeniden oluşturun ve bunların içindeki çoğaltmalar yeniden başlatmak otomatik olarak çalışır. Bu başarısız olursa Service Fabric sistem durumu hataları bildirir. Bunlar çözümlenebilir, ardından çoğaltmaların genellikle geri dönün. Bazı durumlarda, yine de çoğaltmaları yeniden yapılamıyor. Örneğin, sürücüleri tüm kaydedilememiş olabilir ya da fiziksel makineler şekilde yok. Bu durumlarda, artık bir kalıcı çekirdek kaybı olayından sahibiz. Geri dönmeniz için aşağı çoğaltmaları beklemeyi durdurmak için Service Fabric bildirmek için bir Küme Yöneticisi, hizmetleri etkilenir ve çağrı hangi bölümlerin belirlemelisiniz `Repair-ServiceFabricPartition -PartitionId` veya ` System.Fabric.FabricClient.ClusterManagementClient.RecoverPartitionAsync(Guid partitionId)` API.  Bu API, QuorumLoss dışında ve olası dataloss içine taşımak için bölüm Kimliğini belirtme sağlar.

   > [!NOTE]
   > Bu _hiçbir zaman_ güvenli dışında bu API belirli bölümleri karşı hedeflenmiş bir yol kullanın. 
   >

3. Gerçek veri kaybı durumunda belirleme ve yedeklerden geri yükleme
   - Service Fabric çağırdığında `OnDataLossAsync` yöntemi, her zaman nedeniyle parçası olduğu _şüpheli_ veri kaybı. Service Fabric, bu çağrı için teslim edilmesini sağlar _en iyi_ kalan çoğaltma. Hangi çoğaltma çoğu ilerleme yaptığını budur. Bunun nedeni her zaman dediğimiz _şüpheli_ veri kaybı olan birincil gittiği zaman yaptığınız gibi kalan çoğaltmayı gerçekten tüm aynı duruma sahip mümkün olduğunu. Ancak, kendisine karşılaştırmak için bu durum, Service Fabric veya işleçleri emin bilmek için iyi bir yolu yoktur. Bu noktada, Service Fabric ayrıca diğer çoğaltmaları yeniden geliyor değil bilir. Biz kendiliğinden çekirdek kayıp bekleniyor durduğunda yapılan kararının. Eylem hizmeti için en iyi yol genellikle dondurma ve belirli yönetim müdahalesi için bekleyin kullanmamaktır. Tipik bir uygulaması, bu nedenle yaptığı `OnDataLossAsync` yöntemi musunuz?
   - İlk olarak, oturum `OnDataLossAsync` tetiklendiğini ve yangın gerekli yönetim uyarılarını kapatın.
   - Duraklatma ve gerçekleştirilecek başka kararlarını ve el ile gerçekleştirilen eylemler için beklemesi için genellikle bu noktada. Yedeklemeleri kullanılabilir olsa bile, bunlar hazırlanması gerekebilir olmasıdır. Örneğin, iki farklı hizmetler bilgi koordine etmek, bu yedekler, geri yükleme gerçekleştikten sonra bu iki hizmet çok değer verdiğiniz bilgileri tutarlı olduğundan emin olmak için değiştirilmesi gerekebilir. 
   - Genellikle da diğer bazı telemetri veya yoktur Egzozu hizmetinden. Bu meta veri günlükleri veya diğer hizmetler içeriyor. Bu bilgiler, yedeklemede mevcut değil veya bu belirli çoğaltmaya çoğaltılır birincil işlenir çağrıları olup olmadığını belirlemek için gereken kullanılabilir. Bu durumdayken ya da yedekleme geri yükleme uygulanabilirdir önce eklenmesi gerekebilir.  
   - Kullanılabilen tüm yedeklemelerin yer alan kalan yinelemenin durumuna karşılaştırmalar. Araçlar ve bunu yapmak için kullanılabilir işlemleri vardır, Service Fabric güvenilir koleksiyonlar kullanarak, açıklanan [bu makalede](service-fabric-reliable-services-backup-restore.md). Çoğaltma durumunun yeterli ya da, hangi yedek eksik olabilir görmek için hedeftir.
   - Karşılaştırma yaptıktan ve gerekli, geri yükleme işlemi tamamlandı hizmet kodu herhangi bir durum değişiklik yapılmışsa true olarak döndürülmelidir. Çoğaltma durumunun iyi kopyalama olan ve hiçbir değişiklik belirlerse false döndürün. TRUE gösterir herhangi bir _diğer_ çoğaltmaları kalan artık olabilir ile tutarsız. Bunlar bırakılacak ve bu çoğaltmadan yeniden. Diğer çoğaltmaları ne sahip oldukları tutacak yanlış durum değişiklik yapıldığını gösterir. 

Hizmet yazarlar Hizmetleri hiç olmadığı kadar üretime dağıtılmadan önce olası veri kaybı ve hata senaryoları uygulama öneme. Veri kaybı olasılığı karşı korumak için düzenli aralıklarla önemlidir [durumu yedekleme](service-fabric-reliable-services-backup-restore.md) herhangi bir coğrafi olarak yedekli depolama, durum bilgisi olan hizmetler. Ayrıca, geri olanağına sahip emin olmalısınız. Yedeklemeler, birçok farklı hizmetler farklı zamanlarda alındığından, bir geri yüklemeden sonra hizmetlerinizi birbiriyle tutarlı bir görünüm olduğundan emin olmak gerekir. Örneğin, burada bir hizmet bir sayı oluşturur ve bunu depolar ve ardından da depolayan başka bir hizmete gönderir, bir durum düşünün. Bir geri yüklemeden sonra ikinci bir hizmet numarasına sahip ancak ilk yoksa bu işlem eklemediğiniz, bu yedekleme olduğundan, fark edebilirsiniz.

Kalan çoğaltmalar gelen bir veri kaybı senaryosunda devam etmek yeterli değil ve hizmet durumunu telemetri veya Egzozu yeniden yapılandırma öğrenmek, yedekleme sıklığını, en iyi olası kurtarma noktası hedefi (RPO) belirler. Service Fabric, kalıcı çekirdek ve veri kaybı gerektiren bir yedekten geri yükleme dahil olmak üzere çeşitli hata senaryolarını test etmek için birçok araç sağlar. Bu senaryolar, Service Fabric'in Test Edilebilirlik Araçlar, hata analizi hizmeti tarafından yönetilen bir parçası olarak dahil edilir. Bu araçlar ve desenleri hakkında daha fazla bilgi edinilebilir [burada](service-fabric-testability-overview.md). 

> [!NOTE]
> Sistem Hizmetleri, söz konusu hizmete özgü olan etkisi olan çekirdek kayıp, aynı zamanda olumsuz etkilenebilir. Örneği için çekirdek kaybına Yük Devretme Yöneticisi hizmeti yeni hizmet oluşturma ve yük devretme işlemleri engeller ancak ad çözümlemesi, adlandırma hizmeti çekirdek kaybına etkiler. Service Fabric sistem hizmetlerinin, Durum Yönetimi Hizmetleri olarak aynı deseni takip ederken çekirdek kayıp dışında ve olası veri kaybı taşıma denemesi önerilmez. Bunun yerine önerilir [destek arama](service-fabric-support.md) mevcut durumunuzu hedeflenen bir çözümü belirlemek için.  Genellikle yalnızca aşağı çoğaltmaları dönene kadar beklemek tercih edilir.
>

## <a name="availability-of-the-service-fabric-cluster"></a>Service Fabric kümesinin kullanılabilirlik
Hiçbir tek hata noktaları ile Service Fabric kümesi genel olarak bakıldığında, yüksek oranda dağıtılmış bir ortamdır. Öncelikle Service Fabric sistem hizmetlerinin aynı daha önce sağlanan yönergeleri izleyin olduğundan herhangi bir düğüm hatası kullanılabilirlik ve güvenilirlik sorunları küme için neden olmaz: Bunlar üç veya daha fazla çoğaltma ile varsayılan ve bu sistemi tarafından her zaman çalıştır durum bilgisi olmayan hizmetler, tüm düğümler üzerinde çalıştırın. Service Fabric ağ ve hata algılama katmanlarda tam olarak dağıtılır. Çoğu Sistem Hizmetleri kümedeki meta verilerden yeniden veya başka yerlerden durumlarına yeniden eşitlemek biliyorsunuz. Kullanılabilirlik kümesinin Sistem Hizmetleri Çekirdek kayıp durumları açıklananlar gibi yukarıdaki alırsanız gizliliği. Bu gibi durumlarda, bir yükseltmeye başlatmadan veya yeni hizmetlerin dağıtımı gibi küme üzerinde belirli işlemlerin gerçekleştirilmesi mümkün olmayabilir, ancak küme hala çalışıyor. Bunlar çalışmaya devam edebilmesi için Sistem Hizmetleri yazmaya gereksinim duymadıkça hizmetleri çalıştırma zaten bu koşulları çalışmaya devam eder. Örneğin, çekirdek kaybında Yük Devretme Yöneticisi, tüm hizmetlerin çalışmaya devam eder, ancak başarısız olan hizmetler bu Yük Devretme Yöneticisi'nin katılımı gerektirdiğinden otomatik olarak, yeniden başlatmanız mümkün olmayacaktır. 

### <a name="failures-of-a-datacenter-or-azure-region"></a>Bir veri merkezine veya Azure bölgesi hataları
Nadiren de olsa bir fiziksel veri merkezinde güç veya ağ bağlantısı kaybı nedeniyle geçici olarak devre dışı hale gelebilir. Bu durumlarda, Service Fabric kümeleri ve Hizmetleri, veri merkezi veya Azure bölgesi içinde kullanılamaz. Ancak, _verileriniz korunur_. Azure'da çalışan kümeler için üzerinde kesintiler güncelleştirmeleri görüntüleyebilirsiniz [Azure durum sayfasında][azure-status-dashboard]. Düşüktür olayda bir fiziksel veri merkezinde kısmen veya tamamen olduğunu yok, yok barındırılan herhangi bir Service Fabric kümeleri veya bunların içindeki Hizmetleri kayıp olabilir. Bu, söz konusu veri merkezinin veya dışında yedeklenmeyen herhangi bir durumu içerir.

Tek bir veri merkezinin veya bölgenin kalıcı veya sürekli hata geri kalan için iki farklı strateji vardır. 

1. Ayrı bir Service Fabric kümeleri gibi birden çok bölgede çalıştırın ve yük devretme ve geri dönmeyi bu ortamlar arasında bazı mekanizması kullanır. Bu tür bir çoklu küme aktif-aktif veya Aktif-Pasif modeli, ek kod yönetimi ve işlemleri gerektirir. Bu da, bunlar diğer veri merkezinde veya bölgede bir kullanılabilir olacak şekilde bir veri merkezinin veya hizmetler yedeklerden eşgüdümünü başarısız gerektirir. 
2. Birden çok veri merkezleri ve bölgeleri kapsayan tek bir Service Fabric kümesi çalıştırmak. Bunun için desteklenen minimum yapılandırma üç veri merkezleri ya da bölgelerdeki ' dir. Önerilen bölgeler ve veri merkezleri sayısı beştir. Bu, daha karmaşık bir küme topolojisi gerektirir. Ancak, bu modelin avantajı hata bir veri merkezinin veya bölgenin normal hata bir olağanüstü durumdan dönüştürülür olur. Bu hatalar, iş mekanizmalarını tek bir bölgede kümeleri tarafından işlenebilir. Hata etki alanlarını, yükseltme etki alanları ve Service Fabric'in yerleştirme kuralları, iş yükleri, böylece bunlar normal hataları tolere dağıtılan emin olun. Bu tür bir küme hizmetler yardımcı olan ilkeleri hakkında daha fazla bilgi için okumaya [yerleştirme ilkeleri](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)

### <a name="random-failures-leading-to-cluster-failures"></a>Küme hatalarını önde gelen rastgele hatalara
Service Fabric çekirdek düğümleri kavramı vardır. Temel alınan kümenin kullanılabilirliğini sürdürmek düğümleri şunlardır. Bu düğümler, diğer düğümlerle kiraları kurma ve belirli türdeki ağ hataları sırasında tiebreakers hizmet veren tarafından yedekleme kümesi kalmasını sağlamak için yardımcı olur. Çekirdek değer düğümü çekirdek ve küme başarısız kaybettiğinizde gibi rastgele hatalara kümede çekirdek düğümleri çoğunu kaldırmak ve geri getirilmez, ardından, küme Federasyon halkası daraltır. Azure'da Service Fabric kaynak sağlayıcısı Service Fabric küme yapılandırmalarını yönetir ve varsayılan olarak, birincil düğüm türü hata ve yükseltme etki alanları arasında çekirdek düğümleri dağıtır; Başka bir olmayan çekirdek değer düğümü kullanılabilir birincil nodetype yükseltmek küme ölçeklendirme, birincil nodetype veya el ile bir çekirdek değer düğümü kaldırmak, bir çekirdek değer düğümü kaldırdığınızda birincil nodetype Silver veya Gold dayanıklılık işaretlenmişse dener Kapasite ve birincil düğüm türünüz için kümenizin güvenilirlik düzeyi gerektirir daha az kullanılabilir kapasite varsa başarısız olur.

Tek başına Service Fabric kümeleri ve Azure içinde "Birincil düğüm türü", çekirdekler çalıştıran bilgisayardır. Birincil düğüm türü tanımlarken, Service Fabric otomatik olarak en fazla 9 çekirdek düğümleri ve sistem hizmetlerinin her biri 7 kopyalarını oluşturarak sağlanan düğüm sayısını avantajlarından yararlanın. Rastgele hatalara bir dizi sistem hizmet yinelemeler çoğunu aynı anda alırsa, yukarıda açıklanan Sistem Hizmetleri Çekirdek kayıp girer. Çekirdek düğümleri çoğunu kaybolması durumunda, kısa süre sonra kümeyi kapanır.

## <a name="next-steps"></a>Sonraki adımlar
- Kullanarak çeşitli hataların benzetimini gerçekleştirin öğrenin [Test Edilebilirlik framework](service-fabric-testability-overview.md)
- Olağanüstü durum kurtarma ve yüksek kullanılabilirlik diğer kaynakları okuyun. Microsoft, şu konularda rehberlik büyük miktarda yayımlamıştır. Bu belgelerin bazı diğer ürünleri kullanmak için belirli teknikler bakın, ancak bunlar birçok genel en iyi uygulamalar Service Fabric bağlamında de uygulayabilirsiniz içerir:
  - [Kullanılabilirlik denetim listesi](../best-practices-availability-checklist.md)
  - [Olağanüstü durum kurtarma tatbikatı gerçekleştirme](../sql-database/sql-database-disaster-recovery-drills.md)
  - [Olağanüstü durum kurtarma ve Azure uygulamaları için yüksek kullanılabilirlik][dr-ha-guide]
- [Service Fabric destek seçenekleri](service-fabric-support.md) hakkında bilgi edinin


<!-- External links -->

[repair-partition-ps]: https://msdn.microsoft.com/library/mt163522.aspx
[azure-status-dashboard]:https://azure.microsoft.com/status/
[azure-regions]: https://azure.microsoft.com/regions/
[dr-ha-guide]: https://msdn.microsoft.com/library/azure/dn251004.aspx


<!-- Images -->

[sfx-cluster-map]: ./media/service-fabric-disaster-recovery/sfx-clustermap.png
