---
title: Olağanüstü durum kurtarma için çoğaltmayı etkinleştirirken Mobility hizmeti anında yükleme hatalarını giderme | Microsoft Docs
description: Olağanüstü durum kurtarma için çoğaltmayı etkinleştirirken Mobility hizmetlerini yükleme hatalarını giderme
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.author: ramamill
ms.date: 12/12/2018
ms.openlocfilehash: 748f4e56b4b7fa52928f8f6507960ec35b5fe6e5
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53314406"
---
# <a name="troubleshoot-mobility-service-push-installation-issues"></a>Mobility hizmeti anında yükleme sorunlarını giderme

Mobility hizmetinin yüklenmesi sırasında bir anahtar çoğaltmayı etkinleştirme sırasında adımdır. Bu adım başarısını yalnızca önkoşulları sağladıktan ve desteklenen yapılandırmalar ile çalışma bağlıdır. Mobility hizmeti yüklemesi sırasında karşılaşacağınız en yaygın hataların nedeni şunlardır:

* Kimlik bilgisi/ayrıcalık hataları
* Oturum açma hataları
* Bağlantı hataları
* Desteklenmeyen işletim sistemleri
* VSS yükleme hataları

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

Etki alanı güven ilişkisi kurulmasını birincil etki alanı ve iş istasyonu arasında kaynak makineye oturum açmaya çalışırken başarısız olduğunda, mobility Aracısı yüklemesi 95518 hata koduyla başarısız oluyor. Bu nedenle, mobility aracısı yüklemek için kullanılan kullanıcı hesabının, kaynak makinenin birincil etki alanı ile oturum açmak için yönetici ayrıcalıkları olduğundan emin olun.

