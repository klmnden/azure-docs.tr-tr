---
title: GRUB ve tek kullanıcı modu Azure seri konsol | Microsoft Docs
description: Azure sanal makineler'de grub seri konsol kullanarak.
services: virtual-machines-linux
documentationcenter: ''
author: alsin
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/14/2018
ms.author: alsin
ms.openlocfilehash: 059cb0cbc7e62af16dbf95693be421feebcc1ee0
ms.sourcegitcommit: 76797c962fa04d8af9a7b9153eaa042cf74b2699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2018
ms.locfileid: "42061146"
---
# <a name="use-serial-console-to-access-grub-and-single-user-mode"></a>GRUB ve tek kullanıcı modu erişmek için seri Konsolu
Tek kullanıcı modunda, en az bir işlevselliğe sahip en az bir ortamdır. Daha az Hizmetleri arka planda çalıştırabilir ve çalışma düzeyi bağlı olarak bir dosya sistemi bile otomatik olarak takılı önyükleme sorunlarını araştırmanıza veya ağ sorunları için faydalı olabilir. Bu, bozuk bir dosya, bozuk bir fstab sistemi gibi durumlarda araştırmak veya ağ bağlantısı (yanlış iptables yapılandırması) kullanışlıdır.

Sanal makine için önyükleme kaydedemediği bazı dağıtım paketlerini otomatik olarak, tek kullanıcı modunda veya Acil Durum modunda kaldıracağız. Bunlar, tek kullanıcılı veya Acil Durum modunda otomatik olarak bırakabilirsiniz önce diğer ancak ek kurulum gerektirir.

GRUB sanal Makinenize erişim tek kullanıcı moduna aktarabilmek için etkinleştirildiğinden emin olmak isteyeceksiniz. Distro bağlı olarak GRUB etkinleştirildiğinden emin olmak için bazı Kurulumu iş olabilir. 


## <a name="access-for-red-hat-enterprise-linux-rhel"></a>Red Hat Enterprise Linux (RHEL) için erişim
Otomatik olarak, normal şekilde önyükleme işlemi yapamıyorsanız RHEL tek kullanıcı moduna kaldıracağız. Ancak, tek kullanıcı modu için kök erişimi ayarlamadıysanız, bir kök parola olmaz ve oturum açmak mümkün olmayacaktır. Geçici çözüm (' El ile tek kullanıcı moduna aşağıda girmesini' bakın), ancak başlangıçta kök erişimi ayarlamak için öneridir.

### <a name="grub-access-in-rhel"></a>RHEL GRUB erişim
RHEL, kullanıma hazır etkin GRUB ile birlikte gelir. GRUB girmek için VM yeniden başlatma `sudo reboot` ve herhangi bir tuşa basın. GRUB ekranın görünmesini görürsünüz.

