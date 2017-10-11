---
title: "StorSimple Cihazınızı güncelleştirin | Microsoft Docs"
description: "StorSimple güncelleştirme özelliğini normal ve Bakım modu güncelleştirmeleri ve düzeltmeleri yüklemeniz için nasıl kullanılacağını açıklar."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 786059f5-2a38-4105-941d-0860ce4ac515
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/18/2016
ms.author: v-sharos
ms.openlocfilehash: 8490110942741b049b6d44ac93697303cef40e8a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="update-your-storsimple-8000-series-device"></a>StorSimple 8000 serisi Cihazınızı güncelleştirin
## <a name="overview"></a>Genel Bakış
StorSimple güncelleştirme özellikler kolayca StorSimple cihazınız güncel tutmanıza olanak sağlar. Güncelleştirme türüne bağlı olarak, Windows PowerShell arabirimi üzerinden veya Klasik Azure Portalı aracılığıyla aygıta güncelleştirmelerini uygulayabilirsiniz. Bu öğretici, güncelleştirme türlerini ve bunların her birine yüklemek açıklar.

Cihaz güncelleştirmeleri iki tür uygulayabilirsiniz: 

* Normal (veya Normal modda) güncelleştirmeleri
* Bakım modu güncelleştirmeleri

Klasik Azure portalı veya Windows PowerShell aracılığıyla düzenli olarak güncelleştirmeleri yükleyebilirsiniz; Ancak, Bakım modu güncelleştirmeleri yüklemek için Windows PowerShell kullanmanız gerekir. 

Ayrıca, aşağıda açıklanan her güncelleştirme türüdür.

### <a name="regular-updates"></a>Düzenli olarak güncelleştirmeleri
Normal cihaz Normal modda olduğunda, yüklenebilen benzer güncelleştirmelerdir. Bu güncelleştirmeleri Microsoft Update Web sitesi aracılığıyla her aygıt denetleyiciye uygulanır. 

> [!IMPORTANT]
> Denetleyici yük devretmesi güncelleştirme işlemi sırasında ortaya çıkabilir. Ancak, bu sistem kullanılabilirliğini veya işlem etkilemez.
> 
> 

