---
title: StorSimple 8000 serisi yedekleme ilkelerini yönetme | Microsoft Docs
description: El ile yedekleme, yedekleme zamanlamaları ve yedekleme bekletme StorSimple 8000 serisi aygıtta oluşturmak ve yönetmek için StorSimple cihaz Yöneticisi hizmeti nasıl kullanabileceğiniz açıklanır.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/05/2017
ms.author: alkohli
ms.openlocfilehash: 569dbfdeb7dcd526cb5a54b487ea1bfb59b13cc6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23874874"
---
# <a name="use-the-storsimple-device-manager-service-in-azure-portal-to-manage-backup-policies"></a>StorSimple cihaz Yöneticisi hizmeti Azure portalında yedekleme ilkelerini yönetmek için kullanın.


## <a name="overview"></a>Genel Bakış

Bu öğretici StorSimple cihaz Yöneticisi hizmetini kullanma açıklanmaktadır **yedekleme İlkesi** yedekleme işlemleri ve yedekleme bekletme için StorSimple birimlerinizi denetlemek için dikey. Ayrıca, el ile yedekleme tamamlamak nasıl açıklanır.

Bir birim yedeklediğinizde, yerel bir anlık görüntüsü veya bir bulut anlık görüntüsü oluşturmayı seçebilirsiniz. Yerel olarak sabitlenmiş bir birim yedekleme yapıyorsanız, bir bulut anlık görüntüsü belirtilmesi önerilir. Çok sayıda karmaşası çok sahip bir veri kümesi ile birlikte yerel olarak sabitlenmiş bir birim yerel anlık görüntüleri alma hızla yerel alana çalıştırabilir durumda neden olur. Yerel anlık görüntüsünü seçerseniz, en son duruma geri, bunları bir gün için korumak için daha az günlük anlık görüntülerini almak ve bunları silmek öneririz.

Yerel olarak sabitlenmiş bir birim bir bulut anlık görüntüsünü, burada, yinelenenleri kaldırılmış sıkıştırılmış ve buluta, yalnızca değiştirilen verileri kopyalayın.

## <a name="the-backup-policy-blade"></a>Yedekleme İlkesi dikey penceresi

**Yedekleme İlkesi** dikey StorSimple cihazınız için yedekleme ilkelerini yönetmek ve yerel zamanlamak ve bulut anlık görüntüleri olanak tanır. Yedekleme ilkeleri yedekleme zamanlamaları ve yedekleme bekletme birimlerin bir koleksiyon için yapılandırmak için kullanılır. Yedekleme ilkeleri aynı anda birden çok birim bir anlık görüntüsünü olanak tanır. Bu, bir yedekleme İlkesi tarafından oluşturulan yedekleri kilitlenme tutarlı kopyaları olacağı anlamına gelir.

Yedekleme ilkeleri sekmeli liste varolan yedekleme ilkeleri bir veya daha fazla aşağıdaki alanları tarafından filtre sağlar:

* **İlke adı** – ilkeyle ilişkili ad. İlkeleri farklı türleri şunlardır:

  * Zamanlanmış ilkeleri, kullanıcı tarafından açıkça oluşturulur.
  * StorSimple anlık görüntü Yöneticisi'nde ilk olarak oluşturulan alınan ilkeleri. Bu ilkeleri alındığı StorSimple Snapshot Manager konak açıklayan bir etiket vardır.

  > [!NOTE]
  > Otomatik veya varsayılan yedekleme ilkeleri artık birim oluşturma sırasında etkinleştirilir.

* **Son başarılı yedekleme** – Bu ilkeyle alındığı son başarılı yedekleme saati ve tarihi.

* **Sonraki yedekleme** – Bu ilke tarafından başlatılan bir sonraki zamanlanmış yedekleme saati ve tarihi.

* **Birimleri** – ilkeyle ilişkili birimler. Yedekleme oluşturulduğunda bir yedekleme ilkesiyle ilişkili tüm birimler birlikte gruplandırılır.

* **Zamanlamalar** – yedekleme ilkeyle ilişkilendirilmiş zamanlamaları sayısı.

Yedekleme ilkelerine gerçekleştirebileceğiniz sık kullanılan işlemleri şunlardır:

* Yedekleme ilkesi ekleme
* Ekleyin veya bir zamanlama değiştirin
* Bir birim Ekle Kaldır
* Bir yedekleme ilkesi silme
* El ile yedekleyin

## <a name="add-a-backup-policy"></a>Yedekleme ilkesi ekleme

Otomatik olarak Yedeklemelerinizin zamanlamak için bir yedekleme ilkesi ekleyin. Bir birim ilk oluşturduğunuzda, biriminiz ile ilişkili varsayılan yedekleme ilkesi bulunmamaktadır. Ekleme ve birim verilerini korumak için bir yedekleme ilkesi atama gerekir.

StorSimple cihazınız için bir yedekleme ilkesi eklemek için Azure portalında aşağıdaki adımları gerçekleştirin. İlkeyi ekledikten sonra bir zamanlama tanımlayabilirsiniz (bkz [Ekle ya da bir zamanlamayı değiştirmek](#add-or-modify-a-schedule)).

[!INCLUDE [storsimple-8000-add-backup-policy-u2](../../includes/storsimple-8000-add-backup-policy-u2.md)]

## <a name="add-or-modify-a-schedule"></a>Ekleyin veya bir zamanlama değiştirin

Ekleyin veya mevcut bir yedekleme İlkesi, StorSimple Cihazınızda bağlı olduğu bir zamanlama değiştirin. Eklemek veya bir zamanlamayı değiştirmek için Azure portalında aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-8000-add-modify-backup-schedule](../../includes/storsimple-8000-add-modify-backup-schedule-u2.md)]


## <a name="add-or-remove-a-volume"></a>Bir birim Ekle Kaldır

Ekleyebilir veya bir yedekleme İlkesi, StorSimple Cihazınızda atanmış bir birim kaldırabilirsiniz. Eklemek veya bir birimden kaldırmak için Azure portalında aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-8000-add-volume-backup-policy-u2](../../includes/storsimple-8000-add-remove-volume-backup-policy-u2.md)]


## <a name="delete-a-backup-policy"></a>Bir yedekleme ilkesi silme

Azure portalında bir yedekleme İlkesi, StorSimple Cihazınızda silmek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-8000-delete-backup-policy](../../includes/storsimple-8000-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a>El ile yedekleyin

Tek bir birim için bir isteğe bağlı (el ile) yedekleme oluşturmak için Azure portalında aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-8000-create-manual-backup](../../includes/storsimple-8000-create-manual-backup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanarak](storsimple-8000-manager-service-administration.md).

