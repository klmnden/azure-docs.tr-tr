---
title: Aralık 2018'den Azure SQL veri ambarı sürüm notları | Microsoft Docs
description: Azure SQL veri ambarı için sürüm notları.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 12/12/2018
ms.author: mausher
ms.reviewer: twounder
ms.openlocfilehash: b897b50edf4d5a7eeabacc6da1505e165f2bb21a
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54431750"
---
# <a name="whats-new-in-azure-sql-data-warehouse-december-2018"></a>Azure SQL veri ambarı'nda yenilikler nelerdir? Aralık 2018
Azure SQL veri ambarı, sürekli olarak iyileştirmeler alır. Bu makalede, aralık 2018'de sunulan değişiklikler ve yeni özellikleri açıklar.

## <a name="virtual-network-service-endpoints-generally-available"></a>Sanal ağ hizmet uç noktalarını kullanıma sunuldu
Bu sürüm, Azure SQL veri ambarı, tüm Azure bölgelerinde genel kullanıma sunulduğunu (VNet) sanal ağ hizmet uç noktaları içerir. Sanal ağ hizmet uç noktaları, bağlantısı verilen alt ağ ya da sanal ağınızdaki bir alt kümesini mantıksal sunucunuza yalıtmak etkinleştirin. Azure SQL veri ambarı için sanal ağınıza gelen trafik her zaman Azure omurga ağında kalır. Bu doğrudan rota, Internet trafiğini sanal gereçler veya şirket içi Süren herhangi belirli yollar üzerinden tercih edilen olarak olacaktır. Hiçbir ek faturalama, hizmet uç noktaları aracılığıyla sanal ağ erişimi için ücretlendirilir. Fiyatlandırma modeli için geçerli [Azure SQL veri ambarı](https://azure.microsoft.com/pricing/details/sql-data-warehouse/gen2/) uygulanır.

Bu sürümle birlikte, biz de PolyBase bağlantısı etkin [Azure Data Lake depolama Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction) (ADLS) aracılığıyla [Azure Blob dosya sistemi](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-abfs-driver) (ABFS) sürücü. Azure Data Lake depolama Gen2 analiz verilerini Azure Storage'a tam yaşam döngüsü için gerekli olan tüm kalitelerini getirir. İki var olan Azure depolama hizmetleri, Azure Blob Depolama ve Azure Data Lake depolama Gen1 özelliklerinin hiper yakınsama. Öğesinden özellikleri [Azure Data Lake depolama Gen1](https://docs.microsoft.com/azure/data-lake-store/index), dosya sistemi sematiğini gibi dosya düzeyinde güvenlik ve ölçek, düşük maliyetli, katmanlı depolama ve yüksek kullanılabilirlik/olağanüstü durum kurtarma özelliklerini birleştirilir [ Azure Blob Depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction). 

Polybase kullanarak veri Azure Vnet'e güvenli depolama alanından Azure SQL veri ambarı'na da aktarabilirsiniz. Benzer şekilde, Azure Vnet'e güvenli depolama Azure SQL veri ambarı verileri dışarı aktarma da Polybase desteklenir. 

Sanal ağ hizmet uç noktaları Azure SQL veri ambarı ile ilgili daha fazla bilgi için [blog gönderisi](https://azure.microsoft.com/blog/general-availability-of-vnet-service-endpoints-for-azure-sql-data-warehouse/) veya [belgeleri](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview?toc=/azure/sql-data-warehouse/toc.json).

## <a name="automatic-performance-monitoring-preview"></a>Otomatik performans izleme (Önizleme)
[Query Store](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store?view=sql-server-2017) Azure SQL veri ambarı Önizleme aşamasında kullanıma sunuldu. Query Store, sorgu performansı izleme tarafından sorguları, sorgu planlarına, çalışma zamanı istatistikleri ve sorgu geçmişi etkinlik ve veri ambarınızın performansını izlemenize yardımcı olması için sorun giderme amacıyla tasarlanmıştır. Query Store, iç depolar ve dinamik yönetim olanak tanıyan görünümlerini (Dmv'ler) kümesidir:

- Tanımlamak ve üst kaynak kullanan sorgular ayarlama
- Tanımlayın ve geçici iş yükleri artırın
- Sorgu performansı ve etkisi plana değişiklikleri istatistikleri, dizinleri veya sistem boyutu (DWU ayarını) değerlendirme
- Tam sorgu metni yürütülen tüm sorgular için bkz.

Query Store üç gerçek ambarlarını içerir:  
- Yürütme planı bilgileri kalıcı hale getirmeniz için bir plan deposu 
- Yürütme istatistik bilgilerini kalıcı hale getirmeniz için çalışma zamanı istatistikleri deposu
- Bekleme istatistikleri deposu bekleme istatistikleri bilgileri kalıcı hale getirme için. 

SQL veri ambarı, bu depoları otomatik olarak yönetir ve sınırsız sayıda ek ücret ödemeden son yedi güne storied sorguları sağlar. Query Store etkinleştirme, bir veritabanı T-SQL ALTER deyimi çalışıyor olarak kadar basittir:

```sql
ALTER DATABASE [Database Name] SET QUERY_STORE = ON;
```
Query Store, Azure SQL veri ambarı hakkında daha fazla bilgi için bkz [Query Store kullanarak performans izleme](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store?view=sql-server-2017)ve Query Store Dmv'leri gibi [sys.query_store_query](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-query-transact-sql?view=sql-server-2017). İşte [blog gönderisi](https://azure.microsoft.com/blog/automatic-performance-monitoring-in-azure-sql-data-warehouse-with-query-store/) sürüm ile tanışın.

## <a name="lower-compute-tiers-for-azure-sql-data-warehouse-gen2"></a>Azure SQL veri ambarı Gen2 için'daha düşük bilgi işlem katmanı
Azure SQL veri ambarı Gen2, artık daha düşük bilgi işlem katmanını destekler. Müşteriler Azure SQL veri ambarı'nın önde gelen performans, esneklik ve güvenlik özellikleri ile 100 cDWU başlangıç deneyimi ([veri ambarı birimleri](https://docs.microsoft.com/azure/sql-data-warehouse/what-is-a-data-warehouse-unit-dwu-cdwu)) ve 30. 000 cDWU dakikalar içinde ölçeklendirin. Aralık 2018'den başlayarak, müşterilerin Gen2 performansı yararlanabilir ve düşük ile esneklik katmanda işlem [bölgeleri](https://docs.microsoft.com/azure/sql-data-warehouse/gen2-lower-tier-regions), 2019 sırasında kullanılabilir bölgeleri geri kalanı ile.

Giriş noktası için tasarlanan yeni nesil veri ambarı bırakarak Microsoft hangi deneme ortamı için en iyi tahmin olmadan güvenli, yüksek performanslı veri ambarı tüm avantajlarını değerlendirmek istediğiniz değer temelli müşteriler kapılarını açar. Müşteriler, aşağı geçerli 500 cDWU giriş noktasından 100 cDWU düşük başlayabilir. SQL veri ambarı Gen2'ye duraklatma destekler ve işlemleri ve yalnızca işlem esneklik ötesinde uygulanmaya devam eder. Gen2 da destekler sütun deposu sınırsız depolama kapasitesi sorgu başına 2,5 kat daha fazla bellek birlikte en fazla 128 eş zamanlı sorguları ve [Uyarlamalı önbelleğe alma](https://azure.microsoft.com/blog/adaptive-caching-powers-azure-sql-data-warehouse-performance-gains/) özellikleri. Bu özellikler ortalama beş kat daha fazla performans Gen1 üzerinde aynı veri ambarı birimi aynı fiyata göre getirin. Coğrafi olarak yedekli yedeklemeler, yerleşik garantili veri korumasıyla için 2. nesil standarttır. Azure SQL veri ambarı Gen2, işiniz ölçeklendirmek hazırdır.

## <a name="columnstore-background-merge"></a>Columnstore arka plan birleştirme
Varsayılan olarak, Azure SQL veri ambarı'nı (Azure SQL DW) verileri sütunlu biçiminde micro-adlı bölümlerle depolar [rowgroups](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-memory-optimizations-for-columnstore-compression). Bazı durumlarda, too nedeniyle bellek kısıtlar derleme veya veri yükleme süresini, rowgroups bir milyon satır en iyi boyutundan küçük olan sıkıştırılmış olabilir. RowGroups siler nedeniyle de parçalanmış. Küçük veya bölünmüş rowgroups daha yüksek bellek tüketimi, yanı sıra verimsiz sorgu yürütme sonucu. Azure SQL DW bu sürümle birlikte, daha iyi bellek kullanmasına ve sorgu yürütme ' hızlandırmak için daha büyük satır grupları oluşturmak için küçük sıkıştırılmış satır grupları columnstore arka plan görevi birleştirir.

## <a name="next-steps"></a>Sonraki adımlar
SQL veri ambarı hakkında biraz bilmek, bilgi nasıl hızlı bir şekilde [SQL veri ambarı oluşturma][create a SQL Data Warehouse]. Azure'da yeniyseniz, bulabilirsiniz [Azure sözlüğünü] [ Azure glossary] yararlı yeni terimlerle öğrenin. Alternatif olarak, aşağıdaki diğer SQL Veri Ambarı Kaynakları’na göz atın.  

* [Müşteri başarı hikayeleri]
* [Bloglar]
* [Özellik istekleri]
* [Videolar]
* [Müşteri Danışma Ekibi blogları]
* [Stack Overflow forumu]
* [Twitter]


[Bloglar]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Müşteri Danışma Ekibi blogları]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Müşteri başarı hikayeleri]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Özellik istekleri]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Stack Overflow forumu]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videolar]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md
