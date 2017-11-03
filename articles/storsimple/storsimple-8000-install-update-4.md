---
title: "Güncelleştirme 4 StorSimple 8000 serisi cihaza yüklemek | Microsoft Docs"
description: "StorSimple 8000 serisi güncelleştirme 4 StorSimple 8000 serisi cihazınıza yüklediğiniz açıklanmaktadır."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/02/2017
ms.author: alkohli
ms.openlocfilehash: 57d6d63c55f8ad4da5d1905a1e209da454b0491c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="install-update-4-on-your-storsimple-device"></a>StorSimple Cihazınızda güncelleştirme 4'ü yükleyin

## <a name="overview"></a>Genel Bakış

Bu öğretici, Azure portalı üzerinden önceki yazılım sürümleri çalıştıran ve düzeltme yöntemini kullanarak bir StorSimple cihazında güncelleştirme 4'ü yükleyin açıklanmaktadır. Düzeltme yöntemi, bir ağ arabiriminde StorSimple cihaz veri 0 dışında bir ağ geçidi yapılandırıldığında ve bir güncelleştirme öncesi 1 yazılımı sürümü güncelleştirmeye çalıştığınız kullanılır.

Güncelleştirme 4 aygıt yazılımı, USM üretici yazılımı, LSI sürücü ve bellenim, Storport ve Spaceport, işletim sistemi güvenlik güncelleştirmelerini ve diğer işletim sistemi güncelleştirmelerini ana bilgisayarını içerir.  Aygıt yazılımı, USM üretici yazılımı, Spaceport, Storport ve diğer işletim sistemi güncelleştirmelerini benzer güncelleştirmelerdir. Benzer olmayan veya normal güncelleştirme düzeltme yöntemle veya Azure Portalı aracılığıyla uygulanabilir. Disk Bellenim güncelleştirmeleri kesintiye uğratan güncelleştirmelerin ve cihazın Windows PowerShell arabirimini kullanarak düzeltme yöntemiyle yalnızca uygulanabilir.

> [!IMPORTANT]
> * Elle ve otomatik ön denetimleri kümesini donanım durumu ve ağ bağlantısı bakımından cihaz durumunu belirlemek için yükleme öncesinde gerçekleştirilir. Azure portalından güncelleştirmeleri uygularsanız, bu ön denetimleri gerçekleştirilir.
> * Yazılım ve diğer düzenli güncelleştirmeler Azure Portalı aracılığıyla yüklemenizi öneririz. Portalda güncelleştirme öncesi ağ geçidi denetimi başarısız olursa (güncelleştirmeleri yüklemek için) cihazın Windows PowerShell arabirimine yalnızca gitmeniz gerekir. Gelen güncelleştirdiğiniz sürümüne bağlı olarak, güncelleştirmeleri 4 saat sürebilir (veya daha büyük) yüklemek için. Bakım modu güncelleştirmeleri ayrıca cihazın Windows PowerShell arabirimi yüklenmesi gerekir. Bakım modu kesintiye uğratan güncelleştirmelerdir gibi bunlar, cihazınız için aşağı zaman neden olur.
> * İsteğe bağlı StorSimple Snapshot Manager çalışıyorsa, anlık görüntü Yöneticisi'ni sürümünüzü güncelleştirme 4'e aygıtı güncelleştirme önce yükselttikten emin olun.


