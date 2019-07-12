---
title: Azure SQL Veri Ambarı için en iyi uygulamalar | Microsoft Belgeleri
description: Azure SQL Veri Ambarı için çözüm geliştirirken bilmeniz gerekenlerle ilgili öneriler ve en iyi yöntemler.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: design
ms.date: 11/26/2018
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 9c9e293a6e9c8126f2b82f68d591aee56ec32aec
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67672287"
---
# <a name="best-practices-for-azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı için en iyi yöntemler
Bu makalede, Azure SQL veri ambarı en iyi performans elde etmenize yardımcı olması için en iyi bir koleksiyonudur.  Bu makalede, temel ve kolay anlaşılır kavramların yanı sıra ileri düzey kavramlarla ilgili özet bilgilere yer verilmektedir.  Bu makalenin amacı, veri ambarınızı oluşturmanız sırasında size temel noktalarda rehberlik yapmak ve odaklanmanız gereken önemli noktalara dikkat çekmektir.  Her bölümde bir kavram tanıtılmakta ve ardından ilgili kavramı ayrıntılı bir şekilde açıklayan ileri düzey makalelere bağlantı verilmektedir.

Azure SQL Veri Ambarı ile çalışmaya yeni başladıysanız, bu makale gözünüzü korkutmasın.  Konular genelde önem sırasına göre verilmiştir.  Başlangıç olarak ilk birkaç kavrama odaklanırsanız, daha kolay ilerleyebilirsiniz.  Daha fazla bilgi edinip SQL veri ambarı'nı kullanarak aldıkça, geri dönün ve birkaç kavrama daha göz atabilirsiniz.  Tüm kavramların oturması çok uzun sürmeyecektir.

Yüklemeyle ilgili rehber için bkz. [Veri yükleme rehberi](guidance-for-loading-data.md).

## <a name="reduce-cost-with-pause-and-scale"></a>Duraklatma ve ölçeklendirme ile maliyetleri azaltın
Duraklatma ve ölçeklendirme aracılığıyla maliyetleri azaltma hakkında daha fazla bilgi için bkz. [İşlem yönetme](sql-data-warehouse-manage-compute-overview.md). 


## <a name="maintain-statistics"></a>İstatistiklerin bakımını yapın
Sütunlarla ilgili istatistikleri otomatik olarak algılayıp oluşturan veya güncelleştiren SQL Server’dan farklı olarak, SQL Veri Ambarı’nda istatistiklerin bakımının kullanıcı tarafından yapılması gerekir.  Bu özelliği ilerleyen zamanlarda değiştirmeyi planlıyoruz ancak şimdilik SQL Veri Ambarı planlarının iyileştirilmiş olduğundan emin olmak için istatistiklerin bakımını sizin yapmanız gerekiyor.  İyileştirici tarafından oluşturulan planların verimi, istatistiklere bağlıdır.  **İstatistik tutmaya başlamanın kolay yollarından biri, her sütun için örnek istatistik oluşturmaktır.**  İstatistiklerin güncelleştirilmesi, verilerinizde yapılan değişiklikler kadar önemlidir.  Garanti yaklaşımlardan biri, istatistiklerinizi her gün veya her yükleme sonrasında güncelleştirmektir.  İstatistiklerin sıfırdan oluşturulması veya mevcut olanların güncelleştirilmesi konusunda karar vermek için performans ve maliyet açısından değerlendirme yapmanız gerekir. Tüm istatistiklerin bakımını yapmanın çok uzun sürdüğünü düşünüyorsanız, istatistik tutulması veya sık güncelleştirilmesi gereken sütunlar konusunda daha ayrıntılı bir seçim yapabilirsiniz.  Örneğin, yeni değer eklenme ihtimali olan tarih sütunlarını her gün güncelleştirmeyi tercih edebilirsiniz. **En çok faydayı birleştirmelerin bulunduğu sütunlar, WHERE yan tümcesinde kullanılan sütunlar ve GROUP BY içinde bulunan sütunlar için istatistik tutarak elde edebilirsiniz.**

Ayrıca bkz: [tablo istatistiklerini yönetme][Manage table statistics], [CREATE STATISTICS][CREATE STATISTICS], [UPDATE STATISTICS][UPDATE STATISTICS]

