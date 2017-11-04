---
title: "StorSimple 8000 serisi Cihazınızı Aç veya kapat | Microsoft Docs"
description: "Yeni bir StorSimple cihazında kapatın, kapatıldı veya güç kesintisi bir cihazda etkinleştirmek ve çalışan bir aygıtı devre dışı açıklanmaktadır."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 8e9c6e6c-965c-4a81-81bd-e1c523a14c82
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/29/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0577c837e0c47ba37a4f586603b0f5b951f1b549
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="turn-on-or-turn-off-your-storsimple-8000-series-device"></a>Açma veya StorSimple 8000 serisi aygıtınızı kapatın
## <a name="overview"></a>Genel Bakış
Bir Microsoft Azure StorSimple cihazı kapatmak normal sistem işleminin bir parçası olarak gerekli değildir. Ancak, yeni bir cihaz veya kapatılmak zorunda kalındı bir aygıtı açmanız gerekebilir. Genellikle, bir kapanma içinde gerekir başarısız donanımı değiştirmeniz, fiziksel olarak bir birimi taşıdığınızda veya bir aygıt hizmet dışına ele durumlarda gereklidir. Bu öğretici, açma ve StorSimple Cihazınızı farklı senaryolarında kapatma gerekli yordamı açıklar.

## <a name="turn-on-a-new-device"></a>Yeni bir aygıtı açın
Bir StorSimple cihaz üzerinde ilk kez kapatma adımları cihazın bir 8100 olup olmadığı veya 8600 model bağlı olarak farklılık gösterir. 8600 birincil muhafaza ve EBOD muhafazası çift muhafaza aygıtla iken 8100 tek bir birincil muhafazada sahiptir. Ayrıntılı adımlar her iki modeli için aşağıdaki bölümlerde ele alınmıştır.

