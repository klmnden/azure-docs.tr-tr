---
title: StorSimple 8000 serisi donanım bileşenini değiştirme | Microsoft Docs
description: Güvenli bir şekilde PCMs, pil, denetleyici modülleri, EBOD denetleyicileri, disk sürücülerini ve StorSimple cihaz kasası nasıl değiştirileceğini açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/02/2017
ms.author: alkohli
ms.custom: ''
ms.openlocfilehash: e05a37122647d4979089f0ba00b1fc15f9b84b0f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60321826"
---
# <a name="replace-a-hardware-component-on-your-storsimple-8000-series-device"></a>StorSimple 8000 serisi Cihazınızda bir donanım bileşenini değiştirme

## <a name="overview"></a>Genel Bakış
Donanım bileşeni değiştirme öğreticileri, Microsoft Azure StorSimple 8000 serisi Cihazınızı kaldırmak ve bunları değiştirmek gerekli adımları ve donanım bileşenleri açıklanmaktadır. Bu makalede güvenlik simgeleri açıklar, ayrıntılı öğreticilere işaretçileri sağlar ve değiştirilebilir olmayan bileşenleri listeler.

> [!IMPORTANT]
> Herhangi bir StorSimple bileşeni değiştirmek veya kaldırmak denemeden önce gözden geçirmenizi emin olun [güvenliği simgesi kuralları](#safety-icon-conventions) ve diğer [güvenlik önlemlerini](storsimple-safety.md).


### <a name="safety-icon-conventions"></a>Güvenlik simgesi kuralları
Aşağıdaki tabloda bu öğreticilerde kullanılan güvenlik simgeler açıklanmaktadır. Kaldırın ve cihaz bileşenleri değiştirin adımlarını kullandıkça Kapat bu güvenlik simgeler dikkat edin.

| Simge | Text | Ek bilgiler |
|:--- |:--- |:--- |
| ![uyarı simgesi](./media/storsimple-hardware-component-replacement/Warning.png) |**TEHLİKE!** |Önlenmiş değil, ayrıca ölüm ya da ciddi neden olur, zararlı bir durumu gösterir. Bu sinyal Word'ün en aşırı durumlarla sınırlıdır. |
| ![uyarı simgesi](./media/storsimple-hardware-component-replacement/Warning.png) |**UYARI!** |Önlenmiş değil, ayrıca ölüm ya da ciddi neden olabilir, zararlı bir durumu gösterir. |
| ![Uyarı simgesi](./media/storsimple-hardware-component-replacement/Caution.png) |**UYARI!** |Önlenmiş değil, küçük veya Orta kasıt neden olabilir, zararlı bir durumu gösterir. |
| ![Bildirim simgesi](./media/storsimple-hardware-component-replacement/NoticeIcon.png) |**UYARI:** |Önemli ancak tehlike ilgili değil olarak kabul bilgileri gösterir. |
| ![Elektrik Şoku simgesi](./media/storsimple-hardware-component-replacement/Electric.png) |**Elektrik Şoku tehlike** |Yüksek voltaj gösterir. |
| ![Büyük ağırlık simgesi](./media/storsimple-hardware-component-replacement/Weight.png) |**Büyük ağırlık** | |
| ![Hiçbir kullanıcı tarafından bakımı yapılabilen parça simgesi](./media/storsimple-hardware-component-replacement/NoUserServiceableParts.png) |**Kullanıcı tarafından bakımı yapılabilen parça yok** |Düzgün şekilde eğitim almış sürece erişim sağlanır. |
| ![Okuma yönergeleri simgesi](./media/storsimple-hardware-component-replacement/ReadInstructions.png) |**Tüm yönergeleri ilk okuyun** | |
| ![İpucu tehlike simgesi](./media/storsimple-hardware-component-replacement/TipHazard.png) |**İpucu tehlike** | |

### <a name="before-you-begin"></a>Başlamadan önce
Bu öğreticide kullanılan, cihaz ve güvenlik simgeler hakkında güvenlik bilgileri edinin. Git [güvenli bir şekilde yüklemek ve StorSimple Cihazınızı çalışması](storsimple-safety.md) eksiksiz bilgi. Gözden geçirmeyi unutmayın [güvenlik önlemlerini](storsimple-safety.md#handling-precautions) StorSimple Cihazınızı işlemek önce.

Bir bileşenini değiştirme girişiminde bulunmadan önce aşağıdaki bilgileri göz önünde bulundurun.

![Uyarı simgesi](./media/storsimple-hardware-component-replacement/Warning.png) ![elektrik Şoku simgesi](./media/storsimple-hardware-component-replacement/Electric.png) **uyarı!**

* Kendinize, düzgün bir Elektrostatik taburcu veya antistatic mat modüller ve StorSimple Cihazınızı bileşenlerinin işlerken kullanarak raporlar.
* Tüm devresi kullanmıyorsunuz. Devresi kullanıma sunması bileşenleri işlerken sağlanan tanıtıcıları ve kılavuzları kullanın.

![Uyarı simgesi](./media/storsimple-hardware-component-replacement/Warning.png) ![fark simgesi](./media/storsimple-hardware-component-replacement/NoticeIcon.png) **dikkat edin:**

Bir modül değiştirdiğinizde **arka kutu içinde bir boş yuva asla terk**. Bir değiştirme ya da boş modülü sorun bölümü kaldırmadan önce edinin.

## <a name="hardware-component-replacement-procedures"></a>Donanım bileşeni değiştirme yordamları
StorSimple 8000 serisi Cihazınızı birincil ve EBOD kasaları birçok eklenti modül oluşur. 8600 birincil muhafaza ve EBOD muhafazası çift muhafaza cihazla bilgileriyse 8100 tek bir birincil kutu vardır.

Cihazınızdaki ana donanım bileşenleri aşağıdaki tabloda özetlenmiştir. Bağlantıya tıklayın **değiştirme yordamı** ilişkili eğitime gitmek için sütun.

| Bileşenler | # Var | Eklenti modülü? | Değiştirme yordamı |
|:--- |:--- |:--- |:--- |
| Kasa |1 |Hayır |[StorSimple Cihazınızda kasayı değiştirme](storsimple-8000-chassis-replacement.md) |
| Birincil denetleyicileri |2 |Evet |[StorSimple Cihazınızda bir Denetleyici Modülü değiştirin](storsimple-8000-controller-replacement.md) |
| 764W güç ve soğutma modülleri (PCMs) |2 |Evet |[StorSimple cihazınızın güç ve soğutma modülünü değiştirme](storsimple-8000-power-cooling-module-replacement.md) |
| Yedek pil |2 |Evet |[StorSimple cihazınızın yedek pil modülünü değiştirme](storsimple-8000-battery-replacement.md) |
| Disk sürücüleri |12 |Evet |[StorSimple Cihazınızda bir disk sürücüsünü değiştirme](storsimple-8000-disk-drive-replacement.md) |

**Tablo 1** birincil muhafazada donanım bileşenleri

Muhafaza birincil ve EBOD muhafazası onların g/ç modüllerde farklı. Ayrıca, farklı gücü PCMs vardır. EBOD muhafazası içinde 580 birincil muhafazada PCMs 764 W, olanlardır W. Birincil muhafazada PCMs yedek pil modülü de içerir.

| Bileşenler | # Var | Eklenti modülü? | Değiştirme yordamı |
|:--- |:--- |:--- |:--- |
| Kasa |1 |Hayır |[StorSimple Cihazınızda kasayı değiştirme](storsimple-8000-chassis-replacement.md) |
| EBOD denetleyicisi |2 |Evet |[StorSimple Cihazınızda bir EBOD denetleyicisini değiştirme](storsimple-8000-ebod-controller-replacement.md) |
| 580W güç ve soğutma modülleri (PCMs) |2 |Evet |[StorSimple cihazınızın güç ve soğutma modülünü değiştirme](storsimple-8000-power-cooling-module-replacement.md) |
| Disk sürücüleri |12 |Evet |[StorSimple Cihazınızda bir disk sürücüsünü değiştirme](storsimple-8000-disk-drive-replacement.md) |

**Tablo 2** EBOD muhafazası donanım bileşenleri

Eklenti modüller cihazda aşağıdaki ön ve arka diyagramları vurgulanır. Bir değişiklik varsa, çeşitli eklenti modüllerinin konumunu belirlemek için bu diyagramları kullanabilirsiniz. Ön diyagram disk sürücülerini ve arka diyagramları EBOD muhafazası birincil Göster ve eklenti modülleri gösterir.

![Disk sürücülerine sahip cihazın ön panelini](./media/storsimple-hardware-component-replacement/IC741028.png)

**Şekil 1** cihazın ön

| Etiket | Açıklama |
|:--- |:--- |
| 0 - 11 |Disk sürücülerini (toplam 12) |

Hem birincil kasası hem de EBOD muhafazası sürücü taşıyıcı modüller vardır. Kasa, on iki 3,5" disk tarafından 3 4 biçimde düzenlenmesi sürücüleri barındırır.

![Cihaz birincil kutusu modüllerinin devre kartı](./media/storsimple-hardware-component-replacement/IC740994.png)

**Şekil 2** birincil muhafaza arkasına

| Etiket | Açıklama |
|:--- |:--- |
| 1 |PCM 0 |
| 2 |PCM 1 |
| 3 |Denetleyici 0 |
| 4 |Denetleyici 1 |

![Aygıt EBOD muhafazası eklenti modüllerinin devre kartı](./media/storsimple-hardware-component-replacement/IC769599.png)

**Şekil 3** EBOD muhafazası arkasına

| Etiket | Açıklama |
|:--- |:--- |
| 1 |PCM 0 |
| 2 |PCM 1 |
| 3 |EBOD Denetleyicisi 0 |
| 4 |EBOD denetleyicisi 1 |

## <a name="field-replaceable-units"></a>Alan değiştirebilen birim
Aşağıdaki alan değiştirebilen birim (FRU), StorSimple cihazınız için kullanılabilir:

* Kasa (tümleşik işlemler paneli dahil)
* 764 W AC PCM
* 580 W AC PCM
* Sabit disk sürücüsü taşıyıcı modülü ile
* Denetleyici Modülü
* EBOD Denetleyici Modülü
* Yedek pili Modülü
* Parmaklık Seti rafa

Lütfen [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) herhangi birini bu değiştirme birimleri sıralamak için.

## <a name="next-steps"></a>Sonraki adımlar
Tüm gözden [güvenlik bilgileri](storsimple-safety.md) StorSimple donanım bileşenini değiştirme girişiminde bulunmadan önce.

