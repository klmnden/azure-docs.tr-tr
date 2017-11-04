---
title: "Tasarım desenleri çok kiracılı SaaS uygulamaları ve Azure SQL veritabanı için | Microsoft Docs"
description: "Gereksinimler ve ortak verileri bulut ortamında çalışan bir hizmet (SaaS) veritabanı uygulamaları olarak çok kiracılı yazılım mimarisi desenlerini öğrenin."
keywords: 
services: sql-database
documentationcenter: 
author: srinia
manager: jhubbard
editor: 
ms.assetid: 1dd20c6b-ddbb-40ef-ad34-609d398d008a
ms.service: sql-database
ms.custom: scale out apps
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: Active
ms.date: 02/01/2017
ms.author: srinia
ms.openlocfilehash: eef48cfcbc7d6c241b5ece863df0be6ecad78ca7
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="design-patterns-for-multi-tenant-saas-applications-and-azure-sql-database"></a>Tasarım desenleri çok kiracılı SaaS uygulamaları ve Azure SQL veritabanı
Bu makalede, bulut ortamında çalışan bir hizmet (SaaS) veritabanı uygulamaları olarak çok kiracılı yazılım mimarisi desenlerini genel verilere ve gereksinimleri hakkında öğrenebilirsiniz. Ayrıca, göz önünde bulundurmanız gereken Etkenler ve dengelemeler farklı tasarım desenleri açıklar. Esnek havuzlar ve esnek araçları Azure SQL veritabanı'nda, diğer hedefleri ödün vermeden belirli gereksinimlerinizi karşılayacak yardımcı olabilir.

Geliştiriciler bazen kiralama modelleri için veri katman çok kiracılı uygulama tasarlarken, uzun vadeli en iyi ilgi alanlarına göre çalışan seçimleri yapın. Başlangıçta, en az bir geliştirici geliştirme ve Kiracı yalıtımı daha önemli olarak bulut hizmeti sağlayıcısı maliyetlerini kolaylığı ya da bir uygulama ölçeklenebilirliğini bakışını. Bu seçimi daha sonra müşteri memnuniyetini sorunları ve pahalı indirmelere-düzeltme açabilir.

Çok kiracılı uygulama bulut ortamında bulunan bir uygulamasıdır ve Hizmetleri yüzlerce veya binlerce kimin etmeyin paylaşma veya diğer kişilerin verileri görmek kiracılar için aynı kümesini sağlar. Bulut tarafından barındırılan bir ortamda kiracıların hizmetleri sağlayan bir SaaS uygulaması örneğidir.

## <a name="multi-tenant-applications"></a>Çok kiracılı uygulamalar
Çok kiracılı uygulamalara veri ve iş yükü kolayca bölümlenebilir. Bir kiracı sınırlar içinde en çok istekte gerçekleştirildiği için verileri ve iş yükü, örneğin, Kiracı bölümlere bölüm. Bu veriler ve iş yükü devredilen bir özelliktir ve bu makalede ele alınan uygulama düzenleri korur.

Geliştiriciler, bulut tabanlı uygulamalar dahil olmak üzere tüm yelpazesini bu tür bir uygulama kullanın:

* SaaS uygulamaları olarak buluta nasıl geçirileceğini ortak veritabanı uygulamaları
* Sıfırdan bulut için yerleşik SaaS uygulamaları
* Doğrudan, müşteri dönük uygulamaları
* Çalışanla yüz yüze gelinen kurumsal uygulamalar