## <a name="group-insert-statements-into-batches"></a>INSERT deyimlerini gruplayın
Küçük bir tabloya bir INSERT deyimiyle tek seferlik yükleme yapmak veya `INSERT INTO MyLookup VALUES (1, 'Type 1')` gibi bir deyimle bir aramanın düzenli aralıklarla yeniden yüklenmesi işinizi fazlasıyla görebilir.  Ancak gün içinde binlerce veya milyonlarca satır yüklemeniz gerekiyorsa, ayrı ayrı INSERT deyimleri gerekli performansı göstermeyebilir.  Bunun yerine işlemlerinizi bir dosyaya yazılacağı ve başka bir işlemin belirli aralıklarla bu dosyayı yükleyeceği şekilde geliştirin.

Ayrıca bkz. [INSERT][INSERT]

## <a name="use-polybase-to-load-and-export-data-quickly"></a>Verileri hızlıca yüklemek ve dışarı aktarmak için PolyBase kullanın
SQL Veri Ambarı, veri yüklemek ve dışarı aktarmak için Azure Data Factory, PolyBase ve BCP gibi birçok aracı destekler.  Performansın yüksek öneme sahip olmadığı küçük miktarlardaki veriler için bu araçlardan herhangi birini kullanabilirsiniz.  Ancak, büyük hacimli verileri yüklerken veya dışarı aktarırken ya da yüksek performansa ihtiyaç duyduğunuzda, PolyBase en iyi çözüm olacaktır.  PolyBase, SQL Veri Ambarı’nın MPP (Massively Parallel Processing - Büyük Ölçekli Paralel İşleme) mimarisini kullanarak büyük verileri diğer tüm araçlardan daha hızlı bir şekilde yükleyecek ve dışarı aktaracak şekilde tasarlanmıştır.  PolyBase yükleri CTAS veya INSERT INTO ile çalıştırılabilir.  **CTAS, işlem günlüğünü en aza indiren ve verilerinizi en hızlı yükleyen seçenektir.**  Azure Data Factory PolyBase yüklerini destekler ve CTAS olarak benzer bir performans elde edebilirsiniz.  PolyBase, Gzip de dahil olmak üzere birçok dosya biçimi için destek sunar.  **gzip metin dosyalarını kullanırken verimi en üst düzeye çıkarmak için dosyaları 60 veya daha fazla parçaya bölerek daha çok paralel işlem oluşturabilirsiniz.**  Toplam hızı artırmak için verilerinizi aynı anda yükleyin.

Ayrıca bkz: [veri yükleme][Load data], [Guide for using PolyBase][Guide for using PolyBase], [Azure SQL veri ambarı yükleme modelleri ve stratejileri][Azure SQL veri ambarı yükleme modelleri ve stratejileri], [ Azure Data Factory ile veri yükleme][Load Data with Azure Data Factory], [Move data with Azure Data Factory][Move data with Azure Data Factory], [CREATE EXTERNAL FILE FORMAT][CREATE EXTERNAL FILE FORMAT], [Create table as select (CTAS)][Create table as select (CTAS)]

## <a name="load-then-query-external-tables"></a>Dış tabloları önce yükleyip sonra sorgu çalıştırın
Dış tablolar olarak da bilinen Polybase, veri yüklemenin en hızlı yolu olsa da sorgular için en iyi çözüm değildir. SQL Veri Ambarı Polybase tabloları şimdilik yalnızca Azure blob dosyalarını ve Azure Data Lake depolama alanını desteklemektedir. Bu dosyaları destekleyen herhangi bir işlem kaynağı yoktur.  Bu nedenle SQL Veri Ambarı bu işin yükünü boşaltamaz ve verileri okumak için dosyanın tamamını tempdb içine yüklemesi gerekir.  Sonuç olarak bu veriler için birden fazla sorgunuz varsa, verileri bir kez yükleyip sorguların yerel tabloyu kullanmalarını sağlamak daha iyi olacaktır.

Ayrıca bkz. [PolyBase kullanma kılavuzu][Guide for using PolyBase]

## <a name="hash-distribute-large-tables"></a>Büyük tabloları karma olarak dağıtın
Tablolar varsayılan olarak Hepsini Bir Kez Deneme yöntemiyle dağıtılmıştır.  Bu durum, kullanıcıların tabloların dağıtma şekli hakkında düşünmelerine gerek kalmadan tablo oluşturmaya başlamalarını sağlar.  Hepsini Bir Kez Deneme tabloları, belirli iş yükleri için yeterli performans sunabilir ancak birçok durumda dağıtım sütunu seçilmesi daha iyi sonuç verecektir.  Sütuna göre dağıtılmış bir tablonun Hepsini Bir Kez Deneme tablosundan daha iyi performans sunacağı bir örnek, iki büyük bilgi tablosunun birleştirilmesidir.  Örneğin, order_id ile dağıtılmış bir sipariş tablonuz ve yine order_id ile dağıtılmış bir işlem tablonuz varsa, sipariş tablonuzla işlem tablonuzu order_id üzerinden birleştirdiğinizde, ilgili sorgu bir geçiş sorgusu olur ve veri taşıma işlemleri atlanır.  Adım sayısı ne kadar az olursa sorgu o kadar hızlı işlenir.  Taşınan veri miktarı azaldıkça sorgunun hızı artar.  Bu yalnızca yüzeysel bir açıklamadır. Dağıtılmış bir tablo yüklenirken, yükleme işleminin yavaşlamaması için gelen verilerinizin dağıtım anahtarına göre sıralanmamış olduğundan emin olun.  Bkz. aşağıdaki bağlantıları artıracağı hakkında daha fazla ayrıntı nasıl dağıtım sütunu seçmenin yanı sıra, CREATE TABLE deyiminin WITH yan tümcesinde dağıtılmış tablo tanımlamak için performansı artırabilir.

