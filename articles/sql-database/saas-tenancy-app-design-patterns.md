---
title: Çok kiracılı SaaS düzenlerinin - Azure SQL veritabanı | Microsoft Docs
description: Gereksinimler ve ortak veri mimarisi düzenlerine ilişkin çok kiracılı yazılım Azure bulut ortamında çalışan bir hizmet (SaaS) veritabanı uygulamaları olarak öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: seoapril2019
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: billgib, sstein
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 6332555c1a176a06004ddfeee513844ad5875c30
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59260553"
---
# <a name="multi-tenant-saas-database-tenancy-patterns"></a>Çok kiracılı SaaS veritabanı kiracılı desenleri

Bu makalede, bir çok kiracılı SaaS uygulaması için kullanılabilen çeşitli kiralama modelleri açıklanmaktadır.

Çok kiracılı bir SaaS uygulama tasarlarken, uygulamanızın ihtiyaçlarına en uygun kiracılı model dikkatli seçmeniz gerekir.  Kiracılı model, her kiracının verileri depolama alanına nasıl eşlenir belirler.  Uygulama tasarımı ve Yönetimi kiracılı model tercih ettiğiniz etkiler.  Daha sonra farklı bir modele geçiş bazen maliyetlidir.

## <a name="a-saas-concepts-and-terminology"></a>A. SaaS kavramlar ve terminoloji

İçinde yazılım olarak hizmet (SaaS) modelini, şirketinizin değil satmak *lisansları* yazılımınızla. Bunun yerine, her müşteri yapmadan şirketiniz için ödemeleri kiralayabilirler her müşterinin yaptığı bir *Kiracı* şirketinizin.

Kira kadar ödediğinizden tanıtmanın karşılığında her Kiracı SaaS uygulama bileşenlerinizin erişimi alır ve kendi veri SaaS sistemde saklanan.

Terim *kiracılı model* kiracılar depolanan verilerin nasıl düzenlendiği için ifade eder:

- *Tek kiracılı:*&nbsp; Her bir veritabanı yalnızca tek bir kiracı verilerini depolar.
- *Çok kiracılı modeli:*&nbsp; Her veritabanını ayrı birden çok kiracıdan gelen veri (veri gizliliği koruyacak şekilde mekanizmalarıyla) depolar.
- Karma kiralama modelleri de mevcuttur.

## <a name="b-how-to-choose-the-appropriate-tenancy-model"></a>B. Uygun kiracılı model seçme

Genel olarak, kiralama modeli, bir uygulamanın işlevi etkilemez, ancak büyük olasılıkla diğer yönleri genel çözümünün etkiler.  Aşağıdaki ölçütleri modellerinin her biri değerlendirmek için kullanılır:

- **Ölçeklenebilirlik:**
    - Kiracıların sayısı.
    - Depolama Kiracı başına.
    - Toplam depolama.
    - İş yükü.

- **Kiracı yalıtımı:**&nbsp; Veri yalıtımı ve performans (olup bir kiracının iş yükünü diğerleri etkiler).

- **Kiracı başına maliyet:**&nbsp; Veritabanı maliyet.

- **Geliştirme karmaşıklığı:**
    - Şema değişiklikleri.
    - Sorgular (deseni tarafından gerekli) yapılan değişiklikler.

- **İşletimsel karmaşıklığın:**
    - İzleme ve performansı yönetme.
    - Şema Yönetimi.
    - Bir kiracıyı geri yükleme.
    - Olağanüstü durum kurtarma.

- **Konularında kendini gösterir:**&nbsp; Olan şema özelleştirmeleri destek kolaylığı kiracıya özgü ya da sınıfa özel Kiracı.

