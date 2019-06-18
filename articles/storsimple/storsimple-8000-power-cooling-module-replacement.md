---
title: StorSimple 8000 serisi Cihazınızda bir PCM'yi değiştirme | Microsoft Docs
description: Kaldırın ve güç ve soğutma Modülü (PCM), StorSimple Cihazınızda değiştirin açıklanmaktadır
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
ms.openlocfilehash: 42561570e24aec5edd33248ef1738e53175e480e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60632506"
---
# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a>Güç ve soğutma modülü, StorSimple Cihazınızda değiştirin
## <a name="overview"></a>Genel Bakış
Güç ve soğutma Modülü (PCM) Microsoft Azure StorSimple Cihazınızı bir güç ve soğutma fanları birincil ve ebod denetlenen oluşur. Yalnızca bir modelin her kasa için sertifikalı PCM yoktur. Birincil muhafaza bir 764 W PCM için sertifikalıdır ve EBOD muhafazası bir 580 W PCM için sertifikalıdır. Değiştirme yordam muhafaza birincil ve EBOD muhafazası PCMs farklı olsa da, aynıdır.

Bu öğreticide, aşağıdaki işlemlerin nasıl yapılacağı açıklanmaktadır:

* Bir PCM Kaldır
* PCM'yi değiştirme yükleyin

> [!IMPORTANT]
> Kaldırma ve bir PCM'yi değiştirme gözden önce güvenlik bilgileri [StorSimple donanım bileşeni değişimi](storsimple-8000-hardware-component-replacement.md).


## <a name="before-you-replace-a-pcm"></a>Önce bir PCM'yi değiştirme
PCM'yi değiştirme önce aşağıdaki önemli sorunları dikkat edin:

* Güç kaynağı PCM biri başarısız olursa, hatalı Modülü yüklü bırakın, ancak güç kablosunu kaldırın. Fan power kutusu almak ve doğru soğutma sağlamaya devam devam eder. Fan başarısız olursa PCM hemen değiştirilmesi gerekiyor.
* PCM kaldırmadan önce güç PCM ana anahtarı (mevcut olduğu) kapatarak veya fiziksel olarak güç kablosunu kaldırarak bağlantısını kesin. Bu, bir uyarı sistem bir güç kapatma olup sağlar.
* Diğer PCM hatalı bir PCM'yi değiştirme önce devam eden bir sistem işlemi için işlevsel olduğundan emin olun. Hatalı bir PCM'yi tarafından tam olarak işlevsel bir PCM olabildiğince çabuk değiştirilmelidir.
* PCM modülü değiştirme tamamlanması yalnızca birkaç dakika sürer, ancak elektriği engellemek için PCM'yi kaldırmanın 10 dakika içinde tamamlanmalıdır.
* Yedek pil modülü içermemesi fabrikasından sevk değiştirme 764 W PCM modülleri unutmayın. Pil, hatalı bir PCM'yi kaldırın ve değişiklik gerçekleştirmeden önce değiştirme Modül Ekle gerekecektir. Daha fazla bilgi için bkz. nasıl [kaldırın ve bir yedek pil Modül Ekle](storsimple-8000-battery-replacement.md).

## <a name="remove-a-pcm"></a>Bir PCM Kaldır
Microsoft Azure StorSimple cihazınızın güç ve soğutma Modülü (PCM) kaldırmak hazır olduğunuzda aşağıdaki yönergeleri izleyin.

> [!NOTE]
> PCM kaldırmadan önce doğru değiştirme (764 W birincil kasa için) veya 580 W EBOD muhafazası için sahip olduğunuzu doğrulayın.

#### <a name="to-remove-a-pcm"></a>Bir PCM kaldırmak için
1. Azure Klasik portalında **Ayarları > İzleme > donanım sistem durumu**. Altındaki PCM bileşenlerinin durumunu **paylaşılan bileşenleri** PCM başarısız oldu tanımlamak için:
   
   * Bir güç kaynağı PCM 0'da başarısız oldu, durumunu **güç kaynağı PCM 0'da** kırmızı olur.
   * Bir güç kaynağı PCM 1'de başarısız oldu, durumunu **güç kaynağı PCM 1'deki** kırmızı olur.
   * PCM 1'deki fan başarısız oldu, ya da durumunu **0 için PCM 0'da Soğuyor** veya **PCM 0'da Soğuyor 1** kırmızı olur.
2. PCM'yi birincil muhafaza arkasında bulun. 8600 model çalıştırıyorsanız, Sistem Birim kimlik ön panelini LED görünen numarasına bakarak birincil muhafaza belirleyin. Birim birincil muhafaza görüntülenen kodu varsayılandır **00**varsayılan birim üzerinde EBOD muhafazası görüntülenen kimlik bilgileriyse **01**. Aşağıdaki tablo ve diyagram LED görüntü ön panelini açıklanmaktadır.
   
    ![Ön OPS panelindeki sistem kimliği](./media/storsimple-power-cooling-module-replacement/IC740991.png)
   
     **Şekil 1** cihazın ön panel  
   
   | Etiket | Açıklama |
   |:--- |:--- |
   | 1 |Sesi kapa düğmesi |
   | 2 |Sistem güç |
   | 3 |Modül hatası |
   | 4 |Mantıksal hata |
   | 5 |Birim kimliği görüntüleme |
