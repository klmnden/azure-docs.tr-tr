---
title: Azure veritabanı geçiş hizmeti kullanma önkoşulları genel bakış | Microsoft Docs
description: Bir genel bakış veritabanı geçişleri gerçekleştirmek için Azure veritabanı geçiş hizmeti kullanmak için önkoşulları hakkında bilgi edinin.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 05/29/2019
ms.openlocfilehash: 4e21014f7b4ed86846a100ed9a2b1cd4b0400974
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66304266"
---
# <a name="overview-of-prerequisites-for-using-the-azure-database-migration-service"></a>Azure veritabanı geçiş hizmeti kullanma önkoşulları genel bakış

Azure veritabanı geçiş Hizmeti'nin veritabanı geçişlerini gerçekleştirirken düzgün çalıştığından emin olmak için gereken birkaç önkoşul vardır. Bazı Önkoşullar, diğer ön koşulları için belirli bir senaryoya benzersiz çalışırken service tarafından desteklenen tüm senaryolarda (kaynak-hedef çiftlerinin) arasında geçerlidir.

Azure veritabanı geçiş hizmeti kullanımıyla ilişkili Önkoşullar aşağıdaki bölümlerde listelenmiştir.

## <a name="prerequisites-common-across-migration-scenarios"></a>Geçiş senaryoları arasında ortak Önkoşullar

Tüm desteklenen geçiş senaryoları arasında ortak olan azure veritabanı geçiş hizmeti Önkoşullar dahil etme gereksinimi:

* Kullanarak şirket içi kaynak sunucularınıza siteden siteye bağlantı sağlar Azure Resource Manager dağıtım modelini kullanarak bir Azure sanal ağı (VNet) için Azure veritabanı geçiş hizmeti oluşturma [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
* VNet ağ güvenlik grubu (NSG) kuralları aşağıdaki engelleme olun iletişim bağlantı noktası 443, 53, 9354, 445, 12000. Azure VNet NSG trafik filtreleme hakkında daha fazla ayrıntı için bkz [ağ güvenlik grupları ile ağ trafiğini filtreleme](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
* Kaynak veritabanlarınız önünde bir güvenlik duvarı Gereci kullanırken oluşan kaynak veritabanları geçiş için erişmek Azure veritabanı geçiş hizmeti izin vermek için güvenlik duvarı kuralları eklemeniz gerekebilir.
* [Windows Güvenlik Duvarınızı veritabanı altyapısı erişimi](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) için yapılandırın.
* [Sunucu Ağ Protokolünü Etkinleştirme veya Devre Dışı Bırakma](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure) makalesindeki yönergeleri izleyerek SQL Server Express yüklemesi sırasında varsayılan olarak devre dışı bırakılan TCP/IP protokolünü etkinleştirin.

    > [!IMPORTANT]
    > Azure veritabanı geçiş hizmeti örneği oluşturma aynı kaynak grubu içinde yer almayan normalde sanal ağ ayarlarını erişim gerektirir. Sonuç olarak, DMS bir örneğini oluşturan kullanıcıya abonelik düzeyinde izni gerektirir. Gerektiği şekilde atayabilir, gerekli rolleri oluşturmak için aşağıdaki betiği çalıştırın:
    >
    > ```
    >
    > $readerActions = `
    > "Microsoft.DataMigration/services/*/read", `
    > "Microsoft.Network/networkInterfaces/ipConfigurations/read"
    >
    > $writerActions = `
    > "Microsoft.DataMigration/services/*/write", `
    > "Microsoft.DataMigration/services/*/delete", `
    > "Microsoft.DataMigration/services/*/action"
    >
    > $writerActions += $readerActions
    >
    > # TODO: replace with actual subscription IDs
    > $subScopes = ,"/subscriptions/00000000-0000-0000-0000-000000000000/","/subscriptions/11111111-1111-1111-1111-111111111111/"
    >
    > function New-DmsReaderRole() {
    > $aRole = [Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition]::new()
    > $aRole.Name = "Azure Database Migration Reader"
    > $aRole.Description = "Lets you perform read only actions on DMS service/project/tasks."
    > $aRole.IsCustom = $true
    > $aRole.Actions = $readerActions
    > $aRole.NotActions = @()
    >
    > $aRole.AssignableScopes = $subScopes
    > #Create the role
    > New-AzRoleDefinition -Role $aRole
    > }
    >
    > function New-DmsContributorRole() {
    > $aRole = [Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition]::new()
    > $aRole.Name = "Azure Database Migration Contributor"
    > $aRole.Description = "Lets you perform CRUD actions on DMS service/project/tasks."
    > $aRole.IsCustom = $true
    > $aRole.Actions = $writerActions
    > $aRole.NotActions = @()
    >
    >   $aRole.AssignableScopes = $subScopes
    > #Create the role
    > New-AzRoleDefinition -Role $aRole
    > }
    > 
    > function Update-DmsReaderRole() {
    > $aRole = Get-AzRoleDefinition "Azure Database Migration Reader"
    > $aRole.Actions = $readerActions
    > $aRole.NotActions = @()
    > Set-AzRoleDefinition -Role $aRole
    > }
    >
    > function Update-DmsConributorRole() {
    > $aRole = Get-AzRoleDefinition "Azure Database Migration Contributor"
    > $aRole.Actions = $writerActions
    > $aRole.NotActions = @()
    > Set-AzRoleDefinition -Role $aRole
    > }
    >
    > # Invoke above functions
    > New-DmsReaderRole
    > New-DmsContributorRole
    > Update-DmsReaderRole
    > Update-DmsConributorRole
    > ```

## <a name="prerequisites-for-migrating-sql-server-to-azure-sql-database"></a>Azure SQL veritabanına geçirme SQL Server için Önkoşullar

Tüm geçiş senaryoları için ortak olan Azure veritabanı geçiş hizmeti önkoşullarının yanı sıra da özellikle bir senaryo veya başka bir geçerli önkoşulları vardır.

SQL Server önkoşulları tüm geçiş senaryoları için ortak olan ek olarak, Azure SQL veritabanı geçişleri gerçekleştirmek için Azure veritabanı geçiş hizmeti kullanırken aşağıdaki ek önkoşulları adres emin olun:

* Makalesinde ayrıntılı olarak C izleyerek bunu Azure SQL veritabanı örneğine bir örneğini oluşturmak[Azure portalında bir Azure SQL veritabanı oluştur](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal).
* [Data Migration Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595) 3.3 veya üzeri sürümünü indirip yükleyin.
* Azure Veritabanı Geçiş Hizmeti'ne kaynak SQL Server erişimi sağlamak için Windows güvenlik duvarınızı açın. Varsayılan ayarlarda 1433 numaralı TCP bağlantı noktası kullanılır.
* Dinamik bağlantı noktası kullanarak birden fazla adlandırılmış SQL Server örneği çalıştırıyorsanız, Azure Veritabanı Geçiş Hizmeti'nin kaynak sunucunuzdaki adlandırılmış örneğe bağlanabilmesi için SQL Browser Hizmeti'ni etkinleştirebilir ve güvenlik duvarınızda 1434 numaralı UDP bağlantı noktasına erişim izni verebilirsiniz.
* Azure Veritabanı Geçiş Hizmeti'nin hedef veritabanlarına erişmesini sağlama amacıyla Azure SQL Veritabanı için sunucu düzeyinde [güvenlik duvarı kuralı](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) oluşturun. Azure Veritabanı Geçiş Hizmeti için kullanılan sanal ağın alt ağ aralığını belirtin.
* SQL Server örneğine bağlanmak için kullanılan kimlik bilgilerinin [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) izinlerine sahip olduğundan emin olun.
* Hedef Azure SQL Veritabanı örneğine bağlanmak için kullanılan kimlik bilgilerinin hedef Azure SQL veritabanlarında CONTROL DATABASE iznine sahip olduğundan emin olun.

   > [!NOTE]
   > Azure veritabanı geçiş hizmeti, SQL Server'dan Azure SQL veritabanı'na geçişler gerçekleştirmeyi kullanmak için gereken önkoşulları tam listesi için bkz [Azure SQL veritabanı için SQL Server'ı geçirme](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql).
   > 

## <a name="prerequisites-for-migrating-sql-server-to-an-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneğine geçirme SQL Server için Önkoşullar

* Makalesinde ayrıntılı olarak bir Azure SQL veritabanı yönetilen örneği oluşturma [Azure portalında Azure SQL veritabanı yönetilen örneği oluşturma](https://aka.ms/sqldbmi).
* SMB trafiği için Azure veritabanı geçiş hizmeti IP adresi veya alt ağ aralığı 445 numaralı bağlantı noktasında izin vermek için güvenlik duvarlarınızdan açın.
* Azure Veritabanı Geçiş Hizmeti'ne kaynak SQL Server erişimi sağlamak için Windows güvenlik duvarınızı açın. Varsayılan ayarlarda 1433 numaralı TCP bağlantı noktası kullanılır.
* Dinamik bağlantı noktası kullanarak birden fazla adlandırılmış SQL Server örneği çalıştırıyorsanız, Azure Veritabanı Geçiş Hizmeti'nin kaynak sunucunuzdaki adlandırılmış örneğe bağlanabilmesi için SQL Browser Hizmeti'ni etkinleştirebilir ve güvenlik duvarınızda 1434 numaralı UDP bağlantı noktasına erişim izni verebilirsiniz.
* Kaynak SQL Server ve hedef Yönetilen Örnek bağlantısı kurmak için kullanılan oturum açma bilgilerinin sysadmin sunucu rolüne üye olduğundan emin olun.
* Azure Veritabanı Geçiş Hizmeti'nin kaynak veritabanını yedeklemek için kullanabileceği bir ağ paylaşımı oluşturun.
* Kaynak SQL Server örneğini çalıştıran hizmet hesabının oluşturduğunuz ağ paylaşımında yazma ayrıcalıklarına sahip olduğundan ve kaynak sunucunun bilgisayar hesabının aynı paylaşımda okuma/yazma erişimine sahip olduğundan emin olun.
* Önceden oluşturduğunuz ağ paylaşımında tam denetim ayrıcalığına sahip olan Windows kullanıcısını (ve parolasını) not edin. Azure Veritabanı Geçiş Hizmeti, geri yükleme işlemi için yedekleme dosyalarını Azure depolama kapsayıcısına yüklemek için kullanıcının kimlik bilgilerini kullanır.
* Bir blob kapsayıcı oluşturun ve bu makaledeki adımları kullanarak SAS URI'sini Al [yönetme Azure Blob depolama kaynaklarını depolama Gezgini'yle](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container). SAS URI'sini oluşturulurken İlkesi penceresinde tüm izinleri (okuma, yazma, silme, listeleme) seçtiğinizden emin olun.

   > [!NOTE]
   > Azure SQL veritabanı yönetilen örneği için SQL Server'dan geçişleri gerçekleştirmek için Azure veritabanı geçiş hizmeti kullanmak için gereken önkoşulları tam listesi için bkz [Azure SQL veritabanı yönetilen örneği SQL Server'ı geçirme ](https://aka.ms/migratetomiusingdms).

## <a name="next-steps"></a>Sonraki adımlar

Bölgesel kullanılabilirlik ve Azure veritabanı geçiş Hizmeti'nin genel bakış için bkz [Azure veritabanı geçiş hizmeti nedir](dms-overview.md).
