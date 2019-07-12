---
title: Azure'da bir SUSE Linux VHD'si oluşturma ve yükleme
description: Oluşturma ve bir Azure sanal SUSE Linux işletim sistemi içeren sabit disk (VHD) yükleme öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: szarkos
manager: gwallace
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 066d01a6-2a54-4718-bcd0-90fe7a5303a1
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: szark
ms.openlocfilehash: 05731acd5e808075145c50281063e8990129d882
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67708680"
---
# <a name="prepare-a-sles-or-opensuse-virtual-machine-for-azure"></a>Azure için SLES veya openSUSE sanal makinesi hazırlama
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, zaten bir SUSE veya openSUSE Linux işletim sistemi sanal sabit diske yüklemiş olduğunuz varsayılır. Birden çok araç, .vhd dosyaları, örneğin bir Hyper-V gibi sanallaştırma çözümü oluşturmak için mevcut. Yönergeler için [Hyper-V rolünü yükleme ve sanal makine yapılandırma](https://technet.microsoft.com/library/hh846766.aspx).

### <a name="sles--opensuse-installation-notes"></a>SLES / openSUSE yükleme notları
* Ayrıca bkz [genel Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes) Linux için Azure hazırlama hakkında daha fazla ipucu için.
* VHDX biçimi, Azure'da yalnızca desteklenmiyor **VHD'yi sabit**.  Disk Hyper-V Yöneticisi'ni veya convert-vhd cmdlet'ini kullanarak VHD biçimine dönüştürebilirsiniz.
* Linux sistemini yüklerken LVM (genellikle birçok yüklemeleri için varsayılan) yerine standart bölümlerini kullanmanız önerilir. Özellikle bir işletim sistemi diski hiç sorun giderme için başka bir VM'ye bağlı gerekiyorsa, bu kopyalanan sanal makineler ile ad çakışmalarını LVM uğraşmasına gerek kalmaz. [LVM'yi](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veri disklerinde, tercih etmeleri durumunda kullanılıyor olabilir.
* İşletim sistemi diski üzerinde takas bölümü yapılandırmayın. Linux Aracısı, geçici kaynak diski üzerinde takas dosyası oluşturmak için yapılandırılabilir.  Aşağıdaki adımlarda bunu hakkında daha fazla bilgi bulunabilir.
* Tüm VHD'leri azure'da bir sanal Boyut 1 MB ile uyumlu olması gerekir. Ham bir diskten VHD'ye dönüştürme yaparken ham disk boyutu 1 MB dönüştürmeden önce bir çok olduğundan emin olmalısınız. Bkz: [Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes) daha fazla bilgi için.

## <a name="use-suse-studio"></a>SUSE Studio'yu kullanın.
[SUSE Studio](http://www.susestudio.com) kolayca oluşturabilir ve Azure ve Hyper-V için SLES ve openSUSE görüntülerinizi yönetme. Bu, kendi SLES ve openSUSE görüntüleri özelleştirmeye yönelik önerilen yaklaşımdır.

Kendi VHD'nizi oluşturmaya alternatif olarak, SUSE de BYOS (Getir kendi aboneliğiniz) görüntüleri için SLES yayımlar [Vmdepot'daki](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/04/using-and-contributing-vms-to-vm-depot.pdf).

## <a name="prepare-suse-linux-enterprise-server-11-sp4"></a>SUSE Linux Enterprise Server 11 SP4 hazırlama
1. Hyper-V Yöneticisi'nin Orta bölmede sanal makineyi seçin.
2. Tıklayın **Connect** sanal makine için penceresini açın.
3. Güncelleştirmeleri Yükle ve paketleri izin verecek şekilde SUSE Linux Enterprise sisteminize kaydedin.
4. Sistem, en son yamalarla güncelleştirin:
   
        # sudo zypper update
5. Azure Linux Aracısı SLES depodan yükleyin:
   
        # sudo zypper install python-azure-agent
6. İçinde chkconfig waagent "açık" ayarlanmış olup olmadığını denetleyin ve aksi takdirde, için AutoStart'ı etkinleştirin:
   
        # sudo chkconfig waagent on
7. Waagent hizmetin çalıştığından ve aksi takdirde, başlatmak kontrol edin: 
   
        # sudo service waagent start
8. Kernel önyükleme satırına grub yapılandırmanızda ek çekirdek parametreleri için Azure içerecek şekilde değiştirin. Bu açık yapmak için "/ boot/grub/menu.lst" bir metin düzenleyicisinde ve varsayılan çekirdek aşağıdaki parametreleri içerir:
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
   
    Bu, Azure size yardımcı olabilir ilk seri bağlantı noktası için tüm konsol iletileri gönderilir sağlayacak sorunları hata ayıklama desteği.
9. /Boot/Grub/Menu.lst ve /etc/fstab (kimliği ile) disk kimliği yerine kendi UUID (UUID tarafından) kullanarak diski başvuru onaylayın. 
   
    Disk UUID Al
   
        # ls /dev/disk/by-uuid/
   
    Varsa /dev/disk/by-id / kullanılan /boot/grub/menu.lst hem/etc/fstab UUID tarafından uygun değeri ile güncelleştirme
   
    Değişiklikten önce
   
        root=/dev/disk/by-id/SCSI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx-part1
   
    Değişiklikten sonra
   
        root=/dev/disk/by-uuid/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
10. Ethernet arabirimleri için statik kuralları oluşturmaktan kaçınmak için udev kurallarını değiştirin. Bu kurallar, bir sanal makinede Microsoft Azure veya Hyper-V: kopyalarken sorunlara neden olabilir
    
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
11. Dosyayı düzenlemek için önerilen "/ etc/sysconfig/ağ/dhcp" olduğundan ve değiştirmek `DHCLIENT_SET_HOSTNAME` aşağıdaki parametre:
    
     DHCLIENT_SET_HOSTNAME="no"
12. "/ Etc/sudoers", açıklama satırı yapın veya varsa, aşağıdaki satırları Kaldır:
    
     Varsayılan olarak targetpw # hedef kullanıcının yani tüm ALL=(ALL) tüm kök parolası isteyin # uyarı! Yalnızca 'İle birlikte varsayılan targetpw' kullanın!
13. SSH sunucusu yüklü ve önyükleme sırasında başlatılacak şekilde yapılandırılmış emin olun.  Bu, genellikle varsayılan değerdir.
14. İşletim sistemi diski üzerinde takas alanı oluşturabilirsiniz.
    
    Azure Linux Aracısı, Azure üzerinde sağladıktan sonra VM'ye bağlı yerel kaynak disk kullanılan takas alanı otomatik olarak yapılandırabilirsiniz. Yerel kaynak olduğuna dikkat edin bir *geçici* disk ve sanal Makinenin sağlaması kaldırıldığında boşaltılabilir. Azure Linux Aracısı'nı yükledikten sonra (önceki adıma bakın), aşağıdaki parametrelerle /etc/waagent.conf uygun şekilde değiştirin:
    
     ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=2048 ## Not: Bu her şeyi olması için gereken ayarlar.
15. Sanal makinenin sağlamasını kaldırma ve Azure'da sağlama için hazırlamak için aşağıdaki komutları çalıştırın:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
16. Tıklayın **eylem -> kapatma aşağı** Hyper-V Yöneticisi'nde. Linux VHD'nizi artık Azure'a yüklenmek hazırdır.

---
## <a name="prepare-opensuse-131"></a>OpenSUSE 13.1 + hazırlama
1. Hyper-V Yöneticisi'nin Orta bölmede sanal makineyi seçin.
2. Tıklayın **Connect** sanal makine için penceresini açın.
3. Komut kabuğu, çalıştırma '`zypper lr`'. Bu komut aşağıdakine benzer çıkış döndürür sonra depoları beklenen--hiçbir düzeltme gerekli olarak yapılandırılır (sürüm numaraları değişebileceğini unutmayın):
   
        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes
   
    Ardından komut "depo tanımlanan..." döndürürse bu depolara eklemek için aşağıdaki komutları kullanın:
   
        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1
        # sudo zypper ar -f https://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates
   
    Komutunu çalıştırarak depoları eklenmiştir ardından doğrulayabilirsiniz '`zypper lr`' yeniden. İlgili güncelleştirme depoları biri etkin değil durumunda, şu komutla etkinleştir:
   
        # sudo zypper mr -e [NUMBER OF REPOSITORY]
4. Çekirdek kullanılabilir en son sürüme güncelleştirin:
   
        # sudo zypper up kernel-default
   
    Veya en son yamalarla sistemi güncelleştirmek için:
   
        # sudo zypper update
5. Azure Linux aracısını yükleyin.
   
        # sudo zypper install WALinuxAgent
6. Kernel önyükleme satırına grub yapılandırmanızda ek çekirdek parametreleri için Azure içerecek şekilde değiştirin. Bunu yapmak için açık "/ boot/grub/menu.lst" bir metin düzenleyicisinde ve varsayılan çekirdek aşağıdaki parametreleri içerir:
   
     Konsol ttyS0 earlyprintk = ttyS0 rootdelay = 300 =
   
   Bu, Azure size yardımcı olabilir ilk seri bağlantı noktası için tüm konsol iletileri gönderilir sağlayacak sorunları hata ayıklama desteği. Ayrıca, aşağıdaki parametreleri kernel önyükleme satırına kaldırın, varsa:
   
     libata.atapi_enabled=0 ayırma 0x1f0, = 0x8
7. Dosyayı düzenlemek için önerilen "/ etc/sysconfig/ağ/dhcp" olduğundan ve değiştirmek `DHCLIENT_SET_HOSTNAME` aşağıdaki parametre:
   
     DHCLIENT_SET_HOSTNAME="no"
8. **Önemli:** "/ Etc/sudoers", açıklama satırı yapın veya varsa, aşağıdaki satırları Kaldır:
   
     Varsayılan olarak targetpw # hedef kullanıcının yani tüm ALL=(ALL) tüm kök parolası isteyin # uyarı! Yalnızca 'İle birlikte varsayılan targetpw' kullanın!
9. SSH sunucusu yüklü ve önyükleme sırasında başlatılacak şekilde yapılandırılmış emin olun.  Bu, genellikle varsayılan değerdir.
10. İşletim sistemi diski üzerinde takas alanı oluşturabilirsiniz.
    
    Azure Linux Aracısı, Azure üzerinde sağladıktan sonra VM'ye bağlı yerel kaynak disk kullanılan takas alanı otomatik olarak yapılandırabilirsiniz. Yerel kaynak olduğuna dikkat edin bir *geçici* disk ve sanal Makinenin sağlaması kaldırıldığında boşaltılabilir. Azure Linux Aracısı'nı yükledikten sonra (önceki adıma bakın), aşağıdaki parametrelerle /etc/waagent.conf uygun şekilde değiştirin:
    
     ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=2048 ## Not: Bu her şeyi olması için gereken ayarlar.
11. Sanal makinenin sağlamasını kaldırma ve Azure'da sağlama için hazırlamak için aşağıdaki komutları çalıştırın:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
12. Azure Linux Aracısı başlangıçta çalıştığından emin olun:
    
        # sudo systemctl enable waagent.service
13. Tıklayın **eylem -> kapatma aşağı** Hyper-V Yöneticisi'nde. Linux VHD'nizi artık Azure'a yüklenmek hazırdır.

## <a name="next-steps"></a>Sonraki adımlar
Artık Azure'da yeni sanal makineler oluşturmak için SUSE Linux sanal sabit diski kullanmaya hazırsınız. Bu Azure'a .vhd dosyasını karşıya ilk kez kullanıyorsanız, bkz. [bir özel diskten Linux VM oluşturma](upload-vhd.md#option-1-upload-a-vhd).