Bulut ve kökleri olanlar için tasarlanmıştır SaaS uygulamaları veritabanı uygulamaları genellikle iş ortağı olarak, çoklu kiracı uygulamalardır. Bu SaaS uygulamaları kiracıları için özelleştirilmiş yazılım uygulamayı bir hizmet olarak sunar. Kiracılar, uygulama hizmeti erişebilir ve uygulamanın bir parçası depolanan ilişkili verilerin tam sahiplik. Ancak SaaS avantajlarından yararlanmak için kiracılar kendi veriler üzerinde bazı denetim surrender gerekir. Bunlar verilerine güvenli ve yalıtılmış diğer kiracılar verileri tutmak için SaaS hizmet sağlayıcısı güven. Bu tür bir çok kiracılı SaaS uygulaması MYOB, SnelStart ve Salesforce.com gösterilebilir. Bu uygulamaların her biri Kiracı sınırları ve Destek uygulamayı tasarım desenleri bu makalede aşağıdakiler ele boyunca bölümlenebilir.

Müşterilere veya çalışanlara (genellikle için kiracılar yerine kullanıcılar adlandırılır) bir kuruluştaki doğrudan hizmet sağlayan çok kiracılı uygulama spektrumun başka bir kategoride uygulamalardır. Müşteriler, hizmete abone olmak ve hizmet sağlayıcısı toplar ve depolar veri sahibi değilsiniz. Hizmet sağlayıcıları birbirinden kamu zorunlu ilke yönetmeliklerini yalıtılmış müşterilerin verileri tutmak için daha az sıkı gereksinimleri vardır. Bu tür bir müşteri dönük çok kiracılı uygulama medya içerik sağlayıcıları Netflix, Spotify ve Xbox LIVE gibi gösterilebilir. Müşteri dönük kolayca bölümlenebilir uygulamaların diğer örnekler, Internet ölçekli uygulamalar ya da her hangi müşteri veya aygıt can nesnelerin interneti (IOT) uygulamalarında bir bölümü olarak hizmet. Bölüm sınırları, kullanıcıların ve aygıtların ayırabilirsiniz.

Kiracı, müşteri, kullanıcı veya aygıt gibi tek bir özellik kolayca boyunca tüm uygulamaları bölüm. (ERP) uygulama planlaması karmaşık kurumsal kaynak, örneğin, ürünler, siparişler ve müşteriler sahiptir. Genellikle, yüksek oranda birbirine bağlı tabloları binlerce ile karmaşık bir şeması vardır.

Tek bölüm stratejisi yok tüm tablolara uygulamak ve bir uygulamanın iş yükü arasında çalışabilirsiniz. Bu makalede kolayca bölümlenebilir veri ve iş yüklerine sahip birden çok kiracılı uygulamalara odaklanılmaktadır.

## <a name="multi-tenant-application-design-trade-offs"></a>Çok kiracılı uygulama tasarım dengelemeler
Çok kiracılı uygulama geliştiricisi genellikle seçer tasarım deseni aşağıdaki etmenlere dikkat etmeniz üzerinde dayanır:

* **Kiracı yalıtımı**. Geliştirici Kiracı istenmeyen diğer kiracılar veri erişimi olduğundan emin olması gerekir. Gürültülü komşu koruma sağlayan, bir kiracının verileri geri yüklemek mümkün ve Kiracı özgü özelleştirmeler uygulamak gibi diğer özelliklere yalıtım gereksinim genişletir.
* **Bulut kaynak maliyeti**. Bir SaaS uygulaması maliyet açısından rekabetçi olması gerekir. Çok kiracılı uygulama geliştiricisi daha düşük maliyetli depolama alanı gibi bulut kaynakları kullanımında en iyi duruma getirme ve işlem maliyetleri seçebilirsiniz.
* **DevOps kolaylığı**. Yalıtım koruma birleştirmek, Bakımı ve kendi uygulama ve veritabanı şeması sağlığını izlemek ve Kiracı sorunlarını gidermek çok kiracılı uygulama geliştiricisi gerekir. Uygulama geliştirme ve işlem karmaşıklığı doğrudan artan maliyet çevirir ve Kiracı tatmin sınar.
* **Ölçeklenebilirlik**. Daha fazla kiracılar ve kapasite gerektiren kiracılar için artımlı olarak ekleme yeteneği başarılı bir SaaS işlem zorunludur.

