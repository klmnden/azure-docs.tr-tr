---
title: Azure IOT hub'ı izlemek için ölçümleri kullanma | Microsoft Docs
description: Değerlendirmek ve IOT hub'ları genel durumunu izlemek için Azure IOT hub'ı ölçümleri kullanma
author: nberdy
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/25/2017
ms.author: nberdy
ms.openlocfilehash: b1c9854d794c88c28c82a4b6e40c81a29552223a
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "45984087"
---
# <a name="understand-iot-hub-metrics"></a>IOT hub'ı ölçüleri anlama
IOT Hub ölçümler, Azure aboneliğinizde Azure IOT kaynakların durumuyla ilgili daha iyi veriler sunar. IOT hub'ı ölçümleri IOT Hub hizmeti ve ona bağlı aygıtlar genel durumunu değerlendirmek etkinleştirin. Kullanıcıya yönelik istatistikleri, IOT hub ve Yardım kök neden sorunlarınızı Azure desteğine başvurun gerek kalmadan neler olduğunu görmenize yardımcı olmak için önemlidir.

Ölçümler, varsayılan olarak etkindir. Azure portalındaki IOT hub'ı ölçümleri görüntüleyebilir.

## <a name="how-to-view-iot-hub-metrics"></a>IOT hub'ı ölçümlerini görüntüleme
1. IOT hub oluşturun. IOT hub'ı oluşturma hakkında yönergeler bulabilirsiniz [Başlarken] [ lnk-get-started] Kılavuzu.
2. IOT hub'ın dikey penceresini açın. Burada, tıklayın **ölçümleri**.
   
    ![][1]
3. Ölçümler dikey penceresinden, IOT hub'ınız için ölçümleri görüntüleyebilir ve ölçümlerinizi özel görünümlerini oluşturma. Tıklayarak bir olay hub'ları uç nokta veya bir Azure depolama hesabı ölçümleri verilerinizi göndermek seçebileceğiniz **tanılama ayarları**.
   
    ![][2]

