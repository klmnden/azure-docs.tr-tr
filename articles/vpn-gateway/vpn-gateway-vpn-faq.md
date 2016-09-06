<properties 
   pageTitle="Virtual Network VPN Ağ Geçidi SSS | Microsoft Azure"
   description="VPN Ağ Geçidi SSS. Microsoft Azure Virtual Network şirket içi ve dışı bağlantılar, karma yapılandırma bağlantıları ve VPN Ağ Geçitleri hakkında SSS"
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor="" />
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/10/2016"
   ms.author="yushwang" />

# VPN Gateway SSS

## Sanal ağlara bağlama

### Farklı Azure bölgelerinde sanal ağlara bağlanabilir miyim?
Evet. Aslında, hiçbir bölge kısıtlaması yoktur. Bir sanal ağ aynı bölgedeki veya farklı bir Azure bölgesindeki başka bir sanal ağa bağlanabilir.

### Farklı aboneliklerle sanal ağlara bağlanabilir miyim?
Evet.

### Tek bir sanal ağdan birden çok siteye bağlanabilir miyim?

Windows PowerShell ve Azure REST API'lerini kullanarak birden çok siteye bağlanabilirsiniz. [Çok siteli ve VNet - VNet Bağlantı](#multi-site-and-vnet-to-vnet-connectivity) SSS bölümüne bakın.
## Şirket içi ve dışı bağlantı seçeneklerim nelerdir?

Aşağıdaki şirket içi ve dışı bağlantılar desteklenmektedir:

- [Siteden Siteye](vpn-gateway-site-to-site-create.md) – IPsec üzerinden (IKE v1 ve IKE v2) VPN bağlantısı. Bu bağlantı türüne şirket içi bir VPN cihazı ya da RRAS gerekir.

- [Noktadan Siteye](vpn-gateway-point-to-site-create.md) – SSTP (Güvenli Yuva Tünel Protokolü) üzerinden VPN bağlantısı Bu bağlantıya VPN cihazı gerekmez.

- [VNet - VNet](virtual-networks-configure-vnet-to-vnet-connection.md) – Bu türden bağlantı Siteden Siteye yapılandırmasıyla aynıdır. VNet - VNet, IPsec üzerinden (IKE v1 ve IKE v2) bir VPN bağlantısıdır. VPN cihazı gerekmez.

- [Çok Siteli](vpn-gateway-multi-site.md) – Birden çok şirket içi sitenin bir sanal ağa bağlanmasına olanak sağlayan Siteden Siteye yapılandırmanın bir çeşididir.

- [ExpressRoute](../expressroute/expressroute-introduction.md) – ExpressRoute genel İnternet üzerinden değil WAN bağlantınızdan Azure’e doğrudan yapılan bir bağlantıdır. Daha fazla bilgi için bkz. [ExpressRoute’a Teknik Genel Bakış](../expressroute/expressroute-introduction.md) ve [.ExpressRoute SSS](../expressroute/expressroute-faqs.md).

Bağlantılar hakkında daha fazla bilgi için bkz. [VPN Gateway Hakkında](vpn-gateway-about-vpngateways.md).

### Siteden Siteye bağlantı ve Noktadan Siteye bağlantı arasındaki fark nelerdir?

**Siteden Siteye** bağlantılar, şirket içinde yer alan bilgisayarlarla sanal makineler arasında bağ kurmanızı sağlayan bağlantılardır ya da rota yapılandırmayı nasıl seçtiğinize bağlı olarak sanal ağınızdaki rol örneğidir. Her zaman kullanıma uygun şirket içi ve dışı bağlantı için müthiş bir seçenek; karma yapılandırmalar için de çok uygundur. Bu tür bir bağlantı; ağınıza ucuna dağıtılmış olması gereken IPsec VPN uygulamasına bağlıdır (donanım veya yazılım aracı). Bu tür bir bağlantı oluşturmak için gerekli VPN donanımına ve dışarıya yönelik bir IPv4 adresine sahip olmanız gerekir.

