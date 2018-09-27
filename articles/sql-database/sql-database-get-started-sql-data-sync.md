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
ms.date: 04/10/2018
ms.openlocfilehash: f1d439d043feb36fba0cc6c9c9d1b5569a4d8182
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166784"
---
# <a name="set-up-sql-data-sync"></a>SQL Data Sync'i Ayarla
Bu öğreticide, hem Azure SQL veritabanı ve SQL Server örneklerini içeren bir karma eşitleme grubu oluşturarak Azure SQL Data Sync'i ayarlama konusunda bilgi edinin. Yeni eşitleme grubu tam olarak yapılandırılmış ve ayarladığınız zamanlamaya göre eşitler.

Bu öğreticide, SQL veritabanı ve SQL Server ile konusunda en azından biraz deneyim sahibi olduğunuzu varsayar. 

SQL Data Sync hizmetine genel bakış için bkz. [Azure SQL Data Sync ile birden fazla bulut ve şirket içi veritabanı arasında veri eşitleme](sql-database-sync-data.md).

SQL Data Sync'in nasıl yapılandırılacağını gösteren tam PowerShell örnekleri için aşağıdaki makalelere bakın:
-   [PowerShell kullanarak birden çok Azure SQL veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-sql-databases.md)
-   [PowerShell kullanarak bir Azure SQL Veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md)

## <a name="step-1---create-sync-group"></a>1. adım - eşitleme grubu oluşturma

### <a name="locate-the-data-sync-settings"></a>Veri Eşitleme ayarlarını bulma

1.  Tarayıcınızda, Azure portalına gidin.

2.  Portalda SQL veritabanlarınızı Panonuzda veya SQL veritabanları simgesine araç bulun.

    ![Azure SQL veritabanları listesi](media/sql-database-get-started-sql-data-sync/datasync-preview-sqldbs.png)

3.  Üzerinde **SQL veritabanları** sayfasında, veri eşitleme için hub veritabanı kullanmak istediğiniz mevcut bir SQL veritabanını seçin. SQL veritabanı sayfası açılır.

    Hub veritabanı eşitleme grubu birden çok veritabanı uç noktaları olan eşitleme topolojinin merkezi uç noktadır. Diğer tüm veritabanı uç hub veritabanı ile aynı eşitleme grubu - diğer bir deyişle, tüm üye veritabanları - eşitleme.

4.  Seçili veritabanı için SQL veritabanı sayfasında, seçin **diğer veritabanlarıyla Eşitle**. Veri Eşitleme sayfası açılır.

    ![Diğer veritabanlarını seçeneğine eşitleme](media/sql-database-get-started-sql-data-sync/datasync-preview-newsyncgroup.png)

### <a name="create-a-new-sync-group"></a>Yeni bir eşitleme grubu oluşturma

1.  Veri Eşitleme sayfasında **yeni eşitleme grubu**. **Yeni eşitleme grubu** sayfasını açar, adım 1 **eşitleme grubu oluşturma**, vurgulanan. **Veri eşitleme grubu oluşturma** sayfası da açılır.

