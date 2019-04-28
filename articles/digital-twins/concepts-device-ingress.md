---
title: Azure dijital çiftleri ile cihaz bağlantısı ve telemetri giriş | Microsoft Docs
description: Bir cihaz ekleme dijital İkizlerini Azure ile nasıl genel bakış
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: alinast
ms.openlocfilehash: 35d12d0114f9677905c85a9df94ecd074e5f8f75
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60926094"
---
# <a name="device-connectivity-and-telemetry-ingress"></a>Cihaz bağlantısı ve telemetri sorunları

Tüm IOT çözümlerinin temel cihazlardan ve sensörlerden tarafından gönderilen telemetri verilerini oluşturur. Bu farklı kaynakları temsil eder ve bir konuma bağlamında yönetmek nasıl baş IOT uygulama geliştirmede edilir. Azure dijital İkizlerini cihazlardan ve sensörlerden olan bir uzamsal zeka grafik uniting tarafından IOT çözümleri geliştirme sürecini basitleştirir.

Başlamak için bir Azure IOT hub'ı kaynak uzamsal graf kökünde oluşturun. IOT hub'ı kaynak ileti göndermek için kök alanı tüm cihazların sağlar. IOT hub'ı oluşturulduktan sonra dijital İkizlerini örneği içinde sensörlerden cihazları kaydedin. Cihazlar bir dijital İkizlerini hizmete veri gönderebilir [Azure IOT cihaz SDK'sını](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks).

Yerleşik cihazlarını getirmesine hakkında adım adım kılavuz için bkz. [dijital İkizlerini yapılandırmak ve dağıtmak için öğreticiyi](tutorial-facilities-setup.md). Bir bakışta adımlar şunlardır:

- Bir dijital İkizlerini örneğinden dağıtma [Azure portalında](https://portal.azure.com).
- Graftaki alanları oluşturun.
- Bir IOT hub'ı kaynağını oluşturun ve grafınızı boşluk atayın.
- Cihazlardan ve sensörlerden graftaki oluşturun ve bunları önceki adımlarda oluşturulan alanları atayabilirsiniz.
- Telemetri iletilerini koşullara göre filtrelemek için bir Eşleştiricisi oluşturun.
- Oluşturma bir [kullanıcı tanımlı işlev](concepts-user-defined-functions.md)ve bir alan grafiğinde, telemetri iletilerini, özel işlenmesi atayın.
- Graf verilerine erişmek kullanıcı tanımlı işlev izin vermek için bir rol atayın.
- IOT Hub cihaz bağlantı dizesini dijital İkizlerini yönetim API'lerinden alın.
- Azure IOT cihaz SDK'sı ile bir cihazda cihaz bağlantı dizesini yapılandırın.

Aşağıdaki bölümlerde, dijital İkizlerini yönetim API'SİNDEN IOT Hub cihaz bağlantı dizesini alma konusunda bilgi edinin. Ayrıca telemetri algılayıcı tabanlı göndermek için IOT hub'a telemetri ileti biçimini kullanmayı öğrenin. Dijital İkizlerini telemetri algılayıcı uzamsal grafik içinde ile ilişkilendirilecek aldığı her bir parçası gerekiyor. Bu gereksinim, veriler işlenir ve uygun uzamsal bağlam içinde yönlendirilen emin olur.

## <a name="get-the-iot-hub-device-connection-string-from-the-management-api"></a>Yönetim API'SİNDEN IOT Hub cihaz bağlantı dizesini alın

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

Cihaz API'si ile temel bir GET çağrısı yapmak bir `includes=ConnectionString` parametresini kullanarak IOT Hub cihaz bağlantı dizesini alın. Cihaz tarafından GUID veya donanım Kimliğini belirli bir cihaz bulmak için filtre.

```plaintext
YOUR_MANAGEMENT_API_URL/devices/YOUR_DEVICE_GUID?includes=ConnectionString
```

| Parametre | Şununla değiştir |
| --- | --- |
| *YOUR_DEVICE_GUID* | Cihaz kimliği |

```plaintext
YOUR_MANAGEMENT_API_URL/devices?hardwareIds=YOUR_DEVICE_HARDWARE_ID&includes=ConnectionString
```

| Parametre değeri | Şununla değiştir |
| --- | --- |
| *YOUR_DEVICE_HARDWARE_ID* | Cihaz donanım kimliği |

Cihazın yanıt yükünde kopyalama **connectionString** özelliği. Azure IOT cihaz SDK'sı için dijital İkizlerini veri göndermek için çağırdığınızda kullanın.

## <a name="device-to-cloud-message"></a>CİHAZDAN buluta ileti

Cihazınızın ileti biçimi ve yükü çözümünüzün gereksinimlerine uyacak şekilde özelleştirebilirsiniz. Bir bayt dizisi ya da tarafından desteklenen akış içine seri hale getirilebilir herhangi bir veri sözleşmesi kullanın [Azure IOT cihaz istemci ileti sınıfı, ileti (bayt [] byteArray)](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.message.-ctor?view=azure-dotnet#Microsoft_Azure_Devices_Client_Message__ctor_System_Byte___). Veri anlaşması karşılık gelen bir kullanıcı tanımlı işlev kodunu çözme sürece ileti tercih ettiğiniz özel bir ikili biçimi olabilir. Bir CİHAZDAN buluta ileti için yalnızca bir gereksinim yoktur. İleti işleme altyapısı için düzgün bir biçimde yönlendirildiğinden emin olmak için özellikler kümesi sürdürmeniz gerekir.

### <a name="telemetry-properties"></a>Telemetri özellikleri

 Yükü içeriğini bir **ileti** rastgele verilerin boyutu 256 KB'a kadar. Özellikleri için beklenen birkaç gereksinim vardır [ `Message.Properties` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.message.properties?view=azure-dotnet) türü. Tablo, sistem tarafından desteklenen gerekli ve isteğe bağlı özellikleri gösterir.

| Özellik adı | Değer | Gerekli | Açıklama |
|---|---|---|---|
| **DigitalTwins Telemetri** | 1.0 | Evet | Bir ileti sistemi tanımlayan bir sabit değer. |
| **DigitalTwins SensorHardwareId** | `string(72)` | Evet | Benzersiz bir tanımlayıcı gönderen algılayıcı **ileti**. Bu değer, bir nesnenin eşleşmelidir **HardwareId** sistemin işlemek özellik. Örneğin, `00FF0643BE88-CO2`. |
| **CreationTimeUtc** | `string` | Hayır | Bir [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) akıştaki örnekleme zamanı belirten tarih dizesi olarak biçimlendirilmiş. Örneğin, `2018-09-20T07:35:00.8587882-07:00`. |
| **Bağıntı Kimliği** | `string` | Hayır | Sistem izleme olayları için kullanılan UUID'si. Örneğin, `cec16751-ab27-405d-8fe6-c68e1412ce1f`.

### <a name="send-your-message-to-digital-twins"></a>Dijital çiftleri için gönderin

DeviceClient kullanın [SendEventAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.sendeventasync?view=azure-dotnet) veya [SendEventBatchAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.sendeventbatchasync?view=azure-dotnet) iletinizi göndermek için dijital çiftleri için çağrı.

## <a name="next-steps"></a>Sonraki adımlar

- Azure dijital İkizlerini veri işleme ve kullanıcı tanımlı işlevleri özellikleri hakkında bilgi edinmek için [Azure dijital İkizlerini veri işleme ve kullanıcı tanımlı işlevleri](concepts-user-defined-functions.md).
