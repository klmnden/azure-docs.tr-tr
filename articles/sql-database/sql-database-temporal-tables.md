---
title: Zamana bağlı tablolarda Azure SQL veritabanı ile çalışmaya başlama | Microsoft Docs
description: Azure SQL veritabanı'nda zamana bağlı tablolarda kullanmaya başlamanıza öğrenin.
services: sql-database
author: bonova
ms.date: 03/21/2018
manager: craigg
ms.service: sql-database
ms.custom: develop databases
ms.topic: article
ms.author: bonova
ms.openlocfilehash: 0299d52396549baf8ea7e5eb7145585c7b5900a6
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="getting-started-with-temporal-tables-in-azure-sql-database"></a>Zamana bağlı tablolarda Azure SQL veritabanı ile çalışmaya başlama
Zamana bağlı tablolarda, izlemek ve özel kodlama için gerek kalmadan, verilerde yapılan değişiklikler tam geçmişini çözümlemek izin veren Azure SQL veritabanı'nın yeni bir programlama özelliğidir. Zamana bağlı tablolarda yalnızca belirli bir dönem içinde depolanan bulguları yorumlanabilen böylece zaman bağlamına yakından ilgili veriler geçerli olarak tutun. Zamana bağlı tablolarda, bu özellik etkili zamana dayalı çözümleme ve veri evrimi alma ilişkin bilgiler sağlar.

## <a name="temporal-scenario"></a>Zamana bağlı senaryosu
Bu makalede, zamana bağlı tablolarda bir uygulama senaryosunda kullanmak için adımları gösterilmektedir. Sıfırdan geliştirilen yeni bir Web sitesi ya da kullanıcı etkinliği analytics ile genişletmek istediğiniz mevcut bir Web kullanıcı etkinliğini izlemek istediğinizi varsayalım. Bu basit örnekte, bir süre sırasında ziyaret ettiğiniz web sayfalarının sayısının yakalanan ve Azure SQL veritabanı üzerinde barındırılan Web sitesi veritabanında izlenen gerekir bir gösterge olduğunu varsayalım. Kullanıcı etkinliği geçmiş çözümleme amacı, Web sitesi yeniden tasarlama ve ziyaretçilere daha iyi deneyimi sağlamak için girişleri almaktır.

Bu senaryo için veritabanı modeli oldukça basittir - kullanıcı etkinliği ölçüm bir tek tamsayı alanıyla temsil **PageVisited**ve kullanıcı profilini temel bilgiler birlikte yakalanır. Ayrıca, temel saat analizi için her satır, belirli bir kullanıcının belirli bir dönem içinde ziyaret sayfaların sayısını temsil ettiği satırları her kullanıcı için bir dizi engelleneceği.

![Şema](./media/sql-database-temporal-tables/AzureTemporal1.png)

Neyse ki, bu etkinlik bilgileri korumak için uygulamanızda herhangi çaba put gerekmez. Zamana bağlı tablolar ile bu işlem - Web sitesi tasarımı ve daha fazla zaman veri analizi kendisini odaklanmaya sırasında tam esneklik sağlayan otomatik hale getirilmiştir. Emin olmak için yapmanız gereken tek şey. **WebSiteInfo** tablo olarak yapılandırılmış [zamana bağlı sistem sürümlü](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0). Bu senaryoda, zamana bağlı tablolarda faydalanmak için uygulanacak adımlar aşağıda açıklanmıştır.

