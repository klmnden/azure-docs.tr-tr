---
title: include dosyası
description: include dosyası
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 07/18/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: d059cab5668eef8d4dafc1442ca9749a7dcf8c9d
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39162523"
---
### <a name="what-is-the-difference-between-an-azure-virtual-network-gateway-vpn-gateway-and-an-azure-virtual-wan-vpngateway"></a>Azure sanal ağ geçidi (VPN Gateway) ile Azure Sanal WAN vpngateway arasında ne fark vardır?

Sanal WAN geniş ölçekli Siteden Siteye bağlantı sağlar; aktarım hızı, ölçeklendirilebilirlik ve kullanım kolaylığı için oluşturulmuştur. CPE dal cihazları otomatik olarak sağlanır ve Azure Sanal WAN'a bağlanır. Bu cihazlar büyüyen bir SD-WAN ve VPN iş ortakları ekosisteminden sağlanır.

### <a name="which-device-providers-virtual-wan-partners-are-supported-at-launch-time"></a>Başlangıçta hangi cihaz sağlayıcıları (Sanal WAN iş ortakları) desteklenir? 

Şu anda, Citrix ve Riverbed tümüyle otomatik Sanal WAN deneyimini desteklemektedir. Daha fazla bilgi için bkz. [Sanal WAN iş ortakları](https://aka.ms/virtualwan).

### <a name="am-i-required-to-use-a-preferred-partner-device"></a>Tercih edilen bir iş ortağı cihazını kullanmam gerekiyor mu?

Hayır. IKEv2 IPsec desteğinin Önizleme gereksinimlerini karşılayan, VPN özellikli herhangi bir cihaz kullanabilirsiniz.

### <a name="how-do-virtual-wan-partners-automate-connectivity-with-azure-virtual-wan"></a>Sanal WAN iş ortakları Azure Virtual WAN ile bağlantıyı nasıl otomatik hale getirir?

Yazılım tanımlı bağlantı çözümleri normalde dal cihazlarını yönetmek için bir denetleyici veya cihaz sağlama merkezi kullanır. Denetleyici, Azure Sanal WAN'a otomatik bağlantı sağlamak için Azure API'lerini kullanabilir. Daha fazla bilgi için bkz. [Sanal WAN iş ortağı otomasyonu](../articles/virtual-wan/virtual-wan-configure-automation-providers.md).

### <a name="does-virtual-wan-change-any-existing-connectivity-features"></a>Sanal WAN mevcut bağlantı özelliklerinden herhangi birini değiştiriyor mu?   

Mevcut Azure bağlantı özelliklerinde hiçbir değişiklik yoktur.

### <a name="are-there-new-resource-manager-resources-available-for-virtual-wan"></a>Sanal WAN için yeni Resource Manager kaynakları sağlanıyor mu?
  
Evet, Sanal WAN yeni Resource Manager kaynakları getirir. Daha fazla bilgi için bkz. [Genel Bakış](https://go.microsoft.com/fwlink/p/?LinkId=2004389).

### <a name="are-there-any-special-requirements-to-join-the-preview"></a>Önizleme'ye katılmak için özel gereksinimler söz konusu mu? 

Bir Azure Sanal WAN yapılandırabilmeniz için önce aboneliğinizi Önizleme'ye kaydetmelisiniz. Kaydetmek için, <azurevirtualwan@microsoft.com> adresine abonelik kimliğinizi içeren bir e-posta gönderin. Aboneliğiniz kaydedildiğinde siz de bir e-posta alırsınız. Aboneliğinizin kaydedildiğine dair onayı alana kadar, portalda Sanal WAN ile çalışamazsınız.

Dikkat edilmesi gerekenler:

* Önizleme, yalnızca Azure genel bölgeleriyle sınırlıdır.
* Sanal hub başına en çok 100 bağlantı desteklenir. Her bağlantı, etkin-etkin yapılandırmasına sahip iki tünelden oluşur. Tüneller bir Azure Sanal Hub vpngateway'de sonlandırılır.
* Aşağıdaki durumlarda Önizleme'yi kullanmayı göz önünde bulundurun:
  * Sanal hub başına toplam 1 Gb/sn'den az bir bant genişliği dağıtmak istiyorsunuz.
  * Yol tabanlı yapılandırmayı ve IKEv2 IPsec bağlantısını destekleyen bir VPN cihazınız var.
  * SD-WAN kullanmayı ve başlangıç iş ortaklarının (Riverbed ve Citrix) dal cihazlarını çalıştırmayı incelemek istiyorsunuz.<br>or<br>Şirket içi cihazınızın yapılandırma yönetimini içeren, daldan dala ve daldan Azure'a bağlantı ayarlamak istiyorsunuz. (Kendi kendine yapılandırma)

### <a name="is-global-vnet-peering-supported-with-azure-virtual-wan"></a>Azure Sanal WAN ile Genel Sanal Ağ Eşleme destekleniyor mu? 

 Hayır.

### <a name="can-spoke-vnets-connected-to-a-virtual-hub-communicate-with-each-other"></a>Sanal hub'a bağlı uç sanal ağları birbiriyle iletişim kurabiliyor mu?

Evet. Sanal hub'a bağlı uçlar arasında doğrudan sanal ağ eşlemesi yapabilirsiniz. Daha fazla bilgi için bkz. [Sanal Ağ Eşlemesi](../articles/virtual-network/virtual-network-peering-overview.md).

### <a name="can-i-deploy-and-use-my-favorite-network-virtual-appliance-in-an-nva-vnet-with-azure-virtual-wan"></a>Azure Sanal WAN ile tercih ettiğim ağ sanal cihazını (bir NVA sanal ağı içinde) dağıtabilir ve kullanabilir miyim?

Evet, tercih ettiğiniz ağ sanal cihazı (NVA) sanal ağını Azure Sanal WAN'a bağlayabilirsiniz ama yol haritamızda yer alacak merkezde yönlendirme özelliklerinin bulunması gerekir. NVA sanal ağına bağlı tüm uçların da sanal hub'a bağlı olması gerekir. 

### <a name="can-an-nva-vnet-have-a-virtual-network-gateway"></a>Bir NVA sanal ağının sanal ağ geçidi olabilir mi?

Hayır. NVA sanal ağı sanal hub'a bağlıysa, bunun sanal ağ geçidi olamaz. 

### <a name="is-there-support-for-bgp"></a>BGP için destek var mı?

Evet, BGP desteklenir. NVA sanal ağından gelen yolların düzgün tanıtıldığından emin olmak için, NVA sanal ağına ve dolayısıyla da bir sanal hub'a bağlı olan uçların BGP'yi devre dışı bırakması gerekir. Buna ek olarak, uç sanal ağlarını sanal hub'a bağlayın.

### <a name="can-i-direct-traffic-using-udr-in-the-virtual-hub"></a>Sanal hub'da UDR kullanarak trafiği yönlendirebilir miyim?

Bu, yol haritamızda yer alır. Bizi izlemeye devam edin!

### <a name="is-there-any-licensing-or-pricing-information-for-virtual-wan"></a>Sanal WAN ile ilgili lisans ve fiyatlandırma bilgileri var mı?
 
Önizleme sırasında ek ücret alınmaz. Önizleme süresince güncel [Azure VPN ve çıkış fiyatlandırması](https://azure.microsoft.com/pricing/details/vpn-gateway/) geçerlidir.

### <a name="how-do-new-partners-that-are-not-listed-in-your-launch-partner-list-get-onboarded"></a>İlk iş ortağı listenizde yer almayan yeni iş ortakları nasıl eklenir?

azurevirtualwan@microsoft.com adresine e-posta gönderin. İdeal bir iş ortağı, IKEv2 IPSec bağlantısına yönelik sağlanabilen bir cihaza sahip olandır.

### <a name="is-it-possible-to-construct-azure-virtual-wan-with-a-resource-manager-template"></a>Azure Sanal WAN’ı bir Resource Manager şablonu ile yapılandırmak mümkün mü?

Üzerinde çalışıyoruz. Şu anda hizmet, REST ve Portal temellidir.

### <a name="is-branch-to-branch-connectivity-allowed-in-virtual-wan"></a>Sanal WAN’da daldan dala bağlantıya izin verilir mi?

Evet, daldan dala bağlantı Sanal WAN’da kullanılabilir.

### <a name="does-branch-to-branch-traffic-traverse-through-the-azure-virtual-wan"></a>Daldan dala trafik Azure Sanal WAN üzerinden geçiş yapar mı?

Evet.

### <a name="how-is-virtual-wan-different-from-the-existing-azure-virtual-network-gateway"></a>Sanal WAN, mevcut Azure Sanal Ağ’dan nasıl farklıdır?

Sanal Ağ Geçidi VPN’i 30 tünelle sınırlıdır. Bağlantılar için, büyük ölçekli VPN’lere yönelik Sanal WAN kullanmanız gerekir. Genel önizlemede bu, merkezde 1 Gbps ile 100 dal bağlantısıyla sınırlıdır.

### <a name="does-this-virtual-wan-require-expressroute-from-each-site"></a>Bu Sanal WAN her siteden ExpressRoute alınmasını gerektirir mi?

Hayır, Sanal WAN her siteden ExpressRoute alınmasını gerektirmez. Cihazdan Azure Sanal WAN merkezine kurulan İnternet bağlantıları aracılığıyla standart IPsec siteden siteye bağlantı kullanır.

### <a name="is-there-a-network-throughput-limit-when-using-azure-virtual-wan"></a>Azure Sanal WAN kullanırken bir ağ aktarım hızı sınırı var mı?

Genel önizlemede dal sayısı merkez/bölge başına 100 bağlantı ve merkezde toplam 1 G ile sınırlıdır.

### <a name="does-virtual-wan-allow-the-on-premises-device-to-utilize-multiple-isps-in-parallel-or-is-it-always-a-single-vpn-tunnel"></a>Sanal WAN, şirket içi cihazın paralel olarak birden fazla ISS’den yararlanmasına izin verir mi, yoksa her zaman tek bir VPN tüneli mi var?

Evet, dal cihazına bağlı olarak tek bir daldan etkin-etkin tünelleriniz (2 tünel = 1 Azure Sanal WAN bağlantısı) olabilir.

### <a name="how-is-traffic-is-routed-on-the-azure-backbone"></a>Azure temelinde trafik nasıl yönlendirilir?

Trafik şu düzeni izler: dal cihazı ->ISS->Microsoft Edge->Microsoft DC->Microsoft Edge->ISS->dal cihazı.

### <a name="in-this-model-what-do-you-need-at-each-site-just-an-internet-connection"></a>Bu modelde her sitede neye ihtiyacınız var? Yalnızca interneti bağlantısına mı?

Evet. İnternet bağlantısı ve tercihen tümleşik iş ortaklarımızdan bir fiziksel cihaz. Tabii Azure bağlantısını ve yapılandırmasını tercih ettiğiniz cihazdan el ile yönetmek istemiyorsanız.