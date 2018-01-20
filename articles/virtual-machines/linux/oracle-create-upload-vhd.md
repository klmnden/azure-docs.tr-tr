---
title: "Bir Oracle Linux VHD'yi oluşturup | Microsoft Docs"
description: "Oluşturma ve bir Azure sanal sabit bir Oracle Linux işletim sistemini içeren disk (VHD) yükleme hakkında bilgi edinme."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-service-management,azure-resource-manager
ms.assetid: dd96f771-26eb-4391-9a89-8c8b6d691822
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: szark
ms.openlocfilehash: 90df1546ddc70667f6c977afba8078b6dfb0e148
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Azure için Oracle Linux sanal makinesi hazırlama
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, bir sanal sabit disk için bir Oracle Linux işletim sistemi zaten yüklediğinizi varsayar. Birden çok araç, .vhd dosyaları, örneğin bir Hyper-V gibi sanallaştırma çözümü oluşturmak için mevcut. Yönergeler için bkz: [Hyper-V rolünü yükleyin ve sanal makine yapılandırma](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="oracle-linux-installation-notes"></a>Oracle Linux yükleme notları
* Ayrıca bkz [genel Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes) Linux Azure için hazırlama hakkında daha fazla ipucu için.
* Oracle'nın Red Hat uyumlu çekirdek ve bunların UEK3 (kesilemeyen kurumsal çekirdek) hem de Hyper-V ve Azure üzerinde desteklenir. En iyi sonuçlar için lütfen en son çekirdek, Oracle Linux VHD hazırlanırken güncelleştirdiğinizden emin olun.
* Gerekli sürücüleri içermez gibi oracle'nın UEK2 Hyper-V ve Azure üzerinde desteklenmiyor.
* VHDX biçimi, Azure'da yalnızca desteklenmiyor **VHD sabit**.  Disk Hyper-V Yöneticisi'ni veya convert-vhd cmdlet'ini kullanarak VHD biçimine dönüştürebilirsiniz.
* Linux sistemini yüklerken LVM (genellikle birçok yüklemeleri için varsayılan) yerine standart bölümlerini kullanmanız önerilir. Özellikle bir işletim sistemi diski şimdiye kadar başka bir VM için sorun giderme için eklenmesi gerekiyorsa, bu kopyalanan VMs LVM ad çakışmalarını önler. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veri disklerde tercih edilen varsa kullanılabilir.
* NUMA Linux çekirdek sürümleri 2.6.37 aşağıda bir hata nedeniyle daha büyük VM boyutları için desteklenmiyor. Bu sorun öncelikle Yukarı Akış kullanarak dağıtımları etkiler Red Hat 2.6.32 çekirdek. Azure Linux Aracısı'nı (waagent) el ile yükleme otomatik olarak NUMA Linux çekirdek kaz yapılandırmasında devre dışı bırakır. Aşağıdaki adımlarda bu hakkında daha fazla bilgi bulunabilir.
* Bir takas bölüm işletim sistemi disk üzerinde yapılandırmayın. Linux Aracısı geçici kaynak disk üzerinde bir takas dosyası oluşturmak için yapılandırılabilir.  Aşağıdaki adımlarda bu hakkında daha fazla bilgi bulunabilir.
* Tüm VHD'leri boyutları 1 MB'ün katları olmalıdır.
* Olduğundan emin olun `Addons` depo etkindir. Dosyayı düzenlemek `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) veya `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux), satır değiştirip `enabled=0` için `enabled=1` altında **[ol6_addons]** veya **[ol7_addons]** bu dosyadaki.

## <a name="oracle-linux-64"></a>Oracle Linux 6.4+
Azure'da çalışması sanal makine için işletim sistemini belirli yapılandırma adımları tamamlamanız gerekir.

1. Hyper-V Yöneticisi'nin Orta bölmede sanal makineyi seçin.
2. Tıklatın **Bağlan** sanal makine için penceresini açın.
3. Aşağıdaki komutu çalıştırarak NetworkManager kaldırın:
   
        # sudo rpm -e --nodeps NetworkManager
   
    **Not:** paket zaten yüklü değilse, bu komutu bir hata iletisiyle başarısız olur. Bu beklenen bir durumdur.
