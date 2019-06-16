---
title: Ağ Topolojileri için Azure veritabanı geçiş hizmetini kullanarak Azure SQL veritabanı yönetilen örneği geçişleri | Microsoft Docs
description: Azure veritabanı geçiş hizmeti için kaynak ve hedef yapılandırmalar hakkında bilgi edinin.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/07/2019
ms.openlocfilehash: 74613599903f7cde606295a1e2d9eaaa0924cf50
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66808412"
---
# <a name="network-topologies-for-azure-sql-db-managed-instance-migrations-using-azure-database-migration-service"></a>İçin Azure veritabanı geçiş hizmetini kullanarak Azure SQL DB yönetilen örneği geçişlerinin ağ topolojileri

Bu makalede, kapsamlı bir geçiş deneyimi için Azure SQL veritabanı yönetilen örneği şirket içi SQL Server'lardaki sağlamak için Azure veritabanı geçiş hizmeti çalışabilirsiniz çeşitli ağ topolojileri açıklanır.

## <a name="azure-sql-database-managed-instance-configured-for-hybrid-workloads"></a>Azure SQL veritabanı yönetilen hibrit iş yükleri için yapılandırılmış örneği 

Azure SQL veritabanı yönetilen örneği, şirket içi ağınıza bağlıysa bu topolojiyi kullanın. Bu yaklaşım, en basit ağ yönlendirme sağlar ve geçiş sırasında en yüksek veri performansı verir.

![Hibrit iş yükleri için ağ topolojisi](media/resource-network-topologies/hybrid-workloads.png)

**Gereksinimler**

- Bu senaryoda, Azure SQL veritabanı yönetilen örneği ve Azure veritabanı geçiş hizmeti örneği aynı Azure sanal ağınızda oluşturulur, ancak bunlar farklı alt ağları kullanın.  
- Bu senaryoda kullanılan VNet kullanarak şirket içi ağa ayrıca bağlı [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).

## <a name="azure-sql-database-managed-instance-isolated-from-the-on-premises-network"></a>Azure SQL veritabanı yönetilen şirket içi ağdan yalıtılmış bir örneği

Bir veya daha fazla aşağıdaki senaryolarda ortamınızın gerektiriyorsa, bu ağ topolojisi kullanın:

- Azure SQL veritabanı yönetilen örneği, şirket içi bağlantısı kesilmişse, ancak Azure veritabanı geçiş hizmeti örneğiniz şirket içi ağa bağlıdır.
- Rol tabanlı erişim denetimi (RBAC) ilkelerinin yerinde olduğundan ve Azure SQL veritabanı yönetilen örneği barındıran aynı aboneliğe erişmek için kullanıcıları sınırlamak gerekiyorsa.
- Azure veritabanı geçiş hizmeti ve Azure SQL veritabanı yönetilen örneği için kullanılan Vnet'ler farklı Aboneliklerde bulunuyor.

![Yönetilen şirket içi ağdan yalıtılmış bir örneği için ağ topolojisi](media/resource-network-topologies/mi-isolated-workload.png)

**Gereksinimler**

