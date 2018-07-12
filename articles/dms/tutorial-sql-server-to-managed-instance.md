---
title: Azure SQL veritabanı yönetilen örneğine geçirmek için DMS kullanın | Microsoft Docs
description: Azure veritabanı geçiş hizmetini kullanarak Azure SQL veritabanı yönetilen örneği için SQL Server şirket içinden geçirmeyi öğrenin.
services: dms
author: edmacauley
ms.author: edmaca
manager: craigg
ms.reviewer: ''
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 05/07/2018
ms.openlocfilehash: be007fe06f7b3354dc88e319a76715d2e94eb3fb
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38971497"
---
# <a name="migrate-sql-server-to-azure-sql-database-managed-instance-using-dms"></a>Azure SQL veritabanı yönetilen DMS kullanarak örneği için SQL Server'ı geçirme
Şirket içi SQL Server örneğine veritabanlarını geçirmek için Azure veritabanı geçiş hizmetini kullanabilirsiniz bir [Azure SQL veritabanı yönetilen örneği](../sql-database/sql-database-managed-instance.md). Bazı el ile çalışma gerektirebilir ek yöntemleri görmek için makaleyi [Azure SQL veritabanı yönetilen örneği SQL Server örneği geçiş](../sql-database/sql-database-managed-instance-migrate.md).

> [!IMPORTANT]
> Geçiş projeleri SQL Server'dan Azure SQL veritabanı yönetilen örneği Önizleme aşamasındadır ve tabi olan [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu öğreticide, geçiş **Adventureworks2012** şirket içi SQL Server örneğini bir veritabanından için Azure veritabanı geçiş hizmetini kullanarak Azure SQL veritabanı yönetilen örneği.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Azure veritabanı geçiş Hizmeti'nin bir örneğini oluşturun.
> * Azure veritabanı geçiş hizmeti kullanarak bir geçiş projesi oluşturun.
> * Bir geçiş çalıştırın.
> * Geçiş izleyin.
> * Geçiş raporunu indirin.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için şunları yapmanız:

