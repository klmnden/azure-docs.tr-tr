---
title: "Azure SQL Veri Ambarı için en iyi uygulamalar | Microsoft Belgeleri"
description: "Azure SQL Veri Ambarı için çözüm geliştirirken bilmeniz gerekenlerle ilgili öneriler ve en iyi yöntemler. Bu veriler, başarılı olmanıza yardımcı olacaktır."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: 7b698cad-b152-4d33-97f5-5155dfa60f79
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: f04cba06cfe6a9094268ea041e85753cbd99f8fd


---
# <a name="best-practices-for-azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı için en iyi yöntemler
Bu makalede, Azure SQL Veri Ambarı çözümünüzden yüksek performans almanıza yardımcı olacak en iyi yöntemler bir arada sunulmaktadır.  Bu makalede, temel ve kolay anlaşılır kavramların yanı sıra ileri düzey kavramlarla ilgili özet bilgilere yer verilmektedir.  Bu makalenin amacı, veri ambarınızı oluşturmanız sırasında size temel noktalarda rehberlik yapmak ve odaklanmanız gereken önemli noktalara dikkat çekmektir.  Her bölümde bir kavram tanıtılmakta ve ardından ilgili kavramı ayrıntılı bir şekilde açıklayan ileri düzey makalelere bağlantı verilmektedir.

Azure SQL Veri Ambarı ile çalışmaya yeni başladıysanız, bu makale gözünüzü korkutmasın.  Konular genelde önem sırasına göre verilmiştir.  Başlangıç olarak ilk birkaç kavrama odaklanırsanız, daha kolay ilerleyebilirsiniz.  SQL Veri Ambarı hakkında daha fazla bilgi edinip daha çok kullanmaya başladıktan sonra bu makaleye dönerek birkaç kavrama daha göz atabilirsiniz.  Tüm kavramların oturması çok uzun sürmeyecektir.

## <a name="reduce-cost-with-pause-and-scale"></a>Duraklatma ve ölçeklendirme ile maliyetleri azaltın
SQL Veri Ambarı’nın önemli özelliklerinden biri, kullanmadığınız zaman duraklatarak işlem kaynakların maliyetlerini durdurma şansına sahip olmanızdır.  Bir diğer önemli özellik ise kaynakları ölçeklendirmektir.  Duraklatma ve Ölçeklendirme işlemlerini Azure portal üzerinden veya PowerShell komutları aracılığıyla yapabilirsiniz.  Veri ambarınız kullanılmadığında ilgili maliyetleri önemli ölçüde düşürebildiğinden bu özellikleri inceleyip keşfetmeniz önerilir.  Veri ambarınızın her zaman erişilebilir durumda olmasını istiyorsanız, duraklatmak yerine en küçük boyut olan DW100’e ölçeklendirmeyi tercih edebilirsiniz.

Ayrıca bkz. [İşlem kaynaklarını duraklatma][İşlem kaynaklarını duraklatma], [İşlem kaynaklarını sürdürme][İşlem kaynaklarını sürdürme], [İşlem kaynaklarını ölçeklendirme][İşlem kaynaklarını ölçeklendirme]

## <a name="drain-transactions-before-pausing-or-scaling"></a>Duraklatma veya ölçeklendirme öncesinde işlemleri boşaltın
SQL Veri Ambarınız için duraklatma veya ölçeklendirme isteğinde bulunduğunuzda, arka planda sorgularınız iptal edilir.  Basit bir SELECT sorgusunu hızlıca ve örnek duraklatma veya ölçeklendirme süresini neredeyse hiç etkilemeden iptal edebilirsiniz.  Ancak, verilerinizi veya verilerinizin yapısını değiştiren işlem sorguları o kadar hızlı durdurulamayabilir.  **Bir işlem sorgusunun tamamlanması veya yaptığı değişiklikleri geri alması gerekir.**  Bir işlem sorgusunun tamamladığı işi geri almak, sorgunun değişiklik yapmak için harcadığı süre kadar, hatta bazen daha fazla zaman alabilir.  Örneğin, bir saattir çalışan ve satır silen bir sorguyu iptal etmeniz halinde sistemin silinmiş olan satırları geri eklemesi bir saat sürebilir.  Duraklatma veya ölçeklendirme isteklerini işlemler devam ederken çalıştırmanız halinde, devam etmek için geri alma işleminin tamamlanmasını bekleyeceğinden ilgili duraklatma veya ölçeklendirme işleminin tamamlanması uzun sürebilir.

