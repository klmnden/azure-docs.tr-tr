---
title: "StorSimple birimlerinizi yönetme | Microsoft Docs"
description: "Ekleme, değiştirme, izlemek ve StorSimple birimleri silin ve bunları gerekirse çevrimdışına alma konusunda açıklanmaktadır."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ccabd859-590c-4568-a81d-6ff38f125be3
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/11/2016
ms.author: v-sharos
ms.openlocfilehash: 31ed9dad8ba56a3746873b7b35e678e97743fbfe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-volumes"></a>Birimleri yönetmek için StorSimple Yöneticisi hizmetini kullanma
[!INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Genel Bakış
Bu öğretici oluşturmak ve birimler StorSimple cihazı ve StorSimple sanal cihazı yönetmek için StorSimple Yöneticisi hizmetini kullanmayı açıklar.

StorSimple Yöneticisi hizmeti, StorSimple çözümünüzün bir tek web arabiriminden yönetmenizi sağlayan Azure Klasik Portalı'nı uzantısıdır. Birimleri yönetmeye ek olarak, StorSimple Yöneticisi hizmeti oluşturmak ve StorSimple hizmetleri yönetmek, görüntülemek ve cihazları yönetmek, uyarıları görüntüleyin ve görüntülemek ve yedekleme ilkeleri ve yedekleme kataloğu yönetmek için kullanabilirsiniz.

> [!NOTE]
> Azure StorSimple yalnızca ölçülü kaynak kullanan birimler oluşturabilir. Tamamen sağlanan veya kısmen sağlanan oluşturulamıyor bir Azure StorSimple sistemi birimlerde.
> 
> Ölçülü kaynak sağlama fiziksel kaynakları aşan kullanılabilir depolama alanı görünür bir sanallaştırma teknolojisidir. Yeterli depolama alanı önceden ayırma yerine Azure StorSimple ölçülü kaynak sağlama geçerli gereksinimlerini karşılamak için yeterli alan ayırmak için kullanır. Azure StorSimple artırabilir veya değişen taleplerini karşılamak üzere bulut depolama azaltmak için bu yaklaşım bulut depolama esnek yapısını kolaylaştırır.
> 
> 

## <a name="the-volumes-page"></a>Birimler sayfası
**Birimleri** sayfası, Microsoft Azure StorSimple cihazı, başlatıcıları (Sunucuları) için sağlanan depolama birimleri yönetmenize olanak sağlar. StorSimple Cihazınızda birimlerin listesini gösterir.

 ![Birimler sayfası](./media/storsimple-manage-volumes/HCS_VolumesPage.png)

Bir birim öznitelikleri bir dizi oluşur:

* **Ad** – benzersiz olmalı ve birim tanımlamanıza yardımcı olan açıklayıcı bir ad. Bu ad Ayrıca belirli bir birimde filtre raporları izlemede kullanılır.
* **Durum** – çevrimiçi veya çevrimdışı olabilir. Bir birim, çevrimdışı durumdaysa birimi kullanmak erişim izni verilen başlatıcılarına (Sunucuları) görünür değil.
* **Kapasite** – ne kadar büyük birim, (sunucu) başlatıcısı tarafından algılanan olarak belirtir. Kapasite toplam Başlatıcı (sunucu) tarafından depolanan veri miktarını belirtir. Birimlerin ölçülü kaynak ve veri yinelenenleri kaldırılmış. Bu, Cihazınızı fiziksel depolama kapasitesi dahili olarak veya Bulut yapılandırılmış birim kapasitesi göre önceden ayırmanız değil anlamına gelir. Birim kapasitesi ayrılmış ve isteğe bağlı olarak tüketilen.
* **Tür** – birim türü katmanlı veya arşivleme olabilir (bir alt türü katmanlı)
* **Erişim** – bu birimi erişmesine izin başlatıcıları (Sunucuları) belirtir. Birimle ilişkilendirilen erişim denetimi kaydı (ACR) bir üyesi olmayan başlatıcıları birim görmezsiniz.
* **İzleme** – olsun veya olmasın bir birim izlenmekte belirtir. Bir birim izleme oluşturulduğunda varsayılan olarak etkin olacaktır. Olacaktır, ancak izleme, bir birim kopya için devre dışı. Bir birim için izlemeyi etkinleştirmek için bir birim İzleyici'ndaki yönergeleri izleyin.

Bir birimi ile ilişkili en yaygın görevleri şunlardır:

* Birim Ekle
* Bir birim değiştirme
* Bir birim Sil
* Bir birim çevrimdışı duruma getirin
* Bir birimi izleyin

## <a name="add-a-volume"></a>Birim Ekle
[Bir birim oluşturduğunuzda](storsimple-deployment-walkthrough-u1.md#step-6-create-a-volume) StorSimple çözümünüzün dağıtımı sırasında. Bir birim ekleme benzer bir yordamdır.

### <a name="to-add-a-volume"></a>Bir birim eklemek için
1. Üzerinde **aygıtları** sayfasında, cihazı seçin, çift tıklayın ve ardından **birim kapsayıcıları** sekmesi.
2. Birim kapsayıcısı seçin ve kapsayıcı ile ilişkili birimlere erişmek için karşılık gelen satırda oka tıklayın.
3. Tıklatın **Ekle** sayfanın sonundaki. Ekle bir birim Sihirbazı'nı başlatır.
   
     ![Birim temel ayarları Ekleme Sihirbazı](./media/storsimple-manage-volumes/AddVolume1.png)
4. Birim ekleme sihirbazının **Temel Ayarlar**’ı altında şunları yapın:
   
   1. Biriminize bir **Ad** verin.
   2. Biriminiz için GB veya TB cinsinden **Sağlanan Kapasite**’yi belirtin. Kapasite, fiziksel aygıt için 1 GB ile 64 TB arasında olmalıdır. StorSimple sanal cihaz üzerindeki bir birimi için sağlanan maksimum kapasite 30 TB'tır.
   3. Seçin **kullanım türü** biriminiz için. Arşiv verileri için katmanlı birim kullanıyorsanız, seçme **bu birimi daha az sıklıkta erişilen arşiv verileri için kullanın** onay kutusunu biriminiz yinelenenleri kaldırma öbek boyutunu 512 KB olarak değiştirir. Bu seçeneği seçmezseniz, ilgili katmanlı birim 64 KB öbek boyutu kullanır. Daha büyük bir yinelenenleri kaldırma öbek boyutu cihazın büyük arşiv verilerinin buluta aktarımını hızlandırmak izin verir. (Katmanlı birimleri eski birincil birimler adı veriliyordu.)
   4. Ok simgesine tıklayın ![ok simgesi](./media/storsimple-manage-volumes/HCS_ArrowIcon.png)gitmek için **ek ayarlar** sayfası.
      
        ![Birim Sihirbazı ek ayarları ekleme](./media/storsimple-manage-volumes/AddVolume2.png)
5. Altında **ek ayarlar**, yeni bir erişim denetimi kaydı (ACR) ekleyin:
   
   1. Erişim denetimi kaydı (ACR) aşağı açılan listeden seçin. Alternatif olarak, yeni ACR ekleyebilirsiniz. ACRs belirlemek IQN konak eşleştirerek, hangi ana bilgisayarların birimlerinizi erişebilmeniz için kayıt içinde listelenen ile.
   2. **Bu birim için varsayılan yedeklemeyi etkinleştir** onay kutusunu seçerek varsayılan yedeği etkinleştirmenizi öneririz.
   3. Onay simgesine tıklayarak ![Onay simgesi](./media/storsimple-manage-volumes/HCS_CheckIcon.png) Belirtilen ayarlarla birim oluşturmak için.

Yeni birim artık kullanıma hazırdır.

## <a name="modify-a-volume"></a>Bir birim değiştirme
Bir birimi genişletin veya birime erişmek konakları değiştirmek gerektiğinde değiştirin.

> [!IMPORTANT]
> * Cihazdaki birim boyutunu değiştirirseniz, birim boyutu konakta değiştirilmesi gerekiyor.
> * Burada açıklanan konak tarafı Windows Server 2012 (2012R2) adımlardır. Yordamlar Linux veya diğer ana bilgisayar işletim sistemleri için farklı olacaktır. Başka bir işletim sistemi çalıştıran bir konak biriminde değiştirirken, ana bilgisayar işletim sistemi yönergelerine başvurun.
> 
> 

### <a name="to-modify-a-volume"></a>Bir birim değiştirmek için
1. Üzerinde **aygıtları** sayfasında, cihazı seçin, çift tıklayın ve ardından **birim kapsayıcısı** sekmesi. Bu sayfa tablo biçiminde aygıtla ilişkili tüm birim kapsayıcıları listeler.
2. Birim kapsayıcısı seçin ve kapsayıcıdaki tüm birimlerin listesini görüntülemek için tıklatın.
3. Üzerinde **birimleri** sayfasında, bir birim seçin ve tıklayın **Değiştir**.
4. Değiştir Birim Sihirbazı'nda altında **temel ayarları**, aşağıdakileri yapabilirsiniz:
   
   * Düzen **adı** ve **türü** seçerek arşivleme bir birime katmanlı birim değiştirmek isteyip istemediğinizi **bu birimi daha az sıklıkta erişilen arşiv verileri için kullanın** biriminiz yinelenenleri kaldırma öbek boyutunu 512 KB olarak değiştirmek için onay kutusunu.
   * Artırmak **sağlanan kapasite**. **Sağlanan kapasite** yalnızca artırılabilir. Oluşturulduktan sonra bir birim küçültülemez.
     
     > [!NOTE]
     > Bir birime atanmış sonra birim kapsayıcısı değiştiremezsiniz.
     > 
     > 
5. Altında **ek ayarlar**, aşağıdakileri yapabilirsiniz:
   
   * Çevrimdışı bir birimdir koşuluyla ACRs değiştirin. Birimin çevrimiçi ise, önce çevrimdışı duruma gerekecektir. ' Ndaki adımları başvurmak [bir birim çevrimdışına](#take-a-volume-offline) ACR değiştirmeden önce.
   * Çevrimdışı bir birimdir sonra ACRs listesini değiştirin.
     
     > [!NOTE]
     > Değiştiremezsiniz **bu birim için varsayılan yedeklemeyi etkinleştir** biriminin seçeneği.
     > 
     > 
6. Onay simgesine tıklayarak yaptığınız değişiklikleri kaydedin ![onay simgesi](./media/storsimple-manage-volumes/HCS_CheckIcon.png). Klasik Azure portalında bir güncelleştirme birim iletisi görüntüler. Birim başarıyla güncelleştirildiğinde bir başarı iletisi görüntüler.
7. Bir birim genişletme, Windows ana bilgisayarınız için aşağıdaki adımları tamamlayın:
   
   1. Git **Bilgisayar Yönetimi** ->**Disk Yönetimi**.
   2. Sağ **Disk Yönetimi** seçip **diskleri**.
   3. Diskleri listesinde, güncelleştirilmiş bir birim, sağ tıklatın ve ardından seçin **birimi Genişlet**. Birim Genişletme Sihirbazı'nı başlatır. **İleri**’ye tıklayın.
   4. Varsayılan değerleri kabul ederek Sihirbazı tamamlayın. Sihirbaz tamamlandıktan sonra birimin boyutunun artmasına göstermelidir.

![Video var](./media/storsimple-manage-volumes/Video_icon.png) **Video var**

Bir birimi genişletmek nasıl gösteren bir video izlemek için tıklatın [burada](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="take-a-volume-offline"></a>Bir birim çevrimdışı duruma getirin
Değiştirmek veya silmek planlama yaparken bir birim çevrimdışına gerekebilir. Bir birim çevrimdışı olduğunda okuma-yazma erişimi için kullanılabilir değildir. Cihaz yanı sıra konak birimi çevrimdışı olması gerekir. Bir birim çevrimdışına almak için aşağıdaki adımları gerçekleştirin.

### <a name="to-take-a-volume-offline"></a>Bir birim çevrimdışına almak için
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

### <a name="to-delete-a-volume"></a>Bir birimi silmek için
1. Üzerinde **aygıtları** sayfasında, cihazı seçin, çift tıklayın ve ardından **birim kapsayıcıları** sekmesi.
2. Silmek istediğiniz biriminin birim kapsayıcısı seçin. Birim kapsayıcısı erişmek için tıklatın **birimleri** sayfası.
3. Bu kapsayıcı ile ilişkili tüm birimleri tablo biçiminde görüntülenir. Silmek istediğiniz birime durumunu denetleyin. Silmek istediğiniz birim çevrimdışı durumda değilse, bunu ilk olarak, aşağıdaki adımları çevrimdışına [bir birim çevrimdışına](#take-a-volume-offline).
4. Çevrimdışı bir birimdir sonra tıklayın **silmek** sayfanın sonundaki.
5. Onayınız istendiğinde **Evet**’e tıklayın. Birim artık silinecek ve **birimleri** sayfa birimleri kapsayıcıdaki güncelleştirilmiş listesini gösterir.

## <a name="monitor-a-volume"></a>Bir birimi izleyin
Birim izleme bir birim için g/Ç ile ilgili istatistikleri toplamanızı sağlar. İzleme, oluşturduğunuz ilk 32 birimleri için varsayılan olarak etkinleştirilir. Ek birimlerin izleme varsayılan olarak devre dışıdır. Kopyalanan birimlerin izleme varsayılan olarak devre dışıdır.

Etkinleştirmek veya bir birim için izlemeyi devre dışı bırakmak için aşağıdaki adımları gerçekleştirin.

### <a name="to-enable-or-disable-volume-monitoring"></a>Etkinleştirmek veya toplu izleme devre dışı bırakmak için
1. Üzerinde **aygıtları** sayfasında, cihazı seçin, çift tıklayın ve ardından **birim kapsayıcıları** sekmesi.
2. Birimin bulunduğu birim kapsayıcısı seçin ve ardından erişmek için birim kapsayıcısı tıklayın **birimleri** sayfası.
3. Bu kapsayıcı ile ilişkili tüm birimleri Tablo görünümünde listelenir. ' I tıklatın ve birim veya toplu kopyalama seçin.
4. Sayfanın alt kısmındaki tıklatın **Değiştir**.
5. Birim Değiştirme Sihirbazı'nda altında **temel ayarları**seçin **etkinleştirmek** veya **devre dışı** gelen **izleme** aşağı açılan liste.
   
    ![Bir birim temel ayarlarını değiştirme](./media/storsimple-manage-volumes/HCS_MonitorVolumeM.png)

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [bir StorSimple biriminin kopyalama](storsimple-clone-volume.md).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