- Sanal ağ için Azure veritabanı geçiş hizmetini kullanarak şirket içi kaynak sunucularınıza siteden siteye bağlantı sağlar Azure Resource Manager dağıtım modelini kullanarak oluşturma [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). [Bilgi edinmek için Azure veritabanı geçiş hizmetini kullanarak Azure SQL DB yönetilen örneği geçişlerinin ağ topolojileri](https://aka.ms/dmsnetworkformi).
- Uygulamanızın Azure sanal ağ (VNET) ağ güvenlik grubu kuralları blok aşağıdaki iletişim bağlantı noktaları 443, 53, 9354, 445, 12000. Azure VNET NSG trafik filtreleme hakkında daha fazla ayrıntı için bkz [ağ güvenlik grupları ile ağ trafiğini filtreleme](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Yapılandırma, [kaynak veritabanı altyapısı erişimi için Windows Güvenlik Duvarı](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Varsayılan olarak 1433 numaralı TCP bağlantı noktası olan SQL Server kaynağına erişmek Azure veritabanı geçiş hizmeti izin vermek için Windows Güvenlik duvarını açın.
- Birden çok adlandırılmış SQL Server örneği dinamik bağlantı noktalarını kullanarak çalıştırıyorsanız, SQL Tarayıcı Hizmeti'ni etkinleştir ve kaynak üzerinde adlandırılmış bir örnek için Azure veritabanı geçiş hizmeti bağlantı kurabilmesi için güvenlik duvarlarınızdan aracılığıyla UDP bağlantı noktası 1434'e erişim izni vermek isteyebilirsiniz Sunucu.
- Kaynak veritabanlarınızı önünde bir güvenlik duvarı Gereci kullanıyorsanız, SMB bağlantı noktası 445 üzerinden dosyaları yanı sıra, geçiş için kaynak veritabanları erişmek Azure veritabanı geçiş hizmeti izin vermek için güvenlik duvarı kuralları eklemek gerekebilir.
- Makalesinde ayrıntılı olarak Azure SQL veritabanı yönetilen örneği oluşturma [Azure portalında Azure SQL veritabanı yönetilen örneği oluşturma](https://aka.ms/sqldbmi).
- Yönetilen örnek hedef ve kaynak SQL Server'ı bağlanmak için kullanılan oturum açma bilgileri sysadmin sunucu rolünün bir üyesi olduğundan emin olun.
- Kaynak veritabanını yedeklemek için Azure veritabanı geçiş hizmeti kullanabileceğiniz bir ağ paylaşımı oluşturun.
- Kaynak çalıştıran hizmet hesabının ayrıcalıkları SQL Server örneğinin, oluşturduğunuz ağ paylaşımı üzerinde yazma ve kaynak sunucunun bilgisayar hesabını aynı paylaşımına okuma/yazma erişimi olduğundan emin olun.
- Daha önce oluşturduğunuz ağ paylaşımı üzerinde tam denetim ayrıcalığı olan bir Windows kullanıcı (ve parola) not edin. Azure veritabanı geçiş hizmetinin yedekleme dosyalarını geri yükleme işlemi için Azure depolama kapsayıcısına karşıya yüklemek için kullanıcı kimlik bilgilerini temsil eder.
- Bir blob kapsayıcı oluşturun ve bu makaledeki adımları kullanarak SAS URI'sini Al [yönetme Azure Blob depolama kaynaklarını depolama Gezgini'yle](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container), tüm izinleri (okuma, yazma, silme, liste) ilkesi penceresinde seçtiğinizden emin olun SAS URI'si oluşturma. Bu ayrıntılı Azure veritabanı geçiş hizmeti geçirmek için kullanılan yedekleme dosyaları karşıya yükleme, erişim için depolama hesabı kapsayıcınıza sağlar veritabanlarını Azure SQL veritabanı yönetilen örneği

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını kaydetme

1. Azure portalında, select oturum **tüm hizmetleri**ve ardından **abonelikleri**.

    ![Portal abonelikleri Göster](media\tutorial-sql-server-to-managed-instance\portal-select-subscriptions.png)        

2. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve sonra **Kaynak sağlayıcıları**’nı seçin.

    ![Kaynak sağlayıcıları göster](media\tutorial-sql-server-to-managed-instance\portal-select-resource-provider.png)

3. Geçiş ve ardından sağındaki arama **Microsoft.DataMigration**seçin **kaydetme**.

    ![Kaynak sağlayıcısını kaydetme](media\tutorial-sql-server-to-managed-instance\portal-register-resource-provider.png)   

## <a name="create-an-azure-database-migration-service-instance"></a>Bir Azure veritabanı geçiş hizmeti örneği oluşturma

1. Azure portalında + **kaynak Oluştur**, arama **Azure veritabanı geçiş hizmeti**ve ardından **Azure veritabanı geçiş hizmeti** açılır listeden Liste.

     ![Azure Market](media\tutorial-sql-server-to-managed-instance\portal-marketplace.png)

2. Üzerinde **Azure veritabanı geçiş hizmeti** ekranındayken **Oluştur**.

    ![Azure veritabanı geçiş hizmeti örneği oluşturma](media\tutorial-sql-server-to-managed-instance\dms-create1.png)

3. Üzerinde **geçiş hizmeti oluşturma** ekranında, hizmet aboneliği ve yeni veya mevcut bir kaynak grubu için bir ad belirtin.

4. Mevcut bir sanal ağ (VNET) seçin veya oluşturun.
 
    Sanal ağ, Azure veritabanı geçiş hizmeti ile SQL Server kaynak ve hedef Azure SQL veritabanı yönetilen örneği için erişim sağlar.

    Azure portalında VNET oluşturma hakkında daha fazla bilgi için bkz [Azure portalını kullanarak bir sanal ağ oluşturma](https://aka.ms/DMSVnet).

    Ek ayrıntılar için bkz [ağ topolojileri için Azure veritabanı geçiş hizmetini kullanarak Azure SQL DB yönetilen örneği geçişlerinin](https://aka.ms/dmsnetworkformi).

5. Fiyatlandırma katmanını seçin.

    Maliyet ve fiyatlandırma katmanları hakkında daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://aka.ms/dms-pricing).
   
    ![DMS hizmeti oluşturma](media\tutorial-sql-server-to-managed-instance\dms-create-service1.png)

6.  Seçin **Oluştur** hizmeti oluşturmak için.

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma

Hizmet oluşturulduktan sonra Azure portalında bulun, açın ve ardından yeni bir geçiş projesi oluşturun.

1. Azure portalında **tüm hizmetleri**aramak için Azure veritabanı geçiş hizmeti ve ardından **Azure veritabanı geçiş hizmetleri**.

    ![Azure veritabanı geçiş hizmeti tüm örneklerini bulun](media\tutorial-sql-server-to-azure-sql\dms-search.png)

2. Üzerinde **Azure veritabanı geçiş hizmeti ekran**, oluşturduğunuz ve örnek seçip örneğinin adını arayın.
 
3. Seçin + **yeni geçiş projesi**.

4. Üzerinde **yeni geçiş projesi** projesi için bir ad belirtin, ekran **kaynak sunucu türü** metin kutusunda **SQL Server**ve ardından **hedef Sunucu türü** metin kutusunda **Azure SQL veritabanı yönetilen örneği**.

   ![DMS projesi oluşturma](media\tutorial-sql-server-to-managed-instance\dms-create-project1.png)

5. Seçin **Oluştur** projeyi oluşturmak için.

## <a name="specify-source-details"></a>Kaynak ayrıntıları belirtin

1. Üzerinde **kaynak ayrıntıları** ekranında, kaynak SQL Server için bağlantı ayrıntılarını belirt.

2. Güvenilen bir sertifika sunucunuz üzerinde yüklemediyseniz seçin **sunucu sertifikasına güven** onay kutusu.

    Güvenilen bir sertifika yüklü değil, SQL Server örneği başlatıldığında otomatik olarak imzalanan bir sertifika oluşturur. Bu sertifika, istemci bağlantıları için kimlik bilgilerini şifrelemek için kullanılır.

    > [!CAUTION]
    > Kendinden imzalı bir sertifika kullanılarak şifrelenir ve SSL bağlantıları, güçlü güvenlik sağlamaz. ADAM-de-adam saldırılarına değildirler. Bir üretim ortamında otomatik olarak imzalanan sertifikaları kullanarak SSL veya internet'e bağlı sunucuları güvenmemelisiniz.

   ![Kaynak Ayrıntıları](media\tutorial-sql-server-to-managed-instance\dms-source-details1.png)

3. **Kaydet**’i seçin.

4. Üzerinde **kaynak veritabanlarını seçin** ekranında, seçin **Adventureworks2012** geçiş için veritabanı.

   ![Kaynak veritabanlarını seçin](media\tutorial-sql-server-to-managed-instance\dms-source-database1.png)

5. **Kaydet**’i seçin.

## <a name="specify-target-details"></a>Hedef ayrıntıları belirtin

1.  Üzerinde **hedef ayrıntıları** ekranında, önceden sağlanmış Azure SQL veritabanı yönetilen örneği hangi olan hedef için bağlantı ayrıntılarını belirt **AdventureWorks2012** veritabanıdır olması Geçişi.

    Azure SQL veritabanı yönetilen örneği sağlanmadı gerekiyorsa bunu seçin **Hayır** örneği sağlaması yardımcı olması için bir bağlantı için. Proje oluşturma ile yine de devam etmek ve bir Azure SQL veritabanı yönetilen örneği hazır olduğunda geçiş yürütmek için bu belirli proje dönün.   
 
       ![Hedef seçin](media\tutorial-sql-server-to-managed-instance\dms-target-details1.png)

2.  **Kaydet**’i seçin.

3.  Üzerinde **Proje Özeti** ekranında, gözden geçirin ve geçiş projeyle ilişkili ayrıntılarını doğrulayın.
 
    ![Geçiş projesi özeti](media\tutorial-sql-server-to-managed-instance\dms-project-summary1.png)

4.  **Kaydet**’i seçin.   

## <a name="run-the-migration"></a>Geçiş çalıştırma

1.  Son kaydedilen projeyi seçin, + **yeni etkinlik**ve ardından **geçişi çalıştırma**.

    ![Yeni Etkinlik Oluşturun](media\tutorial-sql-server-to-managed-instance\dms-create-new-activity1.png)

2.  İstendiğinde, kaynak ve hedef sunucular kimlik bilgilerini girin ve ardından **Kaydet**.

3.  Üzerinde **kaynak veritabanlarını seçin** ekranında, geçirmek istediğiniz kaynak veritabanlarını seçin.

    ![Kaynak veritabanlarını seçin](media\tutorial-sql-server-to-managed-instance\dms-select-source-databases2.png)

4.  Seçin **Kaydet**ve ardından **seçin oturumları** ekranında, geçirmek istediğiniz oturum açmaları seçin.

    Geçerli sürümde, yalnızca geçirme SQL oturumları destekler.

    ![Oturum açmaları seçin](media\tutorial-sql-server-to-managed-instance\dms-select-logins.png)

5. Seçin **Kaydet**ve ardından **geçiş ayarlarını yapılandırma** ekranında, aşağıdaki ayrıntıları sağlayın:

    | | |
    |--------|---------|
    |**Ağ konumu paylaşımında** | Yerel ağ paylaşımı Azure veritabanı geçiş Hizmeti'nin veritabanı yedeklemeleri oluşturmak için kaynak sürebilir. Kaynak SQL Server örneğini çalıştıran hizmet hesabının bu ağ paylaşımında yazma ayrıcalıkları olmalıdır. Örneğin, bir ağ paylaşımındaki sunucunun FQDN veya IP adreslerine sağlamak '\\\servername.domainname.com\backupfolder' veya '\\\IP address\backupfolder'.|
    |**Kullanıcı adı** | Azure veritabanı geçiş hizmeti kimliğine bürünmek ve yedekleme dosyalarını geri yükleme işlemi için Azure depolama kapsayıcısına karşıya windows kullanıcı adı. |
    |**Parola** | Kullanıcının parolası. |
    |**Depolama hesabı ayarları** | Azure SQL veritabanı yönetilen örneğine geçirmek için Azure veritabanı geçiş Hizmeti'nin hizmeti ve yedekleme dosyaları karşıya yükler, depolama hesabı kapsayıcısını erişimle kullanılır sağlayan SAS URI'sini veritabanları. [Blob kapsayıcısı için SAS URI'sini alma hakkında bilgi](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container).|
    
    ![Geçiş ayarlarını yapılandırma](media\tutorial-sql-server-to-managed-instance\dms-configure-migration-settings2.png)

5.  Seçin **Kaydet**ve ardından **geçiş özeti** ekranında **etkinlik adı** metin kutusunda, geçiş etkinliği için bir ad belirtin.

    ![Geçiş özeti](media\tutorial-sql-server-to-managed-instance\dms-migration-summary2.png)

6. Genişletin **doğrulama seçeneği** görüntülemek için bölüm **doğrulama seçeneğini** ekranında, geçirilen veritabanı sorgu doğruluğunu doğrulayın ve ardından belirtin **Kaydet** .  

7. Seçin **geçişi çalıştırma**.

    Geçiş etkinlik penceresi görünür ve etkinlik durumu **bekleyen**.

## <a name="monitor-the-migration"></a>Geçiş izleme

1. Geçiş etkinlik ekranında seçin **Yenile** görüntü güncelleştirilemedi.
 
   ![Geçiş etkinlik sürüyor](media\tutorial-sql-server-to-managed-instance\dms-migration-activity-in-progress.png)

2. İlgili sunucu nesneleriyle geçiş durumunu izlemek için veritabanlarını ve oturum açma bilgileri kategorileri daha da genişletebilirsiniz.

   ![Geçiş etkinlik sürüyor](media\tutorial-sql-server-to-managed-instance\dms-migration-activity-monitor.png)

3. Geçiş tamamlandıktan sonra seçin **raporu indir** geçiş işlemiyle ilgili ayrıntıları gösteren bir rapor almak için.
 
4. Doğrulayın Hedef veritabanında hedef Azure SQL veritabanı yönetilen örneği ortamı.

## <a name="next-steps"></a>Sonraki adımlar

- Bir veritabanını bir T-SQL RESTORE komutunu kullanarak yönetilen örneğe için geçirme işlemini gösteren bir öğretici için bkz [bir restore komutunu kullanarak yönetilen örneği için bir yedeği geri](../sql-database/sql-database-managed-instance-restore-from-backup-tutorial.md).
- Yönetilen örneği hakkında daha fazla bilgi için bkz. [yönetilen örnek nedir](../sql-database/sql-database-managed-instance.md).
- Uygulamalar bir yönetilen örneğe bağlanma hakkında daha fazla bilgi için bkz: [uygulamaları bağlama](../sql-database/sql-database-managed-instance-connect-app.md).
