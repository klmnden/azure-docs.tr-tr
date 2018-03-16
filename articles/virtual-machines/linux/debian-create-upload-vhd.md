---
title: "Azure'da Debian bir Linux VHD hazırlama | Microsoft Docs"
description: "Debian 7 & dağıtımı için 8 VHD dosyalarını Azure'da oluşturmayı öğrenin."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: a6de7a7c-cc70-44e7-aed0-2ae6884d401a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: szark
ms.openlocfilehash: 9b32b298f141e9ee54b4c42d3ee9c15174daf8b7
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="prepare-a-debian-vhd-for-azure"></a>Azure için Debian VHD hazırlama
## <a name="prerequisites"></a>Önkoşullar
Bu bölümde, zaten Debian Linux işletim sistemi yüklenen bir .iso dosyasından yüklediğinizi varsayar [Debian Web sitesi](https://www.debian.org/distrib/) bir sanal sabit disk için. Birden çok araç, .vhd dosyaları oluşturmak için mevcut; Hyper-V yalnızca bir örnektir. Hyper-V kullanma yönergeleri için bkz: [Hyper-V rolünü yükleyin ve sanal makine yapılandırma](https://technet.microsoft.com/library/hh846766.aspx).

## <a name="installation-notes"></a>Yükleme notları
* Ayrıca bkz [genel Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes) Linux Azure için hazırlama hakkında daha fazla ipucu için.
* Azure'da yeni VHDX biçimi desteklenmiyor. Disk Hyper-V Yöneticisi'ni kullanarak VHD biçimine Dönüştür veya **convert-vhd** cmdlet'i.
* Linux sistemini yüklerken LVM (genellikle birçok yüklemeleri için varsayılan) yerine standart bölümlerini kullanmanız önerilir. Özellikle bir işletim sistemi diski şimdiye kadar başka bir VM için sorun giderme için eklenmesi gerekiyorsa, bu kopyalanan VMs LVM ad çakışmalarını önler. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veri disklerde tercih edilen varsa kullanılabilir.
* Bir takas bölüm işletim sistemi disk üzerinde yapılandırmayın. Azure Linux Aracısı geçici kaynak disk üzerinde bir takas dosyası oluşturmak için yapılandırılabilir. Aşağıdaki adımlarda bu hakkında daha fazla bilgi bulunabilir.
* Tüm VHD'leri Azure üzerinde bir sanal boyutu 1 MB ile hizalı olması gerekir. Ham diskten VHD'ye dönüştürülürken ham disk boyutu 1 MB dönüştürmeden önce birden fazla olduğundan emin olmanız gerekir. Bkz: [Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes) daha fazla bilgi için.

## <a name="use-azure-manage-to-create-debian-vhds"></a>Debian VHD oluşturmak için Azure yönetmek kullanın
Araçlar Azure, Debian VHD'ler gibi oluşturmak için kullanılabilir [azure-yönetmek](https://github.com/credativ/azure-manage) betikten [credativ](http://www.credativ.com/). Bu, sıfırdan bir görüntü oluşturma karşı önerilen yaklaşımdır. Örneğin, oluşturmak için bir Debian 8 yüklemek için aşağıdaki komutları çalıştırın azure-yönetebilirsiniz (ve bağımlılıklarını) ve azure_build_image komut dosyasını çalıştırın:

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a>El ile Debian VHD hazırlama
1. Hyper-V Yöneticisi'nde sanal makineyi seçin.
2. Tıklatın **Bağlan** sanal makine için bir konsol penceresi açın.
3. Açıklama satırı çıkışı **deb cdrom** içinde `/etc/apt/source.list` bir ISO dosyası karşı VM ayarlarsanız.
4. Düzen `/etc/default/grub` dosya ve değiştirme **GRUB_CMDLINE_LINUX** gibi ek çekirdek parametreleri için Azure içerecek şekilde parametresi.
   
        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200 earlyprintk=ttyS0,115200 rootdelay=30"
5. Kaz yeniden oluşturun ve çalıştırın:
   
        # sudo update-grub
6. Debian'ın Azure depoları için /etc/apt/sources.list Debian 7 veya 8 için ekleyin:
   
    **Wheezy debian 7.x""**
   
        deb http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb-src http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure wheezy main
        deb-src http://debian-archive.trafficmanager.net/debian-azure wheezy main

    **Debian 8.x "Jessie"**

        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure jessie main
        deb-src http://debian-archive.trafficmanager.net/debian-azure jessie main


1. Azure Linux Aracısı'nı yükleyin:
   
        # sudo apt-get update
        # sudo apt-get install waagent
2. Debian 7'de, backports wheezy depodan 3.16 tabanlı çekirdek çalıştırmak için gereklidir. İlk aşağıdaki içeriğe sahip /etc/apt/preferences.d/linux.pref adlı bir dosya oluşturun:
   
        Package: linux-image-amd64 initramfs-tools
        Pin: release n=wheezy-backports
        Pin-Priority: 500
   
    Then run "sudo apt-get install linux-image-amd64" to install the new kernel.
3. Sanal makine yetkisini kaldırma ve Azure üzerinde sağlamak için hazırlayın ve çalıştırın:
   
        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout
4. Tıklatın **eylem** Hyper-V Yöneticisi'nde kapatma aşağı ->. Linux VHD Azure'a karşıya yüklenecek artık hazırdır.

## <a name="next-steps"></a>Sonraki adımlar
Şimdi yeni sanal makineler oluşturmak için Debian, sanal sabit diski kullanmak hazırsınız. Azure için .vhd dosyasını karşıya yüklüyoruz ilk kez kullanıyorsanız bkz [özel bir diskten bir Linux VM oluşturma](upload-vhd.md#option-1-upload-a-vhd).

