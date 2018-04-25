---
title: 'Hızlı Başlangıç: Azure SQL Veri Ambarı’nda işlem ölçeğini genişletme - T-SQL | Microsoft Docs'
description: T-SQL ve SQL Server Management Studio (SSMS) kullanarak Azure SQL Veri Ambarı’nda işlemi ölçeklendirin. Daha iyi performans için işlemin ölçeğini genişletin veya maliyet tasarrufu sağlamak için işlemin ölçeğini geri daraltın.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: b4e123475679cf1afce09630c157377ee67b5202
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-using-t-sql"></a>Hızlı Başlangıç: T-SQL kullanarak Azure SQL Veri Ambarı’nda işlemi ölçeklendirme

T-SQL ve SQL Server Management Studio (SSMS) kullanarak Azure SQL Veri Ambarı’nda işlemi ölçeklendirin. Daha iyi performans için [işlemin ölçeğini genişletin](sql-data-warehouse-manage-compute-overview.md) veya maliyet tasarrufu sağlamak için işlemin ölçeğini geri daraltın. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

[SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms.md)’nun (SSMS) en yeni sürümünü indirin ve yükleyin.

Burada [Hızlı Başlangıç: Oluşturma ve bağlanma - portal](create-data-warehouse-portal.md) bölümünü tamamladığınız varsayılır. Oluşturma ve Bağlanma hızlı başlangıcını tamamladıktan sonra şunların nasıl yapılacağını öğrenmiş olursunuz: **mySampleDataWarehouse** adlı bir veri ambarı oluşturup buna bağlanma ve istemcimizin sunucuya erişmesini sağlayan güvenlik duvarı kuralı oluşturup yükleme.
 
## <a name="create-a-data-warehouse"></a>Veri ambarı oluşturma

[Hızlı Başlangıç: Oluşturma ve bağlanma - portal](create-data-warehouse-portal.md) bölümünü kullanarak **mySampleDataWarehouse** adlı bir veri ambarı oluşturun. Bir güvenlik duvarı kuralınız olduğundan ve SQL Server Management Studio içinden veri ambarınıza bağlanabildiğinizden emin olmak için hızlı başlangıcı tamamlayın.

## <a name="connect-to-the-server-as-server-admin"></a>Sunucu yöneticisi olarak sunucuya bağlanma

Bu bölümde Azure SQL sunucunuzla bağlantı kurmak için [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms.md) (SSMS) kullanılmaktadır.

1. SQL Server Management Studio’yu açın.

2. **Sunucuya Bağlan** iletişim kutusuna şu bilgileri girin:

   | Ayar       | Önerilen değer | Açıklama | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Sunucu türü | Veritabanı altyapısı | Bu değer gereklidir |
   | Sunucu adı | Tam sunucu adı | Örnek: **mynewserver-20171113.database.windows.net**. |
   | Kimlik Doğrulaması | SQL Server Kimlik Doğrulaması | Bu öğreticide yapılandırılan tek kimlik doğrulaması türü SQL Kimlik Doğrulamasıdır. |
   | Oturum Aç | Sunucu yöneticisi hesabı | Bu, sunucuyu oluştururken belirttiğiniz hesaptır. |
   | Parola | Sunucu yöneticisi hesabınızın parolası | Bu, sunucuyu oluştururken belirttiğiniz paroladır. |

    ![sunucuya bağlan](media/load-data-from-azure-blob-storage-using-polybase/connect-to-server.png)

4. **Bağlan**'a tıklayın. SSMS’te Nesne Gezgini penceresi açılır. 

5. Nesne Gezgini’nde, **Veritabanları**’nı genişletin. Daha sonra yeni veritabanınızdaki nesneleri görüntülemek için **mySampleDatabase**’i genişletin.

    ![veritabanı nesneleri](media/create-data-warehouse-portal/connected.png) 

## <a name="view-service-objective"></a>Hizmet hedefini görüntüleme
Hizmet hedefi ayarı, veri ambarı için veri ambarı birimleri sayısını içerir. 

Veri ambarınız için geçerli veri ambarı birimlerini görüntülemek için:

1. **mynewserver-20171113.database.windows.net** bağlantısının altında **Sistem Veritabanları**’nı genişletin.
2. **Ana** seçeneğine sağ tıklayıp **Yeni Sorgu**’yu seçin. Yeni bir sorgu penceresi açılır.
3. sys.database_service_objectives dinamik yönetim görünümünden seçim yapmak için aşağıdaki sorguyu çalıştırın. 

    ```sql
    SELECT
        db.name [Database]
    ,   ds.edition [Edition]
    ,   ds.service_objective [Service Objective]
    FROM
        sys.database_service_objectives ds
    JOIN
        sys.databases db ON ds.database_id = db.database_id
    WHERE 
        db.name = 'mySampleDataWarehouse'
    ```

4. Aşağıdaki sonuçlar, **mySampleDataWarehouse** öğesinin DW400 hizmet hedefine sahip olduğunu gösterir. 

    ![Geçerli DWU’ları görüntüleme](media/quickstart-scale-compute-tsql/view-current-dwu.png)


## <a name="scale-compute"></a>Hesaplamayı ölçeklendirme
SQL Veri Ambarı’nda, veri ambarı birimlerini ayarlayarak işlem kaynaklarını artırabilir veya azaltabilirsiniz. [Oluşturma ve Bağlanma - portal](create-data-warehouse-portal.md) bölümünde **mySampleDataWarehouse** oluşturuldu ve 400 DWU ile başlatıldı. Aşağıdaki adımlar, **mySampleDataWarehouse** için DWU’ları ayarlar.

Veri ambarı birimlerini değiştirmek için:

1. **Ana** seçeneğine sağ tıklayıp **Yeni Sorgu**’yu seçin.
2. [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) T-SQL deyimini kullanarak hizmet hedefini değiştirin. Hizmet hedefini için DW300 olarak değiştirmek için aşağıdaki sorguyu çalıştırın. 

```Sql
ALTER DATABASE mySampleDataWarehouse
MODIFY (SERVICE_OBJECTIVE = 'DW300')
;
```

## <a name="check-data-warehouse-state"></a>Veri ambarı durumunu denetleme

Bir veri ambarı duraklatıldığında için T-SQL ile buna bağlanamazsınız. Veri ambarının geçerli durumunu görmek için bir PowerShell cmdlet’ini kullanabilirsiniz. Bir örnek için bkz. [Veri ambarı durumunu denetleme - Powershell](quickstart-scale-compute-powershell.md#check-data-warehouse-state). 

## <a name="check-operation-status"></a>İşlem durumunu denetleme

Çeşitli yönetim işlemleriyle ilgili bilgileri SQL Veri Ambarı’na döndürmek için [sys.dm_operation_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) DMV’de aşağıdaki sorguyu çalıştırın. Örneğin, bu işlemi ve işlemin durumunu (IN_PROGRESS veya COMPLETED) döndürür.

```sql
SELECT *
FROM
    sys.dm_operation_status
WHERE
    resource_type_desc = 'Database'
AND 
    major_resource_id = 'MySQLDW'
```


## <a name="next-steps"></a>Sonraki adımlar
Şimdi veri ambarınız için işlemin nasıl ölçeklendirileceğini öğrendiniz. Azure SQL Veri Ambarı hakkında daha fazla bilgi edinmek için, veri yükleme öğreticisiyle devam edin.

> [!div class="nextstepaction"]
>[SQL veri ambarına veri yükleme](load-data-from-azure-blob-storage-using-polybase.md)
