---
title: "StorSimple Cihazınızda güncelleştirme 3'ü yükleme | Microsoft Docs"
description: "StorSimple 8000 serisi aygıtınızda StorSimple 8000 serisi güncelleştirme 3'ü yüklemek açıklanmaktadır."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: c6c4634d-4f3a-4bc4-b307-a22bf18664e1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 72b004a6c2604e0fc20b71b4b69217622f8f9ea0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="install-update-3-on-your-storsimple-8000-series-device"></a>StorSimple 8000 serisi aygıtınızda güncelleştirme 3'ü yükleme

## <a name="overview"></a>Genel Bakış

Bu öğretici, Klasik Azure portalı önceki yazılım sürümleri çalıştıran ve düzeltme yöntemini kullanarak bir StorSimple cihazında güncelleştirme 3'ü yüklemek açıklanmaktadır. Düzeltme yöntemi, bir ağ arabiriminde StorSimple cihaz veri 0 dışında bir ağ geçidi yapılandırıldığında ve bir güncelleştirme öncesi 1 yazılımı sürümü güncelleştirmeye çalıştığınız kullanılır.

Güncelleştirme 3 aygıt yazılımı, LSI sürücü ve bellenim, Storport içerir ve Spaceport güncelleştirir. Güncelleştirme 2 veya daha önceki bir sürümünden güncelleştirme, ayrıca iSCSI, WMI, uygulamak ve bellenim güncelleştirmeleri belirli durumlarda, disk için gerekli olacaktır. Aygıt yazılımı, WMI, iSCSI, LSI sürücü, Spaceport ve Storport düzeltmeleri benzer güncelleştirmeleri ve klasik Azure portalı uygulanabilir. Disk Bellenim güncelleştirmeleri kesintiye uğratan güncelleştirmelerin ve yalnızca aygıt Windows PowerShell arabirimi uygulanabilir. 

> [!IMPORTANT]
> * Elle ve otomatik ön denetimleri kümesini donanım durumu ve ağ bağlantısı bakımından cihaz durumunu belirlemek için yükleme öncesinde gerçekleştirilir. Klasik Azure portalından güncelleştirmeleri uygularsanız, bu ön denetimleri gerçekleştirilir.
> * Klasik Azure Portalı aracılığıyla yazılım ve sürücü güncelleştirmelerini yüklemenizi öneririz. Portalda güncelleştirme öncesi ağ geçidi denetimi başarısız olursa (güncelleştirmeleri yüklemek için) cihazın Windows PowerShell arabirimine yalnızca gitmeniz gerekir. Güncelleştirmekte olduğunuz sürümüne bağlı olarak, güncelleştirmeleri yüklemek için 1.5-2.5 saat sürebilir. Bakım modu güncelleştirmeleri aygıtı Windows PowerShell arabirimi yüklenmesi gerekir. Bakım modu kesintiye uğratan güncelleştirmelerdir gibi bunlar, cihazınız için aşağı zaman neden olur.
> * İsteğe bağlı StorSimple Snapshot Manager çalışıyorsa, anlık görüntü Yöneticisi'ni sürümü güncelleştirme 2'ye aygıtı güncelleştirme önce yükselttikten emin olun.
> 
> 

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-3-via-the-azure-classic-portal"></a>Azure Klasik portalı üzerinden güncelleştirme 3'ü yükleme
İçin Cihazınızı güncelleştirmek için aşağıdaki adımları gerçekleştirin [güncelleştirme 3](storsimple-update3-release-notes.md).

> [!NOTE]
> Daha sonra (de dahil olmak üzere güncelleştirme 2.1) veya güncelleştirme 2 uyguluyorsanız Microsoft aygıttan ek tanılama bilgilerini mümkün olacaktır. Sorunlarınız aygıtları operations ekibimiz belirlediğinde, sonuç olarak, daha iyi aygıttan bilgi toplamak ve sorunları tanılamak donanımlı duyuyoruz. Güncelleştirme 2 veya sonrası kabul ederek, bize bu öngörülü desteği sağlamak izin verir.
> 
> 

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

Cihazınızı çalıştığından emin olun **StorSimple 8000 serisi güncelleştirme 3'ü (6.3.9600.17759)**. **Son tarih güncelleştirme** değiştirilmeleri. 
   - Güncelleştirme 2 önce bir sürümünden güncelleştiriyorsanız, ayrıca Bakım modu güncelleştirmelerinin kullanılabilir olduğunu göreceksiniz (Bu ileti 24 güncelleştirmeleri yükledikten sonra saate kadar gösterilmeye devam).
     Bakım modu aygıt kapalı kalma sürelerine neden ve Cihazınızı Windows PowerShell arabirimi yalnızca uygulanabilir kesintiye uğratan güncelleştirmelerdir. Bazı durumlarda Güncelleştirme 1.2 çalıştırırken, disk bellenim zaten güncel herhangi bir Bakım modu güncelleştirme yüklemeniz gerekmez; bu durumda olabilir.
   - Güncelleştirme güncelleştirme 2 veya sonrası, cihazınız artık güncel olması gerekir. Sonraki adıma atlayabilirsiniz.