Kiralama tartışma odaklanmıştır *veri* katman.  Ancak bir süre için göz önünde bulundurun *uygulama* katman.  Uygulama katmanı, tek parçalı bir varlık olarak kabul edilir.  Uygulama çok sayıda küçük bileşenlere ayırmak kiracılı model seçiminizi değiştirebilirsiniz.  Hem Kiracı ve depolama teknolojisi veya kullanılan platform ile ilgili bazı bileşenler diğerlerinden farklı şekilde davranabilir.

## <a name="c-standalone-single-tenant-app-with-single-tenant-database"></a>C. Tek kiracılı veritabanı içeren tek başına tek kiracılı uygulaması

#### <a name="application-level-isolation"></a>Uygulama düzeyinde yalıtım

Bu modelde, tüm uygulama tekrar tekrar kez her Kiracı için yüklenir.  Uygulamanın her örneği tek başına bir örneğini olduğundan hiçbir zaman herhangi bir tek başına örneğini ile etkileşime geçer.  Uygulamanın her örneği, tek bir kiracının vardır ve bu nedenle yalnızca bir veritabanı gerekir.  Kiracının tüm kendisi için bir veritabanı vardır.

![Tam olarak bir tek kiracılı veritabanı içeren tek başına uygulama tasarımını.][image-standalone-app-st-db-111a]

Her uygulama örneği ayrı bir Azure kaynak grubu içinde yüklenir.  Kaynak grubu, yazılım satıcısına veya Kiracı tarafından sahip olunan bir aboneliğe ait olabilir.  Her iki durumda da, Kiracı için yazılım satıcısına yönetebilirsiniz.  Her uygulama örneği, karşılık gelen bunun veritabanına bağlanmak için yapılandırılır.

Her Kiracı veritabanı tek veritabanı olarak dağıtılır.  Bu model, büyük veritabanı yalıtım sağlar.  Ancak, en fazla işlemek için her bir veritabanı için yeterli kaynak ayrılmasını yalıtım gerektirir.  Burada elastik havuzlar için farklı kaynak gruplarında veya farklı Aboneliklerde dağıtılan veritabanları kullanılamaz önemlidir.  Bu sınırlama, bu tek başına tek kiracılı uygulama genel bir veritabanı maliyet açısından en pahalı çözüm modeli sağlar.

#### <a name="vendor-management"></a>Satıcı Yönetim

Uygulama örnekleri farklı Kiracı aboneliklerine yüklü olsa bile, satıcı tüm tek başına uygulama örnekleri, içindeki tüm veritabanlarına erişebilirsiniz.  SQL bağlantı erişim elde edilir.  Bu örnek arası erişim şema yönetimi ve raporlama ya da analitik amaçlar için platformlar arası sorgu merkezileştirmek satıcı etkinleştirebilirsiniz.  Bu tür bir merkezi yönetim isterseniz, bir katalog veritabanına URI'ler Kiracı tanımlayıcıları eşleyen dağıtılmalıdır.  Azure SQL veritabanı bir SQL veritabanı ile birlikte bir katalog sağlamak için kullanılan bir parçalama kitaplık sağlar.  Parçalama kitaplığı izlerse adlı [elastik veritabanı istemci Kitaplığı][docu-elastic-db-client-library-536r].

## <a name="d-multi-tenant-app-with-database-per-tenant"></a>D. Kiracı başına veritabanı ile çok kiracılı uygulama

Bu sonraki deseni kullanan çok kiracılı bir uygulama birden fazla veritabanıyla tüm veritabanlarını tek kiracılı oluşturuluyor.  Yeni bir veritabanı için her bir yeni Kiracı sağlanır.  Uygulama katmanı ölçeklendirilir *yukarı* düğüm başına daha fazla kaynak ekleyerek dikey.  Veya uygulama ölçeklendirilir *kullanıma* daha fazla düğüm ekleyerek yatay olarak.  Ölçeklendirme iş yüküne göre ve sayı veya ölçek bağımsız veritabanlarının bağımsızdır.

![Kiracı başına veritabanı ile çok kiracılı uygulama tasarımını.][image-mt-app-db-per-tenant-132d]

