---
title: "StorSimple Yöneticisi cihaz Pano kullanın | Microsoft Docs"
description: "StorSimple Yöneticisi hizmet cihaz Pano ve storage ölçümleri ve bağlı başlatıcıları görüntüleyin ve seri numarasını ve IQN bulmak için kullanın açıklar."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 6c213969-a385-461f-b698-78ef5b8a79cc
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0d8035b9608ca3bac3d4822c7c755b81c96d481e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-device-dashboard-in-storsimple-manager-service"></a>Cihaz Pano StorSimple Yöneticisi hizmetini kullanma  

## <a name="overview"></a>Genel Bakış
StorSimple Yöneticisi cihaz Pano bilgi aksine, Microsoft Azure StorSimple çözümünüzde dahil tüm aygıtları hakkında bilgi verir hizmet panosunu belirli bir StorSimple cihazı için genel bir bakış sağlar.

![Cihaz Pano sayfası](./media/storsimple-device-dashboard/StorSimple_DeviceDashbaord1M.png)

Pano aşağıdaki bilgileri içerir:

* **Grafik alanı** – Pano üstündeki grafik alanında ilgili depolama ölçümleri görebilirsiniz. Bu grafik, toplam birincil depolama (aygıtınıza konaklar tarafından yazılan veri miktarı) ve bir süre boyunca cihazınız tarafından kullanılan toplam bulut depolama için ölçüleri görüntüleyebilirsiniz.
  
     Bu bağlamda *birincil depolama* toplam ana bilgisayar tarafından yazılan veri miktarını başvuruyor ve birim türüne göre ayrılabilir: *birincil katmanlı depolama* hem yerel olarak depolanan içeren veri ve veri katmanlı buluta; *birincil yerel olarak sabitlenmiş depolama* yerel olarak depolanan yalnızca verileri içerir. *Bulut depolama*, toplam bulutta depolanan veri miktarına, diğer yandan ölçüsüdür. Bu, katmanlı veri ve yedeklemeler içerir. Birincil depolama veri yinelenenleri kaldırılmış ve sıkıştırılmış önce kullanılan depolama alanı miktarını gösterir ancak bulutta depolanan veriler yinelenenleri kaldırılmış ve sıkıştırılmış, unutmayın. (Sıkıştırma oranı hakkında bir fikir edinmek için bu iki sayı karşılaştırabilirsiniz.) Hem birincil hem de için ve bulut depolama, gösterilen tutarlar, yapılandırdığınız izleme sıklığına bağlı. Bir hafta sıklığı seçerseniz, örneğin, ardından grafik verileri her gün önceki hafta içinde gösterilir.
  
     Grafiği aşağıdaki gibi yapılandırabilirsiniz:
  
  * Zaman içinde kullanılan bulut depolama alanı miktarını görmek için seçin **bulut depolama kullanılan** seçeneği. Ana bilgisayar tarafından yazılan toplam depolama görmek için seçin **birincil KATMANLI depolama kullanılan** ve **birincil yerel olarak SABİTLENMİŞ depolama kullanılan** seçenekleri. Çizimde, her iki seçenek seçilir; Bu nedenle, hem bulut hem de birincil depolama için depolama tutarlar grafik gösterir. Güncelleştirme 2 yüklemeden önce kullanılan tüm birincil depolama tarafından temsil edilen Not **birincil KATMANLI depolama kullanılan** satır.
  * Aşağı açılan menüden grafiğin sağ üst köşesinde 1 hafta, ay 1, 3 aylık veya 1 yıllık dönem belirtmek için kullanın. Üst düzey grafik günde yalnızca bir kez yenilendiğini ve bu nedenle önceki günün toplamları yansıtılacaktır unutmayın.
    
    Daha fazla bilgi için bkz: [StorSimple Cihazınızı izlemek için StorSimple Yöneticisi hizmetini kullanma](storsimple-monitor-device.md).
