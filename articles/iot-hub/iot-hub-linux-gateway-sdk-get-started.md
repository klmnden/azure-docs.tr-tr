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
ms.date: 11/23/2016
ms.author: andbuc
translationtype: Human Translation
ms.sourcegitcommit: e1cf5ed3f2434a9e98027afd0225207ad5d2f1b1
ms.openlocfilehash: 28984e14f5afc27b608ab37daf19d454eb7c3201


---
# <a name="get-started-with-the-azure-iot-gateway-sdk-linux"></a>Azure IoT Ağ Geçidi SDK'sı (Linux) ile çalışmaya başlama
[!INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Örnek oluşturma
Başlamadan önce Linux üzerindeki SDK ile çalışacak [geliştirme ortamınızı ayarlamanız][lnk-setupdevbox] gerekir.

1. Bir kabuk açın.
2. **azure-iot-gateway-sdk** deposunun yerel kopyasındaki kök klasöre gidin.
3. **tools/build.sh** betiğini çalıştırın. Bu betik **cmake** yardımcı programını kullanarak **azure-iot-gateway-sdk** deposu yerel kopyasının kök klasöründe **build** adlı bir klasör ve bir derleme görevleri dosyası oluşturur. Betik daha sonra çözümü derler ve birim testleri ile uçtan uca testleri atlar. Derleme yapmak ve birim testlerini çalıştırmak istiyorsanız **--run-unittests** parametresini ekleyin. Derleme yapmak ve uçtan uca testleri çalıştırmak istiyorsanız **--run-e2e-tests** parametresini ekleyin.

> [!NOTE]
> **build.sh** betiğini her çalıştırdığınızda betik **azure-iot-gateway-sdk** deposu yerel kopyasının kök klasöründe bulunan **build** klasörünü siler ve yeniden oluşturur.
> 
> 

## <a name="how-to-run-the-sample"></a>Örneği çalıştırma
1. **build.sh** betiği, çıktısını **azure-iot-gateway-sdk** deposu yerel kopyasının **build** klasöründe oluşturur. Buna bu örnekte kullanılan iki modül dahildir.
   
    Build betiği **build/modules/logger/** klasöründeki **liblogger.so** ve **build/modules/hello_world/** klasöründeki **libhello_world.so** dosyasını değiştirir. Aşağıdaki JSON ayarları dosyasında gösterildiği gibi **modül yolu** değeri için bu yolları kullanın.
2. hello_world_sample işlemi, JSON yapılandırma dosyasını yolunu komut satırı bağımsız değişkeni olarak kullanır. Örnek JSON dosyası **azure-iot-gateway-sdk/samples/hello_world/src/hello_world_win.json** adresinde deponun bir parçası olarak verilmiş ve aşağı kopyalanmıştır. Build betiğini, modülleri veya örnek yürütülebilir dosyaları başka yere taşıyarak değiştirmediğiniz sürece mevcut haliyle çalışacaktır.

   > [!NOTE]
   > Modül yolları, geçerli çalışma dizinine göre belirlenmiştir ve hello_world_sample yürütülebilir dosyasının bulunduğu değil çalıştırıldığı dizini temel alır. Örnek JSON yapılandırma dosyası varsayılan olarak “log.txt” dosyasını geçerli çalışma dizininize yazar.
   
    ```
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
3. **azure-iot-gateway-sdk/build** klasörüne gidin.
4. Şu komutu çalıştırın:
   
   ```
   ./samples/hello_world/hello_world_sample ./../samples/hello_world/src/hello_world_lin.json
   ``` 

[!INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md



<!--HONumber=Jan17_HO3-->