Ayrıca bkz. [İşlemler hakkında bilgi sahibi olma][İşlemleri hakkında bilgi sahibi olma], [İşlemleri iyileştirme][İşlemleri iyileştirme]

## <a name="maintain-statistics"></a>İstatistiklerin bakımını yapın
Sütunlarla ilgili istatistikleri otomatik olarak algılayıp oluşturan veya güncelleştiren SQL Server’dan farklı olarak, SQL Veri Ambarı’nda istatistiklerin bakımının kullanıcı tarafından yapılması gerekir.  Bu özelliği ilerleyen zamanlarda değiştirmeyi planlıyoruz ancak şimdilik SQL Veri Ambarı planlarının iyileştirilmiş olduğundan emin olmak için istatistiklerin bakımını sizin yapmanız gerekiyor.  İyileştirici tarafından oluşturulan planların verimi, istatistiklere bağlıdır.  **İstatistik tutmaya başlamanın kolay yollarından biri, her sütun için örnek istatistik oluşturmaktır.**  İstatistiklerin güncelleştirilmesi, verilerinizde yapılan değişiklikler kadar önemlidir.  Garanti yaklaşımlardan biri, istatistiklerinizi her gün veya her yükleme sonrasında güncelleştirmektir.  İstatistiklerin sıfırdan oluşturulması veya mevcut olanların güncelleştirilmesi konusunda karar vermek için performans ve maliyet açısından değerlendirme yapmanız gerekir. Tüm istatistiklerin bakımını yapmanın çok uzun sürdüğünü düşünüyorsanız, istatistik tutulması veya sık güncelleştirilmesi gereken sütunlar konusunda daha ayrıntılı bir seçim yapabilirsiniz.  Örneğin, yeni değer eklenme ihtimali olan tarih sütunlarını her gün güncelleştirmeyi tercih edebilirsiniz. **En çok faydayı birleştirmelerin bulunduğu sütunlar, WHERE yan tümcesinde kullanılan sütunlar ve GROUP BY içinde bulunan sütunlar için istatistik tutarak elde edebilirsiniz.**

Ayrıca bkz. [Tablo istatistiklerini yönetme][Tablo istatistiklerini yönetme], [CREATE STATISTICS][CREATE STATISTICS], [UPDATE STATISTICS][UPDATE STATISTICS]

## <a name="group-insert-statements-into-batches"></a>INSERT deyimlerini gruplayın
Küçük bir tabloya bir INSERT deyimiyle tek seferlik yükleme yapmak veya `INSERT INTO MyLookup VALUES (1, 'Type 1')` gibi bir deyimle bir aramanın düzenli aralıklarla yeniden yüklenmesi işinizi fazlasıyla görebilir.  Ancak gün içinde binlerce veya milyonlarca satır yüklemeniz gerekiyorsa, ayrı ayrı INSERT deyimleri gerekli performansı göstermeyebilir.  Bunun yerine işlemlerinizi bir dosyaya yazılacağı ve başka bir işlemin belirli aralıklarla bu dosyayı yükleyeceği şekilde geliştirin.

Ayrıca bkz. [INSERT][INSERT]

## <a name="use-polybase-to-load-and-export-data-quickly"></a>Verileri hızlıca yüklemek ve dışarı aktarmak için PolyBase kullanın
SQL Veri Ambarı, veri yüklemek ve dışarı aktarmak için Azure Data Factory, PolyBase ve BCP gibi birçok aracı destekler.  Performansın yüksek öneme sahip olmadığı küçük miktarlardaki veriler için bu araçlardan herhangi birini kullanabilirsiniz.  Ancak, büyük hacimli verileri yüklerken veya dışarı aktarırken ya da yüksek performansa ihtiyaç duyduğunuzda, PolyBase en iyi çözüm olacaktır.  PolyBase, SQL Veri Ambarı’nın MPP (Massively Parallel Processing - Büyük Ölçekli Paralel İşleme) mimarisini kullanarak büyük verileri diğer tüm araçlardan daha hızlı bir şekilde yükleyecek ve dışarı aktaracak şekilde tasarlanmıştır.  PolyBase yükleri CTAS veya INSERT INTO ile çalıştırılabilir.  **CTAS, işlem günlüğünü en aza indiren ve verilerinizi en hızlı yükleyen seçenektir.**  Azure Data Factory, PolyBase yüklerini de destekler.  PolyBase, Gzip de dahil olmak üzere birçok dosya biçimi için destek sunar.  **gzip metin dosyalarını kullanırken verimi en üst düzeye çıkarmak için dosyaları 60 veya daha fazla parçaya bölerek daha çok paralel işlem oluşturabilirsiniz.**  Toplam hızı artırmak için verilerinizi aynı anda yükleyin.

