---
title: StorSimple Snapshot Manager birim gruplarını | Microsoft Docs
description: StorSimple Snapshot Manager MMC ek bileşeninde birim grupları oluşturmak ve yönetmek için nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: ''
ms.assetid: 7a232414-6a28-4b81-bd7b-cf61e28b33d7
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: e84bc790ac577796e91be010deecc8c5cea1b010
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60303147"
---
# <a name="use-storsimple-snapshot-manager-to-create-and-manage-volume-groups"></a>Birim grupları oluşturmak ve yönetmek için StorSimple Snapshot Manager'ı kullanın
## <a name="overview"></a>Genel Bakış
Kullanabileceğiniz **birim gruplarını** düğümde **kapsam** bölmesinde birim gruplarını, bir birim grubu hakkındaki bilgileri görüntüleme birimler atamak için Yedeklemeler zamanlamak ve birim gruplarını düzenleyin.

Birim, yedeklemeler uygulama ile tutarlı olmasını sağlamak için kullanılan ilişkili birimleri havuzları gruplarıdır. Daha fazla bilgi için [birim ve birim gruplarını](storsimple-what-is-snapshot-manager.md#volumes-and-volume-groups) ve [Windows birim gölge kopyası hizmeti ile tümleştirme](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).

> [!IMPORTANT]
> * Bir birim grubu tüm birimleri, bir tek bulut hizmeti sağlayıcısından gelmelidir.
> * Birim grupları yapılandırdığınızda, Küme Paylaşılan birimleri (CSV) ve Csv'ler aynı birim grubunda olmayan karıştırmayın. StorSimple Snapshot Manager, aynı anlık Csv'leri ve CSV olmayan bir karışımını desteklemez.

![Birim grupları düğümü](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Volume_groups.png)

**Şekil 1: StorSimple Snapshot Manager birim grupları düğümü** 

Bu öğretici için StorSimple anlık görüntü yöneticisini nasıl kullanabileceğinizi açıklar:

* Birim grupları hakkındaki bilgileri görüntüleme
* Bir birim grubu oluşturun
* Bir birim grubunu yedekleme
* Bir birim grubu Düzenle
* Bir birim grubu Sil

Tüm bu eylemler de kullanılabilir olan **eylemleri** bölmesi.

## <a name="view-volume-groups"></a>Birim grupları görüntüleyin
Tıklarsanız **birim gruplarını** düğümünün **sonuçları** sütun seçimlere bağlı olarak, bölmesinde her bir birim grubu hakkında aşağıdaki bilgileri gösterir. (Sütunları **sonuçları** bölmesinde yapılandırılabilir. Sağ **birimleri** düğümünü **görünümü**ve ardından **sütunları Ekle/Kaldır**.)

| Sonuçları sütun | Açıklama |
|:--- |:--- |
| Ad |**Adı** sütun birim grubu adını içerir. |
| Uygulama |**Uygulamaları** sütun Windows konağında VSS yazıcılarının yüklü ve çalışır durumda sayısını gösterir. |
| Seçildi |**Seçili** sütun birim grubunda bulunan birimleri sayısını gösterir. Sıfır (0), uygulama birim grubu birimlerin ile ilişkili olduğunu gösterir. |
| İçeri aktarıldı |**İçeri aktarılan** sütun alınan birimlerin sayısını gösterir. Ayarlandığında **True**, bu sütun bir birim grubu Azure portalından alınan ve StorSimple Snapshot Manager'da oluşturulmamış gösterir. |

> [!NOTE]
> StorSimple Snapshot Manager birim gruplarını da görüntülenir **yedekleme ilkeleri** Azure portalında sekmesi.
> 
> 

## <a name="create-a-volume-group"></a>Bir birim grubu oluşturun
Bir birim grubu oluşturmak için aşağıdaki yordamı kullanın.

#### <a name="to-create-a-volume-group"></a>Bir birim grubu oluşturmak için
1. StorSimple Snapshot Manager'ı başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde sağ **birim gruplarını**ve ardından **birim grubu oluştur**.
   
    ![Birim grubu oluştur](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Create_volume_group.png)
   
    **Bir birim grubu oluştur** iletişim kutusu görüntülenir.
   
    ![Bir birim grubu iletişim kutusu oluşturma](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_CreateVolumeGroup_dialog.png)
3. Aşağıdaki bilgileri girin:
   
   1. İçinde **adı** yeni birim grubu için benzersiz bir ad yazın.
   2. İçinde **uygulamaları** kutusunda birim grubuna ekleyerek birimleri ile ilişkili uygulama.
      
       **Uygulamaları** kutusu listeleri yalnızca StorSimple birimlerini kullanır ve VSS yazıcılarının sahip uygulamalar için etkin. Yalnızca StorSimple birimlerini yazıcı farkında olan tüm birimleri, bir VSS yazıcının etkin olduğu. Ardından uygulamaları kutusu boş olduğunda, Azure StorSimple birimlerini kullanan ve VSS yazıcılarının desteklenen uygulama yüklenmez. (Şu anda Azure StorSimple Microsoft Exchange ve SQL Server destekler.) VSS yazıcılarının hakkında daha fazla bilgi için bkz: [Windows birim gölge kopyası hizmeti ile tümleştirme](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).
      
       Bir uygulama seçtiğinizde, ilişkili tüm birimleri otomatik olarak seçilir. Belirli bir uygulamayla ilişkili birimler seçerseniz, buna karşılık, uygulamayı otomatik olarak seçilir **uygulamaları** kutusu. 
   3. İçinde **birimleri** kutusunda, birim grubuna eklemek için StorSimple birimlerini seçin. 
      
      * Tek veya birden çok bölüme sahip birimler dahil edebilirsiniz. (Birden çok bölüm birim dinamik diskleri veya temel diskleri ile birden çok bölüm olabilir.) Birden fazla bölüm içeren bir birime tek bir birim olarak kabul edilir. Yalnızca bir bölüm için bir birim grubu eklerseniz, sonuç olarak, diğer tüm bölümleri otomatik olarak bu birim gruba aynı anda eklenir. Bir birim grubu için birden çok bölüm birim ekledikten sonra tek bir birim olarak kabul edilmesi birden çok bölüm birim devam eder.
      * Herhangi bir birimi atayarak değil, boş birim gruplarını oluşturabilirsiniz. 
      * Küme Paylaşılan birimleri (CSV) ve Csv'ler aynı birim grubunda olmayan karıştırmayın. StorSimple Snapshot Manager CSV birimleri ve CSV olmayan birimlerin bir karışımı, aynı anlık desteklemez.
4. Tıklayın **Tamam** birim grubunu kaydetmek için.

## <a name="back-up-a-volume-group"></a>Bir birim grubunu yedekleme
Bir birim grubunu yedeklemek için aşağıdaki yordamı kullanın.

#### <a name="to-back-up-a-volume-group"></a>Bir birim grubunu yedeklemek için
1. StorSimple Snapshot Manager'ı başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde genişletin **birim gruplarını** düğümü, bir birim grubu adına sağ tıklayın ve ardından **ele yedekleme**.
   
    ![Birim grubunu hemen yedekleyin](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Take_backup.png)
3. İçinde **ele yedekleme** iletişim kutusunda **yerel anlık görüntü** veya **bulut anlık görüntüsü**ve ardından **Oluştur**.
   
    ![Yedekleme iletişim Al](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_TakeBackup_dialog.png)
4. Backup'ın çalışır durumda olduğunu doğrulamak için genişletin **işleri** düğümünü ve ardından **çalıştıran**. Yedekleme listelenmelidir.
5. Tamamlanan anlık görüntü görüntülemek için genişletin **yedekleme kataloğu** düğümü, birim grup adı'nı genişletin ve ardından **yerel anlık görüntü** veya **bulut anlık görüntüsü**. Yedekleme başarıyla tamamlandıysa listelenir.

## <a name="edit-a-volume-group"></a>Bir birim grubu Düzenle
Bir birim grubu düzenlemek için aşağıdaki yordamı kullanın.

#### <a name="to-edit-a-volume-group"></a>Bir birim grubu düzenlemek için
1. StorSimple Snapshot Manager'ı başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesini genişletin **birim gruplarını** düğümü, bir birim grubu adına sağ tıklayın ve ardından **Düzenle**.
3. ** Bir birim grubu oluşturun ** iletişim kutusu görüntülenir. Değiştirebileceğiniz **adı**, **uygulamaları**, ve **birimleri** girdileri.
4. Değişikliklerinizi kaydetmek için **Tamam**’a tıklayın.

## <a name="delete-a-volume-group"></a>Bir birim grubu Sil
Bir birim grubu silmek için aşağıdaki yordamı kullanın. 

> [!WARNING]
> Bu, birim grubuyla ilişkili olan tüm yedeklemeler de siler.
> 
> 

#### <a name="to-delete-a-volume-group"></a>Bir birim grubu silmek için
1. StorSimple Snapshot Manager'ı başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesini genişletin **birim gruplarını** düğümü, bir birim grubu adına sağ tıklayın ve ardından **Sil**.
3. **Birim grubunu Sil** iletişim kutusu görüntülenir. Tür **Onayla** metin kutusuna ve ardından **Tamam**.
   
    Silinen bir birim grubu listesinden kaybolduktan **sonuçları** bölmesi ve o birimi grubuyla ilişkili olan tüm yedeklemeler yedekleme Kataloğu'ndan silinir.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [StorSimple çözümünüzü yönetmek için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-admin.md).
* Bilgi edinmek için nasıl [oluşturmak ve yedekleme ilkelerini yönetmek için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-manage-backup-policies.md).

