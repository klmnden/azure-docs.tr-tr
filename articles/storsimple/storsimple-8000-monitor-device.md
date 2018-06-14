---
title: StorSimple 8000 serisi Cihazınızı izleme | Microsoft Docs
description: StorSimple cihaz Yöneticisi hizmeti kullanımı, g/ç performans ve kapasite kullanımı izlemek için nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 10/17/2017
ms.author: alkohli
ms.openlocfilehash: 679c1fc8775ad4481bc99c9aea79fe16e9bcac8f
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2017
ms.locfileid: "23933147"
---
# <a name="use-the-storsimple-device-manager-service-to-monitor-your-storsimple-device"></a>StorSimple Cihazınızı izlemek için StorSimple cihaz Yöneticisi hizmetini kullanma

## <a name="overview"></a>Genel Bakış
StorSimple cihaz Yöneticisi hizmeti, StorSimple çözümünüzün içinde belirli aygıtları izlemek için kullanabilirsiniz. G/ç performansı, kapasite kullanımı, ağ verimliliğini ve cihaz performans ölçümleri göre özel grafikler oluşturma ve bu panoya Sabitle. Daha fazla bilgi için Git [portal panonuzu özelleştirebilir](../azure-portal/azure-portal-dashboards.md).

Azure portalında belirli bir aygıt için izleme bilgilerini görüntülemek için StorSimple cihaz Yöneticisi hizmeti seçin. Aygıtların listeden Cihazınızı seçin ve ardından gidin **İzleyici**. Ardından gördüğünüz **kapasite**, **kullanım**, ve **performans** grafikler seçili cihaz için.

## <a name="capacity"></a>Kapasite
**Kapasite** sağlanan alan ve cihazda kalan alan izler. Kalan kapasite, ardından yerel olarak sabitlenmiş veya katmanlı olarak görüntülenir.

Daha fazla katmanlı ve yerel olarak sabitlenmiş birimleri tarafından sağlanan ve kalan kapasite ayrıntılarıyla. Her birim için sağlanan kapasite ve cihazda kalan kapasite görüntülenir.

![G/ç kapasitesi](./media/storsimple-8000-monitor-device/device-capacity.png)



## <a name="usage"></a>Kullanım
**Kullanım** birimler, birim kapsayıcıları veya aygıt tarafından kullanılan veri depolama alanı miktarı ilgili ölçümleri izler. Birincil depolama, bulut depolama veya aygıt depolama kapasitesi kullanımı temelinde raporlar oluşturabilir. Kapasite kullanımı, belirli bir birimin, özel birim kapsayıcısı veya tüm birim kapsayıcıları ölçülebilir.
Varsayılan olarak, son 24 saat için kullanım bildirilir. Üzerinde kullanım seçerek bildirilen süresini değiştirmek için grafik düzenleyebilirsiniz:
* Son 24 saat
* Son 7 gün
* Son 30 gün
* Son 90 gün
* Geçen yıl

İki anahtar metrices, büyüme ve aralığı için kullanım grafiklerini raporlanır. Aralığı en büyük değer ve en düşük değerleri seçili süresi (fo örneği, son 7 gün) içinde bildirilen kullanım ifade eder.

Büyüme kullanım artış son günü ilk günden itibaren seçili süresinde ifade eder. 

Ayrıca büyüme ve aralığı tarafından aşağıdaki denklemini gösterilebilir:

```
Range = {Usage(minimum), Usage(maximum)}

Growth = Usage(Last day) - Usage(first day)

Growth (%) = [{Usage(last day) - Usage(first day)} X 100]/Usage(first day)
```

Birincil Bulut ve kullanılan yerel depolama gibi tanımlanabilen:

### <a name="primary-storage-usage"></a>Birincil depolama kullanımı
Bu grafik veri yinelenenleri kaldırılmış ve sıkıştırılmış önce StorSimple birimlerini yazılan veri miktarını gösterir. Birim kapsayıcısı içinde veya tek bir birim için tüm birimleri tarafından kullanılan birincil depolama görüntüleyebilirsiniz. Kullanılan birincil depolama daha fazla kullanılan birincil katmanlı depolama tarafından ayrılmıştır ve kullanılan depolama alanı birincil yerel olarak sabitlenmiş.

Aşağıdaki grafikleri önce StorSimple cihazını ve bulut anlık görüntü alındıktan sonra için kullanılan birincil depolama gösterir. Bir bulut anlık görüntü yalnızca birim verilerini olarak birincil depolama değiştirmemelisiniz. Gördüğünüz gibi bir bulut anlık sonucu olarak kullanılan birincil katmanlı veya yerel olarak sabitlenmiş depolama fark grafik gösterir. Bulut anlık görüntüsü yaklaşık 11:50 pm cihazdaki başlangıcı.

![Bulut anlık görüntüsü sonra birincil kapasite kullanımı](./media/storsimple-8000-monitor-device/device-primary-storage-after-cloudsnapshot.png)

