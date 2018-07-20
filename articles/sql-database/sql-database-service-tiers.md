---
title: Azure SQL veritabanı'nı satın alma modeli | Microsoft Docs
description: Azure SQL veritabanı için model satın alma hakkında bilgi edinin.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 07/19/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: 33d23de2cd0ddae95a34c2c9f7acabdc7315cd36
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39160051"
---
# <a name="azure-sql-database-purchasing-models-and-resources"></a>Azure SQL veritabanı'nı modelleri ve kaynakları satın alma 

Mantıksal sunucularda [Azure SQL veritabanı](sql-database-technical-overview.md) işlem, depolama ve GÇ kaynakları iki satın alma modeli sunar: DTU tabanlı satın alma modeli ve sanal çekirdek tabanlı satın alma modeli. 

> [!NOTE]
> [Yönetilen örnekler](sql-database-managed-instance.md) yalnızca Azure SQL veritabanı sanal çekirdek tabanlı satın alma modeli sunar.

Aşağıdaki tablo ve grafik karşılaştırın ve bu iki satın alma modeli.

> [!IMPORTANT]
> Sanal çekirdek tabanlı satın alma modeli için bkz: [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md)

|**Satın alma modeli**|**Açıklama**|**En iyi**|
|---|---|---|
|DTU tabanlı model|Bu model, işlem, depolama ve GÇ kaynakları ile birlikte gelen bir ölçüyü temel alır. Performans düzeyleri tek veritabanları için Veritabanı İşlem Birimleri (DTU’lar), elastik havuzlar için de elastik Veritabanı İşlem Birimleri (eDTU’lar) ile ifade edilir. Dtu'lar ve Edtu'lar hakkında daha fazla bilgi için bkz. [Dtu'lar ve Edtu'lar nelerdir](sql-database-service-tiers.md#what-are-database-transaction-units-dtus)?|Basit, önceden yapılandırılmış kaynak seçenekleri isteyen müşteriler için idealdir.| 
|vCore tabanlı model|Bu model, işlem ve depolama kaynaklarını bağımsız olarak ölçeklendirmenize olanak sağlar. Ayrıca, maliyet tasarrufu elde etmek için SQL Server için Azure hibrit avantajı kullanmanıza olanak sağlar.|Esneklik, Denetim ve saydamlık değerini müşteriler için idealdir.|
||||  

![Fiyatlandırma modeli](./media/sql-database-service-tiers/pricing-model.png)

## <a name="vcore-based-purchasing-model"></a>Sanal çekirdek tabanlı satın alma modeli 

Sanal çekirdek, donanım Nesilleri arasında seçim yapma olanağı ile sunulan mantıksal CPU'yu temsil eder. Sanal çekirdek tabanlı satın alma modeli, esneklik, denetimi, bireysel kaynak kullanımının saydamlığı sağlar ve basit bir yol çevirmek için şirket iş yükü gereksinimlerini buluta. Bu model, Ölçek işlem, bellek ve iş yükü gereksinimlerine göre depolama sağlar. Sanal çekirdek tabanlı satın alma modeli, müşterilerin genel amaçlı ve iş kritik hizmet katmanları arasında hem seçebilirsiniz [tek veritabanları](sql-database-single-database-scale.md) ve [elastik havuzlar](sql-database-elastic-pool.md). 

Sanal çekirdek tabanlı satın alma modeli, bağımsız olarak işlem ve depolama kaynaklarının ölçeğini, aynı şirket içi performans ve fiyat iyileştirme sağlar. Veritabanı veya elastik Havuzu'nu sanal çekirdek 300'den fazla DTU dönüştürme kullanırsa maliyetinizi azaltabilir. API'nizi tercih ettiğiniz veya kapalı kalma süresi ile Azure portalını kullanarak dönüştürebilirsiniz. Ancak, dönüştürme gerekli değildir. DTU satın alma modeli, performans ve iş gereksinimlerini karşılıyorsa, onu kullanmaya devam etmek. Sanal çekirdek-model DTU modelden dönüştürmeye karar verirseniz, aşağıdaki kural karşısında kullanarak performans düzeyi seçmeniz gerekir: genel amaçlı katmanında; en az 1 sanal çekirdek standart katmandaki her 100 DTU gerektirir Her Premium katmanda 125 DTU, iş açısından kritik katmanında en az 1 sanal çekirdek gerektirir.

Sanal çekirdek tabanlı satın alma modeli, müşteriler için ödeme yapar:
- İşlem (hizmet katmanı + sanal çekirdek + donanımın sayısı) *
- Tür ve veri ve günlük depolama miktarı 
- IOs ** sayısı
- Yedekleme depolama (RA-GRS) ** 

\* İlk genel önizlemede Gen 4 mantıksal CPU'lar Intel E5-2673 v3 dayalı (Haswell) 2,4 GHz işlemcileri.

\*\* Önizleme sırasında yedeklemeleri ve IOs 7 gün ücretsizdir.

> [!IMPORTANT]
> İşlem, IOs, veri ve günlük depolama, veritabanı veya elastik havuz başına ücretlendirilir. Yedekleme depolama, her veritabanı ücretlendirilir. Yönetilen örnek ücretleri ayrıntılarını başvurmak [Azure SQL veritabanı yönetilen örneği](sql-database-managed-instance.md).
> **Bölge kısıtlamaları:** sanal çekirdek tabanlı satın alma modeli henüz aşağıdaki bölgelerde kullanılabilir değil: Batı Avrupa, Fransa Orta, Birleşik Krallık Güney, UK Batı ve Avustralya Güneydoğu.

## <a name="dtu-based-purchasing-model"></a>DTU tabanlı satın alma modeli

Veritabanı işlem birimi (DTU) temsil eden bir ölçüyle CPU, bellek, okur ve yazar. DTU tabanlı satın alma modeli, bir dizi işlem kaynakları, önceden yapılandırılmış paketleri sunar ve dahil edilen depolama alanı için çeşitli uygulama performans düzeylerini sürücü. Önceden yapılandırılmış bir paket sabit ödemeler ve Basitlik, her ay tercih eden müşteriler bulun DTU tabanlı model ihtiyaçları için daha uygun. DTU tabanlı satın alma modeli, müşteriler arasında seçim yapabilirsiniz **temel**, **standart**, ve **Premium** hizmet katmanları için her ikisi de [tek veritabanları](sql-database-single-database-scale.md) ve [elastik havuzlar](sql-database-elastic-pool.md). 

### <a name="what-are-database-transaction-units-dtus"></a>Veritabanı işlem birimleri (Dtu) nedir?
İçinde bir belirli bir performans düzeyinde tek bir Azure SQL veritabanı için bir [hizmet katmanı](sql-database-single-database-scale.md), Microsoft garanti kaynakları (bağımsız olarak herhangi bir veritabanı Azure bulutunda) Bu veritabanı, belirli bir düzeyde sağlayan bir tahmin edilebilir performans düzeyi. Kaynakların miktarını, veritabanı işlem birimleri veya Dtu'lar sayısı hesaplanır ve işlem, depolama ve GÇ kaynakları ile birlikte gelen bir ölçüdür. Bu kaynaklar arasındaki oran başlangıçta tarafından belirlenen bir [OLTP Kıyaslama iş yükünün](sql-database-benchmark-overview.md), gerçek OLTP iş yükleri tipik olacak şekilde tasarlanmıştır. İş yükünüz şu kaynaklara miktarı aşarsa, aktarım hızınızı daraltılmış - yavaş performans ve zaman aşımları sonuç. İş yükünüz tarafından kullanılan kaynakları başka bir SQL veritabanına Azure bulutunda kullanılabilir kaynakları etkilemez ve diğer iş yükleri tarafından kullanılan kaynakları SQL veritabanınıza kullanılabilir kaynakları etkilemez.

![sınırlayıcı kutu](./media/sql-database-what-is-a-dtu/bounding-box.png)

Dtu en kaynakları farklı performans düzeylerinde Azure SQL veritabanı hizmet katmanları arasındaki göreli miktarını anlamak için kullanışlıdır. Örneğin, bir veritabanının performans düzeyini artırarak dtu'ları Katlama konusu veritabanının kullanabileceği kaynakları kümesi Katlama için karşılık gelmektedir. Örneğin, 1750 DTU’ya sahip Premium P11 veritabanı 5 DTU’ya sahip Temel veritabanına göre 350 kat daha fazla DTU işlem gücü sağlıyor.  

İş yükünüz kaynak (DTU) kullanımını daha derin bir anlayış kazanmak için kullanın [Azure SQL veritabanı sorgu performansı İçgörüleri](sql-database-query-performance.md) için:

- En sık kullanılan sorgular, potansiyel olarak daha iyi performans için ayarlanmış CPU/süresi/yürütme sayısına göre belirleyin. Örneğin, bir g/ç kullanımı yoğun sorgu kullanımından yararlanabilir [bellek içi iyileştirme teknikleri](sql-database-in-memory.md) bir belirli hizmet katmanını ve performans düzeyinde kullanılabilir belleği daha iyi kullanılmasını sağlamak için.
- Bir sorgunun ayrıntılarına detaya, kendi metin ve kaynak kullanımı geçmişini görüntüleyin.
- Erişim performans tarafından gerçekleştirilen eylemleri göster önerilerinde [SQL veritabanı Danışmanı](sql-database-advisor.md).

Değiştirebileceğiniz [DTU hizmet katmanları](sql-database-service-tiers-dtu.md) (genellikle ortalama dört saniyenin altında) uygulamanızda çok az kesinti ile dilediğiniz zaman. Veritabanı oluşturabilmek ve veritabanı performansını isteğe göre yükseltip düşürebilmek, özellikle kullanım biçimlerinin nispeten tahmin edilebilir olduğu durumlarda birçok işletme ve uygulama için yeterlidir. Ancak tahmin edilemeyen kullanım biçimlerine sahipseniz bu durum maliyetlerin ve iş modelinizin yönetimini zorlaştırabilir. Bu senaryoda, belirli bir sayıda Edtu havuzunda birden fazla veritabanı arasında paylaşılan ile elastik havuz kullanın.

![SQL Veritabanı'na Giriş: Katmana ve düzeye göre tek veritabanı DTU’ları](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

### <a name="what-are-elastic-database-transaction-units-edtus"></a>Elastik veritabanı işlem birimleri (Edtu'lar) nedir?
Bunun yerine her zaman kullanılabilir bir SQL veritabanı için her zaman gerekli değildir (Dtu) kaynakları ayrılmış bir dizi sağlamak daha veritabanlarına yerleştirebilirsiniz bir [elastik havuz](sql-database-elastic-pool.md) bir SQL veritabanı sunucusundaki kaynakları arasında bir havuzu paylaşır Bu veritabanları. Bir elastik havuzdaki paylaşılan kaynakları, elastik veritabanı işlem birimleri veya Edtu'lar tarafından ölçülür. Elastik havuzlar, birden çok veritabanına sahip son derece farklı ve öngörülemeyen kullanım düzenlerine ilişkin performans hedeflerini yönetmek için basit ve uygun maliyetli çözüm sağlar. Elastik havuz, havuzdaki bir veritabanı tarafından havuzdaki her bir veritabanına her zaman sağlamak gerekli kaynakları kullanılabilir en düşük düzeyde sahipken kaynakları tüketilemiyor garanti eder. 

![SQL Veritabanı'na Giriş: Katmana ve düzeye göre eDTU’lar](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Bir havuz için belirli bir fiyat sayıda Edtu verilir. Elastik havuz içerisinde tek veritabanlarına yapılandırılan sınırlar içinde otomatik olarak ölçeklendirme elastikliği tanınır. Ağır yük altındaki bir veritabanı, talebi karşılamak üzere daha fazla Edtu kullanacaktır. Daha hafif yükler altında veritabanları daha az Edtu kullanacaktır. Hiçbir yük veritabanlarıyla hiçbir Edtu kullanacaktır. Tüm havuz için kaynakları sağlama tarafından yerine veritabanı başına yönetim görevleri, havuz için tahmin edilebilir bir bütçe sağlama basitleştirilmiştir.

Mevcut bir havuza, veritabanı kapalı kalma süresi ve havuzdaki veritabanları üzerinde herhangi bir etkisi olmadan ek eDTU’lar eklenebilir. Benzer şekilde, ek eDTU’lara artık ihtiyaç yoksa bunlar mevcut bir havuzdan ne zaman isterseniz kaldırılabilir. Ekleyebilir veya havuzdan veritabanları kaldırabilirsiniz veya edtu'ları diğer veritabanları için ayırmak üzere ağır yük altında bir veritabanı Edtu miktarını sınırlayabilirsiniz kullanabilirsiniz. Bir veritabanını havuzdan durumdaysa kaynakları, havuz dışına taşımak ve tahmin edilebilir miktarda gerekli kaynaklara sahip tek bir veritabanı olarak yapılandırır.

### <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>İş yükümün ihtiyacı olan DTU sayısını nasıl belirleyebilirim?
Mevcut bir şirket içi veya SQL Server sanal makine iş yükünü Azure SQL Veritabanı’na geçirmek istiyorsanız, gereken yaklaşık DTU sayısını belirlemek için [DTU Hesaplayıcı](http://dtucalculator.azurewebsites.net/)’yı kullanabilirsiniz. Mevcut bir Azure SQL veritabanı iş yükünü için kullandığınız [SQL veritabanı sorgu performansı İçgörüleri](sql-database-query-performance.md) veritabanı kaynak tüketiminizi (Dtu'lar) ilişkin iş yükünüz iyileştirmek için daha kapsamlı içgörüler anlamak için. Ayrıca [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV'sini kullanarak son bir saat kaynak tüketimi görüntüleyin. Alternatif olarak, katalog görünümünü [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) son 14 gündür, ancak daha düşük bir aslına uygunluk beş dakikalık ortalamalar kaynak tüketimini görüntüler.

### <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Esnek bir kaynak havuzundan fayda sağlayıp sağlayamayacağımı nasıl öğrenebilirim?
Havuzlar, belirli kullanım düzenlerine sahip çok sayıda veritabanı bulunan durumlar için uygundur. Belirli bir veritabanı için bu düzen bir düşük kullanımı ortalama nispeten nadir zamanlarda kullanımın ani olarak artması şeklindedir. SQL Veritabanı, mevcut bir SQL Veritabanı sunucusundaki veritabanlarının geçmiş kaynak kullanımını otomatik olarak değerlendirir ve Azure portalda uygun havuz yapılandırmasını önerir. Daha fazla bilgi için bkz. [ne zaman elastik bir havuz kullanılması gerekir?](sql-database-elastic-pool.md)

### <a name="what-happens-when-i-hit-my-maximum-dtus"></a>Ben maksimum Dtu sayıma ulaştığımda ne olur?
Performans düzeyleri kalibre ve veritabanı iş yükünüzü seçilen hizmet katmanı/performans düzeyiniz için izin verilen üst sınırı kadar çalıştırmak için gereken kaynakları sağlamak üzere kapsamındadır. İş yükünüz CPU/veri GÇ/günlük GÇ sınırlarından birine ulaşıyorsa kaynakları izin verilen en üst düzeyi almaya devam eder, ancak daha yüksek sorgu gecikme süreleri de büyük olasılıkla yaşar. Bu sınırlar herhangi bir hataya yol açmaz, yalnızca iş yükünü yavaşlatır. Yavaşlama sorguların zaman aşımına uğramasına sebep olacak kadar şiddetli hale gelirse hatayla karşılaşırsınız. İzin verilen en yüksek eş zamanlı kullanıcı oturumu/isteği (çalışan iş parçacıkları) ulaşırsanız açık hatalar görürsünüz. Bkz: [Azure SQL veritabanı kaynak limitleri]( sql-database-resource-limits.md#what-happens-when-database-resource-limits-are-reached) CPU, bellek, veri GÇ ve işlem günlüğü g/ç için ilgili olmayan kaynak sınırları hakkında bilgi için.

### <a name="correlating-benchmark-results-to-real-world-database-performance"></a>Gerçek dünya veritabanı performans için kıyaslama sonuçları ilişkilendirme
Tüm değerlendirmesi yalnızca temsili ve hatırlanması olduğunu anlamak önemlidir. Kıyaslama uygulama ile elde edilen işlem hızları, diğer uygulamalarla elde aynı olması değil. Kıyaslama farklı işlem türleri birçok farklı tablolar ve veri türlerini içeren bir şema karşı çalıştırmak koleksiyonunu içerir. Tüm OLTP iş yükleri için ortak olan temel işlemlerin aynısını Kıyaslama uygular, ancak veritabanı veya uygulamanın belirli herhangi bir sınıf temsil etmiyor. Kıyaslama amacı, göreli ölçeği artırılabilen veya azaltılabilen olduğunda performans düzeyleri arasında beklenebilir bir veritabanının performansı için makul bir kılavuz sağlamaktır. Veritabanları farklı boyutta ve karmaşıklık gerçekte, farklı iş yüklerini bileşimleri karşılaştığınız ve farklı şekilde yanıt verir. Örneğin, yoğun g/ç uygulama GÇ eşiklerini daha çabuk karşılaşabilirsiniz veya CPU yoğunluklu uygulama CPU sınırları daha çabuk karşılaşabilirsiniz. Herhangi bir veritabanı Kıyaslama artan yük altında aynı şekilde, ölçeği bir garanti yoktur.

Kıyaslama ve metodolojisi aşağıda daha ayrıntılı olarak açıklanmıştır.

### <a name="benchmark-summary"></a>Kıyaslama özeti
Çevrimiçi işlem işleme (OLTP) iş yükleri en sık karşılaşılan temel veritabanı işlemlerinin karışık performansını ASDB ölçer. Kıyaslama bulut bilgi işlem, veritabanı şemasını, veri doldurma göz önünde tasarlanmıştır ve işlem OLTP iş yüklerinde en sık kullanılan temel öğe eyaleti'nde temsili olacak şekilde tasarlanmıştır ancak.

### <a name="schema"></a>Şema
Şema yeterli çeşitlilik ve karmaşıklığı operations geniş bir destek sağlamak için tasarlanmıştır. Kıyaslama altı tablolarından oluşan bir veritabanına karşı çalışır. Tabloları üç kategoriye ayrılır: sabit-ölçeklendirme ve büyüyen boyutu. İki sabit boyutlu tablo vardır. üç ölçeklendirme tablo; ve giderek büyüyen bir tablo. Sabit boyutlu tablolar sabit sayıda satır vardır. Ölçeklendirme tabloların veritabanı performans için doğru orantılıdır ancak sırasında Kıyaslama değişmez bir kardinalite vardır. Büyüyen tablonun ilk yük ancak satırlar eklenen ve Silinen Kıyaslama çalıştırma sırasında kardinalite değişiklikleri ölçeklendirme bir tablo gibi boyutlandırılır.

Şema, tamsayı dahil olmak üzere, veri türlerinin bir karışımını içeren sayısal, karakter ve tarih/saat. Birincil ve ikincil anahtarları, ancak hiçbir yabancı anahtarlar şeması içerir - diğer bir deyişle, hiçbir başvurusal bütünlük kısıtlamaları tablolar bulunur arasında.

Bir veri nesil program başlangıç veritabanı verileri oluşturur. Tamsayı ve sayısal veriler, çeşitli stratejileri ile oluşturulur. Bazı durumlarda değerler rastgele bir aralığı içinde dağıtılır. Diğer durumlarda, bir dizi rastgele bir belirli dağıtım korunduğundan emin olmak için dizilmiş. Metin alanları ağırlıklı gerçekçi görünen verileri üretmek üzere sözcükler listesinden oluşturulur.

Bir "ölçek faktörü üzerinde." temel veritabanı boyutu Ölçeklendirme ve tabloları büyüyen kardinalite (SF kısaltılır) ölçek faktörü belirler. Aşağıdaki bölümde kullanıcılar ve ilerleme, veritabanı boyutu, kullanıcılar ve tüm diğer derlemekten ölçeklendirme en yüksek performansı sayısı açıklandığı gibi.

### <a name="transactions"></a>İşlemler
Aşağıdaki tabloda gösterildiği gibi iş yükü dokuz işlem türleri, oluşur. Her işlem, belirli bir veritabanı altyapısı ve sistem donanımı diğer işlemlerden yüksek karşıtlık ile sistem özelliklerine kümesini vurgulamak için tasarlanmıştır. Bu yaklaşım, farklı bileşenleri için genel performans etkisini değerlendirmek kolaylaştırır. Örneğin, "Okuma yoğun" işlem önemli bir diskten okunan işlem sayısını üretir.

| İşlem Türü | Açıklama |
| --- | --- |
| Lite okuyun |SEÇİN. bellek içi; salt okunur |
| Okuma Orta |SEÇİN. genellikle belleğe; salt okunur |
| Okuma ağır |SEÇİN. çoğunlukla değil bellek içi; salt okunur |
| Lite güncelleştirme |GÜNCELLEŞTİRME; bellek içi; okuma-yazma |
| Ağır güncelleştir |GÜNCELLEŞTİRME; çoğunlukla değil bellek içi; okuma-yazma |
| Lite Ekle |EKLEYİN. bellek içi; okuma-yazma |
| Ağır Ekle |EKLEYİN. çoğunlukla değil bellek içi; okuma-yazma |
| Sil |DELETE; bellek içi ve bellek içinde olmayan karışımı; okuma-yazma |
| CPU yoğun |SEÇİN. bellek içi; oldukça yoğun CPU yükü; salt okunur |

### <a name="workload-mix"></a>İş yükü karışımı
İşlem, aşağıdaki genel karışımı ile ağırlıklı bir dağıtım rastgele seçilir. Okuma/yazma yaklaşık 2:1 oranında genel karışımı vardır.

| İşlem Türü | Karışımı yüzdesi |
| --- | --- |
| Lite okuyun |35 |
| Okuma Orta |20 |
| Okuma ağır |5 |
| Lite güncelleştirme |20 |
| Ağır güncelleştir |3 |
| Lite Ekle |3 |
| Ağır Ekle |2 |
| Sil |2 |
| CPU yoğun |10 |

### <a name="users-and-pacing"></a>Kullanıcılar ve hızı
Kıyaslama iş yükünün, işlemler arasında eş zamanlı kullanıcı sayısıyla davranışını benzetmek için bağlantıları birtakım gönderen aracından yönlendirilir. Tüm bağlantılar ve işlemleri oluşturulan makine olsa da, kolaylık olması için bu bağlantıları "kullanıcılar" diyoruz Her kullanıcının diğer tüm kullanıcıları bağımsız olarak çalışır, ancak tüm kullanıcılar aynı döngüsünü aşağıda gösterilen adımları uygulayın:

1. Veritabanı bağlantı kurun.
2. Çıkmak için sinyal kadar yineleyin:
   * Bir işlemden rastgele (Ağırlıklı dağılım için) seçin.
   * Seçili işlem gerçekleştirin ve yanıt süresini ölçme.
   * İlerleme Gecikmesini tamamlanmasını bekleyin.
3. Veritabanı bağlantısını kapatın.
4. Çıkış.

Gönderilmemiş gecikmeyi (Adım 2c) rastgele seçilir, ancak 1.0 ikinci bir ortalaması olan bir dağıtım ile. Bu nedenle her kullanıcı ortalama olarak, saniyede en fazla bir işlem oluşturur.

### <a name="scaling-rules"></a>Ölçeklendirme kuralları
Kullanıcı sayısı, veritabanı boyutu (Ölçek faktörü birimleri) tarafından belirlenir. Her beş ölçek faktörü birim için bir kullanıcı yoktur. Gönderilmemiş gecikme nedeniyle, bir kullanıcı en fazla bir işlem, saniyede ortalama oluşturabilirsiniz.

Örneğin, bir ölçek faktörü 500 (SF = 500) veritabanı 100 kullanıcı sahip olur ve en çok 100 TPS saniyede elde edebilirsiniz. Daha yüksek bir TPS sürücüsüne oranı daha fazla kullanıcı ve daha büyük bir veritabanı gerektirir.

Aşağıdaki tabloda, her hizmet katmanı ve performans düzeyi için gerçekten Sürdürülen kullanıcı sayısını gösterir.

| Hizmet Katmanı (performans düzeyi) | Kullanıcılar | Veritabanı Boyutu |
| --- | --- | --- |
| Temel |5 |720 MB |
| Standart (S0) |10 |1 GB |
| Standart (S1) |20 |2.1 GB |
| Standart (S2 için) |50 |7.1 GB |
| Premium (P1) |100 |14 GB |
| Premium (P2) |200 |28 GB |
| Premium (P6) |800 |114 GB |

### <a name="measurement-duration"></a>Ölçüm süresi
Geçerli bir Kıyaslama en az bir saat kararlı bir duruma ölçüm bulunulmasını gerektirir.

### <a name="metrics"></a>Ölçümler
Ana ölçümler Kıyaslama aktarım hızı ve yanıt süresi ' dir.

* Temel performans ölçüsü Kıyaslama aktarım hızıdır. Aktarım hızı birimi,-tüm işlem türleri sayım zamanı başına işlemlerde bildirilir.
* Yanıt süresi, performans öngörülebilirliğini ölçüsüdür. Yanıt süresi kısıtlaması hizmetiyle aşağıda gösterildiği gibi daha katı bir yanıt zaman gereksinimini olması daha yüksek sınıflarına sınıfının göre değişir.

| Hizmet sınıfı | Aktarım hızı ölçümü | Yanıt süresi gereksinimi |
| --- | --- | --- |
| Premium |Saniye başına işlem |95. yüzdebirlik 0,5 saniye |
| Standart |Dakika başına işlem |1.0 saniye 90. yüzdebirlik |
| Temel |Saat başına işlem |2.0 saniye 80. yüzdebirlik |

## <a name="next-steps"></a>Sonraki adımlar

- Sanal çekirdek tabanlı satın alma modeli için bkz: [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md)
- DTU tabanlı satın alma modeli için bkz: [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md).