2.  Üzerinde **veri eşitleme grubu oluşturma** sayfasında, şunları yapın:

    1.  İçinde **eşitleme grubu adı** yeni eşitleme grubu için bir ad girin.

    2.  İçinde **eşitleme meta verileri veritabanı** bölümünde, (önerilen) yeni bir veritabanı oluşturmak mı, yoksa mevcut bir veritabanını kullanmayı seçin.

        > [!NOTE]
        > Microsoft Eşitleme meta verileri veritabanı olarak kullanmak için yeni, boş bir veritabanı oluşturma önerir. Veri eşitleme, bu veritabanında tablolar oluşturur ve sık kullanılan bir iş yükü çalıştırır. Bu veritabanı, seçili bölgesinde eşitleme gruplarınızın tümü için eşitleme meta verileri veritabanı olarak otomatik olarak paylaşılır. Sürükleyip bırakarak olmadan, eşitleme meta verileri veritabanı veya adını değiştiremezsiniz.

        Seçerseniz, **yeni veritabanı**seçin **yeni veritabanı oluştur.** **SQL veritabanı** sayfası açılır. Üzerinde **SQL veritabanı** sayfasında ad ve yeni veritabanını yapılandırın. Sonra **Tamam**’ı seçin.

        Seçerseniz, **varolan veritabanını kullan**, veritabanını listeden seçin.

    3.  İçinde **otomatik eşitleme** bölümünde, ilk seçin **üzerinde** veya **kapalı**.

        Seçerseniz, **üzerinde**, **eşitleme sıklığı** bölümünde, bir sayı girin ve saniye, dakika, saat veya gün seçin.

        ![Eşitleme sıklığı belirtin](media/sql-database-get-started-sql-data-sync/datasync-preview-syncfreq.png)

    4.  İçinde **çakışma** bölümünde, "Hub wins" veya "Üyesi WINS" seçin

        "Hub wins" bir çakışma meydana geldiğinde, hub veritabanındaki verilerle çakışan üye veritabanı verilerini yazıldığını anlamına gelir. "Üye WINS" bir çakışma oluştuğunda, üye veritabanındaki verilerle çakışan hub veritabanındaki verilerle yazıldığını anlamına gelir. 

        ![Çakışmalarını belirtin](media/sql-database-get-started-sql-data-sync/datasync-preview-conflictres.png)

    5.  Seçin **Tamam** oluşturulup dağıtıldığında için yeni eşitleme grubu bekleyin.

## <a name="step-2---add-sync-members"></a>2. adım - eşitleme üyesi ekleme

Yeni eşitleme grubu oluşturup, adım 2 ' yi dağıttıktan sonra **eşitleme üyesi ekleme**, vurgulanan **yeni eşitleme grubu** sayfası.

İçinde **Hub veritabanı** bölümünde, hub veritabanının bulunduğu SQL veritabanı sunucusu için var olan kimlik bilgilerini girin. Girmeyin *yeni* Bu bölümde kimlik bilgileri.

![Hub veritabanı eşitleme grubuna eklendi.](media/sql-database-get-started-sql-data-sync/datasync-preview-hubadded.png)

### <a name="add-an-azure-sql-database"></a>Bir Azure SQL veritabanı ekleyin

İçinde **üye veritabanı** bölümüne, isteğe bağlı olarak seçerek Azure SQL veritabanı eşitleme grubuna ekleyin **Azure veritabanı ekleme**. **Azure veritabanını Yapılandır** sayfası açılır.

Üzerinde **Azure veritabanını Yapılandır** sayfasında, şunları yapın:

1.  İçinde **eşitleme üyesi adı** alan, yeni eşitleme üyesi için bir ad sağlayın. Bu ad, veritabanı adından farklıdır.

2.  İçinde **abonelik** alan, faturalama amacıyla ilişkili Azure aboneliği seçin.

3.  İçinde **Azure SQL Server** alanında, var olan SQL veritabanı sunucusu seçin.

4.  İçinde **Azure SQL veritabanı** alanında, mevcut bir SQL veritabanını seçin.

5.  İçinde **eşitleme yönergeleri** alan, select iki yönlü eşitleme, için Hub veya gelen Hub.

    ![Yeni bir SQL veritabanı eşitleme üyesi ekleme](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadding.png)

6.  İçinde **kullanıcıadı** ve **parola** alanları üye veritabanının bulunduğu SQL veritabanı sunucusu için var olan kimlik bilgilerini girin. Girmeyin *yeni* Bu bölümde kimlik bilgileri.

7.  Seçin **Tamam** oluşturulup dağıtıldığında yeni eşitleme üyesi için bekleyin.

    ![Yeni SQL veritabanı eşitleme üyesi eklenmiştir.](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadded.png)

### <a name="add-on-prem"></a> Bir şirket içi SQL Server veritabanı ekleyin

İçinde **üye veritabanı** bölümüne, isteğe bağlı olarak şirket içi SQL Server seçerek eşitleme grubuna ekleyin **bir şirket içi veritabanı Ekle**. **Yapılandırma şirket içi** sayfası açılır.

Üzerinde **yapılandırma şirket içi** sayfasında, şunları yapın:

