---
title: StorSimple yedekleme kataloğu yönetme | Microsoft Docs
description: StorSimple cihaz Yöneticisi hizmeti yedekleme kataloğu sayfası listesinde seçin ve yedekleme kümelerini silmek için nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: ac2577c6cd350d6d437d55e61ec73d954cb24893
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23874930"
---
# <a name="use-the-storsimple-device-manager-service-to-manage-your-backup-catalog"></a>Yedekleme kataloğu yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma
## <a name="overview"></a>Genel Bakış
StorSimple cihaz Yöneticisi hizmeti **yedekleme kataloğu** dikey penceresinde el ile veya zamanlanmış yedeklemeler durumdayken, oluşturulan tüm yedekleme kümelerini görüntüler. Bir yedekleme İlkesi ya da bir birim seçin için tüm yedeklemeler listelemek veya yedekleri de silmek için bu sayfayı kullanın ya da bir yedeği geri yüklemek veya bir birim kopyalamak için kullanın.

Bu öğretici, liste, seçin ve bir yedekleme kümesi silme açıklanmaktadır. Cihazınızı yedekten geri yükleme konusunda bilgi almak için şu adrese gidin [Cihazınızı yedekleme kümesinden geri](storsimple-8000-restore-from-backup-set-u2.md). Birim kopyalama öğrenmek için şu adrese gidin [bir StorSimple biriminin kopyalama](storsimple-8000-clone-volume-u2.md).

![Yedekleme kataloğu](./media/storsimple-8000-manage-backup-catalog/bucatalog.png) 

**Yedekleme kataloğu** dikey sağlar yedekleme daraltmak için bir sorgu seçimi ayarlayın. Aşağıdaki parametreleri temel alınarak alınır yedekleme kümelerini filtreleyebilirsiniz:

* **Aygıt** – yedekleme kümesi oluşturulduğu aygıt.
* **Yedekleme İlkesi veya birim** – yedekleme İlkesi veya bu yedekleme kümesiyle ilişkili birim.
* **Başlangıç ve bitiş** – yedekleme kümesi oluşturduğunuzda tarih ve saat aralığı.

Filtrelenmiş yedekleme kümelerini, ardından aşağıdaki özniteliklerini temel alarak tabloda verilmiştir:

* **Ad** – yedekleme İlkesi veya yedekleme kümesiyle ilişkili birim adı.
* **Boyutu** – yedekleme kümesi gerçek boyutu.
* **Oluşturma tarihi** – yedeklemelerin ne zaman oluşturulduğu tarih ve saat. 
* **Tür** – yedekleme kümelerini yerel anlık görüntüleri olması veya Bulut anlık görüntüler. Bir bulut anlık görüntüsü bulutta bulunan birim verilerin yedeğini başvuruyor ancak yedekleme verilerinizin cihazda yerel olarak depolanan tüm birim yerel bir anlık görüntüdür. Bulut anlık görüntüleri veri dayanıklılığı için seçilen ancak yerel anlık görüntüleri daha hızlı erişim sağlar.
* **Başlatan** – yedeklemeleri otomatik olarak bir zamanlamaya göre veya el ile bir kullanıcı tarafından başlatılabilir. Yedeklemeleri zamanlamak için bir yedekleme İlkesi kullanabilirsiniz. Alternatif olarak, kullanabileceğiniz **yedek alın** seçeneği el ile yedekleyin.

## <a name="list-backup-sets-for-a-backup-policy"></a>Liste yedekleme için bir yedekleme İlkesi ayarlar
Bir yedekleme ilkesi için tüm yedeklemeler listelemek için aşağıdaki adımları tamamlayın.

#### <a name="to-list-backup-sets"></a>Yedekleme kümelerini listesi
1. ' I tıklatın ve StorSimple cihaz yöneticinize hizmeti Git **yedekleme kataloğu**.

