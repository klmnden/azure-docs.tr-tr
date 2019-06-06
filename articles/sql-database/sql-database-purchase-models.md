---
title: Azure SQL veritabanı'nı satın alma modeli | Microsoft Docs
description: Azure SQL veritabanı için kullanılabilir olan satın alma modelleri hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
manager: craigg
ms.date: 04/26/2019
ms.openlocfilehash: 98c19732c372fbcda3ca8e746d002f94c2687b22
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66431378"
---
# <a name="choose-between-the-vcore-and-the-dtu-purchasing-models"></a>Sanal çekirdek ve satın alma modeli DTU arasında seçim yapma

Azure SQL veritabanı, tam olarak yönetilen bir platform, performans ve maliyet ihtiyaçlarınıza uygun bir hizmet (PaaS) veritabanı altyapısı kolayca satın almanıza olanak tanır. Azure SQL veritabanı için seçtiğiniz dağıtım modeline bağlı olarak sizin için uygun satın alma modeli seçebilirsiniz:

- [Sanal çekirdek (vCore)-satın alma modeli tabanlı](sql-database-service-tiers-vcore.md) (önerilir). Bu satın alma modeli, sağlanan işlem katmanı ve bir sunucusuz (Önizleme) bilgi işlem katmanı arasında seçim yapma sağlar. Sağlanan işlem katmanı ile her zaman sağlanan işlem kaynakları miktarda iş yükünüz için seçin. Sunucusuz bilgi işlem katmanı ile yapılandırılabilir işlem aralığında işlem kaynaklarını otomatik ölçeklendirme belirtin. Bu işlem katmanı ile da otomatik olarak duraklatabilir ve iş yükü etkinliklere göre veritabanı sürdürülemedi. Zaman birimi sanal çekirdek birim fiyatı sağlanan işlem katmanında sunucusuz işlem katmanında olandan daha küçük.
- [Veritabanı işlem birimi (DTU)-satın alma modeli tabanlı](sql-database-service-tiers-dtu.md). Bu satın alma modeli, yaygın iş yükleri için dengeli ile birlikte gelen işlem ve depolama paketlerini sağlar.

Farklı satın alma modelleri, farklı Azure SQL veritabanı dağıtım modelleri için kullanılabilir:

- [Tek veritabanı](sql-database-single-databases-manage.md) ve [elastik havuz](sql-database-elastic-pool.md) dağıtım seçenekleri [Azure SQL veritabanı](sql-database-technical-overview.md) hem teklif [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md).
- [Yönetilen örnek](sql-database-managed-instance.md) Azure SQL veritabanı'nda dağıtım seçeneği sunar yalnızca [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md).
- [Hiper ölçekli hizmet katmanı](sql-database-service-tier-hyperscale.md) kullanıyorsanız, tek veritabanları için sağlanır [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md).

Aşağıdaki tablo ve grafik karşılaştırın ve sanal çekirdek tabanlı hem de DTU tabanlı satın alma modeli:

|**Satın alma modeli**|**Açıklama**|**En iyi**|
|---|---|---|
|DTU tabanlı model|Bu model, işlem, depolama ve g/ç kaynakları ile birlikte gelen bir ölçüyü temel alır. İşlem boyutları tek veritabanı dtu'ları ve elastik havuzlar için esnek veritabanı işlem birimleri (Edtu) cinsinden ifade edilir. Dtu'lar ve Edtu'lar hakkında daha fazla bilgi için bkz: [Dtu'lar ve Edtu'lar nelerdir?](sql-database-purchase-models.md#dtu-based-purchasing-model).|Basit, önceden yapılandırılmış kaynak seçenekleri isteyen müşteriler için idealdir.|
|vCore tabanlı model|Bu model, işlem ve depolama kaynaklarını bağımsız olarak seçmenizi sağlar. Sanal çekirdek tabanlı satın alma modeli de kullanmanıza olanak tanır [SQL Server için Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) maliyet tasarrufu elde etmek için.|Esneklik, Denetim ve saydamlık değerini müşteriler için idealdir.|
||||  

![Fiyatlandırma modeli karşılaştırması](./media/sql-database-service-tiers/pricing-model.png)

## <a name="compute-costs"></a>İşlem maliyetleri

