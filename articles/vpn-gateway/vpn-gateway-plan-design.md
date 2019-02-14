---
title: 'Planlama ve tasarım şirketler arası bağlantılar için: Azure VPN ağ geçidi | Microsoft Docs'
description: Şirketler arası ve karma VNet-VNet bağlantıları için VPN Gateway planlama ve tasarım hakkında bilgi edinin
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: article
ms.date: 07/27/2017
ms.author: yushwang
ms.openlocfilehash: 7802061ba09a30ca34ed3804ace846118c5edb9b
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56235379"
---
# <a name="planning-and-design-for-vpn-gateway"></a>VPN Gateway için planlama ve tasarım

Planlama ve tasarlama şirketler arası ve VNet-VNet yapılandırmaları, basit veya karmaşık ağ gereksinimlerinize bağlı olarak olabilir. Bu makalede temel planlama ve tasarım konuları açıklanmaktadır.

## <a name="planning"></a>Planlama

### <a name="compare"></a>Şirketler arası bağlantı seçenekleri

Şirket içi sitelerinize güvenli bir şekilde bir sanal ağa bağlanmak istiyorsanız, bunu yapmak için üç farklı yolu vardır: Siteye, noktadan siteye ve ExpressRoute. Kullanılabilir farklı şirketler arası bağlantılar karşılaştırın. Belirlediğiniz seçeneğe gibi çeşitli etmenlere bağlı olabilir:

* Çözümünüz ne tür bir verimlilik gerektiriyor?
* Güvenli VPN aracılığıyla ortak Internet üzerinden mi, yoksa özel bir bağlantı üzerinden mi iletişim kurmak istiyorsunuz?
* Kullanıma uygun bir genel bir IP adresiniz var mı?
* VPN cihazı kullanmayı planlıyor musunuz? Bu uyumlu bir VPN cihazı mı?
* Sadece bir kaç bilgisayarla mı bağlanıyorsunuz yoksa siteniz için kalıcı bir bağlantı istiyor musunuz?
* Oluşturmak istediğiniz çözüm için ne tür bir VPN ağ geçidi gerekli?
* Hangi ağ geçidi SKU'sunu kullanmanız gerekir?

### <a name="planningtable"></a>Planlama tablosu

Aşağıdaki tablo çözümünüz için en iyi bağlantı seçeneğine karar vermenize yardımcı olabilir.

[!INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]

### <a name="gwsku"></a>Ağ Geçidi SKU'ları

