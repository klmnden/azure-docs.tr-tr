---
title: Azure Stack'te birim düğüm eylemleri ölçeklendirme | Microsoft Docs
description: Düğüm durumunu görüntüleme ve üzerinde kapatma, boşaltma gücünden yararlanın ve bir Azure Stack tümleşik sisteminde düğüm eylemleri sürdürme hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 1f59f2ce6e3bf8d34ce225aa93da76ad523775e0
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2018
ms.locfileid: "42056011"
---
# <a name="scale-unit-node-actions-in-azure-stack"></a>Azure stack'teki ölçek birimi düğüm eylemleri

*İçin geçerlidir: Azure Stack tümleşik sistemleri*

Bu makalede, bir ölçek birimi ve onun ilişkili düğümlerin durumunun nasıl görüntüleneceği ve kullanılabilir düğüm eylemleri kullanmayı açıklar. Düğüm eylemleri gücüyle, güç kapalı dahil, boşaltma, sürdürme ve onarın. Genellikle, bölümleri veya düğüm kurtarma senaryoları için alan değiştirme sırasında bu düğüm eylemlerini kullanın.

> [!Important]  
> Bu makalede açıklanan tüm düğüm Eylemler aynı anda yalnızca hedef bir düğüm olmalıdır.


## <a name="view-the-status-of-a-scale-unit-and-its-nodes"></a>Bir ölçek birimi ve düğümlerini durumunu görüntüleme

Yönetici portalı'nda, bir ölçek birimi ve ilişkili düğümlerinin durumunu kolayca görüntüleyebilirsiniz.

Bir ölçek birimi durumunu görüntülemek için:

1. Üzerinde **bölge Yönetimi** kutucuğunda, bölgeyi seçin.
2. Sol taraftaki altında **altyapı kaynaklarını**seçin **ölçek birimleri**.
3. Sonuçlarda ölçek birimi seçin.
 
Burada, aşağıdaki bilgileri görüntüleyebilirsiniz:

- bölge adı. Bölge adı ile başvurulan **-konum** PowerShell modülünde.
- tür sistemi
- Toplam mantıksal çekirdek sayısı
- toplam bellek
- tek tek düğümler ve durumlarını listesi; Her iki **çalıştıran** veya **durduruldu**.

![Her düğüm için çalıştırma durumunu gösteren ölçek birimi kutucuğu](media/azure-stack-node-actions/ScaleUnitStatus.PNG)

## <a name="view-information-about-a-scale-unit-node"></a>Bir ölçek birimi düğüm hakkındaki bilgileri görüntüleme

Tek bir düğüm seçerseniz, aşağıdaki bilgileri görüntüleyebilirsiniz:

- bölge adı
- sunucu modeli
- Temel Kart Yönetim denetleyicisine (BMC) IP adresi
- İşlemsel durumu
- Toplam çekirdek sayısı
- toplam bellek miktarı
 
![Her düğüm için çalıştırma durumunu gösteren ölçek birimi kutucuğu](media/azure-stack-node-actions/NodeActions.PNG)

Ayrıca, buradan ölçek birimi düğüm eylemler gerçekleştirebilirsiniz.

## <a name="scale-unit-node-actions"></a>Ölçek birimi düğüm eylemleri

Bir ölçek birimi düğüm hakkında bilgi görüntülediğinizde gibi düğüm eylemleri de gerçekleştirebilirsiniz:

- Boşaltma ve devam et
- Onar

Hangi seçenekler kullanılabilir düğüm işletimsel durumunu belirler.

### <a name="power-off"></a>Kapatma

**Gücünün kapatılmasını** eylem düğümü devre dışı bırakır. Güç düğmesine basın, sanki aynı şeydir. Mevcut **değil** işletim sistemine bir kapatma sinyali gönderir. İşlemleri planlanmış kapatma için öncelikle bir ölçek birimi düğüm boşaltma emin olun.

