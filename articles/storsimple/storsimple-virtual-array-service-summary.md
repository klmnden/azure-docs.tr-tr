---
title: StorSimple sanal dizinin hizmeti Özet dikey | Microsoft Docs
description: StorSimple cihaz yöneticisinin hizmeti Özet dikey açıklar ve StorSimple sanal dizinizi sağlığını izlemek için kullanımı açıklanmaktadır.
services: storsimple
documentationcenter: ''
author: manuaery
manager: syadav
editor: ''
ms.assetid: 8a2b9a84-b0e6-48b9-b366-d16f004241a5
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: 284e404c44505a98d9e0ed5abe87cd945415af56
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23875742"
---
# <a name="use-the-service-summary-blade-for-storsimple-device-manager-connected-to-storsimple-virtual-array"></a>Kullanım hizmeti Özet dikey için StorSimple Aygıt Yöneticisi'ni StorSimple sanal diziye bağlı
## <a name="overview"></a>Genel Bakış
Hizmet özeti dikey pencere için StorSimple Aygıt Yöneticisi'ni StorSimple sanal (olarak da bilinen StorSimple sanal cihazlar veya sanal cihazlar şirket) hizmetinize bağlanan bir sistem yöneticisinin dikkat etmeniz gereken olanlar vurgulama diziler Özet görünümünü sağlar. Bu öğretici hizmeti Özet dikey tanıtır, içerik ve işlevi açıklanmaktadır ve bu dikey pencereden gerçekleştirebileceğiniz görevleri açıklar.

![Hizmet panosu](./media/storsimple-virtual-array-service-summary/service-blade.png)

## <a name="management-commands-and-essentials"></a>Yönetimi komutları ve temelleri
StorSimple Özet dikey penceresinde, bu hizmete kayıtlı sanal diziler yanı sıra, StorSimple cihaz Yöneticisi hizmetiniz yönetmek için seçenekler bakın. Dikey ve sol tarafındaki üstte yönetimi komutları bakın.

Paylaşımlar veya birimler veya sanal diziler üzerinde çalışan çeşitli işleri izleme gibi çeşitli işlemleri gerçekleştirmek için bu seçenekleri eklemek kullanın.

Temel alan kaynak grubu, konum ve StorSimple Aygıt Yöneticisi'ni oluşturulduğu abonelik gibi önemli özelliklerinden bazıları yakalar.

## <a name="storsimple-device-manager-service-summary"></a>StorSimple cihaz Yöneticisi hizmet özeti
* **Uyarıları** kutucuğu uyarı önem derecesine göre gruplandırılmış tüm sanal cihazlar arasında anlık görüntüsünü tüm etkin uyarıları sağlar. Kutucuğa tıkladığınızda açılır **uyarıları** dikey penceresinde, uyarı hakkında ek ayrıntıları görüntülemek için bir bireysel uyarı tıklayabilirsiniz nerede herhangi dahil olmak üzere önerilen eylemler. Sorunu çözerseniz uyarı işaretini kaldırabilirsiniz.
* **Kapasite** döşeme görüntüler göre toplam depolama alanı kullanılabilir tüm sanal cihazlar arasında sanal cihazlara sağlanan ve kalan birincil depolama gösterir. **Sağlanan** hazır ve kullanım için ayrılan depolama alanı miktarını başvurduğu **kalan** sanal cihazlara sağlanabilir kalan kapasite başvuruyor. **Kalan katmanlı** kapasitesi ise bulut dahil olmak üzere sağlanabilir kullanılabilir kapasite sırada **kalan yerel** olan sanal diziler bağlı diskler kalan kapasitesi.
* İçinde **kullanım** grafik, sanal cihazlar için ilgili ölçümleri görebilirsiniz. Son 7 gün içindeki, varsayılan süre sanal cihazlar tarafından kullanılan bulut depolama yanı sıra, tüm sanal cihazlar arasında kullanılan birincil depolama görüntüleyebilirsiniz. Kullanım **Düzenle** farklı zaman ölçeği seçmek için sağ üst köşedeki grafik seçeneği.
* **Aygıtları** kutucuğu, StorSimple cihaz cihaz durumuna göre gruplandırılmış Yöneticisi'nde sanal dizi numarası özetini sağlar. Açmak için bu kutucuğa tıklayın **aygıtları** listesinde dikey ve aygıta belirli cihaz özeti incelemek için tek bir cihaza tıklayın. Belirli bir aygıt Özet dikey penceresinden aygıt belirli eylemleri de gerçekleştirebilirsiniz. Cihaz Özet dikey hakkında daha fazla bilgi için Git [aygıt Özet dikey](storsimple-virtual-array-device-summary.md).

## <a name="view-the-activity-logs"></a>Etkinlik günlükleri görüntüleme
StorSimple cihaz yöneticinize içinde gerçekleştirilen çeşitli işlemleri görüntülemek için **etkinlik günlükleri** StorSimple hizmeti Özet dikey pencerenin sol tarafında bağlantı. Bu, olanak sürer **etkinlik günlükleri** dikey penceresinde, burada görebilirsiniz gerçekleştirilen son işlemlerinin bir Özet.

![Etkinlik Günlükleri](./media/storsimple-virtual-array-service-summary/activity-log.png)

## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [StorSimple sanal dizinizi yönetmek için yerel web kullanıcı arabirimini kullanın](storsimple-ova-web-ui-admin.md).

