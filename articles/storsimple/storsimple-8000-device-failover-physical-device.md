---
title: "StorSimple yük devretme, StorSimple 8000 serisi fiziksel aygıt için olağanüstü durum kurtarma | Microsoft Docs"
description: "StorSimple 8000 serisi fiziksel aygıtınıza başka bir fiziksel aygıt üzerinden vermesine öğrenin."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/03/2017
ms.author: alkohli
ms.openlocfilehash: f3ac9545a341fc24ca12c9f2547805d6956cd98a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="fail-over-to-a-storsimple-8000-series-physical-device"></a>StorSimple 8000 serisi fiziksel cihaza yük devri

## <a name="overview"></a>Genel Bakış

Bu öğretici bir StorSimple 8000 serisi fiziksel cihazı başka bir StorSimple fiziksel cihazı bir olağanüstü durum varsa devretmek için gereken adımları açıklar. StorSimple cihaz yük devretme özelliğini veri merkezindeki fiziksel bir kaynak aygıtı başka bir fiziksel cihaza geçirmek için kullanır. Bu öğreticideki yönergeler yazılım sürümleri güncelleştirme 3 ve sonraki sürümleri çalıştıran fiziksel cihazları StorSimple 8000 serisi için geçerlidir.

Cihaz yük devretme ve olağanüstü durumdan kurtarmak için nasıl kullanıldığı hakkında daha fazla bilgi için şuraya gidin [StorSimple 8000 serisi cihazlar için yük devretme ve olağanüstü durum kurtarma](storsimple-8000-device-failover-disaster-recovery.md).

Bir StorSimple bulut uygulaması StorSimple fiziksel cihaza yük devretme için Git [StorSimple bulut uygulaması için yük devri](storsimple-8000-device-failover-cloud-appliance.md). Kendisi için fiziksel bir aygıtı devretmek için Git [aynı StorSimple fiziksel cihazı yük devri](storsimple-8000-device-failover-same-device.md).


## <a name="prerequisites"></a>Ön koşullar

- Cihaz yük devretme için konuları gözden geçirdiğinizden emin olun. Daha fazla bilgi için Git [aygıt yük devretme için ortak konuları](storsimple-8000-device-failover-disaster-recovery.md).

- Veri merkezinde dağıtılan bir StorSimple 8000 serisi fiziksel bir aygıtı olması gerekir. Cihaz, güncelleştirme 3 veya sonraki yazılım sürümü çalıştırmanız gerekir. Daha fazla bilgi için Git [şirket içi StorSimple Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md).


## <a name="steps-to-fail-over-to-a-physical-device"></a>Fiziksel bir cihaza yük devri için adımları

Cihazınızı bir hedef fiziksel cihaza geri yüklemek için aşağıdaki adımları gerçekleştirin.

1. Devretmek istediğiniz birim kapsayıcısı bulut anlık görüntüleri ilişkili olduğunu doğrulayın. Daha fazla bilgi için Git [yedeklemeler oluşturmak için Aygıt Yöneticisi'ni StorSimple hizmeti](storsimple-8000-manage-backup-policies-u2.md).
2. StorSimple cihaz yöneticinize gidin ve ardından **aygıtları**. İçinde **aygıtları** dikey penceresinde, hizmetiniz ile bağlı cihazlar listesine gidin.
    ![Cihazı seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev1.png)
3. Seçin ve kaynak Cihazınızı tıklayın. Kaynak aygıtın devretmek istediğiniz birim kapsayıcıları vardır. Git **ayarlar > birim kapsayıcıları**.
4. Başka bir cihaza yük devri istediğiniz bir birim kapsayıcısı seçin. Bu kapsayıcı içindeki birimlerin listesini görüntülemek için birim kapsayıcısı'ı tıklatın. Bir birim, sağ tıklatın ve tıklatın seçin **Çevrimdışına Al** birimi çevrimdışı duruma getirmek için. Bu işlemi, birim kapsayıcısındaki tüm birimler için yineleyin.
5. Başka bir cihaza yük devri istediğiniz tüm birim kapsayıcıları için önceki adımı yineleyin.
6. Geri dönerek **aygıtları** dikey. Komut çubuğundaki **yük devri**.
    ![Üzerinden başarısız'ı tıklatın](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev2.png)
    
7. İçinde **yük devri** dikey penceresinde, aşağıdaki adımları gerçekleştirin:
   
   1. Tıklatın **kaynak**. Bulut anlık görüntüleri ile ilişkili birimi ile birim kapsayıcıları görüntülenir. Yalnızca görüntülenen kapsayıcıları, yük devretme için uygundur. Birim kapsayıcıları listesinde devretmek istediğiniz birim kapsayıcıları seçin. **Yalnızca birim kapsayıcıları ilişkili bulut anlık görüntüleri ve çevrimdışı birimlerle birlikte görüntülenir.**

       ![Kaynak seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev5.png)
   2. Tıklatın **hedef**. Önceki adımda seçtiğiniz birim kapsayıcıları için hedef aygıtta kullanılabilir aygıtları aşağı açılan listeden seçin. Yalnızca kaynak birim kapsayıcıları karşılamak için yeterli kapasiteye sahip aygıtlar listesinde görüntülenir.

        ![Hedef seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev6.png)

   3. Son olarak, tüm yük devretme ayarları altında gözden **Özet**. Ayarları gözden geçirdikten sonra seçilen birim kapsayıcıları birimlerin çevrimdışı olduğunu gösteren onay kutusunu seçin. **Tamam** düğmesine tıklayın.

       ![Yük devretme ayarlarını gözden geçirin](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev8.png)
  
8. StorSimple bir yük devretme işi oluşturur. Yük devretme işlemi aracılığıyla izlemek için iş bildirimi tıklatın **işleri** dikey.

    Üzerinden başarısız birim kapsayıcısı yerel birim varsa, tek tek geri yükleme işleri kapsayıcıdaki her yerel birimi (değil için katmanlı birimleri) konusuna bakın. Bu geri yükleme işlerinin tamamlanmasını oldukça biraz zaman alabilir. Yük devretme işine önceki tamamlanabilir olasıdır. Yalnızca geri yükleme işi tamamlandıktan sonra bu birimler yerel GARANTİLERİN sahip olur.

    ![İzleyici yük devretme işine](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

9. Yük devretme işlemi tamamlandıktan sonra Git **aygıtları** dikey.
   
   1. Yük devretme işlemi için hedef cihaz olarak kullanılan cihazı seçin.

       ![Cihazı seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

   2. Git **birim kapsayıcıları** dikey. Eski aygıt birimlerden yanı sıra tüm birim kapsayıcıları, listelenmelidir.

       ![Görünüm hedef birim kapsayıcıları](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev16.png)


## <a name="next-steps"></a>Sonraki adımlar

* Bir yük devretme gerçekleştirdikten sonra gerekebilir [StorSimple Cihazınızı silmek veya devre dışı bırakma](storsimple-8000-deactivate-and-delete-device.md).
* StorSimple cihaz Yöneticisi hizmetini kullanma hakkında daha fazla bilgi için Git [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

