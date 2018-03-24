---
title: Azure SQL veritabanı yönetilen örneği denetim | Microsoft Docs
description: Azure SQL veritabanı yönetilen örnek T-SQL kullanarak denetimi ile çalışmaya başlama öğrenin
services: sql-database
author: giladm
manager: craigg
ms.reviewer: carlrab
ms.service: sql-database
ms.custom: security
ms.topic: article
ms.date: 03/19/2018
ms.author: giladm
ms.openlocfilehash: 3d5a4ad3f4046dfdfe6eb3f7ddd931ccb240b1a9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="get-started-with-azure-sql-database-managed-instance-auditing"></a>Azure SQL veritabanı yönetilen örneği denetimi ile çalışmaya başlama

[Yönetilen Azure SQL veritabanı örneği](sql-database-managed-instance.md) denetim parçaları veritabanı olayları ve Azure depolama hesabınızdaki bunları Denetim günlüğüne yazar. Ayrıca denetleme:
- Yönetmeliklere uygunluğu korumanıza, veritabanı etkinliklerini anlamanıza ve ifade eden tutarsızlıklar ve iş endişeleri veya güvenlik ihlalleri hakkında daha fazla bilgi kavramanıza yardımcı olur.
- Etkinleştirir ve uyumluluk garanti etmez ancak Uyumluluk standartlara uyulması kolaylaştırır. Bu destek standartları uyumluluğu Azure hakkında daha fazla bilgi programlar için bkz: [Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/compliance/).


## <a name="set-up-auditing-for-your-server"></a>Sunucunuz için denetimi ayarlamanız

