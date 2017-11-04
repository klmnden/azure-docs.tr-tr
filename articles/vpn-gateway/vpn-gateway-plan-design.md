---
title: "Planlama ve tasarım şirket içi bağlantılar için: Azure VPN ağ geçidi | Microsoft Docs"
description: "VPN ağ geçidi planlama ve tasarım şirketler arası, karma ve VNet-VNet bağlantıları hakkında bilgi edinin"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: d5aaab83-4e74-4484-8bf0-cc465811e757
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/27/2017
ms.author: cherylmc
ms.openlocfilehash: 0ebc3ef4a64432e993dd6ed69766bb64544fe433
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="planning-and-design-for-vpn-gateway"></a>VPN Gateway için planlama ve tasarım

Planlama ve tasarlama şirketler arası ve VNet-VNet yapılandırmaları basit ya da ağ gereksinimlerinize bağlı olarak karmaşık olabilir. Bu makalede temel planlama ve tasarım konuları anlatılmaktadır.

## <a name="planning"></a>Planlama

### <a name="compare"></a>Şirketler arası bağlantı seçenekleri

Şirket içi siteleriniz güvenli bir sanal ağa bağlanmak istiyorsanız, bunu yapmak için üç farklı yolu vardır: siteden siteye noktadan siteye ve ExpressRoute. Kullanılabilir farklı şirket içi bağlantılar karşılaştırın. Belirlediğiniz seçenek gibi çeşitli etmenlere bağlı olabilir:

* Çözümünüz ne tür bir verimlilik gerektiriyor?
* Güvenli VPN aracılığıyla ortak Internet üzerinden mi, yoksa özel bir bağlantı üzerinden mi iletişim kurmak istiyorsunuz?
* Kullanıma uygun bir genel bir IP adresiniz var mı?
* VPN cihazı kullanmayı planlıyor musunuz? Bu uyumlu bir VPN cihazı mı?
* Sadece bir kaç bilgisayarla mı bağlanıyorsunuz yoksa siteniz için kalıcı bir bağlantı istiyor musunuz?
* Oluşturmak istediğiniz çözüm için ne tür bir VPN ağ geçidi gerekli?
* Hangi ağ geçidi SKU'su kullanmalısınız?

### <a name="planningtable"></a>Tablo planlama

Aşağıdaki tablo çözümünüz için en iyi bağlantı seçeneğine karar vermenize yardımcı olabilir.

[!INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]

### <a name="gwsku"></a>Ağ Geçidi SKU'ları

