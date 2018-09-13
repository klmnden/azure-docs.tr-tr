---
title: Azure SQL veritabanı'nı satın alma modeli | Microsoft Docs
description: Azure SQL veritabanı için model satın alma hakkında bilgi edinin.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 08/17/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: a4bf21f972da1aa92d6f127e8cbabb89a9c31489
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44719962"
---
# <a name="azure-sql-database-purchasing-models-and-resources"></a>Azure SQL veritabanı'nı modelleri ve kaynakları satın alma 

Azure SQL veritabanı, performans ve maliyet ihtiyaçlarınıza uygun veritabanı altyapısı tam olarak yönetilen PaaS kolayca satın almanızı sağlar. Azure SQL veritabanı dağıtım modeline bağlı olarak gereksinimlerinize uyan bir satın alma modeli seçebilirsiniz: 
 - [Mantıksal sunucu](sql-database-logical-servers.md) içinde [Azure SQL veritabanı](sql-database-technical-overview.md) işlem, depolama ve GÇ kaynakları iki satın alma modeli sunar: DTU tabanlı satın alma modeli ve [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md). 
 - [Yönetilen örnekler](sql-database-managed-instance.md) Azure SQL veritabanı yalnızca teklifte [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md).

Aşağıdaki tablo ve grafik karşılaştırın ve bu iki satın alma modeli.

