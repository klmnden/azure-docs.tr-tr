---
title: "StorSimple 8000 serisi aygıt kullanma Özet | Microsoft Docs"
description: "StorSimple hizmeti Özet dikey tanımlar ve bunu StorSimple çözümünüzün sağlığını izlemek için nasıl kullanılacağını açıklar."
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
ms.date: 03/27/2017
ms.author: alkohli
ms.openlocfilehash: d987a4ae170f21532a886552cbe1eb5a0d25fc3f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-service-summary-blade-for-storsimple-8000-series-device"></a>StorSimple 8000 serisi cihaz için hizmeti Özet dikey kullanın

## <a name="overview"></a>Genel Bakış

StorSimple cihaz Yöneticisi hizmeti Özet dikey bir sistem yöneticisinin dikkat etmeniz gereken aygıtları vurgulama StorSimple cihaz Yöneticisi hizmetine bağlı olan tüm aygıtları Özet görünümünü sağlar. Bu öğretici hizmeti Özet dikey tanıtır, Pano içeriği ve işlevi açıklanmaktadır ve bu sayfadan gerçekleştirebileceğiniz görevleri açıklar.

![Hizmet özeti](./media/storsimple-8000-service-dashboard/service-summary1.png)


## <a name="management-commands"></a>Yönetim komutları

StorSimple hizmeti Özet dikey penceresinde, StorSimple cihaz Yöneticisi hizmetiniz ve bu hizmete kayıtlı StorSimple 8000 serisi cihazlar yönetmek için seçenekler bakın. Dikey ve sol tarafındaki üstte yönetimi komutları bakın.

![Komut çubuğu](./media/storsimple-8000-service-dashboard/service-summary2.png)

Paylaşımlar veya birimler veya StorSimple cihazlarda çalışan çeşitli işleri izleme gibi çeşitli işlemleri gerçekleştirmek için bu seçenekleri eklemek kullanın.


## <a name="essentials"></a>Temel Bileşenler

Temel alan kaynak grubu, konum ve StorSimple Aygıt Yöneticisi'ni oluşturulduğu abonelik gibi önemli özelliklerinden bazıları yakalar.

![Temel Bileşenler](./media/storsimple-8000-service-dashboard/service-summary3.png)

## <a name="storsimple-device-manager-service-summary"></a>StorSimple cihaz Yöneticisi hizmet özeti

* **Uyarıları** kutucuğu uyarı önem derecesine göre gruplandırılmış tüm aygıtlar arasında anlık görüntüsünü tüm etkin uyarıları sağlar.

    ![Uyarılar kutucuğu](./media/storsimple-8000-service-dashboard/service-summary4.png)

    Kutucuğa tıkladığınızda açılır **uyarıları** dikey penceresinde, uyarı hakkında ek ayrıntıları görüntülemek için bir bireysel uyarı tıklayabilirsiniz nerede herhangi dahil olmak üzere önerilen eylemler. Sorunu çözerseniz uyarı işaretini kaldırabilirsiniz.

    ![Uyarılar kutucuğu tıklayın](./media/storsimple-8000-service-dashboard/service-summary8.png)

* **Kapasite** döşeme görüntüler toplam depolama alanı kullanılabilir göre tüm cihazlar arasında cihazlara sağlanan ve kalan birincil depolama gösterir. **Sağlanan** hazır ve kullanım için ayrılan depolama alanı miktarını başvurduğu **kalan** cihazlara sağlanabilir kalan kapasite başvuruyor.

    ![Kapasite döşeme](./media/storsimple-8000-service-dashboard/service-summary6.png)

    **Kalan katmanlı** kapasitesi ise bulut dahil olmak üzere sağlanabilir kullanılabilir kapasite sırada **kalan yerel** olan StorSimple 8000 bağlı diskler kalan kapasitesi serisi cihazlar.


* İçinde **kullanım** grafik aygıtlarınız için ilgili ölçümleri görebilirsiniz. Tüm cihazlar arasında kullanılan birincil depolama ve son 7 gün içindeki, varsayılan süre cihazlar tarafından kullanılan bulut depolama görüntüleyebilirsiniz. 

    ![Kullanımı kutucuğu](./media/storsimple-8000-service-dashboard/service-summary7.png) 

    Farklı bir zaman ölçeği seçmek için kullanın **Düzenle** grafiğin sağ üst köşedeki seçeneği.

     ![Kullanım kutucuğa tıklayın](./media/storsimple-8000-service-dashboard/service-summary10.png)

     ![Grafik verileri dışarı aktarma](./media/storsimple-8000-service-dashboard/service-summary11.png)

* **Aygıtları** kutucuğu sayısı, StorSimple 8000 serisi cihazlar cihaz durumuna göre gruplandırılmış, StorSimple Aygıt Yöneticisi özetini sağlar. 

    ![Aygıtları döşeme](./media/storsimple-8000-service-dashboard/service-summary5.png)

    Açmak için bu kutucuğa tıklayın **aygıtları** listesinde dikey ve aygıta belirli cihaz özeti incelemek için tek bir cihaza tıklayın. Ayrıca, belirli bir aygıt Özet dikey penceresinden aygıta özgü eylemleri gerçekleştirebilirsiniz. Cihaz Özet dikey hakkında daha fazla bilgi için Git [aygıt Özet dikey](storsimple-8000-device-dashboard.md).

    ![Aygıtları kutucuğa tıklayın](./media/storsimple-8000-service-dashboard/service-summary9.png)

## <a name="view-the-activity-logs"></a>Etkinlik günlükleri görüntüleme

StorSimple cihaz yöneticinize içinde gerçekleştirilen çeşitli işlemleri görüntülemek için **etkinlik günlükleri** StorSimple hizmeti Özet dikey pencerenin sol tarafında bağlantı. Bu, olanak sürer **etkinlik günlükleri** dikey penceresinde, burada görebilirsiniz gerçekleştirilen son işlemlerinin bir Özet.

![Etkinlik Günlükleri](./media/storsimple-8000-service-dashboard/activity-logs1.png)
## <a name="next-steps"></a>Sonraki adımlar

* Nasıl yapılır hakkında daha fazla bilgi [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

