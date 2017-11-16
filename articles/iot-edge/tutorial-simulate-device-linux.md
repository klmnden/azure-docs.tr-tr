---
title: "Linux Azure IOT kenarına benzetimini | Microsoft Docs"
description: "Linux sanal bir cihaz üzerinde Azure IOT kenar çalışma zamanı yükleme ve ilk modülünüzün dağıtma"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 10/05/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 041919fd729880d429e08d8942f8d1ee087ccf61
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-azure-iot-edge-on-a-simulated-device-in-linux---preview"></a>Linux sanal bir cihaz üzerinde Azure IOT kenar dağıtma - Önizleme

Azure IOT kenar, tüm verileri buluta itme zorunda kalmak yerine cihazlarınızda analizi ve veri işleme gerçekleştirmenizi sağlar. IOT kenar öğreticileri Azure hizmetlerine veya özel kod yerleşik modülleri, farklı türlerde dağıtmak nasıl gerçekleştirileceğini gösterir ancak önce test etmek için bir aygıt gerekir. 

Bu öğretici, algılayıcı verilerini oluşturan bir modül dağıtma sanal IOT kenar cihazı oluşturmada size yol gösterir. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

![Öğretici mimarisi][2]

Bu öğreticide oluşturduğunuz sanal cihaz sıcaklık ve nem Basıncı verileri oluşturan bir izleyicisidir. İşletme öngörüleri için verileri analiz modülleri dağıtarak yapılacağını burada iş bağlı diğer Azure IOT kenar öğreticileri oluşturun. 

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici, bir bilgisayar veya Linux çalıştıran sanal makine bir nesnelerin interneti aygıt benzetimini yapmak için kullanmakta olduğunuz olduğunu varsayar. Bir IOT sınır cihazı başarıyla dağıtmak için aşağıdaki hizmetler gerekir:

- [Linux için Docker yükleme] [ lnk-docker-ubuntu] ve emin olun çalıştığından. 
- Ubuntu, dahil olmak üzere çoğu Linux dağıtımları Python 2.7 yüklü zaten var. PIP yüklendiğinden emin olmak için aşağıdaki komutu kullanın: `sudo apt-get install python-pip`.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Öğretici, IOT Hub'ınızı oluşturarak başlayın.
![IOT Hub oluşturma][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>Bir IOT sınır cihazı kaydetme

Bir IOT sınır cihazı, yeni oluşturulan IOT Hub ile kaydedin.
![Bir cihaz kaydetme][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="install-and-start-the-iot-edge-runtime"></a>Yükleyin ve IOT kenar çalışma zamanı başlatın

Yükleyin ve Azure IOT kenar çalışma zamanı aygıtınızda başlatın. 
![Bir cihaz kaydetme][5]

IOT kenar çalışma zamanı, tüm IOT kenar aygıtlarda dağıtılır. İki modülden oluşur. İlk olarak, IOT kenar Aracısı dağıtımı ve IOT sınır cihazı modülleri izlenmesini kolaylaştırır. İkinci olarak, IOT kenar hub IOT sınır cihazı modülleri arasında ve cihaz IOT hub'ı arasındaki iletişim yönetir. 

Yükleme ve IOT kenar çalışma zamanı başlatmak için aşağıdaki adımları kullanın:

1. IOT sınır cihazı çalıştırdığı makinede IOT kenar denetim komut dosyasını indirin.

   ```
   sudo pip install -U azure-iot-edge-runtime-ctl
   ```

1. Çalışma zamanı IOT kenar cihaz bağlantı dizenizi önceki bölümdeki yapılandırın.

   ```
   sudo iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords
   ```

1. Çalışma zamanı başlatın.

   ```
   sudo iotedgectl start
   ```

1. Docker IOT kenar Aracısı'nı bir modül olarak çalışıp çalışmadığını denetleyin.

   ```
   sudo docker ps
   ```

## <a name="deploy-a-module"></a>Bir modül dağıtma

Azure IOT kenar Cihazınızı IOT Hub'ına telemetri verileri gönderecek bir modül dağıtmak için buluttan yönetin.
![Bir cihaz kaydetme][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Oluşturulan görünüm verileri

Bu hızlı başlangıç yeni bir IOT sınır cihazı oluşturan ve IOT kenar çalışma zamanı yüklü. Ardından, cihaz için değişiklik yapmak zorunda kalmadan cihazda çalıştırmak için bir IOT kenar modülü göndermek için Azure portal kullanılır. Bu durumda, gönderilen modülü öğreticileri için kullanabileceğiniz çevresel veri oluşturur. 

TempSensor modülünden gönderilen iletiler görüntüleyin:

```cmd/sh
docker logs -f tempSensor
```

Cihaz kullanarak göndermeyi telemetriyi de görüntüleyebilirsiniz [IOT hub'ı explorer aracı][lnk-iothub-explorer]. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, yeni bir IOT sınır cihazı oluşturulur ve Azure IOT kenar bulut arabirimi kodu cihaza dağıtmak için kullanılır. Artık, kendi ortamı hakkında ham verileri oluşturma bir sanal cihaz var. 

Bu öğretici, tüm diğer IOT kenar öğreticileri için önkoşuldur. Azure IOT kenar işletme öngörüleri kenarına bu verileri dönüştürmek nasıl yardımcı olabileceğini öğrenmek için diğer öğreticileri birini açın devam edebilirsiniz.

> [!div class="nextstepaction"]
> [C# kod bir modül olarak geliştirmek ve dağıtmak](tutorial-csharp-module.md)


<!-- Images -->
[1]: ./media/tutorial-install-iot-edge/view-module.png
[2]: ./media/tutorial-install-iot-edge/install-edge-full.png
[3]: ./media/tutorial-install-iot-edge/create-iot-hub.png
[4]: ./media/tutorial-install-iot-edge/register-device.png
[5]: ./media/tutorial-install-iot-edge/start-runtime.png
[6]: ./media/tutorial-install-iot-edge/deploy-module.png

<!-- Links -->
[lnk-docker-ubuntu]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/ 
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
