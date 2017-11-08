---
title: "StorSimple Cihazınızda PCM Değiştir | Microsoft Docs"
description: "Kaldırdığınızda ve değiştirdiğinizde güç ve soğutma Modülü (PCM), StorSimple Cihazınızda açıklanmaktadır"
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 24a158cb-0b79-4908-bb5a-431e48760f6a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: 7031aff9d4797e99e6523a65ded7495c88aff282
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a>StorSimple Cihazınızda güç ve soğutma modülü değiştirin
> [!NOTE]
> StorSimple için Klasik portalı kullanım dışıdır. StorSimple cihaz yöneticileri yeni Azure portalına kullanımdan zamanlamaya göre otomatik olarak taşır. Bir e-posta ve bu taşıma için portal bir bildirim alırsınız. Bu belgede ayrıca yakında kullanımdan kaldırılacaktır. Bu makalede yeni Azure portalına için sürümünü görüntülemek için şu adrese gidin [güç ve soğutma modülü StorSimple Cihazınızda Değiştir](storsimple-8000-power-cooling-module-replacement.md). Taşıma hakkında herhangi bir sorunuz için bkz: [SSS: Azure portalına taşıma](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Genel Bakış
Güç ve soğutma Modülü (PCM) Microsoft Azure StorSimple Cihazınızı içinde oluşan bir güç ve soğutma fanları birincil ve EBOD kutularının denetlenir. Her kasa için sertifikalı PCM, yalnızca bir model yok. Birincil muhafaza 764 W PCM için sertifikalı ve EBOD muhafazası 580 W PCM için sertifikalıdır. Birincil muhafaza ve EBOD muhafazası PCMs farklı olmasına rağmen değiştirme yordamı aynıdır.

Bu öğretici açıklar nasıl yapılır:

* Bir PCM Kaldır
* PCM yenisini yükleyin

> [!IMPORTANT]
> Kaldırma ve bir PCM değiştirme gözden önce güvenlik bilgileri [StorSimple donanım bileşeni değiştirme](storsimple-hardware-component-replacement.md).
> 
> 

## <a name="before-you-replace-a-pcm"></a>Bir PCM değiştirmeden önce
PCM değiştirmeden önce aşağıdaki önemli sorunları dikkat edin:

* Güç kaynağı PCM biri başarısız olursa, hatalı modül yüklü bırakın, ancak güç kablosu kaldırın. Fan muhafaza güç almak ve uygun soğutma sağlamaya devam devam eder. Fan başarısız olursa, PCM hemen değiştirilmesi gerekiyor.
* PCM kaldırmadan önce güç PCM'den ana anahtarı (var olduğunda) kapatarak veya fiziksel olarak güç kablosu kaldırarak bağlantısını kesin. Bu, bir uyarı sistem bir güç kapatma olup sağlar.
* Diğer PCM hatalı PCM değiştirme önce devam eden sistem işlemi için işlevsel olduğundan emin olun. Hatalı PCM tarafından tam olarak işlevsel bir PCM mümkün olan en kısa sürede değiştirilmelidir.
* PCM modülü değiştirme tamamlamak için yalnızca birkaç dakika sürer ancak aşırı önlemek için başarısız PCM kaldırmanın 10 dakika içinde tamamlanması gerekir.
* Not fabrikası'ndan sevk değiştirme 764 W PCM modülleri yedek pil modülü içermez. Hatalı PCM'den pil kaldırın ve değiştirme gerçekleştirmeden önce değiştirme Modülü Ekle gerekecektir. Daha fazla bilgi için bkz: nasıl yapılır [kaldırın ve bir yedek pil Modül Ekle](storsimple-battery-replacement.md).

## <a name="remove-a-pcm"></a>Bir PCM Kaldır
Microsoft Azure StorSimple cihazınızın güç ve soğutma Modülü (PCM) kaldırmak hazır olduğunuzda bu yönergeleri izleyin.

> [!NOTE]
> PCM kaldırmadan önce doğru değiştirme (764 W birincil kasası için) veya 580 W EBOD muhafazası için sahip olduğunuzu doğrulayın.
> 
> 

#### <a name="to-remove-a-pcm"></a>Bir PCM kaldırmak için
1. Klasik Azure portalında tıklatın **aygıtları** > **Bakım** > **donanım durum**. Altındaki PCM bileşenlerinin durumunu denetleme **paylaşılan bileşenleri** hangi PCM başarısız oldu tanımlamak için:
   
   * Güç kaynağı PCM 0'ın başarısız olduysa, durumunu **güç kaynağı PCM 0'ın** kırmızı olur.
   * Güç kaynağı PCM 1 başarısız olduysa, durumunu **güç kaynağı PCM 1** kırmızı olur.
   * Fan PCM 1 başarısız oldu, ya da durumunu **PCM 0 0 soğutma** veya **PCM 0 için 1 soğutma** kırmızı olur.
2. Birincil muhafaza arkasında başarısız PCM bulun. 8600 model çalıştırıyorsanız, sistem birimi tanımlayıcısı ön panelini LED Ekran numarasına bakarak birincil muhafaza tanımlayın. Birim üzerinde birincil muhafaza görüntülenen kodu varsayılandır **00**, varsayılan birim üzerinde EBOD muhafazası görüntülenen kodu iken **01**. Aşağıdaki diyagram ve tablo LED Ekran ön panelini açıklanmaktadır.
   
    ![Ön OPS panelindeki sistem kimliği](./media/storsimple-power-cooling-module-replacement/IC740991.png)
   
     **Şekil 1** cihazın ön panel  
   
   | Etiket | Açıklama |
   |:--- |:--- |
   | 1 |Sessiz düğmesi |
   | 2 |Sistem gücü |
   | 3 |Modül hatası |
   | 4 |Mantıksal hatası |
   | 5 |Birim kimliği görüntüleme |
3. Birincil muhafaza arkasında izleme gösterge LED'leri hatalı PCM tanımlamak için de kullanılabilir. Aşağıdaki diyagram ve LED'leri hatalı PCM bulmak için nasıl kullanılacağını anlamak için tablosuna bakın. Örneğin, varsa LED karşılık gelen **Fan başarısız** olan aydınlatma, fan başarısız oldu. Benzer şekilde, varsa LED karşılık gelen **AC başarısız** olan aydınlatma, güç kaynağı başarısız oldu. 
   
    ![Cihaz PCM izleme gösterge LED'leri devre kartı](./media/storsimple-power-cooling-module-replacement/IC740992.png)
   
     **Şekil 2** geri of PCM LED'leri göstergesi
   
   | Etiket | Açıklama |
   |:--- |:--- |
   | 1 |AC güç kesintisi |
   | 2 |Fan hatası |
   | 3 |Pil hatası |
   | 4 |PCM TAMAM |
   | 5 |DC Güç kesintisi |
   | 6 |Sağlıklı pil |
4. Başarısız PCM modülü bulmak için StorSimple cihazı arkası aşağıdaki diyagrama bakın. PCM 0 solda ve PCM 1 sağ tarafta. Aşağıdaki tabloda modülleri açıklanmaktadır.
   
     ![Aygıt birincil muhafaza modüllerinin devre kartı](./media/storsimple-power-cooling-module-replacement/IC740994.png)
   
     **Şekil 3** eklenti modülleri aygıtla arkasına 
   
   | Etiket | Açıklama |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Denetleyici 0 |
   | 4 |Denetleyici 1 |
5. Hatalı PCM devre dışı bırakma ve güç kaynağı kablosunun bağlantısını kesebilirsiniz. PCM şimdi kaldırabilirsiniz.
6. Mandal ve Flash erişebildiğinizden arasındaki PCM tanıtıcısı tarafında kavramak ve bunları birlikte tanıtıcı açmak için sığdırması.
   
    ![PCM tanıtıcısı açılıyor](./media/storsimple-power-cooling-module-replacement/IC740995.png)
   
    **Şekil 4** PCM tanıtıcısı açma
7. Tanıtıcı kavramayın ve PCM kaldırın.
   
    ![Cihaz PCM'i kaldırılıyor](./media/storsimple-power-cooling-module-replacement/IC740996.png)
   
    **Şekil 5** PCM kaldırma

## <a name="install-a-replacement-pcm"></a>PCM yenisini yükleyin
StorSimple Cihazınızı bir PCM yüklemek için bu yönergeleri izleyin. Değiştirme (764 W PCMs için yalnızca geçerlidir) PCM yüklemeden önce yedek pil modülü eklediğiniz emin olun. Daha fazla bilgi için bkz: nasıl yapılır [kaldırın ve bir yedek pil Modül Ekle](storsimple-battery-replacement.md).

#### <a name="to-install-a-pcm"></a>Bir PCM yüklemek için
1. Bu kutu doğru yerini PCM sahip olduğunuzu doğrulayın. 764 W PCM birincil muhafaza gerekir ve EBOD muhafazası 580 W PCM gerekiyor. Birincil muhafazada 580 W PCM veya EBOD muhafazada 764 W PCM kullanmaya çalışmamalısınız. Aşağıdaki resimde bu bilgileri PCM yapıştırılmış etiketini tanımlamak nereye gösterir.
   
    ![Cihaz PCM etiketi](./media/storsimple-power-cooling-module-replacement/IC740973.png)
   
    **Şekil 6** PCM etiketi
2. Bağlayıcılar için belirli dikkat kasası hasara karşı denetleyin. 
   
   > [!NOTE]
   > **Bağlayıcı PIN'ler Bükülü durumunda modülünü yüklemeyin.**
   > 
   > 
3. Açık konumda PCM tanıtıcısı ile modülün kasası kaydırın.
   
    ![Cihaz PCM'i yükleniyor](./media/storsimple-power-cooling-module-replacement/IC740975.png)
   
    **Şekil 7** PCM yükleme
4. PCM tanıtıcısı el ile kapatın. Tanıtıcı Mandal prosese gibi bir tıklama sesi. 
   
   > [!NOTE]
   > Bağlayıcı PIN'ler gerçekleştiriliyor olmak için hafifçe mandalı bırakılıyor olmadan tutamacı tug. Out PCM slayt, bağlayıcıları gerçekleştiriliyor önce Mandal kapatıldı anlamına gelir.
   > 
   > 
5. Güç kablolarını güç kaynağı ve PCM bağlayın.
6. Yükü Tahliye bales güvenli hale getirin. 
7. Üzerinde PCM açın.
8. Değiştirme başarılı olduğunu doğrulayın: Klasik Azure portalında, StorSimple Yöneticisi hizmeti, gitmek **aygıtları** > **Bakım** > **donanım durum**. Altında **paylaşılan bileşenleri**, PCM durumu yeşil olması gerekir. 
   
   > [!NOTE]
   > Değiştirme PCM tamamen başlatmak için birkaç dakika sürebilir.
   > 
   > 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [StorSimple donanım bileşeni değiştirme](storsimple-hardware-component-replacement.md).

