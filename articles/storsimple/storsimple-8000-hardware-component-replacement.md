---
title: "StorSimple 8000 serisi donanım bileşeni değiştirme | Microsoft Docs"
description: "Güvenli bir şekilde PCMs, pil, denetleyici modülleri, EBOD denetleyicileri, disk sürücüleri ve StorSimple cihazını kasa değiştirme açıklar."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/02/2017
ms.author: alkohli
ms.custom: 
ms.openlocfilehash: 6de50c5031db59176bdf17ecc69b934559220f6a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="replace-a-hardware-component-on-your-storsimple-8000-series-device"></a>Donanım bileşeni, StorSimple 8000 serisi Cihazınızda Değiştir

## <a name="overview"></a>Genel Bakış
Donanım bileşeni değiştirme öğreticileri donanım bileşenleri Microsoft Azure StorSimple 8000 serisi Cihazınızı ve kaldırma ve bunları değiştirmek için gereken adımları açıklar. Bu makalede güvenliği simgelerini açıklar, ayrıntılı öğreticileri işaretçiler sağlar ve değiştirebilen bileşenlerini listeler.

> [!IMPORTANT]
> Kaldırın veya herhangi bir StorSimple bileşeni değiştirmek denemeden önce gözden geçirmenizi emin olun [güvenliği simgesi kuralları](#safety-icon-conventions) ve diğer [güvenlik önlemlerini](storsimple-safety.md).


### <a name="safety-icon-conventions"></a>Güvenlik simgesi kuralları
Aşağıdaki tabloda bu öğreticileri kullanılan güvenlik simgeleri açıklanmaktadır. Kaldırın ve cihaz bileşenlerini değiştirmek için adımlara devam ederken Kapat bu güvenlik simgeleri dikkat edin.

| Simgesi | Metin | Ek bilgiler |
|:--- |:--- |:--- |
| ![Uyarı simgesi](./media/storsimple-hardware-component-replacement/Warning.png) |**TEHLİKE!** |Kaçınılması değil, ölüm veya ciddi yaralanma neden olur, zararlı bir durumu gösterir. Bu sinyal Word'ün en aşırı durumlarda sınırlıdır. |
| ![Uyarı simgesi](./media/storsimple-hardware-component-replacement/Warning.png) |**UYARI!** |Kaçınılması değil, ölüm veya ciddi yaralanma neden olabilir, zararlı bir durumu gösterir. |
| ![Uyarı simgesi](./media/storsimple-hardware-component-replacement/Caution.png) |**DİKKAT!** |Kaçınılması değil, küçük veya Orta yaralanma neden olabilir, zararlı bir durumu gösterir. |
| ![Bildirim simgesi](./media/storsimple-hardware-component-replacement/NoticeIcon.png) |**UYARI:** |Önemli ancak hasar ilgili olmayan kabul bilgi gösterir. |
| ![Elektrik Şoku simgesi](./media/storsimple-hardware-component-replacement/Electric.png) |**Elektrik Şoku hasar** |Yüksek voltaj gösterir. |
| ![Büyük ağırlık simgesi](./media/storsimple-hardware-component-replacement/Weight.png) |**Büyük ağırlık** | |
| ![Hiçbir kullanıcı tarafından bakımı yapılabilen parça simgesi](./media/storsimple-hardware-component-replacement/NoUserServiceableParts.png) |**Hiçbir kullanıcı tarafından bakımı yapılabilen parça** |Düzgün şekilde eğitim almış sürece erişmez. |
| ![Okuma yönergeleri simgesi](./media/storsimple-hardware-component-replacement/ReadInstructions.png) |**Önce tüm yönergelerini okuyun** | |
| ![İpucu tehlike simgesi](./media/storsimple-hardware-component-replacement/TipHazard.png) |**İpucu tehlike** | |

### <a name="before-you-begin"></a>Başlamadan önce
Bu öğreticide kullanılan aygıt ve güvenliği simgelerini güvenlik bilgilerini tanıyın. Git [güvenli bir şekilde yüklemek ve StorSimple Cihazınızı çalıştırmak](storsimple-safety.md) tam bilgi için. Gözden geçirmeyi unutmayın [güvenlik önlemlerini](storsimple-safety.md#handling-precautions) StorSimple Cihazınızı işlemek önce.

Bir bileşenin değiştirmeye çalışmadan önce aşağıdaki bilgileri göz önünde bulundurun.

![Uyarı simgesi](./media/storsimple-hardware-component-replacement/Warning.png) ![elektrik Şoku simgesi](./media/storsimple-hardware-component-replacement/Electric.png) **uyarı!**

* Kendiniz düzgün modülleri ve StorSimple Cihazınızı bileşenlerinin işlerken bir Elektrostatik deşarj veya antistatic mat kullanarak plan.
* Tüm devresi touch değil. Sağlanan tanıtıcı ve Kılavuzlar devresi kullanıma bileşenleri işlerken kullanın.

![Uyarı simgesi](./media/storsimple-hardware-component-replacement/Warning.png) ![fark simgesi](./media/storsimple-hardware-component-replacement/NoticeIcon.png) **dikkat edin:**

Bir modül değiştirdiğinizde **hiçbir zaman bir boş yuva muhafaza arkada ayrılmaz**. Değiştirme veya boş modülü sorun bölümü kaldırmadan önce edinin.

## <a name="hardware-component-replacement-procedures"></a>Donanım bileşeni değiştirme yordamları
StorSimple 8000 serisi Cihazınızı birincil ve EBOD kutularının birçok eklenti modül oluşur. 8600 birincil muhafaza ve EBOD muhafazası çift muhafaza aygıtla iken 8100 tek bir birincil muhafazada sahiptir.

Cihazınızı ana donanım bileşenleri aşağıdaki tabloda özetlenmiştir. Bağlantıya tıklayın **değiştirme yordamı** ilişkili öğreticiye gitmek için sütun.

| Bileşenler | # Var | Eklenti modülü? | Değiştirme yordamı |
|:--- |:--- |:--- |:--- |
| Kasa |1 |Hayır |[Kasa, StorSimple Cihazınızda değiştirin](storsimple-8000-chassis-replacement.md) |
| Birincil denetleyicileri |2 |Evet |[Bir denetleyici modülü, StorSimple Cihazınızda değiştirin](storsimple-8000-controller-replacement.md) |
| 764W güç ve soğutma modülleri (PCMs) |2 |Evet |[StorSimple Cihazınızda güç ve soğutma modülü değiştirin](storsimple-8000-power-cooling-module-replacement.md) |
| Yedek pil |2 |Evet |[StorSimple Cihazınızda yedek pil modülü değiştirin](storsimple-8000-battery-replacement.md) |
| Disk sürücüleri |12 |Evet |[StorSimple Cihazınızı disk sürücüsünde değiştirin](storsimple-8000-disk-drive-replacement.md) |

**Tablo 1** birincil muhafazada donanım bileşenleri

Birincil muhafaza ve EBOD muhafazası onların g/ç modülleri farklılık gösterir. Buna ek olarak PCMs farklı gücü sahiptir. EBOD muhafazada 580 PCMs birincil muhafazada 764 W, olanlardır W. Birincil muhafazada PCMs bir yedek pil modülü de içerir.

| Bileşenler | # Var | Eklenti modülü? | Değiştirme yordamı |
|:--- |:--- |:--- |:--- |
| Kasa |1 |Hayır |[Kasa, StorSimple Cihazınızda değiştirin](storsimple-8000-chassis-replacement.md) |
| EBOD denetleyicisi |2 |Evet |[EBOD denetleyicisi, StorSimple Cihazınızda değiştirin](storsimple-8000-ebod-controller-replacement.md) |
| 580W güç ve soğutma modülleri (PCMs) |2 |Evet |[StorSimple Cihazınızda güç ve soğutma modülü değiştirin](storsimple-8000-power-cooling-module-replacement.md) |
| Disk sürücüleri |12 |Evet |[StorSimple Cihazınızı disk sürücüsünde değiştirin](storsimple-8000-disk-drive-replacement.md) |

**Tablo 2** EBOD muhafazası donanım bileşenleri

Eklenti modülleri cihazdaki aşağıdaki ön ve arka diyagramları vurgulanır. Bu diyagramları çeşitli eklenti modüllerin konumu olarak bir yedek gerekli olup olmadığını belirlemek için kullanabilirsiniz. Ön diyagram disk sürücülerini ve EBOD Muhafazası ve birincil muhafaza Göster arka diyagramları eklenti modülleri gösterir.

![Disk sürücüleri ile cihazın ön düzlemi](./media/storsimple-hardware-component-replacement/IC741028.png)

**Şekil 1** cihazın ön

| Etiket | Açıklama |
|:--- |:--- |
| 0 - 11 |Disk sürücülerini (12 toplam) |

Hem birincil muhafaza hem de EBOD muhafazası sürücü taşıyıcı modülleri vardır. Kasa on iki 3,5" disk 3 tarafından 4 biçiminde düzenlenmiş sürücüleri barındırır.

![Aygıt birincil muhafaza modüllerinin devre kartı](./media/storsimple-hardware-component-replacement/IC740994.png)

**Şekil 2** birincil muhafaza arkasına

| Etiket | Açıklama |
|:--- |:--- |
| 1 |PCM 0 |
| 2 |PCM 1 |
| 3 |Denetleyici 0 |
| 4 |Denetleyici 1 |

![Cihaz EBOD muhafazası eklenti modüllerinin devre kartı](./media/storsimple-hardware-component-replacement/IC769599.png)

**Şekil 3** EBOD muhafazası arkasına

| Etiket | Açıklama |
|:--- |:--- |
| 1 |PCM 0 |
| 2 |PCM 1 |
| 3 |EBOD denetleyici 0 |
| 4 |EBOD Denetleyici 1 |

## <a name="field-replaceable-units"></a>Alan değiştirebilen birim
Aşağıdaki alan birimler (FRU), StorSimple cihazınız için kullanılabilir:

* Kasa (tümleşik işlemleri panel dahil)
* 764 W AC PCM
* 580 W AC PCM
* Sürücü taşıyıcı modülü ile sabit disk sürücüsü
* Denetleyici Modülü
* EBOD Denetleyici Modülü
* Yedek pil Modülü
* Raylar Seti rafa

Lütfen [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) bu değiştirme birimleri hiçbirini sıralamak için.

## <a name="next-steps"></a>Sonraki adımlar
Tüm gözden [güvenlik bilgileri](storsimple-safety.md) StorSimple donanım bileşeni değiştirmeye çalışmadan önce.

