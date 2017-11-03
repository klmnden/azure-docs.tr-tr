---
title: "StorSimple 8000 serisi aygıt kullanma Özet | Microsoft Docs"
description: "StorSimple cihaz Yöneticisi Hizmeti aygıt Özet ve storage ölçümleri ve bağlı başlatıcıları görüntüleyin ve seri numarasını ve IQN bulmak için kullanın açıklar."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: 784d3ce9d8f926b00ac1c6fbf48a05c0b04f900a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-device-summary-in-storsimple-device-manager-service"></a>StorSimple Aygıt Yöneticisi'ni hizmetinde Özet aygıtı kullanma

## <a name="overview"></a>Genel Bakış
StorSimple cihaz Özet dikey bilgi Microsoft Azure StorSimple çözümünüzde dahil tüm cihazlar hakkında bilgi verir hizmeti Özet dikey aksine belirli bir StorSimple cihazı için genel bir bakış sağlar.

Cihaz Özet dikey pencere, sistem yöneticisinin dikkat etmeniz gereken bu cihaz sorunları vurgulayan ile belirli bir StorSimple Aygıt Yöneticisi, kayıtlı bir StorSimple 8000 serisi aygıt Özet görünümünü sağlar. Bu öğretici aygıt Özet dikey tanıtır, içerik ve işlevi açıklanmaktadır ve bu dikey pencereden gerçekleştirebileceğiniz görevleri açıklar.

Cihaz Özet dikey penceresinde aşağıdaki bilgileri görüntüler:

![Cihaz Özet dikey penceresi](./media/storsimple-8000-device-dashboard/device-summary1.png)

## <a name="management-command-bar"></a>Yönetim Komut çubuğu

StorSimple cihaz dikey penceresinde, StorSimple Cihazınızı yönetmek için seçenekler bakın. Dikey ve sol tarafındaki üstte yönetimi komutları bakın. Paylaşımlar veya birimler eklemek veya güncelleştirmek veya Cihazınızı başarısız için bu seçenekleri kullanın.

![Yönetim Komut çubuğu](./media/storsimple-8000-device-dashboard/device-summary2.png)

## <a name="essentials"></a>Temel Bileşenler

Temel alan bazı gibi önemli özellikleri, durumu, model, hedef IQN ve yazılım sürümü yakalar. 

![Cihaz temelleri](./media/storsimple-8000-device-dashboard/device-summary3.png)

## <a name="monitoring"></a>İzleme

* **Uyarıları** kutucuğu uyarı önem derecesine göre gruplandırılmış cihazınız için bir anlık görüntüsünü tüm etkin uyarıları sağlar.

    ![Uyarı döşeme](./media/storsimple-8000-device-dashboard/device-summary4.png)

    Açmak için kutucuğa tıklayın **uyarıları** dikey ve ardından herhangi dahil olmak üzere, bu uyarı hakkında ek ayrıntıları görüntülemek için bir bireysel uyarı önerilen eylemler. Sorunu çözerseniz uyarı işaretini kaldırabilirsiniz.

    ![Uyarı kutucuğa tıklayın](./media/storsimple-8000-device-dashboard/device-summary10.png)

* **Durumunu** kutucuğu Aygıt durumu da dahil olmak üzere bir aygıt için donanım bileşen durumu fikir sağlar. Cihaz durumu, çevrimdışı, çevrimiçi, devre dışı bırakılmış veya ayarlamak hazır olması olabilir.

    ![Durum ve sistem durumu kutucuğu](./media/storsimple-8000-device-dashboard/device-summary5.png)

* **Birimleri** kutucuğu duruma göre gruplanmış Cihazınızı birimlerin sayısı özetini sağlar.

    ![Birimler kutucuğunu](./media/storsimple-8000-device-dashboard/device-summary6.png)

    Açmak için kutucuğa tıklayın **birimleri** listesinde dikey ve onun özelliklerini görüntülemek veya değiştirmek için tek bir birimde'ye tıklayın.
    
    ![Birimler kutucuğunu tıklatın](./media/storsimple-8000-device-dashboard/device-summary9.png)
    
    Daha fazla bilgi için bkz: nasıl yapılır [birimleri yönetme](storsimple-8000-manage-volumes-u2.md).

