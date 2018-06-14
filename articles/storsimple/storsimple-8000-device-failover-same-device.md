---
title: StorSimple yük devretme, 8000 serisi cihazlar için olağanüstü durum kurtarma | Microsoft Docs
description: StorSimple Cihazınızı aynı cihaza yük devri öğrenin.
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
ms.openlocfilehash: acc8929dc3476e9590e8e4d9526b38b7c0719570
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23874825"
---
# <a name="fail-over-your-storsimple-physical-device-to-same-device"></a>StorSimple fiziksel cihazınız aynı cihaza yük devri

## <a name="overview"></a>Genel Bakış

Bu öğretici, StorSimple 8000 serisi fiziksel bir aygıtı kendisine bir olağanüstü durum varsa devretmek için gereken adımları açıklar. StorSimple cihaz yük devretme özelliğini veri merkezindeki fiziksel bir kaynak aygıtı başka bir fiziksel cihaza geçirmek için kullanır. Bu öğreticideki yönergeler yazılım sürümleri güncelleştirme 3 ve sonraki sürümleri çalıştıran fiziksel cihazları StorSimple 8000 serisi için geçerlidir.

Cihaz yük devretme ve olağanüstü durumdan kurtarmak için nasıl kullanıldığı hakkında daha fazla bilgi için şuraya gidin [StorSimple 8000 serisi cihazlar için yük devretme ve olağanüstü durum kurtarma](storsimple-8000-device-failover-disaster-recovery.md).

Başka bir fiziksel aygıt fiziksel bir cihaza yük devretme için Git [aynı StorSimple fiziksel cihazı yük devri](storsimple-8000-device-failover-physical-device.md). Bir StorSimple bulut uygulaması StorSimple fiziksel cihaza yük devretme için Git [StorSimple bulut uygulaması için yük devri](storsimple-8000-device-failover-cloud-appliance.md).


## <a name="prerequisites"></a>Ön koşullar

- Cihaz yük devretme için konuları gözden geçirdiğinizden emin olun. Daha fazla bilgi için Git [aygıt yük devretme için ortak konuları](storsimple-8000-device-failover-disaster-recovery.md).


## <a name="steps-to-fail-over-to-the-same-device"></a>Aynı aygıt üzerinden vermesine adımları

Aynı cihaza yük devri gerekiyorsa, aşağıdaki adımları gerçekleştirin.

1. Tüm birimlerin bulut anlık görüntülerini kullanarak Cihazınızı alın. Daha fazla bilgi için Git [yedeklemeler oluşturmak için Aygıt Yöneticisi'ni StorSimple hizmeti](storsimple-8000-manage-backup-policies-u2.md).
2. Cihazınızı fabrika ayarlarına döndürebilir. Ayrıntılı yönergeleri izleyin [StorSimple cihazı fabrika varsayılan ayarlarına sıfırlamaya](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).
3. StorSimple cihaz Yöneticisi hizmetine gidin ve ardından **aygıtları**. İçinde **aygıtları** eski aygıt dikey penceresinde Göster olarak **çevrimdışı**.

    ![Kaynak aygıt çevrimdışı](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev2.png)

4. Cihazınızı yapılandırmak ve StorSimple cihaz Yöneticisi hizmetiniz ile yeniden kaydedin. Yeni kaydettiğiniz aygıt olarak göstermelidir **ayarlamak hazır**. Yeni cihaz için cihaz adı eski aygıt ile aynıdır ancak cihazı fabrika varsayılan değerlerine Sıfırla ve yeniden kayıtlı olduğunu belirten bir sayı eklenir.

    ![Yeni kaydettiğiniz aygıt ayarlamak hazır](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev3.png)
5. Yeni cihaz için cihaz kurulumunu tamamlayın. Daha fazla bilgi için Git [4. adım: minimum cihaz kurulumunu Tamamla](storsimple-8000-deployment-walkthrough-u2.md#step-4-complete-minimum-device-setup). Üzerinde **aygıtları** dikey penceresinde, cihazın durumu değişiklikleri **çevrimiçi**.

   > [!IMPORTANT]
   > **En düşük yapılandırmayı ilk tamamlamak ya da, DR başarısız olabilir.**

    ![Yeni kaydedilen cihaz çevrimiçi](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev7.png)

6. Eski aygıt (çevrimdışı durumu) seçin ve Komut çubuğundaki **yük devri**. İçinde **yük devri** dikey penceresinde kaynak olarak eski aygıt seçin ve hedef aygıt yeni kayıtlı cihazı olarak belirtin.

    ![Yük devretme özeti](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev11.png)

    Ayrıntılı yönergeler için bkz [başka bir fiziksel cihaza yük devri](#fail-over-to-another-physical-device).

7. Bir aygıt geri yükleme iş öğesinden izleyebilirsiniz oluşturulur **işleri** dikey.

8. İş başarıyla tamamlandıktan sonra yeni cihaz erişmek ve gidin **birim kapsayıcıları** dikey. Eski aygıt tüm birim kapsayıcıları için yeni cihaz geçirildiğini doğrulayın.

   ![Birim kapsayıcıları geçişi](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev13.png)

9. Yük devretme işlemi tamamlandıktan sonra devre dışı bırakmanız ve portaldan eski aygıt silme. Eski aygıt (çevrimdışı), sağ seçin ve ardından **etkinliğini**. Aygıt devre dışı bırakıldıktan sonra aygıtın durumu güncelleştirilir.

     ![Kaynak aygıt devre dışı](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev14.png)

10. Devre dışı bırakılmış aygıt, sağ tıklatın ve ardından seçin **silmek**. Bu aygıtın aygıt listesinden siler.

    ![Kaynak aygıt silindi](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev15.png)



## <a name="next-steps"></a>Sonraki adımlar

* Bir yük devretme gerçekleştirdikten sonra gerekebilir [StorSimple Cihazınızı silmek veya devre dışı bırakma](storsimple-8000-deactivate-and-delete-device.md).
* StorSimple cihaz Yöneticisi hizmetini kullanma hakkında daha fazla bilgi için Git [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

