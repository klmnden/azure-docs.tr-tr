# Genel Bakış
## [Site Recovery nedir?](site-recovery-overview.md)
## Site Recovery nasıl çalışır?
### [Azure-Azure arası mimari](site-recovery-azure-to-azure-architecture.md)
### [Hyper-V-Azure arası mimari](site-recovery-architecture-hyper-v-to-azure.md)
### [İkincil bir siteye mimarisine çoğaltma](site-recovery-architecture-to-secondary-site.md)
## [Hangi iş yüklerini korur?](site-recovery-workload.md)
## Site Recovery destek matrisi
### [Azure-Azure arası destek](site-recovery-support-matrix-azure-to-azure.md)
### [Şirket içi-Azure arası destek](site-recovery-support-matrix-to-azure.md)
### [Şirket içi-ikincil site arası destek](site-recovery-support-matrix-to-sec-site.md)
## [SSS](site-recovery-faq.md)
## [Tanıtımı izleyin](https://azure.microsoft.com/resources/videos/index/?services=site-recovery)

# Kullanmaya Başlama
## [Azure VM’lerini çoğaltma (önizleme)](site-recovery-azure-to-azure.md)
## [VMware VM'lerini Azure'a çoğaltma](vmware-walkthrough-overview.md)
### [1. Adım: Mimariyi gözden geçirme](vmware-walkthrough-architecture.md)
### [2. Adım: Önkoşulları ve sınırlamaları gözden geçirme](vmware-walkthrough-prerequisites.md)
### [3. Adım: Kapasiteyi planlama](vmware-walkthrough-capacity.md)
### [4. Adım: Ağı planlama](vmware-walkthrough-network.md)
### [5. Adım: Azure’u hazırlama](vmware-walkthrough-prepare-azure.md)
### [6. Adım: VMware’i hazırlama](vmware-walkthrough-prepare-vmware.md)
### [7. Adım: Kasa oluşturma](vmware-walkthrough-create-vault.md)
### [8. Adım: Kaynağı ve hedefi ayarlama](vmware-walkthrough-source-target.md)
### [9. Adım: Çoğaltma ilkesi oluşturma](vmware-walkthrough-replication.md)
### [10. Adım: Mobility hizmetini yükleme](vmware-walkthrough-install-mobility.md)
### [11. Adım: Çoğaltmayı etkinleştirme](vmware-walkthrough-enable-replication.md)
### [12. Adım: Yük devretme testi çalıştırma](vmware-walkthrough-test-failover.md)
## [Hyper-V VM'lerini Azure'a çoğaltma](hyper-v-site-walkthrough-overview.md)
### [1. Adım: Mimariyi gözden geçirme](hyper-v-site-walkthrough-architecture.md)
### [2. Adım: Önkoşulları ve sınırlamaları gözden geçirme](hyper-v-site-walkthrough-prerequisites.md)
### [3. Adım: Kapasiteyi planlama](hyper-v-site-walkthrough-capacity.md)
### [4. Adım: Ağı planlama](hyper-v-site-walkthrough-network.md)
### [5. Adım: Azure’u hazırlama](hyper-v-site-walkthrough-prepare-azure.md)
### [6. Adım: Hyper-V konaklarını hazırlama](hyper-v-site-walkthrough-prepare-hyper-v.md)
### [7. Adım: Kasa oluşturma](hyper-v-site-walkthrough-create-vault.md)
### [8. Adım: Kaynağı ve hedefi ayarlama](hyper-v-site-walkthrough-source-target.md)
### [9. Adım: Çoğaltma ilkesi oluşturma](hyper-v-site-walkthrough-replication.md)
### [10. Adım: Çoğaltmayı etkinleştirme](hyper-v-site-walkthrough-enable-replication.md)
### [11. Adım: Yük devretme testi çalıştırma](hyper-v-site-walkthrough-test-failover.md)
## [Hyper-V VM’lerini Azure’a çoğaltma (VMM ile)](site-recovery-vmm-to-azure.md)
## [Fiziksel sunucuları Azure'a çoğaltma](physical-walkthrough-overview.md)
### [1. Adım: Mimariyi gözden geçirme](physical-walkthrough-architecture.md)
### [2. Adım: Önkoşulları ve sınırlamaları gözden geçirme](physical-walkthrough-prerequisites.md)
### [3. Adım: Kapasiteyi planlama](physical-walkthrough-capacity.md)
### [4. Adım: Ağı planlama](physical-walkthrough-network.md)
### [5. Adım: Azure’u hazırlama](physical-walkthrough-prepare-azure.md)
### [6. Adım: Kasa oluşturma](physical-walkthrough-create-vault.md)
### [7. Adım: Kaynağı ve hedefi ayarlama](physical-walkthrough-source-target.md)
### [8. Adım: Çoğaltma ilkesi oluşturma](physical-walkthrough-replication.md)
### [9. Adım: Mobility hizmetini yükleme](physical-walkthrough-install-mobility.md)
### [10. Adım: Çoğaltmayı etkinleştirme](physical-walkthrough-enable-replication.md)
### [11. Adım: Yük devretme testi çalıştırma](physical-walkthrough-test-failover.md)
## [Hyper-V VM'lerini ikincil bir siteye çoğaltma (VMM ile)](site-recovery-vmm-to-vmm.md)
## [VMware VM’lerini ve fiziksel sunucuları ikincil bir siteye çoğaltma](site-recovery-vmware-to-vmware.md)
## [VMware VM’lerini çok kiracılı bir dağıtımda (CSP) Azure’a dağıtma](site-recovery-multi-tenant-support-vmware-using-csp.md)

