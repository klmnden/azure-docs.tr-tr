---
title: Azure Lab Services içinde bir eş ağa bağlayın | Microsoft Docs
description: Laboratuvar ağınıza bir eş olarak başka bir ağa bağlanmayı öğreneceksiniz. Örneğin, azure'da Laboratuvar'ın sanal ağ ile şirket içi Okul/üniversite ağınıza bağlayın.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2019
ms.author: spelluru
ms.openlocfilehash: c9b305beae1b385d4714e3a80e6843c7e76a4f60
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65410982"
---
# <a name="connect-your-labs-network-with-a-peer-virtual-network-in-azure-lab-services"></a>Azure Lab Services, eş sanal ağ ile Laboratuvar ağı bağlama 
Bu makalede labs ağınızla başka bir ağ eşlemesi hakkında bilgi sağlar. 

## <a name="overview"></a>Genel Bakış
Sanal Ağ eşlemesi, Azure sanal ağları sorunsuz bir şekilde bağlamanızı sağlar. Eşleme yapıldıktan sonra, bağlantı açısından sanal ağlar tek bir sanal ağ gibi görünür. Eşlenen sanal ağlarda bulunan sanal makineler arasındaki trafik Microsoft omurga altyapısı aracılığıyla yönlendirilir, yalnızca özel IP adresleri üzerinden aynı sanal ağdaki sanal makineler arasındaki trafik gibi yönlendirilir. Daha fazla bilgi için [sanal ağ eşlemesi](../../virtual-network/virtual-network-peering-overview.md).

Aşağıdaki sürücüler dahil olmak üzere bazı senaryolarda bir eş sanal ağ ile Laboratuvar ağı bağlama gerekebilir:

- Yazılım lisansı almak için şirket içi lisans sunucularına bağlanan sanal makinelerinizin Laboratuvardaki
- Laboratuvarındaki sanal makinelerde üniversitenin ağ paylaşımlarında veri kümelerine erişim (veya diğer dosyaları) gerekir. 

Belirli şirket içi ağlara ya da Azure sanal ağa bağlı aracılığıyla [ExpressRoute](../../expressroute/expressroute-introduction.md) veya [sanal ağ geçidi](../../vpn-gateway/vpn-gateway-about-vpngateways.md). Azure Lab Services dışında bu hizmetler ayarlanması gerekir. Şirket içi bir ağı ExpressRoute kullanarak Azure'a bağlama hakkında daha fazla bilgi edinmek için bkz. [Expressroute'a genel bakış]) (.. /expressroute/expressroute-introduction.MD). Sanal ağ ağ geçidi bir sanal ağ geçidi kullanarak şirket içi bağlantı için belirtilen ve Laboratuvar hesabı tümü aynı bölgede olmalıdır.

## <a name="configure-at-the-time-of-lab-account-creation"></a>Laboratuvar hesap oluşturma sırasında yapılandırma
Yeni Laboratuvar hesap oluşturma sırasında gösteren bir sanal ağınız çekme **eş sanal ağ** açılır liste. Seçilen sanal ağ, Laboratuvar hesabı altında oluşturulan laboratuvarlara connected(peered) ' dir. Tüm sanal makineleri yaptıktan sonra oluşturulan Labs bu değişiklik eşlenen sanal ağda kaynaklara erişime sahip. 

![Eş VNet seçin](../media/how-to-connect-peer-virtual-network/select-vnet-to-peer.png)

> [!NOTE]
> Bir laboratuvar hesabı oluşturmaya yönelik ayrıntılı adım adım yönergeler için bkz. [bir laboratuvar hesabı ayarlama](tutorial-setup-lab-account.md)


## <a name="configure-after-the-lab-is-created"></a>Laboratuvarı oluşturduktan sonra yapılandırma
Aynı özellik etkin getirilebilir **Labs yapılandırma** sekmesinde **Laboratuvar hesabı** Laboratuvar hesap oluşturma sırasında bir eş ağ ayarlamadınız, sayfa. Bu ayar için yaptığınız değişikliği değişiklikten sonra oluşturulan Laboratuvarları için geçerlidir. Görüntüde görebileceğiniz gibi etkinleştirebilir veya devre **eş sanal ağ** Labs'de bir laboratuvar hesabı için. 

![Etkinleştirmek veya devre dışı VNet laboratuvarı oluşturduktan sonra eşlemesi](../media/how-to-connect-peer-virtual-network/select-vnet-to-peer-existing-lab.png) 

Bir sanal ağ için seçtiğinizde, **eş sanal ağ** alanı **Laboratuvar konumunu seçmek için izin Laboratuvar oluşturan** seçeneği devre dışıdır. Laboratuvar hesabı Labs'de eş sanal ağ içindeki kaynaklarla bağlanabilmeleri için bir laboratuvar hesabı ile aynı bölgede olmalıdır olmasıdır. 

> [!IMPORTANT]
> Bu ayar değişikliği değil mevcut labs kullanarak değişiklik yapıldıktan sonra oluşturulan Laboratuvarları için geçerlidir. 


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Bir yönetici olarak oluşturun ve Laboratuvar hesaplarını yönetme](how-to-manage-lab-accounts.md)
- [Laboratuvar sahibi olarak oluşturun ve Laboratuvarları yönetin](how-to-manage-classroom-labs.md)
- [Laboratuvar sahibi olarak ayarlama ve şablonları Yayımlama](how-to-create-manage-template.md)
- [Bir laboratuvar kullanıcı olarak sınıf laboratuvarlarına erişim](how-to-use-classroom-lab.md)

