---
title: Yedeklemeyi Azure SQL Veritabanı Yönetilen Örneği’ne geri yükleme| Microsoft Docs
description: SSMS kullanarak veritabanı yedeklemesini Azure SQL Veritabanı Yönetilen Örneğine geri yükleyin.
keywords: sql veritabanı öğreticisi, sql veritabanı yönetilen örneği, yedeklemeyi geri yükleme
services: sql-database
author: bonova
ms.reviewer: carlrab, srbozovi
ms.service: sql-database
ms.custom: managed instance
ms.topic: tutorial
ms.date: 04/10/2018
ms.author: bonova
manager: craigg
ms.openlocfilehash: ff605b7512a27f81b111560f5d151010dbb62273
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31426277"
---
# <a name="restore-a-database-backup-to-an-azure-sql-database-managed-instance"></a>Veritabanı yedeklemesini Azure SQL Veritabanı Yönetilen Örneğine geri yükleme

Bu öğreticide, Wide World Importers - Standart yedekleme dosyasını kullanarak Azure blob depolama’da depolanan bir veritabanı yedeklemesinin Yönetilen Örneğe nasıl geri yükleneceği gösterilmektedir. Bu yöntem miktar biraz kesinti süresine neden olur. Azure Veritabanı Geçiş Hizmeti’ni (DMS) geçiş için kullanmaya ilişkin bir öğretici için bkz. [DMS kullanarak Yönetilen Örnek geçişi](../dms/tutorial-sql-server-to-managed-instance.md). Çeşitli geçiş yöntemleriyle ilgili tartışma için bkz. [SQL Server örneğinin Azure SQL Veritabanı Yönetilen Örneği’ne geçişi](sql-database-managed-instance-migrate.md).

> [!div class="checklist"]
> * Wide World Importers - Standart yedekleme dosyasını indirme
> * Azure depolama hesabı oluşturma ve yedekleme dosyasını karşıya yükleme
> * Wide World Importers veritabanını bir yedekleme dosyasından geri yükleme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide, [Azure SQL Veritabanı Yönetilen Örneği Oluşturma](sql-database-managed-instance-create-tutorial-portal.md) öğreticisinde oluşturulan kaynaklar başlangıç noktası olarak kullanılmaktadır.

## <a name="download-the-wide-world-importers---standard-backup-file"></a>Wide World Importers - Standart yedekleme dosyasını indirme

Wide World Importers - Standart yedekleme dosyasını indirmek için aşağıdaki adımları kullanın.

Internet Explorer’ı kullanarak URL adres kutusuna https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImporters-Standard.bak adresini girin ve sonra istendiğinde bu dosyayı **İndirilenler** klasörüne kaydetmek için **Kaydet**’e tıklayın.

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/#create/Microsoft.SQLManagedInstance)’da oturum açın.

## <a name="create-azure-storage-account-and-upload-backup-file"></a>Azure depolama hesabı oluşturma ve yedekleme dosyasını karşıya yükleme

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. **Depolama**’yı bulup **Depolama Hesabı**’na tıklayarak depolama hesabı formunu açın.

   ![depolama hesabı](./media/sql-database-managed-instance-tutorial/storage-account.png)

3. Depolama hesabı formunu aşağıdaki tabloda verilen bilgileri kullanarak istenen bilgilerle doldurun:

   | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   |**Ad**|Geçerli bir ad|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Dağıtım modeli**|Kaynak modeli||
   |**Hesap türü**|Blob depolama||
   |**Performans**|Standart veya premium|Manyetik sürücüler veya SSD’ler|
   |**Çoğaltma**|Yerel olarak yedekli depolama||
   |\*\*Erişim katmanı (varsayılan)|Seyrek veya sık||
   |**Güvenli aktarım gerekir**|Devre dışı||
   |**Abonelik**|Aboneliğiniz|Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions).|
   |**Kaynak grubu**|Önceden oluşturduğunuz kaynak grubu|| 
   |**Konum**|Daha önce seçtiğiniz konum||
   |**Sanal ağlar**|Devre dışı||

4. **Oluştur**’a tıklayın.

   ![depolama hesabı ayrıntıları](./media/sql-database-managed-instance-tutorial/storage-account-details.png)

5. Depolama hesabı dağıtımı başarılı olduktan sonra yeni depolama hesabınızı açın.
6. **Ayarlar** altında **Paylaşılan Erişim İmzası**’na tıklayarak Paylaşılan erişim imzası (SAS) formunu açın.

   ![sas formu](./media/sql-database-managed-instance-tutorial/sas-form.png)

7. SAS formunda varsayılan değerleri istenen şekilde değiştirin. Sona erme tarihi/saatinin varsayılan olarak yalnızca 8 saat olduğuna dikkat edin.
8. **SAS Oluştur**’a tıklayın.

   ![sas formu tamamlandı](./media/sql-database-managed-instance-tutorial/sas-generate.png)

9. **SAS belirtecini** ve **Blob sunucusu SAS URL’sini** kopyalayıp kaydedin.
10. **Ayarlar** altında **Kapsayıcılar**'a tıklayın.

    ![containers](./media/sql-database-managed-instance-tutorial/containers.png)

