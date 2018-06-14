---
title: Microsoft Azure StorSimple sanal dizinin yedekleme Öğreticisi | Microsoft Docs
description: StorSimple sanal dizinin paylaşımları ve birimler yedekleme açıklar.
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
ms.openlocfilehash: c926f0c80ce56cac3106ad97ec3ec2e18a8e2cc6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23875651"
---
# <a name="back-up-shares-or-volumes-on-your-storsimple-virtual-array"></a>Paylaşımlar veya birimler, StorSimple sanal dizisinde yedekle

## <a name="overview"></a>Genel Bakış

StorSimple sanal dizinin bir dosya sunucusu veya bir iSCSI sunucusu yapılandırılmış bir karma bulut depolama şirket içi sanal cihazdır. Sanal dizinin cihazda zamanlanmış ve el ile yedeklerini tüm paylaşımlar veya birimler oluşturmasına olanak tanır. Dosya sunucusu olarak yapılandırıldığında, öğe düzeyinde kurtarma de sağlar. Bu öğretici, zamanlanmış ve el ile yedekleme oluşturmak ve sanal dizisindeki silinmiş bir dosyayı geri yüklemek için öğe düzeyinde kurtarma gerçekleştirmek açıklar.

Bu öğretici, StorSimple sanal diziler için yalnızca geçerlidir. 8000 serisi hakkında daha fazla bilgi için Git [8000 serisi aygıtı için bir yedekleme oluşturmak](storsimple-manage-backup-policies-u2.md)

## <a name="back-up-shares-and-volumes"></a>Paylaşımları ve birimler yedekleme

Yedeklemeleri zaman noktası korumasını sağlar, kurtarılabilirliği iyileştirmeye ve paylaşımları ve birimler için geri yükleme sürelerini en aza. StorSimple Cihazınızda iki yolla paylaşımı veya birimi yedekleyebilirsiniz: **zamanlanmış** veya **el ile**. Yöntemlerin her biri aşağıdaki bölümlerde ele alınmıştır.

## <a name="change-the-backup-start-time"></a>Yedekleme başlangıç saatini değiştir

> [!NOTE]
> Bu sürümde, zamanlanmış yedeklemeler, her gün belirtilen zamanda çalışan ve tüm paylaşımlar veya birimler cihazda yedekler varsayılan bir ilke tarafından oluşturulur. Şu anda zamanlanmış yedeklemeler için özel ilkeler oluşturmak mümkün değil.


