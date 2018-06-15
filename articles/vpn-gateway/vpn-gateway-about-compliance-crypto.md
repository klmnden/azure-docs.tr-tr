---
title: Şifreleme gereksinimleri ve Azure VPN ağ geçitleri hakkında | Microsoft Docs
description: Bu makalede, şifreleme gereksinimleri ve Azure VPN ağ geçitleri açıklanmaktadır
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: ''
tags: azure-resource-manager
ms.assetid: 238cd9b3-f1ce-4341-b18e-7390935604fa
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/22/2017
ms.author: yushwang
ms.openlocfilehash: d2f3da47f1d4eebe1b81964790ff6612dd78155d
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
ms.locfileid: "23884254"
---
# <a name="about-cryptographic-requirements-and-azure-vpn-gateways"></a>Şifreleme gereksinimleri ve Azure VPN ağ geçitleri hakkında

Bu makalede, Azure VPN ağ geçitleri, hem şirket içi S2S VPN tünelleri hem de azure'daki VNet-VNet bağlantıları için şifreleme gereksinimlerinizi karşılamak için nasıl yapılandırabileceğiniz anlatılmaktadır. 

## <a name="about-ipsec-and-ike-policy-parameters-for-azure-vpn-gateways"></a>IPSec ve IKE İlkesi parametreleri hakkında Azure VPN ağ geçitleri için
IPSec ve IKE protokolü standart çeşitli bileşimlerde çok çeşitli şifreleme algoritmalarını destekler. Müşteriler, şifreleme algoritmaları ve parametreleri belirli bir bileşimini isteme, Azure VPN ağ geçitleri varsayılan teklifleri kümesi kullanın. Varsayılan ilkeyi ayarlar, çok çeşitli üçüncü taraf VPN aygıtları varsayılan yapılandırmaları ile birlikte çalışabilirlik en üst düzeye çıkarmak için seçilmiştir. Sonuç olarak, ilkeler ve tekliflerini sayısı kullanılabilir şifreleme algoritmaları ve anahtar gücü tüm olası birleşimlerini ele olamaz.

Azure VPN ağ geçidi belgesinde listelenen için ayarlanan varsayılan ilkesi: [VPN cihazları hakkında ve siteden siteye VPN Gateway bağlantıları için IPSec/IKE parametreleri](vpn-gateway-about-vpn-devices.md).

## <a name="cryptographic-requirements"></a>Şifreleme gereksinimleri
Belirli şifreleme algoritmalarının veya parametreler gerektiren iletişimleri için genellikle uyumluluk veya güvenlik gereksinimleri nedeniyle, müşteriler artık kendi Azure VPN ağ geçitleri, belirli şifreleme algoritmaları ve anahtar gücü ile özel bir IPSec/IKE ilkesini kullanmak için yapılandırabilirsiniz yerine Azure varsayılan ilkeyi ayarlar.

Örneğin, müşteriler Grup 14 (2048 bit), Grup 24 (2048 bit MODP grup) ya da ECP (Eliptik Eğri gruplar) gibi IKE kullanılacak daha güçlü gruplarını belirtmeniz gerekebilir ancak Azure VPN ağ geçitleri için Ikev2 ana mod ilkeleri yalnızca Diffie-Hellman grubu 2 (1024 bit) kullanan 256 veya 384 bit (sırasıyla 19 ve Grup 20, Grup). Benzer gereksinimleri de IPSec hızlı mod ilkelerini uygula.

## <a name="custom-ipsecike-policy-with-azure-vpn-gateways"></a>Azure VPN ağ geçitleri ile özel IPSec/IKE İlkesi
Azure VPN ağ geçitleri artık bağlantı başına, özel IPSec/IKE ilke destekler. Siteden siteye veya VNet-VNet bağlantısı için şifreleme algoritmaları belirli bir bileşimini IPSec ve IKE için istenen anahtar gücü ile aşağıdaki örnekte gösterildiği gibi seçebilirsiniz:

![ipsec-ike-policy](./media/vpn-gateway-about-compliance-crypto/ipsecikepolicy.png)

Bir IPSec/IKE ilkesi oluşturun ve yeni veya var olan bağlantı uygulayın. 

### <a name="workflow"></a>İş akışı

1. Sanal ağlar, VPN ağ geçitleri veya yerel ağ geçitleri diğer yapılır belgelerinde açıklandığı gibi bağlantı topolojiniz için oluşturma
2. Bir IPSec/IKE ilkesi oluşturun
3. S2S veya VNet-VNet bağlantısı oluşturduğunuzda ilkesi uygulayabilir.
4. Bağlantıyı zaten oluşturduysanız, uygulama veya mevcut bir bağlantı İlkesi güncelleştir


## <a name="ipsecike-policy-faq"></a>IPSec/IKE İlkesi hakkında SSS

[!INCLUDE [vpn-gateway-ipsecikepolicy-faq-include](../../includes/vpn-gateway-faq-ipsecikepolicy-include.md)]


## <a name="next-steps"></a>Sonraki adımlar
Bkz: [IPSec/IKE yapılandırma İlkesi](vpn-gateway-ipsecikepolicy-rm-powershell.md) bir bağlantıda özel IPSec/IKE ilkesini yapılandırma hakkında adım adım yönergeler için.

Ayrıca bkz. [birden çok ilke tabanlı VPN aygıtlarını bağlamak](vpn-gateway-connect-multiple-policybased-rm-ps.md) UsePolicyBasedTrafficSelectors seçenek hakkında daha fazla bilgi edinmek için.