4. Adlı bir dosya oluşturun **ağ** içinde `/etc/sysconfig/` aşağıdaki metni içeren dizini:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
5. Adlı bir dosya oluşturun **ifcfg eth0** içinde `/etc/sysconfig/network-scripts/` aşağıdaki metni içeren dizini:
   
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
   
        # chkconfig network on
8. Aşağıdaki komutu çalıştırarak Python pyasn1 yükleyin:
   
        # sudo yum install python-pyasn1
9. Azure için ek çekirdek parametreleri içerecek şekilde kaz yapılandırma çekirdek önyükleme satırı değiştirin. Bu açık yapmak için "/ boot/grub/menu.lst" bir metin düzenleyicisinde ve varsayılan çekirdek aşağıdaki parametreleri içerdiğinden emin olun:
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off
   
   Bu tüm konsol iletileri hangi Azure yardımcı olabilecek ilk seri bağlantı noktasına gönderilen sağlayacak sorunları ayıklama desteği. Bu NUMA Oracle'nın Red Hat uyumlu Çekirdeği'nde bir hata nedeniyle devre dışı bırakır.
   
   Yukarıdakilerin yanı sıra için önerilir *kaldırmak* aşağıdaki parametreleri:
   
        rhgb quiet crashkernel=auto
   
   Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilecek tüm günlükleri burada istiyoruz bulut ortamında bulunan kullanışlı değildir.
   
   `crashkernel` Seçeneği olabilir sol isterseniz yapılandırılmış, ancak Not küçük VM boyutlarını sorunlu olabilecek bu parametre VM tarafından 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır.
10. SSH sunucusu yüklü ve önyükleme sırasında başlatılacak şekilde yapılandırılmış olduğundan emin olun.  Bu genellikle varsayılan seçenektir.
11. Aşağıdaki komutu çalıştırarak Azure Linux Aracısı'nı yükleyin. En son sürüm 2.0.15 ' dir.
    
        # sudo yum install WALinuxAgent
    
    WALinuxAgent paketi yükleniyor NetworkManager kaldırır ve bunlar zaten açıklandığı gibi kaldırılmamışsa, NetworkManager gnome paketleri 2. adımda not edin.
12. Takas alanı işletim sistemi disk üzerinde oluşturmayın.
    
    Azure Linux Aracısı'nı otomatik olarak takas alanı Azure üzerinde sağladıktan sonra VM'ye bağlı yerel kaynak diski kullanarak yapılandırabilirsiniz. Yerel kaynak disk Not bir *geçici* disk ve VM sağlaması kaldırılıyor. sağlaması zaman boşaltılabilir. Azure Linux Aracısı'nı yükledikten sonra (önceki adıma bakın), /etc/waagent.conf aşağıdaki parametrelerinde uygun şekilde değiştirin:
    
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

- - -
## <a name="oracle-linux-70"></a>Oracle Linux 7.0+
**Oracle Linux 7 değişiklikleri**

Bir Oracle Linux 7 sanal makine için Azure hazırlanıyor çok ancak eşitlenmeyeceği birkaç önemli farklılıklar vardır, Oracle Linux 6 benzer:

* Red Hat uyumlu çekirdek ve Oracle'nın UEK3 Azure üzerinde desteklenir.  UEK3 çekirdek önerilir.
* NetworkManager paket artık Azure Linux Aracısı ile çakışıyor. Bu paketi varsayılan olarak yüklenir ve onu kaldırılmaz öneririz.
* Çekirdek parametreleri düzenleme yordamı (aşağıya bakın) değişti biçimde GRUB2 şimdi varsayılan önyükleme yükleyicisi kullanılır.
* XFS varsayılan dosya sistemi sunulmuştur. Ext4 dosya sistemi hala isterseniz kullanılabilir.

**Yapılandırma adımları**

