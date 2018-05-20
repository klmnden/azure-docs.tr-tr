---
title: 'Azure yığın ağ: Farklar ve dikkat edilecek noktalar'
description: Farklar ve konuları Azure yığınında ağ ile çalışırken öğrenin.
services: azure-stack
keywords: ''
author: mattbriggs
manager: femila
ms.author: mabrigg
ms.date: 05/14/2018
ms.topic: article
ms.service: azure-stack
ms.openlocfilehash: 2a4c5bce072970f158a89763ebdf4132eafe9cbe
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="considerations-for-azure-stack-networking"></a>Azure yığın ağ için ilgili önemli noktalar

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure ağ yığını, Azure ağ tarafından sağlanan özelliklerinin çoğu vardır. Ancak, bir Azure yığın ağ dağıtmadan önce anlamanız gereken bazı temel farklar vardır.

Bu makalede Azure ağ yığını ve özellikleri için benzersiz konuları genel bir bakış sağlar. Azure yığını ve Azure arasında üst düzey farklılıklar hakkında bilgi edinmek için [anahtar konuları](azure-stack-considerations.md) konu.

## <a name="cheat-sheet-networking-differences"></a>Kopya sayfası: ağ farklar

|Hizmet | Özellik | Azure (Genel) | Azure Stack |
| --- | --- | --- | --- |
| DNS | Çok kiracılı DNS | Desteklenen| Henüz desteklenmiyor|
| |DNS AAAA kayıt|Desteklenen|Desteklenmiyor|
| |Abonelik başına DNS bölgeleri|100 (varsayılan)<br>İstek üzerine artırılabilir.|100|
| |Her bölge için DNS kayıt kümeleri|5000 (varsayılan)<br>İstek üzerine artırılabilir.|5000|
||Ad sunucuları için bölge temsilcisi seçme|Azure oluşturulan her kullanıcı (Kiracı) bölge için dört ad sunucuları sağlar.|Azure yığın oluşturulan her kullanıcı (Kiracı) bölge için iki ad sunucuları sağlar.|
| Sanal ağ|Sanal ağ eşleme|Aynı bölgedeki iki sanal ağı Azure omurga ağı aracılığıyla bağlayın.|Henüz desteklenmiyor|
| |IPv6 adresleri|Bir IPv6 adresi bir parçası olarak atayabilir [ağ arabirimi yapılandırması](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface-addresses#ip-address-versions).|Yalnızca IPv4 desteklenir.|
|VPN ağ geçitleri|Noktadan siteye VPN ağ geçidi|Desteklenen|Henüz desteklenmiyor|
| |Vnet-Vnet ağ geçidi|Desteklenen|Henüz desteklenmiyor|
| |VPN ağ geçidi SKU'ları|Basic, GW1, GW2, GW3, standart yüksek performans, son derece yüksek performans için destek. |Temel, standart ve yüksek performanslı SKU'ları için destek.|
|Yük dengeleyici|IPv6 genel IP adresleri|Bir yük dengeleyiciye IPv6 ortak IP adresi atamak için destek.|Yalnızca IPv4 desteklenir.|
|Ağ İzleyicisi|Ağ İzleyicisi Kiracı ağ izleme kapasiteleri|Desteklenen|Henüz desteklenmiyor|
|CDN|İçerik teslim ağı profilleri|Desteklenen|Henüz desteklenmiyor|
|Uygulama ağ geçidi|Katman 7 Yük Dengeleme|Desteklenen|Henüz desteklenmiyor|
|Traffic Manager|En iyi uygulama performansı ve güvenilirliği gelen trafiğin yönlendirin.|Desteklenen|Henüz desteklenmiyor|
|Express Route|Bir hızlı, özel bağlantısı Microsoft bulut hizmetlerine şirket içi altyapı veya birlikte bulundurma tesis ayarlayın.|Desteklenen|Hızlı rota devresi Azure yığın bağlamak için destek.|

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack’te DNS](azure-stack-dns.md)
