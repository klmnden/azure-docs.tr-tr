---
title: StorSimple 8000 serisi cihaz denetleyicisini değiştirme | Microsoft Docs
description: Kaldırın ve bir veya iki denetleyici modülleri StorSimple 8000 serisi Cihazınızı açıklanmaktadır.
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
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: dd2f6fcc9b2f5d716566e91e89487969613d1005
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61482915"
---
# <a name="replace-a-controller-module-on-your-storsimple-device"></a>StorSimple Cihazınızda bir Denetleyici Modülü değiştirin
## <a name="overview"></a>Genel Bakış
Bu öğreticide, kaldırın ve StorSimple cihaz bir veya iki denetleyici modülleri açıklanmaktadır. Ayrıca, tek ve çift denetleyicisi değiştirme senaryoları için temel mantığı anlatılmaktadır.

> [!NOTE]
> Denetleyici değiştirme gerçekleştirmeden önce denetleyici üretici yazılımı her zaman en son sürüme güncelleştirme öneririz.
> 
> LED'lerini aşağıdakilerden biri gösterildiğini kadar StorSimple cihazınıza hasarı önlemek için denetleyici çıkarma değil:
> 
> * Tüm ışıklarının kapalı.
> * LED 3 ![yeşil onay simgesi](./media/storsimple-controller-replacement/HCS_GreenCheckIcon.png), ve ![kırmızı çarpı simgesi](./media/storsimple-controller-replacement/HCS_RedCrossIcon.png) yanıp olan LED 0 ve 7 LED **ON**.


Aşağıdaki tabloda, desteklenen denetleyicisi değiştirme senaryoları gösterilmektedir.