* İçinde **kullanım** grafik, Cihazınızı kullanılan birincil depolama ve son 7 gün içindeki, varsayılan süre tüketilen bulut depolama görüntüleyebilirsiniz.

     ![Kullanımı kutucuğu](./media/storsimple-8000-device-dashboard/device-summary7.png)
    
     Farklı bir zaman ölçeği seçmek için kullanın **Düzenle** grafiğin sağ üst köşedeki seçeneği.

     ![Kullanım grafiği Düzenle](./media/storsimple-8000-device-dashboard/device-summary12.png)

     Bu grafik, toplam birincil depolama (aygıtınıza konaklar tarafından yazılan veri miktarı) ve bir süre boyunca cihazınız tarafından kullanılan toplam bulut depolama için ölçüleri görüntüleyebilirsiniz.
  
     Bu bağlamda *birincil depolama* toplam ana bilgisayar tarafından yazılan veri miktarını başvuruyor ve birim türüne göre ayrılabilir: *birincil katmanlı depolama* hem yerel olarak depolanan içeren veri ve veri katmanlı buluta. *Birincil yerel olarak sabitlenmiş depolama* yerel olarak depolanan yalnızca verileri içerir. *Bulut depolama*, toplam bulutta depolanan veri miktarına, diğer yandan ölçüsüdür. Bu depolama katmanlı veri ve yedeklemeler içerir. Bulutta depolanan veriler yinelenenleri kaldırılmış ve birincil depolama veri yinelenenleri kaldırılmış ve sıkıştırılmış önce kullanılan depolama alanı miktarını gösterir ancak sıkıştırılmış. (Sıkıştırma oranı hakkında bir fikir edinmek için bu iki sayı karşılaştırabilirsiniz.) Hem birincil hem de için ve bulut depolama, gösterilen tutarlar, yapılandırdığınız izleme sıklığına bağlı. Bir hafta sıklığı seçerseniz, örneğin, daha sonra grafik veri önceki hafta içinde her gün için gösterir.

     Zaman içinde kullanılan bulut depolama alanı miktarını görmek için seçin **bulut depolama kullanılan** seçeneği. Ana bilgisayar tarafından yazılan toplam depolama görmek için seçin **birincil KATMANLI depolama kullanılan** ve **birincil yerel olarak SABİTLENMİŞ depolama kullanılan** seçenekleri. 
     Daha fazla bilgi için bkz: [StorSimple Cihazınızı izlemek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-monitor-device.md).


* **Kapasite** kutucuğu aynı için toplam depolama alanı kullanılabilir göre aygıt üzerinden sağlanan ve kalan birincil depolama görüntüler. **Sağlanan** hazır ve kullanım için ayrılan depolama alanı miktarını başvurduğu **kalan** bu aygıt arasında sağlanabilir kalan kapasite başvuruyor. 

    ![Kullanımı kutucuğu](./media/storsimple-8000-device-dashboard/device-summary8.png)

    Kapasite katmanlı ve yerel olarak sabitlenmiş birimleri arasında nasıl sağlandığında görüntülemek için bu kutucuğa tıklayın. **Kalan katmanlı** kapasitesi ise bulut dahil olmak üzere sağlanabilir kullanılabilir kapasite sırada **kalan yerel** olduğundan bu cihaza bağlı diskler kalan kapasitesi.

    ![Kullanım grafiği tıklatın](./media/storsimple-8000-device-dashboard/device-summary13.png)


## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [StorSimple hizmeti Özet dikey](storsimple-8000-service-dashboard.md).
* Daha fazla bilgi edinmek [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanarak](storsimple-8000-manager-service-administration.md).

