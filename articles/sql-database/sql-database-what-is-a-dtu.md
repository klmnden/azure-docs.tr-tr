---
title: "SQL Veritabanı: DTU nedir? | Microsoft Belgeleri"
description: "Azure SQL Veritabanı işlem biriminin ne olduğunu anlama."
keywords: "veritabanı seçenekleri,veritabanı performansı"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: CarlRabeler
ms.assetid: 89e3e9ce-2eeb-4949-b40f-6fc3bf520538
ms.service: sql-database
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: NA
ms.date: 09/06/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 5a101aa78dbac4f1a0edb7f414b44c14db392652
ms.openlocfilehash: e062d55e990faeb0776f643b297788afd87ac1e3


---
# <a name="explaining-database-transaction-units-dtus-and-elastic-database-transaction-units-edtus"></a>Veritabanı İşlem Birimlerini (DTU'lar) ve esnek Veritabanı İşlem Birimlerini (eDTU'lar) açıklama
Bu makalede, Veritabanı İşlem Birimleri (DTU'lar) ve esnek Veritabanı İşlem Birimlerinin (eDTU'lar) yanı sıra maksimum DTU veya eDTU sayısına ulaşıldığında ne olacağı açıklanmaktadır.  

## <a name="what-are-database-transaction-units-dtus"></a>Veritabanı İşlem Birimleri (DTU'lar) nedir?
DTU, tek başına bir Azure SQL veritabanı için [tek başına bir veritabanı hizmet katmanı](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels) içinde belirli bir performans düzeyinde kullanılabilir olması garantilenen kaynaklara yönelik bir ölçü birimidir. DTU, gerçek OLTP iş yüklerine örnek olmak üzere tasarlanmış bir OLTP kıyaslama iş yükü tarafından belirlenen bir oranda CPU, bellek, veri G/Ç ve işlem günlüğü G/Ç karışımından oluşan bir ölçüdür. Veritabanının performans düzeyini artırarak DTU'ları iki katına çıkarmak, söz konusu veritabanının kullanabileceği kaynakları iki katına çıkarmaya eşittir. Örneğin, 1750 DTU’ya sahip Premium P11 veritabanı 5 DTU’ya sahip Temel veritabanına göre 350 kat daha fazla DTU işlem gücü sağlıyor. DTU karışımını belirlemek için kullanılan OLTP kıyaslama iş yükünün arkasındaki metodolojiyi anlamak için bkz. [SQL Database benchmark overview](sql-database-benchmark-overview.md) (SQL Veritabanı kıyaslamaya genel bakış).

