---
title: Azure IOT hub'ı izlemek için ölçümleri kullanma | Microsoft Docs
description: Değerlendirmek ve IOT hub'ları genel durumunu izlemek için Azure IOT hub'ı ölçümleri kullanma
author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: jlian
ms.openlocfilehash: 6afebfe9a5db713e31fed0acd2e8ad7244f30037
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67274929"
---
# <a name="understand-iot-hub-metrics"></a>IOT hub'ı ölçüleri anlama

IOT Hub ölçümler, Azure aboneliğinizde Azure IOT kaynakların durumuyla ilgili daha iyi veriler sunar. IOT hub'ı ölçümleri IOT Hub hizmeti ve ona bağlı aygıtlar genel durumunu değerlendirmek etkinleştirin. Kullanıcıya yönelik istatistikleri, IOT hub ve Yardım kök neden sorunlarınızı Azure desteğine başvurun gerek kalmadan neler olduğunu görmenize yardımcı olmak için önemlidir.

Ölçümler, varsayılan olarak etkindir. Azure portalındaki IOT hub'ı ölçümleri görüntüleyebilir.

## <a name="how-to-view-iot-hub-metrics"></a>IOT hub'ı ölçümlerini görüntüleme

1. IOT hub oluşturun. IOT hub'ı oluşturma hakkında yönergeler bulabilirsiniz [telemetri gönderir bir CİHAZDAN IOT Hub'ına](quickstart-send-telemetry-dotnet.md) Kılavuzu.

