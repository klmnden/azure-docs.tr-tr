---
title: Azure SQL veri ambarı için en iyi geliştirme | Microsoft Docs
description: Azure SQL Veri Ambarı için çözüm geliştirirken bilmeniz gerekenlerle ilgili öneriler ve en iyi yöntemler.
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 09/04/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 76297be79fca62b1f2f777f9cba4a0a8fe134768
ms.sourcegitcommit: 16cb78a0766f9b3efbaf12426519ddab2774b815
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65851630"
---
# <a name="development-best-practices-for-azure-sql-data-warehouse"></a>Azure SQL veri ambarı için en iyi uygulamaları geliştirme
Bu makalede, rehberlik ve en iyi uygulamalar, veri ambarı Çözümünüzü geliştirirken açıklanır. 

## <a name="reduce-cost-with-pause-and-scale"></a>Duraklatma ve ölçeklendirme ile maliyetleri azaltın
Duraklatma ve ölçeklendirme aracılığıyla maliyetleri azaltma hakkında daha fazla bilgi için bkz. [İşlem yönetme](sql-data-warehouse-manage-compute-overview.md). 


## <a name="maintain-statistics"></a>İstatistiklerin bakımını yapın
İstatistiklerinizi her gün veya her yükleme sonrasında güncelleştirdiğinizden emin olun.  İstatistiklerin sıfırdan oluşturulması veya mevcut olanların güncelleştirilmesi konusunda karar vermek için performans ve maliyet açısından değerlendirme yapmanız gerekir. Tüm istatistiklerin bakımını yapmanın çok uzun sürdüğünü düşünüyorsanız, istatistik tutulması veya sık güncelleştirilmesi gereken sütunlar konusunda daha ayrıntılı bir seçim yapabilirsiniz.  Örneğin, yeni değer eklenme ihtimali olan tarih sütunlarını her gün güncelleştirmeyi tercih edebilirsiniz. **En çok faydayı birleştirmelerin bulunduğu sütunlar, WHERE yan tümcesinde kullanılan sütunlar ve GROUP BY içinde bulunan sütunlar için istatistik tutarak elde edebilirsiniz.**

Ayrıca bkz. [Tablo istatistiklerini yönetme][Manage table statistics], [CREATE STATISTICS][CREATE STATISTICS], [UPDATE STATISTICS][UPDATE STATISTICS]

## <a name="hash-distribute-large-tables"></a>Büyük tabloları karma olarak dağıtın
Tablolar varsayılan olarak Hepsini Bir Kez Deneme yöntemiyle dağıtılmıştır.  Bu durum, kullanıcıların tabloların dağıtma şekli hakkında düşünmelerine gerek kalmadan tablo oluşturmaya başlamalarını sağlar.  Hepsini Bir Kez Deneme tabloları, belirli iş yükleri için yeterli performans sunabilir ancak birçok durumda dağıtım sütunu seçilmesi daha iyi sonuç verecektir.  Sütuna göre dağıtılmış bir tablonun Hepsini Bir Kez Deneme tablosundan daha iyi performans sunacağı bir örnek, iki büyük bilgi tablosunun birleştirilmesidir.  Örneğin, order_id ile dağıtılmış bir sipariş tablonuz ve yine order_id ile dağıtılmış bir işlem tablonuz varsa, sipariş tablonuzla işlem tablonuzu order_id üzerinden birleştirdiğinizde, ilgili sorgu bir geçiş sorgusu olur ve veri taşıma işlemleri atlanır.  Adım sayısı ne kadar az olursa sorgu o kadar hızlı işlenir.  Taşınan veri miktarı azaldıkça sorgunun hızı artar.  Bu yalnızca yüzeysel bir açıklamadır. Dağıtılmış bir tablo yüklenirken, yükleme işleminin yavaşlamaması için gelen verilerinizin dağıtım anahtarına göre sıralanmamış olduğundan emin olun.  Dağıtım sütunu seçmenin performansı nasıl artıracağı hakkında daha fazla bilgi edinmek ve CREATE TABLES deyiminizin WITH yan tümcesinde dağıtılmış tablo tanımlamayı öğrenmek için aşağıdaki bağlantıları inceleyin.

