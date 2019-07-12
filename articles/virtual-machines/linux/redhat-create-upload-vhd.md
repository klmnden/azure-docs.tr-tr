---
title: Oluşturma ve karşıya yükleme azure'da kullanım için Red Hat Enterprise Linux VHD'si | Microsoft Docs
description: Oluşturma ve bir Azure sanal bir Red Hat Linux işletim sistemi içeren sabit disk (VHD) yükleme öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: szarkos
manager: gwallace
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 6c6b8f72-32d3-47fa-be94-6cb54537c69f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/17/2019
ms.author: szark
ms.openlocfilehash: f7f9baec5421a63e46536480cde8d60d3877d4d7
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67670966"
---
# <a name="prepare-a-red-hat-based-virtual-machine-for-azure"></a>Azure için Red Hat tabanlı bir sanal makine hazırlama
Bu makalede, azure'da kullanım için Red Hat Enterprise Linux (RHEL) sanal makineyi hazırlama öğreneceksiniz. Bu makalede ele RHEL 6.7 + ve 7.1 + sürümleridir. Bu makalede ele hiper hazırlama için Hyper-V, çekirdek tabanlı sanal makine (KVM) ve VMware ' dir. Red Hats bulut erişimi Program'a uygunluk gereksinimleri hakkında daha fazla bilgi için bkz. [Red Hats bulut Access Web sitesinin](https://www.redhat.com/en/technologies/cloud-computing/cloud-access) ve [Azure üzerinde çalışan RHEL](https://access.redhat.com/ecosystem/ccsp/microsoft-azure). Görüntüleri RHEL oluşturmayı otomatikleştirme yollarını görmek [Azure Görüntü Oluşturucu](https://docs.microsoft.com/azure/virtual-machines/linux/image-builder-overview).

## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a>Red Hat tabanlı bir sanal makine Hyper-V Yöneticisi'nden hazırlama

### <a name="prerequisites"></a>Önkoşullar
Bu bölümde, zaten bir ISO dosyası Red Hat Web sitesinden alınan ve RHEL görüntüsü sanal sabit diske (VHD) yüklü olduğunu varsayar. Bir işletim sistemi görüntüsünü yüklemek için Hyper-V Yöneticisi'ni kullanma hakkında daha fazla ayrıntı için bkz. [Hyper-V rolünü yükleme ve sanal makine yapılandırma](https://technet.microsoft.com/library/hh846766.aspx).

**RHEL yükleme notları**