Ayrıca bkz. [Veri yükleme][Veri yükleme], [PolyBase’i kullanma kılavuzu][PolyBase’i kullanma kılavuzu], [Azure SQL Veri Ambarı yükleme düzenleri ve stratejileri][Azure SQL Veri Ambarı yükleme düzenleri ve stratejileri], [Azure Data Factory ile Veri Yükleme][Azure Data Factory ile Veri Yükleme], [Azure Data Factory ile veri taşıma][Azure Data Factory ile veri taşıma], [CREATE EXTERNAL FILE FORMAT][CREATE EXTERNAL FILE FORMAT], [Create table as select (CTAS)][Create table as select (CTAS)]

## <a name="load-then-query-external-tables"></a>Dış tabloları önce yükleyip sonra sorgu çalıştırın
Dış tablolar olarak da bilinen Polybase, veri yüklemenin en hızlı yolu olsa da sorgular için en iyi çözüm değildir. SQL Veri Ambarı Polybase tabloları şimdilik yalnızca Azure blob dosyalarını desteklemektedir. Bu dosyaları destekleyen herhangi bir işlem kaynağı yoktur.  Bu nedenle SQL Veri Ambarı bu işin yükünü boşaltamaz ve verileri okumak için dosyanın tamamını tempdb içine yüklemesi gerekir.  Sonuç olarak bu veriler için birden fazla sorgunuz varsa, verileri bir kez yükleyip sorguların yerel tabloyu kullanmalarını sağlamak daha iyi olacaktır.

Ayrıca bkz. [PolyBase'i kullanma kılavuzu][PolyBase'i kullanma kılavuzu]

## <a name="hash-distribute-large-tables"></a>Büyük tabloları karma olarak dağıtın
Tablolar varsayılan olarak Hepsini Bir Kez Deneme yöntemiyle dağıtılmıştır.  Bu durum, kullanıcıların tabloların dağıtma şekli hakkında düşünmelerine gerek kalmadan tablo oluşturmaya başlamalarını sağlar.  Hepsini Bir Kez Deneme tabloları, belirli iş yükleri için yeterli performans sunabilir ancak birçok durumda dağıtım sütunu seçilmesi daha iyi sonuç verecektir.  Sütuna göre dağıtılmış bir tablonun Hepsini Bir Kez Deneme tablosundan daha iyi performans sunacağı bir örnek, iki büyük bilgi tablosunun birleştirilmesidir.  Örneğin, order_id ile dağıtılmış bir sipariş tablonuz ve yine order_id ile dağıtılmış bir işlem tablonuz varsa, sipariş tablonuzla işlem tablonuzu order_id üzerinden birleştirdiğinizde, ilgili sorgu bir geçiş sorgusu olur ve veri taşıma işlemleri atlanır.  Adım sayısı ne kadar az olursa sorgu o kadar hızlı işlenir.  Taşınan veri miktarı azaldıkça sorgunun hızı artar.  Bu yalnızca yüzeysel bir açıklamadır. Dağıtılmış bir tablo yüklenirken, yükleme işleminin yavaşlamaması için gelen verilerinizin dağıtım anahtarına göre sıralanmamış olduğundan emin olun.  Dağıtım sütunu seçmenin performansı nasıl artıracağı hakkında daha fazla bilgi edinmek ve CREATE TABLES deyiminizin WITH yan tümcesinde dağıtılmış tablo tanımlamayı öğrenmek için aşağıdaki bağlantıları inceleyin.

