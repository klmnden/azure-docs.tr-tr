---
title: 
description: 
services: site-recovery
documentationcenter: 
author: 
manager: 
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 
ms.author: 
translationtype: Human Translation
ms.sourcegitcommit: 4871e5959483058aa957e3373ddc5121b398e1d9
ms.openlocfilehash: 1d6d595bf788fc3fdba0bbc98887614e17c791dc


---



### <a name="install-the-mobility-service"></a>Mobility hizmetini yükleme
Sanal makineler ve fiziksel sunucuları için korumayı etkinleştirmenin ilk adımı, Mobility hizmetini yüklemektir. Bunu çeşitli şekillerde yapabilirsiniz:

* **İşlem sunucusu gönderimi**: Bir makinede çoğaltmayı etkinleştirirken Mobility hizmeti bileşenini işlem sunucusundan gönderip yükleyin. 
Makineler zaten bileşenin güncel bir sürümünü çalıştırıyorsa gönderme temelli yüklemenin gerçekleşmeyeceğini *unutmayın*.
* **Kurumsal gönderim**: WSUS, System Center Configuration Manager veya [Azure Otomasyonu ve İstenen Durum yapılandırması](site-recovery-automate-mobility-service-install.md) gibi kurumsal gönderim işleminizi kullanarak bileşeni otomatik olarak yükleyin. Bunu yapmadan önce yapılandırma sunucusunu ayarlayın.
* **El ile yükleme**: Bileşeni, çoğaltmak istediğiniz her makineye el ile yükleyin. Bunu yapmadan önce yapılandırma sunucusunu ayarlayın.

#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Windows makinelerinde otomatik gönderim için hazırlanma
Mobility hizmetinin işlem sunucusu tarafından otomatik olarak yüklenebilmesi için Windows makineleri aşağıdaki şekilde hazırlanır.

1. İşlem sunucusunun makineye erişmek için kullanabileceği bir hesap oluşturun. Hesabın yönetici ayrıcalıkları (yerel veya etki alanı) olmalıdır ve bu hesap yalnızca gönderim yüklemesi için kullanılır.

   > [!NOTE]
   > Bir etki alanı hesabı kullanmıyorsanız yerel makinede Uzak Kullanıcı Erişim denetimini devre dışı bırakmanız gerekir. Bunu yapmak için kayıt defterinde HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System altında LocalAccountTokenFilterPolicy adlı DWORD girişini 1 değeriyle ekleyin. Kayıt defteri girişini bir CLI ile eklemek için şunu yazın: **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**.
   >
   >
2. Korumak istediğiniz makinenin Windows Güvenlik Duvarı’nda **Bir uygulama veya özelliğin Windows Güvenlik Duvarı‘ndan geçmesine izin ver**’i seçin. **Dosya ve Yazıcı Paylaşımı**’nı ve **Windows Yönetim Araçları**’nı etkinleştirin. Bir etki alanına ait makinelerin güvenlik duvarı ayarlarını bir GPO ile yapılandırabilirsiniz.

   ![Güvenlik duvarı ayarları](./media/site-recovery-vmware-to-azure/mobility1.png)
3. Oluşturduğunuz hesabı ekleyin:

   * **Cspsconfigtool**’u açın. Masaüstünde bir kısayolu bulunur ve kendisi de [YÜKLEME KONUMU]\home\svsystems\bin klasöründedir.
   * **Hesaplarını Yönet** sekmesinde **Hesap Ekle**’ye tıklayın.
   * Oluşturduğunuz hesabı ekleyin. Hesabı ekledikten sonra, bir makine için çoğaltmayı etkinleştirirken kimlik bilgilerini sağlamanız gerekir.

