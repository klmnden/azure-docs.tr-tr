---
title: Azure IOT Hub ile ölçeklendirme | Microsoft Docs
description: Beklenen ileti işleme ve istenen özelliklerini desteklemek için IOT hub'ını ölçeklendirmek nasıl. Her katman için desteklenen işleme ve parçalama seçeneklerini özetini içerir.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/02/2018
ms.author: kgremban
ms.openlocfilehash: b7751bd1b309333d5ef40530b0fa499a42a57cd1
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36752257"
---
# <a name="choose-the-right-iot-hub-tier-for-your-solution"></a>Çözümünüz için doğru IOT hub'ı katmanı seçin

Her IOT çözüm farklı olduğundan Azure IOT Hub fiyatlandırma ve ölçek göre çeşitli seçenekler sağlar. Bu makalede, IOT hub'ı gereksinimlerinizi değerlendirmenize yardımcı olmak için tasarlanmıştır. IOT hub'ı katmanları hakkında bilgi fiyatlandırma için başvurmak [IOT Hub'ın fiyatlandırma](https://azure.microsoft.com/pricing/details/iot-hub). 

Hangi IOT hub'ı katmanı çözümünüz için uygun olduğuna karar vermek için iki soruları kendinize sorun:

**Kullanmak hangi özelliklerin planlıyor musunuz?**
Azure IOT Hub sayısını destekledikleri özellikleri farklı iki katmanı, temel ve standart, sunar. IOT çözümünüzü cihazlardan veri toplama ve merkezi olarak çözümlemenin geçici dayanıyorsa temel katmana sizin için büyük olasılıkla uygun olan. IOT cihazları uzaktan denetleme veya bazı aygıtlar, iş yükünü dağıtmak için daha gelişmiş yapılandırmaları kullanmak istiyorsanız, standart katmanı dikkate almalısınız. Hangi dahil edilen özelliklerini her katmanında ayrıntılı bir çözümleme için devam [temel ve standart katmanları](#basic-and-standard-tiers).

**Günlük taşımak ne kadar veri planlıyor musunuz?**
Her IOT hub'ı katmanı tabanlı üç boyutlarında kullanılabilir ne kadar veri işleme herhangi belirli bir gün içinde ele alabilir. Bu boyutlar sayısal 1, 2 ve 3 tanımlanır. Örneğin, bir düzey 3 birim 300 milyon işleyebileceği sırasında bir düzey 1 IOT hub'ın her birimi günde, 400 bin iletileri işleyebilir. Veri kılavuzları hakkında daha fazla ayrıntı için devam [ileti işleme](#message-throughput).

## <a name="basic-and-standard-tiers"></a>Temel ve standart katmanları

Standart katmanı IOT Hub'ın tüm özelliklerini etkinleştirir ve hale getirmek istediğiniz tüm IOT çözümleri için gerekli çift yönlü iletişim özelliklerini kullanın. Temel katman özelliklerinin bir alt kümesi sağlar ve buluta tek yönlü iletişim aygıtlardan yeterlidir IOT çözümleri için tasarlanmıştır. Her iki katmanda aynı güvenlik ve kimlik doğrulama özellikleri sunar.

IOT hub'ınızı oluşturduğunuzda mevcut işlemlerinizin kesintiye uğratmadan temel katmanından standart katmanına yükseltme yapabilirsiniz. Daha fazla bilgi için bkz: [IOT hub'ınızı yükseltme](iot-hub-upgrade.md).

| Özellik | Temel katman | Standart katman |
| ---------- | ---------- | ------------- |
| [Cihaz bulut telemetri](iot-hub-devguide-messaging.md) | Evet | Evet |
| [Cihaz başına kimliği](iot-hub-devguide-identity-registry.md) | Evet | Evet |
| [İleti yönlendirme](iot-hub-devguide-messages-read-custom.md) ve [olay kılavuz tümleştirme](iot-hub-event-grid.md) | Evet | Evet |
| [HTTP, AMQP ve MQTT protokolleri](iot-hub-devguide-protocols.md) | Evet | Evet |
| [Cihaz sağlama hizmeti](../iot-dps/about-iot-dps.md) | Evet | Evet |
| [İzleme ve tanılama](iot-hub-monitor-resource-health.md) | Evet | Evet |
| [Bulut cihaz Mesajlaşma](iot-hub-devguide-c2d-guidance.md) |   | Evet |
| [Cihaz çiftlerini](iot-hub-devguide-device-twins.md), [modülü çiftlerini](iot-hub-devguide-module-twins.md) ve [Aygıt Yönetimi](iot-hub-device-management-overview.md) |   | Evet |
| [Azure IoT Edge](../iot-edge/how-iot-edge-works.md) |   | Evet |

IOT hub'ı aynı zamanda sınama ve değerlendirme için tasarlanmıştır ücretsiz bir katman sağlar. Standart katmanı, ancak sınırlı Mesajlaşma kesintileri tüm özelliklerine sahiptir. Temel veya standart için ücretsiz katmanından yükseltemezsiniz. 

### <a name="iot-hub-rest-apis"></a>IoT Hub REST API’leri

Desteklenen yeteneklerin IOT Hub'ın temel ve standart katmanları arasındaki farkı bazı API çağrıları temel katmana hub'larıyla çalışmıyor anlamına gelir. Aşağıdaki tabloda, hangi API'leri kullanılabilir olduğunu gösterir: 

| API | Temel katman | Standart katman |
| --- | ---------- | ------------- |
| [Aygıtı silme](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/deletedevice) | Evet | Evet |
| [Cihaz Al](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/getdevice) | Evet | Evet |
| Modül Sil | Evet | Evet |
| Modülü Al | Evet | Evet |
| [Kayıt defteri istatistiklerini alın](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/getdeviceregistrystatistics) | Evet | Evet |
| [Hizmetleri istatistiklerini alın](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/getservicestatistics) | Evet | Evet |
| [PUT cihaz](https://docs.microsoft.com/rest/api/iothub/deviceapi/putdevice) | Evet | Evet |
| PUT Modülü | Evet | Evet |
| [Sorgu cihazları](https://docs.microsoft.com/rest/api/iothub/deviceapi/querydevices) | Evet | Evet |
| Sorgu modülleri | Evet | Evet |
| [Karşıya dosya yükleme SAS URI'sini oluşturma](https://docs.microsoft.com/en-us/rest/api/iothub/device/device/createfileuploadsasuri) | Evet | Evet |
| [Bağlı cihaz bildirim alma](https://docs.microsoft.com/en-us/rest/api/iothub/device/device/receivedeviceboundnotification) | Evet | Evet |
| [Aygıt olay gönderin](https://docs.microsoft.com/en-us/rest/api/iothub/device/device/senddeviceevent) | Evet | Evet |
| Modül olay gönderin | Evet | Evet |
| [Dosya karşıya yükleme durumunu güncelleştir](https://docs.microsoft.com/en-us/rest/api/iothub/device/device/updatefileuploadstatus) | Evet | Evet |
| [Toplu aygıt işlemi](https://docs.microsoft.com/en-us/rest/api/iot-dps/deviceenrollment/bulkoperation) | Evet, IOT kenar özellikleri dışında | Evet | 
| [Komut Sıra Temizleme](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/purgecommandqueue) |   | Evet |
| [Cihaz çifti Al](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/gettwin) |   | Evet |
| Modül twin Al |   | Evet |
| [Cihaz yöntemi çağırma](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/invokedevicemethod) |   | Evet |
| [Cihaz çifti güncelleştir](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/updatetwin) |   | Evet | 
| Güncelleştirme modülü twin |   | Evet | 
| [Bağlı aygıt bildirimi bırakın](https://docs.microsoft.com/en-us/rest/api/iothub/device/device/abandondeviceboundnotification) |   | Evet |
| [Bildirim tam cihaz bağlı](https://docs.microsoft.com/en-us/rest/api/iothub/device/device/completedeviceboundnotification) |   | Evet |
| [İşi iptal et](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/canceljob) |   | Evet |
| [Proje oluşturma](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/createjob) |   | Evet |
| [İşini alın](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/getjob) |   | Evet |
| [Sorgu işleri](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/queryjobs) |   | Evet |

## <a name="message-throughput"></a>İleti işleme

IOT hub'ı çözümünü boyutu için en iyi birim başına temelinde trafiği değerlendirmek için yoludur. Özellikle, gerekli en yüksek verimlilik işlemlerinin Aşağıdaki kategorilerde göz önünde bulundurun:

* Cihazdan buluta iletiler
* Bulut-cihaz iletilerini
* Kimlik kayıt defteri işlemleri

Trafik hub başına değil, birim başına temelinde ölçülür. Düzey 1 veya 2 IOT Hub örneği, kendisiyle ilişkili olarak en fazla 200 birimleri olabilir. Düzey 3 IOT Hub örneği en fazla 10 birim olabilir. IOT hub'ınızı oluşturduğunuzda birim sayısını değiştirin veya varolan işlemlerinizin kesintiye uğratmadan 1, 2 ve 3 boyutları belirli bir katman içinde arasında taşıyın. Daha fazla bilgi için bkz: [IOT Hub'ınızı yükseltme](iot-hub-upgrade.md).

Her katmanın trafiği yeteneklerini örnek olarak, cihaz bulut iletilerini aşağıdaki aralıksız üretilen yönergeleri izleyin:

| Katman | Aralıksız üretilen | Sürdürülen gönderme oranı |
| --- | --- | --- |
| B1, S1 |Birim başına 1111 KB/dakika kadar<br/>(1,5 GB/gün/birim) |278 iletileri dakikada birim başına ortalama<br/>(400.000 iletileri/gün birim başına) |
| B2, S2 |Birim başına 16 MB/dakika kadar<br/>(22.8 GB/gün/birim) |4,167 iletileri dakikada birim başına ortalama<br/>(6 milyon iletileri/gün birim başına) |
| B3 S3 |Birim başına 814 MB/dakika kadar<br/>(1144.4 GB/gün/birim) |208,333 iletileri dakikada birim başına ortalama<br/>(300 milyon iletileri/gün birim başına) |

Bu işleme ek bilgi [IOT hub'ı kotaları ve kısıtlamaları] [ IoT Hub quotas and throttles] ve çözümünüzün uygun şekilde tasarlayın.

### <a name="identity-registry-operation-throughput"></a>Kimlik kayıt defteri işlemi işleme
Cihaz sağlamak için çoğunlukla ilişkili oldukları gibi IOT Hub kimlik kayıt defteri işlemlerini çalıştırma işlemleri olması gereken değil.

Belirli veri bloğu performans numaraları için bkz: [IOT hub'ı kotaları ve kısıtlamaları][IoT Hub quotas and throttles].

## <a name="sharding"></a>Parçalama
Bazen tek bir IOT hub, milyonlarca cihaza için ölçeklendirebilirsiniz olmakla birlikte, çözümünüzü tek bir IOT hub garanti edemez özel performans özellikleri gerektirir. Bu durumda birden çok IOT hub'ları aygıtlarınızı bölüm. Birden çok IOT hub'ları trafiği WINS'e kesintisiz ve gerekli işlem hızları ve gerekli verimlilik elde edin.

## <a name="next-steps"></a>Sonraki adımlar

* IOT hub'ı yetenekleri ve performans ayrıntıları hakkında ek bilgi için bkz: [IOT fiyatlandırma Hub] [bağlantı fiyatlandırması] veya [IOT hub'ı kotaları ve kısıtlamaları][IoT Hub quotas and throttles].
* IOT hub'ı katmanını değiştirmek için adımları [IOT hub'ınızı yükseltme](iot-hub-upgrade.md).

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub quotas and throttles]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