* Azure, VHDX biçimini desteklemiyor. Azure'un destekledikleri yalnızca VHD düzeltildi. Disk VHD biçimine dönüştürmek için Hyper-V Yöneticisi'ni kullanabilirsiniz veya convert-vhd cmdlet'ini kullanabilirsiniz. VirtualBox kullanıyorsanız belirleyin **boyutu sabit** diski oluştururken seçeneği dinamik olarak ayrılan varsayılan aksine.
* Azure yalnızca 1. kuşak sanal makineleri destekler. 1\. nesil sanal makine VHD dosya biçimine VHDX ve bir sabit boyutlu disk için dinamik olarak genişletilen dönüştürebilirsiniz. Bir sanal makinenin oluşturulması değiştiremezsiniz. Daha fazla bilgi için [Hyper-V'de 1 veya 2. kuşak sanal makine oluşturmalısınız?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).
* VHD için izin verilen en büyük boyutu 1,023 GB'dir.
* Mantıksal birim Yöneticisi (LVM) desteklenir ve işletim sistemi diski veya Azure sanal makineler'de veri diskleri kullanılabilir. Ancak, genel olarak, işletim sistemi diski LVM yerine standart bir bölüm kullanmak için önerilir. Özellikle, hiç olmadığı kadar başka bir özdeş sanal sorun giderme için makine bir işletim sistemi diski gerekiyorsa bu yöntem LVM adı kopyalanan sanal makineler ile çakışıyor uğraşmasına gerek kalmaz. Ayrıca bkz: [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ve [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) belgeleri.
* Evrensel Disk Biçimi (UDF) dosya sistemleri bağlamak için çekirdek desteği gereklidir. Azure'da ilk önyüklemede Konuk bağlı UDF biçimli medya Linux sanal makinesi için sağlama yapılandırması geçirir. Azure Linux Aracısı yapılandırmasını okuma ve sanal makine sağlamak için UDF dosya sistemi monte etmesini mümkün olması gerekir.
* İşletim sistemi diski üzerinde takas bölümü yapılandırmayın. Linux Aracısı, geçici kaynak diski üzerinde takas dosyası oluşturmak için yapılandırılabilir.  Aşağıdaki adımlarda bunu hakkında daha fazla bilgi bulunabilir.
* Tüm VHD'leri azure'da bir sanal Boyut 1 MB ile uyumlu olması gerekir. Ham bir diskten VHD'ye dönüştürme yaparken ham disk boyutu 1 MB dönüştürmeden önce bir çok olduğundan emin olmalısınız. Aşağıdaki adımlarda daha fazla ayrıntı bulunabilir. Ayrıca bkz: [Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes) daha fazla bilgi için.

### <a name="prepare-a-rhel-6-virtual-machine-from-hyper-v-manager"></a>Hyper-V Yöneticisi'nden bir RHEL 6 sanal makinesi hazırlama

1. Hyper-V Yöneticisi'nde, sanal makineyi seçin.

1. Tıklayın **Connect** sanal makine için bir konsol penceresi açın.

1. RHEL 6'da, Azure Linux Aracısı ile NetworkManager engelleyebilir. Bu paket, aşağıdaki komutu çalıştırarak kaldırın:
   
        # sudo rpm -e --nodeps NetworkManager

1. Oluşturma veya düzenleme `/etc/sysconfig/network` dosyasını bulun ve aşağıdaki metni ekleyin:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

1. Oluşturma veya düzenleme `/etc/sysconfig/network-scripts/ifcfg-eth0` dosyasını bulun ve aşağıdaki metni ekleyin:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

1. Ethernet arabirimi statik kurallarını oluşturmaktan kaçınmak için udev kuralları taşıyın (veya kaldırma). Bir sanal makinede Microsoft Azure veya Hyper-V: kopyaladığınızda, bu kurallar sorunlara neden

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

1. Ağ hizmeti aşağıdaki komutu çalıştırarak önyükleme sırasında başlayacağını emin olun:

        # sudo chkconfig network on

1. Aşağıdaki komutu çalıştırarak RHEL depodan paketler yüklenmesini etkinleştirmek için Red Hat aboneliğinizi kaydedin:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

1. WALinuxAgent paket `WALinuxAgent-<version>`, Red Hat ek özellikler deposuna gönderildi. Ek özellikler depo, aşağıdaki komutu çalıştırarak etkinleştirin:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

1. Kernel önyükleme satırına grub yapılandırmanızda ek çekirdek parametreleri için Azure içerecek şekilde değiştirin. Bu değişikliği yapmak için açık `/boot/grub/menu.lst` bir metin düzenleyicisinde ve varsayılan çekirdek aşağıdaki parametreleri içerir:
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    Bu, Azure size yardımcı olabilir ilk seri bağlantı noktası için tüm konsol iletileri gönderileceğini sağlayacak sorunları hata ayıklama desteği.
    
    Ayrıca, aşağıdaki parametreleri kaldırmanız önerilir:
    
        rhgb quiet crashkernel=auto
    
    Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilmesi için tüm günlükleri istediğimiz bir bulut ortamında kullanışlı değildir.  Bırakabilirsiniz `crashkernel` isterseniz yapılandırılmış seçeneği. Bu parametre sanal makinede 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır unutmayın. Bu yapılandırma daha küçük sanal makine boyutlarına sorunlu olabilir.


1. Güvenli Kabuk (SSH) sunucusu yüklü ve, genellikle varsayılan önyükleme sırasında başlatılacak şekilde yapılandırılmış emin olun. /Etc/ssh/sshd_config aşağıdaki satırı içerecek şekilde değiştirin:

        ClientAliveInterval 180

1. Azure Linux Aracısı, aşağıdaki komutu çalıştırarak yükleyin:

        # sudo yum install WALinuxAgent

        # sudo chkconfig waagent on

    WALinuxAgent paket yükleme NetworkManager kaldırır ve zaten kaldırılmadı, NetworkManager gnome paketleri 3. adımda.

1. İşletim sistemi diski üzerinde takas alanı oluşturabilirsiniz.

    Azure Linux Aracısı, Azure üzerinde sanal makine sağlandıktan sonra sanal makineye bağlı yerel kaynak diski kullanarak takas alanı otomatik olarak yapılandırabilirsiniz. Yerel kaynak geçici bir diskle olduğunu ve sanal makinenin sağlaması varsa, boşaltılabilir olduğunu unutmayın. Önceki adımda Azure Linux Aracısı yükledikten sonra aşağıdaki parametrelerle /etc/waagent.conf uygun şekilde değiştirin:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

1. Abonelik (gerekiyorsa) aşağıdaki komutu çalıştırarak kaydı:

        # sudo subscription-manager unregister

1. Sanal makinenin sağlamasını kaldırma ve Azure'da sağlama için hazırlamak için aşağıdaki komutları çalıştırın:

        # Mote: if you are migrating a specific virtual machine and do not wish to create a generalized image,
        # skip the deprovision step
        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

1. Tıklayın **eylem** > **kapatma** Hyper-V Yöneticisi'nde. Linux VHD'nizi artık Azure'a yüklenmek hazırdır.


### <a name="prepare-a-rhel-7-virtual-machine-from-hyper-v-manager"></a>Hyper-V Yöneticisi'nden RHEL 7 sanal makinesini hazırlama

1. Hyper-V Yöneticisi'nde, sanal makineyi seçin.

1. Tıklayın **Connect** sanal makine için bir konsol penceresi açın.

1. Oluşturma veya düzenleme `/etc/sysconfig/network` dosyasını bulun ve aşağıdaki metni ekleyin:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

1. Oluşturma veya düzenleme `/etc/sysconfig/network-scripts/ifcfg-eth0` dosyasını bulun ve aşağıdaki metni ekleyin:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

1. Ağ hizmeti aşağıdaki komutu çalıştırarak önyükleme sırasında başlayacağını emin olun:

        # sudo systemctl enable network

1. Aşağıdaki komutu çalıştırarak RHEL depodan paketler yüklenmesini etkinleştirmek için Red Hat aboneliğinizi kaydedin:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

1. Kernel önyükleme satırına grub yapılandırmanızda ek çekirdek parametreleri için Azure içerecek şekilde değiştirin. Bu değişikliği yapmak için açık `/etc/default/grub` bir metin düzenleyicisi ve düzenleme `GRUB_CMDLINE_LINUX` parametresi. Örneğin:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Bu, Azure size yardımcı olabilir ilk seri bağlantı noktası için tüm konsol iletileri gönderileceğini sağlayacak sorunları hata ayıklama desteği. Bu yapılandırma, ayrıca NIC'ler için yeni RHEL 7 adlandırma kurallarını devre dışı bırakır. Ayrıca, aşağıdaki parametreleri kaldırmanızı öneririz:
   
        rhgb quiet crashkernel=auto
   
    Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilmesi için tüm günlükleri istediğimiz bir bulut ortamında kullanışlı değildir. Bırakabilirsiniz `crashkernel` isterseniz yapılandırılmış seçeneği. Bu parametre, daha küçük sanal makine boyutlarına sorunlu olabilir sanal makinede 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır unutmayın.

1. Bitirdikten sonra düzenleme `/etc/default/grub`, grub yapılandırması yeniden oluşturmak için aşağıdaki komutu çalıştırın:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

1. SSH sunucusu yüklü ve, genellikle varsayılan önyükleme sırasında başlatılacak şekilde yapılandırılmış emin olun. Değiştirme `/etc/ssh/sshd_config` aşağıdaki satırı dahil etmek için:

        ClientAliveInterval 180

1. WALinuxAgent paket `WALinuxAgent-<version>`, Red Hat ek özellikler deposuna gönderildi. Ek özellikler depo, aşağıdaki komutu çalıştırarak etkinleştirin:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

1. Azure Linux Aracısı, aşağıdaki komutu çalıştırarak yükleyin:

        # sudo yum install WALinuxAgent

        # sudo systemctl enable waagent.service

1. İşletim sistemi diski üzerinde takas alanı oluşturabilirsiniz.

    Azure Linux Aracısı, Azure üzerinde sanal makine sağlandıktan sonra sanal makineye bağlı yerel kaynak diski kullanarak takas alanı otomatik olarak yapılandırabilirsiniz. Yerel kaynak disk geçici bir diski olduğundan ve sanal makinenin sağlaması, boşaltılabilir unutmayın. Önceki adımda Azure Linux Aracısı yükledikten sonra aşağıdaki parametrelerle değiştirin `/etc/waagent.conf` uygun şekilde:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

1. Abonelik kaydını kaldırmak istiyorsanız, aşağıdaki komutu çalıştırın:

        # sudo subscription-manager unregister

1. Sanal makinenin sağlamasını kaldırma ve Azure'da sağlama için hazırlamak için aşağıdaki komutları çalıştırın:

        # Mote: if you are migrating a specific virtual machine and do not wish to create a generalized image,
        # skip the deprovision step
        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

1. Tıklayın **eylem** > **kapatma** Hyper-V Yöneticisi'nde. Linux VHD'nizi artık Azure'a yüklenmek hazırdır.


## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a>Red Hat tabanlı bir sanal makine KVM hazırlama
### <a name="prepare-a-rhel-6-virtual-machine-from-kvm"></a>KVM RHEL 6 sanal makineyi hazırlama

1. RHEL 6 KVM görüntüsü Red Hat Web sitesinden indirin.

1. Bir kök parola ayarlayın.

    Şifreli bir parola oluşturmak ve komutunun çıktısını kopyalayın:

        # openssl passwd -1 changeme

    Guestfish ile bir kök parola ayarlayın:
        
        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   Kök kullanıcı ikinci alanı Değiştir "!!" şifrelenmiş parolası.

1. Bir sanal makine içinde KVM qcow2 görüntüden oluşturun. Disk türünü ayarlamak **qcow2**, sanal ağ arabirimi cihaz modeli ayarlanmış ve **virtio**. Ardından, sanal makineyi başlatın ve kök olarak oturum açın.

1. Oluşturma veya düzenleme `/etc/sysconfig/network` dosyasını bulun ve aşağıdaki metni ekleyin:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

1. Oluşturma veya düzenleme `/etc/sysconfig/network-scripts/ifcfg-eth0` dosyasını bulun ve aşağıdaki metni ekleyin:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

1. Ethernet arabirimi statik kurallarını oluşturmaktan kaçınmak için udev kuralları taşıyın (veya kaldırma). Bir sanal makineyi Azure'a veya Hyper-V: kopyalayıp, bu kurallar sorunlara neden

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

1. Ağ hizmeti aşağıdaki komutu çalıştırarak önyükleme sırasında başlayacağını emin olun:

        # chkconfig network on

1. Aşağıdaki komutu çalıştırarak RHEL depodan paketler yüklenmesini etkinleştirmek için Red Hat aboneliğinizi kaydedin:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

1. Kernel önyükleme satırına grub yapılandırmanızda ek çekirdek parametreleri için Azure içerecek şekilde değiştirin. Bu yapılandırma için açmak `/boot/grub/menu.lst` bir metin düzenleyicisinde ve varsayılan çekirdek aşağıdaki parametreleri içerir:
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    Bu, Azure size yardımcı olabilir ilk seri bağlantı noktası için tüm konsol iletileri gönderileceğini sağlayacak sorunları hata ayıklama desteği.
    
    Ayrıca, aşağıdaki parametreleri kaldırmanızı öneririz:
    
        rhgb quiet crashkernel=auto
    
    Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilmesi için tüm günlükleri istediğimiz bir bulut ortamında kullanışlı değildir. Bırakabilirsiniz `crashkernel` isterseniz yapılandırılmış seçeneği. Bu parametre, daha küçük sanal makine boyutlarına sorunlu olabilir sanal makinede 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır unutmayın.


1. Hyper-V modüller için initramfs ekleyin:  

    Düzen `/etc/dracut.conf`ve aşağıdaki içeriği ekleyin:

        add_drivers+=" hv_vmbus hv_netvsc hv_storvsc "

    İnitramfs yeniden oluşturun:

        # dracut -f -v

1. Cloud-init kaldırın:

        # yum remove cloud-init

1. SSH sunucusu yüklü ve önyükleme sırasında başlatılacak şekilde yapılandırılmış olduğundan emin olun:

        # chkconfig sshd on

    /Etc/ssh/sshd_config aşağıdaki satırları içerecek şekilde değiştirin:

        PasswordAuthentication yes
        ClientAliveInterval 180

1. WALinuxAgent paket `WALinuxAgent-<version>`, Red Hat ek özellikler deposuna gönderildi. Ek özellikler depo, aşağıdaki komutu çalıştırarak etkinleştirin:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

1. Azure Linux Aracısı, aşağıdaki komutu çalıştırarak yükleyin:

        # yum install WALinuxAgent

        # chkconfig waagent on

1. Azure Linux Aracısı, Azure üzerinde sanal makine sağlandıktan sonra sanal makineye bağlı yerel kaynak diski kullanarak takas alanı otomatik olarak yapılandırabilirsiniz. Yerel kaynak disk geçici bir diski olduğundan ve sanal makinenin sağlaması, boşaltılabilir unutmayın. Önceki adımda Azure Linux Aracısı yükledikten sonra aşağıdaki parametrelerle değiştirin **/etc/waagent.conf** uygun şekilde:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

1. Abonelik (gerekiyorsa) aşağıdaki komutu çalıştırarak kaydı:

        # subscription-manager unregister

1. Sanal makinenin sağlamasını kaldırma ve Azure'da sağlama için hazırlamak için aşağıdaki komutları çalıştırın:

        # Mote: if you are migrating a specific virtual machine and do not wish to create a generalized image,
        # skip the deprovision step
        # waagent -force -deprovision

        # export HISTSIZE=0

        # logout

1. KVM sanal makineyi kapatın.

1. Qcow2 görüntünün VHD biçimine dönüştürün.

> [!NOTE]
> Qemu img sürümlerinde bilinen bir hata olduğunu > düzgün şekilde biçimlendirilmemiş bir VHD sonuçları 2.2.1 =. Sorun QEMU 2.6 içinde düzeltilmiştir. Qemu-img 2.2.0 ya da alt kullanın veya 2.6 veya sonraki bir sürüme güncelleştirmek için önerilir. Başvuru: https://bugs.launchpad.net/qemu/+bug/1490611.
>


    First convert the image to raw format:

        # qemu-img convert -f qcow2 -O raw rhel-6.9.qcow2 rhel-6.9.raw

    Make sure that the size of the raw image is aligned with 1 MB. Otherwise, round up the size to align with 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.9.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.9.raw $rounded_size

    Convert the raw disk to a fixed-sized VHD:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.9.raw rhel-6.9.vhd

    Or, with qemu version **2.6+** include the `force_size` option:

        # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-6.9.raw rhel-6.9.vhd

        
### <a name="prepare-a-rhel-7-virtual-machine-from-kvm"></a>KVM RHEL 7 sanal makinesini hazırlama

1. RHEL 7 KVM görüntüsü Red Hat Web sitesinden indirin. Bu yordam, RHEL 7 örnek olarak kullanır.

1. Bir kök parola ayarlayın.

    Şifreli bir parola oluşturmak ve komutunun çıktısını kopyalayın:

        # openssl passwd -1 changeme

    Guestfish ile bir kök parola ayarlayın:

        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   Kök kullanıcı ikinci alanı Değiştir "!!" şifrelenmiş parolası.

1. Bir sanal makine içinde KVM qcow2 görüntüden oluşturun. Disk türünü ayarlamak **qcow2**, sanal ağ arabirimi cihaz modeli ayarlanmış ve **virtio**. Ardından, sanal makineyi başlatın ve kök olarak oturum açın.

1. Oluşturma veya düzenleme `/etc/sysconfig/network` dosyasını bulun ve aşağıdaki metni ekleyin:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

1. Oluşturma veya düzenleme `/etc/sysconfig/network-scripts/ifcfg-eth0` dosyasını bulun ve aşağıdaki metni ekleyin:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

1. Ağ hizmeti aşağıdaki komutu çalıştırarak önyükleme sırasında başlayacağını emin olun:

        # sudo systemctl enable network

1. Aşağıdaki komutu çalıştırarak RHEL depodan paketler yüklenmesini etkinleştirmek için Red Hat aboneliğinizi kaydedin:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

1. Kernel önyükleme satırına grub yapılandırmanızda ek çekirdek parametreleri için Azure içerecek şekilde değiştirin. Bu yapılandırma için açmak `/etc/default/grub` bir metin düzenleyicisi ve düzenleme `GRUB_CMDLINE_LINUX` parametresi. Örneğin:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Bu komut, ayrıca, Azure size yardımcı olabilir ilk seri bağlantı noktası için tüm konsol iletileri gönderilmesini sağlar sorunları hata ayıklama desteği. Komutu, ayrıca NIC'ler için yeni RHEL 7 adlandırma kurallarını devre dışı bırakır. Ayrıca, aşağıdaki parametreleri kaldırmanızı öneririz:
   
        rhgb quiet crashkernel=auto
   
    Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilmesi için tüm günlükleri istediğimiz bir bulut ortamında kullanışlı değildir. Bırakabilirsiniz `crashkernel` isterseniz yapılandırılmış seçeneği. Bu parametre, daha küçük sanal makine boyutlarına sorunlu olabilir sanal makinede 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır unutmayın.

1. Bitirdikten sonra düzenleme `/etc/default/grub`, grub yapılandırması yeniden oluşturmak için aşağıdaki komutu çalıştırın:

        # grub2-mkconfig -o /boot/grub2/grub.cfg

1. Hyper-V modüllerini initramfs ekleyin.

    Düzen `/etc/dracut.conf` ve içeriği ekleyin:

        add_drivers+=" hv_vmbus hv_netvsc hv_storvsc "

    İnitramfs yeniden oluşturun:

        # dracut -f -v

1. Cloud-init kaldırın:

        # yum remove cloud-init

1. SSH sunucusu yüklü ve önyükleme sırasında başlatılacak şekilde yapılandırılmış olduğundan emin olun:

        # systemctl enable sshd

    /Etc/ssh/sshd_config aşağıdaki satırları içerecek şekilde değiştirin:

        PasswordAuthentication yes
        ClientAliveInterval 180

1. WALinuxAgent paket `WALinuxAgent-<version>`, Red Hat ek özellikler deposuna gönderildi. Ek özellikler depo, aşağıdaki komutu çalıştırarak etkinleştirin:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

1. Azure Linux Aracısı, aşağıdaki komutu çalıştırarak yükleyin:

        # yum install WALinuxAgent

    Waagent hizmeti etkinleştirin:

        # systemctl enable waagent.service

1. İşletim sistemi diski üzerinde takas alanı oluşturabilirsiniz.

    Azure Linux Aracısı, Azure üzerinde sanal makine sağlandıktan sonra sanal makineye bağlı yerel kaynak diski kullanarak takas alanı otomatik olarak yapılandırabilirsiniz. Yerel kaynak disk geçici bir diski olduğundan ve sanal makinenin sağlaması, boşaltılabilir unutmayın. Önceki adımda Azure Linux Aracısı yükledikten sonra aşağıdaki parametrelerle değiştirin `/etc/waagent.conf` uygun şekilde:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

1. Abonelik (gerekiyorsa) aşağıdaki komutu çalıştırarak kaydı:

        # subscription-manager unregister

1. Sanal makinenin sağlamasını kaldırma ve Azure'da sağlama için hazırlamak için aşağıdaki komutları çalıştırın:

        # Mote: if you are migrating a specific virtual machine and do not wish to create a generalized image,
        # skip the deprovision step
        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

1. KVM sanal makineyi kapatın.

1. Qcow2 görüntünün VHD biçimine dönüştürün.

> [!NOTE]
> Qemu img sürümlerinde bilinen bir hata olduğunu > düzgün şekilde biçimlendirilmemiş bir VHD sonuçları 2.2.1 =. Sorun QEMU 2.6 içinde düzeltilmiştir. Qemu-img 2.2.0 ya da alt kullanın veya 2.6 veya sonraki bir sürüme güncelleştirmek için önerilir. Başvuru: https://bugs.launchpad.net/qemu/+bug/1490611.
>


    First convert the image to raw format:

        # qemu-img convert -f qcow2 -O raw rhel-7.4.qcow2 rhel-7.4.raw

    Make sure that the size of the raw image is aligned with 1 MB. Otherwise, round up the size to align with 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-7.4.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-7.4.raw $rounded_size

    Convert the raw disk to a fixed-sized VHD:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.4.raw rhel-7.4.vhd

    Or, with qemu version **2.6+** include the `force_size` option:

        # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-7.4.raw rhel-7.4.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a>Vmware'den Red Hat tabanlı bir sanal makine hazırlama
### <a name="prerequisites"></a>Önkoşullar
Bu bölümde, VMware ortamınızda RHEL sanal makine zaten yüklediğinizi varsayar. VMware ortamınızda bir işletim sistemi yükleme hakkında daha fazla bilgi için bkz. [VMware konuk işletim sistemi Yükleme Kılavuzu](https://partnerweb.vmware.com/GOSIG/home.html).

* Linux işletim sistemi yüklediğinizde, genellikle çoğu yüklemeleri için varsayılan seçenek LVM, yerine standart bölümleri kullanmanızı öneririz. Özellikle bir işletim sistemi diski hiç sorun giderme için başka bir sanal makineye bağlı gerekiyorsa, bu kopyalanan sanal makinenin adı çakışıyor LVM uğraşmasına gerek kalmaz. LVM'yi veya RAID veri disklerinde, tercih etmeleri durumunda kullanılabilir.
* İşletim sistemi diski üzerinde takas bölümü yapılandırmayın. Geçici kaynak diski üzerinde takas dosyası oluşturmak için Linux Aracısı yapılandırabilirsiniz. İzleyen adımları bu hakkında daha fazla bilgi bulabilirsiniz.
* Sanal sabit diski oluşturduğunuzda **Store sanal disk, tek bir dosya olarak**.

### <a name="prepare-a-rhel-6-virtual-machine-from-vmware"></a>Vmware'den RHEL 6 sanal makineyi hazırlama
1. RHEL 6'da, Azure Linux Aracısı ile NetworkManager engelleyebilir. Bu paket, aşağıdaki komutu çalıştırarak kaldırın:
   
        # sudo rpm -e --nodeps NetworkManager

1. Adlı bir dosya oluşturun **ağ** şu metni içeren/etc/sysconfig/dizine:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

1. Oluşturma veya düzenleme `/etc/sysconfig/network-scripts/ifcfg-eth0` dosyasını bulun ve aşağıdaki metni ekleyin:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

1. Ethernet arabirimi statik kurallarını oluşturmaktan kaçınmak için udev kuralları taşıyın (veya kaldırma). Bir sanal makineyi Azure'a veya Hyper-V: kopyalayıp, bu kurallar sorunlara neden

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

1. Ağ hizmeti aşağıdaki komutu çalıştırarak önyükleme sırasında başlayacağını emin olun:

        # sudo chkconfig network on

1. Aşağıdaki komutu çalıştırarak RHEL depodan paketler yüklenmesini etkinleştirmek için Red Hat aboneliğinizi kaydedin:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

1. WALinuxAgent paket `WALinuxAgent-<version>`, Red Hat ek özellikler deposuna gönderildi. Ek özellikler depo, aşağıdaki komutu çalıştırarak etkinleştirin:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

1. Kernel önyükleme satırına grub yapılandırmanızda ek çekirdek parametreleri için Azure içerecek şekilde değiştirin. Bunu yapmak için açık `/etc/default/grub` bir metin düzenleyicisi ve düzenleme `GRUB_CMDLINE_LINUX` parametresi. Örneğin:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0"
   
   Bu, Azure size yardımcı olabilir ilk seri bağlantı noktası için tüm konsol iletileri gönderileceğini sağlayacak sorunları hata ayıklama desteği. Ayrıca, aşağıdaki parametreleri kaldırmanızı öneririz:
   
        rhgb quiet crashkernel=auto
   
    Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilmesi için tüm günlükleri istediğimiz bir bulut ortamında kullanışlı değildir. Bırakabilirsiniz `crashkernel` isterseniz yapılandırılmış seçeneği. Bu parametre, daha küçük sanal makine boyutlarına sorunlu olabilir sanal makinede 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır unutmayın.

1. Hyper-V modüller için initramfs ekleyin:

    Düzen `/etc/dracut.conf`ve aşağıdaki içeriği ekleyin:

        add_drivers+=" hv_vmbus hv_netvsc hv_storvsc "

    İnitramfs yeniden oluşturun:

        # dracut -f -v

1. SSH sunucusu yüklü ve, genellikle varsayılan önyükleme sırasında başlatılacak şekilde yapılandırılmış emin olun. Değiştirme `/etc/ssh/sshd_config` aşağıdaki satırı dahil etmek için:

    Satırını Clientaliveınterval 180

1. Azure Linux Aracısı, aşağıdaki komutu çalıştırarak yükleyin:

        # sudo yum install WALinuxAgent

        # sudo chkconfig waagent on

1. İşletim sistemi diski üzerinde takas alanı oluşturabilirsiniz.

    Azure Linux Aracısı, Azure üzerinde sanal makine sağlandıktan sonra sanal makineye bağlı yerel kaynak diski kullanarak takas alanı otomatik olarak yapılandırabilirsiniz. Yerel kaynak disk geçici bir diski olduğundan ve sanal makinenin sağlaması, boşaltılabilir unutmayın. Önceki adımda Azure Linux Aracısı yükledikten sonra aşağıdaki parametrelerle değiştirin `/etc/waagent.conf` uygun şekilde:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

1. Abonelik (gerekiyorsa) aşağıdaki komutu çalıştırarak kaydı:

        # sudo subscription-manager unregister

1. Sanal makinenin sağlamasını kaldırma ve Azure'da sağlama için hazırlamak için aşağıdaki komutları çalıştırın:

        # Mote: if you are migrating a specific virtual machine and do not wish to create a generalized image,
        # skip the deprovision step
        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

1. Sanal makineyi kapatın ve VMDK dosyasını bir .vhd dosyasına dönüştürmek.

> [!NOTE]
> Qemu img sürümlerinde bilinen bir hata olduğunu > düzgün şekilde biçimlendirilmemiş bir VHD sonuçları 2.2.1 =. Sorun QEMU 2.6 içinde düzeltilmiştir. Qemu-img 2.2.0 ya da alt kullanın veya 2.6 veya sonraki bir sürüme güncelleştirmek için önerilir. Başvuru: https://bugs.launchpad.net/qemu/+bug/1490611.
>


    First convert the image to raw format:

        # qemu-img convert -f vmdk -O raw rhel-6.9.vmdk rhel-6.9.raw

    Make sure that the size of the raw image is aligned with 1 MB. Otherwise, round up the size to align with 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.9.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.9.raw $rounded_size

    Convert the raw disk to a fixed-sized VHD:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.9.raw rhel-6.9.vhd

    Or, with qemu version **2.6+** include the `force_size` option:

        # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-6.9.raw rhel-6.9.vhd


### <a name="prepare-a-rhel-7-virtual-machine-from-vmware"></a>Vmware'den RHEL 7 sanal makinesini hazırlama
1. Oluşturma veya düzenleme `/etc/sysconfig/network` dosyasını bulun ve aşağıdaki metni ekleyin:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

1. Oluşturma veya düzenleme `/etc/sysconfig/network-scripts/ifcfg-eth0` dosyasını bulun ve aşağıdaki metni ekleyin:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

1. Ağ hizmeti aşağıdaki komutu çalıştırarak önyükleme sırasında başlayacağını emin olun:

        # sudo systemctl enable network

1. Aşağıdaki komutu çalıştırarak RHEL depodan paketler yüklenmesini etkinleştirmek için Red Hat aboneliğinizi kaydedin:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

1. Kernel önyükleme satırına grub yapılandırmanızda ek çekirdek parametreleri için Azure içerecek şekilde değiştirin. Bu değişikliği yapmak için açık `/etc/default/grub` bir metin düzenleyicisi ve düzenleme `GRUB_CMDLINE_LINUX` parametresi. Örneğin:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Bu yapılandırma, ayrıca, Azure size yardımcı olabilir ilk seri bağlantı noktası için tüm konsol iletileri gönderilmesini sağlar sorunları hata ayıklama desteği. NIC için de yeni RHEL 7 adlandırma kurallarını devre dışı bırakır. Ayrıca, aşağıdaki parametreleri kaldırmanızı öneririz:
   
        rhgb quiet crashkernel=auto
   
    Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilmesi için tüm günlükleri istediğimiz bir bulut ortamında kullanışlı değildir. Bırakabilirsiniz `crashkernel` isterseniz yapılandırılmış seçeneği. Bu parametre, daha küçük sanal makine boyutlarına sorunlu olabilir sanal makinede 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır unutmayın.

1. Bitirdikten sonra düzenleme `/etc/default/grub`, grub yapılandırması yeniden oluşturmak için aşağıdaki komutu çalıştırın:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

1. Hyper-V modüller için initramfs ekleyin.

    Düzen `/etc/dracut.conf`, içeriği ekleyin:

        add_drivers+=" hv_vmbus hv_netvsc hv_storvsc "

    İnitramfs yeniden oluşturun:

        # dracut -f -v

1. SSH sunucusu yüklü ve önyükleme sırasında başlatılacak şekilde yapılandırılmış emin olun. Bu genellikle varsayılan ayardır. Değiştirme `/etc/ssh/sshd_config` aşağıdaki satırı dahil etmek için:

        ClientAliveInterval 180

1. WALinuxAgent paket `WALinuxAgent-<version>`, Red Hat ek özellikler deposuna gönderildi. Ek özellikler depo, aşağıdaki komutu çalıştırarak etkinleştirin:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

1. Azure Linux Aracısı, aşağıdaki komutu çalıştırarak yükleyin:

        # sudo yum install WALinuxAgent

        # sudo systemctl enable waagent.service

1. İşletim sistemi diski üzerinde takas alanı oluşturabilirsiniz.

    Azure Linux Aracısı, Azure üzerinde sanal makine sağlandıktan sonra sanal makineye bağlı yerel kaynak diski kullanarak takas alanı otomatik olarak yapılandırabilirsiniz. Yerel kaynak disk geçici bir diski olduğundan ve sanal makinenin sağlaması, boşaltılabilir unutmayın. Önceki adımda Azure Linux Aracısı yükledikten sonra aşağıdaki parametrelerle değiştirin `/etc/waagent.conf` uygun şekilde:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

1. Abonelik kaydını kaldırmak istiyorsanız, aşağıdaki komutu çalıştırın:

        # sudo subscription-manager unregister

1. Sanal makinenin sağlamasını kaldırma ve Azure'da sağlama için hazırlamak için aşağıdaki komutları çalıştırın:

        # Mote: if you are migrating a specific virtual machine and do not wish to create a generalized image,
        # skip the deprovision step
        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

1. Sanal makineyi kapatır ve VMDK dosyasını VHD biçimine dönüştürün.

> [!NOTE]
> Qemu img sürümlerinde bilinen bir hata olduğunu > düzgün şekilde biçimlendirilmemiş bir VHD sonuçları 2.2.1 =. Sorun QEMU 2.6 içinde düzeltilmiştir. Qemu-img 2.2.0 ya da alt kullanın veya 2.6 veya sonraki bir sürüme güncelleştirmek için önerilir. Başvuru: https://bugs.launchpad.net/qemu/+bug/1490611.
>


    First convert the image to raw format:

        # qemu-img convert -f vmdk -O raw rhel-7.4.vmdk rhel-7.4.raw

    Make sure that the size of the raw image is aligned with 1 MB. Otherwise, round up the size to align with 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-7.4.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-7.4.raw $rounded_size

    Convert the raw disk to a fixed-sized VHD:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.4.raw rhel-7.4.vhd

    Or, with qemu version **2.6+** include the `force_size` option:

        # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-7.4.raw rhel-7.4.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a>Red Hat tabanlı bir sanal makine bir ISO'dan bir kickstart dosyası otomatik olarak kullanarak hazırlama
### <a name="prepare-a-rhel-7-virtual-machine-from-a-kickstart-file"></a>Kickstart dosyasından RHEL 7 sanal makineyi hazırlama

1.  Aşağıdaki içeriği içeren bir kickstart dosyası oluşturun ve dosyayı kaydedin. Kickstart yükleme hakkında daha fazla ayrıntı için bkz: [Kickstart Yükleme Kılavuzu](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).

        # Kickstart for provisioning a RHEL 7 Azure VM

        # System authorization information
          auth --enableshadow --passalgo=sha512

        # Use graphical install
        text

        # Do not run the Setup Agent on first boot
        firstboot --disable

        # Keyboard layouts
        keyboard --vckeymap=us --xlayouts='us'

        # System language
        lang en_US.UTF-8

        # Network information
        network  --bootproto=dhcp

        # Root password
        rootpw --plaintext "to_be_disabled"

        # System services
        services --enabled="sshd,waagent,NetworkManager"

        # System timezone
        timezone Etc/UTC --isUtc --ntpservers 0.rhel.pool.ntp.org,1.rhel.pool.ntp.org,2.rhel.pool.ntp.org,3.rhel.pool.ntp.org

        # Partition clearing information
        clearpart --all --initlabel

        # Clear the MBR
        zerombr

        # Disk partitioning information
        part /boot --fstype="xfs" --size=500
        part / --fstyp="xfs" --size=1 --grow --asprimary

        # System bootloader configuration
        bootloader --location=mbr

        # Firewall configuration
        firewall --disabled

        # Enable SELinux
        selinux --enforcing

        # Don't configure X
        skipx

        # Power down the machine after install
        poweroff

        %packages
        @base
        @console-internet
        chrony
        sudo
        parted
        -dracut-config-rescue

        %end

        %post --log=/var/log/anaconda/post-install.log

        #!/bin/bash

        # Register Red Hat Subscription
        subscription-manager register --username=XXX --password=XXX --auto-attach --force

        # Install latest repo update
        yum update -y

        # Enable extras repo
        subscription-manager repos --enable=rhel-7-server-extras-rpms

        # Install WALinuxAgent
        yum install -y WALinuxAgent

        # Unregister Red Hat subscription
        subscription-manager unregister

        # Enable waaagent at boot-up
        systemctl enable waagent

        # Disable the root account
        usermod root -p '!!'

        # Configure swap in WALinuxAgent
        sed -i 's/^\(ResourceDisk\.EnableSwap\)=[Nn]$/\1=y/g' /etc/waagent.conf
        sed -i 's/^\(ResourceDisk\.SwapSizeMB\)=[0-9]*$/\1=2048/g' /etc/waagent.conf

        # Set the cmdline
        sed -i 's/^\(GRUB_CMDLINE_LINUX\)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

        # Enable SSH keepalive
        sed -i 's/^#\(ClientAliveInterval\).*$/\1 180/g' /etc/ssh/sshd_config

        # Build the grub cfg
        grub2-mkconfig -o /boot/grub2/grub.cfg

        # Configure network
        cat << EOF > /etc/sysconfig/network-scripts/ifcfg-eth0
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no
        EOF

        # Deprovision and prepare for Azure if you are creating a generalized image
        waagent -force -deprovision

        %end

1. Yükleme Sistem, erişebileceğiniz kickstart dosyası yerleştirin.

1. Hyper-V Yöneticisi'nde yeni bir sanal makine oluşturun. Üzerinde **Sanal Sabit Disk Bağla** sayfasında **daha sonra bir sanal sabit disk ekleme**ve yeni sanal makine Sihirbazı tamamlayın.

1. Sanal makine ayarlarını açın:

    a.  Yeni bir sanal sabit disk sanal makineye ekleyin. Seçtiğinizden emin olun **VHD biçimi** ve **sabit boyutlu**.

    b.  ISO yükleme DVD sürücüsüne ekleyin.

    c.  CD'den önyükleme BIOS'u ayarlayın.

1. Sanal makineyi başlatın. Yükleme Kılavuzu görüntülendiğinde basın **sekmesini** önyükleme seçeneklerini yapılandırmak için.

1. ENTER `inst.ks=<the location of the kickstart file>` önyükleme seçenekleri ve ENTER tuşuna sonunda **Enter**.

1. Yüklemenin tamamlanmasını bekleyin. Tamamlandığında, sanal makine otomatik olarak kapatılır. Linux VHD'nizi artık Azure'a yüklenmek hazırdır.

## <a name="known-issues"></a>Bilinen sorunlar
### <a name="the-hyper-v-driver-could-not-be-included-in-the-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>Hyper-V sürücü başlangıç RAM disk bir Hyper-V olmayan hiper kullanırken eklenemedi

Bir Hyper-V ortamında çalıştığından emin Linux algılamadığı sürece bazı durumlarda, Linux yükleyicileri sürücüleri Hyper-V için başlangıç RAM disk (initrd veya initramfs) içermeyebilir.

Farklı sanallaştırma sistemi (diğer bir deyişle, Virtualbox, Xen, vb.), Linux görüntüsünü hazırlamak için kullanırken, en az emin olmak için initrd yeniden oluşturmanız gerekebilir hv_vmbus ve hv_storvsc çekirdek modülleri başlangıç RAM disk üzerinde kullanılabilir. Bilinen bir sorun en az Yukarı Akış Red Hat dağıtım tabanlı sistemlerde budur.

Bu sorunu çözmek için initramfs Hyper-V modüllerini ekleyin ve yeniden oluşturmak için:

Düzen `/etc/dracut.conf`ve aşağıdaki içeriği ekleyin:

        add_drivers+=" hv_vmbus hv_netvsc hv_storvsc "

İnitramfs yeniden oluşturun:

        # dracut -f -v

Daha fazla ayrıntı için bkz [initramfs yeniden](https://access.redhat.com/solutions/1958).

## <a name="next-steps"></a>Sonraki adımlar
Artık Azure'da yeni sanal makineler oluşturmak için Red Hat Enterprise Linux sanal sabit diski kullanmaya hazırsınız. Bu Azure'a .vhd dosyasını karşıya ilk kez kullanıyorsanız, bkz. [bir özel diskten Linux VM oluşturma](upload-vhd.md#option-1-upload-a-vhd).

Red Hat Enterprise Linux'ı çalıştırmak için sertifikalı hiper hakkında daha fazla ayrıntı için bkz. [Red Hat Web sitesi](https://access.redhat.com/certified-hypervisors).
