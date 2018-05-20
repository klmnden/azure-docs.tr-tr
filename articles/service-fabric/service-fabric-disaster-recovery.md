---
title: Azure Service Fabric olağanüstü durum kurtarma | Microsoft Docs
description: Azure Service Fabric afetler tüm türleri ile mücadele etmek gerekli özellikleri sunar. Bu makalede oluşabilir afetler ve bunları nasıl türleri açıklanmaktadır.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 295772b70529f79c7a4c135d8ea7c12a1c661fe6
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="disaster-recovery-in-azure-service-fabric"></a>Azure Service Fabric olağanüstü durum kurtarma
Yüksek kullanılabilirlik gerçekleştirirken önemli bir bölümü Hizmetleri hataları tüm farklı türlerde varlığını sürdürmesini olduğundan emin olmaktır. Bu, Planlanmayan hatalar için özellikle önemlidir ve denetiminizin dışında. Bu makalede afetler olabilir değilse modellenir ve doğru şekilde yönetilen bazı ortak hatası modları açıklanır. Bu da ele Azaltıcı Etkenler ve yine de bir olağanüstü durum oluştuysa, gerçekleştirilecek eylemler. Sınırlamaya veya planlanan hataları oluştuğunda kapalı kalma süresi veya veri kaybı riski ortadan kaldırmak veya aksi halde, ortaya hedeftir.

## <a name="avoiding-disaster"></a>Olağanüstü durum önleme
Service Fabric'ın birincil amacı, ortamınıza ve hizmetlerinizi genel hata türlerini afetler olmayan şekilde model yardımcı olmaktır. 

Genel olağanüstü durum/hatası senaryoları iki tür vardır:

1. Donanım veya yazılım hatası
2. İşletimsel hataları

### <a name="hardware-and-software-faults"></a>Donanım ve yazılım hataları
Donanım ve yazılım hataları öngörülemeyen sonuçlara yol açabilir. Hataları varlığını sürdürmesi için en kolay yolu fazla kopyasını donanım veya yazılım hatası sınırlarında yayılmış hizmeti çalışıyor. Örneğin, hizmetiniz belirli olan yalnızca bir makinede çalışıyorsa, bu bir makine başarısızlığını bu hizmet için bir olağanüstü durum olur. Bu olağanüstü durum önlemenin en basit yolu, hizmet birden çok makineye çalıştırdığını sağlamaktır. Sınama ayrıca bir makine başarısızlığını çalışan hizmetin kesintiye değil sağlamak gereklidir. Kapasite planlama değiştirme örneği başka bir yerde oluşturulabilir ve kapasite azalma kalan hizmet aşırı değil sağlar. Aynı desende ne, hatasını önlemek çalıştığınız bağımsız olarak çalışır. Örneğin. bir SAN hata hakkında ilgilenen, birden çok SAN çalıştırın. Bir sunucu rafı kaybı hakkında ilgilenen, birden çok kabin çalıştırın. Veri merkezleri kaybı hakkında endişeleniyoruz, hizmetiniz birden çok Azure bölgeleri veya veri merkezleri arasında çalıştırmanız gerekir. 

Bu dağıtılmış modu türünde çalıştırırken, hala eşzamanlı hataları, ancak tek bazı türleri ve belirli bir türdeki bile birden çok hatadan tabi olduğunuz (örn: tek bir VM veya ağ bağlantısı başarısız) otomatik olarak işlenir (ve bu nedenle artık bir "olağanüstü durum"). Service Fabric kümesi genişleterek birçok mekanizma sağlar ve düğümler ve Hizmetleri geri getirme işler başarısız oldu. Service Fabric de bu tür planlanmamış gerçek afetler oturum açma hatalarından kaçınmak için hizmetlerinizi birçok örneklerini çalıştırılmasını sağlar.

Neden hataları span yetecek büyüklükte bir dağıtımı çalıştıran uygun olmadığı nedenleri olabilir. Örneğin, hata olasılığını göre ödeme istekli çok daha fazla donanım kaynakları alabilir. Dağıtılmış uygulamaları ile ilgilenirken, ek iletişim atlama veya durum çoğaltma coğrafi uzaklıklar nedenler kabul edilebilir gecikme süresi arasında maliyetleri olabilir. Burada bu çizgi çizilir her uygulama için farklıdır. Yazılım hataları için özellikle ölçeklendirmek için çalıştığınız hizmetinde hataya olabilir. Bu durumda hata koşulu boyunca tüm örneklerde bağıntılı olduğundan daha fazla kopyaları olağanüstü durum engellemez.

