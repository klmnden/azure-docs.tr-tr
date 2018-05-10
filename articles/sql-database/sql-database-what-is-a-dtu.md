---
title: 'SQL Veritabanı: DTU nedir? | Microsoft Docs'
description: Azure SQL Veritabanı işlem biriminin ne olduğunu anlama.
keywords: veritabanı seçenekleri,veritabanı performansı
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: article
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: 3fb0d0f73f475e246580f7ea6e4aea60b069c230
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="database-transaction-units-dtus-and-elastic-database-transaction-units-edtus"></a>Veritabanı işlem birimleri (Dtu'lar) ve esnek veritabanı işlem birimleri (Edtu'lar)
Bu makalede, Veritabanı İşlem Birimleri (DTU'lar) ve esnek Veritabanı İşlem Birimlerinin (eDTU'lar) yanı sıra maksimum DTU veya eDTU sayısına ulaşıldığında ne olacağı açıklanmaktadır. Belirli fiyatlandırma bilgileri için bkz: [Azure SQL Database fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/single/).

## <a name="what-are-database-transaction-units-dtus"></a>Veritabanı işlem birimleri (Dtu'lar) nelerdir?
Belirli bir performans düzeyinde içinde tek bir Azure SQL veritabanı için bir [hizmet katmanı](sql-database-single-database-resources.md), Microsoft garanti belirli bir düzeyde veritabanına (herhangi bir Azure bulut veritabanında bağımsız) için kaynaklar sağlayan bir tahmin edilebilir performans düzeyi. Kaynakları miktarını veritabanı işlem birimleri veya Dtu'lar sayısı olarak hesaplanır ve ile birlikte gelen bir işlem, depolama ve g/ç kaynakları ölçüsüdür. Bu kaynaklar arasında oranı başlangıçta tarafından belirlenen bir [OLTP Kıyaslama iş yükü](sql-database-benchmark-overview.md), gerçek OLTP iş yükü tipik olması için tasarlanmıştır. İş yükünüzün bu kaynaklardan herhangi birini miktarını aşarsa, üretilen iş daraltılmış - yavaş performans ve zaman aşımları sonuç. İş yükünüzün kullandığı kaynakları diğer SQL veritabanları Azure bulutta kullanılabilir kaynakları etkilemeyen ve diğer iş yükleri tarafından kullanılan kaynakları SQL veritabanınız kullanılabilir kaynakları etkilemez.

![sınırlama kutusu](./media/sql-database-what-is-a-dtu/bounding-box.png)

Dtu'lar en kaynakları Azure SQL veritabanları farklı performans düzeyleri ve hizmet katmanları arasındaki göreli miktarını anlamak için kullanışlıdır. Örneğin, bir veritabanı performans düzeyini artırarak Dtu'lar Katlama o veritabanına kullanılabilir kaynakları kümesi Katlama için karşılık gelir. Örneğin, 1750 DTU’ya sahip Premium P11 veritabanı 5 DTU’ya sahip Temel veritabanına göre 350 kat daha fazla DTU işlem gücü sağlıyor.  

İş yükünüzün (DTU) kaynak tüketimini daha derin bir anlayış kazanmak için [Azure SQL veritabanı sorgu performansı öngörüleri](sql-database-query-performance.md) için:

