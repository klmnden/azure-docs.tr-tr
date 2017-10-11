---
title: "StorSimple cihazı disk sürücüsünde Değiştir | Microsoft Docs"
description: "StorSimple birincil muhafaza veya EBOD muhafazası disk sürücüsünde Değiştir açıklanmaktadır."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 98890d36-b613-40fd-994e-330dd907a8a1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 0659ab9d304dbfcce72e8c3c79edad68e70b9630
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="replace-a-disk-drive-on-your-storsimple-device"></a>StorSimple Cihazınızı disk sürücüsünde değiştirin
## <a name="overview"></a>Genel Bakış
Bu öğretici nasıl kaldırın ve bir hatalı çalışan veya başarısız sabit disk sürücüsünde bir Microsoft Azure StorSimple cihaz Değiştir açıklanmaktadır. Bir disk sürücüsü değiştirmek için aktarmanız gerekir:

* Antitamper kilit disengage
* Disk sürücüsünü Kaldır
* Değiştirme disk sürücüsü yükleyin

> [!IMPORTANT]
> Bir disk sürücüsü değiştirme ve kaldırma gözden önce güvenlik bilgileri [StorSimple donanım bileşeni değiştirme](storsimple-hardware-component-replacement.md).
> 
> 

## <a name="disengage-the-antitamper-lock"></a>Antitamper kilit disengage
Bu yordam, nasıl, StorSimple Cihazınızda antitamper kilitler disk sürücülerini değiştirdiğinizde boşta veya açıklar. Antitamper kilitler sürücü taşıyıcı tanıtıcılarını donatılmıştır ve küçük açıklığı tanıtıcının Mandal bölümdeki aracılığıyla erişilir. Sürücüleri kilitli konumu ayarlayın kilitleri ile sağlanır.

#### <a name="to-unlock-the-antitamper-lock"></a>Antitamper kilitleme kilidini açmak için
1. Dikkatle tanıtıcı açıklığı ve onun yuva lock tuşunun (Microsoft sağlanan bir "tamperproof" T10 tornavida) ekleyin. 
   
   > [!NOTE]
   > Antitamper kilidi etkinleştirilmişse, kırmızı gösterge açıklığı görünür olur.
   > 
   > 
   
    ![Kilitli disk sürücüsü](./media/storsimple-disk-drive-replacement/IC741056.png)
   
    **Şekil 1** koruma yetkisiz değiştirmeye karşı kilit gerçekleştiriliyor
   
   | Etiket | Açıklama |
   |:--- |:--- |
   | 1 |Gösterge açıklığı |
   | 2 |Antitamper Kilitle |
2. Kırmızı gösterge açıklığı anahtarı yukarıda görünür değil kadar anticlockwise yönü anahtarında döndürün.
3. Anahtarı kaldırın.
   
    ![Kilidi açılmış disk sürücüsü](./media/storsimple-disk-drive-replacement/IC741057.png)
   
    **Şekil 2** kilitli değil disk sürücüsü
4. Disk sürücüsü artık kaldırılabilir.

Kilidi bulunmaya ters'ndaki adımları izleyin.

## <a name="remove-the-disk-drive"></a>Disk sürücüsünü Kaldır
StorSimple Cihazınızı bir RAID 10 benzeri depolama alanları yapılandırmasını destekler. Bu, başarısız olan bir diski, katı hal sürücüsü (SSD) normal olarak çalışabilir veya sabit disk sürücüsü (HDD) anlamına gelir. 

> [!IMPORTANT]
> * Sisteminizde birden fazla başarısız disk varsa, birden fazla SSD veya HDD herhangi bir noktada sisteminden zamanında kaldırmayın. Bunun yapılması, veri kaybına neden olabilir.
> * Daha önce bir SSD bulunan bir yuvada SSD yerine koyun emin olun. Benzer şekilde, daha önce bir HDD bulunan bir yuvada değiştirme HDD yerleştirin.
> * Klasik Azure portalında yuva 0'dan – numaralandırılır 11. Portal bir disk 2 yuvadaki, cihazda başarısız olduğunu gösterirse bu nedenle, üçüncü yuvasındaki başarısız disk sol üstten arayın.
> 
> 

