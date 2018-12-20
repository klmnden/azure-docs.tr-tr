---
title: "Öğretici: SQL Server'dan Azure SQL Veritabanı Yönetilen Örneği'ne çevrimiçi geçiş gerçekleştirmek için Azure Veritabanı Geçiş Hizmeti'ni kullanma | Microsoft Docs"
description: Azure Veritabanı Geçiş Hizmeti'ni kullanarak şirket içi SQL Server'dan Azure SQL Veritabanı Yönetilen Örneği'ne çevrimiçi geçiş gerçekleştirmeyi öğrenin.
services: dms
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: ''
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 12/19/2018
ms.openlocfilehash: 3f52dde1e091ee089b83fe8f7f2d860941c11318
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53631456"
---
# <a name="tutorial-migrate-sql-server-to-azure-sql-database-managed-instance-online-using-dms"></a>Öğretici: DMS kullanarak çevrimiçi biçimde SQL Server'ı Azure SQL Veritabanı Yönetilen Örneği'ne geçirme
Azure Veritabanı Geçiş Hizmeti'ni kullanarak şirket içi SQL Server örneğindeki veritabanlarını minimum çalışmama süresi ile [Azure SQL Veritabanı Yönetilen Örneği](../sql-database/sql-database-managed-instance.md)'ne geçirebilirsiniz. El ile gerçekleştirilmesi gereken adımlar içeren ek yöntemler için bkz. [Azure SQL Veritabanı Yönetilen Örneği'ne SQL Server örneği geçişi](../sql-database/sql-database-managed-instance-migrate.md).

Bu öğreticide şirket içi SQL Server örneğindeki **Adventureworks2012** veritabanını Azure Veritabanı Geçiş Hizmeti'ni kullanarak minimum çalışmama süresi ile bir Azure SQL Veritabanı Yönetilen Örneği'ne geçireceksiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Azure Veritabanı Geçiş Hizmeti örneği oluşturma.
> * Azure Veritabanı Geçiş Hizmeti'ni kullanarak geçiş projesi oluşturma ve çevrimiçi geçişi başlatma.
> * Geçişi izleme.
> * Hazır olduğunuzda tam geçişi gerçekleştirme.

> [!NOTE]
> Azure veritabanı geçiş hizmeti çevrimiçi bir geçiş gerçekleştirmek için Premium fiyatlandırma katmanını temel alan bir örneği oluşturmanız gerekir.

> [!IMPORTANT]
> En iyi geçiş deneyimi için Microsoft, Azure Veritabanı Geçiş Hizmeti’nin bir örneğini hedef veritabanıyla aynı Azure bölgesinde oluşturmayı önerir. Verileri bölgeler veya coğrafyalar arasında taşımak, geçiş sürecini yavaşlatabilir ve hatalara neden olabilir.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Bu makalede, SQL Server'dan Azure SQL Veritabanı Yönetilen Örneği’ne çevrimiçi bir geçiş açıklanılır. Çevrimdışı geçiş için bkz. [DMS kullanarak çevrimdışı biçimde SQL Server'ı Azure SQL Veritabanı Yönetilen Örneği'ne geçirme](tutorial-sql-server-to-managed-instance.md).

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

- Azure Resource Manager dağıtım modelini kullanarak Azure Veritabanı Geçiş Hizmeti için [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) kullanarak şirket içi kaynak sunucularınıza siteden siteye bağlantı sağlayan bir sanal ağ oluşturun. [Azure Veritabanı Geçiş Hizmeti'ni kullanarak yapılan Azure SQL Veritabanı Yönetilen Örnek geçişleri ağ topolojileri hakkında bilgi edinin](https://aka.ms/dmsnetworkformi).
- Azure Sanal Ağ (VNET) Ağ Güvenlik Grubu kurallarının 443, 53, 9354, 445 ve 12000 numaralı iletişim bağlantı noktalarını engellemediğinden emin olun. Azure VNET NSG trafiğini filtreleme hakkında ayrıntılı bilgi için [Ağ güvenlik grupları ile ağ trafiğini filtreleme](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) makalesine bakın.
- [Windows Güvenlik Duvarınızı kaynak veritabanı altyapısı erişimi](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) için yapılandırın.
- Azure Veritabanı Geçiş Hizmeti'ne kaynak SQL Server erişimi sağlamak için Windows güvenlik duvarınızı açın. Varsayılan ayarlarda 1433 numaralı TCP bağlantı noktası kullanılır.
- Dinamik bağlantı noktası kullanarak birden fazla adlandırılmış SQL Server örneği çalıştırıyorsanız Azure Veritabanı Geçiş Hizmeti'nin kaynak sunucunuzdaki adlandırılmış örneğe bağlanabilmesi için SQL Browser Hizmeti'ni etkinleştirebilir ve güvenlik duvarınızda 1434 numaralı UDP bağlantı noktasına erişim izni verebilirsiniz.
- Kaynak veritabanlarınızın önünde bir güvenlik duvarı cihazı kullanıyorsanız Azure Veritabanı Geçiş Hizmeti'nin geçiş amacıyla kaynak veritabanlarına ve 445 numaralı SMB bağlantı noktası aracılığıyla dosyalara erişmesi için güvenlik duvarı kuralları eklemeniz gerekebilir.
- [Azure portalda Azure SQL Veritabanı Yönetilen Örneği oluşturma](https://aka.ms/sqldbmi) makalesindeki adımları izleyerek Azure SQL Veritabanı Yönetilen Örneği oluşturun.
- Kaynak SQL Server ve hedef Yönetilen Örnek bağlantısı kurmak için kullanılan oturum açma bilgilerinin sysadmin sunucu rolüne üye olduğundan emin olun.
- Azure Veritabanı Geçiş Hizmeti'nin veritabanı geçişi için kullanabileceği veritabanınızın tam veritabanı yedekleme dosyalarını ve işlem günlüğü yedekleme dosyalarını içeren bir SMB ağ paylaşımı sağlayın.
- Kaynak SQL Server örneğini çalıştıran hizmet hesabının oluşturduğunuz ağ paylaşımında yazma ayrıcalıklarına sahip olduğundan ve kaynak sunucunun bilgisayar hesabının aynı paylaşımda okuma/yazma erişimine sahip olduğundan emin olun.
- Önceden oluşturduğunuz ağ paylaşımında tam denetim ayrıcalığına sahip olan Windows kullanıcısını (ve parolasını) not edin. Azure Veritabanı Geçiş Hizmeti, geri yükleme işlemi için yedekleme dosyalarını Azure depolama kapsayıcısına yüklemek için kullanıcının kimlik bilgilerini kullanır.
- DMS Hizmeti'nin hedef Azure Veritabanı Yönetilen Örneği'ne ve Azure Depolama Kapsayıcısı'na bağlanmak için kullanabileceği Uygulama Kimliği anahtarını oluşturan bir Azure Active Directory Uygulama Kimliği oluşturun. Daha fazla bilgi için bkz. [Kaynaklara erişebilen bir Azure Active Directory uygulaması ve hizmet sorumlusu oluşturmak için portalı kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal).
- DMS hizmetinin veritabanı yedekleme dosyalarını yükleyebileceği ve veritabanlarını geçirmek için kullanabileceği bir **Standart Performans katmanı** Azure Depolama Hesabı oluşturun veya belirtin.  Azure Depolama Hesabının oluşturulan DMS hizmetiyle aynı bölgede olduğundan emin olun.

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

4.  DMS örneğini oluşturmak istediğiniz konumu seçin.

5. Var olan bir sanal ağı (VNET) seçin veya bir tane oluşturun.
 
    Sanal ağ, Azure Veritabanı Geçiş Hizmeti'nin kaynak SQL Server ve hedef Azure SQL Veritabanı Yönetilen Örneği'ne erişmesini sağlar.

    Azure portalda sanal ağ oluşturma hakkında daha fazla bilgi için [Azure portalı kullanarak sanal ağ oluşturma](https://aka.ms/DMSVnet) makalesine bakın.

    Daha fazla ayrıntı için bkz. [Azure Veritabanı Geçiş Hizmeti'ni kullanarak yapılan Azure SQL Veritabanı Yönetilen Örnek geçişleri ağ topolojileri](https://aka.ms/dmsnetworkformi).

6. Bir SKU Premium fiyatlandırma katmanı seçin.

    > [!NOTE]
    > Çevrimiçi geçişler yalnızca Premium katmanda kullanılırken desteklenir. 
   
    Maliyetler ve fiyatlandırma katmanları hakkında daha fazla bilgi için [fiyatlandırma sayfasına](https://aka.ms/dms-pricing) bakın.
   
    ![DMS hizmetini başlatma](media/tutorial-sql-server-to-managed-instance-online/dms-create-service3.png)

7.  Hizmeti oluşturmak için **Oluştur**’u seçin.

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

## <a name="specify-target-details"></a>Hedef ayrıntılarını belirtme

1.  **Geçiş hedef ayrıntıları** ekranında DMS örneğinin hedef Azure SQL Veritabanı Yönetilen Hizmeti ve Azure Depolama Hesabı örneğine bağlanmak için kullanabileceği **Uygulama Kimliği** ve **Anahtar** değerini belirtin.

    Daha fazla bilgi için bkz. [Kaynaklara erişebilen bir Azure Active Directory uygulaması ve hizmet sorumlusu oluşturmak için portalı kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal).
    
2. Hedef Azure SQL Veritabanı Yönetilen Örneği'ni içeren **Abonelik** girişini ve ardından hedef örneği seçin.

    Azure SQL Veritabanı Yönetilen Örneği'ni sağlamadıysanız örneği sağlama konusunda yardım almak için [bağlantıyı](https://aka.ms/SQLDBMI) seçin. Azure SQL Veritabanı Yönetilen Örneği hazır olduğunda bu projeye dönerek geçişi yürütebilirsiniz.

3. Hedef Azure SQL Veritabanı Yönetilen Örneği'ne bağlanmak için **SQL Kullanıcısı** ve **Parola** bilgilerini girin.

    ![Hedef seçme](media/tutorial-sql-server-to-managed-instance-online/dms-target-details3.png)

2.  **Kaydet**’i seçin.

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

Tam veritabanı yedeği hedef Azure SQL Veritabanı Yönetilen Örneği'ne geri yüklendikten sonra veritabanı için tam geçiş gerçekleştirilebilir.

1.  Çevrimiçi veritabanı geçişini tamamlamaya hazır olduğunuzda **Tam Geçişi Başlat**'ı seçin.

2.  Kaynak veritabanlarına gelen tüm trafiği durdurun.

3.  [Sonradan alınan günlük yedeğini] alın, yedekleme dosyasını SMB ağ paylaşımına yerleştirin ve bu son işlem günlüğü yedeği geri yüklenene kadar bekleyin.

    Bu noktada **Bekleyen değişiklikler** 0 olmalıdır.

4.  **Onayla**'yı ve ardından, **Uygula**'yı seçin.

    ![Tam geçişi tamamlamaya hazırlanma](media/tutorial-sql-server-to-managed-instance-online/dms-complete-cutover.png)

5.  Veritabanı geçişi durumu **Tamamlandı** olarak gösterildiğinde, uygulamalarınızı yeni hedef Azure SQL Veritabanı Yönetilen Örneği'ne bağlayın.

    ![Tam geçiş tamamlandı](media/tutorial-sql-server-to-managed-instance-online/dms-cutover-complete.png)

## <a name="next-steps"></a>Sonraki adımlar

- T-SQL RESTORE komutunu kullanarak veritabanını Yönetilen Örnek hedefine geçirme öğreticisi için bkz. [Geri yükleme komutunu kullanarak yedeklemeyi Yönetilen Örneğe geri yükleme](../sql-database/sql-database-managed-instance-restore-from-backup-tutorial.md).
- Yönetilen Örnek hakkında daha fazla bilgi için bkz. [Yönetilen Örnek nedir?](../sql-database/sql-database-managed-instance.md).
- Uygulamaları Yönetilen Örneğe bağlama hakkında daha fazla bilgi için bkz. [Uygulamaları bağlama](../sql-database/sql-database-managed-instance-connect-app.md).