## <a name="iot-hub-metrics-and-how-to-use-them"></a>IOT hub'ı ölçümleri ve bunları kullanma
IOT Hub, hub'ınıza bağlı cihazların toplam sayısı ve sistem durumu özetini size çeşitli ölçümleri sağlar. IOT hub durumunu daha kapsamlı bir resim boyamak için birden çok Ölçüm bilgilerini birleştirebilirsiniz. Aşağıdaki tabloda her IOT hub'ı izler ölçümleri ve her ölçümü genel durumu IOT hub'ın ilişkisini açıklar.

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Telemetri ileti gönderme denemeleri|Sayı|Toplam|IOT hub'ınıza gönderilecek CİHAZDAN buluta telemetri iletilerini sayısı çalıştı|Boyut yok|
|d2c.telemetry.ingress.Success|Gönderilen telemetri iletilerini|Sayı|Toplam|Başarılı bir şekilde IOT hub'ınıza CİHAZDAN buluta telemetri ileti sayısı|Boyut yok|
|c2d.commands.egress.complete.success|Komut tamamlandı|Sayı|Toplam|Cihaz tarafından başarıyla tamamlandı bulut-cihaz komutlarının sayısı|Boyut yok|
|c2d.commands.egress.abandon.success|Terk komutları|Sayı|Toplam|Cihaz tarafından terk bulut-cihaz komutlarının sayısı|Boyut yok|
|c2d.commands.egress.reject.success|Reddedilen komutları|Sayı|Toplam|Cihaz tarafından reddedilen bulut-cihaz komutlarının sayısı|Boyut yok|
|devices.totalDevices|(Kullanım dışı) toplam cihaz sayısı|Sayı|Toplam|IOT hub'ınıza kayıtlı cihaz sayısı|Boyut yok|
|devices.connectedDevices.allProtocol|Bağlı cihazlar (kullanım dışı) |Sayı|Toplam|IOT hub'ınıza bağlı cihazların sayısı|Boyut yok|
|d2c.telemetry.egress.Success|Yönlendirme: teslim telemetri iletilerini|Sayı|Toplam|İletileri IOT Hub'ın yönlendirme kullanarak tüm uç noktalara başarıyla teslim sayısı. Bir ileti birden çok Uç noktalara yönlendirilir, bu değer her başarılı bir teslimat için bir tane artırır. Bir ileti birden çok kez aynı uç noktasına teslim ise bu değer her başarılı bir teslimat için bir tane artırır.|Boyut yok|
|d2c.telemetry.egress.dropped|Yönlendirme: bırakılan telemetri iletilerini |Sayı|Toplam|İletileri IOT Hub'ın nedeniyle ölü uç noktalarına yönlendirme tarafından bırakılan sayısı. Bu değer, geri dönüş yol bırakılan iletiler var. iletilmemiş olarak sunulan iletiler sayılmaz.|Boyut yok|
|d2c.telemetry.egress.orphaned|Yönlendirme: yalnız bırakılmış telemetri iletilerini |Sayı|Toplam|(Temel kural dahil) tüm yönlendirme kuralları ile eşleşmedi çünkü iletileri IOT Hub'ı yönlendirerek yalnız bırakılmış sayısı. |Boyut yok|
|d2c.telemetry.egress.invalid|Yönlendirme: uyumsuz telemetri iletilerini|Sayı|Toplam|IOT Hub'ın yönlendirme uç nokta ile uyumsuzluk nedeniyle ileti teslim edilemedi sayısı. Bu değer, yeniden denemeler içermez.|Boyut yok|
|d2c.telemetry.egress.fallback|Yönlendirme: ileti geri dönüş için teslim|Sayı|Toplam|Geri dönüş rota ile ilişkili uç noktasına iletileri IOT Hub'ın yönlendirme teslim sayısı.|Boyut yok|
|d2c.endpoints.egress.eventHubs|Yönlendirme: iletilerin olay Hub'ına teslim|Sayı|Toplam|IOT hub'ı başarıyla yönlendirme, olay hub'ı uç noktalar için ileti teslim sayısı.|Boyut yok|
|d2c.endpoints.latency.eventHubs|Yönlendirme: gecikme süresi için Event Hub ileti|Milisaniye|Ortalama|Ortalama gecikme süresi (milisaniye) IOT hub'ına giriş iletisi ve ileti giriş arasında bir olay hub'ı uç nokta olarak.|Boyut yok|
|d2c.endpoints.egress.serviceBusQueues|Yönlendirme: Service Bus kuyruğuna iletiler teslim|Sayı|Toplam|IOT hub'ı başarıyla yönlendirme iletileri Service Bus kuyruğu Uç noktalara teslim sayısı.|Boyut yok|
|d2c.endpoints.latency.serviceBusQueues|Yönlendirme: gecikme süresi için Service Bus kuyruğuna ileti|Milisaniye|Ortalama|Ortalama gecikme süresi (milisaniye) ileti giriş IOT hub'ına telemetri iletisi giriş arasındaki içine bir Service Bus kuyruk uç noktası.|Boyut yok|
|d2c.endpoints.egress.serviceBusTopics|Yönlendirme: Service Bus konusuna iletiler teslim|Sayı|Toplam|IOT hub'ı başarıyla yönlendirme iletileri Service Bus konu Uç noktalara teslim sayısı.|Boyut yok|
|d2c.endpoints.latency.serviceBusTopics|Yönlendirme: gecikme süresi için Service Bus konusuna ileti|Milisaniye|Ortalama|Ortalama gecikme süresi (milisaniye) ileti giriş IOT hub'ına telemetri iletisi giriş arasındaki içine bir Service Bus konu başlığı uç noktası.|Boyut yok|
|d2c.endpoints.egress.builtIn.events|Yönlendirme: iletiler iletiler/olaylar için teslim|Sayı|Toplam|IOT hub'ı başarıyla Yönlendirme iletilerini yerleşik uç noktası (iletiler/olaylar) teslim sayısı.|Boyut yok|
|d2c.endpoints.latency.builtIn.events|Yönlendirme: gecikme süresi, iletiler/olaylar ileti|Milisaniye|Ortalama|Ortalama gecikme süresi (milisaniye) ileti giriş IOT hub'ına telemetri iletisi giriş arasındaki içine yerleşik uç noktası (iletiler/olaylar).|Boyut yok|
|d2c.endpoints.egress.Storage|Yönlendirme: depolama için ileti teslim|Sayı|Toplam|IOT hub'ı başarıyla yönlendirme depolama uç noktaları için ileti teslim sayısı.|Boyut yok|
|d2c.endpoints.latency.Storage|Yönlendirme: depolama gecikmesi ileti|Milisaniye|Ortalama|Ortalama gecikme süresi (milisaniye) ileti giriş IOT hub'ına telemetri iletisi giriş arasındaki içine bir depolama uç noktası.|Boyut yok|
|d2c.endpoints.egress.Storage.bytes|Yönlendirme: veri depolama için teslim|Bayt|Toplam|Veri (bayt), IOT Hub'ın yönlendirme depolama uç noktaları için teslim.|Boyut yok|
|d2c.endpoints.egress.Storage.BLOBS|Yönlendirme: BLOB depolama alanına teslim|Sayı|Toplam|IOT Hub'ın yönlendirme BLOB Depolama uç noktaları için teslim sayısı.|Boyut yok|
|d2c.twin.read.success|Cihazlardan başarılı ikizi okumaları|Sayı|Toplam|Tüm başarılı cihaz tarafından başlatılan çiftlerde okuma sayısı.|Boyut yok|
|d2c.twin.read.failure|Cihazlardan çiftlerde okuma başarısız oldu|Sayı|Toplam|Tüm sayısı, cihaz tarafından başlatılan çiftlerde okuma başarısız oldu.|Boyut yok|
|d2c.Twin.Read.size|Çiftlerde okuma cihazlardan yanıt boyutu|Bayt|Ortalama|Ortalama, en düşük ve en fazla başarılı olan tüm cihaz tarafından başlatılan ikizi okur.|Boyut yok|
|d2c.Twin.Update.Success|Cihazlardan başarılı ikizi güncelleştirmeleri|Sayı|Toplam|Tüm başarılı tarafından başlatılan cihaz ikizi güncelleştirmeleri sayısı.|Boyut yok|
|d2c.twin.update.failure|Cihaz ikizi güncelleştirmeleri başarısız oldu|Sayı|Toplam|Tüm sayısı tarafından başlatılan cihaz ikizi güncelleştirmeleri başarısız oldu.|Boyut yok|
|d2c.twin.update.size|Cihaz ikizi güncelleştirmeleri boyutu|Bayt|Ortalama|Cihaz tarafından başlatılan ortalama, en düşük ve en büyük boyutu başarılı olan tüm güncelleştirmeleri çifti.|Boyut yok|
|c2d.methods.success|Başarılı bir doğrudan yöntem çağrıları|Sayı|Toplam|Tüm başarılı bir doğrudan yöntem çağrılarının sayısı.|Boyut yok|
|c2d.methods.failure|Doğrudan yöntem çağrıları başarısız oldu|Sayı|Toplam|Tüm sayısı doğrudan yöntem çağrısı başarısız oldu.|Boyut yok|
|c2d.methods.requestSize|Doğrudan yöntem çağrılarını isteği boyutu|Bayt|Ortalama|Ortalama, minimum ve maksimum başarılı olan tüm yöntemi istekleri doğrudan.|Boyut yok|
|c2d.methods.responseSize|Doğrudan yöntem çağrılarını yanıt boyutu|Bayt|Ortalama|Ortalama, en az ve en fazla doğrudan yöntem yanıtların tümü başarılı.|Boyut yok|
|c2d.twin.read.success|Arka uçtan başarılı ikizi okumaları|Sayı|Toplam|Tüm başarılı arka uç başlatılan çiftlerde okuma sayısı.|Boyut yok|
|c2d.Twin.Read.failure|Arka uçtan başarısız ikizi okumaları|Sayı|Toplam|Tüm sayısı, arka uç başlatılan çiftlerde okuma başarısız oldu.|Boyut yok|
|c2d.Twin.Read.size|Yanıt boyutu ikizinin arka ucundan okur|Bayt|Ortalama|Ortalama, en düşük ve en fazla başarılı olan tüm arka uç başlatılan ikizi okur.|Boyut yok|
|c2d.Twin.Update.Success|Arka uç başarılı ikizi güncelleştirmeleri|Sayı|Toplam|Tüm başarılı arka uç başlatılan ikizi güncelleştirmeleri sayısı.|Boyut yok|
|c2d.Twin.Update.failure|Arka uç başarısız ikizi güncelleştirmeleri|Sayı|Toplam|Tüm sayısı, arka uç başlatılan ikizi güncelleştirmeleri başarısız oldu.|Boyut yok|
|c2d.twin.update.size|Arka uç ikizi güncelleştirmeleri boyutu|Bayt|Ortalama|Arka uç başlatılan ortalama, en düşük ve en büyük boyutu başarılı olan tüm güncelleştirmeleri çifti.|Boyut yok|
|twinQueries.success|Başarılı çifti sorguları|Sayı|Toplam|Tüm başarılı ikizi sorgularının sayısı.|Boyut yok|
|twinQueries.failure|Başarısız çifti sorguları|Sayı|Toplam|Tüm başarısız ikizi sorgularının sayısı.|Boyut yok|
|twinQueries.resultSize|İkiz sorgu sonucu boyutu|Bayt|Ortalama|Ortalama, en düşük ve en fazla sonuç boyutunun tüm başarılı çifti sorguları.|Boyut yok|
|jobs.createTwinUpdateJob.success|İkiz güncelleştirmesi işlerinin başarılı oluşturma|Sayı|Toplam|Tüm oluşturma başarılı ikiz güncelleştirmesi işlerin sayısı.|Boyut yok|
|jobs.createTwinUpdateJob.failure|İkiz güncelleştirmesi işlerinin başarısız oluşturma|Sayı|Toplam|İkiz güncelleştirmesi işlerinin başarısız olan tüm oluşturma sayısı.|Boyut yok|
|jobs.createDirectMethodJob.success|Yöntem çağırma işlerinin başarılı oluşturma|Sayı|Toplam|Tüm oluşturma başarılı doğrudan yöntem çağırma işlerin sayısı.|Boyut yok|
|jobs.createDirectMethodJob.failure|Yöntem çağırma işlerinin başarısız oluşturma|Sayı|Toplam|Tüm başarısız oluşturma doğrudan yöntem çağırma işlerin sayısı.|Boyut yok|
|jobs.listJobs.success|Başarılı çağrılar listeleyemeyeceksiniz|Sayı|Toplam|Tüm başarılı çağrı listeleyemeyeceksiniz sayısı.|Boyut yok|
|jobs.listJobs.failure|Başarısız çağrıları listeleyemeyeceksiniz|Sayı|Toplam|Tüm başarısız çağrılar listeleyemeyeceksiniz sayısı.|Boyut yok|
|jobs.cancelJob.success|Başarılı iş iptalleri|Sayı|Toplam|Bir işi iptal etmek için tüm başarılı çağrı sayısı.|Boyut yok|
|jobs.cancelJob.failure|Başarısız iş iptalleri|Sayı|Toplam|Bir işi iptal etmek için tüm başarısız çağrıların sayısı.|Boyut yok|
|jobs.queryJobs.success|İş başarılı sorguları|Sayı|Toplam|Sorgu işleri yapılan tüm başarılı çağrı sayısı.|Boyut yok|
|jobs.queryJobs.failure|İş sorgularının başarısız oldu|Sayı|Toplam|Tüm başarısız çağrılar sorgu iş sayısı.|Boyut yok|
|Jobs.Completed|Tamamlanan işler|Sayı|Toplam|Tüm tamamlanmış işlerin sayısı.|Boyut yok|
|Jobs.Failed|Başarısız olan işler|Sayı|Toplam|Başarısız olan tüm işleri sayısı.|Boyut yok|
|d2c.telemetry.ingress.sendThrottle|Azaltma hatalarının sayısındaki|Sayı|Toplam|Cihaz işleme nedeniyle azaltma hatalarının sayısındaki kısıtlar|Boyut yok|
|dailyMessageQuotaUsed|Kullanılan iletilerinin toplam sayısı|Sayı|Ortalama|Günümüzde kullanılan toplam ileti sayısı. Bu, sıfırlanır, toplu bir değerdir, her gün 00:00 UTC.|Boyut yok|
|deviceDataUsage|Toplam devicedata kullanım (kullanım dışı)|Bayt|Toplam|Iothub için bağlı tüm cihazlara giden ve gelen aktarılan bayt|Boyut yok|
|deviceDataUsageV2|Toplam cihaz veri kullanımı (Önizleme)|Bayt|Toplam|Iothub için bağlı tüm cihazlara giden ve gelen aktarılan bayt|Boyut yok|
|totalDeviceCount|Toplam cihaz sayısı (Önizleme)|Sayı|Ortalama|IOT hub'ınıza kayıtlı cihaz sayısı|Boyut yok|
|connectedDeviceCount|Bağlı cihazlar (Önizleme)|Sayı|Ortalama|IOT hub'ınıza bağlı cihazların sayısı|Boyut yok|
|Yapılandırmaları|Ölçümleri yapılandırma|Sayı|Toplam|Yapılandırma işlemleri için ölçümleri|Boyut yok|

## <a name="next-steps"></a>Sonraki adımlar
IOT hub'ı ölçümlere genel bakış gördüğünüze göre Azure IOT hub'ı yönetme hakkında daha fazla bilgi için bu bağlantıyı izleyin:

* [İşlem izleme][lnk-monitor]

Daha fazla IOT Hub'ın özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu][lnk-devguide]
* [Azure IOT Edge ile sınır cihazlarına Al dağıtma][lnk-iotedge]

<!-- Links and images -->
[1]: media/iot-hub-metrics/enable-metrics-1.png
[2]: media/iot-hub-metrics/enable-metrics-2.png

[lnk-get-started]: quickstart-send-telemetry-dotnet.md
[lnk-operations-monitoring]: iot-hub-operations-monitoring.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
