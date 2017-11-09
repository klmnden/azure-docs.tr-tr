---
title: "Azure SQL veri eşitleme (Önizleme) ile çalışmaya başlama | Microsoft Docs"
description: "Bu öğreticide Azure SQL veri eşitleme (Önizleme) ile çalışmaya başlamanıza yardımcı olur."
services: sql-database
documentationcenter: 
author: douglaslms
manager: craigg
editor: 
ms.assetid: a295a768-7ff2-4a86-a253-0090281c8efa
ms.service: sql-database
ms.custom: load & move data
ms.workload: Active
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: douglasl
ms.reviewer: douglasl
ms.openlocfilehash: ddcf6868a0fca88a52774e20623d25de31c063bb
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2017
---
# <a name="get-started-with-azure-sql-data-sync-preview"></a>Azure SQL veri eşitleme (Önizleme) ile çalışmaya başlama
Bu öğreticide, Azure SQL Database ve SQL Server örneklerini içeren bir karma eşitleme grubu oluşturarak Azure SQL veri eşitlemeyi ayarlamak nasıl öğrenin. Yeni eşitleme grubunu tam olarak yapılandırılmamış ve belirlediğiniz bir zamanlamaya göre eşitler.

Bu öğretici, SQL Database ve SQL Server ile en az bazı konusunda deneyim sahibi olduğunuzu varsayar. 

SQL veri eşitleme genel bakış için bkz: [verileri Eşitle birden çok Bulut ve şirket içi veritabanları arasında Azure SQL veri eşitleme (Önizleme) ile](sql-database-sync-data.md).

SQL veri eşitleme yapılandırmayı gösterir tam PowerShell örnekler için aşağıdaki makalelere bakın:
-   [Birden çok Azure SQL veritabanları arasında eşitlemek için PowerShell kullanma](scripts/sql-database-sync-data-between-sql-databases.md)
-   [Bir Azure SQL Database ve SQL Server içi veritabanı arasında eşitlemek için PowerShell kullanma](scripts/sql-database-sync-data-between-azure-onprem.md)

## <a name="step-1---create-sync-group"></a>1. adım - eşitleme grubu oluşturma

### <a name="locate-the-data-sync-settings"></a>Veri Eşitleme ayarlarını bulun

1.  Tarayıcınızda, Azure portalına gidin.

2.  Portalda, SQL veritabanlarınızın Panonuzda veya SQL veritabanları simgesi araç çubuğunda bulun.

    ![Azure SQL veritabanı listesi](media/sql-database-get-started-sql-data-sync/datasync-preview-sqldbs.png)

3.  Üzerinde **SQL veritabanları** sayfasında, veri eşitleme için hub veritabanı olarak kullanmak istediğiniz varolan bir SQL veritabanını seçin. SQL veritabanı sayfası açılır.

4.  Seçili veritabanı için SQL veritabanı sayfasında, seçin **diğer veritabanlarına eşitleme**. Veri Eşitleme sayfası açılır.

    ![Diğer veritabanlarını seçeneğine eşitleme](media/sql-database-get-started-sql-data-sync/datasync-preview-newsyncgroup.png)

### <a name="create-a-new-sync-group"></a>Yeni bir eşitleme grubu oluşturun

1.  Veri Eşitleme sayfasında seçin **yeni eşitleme grubu**. **Yeni eşitleme grubu** sayfası açılır adım 1 ile **eşitleme Grup Oluştur**, vurgulanan. **Veri eşitleme grubu oluşturma** sayfası da açılır.

