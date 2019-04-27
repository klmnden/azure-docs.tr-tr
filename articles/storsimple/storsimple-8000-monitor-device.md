---
title: StorSimple 8000 serisi Cihazınızı izleyin | Microsoft Docs
description: Kullanım, g/ç performans ve kapasite kullanımı izlemek için StorSimple cihaz Yöneticisi hizmetini kullanmayı açıklar.
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
ms.openlocfilehash: 602514df69977891167f341db0ab20913bcacc9f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60634635"
---
# <a name="use-the-storsimple-device-manager-service-to-monitor-your-storsimple-device"></a>StorSimple Cihazınızı izlemek için StorSimple cihaz Yöneticisi hizmetini kullanma

## <a name="overview"></a>Genel Bakış
StorSimple cihaz Yöneticisi hizmeti, StorSimple çözümünüzün belirli cihazları izlemek için kullanabilirsiniz. G/ç performansı, kapasite kullanımı, ağ aktarım hızı ve cihaz performans ölçümlerine göre özel grafikler oluşturma ve bunları panoya sabitleyin. Daha fazla bilgi için Git [portal panonuza özelleştirme](../azure-portal/azure-portal-dashboards.md).

Azure portalında belirli bir cihaz için izleme bilgilerini görüntülemek için StorSimple cihaz Yöneticisi hizmetini seçin. Cihaz listesinden Cihazınızı seçin ve ardından Git **İzleyici**. Ardından gördüğünüz **kapasite**, **kullanım**, ve **performans** seçili cihaz için grafikler.

## <a name="capacity"></a>Kapasite
**Kapasite** sağlanan alan ve cihazda kalan alan izler. Kalan kapasite, ardından yerel olarak sabitlenmiş veya katmanlı olarak görüntülenir.

Sağlanan ve kalan kapasite daha katmanlı ve yerel olarak sabitlenmiş birimler tarafından ayrılmıştır. Her birim için sağlanan kapasiteyi ve cihazda kalan kapasite görüntülenir.

![G/ç kapasitesi](./media/storsimple-8000-monitor-device/device-capacity.png)



## <a name="usage"></a>Kullanım
**Kullanım** birimler, birim kapsayıcıları veya cihaz tarafından kullanılan veri depolama alanı miktarı ilgili ölçümleri izler. Birincil depolama, bulut depolama alanınızın veya cihaz depolama kapasitesi kullanımını temel alan raporlar oluşturabilirsiniz. Kapasite kullanımı, herhangi bir birimi, belirli bir birim kapsayıcısı veya tüm birim kapsayıcıları ölçülebilir.
Varsayılan olarak, son 24 saat için kullanım bildirilir. Grafik üzerinde kullanım seçerek bildirilen süresini değiştirmek için düzenleyebilirsiniz:
* Son 24 saat
* Son 7 gün
* Son 30 gün
* Son 90 gün
* Son yıl

İki anahtar metrices ve büyüme aralığı için kullanım grafiklerini raporlanır. Aralığın maksimum değeri ve minimum değerler seçili süresi (fo örneği, son 7 gün) boyunca bildirilen kullanımı ifade eder.

Büyüme kullanım artış ilk gününden son gününe seçili süre içinde ifade eder. 

Ayrıca büyüme ve aralığı tarafından aşağıdaki denklemler gösterilebilir:

```
Range = {Usage(minimum), Usage(maximum)}

Growth = Usage(Last day) - Usage(first day)

Growth (%) = [{Usage(last day) - Usage(first day)} X 100]/Usage(first day)
```

Birincil Bulut ve kullanılan yerel depolama gibi tanımlanabilen:

### <a name="primary-storage-usage"></a>Birincil depolama alanı kullanımı
Bu grafikler verileri kaldırılmış ve sıkıştırılmış önce StorSimple birimlerini yazılan veri miktarını gösterir. Birim kapsayıcısı veya tek bir birimin tüm birimleri tarafından kullanılan birincil depolama görüntüleyebilirsiniz. Kullanılan birincil depolama daha fazla kullanılan birincil katmanlı depolama ayrılmıştır ve birincil yerel olarak sabitlenmiş depolama kullanılıyor.

Aşağıdaki grafikleri önce StorSimple cihazını ve sonra bir bulut anlık görüntünün alındığı için kullanılan birincil depolama alanı gösterir. Bu birim verilerini olduğu gibi bir bulut anlık görüntüsünü birincil depolama değiştirmemelisiniz. Gördüğünüz gibi bir bulut anlık görüntü alma sonucu olarak kullanılan birincil katmanlı ya da yerel olarak sabitlenmiş depolama fark grafik gösterir. Bulut anlık görüntüsü yaklaşık 11:50 pm, cihazdaki başlangıcı.

![Bulut anlık görüntüsü sonra birincil kapasite kullanımı](./media/storsimple-8000-monitor-device/device-primary-storage-after-cloudsnapshot.png)