1.  Seçin **eşitleme Aracısı ağ geçidini seçin**. **Eşitleme Aracısı seçin** sayfası açılır.

    ![Eşitleme Aracısı ağ geçidini seçin](media/sql-database-get-started-sql-data-sync/datasync-preview-choosegateway.png)

2.  Üzerinde **eşitleme Aracısı ağ geçidini seçin** sayfasında, yeni bir aracı oluşturun veya var olan bir aracı kullanmak isteyip istemediğinizi seçin.

    Seçerseniz, **varolan aracıları**, var olan aracıyı listeden seçin.

    Seçerseniz, **yeni bir aracı oluşturun**, şunları yapın:

    1.  Sağlanan bağlantıdan istemci eşitleme Aracısı yazılımını indirin ve SQL Server bulunduğu bilgisayara yükleyin.
 
        > [!IMPORTANT]
        > Sunucuyla iletişim istemci Aracısı izin vermek için güvenlik duvarında giden TCP bağlantı noktası 1433 açmanız gerekmez.


    2.  Aracı için bir ad girin.

    3.  Seçin **anahtar oluşturma**.

    4.  Aracı anahtarını panoya kopyalayın.
        
        ![Yeni bir eşitleme Aracısı oluşturma](media/sql-database-get-started-sql-data-sync/datasync-preview-selectsyncagent.png)

    5.  Seçin **Tamam** kapatmak için **eşitleme Aracısı seçin** sayfası.

    6.  SQL Server bilgisayarında, bulun ve istemci eşitleme aracısını uygulamayı çalıştırın.

        ![İstemci Aracısı uygulaması verileri eşitleme](media/sql-database-get-started-sql-data-sync/datasync-preview-clientagent.png)

    7.  Eşitleme Aracısı uygulamayı seçin **aracı anahtarını Gönder**. **Eşitleme meta verileri veritabanı yapılandırması** iletişim kutusu açılır.

    8.  İçinde **eşitleme meta verileri veritabanı yapılandırması** iletişim kutusu, Azure portaldan kopyaladığınız aracı anahtarını yapıştırın. Ayrıca meta veri veritabanının bulunduğu Azure SQL veritabanı sunucusu için var olan kimlik bilgilerini sağlayın. (Yeni bir meta veri veritabanı oluşturduysanız, bu hub veritabanı ile aynı sunucuda veritabanıdır.) Seçin **Tamam** ve yapılandırmasını tamamlamak bekleyin.

        ![Aracı anahtarını ve sunucu kimlik bilgilerini girin](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-enterkey.png)

        >   [!NOTE] 
        >   Bu noktada bir güvenlik duvarı hatası alırsanız, SQL Server bilgisayarından gelen trafiğe izin vermek için Azure üzerinde bir güvenlik duvarı kuralı oluşturmak zorunda. Kural portalında el ile oluşturabilirsiniz, ancak SQL Server Management Studio (SSMS) oluşturmak daha kolay bulabilirsiniz. SSMS'de, azure'da hub veritabanına bağlanmayı deneyin. < Hub_database_name > olarak adını girin. database.windows.net. Azure güvenlik duvarı kuralını yapılandırmak için iletişim kutusunda adımları izleyin. Ardından istemci eşitleme aracısını uygulamasına dönün.

    9.  İstemci eşitleme aracısını uygulamanın tıklayın **kaydetme** bir SQL Server veritabanı aracıyla kaydetmek için. **SQL Server Yapılandırması** iletişim kutusu açılır.

        ![Ekleme ve bir SQL Server veritabanı yapılandırma](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-adddb.png)

    10. İçinde **SQL Server Yapılandırması** iletişim kutusunda, SQL Server kimlik doğrulaması veya Windows kimlik doğrulamasını kullanarak bağlanma isteyip istemediğinizi seçin. SQL Server kimlik doğrulamasını seçerseniz, var olan kimlik bilgilerini girin. SQL Server adını ve eşitlemek istediğiniz veritabanının adını belirtin. Seçin **Bağlantıyı Sına** ayarlarınızı test etmek için. Daha sonra **Kaydet**’e tıklayın. Kayıtlı veritabanı listesinde görünür.

        ![Artık kayıtlı SQL Server veritabanı](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-dbadded.png)

    11. İstemci eşitleme aracısını uygulama artık kapatabilirsiniz.

    12. Portalında, üzerinde **yapılandırma şirket içi** sayfasında **veritabanını seçin.** **Veritabanı Seç** sayfası açılır.

    13. Üzerinde **Veritabanı Seç** sayfasında **eşitleme üyesi adı** alan, yeni eşitleme üyesi için bir ad sağlayın. Bu ad, veritabanı adından farklıdır. Veritabanını listeden seçin. İçinde **eşitleme yönergeleri** alan, select iki yönlü eşitleme, için Hub veya gelen Hub.

        ![Şirket içi veritabanı seçin](media/sql-database-get-started-sql-data-sync/datasync-preview-selectdb.png)

    14. Seçin **Tamam** kapatmak için **Veritabanı Seç** sayfası. Ardından **Tamam** kapatmak için **yapılandırma şirket içi** sayfasında ve oluşturulup dağıtıldığında yeni eşitleme üyesi için bekleyin. Son olarak, tıklayın **Tamam** kapatmak için **eşitleme üyelerini seçin** sayfası.

        ![İçi veritabanı eşitleme grubuna eklendi](media/sql-database-get-started-sql-data-sync/datasync-preview-onpremadded.png)

