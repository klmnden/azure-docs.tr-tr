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
ms.reviewer: carlrab
manager: craigg
ms.date: 01/14/2019
ms.openlocfilehash: 82b85ffd685df52e702db15e5a5b57a53a3b4f64
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58486011"
---
# <a name="tutorial-set-up-sql-data-sync-between-azure-sql-database-and-sql-server-on-premises"></a>Öğretici: Şirket içi Azure SQL veritabanı ve SQL Server arasındaki SQL Data Sync'i ayarlama

Bu öğreticide, hem Azure SQL veritabanı ve SQL Server örneklerini içeren bir eşitleme grubu oluşturarak Azure SQL Data Sync'i ayarlama konusunda bilgi edinin. Eşitleme grubu, yapılandırılmış özel ve ayarladığınız zamanlamaya göre eşitler.

Bu öğretici, SQL veritabanı ve SQL Server ile konusunda en azından biraz deneyim sahibi varsayar.

SQL Data Sync genel bakış için bkz. [verileri Eşitle Bulut ve şirket içi veritabanı arasında Azure SQL Data Sync ile](sql-database-sync-data.md).

SQL Data Sync yapılandırma PowerShell örnekleri için bkz [Azure SQL veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-sql-databases.md) veya [Azure SQL veritabanı ile SQL Server şirket içi veritabanı](scripts/sql-database-sync-data-between-azure-onprem.md)

> [!IMPORTANT]
> Azure SQL Data Sync mu **değil** şu anda Azure SQL veritabanı yönetilen örneği destekler.

## <a name="create-sync-group"></a>Eşitleme grubu oluşturma

1. Tarayıcıda, Azure portalına gidin. SQL veritabanınız doğrudan Pano veya, select bulun **SQL veritabanları** simgesi araç çubuğunda ve **SQL veritabanları** sayfasında, veri eşitleme için hub veritabanı kullanmak istediğiniz veritabanını seçin.

    > [!NOTE]
    > Hub veritabanı bir eşitleme grubuna birden fazla veritabanı uç noktası olan bir eşitleme topolojisi ait Yönetim uç noktası. Uç noktaları eşitleme grubundaki diğer tüm üye veritabanlarını hub veritabanı ile eşitleyin.

1. Üzerinde **SQL veritabanı** Seçilen veritabanı için sayfa **diğer veritabanlarıyla Eşitle**.

    ![Diğer veritabanlarını seçeneğine eşitleme](media/sql-database-get-started-sql-data-sync/datasync-overview.png)

1. Üzerinde **diğer veritabanlarıyla Eşitle** sayfasında **yeni eşitleme grubu**. **Yeni eşitleme grubu** içeren sayfa açılır **eşitleme grubu oluşturma (1. adım)** vurgulanan.

   ![1. adım ayarları](media/sql-database-get-started-sql-data-sync/stepone.png)

   Üzerinde **veri eşitleme grubu oluşturma** sayfasında, aşağıdaki ayarları değiştirin:

   | Ayar                        | Açıklama |
   | ------------------------------ | ------------------------------------------------- |
   | **Eşitleme grubu adı** | Yeni eşitleme grubu için bir ad girin. Bu ad, veritabanı adından farklıdır. |
   | **Eşitleme meta verileri veritabanı** | (Önerilen) bir veritabanı oluşturun veya var olan bir veritabanını kullanmayı seçin.<br/><br/>Seçerseniz **yeni veritabanı**seçin **yeni veritabanı oluştur.** Ardından **SQL veritabanı** sayfasında, ad ve yeni veritabanını yapılandırın ve seçin **Tamam**.<br/><br/>Seçerseniz **varolan veritabanını kullan**, veritabanını listeden seçin. |
   | **Otomatik eşitleme** | Seçin **üzerinde** veya **kapalı**.<br/><br/>Seçerseniz **üzerinde**, bir sayı girin ve seçin **saniye**, **dakika**, **saat**, veya **gün** içinde **Eşitleme sıklığı** bölümü. |
   | **Çakışma çözümü** | Seçin **Hub kazanımı** veya **üye kazanımı**.<br/><br/>**Hub kazanımı** ne zaman anlamına gelir çakışmalar oluşur, hub veritabanındaki verileri çakışan üye veritabanı verilerini üzerine yazar.<br/><br/>**Üye kazanımı** ne zaman anlamına gelir çakışmalar oluşur, üye veritabanı verilerini hub veritabanındaki çakışan verilerin üzerine yazar. |

   > [!NOTE]
   > Microsoft öneriyor olarak kullanılmak üzere yeni, boş bir veritabanı oluşturmak için **eşitleme meta verileri veritabanı**. Veri eşitleme, bu veritabanında tablolar oluşturur ve sık kullanılan bir iş yükü çalıştırır. Bu veritabanı olarak paylaşılan **eşitleme meta verileri veritabanı** için tüm eşitleme gruplarını belirli bir bölgesine ve veritabanı ya da adını bölgede tüm eşitleme grupları ve eşitleme aracıları kaldırmadan değiştiremezsiniz.

   Seçin **Tamam** ve oluşturulan ve dağıtılan eşitleme grubu için bekleyin.

