---
title: "Azure IOT kenarıyla IOT ağ geçidi veri dönüştürme | Microsoft Docs"
description: "Azure IOT kenarından özelleştirilmiş bir modül üzerinden algılayıcı veri biçimine dönüştürmek için IOT ağ geçidi kullanın."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT ağ geçidi veri dönüştürme, IOT ağ geçidi veri dönüştürme"
ms.assetid: 75f2573d-500b-4405-bff7-61021c4c3500
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/25/2017
ms.author: xshi
ms.openlocfilehash: d5c735a4adbc59e9526ec4fd40720c5ec136d63d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-iot-gateway-for-sensor-data-transformation-with-azure-iot-edge"></a>Azure IOT kenarıyla algılayıcı veri dönüştürme için IOT ağ geçidi kullan

> [!NOTE]
> Bu öğretici başlamadan önce sırayla aşağıdaki dersleri tamamladınız emin olun:
> * [Intel NUC cihazını IoT ağ geçidi olarak ayarlama](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
> * [IOT ağ geçidi bulut - Azure IOT Hub'ına SensorTag şeyler bağlamak için kullanın](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

Buluta göndermeden önce toplanan verileri işlemek için bir IOT ağ geçidi tek amacı budur. Azure IOT sınır olan oluşturulur ve veri işleme iş akışı oluşturmak için birleştirilmiş modüller tanıtır. Bir modül bir ileti alır, üzerinde bazı eylemleri gerçekleştirir ve sonra taşıyın diğer modüllerin işlemesi için.

## <a name="what-you-learn"></a>Öğrenecekleriniz

İletileri SensorTag farklı bir biçime dönüştürmek için bir modül oluşturmayı öğrenin.

## <a name="what-you-do"></a>Neler

* Alınan ileti .json biçime dönüştürmek üzere modül oluşturun.
* Modül derleyin.
* Modül Azure IOT kenarından bırak örnek uygulamaya ekleyin.
* Örnek uygulamayı çalıştırın.

## <a name="what-you-need"></a>Ne gerekiyor

* Aşağıdaki öğreticiler sırayla tamamlandı:
  * [Intel NUC cihazını IoT ağ geçidi olarak ayarlama](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
  * [IOT ağ geçidi bulut - Azure IOT Hub'ına SensorTag şeyler bağlamak için kullanın](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)
* Ana bilgisayar üzerinde çalışan bir SSH istemcisi. Putty'yi Windows önerilir. Linux ve macOS zaten bir SSH istemcisi ile gelir.
* IP adresi ve kullanıcı adı ve SSH istemciden ağ geçidi erişim için parola.
* Bir Internet bağlantısı.

## <a name="create-a-module"></a>Bir modül oluşturma

1. Ana bilgisayarda SSH istemcisi çalıştırın ve IOT ağ geçidi bağlanın.
1. Aşağıdaki komutları çalıştırarak github'dan dönüştürme modülü giriş dizinine IOT ağ geçidi'nin kaynak dosyalarını kopyalayın:

   ```bash
   cd ~
   git clone https://github.com/Azure-Samples/iot-hub-c-intel-nuc-gateway-customized-module.git
   ```

   C programlama dilinde yazılmış bir yerel Azure kenar modül budur. Modülün aşağıdaki bir alınan ileti biçimi dönüştürür:

   ```json
   {"deviceId": "Intel NUC Gateway", "messageId": 0, "temperature": 0.0}
   ```

## <a name="compile-the-module"></a>Modül derleme

Modül derlemek için aşağıdaki komutları çalıştırın:

```bash
cd iot-hub-c-intel-nuc-gateway-customized-module/my_module
# change the build script runnable
chmod 777 build.sh
# remove the invalid windows character
sed -i -e "s/\r$//" build.sh
# run the build shell script
./build.sh
```

Size bir `libmy_module.so` derleme tamamlandıktan sonra dosya. Bu dosyanın mutlak yolu not edin.

## <a name="add-the-module-to-the-ble-sample-application"></a>Modül bırak örnek uygulamaya ekleyin

1. Aşağıdaki komutu çalıştırarak örnekleri klasöre gidin:

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples
   ```

1. Aşağıdaki komutu çalıştırarak yapılandırma dosyasını açın:

   ```bash
   vi ble_gateway.json
   ```

1. Aşağıdaki kodu ekleyerek bir modül ekleyin `modules` bölümü.

   ```json
   {
       "name": "MyModule",
       "loader": {
           "name": "native",
           "entrypoint":{
               "module.path": "[Your libmy_module.so path]"
               }
            },
        "args": null
    },
    ```

1. Değiştir `[Your libmy_module.so path]` libmy_module.so ' mutlak yoluna sahip Code ' dosyası.
1. Kodla `links` aşağıdaki bir bölümle:

   ```json
   {
       "source": "SensorTag",
       "sink": "MyModule"
   },
   {
       "source": "MyModule",
       "sink": "mapping"
   }
   ```

1. Tuşuna `ESC`ve ardından `:wq` dosyayı kaydetmek için.

## <a name="run-the-sample-application"></a>Örnek uygulamayı çalıştırın

1. SensorTag açma.
1. Aşağıdaki komutu çalıştırarak SSL_CERT_FILE ortam değişkeni ayarlayın:

   ```bash
   export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt
   ```

1. Örnek uygulama, aşağıdaki komutu çalıştırarak eklenen modülüyle çalıştırın:

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a>Sonraki adımlar

Olduğunuz başarıyla IOT ağ geçidi ileti SensorTag .json biçime dönüştürmek için kullanın.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
