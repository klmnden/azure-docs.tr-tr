---
title: "Çok kiracılı SaaS desenler - Azure SQL veritabanı | Microsoft Docs"
description: "Gereksinimler ve ortak verileri ve Azure bulut ortamında çalışan bir hizmet (SaaS) veritabanı uygulamaları olarak çok kiracılı yazılım mimarisi desenlerini öğrenin."
keywords: "sql veritabanı öğreticisi"
services: sql-database
documentationcenter: 
author: billgib
manager: craigg
editor: MightyPen,srinia
ms.assetid: 1dd20c6b-ddbb-40ef-ad34-609d398d008a
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Active
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/12/2017
ms.author: billgib
ms.openlocfilehash: 0377baaa4a0db7e3cb2041f3ca018322e379f0df
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="multi-tenant-saas-database-tenancy-patterns"></a>Çok kiracılı SaaS veritabanı kiralama desenleri

Çok kiracılı SaaS uygulama tasarlarken, uygulamanızın gereksinimlerine en uygun kiralama modeli dikkatle seçmeniz gerekir.  Kiracı modelini, her bir kiracının veri depolama alanına nasıl eşleştiğini belirler.  Uygulama tasarımı ve Yönetimi Kiracı modelinin tercih ettiğiniz etkiler.  Daha sonra farklı bir modele geçiş bazen maliyetlidir.

Alternatif kiralama modelleri tartışması izler.

## <a name="a-how-to-choose-the-appropriate-tenancy-model"></a>A. Uygun kiralama modeli seçme

Genel olarak, Kiracı modeli uygulamanın işlevini etkilemez, ancak bunu büyük olasılıkla diğer yönleri genel çözümünün etkiler.  Aşağıdaki ölçütleri modellerinin her biri değerlendirmek için kullanılır:

- **Ölçeklenebilirlik:**
    - Kiracı sayısı.
    - Depolama alanı başına-Kiracı.
    - Toplam depolama.
    - İş yükü.

- **Kiracı yalıtımı:** &nbsp; veri yalıtımı ve performans (olup bir kiracının iş yükü başkalarının etkiler).

- **Kiracı başına maliyet:** &nbsp; veritabanı maliyetleri.

- **Geliştirme karmaşıklığı:**
    - Şema değişiklikleri.
    - (Bir desen gerekli) sorguları yapılan değişiklikler.

- **İşletim karmaşıklığını:**
    - İzleme ve performansı yönetme.
    - Şema Yönetimi.
    - Bir kiracı geri yükleniyor.
    - Olağanüstü durum kurtarma.

- **Özelleştirilebilirliğini:** &nbsp; olan şema özelleştirmeleri destekleme kolaylığı Kiracı özgü veya sınıf özgü Kiracı.

Kiracı tartışma odaklanmıştır *veri* katmanı.  Ancak bir süre için göz önünde bulundurun *uygulama* katmanı.  Uygulama katmanı tek yapılı bir varlık olarak kabul edilir.  Çok sayıda küçük bileşenleri uygulamasına bölerseniz, tercih ettiğiniz kiralama modelinin değişebilir.  Hem Kiracı ve depolama teknolojisi veya kullanılan platform ile ilgili bazı bileşenleri diğerlerinden farklı davranabilir.

## <a name="b-standalone-single-tenant-app-with-single-tenant-database"></a>B. Tek Kiracı veritabanı ile tek başına tek Kiracı uygulama

#### <a name="application-level-isolation"></a>Uygulama düzeyi yalıtımı

Bu modelde, tüm uygulama tekrar tekrar kez her bir kiracı için yüklenir.  Her uygulama örneği tek başına bir örneğini olduğundan, hiçbir zaman başka bir tek başına örneği ile etkileşim kurar.  Her uygulama örneği yalnızca bir kiracı var ve bu nedenle yalnızca bir veritabanı gerekiyor.  Kiracı veritabanına tüm kendisini sahiptir.

![Tek tek Kiracı veritabanı ile tek başına uygulama tasarımını.][image-standalone-app-st-db-111a]

Her bir uygulama örneği ayrı Azure kaynak grubunda yüklenir.  Kaynak grubu, yazılım satıcısına veya Kiracı tarafından sahip olunan bir aboneliğe ait olabilir.  Her iki durumda da, Kiracı için yazılım satıcısına yönetebilirsiniz.  Her uygulama örneği karşılık gelen veritabanına bağlanmak için yapılandırılır.

