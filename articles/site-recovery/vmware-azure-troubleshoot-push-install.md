---
title: Olağanüstü durum kurtarma için çoğaltmayı etkinleştirirken Mobility hizmeti anında yükleme hatalarını giderme | Microsoft Docs
description: Olağanüstü durum kurtarma için çoğaltmayı etkinleştirirken Mobility hizmetlerini yükleme hatalarını giderme
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.author: ramamill
ms.date: 10/29/2018
ms.openlocfilehash: a9738f95ce8a0de750ffa348e167bce3b0e659f6
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2018
ms.locfileid: "51821404"
---
# <a name="troubleshoot-mobility-service-push-installation-issues"></a>Mobility hizmeti anında yükleme sorunlarını giderme

Mobility hizmetinin yüklenmesi sırasında bir anahtar çoğaltmayı etkinleştirme sırasında adımdır. Bu adım başarısını yalnızca önkoşulları sağladıktan ve desteklenen yapılandırmalar ile çalışma bağlıdır. Mobility hizmeti yüklemesi sırasında karşılaşacağınız en yaygın hataları nedeniyle üzeresiniz

* Kimlik bilgisi/ayrıcalık hataları
* Bağlantı hataları
* Desteklenmeyen işletim sistemleri
* VSS yükleme hataları

Çoğaltmayı etkinleştirdiğinizde göndermeye çalıştığında Azure Site Recovery mobility Hizmeti Aracısı sanal makinenize yükleyin. Bunun bir parçası olarak, yapılandırma sunucusu ile sanal makineye bağlanın ve aracıyı kopyalamak çalışır. Başarılı yükleme etkinleştirmek için aşağıda verilen adım adım sorun giderme yönergeleri izleyin.

## <a name="credentials-check-errorid-95107--95108"></a>Kimlik onay (errorID: 95107 & 95108)

* Çoğaltmayı etkinleştirme sırasında seçilen kullanıcı hesabı olup olmadığını doğrulamak **geçerli, doğru**.
* Azure Site Recovery gerektirir **yönetici ayrıcalığı** anında yükleme gerçekleştirmek için.
  * Windows için kullanıcı hesabının yönetici erişimi olup olmadığını doğrulayın. yerel veya etki alanı, kaynak makinede.
  * Bir etki alanı hesabı kullanmıyorsanız yerel bilgisayarda Uzak kullanıcı erişim denetimini devre dışı bırakmanız gerekir.
    * Yeni bir DWORD kayıt defteri anahtarının hkey_local_machıne\software\microsoft\windows\currentversion\policies\system altında uzak kullanıcı erişim denetimini devre dışı bırakma eklemek için: LocalAccountTokenFilterPolicy. Değer 1 olarak ayarlayın. Bu adım yürütülecek komut isteminden aşağıdaki komutu çalıştırın:

         `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`
  * Linux için mobility Aracısı başarılı yükleme için kök hesabı seçmeniz gerekir.

Seçilen kullanıcı hesabının kimlik bilgilerini değiştirmek istiyorsanız, verilen yönergeleri izleyin [burada](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="connectivity-check-errorid-95117--97118"></a>**Bağlantı denetimi (errorID: 95117 & 97118)**

* Yapılandırma Sunucusu'na kaynak makinenizden ping mümkün olduğundan emin olun. Çoğaltmayı etkinleştirme sırasında genişleme işlem sunucusu seçtiyseniz, işlem sunucusu, kaynak makineden ping mümkün olduğundan emin olun.
  * Kaynak sunucu makine komut satırından Telnet kullanma ping yapılandırma sunucusu / genişleme işlem sunucusu ile https bağlantı noktası (herhangi bir ağ bağlantısı sorunları veya güvenlik duvarı bağlantı noktası engelleme sorunları olup olmadığını görmek için aşağıda gösterildiği gibi 135).

     `telnet <CS/ scale-out PS IP address> <135>`
  * Hizmet durumunu **Inmage Scout VX Aracısı-Sentinel/Outpost**. Çalışır durumda değilse hizmeti başlatın.
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

## <a name="file-and-printer-sharing-services-check-errorid-95105--95106"></a>Dosya ve Yazıcı Paylaşımı hizmetleri onay (errorID: 95105 & 95106)

Bağlantı denetimi sonra dosya ve Yazıcı Paylaşımı hizmet etkin değilse, sanal makinenizde doğrulayın.

İçin **windows 2008 R2 ve önceki sürümler**,

* Dosya ve yazıcı paylaşımını Windows Güvenlik Duvarı üzerinden etkinleştirmek için
  * Denetim Masası -> Sistem ve Güvenlik -> Windows Güvenlik Duvarı'nı Gelişmiş'i tıklatın, soldaki bölmede -> Ayarlar -> konsol ağacında gelen kuralları tıklayın.
  * Dosya ve Yazıcı Paylaşımı (NB-Oturum-gelen) ve dosya ve Yazıcı Paylaşımı (SMB-gelen) kurallarını bulun. Her kural için bir kurala sağ tıklayın ve ardından **kuralı etkinleştir**.
* Dosya Paylaşımı ile Grup İlkesi'ni etkinleştirmek için
  * Başlat'a gidin, arama ve gpmc.msc yazın.
  * Gezinti bölmesinde, aşağıdaki klasörleri açın: yerel bilgisayar ilkesi, kullanıcı yapılandırma, Yönetim Şablonları, Windows bileşenleri ve ağ paylaşımı.
  * Ayrıntılar bölmesinde **kullanıcı profilleri içinde dosyaları paylaşmasını engelleyebilir**. Grup İlkesi ayarını devre dışı bırakın ve dosyaları paylaşmak kullanıcının özelliğini etkinleştirmek için devre dışı bırak Değişikliklerinizi kaydetmek için Tamam'a tıklayın. Daha fazla bilgi edinmek için tıklayın [burada](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754359(v=ws.10)).

İçin **sonraki sürümlerinde**, sağlanan yönergeleri izleyin [burada](vmware-azure-install-mobility-service.md) dosya ve Yazıcı Paylaşımı'nı etkinleştirmek için.

## <a name="windows-management-instrumentation-wmi-configuration-check"></a>Windows Yönetim Araçları (WMI) yapılandırma denetimi

Dosya ve yazıcı hizmetlerini iade ettikten sonra WMI hizmetinin güvenlik duvarı aracılığıyla etkinleştirin.

* Denetim Masası ' nda güvenlik'e tıklayın ve sonra Windows Güvenlik Duvarı'nı tıklatın.
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

## <a name="vss-installation-failures"></a>VSS yükleme hataları

VSS, Mobility Aracısı yüklemesinin parçası yüklemedir. Bu hizmet oluşturma sürecinde uygulama tutarlı kurtarma noktalarına kullanılır. Hataları VSS yüklemesi sırasında birden çok nedenlerden ötürü oluşabilir. Tam hataları belirlemek için başvurmak **c:\ProgramData\ASRSetupLogs\ASRUnifiedAgentInstaller.log**. Aşağıdaki bölümde, bazı yaygın hatalar ve çözüm adımları vurgulanır.

### <a name="vss-error--2147023170-0x800706be---exit-code-511"></a>VSS hatası-2147023170 [0x800706BE] - 511 çıkış kodu

Bu sorun, çoğunlukla bir virüsten koruma yazılımının Azure Site Recovery hizmetleri işlemlerini engellediğinde görülür. Bu sorunu gidermek için

1. Bahsedilen tüm klasörleri dışarıda [burada](vmware-azure-set-up-source.md#exclude-antivirus-on-the-configuration-server).
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