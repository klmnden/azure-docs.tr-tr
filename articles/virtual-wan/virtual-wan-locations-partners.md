---
title: Azure sanal WAN ortakları konumları | Microsoft Docs
description: Bu makalede, Azure sanal WAN iş ortakları ve hub konumları listesini içerir.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect find a Virtual WAN partner
ms.openlocfilehash: f38cd0565b2e90fe0803d8e815c622e22e954a18
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60459861"
---
# <a name="virtual-wan-partners-and-virtual-hub-locations"></a>Sanal WAN iş ortakları ve sanal hub konumları

Bu makalede, sanal WAN hakkında bilgi sanal hub bağlantısı için bölgeler ve iş ortakları desteklenen sağlar.

Azure Sanal WAN, Azure aracılığıyla şubeden şubeye iyileştirilmiş ve otomatik bağlantı sağlayan bir ağ hizmetidir. Sanal WAN, Azure ile iletişim kurmak için dal cihazlarını bağlamanızı ve yapılandırmanızı sağlar. Bu, el ile veya bir sanal WAN iş ortağı aracılığıyla sağlayıcısı cihazları kullanarak yapılabilir. İş ortağı cihazları kullanarak, bağlantı ve yapılandırma yönetimini basitleştirme kullanımını kolaylaştırmak sağlar.

Şirket içi cihaz bağlantısı, sanal hub'ına otomatik bir şekilde kurulur. Bir sanal hub'ı Microsoft tarafından yönetilen bir sanal ağ ' dir. Hub'da, şirket içi ağınızdan (vpnsite) gelen bağlantıyı etkinleştirmek için çeşitli hizmet uç noktaları bulunur. Bölge başına yalnızca tek bir hub olabilir.

## <a name="automation"></a>Otomasyon bağlantı iş ortakları

Azure sanal WAN için bağlanan cihazların bağlanmak için yerleşik Otomasyon vardır. Bu ayar genellikle VPN dal cihaza bir Azure sanal Hub VPN uç noktası (VPN ağ geçidi) arasında bağlantı ve yapılandırma yönetimi ayarlayan yukarı cihaz Yönetimi kullanıcı Arabirimi (veya eşdeğer).

Aşağıdaki üst düzey Otomasyon cihaz konsol/Yönetim Merkezi'nde ayarlanır:

* Azure sanal WAN kaynak grubuna erişmek cihaz için uygun izinleri
* Azure sanal WAN dal cihazı karşıya yükleme
* Azure bağlantı bilgileri otomatik indirilmesi
* Şirket içi dal cihaz yapılandırması 

Bazı bağlantı iş ortakları, Azure sanal Hub sanal ağ ve VPN ağ geçidi oluşturma dahil etmek için Otomasyon genişletebilir. Otomasyonu hakkında daha fazla bilgi edinmek istiyorsanız bkz [yapılandırma Otomasyonu – WAN iş ortakları](virtual-wan-configure-automation-providers.md).

## <a name="partners"></a>İş ortakları üzerinden bağlantı

[!INCLUDE [partners](../../includes/virtual-wan-partners-include.md)]

## <a name="locations"></a>konumları

[!INCLUDE [regions](../../includes/virtual-wan-regions-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Sanal WAN hakkında daha fazla bilgi için bkz: [sanal WAN SSS](virtual-wan-faq.md).

* Azure sanal WAN bağlantısı otomatikleştirme hakkında daha fazla bilgi için bkz. [sanal WAN iş ortakları - otomatikleştirme](virtual-wan-configure-automation-providers.md).