Her Kiracı veritabanı bir tek başına veritabanı olarak dağıtılır.  Bu model, en büyük veritabanı yalıtım sağlar.  Ancak, yoğun yüklerini işlemek için her bir veritabanı için yeterli kaynak ayrılması bir yalıtım gerektirir.  Burada esnek havuzlar farklı kaynak gruplarında veya farklı Aboneliklerde dağıtılan veritabanları için kullanılamaz önemlidir.  Bu sınırlama genel bir veritabanı maliyet açısından en pahalı çözüm modeli bu tek başına tek Kiracı uygulama yapmaz.

#### <a name="vendor-management"></a>Satıcı Yönetim

Uygulama örnekleri farklı Kiracı aboneliklerine yüklü olsa bile satıcı tüm tek başına app örnekleri, tüm veritabanlarına erişim sağlayabilir.  Erişim SQL bağlantıları elde edilir.  Bu örnek arası erişim şema yönetimi ve veritabanları arası sorgu raporlama ya da analiz amacıyla merkezileştirmek satıcı etkinleştirebilirsiniz.  Bu tür bir merkezi yönetim istenirse, Kiracı tanımlayıcıları URI'ler veritabanı için eşleşen bir katalog dağıtılmalıdır.  Azure SQL veritabanı bir SQL veritabanı ile birlikte bir katalog sağlamak için kullanılan bir parçalama kitaplık sağlar.  Parçalama kitaplık resmi olarak adlandırılan [esnek veritabanı istemci Kitaplığı][docu-elastic-db-client-library-536r].

## <a name="c-multi-tenant-app-with-database-per-tenant"></a>C. Kiracı başına veritabanı ile çok kiracılı uygulama

Çok kiracılı uygulama bu sonraki desen kullanan birçok veritabanı ile tüm tek Kiracı Veritabanları bırakılıyor.  Yeni bir veritabanı, her yeni bir kiracı için sağlanır.  Uygulama katmanı ölçeği *yukarı* düğüm başına daha fazla kaynak ekleyerek dikey olarak.  Veya uygulama ölçeklendirilir *çıkışı* daha fazla düğüm ekleyerek yatay olarak.  Ölçeklendirme iş yüküne göre ve sayı veya veritabanlarını tek tek ölçek bağımsızdır.

![Kiracı başına veritabanı ile çok kiracılı uygulama tasarımını.][image-mt-app-db-per-tenant-132d]

#### <a name="customize-for-a-tenant"></a>Bir kiracı için özelleştirme

Tek başına uygulama deseni gibi Kiracı tek veritabanlarının kullanımını güçlü Kiracı yalıtımı sağlar.  Yalnızca tek Kiracı veritabanları, modeli belirtir, herhangi bir uygulamada herhangi bir belirtilen veritabanı için şema özelleştirilebilir ve kendi Kiracı için en iyi duruma getirilmiş.  Bu özelleştirme uygulama diğer kiracılar etkilemez. Belki de bir kiracı tüm kiracılar gereken temel veri alanları aşan veri gerekebilir.  Ayrıca, ek verileri alan bir dizin gerekebilir.

Veritabanı başına-Kiracı ile bir veya daha fazla tek tek kiracılar için şema özelleştirme elde etmek basittir.  Uygulamanın satıcısına dikkatle şema özelleştirmeleri ölçekte yönetmek için yordamları tasarlamanız gerekir.

#### <a name="elastic-pools"></a>Esnek havuzlar

Veritabanları aynı kaynak grubunda dağıtıldığında, esnek veritabanı havuzları halinde gruplandırılabilir.  Havuzlar birçok veritabanı arasında kaynak paylaşımı, düşük maliyetli bir yolunu sağlar.  Bu havuz her veritabanının karşılaştığı kullanım yükselmeleri uyabilecek kadar büyük olmasını gerektiren daha ucuz bir seçenektir.  Havuza alınmış veritabanları kaynaklarına erişimi paylaşırlar hala yüksek dereceli bir performans yalıtımı elde edebilirsiniz.

![Veritabanı başına-esnek havuz kullanarak Kiracı, çok kiracılı uygulamayla tasarımını.][image-mt-app-db-per-tenant-pool-153p]

