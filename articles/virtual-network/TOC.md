# Genel Bakış
## [Sanal ağlar](virtual-networks-overview.md)
## [Yönlendirme](virtual-networks-udr-overview.md)
## [Sanal ağ eşleme](virtual-network-peering-overview.md)
## [Sanal ağ hizmeti uç noktaları](virtual-network-service-endpoints-overview.md)
## [Azure hizmetleri için sanal ağ](virtual-network-for-azure-services.md)
## [Güvenlik](security-overview.md)
## [İş sürekliliği](virtual-network-disaster-recovery-guidance.md)
## [IP adresleme](virtual-network-ip-addresses-overview-arm.md)
## [DDoS Koruması](ddos-protection-overview.md)
## [SSS](virtual-networks-faq.md)
## Klasik
### [IP adresleme](virtual-network-ip-addresses-overview-classic.md)
### [Erişim denetim listeleri](virtual-networks-acl.md)

# Kullanmaya Başlama
## [İlk sanal ağınızı oluşturma](virtual-network-get-started-vnet-subnet.md)

# Nasıl yapılır
## Planlama ve tasarım
### [Sanal ağlar](virtual-network-vnet-plan-design-arm.md)
### [Ağ güvenlik grupları](virtual-networks-nsg.md)

## Dağıtma
### [Sanal ağlar](virtual-networks-create-vnet-arm-pportal.md)
#### [Azure PowerShell](virtual-networks-create-vnet-arm-ps.md)
#### [Azure CLI 2.0](virtual-networks-create-vnet-arm-cli.md)
#### [Azure CLI 1.0](virtual-networks-create-vnet-cli-nodejs.md)
#### [Şablon](virtual-networks-create-vnet-arm-template-click.md)

### Ağ güvenlik grupları
#### [Azure portal](virtual-networks-create-nsg-arm-pportal.md)
#### [Azure PowerShell](virtual-networks-create-nsg-arm-ps.md)
#### [Azure CLI 2.0](virtual-networks-create-nsg-arm-cli.md)
#### [Azure CLI 1.0](virtual-networks-create-nsg-cli-nodejs.md)
#### [Şablon](virtual-networks-create-nsg-arm-template.md)
#### [Uygulama güvenliği grupları](create-network-security-group-preview.md)
#### Klasik
##### [Azure PowerShell](virtual-networks-create-nsg-classic-ps.md)
##### [Azure CLI 1.0](virtual-networks-create-nsg-classic-cli.md)

### Kullanıcı tanımlı yollar
#### [Azure portal](create-user-defined-route-portal.md)
#### [Azure PowerShell](virtual-network-create-udr-arm-ps.md)
#### [Azure CLI 2.0](virtual-network-create-udr-arm-cli.md)
#### [Azure CLI 1.0](virtual-network-create-udr-arm-cli-nodejs.md)
#### [Şablon](virtual-network-create-udr-arm-template.md)
#### Klasik
##### [Azure PowerShell](virtual-network-create-udr-classic-ps.md)
##### [Azure CLI](virtual-network-create-udr-classic-cli.md)

### Sanal ağ eşleme
#### [Aynı dağıtım modeli - aynı abonelik](virtual-network-create-peering.md)
#### [Aynı dağıtım modeli - farklı abonelikler](create-peering-different-subscriptions.md)
#### [Farklı dağıtım modelleri - aynı abonelik](create-peering-different-deployment-models.md)
#### [Farklı dağıtım modelleri - farklı abonelikler](create-peering-different-deployment-models-subscriptions.md)

### [Sanal ağ hizmeti uç noktaları](virtual-network-service-endpoints-configure.md)

### Genel IP adresi - Kullanılabilirlik alanı
#### [Azure portal](create-public-ip-availability-zone-portal.md)
#### [Azure CLI](create-public-ip-availability-zone-cli.md)
#### [PowerShell](create-public-ip-availability-zone-powershell.md)

### Sanal makineler
#### Statik genel IP adresiyle VM oluşturma
##### [Azure portal](virtual-network-deploy-static-pip-arm-portal.md)
##### [Azure PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
##### [Azure CLI 2.0](virtual-network-deploy-static-pip-arm-cli.md)
##### [Azure CLI 1.0](virtual-network-deploy-static-pip-cli-nodejs.md)
##### [Şablon](virtual-network-deploy-static-pip-arm-template.md)
##### Klasik
###### [Azure PowerShell](virtual-networks-reserved-public-ip.md)

