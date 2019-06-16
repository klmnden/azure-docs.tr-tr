---
title: Cihaz şema Uzaktan izleme çözümünde - Azure | Microsoft Docs
description: Bu makalede Uzaktan izleme çözümünde bir simülasyon cihazı tanımlayan bir JSON şeması.
author: dominicbetts
manager: philmea
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 12/18/2018
ms.topic: conceptual
ms.openlocfilehash: 0f9669d491648ecc621aab27d0908dcc3dc84438
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65823339"
---
# <a name="understand-the-device-model-schema"></a>Cihaz modeli şemasını anlama

Sanal cihazlar, Uzaktan izleme çözümünde davranışını test etmek için kullanabilirsiniz. Uzaktan izleme çözümü, sanal cihazlar çalıştırmak için bir cihaz benzetimi hizmeti içerir. Uzaktan izleme çözümünü dağıttığınızda, sanal cihaz koleksiyonunu otomatik olarak sağlanır. Mevcut sanal cihazlar özelleştirme veya kendinizinkini oluşturun.

Bu makalede, bir sanal cihaz davranışını ve özellikleri belirten cihaz modeli şemasını açıklar. Cihaz modeli, bir JSON dosyasında depolanır.

> [!NOTE]
> Bu cihaz modeli şemasını, yalnızca cihaz benzetimi hizmette barındırılan sanal cihazlar içindir. Gerçek bir cihaz oluşturmak istiyorsanız, bkz. [Cihazınızı Uzaktan izleme çözüm hızlandırıcısına bağlamayı](iot-accelerators-connecting-devices.md).

Aşağıdaki makaleler için geçerli makalenin ilgili:

* [Cihaz modeli davranışlarını uygulamak](iot-accelerators-remote-monitoring-device-behavior.md) bir sanal cihaz davranışını uygulamak için kullandığınız JavaScript dosyalarını açıklar.
* [Yeni bir simülasyon cihazı oluşturma](iot-accelerators-remote-monitoring-create-simulated-device.md) tümünü bir araya getirir ve çözümünüze yeni bir sanal cihaz türü dağıtma işlemi gösterilmektedir.

Bu makalede şunları öğreneceksiniz:

>[!div class="checklist"]
> * Bir sanal cihaz modeli tanımlamak için bir JSON dosyası kullanma
> * Sanal cihaz özelliklerini belirtin
> * Sanal cihazın gönderdiği telemetriyi belirtin
> * Cihaz yanıt veren bulut-cihaz yöntemleri belirtin

## <a name="the-parts-of-the-device-model-schema"></a>Cihaz modeli şemasını bölümleri

Her cihaz modeli, kamyon, ya da Soğutucu gibi cihaz benzetimi hizmet benzetimini yapabilirsiniz türü tanımlar. Her cihaz modeli, aşağıdaki üst düzey şema içeren bir JSON dosyası olarak depolanır:

```json
{
  "SchemaVersion": "1.0.0",
  "Id": "elevator-01",
  "Version": "0.0.1",
  "Name": "Elevator",
  "Description": "Elevator with floor, vibration and temperature sensors.",
  "Protocol": "AMQP",
  "Simulation": {
    // Specify the simulation behavior
  },
  "Properties": {
    // Define properties
  },
  "Telemetry": [
    // Specify telemetry
  ],
  "CloudToDeviceMethods": {
    // Specify methods
  }
}
```

Varsayılan sanal cihazlar için şema dosyalarını görüntüleyebilirsiniz [devicemodels klasör](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels) GitHub üzerinde.

Aşağıdaki tabloda, en üst düzey şema girişleri açıklanmaktadır:

| Şema giriş | Açıklama |
| -- | --- |
| `SchemaVersion` | Şema sürümü her zaman olduğu `1.0.0` ve bu dosya biçiminin özgüdür. |
| `Id` | Bu cihaz modeli için bir benzersiz kimliği. |
| `Version` | Cihaz modeli sürümünü tanımlar. |
| `Name` | Cihaz modeli için bir kolay ad. |
| `Description` | Cihaz modeli açıklaması. |
| `Protocol` | Cihaz bağlantı protokolünü kullanır. Herhangi birini `AMQP`, `MQTT`, ve `HTTP`. |

