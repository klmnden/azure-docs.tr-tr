---
title: Azure MQTT istemci kitaplığını kullanarak bir MQTT sunucusuna ileti gönderme | Microsoft Docs
description: Bir MQTT sunucusuna ileti göndermek için bir istemci olarak DevKit kullanma
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/02/2018
ms.author: liydu
ms.openlocfilehash: 60520f5a72fd7e27d4ea64ac76511a00a727426e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61369356"
---
# <a name="send-messages-to-an-mqtt-server"></a>Bir MQTT sunucusuna ileti gönderme

Nesnelerin interneti (IOT) sistemleri genellikle aralıklı ve düşük kaliteli ile baş veya internet bağlantısı yavaş. Bu zorluklar aklınızda ile geliştirilmiş bir makineden makineye (M2M) bağlantısı protokol MQTT olarak belirlenmiştir. 

Burada kullanılan MQTT istemci kitaplığının parçasıdır [Eclipse Paho](https://www.eclipse.org/paho/) taşıma birden çok yol MQTT kullanmak için API'leri sağlayan bir proje.

## <a name="what-you-learn"></a>Öğrenecekleriniz

Bu projede şunları öğrenirsiniz:
- MQTT aracıya bir ileti göndermek için MQTT istemci kitaplığını kullanma
- MXChip IOT Devkit'inize bir MQTT istemci olarak yapılandırılır.

## <a name="what-you-need"></a>Ne gerekiyor

Son [Başlangıç Kılavuzu](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) için:

* DevKit Wi-Fi'a bağlandıktan
* Geliştirme ortamını hazırlama

## <a name="open-the-project-folder"></a>Proje klasörünü açın

1. DevKit zaten varsa bilgisayarınıza bağlanmak, bu bağlantıyı kes.

2. VS Code'u başlatın.

3. DevKit bilgisayarınıza bağlayın.

## <a name="open-the-mqttclient-sample"></a>MQTTClient örneği açın

Sol tarafındaki genişletme **ARDUINO ÖRNEKLER** bölümünde **MXCHIP AZ3166 için örnek > MQTT**seçip **MQTTClient**. Bu proje klasöründe yeni bir VS Code penceresinin açılır.

> [!NOTE]
> Komut paletinden örnek de açabilirsiniz. Kullanım `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) komut paletini açmak için şunu yazın **Arduino**ve ardından bulmak ve seçmek **Arduino: Örnekler**.

## <a name="build-and-upload-the-arduino-sketch-to-the-devkit"></a>Derleme ve Arduino taslak Devkit'e karşıya yükleyin

Tür `Ctrl+P` (macOS: `Cmd+P`) çalıştırılacak `task device-upload`. Karşıya yükleme tamamlandığında, DevKit yeniden başlatır ve taslak çalıştırır.

![cihaz karşıya yükleme](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/device-upload.jpg)

> [!NOTE]
> Alabileceğiniz bir "hata: AZ3166: Bilinmeyen Paket"hata iletisi. Pano paket dizinini doğru şekilde yenilenmez bu hata oluşur. Bu hatayı gidermek için başvurmak [geliştirme IOT DevKit SSS bölümünü](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

## <a name="test-the-project"></a>Test projesi

VS Code'da açın ve seri İzleyicisi ayarlamak için aşağıdaki yordamı izleyin:

1. Tıklayın `COM[X]` doğru COM bağlantı noktası ile ayarlamak için durum çubuğuna word `STMicroelectronics`: ![com-kümesi-bağlantı noktası](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/set-com-port.jpg)

2. Seri İzleyicisi'ni açmak için durum çubuğunu power Tak simgesine tıklayın: ![seri İzleyicisi](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/serial-monitor.jpg)
  
3. Durum çubuğunda Baud hızı temsil eden bir sayıya tıklayın ve değerini `115200`: ![kümesi baud hızı](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/set-baud-rate.jpg)

Seri İzleyici örnek taslak tarafından gönderilen tüm iletileri görüntüler. Taslak, Wi-Fi DevKit bağlanır. Wi-Fi bağlantısı başarılı olduktan sonra taslak MQTT aracıya bir ileti gönderir. Bundan sonra örnek art arda QoS 0 ve 1 QoS, sırasıyla kullanarak iki "iot.eclipse.org" iletileri gönderir.

![seri çıkış](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/serial-output.jpg)

## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Sorunlarla karşılaşırsanız, başvurmak [IOT DevKit SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) veya aşağıdaki kanalları kullanarak bağlanın:

* [Gitter.im](https://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="see-also"></a>Ayrıca bkz.

* [Azure IOT hub'ı bulutta IOT DevKit AZ3166 bağlanma](iot-hub-arduino-iot-devkit-az3166-get-started.md)
* [Sallama, bir Tweet için sallayın](iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message.md)

## <a name="next-steps"></a>Sonraki adımlar

MXChip IOT Devkit'inize bir MQTT istemci olarak yapılandırılır ve MQTT aracıya bir ileti göndermek için MQTT istemci kitaplığı öğrendiniz, önerilen sonraki adımlar şunlardır:

* [Azure IOT Uzaktan izleme çözüm hızlandırıcısına genel bakış](https://docs.microsoft.com/azure/iot-suite/)
* [Azure IOT Central uygulamanıza bir MXChip IOT DevKit cihazı bağlayın](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)
