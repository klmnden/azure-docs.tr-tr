---
title: Azure SQL veri eşitleme ayarı | Microsoft Docs
description: Bu öğretici Azure SQL veri eşitlemeyi ayarlamak nasıl gösterir
services: sql-database
author: allenwux
manager: craigg
ms.service: sql-database
ms.custom: load & move data
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: xiwu
ms.reviewer: douglasl
ms.openlocfilehash: df7ca91d403374e8d320822f5fa384a866fac0ae
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37026091"
---
# <a name="set-up-sql-data-sync"></a>SQL veri eşitleme ayarı
Bu öğreticide, Azure SQL Database ve SQL Server örneklerini içeren bir karma eşitleme grubu oluşturarak Azure SQL veri eşitlemeyi ayarlamak nasıl öğrenin. Yeni eşitleme grubunu tam olarak yapılandırılmamış ve belirlediğiniz bir zamanlamaya göre eşitler.

Bu öğretici, SQL Database ve SQL Server ile en az bazı konusunda deneyim sahibi olduğunuzu varsayar. 

SQL veri eşitleme genel bakış için bkz: [verileri Eşitle birden çok Bulut ve şirket içi veritabanları arasında Azure SQL veri eşitleme ile](sql-database-sync-data.md).

SQL veri eşitleme yapılandırmayı gösterir tam PowerShell örnekler için aşağıdaki makalelere bakın:
-   [PowerShell kullanarak birden çok Azure SQL veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-sql-databases.md)
-   [PowerShell kullanarak bir Azure SQL Veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md)

## <a name="step-1---create-sync-group"></a>1. adım - eşitleme grubu oluşturma

### <a name="locate-the-data-sync-settings"></a>Veri Eşitleme ayarlarını bulun

1.  Tarayıcınızda, Azure portalına gidin.

2.  Portalda, SQL veritabanlarınızın Panonuzda veya SQL veritabanları simgesi araç çubuğunda bulun.

    ![Azure SQL veritabanı listesi](media/sql-database-get-started-sql-data-sync/datasync-preview-sqldbs.png)

3.  Üzerinde **SQL veritabanları** sayfasında, veri eşitleme için hub veritabanı olarak kullanmak istediğiniz varolan bir SQL veritabanını seçin. SQL veritabanı sayfası açılır.

    Eşitleme grubu birden çok veritabanı uç noktaları olan eşitleme topolojisi merkezi uç noktası hub veritabanıdır. Diğer tüm veritabanı bitiş hub veritabanı ile aynı eşitleme grubu - diğer bir deyişle, tüm üye veritabanları - eşitleme.

4.  Seçili veritabanı için SQL veritabanı sayfasında, seçin **diğer veritabanlarına eşitleme**. Veri Eşitleme sayfası açılır.

    ![Diğer veritabanlarını seçeneğine eşitleme](media/sql-database-get-started-sql-data-sync/datasync-preview-newsyncgroup.png)

### <a name="create-a-new-sync-group"></a>Yeni bir eşitleme grubu oluşturun

1.  Veri Eşitleme sayfasında seçin **yeni eşitleme grubu**. **Yeni eşitleme grubu** sayfası açılır adım 1 ile **eşitleme Grup Oluştur**, vurgulanan. **Veri eşitleme grubu oluşturma** sayfası da açılır.

