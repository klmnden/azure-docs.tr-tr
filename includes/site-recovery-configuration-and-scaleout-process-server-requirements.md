| **Donanım** | |
| --- |---|
| CPU çekirdeği sayısı| 8 |
| RAM| 12 GB|
| Disk sayısı | 3 <br><br> - İşletim sistemi diski<br> - İşlem sunucusu önbellek diski<br> - Bekletme sürücüsü (yeniden çalışma için)|
| Boş disk alanı (işlem sunucusu önbelleği) | 600 GB
| Boş disk alanı (bekletme diski) | 600 GB|
| **Yazılım** | |
| İşletim sistemi sürümü | Windows Server 2012 R2 |
| İşletim sistemi yerel ayarı | İngilizce (en-us)|
| VMware vSphere PowerCLI sürümü | [PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0")|
| Windows Server rolleri | Aşağıdaki rolleri etkinleştirmeyin: <br> - Active Directory Domain Services <br>- İnternet Bilgi Hizmetleri <br> - Hyper-V |
| **Ağ** | |
| Ağ arabirimi kart türü | VMXNET3 |
| IP adresi türü | Statik |
| İnternet erişimi | Sunucunun aşağıdaki URL'lere doğrudan veya bir ara sunucu üzerinden erişebilmesi gerekir: <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com <br> - https://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi (Genişleme İşlem Sunucuları için gerekli değildir) <br> - time.nist.gov <br> - time.windows.com |
| Bağlantı Noktaları | 443 (Denetim kanalı düzenleme)<br>9443 (Veri aktarımı)|
