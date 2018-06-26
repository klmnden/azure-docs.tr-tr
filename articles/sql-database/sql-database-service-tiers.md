---
title: Azure SQL veritabanı satın alma modeli | Microsoft Docs
description: Azure SQL veritabanı için model satın alma hakkında bilgi edinin.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: dda37600dffaf1e8e1c85c9757696f2d8219febb
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751679"
---
# <a name="azure-sql-database-purchasing-models-and-resources"></a>Azure SQL veritabanı modelleri ve kaynakları satın alma 

Mantıksal sunucu [Azure SQL veritabanı](sql-database-technical-overview.md) işlem, depolama ve g/ç kaynaklar için iki satın alma modeli sunar: DTU tabanlı satın alma modeli ve vCore tabanlı satın alma modeli (Önizleme). 

> [!NOTE]
> [Örnekleri yönetilen](sql-database-managed-instance.md) yalnızca Azure SQL veritabanında vCore tabanlı satın alma modeli sunar.

Aşağıdaki tablo ve grafik karşılaştırır ve bu iki satın alma modeli karşılaştırın.

> [!IMPORTANT]
> VCore tabanlı satın alma modeli için (Önizleme), bkz: [vCore tabanlı satın alma modeli](sql-database-service-tiers-vcore.md)

|**Satın alma modeli**|**Açıklama**|**En iyi**|
|---|---|---|
|DTU tabanlı model|Bu model, işlem, depolama ve g/ç kaynakları ile birlikte gelen bir ölçüye temel alır. Performans düzeyleri tek veritabanları için Veritabanı İşlem Birimleri (DTU’lar), elastik havuzlar için de elastik Veritabanı İşlem Birimleri (eDTU’lar) ile ifade edilir. Dtu ve Edtu hakkında daha fazla bilgi için bkz: [Dtu ve Edtu nelerdir](sql-database-service-tiers.md#what-are-database-transaction-units-dtus)?|Basit, önceden yapılandırılmış kaynak seçenekleri isteyen müşteriler için en iyisidir.| 
|vCore tabanlı model|Bu model, işlem ve depolama kaynaklarını bağımsız olarak ölçeklendirebilirsiniz sağlar. Ayrıca, maliyet tasarrufu sağlamak için SQL Server için Azure karma avantajı kullanmanıza olanak sağlar.|Esneklik, Denetim ve saydam değer müşteriler için en iyisidir.|
||||  

![Fiyatlandırma modeli](./media/sql-database-service-tiers/pricing-model.png)

## <a name="vcore-based-purchasing-model--preview"></a>satın alma modeli vCore tabanlı (Önizleme)

Sanal bir çekirdek donanım nesli arasında seçmek için bir seçenek ile birlikte sunulan mantıksal CPU temsil eder. Ve çevirmek için basit bir yol içi buluta iş yükü gereksinimlerini esneklik, Denetim, tek tek kaynak tüketimini saydamlığını vCore tabanlı satın alma modeli (Önizleme) sağlar. Bu model, bilgi işlem, bellek ve kendi iş yükü ihtiyaçlarına depolama olanak tanır. VCore tabanlı satın alma modeli (Önizleme), müşteriler genel amaçlı ve iş kritik hizmet katmanları (Önizleme) her ikisi için seçebilir [tek veritabanlarını](sql-database-single-database-scale.md) ve [esnek havuzlar](sql-database-elastic-pool.md). 

He vCore tabanlı satın alma modeli (Önizleme) bağımsız olarak işlem ve depolama kaynaklarını ölçeklendirme, şirket içi performans eşleşen ve fiyat en iyi duruma olanak tanır. Veritabanı veya esnek havuz vCore 300'den fazla DTU dönüştürme kullanırsa maliyetinizi azaltabilir. API'nizi tercih veya kapalı kalma süresi ile Azure portal kullanarak dönüştürebilirsiniz. Ancak, dönüştürme gerekli değildir. DTU satın alma modeli performans ve iş gereksinimleri karşılıyorsa kullanmaya devam etmelidir. DTU modelden vCore modeline dönüştürmeye karar verirseniz, aşağıdaki kural altın kullanarak performans düzeyini seçmeniz gerekir: genel amaçlı katmanındaki; en az 1 vCore her 100 DTU standart katmanındaki gerektirir Premium katmanındaki 125 her DTU iş kritik katmanındaki en az 1 vCore gerektirir.

VCore tabanlı satın alma modeli (Önizleme), müşteriler için ödeme:
- İşlem (hizmet katmanı + vCores + donanım nesil sayısı) *
- Türü ve veri ve günlük depolama alanı miktarı 
- IOs ** sayısı
- Yedekleme depolama (RA-GRS) ** 

\* İlk genel önizlemede Gen 4 mantıksal CPU'ları üzerinde Intel E5-2673 v3 temel alır (Haswell) 2.4 GHz işlemci.

\*\* Önizleme sırasında yedeklemeler ve IOs 7 gün ücretsizdir.

> [!IMPORTANT]
> İşlem, IOs, veri ve günlük depolama veritabanı veya esnek havuz ücretlendirilirsiniz. Yedekleme depolama her veritabanı başına ücret kesilir. Yönetilen örneği ücretleri ayrıntılarını başvurmak [yönetilen Azure SQL veritabanı örneği](sql-database-managed-instance.md).
> **Bölge sınırlamaları:** vCore tabanlı satın alma modeli (Önizleme) henüz aşağıdaki bölgelerde kullanılabilir değil: Batı Avrupa, Fransa Merkezi, Birleşik Krallık Güney, Birleşik Krallık Batı ve Avustralya Güneydoğu.

## <a name="dtu-based-purchasing-model"></a>DTU tabanlı satın alma modeli

Veritabanı işleme birimi (DTU) temsil eden karışık bir ölçüyü CPU, bellek, okur ve yazar. DTU tabanlı satın alma modeli bir dizi önceden yapılandırılmış paketleri işlem kaynakları sunar ve sürücü farklı düzeylerde uygulama performansı için depolama dahil. Önceden yapılandırılmış bir paket ve sabit ödemeler basitliği her ay tercih müşteriler Bul DTU tabanlı modeli gereksinimlerine için daha uygun. DTU tabanlı satın alma modelinde, müşterilerin arasından seçim yapabilirsiniz **temel**, **standart**, ve **Premium** hizmet katmanları için her ikisini de [tek veritabanlarını](sql-database-single-database-scale.md) ve [esnek havuzlar](sql-database-elastic-pool.md). 

### <a name="what-are-database-transaction-units-dtus"></a>Veritabanı işlem birimleri (Dtu'lar) nelerdir?
Belirli bir performans düzeyinde içinde tek bir Azure SQL veritabanı için bir [hizmet katmanı](sql-database-single-database-scale.md), Microsoft garanti belirli bir düzeyde veritabanına (herhangi bir Azure bulut veritabanında bağımsız) için kaynaklar sağlayan bir tahmin edilebilir performans düzeyi. Kaynakları miktarını veritabanı işlem birimleri veya Dtu'lar sayısı olarak hesaplanır ve ile birlikte gelen bir işlem, depolama ve g/ç kaynakları ölçüsüdür. Bu kaynaklar arasında oranı başlangıçta tarafından belirlenen bir [OLTP Kıyaslama iş yükü](sql-database-benchmark-overview.md), gerçek OLTP iş yükü tipik olması için tasarlanmıştır. İş yükünüzün bu kaynaklardan herhangi birini miktarını aşarsa, üretilen iş daraltılmış - yavaş performans ve zaman aşımları sonuç. İş yükünüzün kullandığı kaynakları diğer SQL veritabanları Azure bulutta kullanılabilir kaynakları etkilemeyen ve diğer iş yükleri tarafından kullanılan kaynakları SQL veritabanınız kullanılabilir kaynakları etkilemez.

![sınırlama kutusu](./media/sql-database-what-is-a-dtu/bounding-box.png)

Dtu'lar en kaynakları Azure SQL veritabanları farklı performans düzeyleri ve hizmet katmanları arasındaki göreli miktarını anlamak için kullanışlıdır. Örneğin, bir veritabanı performans düzeyini artırarak Dtu'lar Katlama o veritabanına kullanılabilir kaynakları kümesi Katlama için karşılık gelir. Örneğin, 1750 DTU’ya sahip Premium P11 veritabanı 5 DTU’ya sahip Temel veritabanına göre 350 kat daha fazla DTU işlem gücü sağlıyor.  

İş yükünüzün (DTU) kaynak tüketimini daha derin bir anlayış kazanmak için [Azure SQL veritabanı sorgu performansı öngörüleri](sql-database-query-performance.md) için:

- Potansiyel olarak performansı için ayarlanan süre/CPU/yürütme sayısı tarafından en sık kullanılan sorguların tanımlayın. Örneğin, bir g/ç yoğun sorgusu kullanımından yararlanabilir [bellek içi iyileştirme tekniklerini](sql-database-in-memory.md) düzeyde belirli hizmet katmanını ve performans kullanılabilir belleği daha iyi kullanılmasını sağlamak için.
- Bir sorgu ayrıntıları detaya, metin ve kaynak kullanımı geçmişini görüntüleyin.
- Erişim performans tarafından gerçekleştirilen eylemler Göster önerileri ayarlama [SQL veritabanı Danışmanı'nı](sql-database-advisor.md).

Değiştirebileceğiniz [DTU hizmet katmanları](sql-database-service-tiers-dtu.md) (genellikle altında dört saniye ortalaması) uygulamanız için en az kapalı kalma süresi ile herhangi bir zamanda. Veritabanı oluşturabilmek ve veritabanı performansını isteğe göre yükseltip düşürebilmek, özellikle kullanım biçimlerinin nispeten tahmin edilebilir olduğu durumlarda birçok işletme ve uygulama için yeterlidir. Ancak tahmin edilemeyen kullanım biçimlerine sahipseniz bu durum maliyetlerin ve iş modelinizin yönetimini zorlaştırabilir. Bu senaryoda, belirli bir havuzdaki birden fazla veritabanı arasında paylaşılan Edtu sayısı ile bir esnek havuz kullanın.

![SQL Veritabanı'na Giriş: Katmana ve düzeye göre tek veritabanı DTU’ları](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

### <a name="what-are-elastic-database-transaction-units-edtus"></a>Esnek veritabanı işlem birimleri (Edtu'lar) nelerdir?
Bunun yerine her zaman kullanılabilir olan bir SQL veritabanı için her zaman gerekli değildir (Dtu'lar) kaynakları ayrılmış bir dizi sağlayan daha veritabanlarına yerleştireceğiniz bir [esnek havuz](sql-database-elastic-pool.md) bir SQL veritabanı sunucusundaki kaynakları arasında bir havuzu paylaşır Bu veritabanları. Paylaşılan kaynaklar esnek havuzdaki esnek veritabanı işlem birimleri veya Edtu'lar tarafından ölçülür. Esnek havuzlar yaygın olarak değişen sahip birden çok veritabanı ve tahmin edilemeyen kullanım biçimlerine için performans hedeflerini yönetmek için uygun maliyetli bir basit çözüm sağlar. Bir esnek havuz havuzundaki bir veritabanı tarafından her veritabanı havuzundaki her zaman sağlama gerekli kaynaklar kullanılabilir en düşük düzeyde sahipken kaynaklar kullanılamaz güvence altına alır. 

![SQL Veritabanı'na Giriş: Katmana ve düzeye göre eDTU’lar](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Bir havuz Edtu kümesi sayısı için bir set fiyatı verilir. Elastik havuz içerisinde tek veritabanlarına yapılandırılan sınırlar içinde otomatik olarak ölçeklendirme elastikliği tanınır. Bir veritabanını daha ağır yük altında talebi karşılamak üzere daha fazla Edtu'lar tüketir. Açık yükleri altındaki veritabanları daha az Edtu tüketir. Hiçbir yük veritabanlarıyla hiçbir Edtu'lar tüketir. Tüm havuz için kaynakları sağlama tarafından yerine veritabanı başına yönetim görevleri, tahmin edilebilir bir bütçe havuzu için sağlama basitleştirilmiştir.

Mevcut bir havuza, veritabanı kapalı kalma süresi ve havuzdaki veritabanları üzerinde herhangi bir etkisi olmadan ek eDTU’lar eklenebilir. Benzer şekilde, ek eDTU’lara artık ihtiyaç yoksa bunlar mevcut bir havuzdan ne zaman isterseniz kaldırılabilir. Ekleyebilir veya havuza veritabanlarını çıkarma veya sınırı Edtu'lar bir veritabanı miktarını ağır yük altında diğer veritabanları için Edtu ayırmak için kullanabilirsiniz. Bir veritabanı beklendiği altında-kullanılarak durumdaysa kaynakları, havuzu dışına taşıyın ve gereken kaynakları tahmin edilebilir bir miktar tek bir veritabanı olarak yapılandırın.

### <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>İş yükümün ihtiyacı olan DTU sayısını nasıl belirleyebilirim?
Mevcut bir şirket içi veya SQL Server sanal makine iş yükünü Azure SQL Veritabanı’na geçirmek istiyorsanız, gereken yaklaşık DTU sayısını belirlemek için [DTU Hesaplayıcı](http://dtucalculator.azurewebsites.net/)’yı kullanabilirsiniz. Bir mevcut Azure SQL veritabanı iş yükü için kullandığınız [SQL veritabanı sorgu performansı öngörüleri](sql-database-query-performance.md) veritabanı kaynak tüketimini, iş yükü en iyi duruma getirme için daha iyi kavramak için (Dtu'lar) anlamak için. Aynı zamanda [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV son bir saat için kaynak tüketimini görüntüleyin. Alternatif olarak, katalog görünümünü [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) son 14 gündür, ancak daha düşük bir doğruluğu beş dakikalık ortalamalar, kaynak tüketimini görüntüler.

### <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Esnek bir kaynak havuzundan fayda sağlayıp sağlayamayacağımı nasıl öğrenebilirim?
Havuzlar, belirli kullanım düzenlerine sahip çok sayıda veritabanı bulunan durumlar için uygundur. Verilen bir veritabanı için bu deseni oldukça sık kullanımı ani bir düşük kullanımı ortalama tarafından belirlenir. SQL Veritabanı, mevcut bir SQL Veritabanı sunucusundaki veritabanlarının geçmiş kaynak kullanımını otomatik olarak değerlendirir ve Azure portalda uygun havuz yapılandırmasını önerir. Daha fazla bilgi için bkz. [ne zaman elastik bir havuz kullanılması gerekir?](sql-database-elastic-pool.md)

### <a name="what-happens-when-i-hit-my-maximum-dtus"></a>My maksimum Dtu isabet ne olur?
Performans düzeyleri kalibre ve veritabanının yükünüzü için seçilen hizmet katmanı/performans düzeyi izin verilen maksimum kadar çalıştırmak için gerekli kaynakları sağlamak üzere kapsamındadır. İş yükünüzün CPU/Data GÇ/günlük GÇ sınırlardan biri basarsa, kaynakları izin verilen en fazla düzeyi almaya devam eder, ancak artırılmış sorgu gecikme süreleri de büyük olasılıkla karşılaşırsınız. Bu sınırlar herhangi bir hataya yol açmaz, yalnızca iş yükünü yavaşlatır. Yavaşlama sorguların zaman aşımına uğramasına sebep olacak kadar şiddetli hale gelirse hatayla karşılaşırsınız. İzin verilen en fazla eş zamanlı kullanıcı oturumları/istekleri (çalışan iş parçacıkları) ulaştıysanız, açık hataları görürsünüz. Bkz: [Azure SQL veritabanı kaynak sınırları]( sql-database-resource-limits.md#what-happens-when-database-resource-limits-are-reached) CPU, bellek, veri g/ç veya işlem günlük GÇ ilgili olmayan kaynak sınırları hakkında bilgi için.

### <a name="correlating-benchmark-results-to-real-world-database-performance"></a>Gerçek dünya veritabanı performans kıyaslama sonuçları ilişkilendirme
Tüm değerlendirmeleri yalnızca temsili ve hatırlanması olduğunu anlamak önemlidir. Kıyaslama uygulama ile elde edilen işlem hızları aynı olan diğer uygulamalarla elde olmaz. Kıyaslama farklı bir işlem türleri bir dizi tablolar ve veri türlerini içeren bir şemaya karşı çalıştırmak koleksiyonunu içerir. Tüm OLTP iş yükleri için ortak olan aynı temel işlemleri Kıyaslama uygular, ancak herhangi belirli bir sınıf uygulama veya veritabanı göstermiyor. Kıyaslama amacı yukarı veya aşağı Ölçeklendirmesi, performans düzeyleri arasında beklenen bir veritabanının göreli performansı için makul bir kılavuz sağlamaktır. Gerçekte, veritabanları farklı boyutlarda ve karmaşıklık olan, iş yüklerini farklı bileşimleri karşılaştığınız ve farklı şekilde yanıt verir. Örneğin, bir g/ç yoğunluklu uygulama GÇ eşikleri er isabet veya bir CPU-yoğun uygulama CPU sınırları er isabet. Herhangi bir veritabanı artan yük altında Kıyaslama aynı şekilde ölçeklendirir garantisi yoktur.

Kıyaslama ve metodolojisi aşağıdaki daha ayrıntılı olarak açıklanmıştır.

### <a name="benchmark-summary"></a>Kıyaslama özeti
ASDB çevrimiçi işlem işleme (OLTP) iş yükleri en sık karşılaşılan temel veritabanı işlemleri bir karışımını performansını ölçer. Kıyaslama unutmayın, veritabanı şemasını, veri doldurma bulut tasarlanmıştır ve işlemleri OLTP iş yüklerini en sık kullanılan temel öğelerinin kapsamlı temsilcisi olacak şekilde tasarlanmış olmasını rağmen.

### <a name="schema"></a>Şema
Şema yeterli çeşitli ve çok çeşitli işlemleri desteklemek için karmaşıklık sağlamak üzere tasarlanmıştır. Kıyaslama altı tabloları oluşan bir veritabanına karşı çalışır. Tabloları üç kategoriye ayrılır: sabit ölçekleme ve büyüyen boyutlu,. İki sabit boyutlu tablo vardır; üç ölçeklendirme tablolar; ve artan bir tablo. Sabit boyutlu tabloları sabit sayıda satıra sahip. Ölçeklendirme tablolar için veritabanı performansını orantılıdır ancak Kıyaslama sırasında değişmez bir önem düzeyi vardır. Büyüyen tablonun ilk yük ancak satırları eklenen ve Silinen Kıyaslama çalıştırma esnasında kardinalite değişiklikleri ölçeklendirme bir tablo gibi boyutlandırılır.

Şema, tamsayı dahil olmak üzere veri türlerinin bir karışımını içeren sayısal karakter ve tarih/saat. Birincil ve ikincil anahtarlar, ancak hiçbir yabancı anahtarlar şeması içerir - diğer bir deyişle, vardır hiçbir başvuru bütünlüğü kısıtlamalarını tablolar arasında.

Bir veri nesil program ilk veritabanı için veriler üretir. Tamsayı ve sayısal veriler çeşitli stratejiler kullanılarak oluşturulur. Bazı durumlarda değerler rastgele bir aralığı içinde dağıtılır. Diğer durumlarda, bir değerleri kümesi rastgele belirli bir dağıtım korunduğundan emin olmak için dizilmiş. Metin alanları gerçekçi görünümlü veri üretmek için sözcükler ağırlıklı listesinden üretilir.

Bir "ölçek faktörü üzerinde." temel veritabanı boyutu (BT kısaltılır) ölçek çarpanı ölçekleme ve tabloları büyütme önemi belirler. Aşağıdaki bölümde kullanıcılar ve ilerleme, veritabanı boyutu, kullanıcılar ve tüm diğer orantılı olarak ölçekleme en yüksek performans sayısı açıklandığı gibi.

### <a name="transactions"></a>İşlemler
Aşağıdaki tabloda gösterildiği gibi iş yükü dokuz işlem türlerini oluşur. Her işlem, sistem özelliklerine veritabanı motoru ve sistem donanımı diğer işlemlerdeki yüksek karşıtlık ile belirli bir dizi vurgulamak için tasarlanmıştır. Bu yaklaşım, farklı bileşenler için genel performans etkisi değerlendirmek kolaylaştırır. Örneğin, "Okuma ağır" işlem çok sayıda diskten okuma işlemleri üretir.

| İşlem Türü | Açıklama |
| --- | --- |
| Lite okuma |SEÇİN; bellek içi; salt okunur |
| Okuma Orta |SEÇİN; çoğunlukla bellek içi; salt okunur |
| Okuma ağır |SEÇİN; çoğunlukla olmayan bellek içi; salt okunur |
| Lite güncelleştirme |GÜNCELLEŞTİRME; bellek içi; okuma-yazma |
| Ağır güncelleştir |GÜNCELLEŞTİRME; çoğunlukla olmayan bellek içi; okuma-yazma |
| Lite Ekle |INSERT; bellek içi; okuma-yazma |
| Ağır Ekle |INSERT; çoğunlukla olmayan bellek içi; okuma-yazma |
| Sil |DELETE; bellek içi ve değil bellek karışımı; okuma-yazma |
| CPU ağır |SEÇİN; bellek içi; görece yoğun CPU yükünü; salt okunur |

### <a name="workload-mix"></a>İş yükü karışımı
İşlemleri, aşağıdaki genel karışımı ile ağırlıklı dağıtım rastgele seçilir. Genel karışımı yaklaşık 2:1 okuma/yazma oranını sahiptir.

| İşlem Türü | Karışımı yüzdesi |
| --- | --- |
| Lite okuma |35 |
| Okuma Orta |20 |
| Okuma ağır |5 |
| Lite güncelleştirme |20 |
| Ağır güncelleştir |3 |
| Lite Ekle |3 |
| Ağır Ekle |2 |
| Sil |2 |
| CPU ağır |10 |

### <a name="users-and-pacing"></a>Kullanıcılar ve Adımlama
Kıyaslama iş yükü, bir dizi eş zamanlı kullanıcı sayısı davranışını benzetmek için bağlantıları işlemleri gönderir bir aracından yönetilir. Tüm bağlantılar ve işlemleri oluşturulan makine olsa da, kolaylık sağlamak için Biz bu bağlantılar "kullanıcılar" olarak bakın Diğer tüm kullanıcıları bağımsız olarak her kullanıcı çalışır rağmen tüm kullanıcılar adımları aşağıda gösterilen aynı döngüsünü gerçekleştirin:

1. Bir veritabanı bağlantısı kurun.
2. Çıkmak için sinyal kadar yineleyin:
   * Bir işlem rastgele (dağıtımdan bir ağırlıklı) seçin.
   * Seçili işlem gerçekleştirmek ve yanıt süresini ölçün.
   * İlerleme Gecikmesini bekleyin.
3. Veritabanı bağlantısı kapatın.
4. Çıkış.

(2c adımında) ilerleme gecikmesini rastgele seçilir, ancak bir dağıtım 1.0 ikinci ortalama sahip. Bu nedenle her kullanıcı ortalama, saniye başına en fazla bir işlem oluşturabilir.

### <a name="scaling-rules"></a>Ölçeklendirme kuralları
Kullanıcı sayısı, veritabanı boyutu (Ölçek çarpanı birimleri) tarafından belirlenir. Her beş ölçek çarpanı birimler için bir kullanıcı yoktur. İlerleme Gecikmesini nedeniyle, bir kullanıcı en fazla bir işlem saniye başına ortalama oluşturabilir.

Örneğin, bir ölçek faktörü 500 (BT = 500) veritabanı 100 kullanıcıların gerekir ve en çok saniyede 100 TP'leri elde edebilirsiniz. Daha yüksek bir TP'leri sürücü oranı daha fazla kullanıcı ve daha büyük bir veritabanı gerektirir.

Aşağıdaki tabloda her hizmet katmanını ve performans düzeyini gerçekten Sürdürülen kullanıcı sayısını gösterir.

| Hizmet Katmanı (performans düzeyi) | Kullanıcılar | Veritabanı Boyutu |
| --- | --- | --- |
| Temel |5 |720 MB |
| Standart (S0) |10 |1 GB |
| Standart (S1) |20 |2.1 GB |
| Standart (S2) |50 |7.1 GB |
| Premium (P1) |100 |14 GB |
| Premium (P2) |200 |28 GB |
| Premium (P6) |800 |114 GB |

### <a name="measurement-duration"></a>Ölçüm süresi
Geçerli bir Kıyaslama en az bir saat kararlı durum ölçüm süresi gerektirir.

### <a name="metrics"></a>Ölçümler
Anahtar Kıyaslama üretilen iş ve yanıt süresi ölçümleridir.

* Üretilen iş Kıyaslama temel performans ölçüsüdür. İşleme birim--tüm işlem türleri sayım zaman, başına işlemlerde bildirilir.
* Yanıt süresi, performans öngörülebilirlik ölçüsüdür. Yanıt süresi kısıtlaması hizmetiyle aşağıda gösterildiği gibi daha sıkı bir yanıt süresi gereksinimi olan hizmet yüksek sınıfları sınıfının göre değişir.

| Hizmet sınıfı | Üretilen iş ölçü | Yanıt süresi gereksinimi |
| --- | --- | --- |
| Premium |Saniye başına işlem |0,5 saniye adresindeki 95. yüzdebirlik |
| Standart |Dakika başına işlem |1.0 saniye adresindeki 90 yüzdebirlik |
| Temel |Saat başına işlem |2.0 saniye adresindeki 80. yüzdebirlik |

## <a name="next-steps"></a>Sonraki adımlar

- VCore tabanlı satın alma modeli için (Önizleme), bkz: [vCore tabanlı satın alma modeli](sql-database-service-tiers-vcore.md)
- DTU tabanlı satın alma modeli için bkz: [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md).
