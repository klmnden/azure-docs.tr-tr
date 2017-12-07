| **Donanım** | |
| --- |---|
| CPU çekirdeği sayısı| 8 |
| RAM| 12 GB|
| Disk sayısı | 3 <br><br> - İşletim sistemi diski<br> - İşlem sunucusu önbellek diski<br> - Bekletme sürücüsü (yeniden çalışma için)|
| Boş disk alanı (işlem sunucusu önbelleği) | 600 GB
| Boş disk alanı (bekletme diski) | 600 GB|
| **Yazılım** | |
| İşletim sistemi sürümü | Windows Server 2012 R2 <br> Windows Server 2016 |
| İşletim sistemi yerel ayarı | İngilizce (en-us)|
| VMware vSphere PowerCLI sürümü | [PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0")|
| Windows Server rolleri | Aşağıdaki rolleri etkinleştirmeyin: <br> - Active Directory Domain Services <br>- İnternet Bilgi Hizmetleri <br> - Hyper-V |
| Grup İlkeleri| Aşağıdaki grup ilkeleri sunucuda etkinleştirilmemelidir <br> -Komut istemi erişimi engelle <br> -Kayıt defteri düzenleme araçları erişimi engelle <br> -Dosya ekleri için mantığı güven <br> -Komut dosyası yürütme Aç <br> **Not:** bu grup ilkeleri hakkında daha fazla bilgi bulunabilir [burada](https://technet.microsoft.com/en-us/library/gg176671(v=ws.10).aspx)|
| Internet bilgi Service(IIS) yapılandırma | -Önceden var olan varsayılan Web sitesi <br> -Etkinleştirin [anonim kimlik doğrulaması](https://technet.microsoft.com/en-us/library/cc731244(v=ws.10).aspx) <br> -Etkinleştirin [Fastcgı](https://technet.microsoft.com/en-us/library/cc753077(v=ws.10).aspx) ayarı  <br> -Önceden varolan hiçbir websit/uygulama 443 numaralı bağlantı noktasını dinlemeye<br>|
| **Ağ** | |
| Ağ arabirimi kart türü | (Bir VMware sanal makinesi olarak dağıtıldığında) VMXNET3 |
| IP adresi türü | Statik |
| İnternet erişimi | Sunucunun aşağıdaki URL'lere doğrudan veya bir ara sunucu üzerinden erişebilmesi gerekir: <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com <br> - https://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi (Genişleme İşlem Sunucuları için gerekli değildir) <br> - time.nist.gov <br> - time.windows.com |
| Bağlantı Noktaları | 443 (Denetim kanalı düzenleme)<br>9443 (Veri aktarımı)|
