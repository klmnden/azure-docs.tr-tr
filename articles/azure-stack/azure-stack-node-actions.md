---
title: Birim düğümü Eylemler Azure yığınında ölçeklendirin | Microsoft Docs
description: Düğüm durumunu görüntüleyin ve güç, kapatma, boşaltma kullanın ve düğüm Eylemler bir Azure tümleşik yığını sistemde sürdürme hakkında bilgi edinin.
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
ms.date: 05/10/2018
ms.author: mabrigg
ms.openlocfilehash: 202854157dee28f3ab3dc73c6f22508a8bf510b3
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="scale-unit-node-actions-in-azure-stack"></a>Ölçek birimi düğümü Eylemler Azure yığınında

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Bu makalede, bir ölçek birimi ve ilişkili düğümlerini durumunu görüntülemek nasıl ve kullanılabilir düğüm Eylemler kullanmayı açıklar. Düğüm Eylemler gücüyle, güç kapalı dahil boşaltma sürdürmek ve onarın. Genellikle, alan değiştirmesi bölümlerinin veya düğüm kurtarma senaryoları için sırasında bu düğüm eylemlerini kullanın.

> [!Important]  
> Bu makalede açıklanan tüm düğüm Eylemler aynı anda yalnızca hedef bir düğüm olmalıdır.


## <a name="view-the-status-of-a-scale-unit-and-its-nodes"></a>Bir ölçek birimi ve düğümlerini durumunu görüntüleme

Yönetici portalı'nda bir ölçek birimi ve ilişkili düğümlerini durumunu kolayca görüntüleyebilirsiniz.

Bir ölçek birimi durumunu görüntülemek için:

1. Üzerinde **bölge Yönetimi** döşeme, bölgeyi seçin.
2. Solda, altında **altyapı kaynaklarıdır**seçin **ölçek birimleri**.
3. Sonuçlarda ölçek birimi seçin.
 
Burada, aşağıdaki bilgileri görüntüleyebilirsiniz:

- Bölge adı
- tür sistemi
- Toplam mantıksal çekirdekler
- toplam bellek
- tek düğümler ve durumlarının listesi; Çalışan veya durduruldu.

![Her düğüm için çalışan durumunu gösteren ölçek birimi döşeme](media/azure-stack-node-actions/ScaleUnitStatus.PNG)

## <a name="view-information-about-a-scale-unit-node"></a>Bir ölçek birimi düğüm hakkındaki bilgileri görüntüleme

Tek bir düğüm seçerseniz, aşağıdaki bilgileri görüntüleyebilirsiniz:

- Bölge adı
- sunucu modeli
- Temel kart yönetim denetleyicisi (BMC) IP adresi
- İşlemsel durumu
- Çekirdek toplam sayısı
- toplam bellek miktarı
 
![Her düğüm için çalışan durumunu gösteren ölçek birimi döşeme](media/azure-stack-node-actions/NodeActions.PNG)

Ayrıca, buradan ölçek birimi düğümü eylemleri de gerçekleştirebilirsiniz.

## <a name="scale-unit-node-actions"></a>Ölçek birimi düğümü Eylemler

Bir ölçek birimi düğümü bilgilerini görüntülediğinizde de düğüm eylemleri gibi gerçekleştirebilirsiniz:

- açma ve kapatma
- boşaltma ve sürdürme
- onar

Düğüm işlemsel durumu hangi seçeneklerin kullanılabilir olduğunu belirler.

### <a name="power-off"></a>Kapatma

**Kapatmak** eylem düğümü devre dışı bırakır. Güç düğmesine basın gibi aynı olur. Mevcut **değil** işletim sistemine bir kapatma sinyali göndermek. Operations planlı kapatma için öncelikle bir ölçek birimi düğüm boşaltma emin olun.

Bu eylem genellikle bir düğüm askıdaki durumda ve artık isteklerine yanıt kullanılır.

> [!Important] 
> Bu işlevsellik yalnızca PowerShell ile kullanılabilir. Daha sonra yeniden Azure yığın Yönetici portalı'nda kullanılabilir olacaktır.


Eylem PowerShell aracılığıyla kapatma çalıştırmak için:

````PowerShell
  Stop-AzsScaleUnitNode -Region <RegionName> -Name <NodeName>
```` 

Eylem kapatma işe yaramazsa olası durumda da, bunun yerine BMC web arabirimi kullanın.

### <a name="power-on"></a>Açma

**Üzerinde güç** eylem düğümde kapatır. Güç düğmesine basın gibi aynı olur. 

> [!Important] 
> Bu işlevsellik yalnızca PowerShell ile kullanılabilir. Daha sonra yeniden Azure yığın Yönetici portalı'nda kullanılabilir olacaktır.

PowerShell aracılığıyla eylemi güç çalıştırmak için:

````PowerShell
  Start-AzsScaleUnitNode -Region <RegionName> -Name <NodeName>
````

Eylem gücüyle işe yaramazsa olası durumda da, bunun yerine BMC web arabirimi kullanın.

### <a name="drain"></a>Boşaltma

**Boşaltma** eylem bu belirli ölçek biriminde kalan düğümleri arasında dağıtarak tüm etkin iş yükleri evacuates.

Bu eylem, genellikle tüm düğüm değiştirme gibi bölümlerinin alanı değiştirmesi sırasında kullanılır.

> [!IMPORTANT]
> Burada kullanıcılara bildirimde yalnızca bir planlı bakım penceresi sırasında bir düğüm boşaltma emin olun. Bazı koşullarda, etkin iş yükleri kesintiye uğraması yaşayabilirsiniz.

PowerShell aracılığıyla boşaltma eylemi çalıştırmak için:

  ````PowerShell
  Disable-AzsScaleUnitNode -Region <RegionName> -Name <NodeName>
  ````

### <a name="resume"></a>Sürdür

**Sürdür** eylem boşaltılmış düğümü sürdürür ve iş yükü yerleştirme için etkin olarak işaretler. Düğüm üzerinde çalışmakta olan önceki iş yükleri geri başarısız. (Bir düğüm ve sonra kapalı, düğüm geri güç zaman güç boşaltma, bu iş yükü yerleştirme etkin olarak işaretlenmez. Hazır olduğunuzda, Sürdür eylemi düğüm etkin olarak işaretlemek için kullanmanız gerekir.)

PowerShell aracılığıyla Sürdür eylemi çalıştırmak için:

  ````PowerShell
  Enable-AzsScaleUnitNode -Region <RegionName> -Name <NodeName>
  ````

### <a name="repair"></a>Onar

**Onarım** eylemi bir düğüm onarır. Bunu yalnızca birini aşağıdaki senaryolar için kullanın:

- Tam düğümü değiştirme (olan veya olmayan yeni veri diskleri)
- Donanım bileşeni hatası sonra (alan değiştirilebilen biriminin (FRU) belgelerinde tavsiye varsa) değiştirme.

> [!IMPORTANT]
> Bir düğüm ya da tek tek donanım bileşenleri değiştirmek gerektiğinde tam adımlar için OEM donanım satıcısının FRU belgelerine bakın. FRU belgelerine donanım bileşeni değiştirildikten sonra onarma eylemini çalıştırın gerekip gerekmediğini belirtir.  

Onarım işlemi çalıştırdığınızda, BMC IP adresini belirtmeniz gerekir. 

PowerShell aracılığıyla Onarma eylemi çalıştırmak için:

  ````PowerShell
  Repair-AzsScaleUnitNode -Region <RegionName> -Name <NodeName> -BMCIPAddress <BMCIPAddress>
  ````