2.  Üzerinde **veri eşitleme grubu oluşturma** sayfasında, şunları yapın:

    1.  İçinde **eşitleme grubu adı** alan, yeni eşitleme grubu için bir ad girin.

    2.  İçinde **eşitleme meta veri veritabanı** bölümünde, (önerilen) yeni bir veritabanı oluşturun veya varolan bir veritabanını kullanmak için seçin.

        > [!NOTE]
        > Microsoft Eşitleme meta veri veritabanı olarak kullanmak için yeni, boş bir veritabanı oluşturmak önerir. Veri Eşitleme bu veritabanında tablolar oluşturur ve sık kullanılan bir iş yükü çalıştırır. Bu veritabanı, tüm seçili bölgede eşitleme gruplarınızın için eşitleme meta veri veritabanı olarak otomatik olarak paylaşılır. Bırakma olmadan eşitleme meta verileri veritabanı veya adını değiştiremezsiniz.

        Seçerseniz **yeni veritabanı**seçin **yeni veritabanı oluştur.** **SQL veritabanı** sayfası açılır. Üzerinde **SQL veritabanı** sayfa, ad ve yeni veritabanını yapılandırın. Ardından **Tamam**.

        Seçerseniz **varolan veritabanını kullan**, veritabanını listeden seçin.

    3.  İçinde **otomatik eşitleme** bölümünde, ilk seçin **üzerinde** veya **devre dışı**.

        Seçerseniz **üzerinde**, **eşitleme sıklığı** bölümünde, bir sayı girin ve saniye, dakika, saat veya gün seçin.

        ![Eşitleme sıklığı belirtin](media/sql-database-get-started-sql-data-sync/datasync-preview-syncfreq.png)

    4.  İçinde **çakışma çözümü** bölümünde, "Hub wins" veya "Üye WINS." seçin

        ![Çakışmalar nasıl çözümlenir belirtin](media/sql-database-get-started-sql-data-sync/datasync-preview-conflictres.png)

    5.  Seçin **Tamam** ve oluşturulan ve dağıtılan için yeni eşitleme grubu bekleyin.

## <a name="step-2---add-sync-members"></a>2. adım - eşitleme üyeleri Ekle

Yeni eşitleme grubu oluşturup, adım 2 ' yi dağıttıktan sonra **eşitleme üye eklemek**, vurgulanan **yeni eşitleme grubu** sayfası.

İçinde **Hub veritabanı** bölümünde, hangi hub veritabanının bulunduğu SQL veritabanı sunucusu için varolan kimlik bilgilerini girin. Girmeyin *yeni* Bu bölümde kimlik bilgileri.

![Hub veritabanı grubunu eşitlemek için eklenen](media/sql-database-get-started-sql-data-sync/datasync-preview-hubadded.png)

### <a name="add-an-azure-sql-database"></a>Bir Azure SQL veritabanı Ekle

İçinde **üye veritabanı** bölümünde, isteğe bağlı olarak bir Azure SQL veritabanı seçerek eşitleme grubuna ekleyin **Azure veritabanı Ekle**. **Azure veritabanını yapılandırma** sayfası açılır.

Üzerinde **Azure veritabanını yapılandırma** sayfasında, şunları yapın:

1.  İçinde **eşitleme üye adı** alanında, yeni eşitleme üyesi için bir ad sağlayın. Bu ad, veritabanı adından farklıdır.

2.  İçinde **abonelik** alan, faturalandırma için ilişkili Azure aboneliğini seçin.

3.  İçinde **Azure SQL Server** alanında, varolan bir SQL veritabanı sunucusuna seçin.

4.  İçinde **Azure SQL veritabanı** alanında, varolan bir SQL veritabanını seçin.

5.  İçinde **eşitleme yönergeleri** alan, select çift yönlü eşitleme, için Hub veya gelen Hub.

    ![Yeni bir SQL veritabanı eşitleme üye ekleme](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadding.png)

6.  İçinde **kullanıcıadı** ve **parola** alanları üye veritabanının bulunduğu SQL veritabanı sunucusu için var olan kimlik bilgilerini girin. Girmeyin *yeni* Bu bölümde kimlik bilgileri.

7.  Seçin **Tamam** ve oluşturulan ve dağıtılan yeni eşitleme üyesi için bekleyin.

    ![Yeni SQL veritabanı eşitleme üye eklendi](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadded.png)

### <a name="add-an-on-premises-sql-server-database"></a>Bir şirket içi SQL Server veritabanı ekleyin

İçinde **üye veritabanı** bölümünde, isteğe bağlı olarak seçerek bir şirket içi SQL Server eşitleme grubuna ekleyin **bir şirket içi veritabanı Ekle**. **Yapılandırma şirket içi** sayfası açılır.

