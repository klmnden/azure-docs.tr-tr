---
title: SQL Server Integration Services paketlerini Azure SQL veritabanı yönetilen örneğine geçirme | Microsoft Docs
description: Bir Azure SQL veritabanı yönetilen örneği SQL Server Integration Services paketlerini geçirmeyi öğrenin.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/08/2019
ms.openlocfilehash: 82a047616c199e37bfa22f53e02f3f7b224b47c1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67083190"
---
# <a name="migrate-sql-server-integration-services-packages-to-an-azure-sql-database-managed-instance"></a>SQL Server Integration Services paketlerini Azure SQL veritabanı yönetilen örneğine geçirme
SQL Server Integration Services (SSIS) kullanın ve bir Azure SQL veritabanı yönetilen örneği tarafından barındırılan SSISDB hedef SQL Server tarafından barındırılan SSISDB kaynağından SSIS projelerini/paketlerini geçiş yapmak istiyorsanız, Azure veritabanı geçiş hizmeti kullanabilirsiniz.

SSIS kullandığınız sürümü 2012'den daha eski veya geçiş, SSIS projelerini/paketlerini önce SSISDB olmayan paket deposu türlerini kullanın tümleştirme hizmetleri proje dönüştürme SSMS başlatılabilir Sihirbazı'nı kullanarak bunları dönüştürmeniz gerekir. Daha fazla bilgi için bkz [projeleri için proje dağıtım modeli dönüştürme](https://docs.microsoft.com/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages?view=sql-server-2017#convert).

> [!NOTE]
> Azure veritabanı geçiş hizmeti (DMS) geçişi hedef Azure SQL veritabanı şu anda desteklemiyor. SSIS projelerini/paketlerini Azure SQL veritabanı yeniden dağıtmak için makalesine bakın. [yeniden SQL Server Integration Services paketlerini Azure SQL veritabanı](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages).

Bu makalede şunları öğreneceksiniz:
> [!div class="checklist"]
>
> * Kaynak SSIS projelerini/paketlerini değerlendirin.
> * SSIS projelerini/paketlerini Azure'a geçirin.

## <a name="prerequisites"></a>Önkoşullar

Bu adımları tamamlamak için ihtiyacınız vardır:

* Kullanarak şirket içi kaynak sunucularınıza siteden siteye bağlantı sağlar Azure Resource Manager dağıtım modelini kullanarak bir Azure sanal ağı (VNet) için Azure veritabanı geçiş hizmeti oluşturmak için [ExpressRoute ](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Daha fazla bilgi için bkz [ağ topolojileri için Azure SQL veritabanı yönetilen örneği geçişlerinin Azure veritabanı geçiş hizmetini kullanarak]( https://aka.ms/dmsnetworkformi). Sanal ağ oluşturma hakkında daha fazla bilgi için bkz. [sanal ağ belgeleri](https://docs.microsoft.com/azure/virtual-network/)ve özellikle hızlı başlangıç makalelerini ile adım adım ayrıntıları.
* VNet ağ güvenlik grubu kurallarınızı aşağıdaki gelen iletişim bağlantı noktaları için Azure veritabanı geçiş hizmeti engelleme emin olmak için: 443, 53, 9354, 445, 12000. Azure VNet NSG trafik filtreleme hakkında daha fazla ayrıntı için bkz [ağ güvenlik grupları ile ağ trafiğini filtreleme](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm).
* Yapılandırmak için [kaynak veritabanı altyapısı erişimi için Windows Güvenlik Duvarı](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access?view=sql-server-2017).
* Varsayılan olarak 1433 numaralı TCP bağlantı noktası olan SQL Server kaynağına erişmek Azure veritabanı geçiş hizmeti izin vermek için Windows Güvenlik duvarını açmak için.
* Dinamik bağlantı noktası kullanarak birden fazla adlandırılmış SQL Server örneği çalıştırıyorsanız Azure Veritabanı Geçiş Hizmeti'nin kaynak sunucunuzdaki adlandırılmış örneğe bağlanabilmesi için SQL Browser Hizmeti'ni etkinleştirebilir ve güvenlik duvarınızda 1434 numaralı UDP bağlantı noktasına erişim izni verebilirsiniz.
* Kaynak veritabanlarınızın önünde bir güvenlik duvarı cihazı kullanıyorsanız Azure Veritabanı Geçiş Hizmeti'nin geçiş amacıyla kaynak veritabanlarına ve 445 numaralı SMB bağlantı noktası aracılığıyla dosyalara erişmesi için güvenlik duvarı kuralları eklemeniz gerekebilir.
* Azure SQL veritabanı yönetilen örneği SSISDB'yi barındırmak için. Makalesinde ayrıntılı olarak oluşturmak ihtiyacınız varsa izleyin [bir Azure SQL veritabanı yönetilen örneği oluşturma](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started).
* SQL Server ve hedef yönetilen örnek kaynağına bağlanmak için kullanılan oturum açma bilgileri sağlamak için sysadmin sunucu rolünün üyeleri olan.
* Yönetilen örnek olarak Azure Data Factory (' % s'hedef Azure SQL veritabanı tarafından barındırılan SSISDB ile Azure-SSIS Integration Runtime (IR) içeren ADF) SSIS sağlandığından emin olun (makalesinde açıklandığı şekilde [Azure-SSIS oluşturma Azure Data factory'de tümleştirme çalışma zamanı](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)).

## <a name="assess-source-ssis-projectspackages"></a>Kaynak SSIS projelerini/paketlerini değerlendirmek

Değerlendirme kaynağı SSISDB veritabanı geçiş Yardımcısı (DMA içine) henüz tümleşik değil, ancak SSIS projelerini/paketlerini değerlendirilen/SSISDB barındırılan bir Azure SQL veritabanı yönetilen örneği hedefine imzalanmasını gibi doğrulanması.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını kaydetme

1. Azure portal'da oturum açın, **Tüm hizmetler** seçeneğini belirleyin ve ardından **Abonelikler**'i seçin.

    ![Portal aboneliklerini gösterme](media/how-to-migrate-ssis-packages-mi/portal-select-subscriptions.png)

2. Azure veritabanı geçiş hizmeti örneği oluşturun ve ardından istediğiniz aboneliği seçin **kaynak sağlayıcıları**.

    ![Kaynak sağlayıcılarını gösterme](media/how-to-migrate-ssis-packages-mi/portal-select-resource-provider.png)

3. "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.

    ![Kaynak sağlayıcısını kaydetme](media/how-to-migrate-ssis-packages-mi/portal-register-resource-provider.png)

## <a name="create-an-azure-database-migration-service-instance"></a>Azure Veritabanı Geçiş Hizmeti örneğini oluşturma

1. Azure portalda +**Kaynak oluştur**'u seçin, **Azure Veritabanı Geçiş Hizmeti** araması yapın ve açılan listeden **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

     ![Azure Market](media/how-to-migrate-ssis-packages-mi/portal-marketplace.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında **Oluştur**'u seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğini oluşturma](media/how-to-migrate-ssis-packages-mi/dms-create1.png)

3. **Geçiş Hizmeti oluşturun** ekranında hizmet için bir ad belirtin, aboneliği ve yeni ya da var olan bir kaynak grubunu seçin.

4. DMS örneğini oluşturmak istediğiniz konumu seçin.

5. Mevcut bir VNet seçin veya oluşturun.

    VNet Azure veritabanı geçiş hizmeti SQL Server kaynak ve hedef Azure SQL veritabanı yönetilen örneğine erişim sağlar.

    Azure portalında VNet oluşturma hakkında daha fazla bilgi için bkz [Azure portalını kullanarak bir sanal ağ oluşturma](https://aka.ms/DMSVnet).

    Ek ayrıntılar için bkz [kullanarak Azure veritabanı geçiş hizmeti örneği geçişlerinin ağ topolojileri için Azure SQL DB yönetilen](https://aka.ms/dmsnetworkformi).

6. Fiyatlandırma katmanını seçin.

    Maliyetler ve fiyatlandırma katmanları hakkında daha fazla bilgi için [fiyatlandırma sayfasına](https://aka.ms/dms-pricing) bakın.

    ![DMS hizmetini başlatma](media/how-to-migrate-ssis-packages-mi/dms-create-service2.png)

7. Hizmeti oluşturmak için **Oluştur**’u seçin.

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma

Hizmetin bir örneği oluşturulduktan sonra Azure portaldan bulun, açın ve yeni bir geçiş projesi oluşturun.

1. Azure portalda **Tüm hizmetler**'i seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

    ![Azure Veritabanı Geçiş Hizmeti’nin tüm örneklerini bulma](media/how-to-migrate-ssis-packages-mi/dms-search.png)

2. **Azure Veritabanı Geçiş Hizmeti ekranında** oluşturduğunuz örneğin adını arayın ve sonuçlardan bu örneği seçin.

3. +**Yeni Geçiş Projesi**'ni seçin.

4. Üzerinde **yeni geçiş projesi** projesi için bir ad belirtin, ekran **kaynak sunucu türü** metin kutusunda **SQL Server**, **hedef sunucu tür** metin kutusunda **Azure SQL veritabanı yönetilen örneği**ve ardından **etkinlik türünü seçin**seçin **SSIS paketi geçiş**.

   ![DMS projesi oluşturma](media/how-to-migrate-ssis-packages-mi/dms-create-project2.png)

5. Projeyi oluşturmak için **Oluştur**'u seçin.

## <a name="specify-source-details"></a>Kaynak ayrıntılarını belirtme

1. **Geçiş kaynağı ayrıntıları** ekranında SQL Server bağlantı ayrıntılarını belirtin.

2. Sunucunuza bir güvenilir sertifika yüklemediyseniz **Sunucu sertifikasına güven** onay kutusunu işaretleyin.

    Güvenilir sertifika yüklü değilse SQL Server, örnek başlatıldığında otomatik olarak imzalanan bir sertifika oluşturur. Bu sertifika, istemci bağlantılarında kimlik bilgilerini şifrelemek için kullanılır.

    > [!CAUTION]
    > Otomatik olarak imzalanan sertifika kullanarak şifrelenmiş SSL bağlantıları yüksek güvenlik sağlamaz. Ortadaki adam saldırılarına maruz kalabilirler. Üretim ortamında veya internete bağlı sunucularda otomatik olarak imzalanan sertifika ile SSL kullanımına güvenmemeniz gerekir.

   ![Kaynak Ayrıntıları](media/how-to-migrate-ssis-packages-mi/dms-source-details1.png)

3. **Kaydet**’i seçin.

## <a name="specify-target-details"></a>Hedef ayrıntılarını belirtme

1. Üzerinde **geçiş hedef ayrıntıları** ekranında, hedef bağlantı ayrıntılarını belirtin.

     ![Hedef ayrıntıları](media/how-to-migrate-ssis-packages-mi/dms-target-details2.png)

2. **Kaydet**’i seçin.

## <a name="review-the-migration-summary"></a>Geçiş özetini gözden geçirme

1. **Geçiş özeti** ekranının **Etkinlik adı** metin kutusunda geçiş etkinliği için bir ad belirtin.

2. İçin **SSIS projeleri ve bırakmaya üzerine seçeneği**, üzerine veya var olan SSIS projelerini ve ortamları yoksay belirtin.

    ![Geçiş projesi özeti](media/how-to-migrate-ssis-packages-mi/dms-project-summary2.png)

3. Geçiş projesiyle ilgili ayrıntıları gözden geçirin ve doğrulayın.

## <a name="run-the-migration"></a>Geçişi çalıştırma

* **Geçişi çalıştır**'ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

* Microsoft Geçiş Kılavuzu gözden [veritabanı Geçiş Kılavuzu](https://datamigration.microsoft.com/).