**Noktadan Siteye** bağlantılar, sanal ağınızda bulunan her yerden her şeye tek bir bilgisayardan bağlanmanızı sağlar. Windows yerleşik VPN istemcisi kullanır. Noktadan Siteye yapılandırmasının bir parçası olarak, bir sertifika ve bir VPN istemci yapılandırma paketi yüklerseniz; bu pakette, bilgisayarınızın sanal ağda herhangi bir sanal makineye veya rol örneğine bağlanmasını sağlayan ayarlar bulunur. Şirket içi olmayan sanal ağa bağlanmak istediğinizde çok yararlıdır. Her ikisi de Siteden Siteye bağlantısına gereken VPN donanımına veya dışarıya dönük IPv4 adresine erişiminiz yoksa bu da iyi bir seçenektir. 

Siteden Siteye bağlantınızı ağ geçidinizi için rota tabanlı VPN türü kullanarak oluşturmanız kaydıyla, sanal ağınızı aynı anda hem Siteden Siteye, hem de Noktadan Siteye bağlanacak şekilde yapılandırabilirsiniz. Rota tabanlı VPN türlerine klasik dağıtım modelinde dinamik ağ geçitleri adı verilir.

### ExpressRoute nedir?

ExpressRoute, Microsoft veri merkezleri ile şirketinizde veya bir birlikte bulundurma ortamında bulunan altyapı arasında özel bağlantılar oluşturmanızı sağlar. ExpressRoute ile bir ExpressRoute iş ortağı ortak konum tesisinde Microsoft Azure ve Office 365 gibi Microsoft bulut hizmetleri arasında bağlantı kurabilir veya doğrudan bir ağ hizmeti sağlayıcısı tarafından sağlanan mevcut WAN ağınız (örneğin, bir MPLS VPN gibi) üzerinden bağlanabilirsiniz.

ExpressRoute bağlantısı İnternet üzerinden genel bağlantılara göre daha iyi güvenlik, daha fazla güvenilirlik, daha yüksek bant genişliği ve daha düşük gecikme sunar. Bazı durumlarda, ExpressRoute bağlantıları kullanarak şirket içi ağınız ve Azure arasında veri aktarmak önemli maliyet avantajları da sağlar. Şirket içi ağınızdan Azure’e şirket içi ve dışı bir bağlantıyı zaten oluşturduysanız, sanal ağınızı olduğu gibi tutarken ExpressRoute bağlantısına zaten geçmişsinizdir.

Daha fazla ayrıntı için bkz. [ExpressRoute SSS](../expressroute/expressroute-faqs.md).

## Siteden Siteye bağlantılar ve VPN cihazları

### VPN cihazını seçerken nelere dikkat etmeliyim?

Cihaz satıcılarıyla işbirliğiyle bir dizi standart Siteden Siteye VPN cihazını doğruladık. Bilinen uyumlu VPN cihazlarının listesi, ilgili yapılandırma yönergeleri veya örnekleri ve cihaz özellikleri listesi [burada](vpn-gateway-about-vpn-devices.md) bulunabilir. Bilinen uyumlu olarak listelenen cihaz ailelerindeki tüm cihazlar Virtual Network ile çalışmalıdır. VPN cihazınızı yapılandırmaya yardım için uygun cihaz ailesine karşılık gelen cihaz yapılandırma örneğine veya bağlantılara bakın.

### Bilinen uyumlu aygıt listesinde olmayan bir VPN cihazım varsa ne yapmalıyım?

