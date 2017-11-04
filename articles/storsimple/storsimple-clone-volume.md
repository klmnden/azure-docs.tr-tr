---
title: StorSimple birim kopyalama | Microsoft Docs
description: "Farklı kopya türlerini ve bunların ne zaman kullanılacağı ve bir yedekleme kümesi bağımsız bir birim kopyalamak için nasıl kullanabileceğiniz açıklanmaktadır."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: b5d615f2-02a7-4222-9867-6c0385ce748c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/02/2017
ms.author: alkohli
ms.openlocfilehash: cf11a369549b887f79a81c19780048d31e56beae
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="use-the-storsimple-manager-service-to-clone-a-volume"></a>Bir birim kopyalamak için StorSimple Yöneticisi hizmetini kullanma
> [!NOTE]
> StorSimple için Klasik portalı kullanım dışıdır. StorSimple cihaz yöneticileri yeni Azure portalına kullanımdan zamanlamaya göre otomatik olarak taşır. Bir e-posta ve bu taşıma için portal bir bildirim alırsınız. Bu belgede ayrıca yakında kullanımdan kaldırılacaktır. Taşıma hakkında herhangi bir sorunuz için bkz: [SSS: Azure portalına taşıma](storsimple-8000-move-azure-portal-faq.md).

[!INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Genel Bakış
StorSimple Yöneticisi hizmeti **yedekleme kataloğu** sayfası el ile veya otomatik yedeklemeler durumdayken, oluşturulan tüm yedekleme kümelerini görüntüler. Bir yedekleme İlkesi ya da bir birim seçin için tüm yedeklemeler listelemek veya yedekleri de silmek için bu sayfayı kullanın ya da bir yedeği geri yüklemek veya bir birim kopyalamak için kullanın.

![Yedekleme kataloğu sayfası](./media/storsimple-clone-volume/HCS_BackupCatalog.png)  

Bu öğretici, bir yedekleme kümesi bağımsız bir birim kopyalamak için nasıl kullanabileceğinizi açıklar. Ayrıca arasındaki farkı açıklar *geçici* ve *kalıcı* klonlar. 

## <a name="create-a-clone-of-a-volume"></a>Bir birimin bir kopyasını oluşturun
Bir kopya, yerel veya bir bulut anlık görüntüsü kullanarak aynı aygıt, başka bir aygıt veya sanal makine oluşturabilirsiniz.

#### <a name="to-clone-a-volume"></a>Birim kopyalama
1. StorSimple Yöneticisi hizmet sayfasında, tıklatın **yedekleme kataloğu** sekmesinde ve bir yedekleme kümesi seçin.
2. İlişkili birimleri görüntülemek için yedekleme kümesini genişletin. ' I tıklatın ve yedekleme kümesinden bir birim seçin.
   
     ![Bir birimi kopyalama](./media/storsimple-clone-volume/HCS_Clone.png) 
3. Tıklatın **kopya** seçilen birim kopyalamaya başlamak için.
4. Kopya Birim Sihirbazı'nda altında **ad ve konum belirtin**:
   
   1. Bir hedef cihaz tanımlayın. Bu, kopyanın oluşturulacağı konumdur. Aynı aygıt seçin veya başka bir aygıt belirtin. Diğer bulut hizmeti sağlayıcıları ile ilişkili bir birim seçerseniz (değil Azure), hedef cihaz için aşağı açılan listesi yalnızca fiziksel cihazları gösterir. Bir sanal cihazdaki diğer bulut hizmeti sağlayıcıları ile ilişkili bir birim kopyalayamıyor.
      
      > [!NOTE]
      > Kopya için gereken kapasiteyi hedef cihazda kullanılabilir kapasiteden daha düşük olduğundan emin olun.
      > 
      > 
   2. Kopya için benzersiz birim adı belirtin. Adı 3 ile 127 karakter içermelidir.
   3. Ok simgesine tıklama ![ok simgesi](./media/storsimple-clone-volume/HCS_ArrowIcon.png) sonraki sayfaya devam etmek için.
5. Altında **bu birimi kullanabilir ana bilgisayarları belirleyin**:
   
   1. Kopya için bir erişim denetimi kaydı (ACR) belirtin. Yeni bir ACR ekleyebilir veya var olan listeden seçin.
   2. Onay simgesine tıklayarak ![onay simgesi](./media/storsimple-clone-volume/HCS_CheckIcon.png)işlemi tamamlamak için.
6. Bir kopya iş başlatılır ve kopya başarıyla oluşturulduğunda size bildirilecek. Tıklatın **işi görüntüle** üzerinde kopya işi izlemek için **işleri** sayfası.
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
Yalnızca farklı bir cihaz açın kopyalarken geçici ve kalıcı klonlar oluşturulur. Yedekleme için farklı bir cihaz kümesi belirli bir birimden kopyalayabilirsiniz. Bu yolla oluşturulan bir kopya olduğundan bir *geçici* kopya. Geçici kopya özgün birimin başvuran ve bu birimde yerel olarak yazılırken okumak için kullanır. 

Bir geçici kopya bir bulut anlık görüntüsünü sonra sonuçta elde edilen kopya olacak bir *kalıcı* kopya. Kalıcı kopya bağımsızdır ve tüm başvuruları gelen kopyalandı özgün birimine sahip değil.  

## <a name="scenarios-for-transient-and-permanent-clones"></a>Geçici ve kalıcı klonlar senaryoları
Aşağıdaki bölümlerde geçici ve kalıcı klonlar kullanılabilir örnek durumlar açıklanmaktadır.

### <a name="item-level-recovery-with-a-transient-clone"></a>Geçici bir kopya ile öğe düzeyinde kurtarma
Bir yıl eski Microsoft PowerPoint sunusu dosyası kurtarmak gerekir. BT yöneticiniz, o zaman çerçevesi belirli yedekten tanımlar ve birim filtreler. Yönetici daha sonra birim klonlar, aradığınız dosya bulur ve olanak sağlar. Bu senaryoda, bir geçici kopya kullanılır. 

![Video var](./media/storsimple-clone-volume/Video_icon.png) **Video var**

Nasıl kopya kullanın ve geri yükleme silinmiş dosyaları kurtarmak için StorSimple özellikleri gösteren bir video izlemek için tıklatın [burada](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>Kalıcı bir kopya ile üretim ortamında test etme
Üretim ortamında test hata doğrulamanız gerekir. Üretim ortamında bir bulut bu kopya anlık görüntüsü tarafından birimin bir kopyasını oluşturun. Kopyalanan birim artık bağımsızdır. Bu senaryoda, kalıcı bir kopya kullanılır.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [bir yedeklemek kümesinden StorSimple birimini geri](storsimple-restore-from-backup-set.md).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

