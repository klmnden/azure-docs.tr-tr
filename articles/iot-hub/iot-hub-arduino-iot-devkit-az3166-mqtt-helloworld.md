---
title: Azure MQTT istemci kitaplığı kullanılarak bir MQTT sunucusuna ileti gönderme | Microsoft Docs.
description: DevKit bir MQTT sunucuya iletileri göndermek için bir istemci olarak kullanın.
services: iot-hub
documentationcenter: ''
author: liydu
manager: timlt
tags: ''
keywords: ''
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/02/2018
ms.author: liydu
ms.openlocfilehash: 13b1c5b9ae05a6c2d11420812efc1af17912aa28
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="send-messages-to-an-mqtt-server"></a>Bir MQTT sunucusuna ileti gönderme

Nesnelerin interneti (IOT) sistemleri genellikle aralıklı, zayıf kalitesiyle mücadele etmek veya internet bağlantısı yavaş. MQTT böyle zorluklar aklınızda ile geliştirilen bir makine makine (M2M) bağlantısı iletişim kuralıdır. 

Burada kullanılan MQTT istemci kitaplığının parçasıdır [Eclipse Paho](http://www.eclipse.org/paho/) projesi aktarım birden çok yol MQTT kullanmak için API'ler sağlar.

## <a name="what-you-learn"></a>Öğrenecekleriniz

Bu projede, öğreneceksiniz:
- Bir MQTT Aracısı iletileri göndermek için MQTT istemci kitaplığını kullanma
- MXChip IOT DevKit MQTT istemci olarak yapılandırılır.

## <a name="what-you-need"></a>Ne gerekiyor

Son [Getting Started Guide](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) için:

* Wi-Fi bağlı, DevKit sahip
* Geliştirme ortamını hazırlama

## <a name="open-the-project-folder"></a>Proje klasörünü açın

1. Zaten bağlıysa, bilgisayarınızdan DevKit kesin.

2. VS Code'u başlatın.

3. DevKit bilgisayarınıza bağlayın.

## <a name="open-the-mqttclient-sample"></a>MQTTClient örneği açın

Sol tarafta genişletin **ARDUINO ÖRNEKLER** bölümünde **MXCHIP AZ3166 örnekler > MQTT**seçip **MQTTClient**. Bu proje klasöründe ile yeni bir VS Code penceresi açar.

> [!NOTE]
> Örnek komut paletinden da açabilirsiniz. Kullanım `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) komutu paletini açmak için şunu yazın **Arduino**ve ardından bulmak ve seçmek **Arduino: örnekler**.

## <a name="build-and-upload-the-arduino-sketch-to-the-devkit"></a>Derleme ve Arduino taslak DevKit karşıya yükle

Tür `Ctrl+P` (macOS: `Cmd+P`) çalıştırmak için `task device-upload`. Karşıya yükleme tamamlandığında, DevKit yeniden başlatır ve taslak çalıştırır.

![cihaz karşıya yükleme](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/device-upload.jpg)

> [!NOTE]
> Alabileceğiniz bir "hata: AZ3166: Bilinmeyen Paket" hata iletisi. Bu hata, Panosu paket dizinini doğru yenilenmemiş oluşur. Bu hatayı gidermek için bu başvuru [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

## <a name="test-the-project"></a>Projeyi test

VS Code'da açın ve seri İzleyicisi ayarlamak için aşağıdaki yordamı izleyin:

1. Tıklatın `COM[X]` sağ COM bağlantı noktası ile ayarlamak için durum çubuğunda word `STMicroelectronics`: ![kümesi com bağlantı](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/set-com-port.jpg)

2. Seri İzleyicisi'ni açmak için durum çubuğunda güç Tak simgesine tıklayın: ![seri İzleyicisi](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/serial-monitor.jpg)
  
3. Durum çubuğu Baud hızı temsil eden sayı tıklatın ve ayarlamak `115200`: ![kümesi baud hızı](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/set-baud-rate.jpg)

Seri İzleyicisi örnek taslak tarafından gönderilen tüm iletileri görüntüler. Taslak DevKit Wi-Fi bağlanır. Wi-Fi bağlantısı başarılı olduktan sonra taslak MQTT aracısı için bir ileti gönderir. Bundan sonra örnek art arda sırasıyla QoS 0 ve QoS 1 kullanarak iki "iot.eclipse.org" iletileri gönderir.

![seri çıkış](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/serial-output.jpg)

## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Sorunlarla karşılaşırsanız, başvurmak [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) veya aşağıdaki kanallar kullanarak bağlan:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="see-also"></a>Ayrıca bkz.

* [Bulutta Azure IOT Hub'ına IOT DevKit AZ3166 Bağlan]({{"/docs/getting-started/" | absolute_url }})
* [Sallama, Tweet için sallama]({{"/docs/projects/shake-shake/" | absolute_url }})

## <a name="next-steps"></a>Sonraki adımlar

MXChip IOT DevKit MQTT istemci olarak yapılandırmak ve bir MQTT Aracısı iletileri göndermek için MQTT istemci kitaplığını kullanma öğrendiniz, önerilen sonraki adımlar şunlardır:

* [Azure IOT Paketi'ne Genel Bakış](https://docs.microsoft.com/azure/iot-suite/)
* [Microsoft IoT Central uygulamanıza bir MXChip IOT DevKit cihazı bağlayın](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)