Faktörlerin her biri için başka bir karşılaştırma dengelemeler sahiptir. En düşük maliyeti bulut en kolay geliştirme deneyimi sunabilir değil teklifi. Uygulamasının Tasarım işlemi sırasında bu seçenekleri ve bunların dengelemeler hakkında bilinçli seçimleri yapmak bir geliştirici önemlidir.

Popüler geliştirme desen bir veya birkaç veritabanlarına birden çok kiracıya paketi sağlamaktır. Bu yaklaşımın avantajları daha düşük maliyetli bir olduklarından bazı veritabanları ve sınırlı sayıda veritabanları ile çalışmanın göreli kolaylık sağlaması için ödeme yaparsınız. Ancak, zaman içinde bir SaaS çok kiracılı uygulama geliştiricisinin bu seçenek Kiracı yalıtımı ve ölçeklenebilirlik önemli downsides olduğunu unutmayın. Kiracı yalıtımı önemli hale gelirse, yetkisiz erişime veya gürültülü komşu paylaşılan depolama alanında Kiracı verilerini korumak için ek çaba gerekir. Bu ek çaba, geliştirme çalışmalarını ve yalıtım bakım maliyetlerini önemli ölçüde artırabilir. Benzer şekilde, kiracılar eklenmesi gerekiyorsa, bu tasarım deseni genellikle Kiracı verilerini veri katmanı uygulamasının düzgün için veritabanları arasında yeniden dağıtmak için uzmanlık gerektirir.  

Genellikle Kiracı yalıtımı, işletmelere ve kuruluşlara karşılamak SaaS çok kiracılı uygulamalarda temel bir gereksinimdir. Bir geliştirici tarafından algılanan avantajları Basitlik ve maliyet Kiracı yalıtımı ve ölçeklenebilirlik tempted. Bu dengelemeyi karmaşık ve pahalı hizmet büyüdükçe ve Kiracı yalıtımı gereksinimleri hale gibi daha önemli ve yönetilen uygulama katmanında kanıtlayabilirsiniz. Ancak, müşterilere doğrudan, tüketiciye yönelik bir hizmeti sağlayan çok kiracılı uygulamalara, Kiracı yalıtımı için bulut kaynak maliyetini en iyi duruma getirme değerinden daha düşük bir öncelik olabilir.

## <a name="multi-tenant-data-models"></a>Çok kiracılı veri modelleri
Kiracı verileri yerleştirmek için ortak tasarım uygulamaları Şekil 1'de gösterilen üç ayrı modeli izleyin.

![çok kiracılı uygulama veri modelleri](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-multi-tenant-data-models.png)

Şekil 1: Çok kiracılı veri modelleri için ortak tasarım yöntemler

* **Kiracı başına veritabanı**. Her bir kiracı kendi veritabanına sahip. Tüm Kiracı özgü verileri kiracının veritabanına sınırlı ve diğer kiracılar ve bunların verilerinden yalıtılır.
* **Veritabanı parçalı paylaşılan**. Birden çok Kiracı birden çok veritabanlarından birini paylaşır. Kiracılar ayrı bir dizi karma, aralığı veya liste bölümleme gibi bölümleme stratejisine kullanarak her bir veritabanı için atanır. Bu veri Dağıtım stratejisi, genellikle parçalama da adlandırılır.
* **Veritabanı tek paylaşılan**. Tek, bazen büyük bir veritabanı, bir kiracı Kimlik sütununda disambiguated tüm kiracılar için verileri içerir.

> [!NOTE]
> Bir uygulama geliştiricisi farklı kiracıların farklı veritabanı şemalarını yerleştirin ve farklı kiracıların belirsizliğini ortadan kaldırmak için şema adı kullanmak isteyebilirsiniz. Dinamik SQL kullanımı genellikle gerektirdiğinden ve planı önbelleğe alma etkin olamaz bu yaklaşım önerilmez. Bu makalenin sonraki bölümlerinde Biz bu kategori, çok kiracılı uygulama için paylaşılan tablo yaklaşım odaklanılmaktadır.
> 
> 

