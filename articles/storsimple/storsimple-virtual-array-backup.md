---
title: Microsoft Azure StorSimple Virtual Array yedekleme Öğreticisi | Microsoft Docs
description: StorSimple sanal dizisi paylaşımları ve birimleri yedekleme açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: e3cdcd9e-33b1-424d-82aa-b369d934067e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a61dcca1f78b6ba444a2deefcf6b8bb4fd5c5087
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60581403"
---
# <a name="back-up-shares-or-volumes-on-your-storsimple-virtual-array"></a>Paylaşımları veya birimler, StorSimple sanal dizisi yedekleme

## <a name="overview"></a>Genel Bakış

StorSimple sanal dizisi, bir dosya sunucusu veya bir iSCSI sunucusu olarak yapılandırılmış bir karma bulut depolama şirket içi sanal cihazı ' dir. Sanal dizi cihaz üzerinde tüm paylaşımları veya birimler zamanlanmış ve el ile yedekleme oluşturmasına olanak tanır. Dosya sunucusu olarak yapılandırıldığında, ayrıca öğe düzeyinde kurtarma sağlar. Bu öğreticide, zamanlanmış ve el ile yedekleme oluşturmak ve sanal diziniz silinen bir dosyaya geri yüklemek için öğe düzeyinde kurtarma gerçekleştirmek açıklar.

Bu öğretici, StorSimple sanal diziler için yalnızca geçerlidir. 8000 serisi hakkında daha fazla bilgi için Git [8000 serisi cihaz için bir yedekleme oluşturma](storsimple-manage-backup-policies-u2.md)

## <a name="back-up-shares-and-volumes"></a>Paylaşımları ve birimleri yedekleme

Yedeklemeler, zaman içinde nokta koruma sağlamak, kurtarılabilirliği iyileştirmeye ve paylaşımları ve birimler için geri yükleme sürelerini en aza indirin. StorSimple Cihazınızda iki yolla bir paylaşım veya birim yedekleme: **Zamanlanmış** veya **el ile**. Bu yöntemlerin her biri, aşağıdaki bölümlerde ele alınmıştır.

## <a name="change-the-backup-start-time"></a>Yedekleme başlangıç saatini değiştir

> [!NOTE]
> Bu sürümde, zamanlanmış yedeklemeleri her gün belirtilen saatte çalıştırır ve cihazda tüm paylaşımları veya birimleri yedekleyen bir varsayılan ilke oluşturulur. Şu anda zamanlanmış yedeklemeler için özel ilkeler oluşturmak mümkün değildir.