Azure SQL veritabanı yapılandırmak, izlemek ve paylaşımı yönetmek için gerekli araçları sağlar.  Her iki havuzu ve veritabanı düzeyi performans ölçümleri, Azure portalında ve günlük analizi ile kullanılabilir.  Ölçümleri harika performans toplama ve Kiracı özgü fikir verebilir.  Tek veritabanlarını belirli bir kiracı için ayrılmış kaynakları sağlamak üzere havuzları arasında taşınabilir.  Bu araçlar, uygun maliyetli bir şekilde iyi bir performans sağlamak etkinleştirin.

#### <a name="operations-scale-for-database-per-tenant"></a>Kiracı başına veritabanı için işlemler ölçek

Azure SQL veritabanı platform veritabanları ölçekte 100.000 iyi veritabanları gibi çok sayıda yönetim için tasarlanmış birçok yönetim özelliği vardır.  Bu özellikler Kiracı başına veritabanı düzeni yatkýn olun.

Örneğin, yalnızca bir veritabanı olarak 1000 Kiracı veritabanı bir sistem olduğunu varsayalım.  Veritabanı 20 dizinleri sahip olabilir.  Sistem 1000 tek Kiracı veritabanları zorunda dönüştürür, 20.000 için dizinler miktar artar.  Bir parçası olarak SQL veritabanında [otomatik ayarlama][docu-sql-db-automatic-tuning-771a], otomatik dizin oluşturma özellikler varsayılan olarak etkindir.  Otomatik dizin oluşturma sizin için tüm 20.000 dizinleri ve bunların devam eden oluşturma ve bırakma iyileştirmeleri yönetir.  Tek bir veritabanının bu otomatik eylemler gerçekleşir ve bunlar değil Eşgüdümlü veya diğer veritabanlarındaki benzer eylemler tarafından kısıtlanmış.  Otomatik dizin oluşturma dizinleri farklı daha az meşgul veritabanı meşgul bir veritabanında değerlendirir.  Bu büyük yönetim görevi el ile yapılması gerekiyordu ise bu tür dizin yönetimi özelleştirme Kiracı başına veritabanı ölçekte pratik olabilir.

İyi ölçeklendirme diğer yönetim özellikleri şunlardır:

- Yerleşik yedeklemeler.
- Yüksek kullanılabilirlik.
- Disk üzerinde şifreleme.
- Performans telemetrisini.

#### <a name="automation"></a>Otomasyon

Yönetim işlemlerini komut dosyası ve aracılığıyla sunulan bir [devops] [ http-visual-studio-devops-485m] modeli.  İşlemler bile otomatik ve uygulamada açık.

Örneğin, tek bir kiracı kurtarma daha önceki bir noktaya zamanında otomatikleştirmek.  Kurtarma yalnızca Kiracı depolar tek tek Kiracı veritabanının geri yüklenmesi gerekir.  Bu geri yükleme yönetim işlemlerini tek tek her Kiracı ince parçalı düzeyde olduğunu doğrulayan diğer kiracılar üzerinde etkisi yoktur.

## <a name="d-multi-tenant-app-with-multi-tenant-databases"></a>D. Çoklu kiracı veritabanları ile çok kiracılı uygulama

Başka bir kullanılabilir deseni, bir çok kiracılı veritabanında çok sayıda Kiracı depolamaktır.  Uygulama örneği çok Kiracı veritabanı herhangi bir sayıda olabilir.  Çok Kiracı veritabanı şeması, bir veya daha fazla Kiracı Kimlik sütunları olması gerekir, böylece verilen tüm Kiracı verileri seçmeli olarak alınabilir değil.  Ayrıca, şemayı birkaç tablolar veya yalnızca bir alt kümesini kiracılar tarafından kullanılan sütunlar gerektirebilir.  Ancak, statik kod ve başvuru verileri yalnızca bir kere depolanır ve tüm kiracılar tarafından paylaşılır.

#### <a name="tenant-isolation-is-sacrificed"></a>Kiracı yalıtımı feda

*Veri:* &nbsp; çok Kiracı veritabanı Kiracı yalıtımı mutlaka gözden çıkarır.  Birden çok kiracıya veri birlikte bir veritabanında depolanır.  Geliştirme sırasında sorguları hiçbir zaman birden çok Kiracı verilerden kullanıma emin olun.  SQL veritabanı destekler [satır düzeyi güvenlik][docu-sql-svr-db-row-level-security-947w], bir sorgudan döndürülen verilerin zorunlu kılabilir tek bir kiracı kapsamı.

