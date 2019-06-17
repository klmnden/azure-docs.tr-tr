---
title: Zamana bağlı tablolarda Azure SQL veritabanı ile çalışmaya başlama | Microsoft Docs
description: Azure SQL veritabanı'nda zamana bağlı tabloları kullanmaya başlama hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab
manager: craigg
ms.date: 09/25/2018
ms.openlocfilehash: fbb2458e73330a09124c00cebe3eb7bcaba5408d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65951505"
---
# <a name="getting-started-with-temporal-tables-in-azure-sql-database"></a>Zamana bağlı tablolarda Azure SQL veritabanı ile çalışmaya başlama

Zamana bağlı tablolarda, Azure SQL veritabanı izleme ve özel kodlama için gerek kalmadan verilerinizdeki değişiklikleri tam geçmişini analiz etmenize olanak tanıyan yeni bir programlama özelliğidir. Zamana bağlı tablolarda yalnızca belirli bir dönem içinde depolanan bilgiler yorumlanabilir böylece zaman bağlamına yakından ilgili verileri geçerli olarak tutun. Zamana bağlı tablolarda, bu özellik etkili zaman tabanlı analiz ve veri evrimi alınırken ınsights'tan sağlar.

## <a name="temporal-scenario"></a>Zamana bağlı senaryosu

Bu makalede, bir uygulama senaryosunda zamana bağlı tabloları kullanmaya adımları gösterir. Kullanıcı etkinliği analytics ile genişletmek istediğiniz mevcut bir Web sitesini veya sıfırdan geliştirilen yeni bir Web sitesi üzerinde kullanıcı etkinliğini izlemek istediğinizi varsayalım. Bu basit örnekte, bir süre boyunca ziyaret edilen web sayfa sayısını yakalanır ve Azure SQL veritabanı'nda barındırılan Web sitesi veritabanı'nda izlenmesi gereken bir göstergesi olduğunu varsayıyoruz. Web sitesi yeniden tasarlayın ve ziyaretçiler için daha iyi bir deneyim sağlamak için girişler, kullanıcı etkinliğinin geçmişe dönük analiz amacı almaktır.

Bu senaryo için veritabanı modeli oldukça basittir: kullanıcı etkinliği ölçüm, tek tamsayılardır alana ile temsil edilir **PageVisited**ve kullanıcı profilindeki temel bilgilerle birlikte yakalanır. Ayrıca, zaman tabanlı analiz için her satırın belirli bir kullanıcı belirli bir süre içinde ziyaret edilen sayfa sayısını temsil ettiği bir dizi satır her kullanıcı için tutacak.

![Şema](./media/sql-database-temporal-tables/AzureTemporal1.png)

Neyse ki, bu etkinlik bilgileri korumak için uygulamanızda herhangi bir çaba put gerekmez. Zamana bağlı tablolar ile bu işlem - Web sitesi tasarımı ve daha fazla zaman veri analizi kendisini odaklanmak için sırasında tam esnekliğini otomatik hale getirilmiştir. Yapmanız gereken tek şey emin olmaktır **WebSiteInfo** tablo olarak yapılandırıldığında [zamana bağlı sistem sürümü tutulan](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0). Bu senaryoda, zamana bağlı tablolarda yararlanmak için uygulanacak adımlar aşağıda açıklanmıştır.

