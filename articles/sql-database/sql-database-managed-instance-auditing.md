---
title: Azure SQL veritabanı yönetilen örneği denetim | Microsoft Docs
description: Azure SQL veritabanı yönetilen örnek T-SQL kullanarak denetimini kullanmaya başlama hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
f1_keywords:
- mi.azure.sqlaudit.general.f1
author: vainolo
ms.author: arib
ms.reviewer: vanto
manager: craigg
ms.date: 01/15/2019
ms.openlocfilehash: 3a445fbc135e0d7dc19907339506fd0c32bffb45
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55456043"
---
# <a name="get-started-with-azure-sql-database-managed-instance-auditing"></a>Azure SQL veritabanı yönetilen örneği denetimini kullanmaya başlama

[Azure SQL veritabanı yönetilen örneği](sql-database-managed-instance.md) denetimi veritabanı olaylarını izler ve bir denetim günlüğüne Azure depolama hesabınızdaki yazar. Ayrıca denetleme:

- Mevzuatla uyumluluk, veritabanı etkinliğini anlama ve tutarsızlıklar ve işletme sorunlarını veya şüpheli güvenlik ihlallerini anomalileri kavramanıza yardımcı olur.
- Etkinleştirir ve uyumluluk garanti etmez ancak uyumluluk standardını da kıldığı kolaylaştırır. Azure hakkında daha fazla bilgi bu standartlara uyumluluk programları için bkz: [Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/compliance/).

## <a name="set-up-auditing-for-your-server-to-azure-storage"></a>Azure depolama sunucunuza için denetimi ayarlamanız

Aşağıdaki bölümde, yönetilen Örneğinize denetim yapılandırma açıklanmaktadır.