*İşlem:* &nbsp; çok Kiracı veritabanı işlem ve depolama kaynaklarını, kiracılar arasında paylaşır.  Veritabanı bir bütün olarak çalışarak gerçekleştirme emin olmak için izlenebilir.  Ancak, Azure sistem izlemek veya bu kaynakları tek bir kiracıya tarafından kullanımını yönetmek için yerleşik hiçbir yolu yoktur.  Bu nedenle, çok Kiracı veritabanı artan bir burada performans deneyimi diğer kiracıların aynı veritabanında bir overactive Kiracı İş yükünü etkiler gürültülü komşu karşılaşmadan riski taşır.  Uygulama düzeyi ek izleme Kiracı düzeyindeki Performans İzleyici.

#### <a name="lower-cost"></a>Düşük maliyet

Genel olarak, birden çok Kiracı veritabanı en düşük başına maliyet Kiracı sahip.  Bir tek başına veritabanı için kaynak maliyetleri eşdeğer boyutlu esnek havuz için daha düşük.  Buna ek olarak, kiracılar yalnızca sınırlı depolama burada gerektiği senaryolar için büyük olasılıkla kiracılar milyonlarca tek bir veritabanında depolanabilir.  Esnek havuz olmaması, veritabanları milyonlarca içerebilir.  Ancak, 1000 havuzlarıyla havuz başına 1000 veritabanlarını içeren bir çözüm yönetmek oldukça karmaşık bir görünüme olma at the risk of milyonlarca ölçeğini ulaşabilir.

Bir çok Kiracı veritabanı modelin iki çeşit, çok kiracılı parçalı modeli en esnek ve ölçeklenebilir aşağıda içinde ele alınmıştır.

## <a name="e-multi-tenant-app-with-a-single-multi-tenant-database"></a>E. Tek bir çok Kiracı veritabanı ile çok kiracılı uygulama

En basit çok Kiracı veritabanı düzeni verileri barındırmak için bir tek tek başına veritabanı tüm kiracılar için kullanır.  Daha fazla kiracılar eklendikçe veritabanı ile daha fazla depolama ve işlem kaynakları ölçeklenir.  Her zaman bir ultimate ölçek sınır olmasa bu ölçek büyütme gereklidir, tüm olabilir.  Ancak, veritabanı bu sınırı ulaşılmadan uzun yönetmek oldukça karmaşık bir görünüme haline gelir.

Tek tek kiracılar odaklanmış yönetim işlemlerini çok Kiracı veritabanı uygulamak için daha karmaşıktır.  Ve ölçekte bu işlem edilemeyecek yavaş olabilir.  Zaman içinde nokta geri yükleme yalnızca bir kiracı için verilerin bir örnektir.

## <a name="f-multi-tenant-app-with-sharded-multi-tenant-databases"></a>F Çok kiracılı uygulama parçalı çok Kiracı veritabanı ile

Birçok SaaS uygulamaları aynı anda yalnızca bir kiracı verilere.  Veya bir parça parça, burada herhangi biri için tüm veriler Kiracı yer alan birden çok veritabanı arasında dağıtılacak Kiracı verilerini bu erişim düzeni sağlar.  Çok Kiracı veritabanı deseni ile birleştirildiğinde, neredeyse sınırsız ölçek parçalı bir modeli sağlar.

![Parçalı çok Kiracı veritabanları ile çok kiracılı uygulama tasarımını.][image-mt-app-sharded-mt-db-174s]

#### <a name="manage-shards"></a>Parça yönetme

Parçalama karmaşıklık hem tasarım ve işlemsel yönetim ekler.  Bir katalog, kiracılar ve veritabanları arasında eşleme korumak gereklidir.  Ayrıca, yönetimi yordamları, parça ve Kiracı popülasyon yönetmek için gereklidir.  Örneğin, yordamlar parça ekleyip ve Kiracı veri parça arasında taşımak için tasarlanmış olması gerekir.  Ölçeklendirmek için bir için yeni bir parça ekleyerek ve yeni kiracılar ile doldurma yoludur.  Diğer saatlerde daha az densely doldurulan iki parça densely doldurulan bir parça bölme.  Çeşitli kiracılar taşınmış veya kullanımdan sonra seyrek doldurulmuş parça birlikte birleştirme.  Birleştirme daha fazla ekonomik kaynak kullanımı neden olur.  Kiracılar ayrıca dengelemeye parça arasındaki taşınmış olabilir.

