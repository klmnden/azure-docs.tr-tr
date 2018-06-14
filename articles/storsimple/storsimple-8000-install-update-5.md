---
title: Güncelleştirme 5 StorSimple 8000 serisi cihaza yüklemek | Microsoft Docs
description: StorSimple 8000 serisi güncelleştirme 5 StorSimple 8000 serisi cihazınıza yüklediğiniz açıklanmaktadır.
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
ms.openlocfilehash: d6e17c7609fd41b8f4457edda373f6882a1a9d2b
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2018
ms.locfileid: "28108729"
---
# <a name="install-update-5-on-your-storsimple-device"></a>StorSimple Cihazınızda güncelleştirme 5 yükleyin

## <a name="overview"></a>Genel Bakış

Bu öğretici, Azure portalı üzerinden önceki yazılım sürümleri çalıştıran ve düzeltme yöntemini kullanarak bir StorSimple cihazında güncelleştirme 5'i yüklemeniz açıklanmaktadır. Güncelleştirme öncesi 3 sürümlerini çalıştıran bir cihazda güncelleştirme 5 yüklenmeye çalışırken düzeltme yöntemi kullanılır. Düzeltme yöntemi, bir ağ arabiriminde StorSimple cihaz veri 0 dışında bir ağ geçidi yapılandırıldığında ve bir güncelleştirme öncesi 1 yazılımı sürümü güncelleştirmeye çalıştığınız de kullanılır.

Güncelleştirme 5 içerir aygıt yazılımı, Storport ve Spaceport, işletim sistemi güvenlik güncelleştirmelerini ve işletim sistemi güncelleştirmelerini ve disk Bellenim güncelleştirmeleri.  Aygıt yazılımı, Spaceport, Storport, güvenlik ve diğer işletim sistemi güncelleştirmelerini benzer güncelleştirmelerdir. Benzer olmayan veya normal güncelleştirme düzeltme yöntemle veya Azure Portalı aracılığıyla uygulanabilir. Disk Bellenim güncelleştirmeleri kesintiye uğratan güncelleştirmelerin ve cihaz cihazın Windows PowerShell arabirimini kullanarak düzeltme yöntemle bakım modunda olduğunda uygulanır.

> [!IMPORTANT]
> * Güncelleştirme 5 zorunlu bir güncelleştirme ve hemen yüklenmesi gerekir. Daha fazla bilgi için bkz: [güncelleştirme 5 sürüm notları](storsimple-update5-release-notes.md).
> * Elle ve otomatik ön denetimleri kümesini donanım durumu ve ağ bağlantısı bakımından cihaz durumunu belirlemek için yükleme öncesinde gerçekleştirilir. Azure portalından güncelleştirmeleri uygularsanız, bu ön denetimleri gerçekleştirilir.
> * Güncelleştirme 3'den önceki sürümleri çalıştıran bir cihazda güncelleştirirken düzeltme yöntemi kullanarak güncelleştirmeleri yüklemeniz önerilir. Herhangi bir sorunla karşılaşırsanız [bir destek bileti oturum](storsimple-8000-contact-microsoft-support.md).
> * Yazılım ve diğer düzenli güncelleştirmeler Azure Portalı aracılığıyla yüklemenizi öneririz. Portalda güncelleştirme öncesi ağ geçidi denetimi başarısız olursa (güncelleştirmeleri yüklemek için) cihazın Windows PowerShell arabirimine yalnızca gitmeniz gerekir. Gelen güncelleştirdiğiniz sürümüne bağlı olarak, güncelleştirmeleri 4 saat sürebilir (veya daha büyük) yüklemek için. Bakım modu güncelleştirmeleri aygıtı Windows PowerShell arabirimi yüklenmesi gerekir. Bakım modu kesintiye uğratan güncelleştirmelerdir gibi bu cihazınız için aşağı zaman sonuçlanır.
> * İsteğe bağlı StorSimple Snapshot Manager çalışıyorsa, anlık görüntü Yöneticisi'ni sürümünüzü güncelleştirme 5 aygıtı güncelleştirme önce yükselttikten emin olun.