## <a name="popular-multi-tenant-data-models"></a>Popüler çok kiracılı veri modelleri
Uygulama tasarımı zaten belirledik dengelemeler açısından çok kiracılı veri modelleri farklı türde değerlendirmek önemlidir. Bu etkenler daha önce açıklanan üç yaygın çok kiracılı veri modeli ve Şekil 2'de gösterildiği gibi veritabanı kullanımı niteleyen yardımcı olur.

* **Yalıtım**. Kiracılar arasında yalıtım düzeyini bir veri modeli başarır ne kadar Kiracı yalıtımı ölçüsü olabilir.
* **Bulut kaynak maliyeti**. Kaynak kiracılar arasında paylaşımı miktarı, bulut kaynak maliyeti en iyi duruma getirebilirsiniz. Bir kaynak işlem ve depolama maliyeti olarak tanımlanabilir.
* **DevOps maliyet**. Uygulama geliştirme, dağıtım ve yönetilebilirlik kolaylığı genel SaaS işlemi maliyeti azaltır.  

Şekil 2'de Y ekseni Kiracı yalıtımı düzeyini gösterir. X ekseni kaynak paylaşma düzeyini gösterir. Gri ortasında çapraz ok artırmak veya azaltmak için eğilimi gösterir, DevOps maliyetleri yönünü belirtir.

![Popüler çok kiracılı uygulama tasarım desenleri](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-popular-application-patterns.png)

Şekil 2: Popüler çok kiracılı veri modelleri

Şekil 2'deki sağ alt çeyreğinde potansiyel olarak büyük, kullanan bir uygulama düzeni gösterilir paylaşılan tek veritabanı ve paylaşılan tablosu (veya ayrı şema) yaklaşım. Kaynak tüm kiracılar tek bir veritabanında aynı veritabanı kaynakları (CPU, bellek, giriş/çıkış) kullandığından paylaşımı uygundur. Ancak, Kiracı yalıtımı sınırlıdır. Kiracılar birbirinden uygulama katmanında korumak için ek adımlar gerekebilir. Aşağıdaki ek adımları geliştirmek ve uygulama yönetmek DevOps maliyetini önemli ölçüde artırabilir. Veritabanını barındıran donanım ölçek tarafından sınırlı ölçeklenebilirlik.

Şekil 2'deki sol alt Dörtgen Bölümlü parçalı birden çok Kiracı arasında birden fazla veritabanı gösterilmektedir (genellikle, farklı donanım ölçek birimleri). Her veritabanını diğer desenleri ölçeklenebilirlik sorunu giderir kiracılar, bir alt barındırır. Daha fazla kiracılar için daha fazla kapasite gerekiyorsa, kiracılar yeni veritabanları yeni donanım ölçek birimi için ayrılan üzerinde kolayca yerleştirebilirsiniz. Ancak, kaynak paylaşma miktarı azalır. Yalnızca kiracılar aynı ölçek birimleri üzerinde paylaşım kaynaklar yerleştirilir. Bu yaklaşım, çok sayıda Kiracı hala otomatik olarak diğer kişilerin eylemlerine korunan olmadan birlikte bulunan için Kiracı yalıtımını için çok az geliştirme sağlar. Uygulama karmaşıklığını yüksek kalır.