Ayrıca bkz. [Tabloya genel bakış][Table overview], [Tablo dağıtımı][Table distribution], [Tablo dağıtımına genel bakış][Selecting table distribution], [CREATE TABLE][CREATE TABLE], [CREATE TABLE AS SELECT][CREATE TABLE AS SELECT]

## <a name="do-not-over-partition"></a>Aşırı bölümleme yapmayın
Verileri bölümleme, bölüm değiştirme ile verilerinizin bakımını yapma veya bölüm eleme ile tarama iyileştirme konularında etkili olsa da, bölüm sayısının çok fazla olması sorgularınızı yavaşlatabilir.  Genelde SQL Server üzerinde iyi çalışan çok parçalı bölüm stratejisi SQL Veri Ambarı söz konusu olduğunda verimli olmayabilir.  Bölüm sayısının çok fazla olması, her bir bölümdeki satır sayısının 1 milyondan az olması halinde kümelenmiş columnstore dizinlerinin verimini de düşürebilir.  Ayrıca, SQL Veri Ambarı’nın verilerinizi arka planda 60 veritabanına böldüğünü unutmayın. 100 bölüme sahip bir tablo oluşturduğunuzda, toplam 6000 bölüm oluşturulmuş olur.  Her iş yükü farklı olduğundan, en iyi yöntem deneme yanılma ile iş yükünüze en uygun bölümleme şeklini belirlemektir.  SQL Server’da kullandığınızdan daha az parça kullanmayı deneyin.  Örneğin, günlük bölümler yerine haftalık veya aylık bölümler kullanın.

Ayrıca bkz. [Tablo bölümleme][Table partitioning]

## <a name="minimize-transaction-sizes"></a>İşlem boyutları en aza indirin
Bir işlemde çalışan INSERT, UPDATE ve DELETE deyimleri başarısız olduğunda gerçekleştirilen adımların geri alınması gerekir.  Uzun sürecek bir geri alma işlemi olasılığını en aza indirmek için işlem boyutlarını mümkün oldukça küçültün.  Bunu yapmak için INSERT, UPDATE ve DELETE deyimlerini parçalara ayırabilirsiniz.  Örneğin, 1 saat sürmesini beklediğiniz bir INSERT deyiminiz varsa, bu INSERT deyimini her biri 15 dakika sürecek 4 parçaya ayırın.  Geri alma riskini azaltmak için CTAS, TRUNCATE, DROP TABLE veya INSERT gibi özel En Az Günlüğe Kaydetme durumlarında boş tablo kullanın.  Geri alma işlemlerini ortadan kaldırmanın başka bir yöntemi de veri yönetimi için bölüm değiştirme gibi Yalnızca Meta Veri işlemlerini kullanmaktır.  Örneğin, bir tablodaki order_date değerleri Ekim 2001 içinde olan tüm satırları silmek amacıyla bir DELETE deyimi çalıştırmak yerine verilerinizi aylık bölümlere ayırıp bölümü başka bir tablonun boş bölümüyle değiştirebilirsiniz (ALTER TABLE örneklerini inceleyin).  Bölümlenmemiş tablolar için DELETE deyimini kullanmak yerine CTAS kullanarak tutmak istediğiniz verileri bir tabloya yazabilirsiniz.  Gereken süre aynıysa, günlüğe çok az işlem kaydı yapıldığı ve gerekli olması halinde hızlıca iptal edilebildiği için CTAS işlemi daha güvenli bir seçenektir.

Ayrıca bkz. [İşlemleri anlama][Understanding transactions], [İşlemleri iyileştirme][Optimizing transactions], [Tablo bölümleme][Table partitioning], [TRUNCATE TABLE][TRUNCATE TABLE], [ALTER TABLE][ALTER TABLE], [Create table as select (CTAS)][Create table as select (CTAS)]