### <a name="provisioned-compute-costs"></a>Sağlanan işlem maliyetleri

Sağlanan işlem katmanında işlem maliyeti uygulaması için sağlanan toplam işlem kapasitesini yansıtır.

İş kritik hizmet katmanında, biz otomatik olarak en az 3 çoğaltma ayırın. Bu ek işlem kaynakları ayrılması yansıtacak şekilde yaklaşık 2.7 x genel amaçlı hizmet katmanında daha yüksek iş kritik hizmet katmanındaki sanal çekirdek tabanlı satın alma modeli fiyatına olduğu. Benzer şekilde, GB başına daha yüksek depolama fiyatı iş kritik hizmet katmanındaki SSD depolama, düşük gecikme süresi ve yüksek g/ç yansıtır.

Her iki katmanda standart depolama kullandığından yedekleme depolama maliyetini iş kritik hizmet katmanı ve genel amaçlı hizmet katmanı için aynıdır.

### <a name="serverless-compute-costs"></a>Sunucusuz bilgi işlem maliyetleri

Sunucusuz bilgi işlem katmanı için işlem kapasitesi nasıl tanımlandığını açıklaması ve maliyetleri hesaplanır için bkz: [sunucusuz SQL veritabanı (Önizleme)](sql-database-serverless.md).

## <a name="storage-costs"></a>Depolama maliyetleri

Farklı depolama türlerini farklı faturalandırılır. Veri depolama için seçtiğiniz en fazla veritabanı veya havuz boyutuna bağlı olarak sağlanan depolama alanı için ücret ödersiniz. Maliyeti azaltmak veya artırmak, en fazla sürece değiştirmez. Yedekleme depolama alanı, örneğinizin otomatik yedekleme işlemleriyle ilişkilidir ve dinamik olarak ayrılır. Yedekleme saklama döneminizin artırılması, örneğiniz tarafından kullanılan yedekleme alanının artırır.

Varsayılan olarak, otomatik yedekleri veritabanlarınızın 7 günlük bir okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) standart Blob Depolama hesabına kopyalanır. Bu depolama, haftalık tam yedeklemeler, günlük fark yedekleri ve 5 dakikada bir kopyalanan işlem günlüğü yedeklemeleri tarafından kullanılır. İşlem günlüklerini boyutuna veritabanı değişiklik oranına bağlıdır. Veritabanı boyutunun yüzde 100'e eşit bir en düşük depolama alanı miktarı, ek ücret alınmadan sağlanır. Ek yedekleme alanı kullanımı, GB cinsinden aylık olarak ücretlendirilir.

