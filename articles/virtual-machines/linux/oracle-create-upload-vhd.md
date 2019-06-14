---
title: Bir Oracle Linux VHD'si oluşturma ve yükleme | Microsoft Docs
description: Oluşturma ve bir Azure sanal bir Oracle Linux işletim sistemini içeren sabit disk (VHD) yükleme öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: szarkos
manager: jeconnoc
editor: tysonn
tags: azure-service-management,azure-resource-manager
ms.assetid: dd96f771-26eb-4391-9a89-8c8b6d691822
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: szark
ms.openlocfilehash: ecd30d30434d91893102ce6ec0df21daa84b677c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60542416"
---
# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Azure için Oracle Linux sanal makinesi hazırlama
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, bir sanal sabit disk için Oracle Linux işletim sistemi zaten yüklediğinizi varsayar. Birden çok araç, .vhd dosyaları, örneğin bir Hyper-V gibi sanallaştırma çözümü oluşturmak için mevcut. Yönergeler için [Hyper-V rolünü yükleme ve sanal makine yapılandırma](https://technet.microsoft.com/library/hh846766.aspx).

### <a name="oracle-linux-installation-notes"></a>Oracle Linux yükleme notları
* Ayrıca bkz [genel Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes) Linux için Azure hazırlama hakkında daha fazla ipucu için.
* Oracle'nın Red Hat uyumlu çekirdek ve bunların UEK3 (kesilemeyen Enterprise çekirdeği) hem de Hyper-V ve Azure üzerinde desteklenir. En iyi sonuçlar için lütfen Oracle Linux VHD'nizi hazırlanırken son çekirdeğe güncelleştirdiğinizden emin olun.
* Gerekli sürücüleri içermez gibi Hyper-V ve Azure'da oracle'nın UEK2 desteklenmiyor.
* VHDX biçimi, Azure'da yalnızca desteklenmiyor **VHD'yi sabit**.  Disk Hyper-V Yöneticisi'ni veya convert-vhd cmdlet'ini kullanarak VHD biçimine dönüştürebilirsiniz.
* Linux sistemini yüklerken LVM (genellikle birçok yüklemeleri için varsayılan) yerine standart bölümlerini kullanmanız önerilir. Özellikle bir işletim sistemi diski hiç sorun giderme için başka bir VM'ye bağlı gerekiyorsa, bu kopyalanan sanal makineler ile ad çakışmalarını LVM uğraşmasına gerek kalmaz. [LVM'yi](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veri disklerinde, tercih etmeleri durumunda kullanılıyor olabilir.
* NUMA'yı Linux çekirdeği sürümlerinde 2.6.37 aşağıdaki hata nedeniyle daha büyük VM boyutları için desteklenmiyor. Bu sorun öncelikle Yukarı Akış kullanan dağıtımlar etkiler Red Hat 2.6.32 çekirdek. Azure Linux aracısını (waagent) el ile yüklenmesini otomatik olarak NUMA Linux çekirdeğinin GRUB yapılandırmasında devre dışı bırakır. Aşağıdaki adımlarda bunu hakkında daha fazla bilgi bulunabilir.
* İşletim sistemi diski üzerinde takas bölümü yapılandırmayın. Linux Aracısı, geçici kaynak diski üzerinde takas dosyası oluşturmak için yapılandırılabilir.  Aşağıdaki adımlarda bunu hakkında daha fazla bilgi bulunabilir.
* Tüm VHD'leri azure'da bir sanal Boyut 1 MB ile uyumlu olması gerekir. Ham bir diskten VHD'ye dönüştürme yaparken ham disk boyutu 1 MB dönüştürmeden önce bir çok olduğundan emin olmalısınız. Bkz: [Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes) daha fazla bilgi için.
* Emin olun `Addons` depo etkinleştirilir. Dosyayı düzenlemek `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) veya `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux 7) satır değiştirip `enabled=0` için `enabled=1` altında **[ol6_addons]** veya **[ol7_addons]** bu dosyadaki.

## <a name="oracle-linux-64"></a>Oracle Linux 6.4+
Azure'da çalıştırmak sanal makine için işletim sistemi belirli yapılandırma adımları tamamlamanız gerekir.

