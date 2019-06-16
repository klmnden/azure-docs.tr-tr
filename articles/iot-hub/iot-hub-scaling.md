---
title: Azure IOT Hub ile ölçeklendirme | Microsoft Docs
description: Beklenen bir ileti aktarım hızı ve istenen özellikleri desteklemek için IOT hub'ını ölçeklendirmek nasıl. Her katman için desteklenen üretilen iş ve parçalara ayırma için seçenekleri bir özetini içerir.
author: wesmc7777
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/02/2018
ms.author: wesmc
ms.openlocfilehash: 49e0db690818e67f96f5bcefa4f581b1db6da451
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64697328"
---
# <a name="choose-the-right-iot-hub-tier-for-your-solution"></a>Çözümünüz için doğru IOT Hub katmanını seçme

Her IOT çözümü farklı olduğundan Azure IOT Hub fiyatlandırma ve ölçek göre çeşitli seçenekler sunar. Bu makalede, IOT hub'ı gereksinimlerinizi değerlendirmenize yardımcı olmak için tasarlanmıştır. Fiyatlandırma IOT Hub katmanları hakkında daha fazla bilgi için bkz [IOT Hub fiyatlandırması](https://azure.microsoft.com/pricing/details/iot-hub).

Hangi IOT Hub katmanını çözümünüz için doğru olduğuna karar vermek için iki soruları kendinize sorun:

**Kullanmak hangi özelliklerin planlıyor musunuz?**

Azure IOT hub'ı destekledikleri özellikler sayısında farklı iki katmanı, temel ve standart, sunar. IOT çözümünüzü cihazlarından veri toplamak ve merkezi olarak analiz etme etrafında alıyorsa, temel katman için büyük olasılıkla uygun. IOT cihazları uzaktan denetleme veya bazı iş yüklerinizi aygıtlara dağıtmak için daha gelişmiş yapılandırmaları kullanmak istiyorsanız, standart katman, düşünmelisiniz. Hangi özelliklerin dahil her katmanında ayrıntılı bir dökümü için devam [temel ve standart katmanları](#basic-and-standard-tiers).

**Günlük taşımak ne kadar veri planlıyor musunuz?**

Her IOT Hub katmanını göre üç boyutlarında kullanılabilir ne kadar veri işleme geçici bir çözüm içinde belirli bir günde başa çıkabilir. Bu boyutları, sayısal olarak 1, 2 ve 3 tanımlanır. Örneğin, bir düzey 3 birim, 300 milyon işleyebilir sırasında her bir birimi bir düzey 1 IOT hub'ı, günde 400 bin iletileri işleyebilir. Veri kılavuzları hakkında daha fazla ayrıntı için devam [ileti işleme hızı](#message-throughput).

## <a name="basic-and-standard-tiers"></a>Temel ve standart katmanları

Standart katman IOT Hub'ın tüm özelliklerini etkinleştirir ve devre dışı hale getirmek istediğiniz tüm IOT çözümleri için gereklidir çift yönlü iletişim yeteneklerini kullanın. Temel katman özelliklerinin bir alt kümesi sağlar ve buluta cihazlardan gelen tek yönlü iletişimi yalnızca ihtiyacınız olan IOT çözümleri için tasarlanmıştır. Her iki katmanda aynı güvenlik ve kimlik doğrulama özellikleri sunar.

