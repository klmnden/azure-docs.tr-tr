---
title: Azure SQL veritabanı yönetilen örneği denetim | Microsoft Docs
description: T-SQL kullanarak örneği yönetilen bir Azure SQL veritabanı denetimini kullanmaya başlama hakkında bilgi edinin
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
ms.date: 04/08/2019
ms.openlocfilehash: 6ada2a5e505bfe37f4f9a956570d8b6f38f55e55
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60702937"
---
# <a name="get-started-with-azure-sql-database-managed-instance-auditing"></a>Azure SQL veritabanı yönetilen örneği denetimini kullanmaya başlama

[Yönetilen örnek](sql-database-managed-instance.md) denetimi veritabanı olaylarını izler ve Azure depolama hesabınızdaki Denetim günlüğüne yazar. Ayrıca denetleme:

- Mevzuatla uyumluluk, veritabanı etkinliğini anlama ve tutarsızlıklar ve işletme sorunlarını veya şüpheli güvenlik ihlallerini anomalileri kavramanıza yardımcı olur.
- Etkinleştirir ve uyumluluk garanti etmez ancak uyumluluk standardını da kıldığı kolaylaştırır. Azure hakkında daha fazla bilgi bu standartlara uyumluluk programları için bkz: [Azure Güven Merkezi](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) burada bulabilirsiniz SQL veritabanı uyumluluk sertifikaları en güncel listesi.

## <a name="set-up-auditing-for-your-server-to-azure-storage"></a>Azure depolama sunucunuz için denetimi ayarlamanız

Aşağıdaki bölümde, yönetilen Örneğinize denetim yapılandırma açıklanmaktadır.

