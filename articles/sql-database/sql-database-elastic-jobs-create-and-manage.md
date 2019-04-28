---
title: Azure SQL veritabanı grupları yönetme | Microsoft Docs
description: Oluşturulması ve esnek bir iş yönetimi yol.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: a95b62ab8f639ad38ee3ac9ace4f30b62bd852bf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61476060"
---
# <a name="create-and-manage-scaled-out-azure-sql-databases-using-elastic-jobs-preview"></a>Oluşturma ve elastik işleri (Önizleme) kullanarak Azure SQL veritabanlarına ölçeği yönetme


[!INCLUDE [elastic-database-jobs-deprecation](../../includes/sql-database-elastic-jobs-deprecate.md)]


**Elastik veritabanı işleri** şema değişiklikleri, kimlik yönetimi, başvuru verilerini güncelleştirme, performans verileri toplama veya Kiracı (müşteri) telemetrisi gibi yönetim işlemlerini yürüterek veritabanı grupların yönetimini basitleştirin koleksiyonu. Elastik veritabanı işleri PowerShell cmdlet'leri ve Azure portalı şu anda mevcut değildir. Ancak, Azure portal yüzeyleri için yürütme tüm veritabanları arasında sınırlı işlevselliği azaltılmış bir [elastik havuz](sql-database-elastic-pool.md). Ek özellikler ve komut dosyası yürütme, özel tanımlı bir koleksiyon veya bir parça dahil olmak üzere veritabanlarından oluşan bir grupta erişmek için (kullanılarak oluşturulan [elastik veritabanı istemci Kitaplığı](sql-database-elastic-scale-introduction.md)), bkz: [oluşturma ve yönetme PowerShell kullanarak işleri](sql-database-elastic-jobs-powershell.md). İşleri hakkında daha fazla bilgi için bkz. [esnek veritabanı işlerine genel bakış](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Önkoşullar
* Azure aboneliği. Ücretsiz deneme için bkz: [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).
* Elastik havuz. Bkz: [elastik havuzlar hakkında](sql-database-elastic-pool.md).
* Elastik veritabanı iş Hizmeti bileşenlerinin yüklenmesi. Bkz: [elastik veritabanı iş hizmetini yüklemeden](sql-database-elastic-jobs-service-installation.md).

## <a name="creating-jobs"></a>İşleri oluşturma
1. Kullanarak [Azure portalında](https://portal.azure.com), var olan bir esnek veritabanı iş havuzundan tıklayın **oluşturma işi**.
2. İşleri denetim veritabanı (işleri için meta veri depolama) için kullanıcı adı ve (işlerinin bir yükleme sırasında oluşturulan) veritabanı yöneticisi parolasını yazın.
   
    ![İş adı yazın veya kodu yapıştırın ve Çalıştır'a tıklayın][1]
3. İçinde **işi oluştur** dikey penceresinde, iş için bir ad yazın.
4. Betik yürütme başarılı olması için yeterli izinlere sahip hedef veritabanlarına bağlanmak için parola ve kullanıcı adını yazın.
5. Yapıştırma veya T-SQL betiğini yazın.
6. Tıklayın **Kaydet** ve ardından **çalıştırma**.
   
    ![İşleri oluşturma ve çalıştırma][5]

## <a name="run-idempotent-jobs"></a>Idempotent işleri çalıştırma
Bir veritabanları kümesi karşı bir komut çalıştırdığınızda, betiğin kez etkili olduğundan emin olmanız gerekir. Diğer bir deyişle, bunu önce tamamlanmamış bir durum içinde başarısız olsa bile komut dosyası birden çok kez çalıştırmak mümkün olması gerekir. Bir betiği başarısız olduğunda, başarılı olana kadar gibi iş otomatik olarak yeniden denenecek (sınırlar içinde yeniden deneme mantığı sonunda yeniden deneniyor sona erecek). Bunu yapmanın bir yolu bir "IF var" yan tümcesi ve yeni bir nesne oluşturmadan önce tüm bulunan örneğini silin. Bir örnek aşağıda gösterilmiştir:

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

Alternatif olarak, yeni bir örneğini oluşturmadan önce bir "IF NOT var" yan tümcesi kullanın:

    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
     CREATE TABLE TestTable(
      TestTableId INT PRIMARY KEY IDENTITY,
      InsertionTime DATETIME2
     );
    END
    GO

    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO

Bu betik, ardından daha önce oluşturduğunuz tabloda güncelleştirir.

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN

    ALTER TABLE TestTable

    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO


## <a name="checking-job-status"></a>İş durumunu kontrol etme
Bir işi başladıktan sonra, ilerleme durumunu kontrol edebilirsiniz.

1. Elastik havuz sayfasında **yönetmek, işleri**.
   
    !["İşler Yönet" tıklayın][2]
2. Adına bir işin (a) tıklayın. **Durumu** "Tamamlandı" veya "Başarısız oldu." İşin ayrıntıları (b), tarih ve saati oluşturma ve çalıştırma ile görünür. Aşağıdaki listeye (c), tarih ve saat ayrıntılarını vererek, havuzda her veritabanında betik ilerlemesini gösterir.
   
    ![Tamamlanan iş denetleniyor][3]

## <a name="checking-failed-jobs"></a>Denetimi başarısız olan işler
Bir işi başarısız olursa, yürütme günlüğü bulunabilir. Ayrıntılarını görmek için başarısız işi adına tıklayın.

![Başarısız işi inceleyin][4]

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png