1. [Azure Portal](https://portal.azure.com) gidin.
1. Bir Azure depolama oluşturma **kapsayıcı** denetim günlükleri nerede depolanır.

   1. Azure denetim günlüklerinizi depolamak için istediğiniz depoya gidin.

      > [!IMPORTANT]
      > Bölgeler arası okumayı/yazmayı önlemek için yönetilen örnek sunucusu ile aynı bölgede bir depolama hesabı kullanın.

   1. Depolama hesabında Git **genel bakış** tıklatıp **Blobları**.

      ![Azure Blob pencere öğesi](./media/sql-managed-instance-auditing/1_blobs_widget.png)

   1. Üst menüde **+ kapsayıcı** yeni bir kapsayıcı oluşturmak için.

      ![BLOB kapsayıcı simge oluşturma](./media/sql-managed-instance-auditing/2_create_container_button.png)

   1. Bir kapsayıcı **adı**, genel erişim düzeyini ayarlayın **özel**ve ardından **Tamam**.

     ![BLOB kapsayıcı yapılandırması oluşturma](./media/sql-managed-instance-auditing/3_create_container_config.png)

1. Denetim için kapsayıcıyı oluşturduktan sonra günlükleri vardır denetim günlükleri için hedef olarak yapılandırmak için kullanabileceğiniz iki yöntemdir: [T-SQL kullanarak](#blobtsql) veya [SQL Server Management Studio (SSMS) kullanıcı arabirimini kullanarak](#blobssms):

   - <a id="blobtsql"></a>Blog depolama için denetim günlüklerini T-SQL kullanarak yapılandırın:

     1. Kapsayıcılar listesinde yeni oluşturulan kapsayıcı tıklayın ve ardından **kapsayıcı özellikleri**.

        ![BLOB kapsayıcı Özellikler düğmesi](./media/sql-managed-instance-auditing/4_container_properties_button.png)

     1. Kopyala simgesine tıklayarak kapsayıcı URL'sini kopyalayın ve kaydedin (örneğin, Not Defteri'nde) gelecekte kullanım için URL. Kapsayıcı URL biçiminde olmalıdır `https://<StorageName>.blob.core.windows.net/<ContainerName>`

        ![BLOB kapsayıcı kopya URL'si](./media/sql-managed-instance-auditing/5_container_copy_name.png)

     1. Bir Azure depolama alanı oluşturma **SAS belirteci** yönetilen örnek denetim, depolama hesabına erişim hakları vermek için:

        - Kapsayıcı önceki adımda oluşturduğunuz Azure depolama hesabına gidin.

        - Tıklayarak **paylaşılan erişim imzası** depolama ayarları menüsünde.

          ![Paylaşılan erişim imzası simgesi depolama ayarları menüsünde](./media/sql-managed-instance-auditing/6_storage_settings_menu.png)

        - SAS aşağıdaki gibi yapılandırın:

          - **İzin verilen hizmetler**: Blob

          - **Başlangıç tarihi**: saat dilimi ile ilgili sorunları önlemek için önceki günün tarihi kullanılması önerilir

          - **Bitiş tarihi**: üzerinde bu SAS belirteci süre sonu tarihi seçin

            > [!NOTE]
            > Denetim hatalarını önlemek için son kullanma tarihinden sonra belirteci yenileyin.

          - **SAS Oluştur**’a tıklayın.
            
            ![SAS yapılandırma](./media/sql-managed-instance-auditing/7_sas_configure.png)

        - SAS Oluştur tıklandıktan sonra SAS belirteci altında görüntülenir. Kopyala simgesine tıklayarak belirteci kopyalayın ve gelecekte kullanım için (örneğin, Not Defteri'nde) kaydedin.

          ![SAS belirteci kopyalayın](./media/sql-managed-instance-auditing/8_sas_copy.png)

          > [!IMPORTANT]
          > Soru işareti kaldırın ("?") belirteci başından itibaren karakter.

     1. SQL Server Management Studio (SSMS) veya herhangi bir desteklenen aracı üzerinden yönetilen Örneğinize bağlanın.

     1. Aşağıdaki T-SQL deyimi için yürütün **yeni kimlik bilgisi oluşturma** kapsayıcı URL'si ve SAS önceki adımlarda oluşturduğunuz belirteci kullanarak:

        ```SQL
        CREATE CREDENTIAL [<container_url>]
        WITH IDENTITY='SHARED ACCESS SIGNATURE',
        SECRET = '<SAS KEY>'
        GO
        ```

     1. Yeni bir sunucu denetim oluşturmak için aşağıdaki T-SQL deyimi yürütme (kendi denetim adınızı seçin, önceki adımlarda oluşturduğunuz kapsayıcı URL'yi kullanın). Belirtilmezse, `RETENTION_DAYS` varsayılan: 0 (sınırsız bekletme):

        ```SQL
        CREATE SERVER AUDIT [<your_audit_name>]
        TO URL ( PATH ='<container_url>' [, RETENTION_DAYS =  integer ])
        GO
        ```

      1. Tarafından devam [Server Audıt SPECIFICATION veya veritabanı denetim belirtimine oluşturma](#createspec)

   - <a id="blobssms"></a>BLOB Depolama alanı için denetim günlüklerini SQL Server Management Studio (SSMS) 18'e (Önizleme) kullanarak yapılandırın:

     1. SQL Server Management Studio (SSMS) kullanıcı arabirimini kullanarak yönetilen örneğe bağlanın.

     1. Nesne Gezgini kök Not genişletin.

     1. Genişletin **güvenlik** düğümünü sağ tıklatın **denetimleri** düğüm ve "Yeni denetim" tıklayın:

        ![Güvenlik'i genişletin ve denetim düğümü](./media/sql-managed-instance-auditing/10_mi_SSMS_new_audit.png)

     1. Emin "URL" seçildiğinde, yapma **denetim hedef** tıklayın **Gözat**:

        ![Azure depolamaya Gözat](./media/sql-managed-instance-auditing/11_mi_SSMS_audit_browse.png)

     1. (İsteğe bağlı) Azure hesabınızda oturum açın:

        ![Azure'da oturum açma](./media/sql-managed-instance-auditing/12_mi_SSMS_sign_in_to_azure.png)

     1. Bir abonelik, depolama hesabı ve Blob kapsayıcısı, açılan menüden seçin ya da tıklayarak kendi kapsayıcınızı oluşturun **Oluştur**. İşlemi tamamladıktan sonra tıklayın **Tamam**:

        ![Azure aboneliği, depolama hesabı ve blobl kapsayıcı seçin](./media/sql-managed-instance-auditing/13_mi_SSMS_select_subscription_account_container.png)

     1. Tıklayın **Tamam** "Denetim oluştur" iletişim kutusunda.

1. <a id="createspec"></a>Yapılandırdıktan sonra Denetim günlükleri için hedef olarak Blob kapsayıcısı oluşturursunuz sunucu denetim belirtimi veya veritabanı denetim belirtimine için SQL Server gibi:

   - [Sunucu denetimi belirtimi T-SQL kılavuzu oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-specification-transact-sql)
   - [Veritabanı denetim belirtimine T-SQL kılavuzu oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-database-audit-specification-transact-sql)

1. 6. adımda oluşturduğunuz sunucu denetimi etkinleştir:

    ```SQL
    ALTER SERVER AUDIT [<your_audit_name>]
    WITH (STATE=ON);
    GO
    ```

Ek bilgi için:

- [Yönetilen örneği, Azure SQL veritabanı ve SQL Server arasındaki farklar denetleme](#auditing-differences-between-managed-instance-azure-sql-database-and-sql-server)
- [SUNUCU DENETİMİ OLUŞTURMA](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)
- [ALTER SERVER DENETİM](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)

## <a name="set-up-auditing-for-your-server-to-event-hub-or-log-analytics"></a>Olay hub'ı veya Log Analytics sunucunuz için denetimi ayarlamanız

Yönetilen örneğe denetim günlüklerinden bile hub veya Azure İzleyicisi'ni kullanarak Log Analytics'e gönderilir. Bu bölümde, bunun nasıl yapılandırıldığı açıklanmaktadır:

1. Gezinme [Azure portalında](https://portal.azure.com/) SQL yönetilen örneği.

2. Tıklayarak **tanılama ayarları**.

3. Tıklayarak **tanılamayı Aç**. Tanılama zaten etkin değilse *+ tanılama ayarı ekleme* yerine gösterilir.

4. Seçin **SQLSecurityAuditEvents** günlükleri listesinde.

5. Denetim olaylar - olay hub'ı, Log Analytics veya her ikisi için bir hedef seçin. Her hedef için gerekli parametreler (örneğin Log Analytics çalışma alanı) yapılandırın.

6. **Kaydet**’e tıklayın.

    ![Tanılama ayarlarını yapılandırma](./media/sql-managed-instance-auditing/9_mi_configure_diagnostics.png)

7. Kullanarak yönetilen örneğe bağlanma **SQL Server Management Studio (SSMS)** veya desteklenen herhangi bir istemci.

8. Sunucu denetimi oluşturmak için aşağıdaki T-SQL deyimi çalıştırın:

    ```SQL
    CREATE SERVER AUDIT [<your_audit_name>] TO EXTERNAL_MONITOR;
    GO
    ```

9. SQL Server gibi sunucu denetimi belirtimi veya veritabanı denetim belirtimine oluşturun:

   - [Sunucu denetimi belirtimi T-SQL kılavuzu oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-specification-transact-sql)
   - [Veritabanı denetim belirtimine T-SQL kılavuzu oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-database-audit-specification-transact-sql)

10. 7. adımda oluşturduğunuz sunucu denetimi etkinleştir:
 
    ```SQL
    ALTER SERVER AUDIT [<your_audit_name>] WITH (STATE=ON);
    GO
    ```

## <a name="consume-audit-logs"></a>Denetim günlüklerini kullanma

### <a name="consume-logs-stored-in-azure-storage"></a>Azure Depolama'da depolanan günlüklerini kullanma

Blob günlükleri denetleme görüntülemek için kullanabileceğiniz çeşitli yöntemler vardır.

- Sistem işlevi kullanın `sys.fn_get_audit_file` (Denetim günlüğü verileri tablo biçiminde döndürmek için T-SQL). Bu işlev kullanma hakkında daha fazla bilgi için bkz. [sys.fn_get_audit_file belgeleri](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).

- Denetim günlükleri gibi bir araç kullanarak keşfedebilirsiniz [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/). Azure depolama alanında, Denetim günlükleri, denetim günlüklerini depolamak için tanımlanmış olan bir kapsayıcıdaki blob dosyaları koleksiyonu olarak kaydedilir. Depolama klasörü hiyerarşisi hakkında daha fazla ayrıntı için bkz: adlandırma kuralları ve günlük biçimi, [Blob denetim günlük biçimi başvurusu](https://go.microsoft.com/fwlink/?linkid=829599).

- Denetim Günlüğü Tüketim yöntemi tam bir listesi için başvurmak [SQL veritabanı denetimini kullanmaya başlama](sql-database-auditing.md).

  > [!IMPORTANT]
  > Denetim kayıtları Azure portalından ('denetim kayıtları' bölme), yönetilen örneği için şu anda kullanılamıyor.

### <a name="consume-logs-stored-in-event-hub"></a>Olay Hub'ında depolanan günlüklerini kullanma

Denetim günlükleri verileri olay hub'ı kullanmak için olayları kullanma ve bir hedef yazmak için bir akış ayarlamanız gerekir. Daha fazla bilgi için Azure Event Hubs belgeleri bakın.

### <a name="consume-and-analyze-logs-stored-in-log-analytics"></a>Kullanma ve Log Analytics içinde depolanan günlüklerini çözümleme

Denetim günlüklerini Log Analytics'e yazılır, bunlar burada denetim veriler üzerinde Gelişmiş aramaları çalıştırabilirsiniz. Log Analytics çalışma alanında kullanılabilir. Log Analytics ve altında bir başlangıç noktası olarak gidin *genel* bölümünde *günlükleri* ve basit bir sorgu girin: `search "SQLSecurityAuditEvents"` denetim görüntülemek üzere günlüğe kaydeder.  

Log Analytics, tüm iş yüklerinizde ve sunucularınızda milyonlarca kaydı kolayca analiz etmek için tümleşik arama ve özel panoları kullanarak gerçek zamanlı operasyonel içgörüler sunar. Log Analytics arama dili ve komutlar hakkında başka yararlı bilgiler için bkz. [Log Analytics Arama başvurusu](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).

## <a name="auditing-differences-between-managed-instance-azure-sql-database-and-sql-server"></a>Yönetilen örnek, Azure SQL veritabanı ve SQL Server arasındaki farklar denetleme

SQL yönetilen örneği, Azure SQL veritabanı ve SQL Server şirket içi denetleme arasındaki temel farklılıklar şunlardır:

- Yönetilen örneği'nde sunucu düzeyinde ve depoları SQL denetim çalışır `.xel` günlük dosyalarını Azure blob depolama hesabı.
- Azure SQL veritabanı'nda SQL denetim veritabanı düzeyinde çalışır.
- Şirket içi SQL Server / sanal makineler, sunucuda SQL denetim çalışır düzeyi, dosya sistem/windows olaylarına depolar ancak olay günlükleri.

XEvent denetim yönetilen örneği'nde, Azure blob depolama hedeflerini destekler. Dosya ve windows günlükleri **desteklenmiyor**.

Anahtarının farklar içinde `CREATE AUDIT` denetleme için Azure blob depolama söz dizimi olan:

- Yeni bir söz dizimi `TO URL` sağlanır ve Azure blob depolama kapsayıcısının URL'sini belirtmenize olanak tanıyan burada `.xel` dosyalar yerleştirilir.
- Yeni bir söz dizimi `TO EXTERNAL MONITOR` bile Hub ve Log Analytics hedefleri etkinleştirmek için sağlanır.
- Söz dizimi `TO FILE` olduğu **desteklenmiyor** çünkü yönetilen örneği, Windows dosya paylaşımlarına erişemez.
- Kapatma seçeneği **desteklenmiyor**.
- `queue_delay` 0'ın **desteklenmiyor**.

## <a name="next-steps"></a>Sonraki adımlar

- Denetim Günlüğü Tüketim yöntemi tam bir listesi için başvurmak [SQL veritabanı denetimini kullanmaya başlama](sql-database-auditing.md).
- Azure hakkında daha fazla bilgi bu standartlara uyumluluk programları için bkz: [Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/compliance/).

<!--Image references-->









