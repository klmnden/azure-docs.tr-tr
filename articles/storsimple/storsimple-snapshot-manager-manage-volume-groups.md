---
title: "StorSimple Snapshot Manager birim grupları | Microsoft Docs"
description: "StorSimple Snapshot Manager MMC ek bileşenini birim grupları oluşturmak ve yönetmek için nasıl kullanılacağını açıklar."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 7a232414-6a28-4b81-bd7b-cf61e28b33d7
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: 6067a88cd42d29c3d2f4b74580095424de77561e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-storsimple-snapshot-manager-to-create-and-manage-volume-groups"></a>Birim grupları oluşturmak ve yönetmek için StorSimple anlık görüntü Yöneticisi'ni kullanın
## <a name="overview"></a>Genel Bakış
Kullanabilirsiniz **birim grupları** düğümde **kapsam** bölmesinde birim grupları, bir birim grubu hakkındaki bilgileri görüntüleyin birimler atamak için yedeklemeleri zamanlamak ve birim grupları düzenleyin.

Birim, ilgili birimlerin yedekleri uygulama tutarlı olduğundan emin olmak için kullanılan havuzları gruplarıdır. Daha fazla bilgi için bkz: [birim ve birim grupları](storsimple-what-is-snapshot-manager.md#volumes-and-volume-groups) ve [Windows birim gölge kopyası hizmeti ile tümleştirme](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).

> [!IMPORTANT]
> * Bir birim grubundaki tüm birimleri, bir tek bulut hizmeti sağlayıcısını gelmesi gerekir.
> * Birim grupları yapılandırdığınızda, Küme Paylaşılan birimleri (CSV) ve CSV olmayan aynı birim grubundaki karışık kullanmayın. StorSimple Snapshot Manager csv ve CSV olmayan bir karışımını aynı anlık desteklemez.

![Birim grupları düğümü](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Volume_groups.png)

**Şekil 1: StorSimple anlık görüntü Yöneticisi birim grupları düğümü** 

Bu öğretici, StorSimple Snapshot Manager için nasıl kullanabileceğiniz açıklanır:

* Birim grupları hakkındaki bilgileri görüntüleme
* Birim grubu oluştur
* Bir birim grubunu yedekleyin
* Bir birim grubu Düzenle
* Bir birim grubu Sil

Tüm bu eylemleri de mevcuttur **Eylemler** bölmesi.

## <a name="view-volume-groups"></a>Birim grupları görüntüle
Tıklatırsanız **birim grupları** düğümü, **sonuçları** bölmesinde her bir birim grubu hakkında aşağıdaki bilgileri gösterir, yaptığınız sütun seçimlerini bağlı olarak. (Sütunları **sonuçları** bölmesinde yapılandırılabilir. Sağ **birimleri** düğümü, select **Görünüm**ve ardından **Sütun Ekle/Kaldır**.)

| Sonuçları sütun | Açıklama |
|:--- |:--- |
| Ad |**Adı** sütun birim grubu adını içerir. |
| Uygulama |**Uygulamaları** sütun Windows konakta VSS yazıcılarının şu anda yüklü ve çalışır durumda sayısını gösterir. |
| Seçildi |**Seçili** sütun birim grubunda bulunan birimleri sayısını gösterir. Sıfır (0), uygulama birimlerin birim grubu ile ilişkili olduğunu gösterir. |
| İçeri aktarılan |**Imported** sütun alınan birimler sayısını gösterir. Ayarlandığında **doğru**, bu sütun bir birim grubu Azure portalından alınan ve StorSimple Snapshot Manager'da oluşturulmadı belirtir. |

> [!NOTE]
> StorSimple Snapshot Manager birim grupları ayrıca görüntülenir **yedekleme ilkeleri** Azure portalında sekmesi.
> 
> 

## <a name="create-a-volume-group"></a>Birim grubu oluştur
Bir birim grubu oluşturmak için aşağıdaki yordamı kullanın.

#### <a name="to-create-a-volume-group"></a>Bir birim grubu oluşturmak için
1. StorSimple anlık görüntü Yöneticisi'ni başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde sağ **birim grupları**ve ardından **birim grubu oluştur**.
   
    ![Birim grubu oluştur](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Create_volume_group.png)
   
    **Bir birim grubu oluşturun** iletişim kutusu görüntülenir.
   
    ![Bir birim grubu iletişim kutusu oluşturma](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_CreateVolumeGroup_dialog.png)
3. Aşağıdaki bilgileri girin:
   
   1. İçinde **adı** kutusuna, yeni birim grubu için benzersiz bir ad yazın.
   2. İçinde **uygulamaları** kutusuna, birim gruba eklemeyi birimleri ile ilişkili uygulama.
      
       **Uygulamaları** VSS yazıcılarının olan ve StorSimple birimlerini kullanan uygulamalar için bunları etkin kutusunu listeler. Yalnızca yazıcı bilmektedir tüm birimleri StorSimple birimlerini varsa bir VSS yazıcının etkin olduğu. Uygulamaları kutusu boş ise, VSS yazıcılarının desteklenen ve Azure StorSimple birimlerini kullanan bir uygulama yüklenmez. (Şu anda Azure StorSimple Microsoft Exchange ve SQL Server destekler.) VSS yazıcılarının hakkında daha fazla bilgi için bkz: [Windows birim gölge kopyası hizmeti ile tümleştirme](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).
      
       Bir uygulama seçerseniz, kendisiyle ilişkilendirilmiş tüm birimleri otomatik olarak seçilir. Belirli bir uygulama ile ilişkili birimler seçerseniz, buna karşılık, uygulama otomatik olarak seçilir **uygulamaları** kutusu. 
   3. İçinde **birimleri** kutusunda, StorSimple birimlerini birim grubuna eklemek için seçin. 
      
      * Tek veya birden çok bölüm birimlerle içerebilir. (Birden çok bölüm birim dinamik diskleri veya temel diskleri birden çok bölüm olabilir.) Birden çok bölüm içeren bir birimdeki tek bir birim olarak kabul edilir. Yalnızca bir bölüm için bir birim grubu eklerseniz, sonuç olarak, diğer tüm bölümler otomatik olarak o birimi gruba aynı anda eklenir. Birden çok bölüm birim bir birim grubuna ekledikten sonra birden çok bölüm birim tek bir birim olarak kabul edilecek şekilde devam eder.
      * Herhangi bir birimi atayarak değil boş birim grupları oluşturabilirsiniz. 
      * Küme Paylaşılan birimleri (CSV) ve CSV olmayan aynı birim grubundaki karışık kullanmayın. StorSimple Snapshot Manager CSV birimleri ve CSV olmayan birimlerin bir karışımını aynı anlık desteklemez.
4. Tıklatın **Tamam** birim grubunu kaydetmek için.

## <a name="back-up-a-volume-group"></a>Bir birim grubunu yedekleyin
Bir birim grubu için aşağıdaki yordamı kullanın.

#### <a name="to-back-up-a-volume-group"></a>Bir birim grubunu yedeklemek için
1. StorSimple anlık görüntü Yöneticisi'ni başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde genişletin **birim grupları** düğümü, bir birim grubu adını sağ tıklatın ve ardından **ele yedekleme**.
   
    ![Birim grubunu hemen yedekleyin](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Take_backup.png)
3. İçinde **ele yedekleme** iletişim kutusunda **yerel anlık görüntü** veya **bulut anlık görüntüsü**ve ardından **oluşturma**.
   
    ![Yedekleme iletişim alın](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_TakeBackup_dialog.png)
4. Yedekleme çalışır durumda olduğunu doğrulamak için Genişlet **işleri** düğümünü ve ardından **çalıştıran**. Yedekleme listelenmelidir.
5. Tamamlanan anlık görüntü görüntülemek üzere genişletme **yedekleme kataloğu** düğümü, birim grubu adını genişletin ve ardından **yerel anlık görüntü** veya **bulut anlık görüntüsü**. Başarıyla tamamlandı, yedekleme listelenir.

## <a name="edit-a-volume-group"></a>Bir birim grubu Düzenle
Bir birim grubu düzenlemek için aşağıdaki yordamı kullanın.

#### <a name="to-edit-a-volume-group"></a>Birim grubu düzenlemek için
1. StorSimple anlık görüntü Yöneticisi'ni başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesini genişletin **birim grupları** düğümü, bir birim grubu adını sağ tıklatın ve ardından **Düzenle**.
3. ** Bir birim grubu oluşturun ** iletişim kutusu görüntülenir. Değiştirebileceğiniz **adı**, **uygulamaları**, ve **birimleri** girişleri.
4. Değişikliklerinizi kaydetmek için **Tamam**’a tıklayın.

## <a name="delete-a-volume-group"></a>Bir birim grubu Sil
Bir birim grubu silmek için aşağıdaki yordamı kullanın. 

> [!WARNING]
> Bu, birim grubu ile ilişkili tüm yedeklemeler de siler.
> 
> 

#### <a name="to-delete-a-volume-group"></a>Bir birim grubu silmek için
1. StorSimple anlık görüntü Yöneticisi'ni başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesini genişletin **birim grupları** düğümü, bir birim grubu adını sağ tıklatın ve ardından **silmek**.
3. **Birim grubunu Sil** iletişim kutusu görüntülenir. Tür **Onayla** metin kutusuna ve ardından **Tamam**.
   
    Listeden silinen birim grubu kaybolduktan **sonuçları** bölmesinde ve o birimi grubuyla ilişkili olan tüm yedeklemeler yedekleme Kataloğu'ndan silinir.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [StorSimple çözümünüzün yönetmek için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-admin.md).
* Bilgi edinmek için nasıl [oluşturmak ve yedekleme ilkeleri yönetmek için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-manage-backup-policies.md).

