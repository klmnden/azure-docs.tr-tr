---
title: Azure SQL Data Sync'i Ayarla | Microsoft Docs
description: Bu öğreticide Azure SQL Data Sync'i ayarlama işlemi gösterilmektedir
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: douglasl
manager: craigg
ms.date: 11/07/2018
ms.openlocfilehash: 9175ed0b4f362a40e1d29a20a8378854b5f4cc81
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53310395"
---
# <a name="tutorial-set-up-sql-data-sync-to-sync-data-between-azure-sql-database-and-sql-server-on-premises"></a>Öğretici: Azure SQL veritabanı ve SQL Server arasında verileri eşitlemek amacıyla şirket içi SQL Data Sync'i Ayarla

Bu öğreticide, hem Azure SQL veritabanı ve SQL Server örneklerini içeren bir karma eşitleme grubu oluşturarak Azure SQL Data Sync'i ayarlama konusunda bilgi edinin. Yeni eşitleme grubu tam olarak yapılandırılmış ve ayarladığınız zamanlamaya göre eşitler.

Bu öğreticide, SQL veritabanı ve SQL Server ile konusunda en azından biraz deneyim sahibi olduğunuzu varsayar.

SQL Data Sync hizmetine genel bakış için bkz. [Azure SQL Data Sync ile birden fazla bulut ve şirket içi veritabanı arasında veri eşitleme](sql-database-sync-data.md).

SQL Data Sync'in nasıl yapılandırılacağını gösteren tam PowerShell örnekleri için aşağıdaki makalelere bakın:

- [PowerShell kullanarak birden çok Azure SQL veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-sql-databases.md)
- [PowerShell kullanarak bir Azure SQL Veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md)

## <a name="step-1---create-sync-group"></a>1. adım - eşitleme grubu oluşturma

### <a name="locate-the-data-sync-settings"></a>Veri Eşitleme ayarlarını bulma

1. Tarayıcınızda, Azure portalına gidin.

2. Portalda SQL veritabanlarınızı Panonuzda veya SQL veritabanları simgesine araç bulun.

    ![Azure SQL veritabanları listesi](media/sql-database-get-started-sql-data-sync/datasync-preview-sqldbs.png)

3. Üzerinde **SQL veritabanları** sayfasında, veri eşitleme için hub veritabanı kullanmak istediğiniz mevcut bir SQL veritabanını seçin. SQL veritabanı sayfası açılır.

    Hub veritabanı eşitleme grubu birden çok veritabanı uç noktaları olan eşitleme topolojinin merkezi uç noktadır. Diğer tüm veritabanı uç hub veritabanı ile aynı eşitleme grubu - diğer bir deyişle, tüm üye veritabanları - eşitleme.

4. Seçili veritabanı için SQL veritabanı sayfasında, seçin **diğer veritabanlarıyla Eşitle**. Veri Eşitleme sayfası açılır.

    ![Diğer veritabanlarını seçeneğine eşitleme](media/sql-database-get-started-sql-data-sync/datasync-preview-newsyncgroup.png)

### <a name="create-a-new-sync-group"></a>Yeni bir eşitleme grubu oluşturma

1. Veri Eşitleme sayfasında **yeni eşitleme grubu**. **Yeni eşitleme grubu** sayfasını açar, adım 1 **eşitleme grubu oluşturma**, vurgulanan. **Veri eşitleme grubu oluşturma** sayfası da açılır.

