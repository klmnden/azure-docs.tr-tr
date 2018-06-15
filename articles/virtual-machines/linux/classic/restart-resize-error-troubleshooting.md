---
title: Yeniden başlatmadan veya sorunları yeniden boyutlandırma VM | Microsoft Docs
description: Yeniden başlatmadan veya varolan bir Linux sanal makinesini Azure yeniden boyutlandırma Klasik dağıtım sorunlarını giderme
services: virtual-machines-linux
documentationcenter: ''
author: Deland-Han
manager: felixwu
editor: ''
tags: top-support-issue
ms.assetid: 73f2672c-602e-4766-8948-2b180115d299
ms.service: virtual-machines-linux
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-linux
ms.workload: required
ms.date: 01/10/2017
ms.devlang: na
ms.author: delhan
ms.openlocfilehash: 7f5718a7e1ab2b14902fa61ffb3053910e584ac6
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
ms.locfileid: "24051622"
---
# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a>Yeniden başlatmadan veya varolan bir Linux sanal makinesini Azure yeniden boyutlandırma Klasik dağıtım sorunlarını giderme
> [!div class="op_single_selector"]
> * [Klasik](restart-resize-error-troubleshooting.md)
> * [Resource Manager](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> 
> 

Durdurulmuş bir Azure sanal makine (VM) Başlat veya mevcut bir Azure VM'yi yeniden boyutlandırmak çalıştığınızda karşılaştığınız ortak bir ayırma hatası hatadır. Küme veya bölgede kullanılabilir kaynak yok veya istenen VM boyutu destekleyemez olduğunda bu hata oluşur.

> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Resource Manager sürümü için bkz: [burada](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Toplama denetim günlüklerini
Sorun giderme başlatmak için sorunu ile ilişkili hata tanımlamak için denetim günlüklerini toplayın.

Azure portalında tıklatın **Gözat** > **sanal makineleri** > *Linux sanal makineniz*  >   **Ayarları** > **denetim günlüklerini**.

## <a name="issue-error-when-starting-a-stopped-vm"></a>Sorun: durmuş bir VM'yi başlatma sırasında hata
Durmuş bir VM'yi Başlat ancak alma ayırma hatası deneyin.

### <a name="cause"></a>Nedeni
Bulut hizmeti barındıran özgün kümesine denenmesi durdurulmuş VM başlatmak için bir istek aldı. Ancak, Küme isteği gerçekleştirmek kullanılabilir boş disk alanı yok.

### <a name="resolution"></a>Çözüm
* Yeni bir bulut hizmeti oluşturup bir bölge veya bölge tabanlı bir sanal ağ, ancak bir benzeşim grubu ile ilişkilendirin.
* Durdurulan VM silin.
* Yeni bulut hizmeti VM diskleri kullanarak yeniden oluşturun.
* Yeniden oluşturulan VM başlatın.

Yeni bir bulut hizmeti oluşturmaya çalışırken bir hata alırsanız, daha sonra yeniden deneyin veya Bulut hizmeti için bölge değiştirin.

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
Azure'da yeni bir Linux VM oluşturma sırasında sorunlarla karşılaşırsanız, bkz: [Azure'da yeni bir Linux sanal makine oluşturma ile dağıtım sorunlarını giderme](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

