---
title: Bir StorSimple biriminin yedekten geri | Microsoft Docs
description: "StorSimple Yöneticisi hizmet yedekleme kataloğu sayfasında yedekleme kümesinden StorSimple birimini geri için nasıl kullanılacağını açıklar."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: b979782e-3184-4465-ad5f-e4516a5885d2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: 0ed433ba93cb08742e3f0b09cc7acaa2cd62461e
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="restore-a-storsimple-volume-from-a-backup-set"></a>Bir yedeklemek kümesinden StorSimple birimini geri
> [!NOTE]
> StorSimple için Klasik portalı kullanım dışıdır. StorSimple cihaz yöneticileri yeni Azure portalına kullanımdan zamanlamaya göre otomatik olarak taşır. Bir e-posta ve bu taşıma için portal bir bildirim alırsınız. Bu belgede ayrıca yakında kullanımdan kaldırılacaktır. Taşıma hakkında herhangi bir sorunuz için bkz: [SSS: Azure portalına taşıma](storsimple-8000-move-azure-portal-faq.md).

[!INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Genel Bakış
**Yedekleme kataloğu** sayfası el ile veya otomatik yedeklemeler durumdayken, oluşturulan tüm yedekleme kümelerini görüntüler. Bir yedekleme İlkesi ya da bir birim seçin için tüm yedeklemeler listelemek veya yedekleri de silmek için bu sayfayı kullanın ya da bir yedeği geri yüklemek veya bir birim kopyalamak için kullanın.

 ![Yedekleme kataloğu sayfası](./media/storsimple-restore-from-backup-set/HCS_BackupCatalog.png)

Bu öğretici nasıl kullanılacağı açıklanmaktadır **yedekleme kataloğu** Cihazınızı bir birimde yedekleme kümesinden geri yüklemek için sayfa.

## <a name="how-to-use-the-backup-catalog"></a>Yedekleme kataloğunu kullanma
**Yedekleme kataloğu** sayfası, yedekleme daraltmaya yardımcı olacak bir sorgu olarak seçimi sağlar. Aşağıdaki parametreleri temel alınarak alınır yedekleme kümelerini filtreleyebilirsiniz:

* **Aygıt** – yedekleme kümesi oluşturulduğu aygıt.
* **Yedekleme İlkesi** veya **birim** – yedekleme İlkesi veya bu yedekleme kümesiyle ilişkili birim.
* **Gelen** ve **için** – yedekleme kümesi oluşturduğunuzda tarih ve saat aralığı.

Filtrelenmiş yedekleme kümelerini, ardından aşağıdaki özniteliklerini temel alarak tabloda verilmiştir:

* **Ad** – yedekleme İlkesi veya yedekleme kümesiyle ilişkili birim adı.
* **Boyutu** – yedekleme kümesi gerçek boyutu.
* **Oluşturulan** – yedeklemelerin ne zaman oluşturulduğu tarih ve saat. 
* **Tür** – yedekleme kümelerini yerel anlık görüntüleri olması veya Bulut anlık görüntüler. Bir bulut anlık görüntüsü bulutta bulunan birim verilerin yedeğini başvuruyor ancak yedekleme verilerinizin cihazda yerel olarak depolanan tüm birim yerel bir anlık görüntüdür. Bulut anlık görüntüleri veri dayanıklılığı için seçilen ancak yerel anlık görüntüleri daha hızlı erişim sağlar.
* **Tarafından başlatılan** – yedeklemeleri otomatik olarak bir zamanlamaya göre veya el ile bir kullanıcı tarafından başlatılabilir. (Bir yedekleme İlkesi yedeklemeleri zamanlamak için kullanabilirsiniz. Alternatif olarak, kullanabileceğiniz **yedek alın** etkileşimli bir yedek almak için seçeneği.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>StorSimple birim bir yedekten geri yükleme
Kullanabileceğiniz **yedekleme kataloğu** sayfasında StorSimple biriminiz belirli bir yedekten geri yükleyin. 

> [!WARNING]
> Bir yedekten geri yükleme, var olan birimler yedekten yerini alır. Bu, yedeğin alındığı sonra yazıldı herhangi bir veri kaybına neden olabilir.
> 
> 

Bir birime geri başlatmadan önce birim çevrimdışı olduğundan emin olun. Birimin çevrimdışı konakta ilk gerçekleştirmeniz gerekir ve ardından cihaz. Adımları [bir birim çevrimdışına](storsimple-manage-volumes.md#take-a-volume-offline). Bir birim yedekleme kümesinden geri yüklemek için aşağıdaki adımları gerçekleştirin.

### <a name="to-restore-from-a-backup-set"></a>Bir yedekleme kümesinden geri yüklemek için
1. StorSimple Yöneticisi hizmet sayfasında, tıklatın **yedekleme kataloğu** sekmesi.
   
    ![Yedekleme kataloğu](./media/storsimple-restore-from-backup-set/HCS_Restore.png)
2. Bir yedekleme kümesi aşağıdaki gibi seçin:
   
   1. Uygun cihazı seçin.
   2. Aşağı açılan listesinde, seçmek istediğiniz yedekleme için birim veya yedekleme ilkesini seçin.
   3. Zaman aralığı belirtin.
   4. Onay simgesine tıklayarak ![onay simgesi](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png) Bu sorguyu yürütmek için.
      
      Yedekleme seçili birimle ilişkilendirilen veya yedekleme İlkesi yedekleme kümelerini listesinde görüntülenmelidir.
3. İlişkili birimleri görüntülemek için yedekleme kümesini genişletin. Geri yüklemeden önce bu birimleri ana bilgisayar ve aygıt çevrimdışına alınması gerekir. Adımları [bir birim çevrimdışına](storsimple-manage-volumes.md#take-a-volume-offline).
   
   > [!IMPORTANT]
   > Çevrimdışı birimler cihazda olabilmesi birimlerde çevrimdışı konak ilk olarak, gerçekleştirdiğinizden emin olun. Konakta birimler çevrimdışı almazsanız, olası veri bozulmasına yol açabilir.
   > 
   > 
4. Bir yedekleme kümesi seçin. Tıklatın **geri** sayfanın sonundaki.
5. Onayınız istenir. 
   
    ![Onay sayfası](./media/storsimple-restore-from-backup-set/HCS_ConfirmRestore.png)
6. Geri yükleme bilgilerini gözden geçirin ve onay simgesine ![onay simgesi](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png). Bu erişerek görüntüleyebileceğiniz bir geri yükleme işi başlatır **işleri** sayfası. 
7. Geri yükleme tamamlandıktan sonra birimlerinizi içeriğini yedekleme birimlerden değiştirilir olduğunu doğrulayabilirsiniz.

![Video var](./media/storsimple-restore-from-backup-set/Video_icon.png) **Video var**

Nasıl kopya kullanın ve geri yükleme silinmiş dosyaları kurtarmak için StorSimple özellikleri gösteren bir video izlemek için tıklatın [burada](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [yönetmek StorSimple birimlerini](storsimple-manage-volumes.md).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