Ayrıca bkz: [tabloya genel bakış][Table overview], [Table distribution][Table distribution], [Tablo dağıtımı seçme][Tablo dağıtımı seçme], [CREATE TABLE][CREATE TABLE], [CREATE TABLE AS SELECT][CREATE TABLE AS SELECT]

## <a name="do-not-over-partition"></a>Aşırı bölümleme yapmayın
Verileri bölümleme, bölüm değiştirme ile verilerinizin bakımını yapma veya bölüm eleme ile tarama iyileştirme konularında etkili olsa da, bölüm sayısının çok fazla olması sorgularınızı yavaşlatabilir.  Genelde SQL Server üzerinde iyi çalışan çok parçalı bölüm stratejisi SQL Veri Ambarı söz konusu olduğunda verimli olmayabilir.  Bölüm sayısının çok fazla olması, her bir bölümdeki satır sayısının 1 milyondan az olması halinde kümelenmiş columnstore dizinlerinin verimini de düşürebilir.  Ayrıca, SQL Veri Ambarı’nın verilerinizi arka planda 60 veritabanına böldüğünü unutmayın. 100 bölüme sahip bir tablo oluşturduğunuzda, toplam 6000 bölüm oluşturulmuş olur.  Her iş yükü farklı olduğundan, en iyi yöntem deneme yanılma ile iş yükünüze en uygun bölümleme şeklini belirlemektir.  SQL Server’da kullandığınızdan daha az parça kullanmayı deneyin.  Örneğin, günlük bölümler yerine haftalık veya aylık bölümler kullanın.

Ayrıca bkz. [Tablo bölümleme][Table partitioning]

## <a name="minimize-transaction-sizes"></a>İşlem boyutları en aza indirin
Bir işlemde çalışan INSERT, UPDATE ve DELETE deyimleri başarısız olduğunda gerçekleştirilen adımların geri alınması gerekir.  Uzun sürecek bir geri alma işlemi olasılığını en aza indirmek için işlem boyutlarını mümkün oldukça küçültün.  Bunu yapmak için INSERT, UPDATE ve DELETE deyimlerini parçalara ayırabilirsiniz.  Örneğin, 1 saat sürmesini beklediğiniz bir INSERT deyiminiz varsa, bu INSERT deyimini her biri 15 dakika sürecek 4 parçaya ayırın.  Geri alma riskini azaltmak için CTAS, TRUNCATE, DROP TABLE veya INSERT gibi özel En Az Günlüğe Kaydetme durumlarında boş tablo kullanın.  Geri alma işlemlerini ortadan kaldırmanın başka bir yöntemi de veri yönetimi için bölüm değiştirme gibi Yalnızca Meta Veri işlemlerini kullanmaktır.  Örneğin, bir tablodaki order_date değerleri Ekim 2001 içinde olan tüm satırları silmek amacıyla bir DELETE deyimi çalıştırmak yerine verilerinizi aylık bölümlere ayırıp bölümü başka bir tablonun boş bölümüyle değiştirebilirsiniz (ALTER TABLE örneklerini inceleyin).  Bölümlenmemiş tablolar için DELETE deyimini kullanmak yerine CTAS kullanarak tutmak istediğiniz verileri bir tabloya yazabilirsiniz.  Gereken süre aynıysa, günlüğe çok az işlem kaydı yapıldığı ve gerekli olması halinde hızlıca iptal edilebildiği için CTAS işlemi daha güvenli bir seçenektir.

Ayrıca bkz: [işlemleri anlama][Understanding transactions], [Optimizing transactions][Optimizing transactions], [tablo bölümleme][tablo bölümleme], [TRUNCATE TABLE][TRUNCATE TABLE] , [ALTER TABLE][ALTER TABLE], [Create table as select (CTAS)][Create table as select (CTAS)]

