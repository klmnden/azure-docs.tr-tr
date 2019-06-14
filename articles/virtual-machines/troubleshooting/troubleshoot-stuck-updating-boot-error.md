---
title: Azure VM başlatma sırasında Windows Update takılı | Microsoft Docs
description: Bir Azure VM başlatma sırasında Windows takıldığında sorun gidermeyi öğrenin güncelleştirin.
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: v-jesits
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/09/2018
ms.author: genli
ms.openlocfilehash: cff1577eacd0af86d3ad1c99e1eb2164b64318c4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60443781"
---
# <a name="azure-vm-startup-is-stuck-at-windows-update"></a>Azure VM başlatma sırasında Windows takılı güncelleştir

Bu makalede, sanal makinenize (VM) başlatma sırasında Windows Update aşamada takıldığında sorunu yardımcı olur. 

> [!NOTE] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Resource Manager dağıtım modelini incelemektedir. Bu model Klasik dağıtım modeli kullanılarak yerine yeni dağıtımlar için kullanmanızı öneririz.

## <a name="symptom"></a>Belirti

 Bir Windows VM başlamaz. Ne zaman iade ekran görüntüleri [önyükleme tanılaması](../troubleshooting/boot-diagnostics.md) penceresinde görürsünüz başlangıç güncelleştirme sürecinde takılı kalıyor. Alabileceğiniz iletileri örnekleri şunlardır:

- Yükleme Windows ##% bilgisayarınızı kapatmayın. Bu işlem biraz sürebilir koruyun, birkaç kez yeniden başlatılacak
- Bu işlemi tamamlanana kadar bilgisayarınıza takip edin. # Sayısı güncelleştirme yükleniyor... 
- Biz değişiklikler geri alınıyor, bilgisayarınızı kapatmayın güncelleştirmeleri tamamlanamadı
- Hata Yapılandırma Windows güncelleştirmeleri Reverting değişiklikleri bilgisayarınızı kapatmayın
- Güncelleştirme işlemi uygulama hatası < hata kodu > ###, ### (\Regist...)
- Önemli hata güncelleştirme işlemleri uygulayarak < hata kodu > ###, ### ($$...)


## <a name="solution"></a>Çözüm

Alıyorsanız güncelleştirmeleri sayısına bağlı olarak yüklü veya yedekleme toplu güncelleştirme işlemi biraz zaman alabilir. VM için 8 saat bu durumda bırakın. VM, yine de bu durumda, bu süreden sonra ise, Azure portalında VM'yi yeniden başlatın ve normalde başlatabilmeniz için bkz. Bu adım işe yaramazsa, aşağıdaki çözümleri deneyin.

### <a name="remove-the-update-that-causes-the-problem"></a>Soruna neden olan güncelleştirmeyi kaldırın

1. Etkilenen sanal makinenin işletim sistemi diskinin anlık yedekleyin. Daha fazla bilgi için [bir diskin anlık görüntüsünü alma](../windows/snapshot-copy-managed-disk.md). 
2. [İşletim sistemi diskini bir kurtarma VM'si ekleme](troubleshoot-recovery-disks-portal-windows.md).
3. İşletim sistemi diski kurtarma sanal makinesinde bağlandıktan sonra Çalıştır **diskmgmt.msc** Disk Yönetimi'ni açın ve bağlı disk olduğundan emin olmak için **çevrimiçi**. Ekli işletim sistemi diskinin \windows klasörünü bulunduran atanan sürücü harfini not alın. Disk şifreli değilse, bu belgenin sonraki adımlar ile devam etmeden önce disk şifresini çözemez.

4. Yükseltilmiş bir komut istemi (yönetici olarak çalıştır) örneği açın. Ekli işletim sistemi diskinde güncelleştirme paketleri listesini almak için aşağıdaki komutu çalıştırın:

        dism /image:<Attached OS disk>:\ /get-packages > c:\temp\Patch_level.txt

    Örneğin, F sürücü ekli işletim sistemi diski ise aşağıdaki komutu çalıştırın:

        dism /image:F:\ /get-packages > c:\temp\Patch_level.txt
5. C:\temp\Patch_level.txt dosyasını açın ve ardından alt okumak. İçinde güncelleştirme bulun **yükleme bekletiliyor** veya **kaldırma bekleyen** durumu.  Güncelleştirme durumu, bir örnek verilmiştir:

     ```
    Package Identity : Package_for_RollupFix~31bf3856ad364e35~amd64~~17134.345.1.5
    State : Install Pending
    Release Type : Security Update
    Install Time :
    ```
6. Sorunun nedeni güncelleştirmeyi kaldırın:
    
    ```
    dism /Image:<Attached OS disk>:\ /Remove-Package /PackageName:<PACKAGE NAME TO DELETE>
    ```
    Örnek: 

    ```
    dism /Image:F:\ /Remove-Package /PackageName:Package_for_RollupFix~31bf3856ad364e35~amd64~~17134.345.1.5
    ```

    > [!NOTE] 
    > Paket boyutuna bağlı olarak, DISM aracı yüklemeyi işlemek için biraz zaman alır. Normal işlem 16 dakika içinde tamamlanır.

7. [İşletim sistemi diskini ve VM yeniden](troubleshoot-recovery-disks-portal-windows.md#unmount-and-detach-original-virtual-hard-disk). Daha sonra sorun çözülmüş olup olmadığını denetleyin.
