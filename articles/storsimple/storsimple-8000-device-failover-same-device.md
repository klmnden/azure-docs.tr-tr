---
title: StorSimple yük devretme, 8000 serisi cihazlar için olağanüstü durum kurtarma | Microsoft Docs
description: StorSimple Cihazınızı aynı cihaza yük devretme öğrenin.
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
ms.date: 06/23/2017
ms.author: alkohli
ms.openlocfilehash: dd207eaad1a3e821724d51a890d0882bfffda131
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60577400"
---
# <a name="fail-over-your-storsimple-physical-device-to-same-device"></a>StorSimple fiziksel Cihazınızı aynı cihaza yük devretme

## <a name="overview"></a>Genel Bakış

Bu öğreticide, bir StorSimple 8000 serisi fiziksel cihazının kendine olağanüstü bir durum olursa devretmek için gereken adımları açıklar. StorSimple, veri merkezindeki fiziksel kaynak CİHAZDAN başka bir fiziksel cihaza geçirmek için cihaz yük devretme özelliğini kullanır. Bu öğreticideki yönergeler fiziksel cihazlar yazılım sürümleri güncelleştirme 3 ve üzerini çalıştıran StorSimple 8000 serisi için geçerlidir.

Cihaz yük devretme ve olağanüstü durumdan kurtarmak için nasıl kullanıldığı hakkında daha fazla bilgi için şuraya gidin [StorSimple 8000 serisi cihazlar için yük devretme ve olağanüstü durum kurtarma](storsimple-8000-device-failover-disaster-recovery.md).

Fiziksel bir cihaz başka bir fiziksel cihaza yük devretme için şuraya gidin: [aynı StorSimple fiziksel cihaza yük devretme](storsimple-8000-device-failover-physical-device.md). StorSimple fiziksel cihazı StorSimple Cloud Appliance için yük devretme için şuraya gidin: [bir StorSimple bulut Gerecine yük devretme](storsimple-8000-device-failover-cloud-appliance.md).


## <a name="prerequisites"></a>Önkoşullar

- Cihaz yük devretme için değerlendirmeleri gözden geçirdiğinizden emin olun. Daha fazla bilgi için Git [cihaz yük devretme için ortak düşünceler](storsimple-8000-device-failover-disaster-recovery.md).


## <a name="steps-to-fail-over-to-the-same-device"></a>Aynı cihaza yük devretme için adımları

Aynı cihaza yük devretme gerekiyorsa aşağıdaki adımları gerçekleştirin.

1. Tüm birimler bulut anlık görüntüleri kullanarak Cihazınızı alın. Daha fazla bilgi için Git [yedeklemeler oluşturmak için kullanımı StorSimple cihaz Yöneticisi hizmeti](storsimple-8000-manage-backup-policies-u2.md).
2. Cihazınızı fabrika ayarlarına döndürebilir. Ayrıntılı yönergeleri izleyin [bir StorSimple cihazı varsayılan fabrika ayarlarına sıfırlama](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).
3. StorSimple cihaz Yöneticisi hizmetine gidin ve ardından **cihazları**. İçinde **cihazları** dikey penceresinde eski cihaz olarak göstermelidir **çevrimdışı**.

    ![Kaynak cihaz çevrimdışı](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev2.png)

4. Cihazınızı yapılandırmak ve StorSimple cihaz Yöneticisi hizmetinize yeniden kaydedin. Yeni kaydettiğiniz cihazın olarak göstermelidir **kuruluma hazır**. Yeni cihaz için cihaz adı, eski cihaz ile aynıdır ancak cihazı fabrika varsayılan değerlerine sıfırlamak ve yeniden kayıtlı olduğunu belirten bir sayı ile eklenir.

    ![Yeni kaydettiğiniz cihazın kuruluma hazır](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev3.png)
5. Yeni cihaz için cihaz kurulumunu tamamlayın. Daha fazla bilgi için Git [4. adım: Minimum cihaz kurulumunu Tamamla](storsimple-8000-deployment-walkthrough-u2.md#step-4-complete-minimum-device-setup). Üzerinde **cihazları** dikey penceresinde cihazın durumu değişiklikleri **çevrimiçi**.

   > [!IMPORTANT]
   > **En düşük yapılandırmayı ilk tamamlamak veya DR'niz başarısız olabilir.**

    ![Yeni kaydettiğiniz cihaz çevrimiçi](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev7.png)

6. Eski cihaz (çevrimdışı durum) seçin ve Komut çubuğundaki **yük devretme**. İçinde **yük devretme** dikey penceresinde, kaynak olarak eski bir cihaz seçin ve yeni kaydettiğiniz cihazın hedef cihaz belirtin.

    ![Yük devretme özeti](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev11.png)

    Ayrıntılı yönergeler için başarısız için başka bir fiziksel cihaza üzerinden bakın.

7. Bir cihaz geri yükleme iş öğesinden izleyebilirsiniz oluşturulur **işleri** dikey penceresi.

8. İş başarıyla tamamlandıktan sonra yeni cihaz erişim ve gidin **birim kapsayıcıları** dikey penceresi. Eski bir CİHAZDAN tüm birim kapsayıcıları için yeni cihaz geçirildiğini doğrulayın.

   ![Birim kapsayıcıları geçirildi](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev13.png)

9. Yük devretme işlemi tamamlandıktan sonra devre dışı bırakıp portaldan eski cihaz silme. Eski cihaz (çevrimdışı), sağ tıklama seçin ve ardından **devre dışı bırak**. Cihaz devre dışı bırakıldıktan sonra cihazın durumu güncelleştirilir.

     ![Kaynak cihaz devre dışı bırakıldı](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev14.png)

10. Devre dışı bırakılmış bir cihaz, sağ tıklayın ve ardından seçin **Sil**. Bu, cihaz listesinden cihaz siler.

    ![Kaynak cihaz silindi](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev15.png)



## <a name="next-steps"></a>Sonraki adımlar

* Bir yük devretme gerçekleştirdikten sonra gerekebilir [StorSimple Cihazınızı silme veya devre dışı bırakma](storsimple-8000-deactivate-and-delete-device.md).
* StorSimple cihaz Yöneticisi hizmetini kullanma hakkında daha fazla bilgi için Git [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

