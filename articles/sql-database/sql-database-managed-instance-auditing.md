---
title: Azure SQL veritabanı yönetilen örneği denetim | Microsoft Docs
description: Azure SQL veritabanı yönetilen örnek T-SQL kullanarak denetimini kullanmaya başlama hakkında bilgi edinin
services: sql-database
author: giladm
manager: craigg
ms.reviewer: carlrab
ms.service: sql-database
ms.custom: security
ms.topic: conceptual
ms.date: 08/28/2018
ms.author: giladm
ms.openlocfilehash: a43ca95717c712c932d29a619b7f1a0671c500bf
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43125450"
---
# <a name="get-started-with-azure-sql-database-managed-instance-auditing"></a>Azure SQL veritabanı yönetilen örneği denetimini kullanmaya başlama

[Azure SQL veritabanı yönetilen örneği](sql-database-managed-instance.md) denetimi veritabanı olaylarını izler ve bir denetim günlüğüne Azure depolama hesabınızdaki yazar. Ayrıca denetleme:
- Mevzuatla uyumluluk, veritabanı etkinliğini anlama ve tutarsızlıklar ve işletme sorunlarını veya şüpheli güvenlik ihlallerini anomalileri kavramanıza yardımcı olur.
- Etkinleştirir ve uyumluluk garanti etmez ancak uyumluluk standardını da kıldığı kolaylaştırır. Azure hakkında daha fazla bilgi bu standartlara uyumluluk programları için bkz: [Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/compliance/).


## <a name="set-up-auditing-for-your-server"></a>Sunucunuz için denetimi ayarlamanız

Aşağıdaki bölümde, yönetilen Örneğinize denetim yapılandırma açıklanmaktadır.
1. [Azure Portal](https://portal.azure.com) gidin.
2. Aşağıdaki adımlar bir Azure depolama oluşturur **kapsayıcı** denetim günlükleri nerede depolanır.

   - Azure denetim günlüklerinizi depolamak için istediğiniz depoya gidin.

     > [!IMPORTANT]
     > Bölgeler arası okumayı/yazmayı önlemek için yönetilen örnek sunucusu ile aynı bölgede bir depolama hesabı kullanın.

   - Depolama hesabında Git **genel bakış** tıklatıp **Blobları**.

     ![Gezinti bölmesi][1]

   - Üst menüde **+ kapsayıcı** yeni bir kapsayıcı oluşturmak için.

     ![Gezinti bölmesi][2]

   - Bir kapsayıcı **adı**, genel erişim düzeyini ayarlayın **özel**ve ardından **Tamam**.

     ![Gezinti bölmesi][3]

   - Kapsayıcılar listesinde yeni oluşturulan kapsayıcı tıklayın ve ardından **kapsayıcı özellikleri**.

     ![Gezinti bölmesi][4]

   - Kopyala simgesine tıklayarak kapsayıcı URL'sini kopyalayın ve kaydedin (örneğin, Not Defteri'nde) gelecekte kullanım için URL. Kapsayıcı URL biçiminde olmalıdır `https://<StorageName>.blob.core.windows.net/<ContainerName>`

     ![Gezinti bölmesi][5]

