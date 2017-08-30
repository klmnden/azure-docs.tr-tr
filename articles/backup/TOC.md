
# Genel Bakış
## [Azure Backup nedir?](backup-introduction-to-azure-backup.md)

# Başlarken
## [Azure VM'lerini yedekleme](backup-azure-vms-first-look-arm.md)
## [Windows Server veya Windows bilgisayarlarını yedekleme](backup-try-azure-backup-in-10-mins.md)
## [VMware sunucularını yedekleme](backup-azure-backup-server-vmware.md)

# Nasıl yapılır?

## Azure Backup Sunucusu
### [Azure Backup Sunucusu koruma matrisi](backup-mabs-protection-matrix.md)
### Yükleme veya yükseltme
#### [Azure portalında Azure Backup Sunucusu iş yükleri hazırlama](backup-azure-microsoft-azure-backup.md)
#### [Klasik Azure portalında Azure Backup Sunucusu iş yükleri hazırlama](backup-azure-microsoft-azure-backup-classic.md)
#### [Azure Backup Sunucusu’na depolama alanı ekleme](backup-mabs-add-storage.md)
#### [Azure Backup Sunucusu’nu v.2’ye yükseltme](backup-mabs-upgrade-to-v2.md)
#### [Azure Backup Sunucusu için katılımsız yükleme](backup-mabs-unattended-install.md)
### İş yüklerini koruma
#### [Bir VMware sunucusunu yedeklemek için Azure Backup Sunucusu kullanma](backup-azure-backup-server-vmware.md)
#### [Exchange’i yedeklemek için Azure Backup Sunucusu kullanma](backup-azure-exchange-mabs.md)
#### [Bir SharePoint grubunu yedeklemek için Azure Backup Sunucusu kullanma](backup-azure-backup-sharepoint-mabs.md)
#### [SQL’i yedeklemek için Azure Backup Sunucusu kullanma](backup-azure-sql-mabs.md)
#### [Sistem durumunu koruma ve tam kurtarma](backup-mabs-system-state-and-bmr.md)
### [Azure Backup Sunucusu’ndan veri kurtarma](backup-azure-alternate-dpm-server.md)

## Azure VM’leri
### VM’yi hazırlama
#### [Resource Manager ile dağıtılan sanal makineleri hazırlama](backup-azure-arm-vms-prepare.md)
#### [Uygulama ile tutarlı Linux VM yedekleri](backup-azure-linux-app-consistent.md)
#### [Azure sanal makineleri hazırlama](backup-azure-vms-prepare.md)
### Ortamınızı planlama
#### [VM yedekleme altyapısını planlama](backup-azure-vms-introduction.md)
### VM’leri yedekleme
#### [Azure sanal makinelerini bir Kurtarma Hizmetleri kasasına yedekleme](backup-azure-arm-vms.md)
#### [Şifrelenmiş sanal makineleri yedekleme](backup-azure-vms-encryption.md)
#### [Azure sanal makinelerini yedekleme](backup-azure-vms.md)
### VM’leri yönetme ve izleme
#### [Azure portal’da Azure VM yedeklemelerini yönetme](backup-azure-manage-vms.md)
#### [Azure portal’da Azure VM yedeklemeleri için uyarıları yönetme](backup-azure-monitor-vms.md)
#### [Klasik portalda Azure VM yedeklemelerini yönetme ve izleme](backup-azure-manage-vms-classic.md)
### Verileri VM'lerden geri yükleme
#### [Azure VM yedeklerinden dosya kurtarma](backup-azure-restore-files-from-vm.md)
#### [Azure portalında Resource Manager tarafından dağıtılan VM'leri geri yükleme](backup-azure-arm-restore-vms.md)
#### [Şifrelenmiş sanal makineleri geri yükleme](backup-azure-vms-encryption.md)
#### [Azure’da sanal makineleri geri yükleme](backup-azure-restore-vms.md)
#### [Şifreli VM’ler için Key Vault anahtarını ve gizli dizisini geri yükleme](backup-azure-restore-key-secret.md)

## Azure Backup raporlarını yapılandırma
### [Azure Backup raporlarını yapılandırma](backup-azure-configure-reports.md)
### [Azure Backup raporları için veri modeli](backup-azure-reports-data-model.md)
### [Azure Backup için Log Analytics veri modeli](backup-azure-log-analytics-data-model.md)

## Data Protection Manager
### [Azure portalında DPM iş yükleri hazırlama](backup-azure-dpm-introduction.md)
### [Klasik portalda DPM iş yükleri hazırlama](backup-azure-dpm-introduction-classic.md)
### [Exchange sunucusunu yedeklemek için System Center DPM'yi kullanma](backup-azure-backup-exchange-server.md)
### [Verileri başka bir DPM sunucusuna kurtarma](backup-azure-alternate-dpm-server.md)
### [SQL Server iş yüklerini yedeklemek için DPM'yi kullanma](backup-azure-backup-sql.md)
### [SharePoint grubunu yedeklemek için DPM'yi kullanma](backup-azure-backup-sharepoint.md)

