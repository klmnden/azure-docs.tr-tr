---
title: Azure SQL veritabanı yönetilen örneğine geçirmek için DMS kullanın | Microsoft Docs
description: Azure veritabanı geçiş hizmetini kullanarak Azure SQL veritabanı yönetilen örneği için SQL Server şirket içi geçirilecek öğrenin.
services: dms
author: edmacauley
ms.author: edmaca
manager: craigg
ms.reviewer: ''
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 04/22/2018
ms.openlocfilehash: e239077b0c2c547ceb73d4d1ab975ee633ecb2e4
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="migrate-sql-server-to-azure-sql-database-managed-instance-using-dms"></a>Azure SQL veritabanı yönetilen DMS kullanma örneği için SQL Server geçirme
Azure veritabanı geçiş hizmeti veritabanlarını şirket içi SQL Server örneğine geçirmek için kullanabileceğiniz bir [yönetilen Azure SQL veritabanı örneği](../sql-database/sql-database-managed-instance.md) sıfır kapalı kalma süresi ile. Miktar kapalı kalma süresi gerektiren ek yöntemleri için bkz: [yönetilen Azure SQL veritabanı örneğine SQL Server örneği geçiş](../sql-database/sql-database-managed-instance-migrate.md).

Bu öğreticide, geçiş **Adventureworks2012** şirket içi örneğini SQL Server veritabanına bir Azure SQL veritabanı yönetilen Azure veritabanı geçiş hizmetini kullanarak örneği.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Azure veritabanı geçiş hizmeti örneğini oluşturun.
> * Azure veritabanı geçiş hizmetini kullanarak bir geçiş projesi oluşturun.
> * Geçiş çalıştırın.
> * Geçiş izleyin.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aktarmanız gerekir:

- Kullanarak, şirket içi kaynak sunucular için siteden siteye bağlantı sağlar Azure Resource Manager dağıtım modelini kullanarak Azure veritabanı geçiş hizmeti için bir VNET oluşturma [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). [Azure veritabanı geçiş hizmeti ile Azure SQL DB yönetilen örneği geçişler için ağ topolojileri öğrenin](https://aka.ms/dmsnetworkformi).
- Azure sanal ağ (VNET) ağ güvenlik grubu kuralları blok aşağıdaki iletişim bağlantı noktaları 443, 53, 9354, 445, 12000. Azure VNET NSG trafik filtreleme daha ayrıntılı bilgi için bkz: [filtre ağ güvenlik grupları ile ağ trafiği](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Yapılandırma, [kaynak veritabanı altyapısı erişimi için Windows Güvenlik Duvarı](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Windows, varsayılan olarak TCP bağlantı noktası 1433 olan SQL Server kaynağına erişmek Azure veritabanı geçiş Hizmeti Güvenlik Duvarı'nı açın.
- Dinamik bağlantı noktaları kullanan birden fazla adlandırılmış SQL Server örneklerini çalıştırıyorsanız, SQL Tarayıcı Hizmeti'ni etkinleştir ve böylece Azure veritabanı geçiş hizmeti kaynağınız adlandırılmış bir örnekte bağlanabilir, güvenlik duvarları üzerinden UDP bağlantı noktası 1434 erişmesine izin vermek isteyebilir Sunucu.
- Bir güvenlik duvarı gerecini kaynak veritabanlarınızı önünde kullanıyorsanız, SMB 445 bağlantı noktası aracılığıyla dosyaların yanı sıra, geçiş için kaynak veritabanlarının erişmek Azure veritabanı geçiş hizmeti izin veren güvenlik duvarı kuralları eklemeniz gerekebilir.
- Yönetilen bir Azure SQL veritabanı örneği makalede ayrıntı izleyerek oluşturun [Azure portalında bir Azure SQL veritabanı örneği'ni yönetilen oluşturma](https://aka.ms/sqldbmi).
- Yönetilen örneğini hedeflemek ve SQL Server Kaynak bağlanmak için kullanılan oturum açma bilgileri sysadmin sunucu rolünün üyesi olduğundan emin olun.
- Azure veritabanı geçiş hizmeti kaynak veritabanını yedeklemek için kullanabileceğiniz bir ağ paylaşımı oluşturun.
- Kaynak çalıştıran hizmet hesabını ayrıcalıkları SQL Server örneğinin oluşturduğunuz ağ paylaşımı üzerinde yazma ve bilgisayar hesabı kaynak sunucu için aynı paylaşımına okuma/yazma erişimi olduğundan emin olun.
- Daha önce oluşturulmuş ağ paylaşımında tam denetim ayrıcalığına sahip bir Windows kullanıcısı (ve parola) not edin. Azure veritabanı geçiş hizmeti, yedekleme dosyalarını geri yükleme işlemi Azure depolama kapsayıcısının karşıya yüklemek için kullanıcı kimlik bilgilerini temsil eder.
- Bir blob kapsayıcı oluşturun ve makaledeki adımları kullanarak SAS URI'sini Al [Depolama Gezgini ile Azure Blob Storage'ı yönetme kaynaklarını](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container), tüm izinlerine (okuma, yazma, silme, liste) İlkesi penceresi sırasında seçtiğinizden emin olun SAS URI'sini oluşturuluyor. Bu ayrıntı Azure veritabanı geçiş hizmeti geçirmek için kullanılan yedekleme dosyaları karşıya yükleme için depolama hesabı kapsayıcısının erişim sağlayan yönetilen Azure SQL veritabanı örneğine veritabanları

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını Kaydet

1.  Azure portalında, select oturum açma **tüm hizmetleri**ve ardından **abonelikleri**.
![Portal abonelikleri Göster](media\tutorial-sql-server-to-managed-instance\portal-select-subscription.png)

2.  Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve sonra **Kaynak sağlayıcıları**’nı seçin.
![kaynak sağlayıcıları göster](media\tutorial-sql-server-to-managed-instance\portal-select-resource-provider.png)

3.  Arama geçiş ve ardından sağ tarafındaki **Microsoft.DataMigration**seçin **kaydetmek**.
![Kayıt kaynak sağlayıcısı](media\tutorial-sql-server-to-managed-instance\portal-register-resource-provider.png)   

## <a name="create-an-azure-database-migration-service-instance"></a>Bir Azure veritabanı geçiş hizmet örneği oluşturma

1.  Azure portalında seçin **+ kaynak oluşturma**, arama **Azure veritabanı geçiş hizmeti**ve ardından **Azure veritabanı geçiş hizmeti** açılan gelen Liste.

     ![Azure Market](media\tutorial-sql-server-to-managed-instance\portal-marketplace.png)

2.  Üzerinde **Azure veritabanı geçiş hizmeti (Önizleme)** ekran, select **oluşturma**.

    ![Azure veritabanı geçiş hizmet örneği oluşturma](media\tutorial-sql-server-to-managed-instance\dms-create.png)

3.  Üzerinde **veritabanı geçiş hizmeti** ekranında, hizmet, abonelik, kaynak grubu, bir sanal ağ ve fiyatlandırma katmanı için bir ad belirtin.

    Maliyetleri ve fiyatlandırma katmanları hakkında daha fazla bilgi için bkz: [fiyatlandırma sayfası](https://aka.ms/dms-pricing). *Azure veritabanı geçiş hizmeti şu anda önizlemede ve herhangi bir ücret alınmaz.*

    **Ağ:** varolan birini seçin veya SQL Server kaynak erişimi ve hedef Azure SQL veritabanı örneği yönetilen Azure veritabanı geçiş hizmeti sağlayan yeni bir VNET oluşturun. [Azure veritabanı geçiş hizmeti ile Azure SQL DB yönetilen örneği geçişler için ağ topolojileri öğrenin](https://aka.ms/dmsnetworkformi).

    Azure portalında VNET oluşturma hakkında daha fazla bilgi için bkz: [Azure portalını kullanarak birden çok alt ağı ile bir sanal ağ oluşturma](https://aka.ms/DMSVnet).

    ![DMS hizmet oluşturma](media\tutorial-sql-server-to-managed-instance\dms-create-service.png)

4.  Seçin **oluşturma** hizmeti oluşturmak için.

## <a name="create-a-migration-project"></a>Bir geçiş projesi oluşturma

Hizmet oluşturulduktan sonra Azure portalını bulun ve açın.

1.  Seçin **+ yeni geçiş proje**.

2.  Üzerinde **yeni geçiş proje** ekranında, proje için bir ad belirtin **kaynak sunucu türünü** metin kutusunda **SQL Server**ve ardından **hedef Sunucu türü** metin kutusunda **yönetilen Azure SQL veritabanı örneği**.

    ![DMS projesi oluşturma](media\tutorial-sql-server-to-managed-instance\dms-create-project.png)

3.  Seçin **oluşturma** projesi oluşturmak için.

## <a name="specify-source-details"></a>Kaynak ayrıntıları belirtin

1.  Üzerinde **kaynak ayrıntıları** ekranında, SQL Server kaynak bağlantı ayrıntılarını belirtin.

    ![Kaynak Ayrıntıları](media\tutorial-sql-server-to-managed-instance\dms-source-details.png)

2.  Seçin **kaydetmek**ve ardından **Adventureworks2012** geçiş için veritabanı.

    ![Kaynak veritabanlarını seçin](media\tutorial-sql-server-to-managed-instance\dms-source-database.png)

## <a name="specify-target-details"></a>Hedef ayrıntıları belirtin

1.  Seçin **kaydetmek**ve ardından **hedef ayrıntıları** ekranında, önceden sağlanan Azure SQL veritabanı yönetilen örnek için hedef bağlantı ayrıntılarını belirt  **AdventureWorks2012** veritabanının taşınır.

    ![Hedef seçin](media\tutorial-sql-server-to-managed-instance\dms-target-details.png)

2.  **Kaydet**’i seçin.

3.  Üzerinde **Proje Özeti** ekranında, gözden geçirin ve geçiş projeyle ilişkili ayrıntılarını doğrulayın.

## <a name="run-the-migration"></a>Geçişi çalıştırma

1.  En son kaydedilen proje seçin, **+ yeni etkinlik**ve ardından **geçişi çalıştırma**.

    ![Yeni etkinlik oluşturmak](media\tutorial-sql-server-to-managed-instance\dms-create-new-activity.png)

2.  İstendiğinde, kaynak ve hedef sunucular kimlik bilgilerini girin ve ardından **kaydetmek**.

3.  Üzerinde **hedef veritabanlarına harita** ekranında, geçirmek istediğiniz kaynak veritabanlarını seçin.

    ![Kaynak veritabanlarını seçin](media\tutorial-sql-server-to-managed-instance\dms-select-source-databases.png)

4.  Seçin **kaydetmek**, **geçiş ayarları yapılandırmak** ekranında, aşağıdaki ayrıntıları girin:

    | | |
    |--------|---------|
    |**Sunucu yedekleme konumu** | Yerel ağ paylaşımı Azure veritabanı geçiş hizmeti kaynak veritabanı yedeklemeleri alabilir. Kaynak SQL Server örneğini çalıştıran hizmet hesabını, bu ağ paylaşımına yazma ayrıcalıkları olmalıdır. |
    |**Kullanıcı adı** | Azure veritabanı geçiş hizmeti kimliğine bürünmek ve geri yükleme işlemi için Azure depolama kapsayıcısı için yedekleme dosyalarının karşıya windows kullanıcı adı. |
    |**Parola** | Yukarıdaki kullanıcının parolası. |
    |**Depolama SAS URI'sini** | Azure veritabanı geçiş hizmeti hizmeti yedekleme dosyaları ve karşıya yükleme, depolama hesabının kapsayıcıya erişimi ile geçirmek için kullanılan sağlar SAS URI'sini Azure SQL veritabanı yönetilen örneğine veritabanları. [Blob kapsayıcısı için SAS URI'sini alma hakkında bilgi](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container).|
    
    ![Geçiş ayarları yapılandırma](media\tutorial-sql-server-to-managed-instance\dms-configure-migration-settings.png)

5.  Seçin **kaydetmek**, Özet geçiş ekran **etkinlik adı** metin kutusunda, geçiş etkinliği için bir ad belirtin.

    ![Geçiş özeti](media\tutorial-sql-server-to-managed-instance\dms-migration-summary.png)

## <a name="monitor-the-migration"></a>Geçiş izleme

1.  Etkinlik durumunu gözden geçirmek için geçiş etkinliği seçin.

2.  Geçiş tamamlandıktan sonra hedef veritabanlarına yönetilen Azure SQL veritabanı örneği hedefte doğrulayın.

    ![Geçiş izleme](media\tutorial-sql-server-to-managed-instance\dms-monitor-migration.png)

## <a name="next-steps"></a>Sonraki adımlar

- Bir yönetilen T-SQL Geri Yükle komutunu kullanarak örneği için bir veritabanı geçirmek nasıl gösteren bir öğretici için bkz: [bir yönetilen restore komutu kullanma örneği için bir yedeğini geri](../sql-database/sql-database-managed-instance-restore-from-backup-tutorial.md).
- Bir veritabanı bir BACPAC dosyadan içeri aktarma hakkında daha fazla bilgi için bkz: [yeni bir Azure SQL veritabanı için bir BACPAC dosyasını içe](../sql-database/sql-database-import.md).
- Yönetilen örneği hakkında daha fazla bilgi için bkz: [yönetilen örneği nedir](../sql-database/sql-database-managed-instance.md).
- Uygulamaları yönetilen bir örneğine bağlanma hakkında daha fazla bilgi için bkz: [uygulamalarını bağlamak](../sql-database/sql-database-managed-instance-connect-app.md).