[!INCLUDE [vpn-gateway-table-gwtype-aggtput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

### <a name="wf"></a>İş akışı

Aşağıdaki listede bulut bağlantısı için genel iş akışını açıklar:

1. Tasarım bağlantısı topolojinizi planlama ve bağlanmak istediğiniz tüm ağ adresi alanlarını listeleyin.
2. Bir Azure sanal ağı oluşturun. 
3. Sanal ağ için bir VPN ağ geçidi oluşturun.
4. Oluşturun ve şirket içi ağlara veya diğer sanal ağlara bağlantılar (gereken şekilde) yapılandırın.
5. Oluşturma ve noktadan siteye bağlantı (gereken şekilde), Azure VPN ağ geçidi için yapılandırın.

## <a name="design"></a>Tasarım
### <a name="topologies"></a>Bağlantı topolojileri

Başlangıç diyagramlara bakarak [VPN Gateway hakkında](vpn-gateway-about-vpngateways.md) makalesi. Makale temel çizimler, dağıtım modelleri için her topoloji ve yapılandırmanızı dağıtmak için kullanabileceğiniz kullanılabilen dağıtım araçları içerir.

### <a name="designbasics"></a>Tasarım temelleri

Aşağıdaki bölümlerde, VPN ağ geçidi temel açıklanmaktadır. 

#### <a name="servicelimits"></a>Ağ Hizmetleri limitleri

Görüntülemek için tablolar arasında kaydırma [Ağ Hizmetleri sınırları](../azure-subscription-service-limits.md#networking-limits). Listelenen limitler tasarımınızı etkileyebilir.

#### <a name="subnets"></a>Alt ağlar hakkında

Bir bağlantı oluşturulurken, alt ağ aralıklarını dikkate almanız gerekir. Çakışan alt ağ adres aralıkları olamaz. Çakışan bir alt ağ, bir sanal ağ ya da şirket içi konum başka bir konuma içeren aynı adres alanını içeren andır. Bu, Azure IP için kullanabileceğiniz bir aralığı ayırma işlemini koordine ağ mühendisleri yerel şirket içi ağlarınız için gereken alanı/alt ağlar adresleme anlamına gelir. Yerel şirket içi ağdaki kullanılmayan adres alanı gerekir.

VNet-VNet bağlantıları ile çalışırken çakışan alt ağlarınız önleme da önemlidir. VNet-VNet bağlantılarında çakışan alt ağlarınızı ve hem gönderen hem de hedef sanal ağlar içinde bir IP adresi varsa başarısız. Azure veri, hedef adresi gönderen sanal ağın bir parçası olduğundan bir sanal ağa yönlendirme yapamazsınız.

VPN ağ geçitleri, bir ağ geçidi alt ağı adlı belirli bir alt ağ gerekir. Tüm ağ geçidi alt ağlarının düzgün çalışması için GatewaySubnet şeklinde adlandırılması gerekir. Farklı bir ad ağ geçidi alt ağınızı adlandırın değil emin olun ve sanal makineleri veya diğer her şey ağ geçidi alt ağına dağıtmayın. Bkz: [ağ geçidi alt ağları](vpn-gateway-about-vpn-gateway-settings.md#gwsub).

#### <a name="local"></a>Yerel ağ geçitleri hakkında

Yerel ağ geçidi genellikle şirket içi konumunuz anlamına gelir. Klasik dağıtım modelinde, yerel ağ geçidi, bir yerel ağ alanı adlandırılır. Bir yerel ağ geçidi yapılandırdığınızda, şirket içi VPN cihazının genel IP adresini belirtin. bir ad verin ve şirket içi konumlarda adres öneklerini belirtirsiniz. Azure ağ trafiği için hedef adres öneklerine bakar, yerel ağ geçidi için belirttiğiniz yapılandırma bakar ve paketleri buna göre yönlendirir. Adres öneklerini gerektiği gibi değiştirebilirsiniz. Daha fazla bilgi için [yerel ağ geçitleri](vpn-gateway-about-vpn-gateway-settings.md#lng).

#### <a name="gwtype"></a>Ağ geçidi türleri hakkında

Topolojiniz için doğru ağ geçidi türünün seçilmesi önemlidir. Yanlış tür seçerseniz, ağ geçidi düzgün çalışmaz. Ağ geçidi türü, ağ geçidinin nasıl bağlandığını belirtir ve Resource Manager dağıtım modeli için gereken yapılandırma ayarıdır.

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

Her yapılandırma, belirli bir VPN türü gerektirir. Aynı VNet’e Siteden Siteye bağlantı ve Noktadan Siteye bağlantı oluşturma gibi iki yapılandırmayı birleştiriyorsanız, her iki bağlantı gereksinimini de karşılayan bir VPN türü kullanmalısınız.

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

Aşağıdaki tablolar, her bağlantı yapılandırması için eşlerken VPN türünü gösterir. Ağ geçidiniz için VPN türünü oluşturmak istediğiniz yapılandırma eşleştiğinden emin olun. 

[!INCLUDE [vpn-gateway-table-vpntype](../../includes/vpn-gateway-table-vpntype-include.md)]

### <a name="devices"></a>Siteden siteye bağlantıları için VPN cihazları

Dağıtım modeli ne olursa olsun, siteden siteye bağlantı yapılandırmak için aşağıdaki öğeler gerekir:

* Azure VPN ağ geçitleri ile uyumlu bir VPN cihazı
* Bir genel kullanıma yönelik IPv4 IP adresi bir NAT'nin arkasında değildir

VPN Cihazınızı yapılandırma deneyimi varsa veya cihaz için yapılandırabileceğiniz biri gerekir.

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

### <a name="forcedtunnel"></a>Zorlamalı tünel yönlendirme göz önünde bulundurun

Çoğu yapılandırma için zorlamalı tünel yapılandırabilirsiniz. Zorlamalı tünel yeniden yönlendirme veya tüm İnternet'e bağlı trafiği, inceleme ve denetim için bir siteden siteye VPN tüneli aracılığıyla şirket içi konumunuza geri "force" sağlar. Bu, çoğu kurumsal BT için kritik güvenlik gereksinimdir ilkeleri. 

Zorlamalı tünel olmadan, Internet'e bağlı trafik azure'daki sanal makinelerinize her zaman Azure ağ altyapısından doğrudan inceleme veya trafiği denetlemek için izin verme seçeneği olmadan Internet'e gerçekleşmedikçe. Yetkisiz Internet erişimi, bilgilerin açıklanması veya diğer tür güvenlik ihlallerini olası neden olabilir.

Her iki dağıtım modeli ve farklı araçlar kullanarak zorlamalı Tünel bağlantısı yapılandırılabilir. Daha fazla bilgi için [zorlamalı tüneli yapılandırma](vpn-gateway-forced-tunneling-rm.md).

**Zorlamalı tünel diyagramı**

![Azure VPN ağ geçidi zorlamalı tünel diyagramı](./media/vpn-gateway-plan-design/forced-tunneling-diagram.png)

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [VPN Gateway SSS](vpn-gateway-vpn-faq.md) ve [VPN Gateway hakkında](vpn-gateway-about-vpngateways.md) makaleleri ile tasarımınızı yardımcı olacak daha fazla bilgi için.

Belirli bir ağ geçidi ayarları hakkında daha fazla bilgi için bkz. [VPN Gateway ayarları hakkında](vpn-gateway-about-vpn-gateway-settings.md).