Depolama fiyatları hakkında daha fazla bilgi için bkz. [fiyatlandırma](https://azure.microsoft.com/pricing/details/sql-database/single/) sayfası.

## <a name="vcore-based-purchasing-model"></a>Sanal çekirdek tabanlı satın alma modeli

Sanal çekirdek (vCore), mantıksal bir CPU'yu temsil eder ve fiziksel özellikleri, donanım (örneğin, sayı çekirdekleri, bellek ve depolama alanı boyutu) ve donanım Nesilleri arasında seçim seçeneği sunar. Sanal çekirdek tabanlı satın alma modeli, esneklik, denetimi, bireysel kaynak kullanımının saydamlığı sağlar ve basit bir yol çevirmek için şirket iş yükü gereksinimlerini buluta. Bu model, iş yükü gereksinimlerinize bağlı olarak işlem, bellek ve depolama kaynakları seçmenize olanak sağlar.

Sanal çekirdek tabanlı satın alma modeli, arasından seçim yapabilirsiniz [genel amaçlı](sql-database-high-availability.md#basic-standard-and-general-purpose-service-tier-availability) ve [iş açısından kritik](sql-database-high-availability.md#premium-and-business-critical-service-tier-availability) hizmet katmanları için [tek veritabanları](sql-database-single-database-scale.md), [ Elastik havuzlar](sql-database-elastic-pool.md), ve [yönetilen örnekleri](sql-database-managed-instance.md). Tek veritabanları için de seçebilirsiniz [hiper ölçekli hizmet katmanı](sql-database-service-tier-hyperscale.md).

Sanal çekirdek tabanlı satın alma modeli, bağımsız olarak işlem ve depolama kaynaklarını seçin, aynı şirket içi performans ve fiyat iyileştirme olanak tanır. Sanal çekirdek tabanlı satın alma modeli, ödeme yaparsınız:

- İşlem kaynakları (hizmet katmanı + sanal çekirdek sayısı ve bellek + donanımın miktarı).
- Veri ve günlük depolama alanı miktarını ve türünü.
- Yedek depolama (RA-GRS).

> [!IMPORTANT]
> Veri ve günlük depolama, g/ç ve işlem kaynaklarını veritabanınız veya elastik havuz başına ücretlendirilir. Yedekleme depolama alanı, her veritabanı ücretlendirilir. Yönetilen örnek ücretleri hakkında daha fazla bilgi için bkz: [yönetilen örnekleri](sql-database-managed-instance.md).
> **Bölge kısıtlamaları:** Desteklenen bölgelerin güncel listesi için bkz. [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=sql-database&regions=all). Şu anda desteklenmemektedir, bir bölgede yönetilen örnek oluşturma için [Azure portalından bir destek isteği gönderin](sql-database-managed-instance-resource-limits.md#obtaining-a-larger-quota-for-sql-managed-instance).

300'den fazla Dtu, tek veritabanı veya elastik Havuzu'nu kullanırsa, dönüştürme için sanal çekirdek tabanlı satın alma modeli, maliyetleri azaltabilir. Tercih ettiğiniz API kullanarak veya kapalı kalma süresi ile Azure portalını kullanarak dönüştürebilirsiniz. Ancak, dönüştürme gerekli değildir ve otomatik olarak yapılır değil. DTU tabanlı satın alma modeli, performans ve iş gereksinimlerini karşılıyorsa, onu kullanmaya devam etmek.

Sanal çekirdek tabanlı satın alma modeli için DTU tabanlı satın alma modeli dönüştürmek için aşağıdaki kuralları karşısında kullanarak işlem boyutu seçin:

- Her 100 Dtu ve standart katmanda en az 1 sanal çekirdek genel amaçlı hizmet katmanındaki gerektirir.
- Her bir 125 Dtu premium katmanda en az 1 sanal çekirdek iş kritik hizmet katmanındaki gerektirir.

## <a name="dtu-based-purchasing-model"></a>DTU tabanlı satın alma modeli

Veritabanı işlem birimi (DTU), CPU, bellek, okuma ve yazma bir ölçüyle temsil eder. DTU tabanlı satın alma modeli, bir dizi işlem kaynakları, önceden yapılandırılmış paketleri sunar ve dahil edilen depolama alanı için çeşitli uygulama performans düzeylerini sürücü. Her ay önceden yapılandırılmış bir paket ve sabit ödemelerin basitliğini tercih ediyorsunuz, DTU tabanlı model gereksinimlerinize daha uygun olabilir.

DTU tabanlı satın alma modeli, temel, standart ve premium hizmet katmanları her ikisi için seçebileceğiniz [tek veritabanları](sql-database-single-database-scale.md) ve [elastik havuzlar](sql-database-elastic-pool.md). DTU tabanlı satın alma modeli için kullanılabilir değil [yönetilen örnekleri](sql-database-managed-instance.md).

### <a name="database-transaction-units-dtus"></a>Veritabanı işlem birimleri (Dtu)

Belirli bir anda tek bir veritabanı için boyutu içinde işlem bir [hizmet katmanı](sql-database-single-database-scale.md), Microsoft o veritabanı için kaynakların belirli bir düzeyde (bağımsız olarak herhangi bir veritabanı Azure bulutunda) garanti. Bu garanti, öngörülebilir bir performans düzeyi sağlar. Bir veritabanı için ayrılan kaynakların miktarını Dtu sayısı olarak hesaplanır ve işlem, depolama ve g/ç kaynakları ile birlikte gelen bir ölçüdür.

Bu kaynaklar arasındaki oran başlangıçta tarafından belirlenen bir [çevrimiçi işlem gerçekleştirme (OLTP) Kıyaslama iş yükünün](sql-database-benchmark-overview.md) gerçek OLTP iş yükleri için tipik olarak tasarlanmıştır. İş yükünüz şu kaynaklara miktarı aşarsa, daha yavaş performans ve zaman aşımları elde edilen aktarım hızınızı kısıtlanır.

İş yükünüz tarafından kullanılan kaynakları başka bir SQL veritabanına Azure bulutunda kullanılabilir kaynakları etkisi yok. Benzer şekilde, diğer iş yükleri tarafından kullanılan kaynakları SQL veritabanınıza kullanılabilir kaynakları etkisi yoktur.

![sınırlayıcı kutu](./media/sql-database-what-is-a-dtu/bounding-box.png)

Dtu çok farklı bilgi işlem boyutlarına ve hizmet katmanları Azure SQL veritabanları için ayrılan göreli kaynakları anlamak için kullanışlıdır. Örneğin:

- Bir veritabanının işlem boyutunu artırarak dtu'ları Katlama konusu veritabanının kullanabileceği kaynakları kümesi Katlama için karşılık gelmektedir.
- 350 kat daha fazla DTU işlem gücü temel hizmet katmanı veritabanı 5 Dtu'ya sahip 1750 Dtu'ya sahip premium Hizmet katmanını P11 veritabanı sağlar.  

İş yükünüz kaynak (DTU) kullanımını daha derin bir anlayış kazanmak için kullanın [sorgu performansı öngörüleri](sql-database-query-performance.md) için:

- En sık kullanılan sorgular, potansiyel olarak daha iyi performans için ayarlanmış CPU/süresi/yürütme sayısına göre belirleyin. Örneğin, bir g/Ç açısından yoğun sorgu kaldırmadan yararlanabilecek [bellek içi iyileştirme teknikleri](sql-database-in-memory.md) kullanılabilir bellek belirli bir hizmet katmanını ve işlem boyutu daha iyi kullanılmasını sağlamak için.
- Metin ve kaynak kullanımının geçmişini görüntülemek için bir sorgu ayrıntılarına detayına gidin.
- Tarafından gerçekleştirilen eylemleri gösteren performans ayarlama önerileri erişim [SQL veritabanı Danışmanı](sql-database-advisor.md).

### <a name="elastic-database-transaction-units-edtus"></a>Elastik veritabanı işlem birimleri (Edtu)

Her zaman kullanılabilen SQL veritabanları için bunun yerine her zaman gerekebilecek değil, kaynakları (Dtu) ayrılmış bir dizi sağladığı bu veritabanlarına yerleştirebilirsiniz bir [elastik havuz](sql-database-elastic-pool.md). Bir elastik havuzdaki veritabanları, tek bir Azure SQL veritabanı sunucusunda bulunan ve bir kaynak havuzu paylaşın.

Bir elastik havuzdaki paylaşılan kaynakları, elastik veritabanı işlem birimleri (Edtu'lar) tarafından ölçülür. Elastik havuzlar, son derece farklı ve öngörülemeyen kullanım modellerine sahiptir birden çok veritabanı performans hedeflerini yönetmek için basit, az maliyetli bir çözüm sağlar. Bir elastik havuzdaki tüm kaynakları havuzdaki bir veritabanı tarafından havuzdaki her bir veritabanına her zaman gerekli kaynaklara kullanılabilir en düşük düzeyde olduğunu sağlarken tüketilemiyor olduğunu garanti eder.

Bir havuz için belirli bir fiyat sayıda Edtu verilir. Elastik havuzda tek veritabanlarına yapılandırılan sınırlar içinde otomatik olarak ölçeklendirilebilir. Ağır yük altındaki bir veritabanı, talebi karşılamak üzere daha fazla Edtu kullanacaktır. Daha hafif yükler altında veritabanları daha az Edtu kullanacaktır. Hiçbir yük veritabanlarıyla hiçbir Edtu kullanacaktır. Kaynaklar, veritabanı başına yerine tüm havuz için sağlanan olduğundan, elastik havuzlar, yönetim görevlerinizi basitleştirir ve havuz için tahmin edilebilir bir bütçe sağlar.

Mevcut bir havuza veritabanı kapalı kalma süresi olmadan ve havuzdaki veritabanları üzerinde hiçbir etkisi olmadan ek Edtu'lar ekleyebilirsiniz. Ek Edtu'lara artık ihtiyaç duymuyorsanız, benzer şekilde, mevcut bir havuzdan herhangi bir zamanda kaldırın. Ayrıca, veritabanlarına ekleyebilir veya veritabanlarını bir havuzda herhangi bir zamanda çıkarma. Edtu'ları diğer veritabanları için ayırmak üzere ağır bir yük altında bir veritabanı Edtu sayısı sınırı kullanabilirsiniz. Bir veritabanı kaynakları tutarlı bir şekilde underuses havuz dışına taşımak ve tahmin edilebilir miktarda gerekli kaynaklara sahip tek bir veritabanı olarak yapılandırır.

### <a name="determine-the-number-of-dtus-needed-by-a-workload"></a>Bir iş yüküne göre gerekli Dtu sayısını belirlemek

Mevcut bir geçirmek istiyorsanız şirket içi veya SQL Server sanal makine iş yükünü Azure SQL veritabanı, kullanım [DTU hesaplayıcıyı](https://dtucalculator.azurewebsites.net/) gerekli Dtu sayısını yaklaşık olarak belirlemenizi sağlayan. Mevcut bir Azure SQL veritabanı yükünü kullanın [sorgu performansı öngörüleri](sql-database-query-performance.md) veritabanı kaynak tüketiminizi (Dtu'lar) ve iş yükünüz iyileştirmek için daha ayrıntılı Öngörüler edinin. [Sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) dinamik yönetim görünümünü (DMV), son bir saat kaynak tüketimi görüntülemenizi sağlar. [Sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) son 14 gündür, ancak daha düşük bir aslına uygunluk beş dakikalık ortalamalar, kaynak tüketimine katalog görünümünü görüntüler.

### <a name="workloads-that-benefit-from-an-elastic-pool-of-resources"></a>Esnek bir kaynak havuzundan fayda iş yükleri

Havuzlar, veritabanları bir düşük kaynak kullanımı ortalama ve nispeten nadir zamanlarda kullanımın ani için çok uygundur. SQL veritabanı, otomatik olarak mevcut bir SQL veritabanı sunucusundaki veritabanlarının geçmiş kaynak kullanımını değerlendirir ve Azure Portalı'ndaki uygun havuz yapılandırmasını önerir. Daha fazla bilgi için [göz önüne aldığınızda, bir SQL veritabanı esnek havuzunu?](sql-database-elastic-pool.md).

## <a name="frequently-asked-questions-faqs"></a>Sık sorulan sorular (SSS)

### <a name="do-i-need-to-take-my-application-offline-to-convert-from-a-dtu-based-service-tier-to-a-vcore-based-service-tier"></a>DTU tabanlı hizmet katmanından bir sanal çekirdek tabanlı hizmet katmanına dönüştürme yapmak için uygulamamı çevrimdışı duruma getirmem gerekiyor mu?

Hayır. Nastavit aplikaci offline gerek yoktur. Yeni hizmet katmanları, var olan veritabanlarını standart katmandan premium Hizmet katmanını ve tersine yükseltme işleminin benzeyen basit bir çevrimiçi dönüştürme yöntemi sunar. Bu dönüştürme, Azure portalı, PowerShell, Azure CLI, T-SQL veya REST API'yi kullanarak başlatabilirsiniz. Bkz: [tek veritabanlarını yönetmek](sql-database-single-database-scale.md) ve [elastik havuzları yönetme](sql-database-elastic-pool.md).

### <a name="can-i-convert-a-database-from-a-service-tier-in-the-vcore-based-purchasing-model-to-a-service-tier-in-the-dtu-based-purchasing-model"></a>Ben bir veritabanı sanal çekirdek tabanlı satın alma modeli, bir hizmet katmanından bir hizmet katmanı DTU tabanlı satın alma modeli olarak dönüştürebilir miyim?

Evet, kolayca veritabanınız için herhangi bir desteklenen performans hedefi Azure portalı, PowerShell, Azure CLI, T-SQL veya REST API'yi kullanarak dönüştürebilirsiniz. Bkz: [tek veritabanlarını yönetmek](sql-database-single-database-scale.md) ve [elastik havuzları yönetme](sql-database-elastic-pool.md).

## <a name="next-steps"></a>Sonraki adımlar

- Sanal çekirdek tabanlı satın alma modeli hakkında daha fazla bilgi için bkz: [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md).
- DTU tabanlı satın alma modeli hakkında daha fazla bilgi için bkz: [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md).
