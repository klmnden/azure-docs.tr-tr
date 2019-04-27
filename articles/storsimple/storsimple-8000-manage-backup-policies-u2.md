---
title: StorSimple 8000 serisi yedekleme ilkelerini yönetme | Microsoft Docs
description: El ile yedekleme, yedekleme zamanlamaları ve yedekleme bekletme bir StorSimple 8000 serisi cihaz üzerinde oluşturmak ve yönetmek için StorSimple cihaz Yöneticisi hizmetini nasıl kullanabileceğini açıklar.
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
ms.openlocfilehash: 607379f8645226a031646376df9ca18f4d3164bf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60819190"
---
# <a name="use-the-storsimple-device-manager-service-in-azure-portal-to-manage-backup-policies"></a>StorSimple cihaz Yöneticisi hizmeti, yedekleme ilkelerini yönetmek için Azure Portalı'nda kullanın.


## <a name="overview"></a>Genel Bakış

Bu öğreticide, StorSimple cihaz Yöneticisi hizmetini kullanmayı açıklar **yedekleme İlkesi** dikey penceresinde, yedekleme işlemlerini ve yedekleme bekletme için StorSimple birimlerinizi denetlemek için. Ayrıca, el ile yedekleme işlemi nasıl açıklar.

Bir birimin yedeklediğinizde, yerel anlık görüntü veya bir bulut anlık görüntüsünü oluşturmak seçebilirsiniz. Bir yerel olarak sabitlenmiş birimin yedekliyorsanız, bir bulut anlık görüntüsünü belirtmenizi öneririz. Çok sayıda değişim çok sahip bir veri kümesi ile birlikte yerel olarak sabitlenmiş birimin yerel anlık görüntüler alma hızla yerel alanı yetersiz çalıştırabilir durumda neden olur. Yerel anlık görüntülerini almak isterseniz, bunları bir gün için en son durumu geri korumak için daha az günlük anlık görüntülerini alabilir ve bunları silin öneririz.

Yerel olarak sabitlenmiş birimin bir bulut anlık görüntüsünü alın, burada, yinelenen verileri kaldırma işlemi sıkıştırılmış ve buluta yalnızca değiştirilen verileri kopyalayın.

## <a name="the-backup-policy-blade"></a>Yedekleme İlkesi dikey penceresi

**Yedekleme İlkesi** dikey StorSimple cihazınız için yedekleme ilkelerini yönetme ve yerel zamanlayın ve bulut anlık görüntüleri olanak tanır. Yedekleme ilkelerini, yedekleme zamanlamaları ve yedekleme bekletmede birimlerin bir koleksiyon için yapılandırmak için kullanılır. Yedekleme ilkelerini aynı anda birden çok birim anlık görüntüsünü almak etkinleştirin. Bu, bir yedekleme İlkesi tarafından oluşturulan yedekleri kilitlenmeyle tutarlı kopyaları olacağı anlamına gelir.

Yedekleme ilkelerini tablosal bir veya birden çok aşağıdaki alanları mevcut yedekleme ilkelerini filtrelemenize olanak sağlar:

* **İlke adı** -ilkeyle ilişkili ad. Farklı türde ilkeler şunlardır:

  * Zamanlanmış ilkeleri, kullanıcı tarafından açıkça oluşturulur.
  * StorSimple Snapshot Manager'da başlangıçta oluşturulan içeri aktarılan ilkeleri. Bu ilkeler alındığı StorSimple Snapshot Manager konak açıklayan bir etiketi vardır.

  > [!NOTE]
  > Otomatik veya varsayılan yedekleme İlkesi artık birim oluşturma sırasında etkinleştirilir.

* **Son başarılı yedeklemenin** – Bu ilkeyle alınan son başarılı yedeklemenin saati ve tarihi.

* **Sonraki yedeklemede** – tarih ve saat bu ilke tarafından başlatılan sonraki zamanlanmış yedekleme.

* **Birimleri** – ilkesiyle ilişkili birimler. Yedekleme oluşturulduğunda bir yedekleme ilkesiyle ilişkili birimler birlikte gruplandırılır.

* **Zamanlamalar** – yedekleme ilkesiyle ilişkili zamanlamaları sayısı.

Yedekleme ilkelerini gerçekleştirebileceğiniz sık kullanılan işlemleri şunlardır:

* Yedekleme ilkesi ekleme
* Bir zamanlamayı değiştirmek veya ekleme
* Birim Ekle Kaldır
* Yedekleme ilkesini silme
* El ile yedekleyin

## <a name="add-a-backup-policy"></a>Yedekleme ilkesi ekleme

Otomatik olarak yedeklemelerinizi zamanlamak için bir yedekleme ilkesi ekleyin. Bir birim ilk oluşturduğunuzda biriminiz ile ilişkili hiçbir varsayılan yedekleme İlkesi yok. Eklemeniz ve birim verilerini korumak için bir yedekleme İlkesi atamanız gerekir.

Azure portalında StorSimple cihazınız için bir yedekleme ilkesi eklemek için aşağıdaki adımları gerçekleştirin. İlkeyi ekledikten sonra bir zamanlama tanımlayabilirsiniz (bkz [Ekle veya bir zamanlamayı değiştirmek](#add-or-modify-a-schedule)).

[!INCLUDE [storsimple-8000-add-backup-policy-u2](../../includes/storsimple-8000-add-backup-policy-u2.md)]

## <a name="add-or-modify-a-schedule"></a>Bir zamanlamayı değiştirmek veya ekleme

StorSimple Cihazınızda mevcut bir yedekleme İlkesi bağlı olduğu bir zamanlamayı değiştirmek ya da ekleyin. Azure portalında eklemek veya bir zamanlamayı değiştirmek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-8000-add-modify-backup-schedule](../../includes/storsimple-8000-add-modify-backup-schedule-u2.md)]


## <a name="add-or-remove-a-volume"></a>Birim Ekle Kaldır

StorSimple Cihazınızda bir yedekleme ilkesi için atanan bir birimi kaldırın ya da ekleyin. Ekleme veya kaldırma bir birim için Azure portalında aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-8000-add-volume-backup-policy-u2](../../includes/storsimple-8000-add-remove-volume-backup-policy-u2.md)]


## <a name="delete-a-backup-policy"></a>Yedekleme ilkesini silme

StorSimple Cihazınızda bir yedekleme İlkesi silmek için Azure portalında aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-8000-delete-backup-policy](../../includes/storsimple-8000-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a>El ile yedekleyin

Tek bir birim için bir isteğe bağlı (el ile) yedekleme oluşturmak üzere Azure portalında aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-8000-create-manual-backup](../../includes/storsimple-8000-create-manual-backup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanarak](storsimple-8000-manager-service-administration.md).

