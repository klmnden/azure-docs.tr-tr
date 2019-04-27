---
title: Kullanımı Özet StorSimple 8000 serisi cihaz | Microsoft Docs
description: StorSimple cihaz Yöneticisi hizmeti cihaz özetini ve bu depolama ölçümlerini ve bağlı başlatıcıları görüntüleyin ve seri numarasını ve IQN bulmak için nasıl kullanılacağını açıklar.
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
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: 1d88af2c0739c30b2562bad7660015b890e8159c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60578326"
---
# <a name="use-the-device-summary-in-storsimple-device-manager-service"></a>Cihaz StorSimple cihaz Yöneticisi hizmet özetini kullanma

## <a name="overview"></a>Genel Bakış
StorSimple cihaz Özet dikey penceresinde, bilgi, Microsoft Azure StorSimple çözümde yer alan tüm cihazlar hakkında bilgi verir hizmeti Özet dikey aksine belirli bir StorSimple cihazı için genel bir bakış sağlar.

Cihaz Özet dikey penceresinde, bir sistem yöneticisinin dikkat edilmesi gereken cihaz devretmeyle vurgulama ile belirli bir StorSimple cihaz Yöneticisi, kayıtlı bir StorSimple 8000 serisi cihaz Özet görünümünü sağlar. Bu öğretici, cihaz Özet dikey penceresinde sunar, içerik ve işlevi açıklanmaktadır ve bu dikey pencereden gerçekleştirebileceğiniz görevler açıklanmaktadır.

Cihaz Özet dikey penceresinde aşağıdaki bilgileri görüntüler:

![Cihaz özeti dikey penceresi](./media/storsimple-8000-device-dashboard/device-summary1.png)

## <a name="management-command-bar"></a>Yönetim Komut çubuğu

StorSimple cihaz dikey penceresinde, StorSimple Cihazınızı yönetmek için seçeneklere bakın. Dikey pencerenin ve sol taraftaki üstteki yönetimi komutları bakın. Paylaşımları veya birimler eklemek, güncelleştirmek veya Cihazınızda yük devretme gerçekleştirme için bu seçenekleri kullanın.

![Yönetim Komut çubuğu](./media/storsimple-8000-device-dashboard/device-summary2.png)

## <a name="essentials"></a>Temel Bileşenler

Temel alan gibi önemli özelliklerini, durumunu, model, hedef IQN ve yazılım sürümü bazıları yakalar. 

![Cihaz temelleri](./media/storsimple-8000-device-dashboard/device-summary3.png)

## <a name="monitoring"></a>İzleme

* **Uyarılar** kutucuğu uyarı önem derecesine göre gruplandırılmış cihazınız için tüm etkin uyarıları anlık görüntüsünü sağlar.

    ![Uyarı kutucuğu](./media/storsimple-8000-device-dashboard/device-summary4.png)

    Açmak için kutucuğa tıklayın **uyarılar** dikey penceresinde ve ardından Eylemler dahil, bu uyarı hakkında ek ayrıntıları görüntülemek için bir bireysel uyarı önerilir. Uyarı, sorunu çözerseniz de temizleyebilirsiniz.

    ![Uyarı kutucuğa tıklayın](./media/storsimple-8000-device-dashboard/device-summary10.png)

* **Durum ve sistem durumu** kutucuğu, bir cihaz için cihaz durumu dahil olmak üzere donanım bileşeni sistem durumu hakkında Öngörüler sağlar. Cihaz durumu, çevrimdışı, çevrimiçi, devre dışı bırakılmış veya kuruluma hazır olabilir.

    ![Durum ve sistem durumu kutucuğu](./media/storsimple-8000-device-dashboard/device-summary5.png)

