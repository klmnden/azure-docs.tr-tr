---
title: StorSimple yük devretme, StorSimple 8000 serisi fiziksel bir cihaz için olağanüstü durum kurtarma | Microsoft Docs
description: StorSimple 8000 serisi fiziksel Cihazınızı başka bir fiziksel cihaza yük devretme öğrenin.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/03/2017
ms.author: alkohli
ms.openlocfilehash: 5fcf95a1a3033a5150945dbd841f12d50ebb023b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60577264"
---
# <a name="fail-over-to-a-storsimple-8000-series-physical-device"></a>Bir StorSimple 8000 serisi fiziksel cihaza yük devretme

## <a name="overview"></a>Genel Bakış

Bu öğreticide, başka bir StorSimple fiziksel cihaz için StorSimple 8000 serisi fiziksel cihazının olağanüstü bir durum olursa devretmek için gereken adımları açıklar. StorSimple, veri merkezindeki fiziksel kaynak CİHAZDAN başka bir fiziksel cihaza geçirmek için cihaz yük devretme özelliğini kullanır. Bu öğreticideki yönergeler fiziksel cihazlar yazılım sürümleri güncelleştirme 3 ve üzerini çalıştıran StorSimple 8000 serisi için geçerlidir.

Cihaz yük devretme ve olağanüstü durumdan kurtarmak için nasıl kullanıldığı hakkında daha fazla bilgi için şuraya gidin [StorSimple 8000 serisi cihazlar için yük devretme ve olağanüstü durum kurtarma](storsimple-8000-device-failover-disaster-recovery.md).

StorSimple fiziksel cihazı StorSimple Cloud Appliance için yük devretme için şuraya gidin: [bir StorSimple bulut Gerecine yük devretme](storsimple-8000-device-failover-cloud-appliance.md). Fiziksel bir cihaz kendisine yük devretmek için Git [aynı StorSimple fiziksel cihaza yük devretme](storsimple-8000-device-failover-same-device.md).


## <a name="prerequisites"></a>Önkoşullar

- Cihaz yük devretme için değerlendirmeleri gözden geçirdiğinizden emin olun. Daha fazla bilgi için Git [cihaz yük devretme için ortak düşünceler](storsimple-8000-device-failover-disaster-recovery.md).

- Veri merkezinde dağıtılan bir StorSimple 8000 serisi fiziksel bir cihazı olması gerekir. Cihaz, güncelleştirme 3 veya sonraki yazılım sürümü çalıştırması gerekir. Daha fazla bilgi için Git [şirket içi StorSimple Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md).


## <a name="steps-to-fail-over-to-a-physical-device"></a>Fiziksel bir cihaza yük devretme için adımları

Cihazınızı bir hedef fiziksel cihaza geri yüklemek için aşağıdaki adımları gerçekleştirin.

1. Bulut anlık görüntüleri yük devretmek istediğiniz birim kapsayıcısı ilişkili olduğunu doğrulayın. Daha fazla bilgi için Git [yedeklemeler oluşturmak için kullanımı StorSimple cihaz Yöneticisi hizmeti](storsimple-8000-manage-backup-policies-u2.md).
2. StorSimple cihaz Yöneticisi'ne gidin ve ardından **cihazları**. İçinde **cihazları** dikey penceresinde, hizmetinize bağlı cihazların listesine gidin.
    ![Cihaz seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev1.png)
3. Seçin ve kaynak cihazınız tıklayın. Kaynak cihaz yük devretmek istediğiniz birim kapsayıcıları sahiptir. Git **Ayarları > birim kapsayıcıları**.
4. Başka bir cihaza yük devretmek istediğiniz bir birim kapsayıcısı seçin. Birim kapsayıcısı, bu kapsayıcıdaki birimlerin listesini görüntülemek için tıklayın. Bir birim, sağ tıklatın ve tıklatın seçin **Çevrimdışına Al** birimi çevrimdışı duruma getirmek için. Birim kapsayıcısındaki tüm birimler için bu işlemi yineleyin.
5. Başka bir cihaza yük devretmek istediğiniz tüm birim kapsayıcıları için önceki adımı yineleyin.
6. Geri Git **cihazları** dikey penceresi. Komut çubuğundaki **yük devretme**.
    ![Başarısız üzerine tıklayın](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev2.png)
    
7. İçinde **yük devretme** dikey penceresinde aşağıdaki adımları gerçekleştirin:
   
   1. Tıklayın **kaynak**. Bulut anlık görüntüleri ilişkili birimleri ile birim kapsayıcıları görüntülenir. Yalnızca görüntülenen kapsayıcıları, yük devretme için uygundur. Birim kapsayıcıları listesi içinde yük devretme istediğiniz birim kapsayıcıları seçin. **Yalnızca birim kapsayıcıları ile ilişkili bir bulut anlık görüntüleri ve çevrimdışı birimler gösterilir.**

       ![Kaynak seçme](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev5.png)
   2. Tıklayın **hedef**. Önceki adımda seçilen birim kapsayıcıları için kullanılabilir cihazlarını aşağı açılan listesinden bir hedef cihaz seçin. Yalnızca kaynak birim kapsayıcıları uyum sağlamak için yeterli kapasiteye sahip cihazlar listesinde görüntülenir.

        ![Hedef seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev6.png)

   3. Son olarak, tüm yük devretme ayarlarda gözden **özeti**. Ayarları gözden geçirdikten sonra seçilen birim kapsayıcıları birimlerin çevrimdışı olduğunu gösteren onay kutusunu işaretleyin. **Tamam** düğmesine tıklayın.

       ![Yük devretme ayarları gözden geçirin](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev8.png)
  
8. StorSimple, yük devretme işi oluşturur. Yük devretme işi aracılığıyla izlemek için iş bildirime tıklayın **işleri** dikey penceresi.

    Yük devretme birim kapsayıcısı yerel birim varsa, kapsayıcıdaki her yerel birim (değil için katmanlı birim) için tek tek geri yükleme işi görürsünüz. Bu geri yükleme işleri tamamlanması oldukça biraz zaman alabilir. Yük devretme işi daha önce tamamlanabilir olasıdır. Yalnızca geri yükleme işleri tamamlandıktan sonra bu birimler yerel GARANTİLERİN olacaktır.

    ![Yük devretme işi izleme](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

9. Yük devretme tamamlandıktan sonra Git **cihazları** dikey penceresi.
   
   1. Yük devretme işlemi için hedef cihazla olarak kullanılan bir cihaz seçin.

       ![Cihaz seç](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

   2. Git **birim kapsayıcıları** dikey penceresi. Eski bir CİHAZDAN birimleri ile birlikte tüm birim kapsayıcıları, listelenmelidir.

       ![Görünüm hedef birim kapsayıcıları](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev16.png)


## <a name="next-steps"></a>Sonraki adımlar

* Bir yük devretme gerçekleştirdikten sonra gerekebilir [StorSimple Cihazınızı silme veya devre dışı bırakma](storsimple-8000-deactivate-and-delete-device.md).
* StorSimple cihaz Yöneticisi hizmetini kullanma hakkında daha fazla bilgi için Git [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