SQL veritabanı parçalama kitaplığı ve Katalog veritabanı ile birlikte çalışan bir bölme/merge araç sağlar.  Sağlanan uygulama bölme ve birleştirme parça ve Kiracı veri parça arasında taşıyabilirsiniz.  Uygulamanın, ayrıca kiracılar bunları taşıma önce çevrimdışı olarak işaretleme bu işlemleri sırasında katalog etkilenen korur.  Taşıma sonrasında uygulama Kataloğu yeniden yeni eşleme ve Kiracı tekrar çevrimiçi olarak işaretleme ile güncelleştirir.

#### <a name="smaller-databases-more-easily-managed"></a>Daha küçük veritabanları daha fazla kolayca yönetilen

Kiracılar birden çok veritabanı arasında dağıtarak parçalı çok kiracılı çözüm daha kolay yönetilir küçük veritabanlarında sonuçlanır.  Örneğin, belirli bir kiracı şimdi zaman içinde önceki bir noktaya geri yükleme tek bir küçük veritabanı tüm kiracılar içeren daha büyük bir veritabanı yerine bir yedekleme geri içerir. Veritabanı boyutu ve veritabanı başına Kiracı sayısı iş yükü ve Yönetimi çalışmalarını dengelemek için seçilebilir.

#### <a name="tenant-identifier-in-the-schema"></a>Şemadaki Kiracı tanımlayıcısı

Kullanılan parçalama yaklaşım bağlı olarak ek kısıtlamalar veritabanı şemasını temel neden.  SQL veritabanı bölünmüş/merge uygulama şema genellikle Kiracı tanımlayıcısıdır parçalama anahtarı içerdiğini gerektirir.  Önde gelen tüm parçalı tabloların birincil anahtar öğesinde Kiracı tanımlayıcısıdır.  Kiracı tanımlayıcı hızla bulup belirli bir kiracı ile ilişkilendirilen veri taşımak bölünmüş/merge uygulama sağlar.

#### <a name="elastic-pool-for-shards"></a>Esnek havuz parça için

Esnek havuzlarını parçalı çok Kiracı veritabanı yerleştirilebilir.  Genel olarak, maliyet çok sayıda Kiracı birkaç çok kiracılı veritabanlarında sahip olarak verimli bir havuzda çok sayıda tek Kiracı veritabanları sahip istenir.  Çok sayıda görece etkin olmayan kiracılar olduğunda çok Kiracı veritabanı avantaj sağlar.

## <a name="g-hybrid-sharded-multi-tenant-database-model"></a>G. Karma parçalı çok Kiracı veritabanı modeli

Karma modelinde tüm veritabanları kendi şemasında Kiracı tanımlayıcısına sahip.  Veritabanlarını birden fazla Kiracı depolayabilen tüm özellikli ve veritabanlarını parçalı olabilir.  Böylece şema anlamda tüm çok Kiracı veritabanı oldukları.  Henüz uygulamada bu veritabanlarından bazıları yalnızca bir kiracı içerir.  Ne olursa olsun, belirli bir veritabanında depolanan kiracılar miktarını veritabanı şeması üzerinde etkisi yoktur.

#### <a name="move-tenants-around"></a>Kiracılar hareket etme

Herhangi bir zamanda, belirli bir kiracı kendi çok kiracılı veritabanına taşıyabilirsiniz.  Ve herhangi bir zamanda fikrinizi değiştirirseniz ve Kiracı birden çok kiracıya içeren bir veritabanına geri dönün.  Yeni veritabanı sağladığınızda, bir kiracı yeni tek Kiracı veritabanına atayabilirsiniz.

Kaynak gereksinimlerini kiracılar tanımlama grupları arasında büyük farklar olduğunda karma modeli inanılmaz.  Örneğin, ücretsiz bir deneme katılan kiracıların abone kiracılar olan performans aynı yüksek düzeyde garanti varsayalım.  İlke, tüm ücretsiz deneme kiracılar arasında paylaşılan bir çok kiracılı veritabanında depolanması için ücretsiz deneme aşamasında kiracılar için olabilir.  Temel hizmet düzeyi için ücretsiz bir deneme Kiracı abone olduğunda, Kiracı daha az kiracılar olabilecek başka bir çok kiracılı veritabanına taşınabilir.  Premium hizmet düzeyi ödeyen abone kazanılan yeni tek Kiracı veritabanına taşınamıyor.