StorSimple cihazınıza bağlı olarak konak üzerindeki GÇ Şimdi Çalıştır, birincil katmanlı depolama artış görürsünüz veya kullanılan depolama alanı bağlı birincil yerel olarak sabitlenmiş hangi birimlerin (katmanlı veya yerel olarak sabitlenmiş) veri yazma. Bir StorSimple cihazı için birincil depolama kullanımı grafikleri aşağıda verilmiştir. Bu cihazda, hizmet çalışmaya StorSimple konak cihazda katmanlı birim üzerinde yaklaşık 2:30 pm yazar. Konakta çalışan g/ç karşılık gelen Yazma Bayt/sn cinsinden en yoğun görebilirsiniz.

![Katmanlı birimleri üzerinde çalışan g/ç performansı](./media/storsimple-8000-monitor-device/device-io-from-initiator.png)

Kullanılan birincil katmanlı depolama bakarsanız, birincil yerel olarak sabitlenmiş ise, hiçbir yazma cihazda yerel olarak sabitlenmiş birimler için hizmet olarak kullanımı değişmeden kalır geçti.

![GÇ çalışıyorsa, katmanlı birimlerde birincil kapasite kullanımı](./media/storsimple-8000-monitor-device/device-primary-storage-io-from-initiator.png)

Çalıştırıyorsanız güncelleştirme 3 veya üzeri, birincil depolama kapasitesi kullanımı tek bir birimin, tüm birimleri, tüm katmanlı birimlerin ve tüm yerel olarak sabitlenmiş birimler tarafından aşağıda gösterildiği gibi bozabilir. Tüm yerel olarak sabitlenmiş birimler tarafından bozucu, hızlı bir şekilde ne kadar yerel katmanı kullanılacağını belirlemek izin verir.

![Tüm katmanlı birimler için birincil kapasite kullanımı](./media/storsimple-8000-monitor-device/monitor-usage3.png)

![Tüm yerel olarak sabitlenmiş birimler için birincil kapasite kullanımı](./media/storsimple-8000-monitor-device/monitor-usage4.png)

Daha fazla, her bir birimi listesinde tıklayın ve ilgili kullanım bakın.

![Tüm yerel olarak sabitlenmiş birimler için birincil kapasite kullanımı](./media/storsimple-8000-monitor-device/device-primary-storage-usage-by-volume.png)

### <a name="cloud-storage-usage"></a>Bulut depolama kullanımı
Bu grafikler kullanılan bulut depolama alanı miktarını gösterir. Bu verileri kaldırılmış ve sıkıştırılmış. Bu miktar, birincil herhangi bir birim yansıtılan değildir ve eski ya da gerekli elde tutma amacıyla tutulur verileri içerebilen bir bulut anlık görüntüleri içerir. Birincil karşılaştırın ve sayı tam olmaz ancak depolama tüketimi rakamlarını veri azaltma oranı hakkında bir fikir edinmek için bulut.

Bir bulut anlık görüntü alındığında aşağıdaki grafikleri bir StorSimple cihazı bulut depolama kullanımını gösterir.

* Bulut anlık görüntüsü yaklaşık 11:50 da o cihazda başlatılma ve bulut anlık görüntü önce olduğunu kullanılan bulut depolama alanı görebilirsiniz. 
* Bir kez tamamlandı bulut anlık görüntüsü bulut depolama alanı kullanımı 0,89 GB görüntüsü. 
* Bulut anlık görüntüsü devam ederken bulunmaktadır ayrıca karşılık gelen yoğun CİHAZDAN buluta GÇ.

    ![Bulut anlık görüntüsü önce bulut depolama alanı kullanımı](./media/storsimple-8000-monitor-device/device-cloud-storage-before-cloudsnapshot.png)

    ![Bulut anlık görüntüsü sonra bulut depolama alanı kullanımı](./media/storsimple-8000-monitor-device/device-cloud-storage-after-cloudsnapshot.png)

    ![CİHAZDAN bulut anlık görüntüsü sırasında buluta GÇ](./media/storsimple-8000-monitor-device/device-io-to-cloud-during-cloudsnapshot.png)


### <a name="local-storage-usage"></a>Yerel depolama alanı kullanımı
Bu grafikler birden fazla birincil depolama alanı kullanımı doğrusal SSD katmanına içerdiğinden olacağı cihaz için toplam kullanımını gösterir. Bir de bulunuyor. veri miktarı bu katmanını içeren cihaz kullanıcının diğer katmanları. Böylece yeni veri geldiğinde, eski verileri (aynı zamanda, yinelenen verileri kaldırma işlemi sıkıştırılmış ve) HDD katmanı ve daha sonra bulut taşınır doğrusal SSD katmanına kapasitede yeniden sağlanıncaya.

Verileri buluta katmanlanmış başlar kadar süre, birincil depolama alanı üzerinden kullanılan ve yerel depolama kullanılan büyük olasılıkla birlikte artar. Bu noktada kullanılan yerel depolama büyük olasılıkla durağanlığının başladığı başlayacaktır, ancak daha fazla veri yazıldığı gibi kullanılan birincil depolama artacaktır.

