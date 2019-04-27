---
title: Azure hizmetleri için sanal ağ
titlesuffix: Azure Virtual Network
description: Bir sanal ağa dağıtma kaynakları avantajları hakkında bilgi edinin. Sanal ağlardaki kaynaklar birbiriyle iletişim kurabilir ve kaynaklar olmadan, Internet'e geçiş trafiği şirket.
services: virtual-network
documentationcenter: na
author: malopMSFT
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: malop;kumud
ms.openlocfilehash: c9b2f7244731be67628776b032e041457900353c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60742214"
---
# <a name="virtual-network-integration-for-azure-services"></a>Azure Hizmetleri için sanal ağ tümleştirmesi

Azure Hizmetleri için bir Azure sanal ağ tümleştirme özel erişim için hizmet sanal makine veya sanal ağda işlem kaynakları sağlar.
Azure Hizmetleri, sanal ağınızda aşağıdaki seçeneklerle tümleştirebilirsiniz:
- Adanmış hizmet örneğini bir sanal ağa dağıtma. Hizmetler, ardından sanal ağ içinde ve şirket içi ağlardan özel erişim sağlanabilir.
- Bir sanal ağ hizmet uç noktaları aracılığıyla hizmetine genişletme. Hizmet uç noktaları sanal ağa güvenli olması için bireysel hizmet kaynakları sağlar.

Birden çok Azure hizmeti sanal ağınıza tümleştirmek için bir veya daha fazla yukarıdaki desenleri birleştirebilirsiniz. Örneğin, HDInsight, sanal ağınıza dağıtma ve HDInsight alt ağ hizmet uç noktaları aracılığıyla bir depolama hesabına güvenli.
 
## <a name="deploy-azure-services-into-virtual-networks"></a>Azure hizmetlerini sanal ağlara dağıtma

Adanmış Azure hizmetlerinde dağıtırken bir [sanal ağ](virtual-networks-overview.md), hizmet kaynakları ile özel olarak, özel IP adresleri aracılığıyla iletişim kurabilir.

![Bir sanal ağda dağıtılan hizmetler](./media/virtual-network-for-azure-services/deploy-service-into-vnet.png)

Bir sanal ağ içindeki Hizmetleri dağıtımı aşağıdaki özellikleri sağlar:

- Sanal ağ içindeki kaynakların birbiriyle özel olarak, özel IP adresleri aracılığıyla iletişim kurabilir. Örneğin, doğrudan sanal ağdaki bir sanal makinede çalışan SQL Server ile HDInsight arasındaki veri aktarımı.
- Şirket kaynakları, özel IP adresleri üzerinden bir sanal ağdaki kaynaklara erişebilir bir [siteden siteye VPN (VPN ağ geçidi)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti) veya [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Sanal ağların [eşlenmiş](virtual-network-peering-overview.md) birbirleri ile iletişim kurmak için sanal ağlarda bulunan kaynaklar etkinleştirmek için özel IP adresleri kullanarak.
- Bir sanal ağ hizmet örnekleri örneklerinin sistem durumunu izleme ve yüküne göre gerekli ölçek sağlamak için Azure hizmeti tarafından tamamen yönetilir.
- Hizmet örnekleri, bir sanal ağdaki bir alt ağa dağıtılır. Gelen ve giden ağ erişimi açılır, aracılığıyla [ağ güvenlik grupları](security-overview.md#network-security-groups) alt ağına göre hizmetler tarafından sağlanan yönergeler için.
- İsteğe bağlı olarak, hizmetleri gerektirebilecek bir [alt temsilci](virtual-network-manage-subnet.md#add-a-subnet) bir alt ağ, belirli bir hizmeti barındırabilir açık bir tanımlayıcı olarak. Alt ağ temsilci hizmet alt ağında hizmete özgü kaynakları oluşturmak için açık izinler verir.

### <a name="services-that-can-be-deployed-into-a-virtual-network"></a>Bir sanal ağa dağıtılan hizmetler

|Kategori|Hizmet|
|-|-|
| İşlem | Sanal makineler: [Linux](../virtual-machines/linux/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Windows](../virtual-machines/windows/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Sanal makine ölçek kümeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-mvss-existing-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Bulut hizmeti](https://msdn.microsoft.com/library/azure/jj156091): Sanal ağ (Klasik) yalnızca<br/> [Azure Batch](../batch/batch-api-basics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-network-vnet-and-firewall-configuration)  |
| Ağ | [Uygulama ağ geçidi - WAF](../application-gateway/application-gateway-ilb-arm.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure güvenlik duvarı](../firewall/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) <br/>[Ağ sanal Gereçleri](/windows-server/networking/sdn/manage/use-network-virtual-appliances-on-a-vn) 
|Veriler|[RedisCache](../azure-cache-for-redis/cache-how-to-premium-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure SQL veritabanı yönetilen örneği](../sql-database/sql-database-managed-instance-connectivity-architecture.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
Analiz | [Azure HDInsight](../hdinsight/hdinsight-extend-hadoop-virtual-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Databricks](../azure-databricks/what-is-azure-databricks.md?toc=%2fazure%2fvirtual-network%2ftoc.json) |
| Kimlik | [Azure Active Directory etki alanı Hizmetleri](../active-directory-domain-services/active-directory-ds-getting-started-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json) |
| Kapsayıcılar | [Azure Kubernetes Service (AKS)](../aks/concepts-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Container örneği (ACI)](https://www.aka.ms/acivnet)<br/>[Azure Container Service altyapısı](https://github.com/Azure/acs-engine) ile Azure sanal ağ CNI [eklentisi](https://github.com/Azure/acs-engine/tree/master/examples/vnet)|
| Web | [API Management](../api-management/api-management-using-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[App Service Ortamı](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>|
| Şirket Dışında Barındırılan | [Azure ayrılmış HSM](../dedicated-hsm/index.yml?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure NetApp dosyaları](../azure-netapp-files/azure-netapp-files-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>|
|||



## <a name="service-endpoints-for-azure-services"></a>Azure Hizmetleri için hizmet uç noktası

Bazı Azure Hizmetleri, sanal ağları dağıtılamıyor. Bir sanal ağ hizmet uç noktası etkinleştirerek seçerseniz, bazı hizmet kaynaklarının yalnızca belirli bir sanal ağ alt ağları için erişimi kısıtlayabilirsiniz.  Daha fazla bilgi edinin [sanal ağ hizmet uç noktaları](virtual-network-service-endpoints-overview.md)ve uç noktaları için etkin hale getirilebilir Hizmetleri.
