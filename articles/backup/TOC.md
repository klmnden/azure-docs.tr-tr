
# Genel Bakış
## [Azure Backup nedir?](backup-introduction-to-azure-backup.md)
# Başlarken
## [Dosya ve klasörleri yedekleme](backup-try-azure-backup-in-10-mins.md)
## [Azure sanal makinelerini yedekleme](backup-azure-vms-first-look.md)
## [Azure VM'lerini koruma](backup-azure-vms-first-look-arm.md)
## [SSS](backup-azure-backup-faq.md)
# Nasıl yapılır
## PowerShell'i kullanarak Yedeklemeyi Otomatikleştirme
### [Azure portalındaki Azure VM'leri](backup-azure-vms-automation.md)
### [Klasik portaldaki Azure VM'leri](backup-azure-vms-classic-automation.md)
### [Azure portalındaki DPM](backup-dpm-automation.md)
### [Klasik portaldaki DPM](backup-dpm-automation-classic.md)
### [Azure portalındaki Windows Server](backup-client-automation.md)
### [Klasik portaldaki Windows Server](backup-client-automation-classic.md)
## Uygulama iş yüklerini yedekleme ve geri yükleme
### [Azure portalında DPM iş yükleri hazırlama](backup-azure-dpm-introduction.md)
### [Klasik portalda DPM iş yükleri hazırlama](backup-azure-dpm-introduction-classic.md)
### [Azure portalında Azure Backup Sunucusu iş yükleri hazırlama](backup-azure-microsoft-azure-backup.md)
### [Klasik Azure portalında Azure Backup Sunucusu iş yükleri hazırlama](backup-azure-microsoft-azure-backup-classic.md)
### [Exchange sunucusunu yedeklemek için System Center DPM'yi kullanma](backup-azure-backup-exchange-server.md)
### [Backup kasasındaki verileri alternatif bir DPM sunucusuna kurtarma](backup-azure-alternate-dpm-server.md)
### [SQL Server iş yüklerini yedeklemek için DPM'yi kullanma](backup-azure-backup-sql.md)
### [SharePoint grubunu yedeklemek için DPM'yi kullanma](backup-azure-backup-sharepoint.md)
## Azure VM’lerini yedekleme ve geri yükleme
### [Azure sanal makineleri hazırlama](backup-azure-vms-prepare.md)
### [Resource Manager ile dağıtılan sanal makineleri hazırlama](backup-azure-arm-vms-prepare.md)
### [VM yedekleme altyapısını planlama](backup-azure-vms-introduction.md)
### [Azure sanal makinelerini Backup kasasına yedekleme](backup-azure-vms.md)
### [Azure sanal makinelerini bir Kurtarma Hizmetleri kasasına yedekleme](backup-azure-arm-vms.md)
### [Şifrelenmiş sanal makineleri yedekleme ve geri yükleme](backup-azure-vms-encryption.md)
### [Klasik portalda Azure VM yedeklemelerini yönetme ve izleme](backup-azure-manage-vms-classic.md)
### [Azure portal’da Azure VM yedeklemelerini yönetme](backup-azure-manage-vms.md)
### [Azure portal’da Azure VM yedeklemeleri için uyarıları yönetme](backup-azure-monitor-vms.md)
### [Azure VM yedeklerinden dosya kurtarma](backup-azure-restore-files-from-vm.md)
### [Azure’da sanal makineleri geri yükleme](backup-azure-restore-vms.md)
### [Azure portalında Resource Manager tarafından dağıtılan VM'leri geri yükleme](backup-azure-arm-restore-vms.md)
### [Azure Backup kullanarak şifreli VM’ler için Key Vault anahtarını ve parolasını geri yükleme](backup-azure-restore-key-secret.md)
## Azure SQL Veritabanı’nı yedekleme ve geri yükleme
### [Uzun süreli yedek saklama yapılandırma](../sql-database/sql-database-configure-long-term-retention.md?toc=%2fazure%2fbackup%2ftoc.json)
### [Kurtarma Hizmetleri kasasındaki yedekleri görüntüleme](../sql-database/sql-database-view-backups-in-vault.md?toc=%2fazure%2fbackup%2ftoc.json)
### [Uzun süreli yedek saklamadan geri yükleme](../sql-database/sql-database-restore-from-long-term-retention.md?toc=%2fazure%2fbackup%2ftoc.json)
### [Uzun süreli Azure SQL yedeklerini silme](../sql-database/sql-database-long-term-retention-delete.md?toc=%2fazure%2fbackup%2ftoc.json)

## Windows makineleri yedekleme ve geri yükleme
### [Klasik dağıtım modelini kullanan Windows Server](backup-configure-vault-classic.md)
### [Resource Manager dağıtım modelini kullanan Windows Server](backup-configure-vault.md)
### [Klasik dağıtım modelini kullanarak Backup kasalarını yönetme](backup-azure-manage-windows-server-classic.md)
### [Kurtarma Hizmetleri kasalarını izleme ve yönetme](backup-azure-manage-windows-server.md)
### [Dosyaları Resource Manager dağıtım modelini kullanarak Windows Server’a kurtarma](backup-azure-restore-windows-server.md)
### [Klasik dağıtım modelini kullanarak dosyaları Windows Server'a kurtarma](backup-azure-restore-windows-server-classic.md)

## [Yedekleme yönetimi için Rol Tabanlı Access Control kullanma](backup-rbac-rs-vault.md)
## [Karma yedeklemeler için güvenlik özelliklerini etkinleştirme](backup-azure-security-feature.md)
## [Azure Backup kasasını silme](backup-azure-delete-vault.md)
## [Çevrimdışı yedekleme yapılandırma](backup-azure-backup-import-export.md)
## [Azure Backup'ı kullanarak bant altyapınızı değiştirme](backup-azure-backup-cloud-as-tape.md)
## Sorun giderme
### [Azure portalındaki Azure VM yedekleme sorunları](backup-azure-vms-troubleshoot.md)
### [Klasik portaldaki Azure VM yedekleme sorunları](backup-azure-vms-troubleshoot-classic.md)
### [Azure VM Yedeklemesi başarısız oluyor: Anlık görüntü durumu için VM aracısı ile iletişim kurulamadı - Anlık görüntü VM alt görevi zaman aşımına uğradı](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md)
### [Azure Backup'ta dosya ve klasörlerin yavaş yedeklenmesi](backup-azure-troubleshoot-slow-backup-performance-issue.md)

# Başvuru
## [PowerShell](/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/azurerm.recoveryservices.backup)
## [.NET](/dotnet/api/microsoft.azure.management.recoveryservices.backup)

# Kaynaklar
## [Fiyatlandırma](https://azure.microsoft.com/pricing/details/backup/)
## [MSDN forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowsazureonlinebackup)
## [Videolar](https://azure.microsoft.com/documentation/videos/index/?services=backup)
## [Hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?product=backup)


<!--HONumber=Feb17_HO2-->


