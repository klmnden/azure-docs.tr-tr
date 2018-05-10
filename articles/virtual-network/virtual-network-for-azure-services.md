---
title: Azure Hizmetleri için sanal ağ | Microsoft Docs
description: Bir sanal ağa dağıtma kaynakları avantajları hakkında bilgi edinin. Sanal ağlarda bulunan Kaynaklar birbirleri ile iletişim kurabilir ve kaynakları, Internet geçen trafik şirket.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: jdial
ms.openlocfilehash: ecfe3fb6db6b0fb0561e31b3c8aa70b74785b807
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="virtual-network-integration-for-azure-services"></a>Azure Hizmetleri için sanal ağ tümleştirme

Bir Azure sanal ağı Azure hizmetlerine tümleştirme sanal ağda dağıtılmış bir hizmet örneklerini özel erişim sağlar.

Aşağıdaki seçenekler ile sanal ağınız ile Azure Hizmetleri Tümleştirme:
- Doğrudan hizmetin adanmış örneğini bir sanal ağ uygulamasına dağıtma. Bu hizmetler ayrılmış örneklerinin özel olarak şirket içi ağlar ve sanal ağda erişilebilir.
- Hizmet, hizmet uç noktaları aracılığıyla bir sanal ağa genişleterek. Hizmet uç noktaları sanal ağa güvenli olması ayrı ayrı hizmet kaynakları izin verir.
 
## <a name="deploy-azure-services-into-virtual-networks"></a>Azure Hizmetleri sanal ağlar dağıtın

Genel IP adresleri üzerinden Internet üzerinden en Azure kaynakları ile iletişim kurabilir. Azure hizmetlerinde dağıttığınızda bir [sanal ağ](virtual-networks-overview.md), özel IP adresleri özel olarak, hizmet kaynaklarla kurabilir.

![Bir sanal ağda dağıtılmış hizmetleri](./media/virtual-network-for-azure-services/deploy-service-into-vnet.png)

Bir sanal ağ içindeki Hizmetler dağıtmak aşağıdaki yetenekleri sağlar:

- Sanal ağ içindeki kaynakların birbirleri ile özel olarak, özel IP adresleri ile iletişim kurabilir. Örneğin, doğrudan Hdınsight ve sanal ağda bir sanal makinede çalışan SQL Server arasında veri aktarma.
- Şirket içi kaynaklara üzerinden özel IP adresleri kullanarak sanal ağınızdaki kaynaklara erişmek bir [siteden siteye VPN (VPN ağ geçidi)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti) veya [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Sanal ağlar olabilir [eşlenen](virtual-network-peering-overview.md) birbirleri ile iletişim kurmak üzere sanal ağ kaynaklarında etkinleştirmek için özel IP adresleri.
- Bir sanal ağ hizmeti örneklerinin tam olarak örneklerinin durumunu izleyin ve yüküne göre gerekli ölçek sağlamak üzere Azure hizmeti tarafından yönetilir.
- Hizmet örneği, bir sanal ağ içinde ayrılmış bir alt ağ içinde dağıtılır. Gelen ve giden ağ erişimi açılır, aracılığıyla [ağ güvenlik grubu](security-overview.md#network-security-groups) Hizmetleri tarafından sağlanan rehberliğinde alt ağ.

### <a name="services-that-can-be-deployed-into-a-virtual-network"></a>Bir sanal ağ içinde dağıtılabilir Hizmetleri

Sanal ağa doğrudan dağıtılan her bir hizmet yönlendirmesi için belirli gereksinimleri ve içine ve dışına alt ağlar için izin verilmelidir trafik türlerini vardır. Daha fazla bilgi için bkz. 
 
- Sanal makineler: [Linux](../virtual-machines/linux/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Windows](../virtual-machines/windows/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Service fabric](../service-fabric/service-fabric-patterns-networking.md?toc=%2fazure%2fvirtual-network%2ftoc.json#existingvnet)
- [Sanal makine ölçekleme kümeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-mvss-existing-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [HDInsight](../hdinsight/hdinsight-extend-hadoop-virtual-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [App Service Ortamı](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [RedisCache](../redis-cache/cache-how-to-premium-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [API Management](../api-management/api-management-using-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Uygulama ağ geçidi (iç)](../application-gateway/application-gateway-ilb-arm.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure Kubernetes hizmeti (AKS)](../aks/networking-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure kapsayıcı Hizmeti altyapısının](https://github.com/Azure/acs-engine) Azure sanal ağ CNI ile [eklentisi](https://github.com/Azure/acs-engine/tree/master/examples/vnet)
- [Azure Active Directory etki alanı Hizmetleri](../active-directory-domain-services/active-directory-ds-getting-started-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json): sanal ağ (Klasik) yalnızca
- [Azure Batch](../batch/batch-api-basics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-network-vnet-and-firewall-configuration)
- [Bulut Hizmetleri](https://msdn.microsoft.com/library/azure/jj156091): sanal ağ (Klasik) yalnızca

Dağıtabilmeniz için bir [iç Azure yük dengeleyici](../load-balancer/load-balancer-internal-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) yüklemek için önceki listede bulunan kaynaklar çoğunu dengeleyin. Bazı durumlarda, hizmet otomatik olarak oluşturur ve bir kaynak oluşturduğunuzda, bir yük dengeleyici dağıtır.

## <a name="service-endpoints-for-azure-services"></a>Azure Hizmetleri için hizmet uç noktaları

Bazı Azure Hizmetleri sanal ağlarda dağıtılamıyor. Bir sanal ağ hizmeti uç noktası etkinleştirerek tercih ederseniz, bazı yalnızca belirli bir sanal ağ alt ağlara hizmet kaynaklara erişimi kısıtlayabilirsiniz. Daha fazla bilgi edinmek [sanal ağ hizmet uç noktaları](virtual-network-service-endpoints-overview.md).

Şu anda hizmet uç noktaları aşağıdaki hizmetleri için desteklenir: 
- **Azure depolama**: [güvenli hale getirme Azure depolama hesapları için sanal ağlar](../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- **Azure SQL veritabanı**: [sanal ağlar Azure SQL veritabanına güvenliğini sağlama](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

## <a name="virtual-network-integration-across-multiple-azure-services"></a>Sanal ağ tümleştirmesinin birden çok Azure hizmetleri arasında

Bir Azure hizmetini bir alt ağ bir sanal ağ ve bu alt ağ kaynaklarına güvenli kritik hizmet uygulamasına dağıtabilirsiniz. Örneğin, sanal ağınıza Hdınsight dağıtın ve bir depolama hesabı Hdınsight alt ağa güvenli.