2. IOT hub'ın dikey penceresini açın. Burada, tıklayın **ölçümleri**.
   
    ![Ölçümleri seçeneğin IOT hub'ı kaynak sayfasında olduğu gösteren ekran görüntüsü](./media/iot-hub-metrics/enable-metrics-1.png)

3. Ölçümler dikey penceresinden, IOT hub'ınız için ölçümleri görüntüleyebilir ve ölçümlerinizi özel görünümlerini oluşturma. 
   
    ![IOT hub'ı için ölçümleri sayfasını gösteren ekran görüntüsü](./media/iot-hub-metrics/enable-metrics-2.png)
    
4. Tıklayarak bir olay hub'ları uç nokta veya bir Azure depolama hesabı ölçümleri verilerinizi göndermek seçebileceğiniz **tanılama ayarları**, ardından **tanılama ayarı ekleme**

   ![Tanılama ayarları düğmesi nerede gösteren ekran görüntüsü](./media/iot-hub-metrics/enable-metrics-3.png)

## <a name="iot-hub-metrics-and-how-to-use-them"></a>IOT hub'ı ölçümleri ve bunları kullanma

IOT Hub, hub'ınıza bağlı cihazların toplam sayısı ve sistem durumu özetini size çeşitli ölçümleri sağlar. IOT hub durumunu daha kapsamlı bir resim boyamak için birden çok Ölçüm bilgilerini birleştirebilirsiniz. Aşağıdaki tabloda her IOT hub'ı izler ölçümleri ve her ölçümü genel durumu IOT hub'ın ilişkisini açıklar.

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|d2c<br>.telemetry<br>.ingress.<br>allProtocol|Telemetri ileti gönderme denemeleri|Count|Toplam|IOT hub'ınıza gönderilecek CİHAZDAN buluta telemetri iletilerini sayısı çalıştı|Boyut yok|
|d2c<br>.telemetry<br>.ingress<br>.Success|Gönderilen telemetri iletilerini|Sayı|Toplam|Başarılı bir şekilde IOT hub'ınıza CİHAZDAN buluta telemetri ileti sayısı|Boyut yok|
|c2d<br>.Commands<br>.egress<br>.Complete<br>.Success|Komut tamamlandı|Count|Toplam|Cihaz tarafından başarıyla tamamlandı bulut-cihaz komutlarının sayısı|Boyut yok|
|c2d<br>.Commands<br>.egress<br>.abandon<br>.Success|Terk komutları|Count|Toplam|Cihaz tarafından terk bulut-cihaz komutlarının sayısı|Boyut yok|
|c2d<br>.Commands<br>.egress<br>.Reject<br>.Success|Reddedilen komutları|Sayı|Toplam|Cihaz tarafından reddedilen bulut-cihaz komutlarının sayısı|Boyut yok|
|cihazlar<br>.totalDevices|(Kullanım dışı) toplam cihaz sayısı|Sayı|Toplam|IOT hub'ınıza kayıtlı cihaz sayısı|Boyut yok|
|cihazlar<br>.connectedDevices<br>.allProtocol|Bağlı cihazlar (kullanım dışı) |Sayı|Toplam|IOT hub'ınıza bağlı cihazların sayısı|Boyut yok|
|d2c<br>.telemetry<br>.egress<br>.Success|Yönlendirme: teslim telemetri iletilerini|Count|Toplam|İletileri IOT Hub'ın yönlendirme kullanarak tüm uç noktalara başarıyla teslim sayısı. Bir ileti birden çok Uç noktalara yönlendirilir, bu değer her başarılı bir teslimat için bir tane artırır. Bir ileti birden çok kez aynı uç noktasına teslim ise bu değer her başarılı bir teslimat için bir tane artırır.|Boyut yok|
|d2c<br>.telemetry<br>.egress<br>.dropped|Yönlendirme: bırakılan telemetri iletilerini |Sayı|Toplam|İletileri IOT Hub'ın nedeniyle ölü uç noktalarına yönlendirme tarafından bırakılan sayısı. Bu değer, geri dönüş yol bırakılan iletiler var. iletilmemiş olarak sunulan iletiler sayılmaz.|Boyut yok|
|d2c<br>.telemetry<br>.egress<br>.orphaned|Yönlendirme: yalnız bırakılmış telemetri iletilerini |Count|Toplam|(Temel kural dahil) tüm yönlendirme kuralları ile eşleşmedi çünkü iletileri IOT Hub'ı yönlendirerek yalnız bırakılmış sayısı. |Boyut yok|
|d2c<br>.telemetry<br>.egress<br>.invalid|Yönlendirme: uyumsuz telemetri iletilerini|Count|Toplam|IOT Hub'ın yönlendirme uç nokta ile uyumsuzluk nedeniyle ileti teslim edilemedi sayısı. Bu değer, yeniden denemeler içermez.|Boyut yok|
|d2c<br>.telemetry<br>.egress<br>.fallback|Yönlendirme: ileti geri dönüş için teslim|Sayı|Toplam|Geri dönüş rota ile ilişkili uç noktasına iletileri IOT Hub'ın yönlendirme teslim sayısı.|Boyut yok|
|d2c<br>.endpoints<br>.egress<br>.eventHubs|Yönlendirme: iletilerin olay Hub'ına teslim|Count|Toplam|IOT hub'ı başarıyla yönlendirme, olay hub'ı uç noktalar için ileti teslim sayısı.|Boyut yok|
|d2c<br>.endpoints<br>.latency<br>.eventHubs|Yönlendirme: gecikme süresi için Event Hub ileti|Milisaniye|Ortalama|Ortalama gecikme süresi (milisaniye) IOT hub'ına giriş iletisi ve ileti giriş arasında bir olay hub'ı uç nokta olarak.|Boyut yok|
|d2c<br>.endpoints<br>.egress<br>.serviceBusQueues|Yönlendirme: Service Bus kuyruğuna iletiler teslim|Sayı|Toplam|IOT hub'ı başarıyla yönlendirme iletileri Service Bus kuyruğu Uç noktalara teslim sayısı.|Boyut yok|
|d2c<br>.endpoints<br>.latency<br>.serviceBusQueues|Yönlendirme: gecikme süresi için Service Bus kuyruğuna ileti|Milisaniye|Ortalama|Ortalama gecikme süresi (milisaniye) ileti giriş IOT hub'ına telemetri iletisi giriş arasındaki içine bir Service Bus kuyruk uç noktası.|Boyut yok|
|d2c<br>.endpoints<br>.egress<br>.serviceBusTopics|Yönlendirme: Service Bus konusuna iletiler teslim|Count|Toplam|IOT hub'ı başarıyla yönlendirme iletileri Service Bus konu Uç noktalara teslim sayısı.|Boyut yok|
|d2c<br>.endpoints<br>.latency<br>.serviceBusTopics|Yönlendirme: gecikme süresi için Service Bus konusuna ileti|Milisaniye|Ortalama|Ortalama gecikme süresi (milisaniye) ileti giriş IOT hub'ına telemetri iletisi giriş arasındaki içine bir Service Bus konu başlığı uç noktası.|Boyut yok|
|d2c<br>.endpoints<br>.egress<br>.builtIn<br>.events|Yönlendirme: iletiler iletiler/olaylar için teslim|Count|Toplam|IOT hub'ı başarıyla Yönlendirme iletilerini yerleşik uç noktası (iletiler/olaylar) teslim sayısı. Bu ölçüm, yalnızca Yönlendirme etkinleştirildiğinde çalışmaya başlar (https://aka.ms/iotrouting) IOT hub'ının.|Boyut yok|
|d2c<br>.endpoints<br>.latency<br>. builtIn.events|Yönlendirme: gecikme süresi, iletiler/olaylar ileti|Milisaniye|Ortalama|Ortalama gecikme süresi (milisaniye) ileti giriş IOT hub'ına telemetri iletisi giriş arasındaki içine yerleşik uç noktası (iletiler/olaylar). Bu ölçüm, yalnızca Yönlendirme etkinleştirildiğinde çalışmaya başlar (https://aka.ms/iotrouting) IOT hub'ının.|Boyut yok|
|d2c<br>.endpoints<br>.egress<br>.Storage|Yönlendirme: depolama için ileti teslim|Count|Toplam|IOT hub'ı başarıyla yönlendirme depolama uç noktaları için ileti teslim sayısı.|Boyut yok|
|d2c<br>.endpoints<br>.latency<br>.Storage|Yönlendirme: depolama gecikmesi ileti|Milisaniye|Ortalama|Ortalama gecikme süresi (milisaniye) ileti giriş IOT hub'ına telemetri iletisi giriş arasındaki içine bir depolama uç noktası.|Boyut yok|
|d2c<br>.endpoints<br>.egress<br>.Storage<br>.bytes|Yönlendirme: veri depolama için teslim|Bayt|Toplam|Veri (bayt), IOT Hub'ın yönlendirme depolama uç noktaları için teslim.|Boyut yok|
|d2c<br>.endpoints<br>.egress<br>.Storage<br>.blobs|Yönlendirme: BLOB depolama alanına teslim|Sayı|Toplam|IOT Hub'ın yönlendirme BLOB Depolama uç noktaları için teslim sayısı.|Boyut yok|
|EventGridDeliveries|Event Grid teslim (Önizleme)|Count|Toplam|IOT hub'ı olayların sayısı, Event Grid yayımladı. Sonucu boyutu başarılı ve başarısız istek sayısı için kullanın. EventType boyut olay türünü gösterir (https://aka.ms/ioteventgrid). Where görmek için istekler gelir, EventType boyut kullanın.|Sonuç, olay türü|
|EventGridLatency|Event Grid gecikme süresi (Önizleme)|Milisaniye|Ortalama|Event Grid olay yayımlandığında IOT hub'ı olayın için ne zaman oluşturulduğu gelen ortalama gecikme süresi (milisaniye). Bu sayı, tüm olay türleri arasındaki bir ortalamadır. Belirli bir olay türü gecikme görmek için EventType boyut kullanın.|olay türü|
|d2c<br>.twin<br>.Read<br>.Success|Cihazlardan başarılı ikizi okumaları|Sayı|Toplam|Tüm başarılı cihaz tarafından başlatılan çiftlerde okuma sayısı.|Boyut yok|
|d2c<br>.twin<br>.Read<br>.failure|Cihazlardan çiftlerde okuma başarısız oldu|Sayı|Toplam|Tüm sayısı, cihaz tarafından başlatılan çiftlerde okuma başarısız oldu.|Boyut yok|
|d2c<br>.twin<br>.Read<br>.size|Çiftlerde okuma cihazlardan yanıt boyutu|Bayt|Ortalama|Ortalama, en düşük ve en fazla başarılı olan tüm cihaz tarafından başlatılan ikizi okur.|Boyut yok|
|d2c<br>.twin<br>.Update<br>.Success|Cihazlardan başarılı ikizi güncelleştirmeleri|Count|Toplam|Tüm başarılı tarafından başlatılan cihaz ikizi güncelleştirmeleri sayısı.|Boyut yok|
|d2c<br>.twin<br>.Update<br>.failure|Cihaz ikizi güncelleştirmeleri başarısız oldu|Count|Toplam|Tüm sayısı tarafından başlatılan cihaz ikizi güncelleştirmeleri başarısız oldu.|Boyut yok|
|d2c<br>.twin<br>.Update<br>.size|Cihaz ikizi güncelleştirmeleri boyutu|Bayt|Ortalama|Cihaz tarafından başlatılan ortalama, en düşük ve en büyük boyutu başarılı olan tüm güncelleştirmeleri çifti.|Boyut yok|
|c2d<br>.Methods<br>.Success|Başarılı bir doğrudan yöntem çağrıları|Count|Toplam|Tüm başarılı bir doğrudan yöntem çağrılarının sayısı.|Boyut yok|
|c2d<br>.Methods<br>.failure|Doğrudan yöntem çağrıları başarısız oldu|Count|Toplam|Tüm sayısı doğrudan yöntem çağrısı başarısız oldu.|Boyut yok|
|c2d<br>.Methods<br>.requestSize|Doğrudan yöntem çağrılarını isteği boyutu|Bayt|Ortalama|Ortalama, minimum ve maksimum başarılı olan tüm yöntemi istekleri doğrudan.|Boyut yok|
|c2d<br>.Methods<br>.responseSize|Doğrudan yöntem çağrılarını yanıt boyutu|Bayt|Ortalama|Ortalama, en az ve en fazla doğrudan yöntem yanıtların tümü başarılı.|Boyut yok|
|c2d<br>.twin<br>.Read<br>.Success|Arka uçtan başarılı ikizi okumaları|Count|Toplam|Tüm başarılı arka uç başlatılan çiftlerde okuma sayısı.|Boyut yok|
|c2d<br>.twin<br>.Read<br>.failure|Arka uçtan başarısız ikizi okumaları|Count|Toplam|Tüm sayısı, arka uç başlatılan çiftlerde okuma başarısız oldu.|Boyut yok|
|c2d<br>.twin<br>.Read<br>.size|Yanıt boyutu ikizinin arka ucundan okur|Bayt|Ortalama|Ortalama, en düşük ve en fazla başarılı olan tüm arka uç başlatılan ikizi okur.|Boyut yok|
|c2d<br>.twin<br>.Update<br>.Success|Arka uç başarılı ikizi güncelleştirmeleri|Count|Toplam|Tüm başarılı arka uç başlatılan ikizi güncelleştirmeleri sayısı.|Boyut yok|
|c2d<br>.twin<br>.Update<br>.failure|Arka uç başarısız ikizi güncelleştirmeleri|Count|Toplam|Tüm sayısı, arka uç başlatılan ikizi güncelleştirmeleri başarısız oldu.|Boyut yok|
|c2d<br>.twin<br>.Update<br>.size|Arka uç ikizi güncelleştirmeleri boyutu|Bayt|Ortalama|Arka uç başlatılan ortalama, en düşük ve en büyük boyutu başarılı olan tüm güncelleştirmeleri çifti.|Boyut yok|
|TwinQueries<br>.Success|Başarılı çifti sorguları|Count|Toplam|Tüm başarılı ikizi sorgularının sayısı.|Boyut yok|
|TwinQueries<br>.failure|Başarısız çifti sorguları|Sayı|Toplam|Tüm başarısız ikizi sorgularının sayısı.|Boyut yok|
|TwinQueries<br>.resultSize|İkiz sorgu sonucu boyutu|Bayt|Ortalama|Ortalama, en düşük ve en fazla sonuç boyutunun tüm başarılı çifti sorguları.|Boyut yok|
|İşleri<br>.createTwinUpdateJob<br>.Success|İkiz güncelleştirmesi işlerinin başarılı oluşturma|Count|Toplam|Tüm oluşturma başarılı ikiz güncelleştirmesi işlerin sayısı.|Boyut yok|
|İşleri<br>.createTwinUpdateJob<br>.failure|İkiz güncelleştirmesi işlerinin başarısız oluşturma|Count|Toplam|İkiz güncelleştirmesi işlerinin başarısız olan tüm oluşturma sayısı.|Boyut yok|
|İşleri<br>.createDirectMethodJob<br>.Success|Yöntem çağırma işlerinin başarılı oluşturma|Sayı|Toplam|Tüm oluşturma başarılı doğrudan yöntem çağırma işlerin sayısı.|Boyut yok|
|İşleri<br>.createDirectMethodJob<br>.failure|Yöntem çağırma işlerinin başarısız oluşturma|Sayı|Toplam|Tüm başarısız oluşturma doğrudan yöntem çağırma işlerin sayısı.|Boyut yok|
|İşleri<br>.listJobs<br>.Success|Başarılı çağrılar listeleyemeyeceksiniz|Count|Toplam|Tüm başarılı çağrı listeleyemeyeceksiniz sayısı.|Boyut yok|
|İşleri<br>.listJobs<br>.failure|Başarısız çağrıları listeleyemeyeceksiniz|Count|Toplam|Tüm başarısız çağrılar listeleyemeyeceksiniz sayısı.|Boyut yok|
|İşleri<br>.cancelJob<br>.Success|Başarılı iş iptalleri|Count|Toplam|Bir işi iptal etmek için tüm başarılı çağrı sayısı.|Boyut yok|
|İşleri<br>.cancelJob<br>.failure|Başarısız iş iptalleri|Count|Toplam|Bir işi iptal etmek için tüm başarısız çağrıların sayısı.|Boyut yok|
|İşleri<br>.queryJobs<br>.Success|İş başarılı sorguları|Sayı|Toplam|Sorgu işleri yapılan tüm başarılı çağrı sayısı.|Boyut yok|
|İşleri<br>.queryJobs<br>.failure|İş sorgularının başarısız oldu|Count|Toplam|Tüm başarısız çağrılar sorgu iş sayısı.|Boyut yok|
|İşleri<br>.Completed|Tamamlanan İşler|Sayı|Toplam|Tüm tamamlanmış işlerin sayısı.|Boyut yok|
|İşleri<br>.Failed|Başarısız olan işler|Count|Toplam|Başarısız olan tüm işleri sayısı.|Boyut yok|
|d2c<br>.telemetry<br>.ingress<br>.sendThrottle|Azaltma hatalarının sayısındaki|Sayı|Toplam|Cihaz işleme nedeniyle azaltma hatalarının sayısındaki kısıtlar|Boyut yok|
|dailyMessage<br>QuotaUsed|Kullanılan iletilerinin toplam sayısı|Sayı|Ortalama|Günümüzde kullanılan toplam ileti sayısı. Bu, sıfırlanır, toplu bir değerdir, her gün 00:00 UTC.|Boyut yok|
|deviceDataUsage|Toplam cihaz veri kullanımı|Bayt|Toplam|Iothub için bağlı tüm cihazlara giden ve gelen aktarılan bayt|Boyut yok|
|totalDeviceCount|Toplam cihaz sayısı (Önizleme)|Count|Ortalama|IOT hub'ınıza kayıtlı cihaz sayısı|Boyut yok|
|Bağlı<br>DeviceCount|Bağlı cihazlar (Önizleme)|Sayı|Ortalama|IOT hub'ınıza bağlı cihazların sayısı|Boyut yok|
|Yapılandırmaları|Ölçümleri yapılandırma|Count|Toplam|Yapılandırma işlemleri için ölçümleri|Boyut yok|

## <a name="next-steps"></a>Sonraki adımlar

IOT hub'ı ölçümlere genel bakış gördüğünüze göre Azure IOT hub'ı yönetme hakkında daha fazla bilgi için bu bağlantıyı izleyin:

* [İşlemleri izleme](iot-hub-operations-monitoring.md)

Daha fazla IOT Hub'ın özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu](iot-hub-devguide.md)

* [Yapay ZEKA, Azure IOT Edge ile uç cihazlarına dağıtma](../iot-edge/tutorial-simulate-device-linux.md)
