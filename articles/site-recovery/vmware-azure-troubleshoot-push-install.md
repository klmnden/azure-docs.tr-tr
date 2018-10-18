---
title: Mobility hizmeti anında yükleme hataları sırasında Replication(VMware to Azure) etkinleştirme sorunlarını giderme | Microsoft Docs
description: Azure sanal makineleri çoğaltırken mobility hizmeti/göndererek yükleme hataları giderin.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.author: ramamill
ms.date: 09/19/2018
ms.openlocfilehash: 4c57d048f4c3222ac180355a6a700562415f601c
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49390202"
---
# <a name="troubleshoot-mobility-service-push-installation-issues"></a>Mobility hizmeti anında yükleme sorunlarını giderme

Mobility hizmetinin yüklenmesi sırasında bir anahtar çoğaltmayı etkinleştirme sırasında adımdır. Bu adım başarısını yalnızca önkoşulları sağladıktan ve desteklenen yapılandırmalar ile çalışma bağlıdır. Mobility hizmeti yüklemesi sırasında karşılaşacağınız en yaygın hataları nedeniyle üzeresiniz

* Kimlik bilgisi/ayrıcalık hataları
* Bağlantı hataları
* Desteklenmeyen işletim sistemleri

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
  * Kaynak sunucu makine komut satırından Telnet kullanma ping yapılandırma sunucusu / genişleme işlem sunucusu ile https bağlantı noktası (varsayılan 9443) herhangi bir ağ bağlantısı sorunları veya güvenlik duvarı bağlantı noktası engelleme sorunları olup olmadığını görmek için aşağıda gösterildiği gibi.

     `telnet <CS/ scale-out PS IP address> <port>`

  * Bağlanmak bulamıyorsanız, gelen bağlantı noktası 9443 yapılandırma sunucusu / genişleme işlem sunucusu sağlar.
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

İçin **sonraki sürümlerinde**, sağlanan yönergeleri izleyin [burada](vmware-azure-install-mobility-service.md#install-mobility-service-by-push-installation-from-azure-site-recovery) dosya ve Yazıcı Paylaşımı'nı etkinleştirmek için

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

## <a name="next-steps"></a>Sonraki adımlar

[Bilgi nasıl](vmware-azure-tutorial.md) VMware Vm'leri için olağanüstü durum kurtarma ayarlama.