11. **+Kapsayıcı**’ya tıklayarak yedekleme dosyanızın tutulacağı kapsayıcıyı oluşturun.
12. Kapsayıcı formunu aşağıdaki tabloda verilen bilgileri kullanarak istenen bilgilerle doldurun:

    | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   |**Ad**|Geçerli bir ad|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Genel erişim düzeyi**|Özel (anonim erişim yok)||

    ![kapsayıcı ayrıntısı](./media/sql-database-managed-instance-tutorial/container-detail.png)

13. **Tamam**’a tıklayın.
14. Kapsayıcı oluşturulduktan sonra kapsayıcıyı açın.

    ![kapsayıcı](./media/sql-database-managed-instance-tutorial/container.png)

15. **Kapsayıcı özellikleri**’ne tıklayın ve ardından kapsayıcıya URL'yi kopyalayın.

    ![kapsayıcı URL’si](./media/sql-database-managed-instance-tutorial/container-url.png)

16. **Karşıya Yükle**’ye tıklayarak **Blobu karşıya yükle** formunu açın.

    ![karşıya yükle](./media/sql-database-managed-instance-tutorial/upload.png)

17. İndirilenler klasörünüze göz atın ve **WideWorldIimporters-Standard.bak** dosyasını seçin.

    ![karşıya yükle](./media/sql-database-managed-instance-tutorial/upload-bak.png)

18. **Karşıya Yükle**'ye tıklayın.
19. Karşıya yükleme işlemi tamamlanana kadar devam etmeyin.

    ![karşıya yükleme tamamlandı](./media/sql-database-managed-instance-tutorial/upload-complete.png)


## <a name="restore-the-wide-world-importers-database-from-a-backup-file"></a>Wide World Importers veritabanını bir yedekleme dosyasından geri yükleme

SSMS ile Wide World Importers veritabanını yedekleme dosyasından Yönetilen Örneğinize geri yüklemek için aşağıdaki adımları kullanın.

1. SSMS içinde yeni bir sorgu penceresi açın.
2. Aşağıdaki betiği kullanarak bir SAS kimlik bilgisi oluşturun; depolama hesabı kapsayıcısının URL’sini ve SAS anahtarını belirtilen şekilde sağlayın.

   ```sql
   CREATE CREDENTIAL [https://<storage_account_name>.blob.core.windows.net/<container>] 
      WITH IDENTITY = 'SHARED ACCESS SIGNATURE'
      , SECRET = '<shared_access_signature_key_with_removed_first_?_symbol>' 
   ```

    ![kimlik bilgisi](./media/sql-database-managed-instance-tutorial/credential.png)

3. Aşağıdaki betiği kullanarak SAS kimlik bilgisini ve yedekleme geçerliliğini denetleyin; yedekleme dosyasını içeren kapsayıcının URL’sini sağlayın:

   ```sql
   RESTORE FILELISTONLY FROM URL = 
      'https://<storage_account_name>.blob.core.windows.net/<container>/WideWorldImporters-Standard.bak'
   ```

    ![dosya listesi](./media/sql-database-managed-instance-tutorial/file-list.png)

4. Aşağıdaki betiği kullanarak Adventure Works 2012 veritabanını bir yedekleme dosyasından geri yükleyin; yedekleme dosyasını içeren kapsayıcının URL’sini sağlayın:

   ```sql
   RESTORE DATABASE [Wide World Importers] FROM URL =
     'https://<storage_account_name>.blob.core.windows.net/<container>/WideWorldImporters-Standard.bak'`
   ```

    ![geri yükleme yürütülüyor](./media/sql-database-managed-instance-tutorial/restore-executing.png)

5. Geri yükleme işleminizin durumunu izlemek için aşağıdaki sorguyu yeni bir sorgu oturumunda çalıştırın:

   ```sql
   SELECT session_id as SPID, command, a.text AS Query, start_time, percent_complete
      , dateadd(second,estimated_completion_time/1000, getdate()) as estimated_completion_time 
   FROM sys.dm_exec_requests r 
   CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a 
   WHERE r.command in ('BACKUP DATABASE','RESTORE DATABASE')`
   ```

    ![geri yükleme tamamlanma yüzdesi](./media/sql-database-managed-instance-tutorial/restore-percent-complete.png)

6. Geri yükleme tamamlandığında Nesne Gezgini içinde görüntüleyin. 

    ![geri yükleme tamamlandı](./media/sql-database-managed-instance-tutorial/restore-complete.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Wide World Importers - Standart yedekleme dosyasını kullanarak Azure blob depolama’da depolanan bir veritabanı yedeklemesinin Yönetilen Örneğe nasıl geri yükleneceğini öğrendiniz. Şunları öğrendiniz: 

> [!div class="checklist"]
> * Wide World Importers - Standart yedekleme dosyasını indirme
> * Azure depolama hesabı oluşturma ve yedekleme dosyasını karşıya yükleme
> * Wide World Importers veritabanını bir yedekleme dosyasından geri yükleme

DMS kullanarak SQL Server’ın Azure SQL Veritabanı Yönetilen Örneği’ne nasıl geçirileceğini öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
>[DMS kullanarak SQL Server’ı Azure SQL Veritabanı Yönetilen Örneği’ne geçirme](../dms/tutorial-sql-server-to-managed-instance.md)