[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-4-via-the-azure-portal"></a>Azure portalı üzerinden güncelleştirme 4'ü yükleyin
İçin Cihazınızı güncelleştirmek için aşağıdaki adımları gerçekleştirin [güncelleştirme 4](storsimple-update4-release-notes.md).

> [!NOTE]
> Microsoft aygıttan ek tanılama bilgilerinin çeker. Sorunlarınız aygıtları operations ekibimiz belirlediğinde, sonuç olarak, daha iyi aygıttan bilgi toplamak ve sorunları tanılamak donanımlı duyuyoruz. 

[!INCLUDE [storsimple-8000-install-update4-via-portal](../../includes/storsimple-8000-install-update4-via-portal.md)]

Cihazınızı çalıştığından emin olun **StorSimple 8000 serisi güncelleştirme 4 (6.3.9600.17820)**. **Son tarih güncelleştirme** değiştirilmeleri.

* Şimdi Bakım modu güncelleştirmelerinin kullanılabilir olduğunu göreceksiniz (Bu ileti 24 güncelleştirmeleri yükledikten sonra saate kadar gösterilmeye devam). Bakım modu aygıt kapalı kalma sürelerine neden ve Cihazınızı Windows PowerShell arabirimi yalnızca uygulanabilir kesintiye uğratan güncelleştirmelerdir.

* Bakım modu listelenen adımları kullanarak güncelleştirmeler [düzeltmeleri indirmek için](#to-download-hotfixes) aramak ve KB4011837, (diğer güncelleştirmeleri zaten yüklenmesi gerekir artık) hangi yükler disk Bellenim güncelleştirmeleri karşıdan yüklemek için. Listelenen adımları izleyin [yükleme ve Bakım modu düzeltmeleri doğrulama](#to-install-and-verify-maintenance-mode-hotfixes) Bakım modu yüklemek için güncelleştirir.

## <a name="install-update-4-as-a-hotfix"></a>Bir düzeltme olarak güncelleştirme 4'ü yükleyin
Azure portalı üzerinden güncelleştirme 4 yüklemek için önerilen yöntemdir.

Ağ geçidi onay Azure Portalı aracılığıyla güncelleştirmeleri yüklenmeye çalışırken başarısız olursa bu yordamı kullanın. Bir veri dışı 0 ağ arabirimine atanmış bir ağ geçidine sahip ve Cihazınızı yazılım sürümü güncelleştirme 1 önce çalışan denetimi başarısız olur.

Düzeltme yöntemini kullanarak yükseltilebilir yazılım sürümleri şunlardır:

* Güncelleştirme 0.1, 0.2, 0.3
* 1, 1.1 ve 1.2 güncelleştirmesi
* Güncelleştirme 2, 2.1, 2.2
* Güncelleştirme 3, 3.1


Düzeltme yöntemi aşağıdaki üç adımdan oluşur:

1. Düzeltmeleri Microsoft Update Kataloğu'ndan indirin.
2. Yükleyin ve normal modu düzeltmeleri doğrulayın.
3. Yükleme ve Bakım modu düzeltme doğrulayın.

#### <a name="download-updates-for-your-device"></a>Cihazınız için güncelleştirmeleri karşıdan yükle

Karşıdan yükle ve önceden belirlenen sırasını ve önerilen klasörlerinde aşağıdaki düzeltmeleri yüklemeniz gerekir:

| Sırası | KB | Açıklama | Güncelleştirme türü | Yükleme saati |Klasöre yükleyin|
| --- | --- | --- | --- | --- | --- |
| 1. |KB4011839 |Yazılım güncelleştirmesi |Normal <br></br>Olmayan kesintiye uğratan |~ 25 dakika |FirstOrderUpdate|
| 2A. |KB4011841 <br> KB4011842 |LSI sürücü ve bellenim güncelleştirmeleri <br> USM bellenim güncelleştirme (sürüm 3,38) |Normal <br></br>Olmayan kesintiye uğratan |~ 3 saat <br> (2A içerir. + 2B. + 2 C.)|SecondOrderUpdate|
| 2B. |KB3139398, KB3108381 <br> KB3205400, KB3142030 <br> KB3197873, KB3197873 <br> KB3192392, KB3153704 <br> KB3174644, KB3139914  |İşletim sistemi güvenlik güncelleştirmeleri paketi <br> Windows Server 2012 R2 karşıdan yükle |Normal <br></br>Olmayan kesintiye uğratan |- |SecondOrderUpdate|
| 2 C |KB3210083, KB3103616 <br> KB3146621, KB3121261 <br> KB3123538 |İşletim sistemi güncelleştirmeleri paketi <br> Windows Server 2012 R2 karşıdan yükle |Normal <br></br>Olmayan kesintiye uğratan |- |SecondOrderUpdate|

Önceki tabloda gösterilen tüm güncelleştirmeleri üstünde disk Bellenim güncelleştirmeleri yüklemeniz gerekebilir. Disk Bellenim güncelleştirmeleri çalıştırarak gerekmediğini doğrulayabilirsiniz `Get-HcsFirmwareVersion` cmdlet'i. Bu bellenim sürümleri çalıştırıyorsanız: `XMGJ`, `XGEG`, `KZ50`, `F6C2`, `VR08`, `N002`, `0106`, sonra da bu güncelleştirmeleri yüklemek gerekmez.

| Sırası | KB | Açıklama | Güncelleştirme türü | Yükleme saati | Klasöre yükleyin|
| --- | --- | --- | --- | --- | --- |
| 3. |KB3121899 |Disk bellenim |Bakım <br></br>Kesintiye uğratan |~ 30 dakika | ThirdOrderUpdate |

<br></br>

> [!IMPORTANT]
> * Bu yordamı güncelleştirme 4 uygulamak için yalnızca bir kez gerçekleştirilmesi gerekir. Sonraki güncelleştirmeleri uygulamak için Azure portalını kullanabilirsiniz.
> * Toplam yükleme saatini güncelleştirme 3'ü veya 3.1 güncelleştirmek, yakın 4 saattir.
> * Güncelleştirmeyi uygulamak için bu yordamı kullanmadan önce hem aygıt denetleyicileri çevrimiçi olduğundan ve tüm donanım bileşenleri sağlıklı olduğundan emin olun.

Karşıdan yüklemek ve düzeltmeleri yüklemek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-install-update4-hotfix](../../includes/storsimple-install-update4-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [güncelleştirme 4 yayın](storsimple-update4-release-notes.md).

