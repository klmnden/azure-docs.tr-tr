---
title: Uzaktan izleme çözümü - Azure aygıt şemada | Microsoft Docs
description: Bu makalede Uzaktan izleme çözümünde benzetilmiş bir aygıtı tanımlayan JSON şeması açıklanır.
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 01/29/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 186eaee952435573a861d144195c3165e4940cc1
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="understand-the-device-model-schema"></a>Cihaz modeli şeması anlama

Uzaktan izleme çözümünde sanal cihazlar davranışını test etmek için kullanabilirsiniz. Uzaktan izleme çözümü dağıttığınızda, bir sanal cihaz koleksiyonu otomatik olarak sağlanır. Varolan sanal cihazlar özelleştirebilir veya kendinizinkini oluşturun.

Bu makalede özellikleri ve bir sanal cihaz davranışını belirtir aygıt model şeması açıklanır. Cihaz modeli JSON dosyasında depolanır.

Aşağıdaki makaleler geçerli makaleyi ilişkili:

* [Cihaz modeli davranışı uygulamak](../iot-suite/iot-suite-remote-monitoring-device-behavior.md) kullanan bir sanal cihaz davranışını uygulamak için JavaScript dosyaları açıklar.
* [Yeni bir sanal cihaz oluşturma](iot-accelerators-remote-monitoring-test.md) hepsini bir araya getirir ve çözümünüz için yeni bir sanal cihaz türü dağıtılacağı gösterilmektedir.

Bu makalede şunları öğreneceksiniz:

>[!div class="checklist"]
> * Bir sanal cihaz modeli tanımlamak için bir JSON dosyası kullanın
> * Sanal cihaz özelliklerini belirtin
> * Sanal cihaz gönderdiği telemetri belirtin
> * Cihaz yanıtlaması bulut cihaz yöntemleri belirtin

## <a name="the-parts-of-the-device-model-schema"></a>Cihaz modeli şema bölümleri

Soğutucu veya kamyon, gibi her cihaz modeli Uzaktan izleme çözümüne bağlama için sanal cihaz türünü tanımlar. Her cihaz modeli bir JSON dosyası aşağıdaki üst düzey şema depolanır:

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

Benzetimli varsayılan aygıtlar için şema dosyaları görüntüleyebilirsiniz [devicemodels klasörü](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels) github'da.

Aşağıdaki tabloda, en üst düzey şeması girdilerinin açıklanmaktadır:

| Şema giriş | Açıklama |
| -- | --- |
| `SchemaVersion` | Şema sürümü her zaman olduğu `1.0.0` ve bu dosya biçimine özeldir. |
| `Id` | Bu cihaz modeli için benzersiz bir kimliği. |
| `Version` | Cihaz modeli sürümü tanımlar. |
| `Name` | Cihaz modeli için bir kolay ad. |
| `Description` | Cihaz modeli açıklaması. |
| `Protocol` | Cihaz bağlantı protokolünü kullanır. Aşağıdakilerden biri olabilir `AMQP`, `MQTT`, ve `HTTP`. |

Aşağıdaki bölümlerde, JSON şeması diğer bölümlerinde açıklanmaktadır:

## <a name="simulation"></a>Benzetim

İçinde `Simulation` bölümünde, sanal cihaz iç durumu tanımlayın. Aygıt tarafından gönderilen herhangi bir telemetri değeri, bu cihaz durumu parçası olması gerekir.

Cihaz durumu tanımını iki öğe vardır:

* `InitialState` cihaz durumu nesnenin tüm özelliklerinin ilk değerleri tanımlar.
* `Script` Aygıt durumunu güncelleştirmek için bir zamanlamaya göre çalışan bir JavaScript dosyası tanımlar. Aygıt tarafından gönderilen telemetriyi değerleri rastgele seçmek için bu komut dosyasını kullanabilirsiniz.

Cihaz durumu nesnesi güncelleştirmeleri JavaScript dosyası hakkında daha fazla bilgi için bkz: [aygıt modeli davranışlarını anlamak](iot-accelerators-remote-monitoring-device-behavior.md).

Aşağıdaki örnek, bir sanal Soğutucu cihaz için cihaz durumu nesnesinin tanımı gösterir:

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

