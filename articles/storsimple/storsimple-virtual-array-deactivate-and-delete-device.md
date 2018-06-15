---
title: Devre dışı bırakın ve bir Microsoft Azure StorSimple sanal dizinin Sil | Microsoft Docs
description: StorSimple cihazı hizmetinden önce devre dışı bırakma ve silme kaldırmayı açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: carmonm
editor: ''
ms.assetid: a929f5bc-03e2-4b01-b925-973db236f19f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: alkohli
ms.openlocfilehash: 8dea36f92b034f8c6cdb6875634848d37f4c6606
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23875833"
---
# <a name="deactivate-and-delete-a-storsimple-virtual-array"></a>Devre dışı bırakın ve bir StorSimple sanal dizi Sil

## <a name="overview"></a>Genel Bakış

StorSimple sanal dizinin devre dışı bıraktığınızda, aygıt ve karşılık gelen StorSimple cihaz Yöneticisi hizmeti arasındaki bağlantıyı kesmek. Bu öğretici açıklar nasıl yapılır:

* Bir cihazı devre dışı 
* Devre dışı bırakılmış bir aygıtı silme

Bu makaledeki bilgileri yalnızca StorSimple sanal diziler için geçerlidir. Nasıl yapılır 8000 serisi hakkında daha fazla bilgi için Git [bir cihazı silmek veya devre dışı bırakma](storsimple-deactivate-and-delete-device.md).

## <a name="when-to-deactivate"></a>Ne zaman devre dışı bırakmak?

Devre dışı bırakma kalıcı bir işlemdir ve geri alınamaz. StorSimple cihaz Yöneticisi hizmeti ile yeniden devre dışı bırakılmış bir aygıt kaydedilemiyor. Devre dışı bırakın ve bir StorSimple sanal dizisi aşağıdaki senaryolarda silme gerekebilir:

* **Planlanan yük devretme** : Cihazınızı çevrimiçi olduğundan ve Cihazınızı vermesine planlayın. Daha büyük bir aygıta yükseltme planlıyorsanız, Cihazınızı vermesine gerekebilir. Veri sahipliği aktarılır ve yük devretme işlemi tamamlandıktan sonra kaynak cihaz otomatik olarak silinir.
* **Planlanmamış yük devretme** : Cihazınızı çevrimdışıysa ve aygıt üzerinden vermesine gerekir. Bu senaryoda veri merkezinde bir kesinti olduğunda ve aşağı birincil Cihazınızı olağanüstü durum sırasında ortaya çıkabilir. İkincil bir aygıt cihaza yük devri planlayın. Veri sahipliği aktarılır ve yük devretme işlemi tamamlandıktan sonra kaynak cihaz otomatik olarak silinir.
* **Yetkisini** : cihaz yetkisini istiyor. Bu, ilk aygıt devre dışı bırakın ve ardından silmek gerektirir. Bir aygıtı devre dışı bıraktığınızda, yerel olarak depolanan tüm verileri artık erişemez. Yalnızca erişmek ve bulutta depolanan verileri kurtarabilirsiniz. Devre dışı bırakma sonra cihaz verileri tutmak planlıyorsanız, bir cihazın devre dışı bırakmadan önce tüm verilerinizin bulut anlık görüntüsü almanız gerekir. Bu bulut anlık görüntüsü, daha sonraki bir aşamada tüm verileri kurtarmanıza olanak tanır.

## <a name="deactivate-a-device"></a>Bir cihazı devre dışı

Cihazınızı devre dışı bırakmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-deactivate-the-device"></a>Aygıt devre dışı bırakmak için

1. Hizmetiniz, Git **Yönetim > aygıtları**. İçinde **aygıtları** dikey penceresinde,'ı tıklatın ve devre dışı bırakmak istediğiniz cihazı seçin.
   
    ![Devre dışı bırakmak için cihazı seçin](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete7.png)
2. İçinde **cihaz Pano** dikey penceresinde tıklatın **... Daha fazla** ve listesinden **etkinliğini**.
   
    ![Tıklatın devre dışı bırakma](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete8.png)
3. İçinde **etkinliğini** dikey penceresinde, cihaz adını yazın ve ardından **etkinliğini**. 
   
    ![Onayla devre dışı bırakma](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete1.png)
   
    Devre dışı bırakma işlemi başlatılır ve tamamlanması birkaç dakika sürer.
   
    ![Devam eden devre dışı bırakma](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete2.png)
4. Devre dışı bırakma sonra cihaz listesini yeniler.
   
    ![Tam devre dışı bırakma](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete3.png)
   
    Bu cihaz artık silebilirsiniz.

## <a name="delete-the-device"></a>Aygıtı silme

Bir cihazı silmek için önce etkinliği var. Bir cihazı silme hizmete bağlı aygıtlar listesinden kaldırır. Hizmeti, silinen cihaz artık yönetebilirsiniz. Cihaz ile ilişkili veri ancak bulutta kalır. Bu veriler, ardından ücretler tahakkuk.

Cihazı silmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-delete-the-device"></a>Cihazı silmek için

1. StorSimple Aygıt Yöneticisi'nde Git **Yönetim > aygıtları**. İçinde **aygıtları** dikey penceresinde, silmek istediğiniz devre dışı bırakılan bir cihaz seçin.
2. İçinde **cihaz Pano** dikey penceresinde tıklatın **... Daha fazla** ve ardından **silmek**.
   
   ![Silme için aygıt seçin](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete4.png)
3. İçinde **silmek** dikey penceresinde silme işlemini onaylayın ve ardından Cihazınızı adını yazın **silmek**. Cihaz silme cihaz ile ilişkili bulut verilerini silmez. 
   
   ![Silmeyi onayla](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete5.png) 
4. Silme işlemini başlatır ve tamamlanması birkaç dakika sürer.
   
   ![Silme sürüyor](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete6.png)
   
    Cihaz silindikten sonra güncelleştirilen cihaz listesini görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Yük devretme hakkında daha fazla bilgi için Git [yük devretme ve olağanüstü durum kurtarma, StorSimple sanal dizinin](storsimple-virtual-array-failover-dr.md).

* StorSimple cihaz Yöneticisi hizmetini kullanma hakkında daha fazla bilgi için şuraya gidin [StorSimple sanal dizinizi yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-virtual-array-manager-service-administration.md). 

