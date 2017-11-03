---
title: StorSimple 8000 serisi toplu kopyalama | Microsoft Docs
description: "Farklı kopya türlerini ve bunların ne zaman kullanılacağı ve bir yedekleme kümesi bağımsız bir birim kopyalamak için nasıl kullanabileceğiniz açıklanmaktadır."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 070ac53e-7388-4c48-b8a5-8ed7f9108b2c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/26/2017
ms.author: alkohli
ms.openlocfilehash: 2b627250df62bbe3299869ef3812947afbb8f29f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-manager-service-to-clone-a-volume-update-2"></a>Bir birim (güncelleştirme 2) kopyalamak için StorSimple Yöneticisi hizmetini kullanma
[!INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Genel Bakış
StorSimple Yöneticisi hizmeti **yedekleme kataloğu** sayfası el ile veya otomatik yedeklemeler durumdayken, oluşturulan tüm yedekleme kümelerini görüntüler. Bir yedekleme İlkesi ya da bir birim seçin için tüm yedeklemeler listelemek veya yedekleri de silmek için bu sayfayı kullanın ya da bir yedeği geri yüklemek veya bir birim kopyalamak için kullanın.

![Yedekleme kataloğu sayfası](./media/storsimple-clone-volume-u2/backupCatalog.png)  

Bu öğretici, bir yedekleme kümesi bağımsız bir birim kopyalamak için nasıl kullanabileceğinizi açıklar. Ayrıca arasındaki farkı açıklar *geçici* ve *kalıcı* klonlar.

> [!NOTE]
> Yerel olarak sabitlenmiş bir birim katmanlı birim kopyalanma. Yerel olarak sabitlenmiş için kopyalanan birimi gerekiyorsa, kopyalama işlemi başarıyla tamamlandıktan sonra kopyanın yerel olarak sabitlenmiş bir birim dönüştürebilirsiniz. Yerel olarak sabitlenmiş bir birim için katmanlı birim dönüştürme hakkında daha fazla bilgi için Git [birim türünü değiştirme](storsimple-manage-volumes-u2.md#change-the-volume-type).
> 
> Kopyalanan bir birimden dönüştürmeyi deneyin (Bunu yine bir geçici kopya olduğunda) hemen kopyalandıktan sonra yerel olarak sabitlenmiş katmanlı dönüştürme şu hata iletisiyle başarısız olur:
> 
> `Unable to modify the usage type for volume {0}. This can happen if the volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry the modify operation.` 
> 
> Yalnızca farklı bir cihaz açın kopyalama, bu hata alınır. Yerel olarak sabitlenmiş geçici kopya kalıcı bir kopya ilk dönüştürürseniz birimi başarıyla dönüştürebilirsiniz. Geçici kopya kalıcı bir kopya dönüştürmek için bunu bir bulut anlık görüntüsünü alın.
> 
> 

## <a name="create-a-clone-of-a-volume"></a>Bir birimin bir kopyasını oluşturun
Yerel kullanarak aynı aygıt, başka bir aygıt veya sanal makine bir kopya oluşturabilirsiniz veya Bulut anlık görüntüsü.

#### <a name="to-clone-a-volume"></a>Birim kopyalama
1. StorSimple Yöneticisi hizmet sayfasında, tıklatın **yedekleme kataloğu** sekmesinde ve bir yedekleme kümesi seçin.
2. İlişkili birimleri görüntülemek için yedekleme kümesini genişletin. ' I tıklatın ve yedekleme kümesinden bir birim seçin.
   
     ![Bir birimi kopyalama](./media/storsimple-clone-volume-u2/CloneVol.png) 
3. Tıklatın **kopya** seçilen birim kopyalamaya başlamak için.
4. Kopya Birim Sihirbazı'nda altında **ad ve konum belirtin**:
   
   1. Bir hedef cihaz tanımlayın. Bu, kopyanın oluşturulacağı konumdur. Aynı aygıt seçin veya başka bir aygıt belirtin. Diğer bulut hizmeti sağlayıcıları ile ilişkili bir birim seçerseniz (değil Azure), hedef cihaz için aşağı açılan listesi yalnızca fiziksel cihazları gösterir. Bir sanal cihazdaki diğer bulut hizmeti sağlayıcıları ile ilişkili bir birim kopyalayamıyor.
      
      > [!NOTE]
      > Kopya için gereken kapasiteyi hedef cihazda kullanılabilir kapasiteden daha düşük olduğundan emin olun.
      > 
      > 
   2. Kopya için benzersiz birim adı belirtin. Adı 3 ile 127 karakter içermelidir. 
      
      > [!NOTE]
      > **Kopya birim olarak** alan **katmanlı** yerel olarak sabitlenmiş bir birim kopyalama olsa bile. Bu ayarı değiştiremezsiniz; Ancak, yerel olarak da sabitlenmelidir kopyalanan birimi gerekiyorsa, kopya yerel olarak sabitlenmiş bir birim dönüştürebilir, kopya başarıyla oluşturduktan sonra. Yerel olarak sabitlenmiş bir birim için katmanlı birim dönüştürme hakkında daha fazla bilgi için Git [birim türünü değiştirme](storsimple-manage-volumes-u2.md#change-the-volume-type).
      > 
      > 
      
        ![Kopya Sihirbazı 1](./media/storsimple-clone-volume-u2/clone1.png) 
   3. Ok simgesine tıklama ![ok simgesi](./media/storsimple-clone-volume-u2/HCS_ArrowIcon.png) sonraki sayfaya devam etmek için.
5. Altında **bu birimi kullanabilir ana bilgisayarları belirleyin**:
   
   1. Kopya için bir erişim denetimi kaydı (ACR) belirtin. Yeni bir ACR ekleyebilir veya var olan listeden seçin.
      
        ![Kopyalama Sihirbazı'nı 2](./media/storsimple-clone-volume-u2/clone2.png) 
   2. Onay simgesine tıklayarak ![onay simgesi](./media/storsimple-clone-volume-u2/HCS_CheckIcon.png)işlemi tamamlamak için.
6. Bir kopya iş başlatılır ve kopya başarıyla oluşturulduğunda size bildirilecek. Tıklatın **işi görüntüle** üzerinde kopya işi izlemek için **işleri** sayfası. Kopya işi tamamlandığında aşağıdaki iletiyi görürsünüz:
   
    ![Kopya iletisi](./media/storsimple-clone-volume-u2/CloneMsg.png) 
7. Kopyalama işlemi tamamlandıktan sonra:
   
   1. Git **aygıtları** sayfasında ve seçin **birim kapsayıcıları** sekmesi. 
   2. Kopyaladığınız kaynak birimle ilişkilendirilen birim kapsayıcısı seçin. Birimleri listesinde, yeni oluşturduğunuz kopya görmeniz gerekir.

> [!NOTE]
> İzleme ve varsayılan yedekleme otomatik olarak kopyalanan bir birimde devre dışı.
> 
> 

Bu yolla oluşturulan bir kopya geçici kopya ' dir. Kopya türleri hakkında daha fazla bilgi için bkz: [geçici ve kalıcı klonlar](#transient-vs-permanent-clones).

Bu kopya artık normal bir birim ve bir birimde mümkündür herhangi bir işlem için kopya kullanılabilir olacaktır. Bu birimi tüm yedeklemeler için yapılandırmanız gerekir.

## <a name="transient-vs-permanent-clones"></a>Geçici ve kalıcı klonlar karşılaştırması
Yalnızca farklı bir cihaz kopyalarken geçici klonlar oluşturulur. Yedekleme StorSimple Yöneticisi tarafından yönetilen farklı bir cihaz kümesi belirli bir birimden kopyalayabilirsiniz. Geçici kopya verilere başvuru özgün birimin sahip olur ve bu verileri okuyup hedef cihazda yerel olarak yazmak için kullanır. 

Bir geçici kopya bir bulut anlık görüntüsünü sonra sonuçta elde edilen kopya olacak bir *kalıcı* kopya. Bu işlem sırasında verilerin bir kopyasını Bulutu üzerinde oluşturulur ve bu verileri kopyalamak için saat veri ve (bir Azure Azure kopyalama budur) Azure gecikmeleri boyutu tarafından belirlenir. Bu işlem için hafta gün sürebilir. Geçici kopya bu şekilde kalıcı bir kopya olur ve tüm başvuruları gelen kopyalandı özgün birimin verilere sahip değil. 

## <a name="scenarios-for-transient-and-permanent-clones"></a>Geçici ve kalıcı klonlar senaryoları
Aşağıdaki bölümlerde geçici ve kalıcı klonlar kullanılabilir örnek durumlar açıklanmaktadır.

### <a name="item-level-recovery-with-a-transient-clone"></a>Geçici bir kopya ile öğe düzeyinde kurtarma
Bir yıl eski Microsoft PowerPoint sunusu dosyası kurtarmak gerekir. BT yöneticiniz, o zaman çerçevesi belirli yedekten tanımlar ve birim filtreler. Yönetici daha sonra birim klonlar, aradığınız dosya bulur ve olanak sağlar. Bu senaryoda, bir geçici kopya kullanılır. 

![Video var](./media/storsimple-clone-volume-u2/Video_icon.png) **Video var**

Nasıl kopya kullanın ve geri yükleme silinmiş dosyaları kurtarmak için StorSimple özellikleri gösteren bir video izlemek için tıklatın [burada](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>Kalıcı bir kopya ile üretim ortamında test etme
Üretim ortamında test hata doğrulamanız gerekir. Üretim ortamında birimin bir kopyasını oluşturun ve sonra bu kopya bağımsız kopyalanan birim oluşturmak için bir bulut anlık görüntüsünü. Bu senaryoda, kalıcı bir kopya kullanılır.  

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [bir yedeklemek kümesinden StorSimple birimini geri](storsimple-restore-from-backup-set-u2.md).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