#### <a name="customize-for-a-tenant"></a>Bir kiracı için özelleştirme

Tek başına uygulama desen gibi tek kiracılı veritabanlarının kullanımını güçlü Kiracı yalıtımı sağlar.  Yalnızca tek kiracılı veritabanları, modelini belirtir, herhangi bir uygulamada herhangi bir belirli bir veritabanı için şema özelleştirilebilir ve kendi Kiracı için en iyi duruma getirilmiş.  Bu özelleştirme, diğer kiracıların uygulamayı etkilemez. Belki de bir kiracının tüm kiracılar gereken temel veri alanlarını aşan veri gerekebilir.  Daha fazla veri alanı dizin gerekebilir.

Veritabanı Kiracı başına ile bir veya daha fazla bireysel Kiracı için şemayı özelleştirme elde etmek oldukça basittir.  Uygulama satıcısıyla şema özelleştirmeleri uygun ölçekte dikkatli bir şekilde yönetmek için yordamları tasarlamanız gerekir.

#### <a name="elastic-pools"></a>Esnek havuzlar

Aynı kaynak grubunda dağıtılan veritabanları, elastik havuzları halinde gruplandırılabilir.  Havuzlar, birçok veritabanları arasında kaynakların paylaşılması için uygun maliyetli bir yol sağlar.  Bu havuz seçeneği her veritabanının karşılaştığı kullanımın en üst düzeye tutabilecek kadar büyük olmasını gerektiren daha ucuz.  Havuza alınmış veritabanlarını paylaşım kaynaklara erişimi olsa da hala yüksek düzeyde performans yalıtımı elde edebilirsiniz.

![Çok kiracılı uygulaması ile veritabanı-elastik havuz kullanımının Kiracı başına, tasarım.][image-mt-app-db-per-tenant-pool-153p]

Azure SQL veritabanı, yapılandırmayı, izlemeyi ve şirket dışı paylaşımı yönetme için gereken araçları sağlar.  Her iki havuz ve veritabanı düzeyinde performans ölçümleri, Azure İzleyici günlüklerine ve Azure portalında kullanılabilir.  Ölçümleri toplar ve kiracıya özgü performans harika Öngörüler verebilirsiniz.  Tek veritabanları havuzlar, belirli bir kiracıya ayrılan kaynakları sağlamak üzere arasında taşınabilir.  Bu araçlar, uygun maliyetli bir şekilde iyi performansı elde etmeyi sağlar.

#### <a name="operations-scale-for-database-per-tenant"></a>Kiracı başına veritabanı için ölçeklendirme işlemleri

Azure SQL veritabanı platformuna veritabanlarını ölçeğe 100.000 iyi veritabanları gibi çok sayıda yönetmek üzere tasarlanmış birçok yönetim özelliği vardır.  Bu özellikler, Kiracı başına veritabanı düzenini yatkýn hale getirir.

Örneğin, 1000-Kiracı veritabanı tek veritabanı olarak bir sistem olduğunu varsayalım.  Veritabanı 20 dizinlere sahip.  Sistem, 1000 tek kiracılı veritabanına sahip olmanın için dönüştürür, dizinleri miktarını 20,000 ile artar.  Bir parçası olarak SQL veritabanı'nda [otomatik ayarlama][docu-sql-db-automatic-tuning-771a], otomatik dizin oluşturma özellikleri varsayılan olarak etkindir.  Otomatik dizin oluşturma, tüm 20.000 dizinleri ve bunların sürekli oluşturma ve bırakma iyileştirmeleri yönetir.  Bu otomatik eylemler içindeki tek veritabanı oluşur ve bunlar katılmamaları Eşgüdümlü veya diğer veritabanlarındaki benzer işlemler tarafından kısıtlanmış.  Otomatik dizin oluşturma dizinleri farklı daha az meşgul veritabanı meşgul bir veritabanındaki değerlendirir.  Bu büyük bir yönetim görevi el ile yapılması gerekirse bu tür bir dizin yönetimi özelleştirme Kiracı başına veritabanı ölçekte pratik olacaktır.

