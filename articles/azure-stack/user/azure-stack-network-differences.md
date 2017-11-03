---
title: "Azure yığın ağ: Farklar ve dikkat edilecek noktalar"
description: "Farklar ve konuları Azure yığınında ağ ile çalışırken öğrenin."
services: azure-stack
keywords: 
author: ScottNapolitan
ms.author: victorh
ms.date: 9/25/2017
ms.topic: article
ms.service: azure-stack
ms.openlocfilehash: 7b7bac508a759a1367ac7328840848efe17ea3c5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="considerations-for-azure-stack-networking"></a>Azure yığın ağ için ilgili önemli noktalar

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığınında ağ dağıtmaya başlamadan önce anlamanız gereken bazı farklar ile Azure içinde bulma özelliklerinin çoğunu sağlar.


Bu makalede, ağ ve özelliklerini Azure yığınında benzersiz konular genel bir bakış sağlar. Azure yığını ve Azure arasında üst düzey farklılıklar hakkında bilgi edinmek için [anahtar konuları](azure-stack-considerations.md) konu.


## <a name="cheat-sheet-networking-differences"></a>Kopya sayfası: ağ farklar

|Hizmet | Özellik | Azure (Genel) | Azure Stack |
| --- | --- | --- | --- |
| DNS | Çok kiracılı DNS | Destekleniyor| Henüz desteklenmiyor|
| |DNS AAAA kayıt|Destekleniyor|Desteklenmiyor|
| |Abonelik başına DNS bölgeleri|100 (varsayılan)<br>İstek üzerine artırılabilir.|100|
| |Her bölge için DNS kayıt kümeleri|5000 (varsayılan)<br>İstek üzerine artırılabilir.|5000|
||Ad sunucuları için bölge temsilcisi seçme|Azure oluşturulan her kullanıcı (Kiracı) bölge için dört ad sunucuları belirtin.|Azure yığın oluşturulan her kullanıcı (Kiracı) bölge için iki ad sunucuları sağlar.|
| Sanal ağ|Sanal ağ eşleme|Aynı bölgedeki iki sanal ağı Azure omurga ağı aracılığıyla bağlayın.|Henüz desteklenmiyor|
| |IPv6 adresleri|Bir IPv6 adresi bir parçası olarak atayabilir [ağ arabirimi yapılandırması](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-network-interface-addresses#ip-address-versions).|Yalnızca IPv4 desteklenir.|
|VPN ağ geçitleri|Noktadan siteye VPN ağ geçidi|Destekleniyor|Henüz desteklenmiyor|
| |Vnet-Vnet ağ geçidi|Destekleniyor|Henüz desteklenmiyor|
| |VPN ağ geçidi SKU'ları|Basic, GW1, GW2, GW3, standart yüksek performans, son derece yüksek performans için destek. |Temel, standart ve yüksek performanslı SKU'ları için destek.|
|Yük dengeleyici|IPv6 genel IP adresleri|Bir yük dengeleyiciye IPv6 ortak IP adresi atamak için destek.|Yalnızca IPv4 desteklenir.|
|Ağ İzleyicisi|Ağ İzleyicisi Kiracı ağ izleme kapasiteleri|Destekleniyor|Henüz desteklenmiyor|
|CDN|İçerik teslim ağı profilleri|Destekleniyor|Henüz desteklenmiyor|
|Uygulama ağ geçidi|Katman 7 Yük Dengeleme|Destekleniyor|Henüz desteklenmiyor|
|Traffic Manager|En iyi uygulama performansı ve güvenilirliği gelen trafiğin yönlendirin.|Destekleniyor|Henüz desteklenmiyor|
|Express Route|Bir hızlı, özel bağlantısı Microsoft bulut hizmetlerine şirket içi altyapı veya birlikte bulundurma tesis ayarlayın.|Destekleniyor|Hızlı rota devresi Azure yığın bağlamak için destek.|

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack’te DNS](azure-stack-dns.md)
