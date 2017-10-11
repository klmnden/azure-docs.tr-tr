---
title: "StorSimple Cihazınızı izleme | Microsoft Docs"
description: "G/ç performansı, kapasite kullanımı, ağ verimliliğini ve cihaz performansı izlemek için StorSimple Yöneticisi hizmetini kullanmayı açıklar."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: bd4f7704-4f6f-47d0-927a-b1c91eabc453
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/16/2016
ms.author: alkohli
ms.openlocfilehash: d45bb37c8417785db0ea38be4375a998b6d9f109
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-monitor-your-storsimple-device"></a>StorSimple Cihazınızı izlemek için StorSimple Yöneticisi hizmetini kullanma
## <a name="overview"></a>Genel Bakış
StorSimple çözümünüzün içinde belirli aygıtları izlemek için StorSimple Yöneticisi hizmetini kullanabilirsiniz. G/ç performansı, kapasite kullanımı, ağ verimliliğini ve cihaz performans ölçümleri göre özel grafikler oluşturabilirsiniz. 

Azure Klasik Portalı'nda belirli bir aygıt için izleme bilgilerini görüntülemek için StorSimple Yöneticisi hizmeti seçin. Tıklatın **İzleyici** sekmesini tıklatın ve sonra aygıt listesinden seçin. **İzleyici** sayfasında aşağıdaki bilgileri içerir.

## <a name="io-performance"></a>G/ç performansı
**G/ç performansı** okuma sayısını ilgili ölçümlerini izler ve ana bilgisayar sunucusu ve aygıt veya aygıtın ve bulut iSCSI başlatıcısı arabirimleri yazma işlemleri ya da arasında. Bu performans, belirli bir birimin, özel birim kapsayıcısı ve tüm birim kapsayıcıları için ölçülebilir.

Grafik başlatıcıya Cihazınızı üretim cihaz için tüm birimler için g/ç gösterir. Çizilen ölçümleri salt okunurdur ve saniye başına baytların yazma, okuma ve yazma g/ç işlemleri, saniyede ve okuma ve gecikme yazma.

![Aygıta Başlatıcı g/ç performansı](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_InitiatorTODevice_For_AllVolumesM.png)

Aynı aygıt için g/ç işlemleri için verileri CİHAZDAN buluta tüm birim kapsayıcıları için çizilir. Bu aygıtta veri yalnızca doğrusal katmanında ve hiçbir şey buluta geçmiş. CİHAZDAN buluta gerçekleşen hiçbir okuma-yazma işlemleri vardır. Bu nedenle, grafik yükteki sinyal cihaz ile hizmet arasında denetleneceği zaman sıklığı karşılık gelen bir aralığı 5 dakika altındadır. 

![Bulut aygıttan g/ç performansı](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainersM.png)

Aynı aygıt için 2: 00'dan başlayarak birim veriler için bir bulut anlık görüntüsü yapılmadı. Bu verileri CİHAZDAN buluta akan ile sonuçlandı. Okuma-yazma işlemleri, bu süre içinde buluta nasıl sunulduğunu. G/ç grafik bir yoğun zaman anlık görüntünün alındığı zamanki için karşılık gelen çeşitli ölçümleri gösterir. 

![Bulut anlık görüntü sonrası bulut cihazın g/ç performansı](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainers2M.png)

## <a name="capacity-utilization"></a>Kapasite kullanımı
**Kapasite kullanımı** birimler, birim kapsayıcıları veya aygıt tarafından kullanılan veri depolama alanı miktarı ilgili ölçümleri izler. Birincil depolama, bulut depolama veya aygıt depolama kapasitesi kullanımı temelinde raporlar oluşturabilir. Kapasite kullanımı, belirli bir birimin, özel birim kapsayıcısı veya tüm birim kapsayıcıları ölçülebilir.

Birincil Bulut ve cihaz depolama kapasitesi gibi tanımlanabilen:

### <a name="primary-storage-capacity-utilization"></a>Birincil depolama kapasitesi kullanımı
Bu grafik veri yinelenenleri kaldırılmış ve sıkıştırılmış önce StorSimple birimlerini yazılan veri miktarını gösterir. Tüm birimleri veya tek bir birim için birincil depolama alanı kullanımı görüntüleyebilirsiniz.

