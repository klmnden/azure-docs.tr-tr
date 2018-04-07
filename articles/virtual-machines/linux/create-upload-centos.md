---
title: Oluşturma ve Azure CentOS tabanlı Linux VHD'yi yükleme
description: Oluşturma ve bir Azure sanal sabit CentOS tabanlı Linux işletim sistemi içeren disk (VHD) yükleme hakkında bilgi edinme.
services: virtual-machines-linux
documentationcenter: ''
author: szarkos
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 0e518e92-e981-43f4-b12c-9cba1064c4bb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: szark
ms.openlocfilehash: e2af462d6fe0a6a9811e885199d70a182bf145c7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a>Azure’da CentOS tabanlı bir sanal makine hazırlama
* [Azure için CentOS 6.x sanal makineyi hazırlama](#centos-6x)
* [Azure için CentOS 7.0 + sanal makineyi hazırlama](#centos-70)

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, bir CentOS zaten yüklemiş olduğunuz varsayılmaktadır (veya benzer türevi) Linux işletim sistemi sanal sabit diske. Birden çok araç, .vhd dosyaları, örneğin bir Hyper-V gibi sanallaştırma çözümü oluşturmak için mevcut. Yönergeler için bkz: [Hyper-V rolünü yükleyin ve sanal makine yapılandırma](http://technet.microsoft.com/library/hh846766.aspx).

**CentOS yükleme notları**

* Ayrıca bkz [genel Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes) Linux Azure için hazırlama hakkında daha fazla ipucu için.
* VHDX biçimi, Azure'da yalnızca desteklenmiyor **VHD sabit**.  Disk Hyper-V Yöneticisi'ni veya convert-vhd cmdlet'ini kullanarak VHD biçimine dönüştürebilirsiniz. VirtualBox kullanıyorsanız bu seçerek gelir **boyutu sabit** disk oluşturulurken dinamik olarak ayrılan varsayılan aksine.
* Linux sistemini yüklerken olan *önerilen* LVM (genellikle birçok yüklemeleri için varsayılan) yerine standart bölümleri kullanın. Özellikle bir işletim sistemi diski şimdiye kadar aynı başka bir VM için sorun giderme için eklenmesi gerekiyorsa, bu kopyalanan VMs LVM ad çakışmalarını önler. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veri diskler üzerinde kullanılabilir.
* UDF dosya sistemleri bağlanması için çekirdek desteği gereklidir. Azure üzerinde ilk önyükleme sırasında sağlama yapılandırma Linux VM konağına bağlı UDF biçimli medya üzerinden geçirilir. Azure Linux Aracısı'nı yapılandırmasını okuma ve VM sağlamak için UDF dosya sistemi bağlama kurabilmesi gerekir.
* Linux çekirdek sürümleri 2.6.37 aşağıda NUMA ile büyük VM boyutları üzerinde Hyper-V desteklemez. Yukarı Akış kullanarak eski dağıtımları Bu sorun öncelikle etkileri Red Hat 2.6.32 çekirdek ve RHEL 6.6 (çekirdek 2.6.32 504) giderilmiştir. 2.6.32-504 önyükleme parametresinin ayarlamalısınız daha özel tekrar 2.6.37 eski veya tekrar eski RHEL tabanlı çalıştıran sistemlerde `numa=off` grub.conf içinde komut satırı çekirdeğini. Red Hat daha fazla bilgi için bkz: [KB 436883](https://access.redhat.com/solutions/436883).
* Bir takas bölüm işletim sistemi disk üzerinde yapılandırmayın. Linux Aracısı geçici kaynak disk üzerinde bir takas dosyası oluşturmak için yapılandırılabilir.  Aşağıdaki adımlarda bu hakkında daha fazla bilgi bulunabilir.
* Tüm VHD'leri Azure üzerinde bir sanal boyutu 1 MB ile hizalı olması gerekir. Ham diskten VHD'ye dönüştürülürken ham disk boyutu 1 MB dönüştürmeden önce birden fazla olduğundan emin olmanız gerekir. Bkz: [Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes) daha fazla bilgi için.

## <a name="centos-6x"></a>CentOS 6.x

1. Hyper-V Yöneticisi'nde sanal makineyi seçin.

2. Tıklatın **Bağlan** sanal makine için bir konsol penceresi açın.

3. CentOS 6'da, Azure Linux Aracısı ile NetworkManager etkileyebilir. Bu paket, aşağıdaki komutu çalıştırarak kaldırın:
   
        # sudo rpm -e --nodeps NetworkManager

4. Oluşturma veya dosyayı düzenleme `/etc/sysconfig/network` ve aşağıdaki metni ekleyin:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. Oluşturma veya dosyayı düzenleme `/etc/sysconfig/network-scripts/ifcfg-eth0` ve aşağıdaki metni ekleyin:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. Ethernet arabirimi için statik kuralları oluşturmamak için udev kuralları Değiştir. Bu kurallar, bir sanal makinede Microsoft Azure veya Hyper-V: kopyalarken sorunlara neden olabilir
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. Ağ hizmeti aşağıdaki komutu çalıştırarak önyükleme sırasında başlar emin olun:
   
        # sudo chkconfig network on

8. Azure veri merkezleri içinde barındırılır ve ardından değiştirmek OpenLogic yansıtmalar kullanmak istiyorsanız `/etc/yum.repos.d/CentOS-Base.repo` aşağıdaki depoları dosyasıyla.  Bu ayrıca ekler **[openlogic]** Azure Linux aracısı gibi ek paketler içeren depo:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0
        
        [base]
        name=CentOS-$releasever - Base
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
        
        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
        
        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
        
        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=contrib&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

    >[!Note]
    Bu kılavuzun ilerleyen kullanmakta olduğunuz varsayacak en az `[openlogic]` aşağıdaki Azure Linux aracısı yüklemek için kullanılan depo.


9. /Etc/yum.conf için aşağıdaki satırı ekleyin:
    
        http_caching=packages

10. Geçerli yum meta verileri temizlemek ve en son paketlerle sistemini güncelleştirmek için aşağıdaki komutu çalıştırın:
    
        # yum clean all

    CentOS daha eski bir sürümü için görüntü oluşturmakta olduğunuz sürece, tüm paketleri en son sürüme güncelleştirmek için önerilir:

        # sudo yum -y update

    Bu komutu çalıştırdıktan sonra bir yeniden başlatma gerekli olabilir.

11. (İsteğe bağlı) Sürücüleri Linux Tümleştirme hizmetleri (LIS) yükleyin.
   
    >[!IMPORTANT]
    Adım **gerekli** için CentOS 6,3 ve önceki ve sonraki sürümler için isteğe bağlıdır.

        # sudo rpm -e hypervkvpd  ## (may return error if not installed, that's OK)
        # sudo yum install microsoft-hyper-v

    Alternatif olarak, el ile yükleme yönergeleri izleyerek [LIS indirme sayfasına](https://go.microsoft.com/fwlink/?linkid=403033) RPM VM üzerine yüklemek için.
 
12. Azure Linux aracısı ve bağımlılıkları yükleyin:
    
        # sudo yum install python-pyasn1 WALinuxAgent
    
    Bunlar zaten açıklandığı gibi kaldırılmamışsa, NetworkManager gnome paketleri adım 3'te ve WALinuxAgent paket NetworkManager kaldırılır.


13. Azure için ek çekirdek parametreleri içerecek şekilde kaz yapılandırma çekirdek önyükleme satırı değiştirin. Bunu yapmak için açın `/boot/grub/menu.lst` bir metin düzenleyicisinde ve varsayılan çekirdek aşağıdaki parametreleri içerdiğinden emin olun:
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    Bu tüm konsol iletileri hangi Azure yardımcı olabilecek ilk seri bağlantı noktasına gönderilen sağlayacak sorunları ayıklama desteği.
    
    Yukarıdakilerin yanı sıra için önerilir *kaldırmak* aşağıdaki parametreleri:
    
        rhgb quiet crashkernel=auto
    
    Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilecek tüm günlükleri burada istiyoruz bulut ortamında bulunan kullanışlı değildir.  `crashkernel` Seçeneği olabilir sol isterseniz yapılandırılmış, ancak Not küçük VM boyutlarını sorunlu olabilecek bu parametre VM tarafından 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır.

    >[!Important]
    CentOS 6.5 ve daha önceki çekirdek parametresi de ayarlamanız gerekir `numa=off`. Red Hat bkz [KB 436883](https://access.redhat.com/solutions/436883).

14. SSH sunucusu yüklü ve önyükleme sırasında başlatılacak şekilde yapılandırılmış olduğundan emin olun.  Bu genellikle varsayılan seçenektir.

15. Takas alanı işletim sistemi disk üzerinde oluşturmayın.
    
    Azure Linux Aracısı'nı otomatik olarak takas alanı Azure üzerinde sağladıktan sonra VM'ye bağlı yerel kaynak diski kullanarak yapılandırabilirsiniz. Yerel kaynak disk Not bir *geçici* disk ve VM sağlaması kaldırılıyor. sağlaması zaman boşaltılabilir. Azure Linux Aracısı'nı yükledikten sonra (önceki adıma bakın), aşağıdaki parametreleri değiştirin `/etc/waagent.conf` uygun şekilde:
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Sanal makine yetkisini kaldırma ve Azure üzerinde sağlamak için hazırlamak için aşağıdaki komutları çalıştırın:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

17. Tıklatın **eylem -> kapatma aşağı** Hyper-V Yöneticisi'nde. Linux VHD Azure'a karşıya yüklenecek artık hazırdır.


- - -
## <a name="centos-70"></a>CentOS 7.0+
**CentOS 7 (ve benzer türevleri) değişiklikleri**

CentOS 7 sanal makine için Azure hazırlanıyor çok benzeyen CentOS 6, ancak eşitlenmeyeceği birkaç önemli farklılıklar vardır:

* NetworkManager paket artık Azure Linux Aracısı ile çakışıyor. Bu paketi varsayılan olarak yüklenir ve onu kaldırılmaz öneririz.
* Çekirdek parametreleri düzenleme yordamı (aşağıya bakın) değişti biçimde GRUB2 şimdi varsayılan önyükleme yükleyicisi kullanılır.
* XFS varsayılan dosya sistemi sunulmuştur. Ext4 dosya sistemi hala isterseniz kullanılabilir.

**Yapılandırma adımları**

1. Hyper-V Yöneticisi'nde sanal makineyi seçin.

2. Tıklatın **Bağlan** sanal makine için bir konsol penceresi açın.

3. Oluşturma veya dosyayı düzenleme `/etc/sysconfig/network` ve aşağıdaki metni ekleyin:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4. Oluşturma veya dosyayı düzenleme `/etc/sysconfig/network-scripts/ifcfg-eth0` ve aşağıdaki metni ekleyin:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

5. Ethernet arabirimi için statik kuralları oluşturmamak için udev kuralları Değiştir. Bu kurallar, bir sanal makinede Microsoft Azure veya Hyper-V: kopyalarken sorunlara neden olabilir
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Azure veri merkezleri içinde barındırılır ve ardından değiştirmek OpenLogic yansıtmalar kullanmak istiyorsanız `/etc/yum.repos.d/CentOS-Base.repo` aşağıdaki depoları dosyasıyla.  Bu ayrıca ekler **[openlogic]** paketler için Azure Linux Aracısı'nı içeren depo:
   
        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0
        
        [base]
        name=CentOS-$releasever - Base
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
        
        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
        
        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
        
        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

    >[!Note]
    Bu kılavuzun ilerleyen kullanmakta olduğunuz varsayacak en az `[openlogic]` aşağıdaki Azure Linux aracısı yüklemek için kullanılan depo.

7. Geçerli yum meta verileri temizlemek ve tüm güncelleştirmeleri yüklemek için aşağıdaki komutu çalıştırın:
   
        # sudo yum clean all

    CentOS daha eski bir sürümü için görüntü oluşturmakta olduğunuz sürece, tüm paketleri en son sürüme güncelleştirmek için önerilir:

        # sudo yum -y update

    Bir yeniden başlatma belki de bu komutu çalıştırdıktan sonra gerekiyor.

8. Azure için ek çekirdek parametreleri içerecek şekilde kaz yapılandırma çekirdek önyükleme satırı değiştirin. Bunu yapmak için açın `/etc/default/grub` bir metin Düzenleyicisi'ni ve düzenleme `GRUB_CMDLINE_LINUX` parametresi, örneğin:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Bu tüm konsol iletileri hangi Azure yardımcı olabilecek ilk seri bağlantı noktasına gönderilen sağlayacak sorunları ayıklama desteği. Aynı zamanda NIC için yeni CentOS 7 adlandırma kuralları devre dışı bırakır. Yukarıdakilerin yanı sıra için önerilir *kaldırmak* aşağıdaki parametreleri:
   
        rhgb quiet crashkernel=auto
   
    Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilecek tüm günlükleri burada istiyoruz bulut ortamında bulunan kullanışlı değildir. `crashkernel` Seçeneği olabilir sol isterseniz yapılandırılmış, ancak Not küçük VM boyutlarını sorunlu olabilecek bu parametre VM tarafından 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır.

9. İşiniz bittiğinde düzenleme `/etc/default/grub` başına, yukarıda kaz yapılandırma yeniden oluşturmak için aşağıdaki komutu çalıştırın:
   
        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

10. Görüntüden oluşturuluyorsa **VMWare, VirtualBox veya KVM:** emin olun Hyper-V sürücüleri initramfs eklenir:
   
   Düzen `/etc/dracut.conf`, içeriği ekleyin:
   
        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”
   
   İnitramfs yeniden oluşturun:
   
        # sudo dracut –f -v

11. Azure Linux aracısı ve bağımlılıkları yükleyin:

        # sudo yum install python-pyasn1 WALinuxAgent
        # sudo systemctl enable waagent

12. Takas alanı işletim sistemi disk üzerinde oluşturmayın.
   
   Azure Linux Aracısı'nı otomatik olarak takas alanı Azure üzerinde sağladıktan sonra VM'ye bağlı yerel kaynak diski kullanarak yapılandırabilirsiniz. Yerel kaynak disk Not bir *geçici* disk ve VM sağlaması kaldırılıyor. sağlaması zaman boşaltılabilir. Azure Linux Aracısı'nı yükledikten sonra (önceki adıma bakın), aşağıdaki parametreleri değiştirin `/etc/waagent.conf` uygun şekilde:
   
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Sanal makine yetkisini kaldırma ve Azure üzerinde sağlamak için hazırlamak için aşağıdaki komutları çalıştırın:
   
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. Tıklatın **eylem -> kapatma aşağı** Hyper-V Yöneticisi'nde. Linux VHD Azure'a karşıya yüklenecek artık hazırdır.

## <a name="next-steps"></a>Sonraki adımlar
Şimdi yeni sanal makineler oluşturmak için CentOS Linux sanal sabit diski kullanmak hazırsınız. Azure için .vhd dosyasını karşıya yüklüyoruz ilk kez kullanıyorsanız bkz [özel bir diskten bir Linux VM oluşturma](upload-vhd.md#option-1-upload-a-vhd).

