---
title: Azure SQL veritabanı'nı satın alma modeli | Microsoft Docs
description: Azure SQL veritabanı hizmetinde kullanılabilir veritabanları satın alma modelleri modeli hakkında bilgi edinin.
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
ms.date: 02/08/2019
ms.openlocfilehash: 46a620900896d07273da22e53171330b85d3f1ec
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59360179"
---
# <a name="azure-sql-database-purchasing-models"></a>Azure SQL veritabanı'nı satın alma modeli

Azure SQL veritabanı, performans ve maliyet ihtiyaçlarınıza uygun veritabanı altyapısı tam olarak yönetilen PaaS kolayca satın almanızı sağlar. Azure SQL veritabanı dağıtım modeline bağlı olarak gereksinimlerinize uyan bir satın alma modeli seçebilirsiniz:

- [Sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) (önerilen) depolama kapasitesi miktarda seçin ve iş yükünüz için gereken işlem olanak sağlar.
- [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) seçebileceğiniz ortak iş yükleri için dengeli işlem ve depolama paketleri paket.

Farklı satın alma modelleri, Azure SQL veritabanı dağıtım modellerinde kullanılabilir:

- [Tek veritabanı](sql-database-single-databases-manage.md) ve [elastik havuz](sql-database-elastic-pool.md) dağıtım seçenekleri [Azure SQL veritabanı](sql-database-technical-overview.md) hem teklif [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md).
- [Yönetilen örnek](sql-database-managed-instance.md) yalnızca Azure SQL veritabanı'nda dağıtım seçeneği sunar [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md).

> [!IMPORTANT]
> [Hiper ölçekli hizmet Katmanı (Önizleme)](sql-database-service-tier-hyperscale.md) satın alma modeli sanal çekirdek kullanarak yalnızca tek veritabanları için genel önizlemeye sunulmuştur.

Aşağıdaki tablo ve grafik karşılaştırın ve bu iki satın alma modeli.