> Not: Red Hat, aynı zamanda önyükleme kurtarma modu, Acil durum modu, hata ayıklama modu ile ve kök parolayı sıfırlama için belgeler sağlar. [Erişmek için buraya tıklayın](https://aka.ms/rhel7grubterminal).

### <a name="set-up-root-access-for-single-user-mode-in-rhel"></a>RHEL tek kullanıcı modunda için kök erişimi ayarlama
RHEL tek kullanıcı modunda varsayılan olarak devre dışı etkinleştirilmesi kök kullanıcı gerektirir. Tek kullanıcı modunda etkinleştirmek için bir gereksinimi varsa, aşağıdaki yönergeleri kullanın:

1. Red Hat sisteme SSH aracılığıyla oturum açın
1. Kök dizinine geçin
1. Kök kullanıcı parolası etkinleştir 
    * `passwd root` (güçlü bir kök parola ayarlayın)
1. Kök kullanıcı yalnızca ttyS0 oturum açabildiğinizden emin olun.
    * `edit /etc/ssh/sshd_config` ve PermitRootLogIn Hayır ayarlandığından emin olun
    * `edit /etc/securetty file` yalnızca oturum açma ttyS0 aracılığıyla izin vermek için 

Sistem tek kullanıcı modunda önyükleniyorsa artık, kök parolası ile oturum açabilir.

GRUB RHEL 7.4 + veya 6.9 + etkinleştirebilirsiniz tek kullanıcı modunda istemleri için yönergeler bunun yerine bkz [burada](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/installation_guide/s1-rescuemode-booting-single)

### <a name="manually-enter-single-user-mode-in-rhel"></a>Tek kullanıcı modunda RHEL içinde el ile girin
Ayarlamış olduğunuz, yukarıdaki yönergeleri ile erişim GRUB ve kök ve ardından, aşağıdaki yönergelere tek kullanıcı moduna girebilirsiniz:

1. GRUB girmek için VM yeniden başlatma sırasında 'Esc' tuşuna basın
1. GRUB seçilen işletim sistemi, (genellikle ilk satırına) önyükleme yapmak istediğiniz düzenlemek için ' e basın
1. Çekirdek satırı - Azure'da bulun, ile başlar `linux16`
1. Satır sonuna git için Ctrl + E basın
1. Şu satırın sonuna ekleyin: `systemd.unit=rescue.target`
    * Bu tek kullanıcı moduna önyükleme yapmaz. Acil durum modu kullanmak istiyorsanız, ekleme `systemd.unit=emergency.target` yerine satır sonuna `systemd.unit=rescue.target`
1. Uygulanan ayarları ile yeniden başlatma işleminden çıkmak için Ctrl + X tuşlarına basın.
1. Yönetici parolasını sizden istenir tek kullanıcı moduna girmek için - bu, yukarıdaki yönergeleri oluşturduğunuz parolayı    

    ![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-rhel-enter-emergency-shell.gif)

### <a name="enter-single-user-mode-without-root-account-enabled-in-rhel"></a>Kök hesabı RHEL etkin olmayan tek kullanıcı moduna gir
Kök kullanıcı etkinleştirmek için yukarıdaki adımları gitti değil ise, hala kök parolanızı sıfırlayabilirsiniz. Aşağıdaki yönergeleri kullanın:

> Not: SELinux kullanıyorsanız Red Hat belgelerinde açıklanan ek adımları sunucucu olun [burada](https://aka.ms/rhel7grubterminal) kök parolayı sıfırlarken.

1. GRUB girmek için VM yeniden başlatma sırasında 'Esc' tuşuna basın
1. GRUB seçilen işletim sistemi, (genellikle ilk satırına) önyükleme yapmak istediğiniz düzenlemek için ' e basın
1. Çekirdek satırı - Azure'da bulun, ile başlar `linux16`
1. Ekleme `rd.break` satırın sonuna kadar önce bir boşluk olup var. sağlama `rd.break` (aşağıdaki örneğe bakın)
    - Gelen denetim geçirilmeden önce bu önyükleme işlemi kesintiye uğrar `initramfs` için `systemd`, Red Hat belgelerinde açıklanan şekilde [burada](https://aka.ms/rhel7rootpassword).
1. Uygulanan ayarları ile yeniden başlatma işleminden çıkmak için Ctrl + X tuşlarına basın.
1. Önyüklediğinizde, salt okunur dosya sistemi ile Acil moduna bırakılır. Girin `mount -o remount,rw /sysroot` okuma/yazma izinlerine sahip kök dosya sistemini yeniden bağlamaya yönelik Kabuk içine
1. Tek kullanıcı moduna önyüklemesini sonra yazın `chroot /sysroot` uygulamasına geçmeniz `sysroot` jailbreak
1. Kök sunulmuştur. Kök parolanızı sıfırlayabilir `passwd` ve tek kullanıcı moduna girmek için yukarıdaki yönergeleri kullanın. Tür `reboot -f` tamamladıktan sonra yeniden başlatmak için.

![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-rhel-emergency-mount-no-root.gif)

> Not: düzenleme gibi görevleri de yapabilmesi için yukarıdaki yönergeleri ile çalışan, Acil Durum kabuğundan bıraktı.%n%ndizinleri `fstab`. Ancak, tek kullanıcı moduna girmek için kullanın ve kök parolanızı sıfırlamak için genel olarak kabul edilen öneri olur. 


## <a name="access-for-centos"></a>CentOS erişimi
Çok gibi Red Hat Enterprise Linux, CentOS tek kullanıcı modunda GRUB ve kök kullanıcı etkinleştirilmesini gerektirir. 

### <a name="grub-access-in-centos"></a>CentOS GRUB erişim
CentOS yepyeni etkin GRUB ile gelir. GRUB girmek için VM yeniden başlatma `sudo reboot` ve herhangi bir tuşa basın. GRUB ekranın görünmesini görürsünüz.

### <a name="single-user-mode-in-centos"></a>CentOS tek kullanıcı modunda
CentOS tek kullanıcı modunda etkinleştirmek için yukarıdaki RHEL yönergelerini izleyin.

## <a name="access-for-ubuntu"></a>Ubuntu için erişim 
Ubuntu görüntüleri bir kök parola gerektirmez. Sistem tek kullanıcı modunda önyükleniyorsa ek kimlik bilgileri olmadan kullanabilirsiniz. 

### <a name="grub-access-in-ubuntu"></a>Ubuntu GRUB erişim
GRUB erişmek için basılı VM yedekleme önyüklenirken 'Esc'.

### <a name="single-user-mode-in-ubuntu"></a>Ubuntu tek kullanıcı modunda
Otomatik olarak, normal şekilde önyükleme işlemi yapamıyorsanız Ubuntu tek kullanıcı moduna kaldıracağız. El ile tek kullanıcı moduna girmek için aşağıdaki yönergeleri kullanın:

1. GRUB ' (Ubuntu girişi), önyükleme girişi düzenlemek için ' e basın
1. İle başlayan satırı bulun `linux`, ardından Ara `ro`
1. Ekleme `single` sonra `ro`, kalmasını sağlama önce ve sonra bir boşluk olup `single`
1. Bu ayarlarla yeniden başlatın ve tek kullanıcı moduna girmek için Ctrl + X tuşlarına basın.

## <a name="access-for-coreos"></a>CoreOS erişimi
CoreOS tek kullanıcı modunda GRUB etkinleştirilmesini gerektirir. 

### <a name="grub-access-in-coreos"></a>CoreOS GRUB erişim
GRUB erişmek için yedekleme, sanal Makinenizin önyükleme yaparken herhangi bir tuşa basın.

### <a name="single-user-mode-in-coreos"></a>CoreOS tek kullanıcı modunda
Otomatik olarak, normal şekilde önyükleme işlemi yapamıyorsanız CoreOS tek kullanıcı moduna kaldıracağız. El ile tek kullanıcı moduna girmek için aşağıdaki yönergeleri kullanın:
1. GRUB ', önyükleme girişi düzenlemek için ' e basın
1. İle başlayan satırı bulun `linux$`. 2 olmalıdır farklı if/else tümceleri kapsüllenmiş
1. Append `coreos.autologin=ttyS0` hem sonuna `linux$` satırları
1. Bu ayarlarla yeniden başlatın ve tek kullanıcı moduna girmek için Ctrl + X tuşlarına basın.

## <a name="access-for-suse-sles"></a>SUSE SLES için erişim
Yeni görüntüler, SLES 12 SP3 + seri Konsolu aracılığıyla erişim sağlar, bu durumda sistem Acil Modu'nda önyüklenir. 

### <a name="grub-access-in-suse-sles"></a>GRUB erişim SUSE SLES
SLES GRUB erişim YaST aracılığıyla şifresizdir yapılandırma gerektirir. Bunu yapmak için bu yönergeleri izleyin:

1. SSH, SLES VM ve çalışma `sudo yast bootloader`. Kullanım `tab` anahtar `enter` anahtar ve menüsünde gezinmek için ok tuşlarını. 
1. Gidin `Kernel Parameters`ve `Use serial console`. 
1. Ekleme `serial --unit=0 --speed=9600 --parity=no` konsol bağımsız

1. Ayarlarınızı kaydedip çıkmak için F10 tuşlarına basın
1. GRUB girmek için VM'yi yeniden başlatın ve GRUB ekranda kalın hale getirmek için önyükleme sırası sırasında herhangi bir tuşa basın.
    - GRUB varsayılan zaman aşımını 1s ' dir. Bunu değiştirerek değiştirebilirsiniz `GRUB_TIMEOUT` değişkeninde `/etc/default/grub`

![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-sles-yast-grub-config.gif)

### <a name="single-user-mode-in-suse-sles"></a>Tek kullanıcı modunda SUSE SLES
SLES normal şekilde önyükleme işlemi yapamıyorsanız Acil shell'e otomatik olarak bırakılır. Acil Durum Kabuk el ile girmek için aşağıdaki yönergeleri kullanın:

1. GRUB ' (SLES girişi), önyükleme girişi düzenlemek için ' e basın
1. İle başlar çekirdek satırı arayın `linux`
1. Append `systemd.unit=emergency.target` satırın sonuna
1. Bu ayarlarla yeniden başlatın ve Acil Durum Kabuk girmek için Ctrl + X tuşlarına basın.
> Unutmayın, Acil Durum kabuğunda oturum bırakılacak bir _salt okunur_ dosya sistemi. Tüm dosyalara düzenlemeler yapmak istiyorsanız, okuma-yazma izinlerine sahip bir dosya sistemi yeniden bağlamak gerekir. Bunu yapmak için girin `mount -o remount,rw /` Kabuk içine

## <a name="access-for-oracle-linux"></a>Oracle Linux için erişim
Çok gibi Red Hat Enterprise Linux, Oracle Linux tek kullanıcı modunda GRUB ve kök kullanıcı etkinleştirilmesini gerektirir. 

### <a name="grub-access-in-oracle-linux"></a>Oracle Linux'ta GRUB erişim
Oracle Linux, kullanıma hazır etkin GRUB ile birlikte gelir. GRUB girmek için VM ile yeniden `sudo reboot` 'Esc' tuşuna basın. GRUB ekranın görünmesini görürsünüz.

### <a name="single-user-mode-in-oracle-linux"></a>Oracle Linux tek kullanıcı modunda
Oracle Linux tek kullanıcı modunda etkinleştirmek için yukarıdaki RHEL yönergelerini izleyin.

## <a name="next-steps"></a>Sonraki adımlar
* Ana seri konsol Linux belgeleri sayfasının bulunduğu [burada](serial-console.md).
* Seri Konsolu [NMI ve SysRq çağrıları](serial-console-nmi-sysrq.md)
* Seri konsol için de kullanılabilir olan [Windows](../windows/serial-console.md) VM'ler
* Daha fazla bilgi edinin [önyükleme tanılaması](boot-diagnostics.md)