#### Statik özel IP adresiyle VM oluşturma
##### [Azure portal](virtual-networks-static-private-ip-arm-pportal.md)
##### [Azure PowerShell](virtual-networks-static-private-ip-arm-ps.md)
##### [Azure CLI](virtual-networks-static-private-ip-arm-cli.md)
##### Klasik
###### [Azure portal](virtual-networks-static-private-ip-classic-pportal.md)
###### [Azure PowerShell](virtual-networks-static-private-ip-classic-ps.md)
###### [Azure CLI](virtual-networks-static-private-ip-classic-cli.md)

#### Birden çok ağ arabirimi ile VM oluşturma
##### [Azure PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [Azure CLI 2.0](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [Azure CLI 1.0](../virtual-machines/linux/multiple-nics-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [Şablon](virtual-network-deploy-multinic-arm-template.md)

##### Klasik
###### [Azure PowerShell](virtual-network-deploy-multinic-classic-ps.md)
###### [Azure CLI](virtual-network-deploy-multinic-classic-cli.md)

#### Birden çok IP adresi ile VM oluşturma
##### [Azure portal](virtual-network-multiple-ip-addresses-portal.md)
##### [Azure PowerShell](virtual-network-multiple-ip-addresses-powershell.md)
##### [Azure CLI 2.0](virtual-network-multiple-ip-addresses-cli.md)
##### [Azure CLI 1.0](virtual-network-multiple-ip-addresses-cli-nodejs.md)
##### [Şablon](virtual-network-multiple-ip-addresses-template.md)

#### [Hızlandırılmış ağ ile bir VM oluşturma](virtual-network-create-vm-accelerated-networking.md)

### Bağlantı senaryoları
#### [Sanal ağdan (VNet) Sanal ağa](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [Sanal ağdan (Resource Manager) Sanal ağa (Klasik)](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [Sanal ağdan şirket içi ağa (VPN)](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [Sanal ağdan şirket içi ağa (ExpressRoute)](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [Yüksek oranda kullanılabilir karma ağ mimarisi](../guidance/guidance-hybrid-network-expressroute-vpn-failover.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

### Güvenlik senaryoları
#### [Sanal gereçlerle ağ güvenliği sağlama](virtual-network-scenario-udr-gw-nva.md)
#### [Azure ve İnternet arasında DMZ](../guidance/guidance-iaas-ra-secure-vnet-dmz.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [Bulut hizmeti ve ağ güvenliği](../best-practices-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [NSG’ler ile DMZ oluşturma](virtual-networks-dmz-nsg.md)
##### [NSG’ler ile DMZ oluşturma (Klasik)](virtual-networks-dmz-nsg-asm.md)
##### [Güvenlik duvarı ve NSG’ler ile DMZ oluşturma (Klasik)](virtual-networks-dmz-nsg-fw-asm.md)
##### [Güvenlik duvarı, UDR ve NSG’ler ile DMZ (Klasik)](virtual-networks-dmz-nsg-fw-udr-asm.md)

##### [Örnek uygulama](virtual-networks-sample-app.md)

### Klasik
#### [Sanal ağ](create-virtual-network-classic.md)
##### [Azure portal](virtual-networks-create-vnet-classic-pportal.md)
##### [Azure PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md)
##### [Azure CLI](virtual-networks-create-vnet-classic-cli.md)
#### [Bir sanal ağ yapılandırma dosyasında DNS ayarlarını belirtme](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)
#### [Bir hizmet yapılandırma dosyasında DNS ayarlarını belirleme](virtual-networks-specifying-dns-settings-in-a-service-configuration-file.md)