Şekil 2'deki sol üst Dörtgen Bölümlü üçüncü bir yaklaşımdır. Her bir kiracının veri kendi veritabanında yerleştirir. Bu yaklaşım iyi Kiracı yalıtımı özelliklere sahiptir ancak her veritabanı kendi özel kaynakları olduğunda kaynak paylaşma sınırlar. Bu yaklaşım tüm kiracılar tahmin edilebilir iş yükleriniz varsa iyi olur. Kiracı İş yükleri daha az öngörülebilir hale gelirse, sağlayıcı kaynak paylaşma iyileştirilemiyor. Karşılaştırıldığında, SaaS uygulamaları için yaygın bir durumdur. Sağlayıcı taleplerini karşılamak üzere atlayarak sağlamak ya da alt kaynakları gerekir. Her iki eylem sonuçlarını daha yüksek maliyetleri ya da daha düşük Kiracı memnuniyet. Bir kaynak kiracılar arasında paylaşımı daha yüksek derecede çözümü daha düşük maliyetli hale arzu haline gelir. Veritabanlarının sayısını artırmayı DevOps dağıtma ve uygulama bakım maliyeti artırır. Bu sorunları rağmen bu yöntem, kiracılar için en iyi ve en kolay yalıtım sağlar.

Bu etkenler ayrıca bir müşteri seçer tasarım deseni etkiler:

* **Kiracı veri sahipliğini**. Kiracıların kendi veri sahipliğini korumak uygulama düzeni Kiracı başına tek bir veritabanının korur.
* **Ölçek**. Binlerce veya kiracılar milyonlarca yüzlerce hedefleyen bir uygulama veritabanı parçalama gibi yaklaşımlar paylaşımı korur. Yalıtım gereksinimleri hala yol açabilir.
* **Değer ve iş modeli**. Bir uygulama, kullanıcının her Kiracı gelir küçük (değerinden ABD Doları), daha az önemli hale yalıtımı gereksinimleri ve paylaşılan bir veritabanı algılama yapıyorsa. Kiracı başına gelir birkaç dolar veya daha fazla ise, bir kiracı başına veritabanı daha uygun modelidir. Geliştirme maliyetleri azaltmasına yardımcı olabilir.

Tasarım Şekil 2'de gösterilen dengelemeler göz önüne alındığında, en iyi kaynak kiracılar arasında paylaşımı ile iyi Kiracı yalıtımı özellikleri içerecek şekilde ideal çok kiracılı bir model gerekir. Bu model, Şekil 2 sağ üst köşede çeyreğinde içinde açıklanan kategorisinde sığar.

