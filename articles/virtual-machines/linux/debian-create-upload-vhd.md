---
title: Azure'da bir Debian Linux VHD hazırlama | Microsoft Docs
description: Azure'da dağıtım için Debian VHD görüntüleri oluşturmayı öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: szarkos
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: a6de7a7c-cc70-44e7-aed0-2ae6884d401a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/13/2018
ms.author: szark
ms.openlocfilehash: bdeaf4ec4a276e7cdd94402159f6adac474b3af8
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67671540"
---
# <a name="prepare-a-debian-vhd-for-azure"></a>Azure için Debian VHD hazırlama
## <a name="prerequisites"></a>Önkoşullar
Bu bölümde, zaten bir Debian Linux işletim sistemi indirilen bir .iso dosyasından yüklediğiniz varsayılarak hazırlanmıştır [Debian Web sitesi](https://www.debian.org/distrib/) bir sanal sabit diske. Birden çok araç, .vhd dosyaları oluşturmak için mevcut; Hyper-V, yalnızca bir örnektir. Hyper-V kullanma yönergeleri için bkz: [Hyper-V rolünü yükleme ve sanal makine yapılandırma](https://technet.microsoft.com/library/hh846766.aspx).

## <a name="installation-notes"></a>Yükleme notları
* Ayrıca bkz: [genel Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes) Linux için Azure hazırlama hakkında daha fazla ipucu için.
* Azure'da yeni VHDX biçimi desteklenmiyor. Disk Hyper-V Yöneticisi'ni kullanarak VHD biçimine dönüştürebilir veya **convert-vhd** cmdlet'i.
* Linux sistemini yüklerken LVM (genellikle birçok yüklemeleri için varsayılan) yerine standart bölümlerini kullanmanız önerilir. Özellikle bir işletim sistemi diski hiç sorun giderme için başka bir VM'ye bağlı gerekiyorsa, bu kopyalanan sanal makineler ile ad çakışmalarını LVM uğraşmasına gerek kalmaz. [LVM'yi](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veri disklerinde, tercih etmeleri durumunda kullanılıyor olabilir.
* İşletim sistemi diski üzerinde takas bölümü yapılandırmayın. Azure Linux Aracısı, geçici kaynak diski üzerinde takas dosyası oluşturmak için yapılandırılabilir. Aşağıdaki adımlarda daha fazla bilgi bulunabilir.
* Tüm VHD'leri azure'da bir sanal Boyut 1 MB ile uyumlu olması gerekir. Ham bir diskten VHD'ye dönüştürme yaparken, ham disk boyutu 1 MB dönüştürmeden önce bir çok olduğundan emin olmalısınız. Daha fazla bilgi için [Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes).

## <a name="use-azure-manage-to-create-debian-vhds"></a>Debian VHD oluşturmak için Azure Yönet'i kullanın
Azure için Debian VHD gibi oluşturmak için kullanılabilen araçları vardır [azure-yönetme](https://github.com/credativ/azure-manage) komut dosyalarını [Credativ](https://www.credativ.com/). Bu, sıfırdan bir görüntü oluşturma ve önerilen yaklaşımdır. Örneğin, bir Debian 8 VHD çalıştırma aşağıdaki oluşturmak için indirmek için komutları `azure-manage` yardımcı programını (ve bağımlılıkları) çalıştırıp `azure_build_image` betiği:

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a>El ile bir Debian VHD hazırlama
1. Hyper-V Yöneticisi'nde, sanal makineyi seçin.
2. Tıklayın **Connect** sanal makine için bir konsol penceresi açın.
3. Ardından bir ISO kullanarak işletim sistemi yüklü değilse, ilgili herhangi bir satırı Açıklama "`deb cdrom`" içinde `/etc/apt/source.list`.

4. Düzen `/etc/default/grub` dosya ve değiştirme **GRUB_CMDLINE_LINUX** gibi ek çekirdek parametreleri için Azure içerecek şekilde parametresi.
   
        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200n8 earlyprintk=ttyS0,115200"

5. Grub'ı yeniden derleyin ve çalıştırın:

        # sudo update-grub

6. Debian'ın Azure depoları için /etc/apt/sources.list Debian 8 veya 9 ekleyin:

    **Debian 8.x "Jessie"**

        deb http://debian-archive.trafficmanager.net/debian jessie main
        deb-src http://debian-archive.trafficmanager.net/debian jessie main
        deb http://debian-archive.trafficmanager.net/debian-security jessie/updates main
        deb-src http://debian-archive.trafficmanager.net/debian-security jessie/updates
        deb http://debian-archive.trafficmanager.net/debian jessie-updates main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-updates main
        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main

    **Debian 9.x "Uzat"**

        deb http://debian-archive.trafficmanager.net/debian stretch main
        deb-src http://debian-archive.trafficmanager.net/debian stretch main
        deb http://debian-archive.trafficmanager.net/debian-security stretch/updates main
        deb-src http://debian-archive.trafficmanager.net/debian-security stretch/updates main
        deb http://debian-archive.trafficmanager.net/debian stretch-updates main
        deb-src http://debian-archive.trafficmanager.net/debian stretch-updates main
        deb http://debian-archive.trafficmanager.net/debian stretch-backports main
        deb-src http://debian-archive.trafficmanager.net/debian stretch-backports main


7. Azure Linux Aracısı'nı yükleyin:
   
        # sudo apt-get update
        # sudo apt-get install waagent

8. Debian 9 + için bu yeni Debian bulut çekirdek azure'da sanal makineler ile kullanılmak için önerilir. Bu yeni çekirdek yüklemek için önce aşağıdaki içeriklerle /etc/apt/preferences.d/linux.pref adlı bir dosya oluşturun:
   
        Package: linux-* initramfs-tools
        Pin: release n=stretch-backports
        Pin-Priority: 500
   
    Yeni bir Debian bulut çekirdek yüklemek için çalıştırın "sudo apt-get install linux-görüntü-bulut-amd64".

9. Sanal makinenin sağlamasını kaldırma ve Azure'da sağlama için hazırlamak ve çalıştırın:
   
        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout

10. Tıklayın **eylem** kapatma aşağı Hyper-V Yöneticisi'nde ->. Linux VHD'nizi artık Azure'a yüklenmek hazırdır.

## <a name="next-steps"></a>Sonraki adımlar
Artık Azure'da yeni sanal makineler oluşturmak için Debian sanal sabit diski kullanmaya hazırsınız. Bu Azure'a .vhd dosyasını karşıya ilk kez kullanıyorsanız, bkz. [bir özel diskten Linux VM oluşturma](upload-vhd.md#option-1-upload-a-vhd).

