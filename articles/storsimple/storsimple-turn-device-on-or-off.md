---
title: StorSimple 8000 serisi Cihazınızı açma veya kapatma | Microsoft Docs
description: Yeni bir StorSimple cihazı açın, kapatıldığı veya güç kayıp bir cihazı açın ve çalışan bir cihazı kapatıp açıklanmaktadır.
services: storsimple
documentationcenter: ''
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: 8e9c6e6c-965c-4a81-81bd-e1c523a14c82
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/09/2018
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 29a45c0d32e35b5d321670bf25334a2976b93e56
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64693670"
---
# <a name="turn-on-or-turn-off-your-storsimple-8000-series-device"></a>Açma veya kapatma StorSimple 8000 serisi Cihazınızı devre dışı

## <a name="overview"></a>Genel Bakış
Bir Microsoft Azure StorSimple cihazı kapatma normal sistem işleminin bir parçası olarak gerekli değildir. Ancak, yeni bir cihaz veya kapatma için olan bir cihaz açmanız gerekebilir. Genellikle, bir kapanma içinde gerekir başarısız donanımı değiştirmeniz, fiziksel bir birim taşıdığınızda veya bir cihaz hizmet dışına ele durumlarda gereklidir. Bu öğreticide, açma ve StorSimple Cihazınızı farklı senaryolarda kapatma için gerekli yordam açıklanmaktadır.

## <a name="turn-on-a-new-device"></a>Yeni bir cihazı açın
Bir StorSimple cihazında ilk kez kapatma adımları, cihazın olup 8100 veya 8600 model bağlı olarak farklılık gösterir. 8600 birincil muhafaza ve EBOD muhafazası çift muhafaza cihazla bilgileriyse 8100 tek bir birincil kutu vardır. Ayrıntılı adımlar için iki modeli de, aşağıdaki bölümlerde ele alınmıştır.