2. Şu şekilde filtre seçimleri:
   
   1. Zaman aralığı belirtin.
   2. Uygun cihazı seçin.
   3. Filtre ölçütü **yedekleme İlkesi** karşılık gelen yedeklemeler görüntülemek için.
   3. Yedekleme İlkesi açılır listeden seçin **tüm** seçili cihazda tüm yedeklemeler görüntülemek için.
   4. Tıklatın **Uygula** bu sorguyu yürütmek için.
      
      Seçili yedekleme ilkesiyle ilişkili yedeklemeler yedekleme kümelerini listesinde görünmesi gerekir.

      ![Yedekleme Kataloğu'na gidin](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

## <a name="select-a-backup-set"></a>Bir yedekleme kümesi seçin
Bir birim veya yedekleme ilkesi için bir yedek seçmek için aşağıdaki adımları tamamlayın.

#### <a name="to-select-a-backup-set"></a>Bir yedekleme kümesi seçmek için
1. ' I tıklatın ve StorSimple cihaz yöneticinize hizmeti Git **yedekleme kataloğu**.
2. Şu şekilde filtre seçimleri:
   
   1. Zaman aralığı belirtin. 
   2. Uygun cihazı seçin. 
   3. Seçmek istediğiniz yedekleme için birim veya yedekleme İlkesi tarafından filtre.
   4. Tıklatın **Uygula** bu sorguyu yürütmek için.
      
      Yedekleme seçili birimle ilişkilendirilen veya yedekleme İlkesi yedekleme kümelerini listesinde görüntülenmelidir.

      ![Yedekleme Kataloğu'na gidin](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. Seçin ve bir yedekleme kümesi'ni genişletin. Şimdi onu içeren birimlerle ayrıntılarıyla yedekleme kümelerini görebilirsiniz. **Geri** ve **silmek** yedekleme kümesi bağlam menüsünü (sağ tıklatma) aracılığıyla seçenekleri mevcuttur. Seçtiğiniz yedekleme kümesinde aşağıdaki işlemlerden birini gerçekleştirebilirsiniz.

    ![Yedekleme Kataloğu'na gidin](./media/storsimple-8000-manage-backup-catalog/bucatalog2.png)

## <a name="delete-a-backup-set"></a>Bir yedekleme kümesi Sil
Kendisiyle ilişkili verileri tutmak istediğiniz zaman bir yedekleme silin. Bir yedekleme kümesi silmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-delete-a-backup-set"></a>Bir yedekleme kümesi silmek için
 ' I tıklatın ve StorSimple cihaz yöneticinize hizmeti Git **yedekleme kataloğu**.
2. Şu şekilde filtre seçimleri:
   
   1. Zaman aralığı belirtin. 
   2. Uygun cihazı seçin. 
   3. Seçmek istediğiniz yedekleme için birim veya yedekleme İlkesi tarafından filtre.
   4. Tıklatın **Uygula** bu sorguyu yürütmek için.
      
      Yedekleme seçili birimle ilişkilendirilen veya yedekleme İlkesi yedekleme kümelerini listesinde görüntülenmelidir.

      ![Yedekleme Kataloğu'na gidin](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. Seçin ve bir yedekleme kümesi'ni genişletin. Şimdi onu içeren birimlerle ayrıntılarıyla yedekleme kümelerini görebilirsiniz. **Geri** ve **silmek** yedekleme kümesi bağlam menüsünü (sağ tıklatma) aracılığıyla seçenekleri mevcuttur. Seçili yedekleme kümesine sağ tıklatın ve bağlam menüsünden seçin **silmek**.

    ![Yedekleme Kataloğu'na gidin](./media/storsimple-8000-manage-backup-catalog/bucatalog3.png)

4. Onaylamanız istendiğinde, görüntülenen bilgileri gözden geçirin ve tıklatın **silmek**. Seçili yedekleme kalıcı olarak silinir.

    ![Yedekleme Kataloğu'na gidin](./media/storsimple-8000-manage-backup-catalog/bucatalog4.png)  

5. Silme işlemi devam ediyor ve ne zaman başarıyla tamamlandıktan olduğunda size bildirilecek. Silme işlemini gerçekleştirdikten sonra bu sayfada sorguyu yenileyin. Silinen yedekleme kümesi artık yedekleme kümelerini listesinde görünür.

    ![Yedekleme Kataloğu'na gidin](./media/storsimple-8000-manage-backup-catalog/bucatalog7.png)

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [Cihazınızı yedekleme kümesinden geri yüklemek için yedekleme Kataloğu'nu kullanma](storsimple-8000-restore-from-backup-set-u2.md).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

