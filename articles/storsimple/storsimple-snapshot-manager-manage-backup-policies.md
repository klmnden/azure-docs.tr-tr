---
title: Yedekleme ilkelerini StorSimple Snapshot Manager | Microsoft Docs
description: StorSimple Snapshot Manager MMC ek bileşeninde oluşturmak ve zamanlanmış yedeklemeleri denetleyen yedekleme ilkelerini yönetmek için nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: ''
ms.assetid: 04415d0b-42f0-4737-8afa-257fb2dbe5d0
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: 7dac26b058b959011e38b4373369b8a1115d2705
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64687270"
---
# <a name="use-storsimple-snapshot-manager-to-create-and-manage-backup-policies"></a>Yedekleme ilkeleri oluşturma ve yönetme için StorSimple Snapshot Manager'ı kullanın
## <a name="overview"></a>Genel Bakış
Bir yedekleme ilkesine birim verilerinizi yerel olarak veya bulutta yedeklemek için bir zamanlama oluşturur. Bir yedekleme ilkesi oluşturduğunuzda, bekletme ilkesi belirtebilirsiniz. (En fazla 64 anlık görüntüleri tutabilirsiniz.) Yedekleme ilkeleri hakkında daha fazla bilgi için bkz. [yedekleme türleri](storsimple-what-is-snapshot-manager.md#backup-types-and-backup-policies) içinde [StorSimple 8000 serisi: bir karma bulut çözümü](storsimple-overview.md).

Bu öğreticide, aşağıdaki işlemlerin nasıl yapılacağı açıklanmaktadır:

* Bir yedekleme ilkesi oluşturma
* Yedekleme ilkesini Düzenle
* Yedekleme ilkesini silme

## <a name="create-a-backup-policy"></a>Bir yedekleme ilkesi oluşturma
Yeni bir yedekleme ilkesi oluşturmak için aşağıdaki yordamı kullanın.

#### <a name="to-create-a-backup-policy"></a>Bir yedekleme ilkesi oluşturmak için
1. StorSimple Snapshot Manager'ı başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde sağ **yedekleme ilkeleri**, tıklatıp **yedekleme İlkesi Oluştur**.

    ![Bir yedekleme ilkesi oluşturma](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    **Bir ilke oluşturmak** iletişim kutusu görüntülenir.

    ![İlke - Genel sekmesi oluştur](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)
3. Üzerinde **genel** sekmesinde, aşağıdaki bilgileri doldurun:

   1. İçinde **adı** metin kutusunda, ilke için bir ad yazın.
   2. İçinde **birim grubu** birim grup adı metin kutusunda, ilke ile ilişkili.
   3. Şunlardan birini seçin **yerel anlık görüntü** veya **bulut anlık görüntü**.
   4. Korumak için anlık görüntü sayısını seçin. Seçerseniz **tüm**, 64 anlık görüntüleri olur (maksimum) korunur.
4. Tıklayın **zamanlama** sekmesi.

    ![İlke - zamanlama sekmesi oluştur](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)
5. Üzerinde **zamanlama** sekmesinde, aşağıdaki bilgileri doldurun:

   1. Tıklayın **etkinleştirme** sonraki yedekleme işini zamanlamak için onay kutusu.
   2. Altında **ayarları**seçin **bir kez**, **günlük**, **haftalık**, veya **aylık**.
   3. İçinde **Başlat** metin kutusuna Takvim simgesine tıklayın ve bir başlangıç tarihini seçin.
   4. Altında **Gelişmiş ayarlar**, isteğe bağlı bir yineleme zamanlamaları ve bitiş tarihi ayarlayabilirsiniz.
   5. **Tamam**'ı tıklatın.

Yedekleme ilkesini oluşturduktan sonra aşağıdaki bilgiler gözükür **sonuçları** bölmesi:

* **Adı** – yedekleme ilkesi adı.
* **Tür** – yerel anlık görüntü veya Bulut anlık görüntüsü.
* **Birim grubu** -ilkeyle ilişkili birim grubu.
* **Bekletme** – anlık görüntü sayısını korunur; sınır 64'tür.
* **Oluşturulan** – bu ilkenin oluşturulduğu tarih.
* **Etkin** : ilke şu anda etkin olup: **Doğru** yürürlükte; olduğunu gösterir **False** etkin olmadığını gösterir.

## <a name="edit-a-backup-policy"></a>Yedekleme ilkesini Düzenle
Mevcut bir yedekleme ilkesi düzenlemek için aşağıdaki yordamı kullanın.

#### <a name="to-edit-a-backup-policy"></a>Bir yedekleme ilkesi düzenlemek için
1. StorSimple Snapshot Manager'ı başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde tıklayın **yedekleme ilkeleri** düğümü. Yedekleme ilkelerini görünür **sonuçları** bölmesi.
3. Düzenleyin ve ardından istediğiniz ilkesine sağ **Düzenle**.

    ![Yedekleme ilkesini Düzenle](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png)
4. Zaman **bir ilke oluşturmak** penceresi görüntülenirse, değişikliklerinizi girin ve ardından **Tamam**.

## <a name="delete-a-backup-policy"></a>Yedekleme ilkesini silme
Bir yedekleme İlkesi silmek için aşağıdaki yordamı kullanın.

#### <a name="to-delete-a-backup-policy"></a>Bir yedekleme İlkesi silinemedi
1. StorSimple Snapshot Manager'ı başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde tıklayın **yedekleme ilkeleri** düğümü. Yedekleme ilkelerini görünür **sonuçları** bölmesi.
3. Silin ve ardından istediğiniz yedekleme ilkesine sağ **Sil**.
4. Onay iletisi göründüğünde tıklayın **Evet**.

    ![Yedek İlkesi onayını Sil](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [StorSimple çözümünüzü yönetmek için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-admin.md).
* Bilgi edinmek için nasıl [yedekleme işleri görüntüleme ve yönetme için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-manage-backup-jobs.md).
