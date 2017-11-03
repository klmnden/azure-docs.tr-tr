---
title: "Windows sanal makineleri bir bulut hizmetine bağlanmak | Microsoft Docs"
description: "Bir Azure bulut hizmeti veya sanal ağı Klasik dağıtım modeliyle oluşturulan Windows sanal makineleri bağlayın."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: c1cbc802-4352-4d2e-9e49-4ccbd955324b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: cynthn
ms.openlocfilehash: 0c7a21461e5bb111c4359df8e949d48382b591c1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="connect-windows-virtual-machines-created-with-the-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a>Bir sanal ağ veya Bulut hizmeti ile klasik dağıtım modeliyle oluşturulan Windows sanal makineleri Bağlan
> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

Klasik dağıtım modeliyle oluşturulan Windows sanal makinelerin her zaman bir bulut hizmetinde yerleştirilir. Bulut hizmeti bir kapsayıcı gibi davranır ve benzersiz bir genel DNS adı, bir ortak IP adresi ve sanal makine Internet üzerinden erişmek için uç noktalar kümesi sağlar. Bulut hizmeti sanal bir ağa olabilir, ancak bu zorunlu değildir. Ayrıca [Linux sanal makineleri bir sanal ağ veya Bulut hizmetiyle bağlantı](../../linux/classic/connect-vms.md).

Bir bulut hizmeti bir sanal ağ içinde değilse, adlı bir *tek başına* bulut hizmeti. Sanal makine bir tek başına bulut hizmeti, diğer sanal makineler ortak DNS adlarını kullanarak diğer sanal makinelerle iletişim kurmak ve trafik Internet üzerinden geçen. Bir bulut hizmeti sanal bir ağda ise, bulut hizmetinin'nde sanal makinelerin sanal ağdaki diğer tüm sanal makinelerin tüm trafik Internet üzerinden göndermeden iletişim kurabilir.

Sanal makinelerinizi aynı tek başına bulut hizmeti yerleştirirseniz, Yük Dengeleme ve kullanılabilirlik kümelerini kullanmaya devam edebilirsiniz. Ayrıntılar için bkz [Yük Dengeleme sanal makineleri](../../virtual-machines-windows-load-balance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ve [sanal makinelerin kullanılabilirliğini yönetme](../../virtual-machines-windows-manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Ancak, alt ağlar sanal makinelerde düzenlemek veya tek başına bulut hizmeti şirket içi ağınıza bağlayın. Örnek aşağıda verilmiştir:

[!INCLUDE [virtual-machines-common-classic-connect-vms](../../../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bir sanal makine oluşturduktan sonra iyi bir fikir olduğu [bir veri diski Ekle](attach-disk.md) böylece Hizmetleri ve iş yüklerini veri depolamak için bir konum.
