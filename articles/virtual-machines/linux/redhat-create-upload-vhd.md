---
title: "Red Hat Enterprise Linux VHD kullanılmak üzere Azure oluşturup | Microsoft Docs"
description: "Oluşturma ve bir Azure sanal sabit bir Red Hat Linux işletim sistemi içeren disk (VHD) yükleme hakkında bilgi edinme."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 6c6b8f72-32d3-47fa-be94-6cb54537c69f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: szark
ms.openlocfilehash: 2c48f95306ddce5d51100e869cc4ac80a4b55c20
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="prepare-a-red-hat-based-virtual-machine-for-azure"></a>Azure için Red Hat tabanlı bir sanal makine hazırlama
Bu makalede, Azure kullanmak için Red Hat Enterprise Linux (RHEL) sanal makineyi hazırlama öğreneceksiniz. Bu makalede ele alınan RHEL sürümleri 6.7 + ve 7.1 +. Bu makalede ele alınan hiper hazırlık için Hyper-V, çekirdek tabanlı sanal makine (KVM) ve VMware ' dir. Red Hat'ın bulut erişimi programına katılmasını için uygunluk gereksinimleri hakkında daha fazla bilgi için bkz: [Red Hat'ın bulut Access Web sitesinin](http://www.redhat.com/en/technologies/cloud-computing/cloud-access) ve [Azure üzerinde çalışan RHEL](https://access.redhat.com/ecosystem/ccsp/microsoft-azure).

## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a>Red Hat tabanlı sanal makineyi Hyper-V Yöneticisi'nden hazırlama

### <a name="prerequisites"></a>Önkoşullar
Bu bölümde, zaten bir ISO dosyası Red Hat Web sitesinden aldığınız ve RHEL görüntü sanal sabit diske (VHD) yüklü olduğunu varsayar. Bir işletim sistemi görüntüsünü yüklemek için Hyper-V Yöneticisi'ni kullanma hakkında daha fazla ayrıntı için bkz: [Hyper-V rolünü yükleyin ve sanal makine yapılandırma](http://technet.microsoft.com/library/hh846766.aspx).

**RHEL yükleme notları**

* Azure VHDX biçimi desteklemiyor. Azure destekler, VHD yalnızca sabit. Disk VHD biçimine dönüştürmek için Hyper-V Yöneticisi'ni kullanabilirsiniz veya convert-vhd cmdlet'ini kullanabilirsiniz. VirtualBox kullanıyorsanız seçin **boyutu sabit** disk oluşturduğunuzda seçeneği dinamik olarak ayrılan varsayılan aksine.
* Azure yalnızca 1. nesil sanal makineler destekler. 1. nesil sanal makine VHD dosya biçimine VHDX ve dinamik olarak genişletilen bir sabit boyutlu disk dönüştürebilirsiniz. Bir sanal makinenin oluşturma değiştiremezsiniz. Daha fazla bilgi için bkz: [Hyper-V'de 1 veya 2. nesil sanal makine oluşturmalısınız?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).
* VHD için izin verilen en büyük boyutu 1,023 GB'dir.
* Linux işletim sistemi yüklediğinizde, genellikle birçok yüklemeleri için varsayılan olduğu mantıksal Birimi Yöneticisi (LVM) yerine standart bölümleri kullanmanızı öneririz. Özellikle herhangi bir işletim sistemi diski başka bir özdeş sanal sorun giderme için makine ekleme gerekirse bu yöntem, kopyalanan sanal makineleri LVM adıyla çakışıyor kaçının. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veri diskler üzerinde kullanılabilir.
* Evrensel Disk Biçimi (UDF) dosya sistemleri bağlanması için çekirdek desteği gereklidir. Azure üzerinde ilk önyükleme sırasında Konuk bağlı UDF biçimli medya sağlama yapılandırma Linux sanal makineye iletir. Azure Linux Aracısı'nı yapılandırmasını okumak ve sanal makine sağlamak için UDF dosya sistemi bağlama kurabilmesi gerekir.
* Linux çekirdek Internet Explorer'ın 2.6.37 önceki sürümlerini Tekdüzen olmayan bellek erişimi (NUMA) üzerinde Hyper-V ile daha büyük sanal makine boyutları desteklemez. Yukarı Akış kullanan eski dağıtımları Bu sorun öncelikle etkileri Red Hat 2.6.32 çekirdek ve RHEL 6.6 (çekirdek 2.6.32 504) giderilmiştir. Özel tekrar çalıştıran sistemleri 2.6.37 eski veya 2.6.32-504 eski olan RHEL tabanlı tekrar ayarlamalıdır `numa=off` grub.conf çekirdek komut satırında parametre önyükleme. Red Hat daha fazla bilgi için bkz: [KB 436883](https://access.redhat.com/solutions/436883).
* Bir takas bölüm işletim sistemi disk üzerinde yapılandırmayın. Linux Aracısı geçici kaynak disk üzerinde bir takas dosyası oluşturmak için yapılandırılabilir.  Aşağıdaki adımlarda bu hakkında daha fazla bilgi bulunabilir.
* Tüm VHD'leri Azure üzerinde bir sanal boyutu 1 MB ile hizalı olması gerekir. Ham diskten VHD'ye dönüştürülürken ham disk boyutu 1 MB dönüştürmeden önce birden fazla olduğundan emin olmanız gerekir. Aşağıdaki adımlarda, daha fazla ayrıntı bulunabilir. Ayrıca bkz. [Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes) daha fazla bilgi için.

### <a name="prepare-a-rhel-6-virtual-machine-from-hyper-v-manager"></a>Hyper-V Yöneticisi'nden RHEL 6 sanal makineyi hazırlama

1. Hyper-V Yöneticisi'nde sanal makineyi seçin.

2. Tıklatın **Bağlan** sanal makine için bir konsol penceresi açın.

3. RHEL 6'da, Azure Linux Aracısı ile NetworkManager etkileyebilir. Bu paket, aşağıdaki komutu çalıştırarak kaldırın:
   
        # sudo rpm -e --nodeps NetworkManager

4. Oluşturma veya düzenleme `/etc/sysconfig/network` dosyasını bulun ve aşağıdaki metni ekleyin:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. Oluşturma veya düzenleme `/etc/sysconfig/network-scripts/ifcfg-eth0` dosyasını bulun ve aşağıdaki metni ekleyin:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. Ethernet arabirimi için statik kuralları oluşturulmasını önlemek için udev kuralları taşıyın (veya kaldırın). Bir sanal makinede Microsoft Azure veya Hyper-V: kopyaladığınızda, bu kurallar sorunlara neden

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. Ağ hizmeti aşağıdaki komutu çalıştırarak önyükleme sırasında başlayacağını emin olun:

        # sudo chkconfig network on

8. Aşağıdaki komutu çalıştırarak RHEL depodan paketler yüklemesini etkinleştirmek için Red Hat aboneliğinizin kaydedin:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

9. WALinuxAgent paket `WALinuxAgent-<version>`, Red Hat ek özellikler depoya gönderilir. Ek özellikler deposu, aşağıdaki komutu çalıştırarak etkinleştirin:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

10. Azure için ek çekirdek parametreleri içerecek şekilde kaz yapılandırma çekirdek önyükleme satırı değiştirin. Bu değişikliği yapmak için açık `/boot/grub/menu.lst` bir metin düzenleyicisinde ve varsayılan çekirdek aşağıdaki parametreleri içerdiğinden emin olun:
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    Bu, Azure yardımcı olabilecek ilk seri bağlantı noktasına gönderilen tüm konsol iletileri sağlayacak sorunları ayıklama desteği.
    
    Ayrıca, aşağıdaki parametreleri Kaldır önerilir:
    
        rhgb quiet crashkernel=auto
    
    Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilecek tüm günlükleri burada istiyoruz bulut ortamında bulunan kullanışlı değildir.  Bırakabilirsiniz `crashkernel` isterseniz yapılandırılmış seçeneği. Bu parametre sanal makine tarafından 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır unutmayın. Bu yapılandırma daha küçük sanal makine boyutlarını sorunlu olabilir.

    >[!Important]
    RHEL 6.5 ve önceki sürümleri de ayarlamanız gerekir `numa=off` çekirdek parametresi. Red Hat bkz [KB 436883](https://access.redhat.com/solutions/436883).

11. Güvenli Kabuk (SSH) sunucusu yüklü ve genellikle varsayılan değer önyükleme sırasında başlatılacak şekilde yapılandırılmış olduğundan emin olun. /Etc/ssh/sshd_config aşağıdaki satırı içerecek şekilde değiştirin:

        ClientAliveInterval 180

12. Aşağıdaki komutu çalıştırarak Azure Linux Aracısı'nı yükleyin:

        # sudo yum install WALinuxAgent

        # sudo chkconfig waagent on

    WALinuxAgent paketi yükleniyor NetworkManager kaldırır ve zaten kaldırılmadı varsa NetworkManager gnome paketleri adım 3'te.

13. Takas alanı işletim sistemi disk üzerinde oluşturmayın.

    Azure Linux Aracısı, Azure üzerinde sanal makine sağlandıktan sonra sanal makineye bağlı yerel kaynak disk kullanarak takas alanı otomatik olarak yapılandırabilirsiniz. Yerel kaynak disk geçici bir diski olduğundan ve sanal makine sağlaması kaldırılıyor. sağlaması zaman, boşaltılabilir olduğunu unutmayın. Önceki adımda Azure Linux Aracısı'nı yükledikten sonra aşağıdaki /etc/waagent.conf parametrelerinde uygun şekilde değiştirin:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

14. Abonelik, aşağıdaki komutu çalıştırarak (gerekirse) kaydı:

        # sudo subscription-manager unregister

15. Sanal makine yetkisini kaldırma ve Azure üzerinde sağlamak için hazırlamak için aşağıdaki komutları çalıştırın:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

16. Tıklatın **eylem** > **kapatma** Hyper-V Yöneticisi'nde. Linux VHD Azure'a karşıya yüklenecek artık hazırdır.


### <a name="prepare-a-rhel-7-virtual-machine-from-hyper-v-manager"></a>Hyper-V Yöneticisi'nden RHEL 7 sanal makineyi hazırlama

1. Hyper-V Yöneticisi'nde sanal makineyi seçin.

2. Tıklatın **Bağlan** sanal makine için bir konsol penceresi açın.

3. Oluşturma veya düzenleme `/etc/sysconfig/network` dosyasını bulun ve aşağıdaki metni ekleyin:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4. Oluşturma veya düzenleme `/etc/sysconfig/network-scripts/ifcfg-eth0` dosyasını bulun ve aşağıdaki metni ekleyin:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

5. Ağ hizmeti aşağıdaki komutu çalıştırarak önyükleme sırasında başlayacağını emin olun:

        # sudo chkconfig network on

6. Aşağıdaki komutu çalıştırarak RHEL depodan paketler yüklemesini etkinleştirmek için Red Hat aboneliğinizin kaydedin:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7. Azure için ek çekirdek parametreleri içerecek şekilde kaz yapılandırma çekirdek önyükleme satırı değiştirin. Bu değişikliği yapmak için açık `/etc/default/grub` bir metin Düzenleyicisi'ni ve düzenleme `GRUB_CMDLINE_LINUX` parametresi. Örneğin:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Bu, Azure yardımcı olabilecek ilk seri bağlantı noktasına gönderilen tüm konsol iletileri sağlayacak sorunları ayıklama desteği. Bu yapılandırma aynı zamanda NIC için yeni RHEL 7 adlandırma kuralları devre dışı bırakır. Ayrıca, aşağıdaki parametreleri kaldırmanızı öneririz:
   
        rhgb quiet crashkernel=auto
   
    Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilecek tüm günlükleri burada istiyoruz bulut ortamında bulunan kullanışlı değildir. Bırakabilirsiniz `crashkernel` isterseniz yapılandırılmış seçeneği. Bu parametre, daha küçük sanal makine boyutlarını sorunlu olabilir sanal makine tarafından 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır unutmayın.

8. Tamamlandıktan sonra düzenleme `/etc/default/grub`, kaz yapılandırma yeniden oluşturmak için aşağıdaki komutu çalıştırın:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

9. SSH sunucusu yüklü ve genellikle varsayılan değer önyükleme sırasında başlatılacak şekilde yapılandırılmış olduğundan emin olun. Değiştirme `/etc/ssh/sshd_config` aşağıdaki satırı eklemek için:

        ClientAliveInterval 180

10. WALinuxAgent paket `WALinuxAgent-<version>`, Red Hat ek özellikler depoya gönderilir. Ek özellikler deposu, aşağıdaki komutu çalıştırarak etkinleştirin:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

11. Aşağıdaki komutu çalıştırarak Azure Linux Aracısı'nı yükleyin:

        # sudo yum install WALinuxAgent

        # sudo systemctl enable waagent.service

12. Takas alanı işletim sistemi disk üzerinde oluşturmayın.

    Azure Linux Aracısı, Azure üzerinde sanal makine sağlandıktan sonra sanal makineye bağlı yerel kaynak disk kullanarak takas alanı otomatik olarak yapılandırabilirsiniz. Yerel kaynak disk geçici bir diski olduğundan ve sanal makine sağlaması kaldırılıyor. sağlaması zaman boşaltılabilir unutmayın. Önceki adımda Azure Linux Aracısı'nı yükledikten sonra aşağıdaki parametreleri değiştirmek `/etc/waagent.conf` uygun şekilde:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Abonelik kaydı istiyorsanız, aşağıdaki komutu çalıştırın:

        # sudo subscription-manager unregister

14. Sanal makine yetkisini kaldırma ve Azure üzerinde sağlamak için hazırlamak için aşağıdaki komutları çalıştırın:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

15. Tıklatın **eylem** > **kapatma** Hyper-V Yöneticisi'nde. Linux VHD Azure'a karşıya yüklenecek artık hazırdır.


## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a>Red Hat tabanlı sanal makineyi KVM hazırlama
### <a name="prepare-a-rhel-6-virtual-machine-from-kvm"></a>KVM RHEL 6 sanal makineyi hazırlama

1. RHEL 6 KVM görüntüsü Red Hat Web sitesinden indirin.

2. Bir kök parola ayarlayın.

    Şifrelenmiş bir parola oluşturmak ve komutunun çıktısını kopyalayın:

        # openssl passwd -1 changeme

    Guestfish olan bir kök parola ayarlayın:
        
        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   Kök kullanıcıdan ikinci alanı değiştirme "!!" şifrelenmiş parolası.

3. Bir sanal makine içinde KVM qcow2 görüntüden oluşturun. Disk türünü ayarlamak **qcow2**, sanal ağ arabirimi cihaz modeli ayarlanmış ve **virtio**. Ardından, sanal makineyi başlatmak ve kök olarak oturum açın.

4. Oluşturma veya düzenleme `/etc/sysconfig/network` dosyasını bulun ve aşağıdaki metni ekleyin:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. Oluşturma veya düzenleme `/etc/sysconfig/network-scripts/ifcfg-eth0` dosyasını bulun ve aşağıdaki metni ekleyin:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. Ethernet arabirimi için statik kuralları oluşturulmasını önlemek için udev kuralları taşıyın (veya kaldırın). Bir sanal makinede Azure veya Hyper-V: kopyaladığınızda, bu kurallar sorunlara neden

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. Ağ hizmeti aşağıdaki komutu çalıştırarak önyükleme sırasında başlayacağını emin olun:

        # chkconfig network on

8. Aşağıdaki komutu çalıştırarak RHEL depodan paketler yüklemesini etkinleştirmek için Red Hat aboneliğinizin kaydedin:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

9. Azure için ek çekirdek parametreleri içerecek şekilde kaz yapılandırma çekirdek önyükleme satırı değiştirin. Bu yapılandırma yapmak için açık `/boot/grub/menu.lst` bir metin düzenleyicisinde ve varsayılan çekirdek aşağıdaki parametreleri içerdiğinden emin olun:
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    Bu, Azure yardımcı olabilecek ilk seri bağlantı noktasına gönderilen tüm konsol iletileri sağlayacak sorunları ayıklama desteği.
    
    Ayrıca, aşağıdaki parametreleri kaldırmanızı öneririz:
    
        rhgb quiet crashkernel=auto
    
    Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilecek tüm günlükleri burada istiyoruz bulut ortamında bulunan kullanışlı değildir. Bırakabilirsiniz `crashkernel` isterseniz yapılandırılmış seçeneği. Bu parametre, daha küçük sanal makine boyutlarını sorunlu olabilir sanal makine tarafından 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır unutmayın.

    >[!Important]
    RHEL 6.5 ve önceki sürümleri de ayarlamanız gerekir `numa=off` çekirdek parametresi. Red Hat bkz [KB 436883](https://access.redhat.com/solutions/436883).

10. Hyper-V modüllerini için initramfs ekleyin:  

    Düzen `/etc/dracut.conf`, aşağıdaki içeriği ekleyin:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    İnitramfs yeniden oluşturun:

        # dracut -f -v

11. Bulut init kaldırın:

        # yum remove cloud-init

12. SSH sunucusu yüklü ve önyükleme sırasında başlatılacak şekilde yapılandırılmış olduğundan emin olun:

        # chkconfig sshd on

    Aşağıdaki satırları içerecek şekilde /etc/ssh/sshd_config değiştirin:

        PasswordAuthentication yes
        ClientAliveInterval 180

13. WALinuxAgent paket `WALinuxAgent-<version>`, Red Hat ek özellikler depoya gönderilir. Ek özellikler deposu, aşağıdaki komutu çalıştırarak etkinleştirin:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

14. Aşağıdaki komutu çalıştırarak Azure Linux Aracısı'nı yükleyin:

        # yum install WALinuxAgent

        # chkconfig waagent on

15. Azure Linux Aracısı, Azure üzerinde sanal makine sağlandıktan sonra sanal makineye bağlı yerel kaynak disk kullanarak takas alanı otomatik olarak yapılandırabilirsiniz. Yerel kaynak disk geçici bir diski olduğundan ve sanal makine sağlaması kaldırılıyor. sağlaması zaman boşaltılabilir unutmayın. Önceki adımda Azure Linux Aracısı'nı yükledikten sonra aşağıdaki parametre değiştirme **/etc/waagent.conf** uygun şekilde:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Abonelik, aşağıdaki komutu çalıştırarak (gerekirse) kaydı:

        # subscription-manager unregister

17. Sanal makine yetkisini kaldırma ve Azure üzerinde sağlamak için hazırlamak için aşağıdaki komutları çalıştırın:

        # waagent -force -deprovision

        # export HISTSIZE=0

        # logout

18. KVM sanal makineyi kapatın.

19. Qcow2 görüntü VHD biçimine dönüştürün.

> [!NOTE]
> Qemu img sürümlerinde bilinen bir hata varsa > düzgün biçimlendirilmemiş bir VHD sonuçları 2.2.1 =. Sorun QEMU 2.6 düzeltilmiştir. Qemu img 2.2.0 veya alt kullanın veya 2.6 veya daha yüksek güncelleştirmek için önerilir. Başvuru: https://bugs.launchpad.net/qemu/+bug/1490611.
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

        
### <a name="prepare-a-rhel-7-virtual-machine-from-kvm"></a>KVM RHEL 7 sanal makineyi hazırlama

1. RHEL 7 KVM görüntüsü Red Hat Web sitesinden indirin. Bu yordamda RHEL 7 örnek olarak kullanılmıştır.

2. Bir kök parola ayarlayın.

    Şifrelenmiş bir parola oluşturmak ve komutunun çıktısını kopyalayın:

        # openssl passwd -1 changeme

    Guestfish olan bir kök parola ayarlayın:

        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   Kök kullanıcıdan ikinci alanı değiştirme "!!" şifrelenmiş parolası.

3. Bir sanal makine içinde KVM qcow2 görüntüden oluşturun. Disk türünü ayarlamak **qcow2**, sanal ağ arabirimi cihaz modeli ayarlanmış ve **virtio**. Ardından, sanal makineyi başlatmak ve kök olarak oturum açın.

4. Oluşturma veya düzenleme `/etc/sysconfig/network` dosyasını bulun ve aşağıdaki metni ekleyin:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. Oluşturma veya düzenleme `/etc/sysconfig/network-scripts/ifcfg-eth0` dosyasını bulun ve aşağıdaki metni ekleyin:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

6. Ağ hizmeti aşağıdaki komutu çalıştırarak önyükleme sırasında başlayacağını emin olun:

        # chkconfig network on

7. Aşağıdaki komutu çalıştırarak RHEL depodan paketler yüklenmesine olanak sağlamak için Red Hat aboneliğinizi kaydedin:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

8. Azure için ek çekirdek parametreleri içerecek şekilde kaz yapılandırma çekirdek önyükleme satırı değiştirin. Bu yapılandırma yapmak için açık `/etc/default/grub` bir metin Düzenleyicisi'ni ve düzenleme `GRUB_CMDLINE_LINUX` parametresi. Örneğin:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Bu komut, aynı zamanda tüm konsol iletileri hangi Azure yardımcı olabilecek ilk seri bağlantı noktasına gönderilir sağlar sorunları ayıklama desteği. Komut ayrıca NIC'ler için yeni RHEL 7 adlandırma kuralları devre dışı bırakır. Ayrıca, aşağıdaki parametreleri kaldırmanızı öneririz:
   
        rhgb quiet crashkernel=auto
   
    Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilecek tüm günlükleri burada istiyoruz bulut ortamında bulunan kullanışlı değildir. Bırakabilirsiniz `crashkernel` isterseniz yapılandırılmış seçeneği. Bu parametre, daha küçük sanal makine boyutlarını sorunlu olabilir sanal makine tarafından 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır unutmayın.

9. Tamamlandıktan sonra düzenleme `/etc/default/grub`, kaz yapılandırma yeniden oluşturmak için aşağıdaki komutu çalıştırın:

        # grub2-mkconfig -o /boot/grub2/grub.cfg

10. Hyper-V modüllerini initramfs ekleyin.

    Düzenleme `/etc/dracut.conf` ve içeriği ekleyin:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    İnitramfs yeniden oluşturun:

        # dracut -f -v

11. Bulut init kaldırın:

        # yum remove cloud-init

12. SSH sunucusu yüklü ve önyükleme sırasında başlatılacak şekilde yapılandırılmış olduğundan emin olun:

        # systemctl enable sshd

    Aşağıdaki satırları içerecek şekilde /etc/ssh/sshd_config değiştirin:

        PasswordAuthentication yes
        ClientAliveInterval 180

13. WALinuxAgent paket `WALinuxAgent-<version>`, Red Hat ek özellikler depoya gönderilir. Ek özellikler deposu, aşağıdaki komutu çalıştırarak etkinleştirin:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

14. Aşağıdaki komutu çalıştırarak Azure Linux Aracısı'nı yükleyin:

        # yum install WALinuxAgent

    Waagent hizmetini etkinleştirin:

        # systemctl enable waagent.service

15. Takas alanı işletim sistemi disk üzerinde oluşturmayın.

    Azure Linux Aracısı, Azure üzerinde sanal makine sağlandıktan sonra sanal makineye bağlı yerel kaynak disk kullanarak takas alanı otomatik olarak yapılandırabilirsiniz. Yerel kaynak disk geçici bir diski olduğundan ve sanal makine sağlaması kaldırılıyor. sağlaması zaman boşaltılabilir unutmayın. Önceki adımda Azure Linux Aracısı'nı yükledikten sonra aşağıdaki parametreleri değiştirmek `/etc/waagent.conf` uygun şekilde:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Abonelik, aşağıdaki komutu çalıştırarak (gerekirse) kaydı:

        # subscription-manager unregister

17. Sanal makine yetkisini kaldırma ve Azure üzerinde sağlamak için hazırlamak için aşağıdaki komutları çalıştırın:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

18. KVM sanal makineyi kapatın.

19. Qcow2 görüntü VHD biçimine dönüştürün.

> [!NOTE]
> Qemu img sürümlerinde bilinen bir hata varsa > düzgün biçimlendirilmemiş bir VHD sonuçları 2.2.1 =. Sorun QEMU 2.6 düzeltilmiştir. Qemu img 2.2.0 veya alt kullanın veya 2.6 veya daha yüksek güncelleştirmek için önerilir. Başvuru: https://bugs.launchpad.net/qemu/+bug/1490611.
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


## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a>VMware Red Hat tabanlı sanal makineyi hazırlama
### <a name="prerequisites"></a>Önkoşullar
Bu bölümde VMware RHEL sanal makine zaten yüklediğinizi varsayar. VMware içinde bir işletim sistemi yükleme hakkında daha fazla bilgi için bkz [VMware konuk işletim sistemi Yükleme Kılavuzu](http://partnerweb.vmware.com/GOSIG/home.html).

* Linux işletim sistemi yüklediğinizde, birçok yüklemeleri için varsayılan olduğu çoğunlukla LVM, yerine standart bölümleri kullanmanızı öneririz. Özellikle bir işletim sistemi diski hiç sorun giderme için başka bir sanal makineye bağlı gerekiyorsa, bu kopyalanan sanal makinenin LVM adıyla çakışıyor önler. LVM veya RAID veri disklerde tercih edilen varsa kullanılabilir.
* Bir takas bölüm işletim sistemi disk üzerinde yapılandırmayın. Geçici kaynak disk üzerinde bir takas dosyası oluşturmak için Linux Aracısı yapılandırabilirsiniz. Aşağıdaki adımlarda bu hakkında daha fazla bilgi bulabilirsiniz.
* Sanal sabit disk oluşturduğunuzda, seçin **depolama sanal diski tek bir dosya olarak**.

### <a name="prepare-a-rhel-6-virtual-machine-from-vmware"></a>VMware RHEL 6 sanal makineyi hazırlama
1. RHEL 6'da, Azure Linux Aracısı ile NetworkManager etkileyebilir. Bu paket, aşağıdaki komutu çalıştırarak kaldırın:
   
        # sudo rpm -e --nodeps NetworkManager

2. Adlı bir dosya oluşturun **ağ** aşağıdaki metni içeren/etc/sysconfig/dizininde:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

3. Oluşturma veya düzenleme `/etc/sysconfig/network-scripts/ifcfg-eth0` dosyasını bulun ve aşağıdaki metni ekleyin:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

4. Ethernet arabirimi için statik kuralları oluşturulmasını önlemek için udev kuralları taşıyın (veya kaldırın). Bir sanal makinede Azure veya Hyper-V: kopyaladığınızda, bu kurallar sorunlara neden

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

5. Ağ hizmeti aşağıdaki komutu çalıştırarak önyükleme sırasında başlayacağını emin olun:

        # sudo chkconfig network on

6. Aşağıdaki komutu çalıştırarak RHEL depodan paketler yüklemesini etkinleştirmek için Red Hat aboneliğinizin kaydedin:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7. WALinuxAgent paket `WALinuxAgent-<version>`, Red Hat ek özellikler depoya gönderilir. Ek özellikler deposu, aşağıdaki komutu çalıştırarak etkinleştirin:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

8. Azure için ek çekirdek parametreleri içerecek şekilde kaz yapılandırma çekirdek önyükleme satırı değiştirin. Bunu yapmak için açın `/etc/default/grub` bir metin Düzenleyicisi'ni ve düzenleme `GRUB_CMDLINE_LINUX` parametresi. Örneğin:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0"
   
   Bu, Azure yardımcı olabilecek ilk seri bağlantı noktasına gönderilen tüm konsol iletileri sağlayacak sorunları ayıklama desteği. Ayrıca, aşağıdaki parametreleri kaldırmanızı öneririz:
   
        rhgb quiet crashkernel=auto
   
    Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilecek tüm günlükleri burada istiyoruz bulut ortamında bulunan kullanışlı değildir. Bırakabilirsiniz `crashkernel` isterseniz yapılandırılmış seçeneği. Bu parametre, daha küçük sanal makine boyutlarını sorunlu olabilir sanal makine tarafından 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır unutmayın.

9. Hyper-V modüllerini için initramfs ekleyin:

    Düzen `/etc/dracut.conf`, aşağıdaki içeriği ekleyin:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    İnitramfs yeniden oluşturun:

        # dracut -f -v

10. SSH sunucusu yüklü ve genellikle varsayılan değer önyükleme sırasında başlatılacak şekilde yapılandırılmış olduğundan emin olun. Değiştirme `/etc/ssh/sshd_config` aşağıdaki satırı eklemek için:

    ClientAliveInterval 180

11. Aşağıdaki komutu çalıştırarak Azure Linux Aracısı'nı yükleyin:

        # sudo yum install WALinuxAgent

        # sudo chkconfig waagent on

12. Takas alanı işletim sistemi disk üzerinde oluşturmayın.

    Azure Linux Aracısı, Azure üzerinde sanal makine sağlandıktan sonra sanal makineye bağlı yerel kaynak disk kullanarak takas alanı otomatik olarak yapılandırabilirsiniz. Yerel kaynak disk geçici bir diski olduğundan ve sanal makine sağlaması kaldırılıyor. sağlaması zaman boşaltılabilir unutmayın. Önceki adımda Azure Linux Aracısı'nı yükledikten sonra aşağıdaki parametreleri değiştirmek `/etc/waagent.conf` uygun şekilde:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Abonelik, aşağıdaki komutu çalıştırarak (gerekirse) kaydı:

        # sudo subscription-manager unregister

14. Sanal makine yetkisini kaldırma ve Azure üzerinde sağlamak için hazırlamak için aşağıdaki komutları çalıştırın:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

15. Sanal makineyi kapatın ve VMDK dosyasını bir .vhd dosyasına dönüştürün.

> [!NOTE]
> Qemu img sürümlerinde bilinen bir hata varsa > düzgün biçimlendirilmemiş bir VHD sonuçları 2.2.1 =. Sorun QEMU 2.6 düzeltilmiştir. Qemu img 2.2.0 veya alt kullanın veya 2.6 veya daha yüksek güncelleştirmek için önerilir. Başvuru: https://bugs.launchpad.net/qemu/+bug/1490611.
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


### <a name="prepare-a-rhel-7-virtual-machine-from-vmware"></a>VMware RHEL 7 sanal makineyi hazırlama
1. Oluşturma veya düzenleme `/etc/sysconfig/network` dosyasını bulun ve aşağıdaki metni ekleyin:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

2. Oluşturma veya düzenleme `/etc/sysconfig/network-scripts/ifcfg-eth0` dosyasını bulun ve aşağıdaki metni ekleyin:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

3. Ağ hizmeti aşağıdaki komutu çalıştırarak önyükleme sırasında başlayacağını emin olun:

        # sudo chkconfig network on

4. Aşağıdaki komutu çalıştırarak RHEL depodan paketler yüklemesini etkinleştirmek için Red Hat aboneliğinizin kaydedin:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

5. Azure için ek çekirdek parametreleri içerecek şekilde kaz yapılandırma çekirdek önyükleme satırı değiştirin. Bu değişikliği yapmak için açık `/etc/default/grub` bir metin Düzenleyicisi'ni ve düzenleme `GRUB_CMDLINE_LINUX` parametresi. Örneğin:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Bu yapılandırma, aynı zamanda tüm konsol iletileri hangi Azure yardımcı olabilecek ilk seri bağlantı noktasına gönderilir sağlar sorunları ayıklama desteği. Aynı zamanda NIC için yeni RHEL 7 adlandırma kuralları devre dışı bırakır. Ayrıca, aşağıdaki parametreleri kaldırmanızı öneririz:
   
        rhgb quiet crashkernel=auto
   
    Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilecek tüm günlükleri burada istiyoruz bulut ortamında bulunan kullanışlı değildir. Bırakabilirsiniz `crashkernel` isterseniz yapılandırılmış seçeneği. Bu parametre, daha küçük sanal makine boyutlarını sorunlu olabilir sanal makine tarafından 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır unutmayın.

6. Tamamlandıktan sonra düzenleme `/etc/default/grub`, kaz yapılandırma yeniden oluşturmak için aşağıdaki komutu çalıştırın:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

7. Hyper-V modüllerini initramfs için ekleyin.

    Düzen `/etc/dracut.conf`, içeriği ekleyin:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    İnitramfs yeniden oluşturun:

        # dracut -f -v

8. SSH sunucusu yüklü ve önyükleme sırasında başlatılacak şekilde yapılandırılmış olduğundan emin olun. Bu genellikle varsayılan ayardır. Değiştirme `/etc/ssh/sshd_config` aşağıdaki satırı eklemek için:

        ClientAliveInterval 180

9. WALinuxAgent paket `WALinuxAgent-<version>`, Red Hat ek özellikler depoya gönderilir. Ek özellikler deposu, aşağıdaki komutu çalıştırarak etkinleştirin:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

10. Aşağıdaki komutu çalıştırarak Azure Linux Aracısı'nı yükleyin:

        # sudo yum install WALinuxAgent

        # sudo systemctl enable waagent.service

11. Takas alanı işletim sistemi disk üzerinde oluşturmayın.

    Azure Linux Aracısı, Azure üzerinde sanal makine sağlandıktan sonra sanal makineye bağlı yerel kaynak disk kullanarak takas alanı otomatik olarak yapılandırabilirsiniz. Yerel kaynak disk geçici bir diski olduğundan ve sanal makine sağlaması kaldırılıyor. sağlaması zaman boşaltılabilir unutmayın. Önceki adımda Azure Linux Aracısı'nı yükledikten sonra aşağıdaki parametreleri değiştirmek `/etc/waagent.conf` uygun şekilde:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

12. Abonelik kaydı istiyorsanız, aşağıdaki komutu çalıştırın:

        # sudo subscription-manager unregister

13. Sanal makine yetkisini kaldırma ve Azure üzerinde sağlamak için hazırlamak için aşağıdaki komutları çalıştırın:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

14. Sanal makineyi kapatın ve VMDK dosyasını VHD biçimine dönüştürün.

> [!NOTE]
> Qemu img sürümlerinde bilinen bir hata varsa > düzgün biçimlendirilmemiş bir VHD sonuçları 2.2.1 =. Sorun QEMU 2.6 düzeltilmiştir. Qemu img 2.2.0 veya alt kullanın veya 2.6 veya daha yüksek güncelleştirmek için önerilir. Başvuru: https://bugs.launchpad.net/qemu/+bug/1490611.
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


## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a>Red Hat tabanlı sanal makineyi kickstart dosyası otomatik olarak kullanarak bir ISO hazırlama
### <a name="prepare-a-rhel-7-virtual-machine-from-a-kickstart-file"></a>Bir kickstart dosyasından RHEL 7 sanal makineyi hazırlama

1.  Aşağıdaki içeriği içeren bir kickstart dosyası oluşturun ve dosyayı kaydedin. Kickstart yükleme hakkında daha fazla ayrıntı için bkz: [Kickstart Yükleme Kılavuzu'na](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).

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

        # Deprovision and prepare for Azure
        waagent -force -deprovision

        %end

2. Burada yükleme sistem erişebildiğinizi kickstart dosyasını yerleştirin.

3. Hyper-V Yöneticisi'nde yeni bir sanal makine oluşturun. Üzerinde **sanal Hard diske Bağlan** sayfasında, **bir sanal sabit diski sonra Ekle**ve yeni sanal makine Sihirbazı'nı tamamlayın.

4. Sanal makine ayarlarını açın:

    a.  Yeni bir sanal sabit disk sanal makineye Ekle. Seçtiğinizden emin olun **VHD biçimi** ve **sabit boyutlu**.

    b.  ISO yükleme DVD sürücüsü ekleyin.

    c.  CD'den önyükleme için BIOS ayarlayın.

5. Sanal makineyi başlatın. Yükleme Kılavuzu göründüğünde, basın **sekmesini** önyükleme seçeneklerini yapılandırmak için.

6. ENTER `inst.ks=<the location of the kickstart file>` önyükleme seçenekleri ve tuşuna sonunda **Enter**.

7. Yüklemesinin tamamlanması için bekleyin. Tamamlandığında, sanal makine otomatik olarak kapatılacak. Linux VHD Azure'a karşıya yüklenecek artık hazırdır.

## <a name="known-issues"></a>Bilinen sorunlar
### <a name="the-hyper-v-driver-could-not-be-included-in-the-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>Hyper-V sürücü başlangıç RAM diski, bir Hyper-V olmayan hiper kullanırken eklenemedi

Bir Hyper-V ortamında çalıştığını Linux algılar sürece bazı durumlarda, Linux yükleyicileri sürücüleri Hyper-V için başlangıç RAM disk (initrd veya initramfs) içermeyebilir.

Linux görüntüsünü hazırlamak için farklı sanallaştırma sistem (diğer bir deyişle, Virtualbox, Xen, vb.) kullanırken, en az emin olmak için initrd yeniden oluşturmanız gerekebilir hv_vmbus ve hv_storvsc çekirdek modülleri başlangıç RAM disk üzerindeki kullanılabilir. Bilinen bir sorun en az Yukarı Akış Red Hat dağıtım tabanlı sistemlerde budur.

Bu sorunu çözmek için initramfs Hyper-V modüllerini ekleyin ve yeniden oluşturmak için:

Düzen `/etc/dracut.conf`, aşağıdaki içeriği ekleyin:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

İnitramfs yeniden oluşturun:

        # dracut -f -v

Daha fazla ayrıntı için bilgi [initramfs yeniden](https://access.redhat.com/solutions/1958).

## <a name="next-steps"></a>Sonraki adımlar
Şimdi yeni sanal makineler oluşturmak için Red Hat Enterprise Linux sanal sabit diski kullanmak hazırsınız. Azure için .vhd dosyasını karşıya yüklüyoruz ilk kez kullanıyorsanız bkz [özel bir diskten bir Linux VM oluşturma](upload-vhd.md#option-1-upload-a-vhd).

Red Hat Enterprise Linux çalıştırmak için sertifikalı hiper hakkında daha fazla ayrıntı için bkz: [Red Hat Web sitesi](https://access.redhat.com/certified-hypervisors).