|**Satın alma modeli**|**Açıklama**|**En iyi**|
|---|---|---|
|DTU tabanlı model|Bu model, işlem, depolama ve GÇ kaynakları ile birlikte gelen bir ölçüyü temel alır. Performans düzeyleri tek veritabanları için Veritabanı İşlem Birimleri (DTU’lar), elastik havuzlar için de elastik Veritabanı İşlem Birimleri (eDTU’lar) ile ifade edilir. Dtu'lar ve Edtu'lar hakkında daha fazla bilgi için bkz. [Dtu'lar ve Edtu'lar nelerdir](sql-database-service-tiers.md#what-are-database-transaction-units-dtus)?|Basit, önceden yapılandırılmış kaynak seçenekleri isteyen müşteriler için idealdir.| 
|vCore tabanlı model|Bu model, işlem ve depolama kaynaklarını bağımsız olarak seçmenizi sağlar. Ayrıca, maliyet tasarrufu elde etmek için SQL Server için Azure hibrit avantajı kullanmanıza olanak sağlar.|Esneklik, Denetim ve saydamlık değerini müşteriler için idealdir.|
||||  

![Fiyatlandırma modeli](./media/sql-database-service-tiers/pricing-model.png)

## <a name="vcore-based-purchasing-model"></a>Sanal çekirdek tabanlı satın alma modeli 

Sanal çekirdek, fiziksel donanım (örneğin, sayı çekirdekleri, bellek, depolama boyutu) özelliklerini ve donanım Nesilleri arasında seçim yapma olanağı ile sunulan mantıksal CPU'yu temsil eder. Sanal çekirdek tabanlı satın alma modeli, esneklik, denetimi, bireysel kaynak kullanımının saydamlığı sağlar ve basit bir yol çevirmek için şirket iş yükü gereksinimlerini buluta. Bu model, işlem, bellek ve depolama iş yükü gereksinimlerine göre seçmenize olanak sağlar. Sanal çekirdek tabanlı satın alma modeli, müşteriler arasında seçim yapabilirsiniz [genel amaçlı](sql-database-high-availability.md#standardgeneral-purpose-availability) ve [iş açısından kritik](sql-database-high-availability.md#premiumbusiness-critical-availability) hizmet katmanları için her ikisi de [tek veritabanları](sql-database-single-database-scale.md), [yönetilen örnekleri](sql-database-managed-instance.md), ve [elastik havuzlar](sql-database-elastic-pool.md). 

Sanal çekirdek tabanlı satın alma modeli, bağımsız olarak işlem ve depolama kaynaklarını seçin, aynı şirket içi performans ve fiyat iyileştirme sağlar. Sanal çekirdek tabanlı satın alma modeli, müşteriler için ödeme yapar:
- İşlem (hizmet katmanı + sanal çekirdek sayısı ve bellek + donanımın miktar) *
- Tür ve veri ve günlük depolama miktarı 
- IOs ** - geçerli sayısını [mantıksal sunucuları](sql-database-logical-servers.md) yalnızca
- Yedekleme depolama (RA-GRS) ** 

\* İlk genel önizlemede Gen 4 mantıksal CPU'lar Intel E5-2673 v3 dayalı (Haswell) 2,4 GHz işlemcileri.

\*\* Önizleme sırasında yedeklemeleri ve IOs yedi gün ücretsizdir.

> [!IMPORTANT]
> İşlem, IOs, veri ve günlük depolama, veritabanı veya elastik havuz başına ücretlendirilir. Yedekleme depolama, her veritabanı ücretlendirilir. Yönetilen örnek ücretleri ayrıntılarını başvurmak [Azure SQL veritabanı yönetilen örneği](sql-database-managed-instance.md).
> **Bölge kısıtlamaları:** sanal çekirdek tabanlı satın alma modeli henüz aşağıdaki bölgelerde kullanılabilir değil: Batı Avrupa, Fransa Orta, Birleşik Krallık Güney, UK Batı ve Avustralya Güneydoğu.

Veritabanı veya elastik Havuzu'nu sanal çekirdek 300'den fazla DTU dönüştürme kullanırsa maliyetinizi azaltabilir. API'nizi tercih ettiğiniz veya kapalı kalma süresi ile Azure portalını kullanarak dönüştürebilirsiniz. Ancak, dönüştürme gerekli değildir. DTU satın alma modeli, performans ve iş gereksinimlerini karşılıyorsa, onu kullanmaya devam etmek. Sanal çekirdek-model DTU modelden dönüştürmeye karar verirseniz, aşağıdaki kural karşısında kullanarak performans düzeyi seçmeniz gerekir: genel amaçlı katmanında; en az 1 sanal çekirdek standart katmandaki her 100 DTU gerektirir Her Premium katmanda 125 DTU, iş açısından kritik katmanında en az 1 sanal çekirdek gerektirir.

## <a name="dtu-based-purchasing-model"></a>DTU tabanlı satın alma modeli

Veritabanı işlem birimi (DTU) temsil eden bir ölçüyle CPU, bellek, okur ve yazar. DTU tabanlı satın alma modeli, bir dizi işlem kaynakları, önceden yapılandırılmış paketleri sunar ve dahil edilen depolama alanı için çeşitli uygulama performans düzeylerini sürücü. Önceden yapılandırılmış bir paket sabit ödemeler ve Basitlik, her ay tercih eden müşteriler bulun DTU tabanlı model ihtiyaçları için daha uygun. DTU tabanlı satın alma modeli, müşteriler arasında seçim yapabilirsiniz **temel**, **standart**, ve **Premium** hizmet katmanları için her ikisi de [tek veritabanları](sql-database-single-database-scale.md) ve [elastik havuzlar](sql-database-elastic-pool.md). Bu satın alma modeli kullanıma sunulmadı [yönetilen örnekleri](sql-database-managed-instance.md).

### <a name="what-are-database-transaction-units-dtus"></a>Veritabanı işlem birimleri (Dtu) nedir?
İçinde bir belirli bir performans düzeyinde tek bir Azure SQL veritabanı için bir [hizmet katmanı](sql-database-single-database-scale.md), Microsoft garanti kaynakları (bağımsız olarak herhangi bir veritabanı Azure bulutunda) Bu veritabanı, belirli bir düzeyde sağlayan bir tahmin edilebilir performans düzeyi. Kaynakların miktarını, veritabanı işlem birimleri veya Dtu'lar sayısı hesaplanır ve işlem, depolama ve GÇ kaynakları ile birlikte gelen bir ölçüdür. Bu kaynaklar arasındaki oran başlangıçta tarafından belirlenen bir [OLTP Kıyaslama iş yükünün](sql-database-benchmark-overview.md), gerçek OLTP iş yükleri tipik olacak şekilde tasarlanmıştır. İş yükünüz şu kaynaklara miktarı aşarsa, aktarım hızınızı daraltılmış - yavaş performans ve zaman aşımları sonuç. İş yükünüz tarafından kullanılan kaynakları başka bir SQL veritabanına Azure bulutunda kullanılabilir kaynakları etkilemez ve diğer iş yükleri tarafından kullanılan kaynakları SQL veritabanınıza kullanılabilir kaynakları etkilemez.

![sınırlayıcı kutu](./media/sql-database-what-is-a-dtu/bounding-box.png)

Dtu en kaynakları farklı performans düzeylerinde Azure SQL veritabanı hizmet katmanları arasındaki göreli miktarını anlamak için kullanışlıdır. Örneğin, bir veritabanının performans düzeyini artırarak dtu'ları Katlama konusu veritabanının kullanabileceği kaynakları kümesi Katlama için karşılık gelmektedir. Örneğin, 1750 DTU’ya sahip Premium P11 veritabanı 5 DTU’ya sahip Temel veritabanına göre 350 kat daha fazla DTU işlem gücü sağlıyor.  

İş yükünüz kaynak (DTU) kullanımını daha derin bir anlayış kazanmak için kullanın [Azure SQL veritabanı sorgu performansı İçgörüleri](sql-database-query-performance.md) için:

- En sık kullanılan sorgular, potansiyel olarak daha iyi performans için ayarlanmış CPU/süresi/yürütme sayısına göre belirleyin. Örneğin, bir g/ç kullanımı yoğun sorgu kullanımından yararlanabilir [bellek içi iyileştirme teknikleri](sql-database-in-memory.md) bir belirli hizmet katmanını ve performans düzeyinde kullanılabilir belleği daha iyi kullanılmasını sağlamak için.
- Bir sorgunun ayrıntılarına detaya, kendi metin ve kaynak kullanımı geçmişini görüntüleyin.
- Erişim performans tarafından gerçekleştirilen eylemleri göster önerilerinde [SQL veritabanı Danışmanı](sql-database-advisor.md).

### <a name="what-are-elastic-database-transaction-units-edtus"></a>Elastik veritabanı işlem birimleri (Edtu'lar) nedir?
Bunun yerine her zaman kullanılabilir bir SQL veritabanı için her zaman gerekli değildir (Dtu) kaynakları ayrılmış bir dizi sağlamak daha veritabanlarına yerleştirebilirsiniz bir [elastik havuz](sql-database-elastic-pool.md) bir SQL veritabanı sunucusundaki kaynakları arasında bir havuzu paylaşır Bu veritabanları. Bir elastik havuzdaki paylaşılan kaynakları, elastik veritabanı işlem birimleri veya Edtu'lar tarafından ölçülür. Elastik havuzlar, birden çok veritabanına sahip son derece farklı ve öngörülemeyen kullanım düzenlerine ilişkin performans hedeflerini yönetmek için basit ve uygun maliyetli çözüm sağlar. Elastik havuz, havuzdaki bir veritabanı tarafından havuzdaki her bir veritabanına her zaman sağlamak gerekli kaynakları kullanılabilir en düşük düzeyde sahipken kaynakları tüketilemiyor garanti eder. 

![SQL Veritabanı'na Giriş: Katmana ve düzeye göre eDTU’lar](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Bir havuz için belirli bir fiyat sayıda Edtu verilir. Elastik havuz içerisinde tek veritabanlarına yapılandırılan sınırlar içinde otomatik olarak ölçeklendirme elastikliği tanınır. Ağır yük altındaki bir veritabanı, talebi karşılamak üzere daha fazla Edtu kullanacaktır. Daha hafif yükler altında veritabanları daha az Edtu kullanacaktır. Hiçbir yük veritabanlarıyla hiçbir Edtu kullanacaktır. Tüm havuz için kaynakları sağlama tarafından yerine veritabanı başına yönetim görevleri, havuz için tahmin edilebilir bir bütçe sağlama basitleştirilmiştir.

Mevcut bir havuza, veritabanı kapalı kalma süresi ve havuzdaki veritabanları üzerinde herhangi bir etkisi olmadan ek eDTU’lar eklenebilir. Benzer şekilde, ek eDTU’lara artık ihtiyaç yoksa bunlar mevcut bir havuzdan ne zaman isterseniz kaldırılabilir. Ekleyebilir veya havuzdan veritabanları kaldırabilirsiniz veya edtu'ları diğer veritabanları için ayırmak üzere ağır yük altında bir veritabanı Edtu miktarını sınırlayabilirsiniz kullanabilirsiniz. Bir veritabanını havuzdan durumdaysa kaynakları, havuz dışına taşımak ve tahmin edilebilir miktarda gerekli kaynaklara sahip tek bir veritabanı olarak yapılandırır.

### <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>İş yükümün ihtiyacı olan DTU sayısını nasıl belirleyebilirim?
Mevcut bir şirket içi veya SQL Server sanal makine iş yükünü Azure SQL Veritabanı’na geçirmek istiyorsanız, gereken yaklaşık DTU sayısını belirlemek için [DTU Hesaplayıcı](http://dtucalculator.azurewebsites.net/)’yı kullanabilirsiniz. Mevcut bir Azure SQL veritabanı iş yükünü için kullandığınız [SQL veritabanı sorgu performansı İçgörüleri](sql-database-query-performance.md) veritabanı kaynak tüketiminizi (Dtu'lar) ilişkin iş yükünüz iyileştirmek için daha kapsamlı içgörüler anlamak için. Ayrıca [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV'sini kullanarak son bir saat kaynak tüketimi görüntüleyin. Alternatif olarak, katalog görünümünü [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) son 14 gündür, ancak daha düşük bir aslına uygunluk beş dakikalık ortalamalar kaynak tüketimini görüntüler.

### <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Esnek bir kaynak havuzundan fayda sağlayıp sağlayamayacağımı nasıl öğrenebilirim?
Havuzlar, belirli kullanım düzenlerine sahip çok sayıda veritabanı bulunan durumlar için uygundur. Belirli bir veritabanı için bu düzen bir düşük kullanımı ortalama nispeten nadir zamanlarda kullanımın ani olarak artması şeklindedir. SQL Veritabanı, mevcut bir SQL Veritabanı sunucusundaki veritabanlarının geçmiş kaynak kullanımını otomatik olarak değerlendirir ve Azure portalda uygun havuz yapılandırmasını önerir. Daha fazla bilgi için bkz. [ne zaman elastik bir havuz kullanılması gerekir?](sql-database-elastic-pool.md)


## <a name="next-steps"></a>Sonraki adımlar

- Sanal çekirdek tabanlı satın alma modeli için bkz: [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md)
- DTU tabanlı satın alma modeli için bkz: [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md).