Aşağıdaki bölümde yönetilen örneğinizi denetimi yapılandırmasını açıklar.
1. [Azure Portal](https://portal.azure.com) gidin.
2. Aşağıdaki adımlar bir Azure Storage oluşturma **kapsayıcı** denetim günlüklerinin depolandığı.

   - Burada, denetim günlüklerini depolamak istediğiniz Azure Storage gidin.

     > [!IMPORTANT]
     > Çapraz bölge okuma/yazma önlemek için örnek yönetilen sunucu ile aynı bölgede bir depolama hesabı kullanın.

   - Depolama hesabı'nda Git **genel bakış** tıklatıp **BLOB'lar**.

     ![Gezinti bölmesi][1]

   - Üst menüde tıklatın **+ kapsayıcı** yeni bir kapsayıcı oluşturmak için.

     ![Gezinti bölmesi][2]

   - Bir kapsayıcı sağlayan **adı**, genel erişim düzeyini ayarlamak **özel**ve ardından **Tamam**.

     ![Gezinti bölmesi][3]

   - Kapsayıcıları listesinde yeni oluşturulan kapsayıcısını tıklatın ve ardından **kapsayıcı özellikleri**.

     ![Gezinti bölmesi][4]

   - Kopyala simgesine tıklayarak kapsayıcı URL'yi kopyalayın ve URL'de (örneğin, Not Defteri) gelecekte kullanım için kaydedin. Kapsayıcı URL biçiminde olmalıdır `https://<StorageName>.blob.core.windows.net/<ContainerName>`

     ![Gezinti bölmesi][5]

3. Aşağıdaki adımlar bir Azure depolama oluşturulması **SAS belirteci** depolama hesabı örneği yönetilen denetim erişim hakkı vermek için kullanılır.

   - Kapsayıcı önceki adımda oluşturduğunuz Azure depolama hesabına gidin.

   - Tıklayın **paylaşılan erişim imzası** depolama Ayarlar menüsünden.

     ![Gezinti bölmesi][6]

   - SAS aşağıdaki gibi yapılandırın:
     - **Hizmetleri izin**: Blob
     - **Başlangıç tarihi**: saat dilimi ilgili sorunlardan kaçınmak amacıyla, düne ait tarih kullanılması önerilir.
     - **Bitiş tarihi**: üzerinde bu SAS belirteci süre sonu tarihi seçin. 

       > [!NOTE]
       > Denetim hatalarını önlemek için süre sonu bağlı belirtecini yenileyin.

     - **SAS Oluştur**’a tıklayın.

       ![Gezinti bölmesi][7]

   - SAS oluşturmak tıkladıktan sonra SAS belirteci en altında görüntülenir. Kopyala simgesine tıklayarak belirteci kopyalayın ve gelecekte kullanılmak üzere (örneğin, Not Defteri'nde) kaydedin.

     > [!IMPORTANT]
     > Soru işareti kaldırın ("?") belirteç başından itibaren karakter.

     ![Gezinti bölmesi][8]

4. Yönetilen örneğinizi SQL Server Management Studio'yu (SSMS) üzerinden bağlanır.

5. Aşağıdaki T-SQL deyimini yürütün **yeni bir kimlik bilgisi oluşturma** kapsayıcı URL'si ve SAS, önceki adımda oluşturduğunuz belirteç kullanma:

    ```SQL
    CREATE CREDENTIAL [<container_url>]
    WITH IDENTITY='SHARED ACCESS SIGNATURE',
    SECRET = '<SAS KEY>'
    GO
    ```

6. Sunucu yeni bir denetim oluşturmak için aşağıdaki T-SQL deyimini yürütün (kendi denetim adı seçin, önceki adımlarda oluşturduğunuz kapsayıcı URL'sini kullanarak):

    ```SQL
    CREATE SERVER AUDIT [<your_audit_name>]
    TO URL ( PATH ='<container_url>' [, RETENTION_DAYS =  integer ])
    GO
    ```

    Belirtilmezse, `RETENTION_DAYS` varsayılan değer 0 (sınırsız bekletme).

    Ek bilgi için:
    - [Yönetilen örneği, Azure SQL DB ve SQL Server arasındaki farklar denetleme](#subheading-3)
    - [SUNUCU DENETİM OLUŞTURMA](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)
    - [ALTER SERVER DENETİM](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)

7. SQL Server gibi sunucu denetim belirtimi veya veritabanı denetim belirtimine oluşturun:
    - [Sunucu denetim belirtimi T-SQL kılavuzu oluşturma](https://docs.microsoft.com/ sql/t-sql/statements/create-server-audit-specification-transact-sql)
    - [Veritabanı denetim belirtimine T-SQL kılavuzu oluşturma](https://docs.microsoft.com/ sql/t-sql/statements/create-database-audit-specification-transact-sql)

8. 6. adımda oluşturduğunuz sunucu denetimi etkinleştirin:

    ```SQL
    ALTER SERVER AUDIT [<your_audit_name>]
    WITH (STATE=ON);
    GO
    ```

## <a name="analyze-audit-logs"></a>Denetim günlüklerini analiz edin
Blob denetim günlüklerini görüntülemek için kullanabileceğiniz çeşitli yöntemler vardır.

- Bir sistem işlevi kullanın `sys.fn_get_audit_file` (Denetim günlüğü verilerini tablo biçiminde döndürmek için T-SQL). Bu işlev kullanma hakkında daha fazla bilgi için bkz: [sys.fn_get_audit_file belgelerine](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).

- Denetim Günlüğü Tüketim yöntemi tam listesi için bkz [SQL veritabanı denetimi ile çalışmaya başlama](https://docs.microsoft.com/ azure/sql-database/sql-database-auditing).

> [!IMPORTANT]
> Denetim kayıtlarının ('kayıtları denetim' bölme) Azure portalından görüntüleme yöntemi yönetilen örneği için şu anda kullanılamıyor.

## <a name="auditing-differences-between-managed-instance-azure-sql-database-and-sql-server"></a>Yönetilen örneği, Azure SQL Database ve SQL Server arasındaki farklar denetleme

Şirket içi yönetilen örneği, Azure SQL Database ve SQL Server SQL denetleme arasındaki temel farklılıklar şunlardır:

- Sunucu düzeyinde ve depoları yönetilen örneğinde SQL denetim çalışır `.xel` günlük dosyaları Azure blob depolama hesabı üzerinde.
- Azure SQL veritabanı'nda SQL denetim veritabanı düzeyinde çalışır.
- Şirket içi SQL Server / sanal makineler, sunucudaki SQL denetim works düzeyi, dosyaları sistem/windows olaylarına depolar ancak olay günlükleri.

Yönetilen örneğinde XEvent denetim Azure blob depolama hedefleri destekler. Dosya ve windows günlükleri **desteklenmiyor**.

Anahtar farkları `CREATE AUDIT` sözdizimi denetimi için Azure blob depolama için:
- Yeni bir sözdizimi `TO URL` sağlanır ve Azure blob depolama kapsayıcısını URL'sini belirtmenize olanak tanıyan nerede `.xel` dosyaları yerleştirilir.
- Sözdizimi `TO FILE` olan **desteklenmiyor** yönetilen örneği Windows dosya paylaşımlarında erişemediği için.
- Kapatma seçenek **desteklenmiyor**.
- `queue_delay` 0 **desteklenmiyor**.


## <a name="next-steps"></a>Sonraki adımlar

- Denetim Günlüğü Tüketim yöntemi tam listesi için bkz [SQL veritabanı denetimi ile çalışmaya başlama](https://docs.microsoft.com/ azure/sql-database/sql-database-auditing).
- Bu destek standartları uyumluluğu Azure hakkında daha fazla bilgi programlar için bkz: [Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/compliance/).


<!--Anchors-->
[Set up auditing for your server]: #subheading-1
[Analyze audit logs]: #subheading-2
[Auditing differences between Managed Instance, Azure SQL DB and SQL Server]: #subheading-3

<!--Image references-->
[1]: ./media/sql-managed-instance-auditing/1_blobs_widget.png
[2]: ./media/sql-managed-instance-auditing/2_create_container_button.png
[3]: ./media/sql-managed-instance-auditing/3_create_container_config.png
[4]: ./media/sql-managed-instance-auditing/4_container_properties_button.png
[5]: ./media/sql-managed-instance-auditing/5_container_copy_name.png
[6]: ./media/sql-managed-instance-auditing/6_storage_settings_menu.png
[7]: ./media/sql-managed-instance-auditing/7_sas_configure.png
[8]: ./media/sql-managed-instance-auditing/8_sas_copy.png
