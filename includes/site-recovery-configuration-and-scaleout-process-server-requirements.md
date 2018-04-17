| **Bileşen** | **Gereksinim** |
| --- |---|
| CPU çekirdekleri| 8 |
| RAM | 16 GB|
| Disk sayısı | işletim sistemi diski, işlem sunucusu önbellek disk ve yeniden çalışma için bekletme sürücüsü dahil olmak üzere 3 |
| Boş disk alanı (işlem sunucusu önbelleği) | 600 GB
| Boş disk alanı (bekletme diski) | 600 GB|
| İşletim sistemi  | Windows Server 2012 R2 <br> Windows Server 2016 |
| İşletim sistemi yerel ayarı | İngilizce (en-us)|
| VMware vSphere PowerCLI sürümü | [PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0")|
| Windows Server rolleri | Bu rolleri etkinleştirme: <br> - Active Directory Domain Services <br>- İnternet Bilgi Hizmetleri <br> - Hyper-V |
| Grup İlkeleri| Bu grup ilkeleri etkinleştirme: <br> -Komut istemi erişimi engelleyin. <br> -Düzenleme araçları kayıt defterine erişim engelleyin. <br> -Dosya ekleri için mantığı güven. <br> -Komut dosyası yürütme açın. <br> [Daha fazla bilgi](https://technet.microsoft.com/en-us/library/gg176671(v=ws.10).aspx)|
| IIS | -Önceden var olan varsayılan Web sitesi <br> -Etkinleştirin [anonim kimlik doğrulaması](https://technet.microsoft.com/en-us/library/cc731244(v=ws.10).aspx) <br> -Etkinleştirin [Fastcgı](https://technet.microsoft.com/en-us/library/cc753077(v=ws.10).aspx) ayarı  <br> -Önceden var olan Web sitesi/443 numaralı bağlantı noktasını dinlemeye uygulama<br>|
| NIC türü | (VMware VM olarak dağıtıldığında) VMXNET3 |
| IP adresi türü | Statik |
| İnternet erişimi | Sunucunun aşağıdaki URL'lere erişim gerekir: <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com <br> - https://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi (genişleme işlem sunucuları için gerekli değildir) <br> - time.nist.gov <br> - time.windows.com |
| Bağlantı Noktaları | 443 (Denetim kanalı düzenleme)<br>9443 (Veri aktarımı)|