## <a name="use-the-smallest-possible-column-size"></a>Mümkün olan en küçük sütun boyutunu kullanın
Sorgu performansını artırmak için, DDL tanımlaması yaparken verilerinizi destekleyecek en küçük veri türünü kullanın.  Bu durum özellikle CHAR ve VARCHAR sütunları için önemlidir.  Bir sütundaki en uzun değer 25 karakterse, sütununuzu VARCHAR(25) olarak tanımlayın.  Tüm karakter sütunları için varsayılan uzunluk değeri olarak yüksek bir değer kullanmaktan kaçının.  Ayrıca, NVARCHAR şart değilse sütunları VARCHAR olarak tanımlayın.

Ayrıca bkz: [tabloya genel bakış][Table overview], [Table data types][Table data types], [CREATE TABLE][CREATE TABLE]

## <a name="use-temporary-heap-tables-for-transient-data"></a>Geçiş verileri için geçici yığın tabloları kullanın
SQL Veri Ambarı’na geçici veri yüklemesi yapıyorsanız, yığın tabloları kullanmanın işlem hızını artırdığını fark edeceksiniz.  Verileri yalnızca daha fazla dönüştürme gerçekleştirmeden önce hazırlamak için yüklüyorsanız, tabloyu yığın tablosuna yüklemek verileri kümelenmiş bir columnstore tablosuna yüklemekten çok daha hızlı olacaktır.  Ayrıca verileri geçici bir tabloya yüklemek, tabloyu kalıcı bir depolama alanına yüklemekten çok daha hızlı gerçekleştirilecektir.  Geçici tablolar "#" ile başlar ve yalnızca kendilerini oluşturan oturum tarafından erişilebilir. Bu nedenle sınırlı sayıda senaryoda çalışabilirler.   Yığın tabloları, CREATE TABLE deyiminin WITH yan tümcesinde tanımlanır.  Geçici tablo kullanıyorsanız, onun için de istatistik oluşturmayı unutmayın.

Ayrıca bkz: [geçici tablolar][Temporary tables], [CREATE TABLE][CREATE TABLE], [CREATE TABLE AS SELECT][CREATE TABLE AS SELECT]

## <a name="optimize-clustered-columnstore-tables"></a>Kümelenmiş columnstore tablolarını iyileştirin
Kümelenmiş columnstore dizinleri, verilerinizi SQL Veri Ambarı’nda depolamanın en verimli yöntemlerinden biridir.  SQL Veri Ambarı tabloları varsayılan ayarda Kümelenmiş ColumnStore olarak oluşturulur.  Columnstore tablolarında yapılan sorgularda en iyi performansı elde etmek için segment kalitesinin yüksek olması önemlidir.  Satırlar columnstore tablolarına bellek baskısı altında yazıldığında, segment kalitesi düşebilir.  Segment kalitesi, sıkıştırılmış Satır Grubu içindeki satır sayısıyla ölçülebilir.  Bkz: [neden kötü columnstore dizin kalitesinin][Causes of poor columnstore index quality] in the [Table indexes][Table indexes] makale tespit etme ve kümelenmiş columnstore tablolarının segment kalitesini iyileştirmeye yönelik adım adım yönergeler için.  Yüksek kaliteli columnstore segmentleri önemli olduğundan, kullanıcılar veri yükleme Orta veya büyük kaynak sınıfındaki olan kimlikleri kullanmak üzere iyi bir fikirdir. Düşük kullanarak [veri ambarı birimleri](what-is-a-data-warehouse-unit-dwu-cdwu.md) yükleme kullanıcınıza daha büyük bir kaynak sınıfı atamak istediğiniz anlamına gelir.

Columnstore tabloları genelde tablo başına 1 milyon satır sınırı aşılmadan verileri sıkıştırılmış columnstore segmentlerine aktarmadığından ve her SQL Veri Ambarı tablosu 60 tabloya ayrıldığından, tablodaki satır sayısı 60 milyonu aşana kadar sorgular için columnstore tabloları kullanmaz.  60 milyondan az satıra sahip tablolarda columnstore dizini kullanmaya gerek olmayabilir.  Kullanmanın da bir zararı olmayacaktır.  Ayrıca, verilerinizi bölümlemeniz halinde her bir bölümün kümelenmiş columnstore dizini kullanabilmesi için en az 1 milyon satıra ihtiyaç duyacağını unutmayın.  100 bölüme sahip bir tablonun kümelenmiş columnstore kullanabilmesi için en az 6 milyar satıra sahip olması gerekir (60 dağıtım * 100 bölüm * 1 milyon satır).  Bu örnekte tablonuzda 6 milyar satır yoksa, bölüm sayısını azaltabilir veya yığın tablo kullanabilirsiniz.  Deneme yaparak columnstore tablosu yerine ikincil dizine sahip yığın tablo ile daha iyi performans elde edip etmeyeceğinizi görebilirsiniz.

