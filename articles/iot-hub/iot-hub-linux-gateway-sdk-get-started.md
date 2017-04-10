---
title: "Azure IoT Ağ Geçidi SDK&quot;sı (Linux) ile çalışmaya başlama | Microsoft Belgeleri"
description: "Linux makine üzerinde ağ geçidi oluşturmanın yanı sıra modüller ve JSON yapılandırma dosyaları gibi temel Azure IoT Ağ Geçidi SDK&quot;sı kavramları hakkında bilgi edinin."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: cf537bdd-2352-4bb1-96cd-a283fcd3d6cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/28/2017
ms.author: andbuc
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 856ffeeeb8f9d8296ba972a9e070686171f7fde8
ms.lasthandoff: 03/31/2017


---
# <a name="explore-the-iot-gateway-sdk-architecture-on-linux"></a>Linux’ta IoT Ağ Geçidi SDK mimarisini keşfetme

[!INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Örnek oluşturma

Başlamadan önce Linux üzerindeki SDK ile çalışacak [geliştirme ortamınızı ayarlamanız][lnk-setupdevbox] gerekir.

1. Bir kabuk açın.
1. **azure-iot-gateway-sdk** deposunun yerel kopyasındaki kök klasöre gidin.
1. **tools/build.sh** betiğini çalıştırın. Bu betik **cmake** yardımcı programını kullanarak **azure-iot-gateway-sdk** deposu yerel kopyasının kök klasöründe **build** adlı bir klasör ve bir derleme görevleri dosyası oluşturur. Betik daha sonra çözümü derler ve birim testleri ile uçtan uca testleri atlar. Derleme yapmak ve birim testlerini çalıştırmak istiyorsanız **--run-unittests** parametresini ekleyin. Derleme yapmak ve uçtan uca testleri çalıştırmak istiyorsanız **--run-e2e-tests** parametresini ekleyin.

> [!NOTE]
> **build.sh** betiğini her çalıştırdığınızda betik **azure-iot-gateway-sdk** deposu yerel kopyasının kök klasöründe bulunan **build** klasörünü siler ve yeniden oluşturur.

## <a name="how-to-run-the-sample"></a>Örneği çalıştırma

1. **build.sh** betiği, çıktısını **azure-iot-gateway-sdk** deposu yerel kopyasının **build** klasöründe oluşturur. Bu çıktı bu örnekte kullanılan iki modülü içerir.

    Derleme betiği, **liblogger.so** dosyasını **build/modules/logger/** klasörüne ve **libhello\_world.so** dosyasını **build/modules/hello_world/** klasörüne yerleştirir. Aşağıdaki örnek JSON ayarları dosyasında gösterildiği gibi **modül yolu** değeri için bu yolları kullanın.
1. Merhaba\_dünya\_örneği işleminde JSON yapılandırma dosyasının yolu komut satırı bağımsız değişkeni olarak kullanılır. Aşağıdaki örnek JSON dosyası, SDK deposundaki **samples/hello\_world/src/hello\_world\_lin.json** dizininde sağlanır. Derleme betiğini, modül ya da örnek yürütülebilir dosyaları varsayılan olmayan konumlara yerleştirecek şekilde değiştirmediğiniz sürece bu yapılandırma dosyası olduğu gibi çalışır.

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
1. **Derleme** klasörüne gidin.
1. Şu komutu çalıştırın:

   `./samples/hello_world/hello_world_sample ./../samples/hello_world/src/hello_world_lin.json`

[!INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md