Ayrıca bkz. [Tabloya genel bakış][Tabloya genel bakış], [Tablo dağıtımı][Tablo dağıtımı], [Tablo dağıtımı seçme][Tablo dağıtımı seçme], [CREATE TABLE][CREATE TABLE], [CREATE TABLE AS SELECT][CREATE TABLE AS SELECT]

## <a name="do-not-overpartition"></a>Aşırı bölümleme yapmayın
Verileri bölümleme, bölüm değiştirme ile verilerinizin bakımını yapma veya bölüm eleme ile tarama iyileştirme konularında etkili olsa da, bölüm sayısının çok fazla olması sorgularınızı yavaşlatabilir.  Genelde SQL Server üzerinde iyi çalışan çok parçalı bölüm stratejisi SQL Veri Ambarı söz konusu olduğunda verimli olmayabilir.  Bölüm sayısının çok fazla olması, her bir bölümdeki satır sayısının 1 milyondan az olması halinde kümelenmiş columnstore dizinlerinin verimini de düşürebilir.  Ayrıca, SQL Veri Ambarı’nın verilerinizi arka planda 60 veritabanına böldüğünü unutmayın. 100 bölüme sahip bir tablo oluşturduğunuzda, toplam 6000 bölüm oluşturulmuş olur.  Her iş yükü farklı olduğundan, en iyi yöntem deneme yanılma ile iş yükünüze en uygun bölümleme şeklini belirlemektir.  SQL Server’da kullandığınızdan daha az parça kullanmayı deneyin.  Örneğin, günlük bölümler yerine haftalık veya aylık bölümler kullanın.

Ayrıca bkz. [Tablo bölümleme][Tablo bölümleme]

## <a name="minimize-transaction-sizes"></a>İşlem boyutları en aza indirin
Bir işlemde çalışan INSERT, UPDATE ve DELETE deyimleri başarısız olduğunda gerçekleştirilen adımların geri alınması gerekir.  Uzun sürecek bir geri alma işlemi olasılığını en aza indirmek için işlem boyutlarını mümkün oldukça küçültün.  Bunu yapmak için INSERT, UPDATE ve DELETE deyimlerini parçalara ayırabilirsiniz.  Örneğin, 1 saat sürmesini beklediğiniz bir INSERT deyiminiz varsa, bu INSERT deyimini her biri 15 dakika sürecek 4 parçaya ayırın.  Geri alma riskini azaltmak için CTAS, TRUNCATE, DROP TABLE veya INSERT gibi özel En Az Günlüğe Kaydetme durumlarında boş tablo kullanın.  Geri alma işlemlerini ortadan kaldırmanın başka bir yöntemi de veri yönetimi için bölüm değiştirme gibi Yalnızca Meta Veri işlemlerini kullanmaktır.  Örneğin, bir tablodaki order_date değerleri Ekim 2001 içinde olan tüm satırları silmek amacıyla bir DELETE deyimi çalıştırmak yerine verilerinizi aylık bölümlere ayırıp bölümü başka bir tablonun boş bölümüyle değiştirebilirsiniz (ALTER TABLE örneklerini inceleyin).  Bölümlenmemiş tablolar için DELETE deyimini kullanmak yerine CTAS kullanarak tutmak istediğiniz verileri bir tabloya yazabilirsiniz.  Gereken süre aynıysa, günlüğe çok az işlem kaydı yapıldığı ve gerekli olması halinde hızlıca iptal edilebildiği için CTAS işlemi daha güvenli bir seçenektir.

Ayrıca bkz. [İşlemler hakkında bilgi sahibi olma][İşlemleri hakkında bilgi sahibi olma], [İşlemleri iyileştirme][İşlemleri iyileştirme], [Tablo bölümleme][Tablo bölümleme], [TRUNCATE TABLE][TRUNCATE TABLE], [ALTER TABLE][ALTER TABLE], [Create table as select (CTAS)][Create table as select (CTAS)]

## <a name="use-the-smallest-possible-column-size"></a>Mümkün olan en küçük sütun boyutunu kullanın
Sorgu performansını artırmak için, DDL tanımlaması yaparken verilerinizi destekleyecek en küçük veri türünü kullanın.  Bu durum özellikle CHAR ve VARCHAR sütunları için önemlidir.  Bir sütundaki en uzun değer 25 karakterse, sütununuzu VARCHAR(25) olarak tanımlayın.  Tüm karakter sütunları için varsayılan uzunluk değeri olarak yüksek bir değer kullanmaktan kaçının.  Ayrıca, NVARCHAR şart değilse sütunları VARCHAR olarak tanımlayın.

