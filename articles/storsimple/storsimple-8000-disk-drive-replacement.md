---
title: StorSimple 8000 serisi cihaz üzerinde bir disk sürücüsünü değiştirme | Microsoft Docs
description: Bir StorSimple birincil kutu veya EBOD muhafazası disk sürücüsünde nasıl değiştirileceğini açıklar.
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
ms.date: 8/25/2017
ms.author: alkohli
ms.openlocfilehash: 3d6ef22e4df36996d68194589f43ea0f57def22c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60576960"
---
# <a name="replace-a-disk-drive-on-your-storsimple-8000-series-device"></a>StorSimple 8000 serisi Cihazınızı disk sürücüsünde değiştirin

## <a name="overview"></a>Genel Bakış
Bu öğreticide, kaldırın ve Microsoft Azure StorSimple cihazında yapıyor ya da başarısız sabit disk sürücüsünü değiştirme nasıl açıklar. Disk sürücüsünü değiştirme için için gerekir:

* Antitamper kilit disengage
* Disk sürücüsünü Kaldır
* Yeni disk sürücüsü yükleme

> [!IMPORTANT]
> Bir disk sürücüsünü değiştirme ve kaldırma gözden önce güvenlik bilgileri [StorSimple donanım bileşeni değişimi](storsimple-8000-hardware-component-replacement.md).
 

## <a name="disengage-the-antitamper-lock"></a>Antitamper kilit disengage
Bu yordam, nasıl antitamper kilitler StorSimple cihazınız disk sürücülerini değiştirdiğinizde boşta veya açıklar. Sürücü taşıyıcı işleyicileri antitamper kilitler donatılmıştır ve tanıtıcının Mandal bölümündeki küçük bir açıklık aracılığıyla erişilir. Sürücüleri, kilitli konuma kilit ile birlikte verilir.

#### <a name="to-unlock-the-antitamper-lock"></a>Antitamper kilit kilidini açmak için
1. Dikkatli bir şekilde kilit (Microsoft gelen bir "tamperproof" T10 tornavida) tanıtıcı, açıklık ve kendi yuva Ekle anahtarı. 
   
   Antitamper kilidi etkinleştirilmişse, kırmızı göstergesi açıklık içinde görülebilir.
  
    ![Kilitli disk sürücüsü](./media/storsimple-disk-drive-replacement/IC741056.png)
   
    **Şekil 1** koruma kurcalamaya kilit gerçekleştiriliyor
   
   | Etiket | Açıklama |
   |:--- |:--- |
   | 1 |Gösterge açıklık |
   | 2 |Antitamper Kilitle |
2. Kırmızı göstergesi açıklığı anahtar yukarıda görünür değil kadar anticlockwise bir yönde anahtarı döndür.
3. Anahtarı kaldırın.
   
    ![Kilidi açılmış disk sürücüsü](./media/storsimple-disk-drive-replacement/IC741057.png)
   
    **Şekil 2** kilitli değil disk sürücüsü
4. Disk sürücüsünü artık kaldırılabilir.

Kilit etkileşim kurmak için ters adımları izleyin.

## <a name="remove-the-disk-drive"></a>Disk sürücüsünü Kaldır
StorSimple cihazınız için bir RAID 10 benzeri depolama alanları yapılandırmasını destekler. Bu, başarısız olan bir diski, katı hal sürücüsü (SSD), normal olarak çalışabilir veya sabit disk sürücüsü (HDD) anlamına gelir.

> [!IMPORTANT]
> * Sisteminizde birden fazla başarısız disk varsa, birden fazla SSD veya HDD herhangi bir noktada sisteminden sürede kaldırmayın. Bunun yapılması, veri kaybına neden olabilir.
> * Daha önce bir SSD bulunan bir yuva değiştirme SSD eklediğinden emin olun. Benzer şekilde, daha önce bir HDD yer alan bir yuvaya HDD değiştirme yerleştirin.
> * Azure portalında yuva 0'dan – numaralandırılır 11. Portal bir disk yuvasındaki 2, cihaz üzerinde başarısız olduğunu gösterir. Bu nedenle, üçüncü yuvasındaki hatalı disk sol üstten bakın.
> 
> 