StorSimple sanal dizinizi (22:30) günün belirli bir zamanda başlatır ve tüm paylaşımlar veya birimler cihazda günde bir kez yedekler varsayılan yedekleme ilkesine sahip. Hangi yedekleme başlar, ancak sıklığı ve (hangi korumak için yedeklemeler sayısını belirtir) bekletme değiştirilemez zaman değiştirebilirsiniz. Bu yedeklemeler sırasında sanal cihazın tamamının yedeklenir. Bu olası cihaz performansını etkileyebilir ve cihazda dağıtılan iş yükleri etkiler. Bu nedenle, bu yedeklemeler için yoğun olmayan saatler zamanlamanızı öneririz.

 Varsayılan yedekleme başlangıç saati değiştirmek için aşağıdaki adımları gerçekleştirin [Azure portal](https://portal.azure.com/).

#### <a name="to-change-the-start-time-for-the-default-backup-policy"></a>Varsayılan yedekleme ilkesinin başlangıç saati değiştirmek için

1. Git **aygıtları**. StorSimple cihaz Yöneticisi hizmetiniz ile kaydedilmiş cihazların listesi görüntülenir. 
   
    ![aygıtlara gidin](./media/storsimple-virtual-array-backup/changebuschedule1.png)

2. Seçin ve aygıtınızı tıklatın. **Ayarları** dikey penceresinde görüntülenir. Git **Yönet > Yedekleme ilkeleri**.
   
    ![Cihazınızı seçin](./media/storsimple-virtual-array-backup/changebuschedule2.png)

3. İçinde **yedekleme ilkeleri** dikey penceresinde, varsayılan başlangıç zamanıdır 22:30. Günlük Zamanlama için yeni başlangıç saati aygıt saat diliminde belirtebilirsiniz.
   
    ![Yedekleme ilkeleri gidin](./media/storsimple-virtual-array-backup/changebuschedule5.png)

4. **Kaydet** düğmesine tıklayın.

### <a name="take-a-manual-backup"></a>El ile yedekleyin

Zamanlanmış yedeklemeleri yanı sıra, el ile (isteğe bağlı) herhangi bir zamanda cihaz verilerin yedeğini sürebilir.

#### <a name="to-create-a-manual-backup"></a>El ile yedekleme oluşturmak için

1. Git **aygıtları**. Cihazınızı seçin ve sağ **...**  seçili satırdaki sağ uçta. Bağlam menüsünden seçin **yedek alın**.
   
    ![yedek alın gidin](./media/storsimple-virtual-array-backup/takebackup1m.png)

2. İçinde **yedek alın** dikey penceresinde tıklatın **yedek alın**. Bu dosya sunucusundaki tüm paylaşımlar veya iSCSI sunucunuzdaki tüm birimleri yedekleyin. 
   
    ![Başlangıç yedekleme](./media/storsimple-virtual-array-backup/takebackup2m.png)
   
    Bir talep üzerine yedekleme başlatılır ve bir yedekleme işi başlatıldığını görebilirsiniz.
   
    ![Başlangıç yedekleme](./media/storsimple-virtual-array-backup/takebackup3m.png) 
   
    İş başarıyla tamamlandıktan sonra yeniden bildirilir. Yedekleme işleminin ardından başlatır.
   
    ![Yedekleme işi oluşturuldu](./media/storsimple-virtual-array-backup/takebackup4m.png)

3. Yedeklemeleri ilerlemesini izlemek ve iş ayrıntılarını inceleyin için bildirime tıklayın. Bu, olanak sürer **iş ayrıntıları**.
   
     ![yedekleme işi ayrıntıları](./media/storsimple-virtual-array-backup/takebackup5m.png)

4. Yedekleme tamamlandıktan sonra Git **Yönetim > Yedekleme kataloğu**. Cihazınızda bir bulut anlık görüntüsü tüm paylaşımlar (veya birimleri) görürsünüz.
   
    ![Tamamlanan yedekleme](./media/storsimple-virtual-array-backup/takebackup19m.png) 

## <a name="view-existing-backups"></a>Var olan yedekleri görüntüleyin
Var olan yedekleri görüntülemek için Azure portalında aşağıdaki adımları gerçekleştirin.

#### <a name="to-view-existing-backups"></a>Var olan yedekleri görüntülemek için

1. Git **aygıtları** dikey. Seçin ve aygıtınızı tıklatın. İçinde **ayarları** dikey penceresinde, Git **Yönetim > Yedekleme kataloğu**.
   
    ![Yedekleme Kataloğu'na gidin](./media/storsimple-virtual-array-backup/viewbackups1.png)
2. Filtreleme için kullanılmak üzere aşağıdaki ölçütleri belirtin:
   
    - **Zaman aralığı** – olabilir **son 1 saat**, **son 24 saat**, **son 7 gün**, **son 30 gündeki**, **geçen yıl** , ve **özel tarih**.
    
    - **Aygıtları** – dosya sunucuları, StorSimple cihaz Yöneticisi hizmetine kayıtlı iSCSI sunucuları veya listeden seçin.
   
    - **Başlatılan** – otomatik olarak **zamanlanmış** (göre bir yedekleme İlkesi) veya **el ile** (sizin tarafınızdan) başlatıldı.
   
    ![Filtre yedekleri](./media/storsimple-virtual-array-backup/viewbackups2.png)

3. **Uygula**'ya tıklayın. Yedeklemeleri filtrelenmiş listesi görüntülenir **yedekleme kataloğu** dikey. Not yalnızca 100 yedekleme öğeleri belirli bir zamanda görüntülenebilir.
   
    ![Güncelleştirilmiş yedekleme kataloğu](./media/storsimple-virtual-array-backup/viewbackups3.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek [StorSimple sanal dizinizi yönetme](storsimple-ova-web-ui-admin.md).

