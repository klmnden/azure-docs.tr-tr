---
title: Olağanüstü durum kurtarma için çoğaltmayı etkinleştirirken Mobility hizmeti anında yükleme hatalarını giderme | Microsoft Docs
description: Olağanüstü durum kurtarma için çoğaltmayı etkinleştirirken Mobility hizmetlerini yükleme hatalarını giderme
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.author: ramamill
ms.date: 02/27/2019
ms.openlocfilehash: 58c09c71aad2b6244f6e2f3d144c033665932f50
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925572"
---
# <a name="troubleshoot-mobility-service-push-installation-issues"></a>Mobility hizmeti anında yükleme sorunlarını giderme

Mobility hizmetinin yüklenmesi sırasında bir anahtar çoğaltmayı etkinleştirme sırasında adımdır. Bu adım başarısını yalnızca önkoşulları sağladıktan ve desteklenen yapılandırmalar ile çalışma bağlıdır. Mobility hizmeti yüklemesi sırasında karşılaşacağınız en yaygın hataların nedeni şunlardır:

* [Kimlik bilgisi/ayrıcalık hataları](#credentials-check-errorid-95107--95108)
* [Oturum açma hataları](#login-failures-errorid-95519-95520-95521-95522)
* [Bağlantı hataları](#connectivity-failure-errorid-95117--97118)
* [Dosya ve yazıcı paylaşımı hataları](#file-and-printer-sharing-services-check-errorid-95105--95106)
* [WMI Hatası](#windows-management-instrumentation-wmi-configuration-check-error-code-95103)
* [Desteklenmeyen işletim sistemleri](#unsupported-operating-systems)
* [Desteklenmeyen önyükleme yapılandırmaları](#unsupported-boot-disk-configurations-errorid-95309-95310-95311)
* [VSS yükleme hataları](#vss-installation-failures)
* [GRUB yapılandırma cihaz UUID yerine cihaz adı](#enable-protection-failed-as-device-name-mentioned-in-the-grub-configuration-instead-of-uuid-errorid-95320)
* [LVM'yi birim](#lvm-support-from-920-version)
* [Uyarıları yeniden başlatma](#install-mobility-service-completed-with-warning-to-reboot-errorid-95265--95266)

Çoğaltmayı etkinleştirdiğinizde göndermeye çalıştığında Azure Site Recovery mobility Hizmeti Aracısı sanal makinenize yükleyin. Bunun bir parçası olarak, yapılandırma sunucusu ile sanal makineye bağlanın ve aracıyı kopyalamak çalışır. Başarılı yükleme etkinleştirmek için aşağıda verilen adım adım sorun giderme yönergeleri izleyin.

## <a name="credentials-check-errorid-95107--95108"></a>Kimlik onay (errorID: 95107 & 95108)

* Çoğaltmayı etkinleştirme sırasında seçilen kullanıcı hesabı olup olmadığını doğrulamak **geçerli, doğru**.
* Azure Site Recovery gerektirir **kök** hesabı veya kullanıcı hesabıyla **yönetici ayrıcalıkları** anında yükleme gerçekleştirmek için. Aksi takdirde, kaynak makinede anında yükleme engellenir.
  * Windows için (**hata 95107**), kullanıcı hesabının yönetici erişimi olup olmadığını doğrulamak yerel veya etki alanı, kaynak makinede.
  * Bir etki alanı hesabı kullanmıyorsanız yerel bilgisayarda Uzak kullanıcı erişim denetimini devre dışı bırakmanız gerekir.
    * Kayıt defteri anahtarının hkey_local_machıne\software\microsoft\windows\currentversion\policies\system altında uzak kullanıcı erişim denetimini devre dışı bırakmak için yeni bir DWORD değeri ekleyin: LocalAccountTokenFilterPolicy. Değer 1 olarak ayarlayın. Bu adım yürütülecek komut isteminden aşağıdaki komutu çalıştırın:

         `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`
  * Linux için (**hata 95108**), mobility Aracısı başarılı yükleme için kök hesabı seçmeniz gerekir. Ayrıca, SFTP hizmetlerini çalıştırıyor olmalıdır. SFTP alt sistemi ve parola kimlik doğrulamasını sshd_config dosyasında etkinleştirmek için:
    1. Kök olarak oturum açın.
    2. /Etc/SSH/sshd_config dosyasına gidin, PasswordAuthentication ile başlayan satırı bulun.
    3. Satırı açıklamadan çıkarın ve değerini Evet olarak değiştirin.
    4. Alt sistemi ile başlayan satırı bulun ve bu satırı açıklamadan çıkarın.
    5. Sshd hizmetini yeniden başlatın.

Seçilen kullanıcı hesabının kimlik bilgilerini değiştirmek istiyorsanız, verilen yönergeleri izleyin [burada](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="insufficient-privileges-failure-errorid-95517"></a>Ayrıcalıklar yetersiz hatası (errorID: 95517)

Mobility aracısı yüklemek için seçilen kullanıcı yönetici ayrıcalıklarına sahip olmadığı durumlarda, yapılandırma sunucusu/genişleme işlem sunucusu kaynak makinenin açın mobility Aracısı yazılımı kopyalamanız için verilmez. Bu nedenle, bu hata bir erişim reddedildi hatası sonucudur. Kullanıcı hesabının yönetici ayrıcalıkları olduğundan emin olun.

Seçilen kullanıcı hesabının kimlik bilgilerini değiştirmek istiyorsanız, verilen yönergeleri izleyin [burada](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="insufficient-privileges-failure-errorid-95518"></a>Ayrıcalıklar yetersiz hatası (errorID: 95518)

Etki alanı güven ilişkisi kurulmasını birincil etki alanı ve iş istasyonu arasında kaynak makineye oturum açmaya çalışırken başarısız olduğunda, mobility Aracısı yüklemesi 95518 hata Koduyla başarısız oluyor. Bu nedenle, mobility aracısı yüklemek için kullanılan kullanıcı hesabının, kaynak makinenin birincil etki alanı ile oturum açmak için yönetici ayrıcalıkları olduğundan emin olun.

Seçilen kullanıcı hesabının kimlik bilgilerini değiştirmek istiyorsanız, verilen yönergeleri izleyin [burada](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="login-failures-errorid-95519-95520-95521-95522"></a>Oturum açma hataları (errorID: 95519, 95520, 95521, 95522)

### <a name="credentials-of-the-user-account-have-been-disabled-errorid-95519"></a>Kullanıcı hesabının kimlik bilgilerini devre dışı bırakıldı (errorID: 95519)

Çoğaltmayı etkinleştirme sırasında seçilen kullanıcı hesabı devre dışı bırakıldı. Kullanıcı hesabını etkinleştirmek için makalesine bakabilirsiniz [burada](https://aka.ms/enable_login_user) veya metin değiştirerek aşağıdaki komutu çalıştırın *kullanıcıadı* gerçek kullanıcı adına sahip.
`net user 'username' /active:yes`

### <a name="credentials-locked-out-due-to-multiple-failed-login-attempts-errorid-95520"></a>Kimlik bilgileri nedeniyle birden çok başarısız oturum açma denemesi kilitli (errorID: 95520)

Kullanıcı hesabının bir makineye erişmek için birden fazla başarısız deneme çalışmalarını kilitleyecek. Hatanın nedeni şunlar olabilir:

* Yapılandırma Kurulum sırasında sağlanan kimlik bilgileri yanlış veya
* Çoğaltmayı etkinleştirme sırasında seçilen kullanıcı hesabı yanlış.

Bu nedenle, verilen yönergeleri izleyerek seçilen kimlik bilgilerini değiştirme [burada](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation) süre sonra işlemi yeniden deneyin.

### <a name="logon-servers-are-not-available-on-the-source-machine-errorid-95521"></a>Kaynak makinede oturum açma sunucusu kullanılamıyor (errorID: 95521)

Oturum açma sunucusu, kaynak makinede mevcut olmadığı durumlarda, bu hata oluşur. Oturum açma sunucusu olarak kullanım dışı kalması oturum açma isteği başarısız olmasına neden ve bu nedenle mobility Aracısı yüklenemiyor. Başarılı oturum açma için oturum açma sunucusu kaynak makinede kullanılabilir ve oturum açma hizmeti başlatmak emin olun. Ayrıntılı yönergeler için bkz. KB [139410](https://support.microsoft.com/en-in/help/139410/err-msg-there-are-currently-no-logon-servers-available) hata iletisi: Şu anda yok oturum açma sunucuları kullanılabilir vardır.

### <a name="logon-service-isnt-running-on-the-source-machine-errorid-95522"></a>Oturum açma hizmeti kaynak makinede çalışmadığından (errorID: 95522)

Oturum açma hizmeti, kaynak makinede çalışmıyor ve oturum açma isteğinin hataya neden oldu. Bu nedenle, mobility Aracısı yüklenemiyor. Gidermek için oturum açma hizmeti için başarılı oturum açma kaynak makinede çalıştığından emin olun. Oturum açma hizmeti başlatmak için komut "net start oturum açma" komut isteminden çalıştırın veya Görev Yöneticisi'nden "NetLogon" hizmetini başlatın.

## <a name="connectivity-failure-errorid-95117--97118"></a>**Bağlantı hatası (errorID: 95117 & 97118)**

Yapılandırma sunucusu / genişleme işlem sunucusu kaynağına Mobility aracısı yüklemek için VM bağlanmak dener. Kaynak makine, ağ bağlantısı sorunları nedeniyle erişilebilir değil. Bu hata oluşur. Çözümlemek için

* Yapılandırma Sunucusu'na kaynak makinenizden ping mümkün olduğundan emin olun. Çoğaltmayı etkinleştirme sırasında genişleme işlem sunucusu seçtiyseniz, işlem sunucusu, kaynak makineden ping mümkün olduğundan emin olun.
  * Kaynak sunucu makine komut satırından Telnet kullanma ping yapılandırma sunucusu / genişleme işlem sunucusu ile https bağlantı noktası (herhangi bir ağ bağlantısı sorunları veya güvenlik duvarı bağlantı noktası engelleme sorunları olup olmadığını görmek için aşağıda gösterildiği gibi 135).

     `telnet <CS/ scale-out PS IP address> <135>`
* Ayrıca **Linux VM**,
  * En son openssh, openssh-server ve openssl paketlerini yüklü olup olmadığını denetleyin.
  * Denetleyin ve Secure Shell (SSH) etkin ve bağlantı noktası 22 üzerinde çalıştığından emin olun.
  * SFTP hizmetleri çalışıyor. SFTP alt sistemi ve parola kimlik doğrulamasını sshd_config dosyasında etkinleştirmek için
    * Kök olarak oturum açın.
    * /Etc/SSH/sshd_config dosyasına gidin, PasswordAuthentication ile başlayan satırı bulun.
    * Satırı açıklamadan çıkarın ve değerini Evet olarak değiştirin
    * Alt sistemi ile başlayan satırı bulun ve bu satırı açıklamadan çıkarın
    * Sshd hizmetini yeniden başlatın.
* Bir süre sonra doğru yanıt ise veya yanıt konağına bağlı olduğundan kurulu bağlantı başarısız oldu bağlantı girişimi başarısız olmuş.
* Bağlantı/ağ/etki alanı olabilir ilgili sorun. Ayrıca, sorunu veya TCP bağlantı noktası tükenmesi sorunu çözme DNS adı nedeniyle de olabilir. Etki alanınızdaki gibi bilinen sorunlar olup olmadığını denetleyin.

## <a name="connectivity-failure-errorid-95523"></a>Bağlantı hatası (errorID: 95523)

Kaynak makine bulunduğu ağ bulunamadı veya silinmiş olabilir veya artık kullanılamıyor, bu hata oluşur. Hatayı gidermek için tek ağ bulunduğunu sağlayarak yoludur.

## <a name="file-and-printer-sharing-services-check-errorid-95105--95106"></a>Dosya ve Yazıcı Paylaşımı hizmetleri onay (errorID: 95105 & 95106)

Bağlantı denetimi sonra dosya ve Yazıcı Paylaşımı hizmet etkin değilse, sanal makinenizde doğrulayın. Bu ayarlar, kaynak makinenin açın Mobility Aracısı kopyalamak için gereklidir.

İçin **windows 2008 R2 ve önceki sürümler**,

* Dosya ve yazıcı paylaşımını Windows Güvenlik Duvarı üzerinden etkinleştirmek için
  * Denetim Masası -> Sistem ve Güvenlik -> Windows Güvenlik Duvarı'nı Gelişmiş'i tıklatın, soldaki bölmede -> Ayarlar -> konsol ağacında gelen kuralları tıklayın.
  * Dosya ve Yazıcı Paylaşımı (NB-Oturum-gelen) ve dosya ve Yazıcı Paylaşımı (SMB-gelen) kurallarını bulun. Her kural için bir kurala sağ tıklayın ve ardından **kuralı etkinleştir**.
* Dosya Paylaşımı ile Grup İlkesi'ni etkinleştirmek için
  * Başlat'a gidin, arama ve gpmc.msc yazın.
  * Gezinti bölmesinde, aşağıdaki klasörleri açın: Yerel bilgisayar ilkesi, kullanıcı yapılandırma, Yönetim Şablonları, Windows bileşenleri ve ağ paylaşımı.
  * Ayrıntılar bölmesinde **kullanıcı profilleri içinde dosyaları paylaşmasını engelleyebilir**. Grup İlkesi ayarını devre dışı bırakın ve dosyaları paylaşmak kullanıcının özelliğini etkinleştirmek için devre dışı bırak Değişikliklerinizi kaydetmek için Tamam'a tıklayın. Daha fazla bilgi için bkz. [etkinleştirmek veya devre dışı dosya paylaşımı Grup İlkesi ile](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754359(v=ws.10)).

İçin **sonraki sürümlerinde**, bölümlerinde sağlanan yönergeleri izleyin [VMware Vm'lerini ve fiziksel sunucuları olağanüstü durum kurtarma için mobilite hizmetini yüklemeyi](vmware-azure-install-mobility-service.md) dosya ve Yazıcı Paylaşımı'nı etkinleştirmek için.

## <a name="windows-management-instrumentation-wmi-configuration-check-error-code-95103"></a>Windows Yönetim Araçları (WMI) yapılandırma denetimi (hata kodu: 95103)

Dosya ve yazıcı hizmetlerini iade ettikten sonra WMI hizmetinin güvenlik duvarı aracılığıyla özel, genel ve etki alanı profilleri için etkinleştirin. Bu ayarlar, kaynak makinede uzaktan yürütme tamamlamak için gereklidir. Etkinleştirmek için

* Denetim Masası'na gidin, güvenlik'e tıklayın ve sonra Windows Güvenlik Duvarı'nı tıklatın.
* Değiştir'e tıklayın ve ardından özel durumlar sekmesine tıklayın.
* Özel durumlar penceresinde için Windows Yönetim Araçları (WMI trafiğinin güvenlik duvarından etkinleştirmek için WMI) onay kutusunu işaretleyin. 

Komut isteminde bir Güvenlik Duvarı'ndan WMI trafiğine de etkinleştirebilirsiniz. Aşağıdaki komutu kullanın `netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes`
Diğer WMI sorun giderme makaleleri aşağıdaki makalelerinden bulunamadı.

* [Temel WMI test etme](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/)
* [WMI sorunlarını giderme](https://msdn.microsoft.com/library/aa394603(v=vs.85).aspx)
* [WMI komut dosyaları ve WMI hizmetleri ile ilgili sorunları giderme](https://technet.microsoft.com/library/ff406382.aspx#H22)

## <a name="unsupported-operating-systems"></a>Desteklenmeyen işletim sistemleri

Başka bir yaygın başarısızlık nedeni desteklenmeyen bir işletim sistemi nedeniyle olabilir. Desteklenen işletim sistemi/çekirdek sürümü mobilite hizmetinin başarılı yükleme için üzerinde olduğundan emin olun. Özel bir düzeltme eki kullanımını kaçının.
İşletim sistemleri ve Azure Site Recovery tarafından desteklenen bir çekirdek sürümleri listesini görüntülemek için bkz bizim [destek matrisi belge](vmware-physical-azure-support-matrix.md#replicated-machines).

## <a name="unsupported-boot-disk-configurations-errorid-95309-95310-95311"></a>Desteklenmeyen önyükleme disk yapılandırmaları (errorID: 95309, 95310, 95311)

### <a name="boot-and-system-partitions--volumes-are-not-the-same-disk-errorid-95309"></a>Önyükleme ve sistem bölümleri veya birimleri aynı diskte (errorID: 95309)

Önce 9.20 sürümü, önyükleme ve sistem bölümleri / birimler farklı disklerde was'da desteklenmeyen bir yapılandırma. Gelen [9.20 sürüm](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), bu yapılandırma desteklenir. Bu destek için en son sürümünü kullanın.

### <a name="the-boot-disk-is-not-available-errorid-95310"></a>Önyükleme diski kullanılamıyor (errorID: 95310)

Bir önyükleme diski olmadan bir sanal makine korunamaz. Bu yük devretme işlemi sırasında kesintisiz kurtarma sanal makinesinin emin olmaktır. Yük devretmeden sonra makineyi önyüklemek için hata olmaması önyükleme diski sonuçlanır. Sanal makine önyükleme diski içerdiğinden emin olun ve işlemi yeniden deneyin. Ayrıca, aynı makinede birden fazla önyükleme diski desteklenmediğini unutmayın.

### <a name="multiple-boot-disks-present-on-the-source-machine-errorid-95311"></a>Kaynak makinede birden fazla önyükleme diski sunar (errorID: 95311)

Birden fazla önyükleme diski sanal makineyle değil bir [yapılandırma desteklenen](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage).

## <a name="system-partition-on-multiple-disks-errorid-95313"></a>Sistem bölümü birden çok diskte (errorID: 95313)

9.20 sürümünden önce desteklenmeyen bir yapılandırma kök bölümü veya birimi birden fazla diskte olduğu. Gelen [9.20 sürüm](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), bu yapılandırma desteklenir. Bu destek için en son sürümünü kullanın.

## <a name="enable-protection-failed-as-device-name-mentioned-in-the-grub-configuration-instead-of-uuid-errorid-95320"></a>GRUB yapılandırması UUID yerine belirtilen bir cihaz adı olarak koruma etkinleştirilemedi (errorID: 95320)

**Olası neden:** </br>
GRUB yapılandırma dosyaları ("/ boot/grub/menu.lst", "/ boot/grub/grub.cfg", "/ boot/grub2/grub.cfg" veya "/ varsayılan/etc/grub") parametre değeri içerebilir **kök** ve **sürdürme** olarak UUID yerine gerçek cihaz adları. Site Recovery VM gelen aynı ada sahip sorunları kaynaklanan yük devretmede büyütme gibi değil aygıtlarını adı VM yeniden başlatma arasında değişiklik gösterebileceği UUID yaklaşım zorunlu kılar. Örneğin: </br>


- GRUB dosyasıdır aşağıdaki satırı **/boot/grub2/grub.cfg**. <br>
  *Linux /boot/vmlinuz-3.12.49-11-default **kök = / dev/sda2** ${extra_cmdline} **= / dev/sda1 sürdürme** splash sessiz sessiz showopts =*


- GRUB dosyasıdır aşağıdaki satırı **/boot/grub/menu.lst**
  *çekirdek /boot/vmlinuz-3.0.101-63-default **kök = / dev/sda2** **= / dev/sda1 Sürdür ** splash sessiz crashkernel = 256M-:128M showopts vga = 0x314 =*

Yukarıdaki kalın dize gözlemlerseniz, GRUB parametreleri "root" ve "Devam" UUID yerine gerçek cihaz adları vardır.
 
**Nasıl:**<br>
Cihaz adları, karşılık gelen UUID ile değiştirilmelidir.<br>


1. Komutunu yürüterek cihazı UUID'si Bul "blkid \<cihaz adı >". Örneğin:<br>
   ```
   blkid /dev/sda1
   /dev/sda1: UUID="6f614b44-433b-431b-9ca1-4dd2f6f74f6b" TYPE="swap"
   blkid /dev/sda2 
   /dev/sda2: UUID="62927e85-f7ba-40bc-9993-cc1feeb191e4" TYPE="ext3" 
   ```

2. Biçimde, UUID artık cihaz adı yerine "kök UUID = =\<UUID >". Örneğin cihaz adları için kök UUID ile değiştirin ve parametre dosyaları yukarıdaki sürdürmek için "/ boot/grub2/grub.cfg", "/ boot/grub2/grub.cfg" veya "/ varsayılan/etc/grub: dosyalarda satır ardından aramak gibi. <br>
   *Çekirdek /boot/vmlinuz-3.0.101-63-default **kök UUID = 62927e85-f7ba-40bc-9993-cc1feeb191e4 =** **sürdürme UUID = 6f614b44-433b-431b-9ca1-4dd2f6f74f6b =** splash sessiz crashkernel = 256M-:128M = showopts vga 0x314 =*
3. Korumayı yeniden yeniden başlatın

## <a name="install-mobility-service-completed-with-warning-to-reboot-errorid-95265--95266"></a>Mobility hizmetini yeniden başlatmak için uyarı ile tamamlandı yükleme (errorID: 95265 & 95266)

Site Recovery mobility hizmeti biri filtre sürücüsü adlı birçok bileşen vardır. Filtre sürücüsü, yalnızca bir anda sistemin yeniden başlatılması, sistem belleğe yüklenen. Bu, yeni bir filtre sürücüsü yüklendiğinde, filtre sürücüsü düzeltmesi'nin yalnızca gerçekleşmiş anlamına gelir; Bu, yalnızca sistem yeniden başlatma sırasında gerçekleşebilir.

**Lütfen unutmayın** bu bir uyarıdır ve mevcut çoğaltma bile yeni aracı güncelleştirmesinden sonra çalışır. Yeni filtre sürücüsü ancak eski filtre sürücüsü tutar çalışma yeniden yoksa, avantajlarını almak istediğiniz herhangi bir zamanda yeniden başlatmayı seçebilirsiniz. Bu nedenle, bir güncelleştirme filtre sürücüsü dışında yeniden başlatma olmadan sonra **diğer iyileştirmeler ve düzeltmeler mobility hizmetinin avantajlarından gerçekleşen**. Bu nedenle, önerilen olsa da her yükseltme işleminden sonra yeniden başlatmak için zorunlu değildir. Yeniden başlatma zorunlu olduğunda hakkında daha fazla bilgi için ayarlanmış [mobility Aracısı yükselttikten sonra kaynak makinenin yeniden başlatılması ](https://aka.ms/v2a_asr_reboot) hizmet güncelleştirmeleri Azure Site recovery'de bölümünde.

> [!TIP]
>Yükseltme, bakım penceresi sırasında zamanlama ile ilgili en iyi uygulamalar için bkz: [en son işletim sistemi/kernel sürümleri için destek](https://aka.ms/v2a_asr_upgrade_practice) Azure Site recovery'de hizmet güncelleştirmeleri de.

## <a name="lvm-support-from-920-version"></a>9.20 sürümünden LVM desteği

9.20 sürümünden önce yalnızca veri diskleri için LVM destekleniyordu. makinesiyse bir disk bölümünde olmalı ve LVM birim olmaması gerekir.

Gelen [9.20 sürüm](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), [LVM işletim sistemi diskinde](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) desteklenir. Bu destek için en son sürümünü kullanın.

## <a name="insufficient-space-errorid-95524"></a>Yetersiz alan (errorID: 95524)

Mobility Aracısı kaynak makineyi açın kopyalandığında, en az 100 MB boş alan gereklidir. Bu nedenle, kaynak makineniz boş alan gerekli olduğundan emin olun ve işlemi yeniden deneyin.

## <a name="vss-installation-failures"></a>VSS yükleme hataları

VSS, Mobility Aracısı yüklemesinin parçası yüklemedir. Bu hizmet oluşturma sürecinde uygulama tutarlı kurtarma noktalarına kullanılır. Hataları VSS yüklemesi sırasında birden çok nedenlerden ötürü oluşabilir. Tam hataları belirlemek için başvurmak **c:\ProgramData\ASRSetupLogs\ASRUnifiedAgentInstaller.log**. Aşağıdaki bölümde, bazı yaygın hatalar ve çözüm adımları vurgulanır.

### <a name="vss-error--2147023170-0x800706be---exit-code-511"></a>VSS hatası-2147023170 [0x800706BE] - 511 çıkış kodu

Bu sorun, virüsten koruma yazılımının Azure Site Recovery hizmetleri işlemlerini engellediğinde çoğunlukla görülür. Bu sorunu çözmek için:

1. Bahsedilen tüm klasörleri dışarıda [burada](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program).
2. Windows DLL kaydını engelini kaldırmak için virüsten koruma sağlayıcınız tarafından yayımlanan yönergeleri izleyin.

### <a name="vss-error-7-0x7---exit-code-511"></a>7 [0x7] - çıkış kodu 511 VSS hatası

Bu çalışma zamanı hatası ve VSS'yi yüklemek için yetersiz bellek nedeniyle neden olur Bu işlemin başarıyla tamamlanması için disk alanını artırmak için emin olun.

### <a name="vss-error--2147023824-0x80070430---exit-code-517"></a>VSS hatası-2147023824 [0x80070430] - 517 çıkış kodu

Azure Site Recovery VSS sağlayıcısı hizmetidir. Bu hata oluşur [silinmek üzere işaretlenmiş](https://msdn.microsoft.com/library/ms838153.aspx). Kaynak makinede el ile yüklemeyi aşağıdaki komutu çalıştırarak VSS deneyin

`C:\Program Files (x86)\Microsoft Azure Site Recovery\agent>"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

### <a name="vss-error--2147023841-0x8007041f---exit-code-512"></a>VSS hatası-2147023841 [0x8007041F] - 512 çıkış kodu

Azure Site Recovery VSS sağlayıcısı hizmeti veritabanı olduğunda bu hata oluşur [kilitli](https://msdn.microsoft.com/library/ms833798.aspx). Kaynak makinede el ile yüklemeyi aşağıdaki komutu çalıştırarak VSS deneyin

`C:\Program Files (x86)\Microsoft Azure Site Recovery\agent>"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

### <a name="vss-exit-code-806"></a>VSS çıkış kodu 806

Yükleme için kullanılan kullanıcı hesabının CSScript komutu yürütmek için izinlere sahip olmadığında bu hata oluşur. Bu betiği yürütün ve işlemi yeniden denemek için kullanıcı hesabına gereken izinler sağlayın.

### <a name="other-vss-errors"></a>Diğer VSS hataları

Kaynak makinede el ile yüklemeyi aşağıdaki komutu çalıştırarak VSS sağlayıcısı hizmeti deneyin

`C:\Program Files (x86)\Microsoft Azure Site Recovery\agent>"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`



## <a name="vss-error---0x8004e00f"></a>VSS hatası - 0x8004E00F

Bu hata genellikle DCOM ve DCOM kritik durumda olan sorunları nedeniyle mobility aracısının yüklenmesi sırasında karşılaşıldı.

Hatanın nedenini belirlemek için aşağıdaki yordamı kullanın.

**Yükleme günlüklerini inceleyin**

1. C:\ProgramData\ASRSetupLogs\ASRUnifiedAgentInstaller.log bulunan yükleme günlüğü'nü açın.
2. Bu sorun aşağıdaki hata varlığını gösterir:

    Mevcut uygulama kaydı siliniyor...  Katalog nesnesi oluşturma uygulamaları koleksiyonunu alma 

    HATA:

    - Hata kodu:-2147164145 [0x8004E00F]
    - Çıkış kodu: 802

Bu sorunu çözmek için:

İlgili kişi [Microsoft Windows platformu ekibinden](https://aka.ms/Windows_Support) DCOM sorunu giderme konusunda yardım almak için.

DCOM sorun çözüldüğünde, Azure Site Recovery VSS sağlayıcısı aşağıdaki komutu kullanarak el ile yeniden yükleyin:
 
**C:\Program dosyaları (x86) \Microsoft Azure Site Recovery\agent > "C:\Program dosyaları (x86) \Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd**
  
Uygulama tutarlılığı, olağanüstü durum kurtarma gereksinimlerinizi kritik durumda değilse, VSS sağlayıcısı yüklemesi devre dışı bırakabilir. 

Azure Site Recovery VSS sağlayıcısı yüklemesi atlayabilir ve Azure Site Recovery VSS sağlayıcısı yüklemeden el ile yüklemek için:

1. Mobility hizmetini yükleyin. 
   > [!Note]
   > 
   > 'Posta yükleme yapılandırma' adım yükleme başarısız olur. 
2. VSS yüklemeyi atlamak için:
   1. Konumunda bulunan Azure Site Recovery Mobility hizmeti yükleme dizinini açın:
   
      C:\Program dosyaları (x86) \Microsoft Azure Site Recovery\agent
   2. Azure Site Recovery VSS sağlayıcısı yükleme komut dosyasını değiştirmek **nMageVSSProvider_Install** ve **InMageVSSProvider_Uninstall.cmd** her zaman aşağıdaki satırı ekleyerek başarılı olması için:
    
      ```     
      rem @echo off
      setlocal
      exit /B 0
      ```

3. Mobility Aracısı yükleme el ile yeniden çalıştırın. 
4. Yükleme başarılı olur ve sonraki adıma taşır **yapılandırma**, eklediğiniz satırları kaldırın.
5. VSS sağlayıcısını yüklemek için yönetici olarak bir komut istemi açın ve aşağıdaki komutu çalıştırın:
   
    **C:\Program dosyaları (x86) \Microsoft Azure Site Recovery\agent >.\InMageVSSProvider_Install.cmd**

9. Bir hizmet olarak Windows Hizmetleri ASR VSS sağlayıcısı yüklü olduğunu doğrulayın ve ASR VSS sağlayıcısı listelendiğini doğrulamak için bileşen hizmeti MMC'yi açın.
10. VSS sağlayıcısını yüklerseniz başarısız, CX CAPI2 izinleri hataları çözmeye çalışmak devam eder.

## <a name="vss-provider-installation-fails-due-to-the-cluster-service-being-enabled-on-non-cluster-machine"></a>Küme olmayan makine üzerinde etkinleştiriliyor Küme hizmetinin nedeniyle VSS sağlayıcısı yüklemesi başarısız olur

Bu sorun, Azure Site Recovery Mobility Aracısı yüklemesinin VSS sağlayıcısı yüklenmesini engelleyen com ile ilgili bir sorun nedeniyle ASAzure Site RecoveryR VSS sağlayıcısı yüklemesi adımı sırasında başarısız olmasına neden olur.
 
### <a name="to-identify-the-issue"></a>Sorunu tanımlamak için

C:\ProgramData\ASRSetupLogs\UploadedLogs yapılandırma sunucusunda bulunan günlüğünde\<tarih-saat > UA_InstallLogFile.log, şu özel durum bulacaksınız:

COM + Microsoft Dağıtılmış İşlem Düzenleyicisi ile iletişim kuramadı (HRESULT özel durum: 0x8004E00F)

Bu sorunu çözmek için:

1.  Bu makine bir küme içi makine olduğundan ve küme bileşenleri kullanılmayan doğrulayın.
3.  Bileşenleri kullanılmayan küme bileşenleri makineden kaldırın.

## <a name="drivers-are-missing-on-the-source-server"></a>Kaynak sunucuda sürücüleri eksik

Mobility Aracısı yüklemesi başarısız olursa, bazı gerekli sürücüler bazı denetim kümeleri üzerinde eksik olup olmadığını belirlemek için C:\ProgramData\ASRSetupLogs altında günlüklerini inceleyin.
 
Bu sorunu çözmek için:
  
1. Regedit.msc gibi bir kayıt defteri düzenleyicisi kullanarak kayıt defterini açın.
2. HKEY_LOCAL_MACHINE\System düğümünü açın.
3. Denetim sistemi düğümünde bulun ayarlar.
4. Her denetim kümesini açın ve aşağıdaki Windows sürücülerinin mevcut olduğunu doğrulayın:

   - Atapi
   - Vmbus
   - storflt
   - storvsc
   - intelide
 
Eksik sürücülerin yeniden yükleyin.

## <a name="next-steps"></a>Sonraki adımlar

[Bilgi nasıl](vmware-azure-tutorial.md) VMware Vm'leri için olağanüstü durum kurtarma ayarlama.