2. Üzerinde **veri eşitleme grubu oluşturma** sayfasında, şunları yapın:

   1. İçinde **eşitleme grubu adı** yeni eşitleme grubu için bir ad girin.

   2. İçinde **eşitleme meta verileri veritabanı** bölümünde, (önerilen) yeni bir veritabanı oluşturmak mı, yoksa mevcut bir veritabanını kullanmayı seçin.

        > [!NOTE]
        > Microsoft Eşitleme meta verileri veritabanı olarak kullanmak için yeni, boş bir veritabanı oluşturma önerir. Veri eşitleme, bu veritabanında tablolar oluşturur ve sık kullanılan bir iş yükü çalıştırır. Bu veritabanı, seçili bölgesinde eşitleme gruplarınızın tümü için eşitleme meta verileri veritabanı olarak otomatik olarak paylaşılır. Eşitleme grupları ve eşitleme aracıları bölgede kaldırmadan eşitleme meta verileri veritabanı veya adını değiştiremezsiniz.

        Seçerseniz, **yeni veritabanı**seçin **yeni veritabanı oluştur.** **SQL veritabanı** sayfası açılır. Üzerinde **SQL veritabanı** sayfasında ad ve yeni veritabanını yapılandırın. Sonra **Tamam**’ı seçin.

        Seçerseniz, **varolan veritabanını kullan**, veritabanını listeden seçin.

   3. İçinde **otomatik eşitleme** bölümünde, ilk seçin **üzerinde** veya **kapalı**.

        Seçerseniz, **üzerinde**, **eşitleme sıklığı** bölümünde, bir sayı girin ve saniye, dakika, saat veya gün seçin.

        ![Eşitleme sıklığı belirtin](media/sql-database-get-started-sql-data-sync/datasync-preview-syncfreq.png)

   4. İçinde **çakışma** bölümünde, "Hub wins" veya "Üyesi WINS" seçin

        "Hub wins" bir çakışma meydana geldiğinde, hub veritabanındaki verilerle çakışan üye veritabanı verilerini yazıldığını anlamına gelir. "Üye WINS" bir çakışma oluştuğunda, üye veritabanındaki verilerle çakışan hub veritabanındaki verilerle yazıldığını anlamına gelir.

       ![Çakışmalarını belirtin](media/sql-database-get-started-sql-data-sync/datasync-preview-conflictres.png)

   5. Seçin **Tamam** oluşturulup dağıtıldığında için yeni eşitleme grubu bekleyin.

## <a name="step-2---add-sync-members"></a>2. adım - eşitleme üyesi ekleme

Yeni eşitleme grubu oluşturup, adım 2 ' yi dağıttıktan sonra **eşitleme üyesi ekleme**, vurgulanan **yeni eşitleme grubu** sayfası.

İçinde **Hub veritabanı** bölümünde, hub veritabanının bulunduğu SQL veritabanı sunucusu için var olan kimlik bilgilerini girin. Girmeyin *yeni* Bu bölümde kimlik bilgileri.

![Hub veritabanı eşitleme grubuna eklendi.](media/sql-database-get-started-sql-data-sync/datasync-preview-hubadded.png)

### <a name="add-an-azure-sql-database"></a>Bir Azure SQL veritabanı ekleyin

İçinde **üye veritabanı** bölümüne, isteğe bağlı olarak seçerek Azure SQL veritabanı eşitleme grubuna ekleyin **Azure veritabanı ekleme**. **Azure veritabanını Yapılandır** sayfası açılır.

Üzerinde **Azure veritabanını Yapılandır** sayfasında, şunları yapın:

1. İçinde **eşitleme üyesi adı** alan, yeni eşitleme üyesi için bir ad sağlayın. Bu ad, veritabanı adından farklıdır.

2. İçinde **abonelik** alan, faturalama amacıyla ilişkili Azure aboneliği seçin.

3. İçinde **Azure SQL Server** alanında, var olan SQL veritabanı sunucusu seçin.

4. İçinde **Azure SQL veritabanı** alanında, mevcut bir SQL veritabanını seçin.

5. İçinde **eşitleme yönergeleri** alan, select iki yönlü eşitleme, için Hub veya gelen Hub.

    ![Yeni bir SQL veritabanı eşitleme üyesi ekleme](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadding.png)

6. İçinde **kullanıcıadı** ve **parola** alanları üye veritabanının bulunduğu SQL veritabanı sunucusu için var olan kimlik bilgilerini girin. Girmeyin *yeni* Bu bölümde kimlik bilgileri.

7. Seçin **Tamam** oluşturulup dağıtıldığında yeni eşitleme üyesi için bekleyin.

    ![Yeni SQL veritabanı eşitleme üyesi eklenmiştir.](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadded.png)

### <a name="add-on-prem"></a> Bir şirket içi SQL Server veritabanı ekleyin

İçinde **üye veritabanı** bölümüne, isteğe bağlı olarak şirket içi SQL Server seçerek eşitleme grubuna ekleyin **bir şirket içi veritabanı Ekle**. **Yapılandırma şirket içi** sayfası açılır.

Üzerinde **yapılandırma şirket içi** sayfasında, şunları yapın:

1. Seçin **eşitleme Aracısı ağ geçidini seçin**. **Eşitleme Aracısı seçin** sayfası açılır.

    ![Eşitleme Aracısı ağ geçidini seçin](media/sql-database-get-started-sql-data-sync/datasync-preview-choosegateway.png)