3.  SQL Data Sync ve yerel aracı bağlanmak için kullanıcı adınızı rolüne ekleme `DataSync_Executor`. Veri Eşitleme SQL Server örneğinde bu rolü oluşturur.

## <a name="step-3---configure-sync-group"></a>3. adım: eşitleme grubunu yapılandırma

Yeni eşitleme grubu üyeleri oluşturulan ve dağıtılan, 3. adım, sonra **yapılandırma eşitleme grubu**, vurgulanan **yeni eşitleme grubu** sayfası.

1.  Üzerinde **tabloları** sayfasında bir veritabanı eşitleme grubuna üye listesinden seçin ve ardından **yenileme şema**.

2.  Kullanılabilir tablolar listeden eşitlemek istediğiniz tabloları seçin.

    ![Eşitlenecek tabloları seçin](media/sql-database-get-started-sql-data-sync/datasync-preview-tables.png)

3.  Varsayılan olarak, tablodaki tüm sütun seçilmedi. Tüm sütunları eşitlenecek istemiyorsanız eşitlemek istemediğiniz sütunları için onay kutusunu devre dışı bırakın. Seçili birincil anahtar sütunu olduğundan emin olun.

    ![Eşitlenecek alanları seçin](media/sql-database-get-started-sql-data-sync/datasync-preview-tables2.png)

4.  Son olarak, seçin **Kaydet**.

## <a name="faq-about-setup-and-configuration"></a>Kurulumu ve yapılandırması hakkında SSS

### <a name="how-frequently-can-data-sync-synchronize-my-data"></a>Veri eşitleme, verilerimi ne sıklıkta eşitleyebilir misiniz? 
En az beş dakikada sıklığıdır.

### <a name="does-sql-data-sync-fully-create-and-provision-tables"></a>SQL Data Sync tam olarak oluşturmak ve tabloları sağlama?

Eşitleme şeması tabloları hedef veritabanında oluşturulmazsa, SQL Data Sync bunları seçtiğiniz sütunları oluşturur. Ancak, bu davranışı tam uygunlukta Şeması'nda, aşağıdaki nedenlerden dolayı sonuç vermez:

-   Yalnızca Seçili sütunları, hedef tabloda oluşturulur. Kaynak tablolardaki bazı sütunların eşitleme grubunun parçası değilse, bu sütunlar hedef tabloda sağlanmadı.

-   Yalnızca Seçili sütunları için dizin oluşturulur. Kaynak tablo dizin eşitleme grubunun parçası olmayan sütunları varsa, bu dizinler hedef tabloda sağlanmadı.

-   XML türü sütunlarındaki dizinler sağlanmadı.

-   Denetim kısıtlamalarında sağlanmadı.

-   Kaynak tablolarda varolan tetikleyicilerinizi sağlanmadı.

-   Görünümler ve saklı yordamlar hedef veritabanında oluşturulmaz.

