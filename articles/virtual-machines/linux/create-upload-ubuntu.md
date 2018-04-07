---
title: Oluşturma ve Azure bir Ubuntu Linux VHD yükleme
description: Oluşturma ve bir Azure sanal sabit bir Ubuntu Linux işletim sistemini içeren disk (VHD) yükleme hakkında bilgi edinme.
services: virtual-machines-linux
documentationcenter: ''
author: szarkos
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 3e097959-84fc-4f6a-8cc8-35e087fd1542
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: szark
ms.openlocfilehash: f9a4588a444b5bfeb37f0bd98ada6a336baabb04
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a>Azure’da Ubuntu sanal makinesi hazırlama
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="official-ubuntu-cloud-images"></a>Resmi Ubuntu bulut görüntüleri
Ubuntu şimdi adresten resmi Azure VHD'ler yayımlar [ http://cloud-images.ubuntu.com/ ](http://cloud-images.ubuntu.com/). Azure için kendi özel Ubuntu görüntü oluşturmak gerekiyorsa, bunun yerine el ile aşağıdaki yordamı kullanın daha VHD'ler çalışma bilinen bunlarla başlatmak ve gerektiği şekilde özelleştirmek için önerilir. En son görüntü yayımları her zaman aşağıdaki konumlarda bulunabilir:

* Ubuntu 12.04/kesin: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](https://cloud-images.ubuntu.com/precise/current/precise-server-cloudimg-amd64-disk1.vhd.zip)
* Ubuntu 14.04/Trusty: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)
* Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, bir sanal sabit disk için bir Ubuntu Linux işletim sistemi zaten yüklediğinizi varsayar. Birden çok araç, .vhd dosyaları, örneğin bir Hyper-V gibi sanallaştırma çözümü oluşturmak için mevcut. Yönergeler için bkz: [Hyper-V rolünü yükleyin ve sanal makine yapılandırma](http://technet.microsoft.com/library/hh846766.aspx).

**Ubuntu yükleme notları**

* Ayrıca bkz [genel Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes) Linux Azure için hazırlama hakkında daha fazla ipucu için.
* VHDX biçimi, Azure'da yalnızca desteklenmiyor **VHD sabit**.  Disk Hyper-V Yöneticisi'ni veya convert-vhd cmdlet'ini kullanarak VHD biçimine dönüştürebilirsiniz.
* Linux sistemini yüklerken LVM (genellikle birçok yüklemeleri için varsayılan) yerine standart bölümlerini kullanmanız önerilir. Özellikle bir işletim sistemi diski şimdiye kadar başka bir VM için sorun giderme için eklenmesi gerekiyorsa, bu kopyalanan VMs LVM ad çakışmalarını önler. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veri disklerde tercih edilen varsa kullanılabilir.
* Bir takas bölüm işletim sistemi disk üzerinde yapılandırmayın. Linux Aracısı geçici kaynak disk üzerinde bir takas dosyası oluşturmak için yapılandırılabilir.  Aşağıdaki adımlarda bu hakkında daha fazla bilgi bulunabilir.
* Tüm VHD'leri Azure üzerinde bir sanal boyutu 1 MB ile hizalı olması gerekir. Ham diskten VHD'ye dönüştürülürken ham disk boyutu 1 MB dönüştürmeden önce birden fazla olduğundan emin olmanız gerekir. Bkz: [Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes) daha fazla bilgi için.

## <a name="manual-steps"></a>El ile adımlar
> [!NOTE]
> Azure için kendi özel Ubuntu görüntü oluşturmayı denemeden önce lütfen önceden oluşturulmuş ve test edilen görüntülerden kullanmayı [ http://cloud-images.ubuntu.com/ ](http://cloud-images.ubuntu.com/) yerine.
> 
> 

1. Hyper-V Yöneticisi'nin Orta bölmede sanal makineyi seçin.

2. Tıklatın **Bağlan** sanal makine için penceresini açın.

3. Ubuntu'nın Azure depoları kullanmak üzere görüntüsündeki geçerli depoları değiştirin. Adımlar, Ubuntu sürüm bağlı olarak biraz farklılık gösterir.
   
    Düzenleme önce `/etc/apt/sources.list`, yedekleme yapmak için önerilir:
   
        # sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    Ubuntu 12.04:
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 14.04:
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 16.04:
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

4. Ubuntu Azure görüntüleri şimdi aşağıdaki *donanım etkinleştirme* (HWE) çekirdek. İşletim sistemi, aşağıdaki komutları çalıştırarak son çekirdeğe güncelleştirin:

    Ubuntu 12.04:
   
        # sudo apt-get update
        # sudo apt-get install linux-image-generic-lts-trusty linux-cloud-tools-generic-lts-trusty
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade
   
        # sudo reboot
   
    Ubuntu 14.04:
   
        # sudo apt-get update
        # sudo apt-get install linux-image-virtual-lts-vivid linux-lts-vivid-tools-common
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade
   
        # sudo reboot

    Ubuntu 16.04:
   
        # sudo apt-get update
        # sudo apt-get install linux-generic-hwe-16.04 linux-cloud-tools-generic-hwe-16.04
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot

    **Ayrıca bkz.:**
    - [https://wiki.ubuntu.com/Kernel/LTSEnablementStack](https://wiki.ubuntu.com/Kernel/LTSEnablementStack)
    - [https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack](https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack)


5. Azure için ek çekirdek parametreleri içerecek şekilde kaz için çekirdek önyükleme satırı değiştirin. Bu açık yapmak için `/etc/default/grub` adlı değişken bir metin düzenleyicisinde Bul `GRUB_CMDLINE_LINUX_DEFAULT` (veya gerekirse ekleyin) ve aşağıdaki parametreleri içerecek şekilde düzenleyin:
   
        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    Kaydet ve bu dosyayı kapatın ve ardından çalıştırın `sudo update-grub`. Bu, tüm konsol iletileri sorunları ayıklama Azure teknik destek yardımcı olabilecek ilk seri bağlantı noktasına gönderilen güvence altına alır.

6. SSH sunucusu yüklü ve önyükleme sırasında başlatılacak şekilde yapılandırılmış olduğundan emin olun.  Bu genellikle varsayılan seçenektir.

7. Azure Linux Aracısı'nı yükleyin:
   
        # sudo apt-get update
        # sudo apt-get install walinuxagent

    >[!Note]
    `walinuxagent` Paketi kaldırmak `NetworkManager` ve `NetworkManager-gnome` yüklenmişlerse paketler.

8. Sanal makine yetkisini kaldırma ve Azure üzerinde sağlamak için hazırlamak için aşağıdaki komutları çalıştırın:
   
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

9. Tıklatın **eylem -> kapatma aşağı** Hyper-V Yöneticisi'nde. Linux VHD Azure'a karşıya yüklenecek artık hazırdır.

## <a name="next-steps"></a>Sonraki adımlar
Şimdi yeni sanal makineler oluşturmak için Ubuntu Linux sanal sabit diski kullanmak hazırsınız. Azure için .vhd dosyasını karşıya yüklüyoruz ilk kez kullanıyorsanız bkz [özel bir diskten bir Linux VM oluşturma](upload-vhd.md#option-1-upload-a-vhd).

## <a name="references"></a>Başvurular
Ubuntu donanım etkinleştirme (HWE) Çekirdek:

* [http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html](http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html)
* [http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html](http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html)

