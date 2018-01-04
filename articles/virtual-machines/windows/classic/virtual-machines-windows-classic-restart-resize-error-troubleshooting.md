---
title: "Yeniden başlatmadan veya sorunları yeniden boyutlandırma VM | Microsoft Docs"
description: "Yeniden başlatmadan veya mevcut bir Windows sanal makine azure'da yeniden boyutlandırma Klasik dağıtım sorunlarını giderme"
services: virtual-machines-windows
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: aa854fff-c057-4b8e-ad77-e4dbc39648cc
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: required
ms.date: 11/03/2017
ms.devlang: na
ms.author: delhan
ms.openlocfilehash: bed5da25042d29983bad9a80cd44bdd7df261c2e
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-windows-virtual-machine-in-azure"></a>Yeniden başlatmadan veya mevcut bir Windows sanal makine azure'da yeniden boyutlandırma Klasik dağıtım sorunlarını giderme
> [!div class="op_single_selector"]
> * [Klasik](virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)
> * [Resource Manager](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
> 
> 

Durdurulmuş bir Azure sanal makine (VM) Başlat veya mevcut bir Azure VM'yi yeniden boyutlandırmak çalıştığınızda karşılaştığınız ortak bir ayırma hatası hatadır. Küme veya bölgede kullanılabilir kaynak yok veya istenen VM boyutu destekleyemez olduğunda bu hata oluşur.

> [!IMPORTANT]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md).  Bu makale klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.
> 
> 

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Toplama denetim günlüklerini
Sorun giderme başlatmak için sorunu ile ilişkili hata tanımlamak için denetim günlüklerini toplayın.

Azure portalında tıklatın **Gözat** > **sanal makineleri** > *Windows sanal makinenizi* > **ayarları** > **denetim günlüklerini**.

## <a name="issue-error-when-starting-a-stopped-vm"></a>Sorun: durmuş bir VM'yi başlatma sırasında hata
Durmuş bir VM'yi Başlat ancak alma ayırma hatası deneyin.

### <a name="cause"></a>Nedeni
Bulut hizmeti barındıran özgün kümesine denenmesi durdurulmuş VM başlatmak için bir istek aldı. Ancak, Küme isteği gerçekleştirmek kullanılabilir boş disk alanı yok.

### <a name="resolution"></a>Çözüm
* Yeni bir bulut hizmeti oluşturup bir bölge veya bölge tabanlı bir sanal ağ, ancak bir benzeşim grubu ile ilişkilendirin.
* Durdurulan VM silin.
* Yeni bulut hizmeti VM diskleri kullanarak yeniden oluşturun.
* Yeniden oluşturulan VM başlatın.

Yeni bir bulut hizmeti oluşturmaya çalışırken bir hata alırsanız, daha sonra yeniden deneyin veya Bulut hizmeti için bölgeyi değiştirin.

> [!IMPORTANT]
> Bu bilgi için bu bilgileri mevcut bulut hizmeti için kullandığınız tüm bağımlılıkları değiştirmeniz gerekir böylece yeni bulut hizmeti yeni bir ad ve VIP, gerekir.
> 
> 

## <a name="issue-error-when-resizing-an-existing-vm"></a>Sorun: mevcut bir VM'yi yeniden boyutlandırılırken hata
Mevcut bir VM'yi yeniden boyutlandırın, ancak bir ayırma hatası almak deneyin.

### <a name="cause"></a>Nedeni
Bulut hizmeti barındıran özgün kümesine denenmesi VM yeniden boyutlandırmak için bir istek aldı. Ancak, küme istenen VM boyutu desteklemez.

### <a name="resolution"></a>Çözüm
İstenen VM boyutu azaltın ve yeniden boyutlandırma isteği yeniden deneyin.

* Tıklatın **tümüne Gözat** > **sanal makineleri (Klasik)** > *sanal makineniz* > **ayarları** > **boyutu**. Ayrıntılı adımlar için bkz: [sanal makine yeniden boyutlandırma](https://msdn.microsoft.com/library/dn168976.aspx).

VM boyutunu azaltmak mümkün değilse, aşağıdaki adımları izleyin:

* Bir benzeşim grubuna bağlı değil ve bir benzeşim grubuna bağlı bir sanal ağ ile ilişkilendirilmemiş sağlayarak yeni bir bulut hizmeti oluşturun.
* Yeni, büyük ölçekli bir VM içinde oluşturun.

Aynı bulut hizmetindeki tüm Vm'leriniz birleştirebilir. Mevcut bulut hizmetiniz bir bölge tabanlı sanal ağ ile ilişkili ise, yeni bulut hizmeti varolan bir sanal ağa bağlanabilir.

Mevcut bulut hizmeti bir bölge tabanlı sanal ağ ile ilişkili değilse, mevcut bulut hizmetindeki sanal makineleri silin ve bunları kendi disklerden yeni bulut hizmeti yeniden sahip. Ancak, bunlar şu anda mevcut bulut hizmeti için bu bilgileri kullanan tüm bağımlılıkları için güncelleştirmeniz gerekir böylece yeni bulut hizmeti yeni bir ad ve VIP, olacaktır unutmamak önemlidir.

## <a name="next-steps"></a>Sonraki adımlar
Azure üzerinde bir Windows VM oluşturduğunuzda sorunlarıyla karşılaşırsanız bkz [Azure'da Windows sanal makine oluşturma ile dağıtım sorunlarını giderme](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