* [Yalnızca birincil kasası ile yeni cihaz](#new-device-with-primary-enclosure-only)
* [Yeni cihaz EBOD muhafazası ile](#new-device-with-ebod-enclosure)

### <a name="new-device-with-primary-enclosure-only"></a>Yalnızca birincil kasası ile yeni cihaz
Tek kutu cihaz StorSimple 8100 modelidir. Cihazınız, gereksiz güç ve soğutma modülleri (PCMs) içerir. Hem PCMs yüklenmeli ve yüksek kullanılabilirlik sağlamak için farklı güç kaynaklarına bağlandınız.

Güç için cihazınızın kablolarını bağlama için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

> [!NOTE]
> Tam cihaz kurulumu ve kablolama yönergeleri için Git [StorSimple 8100 cihazınızın yükleme](storsimple-8100-hardware-installation.md). Tam olarak yönergeleri izlediğinizden emin olun.
> 
> 

### <a name="new-device-with-ebod-enclosure"></a>Yeni cihaz EBOD muhafazası ile
StorSimple 8600 model, hem birincil bir kutu hem de EBOD muhafazası sahiptir. Bu birimlerin birlikte seri ekli SCSI (SAS) bağlantı ve güç için kablolu gerektirir.

Bu cihazı ilk kez ayarlarken, ilk SAS kabloları adımları uygulayın ve ardından power kablo adımlarını tamamlayın.

[!INCLUDE [storsimple-sas-cable-8600](../../includes/storsimple-sas-cable-8600.md)]

[!INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

> [!NOTE]
> Tam cihaz kurulumu ve kablolama yönergeleri için Git [StorSimple 8600 cihazınızın yükleme](storsimple-8600-hardware-installation.md). Tam olarak yönergeleri izlediğinizden emin olun.

## <a name="turn-on-a-device-after-shutdown"></a>Bir cihazı kapatıldıktan sonra açın
Bunu kapatıldı sonra bir StorSimple cihazında kapatma adımları cihazın olup 8100 veya 8600 model bağlı olarak farklılık gösterir. 8600 birincil muhafaza ve EBOD muhafazası çift muhafaza cihazla bilgileriyse 8100 tek bir birincil kutu vardır.

* [Yalnızca birincil kasası ile cihaz](#device-with-primary-enclosure-only)
* [Cihaz EBOD muhafazası ile](#device-with-ebod-enclosure)

### <a name="device-with-primary-enclosure-only"></a>Yalnızca birincil kasası ile cihaz
Bir kapatmadan sonra birincil bir kutu ve hiçbir EBOD muhafazası bir StorSimple cihazına etkinleştirmek için aşağıdaki yordamı kullanın.

#### <a name="to-turn-on-a-device-with-a-primary-enclosure-only"></a>Bir cihazı yalnızca birincil bir kasa ile etkinleştirmek için
1. Her iki gücüyle power geçer ve soğutma modülleri (PCMs) kapalı konumda olduğundan emin olun. Anahtarlar OFF konumda değilse, kapalı konuma ters çevirin ve alarmın ışıkları bekleyin.
2. Her iki PCMs açık konuma üzerinde güç anahtarları döndürerek cihazı açın. Cihaz etkinleştirmeniz gerekir.
3. Cihazı tamamen açık olduğunu doğrulamak için aşağıdakileri denetleyin:
   
   1. Tamam LED'lerini üzerinde hem PCM modülleri yeşildir.
   2. ' % S'durumu LED'lerini her iki denetleyicide Düz yeşil hazırdır.
   3. Denetleyicilerden biri üzerinde mavi LED yanıp, denetleyici etkin olduğunu gösterir.
      
      Bu koşullardan biri karşılanmazsa, cihazınızın iyi durumda değil. Lütfen [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md).

### <a name="device-with-ebod-enclosure"></a>Cihaz EBOD muhafazası ile
Bir kapatmadan sonra birincil bir kutu ve bir EBOD muhafazası bir StorSimple cihazına etkinleştirmek için aşağıdaki yordamı kullanın. Her adımda açıklandığı gibi tam olarak sırayla gerçekleştirin. Bunun yapılmaması, veri kaybına neden olabilir.

#### <a name="to-turn-on-a-device-with-a-primary-and-an-ebod-enclosure"></a>Birincil ve EBOD muhafazası sahip bir cihazda etkinleştirmek için
1. EBOD muhafazası için birincil muhafaza bağlı olduğundan emin olun. Daha fazla bilgi için [StorSimple 8600 cihazınızın yükleme](storsimple-8600-hardware-installation.md).
2. Güç ve soğutma modülleri (PCMs) EBOD hem birincil kasaları OFF konumda olduğundan emin olun. Anahtarlar OFF konumda değilse, kapalı konuma ters çevirin ve alarmın ışıkları bekleyin.
3. EBOD muhafazası Aç ilk power döndürerek üzerinde hem PCMs açık konuma geçer. PCM LED'leri yeşil olmalıdır. Bu birimi bir yeşil EBOD denetleyicisi LED EBOD muhafazası üzerinde olduğunu gösterir.
4. Her iki PCMs açık konuma üzerinde güç anahtarları döndürerek birincil muhafaza açın. Tüm sistemin şimdi üzerinde olmalıdır.
5. Hangi EBOD Muhafazası ve birincil muhafaza arasındaki bağlantıyı iyi olmasını sağlar, SAS LED'lerini yeşil, olduğunu doğrulayın.

## <a name="turn-on-a-device-after-a-power-loss"></a>Bir cihazı sonra güç kaybı açın
Güç kesintisi ya da kesinti bir StorSimple cihazı kapatmanız yeterlidir. Güç kesintisi güç kaynakları birini veya her iki güç kaynakları gerçekleşebilir. Kurtarma adımları, cihazın bir 8100 veya 8600 model olduğuna bağlı olarak farklılık gösterir. 8600 birincil muhafaza ve EBOD muhafazası çift muhafaza cihazla bilgileriyse 8100 tek bir birincil kutu vardır. Bu bölümde, her senaryo için kurtarma yordamı açıklar.

* [Yalnızca birincil kasası ile cihaz](#8100)
* [Cihaz EBOD muhafazası ile](#8600)

### <a name="device-with-primary-enclosure-only-a-name8100"></a>Yalnızca birincil kasası ile cihaz <a name="8100">
Güç kaybı, güç kaynakları birine ise sistemin normal işleyişi devam edebilirsiniz. Ancak, cihazın yüksek kullanılabilirlik sağlamak için güç güç kaynağına olabildiğince çabuk geri yükleyin.

Sistem bir güç kesintisi ya da her iki güç kaynakları güç kesintisi ise, düzenli ve denetimli bir şekilde kapanır. Güç geri geldiğinde, sistem otomatik olarak aç.

### <a name="device-with-ebod-enclosure-a-name8600"></a>Cihaz EBOD muhafazası ile <a name="8600">
#### <a name="power-loss-on-one-power-supply"></a>Güç kayıplarına bağlı bir güç kaynağı
Güç kaybı, güç kaynakları birincil kasası veya EBOD muhafazası birine ise sistemin normal işleyişi devam edebilirsiniz. Ancak, cihazın yüksek kullanılabilirlik sağlamak için lütfen power güç kaynağına olabildiğince çabuk geri yükleyin.

#### <a name="power-loss-on-both-power-supplies-on-primary-and-ebod-enclosures"></a>Güç kayıplarına bağlı birincil ve ebod üzerinde hem güç kaynakları
Güç kesintisi ya da her iki güç kaynakları güç kesintisi ise EBOD muhafazası hemen kapanacak ve birincil muhafaza otomatikleştirir ve denetimli bir şekilde kapanır. Gereç, güç geri geldiğinde otomatik olarak başlatılacak.

Ardından power el ile kapalı, güç sistemine geri yüklemek için aşağıdaki adımları uygulayın.

1. EBOD muhafazası üzerinde açın.
2. EBOD muhafazası açıktır sonra birincil muhafaza etkinleştirin.

### <a name="power-loss-on-both-power-supplies-on-ebod-enclosure"></a>EBOD muhafazası üzerinde hem güç kaynakları güç kaybı
, Kablolar ayarladığınızda EBOD hiçbir zaman tek başına bir ayrı PDU için bağlı olduğundan emin olmanız gerekir. Sistem, birincil ve EBOD aynı anda başarısız olursa, kurtarılır.

EBOD muhafazası hem güç kaynakları üzerinde başarısız olursa yalnızca, sistem otomatik olarak kurtarılacak değil. Sistemde kapatmak ve sağlıklı bir duruma geri yüklemek için aşağıdaki adımları uygulayın:

1. Birincil muhafaza açıksa, güç ve soğutma modülleri (PCMs) devre dışı geçin.
2. Sistemin kapatmak birkaç dakika bekleyin.
3. EBOD muhafazası üzerinde açın.
4. EBOD muhafazası açıktır sonra birincil muhafaza etkinleştirin.

## <a name="turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost"></a>Bir cihazı açın sonra birincil ve EBOD muhafazası bağlantısı kesildiği
Hazır bekleyen denetleyicinin ve karşılık gelen EBOD denetleyicisi arasında bağlantı kaybedilirse cihaz çalışmaya devam eder. Sistem etkin denetleyiciyi ve karşılık gelen EBOD denetleyicisi arasındaki bağlantı kaybı olduğunda yük devretme oluşması ve cihazın normal şekilde çalışmaya devam etmesi gerekir.

Cihaz EBOD Muhafazası ve birincil muhafaza arasındaki bağlantıyı zarar görmesi veya hem seri ekli SCSI (SAS) kablolar kaldırılır, çalışmayı durdurur. Bu noktada, aşağıdaki adımları gerçekleştirin.

### <a name="to-turn-on-the-device-after-connection-is-lost"></a>Bağlantı kaybedildikten sonra cihazda etkinleştirmek için
1. Cihazın arkasına erişin.
2. EBOD Muhafazası ve birincil muhafaza arasında SAS kablo bağlantısı kesildiğinde, EBOD muhafazası tüm SAS yolundaki LED'lerini devre dışı olacaktır.
3. Güç ve soğutma modülleri (PCMs) aşağı EBOD Muhafazası ve birincil Kasası'nı kapatın.
4. Her iki kasaları arkasında tüm ışıkların devre dışı bırakmak kadar bekleyin.
5. SAS kablolu jbod'lere takın ve EBOD Muhafazası ve birincil muhafaza arasında iyi bir bağlantı olduğundan emin olun.
6. EBOD muhafazası açma ilk iki PCM döndürerek açık konuma geçer.
7. EBOD muhafazası üzerinde yeşil LED açık olduğunu kontrol ederek olduğundan emin olun.
8. Birincil muhafaza açın.
9. Birincil kasası üzerinde denetleyicisi yeşil LED açık olduğunu kontrol ederek olduğundan emin olun.
10. SAS Kulvar LED'lerini (EBOD denetleyici başına dört) olan tüm şirket denetleyerek birincil muhafaza EBOD muhafazası bağlantıyla iyi olduğundan emin olun.

> [!IMPORTANT]
> SAS kablolu jbod'lere bozuk veya sistemde açtığınızda EBOD Muhafazası ve birincil muhafaza arasındaki bağlantıyı iyi değil, bu kurtarma moduna geçer. Lütfen [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) böyle bir durumda.


## <a name="turn-off-a-running-device"></a>Çalışan bir cihazı kapatıp
Çalışan bir StorSimple cihazı hizmet dışı geçen taşınırken veya değiştirilmesi gereken düzgün çalışmayan bir bileşen varsa kapatılması gerekebilir. Adımlar, StorSimple cihazı olup 8100 veya 8600 model bağlı olarak değişir. 8600 birincil muhafaza ve EBOD muhafazası çift muhafaza cihazla bilgileriyse 8100 tek bir birincil kutu vardır. Bu bölümde, çalışan bir cihazı kapatmak için adımları açıklanmaktadır.

* [Birincil cihaz](#8100a)
* [Cihaz EBOD muhafazası ile](#8600a)

### <a name="device-with-primary-enclosure-a-name8100a"></a>Birincil cihaz <a name="8100a">
Sıralı ve denetimli bir şekilde cihazı kapatmak için Azure portalı üzerinden veya Windows PowerShell aracılığıyla StorSimple için bunu yapabilirsiniz. 

> [!IMPORTANT]
> Çalışan bir cihazı, cihaz arkasındaki güç düğmesini kullanarak kapatmayın.
> 
> Cihazı kapatmadan önce tüm cihaz bileşenleri iyi durumda olduğundan emin olun. Azure portalında gidin **cihazları** > **İzleyici** > **donanım sistem durumu**ve tüm bileşenlerin durumunun yeşil olduğunu doğrulayın. Bu işlem yalnızca sağlıklı bir sistem için de geçerlidir. Sistem aşağı kapatılıyor düzgün çalışmayan bir bileşenini değiştirme, başarısız (kırmızı) görür veya ilgili bileşen (sarı) durumunun düşürülmüş **donanım durumunu**.
> 
> 

Windows PowerShell için StorSimple'ı veya Azure portalında eriştikten sonra adımları [bir StorSimple cihazı kapatmak](storsimple-8000-manage-device-controller.md#shut-down-a-storsimple-device). 

### <a name="device-with-ebod-enclosure-a-name8600a"></a>Cihaz EBOD muhafazası ile <a name="8600a">
> [!IMPORTANT]
> Muhafaza birincil ve EBOD muhafazası aşağı kapatmadan önce tüm cihaz bileşenleri iyi durumda olduğundan emin olun. Azure portalında gidin **cihazları** > **İzleyici** > **donanım sistem durumu**ve tüm bileşenleri iyi durumda olduğunu doğrulayın.


#### <a name="to-shut-down-a-running-device-with-ebod-enclosure"></a>EBOD muhafazası çalıştıran bir cihazla kapatmak için
1. Listelenen tüm adımları izleyin [bir StorSimple cihazı kapatmak](storsimple-8000-manage-device-controller.md#shut-down-a-storsimple-device) birincil kasası için.
2. Birincil muhafaza kapattığınızda, hem kapatma döndürerek EBOD kapatın ve soğutma Modülü (PCM) geçer.
3. EBOD kapatıldı doğrulamak için tüm ışıkları EBOD muhafazası arkasında kapalı olduğunu kontrol edin.

> [!NOTE]
> Sistem kapatılır sonra birincil kasasına EBOD muhafazası bağlanmak için kullanılan SAS kablolu jbod'lere kadar kaldırılmamalıdır.

## <a name="next-steps"></a>Sonraki adımlar
[Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) açma veya kapatma bir StorSimple cihazı sorunlarla karşılaşırsanız.

