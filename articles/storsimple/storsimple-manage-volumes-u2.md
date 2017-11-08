---
title: "StorSimple birimlerinizi (U2) yönetme | Microsoft Docs"
description: "Ekleme, değiştirme, izlemek ve StorSimple birimleri silin ve bunları gerekirse çevrimdışına alma konusunda açıklanmaktadır."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 57896932-0aa5-4805-970c-d13403ae7551
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: db24d33249767b42f00d96081f21ca8ffb1ac985
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-volumes-update-2"></a>Birimler (güncelleştirme 2) yönetmek için StorSimple Yöneticisi hizmetini kullanma
> [!NOTE]
> StorSimple için Klasik portalı kullanım dışıdır. StorSimple cihaz yöneticileri yeni Azure portalına kullanımdan zamanlamaya göre otomatik olarak taşır. Bir e-posta ve bu taşıma için portal bir bildirim alırsınız. Bu belgede ayrıca yakında kullanımdan kaldırılacaktır. Bu makalede yeni Azure portalına için sürümünü görüntülemek için şu adrese gidin [birimler (güncelleştirme 2) yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-8000-manage-volumes-u2.md). Taşıma hakkında herhangi bir sorunuz için bkz: [SSS: Azure portalına taşıma](storsimple-8000-move-azure-portal-faq.md).

