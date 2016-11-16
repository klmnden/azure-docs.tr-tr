---
title: "IoT Hub Ağ Geçidi SDK’sı ile çalışmaya başlama | Microsoft Belgeleri"
description: "Bu Azure IoT Ağ Geçidi SDK’sı kılavuzu, Azure IoT Ağ Geçidi SDK’sı kullanırken anlamanız gereken temel kavramları göstermek üzere Linux kullanır."
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
ms.date: 08/25/2016
ms.author: andbuc
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 23176a9251a90a985a5d2fbce23ceeb9d0925234


---
# <a name="azure-iot-gateway-sdk-beta-get-started-using-linux"></a>Azure IoT Ağ Geçidi SDK’sı (beta) - Linux kullanmaya başlama
[!INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Örnek oluşturma
Başlamadan önce Linux üzerindeki SDK ile çalışacak [geliştirme ortamınızı ayarlamanız][lnk-setupdevbox] gerekir.

1. Bir kabuk açın.
2. **azure-iot-gateway-sdk** deposunun yerel kopyasındaki kök klasöre gidin.
3. **tools/build.sh** betiğini çalıştırın. Bu betik **cmake** yardımcı programını kullanarak **azure-iot-gateway-sdk** deposu yerel kopyasının kök klasöründe **build** adlı bir klasör ve bir derleme görevleri dosyası oluşturur. Betik daha sonra çözümü oluşturur ve testleri çalıştırır.

> [!NOTE]
> **build.sh** betiğini her çalıştırdığınızda betik **azure-iot-gateway-sdk** deposu yerel kopyasının kök klasöründe bulunan **build** klasörünü siler ve yeniden oluşturur.
> 
> 

## <a name="how-to-run-the-sample"></a>Örneği çalıştırma
1. **build.sh** betiği, çıktısını **azure-iot-gateway-sdk** deposu yerel kopyasının **build** klasöründe oluşturur. Buna bu örnekte kullanılan iki modül dahildir.
   
    Build betiği **build/modules/logger/** klasöründeki **liblogger_hl.so** ve **build/modules/hello_world/** klasöründeki **libhello_world_hl.so** dosyasını değiştirir. Aşağıdaki JSON ayarları dosyasında gösterildiği gibi **modül yolu** değeri için bu yolları kullanın.
2. **samples/hello_world/src** klasöründeki **hello_world_lin.json** dosyası örneği çalıştırmak için kullanabileceğiniz Linux’a yönelik bir örnek JSON ayarları dosyasıdır. Aşağıdaki örnekte gösterilen örnek JSON ayarları, örneği **azure-iot-gateway-sdk** deposu yerel kopyasının kökünden çalıştıracağınızı varsayar.
3. **logger_hl** modülü için **args** bölümünde **filename** değerini günlük verilerini içerecek dosyanın adına ve yoluna ayarlayın.
   
    Bu dosya, örneği çalıştıracağınız klasöre **log.txt** dosyasını yazacak Linux’a yönelik bir JSON ayarları dosyasının örneğidir.
   
    ```
    {
      "modules" :
      [ 
        {
          "module name" : "logger_hl",
          "loading args": {
            "module path" : "./build/modules/logger/liblogger_hl.so"
          },
          "args" : 
          {
            "filename":"./log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "loading args": {
            "module path" : "./build/modules/hello_world/libhello_world_hl.so"
          },
          "args" : null
        }
      ],
      "links" :
      [
        {
          "source": "hello_world",
          "sink": "logger_hl"
        }
      ]
    }
    ```
4. Kabuğunuzda **azure-iot-gateway-sdk** klasörüne gidin.
5. Şu komutu çalıştırın:
   
   ```
   ./build/samples/hello_world/hello_world_sample ./samples/hello_world/src/hello_world_lin.json
   ``` 

[!INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md



<!--HONumber=Nov16_HO2-->


