---
title: Şirket içi veri ağ geçidini kullan Azure sanal ağı veri kaynakları için | Microsoft Docs
description: VNet üzerinde veri kaynakları için bir ağ geçidi kullanmak için bir sunucuyu yapılandırmak öğrenin.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 7e792114c61c6257f4f5be5bfa65474d595f0d36
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="use-gateway-for-data-sources-on-an-azure-virtual-network-vnet"></a>Bir Azure sanal ağ (VNet) üzerindeki veri kaynakları için ağ geçidini kullan

Bu makalede **AlwaysUseGateway** veri kaynaklarının etkin durumdayken kullanmak için sunucu özelliği bir [Azure sanal ağ (VNet)](../virtual-network/virtual-networks-overview.md).

## <a name="server-access-to-vnet-data-sources"></a>VNet veri kaynaklarına sunucu erişimi

Veri kaynaklarınızı bir sanal ağ erişilen, şirket içi, kendi ortamınızdaki varsa gibi Azure Analysis Services sunucunuzun bu veri kaynakları için bağlanmanız gerekir. Yapılandırabileceğiniz **AlwaysUseGateway** tüm veri kaynağı verilerine erişmek için sunucu belirtmek için sunucu özelliği bir [şirket içi ağ geçidi](analysis-services-gateway.md). 

> [!NOTE]
> Bu özellik yalnızca etkin olduğunda olan bir [şirket içi veri ağ geçidi](analysis-services-gateway.md) yüklenir ve yapılandırılır. Ağ geçidi VNet üzerinde olabilir.

## <a name="configure-alwaysusegateway-property"></a>AlwaysUseGateway özelliğini yapılandırın

1. SSMS içinde > server > **özellikleri** > **genel**seçin **Göster, Gelişmiş (Tümü) özellikleri**.
2. İçinde **ASPaaS\AlwaysUseGateway**seçin **doğru**.

    ![Her zaman ağ geçidi özelliğini kullanın](media/analysis-services-vnet-gateway/aas-ssms-always-property.png)


## <a name="see-also"></a>Ayrıca bkz.
[Şirket içi veri kaynaklarına bağlanma](analysis-services-gateway.md)   
[Yükleme ve bir şirket içi veri ağ geçidi yapılandırma](analysis-services-gateway-install.md)   
[Azure sanal ağı (VNET)](../virtual-network/virtual-networks-overview.md)   