[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-5-via-the-azure-portal"></a>Güncelleştirme 5 Azure portalı üzerinden yükleyin
İçin Cihazınızı güncelleştirmek için aşağıdaki adımları gerçekleştirin [güncelleştirme 5](storsimple-update5-release-notes.md).

> [!NOTE]
> Microsoft aygıttan ek tanılama bilgilerinin çeker. Sorunlarınız aygıtları operations ekibimiz belirlediğinde, sonuç olarak, daha iyi aygıttan bilgi toplamak ve sorunları tanılamak donanımlı duyuyoruz.

[!INCLUDE [storsimple-8000-install-update4-via-portal](../../includes/storsimple-8000-install-update5-via-portal.md)]

Cihazınızı çalıştığından emin olun **StorSimple 8000 serisi güncelleştirme 5 (6.3.9600.17845)**. **Son tarih güncelleştirme** değiştirilmemelidir.

Şimdi Bakım modu güncelleştirmelerinin kullanılabilir olduğunu göreceksiniz (Bu ileti 24 güncelleştirmeleri yükledikten sonra saate kadar gösterilmeye devam). Bakım modu güncelleştirmeyi yüklemek için adımlar sonraki bölümde ayrıntılı olarak açıklanmaktadır.

[!INCLUDE [storsimple-8000-install-maintenance-mode-updates](../../includes/storsimple-8000-install-maintenance-mode-updates.md)]

## <a name="install-update-5-as-a-hotfix"></a>Güncelleştirme 5 bir düzeltmenin yüklenmesi

Düzeltme yöntemini kullanarak yükseltilebilir yazılım sürümleri şunlardır:

* Güncelleştirme 0.1, 0.2, 0.3
* 1, 1.1 ve 1.2 güncelleştirmesi
* Güncelleştirme 2, 2.1, 2.2
* Güncelleştirme 3, 3.1
* Güncelleştirme 4

> [!NOTE] 
> Güncelleştirme 5 yüklemek için önerilen Azure portalı üzerinden güncelleştirme 3 ve sonraki sürüm güncelleştirmeye çalıştığınızda yöntemdir. Güncelleştirme 3'den önceki sürümleri çalıştıran bir cihazda güncelleştirirken, bu yordamı kullanın. Ağ geçidi onay Azure Portalı aracılığıyla güncelleştirmeleri yüklenmeye çalışırken başarısız olursa, bu yordamı kullanabilirsiniz. Bir veri dışı 0 ağ arabirimine atanmış bir ağ geçidi varsa ve Cihazınızı güncelleştirme 1'den önceki bir yazılım sürümleri çalıştıran denetimi başarısız olur.

Düzeltme yöntemi aşağıdaki üç adımdan oluşur:

1. Düzeltmeleri Microsoft Update Kataloğu'ndan indirin.
2. Yükleyin ve normal modu düzeltmeleri doğrulayın.
3. Yükleme ve Bakım modu düzeltme doğrulayın.

#### <a name="download-updates-for-your-device"></a>Cihazınız için güncelleştirmeleri karşıdan yükle

Karşıdan yükle ve önceden belirlenen sırasını ve önerilen klasörlerinde aşağıdaki düzeltmeleri yüklemeniz gerekir:

| Sıra | KB | Açıklama | Güncelleştirme türü | Yükleme saati |Klasöre yükleyin|
| --- | --- | --- | --- | --- | --- |
| 1. |KB4037264 |Yazılım güncelleştirmesi<br> Her ikisi de karşıdan _HcsSoftwareUpdate.exe_ ve _CisMSDAgent.exe_ |Normal <br></br>Olmayan kesintiye uğratan |~ 25 dakika |FirstOrderUpdate|

Güncelleştirme 4 çalıştıran bir CİHAZDAN güncelleştirme, yalnızca işletim sistemi toplu güncelleştirmeler ikinci sipariş güncelleştirmeleri yüklemeniz gerekir.

| Sıra | KB | Açıklama | Güncelleştirme türü | Yükleme saati |Klasöre yükleyin|
| --- | --- | --- | --- | --- | --- |
| 2A. |KB4025336 |İşletim sistemi toplu güncelleştirmeler paketi <br> Windows Server 2012 R2 sürümünü karşıdan yükleyin |Normal <br></br>Olmayan kesintiye uğratan |- |SecondOrderUpdate|

Güncelleştirme 3'ü çalıştıran bir aygıttan yükleme veya önceki sürümleri, aşağıdaki yanı sıra toplu güncelleştirmeleri yükleyin.

| Sıra | KB | Açıklama | Güncelleştirme türü | Yükleme saati |Klasöre yükleyin|
| --- | --- | --- | --- | --- | --- |
| 2B. |KB4011841 <br> KB4011842 |LSI sürücü ve bellenim güncelleştirmeleri <br> USM bellenim güncelleştirme (sürüm 3,38) |Normal <br></br>Olmayan kesintiye uğratan |~ 3 saat <br> (2A içerir. + 2B. + 2C.)|SecondOrderUpdate|
| 2C. |KB3139398 <br> KB3142030 <br> KB3108381 <br> KB3153704 <br> KB3174644 <br> KB3139914   |İşletim sistemi güvenlik güncelleştirmeleri paketi <br> Windows Server 2012 R2 sürümünü karşıdan yükleyin |Normal <br></br>Olmayan kesintiye uğratan |- |SecondOrderUpdate|
| 2D. |KB3146621 <br> KB3103616 <br> KB3121261 <br> KB3123538 |İşletim sistemi güncelleştirmeleri paketi <br> Windows Server 2012 R2 sürümünü karşıdan yükleyin |Normal <br></br>Olmayan kesintiye uğratan |- |SecondOrderUpdate|


Önceki tabloda gösterilen tüm güncelleştirmeleri üstünde disk Bellenim güncelleştirmeleri yüklemeniz gerekebilir. Disk Bellenim güncelleştirmeleri çalıştırarak gerekmediğini doğrulayabilirsiniz `Get-HcsFirmwareVersion` cmdlet'i. Bu bellenim sürümleri çalıştırıyorsanız: `XMGJ`, `XGEG`, `KZ50`, `F6C2`, `VR08`, `N003`, `0107`, sonra da bu güncelleştirmeleri yüklemek gerekmez.

| Sıra | KB | Açıklama | Güncelleştirme türü | Yükleme saati | Klasöre yükleyin|
| --- | --- | --- | --- | --- | --- |
| 3. |KB4037263 |Disk bellenim |Bakım <br></br>Kesintiye uğratan |~ 30 dakika | ThirdOrderUpdate |

<br></br>

> [!IMPORTANT]
> * Toplam yükleme saatini güncelleştirme 4'ten güncelleştirme, yakın 4 saattir.
> * Güncelleştirmeyi uygulamak için bu yordamı kullanmadan önce hem aygıt denetleyicileri çevrimiçi olduğundan ve tüm donanım bileşenleri sağlıklı olduğundan emin olun.

Karşıdan yüklemek ve düzeltmeleri yüklemek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-install-update5-hotfix](../../includes/storsimple-install-update5-hotfix.md)]

[!INCLUDE [storsimple-8000-install-troubleshooting](../../includes/storsimple-8000-install-troubleshooting.md)]

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [güncelleştirme 5 yayın](storsimple-update5-release-notes.md).

