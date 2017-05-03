Bir şirket içi ağı ile Siteden Siteye bağlantılar için VPN cihazı gerekir. Tüm VPN cihazlarına yönelik yapılandırma adımları verilmese de, aşağıdaki bağlantılarda verilen bilgileri faydalı bulabilirsiniz:

- Uyumlu VPN cihazları hakkında bilgi için bkz. [VPN Cihazları](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md). 
- Cihaz yapılandırma ayarlarının bağlantıları için bkz. [Doğrulanan VPN Cihazları](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#devicetable). Mümkün olan en iyi cihaz yapılandırma bağlantıları verilmiştir. En son yapılandırma bilgileri için her zaman cihaz üreticinize başvurmanız en iyi yöntemdir.
- Cihaz yapılandırma örneklerini düzenleme hakkında daha fazla bilgi için bkz. [Örnekleri düzenleme](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#editing).
- IPsec/IKE parametreleri için bkz. [Parametreler](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec).
- VPN cihazınızı yapılandırmadan önce, kullanmak istediğiniz VPN cihazının [Bilinen cihaz uyumluluğu sorunları](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#known) olup olmadığını denetleyin.

VPN cihazınızı yapılandırırken şunlar gerekir:

- Paylaşılan bir anahtar. Siteden Siteye VPN bağlantınızı oluştururken belirttiğiniz paylaşılan anahtarın aynısıdır. Bu örneklerde temel bir paylaşılan anahtar kullanılır. Kullanmak için daha karmaşık bir anahtar oluşturmanız önerilir.

- Sanal ağ geçidinizin Genel IP adresi. Azure Portal, PowerShell veya CLI kullanarak genel IP adresini görüntüleyebilirsiniz.