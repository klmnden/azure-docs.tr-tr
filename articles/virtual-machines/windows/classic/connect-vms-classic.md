---
title: Windows sanal makineleri bir bulut hizmetine bağlanmak | Microsoft Docs
description: Bir Azure bulut hizmeti veya sanal ağı Klasik dağıtım modeliyle oluşturulan Windows sanal makineleri bağlayın.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: c1cbc802-4352-4d2e-9e49-4ccbd955324b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: cynthn
ms.openlocfilehash: b5778336b2dfb3d65cb678c96525ec22da1634a0
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30917767"
---
# <a name="connect-windows-virtual-machines-created-with-the-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a>Bir sanal ağ veya Bulut hizmeti ile klasik dağıtım modeliyle oluşturulan Windows sanal makineleri Bağlan
> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

Klasik dağıtım modeliyle oluşturulan Windows sanal makinelerin her zaman bir bulut hizmetinde yerleştirilir. Bulut hizmeti bir kapsayıcı gibi davranır ve benzersiz bir genel DNS adı, bir ortak IP adresi ve sanal makine Internet üzerinden erişmek için uç noktalar kümesi sağlar. Bulut hizmeti sanal bir ağa olabilir, ancak bu zorunlu değildir.

Bir bulut hizmeti bir sanal ağ içinde değilse, adlı bir *tek başına* bulut hizmeti. Sanal makine bir tek başına bulut hizmeti, diğer sanal makineler ortak DNS adlarını kullanarak diğer sanal makinelerle iletişim kurmak ve trafik Internet üzerinden geçen. Bir bulut hizmeti sanal bir ağda ise, bulut hizmetinin'nde sanal makinelerin sanal ağdaki diğer tüm sanal makinelerin tüm trafik Internet üzerinden göndermeden iletişim kurabilir.

Sanal makinelerinizi aynı tek başına bulut hizmeti yerleştirirseniz, Yük Dengeleme ve kullanılabilirlik kümelerini kullanmaya devam edebilirsiniz. Ayrıntılar için bkz [Yük Dengeleme sanal makineleri](../../virtual-machines-windows-load-balance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ve [sanal makinelerin kullanılabilirliğini yönetme](../../virtual-machines-windows-manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Ancak, alt ağlar sanal makinelerde düzenlemek veya tek başına bulut hizmeti şirket içi ağınıza bağlayın. Bir örneği aşağıda verilmiştir:

[!INCLUDE [virtual-machines-common-classic-connect-vms](../../../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bir sanal makine oluşturduktan sonra iyi bir fikir olduğu [bir veri diski Ekle](attach-disk.md) böylece Hizmetleri ve iş yüklerini veri depolamak için bir konum.