Ayrıca bkz. [Tabloya genel bakış][Tabloya genel bakış], [Tablo veri türleri][Tablo veri türleri], [CREATE TABLE][CREATE TABLE]

## <a name="use-temporary-heap-tables-for-transient-data"></a>Geçiş verileri için geçici yığın tabloları kullanın
SQL Veri Ambarı’na geçici veri yüklemesi yapıyorsanız, yığın tabloları kullanmanın işlem hızını artırdığını fark edeceksiniz.  Verileri yalnızca daha fazla dönüştürme gerçekleştirmeden önce hazırlamak için yüklüyorsanız, tabloyu yığın tablosuna yüklemek verileri kümelenmiş bir columnstore tablosuna yüklemekten çok daha hızlı olacaktır.  Ayrıca verileri geçici bir tabloya yüklemek, tabloyu kalıcı bir depolama alanına yüklemekten çok daha hızlı gerçekleştirilecektir.  Geçici tablolar "#" ile başlar ve yalnızca kendilerini oluşturan oturum tarafından erişilebilir. Bu nedenle sınırlı sayıda senaryoda çalışabilirler.   Yığın tabloları, CREATE TABLE deyiminin WITH yan tümcesinde tanımlanır.  Geçici tablo kullanıyorsanız, onun için de istatistik oluşturmayı unutmayın.

Ayrıca bkz. [Geçici tablolar][Geçici tablolar], [CREATE TABLE][CREATE TABLE], [CREATE TABLE AS SELECT][CREATE TABLE AS SELECT]

## <a name="optimize-clustered-columnstore-tables"></a>Kümelenmiş columnstore tablolarını iyileştirin
Kümelenmiş columnstore dizinleri, verilerinizi Azure SQL Veri Ambarı’nda depolamanın en verimli yöntemlerinden biridir.  SQL Veri Ambarı tabloları varsayılan ayarda Kümelenmiş ColumnStore olarak oluşturulur.  Columnstore tablolarında yapılan sorgularda en iyi performansı elde etmek için segment kalitesinin yüksek olması önemlidir.  Satırlar columnstore tablolarına bellek baskısı altında yazıldığında, segment kalitesi düşebilir.  Segment kalitesi, sıkıştırılmış Satır Grubu içindeki satır sayısıyla ölçülebilir.  Kümelenmiş columnstore tablolarının segment kalitesini tespit etme ve iyileştirmeye yönelik adım adım yönergeler için [Tablo dizinleri][Tablo dizinleri] makalesindeki [Columnstore dizin kalitesinin düşük olmasının nedenleri][Columnstore dizin kalitesinin düşük olmasının nedenleri] bölümüne bakın.  Yüksek kaliteli columnstore segmentleri önemli olduğundan, veri yüklemek için orta veya büyük kaynak sınıfındaki kullanıcı kimliklerinden faydalanabilirsiniz.  Ne kadar az DWU kullanırsanız, yüklenen kullanıcınıza o kadar büyük kaynak sınıfı atamanız gerekir. 

Columnstore tabloları genelde tablo başına 1 milyon satır sınırı aşılmadan verileri sıkıştırılmış columnstore segmentlerine aktarmadığından ve her SQL Veri Ambarı tablosu 60 tabloya ayrıldığından, tablodaki satır sayısı 60 milyonu aşana kadar sorgular için columnstore tabloları kullanmaz.  60 milyondan az satıra sahip tablolarda columnstore dizini kullanmaya gerek olmayabilir.  Kullanmanın da bir zararı olmayacaktır.  Ayrıca, verilerinizi bölümlemeniz halinde her bir bölümün kümelenmiş columnstore dizini kullanabilmesi için en az 1 milyon satıra ihtiyaç duyacağını unutmayın.  100 bölüme sahip bir tablonun kümelenmiş columnstore kullanabilmesi için en az 6 milyar satıra sahip olması gerekir (60 dağıtım * 100 bölüm * 1 milyon satır).  Bu örnekte tablonuzda 6 milyar satır yoksa, bölüm sayısını azaltabilir veya yığın tablo kullanabilirsiniz.  Deneme yaparak columnstore tablosu yerine ikincil dizine sahip yığın tablo ile daha iyi performans elde edip etmeyeceğinizi görebilirsiniz.  Columnstore tabloları ikincil dizinleri henüz desteklememektedir.