## Yapılandırma
### Sanal makineler
#### [Ağ arabirimi ekleme veya kaldırma](virtual-network-network-interface-vm.md)
#### [VM’ler ve bulut hizmetleri için ad çözümlemesi](virtual-networks-name-resolution-for-vms-and-role-instances.md)
#### [Kendi DNS sunucunuzda ana bilgisayar adlarını kaydetmek için dinamik DNS kullanma](virtual-networks-name-resolution-ddns.md)
#### [Ağ aktarım hızını iyileştirme](virtual-network-optimize-network-bandwidth.md)
#### [Konak adlarını görüntüleme ve değiştirme](virtual-networks-viewing-and-modifying-hostnames.md)
#### Klasik
##### Statik IP adresleri
###### [PowerShell](virtual-networks-reserved-private-ip.md)
###### [CLI](virtual-networks-static-private-ip-cli-nodejs.md)
##### [Örnek düzeyinde ortak IP adresi](virtual-networks-instance-level-public-ip.md)

### Klasik
#### Erişim denetimi listeleri
##### [Azure portal](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [Azure PowerShell](virtual-networks-acl-powershell.md)

## Yönet
### [Sanal ağlar](virtual-network-manage-network.md)
#### [Alt ağlar](virtual-network-manage-subnet.md)
#### [Eşlemeler](virtual-network-manage-peering.md)
#### Klasik
##### [Ağ yapılandırması dosyası](virtual-networks-using-network-configuration-file.md)
##### [Bir benzeşim grubundan bölgeye geçiş](virtual-networks-migrate-to-regional-vnet.md)
### Ağ güvenlik grupları
#### [Azure portal](virtual-network-manage-nsg-arm-portal.md)
#### [Azure PowerShell](virtual-network-manage-nsg-arm-ps.md)
#### [Azure CLI 2.0](virtual-network-manage-nsg-arm-cli.md)
#### [Azure CLI 1.0](virtual-network-manage-nsg-cli-nodejs.md)

#### [Günlükler](virtual-network-nsg-manage-log.md)
### Ağ arabirimleri (NIC’ler)
#### [NIC’leri oluşturma, değiştirme veya silme](virtual-network-network-interface.md)
#### [IP adreslerini ekleme, değiştirme veya kaldırma](virtual-network-network-interface-addresses.md)
### Sanal makineler
#### [VM’yi farklı bir alt ağa taşıma](virtual-networks-move-vm-role-to-subnet.md)
### [Genel IP adresleri](virtual-network-public-ip-address.md)
### DDOS Koruması
#### [Azure portal](ddos-protection-manage-portal.md)
#### [Azure PowerShell](ddos-protection-manage-ps.md)

## Sorun giderme
### Ağ güvenlik grupları
#### [Azure portal](virtual-network-nsg-troubleshoot-portal.md)
#### [Azure PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
### Yollar
#### [Azure portal](virtual-network-routes-troubleshoot-portal.md)
#### [Azure PowerShell](virtual-network-routes-troubleshoot-powershell.md)
### [Aktarım hızı testi](virtual-network-bandwidth-testing.md)
### [Sanal ağlar silinemiyor](virtual-network-troubleshoot-cannot-delete-vnet.md)
### [VM’den VM’ye bağlantı sorunları](virtual-network-troubleshoot-connectivity-problem-between-vms.md)

# Başvuru
## [Kod örnekleri](https://azure.microsoft.com/en-us/resources/samples/?service=virtual-network)
## [Azure PowerShell (Resource Manager)](/powershell/module/azurerm.network)
## [Azure PowerShell (Klasik)](/powershell/module/azure/)
## [Azure CLI](/cli/azure/network)
## [Java](/java/api/)
## [REST (Resource Manager)](https://msdn.microsoft.com/library/mt163658.aspx)
## [REST (Klasik)](https://msdn.microsoft.com/library/jj157182.aspx)


# İlgili
## [Sanal Makineler](/azure/virtual-machines/)
## [Application Gateway](/azure/application-gateway/)
## [Azure DNS](/azure/dns/)
## [Traffic Manager](/azure/traffic-manager/)
## [Yük Dengeleyici](/azure/load-balancer/)
## [VPN Gateway](/azure/vpn-gateway/)
## [ExpressRoute](/azure/expressroute/)

# Kaynaklar
## [Azure yol haritası](https://azure.microsoft.com/roadmap/?category=networking)
## [Ağ blogu](http://azure.microsoft.com/blog/topics/networking)
## [Ağ forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesVirtualNetwork)
## [Fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-network)
## [Fiyatlandırma hesaplayıcı](https://azure.microsoft.com/pricing/calculator/)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-virtual-network)
## [Ağ kaynağı sağlayıcı](resource-groups-networking.md)