2.  Üzerinde **veri eşitleme grubu oluşturma** sayfasında, şunları yapın:

    1.  İçinde **eşitleme grubu adı** alan, yeni eşitleme grubu için bir ad girin.

    2.  İçinde **eşitleme meta veri veritabanı** bölümünde, (önerilen) yeni bir veritabanı oluşturun veya varolan bir veritabanını kullanmak için seçin.

        > [!NOTE]
        > Microsoft Eşitleme meta veri veritabanı olarak kullanmak için yeni, boş bir veritabanı oluşturmak önerir. Veri Eşitleme bu veritabanında tablolar oluşturur ve sık kullanılan bir iş yükü çalıştırır. Bu veritabanı, tüm seçili bölgede eşitleme gruplarınızın için eşitleme meta veri veritabanı olarak otomatik olarak paylaşılır. Bırakma olmadan eşitleme meta verileri veritabanı veya adını değiştiremezsiniz.

        Seçerseniz **yeni veritabanı**seçin **yeni veritabanı oluştur.** **SQL veritabanı** sayfası açılır. Üzerinde **SQL veritabanı** sayfa, ad ve yeni veritabanını yapılandırın. Sonra **Tamam**’ı seçin.

        Seçerseniz **varolan veritabanını kullan**, veritabanını listeden seçin.

    3.  İçinde **otomatik eşitleme** bölümünde, ilk seçin **üzerinde** veya **devre dışı**.

        Seçerseniz **üzerinde**, **eşitleme sıklığı** bölümünde, bir sayı girin ve saniye, dakika, saat veya gün seçin.

        ![Eşitleme sıklığı belirtin](media/sql-database-get-started-sql-data-sync/datasync-preview-syncfreq.png)

    4.  İçinde **çakışma çözümü** bölümünde, "Hub wins" veya "Üye WINS." seçin

        "Hub wins" bir çakışma meydana geldiğinde hub veritabanındaki verileri üye veritabanında çakışan verilerin üzerine yazar, anlamına gelir. Bir çakışma meydana geldiğinde üye veritabanındaki verileri hub veritabanındaki çakışan verilerin üzerine yazar, "Üye WINS" anlamına gelir. 

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

### <a name="add-on-prem"></a> Bir şirket içi SQL Server veritabanı ekleyin

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
        >   Bu noktada bir güvenlik duvarı hata alırsanız, SQL Server bilgisayardan gelen trafiğe izin vermek için Azure üzerinde bir güvenlik duvarı kuralı oluşturmanız gerekir. Kural Portalı'nda el ile oluşturabilirsiniz, ancak SQL Server Management Studio (SSMS) oluşturmak daha kolay bulabilirsiniz. SSMS, Azure hub veritabanına bağlanmayı deneyin. < Hub_database_name > olarak adını girin. database.windows.net. Azure güvenlik duvarı kuralı yapılandırmak için iletişim kutusunda adımları izleyin. Ardından istemci eşitleme Aracısı uygulamaya geri dönün.

    9.  İstemci eşitleme Aracısı uygulamanın tıklayın **kaydetmek** aracı ile bir SQL Server veritabanına kaydetmek için. **SQL Server yapılandırma** iletişim kutusu açılır.

        ![Ekleme ve bir SQL Server veritabanı yapılandırma](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-adddb.png)

    10. İçinde **SQL Server yapılandırma** iletişim kutusu, SQL Server kimlik doğrulaması veya Windows kimlik doğrulaması kullanarak bağlanmak üzere seçim yapın. SQL Server kimlik doğrulamasını seçerseniz, var olan kimlik bilgilerini girin. SQL Server adı ve eşitlemek istediğiniz veritabanının adını belirtin. Seçin **Bağlantıyı Sına** ayarlarınızı sınamak için. Daha sonra **Kaydet**’e tıklayın. Kayıtlı veritabanı listede görüntülenir.

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

    ![Eşitlenecek alanları seçin](media/sql-database-get-started-sql-data-sync/datasync-preview-tables2.png)

4.  Son olarak, seçin **kaydetmek**.

## <a name="faq-about-setup-and-configuration"></a>Kurulum ve yapılandırma hakkında SSS

### <a name="how-frequently-can-data-sync-synchronize-my-data"></a>Veri Eşitleme verilerimi ne sıklıkta eşitleyebilirsiniz? 
En az beş dakikada sıklığıdır.

### <a name="does-sql-data-sync-fully-create-and-provision-tables"></a>SQL veri eşitleme tam olarak oluşturun ve tabloları sağlamak?

Eşitleme şema tabloları hedef veritabanına zaten oluşturulmadıysa, SQL veri eşitleme bunları seçtiğiniz sütunlarla oluşturur. Ancak, bu davranış tam uygunluğunu Şeması'nda, aşağıdaki nedenlerle oluşmaz:

-   Yalnızca seçtiğiniz sütunları hedef tabloda oluşturulur. Kaynak tablolarda bazı sütunları eşitleme grubunun parçası değilse, bu sütunları hedef tablolarında sağlanmayan.

-   Dizinler yalnızca seçilen sütunlar için oluşturulur. Kaynak tablo dizin eşitleme grubunun parçası olmayan sütunları varsa, bu dizinler hedef tabloda sağlanan değil.

-   XML türü sütunlarındaki dizinler sağlanmayan.

-   Denetim kısıtlamalarında sağlanmayan.

-   Kaynak tablolarda varolan Tetikleyiciler sağlanmayan.

-   Görünümleri ve saklı yordamlar hedef veritabanı oluşturulmadı.

Bu sınırlamalar nedeniyle şunları öneririz:
-   Üretim ortamları için tam uygunluğunu şema kendiniz sağlayın.
-   Servisi denemek için otomatik sağlama özelliği SQL veri eşitleme iyi çalışır.

### <a name="why-do-i-see-tables-that-i-did-not-create"></a>Oluşturulamadı tabloları neden görüyor musunuz?  
Veri Eşitleme yan tablolar değişiklik izleme, veritabanınızdaki oluşturur. Bunları silmeyin veya veri eşitleme çalışmayı durdurur.

### <a name="is-my-data-convergent-after-a-sync"></a>Verilerim bir eşitleme sonrasında convergent mi?

Olmayabilir. Bir hub ile bir eşitleme grubu ve üç bağlı bileşen (A, B ve C), bir Hub, Hub'ına B ve c hub'a eşitlemeleri olan Bir veritabanına bir değişiklik yaptıysanız *sonra* değişiklik bir sonraki eşitleme görevi kadar veritabanı B veya C veritabanı yazılmaz eşitleme, hub'a.

### <a name="how-do-i-get-schema-changes-into-a-sync-group"></a>Şema değişiklikleri eşitleme grubuna nasıl sağlarım?

Olun ve tüm şema değişiklikleri el ile yayılması gerekir.
1. Şema değişiklikleri el ile hub'ına ve tüm eşitleme üyelerine çoğaltılır.
2. Eşitleme şeması'nı güncelleştirme.

**Yeni tablolar ve sütunlar ekleme**. Yeni tablolar ve sütunlar geçerli eşitleme etkisi yoktur. Eşitleme şeması için eklenene kadar veri eşitleme yeni tablolar ve sütunlar yok sayar. Yeni veritabanı nesneleri eklediğinizde, bu en iyi sıra izleyin.
1. Yeni tablolar ve sütunlar hub'ına ve tüm eşitleme üyeleri ekleyin.
2. Yeni tablolar ve sütunlar için eşitleme şema ekleyin.
3. Değer yeni tablolar ve sütunlar eklemek için Başlat'ı tıklatın.

**Bir sütunun veri türünü değiştirme**. Varolan bir sütunun veri türünü değiştirdiğinizde, veri eşitleme yeni değerleri eşitleme şemada tanımlanan özgün veri türü uygun sürece çalışmaya devam eder. Örneğin, kaynak veritabanından türünde değiştirirseniz **int** için **bigint**, veri eşitleme için çok büyük bir değer Ekle kadar çalışmaya devam **int** veri türü . Değişikliği tamamlamak için hub'ına ve tüm eşitleme üyelerine şema değişikliği el ile çoğaltmak ve eşitleme Şeması'nı güncelleştirme.

### <a name="how-can-i-export-and-import-a-database-with-data-sync"></a>Nasıl dışarı aktarma ve veri eşitleme ile veritabanı alma?
Bir veritabanı olarak dışarı aktardıktan sonra bir `.bacpac` dosya ve yeni bir veritabanı oluşturmak için dosya alma, veri eşitleme yeni veritabanı kullanmak için aşağıdaki iki birini yapmanız gerekir:
1.  Veri eşitleme nesneleri yan tablolar üzerinde Temizleme **yeni veritabanı** kullanarak [bu komut dosyası](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/clean_up_data_sync_objects.sql). Bu komut tüm gerekli veri eşitleme nesneleri veritabanından siler.
2.  Yeni bir veritabanı ile eşitleme grubunu yeniden oluşturun. Eski eşitleme grubu artık ihtiyacınız varsa dosyayı silin.

