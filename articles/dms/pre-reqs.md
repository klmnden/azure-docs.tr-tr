---
title: Azure veritabanı geçiş hizmeti kullanma önkoşulları genel bakış | Microsoft Docs
description: Veritabanı geçişleri gerçekleştirmek üzere Azure veritabanı geçiş hizmeti kullanmak için önkoşulları genel bir bakış hakkında bilgi edinin.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 04/22/2018
ms.openlocfilehash: b3fe97343d844eb5c030cf0aaa9da2a6f54a2472
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32168216"
---
# <a name="overview-of-prerequisites-for-using-the-azure-database-migration-service"></a>Azure veritabanı geçiş hizmeti kullanma önkoşulları genel bakış
Azure veritabanı geçiş hizmeti düzgün veritabanı geçiş gerçekleştirirken çalıştığından emin olmak için gereken birkaç önkoşul vardır. Bazı Önkoşullar Önkoşullar belirli bir senaryoyla benzersiz durumdayken service tarafından desteklenen tüm senaryoları (kaynak hedef çiftleri) uygulamak.

Azure veritabanı geçiş hizmeti kullanımı ile ilişkili Önkoşullar aşağıdaki bölümlerde listelenmiştir.

## <a name="prerequisites-common-across-migration-scenarios"></a>Geçiş senaryoları arasında ortak önkoşulları
Tüm desteklenen geçiş senaryoları arasında ortak olan azure veritabanı geçiş hizmeti Önkoşullar gerek şunlardır:
- Kullanarak, şirket içi kaynak sunucular için siteden siteye bağlantı sağlar Azure Resource Manager dağıtım modelini kullanarak Azure veritabanı geçiş hizmeti için bir VNET oluşturma [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Azure sanal ağ (VNET) ağ güvenlik grubu kuralları blok aşağıdaki iletişim bağlantı noktaları 443, 53, 9354, 445, 12000. Azure VNET NSG trafik filtreleme daha ayrıntılı bilgi için bkz: [filtre ağ güvenlik grupları ile ağ trafiği](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Bir güvenlik duvarı gerecini kaynak veritabanları önünde kullanırken, geçiş için kaynak veritabanlarının erişmek Azure veritabanı geçiş hizmeti izin veren güvenlik duvarı kuralları eklemeniz gerekebilir.
- Yapılandırma, [veritabanı altyapısı erişimi için Windows Güvenlik Duvarı](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Varsayılan olarak SQL Server Express yüklemesi sırasında göre makalesindeki yönergeleri izleyerek devre dışıdır TCP/IP protokolünü etkinleştirin [etkinleştirmek veya devre dışı bir sunucu ağ protokolü](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).

## <a name="prerequisites-for-migrating-sql-server-to-azure-sql-database"></a>Azure SQL veritabanına geçirme SQL Server için Önkoşullar 
Tüm geçiş senaryoları için ortak olan Azure veritabanı geçiş hizmeti önkoşullara ek olarak, aynı zamanda özellikle bir senaryo veya başka bir uygulama önkoşulları vardır.

SQL Server, tüm geçiş senaryoları için ortak olan önkoşulların yanı sıra Azure SQL veritabanı geçiş işlemini gerçekleştirmek için Azure veritabanı geçiş hizmetini kullanırken aşağıdaki ek önkoşulları adres emin olun:

- C makaledeki ayrıntı izleyerek bunu Azure SQL veritabanı örneğinde bir örneğini oluşturmak[Azure portalında bir Azure SQL veritabanı oluştur](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal).
- İndirme ve yükleme [veri geçiş Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 veya sonraki bir sürümü.
- Windows, varsayılan olarak TCP bağlantı noktası 1433 olan SQL Server kaynağına erişmek Azure veritabanı geçiş Hizmeti Güvenlik Duvarı'nı açın.
- Dinamik bağlantı noktaları kullanan birden fazla adlandırılmış SQL Server örneklerini çalıştırıyorsanız, SQL Tarayıcı Hizmeti'ni etkinleştir ve böylece Azure veritabanı geçiş hizmeti kaynağınız adlandırılmış bir örnekte bağlanabilir, güvenlik duvarları üzerinden UDP bağlantı noktası 1434 erişmesine izin vermek isteyebilir Sunucu.
- Sunucu düzeyinde oluşturma [güvenlik duvarı kuralı](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) Azure SQL veritabanı sunucusunun hedef veritabanlarına Azure veritabanı geçiş hizmeti erişmesine izin vermek. Azure veritabanı geçiş hizmeti için kullanılan sanal ağ alt aralığını belirtin.
- Kaynak SQL Server örneğine bağlanmak için kullanılan kimlik bilgilerini sağlamak [denetim SUNUCUSUNA](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) izinleri.
- Hedef Azure SQL veritabanı örneğine bağlanmak için kullanılan kimlik bilgilerini hedef Azure SQL veritabanlarına CONTROL DATABASE izninizin olduğundan emin olun.

   > [!NOTE]
   > Öğretici, SQL Server'dan Azure SQL veritabanına geçişler gerçekleştirmeyi Azure veritabanı geçiş hizmeti kullanmak için gereken önkoşulları tam listesi için bkz [SQL Server'a geçirmek Azure SQL veritabanı](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql).
   > 

## <a name="prerequisites-for-migrating-sql-server-to-azure-sql-database-managed-instance"></a>Yönetilen Azure SQL veritabanı örneğine geçirme SQL Server için Önkoşullar
- Makaleyi ayrıntısı izleyerek yönetilen Azure SQL veritabanı örneğinin örnek oluşturmak [Azure portalında bir Azure SQL veritabanı örneği'ni yönetilen oluşturma](https://aka.ms/sqldbmi).
- Bağlantı noktası 445 Azure veritabanı geçiş hizmeti IP adresi veya alt ağ aralığı için SMB trafiğine izin vermek için güvenlik duvarlarınızdan açın.
- Windows, varsayılan olarak TCP bağlantı noktası 1433 olan SQL Server kaynağına erişmek Azure veritabanı geçiş Hizmeti Güvenlik Duvarı'nı açın.
- Dinamik bağlantı noktaları kullanan birden fazla adlandırılmış SQL Server örneklerini çalıştırıyorsanız, SQL Tarayıcı Hizmeti'ni etkinleştir ve böylece Azure veritabanı geçiş hizmeti kaynağınız adlandırılmış bir örnekte bağlanabilir, güvenlik duvarları üzerinden UDP bağlantı noktası 1434 erişmesine izin vermek isteyebilir Sunucu.
- Yönetilen örneğini hedeflemek ve SQL Server Kaynak bağlanmak için kullanılan oturum açma bilgileri sysadmin sunucu rolünün üyesi olduğundan emin olun.
- Azure veritabanı geçiş hizmeti kaynak veritabanını yedeklemek için kullanabileceğiniz bir ağ paylaşımı oluşturun.
- Kaynak çalıştıran hizmet hesabını ayrıcalıkları SQL Server örneğinin oluşturduğunuz ağ paylaşımı üzerinde yazma ve bilgisayar hesabı kaynak sunucu için aynı paylaşımına okuma/yazma erişimi olduğundan emin olun.
- Daha önce oluşturulmuş ağ paylaşımında tam denetim ayrıcalığına sahip bir Windows kullanıcısı (ve parola) not edin. Azure veritabanı geçiş hizmeti, yedekleme dosyalarını geri yükleme işlemi Azure depolama kapsayıcısının karşıya yüklemek için kullanıcı kimlik bilgilerini temsil eder.
- Bir blob kapsayıcı oluşturun ve makaledeki adımları kullanarak SAS URI'sini Al [Depolama Gezgini ile Azure Blob Storage'ı yönetme kaynaklarını](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container). SAS URI'sini oluşturulurken tüm izinlerine (okuma, yazma, silme, listeleme) İlkesi penceresi seçtiğinizden emin olun.

   > [!NOTE]
   > Öğretici, SQL Server'dan Azure SQL veritabanı yönetilen örneğine geçişler gerçekleştirmeyi Azure veritabanı geçiş hizmeti kullanmak için gereken önkoşulları tam listesi için bkz [SQL Server'a geçirmek yönetilen Azure SQL veritabanı örneği ](https://aka.ms/migratetomiusingdms).

## <a name="next-steps"></a>Sonraki adımlar
Azure veritabanı geçiş hizmeti ve genel Önizleme sırasında bölgesel kullanılabilirlik genel bakış için bkz: [Azure veritabanı geçiş hizmeti Önizleme nedir](dms-overview.md). 