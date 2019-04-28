---
title: StorSimple Cihazınızı güncelleştirme | Microsoft Docs
description: StorSimple güncelleştirme özelliğini normal ve Bakım modu güncelleştirmeleri ve düzeltmeleri yüklemeniz için nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: ''
ms.assetid: 786059f5-2a38-4105-941d-0860ce4ac515
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/23/2018
ms.author: v-sharos
ms.openlocfilehash: d973a16c121a1e8ebee10826d135bcbb33ef748c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61409996"
---
# <a name="update-your-storsimple-8000-series-device"></a>StorSimple 8000 serisi Cihazınızı güncelleştirin
> [!NOTE]
> StorSimple için klasik portal kullanım dışıdır. StorSimple Cihaz Yöneticileriniz, yeni Azure portalına kullanımdan kaldırma zamanlamasına göre otomatik olarak taşınacaktır. Bu taşımayla ilgili bir e-posta ve portal bildirimi alacaksınız. Bu belge de yakında kullanımdan kaldırılacaktır. Taşıma hakkında tüm sorularınız için bkz. [SSS: Azure portalına taşıma](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Genel Bakış
StorSimple güncelleştirme özellikleri, diğer bir kolayca StorSimple cihazınızın güncel tutmak imkan tanır. Güncelleştirme türüne bağlı olarak, Klasik Azure Portalı aracılığıyla ya da Windows PowerShell arabirimi üzerinden cihaz güncelleştirmeleri uygulayabilirsiniz. Bu öğretici, güncelleştirme türlerini ve bunların her birini yüklemeyi açıklar.

İki tür cihaz güncelleştirmeleri uygulayabilirsiniz: 

* Normal (veya Normal Mod) güncelleştirmeleri
* Bakım modu güncelleştirmeleri

Klasik Azure portalı veya Windows PowerShell aracılığıyla düzenli güncelleştirmeler yükleyebilirsiniz; Ancak, Bakım modu güncelleştirmeleri yüklemek için Windows PowerShell kullanmanız gerekir. 

Ayrıca, aşağıda açıklandığı gibi her güncelleştirme türüdür.

### <a name="regular-updates"></a>Düzenli güncelleştirmeler
Düzenli güncelleştirmeler cihaz Normal modda olduğunda, yüklenebilen kesintiye uğratmayan güncelleştirmelerin. Bu güncelleştirmeleri Microsoft Update Web sitesi üzerinden her bir cihaz denetleyicisi için uygulanır. 

> [!IMPORTANT]
> Denetleyici yük devretmesi, güncelleştirme işlemi sırasında meydana gelebilir. Ancak, bu işlemi ya da sistem kullanılabilirliği etkilemez.
> 
> 

* Klasik Azure Portalı aracılığıyla düzenli olarak güncelleştirmeleri yükleme hakkında daha fazla ayrıntı için bkz. [yükleme Klasik Azure Portalı aracılığıyla düzenli güncelleştirmeler](#install-regular-updates-via-the-azure-classic-portal).
* StorSimple için Windows PowerShell aracılığıyla düzenli güncelleştirmeler de yükleyebilirsiniz. Ayrıntılar için bkz [StorSimple için Windows PowerShell aracılığıyla düzenli güncelleştirmeler yüklemek](#install-regular-updates-via-windows-powershell-for-storsimple).

### <a name="maintenance-mode-updates"></a>Bakım modu güncelleştirmeleri
Bakım modu güncelleştirmeleri, disk üretici yazılımını yükseltme gibi kesintiye uğratan güncelleştirmelerdir. Bu güncelleştirmeler, cihaz, bakım moduna yerleştirilecek gerektirir. Ayrıntılar için bkz [2. adım: Bakım Moduna gir](#step2). Bakım modu güncelleştirmeleri yüklemek için Azure Klasik portalı kullanamazsınız. Bunun yerine, StorSimple için Windows PowerShell kullanmanız gerekir. 

Bakım modu güncelleştirmeleri yükleme hakkında daha fazla ayrıntı için bkz. [yükleme Bakım modu güncelleştirmeleri StorSimple için Windows PowerShell aracılığıyla](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).

> [!IMPORTANT]
> Bakım modu güncelleştirmeleri her denetleyici için ayrı ayrı uygulanması gerekir. 
> 
> 

## <a name="install-regular-updates-via-the-azure-classic-portal"></a>Klasik Azure Portalı aracılığıyla düzenli güncelleştirmeler yükleyin
Klasik Azure portalında StorSimple Cihazınızı güncelleştirmeleri uygulamak için kullanabilirsiniz.

[!INCLUDE [storsimple-install-updates-manually](../../includes/storsimple-install-updates-manually.md)]

## <a name="install-regular-updates-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell aracılığıyla düzenli güncelleştirmeler yükleyin
Alternatif olarak, normal (Normal Mod) güncelleştirmeleri uygulamak için StorSimple için Windows PowerShell kullanabilirsiniz.

> [!IMPORTANT]
> StorSimple için Windows PowerShell kullanarak düzenli güncelleştirmeler yükleyebilmenize rağmen Klasik Azure Portalı aracılığıyla düzenli güncelleştirmeler yüklemeniz önerilir. Güncelleştirme 1'den başlayarak, ön denetimleri portaldan güncelleştirmeleri yüklemeden önce gerçekleştirilir. Bu ön denetimleri hatalarını etkisiz hale ve daha sorunsuz bir deneyim emin olun. 
> 
> 

[!INCLUDE [storsimple-install-regular-updates-powershell](../../includes/storsimple-install-regular-updates-powershell.md)]

## <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell aracılığıyla Bakım modu güncelleştirmeleri yükle
StorSimple için Windows PowerShell, StorSimple cihazınız için Bakım modu güncelleştirmeleri uygulamak için kullanın. Tüm g/ç istekleri bu modda duraklatıldı. Geçici olmayan rastgele erişim belleği (NVRAM) gibi hizmetler veya Küme hizmetini de durduruldu. Her iki denetleyicilerinin girin ya da bu modundan çıkmak yeniden başlatılır. Bu mod çıktığınızda tüm hizmetleri devam eder ve iyi durumda olması gerekir. (Bu işlem birkaç dakika sürebilir.)

Bakım modu güncelleştirmeleri uygulamak gerekiyorsa, Klasik Azure Portalı aracılığıyla yüklenmesi gereken güncelleştirmelerin sahip uyarı alırsınız. Bu uyarı güncelleştirmeleri yüklemek üzere StorSimple için Windows PowerShell kullanımıyla ilgili yönergeleri içerir. Cihazınızı güncelleştirmeden sonra cihaz normal moda geçmek için aynı yordamı kullanın. Adım adım yönergeler için bkz: [4. adım: Bakım modundan çık](#step4).

> [!IMPORTANT]
> * Bakım modu girmeden önce kontrol ederek her iki cihaz denetleyicilerinin sağlıklı olduğunu doğrulayın **donanım durumunu** üzerinde **Bakım** Klasik Azure portalında sayfa. Denetleyicinin sağlam değilse Microsoft Support için sonraki adımlara başvurun. Daha fazla bilgi için ilgili Microsoft Desteği'ne gidin. 
> * Bakım modunda olduğunda, güncelleştirme, öncelikle bir denetleyici ve ardından diğer denetleyiciye uygulamanız gerekir.
> 
> 

### <a name="step-1-connect-to-the-serial-console-a-namestep1"></a>1. Adım: Seri konsoluna Bağlan <a name="step1">
İlk olarak, seri konsoluna erişmek için PuTTY gibi bir uygulama kullanın. Aşağıdaki yordam seri konsoluna bağlanmak için PuTTY kullanın açıklanmaktadır.

[!INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="step-2-enter-maintenance-mode-a-namestep2"></a>2. Adım: Bakım Moduna gir <a name="step2">
Konsola bağlandıktan sonra yüklemek ve bunları yüklemek için bakım moduna girmek için güncelleştirmeler olup olmadığını belirler.

[!INCLUDE [storsimple-enter-maintenance-mode](../../includes/storsimple-enter-maintenance-mode.md)]

### <a name="step-3-install-your-updates-a-namestep3"></a>3. Adım: Güncelleştirmelerinizi yükleyin <a name="step3">
Ardından, güncelleştirmelerinizi yükleyin.

[!INCLUDE [storsimple-install-maintenance-mode-updates](../../includes/storsimple-install-maintenance-mode-updates.md)]

### <a name="step-4-exit-maintenance-mode-a-namestep4"></a>4. Adım: Bakım modundan çık <a name="step4">
Son olarak, bakım modundan çıkın.

[!INCLUDE [storsimple-exit-maintenance-mode](../../includes/storsimple-exit-maintenance-mode.md)]

## <a name="install-hotfixes-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell aracılığıyla düzeltmelerini
Microsoft Azure StorSimple güncelleştirmeleri, düzeltmeler bir paylaşılan klasöre yüklenir. Güncelleştirmelerinde olduğu gibi düzeltmeleri iki tür vardır: 

* Normal düzeltmeler 
* Bakım modu düzeltmelerini  

Aşağıdaki yordamlarda, normal ve Bakım modu düzeltmelerini yüklemek için StorSimple için Windows PowerShell kullanma açıklanmaktadır.

[!INCLUDE [storsimple-install-regular-hotfixes](../../includes/storsimple-install-regular-hotfixes.md)]

[!INCLUDE [storsimple-install-maintenance-mode-hotfixes](../../includes/storsimple-install-maintenance-mode-hotfixes.md)]

## <a name="what-happens-to-updates-if-you-perform-a-factory-reset-of-the-device"></a>Cihazın Fabrika sıfırlaması yapmasını güncelleştirmeleri ne olur?
Bir cihazı fabrika ayarlarına sıfırlarsanız tüm güncelleştirmeleri kaybolur. Fabrika sıfırlaması cihaz kaydedildikten ve yapılandırıldıktan sonra el ile StorSimple için Klasik Azure portalı ve/veya Windows PowerShell aracılığıyla güncelleştirmeleri yüklemeniz gerekir. Fabrika sıfırlaması hakkında daha fazla bilgi için bkz: [cihazı fabrika varsayılan ayarlarına geri döndürmeyi](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [StorSimple Cihazınızı yönetmek için StorSimple için Windows PowerShell kullanarak](storsimple-windows-powershell-administration.md).
* Daha fazla bilgi edinin [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanarak](storsimple-manager-service-administration.md).

