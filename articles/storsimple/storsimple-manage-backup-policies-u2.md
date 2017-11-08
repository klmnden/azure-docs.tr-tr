---
title: "StorSimple yedekleme ilkelerini yönetme | Microsoft Docs"
description: "El ile yedekleme, yedekleme zamanlamaları ve yedekleme bekletme oluşturmak ve yönetmek için StorSimple Yöneticisi hizmeti nasıl kullanabileceğiniz açıklanır."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 4a2db707-bbfc-425c-bfeb-bc5c97781873
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: v-sharos
ms.openlocfilehash: 8bf90b0ca5e488b676b50c37b24202002824c99b
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-backup-policies-update-2"></a>Yedekleme ilkeleri (güncelleştirme 2) yönetmek için StorSimple Yöneticisi hizmetini kullanma
> [!NOTE]
> StorSimple için Klasik portalı kullanım dışıdır. StorSimple cihaz yöneticileri yeni Azure portalına kullanımdan zamanlamaya göre otomatik olarak taşır. Bir e-posta ve bu taşıma için portal bir bildirim alırsınız. Bu belgede ayrıca yakında kullanımdan kaldırılacaktır. Bu makalede yeni Azure portalına için sürümünü görüntülemek için şu adrese gidin [yedekleme ilkeleri (güncelleştirme 2) yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-8000-manage-backup-policies-u2.md). Taşıma hakkında herhangi bir sorunuz için bkz: [SSS: Azure portalına taşıma](storsimple-8000-move-azure-portal-faq.md).

[!INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a>Genel Bakış
Bu öğretici StorSimple Yöneticisi hizmetini kullanma açıklanmaktadır **yedekleme ilkeleri** yedekleme işlemleri ve yedekleme bekletme için StorSimple birimlerinizi denetlemek için sayfa. Ayrıca, el ile yedekleme tamamlamak nasıl açıklanır.

Bir birim yedeklediğinizde, yerel bir anlık görüntüsü veya bir bulut anlık görüntüsü oluşturmayı seçebilirsiniz. Yerel olarak sabitlenmiş bir birim yedekleme yapıyorsanız, bir bulut anlık görüntüsü belirtilmesi önerilir. Çok sayıda karmaşası çok sahip bir veri kümesi ile birlikte yerel olarak sabitlenmiş bir birim yerel anlık görüntüleri alma hızla yerel alana çalıştırabilir durumda neden olur. Yerel anlık görüntüsünü seçerseniz, en son duruma geri, bunları bir gün için korumak için daha az günlük anlık görüntülerini almak ve bunları silmek öneririz.

Yerel olarak sabitlenmiş bir birim bir bulut anlık görüntüsünü, burada, yinelenenleri kaldırılmış sıkıştırılmış ve buluta, yalnızca değiştirilen verileri kopyalayın. 

## <a name="the-backup-policies-page"></a>Yedekleme ilkeleri sayfası
**Yedekleme ilkeleri** sayfası yedekleme ilkelerini yönetmek ve yerel zamanlamak ve bulut anlık görüntüleri olanak tanır. (Yedekleme ilkeleri yedekleme zamanlamaları ve yedekleme bekletme birimlerin bir koleksiyon için yapılandırmak için kullanılır.) Yedekleme ilkeleri aynı anda birden çok birim bir anlık görüntüsünü olanak tanır. Bu, bir yedekleme İlkesi tarafından oluşturulan yedekleri kilitlenme tutarlı kopyaları olacağı anlamına gelir. **Yedekleme ilkeleri** sayfasında yedekleme ilkelerini, türlerini, ilişkili birimler, yedeklerini korunur ve bu ilkeler etkinleştirme seçeneği listelenir.

**Yedekleme ilkeleri** sayfa Ayrıca, varolan yedekleme ilkeleri bir veya daha fazla aşağıdaki alanları tarafından filtre olanak sağlar:

* **İlke adı** – ilkeyle ilişkili ad. İlkeleri farklı türleri şunlardır:
  
  * Zamanlanmış ilkeleri, kullanıcı tarafından açıkça oluşturulur.
  * Bu birimi seçeneği için varsayılan yedekleme birim oluşturma sırasında etkinleştirildiğinde oluşturulan otomatik ilkeleri. Bu ilkeler olarak adlandırılır *VolumeName*_Default nerede *VolumeName* Klasik Azure portalındaki kullanıcı tarafından yapılandırılan StorSimple biriminin adına başvuruyor. 22:30 aygıt zamanında başlayan günlük bulut anlık görüntüleri otomatik ilkeleri sonuçlanır.
  * StorSimple anlık görüntü Yöneticisi'nde ilk olarak oluşturulan alınan ilkeleri. Bu ilkeleri alındığı StorSimple Snapshot Manager konak açıklayan bir etiket vardır.
* **Birimleri** – ilkeyle ilişkili birimler. Yedekleme oluşturulduğunda bir yedekleme ilkesiyle ilişkili tüm birimler birlikte gruplandırılır.
* **Son başarılı yedekleme** – Bu ilkeyle alındığı son başarılı yedekleme saati ve tarihi.
* **Sonraki yedekleme** – Bu ilke tarafından başlatılan bir sonraki zamanlanmış yedekleme saati ve tarihi.
* **Zamanlamalar** – yedekleme ilkeyle ilişkilendirilmiş zamanlamaları sayısı.

Bu sayfadan gerçekleştirebileceğiniz sık kullanılan işlemleri şunlardır:

* Yedekleme ilkesi ekleme 
* Ekleyin veya bir zamanlama değiştirin 
* Bir yedekleme ilkesi silme 
* El ile yedekleyin 
* Birden çok birimleri ve zamanlamaları özel bir yedekleme ilkesi oluşturma 

## <a name="add-a-backup-policy"></a>Yedekleme ilkesi ekleme
Otomatik olarak Yedeklemelerinizin zamanlamak için bir yedekleme ilkesi ekleyin. StorSimple cihazınız için bir yedekleme ilkesi eklemek için Azure Klasik portalında aşağıdaki adımları gerçekleştirin. İlkeyi ekledikten sonra bir zamanlama tanımlayabilirsiniz (bkz [Ekle ya da bir zamanlamayı değiştirmek](#add-or-modify-a-schedule)).

[!INCLUDE [storsimple-add-backup-policy-u2](../../includes/storsimple-add-backup-policy-u2.md)]

![Video var](./media/storsimple-manage-backup-policies-u2/Video_icon.png) **Video var**

Yerel oluşturma veya yedekleme İlkesi bulut gösteren bir video izlemek için tıklatın [burada](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).

## <a name="add-or-modify-a-schedule"></a>Ekleyin veya bir zamanlama değiştirin
Ekleyin veya mevcut bir yedekleme İlkesi, StorSimple Cihazınızda bağlı olduğu bir zamanlama değiştirin. Eklemek veya bir zamanlamayı değiştirmek için Azure Klasik portalında aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule-u2.md)]

## <a name="delete-a-backup-policy"></a>Bir yedekleme ilkesi silme
Bir yedekleme İlkesi, StorSimple Cihazınızda silmek için Azure Klasik portalında aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a>El ile yedekleyin
Tek bir birim için bir isteğe bağlı (el ile) yedekleme oluşturmak için Azure Klasik portalında aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a>Birden çok birimleri ve zamanlamaları özel bir yedekleme ilkesi oluşturma
Birden çok birimleri ve zamanlamaları sahip özel bir yedekleme ilkesi oluşturmak için Azure Klasik portalında aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy-u2.md)]

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

