---
title: Azure sanal makine seri Konsolu | Microsoft Docs
description: Azure sanal makineleri için çift yönlü seri konsol.
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
ms.openlocfilehash: 00a776131321500386b507f45ea84817b08147f7
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37867870"
---
# <a name="virtual-machine-serial-console-preview"></a>Sanal makinenin seri konsol (Önizleme) 


Azure'da sanal makine seri konsolu, Linux ve Windows sanal makineler için metin tabanlı bir konsol erişim sağlar. Bu seri bağlantı COM1 için sanal makinenin seri bağlantı noktasıdır ve sanal makineye erişim sağlar ve sanal makinenin ağa ilgili olmayan / işletim sistemi durumu. Seri konsol erişimi için bir sanal makine şu anda yalnızca Azure portalı üzerinden yapılır ve yalnızca VM katkıda bulunanı olan kullanıcılar için veya sanal makineye erişimi yukarıda izin. 

> [!Note] 
> Kullanım koşullarını kabul etmiş olursunuz şartıyla önizlemeleri için kullanılabilir hale getirilir. Daha fazla bilgi için bkz. [Microsoft Azure ek kullanım koşulları Microsoft Azure önizlemeleri için.] (https://azure.microsoft.com/support/legal/preview-supplemental-terms/) Bu hizmeti şu anda aşamasındadır **genel Önizleme** ve genel Azure bölgelerine seri konsoluna erişimi sanal makineler için kullanılabilir. Bu noktada seri konsol kullanılabilir Azure kamu, Azure Almanya ve Azure Çin bulut değil.


## <a name="prerequisites"></a>Önkoşullar 

* Kaynak Yönetimi dağıtım modeline kullanıyor olmanız gerekir. Klasik dağıtımlar desteklenmez. 
* Sanal makinesinin olmalıdır [önyükleme tanılaması](boot-diagnostics.md) etkin 
* Seri konsol kullanarak hesabı olmalıdır [katkıda bulunan rolü](../../role-based-access-control/built-in-roles.md) VM için ve [önyükleme tanılaması](boot-diagnostics.md) depolama hesabı. 
* Linux distro için özel ayarları için bkz: [seri konsol için Linux erişen](#accessing-serial-console-for-linux)


## <a name="open-the-serial-console"></a>Seri Konsolu
sanal makineler için seri konsol üzerinden erişilebilir, yalnızca [Azure portalında](https://portal.azure.com). Portal aracılığıyla sanal makineler için seri konsoluna erişmek için adımları aşağıda verilmiştir 

  1. Azure portalını açın
  2. Sol taraftaki menüde, sanal makineleri seçin.
  3. VM listesinde tıklayın. VM için genel bakış sayfası açılır.
  4. Destek + sorun giderme bölümüne aşağı kaydırın ve seri konsol (Önizleme) seçeneğine tıklayın. Seri konsolu ile yeni bir bölme açılır ve bağlantıyı başlatın.

![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-connect.gif)


> [!NOTE] 
> Seri konsol yapılandırılmış bir parolayla yerel bir kullanıcı gerektirir. Şu anda yalnızca SSH ortak anahtarı ile yapılandırılan VM'ler seri konsoluna erişimi yoktur. Parola ile yerel bir kullanıcı oluşturmak için takip [VM erişimi uzantısı](https://docs.microsoft.com/azure/virtual-machines/linux/using-vmaccess-extension) ve parola ile yerel bir kullanıcı oluşturun.

### <a name="disable-feature"></a>Özelliği devre dışı
Seri konsol işlevlerini belirli sanal makineler için bu sanal makinenin önyükleme tanılama ayarı devre dışı bırakılarak devre dışı bırakılabilir.

## <a name="serial-console-security"></a>Seri konsol güvenlik 

### <a name="access-security"></a>Erişimi güvenliği 
Seri konsol erişimi olan kullanıcılarla sınırlıdır [VM katkıda bulunanlar](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) veya sanal makineye erişimi. AAD kiracınızın çok faktörlü kimlik doğrulaması gerektiren sonra aracılığıyla erişimini olduğu gibi seri konsoluna erişimi MFA ayrıca gerekir [Azure portalında](https://portal.azure.com).

### <a name="channel-security"></a>Kanalı güvenliği
Tüm veriler geri gönderilir ve kablo İleri şifrelenir.

### <a name="audit-logs"></a>Denetim günlükleri
Seri konsola tüm erişim şu anda oturum [önyükleme tanılaması](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) sanal makinenin günlükleri. Bu günlükler için erişim sahibi ve Azure sanal makine yöneticisi tarafından denetlenen.  

>[!CAUTION] 
Konsolun içinden çalıştırma komutları içeriyorsa ya da parolalar, gizli dizileri, kullanıcı adlarını veya diğer bir form herhangi biri kişisel bilgilerin (PII) çıktı Konsolu için erişim parolası, oturumunuz açıkken, bu sanal makine önyükleme tanılaması için yazılır Tüm diğer görünen metni, uygulanması seri Konsolu scrollback işlevselliğinin bir parçası olarak birlikte günlüğe kaydeder. Bu günlükler döngüsel ve gizli dizileri ve/veya PII içerebilir, her şey için SSH konsolunu kullanmanın en iyi yöntemin izlenmesi önerilir ancak tanılama depolama hesabı için Okuma izinleri olan tek kişilere, onlara yönelik erişimi sahiptir. 

### <a name="concurrent-usage"></a>Eşzamanlı kullanım
Seri konsola bir kullanıcı bağlandığından ve başka bir kullanıcı, aynı sanal makineye erişimi başarıyla istekleri, ilk kullanıcı bağlantısı kesilir ve duran ve fiziksel konsol ve yeni bir bırakarak ilk kullanıcı benzer şekilde ikinci kullanıcı bağlı Kullanıcı oturan.

>[!CAUTION] 
Başka bir deyişle, bağlantısı kesilse kullanıcı kapatılacak değil! Bağlantıyı Kes (aracılığıyla SIGHUP veya benzer bir mekanizma) bağlı bir oturum kapatma zorunlu tutma becerisi hala Yol Haritası ' dir. Windows için otomatik bir zaman aşımı SAC içinde etkin ancak Linux için terminal zaman aşımı ayarını yapılandırabilirsiniz. Bunu yapmak için yalnızca ekleyin `export TMOUT=600` .bash_profile veya kullanıcı profili, 10 dakika sonra oturumu zaman aşımına konsolunda oturum.

### <a name="disable-feature"></a>Özelliği devre dışı
Seri konsol işlevlerini belirli sanal makineler için bu sanal makinenin önyükleme tanılama ayarı devre dışı bırakılarak devre dışı bırakılabilir.

## <a name="common-scenarios-for-accessing-serial-console"></a>Seri konsoluna erişmek için genel senaryolar 
Senaryo          | Seri konsol eylemleri                |  İşletim sistemi uygulanabilirliği 
:------------------|:-----------------------------------------|:------------------
FSTAB dosyası bozuk | Bir metin düzenleyici kullanarak fstab dosyasını düzeltin ve devam etmek için anahtarı girin. Bkz: [fstab sorunlarını düzeltme](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors) | Linux 
Yanlış güvenlik duvarı kuralları | Seri konsola erişin, iptables veya Windows Güvenlik duvarı kuralları Düzelt | Linux/Windows 
Dosya Sistemi Bozulması/işaretleyin | Seri konsola erişin, dosya sistemi Kurtar | Linux/Windows 
SSH/RDP yapılandırma sorunları | Seri konsola erişin, ayarlarını değiştirme | Linux/Windows 
Sistem ağ kilitleme| Sistemi yönetmek için portal aracılığıyla erişim seri Konsolu | Linux/Windows 
Önyükleme yükleyicisi ile etkileşim kurma | Erişim GRUB/BCD seri Konsolu aracılığıyla | Linux/Windows 

## <a name="accessing-serial-console-for-linux"></a>Linux için seri konsoluna erişme
Okuma ve seri bağlantı noktasına konsol iletileri yazma, düzgün bir şekilde seri konsol için sırada, konuk işletim sistemi yapılandırılması gerekir. Çoğu [desteklenen Azure Linux dağıtımı](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) varsayılan olarak yapılandırılmış seri Konsolu. Seri konsol bölümüne şirket Portalı'nda yalnızca tıklayarak konsoluna erişim sağlar. 

### <a name="access-for-red-hat"></a>Red Hat erişimi 
Azure'da kullanılabilen Red Hat görüntülerini, varsayılan olarak etkin konsol erişimi. Red Hat tek kullanıcı modunda varsayılan olarak devre dışı etkinleştirilmesi, kök kullanıcının olmasını gerektirir. Tek kullanıcı modunda etkinleştirmek için bir gereksinimi varsa, aşağıdaki yönergeleri kullanın:

1. Red Hat sisteme SSH aracılığıyla oturum açın
2. Kök kullanıcı parolası etkinleştir 
 * `passwd root` (güçlü bir kök parola ayarlayın)
3. Kök kullanıcı yalnızca ttyS0 oturum açabildiğinizden emin olun.
 * `edit /etc/ssh/sshd_config` ve içinde PermitRootLog Hayır ayarlandığından emin olun
 * `edit /etc/securetty file` yalnızca oturum açma ttyS0 aracılığıyla izin vermek için 

Sistem tek kullanıcı modunda önyükleniyorsa artık, kök parolası ile oturum açabilir.

GRUB RHEL 7.4 + veya 6.9 + etkinleştirebilirsiniz tek kullanıcı modunda istemleri için yönergeler bunun yerine bkz [burada](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/installation_guide/s1-rescuemode-booting-single)

### <a name="access-for-ubuntu"></a>Ubuntu için erişim 
Azure'da ubuntu görüntülerinde varsayılan olarak etkin konsol erişebilir. Sistem tek kullanıcı modunda önyükleniyorsa ek kimlik bilgileri olmadan erişebilir. 

### <a name="access-for-coreos"></a>CoreOS erişimi
Azure'da CoreOS görüntülerinden, varsayılan olarak etkin konsol erişimi. Gerekli sistem GRUB parametrelerinin değiştirilmesi ve ekleme yoluyla tek kullanıcı moduna önyüklenebilecek varsa `coreos.autologin=ttyS0` oturum açma ve seri konsolunda kullanılabilir çekirdek kullanıcı olanak sağlar. 

### <a name="access-for-suse"></a>SUSE erişim
SLES görüntülerini Azure'da sunulan, varsayılan olarak etkin konsol erişebilir. Azure üzerinde SLES eski sürümleri kullanıyorsanız izleyin [KB makalesi](https://www.novell.com/support/kb/doc.php?id=3456486) seri konsol etkinleştirmek için. Sistem Acil Modu'nda önyüklenir durumunda yeni görüntüleri, SLES 12 SP3 + ayrıca seri Konsolu aracılığıyla erişim sağlar.

### <a name="access-for-centos"></a>CentOS erişimi
Azure'da centOS görüntülerinden, varsayılan olarak etkin konsol erişimi. Tek kullanıcı modu, Red Hat görüntülerini yukarıdaki benzer yönergeleri izleyin. 

### <a name="access-for-oracle-linux"></a>Oracle Linux için erişim
Azure'da Oracle Linux görüntüleri, varsayılan olarak etkin konsol erişebilir. Tek kullanıcı modu, Red Hat görüntülerini yukarıdaki benzer yönergeleri izleyin.

### <a name="access-for-custom-linux-image"></a>Erişim için özel bir Linux görüntüsü
Seri konsol özel Linux VM görüntünüz için etkinleştirmek üzere bir terminal ttyS0 üzerinde çalıştırılacak /etc/inittab konsol erişimi etkinleştirin. Bu inittab dosyasına eklemek için bir örnek aşağıda verilmiştir 

`S0:12345:respawn:/sbin/agetty -L 115200 console vt102` 

## <a name="errors"></a>Hatalar
Çoğu hataların doğası ve bağlantı adresi bunlar yeniden deneniyor geçicidir. Aşağıdaki tabloda, hataları ve risk azaltma listesini gösterir 

Hata                            |   Risk azaltma 
:---------------------------------|:--------------------------------------------|
Önyükleme tanılama ayarları alınamadı. '<VMNAME>'. Seri konsol kullanmak için bu VM için o önyükleme tanılaması etkin emin olun. | Sanal makine olduğundan emin olun [önyükleme tanılaması](boot-diagnostics.md) etkin. 
Durdurulan serbest bırakılmış durumda vm'dir. VM'yi başlatın ve seri konsol bağlantısı yeniden deneyin. | Sanal makinenin seri konsol erişmek için başlatılmış durumda olması gerekir
Bu sanal makine seri konsolu kullanmak için gerekli izinlere sahip değil. En az olduğundan emin olun VM katkıda bulunan rol izinleri.| Seri konsol erişimi erişmek için belirli bir izni gerektirir. Bkz: [erişim gereksinimlerini](#prerequisites) için Ayrıntılar
Önyükleme tanılaması depolama hesabı için kaynak grubu belirlenemiyor '<STORAGEACCOUNTNAME>'. Bu VM için önyükleme tanılaması etkin ve bu depolama hesabına erişiminiz olduğunu doğrulayın. | Seri konsol erişimi erişmek için belirli bir izni gerektirir. Bkz: [erişim gereksinimlerini](#prerequisites) için Ayrıntılar

## <a name="known-issues"></a>Bilinen sorunlar 
Hala Önizleme aşamaları seri konsol erişimi için de bizim yaptığımız gibi biz bazı bilinen sorunlar ile çalışıyorsanız, olası geçici çözümleri ile bunların bir listesi aşağıda verilmiştir 

Sorun                           |   Risk azaltma 
:---------------------------------|:--------------------------------------------|
Sanal makine ölçek kümesi örneği seri konsolu ile seçeneği yoktur |  Önizleme sırasında sanal makine ölçek kümesi örneklerine ait seri konsoluna erişimi desteklenmiyor.
Ulaşmaktan bağlantı başlık satırında bir günlük göstermez sonra girin | [Ulaşmaktan girin hiçbir şey yapılmaz](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md)


## <a name="frequently-asked-questions"></a>Sık sorulan sorular 
**SORU. Nasıl geri bildirim gönderebilir miyim?**

A. Giderek bir sorun geri bildirim sağlamak https://aka.ms/serialconsolefeedback. Alternatif olarak daha az (tercih edilen) gönderme aracılığıyla geri bildirim azserialhelp@microsoft.com veya sanal makine kategorisi http://feedback.azure.com

**Q.I hata Al "mevcut konsol, çakışan işletim sistemi türü istenen Linux işletim sistemi türü ile" Windows"var?**

A. Bu, bu sorunu gidermek için bilinen bir sorundur açıp [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) bash modu ve yeniden deneyin.

**SORU. Burada bir destek talebi belirtebileceği seri konsoluna erişmek gönderemiyorum?**

A. Bu önizleme özelliği Azure Önizleme koşulları kapsamındadır. Desteği, en iyi yukarıda belirtilen kanallar aracılığıyla işlenir. 

## <a name="next-steps"></a>Sonraki adımlar
* Seri konsol için de kullanılabilir olan [Windows](../windows/serial-console.md) VM'ler
* Daha fazla bilgi edinin [bootdiagnostics](boot-diagnostics.md)