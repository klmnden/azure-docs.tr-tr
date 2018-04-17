---
title: Azure sanal makinesi seri konsol | Microsoft Docs
description: Azure sanal makineleri için çift yönlü seri Konsolu.
services: virtual-machines-linux
documentationcenter: ''
author: harijay
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/05/2018
ms.author: harijay
ms.openlocfilehash: 69f5e29be77f25d649ce357dae6e3905ab2bf6b8
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="virtual-machine-serial-console-preview"></a>Sanal makine seri konsol (Önizleme) 


Azure sanal makine seri konsolunuzdaki Linux ve Windows sanal makineler için metin tabanlı bir konsol erişim sağlar. Bu seri bağlantı COM1 seri bağlantı noktasına sanal makine ve sanal makine erişim sağlar ve sanal makinenin ağa ilgili olmayan / sistem durumu işletim. Seri konsol erişimi için bir sanal makine şu anda yalnızca Azure portal aracılığıyla yapılır ve VM katılımcı olan kullanıcılar için veya sanal makineye erişim yukarıda izin. 

> [!Note] 
> Kullanım koşullarını kabul ediyorum koşuluyla önizlemeleri için kullanılabilir hale getirilir. Daha fazla bilgi için bkz: [Microsoft Azure ek kullanım koşulları'nı Microsoft Azure önizlemeleri.] (https://azure.microsoft.com/support/legal/preview-supplemental-terms/) Bu hizmet olarak şu anda **genel Önizleme** ve seri konsoluna erişimi sanal makineler için genel Azure bölgeler için kullanılabilir. Bu noktada seri konsol kullanılabilir Azure kamu, Azure Almanya ve Azure Çin bulut değil.


## <a name="prerequisites"></a>Önkoşullar 

* Sanal makine olmalıdır [önyükleme tanılama](boot-diagnostics.md) etkin 
* Seri konsol kullanarak hesabı olmalıdır [katkıda bulunan rolü](../../role-based-access-control/built-in-roles.md) VM için ve [önyükleme tanılama](boot-diagnostics.md) depolama hesabı. 
* Linux distro için belirli ayarları için bkz: [seri konsol için Linux erişme](#accessing-serial-console-for-linux)


## <a name="open-the-serial-console"></a>Seri konsol açın
sanal makineler için seri Konsolu aracılığıyla erişilebilir durumda yalnızca [Azure portal](https://portal.azure.com). Portal üzerinden sanal makineler için seri konsoluna erişmek için adımları aşağıda verilmiştir 

  1. Azure portalını açın
  2. Soldaki menüde sanal makineleri seçin.
  3. VM listesinde tıklayın. VM için genel bakış sayfası açılır.
  4. Destek + sorun giderme bölümüne aşağı kaydırın ve seri konsol (Önizleme) seçeneğine tıklayın. Seri konsolu ile yeni bir bölmesini açın ve bağlantıyı başlatın.

![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-connect.gif)


> [!NOTE] 
> Seri konsol yerel bir kullanıcı yapılandırılmış olan bir parola gerektirir. Şu anda yalnızca SSH ortak anahtarı ile yapılandırılmış sanal makineleri seri Konsolu erişebilir değil. Parola ile yerel bir kullanıcı oluşturmak için izlemeniz [VM erişim uzantısı](https://docs.microsoft.com/azure/virtual-machines/linux/using-vmaccess-extension) ve yerel kullanıcı parolasıyla oluşturun.

### <a name="disable-feature"></a>Özelliğini devre dışı bırakma
Seri konsol işlevlerini belirli VM'ler için bu VM'in önyükleme tanılama ayarını devre dışı bırakarak devre dışı bırakılabilir.

## <a name="serial-console-security"></a>Seri konsol güvenlik 

### <a name="access-security"></a>Erişimi güvenliği 
Seri konsol erişimi olan kullanıcılar için sınırlı [VM katkıda bulunanlar](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) ya da sanal makineye erişim üstünde. AAD kiracınızın çok faktörlü kimlik doğrulaması gerektiren sonra uygulamaya erişim aracılığıyla olduğundan seri konsoluna erişimi MFA ayrıca gerekir [Azure portal](https://portal.azure.com).

### <a name="channel-security"></a>Kanalı güvenliği
Tüm verileri geri gönderilir ve kablo İleri şifrelenir.

### <a name="audit-logs"></a>Denetim günlükleri
Seri konsol tüm erişim şu anda oturum [önyükleme tanılama](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) sanal makinenin günlükleri. Bu günlükler erişim sahibi ve Azure sanal makine yöneticisi tarafından denetlenir.  

>[!CAUTION] 
Konsolda çalıştırma komutları içeriyorsa ya da parolaları, gizli, kullanıcı adlarını veya diğer, kişisel bilgilerin (PII) çıkış konsol için erişim parolası günlüğe kaydedilen olsa da, bu sanal makine önyükleme tanılaması için yazılır Tüm diğer görünür metin, seri konsolun scrollback işlevselliği uyarlamasını bir parçası olarak birlikte günlüğe kaydeder. Bu günlükler döngüsel ve gizli anahtarları ve/veya PII içeren herhangi bir şey için SSH konsolunu kullanarak en iyi uygulama olarak önerilir ancak tanılama depolama hesabı için Okuma izinlerine sahip tek kişiler, erişebilir. 

### <a name="concurrent-usage"></a>Eş zamanlı kullanım
Seri konsoluna bir kullanıcının bağlı olduğu ve başka bir kullanıcı başarıyla aynı sanal makinenin erişim isteğinde, ilk kullanıcının bağlantısı kesilecek ve ikinci kullanıcı duran ve fiziksel konsolunda ve yeni bir bırakarak ilk kullanıcı benzer bir şekilde bağlı durduğunu kullanıcı.

>[!CAUTION] 
Başka bir deyişle, bağlantısı kesilen kullanıcının oturumu değil! Bağlantıyı Kes (aracılığıyla SIGHUP veya benzer bir mekanizma) bağlı bir oturum kapatma zorlamak için hala yol haritası özelliğidir. Windows SAC içinde etkin otomatik bir zaman aşımı yoktur, ancak Linux için terminal zaman aşımı ayarını yapılandırabilirsiniz. Bunu yapmak için sadece ekleyin `export TMOUT=600` .bash_profile veya kullanıcı profili için 10 dakika sonra oturum zaman aşımı, konsolunda oturum.

### <a name="disable-feature"></a>Özelliğini devre dışı bırakma
Seri konsol işlevlerini belirli VM'ler için bu VM'in önyükleme tanılama ayarını devre dışı bırakarak devre dışı bırakılabilir.

## <a name="common-scenarios-for-accessing-serial-console"></a>Seri konsol erişmek için genel senaryolar 
Senaryo          | Seri konsol Eylemler                |  İşletim sistemi Uygulanabilirlik 
:------------------|:-----------------------------------------|:------------------
Bozuk FSTAB dosyası | Devam etmek ve fstab dosyasını bir metin düzenleyicisi kullanarak düzeltmek için anahtarı girin. Bkz: [fstab sorunlarını gidermeye yönelik](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors) | Linux 
Yanlış güvenlik duvarı kuralları | Seri Konsolu erişebilir ve iptables ya da Windows Güvenlik duvarı kuralları Düzelt | Linux/Windows 
Dosya Sistemi Bozulması/onay | Seri konsol erişmek ve dosya sistemi kurtarma | Linux/Windows 
SSH/RDP yapılandırma sorunları | Seri Konsolu erişebilir ve ayarlarını değiştirme | Linux/Windows 
Sistem ağ kilitleme| Sistem yönetmek için portal aracılığıyla erişim seri Konsolu | Linux/Windows 
Önyükleme yükleyicisi ile etkileşim kurma | Erişim kaz/BCD seri Konsolu aracılığıyla | Linux/Windows 

## <a name="accessing-serial-console-for-linux"></a>Linux için seri konsol erişme
Düzgün bir şekilde seri Konsolu için sırayla okumak ve konsol iletileri seri bağlantı noktasına yazmak için konuk işletim sistemi yapılandırılmalıdır. Çoğu [destekli Azure Linux dağıtımları](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) varsayılan olarak yapılandırılan seri konsol vardır. Seri Konsol bölümünde portalında yalnızca tıklayarak konsoluna erişimi sağlar. 

### <a name="access-for-redhat"></a>RedHat için erişim 
RedHat görüntüleri Azure üzerinde kullanılabilir, varsayılan olarak etkin Konsolu erişebilir. Red Hat tek kullanıcı modunda varsayılan olarak devre dışı etkinleştirilmesi, kök kullanıcının gerektirir. Tek kullanıcı modunda etkinleştirmek için bir gereksiniminiz varsa, aşağıdaki yönergeleri kullanın:

1. Red Hat sistem SSH aracılığıyla oturum açın
2. Kök kullanıcı için parola etkinleştir 
 * `passwd root` (güçlü kök parolasını ayarlayın)
3. Kök kullanıcı ttyS0 yalnızca oturum açabildiğinizden emin olun
 * `edit /etc/ssh/sshd_config` ve PermitRootLog içinde Hayır ayarlandığından emin olun
 * `edit /etc/securetty file` yalnızca günlüğüne ttyS0 aracılığıyla izin vermek için 

Sistem tek kullanıcı moduna önyüklenirse artık, yoluyla root parolasının oturum açabilirsiniz.

RHEL 7.4 + veya 6.9 + etkinleştirebilirsiniz tek kullanıcı modunda kaz ister, alternatif olarak yönergeleri için bkz [burada](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/installation_guide/s1-rescuemode-booting-single)

### <a name="access-for-ubuntu"></a>Ubuntu için erişim 
Ubuntu görüntüleri Azure üzerinde kullanılabilir, varsayılan olarak etkin Konsolu erişebilir. Sistem tek kullanıcı Modu'nda önyüklenirse, ek kimlik bilgileri olmadan erişebilirsiniz. 

### <a name="access-for-coreos"></a>CoreOS için erişim
CoreOS görüntüleri Azure üzerinde kullanılabilir, varsayılan olarak etkin Konsolu erişebilir. Gerekli sistem kaz parametrelerinin değiştirilmesi ve ekleme yoluyla tek kullanıcı moduna önyüklendiğinde, `coreos.autologin=ttyS0` oturum açma ve seri konsolunda kullanılabilir çekirdek kullanıcı olanak sağlar. 

### <a name="access-for-suse"></a>SUSE için erişim
SLES görüntüleri Azure üzerinde kullanılabilir, varsayılan olarak etkin Konsolu erişebilir. Azure üzerinde SLES eski sürümleri kullanıyorsanız izleyin [KB makalesi](https://www.novell.com/support/kb/doc.php?id=3456486) seri konsol etkinleştirmek için. Sistem Acil Durum Modu'nda önyükleniyor durumda yeni görüntüleri, SLES 12 SP3 + de seri konsol üzerinden erişim sağlar.

### <a name="access-for-centos"></a>CentOS için erişim
CentOS görüntüleri Azure üzerinde kullanılabilir, varsayılan olarak etkin Konsolu erişebilir. Tek kullanıcı modu için yukarıdaki Red Hat görüntülere benzer yönergeleri izleyin. 

### <a name="access-for-oracle-linux"></a>Erişim için Oracle Linux
Oracle Linux görüntüleri Azure üzerinde kullanılabilir, varsayılan olarak etkin Konsolu erişebilir. Tek kullanıcı modu için yukarıdaki Red Hat görüntülere benzer yönergeleri izleyin.

### <a name="access-for-custom-linux-image"></a>Özel Linux görüntü için erişim
Seri konsol özel Linux VM görüntüsü için etkinleştirmek için bir terminal ttyS0 üzerinde çalıştırmak için /etc/inittab konsol erişimi etkinleştirin. Aşağıda bu inittab dosyasında eklemek için bir örnek verilmiştir 

`S0:12345:respawn:/sbin/agetty -L 115200 console vt102` 

## <a name="errors"></a>Hatalar
Hataların çoğu doğasını ve bu bağlantı adresi denemeden geçicidir. Aşağıdaki tablo hataları ve azaltma listesini gösterir 

Hata                            |   Risk azaltma 
:---------------------------------|:--------------------------------------------|
İçin önyükleme tanılaması ayarları alınamadı '<VMNAME>'. Seri konsol kullanmak için bu önyükleme tanılaması bu VM için etkinleştirildiğinden emin olun. | VM sahip olduğundan emin olun [önyükleme tanılama](boot-diagnostics.md) etkin. 
VM durduruldu deallocated bir durumda değil. VM başlatmak ve seri konsol bağlantısı yeniden deneyin. | Sanal makinenin seri konsoluna erişmek için başlatılmış bir durumda olması gerekir
Bu VM seri konsol kullanmak için gerekli izinlere sahip değil. En az olduğundan emin olun VM katkıda bulunan rolü izinleri.| Seri konsol erişimi erişim izni belirli gerektirir. Bkz: [erişim gereksinimleri](#prerequisites) Ayrıntılar için
Önyükleme tanılama depolama hesabı için kaynak grubu belirlenemiyor '<STORAGEACCOUNTNAME>'. Bu VM için önyükleme tanılaması etkin ve bu depolama hesabına erişimi doğrulayın. | Seri konsol erişimi erişim izni belirli gerektirir. Bkz: [erişim gereksinimleri](#prerequisites) Ayrıntılar için

## <a name="known-issues"></a>Bilinen sorunlar 
Biz yine seri konsol erişimi için Önizleme aşamalarını olduğu gibi biz bilinen bazı sorunlar çalışıyorsanız, bunların olası geçici çözümler ile listesi aşağıdadır 

Sorun                           |   Risk azaltma 
:---------------------------------|:--------------------------------------------|
Sanal makine ölçek kümesi örnek seri konsolu ile bir seçenek yoktur |  Önizleme zaman seri konsoluna erişimi sanal makine ölçek kümesi örneklerinin desteklenmiyor.
Devreyi enter bağlantı başlık satırında bir günlük göstermez | [Devreyi girin, hiçbir şey yapmıyor](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md)


## <a name="frequently-asked-questions"></a>Sık sorulan sorular 
**Q. Geri bildirim nasıl gönderebilir miyim?**

A. Giderek bir sorun görüş https://aka.ms/serialconsolefeedback. Alternatif olarak daha az (tercih edilen) geribildirim gönderme aracılığıyla azserialhelp@microsoft.com veya sanal makine kategorisi http://feedback.azure.com

**Q.I hata Al "Varolan konsol çakışan işletim sistemi türü istenen Linux işletim sistemi türü olan" Windows"var?**

A. Bu bu sorunu düzeltmek için bilinen bir sorundur yalnızca açık [Azure bulut Kabuk](https://docs.microsoft.com/azure/cloud-shell/overview) bash modunda ve yeniden deneyin.

**Q. I bir destek servis talebi burada dosya seri Konsolu erişebilir değilim?**

A. Bu önizleme özelliği Azure Önizleme koşulları ele alınmıştır. Bu destek en iyi yukarıda belirtilen kanallar aracılığıyla gerçekleştirilir. 

## <a name="next-steps"></a>Sonraki adımlar
* Seri konsol ayrıca kullanılabilir [Windows](../windows/serial-console.md) VM'ler
* Daha fazla bilgi edinmek [bootdiagnostics](boot-diagnostics.md)