---
title: Azure IOT Hub ile ölçeklendirme | Microsoft Docs
description: Beklenen bir ileti aktarım hızı ve istenen özellikleri desteklemek için IOT hub'ını ölçeklendirmek nasıl. Her katman için desteklenen üretilen iş ve parçalara ayırma için seçenekleri bir özetini içerir.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/02/2018
ms.author: kgremban
ms.openlocfilehash: b4c5bf3b11c2ee661d95dc50f5c93e12fe2d56bf
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37901050"
---
# <a name="choose-the-right-iot-hub-tier-for-your-solution"></a>Çözümünüz için doğru IOT Hub katmanını seçme

Her IOT çözümü farklı olduğundan Azure IOT Hub fiyatlandırma ve ölçek göre çeşitli seçenekler sunar. Bu makalede, IOT hub'ı gereksinimlerinizi değerlendirmenize yardımcı olmak için tasarlanmıştır. IOT Hub katmanları hakkında bilgi fiyatlandırma için başvurun [IOT Hub fiyatlandırması](https://azure.microsoft.com/pricing/details/iot-hub). 

Hangi IOT Hub katmanını çözümünüz için doğru olduğuna karar vermek için iki soruları kendinize sorun:

**Kullanmak hangi özelliklerin planlıyor musunuz?**
Azure IOT hub'ı destekledikleri özellikler sayısında farklı iki katmanı, temel ve standart, sunar. IOT çözümünüzü cihazlarından veri toplamak ve merkezi olarak analiz etme etrafında alıyorsa temel katman için büyük olasılıkla uygun. IOT cihazları uzaktan denetleme veya bazı iş yüklerinizi aygıtlara dağıtmak için daha gelişmiş yapılandırmaları kullanmak istiyorsanız, standart katman düşünmelisiniz. Hangi özelliklerin dahil her katmanında ayrıntılı bir dökümü için devam [temel ve standart katmanları](#basic-and-standard-tiers).

**Günlük taşımak ne kadar veri planlıyor musunuz?**
Her IOT Hub katmanını göre üç boyutlarında kullanılabilir ne kadar veri işleme geçici bir çözüm içinde belirli bir günde başa çıkabilir. Bu boyutları, sayısal olarak 1, 2 ve 3 tanımlanır. Örneğin, bir düzey 3 birim, 300 milyon işleyebilir sırasında her bir birimi bir düzey 1 IOT hub'ı, günde 400 bin iletileri işleyebilir. Veri kılavuzları hakkında daha fazla ayrıntı için devam [ileti işleme hızı](#message-throughput).

## <a name="basic-and-standard-tiers"></a>Temel ve standart katmanları

Standart katman IOT Hub'ın tüm özelliklerini etkinleştirir ve devre dışı hale getirmek istediğiniz tüm IOT çözümleri için gereklidir çift yönlü iletişim yeteneklerini kullanın. Temel katman özelliklerinin bir alt kümesi sağlar ve buluta cihazlardan gelen tek yönlü iletişimi yalnızca ihtiyacınız olan IOT çözümleri için tasarlanmıştır. Her iki katmanda aynı güvenlik ve kimlik doğrulama özellikleri sunar.

IOT hub'ınızı oluşturduğunuzda, mevcut işlemleri kesintiye uğratmadan Temel katmandan standart katmana yükseltebilirsiniz. Daha fazla bilgi için [IOT hub'ınıza yükseltme](iot-hub-upgrade.md). IOT hub'ı bölüm sınırından temel katmanı Not 8'dir. Bu sınırı kaldığından Temel katmandan standart katmana geçiş yaptığınızda.

| Özellik | Temel katman | Standart katman |
| ---------- | ---------- | ------------- |
| [CİHAZDAN buluta telemetri](iot-hub-devguide-messaging.md) | Evet | Evet |
| [Cihaz kimliği başına](iot-hub-devguide-identity-registry.md) | Evet | Evet |
| [İleti yönlendirme](iot-hub-devguide-messages-read-custom.md) ve [Event Grid tümleştirmesi](iot-hub-event-grid.md) | Evet | Evet |
| [HTTP, AMQP, MQTT protokolleri](iot-hub-devguide-protocols.md) | Evet | Evet |
| [Cihaz sağlama hizmeti](../iot-dps/about-iot-dps.md) | Evet | Evet |
| [İzleme ve tanılama](iot-hub-monitor-resource-health.md) | Evet | Evet |
| [Bulut-cihaz Mesajlaşma](iot-hub-devguide-c2d-guidance.md) |   | Evet |
| [Cihaz ikizlerini](iot-hub-devguide-device-twins.md), [modül ikizlerini](iot-hub-devguide-module-twins.md) ve [cihaz Yönetimi](iot-hub-device-management-overview.md) |   | Evet |
| [Azure IoT Edge](../iot-edge/how-iot-edge-works.md) |   | Evet |

IOT Hub ayrıca test ve değerlendirme için tasarlanmıştır ücretsiz bir katmanı sunar. Bu, standart katman, ancak sınırlı Mesajlaşma kesintileri tüm özelliklerine sahiptir. Ücretsiz katmanındaki temel veya standart olarak yükseltemezsiniz. 

