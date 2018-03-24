---
title: "Hızlı Başlangıç: Ölçek genişletme Azure SQL Data warehouse'da - T-SQL işlem | Microsoft Docs"
description: Dwu ayarlayarak işlem kaynaklarını ölçeklendirme için T-SQL komutları.
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: ''
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 03/16/2018
ms.author: elbutter;barbkess
ms.openlocfilehash: 1591192c72f5bf201dbbef80acc5895c8324fca4
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-using-t-sql"></a>Hızlı Başlangıç: T-SQL kullanarak Azure SQL veri ambarındaki bilgi işlem

Bilgi işlem T-SQL ve SQL Server Management Studio (SSMS) kullanarak Azure SQL veri ambarındaki. [Ölçeklendirme işlem](sql-data-warehouse-manage-compute-overview.md) daha iyi performans veya ölçek için maliyet tasarrufu sağlamak işlem yedekleyin. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

[SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms.md)’nun (SSMS) en yeni sürümünü indirin ve yükleyin.

Bu tamamladığınızdan varsayar [hızlı başlangıç: oluşturun ve bağlanın - portal](create-data-warehouse-portal.md). Oluşturma ve Connect hızlı başlangıç tamamladıktan sonra nasıl bağlanacağınızı bilmeniz: adlı bir veri ambarı oluşturulan **mySampleDataWarehouse**, bizim istemcinin yüklü olan sunucu erişmesine izin veren bir güvenlik duvarı kuralı oluşturdu.
 
## <a name="create-a-data-warehouse"></a>Veri ambarı oluşturma

Kullanım [hızlı başlangıç: oluşturun ve bağlanın - portal](create-data-warehouse-portal.md) adlı bir veri ambarı oluşturmak için **mySampleDataWarehouse**. Bir güvenlik duvarı kuralı varsa ve, veri ambarı SQL Server Management Studio içinde bağlanıp emin olmak için hızlı başlangıç tamamlayın.

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

## <a name="view-service-objective"></a>Görünüm hizmet hedefi
Hizmet hedefi ayarı veri ambarı için veri ambarı birim sayısını içerir. 

Veri ambarınız için geçerli veri ambarı birimlerini görüntülemek için:

1. Bağlantı altında **mynewserver 20171113.database.windows.net**, genişletin **sistem veritabanları**.
2. Sağ **ana** seçip **yeni sorgu**. Yeni bir sorgu penceresi açılır.
3. Sys.database_service_objectives dinamik yönetim görünümünden seçmek için aşağıdaki sorguyu çalıştırın. 

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

4. Aşağıdaki sonuçları göster **mySampleDataWarehouse** bir hizmet hedefi DW400 olan sahiptir. 

    ![Görünüm geçerli Dwu](media/quickstart-scale-compute-tsql/view-current-dwu.png)


## <a name="scale-compute"></a>Hesaplamayı ölçeklendirme
SQL veri ambarı'nda artırın veya işlem kaynakları data warehouse birimleri ayarlayarak azaltın. [Oluşturma ve Connect - portal](create-data-warehouse-portal.md) oluşturulan **mySampleDataWarehouse** ve 400 Dwu ile başlatıldı. Dwu için aşağıdaki adımları ayarlamak **mySampleDataWarehouse**.

Data warehouse birimleri değiştirmek için:

1. Sağ **ana** seçip **yeni sorgu**.
2. Kullanım [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) hizmet hedefi değiştirmek için T-SQL ifadesi. Hizmet hedefi için DW300 değiştirmek için aşağıdaki sorguyu çalıştırın. 

```Sql
ALTER DATABASE mySampleDataWarehouse
MODIFY (SERVICE_OBJECTIVE = 'DW300')
;
```

## <a name="check-data-warehouse-state"></a>Veri ambarı durumunu denetle

Bir veri ambarı duraklatıldığında için T-SQL ile bağlantı kurulamıyor. Veri ambarı geçerli durumunu görmek için bir PowerShell cmdlet'ini kullanabilirsiniz. Bir örnek için bkz: [veri ambarı durumu - Powershell denetleyin](quickstart-scale-compute-powershell.md#check-data-warehouse-state). 

## <a name="check-operation-status"></a>Onay işlemi durumu

SQL veri ambarı üzerinde çeşitli yönetim işlemlerini hakkında bilgi döndürmek için aşağıdaki sorguyu çalıştırın [sys.dm_operation_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) DMV. Örneğin, işlem ve tamamlandı veya IN_PROGRESS olacaktır işlemin durumunu döndürür.

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
Şimdi, veri ambarı için işlem ölçeklendirmek öğrendiniz. Azure SQL Veri Ambarı hakkında daha fazla bilgi edinmek için, veri yükleme öğreticisiyle devam edin.

> [!div class="nextstepaction"]
>[SQL veri ambarına veri yükleme](load-data-from-azure-blob-storage-using-polybase.md)