2. Üzerinde **eşitleme Aracısı ağ geçidini seçin** sayfasında, yeni bir aracı oluşturun veya var olan bir aracı kullanmak isteyip istemediğinizi seçin.

    Seçerseniz, **varolan aracıları**, var olan aracıyı listeden seçin.

    Seçerseniz, **yeni bir aracı oluşturun**, şunları yapın:

   1. Sağlanan bağlantıdan istemci eşitleme Aracısı yazılımını indirin ve SQL Server bulunduğu bilgisayara yükleyin. Veri Eşitleme Aracısı'ndan doğrudan da indirebilirsiniz [SQL Azure veri eşitleme Aracısı](https://www.microsoft.com/download/details.aspx?id=27693).

        > [!IMPORTANT]
        > Sunucuyla iletişim istemci Aracısı izin vermek için güvenlik duvarında giden TCP bağlantı noktası 1433 açmanız gerekmez.

   2. Aracı için bir ad girin.

   3. Seçin **anahtar oluşturma**.

   4. Aracı anahtarını panoya kopyalayın.

        ![Yeni bir eşitleme Aracısı oluşturma](media/sql-database-get-started-sql-data-sync/datasync-preview-selectsyncagent.png)

   5. Seçin **Tamam** kapatmak için **eşitleme Aracısı seçin** sayfası.

   6. SQL Server bilgisayarında, bulun ve istemci eşitleme aracısını uygulamayı çalıştırın.

        ![İstemci Aracısı uygulaması verileri eşitleme](media/sql-database-get-started-sql-data-sync/datasync-preview-clientagent.png)

   7. Eşitleme Aracısı uygulamayı seçin **aracı anahtarını Gönder**. **Eşitleme meta verileri veritabanı yapılandırması** iletişim kutusu açılır.

   8. İçinde **eşitleme meta verileri veritabanı yapılandırması** iletişim kutusu, Azure portaldan kopyaladığınız aracı anahtarını yapıştırın. Ayrıca meta veri veritabanının bulunduğu Azure SQL veritabanı sunucusu için var olan kimlik bilgilerini sağlayın. (Yeni bir meta veri veritabanı oluşturduysanız, bu hub veritabanı ile aynı sunucuda veritabanıdır.) Seçin **Tamam** ve yapılandırmasını tamamlamak bekleyin.

        ![Aracı anahtarını ve sunucu kimlik bilgilerini girin](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-enterkey.png)

        > [!NOTE]
        > Bu noktada bir güvenlik duvarı hatası alırsanız, SQL Server bilgisayarından gelen trafiğe izin vermek için Azure üzerinde bir güvenlik duvarı kuralı oluşturmak zorunda. Kural portalında el ile oluşturabilirsiniz, ancak SQL Server Management Studio (SSMS) oluşturmak daha kolay bulabilirsiniz. SSMS'de, azure'da hub veritabanına bağlanmayı deneyin. < Hub_database_name > olarak adını girin. database.windows.net. Azure güvenlik duvarı kuralını yapılandırmak için iletişim kutusunda adımları izleyin. Ardından istemci eşitleme aracısını uygulamasına dönün.

   9. İstemci eşitleme aracısını uygulamanın tıklayın **kaydetme** bir SQL Server veritabanı aracıyla kaydetmek için. **SQL Server Yapılandırması** iletişim kutusu açılır.

        ![Ekleme ve bir SQL Server veritabanı yapılandırma](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-adddb.png)

   10. İçinde **SQL Server Yapılandırması** iletişim kutusunda, SQL Server kimlik doğrulaması veya Windows kimlik doğrulamasını kullanarak bağlanma isteyip istemediğinizi seçin. SQL Server kimlik doğrulamasını seçerseniz, var olan kimlik bilgilerini girin. SQL Server adını ve eşitlemek istediğiniz veritabanının adını belirtin. Seçin **Bağlantıyı Sına** ayarlarınızı test etmek için. Daha sonra **Kaydet**’e tıklayın. Kayıtlı veritabanı listesinde görünür.

        ![Artık kayıtlı SQL Server veritabanı](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-dbadded.png)

   11. İstemci eşitleme aracısını uygulama artık kapatabilirsiniz.

   12. Portalında, üzerinde **yapılandırma şirket içi** sayfasında **veritabanını seçin.** **Veritabanı Seç** sayfası açılır.

   13. Üzerinde **Veritabanı Seç** sayfasında **eşitleme üyesi adı** alan, yeni eşitleme üyesi için bir ad sağlayın. Bu ad, veritabanı adından farklıdır. Veritabanını listeden seçin. İçinde **eşitleme yönergeleri** alan, select iki yönlü eşitleme, için Hub veya gelen Hub.

        ![Şirket içi veritabanı seçin](media/sql-database-get-started-sql-data-sync/datasync-preview-selectdb.png)

   14. Seçin **Tamam** kapatmak için **Veritabanı Seç** sayfası. Ardından **Tamam** kapatmak için **yapılandırma şirket içi** sayfasında ve oluşturulup dağıtıldığında yeni eşitleme üyesi için bekleyin. Son olarak, tıklayın **Tamam** kapatmak için **eşitleme üyelerini seçin** sayfası.

        ![İçi veritabanı eşitleme grubuna eklendi](media/sql-database-get-started-sql-data-sync/datasync-preview-onpremadded.png)