|**Satın alma modeli**|**Açıklama**|**En iyi**|
|---|---|---|
|DTU tabanlı model|Bu model, işlem, depolama ve GÇ kaynakları ile birlikte gelen bir ölçüyü temel alır. İşlem boyutları, tek veritabanları için veritabanı işlem birimleri (Dtu'lar) ve elastik havuzlar için esnek veritabanı işlem birimleri (Edtu) cinsinden ifade edilir. Dtu'lar ve Edtu'lar hakkında daha fazla bilgi için bkz. [Dtu'lar ve Edtu'lar nelerdir?](sql-database-purchase-models.md#dtu-based-purchasing-model).|Basit, önceden yapılandırılmış kaynak seçenekleri isteyen müşteriler için idealdir.|
|vCore tabanlı model|Bu model, işlem ve depolama kaynaklarını bağımsız olarak seçmenizi sağlar. Sanal çekirdek tabanlı satın alma modeli de kullanmanıza olanak tanır [SQL Server için Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) maliyet tasarrufu elde etmek için.|Esneklik, Denetim ve saydamlık değerini müşteriler için idealdir.|
||||  

![Fiyatlandırma modeli](./media/sql-database-service-tiers/pricing-model.png)

## <a name="compute-costs"></a>İşlem maliyetleri

İşlem maliyet, uygulama için sağlanan toplam işlem kapasitesini yansıtır. İş kritik hizmet katmanında, biz otomatik olarak en az 3 çoğaltma ayırın. Bu ek işlem kaynakları ayrılması yansıtacak şekilde yaklaşık 2.7 x daha yüksek iş kritik hizmet katmanındaki genel amaçlı hizmet katmanındaki sanal çekirdek tabanlı satın alma modeli fiyatına değeridir. Aynı nedenden dolayı GB başına daha yüksek depolama fiyatı iş kritik hizmet katmanındaki SSD depolama, düşük gecikme süresi ve yüksek g/ç yansıtır. Her iki durumda da bir standart depolama sınıfı kullandığımızdan aynı zamanda, yedekleme depolama maliyeti, bu iki hizmet katmanları arasında farklı değildir.

## <a name="storage-costs"></a>Depolama maliyetleri

Farklı depolama türlerini farklı faturalandırılır. Veri depolama için seçtiğiniz en fazla veritabanı veya havuz boyutuna bağlı olarak sağlanan depolama alanı için ücretlendirilirsiniz. Maliyeti azaltmak veya artırmak, en fazla sürece değiştirmez. Yedekleme depolama alanı, örneğinizin otomatik yedekleme işlemleriyle ilişkilidir ve dinamik olarak ayrılır. Yedekleme saklama döneminizin artırılması, örneğiniz tarafından kullanılan yedekleme alanının artmasına neden olur.

Veritabanlarınızın 7 günlük otomatik yedeklemeleri, varsayılan olarak RA-GRS Standart blob depolamasına kopyalanır. Depolama alanı, haftalık tam yedeklemeler, günlük fark yedekleri ve 5 dakikada bir kopyalanan işlem günlüğü yedeklemeleri tarafından kullanılır. İşlem günlüğü boyutu veritabanı değişiklik oranına bağlıdır. Veritabanı boyutunun %100’üne eşit bir depolama alt sınırı ek ücret alınmadan sağlanır. Ek yedekleme alanı kullanımı, GB/ay üzerinden ücretlendirilir.

Depolama fiyatları hakkında daha fazla bilgi için bkz. [fiyatlandırma](https://azure.microsoft.com/pricing/details/sql-database/single/) sayfası.

## <a name="vcore-based-purchasing-model"></a>Sanal çekirdek tabanlı satın alma modeli

Sanal çekirdek, fiziksel donanım (örneğin, sayı çekirdekleri, bellek, depolama boyutu) özelliklerini ve donanım Nesilleri arasında seçim yapma olanağı ile sunulan mantıksal CPU'yu temsil eder. Sanal çekirdek tabanlı satın alma modeli, esneklik, Denetim, saydamlık bireysel kaynak kullanımının ve şirket içi iş yükü gereksinimlerini buluta çevirmek için basit bir yol sağlar. Bu model, işlem, bellek ve depolama iş yükü gereksinimlerine göre seçmenize olanak sağlar. Sanal çekirdek tabanlı satın alma modeli, arasından seçim yapabilirsiniz [genel amaçlı](sql-database-high-availability.md#basic-standard-and-general-purpose-service-tier-availability) ve [iş açısından kritik](sql-database-high-availability.md#premium-and-business-critical-service-tier-availability) hizmet katmanları için [tek veritabanları](sql-database-single-database-scale.md), [ Elastik havuzlar](sql-database-elastic-pool.md), ve [yönetilen örnekleri](sql-database-managed-instance.md). Tek veritabanları için de seçebilirsiniz [hiper ölçekli hizmet Katmanı (Önizleme)](sql-database-service-tier-hyperscale.md).

Sanal çekirdek tabanlı satın alma modeli, bağımsız olarak işlem ve depolama kaynaklarını seçin, aynı şirket içi performans ve fiyat iyileştirme sağlar. Sanal çekirdek tabanlı satın alma modeli, müşteriler için ödeme yapar:

- İşlem (hizmet katmanı + sanal çekirdek sayısı ve bellek + donanımın miktarı)
- Tür ve veri ve günlük depolama miktarı
- Yedek depolama (RA-GRS)

> [!IMPORTANT]
> İşlem, IOs, veri ve günlük depolama, veritabanı veya elastik havuz başına ücretlendirilir. Yedekleme depolama, her veritabanı ücretlendirilir. Yönetilen örnek ücretleri hakkında daha fazla bilgi için bkz: [yönetilen örnekleri](sql-database-managed-instance.md).
> **Bölge kısıtlamaları:** Desteklenen bölgelerin güncel listesi için bkz. [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=sql-database&regions=all). Şu anda desteklenmeyen bir bölgede bir yönetilen örnek oluşturmak istiyorsanız, aşağıdakileri yapabilirsiniz [Azure portalı üzerinden destek isteği Gönder](sql-database-managed-instance-resource-limits.md#obtaining-a-larger-quota-for-sql-managed-instance).
.

300'den fazla Dtu, tek veritabanı veya elastik Havuzu'nu kullanırsa, sanal çekirdek tabanlı satın alma modeli için dönüştürme maliyetinizi azaltabilir. Dönüştürülecek karar verirseniz, API'nizi tercih ettiğiniz veya kapalı kalma süresi ile Azure portalını kullanarak dönüştürebilirsiniz. Ancak, dönüştürme gerekli değildir ve otomatik olarak yapılmaz. DTU tabanlı satın alma modeli, performans ve iş gereksinimlerini karşılıyorsa, onu kullanmaya devam etmek. Sanal çekirdek tabanlı satın alma modeli için DTU tabanlı satın alma modeli dönüştürmeye karar verirseniz, aşağıdaki kurallar karşısında kullanarak işlem boyutu seçin:

- Standart katmandaki her 100 DTU genel amaçlı katmanında en az 1 sanal çekirdek gerektirir
- Her Premium katmanda 125 DTU iş açısından kritik katmanında en az 1 sanal çekirdek gerektirir

## <a name="dtu-based-purchasing-model"></a>DTU tabanlı satın alma modeli

Veritabanı işlem birimi (DTU) temsil eden bir ölçüyle CPU, bellek, okur ve yazar. DTU tabanlı satın alma modeli, bir dizi işlem kaynakları, önceden yapılandırılmış paketleri sunar ve dahil edilen depolama alanı için çeşitli uygulama performans düzeylerini sürücü. Önceden yapılandırılmış bir paket sabit ödemeler ve Basitlik, her ay tercih eden müşteriler bulun DTU tabanlı model ihtiyaçları için daha uygun. DTU tabanlı satın alma modeli, müşteriler arasında seçim yapabilirsiniz **temel**, **standart**, ve **premium** hizmet katmanları için her ikisi de [tek veritabanları](sql-database-single-database-scale.md) ve [elastik havuzlar](sql-database-elastic-pool.md). Bu satın alma modeli kullanıma sunulmadı [yönetilen örnekleri](sql-database-managed-instance.md).

### <a name="database-transaction-units-dtus"></a>Veritabanı işlem birimleri (Dtu)

Belirli bir anda tek bir veritabanı için boyutu içinde işlem bir [hizmet katmanı](sql-database-single-database-scale.md), Microsoft, tahmin edilebilir bir düzeyde sağlayan kaynaklar (bağımsız olarak herhangi bir veritabanı Azure bulutunda) Bu veritabanı, belirli bir düzeyde garanti eder performans. Kaynakların miktarını, veritabanı işlem birimleri veya Dtu'lar sayısı hesaplanır ve işlem, depolama ve GÇ kaynakları ile birlikte gelen bir ölçüdür. Bu kaynaklar arasındaki oran başlangıçta tarafından belirlenen bir [OLTP Kıyaslama iş yükünün](sql-database-benchmark-overview.md), gerçek OLTP iş yükleri tipik olacak şekilde tasarlanmıştır. İş yükünüz şu kaynaklara miktarı aşarsa, aktarım hızınızı daraltılmış - yavaş performans ve zaman aşımları sonuç. İş yükünüz tarafından kullanılan kaynakları başka bir SQL veritabanına Azure bulutunda kullanılabilir kaynakları etkilemez ve diğer iş yükleri tarafından kullanılan kaynakları SQL veritabanınıza kullanılabilir kaynakları etkilemez.

![sınırlayıcı kutu](./media/sql-database-what-is-a-dtu/bounding-box.png)

Dtu en kaynakları farklı bilgi işlem boyutlarına, Azure SQL veritabanı hizmet katmanları arasındaki göreli miktarını anlamak için kullanışlıdır. Örneğin, bir veritabanı işlem boyutunu artırarak dtu'ları Katlama konusu veritabanının kullanabileceği kaynakları kümesi Katlama için karşılık gelmektedir. Örneğin, 1750 DTU’ya sahip Premium P11 veritabanı 5 DTU’ya sahip Temel veritabanına göre 350 kat daha fazla DTU işlem gücü sağlıyor.  

İş yükünüz kaynak (DTU) kullanımını daha derin bir anlayış kazanmak için kullanın [sorgu performansı öngörüleri](sql-database-query-performance.md) için:

- En sık kullanılan sorgular, potansiyel olarak daha iyi performans için ayarlanmış CPU/süresi/yürütme sayısına göre belirleyin. Örneğin, bir g/ç kullanımı yoğun sorgu kullanımından yararlanabilir [bellek içi iyileştirme teknikleri](sql-database-in-memory.md) kullanılabilir bellek belirli bir hizmet katmanını ve işlem boyutu daha iyi kullanılmasını sağlamak için.
- Bir sorgunun ayrıntılarına detaya, kendi metin ve kaynak kullanımı geçmişini görüntüleyin.
- Erişim performans tarafından gerçekleştirilen eylemleri göster önerilerinde [SQL veritabanı Danışmanı](sql-database-advisor.md).

### <a name="elastic-database-transaction-units-edtus"></a>Elastik veritabanı işlem birimleri (Edtu)

Bunun yerine her zaman kullanılabilir bir SQL veritabanı için her zaman gerekli değildir (Dtu) kaynakları ayrılmış bir dizi sağlamak daha veritabanlarına yerleştirebilirsiniz bir [elastik havuz](sql-database-elastic-pool.md) bir SQL veritabanı sunucusundaki kaynakları arasında bir havuzu paylaşır Bu veritabanları. Bir elastik havuzdaki paylaşılan kaynakları, elastik veritabanı işlem birimleri veya Edtu'lar tarafından ölçülür. Elastik havuzlar, birden çok veritabanına sahip son derece farklı ve öngörülemeyen kullanım düzenlerine ilişkin performans hedeflerini yönetmek için basit ve uygun maliyetli çözüm sağlar. Elastik havuz, havuzdaki bir veritabanı tarafından havuzdaki her bir veritabanına her zaman sağlamak gerekli kaynakları kullanılabilir en düşük düzeyde sahipken kaynakları tüketilemiyor garanti eder.

Bir havuz için belirli bir fiyat sayıda Edtu verilir. Elastik havuz içerisinde tek veritabanlarına yapılandırılan sınırlar içinde otomatik olarak ölçeklendirme elastikliği tanınır. Ağır yük altındaki bir veritabanı, talebi karşılamak üzere daha fazla Edtu kullanacaktır. Daha hafif yükler altında veritabanları daha az Edtu kullanacaktır. Hiçbir yük veritabanlarıyla hiçbir Edtu kullanacaktır. Tüm havuz için kaynakları sağlama tarafından yerine veritabanı başına yönetim görevleri, havuz için tahmin edilebilir bir bütçe sağlama basitleştirilmiştir.

Mevcut bir havuza, veritabanı kapalı kalma süresi ve havuzdaki veritabanları üzerinde herhangi bir etkisi olmadan ek eDTU’lar eklenebilir. Benzer şekilde, ek eDTU’lara artık ihtiyaç yoksa bunlar mevcut bir havuzdan ne zaman isterseniz kaldırılabilir. Ekleyebilir veya havuzdan veritabanları kaldırabilirsiniz veya edtu'ları diğer veritabanları için ayırmak üzere ağır yük altında bir veritabanı Edtu miktarını sınırlayabilirsiniz kullanabilirsiniz. Bir veritabanını havuzdan durumdaysa kaynakları, havuz dışına taşımak ve tahmin edilebilir miktarda gerekli kaynaklara sahip tek bir veritabanı olarak yapılandırır.

### <a name="determine-the-number-of-dtus-needed-by-a-workload"></a>Bir iş yüküne göre gerekli Dtu sayısını belirlemek

Bir mevcut şirket içi veya kullanabileceğiniz SQL Server sanal makine iş yükünü Azure SQL veritabanına geçirmek istiyorsanız [DTU hesaplayıcıyı](https://dtucalculator.azurewebsites.net/) gerekli Dtu sayısını yaklaşık olarak belirlemenizi sağlayan. Mevcut bir Azure SQL veritabanı iş yükünü için kullandığınız [sorgu performansı içgörüleri](sql-database-query-performance.md) veritabanı kaynak tüketiminizi (Dtu'lar) ilişkin iş yükünüz iyileştirmek için daha kapsamlı içgörüler anlamak için. Ayrıca [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV'sini kullanarak son bir saat kaynak tüketimi görüntüleyin. Alternatif olarak, katalog görünümünü [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) son 14 gündür, ancak daha düşük bir aslına uygunluk beş dakikalık ortalamalar kaynak tüketimini görüntüler.

### <a name="workloads-that-benefit-from-an-elastic-pool-of-resources"></a>Esnek bir kaynak havuzundan fayda iş yükleri

Havuzlar, belirli kullanım düzenlerine sahip çok sayıda veritabanı bulunan durumlar için uygundur. Belirli bir veritabanı için bu düzen bir düşük kullanımı ortalama nispeten nadir zamanlarda kullanımın ani olarak artması şeklindedir. SQL Veritabanı, mevcut bir SQL Veritabanı sunucusundaki veritabanlarının geçmiş kaynak kullanımını otomatik olarak değerlendirir ve Azure portalda uygun havuz yapılandırmasını önerir. Daha fazla bilgi için bkz. [ne zaman elastik bir havuz kullanılması gerekir?](sql-database-elastic-pool.md)

## <a name="purchase-model-frequently-asked-questions-faq"></a>Sık sorulan sorular (SSS) satın alma modeli

### <a name="do-i-need-to-take-my-application-offline-to-convert-from-a-dtu-based-database-to-a-vcore-based-service-tier"></a>DTU tabanlı bir veritabanından sanal çekirdek tabanlı hizmet katmanı için dönüştürmek için uygulamamı çevrimdışı duruma getirmem gerekiyor mu

Yeni hizmet katmanları, Standard hizmet katmanından Premium hizmet katmanına ve tam tersi yönde veritabanı yükseltmesi için kullanılan mevcut işleme benzeyen basit bir çevrimiçi dönüştürme yöntemi sunar. Bu dönüştürme, Azure portalı, PowerShell, Azure CLI, T-SQL veya REST API kullanılarak başlatılabilir. Bkz: [tek veritabanlarını yönetmek](sql-database-single-database-scale.md) ve [elastik havuzları yönetme](sql-database-elastic-pool.md).

### <a name="can-i-convert-a-database-from-a-service-tier-using-the-vcore-based-purchase-to-a-service-tier-using-the-dtu-based-purchasing-model"></a>Bir veritabanı sanal çekirdek tabanlı satın alma için DTU tabanlı satın alma modeli kullanarak bir hizmet katmanı kullanarak bir hizmet katmanından dönüştürebilirsiniz

Evet, Azure portalı, PowerShell, Azure CLI, T-SQL veya REST API'yi kullanarak tüm desteklenen performans hedefi veritabanınızı kolayca dönüştürebilirsiniz. Bkz: [tek veritabanlarını yönetmek](sql-database-single-database-scale.md) ve [elastik havuzları yönetme](sql-database-elastic-pool.md).

## <a name="next-steps"></a>Sonraki adımlar

- Sanal çekirdek tabanlı satın alma modeli için bkz: [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md)
- DTU tabanlı satın alma modeli için bkz: [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md).
