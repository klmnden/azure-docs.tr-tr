### <a name="supportedclientos"></a>Noktadan Siteye ile hangi istemci işletim sistemlerini kullanabilirim?

Aşağıdaki istemci işletim sistemleri desteklenmektedir:

* Windows 7 (32 bit ve 64 bit)
* Windows Server 2008 R2 (yalnızca 64 bit)
* Windows 8 (32 bit ve 64 bit)
* Windows 8.1 (32 bit ve 64 bit)
* Windows Server 2012 (yalnızca 64 bit)
* Windows Server 2012 R2 (yalnızca 64 bit)
* Windows 10
* Mac için OSX sürüm 10.11 (El Capitan)
* Mac için macOS sürüm 10.12 (Sierra)

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Noktadan Siteye yapılandırmamda kaç VPN istemci uç noktam olabilir?

Bir sanal ağa aynı anda bağlanabilen en fazla 128 VPN istemcisini destekliyoruz.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Noktadan Siteye özelliğini kullanarak ara sunucuları ve güvenlik duvarlarını geçirebilir miyim?

Azure iki Noktadan Siteye VPN seçeneği türünü destekler:

* Güvenli Yuva Tünel Protokolü (SSTP). SSTP, Microsoft’a özel SSL tabanlı bir çözümdür ve çoğu güvenlik duvarı 443 SSL’nin kullandığı TCP bağlantı noktasını açtığı için güvenlik duvarlarından geçebilir.

* IKEv2 VPN. Standart tabanlı bir IPsec VPN çözümü olan IKEv2 VPN, UDP bağlantı noktası 500 ve 4500 ile 50 numaralı IP protokolünü kullanır. Güvenlik duvarları her zaman bu bağlantı noktalarını açmaz ve bu nedenle IKEv2 VPN’nin proxy ile güvenlik duvarlarını geçememe olasılığı vardır.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Noktadan Siteye için yapılandırılmış istemci bilgisayarını yeniden başlatırsam VPN de otomatik olarak yeniden bağlanacak mı?

Varsayılan olarak, istemci bilgisayar VPN bağlantısını otomatik olarak yeniden başlatmaz.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Noktadan Siteye, VPN istemcilerde otomatik yeniden bağlanmayı ve DDNS’yi destekler mi?

Otomatik olarak yeniden ve DDNS şu anda Noktadan Siteye VPN'lerde desteklenmiyor.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Siteden Siteye ve Noktadan Siteye yapılandırmalarına aynı sanal ağda birlikte sahip olabilir miyim?

Evet. Resouce Manager dağıtım modeli için, ağ geçidiniz için RouteBased VPN türü olmalıdır. Klasik dağıtım modeli için dinamik bir ağ geçidiniz olması gerekir. Statik yönlendirme VPN ağ geçitleri veya PolicyBased VPN ağ geçitleri için Noktadan Siteye çözümünü desteklemiyoruz.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Aynı anda birden çok sanal ağa bağlanmak için Noktadan Siteye istemcisi yapılandırabilir miyim?

Hayır. Noktadan Siteye bir istemci, yalnızca sanal ağ geçidinin bulunduğu sanal ağdaki kaynaklara bağlanabilir.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Siteden Siteye ve Noktadan Siteye bağlantılardan ne kadar verimlilik bekleyebilirim?

VPN tünellerinin tam verimini elde etmek zordur. IPsec ve SSTP şifrelemesi ağır VPN protokolleridir. Verimlilik, şirket içi ve İnternet arasındaki bant genişliğiyle ve gecikme süresiyle de sınırlıdır. Yalnızca IKEv2 Noktadan Siteye VPN bağlantıları olan bir VPN Gateway için, bekleyebileceğiniz toplam aktarım hızı Ağ Geçidi SKU’suna bağlıdır. Aktarım hızı hakkında daha fazla bilgi için bkz. [Ağ geçidi SKU’ları](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku).

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp-andor-ikev2"></a>SSTP ve/veya IKEv2 desteği sağlayan Noktadan Siteye bağlantı için herhangi bir yazılım VPN istemcisi kullanabilir miyim?

Hayır. SSTP için yalnızca Windows’daki yerel VPN istemcisini ve IKEv2 için yalnızca Mac’teki yerel VPN istemcisini kullanabilirsiniz. Desteklenen istemci işletim sistemleri listesine başvurun.

### <a name="can-i-access-the-internet-when-i-am-connected-over-p2s-vpn"></a>P2S VPN üzerinden bağlıyken İnternet’e erişebilir miyim?

Evet, bir P2S VPN üzerindeyken İnternet’e erişebilirsiniz.

### <a name="does-azure-support-ikev2-vpn-with-windows"></a>Azure Windows ile IKEv2 VPN destekler mi?

Kullanıcılar IKEv2’yi destekleyen yerleşik Windows VPN istemcisini kullanarak Azure’a bağlanabilir. Ancak bir Windows cihazından IKEv2 bağlantıları aşağıdaki senaryoda çalışmaz:

  Kullanıcının cihazında çok sayıda güvenilen kök sertifika olduğunda, IKE değişimi sırasında ileti yükü boyutu büyüktür ve bu da IP katmanı parçalanmasına neden olur. Parçalar Azure ucunda reddedilir ve bu da bağlantının başarısız olmasıyla sonuçlanır. Bu sorunun oluştuğu tam sertifika sayısını tahmin etmek zordur. Sonuç olarak, Windows cihazlarından IKEv2 bağlantılarının çalışacağı garanti edilmez. Karma bir ortamda (Windows ve Mac cihazlarından oluşan) hem SSTP hem de IKEv2 yapılandırdığınızda, Windows VPN profili her zaman ilk olarak IKEv2 tünelini dener. Yukarıda açıklanan nedenden dolayı başarısız olursa, SSTP’ye geri döner.

### <a name="other-than-windows-and-mac-which-other-platforms-does-azure-support-for-p2s-vpn"></a>Azure, P2S VPN için Windows ve Mac dışında hangi platformları destekliyor?

Azure, P2S VPN için yalnızca Windows ve Mac’i desteklemektedir.

### <a name="i-already-have-an-azure-vpn-gateway-deployed-can-i-enabled-radius-andor-ikev2-vpn-on-it"></a>Zaten dağıtılmış bir Azure VPN Gateway’im var. Bu ağ geçidi üzerinde RADIUS ve/veya IKEv2 VPN etkinleştirebilir miyim?

Evet, bu yeni özellikleri önceden dağıtılmış ağ geçitleri üzerinde hem PowerShell hem de Azure portalı kullanarak etkinleştirebilirsiniz.