## <a name="step-1-configure-tables-as-temporal"></a>1. adım: tabloları zamana bağlı olarak yapılandırma
Yeni yazılım geliştirme başlangıç mı var olan uygulama yükseltme bağlı olarak, zamana bağlı tablolarda oluşturun veya var olanları zamana bağlı öznitelikleri ekleyerek değiştirin. Genel durumda senaryonuz iki bu seçeneklerin bir bileşimi olabilir. Bunlar gerçekleştirmek eylemini kullanarak [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS) [SQL Server veri Araçları](https://msdn.microsoft.com/library/mt204009.aspx) (SSDT) veya başka bir Transact-SQL geliştirme aracıdır.

> [!IMPORTANT]
> Microsoft Azure ve SQL Veritabanı güncelleştirmeleriyle aynı sürümde olmak için her zaman en güncel Management Studio sürümünü kullanmanız önerilir. [SQL Server Management Studio’yu güncelleyin](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

### <a name="create-new-table"></a>Yeni Tablo Oluştur
Bağlam menüsü öğesini "Yeni sistem sürümlü tablo" SSMS nesne Gezgini'nde bir zamana bağlı tablo şablonu komut dosyasıyla sorgu Düzenleyicisi'ni açın ve şablon doldurmak için "Değerler için şablon parametrelerini belirt" (Ctrl + Shift + M) kullanmak için kullanın:

![SSMSNewTable](./media/sql-database-temporal-tables/AzureTemporal2.png)

Yeni öğe için veritabanı projesi eklerken SSDT içinde "(sistem sürümü tutulan) zamana bağlı tablo" Şablon seçtiniz. Bu Tablo Tasarımcısı'nı açın ve kolayca Tablo düzeni belirtmenize olanak sağlar:

![SSDTNewTable](./media/sql-database-temporal-tables/AzureTemporal3.png)

Ayrıca bir oluşturma zamana bağlı tablo Transact-SQL deyimlerini doğrudan belirterek aşağıdaki örnekte gösterildiği gibi erişebilirsiniz. Her zamana bağlı tabloda zorunlu öğelerini süre tanımının ve geçmiş satır sürümlerini depolamak başka bir kullanıcı tablosu başvuru system_versıonıng yan tümcesini olduğuna dikkat edin:

````
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
````

Sistem sürümü tutulan zamana bağlı tablo oluşturduğunuzda eşlik eden geçmiş tablosu varsayılan yapılandırması otomatik olarak oluşturulur. Varsayılan geçmiş tablosu sayfası sıkıştırmayı etkin dönem sütunlarında (end, start) bir kümelenmiş B-Ağacı dizini içerir. Bu yapılandırma en iyi zamana bağlı tablolarda kullanılan senaryoları çoğunluğu için özellikle de [verileri denetleme](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0). 

Bu özel durumda biz depolama için geçmiş tablosu kümelenmiş columnstore dizini seçimdir şekilde zamana dayalı eğilim analizi üzerinden uzun bir veri geçmişini ve büyük veri kümeleriyle gerçekleştirmek için hedefleyin. Kümelenmiş columnstore çok iyi sıkıştırma ve analitik sorguları için performans sağlar. Zamana bağlı tablolarda geçerli ve zamana bağlı tablolarda tamamen bağımsız olarak Dizinleri'ni esnekliği sağlar. 

> [!NOTE]
> Columnstore dizinleri, Premium katmanındaki ve standart katmanındaki S3 ve üstü kullanılabilir.
>

Aşağıdaki komut dosyasında varsayılan dizini geçmiş tablosu kümelenmiş columnstore nasıl değiştirilebilir gösterir:

````
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

Geçmiş tablosu bir alt düğüm olarak görüntülendiği sırada zamana bağlı tablolarda daha kolay tanımlama için belirli simgesiyle nesne Gezgini'nde temsil edilir.

![AlterTable](./media/sql-database-temporal-tables/AzureTemporal4.png)

### <a name="alter-existing-table-to-temporal"></a>Zamana bağlı için var olan tablo değiştirme
Şimdi, WebsiteUserInfo tablo zaten var, ancak değişikliklerin geçmişini tutmak için tasarlanmamıştır alternatif senaryo kapsar. Bu durumda, aşağıdaki örnekte gösterildiği gibi zamana bağlı, olmasını varolan tablonun yalnızca genişletebilirsiniz:

````
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
````

## <a name="step-2-run-your-workload-regularly"></a>2. adım: İş yükünüzün düzenli olarak çalıştırma
Zamana bağlı tablolarda ana avantajı, Web sitenizi değişiklik izlemeyi gerçekleştirmek için herhangi bir şekilde ayarlamak veya değiştirmek gerekmez ' dir. Değişiklikler, verilerinizde gerçekleştirilecek her zaman oluşturduktan sonra zamana bağlı tablolarda saydam önceki satır sürümleri kalıcı olmasını sağlar. 

Otomatik değişiklik izleme bu belirli bir senaryo için kullanılabilmesi için şirketinizdeki yalnızca sütun güncelleştirme **PagesVisited** kullanıcı Web sitesinde her/his oturumu sona erdiğinde her zaman:

````
UPDATE WebsiteUserInfo  SET [PagesVisited] = 5 
WHERE [UserID] = 1;
````

Güncelleştirme sorgusu tam zaman gerçek işlemi oluştuğunda veya geçmiş verileri gelecekteki analize korunacak nasıl bilmesi gerekmez fark önemlidir. Her iki yönlerini otomatik olarak Azure SQL veritabanı tarafından işlenir. Aşağıdaki diyagram, geçmiş verileri üzerindeki her bir güncelleştirme oluşturulan nasıl gösterir.

![TemporalArchitecture](./media/sql-database-temporal-tables/AzureTemporal5.png)

## <a name="step-3-perform-historical-data-analysis"></a>3. adım: geçmiş veri çözümlemesi gerçekleştirme
Zamana bağlı sistem sürümü oluşturma etkin olduğunda, geçmiş verileri çözümleme, uzakta yalnızca bir sorgu sunulmuştur. Bu makalede, söz tüm ayrıntıları öğrenmek için çeşitli seçenekler ile sunulan keşfedin ortak analiz - senaryosu birkaç örnek sağlarız [FOR system_tıme](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) yan tümcesi.

Bir saatten önce itibariyle ziyaret edilen web sayfalarının sayısı bazında ilk 10 kullanıcılar görmek için bu sorguyu çalıştırın:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
SELECT TOP 10 * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME AS OF @hourAgo
ORDER BY PagesVisited DESC
````

Geçmişteki herhangi bir noktada, istediğiniz veya bir ay önce bir gün önce itibariyle site ziyaret çözümlemek için bu sorguyu kolayca değiştirebilirsiniz.

Önceki gün için temel istatistiksel analizler yapmak için aşağıdaki örneği kullanın:

````
DECLARE @twoDaysAgo datetime2 = DATEADD(DAY, -2, SYSUTCDATETIME());
DECLARE @aDayAgo datetime2 = DATEADD(DAY, -1, SYSUTCDATETIME());

SELECT UserID, SUM (PagesVisited) as TotalVisitedPages, AVG (PagesVisited) as AverageVisitedPages,
MAX (PagesVisited) AS MaxVisitedPages, MIN (PagesVisited) AS MinVisitedPages,
STDEV (PagesVisited) as StDevViistedPages
FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME BETWEEN @twoDaysAgo AND @aDayAgo
GROUP BY UserId
````

Bir süre sonunda süresi, belirli bir kullanıcı etkinlikleri için aranacak bulunan yan tümcesi kullanın:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
DECLARE @twoHoursAgo datetime2 = DATEADD(HOUR, -2, SYSUTCDATETIME());
SELECT * FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME CONTAINED IN (@twoHoursAgo, @hourAgo)
WHERE [UserID] = 1;
````

Eğilimleri ve kullanım düzenlerini sezgisel bir yolla kolayca gösterebilirsiniz gibi grafik görselleştirme zamana bağlı sorguları için özellikle kullanışlıdır:

![TemporalGraph](./media/sql-database-temporal-tables/AzureTemporal6.png)

## <a name="evolving-table-schema"></a>Tablo şemasını gelişen
Genellikle, uygulama geliştirme yaparken zamana bağlı tablo şemasını değiştirmeniz gerekir. Bunun için çalıştırmanız yeterlidir normal ALTER TABLE deyimleri ve Azure SQL veritabanı uygun şekilde geçmiş tablosu değişiklikleri yayılır. Aşağıdaki komut dosyası, izleme için ek öznitelik nasıl ekleyebileceğiniz gösterilmektedir:

````
/*Add new column for tracking source IP address*/
ALTER TABLE dbo.WebsiteUserInfo 
ADD  [IPAddress] varchar(128) NOT NULL CONSTRAINT DF_Address DEFAULT 'N/A';
````

Benzer şekilde, iş yükünüzü etkinken sütun tanımı değiştirebilirsiniz:

````
/*Increase the length of name column*/
ALTER TABLE dbo.WebsiteUserInfo 
    ALTER COLUMN  UserName nvarchar(256) NOT NULL;
````

Son olarak, artık ihtiyacınız olmayan bir sütun kaldırabilirsiniz.

````
/*Drop unnecessary column */
ALTER TABLE dbo.WebsiteUserInfo 
    DROP COLUMN TemporaryColumn; 
````

Alternatif olarak, en son kullanın [SSDT](https://msdn.microsoft.com/library/mt204009.aspx) (çevrimiçi mod) veritabanı veya veritabanı projesi (Çevrimdışı mod) bir parçası olarak bağlıyken zamana bağlı tablo şemasını değiştirmek için.

## <a name="controlling-retention-of-historical-data"></a>Geçmiş verilerin bekletilmesini denetleme
Sistem sürümü tutulan zamana bağlı tablolarda ile geçmiş tablosu normal tablolardaki birden çok veritabanı boyutunu artırabilir. Büyük ve sürekli büyüyen geçmiş tablosu, hem saf depolama maliyetleri yanı sıra bir performans etkileyici nedeniyle zamana bağlı sorgulama vergi bir sorun olabilir. Bu nedenle, geçmiş tablosundaki verileri yönetmek için bir veri bekletme ilkesi geliştirme planlama ve her zamana bağlı tablo yaşam döngüsü yönetiminden önemli bir özelliği olan. Azure SQL veritabanı ile zamana bağlı tabloda geçmiş verileri yönetmek için aşağıdaki yaklaşımlardan vardır:

* [Tablo bölümleme](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_2)
* [Özel temizleme betiğini](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_3)

## <a name="next-steps"></a>Sonraki adımlar
Zamana bağlı tablolarda hakkında ayrıntılı bilgi için kullanıma [MSDN belgelerine](https://msdn.microsoft.com/library/dn935015.aspx).
Dinlemek için kanal 9 ziyaret bir [gerçek müşteri zamana bağlı implemenation başarı Öykü](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) ve izleme bir [zamana bağlı tanıtım canlı](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).