StorSimple Cihazınızı bağlı ana bilgisayardaki g/ç Şimdi Çalıştır, birincil katmanlı depolama artış görürsünüz ya da kullanılan depolama alanı bağlı olarak birincil yerel olarak sabitlenmiş (katmanlı veya yerel olarak sabitlenmiş) hangi birimlerin veri yazma. Burada, StorSimple cihazını birincil depolama kullanımı grafiklerde bulunmaktadır. Bu aygıtta hizmet veren başlatılan StorSimple konak cihazda katmanlı birim üzerinde yaklaşık 2: 30'e yazar. Ana bilgisayarda çalışan GÇ karşılık gelen Yazma Bayt/sn cinsinden en yüksek görebilirsiniz.

![Birimler üzerinde çalışan GÇ katmanlı performansı](./media/storsimple-8000-monitor-device/device-io-from-initiator.png)

Kullanılan birincil katmanlı depolama bakarsanız, birincil yerel olarak sabitlenmiş ise, cihazda yerel olarak sabitlenmiş birimleri için sunulan hiçbir yazma gibi kullanım değişmeden kalır geçti.

![G/ç çalıştırırken katmanlı birimlerin birincil kapasite kullanımı](./media/storsimple-8000-monitor-device/device-primary-storage-io-from-initiator.png)

Çalıştırıyorsanız, güncelleştirme 3 veya sonraki, birincil depolama kapasitesi kullanımını tek bir birim, tüm birimleri, tüm katmanlı birimlerin ve tüm yerel olarak sabitlenmiş birimleri tarafından aşağıda gösterildiği gibi bölünebilir. Tüm yerel olarak sabitlenmiş birimleri tarafından kesiliyor hızlı bir şekilde ne kadar yerel katmanının kullanıldığını onaylaması izin verir.

![Tüm katmanlı birimlerin birincil kapasite kullanımı](./media/storsimple-8000-monitor-device/monitor-usage3.png)

![Tüm yerel olarak sabitlenmiş birimler için birincil kapasite kullanımı](./media/storsimple-8000-monitor-device/monitor-usage4.png)

Daha fazla listesinde birimlerin her birinde tıklayın ve karşılık gelen kullanımına bakın.

![Tüm yerel olarak sabitlenmiş birimler için birincil kapasite kullanımı](./media/storsimple-8000-monitor-device/device-primary-storage-usage-by-volume.png)

### <a name="cloud-storage-usage"></a>Bulut depolama kullanımı
Bu grafikler kullanılan bulut depolama alanı miktarını gösterir. Bu veri yinelenenleri kaldırılmış ve sıkıştırılmış. Bu miktar, tüm birincil biriminde yansıtılan değil ve gerekli veya eski bekletme amacıyla tutulur verileri içerebilen bulut anlık görüntüleri içerir. Birincil karşılaştırın ve sayı tam olmaz ancak veri azaltma oranı hakkında bir fikir edinmek için depolama tüketim rakamlarını bulut.

Bir bulut anlık görüntüsü durumdayken aşağıdaki grafikleri StorSimple cihazını bulut depolama kullanımını gösterir.

* Bulut anlık görüntüsü yaklaşık 11:50:00 cihazdaki başlangıcı ve anlık görüntü bulut önce olduğunu kullanılan bulut depolama görebilirsiniz. 
* Bir kez tamamlandı bulut anlık görüntüsü bulut depolama alanı kullanımı 0,89 GB görüntüsü. 
* Bulut anlık görüntüsü devam ederken bulunmaktadır ayrıca karşılık gelen yoğun GÇ bulut aygıttan.

    ![Bulut anlık görüntüsü önce bulut depolama alanı kullanımı](./media/storsimple-8000-monitor-device/device-cloud-storage-before-cloudsnapshot.png)

    ![Bulut anlık görüntüsü sonra bulut depolama alanı kullanımı](./media/storsimple-8000-monitor-device/device-cloud-storage-after-cloudsnapshot.png)

    ![Bir bulut anlık görüntüsü sırasında bulut aygıttan g/ç](./media/storsimple-8000-monitor-device/device-io-to-cloud-during-cloudsnapshot.png)


### <a name="local-storage-usage"></a>Yerel depolama kullanımı
Bu grafik birden fazla birincil depolama kullanımı SSD doğrusal katmanı içerdiğinden olacağı cihaz için toplam kullanımını gösterir. Bir de var. veri miktarı bu katmanını içeren diğer katmanları cihaz kullanıcının. Böylece yeni verileri geldiğinde, eski verileri (aynı zamanda, yinelenenleri kaldırılmış sıkıştırılmış ve) HDD katmanı ve daha sonra bulut taşınır SSD doğrusal katmanındaki kapasite geçiş sırasında.

Verileri buluta katmanlı başlar kadar süre, birincil depolama üzerinde kullanılan kullanılan ve yerel depolama büyük olasılıkla birlikte artacaktır. Bu noktada, kullanılan yerel depolama büyük olasılıkla durağanlığının başlar, ancak daha fazla veri yazıldığı kullanılan birincil depolama artıracaktır.