#### <a name="pools"></a>Havuzlar

Bu karma modelde tek Kiracı veritabanları abone kiracılar için Kiracı başına veritabanınızın maliyetlerini azaltmak için kaynak havuzları yerleştirilebilir.  Bu ayrıca Kiracı başına veritabanı modelinde gerçekleştirilir.

## <a name="h-tenancy-models-compared"></a>H. Karşılaştırılan kiralama modelleri

Aşağıdaki tabloda ana kiralama modelleri arasındaki farklar özetlenmektedir.

| Ölçüm | Tek başına uygulama | Kiracı başına veritabanı | Parçalı çok kiracılı |
| :---------- | :------------- | :------------------ | :------------------- |
| Ölçek | Orta<br />1-100s | Çok yüksek<br />1-100,000s | Sınırsız<br />1-1.000.000'luk bloklar |
| Kiracı yalıtımı | Çok yüksek | Yüksek | Düşük; (yani bir MT db tek başına) tüm singleton Kiracı dışında. |
| Kiracı başına veritabanı maliyet | Yüksek; yükselmeleri için boyutlandırılır. | Düşük; kullanılan havuzları. | En düşük, MT DBs küçük kiracılar için. |
| Performans izleme ve yönetim | Her Kiracı yalnızca | Toplama + Kiracı başına | Toplama; ancak başına-Kiracı yalnızca teklileri için. |
| Geliştirme karmaşıklık | Düşük | Düşük | Orta; Parçalama nedeniyle. |
| İşletim karmaşıklığını | Düşük yüksek. Tek tek basit karmaşık ölçekte. | Düşük-Orta. Desenler ölçekte karmaşıklık adres. | Düşük yüksek. Bireysel Kiracı Yönetimi karmaşıktır. |
| &nbsp; ||||

## <a name="next-steps"></a>Sonraki adımlar

- [Dağıtma ve veritabanı başına Kiracı SaaS model - Azure SQL Database kullanan çok kiracılı Wingtip uygulamayı keşfedin][docu-sql-db-saas-tutorial-deploy-wingtip-db-per-tenant-496y]

- [Wingtip biletleri örnek SaaS Azure SQL veritabanı kiralama uygulaması'na Hoş Geldiniz][docu-saas-tenancy-welcome-wingtip-tickets-app-384w]


<!--  Article link references.  -->

[http-visual-studio-devops-485m]: https://www.visualstudio.com/devops/

[docu-sql-svr-db-row-level-security-947w]: https://docs.microsoft.com/sql/relational-databases/security/row-level-security

[docu-elastic-db-client-library-536r]: sql-database-elastic-database-client-library.md
[docu-sql-db-saas-tutorial-deploy-wingtip-db-per-tenant-496y]: sql-database-saas-tutorial.md
[docu-sql-db-automatic-tuning-771a]: sql-database-automatic-tuning.md
[docu-saas-tenancy-welcome-wingtip-tickets-app-384w]: saas-tenancy-welcome-wingtip-tickets-app.md


<!--  Image references.  -->

[image-standalone-app-st-db-111a]: media/saas-tenancy-app-design-patterns/saas-standalone-app-single-tenant-database-11.png "Tek tek Kiracı veritabanı ile tek başına uygulama tasarımını."

[image-mt-app-db-per-tenant-132d]: media/saas-tenancy-app-design-patterns/saas-multi-tenant-app-database-per-tenant-13.png "Kiracı başına veritabanı ile çok kiracılı uygulama tasarımını."

[image-mt-app-db-per-tenant-pool-153p]: media/saas-tenancy-app-design-patterns/saas-multi-tenant-app-database-per-tenant-pool-15.png "Veritabanı başına-esnek havuz kullanarak Kiracı, çok kiracılı uygulamayla tasarımını."

[image-mt-app-sharded-mt-db-174s]: media/saas-tenancy-app-design-patterns/saas-multi-tenant-app-sharded-multi-tenant-databases-17.png "Parçalı çok Kiracı veritabanları ile çok kiracılı uygulama tasarımını."