#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Linux sunucularında otomatik gönderim için hazırlanma
1. Korumak istediğiniz Linux makinesinin [korumalı makine önkoşullarında](#protected-machine-prerequisites) açıklandığı şekilde desteklendiğinden emin olun. Linux makinesi ve işlem sunucusu arasında ağ bağlantısı bulunduğundan emin olun.
2. İşlem sunucusunun makineye erişmek için kullanabileceği bir hesap oluşturun. Hesabın kaynak Linux sunucusunda kök kullanıcı olması gerekir ve bu hesap yalnızca gönderim yüklemesi için kullanılır.

   * **Cspsconfigtool**’u açın. Masaüstünde bir kısayolu bulunur ve kendisi de [YÜKLEME KONUMU]\home\svsystems\bin klasöründedir.
   * **Hesaplarını Yönet** sekmesinde **Hesap Ekle**’ye tıklayın.
   * Oluşturduğunuz hesabı ekleyin. Hesabı ekledikten sonra, bir makine için çoğaltmayı etkinleştirirken kimlik bilgilerini sağlamanız gerekir.
3. Kaynak Linux sunucusundaki /etc/hosts dosyasında, yerel ana bilgisayar adını tüm ağ bağdaştırıcıları ile ilişkili IP adreslerine eşleyen girişler olduğunu denetleyin.
4. Çoğaltmak istediğiniz makineye en son openssh, openssh-server, openssl paketlerini yükleyin.
5. SSH’nin etkin olduğundan ve bağlantı noktası 22’de çalıştırıldığından emin olun.
6. SFTP alt sistemi ve parola ile kimlik doğrulamasını sshd_config dosyasında aşağıdaki gibi etkinleştirin:

   * Kök kullanıcı olarak oturum açın.
   * /etc/ssh/sshd_config dosyasında **PasswordAuthentication** ile başlayan satırı bulun.
   * Satırı açıklama durumundan çıkarın ve değerini **no (hayır)** yerine **yes (evet)** olarak değiştirin.
   * **Subsystem** ile başlayan satırı bulun ve açıklama durumundan çıkarın.

     ![Linux](./media/site-recovery-vmware-to-azure/mobility2.png)

### <a name="install-the-mobility-service-manually"></a>Mobility Hizmetini el ile yükleme
Yükleyiciler, Yapılandırma sunucusundaki **C:\Program Files (x86)\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository** konumunda yer almaktadır.

| Kaynak işletim sistemi | Mobility hizmeti yükleme dosyası |
| --- | --- |
| Windows Server (yalnızca&64; bit) |Microsoft-ASR_UA_9.*.0.0_Windows_* release.exe |
| CentOS 6.4, 6.5, 6.6 (yalnızca 64 bit) |Microsoft-ASR_UA_9.*.0.0_RHEL6-64_*release.tar.gz |
| SUSE Linux Enterprise Server 11 SP3 (yalnızca 64 bit) |Microsoft-ASR_UA_9.*.0.0_SLES11-SP3-64_*release.tar.gz |
| Oracle Enterprise Linux 6.4, 6.5 (yalnızca 64 bit) |Microsoft-ASR_UA_9.*.0.0_OL6-64_*release.tar.gz |

#### <a name="install-mobility-service-on-a-windows-server"></a>Mobility Hizmetini Windows Server'a yükleme
1. İlgili yükleyiciyi indirip çalıştırın.
2. **Başlamadan önce** bölümünde **Mobility hizmeti**’ni seçin.

    ![Mobility hizmeti](./media/site-recovery-vmware-to-azure/mobility3.png)
3. **Yapılandırma Sunucusu Ayrıntıları**’nda yapılandırma sunucusunun IP adresini ve Birleşik Kurulum’u çalıştırdığınızda oluşturulan parolayı belirtin. Parolayı almak için yapılandırma sunucusunda şu komutu çalıştırabilirsiniz: **<SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe –v**

    ![Mobility hizmeti](./media/site-recovery-vmware-to-azure/mobility6.png)
4. **Yükleme Konumu**’nda varsayılan ayarı bırakın ve **İleri**’ye tıklayarak yüklemeyi başlatın.
5. **Yükleme İlerleme Durumu**’nda yüklemeyi izleyin ve istenirse makineyi yeniden başlatın. Hizmeti yükledikten sonra portalda durumun güncelleştirilmesi yaklaşık 15 dakika sürebilir.

#### <a name="install-mobility-service-on-a-windows-server-using-the-command-prompt"></a>Mobility Hizmetini Komut istemini kullanarak bir Windows sunucusuna yükleme
1. Yükleyiciyi, korumak istediğiniz sunucuda bir yerel klasöre (örneğin, C:\Temp) kopyalayın. Yükleyici, yapılandırma sunucusunda **[Yükleme Konumu]\home\svsystems\pushinstallsvc\repository** altında bulunabilir. Windows İşletim Sistemlerine yönelik paketin Microsoft-ASR_UA_9.3.0.0_Windows_GA_17thAug2016_release.exe benzeri bir adı olacaktır.
2. Bu dosyanın adını MobilitySvcInstaller.exe olarak değiştirin.
3. MSI yükleyicisini ayıklamak için aşağıdaki komutu çalıştırın:

    ``C:\> cd C:\Tempww
    ``C:\Temp> MobilitySvcInstaller.exe /q /xC:\Temp\Extracted``
    ``C:\Temp> cd Extracted``
    ``C:\Temp\Extracted> UnifiedAgent.exe /Role "Agent" /CSEndpoint "Yapılandırma Sunucusunun IP Adresi" /PassphraseFilePath <Full path to the passphrase file>``

##### <a name="full-command-line-syntax"></a>Tam komut satırı söz dizimi
    UnifiedAgent.exe [/Role <Agent/MasterTarget>] [/InstallLocation <Installation Directory>] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>] [/LogFilePath <Log File Path>]<br/>

