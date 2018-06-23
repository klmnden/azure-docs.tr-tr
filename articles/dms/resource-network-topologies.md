---
title: Ağ topolojileri Azure veritabanı geçiş hizmeti ile Azure SQL veritabanı örneği yönetilen geçişler için | Microsoft Docs
description: Veritabanı geçiş hizmeti için kaynak ve hedef yapılandırmaları hakkında bilgi edinin.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/21/2018
ms.openlocfilehash: 9fcee103854209016d73e29b598c9f33d35c4b6c
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36316876"
---
# <a name="network-topologies-for-azure-sql-db-managed-instance-migrations-using-the-azure-database-migration-service"></a>Azure veritabanı geçiş hizmeti ile Azure SQL DB yönetilen örneği geçişler için ağ topolojisi
Bu makalede Azure veritabanı geçiş hizmeti Azure SQL veritabanı yönetilen örneği için şirket içi SQL sunucularından kapsamlı geçiş deneyimi sağlamak için birlikte çalışabilir çeşitli ağ topolojileri açıklanır.

## <a name="azure-sql-database-managed-instance-configured-for-hybrid-workloads"></a>Azure SQL veritabanı yönetilen karma iş yükleri için yapılandırılmış örneği 
Azure SQL veritabanı yönetilen örneğiniz şirket içi ağınıza bağlanırsa bu topoloji kullanın. Bu yaklaşım ağ en Basitleştirilmiş yönlendirme sağlar ve geçiş sırasında maksimum veri işleme verir.

![Karma iş yükleri için ağ topolojisi](media\resource-network-topologies\hybrid-workloads.png)

**Gereksinimleri**
- Bu senaryoda, yönetilen Azure SQL veritabanı örneği ve Azure veritabanı geçiş hizmet örneği aynı Azure VNET içinde oluşturulur, ancak bunlar farklı alt ağları kullanın.  
- Bu senaryoda kullanılan VNET kullanarak şirket içi ağı'na da bağlı [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).

## <a name="azure-sql-database-managed-instance-isolated-from-the-on-premises-network"></a>Azure SQL veritabanı yönetilen şirket içi ağdan yalıtılmış örneği
Bir veya daha fazla aşağıdaki senaryolarda ortamınızı gerektiriyorsa, bu ağ topolojisi kullanın:
- Yönetilen Azure SQL veritabanı örneği şirket içi bağlantısı kesilmişse, ancak Azure veritabanı geçiş hizmeti örneğinizi şirket içi ağa bağlıdır.
- Rol tabanlı erişim denetimi (RBAC) ilkelerinin yerinde olduğundan ve Azure SQL veritabanı yönetilen örneğini barındıran aynı abonelik erişmek için kullanıcıları sınırlamak üzere ihtiyacınız varsa.
- Azure veritabanı geçiş hizmeti olan farklı Aboneliklerde ve sanal ağlar Azure SQL veritabanı yönetilen örneği için kullanılır.

![Yönetilen şirket içi ağdan yalıtılmış örneği için ağ topolojisi](media\resource-network-topologies\mi-isolated-workload.png)

**Gereksinimleri**
- Bu senaryo için Azure veritabanı geçiş hizmetini kullanan VNET ayrıca şirket içi ağ kullanarak bağlanmalıdır (https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Ayarlanan [VNET ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) yönetilen Azure SQL veritabanı örneği ve Azure veritabanı geçiş hizmeti için kullanılan VNET arasında.


## <a name="cloud-to-cloud-migrations-shared-vnet"></a>Bulut Bulut geçişler: VNET paylaşılan

Bu topoloji, SQL Server Kaynak bir Azure VM ile barındırılan ve yönetilen Azure SQL veritabanı örneği ve Azure veritabanı geçiş hizmeti ile aynı sanal ağ paylaşımları kullanın.

![Paylaşılan bir sanal ağ ile Bulut Bulut geçişler için ağ topolojisi](media\resource-network-topologies\cloud-to-cloud.png)

**Gereksinimleri**
- Ek gereksinim yok.

## <a name="cloud-to-cloud-migrations-isolated-vnet"></a>Bulut geçişler buluta: VNET yalıtılmış

Bir veya daha fazla aşağıdaki senaryolarda ortamınızı gerektiriyorsa, bu ağ topolojisi kullanın:
- Yönetilen Azure SQL veritabanı örneği yalıtılmış bir VNET içinde sağlanır.
- Rol tabanlı erişim denetimi (RBAC) ilkelerinin yerinde olduğundan ve Azure SQL veritabanı yönetilen örneğini barındıran aynı abonelik erişmek için kullanıcıları sınırlamak üzere ihtiyacınız varsa.
- Azure SQL veritabanı yönetilen örneği ve Azure veritabanı geçiş hizmeti için kullanılan sanal ağlar farklı Aboneliklerde bulunuyor.

![Yalıtılmış bir vnet ile Bulut Bulut geçişler için ağ topolojisi](media\resource-network-topologies\cloud-to-cloud-isolated.png)

**Gereksinimleri**
- Ayarlanan [VNET ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) yönetilen Azure SQL veritabanı örneği ve Azure veritabanı geçiş hizmeti için kullanılan VNET arasında.


## <a name="see-also"></a>Ayrıca Bkz.
- [Azure SQL veritabanı yönetilen örneğine SQL sunucusu geçirme](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)
- [Azure veritabanı geçiş hizmeti kullanma önkoşulları genel bakış](https://docs.microsoft.com/azure/dms/pre-reqs)
- [Azure portalını kullanarak bir sanal ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## <a name="next-steps"></a>Sonraki adımlar
Azure veritabanı geçiş hizmeti ve genel Önizleme sırasında bölgesel kullanılabilirlik genel bakış için bkz: [Azure veritabanı geçiş hizmeti Önizleme nedir](dms-overview.md). 