Yalnızca bir tür [edition](https://azure.microsoft.com/pricing/details/iot-hub/) IOT hub'ı bir katman içinde seçilebilir. Örneğin, birden çok S1 birimi olan, ancak bir karışımını birimleri S1 ve B3 ya da S1 ve S2 gibi farklı sürümleri ile değil, bir IOT hub'ı oluşturabilirsiniz.

| Özellik | Temel katman | Ücretsiz/standart katmanı |
| ---------- | ---------- | ------------- |
| [CİHAZDAN buluta telemetri](iot-hub-devguide-messaging.md) | Evet | Evet |
| [Cihaz kimliği başına](iot-hub-devguide-identity-registry.md) | Evet | Evet |
| [İleti yönlendirme](iot-hub-devguide-messages-read-custom.md) ve [Event Grid tümleştirmesi](iot-hub-event-grid.md) | Evet | Evet |
| [HTTP, AMQP, MQTT protokolleri](iot-hub-devguide-protocols.md) | Evet | Evet |
| [Cihaz sağlama hizmeti](../iot-dps/about-iot-dps.md) | Evet | Evet |
| [İzleme ve tanılama](iot-hub-monitor-resource-health.md) | Evet | Evet |
| [Bulut-cihaz Mesajlaşma](iot-hub-devguide-c2d-guidance.md) |   | Evet |
| [Cihaz ikizlerini](iot-hub-devguide-device-twins.md), [modül ikizlerini](iot-hub-devguide-module-twins.md), ve [cihaz Yönetimi](iot-hub-device-management-overview.md) |   | Evet |
| [Cihaz akışları (Önizleme)](iot-hub-device-streams-overview.md) |   | Evet |
| [Azure IoT Edge](../iot-edge/about-iot-edge.md) |   | Evet |

IOT Hub ayrıca test ve değerlendirme için tasarlanmıştır ücretsiz bir katmanı sunar. Bu, standart katman, ancak sınırlı Mesajlaşma kesintileri tüm özelliklerine sahiptir. Ücretsiz katmanındaki temel veya standart olarak yükseltemezsiniz.

## <a name="partitions"></a>Bölümler

Azure IOT hub'ları içeren birçok temel bileşenleri [Azure Event Hubs](../event-hubs/event-hubs-features.md)de dahil olmak üzere [bölümler](../event-hubs/event-hubs-features.md#partitions). IOT hub'ları için olay akışları, genellikle çeşitli IOT cihazlar tarafından bildirilen gelen telemetri verilerini ile doldurulur. Bölümleme olay akışını aynı anda okuma ve olay akışlara yazmak oluşan çakışmaları azaltmak için kullanılır.

IOT hub'ı oluşturulduğunda ve değiştirilemez ' ün bölüm sınırından seçilir. Temel katman IOT Hub ve IOT hub'ı standart katman için en yüksek bölüm sınırı 32'dir. Çoğu IOT hub'ları yalnızca 4 bölüm gerekir. Event Hubs SSS Sayfasındaki bölümleri belirleme hakkında daha fazla bilgi için bkz. [kaç bölümler yapmam gerekir mi?](../event-hubs/event-hubs-faq.md#how-many-partitions-do-i-need)

## <a name="tier-upgrade"></a>Katmanı yükseltme

IOT hub'ınızı oluşturduğunuzda, mevcut işlemleri kesintiye uğratmadan Temel katmandan standart katmana yükseltebilirsiniz. Daha fazla bilgi için [IOT hub'ınıza yükseltme](iot-hub-upgrade.md).

Birim yapılandırması, Temel katmandan standart katmana geçiş yaptığınızda değişmeden kalır.

## <a name="iot-hub-rest-apis"></a>IoT Hub REST API’leri

Desteklenen yeteneklerin IOT Hub'ın temel ve standart katmanları arasındaki farkı, bazı API çağrıları, temel katmanı hub'ları ile çalışmaz anlamına gelir. Aşağıdaki tabloda, hangi API'ler kullanılabilir olduğunu gösterir:

| API | Temel katman | Ücretsiz/standart katmanı |
| --- | ---------- | ------------- |
| [Cihaz silme](https://docs.microsoft.com/rest/api/iothub/service/deletedevice) | Evet | Evet |
| [Aygıt alma](https://docs.microsoft.com/rest/api/iothub/service/getdevice) | Evet | Evet |
| Modülü Sil | Evet | Evet |
| Modülü Al | Evet | Evet |
| [Kayıt defteri istatistikleri alma](https://docs.microsoft.com/rest/api/iothub/service/getdeviceregistrystatistics) | Evet | Evet |
| [Hizmet istatistikleri alma](https://docs.microsoft.com/rest/api/iothub/service/getservicestatistics) | Evet | Evet |
| [Cihaz güncelle](https://docs.microsoft.com/rest/api/iothub/service/createorupdatedevice) | Evet | Evet |
| PUT Modülü | Evet | Evet |
| [IOT hub'ı sorgulama](https://docs.microsoft.com/rest/api/iothub/service/queryiothub) | Evet | Evet |
| Sorgu modülleri | Evet | Evet |
| [Karşıya dosya yükleme SAS URI'si oluşturma](https://docs.microsoft.com/rest/api/iothub/device/createfileuploadsasuri) | Evet | Evet |
| [Bağlı cihaz bildirim alma](https://docs.microsoft.com/rest/api/iothub/device/receivedeviceboundnotification) | Evet | Evet |
| [Cihaz olayı Gönder](https://docs.microsoft.com/rest/api/iothub/device/senddeviceevent) | Evet | Evet |
| Modül olayı Gönder | Evet | Evet |
| [Dosya karşıya yükleme durumu güncelleştirme](https://docs.microsoft.com/rest/api/iothub/device/updatefileuploadstatus) | Evet | Evet |
| [Toplu cihaz işlemi](/rest/api/iot-dps/runbulkenrollmentgroupoperation/runbulkenrollmentgroupoperation) | Evet, IOT Edge özellikleri dışında | Evet | 
| [Komut kuyruğu Temizle](https://docs.microsoft.com/rest/api/iothub/service/purgecommandqueue) |   | Evet |
| [Cihaz ikizi Al](https://docs.microsoft.com/rest/api/iothub/service/gettwin) |   | Evet |
| Modül ikizi Al |   | Evet |
| [Cihaz yöntemi çağırma](https://docs.microsoft.com/rest/api/iothub/service/invokedevicemethod) |   | Evet |
| [Cihaz ikizi güncelleştir](https://docs.microsoft.com/rest/api/iothub/service/updatetwin) |   | Evet | 
| Modül ikizi güncelleştir |   | Evet | 
| [Bağlı cihaz bildirim abandon](https://docs.microsoft.com/rest/api/iothub/device/abandondeviceboundnotification) |   | Evet |
| [Tam cihaz bildirim bağlı](https://docs.microsoft.com/rest/api/iothub/device/completedeviceboundnotification) |   | Evet |
| [İşi iptal et](https://docs.microsoft.com/rest/api/iothub/service/canceljob) |   | Evet |
| [İş oluşturma](https://docs.microsoft.com/rest/api/iothub/service/createjob) |   | Evet |
| [İşi Al](https://docs.microsoft.com/rest/api/iothub/service/getjob) |   | Evet |
| [Sorgu işleri](https://docs.microsoft.com/rest/api/iothub/service/queryjobs) |   | Evet |

## <a name="message-throughput"></a>İleti işleme hızı

Bir IOT hub'ı çözüm boyutu için en iyi birim başına temelinde trafiği değerlendirilecek yoludur. Özellikle, aşağıdaki kategorilerde işlemlerinin gerekli en yüksek aktarım hızını göz önünde bulundurun:

* Cihazdan buluta iletiler
* Bulut-cihaz iletilerini
* Kimlik kayıt defteri işlemleri

Trafik, hub başına bir birim başına temelinde ölçülür. Düzey 1 veya 2 IOT Hub örneği kadar 200 birimleri ile ilişkili olabilir. Düzey 3 IOT Hub örneği 10 birime kadar olabilir. IOT hub'ınıza oluşturduktan sonra birim sayısını değiştirmek ya da, mevcut işlemleri kesintiye uğratmadan 1, 2 ve 3 boyutu içinde belirli bir katman arasında taşıyın. Daha fazla bilgi için [IOT Hub'ınıza yükseltme](iot-hub-upgrade.md).

Her katmanın trafik özellikleri örnek olarak, cihaz bulut iletilerini aşağıdaki aralıksız üretilen yönergeleri izleyin:

| Katman | Kesintisiz aktarım hızı | Sürdürülen gönderme oranı |
| --- | --- | --- |
| B1, S1 |Birim başına 1111 KB/dakika kadar<br/>(1.5 GB/gün/birim) |Birim başına 278 iletileri/dakika ortalama<br/>(400.000 ileti/gün birim başına) |
| S2 B2 |Birim başına 16 MB/dakika kadar<br/>(22.8 GB/gün/birim) |Birim başına 4,167 iletileri/dakika ortalama<br/>(6 milyon ileti/gün birim başına) |
| B3, S3 |Birim başına 814 MB/dakika kadar<br/>(1144.4 GB/gün/birim) |Birim başına 208,333 iletileri/dakika ortalama<br/>(300 milyon ileti/gün birim başına) |

Bu aktarım hızı yanı sıra bilgi [IOT Hub kotaları ve kısıtlamaları](iot-hub-devguide-quotas-throttling.md) ve çözümünüzün uygun şekilde tasarlayın.

### <a name="identity-registry-operation-throughput"></a>Kimlik kayıt defteri işlemi aktarım hızı

Cihaz sağlama için çoğunlukla ilişkili oldukları gibi IOT Hub kimlik kayıt defteri işlemlerini çalıştırma işlemleri olması gereken değil.

Belirli veri bloğu performans rakamlarına ulaşmak için bkz. [IOT Hub kotaları ve kısıtlamaları](iot-hub-devguide-quotas-throttling.md).

## <a name="auto-scale"></a>Otomatik Ölçeklendirme

IOT Hub'ınızda izin verilen ileti sınırına yaklaşılıyor, bunları kullanabilirsiniz [adımları otomatik olarak ölçeklendirmek için](https://azure.microsoft.com/resources/samples/iot-hub-dotnet-autoscale/) aynı IOT hub'ı katmanında bir IOT Hub birimi artırmak için.

## <a name="sharding"></a>Parçalama

Bazen tek bir IOT hub, milyonlarca cihaza ölçeklendirebilirsiniz olmakla birlikte, çözümünüzü tek bir IOT hub'a garanti edemez belirli performans özelliklerini gerektirir. Bu durumda, birden çok IOT hub'ları arasında cihazlarınızı bölümleyebilirsiniz. IOT hub'ları birden çok trafik artışlarıyla başa kesintisiz ve gerekli olan işlem hızları ve gerekli aktarım hızı elde.

## <a name="next-steps"></a>Sonraki adımlar

* IOT hub'ı özellikleri ve performans ayrıntıları hakkında daha fazla bilgi için bkz. [IOT Hub fiyatlandırması](https://azure.microsoft.com/pricing/details/iot-hub) veya [IOT Hub kotaları ve kısıtlamaları](iot-hub-devguide-quotas-throttling.md).

* IOT Hub katmanını değiştirmek için adımları izleyin. [IOT hub'ınıza yükseltme](iot-hub-upgrade.md).
