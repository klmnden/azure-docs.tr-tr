---
title: StorSimple birimlerini (Aktualizace 3) yönetme | Microsoft Docs
description: Ekleme, değiştirme, izleme ve StorSimple birimleri silin ve bunları gerekirse çevrimdışına almak nasıl açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/08/2017
ms.author: alkohli
ms.openlocfilehash: f7bfe41b4cdc9989c6b949011bc240275886b6f0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60634903"
---
# <a name="use-the-storsimple-device-manager-service-to-manage-volumes-update-3-or-later"></a>Birimler (güncelleştirme 3 veya üzeri) yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma

## <a name="overview"></a>Genel Bakış

Bu öğreticide StorSimple cihaz Yöneticisi hizmeti oluşturup güncelleştirme 3 ve üzerini çalıştıran StorSimple 8000 serisi cihazlar birimlerde yönetmek için nasıl kullanılacağını açıklar.

StorSimple cihaz Yöneticisi hizmeti, StorSimple çözümünüzü tek bir web arabiriminden yönetmenizi sağlayan Azure portalında bir uzantısıdır. Tüm cihazlarınızı birimlerde yönetmek için Azure portalını kullanın. Ayrıca oluşturabilir ve StorSimple Hizmetleri yönetin cihazları, yedekleme ilkelerini ve yedekleme kataloğunu yönetme ve Uyarıları görüntüleyin.

## <a name="volume-types"></a>Birim türü

StorSimple birimlerini olabilir:

* **Birimleri'yerel olarak sabitlenmiş**: Bu birimlerdeki verileri her zaman yerel StorSimple cihazında kalır.
* **Katmanlı birimlerin**: Bu birimlerdeki verileri buluta sığdırmaya.

Bir arşiv birim katmanlı birimin türüdür. Arşiv birimleri için kullanılan büyük yinelenenleri kaldırma öbek boyutu cihazın büyük veri parçalarını buluta aktararak izin verir.