Columnstore tablosunda çalıştırılan sorgular yalnızca ihtiyacınız olan sütunları seçmeniz halinde daha hızlı olacaktır.  

Ayrıca bkz. [Tablo dizinleri][Tablo dizinleri], [Columnstore dizinleri kılavuzu][Columnstore dizinleri kılavuzu], [Columnstore dizinlerini yeniden oluşturma][Columnstore dizinlerini yeniden oluşturma]

## <a name="use-larger-resource-class-to-improve-query-performance"></a>Sorgu performansını artırmak için daha büyük kaynak sınıfı kullanın
SQL Veri Ambarı, kaynak gruplarını sorgulara bellek ayırma yöntemi olarak kullanır.  Sistem ilk kurulduğunda tüm kullanıcılar, dağıtım başına 100 MB bellek veren küçük kaynak sınıfına atanır.  Her zaman 60 dağıtım olduğundan ve her dağıtıma en az 100 MB verildiğinden, sistem genelinde ayrılan bellek toplam 6000 MB (yaklaşık 6 GB) olur.  Büyük birleştirmeler veya kümelenmiş columnstore tablolarına yapılan yüklemeler gibi belirli sorgulara daha fazla bellek atanır.  Yalnızca tarama gibi bazı sorgulara ek atama yapılmaz.  Diğer taraftan, daha büyük kaynak sınıflarının kullanılması eşzamanlı çalışmayı etkiler. Bu nedenle tüm kullanıcılarınızı büyük bir kaynak sınıfına taşımadan önce bu noktayı dikkate almanız gerekir.

Ayrıca bkz. [Eşzamanlılık ve iş yükü yönetimi][Eşzamanlılık ve iş yükü yönetimi]

## <a name="use-smaller-resource-class-to-increase-concurrency"></a>Eşzamanlılığı artırmak için daha küçük kaynak sınıfı kullanın
Kullanıcı sorgularında uzun süreli gecikmeler olduğunu fark ederseniz, kullanıcılarınız geniş kaynak sınıflarında çalışıyor ve çok fazla eşzamanlılık yuvası kullanarak diğer sorguların kuyrukta beklemesine neden oluyor olabilir.  Kullanıcı sorgularının kuyrukta olup olmadığını görmek için `SELECT * FROM sys.dm_pdw_waits` çalıştırıp dönen satırlara bakın.

Ayrıca bkz. [Eşzamanlılık ve iş yükü yönetimi][Eşzamanlılık ve iş yükü yönetimi], [sys.dm_pdw_waits][sys.dm_pdw_waits]

## <a name="use-dmvs-to-monitor-and-optimize-your-queries"></a>Sorgularınızı izlemek ve iyileştirmek için DMV’leri kullanın
SQL Veri Ambarı’nda sorgu yürütmeyi izlemek için kullanabileceğiniz birçok DMV vardır.  Aşağıdaki izleme makalesinde çalıştırılan bir sorgunun ayrıntılarını incelemeyle ilgili adım adım talimatlar yer almaktadır.  Bu DMV’lerdeki sorguları hızlıca bulmak için sorgularınızla LABEL seçeneğini kullanabilirsiniz.

Ayrıca bkz. [DMV’leri kullanarak iş yükünüzü izleme][DMV’leri kullanarak iş yükünüzü izleme], [LABEL][LABEL], [OPTION][OPTION], [sys.dm_exec_sessions][sys.dm_exec_sessions], [sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests], [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps], [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests], [sys.dm_pdw_dms_workers], [DBCC PDW_SHOWEXECUTIONPLAN][DBCC PDW_SHOWEXECUTIONPLAN], [sys.dm_pdw_waits][sys.dm_pdw_waits]

## <a name="other-resources"></a>Diğer kaynaklar
Genel sorunlar ve çözümleri için [Sorun giderme][Sorun giderme] makalemizi de inceleyin.