Seçilen kullanıcı hesabının kimlik bilgilerini değiştirmek istiyorsanız, verilen yönergeleri izleyin [burada](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="login-failure-errorid-95519"></a>Oturum açma hatası (errorID: 95519)

Çoğaltmayı etkinleştirme sırasında seçilen kullanıcı hesabı devre dışı bırakıldı. Kullanıcı hesabını etkinleştirmek için makalesine bakabilirsiniz [burada](https://aka.ms/enable_login_user) veya metin değiştirerek aşağıdaki komutu çalıştırın *kullanıcıadı* gerçek kullanıcı adına sahip.
`net user 'username' /active:yes`

## <a name="login-failure-errorid-95520"></a>Oturum açma hatası (errorID: 95520)

Kullanıcı hesabının bir makineye erişmek için birden fazla başarısız deneme çalışmalarını kilitleyecek. Hatanın nedeni şunlar olabilir:

* Yapılandırma Kurulum sırasında sağlanan kimlik bilgileri yanlış veya
* Çoğaltmayı etkinleştirme sırasında seçilen kullanıcı hesabı yanlış.

Bu nedenle, verilen yönergeleri izleyerek seçilen kimlik bilgilerini değiştirme [burada](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation) süre sonra işlemi yeniden deneyin.

## <a name="login-failure-errorid-95521"></a>Oturum açma hatası (errorID: 95521)

Oturum açma sunucusu, kaynak makinede mevcut olmadığı durumlarda, bu hata oluşur. Oturum açma sunucusu olarak kullanım dışı kalması oturum açma isteği başarısız olmasına neden ve bu nedenle mobility Aracısı yüklenemiyor. Başarılı oturum açma için oturum açma sunucusu kaynak makinede kullanılabilir ve oturum açma hizmeti başlatmak emin olun. Ayrıntılı yönergeler için tıklayın [burada](https://support.microsoft.com/en-in/help/139410/err-msg-there-are-currently-no-logon-servers-available).

## <a name="login-failure-errorid-95522"></a>Oturum açma hatası (errorID: 95522)

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
  * Ayrıntılar bölmesinde **kullanıcı profilleri içinde dosyaları paylaşmasını engelleyebilir**. Grup İlkesi ayarını devre dışı bırakın ve dosyaları paylaşmak kullanıcının özelliğini etkinleştirmek için devre dışı bırak Değişikliklerinizi kaydetmek için Tamam'a tıklayın. Daha fazla bilgi edinmek için tıklayın [burada](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754359(v=ws.10)).

İçin **sonraki sürümlerinde**, sağlanan yönergeleri izleyin [burada](vmware-azure-install-mobility-service.md) dosya ve Yazıcı Paylaşımı'nı etkinleştirmek için.

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

Başka bir yaygın başarısızlık nedeni desteklenmeyen bir işletim sistemi nedeniyle olabilir. Desteklenen işletim sistemi/çekirdek sürümü mobilite hizmetinin başarılı yükleme için üzerinde olduğundan emin olun.

Azure Site Recovery tarafından desteklenen işletim sistemleri hakkında bilgi edinmek için bkz bizim [destek matrisi belge](vmware-physical-azure-support-matrix.md#replicated-machines).

## <a name="boot-and-system-partitions--volumes-are-not-the-same-disk-errorid-95309"></a>Önyükleme ve sistem bölümleri veya birimleri aynı diskte (errorID: 95309)

Önce 9.20 sürümü, önyükleme ve sistem bölümleri / birimler farklı disklerde was'da desteklenmeyen bir yapılandırma. Gelen [9.20 sürüm](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), bu yapılandırma desteklenir. Bu destek için en son sürümünü kullanın.

## <a name="system-partition-on-multiple-disks-errorid-95313"></a>Sistem bölümü birden çok diskte (errorID: 95313)

9.20 sürümünden önce desteklenmeyen bir yapılandırma kök bölümü veya birimi birden fazla diskte olduğu. Gelen [9.20 sürüm](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), bu yapılandırma desteklenir. Bu destek için en son sürümünü kullanın.

## <a name="lvm-support-from-920-version"></a>9.20 sürümünden LVM desteği

9.20 sürümünden önce yalnızca veri diskleri için LVM destekleniyordu. makinesiyse bir disk bölümünde olmalı ve LVM birim olmaması gerekir.

Gelen [9.20 sürüm](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), [LVM işletim sistemi diskinde](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) desteklenir. Bu destek için en son sürümünü kullanın.

## <a name="insufficient-space-errorid-95524"></a>Yetersiz alan (errorID: 95524)

Mobility Aracısı kaynak makineyi açın kopyalandığında, en az 100 MB boş alan gereklidir. Bu nedenle, kaynak makineniz boş alan gerekli olduğundan emin olun ve işlemi yeniden deneyin.

## <a name="vss-installation-failures"></a>VSS yükleme hataları

VSS, Mobility Aracısı yüklemesinin parçası yüklemedir. Bu hizmet oluşturma sürecinde uygulama tutarlı kurtarma noktalarına kullanılır. Hataları VSS yüklemesi sırasında birden çok nedenlerden ötürü oluşabilir. Tam hataları belirlemek için başvurmak **c:\ProgramData\ASRSetupLogs\ASRUnifiedAgentInstaller.log**. Aşağıdaki bölümde, bazı yaygın hatalar ve çözüm adımları vurgulanır.

### <a name="vss-error--2147023170-0x800706be---exit-code-511"></a>VSS hatası-2147023170 [0x800706BE] - 511 çıkış kodu

Bu sorun, çoğunlukla bir virüsten koruma yazılımının Azure Site Recovery hizmetleri işlemlerini engellediğinde görülür. Bu sorunu gidermek için

1. Bahsedilen tüm klasörleri dışarıda [burada](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program).
2. Windows DLL kaydını engelini kaldırmak için virüsten koruma sağlayıcınız tarafından yayımlanan yönergeleri izleyin.

### <a name="vss-error-7-0x7---exit-code-511"></a>7 [0x7] - çıkış kodu 511 VSS hatası

Bu çalışma zamanı hatası ve VSS'yi yüklemek için yetersiz bellek nedeniyle neden olur Bu işlemin başarıyla tamamlanması için disk alanını artırmak için emin olun.

### <a name="vss-error--2147023824-0x80070430---exit-code-517"></a>VSS hatası-2147023824 [0x80070430] - 517 çıkış kodu

Azure Site Recovery VSS sağlayıcısı hizmetidir. Bu hata oluşur [silinmek üzere işaretlenmiş](https://msdn.microsoft.com/en-us/library/ms838153.aspx). Kaynak makinede el ile yüklemeyi aşağıdaki komutu çalıştırarak VSS deneyin

`C:\Program Files (x86)\Microsoft Azure Site Recovery\agent>"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

### <a name="vss-error--2147023841-0x8007041f---exit-code-512"></a>VSS hatası-2147023841 [0x8007041F] - 512 çıkış kodu

Azure Site Recovery VSS sağlayıcısı hizmeti veritabanı olduğunda bu hata oluşur [kilitli](https://msdn.microsoft.com/en-us/library/ms833798.aspx). Kaynak makinede el ile yüklemeyi aşağıdaki komutu çalıştırarak VSS deneyin

`C:\Program Files (x86)\Microsoft Azure Site Recovery\agent>"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

### <a name="vss-exit-code-806"></a>VSS çıkış kodu 806

Yükleme için kullanılan kullanıcı hesabının CSScript komutu yürütmek için izinlere sahip olmadığında bu hata oluşur. Bu betiği yürütün ve işlemi yeniden denemek için kullanıcı hesabına gereken izinler sağlayın.

### <a name="other-vss-errors"></a>Diğer VSS hataları

Kaynak makinede el ile yüklemeyi aşağıdaki komutu çalıştırarak VSS sağlayıcısı hizmeti deneyin

`C:\Program Files (x86)\Microsoft Azure Site Recovery\agent>"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

## <a name="next-steps"></a>Sonraki adımlar

[Bilgi nasıl](vmware-azure-tutorial.md) VMware Vm'leri için olağanüstü durum kurtarma ayarlama.
