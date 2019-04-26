---
title: Microsoft Azure StorSimple 8000 serisi cihaz pille Değiştir | Microsoft Docs
description: Kaldır, değiştirin ve StorSimple Cihazınızı yedek pil modüldeki bakımını açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: 4ebf3f28d40e0461d140a3fe74fb940720f26db6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60418967"
---
# <a name="replace-the-backup-battery-module-on-your-storsimple-device"></a>StorSimple Cihazınızda yedek pil modülü değiştirin

## <a name="overview"></a>Genel Bakış
Birincil muhafaza güç ve soğutma Modülü (PCM) Microsoft Azure StorSimple Cihazınızda ek pil paketi vardır. Bu paketi power sağladığından AC elektrik için birincil kasası varsa veri StorSimple cihaz kaydedebilir. Bu pil paketi olarak adlandırılır *yedek pil Modülü*. Yedek pili modülü yalnızca birincil kutu (EBOD muhafazası yedek pil modülü içermiyor) StorSimple cihazınızdaki vardır.

Bu öğreticide, aşağıdaki işlemlerin nasıl yapılacağı açıklanmaktadır:

* Yedek pili modülü kaldırıldı
* Yeni bir yedek pil modülünü yükleme
* Yedek pili modülü koru

> [!IMPORTANT]
> Kaldırma ve bir yedek pil modül değiştirme gözden önce güvenlik bilgileri [StorSimple donanım bileşeni değişimi giriş](storsimple-8000-hardware-component-replacement.md).


## <a name="remove-the-backup-battery-module"></a>Yedek pili modülü kaldırıldı
Yedek pili StorSimple cihazınız için bir alan değiştirebilen birim modülüdür. PCM yüklenmeden önce pil modülü, özgün paketleme içinde depolanması gerekir. Yedek pili kaldırmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-remove-the-backup-battery-module"></a>Yedek pili modülünü kaldırmak için
1. Azure portalında StorSimple cihaz Yöneticisi hizmeti dikey pencerenize gidin. Git **cihazları** ve cihaz listesinden Cihazınızı seçin. Gidin **İzleyici** > **donanım sistem durumu**. Altında **paylaşılan bileşenleri**, pil durum öğesine bakın.
2. Pil başarısız PCM belirleyin. Şekil 1, StorSimple cihazının arkasına gösterir.
   
    ![Cihaz birincil kutusu modüllerinin devre kartı](./media/storsimple-battery-replacement/IC740994.png)
   
    **Şekil 1** arkasına birincil cihaz PCM ve denetleyici modülleri gösteriliyor
   
   | Etiket | Açıklama |
   |:--- |:--- |
   | 1 |PCM 0'DA |
   | 2 |PCM 1 |
   | 3 |Denetleyici 0 |
   | 4 |Denetleyici 1 |
   
    Sayı 3 Şekil 2'de gösterildiği gibi izleme gösterge LED'i karşılık gelen PCM 0'da bulunan **pil hata** aydınlatma.
   
    ![Cihaz PCM izleme gösterge LED'lerini devre kartı](./media/storsimple-battery-replacement/IC740992.png)
   
    **Şekil 2** arka of PCM izleme gösterge LED'lerini gösteriliyor
   
   | Etiket | Açıklama |
   |:--- |:--- |
   | 1 |AC güç kesintisi |
   | 2 |Fanı hatalı |
   | 3 |Pil hata |
   | 4 |PCM TAMAM |
   | 5 |DC Güç kesintisi |
   | 6 |Pil Sağlıklı |
3. Başarısız bir pil PCM kaldırmak için adımları [bir PCM Kaldır](storsimple-8000-power-cooling-module-replacement.md#remove-a-pcm).
4. Kaldırılan PCM ile lift ve pil modül tanıtıcısı aşağıdaki şekilde gösterildiği gibi Yukarı Döndür Kaldır pil en fazla istek.
   
    ![Pcm'den pil](./media/storsimple-battery-replacement/IC741019.png)
   
    **Şekil 3** Pcm'den pil
5. Paketleme alan değiştirebilen birim modülünde yer.
6. Hatalı birim uygun Bakım ve işleme için Microsoft'a döndürür.

## <a name="install-a-new-backup-battery-module"></a>Yeni bir yedek pil modülünü yükleme
StorSimple cihazınızın birincil muhafazada PCM değiştirme pil modülü yüklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-install-the-battery-module"></a>Pil modülünü yüklemek için
1. Yedek pili Modülü doğru yönlendirmeyi PCM yerleştirin.
2. Tüm bağlayıcı bilgisayar lisansı için pil modül tanıtıcısı basın.
3. Yönergeleri izleyerek birincil muhafazada PCM'yi değiştirme [güç ve soğutma modülü, StorSimple Cihazınızda değiştirin](storsimple-8000-power-cooling-module-replacement.md).
4. Bu değişiklik tamamlandıktan sonra cihazınıza gidin ve ardından Git **İzleyici** > **donanım sistem durumu** Azure portalında. Yüklemenin başarılı olduğunu emin olmak için pil durumunu doğrulayın. Yeşil durum pil sağlıklı olduğunu gösterir.

## <a name="maintain-the-backup-battery-module"></a>Yedek pili modülü koru
StorSimple Cihazınızı yedek pil modülü güç kaybı olayı sırasında denetleyici güç sağlar. Bu, StorSimple cihazın denetimli bir biçimde kapatmadan önce önemli verilerinizi kaydedin izin verir. İki tam şarjlı pilleri ile PCMs, sistemin iki ardışık kaybı olaylarından başa çıkabilir.

Azure portalında **donanım sistem durumu** altında **İzleyici** pil hatalı ya da son yaşam yaklaştığını dikey penceresinde gösterir. Stav baterie belirtilir **pil PCM 0'da** veya **pil PCM 1'deki** altında **paylaşılan bileşenleri**. Bu dikey pencereyi gösterecek bir **DEGRADED** ömrü sonu yaklaşan, durumunda ve **başarısız** ömür son ulaşıldı.

> [!NOTE]
> Pil bildirebilirsiniz **başarısız** ihtiyaç duyduğu sırada erişir yalnızca uygulanacak.


Varsa **DEGRADED** durumu görünür, eylem aşağıdaki kursu öneririz:

* Sistem son güç kaybı yaşamış olabilirsiniz veya pil düzenli bakım aşamasında olabilir. Devam etmeden önce 12 saat olarak sistemi gözlemleyin.
  
  * Durumu hala ise **DEGRADED** PCMs çalıştıran ve denetleyicileri ile AC sürekli bağlantı 12 saatlik güç sonra ardından pil değiştirilmesi gerekiyor. Lütfen [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) yedek pili değiştirme modülü için.
  * 12 saat sonra Tamam durumu hale gelirse, pil çalışır durumda ve bir bakım ücreti yalnızca gerekli.
* İlişkili bir AC güç kaybı çalıştırılmadı ve PCM açık ve bağlı AC gücü, pil değiştirilmesi gerekiyor. [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) yedek pili değiştirme modül sıralamak için.

> [!IMPORTANT]
> Ulusal ve bölgesel düzenlemeleri göre başarısız pil Dispose.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [StorSimple donanım bileşeni değişimi](storsimple-8000-hardware-component-replacement.md).

