---
title: Azure'da CentOS tabanlı Linux VHD'si oluşturma ve yükleme
description: Oluşturma ve bir Azure sanal bir CentOS tabanlı Linux işletim sistemi içeren sabit disk (VHD) yükleme öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: szarkos
manager: gwallace
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 0e518e92-e981-43f4-b12c-9cba1064c4bb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/04/2018
ms.author: szark
ms.openlocfilehash: 8c7c3a31b36705e90cec9775806e8d1c8bf5cebe
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67668032"
---
# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a>Azure’da CentOS tabanlı bir sanal makine hazırlama

Oluşturma ve bir Azure sanal bir CentOS tabanlı Linux işletim sistemi içeren sabit disk (VHD) yükleme öğrenin.

* [Azure'da CentOS 6.x sanal makine hazırlama](#centos-6x)
* [Azure'da CentOS 7.0 + sanal makine hazırlama](#centos-70)


## <a name="prerequisites"></a>Önkoşullar

Bu makalede, bir CentOS zaten yüklü olduğunu varsayar (veya benzer bir türev) bir sanal sabit disk için Linux işletim sistemi. Birden çok araç, .vhd dosyaları, örneğin bir Hyper-V gibi sanallaştırma çözümü oluşturmak için mevcut. Yönergeler için [Hyper-V rolünü yükleme ve sanal makine yapılandırma](https://technet.microsoft.com/library/hh846766.aspx).

**CentOS yükleme notları**

* Ayrıca bkz [genel Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes) Linux için Azure hazırlama hakkında daha fazla ipucu için.
* VHDX biçimi, Azure'da yalnızca desteklenmiyor **VHD'yi sabit**.  Disk Hyper-V Yöneticisi'ni veya convert-vhd cmdlet'ini kullanarak VHD biçimine dönüştürebilirsiniz. VirtualBox kullanıyorsanız seçerek yani **boyutu sabit** diski oluştururken dinamik olarak ayrılan varsayılan aksine.
* Linux sistemini yüklerken olduğu *önerilen* LVM (genellikle birçok yüklemeleri için varsayılan) yerine standart bölümleri kullanın. Özellikle bir işletim sistemi diski hiç sorun giderme için başka bir özdeş sanal makineye eklenmesi gerekiyorsa bu kopyalanan sanal makineler ile ad çakışmalarını LVM uğraşmasına gerek kalmaz. [LVM'yi](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veri diskleri üzerinde kullanılabilir.
* UDF dosya sistemleri bağlamak için çekirdek desteği gereklidir. Azure'da ilk önyüklemede sağlama yapılandırması Linux VM Konuk bağlı UDF biçimli ortam aracılığıyla geçirilir. Azure Linux Aracısı yapılandırmasını okuma ve VM sağlama UDF dosya sistemi monte etmesini mümkün olması gerekir.
* Linux çekirdek sürümleri 2.6.37 NUMA büyük VM boyutları ile Hyper-V üzerinde desteklemez. Yukarı Akış kullanan eski dağıtımlar Bu sorun öncelikle etkileri Red Hat 2.6.32 çekirdek ve RHEL 6.6 (çekirdek 2.6.32 504) düzeltildi. 2\.6.32-504 önyükleme parametresi ayarlanmalıdır daha özel çekirdekler 2.6.37 eski veya RHEL tabanlı çekirdekler eski çalıştıran sistemlere `numa=off` çekirdeğini grub.conf içinde komut satırı. Daha fazla bilgi için bkz: Red Hat [KB 436883](https://access.redhat.com/solutions/436883).
* İşletim sistemi diski üzerinde takas bölümü yapılandırmayın. Linux Aracısı, geçici kaynak diski üzerinde takas dosyası oluşturmak için yapılandırılabilir.  Aşağıdaki adımlarda bunu hakkında daha fazla bilgi bulunabilir.
* Tüm VHD'leri azure'da bir sanal Boyut 1 MB ile uyumlu olması gerekir. Ham bir diskten VHD'ye dönüştürme yaparken ham disk boyutu 1 MB dönüştürmeden önce bir çok olduğundan emin olmalısınız. Bkz: [Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes) daha fazla bilgi için.

## <a name="centos-6x"></a>CentOS 6.x

1. Hyper-V Yöneticisi'nde, sanal makineyi seçin.

2. Tıklayın **Connect** sanal makine için bir konsol penceresi açın.

3. CentOS 6'da, Azure Linux Aracısı ile NetworkManager engelleyebilir. Bu paket, aşağıdaki komutu çalıştırarak kaldırın:

    ```bash
    sudo rpm -e --nodeps NetworkManager
    ```

4. Oluşturma veya düzenleme dosya `/etc/sysconfig/network` ve aşağıdaki metni ekleyin:

    ```console
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

5. Oluşturma veya düzenleme dosya `/etc/sysconfig/network-scripts/ifcfg-eth0` ve aşağıdaki metni ekleyin:

    ```console
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    ```

6. Ethernet arabirimleri için statik kuralları oluşturmaktan kaçınmak için udev kurallarını değiştirin. Bu kurallar, bir sanal makinede Microsoft Azure veya Hyper-V: kopyalarken sorunlara neden olabilir

    ```bash
    sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
    sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
    ```

7. Ağ hizmeti, aşağıdaki komutu çalıştırarak önyükleme sırasında başlar emin olun:

    ```bash
    sudo chkconfig network on
    ```

8. Azure veri merkezleri içinde barındırılır ve ardından değiştirin OpenLogic yansıtmalar kullanmak istiyorsanız, `/etc/yum.repos.d/CentOS-Base.repo` aşağıdaki depolardan dosyasıyla.  Bu ayrıca ekler **[openlogic]** Azure Linux aracısı gibi ek paketlerini içeren depo:

    ```console
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
    ```

    > [!Note]
    > Bu kılavuzda kalan kullanmakta olduğunuz varsayılır en az `[openlogic]` aşağıdaki Azure Linux Aracısı'nı yüklemek için kullanılacak bir depo.

9. /Etc/yum.conf için aşağıdaki satırı ekleyin:

    ```console
    http_caching=packages
    ```

10. Geçerli yum meta verileri temizlemek ve en son paketlerle sistemi güncelleştirmek için aşağıdaki komutu çalıştırın:

    ```bash
    yum clean all
    ```

    CentOS daha eski bir sürümü için bir görüntü oluşturmakta olduğunuz sürece, tüm paketlerin en son sürüme güncelleştirmek için önerilir:

    ```bash
    sudo yum -y update
    ```

    Bu komutu çalıştırdıktan sonra bir yeniden başlatma gerekli olabilir.

11. (İsteğe bağlı) Linux Integration Services (LIS) sürücülerini yükleyin.

    > [!IMPORTANT]
    > Adım **gerekli** için CentOS 6,3 ve önceki ve sonraki sürümleri için isteğe bağlı.

    ```bash
    sudo rpm -e hypervkvpd  ## (may return error if not installed, that's OK)
    sudo yum install microsoft-hyper-v
    ```

    Alternatif olarak, el ile yükleme yönergeleri izleyebilirsiniz [LIS indirme sayfasına](https://go.microsoft.com/fwlink/?linkid=403033) RPM VM'nizi üzerine yüklemek için.

12. Azure Linux Aracısı'nı ve bağımlılıklarını yükleyin:

    ```bash
    sudo yum install python-pyasn1 WALinuxAgent
    ```

    Bunlar zaten açıklandığı kaldırılmamışsa NetworkManager gnome paketleri 3. adımında ve WALinuxAgent paket NetworkManager kaldırılır.

13. Kernel önyükleme satırına grub yapılandırmanızda ek çekirdek parametreleri için Azure içerecek şekilde değiştirin. Bunu yapmak için açık `/boot/grub/menu.lst` bir metin düzenleyicisinde ve varsayılan çekirdek aşağıdaki parametreleri içerir:

    ```console
    console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    ```

    Bu, Azure size yardımcı olabilir ilk seri bağlantı noktası için tüm konsol iletileri gönderilir sağlayacak sorunları hata ayıklama desteği.

    Yukarıdakine ek olarak, ancak önerilir *Kaldır* aşağıdaki parametreleri:

    ```console
    rhgb quiet crashkernel=auto
    ```

    Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilmesi için tüm günlükleri istediğimiz bir bulut ortamında kullanışlı değildir.  `crashkernel` Seçenek olabilir sol isterseniz yapılandırılmış, ancak Not daha küçük VM boyutlarına sorunlu olabilecek bu parametre VM'de 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır.

    > [!Important]
    > CentOS 6.5 ve daha önceki çekirdek parametresi de ayarlamanız gerekir `numa=off`. Red Hat bkz [KB 436883](https://access.redhat.com/solutions/436883).

14. SSH sunucusu yüklü ve önyükleme sırasında başlatılacak şekilde yapılandırılmış emin olun.  Bu, genellikle varsayılan değerdir.

15. İşletim sistemi diski üzerinde takas alanı oluşturabilirsiniz.

    Azure Linux Aracısı, Azure üzerinde sağladıktan sonra VM'ye bağlı yerel kaynak disk kullanılan takas alanı otomatik olarak yapılandırabilirsiniz. Yerel kaynak olduğuna dikkat edin bir *geçici* disk ve sanal Makinenin sağlaması kaldırıldığında boşaltılabilir. Azure Linux Aracısı'nı yükledikten sonra (önceki adıma bakın), aşağıdaki parametrelerle değiştirin `/etc/waagent.conf` uygun şekilde:

    ```console
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.EnableSwap=y
    ResourceDisk.SwapSizeMB=2048 ## NOTE: set this to whatever you need it to be.
    ```

16. Sanal makinenin sağlamasını kaldırma ve Azure'da sağlama için hazırlamak için aşağıdaki komutları çalıştırın:

    ```bash
    sudo waagent -force -deprovision
    export HISTSIZE=0
    logout
    ```

17. Tıklayın **eylem -> kapatma aşağı** Hyper-V Yöneticisi'nde. Linux VHD'nizi artık Azure'a yüklenmek hazırdır.



## <a name="centos-70"></a>CentOS 7.0+

**CentOS 7 (ve benzer türevleri) değişiklikleri**

Azure için bir CentOS 7 sanal makinesini hazırlama çok benzeyen CentOS 6, ancak önemli bazı önemli farklılıklar vardır:

* NetworkManager paket artık Azure Linux Aracısı ile çakışıyor. Bu paket, varsayılan olarak yüklenir ve bu kaldırılmaz önerilir.
* Çekirdek parametreleri düzenleme yordamı (aşağıya bakın) değiştirildi. Bu nedenle GRUB2 artık varsayılan önyükleme yükleyicisi kullanılır.
* XFS artık varsayılan dosya sistemidir. İsterseniz ext4 dosya sistemi hala kullanılabilir.

**Yapılandırma adımları**

1. Hyper-V Yöneticisi'nde, sanal makineyi seçin.

2. Tıklayın **Connect** sanal makine için bir konsol penceresi açın.

3. Oluşturma veya düzenleme dosya `/etc/sysconfig/network` ve aşağıdaki metni ekleyin:

    ```console
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

4. Oluşturma veya düzenleme dosya `/etc/sysconfig/network-scripts/ifcfg-eth0` ve aşağıdaki metni ekleyin:

    ```console
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    NM_CONTROLLED=no
    ```

5. Ethernet arabirimleri için statik kuralları oluşturmaktan kaçınmak için udev kurallarını değiştirin. Bu kurallar, bir sanal makinede Microsoft Azure veya Hyper-V: kopyalarken sorunlara neden olabilir

    ```bash
    sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
    ```

6. Azure veri merkezleri içinde barındırılır ve ardından değiştirin OpenLogic yansıtmalar kullanmak istiyorsanız, `/etc/yum.repos.d/CentOS-Base.repo` aşağıdaki depolardan dosyasıyla.  Bu ayrıca ekler **[openlogic]** Azure Linux Aracısı paketlerini içeren depo:

    ```console
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
    ```
    
    > [!Note]
    > Bu kılavuzda kalan kullanmakta olduğunuz varsayılır en az `[openlogic]` aşağıdaki Azure Linux Aracısı'nı yüklemek için kullanılacak bir depo.

7. Geçerli yum meta verileri temizlemek ve tüm güncelleştirmeleri yüklemek için aşağıdaki komutu çalıştırın:

    ```bash
    sudo yum clean all
    ```

    CentOS daha eski bir sürümü için bir görüntü oluşturmakta olduğunuz sürece, tüm paketlerin en son sürüme güncelleştirmek için önerilir:

    ```bash
    sudo yum -y update
    ```

    Bir yeniden başlatma belki de bu komutu çalıştırdıktan sonra gerekiyor.

8. Kernel önyükleme satırına grub yapılandırmanızda ek çekirdek parametreleri için Azure içerecek şekilde değiştirin. Bunu yapmak için açık `/etc/default/grub` bir metin düzenleyicisi ve düzenleme `GRUB_CMDLINE_LINUX` parametresi, örneğin:

    ```console
    GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
    ```

   Bu, Azure size yardımcı olabilir ilk seri bağlantı noktası için tüm konsol iletileri gönderilir sağlayacak sorunları hata ayıklama desteği. NIC için de yeni CentOS 7 adlandırma kurallarını devre dışı bırakır. Yukarıdakine ek olarak, ancak önerilir *Kaldır* aşağıdaki parametreleri:

    ```console
    rhgb quiet crashkernel=auto
    ```

    Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilmesi için tüm günlükleri istediğimiz bir bulut ortamında kullanışlı değildir. `crashkernel` Seçenek olabilir sol isterseniz yapılandırılmış, ancak Not daha küçük VM boyutlarına sorunlu olabilecek bu parametre VM'de 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır.

9. İşiniz bittiğinde düzenleme `/etc/default/grub` başına, yukarıda grub yapılandırması yeniden oluşturmak için aşağıdaki komutu çalıştırın:

    ```bash
    sudo grub2-mkconfig -o /boot/grub2/grub.cfg
    ```

10. Bir görüntüden oluşturuyorsanız **VMware, VirtualBox veya KVM:** Hyper-V sürücüleri içinde initramfs dahil olduklarından emin olun:

    Düzen `/etc/dracut.conf`, içeriği ekleyin:

    ```console
    add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”
    ```

    İnitramfs yeniden oluşturun:

    ```bash
    sudo dracut -f -v
    ```

11. Azure Linux Aracısı'nı ve bağımlılıklarını yükleyin:

    ```bash
    sudo yum install python-pyasn1 WALinuxAgent
    sudo systemctl enable waagent
    ```

12. İşletim sistemi diski üzerinde takas alanı oluşturabilirsiniz.

    Azure Linux Aracısı, Azure üzerinde sağladıktan sonra VM'ye bağlı yerel kaynak disk kullanılan takas alanı otomatik olarak yapılandırabilirsiniz. Yerel kaynak olduğuna dikkat edin bir *geçici* disk ve sanal Makinenin sağlaması kaldırıldığında boşaltılabilir. Azure Linux Aracısı'nı yükledikten sonra (önceki adıma bakın), aşağıdaki parametrelerle değiştirin `/etc/waagent.conf` uygun şekilde:

    ```console
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.EnableSwap=y
    ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.
    ```

13. Sanal makinenin sağlamasını kaldırma ve Azure'da sağlama için hazırlamak için aşağıdaki komutları çalıştırın:

    ```bash
    sudo waagent -force -deprovision
    export HISTSIZE=0
    logout
    ```

14. Tıklayın **eylem -> kapatma aşağı** Hyper-V Yöneticisi'nde. Linux VHD'nizi artık Azure'a yüklenmek hazırdır.

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure'da yeni sanal makineler oluşturmak için CentOS Linux sanal sabit diski kullanmaya hazırsınız. Bu Azure'a .vhd dosyasını karşıya ilk kez kullanıyorsanız, bkz. [bir özel diskten Linux VM oluşturma](upload-vhd.md#option-1-upload-a-vhd).
