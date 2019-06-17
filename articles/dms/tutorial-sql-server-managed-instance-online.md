---
title: "Öğretici: Azure SQL veritabanı için SQL Server'ın çevrimiçi bir geçiş gerçekleştirmek için kullanım Azure veritabanı geçiş hizmeti yönetilen örneği | Microsoft Docs"
description: Bir Azure SQL veritabanı yönetilen örneği SQL Server şirket içinden Azure veritabanı geçiş hizmeti kullanarak çevrimiçi bir geçiş gerçekleştirmek öğrenin.
services: dms
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 06/14/2019
ms.openlocfilehash: 4e45251147561f2376ac4b044ebdf3a599092dcf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67126099"
---
# <a name="tutorial-migrate-sql-server-to-an-azure-sql-database-managed-instance-online-using-dms"></a>Öğretici: SQL Server'ı Azure SQL veritabanı yönetilen örneğine geçirme çevrimiçi DMS kullanarak

Şirket içi SQL Server örneğine veritabanlarını geçirmek için Azure veritabanı geçiş hizmeti kullanabilirsiniz bir [Azure SQL veritabanı yönetilen örneği](../sql-database/sql-database-managed-instance.md) en düşük kapalı kalma süresi. Bazı el ile çalışma gerektirebilir ek yöntemleri görmek için makaleyi [SQL Server örneği geçiş Azure SQL veritabanı yönetilen örneği](../sql-database/sql-database-managed-instance-migrate.md).

Bu öğreticide, geçiş **Adventureworks2012** şirket içi örneğini SQL Server veritabanından Azure veritabanı geçiş hizmetini kullanarak en az kapalı kalma süresi ile Azure SQL veritabanı yönetilen örneğine.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
>
> * Azure veritabanı geçiş Hizmeti'nin bir örneğini oluşturun.
> * Bir geçiş projesi oluşturun ve Azure veritabanı geçiş hizmeti kullanarak çevrimiçi geçişi başlatın.
> * Geçişi izleme.
> * Hazır olduğunuzda tam geçişi gerçekleştirme.