* Klasik Azure Portalı aracılığıyla normal güncelleştirmelerinin nasıl yükleneceği hakkında daha fazla bilgi için bkz: [Klasik Azure Portalı aracılığıyla normal güncelleştirmelerini yükleme](#install-regular-updates-via-the-azure-classic-portal).
* StorSimple için Windows PowerShell aracılığıyla düzenli olarak güncelleştirmeleri de yükleyebilirsiniz. Ayrıntılar için bkz [StorSimple için Windows PowerShell aracılığıyla normal güncelleştirmelerini yükleme](#install-regular-updates-via-windows-powershell-for-storsimple).

### <a name="maintenance-mode-updates"></a>Bakım modu güncelleştirmeleri
Bakım modu kesintiye uğratan disk bellenim yükseltmeleri gibi güncelleştirmelerdir. Bu güncelleştirmeler bakım moduna aygıta gerektirir. Ayrıntılar için bkz [2. adım: girin Bakım modu](#step2). Bakım modu güncelleştirmeleri yüklemek için Azure Klasik portalı kullanamazsınız. Bunun yerine, StorSimple için Windows PowerShell kullanmanız gerekir. 

Bakım modu güncelleştirmelerinin nasıl yükleneceği hakkında daha fazla bilgi için bkz: [yükleme Bakım modu güncelleştirmeleri StorSimple için Windows PowerShell aracılığıyla](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).

> [!IMPORTANT]
> Bakım modu güncelleştirmeleri ayrı ayrı her denetleyiciye uygulanmış olması gerekir. 
> 
> 

## <a name="install-regular-updates-via-the-azure-classic-portal"></a>Klasik Azure Portalı aracılığıyla düzenli olarak güncelleştirmeleri yükle
StorSimple Cihazınızı güncelleştirmeleri uygulamak için Klasik Azure portalını kullanabilirsiniz.

[!INCLUDE [storsimple-install-updates-manually](../../includes/storsimple-install-updates-manually.md)]

## <a name="install-regular-updates-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell aracılığıyla düzenli olarak güncelleştirmeleri yükle
Alternatif olarak, normal (Normal modda) güncelleştirmeleri uygulamak için StorSimple için Windows PowerShell'i kullanabilirsiniz.

> [!IMPORTANT]
> StorSimple için Windows PowerShell kullanarak düzenli olarak güncelleştirmeleri yükleyebilmenize rağmen Klasik Azure Portalı aracılığıyla düzenli olarak güncelleştirmeleri yüklemenizi öneririz. Güncelleştirme 1'den başlayarak, ön denetimleri portaldan güncelleştirmeleri yüklemeden önce gerçekleştirilir. Bu ön denetimleri hatalarını önüne geçer ve daha sorunsuz bir deneyim emin olun. 
> 
> 

[!INCLUDE [storsimple-install-regular-updates-powershell](../../includes/storsimple-install-regular-updates-powershell.md)]

## <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell aracılığıyla Bakım modu güncelleştirmeleri yükle
StorSimple Cihazınızı Bakım modu güncelleştirmeleri uygulamak için StorSimple için Windows PowerShell kullanın. Tüm g/ç istekleri bu modda duraklatıldı. Geçici olmayan rasgele erişim belleği (NVRAM) gibi hizmetleri veya Küme hizmeti de durdurulur. Girin ya da bu modundan çıkmak hem denetleyicileri yeniden başlatılır. Bu mod çıktığınızda, tüm hizmetleri devam eder ve iyi durumda olmalıdır. (Bu işlem birkaç dakika sürebilir.)

Bakım modu güncelleştirmeleri uygulamak gerekiyorsa, Klasik Azure Portalı aracılığıyla yüklenmesi gereken güncelleştirmelerin yüklü olduğundan uyarı alırsınız. Bu uyarı güncelleştirmeleri yüklemek için StorSimple için Windows PowerShell kullanma yönergeleri içerir. Cihazınızı güncelleştirdikten sonra aygıtın normal moduna geçmek için aynı yordamı kullanın. Adım adım yönergeler için bkz: [4. adım: çıkış Bakım modu](#step4).

> [!IMPORTANT]
> * Bakım modu girmeden önce denetleyerek hem de cihaz denetleyicilerinin sağlıklı olduğunu doğrula **donanım durum** üzerinde **Bakım** Klasik Azure portalındaki sayfası. Denetleyicisi iyi durumda değil, Microsoft Support sonraki adımlar için başvurun. Daha fazla bilgi için ilgili kişi Microsoft Desteği'ne gidin. 
> * Bakım modunda olduğunda, önce bir denetleyici ve ardından diğer denetleyici güncelleştirmesini gerekir.
> 
> 

### <a name="step-1-connect-to-the-serial-console-a-namestep1"></a>1. adım: seri konsoluna Bağlan<a name="step1">
İlk olarak, seri konsoluna erişmek için PuTTY gibi bir uygulama kullanın. Aşağıdaki yordamda, seri konsoluna bağlanmak için PuTTY kullanın açıklanmaktadır.

[!INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="step-2-enter-maintenance-mode-a-namestep2"></a>2. adım: Bakım modu girin<a name="step2">
Konsola bağlandıktan sonra yüklemek ve bunları yüklemek için bakım moduna girmek için güncelleştirmeler olup olmadığını belirleyin.

[!INCLUDE [storsimple-enter-maintenance-mode](../../includes/storsimple-enter-maintenance-mode.md)]

### <a name="step-3-install-your-updates-a-namestep3"></a>3. adım: güncellemelerinizi yükleme<a name="step3">
Ardından, güncelleştirmelerinizi yükleyin.

[!INCLUDE [storsimple-install-maintenance-mode-updates](../../includes/storsimple-install-maintenance-mode-updates.md)]

### <a name="step-4-exit-maintenance-mode-a-namestep4"></a>4. adım: Çıkış Bakım modu<a name="step4">
Son olarak, bakım modundan çıkın.

[!INCLUDE [storsimple-exit-maintenance-mode](../../includes/storsimple-exit-maintenance-mode.md)]

## <a name="install-hotfixes-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell aracılığıyla düzeltmeleri yükleme
Microsoft Azure StorSimple güncelleştirmeleri, paylaşılan bir klasörden düzeltmeleri yüklenir. Güncelleştirmelerinde olduğu gibi düzeltmeler iki tür vardır: 

* Normal düzeltmeleri 
* Bakım modu düzeltmeleri  

Aşağıdaki yordamlarda, StorSimple için Windows PowerShell normal ve Bakım modu düzeltmeleri yüklemeniz için nasıl kullanılacağı açıklanmaktadır.

[!INCLUDE [storsimple-install-regular-hotfixes](../../includes/storsimple-install-regular-hotfixes.md)]

[!INCLUDE [storsimple-install-maintenance-mode-hotfixes](../../includes/storsimple-install-maintenance-mode-hotfixes.md)]

## <a name="what-happens-to-updates-if-you-perform-a-factory-reset-of-the-device"></a>Bir cihazı fabrika ayarlarına gerçekleştirmek güncelleştirmeleri ne olur?
Bir cihazı fabrika ayarlarına sıfırlarsanız tüm güncelleştirmeler kaybolur. Fabrika sıfırlaması cihaz kayıtlı yapılandırıldıktan sonra Klasik Azure portalı ve/veya Windows PowerShell aracılığıyla güncelleştirmeleri StorSimple için el ile yüklemeniz gerekir. Fabrika sıfırlaması hakkında daha fazla bilgi için bkz: [cihazı fabrika varsayılan ayarlarına sıfırlama](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [StorSimple Cihazınızı yönetmek için StorSimple için Windows PowerShell kullanarak](storsimple-windows-powershell-administration.md).
* Daha fazla bilgi edinmek [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

