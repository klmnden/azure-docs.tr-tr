---
title: Azure SQL veritabanı örneği yönetilen uygulama bağlanma | Microsoft Docs
description: Bu makalede, uygulamanız yönetilen Azure SQL veritabanı örneğine bağlanma anlatılmaktadır.
ms.service: sql-database
author: srdjan-bozovic
manager: craigg
ms.custom: managed instance
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: bea1dc88d66717717cdeacbc8504f5df7e37ba04
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34647842"
---
# <a name="connect-your-application-to-azure-sql-database-managed-instance"></a>Uygulamanızı Azure SQL Veritabanı Yönetilen Örneği'ne bağlayın

Bugün nasıl ve nerede uygulamanızı barındırmak karar verirken birden çok seçeneğiniz vardır. 
 
Uygulamayı bulutta Azure uygulama hizmeti veya bazı Azure uygulama hizmeti ortamı, sanal makine, sanal makine ölçek kümesi gibi Azure'nın tümleşik sanal ağ (VNet) seçeneklerini kullanarak ya da barındırmak tercih edebilirsiniz. Ayrıca karma bulut yaklaşımı ve uygulamaları şirket içi tutun. 
 
Yaptığınız, herhangi bir seçim bir yönetilen örneğine (Önizleme) bağlanabilir.  

![Yüksek kullanılabilirlik](./media/sql-database-managed-instance/application-deployment-topologies.png)  

## <a name="connect-an-application-inside-the-same-vnet"></a>Aynı sanal ağ içindeki bir uygulama Bağlan 

Bu senaryo en kolayıdır. İçinde farklı alt ağlarda olsalar bile, sanal makineler sanal ağ içinde birbirlerine doğrudan bağlanabilir. Tüm uygulama içinde bir Azure uygulama ortamı ya da sanal makineye bağlanmak için gereken bağlantı dizesi uygun şekilde ayarlanmış olması anlamına gelir.  
 
Bağlantı kurulamıyor durumunda, bir ağ güvenlik grubu uygulama alt ağda ayarlanmış olup olmadığını denetleyin. Bu durumda, giden bağlantı SQL bağlantı noktası 1433 ve bunun yanı sıra 11000 12000 yeniden yönlendirme için bağlantı noktası aralığını açmanız gerekir. 

## <a name="connect-an-application-inside-a-different-vnet"></a>Farklı bir sanal ağ içindeki bir uygulama Bağlan 

Bu senaryo örneği yönetilen kendi VNet özel IP adresi olduğundan biraz daha karmaşıktır. Bağlanmak için bir uygulama örneği yönetilen dağıtıldığı sanal ağ erişimi olmalıdır. Bu nedenle, ilk olarak, uygulama ve yönetilen örneği VNet arasında bir bağlantı yapmanız gerekir. Sanal ağlar aynı abonelikte bu senaryonun işe yaraması için sırayla olmak zorunda değildir. 
 
Sanal ağlara bağlanmak için iki seçenek vardır: 
- [Azure sanal ağ eşlemesi](../virtual-network/virtual-network-peering-overview.md) 
- VNet-VNet VPN ağ geçidi ([Azure portal](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md), [Azure CLI](../vpn-gateway/vpn-gateway-howto-vnet-vnet-cli.md)) 
 
Eşleme Microsoft omurga ağı, bağlantı açısından kullandığından tercih edilen bir eşleme seçeneğidir, eşlenmiş VNet ve aynı Vnet'i sanal makineleri arasındaki gecikme süresi içinde dikkat çekici fark yoktur. VNet eşlemesi aynı bölgede ağları ile sınırlıdır.  
 
> [!IMPORTANT]
> VNet eşleme yönetilen örneği için bir senaryodur nedeniyle aynı bölgede ağlara sınırlı [genel sanal ağ eşlemesi, kısıtlamalar](../virtual-network/virtual-network-manage-peering.md#requirements-and-constraints). 

## <a name="connect-an-on-premises-application"></a>Bir şirket içi uygulama Bağlan 

Yönetilen örneği yalnızca özel bir IP adresi erişilebilir. Şirket içinden erişmek için uygulama ve yönetilen örneği VNet arasında bir siteden siteye bağlantı yapmanız gerekir. 
 
Şirket içi Azure Vnet'e bağlama iki seçenek vardır: 
- Siteden siteye VPN bağlantısı ([Azure portal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md), [Azure CLI](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)) 
- [ExpressRoute](../expressroute/expressroute-introduction.md) bağlantı  
 
Şirket içi Azure bağlantısı başarıyla belirlediğinize göre ve yönetilen örneğine bağlantı kuramıyor, güvenlik duvarı bağlantı noktaları yeniden yönlendirme 11000 12000 aralığı yanı sıra, SQL bağlantı noktası 1433 açık giden bağlantı olup olmadığını kontrol edin. 

## <a name="connect-an-azure-app-service-hosted-application"></a>Azure uygulama hizmeti barındırılan uygulamaya Bağlan 

Azure App hizmetinden erişmek için ilk uygulama ve yönetilen örneği VNet arasında bir bağlantı yapmanız gereken şekilde yönetilen örneği yalnızca özel bir IP adresi erişilebilir. Bkz: [uygulamanızı Azure sanal ağı ile tümleştirmek](../app-service/web-sites-integrate-with-vnet.md).  
 
Sorun giderme için bkz: [sorun giderme sanal ağlar ve uygulamaları](../app-service/web-sites-integrate-with-vnet.md#troubleshooting). Bir bağlantı kurulamazsa deneyin [ağ yapılandırmasını senkronize](sql-database-managed-instance-sync-network-configuration.md). 
 
Azure uygulama hizmeti yönetilen örneğine bağlayan bir özel durum bir ağ için Azure App Service yönetilen örneği VNet eşlendikten, tümleşik durumdur. Bu durumda ayarlanması aşağıdaki yapılandırmayı gerektirir: 

- Yönetilen örneği VNet ağ geçidi sahip olmamalıdır  
- Yönetilen örneği VNet kullanım uzak ağ geçitleri seçeneği ayarlanmış olması gerekir 
- Eşlenmiş Vnet'teki izin ağ geçidi geçiş seçeneği ayarlanmış olması gerekir 
 
Bu senaryo, aşağıdaki çizimde gösterilmiştir:

![tümleşik uygulama eşleme](./media/sql-database-managed-instance/integrated-app-peering.png)
 
## <a name="connect-an-application-on-the-developers-box"></a>Bir uygulama geliştiriciler kutusundaki Bağlan 

Yönetilen örneği yalnızca özel bir IP adresi Geliştirici kutusundan erişmek için bu nedenle erişilebilir, ilk Geliştirici kutunuzu ve yönetilen örneği VNet arasında bir bağlantı yapmanız gerekir.  
 
Yerel Azure sertifika kimlik doğrulaması makaleleri kullanarak bir sanal ağa noktadan siteye bağlantı yapılandırma ([Azure portal](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md), [Azure CLI](../vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal.md)) ayrıntılı olarak gösterilmiştir nasıl yapılabilir.  

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen örneği hakkında daha fazla bilgi için bkz: [yönetilen örneği nedir](sql-database-managed-instance.md).
- Yeni bir yönetilen örneğinin nasıl oluşturulacağını gösteren bir öğretici için bkz: [bir yönetilen örneği oluşturmayı](sql-database-managed-instance-create-tutorial-portal.md).
