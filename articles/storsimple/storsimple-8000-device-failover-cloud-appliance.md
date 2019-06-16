---
title: StorSimple yük devretme, bir StorSimple Cloud Appliance için olağanüstü durum kurtarma | Microsoft Docs
description: StorSimple 8000 serisi fiziksel Cihazınızı bulut gerecine yük devretme öğrenin.
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
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: 45c521fd044fa258b8052a3f0de48784cf4160e8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60584441"
---
# <a name="fail-over-to-your-storsimple-cloud-appliance"></a>StorSimple Cloud Appliance'ınız için yük devretme

## <a name="overview"></a>Genel Bakış

Bu öğreticide, StorSimple 8000 serisi fiziksel bir cihaz için StorSimple Cloud Appliance olağanüstü bir durum olursa devretmek için gereken adımları açıklar. StorSimple, veri merkezindeki fiziksel kaynak CİHAZDAN Azure'da çalışan bir bulut Gereci geçirmek için cihaz yük devretme özelliğini kullanır. Bu öğreticideki yönergeler, StorSimple 8000 serisi fiziksel cihazlar ve yazılım sürümlerini güncelleştirme 3 ve üzerini çalıştıran cloud appliance'lar için geçerlidir.

Cihaz yük devretme ve olağanüstü durumdan kurtarmak için nasıl kullanıldığı hakkında daha fazla bilgi için şuraya gidin [StorSimple 8000 serisi cihazlar için yük devretme ve olağanüstü durum kurtarma](storsimple-8000-device-failover-disaster-recovery.md).

StorSimple fiziksel cihazı başka bir fiziksel cihaza yük devretme için şuraya gidin: [bir StorSimple fiziksel cihaza yük devretme](storsimple-8000-device-failover-physical-device.md). Bir cihaz kendisine yük devretmek için Git [aynı StorSimple fiziksel cihaza yük devretme](storsimple-8000-device-failover-same-device.md).

## <a name="prerequisites"></a>Önkoşullar

- Cihaz yük devretme için değerlendirmeleri gözden geçirdiğinizden emin olun. Daha fazla bilgi için Git [cihaz yük devretme için ortak düşünceler](storsimple-8000-device-failover-disaster-recovery.md).

- StorSimple Cloud Appliance oluşturulur ve bu yordamı çalıştırmadan önce yapılandırılmış olması gerekir. Çalışan 3 yazılım sürümü güncelleştirme ya da daha sonra 8020 bulut gerecini DR için kullanmayı göz önünde bulundurun. Model 8020 64 TB ve Premium depolama kullanır. Daha fazla bilgi için Git [Dağıt ve StorSimple bulut Gereci yönetme](storsimple-8000-cloud-appliance-u2.md).

## <a name="steps-to-fail-over-to-a-cloud-appliance"></a>Bulut gerecine yük devretme için adımları

Bir hedef StorSimple Cloud Appliance cihazı geri yüklemek için aşağıdaki adımları gerçekleştirin.

1.  Bulut anlık görüntüleri yük devretmek istediğiniz birim kapsayıcısı ilişkili olduğunu doğrulayın. Daha fazla bilgi için Git [yedeklemeler oluşturmak için kullanımı StorSimple cihaz Yöneticisi hizmeti](storsimple-8000-manage-backup-policies-u2.md).
2. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. İçinde **cihazları** dikey penceresinde, hizmetinize bağlı cihazların listesine gidin.
    ![Cihaz seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev1.png)
3. Seçin ve kaynak cihazınız tıklayın. Kaynak cihaz yük devretmek istediğiniz birim kapsayıcıları sahiptir. Git **Ayarları > birim kapsayıcıları**.

    ![Cihaz seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev2.png)
    
4. Başka bir cihaza yük devretmek istediğiniz bir birim kapsayıcısı seçin. Birim kapsayıcısı, bu kapsayıcıdaki birimlerin listesini görüntülemek için tıklayın. Bir birim, sağ tıklatın ve tıklatın seçin **Çevrimdışına Al** birimi çevrimdışı duruma getirmek için.

    ![Cihaz seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev5.png)

5. Birim kapsayıcısındaki tüm birimler için bu işlemi yineleyin.

     ![Cihaz seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev7.png)

6. Başka bir cihaza yük devretmek istediğiniz tüm birim kapsayıcıları için önceki adımı yineleyin.

7. Geri Git **cihazları** dikey penceresi. Komut çubuğundaki **yük devretme**.

    ![Başarısız üzerine tıklayın](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev8.png)
8. İçinde **yük devretme** dikey penceresinde aşağıdaki adımları gerçekleştirin:
   
    1. Tıklayın **kaynak**. Yük devretme için birim kapsayıcıları seçin. **Yalnızca birim kapsayıcıları ile ilişkili bir bulut anlık görüntüleri ve çevrimdışı birimler gösterilir.**
        ![Kaynak seçme](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev11.png)
    2. Tıklayın **hedef**. Kullanılabilir cihazları açılan listesinden bir hedef bulut Gereci seçin. **Yalnızca kaynak birim kapsayıcıları uyum sağlamak için yeterli kapasiteye sahip cihazlar listesinde görüntülenir.**

        ![Hedef seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev12.png)

    3. Yük devretme ayarları gözden geçirin **özeti** seçilen birim kapsayıcıları birimlerin çevrimdışı olduğunu gösteren onay kutusunu işaretleyin. 

        ![Yük devretme ayarları gözden geçirin](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev13.png)

9. Yük devretme işi oluşturulur. Yük devretme işi izlemek için iş bildirimi tıklayın.

    ![Yük devretme işi izleme](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

10. Yük devretme tamamlandıktan sonra dönün **cihazları** dikey penceresi.

    1. Yük devretme için hedef olarak kullanılan bir cihaz seçin.

       ![Cihaz seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

    2. Tıklayın **birim kapsayıcıları**. Eski bir CİHAZDAN birimleri ile birlikte tüm birim kapsayıcıları, listelenmelidir.

       Yük devretme birim kapsayıcısı yerel olarak sabitlenmiş birimler, bu birimlerdeki katmanlı birimler olarak yük devredildi. Yerel olarak sabitlenmiş birimler, StorSimple Cloud Appliance'ta desteklenmez.

       ![Görünüm hedef birim kapsayıcıları](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev17.png)


## <a name="next-steps"></a>Sonraki adımlar

* Bir yük devretme gerçekleştirdikten sonra gerekebilir [StorSimple Cihazınızı silme veya devre dışı bırakma](storsimple-8000-deactivate-and-delete-device.md).

* StorSimple cihaz Yöneticisi hizmetini kullanma hakkında daha fazla bilgi için Git [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

