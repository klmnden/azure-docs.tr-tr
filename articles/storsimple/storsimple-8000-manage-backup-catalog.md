---
title: StorSimple yedekleme kataloğunu yönetme | Microsoft Docs
description: StorSimple cihaz Yöneticisi hizmeti yedekleme Kataloğu sayfasına listesinde seçin ve yedekleme kümelerini silmek için nasıl kullanılacağını açıklar.
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
ms.openlocfilehash: 07d9e03f1631ebce88a7a7c2e33be62f21dda522
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60319692"
---
# <a name="use-the-storsimple-device-manager-service-to-manage-your-backup-catalog"></a>StorSimple cihaz Yöneticisi hizmeti, yedekleme kataloğunu yönetmek için kullanın
## <a name="overview"></a>Genel Bakış
StorSimple cihaz Yöneticisi hizmeti **yedekleme kataloğunu** dikey penceresinde el ile veya zamanlanmış yedeklemeler alındığında, oluşturulan tüm yedekleme kümelerini görüntüler. Bir yedekleme İlkesi ya da bir birim seçin için tüm yedeklemeler listelemek veya yedekleri silmek için bu sayfayı kullanın veya bir yedeği geri yüklemek veya bir birimi kopyalama için kullanın.

Bu öğreticide, liste, seçin ve bir yedekleme kümesi silme açıklanmaktadır. Cihazınızı yedekten geri yükleme konusunda bilgi almak için Git [Cihazınızı bir yedekleme kümesinden geri](storsimple-8000-restore-from-backup-set-u2.md). Bir birimi kopyalama öğrenmek için Git [bir StorSimple biriminin kopyalama](storsimple-8000-clone-volume-u2.md).

![Yedekleme kataloğu](./media/storsimple-8000-manage-backup-catalog/bucatalog.png) 

**Yedekleme kataloğunu** dikey pencere, yedekleme daraltmak için bir sorgu kümesi seçimi sağlar. Aşağıdaki parametreleri temel alarak alınır yedek kümelerini filtreleyebilirsiniz:

* **Cihaz** – cihaz yedekleme kümesi oluşturuldu.
* **Yedekleme İlkesi veya birim** – Bu yedekleme kümesiyle ilişkili birim ve yedekleme ilkesi.
* **Başlangıç ve bitiş** – yedekleme kümesi oluşturulduğunda tarih ve saat aralığı.

Filtrelenmiş yedek kümelerini, ardından aşağıdaki özniteliklere bağlı tabloda verilmiştir:

* **Adı** – yedekleme kümesiyle ilişkili birim ve yedekleme ilkesi adı.
* **Boyutu** – yedekleme kümesi gerçek boyutu.
* **Oluşturulma tarihi** – yedeklemelerin ne zaman oluşturulduğu tarih ve saat. 
* **Tür** – kümeleri yedekleme yerel anlık görüntüler olması veya Bulut anlık görüntüleri. Birim verilerinin bulutta bulunan bir bulut anlık görüntüsünü başvuruyor ancak bir yedeğini cihaz üzerinde yerel olarak depolanan tüm birim verilerinizi yerel anlık görüntüsüdür. Veri dayanıklılığı için bulut anlık görüntüleri seçilir ancak yerel anlık görüntüleri daha hızlı erişim sağlar.
* **Başlatan** – yedeklemeleri otomatik olarak bir zamanlamaya göre veya el ile bir kullanıcı tarafından başlatılabilir. Yedeklemeleri zamanlamak için bir yedekleme İlkesi'ni kullanabilirsiniz. Alternatif olarak, **yedek Al** seçeneği el ile yedekleyin.

## <a name="list-backup-sets-for-a-backup-policy"></a>Liste yedekleme için bir yedekleme İlkesi ayarlar.
Bir yedekleme ilkesi için tüm yedeklemeler listelemek için aşağıdaki adımları tamamlayın.

#### <a name="to-list-backup-sets"></a>Yedekleme kümelerini listesi
1. ' A tıklayın ve StorSimple cihaz Yöneticisi hizmetinize gidin **yedekleme kataloğu**.

