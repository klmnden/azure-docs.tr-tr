---
title: Azure dijital çiftleri ile cihaz bağlantısı ve telemetri giriş | Microsoft Docs
description: Azure dijital çiftleri ile bir cihaz ekleme genel bakış
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: alinast
ms.openlocfilehash: 61d05a7e9936a0edd17c5528ce4f55233b6e7e0e
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324306"
---
# <a name="device-connectivity-and-telemetry-ingress"></a>Cihaz bağlantısı ve telemetri giriş

Tüm IOT çözümlerinin temel cihazlardan ve sensörlerden tarafından gönderilen telemetri verilerini oluşturur. Bu nedenle, bu farklı kaynakları temsil eden ve bunları bir konuma bağlamında yönetme bir baş IOT uygulama geliştirmede konudur. Azure dijital İkizlerini cihazlardan ve sensörlerden olan bir uzamsal zeka grafik uniting tarafından IOT çözümleri geliştirme sürecini basitleştirir.

Başlamak için bir `IoTHub` ileti göndermek için kök alanı tüm cihazlara izin verme, uzamsal grafiğin kök kaynak oluşturulmalıdır. IOT hub'ı oluşturuldu ve sensörlerden cihazlarla dijital İkizlerini örneğinde kaydedildi sonra cihazları bir dijital İkizlerini hizmete veri göndermeye başlayabilir [Azure IOT cihaz SDK'sı](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks#azure-iot-device-sdks).

Onboarding cihazlar için adım adım kılavuzu bulunabilir [dijital İkizlerini yapılandırmak ve dağıtmak için öğreticiyi](tutorial-facilities-setup.md). Bir bakışta adımlar şunlardır:

- Azure dijital İkizlerini örneğini dağıtma [Azure portalı](https://portal.azure.com)
- Graftaki alanları oluşturma
- Oluşturma bir `IoTHub` kaynak ve grafınızı boşluk atayın
- Cihazlardan ve sensörlerden graftaki oluşturun ve bunları Yukarıdaki adımlarda alanları atama
- Telemetri iletilerini koşullara göre filtrelemek için bir Eşleştiricisi oluşturma
- Oluşturma bir [ **kullanıcı tanımlı işlev** ](concepts-user-defined-functions.md) ve bir alan grafiğinde, telemetri iletilerini, özel işlenmesi atayın
- Graf verilerine erişmek kullanıcı tanımlı işlev izin vermek için rol atama
- Dijital İkizlerini yönetim API'lerinden IOT Hub cihaz bağlantı dizesini alın
- Azure IOT cihaz SDK'sı ile bir cihazda cihaz bağlantı dizesini yapılandırma

Aşağıda, dijital İkizlerini yönetim API'SİNDEN IOT Hub cihaz bağlantı dizesi alma ve telemetri algılayıcı tabanlı göndermek için IOT hub'a telemetri ileti biçimini kullanmayı öğreneceksiniz. Her parça telemetri algılayıcı verileri emin olmak için uzamsal grafik içinde ile ilişkilendirilecek aldığı işlenir ve uygun uzamsal bağlam içinde yönlendirilen dijital çiftleri gerektirir.

## <a name="get-the-iot-hub-device-connection-string-from-the-management-api"></a>Yönetim API'SİNDEN IOT Hub cihaz bağlantı dizesini alın

Cihaz API'si ile temel bir GET çağrısı yapmak bir `includes=ConnectionString` parametresini kullanarak IOT Hub cihaz bağlantı dizesini alın. Belirli bir cihaz bulmak için cihaz GUID veya donanım Kimliğine göre filtreleyebilirsiniz:

```plaintext
https://yourManagementApiUrl/api/v1.0/devices/yourDeviceGuid?includes=ConnectionString
```

```plaintext
https://yourManagementApiUrl/api/v1.0/devices?hardwareIds=yourDeviceHardwareId&includes=ConnectionString
```

| Özel öznitelik adı | Değiştirin |
| --- | --- |
| `yourManagementApiUrl` | Yönetim API'niz için tam URL yolu |
| `yourDeviceGuid` | Cihaz kimliği |
| `yourDeviceHardwareId` | Cihaz donanım kimliği |

Cihazın yanıt yükünde kopyalama `connectionString` Azure dijital çiftleri için veri göndermek için Azure IOT cihaz SDK'sı çağrılırken kullanacağınız özelliği.

## <a name="device-to-cloud-message"></a>Cihaz bulut iletisi

Cihazınızın ileti biçimi ve yükü çözümünüzün gereksinimlerine uyacak şekilde özelleştirebilirsiniz. Bir bayt dizisi ya da tarafından desteklenen akış içine seri hale getirilebilir herhangi bir veri anlaşması kullanacağınız [Azure IOT cihaz istemci ileti sınıfı, ileti (bayt [] byteArray)](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.message.-ctor?view=azure-dotnet#Microsoft_Azure_Devices_Client_Message__ctor_System_Byte___). Veri anlaşması karşılık gelen bir kullanıcı tanımlı işlev kodunu çözme sürece ileti tercih ettiğiniz özel bir ikili biçimi olabilir. Bir CİHAZDAN buluta ileti için tek gereksinim, ileti işleme Altyapısı'na uygun şekilde yönlendirilmesini sağlamak amacıyla bir dizi özellikler (aşağıya bakın) korumaktır.

### <a name="telemetry-properties"></a>Telemetri özellikleri

While yükü içeriğini bir `Message` rastgele verilerin yedekleme boyutu 256 KB'den az sayıda gereksinim vardır üzerinde beklenen [Message.Properties](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.message.properties?view=azure-dotnet). Aşağıda özetlenen adımlar, sistem tarafından desteklenen gerekli ve isteğe bağlı özellikleri yansıtır:

| Özellik Adı | Değer | Gerekli | Açıklama |
|---|---|---|---|
| DigitalTwins Telemetri | 1.0 | evet | Bir ileti sistemi tanımlayan bir sabit değer |
| DigitalTwins SensorHardwareId | `string(72)` | evet | Algılayıcı gönderme benzersiz bir tanımlayıcı `Message`. Bu değer, bir nesnenin eşleşmelidir `HardwareId` sistemin işlemek özellik. Örneğin, `00FF0643BE88-CO2` |
| CreationTimeUtc | `string` | hayır | Bir [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) biçimlendirilmiş tarih dize örnekleme süresi akıştaki tanımlama. Örneğin, `2018-09-20T07:35:00.8587882-07:00` |
| CorrelationId | `string` | hayır | A `uuid` kullanılabilen izleme olayları için sistem. Örneğin, `cec16751-ab27-405d-8fe6-c68e1412ce1f`

### <a name="sending-your-message-to-digital-twins"></a>Dijital çiftleri için ileti gönderme

DeviceClient kullanın [SendEventAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.sendeventasync?view=azure-dotnet) veya [SendEventBatchAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.sendeventbatchasync?view=azure-dotnet) dijital İkizlerini hizmetinize iletinizi göndermek için arama.

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital İkizlerini veri işleme ve kullanıcı tanımlı işlevler özellikleri hakkında bilgi edinmek için [Azure dijital İkizlerini veri işleme ve kullanıcı tanımlı işlevleri](concepts-user-defined-functions.md).

