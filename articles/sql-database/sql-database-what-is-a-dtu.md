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
ms.openlocfilehash: 178eba46e0d128c8d93f2ba664a4a0916889fbbd
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="database-transaction-units-dtus-and-elastic-database-transaction-units-edtus"></a>Veritabanı işlem birimleri (Dtu'lar) ve esnek veritabanı işlem birimleri (Edtu'lar)
Bu makalede, Veritabanı İşlem Birimleri (DTU'lar) ve esnek Veritabanı İşlem Birimlerinin (eDTU'lar) yanı sıra maksimum DTU veya eDTU sayısına ulaşıldığında ne olacağı açıklanmaktadır.  

## <a name="what-are-database-transaction-units-dtus"></a>Veritabanı işlem birimleri (Dtu'lar) nelerdir?
Belirli bir performans düzeyinde içinde tek bir Azure SQL veritabanı için bir [hizmet katmanı](sql-database-single-database-resources.md), Microsoft, belirli bir düzeyde (herhangi bir Azure bulut veritabanında bağımsız) veritabanının ve öngörülebilir bir performans düzeyini sağlamak için kaynaklar güvence altına alır. Bu miktarda kaynak veritabanı işlem birimleri veya Dtu'lar sayısı olarak hesaplanır ve ile birlikte gelen bir işlem, depolama ve g/ç kaynakları ölçüsüdür. Bu kaynaklar arasında oranı başlangıçta tarafından belirlenen bir [OLTP Kıyaslama iş yükü](sql-database-benchmark-overview.md) gerçek OLTP iş yükü tipik olarak tasarlanmıştır. İş yükünüzün bu kaynaklardan herhangi birini miktarını aşarsa, üretilen iş daraltılmış - yavaş performans ve zaman aşımları sonuç. İş yükünüzün kullandığı kaynakları diğer SQL veritabanları Azure bulutta kullanılabilir kaynakları etkilemeyen ve diğer iş yükleri tarafından kullanılan kaynak SQL veritabanınız kullanılabilir kaynakları etkileyen değil.

![sınırlama kutusu](./media/sql-database-what-is-a-dtu/bounding-box.png)

Dtu'lar en kaynakları Azure SQL veritabanları farklı performans düzeyleri ve hizmet katmanları arasındaki göreli miktarını anlamak için kullanışlıdır. Örneğin, bir veritabanı performans düzeyini artırarak Dtu'lar Katlama o veritabanına kullanılabilir kaynak kümesi Katlama için karşılık gelir. Örneğin, 1750 DTU’ya sahip Premium P11 veritabanı 5 DTU’ya sahip Temel veritabanına göre 350 kat daha fazla DTU işlem gücü sağlıyor.  

İş yükünüzün (DTU) kaynak tüketimini daha derin bir anlayış kazanmak için [Azure SQL veritabanı sorgu performansı öngörüleri](sql-database-query-performance.md) için:

- Potansiyel olarak performansı için ayarlanan süre/CPU/yürütme sayısı tarafından en sık kullanılan sorguların tanımlayın. Örneğin, bir g/ç yoğun sorgusu kullanımından yararlanabilir [bellek içi iyileştirme tekniklerini](sql-database-in-memory.md) düzeyde belirli hizmet katmanını ve performans kullanılabilir belleği daha iyi kullanılmasını sağlamak için.
- Bir sorgu ayrıntıları detaya, metin ve kaynak kullanımı geçmişini görüntüleyin.
- Erişim performans tarafından gerçekleştirilen eylemler Göster önerileri ayarlama [SQL veritabanı Danışmanı'nı](sql-database-advisor.md).

Uygulamanızda çok az (genellikle ortalama dört saniyenin altında) kesinti ile dilediğiniz zaman [hizmet katmanlarını değiştirebilirsiniz](sql-database-service-tiers.md). Veritabanı oluşturabilmek ve veritabanı performansını isteğe göre yükseltip düşürebilmek, özellikle kullanım biçimlerinin nispeten tahmin edilebilir olduğu durumlarda birçok işletme ve uygulama için yeterlidir. Ancak tahmin edilemeyen kullanım biçimlerine sahipseniz bu durum maliyetlerin ve iş modelinizin yönetimini zorlaştırabilir. Bu senaryoda, belirli bir havuzdaki birden fazla veritabanı arasında paylaşılan Edtu sayısı ile bir esnek havuz kullanın.

![SQL Veritabanı'na Giriş: Katmana ve düzeye göre tek veritabanı DTU’ları](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

## <a name="what-are-elastic-database-transaction-units-edtus"></a>Esnek veritabanı işlem birimleri (Edtu'lar) nelerdir?
Bunun yerine her zaman gerekmeyen olup olmadığını bağımsız olarak, kullanılabilir bir SQL veritabanına adanmış bir kaynak (Dtu'lar) kümesi sağlayan daha veritabanlarına yerleştireceğiniz bir [esnek havuz](sql-database-elastic-pool.md) bir SQL veritabanı sunucusunda bu veritabanı arasında kaynak havuzunu paylaşır. Paylaşılan kaynaklar bir esnek havuzdaki esnek veritabanı işlem birimleri veya Edtu ölçülür. Esnek havuzlar, son derece farklı ve öngörülemeyen kullanım düzenlerine sahip birden çok veritabanına ilişkin performans hedeflerini yönetmek için basit ve uygun maliyetli bir çözüm sağlar. Bir esnek havuz tek veritabanı tüm kaynakları kullanır garanti edebilir havuzda olduğunu ve ayrıca en az miktarda kaynak her zaman bir veritabanını bir esnek havuz için kullanılabilir. 

![SQL Veritabanı'na Giriş: Katmana ve düzeye göre eDTU’lar](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Havuza belirli bir fiyat karşılığında, belirli bir sayıda eDTU verilir. Elastik havuz içerisinde tek veritabanlarına yapılandırılan sınırlar içinde otomatik olarak ölçeklendirme elastikliği tanınır. Bir veritabanı ağır yük altında talebi karşılamak için daha fazla eDTU kullanırken yükü hafif olan veritabanları daha az kullanabilir ve yük altında olmayan veritabanları eDTU’ya hiç ihtiyaç duymayabilir. Kaynakları tek bir veritabanı yerine havuzun tamamına sağlayarak yönetim görevlerini daha kolay hale getirebilir ve havuz için tahmin edilebilir bir bütçe oluşturabilirsiniz.

Mevcut bir havuza, veritabanı kapalı kalma süresi ve havuzdaki veritabanları üzerinde herhangi bir etkisi olmadan ek eDTU’lar eklenebilir. Benzer şekilde, ek eDTU’lara artık ihtiyaç yoksa bunlar mevcut bir havuzdan ne zaman isterseniz kaldırılabilir. Havuza veritabanı ekleyebilir veya var olanları kaldırabilir ya da eDTU’ları diğer veritabanları için ayırmak üzere ağır yük altındaki bir veritabanının kullanabileceği eDTU miktarını sınırlayabilirsiniz. Bir veritabanı kaynakları çok az kullanıyorsa onu havuzdan çıkarabilir ve gereken tahmini kaynaklarla tek veritabanı olarak yapılandırabilirsiniz.

## <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>İş yükümün ihtiyacı olan DTU sayısını nasıl belirleyebilirim?
Mevcut bir şirket içi veya SQL Server sanal makine iş yükünü Azure SQL Veritabanı’na geçirmek istiyorsanız, gereken yaklaşık DTU sayısını belirlemek için [DTU Hesaplayıcı](http://dtucalculator.azurewebsites.net/)’yı kullanabilirsiniz. Mevcut bir Azure SQL Veritabanı iş yükünüzü iyileştirecek İçgörüleri kazanmak amacıyla veritabanı kaynak tüketiminizi (DTU'lar) daha iyi anlamak için [SQL Veritabanı Sorgu Performansı İçgörüleri](sql-database-query-performance.md)’ni kullanabilirsiniz. [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV’sini kullanarak son bir saate ait kaynak tüketimi bilgilerini de alabilirsiniz. Alternatif olarak, [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) katalog görünümü sorgulanarak aynı veriler son 14 gün için de alınabilir. Ancak bu veriler, beş dakikalık ortalamalar şeklinde daha düşük bir aslına uygunluk düzeyindedir.

## <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Esnek bir kaynak havuzundan fayda sağlayıp sağlayamayacağımı nasıl öğrenebilirim?
Havuzlar, belirli kullanım düzenlerine sahip çok sayıda veritabanı bulunan durumlar için uygundur. Söz konusu kullanım düzeni, belirli bir veritabanı için ortalama düşük düzeyde kullanım ile nispeten nadir zamanlarda kullanımın ani olarak artması şeklindedir. SQL Veritabanı, mevcut bir SQL Veritabanı sunucusundaki veritabanlarının geçmiş kaynak kullanımını otomatik olarak değerlendirir ve Azure portalda uygun havuz yapılandırmasını önerir. Daha fazla bilgi için bkz. [ne zaman elastik bir havuz kullanılması gerekir?](sql-database-elastic-pool.md)

## <a name="what-happens-when-i-hit-my-maximum-dtus"></a>My maksimum Dtu isabet ne olur?
Performans düzeyleri, veritabanı iş yükünüzü seçilen hizmet katmanı/performans düzeyiniz için izin verilen en üst sınırlara kadar çalıştırmak üzere gereken kaynakları sağlamak için ayarlanıp yönetilir. İş yükünüz, CPU/Veri GÇ/Günlük GÇ sınırlarından birine ulaşıyorsa kaynakları, izin verilen maksimum düzeyde almaya devam ederseniz, ancak sorgularınızda artan gecikme süreleriyle karşılaşmanız olasıdır. Bu sınırlar herhangi bir hataya yol açmaz, yalnızca iş yükünü yavaşlatır. Yavaşlama sorguların zaman aşımına uğramasına sebep olacak kadar şiddetli hale gelirse hatayla karşılaşırsınız. İzin verilen en yüksek eş zamanlı kullanıcı oturumu/isteği (çalışan iş parçacıkları) sayısı sınırına ulaşırsanız açık hatalar görürsünüz. Bkz: [Azure SQL veritabanı kaynak sınırları]( sql-database-dtu-resource-limits.md#what-happens-when-database-and-elastic-pool-resource-limits-are-reached) sınır CPU dışındaki kaynaklar hakkında daha fazla bilgi için bellek, veri g/ç ve işlem günlük GÇ.

## <a name="next-steps"></a>Sonraki adımlar
* Bkz: [hizmet katmanı](sql-database-service-tiers.md) Dtu ve Edtu tek veritabanları ve esnek havuzlar için kullanılabilir bilgi, yanı sıra CPU dışındaki kaynaklarda sınırları, bellek, veri g/ç ve işlem günlük GÇ.
* DTU tüketiminizi anlamak için bkz. [SQL Veritabanı Sorgu Performansı İçgörüleri](sql-database-query-performance.md).
* DTU karışımını belirlemek için kullanılan OLTP kıyaslama iş yükünün arkasındaki metodolojiyi anlamak için bkz. [SQL Database benchmark overview](sql-database-benchmark-overview.md) (SQL Veritabanı kıyaslamaya genel bakış).
