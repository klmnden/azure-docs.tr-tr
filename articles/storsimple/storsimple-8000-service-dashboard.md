---
title: Kullanımı Özet StorSimple 8000 serisi cihaz | Microsoft Docs
description: StorSimple hizmet özeti dikey ve bunu StorSimple çözümünüzün durumunu izlemek için nasıl kullanılacağını açıklar.
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
ms.date: 03/27/2017
ms.author: alkohli
ms.openlocfilehash: c174f6ce0fb3d40af953be205a7bfcca60fbfeec
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60633206"
---
# <a name="use-the-service-summary-blade-for-storsimple-8000-series-device"></a>StorSimple 8000 serisi cihaz için hizmeti Özet dikey kullanın

## <a name="overview"></a>Genel Bakış

StorSimple cihaz Yöneticisi hizmeti Özet dikey penceresinde, bir sistem yöneticisinin dikkat etmeniz gereken aygıtları vurgulama StorSimple cihaz Yöneticisi hizmetine bağlı cihazların Özet görünümünü sağlar. Bu öğreticide hizmet özeti dikey sunar, Pano içeriğini ve işlevi açıklanmaktadır ve bu sayfadan gerçekleştirebileceğiniz görevler açıklanmaktadır.

![Hizmet özeti](./media/storsimple-8000-service-dashboard/service-summary1.png)


## <a name="management-commands"></a>Yönetim komutları

StorSimple hizmeti Özet dikey penceresinde, StorSimple cihaz Yöneticisi hizmetiniz ve bu hizmete kayıtlı bir StorSimple 8000 serisi cihazları yönetmeye yönelik seçenekler görürsünüz. Dikey pencerenin ve sol taraftaki üstteki yönetimi komutları bakın.

![Komut çubuğu](./media/storsimple-8000-service-dashboard/service-summary2.png)

Paylaşımları veya birimler eklemek gibi veya İzleyici çeşitli iş StorSimple cihaz üzerinde çalışan çeşitli işlemler gerçekleştirmek için bu seçenekleri kullanın.


## <a name="essentials"></a>Temel Bileşenler

Temel alan, kaynak grubu, konum ve StorSimple cihaz Yöneticisi oluşturulduğu abonelik gibi önemli özelliklerin bazılarını yakalar.

![Temel Bileşenler](./media/storsimple-8000-service-dashboard/service-summary3.png)

## <a name="storsimple-device-manager-service-summary"></a>StorSimple cihaz Yöneticisi hizmet özeti

* **Uyarılar** kutucuğu uyarı önem derecesine göre gruplandırılmış tüm cihazlardaki tüm etkin uyarıları anlık görüntüsünü sağlar.

    ![Uyarılar kutucuğu](./media/storsimple-8000-service-dashboard/service-summary4.png)

    Kutucuğa tıklandığında açılır **uyarılar** dikey penceresinde, uyarı hakkında ek ayrıntıları görüntülemek için bir bireysel uyarı tıklayabilirsiniz burada dahil önerilen eylemler. Uyarı, sorunu çözerseniz de temizleyebilirsiniz.

    ![Uyarıları kutucuğuna tıklayın.](./media/storsimple-8000-service-dashboard/service-summary8.png)

* **Kapasite** kutucuk görüntüler, kullanılabilir toplam depolama alanı göre tüm cihazlardaki tüm cihazlarda sağlanan ve kalan birincil depolama gösterir. **Sağlanan** hazır ve kullanılmak için ayrılan depolama alanı miktarının başvurduğu **kalan** tüm cihazlarda sağlanabilir kalan kapasiteyi ifade eder.

    ![Kapasite kutucuğu](./media/storsimple-8000-service-dashboard/service-summary6.png)

    **Kalan katmanlı** bulut dahil olmak üzere sağlanabilir kullanılabilir kapasiteyi kapasitesidir sırada **kalan yerel** için StorSimple 8000 bağlı diskler kalan kapasite serisi cihazlar.


* İçinde **kullanım** grafik, cihazlarınız için ilgili ölçümleri görebilirsiniz. Tüm cihazlarda kullanılan birincil depolama alanı ve son 7 gün içindeki, varsayılan süre cihazlar tarafından kullanılan bulut depolama alanına görüntüleyebilirsiniz. 

    ![Kullanım kutucuğu](./media/storsimple-8000-service-dashboard/service-summary7.png) 

    Farklı zaman ölçeği seçmek için kullanın **Düzenle** grafiğin sağ üst köşedeki seçeneği.

     ![Kullanım kutucuğa tıklayın](./media/storsimple-8000-service-dashboard/service-summary10.png)

     ![Grafik verilerini dışarı aktarma](./media/storsimple-8000-service-dashboard/service-summary11.png)

* **Cihazları** kutucuğu, StorSimple 8000 serisi cihaz, StorSimple cihaz cihaz durumlarına göre gruplandırılmış Yöneticisi'nde sayısının bir özeti sağlar. 

    ![Cihazları kutucuğu](./media/storsimple-8000-service-dashboard/service-summary5.png)

    Açmak için bu kutucuğa tıklayarak **cihazları** listesi dikey penceresinde ve sonra cihaza özgü cihaz özeti incelemek için tek bir cihaza tıklayın. Ayrıca, belirli bir cihaz özeti dikey penceresinden cihaza özel eylemler gerçekleştirebilirsiniz. Cihaz özeti dikey penceresi hakkında daha fazla bilgi için Git [Özet dikey penceresinde cihaz](storsimple-8000-device-dashboard.md).

    ![Cihazları kutucuğa tıklayın](./media/storsimple-8000-service-dashboard/service-summary9.png)

## <a name="view-the-activity-logs"></a>Etkinlik günlüklerini görüntüleme

StorSimple cihaz Yöneticisi içinde gerçekleştirilen çeşitli işlemler görüntülemek için tıklayın **etkinlik günlüklerini** StorSimple hizmeti Özet dikey penceresinin sol tarafındaki bağlantıyı. Sayfasına yönlendirileceksiniz **etkinlik günlüklerini** dikey penceresinde gerçekleştirilen son işlem özetini görebileceğiniz.

![Etkinlik Günlükleri](./media/storsimple-8000-service-dashboard/activity-logs1.png)
## <a name="next-steps"></a>Sonraki adımlar

* Kullanma hakkında daha fazla bilgi edinin [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

