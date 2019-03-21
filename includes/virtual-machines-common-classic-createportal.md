---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 361d0ce5091d80198d47e4ad164f7cba8e21a55d
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58099517"
---
*Özel* bir sanal makine, **Mağaza**’daki **öne çıkan uygulamaları** kullanarak oluşturabileceğiniz bir sanal makinedir ve işin çoğunu sizin yerinize yapar. Ancak siz de aşağıdakiler dahil olmak üzere belirli yapılandırma seçeneklerini kullanabilirsiniz:

* Sanal makineyi bir sanal ağa bağlama.
* Kötü amaçlı yazılımdan korunma gibi nedenlerle Azure Sanal Makinesi Aracısı ve Azure Sanal Makine Uzantılarını yükleme.
* Var olan bulut hizmetlerine sanal makine ekleme.
* Sanal makineyi var olan Depolama hesabına ekleme.
* Sanal makineyi bir kullanılabilirlik kümesine ekleme.

<!--
> [!IMPORTANT]
> If you want your virtual machine to use a virtual network so you can connect to it directly by host name or set up cross-premises connections, make sure that you specify the virtual network when you create the virtual machine. A virtual machine can be configured to join a virtual network only when you create the virtual machine. For details on virtual networks, see [Azure Virtual Network overview](../articles/virtual-network/virtual-networks-overview.md).
>
>
 -->

> [!IMPORTANT]
> Sanal makinenizin bir sanal ağı kullanmasını istiyorsanız, sanal makineyi oluştururken sanal ağı da belirtmeniz gerekir.
> 
> * Sanal ağ kullanmanın iki avantajı, sanal makineye doğrudan bağlanmak ve şirket içi bağlantıları kurmaktır.
> 
> * Bir sanal makine, yalnızca sanal makineyi oluşturduğunuzda sanal ağa katılmak için yapılandırılabilir. Sanal ağlar hakkında daha fazla bilgi için bkz. [Azure Sanal Ağına Genel Bakış](../articles/virtual-network/virtual-networks-overview.md).

## <a name="to-create-the-virtual-machine"></a>Sanal makineyi oluşturmak için
