---
title: "Azure SQL veritabanlarının gruplarını yönetme | Microsoft Docs"
description: "Oluşturulması ve esnek bir iş yönetimi yol."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: f858344d-085b-4022-935e-1b5fa20adbac
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 9ccd7d78169fa5324808e91724e8e193b56b0290
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="create-and-manage-scaled-out-azure-sql-databases-using-elastic-jobs-preview"></a>Oluşturma ve esnek işleri (Önizleme) kullanarak Azure SQL veritabanlarını ölçeklendirilmiş yönetme


**Esnek veritabanı iş** şema değişiklikleri, kimlik bilgileri yönetimi, başvuru veri güncelleştirmeleri, performans verileri toplama veya Kiracı (müşteri) telemetri koleksiyonu gibi yönetim işlemlerini yürüterek veritabanlarının gruplarının yönetimi kolaylaştırabilir. Esnek veritabanı iş PowerShell cmdlet'leri ve Azure portal şu anda mevcut değil. Ancak, Azure portal yüzeyleri tüm veritabanları arasında yürütme için sınırlı işlevsellik sınırlı bir [esnek havuz (Önizleme)](sql-database-elastic-pool.md). Ek özellikler ve betiklerinin yürütülmesi özel tanımlı bir koleksiyon veya bir parça dahil olmak üzere veritabanları grubu erişmek için (kullanılarak oluşturulan [esnek veritabanı istemci Kitaplığı](sql-database-elastic-scale-introduction.md)), bkz: [oluşturma ve PowerShell kullanarak işleri yönetme](sql-database-elastic-jobs-powershell.md). İşleri hakkında daha fazla bilgi için bkz: [esnek veritabanı işleri genel bakış](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Ön koşullar
* Azure aboneliği. Ücretsiz deneme için bkz: [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).
* Bir esnek havuz. Bkz: [esnek havuzları hakkında](sql-database-elastic-pool.md).
* Esnek veritabanı iş hizmet bileşenlerini yükleme. Bkz: [esnek veritabanı iş hizmetini yükleme](sql-database-elastic-jobs-service-installation.md).

## <a name="creating-jobs"></a>İşleri oluşturuluyor
1. Kullanarak [Azure portal](https://portal.azure.com), var olan bir esnek veritabanı iş havuzundan tıklatın **oluşturma işi**.
2. Kullanıcı adı ve parolayı (işleri yükleme sırasında oluşturulan) veritabanı yöneticisi, iş kontrol veritabanına (meta veri depolama alanı işleri için) yazın.
   
    ![İş adı yazın veya kodda yapıştırın ve Çalıştır'ı tıklatın][1]
3. İçinde **işi oluştur** dikey penceresinde, iş için bir ad yazın.
4. Betik yürütme işleminin başarılı olması yeterli izinlere sahip hedef veritabanlarına bağlanmak için parola ve kullanıcı adını yazın.
5. T-SQL betiği yazın veya yapıştırın.
6. Tıklatın **kaydetmek** ve ardından **çalıştırmak**.
   
    ![İşleri oluşturma ve çalıştırma][5]

## <a name="run-idempotent-jobs"></a>Idempotent işleri çalıştırma
Bir veritabanları kümesi karşı bir komut çalıştırdığınızda, komut dosyası ıdempotent olduğundan emin olmanız gerekir. Diğer bir deyişle, onu önce tamamlanmamış bir durumda başarısız olmuş olsa bile komut dosyası birden çok kez çalıştırmanız mümkün olması gerekir. Bir komut dosyası başarısız olduğunda, başarılı olana dek Örneğin, iş otomatik olarak yeniden denenecek (sınırlarda yeniden deneme mantığı sonunda yeniden deneniyor olmaz). Bunu yapmanın yolu kullanmaktır bir "IF var" yan tümcesi ve yeni bir nesne oluşturmadan önce bulunan örnek silin. Örneği burada gösterilir:

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

Alternatif olarak, yeni bir örneğini oluşturmadan önce bir "IF değil var" yan tümcesi kullanın:

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

Bu komut dosyası ardından daha önce oluşturduğunuz tablosunu güncelleştirir.

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN

    ALTER TABLE TestTable

    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO


## <a name="checking-job-status"></a>İş durumunu denetleme
Bir işi başladıktan sonra üzerinde ilerleme durumunu denetleyebilirsiniz.

1. Esnek havuz sayfasından tıklatın **yönetmek işleri**.
   
    !["Yönet projeler"'i tıklatın][2]
2. Adına bir işin (a) tıklayın. **Durum** "Tamamlandı" veya "Başarısız" olabilir İş ayrıntıları, tarih ve saati oluşturma ve çalıştırma (b) görünür. , Aşağıdaki listeden (c), tarih ve saat ayrıntılarını vermiş havuzunda her veritabanında betik ilerlemesini gösterir.
   
    ![Tamamlanmış iş denetleniyor][3]

## <a name="checking-failed-jobs"></a>İşlerini denetimi başarısız oldu
Bir işi başarısız olursa, yürütme günlüğü bulunabilir. Ayrıntılarını görmek için başarısız iş adına tıklayın.

![Başarısız işi denetleyin][4]

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png


