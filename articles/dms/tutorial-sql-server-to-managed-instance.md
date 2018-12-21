---
title: 'Öğretici: Azure SQL Veritabanı Yönetilen Örneği’ne geçirmek için DMS kullanma | Microsoft Docs'
description: Azure Veritabanı Geçiş Hizmeti'ni kullanarak şirket içi SQL Server'ı çevrimdışı Azure SQL Veritabanı Yönetilen Örneği'ne geçirmeyi öğrenin.
services: dms
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: douglasl
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 10/10/2018
ms.openlocfilehash: e9baf8c838da2201fbb588d278cbf1ce5bbe6354
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53713328"
---
# <a name="tutorial-migrate-sql-server-to-azure-sql-database-managed-instance-offline-using-dms"></a>Öğretici: DMS kullanarak çevrimdışı biçimde SQL Server'ı Azure SQL Veritabanı Yönetilen Örneği'ne geçirme
Azure Veritabanı Geçiş Hizmeti'ni kullanarak şirket içi SQL Server örneğindeki veritabanlarını [Azure SQL Veritabanı Yönetilen Örneği](../sql-database/sql-database-managed-instance.md)'ne geçirebilirsiniz. El ile gerçekleştirilmesi gereken adımlar içeren ek yöntemler için bkz. [Azure SQL Veritabanı Yönetilen Örneği'ne SQL Server örneği geçişi](../sql-database/sql-database-managed-instance-migrate.md).

Bu öğreticide şirket içi SQL Server örneğindeki **Adventureworks2012** veritabanını Azure Veritabanı Geçiş Hizmeti'ni kullanarak bir Azure SQL Veritabanı Yönetilen Örneği'ne geçireceksiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Azure Veritabanı Geçiş Hizmeti örneği oluşturma.
> * Azure Veritabanı Geçiş Hizmeti'ni kullanarak geçiş projesi oluşturma.
> * Geçişi çalıştırma.
> * Geçişi izleme.
> * Geçiş raporu indirme.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Bu makalede, SQL Server'dan Azure SQL Veritabanı Yönetilen Örneği’ne çevrimdışı bir geçiş açıklanılır. Çevrimiçi geçiş için bkz. [DMS kullanarak çevrimiçi biçimde SQL Server'ı Azure SQL Veritabanı Yönetilen Örneği'ne geçirme](tutorial-sql-server-managed-instance-online.md).

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
- Azure Veritabanı Geçiş Hizmeti'nin kaynak veritabanını yedeklemek için kullanabileceği bir ağ paylaşımı oluşturun.
- Kaynak SQL Server örneğini çalıştıran hizmet hesabının oluşturduğunuz ağ paylaşımında yazma ayrıcalıklarına sahip olduğundan ve kaynak sunucunun bilgisayar hesabının aynı paylaşımda okuma/yazma erişimine sahip olduğundan emin olun.
- Önceden oluşturduğunuz ağ paylaşımında tam denetim ayrıcalığına sahip olan Windows kullanıcısını (ve parolasını) not edin. Azure Veritabanı Geçiş Hizmeti, geri yükleme işlemi için yedekleme dosyalarını Azure depolama kapsayıcısına yüklemek için kullanıcının kimlik bilgilerini kullanır.
- Bir blob kapsayıcısı oluşturun ve [Depolama Gezgini ile Azure Blob Depolama kaynaklarını yönetme](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container) makalesindeki adımları kullanarak SAS URI'sini alın. SAS URI değerini oluştururken ilke penceresinde tüm izinleri (Okuma, Yazma, Silme, Listeleme) seçtiğinizden emin olun. Bu ayrıntı, Azure Veritabanı Geçiş Hizmeti'ne veritabanlarını Azure SQL Veritabanı Yönetilen Örneği'ne geçirmek için kullanılan yedekleme dosyalarını yükleme amacıyla depolama hesabı kapsayıcınıza erişim izni verir.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını kaydetme

1. Azure portal'da oturum açın, **Tüm hizmetler** seçeneğini belirleyin ve ardından **Abonelikler**'i seçin.

    ![Portal aboneliklerini gösterme](media/tutorial-sql-server-to-managed-instance/portal-select-subscriptions.png)        

2. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve sonra **Kaynak sağlayıcıları**’nı seçin.

    ![Kaynak sağlayıcılarını gösterme](media/tutorial-sql-server-to-managed-instance/portal-select-resource-provider.png)

3. "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.

    ![Kaynak sağlayıcısını kaydetme](media/tutorial-sql-server-to-managed-instance/portal-register-resource-provider.png)   

## <a name="create-an-azure-database-migration-service-instance"></a>Azure Veritabanı Geçiş Hizmeti örneğini oluşturma

1. Azure portalda +**Kaynak oluştur**'u seçin, **Azure Veritabanı Geçiş Hizmeti** araması yapın ve açılan listeden **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

     ![Azure Market](media/tutorial-sql-server-to-managed-instance/portal-marketplace.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında **Oluştur**'u seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğini oluşturma](media/tutorial-sql-server-to-managed-instance/dms-create1.png)

3. **Geçiş Hizmeti oluşturun** ekranında hizmet için bir ad belirtin, aboneliği ve yeni ya da var olan bir kaynak grubunu seçin.

4.  DMS örneğini oluşturmak istediğiniz konumu seçin.

5. Var olan bir sanal ağı (VNET) seçin veya bir tane oluşturun.
 
    Sanal ağ, Azure Veritabanı Geçiş Hizmeti'nin kaynak SQL Server ve hedef Azure SQL Veritabanı Yönetilen Örneği'ne erişmesini sağlar.

    Azure portalda sanal ağ oluşturma hakkında daha fazla bilgi için [Azure portalı kullanarak sanal ağ oluşturma](https://aka.ms/DMSVnet) makalesine bakın.

    Daha fazla ayrıntı için bkz. [Azure Veritabanı Geçiş Hizmeti'ni kullanarak yapılan Azure SQL Veritabanı Yönetilen Örnek geçişleri ağ topolojileri](https://aka.ms/dmsnetworkformi).

6. Fiyatlandırma katmanını seçin.

    Maliyetler ve fiyatlandırma katmanları hakkında daha fazla bilgi için [fiyatlandırma sayfasına](https://aka.ms/dms-pricing) bakın.
   
    ![DMS hizmetini başlatma](media/tutorial-sql-server-to-managed-instance/dms-create-service2.png)

7.  Hizmeti oluşturmak için **Oluştur**’u seçin.

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma

Hizmetin bir örneği oluşturulduktan sonra Azure portaldan bulun, açın ve yeni bir geçiş projesi oluşturun.

1. Azure portalda **Tüm hizmetler**'i seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

    ![Azure Veritabanı Geçiş Hizmeti’nin tüm örneklerini bulma](media/tutorial-sql-server-to-managed-instance/dms-search.png)

2. **Azure Veritabanı Geçiş Hizmeti ekranında** oluşturduğunuz örneğin adını arayın ve sonuçlardan bu örneği seçin.
 
3. +**Yeni Geçiş Projesi**'ni seçin.

4. **Yeni geçiş projesi** ekranında proje için bir ad belirtin, **Kaynak sunucu türü** metin kutusunda **SQL Server**, **Hedef sunucu türü** metin kutusunda **Azure SQL Veritabanı Yönetilen Örneği** ve **Etkinlik türünü seçin** alanında **Çevrimdışı veri geçişi** seçimini yapın.

   ![DMS projesi oluşturma](media/tutorial-sql-server-to-managed-instance/dms-create-project2.png)

5. Projeyi oluşturmak için **Oluştur**'u seçin.

## <a name="specify-source-details"></a>Kaynak ayrıntılarını belirtme

1. **Geçiş kaynağı ayrıntıları** ekranında SQL Server bağlantı ayrıntılarını belirtin.

2. Sunucunuza bir güvenilir sertifika yüklemediyseniz **Sunucu sertifikasına güven** onay kutusunu işaretleyin.

    Güvenilir sertifika yüklü değilse SQL Server, örnek başlatıldığında otomatik olarak imzalanan bir sertifika oluşturur. Bu sertifika, istemci bağlantılarında kimlik bilgilerini şifrelemek için kullanılır.

    > [!CAUTION]
    > Otomatik olarak imzalanan sertifika kullanarak şifrelenmiş SSL bağlantıları yüksek güvenlik sağlamaz. Ortadaki adam saldırılarına maruz kalabilirler. Üretim ortamında veya internete bağlı sunucularda otomatik olarak imzalanan sertifika ile SSL kullanımına güvenmemeniz gerekir.

   ![Kaynak Ayrıntıları](media/tutorial-sql-server-to-managed-instance/dms-source-details1.png)

3. **Kaydet**’i seçin.

4. **Kaynak veritabanlarını seçin** ekranında geçiş için **Adventureworks2012** veritabanını seçin.

   ![Kaynak veritabanlarını seçme](media/tutorial-sql-server-to-managed-instance/dms-source-database1.png)

5. **Kaydet**’i seçin.

## <a name="specify-target-details"></a>Hedef ayrıntılarını belirtme

1.  **Geçiş hedefi ayrıntıları** ekranında **AdventureWorks2012** veritabanını geçireceğiniz hedef (önceden sağlanmış Azure SQL Veritabanı Yönetilen Örneği) için bağlantı ayrıntılarını belirtin.

    Azure SQL Veritabanı Yönetilen Örneği'ni sağlamadıysanız örneği sağlama konusunda yardım bağlantısı için **Hayır**'a tıklayın. Projeyi oluşturmaya devam edebilirsiniz ve Azure SQL Veritabanı Yönetilen Örneği hazır olduğunda bu projeye dönerek geçişi yürütebilirsiniz.   
 
       ![Hedef seçme](media/tutorial-sql-server-to-managed-instance/dms-target-details2.png)

2.  **Kaydet**’i seçin.

## <a name="select-source-databases"></a>Kaynak veritabanlarını seçme

1. **Kaynak veritabanlarını seçin** ekranında geçirmek istediğiniz kaynak veritabanını seçin.

    ![Kaynak veritabanlarını seçme](media/tutorial-sql-server-to-managed-instance/select-source-databases.png)

2. **Kaydet**’i seçin.

## <a name="select-logins"></a>Oturum açmaları seçme
 
1. **Oturum açmaları seçin** ekranında geçirmek istediğiniz oturum açmaları seçin.

    >[!NOTE]
    >Bu sürüm yalnızca SQL oturum açma bilgilerinin geçirilmesini destekler.

    ![Oturum açmaları seçme](media/tutorial-sql-server-to-managed-instance/select-logins.png)

2. **Kaydet**’i seçin.
 
## <a name="configure-migration-settings"></a>Geçiş ayarlarını yapılandırma
 
1. **Geçiş ayarlarını yapılandırma** ekranında şu bilgileri girin:

    | | |
    |--------|---------|
    |**Kaynak yedekleme seçeneği seçin** | Veritabanı geçişinde kullanılacak DMS için tam yedekleme dosyaları varsa **En son yedekleme dosyalarını ben sağlayacağım** seçeneğini belirleyin. DMS'nin önce kaynak veritabanının tam yedeğini alıp geçiş için bu yedeği kullanmasını istiyorsanız **Azure Veritabanı Geçiş Hizmeti'nin yedek dosyalar oluşturmasına izin veriyorum** seçeneğini belirleyin. |
    |**Ağ konumu paylaşımı** | Azure Veritabanı Geçiş Hizmeti'nin kaynak veritabanı yedeklerini kaydedebileceği yerel SMB ağ paylaşımı. Kaynak SQL Server örneğini çalıştıran hizmet hesabının ağ paylaşımında yazma ayrıcalıkları olmalıdır. Ağ paylaşımındaki bir sunucunun FQDN veya IP adresi değerini girin, örneğin: '\\\sunucuadi.etkialaniadi.com\yedeklemeklasoru' veya '\\\IP adresi\yedeklemeklasoru'.|
    |**Kullanıcı adı** | Windows kullanıcısının yukarıda belirttiğiniz ağ paylaşımında tam denetim ayrıcalığına sahip olduğundan emin olun. Azure Veritabanı Geçiş Hizmeti, geri yükleme işlemi için yedekleme dosyalarını Azure depolama kapsayıcısına yüklemek için kullanıcının kimlik bilgilerini kullanır. Geçiş için TDE özelliğinin etkin olduğu veritabanlarının seçilmesi durumunda yukarıdaki Windows kullanıcısının yerleşik yönetici hesabı olması ve Azure Veritabanı Geçiş Hizmeti'nin sertifika dosyalarını yükleyip silmesi için [Kullanıcı Hesabı Denetimi](https://docs.microsoft.com/windows/security/identity-protection/user-account-control/user-account-control-overview) özelliğinin devre dışı bırakılmış olması gerekir. |
    |**Parola** | Kullanıcının parolası. |
    |**Depolama hesabı ayarları** | Azure Veritabanı Geçiş Hizmeti'ne veritabanlarını Azure SQL Veritabanı Yönetilen Örneği'ne geçirmek için kullanılan yedekleme dosyalarının yüklendiği depolama hesabı kapsayıcınıza erişim sağlayan SAS URI değeri. [Blob kapsayıcısı için SAS URI değerini almayı öğrenin](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container).|
    |**TDE Ayarları** | Geçirdiğiniz kaynak veritabanlarında Saydam Veri Şifrelemesi (TDE) özelliği etkinse hedef Azure SQL Veritabanı Yönetilen Örneği'nde yazma ayrıcalıklarına sahip olmanız gerekir.  Açılan menüden Azure SQL Veritabanı Yönetilen Örneği'nin sağlandığı aboneliği seçin.  Açılan menüden **Azure SQL Veritabanı Yönetilen Örneği** hedefini seçin. |
    
    ![Geçiş Ayarlarını Yapılandırma](media/tutorial-sql-server-to-managed-instance/dms-configure-migration-settings3.png)

2. **Kaydet**’i seçin.
 
## <a name="review-the-migration-summary"></a>Geçiş özetini gözden geçirme

1. **Geçiş özeti** ekranının **Etkinlik adı** metin kutusunda geçiş etkinliği için bir ad belirtin.

2. **Doğrulama seçeneği** bölümünü genişleterek **Doğrulama seçeneğini belirleyin** ekranını açın ve geçirilen veritabanlarında sorgu doğrulaması yapma tercihini belirtin ve **Kaydet**'i seçin.

3. Geçiş projesiyle ilgili ayrıntıları gözden geçirin ve doğrulayın.
 
    ![Geçiş projesi özeti](media/tutorial-sql-server-to-managed-instance/dms-project-summary2.png)

4.  **Kaydet**’i seçin.   

## <a name="run-the-migration"></a>Geçişi çalıştırma

- **Geçişi çalıştır**'ı seçin.

  Geçiş etkinliği penceresi açılır ve etkinliğin durum bilgisi **Beklemede** olarak değişir.

## <a name="monitor-the-migration"></a>Geçişi izleme

1. Geçiş etkinliği ekranını güncelleştirmek için **Yenile**'yi seçin.
 
   ![Geçiş etkinliği sürüyor](media/tutorial-sql-server-to-managed-instance/dms-monitor-migration1.png)

    İlgili sunucu nesnelerinin geçiş durumunu izlemek için veritabanları ve oturum açma işlemleri kategorilerini genişletebilirsiniz.

   ![Geçiş etkinliği sürüyor](media/tutorial-sql-server-to-managed-instance/dms-monitor-migration-extend.png)

2. Geçiş tamamlandıktan sonra **Raporu indir**'i seçerek geçiş işleminin ayrıntılarını içeren raporu indirebilirsiniz.
 
3. Hedef veritabanının hedef Azure SQL Veritabanı Yönetilen Örnek ortamında olduğunu doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- T-SQL RESTORE komutunu kullanarak veritabanını Yönetilen Örnek hedefine geçirme öğreticisi için bkz. [Geri yükleme komutunu kullanarak yedeklemeyi Yönetilen Örneğe geri yükleme](../sql-database/sql-database-managed-instance-restore-from-backup-tutorial.md).
- Yönetilen Örnek hakkında daha fazla bilgi için bkz. [Yönetilen Örnek nedir?](../sql-database/sql-database-managed-instance.md).
- Uygulamaları Yönetilen Örneğe bağlama hakkında daha fazla bilgi için bkz. [Uygulamaları bağlama](../sql-database/sql-database-managed-instance-connect-app.md).