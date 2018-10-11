---
title: Azure Stack'e Linux görüntüleri ekleme
description: Bilgi nasıl Linux görüntüleri Azure Stack'e ekleme.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: sethm
ms.reviewer: jeffgo
ms.openlocfilehash: db52d145c3bfbd9415072be13ccb502969f07374
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49077656"
---
# <a name="add-linux-images-to-azure-stack"></a>Azure Stack'e Linux görüntüleri ekleme

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Linux sanal makineleri (VM'ler), Linux tabanlı bir görüntü Azure Stack Marketini ekleyerek, bir Azure Stack üzerinde dağıtabilirsiniz. Market Yönetimi üzerinden kolay bir Linux görüntüsü Azure Stack'e ekleme yoludur. Bu görüntüleri hazırlanan ve Azure Stack ile uyumluluk için test.

## <a name="marketplace-management"></a>Market Yönetimi

Azure Market'teki Linux görüntüleri indirmek için şu makalede yordamları kullanın. Azure Stack kullanıcıları sunmak istediğiniz Linux görüntülerini seçin. 

[Azure Stack için Azure Market öğelerini indirme](azure-stack-download-azure-marketplace-item.md).

Bu görüntüler için sık sık güncelleştirme olduğunu unutmayın, bu nedenle Market genellikle güncel tutmak için yönetim denetleyin.

## <a name="prepare-your-own-image"></a>Kendi görüntünüzü hazırlama

 Mümkün olduğunda, Market hazırlanan ve Azure Stack için test yönetimi aracılığıyla sağlanan görüntülerin indirin. 
 
 Azure Linux Aracısı (genellikle adlı `WALinuxAgent` veya `walinuxagent`) gereklidir ve aracıyı'nin tüm sürümleri Azure Stack'te çalışır. Sürüm 2.2.18 veya sürümünü kullanmalısınız kendi görüntünüzü oluşturabilir. Unutmayın [cloud-init](https://cloud-init.io/) Azure Stack üzerinde şu anda desteklenmiyor.

 Aşağıdaki yönergeleri kullanarak kendi Linux görüntünüzü hazırlayabilirsiniz:

   * [CentOS tabanlı dağıtımlar](../virtual-machines/linux/create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Debian Linux](../virtual-machines/linux/debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Red Hat Enterprise Linux](azure-stack-redhat-create-upload-vhd.md)
   * [SLES & openSUSE](../virtual-machines/linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Ubuntu Server](../virtual-machines/linux/create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

    
## <a name="add-your-image-to-the-marketplace"></a>Market'te görüntünüzü ekleme
 
İzleyin [ve Market görüntüsü ekleme](azure-stack-add-vm-image.md). Emin olun `OSType` parametrenin ayarlanmış `Linux`.

Görüntü Marketi'nde ekledikten sonra bir Market öğesi oluşturulur ve kullanıcıların bir Linux sanal makinesi olarak dağıtabilirsiniz.
