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
ms.date: 03/12/2019
ms.openlocfilehash: 6e26ae7dd8637a71b9f4e2f6ea1ed6063cf0ec40
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60584225"
---
# <a name="overview-of-prerequisites-for-using-the-azure-database-migration-service"></a>Azure veritabanı geçiş hizmeti kullanma önkoşulları genel bakış
Azure veritabanı geçiş Hizmeti'nin veritabanı geçişlerini gerçekleştirirken sorunsuz çalıştığından emin olmak için gereken birkaç önkoşul vardır. Bazı Önkoşullar, diğer ön koşulları için belirli bir senaryoya benzersiz çalışırken service tarafından desteklenen tüm senaryolarda (kaynak-hedef çiftlerinin) arasında geçerlidir.

Azure veritabanı geçiş hizmeti kullanımıyla ilişkili Önkoşullar aşağıdaki bölümlerde listelenmiştir.

## <a name="prerequisites-common-across-migration-scenarios"></a>Geçiş senaryoları arasında ortak Önkoşullar
Tüm desteklenen geçiş senaryoları arasında ortak olan azure veritabanı geçiş hizmeti Önkoşullar dahil etme gereksinimi:
- Azure Resource Manager dağıtım modelini kullanarak Azure Veritabanı Geçiş Hizmeti için [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) kullanarak şirket içi kaynak sunucularınıza siteden siteye bağlantı sağlayan bir sanal ağ oluşturun.
- Azure Sanal Ağı (VNET) Ağ Güvenlik Grubu kurallarının 443, 53, 9354, 445, 12000 numaralı iletişim bağlantı noktalarını engellemediğinden emin olun. Azure VNET NSG trafiğini filtreleme hakkında ayrıntılı bilgi için [Ağ güvenlik grupları ile ağ trafiğini filtreleme](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) makalesine bakın.
- Kaynak veritabanlarınızın önünde bir güvenlik duvarı cihazı kullanıyorsanız, Azure Veritabanı Geçiş Hizmeti'nin geçiş amacıyla kaynak veritabanlarına erişmesi için güvenlik duvarı kuralları eklemeniz gerekebilir.
- [Windows Güvenlik Duvarınızı veritabanı altyapısı erişimi](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) için yapılandırın.
- [Sunucu Ağ Protokolünü Etkinleştirme veya Devre Dışı Bırakma](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure) makalesindeki yönergeleri izleyerek SQL Server Express yüklemesi sırasında varsayılan olarak devre dışı bırakılan TCP/IP protokolünü etkinleştirin.

## <a name="prerequisites-for-migrating-sql-server-to-azure-sql-database"></a>Azure SQL veritabanına geçirme SQL Server için Önkoşullar 
Tüm geçiş senaryoları için ortak olan Azure veritabanı geçiş hizmeti önkoşullarının yanı sıra da özellikle bir senaryo veya başka bir geçerli önkoşulları vardır.

SQL Server önkoşulları tüm geçiş senaryoları için ortak olan ek olarak, Azure SQL veritabanı geçişleri gerçekleştirmek için Azure veritabanı geçiş hizmeti kullanırken aşağıdaki ek önkoşulları adres emin olun:

