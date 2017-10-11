---
title: "StorSimple yedekleme kataloğu yönetme | Microsoft Docs"
description: "StorSimple Yöneticisi hizmeti yedekleme kataloğu sayfası listesinde seçin ve bir birim için yedekleme kümelerini silmek için nasıl kullanılacağını açıklar."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ad81bee9-fe43-40b3-a384-b15fb274ecd9
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/28/2016
ms.author: v-sharos
ms.openlocfilehash: 5ee9855e1428c7a2d871d9c215d302c5c3b7101a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-your-backup-catalog"></a>Yedekleme kataloğu yönetmek için StorSimple Yöneticisi hizmetini kullanma
## <a name="overview"></a>Genel Bakış
StorSimple Yöneticisi hizmeti **yedekleme kataloğu** sayfası el ile veya zamanlanmış yedeklemeler durumdayken, oluşturulan tüm yedekleme kümelerini görüntüler. Bir yedekleme İlkesi ya da bir birim seçin için tüm yedeklemeler listelemek veya yedekleri de silmek için bu sayfayı kullanın ya da bir yedeği geri yüklemek veya bir birim kopyalamak için kullanın.

Bu öğretici, liste, seçin ve bir yedekleme kümesi silme açıklanmaktadır. Cihazınızı yedekten geri yükleme konusunda bilgi almak için şu adrese gidin [Cihazınızı yedekleme kümesinden geri](storsimple-restore-from-backup-set.md). Birim kopyalama öğrenmek için şu adrese gidin [bir StorSimple biriminin kopyalama](storsimple-clone-volume.md).

![Yedekleme kataloğu](./media/storsimple-manage-backup-catalog/backupcatalog.png) 

**Yedekleme kataloğu** sayfası, yedekleme daraltmak için bir sorgu olarak seçimi sağlar. Aşağıdaki parametreleri temel alınarak alınır yedekleme kümelerini filtreleyebilirsiniz:

* **Aygıt** – yedekleme kümesi oluşturulduğu aygıt.
* **İlke veya birime yedekleme** – yedekleme İlkesi veya bu yedekleme kümesiyle ilişkili birim.
* **Başlangıç ve bitiş** – yedekleme kümesi oluşturduğunuzda tarih ve saat aralığı.

Filtrelenmiş yedekleme kümelerini, ardından aşağıdaki özniteliklerini temel alarak tabloda verilmiştir:

* **Ad** – yedekleme İlkesi veya yedekleme kümesiyle ilişkili birim adı.
* **Boyutu** – yedekleme kümesi gerçek boyutu.
* **Oluşturma tarihi** – yedeklemelerin ne zaman oluşturulduğu tarih ve saat. 
* **Tür** – yedekleme kümelerini yerel anlık görüntüleri olması veya Bulut anlık görüntüler. Bir bulut anlık görüntüsü bulutta bulunan birim verilerin yedeğini başvuruyor ancak yedekleme verilerinizin cihazda yerel olarak depolanan tüm birim yerel bir anlık görüntüdür. Bulut anlık görüntüleri veri dayanıklılığı için seçilen ancak yerel anlık görüntüleri daha hızlı erişim sağlar.
* **Başlatan** – yedeklemeleri otomatik olarak bir zamanlamaya göre veya el ile bir kullanıcı tarafından başlatılabilir. Yedeklemeleri zamanlamak için bir yedekleme İlkesi kullanabilirsiniz. Alternatif olarak, kullanabileceğiniz **yedek alın** seçeneği el ile yedekleyin.

## <a name="list-backup-sets-for-a-volume"></a>Liste yedekleme için bir birim ayarlar
Bir birim için tüm yedeklemeler listelemek için aşağıdaki adımları tamamlayın.

#### <a name="to-list-backup-sets"></a>Yedekleme kümelerini listesi
1. StorSimple Yöneticisi hizmet sayfasında, tıklatın **yedekleme kataloğu** sekmesi.
2. Şu şekilde filtre seçimleri:
   
   1. Uygun cihazı seçin.
   2. Aşağı açılan listesinde karşılık gelen yedeklemeler görüntülemek için bir birim seçin.
   3. Zaman aralığı belirtin.
   4. Onay simgesine tıklayarak ![Onay simgesi](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) Bu sorguyu yürütmek için.
      
      Seçili birimle ilişkilendirilen yedeklemeler yedekleme kümelerini listesinde görünmesi gerekir.

## <a name="select-a-backup-set"></a>Bir yedekleme kümesi seçin
Bir birim veya yedekleme ilkesi için bir yedek seçmek için aşağıdaki adımları tamamlayın.

#### <a name="to-select-a-backup-set"></a>Bir yedekleme kümesi seçmek için
1. StorSimple Yöneticisi hizmet sayfasında, tıklatın **yedekleme kataloğu** sekmesi.
2. Şu şekilde filtre seçimleri:
   
   1. Uygun cihazı seçin.
   2. Aşağı açılan listesinde, seçmek istediğiniz yedekleme için birim veya yedekleme ilkesini seçin.
   3. Zaman aralığı belirtin.
   4. Onay simgesine tıklayarak ![Onay simgesi](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) Bu sorguyu yürütmek için.
      
      Yedekleme seçili birimle ilişkilendirilen veya yedekleme İlkesi yedekleme kümelerini listesinde görüntülenmelidir.
3. Seçin ve bir yedekleme kümesi'ni genişletin. **Geri** ve **silmek** Seçenekler sayfasının en altında görüntülenir. Seçtiğiniz yedekleme kümesinde aşağıdaki işlemlerden birini gerçekleştirebilirsiniz.

## <a name="delete-a-backup-set"></a>Bir yedekleme kümesi Sil
Kendisiyle ilişkili verileri tutmak istediğiniz zaman bir yedekleme silin. Bir yedekleme kümesi silmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-delete-a-backup-set"></a>Bir yedekleme kümesi silmek için
1. StorSimple Yöneticisi hizmet sayfasında, tıklatın **yedekleme kataloğu sekmesini**.
2. Şu şekilde filtre seçimleri:
   
   1. Uygun cihazı seçin.
   2. Aşağı açılan listesinde, seçmek istediğiniz yedekleme için birim veya yedekleme ilkesini seçin.
   3. Zaman aralığı belirtin.
   4. Onay simgesine tıklayarak ![Onay simgesi](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) Bu sorguyu yürütmek için.
      
      Yedekleme seçili birimle ilişkilendirilen veya yedekleme İlkesi yedekleme kümelerini listesinde görüntülenmelidir.
3. Seçin ve bir yedekleme kümesi'ni genişletin. **Geri** ve **silmek** Seçenekler sayfasının en altında görüntülenir. **Sil**'e tıklayın.
4. Silme işlemi devam ediyor ve ne zaman başarıyla tamamlandıktan olduğunda size bildirilecek. Silme işlemini gerçekleştirdikten sonra bu sayfada sorguyu yenileyin. Silinen yedekleme kümesi artık yedekleme kümelerini listesinde görünür.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [Cihazınızı yedekleme kümesinden geri yüklemek için yedekleme Kataloğu'nu kullanma](storsimple-restore-from-backup-set.md).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