- Potansiyel olarak performansı için ayarlanan süre/CPU/yürütme sayısı tarafından en sık kullanılan sorguların tanımlayın. Örneğin, bir g/ç yoğun sorgusu kullanımından yararlanabilir [bellek içi iyileştirme tekniklerini](sql-database-in-memory.md) düzeyde belirli hizmet katmanını ve performans kullanılabilir belleği daha iyi kullanılmasını sağlamak için.
- Bir sorgu ayrıntıları detaya, metin ve kaynak kullanımı geçmişini görüntüleyin.
- Erişim performans tarafından gerçekleştirilen eylemler Göster önerileri ayarlama [SQL veritabanı Danışmanı'nı](sql-database-advisor.md).

Değiştirebileceğiniz [DTU hizmet katmanları](sql-database-service-tiers-dtu.md) (genellikle altında dört saniye ortalaması) uygulamanız için en az kapalı kalma süresi ile herhangi bir zamanda. Veritabanı oluşturabilmek ve veritabanı performansını isteğe göre yükseltip düşürebilmek, özellikle kullanım biçimlerinin nispeten tahmin edilebilir olduğu durumlarda birçok işletme ve uygulama için yeterlidir. Ancak tahmin edilemeyen kullanım biçimlerine sahipseniz bu durum maliyetlerin ve iş modelinizin yönetimini zorlaştırabilir. Bu senaryoda, belirli bir havuzdaki birden fazla veritabanı arasında paylaşılan Edtu sayısı ile bir esnek havuz kullanın.

![SQL Veritabanı'na Giriş: Katmana ve düzeye göre tek veritabanı DTU’ları](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

## <a name="what-are-elastic-database-transaction-units-edtus"></a>Esnek veritabanı işlem birimleri (Edtu'lar) nelerdir?
Bunun yerine her zaman kullanılabilir olan bir SQL veritabanı için her zaman gerekli değildir (Dtu'lar) kaynakları ayrılmış bir dizi sağlayan daha veritabanlarına yerleştireceğiniz bir [esnek havuz](sql-database-elastic-pool.md) bir SQL veritabanı sunucusundaki kaynakları arasında bir havuzu paylaşır Bu veritabanları. Paylaşılan kaynaklar esnek havuzdaki esnek veritabanı işlem birimleri veya Edtu'lar tarafından ölçülür. Esnek havuzlar yaygın olarak değişen sahip birden çok veritabanı ve tahmin edilemeyen kullanım biçimlerine için performans hedeflerini yönetmek için uygun maliyetli bir basit çözüm sağlar. Bir esnek havuz havuzundaki bir veritabanı tarafından her veritabanı havuzundaki her zaman sağlama gerekli kaynaklar kullanılabilir en düşük düzeyde sahipken kaynaklar kullanılamaz güvence altına alır. 

![SQL Veritabanı'na Giriş: Katmana ve düzeye göre eDTU’lar](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Bir havuz Edtu kümesi sayısı için bir set fiyatı verilir. Elastik havuz içerisinde tek veritabanlarına yapılandırılan sınırlar içinde otomatik olarak ölçeklendirme elastikliği tanınır. Bir veritabanını daha ağır yük altında talebi karşılamak üzere daha fazla Edtu'lar tüketir. Açık yükleri altındaki veritabanları daha az Edtu tüketir. Hiçbir yük veritabanlarıyla hiçbir Edtu'lar tüketir. Tüm havuz için kaynakları sağlama tarafından yerine veritabanı başına yönetim görevleri, tahmin edilebilir bir bütçe havuzu için sağlama basitleştirilmiştir.

Mevcut bir havuza, veritabanı kapalı kalma süresi ve havuzdaki veritabanları üzerinde herhangi bir etkisi olmadan ek eDTU’lar eklenebilir. Benzer şekilde, ek eDTU’lara artık ihtiyaç yoksa bunlar mevcut bir havuzdan ne zaman isterseniz kaldırılabilir. Ekleyebilir veya havuza veritabanlarını çıkarma veya sınırı Edtu'lar bir veritabanı miktarını ağır yük altında diğer veritabanları için Edtu ayırmak için kullanabilirsiniz. Bir veritabanı beklendiği altında-kullanılarak durumdaysa kaynakları, havuzu dışına taşıyın ve gereken kaynakları tahmin edilebilir bir miktar tek bir veritabanı olarak yapılandırın.

## <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>İş yükümün ihtiyacı olan DTU sayısını nasıl belirleyebilirim?
Mevcut bir şirket içi veya SQL Server sanal makine iş yükünü Azure SQL Veritabanı’na geçirmek istiyorsanız, gereken yaklaşık DTU sayısını belirlemek için [DTU Hesaplayıcı](http://dtucalculator.azurewebsites.net/)’yı kullanabilirsiniz. Bir mevcut Azure SQL veritabanı iş yükü için kullandığınız [SQL veritabanı sorgu performansı öngörüleri](sql-database-query-performance.md) veritabanı kaynak tüketimini, iş yükü en iyi duruma getirme için daha iyi kavramak için (Dtu'lar) anlamak için. Aynı zamanda [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV son bir saat için kaynak tüketimini görüntüleyin. Alternatif olarak, katalog görünümünü [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) son 14 gündür, ancak daha düşük bir doğruluğu beş dakikalık ortalamalar, kaynak tüketimini görüntüler.

## <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Esnek bir kaynak havuzundan fayda sağlayıp sağlayamayacağımı nasıl öğrenebilirim?
Havuzlar, belirli kullanım düzenlerine sahip çok sayıda veritabanı bulunan durumlar için uygundur. Verilen bir veritabanı için bu deseni oldukça sık kullanımı ani bir düşük kullanımı ortalama tarafından belirlenir. SQL Veritabanı, mevcut bir SQL Veritabanı sunucusundaki veritabanlarının geçmiş kaynak kullanımını otomatik olarak değerlendirir ve Azure portalda uygun havuz yapılandırmasını önerir. Daha fazla bilgi için bkz. [ne zaman elastik bir havuz kullanılması gerekir?](sql-database-elastic-pool.md)

## <a name="what-happens-when-i-hit-my-maximum-dtus"></a>My maksimum Dtu isabet ne olur?
Performans düzeyleri kalibre ve veritabanının yükünüzü için seçilen hizmet katmanı/performans düzeyi izin verilen maksimum kadar çalıştırmak için gerekli kaynakları sağlamak üzere kapsamındadır. İş yükünüzün CPU/Data GÇ/günlük GÇ sınırlardan biri basarsa, kaynakları izin verilen en fazla düzeyi almaya devam eder, ancak artırılmış sorgu gecikme süreleri de büyük olasılıkla karşılaşırsınız. Bu sınırlar herhangi bir hataya yol açmaz, yalnızca iş yükünü yavaşlatır. Yavaşlama sorguların zaman aşımına uğramasına sebep olacak kadar şiddetli hale gelirse hatayla karşılaşırsınız. İzin verilen en fazla eş zamanlı kullanıcı oturumları/istekleri (çalışan iş parçacıkları) ulaştıysanız, açık hataları görürsünüz. Bkz: [Azure SQL veritabanı kaynak sınırları]( sql-database-dtu-resource-limits.md#what-happens-when-database-and-elastic-pool-resource-limits-are-reached) CPU, bellek, veri g/ç veya işlem günlük GÇ ilgili olmayan kaynak sınırları hakkında bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
* Bkz: [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) Dtu ve Edtu tek veritabanları ve esnek havuzlar için kullanılabilir bilgi, yanı sıra CPU dışındaki kaynaklarda sınırları, bellek, veri g/ç ve işlem günlük GÇ.
* Bkz: [vCore tabanlı satın alma modeli (Önizleme)](sql-database-service-tiers-vcore.md) vCore tabanlı kaynak ayırma ve hizmet katmanları hakkında bilgi için. 
* DTU tüketiminizi anlamak için bkz. [SQL Veritabanı Sorgu Performansı İçgörüleri](sql-database-query-performance.md).
* DTU karışımını belirlemek için kullanılan OLTP kıyaslama iş yükünün arkasındaki metodolojiyi anlamak için bkz. [SQL Database benchmark overview](sql-database-benchmark-overview.md) (SQL Veritabanı kıyaslamaya genel bakış).
