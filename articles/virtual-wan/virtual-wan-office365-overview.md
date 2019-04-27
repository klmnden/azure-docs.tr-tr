---
title: Office 365 denetim düzlemi, Azure sanal WAN
description: Sanal WAN, Office 365 denetim düzlemi hakkında bilgi edinin.
author: cherylmc
ms.service: virtual-wan
services: virtual-wan
ms.topic: article
ms.date: 9/24/2018
ms.author: cherylmc
ms.openlocfilehash: cb91c1364a91c101ecf8362acd7aab01440143fc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60458603"
---
# <a name="office-365-control-plane-in-virtual-wan"></a>Sanal WAN Office 365 denetim düzlemi

Sanal WAN müşterilerle SDWAN cihazları seçin, Azure portalında güvenilir trafiği için O365 Internet kırılımı ilkeleri yapılandırabilirsiniz. Bu sağlar:
- En iyi kullanıcı deneyimini vererek kullanıcı yakın Microsoft ağına girmeden O365 trafiğin.
- Trafiği arka-stoklarını çekme ve pining, WAN maliyetlerini vermeyip artı önler.
- O365 bağlantı kurallara göre teslim etme.

## <a name="faqs"></a>SSS
### <a name="what-is-the-customer-benefit"></a>Müşteri avantajı nedir?
Sanal WAN ' Bu özelliği kullanarak, müşteriler için doğrudan internet kırılımı güvendikleri Office 365 trafiği kategorileri artık belirtebilirsiniz. Başka bir güvenilir, O365 trafiği intranetlerinden proxy'leri ve en yakın POP Microsoft doğrudan kullanıcı konumu yolu. Bu, trafiği arka-stoklarını çekme ve pining, böylece en iyi kullanıcı deneyimi sağlamaya ve WAN maliyetlerini kaydetmeyi artı önler. 

### <a name="what-are-the-office-365-traffic-categories"></a>Office 365 trafiği kategorileri nelerdir?
Office 365 uç noktaları, ağ adresleri ve alt ağlar temsil eder. Uç noktalar olabilir URL, IP adresi veya IP aralıkları. URL'leri olabilir bir FQDN gibi *account.office.net*, veya bir joker karakter URL **. office365.com*. Uç noktaları üç kategoriye - yinelenmeli **İyileştir**, **izin**, ve **varsayılan**derecesine göre. Uç nokta kategorileri hakkında daha fazla ayrıntıyı [burada](https://docs.microsoft.com/office365/enterprise/office-365-network-connectivity-principles#BKMK_Categories).

### <a name="which-office-365-traffic-category-is-recommended-by-microsoft-for-direct-internet-breakout"></a>Office 365 trafiği kategorisini Microsoft tarafından doğrudan internet kırılımı için tavsiye edilir?
**İyileştir** kategorisi en kritik ağ uç noktaları ve SSL sonu atlamak ve incelemek için gerekli ve diğer güvenlik ağ. Bu, doğrudan Internet çıkış kullanıcılar yakın olması gerekir. Bu uç noktaları, ağ performansı, gecikme süresi ve kullanılabilirlik için en önemli olan Office 365 senaryolarını temsil eder. Bu kategori, URL'leri ve IP alt ağları tanımlı bir dizi Exchange Online, SharePoint Online, Skype gibi çekirdek Office 365 iş yükleri için çevrimiçi ve Microsoft Teams için ayrılmış bir anahtar (bazında ~ 10) küçük bir kümesini içerir. 

**İzin** kategori önerilir doğrudan Internet çıkışı için de. Ağ trafiğini yine de bazı ağ gecikmesi dayanabilir izin verir. Uç noktaları en iyi hale getir ve izin kategorilerdeki tüm Microsoft veri merkezlerinde barındırılan ve yönetilen Office 365'in bir parçası olarak. Varsayılan Kategori varsayılan Internet çıkış konumuna yönlendirilebilir ve değil doğrudan Internet çıkış gerektirir veya SSL kesme atlama ve cihazları inceleyin.

### <a name="how-do-i-set-my-o365-policies-via-virtual-wan"></a>Sanal WAN üzerinden my O365 ilkeleri nasıl ayarlayabilirim?
İlkeleri aracılığıyla etkinleştirebilirsiniz **sanal WAN** -> **ayarları** -> **yapılandırma** sekmesi. Burada, O365 trafiğin doğrudan internet kırılımı için tercih edilen kategorileriniz belirtebilirsiniz.

![Office 365 denetim düzlemi sanal WAN ' yapılandırma](media/virtual-wan-office365-overview/configure-office365-control-plane.png)

### <a name="how-does-this-work"></a>Nasıl çalışır?

1.  O365 trafik Microsoft ağına en iyi deneyim kullanıcı yakın girer.
2.  Rota ilkeleri SDWAN tarafından tüketilir. Güvenilen kategorileri için güvenlik proxy'leri atlar ve doğrudan yerel kırılımı Bu kategoriler için gerçekleştirir.
3.  Geri stoklarını çekme ve trafiği saç pining WAN maliyetlerini kaydetme engellendi.

### <a name="which-partner-devices-support-this-via-virtual-wan"></a>Hangi iş ortağı cihazların bu sanal WAN destekliyor?
Şu anda bu ilkeleri sanal WAN üzerinden Citrix destekler.

### <a name="what-happens-to-the-remaining-categories-of-untrusted-o365-traffic"></a>(Güvenilmeyen) O365 trafik kalan kategorilerine ne olacak?
O365 trafik kalan müşteriler varsayılan internet trafiği yolunu izleyin.

### <a name="what-if-i-have-already-specified-my-o365-policies-via-my-sdwan-provider"></a>Ne miyim zaten my O365 ilkeleri SDWAN sağlayıcım belirttiniz?
İlkeleri SDWAN UX hem de Azure sanal WAN üzerinden belirtirseniz, sanal WAN ' belirlenen politikalara öncelikli olur.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [sanal WAN](virtual-wan-about.md).