## PowerShell kullanma
### [Azure portalındaki Azure VM'leri](backup-azure-vms-automation.md)
### [Klasik portaldaki Azure VM'leri](backup-azure-vms-classic-automation.md)
### [Azure portalındaki DPM](backup-dpm-automation.md)
### [Klasik portaldaki DPM](backup-dpm-automation-classic.md)
### [Azure portalındaki Windows Server](backup-client-automation.md)
### [Klasik portaldaki Windows Server](backup-client-automation-classic.md)

## Azure SQL Database
### [Uzun süreli yedek saklama yapılandırma](../sql-database/sql-database-configure-long-term-retention.md?toc=%2fazure%2fbackup%2ftoc.json)
### [Kurtarma Hizmetleri kasasındaki yedekleri görüntüleme](../sql-database/sql-database-view-backups-in-vault.md?toc=%2fazure%2fbackup%2ftoc.json)
### [Uzun süreli yedek saklamadan geri yükleme](../sql-database/sql-database-restore-from-long-term-retention.md?toc=%2fazure%2fbackup%2ftoc.json)
### [Uzun süreli Azure SQL yedeklerini silme](../sql-database/sql-database-long-term-retention-delete.md?toc=%2fazure%2fbackup%2ftoc.json)

## Windows Server
### [Windows Server dosyalarını ve klasörlerini yedekleme](backup-configure-vault.md)
### [Windows Server Sistem Durumunu yedekleme](backup-azure-system-state.md)
### [Azure’dan Windows Server’a dosya kurtarma](backup-azure-restore-windows-server.md)
### [Windows Server Sistem Durumunu geri yükleme](backup-azure-restore-system-state.md)
### [Kurtarma Hizmetleri kasalarını izleme ve yönetme](backup-azure-manage-windows-server.md)
### Klasik portalı kullanarak yedekleme ve geri yükleme
#### [Klasik dağıtım modelini kullanan Windows Server](backup-configure-vault-classic.md)
#### [Klasik dağıtım modelini kullanarak Backup kasalarını yönetme](backup-azure-manage-windows-server-classic.md)
#### [Klasik dağıtım modelini kullanarak dosyaları Windows Server'a kurtarma](backup-azure-restore-windows-server-classic.md)

## Kurtarma Hizmetleri kasası
### [Kurtarma Hizmetleri kasalarına genel bakış](backup-azure-recovery-services-vault-overview.md)
### [Bir Backup kasasının Kurtarma Hizmetleri kasasına geçişi](backup-azure-upgrade-backup-to-recovery-services.md)
### [Kurtarma Hizmetleri kasası silme](backup-azure-delete-vault.md)

## Sorun giderme
### [Azure portalındaki Azure VM yedekleme sorunları](backup-azure-vms-troubleshoot.md)
### [Klasik portaldaki Azure VM yedekleme sorunları](backup-azure-vms-troubleshoot-classic.md)
### [Azure VM Yedeklemesi başarısız oluyor: Anlık görüntü durumu için VM aracısı ile iletişim kurulamadı - Anlık görüntü VM alt görevi zaman aşımına uğradı](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md)
### [Azure Backup'ta dosya ve klasörlerin yavaş yedeklenmesi](backup-azure-troubleshoot-slow-backup-performance-issue.md)
### [Azure Backup Sunucusu sorunlarını giderme](backup-azure-mabs-troubleshoot.md)

# Kavramlar

## SSS
### [Kurtarma Hizmetleri kasası hakkında SSS](backup-azure-backup-faq.md)
### [Azure VM yedeklemesi hakkında SSS](backup-azure-vm-backup-faq.md)
### [Azure Backup aracısı kullanarak dosya-klasör yedekleme hakkında SSS](backup-azure-file-folder-backup-faq.md)

## [Rol Tabanlı Access Control](backup-rbac-rs-vault.md)
## [Karma yedeklemeler için güvenlik](backup-azure-security-feature.md)
## [Çevrimdışı yedekleme yapılandırma](backup-azure-backup-import-export.md)
## [Bant kitaplığınızı değiştirme](backup-azure-backup-cloud-as-tape.md)


# Başvuru
## [PowerShell](/powershell/module/azurerm.recoveryservices.backup)
## [.NET](/dotnet/api/microsoft.azure.management.recoveryservices.backup)

# Kaynaklar
## [Azure Yol Haritası](https://azure.microsoft.com/roadmap/)
## [MSDN forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowsazureonlinebackup)
## [Fiyatlandırma](https://azure.microsoft.com/pricing/details/backup/)
## [Fiyatlandırma hesaplayıcı](https://azure.microsoft.com/pricing/calculator/)
## [Hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?product=backup)
## [Videolar](https://azure.microsoft.com/documentation/videos/index/?services=backup)