Bakım modu listelenen adımları kullanarak güncelleştirmeler [düzeltmeleri indirmek için](#to-download-hotfixes) aramak ve KB3121899, (diğer güncelleştirmeleri zaten yüklenmesi gerekir artık) hangi yükler disk Bellenim güncelleştirmeleri karşıdan yüklemek için. Listelenen adımları izleyin [yükleme ve Bakım modu düzeltmeleri doğrulama](#to-install-and-verify-maintenance-mode-hotfixes) Bakım modu yüklemek için güncelleştirir. 

## <a name="install-update-3-as-a-hotfix"></a>Bir düzeltme olarak güncelleştirme 3'ü yükleme
Ağ geçidi onay Klasik Azure Portalı aracılığıyla güncelleştirmeleri yüklenmeye çalışırken başarısız olursa bu yordamı kullanın. Bir veri dışı 0 ağ arabirimine atanmış bir ağ geçidine sahip ve Cihazınızı yazılım sürümü güncelleştirme 1 önce çalışan denetimi başarısız olur.

Düzeltme yöntemini kullanarak yükseltilebilir yazılım sürümleri şunlardır:

* Güncelleştirme 0.1, 0.2, 0.3
* 1, 1.1 ve 1.2 güncelleştirmesi
* Güncelleştirme 2, 2.1, 2.2 

> [!IMPORTANT]
> * Cihazınızı sürüm (GA) sürümünü çalıştırıyorsa, temasa [Microsoft Support](storsimple-contact-microsoft-support.md) güncelleştirmeyle yardımcı olmak için.
> 
> 

Düzeltme yöntemi aşağıdaki üç adımdan oluşur:

1. Düzeltmeleri Microsoft Update Kataloğu'ndan indirin.
2. Yükleyin ve normal modu düzeltmeleri doğrulayın.
3. Yükleme ve Bakım modu düzeltme (yalnızca güncelleştirme öncesi 2 yazılımlardan güncelleştirirken) doğrulayın.

#### <a name="download-updates-for-your-device"></a>Cihazınız için güncelleştirmeleri karşıdan yükle
**Cihazınızı güncelleştirme 2.1 veya 2.2 çalışıyorsa,**, indirin ve belirtilen sırayla aşağıdaki düzeltmeleri yüklemeniz gerekir:

| Sırası | KB | Açıklama | Güncelleştirme türü | Yükleme saati |
| --- | --- | --- | --- | --- |
| 1. |KB3186843 |Yazılım güncelleştirme &#42; |Normal <br></br>Olmayan kesintiye uğratan |~ 45 dakika |
| 2. |KB3186859 |LSI sürücü ve bellenim |Normal <br></br>Olmayan kesintiye uğratan |~ 20 dakika |
| 3. |KB3121261 |Storport ve Spaceport düzeltme </br> Windows Server 2012 R2 |Normal <br></br>Olmayan kesintiye uğratan |~ 20 dakika |

&#42;  *Not yazılım güncelleştirmesi iki ikili dosyaları içerir: aygıt yazılım güncelleştirmesi başında ile `all-hcsmdssoftwareupdate` ve ile başlayan CIS ve Mds Aracısı `all-cismdsagentupdatebundle`. Aygıt yazılım güncelleştirmesi önce CIS ve Mds aracısı yüklü olmalıdır. Etkin denetleyicisi aracılığıyla yeniden başlatmalısınız `Restart-HcsController` CIS ve Mds Aracısı uyguladıktan sonra cmdlet güncelleştirme (ve bir kalan uygulama güncelleştirmeleri önce).* 

**Cihazınızı güncelleştirme 0.1, 0.2, 0.3, 1.0, 1.1, 1.2 veya 2.0 çalışıp çalışmadığını**, indirip (önceki tabloda gösterilen), LSI sürücü ve bellenim güncelleştirmeleri belirlenen sırada yazılım ek olarak aşağıdaki düzeltmeleri yüklemeniz gerekir:

| Sırası | KB | Açıklama | Güncelleştirme türü | Yükleme saati |
| --- | --- | --- | --- | --- |
| 4. |KB3146621 |iSCSI paketi |Normal <br></br>Olmayan kesintiye uğratan |~ 20 dakika |
| 5. |KB3103616 |WMI paketi |Normal <br></br>Olmayan kesintiye uğratan |~ 12 dakika |

<br></br>

**Cihazınızı sürümleri 0.2, 0.3, 1.0, 1.1 ve 1.2 çalışıyorsa,**, yukarıdaki tabloda gösterilen tüm güncelleştirmeleri üstünde disk Bellenim güncelleştirmeleri yüklemeniz gerekebilir. Disk Bellenim güncelleştirmeleri çalıştırarak gerekmediğini doğrulayabilirsiniz `Get-HcsFirmwareVersion` cmdlet'i. Bu bellenim sürümleri çalıştırıyorsanız: `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`, sonra da bu güncelleştirmeleri yüklemek gerekmez.

| Sırası | KB | Açıklama | Güncelleştirme türü | Yükleme saati |
| --- | --- | --- | --- | --- |
| 6. |KB3121899 |Disk bellenim |Bakım <br></br>Kesintiye uğratan |~ 30 dakika |

<br></br>

> [!IMPORTANT]
> * Bu yordamı güncelleştirme 3'ü uygulamak için yalnızca bir kez gerçekleştirilmesi gerekir. Sonraki güncelleştirmeleri uygulamak için Klasik Azure portalını kullanabilirsiniz.
> * Toplam yükleme saatini güncelleştirme 2.2 güncelleştirme, yakın 1.1 saattir.
> * Güncelleştirmeyi uygulamak için bu yordamı kullanmadan önce hem aygıt denetleyicileri çevrimiçi olduğundan ve tüm donanım bileşenleri sağlıklı olduğundan emin olun.
> 
> 

Karşıdan yüklemek ve düzeltmeleri yüklemek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-install-update3-hotfix](../../includes/storsimple-install-update3-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [güncelleştirme 3 Sürüm](storsimple-update3-release-notes.md).

