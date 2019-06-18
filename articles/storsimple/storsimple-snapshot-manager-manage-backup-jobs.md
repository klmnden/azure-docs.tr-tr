---
title: StorSimple Snapshot Manager yedekleme işleri | Microsoft Docs
description: StorSimple Snapshot Manager MMC ek bileşeninde zamanlanmış, şu anda çalışan ve tamamlanmış yedekleme işleri görüntüleme ve yönetme için nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: ''
ms.assetid: bf4dcff6-c819-4766-b9d9-9922831cb200
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: c34ff487f03d90b16b6660fbad77c3a16699e165
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64719880"
---
# <a name="use-storsimple-snapshot-manager-to-view-and-manage-backup-jobs"></a>Yedekleme İşleri görüntüleme ve yönetme için StorSimple Snapshot Manager'ı kullanın

## <a name="overview"></a>Genel Bakış
**İşleri** düğümünde **kapsam** bölmesinde gösterildiği **zamanlanmış**, **son 24 saat**, ve **çalıştıran** etkileşimli olarak veya yapılandırılmış bir ilke tarafından başlatılan yedekleme görevleri. 

Bu öğreticide nasıl kullanabileceğinizi açıklar **işleri** zamanlanmış, yeni ve şu anda çalışan yedekleme işleri hakkında bilgi görüntülemek için. (İşler ve ilgili bilgileri listesi görünür **sonuçları** bölmesinde.) Ayrıca, listelenen iş sağ tıklayın ve kullanılabilir eylemler listeleyen bir bağlam menüsü bakın.

## <a name="view-scheduled-jobs"></a>Zamanlanan işleri görüntüle
Zamanlanan yedekleme işlerinin görüntülemek için aşağıdaki yordamı kullanın.

#### <a name="to-view-scheduled-jobs"></a>Zamanlanan işleri görüntülemek için
1. StorSimple Snapshot Manager'ı başlatmak için Masaüstü simgesine tıklayın. 
2. İçinde **kapsam** bölmesini genişletin **işleri** düğüm seçeneğine tıklayıp **zamanlanmış**. Aşağıdaki bilgiler görüntülenir **sonuçları** bölmesi:
   
   * **Adı** – zamanlanmış anlık görüntü adı
   * **Sonraki çalıştırma** – gelecek planlı anlık saati ve tarihi
   * **Son çalıştırma** – en son zamanlanmış anlık görüntünün saat ve tarihi
     
     > [!NOTE]
     > Tek seferlik yalnızca anlık görüntüler için **sonraki çalıştırma** ve **son çalıştırma** aynı olacaktır.
     
     ![Zamanlanan yedekleme işlerinin](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
3. Belirli bir işin üzerinde ek eylemler gerçekleştirmek için proje adına sağ tıklayın **sonuçları** bölmesi ve menü seçenekleri seçin.

## <a name="view-recent-jobs"></a>Son işleri görüntüle
Yedekleme görüntülemek ve son 24 saat içinde tamamlanmış işlerin geri yüklemek için aşağıdaki yordamı kullanın.

#### <a name="to-view-recent-jobs"></a>En son işlerin görüntülemek için
1. StorSimple Snapshot Manager'ı başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesini genişletin **işleri** düğüm seçeneğine tıklayıp **son 24 saat**. **Sonuçları** bölmesi, son 24 saat (en fazla 64 işlerinin için) yedekleme işleri gösterir. Aşağıdaki bilgiler görüntülenir **sonuçları** bağlı bölmesinde **görünümü** belirttiğiniz seçenekleri:
   
   * **Adı** – zamanlanmış anlık görüntünün adı.
   * **Başlatılan** – anlık görüntü başladığında saat ve tarihi.
   * **Durduruldu** – tarihi ve saati ne zaman anlık görüntüsü tamamlandı veya sonlandırıldı.
   * **Geçen** – süreyi **başlatıldı** ve **durduruldu** kez.
   * **Durum** – son tamamlanan iş durumu. **Başarı** Yedekleme başarıyla oluşturulduğunu belirtir. **Başarısız** işi başarıyla çalışmadı gösterir.
   * **Bilgi** – başarısızlık nedeni.
   * **Bayt (MB) işlenen** – (MB) işlenmiş birim grubundan veri miktarı. 
     
     ![Son 24 saat içinde çalışan işler](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 
3. Belirli bir işin üzerinde ek eylemler gerçekleştirmek için proje adına sağ tıklayın **sonuçları** bölmesi ve menü seçenekleri seçin.
   
    ![İş Sil](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png)

## <a name="view-currently-running-jobs"></a>Şu anda çalışan işleri görüntüleyin
Çalışmakta olan işleri görüntüle için aşağıdaki yordamı kullanın.

#### <a name="to-view-currently-running-jobs"></a>Şu anda çalışan işleri görüntülemek için
1. StorSimple Snapshot Manager'ı başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesini genişletin **işleri** düğüm seçeneğine tıklayıp **çalıştıran**. Yapılandırmanıza bağlı olarak **görünümü** seçeneklerini belirlerseniz, aşağıdaki bilgileri görünür **sonuçları** bölmesi:
   
   * **Adı** – zamanlanmış anlık görüntünün adı.
   * **Başlatılan** – anlık görüntü başladığında saat ve tarihi.
   * **Denetim noktası** – yedekleme geçerli eylem.
   * **Durum** – tamamlanma yüzdesi.
   * **Geçen** – yedekleme başlamasından bu yana geçen süre miktarı. 
   * **Ortalama aktarım hızı (MB)** – oranı, (MB) işlenmesi için geçen toplam süre, işlenen verilerin toplam bayt sayısı.
   * **Bayt (MB) işlenen** – toplam bayt veri (MB) işlenir.
   * **(MB) yazılan bayt** – toplam bayt veri yazılır (MB). Bu verilerin yanı sıra meta verileri içerir ve bu nedenle genellikle işlenen bayt büyük.
     
     ![Şu anda çalışan işler](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)
3. Belirli bir işin üzerinde ek eylemler gerçekleştirmek için proje adına sağ tıklayın **sonuçları** bölmesi ve menü seçenekleri seçin.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [StorSimple çözümünüzü yönetmek için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-admin.md).
* Bilgi edinmek için nasıl [yedekleme kataloğunu yönetmek için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-manage-backup-catalog.md).