### <a name="operational-faults"></a>İşletimsel hataları
Hizmetinizi birçok açarken ile dünya çapında yayılmış olsa bile, yine felaket niteliğinde olayları yaşayabilirsiniz. Örneğin, birisi yanlışlıkla hizmeti dns adı yeniden yapılandırır veya depolayabileceği siler. Örnek olarak, bir durum bilgisi olan Service Fabric hizmeti var ve biri hizmet yanlışlıkla silinirse varsayalım. Diğer bir azaltma sürece, bu hizmet ve tüm önceki durumunda olduğu şimdi gitti. Bu işlem olağanüstü türleri ("hata") farklı Azaltıcı Etkenler ve adımları kurtarma normal planlanmayan hatalar daha gerektirir. 

Bu tür işletimsel hataları önlemek için en iyi yöntemleri
1. ortama işletimsel erişimi kısıtlama
2. kesinlikle denetim tehlikeli işlemleri
3. Otomasyon zorunlu tuttukları, el ile veya bant dışı değişiklikleri önlemek ve bunları ederek ilerlemesini kabul ederek önce belirli değişiklikleri gerçek ortama karşı doğrulama
4. bozucu operations "yumuşak" olduğundan emin olun. Yazılım işlemleri hemen etkinleşmesini yok veya bazı zaman penceresi içinde geri alınabilir

Service Fabric sağlama gibi işletimsel hataları önlemek için bazı mekanizmalar sağlar [rol tabanlı](service-fabric-cluster-security-roles.md) erişim denetimi küme işlemleri için. Ancak, bu işlem hatalarının çoğu kuruluş çabalarına ve diğer sistemler gerektirir. Service Fabric işlem hataları, özellikle yedekleme geri kalan için bazı mekanizmaya ve durum bilgisi olan hizmetler için geri yükleyin.

## <a name="managing-failures"></a>Hataları yönetme
Service Fabric neredeyse her zaman otomatik yönetimi hatalar hedefidir. Ancak, bazı türleri hataları işlemek için Hizmetler ek kod olması gerekir. Hataları diğer türleri gereken _değil_ otomatik olarak ele güvenliği ve iş sürekliliği nedenleriyle. 

### <a name="handling-single-failures"></a>Tek hataları işleme
Tek makineler, her türlü nedenleri için başarısız olabilir. Bunlardan bazıları, güç kaynakları ve donanım hatalarına ağ gibi donanım nedenler şunlardır. Diğer yazılım hatalarıdır. Bunlar, gerçek işletim sistemi ve hizmet kesintilerine içerir. Service Fabric makinenin nerede ağ sorunları nedeniyle diğer makinelerden yalıtılmış hale durumlarda dahil hataları, bu tip otomatik olarak algılar.

Tek bir kopya kodunun herhangi bir nedenle başarısız olursa hizmet türü ne olursa olsun, bu hizmet için kapalı kalma süresi sonuçları tek bir örnek çalışıyor. 

Herhangi bir tek hata işlemek amacıyla bunu yapabilirsiniz basit hizmetlerinizi varsayılan olarak birden fazla düğüm üzerinde çalıştığından emin olmak için şeydir. Durum bilgisi olmayan hizmetler için bu sağlayarak gerçekleştirilebilir bir `InstanceCount` 1'den büyük. Durum bilgisi olan hizmetler için en düşük her zaman önerilir bir `TargetReplicaSetSize` ve `MinReplicaSetSize` en az 3. Daha fazla hizmet kodunuzun kopyalarını çalıştıran hizmetinizi herhangi bir tek hata otomatik olarak ele almasını sağlar. 

### <a name="handling-coordinated-failures"></a>Eşgüdümlü işleme hataları
Eşgüdümlü hataları nedeniyle ya da planlanan bir küme veya planlanmamış altyapı hataları ve değişiklikleri veya planlanan yazılım değişiklikleri oluşabilir. Service Fabric Eşgüdümlü hataları hata etki alanları olarak deneyimi altyapı bölgeleri modeller. Eşgüdümlü yazılım değişiklikleri yaşar alanları yükseltme etki alanları Modellenen. Hata ve yükseltme etki alanları hakkında daha fazla bilgi yer [bu belgeyi](service-fabric-cluster-resource-manager-cluster-description.md) küme topolojisi ve tanım açıklar.