- Bu senaryo için Azure veritabanı geçiş hizmeti kullanan sanal ağın da şirket içi ağa kullanarak bağlanmalıdır (https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Ayarlanan [VNet ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) Azure SQL veritabanı için kullanılan sanal ağ arasında örnek ve Azure veritabanı geçiş hizmeti yönetilen.

## <a name="cloud-to-cloud-migrations-shared-vnet"></a>Bulut buluta geçiş: sanal ağ paylaşılan

Kaynak SQL Server bir Azure sanal Makinesinde barındırılan ve Azure SQL veritabanı yönetilen örneği ve Azure veritabanı geçiş hizmeti ile aynı sanal ağ paylaşımları varsa bu topolojiyi kullanın.

![Paylaşılan bir sanal ağ ile bulut buluta geçiş için ağ topolojisi](media/resource-network-topologies/cloud-to-cloud.png)

**Gereksinimler**

- Ek gereksinimi yoktur.

## <a name="cloud-to-cloud-migrations-isolated-vnet"></a>Buluta geçiş için bulut: sanal ağ yalıtılmış

Bir veya daha fazla aşağıdaki senaryolarda ortamınızın gerektiriyorsa, bu ağ topolojisi kullanın:

- Azure SQL veritabanı yönetilen örneği, yalıtılmış bir Vnet'te sağlanır.
- Rol tabanlı erişim denetimi (RBAC) ilkelerinin yerinde olduğundan ve Azure SQL veritabanı yönetilen örneği barındıran aynı aboneliğe erişmek için kullanıcıları sınırlamak gerekiyorsa.
- Azure SQL veritabanı yönetilen örneği ve Azure veritabanı geçiş hizmeti için kullanılan Vnet'ler farklı Aboneliklerde bulunuyor.

![Yalıtılmış bir sanal ağ ile bulut buluta geçiş için ağ topolojisi](media/resource-network-topologies/cloud-to-cloud-isolated.png)

**Gereksinimler**

- Ayarlanan [VNet ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) Azure SQL veritabanı için kullanılan sanal ağ arasında örnek ve Azure veritabanı geçiş hizmeti yönetilen.

## <a name="inbound-security-rules"></a>Gelen güvenlik kuralları

| **ADI**   | **BAĞLANTI NOKTASI** | **PROTOKOLÜ** | **KAYNAK** | **HEDEF** | **EYLEM** |
|------------|----------|--------------|------------|-----------------|------------|
| DMS_subnet | Tüm      | Tüm          | DMS ALT AĞ | Tüm             | İzin Ver      |

## <a name="outbound-security-rules"></a>Giden güvenlik kuralları

| **ADI**                  | **BAĞLANTI NOKTASI**                                              | **PROTOKOLÜ** | **KAYNAK** | **HEDEF**           | **EYLEM** | **Kural nedeni**                                                                                                                                                                              |
|---------------------------|-------------------------------------------------------|--------------|------------|---------------------------|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| yönetim                | 443,9354                                              | TCP          | Tüm        | Tüm                       | İzin Ver      | Service bus ve Azure blob depolama ile yönetim düzlemi iletişim. <br/>(Microsoft eşdüzey hizmet sağlama etkinse, bu kural gerekmeyebilir.)                                                             |
| Tanılama               | 12000                                                 | TCP          | Tüm        | Tüm                       | İzin Ver      | DMS, sorun giderme amacıyla tanılama bilgilerini toplamak için bu kural kullanır.                                                                                                                      |
| SQL kaynak sunucusu         | 1433 (veya SQL Server dinlediği TCP/IP bağlantı noktası) | TCP          | Tüm        | Şirket içi adres alanı | İzin Ver      | DMS SQL Server Kaynak bağlantısı <br/>(Siteden siteye bağlantınız varsa, bu kural gerekmeyebilir.)                                                                                       |
| SQL Server'ın adlandırılmış örneği | 1434                                                  | UDP          | Tüm        | Şirket içi adres alanı | İzin Ver      | SQL Server adlandırılmış örneği DMS kaynak bağlantısı <br/>(Siteden siteye bağlantınız varsa, bu kural gerekmeyebilir.)                                                                        |
| SMB paylaşımı                 | 445                                                   | TCP          | Tüm        | Şirket içi adres alanı | İzin Ver      | SMB ağ paylaşımı için Azure sanal makinesinde Azure SQL veritabanı ve SQL Server'lar geçişler için veritabanı yedekleme dosyaları depolamak DMS <br/>(Siteden siteye bağlantı varsa, bu kural gerekebilir değil). |
| DMS_subnet                | Tüm                                                   | Tüm          | Tüm        | DMS_Subnet                | İzin Ver      |                                                                                                                                                                                                  |

## <a name="see-also"></a>Ayrıca bkz.

- [SQL Server'ı Azure SQL veritabanı yönetilen örneğine geçirme](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)
- [Azure veritabanı geçiş hizmeti kullanma önkoşulları genel bakış](https://docs.microsoft.com/azure/dms/pre-reqs)
- [Azure portalını kullanarak bir sanal ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## <a name="next-steps"></a>Sonraki adımlar

- Azure veritabanı geçiş Hizmeti'nin genel bakış için bkz [Azure veritabanı geçiş hizmeti nedir?](dms-overview.md).
- Azure veritabanı geçiş hizmeti bölgesel kullanılabilirliği hakkında güncel bilgiler için bkz [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=database-migration) sayfası.