**Parametreler**

* **/Role:** Zorunlu. Mobility hizmetinin yüklenmesinin gerekip gerekmediğini belirtir. Giriş değerleri Agent|MasterTarget
* **/InstallLocation:** Zorunlu. Hizmetin yükleneceği konumu belirtir.
* **/PassphraseFilePath:** Zorunlu. Yapılandırma sunucusunun parolası.
* **/LogFilePath:** Zorunlu. Yükleme günlük dosyalarının oluşturulacağı konum.

#### <a name="uninstall-the-mobility-service-manually"></a>Mobility Hizmetini el ile kaldırma
Mobility hizmetini kaldırmak için Denetim Masası'ndaki Program Ekle/Kaldır kullanılabilir veya bu komut satırı komutu çalıştırılabilir: MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}

#### <a name="install-the-mobility-service-on-a-linux-server"></a>Mobility Hizmetini Linux sunucusuna yükleme
1. Yukarıdaki tabloya göre uygun tar arşivini çoğaltmak istediğiniz Linux makinesine kopyalayın.
2. Bir kabuk programı açıp şu komutu çalıştırarak, sıkıştırılmış tar arşivini yerel bir yola ayıklayın: `tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Tar arşivinin içeriklerini ayıkladığınız yerel dizinde bir passphrase.txt dosyası oluşturun. Bunu yapmak için parolayı, yapılandırma sunucusundaki C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase dosyasından kopyalayın ve kabukta *`echo <passphrase> >passphrase.txt`* komutunu çalıştırarak passphrase.txt dosyasına kaydedin.
4. Şu komutu çalıştırarak Mobility hizmetini yükleyin: *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Yapılandırma sunucusunun iç IP adresini belirtin ve 443 numaralı bağlantı noktasının seçili olduğundan emin olun. Hizmeti yükledikten sonra portalda durumun güncelleştirilmesi yaklaşık 15 dakika sürebilir.

**Komut satırından da yükleyebilirsiniz**:

Parolayı, yapılandırma sunucusundaki C:\Program Files (x86)\InMage Systems\private\connection konumundan kopyalayın ve yapılandırma sunucusunda "passphrase.txt" olarak kaydedin. Sonra şu komutları çalıştırın. Örneğimizde, yapılandırma sunucusunun IP adresi 104.40.75.37’dir ve HTTPS bağlantı noktası 443 olmalıdır:


Bir üretim sunucusuna yüklemek için:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

Ana hedef sunucuya yüklemek için:

    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt






<!--HONumber=Feb17_HO3-->