3. İzleme gösterge LED'lerini birincil muhafaza arkasında, hatalı bir PCM'yi tanımlamak için de kullanılabilir. Aşağıdaki diyagram ve LED'lerini hatalı bir PCM'yi bulmak için nasıl kullanılacağını anlamak için tablosuna bakın. Örneğin, ışığı karşılık gelen **fanı başarısız** olan aydınlatma, fan başarısız oldu. Benzer şekilde, ışığı karşılık gelen **AC başarısız** olan aydınlatma, güç kaynağı başarısız oldu. 
   
    ![Cihaz PCM izleme gösterge LED'lerini devre kartı](./media/storsimple-power-cooling-module-replacement/IC740992.png)
   
     **Şekil 2** arka of PCM gösterge LED'lerini ile
   
   | Etiket | Açıklama |
   |:--- |:--- |
   | 1 |AC güç kesintisi |
   | 2 |Fanı hatalı |
   | 3 |Pil hata |
   | 4 |PCM TAMAM |
   | 5 |DC Güç kesintisi |
   | 6 |Pil Sağlıklı |
4. Başarısız PCM modülü bulmak için StorSimple cihazı arkasına aşağıdaki diyagrama bakın. PCM 0'da sol taraftaki ve PCM 1 sağ tarafta. Aşağıdaki tablo, modülleri açıklar.
   
     ![Cihaz birincil kutusu modüllerinin devre kartı](./media/storsimple-power-cooling-module-replacement/IC740994.png)
   
     **Şekil 3** eklenti modülleri ile cihaz arkasına 
   
   | Etiket | Açıklama |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Denetleyici 0 |
   | 4 |Denetleyici 1 |
5. Hatalı bir PCM'yi açın ve güç kaynağı kablosunun bağlantısını kesin. PCM şimdi kaldırabilirsiniz.
6. Mandal ve PCM tanıtıcısı thumb arasındaki erişebildiğinizden kenarını kavrayın ve birlikte tanıtıcı açacak biçimde sığdırması.
   
    ![PCM tanıtıcısı açılıyor](./media/storsimple-power-cooling-module-replacement/IC740995.png)
   
    **Şekil 4** PCM tanıtıcısı açma
7. Tanıtıcı kavramayın ve PCM kaldırın.
   
    ![Cihaz PCM'i kaldırılıyor](./media/storsimple-power-cooling-module-replacement/IC740996.png)
   
    **Şekil 5** PCM'i kaldırılıyor

## <a name="install-a-replacement-pcm"></a>PCM'yi değiştirme yükleyin
StorSimple Cihazınızı bir PCM yüklemek için bu yönergeleri izleyin. Yedek pili Modülü (764 W PCMs için yalnızca geçerlidir) PCM'yi değiştirme yüklemeden önce eklediğiniz emin olun. Daha fazla bilgi için bkz. nasıl [kaldırın ve bir yedek pil Modül Ekle](storsimple-8000-battery-replacement.md).

#### <a name="to-install-a-pcm"></a>Bir PCM yüklemek için
1. Bu kasa için doğru değiştirme PCM sahip olduğunuzu doğrulayın. Birincil muhafaza bir 764 W PCM gerekir ve bir 580 W PCM EBOD muhafazası gerekiyor. EBOD muhafazası 764 W PCM kullanıp birincil kasası 580 W PCM denememeniz gerekir. Aşağıdaki görüntüde bu bilgileri için PCM yapıştırılmış etiketine tanımlamak nereye gösterir.
   
    ![Cihaz PCM etiketi](./media/storsimple-power-cooling-module-replacement/IC740973.png)
   
    **Şekil 6** PCM etiketi
2. Bağlayıcılar özellikle dikkat ederek ödeme muhafaza hasar olup olmadığını denetleyin. 
   
   > [!NOTE]
   > **Herhangi bir bağlayıcıyı PIN'ler Eğilmiş modülü yüklemeyin.**
   > 
   > 
3. Açık konuma PCM tanıtıcısı ile modülü kutu kaydırın.
   
    ![Cihaz PCM'i yükleniyor](./media/storsimple-power-cooling-module-replacement/IC740975.png)
   
    **Şekil 7** PCM'i yükleniyor
4. PCM tanıtıcısı el ile kapatın. Tanıtıcı Mandal ilgilenir gibi bir tıklama sesi.
   
   > [!NOTE]
   > Bağlayıcı PIN'ler bağlı olmak için yavaşça mandalı bırakılıyor olmadan tutamacı tug. Çıkış PCM slaytlar, tutma bağlayıcıların bağlı önce kapatıldı anlamına gelir.
   
5. Güç kaynağı ve PCM power kabloları bağlayın.
6. Basınç Tahliye bales güvenli hale getirin.
7. PCM üzerinde açın.
8. Değiştirme başarılı olup olmadığını doğrulayın: StorSimple cihaz Yöneticisi hizmetinizin Azure portalında, cihazınıza gidin ve ardından **Ayarları > İzleme > donanım sistem durumu**. Altında **paylaşılan bileşenleri**, PCM durumu yeşil olmalıdır.
   
   > [!NOTE]
   > Bu değişiklik tamamen başlatmak için PCM birkaç dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [StorSimple donanım bileşeni değişimi](storsimple-8000-hardware-component-replacement.md).

