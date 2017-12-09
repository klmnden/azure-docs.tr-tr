---
title: "Azure SQL veritabanı için SQL Server geçirmek için Azure veritabanı geçiş hizmeti kullanma | Microsoft Docs"
description: "SQL Server şirket içinden Azure SQL Azure veritabanı geçiş hizmetini kullanarak geçirmek öğrenin."
services: dms
author: HJToland3
ms.author: jtoland
manager: jhubbard
ms.reviewer: 
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 11/17/2017
ms.openlocfilehash: 3938af29caec99f076452529cbc5d93cf2c8802b
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="migrate-sql-server-to-azure-sql-database"></a>SQL Server Azure SQL veritabanına geçirme
Azure veritabanı geçiş hizmeti veritabanlarını Azure SQL veritabanı için bir şirket içi SQL Server örneğinden geçirmek için kullanabilirsiniz. Bu öğreticide, geçiş **Adventureworks2012** veritabanı Azure veritabanı geçiş hizmetini kullanarak şirket içi örneğini SQL Server 2016 (veya üstü) bir Azure SQL veritabanına geri yüklendi.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Veri geçiş Yardımcısı'nı kullanarak şirket içi veritabanınızı değerlendirin.
> * Örnek şeması veri geçiş Yardımcısı'nı kullanarak geçirin.
> * Azure veritabanı geçiş hizmeti örneği oluşturun.
> * Azure veritabanı geçiş hizmetini kullanarak bir geçiş projesi oluşturun.
> * Geçiş çalıştırın.
> * Geçiş izleyin.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiyi tamamlamak için aktarmanız gerekir:

- Karşıdan yükleme ve instanll [SQL Server 2016 veya sonraki](https://www.microsoft.com/sql-server/sql-server-downloads) (herhangi bir sürümünü).
- Varsayılan olarak SQL Server Express yüklemesi sırasında göre makalesindeki yönergeleri izleyerek devre dışıdır TCP/IP protokolünü etkinleştirin [etkinleştirmek veya devre dışı bir sunucu ağ protokolü](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
- Yapılandırma, [veritabanı altyapısı erişimi için Windows Güvenlik Duvarı](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Makalede ayrıntı izleyerek bunu Azure SQL veritabanı örneğinde bir örneğini oluşturmak [Azure portalında bir Azure SQL veritabanı oluşturma](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal).
- İndirme ve yükleme [veri geçiş Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 veya sonraki bir sürümü.
- Kullanarak, şirket içi kaynak sunucular için siteden siteye bağlantı sağlar Azure Resource Manager dağıtım modelini kullanarak Azure veritabanı geçiş hizmeti için bir VNET oluşturma [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Kaynak SQL Server örneğine bağlanmak için kullanılan kimlik bilgilerini sağlamak [denetim SUNUCUSUNA](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) izinleri.
- Hedef Azure SQL veritabanı örneğine bağlanmak için kullanılan kimlik bilgilerini hedef Azure SQL veritabanlarına CONTROL DATABASE izninizin olduğundan emin olun.
- SQL Server kaynağına erişmek Azure veritabanı geçiş hizmeti, Windows Güvenlik Duvarı'nı açın.

## <a name="assess-your-on-premises-database"></a>Şirket içi veritabanınızı değerlendirin
Azure SQL veritabanı için bir şirket içi SQL Server örneğinden verileri geçirmeden önce SQL Server veritabanı için geçişe engel olabilecek herhangi bir engelleyici soruna değerlendirmeniz gerekir. Veri geçiş Yardımcısı v3.3 veya sonraki sürümlerde, makalesinde açıklanan adımları izleyin [bir SQL Server Geçiş değerlendirmesi gerçekleştirmek](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem) tamamlamak için şirket içi değerlendirme veritabanı. Gerekli adımları özetini aşağıdaki gibidir:
1.  Veri geçiş Yardımcısı, New (+) simgesini seçin ve ardından **değerlendirme** proje türü.
2.  Bir proje adı belirtin **kaynak sunucu türünü** metin kutusunda **SQL Server**ve ardından **hedef sunucu türü** metin kutusunda **Azure SQL Veritabanı**.
3.  Seçin **oluşturma** projesi oluşturmak için.

    Azure SQL veritabanına geçirme kaynak SQL Server veritabanı değerlendirirken birini veya her ikisini değerlendirme rapor türlerinden birini seçebilirsiniz:
    - Veritabanı uyumluluğu denetle
    - Özellik eşliği denetleyin

    Her iki rapor türünü varsayılan olarak seçilidir.
4.  Veri geçiş Yardımcısı, üzerinde **seçenekleri** ekran, select **sonraki**.
5.  Üzerinde **seçin kaynakları** ekranında **bir sunucuya Bağlan** iletişim kutusu, SQL Server'ınızı bağlantı ayrıntılarını girin ve ardından **Bağlan**.
6.  İçinde **ekleme kaynakları** iletişim kutusunda **AdventureWorks2012**seçin **Ekle**ve ardından **Başlat değerlendirme**.

    Değerlendirme tamamlandığında, sonuçları aşağıdaki grafikte gösterildiği gibi görüntüleyin:

    ![Veri geçişi değerlendirmek](media\tutorial-sql-server-to-azure-sql\dma-assessments.png)

    Azure SQL veritabanı için Değerlendirmeler geçiş engelleme ve özellik eşliği sorunlarını tanımlayın.

7.  Belirli seçenekleri belirleyerek geçiş engelleme ve özellik eşliği sorunlarını değerlendirme sonuçlarını gözden geçirin.
    - **SQL Server özellik eşliği** Azaltıcı adımlar çaba geçiş projelerinizi planlamanıza yardımcı olması için ve kategorisi öneriler, Azure içinde kullanılabilir alternatif yaklaşımlar kapsamlı bir kümesini sağlar.
    - **Uyumluluk sorunları** kategori tanımlayan kısmen desteklenen veya uyumluluk sorunlarını yansıtır desteklenmeyen özellikler, Azure SQL veritabanına geçirme şirket içi SQL Server veritabanları engelleyebilir. Öneriler de bu sorunları gidermek amacıyla sağlanmıştır.


## <a name="migrate-the-sample-schema"></a>Örnek şeması geçirme
Değerlendirmesi ile deneyimliyseniz ve seçili veritabanını Azure SQL veritabanı geçiş için iyi bir aday olduğunu memnun sonra Azure SQL veritabanı için şema geçirmek için veri geçiş Yardımcısı'nı kullanın.

> [!NOTE]
> Bir geçiş proje veri geçiş Yardımcısı'nda oluşturmadan önce zaten bir Azure SQL veritabanı Önkoşullar belirtildiği gibi sağladığınız olduğundan emin olun. Bu öğreticinin amaçları doğrultusunda, Azure SQL veritabanı adı olduğu varsayılarak **AdventureWorksAzure**, ancak isterseniz, farklı şekilde adlandırabilirsiniz.

Geçirilecek **AdventureWorks2012** Azure SQL Database, şemaya aşağıdaki adımları gerçekleştirin:

1.  Veri geçiş Yardımcısı'nda, New (+) simgesini seçin ve ardından **proje türü**seçin **geçiş**.
3.  Bir proje adı belirtin **kaynak sunucu türünü** metin kutusunda **SQL Server**ve ardından **hedef sunucu türü** metin kutusunda **Azure SQL Veritabanı**.
4.  Altında **geçiş kapsam**seçin **yalnızca şema**.

    Önceki adımları gerçekleştirdikten sonra veri geçiş Yardımcısı arabirimi aşağıdaki grafikte gösterildiği gibi görünmelidir:
    
    ![Veri geçiş Yardımcısı projesi oluşturma](media\tutorial-sql-server-to-azure-sql\dma-create-project.png)

5.  Seçin **oluşturma** projesi oluşturmak için.
6.  Veri geçiş Yardımcısı'nda, kaynak bağlantı ayrıntılarını seçin, SQL Server için belirtmek **Bağlan**ve ardından **AdventureWorks2012** veritabanı.

    ![Veri geçiş Yardımcısı kaynağı bağlantı ayrıntıları](media\tutorial-sql-server-to-azure-sql\dma-source-connect.png)
7.  Seçin **sonraki**altında **hedef sunucuya Bağlan**, Azure SQL veritabanı için hedef bağlantı ayrıntılarını belirt, seçin **Bağlan**ve ardından **AdventureWorksAzure** önceden sağladığınız Azure SQL veritabanında veritabanı.

    ![Veri geçiş Yardımcısı hedef bağlantı ayrıntıları](media\tutorial-sql-server-to-azure-sql\dma-target-connect.png)
8.  Seçin **sonraki** ilerletmek için **nesneleri seçin** üzerinde belirtebilirsiniz şema nesnelerindeki ekran **AdventureWorks2012** Azure'a dağıtılması gerekiyor veritabanı SQL veritabanı.

    Varsayılan olarak, tüm nesneler seçilir.

    ![SQL komut dosyaları oluşturmak](media\tutorial-sql-server-to-azure-sql\dma-assessment-source.png)
9.  Seçin **oluştur SQL komut dosyası** SQL komut dosyaları oluşturmak ve tüm hatalar için komut dosyalarını gözden geçirin.

    ![Şema komut dosyası](media\tutorial-sql-server-to-azure-sql\dma-schema-script.png)
10. Seçin **dağıtma şema** Azure SQL veritabanı için şema dağıtıp şema dağıtıldıktan sonra hedef sunucu herhangi anormallikleri için kontrol edin.

    ![Şema dağıtma](media\tutorial-sql-server-to-azure-sql\dma-schema-deploy.png)

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını Kaydet
1. Azure portalında, select oturum açma **tüm hizmetleri**ve ardından **abonelikleri**.
 
   ![Portal abonelikleri Göster](media\tutorial-sql-server-to-azure-sql\portal-select-subscription.png)
       
2. İçinde Azure veritabanı geçiş hizmeti örneğini oluşturun ve ardından istediğiniz aboneliği seçin **kaynak sağlayıcıları**.
 
    ![kaynak sağlayıcıları göster](media\tutorial-sql-server-to-azure-sql\portal-select-resource-provider.png)    
3.  Arama geçiş ve ardından sağ tarafındaki **Microsoft.DataMigration**seçin **kaydetmek**.
 
    ![Kaynak sağlayıcısını kaydetme](media\tutorial-sql-server-to-azure-sql\portal-register-resource-provider.png)    


## <a name="create-an-instance"></a>Bir örneği oluşturma
1.  Azure portalında seçin **+ kaynak oluşturma**aramak için Azure veritabanı geçiş hizmeti ve ardından **Azure veritabanı geçiş hizmeti** aşağı açılan listeden.

    ![Azure Market](media\tutorial-sql-server-to-azure-sql\portal-marketplace.png)
2.  Üzerinde **Azure veritabanı geçiş hizmeti (Önizleme)** ekran, select **oluşturma**.
 
    ![Azure veritabanı geçiş hizmet örneği oluşturma](media\tutorial-sql-server-to-azure-sql\dms-create.png)
  
3.  Üzerinde **veritabanı geçiş hizmeti** ekranında, hizmet, abonelik, bir sanal ağ ve fiyatlandırma katmanı için bir ad belirtin.

    Maliyetleri ve fiyatlandırma katmanları hakkında daha fazla bilgi için bkz: [fiyatlandırma sayfası](https://aka.ms/dms-pricing).

     ![Azure veritabanı geçiş hizmeti örneği ayarlarını yapılandır](media\tutorial-sql-server-to-azure-sql\dms-settings.png)

4.  Seçin **oluşturma** hizmeti oluşturmak için.

## <a name="create-a-migration-project"></a>Bir geçiş projesi oluşturma
Hizmet oluşturulduktan sonra Azure portalını bulun ve sonra geçiş projesi oluşturun.
1. Azure portalında seçin **tüm hizmetleri**aramak için Azure veritabanı geçiş hizmeti ve ardından **Azure veritabanı geçiş hizmetleri**.
 
      ![Azure veritabanı geçiş hizmeti tüm örneklerini bulun](media\tutorial-sql-server-to-azure-sql\dms-search.png)
2. Üzerinde **Azure veritabanı geçiş hizmetleri** ekranında, Azure DMS adını örneği için oluşturduğunuz aramak ve örneği seçin.
 
     ![Azure veritabanı geçiş hizmeti örneğiniz bulun](media\tutorial-sql-server-to-azure-sql\dms-instance-search.png)
 
3. Seçin **+ yeni geçiş proje**.
4. Üzerinde **yeni geçiş proje** ekranında, proje için bir ad belirtin **kaynak sunucu türünü** metin kutusunda **SQL Server**ve ardından **hedef Sunucu türü** metin kutusunda **Azure SQL veritabanı**.

    ![Veritabanı geçiş hizmeti projesi oluşturma](media\tutorial-sql-server-to-azure-sql\dms-create-project.png)

5.  Seçin **oluşturma** projesi oluşturmak için.


## <a name="specify-source-details"></a>Kaynak ayrıntıları belirtin
1. Üzerinde **kaynak ayrıntıları** ekranında, SQL Server kaynak bağlantı ayrıntılarını belirtin.

    ![Kaynak seçin](media\tutorial-sql-server-to-azure-sql\dms-select-source.png)

2. Seçin **kaydetmek**ve ardından **AdventureWorks2012** geçiş için veritabanı.

    ![Kaynak DB seçin](media\tutorial-sql-server-to-azure-sql\dms-select-source-db.png)

## <a name="specify-target-details"></a>Hedef ayrıntıları belirtin
1. Seçin **kaydetmek**ve ardından **hedef ayrıntıları** ekranında, önceden sağlanan Azure SQL veritabanı için hedef bağlantı ayrıntılarını belirt **AdventureWorks2012**  şema veri geçiş Yardımcısı'nı kullanarak dağıtıldı.

    ![Hedef seçin](media\tutorial-sql-server-to-azure-sql\dms-select-target.png)

2. Seçin **kaydetmek** projeyi kaydetmek için.
3. Üzerinde **geçiş Özet** ekranında, gözden geçirin ve geçiş projeyle ilişkili ayrıntılarını doğrulayın.

    ![DMS özeti](media\tutorial-sql-server-to-azure-sql\dms-summary.png)

4. **Kaydet**’i seçin.

## <a name="run-the-migration"></a>Geçişi çalıştırma
1.  En son kaydedilen proje seçin, **+ yeni etkinlik**ve ardından **veri geçişi çalıştırma**.

    ![Yeni Etkinlik](media\tutorial-sql-server-to-azure-sql\dms-new-activity.png)

2.  İstendiğinde, kaynak ve hedef sunucular için kimlik bilgilerini girin ve ardından **kaydetmek**.
3.  Üzerinde **hedef veritabanlarına harita** ekranında, kaynak ve hedef veritabanı geçiş için harita.

    Hedef veritabanı kaynak veritabanının veritabanı adıyla aynı içeriyorsa, Azure DMS hedef veritabanı varsayılan olarak seçer.

    ![Hedef veritabanlarına eşleme](media\tutorial-sql-server-to-azure-sql\dms-map-targets-activity.png)

4. Seçin **kaydetmek**, **tabloları seçme** ekranında, Tablo listesini genişletin ve etkilenen alanlarının listesini gözden geçirin.

    ![Tabloları seçme](media\tutorial-sql-server-to-azure-sql\dms-configure-setting-activity.png)

5.  Seçin **kaydetmek**, **geçiş Özet** ekranında **etkinlik adı** metin kutusunda, geçiş etkinliği için bir ad belirtin.

    Bu ekranda genişletebilirsiniz **doğrulama seçeneğini** geçirilen veritabanı için doğrulamak için belirtmek için kullanabileceğiniz ekran:
    - Şema
    - Veri tutarlılığı
    - Sorgu doğruluk ve performans

    ![Doğrulama seçeneği](media\tutorial-sql-server-to-azure-sql\dms-configuration.png)

6.  Seçin **kaydetmek**, kaynak ve hedef ayrıntıları ne daha önce belirttiğiniz eşleştiğinden emin olmak için özeti gözden geçirin.

    ![Geçiş özeti](media\tutorial-sql-server-to-azure-sql\dms-run-migration.png)

7.  Seçin **geçişi çalıştırma** geçiş etkinliğini başlatın ve ardından **yenileme** geçerli durumunu gözden geçirmek için.

    ![Etkinlik Durumu](media\tutorial-sql-server-to-azure-sql\dms-activity-status.png)

## <a name="monitor-the-migration"></a>Geçiş izleme
1. Etkinlik durumunu gözden geçirmek için geçiş etkinliği seçin.
2. Geçiş işlemi tamamlandıktan sonra hedef Azure SQL veritabanı doğrulayın.

    ![Tamamlandı](media\tutorial-sql-server-to-azure-sql\dms-completed-activity.png)