## <a name="step-1-configure-tables-as-temporal"></a>1\. adım: Zamana bağlı olarak tablolar yapılandırma
Yeni yazılım geliştirme başlangıç mı var olan bir uygulamayı yükseltme bağlı olarak, zamana bağlı tablolar oluşturmak veya zamana bağlı öznitelikleri ekleyerek var olanları düzenlemeniz. Genel olarak büyük/küçük harf, senaryonuz iki bu seçeneklerin bir karışımı olabilir. Bunlar gerçekleştirmek eylemini kullanarak [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS) [SQL Server veri Araçları](https://msdn.microsoft.com/library/mt204009.aspx) (SSDT) veya herhangi bir Transact-SQL geliştirme aracı.

> [!IMPORTANT]
> Microsoft Azure ve SQL Veritabanı güncelleştirmeleriyle aynı sürümde olmak için her zaman en güncel Management Studio sürümünü kullanmanız önerilir. [SQL Server Management Studio’yu güncelleyin](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

### <a name="create-new-table"></a>Yeni Tablo Oluştur
Bağlam menüsü öğesi "Yeni sistem sürümü tutulan tablo" SSMS nesne Gezgini'nde, zamana bağlı tablo şablon betikle sorgu Düzenleyicisi'ni açın ve ardından "Belirtin değerleri için şablon parametreleri" (Ctrl + Shift + M) şablonu doldurmak için kullanın:

![SSMSNewTable](./media/sql-database-temporal-tables/AzureTemporal2.png)

SSDT'de, veritabanı projesine yeni öğe ekleme "Zamana bağlı tablo (sistem sürümü tutulan)" şablonu seçin. Bu Tablo Tasarımcısı açılır ve kolayca Tablo düzeni belirtmenize olanak verir:

![SSDTNewTable](./media/sql-database-temporal-tables/AzureTemporal3.png)

Ayrıca zamana bağlı tablo Transact-SQL deyimleriyle doğrudan belirterek aşağıdaki örnekte gösterildiği gibi oluşturabilirsiniz. Her zamana bağlı tabloda zorunlu öğelerini süre tanımının ve geçmiş satır sürümleri depolayacak başka bir kullanıcı tablosu başvurusuyla system_versıonıng yan olduğuna dikkat edin:

```
CREATE TABLE WebsiteUserInfo 
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED 
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL 
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
```

Sistem sürümü tutulan zamana bağlı tablo oluşturduğunuzda eşlik eden bir geçmiş tablosu varsayılan yapılandırma ile otomatik olarak oluşturulur. Varsayılan geçmiş tablosu, dönem sütunlarında (end, Başlangıç) bir kümelenmiş B-Ağacı dizini sayfa sıkıştırma etkindir içerir. Bu yapılandırma, zamana bağlı tablolarda kullanılır, senaryoların büyük bölümü için en iyi özellikle [verileri denetleme](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0). 

Bu örnekte biz depolama seçimi geçmiş tablosu için kümelenmiş bir columnstore dizini, bu nedenle uzun bir veri geçmişinizi ve büyük veri kümeleri ile zamana dayalı eğilim analizi gerçekleştirmek için hedeflenir. Kümelenmiş columnstore çok iyi sıkıştırma ve analitik sorgular için performans sağlar. Zamana bağlı tablolarda dizinler geçerli ve zamana bağlı tablolarda tamamen bağımsız olarak yapılandırmak için esneklik sağlar. 

> [!NOTE]
> Columnstore dizinleri, Premium katman ve S3 standart katmanda ve sonraki sürümlerde kullanılabilir.
>

Aşağıdaki betik, varsayılan dizini geçmiş tablosu için kümelenmiş columnstore nasıl değiştirilebilir gösterir:

```
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
```

Geçmiş tablosu, bir alt düğüm görüntülenirken zamana bağlı tablolarda belirli simgesiyle daha kolay tanımlanmak için nesne Gezgini'nde gösterilir.

![AlterTable](./media/sql-database-temporal-tables/AzureTemporal4.png)

### <a name="alter-existing-table-to-temporal"></a>Zamana bağlı için var olan tablo değiştirme
Şimdi burada WebsiteUserInfo tablo zaten var, ancak değişiklik geçmişini tutmak üzere tasarlanmamıştır alternatif senaryoyu kapsar. Bu durumda, aşağıdaki örnekte gösterildiği gibi zamana bağlı, olacak varolan tablonun yalnızca genişletebilirsiniz:

```
ALTER TABLE WebsiteUserInfo 
ADD 
    ValidFrom datetime2 (0) GENERATED ALWAYS AS ROW START HIDDEN  
        constraint DF_ValidFrom DEFAULT DATEADD(SECOND, -1, SYSUTCDATETIME())
    , ValidTo datetime2 (0)  GENERATED ALWAYS AS ROW END HIDDEN   
        constraint DF_ValidTo DEFAULT '9999.12.31 23:59:59.99'
    , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo); 

ALTER TABLE WebsiteUserInfo  
SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
GO

CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
```

## <a name="step-2-run-your-workload-regularly"></a>2\. adım: İş yükünüz düzenli olarak çalıştır
Zamana bağlı tablolarda ana avantajı, sitenizi değişiklik izleme gerçekleştirmek için herhangi bir şekilde ayarlamak veya değiştirmek gerekmez ' dir. Verilerinizde değişiklikleri uygulamak her zaman oluşturulduktan sonra zamana bağlı tablolarda şeffaf bir şekilde önceki satır sürümleri kalıcı. 

Otomatik değişiklik izleme bu belirli bir senaryo için yararlanmak üzere yalnızca sütun güncelleştirelim **PagesVisited** bir kullanıcı kendi Web sitesinde sona her zaman:

```
UPDATE WebsiteUserInfo  SET [PagesVisited] = 5 
WHERE [UserID] = 1;
```

Update sorgusu gerçek işlemi meydana geldiğinde veya geçmiş verileri ileride çözümlenmek üzere korunacak nasıl kesin zaman bilmeniz gerekmez dikkat edin önemlidir. Her iki yönü otomatik olarak Azure SQL veritabanı tarafından işlenir. Aşağıdaki diyagram, geçmiş verileri üzerindeki her bir güncelleştirme oluşturulan nasıl gösterir.

![TemporalArchitecture](./media/sql-database-temporal-tables/AzureTemporal5.png)

## <a name="step-3-perform-historical-data-analysis"></a>3\. adım: Geçmiş veri çözümlemesi gerçekleştirme
Zamana bağlı sistem sürümü oluşturma etkin olduğunda, geçmiş verileri çözümleme, uzakta yalnızca bir sorgu sunulmuştur. Bu makalede, tüm ayrıntıları öğrenin, çeşitli seçenekleri ile sunulan keşfedin ortak analiz senaryoları - birkaç örnek sağlayacağız [FOR system_tıme](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) yan tümcesi.

Bir saatten önce itibarıyla ziyaret edilen web sayfalarının göre sıralanmış ilk 10 kullanıcıları görmek için bu sorguyu çalıştırın:

```
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
SELECT TOP 10 * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME AS OF @hourAgo
ORDER BY PagesVisited DESC
```

Bu sorgu, bir ay önce bir gün önce itibariyle site ziyaretler çözümlemek için kolayca değiştirebilir veya geçmişte herhangi bir noktada, istediğiniz.

Önceki güne ait temel istatistiki analiz gerçekleştirmek için aşağıdaki örneği kullanın:

```
DECLARE @twoDaysAgo datetime2 = DATEADD(DAY, -2, SYSUTCDATETIME());
DECLARE @aDayAgo datetime2 = DATEADD(DAY, -1, SYSUTCDATETIME());

SELECT UserID, SUM (PagesVisited) as TotalVisitedPages, AVG (PagesVisited) as AverageVisitedPages,
MAX (PagesVisited) AS MaxVisitedPages, MIN (PagesVisited) AS MinVisitedPages,
STDEV (PagesVisited) as StDevViistedPages
FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME BETWEEN @twoDaysAgo AND @aDayAgo
GROUP BY UserId
```

Belirli bir kullanıcı etkinliklerinin bir süre içinde yer alan yan tümcesi kullanımı için arama yapmak için:

```
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
DECLARE @twoHoursAgo datetime2 = DATEADD(HOUR, -2, SYSUTCDATETIME());
SELECT * FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME CONTAINED IN (@twoHoursAgo, @hourAgo)
WHERE [UserID] = 1;
```

Grafik görselleştirmesi zamana bağlı sorgular için özellikle kullanışlı aynıdır, eğilimleri ve kullanım biçimlerini sezgisel bir şekilde kolayca gösterebilirsiniz:

![TemporalGraph](./media/sql-database-temporal-tables/AzureTemporal6.png)

## <a name="evolving-table-schema"></a>Tablo şemasını gelişen
Genellikle, uygulama geliştirme yaparken zamana bağlı tablo şemasını değiştirmeniz gerekecektir. Bunun için çalıştırmanız yeterlidir normal ALTER TABLE deyimleri ve Azure SQL veritabanı uygun şekilde geçmiş tablosu için değişiklik yayılır. Aşağıdaki betiği izleme için ek öznitelik nasıl ekleyebileceğiniz gösterilmektedir:

```
/*Add new column for tracking source IP address*/
ALTER TABLE dbo.WebsiteUserInfo 
ADD  [IPAddress] varchar(128) NOT NULL CONSTRAINT DF_Address DEFAULT 'N/A';
```

Benzer şekilde, iş yükünüz etkin durumdayken sütun tanımı değiştirebilirsiniz:

```
/*Increase the length of name column*/
ALTER TABLE dbo.WebsiteUserInfo 
    ALTER COLUMN  UserName nvarchar(256) NOT NULL;
```

Son olarak, artık gerekmeyen bir sütunu kaldırmak için.

```
/*Drop unnecessary column */
ALTER TABLE dbo.WebsiteUserInfo 
    DROP COLUMN TemporaryColumn; 
```

Alternatif olarak, en günceli kullan [SSDT](https://msdn.microsoft.com/library/mt204009.aspx) (çevrimiçi mod) veritabanı veya veritabanı projesi (Çevrimdışı mod) bir parçası olarak bağlıyken zamana bağlı tablo şemasını değiştirmek için.

## <a name="controlling-retention-of-historical-data"></a>Geçmiş verilerin bekletme denetleme
Sistem sürümü tutulan zamana bağlı tablolarda geçmiş tablosunda normal tablolar birden çok veritabanı boyutunu artırabilirsiniz. Geniş ve büyüyen geçmiş tablosunda zamana bağlı sorgulama hem saf depolama maliyetlerinin yanı sıra belirli bir performans nedeniyle vergi bir sorun olabilir. Bu nedenle, geliştirme geçmişi tablosundaki verileri yönetmek için bir veri bekletme ilkesi planlama ve her zamana bağlı tablo yaşam döngüsü yönetimi, bir önemli parçasıdır. Azure SQL veritabanı ile zamana bağlı tabloda geçmiş verileri yönetmek için aşağıdaki yaklaşımlardan vardır:

* [Tablo bölümleme](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_2)
* [Özel bir temizleme betiği](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_3)

## <a name="next-steps"></a>Sonraki adımlar
Zamana bağlı tablolar hakkında ayrıntılı bilgi için kullanıma [MSDN belgelerine](https://msdn.microsoft.com/library/dn935015.aspx).
Dinlemek için Channel 9 ziyaret bir [gerçek müşteri zamana bağlı uygulama başarı hikayesi](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) ve izleme bir [zamana bağlı tanıtım canlı](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).