3. SQL Data Sync ve yerel aracı bağlanmak için kullanıcı adınızı rolüne ekleme `DataSync_Executor`. Veri Eşitleme SQL Server örneğinde bu rolü oluşturur.

## <a name="step-3---configure-sync-group"></a>3. adım: eşitleme grubunu yapılandırma

Yeni eşitleme grubu üyeleri oluşturulan ve dağıtılan, 3. adım, sonra **yapılandırma eşitleme grubu**, vurgulanan **yeni eşitleme grubu** sayfası.

1. Üzerinde **tabloları** sayfasında bir veritabanı eşitleme grubuna üye listesinden seçin ve ardından **yenileme şema**.

2. Kullanılabilir tablolar listeden eşitlemek istediğiniz tabloları seçin.

    ![Eşitlenecek tabloları seçin](media/sql-database-get-started-sql-data-sync/datasync-preview-tables.png)

3. Varsayılan olarak, tablodaki tüm sütun seçilmedi. Tüm sütunları eşitlenecek istemiyorsanız eşitlemek istemediğiniz sütunları için onay kutusunu devre dışı bırakın. Seçili birincil anahtar sütunu olduğundan emin olun.

    ![Eşitlenecek alanları seçin](media/sql-database-get-started-sql-data-sync/datasync-preview-tables2.png)

4. Son olarak, seçin **Kaydet**.

## <a name="faq-about-setup-and-configuration"></a>Kurulumu ve yapılandırması hakkında SSS

### <a name="how-frequently-can-data-sync-synchronize-my-data"></a>Veri eşitleme, verilerimi ne sıklıkta eşitleyebilir misiniz

Eşitlemeler tetikleme arasında en düşük süre beş dakikadır.

### <a name="does-sql-data-sync-fully-create-and-provision-tables"></a>SQL Data Sync tam olarak oluşturmak ve tabloları sağlama

Eşitleme şeması tabloları hedef veritabanında oluşturulmazsa, SQL Data Sync bunları seçtiğiniz sütunları oluşturur. Ancak, bu davranışı tam uygunlukta Şeması'nda, aşağıdaki nedenlerden dolayı sonuç vermez:

- Yalnızca Seçili sütunları, hedef tabloda oluşturulur. Kaynak tablolardaki bazı sütunların eşitleme grubunun parçası değilse, bu sütunlar hedef tabloda sağlanmadı.
- Yalnızca Seçili sütunları için dizin oluşturulur. Kaynak tablo dizin eşitleme grubunun parçası olmayan sütunları varsa, bu dizinler hedef tabloda sağlanmadı.
- XML türü sütunlarındaki dizinler sağlanmadı.
- Denetim kısıtlamalarında sağlanmadı.
- Kaynak tablolarda varolan tetikleyicilerinizi sağlanmadı.
- Görünüm ve saklı yordam hedef veritabanında oluşturulmaz.

Bu kısıtlamalar nedeniyle, şunları öneririz:

- Üretim ortamları için tam uygunluklu şema kendiniz sağlayın.
- Hizmetin ölçeğini çalışmak için SQL Data Sync otomatik sağlama özelliğini de çalışır.

### <a name="why-do-i-see-tables-that-i-did-not-create"></a>Ben oluşturulmayan tabloları neden görüyorum

Veri Eşitleme çalışan tablolarda değişiklik izleme için veritabanı oluşturur. Bunları silme veya veri eşitleme çalışmayı durduruyor.

### <a name="is-my-data-convergent-after-a-sync"></a>Verilerimi bir eşitleme sonrasında convergent mi

Olmayabilir. Bir hub'ı ile bir eşitleme grubu ve üç uçlar (A, B ve C), bir Hub, Hub'ına B ve c hub'a eşitlemeleri olan Bir veritabanı için bir değişiklik yaptıysanız *sonra* değişiklik bir sonraki eşitleme görev tamamlanana kadar veritabanı B veya C veritabanı için yazılmaz eşitleme, hub'a.

### <a name="how-do-i-get-schema-changes-into-a-sync-group"></a>Şema değişiklikleri bir eşitleme grubunda nasıl alabilirim

