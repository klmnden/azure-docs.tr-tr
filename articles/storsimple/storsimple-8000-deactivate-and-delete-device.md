---
title: "Devre dışı bırakın ve bir StorSimple 8000 serisi aygıtı silme | Microsoft Docs"
description: "StorSimple cihazı hizmetinden önce devre dışı bırakma ve silme kaldırmayı açıklar."
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
ms.date: 05/23/2017
ms.author: alkohli
ms.openlocfilehash: 3c00867a29cf8343a57e74e2aabe3971ae6837af
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="deactivate-and-delete-a-storsimple-device"></a>Devre dışı bırakın ve bir StorSimple cihazı Sil

## <a name="overview"></a>Genel Bakış

Bu makalede, devre dışı bırakın ve bir StorSimple cihaz Yöneticisi hizmetine bağlı bir StorSimple cihazı silmek açıklar. Bu makalede yer alan yönergeleri StorSimple bulut cihazları dahil olmak üzere yalnızca StorSimple 8000 serisi cihazlar için geçerlidir. Bir StorSimple sanal dizisi kullanıyorsanız, geçin [devre dışı bırakma ve silme StorSimple sanal dizinin](storsimple-virtual-array-deactivate-and-delete-device.md).

Cihaz ve karşılık gelen StorSimple cihaz Yöneticisi hizmeti arasındaki bağlantı devre dışı bırakma sunucularının. Hizmet dışına StorSimple cihazı (örneğin, değiştirme veya Cihazınızı yükseltmek veya StorSimple artık kullanıyorsanız) almak isteyebilirsiniz. Öyleyse, silebilmek için önce cihazın devre dışı bırakılması gerekir.

Bir aygıtı devre dışı bıraktığınızda, cihazda yerel olarak depolanan tüm verileri artık erişilebilir değil. Yalnızca bulutta depolanan cihaz ile ilişkili verileri kurtarılabilir.

> [!WARNING]
> Devre dışı bırakma kalıcı bir işlemdir ve geri alınamaz. Fabrika ayarlarına sıfırlama sürece devre dışı bırakılan bir cihaz StorSimple cihaz Yöneticisi hizmeti ile kaydedilemedi.
>
> İşlem siler, cihazda yerel olarak depolanan tüm verileri fabrika ayarlarına sıfırlayamaz. Bu nedenle, bir aygıtı devre dışı bırakmadan önce tüm verilerinizi bir bulut anlık görüntüsünü gerekir. Bu bulut anlık görüntüsü, daha sonraki bir aşamada tüm verileri kurtarmanıza olanak tanır.

Bu öğretici okuduktan sonra aşağıdakileri gerçekleştirebilirsiniz:

* Bir cihazı devre dışı bırakmak ve verileri silebilirsiniz.
* Bir aygıtı devre dışı bırakın ve verileri korur.

> [!NOTE]
> Bir StorSimple fiziksel cihazı veya Bulut uygulaması devre dışı bırakmadan önce durdurmak veya istemciler ve bu cihazda bağımlı konakları silin.


## <a name="deactivate-and-delete-data"></a>Devre dışı bırakın ve verileri silme

Cihaz tamamen silme ilgilendiğiniz ve cihazdaki verilerin korumak istemiyorsanız, aşağıdaki adımları tamamlayın.

#### <a name="to-deactivate-the-device-and-delete-the-data"></a>Aygıt devre dışı bırak ve veri silmek için

1. Bir aygıtı devre dışı bırakmadan önce tüm birim silmeniz gerekir aygıtla ilişkili kapsayıcıları (ve birimleri). Birim kapsayıcıları, ilişkili yedeklemelerinizi yalnızca sildikten sonra silebilirsiniz.

    > [!NOTE]
    > Bir StorSimple fiziksel cihazı veya Bulut uygulaması devre dışı bırakmadan önce silinen birim kapsayıcısı verileri gerçekten aygıttan silindiğinden emin olun. Bulut tüketim grafikleri izleyebilirsiniz ve sildiğiniz yedeklemeleri nedeniyle bırakma bulut kullanımı gördüğünüzde, daha sonra aygıt devre dışı bırakmak için geçebilirsiniz. Bu açılan oluşmadan önce aygıt devre dışı bırakırsanız, veri depolama hesabında stranded ve ücretler tahakkuk.