# Nasıl yapılır
## Planlama
### [Azure çoğaltma için önkoşullar](site-recovery-azure-to-azure-prereq.md)
### Ağı planlama
#### [Azure-Azure arası çoğaltma için ağı planlama (önizleme)](site-recovery-azure-to-azure-networking-guidance.md)
#### [Şirket içi makinede çoğaltma için ağı planlama](site-recovery-network-design.md)
#### [Azure VM çoğaltması için ağ eşlemesini planlama](site-recovery-network-mapping-azure-to-azure.md)
#### [Hyper-V VM çoğaltması için ağ eşlemesini planlama](site-recovery-network-mapping.md)
### Kapasite ve ölçeklenebilirlik planlaması
#### [Azure’a VMware çoğaltması için kapasite planlama](site-recovery-plan-capacity-vmware.md)
#### [Azure’a VMware çoğaltması için Dağıtım Planlayıcısı](site-recovery-deployment-planner.md)
#### [Hyper-V çoğaltması için Capacity Planner](site-recovery-capacity-planner.md)
### [VM çoğaltma için rol tabanlı erişimi planlama](site-recovery-role-based-linked-access-control.md)
## Yapılandırma
### Kaynak ortamı ayarlama
#### [VMware-Azure arası için kaynak ortam](site-recovery-set-up-vmware-to-azure.md)
#### [Fiziksel ortam-Azure arası için kaynak ortam](site-recovery-set-up-physical-to-azure.md)
### Hedef ortamı ayarlama
#### [VMware-Azure arası için hedef ortam](site-recovery-prepare-target-vmware-to-azure.md)
#### [Fiziksel ortam-Azure arası için hedef ortam](site-recovery-prepare-target-physical-to-azure.md)
### [Çoğaltma ayarlarını yapılandırma](site-recovery-setup-replication-settings-vmware.md)
### [VMware çoğaltma için Mobility hizmetini dağıtma](site-recovery-vmware-to-azure-install-mob-svc.md)
#### [System Center Configuration Manager ile Mobility hizmetini dağıtma](site-recovery-install-mobility-service-using-sccm.md)
#### [Azure Otomasyonu DSC ile Mobility hizmetini dağıtma](site-recovery-automate-mobility-service-install.md)
### Çoğaltmayı etkinleştirme
#### [Azure-Azure arası çoğaltmayı etkinleştirme](site-recovery-replicate-azure-to-azure.md)
#### [VMware-Azure arası çoğaltmayı etkinleştirme](site-recovery-replicate-vmware-to-azure.md)
## Yük devretme ve ilk duruma döndürme
### [Kurtarma planları oluşturma](site-recovery-create-recovery-plans.md)
#### [Kurtarma planlarına Azure runbook ekleme](site-recovery-runbook-automation.md)
### Yük devretme testi çalıştırma
#### [Azure’a yük devretme testi çalıştırma](site-recovery-test-failover-to-azure.md)
#### [VMM bulutları arasında yük devretme testi çalıştırma](site-recovery-test-failover-vmm-to-vmm.md)
### [Yük devretme korumalı makineler](site-recovery-failover.md)
### Yük devretme işleminden sonra makineleri yeniden koruma
#### [Bir Azure ikincil bölgesinden birincil bölgeye yeniden koruma](site-recovery-how-to-reprotect-azure-to-azure.md)
#### [Azure’dan şirket içi ortama yeniden koruma](site-recovery-how-to-reprotect.md)
### Azure’dan yeniden çalışma
#### [Azure’dan VMware’e yeniden çalışma](site-recovery-failback-azure-to-vmware.md)
#### [Azure’dan Hyper-V’ye yeniden çalışma](site-recovery-failback-from-azure-to-hyper-v.md)
## Geçiş
### [Azure’a geçiş](site-recovery-migrate-to-azure.md)
### [Azure bölgeleri arasında geçiş yapma](site-recovery-migrate-azure-to-azure.md)
### [AWS Windows örneklerini Azure’a geçirme](site-recovery-migrate-aws-to-azure.md)
### [Geçirilen makineleri başka bir Azure bölgesine çoğaltma](site-recovery-azure-to-azure-after-migration.md)
## İş yükleri
### [Active Directory ve DNS](site-recovery-active-directory.md)
### [SQL Server çoğaltma](site-recovery-sql.md)
### [SharePoint](site-recovery-sharepoint.md)
### [Dynamics AX](site-recovery-dynamicsax.md)
### [RDS](site-recovery-workload.md#protect-rds)
### [Exchange](site-recovery-workload.md#protect-exchange)
### [SAP](site-recovery-workload.md#protect-sap)
### [IIS tabanlı web uygulamaları](site-recovery-iis.md)
### [Citrix XenApp ve XenDesktop](site-recovery-citrix-xenapp-and-xendesktop.md)
### [Diğer iş yükleri](site-recovery-workload.md#workload-summary)
## Çoğaltmayı otomatik hale getirme
### [Azure’a Hyper-V çoğaltma işlemini (VMM olmadan) otomatikleştirme](site-recovery-deploy-with-powershell-resource-manager.md)
### [Azure’a Hyper-V çoğaltma işlemini (VMM ile) otomatikleştirme](site-recovery-vmm-to-azure-powershell-resource-manager.md)
### [İkincil siteye Hyper-V çoğaltma işlemini (VMM ile) otomatikleştirme](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
## Yönet
### [Azure’da işlem sunucularını yönetme](site-recovery-vmware-setup-azure-ps-resource-manager.md)
### [Yapılandırma sunucusunu yönetme](site-recovery-vmware-to-azure-manage-configuration-server.md)
### [Ölçekli işlem sunucularını yönetme](site-recovery-vmware-to-azure-manage-scaleout-process-server.md)
### [vCenter sunucularını yönetme](site-recovery-vmware-to-azure-manage-vCenter.md)
### [Sunucuları kaldırma ve korumayı devre dışı bırakma](site-recovery-manage-registration-and-protection.md)

## İzleme ve sorun giderme
### [Azure-Azure arası çoğaltma sorunları](site-recovery-azure-to-azure-troubleshoot-errors.md)
### [Şirket içinden Azure’a çoğaltma sorunları](site-recovery-vmware-to-azure-protection-troubleshoot.md)
### [Günlükleri toplama ve şirket içi ortam sorunlarını giderme](site-recovery-monitoring-and-troubleshooting.md)

# Başvuru
## [PowerShell](/powershell/module/azurerm.siterecovery)
## [PowerShell klasik](/powershell/module/azure/?view=azuresmps-3.7.0)
## [REST](https://msdn.microsoft.com/en-us/library/mt750497)

# İlgili
## [Azure Otomasyonu](/azure/automation/)

# Kaynaklar
## [Azure Yol Haritası](https://azure.microsoft.com/roadmap/)
## [Blog](http://azure.microsoft.com/blog/tag/azure-site-recovery/)
## [Forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hypervrecovmgr)
## [Öğrenme yolu](https://azure.microsoft.com/documentation/learning-paths/site-recovery/)
## [Fiyatlandırma](https://azure.microsoft.com/pricing/details/site-recovery/)
## [Hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?product=site-recovery)