> [!IMPORTANT]
> SQL Server çevrimiçi geçiş için Azure veritabanı geçiş hizmetini kullanarak bir Azure SQL veritabanı yönetilen örneği, tam veritabanı yedeği sağlamanız gerekir ve hizmete veritabanlarınızı geçirme için kullanabileceğiniz SMB ağ sonraki günlük yedeği paylaşın. Azure veritabanı geçiş hizmeti, yedeklemelere başlatılmaz, ancak bunun yerine mevcut yedeklemeler, olağanüstü durum kurtarma planınızın bir parçası zaten olabilir, geçiş için kullanır.
> Dikkate aldığınızdan emin olun [ile sağlama seçeneği kullanarak yedeklemeleri](https://docs.microsoft.com/sql/relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server?view=sql-server-2017). Ayrıca, birden fazla yedekleme (yani, tam ve t-log) tek bir yedekleme ortamı eklemediğinizden emin olun; her ayrı bir yedekleme dosya yedekleyin.

> [!NOTE]
> Azure veritabanı geçiş hizmeti çevrimiçi bir geçiş gerçekleştirmek için Premium fiyatlandırma katmanını temel alan bir örneği oluşturmanız gerekir.

> [!IMPORTANT]
> En iyi geçiş deneyimi için Microsoft, Azure Veritabanı Geçiş Hizmeti’nin bir örneğini hedef veritabanıyla aynı Azure bölgesinde oluşturmayı önerir. Verileri bölgeler veya coğrafyalar arasında taşımak, geçiş sürecini yavaşlatabilir ve hatalara neden olabilir.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Bu makalede, SQL Server'dan Azure SQL veritabanı yönetilen örneği için bir çevrimiçi geçiş açıklanır. Çevrimdışı bir geçiş için bkz: [SQL Server'a geçirme Azure SQL veritabanı yönetilen örneği DMS kullanarak çevrimdışı](tutorial-sql-server-to-managed-instance.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

* Kullanarak şirket içi kaynak sunucularınıza siteden siteye bağlantı sağlar Azure Resource Manager dağıtım modelini kullanarak bir Azure sanal ağı (VNet) için Azure veritabanı geçiş hizmeti oluşturma [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). [Bilgi edinmek için Azure veritabanı geçiş hizmetini kullanarak Azure SQL veritabanı yönetilen örneği geçişlerinin ağ topolojileri](https://aka.ms/dmsnetworkformi). Sanal ağ oluşturma hakkında daha fazla bilgi için bkz. [sanal ağ belgeleri](https://docs.microsoft.com/azure/virtual-network/)ve özellikle hızlı başlangıç makalelerini ile adım adım ayrıntıları.

    > [!NOTE]
    > Microsoft Ağ eşlemesi ile ExpressRoute kullanıyorsanız, sanal ağ kurulumu sırasında şu Hizmet Ekle [uç noktaları](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) hangi hizmet sağlanacağı alt ağ için:
    >
    > * Hedef veritabanı uç noktası (örneğin, SQL uç noktası, Cosmos DB uç noktası vb.)
    > * Depolama uç noktası
    > * Service bus uç noktası
    >
    > Azure veritabanı geçiş hizmeti internet bağlantısı olmadığı için bu gerekli bir yapılandırmadır.

* VNet ağ güvenlik grubu kurallarınızı aşağıdaki gelen iletişim bağlantı noktaları için Azure veritabanı geçiş hizmeti engelleme emin olun: 443, 53, 9354, 445, 12000. Azure VNet NSG trafik filtreleme hakkında daha fazla ayrıntı için bkz [ağ güvenlik grupları ile ağ trafiğini filtreleme](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
* [Windows Güvenlik Duvarınızı kaynak veritabanı altyapısı erişimi](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) için yapılandırın.
* Azure Veritabanı Geçiş Hizmeti'ne kaynak SQL Server erişimi sağlamak için Windows güvenlik duvarınızı açın. Varsayılan ayarlarda 1433 numaralı TCP bağlantı noktası kullanılır.
* Dinamik bağlantı noktası kullanarak birden fazla adlandırılmış SQL Server örneği çalıştırıyorsanız Azure Veritabanı Geçiş Hizmeti'nin kaynak sunucunuzdaki adlandırılmış örneğe bağlanabilmesi için SQL Browser Hizmeti'ni etkinleştirebilir ve güvenlik duvarınızda 1434 numaralı UDP bağlantı noktasına erişim izni verebilirsiniz.
* Kaynak veritabanlarınızın önünde bir güvenlik duvarı cihazı kullanıyorsanız Azure Veritabanı Geçiş Hizmeti'nin geçiş amacıyla kaynak veritabanlarına ve 445 numaralı SMB bağlantı noktası aracılığıyla dosyalara erişmesi için güvenlik duvarı kuralları eklemeniz gerekebilir.
* Makalesinde ayrıntılı olarak bir Azure SQL veritabanı yönetilen örneği oluşturma [Azure portalında bir Azure SQL veritabanı yönetilen örnek oluşturma](https://aka.ms/sqldbmi).
* Oturum açma bilgileri SQL Server kaynağına bağlanmak için kullanılan ve hedef yönetilen örneğinde sysadmin sunucu rolünün bir üyesi olduğundan emin olun.
* Azure Veritabanı Geçiş Hizmeti'nin veritabanı geçişi için kullanabileceği veritabanınızın tam veritabanı yedekleme dosyalarını ve işlem günlüğü yedekleme dosyalarını içeren bir SMB ağ paylaşımı sağlayın.
* Kaynak SQL Server örneğini çalıştıran hizmet hesabının oluşturduğunuz ağ paylaşımında yazma ayrıcalıklarına sahip olduğundan ve kaynak sunucunun bilgisayar hesabının aynı paylaşımda okuma/yazma erişimine sahip olduğundan emin olun.
* Önceden oluşturduğunuz ağ paylaşımında tam denetim ayrıcalığına sahip olan Windows kullanıcısını (ve parolasını) not edin. Azure Veritabanı Geçiş Hizmeti, geri yükleme işlemi için yedekleme dosyalarını Azure depolama kapsayıcısına yüklemek için kullanıcının kimlik bilgilerini kullanır.
* Bir Azure Active Directory Uygulama oluşturan DMS hizmetini hedef Azure veritabanı yönetilen örneğine bağlanmak için kullanabileceğiniz uygulama kimliği anahtarı kimliği ve Azure depolama kapsayıcısı oluşturun. Daha fazla bilgi için bkz. [Kaynaklara erişebilen bir Azure Active Directory uygulaması ve hizmet sorumlusu oluşturmak için portalı kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal).

  > [!NOTE]
  > DMS belirtilen uygulama kimliği için Abonelik üzerinde katkıda bulunan izni gerekir. Etkin bir şekilde bu izinleri gereksinimlerini azaltmak için çalışıyoruz.

* DMS hizmetinin veritabanı yedekleme dosyalarını yükleyebileceği ve veritabanlarını geçirmek için kullanabileceği bir **Standart Performans katmanı** Azure Depolama Hesabı oluşturun veya belirtin.  Azure Depolama Hesabının oluşturulan DMS hizmetiyle aynı bölgede olduğundan emin olun.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını kaydetme

1. Azure portal'da oturum açın, **Tüm hizmetler** seçeneğini belirleyin ve ardından **Abonelikler**'i seçin.

    ![Portal aboneliklerini gösterme](media/tutorial-sql-server-to-managed-instance-online/portal-select-subscriptions.png)

2. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve sonra **Kaynak sağlayıcıları**’nı seçin.

    ![Kaynak sağlayıcılarını gösterme](media/tutorial-sql-server-to-managed-instance-online/portal-select-resource-provider.png)

3. "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.

    ![Kaynak sağlayıcısını kaydetme](media/tutorial-sql-server-to-managed-instance-online/portal-register-resource-provider.png)   

## <a name="create-an-azure-database-migration-service-instance"></a>Azure Veritabanı Geçiş Hizmeti örneğini oluşturma

1. Azure portalda +**Kaynak oluştur**'u seçin, **Azure Veritabanı Geçiş Hizmeti** araması yapın ve açılan listeden **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

     ![Azure Market](media/tutorial-sql-server-to-managed-instance-online/portal-marketplace.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında **Oluştur**'u seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğini oluşturma](media/tutorial-sql-server-to-managed-instance-online/dms-create1.png)

3. **Geçiş Hizmeti oluşturun** ekranında hizmet için bir ad belirtin, aboneliği ve yeni ya da var olan bir kaynak grubunu seçin.

4. DMS örneğini oluşturmak istediğiniz konumu seçin.

5. Mevcut bir VNet seçin veya oluşturun.

    Sanal ağ, Azure veritabanı geçiş hizmeti ile SQL Server kaynak ve hedef Azure SQL veritabanı yönetilen örneğine erişim sağlar.

    Azure portalında VNet oluşturma hakkında daha fazla bilgi için bkz [Azure portalını kullanarak bir sanal ağ oluşturma](https://aka.ms/DMSVnet).

    Ek ayrıntılar için bkz [ağ topolojileri için Azure SQL veritabanı yönetilen örneği geçişlerinin Azure veritabanı geçiş hizmetini kullanarak](https://aka.ms/dmsnetworkformi).

6. Bir SKU Premium fiyatlandırma katmanı seçin.

    > [!NOTE]
    > Çevrimiçi geçişler yalnızca Premium katmanda kullanılırken desteklenir.

    Maliyetler ve fiyatlandırma katmanları hakkında daha fazla bilgi için [fiyatlandırma sayfasına](https://aka.ms/dms-pricing) bakın.

    ![DMS hizmetini başlatma](media/tutorial-sql-server-to-managed-instance-online/dms-create-service3.png)

7. Hizmeti oluşturmak için **Oluştur**’u seçin.

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma

Hizmetin bir örneği oluşturulduktan sonra Azure portaldan bulun, açın ve yeni bir geçiş projesi oluşturun.

1. Azure portalda **Tüm hizmetler**'i seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

    ![Azure Veritabanı Geçiş Hizmeti’nin tüm örneklerini bulma](media/tutorial-sql-server-to-managed-instance-online/dms-search.png)

2. **Azure Veritabanı Geçiş Hizmeti ekranında** oluşturduğunuz örneğin adını arayın ve sonuçlardan bu örneği seçin.

3. +**Yeni Geçiş Projesi**'ni seçin.

4. Üzerinde **yeni geçiş projesi** projesi için bir ad belirtin, ekran **kaynak sunucu türü** metin kutusunda **SQL Server**, **hedef sunucu tür** metin kutusunda **Azure SQL veritabanı yönetilen örneği**ve ardından **etkinlik türünü seçin**seçin **çevrimiçi veri geçişi** .

   ![DMS projesi oluşturma](media/tutorial-sql-server-to-managed-instance-online/dms-create-project3.png)

5. Projeyi oluşturmak için **Etkinlik oluştur ve çalıştır**'ı seçin.

## <a name="specify-source-details"></a>Kaynak ayrıntılarını belirtme

1. **Geçiş kaynağı ayrıntıları** ekranında SQL Server bağlantı ayrıntılarını belirtin.

2. Sunucunuza bir güvenilir sertifika yüklemediyseniz **Sunucu sertifikasına güven** onay kutusunu işaretleyin.

    Güvenilir sertifika yüklü değilse SQL Server, örnek başlatıldığında otomatik olarak imzalanan bir sertifika oluşturur. Bu sertifika, istemci bağlantılarında kimlik bilgilerini şifrelemek için kullanılır.

    > [!CAUTION]
    > Otomatik olarak imzalanan sertifika kullanarak şifrelenmiş SSL bağlantıları yüksek güvenlik sağlamaz. Ortadaki adam saldırılarına maruz kalabilirler. Üretim ortamında veya internete bağlı sunucularda otomatik olarak imzalanan sertifika ile SSL kullanımına güvenmemeniz gerekir.

   ![Kaynak Ayrıntıları](media/tutorial-sql-server-to-managed-instance-online/dms-source-details2.png)

3. **Kaydet**’i seçin.

4. **Kaynak veritabanlarını seçin** ekranında geçiş için **Adventureworks2012** veritabanını seçin.

   ![Kaynak veritabanlarını seçme](media/tutorial-sql-server-to-managed-instance-online/dms-source-database1.png)

    > [!IMPORTANT]
    > SQL Server Integration Services (SSIS) kullanırsanız, DMS şu anda, SSIS projelerini/paketlerini (SSISDB) için katalog veritabanına SQL Server'dan Azure SQL veritabanı yönetilen örneğine geçişi desteklemez. Ancak, Azure Data Factory (ADF) SSIS sağlayabilir ve, listelenen hedef Azure SQL veritabanı yönetilen örneği tarafından barındırılan SSISDB SSIS projelerini/paketlerini yeniden dağıtın. SSIS paketlerini geçirme hakkında daha fazla bilgi için bkz [geçirme SQL Server Integration Services paketlerini azure'a](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages).

5. **Kaydet**’i seçin.

## <a name="specify-target-details"></a>Hedef ayrıntılarını belirtme

1. Üzerinde **geçiş hedef ayrıntıları** belirtin, ekran **uygulama kimliği** ve **anahtar** DMS örneği Azure SQL veritabanı'nın hedef sunucuya bağlanmak için kullanabileceğiniz Yönetilen örnek ve Azure depolama hesabı.

    Daha fazla bilgi için bkz. [Kaynaklara erişebilen bir Azure Active Directory uygulaması ve hizmet sorumlusu oluşturmak için portalı kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal).

2. Seçin **abonelik** hedef Azure SQL veritabanı yönetilen örneğini içeren örnek ve hedef örneği seçin.

    Azure SQL veritabanı yönetilen örneği hazırlamadıysanız seçin [bağlantı](https://aka.ms/SQLDBMI) örneği sağlaması yardımcı olacak. Azure SQL veritabanı yönetilen örneği hazır olduğunda, geçiş işlemi yürütmek için bu belirli proje için döndürür.

3. Sağlamak **SQL kullanıcısı** ve **parola** yönetilen örneği Azure SQL veritabanına bağlanmak için.

    ![Hedef seçme](media/tutorial-sql-server-to-managed-instance-online/dms-target-details3.png)

4. **Kaydet**’i seçin.

## <a name="select-source-databases"></a>Kaynak veritabanlarını seçme

1. **Kaynak veritabanlarını seçin** ekranında geçirmek istediğiniz kaynak veritabanını seçin.

    ![Kaynak veritabanlarını seçme](media/tutorial-sql-server-to-managed-instance-online/dms-select-source-databases2.png)

2. **Kaydet**’i seçin.

## <a name="configure-migration-settings"></a>Geçiş ayarlarını yapılandırma

1. **Geçiş ayarlarını yapılandırma** ekranında şu bilgileri girin:

    | | |
    |--------|---------|
    |**SMB Ağ konumu paylaşımı** | Azure Veritabanı Geçiş Hizmeti'nin geçiş için kullanabileceği tam veritabanı yedekleme dosyalarını ve işlem günlüğü yedekleme dosyalarını içeren yerel SMB ağ paylaşımıdır. Kaynak SQL Server örneğini çalıştıran hizmet hesabının ağ paylaşımında okuma/yazma ayrıcalıkları olmalıdır. Ağ paylaşımındaki bir sunucunun FQDN veya IP adresi değerini girin, örneğin: '\\\sunucuadi.etkialaniadi.com\yedeklemeklasoru' veya '\\\IP adresi\yedeklemeklasoru'.|
    |**Kullanıcı adı** | Windows kullanıcısının yukarıda belirttiğiniz ağ paylaşımında tam denetim ayrıcalığına sahip olduğundan emin olun. Azure Veritabanı Geçiş Hizmeti, geri yükleme işlemi için yedekleme dosyalarını Azure depolama kapsayıcısına yüklemek için kullanıcının kimlik bilgilerini kullanır. |
    |**Parola** | Kullanıcının parolası. |
    |**Azure Depolama Hesabının aboneliği** | Azure Depolama Hesabını içeren aboneliği seçin. |
    |**Azure Depolama Hesabı** | DMS'nin SMB ağ paylaşımındaki yedekleme dosyalarını yükleyebileceği ve veritabanı geçişi için kullanabileceği Azure Depolama Hesabını seçin.  En iyi dosya yükleme performansı için DMS hizmetiyle aynı bölgede bir Depolama Hesabı seçmenizi öneririz. |

    ![Geçiş Ayarlarını Yapılandırma](media/tutorial-sql-server-to-managed-instance-online/dms-configure-migration-settings4.png)

2. **Kaydet**’i seçin.

## <a name="review-the-migration-summary"></a>Geçiş özetini gözden geçirme

1. **Geçiş özeti** ekranının **Etkinlik adı** metin kutusunda geçiş etkinliği için bir ad belirtin.

2. Geçiş projesiyle ilgili ayrıntıları gözden geçirin ve doğrulayın.

    ![Geçiş projesi özeti](media/tutorial-sql-server-to-managed-instance-online/dms-project-summary3.png)

## <a name="run-and-monitor-the-migration"></a>Geçişi çalıştırma ve izleme

1. **Geçişi çalıştır**'ı seçin.

2. Geçiş etkinliği ekranını güncelleştirmek için **Yenile**'yi seçin.

   ![Geçiş etkinliği sürüyor](media/tutorial-sql-server-to-managed-instance-online/dms-monitor-migration2.png)

    İlgili sunucu nesnelerinin geçiş durumunu izlemek için veritabanları ve oturum açma işlemleri kategorilerini genişletebilirsiniz.

   ![Geçiş etkinliği sürüyor](media/tutorial-sql-server-to-managed-instance-online/dms-monitor-migration-extend2.png)

## <a name="performing-migration-cutover"></a>Tam geçişi gerçekleştirme

Tam veritabanı yedeklemesini Azure SQL veritabanı yönetilen örneği hedef örneğini geri yüklendikten sonra veritabanı tam geçişi bir geçiş gerçekleştirmek için kullanılabilir.

1. Çevrimiçi veritabanı geçişini tamamlamaya hazır olduğunuzda **Tam Geçişi Başlat**'ı seçin.

2. Kaynak veritabanlarına gelen tüm trafiği durdurun.

3. [Sonradan alınan günlük yedeğini] alın, yedekleme dosyasını SMB ağ paylaşımına yerleştirin ve bu son işlem günlüğü yedeği geri yüklenene kadar bekleyin.

    Bu noktada **Bekleyen değişiklikler** 0 olmalıdır.

4. **Onayla**'yı ve ardından, **Uygula**'yı seçin.

    ![Tam geçişi tamamlamaya hazırlanma](media/tutorial-sql-server-to-managed-instance-online/dms-complete-cutover.png)

5. Veritabanı geçiş durumu görüntülendiğinde **tamamlandı**, uygulamalarınızı Azure SQL veritabanı yönetilen örneği'nın yeni hedef örneğine bağlanın.

    ![Tam geçiş tamamlandı](media/tutorial-sql-server-to-managed-instance-online/dms-cutover-complete.png)

## <a name="next-steps"></a>Sonraki adımlar

- Bir veritabanı T-SQL RESTORE komutunu kullanarak bir yönetilen örneğe geçiş adımlarını gösteren bir öğretici için bkz [bir yedekleme geri yükleme komutunu kullanarak bir yönetilen örneğine geri](../sql-database/sql-database-managed-instance-restore-from-backup-tutorial.md).
- Yönetilen örnek hakkında daha fazla bilgi için bkz. [yönetilen örnek nedir](../sql-database/sql-database-managed-instance.md).
- Uygulamalar bir yönetilen örneğe bağlanma hakkında daha fazla bilgi için bkz: [uygulamaları bağlama](../sql-database/sql-database-managed-instance-connect-app.md).