## <a name="multi-tenancy-support-in-azure-sql-database"></a>Azure SQL veritabanı çoklu kiracı desteği
Azure SQL veritabanı Şekil 2'de gösterilen tüm çok kiracılı uygulama desenleri destekler. Esnek havuzları ile Kiracı başına veritabanı yalıtım avantajları (bkz. Şekil 3'te sağ üst köşede Dörtgen Bölümlü) yaklaşmak ve iyi kaynak paylaşma birleştiren bir uygulama düzeni de destekler. Esnek veritabanı araçlarını ve SQL veritabanı özelliklerini geliştirmek ve (gölgeli alana Şekil 3'te görünür) birçok veritabanı olan bir uygulama çalıştırmak için maliyetini azaltmaya yardımcı olmak. Bu araçlar, yapı ve birden çok veritabanı desenleri birini kullanan uygulamaları yönetmenize yardımcı olabilir.

![Azure SQL veritabanı düzenleri](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-patterns-sqldb.png)

Şekil 3: Çok kiracılı uygulama düzenleri Azure SQL veritabanı

## <a name="database-per-tenant-model-with-elastic-pools-and-tools"></a>Kiracı başına veritabanı modeliyle esnek havuzlar ve araçları
Esnek havuzlar SQL veritabanındaki Kiracı yalıtımı ile daha iyi desteklemek için Kiracı veritabanları arasında Kiracı başına veritabanı yaklaşım Paylaşımı kaynak birleştirin. SQL veritabanı bir çok kiracılı uygulamalara yapı SaaS sağlayıcısı için veri katmanı çözümüdür. Kaynak kiracılar arasında paylaşımı yük uygulama katmanından veritabanı hizmet katmanına kaydırır. Yönetme ve veritabanları arasında ölçekte sorgulama karmaşıklığını esnek işler, esnek sorgu, esnek işlemleri ve esnek veritabanı istemci kitaplığı ile basitleştirilmiştir.

| Uygulama gereksinimleri | SQL veritabanı özellikleri |
| --- | --- |
| Kiracı yalıtımı ve kaynak paylaşma |[Esnek havuzlar](sql-database-elastic-pool.md): SQL veritabanı kaynak havuzu ayırma ve kaynakları çeşitli veritabanları arasında paylaşın. Ayrıca, tek veritabanlarını kadar kaynak havuzundan kapasite talep ani Kiracı İş yükleri değişiklikleri nedeniyle uyum için gereken çizebilirsiniz. Esnek havuz yukarı veya aşağı gerektiği şekilde genişletilebilir. Esnek havuzlar Ayrıca, yönetilebilirlik ve izleme ve havuzu düzeyinde sorun giderme kolaylığı sağlar. |
| DevOps kolaylığı veritabanları arasında |[Esnek havuzlar](sql-database-elastic-pool.md): daha önce belirtildiği gibi. |
| | [Esnek sorgu](sql-database-elastic-query-horizontal-partitioning.md): Sorgu raporlama veritabanları arasında veya çapraz-Kiracı çözümleme. |
| | [Esnek iş](sql-database-elastic-jobs-overview.md): Paket ve güvenilir bir şekilde birden fazla veritabanı için veritabanı bakım işlemleri veya veritabanı şema değişiklikleri dağıtın. |
| | [Esnek işlemleri](sql-database-elastic-transactions-overview.md): işlem için birden fazla veritabanı atomik ve yalıtılmış bir şekilde değiştirir. Uygulamaları birkaç veritabanı işlemleri "tümü veya hiçbiri" garanti gerektiğinde esnek hareket gerekli değildir. |
| | [Esnek veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md): veri dağıtımları yönetmek ve veritabanlarını kiracıların eşleyin. |

## <a name="shared-models"></a>Paylaşılan modelleri
Birçok SaaS sağlayıcıları için daha önce açıklandığı gibi bir paylaşılan model yaklaşım Kiracı yalıtımı sorunları ile ilgili problemler ve karmaşıklık uygulama geliştirme ve Bakım neden olabilir. Ancak, doğrudan tüketicilere hizmet sağlayan çok kiracılı uygulamalar için Kiracı yalıtımı gereksinimleri maliyeti en aza olarak yüksek bir öncelik olmayabilir. Bunlar, bir veya daha fazla veritabanlarında maliyetlerini azaltmak için yüksek yoğunluklu, kiracılar paketi mümkün olabilir. Paylaşılan veritabanı modelleri tek bir veritabanı veya birden çok parçalı veritabanı kullanan kaynak paylaşma ve genel maliyet ek verimliliği neden olabilir. Azure SQL veritabanı müşterilere yardımcı olun bazı özellikleri için Gelişmiş Güvenlik ve yönetim ölçekli veri katmanındaki yalıtım oluşturmak sağlar.

| Uygulama gereksinimleri | SQL veritabanı özellikleri |
| --- | --- |
| Güvenlik yalıtımı özellikleri |[Satır düzeyi güvenlik](https://msdn.microsoft.com/library/dn765131.aspx) |
| [Veritabanı şeması](https://msdn.microsoft.com/library/dd207005.aspx) | |
| DevOps kolaylığı veritabanları arasında |[Esnek sorgu](sql-database-elastic-query-horizontal-partitioning.md) |
| | [Esnek işler](sql-database-elastic-jobs-overview.md) |
| | [Esnek işlemleri](sql-database-elastic-transactions-overview.md) |
| | [Elastik veritabanı istemci kitaplığı](sql-database-elastic-database-client-library.md) |
| | [Esnek veritabanı bölme ve birleştirme](sql-database-elastic-scale-overview-split-and-merge.md) |

## <a name="summary"></a>Özet
Kiracı yalıtımı gereksinimleri en çok kiracılı SaaS uygulamaları için önemlidir. Yalıtımı sağlamak için en iyi seçenek yoğun doğru Kiracı başına veritabanı yaklaşım leans. Diğer iki yaklaşım, maliyet ve risk önemli ölçüde artırır yalıtımı sağlamak için nitelikli Geliştirme ekibiniz gerektiren karmaşık uygulama katmanları yatırım gerektirir. Yalıtım gereksinimleri erken hizmeti geliştirme katılmayan varsa, bunları retrofitting ilk iki modellerinde daha pahalı olabilir. Kiracı başına veritabanı modeli ile ilişkili ana dezavantajları azaltılmış paylaşma ve bakımını yapma ve birçok veritabanı yönetme nedeniyle artan bulut kaynak maliyetleri ilgilidir. Bu dengelemeler yaptıklarında SaaS uygulama geliştiriciler genellikle güçlük.

Ana engelleri çoğu bulut veritabanı hizmet sağlayıcısı ile dengelemeler olabilir, ancak Azure SQL veritabanı, esnek havuz ve esnek veritabanı özelliklerini engelleri ortadan kaldırır. SaaS geliştiriciler, bir kiracı başına veritabanı modeli yalıtım özelliklerini birleştirmek ve esnek havuzlar ve ilişkili araçlarını kullanarak kaynak paylaşımı ve birçok veritabanı yönetilebilirlik geliştirmeleri en iyi duruma getirme.

Hiçbir Kiracı yalıtımı gereksinimleri sahip ve kimlerin kiracılar yüksek yoğunluklu bir veritabanında paketi çok kiracılı uygulama sağlayıcılarının, paylaşılan veri kaynağı paylaşımı ek verimliliği de sonuç modeller ve genel maliyetini azaltmak bulabilirsiniz. Azure SQL Database esnek veritabanı araçlarını, parçalama kitaplıkları ve güvenlik özellikleri oluşturmak ve yönetmek çok kiracılı uygulamalara SaaS sağlayıcıları yardımcı olur.

## <a name="next-steps"></a>Sonraki adımlar
[Esnek veritabanı araçlarını kullanmaya başlama](sql-database-elastic-scale-get-started.md) istemci Kitaplığı gösteren örnek bir uygulama ile.

Oluşturma bir [SaaS için esnek havuz özel Pano](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) esnek havuzlar için uygun maliyetli ve ölçeklenebilir veritabanı çözümü kullanan bir örnek uygulama ile.

Azure SQL veritabanı araçlarını kullanın [genişletilecek varolan veritabanlarını geçirme](sql-database-elastic-convert-to-use-elastic-tools.md).

Azure Portalı'nı kullanarak bir esnek havuz oluşturmak için bkz: [bir esnek havuz oluşturma](sql-database-elastic-pool-manage-portal.md).  

Bilgi edinmek için nasıl [izlemek ve esnek havuzunu yönetme](sql-database-elastic-pool-manage-portal.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Dağıtma ve Azure SQL veritabanı - Wingtip SaaS kullanan çok kiracılı uygulama keşfedin](sql-database-saas-tutorial.md)
* [Bir Azure esnek havuz nedir?](sql-database-elastic-pool.md)
* [Azure SQL Veritabanı ile ölçek genişletme](sql-database-elastic-scale-introduction.md)
* [çok kiracılı uygulamalarla esnek veritabanı araçlarını ve satır düzeyi güvenlik](sql-database-elastic-tools-multi-tenant-row-level-security.md)
* [Azure Active Directory ve Openıd Connect kullanarak çok kiracılı uygulamalara kimlik doğrulaması](../guidance/guidance-multitenant-identity-authenticate.md)
* [Tailspin Surveys uygulaması](../guidance/guidance-multitenant-identity-tailspin.md)


## <a name="questions-and-feature-requests"></a>Sorular ve özellik istekleri

Sorular için bize Bul [SQL veritabanı Forumu](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted). Bir özellik isteğinde eklemek [SQL veritabanı geri bildirim Forumunda](https://feedback.azure.com/forums/217321-sql-database/).