## <a name="add-sync-members"></a>Eşitleme üyesi ekleme

Yeni eşitleme grubu oluşturulur ve dağıtılır, sonra **(2. adım) eşitleme üyesi ekleme** üzerinde vurgulanır **yeni eşitleme grubu** sayfası.

İçinde **Hub veritabanı** bölümünde, hub veritabanının bulunduğu SQL veritabanı sunucusu için var olan kimlik bilgilerini girin. Girmeyin *yeni* Bu bölümde kimlik bilgileri.

![2. adım ayarları](media/sql-database-get-started-sql-data-sync/steptwo.png)

### <a name="to-add-an-azure-sql-database"></a>Azure SQL veritabanı eklemek için

İçinde **üye veritabanı** bölümüne, isteğe bağlı olarak seçerek Azure SQL veritabanı eşitleme grubuna ekleyin **Azure SQL veritabanı ekleme**. **Azure SQL veritabanını Yapılandır** sayfası açılır.

  ![Adım 2 - veritabanı yapılandırma](media/sql-database-get-started-sql-data-sync/steptwo-configure.png)

  Üzerinde **Azure SQL veritabanını Yapılandır** sayfasında, aşağıdaki ayarları değiştirin:

  | Ayar                       | Açıklama |
  | ----------------------------- | ------------------------------------------------- |
  | **Eşitleme üyesi adı** | Yeni eşitleme üyesi için bir ad belirtin. Bu ad, veritabanı adı kendisini farklıdır. |
  | **Abonelik** | Faturalandırma ve ilişkili Azure aboneliği seçin. |
  | **Azure SQL sunucusu** | Mevcut SQL veritabanı sunucusu seçin. |
  | **Azure SQL Veritabanı** | Mevcut bir SQL veritabanını seçin. |
  | **Eşitleme yönergeleri** | Seçin **iki yönlü eşitleme**, **hub'a**, veya **Hub'ından**. |
  | **Kullanıcı adı** ve **parola** | Üye veritabanının bulunduğu SQL veritabanı sunucusu için var olan kimlik bilgilerini girin. Girmeyin *yeni* Bu bölümde kimlik bilgileri. |

  Seçin **Tamam** oluşturulup dağıtıldığında yeni eşitleme üyesi için bekleyin.

<a name="add-on-prem"></a>
### <a name="to-add-an-on-premises-sql-server-database"></a>Bir şirket içi SQL Server veritabanı eklemek için

İçinde **üye veritabanı** bölümüne, isteğe bağlı olarak şirket içi SQL Server seçerek eşitleme grubuna ekleyin **bir şirket içi veritabanı Ekle**. **Yapılandırma şirket içi** sayfası açılır, burada, aşağıdakileri yapabilirsiniz:

1. Seçin **eşitleme Aracısı ağ geçidini seçin**. **Eşitleme Aracısı seçin** sayfası açılır.

   ![Bir eşitleme Aracısı oluşturma](media/sql-database-get-started-sql-data-sync/steptwo-agent.png)

1. Üzerinde **eşitleme Aracısı seçin** sayfasında, var olan bir aracıyı kullanan veya bir aracı oluşturmak isteyip istemediğinizi seçin.

   Seçerseniz **varolan aracıları**, var olan aracıyı listeden seçin.

   Seçerseniz **yeni bir aracı oluşturun**, şunları yapın:

   1. Sağlanan bağlantıdan veri eşitleme aracısını indirin ve SQL Server bulunduğu bilgisayara yükleyin. Aracıyı doğrudan da indirebilirsiniz [SQL Azure veri eşitleme Aracısı](https://www.microsoft.com/download/details.aspx?id=27693).

      > [!IMPORTANT]
      > Sunucuyla iletişim istemci Aracısı izin vermek için güvenlik duvarında giden TCP bağlantı noktası 1433 açmanız gerekmez.

   1. Aracı için bir ad girin.

   1. Seçin **oluşturma ve anahtar oluştur** ve aracı anahtarını panoya kopyalayın.

   1. Seçin **Tamam** kapatmak için **eşitleme Aracısı seçin** sayfası.

1. SQL Server bilgisayarında, bulun ve istemci eşitleme aracısını uygulamayı çalıştırın.

   ![İstemci Aracısı uygulaması verileri eşitleme](media/sql-database-get-started-sql-data-sync/datasync-preview-clientagent.png)

    1. Eşitleme Aracısı uygulamayı seçin **aracı anahtarını Gönder**. **Eşitleme meta verileri veritabanı yapılandırması** iletişim kutusu açılır.

    1. İçinde **eşitleme meta verileri veritabanı yapılandırması** iletişim kutusu, Azure portaldan kopyaladığınız aracı anahtarını yapıştırın. Ayrıca meta veri veritabanının bulunduğu Azure SQL veritabanı sunucusu için var olan kimlik bilgilerini sağlayın. (Bir meta veri veritabanı oluşturduysanız, bu hub veritabanı ile aynı sunucuda veritabanıdır.) Seçin **Tamam** ve yapılandırmasını tamamlamak bekleyin.

        ![Aracı anahtarını ve sunucu kimlik bilgilerini girin](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-enterkey.png)

        > [!NOTE]
        > Bir güvenlik duvarı hata alırsanız, SQL Server bilgisayarından gelen trafiğe izin vermek için Azure üzerinde bir güvenlik duvarı kuralı oluşturun. Kural Portalı'nda veya SQL Server Management Studio (SSMS) el ile oluşturabilirsiniz. < Hub_database_name > olarak adını girerek SSMS, Azure üzerinde hub veritabanına bağlanın. database.windows.net.

    1. Seçin **kaydetme** bir SQL Server veritabanı aracıyla kaydetmek için. **SQL Server Yapılandırması** iletişim kutusu açılır.

        ![Ekleme ve bir SQL Server veritabanı yapılandırma](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-adddb.png)

    1. İçinde **SQL Server Yapılandırması** iletişim kutusunda, SQL Server kimlik doğrulaması veya Windows kimlik doğrulaması kullanarak bağlanmak seçin. SQL Server kimlik doğrulamasını seçerseniz, var olan kimlik bilgilerini girin. SQL Server adını ve eşitleme ve seçmek için istediğiniz veritabanının adını sağlayın **Bağlantıyı Sına** ayarlarınızı test etmek için. Ardından **Kaydet** ve kayıtlı veritabanı listesinde görünür.

        ![Artık kayıtlı SQL Server veritabanı](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-dbadded.png)

    1. İstemci eşitleme aracı uygulamayı kapatın.

1. Portalında, üzerinde **yapılandırma şirket içi** sayfasında **veritabanını seçin**.

1. Üzerinde **Veritabanı Seç** sayfasında **eşitleme üyesi adı** alan, yeni eşitleme üyesi için bir ad sağlayın. Bu ad, veritabanı adından farklıdır. Veritabanını listeden seçin. İçinde **eşitleme yönergeleri** alanın, Seç **iki yönlü eşitleme**, **için Hub**, veya **gelen Hub**.

    ![Şirket içi veritabanı seçin](media/sql-database-get-started-sql-data-sync/datasync-preview-selectdb.png)

1. Seçin **Tamam** kapatmak için **Veritabanı Seç** sayfası. Ardından **Tamam** kapatmak için **yapılandırma şirket içi** sayfasında ve oluşturulup dağıtıldığında yeni eşitleme üyesi için bekleyin. Son olarak, seçin **Tamam** kapatmak için **eşitleme üyelerini seçin** sayfası.

> [!NOTE]
> SQL Data Sync ve yerel aracı bağlanmak için kullanıcı adınızı rolüne ekleme *DataSync_Executor*. Veri Eşitleme SQL Server örneğinde bu rolü oluşturur.

## <a name="configure-sync-group"></a>Eşitleme grubunu yapılandırma

Yeni eşitleme grubu üyeleri oluşturulan ve dağıtılan, sonra **yapılandırma eşitleme grubu (3. adım)** vurgulanan **yeni eşitleme grubu** sayfası.

![3. adım ayarları](media/sql-database-get-started-sql-data-sync/stepthree.png)

1. Üzerinde **tabloları** sayfasında bir veritabanı eşitleme grubuna üye listesinden seçin ve seçin **yenileme şema**.

1. Listeden eşitlemek istediğiniz tabloları seçin. Varsayılan olarak, tüm sütun seçilmedi, bu nedenle eşitleme istemediğiniz sütunları için onay kutusunu devre dışı bırakın. Seçili birincil anahtar sütunu olduğundan emin olun.

1. **Kaydet**’i seçin.

1. Varsayılan olarak, zamanlanmış veya el ile çalıştırılması kadar veritabanı eşitlenmedi. El ile eşitleme çalıştırmak için'select Azure portalında SQL veritabanınıza gidin **diğer veritabanlarıyla Eşitle**, eşitleme grubunu seçin. **Data Sync** sayfası açılır. **Eşitle**’yi seçin.

    ![El ile eşitleme](media/sql-database-get-started-sql-data-sync/datasync-sync.png)

## <a name="faq"></a>SSS

**Veri eşitleme, verilerimi ne sıklıkta eşitleyebilir misiniz?**

Eşitlemeler arasındaki en düşük süre beş dakikadır.

**SQL Data Sync tamamen tablolar oluşturur?**

Hedef veritabanında Eşitleme şeması tabloları eksikse, SQL Data Sync bunları seçtiğiniz sütunları oluşturur. Ancak, bu tam uygunluklu şemada aşağıdaki nedenlerle neden değil:

- Seçebileceğiniz sütun hedef tabloda oluşturulur. Seçili sütun yok sayılır.
- Yalnızca seçili olan sütunda dizin hedef tabloda oluşturulur. Seçili sütunları için bu dizinler göz ardı edilir.
- XML türü sütunlarda Dizin oluşturulmaz.
- Denetim kısıtlamalarında oluşturulmaz.
- Kaynak tablolarda Tetikleyicileri oluşturulmaz.
- Görünüm ve saklı yordam oluşturulmaz.

Bu kısıtlamalar nedeniyle, şunları öneririz:

- Üretim ortamları için tam uygunluklu şema kendiniz oluşturun.
- Hizmet ile denemeler yaparken otomatik sağlama özelliğini kullanın.

**Tablolar oluşturmak istemediğiniz neden görüyorum?**

Veri eşitleme, veritabanında değişiklik izleme için ek tablolar oluşturur. Bu silme veya veri eşitleme çalışmayı durduruyor.

**Verilerimi bir eşitleme sonrasında convergent mi?**

Olmayabilir. Eşitlemeler bir Hub, Hub'ına B ve c hub'a olduğu bir hub ve bağlı üç bileşenler (A, B ve C) ile bir eşitleme grubuna katılın Bir veritabanı için bir değişiklik yaptıysanız *sonra* Hub'ına kadar sonraki eşitleme görevi değişiklik B veya C veritabanına yazılan değil, bir eşitleme.

**Şema değişiklikleri bir eşitleme grubunda nasıl alabilirim?**

Oluşturun ve tüm şema değişiklikleri el ile aktarabilirsiniz.

1. Şema değişiklikleri el ile hub'ına ve tüm eşitleme üyelerine çoğaltın.
1. Eşitleme şemasını güncelleştirme.

Yeni tablolar ve sütunlar eklemek için:

Geçerli eşitleme yeni tablolar ve sütunlar etkisini ve eşitleme şemasına eklenir kadar veri eşitleme bunları yoksayar. Yeni veritabanı nesnelerini eklerken dizisi izleyin:

1. Yeni Tablo veya sütunları hub'ına ve tüm eşitleme üyeleri ekleyin.
1. Yeni tablolar ve sütunlar eşitleme şemasına ekleyin.
1. Başlangıç değerleri yeni tablolar ve sütunlar haline ekleniyor.

Bir sütunun veri türünü değiştirmek için:

Var olan bir sütunun veri türünü değiştirdiğinizde, Data Sync yeni değerleri eşitleme şemasında tanımlanan orijinal veri türünü uygun olarak çalışmaya devam eder. Örneğin, kaynak veritabanı türü değiştirirseniz **int** için **bigint**, veri eşitleme için çok büyük bir değer eklediğiniz tamamlanana kadar çalışmaya devam eder **int** veri türü. Değişiklik, hub ve tüm eşitleme üyeleri için şema değişikliği el ile çoğaltma ve eşitleme şemasını güncelleştirmek için.

**Nasıl dışarı aktarma ve veri eşitleme ile veritabanını içeri aktarma?**

Bir veritabanı olarak dışarı aktardıktan sonra bir *.bacpac* dosyası ve bir veritabanı oluşturmak, Data Sync yeni veritabanı kullanmak için aşağıdakileri dosyasını içeri aktarın:

1. Veri eşitleme nesneleri ve ek tablolar kullanarak yeni veritabanı temizleme [bu betik](https://github.com/vitomaz-msft/DataSyncMetadataCleanup/blob/master/Data%20Sync%20complete%20cleanup.sql). Betik, tüm gerekli veri eşitleme nesneleri veritabanından siler.
1. Yeni veritabanı ile eşitleme grubunu yeniden oluşturun. Eski eşitleme grubu artık ihtiyacınız yoksa, onu silin.

**İstemci aracısı bilgileri nerede bulabilirim?**

İstemci Aracısı hakkında sık sorulan sorular için bkz. [Aracı SSS](sql-database-data-sync-agent.md#agent-faq).

## <a name="next-steps"></a>Sonraki adımlar

Tebrikler. Hem SQL veritabanı örneğinde hem de SQL Server veritabanını içeren bir eşitleme grubu oluşturdunuz.

SQL Data Sync hakkında daha fazla bilgi için bkz.:

- [Verileri Azure SQL Data Sync için eşitleme Aracısı](sql-database-data-sync-agent.md)
- [En iyi uygulamalar](sql-database-best-practices-data-sync.md) ve [Azure SQL Data Sync ile ilgili sorunları giderme](sql-database-troubleshoot-data-sync.md)
- [SQL Data Sync'i Azure İzleyici ile izleme günlükleri](sql-database-sync-monitor-oms.md)
- [Transact-SQL ile eşitleme şemasını güncelleştirmek](sql-database-update-sync-schema.md) veya [PowerShell](scripts/sql-database-sync-update-schema.md)

SQL Veritabanı hakkında daha fazla bilgi için bkz.:

- [SQL Veritabanı'na Genel Bakış](sql-database-technical-overview.md)
- [Veritabanı Yaşam Döngüsü Yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