Sürücüleri, kaldırılır ve sistem çalışırken değiştirilir.

#### <a name="to-remove-a-drive"></a>Bir sürücü kaldırmak için
1. Azure portalında, başarısız diski tanımlamak için cihazınıza gidin **Ayarları > donanım sistem durumu**. Bir disk birincil muhafazada ve/veya bir EBOD muhafazası (8600 model kullanıyorsanız) içinde başarısız olabileceğinden, altında disklerin durumunu bakmak **paylaşılan bileşenleri** altında **EBOD paylaşılan bileşenler**. Hatalı bir disk ya da muhafazada kırmızı bir duruma sahip gösterilir.
2. Birincil kasası veya EBOD muhafazası kuyruğun sürücüleri bulun. 
3. Kilidi açılmış disk ise, sonraki adıma geçin. Disk kilitliyse yordamı izleyerek kilidini [antitamper kilit Disengage](#disengage-the-antitamper-lock).
4. Siyah Mandal sürücü taşıyıcı modüldeki tuşuna basın ve sürücüsü taşıyıcı işleyici çekerek out ve kasa önünden çıkartın.
   
    ![Disk sürücüsü işleyici bırakılıyor](./media/storsimple-disk-drive-replacement/IC741051.png)
   
    **Şekil 3** sürücüsü işleyici bırakılıyor
5. Sürücü taşıyıcı tanıtıcı tamamen genişletildiğinde kaydırarak disk taşıyıcı kaydırın. 
   
    ![Diski kaydırarak disk sürücüsünden kayan](./media/storsimple-disk-drive-replacement/IC741052.png)
   
    **Şekil 4** disk sürücüsü taşıyıcı dışında kayan

## <a name="install-the-replacement-disk-drive"></a>Yeni disk sürücüsü yükleme
Bir sürücü, StorSimple Cihazınızı başarısız oldu ve, kaldırdıktan sonra yeni bir sürücü ile değiştirmek için bu yordamı izleyin.

#### <a name="to-insert-a-drive"></a>Bir sürücü eklemek için
1. Sürücü taşıyıcı tanıtıcı tam olarak genişletilmiş, aşağıdaki görüntüde gösterildiği gibi emin olun.
   
    ![İşleyicisi genişletilmiş disk sürücüsü](./media/storsimple-disk-drive-replacement/IC741044.png)
   
    **Şekil 5** işleyicisi genişletilmiş sürücü
2. Sürücü taşıyıcı tüm kasaya kaydırın.
   
    ![Disk sürücüsü taşıyıcı kayan diskine](./media/storsimple-disk-drive-replacement/IC741045.png)
   
    **Şekil 6** sürücü taşıyıcı gövdeye yerleştirme
3. Sürücü taşıyıcı eklenen, kilitli bir öğeye sürücüsü taşıyıcı işleyici yaslanıp kadar disk taşıyıcı kasaya göndermeye devam ederken sürücüsü taşıyıcı işleyici kapatın.
4. Taşıyıcı tanıtıcı güvenliğini sağlamak için Microsoft tarafından (tamperproof Torx tornavida) sağlanan kilit anahtarı kilit Sarmal bir çeyrek daire saat yönünde açarak yerine kullanın.
5. Değişiklik başarılı oldu ve sürücü çalışır durumda olduğunu doğrulayın. Azure portalına erişmek ve gidin **cihaz ayarları** > **donanım sistem durumu**. Altında **paylaşılan bileşenleri** veya **EBOD paylaşılan bileşenler**, sürücü durumu sağlıklı olduğunu belirten yeşil olmalıdır.

   
   > [!NOTE]
   > Bu disk durumunun değiştirme işleminden yeşile birkaç saat sürebilir.
  
## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [StorSimple donanım bileşeni değişimi](storsimple-8000-hardware-component-replacement.md).

