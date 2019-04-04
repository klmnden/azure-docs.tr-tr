---
title: Azure Stack'te birim düğüm eylemleri ölçeklendirme | Microsoft Docs
description: Düğüm durumunu görüntüleyin ve gücüyle güç kapalı kullanın, devre dışı bırakın ve bir Azure Stack tümleşik sisteminde düğüm eylemleri sürdürme hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 01/22/2019
ms.author: mabrigg
ms.reviewer: ppacent
ms.lastreviewed: 01/22/2019
ms.openlocfilehash: cd7e66961a0b9a80150a3d3e132efd29485cdb66
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58483157"
---
# <a name="scale-unit-node-actions-in-azure-stack"></a>Azure stack'teki ölçek birimi düğüm eylemleri

*Uygulama hedefi: Azure Stack tümleşik sistemleri*

Bu makalede, bir ölçek birimi durumunun nasıl görüntüleneceği açıklanır. Biriminin düğümleri görüntüleyebilirsiniz. Power gibi düğüm eylemleri, power çalıştırın devre dışı, Kapat, boşaltma, sürdürme ve onarın. Genellikle, bu düğüm eylemler sırasında alan değiştirme bölümlerinin ya da bir düğüm kurtarmanıza yardımcı olması için kullanın.

> [!Important]  
> Bu makalede açıklanan tüm düğüm Eylemler aynı anda tek bir düğüm hedeflemelidir.

## <a name="view-the-node-status"></a>Düğüm durumunu görüntüleme

Yönetici portalı'nda, bir ölçek birimi ve ilişkili düğümlerinin durumunu görüntüleyebilirsiniz.

Ölçek biriminin durumunu görüntülemek için:

1. Üzerinde **bölge Yönetimi** kutucuğunda, bölgeyi seçin.
2. Sol taraftaki altında **altyapı kaynaklarını**seçin **ölçek birimleri**.
3. Sonuçlarda ölçek birimi seçin.
4. Sol taraftaki altında **genel**seçin **düğümleri**.

   Aşağıdaki bilgileri görüntüle:

   - Tek tek düğümler listesi
   - İşlemsel durum (aşağıdaki listeye bakın)
   - Güç durumunu (çalışıyor veya durduruldu)
   - sunucu modeli
   - Temel Kart Yönetim denetleyicisine (BMC) IP adresi
   - Toplam çekirdek sayısı
   - toplam bellek miktarı

![bir ölçek birimi durumu](media/azure-stack-node-actions/multinodeactions.png)

### <a name="node-operational-states"></a>Düğüm işletimsel durumları

| Durum | Açıklama |
|----------------------|-------------------------------------------------------------------|
| Çalışıyor | Düğüm, etkin bir şekilde ölçek birimi katılıyor. |
| Durduruldu | Düğümü, kullanılamıyor. |
| Ekleniyor | Düğüm etkin bir şekilde Ölçek birimine ekleniyor. |
| Onarılıyor | Düğüm etkin bir şekilde onarılıyor. |
| Bakım | Düğüm duraklatılmadan ve hiçbir etkin kullanıcı iş yükü çalışıyor. |
| Düzeltmesi gerektiriyor | Onarılması düğümü gerektiren bir hata algılandı. |

## <a name="scale-unit-node-actions"></a>Ölçek birimi düğüm eylemleri

Bir ölçek birimi düğüm hakkında bilgi görüntülediğinizde gibi düğüm eylemleri de gerçekleştirebilirsiniz:
 - Başlatma ve durdurma (bağlı olarak geçerli güç durumu)
 - Devre dışı bırakın ve sürdürme (işlemlerin durumu bağlı olarak)
 - Onar
 - Kapat

Hangi seçenekler kullanılabilir düğüm işletimsel durumunu belirler.

Azure Stack PowerShell modül yüklemeniz gerekir. Bu cmdlet'ler bulunan **Azs.Fabric.Admin** modülü. Yükleme veya Azure Stack için PowerShell yüklemesini doğrulamak için bkz: [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md).

## <a name="stop"></a>Durdur

**Durdur** eylem düğümü devre dışı bırakır. Güç düğmesine basın, sanki aynı şeydir. İşletim sistemine bir kapatma sinyali göndermez. Planlanan durdurma işlemlerinde, her zaman önce kapatma işlemi deneyin. 

Bu eylem, genellikle bir düğüm askıda ve artık isteklerine yanıt verip olduğunda kullanılır.

Durdurma eylemi çalıştırmak için yükseltilmiş bir PowerShell istemi açın ve aşağıdaki cmdlet'i çalıştırın:

```powershell  
  Stop-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
```

Olası durumda durdurma eylemi iş değil, işlemi yeniden deneyin ve ikinci kez başarısız olursa BMC web arabirimini kullanın.

