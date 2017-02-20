---
title: Azure VPN Gateway SSS | Microsoft Docs
description: "VPN Ağ Geçidi SSS. Microsoft Azure Sanal Ağ şirket içi ve dışı bağlantılar, karma yapılandırma bağlantıları ve VPN Ağ Geçitleri hakkında SSS."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
ms.assetid: 6ce36765-250e-444b-bfc7-5f9ec7ce0742
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 15ac382f72cab455246ffcc05f08c8aba5876c8f
ms.openlocfilehash: c90bb4f41661aedec2bde53abe035fe9bcc80320


---
# <a name="vpn-gateway-faq"></a>VPN Gateway SSS
## <a name="connecting-to-virtual-networks"></a>Sanal ağlara bağlanma
### <a name="can-i-connect-virtual-networks-in-different-azure-regions"></a>Farklı Azure bölgelerinde sanal ağlara bağlanabilir miyim?
Evet. Aslında, hiçbir bölge kısıtlaması yoktur. Bir sanal ağ aynı bölgedeki veya farklı bir Azure bölgesindeki başka bir sanal ağa bağlanabilir. 

### <a name="can-i-connect-virtual-networks-in-different-subscriptions"></a>Farklı aboneliklerle sanal ağlara bağlanabilir miyim?
Evet.

### <a name="can-i-connect-to-multiple-sites-from-a-single-virtual-network"></a>Tek bir sanal ağdan birden çok siteye bağlanabilir miyim?
Windows PowerShell ve Azure REST API'lerini kullanarak birden çok siteye bağlanabilirsiniz. [Çok siteli ve VNet - VNet Bağlantı](#V2VMulti) SSS bölümüne bakın.

### <a name="what-are-my-cross-premises-connection-options"></a>Şirket içi ve dışı bağlantı seçeneklerim nelerdir?
Aşağıdaki şirket içi ve dışı bağlantılar desteklenmektedir:

* [Siteden Siteye](vpn-gateway-howto-site-to-site-resource-manager-portal.md) – IPsec üzerinden (IKE v1 ve IKE v2) VPN bağlantısı. Bu bağlantı türüne şirket içi bir VPN cihazı ya da RRAS gerekir.
* [Noktadan Siteye](vpn-gateway-howto-point-to-site-resource-manager-portal.md) – SSTP (Güvenli Yuva Tünel Protokolü) üzerinden VPN bağlantısı Bu bağlantıya VPN cihazı gerekmez.
* [VNet - VNet](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) – Bu türden bağlantı Siteden Siteye yapılandırmasıyla aynıdır. VNet - VNet, IPsec üzerinden (IKE v1 ve IKE v2) bir VPN bağlantısıdır. VPN cihazı gerekmez.
* [Çok Siteli](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) – Birden çok şirket içi sitenin bir sanal ağa bağlanmasına olanak sağlayan Siteden Siteye yapılandırmanın bir çeşididir.
* [ExpressRoute](../expressroute/expressroute-introduction.md) – ExpressRoute genel İnternet üzerinden değil WAN bağlantınızdan Azure’e doğrudan yapılan bir bağlantıdır. Daha fazla bilgi için bkz. [ExpressRoute’a Teknik Genel Bakış](../expressroute/expressroute-introduction.md) ve [ExpressRoute SSS](../expressroute/expressroute-faqs.md).

VPN ağ geçidi bağlantıları hakkında daha fazla bilgi için bkz. [VPN Gateway Hakkında](vpn-gateway-about-vpngateways.md).

### <a name="what-is-the-difference-between-a-site-to-site-connection-and-point-to-site"></a>Siteden Siteye bağlantı ve Noktadan Siteye bağlantı arasındaki fark nelerdir?
**Siteden Siteye** yapılandırmalar, şirket içi konumunuz ile Azure arasında yapılır. Diğer bir deyişle, şirket içinde yer alan bilgisayarlarla sanal makineler arasında ya da rota yapılandırmayı nasıl seçtiğinize bağlı olarak sanal ağınızdaki rol örneği ile bağlantı kurabilirsiniz. Her zaman kullanıma uygun şirket içi ve dışı bağlantı için müthiş bir seçenek; karma yapılandırmalar için de çok uygundur. Bu tür bir bağlantı; ağınıza ucuna dağıtılmış olması gereken IPsec VPN uygulamasına bağlıdır (donanım veya yazılım aracı). Bu tür bir bağlantı oluşturmak için gerekli VPN donanımına ve dışarıya yönelik bir IPv4 adresine sahip olmanız gerekir.