## <a name="use-the-smallest-possible-column-size"></a>Mümkün olan en küçük sütun boyutunu kullanın
Sorgu performansını artırmak için, DDL tanımlaması yaparken verilerinizi destekleyecek en küçük veri türünü kullanın.  Bu durum özellikle CHAR ve VARCHAR sütunları için önemlidir.  Bir sütundaki en uzun değer 25 karakterse, sütununuzu VARCHAR(25) olarak tanımlayın.  Tüm karakter sütunları için varsayılan uzunluk değeri olarak yüksek bir değer kullanmaktan kaçının.  Ayrıca, NVARCHAR şart değilse sütunları VARCHAR olarak tanımlayın.

Ayrıca bkz. [Tabloya genel bakış][Table overview], [Tablo veri türleri][Table data types], [CREATE TABLE][CREATE TABLE]

## <a name="optimize-clustered-columnstore-tables"></a>Kümelenmiş columnstore tablolarını iyileştirin
Kümelenmiş columnstore dizinleri, verilerinizi SQL Veri Ambarı’nda depolamanın en verimli yöntemlerinden biridir.  SQL Veri Ambarı tabloları varsayılan ayarda Kümelenmiş ColumnStore olarak oluşturulur.  Columnstore tablolarında yapılan sorgularda en iyi performansı elde etmek için segment kalitesinin yüksek olması önemlidir.  Satırlar columnstore tablolarına bellek baskısı altında yazıldığında, segment kalitesi düşebilir.  Segment kalitesi, sıkıştırılmış Satır Grubu içindeki satır sayısıyla ölçülebilir.  Kümelenmiş columnstore tablolarının segment kalitesini tespit etme ve iyileştirme talimatları için [Tablo dizinleri][Table indexes] makalesindeki [Columnstore dizin kalitesinin düşük olmasının nedenleri][Causes of poor columnstore index quality] bölümüne bakın.  Yüksek kaliteli columnstore segmentleri önemli olduğundan, kullanıcılar veri yükleme Orta veya büyük kaynak sınıfındaki olan kimlikleri kullanmak üzere iyi bir fikirdir. Düşük kullanarak [veri ambarı birimleri](what-is-a-data-warehouse-unit-dwu-cdwu.md) yükleme kullanıcınıza daha büyük bir kaynak sınıfı atamak istediğiniz anlamına gelir.

Columnstore tabloları genelde tablo başına 1 milyon satır sınırı aşılmadan verileri sıkıştırılmış columnstore segmentlerine aktarmadığından ve her SQL Veri Ambarı tablosu 60 tabloya ayrıldığından, tablodaki satır sayısı 60 milyonu aşana kadar sorgular için columnstore tabloları kullanmaz.  60 milyondan az satıra sahip tablolarda columnstore dizini kullanmaya gerek olmayabilir.  Kullanmanın da bir zararı olmayacaktır.  Ayrıca, verilerinizi bölümlemeniz halinde her bir bölümün kümelenmiş columnstore dizini kullanabilmesi için en az 1 milyon satıra ihtiyaç duyacağını unutmayın.  100 bölüme sahip bir tablonun kümelenmiş columnstore kullanabilmesi için en az 6 milyar satıra sahip olması gerekir (60 dağıtım * 100 bölüm * 1 milyon satır).  Bu örnekte tablonuzda 6 milyar satır yoksa, bölüm sayısını azaltabilir veya yığın tablo kullanabilirsiniz.  Deneme yaparak columnstore tablosu yerine ikincil dizine sahip yığın tablo ile daha iyi performans elde edip etmeyeceğinizi görebilirsiniz.

Columnstore tablosunda çalıştırılan sorgular yalnızca ihtiyacınız olan sütunları seçmeniz halinde daha hızlı olacaktır.  

Ayrıca bkz. [Tablo dizinleri][Table indexes], [Columnstore dizinleri kılavuzu][Columnstore indexes guide], [Columnstore dizinlerini yeniden oluşturma][Rebuilding columnstore indexes]