Daha fazla bilgi için [Stop-AzsScaleUnitNode](https://docs.microsoft.com/powershell/module/azs.fabric.admin/stop-azsscaleunitnode).

## <a name="start"></a>Başlatma

**Başlat** eylem düğümde kapatır. Güç düğmesine basın, sanki aynı şeydir. 
 
Başlangıç eylemi çalıştırmak için yükseltilmiş bir PowerShell istemi açın ve aşağıdaki cmdlet'i çalıştırın:

```powershell  
  Start-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
```

Olası durumda başlangıç eylemi iş değil, işlemi yeniden deneyin ve ikinci kez başarısız olursa BMC web arabirimini kullanın.

Daha fazla bilgi için [başlangıç AzsScaleUnitNode](https://docs.microsoft.com/powershell/module/azs.fabric.admin/start-azsscaleunitnode).

## <a name="drain"></a>Boşaltma

**Boşaltma** eylem kalan düğümleri, belirli bir ölçek birimindeki tüm etkin iş yüklerini taşır.

Bu eylem, genellikle bir düğümün tamamını değiştirme gibi parçaların alan değiştirme sırasında kullanılır.

> [!Important]
> Bir düğüm boşaltma işlemi burada kullanıcılara bildirim planlı bakım penceresi sırasında kullandığınızdan emin olun. Bazı koşullar altında etkin iş yüklerini kesintiye uğraması oluşabilir.

Boşaltma eylemi çalıştırmak için yükseltilmiş bir PowerShell istemi açın ve aşağıdaki cmdlet'i çalıştırın:

```powershell  
  Disable-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
```

Daha fazla bilgi için [devre dışı bırak AzsScaleUnitNode](https://docs.microsoft.com/powershell/module/azs.fabric.admin/disable-azsscaleunitnode).

## <a name="resume"></a>Sürdür

**Sürdürme** eylem devre dışı bırakılmış bir düğümü sürdürür ve iş yükü yerleştirme için etkin olarak işaretler. Düğüm üzerinde çalışmakta olan bir önceki iş yüklerini geri başarısız. (Bir düğüm boşaltma işlemi kullanırsanız kapatmasına emin olun. Düğümü yeniden açtığında, iş yükü yerleştirme için etkin olarak işaretlenmemiş. Hazır olduğunuzda, sürdürme eylemi düğümü etkin olarak işaretlemek için kullanmanız gerekir.)

Sürdürme eylemi çalıştırmak için yükseltilmiş bir PowerShell istemi açın ve aşağıdaki cmdlet'i çalıştırın:

```powershell  
  Enable-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
```

Daha fazla bilgi için [etkinleştir AzsScaleUnitNode](https://docs.microsoft.com/powershell/module/azs.fabric.admin/enable-azsscaleunitnode).

## <a name="repair"></a>Onar

**Onarım** eylemi, bir düğüm onarır. Bunu yalnızca birini aşağıdaki senaryolar için kullanın:
 - Tam düğüm değiştirme (ile veya olmadan yeni veri diskleri)
 - Donanım bileşeni hatası ve değiştirme (alan bulunduğu yerde değiştirilebilen biriminin (FRU) belgelerinde tavsiye varsa) sonra.

> [!Important]  
> Bir düğüm ya da tek tek donanım bileşenleri değiştirmek gerektiğinde tam adımlar için OEM donanım satıcınızın FRU belgelerine bakın. FRU belgeleri bir donanım bileşenini değiştirdikten sonra onarım işlemi çalıştırmayı gerekip gerekmediğini belirtin. 

Onarım işlemi çalıştırdığınızda, BMC IP adresini belirtmeniz gerekir. 

Onarım işlemi çalıştırmak için yükseltilmiş bir PowerShell istemi açın ve aşağıdaki cmdlet'i çalıştırın:

  ```powershell
  Repair-AzsScaleUnitNode -Location <RegionName> -Name <NodeName> -BMCIPv4Address <BMCIPv4Address>
  ```

## <a name="shutdown"></a>Kapat

**Kapatma** eylem fist kalan düğümleri aynı ölçek birimindeki tüm etkin iş yüklerini taşır. Ardından eylem düzgün bir şekilde ölçek birimi düğümlerini kapatır.

Kapatıldı bir düğüm başlattıktan sonra çalıştırmanız gerekir [sürdürme](#resume) eylem. Düğüm üzerinde çalışmakta olan bir önceki iş yüklerini geri başarısız.

Kapatma işlemi başarısız olursa, deneme [boşaltma](#drain) işlemi kapatma işlemi tarafından izlenen.

Kapatma eylemi çalıştırmak için yükseltilmiş bir PowerShell istemi açın ve aşağıdaki cmdlet'i çalıştırın:

  ```powershell
  Stop-AzsScaleUnitNode -Location <RegionName> -Name <NodeName> -Shutdown
  ```



## <a name="next-steps"></a>Sonraki adımlar

Azure Stack Fabric yönetici modülü hakkında daha fazla bilgi için bkz: [Azs.Fabric.Admin](https://docs.microsoft.com/powershell/module/azs.fabric.admin/?view=azurestackps-1.6.0).