Tek tek birimlerin her birinde karşı tüm birimler için birincil depolama birimi kapasite kullanımı grafikleri görüntüleyin ve bu iki durumda birincil veri toplamak, iki sayı arasında bir uyuşmazlık olabilir. Tüm birimlerde toplam birincil veri ayrı birimleri birincil veri Toplamı toplam ekleyebilir değil. Bu aşağıdakilerden biri nedeniyle olabilir:

* **Tüm birimler için dahil edilen veri anlık görüntü**: yalnızca Sürüm güncelleştirme 3'ten önceki çalıştırıyorsanız bu davranış görülür. Birincil veri birimleri için gösterilen her birim için birincil veri ve anlık görüntü verilerini toplamıdır. Belirli bir birim için gösterilen birincil veri yalnızca birimde tahsis edilen veri miktarını karşılık gelir (ve karşılık gelen birim anlık görüntü verilerini dahil etmez).
  
    Bu ayrıca deneylerin açıklanabilir:
  
    *Birincil veri (tüm birimler) = (birincil veri (birim i) + (birim i) anlık görüntü veri boyutu) toplamı*
  
    *WHERE, birincil veri (birim i) = i birime ayrılmış birincil veri boyutu*
  
    Anlık görüntüler hizmeti aracılığıyla sildiyseniz silme işlemini zaman uyumsuz olarak arka planda gerçekleştirilir. Toplu veri boyutu anlık görüntü silme işlemi güncelleştirilmesi biraz zaman alabilir. 
  
    Güncelleştirme 3 veya sonraki sürümünü çalıştıran, anlık görüntü verilerini toplu veri bulunmaz. Ve birincil kullanımı aşağıdaki gibi hesaplanır:
  
    * Birincil veri (tüm birimler) = TOPLA (birincil veri (birim i)
  
    *WHERE, birincil veri (birim i) = i birime ayrılmış birincil veri boyutu*
* **İzleme devre dışı olan birimler dahil tüm birimleri**: kendisi için izlemeyi devre dışı olduğundan, Cihazınızda birimleri varsa, bu tek tek birimler izleme verilerini grafiklerde kullanılamaz. Ancak, veri Grafikteki tüm birimler için kendileri için izlemeyi devre dışı birimlerini içerir. 
* **Birimler dahil tüm birimler için Canlı ilişkili yedeklemelerinizi ile silinmiş**: anlık görüntü verilerini içeren birimler silinir, ancak ilişkili anlık görüntüleri hala mevcut durumunda bir uyumsuzluk görebilirsiniz.
* **Tüm birimler için dahil silinmiş**: Bu silinen olsa bile bazı durumlarda, eski birimlerin var olabilir. Silme etkisi değil görülen ve aygıt daha düşük kullanılabilir kapasite gösterebilir. Microsoft bu birimleri kaldırmak için Support başvurmanız gerekir.

Aşağıdaki grafikleri önce ve bulut anlık görüntü alındıktan sonra StorSimple cihazını birincil depolama kapasitesi kullanımını gösterir. Bir bulut anlık görüntü yalnızca birim verilerini olarak birincil depolama değiştirmemelisiniz. Gördüğünüz gibi bir bulut anlık sonucunda birincil kapasite kullanımı fark grafik gösterir. Bulut anlık görüntüsü yaklaşık 2:00 pm cihazdaki başlangıcı.

![Bulut anlık görüntüsü önce birincil kapasite kullanımı](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes2M.png)

![Bulut anlık görüntüsü sonra birincil kapasite kullanımı](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes1M.png)

Çalıştırıyorsanız, güncelleştirme 2 veya sonraki, birincil depolama kapasitesi kullanımını tek bir birim, tüm birimleri, tüm katmanlı birimlerin ve tüm yerel olarak sabitlenmiş birimleri tarafından aşağıda gösterildiği gibi bölünebilir. Tüm yerel olarak sabitlenmiş birimleri tarafından kesiliyor hızlı bir şekilde ne kadar yerel katmanının kullanıldığını onaylaması izin verir.

![Tüm yerel olarak sabitlenmiş birimler için birincil kapasite kullanımı](./media/storsimple-monitor-device/localvolumes.png)

### <a name="cloud-storage-capacity-utilization"></a>Bulut depolama kapasitesi kullanımı
Bu grafikler kullanılan bulut depolama alanı miktarını gösterir. Bu veri yinelenenleri kaldırılmış ve sıkıştırılmış. Bu miktar, tüm birincil biriminde yansıtılan değil ve gerekli veya eski bekletme amacıyla tutulur verileri içerebilen bulut anlık görüntüleri içerir. Birincil karşılaştırın ve sayı tam olmaz ancak veri azaltma oranı hakkında bir fikir edinmek için depolama tüketim rakamlarını bulut. Aşağıdaki grafikleri önce StorSimple cihazını ve bulut anlık görüntü alındıktan sonra bulut depolama kapasitesi kullanımını gösterir. Bulut anlık görüntüsü yaklaşık 2:00 pm cihazdaki başlangıcı ve bulut kapasite kullanımı görebilirsiniz 4.04 GB 5,73 MB'tan artırılması aynı anda görüntüsü.

![Bulut anlık görüntüsü önce bulut kapasite kullanımı](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers2M.png)

![Bulut anlık görüntüsü sonra bulut kapasite kullanımı](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers1M.png)

### <a name="device-storage-capacity-utilization"></a>Cihaz depolama kapasitesi kullanımı
Bu grafik birden fazla birincil depolama alanı kullanımı SSD doğrusal katmanı içerdiğinden olacağı cihaz için toplam kullanımını gösterir. Bir de var. veri miktarı bu katmanını içeren diğer katmanları cihaz kullanıcının. Böylece yeni verileri geldiğinde, eski verileri (aynı zamanda, yinelenenleri kaldırılmış sıkıştırılmış ve) HDD katmanı ve daha sonra bulut taşınır SSD doğrusal katmanındaki kapasite geçiş sırasında.

Verileri buluta katmanlı başlar kadar zaman içinde birincil kapasite kullanımı ve cihaz kapasite kullanımı büyük olasılıkla birlikte artıracaktır. Bu noktada, cihaz kapasite kullanımı büyük olasılıkla durağanlığının başlar, ancak daha fazla veri yazıldığı birincil kapasite kullanımını artırır.

Aşağıdaki grafikleri önce ve bulut anlık görüntü alındıktan sonra StorSimple cihazını birincil depolama kapasitesi kullanımını gösterir. Bulut anlık görüntüsü 2: 00'da ve o anda azalan aygıt kapasite kullanımı başlandı. Cihaz depolama kapasitesi kullanımı 11.58 GB ile 7.48 GB için kapandı. Bu büyük olasılıkla doğrusal SSD katmanına sıkıştırılmamış veri yinelenenleri kaldırılmış, sıkıştırılmış olmasını gerektiriyordu ve HDD katmanı taşındı gösterir. Cihaz SSD ve HDD katmanında zaten büyük miktarda veri varsa, bu azaltma göremeyebilirsiniz unutmayın. Bu örnekte, cihazın çok küçük miktarda veri gerekir.

![Bulut anlık görüntüsü önce aygıt kapasite kullanımı](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil2M.png)

![Bulut anlık görüntüsü sonra cihaz kapasite kullanımı](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil1M.png)

## <a name="network-throughput"></a>Ağ verimliliği
**Ağ verimliliği** iSCSI başlatıcısı ağ arabirimlerinden ana bilgisayar sunucusu ve cihaz ve cihaz ve bulut arasında aktarılan veri miktarını ilgili ölçümleri izler. Bu ölçüm, Cihazınızda her iSCSI ağ arabirimleri için izleyebilirsiniz.

Aşağıdaki grafikleri ağ verimliliği veri 0 ve veri 4, her iki 1 GbE ağ arabirimleri, Cihazınızda gösterin. Bu örnekte, veri 0 bulut-veri 4 iSCSI etkin iken etkinleştirildi. StorSimple cihazınız için hem gelen hem de giden trafiği görebilirsiniz. 3:24 pm başlayarak grafik düz satırda biz yalnızca her 5 dakikada bir veri toplamak ve dikkate alınması olgu owing. 

![Data4 için bir ağ verimliliği](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data0M.png)

![Data4 için bir ağ verimliliği](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data4M.png)

## <a name="device-performance"></a>Cihaz performansı
**Cihaz performans** Cihazınızı performansı ile ilgili ölçümleri izler. Aşağıdaki grafikte bir aygıt için CPU kullanımı istatistiklerini üretimde gösterir.

![Cihaz için CPU kullanımı](./media/storsimple-monitor-device/StorSimple_DeviceMonitor_DevicePerformance1M.png)

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [StorSimple Yöneticisi hizmet cihaz Panosu kullanmak](storsimple-device-dashboard.md).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