Olun ve tüm şema değişiklikleri el ile yaymak zorunda.

1. Şema değişiklikleri el ile hub'ına ve tüm eşitleme üyelerine çoğaltın.
2. Eşitleme şemasını güncelleştirme.

**Yeni tablolar ve sütunlar ekleme**.

Yeni tablolar ve sütunlar geçerli eşitleme etkisi yoktur. Veri eşitleme, yeni tablolar ve sütunlar eşitleme şemasına ekleyene kadar yoksayar. Yeni veritabanı nesnelerini eklediğinizde, bu izlenecek en iyi sıra.

1. Yeni Tablo veya sütunları hub'ına ve tüm eşitleme üyeleri ekleyin.
2. Yeni tablolar ve sütunlar eşitleme şemasına ekleyin.
3. Yeni tablolar ve sütunlar haline değerleri eklemeye başlayın.

**Bir sütunun veri türünü değiştirme**.

Var olan bir sütunun veri türünü değiştirdiğinizde, Data Sync yeni değerleri eşitleme şemasında tanımlanan orijinal veri türünü uygun olarak çalışmaya devam eder. Örneğin, kaynak veritabanı türü değiştirirseniz **int** için **bigint**, veri eşitleme için çok büyük bir değer eklediğiniz tamamlanana kadar çalışmaya devam eder **int** veri türü . Değişikliği tamamlamak, hub ve tüm eşitleme üyeleri için şema değişikliği el ile çoğaltma ve ardından eşitleme şemasını güncelleştirmek için.

### <a name="how-can-i-export-and-import-a-database-with-data-sync"></a>Nasıl dışarı aktarma ve veri eşitleme ile veritabanını içeri aktarma

Bir veritabanı olarak dışarı aktardıktan sonra bir `.bacpac` dosya ve yeni bir veritabanı oluşturmak için dosyasını içeri aktarma, Data Sync yeni veritabanı kullanmak için iki şunları yapmanız gerekir:

1. Veri eşitleme nesneleri ve tablolar üzerinde temizlemek **yeni veritabanı** kullanarak [bu betik](https://github.com/vitomaz-msft/DataSyncMetadataCleanup/blob/master/Data%20Sync%20complete%20cleanup.sql). Bu betik, tüm gerekli veri eşitleme nesneleri veritabanından siler.
2. Yeni veritabanı ile eşitleme grubunu yeniden oluşturun. Eski eşitleme grubu artık ihtiyacınız yoksa, onu silin.

## <a name="faq-about-the-client-agent"></a>İstemci Aracısı hakkında SSS

İstemci Aracısı hakkında sık sorulan sorular için bkz. [Aracı SSS](sql-database-data-sync-agent.md#agent-faq).

## <a name="next-steps"></a>Sonraki adımlar

Tebrikler. Hem SQL veritabanı örneğinde hem de SQL Server veritabanını içeren bir eşitleme grubu oluşturdunuz.

SQL Data Sync hakkında daha fazla bilgi için bkz.:

-   Genel Bakış - [verileri Eşitle birden fazla Bulut ve şirket içi veritabanı arasında Azure SQL Data Sync ile](sql-database-sync-data.md)
-   Data Sync'i Ayarla
    - PowerShell ile
        -  [PowerShell kullanarak birden çok Azure SQL veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-sql-databases.md)
        -  [PowerShell kullanarak bir Azure SQL Veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md)
-   Veri Eşitleme Aracısı - [veri Aracısı Azure SQL Data Sync için eşitleme](sql-database-data-sync-agent.md)
-   En iyi uygulamalar - [en iyi uygulamalar için Azure SQL Data Sync](sql-database-best-practices-data-sync.md)
-   İzleyici - [SQL Data Sync'i Log Analytics ile izleme](sql-database-sync-monitor-oms.md)
-   Sorun giderme - [Azure SQL Data Sync ile ilgili sorunları giderme](sql-database-troubleshoot-data-sync.md)
-   Eşitleme şemasını güncelleştirmek
    -   Transact-SQL ile- [Azure SQL Data Sync şema değişikliklerinin çoğaltmayı otomatik hale getirme](sql-database-update-sync-schema.md)
    -   PowerShell ile- [var olan bir eşitleme grubunda eşitleme şemasını güncelleştirmek için PowerShell kullanma](scripts/sql-database-sync-update-schema.md)

SQL Veritabanı hakkında daha fazla bilgi için bkz.:

- [SQL Veritabanı'na Genel Bakış](sql-database-technical-overview.md)
- [Veritabanı Yaşam Döngüsü Yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
