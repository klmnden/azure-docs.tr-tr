---
title: "Ağ topolojileri Azure veritabanı geçiş hizmeti ile Azure SQL DB yönetilen örneği geçişler için | Microsoft Docs"
description: "Veritabanı geçiş hizmeti için kaynak ve hedef yapılandırmaları hakkında bilgi edinin."
services: database-migration
author: HJToland3
ms.author: jtoland
manager: 
ms.reviewer: 
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 03/06/2018
ms.openlocfilehash: 892cff02b5b70f09236bb37ae786f180ddca9316
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="network-topologies-for-azure-sql-db-managed-instance-migrations-using-the-azure-database-migration-service"></a>Azure veritabanı geçiş hizmeti ile Azure SQL DB yönetilen örneği geçişler için ağ topolojisi
Bu makalede, Azure veritabanı geçiş hizmeti ile şirket içi SQL sunucularından Azure SQL veritabanı yönetilen örneğine sorunsuz geçiş deneyimi sağlamak için çalışabilmesi için çeşitli ağ topolojileri hakkında bilgi edineceksiniz.

## <a name="azure-sql-database-managed-instance-configured-for-hybrid-workloads"></a>Azure SQL veritabanı yönetilen karma iş yükleri için yapılandırılmış örneği 
Azure SQL veritabanı yönetilen örneğiniz şirket içi ağınıza bağlanırsa bu topoloji kullanın. Bu yaklaşım ağ en Basitleştirilmiş yönlendirme sağlar ve geçiş sırasında maksimum veri işleme verir.

![Karma iş yükleri için ağ topolojisi](media\resource-network-topologies\hybrid-workloads.png)

**Gereksinimleri**
- Bu senaryoda, yönetilen Azure SQL veritabanı örneği ve Azure veritabanı geçiş hizmet örneği aynı Azure VNET içinde oluşturulur, ancak bunlar farklı alt ağları kullanın.  
- Bu senaryoda kullanılan VNET, ExpressRoute ya da VPN kullanarak şirket içi ağ de bağlıdır.

## <a name="azure-sql-database-managed-instance-isolated-from-the-on-premises-network"></a>Azure SQL veritabanı yönetilen şirket içi ağdan yalıtılmış örneği
Bir veya daha fazla aşağıdaki senaryolarda ortamınızı gerektiriyorsa, bu ağ topolojisi kullanın:
- Yönetilen Azure SQL veritabanı örneği şirket içi bağlantısı kesilmişse, ancak Azure veritabanı geçiş hizmeti örneğinizi şirket içi ağa bağlıdır.
- Rol tabanlı erişim denetimi (RBAC) ilkelerinin yerinde olduğundan ve Azure SQL veritabanı yönetilen örneğini barındıran aynı abonelik kullanıcı erişimi sınırlayabilirsiniz.
- Azure SQL veritabanı yönetilen örneği ve Azure veritabanı geçiş hizmeti için kullanılan sanal ağlar farklı Aboneliklerde bulunuyor.

![Yönetilen şirket içi ağdan yalıtılmış örneği için ağ topolojisi](media\resource-network-topologies\mi-isolated-workload.png)

**Gereksinimleri**
- Bu senaryo için Azure veritabanı geçiş hizmetini kullanan sanal ağ, ExpressRoute ya da VPN kullanarak şirket ağına ayrıca bağlanmalıdır.
- Azure SQL veritabanı yönetilen örneği ve Azure veritabanı geçiş hizmeti için kullanılan VNET arasında eşleme bir sanal ağ oluşturun.


## <a name="cloud-to-cloud-migrations"></a>Bulut geçişler bulut
Bu topoloji, SQL Server Kaynak bir Azure sanal makinesi barındırılıyorsa kullanın.

![Bulut Bulut geçişler için ağ topolojisi](media\resource-network-topologies\cloud-to-cloud.png)

**Gereksinimleri**
- Azure SQL veritabanı yönetilen örneği ve Azure veritabanı geçiş hizmeti için kullanılan VNET arasında eşleme bir sanal ağ oluşturun.

## <a name="see-also"></a>Ayrıca Bkz.
- [Azure SQL veritabanı yönetilen örneğine SQL sunucusu geçirme](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)
- [Azure veritabanı geçiş hizmeti kullanma önkoşulları genel bakış](https://docs.microsoft.com/azure/dms/pre-reqs)
- [Azure portalını kullanarak bir sanal ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## <a name="next-steps"></a>Sonraki adımlar
Azure veritabanı geçiş hizmeti ve genel Önizleme sırasında bölgesel kullanılabilirlik genel bakış için bkz: [Azure veritabanı geçiş hizmeti Önizleme nedir](dms-overview.md). 