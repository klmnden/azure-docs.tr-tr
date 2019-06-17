---
title: StorSimple 8000 serisi Cihazınızı güncelleştirme 5'i yükleyin | Microsoft Docs
description: StorSimple 8000 serisi Cihazınızı StorSimple 8000 serisi güncelleştirme 5 yükleneceği açıklanmaktadır.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/13/2017
ms.author: alkohli
ms.openlocfilehash: d86e77ef0148c0fac3dfa31153364de153b094ef
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62126758"
---
# <a name="install-update-5-on-your-storsimple-device"></a>StorSimple Cihazınızda güncelleştirme 5'i yükleme

## <a name="overview"></a>Genel Bakış

Bu öğreticide, Azure portalı üzerinden önceki yazılım sürümleri çalıştıran ve düzeltme yöntemi kullanarak bir StorSimple cihazında güncelleştirme 5'i yüklemek açıklanmaktadır. Güncelleştirme 5 3 güncelleştirme öncesi sürümleri çalıştıran bir cihaza yüklemeye çalışırken düzeltme yöntemi kullanılır. Düzeltme yöntemi, bir ağ arabiriminde StorSimple cihazının DATA 0 dışındaki bir ağ geçidi yapılandırılmış ve bir güncelleştirme öncesi 1 yazılım sürümü güncelleştirme çalıştığınız de kullanılır.

Güncelleştirme 5 içeren cihaz yazılımı, Storport ve Spaceport, işletim sistemi güvenlik güncelleştirmelerini ve işletim sistemi güncelleştirmelerini ve disk üretici yazılımı güncelleştirmelerini.  Cihaz yazılımı, Spaceport, Storport, güvenlik ve diğer işletim sistemi güncelleştirmelerini kesintiye uğratmayan güncelleştirmelerin. Kesintiye neden olmayan ya da düzenli güncelleştirmeler, düzeltme yöntemi veya Azure Portalı aracılığıyla uygulanabilir. Disk üretici yazılımı güncelleştirmelerini kesintiye uğratan güncelleştirmelerdir ve cihaz, cihazın Windows PowerShell arabirimini kullanarak düzeltme yöntemi aracılığıyla bakım modunda olduğunda uygulanır.

> [!IMPORTANT]
> * Güncelleştirme 5 zorunlu bir güncelleştirme ve hemen yüklü olması gerekir. Daha fazla bilgi için [güncelleştirme 5 sürüm notları](storsimple-update5-release-notes.md).
> * Otomatik ve el ile ön denetimleri kümesini donanım durumu ve ağ bağlantısı açısından cihaz durumu belirlemek için yükleme öncesinde gerçekleştirilir. Azure portalından güncelleştirmeleri uygularsanız, bu ön denetimler gerçekleştirilir.
> * Güncelleştirme 3'ü önceki sürümlerini çalıştıran bir cihaz güncelleştirirken düzeltme yöntemi kullanarak güncelleştirmeleri yüklemeniz önerilir. Herhangi bir sorunla karşılaşırsanız [bir destek biletini günlüğe kaydetme](storsimple-8000-contact-microsoft-support.md).
> * Yazılım ve diğer düzenli güncelleştirmeler Azure portalı üzerinden yüklemeniz önerilir. Portalda güncelleştirme öncesi ağ geçidi denetimi başarısız olursa (güncelleştirmeleri yüklemek için) cihazın Windows PowerShell arabirimine yalnızca gitmeniz gerekir. Gelen güncelleştirdiğiniz sürüm bağlı olarak, güncelleştirmeleri 4 saat sürebilir (veya üstü) yüklemek için. Bakım modu güncelleştirmeleri cihazın Windows PowerShell arabirimi yüklenmesi gerekir. Bakım modu güncelleştirmeleri kesintiye uğratan güncelleştirmelerdir gibi bunlar aşağı birer cihazınız için sonuçlanır.
> * İsteğe bağlı bir StorSimple Snapshot Manager çalıştıran Snapshot Manager sürümünüz için güncelleştirme 5 cihaz güncelleştirilmeden önce yükseltmişseniz emin olun.