Bu kısıtlamalar nedeniyle, şunları öneririz:
-   Üretim ortamları için tam uygunluklu şema kendiniz sağlayın.
-   Hizmetin ölçeğini çalışmak için SQL Data Sync otomatik sağlama özelliğini de çalışır.

### <a name="why-do-i-see-tables-that-i-did-not-create"></a>Ben oluşturulmayan tabloları neden görüyorum?  
Veri Eşitleme çalışan tablolarda değişiklik izleme için veritabanı oluşturur. Bunları silme veya veri eşitleme çalışmayı durduruyor.

### <a name="is-my-data-convergent-after-a-sync"></a>Verilerimi bir eşitleme sonrasında convergent mi?

Olmayabilir. Bir hub'ı ile bir eşitleme grubu ve üç uçlar (A, B ve C), bir Hub, Hub'ına B ve c hub'a eşitlemeleri olan Bir veritabanı için bir değişiklik yaptıysanız *sonra* değişiklik bir sonraki eşitleme görev tamamlanana kadar veritabanı B veya C veritabanı için yazılmaz eşitleme, hub'a.

### <a name="how-do-i-get-schema-changes-into-a-sync-group"></a>Şema değişiklikleri bir eşitleme grubunda nasıl alabilirim?

Olun ve tüm şema değişiklikleri el ile yaymak zorunda.
1. Şema değişiklikleri el ile hub'ına ve tüm eşitleme üyelerine çoğaltın.
2. Eşitleme şemasını güncelleştirme.

**Yeni tablolar ve sütunlar ekleme**. Yeni tablolar ve sütunlar geçerli eşitleme etkisi yoktur. Veri eşitleme, yeni tablolar ve sütunlar eşitleme şemasına ekleyene kadar yoksayar. Yeni veritabanı nesnelerini eklediğinizde, bu izlenecek en iyi sıra.
1. Yeni Tablo veya sütunları hub'ına ve tüm eşitleme üyeleri ekleyin.
2. Yeni tablolar ve sütunlar eşitleme şemasına ekleyin.
3. Yeni tablolar ve sütunlar haline değerleri eklemeye başlayın.

**Bir sütunun veri türünü değiştirme**. Var olan bir sütunun veri türünü değiştirdiğinizde, Data Sync yeni değerleri eşitleme şemasında tanımlanan orijinal veri türünü uygun olarak çalışmaya devam eder. Örneğin, kaynak veritabanı türü değiştirirseniz **int** için **bigint**, veri eşitleme için çok büyük bir değer eklediğiniz tamamlanana kadar çalışmaya devam eder **int** veri türü . Değişikliği tamamlamak, hub ve tüm eşitleme üyeleri için şema değişikliği el ile çoğaltma ve ardından eşitleme şemasını güncelleştirmek için.