İyi diğer yönetim özellikleri şunları içerir:

- Yerleşik yedeklemeler.
- Yüksek kullanılabilirlik.
- Disk üzerinde şifreleme.
- Performans telemetrisi.

#### <a name="automation"></a>Otomasyon

Yönetim işlemlerini komut dosyası ve üzerinden sunulan bir [devops] [ http-visual-studio-devops-485m] modeli.  İşlemleri bile otomatik ve uygulamada gösterilen.

Örneğin, tek bir kiracının kurtarılması daha önceki bir noktaya sürede otomatikleştirin.  Kurtarma yalnızca Kiracı depolar tek tek kiracılı veritabanının geri yüklenmesi gerekir.  Bu geri yükleme yönetimi işlemleri her bir kiracı ince ayrıntılı düzeyde olduğunu doğrulayan diğer kiracıların üzerinde etkisi yoktur.

## <a name="e-multi-tenant-app-with-multi-tenant-databases"></a>E. Çok kiracılı veritabanı içeren çok kiracılı uygulama

Başka bir kullanılabilir bir çok kiracılı veritabanında çok sayıda Kiracı saklamak modelidir.  Uygulama örneği, çok kiracılı veritabanı herhangi bir sayı olabilir.  Herhangi bir kiracı verilerini seçmeli olarak alınabilir böylece çok kiracılı veritabanı şeması bir veya daha fazla Kiracı tanımlayıcısı sütun başlıkları olmalıdır.  Ayrıca, şemayı birkaç tablo veya yalnızca bir alt kümesini kiracılar tarafından kullanılan sütunları gerektirebilir.  Ancak, statik kod ve başvuru verileri yalnızca bir kez depolanır ve tüm kiracılar tarafından paylaşılır.

#### <a name="tenant-isolation-is-sacrificed"></a>Kiracı yalıtımı feda

*Veri:*&nbsp; Çok kiracılı veritabanı, Kiracı yalıtımı mutlaka gözden çıkarır.  Birden çok kiracının verileri birlikte bir veritabanında depolanır.  Geliştirme sırasında sorgular birden fazla Kiracı verilerini hiçbir zaman kullanıma sunduğundan emin olun.  SQL veritabanı destekler [satır düzeyi güvenlik][docu-sql-svr-db-row-level-security-947w], tek bir kiracı için bir sorgudan döndürülen verilerin zorunlu kılabilir kapsamlı.

*İşleme:*&nbsp; Çok kiracılı veritabanı, tüm kiracılar genelinde işlem ve depolama kaynaklarını paylaşır.  Çalışarak gerçekleştiriyor emin olmak için veritabanı bir bütün olarak izlenebilir.  Ancak, Azure sistem bu kaynakları tek bir kiracı tarafından kullanımını yönetmek veya izlemek için yerleşik bir yolu yoktur.  Bu nedenle, çok kiracılı veritabanı burada bir overactive kiracının iş yükünü aynı veritabanında diğer kiracıların performans deneyimini etkiler gürültücü Komşuları karşılaşıldığında, riski taşır.  Uygulama düzeyinde ek izleme Kiracı düzeyinde performansını izleyebilirsiniz.

#### <a name="lower-cost"></a>Düşük maliyet

Genel olarak, en düşük maliyeti Kiracı başına çok kiracılı veritabanına sahip.  Kaynak maliyetleri tek bir veritabanı için bir eşdeğer boyutta bir elastik havuz için daha düşük.  Buna ek olarak, kiracılar yalnızca sınırlı depolama gereken yere senaryoları için büyük olasılıkla kiracılar milyonlarca tek bir veritabanında depolanabilir.  Elastik havuz yok milyonlarca veritabanında içerebilir.  Bununla birlikte, havuz, 1000 havuzlarıyla başına 1000 veritabanı içeren bir çözüm yönetmek için zahmetli hale at the risk of milyonlarca ölçeğini ulaşabilir.