**Noktadan Siteye** yapılandırmalar, sanal ağınızda bulunan her yerden her şeye tek bir bilgisayardan bağlanmanızı sağlar. Windows yerleşik VPN istemcisi kullanır. Noktadan Siteye yapılandırmasının bir parçası olarak, bir sertifika ve bir VPN istemci yapılandırma paketi yüklerseniz; bu pakette, bilgisayarınızın sanal ağda herhangi bir sanal makineye veya rol örneğine bağlanmasını sağlayan ayarlar bulunur. Şirket içi olmayan sanal ağa bağlanmak istediğinizde çok yararlıdır. Her ikisi de Siteden Siteye bağlantısına gereken VPN donanımına veya dışarıya dönük IPv4 adresine erişiminiz yoksa bu da iyi bir seçenektir.

Siteden Siteye bağlantınızı ağ geçidinizi için rota tabanlı VPN türü kullanarak oluşturmanız kaydıyla, sanal ağınızı aynı anda hem Siteden Siteye, hem de Noktadan Siteye bağlanacak şekilde yapılandırabilirsiniz. Rota tabanlı VPN türlerine klasik dağıtım modelinde dinamik ağ geçitleri adı verilir.

## <a name="virtual-network-gateways"></a>Sanal ağ geçitleri

### <a name="is-a-vpn-gateway-a-virtual-network-gateway"></a>VPN ağ geçidi bir sanal ağ geçidi midir?
VPN ağ geçidi bir sanal ağ geçidi türüdür. VPN ağ geçidi, şifrelenmiş trafiği bir genel bağlantı üzerinden sanal ağınız ile şirket içi konumunuz arasında gönderir. Sanal ağlar arasında trafik göndermek için de VPN ağ geçidi kullanabilirsiniz. Bir VPN ağ geçidi oluştururken 'Vpn' -GatewayType değerini kullanın. Daha fazla bilgi için bkz. [VPN Gateway yapılandırma ayarları hakkında](vpn-gateway-about-vpn-gateway-settings.md).

### <a name="what-is-a-policy-based-static-routing-gateway"></a>İlke tabanlı (statik yönlendirme) ağ geçidi nedir?
İlke tabanlı ağ geçitleri, ilke tabanlı VPN'leri uygular. İlke temelli VPN'ler, şirket içi ağınızla Azure VNet'iniz arasında adres öneklerinin birleşimleri temelindeki IPsec tüneller üzerinden paketleri şifreler ve yönlendirirler. İlke (veya Trafik Seçici) çoğunlukla VPN yapılandırmasında bir erişim listesi olarak tanımlanır.

### <a name="what-is-a-route-based-dynamic-routing-gateway"></a>Rota tabanlı (dinamik yönlendirme) ağ geçidi nedir?
Rota tabanlı ağ geçitleri yol tabanlı VPN'leri uygular. Rota temelli VPN'ler, paketleri kendi ilgili arabirimlerine yönlendirmek için IP iletme veya yönlendirme tablosunda "yolları" seçeneğini kullanır. Bundan sonra tünel arabirimleri, paketleri tünellerin içinde veya dışında şifreler veya şifrelerini çözer. Rota temelli VPN’lerle ilgili ilke veya trafik seçici herhangi birinden herhangi birine (veya joker karakterler) olarak yapılandırılır.

### <a name="do-i-need-a-gatewaysubnet"></a>'GatewaySubnet' gerekli mi?
Evet. Ağ geçidi alt ağı, sanal ağ geçidi hizmetlerinin kullandığı IP adreslerini içerir. Bir sanal ağ geçidi yapılandırmak için sanal ağınıza yönelik bir ağ geçidi alt ağı oluşturmanız gerekir. Tüm ağ geçidi alt ağlarının düzgün çalışması için GatewaySubnet şeklinde adlandırılması gerekir. Ağ geçidi alt ağını başka şekilde adlandırmayın. VM’leri veya herhangi başka bir şeyi de ağ geçidi alt ağına dağıtmayın.

