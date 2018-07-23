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
ms.openlocfilehash: b41458f0201c46b99c09d0bfffd219743a36ad50
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39186764"
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

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|
|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Telemetri ileti gönderme denemeleri|Sayı|Toplam|IOT hub'ınıza gönderilecek CİHAZDAN buluta telemetri iletilerini sayısı çalıştı|
|d2c.telemetry.ingress.Success|Gönderilen telemetri iletilerini|Sayı|Toplam|Başarılı bir şekilde IOT hub'ınıza CİHAZDAN buluta telemetri ileti sayısı|
|c2d.commands.egress.complete.success|Komut tamamlandı|Sayı|Toplam|Cihaz tarafından başarıyla tamamlandı bulut-cihaz komutlarının sayısı|
|c2d.commands.egress.abandon.success|Terk komutları|Sayı|Toplam|Cihaz tarafından terk bulut-cihaz komutlarının sayısı|
|c2d.commands.egress.reject.success|Reddedilen komutları|Sayı|Toplam|Cihaz tarafından reddedilen bulut-cihaz komutlarının sayısı|
|devices.totalDevices|Toplam cihaz sayısı|Sayı|Toplam|IOT hub'ınıza kayıtlı cihaz sayısı|
|devices.connectedDevices.allProtocol|Bağlı cihazlar|Sayı|Toplam|IOT hub'ınıza bağlı cihazların sayısı|
|d2c.telemetry.egress.Success|Telemetri mesajlar|Sayı|Toplam|İletileri başarıyla uç noktaları (toplam) yazılmış sayısı|
|d2c.telemetry.egress.dropped|Bırakılan iletiler|Sayı|Toplam|Geri dönüş rota devre dışı bırakıldı ve tüm rotalar eşleşmedi çünkü kesilen iletisi sayısı|
|d2c.telemetry.egress.orphaned|Yalnız bırakılmış ileti|Sayı|Toplam|Geri dönüş yol dahil olmak üzere tüm rotalar eşleşmeyen ileti sayısı|
|d2c.telemetry.egress.invalid|Geçersiz ileti|Sayı|Toplam|Uç nokta ile uyumsuzluğu nedeniyle teslim edilmeyen iletiler sayısı|
|d2c.telemetry.egress.fallback|Geri dönüş koşulla eşleşen iletileri|Sayı|Toplam|Geri dönüş uç noktaya yazılan ileti sayısı|
|d2c.endpoints.egress.eventHubs|Olay hub'ı Uç noktalara mesajlar|Sayı|Toplam|İletilerin olay hub'ı Uç noktalara başarıyla yazılmış sayısı|
|d2c.endpoints.latency.eventHubs|Olay hub'ı uç noktalar için ileti gecikmesi|Milisaniye|Ortalama|Milisaniye cinsinden bir olay hub'ı uç noktası olarak IOT hub'ına ileti giriş iletisi giriş arasındaki ortalama gecikme süresi|
|d2c.endpoints.egress.serviceBusQueues|Hizmet veri yolu kuyruğu Uç noktalara mesajlar|Sayı|Toplam|İletileri Service Bus kuyruğu Uç noktalara başarıyla yazılmış sayısı|
|d2c.endpoints.latency.serviceBusQueues|Hizmet veri yolu kuyruğu uç noktalar için ileti gecikmesi|Milisaniye|Ortalama|Milisaniye cinsinden bir Service Bus kuyruğu uç noktası olarak IOT hub'ına ileti giriş iletisi giriş arasındaki ortalama gecikme süresi|
|d2c.endpoints.egress.serviceBusTopics|Hizmet veri yolu konusu Uç noktalara mesajlar|Sayı|Toplam|İletileri Service Bus konusu Uç noktalara başarıyla yazılmış sayısı|
|d2c.endpoints.latency.serviceBusTopics|Hizmet veri yolu konusu uç noktalar için ileti gecikmesi|Milisaniye|Ortalama|Milisaniye cinsinden bir Service Bus konusu uç noktası olarak IOT hub'ına ileti giriş iletisi giriş arasındaki ortalama gecikme süresi|
|d2c.endpoints.egress.builtIn.events|Mesajlar yerleşik uç noktası (iletiler/olaylar)|Sayı|Toplam|İletiler (iletiler/olaylar) yerleşik uç noktası başarıyla yazılmış sayısı|
|d2c.endpoints.latency.builtIn.events|Yerleşik uç noktası (iletiler/olaylar) için ileti gecikmesi|Milisaniye|Ortalama|Milisaniye cinsinden yerleşik uç noktası (iletiler/olaylar) içine ileti giriş IOT hub'ına ileti giriş arasındaki ortalama gecikme süresi |
|d2c.twin.read.success|Cihazlardan başarılı ikizi okumaları|Sayı|Toplam|Tüm başarılı cihaz tarafından başlatılan çiftlerde okuma sayısı.|
|d2c.twin.read.failure|Cihazlardan çiftlerde okuma başarısız oldu|Sayı|Toplam|Tüm sayısı, cihaz tarafından başlatılan çiftlerde okuma başarısız oldu.|
|d2c.Twin.Read.size|Çiftlerde okuma cihazlardan yanıt boyutu|Bayt|Ortalama|Ortalama, en düşük ve en fazla başarılı olan tüm cihaz tarafından başlatılan ikizi okur.|
|d2c.Twin.Update.Success|Cihazlardan başarılı ikizi güncelleştirmeleri|Sayı|Toplam|Tüm başarılı tarafından başlatılan cihaz ikizi güncelleştirmeleri sayısı.|
|d2c.twin.update.failure|Cihaz ikizi güncelleştirmeleri başarısız oldu|Sayı|Toplam|Tüm sayısı tarafından başlatılan cihaz ikizi güncelleştirmeleri başarısız oldu.|
|d2c.twin.update.size|Cihaz ikizi güncelleştirmeleri boyutu|Bayt|Ortalama|Cihaz tarafından başlatılan ortalama, en düşük ve en büyük boyutu başarılı olan tüm güncelleştirmeleri çifti.|
|c2d.methods.success|Başarılı bir doğrudan yöntem çağrıları|Sayı|Toplam|Tüm başarılı bir doğrudan yöntem çağrılarının sayısı.|
|c2d.methods.failure|Doğrudan yöntem çağrıları başarısız oldu|Sayı|Toplam|Tüm sayısı doğrudan yöntem çağrısı başarısız oldu.|
|c2d.methods.requestSize|Doğrudan yöntem çağrılarını isteği boyutu|Bayt|Ortalama|Ortalama, minimum ve maksimum başarılı olan tüm yöntemi istekleri doğrudan.|
|c2d.methods.responseSize|Doğrudan yöntem çağrılarını yanıt boyutu|Bayt|Ortalama|Ortalama, en az ve en fazla doğrudan yöntem yanıtların tümü başarılı.|
|c2d.twin.read.success|Arka uçtan başarılı ikizi okumaları|Sayı|Toplam|Tüm başarılı arka uç başlatılan çiftlerde okuma sayısı.|
|c2d.Twin.Read.failure|Arka uçtan başarısız ikizi okumaları|Sayı|Toplam|Tüm sayısı, arka uç başlatılan çiftlerde okuma başarısız oldu.|
|c2d.Twin.Read.size|Yanıt boyutu ikizinin arka ucundan okur|Bayt|Ortalama|Ortalama, en düşük ve en fazla başarılı olan tüm arka uç başlatılan ikizi okur.|
|c2d.Twin.Update.Success|Arka uç başarılı ikizi güncelleştirmeleri|Sayı|Toplam|Tüm başarılı arka uç başlatılan ikizi güncelleştirmeleri sayısı.|
|c2d.Twin.Update.failure|Arka uç başarısız ikizi güncelleştirmeleri|Sayı|Toplam|Tüm sayısı, arka uç başlatılan ikizi güncelleştirmeleri başarısız oldu.|
|c2d.twin.update.size|Arka uç ikizi güncelleştirmeleri boyutu|Bayt|Ortalama|Arka uç başlatılan ortalama, en düşük ve en büyük boyutu başarılı olan tüm güncelleştirmeleri çifti.|
|twinQueries.success|Başarılı çifti sorguları|Sayı|Toplam|Tüm başarılı ikizi sorgularının sayısı.|
|twinQueries.failure|Başarısız çifti sorguları|Sayı|Toplam|Tüm başarısız ikizi sorgularının sayısı.|
|twinQueries.resultSize|İkiz sorgu sonucu boyutu|Bayt|Ortalama|Ortalama, en düşük ve en fazla sonuç boyutunun tüm başarılı çifti sorguları.|
|jobs.createTwinUpdateJob.success|İkiz güncelleştirmesi işlerinin başarılı oluşturma|Sayı|Toplam|Tüm oluşturma başarılı ikiz güncelleştirmesi işlerin sayısı.|
|jobs.createTwinUpdateJob.failure|İkiz güncelleştirmesi işlerinin başarısız oluşturma|Sayı|Toplam|İkiz güncelleştirmesi işlerinin başarısız olan tüm oluşturma sayısı.|
|jobs.createDirectMethodJob.success|Yöntem çağırma işlerinin başarılı oluşturma|Sayı|Toplam|Tüm oluşturma başarılı doğrudan yöntem çağırma işlerin sayısı.|
|jobs.createDirectMethodJob.failure|Yöntem çağırma işlerinin başarısız oluşturma|Sayı|Toplam|Tüm başarısız oluşturma doğrudan yöntem çağırma işlerin sayısı.|
|jobs.listJobs.success|Başarılı çağrılar listeleyemeyeceksiniz|Sayı|Toplam|Tüm başarılı çağrı listeleyemeyeceksiniz sayısı.|
|jobs.listJobs.failure|Başarısız çağrıları listeleyemeyeceksiniz|Sayı|Toplam|Tüm başarısız çağrılar listeleyemeyeceksiniz sayısı.|
|jobs.cancelJob.success|Başarılı iş iptalleri|Sayı|Toplam|Bir işi iptal etmek için tüm başarılı çağrı sayısı.|
|jobs.cancelJob.failure|Başarısız iş iptalleri|Sayı|Toplam|Bir işi iptal etmek için tüm başarısız çağrıların sayısı.|
|jobs.queryJobs.success|İş başarılı sorguları|Sayı|Toplam|Sorgu işleri yapılan tüm başarılı çağrı sayısı.|
|jobs.queryJobs.failure|İş sorgularının başarısız oldu|Sayı|Toplam|Tüm başarısız çağrılar sorgu iş sayısı.|
|Jobs.Completed|Tamamlanan işler|Sayı|Toplam|Tüm tamamlanmış işlerin sayısı.|
|Jobs.Failed|Başarısız olan işler|Sayı|Toplam|Başarısız olan tüm işleri sayısı.|

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