Aradığınızı bu makalede bulamadıysanız, bu sayfanın sol tarafındaki "Belge ara" seçeneğini kullanarak tüm Azure SQL Veri Ambarı belgelerinde arama yapın.  [Azure SQL Veri Ambarı MSDN Forumu][Azure SQL Veri Ambarı MSDN Forumu], diğer kullanıcılara ve SQL Veri Ambarı Ürün Grubu’na soru sormanız için tasarlanmış olan bir sayfadır.  Sorularınızın diğer kullanıcılar veya ekibimiz tarafından yanıtlandığından emin olmak için bu forumu sürekli takip ediyoruz.  Sorularınızı Stack Overflow sitesinde sormak isterseniz, [Azure SQL Veri Ambarı Stack Overflow Forumu][Azure SQL Veri Ambarı Stack Overflow Forumu]’nu da kullanabilirsiniz.

Son olarak özellik isteğinde bulunmak için lütfen [Azure SQL Veri Ambarı Geri Bildirim][Azure SQL Veri Ambarı Geri Bildirim] sayfasını kullanın.  İsteklerinizi eklemeniz veya diğer istekleri oylamanız, özellikleri önceliklendirme konusunda bize yardımcı olmaktadır.

<!--Image references-->

<!--Article references-->
[Destek bileti oluşturma]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Eşzamanlılık ve iş yükü yönetimi]: ./sql-data-warehouse-develop-concurrency.md
[Create table as select (CTAS)]: ./sql-data-warehouse-develop-ctas.md
[Tabloya genel bakış]: ./sql-data-warehouse-tables-overview.md
[Tablo veri türleri]: ./sql-data-warehouse-tables-data-types.md
[Tablo dağıtımı]: ./sql-data-warehouse-tables-distribute.md
[Tablo dizinleri]: ./sql-data-warehouse-tables-index.md
[Columnstore dizin kalitesinin düşük olmasının nedenleri]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Columnstore dizinlerini yeniden oluşturma]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Tablo bölümleme]: ./sql-data-warehouse-tables-partition.md
[Tablo istatistiklerini yönetme]: ./sql-data-warehouse-tables-statistics.md
[Geçici tablolar]: ./sql-data-warehouse-tables-temporary.md
[PolyBase kullanma kılavuzu]: ./sql-data-warehouse-load-polybase-guide.md
[Veri yükleme]: ./sql-data-warehouse-overview-load.md
[Azure Data Factory ile veri taşıma]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[Azure Data Factory ile veri yükleme]: ./sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Bcp ile veri yükleme]: ./sql-data-warehouse-load-with-bcp.md
[PolyBase ile veri yükleme]: ./sql-data-warehouse-get-started-load-with-polybase.md
[DMV’leri kullanarak iş yükünüzü izleme]: ./sql-data-warehouse-manage-monitor.md
[İşlem kaynaklarını duraklatma]: ./sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[İşlem kaynaklarını sürdürme]: ./sql-data-warehouse-manage-compute-overview.md#resume-compute-bk
[İşlem kaynaklarını ölçeklendirme]: ./sql-data-warehouse-manage-compute-overview.md#scale-performance-bk
[İşlemleri anlama]: ./sql-data-warehouse-develop-transactions.md
[İşlemler iyileştirme]: ./sql-data-warehouse-develop-best-practices-transactions.md
[Sorun giderme]: ./sql-data-warehouse-troubleshoot.md
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
[Columnstore dizinleri kılavuzu]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->
[Tablo dağıtımını seçme]: https://blogs.msdn.microsoft.com/sqlcat/2015/08/11/choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/
[Azure SQL Veri Ambarı Geri Bildirimi]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Azure SQL Veri Ambarı MSDN Forumu]: https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=AzureSQLDataWarehouse
[Azure SQL Veri Ambarı Stack Overflow Forumu]:  http://stackoverflow.com/questions/tagged/azure-sqldw
[Azure SQL Veri Ambarı yükleme modelleri ve stratejileri]: https://blogs.msdn.microsoft.com/sqlcat/2016/02/06/azure-sql-data-warehouse-loading-patterns-and-strategies



<!--HONumber=Nov16_HO2-->