1. Hyper-V Yöneticisi'nde sanal makineyi seçin.
2. Tıklatın **Bağlan** sanal makine için bir konsol penceresi açın.
3. Adlı bir dosya oluşturun **ağ** içinde `/etc/sysconfig/` aşağıdaki metni içeren dizini:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
4. Adlı bir dosya oluşturun **ifcfg eth0** içinde `/etc/sysconfig/network-scripts/` aşağıdaki metni içeren dizini:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
5. Ethernet arabirimi için statik kuralları oluşturmamak için udev kuralları Değiştir. Bu kurallar, bir sanal makinede Microsoft Azure veya Hyper-V: kopyalarken sorunlara neden olabilir
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
6. Ağ hizmeti aşağıdaki komutu çalıştırarak önyükleme sırasında başlar emin olun:
   
        # sudo chkconfig network on
7. Aşağıdaki komutu çalıştırarak pyasn1 python paketini yükle:
   
        # sudo yum install python-pyasn1
8. Geçerli yum meta verileri temizlemek ve tüm güncelleştirmeleri yüklemek için aşağıdaki komutu çalıştırın:
   
        # sudo yum clean all
        # sudo yum -y update
9. Azure için ek çekirdek parametreleri içerecek şekilde kaz yapılandırma çekirdek önyükleme satırı değiştirin. Bunu yapmak için Aç "/ etc/varsayılan/kaz" bir metin Düzenleyicisi'ni ve düzenleme `GRUB_CMDLINE_LINUX` parametresi, örneğin:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Bu tüm konsol iletileri hangi Azure yardımcı olabilecek ilk seri bağlantı noktasına gönderilen sağlayacak sorunları ayıklama desteği. Aynı zamanda NIC için yeni OEL 7 adlandırma kuralları devre dışı bırakır. Yukarıdakilerin yanı sıra için önerilir *kaldırmak* aşağıdaki parametreleri:
   
       rhgb quiet crashkernel=auto
   
   Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilecek tüm günlükleri burada istiyoruz bulut ortamında bulunan kullanışlı değildir.
   
   `crashkernel` Seçeneği olabilir sol isterseniz yapılandırılmış, ancak Not küçük VM boyutlarını sorunlu olabilecek bu parametre VM tarafından 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır.
10. Tamamladıktan sonra düzenleme "/ etc/varsayılan/kaz" başına, yukarıda kaz yapılandırma yeniden oluşturmak için aşağıdaki komutu çalıştırın:
    
        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg
11. SSH sunucusu yüklü ve önyükleme sırasında başlatılacak şekilde yapılandırılmış olduğundan emin olun.  Bu genellikle varsayılan seçenektir.
12. Aşağıdaki komutu çalıştırarak Azure Linux Aracısı'nı yükleyin:
    
        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent
13. Takas alanı işletim sistemi disk üzerinde oluşturmayın.
    
    Azure Linux Aracısı'nı otomatik olarak takas alanı Azure üzerinde sağladıktan sonra VM'ye bağlı yerel kaynak diski kullanarak yapılandırabilirsiniz. Yerel kaynak disk Not bir *geçici* disk ve VM sağlaması kaldırılıyor. sağlaması zaman boşaltılabilir. Azure Linux Aracısı'nı yükledikten sonra (önceki adıma bakın), /etc/waagent.conf aşağıdaki parametrelerinde uygun şekilde değiştirin:
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.
14. Sanal makine yetkisini kaldırma ve Azure üzerinde sağlamak için hazırlamak için aşağıdaki komutları çalıştırın:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
15. Tıklatın **eylem -> kapatma aşağı** Hyper-V Yöneticisi'nde. Linux VHD Azure'a karşıya yüklenecek artık hazırdır.

## <a name="next-steps"></a>Sonraki adımlar
Şimdi yeni sanal makineler oluşturmak için Oracle Linux .vhd kullanmaya hazırsınız. Adım 2 ve 3'te Azure'a .vhd dosyasını karşıya yüklüyoruz ilk kez kullanıyorsanız bkz [oluşturma ve Linux işletim sistemini içeren bir sanal sabit disk karşıya](classic/create-upload-vhd-classic.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