* [Yalnızca birincil kasası ile yeni cihaz](#new-device-with-primary-enclosure-only)
* [Yeni cihaz EBOD muhafazası ile](#new-device-with-ebod-enclosure)

### <a name="new-device-with-primary-enclosure-only"></a>Yalnızca birincil kasası ile yeni cihaz
StorSimple 8100 model tek muhafaza aygıttır. Cihazınızı yedek güç ve soğutma modüllerini (PCMs) içerir. Hem PCMs yüklü ve yüksek kullanılabilirlik sağlamak için farklı güç kaynaklarına bağlandınız.

Cihazınızı güç kablosu için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

> [!NOTE]
> Tam cihaz kurulumu ve yönergeleri kablo için Git [StorSimple 8100 cihazınız yüklemek](storsimple-8100-hardware-installation.md). Tam olarak yönergeleri izlediğinizden emin olun.
> 
> 

### <a name="new-device-with-ebod-enclosure"></a>Yeni cihaz EBOD muhafazası ile
StorSimple 8600 model birincil muhafaza ve EBOD muhafazası sahiptir. Bu seri bağlı SCSI (SAS) bağlantısı ve güç için birlikte kablolu için birimler gerektirir.

Bu cihazı ilk kez ayarlarken, ilk SAS kabloları adımlarını gerçekleştirin ve güç kabloları adımlarını tamamlayın.

[!INCLUDE [storsimple-sas-cable-8600](../../includes/storsimple-sas-cable-8600.md)]

[!INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

> [!NOTE]
> Tam cihaz kurulumu ve yönergeleri kablo için Git [StorSimple 8600 model Cihazınızı yüklemek](storsimple-8600-hardware-installation.md). Tam olarak yönergeleri izlediğinizden emin olun.

## <a name="turn-on-a-device-after-shutdown"></a>Bir aygıtı kapatıldıktan sonra açın
Cihazın bir 8100 olup olmadığı veya 8600 model bağlı olarak bu kapatıldı sonra StorSimple cihazında kapatma adımları farklıdır. 8600 birincil muhafaza ve EBOD muhafazası çift muhafaza aygıtla iken 8100 tek bir birincil muhafazada sahiptir.

* [Yalnızca birincil muhafaza aygıtla](#device-with-primary-enclosure-only)
* [Cihaz EBOD muhafazası ile](#device-with-ebod-enclosure)

### <a name="device-with-primary-enclosure-only"></a>Yalnızca birincil muhafaza aygıtla
Bir kapanma sonra birincil muhafaza ve EBOD muhafazası bir StorSimple cihazına etkinleştirmek için aşağıdaki yordamı kullanın.

#### <a name="to-turn-on-a-device-with-a-primary-enclosure-only"></a>Yalnızca birincil kasası ile bir cihazda etkinleştirmek için
1. Her iki gücüyle güç geçer ve soğutma modülleri (PCMs) OFF konumunda olduğundan emin olun. Anahtarlar OFF konumda değilse, OFF konuma ters çevirin ve Git ışık bekleyin.
2. Her iki PCMs ON konumuna üzerinde güç anahtarları döndürerek aygıtı açın. Cihaz etkinleştirmeniz gerekir.
3. Cihaz tam olarak açık olduğunu doğrulamak için aşağıdakileri denetleyin:
   
   1. Her iki PCM modülleri üzerinde Tamam LED'leri yeşil.
   2. ' % S'durumu LED'leri hem denetleyicilerinde Düz yeşil hazırdır.
   3. Mavi LED denetleyicilerinden biri üzerinde yanıp, denetleyici etkin olduğunu gösterir.
      
      Bu koşulların herhangi biri karşılanmazsa, Cihazınızı sağlıklı değil. Lütfen [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md).

### <a name="device-with-ebod-enclosure"></a>Cihaz EBOD muhafazası ile
Bir kapanma sonra birincil muhafaza ve EBOD muhafazası bir StorSimple cihazına etkinleştirmek için aşağıdaki yordamı kullanın. Her adım sırayla açıklandığı gibi tam olarak gerçekleştirir. Bunun Sağlanamaması veri kaybına neden olabilir.

#### <a name="to-turn-on-a-device-with-a-primary-and-an-ebod-enclosure"></a>Birincil ve EBOD muhafazası aygıtla etkinleştirmek için
1. EBOD muhafazası birincil muhafaza bağlı olduğundan emin olun. Daha fazla bilgi için bkz: [StorSimple 8600 model Cihazınızı yüklemek](storsimple-8600-hardware-installation.md).
2. Güç ve soğutma modülleri (PCMs) hem EBOD hem de birincil kasaları OFF konumunda olduğundan emin olun. Anahtarlar OFF konumda değilse, OFF konuma ters çevirin ve Git ışık bekleyin.
3. EBOD muhafazası etkinleştirin ilk güç döndürerek üzerinde hem PCMs ON konuma geçer. PCM LED'leri yeşil olması gerekir. Bu birimi bir yeşil EBOD denetleyicisi LED EBOD muhafazası üzerinde olduğunu gösterir.
4. Her iki PCMs ON konumuna üzerinde güç anahtarları döndürerek birincil muhafaza açın. Tüm sistemin artık üzerinde olması gerekir.
5. Hangi EBOD Muhafazası ve birincil muhafaza arasındaki bağlantıyı iyi olmasını sağlar, SAS LED'leri yeşil olduğunu doğrulayın.

## <a name="turn-on-a-device-after-a-power-loss"></a>Bir aygıtı sonra güç kaybı açın
Bir güç kesintisi ya da kesinti bir StorSimple cihazı kapatmanız yeterlidir. Güç kesintisi güç kaynakları birini veya her iki güç kaynakları gerçekleşebilir. Kurtarma adımları, cihazın 8100 veya 8600 model olup bağlı olarak farklıdır. 8600 birincil muhafaza ve EBOD muhafazası çift muhafaza aygıtla iken 8100 tek bir birincil muhafazada sahiptir. Bu bölümde, her senaryo için kurtarma yordamı açıklanmaktadır.

* [Yalnızca birincil muhafaza aygıtla](#8100)
* [Cihaz EBOD muhafazası ile](#8600)

### <a name="device-with-primary-enclosure-only-a-name8100"></a>Yalnızca birincil muhafaza aygıtla<a name="8100">
Sistem, güç kaybı, güç kaynakları birine ise, normal işlem devam edebilirsiniz. Ancak, cihazın yüksek kullanılabilirlik sağlamak için güç güç kaynağı mümkün olan en kısa sürede geri yükleyin.

Sistem bir güç kesintisi veya her iki güç kaynakları üzerinde güç kesintisi ise düzenli ve denetimli bir şekilde kapanır. Güç sağlandığında sistem otomatik olarak aç.

### <a name="device-with-ebod-enclosure-a-name8600"></a>Cihaz EBOD muhafazası ile<a name="8600">
#### <a name="power-loss-on-one-power-supply"></a>Güç kaybı bir güç kaynağı
Sistem, güç kaybı, güç kaynakları birincil muhafaza veya EBOD muhafazası birine ise, normal işlem devam edebilirsiniz. Ancak, cihazın yüksek kullanılabilirlik sağlamak için lütfen power güç kaynağı mümkün olan en kısa sürede geri yükleyin.

#### <a name="power-loss-on-both-power-supplies-on-primary-and-ebod-enclosures"></a>Birincil ve EBOD kutularının hem güç kaynakları güç kaybı
Bir güç kesintisi veya her iki güç kaynakları üzerinde güç kesintisi ise EBOD muhafazası hemen kapanacak ve birincil muhafaza düzenli ve denetimli bir şekilde kapanacak. Güç geri yüklendiğinde Gereci otomatik olarak başlatılacak.

Güç el ile kapalı, güç sisteme geri yüklemek için aşağıdaki adımları gerçekleştirin.

1. EBOD muhafazası açın.
2. EBOD muhafazası açıktır sonra birincil muhafaza etkinleştirin.

### <a name="power-loss-on-both-power-supplies-on-ebod-enclosure"></a>Her iki güç kaynakları EBOD muhafazası üzerinde güç kaybı
, Kablolar ayarladığınızda, EBOD hiçbir zaman tek başına için ayrı bir PDU bağlı olduğundan emin olmalısınız. EBOD ve birincil muhafaza aynı anda başarısız olursa, sistem kurtarır.

Yalnızca her iki güç kaynakları EBOD muhafazası başarısız sistem otomatik olarak kurtarılacak değil. Sistemde açın ve sağlıklı bir duruma geri yüklemek için aşağıdaki adımları gerçekleştirin:

1. Birincil muhafaza açıksa, güç ve soğutma modülleri (PCMs) kapalı geçin.
2. Kapatmak sistem için bir kaç dakika bekleyin.
3. EBOD muhafazası açın.
4. EBOD muhafazası açıktır sonra birincil muhafaza etkinleştirin.

## <a name="turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost"></a>Bir aygıtı açın sonra birincil ve EBOD muhafazası bağlantısı kesildiği
Bekleme denetleyici ve karşılık gelen EBOD denetleyicisi arasında bağlantı kaybolursa, cihaz çalışmaya devam eder. Sistem etkin denetleyicisi ve karşılık gelen EBOD denetleyicisi arasındaki bağlantıyı kaybolursa, yük devretme gerçekleşeceğini ve aygıtın normal olarak çalışmaya devam etmelidir.

EBOD Muhafazası ve birincil muhafaza arasındaki bağlantıyı zarar görmesi veya hem seri bağlı SCSI (SAS) kabloları kaldırılır, cihaz çalışmayı durdurur. Bu noktada, aşağıdaki adımları gerçekleştirin.

### <a name="to-turn-on-the-device-after-connection-is-lost"></a>Bağlantı kaybedildikten sonra cihazda etkinleştirmek için
1. Cihaz geri erişin.
2. EBOD muhafazası birincil muhafaza arasında SAS kablosu bağlantısı kesildiğinde, tüm SAS lane LED'leri EBOD muhafazası üzerinde devre dışı olacaktır.
3. Güç ve soğutma modülleri (PCMs) aşağı EBOD Muhafazası ve birincil muhafaza kapatın.
4. Her iki kasaları arkasındaki tüm ışık kapatmak kadar bekleyin.
5. SAS kabloları takın ve EBOD Muhafazası ve birincil muhafaza arasında iyi bir bağlantı olduğundan emin olun.
6. EBOD muhafazası etkinleştirin ilk iki PCM döndürerek ON konuma geçer.
7. EBOD muhafazası üzerinde yeşil ışığı açık olduğunu denetleyerek olduğundan emin olun.
8. Birincil muhafaza açın.
9. Birincil kasası üzerinde denetleyicisi yeşil ışığı açık olduğunu denetleyerek olduğundan emin olun.
10. SAS lane LED'leri (dört EBOD denetleyici başına) tüm açık olan denetleyerek birincil muhafaza EBOD muhafazası bağlantıyla iyi olduğundan emin olun.

> [!IMPORTANT]
> SAS kabloları bozuk veya sistemde etkinleştirdiğinizde EBOD Muhafazası ve birincil muhafaza arasındaki bağlantıyı iyi olmayan olduğundan, kurtarma moduna geçer. Lütfen [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) böyle bir durumda.


## <a name="turn-off-a-running-device"></a>Çalışan bir aygıtı kapatın
Çalışan StorSimple cihazını hizmet dışı gerçekleştirilecek taşındığı veya değiştirilmesi gereken düzgün çalışmayan bir bileşen olup olmadığını kapatılması gerekir. Adımları StorSimple cihazı bir 8100 olup veya 8600 model bağlı olarak farklılık gösterir. 8600 birincil muhafaza ve EBOD muhafazası çift muhafaza aygıtla iken 8100 tek bir birincil muhafazada sahiptir. Bu bölümde bir çalışan cihazı kapatmak için adımlarını ayrıntılı şekilde açıklar.

* [Birincil muhafaza aygıtla](#8100a)
* [Cihaz EBOD muhafazası ile](#8600a)

### <a name="device-with-primary-enclosure-a-name8100a"></a>Birincil muhafaza aygıtla<a name="8100a">
Sıralı ve denetimli bir şekilde cihazı kapatmak için Klasik Azure portalı veya Windows PowerShell aracılığıyla StorSimple için bunu yapabilirsiniz. 

> [!IMPORTANT]
> Bir çalışan aygıtı aygıtın arkasında güç düğmesini kullanarak kapatmayı değil.
> 
> Cihazı kapatmadan önce tüm aygıt bileşenlerinin sağlıklı olduğundan emin olun. Klasik Azure portalında gidin **aygıtları** > **Bakım** > **donanım durum**ve tüm bileşenlerin durumunun yeşil olduğunu doğrulayın. Bu, yalnızca sağlıklı bir sistem için geçerlidir. Sistem aşağı kapatılıyor düzgün çalışmayan bir bileşeni Değiştir, başarısız (kırmızı) görür veya ilgili bileşeni (sarı) durumunun düşürülmüş **donanım durum**.
> 
> 

StorSimple veya Klasik Azure portalı için Windows PowerShell eriştikten sonra adımları [bir StorSimple cihazı kapatmanız](storsimple-manage-device-controller.md#shut-down-a-storsimple-device). 

### <a name="device-with-ebod-enclosure-a-name8600a"></a>Cihaz EBOD muhafazası ile<a name="8600a">
> [!IMPORTANT]
> Birincil muhafaza ve EBOD muhafazası aşağı kapatmadan önce tüm aygıt bileşenlerinin sağlıklı olduğundan emin olun. Azure portalında gidin **aygıtları** > **İzleyici** > **donanım durumu**ve tüm bileşenlerini sağlıklı olduğunu doğrulayın.


#### <a name="to-shut-down-a-running-device-with-ebod-enclosure"></a>EBOD muhafazası çalışan bir aygıtla kapatmak için
1. Listelenen tüm adımları [bir StorSimple cihazı kapatmanız](storsimple-8000-manage-device-controller.md#shut-down-a-storsimple-device) birincil kasası için.
2. Birincil muhafaza kapattığınızda, güç ve soğutma Modülü (PCM) anahtarları döndürerek EBOD kapatın.
3. EBOD kapatıldı doğrulamak için tüm ışık EBOD muhafazası arkasında kapalı olduğunu denetleyin.

> [!NOTE]
> Sistem kapatılır sonra birincil muhafaza EBOD muhafazası bağlanmak için kullanılan SAS kabloları kadar kaldırılmaması gerekir.

## <a name="next-steps"></a>Sonraki adımlar
[Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) açma veya bir StorSimple cihazı kapatmak sorunlarla karşılaşırsanız.

