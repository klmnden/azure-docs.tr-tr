---
title: Bir StorSimple 8000 serisi cihazını silmek ve devre dışı bırakma | Microsoft Docs
description: StorSimple cihaz hizmetinden önce devre dışı bırakma ve silme kaldırmayı açıklar.
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
ms.date: 07/23/2018
ms.author: alkohli
ms.openlocfilehash: 116ac5c4efda87b5d16336dd326d516299f6955d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61481975"
---
# <a name="deactivate-and-delete-a-storsimple-device"></a>Bir StorSimple cihazını silmek ve devre dışı bırak

## <a name="overview"></a>Genel Bakış

Bu makalede, devre dışı bırakma ve StorSimple cihaz Yöneticisi hizmetine bağlı bir StorSimple cihazını silmek açıklar. Bu makaledeki yönergeler, StorSimple bulut Gereçleri de dahil olmak üzere yalnızca StorSimple 8000 serisi cihazlar için geçerlidir. StorSimple sanal dizisi kullanıyorsanız, geçin [devre dışı bırakma ve silme StorSimple Virtual Array](storsimple-virtual-array-deactivate-and-delete-device.md).

Devre dışı bırakma, cihaz ve karşılık gelen StorSimple cihaz Yöneticisi hizmeti arasındaki bağlantının sunucularından. Hizmet bir StorSimple Cihazınızı (örneğin, değiştirme veya Cihazınızı yükseltmek veya StorSimple artık kullanıyorsanız) almak isteyebilirsiniz. Bu durumda, silebilmeniz için önce cihazı devre dışı bırakmak gerekir.

Bir cihazı devre dışı bıraktığınızda, cihazda yerel olarak depolanan tüm veriler artık erişilebilir değil. Yalnızca bulutta depolanmış cihazla ilişkili veriler kurtarılabilir.

> [!WARNING]
> Devre dışı bırakma işlemi, kalıcı bir işlemdir ve geri alınamaz. Fabrika ayarlarına sıfırlanır sürece StorSimple cihaz Yöneticisi hizmetiyle devre dışı bırakılmış bir cihaz kaydedilemez.
>
> İşlemi siler, Cihazınızda yerel olarak depolanan tüm verileri Fabrika sıfırlayın. Bu nedenle, bir cihazın devre dışı bırakmadan önce tüm verilerinizi bir bulut anlık görüntüsünü alın gerekir. Bu bulut anlık görüntüsü, daha sonraki bir aşamada tüm verileri kurtarmanıza olanak tanır.

Bu öğreticide okuduktan sonra şunları yapabilir:

* Bir cihazı devre dışı bırakmak ve verileri silin.
* Bir cihazı devre dışı bırakmak ve verileri korur.

> [!NOTE]
> StorSimple fiziksel cihaz veya Bulut Gereci devre dışı bırakmadan önce durdurmak veya istemciler ve o cihazda ana bilgisayarları silin.


## <a name="deactivate-and-delete-data"></a>Devre dışı bırakma ve verileri silme

Cihazı tamamen silme ilgilendiğiniz ve cihazdaki verileri elde tutmak istemediğiniz, ardından aşağıdaki adımları tamamlayın.

#### <a name="to-deactivate-the-device-and-delete-the-data"></a>Cihazı devre dışı bırakmak ve verileri silmek için

1. Bir cihazı devre dışı bırakmadan önce tüm birim silmeniz gerekir cihazla ilişkili kapsayıcılar (ve birimleri). Birim kapsayıcıları, ilişkili yedeklemeler yalnızca sildikten sonra silebilirsiniz.

    > [!NOTE]
    > StorSimple fiziksel cihaz veya Bulut Gereci devre dışı bırakmadan önce silinmiş bir birim kapsayıcısı verilerden gerçekten CİHAZDAN silinmeden emin olun. Bulut tüketimi grafikleri izleyebilirsiniz ve sildiğiniz yedeklemeleri nedeniyle doğrudan bulut kullanımı gördüğünüzde, sonra Cihazınızı devre dışı devam edebilirsiniz. Bu açılan gerçekleşmeden önce cihazı devre dışı bırakırsanız, veriler depolama hesabında stranded ve ücretleri tabidir.