Bu eylem, genellikle bir düğüm askıda ve artık isteklerine yanıt verip olduğunda kullanılır.

> [!Important] 
> Bu işlevsellik yalnızca PowerShell üzerinden kullanılabilir. Daha sonra yeniden Azure Stack Yönetici portalında kullanılabilir olacaktır.


PowerShell aracılığıyla eylem kapatmayı çalıştırmak için:

````PowerShell
  Stop-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
```` 

Eylem kapatma çalışmıyor olası durumda da, bunun yerine BMC web arabirimini kullanın.

### <a name="power-on"></a>Güç açma

**Açmayı** eylem düğümde kapatır. Güç düğmesine basın, sanki aynı şeydir. 

> [!Important] 
> Bu işlevsellik yalnızca PowerShell üzerinden kullanılabilir. Daha sonra yeniden Azure Stack Yönetici portalında kullanılabilir olacaktır.

PowerShell üzerinden eyleme power çalıştırmak için:

````PowerShell
  Start-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
````

Eylem gücüyle işe yaramazsa olası durumda da, bunun yerine BMC web arabirimini kullanın.

### <a name="drain"></a>Boşaltma

**Boşaltma** eylemi, belirli bir ölçek birimindeki kalan düğümler arasında dağıtarak tüm etkin iş yüklerini evacuates.

Bu eylem, genellikle bir düğümün tamamını değiştirme gibi parçaların alan değiştirme sırasında kullanılır.

> [!IMPORTANT]  
> Burada kullanıcılara bildirim yalnızca bir planlı bakım penceresi sırasında bir düğümü boşaltabilir emin olun. Bazı koşullar altında etkin iş yüklerini kesintiye uğraması oluşabilir.

PowerShell üzerinden boşaltma eylemi çalıştırmak için:

  ````PowerShell
  Disable-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
  ````

### <a name="resume"></a>Sürdür

**Sürdürme** eylem boşaltılmış düğümü sürdürür ve iş yükü yerleştirme için etkin olarak işaretler. Düğüm üzerinde çalışmakta olan bir önceki iş yüklerini geri başarısız. (Bir düğüm ve sonra güç kapalı, ne zaman yeniden düğüm güç tüketir, bu iş yükü yerleştirme için etkin olarak işaretlenmemiş. Hazır olduğunuzda, sürdürme eylemi düğümü etkin olarak işaretlemek için kullanmanız gerekir.)

PowerShell üzerinden sürdürme eylemi çalıştırmak için:

  ````PowerShell
  Enable-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
  ````

### <a name="repair"></a>Onar

**Onarım** eylemi, bir düğüm onarır. Bunu yalnızca birini aşağıdaki senaryolar için kullanın:

- Tam düğüm değiştirme (ile veya olmadan yeni veri diskleri)
- Donanım bileşeni hatası ve değiştirme (alan bulunduğu yerde değiştirilebilen biriminin (FRU) belgelerinde tavsiye varsa) sonra.

> [!IMPORTANT]  
> Bir düğüm ya da tek tek donanım bileşenleri değiştirmek gerektiğinde tam adımlar için OEM donanım satıcınızın FRU belgelerine bakın. FRU belgeleri bir donanım bileşenini değiştirdikten sonra onarım işlemi çalıştırmayı gerekip gerekmediğini belirtin.  

Onarım işlemi çalıştırdığınızda, BMC IP adresini belirtmeniz gerekir. 

PowerShell üzerinden Onarma eylemi çalıştırmak için:

  ````PowerShell
  Repair-AzsScaleUnitNode -Location <RegionName> -Name <NodeName> -BMCIPAddress <BMCIPAddress>
  ````

## <a name="next-steps"></a>Sonraki adımlar

Azure Stack Fabric yönetici modülü hakkında daha fazla bilgi için bkz: [Azs.Fabric.Admin](https://docs.microsoft.com/powershell/module/azs.fabric.admin/?view=azurestackps-1.4.0).