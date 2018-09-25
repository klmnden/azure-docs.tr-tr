---
title: Azure sanal WAN ortakları konumları | Microsoft Docs
description: Bu makale, Azure sanal WAN iş ortakları ve hub konumları listesini içerir.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect find a Virtual WAN partner
ms.openlocfilehash: 76542392522ff14642ed7ccc6de8ed543d66513f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46985002"
---
# <a name="virtual-wan-partners-and-virtual-hub-locations"></a>Sanal WAN iş ortakları ve sanal hub konumları

Bu makalede, sanal WAN hakkında bilgi sanal hub bağlantısı için bölgeler ve tercih edilen iş ortakları desteklenen sağlar.

Azure Sanal WAN, Azure aracılığıyla şubeden şubeye iyileştirilmiş ve otomatik bağlantı sağlayan bir ağ hizmetidir. Sanal WAN, Azure ile iletişim kurmak için dal cihazlarını bağlamanızı ve yapılandırmanızı sağlar. Bu, el ile veya bir sanal WAN tercih edilen bir iş ortağı aracılığıyla tercih edilen sağlayıcısı cihazları kullanarak yapılabilir. Tercih edilen iş ortağı cihazları kullanarak, bağlantı ve yapılandırma yönetimini basitleştirme kullanımını kolaylaştırmak sağlar. Şirket içi cihaz bağlantısı, sanal hub'ına otomatik bir şekilde kurulur. Bir sanal hub'ı Microsoft tarafından yönetilen bir sanal ağ ' dir. Hub'da, şirket içi ağınızdan (vpnsite) gelen bağlantıyı etkinleştirmek için çeşitli hizmet uç noktaları bulunur. Bölge başına yalnızca tek bir hub olabilir.

## <a name="regions"></a>Bölgeler

Desteklenen ve kullanılabilir bölgelerin listesi şu şekildedir:

|Coğrafi bölge | Azure bölgeleri|
|---|---|
|Kuzey Amerika | Doğu ABD, Batı ABD, Doğu ABD 2, Batı ABD 2, Orta ABD, Orta Güney ABD, Orta Kuzey ABD, Batı Orta ABD, Orta Kanada, Doğu Kanada |
|Güney Amerika |Güney Brezilya |
| Avrupa | Fransa Orta, Fransa Güney, Kuzey Avrupa, Batı Avrupa, UK Batı, UK Güney |
| Asya | Doğu Asya, Güneydoğu Asya |
| Japonya  | Batı Japonya, Doğu Japonya |
| Avustralya | Güneydoğu Avustralya, Doğu Avustralya | 
| Avustralya Devleti | Avustralya Orta, Avustralya Orta 2 |
| Hindistan | Batı Hindistan, Orta Hindistan, Güney Hindistan |
| Güney Kore | Kore Orta, Kore Güney | 

## <a name="automation-from-connectivity-partners"></a>Otomasyon bağlantı iş ortakları

Bağlantı sağlayıcıları Otomasyonu üst düzey ayrıntılarını bu bölümde açıklanmaktadır.

Azure sanal WAN için bağlanan cihazların bağlanmak için yerleşik Otomasyon vardır. Bu ayar genellikle VPN dal cihaza bir Azure sanal Hub VPN uç noktası (VPN ağ geçidi) arasında bağlantı ve yapılandırma yönetimi ayarlayan yukarı cihaz Yönetimi kullanıcı Arabirimi (veya eşdeğer).

Aşağıdaki üst düzey Otomasyon cihaz konsol/Yönetim Merkezi'nde ayarlanır:

* Azure sanal WAN kaynak grubuna erişmek cihaz için uygun izinleri
* Azure sanal WAN dal cihazı karşıya yükleme
* Azure bağlantı bilgileri otomatik indirilmesi 
* Şirket içi dal cihaz yapılandırması 

Bazı bağlantı iş ortakları, Azure sanal Hub sanal ağ ve VPN ağ geçidi oluşturma dahil etmek için Otomasyon genişletebilir. Otomasyonu hakkında daha fazla bilgi edinmek istiyorsanız bkz [yapılandırma Otomasyonu – WAN iş ortakları](virtual-wan-configure-automation-providers.md).

## <a name="connectivity-through-preferred-partners"></a>Tercih edilen iş ortakları üzerinden bağlantı

Aşağıdaki bölümde, dal cihaz iş ortağı listede yoksa, lütfen dal cihaz sağlayıcınıza başvurun ve bunları bir e-posta göndererek bizimle iletişime geçin azurevirtualwan@microsoft.com.

Tercih edilen iş ortakları tarafından sunulan hizmetler hakkında daha fazla bilgi toplamak için aşağıdaki bağlantıları kontrol edebilirsiniz. 

|Tercih edilen iş ortakları|
|---|
|[Barracuda ağlar](https://www.barracuda.com/AzurevWAN)|
| [Denetim noktası](https://www.checkpoint.com/solutions/microsoft-azure-virtual-wan/) |
| [Citrix](https://www.citrix.com/global-partners/microsoft/sd-wan-for-azure-virtual-wan.html)|
|[Netfoundry](https://netfoundry.io/solutions/netfoundry-for-microsoft-azure-virtual-wan/)|
|[Palo Alto Networks tarafından sağlanan](https://researchcenter.paloaltonetworks.com/2018/09/azure-vwan-integration/) |
|[Riverbed teknolojisi](https://www.riverbed.com/go/steelconnect-azurewan.html)|
|[128 teknolojisi](https://www.128technology.com/partners/azure) |

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için bkz: [sanal WAN SSS](virtual-wan-faq.md).

Azure sanal WAN bağlantısı otomatikleştirme hakkında daha fazla bilgi için bkz. [sanal WAN iş ortakları - otomatikleştirme](virtual-wan-configure-automation-providers.md).
