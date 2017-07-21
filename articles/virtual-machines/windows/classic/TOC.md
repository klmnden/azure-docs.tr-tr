# Genel Bakış
## [Sanal makineler hakkında](../../virtual-machines-windows-about.md)
## [Diskler ve VHD’ler](../../../storage/storage-about-disks-and-vhds-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
## [Sanal Ağlar](../../../virtual-network/virtual-networks-overview.md)
## [SSS](faq.md)
## [VM'leri, web sitelerini ve bulut hizmetlerini karşılaştırma](../../../app-service-web/choose-web-site-cloud-service-vm.md)
## [Kapsayıcılar](../../virtual-machines-windows-containers.md)

# Başlarken
## [Portalı kullanarak VM oluşturma](tutorial.md)
## [VM’de oturum açma](connect-logon.md)
## [Azure PowerShell’i yükleme](/powershell/azure/overview)
## [Azure CLI'yı yükleme](../../../cli-install-nodejs.md)

# Nasıl yapılır?

## Depolama'yı kullanma
### [Veri diski ekleme](attach-disk.md)
### [Veri diski çıkarma](detach-disk.md)
### [D: sürücüsünü veri diski olarak kullanma](../../virtual-machines-windows-change-drive-letter.md)

## Ağ
### [Uç noktaları ayarlama](setup-endpoints.md)
### [VNET veya Bulut Hizmeti ile VM’leri bağlama](connect-vms.md)
### [Klasik sanal ağları Resource Manager sanal ağlarına bağlama](../../../vpn-gateway/vpn-gateway-connect-different-deployment-models-powershell.md)
### [Yük dengeleyici oluşturma](../../../load-balancer/load-balancer-get-started-internet-classic-portal.md)
### [Azure PowerShell kullanarak NSG’leri yönetme](../../../virtual-network/virtual-networks-create-nsg-classic-ps.md)

## Dağıtma
### [Özel VM oluşturma](createportal.md)
### [Azure PowerShell kullanarak bir VM oluşturma ve yapılandırma](create-powershell.md)
### [Bir Windows sanal makinesini yakalama](capture-image.md)
### [PowerShell'i kullanarak bir VHD oluşturma ve karşıya yükleme](createupload-vhd.md)
### [Chef ile Azure VM dağıtımını otomatikleştirme](../../virtual-machines-windows-chef-automation.md)
### [Visual Studio'da VM Oluşturma ve Yönetme](manage-visual-studio.md)
### [Visual Studio ile bir web uygulaması için VM oluşturma](web-app-visual-studio.md)
### [Java'da yoğun işlem gücü kullanımlı görev çalıştırma](java-run-compute-intensive-task.md)
### [Django Hello World web uygulaması](python-django-web-app.md)

## Yapılandırma
### [Parolayı veya Uzak Masaüstü hizmetini sıfırlama](../../virtual-machines-windows-reset-rdp.md)
### [Symantec Endpoint Protection'ı yükleme ve yapılandırma](install-symantec.md)
### [Hizmet Olarak Trend Micro Deep Security'yi yükleme ve yapılandırma](install-trend.md)
### [Kullanılabilirlik kümesi yapılandırma](configure-availability.md)
### [Windows Klasik dağıtım modelinde oluşturulan bir VM'yi yeniden boyutlandırma](resize-vm.md)
### [Bakım](planned-maintenance-schedule.md)

## Yönet
### [Klasikten Resource Manager’a Geçiş](../../virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
### [Azure PowerShell kullanarak sanal makinelerinizi yönetme](manage-psh.md)
### [VM aracısı ve uzantıları hakkında](agents-and-extensions.md)
### [VM uzantılarını yönetme](manage-extensions.md)
### [Sanal makineler için Özel Betik uzantısı](extensions-customscript.md)
### [Azure VM'sine özel veri ekleme](inject-custom-data.md)

## Planlama
### [Görüntüler hakkında](about-images.md)
### [VM boyutları](../../virtual-machines-windows-sizes.md)
### [Azure VM’lerinde planlı bakım](../../virtual-machines-windows-planned-maintenance.md)
### [Azure altyapı hizmetleri uygulama yönergeleri](../../virtual-machines-windows-infrastructure-subscription-accounts-guidelines.md)

## İş yüklerini yönetme
### [Yüksek Performanslı Bilgi İşlem (HPC)](../../virtual-machines-windows-hpcpack-cluster-options.md)
#### [Kaynakları otomatik olarak ölçeklendirme](hpcpack-cluster-node-autogrowshrink.md)
#### [İşlem düğümlerini yönetme](hpcpack-cluster-node-manage.md)
#### [Küme oluşturma](hpcpack-cluster-powershell-script.md)
#### [MPI uygulamaları çalıştırmak için bir küme oluşturma](hpcpack-rdma-cluster.md)
#### [Excel ve SOA iş yükleri çalıştırma](../../virtual-machines-windows-excel-cluster-hpcpack.md)
#### [Market görüntüsü ile baş düğümünü oluşturma](../../virtual-machines-windows-hpcpack-cluster-headnode.md)
#### [Şirket içinden Azure'a iş gönderme](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md)
### [MongoDB](install-mongodb.md)
### [MySQL](mysql-2008r2.md)
### [Oracle](../../workloads/oracle/oracle-considerations.md)
### [SAP](sap-get-started.md)
### [SQL Server](../sql/virtual-machines-windows-sql-server-iaas-overview.md)
### [Tomcat](java-run-tomcat-app-server.md)

## Sorun giderme
### [Uzak Masaüstü bağlantıları](../../virtual-machines-windows-troubleshoot-rdp-connection.md)
####[Uzak masaüstü bağlantısı sorunlarına yönelik ayrıntılı sorun giderme adımları](../../virtual-machines-windows-detailed-troubleshoot-rdp.md)
### [Bir uygulamaya erişme](../../virtual-machines-windows-troubleshoot-app-connection.md)
### [Yeni bir VM oluşturmayla ilgili klasik dağıtım sorunları](troubleshoot-deployment-new-vm.md)
### [Mevcut bir VM'yi yeniden başlatma veya yeniden boyutlandırmayla ilgili klasik dağıtım sorunları](virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)
## [RDP parolasını sıfırlama](reset-rdp.md)

# Başvuru
## [PowerShell](/powershell/azure/overview)
## [Azure CLI](/cli/azure/vm)
## [Java](/java/api)
## [.NET](/dotnet/api/microsoft.azure.management.compute)
## [Resource Manager şablonları yazma](../../../resource-group-authoring-templates.md)
## [Topluluk şablonları](https://azure.microsoft.com/documentation/templates)
## [İşlem REST](/rest/api/compute)
## [Ağ REST](/rest/api)
## [Depolama REST](/rest/api/storageservices)

# Kaynaklar
## [Azure Yol Haritası](https://azure.microsoft.com/roadmap/)
## [Fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)
## [Bölgesel kullanılabilirlik](https://azure.microsoft.com/regions/services/)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-virtual-machine)
## [Videolar](https://azure.microsoft.com/documentation/videos/index/?services=virtual-machines)