İki çeşidi çok kiracılı veritabanı modeli, parçalı çok kiracılı model en esnek ve ölçeklenebilir aşağıda içinde ele alınmıştır.

## <a name="f-multi-tenant-app-with-a-single-multi-tenant-database"></a>F. Tek bir çok kiracılı veritabanına sahip çok kiracılı uygulama

En basit desen çok kiracılı veritabanı, tüm kiracılar için verileri barındırmak için tek bir veritabanı kullanır.  Daha fazla Kiracı eklendikçe veritabanı ile daha fazla depolama ve işlem kaynaklarını ölçeği.  Her zaman bir ultimate ölçek sınırına olsa gereklidir, bu ölçek olabilir.  Ancak, veritabanı bu sınırı dolmadan önce uzun yönetmek için zahmetli hale gelir.

Tek tek kiracının odaklı yönetim işlemlerini bir çok kiracılı veritabanında uygulamak için daha karmaşıktır.  Ve uygun ölçekte bu işlemleri edilemeyecek kadar düşük olabilir.  Tek bir kiracının verileri zaman içinde nokta geri yüklemeye bir örnektir.

## <a name="g-multi-tenant-app-with-sharded-multi-tenant-databases"></a>G. Parçalı çok kiracılı veritabanı içeren çok kiracılı uygulama

Çoğu SaaS uygulamalarını aynı anda yalnızca tek bir kiracı verilere.  Bu erişim düzeni Kiracı verilerini birden fazla veritabanında dağıtılacak izin verir veya parçalar herhangi biri için tüm verilerin nerede Kiracı, bir parçada bulunan.  Bir çok kiracılı veritabanı modeli ile birlikte, parçalı bir model neredeyse sınırsız ölçeklendirme sağlar.

![Parçalı çok kiracılı veritabanı içeren çok kiracılı uygulama tasarımını.][image-mt-app-sharded-mt-db-174s]

#### <a name="manage-shards"></a>Parçaları yönetme

Parçalama, hem işletimsel yönetim ve tasarımı için karmaşıklık ekler.  Katalog, kiracılar ve veritabanları arasındaki eşlemeyi tutmak gereklidir.  Ayrıca, yönetim yordamları parçalar ve Kiracı popülasyon yönetmek için gereklidir.  Örneğin, yordamları parçalar ekleyip ve Kiracı verileri parçalar arasında taşımak için tasarlanmış olması gerekir.  Ölçeklendirme için bir yeni bir parça ekleyerek ve yeni Kiracı ile doldurma yöntemdir.  Diğer saatlerde densely doldurulmuş bir parça iki daha az densely doldurulmuş parçaya bölmesi.  Birden çok kiracıyı taşınmış veya kullanımdan sonra seyrek doldurulmuş parçaların birlikte birleştirme.  Daha fazla maliyet açısından verimli kaynak kullanımı birleştirme neden olur.  Kiracılar ayrıca çalışma yükünü dengelemek için parçalar arasında taşınmış olabilir.

SQL veritabanı parçalama kitaplığı ve Katalog veritabanı ile birlikte çalışan bir ayırma/Birleştirme aracı sağlar.  Sağlanan uygulama bölme ve birleştirme parçalar ve Kiracı verileri parçalar arasında taşıyabilirsiniz.  Uygulama Kataloğu işaretlemek, bu işlemleri sırasında kiracılar bunları taşıma önce çevrimdışı olarak etkilenen da korur.  Taşıma sonrasında, uygulama Kataloğu tekrar yeni eşleme ve Kiracı tekrar çevrimiçi olarak işaretleme güncelleştirir.

#### <a name="smaller-databases-more-easily-managed"></a>Daha küçük veritabanları daha kolay yönetilen

