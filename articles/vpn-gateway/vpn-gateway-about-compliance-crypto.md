---
title: Azure VPN ağ geçitleri ve şifreleme gereksinimleri hakkında | Microsoft Docs
description: Bu makalede Azure VPN ağ geçitleri ve şifreleme gereksinimleri açıklanır
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
ms.openlocfilehash: 060e647badcc3bad7b44d7cef3530c36b8ecdf57
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60648688"
---
# <a name="about-cryptographic-requirements-and-azure-vpn-gateways"></a>Şifreleme gereksinimleri ve Azure VPN ağ geçitleri hakkında

Bu makalede, S2S VPN tünelinde şirketler arası hem de azure'da VNet-VNet bağlantıları için şifreleme gereksinimlerinizi karşılamak için Azure VPN ağ geçitleri nasıl yapılandırabileceğiniz açıklanmaktadır. 

## <a name="about-ipsec-and-ike-policy-parameters-for-azure-vpn-gateways"></a>IPSec ve IKE ilke parametreleri hakkında Azure VPN ağ geçitleri
IPSec ve IKE protokolü standart çeşitli birleşimler çok çeşitli şifreleme algoritmalarını destekler. Müşteriler, belirli bir şifreleme algoritmaları ve parametrelerle birleşimi istemeyen, Azure VPN ağ geçitlerine varsayılan teklifleri kümesi kullanın. Varsayılan ilke kümelerin, çok çeşitli üçüncü taraf VPN cihazları varsayılan yapılandırmaları ile birlikte çalışabilirlik en üst düzeye çıkarmak için seçilmiştir. Sonuç olarak, ilkeleri ve teklifler sayısını kullanılabilir şifreleme algoritmaları ve anahtar güçleriyle tüm olası eşleştirme birleşimlerini kapsamamaktadır.

Azure VPN ağ geçidi için varsayılan ilke belgeye listelenir: [VPN cihazları ve siteden siteye VPN Gateway bağlantıları için IPSec/IKE parametreleri hakkında](vpn-gateway-about-vpn-devices.md).

## <a name="cryptographic-requirements"></a>Şifreleme gereksinimleri
Belirli şifreleme algoritma veya parametre gerektiren iletişimleri için genellikle uyumluluk veya güvenlik gereksinimleri nedeniyle müşterileri artık, Azure VPN ağ geçitleri, özel şifreleme ile özel bir IPSec/IKE İlkesi kullanmak için yapılandırabilirsiniz algoritmalar ve anahtar güçleriyle yerine Azure varsayılan ilkesi ayarlar.

Örneğin, müşterilerin gibi Grup 14 (2048-bit), Grup 24 (2048 bit MODP grubu) ya da ECP (Eliptik IKE kullanılmak üzere daha güçlü grupları belirtmeniz gerekebilir ancak Azure VPN ağ geçitlerinde Ikev2 ana mod ilkeler Diffie-Hellman grubu 2 (1024 bit), yalnızca kullanma Eğri grupları) 256 veya 384 bit (sırasıyla 19 ile 20 grubu, Grup). Benzer gereksinimleri de IPSec hızlı modunu ilkeleri için geçerlidir.

## <a name="custom-ipsecike-policy-with-azure-vpn-gateways"></a>Azure VPN gateways ile özel IPSec/IKE İlkesi
Azure VPN ağ geçitleri, her bağlantı, özel IPSec/IKE İlkesi artık desteklenmektedir. Siteden siteye veya VNet-VNet bağlantısı için şifreleme algoritmaları belirli bir birleşimi IPSec ve IKE için istenen anahtar gücü ile aşağıdaki örnekte gösterildiği gibi seçebilirsiniz:

![IPSec IKE İlkesi](./media/vpn-gateway-about-compliance-crypto/ipsecikepolicy.png)

Bir IPSec/IKE ilkesi oluşturun ve yeni veya mevcut bir bağlantı için geçerlidir. 

### <a name="workflow"></a>İş akışı

1. Sanal ağlar, VPN ağ geçitleri veya diğer nasıl yapılır belgeleri açıklandığı gibi bağlantı topolojiniz için yerel ağ geçitleri oluşturma
2. Bir IPSec/IKE ilkesi oluşturma
3. Bir S2S veya VNet-VNet bağlantı oluşturduğunuzda ilke uygulayabilirsiniz.
4. Bağlantıyı zaten oluşturduysanız, uygulama veya mevcut bir bağlantı ilkesini güncelleştirme


## <a name="ipsecike-policy-faq"></a>IPSec/IKE İlkesi hakkında SSS

[!INCLUDE [vpn-gateway-ipsecikepolicy-faq-include](../../includes/vpn-gateway-faq-ipsecikepolicy-include.md)]


## <a name="next-steps"></a>Sonraki adımlar
Bkz: [yapılandırma IPSec/IKE İlkesi](vpn-gateway-ipsecikepolicy-rm-powershell.md) bir bağlantıda özel IPSec/IKE ilke yapılandırma hakkında adım adım yönergeler.

Ayrıca bkz: [birden çok ilke tabanlı VPN cihazını bağlama](vpn-gateway-connect-multiple-policybased-rm-ps.md) UsePolicyBasedTrafficSelectors seçeneği hakkında daha fazla bilgi için.