Varsayılan olarak Service Fabric hata ve yükseltme etki alanı hizmetlerinizi çalıştırdığı planlarken göz önünde bulundurur. Varsayılan olarak, planlanmış veya planlanmamış değişiklikleri görülüyorsa hizmetlerinizin kullanılabilir olmaya devam etmesi için hizmetlerinizi birkaç hata ve yükseltme etki alanları arasında çalıştığından emin olmak Service Fabric dener. 

Örneğin, güç kaynağı başarısızlığın bir rafa aynı anda başarısız makinelerin nedenlerini varsayalım. Hata etki alanı hatası fazla makine kaybı çalışan hizmeti birden çok kopyası ile belirli bir hizmeti için tek hatası başka bir örneği dönüştürür. Hata etki alanlarını yönetme hizmetlerinizi yüksek kullanılabilirliğini sağlamak için önemli olan nedeni budur. Service Fabric Azure'da çalıştırırken, hata etki alanlarını otomatik olarak yönetilir. Diğer ortamlarda bunlar olmayabilir. Şirket içi kendi kümeleri oluşturuyorsanız, eşleme ve hata etki alanı düzeninizi doğru şekilde planlamak emin olun.

Yükseltme etki alanları aynı anda yükseltilmesi yazılım nerede bulunacağını modelleme için faydalıdır. Bu nedenle, yükseltme etki alanları da genellikle yazılım planlanmış yükseltme sırasında nerede alınır sınırlarını tanımlayın. Yükseltmeler Service Fabric ve hizmetlerinizi aynı modelin izleyin. Yükseltme, yükseltme etki alanları ve istenmeyen değişiklikleri küme ve hizmetinizi etkileyen engeller Service Fabric sistem durumu modeli alma hakkında daha fazla bilgi için bu belgelere bakın:

 - [Uygulama yükseltme](service-fabric-application-upgrade.md)
 - [Uygulama yükseltme Öğreticisi](service-fabric-application-upgrade-tutorial.md)
 - [Service Fabric sistem durumu modeli](service-fabric-health-introduction.md)

Sağlanan küme eşlemesi kullanarak kümenizi düzenini görselleştirebilirsiniz [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md):