Benzetim hizmeti çalıştığında **Soğutucu 01 state.js** aygıt durumunu güncelleştirmek için beş saniyede dosya. Benzetimli varsayılan aygıtları için JavaScript dosyaları görebilirsiniz [betikleri klasörü](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels/scripts) github'da. Kurala göre bu JavaScript dosyaları son ekine sahip **-durum** yöntemi davranışlarını uygulamak dosyalarından ayırmak için.

## <a name="properties"></a>Özellikler

`Properties` Şema bölümünü cihaz raporlarına çözüme özellik değerlerini tanımlar. Örneğin:

```json
"Properties": {
  "Type": "Elevator",
  "Location": "Building 2",
  "Latitude": 47.640792,
  "Longitude": -122.126258
}
```

Çözüm başladığında listesini oluşturmak için tüm sanal cihazların sorgular `Type` kullanıcı Arabiriminde kullanılacak değerleri. Çözüm kullanır `Latitiude` ve `Longitude` Pano haritada cihazın konumunu eklemek için özellikler.

## <a name="telemetry"></a>Telemetri

`Telemetry` Dizi sanal cihaz gönderir çözüme tüm telemetri türlerini listeler.

Aşağıdaki örnekte 10 dakikada bir JSON telemetri ileti gönderir `floor`, `vibration`, ve `temperature` fırsatınızdır 's algılayıcı verileri:

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

`MessageTemplate` Sanal cihaz tarafından gönderilen JSON ileti yapısını tanımlar. Yer tutucuları `MessageTemplate` söz dizimini kullanın `${NAME}` nerede `NAME` bir anahtarı olan [cihaz durumu nesnesi](#simulation). Tırnak içine alınmış dizeler, numaraları kullanmamalıdır.

`MessageSchema` Sanal cihaz tarafından gönderilen ileti şeması tanımlar. İleti şeması da arka uç uygulamalarının gelen telemetri yorumlamak için bilgileri yeniden etkinleştirmek için IOT Hub'ına yayımlanır.

Şu anda, yalnızca JSON ileti şemaları kullanabilirsiniz. Şemada listelenen alanları aşağıdaki türde olabilir:

* JSON kullanılarak serileştirilmiş nesne-
* İkili dosya - seri base64 kullanma
* Metin
* Boole
* Tamsayı
* Çift
* DateTime

Farklı aralıklarla telemetri iletilerini göndermek için birden çok telemetri türlerini eklemek `Telemetry` dizi. Aşağıdaki örnek, her 10 sıcaklık ve nem veri gönderir saniye ve dakikada hafif durumu:

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

Bir sanal cihaz Uzaktan izleme çözümü adlı bulut cihaz yöntemler yanıt verebilir. `CloudToDeviceMethods` Aygıt modeli şema dosyası bölümünde:

* Sanal cihazın karşılık verebildiği yöntemleri tanımlar.
* Yürütmek için mantığı içeren JavaScript dosyası tanımlar.

Sanal cihazı Uzaktan izleme çözümüne destekliyorsa yöntemlerin listesi gönderir.

Cihaz davranışını uygulayan JavaScript dosyası hakkında daha fazla bilgi için bkz: [aygıt modeli davranışlarını anlamak](iot-accelerators-remote-monitoring-device-behavior.md).

Aşağıdaki örnek, desteklenen üç yöntem ve bu yöntemleri uygulamak JavaScript dosyaları belirtir:

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

Benzetimli varsayılan aygıtları için JavaScript dosyaları görebilirsiniz [betikleri klasörü](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels/scripts) github'da. Kurala göre bu JavaScript dosyaları son ekine sahip **-yöntemi** durumu davranışını uygulayan dosyalarını ayırmak için.

## <a name="next-steps"></a>Sonraki adımlar

Bu makale, kendi özel sanal cihaz modeli oluşturma açıklanmaktadır. Bu makalede gösterilen, nasıl için:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Bir sanal cihaz modeli tanımlamak için bir JSON dosyası kullanın
> * Sanal cihaz özelliklerini belirtin
> * Sanal cihaz gönderdiği telemetri belirtin
> * Cihaz yanıtlaması bulut cihaz yöntemleri belirtin

JSON şeması hakkında öğrendiniz artık, önerilen sonraki adıma edinmektir nasıl [sanal cihazınız davranışını uygulamak](iot-accelerators-remote-monitoring-device-behavior.md).

Uzaktan izleme çözümü hakkında daha fazla Geliştirici bilgi için bkz:

* [Geliştirici Başvuru Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Geliştirici Sorun Giderme Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->