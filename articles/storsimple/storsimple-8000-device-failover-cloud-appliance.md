---
title: "StorSimple yük devretme, bir StorSimple bulut uygulaması için olağanüstü durum kurtarma | Microsoft Docs"
description: "StorSimple 8000 serisi fiziksel Cihazınızı bulut uygulaması için yük devri öğrenin."
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
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: ec8bebf2854e84a37e84b45564e80fc20b63d8d8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="fail-over-to-your-storsimple-cloud-appliance"></a>StorSimple bulut aygıtınızın yük devri

## <a name="overview"></a>Genel Bakış

Bu öğretici, StorSimple 8000 serisi fiziksel cihaz StorSimple bulut uygulaması için bir olağanüstü durum varsa devretmek için gereken adımları açıklar. StorSimple cihaz yük devretme özelliğini veri merkezindeki kaynak fiziksel CİHAZDAN Azure'da çalışan bulut uygulaması geçirmek için kullanır. Bu öğreticideki yönergeler StorSimple 8000 serisi fiziksel cihazlar ve yazılım sürümlerini güncelleştirme 3 ve sonraki sürümleri çalıştıran bulut uygulamaları için geçerlidir.

Cihaz yük devretme ve olağanüstü durumdan kurtarmak için nasıl kullanıldığı hakkında daha fazla bilgi için şuraya gidin [StorSimple 8000 serisi cihazlar için yük devretme ve olağanüstü durum kurtarma](storsimple-8000-device-failover-disaster-recovery.md).

Başka bir fiziksel cihaz StorSimple fiziksel cihaza yük devretme için Git [bir StorSimple fiziksel cihazı yük devri](storsimple-8000-device-failover-physical-device.md). Kendisini bir cihaza yük devretme için Git [aynı StorSimple fiziksel cihazı yük devri](storsimple-8000-device-failover-same-device.md).

## <a name="prerequisites"></a>Ön koşullar

- Cihaz yük devretme için konuları gözden geçirdiğinizden emin olun. Daha fazla bilgi için Git [aygıt yük devretme için ortak konuları](storsimple-8000-device-failover-disaster-recovery.md).

- Bir StorSimple bulut oluşturulur ve bu yordamı çalıştırmadan önce yapılandırılmış Gereci olması gerekir. Çalışan 3 yazılım sürümü güncelleştirme ya da daha sonra bir 8020 bulut uygulaması için DR kullanmayı düşünün. 8020 modeli 64 TB vardır ve Premium depolama kullanır. Daha fazla bilgi için Git [dağıtma ve StorSimple bulut uygulaması yönetmek](storsimple-8000-cloud-appliance-u2.md).

## <a name="steps-to-fail-over-to-a-cloud-appliance"></a>Bir bulut uygulaması üzerinden vermesine adımları

Bir hedef bulut uygulaması StorSimple cihazı geri yüklemek için aşağıdaki adımları gerçekleştirin.

1.  Devretmek istediğiniz birim kapsayıcısı bulut anlık görüntüleri ilişkili olduğunu doğrulayın. Daha fazla bilgi için Git [yedeklemeler oluşturmak için Aygıt Yöneticisi'ni StorSimple hizmeti](storsimple-8000-manage-backup-policies-u2.md).
2. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. İçinde **aygıtları** dikey penceresinde, hizmetiniz ile bağlı cihazlar listesine gidin.
    ![Cihazı seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev1.png)
3. Seçin ve kaynak Cihazınızı tıklayın. Kaynak aygıtın devretmek istediğiniz birim kapsayıcıları vardır. Git **ayarlar > birim kapsayıcıları**.

    ![Cihazı seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev2.png)
    
4. Başka bir cihaza yük devri istediğiniz bir birim kapsayıcısı seçin. Bu kapsayıcı içindeki birimlerin listesini görüntülemek için birim kapsayıcısı'ı tıklatın. Bir birim, sağ tıklatın ve tıklatın seçin **Çevrimdışına Al** birimi çevrimdışı duruma getirmek için.

    ![Cihazı seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev5.png)

5. Bu işlemi, birim kapsayıcısındaki tüm birimler için yineleyin.

     ![Cihazı seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev7.png)

6. Başka bir cihaza yük devri istediğiniz tüm birim kapsayıcıları için önceki adımı yineleyin.

7. Geri dönerek **aygıtları** dikey. Komut çubuğundaki **yük devri**.

    ![Üzerinden başarısız'ı tıklatın](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev8.png)
8. İçinde **yük devri** dikey penceresinde, aşağıdaki adımları gerçekleştirin:
   
    1. Tıklatın **kaynak**. Yük devretme için birim kapsayıcıları seçin. **Yalnızca birim kapsayıcıları ilişkili bulut anlık görüntüleri ve çevrimdışı birimlerle birlikte görüntülenir.**
        ![Kaynak seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev11.png)
    2. Tıklatın **hedef**. Bir hedef bulut uygulaması kullanılabilir aygıtları açılır listeden seçin. **Yalnızca kaynak birim kapsayıcıları karşılamak için yeterli kapasiteye sahip aygıtlar listesinde görüntülenir.**

        ![Hedef seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev12.png)

    3. Yük devretme ayarları altında gözden **Özet** ve Seçili birim kapsayıcıları birimlerin çevrimdışı olduğunu gösteren onay kutusunu seçin. 

        ![Yük devretme ayarlarını gözden geçirin](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev13.png)

9. Bir yük devretme iş oluşturulur. Yük devretme işi izlemek için iş bildirime tıklayın.

    ![İzleyici yük devretme işine](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

10. Yük devretme işlemi tamamlandıktan sonra geri dönüp **aygıtları** dikey.

    1. Yük devretme için hedef olarak kullanılan cihazı seçin.

       ![Cihazı seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

    2. Tıklatın **birim kapsayıcıları**. Eski aygıt birimlerden yanı sıra tüm birim kapsayıcıları, listelenmelidir.

       Üzerinden başarısız birim kapsayıcısı yerel olarak sabitlenmiş birimleri, bu birimlerin katmanlı birimler olarak yük devredildi. Yerel olarak sabitlenmiş birimlerin bir StorSimple bulut uygulaması üzerinde desteklenmez.

       ![Görünüm hedef birim kapsayıcıları](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev17.png)


## <a name="next-steps"></a>Sonraki adımlar

* Bir yük devretme gerçekleştirdikten sonra gerekebilir [StorSimple Cihazınızı silmek veya devre dışı bırakma](storsimple-8000-deactivate-and-delete-device.md).

* StorSimple cihaz Yöneticisi hizmetini kullanma hakkında daha fazla bilgi için Git [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

