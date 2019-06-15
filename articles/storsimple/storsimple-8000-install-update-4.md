---
title: StorSimple 8000 serisi Cihazınızı güncelleştirme 4'ü yükleyin | Microsoft Docs
description: StorSimple 8000 serisi Cihazınızı StorSimple 8000 serisi güncelleştirme 4 yükleneceği açıklanmaktadır.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/02/2017
ms.author: alkohli
ms.openlocfilehash: 5b48cbd1020cfd51fe989a9be33197f2735f21f4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60860527"
---
# <a name="install-update-4-on-your-storsimple-device"></a>StorSimple Cihazınızı güncelleştirme 4'ü yükleyin

## <a name="overview"></a>Genel Bakış

Bu öğreticide, Azure portalı üzerinden önceki yazılım sürümleri çalıştıran ve düzeltme yöntemi kullanarak bir StorSimple cihazında güncelleştirme 4'ü yükleyin açıklanmaktadır. StorSimple cihaz DATA 0 dışındaki bir ağ arabiriminde bir ağ geçidi yapılandırılmış ve bir güncelleştirme öncesi 1 yazılım sürümü güncelleştirme çalıştığınız düzeltme yöntemi kullanılır.

Cihaz yazılımı, USM üretici yazılımı, LSI sürücü ve bellenim, Storport ve Spaceport, işletim sistemi güvenlik güncelleştirmelerini ve diğer işletim sistemi güncelleştirmelerini çok sayıda güncelleştirme 4 içerir.  Cihaz yazılımı, USM üretici yazılımı, Spaceport, Storport ve diğer işletim sistemi güncelleştirmelerini kesintiye uğratmayan güncelleştirmelerin. Kesintiye neden olmayan ya da düzenli güncelleştirmeler, düzeltme yöntemi veya Azure Portalı aracılığıyla uygulanabilir. Disk üretici yazılımı güncelleştirmelerini kesintiye uğratan güncelleştirmelerdir ve yalnızca cihazın Windows PowerShell arabirimini kullanarak düzeltme yöntemi uygulanabilir.

> [!IMPORTANT]
> * Otomatik ve el ile ön denetimleri kümesini donanım durumu ve ağ bağlantısı açısından cihaz durumu belirlemek için yükleme öncesinde gerçekleştirilir. Azure portalından güncelleştirmeleri uygularsanız, bu ön denetimler gerçekleştirilir.
> * Yazılım ve diğer düzenli güncelleştirmeler Azure portalı üzerinden yüklemeniz önerilir. Portalda güncelleştirme öncesi ağ geçidi denetimi başarısız olursa (güncelleştirmeleri yüklemek için) cihazın Windows PowerShell arabirimine yalnızca gitmeniz gerekir. Gelen güncelleştirdiğiniz sürüm bağlı olarak, güncelleştirmeleri 4 saat sürebilir (veya üstü) yüklemek için. Bakım modu güncelleştirmeleri ayrıca cihazın Windows PowerShell arabirimi yüklenmesi gerekir. Bakım modu güncelleştirmeleri kesintiye uğratan güncelleştirmelerdir gibi bunlar aşağı birer cihazınız için neden olur.
> * İsteğe bağlı bir StorSimple Snapshot Manager çalıştıran Snapshot Manager sürümünüzü güncelleştirme 4'e cihaz güncelleştirilmeden önce yükseltmişseniz emin olun.