![SQL Veritabanı'na Giriş: Katmana ve düzeye göre tek veritabanı DTU’ları](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

Uygulamanızda çok az (genellikle ortalama dört saniyenin altında) kesinti ile dilediğiniz zaman [hizmet katmanlarını değiştirebilirsiniz](sql-database-scale-up.md). Veritabanı oluşturabilmek ve tek veritabanı performansını isteğe göre yükseltip düşürebilmek, özellikle kullanım biçimlerinin nispeten tahmin edilebilir olduğu durumlarda birçok işletme ve uygulama için yeterlidir. Ancak tahmin edilemeyen kullanım biçimlerine sahipseniz bu durum maliyetlerin ve iş modelinizin yönetimini zorlaştırabilir. Bu senaryoda, belirli bir sayıda eDTU bulunduran esnek bir havuz kullanılmaktadır.

## <a name="what-are-elastic-database-transaction-units-edtus"></a>Esnek Veritabanı İşlem Birimleri (eDTU'lar) nedir?
eDTU, Azure SQL sunucusundaki bir grup veritabanı arasında paylaşılan bir kaynak (DTU) kümesine yönelik bir ölçü birimidir. Bu tür kaynak kümelerine [esnek havuz](sql-database-elastic-pool.md) adı verilir. Esnek havuzlar, son derece farklı ve öngörülemeyen kullanım düzenlerine sahip birden çok veritabanına ilişkin performans hedeflerini yönetmek için basit ve uygun maliyetli bir çözüm sağlar. Daha fazla bilgi edinmek için bkz. [esnek havuzlar ve hizmet katmanları](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus).

![SQL Veritabanı'na Giriş: Katmana ve düzeye göre eDTU’lar](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Havuza belirli bir fiyat karşılığında, belirli bir sayıda eDTU verilir. Havuz içerisinde tek tek veritabanlarına belirli parametreler içinde otomatik olarak ölçeklendirme esnekliği tanınır. Veritabanı, yoğun bir yük altındayken talebi karşılamak üzere daha fazla eDTU kullanabilir. Yükü az olan veritabanları daha az eDTU kullanır ve yükü bulunmayan veritabanları eDTU kullanmaz. Tek tek veritabanları yerine tüm havuz için kaynak sağlamak, yönetim görevlerinizi basitleştirir. Ayrıca, havuza yönelik bütçeniz tahmin edilebilir bir hale gelir.

Mevcut bir havuza, veritabanı kapalı kalma süresi veya esnek havuzdaki veritabanları üzerinde herhangi bir etkisi olmadan ek eDTU’lar eklenebilir. Benzer şekilde, ek eDTU’lara artık ihtiyaç yoksa bunlar mevcut bir havuzdan ne zaman isterseniz kaldırılabilir. Havuza veritabanları ekleyebilir veya havuzdan veritabanları kaldırabilirsiniz. Bir veritabanı kaynakları tahmin edilebilir bir şekilde normalden az kullanıyorsa bu veritabanını havuzdan çıkarın.

## <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>İş yükümün ihtiyacı olan DTU sayısını nasıl belirleyebilirim?
Mevcut bir şirket içi veya SQL Server sanal makine iş yükünü Azure SQL Veritabanı’na geçirmek istiyorsanız, gereken yaklaşık DTU sayısını belirlemek için [DTU Hesaplayıcı](http://dtucalculator.azurewebsites.net/)’yı kullanabilirsiniz. Mevcut bir Azure SQL Veritabanı iş yükünüzü iyileştirecek öngörüleri kazanmak amacıyla veritabanı kaynak tüketiminizi (DTU'lar) daha iyi anlamak için [SQL Veritabanı Sorgu Performansı Öngörüleri](sql-database-query-performance.md)’ni kullanabilirsiniz. [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV’sini kullanarak son bir saate ait kaynak tüketimi bilgilerini de alabilirsiniz. Alternatif olarak, [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) katalog görünümü sorgulanarak aynı veriler son 14 gün için de alınabilir. Ancak bu veriler, beş dakikalık ortalamalar şeklinde daha düşük bir aslına uygunluk düzeyindedir.

## <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Esnek bir kaynak havuzundan fayda sağlayıp sağlayamayacağımı nasıl öğrenebilirim?
Havuzlar, belirli kullanım düzenlerine sahip çok sayıda veritabanı bulunan durumlar için uygundur. Söz konusu kullanım düzeni, belirli bir veritabanı için ortalama düşük düzeyde kullanım ile nispeten nadir zamanlarda kullanımın ani olarak artması şeklindedir. SQL Veritabanı, mevcut bir SQL Veritabanı sunucusundaki veritabanlarının geçmiş kaynak kullanımını otomatik olarak değerlendirir ve Azure portalda uygun havuz yapılandırmasını önerir. Daha fazla bilgi için bkz. [ne zaman esnek bir veritabanı havuzu kullanılması gerekir?](sql-database-elastic-pool-guidance.md)

## <a name="what-happens-when-i-hit-my-maximum-dtus"></a>Maksimum DTU sayıma ulaştığımda ne olur?
Performans düzeyleri, veritabanı iş yükünüzü seçilen hizmet katmanı/performans düzeyiniz için izin verilen en üst sınırlara kadar çalıştırmak üzere gereken kaynakları sağlamak için ayarlanıp yönetilir. İş yükünüz, CPU/Veri GÇ/Günlük GÇ sınırlarından birine ulaşıyorsa kaynakları, izin verilen maksimum düzeyde almaya devam ederseniz, ancak sorgularınızda artan gecikme süreleriyle karşılaşmanız olasıdır. Bu sınırlar herhangi bir hataya yol açmaz, yalnızca iş yükünü yavaşlatır. Yavaşlama sorguların zaman aşımına uğramasına sebep olacak kadar şiddetli hale gelirse hatayla karşılaşırsınız. İzin verilen en yüksek eş zamanlı kullanıcı oturumu/isteği (çalışan iş parçacıkları) sayısı sınırına ulaşırsanız açık hatalar görürsünüz. CPU, bellek, veri G/Ç ve işlem günlüğü G/Ç kaynakları dışındaki kaynaklara yönelik sınırlar hakkında bilgi edinmek için bkz. [Azure SQL Database resource limits](sql-database-resource-limits.md) (Azure SQL Veritabanı kaynak sınırları).

## <a name="next-steps"></a>Sonraki adımlar
* Tek başına veritabanları ve esnek havuzlara yönelik olarak kullanılabilecek DTU'lar ve eDTU'lar hakkında daha fazla bilgi edinmek için bkz. [Hizmet katmanı](sql-database-service-tiers.md).
* CPU, bellek, veri G/Ç ve işlem günlüğü G/Ç kaynakları dışındaki kaynaklara yönelik sınırlar hakkında bilgi edinmek için bkz. [Azure SQL Database resource limits](sql-database-resource-limits.md) (Azure SQL Veritabanı kaynak sınırları).
* DTU tüketiminizi anlamak için bkz. [SQL Veritabanı Sorgu Performansı Öngörüleri](sql-database-query-performance.md).
* DTU karışımını belirlemek için kullanılan OLTP kıyaslama iş yükünün arkasındaki metodolojiyi anlamak için bkz. [SQL Database benchmark overview](sql-database-benchmark-overview.md) (SQL Veritabanı kıyaslamaya genel bakış).



<!--HONumber=Nov16_HO2-->