3. Aşağıdaki adımlar bir Azure depolama oluşturmak **SAS belirteci** depolama hesabına yönetilen örnek Denetim erişim hakları vermek için kullanılır.

   - Kapsayıcı önceki adımda oluşturduğunuz Azure depolama hesabına gidin.

   - Tıklayarak **paylaşılan erişim imzası** depolama ayarları menüsünde.

     ![Gezinti bölmesi][6]

   - SAS aşağıdaki gibi yapılandırın:
     - **İzin verilen hizmetler**: Blob
     - **Başlangıç tarihi**: saat dilimi ile ilgili sorunları önlemek için önceki günün tarihi kullanılması önerilir.
     - **Bitiş tarihi**: üzerinde bu SAS belirteci süre sonu tarihi seçin. 

       > [!NOTE]
       > Denetim hatalarını önlemek için son kullanma tarihinden sonra belirteci yenileyin.

     - **SAS Oluştur**’a tıklayın.

       ![Gezinti bölmesi][7]

   - SAS Oluştur tıklandıktan sonra SAS belirteci altında görüntülenir. Kopyala simgesine tıklayarak belirteci kopyalayın ve gelecekte kullanım için (örneğin, Not Defteri'nde) kaydedin.

     > [!IMPORTANT]
     > Soru işareti kaldırın ("?") belirteci başından itibaren karakter.

     ![Gezinti bölmesi][8]

4. SQL Server Management Studio'yu (SSMS) aracılığıyla yönetilen Örneğinize bağlanın.

5. Aşağıdaki T-SQL deyimi için yürütün **yeni kimlik bilgisi oluşturma** kapsayıcı URL'si ve SAS önceki adımlarda oluşturduğunuz belirteci kullanarak:

    ```SQL
    CREATE CREDENTIAL [<container_url>]
    WITH IDENTITY='SHARED ACCESS SIGNATURE',
    SECRET = '<SAS KEY>'
    GO
    ```

6. Yeni bir sunucu denetim oluşturmak için aşağıdaki T-SQL deyimi yürütme (kendi denetim adınızı seçin, önceki adımlarda oluşturduğunuz kapsayıcı URL'yi kullanın):

    ```SQL
    CREATE SERVER AUDIT [<your_audit_name>]
    TO URL ( PATH ='<container_url>' [, RETENTION_DAYS =  integer ])
    GO
    ```

    Belirtilmezse, `RETENTION_DAYS` 0 (sınırsız bekletme) varsayılandır.

    Ek bilgi için:
    - [Yönetilen örneği, Azure SQL veritabanı ve SQL Server arasındaki farklar denetleme](#subheading-3)
    - [SUNUCU DENETİMİ OLUŞTURMA](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)
    - [ALTER SERVER DENETİM](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)

7. SQL Server gibi sunucu denetimi belirtimi veya veritabanı denetim belirtimine oluşturun:
    - [Sunucu denetimi belirtimi T-SQL kılavuzu oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-specification-transact-sql)
    - [Veritabanı denetim belirtimine T-SQL kılavuzu oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-database-audit-specification-transact-sql)

8. 6. adımda oluşturduğunuz sunucu denetimi etkinleştir:

    ```SQL
    ALTER SERVER AUDIT [<your_audit_name>]
    WITH (STATE=ON);
    GO
    ```

## <a name="analyze-audit-logs"></a>Denetim günlüklerini çözümleme
Blob günlükleri denetleme görüntülemek için kullanabileceğiniz çeşitli yöntemler vardır.

- Sistem işlevi kullanın `sys.fn_get_audit_file` (Denetim günlüğü verileri tablo biçiminde döndürmek için T-SQL). Bu işlev kullanma hakkında daha fazla bilgi için bkz. [sys.fn_get_audit_file belgeleri](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).

- Denetim Günlüğü Tüketim yöntemi tam bir listesi için başvurmak [SQL veritabanı denetimini kullanmaya başlama](https://docs.microsoft.com/ azure/sql-database/sql-database-auditing).

> [!IMPORTANT]
> Denetim kayıtları (bölme 'denetim kayıtları') Azure portalından yöntemi yönetilen örneği için şu anda kullanılamıyor.

## <a name="auditing-differences-between-managed-instance-azure-sql-database-and-sql-server"></a>Yönetilen örnek, Azure SQL veritabanı ve SQL Server arasındaki farklar denetleme

SQL yönetilen örneği, Azure SQL veritabanı ve SQL Server şirket içi denetleme arasındaki temel farklılıklar şunlardır:

- Yönetilen örneği'nde sunucu düzeyinde ve depoları SQL denetim çalışır `.xel` günlük dosyalarını Azure blob depolama hesabı.
- Azure SQL veritabanı'nda SQL denetim veritabanı düzeyinde çalışır.
- Şirket içi SQL Server / sanal makineler, sunucuda SQL denetim çalışır düzeyi, dosya sistem/windows olaylarına depolar ancak olay günlükleri.

XEvent denetim yönetilen örneği'nde, Azure blob depolama hedeflerini destekler. Dosya ve windows günlükleri **desteklenmiyor**.

Anahtarının farklar içinde `CREATE AUDIT` denetleme için Azure blob depolama söz dizimi olan:
- Yeni bir söz dizimi `TO URL` sağlanır ve Azure blob depolama kapsayıcısının URL'sini belirtmenize olanak tanıyan burada `.xel` dosyalar yerleştirilir.
- Söz dizimi `TO FILE` olduğu **desteklenmiyor** çünkü yönetilen örneği, Windows dosya paylaşımlarına erişemez.
- Kapatma seçeneği **desteklenmiyor**.
- `queue_delay` 0'ın **desteklenmiyor**.


## <a name="next-steps"></a>Sonraki adımlar

- Denetim Günlüğü Tüketim yöntemi tam bir listesi için başvurmak [SQL veritabanı denetimini kullanmaya başlama](https://docs.microsoft.com/ azure/sql-database/sql-database-auditing).
- Azure hakkında daha fazla bilgi bu standartlara uyumluluk programları için bkz: [Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/compliance/).


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