* **Kullanıma genel bakış** – içinde **kullanıma genel bakış** alanında kullanılan birincil depolama alanı miktarı, sağlanan depolama miktarını ve cihazınız için maksimum depolama kapasitesi görebilirsiniz. Bu maksimum kullanılabilir depolama alanı kullanım sayılara karşılaştırarak, ek depolama alanı elde etmeniz gerekirse bir bakışta görebilirsiniz. Bu genel bakışta 15 dakikada bir güncelleştirilir ve güncelleştirme sıklığı fark nedeniyle olandan farklı numaraları günlük güncelleştirilen grafik alanında yukarıdaki gösterilen gösterebilir unutmayın. Daha fazla bilgi için bkz: [StorSimple Cihazınızı izlemek için StorSimple Yöneticisi hizmetini kullanma](storsimple-monitor-device.md).
* **Uyarıları** – **uyarıları** alanı uyarıları, cihazınız için genel bir bakış içerir. Uyarıları önem derecesine göre gruplandırılır ve bir sayısı her önem düzeyinin uyarılara sayısının sağlanır. Uyarı önem derecesini tıklatılması yalnızca bu önem düzeyi bu cihaz için uyarıları göstermek için Uyarılar sekmesini kapsamlı bir görünümünü açar.
* **İşlerini** – **işleri** alanı son iş etkinliğinin sonucunu gösterir. Bu, beklendiği gibi işletim sistemi veya düzeltici işlem yapmanıza gerek size bildirmek güvence altına almak. Son tamamlanan işler hakkında daha fazla bilgi görmek için tıklatın **işleri başarılı son 24 saat içindeki**.
* **Hızlı Bakış** Pano sağındaki alanı cihaz modeli, seri numarası, durumu, açıklama ve birimlerin sayısı gibi yararlı bilgileri sağlar.

Ayrıca, yük devretme yapılandırma ve cihaz panosundan bağlı başlatıcıları görüntüleyin.

Bu sayfada gerçekleştirilen ortak görevleri şunlardır:

* Bağlı başlatıcıları görüntüleyin
* Cihaz seri numarasını Bul
* Cihaz hedef IQN Bul

## <a name="view-connected-initiators"></a>Bağlı başlatıcıları görüntüleyin
Aygıtınıza tıklayarak bağlı iSCSI başlatıcıları görüntüleyebilirsiniz **görünüm bağlı başlatıcıları** sağlanan bağlantı **Hızlı Bakış** aygıt panosunun alanı. Bu sayfa tablo aygıtınıza başarıyla bağlandıysanız başlatıcıları listesini sağlar. Her bir başlatıcısını görebilirsiniz:

* İSCSI tam adını (IQN) bağlı başlatıcı.
* Bu bağlı başlatıcı veren erişim denetimi kaydı (ACR) adı.
* Bağlı başlatıcı IP adresi.
* Depolama aygıtınızda Başlatıcı bağlandığı ağ arabirimleri. Bu veri 0 ' veri 5 aralığında değişebilir.
* Bağlı başlatıcı ACR yapılandırmasına göre erişim izni tüm birimler.

Bu listedeki beklenmeyen başlatıcıları bakın veya beklenen olanları görüyor musunuz ACR yapılandırmanızı gözden geçirin. En fazla 512 başlatıcıları aygıtınıza bağlanabilir.

## <a name="find-the-device-serial-number"></a>Cihaz seri numarasını Bul
Cihazda Microsoft çok yollu g/ç (MPIO) yapılandırdığınızda cihaz seri numarasını gerekebilir. Cihaz seri numarasını bulmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-find-the-device-serial-number"></a>Cihaz seri numarasını bulmak için
1. Gidin **aygıtları** > **Pano**.
2. Pano sağ bölmede bulun **Hızlı Bakış** alanı.
3. Aşağı kaydırın ve seri numarasını bulun.

## <a name="find-the-device-target-iqn"></a>Cihaz hedef IQN Bul
StorSimple Cihazınızda karşılıklı kimlik doğrulama protokolü (CHAP) yapılandırdığınızda aygıt hedef IQN gerekebilir. Cihaz hedef IQN bulmak için aşağıdaki adımları gerçekleştirin.

### <a name="to-find-the-device-target-iqn"></a>Cihaz hedef IQN bulmak için
1. Gidin **aygıtları** > **Pano**.
2. Pano sağ bölmede bulun **Hızlı Bakış** alanı.
3. Aşağı kaydırın ve IQN hedef bulun.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [StorSimple Yöneticisi hizmet panosunu](storsimple-service-dashboard.md).
* Daha fazla bilgi edinmek [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