Cihazınızın bilinen uyumlu VPN cihazı olarak listelendiğini görmüyorsanız ve bunu VPN bağlantınız için kullanmak istiyorsanız, bunun [burada](vpn-gateway-about-vpn-devices.md#devices-not-on-the-compatible-list) listelenen desteklenen IPsec/IKE yapılandırma seçenekleri ve parametreleriyle eşleştiğini doğrulamanız gerekir. Minimum gereksinimleri karşılayan cihazların VPN gateway’lerle sorunsuz çalışması gerekir. Ek destek ve yapılandırma yönergeleri için cihaz üreticinize başvurun.

### Trafik boştayken neden ilke tabanlı VPN tünelim kayboluyor?

İlke tabanlı (statik rota olarak da bilinir) VPN Ağ geçitleri için bu beklenen bir davranıştır. tünel üzerindeki trafik 5 dakikadan fazla boşta kalırsa tünel bozulur. Herhangi bir yönde trafik akışı başladığında tünel hemen yeniden başlatılır. Rota tabanlı (dinamik olarak da bilinir) VPN ağ geçidiniz varsa bu davranışla karşılaşmazsınız.

### Azure'e bağlanmak için VPN'ler yazılımını kullanabilir miyim?

Siteden Siteyi şirket içi ve dışı yapılandırması için Windows Server 2012 Yönlendirme ve Uzaktan Erişim (RRAS) sunucularını destekliyoruz.

Endüstri standardı IPsec uygulamalarıyla uyumlu olana kadar diğer yazılım VPN çözümleri bizim ağ geçidimizle çalışmalıdır. Yapılandırma ve destek hakkında yönergeler için yazılım satıcısına başvurun.

## Noktadan Siteye bağlantılar

### Noktadan Siteye ile hangi işletim sistemlerini kullanabilirim?

Aşağıdaki işletim sistemleri desteklenmektedir:

- Windows 7 (32 bit ve 64 bit)

- Windows Server 2008 R2 (yalnızca 64 bit)

- Windows 8 (32 bit ve 64 bit)

- Windows 8.1 (32 bit ve 64 bit)

- Windows Server 2012 (yalnızca 64 bit)

- Windows Server 2012 R2 (yalnızca 64 bit)

- Windows 10

### SSTP destekleyen Noktadan Siteye için herhangi bir yazılım VPN istemcisi kullanabilir miyim?

Hayır. Destek, yalnızca yukarıda listelenen Windows işletim sistemi sürümleriyle sınırlıdır.

### Noktadan Siteye yapılandırmamda kaç VPN istemci uç noktam olabilir?

Bir sanal ağa aynı anda bağlanabilen en fazla 128 VPN istemcisini destekliyoruz.

### Noktadan Siteye bağlanabilirlik için kendi iç PKI kök CA’mı kullanabilir miyim?

Evet. Önceden, yalnızca otomatik olarak imzalanan kök sertifikalar kullanılabiliyordu. 20 kök sertifika yükleyebilirsiniz.

### Noktadan Siteye özelliğini kullanarak ara sunucuları ve güvenlik duvarlarını geçirebilir miyim?

Evet. Güvenlik duvarları üzerinden tünele SSTP (Güvenli Yuva Tünel Protokolü) kullanıyoruz. Bu tünel bir HTTPs bağlantısı olarak görünür.

### Noktadan Siteye için yapılandırılmış istemci bilgisayarını yeniden başlatırsam VPN de otomatik olarak yeniden bağlanacak mı?

Varsayılan olarak, istemci bilgisayar VPN bağlantısını otomatik olarak yeniden başlatmaz.

### Noktadan Siteye, VPN istemcilerde otomatik yeniden bağlanmayı ve DDNS’yi destekler mi?

Otomatik olarak yeniden ve DDNS şu anda Noktadan Siteye VPN'lerde desteklenmiyor.

### Siteden Siteye ve Noktadan Siteye yapılandırmalarına aynı sanal ağda birlikte sahip olabilir miyim?

Evet. Bu her iki çözüm de, ağ geçidiniz için Yol Tabanlı VPN türünüz varsa çalışacaktır. Klasik dağıtım modeli için dinamik bir ağ geçidiniz olması gerekir. Statik yönlendirme VPN ağ geçitleri veya -VpnType PolicyBased kullanan ağ geçitleri için Noktadan Siteye çözümünü desteklemiyoruz.

### Aynı anda birden çok sanal ağa bağlanmak için Noktadan Siteye istemcisi yapılandırabilir miyim?

Evet, olabilir. Ancak sanal ağlarda, sanal ağlar arasında çakışmaması gereken IP öneklerinin ve Noktadan Siteye adres alanlarının çakışmaması gerekir.

### Siteden Siteye ve Noktadan Siteye bağlantılardan ne kadar verimlilik bekleyebilirim?

VPN tünellerinin tam verimini elde etmek zordur. IPsec ve SSTP şifrelemesi ağır VPN protokolleridir. Verimlilik, şirket içi ve İnternet arasındaki bant genişliğiyle ve gecikme süresiyle de sınırlıdır.

## Ağ geçitleri

### İlke tabanlı (statik yönlendirme) ağ geçidi nedir?

İlke tabanlı ağ geçitleri, ilke tabanlı VPN'leri uygular. İlke temelli VPN'ler, şirket içi ağınızla Azure VNet'iniz arasında adres öneklerinin birleşimleri temelindeki IPsec tüneller üzerinden paketleri şifreler ve yönlendirirler. İlke (veya Trafik Seçici) çoğunlukla VPN yapılandırmasında bir erişim listesi olarak tanımlanır.

### Rota tabanlı (dinamik yönlendirme) ağ geçidi nedir?

Rota tabanlı ağ geçitleri yol tabanlı VPN'leri uygular. Rota temelli VPN'ler, paketleri kendi ilgili arabirimlerine yönlendirmek için IP iletme veya yönlendirme tablosunda "yolları" seçeneğini kullanır. Bundan sonra tünel arabirimleri, paketleri tünellerin içinde veya dışında şifreler veya şifrelerini çözer. Rota temelli VPN’lerle ilgili ilke veya trafik seçici herhangi birinden herhangi birine (veya joker karakterler) olarak yapılandırılır.

### Oluşturmadan önce VPN ağ geçidi IP adresimi alabilir miyim?

Hayır. IP adresini almak için önce ağ geçidi oluşturmanız gerekir. VPN ağ geçidinizi silip yeniden oluşturursanız IP adresi değişir.

### VPN tünelimin kimliği nasıl doğrulanır?

Azure VPN PSK (Önceden Paylaşılan Anahtar) kimlik doğrulamasını kullanır. VPN tüneli oluşturduğumuzda önceden paylaşılan anahtar (PSK) oluştururuz. Otomatik olarak oluşturulan PSK’yi, Önceden Paylaştırılan Anahtar PowerShell cmdlet'ini veya REST API’sini Ayarla ile kendi istediğiniz gibi değiştirebilirsiniz.

### Önceden Paylaşılan Anahtar API’sini Ayarla’yı ilke tabanlı (statik yönlendirme) ağ geçidi VPN’mi yapılandırmak için kullanabilir miyim?

Evet, Önceden Paylaşılan Anahtar API’sini ve PowerShell cmdlet’ini Ayarla, hem Azure ilke tabanlı (statik) VPN'ler, hem de rota tabanlı (dinamik) yönlendirme VPN'leri yapılandırmak için kullanılabilir.

### Diğer kimlik doğrulama seçeneklerini kullanabilir miyim?

Önceden paylaşılan anahtarı (PSK) kimlik doğrulaması için sınırladık.

### "Ağ geçidi alt ağı" nedir ve neden gerekir?

Şirket içi ve dışı bağlantısını etkinleştirmek üzere çalıştırdığımız bir ağ geçidi hizmetimiz bulunmaktadır. 

Bir VPN ağ geçidi yapılandırmak için VNet’inizi için bir ağ geçidi alt ağı oluşturmanız gerekir. Tüm ağ geçidi alt ağlarının düzgün çalışması için GatewaySubnet şeklinde adlandırılması gerekir. Ağ geçidi alt ağını başka şekilde adlandırmayın. VM’leri veya herhangi başka bir şeyi de ağ geçidi alt ağına dağıtmayın.

Ağ geçidi alt ağı minimum boyutu tümüyle oluşturmak istediğiniz yapılandırmaya bağlıdır. Bazı yapılandırmalar için /29 kadar küçük ağ geçidi alt ağı yapılandırmaları oluşturmak mümkün olmakla birlikte, /28 ya da daha büyük (/28, /27, /26, vb.) ağ geçidi alt ağı oluşturmanızı öneriyoruz. 

### Sanal Makineleri veya rol örneklerini ağ geçidi alt ağıma dağıtabilir miyim?

Hayır.

### VPN ağ geçidime hangi trafiğin gideceğini nasıl belirtirim?

Azure Klasik Portalı kullanıyorsanız, Ağlar sayfasında Yerel Ağlar altında sanal ağınız için ağ geçidinden göndermek istediğiniz aralığı ekleyin.

### Zorlamalı Tüneli yapılandırabilir miyim?

Evet. Bkz. [Zorlamalı tüneli yapılandırma](vpn-gateway-about-forced-tunneling.md).

### Azure'da kendi VPN sunucumu kurup bu sunucuyu şirket içi ağıma bağlanmak üzere kullanabilir miyim?

Evet, Azure’de kendi VPN ağ geçitlerinizi veya sunucularınızı ister Azure Market’ten, ister kendi VPN yönlendiricilerinizi oluşturarak dağıtabilirsiniz. Şirket içi ağlarınız ve sanal ağ alt ağları arasında trafiğin düzgün yönlendirilmesini sağlamak amacıyla sanal ağınızda kullanıcı tanımlı yolları yapılandırmanız gerekir.

### Neden belirli bağlantı noktaları VPN ağ geçidimde açık?

Azure altyapı iletişimi için gereklidirler. Bunlar Azure sertifikaları tarafından korunur (kilitlenir). Uygun sertifikaları olmadan, bu ağ geçitlerinin müşterileri de dahil dış varlıklar, bu uç noktalarında etkiye neden olamazlar.

VPN ağ geçidi temel olarak, müşterinin özel ağında dokunulan tek NIC, ortak ağa da bakan tek NIC sahibi çok konaklı bir cihazdır. Azure altyapı varlıkları uyum nedenleriyle müşterinin özel ağlarına dokunamazlar; bu nedenle altyapı iletişimi için ortak uç noktaları kullanmaları gerekir. Ortak uç noktalar düzenli aralıklarla Azure güvenlik denetimi tarafından taranır.


### Ağ geçidi türleri, gereksinimleri ve verimliliği hakkında daha fazla bilgi

Daha fazla bilgi için bkz. [VPN Gateway Ayarları Hakkında](vpn-gateway-about-vpn gateway-settings.md).

## Çok siteli ve VNet - VNet bağlantısı

### Hangi tür ağ geçitleri çok siteli ve VNet - VNet bağlantısını destekler?

Yalnızca rota tabanlı (dinamik yönlendirme) VPN’ler

### PolicyBased VPN türüne sahip VNet’i RouteBased VPN türüne sahip başka bir VNet’e bağlayabilir miyim?

Hayır, her iki sanal ağın da rota tabanlı (dinamik yönlendirme) VPN kullanıyor olması GEREKİR.

### VNet - VNet trafiği güvenli mi?

Evet, IPsec/IKE şifrelemesiyle korunur.

### VNet - VNet trafiği Azure omurga üzerinden yolculuk ediyor mu?

Evet.

### Bir sanal ağ kaç şirket içi siteye ve sanal ağa bağlanabilir?

En çok, Temel ve Standart Dinamik Yönlendirme ağ geçitleri için 10 birleştirilmiş; Yüksek performans VPN ağ geçitleri için 30.

### Birden çok VPN tüneline sahip sanal ağımla birlikte Noktadan Siteye VPN’lerini kullanabilir miyim?

Evet, Noktadan Siteye (P2S) VPN’ler şirket içi sitelere ve başka sanal ağlara VPN ağ geçitleriyle kullanılabilir.

### Çok siteli VPN kullanarak sanal ağlarım ve şirket içi sitem arasında birden fazla tünel yapılandırabilir miyim?

Hayır, Azure sanal ağı ve şirket içi bir site arasında yedek tüneller desteklenmez.

### Bağlı sanal ağlar ve şirket içi yerel siteleri arasında çakışan adres alanları olabilir mi?

Hayır. Adres alanlarının çakışması ağ yapılandırma dosyasının yüklenmesine veya “Sanal Ağ Oluşturma” işleminin başarısız olmasına neden olur.

### Daha fazla Siteden Siteye VPN ile tek bir sanal ağa göre daha fazla bant genişliği elde edebilir miyim?

Hayır, Noktadan Siteye VPN’lerde dahil tüm VPN tünelleri aynı Azure VPN ağ geçidini ve kullanılabilir bant genişliğini paylaşır.

### Şirket içi sitelerim arasında veya başka bir sanal ağa trafiği geçirmek için Azure VPN ağ geçidini kullanabilir miyim?

**Klasik dağıtım modeli**<br>
Klasik dağıtım modeli kullanılarak Azure VPN ağ geçidi üzerinden trafik geçirilebilse de, ağ yapılandırma dosyasında istatistiksel olarak tanımlanan adres alanlarına bağlıdır. Klasik dağıtım modeli kullanan Azure Virtual Networks ve VPN ağ geçitleri ile BGP henüz desteklenmemektedir. BGP olmadan, geçiş adres alanlarının el ile tanımlanması çok hata eğilimindedir ve önerilmez.<br>
**Resource Manager dağıtım modeli**<br>
Resource Manager dağıtım modeli kullanıyorsanız daha fazla bilgi için [BGP](#bgp) bölümüne bakın.

### Azure, IPsec/IKE önceden paylaşılan anahtarı tüm VPN bağlantılarımla aynı sanal ağ için mi üretiyor?

Hayır, varsayılan olarak Azure farklı VPN bağlantıları için farklı önceden paylaşılan anahtarlar oluşturur. Ancak, isterseniz anahtar değeri ayarlamak için VPN Ağ Geçidi Anahtarı REST API veya PowerShell cmdlet'ini kullanabilirsiniz. Anahtar uzunluğu 1 ila 128 karakter arasında alfasayısal bir dize OLMALIDIR.

### Azure sanal ağlar arasındaki trafik için ücretli mi?

Farklı Azure sanal ağları arasındaki trafik için yalnızca trafik bir Azure bölgesinden diğerine geçtiğinde Azure ücretlidir. Ücretler Azure [VPN Ağ Geçidi Fiyatlandırma](https://azure.microsoft.com/pricing/details/vpn-gateway/) sayfasında listelenmiştir.


### IPsec VPN’lerle sanal ağı ExpressRoute devreme bağlayabilir miyim?

Evet, bu desteklenir. Daha fazla bilgi için bkz. [Bir arada var olan ExpressRoute ve Siteden Siteye VPN bağlantıları yapılandırma](../expressroute/expressroute-howto-coexist-classic.md).

## <a name="bgp"></a>BGP

[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)] 



## Şirket içi ve dışı bağlantısı ve VM'ler

### Sanal makinem sanal bir ağdaysa, şirket içi ve dışı bağlantım varsa VM’ye nasıl bağlanmalıyım?

Birkaç seçeneğiniz vardır. RDP etkinse ve bir uç nokta oluşturduysanız, VIP kullanarak sanal makineye bağlanabilirsiniz. Bu durumda, VIP ve bağlanmak istediğiniz bağlantı noktasını belirtmeniz gerekir. Sanal makinenizde trafik için bağlantı noktası yapılandırmanız gerekir. Genellikle, Klasik Azure Portalı’na gidip RDP bağlantı ayarlarını bilgisayarınıza kaydedersiniz. Ayarlar gerekli bağlantı bilgilerini içerir.

Şirket içi bağlantısı ile yapılandırılmış bir sanal ağ varsa, iç DIP veya özel bir IP adresi kullanılarak sanal makinenizi sanal makineye bağlanabilir. Ayrıca, aynı sanal ağda bulunan başka bir sanal makineden sanal makinenize iç DIP ile bağlanabilirsiniz. Sanal ağınızın dışında bir konumdan bağlanıyorsanız DIP kullanarak sanal makinenizde RDP gerçekleştiremezsiniz. Örneğin, yapılandırılmış bir Noktadan Siteye sanal ağınız varsa ve bilgisayarınızdan bağlantı kurmuyorsanız, sanal makineyi DIP ile bağlayamazsınız.

### Sanal makinem şirket içi ve dışı bağlantılı bir sanal ağdaysa, VM’me ait trafiğin tümü bu bağlantıdan geçer mi?

Hayır. Bir tek, belirttiğiniz sanal ağ Yerel Ağ Ip adresi aralıklarında bulunan hedef IP’si olan trafik sanal ağ geçidinden geçer. Trafikte, sanal ağ içinde kalan sanal ağ içinde yer alan hedef IP vardır. Diğer trafik ortak ağlara yük dengeleyiciyle gönderilir veya zorlamalı tünel kullanılırsa, Azure VPN ağ geçidi üzerinden gönderilir. Sorun gideriyorsanız, ağ geçidi üzerinden göndermek istediğiniz Yerel Ağda tüm aralıklarınızın listelendiğinden emin olmanız önemlidir. Yerel Ağ adres aralıklarınızın sanal ağda başka adres aralıklarıyla çakışmadığını doğrulayın. Ayrıca, kullanmakta olduğunuz DNS sunucusunun uygun IP adresi için ad çözdüğünü doğrulamak istersiniz.


## Virtual Network SSS

Ek sanal ağ ek bilgilerini [Virtual Network SSS](../virtual-network/virtual-networks-faq.md) bölümünde görürsünüz.
 



<!--HONumber=ago16_HO5-->


