---
title: Linux'ta Azure IoT Edge benzetimi | Microsoft Docs
description: Azure IoT Edge çalışma zamanını Linux'ta bir simülasyon cihazına yükleme ve ilk modülünüzü dağıtma
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 01/11/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 0b8b2658af9173cea6a7cdcb0147c7b0dc13a455
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631005"
---
# <a name="deploy-azure-iot-edge-on-a-simulated-device-in-linux-or-macos---preview"></a>Azure IoT Edge'i Linux veya MacOS'ta bir simülasyon cihazına dağıtma - önizleme

Azure IoT Edge, tüm verilerinizi buluta göndermek zorunda kalmak yerine analiz ve veri işlemeyi cihazlarınızda gerçekleştirmenizi sağlar. IoT Edge öğreticileri, Azure hizmetlerinden veya özel koddan geliştirilmiş farklı tür modülleri dağıtmayı gösterir, ancak önce test etmek için bir cihaz gerekir. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

1. IoT Hub'ı oluşturma
2. IoT Edge cihazı kaydetme
3. IoT Edge çalışma zamanını başlatma
4. Modül dağıtma

![Öğretici mimarisi][2]

Bu öğreticide oluşturduğunuz benzetimli cihaz, sıcaklık, nem ve basınç verileri üreten bir monitördür. Diğer Azure IoT Edge öğreticileri, burada iş içgörüsü için verileri analiz eden modüller dağıtarak yaptığınız çalışmayı temel alır. 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide bilgisayarınız veya sanal makineniz bir Nesnelerin İnterneti cihazı olarak kullanılır. Cihazınızı bir IoT Edge cihazına dönüştürmek için aşağıdaki hizmetler gerekir:

* IoT Edge çalışma zamanını yüklemek için Python pip.
   * Linux: `sudo apt-get install python-pip`.
     * _Belirli dağıtımlarda (örn. Raspbian), bazı pip paketlerini yükseltmeniz ve ek bağımlılıklar yüklemeniz de gerekebileceğini unutmayın:_
     ```
     sudo pip install --upgrade setuptools pip
     
     sudo apt-get install python2.7-dev libffi-dev libssl-dev
     ```
   * MacOS: `sudo easy_install pip`.
* IoT Edge modüllerini çalıştırmak için Docker
   * [Windows için Docker][lnk-docker-ubuntu] yükleyin ve çalıştığından emin olun. 
   * [Mac için Docker][lnk-docker-mac] yükleyin ve çalıştığından emin olun. 

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

IoT Hub'ınızı oluşturarak öğreticiyi başlatın.
![IoT Hub'ını oluşturma][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>IoT Edge cihazı kaydetme

Yeni oluşturulan IoT Hub'ına bir IoT Edge cihazı kaydedin.
![Cihaz kaydetme][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="install-and-start-the-iot-edge-runtime"></a>IoT Edge çalışma zamanını yükleme ve başlatma

Azure IoT Edge çalışma zamanını cihazınıza yükleyin ve başlatın. 
![Cihaz kaydetme][5]

IoT Edge çalışma zamanı tüm IoT Edge cihazlarına dağıtılır. İki modülden oluşur. **IoT Edge aracısı**, IoT Edge cihazındaki modüllerin dağıtımını ve izlenmesini kolaylaştırır. **IoT Edge hub'ı** IoT Edge cihazındaki modüller ve cihaz ile IoT Hub'ı arasındaki iletişimi yönetir. Çalışma zamanını yeni cihazınızda yapılandırdığınızda, ilk önce yalnızca IoT Edge aracısı başlayacaktır. Bir modül dağıttığınızda IoT Edge hub'ı daha sonra gelir. 

IoT Edge cihazını çalıştıracağınız makinede, IoT Edge denetim betiğini indirin:
```cmd
sudo pip install -U azure-iot-edge-runtime-ctl
```

Çalışma zamanını önceki bölümdeki IoT Edge cihaz bağlantı dizesi ile yapılandırın:
```cmd
sudo iotedgectl setup --connection-string "{device connection string}" --nopass
```

Çalışma zamanını başlatın:
```cmd
sudo iotedgectl start
```

IoT Edge aracısının bir modül olarak çalıştığından emin olmak için Docker’ı denetleyin:
```cmd
sudo docker ps
```

![Docker’da edgeAgent’a bakın](./media/tutorial-simulate-device-linux/docker-ps.png)

## <a name="deploy-a-module"></a>Modül dağıtma

Azure IoT Edge cihazınızı, IoT Hub'ına telemetri verileri gönderecek bir modül dağıtmak için buluttan yönetin.
![Cihaz kaydetme][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Bu öğreticide yeni bir IoT Edge cihazı oluşturdunuz ve üzerine IoT Edge çalışma zamanını yüklediniz. Ardından, cihazda bir değişiklik yapmak zorunda kalmadan çalışacak bir IoT Edge modülünü göndermek için Azure portalını kullandınız. Bu örnekte gönderdiğiniz modül öğreticiler için kullanabileceğiniz ortam verilerini oluşturmaktadır. 

Benzetimli cihazınızı çalıştıran bilgisayarda yeniden komut istemini açın. Buluttan dağıtılan modülün IoT Edge cihazınızda çalıştığından emin olun:

```cmd
sudo docker ps
```

![Cihazınızda üç modül görüntüleme](./media/tutorial-simulate-device-linux/docker-ps2.png)

tempSensor modülünden buluta gönderilen iletileri görüntüleyin:

```cmd
sudo docker logs -f tempSensor
```

![Verileri modülünüzden görüntüleme](./media/tutorial-simulate-device-linux/docker-logs.png)

Ayrıca, [IoT Hub gezgini aracını][lnk-iothub-explorer] kullanarak cihazın gönderdiği telemetriyi görüntüleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide yeni bir IoT Edge cihazı oluşturdunuz ve cihaza kod dağıtmak için Azure IoT Edge bulut arabirimini kullandınız. Artık ortamı hakkında ham veri üreten benzetimli bir cihazınız var. 

Bu öğretici, diğer tüm IoT Edge öğreticilerinin önkoşuludur. Azure IoT Edge'in bu verileri Edge'de iş içgörüsüne dönüştürmenize nasıl yardımcı olabileceğini öğrenmek için diğer öğreticilere devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Modül olarak C# kodu geliştirme ve dağıtma](tutorial-csharp-module.md)


<!-- Images -->
[1]: ./media/tutorial-install-iot-edge/view-module.png
[2]: ./media/tutorial-install-iot-edge/install-edge-full.png
[3]: ./media/tutorial-install-iot-edge/create-iot-hub.png
[4]: ./media/tutorial-install-iot-edge/register-device.png
[5]: ./media/tutorial-install-iot-edge/start-runtime.png
[6]: ./media/tutorial-install-iot-edge/deploy-module.png

<!-- Links -->
[lnk-docker-ubuntu]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/ 
[lnk-docker-mac]: https://docs.docker.com/docker-for-mac/install/
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
