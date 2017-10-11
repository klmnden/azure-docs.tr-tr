---
title: "Azure IOT hub'ı izlemek için ölçümleri kullanın | Microsoft Docs"
description: "Azure IOT Hub ölçümleri değerlendirmek ve IOT hub'larınız genel durumunu izlemek için nasıl kullanılacağını."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: a47108fd-f994-4105-b21d-5b8f697b699c
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: nberdy
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e850370faf2d271b4adad1af48c1ead7b316fa67
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="understand-iot-hub-metrics"></a>IOT hub'ı ölçümleri anlama
IOT hub'ı ölçümleri Azure aboneliğinizde Azure IOT kaynakları durumuyla ilgili daha iyi veri verin. IOT hub'ı ölçümleri IOT Hub hizmeti ve ona bağlı aygıtlar genel durumunu değerlendirmek etkinleştirin. IOT hub ve Yardım kök neden sorunlarınızı Azure desteğine başvurun gerek kalmadan neler olup bittiğini görmenize yardımcı olmak için kullanıcı dönük istatistikleri önemlidir.

Ölçümleri varsayılan olarak etkinleştirilir. IOT hub'ı ölçümleri Azure portalından görüntüleyebilirsiniz.

## <a name="how-to-view-iot-hub-metrics"></a>IOT hub'ı ölçümleri görüntüleme
1. IOT hub'ı oluşturun. Bir IOT hub oluşturma hakkında yönergeler bulabilirsiniz [Get Started] [ lnk-get-started] Kılavuzu.
2. IOT hub'ınızı dikey penceresini açın. Buradan, tıklatın **ölçümleri**.
   
    ![][1]
3. Ölçümleri dikey penceresinden için IOT hub'ınızı ölçümleri görüntüleyin ve ölçümlerinizi özel görünümler oluşturun. Ölçümleri verilerinizi bir olay hub'ları uç nokta veya bir Azure Storage hesabı tıklayarak göndermeyi seçebilirsiniz **tanılama ayarları**.
   
    ![][2]

