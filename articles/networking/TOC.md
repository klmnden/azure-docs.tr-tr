# Genel Bakış
## [Azure ağı hakkında](networking-overview.md)
## Mimari
### [Sanal Veri Merkezleri](/azure/architecture/vdc/networking-virtual-datacenter)
### [Birden çok ağ yoluyla Asimetrik yönlendirme](../expressroute/expressroute-asymmetric-routing.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Ağ tasarımlarının güvenliğini sağlama](../best-practices-network-security.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Merkez-uç topolojisi](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)
### [Ağ güvenliği için en iyi uygulamalar](../security/azure-security-network-security-best-practices.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Yüksek oranda kullanılabilir ağ sanal gereçleri](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha )
### [Yük dengeleme metotlarını birleştirme](../traffic-manager/traffic-manager-load-balancing-azure.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Azure DNS ve Traffic Manager kullanarak olağanüstü durum kurtarma](disaster-recovery-dns-traffic-manager.md)
## Planlama ve tasarım
### [Sanal ağlar](../virtual-network/virtual-network-vnet-plan-design-arm.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Şirket içi ve dışı bağlantısı - VPN](../vpn-gateway/vpn-gateway-plan-design.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Şirket içi ve dışı bağlantısı - adanmış özel](../expressroute/expressroute-workflows.md?toc=%2fazure%2fnetworking%2ftoc.json)

##  Kavramlar
### [Sanal ağlar](../virtual-network/virtual-networks-overview.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Ağ yük dengeleme](../load-balancer/load-balancer-overview.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Uygulama yük dengeleme](../application-gateway/overview.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [DNS](../dns/dns-overview.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [DNS tabanlı trafik dağılımı](../traffic-manager/traffic-manager-overview.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Şirket içi bağlanma - VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Şirket içi bağlanma - adanmış](../expressroute/expressroute-introduction.md?toc=%2fazure%2fnetworking%2ftoc.json)


# başlarken
## [İlk sanal ağınızı oluşturma](../virtual-network/quick-create-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)

# Nasıl yapılır
## İnternet bağlantısı
### [Ağ yük dengelemesi ortak sunucuları](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Uygulama yük dengelemesi ortak sunucuları](../application-gateway/application-gateway-create-gateway-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Web uygulamalarını koruma](../application-gateway/application-gateway-web-application-firewall-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Konumlar arasında trafiği dağıtma](../traffic-manager/traffic-manager-configure-geographic-routing-method.md?toc=%2fazure%2fnetworking%2ftoc.json)
## İç bağlantı
### [Ağ yük dengelemesi özel sunucuları](../load-balancer/load-balancer-get-started-ilb-arm-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Uygulama yük dengelemesi özel sunucuları](../application-gateway/application-gateway-ilb-arm.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Sanal ağları bağlama (aynı konumlar)](../virtual-network/virtual-networks-create-vnetpeering-arm-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Sanal ağları bağlama (farklı konumlar)](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
## Şirket içi ve dışı bağlantısı
### [S2S VPN bağlantısı oluşturma (IPsec/IKE)](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [P2S VPN bağlantısı oluşturma (sertifikalara sahip SSTP)](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Adanmış özel bağlantı oluşturma (ExpressRoute)](../expressroute/expressroute-howto-circuit-portal-resource-manager.md?toc=%2fazure%2fnetworking%2ftoc.json)

## Yönetim
### [Ağ izlemeye genel bakış](network-monitoring-overview.md)
### [Azure sınırlarına göre kaynak kullanımını denetleme](check-usage-against-limits.md)
### [Ağ topolojisini görüntüleme](../network-watcher/network-watcher-topology-powershell.md?toc=%2fazure%2fnetworking%2ftoc.json)

## Örnek komut dosyaları
### [Azure CLI](cli-samples.md)
### [Azure PowerShell](powershell-samples.md)

## Öğreticiler
### [VM'ler için yük dengeleme](../virtual-machines/linux/tutorial-load-balancer.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Bilgisayarı sanal ağa bağlama](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)

# Başvuru
## [Azure CLI](https://docs.microsoft.com/cli/azure/network)
## [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.network/?view=azurermps-3.8.0)
## [.Net](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.network?view=azuremgmtnetwork-9.1.0-preview)
## [Node.js](https://azure.microsoft.com/develop/nodejs/#azure-sdk)
## [REST](https://msdn.microsoft.com/library/mt163658.aspx)

# Kaynaklar
## [Yazar şablonları](/azure/azure-resource-manager/resource-group-authoring-templates?toc=%2fazure%2fnetworking%2ftoc.json)
## [Azure Yol Haritası](https://azure.microsoft.com/roadmap/?category=networking)
## [Topluluk şablonları](https://azure.microsoft.com/resources/templates/)
## [Ağ blogu](http://azure.microsoft.com/blog/topics/networking)
## [Fiyatlandırma](https://azure.microsoft.com/pricing)
## [Fiyatlandırma hesaplayıcı](https://azure.microsoft.com/pricing/calculator/)
## [Bölgesel kullanılabilirlik](https://azure.microsoft.com/regions/services/)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-virtual-network)
## [Videolar](https://azure.microsoft.com/resources/videos/index/?services=virtual-network)

