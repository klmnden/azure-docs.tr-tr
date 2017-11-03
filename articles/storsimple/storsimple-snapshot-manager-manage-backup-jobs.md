---
title: "StorSimple Snapshot Manager yedekleme işleri | Microsoft Docs"
description: "StorSimple Snapshot Manager MMC ek bileşenini görüntülemek ve zamanlanmış, çalışmakta ve tamamlanan yedekleme işlerini yönetmek için nasıl kullanılacağını açıklar."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: bf4dcff6-c819-4766-b9d9-9922831cb200
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: 03e306b62250f2bb033cc14e856a59760b5406c3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-storsimple-snapshot-manager-to-view-and-manage-backup-jobs"></a>StorSimple Snapshot Manager görüntülemek ve yedekleme işlerini yönetmek için kullanın

## <a name="overview"></a>Genel Bakış
**İşleri** düğümünde **kapsam** bölmesi gösterir **zamanlanmış**, **son 24 saat**, ve **çalıştıran** yedekleme etkileşimli olarak veya yapılandırılmış bir ilke tarafından başlatılan görev. 

Bu öğretici nasıl kullanabileceğinizi açıklar **işleri** zamanlanmış, son ve şu anda çalışan yedekleme işleri hakkındaki bilgileri görüntülemek için düğümü. (İşler ve ilgili bilgi listesi içinde görünür **sonuçları** bölmesinde.) Ayrıca, listelenen işini sağ tıklatın ve kullanılabilir eylemler listeleyen bir bağlam menüsü bakın.

## <a name="view-scheduled-jobs"></a>Zamanlanan işleri görüntüleyin
Zamanlanmış yedekleme işleri görüntülemek için aşağıdaki yordamı kullanın.

#### <a name="to-view-scheduled-jobs"></a>Zamanlanan işleri görüntülemek için
1. StorSimple anlık görüntü Yöneticisi'ni başlatmak için Masaüstü simgesine tıklayın. 
2. İçinde **kapsam** bölmesinde genişletin **işleri** düğümü ve tıklatın **zamanlanmış**. Aşağıdaki bilgiler görüntülenir **sonuçları** bölmesi:
   
   * **Ad** – zamanlanmış anlık görüntü adı
   * **Sonraki çalıştırma** – tarih ve saat, bir sonraki zamanlanmış anlık görüntüsü
   * **Son çalıştırma** – tarih ve saat, en son zamanlanmış anlık görüntüsü
     
     > [!NOTE]
     > Tek seferlik yalnızca anlık görüntüleri için **sonraki çalıştırmaya** ve **son çalıştırma** aynı olacaktır.
     
     ![Zamanlanmış yedekleme işleri](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
3. Belirli bir iş üzerinde ek eylemleri gerçekleştirmek için içindeki iş adına sağ tıklayın **sonuçları** bölmesinde ve menü seçeneklerini seçin.

## <a name="view-recent-jobs"></a>Son işleri görüntüle
Yedekleme görüntülemek ve son 24 saat içindeki tamamlandığını işleri geri yüklemek için aşağıdaki yordamı kullanın.

#### <a name="to-view-recent-jobs"></a>Yeni işleri görüntülemek için
1. StorSimple anlık görüntü Yöneticisi'ni başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde genişletin **işleri** düğümü ve tıklatın **son 24 saat**. **Sonuçları** bölmesi son 24 saat (en fazla 64 işleri) için yedekleme işleri gösterir. Aşağıdaki bilgiler görüntülenir **sonuçları** bölmesinde bağlı olarak, **Görünüm** belirlediğiniz seçenekleri:
   
   * **Ad** – zamanlanmış anlık görüntü adı.
   * **Başlatılan** – tarih ve saat zaman anlık görüntü başladı.
   * **Durdurulmuş** – ne zaman anlık görüntüsü tamamlandı veya sonlandırıldı saat ve tarihi.
   * **Geçen** – arasındaki süre miktarı **başlatıldı** ve **durduruldu** kez.
   * **Durum** – son tamamlanan iş durumu. **Başarı** Yedekleme başarıyla oluşturulduğunu gösterir. **Başarısız** işi başarıyla çalışmadı gösterir.
   * **Bilgi** – başarısızlık nedeni.
   * **Bayt (MB) işlenen** – (MB) işlenmiş birim grubundan veri miktarı. 
     
     ![Son 24 saat içinde çalışan işler](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 
3. Belirli bir iş üzerinde ek eylemleri gerçekleştirmek için içindeki iş adına sağ tıklayın **sonuçları** bölmesinde ve menü seçeneklerini seçin.
   
    ![Bir işi Sil](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png)

## <a name="view-currently-running-jobs"></a>Şu anda çalışan işleri görüntüleyin
Şu anda çalışan işleri görüntüle için aşağıdaki yordamı kullanın.

#### <a name="to-view-currently-running-jobs"></a>Şu anda çalışan işleri görüntülemek için
1. StorSimple anlık görüntü Yöneticisi'ni başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde genişletin **işleri** düğümü ve tıklatın **çalıştıran**. Bağlı olarak **Görünüm** belirttiğiniz, aşağıdaki bilgileri görünür seçenekleri **sonuçları** bölmesi:
   
   * **Ad** – zamanlanmış anlık görüntü adı.
   * **Başlatılan** – tarih ve saat zaman anlık görüntü başladı.
   * **Denetim noktası** – yedeğinin geçerli eylem.
   * **Durum** – tamamlanma yüzdesi.
   * **Geçen** – yedekleme başlamasından bu yana geçen süre miktarı. 
   * **Ortalama verimi (MB)** – toplam bayt sayısı, (MB) işlenmesi için geçen toplam süre için işlenen veri oranı.
   * **Bayt (MB) işlenen** – toplam bayt (MB) işlenen veri.
   * **(MB) yazılan bayt** – toplam bayt (MB) yazılan veri. Veri ve bunun yanı sıra meta verileri içerir ve bu nedenle işlenen bayt genellikle daha büyük.
     
     ![Şu anda çalışan işler](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)
3. Belirli bir iş üzerinde ek eylemleri gerçekleştirmek için içindeki iş adına sağ tıklayın **sonuçları** bölmesinde ve menü seçeneklerini seçin.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [StorSimple çözümünüzün yönetmek için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-admin.md).
* Bilgi edinmek için nasıl [yedekleme kataloğu yönetmek için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-manage-backup-catalog.md).