Kiracılar birden çok veritabanları arasında dağıtarak, parçalı çok kiracılı bir çözüm içinde daha kolay yönetilir küçük veritabanı sonuçlanır.  Örneğin, belirli bir kiracıda artık zaman içinde önceki bir noktaya geri yükleme tek daha küçük bir veritabanı, tüm kiracıların içeren daha büyük bir veritabanı yerine bir yedekleme geri yükleme gerektirir. Veritabanı boyutu ve Kiracı başına veritabanı sayısı, iş yükü ve Yönetimi çalışmalarını dengelemek için seçilebilir.

#### <a name="tenant-identifier-in-the-schema"></a>Şemada Kiracı tanımlayıcısı

Veritabanı şema üzerinde kullanılan parçalama yaklaşım bağlı olarak, ek kısıtlamalar uygulanan.  SQL veritabanı ayırma/birleştirme uygulama şema, genellikle Kiracı tanımlayıcısı olan parçalama anahtarı içerdiğini gerektirir.  Kiracı tanımlayıcısı, parçalı tablonun birincil anahtarında önde gelen öğesidir.  Kiracı tanımlayıcısı, hızla bulup belirli bir kiracı ile ilişkilendirilen veri taşımak bölünmüş/merge uygulamanın sağlar.

#### <a name="elastic-pool-for-shards"></a>Elastik havuz için parçaları

Parçalı çok Kiracı veritabanları elastik havuzlarını yerleştirilebilir.  Genel olarak, birçok tek kiracılı veritabanlarının bir havuzda sahip uygun maliyetli birkaç çok kiracılı veritabanlarında birçok kiracının sahip olduğu gibidir.  Çok kiracılı veritabanları, çok sayıda görece etkin olmayan kiracılar olduğunda yararlı değildir.

## <a name="h-hybrid-sharded-multi-tenant-database-model"></a>H. Karma parçalı çok kiracılı veritabanı modeli

Karma modelinde, Kiracı tanımlayıcısı, şemadaki tüm veritabanlarına sahip.  Veritabanlarını birden fazla Kiracı depolama tüm uyumlu ve veritabanlarını parçalı olabilir.  Bu nedenle şema anlamda, tüm çok kiracılı veritabanları değildirler.  Henüz uygulamada bu veritabanlarından bazıları yalnızca tek bir kiracı içerir.  Ne olursa olsun, belirli bir veritabanında depolanan kiracılar miktarını veritabanı şeması üzerinde etkisi yoktur.

#### <a name="move-tenants-around"></a>Kiracılar gezinme

Herhangi bir zamanda, belirli bir kiracı kendi çok kiracılı veritabanına taşıyabilirsiniz.  Ve istediğiniz zaman fikrinizi değiştirirseniz ve Kiracı birden çok kiracının içeren bir veritabanına geri taşıyın.  Yeni veritabanı sağladığınızda, Kiracı için yeni bir tek kiracılı veritabanı atayabilirsiniz.

Karma modeli kaynak gereksinimlerini tanımlama grupları kiracılar arasında büyük farklar olduğunda ortaya çıkar.  Örneğin, kiracıların abone olan performans yüksek düzeyde bir ücretsiz deneme katılan kiracılar garanti edilmez varsayalım.  İlke, kiracılar için tüm ücretsiz deneme kiracıları arasında paylaşılan bir çok kiracılı veritabanında depolanacak ücretsiz deneme aşamasında olabilir.  Kiracı, temel hizmet katmanı için ücretsiz bir deneme kiracısı abone olduğunda, daha az kiracılar olabilecek başka bir çok kiracılı veritabanına taşınabilir.  Premium hizmet katmanı için ödenen abone yeni kendi tek kiracılı veritabanına taşınması.

#### <a name="pools"></a>Havuzlar

Bu karma modelde, Kiracı başına veritabanı maliyetleri azaltmak için kaynak havuzlarında tek kiracılı veritabanlarını kiracıların abone yerleştirilebilir.  Bu ayrıca Kiracı başına veritabanı modelde gerçekleştirilir.