2. Cihaz şu şekilde devre dışı bırakın:
   
   1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. İçinde **aygıtları** dikey penceresinde, devre dışı bırakmak istediğiniz cihazı seçin sağ tıklatın ve ardından **etkinliğini**.

        ![StorSimple cihazı devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. İçinde **etkinliğini** dikey penceresinde, onaylamak için aygıt adı yazın ve ardından **etkinliğini**. Devre dışı bırakma işlemi başlatılır ve tamamlanması birkaç dakika sürer.

        ![StorSimple cihazı devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)

3. Devre dışı bırakma sonra cihaz tamamen silebilirsiniz. Bir cihazı silme hizmete bağlı aygıtlar listesinden kaldırır. Hizmeti, silinen cihaz artık yönetebilirsiniz. Cihazı silmek için aşağıdaki adımları kullanın:
   
   1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. İçinde **aygıtları** dikey penceresinde, silmek istediğiniz devre dışı bırakılan cihazı seçin sağ tıklatın ve ardından **silmek**.

        ![StorSimple cihazı devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. İçinde **silmek** dikey penceresinde, onaylamak için aygıt adı yazın ve ardından **silmek**. Silme işlemini tamamlamak için birkaç dakika sürer.

        ![StorSimple cihazı devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. Silme işlemi başarıyla tamamlandıktan sonra size bildirilir. Aygıt listesi ayrıca silme yansıtacak şekilde güncelleştirir.

## <a name="deactivate-and-retain-data"></a>Devre dışı bırakın ve verileri tut

Cihaz silme ilginizi çekiyor mu, ancak verileri korumak istiyorsanız, aşağıdaki adımları tamamlayın:

#### <a name="to-deactivate-a-device-and-retain-the-data"></a>Bir cihazı devre dışı bırakmak ve verileri korumak için
1. Aygıt devre dışı bırakın. Tüm birim kapsayıcıları ve cihaz anlık görüntüleri olarak kalır.
   
   1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. İçinde **aygıtları** dikey penceresinde, devre dışı bırakmak istediğiniz cihazı seçin sağ tıklatın ve ardından **etkinliğini**.

         ![StorSimple cihazı devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. İçinde **etkinliğini** dikey penceresinde, onaylamak için aygıt adı yazın ve ardından **etkinliğini**. Devre dışı bırakma işlemi başlatılır ve tamamlanması birkaç dakika sürer.

         ![StorSimple cihazı devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)
2. Şimdi birim kapsayıcıları ve ilişkili anlık görüntüleri üzerinden başarısız olabilir. Yordamlar için Git [StorSimple cihazınız için yük devretme ve olağanüstü durum kurtarma](storsimple-8000-device-failover-disaster-recovery.md).
3. Devre dışı bırakma ve yük devretme aygıtı tamamen silebilirsiniz. Bir cihazı silme hizmete bağlı aygıtlar listesinden kaldırır. Hizmeti, silinen cihaz artık yönetebilirsiniz. Cihazı silmek için aşağıdaki adımları tamamlayın:
   
   1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. İçinde **aygıtları** dikey penceresinde, silmek istediğiniz devre dışı bırakılan cihazı seçin sağ tıklatın ve ardından **silmek**.

       ![StorSimple cihazı devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. İçinde **silmek** dikey penceresinde, onaylamak için aygıt adı yazın ve ardından **silmek**. Silme işlemini tamamlamak için birkaç dakika sürer.

       ![StorSimple cihazı devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. Silme işlemi başarıyla tamamlandıktan sonra size bildirilir. Aygıt listesi ayrıca silme yansıtacak şekilde güncelleştirir.

     
## <a name="deactivate-and-delete-a-cloud-appliance"></a>Devre dışı bırakın ve bir bulut uygulaması Sil

Bir StorSimple bulut uygulaması için devre dışı bırakma portalından kaldırır ve sanal makine ve hazırlandığında oluşturulan kaynakları siler. Bulut gereci devre dışı bırakıldıktan sonra, önceki durumuna geri yüklenemez.

![StorSimple bulut uygulaması devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate7.png)

Aşağıdaki eylemleri sonuçlarında devre dışı bırakma:

* StorSimple bulut uygulaması hizmetten kaldırılır.
* StorSimple bulut uygulaması için sanal makine silinir.
* İşletim sistemi diski ve StorSimple bulut uygulaması için oluşturulan veri diskleri kaldırılır.
* Barındırılan hizmet ve sağlama işlemi sırasında oluşturulan sanal ağ korunur. Bu varlıklar kullanmıyorsanız, bunları el ile silmeniz gerekir.
* StorSimple bulut uygulaması tarafından oluşturulan bulut anlık görüntüleri korunur.

Bulut uygulaması devre dışı bırakıldıktan sonra aygıt listesinden silebilirsiniz. Devre dışı bırakılmış aygıt, sağ tıklatın ve ardından seçin **silmek**. StorSimple, cihaz silindikten sonra ve aygıtları güncelleştirmelerinin listesini bildirir.

## <a name="next-steps"></a>Sonraki adımlar

* Devre dışı bırakılan cihazı fabrika varsayılan ayarlarına geri yüklemek için şu adrese gidin [cihazı fabrika varsayılan ayarlarına sıfırlama](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).
* Teknik destek için [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md).
* StorSimple cihaz Yöneticisi hizmetini kullanma hakkında daha fazla bilgi için şuraya gidin [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

