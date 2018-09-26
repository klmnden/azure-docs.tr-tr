---
title: Başlatma veya yeniden boyutlandırmayla sorunları VM | Microsoft Docs
description: Başlatma veya varolan bir Windows sanal makinesini azure'da yeniden boyutlandırmayla ilgili Klasik dağıtım sorunlarını giderme
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: felixwu
editor: ''
tags: top-support-issue
ms.assetid: aa854fff-c057-4b8e-ad77-e4dbc39648cc
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: required
ms.date: 06/15/2018
ms.devlang: na
ms.author: delhan
ms.openlocfilehash: 400b20e474257650a22e04c89ddde581bf0552f4
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47091739"
---
# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-windows-virtual-machine-in-azure"></a>Başlatma veya varolan bir Windows sanal makinesini azure'da yeniden boyutlandırmayla ilgili Klasik dağıtım sorunlarını giderme
> [!div class="op_single_selector"]
> * [Klasik](virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)
> * [Resource Manager](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
> 
> 

Durdurulmuş bir Azure sanal makine (VM) Başlat veya mevcut bir Azure VM'yi yeniden boyutlandırma çalıştığınızda, karşılaştığınız ortak bir ayırma hatası hatadır. Küme veya bölge kullanılabilir kaynak yok veya istenen VM boyutu desteklemez, bu hata oluşur.

> [!IMPORTANT]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md).  Bu makale klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.
> 
> 

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Toplama denetim günlükleri
Sorun gidermeye başlamak için sorunla ilişkili hatanın tanımlamak için denetim günlüklerini toplayın.

Azure portalında **Gözat** > **sanal makineler** > *Windows sanal makinenizi*  >   **Ayarları** > **denetim günlükleri**.

## <a name="issue-error-when-starting-a-stopped-vm"></a>Sorun: bir VM'yi durdurduğunuzda başlatılırken hata
Bir VM'yi durdurduğunuzda, ancak bir ayırma hatası alma deneyin.

### <a name="cause"></a>Nedeni
VM'yi durdurduğunuzda isteğine bulut hizmetini barındıran konumundaki özgün küme denenmesi gerekir. Ancak, Küme isteği gerçekleştirmek kullanılabilir boş alan yok.

### <a name="resolution"></a>Çözüm
* Yeni bir bulut hizmeti oluşturma ve bir bölgesi veya bölge tabanlı bir sanal ağ, ancak bir benzeşim grubu ile ilişkilendirin.
* VM durduruldu silin.
* Yeni bir bulut hizmeti VM diskleri kullanarak yeniden oluşturun.
* Yeniden oluşturulan bir VM'yi başlatın.

Yeni bir bulut hizmeti oluşturulmaya çalışılırken bir hata alırsanız, daha sonra yeniden deneyin veya Bulut hizmeti için bölge değiştirin.

> [!IMPORTANT]
> Mevcut bulut hizmeti için bu bilgileri kullanan tüm bağımlılıklar için bu bilgileri değiştirmeye ihtiyacınız olacak şekilde yeni bir bulut hizmeti, yeni bir ad ve VIP, gerekir.
> 
> 

## <a name="issue-error-when-resizing-an-existing-vm"></a>Sorun: varolan bir VM'yi yeniden boyutlandırma sırasında hata oluştu
Varolan bir VM'yi yeniden boyutlandırma, ancak bir ayırma hatası alma deneyin.

### <a name="cause"></a>Nedeni
Bulut hizmetini barındıran konumundaki özgün küme denenmesi VM yeniden boyutlandırmak için bir istek aldı. Ancak, küme, istenen VM boyutu desteklemez.

### <a name="resolution"></a>Çözüm
İstenen VM boyutunu azaltın ve yeniden boyutlandırma isteği yeniden deneyin.

* Tıklayın **tümüne Gözat** > **sanal makineler (Klasik)** > *sanal makinenizi* > **ayarları**  >  **Boyutu**. Ayrıntılı adımlar için bkz. [sanal makine](https://msdn.microsoft.com/library/dn168976.aspx).

VM boyutunu küçültmek mümkün değilse, aşağıdaki adımları izleyin:

* Bir benzeşim grubuna bağlı olmayan ve bir benzeşim grubuna bağlı bir sanal ağ ile ilişkili olmayan yeni bir bulut hizmeti oluşturun.
* Yeni, daha büyük boyutta bir VM oluşturun.

Aynı bulut hizmetindeki tüm sanal makinelerinizi birleştirebilir. Mevcut bulut hizmetiniz bölge tabanlı sanal ağ ile ilişkili ise, yeni bir bulut hizmeti mevcut bir sanal ağa bağlanabilir.

Mevcut bulut hizmetine bölge tabanlı sanal ağ ile ilişkili değilse, var olan bulut hizmetindeki sanal makineleri silmek ve bunları kendi disklerden yeni bulut hizmetinde yeniden vardır. Ancak, bunlar şu anda mevcut bulut hizmeti için bu bilgileri kullanan tüm bağımlılıklar için güncelleştirmeniz gerekir için yeni bir bulut hizmeti yeni bir ad ve VIP, sahip olacağını unutmayın.

## <a name="next-steps"></a>Sonraki adımlar
Azure'da Windows VM oluşturduğunuz sırada bir sorunla karşılaşırsanız bkz [Azure'da Windows sanal makine oluştururken dağıtım sorunlarını giderme](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

