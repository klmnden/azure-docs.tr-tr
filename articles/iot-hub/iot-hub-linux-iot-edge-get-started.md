---
title: "Azure IOT kenar (Linux) ile çalışmaya başlama | Microsoft Docs"
description: "Bir Linux makine üzerinde ağ geçidi oluşturmanın yanı sıra modüller ve JSON yapılandırma dosyaları gibi temel Azure IoT Edge kavramları hakkında bilgi edinin."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: cf537bdd-2352-4bb1-96cd-a283fcd3d6cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2017
ms.author: andbuc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fb65e3c34d2b2a14370792d8506c13c8c5fb522e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="explore-azure-iot-edge-architecture-on-linux"></a>Linux üzerinde Azure IoT Edge mimarisini keşfedin

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-to-run-the-sample"></a>Örneği çalıştırma

**build.sh** betiği, çıktısını **iot-edge** deposu yerel kopyasının **build** klasöründe oluşturur. Bu çıktı, bu örnekte kullanılan iki IOT kenar modüller içerir.

Derleme betiği, **liblogger.so** dosyasını **build/modules/logger/** klasörüne ve **libhello\_world.so** dosyasını **build/modules/hello_world/** klasörüne yerleştirir. Bu yolları için kullanma **modül yolu** değerleri örnek JSON ayarları dosyasında gösterildiği gibi.

Merhaba\_world\_örnek işlemi komut satırı bağımsız değişkeni olarak bir JSON yapılandırma dosyasına yolunu alır. Aşağıdaki örnek JSON dosyası, SDK deposundaki **samples/hello\_world/src/hello\_world\_lin.json** dizininde sağlanır. Bu yapılandırma dosyası IOT kenar modülleri veya örnek yürütülebilir dosyalar varsayılan olmayan konumlara yerleştirmek için yapı betik değiştirmediğiniz sürece olduğu gibi çalışır.

> [!NOTE]
> Modül yolları, geçerli çalışma dizinine göre belirlenmiştir ve hello\_world\_sample yürütülebilir dosyasının bulunduğu değil çalıştırıldığı dizini temel alır. Örnek JSON yapılandırma dosyası varsayılan olarak “log.txt” dosyasını geçerli çalışma dizininize yazar.

```json
{
    "modules" :
    [
        {
            "name" : "logger",
            "loader": {
            "name": "native",
            "entrypoint": {
                "module.path": "./modules/logger/liblogger.so"
            }
            },
            "args" : {"filename":"log.txt"}
        },
        {
            "name" : "hello_world",
            "loader": {
            "name": "native",
            "entrypoint": {
                "module.path": "./modules/hello_world/libhello_world.so"
            }
            },
            "args" : null
        }
    ],
    "links":
    [
        {
            "source": "hello_world",
            "sink": "logger"
        }
    ]
}
```

1. Gidin **yapı** yerel kopyasını kök klasöründe **IOT kenar** deposu.

1. Şu komutu çalıştırın:

    ```sh
    ./samples/hello_world/hello_world_sample ../samples/hello_world/src/hello_world_lin.json
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]

