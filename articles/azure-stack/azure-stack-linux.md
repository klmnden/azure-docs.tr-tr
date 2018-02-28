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
ms.date: 02/22/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 6e6f9ca3b314ee2f58d8007e7ddc93ddd213e361
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="add-linux-images-to-azure-stack"></a>Azure yığınına Linux görüntüleri ekleme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Linux tabanlı bir görüntüyü Azure yığın Market ekleyerek Azure yığında Linux sanal makineleri (VM'ler) dağıtabilirsiniz. Market yönetiminden Azure yığınına Linux görüntüsü eklemek için en kolay yoludur. Bu görüntüleri hazır ve Azure yığını ile uyumluluk için test.

## <a name="marketplace-management"></a>Market Yönetimi

Azure Marketi'nden Linux görüntüleri indirmek için aşağıdaki makaleyi yordamları kullanın. Kullanıcılar, Azure yığında sunmak istediğinize Linux görüntülerini seçin.

[Market öğesi Azure'dan Azure yığınına karşıdan](azure-stack-download-azure-marketplace-item.md).

## <a name="prepare-your-own-image"></a>Kendi görüntüsünü hazırlama

Aşağıdaki yönergeler birini kullanarak kendi Linux görüntü hazırlayabilirsiniz:

   * [CentOS tabanlı dağıtımlar](../virtual-machines/linux/create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Debian Linux](../virtual-machines/linux/debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [SLES & openSUSE](../virtual-machines/linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Ubuntu](../virtual-machines/linux/create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

1. İndirme ve yükleme [Azure Linux Aracısı](https://github.com/Azure/WALinuxAgent/).

    Azure Linux Aracısı sürüm 2.2.2 veya üzeri, Linux VM'de Azure yığını ve bazı sürümlerinde çalışmıyor (2.2.12 ve örnekler için 2.2.13) sağlamak için gereklidir. Çoğu, listelenen dağıtımları daha önce zaten Aracısı'nın bir sürümünü içerir (genellikle adlı `WALinuxAgent` veya `walinuxagent`). Özel görüntü oluşturursanız, aracı el ile yüklemeniz gerekir. Paket adı veya çalıştırarak yüklü olan sürümü belirlenebilir `/usr/sbin/waagent -version` VM üzerinde.

    Azure Linux Aracısı'nı el ile yüklemek için aşağıdaki yönergeleri izleyin-

   a. İlk olarak, en son Azure Linux Aracısı'ndan indirin [GitHub](https://github.com/Azure/WALinuxAgent/releases), örneğin:

            # wget https://github.com/Azure/WALinuxAgent/archive/v2.2.21.tar.gz
   b. Azure Aracı paketini açın:

            # tar -vzxf v2.2.21.tar.gz
   c. Python setuptools yükleyin

        **Debian / Ubuntu**

            # sudo apt-get update
            # sudo apt-get install python-setuptools

        **Ubuntu 16.04+**

            # sudo apt-get install python3-setuptools

        **RHEL / CentOS / Oracle Linux**

            # sudo yum install python-setuptools
   d. Azure aracısını yükleyin:

            # cd WALinuxAgent-2.2.21
            # sudo python3 setup.py install --register-service

     Sistemler ile Python 2.x ve Python 3.x yüklü yan yana aşağıdaki komutu çalıştırmanız gerekebilir:

         sudo python3 setup.py install --register-service
     Daha fazla bilgi için bkz: Azure Linux Aracısı [Benioku](https://github.com/Azure/WALinuxAgent/blob/master/README.md).
2. [Market görüntüsü eklemek](azure-stack-add-vm-image.md). Olduğundan emin olun `OSType` parametrenin ayarlanmış `Linux`.
3. Market görüntü ekledikten sonra Market öğesi oluşturulur ve Linux sanal makine kullanıcılara dağıtabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Azure yığınında hizmetleri sunan genel bakış](azure-stack-offer-services-overview.md)