Üzerinde **yapılandırma şirket içi** sayfasında, şunları yapın:

1.  Seçin **eşitleme Aracısı ağ geçidi seçin**. **Seçin eşitleme Aracısı** sayfası açılır.

    ![Eşitleme Aracısı ağ geçidi seçin](media/sql-database-get-started-sql-data-sync/datasync-preview-choosegateway.png)

2.  Üzerinde **eşitleme Aracısı ağ geçidi seçin** sayfasında, var olan bir aracıyı kullanın veya yeni bir aracı oluşturmak isteyip istemediğinizi seçin.

    Seçerseniz **varolan aracıları**, var olan aracıyı listeden seçin.

    Seçerseniz **yeni bir aracı oluşturma**, şunları yapın:

    1.  İstemci eşitleme Aracısı yazılımı sağlanan bağlantısından yükleyin ve SQL Server bulunduğu bilgisayara yükleyin.
 
        > [!IMPORTANT]
        > Sunucuyla iletişim istemci Aracısı izin vermek için Güvenlik Duvarı'nda giden TCP bağlantı noktası 1433 açmanız gerekir.


    2.  Aracı için bir ad girin.

    3.  Seçin **oluşturun ve anahtarı oluşturmak**.

    4.  Aracı anahtarını panoya kopyalayın.
        
        ![Yeni bir eşitleme aracı oluşturma](media/sql-database-get-started-sql-data-sync/datasync-preview-selectsyncagent.png)

    5.  Seçin **Tamam** kapatmak için **seçin eşitleme Aracısı** sayfası.

    6.  SQL Server bilgisayarında, bulun ve istemci eşitleme Aracısı uygulamayı çalıştırın.

        ![İstemci Aracısı uygulama verilerini eşitleme](media/sql-database-get-started-sql-data-sync/datasync-preview-clientagent.png)

    7.  Eşitleme Aracısı uygulamayı seçin **aracı anahtarını Gönder**. **Eşitleme meta veri veritabanı yapılandırması** iletişim kutusu açılır.

    8.  İçinde **eşitleme meta veri veritabanı yapılandırması** iletişim kutusu, Azure portalından kopyalandığından aracı anahtarını yapıştırın. Ayrıca meta veri veritabanının bulunduğu için Azure SQL veritabanı sunucusu varolan kimlik bilgilerini sağlayın. (Yeni bir meta veri veritabanı oluşturduysanız, bu hub veritabanı ile aynı sunucuda veritabanıdır.) Seçin **Tamam** ve yapılandırmayı tamamlamak bekleyin.

        ![Aracı anahtarını ve sunucu kimlik bilgilerini girin](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-enterkey.png)

        >   [!NOTE] 
        >   Bu noktada bir güvenlik duvarı hata alırsanız, SQL Server bilgisayardan gelen trafiğe izin vermek için Azure üzerinde bir güvenlik duvarı kuralı oluşturmanız gerekir. Kural Portalı'nda el ile oluşturabilirsiniz, ancak SQL Server Management Studio (SSMS) oluşturmak daha kolay bulabilirsiniz. SSMS, Azure hub veritabanına bağlanmayı deneyin. Adı olarak girin \<hub_database_name\>. database.windows.net. Azure güvenlik duvarı kuralı yapılandırmak için iletişim kutusunda adımları izleyin. Ardından istemci eşitleme Aracısı uygulamaya geri dönün.

    9.  İstemci eşitleme Aracısı uygulamanın tıklayın **kaydetmek** aracı ile bir SQL Server veritabanına kaydetmek için. **SQL Server yapılandırma** iletişim kutusu açılır.

        ![Ekleme ve bir SQL Server veritabanı yapılandırma](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-adddb.png)

    10. İçinde **SQL Server yapılandırma** iletişim kutusu, SQL Server kimlik doğrulaması veya Windows kimlik doğrulaması kullanarak bağlanmak üzere seçim yapın. SQL Server kimlik doğrulamasını seçerseniz, var olan kimlik bilgilerini girin. SQL Server adı ve eşitlemek istediğiniz veritabanının adını belirtin. Seçin **Bağlantıyı Sına** ayarlarınızı sınamak için. Ardından **kaydetmek**. Kayıtlı veritabanı listede görüntülenir.

        ![SQL Server veritabanı artık kayıtlı](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-dbadded.png)

    11. İstemci eşitleme Aracısı uygulama artık kapatabilirsiniz.

    12. Portalda, üzerinde **yapılandırma şirket içi** sayfasında, **veritabanını seçin.** **Veritabanı Seç** sayfası açılır.

    13. Üzerinde **Veritabanı Seç** sayfasında **eşitleme üye adı** alanında, yeni eşitleme üyesi için bir ad sağlayın. Bu ad, veritabanı adından farklıdır. Veritabanını listeden seçin. İçinde **eşitleme yönergeleri** alan, select çift yönlü eşitleme, için Hub veya gelen Hub.

        ![Şirket içi veritabanını seçin](media/sql-database-get-started-sql-data-sync/datasync-preview-selectdb.png)

    14. Seçin **Tamam** kapatmak için **Veritabanı Seç** sayfası. Ardından **Tamam** kapatmak için **yapılandırma şirket içi** sayfasında ve oluşturulan ve dağıtılan yeni eşitleme üyesi için bekleyin. Son olarak, tıklatın **Tamam** kapatmak için **eşitleme üyeleri seçin** sayfası.

        ![Şirket içi veritabanında eşitleme grubuna eklendi](media/sql-database-get-started-sql-data-sync/datasync-preview-onpremadded.png)

