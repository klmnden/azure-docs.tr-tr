---
title: "StorSimple birimlerini (güncelleştirme 3) yönetme | Microsoft Docs"
description: "Ekleme, değiştirme, izlemek ve StorSimple birimleri silin ve bunları gerekirse çevrimdışına alma konusunda açıklanmaktadır."
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/08/2017
ms.author: alkohli
<<<<<<< HEAD
ms.openlocfilehash: 09f4de79ab9b0cdfafd10c7c7c29b0f8e6304f14
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
=======
ms.openlocfilehash: c9c575f42e6c8730b9404c62fb60e710d9d3bc80
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="use-the-storsimple-device-manager-service-to-manage-volumes-update-3-or-later"></a>Birimler (güncelleştirme 3 veya sonraki) yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma

## <a name="overview"></a>Genel Bakış

Bu öğretici StorSimple cihaz Yöneticisi hizmeti oluşturma ve güncelleştirme 3 ve sonraki sürümleri çalıştıran StorSimple 8000 serisi cihazlar birimlerde yönetmek için nasıl kullanılacağını açıklar.

StorSimple cihaz Yöneticisi hizmeti, StorSimple çözümünüzün bir tek web arabiriminden yönetmenizi sağlayan Azure portalında bir uzantısıdır. Tüm cihazlarınızda birimleri yönetmek için Azure portalını kullanın. Ayrıca oluşturun ve StorSimple hizmetleri yönetmek cihazları, yedekleme ilkeleri ve yedekleme kataloğu yönetmenize ve Uyarıları görüntüleyin.

## <a name="volume-types"></a>Birim türleri

StorSimple birimlerini olabilir:

* **Birimler'yerel olarak sabitlenmiş**: Bu birimlerdeki veriler her zaman yerel StorSimple cihazda kalır.
* **Katmanlı birimlerin**: Bu birimlerdeki verileri buluta sığdırmaya.

Katmanlı birim türü bir arşivleme birimdir. Arşivleme birimleri için kullanılan büyük yinelenenleri kaldırma öbek boyutu cihazın buluta büyük Segment veri aktarımı izin verir.

