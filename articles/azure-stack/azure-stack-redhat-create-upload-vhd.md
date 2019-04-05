---
title: Oluşturma ve Azure Stack'te kullanmak için Red Hat Enterprise Linux VHD'si yükleme | Microsoft Docs
description: Oluşturma ve bir Azure sanal bir Red Hat Linux işletim sistemi içeren sabit disk (VHD) yükleme öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: BradleyB
editor: ''
tags: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2019
ms.author: mabrigg
ms.reviewer: jeffgo
ms.lastreviewed: 08/15/2018
ms.openlocfilehash: 728839accbce80ece6795e098d5d2855320fab06
ms.sourcegitcommit: 045406e0aa1beb7537c12c0ea1fbf736062708e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "59006669"
---
# <a name="prepare-a-red-hat-based-virtual-machine-for-azure-stack"></a>Azure Stack için Red Hat tabanlı bir sanal makine hazırlama

Bu makalede, Azure Stack kullanım için Red Hat Enterprise Linux (RHEL) sanal makineyi hazırlama öğreneceksiniz. Bu makalede ele RHEL 7.1 + sürümleridir. Bu makalede ele hiper hazırlama için Hyper-V, çekirdek tabanlı sanal makine (KVM) ve VMware ' dir.

Red Hat Enterprise Linux destek bilgileri için başvurmak [Red Hat ve Azure Stack: Sık sorulan sorular](https://access.redhat.com/articles/3413531).

## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a>Red Hat tabanlı bir sanal makine Hyper-V Yöneticisi'nden hazırlama

Bu bölümde zaten varsa Red Hat Web sitesinden bir ISO dosyası ve RHEL görüntüsü sanal sabit diske (VHD) yüklü varsayılır. Bir işletim sistemi görüntüsünü yüklemek için Hyper-V Yöneticisi'ni kullanma hakkında daha fazla bilgi için bkz. [Hyper-V rolünü yükleme ve sanal makine yapılandırma](https://technet.microsoft.com/library/hh846766.aspx).

### <a name="rhel-installation-notes"></a>RHEL yükleme notları

* Azure Stack, VHDX biçimini desteklemiyor. Azure'un destekledikleri yalnızca VHD düzeltildi. Disk VHD biçimine dönüştürmek için Hyper-V Yöneticisi'ni kullanabilirsiniz veya convert-vhd cmdlet'ini kullanabilirsiniz. VirtualBox kullanıyorsanız belirleyin **boyutu sabit** diski oluştururken seçeneği dinamik olarak ayrılan varsayılan aksine.
* Azure Stack, yalnızca 1. kuşak sanal makineleri destekler. 1. nesil sanal makine VHD dosya biçimine VHDX ve bir sabit boyutlu disk için dinamik olarak genişletilen dönüştürebilirsiniz. Bir sanal makinenin oluşturulması değiştiremezsiniz. Daha fazla bilgi için [Hyper-V'de 1 veya 2. kuşak sanal makine oluşturmalısınız?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).
* VHD için izin verilen en büyük boyutu 1,023 GB'dir.
* Linux işletim sistemi yüklediğinizde, genellikle çoğu yüklemeleri için varsayılan olan mantıksal birim Yöneticisi (LVM) yerine standart bölümleri kullanmanızı öneririz. Özellikle, hiç olmadığı kadar başka bir özdeş sanal sorun giderme için makine bir işletim sistemi diski gerekiyorsa bu yöntem LVM ad çakışmalarını kopyalanan sanal makinelerle önler.
* Evrensel Disk Biçimi (UDF) dosya sistemleri bağlamak için çekirdek desteği gereklidir. İlk önyüklemede Konuk bağlı UDF biçimli medya Linux sanal makinesi için sağlama yapılandırması geçirir. Azure Linux Aracısı yapılandırmasını okuma ve sanal makine sağlamak için UDF dosya sistemini bağlamalarına gerekir.
* İşletim sistemi diski üzerinde takas bölümü yapılandırmayın. Linux Aracısı, geçici kaynak diski üzerinde takas dosyası oluşturmak için yapılandırılabilir. Aşağıdaki adımlarda hakkında daha fazla bilgi bulunabilir.
* Tüm VHD'leri azure'da bir sanal Boyut 1 MB ile uyumlu olması gerekir. Ham bir diskten VHD'ye dönüştürme yaparken, ham disk boyutu 1 MB dönüştürmeden önce bir çok olduğundan emin olmalısınız. Aşağıdaki adımlarda daha fazla ayrıntı bulunabilir.
* Azure Stack, cloud-init desteklemez. VM'niz ile desteklenen bir sürümünü, Windows Azure Linux Aracısı'nı (WALA) yapılandırılması gerekir.

### <a name="prepare-a-rhel-7-virtual-machine-from-hyper-v-manager"></a>Hyper-V Yöneticisi'nden RHEL 7 sanal makinesini hazırlama

1. Hyper-V Yöneticisi'nde, sanal makineyi seçin.

1. Tıklayın **Connect** sanal makine için bir konsol penceresi açın.

1. Oluşturma veya düzenleme `/etc/sysconfig/network` dosyasını bulun ve aşağıdaki metni ekleyin:

    ```sh
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

1. Oluşturma veya düzenleme `/etc/sysconfig/network-scripts/ifcfg-eth0` dosya ve gerektiğinde aşağıdaki metni ekleyin:

    ```sh
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    NM_CONTROLLED=no
    ```

1. Ağ hizmeti aşağıdaki komutu çalıştırarak önyükleme sırasında başladığından emin olun:

    ```bash
    sudo systemctl enable network
    ```

1. Aşağıdaki komutu çalıştırarak RHEL depodan paketler yüklenmesini etkinleştirmek için Red Hat aboneliğinizi kaydedin:

    ```bash
    sudo subscription-manager register --auto-attach --username=XXX --password=XXX
    ```

1. Kernel önyükleme satırına grub yapılandırmanızda ek çekirdek parametreleri için Azure içerecek şekilde değiştirin. Bu değişikliği yapmak için açık `/etc/default/grub` bir metin düzenleyicisinde ve değiştirme `GRUB_CMDLINE_LINUX` parametresi. Örneğin:

    ```sh
    GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
    ```

   Bu, Azure size yardımcı olabilir ilk seri bağlantı noktası için tüm konsol ileti gönderilmesini sağlar sorunları hata ayıklama desteği. Bu yapılandırma, ayrıca NIC'ler için yeni RHEL 7 adlandırma kurallarını devre dışı bırakır.

   Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilmesi için tüm günlükleri istediğimiz bir bulut ortamında kullanışlı değildir. Bırakabilirsiniz `crashkernel` isterseniz yapılandırılmış seçeneği. Bu parametre, daha küçük sanal makine boyutlarına sorunlu olabilir sanal makinede 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır unutmayın. Aşağıdaki parametreleri kaldırmanızı öneririz:

    ```sh
    rhgb quiet crashkernel=auto
    ```

1. Bitirdikten sonra düzenleme `/etc/default/grub`, grub yapılandırması yeniden oluşturmak için aşağıdaki komutu çalıştırın:

    ```bash
    sudo grub2-mkconfig -o /boot/grub2/grub.cfg
    ```

1. Durdur ve cloud-init kaldırın:

    ```bash
    systemctl stop cloud-init
    yum remove cloud-init
    ```

1. SSH sunucusu yüklü ve, genellikle varsayılan önyükleme sırasında başlatılacak şekilde yapılandırılmış emin olun. Değiştirme `/etc/ssh/sshd_config` aşağıdaki satırı dahil etmek için:

    ```sh
    ClientAliveInterval 180
    ```

1. WALinuxAgent paket `WALinuxAgent-<version>`, Red Hat ek özellikler deposuna gönderildi. Ek özellikler depo, aşağıdaki komutu çalıştırarak etkinleştirin:

    ```bash
    subscription-manager repos --enable=rhel-7-server-extras-rpms
    ```

1. Azure Linux Aracısı, aşağıdaki komutu çalıştırarak yükleyin:

    ```bash
    sudo yum install WALinuxAgent
    sudo systemctl enable waagent.service
    ```

1. İşletim sistemi diski üzerinde takas alanı oluşturabilirsiniz.

    Azure Linux Aracısı, Azure üzerinde sanal makine sağlandıktan sonra sanal makineye bağlı yerel kaynak diski kullanarak takas alanı otomatik olarak yapılandırabilirsiniz. Yerel kaynak disk geçici bir diski olduğundan ve sanal makinenin sağlaması kaldırıldığında boşaltılabilir. Önceki adımda Azure Linux Aracısı yükledikten sonra aşağıdaki parametrelerle değiştirin `/etc/waagent.conf` uygun şekilde:

    ```sh
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.EnableSwap=y
    ResourceDisk.SwapSizeMB=2048    #NOTE: set this to whatever you need it to be.
    ```

1. Abonelik kaydını kaldırmak istiyorsanız, aşağıdaki komutu çalıştırın:

    ```bash
    sudo subscription-manager unregister
    ```

1. Bir kuruluş sertifika yetkilisi kullanılarak dağıtılmıştır bir sistemi kullanıyorsanız, RHEL VM, Azure Stack kök sertifikasını güven yok. Güvenilen kök deposuna yerleştirmeniz gerekir. Bkz: [ekleyerek güvenilen kök sertifikalar sunucuya](https://manuals.gfi.com/en/kerio/connect/content/server-configuration/ssl-certificates/adding-trusted-root-certificates-to-the-server-1605.html).

1. Sanal makinenin sağlamasını kaldırma ve Azure'da sağlama için hazırlamak için aşağıdaki komutları çalıştırın:

    ```bash
    sudo waagent -force -deprovision
    export HISTSIZE=0
    logout
    ```

1. Tıklayın **eylem** > **kapatma** Hyper-V Yöneticisi'nde.

1. VHD'yi sabit boyutlu bir Hyper-V Yöneticisi'ni "Düzen disk" özelliği veya Convert-VHD PowerShell komutunu kullanarak VHD Dönüştür. Linux VHD'nizi artık Azure'a yüklenmek hazırdır.

## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a>Red Hat tabanlı bir sanal makine KVM hazırlama

1. RHEL 7 KVM görüntüsü Red Hat Web sitesinden indirin. Bu yordam, RHEL 7 örnek olarak kullanır.

1. Bir kök parola ayarlayın.

    Şifreli bir parola oluşturmak ve komutunun çıktısını kopyalayın:

    ```bash
    openssl passwd -1 changeme
    ```

   Guestfish ile bir kök parola ayarlayın:

    ```sh
    guestfish --rw -a <image-name>
    > <fs> run
    > <fs> list-filesystems
    > <fs> mount /dev/sda1 /
    > <fs> vi /etc/shadow
    > <fs> exit
    ```

   Kök kullanıcı ikinci alanı Değiştir "!!" şifrelenmiş parolası.

1. Bir sanal makine içinde KVM qcow2 görüntüden oluşturun. Disk türünü ayarlamak **qcow2**, sanal ağ arabirimi cihaz modeli ayarlanmış ve **virtio**. Ardından, sanal makineyi başlatın ve kök olarak oturum açın.

1. Oluşturma veya düzenleme `/etc/sysconfig/network` dosyasını bulun ve aşağıdaki metni ekleyin:

    ```sh
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

1. Oluşturma veya düzenleme `/etc/sysconfig/network-scripts/ifcfg-eth0` dosyasını bulun ve aşağıdaki metni ekleyin:

    ```sh
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    NM_CONTROLLED=no
    ```

1. Ağ hizmeti aşağıdaki komutu çalıştırarak önyükleme sırasında başladığından emin olun:

    ```bash
    sudo systemctl enable network
    ```

1. Aşağıdaki komutu çalıştırarak RHEL depodan paketler yüklenmesini etkinleştirmek için Red Hat aboneliğinizi kaydedin:

    ```bash
    subscription-manager register --auto-attach --username=XXX --password=XXX
    ```

1. Kernel önyükleme satırına grub yapılandırmanızda ek çekirdek parametreleri için Azure içerecek şekilde değiştirin. Bu yapılandırma için açmak `/etc/default/grub` bir metin düzenleyicisinde ve değiştirme `GRUB_CMDLINE_LINUX` parametresi. Örneğin:

    ```sh
    GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
    ```

   Bu komut, ayrıca, Azure size yardımcı olabilir ilk seri bağlantı noktası için tüm konsol iletileri gönderilmesini sağlar sorunları hata ayıklama desteği. Komutu, yeni RHEL 7 adlandırma kurallarını devre dışı da NIC'ler için açar.

   Grafik ve sessiz önyükleme tüm günlükler seri bağlantı noktasına nereye gönderileceğini bir bulut ortamında kullanışlı değildir. Bırakabilirsiniz `crashkernel` isterseniz yapılandırılmış seçeneği. Bu parametre, daha küçük sanal makine boyutlarına sorunlu olabilir sanal makinede 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır. Aşağıdaki parametreleri kaldırmanızı öneririz:

    ```sh
    rhgb quiet crashkernel=auto
    ```

1. Bitirdikten sonra düzenleme `/etc/default/grub`, grub yapılandırması yeniden oluşturmak için aşağıdaki komutu çalıştırın:

    ```bash
    grub2-mkconfig -o /boot/grub2/grub.cfg
    ```

1. Hyper-V modüllerini initramfs ekleyin.

    Düzen `/etc/dracut.conf` ve içeriği ekleyin:

    ```sh
    add_drivers+="hv_vmbus hv_netvsc hv_storvsc"
    ```

    İnitramfs yeniden oluşturun:

    ```bash
    dracut -f -v
    ```

1. Durdur ve cloud-init kaldırın:

    ```bash
    systemctl stop cloud-init
    yum remove cloud-init
    ```

1. SSH sunucusu yüklü ve önyükleme sırasında başlatılacak şekilde yapılandırılmış olduğundan emin olun:

    ```bash
    systemctl enable sshd
    ```

    /Etc/ssh/sshd_config aşağıdaki satırları içerecek şekilde değiştirin:

    ```sh
    PasswordAuthentication yes
    ClientAliveInterval 180
    ```

1. Azure Stack için özel bir vhd oluştururken, bir derleme 1903 önce çalışan Azure Stack ortamlarında WALinuxAgent sürüm 2.2.20 2.2.35.1 (her iki özel) arasındaki çalışmıyor aklınızda bulundurun. Bu sorunu çözmek için 1901/1902 düzeltmeyi veya ikinci yarısında hiç bu bölümündeki yönergeleri izleyin. 

    Bir Azure Stack derleme 1903 çalıştırıyorsanız (veya üzeri) veya 1901/1902 düzeltmesi, Redhat ek özellikler depodan WALinuxAgent paketini indirme şu şekilde:
    
   WALinuxAgent paket `WALinuxAgent-<version>`, Red Hat ek özellikler deposuna gönderildi. Ek özellikler depo, aşağıdaki komutu çalıştırarak etkinleştirin:

    ```bash
    subscription-manager repos --enable=rhel-7-server-extras-rpms
    ```

   Azure Linux Aracısı, aşağıdaki komutu çalıştırarak yükleyin:

    ```bash
    yum install WALinuxAgent
    ```

   Waagent hizmeti etkinleştirin:

    ```bash
    systemctl enable waagent.service
    ```
    
    
    Azure Stack derleme 1903 önce çalışan ve 1901/1902 düzeltme uygulanmamış, ardından WALinuxAgent indirmek için bu yönergeleri izleyin:
    
   a.   Setuptools indirin
    ```bash
    wget https://pypi.python.org/packages/source/s/setuptools/setuptools-7.0.tar.gz --no-check-certificate
    tar xzf setuptools-7.0.tar.gz
    cd setuptools-7.0
    ```
   b. İndirin ve github'ımızı aracısından en son sürümünü açın. Bir örnek burada biz "2.2.36" indirme budur github deposundan sürümü.
    ```bash
    wget https://github.com/Azure/WALinuxAgent/archive/v2.2.36.zip
    unzip v2.2.36.zip
    cd WALinuxAgent-2.2.36
    ```
    c. Setup.PY dosyasını yükleyin
    ```bash
    sudo python setup.py install
    ```
    d. Waagent yeniden başlatın
    ```bash
    sudo systemctl restart waagent
    ```
    e. Aracı sürümü bir, indirilen eşleşiyorsa, test edin. Bu örnekte, 2.2.36 olmalıdır.
    
    ```bash
    waagent -version
    ```

1. İşletim sistemi diski üzerinde takas alanı oluşturabilirsiniz.

    Azure Linux Aracısı, Azure üzerinde sanal makine sağlandıktan sonra sanal makineye bağlı yerel kaynak diski kullanarak takas alanı otomatik olarak yapılandırabilirsiniz. Yerel kaynak disk geçici bir diski olduğundan ve sanal makinenin sağlaması kaldırıldığında boşaltılabilir. Önceki adımda Azure Linux Aracısı yükledikten sonra aşağıdaki parametrelerle değiştirin `/etc/waagent.conf` uygun şekilde:

    ```sh
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.EnableSwap=y
    ResourceDisk.SwapSizeMB=2048    #NOTE: set this to whatever you need it to be.
    ```

1. Abonelik (gerekiyorsa) aşağıdaki komutu çalıştırarak kaydı:

    ```bash
    subscription-manager unregister
    ```

1. Bir kuruluş sertifika yetkilisi kullanılarak dağıtılmıştır bir sistemi kullanıyorsanız, RHEL VM, Azure Stack kök sertifikasını güven yok. Güvenilen kök deposuna yerleştirmeniz gerekir. Bkz: [ekleyerek güvenilen kök sertifikalar sunucuya](https://manuals.gfi.com/en/kerio/connect/content/server-configuration/ssl-certificates/adding-trusted-root-certificates-to-the-server-1605.html).

1. Sanal makinenin sağlamasını kaldırma ve Azure'da sağlama için hazırlamak için aşağıdaki komutları çalıştırın:

    ```bash
    sudo waagent -force -deprovision
    export HISTSIZE=0
    logout
    ```

1. KVM sanal makineyi kapatın.

1. Qcow2 görüntünün VHD biçimine dönüştürün.

    > [!NOTE]
    > Qemu img sürümlerinde bilinen bir hata olduğunu > düzgün şekilde biçimlendirilmemiş bir VHD sonuçları 2.2.1 =. Sorun QEMU 2.6 içinde düzeltilmiştir. Qemu-img 2.2.0 ya da alt kullanın veya 2.6 veya sonraki bir sürüme güncelleştirmek için önerilir. Başvuru: https://bugs.launchpad.net/qemu/+bug/1490611.

    Önce görüntünün ham biçimine dönüştürün:

    ```bash
    qemu-img convert -f qcow2 -O raw rhel-7.4.qcow2 rhel-7.4.raw
    ```

    Ham görüntüsünün boyutu 1 MB ile uyumlu olduğunu doğrulayın. Aksi takdirde, boyutu 1 MB ile hizalamak için yukarı yuvarlar:

    ```bash
    MB=$((1024*1024))
    size=$(qemu-img info -f raw --output json "rhel-7.4.raw" | \
    gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
    rounded_size=$((($size/$MB + 1)*$MB))
    qemu-img resize rhel-7.4.raw $rounded_size
    ```

    Ham disk sabit boyutlu VHD'ye dönüştürür:

    ```bash
    qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.4.raw rhel-7.4.vhd
    ```

    Veya qemu sürümüyle **2.6 +** dahil `force_size` seçeneği:

    ```bash
    qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-7.4.raw rhel-7.4.vhd
    ```

## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a>Vmware'den Red Hat tabanlı bir sanal makine hazırlama

Bu bölümde, VMware ortamınızda RHEL sanal makine zaten yüklediğinizi varsayar. VMware ortamınızda bir işletim sistemi yükleme hakkında daha fazla bilgi için bkz. [VMware konuk işletim sistemi Yükleme Kılavuzu](https://partnerweb.vmware.com/GOSIG/home.html).

* Linux işletim sistemi yüklediğinizde, genellikle çoğu yüklemeleri için varsayılan seçenek LVM, yerine standart bölümleri kullanmanızı öneririz. Özellikle bir işletim sistemi diski hiç sorun giderme için başka bir sanal makineye bağlı gerekiyorsa, bu kopyalanan sanal makinenin adı çakışıyor LVM önler. LVM'yi veya RAID veri disklerinde, tercih etmeleri durumunda kullanılabilir.
* İşletim sistemi diski üzerinde takas bölümü yapılandırmayın. Geçici kaynak diski üzerinde takas dosyası oluşturmak için Linux Aracısı yapılandırabilirsiniz. İzleyen adımları bu hakkında daha fazla bilgi bulabilirsiniz.
* Sanal sabit diski oluşturduğunuzda **Store sanal disk, tek bir dosya olarak**.

### <a name="prepare-a-rhel-7-virtual-machine-from-vmware"></a>Vmware'den RHEL 7 sanal makinesini hazırlama

1. Oluşturma veya düzenleme `/etc/sysconfig/network` dosyasını bulun ve aşağıdaki metni ekleyin:

    ```sh
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

1. Oluşturma veya düzenleme `/etc/sysconfig/network-scripts/ifcfg-eth0` dosyasını bulun ve aşağıdaki metni ekleyin:

    ```sh
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    NM_CONTROLLED=no
    ```

1. Ağ hizmeti aşağıdaki komutu çalıştırarak önyükleme sırasında başlayacağını emin olun:

    ```bash
    sudo chkconfig network on
    ```

1. Aşağıdaki komutu çalıştırarak RHEL depodan paketler yüklenmesini etkinleştirmek için Red Hat aboneliğinizi kaydedin:

    ```bash
    sudo subscription-manager register --auto-attach --username=XXX --password=XXX
    ```

1. Kernel önyükleme satırına grub yapılandırmanızda ek çekirdek parametreleri için Azure içerecek şekilde değiştirin. Bu değişikliği yapmak için açık `/etc/default/grub` bir metin düzenleyicisinde ve değiştirme `GRUB_CMDLINE_LINUX` parametresi. Örneğin:

    ```sh
    GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
    ```

    Bu yapılandırma, ayrıca, Azure size yardımcı olabilir ilk seri bağlantı noktası için tüm konsol iletileri gönderilmesini sağlar sorunları hata ayıklama desteği. NIC için de yeni RHEL 7 adlandırma kurallarını devre dışı bırakır. Ayrıca, aşağıdaki parametreleri kaldırmanızı öneririz:

    ```sh
    rhgb quiet crashkernel=auto
    ```

    Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilmesi için tüm günlükleri istediğimiz bir bulut ortamında kullanışlı değildir. Bırakabilirsiniz `crashkernel` isterseniz yapılandırılmış seçeneği. Bu parametre, daha küçük sanal makine boyutlarına sorunlu olabilir sanal makinede 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır unutmayın.

1. Bitirdikten sonra düzenleme `/etc/default/grub`, grub yapılandırması yeniden oluşturmak için aşağıdaki komutu çalıştırın:

    ```bash
    sudo grub2-mkconfig -o /boot/grub2/grub.cfg
    ```

1. Hyper-V modüller için initramfs ekleyin.

    Düzen `/etc/dracut.conf`, içeriği ekleyin:

    ```sh
    add_drivers+="hv_vmbus hv_netvsc hv_storvsc"
    ```

    İnitramfs yeniden oluşturun:

    ```bash
    dracut -f -v
    ```

1. Durdur ve cloud-init kaldırın:

    ```bash
    systemctl stop cloud-init
    yum remove cloud-init
    ```

1. SSH sunucusu yüklü ve önyükleme sırasında başlatılacak şekilde yapılandırılmış emin olun. Bu genellikle varsayılan ayardır. Değiştirme `/etc/ssh/sshd_config` aşağıdaki satırı dahil etmek için:

    ```sh
    ClientAliveInterval 180
    ```

1. WALinuxAgent paket `WALinuxAgent-<version>`, Red Hat ek özellikler deposuna gönderildi. Ek özellikler depo, aşağıdaki komutu çalıştırarak etkinleştirin:

    ```bash
    subscription-manager repos --enable=rhel-7-server-extras-rpms
    ```

1. Azure Linux Aracısı, aşağıdaki komutu çalıştırarak yükleyin:

    ```bash
    sudo yum install WALinuxAgent
    sudo systemctl enable waagent.service
    ```

1. İşletim sistemi diski üzerinde takas alanı oluşturabilirsiniz.

    Azure Linux Aracısı, Azure üzerinde sanal makine sağlandıktan sonra sanal makineye bağlı yerel kaynak diski kullanarak takas alanı otomatik olarak yapılandırabilirsiniz. Yerel kaynak disk geçici bir diski olduğundan ve sanal makinenin sağlaması kaldırıldığında boşaltılabilir unutmayın. Önceki adımda Azure Linux Aracısı yükledikten sonra aşağıdaki parametrelerle değiştirin `/etc/waagent.conf` uygun şekilde:

    ```sh
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.EnableSwap=y
    ResourceDisk.SwapSizeMB=2048    NOTE: set this to whatever you need it to be.
    ```

1. Abonelik kaydını kaldırmak istiyorsanız, aşağıdaki komutu çalıştırın:

    ```bash
    sudo subscription-manager unregister
    ```

1. Bir kuruluş sertifika yetkilisi kullanılarak dağıtılmıştır bir sistemi kullanıyorsanız, RHEL VM, Azure Stack kök sertifikasını güven yok. Güvenilen kök deposuna yerleştirmeniz gerekir. Bkz: [ekleyerek güvenilen kök sertifikalar sunucuya](https://manuals.gfi.com/en/kerio/connect/content/server-configuration/ssl-certificates/adding-trusted-root-certificates-to-the-server-1605.html).

1. Sanal makinenin sağlamasını kaldırma ve Azure'da sağlama için hazırlamak için aşağıdaki komutları çalıştırın:

    ```bash
    sudo waagent -force -deprovision
    export HISTSIZE=0
    logout
    ```

1. Sanal makineyi kapatır ve VMDK dosyasını VHD biçimine dönüştürün.

    > [!NOTE]
    > Qemu img sürümlerinde bilinen bir hata olduğunu > düzgün şekilde biçimlendirilmemiş bir VHD sonuçları 2.2.1 =. Sorun QEMU 2.6 içinde düzeltilmiştir. Qemu-img 2.2.0 ya da alt kullanın veya 2.6 veya sonraki bir sürüme güncelleştirmek için önerilir. Başvuru: <https://bugs.launchpad.net/qemu/+bug/1490611>.

    Önce görüntünün ham biçimine dönüştürün:

    ```bash
    qemu-img convert -f qcow2 -O raw rhel-7.4.qcow2 rhel-7.4.raw
    ```

    Ham görüntüsünün boyutu 1 MB ile uyumlu olduğunu doğrulayın. Aksi takdirde, boyutu 1 MB ile hizalamak için yukarı yuvarlar:

    ```bash
    MB=$((1024*1024))
    size=$(qemu-img info -f raw --output json "rhel-7.4.raw" | \
    gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
    rounded_size=$((($size/$MB + 1)*$MB))
    qemu-img resize rhel-7.4.raw $rounded_size
    ```

    Ham disk sabit boyutlu VHD'ye dönüştürür:

    ```bash
    qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.4.raw rhel-7.4.vhd
    ```

    Veya qemu sürümüyle **2.6 +** dahil `force_size` seçeneği:

    ```bash
    qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-7.4.raw rhel-7.4.vhd
    ```

## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a>Red Hat tabanlı bir sanal makine bir ISO'dan bir kickstart dosyası otomatik olarak kullanarak hazırlama

1. Aşağıdaki içeriği içeren bir kickstart dosyası oluşturun ve dosyayı kaydedin. Kickstart yükleme hakkında daha fazla ayrıntı için bkz: [Kickstart Yükleme Kılavuzu](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).

    ```sh
    Kickstart for provisioning a RHEL 7 Azure VM

    System authorization information
    auth --enableshadow --passalgo=sha512

    Use graphical install
    text

    Do not run the Setup Agent on first boot
    firstboot --disable

    Keyboard layouts
    keyboard --vckeymap=us --xlayouts='us'

    System language
    lang en_US.UTF-8

    Network information
    network  --bootproto=dhcp

    Root password
    rootpw --plaintext "to_be_disabled"

    System services
    services --enabled="sshd,waagent,NetworkManager"

    System timezone
    timezone Etc/UTC --isUtc --ntpservers 0.rhel.pool.ntp.org,1.rhel.pool.ntp.org,2.rhel.pool.ntp.org,3.rhel.pool.ntp.org

    Partition clearing information
    clearpart --all --initlabel

    Clear the MBR
    zerombr

    Disk partitioning information
    part /boot --fstype="xfs" --size=500
    part / --fstyp="xfs" --size=1 --grow --asprimary

    System bootloader configuration
    bootloader --location=mbr

    Firewall configuration
    firewall --disabled

    Enable SELinux
    selinux --enforcing

    Don't configure X
    skipx

    Power down the machine after install
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

    Register Red Hat Subscription
    subscription-manager register --username=XXX --password=XXX --auto-attach --force

    Install latest repo update
    yum update -y

    Stop and Uninstall cloud-init
    systemctl stop cloud-init
    yum remove cloud-init
    
    Enable extras repo
    subscription-manager repos --enable=rhel-7-server-extras-rpms

    Install WALinuxAgent
    yum install -y WALinuxAgent

    Unregister Red Hat subscription
    subscription-manager unregister

    Enable waaagent at boot-up
    systemctl enable waagent

    Disable the root account
    usermod root -p '!!'

    Configure swap in WALinuxAgent
    sed -i 's/^\(ResourceDisk\.EnableSwap\)=[Nn]$/\1=y/g' /etc/waagent.conf
    sed -i 's/^\(ResourceDisk\.SwapSizeMB\)=[0-9]*$/\1=2048/g' /etc/waagent.conf

    Set the cmdline
    sed -i 's/^\(GRUB_CMDLINE_LINUX\)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

    Enable SSH keepalive
    sed -i 's/^#\(ClientAliveInterval\).*$/\1 180/g' /etc/ssh/sshd_config

    Build the grub cfg
    grub2-mkconfig -o /boot/grub2/grub.cfg

    Configure network
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

    Deprovision and prepare for Azure
    waagent -force -deprovision

    %end
    ```

1. Yükleme Sistem, erişebileceğiniz kickstart dosyası yerleştirin.

1. Hyper-V Yöneticisi'nde yeni bir sanal makine oluşturun. Üzerinde **Sanal Sabit Disk Bağla** sayfasında **daha sonra bir sanal sabit disk ekleme**ve yeni sanal makine Sihirbazı tamamlayın.

1. Sanal makine ayarlarını açın:

    a. Yeni bir sanal sabit disk sanal makineye ekleyin. Seçtiğinizden emin olun **VHD biçimi** ve **sabit boyutlu**.

    b. ISO yükleme DVD sürücüsüne ekleyin.

    c. CD'den önyükleme BIOS'u ayarlayın.

1. Sanal makineyi başlatın. Yükleme Kılavuzu görüntülendiğinde basın **sekmesini** önyükleme seçeneklerini yapılandırmak için.

1. ENTER `inst.ks=<the location of the kickstart file>` önyükleme seçenekleri ve ENTER tuşuna sonunda **Enter**.

1. Yüklemenin tamamlanmasını bekleyin. Tamamlandığında, sanal makine otomatik olarak kapatılır. Linux VHD'nizi artık Azure'a yüklenmek hazırdır.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="the-hyper-v-driver-could-not-be-included-in-the-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>Hyper-V sürücü başlangıç RAM disk bir Hyper-V olmayan hiper kullanırken eklenemedi

Bir Hyper-V ortamında çalıştığından emin Linux algılamadığı sürece bazı durumlarda, Linux yükleyicileri sürücüleri Hyper-V için başlangıç RAM disk (initrd veya initramfs) içermeyebilir.

Farklı sanallaştırma sistemi (diğer bir deyişle, Virtualbox, Xen, vb.), Linux görüntüsünü hazırlamak için kullanırken, en az emin olmak için initrd yeniden oluşturmanız gerekebilir hv_vmbus ve hv_storvsc çekirdek modülleri başlangıç RAM disk üzerinde kullanılabilir. Bilinen bir sorun en az Yukarı Akış Red Hat dağıtım tabanlı sistemlerde budur.

Bu sorunu çözmek için initramfs Hyper-V modüllerini ekleyin ve yeniden oluşturmak için:

Düzen `/etc/dracut.conf`ve aşağıdaki içeriği ekleyin:

```sh
add_drivers+="hv_vmbus hv_netvsc hv_storvsc"
```

İnitramfs yeniden oluşturun:

```bash
dracut -f -v
```

Daha fazla bilgi için [initramfs yeniden](https://access.redhat.com/solutions/1958).

## <a name="next-steps"></a>Sonraki adımlar

Azure Stack'te yeni sanal makineler oluşturmak için Red Hat Enterprise Linux sanal sabit diski kullanmak artık hazırsınız. Bu Azure Stack için VHD dosyasının karşıya ilk kez kullanıyorsanız, bkz. [oluşturun ve bir Market öğesi yayımlama](azure-stack-create-and-publish-marketplace-item.md).

Red Hat Enterprise Linux'ı çalıştırmak için sertifikalı hiper hakkında daha fazla bilgi için bkz: [Red Hat Web sitesi](https://access.redhat.com/certified-hypervisors).