## <a name="next-steps"></a>Sonraki adımlar
Aradığınızı bu makalede bulamadıysanız, bu sayfanın sol tarafındaki "Belge ara" seçeneğini kullanarak tüm Azure SQL Veri Ambarı belgelerinde arama yapın.  [Azure SQL veri ambarı Forumu] [ Azure SQL Data Warehouse MSDN Forum] diğer kullanıcılara ve SQL veri ambarı ürün Grubu'na soru sormanız için bir yerdir.  Sorularınızın diğer kullanıcılar veya ekibimiz tarafından yanıtlandığından emin olmak için bu forumu sürekli takip ediyoruz.  Sorularınızı Stack Overflow sitesinde sormak isterseniz, [Azure SQL Veri Ambarı Stack Overflow Forumu][Azure SQL Data Warehouse Stack Overflow Forum]’nu da kullanabilirsiniz.

Son olarak özellik isteğinde bulunmak için lütfen [Azure SQL Veri Ambarı Geri Bildirim][Azure SQL Data Warehouse Feedback] sayfasını kullanın.  İsteklerinizi eklemeniz veya diğer istekleri oylamanız, özellikleri önceliklendirme konusunda bize yardımcı olmaktadır.

<!--Image references-->

<!--Article references-->
[Create a support ticket]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Create table as select (CTAS)]: ./sql-data-warehouse-develop-ctas.md
[Table overview]: ./sql-data-warehouse-tables-overview.md
[Table data types]: ./sql-data-warehouse-tables-data-types.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Table indexes]: ./sql-data-warehouse-tables-index.md
[Causes of poor columnstore index quality]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Rebuilding columnstore indexes]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Table partitioning]: ./sql-data-warehouse-tables-partition.md
[Manage table statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary tables]: ./sql-data-warehouse-tables-temporary.md
[Guide for using PolyBase]: ./guidance-for-loading-data.md
[Load data]: ./design-elt-data-loading.md
[Move data with Azure Data Factory]: ../data-factory/transform-data-using-machine-learning.md
[Load data with Azure Data Factory]: ../data-factory/load-azure-sql-data-warehouse.md
[Load data with bcp]: /sql/tools/bcp-utility
[Load data with PolyBase]: ./load-data-wideworldimportersdw.md
[Monitor your workload using DMVs]: ./sql-data-warehouse-manage-monitor.md
[Pause compute resources]: ./sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Resume compute resources]: ./sql-data-warehouse-manage-compute-overview.md#resume-compute-bk
[Scale compute resources]: ./sql-data-warehouse-manage-compute-overview.md#scale-compute
[Understanding transactions]: ./sql-data-warehouse-develop-transactions.md
[Optimizing transactions]: ./sql-data-warehouse-develop-best-practices-transactions.md
[Troubleshooting]: ./sql-data-warehouse-troubleshoot.md
[LABEL]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->
[ALTER TABLE]: https://msdn.microsoft.com/library/ms190273.aspx
[CREATE EXTERNAL FILE FORMAT]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE STATISTICS]: https://msdn.microsoft.com/library/ms188038.aspx
[CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx
[CREATE TABLE AS SELECT]: https://msdn.microsoft.com/library/mt204041.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: https://msdn.microsoft.com/library/mt204017.aspx
[INSERT]: https://msdn.microsoft.com/library/ms174335.aspx
[OPTION]: https://msdn.microsoft.com/library/ms190322.aspx
[TRUNCATE TABLE]: https://msdn.microsoft.com/library/ms177570.aspx
[UPDATE STATISTICS]: https://msdn.microsoft.com/library/ms187348.aspx
[sys.dm_exec_sessions]: https://msdn.microsoft.com/library/ms176013.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_request_steps]: https://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: https://msdn.microsoft.com/library/mt203889.aspx
[sys.dm_pdw_dms_workers]: https://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_waits]: https://msdn.microsoft.com/library/mt203893.aspx
[Columnstore indexes guide]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->
[Selecting table distribution]: https://blogs.msdn.microsoft.com/sqlcat/20../../choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/
[Azure SQL Data Warehouse Feedback]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Azure SQL Data Warehouse MSDN Forum]: https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=AzureSQLDataWarehouse
[Azure SQL Data Warehouse Stack Overflow Forum]:  https://stackoverflow.com/questions/tagged/azure-sqldw
[Azure SQL Data Warehouse loading patterns and strategies]: https://blogs.msdn.microsoft.com/sqlcat/20../../azure-sql-data-warehouse-loading-patterns-and-strategies/