2. Cihaz şu şekilde devre dışı bırakın:
   
   1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. İçinde **cihazları** dikey penceresinde devre dışı bırakmak istediğiniz cihazı seçin sağ tıklatın ve ardından **devre dışı bırak**.

        ![StorSimple cihazını devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. İçinde **devre dışı bırak** dikey penceresinde, onaylamak için cihaz adını yazın ve ardından **devre dışı bırak**. Devre dışı bırakma işlemi başlar ve tamamlanması birkaç dakika sürer.

        ![StorSimple cihazını devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)

3. Devre dışı bırakıldıktan sonra cihazın tamamen silebilirsiniz. Bir cihazı silme hizmete bağlı cihazlar listesinden kaldırır. Hizmeti, cihazın silindiğini artık yönetebilirsiniz. Cihazı silmek için aşağıdaki adımları kullanın:
   
   1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. İçinde **cihazları** dikey penceresinde silmek istediğiniz devre dışı bırakılmış cihazı seçin sağ tıklatın ve ardından **Sil**.

        ![StorSimple cihazını devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. İçinde **Sil** dikey penceresinde, onaylamak için cihaz adını yazın ve ardından **Sil**. Silme işlemini tamamlamak için birkaç dakika sürer.

        ![StorSimple cihazını devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. Silme işlemi başarıyla tamamlandığında size bildirilir. Cihaz listesi, aynı zamanda silinmesini yansıtacak şekilde güncelleştirilir.

## <a name="deactivate-and-retain-data"></a>Devre dışı bırakma ve verileri tut

Cihazı silme ilgileniyor ancak verileri korumak istiyorsanız, ardından aşağıdaki adımları tamamlayın:

#### <a name="to-deactivate-a-device-and-retain-the-data"></a>Bir cihazı devre dışı bırakmak ve verileri korumak için
1. Cihaz devre dışı bırakın. Tüm birim kapsayıcılarının ve cihazın anlık görüntüleri olarak kalır.
   
   1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. İçinde **cihazları** dikey penceresinde devre dışı bırakmak istediğiniz cihazı seçin sağ tıklatın ve ardından **devre dışı bırak**.

         ![StorSimple cihazını devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. İçinde **devre dışı bırak** dikey penceresinde, onaylamak için cihaz adını yazın ve ardından **devre dışı bırak**. Devre dışı bırakma işlemi başlar ve tamamlanması birkaç dakika sürer.

         ![StorSimple cihazını devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)
2. Birim kapsayıcıları ve ilişkili anlık görüntüleri üzerinden artık başarısız olabilir. Yordamlar için Git [StorSimple cihazınız için yük devretme ve olağanüstü durum kurtarma](storsimple-8000-device-failover-disaster-recovery.md).
3. Devre dışı bırakma ve yük devretme, cihazın tamamen silebilirsiniz. Bir cihazı silme hizmete bağlı cihazlar listesinden kaldırır. Hizmeti, cihazın silindiğini artık yönetebilirsiniz. Cihazı silmek için aşağıdaki adımları tamamlayın:
   
   1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. İçinde **cihazları** dikey penceresinde silmek istediğiniz devre dışı bırakılmış cihazı seçin sağ tıklatın ve ardından **Sil**.

       ![StorSimple cihazını devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. İçinde **Sil** dikey penceresinde, onaylamak için cihaz adını yazın ve ardından **Sil**. Silme işlemini tamamlamak için birkaç dakika sürer.

       ![StorSimple cihazını devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. Silme işlemi başarıyla tamamlandığında size bildirilir. Cihaz listesi, aynı zamanda silinmesini yansıtacak şekilde güncelleştirilir.

     
## <a name="deactivate-and-delete-a-cloud-appliance"></a>Devre dışı bırakma ve bulut gerecini silme

Bir StorSimple Cloud Appliance için devre dışı bırakma Portalı'ndan kaldırır ve sanal makine ve sağlanırken oluşturulan kaynakları siler. Bulut gereci devre dışı bırakıldıktan sonra, önceki durumuna geri yüklenemez.

![StorSimple bulut Gereci devre dışı bırak](./media/storsimple-8000-deactivate-and-delete-device/deactivate7.png)

Aşağıdaki eylemleri devre dışı bırakma sonuçları:

* StorSimple Cloud Appliance hizmetinden kaldırılır.
* StorSimple Cloud Appliance için sanal makine silindi.
* İşletim sistemi diski ve StorSimple bulut Gereci için oluşturulan veri disklerini korunur. Bu varlıkların kullanmıyorsanız, bunları el ile silmeniz gerekir.
* Barındırılan hizmet ve sağlama sırasında oluşturulan sanal ağ korunur. Bu varlıkların kullanmıyorsanız, bunları el ile silmeniz gerekir.
* StorSimple bulut Gereci tarafından oluşturulan bulut anlık görüntüleri korunur.

Bulut Gereci devre dışı bırakıldıktan sonra aygıt listesinden silebilirsiniz. Devre dışı bırakılmış bir cihaz, sağ tıklayın ve ardından seçin **Sil**. StorSimple, cihaz silindikten sonra ve cihazları güncelleştirmelerin listesini bildirir.

## <a name="next-steps"></a>Sonraki adımlar

* Devre dışı bırakılmış cihazı fabrika ayarlarına geri yüklemek için şu adrese gidin [cihazı fabrika varsayılan ayarlarına geri döndürmeyi](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).
* Teknik destek için [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md).
* StorSimple cihaz Yöneticisi hizmetini kullanma hakkında daha fazla bilgi için şuraya gidin [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