### <a name="iot-hub-rest-apis"></a>IoT Hub REST API’leri

Desteklenen yeteneklerin IOT Hub'ın temel ve standart katmanları arasındaki farkı, bazı API çağrıları, temel katmanı hub'ları ile çalışmaz anlamına gelir. Aşağıdaki tabloda, hangi API'ler kullanılabilir olduğunu gösterir: 

| API | Temel katman | Standart katman |
| --- | ---------- | ------------- |
| [Cihaz silme](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/deletedevice) | Evet | Evet |
| [Aygıt alma](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/getdevice) | Evet | Evet |
| Modülü Sil | Evet | Evet |
| Modülü Al | Evet | Evet |
| [Kayıt defteri istatistikleri alma](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/getdeviceregistrystatistics) | Evet | Evet |
| [Hizmet istatistikleri alma](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/getservicestatistics) | Evet | Evet |
| [Cihaz yerleştirin](https://docs.microsoft.com/rest/api/iothub/deviceapi/putdevice) | Evet | Evet |
| PUT Modülü | Evet | Evet |
| [Sorgu cihazlar](https://docs.microsoft.com/rest/api/iothub/deviceapi/querydevices) | Evet | Evet |
| Sorgu modülleri | Evet | Evet |
| [Karşıya dosya yükleme SAS URI'si oluşturma](https://docs.microsoft.com/en-us/rest/api/iothub/device/device/createfileuploadsasuri) | Evet | Evet |
| [Bağlı cihaz bildirim alma](https://docs.microsoft.com/en-us/rest/api/iothub/device/device/receivedeviceboundnotification) | Evet | Evet |
| [Cihaz olayı Gönder](https://docs.microsoft.com/en-us/rest/api/iothub/device/device/senddeviceevent) | Evet | Evet |
| Modül olayı Gönder | Evet | Evet |
| [Dosya karşıya yükleme durumu güncelleştirme](https://docs.microsoft.com/en-us/rest/api/iothub/device/device/updatefileuploadstatus) | Evet | Evet |
| [Toplu cihaz işlemi](https://docs.microsoft.com/en-us/rest/api/iot-dps/deviceenrollment/bulkoperation) | Evet, IOT Edge özelliklere dışında | Evet | 
| [Komut kuyruğu Temizle](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/purgecommandqueue) |   | Evet |
| [Cihaz ikizi Al](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/gettwin) |   | Evet |
| Modül ikizi Al |   | Evet |
| [Cihaz yöntemi çağırma](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/invokedevicemethod) |   | Evet |
| [Cihaz ikizi güncelleştir](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/updatetwin) |   | Evet | 
| Modül ikizi güncelleştir |   | Evet | 
| [Bağlı cihaz bildirim abandon](https://docs.microsoft.com/en-us/rest/api/iothub/device/device/abandondeviceboundnotification) |   | Evet |
| [Tam cihaz bildirim bağlı](https://docs.microsoft.com/en-us/rest/api/iothub/device/device/completedeviceboundnotification) |   | Evet |
| [İşi iptal et](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/canceljob) |   | Evet |
| [İş oluşturma](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/createjob) |   | Evet |
| [İşi Al](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/getjob) |   | Evet |
| [Sorgu işleri](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/queryjobs) |   | Evet |

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
| S3 B3 |Birim başına 814 MB/dakika kadar<br/>(1144.4 GB/gün/birim) |Birim başına 208,333 iletileri/dakika ortalama<br/>(300 milyon ileti/gün birim başına) |

Bu aktarım hızı yanı sıra bilgi [IOT Hub kotaları ve kısıtlamaları] [ IoT Hub quotas and throttles] ve çözümünüzün uygun şekilde tasarlayın.

### <a name="identity-registry-operation-throughput"></a>Kimlik kayıt defteri işlemi aktarım hızı
Cihaz sağlama için çoğunlukla ilişkili oldukları gibi IOT Hub kimlik kayıt defteri işlemlerini çalıştırma işlemleri olması gereken değil.

Belirli veri bloğu performans rakamlarına ulaşmak için bkz. [IOT Hub kotaları ve kısıtlamaları][IoT Hub quotas and throttles].

## <a name="sharding"></a>Parçalama
Bazen tek bir IOT hub, milyonlarca cihaza ölçeklendirebilirsiniz olmakla birlikte, çözümünüzü tek bir IOT hub'a garanti edemez belirli performans özelliklerini gerektirir. Bu durumda birden çok IOT hub'ları arasında cihazlarınızı bölümleyebilirsiniz. IOT hub'ları birden çok trafik artışlarıyla başa kesintisiz ve gerekli olan işlem hızları ve gerekli aktarım hızı elde.

## <a name="next-steps"></a>Sonraki adımlar

* IOT hub'ı özellikleri ve performans ayrıntıları hakkında ek bilgi için bkz. [IOT Hub fiyatlandırması] [bağlantı fiyatlandırma] veya [IOT Hub kotaları ve kısıtlamaları][IoT Hub quotas and throttles].
* IOT Hub katmanını değiştirmek için adımları izleyin. [IOT hub'ınıza yükseltme](iot-hub-upgrade.md).

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub quotas and throttles]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