StorSimple Virtual Array'iniz (22:30) gün belirtilen saatte başlayıp tüm paylaşımları veya birimler cihazda günde bir kez yedekler varsayılan yedekleme ilkesi vardır. Hangi yedekleme başlatır, ancak sıklığı ve (hangi korur yedeklemelerinin sayısını belirtir) bekletme değiştirilemez süreyi değiştirebilirsiniz. Bu yedeklemeler sırasında sanal cihazın tamamını yedeklenir. Bu potansiyel olarak cihaz performansını etkileyebilir ve cihaza dağıtılan iş yükleri etkiler. Bu nedenle, bu yedeklemeler için yoğun olmayan saatler zamanlamanızı öneririz.

 Varsayılan yedekleme başlangıç saati değiştirmek için aşağıdaki adımları gerçekleştirin. [Azure portalında](https://portal.azure.com/).

#### <a name="to-change-the-start-time-for-the-default-backup-policy"></a>Varsayılan yedekleme ilkesi için başlangıç saatini değiştirmek için

1. Git **cihazları**. StorSimple cihaz Yöneticisi hizmetine kayıtlı cihazların listesi görüntülenir. 
   
    ![cihazlara gidin](./media/storsimple-virtual-array-backup/changebuschedule1.png)

2. Cihazınızı tıklatıp seçin. **Ayarları** dikey penceresi görüntülenir. Git **Yönet > Yedekleme ilkelerine**.
   
    ![Cihazınızı seçin](./media/storsimple-virtual-array-backup/changebuschedule2.png)

3. İçinde **yedekleme ilkeleri** dikey penceresinde varsayılan başlangıç saati, 22:30 olan. Cihaz saat diliminde günlük zamanlama için yeni başlangıç saati belirtebilirsiniz.
   
    ![Yedekleme ilkelerini gidin](./media/storsimple-virtual-array-backup/changebuschedule5.png)

4. **Kaydet**’e tıklayın.

### <a name="take-a-manual-backup"></a>El ile yedekleyin

Zamanlanmış yedeklemeler ek olarak, el ile (isteğe bağlı) herhangi bir zamanda cihaz verilerini yedekleme oluşturabilirsiniz.

#### <a name="to-create-a-manual-backup"></a>El ile yedekleme oluşturmak için

1. Git **cihazları**. Cihazınızı seçin ve sağ **...**  en sağdaki seçilen satırdaki. Bağlam menüsünden seçin **yedek Al**.
   
    ![yedek Al gidin](./media/storsimple-virtual-array-backup/takebackup1m.png)

2. İçinde **yedek Al** dikey penceresinde tıklayın **yedek Al**. Bu dosya sunucusundaki tüm paylaşımları veya iSCSI sunucunuzdaki tüm birimleri yedekleme. 
   
    ![Yedekleme başlangıcı:](./media/storsimple-virtual-array-backup/takebackup2m.png)
   
    İsteğe bağlı yedekleme başlar ve bir yedekleme işi başlatıldı görürsünüz.
   
    ![Yedekleme başlangıcı:](./media/storsimple-virtual-array-backup/takebackup3m.png) 
   
    İş başarıyla tamamlandıktan sonra yeniden bildirilir. Daha sonra yedekleme işlemini başlatır.
   
    ![oluşturulan yedekleme işi](./media/storsimple-virtual-array-backup/takebackup4m.png)

3. İş ayrıntılarını inceleyin ve yedeklemeleri ilerlemesini izlemek için bildirime tıklayın. Sayfasına yönlendirileceksiniz **iş ayrıntıları**.
   
     ![yedekleme işi ayrıntıları](./media/storsimple-virtual-array-backup/takebackup5m.png)

4. Yedekleme tamamlandıktan sonra Git **Yönetim > Yedekleme kataloğu**. Cihazınızda bir bulut anlık görüntüsü veya tüm paylaşımları (birimler) görürsünüz.
   
    ![Tamamlanan yedekleme](./media/storsimple-virtual-array-backup/takebackup19m.png) 

## <a name="view-existing-backups"></a>Var olan yedekleri görüntüleme
Var olan yedekleri görüntülemek için Azure portalında aşağıdaki adımları gerçekleştirin.

#### <a name="to-view-existing-backups"></a>Var olan yedekleri görüntülemek için

1. Git **cihazları** dikey penceresi. Cihazınızı tıklatıp seçin. İçinde **ayarları** dikey penceresinde, Git **Yönetim > Yedekleme kataloğu**.
   
    ![Yedekleme Kataloğu'na gidin](./media/storsimple-virtual-array-backup/viewbackups1.png)
2. Filtreleme için kullanılacak aşağıdaki ölçütleri belirtin:
   
   - **Zaman aralığı** – olabilir **son 1 saat**, **son 24 saat**, **son 7 gün**, **son 30 gündeki**, **son yıl** , ve **özel tarih**.
    
   - **Cihazları** – dosya sunucularında veya iSCSI sunucuları, StorSimple cihaz Yöneticisi hizmetine kayıtlı listesinden seçin.
   
   - **Başlatılan** – otomatik olarak **zamanlanmış** (tarafından bir yedekleme İlkesi) veya **el ile** (sizin tarafınızdan) başlattı.
   
     ![Yedeklemeleri filtrelemek](./media/storsimple-virtual-array-backup/viewbackups2.png)

3. **Uygula**'ya tıklayın. Filtrelenmiş liste yedeklerini görüntülenen **yedekleme kataloğu** dikey penceresi. Not yalnızca 100 yedekleme öğeleri belirli bir anda gösterilebilir.
   
    ![Güncelleştirilmiş yedekleme kataloğu](./media/storsimple-virtual-array-backup/viewbackups3.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [StorSimple Virtual Array'iniz yönetme](storsimple-ova-web-ui-admin.md).