### <a name="how-can-i-export-and-import-a-database-with-data-sync"></a>Nasıl dışarı aktarma ve veri eşitleme ile veritabanını içeri aktarma?
Bir veritabanı olarak dışarı aktardıktan sonra bir `.bacpac` dosya ve yeni bir veritabanı oluşturmak için dosyasını içeri aktarma, Data Sync yeni veritabanı kullanmak için iki şunları yapmanız gerekir:
1.  Veri eşitleme nesneleri ve tablolar üzerinde temizlemek **yeni veritabanı** kullanarak [bu betik](https://github.com/vitomaz-msft/DataSyncMetadataCleanup/blob/master/Data%20Sync%20complete%20cleanup.sql). Bu betik, tüm gerekli veri eşitleme nesneleri veritabanından siler.
2.  Yeni veritabanı ile eşitleme grubunu yeniden oluşturun. Eski eşitleme grubu artık ihtiyacınız yoksa, onu silin.

## <a name="faq-about-the-client-agent"></a>İstemci Aracısı hakkında SSS

### <a name="why-do-i-need-a-client-agent"></a>Bir istemci Aracısı neden gerekiyor?

SQL Data Sync hizmeti, istemci Aracısı aracılığıyla SQL Server veritabanları ile iletişim kurar. Bu güvenlik özelliğini bir güvenlik duvarının arkasındaki veritabanları ile doğrudan iletişim engeller. Ne zaman SQL Data Sync hizmeti iletişim kurar aracıyla, bunu kullanarak yaptığı şifrelenmiş bağlantılar ve benzersiz bir belirteç veya *aracı anahtarı*. SQL Server veritabanları bağlantı dizesi ve aracı anahtarını kullanarak aracı kimlik doğrulaması. Bu tasarım, güvenlik verileriniz için yüksek düzeyde sağlar.

### <a name="how-many-instances-of-the-local-agent-ui-can-be-run"></a>Kaç tane yerel aracı, UI çalıştırabilir miyim?

Kullanıcı Arabirimi yalnızca bir örneği çalıştırılabilir.

### <a name="how-can-i-change-my-service-account"></a>Hizmet Hesabımı nasıl değiştirebilirim?

Bir istemci Aracısı yükledikten sonra hizmet hesabını değiştirmek için tek kaldırıp yeni hizmet hesabını yeni bir istemci Aracısı yükleme yoludur.

### <a name="how-do-i-change-my-agent-key"></a>Aracı anahtarımı nasıl değiştirebilirim?

Bir aracı anahtarı, bir aracı tarafından yalnızca bir kez kullanılabilir. Kaldırın, sonra yeni bir aracıyı yeniden yükleyin veya birden çok aracı tarafından yeniden kullanılabilir olduğunda kullanılamayacak. Var olan bir aracı için yeni bir anahtar oluşturmanız gerekiyorsa, SQL Data Sync hizmet ve istemci Aracısı ile aynı anahtar kaydedilir emin olmanız gerekir.

### <a name="how-do-i-retire-a-client-agent"></a>Bir istemci Aracısı nasıl devre dışı bırakma?

Hemen geçersiz kılmak veya aracıyı devre dışı bırakmak için Portalı'nda kendi anahtarını yeniden ancak aracı Arabiriminde bulunmayın. Bir anahtar yeniden oluşturuluyor, karşılık gelen aracı çevrimiçi veya çevrimdışı ise belirtilmediğine önceki anahtar geçersiz kılar.

### <a name="how-do-i-move-a-client-agent-to-another-computer"></a>Başka bir bilgisayara bir istemci Aracısı nasıl taşırım?

Şu anda açıktır olandan farklı bir bilgisayardan yerel aracı çalıştırmak istiyorsanız, şunları yapın:

1. İstediğiniz bilgisayara aracıyı yükleyin.

2. SQL Data Sync portalında oturum açın ve yeni aracı için bir aracı anahtarı yeniden oluştur.

3. Yeni aracı anahtarı göndermek için yeni aracının kullanıcı Arabirimi kullanın.

4. İstemci Aracısı, daha önce kaydedilmiş şirket içi veritabanlarının listesini yüklerken bekleyin.

5. Veritabanı kimlik bilgileri olarak ulaşılamaz görüntüleyen tüm veritabanları için sağlar. Bu veritabanları, aracının yüklü olduğu yeni bilgisayardan erişilebilir olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar
Tebrikler. Hem SQL veritabanı örneğinde hem de SQL Server veritabanını içeren bir eşitleme grubu oluşturdunuz.

SQL Data Sync hakkında daha fazla bilgi için bkz.:

-   [Azure SQL Data Sync ile birden fazla bulut ve şirket içi veritabanı arasında veri eşitleme](sql-database-sync-data.md)
-   [Azure SQL Data Sync için en iyi yöntemler](sql-database-best-practices-data-sync.md)
-   [Azure SQL Data Sync’i Log Analytics ile izleme](sql-database-sync-monitor-oms.md)
-   [Azure SQL Data Sync ile ilgili sorun giderme](sql-database-troubleshoot-data-sync.md)

-   SQL Data Sync’in nasıl yapılandırılacağını gösteren tam PowerShell örnekleri:
    -   [PowerShell kullanarak birden çok Azure SQL veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [PowerShell kullanarak bir Azure SQL Veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [SQL Data Sync REST API belgelerini indirin](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

SQL Veritabanı hakkında daha fazla bilgi için bkz.:

-   [SQL Veritabanı'na Genel Bakış](sql-database-technical-overview.md)
-   [Veritabanı Yaşam Döngüsü Yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