## <a name="faq-about-the-client-agent"></a>İstemci Aracısı hakkında SSS

### <a name="why-do-i-need-a-client-agent"></a>Bir istemci Aracısı neden gerekiyor mu?

SQL veri eşitleme hizmeti SQL Server veritabanlarını istemci Aracısı üzerinden iletişim kurar. Bu güvenlik özelliğinin bir güvenlik duvarının arkasındaki veritabanları ile doğrudan iletişim engeller. Şifrelenmiş bağlantıları ve benzersiz bir belirteç zaman SQL veri eşitleme hizmeti ile iletişim kurar aracı ile mu bunu kullanarak veya *aracı anahtarını*. SQL Server veritabanları bağlantı dizesi ve aracı anahtarı kullanılarak Aracısı kimlik doğrulaması. Bu tasarım, güvenlik, verileriniz için yüksek düzeyde sağlar.

### <a name="how-many-instances-of-the-local-agent-ui-can-be-run"></a>Kaç tane yerel aracı örneğinin UI çalıştırabilir miyim?

Kullanıcı arabirimini yalnızca bir örneği çalıştırılabilir.

### <a name="how-can-i-change-my-service-account"></a>Hizmet Hesabımı nasıl değiştirebilir miyim?

Bir istemci Aracısı yükledikten sonra hizmet hesabını değiştirmek için yalnızca bunu kaldırın ve yeni bir istemci aracısı yeni hizmet hesabıyla yüklemek için yoludur.

### <a name="how-do-i-change-my-agent-key"></a>Aracı anahtarımı nasıl değişiyor?

Aracı anahtarını bir aracı tarafından yalnızca bir kez kullanılabilir. Bu, kaldırın, sonra yeni bir aracıyı yeniden yükleyin veya birden çok aracı tarafından kullanılabilir olduğunda yeniden kullanılamaz. Varolan bir aracı için yeni bir anahtar oluşturmanız gerekiyorsa, aynı anahtar ile SQL veri eşitleme hizmeti istemci Aracısı ile kaydedilir emin olmalısınız.

### <a name="how-do-i-retire-a-client-agent"></a>Bir istemci Aracısı nasıl devre dışı bırakma?

Hemen geçersiz ya da bir aracı devre dışı bırakmak için Portalı'nda, anahtarı yeniden ancak aracı Arabiriminde bulunmayın. Bir anahtar oluşturma işlemi, karşılık gelen Aracısı çevrimiçi veya çevrimdışı olması durumunda belirtilmediğine önceki anahtar geçersiz kılar.

### <a name="how-do-i-move-a-client-agent-to-another-computer"></a>Bir istemci aracısı başka bir bilgisayara nasıl taşıyabilirim?

Şu anda açıktır olandan farklı bir bilgisayardan yerel aracı çalıştırmak istiyorsanız, şunları yapın:

1. İstediğiniz bilgisayara aracıyı yükleyin.

2. SQL veri eşitleme portalında oturum açın ve yeni aracı için bir aracı anahtarını yeniden oluşturma.

3. Yeni aracı anahtarını göndermek için yeni aracısının kullanıcı arabirimini kullanın.

4. İstemci aracısının daha önce kaydedilen şirket içi veritabanlarının listesini yüklerken bekleyin.

5. Veritabanı kimlik bilgileri olarak ulaşılamaz görüntülemek tüm veritabanları için sağlar. Bu veritabanları aracısının yüklü olduğu yeni bilgisayardan erişilebilir olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Tebrikler. Bir SQL veritabanı örneğini ve SQL Server veritabanı içeren bir eşitleme grubu oluşturdunuz.

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
