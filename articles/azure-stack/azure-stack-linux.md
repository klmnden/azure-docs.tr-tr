---
title: Azure yığınına Linux görüntüleri ekleme
description: Bilgi Linux görüntüleri Azure yığınına nasıl eklenir.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 64a860bc925b9c7499363c1fe39d03df88a9a51d
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="add-linux-images-to-azure-stack"></a>Azure yığınına Linux görüntüleri ekleme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Linux tabanlı bir görüntüyü Azure yığın Market ekleyerek Azure yığında Linux sanal makineleri (VM'ler) dağıtabilirsiniz. Market yönetiminden Azure yığınına Linux görüntüsü eklemek için en kolay yoludur. Bu görüntüleri hazır ve Azure yığını ile uyumluluk için test.

## <a name="marketplace-management"></a>Market Yönetimi

Azure Marketi'nden Linux görüntüleri indirmek için aşağıdaki makaleyi yordamları kullanın. Kullanıcılar, Azure yığında sunmak istediğinize Linux görüntülerini seçin. 

[Market öğesi Azure'dan Azure yığınına karşıdan](azure-stack-download-azure-marketplace-item.md).

Bu görüntüler için sık güncelleştirmeler olduğuna dikkat edin, Market genellikle güncel tutmak üzere Yönetim kontrol edin.

## <a name="prepare-your-own-image"></a>Kendi görüntüsünü hazırlama

 Mümkün olduğunda, hazır ve Azure yığını için test Market Yönetimi aracılığıyla kullanılabilen görüntüleri indirin. 
 
 Azure Linux Aracısı'nı (genellikle adlı `WALinuxAgent` veya `walinuxagent`) gereklidir ve Azure yığında Aracısı'nın tüm sürümlerinde çalışır. Sürüm 2.2.18 veya sürümünü kullanmalısınız kendi görüntünüzü oluşturabilir. Unutmayın [bulut init](https://cloud-init.io/) Azure yığın üzerinde şu anda desteklenmiyor.

 Aşağıdaki yönergeleri kullanarak kendi Linux görüntüsü hazırlayabilirsiniz:

   * [CentOS tabanlı dağıtımlar](../virtual-machines/linux/create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Debian Linux](../virtual-machines/linux/debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Red Hat Enterprise Linux](azure-stack-redhat-create-upload-vhd.md)
   * [SLES & openSUSE](../virtual-machines/linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Ubuntu Server](../virtual-machines/linux/create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

    
## <a name="add-your-image-to-the-marketplace"></a>Market görüntü ekleme
 
İzleyin [Market görüntüsü eklemek](azure-stack-add-vm-image.md). Olduğundan emin olun `OSType` parametrenin ayarlanmış `Linux`.

Market görüntü ekledikten sonra Market öğesi oluşturulur ve Linux sanal makine kullanıcılara dağıtabilirsiniz.
