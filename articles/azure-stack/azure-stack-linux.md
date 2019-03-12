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
ms.date: 02/15/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 11/16/2018
ms.openlocfilehash: 277af6f139e815f46894f5b8df82a1d92ef573d1
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57537142"
---
# <a name="add-linux-images-to-azure-stack"></a>Azure Stack'e Linux görüntüleri ekleme

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Linux sanal makineleri (VM'ler), Linux tabanlı bir görüntü Azure Stack Marketini ekleyerek, bir Azure Stack üzerinde dağıtabilirsiniz. Market Yönetimi üzerinden kolay bir Linux görüntüsü Azure Stack'e ekleme yoludur. Bu görüntüleri hazırlanan ve Azure Stack ile uyumluluk için test.

## <a name="marketplace-management"></a>Market Yönetimi

Azure Market'teki Linux görüntüleri indirmek için yordamları kullanın. [indirme Market öğesi Azure'dan Azure Stack'e](azure-stack-download-azure-marketplace-item.md) makalesi. Azure Stack kullanıcıları sunmak istediğiniz Linux görüntülerini seçin. 

Bu görüntüler için sık sık güncelleştirme olduğunu unutmayın, bu nedenle Market genellikle güncel tutmak için yönetim denetleyin.

## <a name="prepare-your-own-image"></a>Kendi görüntünüzü hazırlama

Mümkün olduğunda, Market hazırlanan ve Azure Stack için test yönetimi aracılığıyla sağlanan görüntülerin indirin.

Azure Linux Aracısı (genellikle adlı `WALinuxAgent` veya `walinuxagent`) gereklidir ve aracıyı'nin tüm sürümleri Azure Stack'te çalışır. Kendi görüntünüzü oluşturursanız, en son sürümünü WALA veya sürüm 2.2.20 kullanmanız gerekir. Azure Stack üzerinde çalışmıyor 2.2.20 2.2.35.1 (dışlamalı) arasındaki sürümleri unutmayın. Unutmayın [cloud-init](https://cloud-init.io/) Azure Stack üzerinde şu anda desteklenmiyor.

Aşağıdaki yönergeleri kullanarak kendi Linux görüntünüzü hazırlayabilirsiniz:

* [CentOS tabanlı dağıtımlar](../virtual-machines/linux/create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Debian Linux](../virtual-machines/linux/debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Red Hat Enterprise Linux](azure-stack-redhat-create-upload-vhd.md)
* [SLES & openSUSE](../virtual-machines/linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ubuntu Server](../virtual-machines/linux/create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="add-your-image-to-the-marketplace"></a>Market'te görüntünüzü ekleme

İzleyin [ve Market görüntüsü ekleme](azure-stack-add-vm-image.md). Emin olun `OSType` parametrenin ayarlanmış `Linux`.

Görüntü Marketi'nde ekledikten sonra bir Market öğesi oluşturulur ve kullanıcıların bir Linux sanal makinesi olarak dağıtabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Stack için Azure Market öğelerini indirme](azure-stack-download-azure-marketplace-item.md)
- [Azure Stack Marketini genel bakış](azure-stack-marketplace.md)