* **Birimleri** kutucuğu durumlarına göre gruplandırılmış cihazınızdaki birim sayısının bir özeti sağlar.

    ![Birimler kutucuğunu](./media/storsimple-8000-device-dashboard/device-summary6.png)

    Açmak için kutucuğa tıklayın **birimleri** listesi dikey penceresinde ve onun özelliklerini görüntülemek veya değiştirmek için tek bir birimin'ye tıklayın.
    
    ![Birimleri kutucuğa tıklayın](./media/storsimple-8000-device-dashboard/device-summary9.png)
    
    Daha fazla bilgi için bkz. nasıl [birimleri yönetme](storsimple-8000-manage-volumes-u2.md).

* İçinde **kullanım** grafik, Cihazınızı kullanılan birincil depolama alanı ve bulut depolama son 7 gün, varsayılan süre miktarı görüntüleyebilirsiniz.

     ![Kullanım kutucuğu](./media/storsimple-8000-device-dashboard/device-summary7.png)
    
     Farklı zaman ölçeği seçmek için kullanın **Düzenle** grafiğin sağ üst köşedeki seçeneği.

     ![Kullanım grafiğini Düzenle](./media/storsimple-8000-device-dashboard/device-summary12.png)

     Bu grafikte, toplam birincil depolama alanı (cihazınıza konaklar tarafından yazılan veri miktarı) ve bir süre cihazınız tarafından tüketilen toplam bulut depolama için ölçümleri görüntüleyebilir.
  
     Bu bağlamda *birincil depolama* toplam ana bilgisayar tarafından yazılan veri miktarı anlamına gelir ve birim türü tarafından ayrılabilir: *birincil katmanlı depolama* içerir hem yerel olarak depolanan verileri ve veri katmanlı için bulut. *Birincil yerel olarak sabitlenmiş depolama* yerel olarak depolanan yalnızca verileri içerir. *Bulut depolama*, diğer taraftan, bulutta depolanan verilerin toplam miktarı bir ölçümüdür. Bu depolama, katmanlı verileri ve yedeklemeler içerir. Bulutta depolanan veriler yinelenenleri kaldırılmış ve sıkıştırılmış ancak birincil depolama verileri kaldırılmış ve sıkıştırılmış önce kullanılan depolama miktarını gösterir. (Sıkıştırma oranı hakkında bir fikir edinmek için bu iki sayı karşılaştırabilirsiniz.) Hem birincil hem de için ve bulut depolama, gösterilen miktarları yapılandırdığınız izleme sıklığına bağlıdır. Bir hafta sıklığı seçin, örneğin, ardından grafik veri önceki haftanın her günü için gösterir.

     Zaman içinde kullanılan bulut depolama miktarını görmek için seçin **bulut depolama kullanılan** seçeneği. Ana bilgisayar tarafından yazılan toplam depolama alanı görmek için seçin **birincil KATMANLI depolama kullanılan** ve **birincil yerel olarak SABİTLENMİŞ depolama kullanılan** seçenekleri. 
     Daha fazla bilgi için [StorSimple Cihazınızı izlemek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-monitor-device.md).


* **Kapasite** kutucuk göre kullanılabilir toplam depolama alanı için aynı cihaz üzerinden sağlanan ve kalan birincil depolama alanı görüntüler. **Sağlanan** hazır ve kullanılmak için ayrılan depolama alanı miktarının başvurduğu **kalan** bu cihaz sağlanabilir kalan kapasiteyi ifade eder. 

    ![Kullanım kutucuğu](./media/storsimple-8000-device-dashboard/device-summary8.png)

    Kapasite katmanlı ve yerel olarak sabitlenmiş birimler arasında nasıl sağlandığında görüntülemek için bu kutucuğa tıklayın. **Kalan katmanlı** bulut dahil olmak üzere sağlanabilir kullanılabilir kapasiteyi kapasitesidir sırada **kalan yerel** olup bu cihaza bağlı diskler kalan kapasitesi.

    ![Kullanım grafiğini tıklatın](./media/storsimple-8000-device-dashboard/device-summary13.png)


## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [StorSimple hizmeti Özet dikey](storsimple-8000-service-dashboard.md).
* Daha fazla bilgi edinin [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanarak](storsimple-8000-manager-service-administration.md).

