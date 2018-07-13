---
title: Azure Hizmetleri için sanal ağ | Microsoft Docs
description: Bir sanal ağa dağıtma kaynakları avantajları hakkında bilgi edinin. Sanal ağlardaki kaynaklar birbiriyle iletişim kurabilir ve kaynaklar olmadan, Internet'e geçiş trafiği şirket.
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
ms.openlocfilehash: 3e31dbce7bd24b3c3bb0f24561464e6303f3908e
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38990605"
---
# <a name="virtual-network-integration-for-azure-services"></a>Azure Hizmetleri için sanal ağ tümleştirmesi

Azure Hizmetleri için bir Azure sanal ağ tümleştirme sanal ağ içinde dağıtılan bir hizmetin örneklerine özel erişim sağlar.

Aşağıdaki seçeneklerle sanal ağınız ile Azure hizmetleriyle tümleştirebilirsiniz:
- Doğrudan bir sanal ağa hizmetinin ayrılmış örnekleri dağıtılıyor. Şirket içi ağlara gelen ve sanal ağ içinde ayrılmış örnekleri bu hizmetlerin özel olarak erişilebilir.
- Bir sanal ağ hizmet uç noktaları aracılığıyla hizmetine genişleterek. Hizmet uç noktaları sanal ağa güvenli olması için bireysel hizmet kaynakları sağlar.
 
## <a name="deploy-azure-services-into-virtual-networks"></a>Azure hizmetlerini sanal ağlara dağıtma

Genel IP adresleri üzerinden Internet üzerinden en Azure kaynaklarıyla iletişim kurabilir. Azure hizmetlerinde dağıtırken bir [sanal ağ](virtual-networks-overview.md), hizmet kaynakları ile özel olarak, özel IP adresleri aracılığıyla iletişim kurabilir.

![Bir sanal ağda dağıtılan hizmetler](./media/virtual-network-for-azure-services/deploy-service-into-vnet.png)

Bir sanal ağ içindeki Hizmetleri dağıtımı aşağıdaki özellikleri sağlar:

- Sanal ağ içindeki kaynakların birbiriyle özel olarak, özel IP adresleri aracılığıyla iletişim kurabilir. Örneğin, doğrudan sanal ağdaki bir sanal makinede çalışan SQL Server ile HDInsight arasındaki veri aktarımı.
- Şirket kaynakları, özel IP adresleri üzerinden bir sanal ağdaki kaynaklara erişebilir bir [siteden siteye VPN (VPN ağ geçidi)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti) veya [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Sanal ağların [eşlenmiş](virtual-network-peering-overview.md) birbirleri ile iletişim kurmak için sanal ağlarda bulunan kaynaklar etkinleştirmek için özel IP adresleri kullanarak.
- Bir sanal ağ hizmet örnekleri örneklerinin sistem durumunu izleme ve yüküne göre gerekli ölçek sağlamak için Azure hizmeti tarafından tamamen yönetilir.
- Hizmet örnekleri, bir sanal ağ içinde ayrılmış bir alt ağa dağıtılır. Gelen ve giden ağ erişimi açılır, aracılığıyla [ağ güvenlik grupları](security-overview.md#network-security-groups) alt ağına göre hizmetler tarafından sağlanan yönergeler için.

### <a name="services-that-can-be-deployed-into-a-virtual-network"></a>Bir sanal ağa dağıtılan hizmetler

Doğrudan sanal ağa dağıtılan her bir hizmet yönlendirmesi için belirli gereksinimler ve içine ve dışına alt ağlar izin verilmelidir trafik türlerini sahiptir. Daha fazla bilgi için bkz. 
 
- Sanal makineler: [Linux](../virtual-machines/linux/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Windows](../virtual-machines/windows/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Service fabric](../service-fabric/service-fabric-patterns-networking.md?toc=%2fazure%2fvirtual-network%2ftoc.json#existingvnet)
- [Sanal makine ölçek kümeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-mvss-existing-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [HDInsight](../hdinsight/hdinsight-extend-hadoop-virtual-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [App Service Ortamı](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [RedisCache](../redis-cache/cache-how-to-premium-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [API Management](../api-management/api-management-using-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Uygulama ağ geçidi (iç)](../application-gateway/application-gateway-ilb-arm.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure Kubernetes Service'i (AKS)](../aks/networking-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure Container Service altyapısı](https://github.com/Azure/acs-engine) ile Azure sanal ağ CNI [eklentisi](https://github.com/Azure/acs-engine/tree/master/examples/vnet)
- [Azure Active Directory etki alanı Hizmetleri](../active-directory-domain-services/active-directory-ds-getting-started-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure Batch](../batch/batch-api-basics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-network-vnet-and-firewall-configuration)
- [Bulut Hizmetleri](https://msdn.microsoft.com/library/azure/jj156091): sanal ağ (Klasik) yalnızca

Dağıtabileceğiniz bir [iç Azure yük dengeleyici](../load-balancer/load-balancer-internal-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) yüklemek için önceki listede kaynak Dengeleme. Bazı durumlarda, hizmet otomatik olarak oluşturur ve bir kaynak oluştururken, bir yük dengeleyicinin dağıtır.

## <a name="service-endpoints-for-azure-services"></a>Azure Hizmetleri için hizmet uç noktası

Bazı Azure Hizmetleri, sanal ağları dağıtılamıyor. Bir sanal ağ hizmet uç noktası etkinleştirerek seçerseniz, bazı hizmet kaynaklarının yalnızca belirli bir sanal ağ alt ağları için erişimi kısıtlayabilirsiniz. Daha fazla bilgi edinin [sanal ağ hizmet uç noktaları](virtual-network-service-endpoints-overview.md)ve uç noktaları için etkin hale getirilebilir Hizmetleri.

## <a name="virtual-network-integration-across-multiple-azure-services"></a>Birden çok Azure hizmetleri arasında sanal ağ tümleştirmesi

Bir Azure hizmeti, bir sanal ağ ve alt ağın güvenli kritik hizmet kaynaklarını bir alt ağa dağıtabilirsiniz. Örneğin, HDInsight'ı, sanal ağa dağıtma ve güvenli HDInsight alt ağ için bir depolama hesabı.