[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-5-via-the-azure-portal"></a>Güncelleştirme 5 Azure portalı üzerinden yükleyin
Cihazınızı güncelleştirmek için aşağıdaki adımları gerçekleştirin [güncelleştirme 5'i](storsimple-update5-release-notes.md).

> [!NOTE]
> Microsoft, ek tanılama bilgilerini CİHAZDAN çeker. Operasyon ekibimiz sorunlarınız cihazları belirlediğinde, sonuç olarak, daha iyi CİHAZDAN bilgi toplamak ve sorunları tanılamak donanımlı duyuyoruz.

[!INCLUDE [storsimple-8000-install-update4-via-portal](../../includes/storsimple-8000-install-update5-via-portal.md)]

Cihazınızı çalıştığını doğrulayın **StorSimple 8000 serisi güncelleştirme 5 (6.3.9600.17845)** . **Son güncelleştirme tarihi** değiştirilmelidir.

Şimdi Bakım modu güncelleştirmeleri kullanılabilir olduğunu göreceksiniz (Bu ileti güncelleştirmeleri yükledikten sonra en fazla 24 saat için görüntülenecek devam edebilir). Bakım modu güncelleştirmesi yüklemek için adımlar sonraki bölümde ayrıntılı olarak açıklanmaktadır.

[!INCLUDE [storsimple-8000-install-maintenance-mode-updates](../../includes/storsimple-8000-install-maintenance-mode-updates.md)]

## <a name="install-update-5-as-a-hotfix"></a>Güncelleştirme 5 kurma

Düzeltme yöntemi kullanılarak yükseltilebilir yazılım sürümleri şunlardır:

* Güncelleştirme 0.1, 0.2 0.3
* Güncelleştirme 1, 1.1, 1.2
* Güncelleştirme 2, 2.1, 2.2
* Güncelleştirme 3, 3.1
* Güncelleştirme 4

> [!NOTE] 
> Güncelleştirme 5'i yüklemek için önerilen yöntem, güncelleştirme 3 ve sonraki bir sürümünü güncelleştirmek çalışırken Azure portalı üzerinden olur. Güncelleştirme 3'ü önceki sürümlerini çalıştıran bir cihaz güncelleştirirken, bu yordamı kullanın. Azure Portalı aracılığıyla güncelleştirmeleri yüklenmeye çalışılırken bir ağ geçidi denetimi başarısız olursa, bu yordamı kullanabilirsiniz. Denetim bir veri 0 ağ arabirimine atanmış bir ağ geçidi varsa ve Cihazınızı güncelleştirme 1'den önceki bir yazılım sürümleri çalıştıran başarısız olur.

Düzeltme yöntemi, aşağıdaki üç adımdan oluşur:

1. Düzeltmeleri, Microsoft Update Kataloğu'ndan indirin.
2. Yükleme ve normal mod düzeltmelerini doğrulayın.
3. Yükleme ve Bakım modu düzeltmesinin doğrulayın.

#### <a name="download-updates-for-your-device"></a>Cihazınız için güncelleştirmeler

İndirin ve önceden belirlenmiş bir sırada ve önerilen klasörlerinde aşağıdaki düzeltmeleri yüklemeniz gerekir:

| Sipariş verme | KB | Açıklama | Güncelleştirme türü | Yükleme saati |Klasöre yükle|
| --- | --- | --- | --- | --- | --- |
| 1. |KB4037264 |Yazılım güncelleştirmesi<br> Her ikisi de indirme _HcsSoftwareUpdate.exe_ ve _CisMSDAgent.exe_ |Normal <br></br>Kesintiye neden olmayan |~ 25 dakika |FirstOrderUpdate|

Güncelleştirme 4 çalıştıran bir CİHAZDAN güncelleştirme, yalnızca işletim sistemi toplu güncelleştirmeler ikinci sipariş güncelleştirmelerini yüklemek gereklidir.

| Sipariş verme | KB | Açıklama | Güncelleştirme türü | Yükleme saati |Klasöre yükle|
| --- | --- | --- | --- | --- | --- |
| 2A. |KB4025336 |İşletim sistemi toplu güncelleştirmeleri paketi <br> Windows Server 2012 R2 sürümünü indirin |Normal <br></br>Kesintiye neden olmayan |- |SecondOrderUpdate|

Güncelleştirme 3'ü çalıştıran bir CİHAZDAN yükleme ya da önceki toplu güncelleştirmelere ek olarak aşağıdaki yükleyin.

| Sipariş verme | KB | Açıklama | Güncelleştirme türü | Yükleme saati |Klasöre yükle|
| --- | --- | --- | --- | --- | --- |
| 2B. |KB4011841 <br> KB4011842 |LSI sürücü ve bellenim güncelleştirmeleri <br> USM üretici yazılımı güncelleştirmesi (sürüm 3,38) |Normal <br></br>Kesintiye neden olmayan |~ 3 saat <br> (2A içerir. + 2B. + 2C.)|SecondOrderUpdate|
| 2 C |KB3139398 <br> KB3142030 <br> KB3108381 <br> KB3153704 <br> KB3174644 <br> KB3139914   |İşletim sistemi güvenlik güncelleştirmeleri paketi <br> Windows Server 2012 R2 sürümünü indirin |Normal <br></br>Kesintiye neden olmayan |- |SecondOrderUpdate|
| 2B. |KB3146621 <br> KB3103616 <br> KB3121261 <br> KB3123538 |İşletim sistemi güncelleştirmeleri paketi <br> Windows Server 2012 R2 sürümünü indirin |Normal <br></br>Kesintiye neden olmayan |- |SecondOrderUpdate|


Yukarıdaki tabloda gösterilen tüm güncelleştirmeleri üzerinde disk üretici yazılımı güncelleştirmelerini yüklemek gerekebilir. Disk üretici yazılımı güncelleştirmelerini çalıştırarak gerekmediğini doğrulayın `Get-HcsFirmwareVersion` cmdlet'i. Bu bellenim sürümleri çalıştırıyorsanız: `XMGJ`, `XGEG`, `KZ50`, `F6C2`, `VR08`, `N003`, `0107`, sonra da bu güncelleştirmeleri yüklemeniz gerekmez.

| Sipariş verme | KB | Açıklama | Güncelleştirme türü | Yükleme saati | Klasöre yükle|
| --- | --- | --- | --- | --- | --- |
| 3. |KB4037263 |Disk üretici yazılımı |Bakım <br></br>Kesintiye uğratan bir durumdur |yaklaşık 30 dakika | ThirdOrderUpdate |

<br></br>

> [!IMPORTANT]
> * Güncelleştirme 4'ten güncelleştiriliyor, toplam yükleme süresi 4 saat yakın olur.
> * Güncelleştirmeyi uygulamak için bu yordamı kullanmadan önce hem cihaz denetleyicinin de çevrimiçi olduğundan ve tüm donanım bileşenlerinin sağlıklı olduğundan emin olun.

Düzeltmeleri yüklemek ve indirmek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-install-update5-hotfix](../../includes/storsimple-install-update5-hotfix.md)]

[!INCLUDE [storsimple-8000-install-troubleshooting](../../includes/storsimple-8000-install-troubleshooting.md)]

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [güncelleştirme 5 üretici sürümü '](storsimple-update5-release-notes.md).