- Makalesinde ayrıntılı olarak C izleyerek bunu Azure SQL veritabanı örneğine bir örneğini oluşturmak[Azure portalında bir Azure SQL veritabanı oluştur](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal).
- [Data Migration Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595) 3.3 veya üzeri sürümünü indirip yükleyin.
- Azure Veritabanı Geçiş Hizmeti'ne kaynak SQL Server erişimi sağlamak için Windows güvenlik duvarınızı açın. Varsayılan ayarlarda 1433 numaralı TCP bağlantı noktası kullanılır.
- Dinamik bağlantı noktası kullanarak birden fazla adlandırılmış SQL Server örneği çalıştırıyorsanız, Azure Veritabanı Geçiş Hizmeti'nin kaynak sunucunuzdaki adlandırılmış örneğe bağlanabilmesi için SQL Browser Hizmeti'ni etkinleştirebilir ve güvenlik duvarınızda 1434 numaralı UDP bağlantı noktasına erişim izni verebilirsiniz.
- Azure Veritabanı Geçiş Hizmeti'nin hedef veritabanlarına erişmesini sağlama amacıyla Azure SQL Veritabanı için sunucu düzeyinde [güvenlik duvarı kuralı](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) oluşturun. Azure Veritabanı Geçiş Hizmeti için kullanılan sanal ağın alt ağ aralığını belirtin.
- SQL Server örneğine bağlanmak için kullanılan kimlik bilgilerinin [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) izinlerine sahip olduğundan emin olun.
- Hedef Azure SQL Veritabanı örneğine bağlanmak için kullanılan kimlik bilgilerinin hedef Azure SQL veritabanlarında CONTROL DATABASE iznine sahip olduğundan emin olun.

   > [!NOTE]
   > Azure veritabanı geçiş hizmeti, SQL Server'dan Azure SQL veritabanı'na geçişler gerçekleştirmeyi kullanmak için gereken önkoşulları tam listesi için bkz [Azure SQL veritabanı için SQL Server'ı geçirme](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql).
   > 

## <a name="prerequisites-for-migrating-sql-server-to-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneğine geçirme SQL Server için Önkoşullar
- Makalesinde ayrıntılı olarak Azure SQL veritabanı yönetilen örneği'nın bir örneğini oluşturma [Azure portalında Azure SQL veritabanı yönetilen örneği oluşturma](https://aka.ms/sqldbmi).
- SMB trafiği için Azure veritabanı geçiş hizmeti IP adresi veya alt ağ aralığı 445 numaralı bağlantı noktasında izin vermek için güvenlik duvarlarınızdan açın.
- Azure Veritabanı Geçiş Hizmeti'ne kaynak SQL Server erişimi sağlamak için Windows güvenlik duvarınızı açın. Varsayılan ayarlarda 1433 numaralı TCP bağlantı noktası kullanılır.
- Dinamik bağlantı noktası kullanarak birden fazla adlandırılmış SQL Server örneği çalıştırıyorsanız, Azure Veritabanı Geçiş Hizmeti'nin kaynak sunucunuzdaki adlandırılmış örneğe bağlanabilmesi için SQL Browser Hizmeti'ni etkinleştirebilir ve güvenlik duvarınızda 1434 numaralı UDP bağlantı noktasına erişim izni verebilirsiniz.
- Kaynak SQL Server ve hedef Yönetilen Örnek bağlantısı kurmak için kullanılan oturum açma bilgilerinin sysadmin sunucu rolüne üye olduğundan emin olun.
- Azure Veritabanı Geçiş Hizmeti'nin kaynak veritabanını yedeklemek için kullanabileceği bir ağ paylaşımı oluşturun.
- Kaynak SQL Server örneğini çalıştıran hizmet hesabının oluşturduğunuz ağ paylaşımında yazma ayrıcalıklarına sahip olduğundan ve kaynak sunucunun bilgisayar hesabının aynı paylaşımda okuma/yazma erişimine sahip olduğundan emin olun.
- Önceden oluşturduğunuz ağ paylaşımında tam denetim ayrıcalığına sahip olan Windows kullanıcısını (ve parolasını) not edin. Azure Veritabanı Geçiş Hizmeti, geri yükleme işlemi için yedekleme dosyalarını Azure depolama kapsayıcısına yüklemek için kullanıcının kimlik bilgilerini kullanır.
- Bir blob kapsayıcı oluşturun ve bu makaledeki adımları kullanarak SAS URI'sini Al [yönetme Azure Blob depolama kaynaklarını depolama Gezgini'yle](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container). SAS URI'sini oluşturulurken İlkesi penceresinde tüm izinleri (okuma, yazma, silme, listeleme) seçtiğinizden emin olun.

   > [!NOTE]
   > Azure SQL veritabanı yönetilen örneği için SQL Server'dan geçişleri gerçekleştirmek için Azure veritabanı geçiş hizmeti kullanmak için gereken önkoşulları tam listesi için bkz [Azure SQL veritabanı yönetilen örneği SQL Server'ı geçirme ](https://aka.ms/migratetomiusingdms).

## <a name="next-steps"></a>Sonraki adımlar
Bölgesel kullanılabilirlik ve Azure veritabanı geçiş Hizmeti'nin genel bakış için bkz [Azure veritabanı geçiş hizmeti nedir](dms-overview.md). 