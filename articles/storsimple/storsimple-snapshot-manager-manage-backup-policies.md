---
title: StorSimple Snapshot Manager yedekleme ilkeleri | Microsoft Docs
description: StorSimple Snapshot Manager MMC ek bileşenini oluşturup zamanlanmış yedeklemeler kontrol yedekleme ilkelerini yönetmek için nasıl kullanılacağını açıklar.
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
ms.openlocfilehash: 218c89e403673c16c72da95aa2c1d685bbed5a86
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23875476"
---
# <a name="use-storsimple-snapshot-manager-to-create-and-manage-backup-policies"></a>StorSimple Snapshot Manager oluşturmak ve yedekleme ilkelerini yönetmek için kullanın
## <a name="overview"></a>Genel Bakış
Bir yedekleme İlkesi birim verilerini yerel olarak veya bulutta yedekleme için bir zamanlama oluşturur. Bir yedekleme ilkesi oluşturduğunuzda, bir bekletme ilkesi de belirtebilirsiniz. (En fazla 64 anlık görüntüleri tutabilirsiniz.) Yedekleme ilkeleri hakkında daha fazla bilgi için bkz: [yedekleme türleri](storsimple-what-is-snapshot-manager.md#backup-types-and-backup-policies) içinde [StorSimple 8000 serisi: karma bulut çözümünün](storsimple-overview.md).

Bu öğretici açıklar nasıl yapılır:

* Bir yedekleme İlkesi Oluştur
* Yedekleme ilkesini Düzenle
* Bir yedekleme ilkesi silme

## <a name="create-a-backup-policy"></a>Bir yedekleme İlkesi Oluştur
Yeni bir yedekleme ilkesi oluşturmak için aşağıdaki yordamı kullanın.

#### <a name="to-create-a-backup-policy"></a>Bir yedekleme ilkesi oluşturmak için
1. StorSimple anlık görüntü Yöneticisi'ni başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde sağ **yedekleme ilkeleri**, tıklatıp **yedekleme İlkesi Oluştur**.

    ![Bir yedekleme İlkesi Oluştur](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    **Bir ilke oluşturmak** iletişim kutusu görüntülenir.

    ![Bir ilke - Genel sekmesi oluştur](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)
3. Üzerinde **genel** sekmesinde, aşağıdaki bilgileri tamamlayın:

   1. İçinde **adı** metin kutusuna, ilke için bir ad yazın.
   2. İçinde **birim grubu** birim grubunun adı metin kutusuna ilke ile ilişkili.
   3. Şunlardan birini seçin **yerel anlık görüntü** veya **bulut anlık görüntü**.
   4. Korumak için anlık görüntü sayısını seçin. Seçerseniz **tüm**, 64 anlık görüntüleri olur (maksimum) korunur.
4. Tıklatın **zamanlama** sekmesi.

    ![Bir ilke - zamanlama sekmesi oluştur](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)
5. Üzerinde **zamanlama** sekmesinde, aşağıdaki bilgileri tamamlayın:

   1. Tıklatın **etkinleştirmek** sonraki yedekleme işini zamanlamak için onay kutusunu.
   2. Altında **ayarları**seçin **bir kez**, **günlük**, **haftalık**, veya **aylık**.
   3. İçinde **Başlat** metin kutusuna takvim simgesini tıklatın ve bir başlangıç tarihi seçin.
   4. Altında **Gelişmiş ayarları**, isteğe bağlı yineleme zamanlamaları ve bitiş tarihi ayarlayabilirsiniz.
   5. **Tamam** düğmesine tıklayın.

Bir yedekleme ilkesi oluşturduktan sonra aşağıdaki bilgileri görünür **sonuçları** bölmesi:

* **Ad** – yedekleme ilkesinin adı.
* **Tür** – yerel anlık görüntü veya Bulut anlık görüntüsü.
* **Birim grubu** – ilkeyle ilişkili birim grubu.
* **Bekletme** – anlık görüntü sayısını tutulur; en fazla 64'tür.
* **Oluşturulan** – bu ilkenin oluşturulduğu tarih.
* **Etkin** – ilkenin şu anda etkin olup olmadığını: **True** yürürlükte; olduğunu gösterir **False** etkin olmadığını gösterir.

## <a name="edit-a-backup-policy"></a>Yedekleme ilkesini Düzenle
Mevcut bir yedekleme ilkesi düzenlemek için aşağıdaki yordamı kullanın.

#### <a name="to-edit-a-backup-policy"></a>Bir yedekleme ilkesi düzenlemek için
1. StorSimple anlık görüntü Yöneticisi'ni başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde tıklatın **yedekleme ilkeleri** düğümü. Tüm yedekleme ilkeleri görünür **sonuçları** bölmesi.
3. Düzenleyin ve ardından istediğiniz ilkesine sağ **Düzenle**.

    ![Yedekleme ilkesini Düzenle](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png)
4. Zaman **bir ilke oluşturmak** penceresi görüntülenir, değişikliklerinizi girin ve ardından **Tamam**.

## <a name="delete-a-backup-policy"></a>Bir yedekleme ilkesi silme
Bir yedekleme İlkesi silmek için aşağıdaki yordamı kullanın.

#### <a name="to-delete-a-backup-policy"></a>Bir yedekleme İlkesi silmek için
1. StorSimple anlık görüntü Yöneticisi'ni başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde tıklatın **yedekleme ilkeleri** düğümü. Tüm yedekleme ilkeleri görünür **sonuçları** bölmesi.
3. Silin ve ardından istediğiniz yedekleme ilkesine sağ **silmek**.
4. Onay iletisi göründüğünde tıklayın **Evet**.

    ![Yedek İlkesi onayını Sil](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [StorSimple çözümünüzün yönetmek için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-admin.md).
* Bilgi edinmek için nasıl [görüntülemek ve yedekleme işlerini yönetmek için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-manage-backup-jobs.md).
