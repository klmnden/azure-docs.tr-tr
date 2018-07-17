---
title: include dosyası
description: include dosyası
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 07/10/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 5cb1360731eeabe4963330210ba6fe090f0e088a
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008828"
---
### <a name="what-is-the-difference-between-an-azure-virtual-network-gateway-vpn-gateway-and-an-azure-virtual-wan-vpngateway"></a>Azure sanal ağ geçidi (VPN Gateway) ile Azure Sanal WAN vpngateway arasında ne fark vardır?

Sanal WAN geniş ölçekli Siteden Siteye bağlantı sağlar; aktarım hızı, ölçeklendirilebilirlik ve kullanım kolaylığı için oluşturulmuştur. CPE dal cihazları otomatik olarak sağlanır ve Azure Sanal WAN'a bağlanır. Bu cihazlar büyüyen bir SD-WAN ve VPN iş ortakları ekosisteminden sağlanır.

### <a name="which-device-providers-virtual-wan-partners-are-supported-at-launch-time"></a>Başlangıçta hangi cihaz sağlayıcıları (Sanal WAN iş ortakları) desteklenir? 

Şu anda, Citrix ve Riverbed tümüyle otomatik Sanal WAN deneyimini desteklemektedir. Önümüzdeki aylarda Nokia Nuage, Palo Alto ve Checkpoint gibi daha fazla iş ortağı eklenecektir. Daha fazla bilgi için bkz. [Sanal WAN iş ortakları](https://aka.ms/virtualwan).

### <a name="am-i-required-to-use-a-preferred-partner-device"></a>Tercih edilen bir iş ortağı cihazını kullanmam gerekiyor mu?

Hayır. IKEv2 IPsec desteğinin Önizleme gereksinimlerini karşılayan, VPN özellikli herhangi bir cihaz kullanabilirsiniz.

### <a name="how-do-virtual-wan-partners-automate-connectivity-with-azure-virtual-wan"></a>Sanal WAN iş ortakları Azure Virtual WAN ile bağlantıyı nasıl otomatik hale getirir?

Yazılım tanımlı bağlantı çözümleri normalde dal cihazlarını yönetmek için bir denetleyici veya cihaz sağlama merkezi kullanır. Denetleyici, Azure Sanal WAN'a otomatik bağlantı sağlamak için Azure API'lerini kullanabilir. Bunun nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Sanal WAN iş ortağı otomasyonu](../articles/virtual-wan/virtual-wan-configure-automation-providers.md).

### <a name="does-virtual-wan-change-any-existing-connectivity-features"></a>Sanal WAN mevcut bağlantı özelliklerinden herhangi birini değiştiriyor mu?   

Mevcut Azure bağlantı özelliklerinde hiçbir değişiklik yoktur.

### <a name="are-there-new-resource-manager-resources-available-for-virtual-wan"></a>Sanal WAN için yeni Resource Manager kaynakları sağlanıyor mu?
  
Evet, Sanal WAN yeni Resource Manager kaynakları getirir. Daha fazla bilgi için bkz. [Genel Bakış](https://go.microsoft.com/fwlink/p/?LinkId=2004389).

### <a name="are-there-any-special-requirements-to-join-the-preview"></a>Önizleme'ye katılmak için özel gereksinimler söz konusu mu? 

Bir Azure Sanal WAN yapılandırabilmeniz için önce aboneliğinizi Önizleme'ye kaydetmelisiniz. Kaydetmek için, <azurevirtualwan@microsoft.com> adresine abonelik kimliğinizi içeren bir e-posta gönderin. Aboneliğiniz kaydedildiğinde siz de bir e-posta alırsınız. Aboneliğinizin kaydedildiğine dair onayı alana kadar, portalda Sanal WAN ile çalışamazsınız.

Dikkat edilmesi gerekenler:

* Önizleme, yalnızca Azure genel bölgeleriyle sınırlıdır.
* Sanal hub başına en çok 100 bağlantı desteklenir. Her bağlantı, etkin-etkin yapılandırmasına sahip 2 tünelden oluşur. Tüneller bir Azure Sanal Hub vpngateway'de sonlandırılır.
* Aşağıdaki durumlarda Önizleme'yi kullanmayı göz önünde bulundurun:
  * Sanal hub başına toplam 1 Gb/sn'den az bir bant genişliği dağıtmak istiyorsunuz.
  * Yol tabanlı yapılandırmayı ve IKEv2 IPsec bağlantısını destekleyen bir VPN cihazınız var.
  * SD-WAN kullanmayı ve başlangıç iş ortaklarının (Riverbed ve Citrix) dal cihazlarını çalıştırmayı incelemek istiyorsunuz.<br>or<br>Şirket içi cihazınızın yapılandırma yönetimini içeren, daldan dala ve daldan Azure'a bağlantı ayarlamak istiyorsunuz. (Kendi kendine yapılandırma)

### <a name="is-global-vnet-peering-supported-with-azure-virtual-wan"></a>Azure Sanal WAN ile Genel Sanal Ağ Eşleme destekleniyor mu? 

 Hayır.

### <a name="can-spoke-vnets-connected-to-a-virtual-hub-communicate-with-each-other"></a>Sanal hub'a bağlı uç sanal ağları birbiriyle iletişim kurabiliyor mu?

Evet. Sanal hub'a bağlı uçlar arasında doğrudan sanal ağ eşlemesi yapabilirsiniz. Daha fazla bilgi için bkz. [Sanal Ağ Eşlemesi](../articles/virtual-network/virtual-network-peering-overview.md).

### <a name="can-i-deploy-and-use-my-favorite-network-virtual-appliance-in-an-nva-vnet-with-azure-virtual-wan"></a>Azure Sanal WAN ile tercih ettiğim ağ sanal cihazını (bir NVA sanal ağı içinde) dağıtabilir ve kullanabilir miyim?

Evet, tercih ettiğiniz ağ sanal cihazı (NVA) sanal ağını Azure Sanal WAN'a bağlayabilirsiniz ama GA'da yer alacak hub'da yönlendirme özelliklerinin bulunması gerekir. NVA sanal ağına bağlı tüm uçların da sanal hub'a bağlı olması gerekir. 

### <a name="can-an-nva-vnet-have-a-virtual-network-gateway"></a>Bir NVA sanal ağının sanal ağ geçidi olabilir mi?

Hayır. NVA sanal ağı sanal hub'a bağlıysa, bunun sanal ağ geçidi olamaz. 

### <a name="is-there-support-for-bgp"></a>BGP için destek var mı?

Evet, BGP desteklenir. NVA sanal ağından gelen yolların düzgün tanıtıldığından emin olmak için, NVA sanal ağına ve dolayısıyla da bir sanal hub'a bağlı olan uçların BGP'yi devre dışı bırakması gerekir. Buna ek olarak, uç sanal ağlarını sanal hub'a bağlayın.

### <a name="can-i-direct-traffic-using-udr-in-the-virtual-hub"></a>Sanal hub'da UDR kullanarak trafiği yönlendirebilir miyim?

UDR ve yönlendirme işlevi GA tarafından sağlanacaktır.

### <a name="is-there-any-licensing-or-pricing-information-for-virtual-wan"></a>Sanal WAN ile ilgili lisans ve fiyatlandırma bilgileri var mı?
 
Ek ücret alınmaz. Önizleme süresince güncel [Azure VPN ve çıkış fiyatlandırması](https://azure.microsoft.com/pricing/details/vpn-gateway/) geçerlidir.