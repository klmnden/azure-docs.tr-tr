---
title: "StorSimple cihaz denetleyicisini değiştirme | Microsoft Docs"
description: "Kaldırdığınızda ve değiştirdiğinizde, StorSimple Cihazınızda biri veya her ikisi denetleyicisi modülleri açıklanmaktadır."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: e25b52b7-60f5-47f3-bffc-6c157d57ab5d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/02/2017
ms.author: alkohli
ms.openlocfilehash: b8c6ebddef89d48d8121da5777e62e311c234666
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="replace-a-controller-module-on-your-storsimple-device"></a>Bir denetleyici modülü, StorSimple Cihazınızda değiştirin
> [!NOTE]
> StorSimple için Klasik portalı kullanım dışıdır. StorSimple cihaz yöneticileri yeni Azure portalına kullanımdan zamanlamaya göre otomatik olarak taşır. Bir e-posta ve bu taşıma için portal bir bildirim alırsınız. Bu belgede ayrıca yakında kullanımdan kaldırılacaktır. Bu makalede yeni Azure portalına için sürümünü görüntülemek için şu adrese gidin [Denetleyici Modülü, StorSimple Cihazınızda Değiştir](storsimple-8000-controller-replacement.md). Taşıma hakkında herhangi bir sorunuz için bkz: [SSS: Azure portalına taşıma](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Genel Bakış
Bu öğretici, kaldırmak ve StorSimple cihazını biri veya her ikisi denetleyicisi modüllerdeki yerini açıklanmaktadır. Ayrıca, tek ve çift denetleyicisi değiştirme senaryoları için temel mantığını anlatılmaktadır.

> [!NOTE]
> Bir denetleyici değiştirme gerçekleştirmeden önce denetleyicisi bellenim her zaman en son sürüme güncelleştirir öneririz.
> 
> StorSimple Cihazınızı zarar önlemek için LED'leri aşağıdakilerden biri gösteren kadar denetleyicisi çıkarmayın:
> 
> * Tüm ışık OFF ' dir.
> * LED 3 ![yeşil onay simgesi](./media/storsimple-controller-replacement/HCS_GreenCheckIcon.png), ve ![kırmızı çarpı simgesi](./media/storsimple-controller-replacement/HCS_RedCrossIcon.png) yanıp olan ve LED 0 ve LED 7 **ON**.
> 
> 

Aşağıdaki tabloda, desteklenen denetleyicisi değiştirme senaryolar gösterilmektedir.

| Durumu | Değiştirme senaryosu | Geçerli ilgili yordamı |
|:--- |:--- |:--- |
| 1 |Bir denetleyici başarısız durumda, diğer denetleyicisi sağlam ve etkin. |[Tek bir denetleyici değiştirme](#replace-a-single-controller), açıklayan [tek denetleyici değiştirme ardındaki mantığı](#single-controller-replacement-logic), yanı sıra [değiştirme adımları](#single-controller-replacement-steps). |
| 2 |İki denetleyiciye başarısız olmuş ve değiştirme gerektirir. Kasa, diskleri ve disk kasası iyi durumda. |[Çift denetleyici değiştirme](#replace-both-controllers), açıklayan [çift denetleyici değiştirme ardındaki mantığı](#dual-controller-replacement-logic), yanı sıra [değiştirme adımları](#dual-controller-replacement-steps). |
| 3 |Aynı aygıttan veya farklı cihaz denetleyicileri değiştirilen. Kasa, diskleri ve disk kasaları iyi durumda. |Bir yuva uyuşmazlığı uyarı iletisi görüntülenir. |
| 4 |Bir denetleyici eksik ve diğer denetleyicisi başarısız olur. |[Çift denetleyici değiştirme](#replace-both-controllers), açıklayan [çift denetleyici değiştirme ardındaki mantığı](#dual-controller-replacement-logic), yanı sıra [değiştirme adımları](#dual-controller-replacement-steps). |
| 5 |Bir veya iki denetleyicileri başarısız oldu. Cihaz seri konsolu veya Windows PowerShell uzaktan iletişimini erişemez. |[Microsoft Support başvurun](storsimple-contact-microsoft-support.md) el ile denetleyicisi değiştirme yordamı. |
| 6 |Denetleyicileri nedeniyle olabilir farklı yapım sürümüne sahip:<ul><li>Denetleyicileri farklı yazılım sürümüne sahip.</li><li>Denetleyicileri farklı üretici yazılımı sürümüne sahip.</li></ul> |Denetleyici yazılım sürümleri farklıysa, değiştirme mantığı olduğunu algılarsa ve yazılım sürümü değiştirme denetleyicisinde güncelleştirir.<br><br>Denetleyici bellenim sürümleri farklıdır ve bellenim sürümü eski **değil** otomatik olarak yükseltilebilir bir uyarı iletisi Azure Klasik Portalı'nda da görüntülenir. Güncelleştirmeleri taramak ve bellenim güncelleştirmeleri yüklemeniz gerekir.</br></br>Denetleyici bellenim sürümleri farklıdır ve eski bellenim sürümü otomatik olarak yükseltilebilir ise, denetleyici değiştirme mantığı bu algılar ve denetleyici başlatıldıktan sonra üretici yazılımı otomatik olarak güncelleştirilir. |

Bu başarısız olursa bir Denetleyici Modülü kaldırmanız gerekir. Bir tek denetleyici değiştirme ya da çift denetleyici değiştirme sonuçlanabilir bir veya iki Denetleyici Modülü başarısız olabilir. Değiştirme yordamları ve bunların ardındaki mantığı için aşağıdakilere bakın:

* [Tek bir denetleyiciyi değiştirme](#replace-a-single-controller)
* [Hem denetleyicileri Değiştir](#replace-both-controllers)
* [Bir denetleyici Kaldır](#remove-a-controller)
* [Bir denetleyici Ekle](#insert-a-controller)
* [Cihazınızı etkin denetleyicisinde tanımlayın](#identify-the-active-controller-on-your-device)

> [!IMPORTANT]
> Bir denetleyici değiştirme ve kaldırma gözden önce güvenlik bilgileri [StorSimple donanım bileşeni değiştirme](storsimple-hardware-component-replacement.md).
> 
> 

## <a name="replace-a-single-controller"></a>Tek bir denetleyici değiştirin
Microsoft Azure StorSimple cihazında iki denetleyicilerinden biri başarısız oldu, arızalı veya eksik olduğunda, tek bir denetleyici değiştirmeniz gerekiyor. 

### <a name="single-controller-replacement-logic"></a>Tek denetleyici değiştirme mantığı
Tek denetleyici yenisini önce başarısız olan denetleyici kaldırmanız gerekir. (Kalan denetleyicisi aygıt etkin denetleyicisidir.) Değiştirme denetleyicisi eklediğinizde, aşağıdaki eylemler gerçekleşir:

1. Yedek denetleyicisini hemen StorSimple cihazı ile iletişim başlatır.
2. Bir anlık görüntü sanal sabit disk (VHD) etkin denetleyiciye değiştirme denetleyicisinde kopyalanır.
3. Böylece bu VHD'den değiştirme Denetleyicisi başlatıldığında, bu bekleme denetleyicisi olarak tanınır anlık görüntü değiştirilir.
4. Değişiklikler tamamlandığında, yedek denetleyicisini bekleme denetleyicisi olarak başlar.
5. İki denetleyiciye çalıştırırken, küme çevrimiçi olur.

### <a name="single-controller-replacement-steps"></a>Tek denetleyici değiştirme adımları
Microsoft Azure StorSimple Cihazınızı denetleyicilerinden biri başarısız olursa, aşağıdaki adımları tamamlayın. (Diğer denetleyicisi etkin ve çalışıyor olması gerekir. Her iki denetleyicileri başarısız veya arıza durumunda Git [çift denetleyici değiştirme adımları](#dual-controller-replacement-steps).)

> [!NOTE]
> Yeniden başlatın ve tamamen tek denetleyici değiştirme yordamdan kurtarmak denetleyici için 30 – 45 dakika sürebilir. Tüm yordamı için toplam süreyi kabloları ekleme de dahil olmak üzere yaklaşık 2 saat değil.
> 
> 

#### <a name="to-remove-a-single-failed-controller-module"></a>Tek bir Denetleyici Modülü kaldırılamadı
1. Azure Klasik portalı, StorSimple Yöneticisi hizmeti Git tıklatın **aygıtları** sekmesine ve ardından izlemek istediğiniz cihazın adına tıklayın.
2. Git **Bakım > donanım durum**. Denetleyici 0 ve denetleyici 1 durumunu hata belirten kırmızı olması gerekir.
   
   > [!NOTE]
   > Tek denetleyici değiştirme başarısız denetleyicide her zaman bir bekleme denetleyicisi değil.
   > 
   > 
3. Şekil 1 ve aşağıdaki tabloda başarısız Denetleyici Modülü bulmak için kullanın.  
   
    ![Aygıt birincil muhafaza modüllerinin devre kartı](./media/storsimple-controller-replacement/IC740994.png)
   
    **Şekil 1** geri of StorSimple cihazı
   
   | Etiket | Açıklama |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Denetleyici 0 |
   | 4 |Denetleyici 1 |
4. Başarısız denetleyicisinde tüm bağlı ağ kablolarını veri noktalarından kaldırın. 8600 model kullanıyorsanız, aynı zamanda EBOD denetleyicisi için denetleyici bağlanmak SAS kabloları kaldırın.
5. Adımları [bir denetleyici kaldırmak](#remove-a-controller) başarısız denetleyicisini kaldırmak için. 
6. Fabrika değiştirme başarısız olan denetleyici kaldırıldığı yuvasında yükleyin. Bu tek denetleyici değiştirme mantığı tetikler. Daha fazla bilgi için bkz: [tek denetleyicisi değiştirme mantığı](#single-controller-replacement-logic).
7. Arka planda tek denetleyici değiştirme mantığı ilerler olsa da, kabloları yeniden bağlanın. Tüm kabloların önce değiştirme bağlanmış aynı şekilde bağlanmak için dikkatli olun.
8. Denetleyici yeniden başlatıldıktan sonra denetleme **denetleyicisi durumunu** ve **küme durumu** denetleyicisi sağlıklı bir duruma ve bekleme modunda olduğunu doğrulamak için Azure Klasik portalında.

> [!NOTE]
> Cihaz seri Konsolu aracılığıyla izliyorsanız, denetleyici değiştirme yordamdan Kurtarma sırasında birden çok kez yeniden görebilirsiniz. Daha sonra seri konsol menüsünde sunulduğunda değiştirme tam olduğunu bildiğiniz. Menü denetleyicisi değiştirme başlatmanın iki saat içinde görünmüyorsa, lütfen [Microsoft Support başvurun](storsimple-contact-microsoft-support.md).
>
> Güncelleştirme 4'ten başlayarak, cmdlet'ini de kullanabilirsiniz `Get-HCSControllerReplacementStatus` denetleyicisi değiştirme işlemi durumunu izlemek için cihazın Windows PowerShell arabiriminde.
> 

## <a name="replace-both-controllers"></a>Hem denetleyicileri Değiştir
Microsoft Azure StorSimple cihazında hem denetleyicileri başarısız oldu, arızalı olan ya da eksik. hem denetleyicileri değiştirmeniz gerekiyor. 

### <a name="dual-controller-replacement-logic"></a>Çift denetleyici değiştirme mantığı
Çift denetleyici değiştirme hem başarısız denetleyicileri kaldırın ve değişiklik ekleyin. İki değiştirme denetleyicileri eklendiğinde, aşağıdaki eylemler gerçekleşir:

1. Yuva 0'ın yedek denetleyicisini aşağıdakileri denetler:
   
   1. Bellenim ve yazılım geçerli sürümleri kullanıyor mu?
   2. Bu kümenin bir parçası?
   3. Çalıştıran eş denetleyicisi ve kümelenmiş olur?
      
      Bu koşulların hiçbiri doğruysa, en son günlük yedekleme için denetleyici görünüyor (bulunan **nonDOMstorage** sürücüde S). Denetleyici VHD'yi anlık görüntüsü en son yedeklemeden kopyalar.
2. Yuva 0 denetleyicisi kendisini görüntü için anlık görüntü kullanır.
3. Bu sırada, denetleyici 1 yuvadaki Imaging tamamlamak ve başlatmak için 0 denetleyicisi için bekler.
4. Denetleyici 0 başlatıldıktan sonra Denetleyici 1 0, denetleyici tarafından oluşturulan küme tek denetleyici değiştirme mantığı tetikler algılar. Daha fazla bilgi için bkz: [tek denetleyicisi değiştirme mantığı](#single-controller-replacement-logic).
5. Daha sonra hem denetleyicileri çalıştıran ve kümeyi çevrimiçi duruma gelir.

> [!IMPORTANT]
> Çift denetleyici yerini daha StorSimple cihazı yapılandırıldıktan sonra bile el ile yedekleme aygıtı ele temel alır. 24 saat geçtikten sonra günlük aygıt yapılandırma yedeklerini kadar tetiklenir değil. Çalışmak [Microsoft Support](storsimple-contact-microsoft-support.md) el ile Cihazınızı yedekleme yapmak için.
> 
> 

### <a name="dual-controller-replacement-steps"></a>Çift denetleyici değiştirme adımları
Microsoft Azure StorSimple Cihazınızı denetleyicileri her ikisi de başarısız olduğunda bu iş akışı gereklidir. Bu, bir veri merkezinde soğutma sistem çalışmayı durdurur ve sonuç olarak, iki denetleyiciye de kısa bir süre içinde başarısız olabilir. 8100 model StorSimple cihazı kapatma ya açık olduğundan ve bir 8600 kullanıp kullanmadığınıza bağlı olarak veya adımları farklı bir dizi gerekli değildir.

> [!IMPORTANT]
> Yeniden başlatın ve tam bir çift denetleyici değiştirme yordamından kurtarmak denetleyici için 1 saat 45 dakika sürebilir. Tüm yordamı için toplam süreyi kabloları ekleme de dahil olmak üzere yaklaşık 2,5 saat değil.
> 
> 

#### <a name="to-replace-both-controller-modules"></a>Her iki denetleyicisi modülleri değiştirmek için
1. Aygıt devre dışı bırakılırsa, bu adımı atlayın ve sonraki adıma geçebilirsiniz. Cihaz açıksa, aygıt devre dışı bırakın.
   
   1. 8600 model kullanıyorsanız, birincil muhafaza ilk kapatın ve ardından EBOD muhafazası devre dışı.
   2. Cihazı tamamen kapatıldı kadar bekleyin. Aygıtın arkasında tüm LED'leri kapalı olacaktır.
2. Veri bağlantı noktalarına bağlı tüm ağ kablolarını kaldırın. 8600 model kullanıyorsanız, aynı zamanda birincil muhafaza EBOD muhafazası bağlanmak SAS kabloları kaldırın.
3. Hem denetleyicileri StorSimple cihazınızdan kaldırın. Daha fazla bilgi için bkz: [bir denetleyici kaldırmak](#remove-a-controller).
4. Denetleyici 0 Fabrika yerini ilk yerleştirin ve denetleyici 1'i takın. Daha fazla bilgi için bkz: [bir denetleyici Ekle](#insert-a-controller). Çift denetleyici değiştirme mantığı tetikler. Daha fazla bilgi için bkz: [çift denetleyici değiştirme mantığı](#dual-controller-replacement-logic).
5. Arka planda denetleyicisi değiştirme mantığı ilerler olsa da, kabloları yeniden bağlanın. Tüm kabloların önce değiştirme bağlanmış aynı şekilde bağlanmak için dikkatli olun. Ayrıntılı yönergeler kablosu modelinizde için aygıt bölümüne bakın [StorSimple 8100 cihazınız yüklemek](storsimple-8100-hardware-installation.md) veya [StorSimple 8600 model Cihazınızı yüklemek](storsimple-8600-hardware-installation.md).
6. StorSimple cihazında açın. 8600 model kullanıyorsanız:
   
   1. EBOD muhafazası ilk açık olduğundan emin olun.
   2. EBOD muhafazası çalışıncaya kadar bekleyin.
   3. Birincil muhafaza açın.
   4. İlk denetleyicisi yeniden başlatır ve iyi durumda sonra sistem çalıştırırsınız.
      
      > [!NOTE]
      > Cihaz seri Konsolu aracılığıyla izliyorsanız, denetleyici değiştirme yordamdan Kurtarma sırasında birden çok kez yeniden görebilirsiniz. Ardından seri konsol menüsünde görüntülendiğinde değiştirme tam olduğunu bildiğiniz. Menü denetleyicisi değiştirme başlatma 2,5 saat içinde görünmüyorsa, lütfen [Microsoft Support başvurun](storsimple-contact-microsoft-support.md).
      > 
      > 

## <a name="remove-a-controller"></a>Bir denetleyici Kaldır
StorSimple Cihazınızı hatalı Denetleyici Modülü kaldırmak için aşağıdaki yordamı kullanın.

> [!NOTE]
> Aşağıdaki çizimler denetleyici 0 ' dir. Denetleyici 1 için bu ters.
> 
> 

#### <a name="to-remove-a-controller-module"></a>Bir Denetleyici Modülü kaldırmak için
1. Kaydırma kutusu ve erişebildiğinizden arasında modülü Mandal kavrayın.
2. Kaydırma kutusu ve denetleyici mandalı birlikte yayımlamayı erişebildiğinizden hafifçe sığdırması.
   
    ![Denetleyici mandalı bırakılıyor](./media/storsimple-controller-replacement/IC741047.png)
   
    **Şekil 2** gönderilirler denetleyici mandalı
3. Mandal gövdeden denetleyiciyi kaydırarak açmak için bir işleyici kullanın.
   
    ![Denetleyiciyi kaydırarak gövdeden çıkarma](./media/storsimple-controller-replacement/IC741048.png)
   
    **Şekil 3** denetleyiciyi kaydırarak gövdeden çıkarma

## <a name="insert-a-controller"></a>Bir denetleyici Ekle
StorSimple Cihazınızı hatalı bir modül kaldırdıktan sonra bir denetleyici üretecini sağlanan modülünü yüklemek için aşağıdaki yordamı kullanın.

#### <a name="to-install-a-controller-module"></a>Bir denetleyici modülünü yüklemek için
1. Arabirim bağlayıcılar için herhangi bir zarar olup olmadığını denetleyin. Bağlayıcı PIN'ler hiçbirini zarar görmüş veya Bükülü modülü yüklemeyin.
2. Mandal tam olarak yayımlanan sırada Denetleyici Modülü kasaya kaydırın. 
   
    ![Denetleyiciyi kaydırarak gövdeye](./media/storsimple-controller-replacement/IC741053.png)
   
    **Şekil 4** hareketli denetleyicisi kasa ile
3. Denetleyici Modülü kasaya göndermek devam ederken mandalı kapatılıyor eklenen Denetleyici Modülü ile başlar. Mandal yerine denetleyicisi kılavuza devreye.
   
    ![Denetleyici mandalı kapatılıyor](./media/storsimple-controller-replacement/IC741054.png)
   
    **Şekil 5** denetleyici mandalı kapatılıyor
4. Mandal yerine tutturur zaman bitirdiniz. **Tamam** LED üzerinde şimdi olmalıdır.  
   
   > [!NOTE]
   > Denetleyici ve etkinleştirmek için LED için 5 dakikaya kadar sürebilir.
   > 
   > 
5. Değiştirme Azure Klasik Portalı'nda başarılı olduğunu doğrulamak için Git **aygıtları** > **Bakım** > **donanım durum**, ve Denetleyici 0 ve denetleyici 1 sağlıklı olduğundan emin olun (durum yeşil).

## <a name="identify-the-active-controller-on-your-device"></a>Cihazınızı etkin denetleyicisinde tanımlayın
Bir StorSimple cihazında etkin denetleyicisi bulmaya gerektiren pek çok durumda, ilk kez aygıt kaydı veya denetleyicisi değiştirme gibi vardır. Etkin denetleyicisi tüm disk bellenim ve ağ işlemleri işler. Etkin denetleyicisi tanımlamak için aşağıdaki yöntemlerden herhangi birini kullanabilirsiniz:

* [Etkin denetleyicisi tanımlamak için Klasik Azure portalını kullanın](#use-the-azure-classic-portal-to-identify-the-active-controller)
* [StorSimple için Windows PowerShell active denetleyicisi tanımlamak için kullanın](#use-windows-powershell-for-storsimple-to-identify-the-active-controller)
* [Etkin denetleyicisi tanımlamak için fiziksel aygıt denetleyin](#check-the-physical-device-to-identify-the-active-controller)

Bu yordamların her biri sonraki açıklanmıştır.

### <a name="use-the-azure-classic-portal-to-identify-the-active-controller"></a>Etkin denetleyicisi tanımlamak için Klasik Azure portalını kullanın
Klasik Azure portalında gidin **aygıtları** > **Bakım**ve kaydırma **denetleyicileri** bölümü. Burada, hangi denetleyicisi etkindir doğrulayabilirsiniz.

![Klasik Azure Portalı'nda etkin denetleyiciyi belirle](./media/storsimple-controller-replacement/IC752072.png)

**Şekil 6** etkin denetleyicisi gösteren Klasik Azure portalı

### <a name="use-windows-powershell-for-storsimple-to-identify-the-active-controller"></a>StorSimple için Windows PowerShell active denetleyicisi tanımlamak için kullanın
Seri konsol üzerinden Cihazınızı eriştiğinizde bir başlık iletisi görüntülenir. Başlık iletisi modeli, ad, yüklü yazılım sürümü ve eriştiğiniz denetleyicisi durumunu gibi temel aygıt bilgileri içerir. Aşağıdaki resimde bir başlık iletisi örneği gösterilmiştir:

![Seri tanıtıcı iletisi](./media/storsimple-controller-replacement/IC741098.png)

**Şekil 7** etkin olarak başlık iletisi gösteren denetleyici 0

Başlık iletisi bağlı denetleyicisi etkin veya Pasif olduğunu belirlemek için kullanabilirsiniz.

### <a name="check-the-physical-device-to-identify-the-active-controller"></a>Etkin denetleyicisi tanımlamak için fiziksel aygıt denetleyin
Cihazınızı etkin denetleyicisinde belirlemek için birincil muhafaza arkasında veri 5 bağlantı noktası yukarıda mavi LED bulun.

Bu ışığı yanıp sönen, denetleyici etkindir ve diğer bekleme modunda denetleyicisidir. Aşağıdaki diyagram ve tablo yardımcı olarak kullanabilirsiniz.

![Veri bağlantı noktalarına sahip cihaz birincil muhafaza devre kartı](./media/storsimple-controller-replacement/IC741055.png)

**Şekil 8** veri bağlantı noktası ve izleme LED'leri birincil muhafaza arkasına

| Etiket | Açıklama |
|:--- |:--- |
| 1-6 |Veri 0 – 5 ağ bağlantı noktaları |
| 7 |Mavi LED |

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [StorSimple donanım bileşeni değiştirme](storsimple-hardware-component-replacement.md).