Gerekirse, birimi değiştirebileceğiniz ise yerel türünden katmanlı veya gelen çok yerel katmanlı. Daha fazla bilgi için Git [birim türünü değiştirme](#change-the-volume-type).

### <a name="locally-pinned-volumes"></a>Yerel olarak sabitlenmiş birimleri

Yerel olarak sabitlenmiş birimlerin değildir katman verileri buluta yapmak tamamen sağlanan birimlerin olduğundan, böylelikle yerel sağlama için birincil veri, bulut bağlantısını bağımsız garanti eder. Yerel olarak sabitlenmiş birimlerdeki veriler yinelenenleri kaldırılmış ve sıkıştırılmış değil; Ancak, yerel olarak sabitlenmiş birimlerin anlık görüntüleri yinelenenleri kaldırılmış. 

Yerel olarak sabitlenmiş birimlerin tam olarak sağlanır; Bu nedenle, bunları oluşturduğunuzda aygıtınızda yeterli alan olmalıdır. Yerel olarak sabitlenmiş birimlerin en büyük boyutunu StorSimple 8100 cihazda 8 TB ve 8600 aygıtta 20 TB'ye kadar hazırlayabilirsiniz. StorSimple anlık görüntüler, meta verileri ve veri işleme için cihazda kalan yerel alan ayırır. Yerel olarak sabitlenmiş bir birim için maksimum alan boyutunu artırabilirsiniz ancak oluşturulduktan sonra bir birimin boyutunu sayısından az olamaz.

Yerel olarak sabitlenmiş bir birim oluşturduğunuzda, katmanlı birimlerin oluşturması için kullanılabilir alan azalır. Bu durumun tersi de geçerlidir: var olan katmanlı birimler varsa, yerel olarak oluşturma sabitlenmiş birimleri için kullanılabilir alanı yukarıda belirtilen maksimum sınırları daha düşük olacaktır. Yerel birimlerde daha fazla bilgi için bkz [üzerinde yerel olarak sabitlenmiş birimlerin sık sorulan sorular](storsimple-8000-local-volume-faq.md).

### <a name="tiered-volumes"></a>Katmanlı birimleri

Katmanlı birimlerin sık erişilen veri cihazda yerel kalmasını ve daha az sık kullanılan veri otomatik olarak buluta katmanlı ölçülü kaynak kullanan birimler listelenmiştir. Ölçülü kaynak sağlama fiziksel kaynakları aşan kullanılabilir depolama alanı görünür bir sanallaştırma teknolojisidir. Önceden yeterli depolama alanı ayırma yerine, StorSimple ölçülü kaynak sağlama geçerli gereksinimlerini karşılamak için yeterli alan ayırmak için kullanır. StorSimple artırabilir veya değişen taleplerini karşılamak üzere bulut depolama azaltmak için bu yaklaşım bulut depolama esnek yapısını kolaylaştırır.

Arşiv verileri için katmanlı birim kullanıyorsanız seçin **bu birimi daha az sıklıkta erişilen arşiv verileri için kullanın** biriminiz yinelenenleri kaldırma öbek boyutunu 512 KB olarak değiştirmek için onay kutusunu. Bu seçeneği seçmezseniz, ilgili katmanlı birim 64 KB öbek boyutu kullanır. Daha büyük bir yinelenenleri kaldırma öbek boyutu cihazın büyük arşiv verilerinin buluta aktarımını hızlandırmasını sağlar.


### <a name="provisioned-capacity"></a>Sağlanan kapasite

Her cihaz ve birim türü için maksimum sağlanan kapasite için aşağıdaki tabloya bakın. (Yerel olarak sabitlenmiş birimleri sanal cihazda kullanılabilir olmadığını unutmayın.)

|  | En çok katmanlı birim boyutu | En fazla yerel olarak sabitlenmiş birim boyutu |
| --- | --- | --- |
| **Fiziksel cihazlar** | | |
| 8100 |64 TB |8 TB |
| 8600 |64 TB |20 TB |
| **Sanal cihazlar** | | |
| 8010 |30 TB |Yok |
| 8020 |64 TB |Yok |

## <a name="the-volumes-blade"></a>Birimleri dikey penceresi

**Birimleri** dikey Microsoft Azure StorSimple cihazı, başlatıcıları (Sunucuları) için sağlanan depolama birimleri yönetmenize olanak sağlar. Hizmetinize bağlı StorSimple cihazlarda birimlerin listesini görüntüler.

 ![Birimler sayfası](./media/storsimple-8000-manage-volumes-u2/volumeslist.png)

Bir birim öznitelikleri bir dizi oluşur:

* **Birim adı** – benzersiz olmalı ve birim tanımlamanıza yardımcı olan açıklayıcı bir ad. Bu ad Ayrıca belirli bir birimde filtre raporları izlemede kullanılır. Birim oluşturulduktan sonra adı değiştirilemez.
* **Durum** – çevrimiçi veya çevrimdışı olabilir. Bir birim çevrimdışıysa, birimi kullanmak erişim izni verilen başlatıcılarına (Sunucuları) görünür değil.
* **Kapasite** – toplam Başlatıcı (sunucu) tarafından depolanan veri miktarını belirtir. Yerel olarak sabitlenmiş birimleri tam olarak sağlanır ve StorSimple cihazında bulunur. Katmanlı birimlerin ölçülü kaynak ve veri yinelenenleri kaldırılmış. Ölçülü kaynak kullanan birimler ile Cihazınızı fiziksel depolama kapasitesi dahili olarak veya bulutta yapılandırılmış birim kapasitesi göre önceden tahsis değil. Birim kapasitesi ayrılmış ve isteğe bağlı olarak tüketilen.
* **Tür** – birim olup olmadığını belirten **katmanlı** (varsayılan) veya **yerel olarak sabitlenmiş**.

Aşağıdaki görevleri gerçekleştirmek için Bu öğreticide yönergeleri kullanın:

* Birim Ekle 
* Bir birim değiştirme 
* Birim türünü değiştirme
* Bir birim Sil 
* Bir birim çevrimdışı duruma getirin 
* Bir birimi izleyin 

## <a name="add-a-volume"></a>Birim Ekle

[Bir birim oluşturduğunuzda](storsimple-8000-deployment-walkthrough-u2.md#step-6-create-a-volume) StorSimple 8000 serisi cihazınızın dağıtım sırasında. Bir birim ekleme benzer bir yordamdır.

#### <a name="to-add-a-volume"></a>Bir birim eklemek için

1. **Cihazlar** dikey penceresindeki tablosal cihaz listesinden cihazınızı seçin. **+ Birim ekle**’ye tıklayın.

    ![Yeni birim ekleme](./media/storsimple-8000-manage-volumes-u2/step5createvol1.png)

2. **Birim ekle** dikey penceresinde:
   
    1. **Cihazı seçin** alanı otomatik olarak geçerli cihazınızla doldurulur.

    2. Açılan listeden, birim eklemeniz gereken birim kapsayıcısını seçin.

    3.  Biriminiz için bir **Ad** yazın. Birim oluşturulduktan sonra birim yeniden adlandırılamıyor.

    4. Açılan listede biriminizin **Tür**’ünü seçin. Yerel garantilerin, düşük gecikme sürelerinin ve yüksek performansın gerektiği iş yükleri için **Yerel olarak sabitlenmiş** bir birim seçin. Diğer tüm veriler için **Katmanlı** bir birim seçin. Bu birimi arşiv verileri için kullanıyorsanız **Bu birimi daha az sıklıkta erişilen arşiv verileri için kullanın**’ı işaretleyin.
      
       Katmanlı birim ölçülü sayıda kaynak kullanarak sağlanır ve hızlı oluşturulabilir. Arşiv verileri için hedeflenen katmanlı birime yönelik olarak **Bu birimi daha az sıklıkta erişilen arşiv verileri için kullan**’ın seçilmesi, biriminizle ilgili yinelenenleri kaldırma öbek boyutunu 512 KB olarak değiştirir. Bu alan işaretli değilse, ilgili katmanlı birim 64 KB öbek boyutu kullanır. Daha büyük bir yinelenenleri kaldırma öbek boyutu cihazın büyük arşiv verilerinin buluta aktarımını hızlandırmasını sağlar.
       
       Yerel olarak sabitlenmiş bir birim sıkı hazırlanmıştır ve birimdeki birincil verilerin cihazda yerel kalmasını ve buluta dağıtılmamasını sağlar.  Yerel olarak sabitlenmiş bir birim oluşturursanız cihaz, istenen boyutta birimi sağlamak için yerel katmanlardaki kullanılabilir alanı denetler. Yerel olarak sabitlenmiş birim oluşturma işlemi var olan verileri cihazdan buluta dağılmasını kapsayabilir ve birim oluşturmak için geçen süre uzun olabilir. Toplam süre hazırlanan birimin boyutuna, kullanılabilir ağ bant genişliğine ve cihazınızdaki verilere bağlıdır.

    5. Biriminiz için **Sağlanan Kapasite**’yi belirtin. Seçili birim türünü temel alan kullanılabilir kapasiteyi not edin. Belirtilen birim boyutu kullanılabilir alanı aşmamalıdır.
      
       8100 cihazında 8,5 TB'a kadar yerel olarak sabitlenmiş birim ya da 200 TB'a kadar katmanlı birim sağlayabilirsiniz. Daha büyük olan 8600 cihazında 22,5 TB'a kadar yerel olarak sabitlenmiş birim ya da 500 TB'a kadar katmanlı birim sağlayabilirsiniz. Katmanlı birimlerin çalışma kümesini barındırmak için cihazda yerel alan gerektiğinden, yerel olarak sabitlenmiş birimlerin oluşturulması, katmanlı birimlerin sağlanması için kullanılabilecek alanı etkiler. Bu nedenle, yerel olarak sabitlenmiş birim oluşturursanız, katmanlı birimlerin oluşturulması için gereken boş alan azalır. Benzer şekilde, katmanlı birim oluşturulduysa, yerel olarak sabitlenmiş birimlerin oluşturulması için kullanılabilir alan azalır.
      
       8100 cihazınızda 8,5 TB boyutunda (izin verilen en yüksek boyut) yerel olarak sabitlenmiş bir birim sağlarsanız, cihazdaki kullanılabilir yerel alanın tümünü kullanmış olursunuz. Katmanlı birimin çalışan kümesinin barındıracak cihazda yerel alan olmadığından, bu noktadan sonra herhangi bir katmanlı birim oluşturamazsınız. Var olan katmanlı birimler kullanılabilir alanı de etkiler. Örneğin, zaten 106 TB boyutunda katmanlı birimlerin bulunduğu bir 8100 cihazınız varsa, yerel olarak sabitlenmiş birimlerin kullanabileceği yalnızca 4 TB’lık alan kalır.

    6. **Bağlı konaklar** alanında oka tıklayın. İçinde **bağlı Konaklar** dikey penceresinde, varolan bir ACR seçin veya yeni bir ACR ekleyin. Yeni bir ACR seçerseniz, ardından tedarik bir **adı** , ACR için sağlayın **iSCSI Qualified Name** (IQN), Windows konağınızın. IQN’niz yoksa [Windows Server konağının IQN’sini al](#get-the-iqn-of-a-windows-server-host)’a gidin. **Oluştur**'a tıklayın. Belirtilen ayarlarla bir birim oluşturulur.

        ![Oluştur’a tıklayın](./media/storsimple-8000-manage-volumes-u2/step5createvol3.png)

Yeni birim artık kullanıma hazırdır.

> [!NOTE]
> Yerel olarak sabitlenmiş bir birim oluşturun ve sonra başka bir yerel olarak sabitlenmiş birimin hemen ardından, işler sırayla çalışır birim oluşturma oluşturmak istiyorsanız. Sonraki birim oluşturma işi başlamadan önce ilk birim oluşturma işi bitmesi gerekir.

## <a name="modify-a-volume"></a>Bir birim değiştirme

Bir birimi genişletin veya birime erişmek konakları değiştirmek gerektiğinde değiştirin.

> [!IMPORTANT]
> * Cihazdaki birim boyutunu değiştirirseniz, birim boyutu konakta değiştirilmesi gerekiyor.
> * Burada açıklanan konak tarafı Windows Server 2012 (2012R2) adımlardır. Yordamlar Linux veya diğer ana bilgisayar işletim sistemleri için farklı olacaktır. Başka bir işletim sistemi çalıştıran bir konak biriminde değiştirirken, ana bilgisayar işletim sistemi yönergelerine başvurun.

#### <a name="to-modify-a-volume"></a>Bir birim değiştirmek için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. Gelen tablo aygıtların listesi, değiştirmek istediğiniz biriminin cihazı seçin. Tıklatın **ayarlar > birimleri**.

    ![Birimleri dikey penceresine gidin](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

2. Birimleri Tablo listesi, birim seçin ve bağlam menüsünden çağırmak için sağ tıklatın. Seçin **çevrimdışına** , Değiştir birim çevrimdışı duruma getirmek için.

    ![Seçin ve birim çevrimdışı duruma getirin](./media/storsimple-8000-manage-volumes-u2/modifyvol4.png)

3. İçinde **çevrimdışına** dikey penceresinde, birim çevrimdışı duruma getirmeden etkisini gözden geçirin ve karşılık gelen onay kutusunu seçin. Ana bilgisayarda karşılık gelen birim ilk çevrimdışı olduğundan emin olun. Ana bilgisayar sunucunuz için StorSimple bağlı bir birim çevrimdışı yap hakkında daha fazla bilgi için işletim sistemi belirli yönergelerine başvurun. Tıklatın **çevrimdışına**.

    ![Birim çevrimdışı duruma getirmeden etkisini gözden geçirin](./media/storsimple-8000-manage-volumes-u2/modifyvol5.png)

4. Birim çevrimdışı (birim durumu ile gösterildiği gibi) sonra birim seçin ve bağlam menüsünden çağırmak için sağ tıklatın. Seçin **birim değiştirmek**.

    ![Birim seçin değiştirme](./media/storsimple-8000-manage-volumes-u2/modifyvol9.png)


5. İçinde **birim değiştirmek** dikey penceresinde, aşağıdaki değişiklikleri yapabilirsiniz:
   
   1. Birim **adı** düzenlenemez.
   2. Dönüştürme **türü** yerel olarak sabitlenmiş için katmanlı veya gelen için yerel olarak sabitlenmiş katmanlı (bkz [birim türünü değiştirme](#change-the-volume-type) daha fazla bilgi için).
   3. Artırmak **sağlanan kapasite**. **Sağlanan kapasite** yalnızca artırılabilir. Oluşturulduktan sonra bir birim küçültülemez.
   4. Altında **bağlı Konaklar**, ACR değiştirebilirsiniz. Bir ACR değiştirmek için birimin çevrimdışı olması gerekir.

       ![Birim çevrimdışı duruma getirmeden etkisini gözden geçirin](./media/storsimple-8000-manage-volumes-u2/modifyvol11.png)

5. Tıklatın **kaydetmek** yaptığınız değişiklikleri kaydetmek için. Onayınız istendiğinde **Evet**’e tıklayın. Azure portalında bir güncelleştirme birim iletisi görüntüler. Birim başarıyla güncelleştirildiğinde bir başarı iletisi görüntüler.

    ![Birim çevrimdışı duruma getirmeden etkisini gözden geçirin](./media/storsimple-8000-manage-volumes-u2/modifyvol5.png)

7. Bir birim genişletme, Windows ana bilgisayarınız için aşağıdaki adımları tamamlayın:
   
   1. Git **Bilgisayar Yönetimi** ->**Disk Yönetimi**.
   2. Sağ **Disk Yönetimi** seçip **diskleri**.
   3. Diskleri listesinde, güncelleştirilmiş bir birim, sağ tıklatın ve ardından seçin **birimi Genişlet**. Birim Genişletme Sihirbazı'nı başlatır. **İleri**’ye tıklayın.
   4. Varsayılan değerleri kabul ederek Sihirbazı tamamlayın. Sihirbaz tamamlandıktan sonra birimin boyutunun artmasına göstermelidir.
      
      > [!NOTE]
      > Yerel olarak sabitlenmiş bir birim genişletin ve ardından yerel olarak başka bir birimi, Birim genişletme işler sırayla çalıştırmanız hemen sabitlenmiş. Sonraki Birim genişletme işi başlamadan önce ilk birim genişletme işi bitmesi gerekir.
      

## <a name="change-the-volume-type"></a>Birim türünü değiştirme

Yerel olarak sabitlenmiş için katmanlı birim türünden değiştirebilir veya gelen yerel olarak çok katmanlı sabitlenir. Ancak, bu dönüştürme sık oluşması olmamalıdır.

### <a name="tiered-to-local-volume-conversion-considerations"></a>Yerel birim dönüştürme konuları için katmanlı

Yerel olarak sabitlenmiş katmanlı bir birimden dönüştürmek için bazı nedenler şunlardır:

* Yerel GARANTİLERİN veri kullanılabilirliği ve performansı ile ilgili
* Bulut gecikmeleri ve bulut bağlantı sorunları ortadan kaldırılması.

Genellikle, bunlar küçük sık erişmek istediğiniz var olan birimler. Yerel olarak sabitlenmiş bir birim oluşturulduğunda tam olarak sağlanır. 

Yerel olarak sabitlenmiş bir birim için katmanlı birim dönüştürüyorsanız StorSimple dönüştürme başlamadan önce Cihazınızda yeterli alana sahip doğrular. Yeterli alanınız yoksa, bir hata alırsınız ve işlemi iptal edilecek. 

> [!NOTE]
> Yerel olarak sabitlenmiş katmanlı dönüştürme başlamadan önce diğer iş yüklerinin alanı gereksinimlerini göz önünde bulundurduğunuzdan emin olun. 

Bir katmanlı dönüştürme yerel olarak sabitlenmiş bir birim için cihaz performansı olumsuz etkileyebilir. Ayrıca, aşağıdaki etmenleri dönüştürme işlemini tamamlamak için geçen süre artabilir:

* Yeterli bant genişliği yoktur.
* Geçerli bir yedekleme yoktur.

Bu etkenler olabilir etkileri en aza indirmek için:

* İlkeleri azaltma bant genişliğinizi gözden geçirin ve ayrılmış 40 MB/sn bant genişliği kullanılabilir olduğundan emin olun.
* Dönüştürme yoğun olmayan saatler için zamanlayın.
* Dönüştürme işlemine başlamadan önce bir bulut anlık görüntüsünü alın.

(Farklı iş yükleri destekleme) birden çok birim dönüştürüyorsanız, böylece daha yüksek öncelik birimleri ilk dönüştürülür birim dönüştürme önceliğini. Örneğin, dosya paylaşımı iş yükleri birimlerle dönüştürmeden önce sanal makineleri (VM'ler) barındırmak birimlerini veya birimleri SQL iş yükleri ile dönüştürmeniz gerekir.

### <a name="local-to-tiered-volume-conversion-considerations"></a>Katmanlı birim dönüştürme konuları yerel

Diğer birimleri sağlamak için ek alan gerekiyorsa yerel olarak sabitlenmiş bir birim için katmanlı birim değiştirmek isteyebilirsiniz. Katmanlı, yayımlanan kapasite boyutunu aygıt artar biriminde kullanılabilir kapasite yerel olarak sabitlenmiş birimin dönüştürürken. Bağlantı sorunları olan bir birimin yerel türünden katmanlı türüne dönüştürme engelliyorsa, dönüştürme işlemi tamamlanana kadar yerel birim katmanlı birim özelliklerini sergiler. Bazı verileri buluta geçmiş olmasıdır. Bu spilled verileri işlemi tamamlandı ve yeniden kadar serbest bırakılamıyor cihazda yerel alan kaplar devam eder.

> [!NOTE]
> Bir birim dönüştürme biraz zaman alabilir ve Dönüştürme başladıktan sonra iptal edemezsiniz. Birim dönüştürme sırasında çevrimiçi kalır ve yedek almadan ancak genişletmek veya birimi dönüştürme işlemi gerçekleşirken geri yükleyin.


#### <a name="to-change-the-volume-type"></a>Birim türünü değiştirmek için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. Gelen tablo aygıtların listesi, değiştirmek istediğiniz biriminin cihazı seçin. Tıklatın **ayarlar > birimleri**.

    ![Birimleri dikey penceresine gidin](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

3. Birimleri Tablo listesi, birim seçin ve bağlam menüsünden çağırmak için sağ tıklatın. Seçin **değiştirmek**.

    ![Bağlam menüsünden seçin değiştirme](./media/storsimple-8000-manage-volumes-u2/changevoltype2.png)

4. Üzerinde **birim değiştirme** dikey penceresinde, yeni türünden seçerek birim türünü değiştirme **türü** aşağı açılan liste.
   
   * Türüne değiştiriyorsanız **yerel olarak sabitlenmiş**, StorSimple yeterli kapasitesi olup olmadığını denetler.
   * Türüne değiştiriyorsanız **katmanlı** ve bu birimi arşiv verileri için select kullanılacak **bu birimi daha az sıklıkta erişilen arşiv verileri için kullanın** onay kutusu.
   * Yerel olarak sabitlenmiş bir birim katmanlı gibi yapılandırıyorsanız veya _tam tersini_, aşağıdaki ileti görüntülenir.
   
    ![Değişiklik birim türü iletisi](./media/storsimple-8000-manage-volumes-u2/changevoltype3.png)

7. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın. Onayınız istendiğinde tıklatın **Evet** dönüştürme işlemini başlatmak üzere. 

    ![Kaydet ve onaylayın](./media/storsimple-8000-manage-volumes-u2/modifyvol11.png)

8. Azure portalı birimi güncelleştirmek işi oluşturmak için bir bildirim görüntüler. Birim dönüştürme işinin durumunu izlemek için bildirime tıklayın.

    ![İş için toplu dönüştürme](./media/storsimple-8000-manage-volumes-u2/changevoltype5.png)

## <a name="take-a-volume-offline"></a>Bir birim çevrimdışı duruma getirin

Değiştirmek veya birim silmek planlama yaparken bir birim çevrimdışına gerekebilir. Bir birim çevrimdışı olduğunda okuma-yazma erişimi için kullanılabilir değildir. Birimde çevrimdışı ana bilgisayar ve aygıt almanız gerekir.

#### <a name="to-take-a-volume-offline"></a>Bir birim çevrimdışına almak için

1. Söz konusu birim çevrimdışı duruma getirmeden önce kullanımda olmadığından emin olun.
2. Birimin çevrimdışı konakta ilk alın. Bu, olası tüm birimdeki veri bozulması riskini ortadan kaldırır. Belirli adımlar için işletim sistemi için yönergelerine bakın.
3. Ana bilgisayar çevrimdışıysa, birim çevrimdışı cihazda aşağıdaki adımları gerçekleştirerek başlatıldıktan sonra:
   
    1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. Gelen tablo aygıtların listesi, değiştirmek istediğiniz biriminin cihazı seçin. Tıklatın **ayarlar > birimleri**.

        ![Birimleri dikey penceresine gidin](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

    2. Birimleri Tablo listesi, birim seçin ve bağlam menüsünden çağırmak için sağ tıklatın. Seçin **çevrimdışına** , Değiştir birim çevrimdışı duruma getirmek için.

        ![Seçin ve birim çevrimdışı duruma getirin](./media/storsimple-8000-manage-volumes-u2/modifyvol4.png)

3. İçinde **çevrimdışına** dikey penceresinde, birim çevrimdışı duruma getirmeden etkisini gözden geçirin ve karşılık gelen onay kutusunu seçin. Tıklatın **çevrimdışına**. 

    ![Birim çevrimdışı duruma getirmeden etkisini gözden geçirin](./media/storsimple-8000-manage-volumes-u2/modifyvol5.png)
      
      Birim çevrimdışı olduğunda size bildirilir. Birim durumu Ayrıca çevrimdışı güncelleştirir.
      
4. Bir birimi sağ tıklatın ve birim seçerseniz çevrimdışı olduğunda **çevrimiçine** seçeneği bağlam menüsünde kullanılabilir olur.

> [!NOTE]
> **Çevrimdışına Al** komutu, birim çevrimdışı duruma getirmek için aygıta bir istek gönderir. Konak birimi kullanmaya devam ediyorsanız bu bozuk bağlantıları olur ancak birim çevrimdışı duruma getirmeden değil başarısız olur.

## <a name="delete-a-volume"></a>Bir birim Sil

> [!IMPORTANT]
> Yalnızca çevrimdışı ise, bir birim silebilirsiniz.

Bir birimi silmek için aşağıdaki adımları tamamlayın.

#### <a name="to-delete-a-volume"></a>Bir birimi silmek için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. Gelen tablo aygıtların listesi, değiştirmek istediğiniz biriminin cihazı seçin. Tıklatın **ayarlar > birimleri**.

    ![Birimleri dikey penceresine gidin](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

3. Silmek istediğiniz birime durumunu denetleyin. Silmek istediğiniz birime çevrimdışı durumda değilse, onu önce çevrimdışına. Adımları [bir birim çevrimdışına](#take-a-volume-offline).
4. Çevrimdışı bir birimdir sonra birimi seçin, bağlam menüsü çağırma ve ardından sağ **silmek**.

    ![Bağlam menüsünden seçin Sil](./media/storsimple-8000-manage-volumes-u2/deletevol1.png)

5. İçinde **silmek** dikey penceresinde, gözden geçirin ve bir birim silindiğinde etkisini karşı onay kutusunu seçin. Bir birimi sildiğinizde, birimde bulunan tüm veriler kaybolur. 

    ![Kaydet ve değişiklikleri onaylayın](./media/storsimple-8000-manage-volumes-u2/deletevol2.png)

6. Birim silindikten sonra silme belirtmek için tablo birimlerin listesini güncelleştirir.

    ![Güncelleştirilmiş birim listesi](./media/storsimple-8000-manage-volumes-u2/deletevol3.png)
   
   > [!NOTE]
   > Yerel olarak sabitlenmiş bir birim silerseniz, yeni birimleri için kullanılabilir alanı hemen güncelleştirilmemiş. StorSimple cihaz Yöneticisi hizmeti, kullanılabilir yerel alan düzenli olarak güncelleştirir. Yeni birim oluşturmak denemeden önce birkaç dakika bekleyin öneririz.
   >
   > Yerel olarak sabitlenmiş bir birim silin ve ardından başka bir yerel olarak silerseniz ek olarak, birim hemen ardından, işler sırayla çalışır toplu silme sabitlenmiş. Sonraki toplu silme işi başlamadan önce ilk toplu silme işi bitmesi gerekir.

## <a name="monitor-a-volume"></a>Bir birimi izleyin

Birim izleme bir birim için g/Ç ile ilgili istatistikleri toplamanızı sağlar. İzleme, oluşturduğunuz ilk 32 birimleri için varsayılan olarak etkinleştirilir. Ek birimlerin izleme varsayılan olarak devre dışıdır. 

> [!NOTE]
> Kopyalanan birimlerin izleme varsayılan olarak devre dışıdır.


Etkinleştirmek veya bir birim için izlemeyi devre dışı bırakmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-enable-or-disable-volume-monitoring"></a>Etkinleştirmek veya toplu izleme devre dışı bırakmak için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. Gelen tablo aygıtların listesi, değiştirmek istediğiniz biriminin cihazı seçin. Tıklatın **ayarlar > birimleri**.
2. Birimleri Tablo listesi, birim seçin ve bağlam menüsünden çağırmak için sağ tıklatın. Seçin **değiştirmek**.
3. İçinde **birim değiştirme** dikey penceresinde için **izleme** seçin **etkinleştirmek** veya **devre dışı** etkinleştirmek veya izleme devre dışı bırakmak için.

    ![İzleme devre dışı bırak](./media/storsimple-8000-manage-volumes-u2/monitorvol1.png) 

4. Tıklatın **kaydetmek** tıklatıp onaylamanız istendiğinde, **Evet**. Azure portalı birimi başarıyla güncelleştirildikten sonra birim ve ardından bir başarı iletisi güncelleştirmek için bir bildirim görüntüler.

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [bir StorSimple biriminin kopyalama](storsimple-8000-clone-volume-u2.md).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

