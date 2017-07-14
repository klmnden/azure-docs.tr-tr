# Genel Bakış

## [Sanal ağlar](virtual-networks-overview.md)

## [Kullanıcı tanımlı yollar ve IP iletme](virtual-networks-udr-overview.md)

## [Sanal ağ eşleme](virtual-network-peering-overview.md)

## [İş sürekliliği](virtual-network-disaster-recovery-guidance.md)

## [SSS](virtual-networks-faq.md)

## [IP adresleme](virtual-network-ip-addresses-overview-arm.md)

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

#### [PowerShell](virtual-networks-create-vnet-arm-ps.md)

#### [CLI](virtual-networks-create-vnet-arm-cli.md)

#### [Şablon](virtual-networks-create-vnet-arm-template-click.md)

#### Klasik

##### [Portal](virtual-networks-create-vnet-classic-pportal.md)

##### [PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md)

##### [CLI](virtual-networks-create-vnet-classic-cli.md)


### Ağ güvenlik grupları

#### [Portal](virtual-networks-create-nsg-arm-pportal.md)

#### [PowerShell](virtual-networks-create-nsg-arm-ps.md)

#### [CLI](virtual-networks-create-nsg-arm-cli.md)

#### [Şablon](virtual-networks-create-nsg-arm-template.md)

#### Klasik

##### [PowerShell](virtual-networks-create-nsg-classic-ps.md)

##### [CLI](virtual-networks-create-nsg-classic-cli.md)


### Kullanıcı tanımlı yollar

#### [PowerShell](virtual-network-create-udr-arm-ps.md)

#### [CLI](virtual-network-create-udr-arm-cli.md)

#### [Şablon](virtual-network-create-udr-arm-template.md)

#### Klasik

##### [PowerShell](virtual-network-create-udr-classic-ps.md)

##### [CLI](virtual-network-create-udr-classic-cli.md)


### [Sanal ağ eşleme](virtual-network-create-peering.md)


### Sanal makineler

#### Statik genel IP adresiyle VM oluşturma

##### [Portal](virtual-network-deploy-static-pip-arm-portal.md)

##### [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)

##### [CLI](virtual-network-deploy-static-pip-arm-cli.md)

##### [Şablon](virtual-network-deploy-static-pip-arm-template.md)

##### Klasik

###### [PowerShell](virtual-networks-reserved-public-ip.md)


#### Statik özel IP adresiyle VM oluşturma

##### [Portal](virtual-networks-static-private-ip-arm-pportal.md)

##### [PowerShell](virtual-networks-static-private-ip-arm-ps.md)

##### [CLI](virtual-networks-static-private-ip-arm-cli.md)

##### Klasik

###### [Portal](virtual-networks-static-private-ip-classic-pportal.md)

###### [PowerShell](virtual-networks-static-private-ip-classic-ps.md)

###### [CLI](virtual-networks-static-private-ip-classic-cli.md)


#### Birden çok ağ arabirimi ile VM oluşturma

##### [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

##### [CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

##### Klasik

###### [PowerShell](virtual-network-deploy-multinic-classic-ps.md)

###### [CLI](virtual-network-deploy-multinic-classic-cli.md)


#### Birden çok IP adresi ile VM oluşturma

##### [Azure portal](virtual-network-multiple-ip-addresses-portal.md)

##### [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)

##### [CLI](virtual-network-multiple-ip-addresses-cli.md)

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

##### [NSG’ler ile basit DMZ](virtual-networks-dmz-nsg-asm.md)

##### [Güvenlik duvarı ve NSG’ler ile DMZ](virtual-networks-dmz-nsg-fw-asm.md)

##### [Güvenlik duvarı, UDR ve NSG’ler ile DMZ](virtual-networks-dmz-nsg-fw-udr-asm.md)

##### [Örnek uygulama](virtual-networks-sample-app.md)


## Yapılandırma

### Sanal makineler

#### [Ağ arabirimi ekleme veya kaldırma](virtual-network-network-interface-vm.md)

#### [VM’ler ve bulut hizmetleri için ad çözümlemesi](virtual-networks-name-resolution-for-vms-and-role-instances.md)

#### [Ağ aktarım hızını iyileştirme](virtual-network-optimize-network-bandwidth.md)

#### [Konak adlarını görüntüleme ve değiştirme](virtual-networks-viewing-and-modifying-hostnames.md)

### Klasik

#### Erişim denetimi listeleri

##### [Portal](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

##### [PowerShell](virtual-networks-acl-powershell.md)


## Yönet

### [Sanal ağlar](virtual-network-manage-network.md)

#### [Alt ağlar](virtual-network-manage-subnet.md)

#### [Eşlemeler](virtual-network-manage-peering.md)

#### Klasik

##### [Ağ yapılandırması dosyası](virtual-networks-using-network-configuration-file.md)

##### [Bir benzeşim grubundan bölgeye geçiş](virtual-networks-migrate-to-regional-vnet.md)

### Ağ güvenlik grupları

#### [Portal](virtual-network-manage-nsg-arm-portal.md)

#### [PowerShell](virtual-network-manage-nsg-arm-ps.md)

#### [CLI](virtual-network-manage-nsg-arm-cli.md)

#### [Günlükler](virtual-network-nsg-manage-log.md)

### Ağ arabirimleri (NIC’ler)

#### [NIC’leri oluşturma, değiştirme veya silme](virtual-network-network-interface.md)

#### [IP adreslerini ekleme, değiştirme veya kaldırma](virtual-network-network-interface-addresses.md)

### Sanal makineler

#### [VM’yi farklı bir alt ağa taşıma](virtual-networks-move-vm-role-to-subnet.md)

### [Genel IP adresleri](virtual-network-public-ip-address.md)


## Sorun giderme

### Ağ güvenlik grupları

#### [Portal](virtual-network-nsg-troubleshoot-portal.md)

#### [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)

### Yollar

#### [Portal](virtual-network-routes-troubleshoot-portal.md)

#### [PowerShell](virtual-network-routes-troubleshoot-powershell.md)

### [Aktarım hızı testi](virtual-network-bandwidth-testing.md)


# Başvuru

## [PowerShell (Resource Manager)](/powershell/module/azurerm.network)

## [PowerShell (Klasik)](/powershell/module/azure/)

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

## [Azure yol haritası](https://azure.microsoft.com/roadmap/)

## [Ağ blogu](http://azure.microsoft.com/blog/topics/networking)

## [Ağ forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesVirtualNetwork)

## [Fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-network)

## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-virtual-network)