## <a name="i-tenancy-models-compared"></a>I. Karşılaştırılan kiralama modelleri

Aşağıdaki tabloda ana kiralama modelleri arasındaki farklar özetlenmektedir.

| Ölçüm | Tek başına uygulama | Kiracı başına veritabanı | Parçalı çok kiracılı |
| :---------- | :------------- | :------------------ | :------------------- |
| Ölçek | Orta<br />1-100'lük bloklar | Çok yüksek<br />1-100,000s | Sınırsız<br />1-1.000.000'luk |
| Kiracı yalıtımı | Çok yüksek | Yüksek | Düşük; (yani bir MT DB'de tek başına) herhangi tek kiracılı dışında. |
| Kiracı başına veritabanı maliyeti | Yüksek; en yüksek sayılar için boyutlandırılır. | Düşük; kullanılan havuzların. | Düşük, MT DBs küçük kiracılar için. |
| Performans izleme ve yönetim | Yalnızca Kiracı başına | Toplama + Kiracı başına | Toplama; ancak Kiracı başına yalnızca tekler için olan. |
| Geliştirme karmaşıklığı | Düşük | Düşük | Orta; Parçalama nedeniyle. |
| İşletimsel karmaşıklığın | Düşük-yüksek. Tek tek basit ve uygun ölçekte karmaşık. | Düşük-Orta. Desenler, karmaşıklığı uygun ölçekte adresi. | Düşük-yüksek. Tek tek Kiracı Yönetimi karmaşıktır. |
| &nbsp; ||||

## <a name="next-steps"></a>Sonraki adımlar

- [Kiracı başına veritabanı SaaS modeli - Azure SQL veritabanı kullanan çok kiracılı Wingtip uygulamasını dağıtma ve keşfetme][docu-sql-db-saas-tutorial-deploy-wingtip-db-per-tenant-496y]

- [Wingtip bilet örnek SaaS Azure SQL veritabanı kiralama uygulaması'na Hoş Geldiniz][docu-saas-tenancy-welcome-wingtip-tickets-app-384w]


<!--  Article link references.  -->

[http-visual-studio-devops-485m]: https://www.visualstudio.com/devops/

[docu-sql-svr-db-row-level-security-947w]: https://docs.microsoft.com/sql/relational-databases/security/row-level-security

[docu-elastic-db-client-library-536r]: sql-database-elastic-database-client-library.md
[docu-sql-db-saas-tutorial-deploy-wingtip-db-per-tenant-496y]: sql-database-saas-tutorial.md
[docu-sql-db-automatic-tuning-771a]: sql-database-automatic-tuning.md
[docu-saas-tenancy-welcome-wingtip-tickets-app-384w]: saas-tenancy-welcome-wingtip-tickets-app.md


<!--  Image references.  -->

[image-standalone-app-st-db-111a]: media/saas-tenancy-app-design-patterns/saas-standalone-app-single-tenant-database-11.png "Tam olarak bir tek kiracılı veritabanı içeren tek başına uygulama tasarımını."

[image-mt-app-db-per-tenant-132d]: media/saas-tenancy-app-design-patterns/saas-multi-tenant-app-database-per-tenant-13.png "Kiracı başına veritabanı ile çok kiracılı uygulama tasarımını."

[image-mt-app-db-per-tenant-pool-153p]: media/saas-tenancy-app-design-patterns/saas-multi-tenant-app-database-per-tenant-pool-15.png "Çok kiracılı uygulaması ile veritabanı-elastik havuz kullanımının Kiracı başına, tasarım."

[image-mt-app-sharded-mt-db-174s]: media/saas-tenancy-app-design-patterns/saas-multi-tenant-app-sharded-multi-tenant-databases-17.png "Parçalı çok kiracılı veritabanı içeren çok kiracılı uygulama tasarımını."