Ağ geçidi alt ağı oluştururken, alt ağın içerdiği IP adresi sayısını belirtirsiniz. Ağ geçidi alt ağındaki IP adresleri, ağ geçidi hizmetine ayrılır. Bazı yapılandırmalar, ağ geçidi hizmetlerine daha fazla IP adresi ayrılmasını gerektirir. Gelecekteki büyümeye ve meydana gelebilecek diğer yeni bağlantı yapılandırmalarına uyum sağlaması için ağ geçidi alt ağınızın yeterince IP adresi içerdiğinden emin olmanız gerekir. Bu nedenle, /29 kadar küçük bir ağ geçidi alt ağı oluşturmanız mümkün olsa da /28 veya daha büyük bir (/28, /27, /26 vb.) ağ geçidi alt ağı oluşturmanız önerilir. Oluşturmak istediğiniz yapılandırmanın gereksinimlerine bakın ve sahip olduğunuz ağ geçidi alt ağının bu gereksinimleri karşıladığından emin olun.

### <a name="can-i-deploy-virtual-machines-or-role-instances-to-my-gateway-subnet"></a>Sanal Makineleri veya rol örneklerini ağ geçidi alt ağıma dağıtabilir miyim?
Hayır.

### <a name="can-i-get-my-vpn-gateway-ip-address-before-i-create-it"></a>Oluşturmadan önce VPN ağ geçidi IP adresimi alabilir miyim?
Hayır. IP adresini almak için önce ağ geçidi oluşturmanız gerekir. VPN ağ geçidinizi silip yeniden oluşturursanız IP adresi değişir.

### <a name="how-does-my-vpn-tunnel-get-authenticated"></a>VPN tünelimin kimliği nasıl doğrulanır?
Azure VPN PSK (Önceden Paylaşılan Anahtar) kimlik doğrulamasını kullanır. VPN tüneli oluşturduğumuzda önceden paylaşılan anahtar (PSK) oluştururuz. Otomatik olarak oluşturulan PSK’yi, Önceden Paylaştırılan Anahtar PowerShell cmdlet'ini veya REST API’sini Ayarla ile kendi istediğiniz gibi değiştirebilirsiniz.

### <a name="can-i-use-the-set-pre-shared-key-api-to-configure-my-policy-based-static-routing-gateway-vpn"></a>Önceden Paylaşılan Anahtar API’sini Ayarla’yı ilke tabanlı (statik yönlendirme) ağ geçidi VPN’mi yapılandırmak için kullanabilir miyim?
Evet, Önceden Paylaşılan Anahtar API’sini ve PowerShell cmdlet’ini Ayarla, hem Azure ilke tabanlı (statik) VPN'ler, hem de rota tabanlı (dinamik) yönlendirme VPN'leri yapılandırmak için kullanılabilir.

### <a name="can-i-use-other-authentication-options"></a>Diğer kimlik doğrulama seçeneklerini kullanabilir miyim?
Önceden paylaşılan anahtarı (PSK) kimlik doğrulaması için sınırladık.


### <a name="how-do-i-specify-which-traffic-goes-through-the-vpn-gateway"></a>VPN ağ geçidime hangi trafiğin gideceğini nasıl belirtirim?

####<a name="resource-manager-deployment-model"></a>Resource Manager dağıtım modeli
* PowerShell: yerel ağ geçidinize ait trafiği belirtmek için "AddressPrefix" kullanın.
* Azure portalı: Yerel ağ geçidi > Yapılandırma > Adres alanı'na gidin.

####<a name="classic-deployment-model"></a>Klasik dağıtım modeli
* Azure portalı: Klasik sanal ağ > VPN bağlantıları > Siteden siteye VPN bağlantıları > Yerel site adı > Yerel site > İstemci adres alanı yolunu izleyin. 
* Klasik portal: Ağlar sayfasında Yerel Ağlar altında sanal ağınız için ağ geçidinden göndermek istediğiniz aralıkları ekleyin. 

