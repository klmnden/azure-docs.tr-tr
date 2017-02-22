# Genel Bakış
## [Site Recovery nedir?](site-recovery-overview.md)
## [Site Recovery nasıl çalışır?](site-recovery-components.md)
## [Hangi iş yüklerini korur?](site-recovery-workload.md)
## [Site Recovery destek matrisi](site-recovery-support-matrix.md)
## [SSS](site-recovery-faq.md)
## [Tanıtımı izleyin](https://www.youtube.com/watch?v=eOOwMQPBKfM)

# Kullanmaya Başlama
## [VMWare VM'lerini Azure'a çoğaltma](site-recovery-vmware-to-azure.md)
## [VMware VM’lerini çok kiracılı bir dağıtımda (CSP) Azure’a dağıtma](site-recovery-multi-tenant-support-vmware-using-csp.md)
## [Hyper-V VM’lerini Azure’a çoğaltma (VMM ile)](site-recovery-vmm-to-azure.md)
## [Hyper-V VM'lerini Azure'a çoğaltma](site-recovery-hyper-v-site-to-azure.md)
## [VMware VM’lerini ve fiziksel sunucuları ikincil bir siteye çoğaltma](site-recovery-vmware-to-vmware.md)
## [Hyper-V VM'lerini ikincil bir siteye çoğaltma (VMM ile)](site-recovery-vmm-to-vmm.md)

# Nasıl yapılır?
## Planlama
### [Dağıtım önkoşulları](site-recovery-prereq.md)
### [Ağ altyapısı ile ilgili önemli noktalar](site-recovery-network-design.md)
### [Site Recovery Capacity Planner’ı kullanma](site-recovery-capacity-planner.md)
### [Kapasite planlama ve Azure'aa VMware çoğaltma işlemini ölçeklendirme](site-recovery-plan-capacity-vmware.md)
## Yapılandırma
### [Kaynak çoğaltma ortamını ayarlama](site-recovery-set-up-vmware-to-azure.md)
### [Çoğaltma ayarlarını yapılandırma](site-recovery-setup-replication-settings-vmware.md)
### [VMware çoğaltma için Mobility hizmetini dağıtma](site-recovery-vmware-to-azure-install-mob-svc.md)
#### [System Center Configuration Manager kullanarak Mobility hizmetini dağıtma](site-recovery-install-mobility-service-using-sccm.md)
#### [Azure Automation DSC kullanarak Mobility hizmetini dağıtma](site-recovery-automate-mobility-service-install.md)
### [Sunucuları kaldırma ve korumayı devre dışı bırakma](site-recovery-manage-registration-and-protection.md)
## Geçiş
### [Azure’a geçiş](site-recovery-migrate-to-azure.md)
### [Azure bölgeleri arasında geçiş yapma](site-recovery-migrate-azure-to-azure.md)
### [AWS Windows örneklerini Azure’a geçirme](site-recovery-migrate-aws-to-azure.md)
## İş yükleri
### [Active Directory ve DNS](site-recovery-active-directory.md)
### [SQL Server](site-recovery-sql.md)
### [SharePoint](site-recovery-workload.md#protect-sharepoint)
### [Dynamics AX](site-recovery-workload.md#protect-dynamics-ax)
### [RDS](site-recovery-workload.md#protect-rds)
### [Exchange](site-recovery-workload.md#protect-exchange)
### [SAP](site-recovery-workload.md#protect-sap)
### [Diğer iş yükleri](site-recovery-workload.md#workload-summary)
## Çoğaltmayı otomatik hale getirme
### [Azure’a Hyper-V çoğaltma işlemini (VMM olmadan) otomatikleştirme](site-recovery-deploy-with-powershell-resource-manager.md)
### [Azure’a Hyper-V çoğaltma işlemini (VMM ile) otomatikleştirme](site-recovery-vmm-to-azure-powershell-resource-manager.md)
### [İkincil siteye Hyper-V çoğaltma işlemini (VMM ile) otomatikleştirme](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
## Yük devretme ve yeniden çalışma
### [Site Recovery'de yük devretme](site-recovery-failover.md)
### [Kurtarma planları oluşturma](site-recovery-create-recovery-plans.md)
#### [Kurtarma planlarına Azure runbook ekleme](site-recovery-runbook-automation.md)
### [VMware’den Azure'a yük devretme testi çalıştırma](site-recovery-test-failover-to-azure.md)
### [İki VMM sitesi arasında yük devretme testi çalıştırma](site-recovery-test-failover-vmm-to-vmm.md)
### [VMware VM’lerini ve fiziksel sunucuları ilk durumuna geri döndürme](site-recovery-failback-azure-to-vmware.md)
## [İzleme ve sorun giderme](site-recovery-monitoring-and-troubleshooting.md)

# Başvuru
## [PowerShell](/powershell/resourcemanager/azurerm.siterecovery/v3.2.0/azurerm.siterecovery)
## [PowerShell klasik](/powershell/servicemanagement/azure.siterecovery/v3.1.0/azure.siterecovery)
## [REST](https://msdn.microsoft.com/en-us/library/mt750497)

# İlgili
## [Azure Otomasyonu](/azure/automation/)

# Kaynaklar
## [Öğrenme yolu](https://azure.microsoft.com/documentation/learning-paths/site-recovery/)
## [Forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hypervrecovmgr)
## [Blog](http://azure.microsoft.com/blog/tag/azure-site-recovery/)
## [Fiyatlandırma](https://azure.microsoft.com/pricing/details/site-recovery/)
## [Hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?product=site-recovery)


<!--HONumber=Feb17_HO2-->