1. [Azure Portal](https://portal.azure.com) gidin.
1. Bir Azure depolama oluşturma **kapsayıcı** denetim günlükleri nerede depolanır.

   1. Azure denetim günlüklerinizi depolamak için istediğiniz depoya gidin.

      > [!IMPORTANT]
      > Bölgeler arası okumayı/yazmayı önlemek için yönetilen örnek ile aynı bölgede bir depolama hesabı kullanın.

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

     1. Bir Azure depolama alanı oluşturma **SAS belirteci** yönetilen örnek denetim depolama hesabına erişim hakları vermek için:

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

        ![Azure aboneliği, depolama hesabı ve blob kapsayıcısı seçin](./media/sql-managed-instance-auditing/13_mi_SSMS_select_subscription_account_container.png)

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

- [Azure SQL veritabanı ve SQL Server'da veritabanlarını tek veritabanları, elastik havuz, s ve yönetilen örnekleri arasındaki farklar denetleme](#auditing-differences-between-databases-in-azure-sql-database-and-databases-in-sql-server)
- [SUNUCU DENETİMİ OLUŞTURMA](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)
- [ALTER SERVER DENETİM](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)

## <a name="set-up-auditing-for-your-server-to-event-hub-or-azure-monitor-logs"></a>Olay hub'ı veya Azure İzleyici günlüklerine sunucunuz için denetimi ayarlamanız

Yönetilen örnek denetim günlüklerinden bile hub veya Azure İzleyici günlüklerine gönderilebilir. Bu bölümde, bunun nasıl yapılandırıldığı açıklanmaktadır:

1. Gezinme [Azure portalı](https://portal.azure.com/) yönetilen örnek için.

2. Tıklayarak **tanılama ayarları**.

3. Tıklayarak **tanılamayı Aç**. Tanılama zaten etkin değilse *+ tanılama ayarı ekleme* yerine gösterilir.

4. Seçin **SQLSecurityAuditEvents** günlükleri listesinde.

5. Denetim olaylar - olay hub'ı, Azure İzleyici günlüklerine veya her ikisi için bir hedef seçin. Her hedef için gerekli parametreler (örneğin Log Analytics çalışma alanı) yapılandırın.

6. **Kaydet**’e tıklayın.

    ![Tanılama ayarlarını yapılandırma](./media/sql-managed-instance-auditing/9_mi_configure_diagnostics.png)

7. Yönetilen örnek kullanarak bağlanmak **SQL Server Management Studio (SSMS)** veya desteklenen herhangi bir istemci.

8. Sunucu denetimi oluşturmak için aşağıdaki T-SQL deyimi çalıştırın:

    ```SQL
    CREATE SERVER AUDIT [<your_audit_name>] TO EXTERNAL_MONITOR;
    GO
    ```

9. SQL Server gibi sunucu denetimi belirtimi veya veritabanı denetim belirtimine oluşturun:

   - [Sunucu denetimi belirtimi T-SQL kılavuzu oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-specification-transact-sql)
   - [Veritabanı denetim belirtimine T-SQL kılavuzu oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-database-audit-specification-transact-sql)

10. 8. adımda oluşturduğunuz sunucu denetimi etkinleştir:
 
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

### <a name="consume-logs-stored-in-event-hub"></a>Olay Hub'ında depolanan günlüklerini kullanma

Denetim günlükleri verileri olay hub'ı kullanmak için olayları kullanma ve bir hedef yazmak için bir akış ayarlamanız gerekir. Daha fazla bilgi için Azure Event Hubs belgeleri bakın.

### <a name="consume-and-analyze-logs-stored-in-azure-monitor-logs"></a>Azure İzleyici günlüklerine depolanan günlüklerini analiz etme ve kullanma

Denetim günlükleri, Azure İzleyici günlüklerine yazılır, bunlar burada denetim veriler üzerinde Gelişmiş aramaları çalıştırabilirsiniz. Log Analytics çalışma alanında kullanılabilir. Bir başlangıç noktası olarak altında ve Log Analytics çalışma alanına gidin *genel* bölümünde *günlükleri* ve basit bir sorgu girin: `search "SQLSecurityAuditEvents"` denetim görüntülemek üzere günlüğe kaydeder.  

Azure İzleyici günlüklerine tüm iş yüklerinizde ve sunucularınızda milyonlarca kaydı kolayca analiz etmek için tümleşik arama ve özel panoları kullanarak gerçek zamanlı operasyonel içgörüler sunar. Azure İzleyici günlüklerine arama dili ve komutlar hakkında başka yararlı bilgiler için bkz. [Azure İzleyici günlüklerini Arama başvurusu](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="auditing-differences-between-databases-in-azure-sql-database-and-databases-in-sql-server"></a>Azure SQL veritabanında veritabanlarını ve SQL Server veritabanları arasındaki farklar denetleme

Azure SQL veritabanı ve SQL Server'da veritabanlarını veritabanlarında denetimi arasındaki temel farklılıklar şunlardır:

- Azure SQL veritabanı yönetilen örnek dağıtım seçeneği ile works sunucu düzeyinde ve depoları denetim `.xel` günlük dosyalarını Azure Blob Depolama alanında.
- Tek veritabanı ve elastik havuz dağıtım seçeneklerinde Azure SQL veritabanı'nda çalışır ve veritabanı düzeyinde denetim.
- Şirket içi SQL Server / sanal makineler, sunucuda denetim works düzey, ancak dosyaları sistem/windows olay günlüklerini olayları depolar.

XEvent yönetilen örneğinde denetimi, Azure Blob Depolama hedeflerini destekler. Dosya ve windows günlükleri **desteklenmiyor**.

Anahtarının farklar içinde `CREATE AUDIT` Azure Blob depolama alanına denetim söz dizimi şunlardır:

- Yeni bir söz dizimi `TO URL` sağlanır ve Azure blob depolama kapsayıcısının URL'sini belirtmenize olanak tanıyan burada `.xel` dosyalar yerleştirilir.
- Yeni bir söz dizimi `TO EXTERNAL MONITOR` bile Hub ve Azure İzleyici günlüklerine hedefleri etkinleştirmek için sağlanır.
- Söz dizimi `TO FILE` olduğu **desteklenmiyor** olduğundan SQL veritabanı, Windows dosya paylaşımlarına erişemez.
- Kapatma seçeneği **desteklenmiyor**.
- `queue_delay` 0'ın **desteklenmiyor**.

## <a name="next-steps"></a>Sonraki adımlar

- Denetim Günlüğü Tüketim yöntemi tam bir listesi için başvurmak [SQL veritabanı denetimini kullanmaya başlama](sql-database-auditing.md).
- Azure hakkında daha fazla bilgi bu standartlara uyumluluk programları için bkz: [Azure Güven Merkezi](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) burada bulabilirsiniz SQL veritabanı uyumluluk sertifikaları en güncel listesi.

<!--Image references-->