### <a name="can-i-configure-forced-tunneling"></a>Zorlamalı Tüneli yapılandırabilir miyim?
Evet. Bkz. [Zorlamalı tüneli yapılandırma](vpn-gateway-about-forced-tunneling.md).

### <a name="can-i-set-up-my-own-vpn-server-in-azure-and-use-it-to-connect-to-my-on-premises-network"></a>Azure'da kendi VPN sunucumu kurup bu sunucuyu şirket içi ağıma bağlanmak üzere kullanabilir miyim?
Evet, Azure’de kendi VPN ağ geçitlerinizi veya sunucularınızı ister Azure Market’ten, ister kendi VPN yönlendiricilerinizi oluşturarak dağıtabilirsiniz. Şirket içi ağlarınız ve sanal ağ alt ağları arasında trafiğin düzgün yönlendirilmesini sağlamak amacıyla sanal ağınızda kullanıcı tanımlı yolları yapılandırmanız gerekir.

### <a name="why-are-certain-ports-opened-on-my-vpn-gateway"></a>Neden belirli bağlantı noktaları VPN ağ geçidimde açık?
Azure altyapı iletişimi için gereklidirler. Bunlar Azure sertifikaları tarafından korunur (kilitlenir). Uygun sertifikaları olmadan, bu ağ geçitlerinin müşterileri de dahil dış varlıklar, bu uç noktalarında etkiye neden olamazlar.

VPN ağ geçidi temel olarak, müşterinin özel ağında dokunulan tek NIC, ortak ağa da bakan tek NIC sahibi çok konaklı bir cihazdır. Azure altyapı varlıkları uyum nedenleriyle müşterinin özel ağlarına dokunamazlar; bu nedenle altyapı iletişimi için ortak uç noktaları kullanmaları gerekir. Ortak uç noktalar düzenli aralıklarla Azure güvenlik denetimi tarafından taranır.

### <a name="more-information-about-gateway-types-requirements-and-throughput"></a>Ağ geçidi türleri, gereksinimleri ve verimliliği hakkında daha fazla bilgi
Daha fazla bilgi için bkz. [VPN Gateway yapılandırma ayarları hakkında](vpn-gateway-about-vpn-gateway-settings.md).

## <a name="site-to-site-connections-and-vpn-devices"></a>Siteden Siteye bağlantılar ve VPN cihazları
### <a name="what-should-i-consider-when-selecting-a-vpn-device"></a>VPN cihazını seçerken nelere dikkat etmeliyim?
Cihaz satıcılarıyla işbirliğiyle bir dizi standart Siteden Siteye VPN cihazını doğruladık. Bilinen uyumlu VPN cihazlarının listesi, ilgili yapılandırma yönergeleri veya örnekleri ve cihaz özellikleri listesi [burada](vpn-gateway-about-vpn-devices.md) bulunabilir. Bilinen uyumlu olarak listelenen cihaz ailelerindeki tüm cihazlar Virtual Network ile çalışmalıdır. VPN cihazınızı yapılandırmaya yardım için uygun cihaz ailesine karşılık gelen cihaz yapılandırma örneğine veya bağlantılara bakın.

### <a name="what-do-i-do-if-i-have-a-vpn-device-that-isnt-in-the-known-compatible-device-list"></a>Bilinen uyumlu aygıt listesinde olmayan bir VPN cihazım varsa ne yapmalıyım?
Cihazınızın bilinen uyumlu VPN cihazı olarak listelendiğini görmüyorsanız ve bunu VPN bağlantınız için kullanmak istiyorsanız, bunun [burada](vpn-gateway-about-vpn-devices.md) listelenen desteklenen IPsec/IKE yapılandırma seçenekleri ve parametreleriyle eşleştiğini doğrulamanız gerekir. Minimum gereksinimleri karşılayan cihazların VPN gateway’lerle sorunsuz çalışması gerekir. Ek destek ve yapılandırma yönergeleri için cihaz üreticinize başvurun.

### <a name="why-does-my-policy-based-vpn-tunnel-go-down-when-traffic-is-idle"></a>Trafik boştayken neden ilke tabanlı VPN tünelim kayboluyor?
İlke tabanlı (statik rota olarak da bilinir) VPN Ağ geçitleri için bu beklenen bir davranıştır. tünel üzerindeki trafik 5 dakikadan fazla boşta kalırsa tünel bozulur. Herhangi bir yönde trafik akışı başladığında tünel hemen yeniden başlatılır.

