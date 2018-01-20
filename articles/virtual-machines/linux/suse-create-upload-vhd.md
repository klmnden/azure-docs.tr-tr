---
title: "Oluşturma ve Azure SUSE Linux VHD'yi yükleme"
description: "Oluşturma ve bir Azure sanal sabit SUSE Linux işletim sistemi içeren disk (VHD) yükleme hakkında bilgi edinme."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 066d01a6-2a54-4718-bcd0-90fe7a5303a1
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: szark
ms.openlocfilehash: e90e928bed41abc00ad4c5b8dcdfad35cb3cbbdc
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="prepare-a-sles-or-opensuse-virtual-machine-for-azure"></a>Azure için SLES veya openSUSE sanal makinesi hazırlama
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, zaten bir SUSE veya openSUSE Linux işletim sistemi sanal sabit diske yüklediğinizi varsayar. Birden çok araç, .vhd dosyaları, örneğin bir Hyper-V gibi sanallaştırma çözümü oluşturmak için mevcut. Yönergeler için bkz: [Hyper-V rolünü yükleyin ve sanal makine yapılandırma](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="sles--opensuse-installation-notes"></a>SLES / openSUSE yükleme notları
* Ayrıca bkz [genel Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes) Linux Azure için hazırlama hakkında daha fazla ipucu için.
* VHDX biçimi, Azure'da yalnızca desteklenmiyor **VHD sabit**.  Disk Hyper-V Yöneticisi'ni veya convert-vhd cmdlet'ini kullanarak VHD biçimine dönüştürebilirsiniz.
* Linux sistemini yüklerken LVM (genellikle birçok yüklemeleri için varsayılan) yerine standart bölümlerini kullanmanız önerilir. Özellikle bir işletim sistemi diski şimdiye kadar başka bir VM için sorun giderme için eklenmesi gerekiyorsa, bu kopyalanan VMs LVM ad çakışmalarını önler. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veri disklerde tercih edilen varsa kullanılabilir.
* Bir takas bölüm işletim sistemi disk üzerinde yapılandırmayın. Linux Aracısı geçici kaynak disk üzerinde bir takas dosyası oluşturmak için yapılandırılabilir.  Aşağıdaki adımlarda bu hakkında daha fazla bilgi bulunabilir.
* Tüm VHD'leri boyutları 1 MB'ün katları olmalıdır.

## <a name="use-suse-studio"></a>SUSE Studio'yu kullanın
[SUSE Studio](http://www.susestudio.com) kolayca oluşturabilir ve SLES ve openSUSE görüntülerinizi Azure ve Hyper-V için yönetebilirsiniz. Bu, kendi SLES ve openSUSE görüntüleri özelleştirmek için önerilen yaklaşımdır.

Kendi VHD oluşturma alternatif olarak, SUSE de BYOS (Getir bilgisayarınızı kendi abonelik) görüntüleri SLES için yayımlar [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007).

## <a name="prepare-suse-linux-enterprise-server-11-sp4"></a>Prepare SUSE Linux Enterprise Server 11 SP4
1. Hyper-V Yöneticisi'nin Orta bölmede sanal makineyi seçin.
2. Tıklatın **Bağlan** sanal makine için penceresini açın.
3. Güncelleştirmeleri Yükle ve paketleri izin vermek, SUSE Linux Enterprise sisteminizi kaydedin.
4. En son düzeltme eklerini sistemiyle güncelleştirmesi:
   
        # sudo zypper update
5. SLES depodan Azure Linux Aracısı'nı yükleyin:
   
        # sudo zypper install WALinuxAgent
6. İçinde chkconfig waagent "açık" ayarlanmış olup olmadığını denetleyin ve aksi durumda, için AutoStart'ı etkinleştirin:
   
        # sudo chkconfig waagent on
7. Waagent hizmeti çalıştıran ve aksi durumda, başlatın kontrol edin: 
   
        # sudo service waagent start
8. Azure için ek çekirdek parametreleri içerecek şekilde kaz yapılandırma çekirdek önyükleme satırı değiştirin. Bu açık yapmak için "/ boot/grub/menu.lst" bir metin düzenleyicisinde ve varsayılan çekirdek aşağıdaki parametreleri içerdiğinden emin olun:
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
   
    Bu tüm konsol iletileri hangi Azure yardımcı olabilecek ilk seri bağlantı noktasına gönderilen sağlayacak sorunları ayıklama desteği.
9. /Boot/Grub/Menu.lst ve /etc/fstab (kimliğine göre) disk kimliği yerine kendi UUID'si (UUID tarafından) kullanılarak disk başvuru onaylayın. 
   
    Get disk UUID
   
        # ls /dev/disk/by-uuid/
   
    Varsa /dev/disk/by-id / kullanılan, /boot/grub/menu.lst hem/etc/fstab UUID tarafından uygun değeri ile güncelleştirme
   
    Değişiklikten önce
   
        root=/dev/disk/by-id/SCSI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx-part1
   
    Değişiklikten sonra
   
        root=/dev/disk/by-uuid/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
10. Ethernet arabirimi için statik kuralları oluşturmamak için udev kuralları Değiştir. Bu kurallar, bir sanal makinede Microsoft Azure veya Hyper-V: kopyalarken sorunlara neden olabilir
    
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
11. Dosyayı düzenlemek için önerilen "/ etc/sysconfig/ağ/dhcp" ve değiştirme `DHCLIENT_SET_HOSTNAME` aşağıdaki parametre:
    
     DHCLIENT_SET_HOSTNAME="no"
12. "/ Etc/sudoers", çıkışı yorum yapmak veya varsa aşağıdaki satırları Kaldır:
    
     Varsayılan olarak targetpw # isteyin yani tüm ALL=(ALL) tüm kök hedef kullanıcının parolasını # uyarı! Yalnızca bu 'Varsayılanları targetpw' ile birlikte kullanın!
13. SSH sunucusu yüklü ve önyükleme sırasında başlatılacak şekilde yapılandırılmış olduğundan emin olun.  Bu genellikle varsayılan seçenektir.
14. Takas alanı işletim sistemi disk üzerinde oluşturmayın.
    
    Azure Linux Aracısı'nı otomatik olarak takas alanı Azure üzerinde sağladıktan sonra VM'ye bağlı yerel kaynak diski kullanarak yapılandırabilirsiniz. Yerel kaynak disk Not bir *geçici* disk ve VM sağlaması kaldırılıyor. sağlaması zaman boşaltılabilir. Azure Linux Aracısı'nı yükledikten sonra (önceki adıma bakın), /etc/waagent.conf aşağıdaki parametrelerinde uygun şekilde değiştirin:
    
     ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=&#2048;# Not: Bu ayar ne olursa olsun, olması gerekir.
15. Sanal makine yetkisini kaldırma ve Azure üzerinde sağlamak için hazırlamak için aşağıdaki komutları çalıştırın:
    
    # <a name="sudo-waagent--force--deprovision"></a>sudo waagent-force - deprovision
    # <a name="export-histsize0"></a>HISTSIZE ver = 0
    # <a name="logout"></a>oturumu kapat
16. Tıklatın **eylem -> kapatma aşağı** Hyper-V Yöneticisi'nde. Linux VHD Azure'a karşıya yüklenecek artık hazırdır.

- - -
## <a name="prepare-opensuse-131"></a>OpenSUSE 13,1 + hazırlama
1. Hyper-V Yöneticisi'nin Orta bölmede sanal makineyi seçin.
2. Tıklatın **Bağlan** sanal makine için penceresini açın.
3. Komut kabuğu, Çalıştır '`zypper lr`'. Bu komut çıktı aşağıdakine benzer döndürür sonra depoları beklenen--ayarlama yapılmayan gerekli olarak yapılandırılan (sürüm numaraları değişebileceğini unutmayın):
   
        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes
   
    Ardından komut "depoları tanımlanan..." döndürürse bu depoları eklemek için aşağıdaki komutları kullanın:
   
        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1
        # sudo zypper ar -f http://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates
   
    Ardından depoları eklenmiş komutunu çalıştırarak doğrulayabilirsiniz '`zypper lr`' yeniden. İlgili güncelleştirme depoları birini etkin durumda aşağıdaki komutla etkinleştirin:
   
        # sudo zypper mr -e [NUMBER OF REPOSITORY]
4. Çekirdek kullanılabilir en son sürüme güncelleştirin:
   
        # sudo zypper up kernel-default
   
    Veya sistem tüm en son düzeltme eki ile güncelleştirmek için:
   
        # sudo zypper update
5. Azure Linux Aracısı'nı yükleyin.
   
   # <a name="sudo-zypper-install-walinuxagent"></a>sudo zypper yükleme WALinuxAgent
6. Azure için ek çekirdek parametreleri içerecek şekilde kaz yapılandırma çekirdek önyükleme satırı değiştirin. Bunu yapmak için Aç "/ boot/grub/menu.lst" bir metin düzenleyicisinde ve varsayılan çekirdek aşağıdaki parametreleri içerdiğinden emin olun:
   
     console=ttyS0 earlyprintk=ttyS0 rootdelay=300
   
   Bu tüm konsol iletileri hangi Azure yardımcı olabilecek ilk seri bağlantı noktasına gönderilen sağlayacak sorunları ayıklama desteği. Ayrıca, varsa aşağıdaki parametreleri çekirdek önyükleme satırından kaldırın:
   
     libata.atapi_enabled=0 ayırma 0x1f0, = 0x8
7. Dosyayı düzenlemek için önerilen "/ etc/sysconfig/ağ/dhcp" ve değiştirme `DHCLIENT_SET_HOSTNAME` aşağıdaki parametre:
   
     DHCLIENT_SET_HOSTNAME="no"
8. **Önemli:** "/ etc/sudoers", çıkışı açıklama veya varsa aşağıdaki satırları Kaldır:
   
     Varsayılan olarak targetpw # isteyin yani tüm ALL=(ALL) tüm kök hedef kullanıcının parolasını # uyarı! Yalnızca bu 'Varsayılanları targetpw' ile birlikte kullanın!
9. SSH sunucusu yüklü ve önyükleme sırasında başlatılacak şekilde yapılandırılmış olduğundan emin olun.  Bu genellikle varsayılan seçenektir.
10. Takas alanı işletim sistemi disk üzerinde oluşturmayın.
    
    Azure Linux Aracısı'nı otomatik olarak takas alanı Azure üzerinde sağladıktan sonra VM'ye bağlı yerel kaynak diski kullanarak yapılandırabilirsiniz. Yerel kaynak disk Not bir *geçici* disk ve VM sağlaması kaldırılıyor. sağlaması zaman boşaltılabilir. Azure Linux Aracısı'nı yükledikten sonra (önceki adıma bakın), /etc/waagent.conf aşağıdaki parametrelerinde uygun şekilde değiştirin:
    
     ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=&#2048;# Not: Bu ayar ne olursa olsun, olması gerekir.
11. Sanal makine yetkisini kaldırma ve Azure üzerinde sağlamak için hazırlamak için aşağıdaki komutları çalıştırın:
    
    # <a name="sudo-waagent--force--deprovision"></a>sudo waagent-force - deprovision
    # <a name="export-histsize0"></a>HISTSIZE ver = 0
    # <a name="logout"></a>oturumu kapat
12. Başlangıçta Azure Linux Aracısı'nı çalıştıran emin olun:
    
        # sudo systemctl enable waagent.service
13. Tıklatın **eylem -> kapatma aşağı** Hyper-V Yöneticisi'nde. Linux VHD Azure'a karşıya yüklenecek artık hazırdır.

## <a name="next-steps"></a>Sonraki adımlar
Şimdi yeni sanal makineler oluşturmak için SUSE Linux sanal sabit diski kullanmak hazırsınız. Adım 2 ve 3'te Azure'a .vhd dosyasını karşıya yüklüyoruz ilk kez kullanıyorsanız bkz [oluşturma ve Linux işletim sistemini içeren bir sanal sabit disk karşıya](classic/create-upload-vhd-classic.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