## <a name="iot-hub-metrics-and-how-to-use-them"></a>IOT hub'ı ölçümleri ve bunların nasıl kullanılacağını
IOT Hub, hub ve bağlı cihazların toplam sayısını durumunu genel bir bakış vermek için çeşitli ölçümleri sağlar. IOT hub'ı durumunun daha kapsamlı bir resim boyamak için birden çok ölçümleri bilgilerinden birleştirebilirsiniz. Aşağıdaki tabloda, her IOT hub'ı izleyen ölçümleri ve her ölçümü IOT hub'ın genel durumunu nasıl ilişkili olduğu açıklanmaktadır.

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|
|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Telemetri ileti gönderme denemeleri|Sayı|Toplam|IOT hub'ınıza gönderilecek cihaz bulut telemetri iletilerini sayısı çalıştı|
|d2c.telemetry.ingress.Success|Gönderilen telemetri iletilerini|Sayı|Toplam|Başarıyla IOT hub'ına gönderilen cihaz bulut telemetri iletisi sayısı|
|c2d.Commands.egress.Complete.Success|Tamamlanan komutları|Sayı|Toplam|Aygıt tarafından başarıyla tamamlandı bulut cihaz komutlarının sayısı|
|c2d.Commands.egress.Abandon.Success|Terk komutları|Sayı|Toplam|Aygıt tarafından terk bulut cihaz komutlarının sayısı|
|c2d.Commands.egress.Reject.Success|Reddedilen komutları|Sayı|Toplam|Aygıt tarafından reddedilen bulut cihaz komutlarının sayısı|
|devices.totalDevices|Toplam aygıt|Sayı|Toplam|IOT hub'ınıza kayıtlı cihaz sayısı|
|devices.connectedDevices.allProtocol|Bağlı aygıtlar|Sayı|Toplam|IOT hub'ına bağlı aygıt sayısı|
|d2c.telemetry.egress.Success|Telemetri iletilerini teslim|Sayı|Toplam|İletiler (toplam) uç noktalara başarıyla yazılmış sayısı|
|d2c.telemetry.egress.dropped|Bırakılan iletiler|Sayı|Toplam|Tüm yollar eşleşmedi ve geri dönüş rota devre dışı bırakıldı kesilen iletisi sayısı|
|d2c.telemetry.egress.orphaned|Yalnız bırakılmış ileti|Sayı|Toplam|Geri dönüş yolu da dahil olmak üzere tüm yollar eşleşmeyen ileti sayısı|
|d2c.telemetry.egress.invalid|Geçersiz iletileri|Sayı|Toplam|Uç noktası ile uyumsuzluğu nedeniyle teslim edilmedi ileti sayısı|
|d2c.telemetry.egress.fallback|Geri dönüş koşulla eşleşen iletileri|Sayı|Toplam|Geri dönüş uç noktasına yazılan ileti sayısı|
|d2c.endpoints.egress.eventHubs|Olay hub'ı uç noktaları için teslim edilen ileti|Sayı|Toplam|İletiler için olay hub'ı uç noktaları başarıyla yazılmış sayısı|
|d2c.endpoints.latency.eventHubs|Olay hub'ı uç noktaları için ileti gecikme süresi|milisaniye|Ortalama|Milisaniye cinsinden bir olay hub'ı uç içine ileti giriş IOT hub'ına ve ileti giriş arasındaki ortalama gecikme süresi|
|d2c.endpoints.egress.serviceBusQueues|Hizmet veri yolu kuyruğu uç noktaları için teslim edilen ileti|Sayı|Toplam|İletileri başarıyla hizmet veri yolu kuyruğu Uç noktalara yazılmış sayısı|
|d2c.endpoints.latency.serviceBusQueues|Hizmet veri yolu kuyruğu uç noktalar için ileti gecikme süresi|milisaniye|Ortalama|Milisaniye cinsinden bir hizmet veri yolu kuyruğu uç noktası içine ileti giriş IOT hub'ına ve ileti giriş arasındaki ortalama gecikme süresi|
|d2c.endpoints.egress.serviceBusTopics|Hizmet veri yolu konusu uç noktaları için teslim edilen ileti|Sayı|Toplam|İletileri başarıyla hizmet veri yolu konusu Uç noktalara yazılmış sayısı|
|d2c.endpoints.latency.serviceBusTopics|Hizmet veri yolu konusu uç noktalar için ileti gecikme süresi|milisaniye|Ortalama|Milisaniye cinsinden bir hizmet veri yolu konusu uç noktası içine ileti giriş IOT hub'ına ve ileti giriş arasındaki ortalama gecikme süresi|
|d2c.endpoints.egress.builtIn.events|Yerleşik uç noktası (iletileri/olayları) teslim edilen ileti|Sayı|Toplam|İletileri (iletileri/olayları) yerleşik uç noktası başarıyla yazılmış sayısı|
|d2c.endpoints.latency.builtIn.events|Yerleşik uç noktası (iletileri/olayları) için ileti gecikme süresi|milisaniye|Ortalama|Milisaniye cinsinden yerleşik uç noktası (iletileri/olayları) içine ileti giriş IOT hub'ına ve ileti giriş arasındaki ortalama gecikme süresi |
|d2c.Twin.Read.Success|Başarılı twin aygıtlardan okur|Sayı|Toplam|Tüm başarılı aygıt tarafından başlatılan twin sayısını okur.|
|d2c.Twin.Read.failure|Aygıtlardan Twin okuma başarısız oldu|Sayı|Toplam|Tüm sayısı aygıt tarafından başlatılan twin okuma başarısız oldu.|
|d2c.Twin.Read.size|Aygıtlardan twin yanıt boyutu okur|Bayt|Ortalama|Cihaz tarafından başlatılan ortalama, min ve max tüm başarılı değeri okuma çifti.|
|d2c.Twin.Update.Success|Aygıtlardan başarılı twin güncelleştirmeleri|Sayı|Toplam|Tüm başarılı twin aygıt tarafından başlatılan güncelleştirme sayısı.|
|d2c.Twin.Update.failure|Aygıtlardan Twin güncelleştirmeler başarısız oldu|Sayı|Toplam|Tüm sayısı aygıt tarafından başlatılan twin güncelleştirmeler başarısız oldu.|
|d2c.Twin.Update.size|Aygıtlardan twin güncelleştirmeleri boyutu|Bayt|Ortalama|Cihaz tarafından başlatılan ortalama, min ve max boyutu tüm başarılı güncelleştirmeleri çifti.|
|c2d.methods.Success|Başarılı doğrudan yöntem çağrıları|Sayı|Toplam|Tüm başarılı doğrudan yöntemi çağrı sayısı.|
|c2d.methods.failure|Doğrudan yöntem çağrılarını başarısız oldu|Sayı|Toplam|Tüm sayısı doğrudan yöntem çağrıları başarısız oldu.|
|c2d.methods.requestSize|Doğrudan yöntem çağrılarını isteği boyutu|Bayt|Ortalama|Ortalama, min ve max tüm başarılı doğrudan yöntemi istekleri.|
|c2d.methods.responseSize|Doğrudan yöntem çağrılarını yanıt boyutu|Bayt|Ortalama|Ortalama, min ve max tüm başarılı doğrudan yöntemi yanıtların.|
|c2d.Twin.Read.Success|Arka ucunuzdan başarılı twin okur|Sayı|Toplam|Tüm başarılı arka uç başlatılan twin sayısını okur.|
|c2d.Twin.Read.failure|Arka ucunuzdan başarısız twin okur|Sayı|Toplam|Tüm sayısı arka uç başlatılan twin okuma başarısız oldu.|
|c2d.Twin.Read.size|Arka uç twin okuma yanıt boyutu|Bayt|Ortalama|Arka uç başlatılan ortalama, min ve max tüm başarılı değeri okuma çifti.|
|c2d.Twin.Update.Success|Arka uç başarılı twin güncelleştirmeleri|Sayı|Toplam|Tüm başarılı twin arka uç başlatılan güncelleştirme sayısı.|
|c2d.Twin.Update.failure|Arka uç başarısız twin güncelleştirmeleri|Sayı|Toplam|Tüm sayısı arka uç başlatılan twin güncelleştirmeler başarısız oldu.|
|c2d.Twin.Update.size|Arka uç twin güncelleştirmelerini boyutu|Bayt|Ortalama|Arka uç başlatılan ortalama, min ve max boyutu tüm başarılı güncelleştirmeleri çifti.|
|twinQueries.success|Başarılı twin sorguları|Sayı|Toplam|Tüm başarılı twin sorguları sayısı.|
|twinQueries.failure|Başarısız twin sorguları|Sayı|Toplam|Tüm başarısız twin sorguları sayısı.|
|twinQueries.resultSize|Twin sorguları sonuç boyutu|Bayt|Ortalama|Ortalama, min ve max tüm başarılı twin sorgular sonuç boyutunun.|
|jobs.createTwinUpdateJob.success|Twin güncelleştirme işlerinin başarılı oluşturma|Sayı|Toplam|Tüm başarılı twin güncelleştirme işlerinin oluşturulması sayısı.|
|jobs.createTwinUpdateJob.failure|Twin güncelleştirme işlerinin başarısız oluşturma|Sayı|Toplam|Tüm oluşturma işlemi başarısız twin güncelleştirme işlerinin sayısı.|
|jobs.createDirectMethodJob.success|Yöntem çağırma işlerinin başarılı oluşturma|Sayı|Toplam|Tüm başarılı bir şekilde oluşturulduktan doğrudan yöntemi çağırma işlerin sayısı.|
|jobs.createDirectMethodJob.failure|Yöntem çağırma işlerinin başarısız oluşturma|Sayı|Toplam|Tüm oluşturma işlemi başarısız doğrudan yöntemi çağırma işlerin sayısı.|
|jobs.listJobs.success|Liste işleri başarılı çağrıları|Sayı|Toplam|Liste işleri için tüm başarılı çağrı sayısı.|
|jobs.listJobs.failure|Liste işleri çağrı başarısız oldu|Sayı|Toplam|Tüm başarısız çağrılar listesi işlerinin sayısı.|
|jobs.cancelJob.success|Başarılı iş iptalleri|Sayı|Toplam|Bir işi iptal etmek için tüm başarılı çağrı sayısı.|
|jobs.cancelJob.failure|Başarısız iş iptalleri|Sayı|Toplam|Bir işi iptal etmek için tüm başarısız çağrı sayısı.|
|jobs.queryJobs.success|İş başarılı sorguları|Sayı|Toplam|Sorgu işlerinin tüm başarılı çağrı sayısı.|
|jobs.queryJobs.failure|Başarısız işi sorgular|Sayı|Toplam|Tüm başarısız çağrılar sorgu işlerinin sayısı.|
|Jobs.Completed|Tamamlanan İşler|Sayı|Toplam|Tüm tamamlanmış işlerin sayısı.|
|Jobs.Failed|Başarısız olan işler|Sayı|Toplam|Tüm başarısız işler sayısı.|

## <a name="next-steps"></a>Sonraki adımlar
IOT hub'ı ölçümleri genel bir bakış gördüğünüze göre Azure IOT hub'ı yönetme hakkında daha fazla bilgi edinmek için bu bağlantıyı izleyin:

* [İzleme işlemleri][lnk-monitor]

Daha fazla IOT hub'ı özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu][lnk-devguide]
* [Bir aygıt ile Azure IOT kenar benzetimini yapma][lnk-iotedge]

<!-- Links and images -->
[1]: media/iot-hub-metrics/enable-metrics-1.png
[2]: media/iot-hub-metrics/enable-metrics-2.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-operations-monitoring]: iot-hub-operations-monitoring.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