### <a name="can-i-use-software-vpns-to-connect-to-azure"></a>Azure'e bağlanmak için VPN'ler yazılımını kullanabilir miyim?
Siteden Siteyi şirket içi ve dışı yapılandırması için Windows Server 2012 Yönlendirme ve Uzaktan Erişim (RRAS) sunucularını destekliyoruz.

Endüstri standardı IPsec uygulamalarıyla uyumlu olana kadar diğer yazılım VPN çözümleri bizim ağ geçidimizle çalışmalıdır. Yapılandırma ve destek hakkında yönergeler için yazılım satıcısına başvurun.

## <a name="a-namep2sapoint-to-site-connections"></a><a name="P2S"></a>Noktadan Siteye bağlantıları

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="a-namev2vmultiavnet-to-vnet-and-multi-site-connections"></a><a name="V2VMulti"></a>Sanal Ağdan Sanal Ağa ve Çok Siteli bağlantılar

[!INCLUDE [vpn-gateway-vnet-vnet-faq-include](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

### <a name="can-i-use-azure-vpn-gateway-to-transit-traffic-between-my-on-premises-sites-or-to-another-virtual-network"></a>Şirket içi sitelerim arasında veya başka bir sanal ağa trafiği geçirmek için Azure VPN ağ geçidini kullanabilir miyim?

**Resource Manager dağıtım modeli**<br>
Evet. Daha fazla bilgi için [BGP](#bgp) bölümüne bakın.

**Klasik dağıtım modeli**<br>
Klasik dağıtım modeli kullanılarak Azure VPN ağ geçidi üzerinden trafik geçirilebilse de, bu geçiş, ağ yapılandırma dosyasında statik olarak tanımlanan adres alanlarına bağlıdır. Klasik dağıtım modeli kullanan Azure Sanal Ağlar ve VPN ağ geçitleri ile BGP henüz desteklenmemektedir. BGP olmadan, geçiş adres alanlarının el ile tanımlanması çok hata eğilimindedir ve önerilmez.

### <a name="does-azure-generate-the-same-ipsecike-pre-shared-key-for-all-my-vpn-connections-for-the-same-virtual-network"></a>Azure, IPsec/IKE önceden paylaşılan anahtarı tüm VPN bağlantılarımla aynı sanal ağ için mi üretiyor?
Hayır, varsayılan olarak Azure farklı VPN bağlantıları için farklı önceden paylaşılan anahtarlar oluşturur. Ancak, isterseniz anahtar değeri ayarlamak için VPN Ağ Geçidi Anahtarı REST API veya PowerShell cmdlet'ini kullanabilirsiniz. Anahtar uzunluğu 1 ila 128 karakter arasında alfasayısal bir dize OLMALIDIR.

### <a name="do-i-get-more-bandwidth-with-more-site-to-site-vpns-than-for-a-single-virtual-network"></a>Daha fazla Siteden Siteye VPN ile tek bir sanal ağa göre daha fazla bant genişliği elde edebilir miyim?
Hayır, Noktadan Siteye VPN’lerde dahil tüm VPN tünelleri aynı Azure VPN ağ geçidini ve kullanılabilir bant genişliğini paylaşır.

### <a name="can-i-configure-multiple-tunnels-between-my-virtual-network-and-my-on-premises-site-using-multi-site-vpn"></a>Çok siteli VPN kullanarak sanal ağlarım ve şirket içi sitem arasında birden fazla tünel yapılandırabilir miyim?
Evet, ancak iki tünelde de aynı konum için BGP yapılandırması gerçekleştirmeniz gerekir.

### <a name="can-i-use-point-to-site-vpns-with-my-virtual-network-with-multiple-vpn-tunnels"></a>Birden çok VPN tüneline sahip sanal ağımla birlikte Noktadan Siteye VPN’lerini kullanabilir miyim?
Evet, Noktadan Siteye (P2S) VPN’ler şirket içi sitelere ve başka sanal ağlara VPN ağ geçitleriyle kullanılabilir.

### <a name="can-i-connect-a-virtual-network-with-ipsec-vpns-to-my-expressroute-circuit"></a>IPsec VPN’lerle sanal ağı ExpressRoute devreme bağlayabilir miyim?
Evet, bu desteklenir. Daha fazla bilgi için bkz. [Bir arada var olan ExpressRoute ve Siteden Siteye VPN bağlantıları yapılandırma](../expressroute/expressroute-howto-coexist-classic.md).

## <a name="a-namebgpabgp"></a><a name="bgp"></a>BGP
[!INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)]

## <a name="cross-premises-connectivity-and-vms"></a>Şirket içi ve dışı bağlantısı ve VM'ler
### <a name="if-my-virtual-machine-is-in-a-virtual-network-and-i-have-a-cross-premises-connection-how-should-i-connect-to-the-vm"></a>Sanal makinem sanal bir ağdaysa, şirket içi ve dışı bağlantım varsa VM’ye nasıl bağlanmalıyım?
Birkaç seçeneğiniz vardır. RDP etkinse ve bir uç nokta oluşturduysanız, VIP kullanarak sanal makineye bağlanabilirsiniz. Bu durumda, VIP ve bağlanmak istediğiniz bağlantı noktasını belirtmeniz gerekir. Sanal makinenizde trafik için bağlantı noktası yapılandırmanız gerekir. Genellikle, Klasik Azure Portalı’na gidip RDP bağlantı ayarlarını bilgisayarınıza kaydedersiniz. Ayarlar gerekli bağlantı bilgilerini içerir.

Şirket içi bağlantısı ile yapılandırılmış bir sanal ağ varsa, iç DIP veya özel bir IP adresi kullanılarak sanal makinenizi sanal makineye bağlanabilir. Ayrıca, aynı sanal ağda bulunan başka bir sanal makineden sanal makinenize iç DIP ile bağlanabilirsiniz. Sanal ağınızın dışında bir konumdan bağlanıyorsanız DIP kullanarak sanal makinenizde RDP gerçekleştiremezsiniz. Örneğin, yapılandırılmış bir Noktadan Siteye sanal ağınız varsa ve bilgisayarınızdan bağlantı kurmuyorsanız, sanal makineyi DIP ile bağlayamazsınız.

### <a name="if-my-virtual-machine-is-in-a-virtual-network-with-cross-premises-connectivity-does-all-the-traffic-from-my-vm-go-through-that-connection"></a>Sanal makinem şirket içi ve dışı bağlantılı bir sanal ağdaysa, VM’me ait trafiğin tümü bu bağlantıdan geçer mi?
Hayır. Bir tek, belirttiğiniz sanal ağ Yerel Ağ Ip adresi aralıklarında bulunan hedef IP’si olan trafik sanal ağ geçidinden geçer. Sanal ağ içinde yer alan bir hedef IP'ye sahip olan trafik, sanal ağ içinde kalır. Diğer trafik ortak ağlara yük dengeleyiciyle gönderilir veya zorlamalı tünel kullanılırsa, Azure VPN ağ geçidi üzerinden gönderilir. Sorun gideriyorsanız, ağ geçidi üzerinden göndermek istediğiniz Yerel Ağda tüm aralıklarınızın listelendiğinden emin olmanız önemlidir. Yerel Ağ adres aralıklarınızın sanal ağda başka adres aralıklarıyla çakışmadığını doğrulayın. Ayrıca, kullandığınız DNS sunucusunun, adı uygun IP adresine çözümlediğini doğrulamak istersiniz.

## <a name="virtual-network-faq"></a>Virtual Network SSS
Ek sanal ağ ek bilgilerini [Virtual Network SSS](../virtual-network/virtual-networks-faq.md) bölümünde görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

* VPN Gateway hakkında daha fazla bilgi için bkz. [VPN Gateway Hakkında](vpn-gateway-about-vpngateways.md).
* VPN Gateway yapılandırma ayarları hakkında daha fazla bilgi için bkz. [VPN Gateway yapılandırma ayarları hakkında](vpn-gateway-about-vpn-gateway-settings.md).


<!--HONumber=Feb17_HO3-->