Aşağıdaki bölümlerde, JSON şema diğer bölümlerinde açıklanmaktadır:

## <a name="simulation"></a>Benzetim

İçinde `Simulation` bölümünde, sanal cihaz iç durumunu tanımlar. Cihaz tarafından gönderilen herhangi bir telemetri değeri, bu cihaz durumu parçası olmalıdır.

Cihaz durumu tanımı iki öğeyi içerir:

* `InitialState` ilk cihaz durum nesnesinin tüm özelliklerinin değerlerini tanımlar.
* `Script` cihaz durumunu güncelleştirmek için bir zamanlamaya göre çalışan bir JavaScript dosyası tanımlar. Cihaz tarafından gönderilen telemetri değerlerini rastgele seçmek için bu betiği kullanabilirsiniz.

Cihaz durumu nesnesini güncelleştirir JavaScript dosyası hakkında daha fazla bilgi için bkz: [cihaz modeli davranışlarını anlama](../../articles/iot-accelerators/iot-accelerators-device-simulation-advanced-device.md).

Aşağıdaki örnek sanal Soğutucu cihaz için cihaz durum nesnesinin tanımı gösterilmektedir:

```json
"Simulation": {
  "InitialState": {
    "online": true,
    "temperature": 75.0,
    "temperature_unit": "F",
    "humidity": 70.0,
    "humidity_unit": "%",
    "pressure": 150.0,
    "pressure_unit": "psig",
    "simulation_state": "normal_pressure"
  },
  "Interval": "00:00:10",
  "Scripts": {
    "Type": "javascript",
    "Path": "chiller-01-state.js"
  }
}
```

Benzetim hizmeti çalıştığında **Soğutucu 01 state.js** cihaz durumunu güncelleştirmek için her beş saniyede bir dosya. Varsayılan sanal cihazlar için JavaScript dosyaları gördüğünüz [scripts klasörü](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels/scripts) GitHub üzerinde. Kural olarak, bu JavaScript dosyaları son ekine sahip **-durum** yöntemi davranışları uygulayan dosyaları bunları ayırt etmek için.

## <a name="properties"></a>Özellikler

`Properties` Bölümü şemanın cihaz raporlarına çözüm özellik değerlerini tanımlar. Örneğin:

```json
"Properties": {
  "Type": "Elevator",
  "Location": "Building 2",
  "Latitude": 47.640792,
  "Longitude": -122.126258
}
```

Çözüm başladığında, bu sorgular bir listesini oluşturmak için tüm sanal cihazların `Type` Arabiriminde kullanılacak değerler. Çözüm `Latitude` ve `Longitude` cihazın konumunu Pano haritaya eklemek için özellikler.

## <a name="telemetry"></a>Telemetri

`Telemetry` Dizi sanal cihaz çözüme gönderdiği tüm telemetri türlerini listeler.

Aşağıdaki örnek 10 saniyede bir JSON telemetri iletisi gönderen `floor`, `vibration`, ve `temperature` Asansör 's sensörlerden alınan verilerin:

```json
"Telemetry": [
  {
    "Interval": "00:00:10",
    "MessageTemplate": "{\"floor\":${floor},\"vibration\":${vibration},\"vibration_unit\":\"${vibration_unit}\",\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\"}",
    "MessageSchema": {
      "Name": "elevator-sensors;v1",
      "Format": "JSON",
      "Fields": {
        "floor": "integer",
        "vibration": "double",
        "vibration_unit": "text",
        "temperature": "double",
        "temperature_unit": "text"
      }
    }
  }
]
```