2. Filtre seçimleri şu şekilde:
   
   1. Zaman aralığını belirtin.
   2. Uygun bir cihaz seçin.
   3. Filtre ölçütü **yedekleme İlkesi** karşılık gelen yedeklemeler görüntülemek için.
   3. Yedekleme İlkesi açılır listeden seçin **tüm** tüm yedeklemeler seçilen cihazda görüntülemek için.
   4. Tıklayın **Uygula** bu sorguyu yürütmek için.
      
      Seçili yedekleme ilkesiyle ilişkili yedeklemelerin yedekleme ayarları listesinde görüntülenmelidir.

      ![Yedekleme Kataloğu'na gidin](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

## <a name="select-a-backup-set"></a>Yedek kümesi seçin
Bir birim veya yedekleme ilkesi için bir yedek seçmek için aşağıdaki adımları tamamlayın.

#### <a name="to-select-a-backup-set"></a>Bir yedekleme kümesi seçin
1. ' A tıklayın ve StorSimple cihaz Yöneticisi hizmetinize gidin **yedekleme kataloğu**.
2. Filtre seçimleri şu şekilde:
   
   1. Zaman aralığını belirtin. 
   2. Uygun bir cihaz seçin. 
   3. Birim veya yedekleme ilkesi seçmek için istediğiniz yedekleme için filtreleyin.
   4. Tıklayın **Uygula** bu sorguyu yürütmek için.
      
      Yedeklemeleri seçili birimle ilişkilendirilen veya yedekleme İlkesi yedekleme ayarları listesinde görüntülenmelidir.

      ![Yedekleme Kataloğu'na gidin](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. Seçin ve bir yedek kümesi genişletin. Artık, içerdiği birimler tarafından ayrılmış yedek kümelerini görebilirsiniz. **Geri** ve **Sil** yedekleme kümesi için bağlam menüsünü (sağ tıklama) seçenekleri kullanılabilir. Seçtiğiniz yedekleme kümesinde aşağıdaki işlemlerden birini gerçekleştirebilirsiniz.

    ![Yedekleme Kataloğu'na gidin](./media/storsimple-8000-manage-backup-catalog/bucatalog2.png)

## <a name="delete-a-backup-set"></a>Yedek kümesini silme
Artık onunla ilişkili verileri korumak istediğinizde, bir yedekleme silin. Bir yedekleme kümesi silmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-delete-a-backup-set"></a>Bir yedekleme kümesi silinemedi
 ' A tıklayın ve StorSimple cihaz Yöneticisi hizmetinize gidin **yedekleme kataloğu**.
1. Filtre seçimleri şu şekilde:
   
   1. Zaman aralığını belirtin. 
   2. Uygun bir cihaz seçin. 
   3. Birim veya yedekleme ilkesi seçmek için istediğiniz yedekleme için filtreleyin.
   4. Tıklayın **Uygula** bu sorguyu yürütmek için.
      
      Yedeklemeleri seçili birimle ilişkilendirilen veya yedekleme İlkesi yedekleme ayarları listesinde görüntülenmelidir.

      ![Yedekleme Kataloğu'na gidin](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

1. Seçin ve bir yedek kümesi genişletin. Artık, içerdiği birimler tarafından ayrılmış yedek kümelerini görebilirsiniz. **Geri** ve **Sil** yedekleme kümesi için bağlam menüsünü (sağ tıklama) seçenekleri kullanılabilir. Seçili yedekleme kümesini sağ tıklatın ve bağlam menüsünden seçin **Sil**.

    ![Yedekleme Kataloğu'na gidin](./media/storsimple-8000-manage-backup-catalog/bucatalog3.png)

1. Onayınız istendiğinde görüntülenen bilgileri gözden geçirin ve tıklayın **Sil**. Seçilen yedeğin kalıcı olarak silinir.

    ![Yedekleme Kataloğu'na gidin](./media/storsimple-8000-manage-backup-catalog/bucatalog4.png)  

1. Silme işlemi ilerleme durumu ve ne zaman başarıyla tamamlandıktan olduğunda size bildirilir. Silme işlemi tamamlandıktan sonra bu sayfadaki sorguyu yenileyin. Silinmiş yedekleme kümesi artık yedekleme kümeleri listesinde görünür.

    ![Yedekleme Kataloğu'na gidin](./media/storsimple-8000-manage-backup-catalog/bucatalog7.png)

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [Cihazınızı bir yedekleme kümesinden geri yüklemek için yedekleme kataloğunu kullanma](storsimple-8000-restore-from-backup-set-u2.md).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