Columnstore tablosunda çalıştırılan sorgular yalnızca ihtiyacınız olan sütunları seçmeniz halinde daha hızlı olacaktır.  

Ayrıca bkz: [tablo dizinleri][Table indexes], [Columnstore indexes guide][Columnstore indexes guide], [columnstore dizinlerini yeniden oluşturma][columnstore dizinlerini yeniden oluşturma]

## <a name="use-larger-resource-class-to-improve-query-performance"></a>Sorgu performansını artırmak için daha büyük kaynak sınıfı kullanın
SQL Veri Ambarı, kaynak gruplarını sorgulara bellek ayırma yöntemi olarak kullanır.  Sistem ilk kurulduğunda tüm kullanıcılar, dağıtım başına 100 MB bellek veren küçük kaynak sınıfına atanır.  Her zaman 60 dağıtım olduğundan ve her dağıtıma en az 100 MB verildiğinden, sistem genelinde ayrılan bellek toplam 6000 MB (yaklaşık 6 GB) olur.  Büyük birleştirmeler veya kümelenmiş columnstore tablolarına yapılan yüklemeler gibi belirli sorgulara daha fazla bellek atanır.  Yalnızca tarama gibi bazı sorgulara ek atama yapılmaz.  Diğer taraftan, daha büyük kaynak sınıflarının kullanılması eşzamanlı çalışmayı etkiler. Bu nedenle tüm kullanıcılarınızı büyük bir kaynak sınıfına taşımadan önce bu noktayı dikkate almanız gerekir.

Ayrıca bkz. [İş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md)

## <a name="use-smaller-resource-class-to-increase-concurrency"></a>Eşzamanlılığı artırmak için daha küçük kaynak sınıfı kullanın
Kullanıcı sorgularında uzun süreli gecikmeler olduğunu fark ederseniz, kullanıcılarınız geniş kaynak sınıflarında çalışıyor ve çok fazla eşzamanlılık yuvası kullanarak diğer sorguların kuyrukta beklemesine neden oluyor olabilir.  Kullanıcı sorgularının kuyrukta olup olmadığını görmek için `SELECT * FROM sys.dm_pdw_waits` çalıştırıp dönen satırlara bakın.

Ayrıca bkz: [iş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md), [sys.dm_pdw_waits][sys.dm_pdw_waits]

## <a name="use-dmvs-to-monitor-and-optimize-your-queries"></a>Sorgularınızı izlemek ve iyileştirmek için DMV’leri kullanın
SQL Veri Ambarı’nda sorgu yürütmeyi izlemek için kullanabileceğiniz birçok DMV vardır.  Aşağıdaki izleme makalesinde çalıştırılan bir sorgunun ayrıntılarını incelemeyle ilgili adım adım talimatlar yer almaktadır.  Bu DMV’lerdeki sorguları hızlıca bulmak için sorgularınızla LABEL seçeneğini kullanabilirsiniz.

Ayrıca bkz: [Dmv'leri kullanarak iş yükünüzü izleme][Monitor your workload using DMVs], [LABEL][LABEL], [seçeneği][seçeneği], [sys.dm_exec_sessions][sys.dm_exec_sessions], [sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests], [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps], [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests], [sys.dm_pdw_dms_workers], [DBCC pdw_showexecutıonplan][DBCC PDW_SHOWEXECUTIONPLAN], [sys.dm_pdw_waits][sys.dm_pdw_waits]

## <a name="other-resources"></a>Diğer kaynaklar
Genel sorunlar ve çözümleri için [Sorun giderme][Troubleshooting] makalemizi de inceleyin.

Aradığınızı bu makalede bulamadıysanız, bu sayfanın sol tarafındaki "Belge ara" seçeneğini kullanarak tüm Azure SQL Veri Ambarı belgelerinde arama yapın.  [Azure SQL veri ambarı Forumu][Azure SQL Data Warehouse MSDN Forum] is a place for you to ask questions to other users and to the SQL Data Warehouse Product Group.  We actively monitor this forum to ensure that your questions are answered either by another user or one of us.  If you prefer to ask your questions on Stack Overflow, we also have an [Azure SQL Data Warehouse Stack Overflow Forum][Azure SQL Data Warehouse Stack Overflow Forum].

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
