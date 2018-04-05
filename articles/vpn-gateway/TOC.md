# [VPN Gateway Belgeleri](index.md)

# Genel Bakış
## [VPN Gateway hakkında](vpn-gateway-about-vpngateways.md)
## [VPN Gateway ile ilgili SSS](vpn-gateway-vpn-faq.md)
## [Abonelik ve hizmet sınırlamaları](../azure-subscription-service-limits.md?toc=%2fazure%2fvpn-gateway%2ftoc.json)

# Başlarken
## Rota temelli VPN ağ geçidi oluşturma
### [Azure Portal](create-routebased-vpn-gateway-portal.md)
### [Azure PowerShell](create-routebased-vpn-gateway-powershell.md)
### [Azure CLI](create-routebased-vpn-gateway-cli.md)

# Kavramlar
## [VPN Gateway için planlama ve tasarım](vpn-gateway-plan-design.md)
## [VPN Gateway ayarları hakkında](vpn-gateway-about-vpn-gateway-settings.md)
## [VPN cihazları hakkında](vpn-gateway-about-vpn-devices.md)
## [Şifreleme gereksinimleri hakkında](vpn-gateway-about-compliance-crypto.md)
## [BGP ve VPN Gateway hakkında](vpn-gateway-bgp-overview.md)
## [Yüksek oranda kullanılabilir bağlantılar hakkında](vpn-gateway-highlyavailable.md)
## [Noktadan Siteye bağlantılar hakkında](point-to-site-about.md)

# Nasıl yapılır
## Siteden Siteye bağlantıları yapılandırma
### [Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
### [Azure PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
### [Azure CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
### [Azure portal (klasik)](vpn-gateway-howto-site-to-site-classic-portal.md)

## [VPN cihazı yapılandırma komut dosyalarını indirme](vpn-gateway-download-vpndevicescript.md)

## Noktada Siteye bağlantıları yapılandırma - Yerel Azure sertifika kimlik doğrulaması
### P2S VPN yapılandırma
#### [Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
#### [Azure PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
#### [Azure portal (klasik)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
### Otomatik olarak imzalanan sertifikalar oluşturma
#### [Azure PowerShell](vpn-gateway-certificates-point-to-site.md)
#### [Makecert](vpn-gateway-certificates-point-to-site-makecert.md)
### [VPN istemcisi yapılandırma dosyalarını oluşturma ve yükleme](point-to-site-vpn-client-configuration-azure-cert.md)
### [İstemci sertifikalarını yükleme](point-to-site-how-to-vpn-client-install-azure-cert.md)

## Noktadan Siteye bağlantıları yapılandırma - RADIUS kimlik doğrulaması
### P2S VPN yapılandırma
#### [Azure PowerShell](point-to-site-how-to-radius-ps.md)
### [VPN istemcisi yapılandırma dosyalarını oluşturma ve yükleme](point-to-site-vpn-client-configuration-radius.md)
### [P2S VPN RADIUS kimlik doğrulamasını NPS sunucusu ile tümleştirme](vpn-gateway-radiuis-mfa-nsp.md)