| Servis talebi | Değiştirme senaryosu | Geçerli ilgili yordamı |
|:--- |:--- |:--- |
| 1 |Bir denetleyici başarısız durumda, diğer denetleyicinin sağlam ve etkin. |[Tek bir denetleyici değiştirme](#replace-a-single-controller), açıklayan [tek denetleyici değiştirme ardındaki mantığı](#single-controller-replacement-logic), hem de [değiştirme adımları](#single-controller-replacement-steps). |
| 2 |Her iki denetleyici de başarısız olmuş ve değiştirme gerektirir. Kasa, diskleri ve disk kasası iyi durumda. |[Çift denetleyici değiştirme](#replace-both-controllers), açıklayan [çift denetleyici değiştirme ardındaki mantığı](#dual-controller-replacement-logic), hem de [değiştirme adımları](#dual-controller-replacement-steps). |
| 3 |Denetleyicileri aynı cihaz veya farklı cihazlardan değiştirilir. Kasa, diskleri ve disk kasaları iyi durumda. |Bir yuva uyuşmazlığı uyarı iletisi görüntülenir. |
| 4 |Bir denetleyici eksik ve diğer denetleyiciye başarısız olur. |[Çift denetleyici değiştirme](#replace-both-controllers), açıklayan [çift denetleyici değiştirme ardındaki mantığı](#dual-controller-replacement-logic), hem de [değiştirme adımları](#dual-controller-replacement-steps). |
| 5 |Bir veya iki denetleyicilerinin başarısız oldu. Cihaz seri konsolu veya Windows PowerShell uzaktan iletişimini erişemez. |[Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) el ile denetleyicisi değiştirme yordamı. |
| 6 |Denetleyicileri nedeniyle olabilir bir farklı bir derleme sürümü vardır:<ul><li>Denetleyicileri farklı yazılım sürümü vardır.</li><li>Denetleyicileri farklı üretici yazılımı sürümü vardır.</li></ul> |Denetleyici yazılım sürümleri farklıysa değiştirme mantığı olduğunu algılarsa ve değiştirme denetleyici yazılım sürümüne güncelleştirir.<br><br>Denetleyici üretici yazılımı sürümlerine farklıdır ve eski üretici yazılımı sürümü **değil** otomatik olarak yükseltilebilir bir uyarı iletisi Azure portalında da görüntülenir. Güncelleştirmeleri taramak ve üretici yazılımı güncelleştirmelerini yüklemek gerekir.</br></br>Denetleyici üretici yazılımı sürümlerine farklıdır ve eski üretici yazılımı sürümü otomatik olarak yükseltilebilir ise, denetleyici değiştirme mantığı bunu algılar ve denetleyici başlatıldıktan sonra üretici yazılımı otomatik olarak güncelleştirilir. |

Başarısız olması durumunda bir Denetleyici Modülü kaldırmanız gerekir. Bir tek denetleyici değiştirme ya da çift denetleyici değiştirme neden denetleyicisi modülleri biri veya ikisi de başarısız olabilir. Değiştirme yordamları ve bunları ardındaki mantığı için aşağıdakilere bakın:

* [Tek bir denetleyiciyi değiştirme](#replace-a-single-controller)
* [Her iki denetleyiciyi de değiştirme](#replace-both-controllers)
* [Bir denetleyici Kaldır](#remove-a-controller)
* [Bir denetleyici Ekle](#insert-a-controller)
* [Cihazınızda etkin denetleyiciyi belirleme](#identify-the-active-controller-on-your-device)

> [!IMPORTANT]
> Bir denetleyici değiştirme ve kaldırma gözden önce güvenlik bilgileri [StorSimple donanım bileşeni değişimi](storsimple-8000-hardware-component-replacement.md).
> 
> 

## <a name="replace-a-single-controller"></a>Tek bir denetleyiciyi değiştirme
Microsoft Azure StorSimple cihazında iki denetleyicilerden biri başarısız olursa, arızalı veya eksik olduğunda, tek bir denetleyiciyi değiştirme gerekir.

### <a name="single-controller-replacement-logic"></a>Tek bir denetleyici değiştirme mantığı
Bir tek denetleyici değiştirme işleminde başarısız denetleyicisi önce kaldırmanız gerekir. (Cihaz kalan denetleyici etkin denetleyiciyi içindir.) Değiştirme denetleyicisi eklediğinizde, aşağıdaki eylemler gerçekleşir:

1. StorSimple cihazı ile iletişim kurulurken değiştirme denetleyicisi hemen başlar.
2. Etkin denetleyici için sanal sabit disk (VHD) bir anlık görüntüsünü değiştirme denetleyicisinde kopyalanır.
3. Anlık görüntü, böylece değiştirme denetleyicisi bu VHD'den başladığında, hazır bekleyen denetleyicinin tanınır değiştirilir.
4. Değişiklikler tamamlandığında, değişiklik denetleyicisi hazır bekleyen denetleyicinin başlar.
5. Her iki denetleyici de çalıştırdığınızda, küme çevrimiçine döner.

### <a name="single-controller-replacement-steps"></a>Tek bir denetleyici değiştirme adımları
Microsoft Azure StorSimple cihazınızdaki denetleyicilerden biri başarısız olursa, aşağıdaki adımları tamamlayın. (Diğer denetleyiciye etkin ve çalışıyor olması gerekir. Her iki denetleyicilerinin başarısız veya sitelerinin arıza yapmasına, Git [çift denetleyici değiştirme adımları](#dual-controller-replacement-steps).)

> [!NOTE]
> Uygulamanın denetleyicisini yeniden başlatın ve tek bir denetleyici değiştirme yordamdan tamamen kurtarmak 30-45 dakika sürebilir. Yordamın tamamı için toplam süreyi kablolar ekleme dahil yaklaşık 2 saat.


#### <a name="to-remove-a-single-failed-controller-module"></a>Tek bir Denetleyici Modülü kaldırılamadı
1. Azure portalında StorSimple cihaz Yöneticisi hizmetine gidin, **cihazları**ve sonra izlemek istediğiniz cihazın adına tıklayın.
2. Git **İzleyici > donanım sistem durumu**. Denetleyici 0'ı veya denetleyici 1 durumu hata gösteren kırmızı, olmalıdır.
   
   > [!NOTE]
   > Başarısız bir tek denetleyici değiştirme, her zaman hazır bekleyen denetleyicinin denetleyicisidir.
   
3. Şekil 1 ve aşağıdaki tabloda başarısız Denetleyici Modülü bulmak için kullanın.
   
    ![Cihaz birincil kutusu modüllerinin devre kartı](./media/storsimple-controller-replacement/IC740994.png)
   
    **Şekil 1** arka of StorSimple cihaz
   
   | Etiket | Açıklama |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Denetleyici 0 |
   | 4 |Denetleyici 1 |
4. Başarısız denetleyicide tüm bağlı ağ kablolarını veri noktalarından kaldırın. Ayrıca bir 8600 modeli kullandığınız EBOD denetleyicisi için denetleyici bağlanan SAS kablolu jbod'lere kaldırın.
5. Bağlantısındaki [bir denetleyicisini kaldırma](#remove-a-controller) başarısız denetleyicisini kaldırmak için.
6. Fabrika değiştirme başarısız denetleyicisi kaldırıldığı yuvasında yükleyin. Bu, tek bir denetleyici değiştirme mantıksal tetikler. Daha fazla bilgi için [tek denetleyicisi değiştirme mantıksal](#single-controller-replacement-logic).
7. Arka planda tek denetleyici değiştirme mantıksal ilerledikçe, ancak kabloları bağlanın. Kabloların değiştirme önce bağlanmış tamamen aynı şekilde bağlanmak için dikkatli olun.
8. Denetleyici yeniden başladıktan sonra denetleme **denetleyici durumu** ve **küme durumu** denetleyicinin sağlam bir durumda olduğunu ve bekleme modunda olduğunu doğrulamak için Azure portalında.

> [!NOTE]
> Cihaz seri Konsolu aracılığıyla izliyorsanız, denetleyici değiştirme yordamdan kurtarılmakta olduğu sırada birden çok kez yeniden görebilirsiniz. Daha sonra seri konsol menüsünde sunulduğunda, bu değişiklik tamamlandıktan bildiğiniz. Menü denetleyicisi değiştirme başlangıç iki saat içinde görünmüyorsa, lütfen [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md).
>
> Güncelleştirme 4'ten başlayarak, cmdlet'ini de kullanabilirsiniz `Get-HCSControllerReplacementStatus` cihaz denetleyicisini değiştirme işleminin durumunu izlemek için Windows PowerShell arabiriminde.
> 

## <a name="replace-both-controllers"></a>Her iki denetleyiciyi de değiştirme
Microsoft Azure StorSimple cihazında her iki denetleyicilerinin başarısız oldu, arızalı olan ya da eksik her iki denetleyiciyi de değiştirme gerekir. 

### <a name="dual-controller-replacement-logic"></a>Çift denetleyici değiştirme mantığı
Çift denetleyici yerine ilk iki başarısız denetleyicilerini kaldırın ve sonra değişiklik ekleyin. İki değiştirme denetleyicileri eklendiğinde, aşağıdaki eylemler gerçekleşir:

1. Yuva 0 değiştirme denetleyicide aşağıdakileri denetler:
   
   1. Bu, yazılım ve üretici yazılımı güncel sürümlerini kullanıyor mu?
   2. Bu kümenin bir parçası?
   3. Çalışan eş denetleyicisidir ve bu kümelenmiş?
      
      Bu koşulların hiçbiri doğruysa, denetleyici, en son günlük yedekleme için arar. (bulunan **nonDOMstorage** sürücüde S). Denetleyici yedekten VHD'nin en son anlık görüntüyü kopyalar.
2. Yuva 0 denetleyicisi kendisini görüntü için anlık görüntü kullanır.
3. Bu arada, görüntüleme tamamlamak ve başlatmak için 0 denetleyicisi için Denetleyici 1 yuvasındaki bekler.
4. Denetleyici 0 başlatıldıktan sonra Denetleyici 1, 0, denetleyici tarafından oluşturulan küme tek denetleyici değiştirme mantıksal tetikler algılar. Daha fazla bilgi için [tek denetleyicisi değiştirme mantıksal](#single-controller-replacement-logic).
5. Ardından, her iki denetleyicilerinin çalıştırıyor olması ve kümeyi çevrimiçi duruma gelir.

> [!IMPORTANT]
> StorSimple cihazı yapılandırdıktan sonra çift denetleyici yerine onu el ile cihazda yedek Al gereklidir. 24 saat geçtikten sonra günlük cihaz yapılandırma yedeklemeler kadar tetiklenir değil. Çalışmak [Microsoft Support](storsimple-8000-contact-microsoft-support.md) el ile cihazınızın yedekleme yapmak.


### <a name="dual-controller-replacement-steps"></a>Çift denetleyici değiştirme adımları
Bu iş akışı, Microsoft Azure StorSimple cihaz denetleyicilerinin hem de başarısız olduğunda gereklidir. Bu, soğutma sistem çalışmayı durduruyor ve sonuç olarak, her iki denetleyici de kısa bir süre içinde başarısız bir veri merkezinde gerçekleşebilir. 8100 model veya StorSimple cihazını kapatma veya açık olup olmadığı ve 8600 ürünü kullanmanıza bağlı olarak, farklı bir dizi adım gereklidir.

> [!IMPORTANT]
> 1 saat boyunca yeniden başlatın ve bir çift denetleyici değiştirme yordamdan tamamen kurtarmak için denetleyici 45 dakika sürebilir. Yordamın tamamı için toplam süreyi kablolar ekleme dahil yaklaşık 2,5 saat.

#### <a name="to-replace-both-controller-modules"></a>Her iki denetleyici modülleri değiştirmek için
1. Cihaz devre dışı bırakılırsa, bu adımı atlayın ve sonraki adıma geçin. Cihaz açıksa, cihazı devre dışı bırakın.
   
   1. 8600 model kullanıyorsanız, birincil kasasının ilk ve EBOD muhafazası açın.
   2. Cihazı tamamen kapatıldı kadar bekleyin. Tüm cihaz arkasındaki LED'lerini devre dışı olacaktır.
2. Veri bağlantı noktalarına bağlı olan tüm ağ kablolarını kaldırın. Ayrıca bir 8600 model kullanıyorsanız, birincil muhafaza bağlanmak için EBOD muhafazası SAS kablolu jbod'lere kaldırın.
3. Her iki denetleyicilerinin StorSimple cihazınızdan kaldırın. Daha fazla bilgi için [bir denetleyicisini kaldırma](#remove-a-controller).
4. Denetleyici 0 için Fabrika değiştirme önce ekleyin ve denetleyici 1'i takın. Daha fazla bilgi için [bir denetleyici Ekle](#insert-a-controller). Bu ikili denetleyicisi değiştirme mantıksal tetikler. Daha fazla bilgi için [çift denetleyici değiştirme mantıksal](#dual-controller-replacement-logic).
5. Arka planda denetleyicisi değiştirme mantıksal ilerledikçe, ancak kabloları bağlanın. Kabloların değiştirme önce bağlanmış tamamen aynı şekilde bağlanmak için dikkatli olun. Ayrıntılı yönergeler için kablo modelinizde, cihaz bölümüne bakın [StorSimple 8100 cihazınızın yükleme](storsimple-8100-hardware-installation.md) veya [StorSimple 8600 cihazınızın yükleme](storsimple-8600-hardware-installation.md).
6. StorSimple cihazı açın. 8600 model kullanıyorsanız:
   
   1. EBOD muhafazası ilk açık olduğundan emin olun.
   2. EBOD muhafazası çalışıncaya kadar bekleyin.
   3. Birincil muhafaza açın.
   4. İlk denetleyicisi iyi durumda yeniden başlatır ve sonra sistemi çalıştırırsınız.
      
      > [!NOTE]
      > Cihaz seri Konsolu aracılığıyla izliyorsanız, denetleyici değiştirme yordamdan kurtarılmakta olduğu sırada birden çok kez yeniden görebilirsiniz. Daha sonra seri konsol menüsünde göründüğünde, bu değişiklik tamamlandıktan bildiğiniz. Menü denetleyicisi değiştirme başlangıç 2,5 saat içinde görünmüyorsa, lütfen [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md).
     
## <a name="remove-a-controller"></a>Bir denetleyici Kaldır
StorSimple cihazınızın hatalı Denetleyici Modülü kaldırmak için aşağıdaki yordamı kullanın.

> [!NOTE]
> Aşağıdaki çizimler için denetleyici 0 ' dir. Denetleyici 1 için bu ters.


#### <a name="to-remove-a-controller-module"></a>Bir denetleyici modülünü kaldırmak için
1. Thumb ve erişebildiğinizden arasında modülü Mandal kavrayın.
2. Thumb ve denetleyici mandalı birlikte yayımlamayı erişebildiğinizden yavaşça sığdırması.
   
    ![Denetleyici mandalı bırakılıyor](./media/storsimple-controller-replacement/IC741047.png)
   
    **Şekil 2** bırakma denetleyici mandalı
3. Mandal denetleyiciyi kaydırarak kaydırarak açmak için bir işleyici kullanın.
   
    ![Denetleyiciyi kaydırarak kayan](./media/storsimple-controller-replacement/IC741048.png)
   
    **Şekil 3** denetleyiciyi kaydırarak kayan

## <a name="insert-a-controller"></a>Bir denetleyici Ekle
StorSimple cihazınızın hatalı bir modül kaldırdıktan sonra bir denetleyici üreteci tarafından sağlanan modülünü yüklemek için aşağıdaki yordamı kullanın.

#### <a name="to-install-a-controller-module"></a>Bir Denetleyici Modülü yüklemek için
1. Arabirimi bağlayıcılar için herhangi bir zarar olup olmadığını denetleyin. Herhangi bir bağlayıcıyı PIN'ler zarar görmüş veya Eğilmiş modülü yüklemeyin.
2. Mandal tam olarak yayımlanır ancak Denetleyici Modülü kasaya kaydırın.
   
    ![Denetleyici kaydırarak gövdeye yerleştirme](./media/storsimple-controller-replacement/IC741053.png)
   
    **Şekil 4** hareketli denetleyicisi kasa ile
3. Denetleyici Modülü kasaya göndermeye devam ederken mandalı kapatılıyor eklenen Denetleyici Modülü ile başlar. Mandal yere denetleyici kılavuza ilgisini çekecek.
   
    ![Denetleyici mandalı kapatılıyor](./media/storsimple-controller-replacement/IC741054.png)
   
    **Şekil 5** denetleyici mandalı kapatılıyor
4. Mandal yere yaslanıp zaman hazırsınız. **Tamam** LED üzerinde artık olmalıdır.
   
   > [!NOTE]
   > Bu denetleyici ve etkinleştirmek için LED 5 dakikaya kadar sürebilir.
  
5. Yeni Azure portalında başarılı olduğunu doğrulamak için cihazınıza gidin ve ardından gidin **İzleyici** > **donanım sistem durumu**, emin olun hem denetleyici 0 ve denetleyici 1 iyi durumda (durum yeşil).

## <a name="identify-the-active-controller-on-your-device"></a>Cihazınızda etkin denetleyiciyi belirleme
Etkin denetleyici bir StorSimple cihazında bulmak ihtiyacınız olan pek çok durumda, ilk cihaz kaydı veya denetleyici değiştirme gibi vardır. Etkin denetleyiciyi tüm disk üretici yazılımı ve ağ işlemlerini işler. Etkin denetleyiciyi tanımlamak için aşağıdaki yöntemlerden herhangi birini kullanabilirsiniz:

* [Etkin denetleyiciyi belirleme için Azure portalını kullanma](#use-the-azure-portal-to-identify-the-active-controller)
* [Etkin denetleyiciyi belirleme için StorSimple için Windows PowerShell kullanın](#use-windows-powershell-for-storsimple-to-identify-the-active-controller)
* [Onay fiziksel cihazın etkin denetleyiciyi belirleme](#check-the-physical-device-to-identify-the-active-controller)

Bu yordamların her biri sonraki açıklanmıştır.

### <a name="use-the-azure-portal-to-identify-the-active-controller"></a>Etkin denetleyiciyi belirleme için Azure portalını kullanma
Azure portalında, cihazınıza gidin ve ardından **İzleyici** > **donanım sistem durumu**, kaydırın **denetleyicileri** bölümü. Hangi active denetleyicisidir doğrulayabilirsiniz.

![Azure Portalı'nda etkin denetleyiciyi belirleme](./media/storsimple-controller-replacement/IC752072.png)

**Şekil 6** etkin denetleyiciyi gösteren Azure portalı

### <a name="use-windows-powershell-for-storsimple-to-identify-the-active-controller"></a>Etkin denetleyiciyi belirleme için StorSimple için Windows PowerShell kullanın
Cihazınızın seri konsol üzerinden eriştiğinizde, bir başlık iletisi görüntülenir. Başlık iletisi modeli, adı, yüklü yazılım sürümü ve eğer denetleyici durumu gibi temel cihaz bilgileri içerir. Aşağıdaki görüntüde bir başlık iletisi örneği gösterilmektedir:

![Seri tanıtıcı iletisi](./media/storsimple-controller-replacement/IC741098.png)

**Şekil 7** etkin olarak başlık iletisini gösterme denetleyici 0

Başlık iletisi bağlandığınızdan denetleyicisini etkin veya Pasif olduğunu belirlemek için kullanabilirsiniz.

### <a name="check-the-physical-device-to-identify-the-active-controller"></a>Onay fiziksel cihazın etkin denetleyiciyi belirleme
Cihazınızda etkin denetleyiciyi belirleme için birincil muhafaza arkasına veri 5 numaralı yukarıdaki mavi LED bulun.

Bu LED yanıp sönen, denetleyici etkindir ve diğer denetleyiciye bekleme modundadır. Aşağıdaki tablo ve diyagram yardımcı olarak kullanabilirsiniz.

![Cihaz birincil devre kartı kutusunun devre kartı](./media/storsimple-controller-replacement/IC741055.png)

**Şekil 8** veri bağlantı noktaları ve izleme LED'lerini birincil muhafaza arkasına

| Etiket | Açıklama |
|:--- |:--- |
| 1-6 |DATA 0 – 5 ağ bağlantı noktaları |
| 7 |Mavi ışığı |

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [StorSimple donanım bileşeni değişimi](storsimple-8000-hardware-component-replacement.md).