[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-4-via-the-azure-portal"></a>Azure portalı üzerinden güncelleştirme 4'ü yükleyin
Cihazınızı güncelleştirmek için aşağıdaki adımları gerçekleştirin [güncelleştirme 4](storsimple-update4-release-notes.md).

> [!NOTE]
> Microsoft, ek tanılama bilgilerini CİHAZDAN çeker. Operasyon ekibimiz sorunlarınız cihazları belirlediğinde, sonuç olarak, daha iyi CİHAZDAN bilgi toplamak ve sorunları tanılamak donanımlı duyuyoruz. 

[!INCLUDE [storsimple-8000-install-update4-via-portal](../../includes/storsimple-8000-install-update4-via-portal.md)]

Cihazınızı çalıştığını doğrulayın **StorSimple 8000 serisi güncelleştirme 4'e (6.3.9600.17820)** . **Son güncelleştirme tarihi** değiştirilmeleri.

* Şimdi Bakım modu güncelleştirmeleri kullanılabilir olduğunu göreceksiniz (Bu ileti güncelleştirmeleri yükledikten sonra en fazla 24 saat için görüntülenecek devam edebilir). Bakım modu güncelleştirmeleri, cihaz kapalı kalma süresi neden ve yalnızca cihazınızın Windows PowerShell arabirimi üzerinden uygulanabilir, kesintiye uğratan güncelleştirmelerdir.

* Bakım modu güncelleştirmeleri, listelenen adımları kullanarak indirme [düzeltmeleri indirmek için](#to-download-hotfixes) arayın ve KB4011837, (diğer güncelleştirmeler zaten yüklenmesi artık) hangi yükler disk üretici yazılımı güncelleştirmelerini indirmek için. İçinde listelenen adımları takip [Bakım modu düzeltmelerini yüklemek ve doğrulamak](#to-install-and-verify-maintenance-mode-hotfixes) Bakım modu güncelleştirmesi.

## <a name="install-update-4-as-a-hotfix"></a>Bir düzeltme olarak güncelleştirme 4'ü yükleyin
Azure portalı üzerinden güncelleştirme 4 yüklemek için önerilen yöntemdir.

Azure Portalı aracılığıyla güncelleştirmeleri yüklenmeye çalışılırken bir ağ geçidi denetimi başarısız olursa, bu yordamı kullanın. Denetim bir veri 0 ağ arabirimine atanmış bir ağ geçidi olması ve Cihazınızı güncelleştirme 1'den önceki bir yazılım sürümü çalıştıran başarısız olur.

Düzeltme yöntemi kullanılarak yükseltilebilir yazılım sürümleri şunlardır:

* Güncelleştirme 0.1, 0.2 0.3
* Güncelleştirme 1, 1.1, 1.2
* Güncelleştirme 2, 2.1, 2.2
* Güncelleştirme 3, 3.1


Düzeltme yöntemi, aşağıdaki üç adımdan oluşur:

1. Düzeltmeleri, Microsoft Update Kataloğu'ndan indirin.
2. Yükleme ve normal mod düzeltmelerini doğrulayın.
3. Yükleme ve Bakım modu düzeltmesinin doğrulayın.

#### <a name="download-updates-for-your-device"></a>Cihazınız için güncelleştirmeler

İndirin ve önceden belirlenmiş bir sırada ve önerilen klasörlerinde aşağıdaki düzeltmeleri yüklemeniz gerekir:

| Sipariş verme | KB | Açıklama | Güncelleştirme türü | Yükleme saati |Klasöre yükle|
| --- | --- | --- | --- | --- | --- |
| 1. |KB4011839 |Yazılım güncelleştirmesi |Normal <br></br>Kesintiye neden olmayan |~ 25 dakika |FirstOrderUpdate|
| 2A. |KB4011841 <br> KB4011842 |LSI sürücü ve bellenim güncelleştirmeleri <br> USM üretici yazılımı güncelleştirmesi (sürüm 3,38) |Normal <br></br>Kesintiye neden olmayan |~ 3 saat <br> (2A içerir. + 2B. + 2C.)|SecondOrderUpdate|
| 2B. |KB3139398, KB3108381 <br> KB3205400, KB3142030 <br> KB3197873, KB3197873 <br> KB3192392, KB3153704 <br> KB3174644, KB3139914  |İşletim sistemi güvenlik güncelleştirmeleri paketi <br> Windows Server 2012 R2 indirin |Normal <br></br>Kesintiye neden olmayan |- |SecondOrderUpdate|
| 2 C |KB3210083, KB3103616 <br> KB3146621, KB3121261 <br> KB3123538 |İşletim sistemi güncelleştirmeleri paketi <br> Windows Server 2012 R2 indirin |Normal <br></br>Kesintiye neden olmayan |- |SecondOrderUpdate|

Yukarıdaki tabloda gösterilen tüm güncelleştirmeleri üzerinde disk üretici yazılımı güncelleştirmelerini yüklemek gerekebilir. Disk üretici yazılımı güncelleştirmelerini çalıştırarak gerekmediğini doğrulayın `Get-HcsFirmwareVersion` cmdlet'i. Bu bellenim sürümleri çalıştırıyorsanız: `XMGJ`, `XGEG`, `KZ50`, `F6C2`, `VR08`, `N002`, `0106`, sonra da bu güncelleştirmeleri yüklemeniz gerekmez.

| Sipariş verme | KB | Açıklama | Güncelleştirme türü | Yükleme saati | Klasöre yükle|
| --- | --- | --- | --- | --- | --- |
| 3. |KB3121899 |Disk üretici yazılımı |Bakım <br></br>Kesintiye uğratan bir durumdur |yaklaşık 30 dakika | ThirdOrderUpdate |

<br></br>

> [!IMPORTANT]
> * Bu yordamı uygulamak Update 4 için yalnızca bir kez gerçekleştirilmesi gerekir. Sonraki güncelleştirmeleri uygulamak için Azure portalını kullanabilirsiniz.
> * Güncelleştirme 3 veya 3.1 güncelleştiriliyor, toplam yükleme süresi 4 saat yakın olur.
> * Güncelleştirmeyi uygulamak için bu yordamı kullanmadan önce hem cihaz denetleyicinin de çevrimiçi olduğundan ve tüm donanım bileşenlerinin sağlıklı olduğundan emin olun.

Düzeltmeleri yüklemek ve indirmek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-install-update4-hotfix](../../includes/storsimple-install-update4-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [güncelleştirme 4 sürüm](storsimple-update4-release-notes.md).