`MessageTemplate` Sanal cihaz tarafından gönderilen JSON ileti yapısını tanımlar. İçindeki yer tutucuları `MessageTemplate` söz dizimini kullanın `${NAME}` burada `NAME` bir anahtarı olan [cihaz durumu nesnesi](#simulation). Dizeleri tırnak içine, numaralar değil.

`MessageSchema` Sanal cihaz tarafından gönderilen ileti şemasını tanımlar. İleti şeması, gelen telemetriyi yorumlamak için bilgileri yeniden kullanmak arka uç uygulamalarını etkinleştirmek için IOT Hub'ına de yayımlanır.

Şu anda yalnızca JSON ileti şemaları kullanabilirsiniz. Şemada listelenen alanları aşağıdaki türde olabilir:

* JSON'ı kullanarak seri durumdan çıkarılmış nesne-
* İkili - base64 kullanarak seri hale getirilmiş
* Text
* Boolean
* Integer
* Double
* DateTime

Farklı aralıklarla telemetri iletilerini göndermek için birden fazla telemetri türleri için ekleme `Telemetry` dizisi. Aşağıdaki örnek, her 10 sıcaklık ve nem veri gönderir saniye ve ışık dakikada durumu:

```json
"Telemetry": [
  {
    "Interval": "00:00:10",
    "MessageTemplate":
      "{\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\",\"humidity\":\"${humidity}\"}",
    "MessageSchema": {
      "Name": "RoomComfort;v1",
      "Format": "JSON",
      "Fields": {
        "temperature": "double",
        "temperature_unit": "text",
        "humidity": "integer"
      }
    }
  },
  {
    "Interval": "00:01:00",
    "MessageTemplate": "{\"lights\":${lights_on}}",
    "MessageSchema": {
      "Name": "RoomLights;v1",
      "Format": "JSON",
      "Fields": {
        "lights": "boolean"
      }
    }
  }
],
```

## <a name="cloudtodevicemethods"></a>CloudToDeviceMethods

Bir sanal cihaz IOT hub'ından adlı bulut-cihaz yöntemler yanıt verebilir. `CloudToDeviceMethods` Cihaz modeli şema dosyasının bir bölümünde:

* Sanal cihazın yanıt verebildiği yöntemleri tanımlar.
* Yürütmek için mantığı içeren bir JavaScript dosyası tanımlar.

Sanal cihaz destekliyorsa yöntemlerin listesi bağlı IOT hub'ına gönderir.

Cihaz davranışını uygulayan bir JavaScript dosyası hakkında daha fazla bilgi için bkz: [cihaz modeli davranışlarını anlama](../../articles/iot-accelerators/iot-accelerators-device-simulation-advanced-device.md).

Aşağıdaki örnek, desteklenen üç yöntem ve bu yöntemleri kullanan JavaScript dosyalarının belirtir:

```json
"CloudToDeviceMethods": {
  "Reboot": {
    "Type": "javascript",
    "Path": "Reboot-method.js"
  },
  "EmergencyValveRelease": {
    "Type": "javascript",
    "Path": "EmergencyValveRelease-method.js"
  },
  "IncreasePressure": {
    "Type": "javascript",
    "Path": "IncreasePressure-method.js"
  }
}
```

Varsayılan sanal cihazlar için JavaScript dosyaları gördüğünüz [scripts klasörü](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels/scripts) GitHub üzerinde. Kural olarak, bu JavaScript dosyaları son ekine sahip **-yöntemi** durumu davranışı uygulayan dosyaları bunları ayırt etmek için.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kendi özel sanal cihaz modeli oluşturmak nasıl kaydedileceği açıklanır. Bu makalede gösterilen, nasıl için:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Bir sanal cihaz modeli tanımlamak için bir JSON dosyası kullanma
> * Sanal cihaz özelliklerini belirtin
> * Sanal cihazın gönderdiği telemetriyi belirtin
> * Cihaz yanıt veren bulut-cihaz yöntemleri belirtin

JSON şeması hakkında öğrendiniz, önerilen sonraki adıma öğrenmektir nasıl [sanal Cihazınızı davranışını uygulayan](iot-accelerators-remote-monitoring-device-behavior.md).

Uzaktan izleme çözümü hakkında daha fazla Geliştirici bilgi için bkz:

* [Geliştirici Başvuru Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Geliştirici Sorun Giderme Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)
