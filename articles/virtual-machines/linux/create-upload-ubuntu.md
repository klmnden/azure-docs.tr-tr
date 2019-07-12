---
title: Azure'da bir Ubuntu Linux VHD'si oluşturma ve yükleme
description: Oluşturma ve bir Azure sanal bir Ubuntu Linux işletim sistemini içeren sabit disk (VHD) yükleme öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: szarkos
manager: gwallace
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 3e097959-84fc-4f6a-8cc8-35e087fd1542
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/24/2019
ms.author: szark
ms.openlocfilehash: 50651a31cd407da3ce32be3c2ddbbd24e6ca6b69
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67671565"
---
# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a>Azure’da Ubuntu sanal makinesi hazırlama
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="official-ubuntu-cloud-images"></a>Resmi Ubuntu bulut görüntüleri
Ubuntu, artık yükleme için resmi Azure VHD'leri yayımlar [ https://cloud-images.ubuntu.com/ ](https://cloud-images.ubuntu.com/). Azure için kendi özel bir Ubuntu görüntüsünü oluşturmak ihtiyacınız varsa, yerine daha el ile aşağıdaki yordamı kullanın, bu VHD çalışma bilinen başlatıp gerektiği gibi özelleştirin önerilir. Son görüntü sürümleri her zaman aşağıdaki konumlarda bulunabilir:

* Ubuntu 12.04/kesin: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](https://cloud-images.ubuntu.com/precise/current/precise-server-cloudimg-amd64-disk1.vhd.zip)
* Ubuntu 14.04/Trusty: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](https://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)
* Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](https://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)
* Ubuntu 18.04/Bionic: [bionic-server-cloudimg-amd64.vhd.zip](https://cloud-images.ubuntu.com/bionic/current/bionic-server-cloudimg-amd64.vhd.zip)
* Ubuntu 18.10/Cosmic: [Kozmik-server-cloudimg-amd64.vhd.zip](https://cloud-images.ubuntu.com/cosmic/current/cosmic-server-cloudimg-amd64.vhd.zip)

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, bir sanal sabit diske bir Ubuntu Linux işletim sistemi zaten yüklediğinizi varsayar. Birden çok araç, .vhd dosyaları, örneğin bir Hyper-V gibi sanallaştırma çözümü oluşturmak için mevcut. Yönergeler için [Hyper-V rolünü yükleme ve sanal makine yapılandırma](https://technet.microsoft.com/library/hh846766.aspx).

**Ubuntu yükleme notları**

* Ayrıca bkz [genel Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes) Linux için Azure hazırlama hakkında daha fazla ipucu için.
* VHDX biçimi, Azure'da yalnızca desteklenmiyor **VHD'yi sabit**.  Disk Hyper-V Yöneticisi'ni veya convert-vhd cmdlet'ini kullanarak VHD biçimine dönüştürebilirsiniz.
* Linux sistemini yüklerken LVM (genellikle birçok yüklemeleri için varsayılan) yerine standart bölümlerini kullanmanız önerilir. Özellikle bir işletim sistemi diski hiç sorun giderme için başka bir VM'ye bağlı gerekiyorsa, bu kopyalanan sanal makineler ile ad çakışmalarını LVM uğraşmasına gerek kalmaz. [LVM'yi](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veri disklerinde, tercih etmeleri durumunda kullanılıyor olabilir.
* İşletim sistemi diski üzerinde takas bölümü yapılandırmayın. Linux Aracısı, geçici kaynak diski üzerinde takas dosyası oluşturmak için yapılandırılabilir.  Aşağıdaki adımlarda bunu hakkında daha fazla bilgi bulunabilir.
* Tüm VHD'leri azure'da bir sanal Boyut 1 MB ile uyumlu olması gerekir. Ham bir diskten VHD'ye dönüştürme yaparken ham disk boyutu 1 MB dönüştürmeden önce bir çok olduğundan emin olmalısınız. Bkz: [Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes) daha fazla bilgi için.

## <a name="manual-steps"></a>El ile yapılacak adımlar
> [!NOTE]
> Azure için kendi özel bir Ubuntu görüntüsünü oluşturmak denemeden önce lütfen önceden oluşturulmuş ve test edilen görüntülerden kullanmayı [ https://cloud-images.ubuntu.com/ ](https://cloud-images.ubuntu.com/) yerine.
> 
> 

1. Hyper-V Yöneticisi'nin Orta bölmede sanal makineyi seçin.

2. Tıklayın **Connect** sanal makine için penceresini açın.

3. Ubuntu'nın Azure depoları kullanmak için görüntünün geçerli depolarında değiştirin. Adımlar, Ubuntu sürümüne göre biraz farklılık gösterir.
   
    Düzenlemeden önce `/etc/apt/sources.list`, yedekleme yapmak için önerilir:
   
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

4. Ubuntu Azure görüntüleri artık aşağıdaki *donanım etkinleştirme* (HWE) çekirdek. İşletim sistemi, aşağıdaki komutları çalıştırarak en son çekirdeğe güncelleştirin:

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

    **Ayrıca bkz:**
    - [https://wiki.ubuntu.com/Kernel/LTSEnablementStack](https://wiki.ubuntu.com/Kernel/LTSEnablementStack)
    - [https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack](https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack)


5. Kernel önyükleme satırına için Grub ek çekirdek parametreleri için Azure içerecek şekilde değiştirin. Bu açık yapmak için `/etc/default/grub` adlı değişken bir metin Düzenleyicisi'nde bulun `GRUB_CMDLINE_LINUX_DEFAULT` (veya gerekirse ekleyin) ve aşağıdaki parametreleri içerecek şekilde düzenleyin:
   
        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    Kaydet ve bu dosyayı kapatın ve ardından çalıştırın `sudo update-grub`. Bu, hata ayıklaması ile Azure teknik desteğine yardımcı olabilecek ilk seri bağlantı noktasına gönderilen tüm konsol iletileri garanti eder.

6. SSH sunucusu yüklü ve önyükleme sırasında başlatılacak şekilde yapılandırılmış emin olun.  Bu, genellikle varsayılan değerdir.

7. Azure Linux Aracısı'nı yükleyin:
   
        # sudo apt-get update
        # sudo apt-get install walinuxagent

   > [!Note]
   >  `walinuxagent` Paketini kaldırmak `NetworkManager` ve `NetworkManager-gnome` yüklenmişlerse paketler.


1. Sanal makinenin sağlamasını kaldırma ve Azure'da sağlama için hazırlamak için aşağıdaki komutları çalıştırın:
   
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

1. Tıklayın **eylem -> kapatma aşağı** Hyper-V Yöneticisi'nde. Linux VHD'nizi artık Azure'a yüklenmek hazırdır.

## <a name="references"></a>Referanslar
[Ubuntu donanım etkinleştirme (HWE) çekirdek](https://wiki.ubuntu.com/Kernel/LTSEnablementStack)

## <a name="next-steps"></a>Sonraki adımlar
Artık Azure'da yeni sanal makineler oluşturmak için Ubuntu Linux sanal sabit diski kullanmaya hazırsınız. Bu Azure'a .vhd dosyasını karşıya ilk kez kullanıyorsanız, bkz. [bir özel diskten Linux VM oluşturma](upload-vhd.md#option-1-upload-a-vhd).

