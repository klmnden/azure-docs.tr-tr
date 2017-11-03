---
title: "Azure IOT kenar (Windows) bir cihazın benzetimini | Microsoft Docs"
description: "Azure IOT kenar Windows üzerinde bir IOT hub'ına bir Azure IOT sınır ağ geçidi üzerinden telemetri gönderen sanal bir cihaz oluşturmak için nasıl kullanılır."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 6a2aeda0-696a-4732-90e1-595d2e2fadc6
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2017
ms.author: andbuc
ms.openlocfilehash: 0aa1836ee1445894022b95fefc2338ef53698240
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-iot-edge-to-send-device-to-cloud-messages-with-a-simulated-device-windows"></a>Bir sanal cihaz (Windows) ile cihaz bulut iletilerini göndermek için Azure IOT kenar kullanın

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="run-the-sample"></a>Örnek çalıştırın

**Build.cmd** komut dosyası oluşturur, çıktı **yapı** yerel kopyasını klasöründe **IOT kenar** deposu. Bu çıktı, bu örnekte kullanılan dört IOT kenar modüller içerir.

Derleme betiğinin aşağıdaki dosyaları oluşturur:

* **Logger.dll** içinde **yapı\\modülleri\\Günlükçü\\hata ayıklama** klasör.
* **iothub.dll** içinde **yapı\\modülleri\\ıothub\\hata ayıklama** klasör.
* **kimlik\_map.dll** içinde **yapı\\modülleri\\identitymap\\hata ayıklama** klasör.
* **Benzetimli\_device.dll** içinde **yapı\\modülleri\\benzetimli\_aygıt\\hata ayıklama** klasör.

Bu yolları için kullanma **modül yolu** değerleri benzetimli gösterildiği gibi\_aygıt\_bulut\_karşıya\_JSON ayarları dosyası win.

Benzetimli\_aygıt\_bulut\_karşıya yükleme örnek işlemi komut satırı bağımsız değişkeni olarak bir JSON yapılandırma dosyasına yolunu alır. Aşağıdaki örnek JSON dosyası SDK deposunda sağlanan **örnekleri\\benzetimli\_aygıt\_bulut\_karşıya\_örnek\\src\\ Benzetimli\_aygıt\_bulut\_karşıya\_win.json**. Bu yapılandırma dosyası IOT kenar modülleri veya örnek yürütülebilir dosyalar varsayılan olmayan konumlara yerleştirmek için yapı betik değiştirmediğiniz sürece olduğu gibi çalışır.

> [!NOTE]
> Dizine göre modülü yollardır burada benzetimli\_aygıt\_bulut\_karşıya\_sample.exe bulunduğu. Örnek JSON yapılandırma dosyası, geçerli çalışma dizini içinde 'deviceCloudUploadGatewaylog.log' yazmak için varsayılan olarak ayarlanır.

Dosyayı bir metin düzenleyicisinde açın **örnekleri\\benzetimli\_aygıt\_bulut\_karşıya\\src\\benzetimli\_aygıt\_bulut\_karşıya\_win.json** yerel kopyasını içinde **IOT kenar** deposu. Bu dosya IOT kenar modüllerini örnek ağ geçidi yapılandırır:

* **Iothub** modülü IOT hub'ınıza bağlanır. IOT hub'ınıza veri göndermek için yapılandırın. Özellikle, ayarlanan **IoTHubName** değerini IOT hub'ınızın adını ve ayarlayın **IoTHubSuffix** değeri **azure devices.net**. Ayarlama **aktarım** değeri birine: **HTTP**, **AMQP**, veya **MQTT**. Şu anda yalnızca **HTTP** tüm aygıt iletiler için bir TCP bağlantısı paylaşır. Değeri ayarlarsanız verilen **AMQP**, veya **MQTT**, IOT hub'ı her aygıt için ayrı bir TCP bağlantı ağ geçidi korur.
* **Eşleme** modülü, IOT Hub cihaz kimliklerini sanal cihazlarınızın MAC adreslerini eşler. Ayarlama **DeviceID** IOT hub'ınıza eklediğiniz iki aygıt kimlikleri için değerler. Ayarlama **deviceKey** iki aygıtlarınızın anahtarlarına değerleri.
* **BLE1** ve **BLE2** modüllerdir sanal cihazlar. Nasıl modülü MAC adresleri eşleşen Not **eşleme** modülü.
* **Günlükçü** modülü, ağ geçidi etkinliği bir dosyaya kaydeder.
* **Modül yolu** aşağıdaki örnekte gösterilen göre dizin değerlerdir burada benzetimli\_aygıt\_bulut\_karşıya\_sample.exe bulunduğu.
* **Bağlantılar** dizi JSON dosyasının sonuna bağlayan **BLE1** ve **BLE2** modüllerini **eşleme** modülü ve **eşleme** modülüne **Iothub** modülü. Ayrıca tüm iletileri tarafından kaydedilir sağlar **Günlükçü** modülü.

```json
{
    "modules" :
    [
      {
        "name": "IotHub",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\iothub\\Debug\\iothub.dll"
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
            "module.path": "..\\..\\..\\modules\\identitymap\\Debug\\identity_map.dll"
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
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
          }
          },
          "args": {
            "macAddress": "01:01:01:01:01:01",
            "messagePeriod" : 2000
          }
        },
      {
        "name": "BLE2",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
          }
          },
          "args": {
            "macAddress": "02:02:02:02:02:02",
            "messagePeriod" : 2000
          }
        },
      {
        "name": "Logger",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\logger\\Debug\\logger.dll"
          }
        },
        "args": {
          "filename": "deviceCloudUploadGatewaylog.log"
        }
      }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IotHub" }
    ]
}
```

Yapılandırma dosyasına yapılan değişiklikleri kaydedin.

Örneği çalıştırmak için:

1. Bir komut isteminde gidin **yapı** yerel kopyasını klasöründe **IOT kenar** deposu.
2. Şu komutu çalıştırın:
   
    ```cmd
    samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe ..\samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```
3. Kullanabileceğiniz [aygıt explorer] [ lnk-device-explorer] veya [iothub-explorer] [ lnk-iothub-explorer] ağ geçidi'nden IOT hub'ınızın aldığı iletileri izlemek için aracı. Örneğin, ıothub explorer kullanarak aşağıdaki komutu kullanarak cihaz bulut iletilerini izleyebilirsiniz:

    ```cmd
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a>Sonraki adımlar

IOT kenar daha gelişmiş anlamak ve bazı kod örnekleri ile denemek için aşağıdaki Geliştirici öğreticiler ve kaynakları ziyaret edin:

* [IOT Edge fiziksel CİHAZDAN cihaz bulut iletilerini gönder][lnk-physical-device]
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