Aşağıdaki grafikleri bir bulut anlık görüntü alındığında bir StorSimple cihazı için kullanılan birincil depolama alanı gösterir. Bulut anlık görüntüsü 11: 50'de ve o anda azalan yerel depolama başlandı. Kullanılan yerel depolama 1.445 GB ile 1,09 GB e düştü. Bu büyük olasılıkla sıkıştırılmamış veri doğrusal SSD katmanında yinelenen verileri kaldırma işlemi, sıkıştırılmış ve HDD katmanına taşınan gösterir. Cihaz SSD ve HDD katmanda zaten büyük miktarda veri varsa, bu azalma göremeyebilirsiniz unutmayın. Bu örnekte, cihazın az miktarda veriniz var.

![Bulut anlık görüntüsü sonra yerel depolama alanı kullanımı](./media/storsimple-8000-monitor-device/device-local-storage-after-cloudsnapshot.png)

## <a name="performance"></a>Performans
**Performans** okuma sayısı ilgili ölçümleri izler ve ana bilgisayar sunucusu ve cihaz veya cihaz ve buluta iSCSI başlatıcısı arabirimleri yazma işlemleri arasında ya da. Bu performans, herhangi bir birimi, belirli bir birim kapsayıcısı ve tüm birim kapsayıcıları için ölçülebilir. Performans, CPU kullanımı ve Cihazınızda çeşitli ağ arabirimleri için ağ verimini de içerir.

### <a name="io-performance-for-initiator-to-device"></a>Başlatıcıdan cihaza GÇ performansı
Aşağıdaki grafik, cihazınıza bir üretim cihaz için tüm birimler için Başlatıcı g/ç gösterir. Çizilen ölçümleri okuma ve yazma bayt / saniye. Ayrıca okuma, yazma ve bekleyen GÇ grafik ya da okuma ve yazma gecikme.

![Başlatıcıdan cihaza GÇ performansı](./media/storsimple-8000-monitor-device/device-io-from-initiator.png)

### <a name="io-performance-for-device-to-cloud"></a>CİHAZDAN buluta GÇ performansı
Aynı cihaz için cihaz verileri için tüm birim kapsayıcıları buluta için g/ç işlemleri çizilir. Bu cihazda veri yalnızca doğrusal katmanında ve hiçbir şey buluta geçmiş. CİHAZDAN buluta yürütülen bir okuma-yazma işlem vardır. Bu nedenle, grafikteki en yüksek sayılar sinyal cihaz ile hizmet arasında denetleneceği zaman sıklığı karşılık gelen bir 5 dakika aralık altındadır.

Aynı cihaz için 11: 50'de başlayarak toplu veriler için bir bulut anlık görüntünün alındığı. CİHAZDAN buluta giden veri ile sonuçlandı. Yazma işlemleri, bu süre içinde buluta sunulduğunu. GÇ grafik, Yazma Bayt/sn, anlık görüntünün alındığı zaman karşılık gelen bir tepe gösterir.

![CİHAZDAN bulut anlık görüntüsü sırasında buluta GÇ](./media/storsimple-8000-monitor-device/device-io-to-cloud-during-cloudsnapshot.png)

### <a name="network-throughput-for-device-network-interfaces"></a>Cihaz ağ arabirimleri için ağ aktarım hızı
**Ağ aktarım hızı** iSCSI başlatıcısı ağ arabirimlerini ana bilgisayar sunucusunda hem de cihaz ve cihaz ile bulut arasında aktarılan veri miktarı için ilgili ölçümleri izler. Bu ölçüm, Cihazınızda her iSCSI ağ arabirimleri için izleyebilirsiniz.

Aşağıdaki grafikleri ağ aktarım hızı 0, 1 1 göstermek hem de bulut özellikli (varsayılan), Cihazınızda GbE ağ ve iSCSI etkin. Bu cihazda 14 Haziran'da saat yaklaşık 21: 00, buluta katmanlanmış verileri (hiçbir bulut anlık görüntüleri olan verileri buluta taşımak için bir mekanizma katmanlama için işaret o zaman çekildiği) içinde buluta sunulmasını GÇ sonuçlandı. Buluta giden ağ trafiğini çoğunu ve aynı anda ağ aktarım hızı grafikte karşılık gelen yoğun yoktur.

![Data 0 için ağ aktarım hızı](./media/storsimple-8000-monitor-device/device-network-throughput-data0.png)

Veri 1 arabirimi aktarım hızı grafiğe baktığımızda, başka bir 1 GbE ağ arabirimi, yalnızca iSCSI etkin olduğu ve bu süre içinde neredeyse hiçbir ağ trafiği vardı.

![Ağ aktarım hızı için veri 1](./media/storsimple-8000-monitor-device/device-network-throughput-data1.png)


## <a name="cpu-utilization-for-device"></a>Cihaz için CPU kullanımı
**CPU kullanımı** Cihazınızda kullandığı CPU ilgili ölçümleri izler. Aşağıdaki grafik, üretim ortamında bir cihaz için CPU kullanımı istatistikleri gösterir.

![Cihaz için CPU kullanımı](./media/storsimple-8000-monitor-device/device-cpu-utilization.png)



## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [StorSimple cihaz Yöneticisi hizmeti cihaz panosunu kullanma](storsimple-device-dashboard.md).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