Sürücüleri kaldırılır ve işletim sistemi sırasında değiştirilir.

#### <a name="to-remove-a-drive"></a>Bir sürücüyü kaldırmak için
1. Azure Klasik portalında başarısız diski tanımlamak için Git **aygıtları** > **Bakım** > **donanım durum**. Bir disk birincil muhafazada ve/veya bir EBOD muhafazası (8600 model kullanıyorsanız) içinde başarısız olabileceğinden altında disklerin durumunu bakmak **paylaşılan bileşenleri** ve altında **EBOD muhafazası paylaşılan bileşenleri**. Başarısız disk ya da muhafazada kırmızı bir duruma sahip gösterilir.
2. Birincil muhafaza veya EBOD muhafazası önündeki sürücüleri bulun. 
3. Kilidi diskse, sonraki adıma geçebilirsiniz. Disk kilitliyse, aşağıdaki yordamı kullanarak kilidini [antitamper kilit Disengage](#disengage-the-antitamper-lock).
4. Sürücü taşıyıcı modülünü siyah Mandal basın ve sürücü taşıyıcı tanıtıcı çekerek out ve kasa Önden çıkartın. 
   
    ![Disk sürücüsü işleyici bırakılıyor](./media/storsimple-disk-drive-replacement/IC741051.png)
   
    **Şekil 3** sürücüsü işleyici bırakılıyor
5. Sürücü taşıyıcı tanıtıcı tam olarak genişletildiğinde gövdeden sürücü taşıyıcı kaydırın. 
   
    ![Disk sürücüsünden kayan disk](./media/storsimple-disk-drive-replacement/IC741052.png)
   
    **Şekil 4** taşıyıcı dışında disk sürücüsü kayan

## <a name="install-the-replacement-disk-drive"></a>Değiştirme disk sürücüsü yükleyin
Bir sürücü StorSimple Cihazınızı başarısız oldu ve kaldırıldı sonra yeni bir sürücüyle değiştirmek için bu yordamı izleyin.

#### <a name="to-insert-a-drive"></a>Bir sürücü eklemek için
1. Sürücü taşıyıcı tanıtıcı tam olarak genişletilmiş, aşağıdaki görüntüde gösterildiği gibi emin olun.
   
    ![İşleyicisi genişletilmiş disk sürücüsü](./media/storsimple-disk-drive-replacement/IC741044.png)
   
    **Şekil 5** işleyicisi genişletilmiş sürücü
2. Sürücü taşıyıcı tüm kasaya kaydırın. 
   
    ![Disk sürücüsü taşıyıcı kayan diskine](./media/storsimple-disk-drive-replacement/IC741045.png)
   
    **Şekil 6** sürücü taşıyıcı kaydırarak gövdeye yerleştirme
3. Sürücü taşıyıcı eklenen sürücü taşıyıcı tanıtıcı kilitli bir konumda tutturur kadar sürücü taşıyıcı kasaya göndermek devam ederken sürücü taşıyıcı tanıtıcı kapatın.
4. Taşıyıcı tanıtıcı güvenli hale getirmek için Microsoft tarafından (tamperproof Torx tornavida) sağlanan lock tuşunun kilit Vidayı Çeyrek Aç yönünde kapatarak yerine kullanın.
5. Değiştirme başarılı oldu ve sürücü, Klasik Azure portalına erişme ve giderek tarafından çalışır durumda olduğunu doğrulayın **Bakım** > **donanım durum**. Altında **paylaşılan bileşenleri** veya **EBOD muhafazası paylaşılan bileşenleri**, sürücü durumu sağlıklı olduğunu gösteren yeşil olması gerekir.
   
   > [!NOTE]
   > Disk durumunun değiştirme işleminden yeşile birkaç saat sürebilir.
   > 
   > 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [StorSimple donanım bileşeni değiştirme](storsimple-hardware-component-replacement.md).