Aşağıdaki grafikleri bir bulut anlık görüntüsü durumdayken bir StorSimple cihaz için kullanılan birincil depolama gösterir. Bulut anlık görüntüsü 11: 50'de ve o anda azalan yerel depolama başlandı. 1,09 GB 1.445 GB ile kullanılan yerel depolama kapandı. Bu büyük olasılıkla doğrusal SSD katmanına sıkıştırılmamış veri yinelenenleri kaldırılmış, sıkıştırılmış olmasını gerektiriyordu ve HDD katmanı taşındı gösterir. Cihaz SSD ve HDD katmanında zaten büyük miktarda veri varsa, bu azaltma göremeyebilirsiniz unutmayın. Bu örnekte, cihazın çok küçük miktarda veri gerekir.

![Bulut anlık görüntüsü sonra yerel depolama alanı kullanımı](./media/storsimple-8000-monitor-device/device-local-storage-after-cloudsnapshot.png)

## <a name="performance"></a>Performans
**Performans** okuma sayısını ilgili ölçümlerini izler ve ana bilgisayar sunucusu ve aygıt veya aygıtın ve bulut iSCSI başlatıcısı arabirimleri yazma işlemleri ya da arasında. Bu performans, belirli bir birimin, özel birim kapsayıcısı ve tüm birim kapsayıcıları için ölçülebilir. Performans, CPU kullanımı ve aygıtınızda çeşitli ağ arabirimleri için ağ verimliliği de içerir.

### <a name="io-performance-for-initiator-to-device"></a>Cihaz başlatıcıya için g/ç performansı
Grafik başlatıcıya Cihazınızı üretim cihaz için tüm birimler için g/ç gösterir. Çizilen ölçümleri salt okunurdur ve saniye başına baytların yazma. Ayrıca okuma, yazma ve bekleyen GÇ grafik ya da okuma ve yazma gecikmeleri.

![Cihaz başlatıcıya için g/ç performansı](./media/storsimple-8000-monitor-device/device-io-from-initiator.png)

### <a name="io-performance-for-device-to-cloud"></a>Bulut cihazın g/ç performansı
Aynı aygıt için g/ç işlemleri için verileri CİHAZDAN buluta tüm birim kapsayıcıları için çizilir. Bu aygıtta veri yalnızca doğrusal katmanında ve hiçbir şey buluta geçmiş. CİHAZDAN buluta gerçekleşen hiçbir okuma-yazma işlemleri vardır. Bu nedenle, grafik yükteki sinyal cihaz ile hizmet arasında denetleneceği zaman sıklığı karşılık gelen bir aralığı 5 dakika altındadır.

Aynı aygıt için 11: 50'de başlangıç birim veriler için bir bulut anlık görüntüsü yapılmadı. Bu verileri CİHAZDAN buluta akan ile sonuçlandı. Yazma buluta bu süre içinde sunulduğunu. G/ç grafik ne zaman anlık görüntünün alındığı zamanki için karşılık gelen bir yoğun Yazma Bayt/sn gösterir.

![Bir bulut anlık görüntüsü sırasında bulut aygıttan g/ç](./media/storsimple-8000-monitor-device/device-io-to-cloud-during-cloudsnapshot.png)

### <a name="network-throughput-for-device-network-interfaces"></a>Cihaz ağ arabirimleri için ağ verimliliği
**Ağ verimliliği** iSCSI başlatıcısı ağ arabirimlerinden ana bilgisayar sunucusu ve cihaz ve cihaz ve bulut arasında aktarılan veri miktarını ilgili ölçümleri izler. Bu ölçüm, Cihazınızda her iSCSI ağ arabirimleri için izleyebilirsiniz.

Aşağıdaki grafikleri ağ verimliliği verileri için 0, 1 1 Göster GbE ağ aygıtınızda olan her iki bulut etkin (varsayılan) ve iSCSI etkin. 14 Haziran yaklaşık 9 pm adresindeki üzerinde bu aygıtta veri bulutunu katmanlı (hiç bulut anlık görüntü verileri buluta taşımak için bir mekanizma olan katmanlama gösteren o anda gerçekleştirilen) buluta sunulmasını GÇ içinde sonuçlandı. Aynı anda Ağ işleme grafiğinde karşılık gelen yoğun yoktur ve çoğu ağ trafiğini buluta giden.

![Data 0 için ağ verimliliği](./media/storsimple-8000-monitor-device/device-network-throughput-data0.png)

Biz verilerin 1 arabirimi üretilen iş grafiği bakarsanız, başka bir 1 GbE ağ, yalnızca iSCSI etkin arabirimi, sonra bu sürenin neredeyse hiçbir ağ trafiğini vardı.

![Veri 1 için bir ağ verimliliği](./media/storsimple-8000-monitor-device/device-network-throughput-data1.png)


## <a name="cpu-utilization-for-device"></a>Cihaz için CPU kullanımı
**CPU kullanımı** aygıtınızda kullanılan CPU ilgili ölçümleri izler. Aşağıdaki grafikte bir aygıt için CPU kullanımı istatistiklerini üretimde gösterir.

![Cihaz için CPU kullanımı](./media/storsimple-8000-monitor-device/device-cpu-utilization.png)



## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [StorSimple Aygıt Yöneticisi'ni hizmet cihaz Panosu kullanmak](storsimple-device-dashboard.md).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