<center>
![Service Fabric Explorer'da hata etki alanları arasında yayılan düğümler][sfx-cluster-map]
</center>

> [!NOTE]
> Çalışan hizmet kodu ve durum, çok sayıda örneklerini çalışırken hatanın alanları modelleme hizmetlerinizi emin olmak için yerleştirme kuralları hata ve yükseltme etki alanları arasında çalıştırın ve yalnızca yerleşik sistem durumu izleme olan **bazı** olağanüstü oturum açma gelen normal işletimsel sorunlar ve hatalar tutmak için Service Fabric özellikleri sağlar. 
>

### <a name="handling-simultaneous-hardware-or-software-failures"></a>Eşzamanlı donanım veya yazılım hataları işleme
Yukarıdaki tek hataları hakkında açıklandı. Gördüğünüz gibi durum bilgisiz ve durum bilgisi olan hizmetler için hata ve yükseltme etki alanları arasında çalışan kod (ve durumu) fazla kopyasını tutarak işlemek kolaydır. Birden çok eşzamanlı rastgele hatalara de oluşabilir. Bunlar için gerçek bir olağanüstü durum sağlama olasılığı daha yüksektir.


### <a name="random-failures-leading-to-service-failures"></a>Hizmet hataları için önde gelen rasgele hataları
Hizmet olduğunu düşünelim bir `InstanceCount` 5 ve birkaç düğümlerinin tüm bu örneklerde çalıştıran aynı anda başarısız oldu. Service Fabric değiştirme örnekleri otomatik olarak diğer düğümlerde oluşturarak yanıt verir. Hizmet istenen örnek sayısına kadar değişiklik oluşturma devam eder. Başka bir örnek olarak, bir durum bilgisiz hizmetiyle vardı diyelim ki bir `InstanceCount`-1, geçerli tüm düğümleri üzerinde çalıştığı anlamına gelir. Bu örneklerde bazıları başarısız olduğunu düşünelim. Bu durumda, Service Fabric hizmeti istenilen durumda değil ve eksik olduğu düğümlerinde örnekleri oluşturmayı dener fark eder. 

Durum bilgisi olan hizmetler için olup hizmet durumu veya kalıcı üzerinde durumunuza bağlıdır. Ayrıca nasıl bağlı olduğu hizmeti yaşadı birçok çoğaltmaları ve kaç tanesinin başarısız. Bir olağanüstü durum bilgisi olan bir hizmet için oluştu ve yönetmeyi üç aşamanın izleyen belirleme:

1. Kaydedilmediğine çekirdek kaybı olmadığını belirleme
 - Çekirdek kayıp durum bilgisi olan hizmet çoğaltmaların çoğunluğu aşağı birincil dahil olmak üzere aynı anda hiçbir zaman ' dir.
2. Çekirdek kayıp kalıcı olup olmadığını belirleme
 - Çoğu zaman, geçici hatalarıdır. İşlemleri yeniden başlatılır, düğümlerin yeniden, sanal makineleri yeniden, ağ bölümleri onarma. Bazen hataları kalıcı olmakla birlikte. 
    - Hizmetleri olmadan kalıcı için durumunda, bir çekirdek veya daha fazla çoğaltmaları sonuçları başarısızlığını _hemen_ kalıcı çekirdek kaybına. Service Fabric durum bilgisi olan kalıcı olmayan hizmetinde çekirdek kayıp algıladığında, (olası) dataloss bildirerek hemen 3. adıma geçer. Bunlar kurtarılan olsa bile bunlar boş olacağından, geri dönmeniz için çoğaltmalar bekleyen noktası yok Service Fabric bildiği için dataloss geçmeden mantıklıdır.
    - Kalıcı durum bilgisi olan hizmetler için bir çekirdek veya daha fazla çoğaltması başarısız çoğaltmaları geri dönün ve çekirdek geri yüklemek bekleyen başlatmak Service Fabric neden olur. Bu bir hizmet kesintisi için sonuçları _Yazar_ hizmet etkilenen bölüm (veya "çoğaltma kümesi"). Ancak, okuma hala azaltılmış tutarlılığı garanti ile mümkün olabilir. Devam etmeden (olası) bir dataloss olayıdır ve diğer riskleri taşır geri çekirdek için Service Fabric bekler varsayılan süreyi sonsuzdur. Varsayılan geçersiz kılma `QuorumLossWaitDuration` değer mümkündür, ancak önerilmez. Bunun yerine şu anda tüm çabalara aşağı çoğaltmaları geri yüklemek için yapılmalıdır. Bu, yedekleme aşağı düğümlerini getiren ve yerel kalıcı durumunu depolandığı sürücüleri yeniden bağlayabilirsiniz sağlama gerektirir. Çekirdek kayıp işlem hatasından kaynaklanıyor, Service Fabric işlemleri yeniden oluşturun ve içerdikleri çoğaltmaları yeniden başlatmak otomatik olarak çalışır. Bu başarısız olursa, Service Fabric sistem durumu hataları bildirir. Bunlar çözümlenebilir, ardından çoğaltmaları genellikle geri dönün. Bazı durumlarda, yine de çoğaltmaları geri duruma getirilemiyor. Örneğin, sürücüleri tüm başarısız olmuş veya fiziksel makineler şekilde yok. Bu durumda biz şimdi kalıcı çekirdek kayıp olay sahip. Geri dönmeniz aşağı çoğaltmaları beklemeyi durdurmak için Service Fabric bildirmek için bir Küme Yöneticisi hizmetleri de etkilenir ve çağrı hangi bölümleri belirlemelisiniz `Repair-ServiceFabricPartition -PartitionId` veya ` System.Fabric.FabricClient.ClusterManagementClient.RecoverPartitionAsync(Guid partitionId)` API.  Bu API QuorumLoss dışında ve olası dataloss içine taşımak için bölüm Kimliğini belirtme sağlar.

> [!NOTE]
> Bu _hiçbir zaman_ dışında bu API belirli bölümlere göre hedeflenen bir şekilde kullanmak güvenli. 
>

3. Gerçek veri kaybı yapıldıysa belirlemek ve yedeklerden geri yükleme
  - Service Fabric çağırdığında `OnDataLossAsync` olduğu her zaman nedeniyle yöntemi _şüpheli_ dataloss. Service Fabric sağlar bu çağrı için teslim edilir _en iyi_ kalan çoğaltma. Hangi çoğaltma çoğu ilerleme yaptı budur. Bunun nedeni her zaman dediğimiz _şüpheli_ dataloss olduğundan oluştu zaman birincil yaptığınız gibi kalan çoğaltma gerçekte tüm aynı durumda olduğunu mümkün olduğunu. Ancak, kendisine karşılaştırmak için bu duruma Service Fabric veya işleçleri emin öğrenmek iyi bir yolu yoktur. Bu noktada, Service Fabric ayrıca diğer çoğaltmaları geri gelen değil bilir. Biz kendiliğinden çekirdek kayıp bekleniyor durduğunda yapılan kararının. Freeze ve belirli yönetim müdahalesi için beklemek için iyi davranış biçimini hizmeti için genellikle olur. Tipik bir uyarlamasını ne kadar `OnDataLossAsync` yöntemi musunuz?
  - İlk olarak, bu oturum `OnDataLossAsync` tetiklendi ve tüm gerekli Yönetim Uyarıları kapatmak tetiklenecek.
   - Duraklatma ve gerçekleştirilecek başka kararları ve el ile gerçekleştirilen eylemleri için beklemek için genellikle bu noktada. Bu durum, yedekleme kullanılabilir olsa bile hazırlıklı olmak ihtiyaç duydukları çünkü. Örneğin, iki farklı hizmet bilgileri koordine etmek, bu yedekleri geri yükleme gerçekleştikten sonra bu iki hizmet çok değer verdiğiniz bilgileri tutarlı olduğundan emin olun için değiştirilmesi gerekebilir. 
  - Genellikle de başka bir telemetri veya yoktur hizmetinden Egzoz. Bu meta veriler diğer hizmetlerin veya günlükleri içeriyor. Bu bilgiler, yedeklemede mevcut değil veya bu belirli çoğaltma çoğaltılmış alınan ve birincil işlenen tüm çağrıları olup olmadığını belirlemek için gereken kullanılabilir. Bu yeniden veya geri yükleme uygulanabilirdir önce yedekleme eklenmesi gerekebilir.  
   - Kullanılabilen tüm yedeklemelerinin bulunan kalan yinelemenin durumuna karşılaştırmaları. Araçlar ve bunu yapmak için kullanılabilir işler vardır sonra Service Fabric güvenilir koleksiyonları kullanarak, açıklanan [bu makalede](service-fabric-reliable-services-backup-restore.md). Çoğaltma içinde yeterli bir durumda ya da hangi yedekleme eksik olabilir görmek için belirtilir.
  - Karşılaştırma yapıldıktan sonra ve gerekirse, geri yükleme işlemi tamamlandı servis kodunu durumu değişiklikler yapılmışsa true olarak döndürülmelidir. Çoğaltma durumunun iyi kopyası olan ve herhangi bir değişiklik olduğunu belirlediyseniz, false döndürür. TRUE gösteren herhangi bir _diğer_ çoğaltmaları kalan şimdi olabilir ile tutarsız. Bunlar bırakılacak ve bu çoğaltmasından yeniden. Diğer çoğaltmaları ne sahip oldukları tutmak için yanlış durum değişiklik yapıldığını gösterir. 

Hizmetleri herhangi bir zamanda üretime dağıtılmadan önce hizmet yazarlar olası dataloss ve hata senaryoları alıştırma oldukça önemlidir. Dataloss olasılığını karşı korumak için düzenli aralıklarla önemlidir [durumu yedekleme](service-fabric-reliable-services-backup-restore.md) herhangi bir durum bilgisi olan hizmetlerinizi coğrafi olarak yedekli depolama. Ayrıca, bu geri yükleme yeteneği sahip olduğundan emin olmalısınız. Birçok farklı hizmet yedeklerini farklı zamanlarda alındığından, bir geri yüklendikten sonra hizmetlerinizi birbiriyle tutarlı bir görünümünü sağlamak gerekir. Örneğin, burada bir hizmet bir sayı oluşturur ve bunu depolar, sonra da depolayan başka bir hizmete gönderir bir durum düşünün. Bir geri yüklendikten sonra ikinci hizmet numarasına sahip ancak ilk yoksa bu işlem eklemediniz, bu yedekleme olduğundan değil, fark edebilirsiniz.

Kalan çoğaltmaları gelen dataloss senaryoda devam etmek yeterli değil ve hizmet durumundan telemetri veya Egzoz yeniden yapılandırma öğrenmek, yedekleme sıklığını, en iyi olası kurtarma noktası hedefi (RPO) belirler. Service Fabric kalıcı çekirdek ve bir yedekten geri yükleme gerektiren dataloss dahil olmak üzere çeşitli hatası senaryoları test etmek için birçok araçlar sağlar. Bu senaryolar, Service Fabric'ın Test Edilebilirlik araçları, hata analizi hizmeti tarafından yönetilen bir parçası olarak dahil edilir. Bu araçlar ve desenleri hakkında daha fazla bilgi edinilebilir [burada](service-fabric-testability-overview.md). 

> [!NOTE]
> Sistem Hizmetleri, söz konusu hizmete özgü olan etkisi olan çekirdek kayıp, ayrıca olumsuz etkilenebilir. Örneğin, yeni hizmet oluşturma ve yük devretme işlemlerini Yük Devretme Yöneticisi hizmeti çekirdek kaybında engeller ancak adlandırma hizmeti çekirdek kaybında ad çözümlemesi etkiler. Service Fabric Sistem Hizmetleri hizmetlerinizi durum yönetimi için farklı aynı düzeni uygular, ancak çekirdek kayıp dışında ve olası dataloss halinde taşıma denemesi önerilmez. Bunun yerine önerilir [destek arama](service-fabric-support.md) belirli durumunuza hedeflenen bir çözümü belirlemek için.  Genellikle yalnızca aşağı çoğaltmaları dönüş beklemek tercih edilir.
>

## <a name="availability-of-the-service-fabric-cluster"></a>Service Fabric kümesi kullanılabilirliği
Genel olarak bakıldığında, Service Fabric kümesi hiç tek nokta ile yüksek oranda dağıtılmış bir ortamdır. Öncelikle Service Fabric sistem hizmetleri daha önce sağlanan aynı yönergelere çünkü herhangi bir düğümü kullanılabilirlik veya küme için güvenilirlik sorunlarını neden: Bunlar her zaman üç veya daha fazla yinelemelerle varsayılan olarak ve durum bilgisiz bu sistem hizmetleri tüm düğümler üzerinde çalıştırın. Service Fabric ağ ve hata algılama katmanlarda tam olarak dağıtılır. Çoğu Sistem Hizmetleri kümedeki meta verilerden yeniden veya diğer yerlerden durumlarına eşitlemek biliyorsunuz. Sistem Hizmetleri Çekirdek kayıp durumları açıklananlar gibi yukarıdaki alırsanız kümenin kullanılabilirliğini tehlikeye haline gelir. Bu durumlarda, kümenin bir yükseltmeye başlatmadan veya yeni hizmetlerin dağıtımı gibi bazı işlemleri gerçekleştirmek mümkün olmayabilir, ancak küme hala çalışıyor. Sistem Hizmetleri çalışmaya devam etmesi için yazma gerektirmedikçe zaten çalıştırma Hizmetleri bu koşulları çalışır durumda. Örneğin, Yük Devretme Yöneticisi çekirdek kaybında ise tüm hizmetlerin çalışmaya devam edecek, ancak başarısız olan hizmetlerin hiçbiri bu Yük Devretme Yöneticisi'nin katılımı gerektirdiğinden otomatik olarak yeniden Başlat, mümkün olmaz. 

### <a name="failures-of-a-datacenter-or-azure-region"></a>Bir veri merkezi veya Azure bölgesinin hataları
Nadir durumlarda, bir fiziksel veri merkezi güç veya ağ bağlantı kaybı nedeniyle geçici olarak kullanılamıyor olabilir. Bu durumlarda, Service Fabric kümeleri ve o veri merkezi ya da Azure bölgesi Hizmetleri kullanılamaz olacaktır. Ancak, _verileriniz korunur_. Azure'da çalışan kümeler için üzerinde kesintileri güncelleştirmeleri görüntüleyebilirsiniz [Azure durum sayfasına][azure-status-dashboard]. Yüksek olasılıkla olay fiziksel veri merkezi kısmen veya tamamen olduğunu, var. barındırılan tüm Service Fabric kümeleri veya yok içerdikleri Hizmetleri kaybolabilir. Bu, bu veri merkezi veya bölge dışında yedeklenmeyen herhangi bir durumu içerir.

Bir tek veri merkezi veya bölge kalıcı veya sürekli başarısızlığını sürdüren iki farklı stratejileri yoktur. 

1. Bu tür birden çok bölgede ayrı Service Fabric kümeleri çalıştırın ve yük devretme ile geri bu ortamlar arasında bazı mekanizması kullanır. Bu tür bir çoklu küme etkin-etkin veya etkin-pasif modeli ek yönetimi ve işlemleri kodu gerektirir. Bu, ayrıca diğer veri merkezleri veya bir zaman bölgelerde kullanılabilir olacak şekilde, bir veri merkezi veya bölge Hizmetleri'nde yedeklemelerden eşgüdümünü başarısız gerektirir. 
2. Birden çok veri merkezleri veya bölgeler kapsayan tek bir Service Fabric kümesi çalıştırın. Desteklenen en düşük yapılandırmayı bu üç veri merkezleri veya bölgeleri ' dir. Önerilen bölgeler ve veri merkezleri beş sayısıdır. Bu, daha karmaşık bir küme topolojisi gerektirir. Ancak, bir veri merkezi veya bölge başarısızlığını normal bir hata olağanüstü durumdan dönüştürülür Bu modelin avantajı vardır. Bu hatalar, tek bir bölge içinde kümeleri için iş mekanizmaları tarafından işlenebilir. Hata etki alanlarını, yükseltme etki alanları ve Service Fabric'ın yerleştirme kuralları normal hataları tolerans böylece iş yükleri dağıtılır emin olun. Bu tür bir küme Hizmetler'in çalıştırılmasında yardımcı olan ilkeleri hakkında daha fazla bilgi için okumaya devam [yerleşim ilkeleri](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)

### <a name="random-failures-leading-to-cluster-failures"></a>Küme hataları için önde gelen rasgele hataları
Service Fabric çekirdek düğüm kavramı vardır. Temel alınan kümenin kullanılabilirliğini sürdürmek düğümleri bunlar. Bu düğümler, diğer düğümlerle kiraları oluşturma ve belirli türdeki ağ hataları sırasında tiebreakers hizmet veren yukarı küme kaldığından emin olmak için yardımcı olur. Rastgele hatalara kümedeki çekirdek düğüm çoğunluğu kaldırmak ve geri getirilmez, küme otomatik olarak kapanır. Azure'da, çekirdek düğümler otomatik olarak yönetilir: Yükseltme etki alanları ve kullanılabilir hataya dağıtılan ve tek çekirdek değer düğümü kümeden kaldırılırsa, başka bir yerinde oluşturulur. 

Tek başına Service Fabric kümeleri hem Azure "Birincil düğüm türü" oluştururken çekirdeği çalıştıran adrestir. Birincil düğüm türü tanımlarken, Service Fabric otomatik olarak en fazla 9 çekirdek düğümleri ve sistem hizmetlerinin her biri, 9 çoğaltmaları oluşturarak sağlanan düğüm sayısını yararlanır. Rastgele hatalara bir dizi sistem hizmeti yinelemeler çoğunu aynı anda alırsa, yukarıda açıklanan Sistem Hizmetleri Çekirdek kayıp girer. Çekirdek düğüm çoğunluğu kaybolursa, küme kısa süre içinde kapanır.

## <a name="next-steps"></a>Sonraki adımlar
- Kullanarak çeşitli hatalar benzetimini öğrenin [Test Edilebilirlik framework](service-fabric-testability-overview.md)
- Olağanüstü durum kurtarma ve yüksek kullanılabilirlik kaynaklar okuyun. Microsoft bu konular üzerinde büyük bir miktarını kılavuzunun yayımladı. Bu belgeler bazıları diğer ürünleri kullanmak için belirli teknikler bakın, ancak birçok genel en iyi yöntemler de Service Fabric bağlamda uygulayabilirsiniz içerir:
  - [Kullanılabilirlik denetim listesi](../best-practices-availability-checklist.md)
  - [Bir olağanüstü durum kurtarma ayrıntıya gerçekleştirme](../sql-database/sql-database-disaster-recovery-drills.md)
  - [Azure uygulamaları için yüksek kullanılabilirlik ve olağanüstü durum kurtarma][dr-ha-guide]
- [Service Fabric destek seçenekleri](service-fabric-support.md) hakkında bilgi edinin

<!-- External links -->

[repair-partition-ps]: https://msdn.microsoft.com/library/mt163522.aspx
[azure-status-dashboard]:https://azure.microsoft.com/status/
[azure-regions]: https://azure.microsoft.com/regions/
[dr-ha-guide]: https://msdn.microsoft.com/library/azure/dn251004.aspx


<!-- Images -->

[sfx-cluster-map]: ./media/service-fabric-disaster-recovery/sfx-clustermap.png
