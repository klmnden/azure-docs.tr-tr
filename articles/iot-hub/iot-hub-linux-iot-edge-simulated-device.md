---
title: "Azure IOT kenar (Linux) bir cihazın benzetimini | Microsoft Docs"
description: "Azure IOT kenar Linux'ta bir IOT hub'ına bir IOT sınır ağ geçidi üzerinden telemetri gönderen sanal bir cihaz oluşturma için nasıl kullanılacağını."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 11e7bf28-ee3d-48d6-a386-eb506c7a31cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/09/2017
ms.author: andbuc
ms.openlocfilehash: 5349960373ae6815862c5f79a69dd6d5d9d624ab
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-iot-edge-to-send-device-to-cloud-messages-with-a-simulated-device-linux"></a>Bir sanal cihaz (Linux) ile cihaz bulut iletilerini göndermek için Azure IOT kenar kullanın

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-to-run-the-sample"></a>Örneği çalıştırma

**build.sh** betiği, çıktısını **iot-edge** deposu yerel kopyasının **build** klasöründe oluşturur. Bu çıktı, bu örnekte kullanılan dört IOT kenar modüller içerir.

Yapı betik basamak:

* **liblogger.SO** içinde **modülleri/yapı/Günlükçü** klasör.
* **libiothub.SO** içinde **modülleri/yapı/ıothub** klasör.
* **LIB\_kimlik\_map.so** içinde **modülleri/yapı/identitymap** klasör.
* **libsimulated\_device.so** içinde **yapı/modülleri/benzetimli\_aygıt** klasör.

Bu yolları için kullanma **modül yolu** değerleri aşağıdaki JSON ayarları dosyasında gösterildiği gibi:

Benzetimli\_aygıt\_bulut\_karşıya\_örnek işlemi komut satırı bağımsız değişkeni olarak bir JSON yapılandırma dosyasına yolunu alır. Aşağıdaki örnek JSON dosyası SDK deposunda sağlanan **örnekleri\\benzetimli\_aygıt\_bulut\_karşıya\_örnek\\src\\ Benzetimli\_aygıt\_bulut\_karşıya\_örnek\_lin.json**. Bu yapılandırma dosyası IOT kenar modülleri veya örnek yürütülebilir dosyalar varsayılan olmayan konumlara yerleştirmek için yapı betik değiştirmediğiniz sürece olduğu gibi çalışır.

> [!NOTE]
> Modül benzetimli çalıştırdığınız gelen dizine göreli yollardır\_aygıt\_bulut\_karşıya\_örnek yürütülebilir, yürütülebilir dosyanın bulunduğu dizin değil. Örnek JSON yapılandırma dosyası, geçerli çalışma dizini içinde 'deviceCloudUploadGatewaylog.log' yazmak için varsayılan olarak ayarlanır.

Dosyayı bir metin düzenleyicisinde açın **örnekleri ve benzetimli\_aygıt\_bulut\_karşıya\_örnek/src/benzetimli\_aygıt\_bulut\_KarşıyaYükle\_lin.json** yerel kopyasını içinde **IOT kenar** deposu. Bu dosya IOT kenar modüllerini örnek ağ geçidi yapılandırır:

* **Iothub** modülü IOT hub'ınıza bağlanır. IOT hub'ınıza veri göndermek için yapılandırın. Özellikle, ayarlanan **IoTHubName** değerini IOT hub'ınızın adını ve ayarlayın **IoTHubSuffix** değeri **azure devices.net**. Ayarlama **aktarım** değeri birine: **HTTP**, **AMQP**, veya **MQTT**. Şu anda yalnızca **HTTP** tüm aygıt iletiler için bir TCP bağlantısı paylaşır. Değeri ayarlarsanız verilen **AMQP**, veya **MQTT**, IOT hub'ı her aygıt için ayrı bir TCP bağlantı ağ geçidi korur.
* **Eşleme** modülü IOT Hub cihaz kimliklerinizi sanal cihazlarınızın MAC adreslerini eşler. Olduğundan emin olun **DeviceID** değerleriyle eşleşen IOT hub'ınızı ve, eklediğiniz iki aygıtların kimliklerini **deviceKey** değerleri içeren iki aygıtlarınızın anahtarları.
* **BLE1** ve **BLE2** modüllerdir sanal cihazlar. Nasıl MAC adreslerini eşleşen Not **eşleme** modülü.
* **Günlükçü** modülü, ağ geçidi etkinliği bir dosyaya kaydeder.
* **Modül yolu** örnekte gösterilen değerleri varsayın örnek alarak çalıştırın **yapı** yerel kopyasını klasöründe **IOT kenar** deposu.
* **Bağlantılar** dizi JSON dosyasının sonuna bağlayan **BLE1** ve **BLE2** modüllerini **eşleme** modülü ve **eşleme** modülüne **Iothub** modülü. Ayrıca tüm iletileri tarafından kaydedilir sağlar **Günlükçü** modülü.

```json
{
    "modules": [
        {
            "name": "IotHub",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "./modules/iothub/libiothub.so"
            }
            },
            "args": {
              "IoTHubName": "<<insert here IoTHubName>>",
              "IoTHubSuffix": "<<insert here IoTHubSuffix>>",
              "Transport": "HTTP"
            }
          },
        {
            "name": "mapping",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "./modules/identitymap/libidentity_map.so"
            }
            },
            "args": [
              {
                "macAddress": "01:01:01:01:01:01",
                "deviceId": "<<insert here deviceId>>",
                "deviceKey": "<<insert here deviceKey>>"
              },
              {
                "macAddress": "02:02:02:02:02:02",
                "deviceId": "<<insert here deviceId>>",
                "deviceKey": "<<insert here deviceKey>>"
              }
            ]
          },
        {
            "name": "BLE1",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "./modules/simulated_device/libsimulated_device.so"
            }
            },
            "args": {
              "macAddress": "01:01:01:01:01:01"
            }
          },
        {
            "name": "BLE2",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "./modules/simulated_device/libsimulated_device.so"
            }
            },
            "args": {
              "macAddress": "02:02:02:02:02:02"
            }
          },
        {
            "name": "Logger",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "./modules/logger/liblogger.so"
            }
            },
            "args": {
              "filename": "deviceCloudUploadGatewaylog.log"
            }
          }
    ],
    "links": [
        {
            "source": "*",
            "sink": "Logger"
        },
        {
            "source": "BLE1",
            "sink": "mapping"
        },
        {
            "source": "BLE2",
            "sink": "mapping"
        },
        {
            "source": "mapping",
            "sink": "IotHub"
        }
    ]
}
```

Yapılandırma dosyasına yapılan değişiklikleri kaydedin.

Örneği çalıştırmak için:

1. Kabuğunda gidin **IOT-edge/yapı** klasör.
2. Şu komutu çalıştırın:
   
    ```sh
    ./samples/simulated_device_cloud_upload/simulated_device_cloud_upload_sample ../samples/simulated_device_cloud_upload/src/simulated_device_cloud_upload_lin.json
    ```
3. Kullanabileceğiniz [aygıt explorer] [ lnk-device-explorer] veya [iothub-explorer] [ lnk-iothub-explorer] ağ geçidi'nden IOT hub'ınızın aldığı iletileri izlemek için aracı. Örneğin, ıothub explorer kullanarak aşağıdaki komutu kullanarak cihaz bulut iletilerini izleyebilirsiniz:

    ```sh
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT kenar daha gelişmiş anlamak ve bazı kod örnekleri ile denemek için aşağıdaki Geliştirici öğreticiler ve kaynakları ziyaret edin:

* [Azure IOT Edge fiziksel CİHAZDAN cihaz bulut iletilerini gönder][lnk-physical-device]
* [Azure IOT kenar][lnk-iot-edge]

Daha fazla IOT hub'ı özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu][lnk-devguide]
* [IOT çözümünüzden zemin oluşturan güvenli][lnk-securing]

<!-- Links -->
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-physical-device]: iot-hub-iot-edge-physical-device.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md
