
## <a name="azure-backup"></a>Azure Backup

Azure üretim iş yükleri çalıştıran VM'lerin yedeklenmesi için Azure Yedekleme'yi kullanın. Azure yedekleme, hem Windows hem de Linux VM'ler için uygulamayla tutarlı yedeklemeler destekler. Azure yedekleme coğrafi olarak yedekli kurtarma kasalarında depolanan kurtarma noktaları oluşturur. Bir kurtarma noktasından geri yüklediğinizde tüm VM ya da yalnızca belirli dosyaları geri yükleyebilirsiniz. 

"Windows sanal makineleri öğretici yedekleme" için basit, uygulamalı giriş Azure yedekleme Azure VM'ler için bkz [Linux](../articles/virtual-machines/linux/tutorial-backup-vms.md) veya [Windows](../articles/virtual-machines/windows/tutorial-backup-vms.md)

Azure yedekleme nasıl çalıştığı hakkında daha fazla bilgi için bkz: [azure'da VM yedekleme altyapınızı planlama](../articles/backup/backup-azure-vms-introduction.md)


## <a name="azure-site-recovery"></a>Azure Site Recovery

Tüm bölge kesinti ana doğal afet ya da yaygın bir hizmet kesintisi nedeniyle yaşadığında azure Site Recovery Vm'leriniz ana olağanüstü durum senaryodan korur. Tek bir tıklatmayla sağlasa da, uygulamanızın dakika kurtarabilmeleri için Azure Site Recovery Vm'leriniz için yapılandırabilirsiniz. Tercih ettiğiniz bir Azure bölgesine çoğaltabilirsiniz, eşleştirilmiş bölgelere sınırlı değildir. 

Üretim iş yükleri veya devam eden çoğaltmayı etkilemeden ile isteğe bağlı yük devretme testlerini, olağanüstü durum kurtarma ayrıntısına çalıştırabilirsiniz. Yük devretme ve yeniden çalışma birden çok VM'ler üzerinde çalıştırılan tüm uygulamasının düzenlemek için kurtarma planları oluşturabilirsiniz. Kurtarma planı özelliği, Azure automation runbook'ları ile tümleşiktir.

Tarafından başlayabiliriz [sanal makinelerinizi çoğaltma](https://aka.ms/a2a-getting-started). 

## <a name="managed-snapshots"></a>Yönetilen anlık görüntüler 

Geliştirme ve test ortamları anlık görüntüleri yönetilen diskler kullanan VM'lerin yedeklenmesi için hızlı ve basit bir seçenek sağlar. Yönetilen bir anlık görüntü bir salt okunur tam yönetilen bir disk kopyasıdır. Anlık görüntüler kaynak disk bağımsız var ve bir VM yeniden oluşturma için yeni yönetilen diskler oluşturmak için kullanılan. Bunlar, disk kullanılan bölümü göre faturalandırılır. Sağlanan kapasite 64 GB ve 10 GB gerçek kullanılan veri boyutunu ile yönetilen bir disk görüntüsünü oluşturursanız, örneğin, anlık görüntü yalnızca kullanılan veri boyutu 10 GB için faturalandırılır.  

Anlık görüntüleri oluşturma hakkında daha fazla bilgi için bkz:

* [Windows’da Anlık Görüntüler kullanılarak Yönetilen Disk olarak depolanmış VHD kopyası oluşturma](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)
* [Linux’ta Anlık Görüntüler kullanılarak Yönetilen Disk olarak depolanmış VHD kopyası oluşturma](../articles/virtual-machines/linux/snapshot-copy-managed-disk.md)



## <a name="next-steps"></a>Sonraki adımlar
Azure Backup işlemi "Windows sanal makineleri öğretici yedekleme" izleyerek için deneyebilirsiniz [Linux](../articles/virtual-machines/linux/tutorial-backup-vms.md) veya [Windows](../articles/virtual-machines/windows/tutorial-backup-vms.md).
