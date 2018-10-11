---
title: Ağ Topolojileri için Azure veritabanı geçiş hizmetini kullanarak Azure SQL veritabanı yönetilen örneği geçişleri | Microsoft Docs
description: Veritabanı geçiş hizmeti için kaynak ve hedef yapılandırmalar hakkında bilgi edinin.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 10/10/2018
ms.openlocfilehash: 39bcea36f3599530413aa9fc4dbb308ee2fb1681
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49066862"
---
# <a name="network-topologies-for-azure-sql-db-managed-instance-migrations-using-the-azure-database-migration-service"></a>İçin Azure veritabanı geçiş hizmetini kullanarak Azure SQL DB yönetilen örneği geçişlerinin ağ topolojileri
Bu makalede, kapsamlı bir geçiş deneyimi için Azure SQL veritabanı yönetilen örneği şirket içi SQL Server'lardaki sağlamak için Azure veritabanı geçiş hizmeti çalışabilirsiniz çeşitli ağ topolojileri açıklanır.

## <a name="azure-sql-database-managed-instance-configured-for-hybrid-workloads"></a>Azure SQL veritabanı yönetilen hibrit iş yükleri için yapılandırılmış örneği 
Azure SQL veritabanı yönetilen örneği, şirket içi ağınıza bağlıysa bu topolojiyi kullanın. Bu yaklaşım, en basit ağ yönlendirme sağlar ve geçiş sırasında en yüksek veri performansı verir.

![Hibrit iş yükleri için ağ topolojisi](media\resource-network-topologies\hybrid-workloads.png)

**Gereksinimleri**
- Bu senaryoda, Azure SQL veritabanı yönetilen örneği ve Azure veritabanı geçiş hizmeti örneği aynı Azure sanal ağınızda oluşturulur, ancak bunlar farklı alt ağları kullanın.  
- Bu senaryoda kullanılan VNET kullanarak şirket içi ağa ayrıca bağlı [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).

## <a name="azure-sql-database-managed-instance-isolated-from-the-on-premises-network"></a>Azure SQL veritabanı yönetilen şirket içi ağdan yalıtılmış bir örneği
Bir veya daha fazla aşağıdaki senaryolarda ortamınızın gerektiriyorsa, bu ağ topolojisi kullanın:
- Azure SQL veritabanı yönetilen örneği, şirket içi bağlantısı kesilmişse, ancak Azure veritabanı geçiş hizmeti örneğiniz şirket içi ağa bağlıdır.
- Rol tabanlı erişim denetimi (RBAC) ilkelerinin yerinde olduğundan ve Azure SQL veritabanı yönetilen örneği barındıran aynı aboneliğe erişmek için kullanıcıları sınırlamak gerekiyorsa.
- Azure SQL veritabanı yönetilen örneği için sanal ağlar ve Azure veritabanı geçiş hizmeti, farklı Aboneliklerde kullanıldığını.

![Yönetilen şirket içi ağdan yalıtılmış bir örneği için ağ topolojisi](media\resource-network-topologies\mi-isolated-workload.png)

**Gereksinimleri**
- Bu senaryo için Azure veritabanı geçiş hizmeti kullanan sanal ağın da şirket içi ağa kullanarak bağlanmalıdır (https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Ayarlanan [VNET ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) Azure SQL veritabanı yönetilen örneği ve Azure veritabanı geçiş hizmeti için kullanılan sanal ağ arasında.


## <a name="cloud-to-cloud-migrations-shared-vnet"></a>Bulut buluta geçiş: sanal ağ paylaşılan

Kaynak SQL Server bir Azure sanal Makinesinde barındırılan ve Azure SQL veritabanı yönetilen örneği ve Azure veritabanı geçiş hizmeti ile aynı sanal ağ paylaşımları varsa bu topolojiyi kullanın.

![Paylaşılan bir sanal ağ ile bulut buluta geçiş ağ topolojisi](media\resource-network-topologies\cloud-to-cloud.png)

**Gereksinimleri**
- Ek gereksinimi yoktur.

## <a name="cloud-to-cloud-migrations-isolated-vnet"></a>Buluta geçiş için bulut: sanal ağ yalıtılmış

Bir veya daha fazla aşağıdaki senaryolarda ortamınızın gerektiriyorsa, bu ağ topolojisi kullanın:
- Azure SQL veritabanı yönetilen örneği, yalıtılmış bir VNET'te sağlanır.
- Rol tabanlı erişim denetimi (RBAC) ilkelerinin yerinde olduğundan ve Azure SQL veritabanı yönetilen örneği barındıran aynı aboneliğe erişmek için kullanıcıları sınırlamak gerekiyorsa.
- Azure SQL veritabanı yönetilen örneği ve Azure veritabanı geçiş hizmeti için kullanılan Vnet'ler farklı Aboneliklerde bulunuyor.

![Ağ topolojisi yalıtılmış bir sanal ağ ile bulut buluta geçiş](media\resource-network-topologies\cloud-to-cloud-isolated.png)

**Gereksinimleri**
- Ayarlanan [VNET ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) Azure SQL veritabanı yönetilen örneği ve Azure veritabanı geçiş hizmeti için kullanılan sanal ağ arasında.


## <a name="see-also"></a>Ayrıca Bkz.
- [SQL Server'ı Azure SQL veritabanı yönetilen örneğine geçirme](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)
- [Azure veritabanı geçiş hizmeti kullanma önkoşulları genel bakış](https://docs.microsoft.com/azure/dms/pre-reqs)
- [Azure portalını kullanarak bir sanal ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## <a name="next-steps"></a>Sonraki adımlar
Bölgesel kullanılabilirlik genel Önizleme süresince ve Azure veritabanı geçiş Hizmeti'nin genel bakış için bkz [Azure veritabanı geçiş hizmeti önizlemesi nedir](dms-overview.md). 