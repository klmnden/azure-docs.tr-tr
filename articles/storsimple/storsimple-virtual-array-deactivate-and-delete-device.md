---
title: Devre dışı bırakın ve bir Microsoft Azure StorSimple Virtual Array delete | Microsoft Docs
description: StorSimple cihaz hizmetinden önce devre dışı bırakma ve silme kaldırmayı açıklar.
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
ms.openlocfilehash: bb1a56d204a46f89213f20e317494120f0ea565e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60580637"
---
# <a name="deactivate-and-delete-a-storsimple-virtual-array"></a>Devre dışı bırakma ve StorSimple sanal dizisi Sil

## <a name="overview"></a>Genel Bakış

StorSimple sanal dizisi devre dışı bıraktığınızda, karşılık gelen StorSimple cihaz Yöneticisi hizmeti ile cihaz arasındaki bağlantıyı kes. Bu öğreticide, aşağıdaki işlemlerin nasıl yapılacağı açıklanmaktadır:

* Bir cihazı devre dışı 
* Devre dışı bırakılmış bir cihaz silme

Bu makaledeki bilgiler, yalnızca StorSimple sanal diziler için geçerlidir. Nasıl yapılır 8000 serisi hakkında daha fazla bilgi için Git [bir cihazı silmek veya devre dışı bırakma](storsimple-deactivate-and-delete-device.md).

## <a name="when-to-deactivate"></a>Devre dışı bırakmak ne zaman?

Devre dışı bırakma işlemi, kalıcı bir işlemdir ve geri alınamaz. StorSimple cihaz Yöneticisi hizmetine yeniden devre dışı bırakılmış bir cihaz kaydedilemiyor. Devre dışı bırakma ve silme StorSimple sanal dizisi aşağıdaki senaryolarda gerekebilir:

* **Planlı yük devretme** : Cihazınız çevrimiçi olduktan ve Cihazınızda yük devretme gerçekleştirme planlayın. Daha büyük bir cihazın yükseltileceği planlıyorsanız, Cihazınızda yük devretme gerçekleştirme gerekebilir. Veri sahipliği aktarılır ve yük devretme işlemi tamamlandıktan sonra kaynak cihaz otomatik olarak silinir.
* **Planlanmamış yük devretme** : Cihazınız çevrimdışıysa ve cihazın yükü başarısız gerekmez. Bu senaryo, aşağı birincil Cihazınızı veri merkezinde bir kesinti olduğunda ve bir olağanüstü durum sırasında ortaya çıkabilir. İkincil bir cihaz için cihaz yük devretme planlayın. Veri sahipliği aktarılır ve yük devretme işlemi tamamlandıktan sonra kaynak cihaz otomatik olarak silinir.
* **Yetkisini** : Cihaz kullanımdan istiyorsunuz. Bu, ilk cihaz devre dışı bırakın ve ardından silin gerektirir. Bir cihazı devre dışı bıraktığınızda, yerel olarak depolanan tüm veriler artık erişemez. Yalnızca erişmek ve bulutta depolanan verileri kurtarın. Cihaz verilerini devre dışı bırakıldıktan sonra devam etmeyi planlıyorsanız, bir cihazın devre dışı bırakmadan önce tüm verilerinizi bir bulut anlık görüntüsünü almanız gerekir. Bu bulut anlık görüntüsü, daha sonraki bir aşamada tüm verileri kurtarmanıza olanak tanır.

## <a name="deactivate-a-device"></a>Bir cihazı devre dışı

Cihazınızı devre dışı bırakmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-deactivate-the-device"></a>Cihaz devre dışı bırakmak için

1. Hizmetiniz, Git **Yönetim > cihazlar**. İçinde **cihazları** dikey penceresinde tıklayın ve devre dışı bırakmak istediğiniz cihazı seçin.
   
    ![Cihazı devre dışı bırakmak için seçin](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete7.png)
2. İçinde **cihaz Panosu** dikey penceresinde tıklayın **... Daha fazla** ve listesinden **devre dışı bırak**.
   
    ![Click devre dışı bırak](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete8.png)
3. İçinde **devre dışı bırak** dikey penceresinde, cihaz adını yazın ve ardından **devre dışı bırak**. 
   
    ![Onayla devre dışı bırak](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete1.png)
   
    Devre dışı bırakma işlemi başlar ve tamamlanması birkaç dakika sürer.
   
    ![Devam eden devre dışı bırak](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete2.png)
4. Devre dışı bırakıldıktan sonra cihazların listesini yeniler.
   
    ![Tam devre dışı bırak](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete3.png)
   
    Artık bu cihazı silebilirsiniz.

## <a name="delete-the-device"></a>Cihazı silme

Bir cihazı silmek için önce etkinliği var. Bir cihazı silme hizmete bağlı cihazlar listesinden kaldırır. Hizmeti, cihazın silindiğini artık yönetebilirsiniz. Cihazla ilişkili verileri, ancak bulutta kalır. Bu veriler daha sonra ücretleri tabidir.

Cihazı silmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-delete-the-device"></a>Cihazı silmek için

1. Git, StorSimple cihaz Yöneticisi'nde **Yönetim > cihazlar**. İçinde **cihazları** dikey penceresinde silmek istediğiniz devre dışı bırakılmış bir cihaz seçin.
2. İçinde **cihaz Panosu** dikey penceresinde tıklayın **... Daha fazla** ve ardından **Sil**.
   
   ![Silmek için cihazı seçin](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete4.png)
3. İçinde **Sil** dikey penceresinde, adını, Cihazınızı silme işlemini onaylayın ve ardından **Sil**. Cihaz silme cihazla ilgili bulut verilerinin silinmesine neden olmaz. 
   
   ![Silmeyi onayla](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete5.png) 
4. Silme işlemi başlar ve tamamlanması birkaç dakika sürer.
   
   ![Silme işlemi devam ediyor](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete6.png)
   
    Cihaz silindikten sonra güncelleştirilen cihaz listesini görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Yük devretme hakkında daha fazla bilgi için Git [StorSimple Virtual Array'iniz yük devretme ve olağanüstü durum kurtarma](storsimple-virtual-array-failover-dr.md).

* StorSimple cihaz Yöneticisi hizmetini kullanma hakkında daha fazla bilgi için şuraya gidin [StorSimple Virtual Array'iniz yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-virtual-array-manager-service-administration.md). 

