---
title: Azure SQL veritabanı için SQL Server geçirmek için Azure veritabanı geçiş hizmeti kullanma | Microsoft Docs
description: SQL Server şirket içinden Azure veritabanı geçiş hizmetini kullanarak Azure SQL veritabanına geçirme öğrenin.
services: dms
author: edmacauley
ms.author: edmaca
manager: craigg
ms.reviewer: ''
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 06/06/2018
ms.openlocfilehash: 823f785bf33fd4d227fbdbb0f3c6b5eec914e08c
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34808018"
---
# <a name="migrate-sql-server-to-azure-sql-database-using-dms"></a>DMS kullanarak Azure SQL veritabanı için SQL Server geçirme
Azure veritabanı geçiş hizmeti veritabanlarını şirket içi SQL Server örneğine geçirmek için kullanabileceğiniz [Azure SQL veritabanı](https://docs.microsoft.com/en-us/azure/sql-database/). Bu öğreticide, geçiş **Adventureworks2012** SQL Server 2016 (veya üstü) şirket içi örneğine geri yüklenen veritabanı Azure veritabanı geçiş hizmetini kullanarak bir Azure SQL veritabanı.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Veri geçiş Yardımcısı'nı kullanarak şirket içi veritabanınızı değerlendirin.
> * Örnek şeması veri geçiş Yardımcısı'nı kullanarak geçirin.
> * Azure veritabanı geçiş hizmeti örneği oluşturun.
> * Azure veritabanı geçiş hizmetini kullanarak bir geçiş projesi oluşturun.
> * Geçiş çalıştırın.
> * Geçiş izleyin.
> * Bir geçiş raporu indirin.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aktarmanız gerekir:

- İndirme ve yükleme [SQL Server 2016 veya sonraki](https://www.microsoft.com/sql-server/sql-server-downloads) (herhangi bir sürümünü).
- Varsayılan olarak SQL Server Express yüklemesi sırasında göre makalesindeki yönergeleri izleyerek devre dışıdır TCP/IP protokolünü etkinleştirin [etkinleştirmek veya devre dışı bir sunucu ağ protokolü](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
- Makalede ayrıntı izleyerek bunu Azure SQL veritabanı örneğinde bir örneğini oluşturmak [Azure portalında bir Azure SQL veritabanı oluşturma](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal).
- İndirme ve yükleme [veri geçiş Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 veya sonraki bir sürümü.
- Kullanarak, şirket içi kaynak sunucular için siteden siteye bağlantı sağlar Azure Resource Manager dağıtım modelini kullanarak Azure veritabanı geçiş hizmeti için bir VNET oluşturma [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Azure sanal ağ (VNET) ağ güvenlik grubu kuralları blok aşağıdaki iletişim bağlantı noktaları 443, 53, 9354, 445, 12000. Azure VNET NSG trafik filtreleme daha ayrıntılı bilgi için bkz: [filtre ağ güvenlik grupları ile ağ trafiği](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Yapılandırma, [veritabanı altyapısı erişimi için Windows Güvenlik Duvarı](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- TCP bağlantı noktası 1433 varsayılan SQL Server kaynağına erişmek Azure veritabanı geçiş hizmeti, Windows Güvenlik Duvarı'nı açın.
- Dinamik bağlantı noktaları kullanan birden fazla adlandırılmış SQL Server örneklerini çalıştırıyorsanız, SQL Tarayıcı Hizmeti'ni etkinleştir ve böylece Azure veritabanı geçiş hizmeti kaynağınız adlandırılmış bir örnekte bağlanabilir, güvenlik duvarları üzerinden UDP bağlantı noktası 1434 erişmesine izin vermek isteyebilir Sunucu.
- Bir güvenlik duvarı gerecini kaynak veritabanları önünde kullanırken, geçiş için kaynak veritabanlarının erişmek Azure veritabanı geçiş hizmeti izin veren güvenlik duvarı kuralları eklemeniz gerekebilir.
- Sunucu düzeyinde oluşturma [güvenlik duvarı kuralı](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) Azure SQL veritabanı sunucusunun hedef veritabanlarına Azure veritabanı geçiş hizmeti erişmesine izin vermek. Azure veritabanı geçiş hizmeti için kullanılan sanal ağ alt aralığını belirtin.
- Kaynak SQL Server örneğine bağlanmak için kullanılan kimlik bilgilerini sağlamak [denetim SUNUCUSUNA](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) izinleri.
- Hedef Azure SQL veritabanı örneğine bağlanmak için kullanılan kimlik bilgilerini hedef Azure SQL veritabanlarına CONTROL DATABASE izninizin olduğundan emin olun.

## <a name="assess-your-on-premises-database"></a>Şirket içi veritabanınızı değerlendirin
Azure SQL veritabanı için bir şirket içi SQL Server örneğinden verileri geçirmeden önce SQL Server veritabanı için geçişe engel olabilecek herhangi bir engelleyici soruna değerlendirmeniz gerekir. Veri geçiş Yardımcısı v3.3 kullanarak ya da daha sonra makalede açıklanan adımları izleyerek [bir SQL Server Geçiş değerlendirmesi gerçekleştirmek](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem) tamamlamak için şirket içi değerlendirme veritabanı. Gerekli adımları özetini aşağıdaki gibidir:
1.  Veri geçiş Yardımcısı, New (+) simgesini seçin ve ardından **değerlendirme** proje türü.
2.  Bir proje adı belirtin **kaynak sunucu türünü** metin kutusunda **SQL Server**, **hedef sunucu türü** metin kutusunda **Azure SQL veritabanı**ve ardından **oluşturma** projesi oluşturmak için.

    Azure SQL veritabanına geçirme kaynak SQL Server veritabanı değerlendirirken birini veya her ikisini değerlendirme rapor türlerinden birini seçebilirsiniz:
    - Veritabanı uyumluluğunu denetleyin
    - Özellik eşliğini denetle

    Her iki rapor türünü varsayılan olarak seçilidir.

3.  Veri geçiş Yardımcısı, üzerinde **seçenekleri** ekran, select **sonraki**.
4.  Üzerinde **seçin kaynakları** ekranında **bir sunucuya Bağlan** iletişim kutusu, SQL Server'ınızı bağlantı ayrıntılarını girin ve ardından **Bağlan**.
5.  İçinde **ekleme kaynakları** iletişim kutusunda **AdventureWorks2012**seçin **Ekle**ve ardından **Başlat değerlendirme**.

    Değerlendirme tamamlandığında, sonuçları aşağıdaki grafikte gösterildiği gibi görüntüleyin:

    ![Veri geçişi değerlendirmek](media\tutorial-sql-server-to-azure-sql\dma-assessments.png)

    Azure SQL veritabanı için Değerlendirmeler özellik eşliği ve geçiş engelleme sorunlarını tanımlayın.

    - **SQL Server özellik eşliği** Azaltıcı adımlar çaba geçiş projelerinizi planlamanıza yardımcı olması için ve kategorisi öneriler, Azure içinde kullanılabilir alternatif yaklaşımlar kapsamlı bir kümesini sağlar.
    - **Uyumluluk sorunları** kategori tanımlayan kısmen desteklenen veya uyumluluk sorunlarını yansıtır desteklenmeyen özellikler, Azure SQL veritabanına geçirme şirket içi SQL Server veritabanları engelleyebilir. Öneriler de bu sorunları gidermek amacıyla sağlanmıştır.

6.  Belirli seçenekleri belirleyerek geçiş engelleme ve özellik eşliği sorunlarını değerlendirme sonuçlarını gözden geçirin.

## <a name="migrate-the-sample-schema"></a>Örnek şeması geçirme
Değerlendirmesi ile deneyimliyseniz ve seçili veritabanını Azure SQL veritabanı geçiş için uygun bir aday olduğunu memnun sonra Azure SQL veritabanı için şema geçirmek için veri geçiş Yardımcısı'nı kullanın.

> [!NOTE]
> Bir geçiş proje veri geçiş Yardımcısı'nda oluşturmadan önce zaten bir Azure SQL veritabanı Önkoşullar belirtildiği gibi sağladığınız olduğundan emin olun. Bu öğreticinin amaçları doğrultusunda, Azure SQL veritabanı adı olduğu varsayılarak **AdventureWorksAzure**, ancak istediğiniz ad sağlayabilir.

Geçirilecek **AdventureWorks2012** Azure SQL Database, şemaya aşağıdaki adımları gerçekleştirin:

1.  Veri geçiş Yardımcısı'nda, New (+) simgesini seçin ve ardından **proje türü**seçin **geçiş**.
2.  Bir proje adı belirtin **kaynak sunucu türünü** metin kutusunda **SQL Server**ve ardından **hedef sunucu türü** metin kutusunda **Azure SQL Veritabanı**.
3.  Altında **geçiş kapsam**seçin **yalnızca şema**.

    Önceki adımları gerçekleştirdikten sonra veri geçiş Yardımcısı arabirimi aşağıdaki grafikte gösterildiği gibi görünmelidir:
    
    ![Veri geçiş Yardımcısı projesi oluşturma](media\tutorial-sql-server-to-azure-sql\dma-create-project.png)

4.  Seçin **oluşturma** projesi oluşturmak için.
5.  Veri geçiş Yardımcısı'nda, kaynak bağlantı ayrıntılarını seçin, SQL Server için belirtmek **Bağlan**ve ardından **AdventureWorks2012** veritabanı.

    ![Veri geçiş Yardımcısı kaynağı bağlantı ayrıntıları](media\tutorial-sql-server-to-azure-sql\dma-source-connect.png)

6.  Seçin **sonraki**altında **hedef sunucuya Bağlan**, Azure SQL veritabanı için hedef bağlantı ayrıntılarını belirt, seçin **Bağlan**ve ardından **AdventureWorksAzure** önceden sağladığınız Azure SQL veritabanında veritabanı.

    ![Veri geçiş Yardımcısı hedef bağlantı ayrıntıları](media\tutorial-sql-server-to-azure-sql\dma-target-connect.png)

7.  Seçin **sonraki** ilerletmek için **nesneleri seçin** üzerinde belirtebilirsiniz şema nesnelerindeki ekran **AdventureWorks2012** Azure'a dağıtılması gerekiyor veritabanı SQL veritabanı.

    Varsayılan olarak, tüm nesneler seçilir.

    ![SQL komut dosyaları oluşturmak](media\tutorial-sql-server-to-azure-sql\dma-assessment-source.png)

8.  Seçin **oluştur SQL komut dosyası** SQL komut dosyaları oluşturmak ve tüm hatalar için komut dosyalarını gözden geçirin.

    ![Şema komut dosyası](media\tutorial-sql-server-to-azure-sql\dma-schema-script.png)

9.  Seçin **dağıtma şema** Azure SQL veritabanı için şema dağıtıp şema dağıtıldıktan sonra hedef sunucu herhangi anormallikleri için kontrol edin.

    ![Şema dağıtma](media\tutorial-sql-server-to-azure-sql\dma-schema-deploy.png)

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını Kaydet
1. Azure portalında, select oturum açma **tüm hizmetleri**ve ardından **abonelikleri**.
 
   ![Portal abonelikleri Göster](media\tutorial-sql-server-to-azure-sql\portal-select-subscription1.png)
       
2. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve sonra **Kaynak sağlayıcıları**’nı seçin.
 
    ![kaynak sağlayıcıları göster](media\tutorial-sql-server-to-azure-sql\portal-select-resource-provider.png)
    
3.  Arama geçiş ve ardından sağ tarafındaki **Microsoft.DataMigration**seçin **kaydetmek**.
 
    ![Kaynak sağlayıcısını kaydetme](media\tutorial-sql-server-to-azure-sql\portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Bir örneği oluşturma
1.  Azure portalında seçin + **kaynak oluşturma**aramak için Azure veritabanı geçiş hizmeti ve ardından **Azure veritabanı geçiş hizmeti** aşağı açılan listeden.

    ![Azure Market](media\tutorial-sql-server-to-azure-sql\portal-marketplace.png)

2.  Üzerinde **Azure veritabanı geçiş hizmeti** ekran, select **oluşturma**.
 
    ![Azure veritabanı geçiş hizmet örneği oluşturma](media\tutorial-sql-server-to-azure-sql\dms-create1.png)
  
3.  Üzerinde **geçiş hizmet oluşturma** ekranında, hizmetin, abonelik ve yeni veya var olan kaynak grubu için bir ad belirtin.

4. Varolan bir sanal ağ (VNET) seçin veya yeni bir tane oluşturun.

    VNET Azure veritabanı geçiş hizmeti SQL Server kaynak erişimi olan ve hedef Azure SQL veritabanı örneğinde sağlar.

    Azure portalında VNET oluşturma hakkında daha fazla bilgi için bkz: [Azure portalını kullanarak bir sanal ağ oluşturma](https://aka.ms/DMSVnet).

5. Fiyatlandırma katmanını seçin.

    Maliyetleri ve fiyatlandırma katmanları hakkında daha fazla bilgi için bkz: [fiyatlandırma sayfası](https://aka.ms/dms-pricing).

    Nakil önerileri sağ Azure veritabanı geçiş hizmeti katmanı seçme özelliği de yardıma gereksinim duyarsanız, başvurmak [burada](https://go.microsoft.com/fwlink/?linkid=861067).  

     ![Azure veritabanı geçiş hizmeti örneği ayarlarını yapılandır](media\tutorial-sql-server-to-azure-sql\dms-settings1.png)

6.  Seçin **oluşturma** hizmeti oluşturmak için.

## <a name="create-a-migration-project"></a>Bir geçiş projesi oluşturma
Hizmet oluşturulduktan sonra Azure portalını bulun, açın ve ardından yeni bir geçiş projesi oluşturun.

1. Azure portalında seçin **tüm hizmetleri**aramak için Azure veritabanı geçiş hizmeti ve ardından **Azure veritabanı geçiş hizmetleri**.
 
      ![Azure veritabanı geçiş hizmeti tüm örneklerini bulun](media\tutorial-sql-server-to-azure-sql\dms-search.png)

2. Üzerinde **Azure veritabanı geçiş hizmetleri** ekranında, Azure veritabanı geçiş hizmeti adı örneği için oluşturduğunuz aramak ve örneği seçin.
 
     ![Azure veritabanı geçiş hizmeti örneğiniz bulun](media\tutorial-sql-server-to-azure-sql\dms-instance-search.png)
 
3. Seçin + **yeni geçiş proje**.
4. Üzerinde **yeni geçiş proje** ekranında, proje için bir ad belirtin **kaynak sunucu türünü** metin kutusunda **SQL Server**ve ardından **hedef Sunucu türü** metin kutusunda **Azure SQL veritabanı**.

    ![Veritabanı geçiş hizmeti projesi oluşturma](media\tutorial-sql-server-to-azure-sql\dms-create-project1.png)

5.  Seçin **oluşturma** projesi oluşturmak için.

## <a name="specify-source-details"></a>Kaynak ayrıntıları belirtin
1. Üzerinde **kaynak ayrıntıları** ekranında, kaynak SQL Server örneği için bağlantı ayrıntıları belirtin.
 
    Bir tam etki alanı adı (FQDN) için kaynak SQL Server örnek adı kullandığınızdan emin olun. DNS ad çözümlemesi mümkün olmadığı durumlar için IP adresini de kullanabilirsiniz.

2. Kaynak sunucunuzda güvenilen bir sertifika yüklemediyseniz seçin **güven sunucu sertifikası** onay kutusu.

    Güvenilen bir sertifika yüklendiğinde değil, SQL Server örneği başlatıldığında otomatik olarak imzalanan bir sertifika oluşturur. Bu sertifika, istemci bağlantıları için kimlik bilgilerini şifrelemek için kullanılır.

    > [!CAUTION]
    > Kendinden imzalı bir sertifika kullanarak encyopted SSL bağlantı güçlü güvenlik sağlamaz. Bunlar man-in--middle saldırılarına açıktır. Bir üretim ortamında otomatik olarak imzalanan sertifikalar kullanarak SSL veya internet'e bağlı sunucularda güvenmemelisiniz.

   ![Kaynak Ayrıntıları](media\tutorial-sql-server-to-azure-sql\dms-source-details1.png)
  
2. Seçin **kaydetmek**ve ardından **AdventureWorks2012** geçiş için veritabanı.

    ![Kaynak DB seçin](media\tutorial-sql-server-to-azure-sql\dms-select-source-db1.png)

## <a name="specify-target-details"></a>Hedef ayrıntıları belirtin
1. Seçin **kaydetmek**ve ardından **hedef ayrıntıları** ekranında, önceden sağlanan Azure SQL veritabanı olan Azure SQL veritabanı sunucusu, hedef bağlantı ayrıntılarını belirt **AdventureWorks2012** şema veri geçiş Yardımcısı'nı kullanarak dağıtıldı.

    ![Hedef seçin](media\tutorial-sql-server-to-azure-sql\dms-select-target1.png)

2. Seçin **kaydetmek** projeyi kaydetmek için.

3. Üzerinde **Proje Özeti** ekranında, gözden geçirin ve geçiş projeyle ilişkili ayrıntılarını doğrulayın.

    ![DMS özeti](media\tutorial-sql-server-to-azure-sql\dms-summary1.png)

4. **Kaydet**’i seçin.

## <a name="run-the-migration"></a>Geçişi çalıştırma
1.  En son kaydedilen proje seçin, + **yeni etkinlik**ve ardından **geçişi çalıştırma**.

    ![Yeni Etkinlik](media\tutorial-sql-server-to-azure-sql\dms-new-activity1.png)

2.  İstendiğinde, kaynak ve hedef sunucular için kimlik bilgilerini girin ve ardından **kaydetmek**.

3.  Üzerinde **hedef veritabanlarına harita** ekranında, kaynak ve hedef veritabanı geçiş için harita.

    Hedef veritabanı kaynak veritabanının veritabanı adıyla aynı içeriyorsa, Azure DMS hedef veritabanı varsayılan olarak seçer.

    ![Hedef veritabanlarıyla eşleyin](media\tutorial-sql-server-to-azure-sql\dms-map-targets-activity1.png)

4. Seçin **kaydetmek**, **tabloları seçme** ekranında, listeleyen bir tablo genişletin ve sonra etkilenen alanlarının listesini gözden geçirin.

    Azure veritabanı geçiş hizmeti otomatik hedef Azure SQL veritabanı örneğinde mevcut tüm boş kaynak tablolarını seçer unutmayın. Zaten verileri içeren tablolar yeniden geçirmek istiyorsanız, bu dikey tablolarda açıkça seçmeniz gerekir.

    ![Tabloları seçme](media\tutorial-sql-server-to-azure-sql\dms-configure-setting-activity1.png)

5.  Seçin **kaydetmek**, **geçiş Özet** ekranında **etkinlik adı** metin kutusunda, geçiş etkinliği için bir ad belirtin.

6. Genişletme **doğrulama seçeneği** görüntülemek için bölüm **doğrulama seçeneğini** ekranında, geçirilen veritabanları için doğrulanıp doğrulanmayacağını belirtin **Şema karşılaştırma**, **Veri tutarlılığını**, ve **sorgu doğruluk**.
    
    ![Doğrulama seçeneğini belirleyin](media\tutorial-sql-server-to-azure-sql\dms-configuration1.png)

6.  Seçin **kaydetmek**, kaynak ve hedef ayrıntıları ne daha önce belirttiğiniz eşleştiğinden emin olmak için özeti gözden geçirin.

    ![Geçiş özeti](media\tutorial-sql-server-to-azure-sql\dms-run-migration1.png)

7.  Seçin **geçişi çalıştırma**.

    Geçiş etkinliği penceresi görünür ve **durum** etkinliğini olan **bekleyen**.

    ![Etkinlik Durumu](media\tutorial-sql-server-to-azure-sql\dms-activity-status1.png)

## <a name="monitor-the-migration"></a>Geçiş izleme
1. Geçiş etkinliği ekranında seçin **yenileme** kadar görüntü güncelleştirmek için **durum** geçişini gösterildiğinden **tamamlandı**.

    ![Etkinlik durumunu tamamlandı](media\tutorial-sql-server-to-azure-sql\dms-completed-activity1.png)

2. Geçiş tamamlandıktan sonra Seç **karşıdan rapor** geçiş işlemle ilişkili ayrıntıları listeleyen bir rapor almak için.

3. Hedef Azure SQL veritabanı sunucusunda hedef veritabanlarının doğrulayın.

### <a name="additional-resources"></a>Ek kaynaklar

 * [Azure veri taşıma hizmeti (DMS) kullanarak SQL geçiş](https://www.microsoft.com/handsonlabs/SelfPacedLabs/?storyGuid=3b671509-c3cd-4495-8e8f-354acfa09587) uygulamalı Laboratuvar.