1. Hyper-V Yöneticisi'nin Orta bölmede sanal makineyi seçin.
2. Tıklayın **Connect** sanal makine için penceresini açın.
3. Aşağıdaki komutu çalıştırarak NetworkManager kaldırın:
   
        # sudo rpm -e --nodeps NetworkManager
   
    **Not:** Bu komut, paket zaten yüklü değilse, bir hata iletisiyle başarısız olur. Bu beklenen bir durumdur.
4. Adlı bir dosya oluşturun **ağ** içinde `/etc/sysconfig/` aşağıdaki metni içeren dizini:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
5. Adlı bir dosya oluşturun **ifcfg-eth0** içinde `/etc/sysconfig/network-scripts/` aşağıdaki metni içeren dizini:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
6. Ethernet arabirimleri için statik kuralları oluşturmaktan kaçınmak için udev kurallarını değiştirin. Bu kurallar, bir sanal makinede Microsoft Azure veya Hyper-V: kopyalarken sorunlara neden olabilir
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
7. Ağ hizmeti, aşağıdaki komutu çalıştırarak önyükleme sırasında başlar emin olun:
   
        # chkconfig network on
8. Python pyasn1, aşağıdaki komutu çalıştırarak yükleyin:
   
        # sudo yum install python-pyasn1
9. Kernel önyükleme satırına grub yapılandırmanızda ek çekirdek parametreleri için Azure içerecek şekilde değiştirin. Bu açık yapmak için "/ boot/grub/menu.lst" bir metin düzenleyicisinde ve varsayılan çekirdek aşağıdaki parametreleri içerir:
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off
   
   Bu, Azure size yardımcı olabilir ilk seri bağlantı noktası için tüm konsol iletileri gönderilir sağlayacak sorunları hata ayıklama desteği. Bu NUMA Oracle'nın Red Hat uyumlu çekirdek bir hata nedeniyle devre dışı bırakır.
   
   Yukarıdakine ek olarak, ancak önerilir *Kaldır* aşağıdaki parametreleri:
   
        rhgb quiet crashkernel=auto
   
   Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilmesi için tüm günlükleri istediğimiz bir bulut ortamında kullanışlı değildir.
   
   `crashkernel` Seçenek olabilir sol isterseniz yapılandırılmış, ancak Not daha küçük VM boyutlarına sorunlu olabilecek bu parametre VM'de 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır.
10. SSH sunucusu yüklü ve önyükleme sırasında başlatılacak şekilde yapılandırılmış emin olun.  Bu, genellikle varsayılan değerdir.
11. Azure Linux Aracısı, aşağıdaki komutu çalıştırarak yükleyin. En son sürüm 2.0.15 ' dir.
    
        # sudo yum install WALinuxAgent
    
    WALinuxAgent paket yükleme NetworkManager kaldırır ve bunlar zaten açıklandığı kaldırılmamışsa NetworkManager gnome paketleri 2. adımda not edin.
12. İşletim sistemi diski üzerinde takas alanı oluşturabilirsiniz.
    
    Azure Linux Aracısı, Azure üzerinde sağladıktan sonra VM'ye bağlı yerel kaynak disk kullanılan takas alanı otomatik olarak yapılandırabilirsiniz. Yerel kaynak olduğuna dikkat edin bir *geçici* disk ve sanal Makinenin sağlaması kaldırıldığında boşaltılabilir. Azure Linux Aracısı'nı yükledikten sonra (önceki adıma bakın), aşağıdaki parametrelerle /etc/waagent.conf uygun şekilde değiştirin:
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.
13. Sanal makinenin sağlamasını kaldırma ve Azure'da sağlama için hazırlamak için aşağıdaki komutları çalıştırın:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
14. Tıklayın **eylem -> kapatma aşağı** Hyper-V Yöneticisi'nde. Linux VHD'nizi artık Azure'a yüklenmek hazırdır.

- - -
## <a name="oracle-linux-70"></a>Oracle Linux 7.0+
**Oracle Linux 7 değişiklikleri**

Azure için Oracle Linux 7 sanal Makine'yi hazırlama çok benzeyen Oracle Linux 6, ancak önemli bazı önemli farklılıklar vardır:

* Azure'da Red Hat uyumlu çekirdek hem Oracle'nın UEK3 desteklenir.  UEK3 çekirdek önerilir.
* NetworkManager paket artık Azure Linux Aracısı ile çakışıyor. Bu paket, varsayılan olarak yüklenir ve bu kaldırılmaz önerilir.
* Çekirdek parametreleri düzenleme yordamı (aşağıya bakın) değiştirildi. Bu nedenle GRUB2 artık varsayılan önyükleme yükleyicisi kullanılır.
* XFS artık varsayılan dosya sistemidir. İsterseniz ext4 dosya sistemi hala kullanılabilir.

