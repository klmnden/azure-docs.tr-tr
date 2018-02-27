---
title: "Azure yığınına Linux görüntüleri ekleme"
description: "Bilgi Linux görüntüleri Azure yığınına nasıl eklenir."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/25/2017
ms.author: brenduns
ms.reviewer: anajod
ms.openlocfilehash: 29e5443de4dc43efe6d536b0f8b9cfc6ad37a669
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="add-linux-images-to-azure-stack"></a>Azure yığınına Linux görüntüleri ekleme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti* 

Linux tabanlı bir görüntüyü Azure yığın Market ekleyerek Linux sanal makineleri Azure yığında dağıtabilirsiniz. Market yönetiminden Azure yığınına Linux görüntüsü eklemek için en kolay yoludur.

## <a name="marketplace-management"></a>Market Yönetimi

Azure Marketi'nden Linux görüntüleri indirmek için aşağıdaki makaleyi yordamları kullanın. Kullanıcılar, Azure yığında sunmak istediğinize Linux görüntülerini seçin.

[Market öğesi Azure'dan Azure yığınına karşıdan](azure-stack-download-azure-marketplace-item.md).

## <a name="download-an-image"></a>Görüntüyü indir

Yükleyin ve aşağıdaki bağlantıları kullanarak Azure yığın uyumlu Linux görüntüleri ayıklayın:


   * [Bitnami](https://bitnami.com/azure-stack)
   * [CentOS](http://olstacks.cloudapp.net/latest/)
   * [CoreOS](https://stable.release.core-os.net/amd64-usr/current/coreos_production_azure_image.vhd.bz2)
   * [SuSE](https://download.suse.com/Download?buildid=VCFi7y7MsFQ~)
   * [Ubuntu 14.04 LTS](https://partner-images.canonical.com/azure/azure_stack/) / [Ubuntu 16.04 LTS](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)

1. Gerekirse, VHD yansımasını ayıklamak ve [Market görüntüsü eklemek](azure-stack-add-vm-image.md). Olduğundan emin olun `OSType` parametrenin ayarlanmış `Linux`.
2. Market görüntü ekledikten sonra Market öğesi oluşturulur ve Linux sanal makine kullanıcılara dağıtabilirsiniz.

## <a name="prepare-your-own-image"></a>Kendi görüntüsünü hazırlama

Aşağıdaki yönergeler birini kullanarak kendi Linux görüntü hazırlayabilirsiniz:
   
   * [CentOS tabanlı dağıtımlar](../virtual-machines/linux/create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Debian Linux](../virtual-machines/linux/debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Oracle Linux](../virtual-machines/linux/oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Red Hat Enterprise Linux](../virtual-machines/linux/redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [SLES & openSUSE](../virtual-machines/linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Ubuntu](../virtual-machines/linux/create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

1. İndirme ve yükleme [Azure Linux Aracısı](https://github.com/Azure/WALinuxAgent/).
   
    Azure Linux Aracısı sürüm 2.1.3 veya üzeri Linux VM'NİZDE Azure yığında sağlamak için gereklidir. Çoğu, listelenen dağıtımları daha önce zaten bu sürümü aracı veya üzeri bir paket olarak kendi depoları içerir (genellikle adlı `WALinuxAgent` veya `walinuxagent`). Ancak, Azure aracı paketi sürümünün değerinden 2.1.3 olup olmadığını (örneğin, 2.0.18 veya alt) Aracısı'nı el ile yüklemeniz gerekir. Paket adı ya da çalıştırarak yüklü olan sürümü belirlenebilir `/usr/sbin/waagent -version` VM üzerinde.
   
    Azure Linux Aracısı'nı el ile yüklemek için aşağıdaki yönergeleri izleyin-
   
   a. İlk olarak, en son Azure Linux Aracısı'ndan indirin [GitHub](https://github.com/Azure/WALinuxAgent/releases), örneğin:
     
            # wget https://github.com/Azure/WALinuxAgent/archive/v2.2.16.tar.gz
   b. Azure Aracı paketini açın:
     
            # tar -vzxf v2.2.16.tar.gz
   c. Python setuptools yükleyin
     
        **Debian / Ubuntu**
     
            # sudo apt-get update
            # sudo apt-get install python-setuptools
     
        **Ubuntu 16.04+**
     
            # sudo apt-get install python3-setuptools
     
        **RHEL / CentOS / Oracle Linux**
     
            # sudo yum install python-setuptools
   d. Azure aracısını yükleyin:
     
            # cd WALinuxAgent-2.2.16
            # sudo python3 setup.py install --register-service
     
     Sistemler ile Python 2.x ve Python 3.x yüklü yan yana aşağıdaki komutu çalıştırmanız gerekebilir:
     
         sudo python3 setup.py install --register-service
     Daha fazla bilgi için bkz: Azure Linux Aracısı [Benioku](https://github.com/Azure/WALinuxAgent/blob/master/README.md).
2. [Market görüntüsü eklemek](azure-stack-add-vm-image.md). Olduğundan emin olun `OSType` parametrenin ayarlanmış `Linux`.
3. Market görüntü ekledikten sonra Market öğesi oluşturulur ve Linux sanal makine kullanıcılara dağıtabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Azure yığınında hizmetleri sunan genel bakış](azure-stack-offer-services-overview.md)

