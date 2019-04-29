---
author: rockboyfor
ms.service: virtual-network
ms.topic: include
origin.date: 11/09/2018
ms.date: 01/21/2019
ms.author: v-yeche
ms.openlocfilehash: 3df4108907a4e1e65a444faf1049163966b7accf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60742257"
---
## <a name="scenario"></a>Senaryo
Tek bir NIC ile bir VM oluşturulur ve bir sanal ağa bağlı. Üç farklı VM gerektirir *özel* IP adresleri ve iki *genel* IP adresleri. Aşağıdaki IP yapılandırmaları için IP adresleri atanır:

* **Ipconfig-1:** Atayan bir *statik* özel IP adresi ve *statik* genel IP adresi.
* **Ipconfig-2:** Atayan bir *statik* özel IP adresi ve *statik* genel IP adresi.
* **Ipconfig-3:** Atayan bir *statik* özel IP adresi ve genel IP adresi yok.

    ![Birden çok IP adresi](./media/virtual-network-multiple-ip-addresses-scenario/multiple-ipconfigs.png)

NIC oluşturulduğunda ve VM oluşturulduğunda VM'ye ekli NIC IP yapılandırmaları NIC'ye ilişkilendirilir. Senaryo için kullanılan IP adresleri için çizim türleridir. İhtiyaç duyduğunuz her IP adresi ve atama türleri atayabilirsiniz.

> [!NOTE]
> Bu makaledeki adımlarda atar ancak tüm IP yapılandırmaları tek bir NIC'ye birden fazla IP yapılandırması multi-NIC VM ağdaki nıc'lerden BİRİNE atayabilirsiniz. Birden çok NIC ile VM oluşturma konusunda bilgi edinmek için [birden çok NIC ile VM oluşturma](../articles/virtual-machines/windows/multiple-nics.md) makalesi.
