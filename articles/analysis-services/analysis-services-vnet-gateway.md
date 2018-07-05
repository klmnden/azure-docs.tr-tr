---
title: Azure sanal ağı veri kaynakları için kullanım şirket içi veri ağ geçidi | Microsoft Docs
description: VNet üzerindeki veri kaynakları için bir ağ geçidi kullanmak üzere bir sunucu yapılandırmayı öğrenin.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: f1cf447f27509bc62cc71a9c64237ff7dc3d7b30
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37446112"
---
# <a name="use-gateway-for-data-sources-on-an-azure-virtual-network-vnet"></a>Bir Azure sanal ağ (VNet) üzerindeki veri kaynakları için ağ geçidi kullanma

Bu makalede **AlwaysUseGateway** veri kaynakları üzerinde olduğunda kullanmak için sunucu özelliği bir [Azure sanal ağı (VNet)](../virtual-network/virtual-networks-overview.md).

## <a name="server-access-to-vnet-data-sources"></a>Sunucu VNet veri kaynaklarına erişim

Azure Analysis Services sunucunuz, şirket içinde kendi ortamınızda olmaları durumunda gibi veri kaynaklarınızı sanal ağ eriştiyseniz bu veri kaynaklarına bağlanmanız gerekir. Yapılandırabileceğiniz **AlwaysUseGateway** sunucu özelliği, tüm veri kaynağı verilerine erişmek için sunucu belirtmek için bir [şirket içi ağ geçidi](analysis-services-gateway.md). 

> [!NOTE]
> Bu özellik yalnızca etkin olduğunda, bir [şirket içi veri ağ geçidi](analysis-services-gateway.md) yüklenir ve yapılandırılır. Ağ geçidi VNet üzerinde olabilir.

## <a name="configure-alwaysusegateway-property"></a>AlwaysUseGateway özelliğini yapılandırın

1. Ssms'de > sunucu > **özellikleri** > **genel**seçin **Özellikleri Göster, Gelişmiş (Tümü)**.
2. İçinde **ASPaaS\AlwaysUseGateway**seçin **true**.

    ![Her zaman ağ geçidi özelliğini kullanın](media/analysis-services-vnet-gateway/aas-ssms-always-property.png)


## <a name="see-also"></a>Ayrıca bkz.
[Şirket içi veri kaynaklarına bağlanma](analysis-services-gateway.md)   
[Yükleme ve bir şirket içi veri ağ geçidi yapılandırma](analysis-services-gateway-install.md)   
[Azure sanal ağı (VNET)](../virtual-network/virtual-networks-overview.md)   