## VNet-VNet bağlantıları yapılandırma
### [Azure Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
### [Azure PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
### [Azure CLI](vpn-gateway-howto-vnet-vnet-cli.md)
### [Azure portal (klasik)](vpn-gateway-howto-vnet-vnet-portal-classic.md)
## Dağıtım modelleri arasında sanal ağdan sanal ağa bağlantı yapılandırma
### [Azure Portal](vpn-gateway-connect-different-deployment-models-portal.md)
### [Azure PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
## Birlikte bulunan Siteden Siteye bağlantılar ile ExpressRoute bağlantılarını yapılandırma
### [Azure PowerShell](../expressroute/expressroute-howto-coexist-resource-manager.md?toc=%2fazure%2fvpn-gateway%2ftoc.json)
## Birden çok Siteden Siteye bağlantı yapılandırma
### [Azure Portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
### [Azure PowerShell (klasik)](vpn-gateway-multi-site.md)
## Birden çok ilke tabanlı VPN cihazı bağlama
### [Azure PowerShell](vpn-gateway-connect-multiple-policybased-rm-ps.md)
## Bağlantılarda IPsec/IKE ilkelerini yapılandırma
### [Azure PowerShell](vpn-gateway-ipsecikepolicy-rm-powershell.md)
## Yüksek oranda kullanılabilir etkin-etkin bağlantıları yapılandırma
### [Azure PowerShell](vpn-gateway-activeactive-rm-powershell.md)
## Bir VPN ağ geçidi için BGP yapılandırma
### [Azure PowerShell](vpn-gateway-bgp-resource-manager-ps.md)
### [Azure CLI](bgp-how-to-cli.md)
## Zorlamalı tünel yapılandırma
### [Azure PowerShell](vpn-gateway-forced-tunneling-rm.md)
### [Azure PowerShell (klasik)](vpn-gateway-about-forced-tunneling.md)
## Yerel ağ geçidi ayarlarını değiştirme
### [Azure Portal](vpn-gateway-modify-local-network-gateway-portal.md)
### [Azure PowerShell](vpn-gateway-modify-local-network-gateway.md)
### [Azure CLI](vpn-gateway-modify-local-network-gateway-cli.md)
## [VPN ağ geçidi bağlantısını doğrulama](vpn-gateway-verify-connection-resource-manager.md)
## [VPN ağ geçidini sıfırlama](vpn-gateway-resetgw-classic.md)
## VPN ağ geçidi silme
### [Azure Portal](vpn-gateway-delete-vnet-gateway-portal.md)
### [Azure PowerShell](vpn-gateway-delete-vnet-gateway-powershell.md)
### [Azure PowerShell (klasik)](vpn-gateway-delete-vnet-gateway-classic-powershell.md)
## [VPN ağ geçidi yapılandırma (klasik)](vpn-gateway-configure-vpn-gateway-mp.md)
## [Ağ geçidi SKU’ları (eski)](vpn-gateway-about-skus-legacy.md)
## Üçüncü taraf VPN cihazlarını yapılandırma
### [Genel bakış ve Azure yapılandırması](vpn-gateway-3rdparty-device-config-overview.md)
### [Örnek: Cisco ASA cihazı (IKEv2/BGP yok)](vpn-gateway-3rdparty-device-config-cisco-asa.md)
## [Klasikten Kaynak Yöneticisi’ne geçiş](vpn-gateway-classic-resource-manager-migration.md)
## [Sorun giderme](vpn-gateway-troubleshoot.md)
### [Topluluk tarafından önerilen VPN veya güvenlik duvarı cihaz ayarları](vpn-gateway-third-party-settings.md)
### [VNet veya VPN bağlantılarını yapılandırma ve doğrulama](https://support.microsoft.com/help/4032151/configuring-and-validating-vnet-or-vpn-connections)
### [Bir sanal ağa VPN aktarım hızını doğrulama](vpn-gateway-validate-throughput-to-vnet.md)
### Noktadan Siteye bağlantı sorunları
#### [Noktadan Siteye bağlantı sorunları](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md)
#### [Noktadan Siteye bağlantı sorunları - Mac OS X VPN istemcisi](vpn-gateway-troubleshoot-point-to-site-osx-ikev2.md)
### Siteden Siteye bağlantı sorunları
#### [Siteden Siteye bağlantı sorunları](vpn-gateway-troubleshoot-site-to-site-cannot-connect.md)
#### [Siteden Siteye bağlantı aralıklı olarak kesiliyor](vpn-gateway-troubleshoot-site-to-site-disconnected-intermittently.md)


# Başvuru
## [Azure PowerShell](/powershell/module/azurerm.network/?view=azurermps-4.0.0#vpn)
## [Azure PowerShell (klasik)](/powershell/module/azure/?view=azuresmps-3.7.0#networking)
## [REST](/rest/api/network/virtualnetworkgateways)
## [REST (klasik)](https://msdn.microsoft.com/library/jj154113)
## [Azure CLI](/cli/azure/network/vnet-gateway)

# İlgili
## [Sanal Ağ](/azure/virtual-network/)
## [Application Gateway](/azure/application-gateway/)
## [Azure DNS](/azure/dns/)
## [Traffic Manager](/azure/traffic-manager/)
## [Yük Dengeleyici](/azure/load-balancer/)
## [ExpressRoute](/azure/expressroute/)

# Kaynaklar
## [Azure Yol Haritası](https://azure.microsoft.com/roadmap/?category=networking)
## [Blog](https://azure.microsoft.com/blog/topics/networking)
## [Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesVirtualNetwork)
## [Fiyatlandırma](https://azure.microsoft.com/pricing/details/vpn-gateway)
## [Fiyatlandırma hesaplayıcı](https://azure.microsoft.com/pricing/calculator/)
## [SLA](https://azure.microsoft.com/support/legal/sla)
## [Videolar](https://azure.microsoft.com/documentation/videos/index/?services=vpn-gateway)