[!INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Genel Bakış
Bu öğretici oluşturmak ve güncelleştirme 2 yüklü birimler StorSimple cihazı ve StorSimple sanal cihazı yönetmek için StorSimple Yöneticisi hizmetini kullanmayı açıklar.

StorSimple Yöneticisi hizmetiniz, StorSimple çözümünüzün bir tek web arabiriminden yönetmenizi sağlayan Azure Klasik portalında bir uzantısıdır. Birimleri yönetmeye ek olarak, StorSimple Yöneticisi hizmeti oluşturmak ve StorSimple hizmetleri yönetmek, görüntülemek ve cihazları yönetmek, uyarıları görüntüleyin ve görüntülemek ve yedekleme ilkeleri ve yedekleme kataloğu yönetmek için kullanabilirsiniz.

## <a name="volume-types"></a>Birim türleri
StorSimple birimlerini olabilir:

* **Birimler'yerel olarak sabitlenmiş**: Bu birimlerdeki veriler her zaman yerel StorSimple cihazda kalır.
* **Katmanlı birimlerin**: Bu birimlerdeki verileri buluta sığdırmaya.

Katmanlı birim türü bir arşivleme birimdir. Arşivleme birimleri için kullanılan büyük yinelenenleri kaldırma öbek boyutu cihazın buluta büyük Segment veri aktarımı izin verir. 

Gerekirse, birimi değiştirebileceğiniz ise yerel türünden katmanlı veya gelen çok yerel katmanlı. Daha fazla bilgi için Git [birim türünü değiştirme](#change-the-volume-type).

### <a name="locally-pinned-volumes"></a>Yerel olarak sabitlenmiş birimleri
Yerel olarak sabitlenmiş birimlerin değildir katman verileri buluta yapmak tamamen sağlanan birimlerin olduğundan, böylelikle yerel sağlama için birincil veri, bulut bağlantısını bağımsız garanti eder. Yerel olarak sabitlenmiş birimlerdeki veriler yinelenenleri kaldırılmış ve sıkıştırılmış değil; Ancak, yerel olarak sabitlenmiş birimlerin anlık görüntüleri yinelenenleri kaldırılmış. 

Yerel olarak sabitlenmiş birimlerin tam olarak sağlanır; Bu nedenle, bunları oluşturduğunuzda aygıtınızda yeterli alan olmalıdır. Yerel olarak sabitlenmiş birimlerin en büyük boyutunu StorSimple 8100 cihazda 8 TB ve 8600 aygıtta 20 TB'ye kadar hazırlayabilirsiniz. StorSimple anlık görüntüler, meta verileri ve veri işleme için cihazda kalan yerel alan ayırır. Yerel olarak sabitlenmiş bir birim için maksimum alan boyutunu artırabilirsiniz ancak oluşturulduktan sonra bir birimin boyutunu sayısından az olamaz.

Yerel olarak sabitlenmiş bir birim oluşturduğunuzda, katmanlı birimlerin oluşturması için kullanılabilir alan azalır. Bu durumun tersi de geçerlidir: var olan katmanlı birimler varsa, yerel olarak oluşturma sabitlenmiş birimleri için kullanılabilir alanı yukarıda belirtilen maksimum sınırları daha düşük olacaktır. Yerel birimlerde daha fazla bilgi için bkz [üzerinde yerel olarak sabitlenmiş birimlerin sık sorulan sorular](storsimple-local-volume-faq.md).   

### <a name="tiered-volumes"></a>Katmanlı birimleri
Katmanlı birimlerin sık erişilen veri cihazda yerel kalmasını ve daha az sık kullanılan veri otomatik olarak buluta katmanlı ölçülü kaynak kullanan birimler listelenmiştir. Ölçülü kaynak sağlama fiziksel kaynakları aşan kullanılabilir depolama alanı görünür bir sanallaştırma teknolojisidir. Önceden yeterli depolama alanı ayırma yerine, StorSimple ölçülü kaynak sağlama geçerli gereksinimlerini karşılamak için yeterli alan ayırmak için kullanır. StorSimple artırabilir veya değişen taleplerini karşılamak üzere bulut depolama azaltmak için bu yaklaşım bulut depolama esnek yapısını kolaylaştırır.

Arşiv verileri için katmanlı birim kullanıyorsanız, seçme **bu birimi daha az sıklıkta erişilen arşiv verileri için kullanın** onay kutusunu biriminiz yinelenenleri kaldırma öbek boyutunu 512 KB olarak değiştirir. Bu seçeneği seçmezseniz, ilgili katmanlı birim 64 KB öbek boyutu kullanır. Daha büyük bir yinelenenleri kaldırma öbek boyutu cihazın büyük arşiv verilerinin buluta aktarımını hızlandırmasını sağlar.

> [!NOTE]
> Bir güncelleştirme öncesi 2 sürümü StorSimple ile oluşturulan arşivleme birimler arşivleme onay kutusu seçili katmanlı alınan olacaktır.
> 
> 

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

## <a name="the-volumes-page"></a>Birimler sayfası
**Birimleri** sayfası, Microsoft Azure StorSimple cihazı, başlatıcıları (Sunucuları) için sağlanan depolama birimleri yönetmenize olanak sağlar. StorSimple Cihazınızda birimlerin listesini gösterir.

 ![Birimler sayfası](./media/storsimple-manage-volumes-u2/VolumePage.png)

Bir birim öznitelikleri bir dizi oluşur:

* **Birim adı** – benzersiz olmalı ve birim tanımlamanıza yardımcı olan açıklayıcı bir ad. Bu ad Ayrıca belirli bir birimde filtre raporları izlemede kullanılır.
* **Durum** – çevrimiçi veya çevrimdışı olabilir. Bir birim çevrimdışıysa, birimi kullanmak erişim izni verilen başlatıcılarına (Sunucuları) görünür değil.
* **Kapasite** – toplam Başlatıcı (sunucu) tarafından depolanan veri miktarını belirtir. Yerel olarak sabitlenmiş birimleri tam olarak sağlanır ve StorSimple cihazında bulunur. Katmanlı birimlerin ölçülü kaynak ve veri yinelenenleri kaldırılmış. Ölçülü kaynak kullanan birimler ile Cihazınızı fiziksel depolama kapasitesi dahili olarak veya bulutta yapılandırılmış birim kapasitesi göre önceden tahsis değil. Birim kapasitesi ayrılmış ve isteğe bağlı olarak tüketilen.
* **Tür** – birim olup olmadığını belirten **katmanlı** (varsayılan) veya **yerel olarak sabitlenmiş**.
* **Yedekleme** – varsayılan bir yedekleme İlkesi birimin var olup olmadığını gösterir.
* **Erişim** – bu birimi erişmesine izin başlatıcıları (Sunucuları) belirtir. Birimle ilişkilendirilen erişim denetimi kaydı (ACR) bir üyesi olmayan başlatıcıları birim görmezsiniz.
* **İzleme** – olsun veya olmasın bir birim izlenmekte belirtir. Bir birim izleme oluşturulduğunda varsayılan olarak etkin olacaktır. Olacaktır, ancak izleme, bir birim kopya için devre dışı. Bir birim için izlemeyi etkinleştirmek için'ndaki yönergeleri izleyin [bir birimi izleyin](#monitor-a-volume). 

Aşağıdaki görevleri gerçekleştirmek için Bu öğreticide yönergeleri kullanın:

* Birim Ekle 
* Bir birim değiştirme 
* Birim türünü değiştirme
* Bir birim Sil 
* Bir birim çevrimdışı duruma getirin 
* Bir birimi izleyin 

## <a name="add-a-volume"></a>Birim Ekle
[Bir birim oluşturduğunuzda](storsimple-deployment-walkthrough-u2.md#step-6-create-a-volume) StorSimple çözümünüzün dağıtımı sırasında. Bir birim ekleme benzer bir yordamdır.

#### <a name="to-add-a-volume"></a>Bir birim eklemek için
1. Üzerinde **aygıtları** sayfasında, cihazı seçin, çift tıklayın ve ardından **birim kapsayıcıları** sekmesi.
2. Listeden bir birim kapsayıcısı seçin ve kapsayıcı ile ilişkili birimlere erişmek için çift tıklatın.
3. Tıklatın **Ekle** sayfanın sonundaki. Ekle bir birim Sihirbazı'nı başlatır.
   
     ![Birim temel ayarları Ekleme Sihirbazı](./media/storsimple-manage-volumes-u2/TieredVolEx.png)
4. Birim ekleme sihirbazının **Temel Ayarlar**’ı altında şunları yapın:
   
   1. Biriminize bir **Ad** verin.
   2. Seçin bir **kullanım türü** aşağı açılan listeden. Her zaman cihazda yerel olarak kullanılabilir olması için veri gerektiren iş yükleri için seçin **yerel olarak sabitlenmiş**. Tüm diğer veri türleri için seçin **katmanlı**. (**Katmanlı** varsayılandır.)
   3. Seçtiyseniz **katmanlı** 2. adımda seçtiğiniz **bu birimi daha az sıklıkta erişilen arşiv verileri için kullanın** arşivleme birim yapılandırmak için onay kutusunu.
   4. Girin **sağlanan kapasite** biriminiz GB veya TB cinsinden. Bkz: [sağlanan kapasite](#provisioned-capacity) her cihaz ve birim türü için en büyük boyutlar için. Bakmak **kullanılabilir kapasite** ne kadar depolama alanı aygıtınızda gerçekte olup olmadığını belirlemek için.
5. Ok simgesine tıklama![Ok simgesi](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png). Yerel olarak sabitlenmiş bir birim yapılandırıyorsanız, şu iletiyi görürsünüz.
   
    ![Birim türü ileti değiştirin](./media/storsimple-manage-volumes-u2/LocalVolEx.png)
6. Ok simgesine tıklayın ![ok simgesi](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png)yeniden gitmek için **ek ayarlar** sayfası.
   
    ![Birim Sihirbazı ek ayarları ekleme](./media/storsimple-manage-volumes-u2/AddVolume2.png)<br>
7. Altında **ek ayarlar**, yeni bir erişim denetimi kaydı (ACR) ekleyin:
   
   1. Erişim denetimi kaydı (ACR) aşağı açılan listeden seçin. Alternatif olarak, yeni ACR ekleyebilirsiniz. ACRs belirlemek IQN konak eşleştirerek, hangi ana bilgisayarların birimlerinizi erişebilmeniz için kayıt içinde listelenen ile. Bir ACR belirtmezseniz, aşağıdaki iletiyi görürsünüz.
      
        ![ACR belirtin](./media/storsimple-manage-volumes-u2/SpecifyACR.png)
   2. Seçtiğiniz öneririz **bu birim için varsayılan yedeklemeyi etkinleştir** onay kutusu.
   3. Onay simgesine tıklayarak ![Onay simgesi](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) Belirtilen ayarlarla birim oluşturmak için.

Yeni birim artık kullanıma hazırdır.

> [!NOTE]
> Yerel olarak sabitlenmiş bir birim oluşturun ve sonra başka bir yerel olarak sabitlenmiş birimin hemen ardından, işler sırayla çalışır birim oluşturma oluşturmak istiyorsanız. Sonraki birim oluşturma işi başlamadan önce ilk birim oluşturma işi bitmesi gerekir.
> 
> 

## <a name="modify-a-volume"></a>Bir birim değiştirme
Bir birimi genişletin veya birime erişmek konakları değiştirmek gerektiğinde değiştirin.

> [!IMPORTANT]
> * Cihazdaki birim boyutunu değiştirirseniz, birim boyutu konakta değiştirilmesi gerekiyor. 
> * Burada açıklanan konak tarafı Windows Server 2012 (2012R2) adımlardır. Yordamlar Linux veya diğer ana bilgisayar işletim sistemleri için farklı olacaktır. Başka bir işletim sistemi çalıştıran bir konak biriminde değiştirirken, ana bilgisayar işletim sistemi yönergelerine başvurun. 
> 
> 

#### <a name="to-modify-a-volume"></a>Bir birim değiştirmek için
1. Üzerinde **aygıtları** sayfasında, cihazı seçin, çift tıklayın ve ardından **birim kapsayıcıları** sekmesi.
2. Kapsayıcı ile ilişkili birimler görüntülemek için çift tıklayın ve listeden bir birim kapsayıcısı seçin.
3. Bir birim seçin ve sayfanın alt kısmındaki tıklatın **Değiştir**. Değiştir Birim Sihirbazı'nı başlatır.
4. Değiştir Birim Sihirbazı'nda altında **temel ayarları**, aşağıdakileri yapabilirsiniz:
   
   * Düzen **adı**.
   * Dönüştürme **kullanım türü** yerel olarak sabitlenmiş için katmanlı veya gelen için yerel olarak sabitlenmiş katmanlı (bkz [birim türünü değiştirme](#change-the-volume-type) daha fazla bilgi için).
   * Artırmak **sağlanan kapasite**. **Sağlanan kapasite** yalnızca artırılabilir. Oluşturulduktan sonra bir birim küçültülemez.
5. Altında **ek ayarlar**, çevrimdışı bir birimdir koşuluyla ACR değiştirebilirsiniz. Birimin çevrimiçi ise, önce çevrimdışı duruma gerekecektir. ' Ndaki adımları başvurmak [bir birim çevrimdışına](#take-a-volume-offline) ACR değiştirmeden önce.
   
   > [!NOTE]
   > Değiştiremezsiniz **varsayılan yedeklemeyi etkinleştir** biriminin seçeneği.
   > 
   > 
6. Onay simgesine tıklayarak yaptığınız değişiklikleri kaydedin ![onay simgesi](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png). Klasik Azure portalında bir güncelleştirme birim iletisi görüntüler. Birim başarıyla güncelleştirildiğinde bir başarı iletisi görüntüler.
7. Bir birim genişletme, Windows ana bilgisayarınız için aşağıdaki adımları tamamlayın:
   
   1. Git **Bilgisayar Yönetimi** ->**Disk Yönetimi**.
   2. Sağ **Disk Yönetimi** seçip **diskleri**.
   3. Diskleri listesinde, güncelleştirilmiş bir birim, sağ tıklatın ve ardından seçin **birimi Genişlet**. Birim Genişletme Sihirbazı'nı başlatır. **İleri**’ye tıklayın.
   4. Varsayılan değerleri kabul ederek Sihirbazı tamamlayın. Sihirbaz tamamlandıktan sonra birimin boyutunun artmasına göstermelidir.
      
      > [!NOTE]
      > Yerel olarak sabitlenmiş bir birim genişletin ve ardından yerel olarak başka bir birimi, Birim genişletme işler sırayla çalıştırmanız hemen sabitlenmiş. Sonraki Birim genişletme işi başlamadan önce ilk birim genişletme işi bitmesi gerekir.
      > 
      > 

![Video var](./media/storsimple-manage-volumes-u2/Video_icon.png) **Video var**

Bir birimi genişletmek nasıl gösteren bir video izlemek için tıklatın [burada](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="change-the-volume-type"></a>Birim türünü değiştirme
Yerel olarak sabitlenmiş için katmanlı birim türünden değiştirebilir veya gelen yerel olarak çok katmanlı sabitlenir. Ancak, bu dönüştürme sık oluşması olmamalıdır. Yerel olarak sabitlenmiş katmanlı bir birimden dönüştürmek için bazı nedenler şunlardır:

* Yerel GARANTİLERİN veri kullanılabilirliği ve performansı ile ilgili
* Bulut gecikmeleri ve bulut bağlantı sorunları ortadan kaldırılması.

Genellikle, bunlar küçük sık erişmek istediğiniz var olan birimler. Yerel olarak sabitlenmiş bir birim oluşturulduğunda tam olarak sağlanır. Yerel olarak sabitlenmiş bir birim için katmanlı birim dönüştürüyorsanız StorSimple dönüştürme başlamadan önce Cihazınızda yeterli alana sahip doğrular. Yeterli alanınız yoksa, bir hata alırsınız ve işlemi iptal edilecek. 

> [!NOTE]
> Yerel olarak sabitlenmiş katmanlı dönüştürme başlamadan önce diğer iş yüklerinin alanı gereksinimlerini göz önünde bulundurduğunuzdan emin olun. 
> 
> 

Diğer birimleri sağlamak için ek alan gerekiyorsa yerel olarak sabitlenmiş bir birim için katmanlı birim değiştirmek isteyebilirsiniz. Katmanlı, yayımlanan kapasite boyutunu aygıt artar biriminde kullanılabilir kapasite yerel olarak sabitlenmiş birimin dönüştürürken. Bağlantı sorunları olan bir birimin yerel türünden katmanlı türüne dönüştürme engelliyorsa dönüştürme tamamlanana kadar yerel birim katmanlı birim özelliklerini sergiler. Bazı verileri buluta geçmiş olmasıdır. Bu spilled verileri işlemi tamamlandı ve yeniden kadar serbest bırakılamıyor cihazda yerel alan kaplar devam eder.

> [!NOTE]
> Bir birim dönüştürme biraz zaman alabilir ve Dönüştürme başladıktan sonra iptal edemezsiniz. Birim dönüştürme sırasında çevrimiçi kalır ve yedek almadan ancak genişletmek veya birimi dönüştürme işlemi gerçekleşirken geri yükleyin.  
> 
> 

Bir katmanlı dönüştürme yerel olarak sabitlenmiş bir birim için cihaz performansı olumsuz etkileyebilir. Ayrıca, aşağıdaki etmenleri dönüştürme işlemini tamamlamak için geçen süre artabilir:

* Yeterli bant genişliği yoktur.
* Geçerli bir yedekleme yoktur.

Bu etkenler olabilir etkileri en aza indirmek için:

* İlkeleri azaltma bant genişliğinizi gözden geçirin ve ayrılmış 40 MB/sn bant genişliği kullanılabilir olduğundan emin olun.
* Dönüştürme yoğun olmayan saatler için zamanlayın.
* Dönüştürme işlemine başlamadan önce bir bulut anlık görüntüsünü alın.

(Farklı iş yükleri destekleme) birden çok birim dönüştürüyorsanız, böylece daha yüksek öncelik birimleri ilk dönüştürülür birim dönüştürme önceliğini. Örneğin, dosya paylaşımı iş yükleri birimlerle dönüştürmeden önce sanal makineleri (VM'ler) barındırmak birimlerini veya birimleri SQL iş yükleri ile dönüştürmeniz gerekir.

#### <a name="to-change-the-volume-type"></a>Birim türünü değiştirmek için
1. Üzerinde **aygıtları** sayfasında, cihazı seçin, çift tıklayın ve ardından **birim kapsayıcıları** sekmesi.
2. Kapsayıcı ile ilişkili birimler görüntülemek için çift tıklayın ve listeden bir birim kapsayıcısı seçin.
3. Bir birim seçin ve sayfanın alt kısmındaki tıklatın **Değiştir**. Değiştir Birim Sihirbazı'nı başlatır.
4. Üzerinde **temel ayarları** sayfasında, yeni türünden seçerek kullanım türünü değiştirme **kullanım türü** aşağı açılan liste.
   
   * Türüne değiştiriyorsanız **yerel olarak sabitlenmiş**, StorSimple yeterli kapasitesi olup olmadığını denetler.
   * Türüne değiştiriyorsanız **katmanlı** ve bu birimi arşiv verileri için select kullanılacak **bu birimi daha az sıklıkta erişilen arşiv verileri için kullanın** onay kutusu.
     
       ![Arşiv onay kutusu](./media/storsimple-manage-volumes-u2/ModifyTieredVolEx.png)
5. Ok simgesine tıklayın ![ok simgesi](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) gitmek için **ek ayarlar** sayfası. Yerel olarak sabitlenmiş bir birim yapılandırıyorsanız, aşağıdaki ileti görüntülenir.
   
    ![Birim türü ileti değiştirin](./media/storsimple-manage-volumes-u2/ModifyLocalVolEx.png)
6. Ok simgesine tıklama ![ok simgesi](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) devam etmek için.
7. Onay simgesine tıklayarak ![Onay simgesi](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) dönüştürme işlemini başlatmak için. Azure portalında bir güncelleştirme birim iletisi görüntüler. Birim başarıyla güncelleştirildiğinde bir başarı iletisi görüntüler.

## <a name="take-a-volume-offline"></a>Bir birim çevrimdışı duruma getirin
Değiştirmek veya silmek planlama yaparken bir birim çevrimdışına gerekebilir. Bir birim çevrimdışı olduğunda okuma-yazma erişimi için kullanılabilir değildir. Cihaz yanı sıra konak birimi çevrimdışı olması gerekir. 

#### <a name="to-take-a-volume-offline"></a>Bir birim çevrimdışına almak için
1. Söz konusu birim çevrimdışı duruma getirmeden önce kullanımda olmadığından emin olun.
2. Birimin çevrimdışı konakta ilk alın. Bu, olası tüm birimdeki veri bozulması riskini ortadan kaldırır. Belirli adımlar için işletim sistemi için yönergelerine bakın.
3. Ana bilgisayar çevrimdışıysa, birim çevrimdışı cihazda aşağıdaki adımları gerçekleştirerek başlatıldıktan sonra:
   
   1. Üzerinde **aygıtları** sayfasında, cihazı seçin, çift tıklayın ve ardından **birim kapsayıcıları** sekmesi. **Birim kapsayıcıları** sekmesi tablo biçiminde aygıtla ilişkili tüm birim kapsayıcıları listeler.
   2. Birim kapsayıcısı seçin ve kapsayıcıdaki tüm birimlerin listesini görüntülemek için tıklatın.
   3. Bir birim seçin ve tıklatın **çevrimdışına**.
   4. Onayınız istendiğinde **Evet**’e tıklayın. Birim artık çevrimdışı olması gerekir.
      
      Bir birim çevrimdışıysa sonra **çevrimiçine** seçeneği kullanılabilir olur.

> [!NOTE]
> **Çevrimdışına Al** komutu, birim çevrimdışı duruma getirmek için aygıta bir istek gönderir. Konak birimi kullanmaya devam ediyorsanız bu bozuk bağlantıları olur ancak birim çevrimdışı duruma getirmeden değil başarısız olur. 
> 
> 

## <a name="delete-a-volume"></a>Bir birim Sil
> [!IMPORTANT]
> Yalnızca çevrimdışı ise, bir birim silebilirsiniz.
> 
> 

Bir birimi silmek için aşağıdaki adımları tamamlayın.

#### <a name="to-delete-a-volume"></a>Bir birimi silmek için
1. Üzerinde **aygıtları** sayfasında, cihazı seçin, çift tıklayın ve ardından **birim kapsayıcıları** sekmesi.
2. Silmek istediğiniz biriminin birim kapsayıcısı seçin. Birim kapsayıcısı erişmek için tıklatın **birimleri** sayfası.
3. Bu kapsayıcı ile ilişkili tüm birimleri tablo biçiminde görüntülenir. Silmek istediğiniz birime durumunu denetleyin. Silmek istediğiniz birim çevrimdışı durumda değilse, bunu ilk olarak, aşağıdaki adımları çevrimdışına [bir birim çevrimdışına](#take-a-volume-offline).
4. Çevrimdışı bir birimdir sonra tıklayın **silmek** sayfanın sonundaki.
5. Onayınız istendiğinde **Evet**’e tıklayın. Birim artık silinecek ve **birimleri** sayfa birimleri kapsayıcıdaki güncelleştirilmiş listesini gösterir.
   
   > [!NOTE]
   > Yerel olarak sabitlenmiş bir birim silerseniz, yeni birimleri için kullanılabilir alanı hemen güncelleştirilmemiş. StorSimple Yöneticisi hizmeti, kullanılabilir yerel alanı düzenli olarak güncelleştirir. Yeni birim oluşturmak denemeden önce birkaç dakika bekleyin öneririz.<br> Yerel olarak sabitlenmiş bir birim silin ve ardından başka bir yerel olarak silerseniz ek olarak, birim hemen ardından, işler sırayla çalışır toplu silme sabitlenmiş. Sonraki toplu silme işi başlamadan önce ilk toplu silme işi bitmesi gerekir.
   > 
   > 

## <a name="monitor-a-volume"></a>Bir birimi izleyin
Birim izleme bir birim için g/Ç ile ilgili istatistikleri toplamanızı sağlar. İzleme, oluşturduğunuz ilk 32 birimleri için varsayılan olarak etkinleştirilir. Ek birimlerin izleme varsayılan olarak devre dışıdır. Kopyalanan birimlerin izleme varsayılan olarak devre dışıdır.

Etkinleştirmek veya bir birim için izlemeyi devre dışı bırakmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-enable-or-disable-volume-monitoring"></a>Etkinleştirmek veya toplu izleme devre dışı bırakmak için
1. Üzerinde **aygıtları** sayfasında, cihazı seçin, çift tıklayın ve ardından **birim kapsayıcıları** sekmesi.
2. Birimin bulunduğu birim kapsayıcısı seçin ve ardından erişmek için birim kapsayıcısı tıklayın **birimleri** sayfası.
3. Bu kapsayıcı ile ilişkili tüm birimleri Tablo görünümünde listelenir. ' I tıklatın ve birim veya toplu kopyalama seçin.
4. Sayfanın alt kısmındaki tıklatın **Değiştir**.
5. Birim Değiştirme Sihirbazı'nda altında **temel ayarları**seçin **etkinleştirmek** veya **devre dışı** gelen **izleme** aşağı açılan liste.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [bir StorSimple biriminin kopyalama](storsimple-clone-volume.md).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