Gerekirse, birimi değiştirebileceğiniz, yerel türden katmanlı veya öğesinden yerel katmanlı. Daha fazla bilgi için Git [birim türünü değiştirmek](#change-the-volume-type).

### <a name="locally-pinned-volumes"></a>Yerel olarak sabitlenmiş birimler

Yerel olarak sabitlenmiş birimler değil katman verileri buluta yapmak tam olarak sağlanan birimler olan, böylece yerel sağlama için birincil veri, bulut bağlantısı bağımsız garanti eder. Verileri yerel olarak sabitlenmiş birim üzerinde yinelenenleri kaldırılmış ve sıkıştırılmış değil; Ancak, yerel olarak sabitlenmiş birimlerin anlık görüntüleri kaldırılma. 

Yerel olarak sabitlenmiş birimler tam olarak sağlanır; Bu nedenle, bunları oluşturduğunuz Cihazınızda yeterli alan olmalıdır. En büyük boyutunu StorSimple 8100 cihazında 8 TB ve 8600 model Cihazınızı 20 TB'a kadar yerel olarak sabitlenmiş birimleri sağlayabilirsiniz. StorSimple anlık görüntü meta verileri ve veri işleme için cihazda yerel kalan alanı ayırır. Yerel olarak sabitlenmiş bir birim için maksimum alan boyutunu artırabilir, ancak oluşturulduktan sonra bir birimin boyutuna sayısından az olamaz.

Yerel olarak sabitlenmiş bir birim oluşturduğunuzda, katmanlı birimlerin oluşturulması için kullanılabilir alan azalır. Bu durumun tersi de geçerlidir: var olan katmanlı birimler varsa, oluşturma yerel olarak sabitlenmiş birimler için mevcut olan alanı yukarıda belirtilen en üst düzeye göre daha düşük olacaktır. Yerel birimler üzerinde daha fazla bilgi için [yerel olarak sabitlenmiş birimler hakkında sık sorulan sorular](storsimple-8000-local-volume-faq.md).

### <a name="tiered-volumes"></a>Katmanlı birim

Katmanlı birimlerin sık erişilen verilerin cihazda yerel kalmasını ve sık kullanılan veriler otomatik olarak buluta katmanlanmış ölçülü kaynak kullanan birimler listelenmiştir. Ölçülü kaynak sağlama fiziksel kaynakları aşmayı kullanılabilir depolama alanı görünür bir sanallaştırma teknolojisidir. Önceden yeterli depolama alanı ayırmak yerine, StorSimple ölçülü kaynak sağlama geçerli gereksinimlerini karşılamak için yeterli alan ayırmak için kullanır. StorSimple artırabilir veya azaltabilirsiniz değişen taleplerini karşılamak üzere bulut depolama çünkü bu yaklaşım bulut depolama esnek yapısı kolaylaştırır.

Arşiv verileri için katmanlı birim kullanıyorsanız, **bu birimi daha az sıklıkta erişilen arşiv verileri için kullanın** biriminiz için yinelenenleri kaldırma öbek boyutunu 512 KB olarak değiştirmek için onay kutusunu. Bu seçeneği belirlemezseniz, ilgili katmanlı birim 64 KB öbek boyutu kullanır. Daha büyük bir yinelenenleri kaldırma öbek boyutu cihazın büyük arşiv verilerinin buluta aktarımını hızlandırmasını sağlar.


### <a name="provisioned-capacity"></a>Sağlanan kapasite

Her bir cihaz ve birim türü için sağlanan kapasite üst sınırı için aşağıdaki tabloya bakın. (Yerel olarak sabitlenmiş birimler, sanal bir cihazda kullanılabilir olmadığını unutmayın.)

|  | En çok katmanlı birim boyutu | En fazla yerel olarak sabitlenmiş birim boyutu |
| --- | --- | --- |
| **Fiziksel cihazlar** | | |
| 8100 |64 TB |8 TB |
| 8600 |64 TB |20 TB |
| **Sanal cihazlar** | | |
| 8010 |30 TB |Yok |
| 8020 |64 TB |Yok |

## <a name="the-volumes-blade"></a>Birimleri dikey penceresi

**Birimleri** dikey penceresinde, başlatıcıları (sunucu) için Microsoft Azure StorSimple cihazında sağlanan depolama birimleri yönetme olanak tanır. Hizmetinize bağlı StorSimple cihazlarda birimlerin listesini görüntüler.

 ![Birimler sayfası](./media/storsimple-8000-manage-volumes-u2/volumeslist.png)

Birim öznitelikleri bir dizi oluşur:

* **Birim adı** – benzersiz olmalıdır ve birim belirlemeye yardımcı olur ve açıklayıcı bir ad. Bu ad, belirli bir birimde filtrelerken raporları izleme de kullanılır. Birim oluşturulduktan sonra adı değiştirilemez.
* **Durum** – çevrimiçi veya çevrimdışı olabilir. Bir birimi çevrimdışı ise, birim kullanmak erişim izni verilen başlatıcılara (Sunucuları) görünür değil.
* **Kapasite** – (sunucu) başlatıcı tarafından depolanan verilerin toplam miktarı belirtir. Yerel olarak sabitlenmiş birimler, tam olarak sağlanır ve StorSimple cihazında bulunur. Katmanlı birim ölçülü ve verileri yinelenen verileri kaldırma işlemi. Ölçülü kaynak kullanan birimler ile Cihazınızı fiziksel depolama kapasitesi dahili olarak veya bulutta yapılandırılmış birim kapasitesi göre önceden tahsis etmez. Birim kapasitesi ayrılan ve isteğe bağlı olarak kullanılan.
* **Tür** – birimin olup olmadığını belirten **katmanlı** (varsayılan) veya **yerel olarak sabitlenmiş**.

Yönergeleri Bu öğreticide aşağıdaki görevleri gerçekleştirmek için kullanın:

* Birim Ekle 
* Bir birimi Değiştir 
* Birim türünü değiştirme
* Birim Sil 
* Bir birimi çevrimdışı duruma getirin 
* Bir birimi izleyin 

## <a name="add-a-volume"></a>Birim Ekle

[Bir birim oluşturduğunuzda](storsimple-8000-deployment-walkthrough-u2.md#step-6-create-a-volume) StorSimple 8000 serisi Cihazınızı dağıtımı sırasında. Birim ekleme benzer bir yordamdır.

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

    6. **Bağlı konaklar** alanında oka tıklayın. İçinde **bağlı Konaklar** dikey penceresinde, mevcut bir ACR'yi seçin veya yeni ACR ekleyin. Yeni ACR seçerseniz, sağlamanız, sonra bir **adı** sağlamak için ACR, **iSCSI Qualified ad** (IQN), Windows ana bilgisayarınız. IQN'niz yoksa, bir Windows Server konağının IQN'ini alın gidin. **Oluştur**’a tıklayın. Belirtilen ayarlarla bir birim oluşturulur.

        ![Oluştur’a tıklayın](./media/storsimple-8000-manage-volumes-u2/step5createvol3.png)

Yeni toplu kullanmak artık hazırdır.

> [!NOTE]
> Varsa yerel olarak sabitlenmiş birim oluşturmak ve ardından başka bir yerel olarak sabitlenmiş birim hemen ardından sırayla işlerinizi birim oluşturma oluşturun. Sonraki birim oluşturma işi başlayabilmeniz için önce ilk birim oluşturma işi bitmesi gerekir.

## <a name="modify-a-volume"></a>Bir birimi Değiştir

Bir birimi genişletmek veya birim erişimi olan konakları değiştirmek gerektiğinde değiştirin.

> [!IMPORTANT]
> * Cihazdaki birim boyutunu değiştirirseniz, birim boyutu konakta değiştirilmesi gerekir.
> * Burada açıklanan konak tarafı (2012 R2) Windows Server 2012 için aynıdır. Linux veya diğer ana bilgisayar işletim sistemleri için yordamlar farklı olacaktır. Birimin başka bir işletim sistemi çalıştıran bir konağa değiştirirken, konak işletim sistemi yönergelerine başvurun.

#### <a name="to-modify-a-volume"></a>Bir birimi değiştirme

1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. Cihazların tablosal listesinde gelen değiştirmek için istediğinize biriminde cihazı seçin. Tıklayın **Ayarları > birimleri**.

    ![Birimleri dikey penceresine gidin](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

2. Birimlerin tablosal listesinde, bir birim seçin ve bağlam menüsünü açmak için sağ tıklayın. Seçin **çevrimdışına** değiştirir, birimi çevrimdışı duruma getirmek.

    ![Seçin ve bu birimi çevrimdışı duruma](./media/storsimple-8000-manage-volumes-u2/modifyvol4.png)

3. İçinde **çevrimdışına** dikey penceresinde birimi çevrimdışı duruma getirmenin etkilerini gözden geçirin ve karşılık gelen onay kutusunu işaretleyin. Ana bilgisayarda karşılık gelen birim önce çevrimdışı olduğundan emin olun. StorSimple için bağlı olarak, ana bilgisayar sunucusunda bir birimi çevrimdışı nasıl hakkında daha fazla bilgi için işletim sistemi belirli yönergelerine başvurun. Tıklayın **çevrimdışına**.

    ![Birimi çevrimdışı duruma getirmenin etkilerini gözden geçirin](./media/storsimple-8000-manage-volumes-u2/modifyvol5.png)

4. Birimi çevrimdışı (birim durumu ile gösterildiği gibi) sonra birim seçin ve bağlam menüsünü açmak için sağ tıklayın. Seçin **birimi değiştirme**.

    ![Birimi Değiştir'i seçin](./media/storsimple-8000-manage-volumes-u2/modifyvol9.png)


5. İçinde **birimi değiştirme** dikey penceresinde, aşağıdaki değişiklikleri yapabilirsiniz:
   
   1. Birim **adı** düzenlenemez.
   2. Dönüştürme **türü** yerel olarak sabitlenmiş için katmanlı veya gelen için yerel olarak sabitlenmiş katmanlı (bkz [birim türünü değiştirmek](#change-the-volume-type) daha fazla bilgi için).
   3. Artırmak **sağlanan kapasite**. **Sağlanan kapasite** yalnızca artırılabilir. Oluşturulduktan sonra bir birim küçültülemez.
   4. Altında **bağlı Konaklar**, ACR değiştirebilirsiniz. Bir ACR'yi değiştirmek için birimi çevrimdışı olması gerekir.

       ![Birimi çevrimdışı duruma getirmenin etkilerini gözden geçirin](./media/storsimple-8000-manage-volumes-u2/modifyvol11.png)

5. Tıklayın **Kaydet** yaptığınız değişiklikleri kaydedin. Onayınız istendiğinde **Evet**’e tıklayın. Azure portalında bir güncelleştirme birim iletisi görüntüler. Birimi başarıyla güncelleştirildiğinde bir başarı iletisi görüntüler.

    ![Birimi çevrimdışı duruma getirmenin etkilerini gözden geçirin](./media/storsimple-8000-manage-volumes-u2/modifyvol5.png)

7. Bir birim genişlettiğiniz, Windows ana bilgisayarınız için aşağıdaki adımları tamamlayın:
   
   1. Git **Bilgisayar Yönetimi** ->**Disk Yönetimi**.
   2. Sağ **Disk Yönetimi** seçip **diskleri**.
   3. Güncelleştirdiğiniz birim, sağ tıklayın ve ardından diskleri listesinde seçin **birimi Genişlet**. Birim Genişletme Sihirbazı'nı başlatır. **İleri**’ye tıklayın.
   4. Varsayılan değerleri kabul Sihirbazı tamamlayın. Sihirbaz tamamlandıktan sonra birim artan boyutu göstermelidir.
      
      > [!NOTE]
      > Yerel olarak sabitlenmiş bir birim genişletin ve ardından yerel olarak başka bir toplu hemen ardından, Birim genişletme işler sırayla çalışır sabitlendi. İlk Birim genişletme işlemi sonraki toplu genişletme işi başlayabilmeniz için önce tamamlanmalıdır.
      

## <a name="change-the-volume-type"></a>Birim türünü değiştirme

Yerel olarak sabitlenmiş için katmanlı birim türünü değiştirebilirsiniz veya öğesinden yerel olarak çok katmanlı sabitlenebilir. Ancak, bu dönüştürme sık oluşum olmamalıdır.

### <a name="tiered-to-local-volume-conversion-considerations"></a>Yerel birim dönüştürme değerlendirmeleri için katmanlı

Yerel olarak sabitlenmiş için katmanlı bir birimden dönüştürmek için bazı nedenler şunlardır:

* Yerel GARANTİLERİN veri kullanılabilirliği ve performansı
* Bulut gecikme süreleri ve bulut bağlantı sorunları ortadan kaldırılması.

Genellikle, küçük bunlar sık erişmek istediğiniz var olan birimler. Yerel olarak sabitlenmiş bir birim, oluşturulduğu sırada tam olarak sağlanır. 

Yerel olarak sabitlenmiş bir birim için katmanlı birim dönüştürüyorsanız, StorSimple dönüştürme başlatılmadan önce Cihazınızda yeterli alana sahip doğrular. Yetersiz alan varsa, hata alırsınız ve işlemi iptal edilecek. 

> [!NOTE]
> Yerel olarak sabitlenmiş için katmanlı dönüştürmeye başlamadan önce diğer iş yüklerinin alanı gereksinimleri göz önünde bulundurduğunuzdan emin olun. 

Katmanlı bir dönüştürme yerel olarak sabitlenmiş bir birim için cihaz performansı olumsuz etkileyebilir. Ayrıca, aşağıdaki etmenlere dönüştürme işlemini tamamlamak için gereken süreyi artırabilir:

* Yeterli bant genişliği var.
* Geçerli yedek yok.

Bu etkenler olabilir etkileri en aza indirmek için:

* Azaltma ilkeleri bant genişliğinizi gözden geçirin ve ayrılmış 40 MB/sn bant genişliği bulunduğundan emin olun.
* Dönüştürme yoğun olmayan saatlere zamanlama.
* Dönüştürme işlemine başlamadan önce bir bulut anlık görüntüsünü alın.

Birden çok birim (farklı iş yüklerini destekleme) dönüştürüyorsanız, böylece daha yüksek öncelikli birimleri ilk dönüştürülür birim dönüştürme öncelik vermelisiniz. Örneğin, dosya paylaşımı iş yüklerine sahip birimler dönüştürmeden önce sanal makineleri (VM'ler) barındıran birimler ya da SQL iş yükleri birimlerle dönüştürmeniz gerekir.

### <a name="local-to-tiered-volume-conversion-considerations"></a>Katmanlı birim dönüştürme değerlendirmeleri için yerel

Diğer birimleri sağlamak için ek alana ihtiyacınız olursa, yerel olarak sabitlenmiş bir birim için katmanlı birim değiştirmek isteyebilirsiniz. Yerel olarak sabitlenmiş birimin katmanlı için kullanılabilir kapasiteye yayımlanan kapasite boyutunu cihaz artar dönüştürürken. Bağlantı sorunları birimin yerel türden katmanlı türüne dönüştürme engelliyorsa, dönüştürme işlemi tamamlanana kadar yerel birim katmanlı birimin özelliklerini sergiler. Bazı verilere buluta geçmiş olmasıdır. Bu spilled veri işlemi tamamlandı ve yeniden kadar serbest bırakılamıyor cihazda yerel alan kaplaması devam eder.

> [!NOTE]
> Bir birim dönüştürme biraz zaman alabilir ve Dönüştürme başladıktan sonra iptal edilemez. Birim dönüştürme sırasında çevrimiçi kalır ve yedek almadan, ancak genişletin veya dönüştürme gerçekleşirken birimi geri yükleyin.


#### <a name="to-change-the-volume-type"></a>Birim türünü değiştirmek için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. Cihazların tablosal listesinde gelen değiştirmek için istediğinize biriminde cihazı seçin. Tıklayın **Ayarları > birimleri**.

    ![Birimleri dikey penceresine gidin](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

3. Birimlerin tablosal listesinde, bir birim seçin ve bağlam menüsünü açmak için sağ tıklayın. Seçin **değiştirme**.

    ![Bağlam menüsünden Değiştir'i seçin](./media/storsimple-8000-manage-volumes-u2/changevoltype2.png)

4. Üzerinde **birimi değiştirme** dikey penceresinde yeni türünden seçerek birim türünü değiştirmek **türü** aşağı açılan listesi.
   
   * Türüne değiştiriyorsanız **yerel olarak sabitlenmiş**, StorSimple yeterli kapasitesi olup olmadığını denetler.
   * Türüne değiştiriyorsanız **katmanlı** ve bu birimi arşiv verileri için seçim kullanılacak **bu birimi daha az sıklıkta erişilen arşiv verileri için kullanın** onay kutusu.
   * Yerel olarak sabitlenmiş bir birim katmanlı olarak yapılandırıyorsanız veya _tersi_, aşağıdaki ileti görüntülenir.
   
     ![Değişiklik birim türü iletisi](./media/storsimple-8000-manage-volumes-u2/changevoltype3.png)

7. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın. Onayınız istendiğinde tıklayın **Evet** dönüştürme işlemini başlatmak için. 

    ![Kaydet ve onaylayın](./media/storsimple-8000-manage-volumes-u2/modifyvol11.png)

8. Azure portalı, birim güncelleştirilecek iş oluşturma için bir bildirim görüntülenir. Birim dönüştürme işi durumunu izlemek için bildirime tıklayın.

    ![İş için birim dönüştürme](./media/storsimple-8000-manage-volumes-u2/changevoltype5.png)

## <a name="take-a-volume-offline"></a>Bir birimi çevrimdışı duruma getirin

Değiştirmek veya birimi silmek planlama yaparken bir birimi çevrimdışı duruma gerekebilir. Bir birimi çevrimdışı olduğunda, okuma-yazma erişimi için kullanılabilir değil. Birim ana bilgisayar ve cihaz çevrimdışı olması gerekir.

#### <a name="to-take-a-volume-offline"></a>Bir birimi çevrimdışı duruma getirmek için

1. Söz konusu birimi çevrimdışı duruma getirmeden önce kullanımda olmadığından emin olun.
2. Birimi çevrimdışı konakta ilk yararlanın. Bu, olası tüm birim üzerindeki veri bozulması riskini ortadan kaldırır. Belirli adımlar için ana bilgisayar işletim sisteminiz için yönergelerine bakın.
3. Ana bilgisayar çevrimdışı olduğunda, birimi aşağıdaki adımları uygulayarak cihazda çevrimdışı duruma:
   
    1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. Cihazların tablosal listesinde gelen değiştirmek için istediğinize biriminde cihazı seçin. Tıklayın **Ayarları > birimleri**.

        ![Birimleri dikey penceresine gidin](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

    2. Birimlerin tablosal listesinde, bir birim seçin ve bağlam menüsünü açmak için sağ tıklayın. Seçin **çevrimdışına** değiştirir, birimi çevrimdışı duruma getirmek.

        ![Seçin ve bu birimi çevrimdışı duruma](./media/storsimple-8000-manage-volumes-u2/modifyvol4.png)

3. İçinde **çevrimdışına** dikey penceresinde birimi çevrimdışı duruma getirmenin etkilerini gözden geçirin ve karşılık gelen onay kutusunu işaretleyin. Tıklayın **çevrimdışına**. 

    ![Birimi çevrimdışı duruma getirmenin etkilerini gözden geçirin](./media/storsimple-8000-manage-volumes-u2/modifyvol5.png)
      
      Birimi çevrimdışı olduğunda size bildirilir. Birim durumu çevrimdışı de güncelleştirir.
      
4. Bir birime sağ tıklayın ve birim seçerseniz çevrimdışı duruma geldikten sonra **çevrimiçine** bağlam menüsü seçeneği kullanılabilir.

> [!NOTE]
> **Çevrimdışına Al** komut cihaza birimi çevrimdışı duruma getirmek için bir istek gönderir. Konak birimi kullanmaya devam ediyorsanız bu bozuk bağlantılar olur ancak birimi çevrimdışı duruma getirmenin değil başarısız olur.

## <a name="delete-a-volume"></a>Birim Sil

> [!IMPORTANT]
> Yalnızca çevrimdışı olduğunda, bir birim silebilirsiniz.

Bir birimi silmek için aşağıdaki adımları tamamlayın.

#### <a name="to-delete-a-volume"></a>Bir birimi silmek için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. Cihazların tablosal listesinde gelen değiştirmek için istediğinize biriminde cihazı seçin. Tıklayın **Ayarları > birimleri**.

    ![Birimleri dikey penceresine gidin](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

3. Silmek istediğiniz birim durumunu denetleyin. Silmek istediğiniz birim çevrimdışı değilse, önce çevrimdışına. Bağlantısındaki [bir birimi çevrimdışı duruma](#take-a-volume-offline).
4. Birimi çevrimdışı duruma geldikten sonra birimi seçin, bağlam menüsünü açmak ve ardından sağ **Sil**.

    ![Bağlam menüsünden seçim Sil](./media/storsimple-8000-manage-volumes-u2/deletevol1.png)

5. İçinde **Sil** dikey penceresinde gözden geçirin ve bir birimi silmenin etkilerini karşı onay kutusunu işaretleyin. Bir birimi sildiğinizde, birimde bulunan tüm veriler kaybolur. 

    ![Kaydet ve değişiklikleri onaylayın](./media/storsimple-8000-manage-volumes-u2/deletevol2.png)

6. Birim silindikten sonra silme işlemini göstermek için birimlerin tablosal listesini güncelleştirir.

    ![Güncelleştirilmiş birim listesi](./media/storsimple-8000-manage-volumes-u2/deletevol3.png)
   
   > [!NOTE]
   > Yerel olarak sabitlenmiş bir birim silerseniz, yeni birimler için mevcut olan alanı hemen güncelleştirilmeyebilir. StorSimple cihaz Yöneticisi hizmeti, kullanılabilir yerel alanın düzenli olarak güncelleştirir. Yeni birim oluşturmak denemeden önce birkaç dakika bekleyin öneririz.
   >
   > Yerel olarak sabitlenmiş bir birim silin ve ardından başka bir yerel olarak silerseniz ek olarak, birim hemen ardından sırayla işlerinizi toplu silme sabitlendi. İlk toplu silme işi sonraki toplu silme işi başlayabilmeniz için önce tamamlanmalıdır.

## <a name="monitor-a-volume"></a>Bir birimi izleyin

Birim izleme, bir birim için miyim GÇ ile ilgili istatistikleri toplamanıza olanak sağlar. İzleme, oluşturduğunuz ilk 32 birim için varsayılan olarak etkindir. Ek birimler izleme, varsayılan olarak devre dışıdır. 

> [!NOTE]
> Kopyalanan hacimlerdeki izleme varsayılan olarak devre dışıdır.


Birimi için izlemeyi devre dışı bırakmak veya etkinleştirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-enable-or-disable-volume-monitoring"></a>Etkinleştirme veya birimi izlemeyi devre dışı

1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. Cihazların tablosal listesinde gelen değiştirmek için istediğinize biriminde cihazı seçin. Tıklayın **Ayarları > birimleri**.
2. Birimlerin tablosal listesinde, bir birim seçin ve bağlam menüsünü açmak için sağ tıklayın. Seçin **değiştirme**.
3. İçinde **birimi değiştirme** dikey penceresinde için **izleme** seçin **etkinleştirme** veya **devre dışı** etkinleştirme veya devre dışı izleme.

    ![İzlemeyi devre dışı bırakma](./media/storsimple-8000-manage-volumes-u2/monitorvol1.png) 

4. Tıklayın **Kaydet** onaylamanız istendiğinde tıklayın **Evet**. Azure portalında birimi başarıyla güncelleştirildikten sonra birim ve başarılı iletisi, güncelleştirme için bir bildirim görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [bir StorSimple biriminin kopyalama](storsimple-8000-clone-volume-u2.md).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

