---
title: VMware çözümüyle CloudSimple - Azure VPN ağ geçitleri
description: CloudSimple siteden siteye VPN ve noktadan siteye VPN kavramları hakkında bilgi edinin
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 2eae81f357904bd5034d7409ef42b681d1085930
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67695231"
---
# <a name="vpn-gateways-overview"></a>VPN ağ geçitleri genel bakış

Bir VPN ağ geçidi, şirket içi konum veya genel Internet üzerinden bir bilgisayar CloudSimple bölge ağ arasında şifrelenmiş trafik göndermek için kullanılır.  Her bölge, yalnızca bir VPN ağ geçidi olabilir. Ancak, aynı VPN ağ geçidi ile birden fazla bağlantı oluşturabilirsiniz. Aynı VPN ağ geçidiyle birden fazla bağlantı oluşturduğunuzda, tüm VPN tünelleri kullanılabilir ağ geçidi bant genişliğini paylaşır.

VPN ağ geçitlerinin iki tür CloudSimple sağlar:

* Siteden siteye VPN ağ geçidi
* Noktadan siteye VPN ağ geçidi

## <a name="site-to-site-vpn-gateway"></a>Siteden siteye VPN ağ geçidi

Siteden siteye VPN ağ geçidi CloudSimple bölge ağ ve şirket içi veri merkezi arasında şifrelenmiş trafik göndermek için kullanılır. Şirket içi ağınız ile CloudSimple bölge ağı arasındaki iletişim için bir alt ağ/CIDR aralığını tanımlamak için bu bağlantıyı kullanın.

VPN ağ geçidi, şirket içi özel bulutunuzda servislerini sağlar ve özel bulut, şirket içi ağdan gelen şirket Hizmetleri.  CloudSimple, şirket içi ağınız arasında bağlantı kurmak için bir ilke tabanlı VPN sunucusu sağlar.

Siteden siteye VPN için kullanım örnekleri şunlardır:

* Şirket içi ağınızda herhangi bir iş istasyonundan, özel bulut vCenter'ın erişilebilirlik.
* VCenter kimlik kaynağı olarak şirket içi Active Directory'nizde kullanımı.
* Özel bulut vCenter'ınıza için şirket içi kaynaklardan kullanışlı aktarım VM şablonları, Iso'lar ve diğer dosyaları.
* Şirket içi ağınızdan özel bulutunuzda çalışan iş yüklerini erişilebilirliğini.

![Siteden siteye VPN bağlantı topolojisi](media/cloudsimple-site-to-site-vpn-connection.png)

> [!IMPORTANT]
> TCP MSS 1078 bayt veya daha düşük sıkıştırmanız gerekir. Veya VPN cihazlarınız MSS clamping desteklemiyorsa, alternatif olarak MTU tünel arabiriminde 1118 bayt için bunun yerine ayarlayabilirsiniz. 

### <a name="cryptographic-parameters"></a>Şifreleme parametreleri

Siteden siteye VPN bağlantısı, güvenli bir bağlantı kurmak için aşağıdaki varsayılan şifreleme parametreleri kullanır.  Şirket içi VPN CİHAZDAN bir bağlantı oluşturduğunuzda, şirket içi VPN ağ geçidi tarafından desteklenen aşağıdaki parametreleri kullanın.

#### <a name="phase-1-proposals"></a>1\. Aşama teklifleri

| Parametre | Teklif 1 | Teklif 2 | Teklif 3 |
|-----------|------------|------------|------------|
| IKE Sürümü | IKEv1 | IKEv1 | IKEv1 |
| Şifreleme | AES 128 | AES 256 | AES 256 |
| Karma algoritması| SHA 256 | SHA 256 | SHA 1 |
| Diffie Hellman grubu (DH grubu) | 2 | 2 | 2 |
| Yaşam süresi | 28.800 saniye | 28.800 saniye | 28.800 saniye |
| Veri boyutu | 4 GB | 4 GB | 4 GB |


#### <a name="phase-2-proposals"></a>2\. Aşama teklifleri 

| Parametre | Teklif 1 | Teklif 2 | Teklif 3 |
|-----------|------------|------------|------------|
| Şifreleme | AES 128 | AES 256 | AES 256 |
| Karma algoritması| SHA 256 | SHA 256 | SHA 1 |
| Mükemmel Forward Secrecy grubu (PFS grubu) | None | Yok. | Yok. |
| Yaşam süresi | 1800 saniye | 1800 saniye | 1800 saniye |
| Veri boyutu | 4 GB | 4 GB | 4 GB |

## <a name="point-to-site-vpn-gateway"></a>Noktadan siteye VPN ağ geçidi

Noktadan siteye VPN CloudSimple bölge ağ ve bir istemci bilgisayar arasında şifrelenmiş trafik göndermek için kullanılır.  Noktadan siteye VPN, özel bulut vCenter ve iş yükü Vm'lerinden dahil olmak üzere, özel bulut ağ erişmek için kolay bir yoludur.  Özel bulut için uzaktan bağlanıyorsanız, noktadan siteye VPN bağlantısı kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* [VPN ağ geçidi ayarlama](https://docs.azure.cloudsimple.com/vpn-gateway/)