**Yapılandırma adımları**

1. Hyper-V Yöneticisi'nde, sanal makineyi seçin.
2. Tıklayın **Connect** sanal makine için bir konsol penceresi açın.
3. Adlı bir dosya oluşturun **ağ** içinde `/etc/sysconfig/` aşağıdaki metni içeren dizini:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
4. Adlı bir dosya oluşturun **ifcfg-eth0** içinde `/etc/sysconfig/network-scripts/` aşağıdaki metni içeren dizini:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
5. Ethernet arabirimleri için statik kuralları oluşturmaktan kaçınmak için udev kurallarını değiştirin. Bu kurallar, bir sanal makinede Microsoft Azure veya Hyper-V: kopyalarken sorunlara neden olabilir
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
6. Ağ hizmeti, aşağıdaki komutu çalıştırarak önyükleme sırasında başlar emin olun:
   
        # sudo chkconfig network on
7. Python pyasn1 paketi, aşağıdaki komutu çalıştırarak yükleyin:
   
        # sudo yum install python-pyasn1
8. Geçerli yum meta verileri temizlemek ve tüm güncelleştirmeleri yüklemek için aşağıdaki komutu çalıştırın:
   
        # sudo yum clean all
        # sudo yum -y update
9. Kernel önyükleme satırına grub yapılandırmanızda ek çekirdek parametreleri için Azure içerecek şekilde değiştirin. Bunu yapmak için açık "/ varsayılan/etc/grub" bir metin düzenleyicisi ve düzenleme `GRUB_CMDLINE_LINUX` parametresi, örneğin:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Bu, Azure size yardımcı olabilir ilk seri bağlantı noktası için tüm konsol iletileri gönderilir sağlayacak sorunları hata ayıklama desteği. NIC için de yeni OEL 7 adlandırma kurallarını devre dışı bırakır. Yukarıdakine ek olarak, ancak önerilir *Kaldır* aşağıdaki parametreleri:
   
       rhgb quiet crashkernel=auto
   
   Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilmesi için tüm günlükleri istediğimiz bir bulut ortamında kullanışlı değildir.
   
   `crashkernel` Seçenek olabilir sol isterseniz yapılandırılmış, ancak Not daha küçük VM boyutlarına sorunlu olabilecek bu parametre VM'de 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır.
10. İşiniz bittiğinde düzenleme "/ varsayılan/etc/grub" başına, yukarıda grub yapılandırması yeniden oluşturmak için aşağıdaki komutu çalıştırın:
    
        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg
11. SSH sunucusu yüklü ve önyükleme sırasında başlatılacak şekilde yapılandırılmış emin olun.  Bu, genellikle varsayılan değerdir.
12. Azure Linux Aracısı, aşağıdaki komutu çalıştırarak yükleyin:
    
        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent
13. İşletim sistemi diski üzerinde takas alanı oluşturabilirsiniz.
    
    Azure Linux Aracısı, Azure üzerinde sağladıktan sonra VM'ye bağlı yerel kaynak disk kullanılan takas alanı otomatik olarak yapılandırabilirsiniz. Yerel kaynak olduğuna dikkat edin bir *geçici* disk ve sanal Makinenin sağlaması kaldırıldığında boşaltılabilir. Azure Linux Aracısı'nı yükledikten sonra (önceki adıma bakın), aşağıdaki parametrelerle /etc/waagent.conf uygun şekilde değiştirin:
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.
14. Sanal makinenin sağlamasını kaldırma ve Azure'da sağlama için hazırlamak için aşağıdaki komutları çalıştırın:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
15. Tıklayın **eylem -> kapatma aşağı** Hyper-V Yöneticisi'nde. Linux VHD'nizi artık Azure'a yüklenmek hazırdır.

## <a name="next-steps"></a>Sonraki adımlar
Artık Azure'da yeni sanal makineler oluşturmak için Oracle Linux .vhd kullanmaya hazırsınız. Bu Azure'a .vhd dosyasını karşıya ilk kez kullanıyorsanız, bkz. [bir özel diskten Linux VM oluşturma](upload-vhd.md#option-1-upload-a-vhd).