[!INCLUDE [vpn-gateway-table-gwtype-aggtput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

### <a name="wf"></a>İş akışı

Aşağıdaki listede, bulut bağlantı için genel iş akışını özetlenmektedir:

1. Tasarım bağlantısı topolojinizi planlamak ve bağlamak istediğiniz tüm ağ adresi alanlarını listeleyin.
2. Bir Azure sanal ağı oluşturun. 
3. Sanal ağ için bir VPN ağ geçidi oluşturun.
4. Oluşturun ve şirket içi ağlar ve diğer sanal ağlara bağlantılar (gerekirse) yapılandırın.
5. Oluşturun ve bir noktadan siteye bağlantı (gerektiğinde), Azure VPN ağ geçidi için yapılandırın.

## <a name="design"></a>Tasarım
### <a name="topologies"></a>Bağlantı topolojileri

Başlangıç diyagramlarında bakarak [VPN Gateway hakkında](vpn-gateway-about-vpngateways.md) makalesi. Makaleyi temel diyagramları, dağıtım modellerini her topolojisi ve yapılandırmanızı dağıtmak için kullanabileceğiniz kullanılabilir dağıtım araçlarını içerir.

### <a name="designbasics"></a>Tasarım temelleri

Aşağıdaki bölümlerde, VPN ağ geçidi temel kavramları açıklanmaktadır. 

#### <a name="servicelimits"></a>Ağ Hizmetleri sınırları

Görüntülemek için tablolar arasında kaydırma [Ağ Hizmetleri sınırları](../azure-subscription-service-limits.md#networking-limits). Listelenen sınırları tasarımınızı etkileyebilir.

#### <a name="subnets"></a>Alt ağlar hakkında

Bağlantıları oluştururken, alt ağ aralıklarını dikkate almanız gerekir. Alt ağ adres aralıklarını çakışan sahip olamaz. Çakışan bir alt ağ, bir sanal ağ ya da şirket içi konumu başka bir konuma içeren aynı adres alanı içeren durumdur. Bu, ağ mühendisleri çıkışı için Azure IP kullanmanız için bir aralığı ayırması yerel şirket içi ağlarınız için gereken alanı/alt adresleme anlamına gelir. Yerel şirket içi ağda kullanılmayan adres alanı gerekir.

VNet-VNet bağlantıları ile çalışırken çakışan alt ağlarınız önleme da önemlidir. Ağlarınızı çakışmayan bir IP adresi hem gönderme hem de hedef sanal ağlar bulunuyorsa, VNet-VNet bağlantı başarısız. Hedef adres gönderen VNet parçası olduğundan azure verileri diğer Vnet'e yönlendiremezsiniz.

VPN ağ geçitleri bir ağ geçidi alt ağı olarak adlandırılan belirli bir alt gerektirir. Tüm ağ geçidi alt ağlarının düzgün çalışması için GatewaySubnet şeklinde adlandırılması gerekir. Ağ geçidi alt ağınızı farklı bir ad adını değil verdiğinizden emin olun ve VM'ler veya başka bir şey ağ geçidi alt ağına dağıtmayın. Bkz: [ağ geçidi alt ağları](vpn-gateway-about-vpn-gateway-settings.md#gwsub).

#### <a name="local"></a>Yerel ağ geçitleri hakkında

Yerel ağ geçidi genellikle şirket içi konumunuz anlamına gelir. Klasik dağıtım modelinde, yerel ağ geçidi, bir yerel ağ sitesi olarak adlandırılır. Bir yerel ağ geçidi yapılandırdığınızda, şirket içi VPN cihazının genel IP adresini belirtin bir ad verin ve şirket içi konumdaysa adres öneklerini belirtirsiniz. Azure ağ trafiği için hedef adres öneklerine bakar, yerel ağ geçidi için belirttiğiniz yapılandırma bakar ve paketleri buna uygun şekilde yönlendirir. Adres öneklerini gerektiği gibi değiştirebilirsiniz. Daha fazla bilgi için bkz: [yerel ağ geçitleri](vpn-gateway-about-vpn-gateway-settings.md#lng).

#### <a name="gwtype"></a>Ağ geçidi türleri hakkında

Topolojiniz için doğru ağ geçidi türünü seçme önemlidir. Yanlış türde seçerseniz, ağ geçidi düzgün çalışmaz. Ağ geçidi türü, ağ geçidinin nasıl bağlandığını belirtir ve Resource Manager dağıtım modeli için gereken yapılandırma ayarıdır.

Ağ geçidi türleri şunlardır:

* VPN
* ExpressRoute

#### <a name="connectiontype"></a>Bağlantı türleri hakkında

Her bağlantı belirli bir bağlantı türü gerektirir. Bağlantı türleri şunlardır:

* IPsec
* Vnet2Vnet
* ExpressRoute
* VPNClient

#### <a name="vpntype"></a>VPN türleri hakkında

Her yapılandırma özel bir VPN türü gerektirir. Aynı VNet’e Siteden Siteye bağlantı ve Noktadan Siteye bağlantı oluşturma gibi iki yapılandırmayı birleştiriyorsanız, her iki bağlantı gereksinimini de karşılayan bir VPN türü kullanmalısınız.

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

Her bağlantı yapılandırması eşlemeleri olarak aşağıdaki tablolarda VPN türü gösterilmektedir. Oluşturmak istediğiniz yapılandırma ağ geçidiniz için VPN türü eşleştiğinden emin olun. 

[!INCLUDE [vpn-gateway-table-vpntype](../../includes/vpn-gateway-table-vpntype-include.md)]

### <a name="devices"></a>Siteden siteye bağlantıları için VPN cihazları

Dağıtım modeli, bağımsız olarak bir siteden siteye bağlantı yapılandırmak için aşağıdaki öğeleri gerekir:

* Azure VPN ağ geçitleri ile uyumlu bir VPN cihazı
* Bir genel kullanıma yönelik IPv4 IP adresi bir NAT'nin arkasında değildir

VPN Cihazınızı yapılandırmak deneyim sahibi veya cihaz için yapılandırabileceğiniz biri olması gerekir.

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

### <a name="forcedtunnel"></a>Zorlamalı tünel yönlendirme göz önünde bulundurun

Çoğu yapılandırmada zorlamalı tünel yapılandırabilirsiniz. Zorlamalı tünel yeniden yönlendirme veya "tüm Internet'e bağlı trafik, denetleme ve denetim için bir siteden siteye VPN tüneli aracılığıyla şirket içi konumunuza geri zorla" olanak sağlar. Bu, çoğu kurumsal BT için kritik güvenlik gereksinimdir ilkeleri. 

Zorlamalı tünel olmadan, Internet'e bağlı trafik, vm'lerden azure'da her zaman Azure ağ altyapısından doğrudan inceleme veya trafiği denetlemek için izin verme seçeneği olmadan Internet'e dizinlere. Yetkisiz Internet erişimine potansiyel olarak bilgilerin açığa çıkmasına veya diğer tür güvenlik ihlallerini yol açabilir.

Zorlamalı Tünel bağlantısı her iki dağıtım modeli ve farklı araçlar kullanılarak yapılandırılabilir. Daha fazla bilgi için bkz: [zorlamalı tüneli yapılandırma](vpn-gateway-forced-tunneling-rm.md).

**Zorlamalı tünel diyagramı**

![Azure VPN ağ geçidi zorlamalı tünel diyagramı](./media/vpn-gateway-plan-design/forced-tunneling-diagram.png)

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [VPN Gateway SSS](vpn-gateway-vpn-faq.md) ve [VPN Gateway hakkında](vpn-gateway-about-vpngateways.md) makaleleri tasarımınız ile yardımcı olmak daha fazla bilgi için.

Belirli ağ geçidi ayarları hakkında daha fazla bilgi için bkz: [hakkında VPN ağ geçidi ayarlarını](vpn-gateway-about-vpn-gateway-settings.md).