3.  SQL veri eşitleme ve yerel aracı bağlanmak için kullanıcı adınızı role ekleyin `DataSync_Executor`. Veri Eşitleme SQL Server örneğinde bu rol oluşturur.

## <a name="step-3---configure-sync-group"></a>3. adım - eşitleme grubunu yapılandırma

Yeni eşitleme Grup üyeleri oluşturulan ve dağıtılan, adım 3, sonra **yapılandırma eşitleme grubu**, vurgulanan **yeni eşitleme grubu** sayfası.

1.  Üzerinde **tabloları** sayfasında, eşitleme Grup üyelerinin listesinden bir veritabanı seçin ve ardından **yenileme şema**.

2.  Kullanılabilir tabloların listeden eşitlemek istediğiniz tabloları seçin.

    ![Eşitlenecek tabloları seçin](media/sql-database-get-started-sql-data-sync/datasync-preview-tables.png)

3.  Varsayılan olarak, tablodaki tüm sütunların seçilir. Tüm sütunları eşitlemek istemiyorsanız eşitlemek istemediğiniz sütunlar için onay kutusunu devre dışı bırakın. Seçili birincil anahtar sütunu olduğundan emin olun.

    ![Eşitlemek için alanları seçin](media/sql-database-get-started-sql-data-sync/datasync-preview-tables2.png)

4.  Son olarak, seçin **kaydetmek**.

## <a name="next-steps"></a>Sonraki adımlar
Tebrikler. Bir SQL veritabanı örneğini ve SQL Server veritabanı içeren bir eşitleme grubu oluşturdunuz.

SQL veri eşitleme hakkında daha fazla bilgi için bkz:

-   [Eşitleme verilerle birden çok Bulut ve şirket içi veritabanları arasında Azure SQL veri eşitleme](sql-database-sync-data.md)
-   [Azure SQL veri eşitleme için en iyi yöntemler](sql-database-best-practices-data-sync.md)
-   [Azure SQL veri eşitleme ile ilgili sorunları giderme](sql-database-troubleshoot-data-sync.md)

-   SQL veri eşitleme yapılandırmayı gösterir PowerShell örnekleri tamamlayın:
    -   [Birden çok Azure SQL veritabanları arasında eşitlemek için PowerShell kullanma](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [Bir Azure SQL Database ve SQL Server içi veritabanı arasında eşitlemek için PowerShell kullanma](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [SQL veri eşitleme REST API belgelerini indirebilirsiniz](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

SQL veritabanı hakkında daha fazla bilgi için bkz:

-   [SQL veritabanı genel bakış](sql-database-technical-overview.md)
-   [Veritabanı yaşam döngüsü yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
