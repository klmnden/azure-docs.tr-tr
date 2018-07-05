---
title: Linux'ta Azure IoT Edge benzetimi | Microsoft Docs
description: Bu hızlı başlangıçta, önceden derlenmiş kodu uzaktan bir IoT Edge cihazına dağıtmayı öğrenin.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/27/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 0e0d22b3363b00c81be5091fd12773f9e486c09e
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37099194"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-linux-x64-device"></a>Hızlı Başlangıç: Bir Linux x64 cihazına ilk IoT Edge modülünüzü dağıtma

Azure IoT Edge, tüm verilerinizi buluta göndermek zorunda kalmak yerine analiz ve veri işlemeyi cihazlarınızda gerçekleştirmenizi sağlar. IoT Edge öğreticileri farklı tür modülleri dağıtmayı gösterir, ancak önce test etmek için bir cihaz gerekir. 

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

1. Bir IoT Hub oluşturma.
2. Bir IoT Edge cihazını IoT hub'ınıza kaydetme.
3. IoT Edge çalışma zamanını başlatma.
4. Bir IoT Edge cihazına uzaktan modül dağıtma.

![Öğretici mimarisi][2]

Bu hızlı başlangıç, Linux bilgisayarınızı veya sanal makinenizi bir IoT Edge cihazına dönüştürür. Bu işlemin ardından Azure portalından cihazınıza bir modül dağıtabilirsiniz. Bu hızlı başlangıçta oluşturduğunuz modül; sıcaklık, nem ve basınç verileri üreten bir sensör simülasyonudur. Diğer Azure IoT Edge öğreticileri, burada iş içgörüsü için simülasyon verilerini analiz eden modüller dağıtarak yaptığınız çalışmayı temel alır. 

Etkin bir Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap][lnk-account] oluşturun.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Hızlı başlangıç adımlarına başlamak için Azure portalında IoT Hub'ınızı oluşturun.
![IoT Hub'ını oluşturma][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>IoT Edge cihazı kaydetme

Yeni oluşturulan IoT Hub'ına bir IoT Edge cihazı kaydedin.
![Cihaz kaydetme][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]


## <a name="install-and-start-the-iot-edge-runtime"></a>IoT Edge çalışma zamanını yükleme ve başlatma

Azure IoT Edge çalışma zamanını cihazınıza yükleyin ve başlatın. 
![Cihaz kaydetme][5]

IoT Edge çalışma zamanı tüm IoT Edge cihazlarına dağıtılır. Üç bileşeni vardır. **IoT Edge güvenlik daemon'u** bir Edge cihazı her başladığında çalışır ve IoT Edge aracısını çalıştırarak cihazı önyükler. **IoT Edge aracısı**, IoT Edge hub'ı dahil olmak üzere IoT Edge cihazındaki modüllerin dağıtımını ve izlenmesini kolaylaştırır. **IoT Edge hub'ı** IoT Edge cihazındaki modüller ve cihaz ile IoT Hub'ı arasındaki iletişimi yönetir. 

### <a name="register-your-device-to-use-the-software-repository"></a>Yazılım deposunu kullanmak için cihazınızı kaydetme

IoT Edge çalışma zamanını çalıştırmak için ihtiyacınız olan paketler, bir yazılım deposunda yönetilir. Bu depoya erişmek için IoT Edge cihazınızı yapılandırın. 

Bu bölümdeki adımlar **Ubuntu 16.04** çalıştıran cihazlara yöneliktir. Diğer Linux sürümlerinde yazılım deposuna erişmek için bkz. [Azure IoT Edge çalışma zamanını Linux (x64) üzerine yükleme](how-to-install-iot-edge-linux.md) veya [Azure IoT Edge çalışma zamanını Linux (ARM32v7/armhf) üzerine yükleme](how-to-install-iot-edge-linux-arm.md).

1. IoT Edge cihazı olarak kullandığınız makineye depo yapılandırmasını yükleyin.

   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

2. Depoya erişmek için bir ortak anahtar yükleyin.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

### <a name="install-a-container-runtime"></a>Kapsayıcı çalışma zamanı yükleme

IoT Edge çalışma zamanı, kapsayıcılardan oluşan bir kümedir ve IoT Edge cihazınıza dağıttığınız mantık, kapsayıcı olarak paketlenir. Cihazlarınızı bu bileşenlere hazır hale getirmek için bir kapsayıcı çalışma zamanı yükleyin.

**apt-get** komutunu kullanın.

   ```bash
   sudo apt-get update
   ```

Kapsayıcı çalışma zamanı olan Moby uygulamasını ve CLI komutlarını yükleyin. 

   ```bash
   sudo apt-get install moby-engine
   sudo apt-get install moby-cli   
   ```

### <a name="install-and-configure-the-iot-edge-security-daemon"></a>IoT Edge güvenlik daemon'unu yükleme ve yapılandırma

Güvenlik daemon'u sistem hizmeti olarak yüklenir ve bu sayede IoT Edge çalışma zamanı cihaz her başlatıldığında çalışır. Yükleme aynı zamanda güvenlik daemon'unun cihazın donanım güvenliğiyle etkileşim kurmasını sağlayan **hsmlib** uygulamasının bir sürümünü de içerir. 

1. IoT Edge Güvenlik Daemon'unu indirin ve yükleyin. 

   ```bash
   sudo apt-get update
   sudo apt-get install iotedge
   ```

2. IoT Edge yapılandırma dosyasını açın. Korumalı bir dosya olduğu için yükseltilmiş ayrıcalıklarla erişmeniz gerekir.
   
   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

3. Cihazınızı kaydederken kopyaladığınız IoT Edge cihazı bağlantı dizesini ekleyin. **device_connection_string** değişkeninin değerini bu hızlı başlangıcın önceki bölümlerinde kopyaladığınız değerle değiştirin.

4. Edge Güvenlik Daemon'unu yeniden başlatın:

   ```bash
   sudo systemctl restart iotedge
   ```

5. Edge Güvenlik Daemon'unun sistem hizmeti olarak çalışıp çalışmadığını kontrol edin:

   ```bash
   sudo systemctl status iotedge
   ```

   ![Edge Daemon'unun sistem hizmeti olarak çalıştığını görün](./media/quickstart-linux/iotedged-running.png)

   Aşağıdaki komutu çalıştırarak Edge Güvenlik Daemon'u günlük dosyalarını da görebilirsiniz:

   ```bash
   journalctl -u iotedge
   ```

6. Cihazınızda çalışan modülleri görüntüleyin: 

   ```bash
   sudo iotedge list
   ```
Oturum kapatma ve açma döngüsünden sonra yukarıdaki komut için *sudo* kullanılması gerekmez.

   ![Cihazınızda bir modülü görüntüleme](./media/quickstart-linux/iotedge-list-1.png)

## <a name="deploy-a-module"></a>Modül dağıtma

Azure IoT Edge cihazınızı, IoT Hub'ına telemetri verileri gönderecek bir modül dağıtmak için buluttan yönetin.
![Cihaz kaydetme][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]


## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Bu hızlı başlangıçta, yeni bir IoT Edge cihazı oluşturdunuz ve üzerine IoT Edge çalışma zamanını yüklediniz. Ardından, cihazda bir değişiklik yapmak zorunda kalmadan çalışacak bir IoT Edge modülünü göndermek için Azure portalını kullandınız. Bu örnekte gönderdiğiniz modül öğreticiler için kullanabileceğiniz ortam verilerini oluşturmaktadır. 

Benzetimli cihazınızı çalıştıran bilgisayarda yeniden komut istemini açın. Buluttan dağıtılan modülün IoT Edge cihazınızda çalıştığından emin olun:

   ```bash
   sudo iotedge list
   ```
Oturum kapatma ve açma döngüsünden sonra yukarıdaki komut için *sudo* kullanılması gerekmez.

   ![Cihazınızda üç modül görüntüleme](./media/quickstart-linux/iotedge-list-2.png)

tempSensor modülünden gönderilen iletileri görüntüleyin:

  ```bash
   sudo iotedge logs tempSensor -f 
   ```

Oturum kapatma ve açma döngüsünden sonra yukarıdaki komut için *sudo* kullanılması gerekmez.

![Verileri modülünüzden görüntüleme](./media/quickstart-linux/iotedge-logs.png)

Günlüğün son satırında `Using transport Mqtt_Tcp_Only` varsa sıcaklık sensörü modülü Edge Hub'ına bağlanmayı bekliyor olabilir. Modülü sonlandırıp Edge Aracısı tarafından yeniden başlatılmasını sağlayın. `sudo docker stop tempSensor` komutuyla sonlandırabilirsiniz.

[IoT Hub gezginini][lnk-iothub-explorer] veya [Visual Studio Code için Azure IoT Toolkit uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) kullanarak cihazın gönderdiği telemetri verilerini de görüntüleyebilirsiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

IoT Edge öğreticilerine devam etmek istiyorsanız bu hızlı başlangıçta kaydettiğiniz ve ayarladığınız cihazı kullanabilirsiniz. Cihazınızdaki yüklemeleri kaldırmak istiyorsanız aşağıdaki komutları kullanın.  

IoT Edge çalışma zamanını kaldırın.

   ```bash
   sudo apt-get remove --purge iotedge
   ```

Cihazınızda oluşturulan kapsayıcıları silin. 

   ```bash
   sudo docker rm -f $(sudo docker ps -aq)
   ```

Kapsayıcı çalışma zamanını kaldırın.

   ```bash
   sudo apt-get remove --purge moby
   ```

Bu hızlı başlangıçta oluşturduğunuz Azure IoT hub'ına veya IoT Edge cihazına ihtiyacınız kalmadığında Azure portalından silebilirsiniz. IoT hub'ınızın genel bakış sayfasına gidip **Sil**'i seçin. 

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta yeni bir IoT Edge cihazı oluşturdunuz ve cihaza kod dağıtmak için Azure IoT Edge bulut arabirimini kullandınız. Artık ortamı hakkında ham veri üreten benzetimli bir cihazınız var. 

Bu hızlı başlangıç, tüm IoT Edge öğreticilerinin önkoşuludur. Azure IoT Edge'in bu verileri Edge'de iş içgörüsüne dönüştürmenize nasıl yardımcı olabileceğini öğrenmek için öğreticilerden herhangi biriyle devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Bir Azure İşlevi kullanarak sensör verilerini filtreleme](tutorial-deploy-function.md)


<!-- Images -->
[1]: ./media/quickstart-linux/view-module.png
[2]: ./media/quickstart-linux/install-edge-full.png
[3]: ./media/quickstart-linux/create-iot-hub.png
[4]: ./media/quickstart-linux/register-device.png
[5]: ./media/quickstart-linux/start-runtime.png
[6]: ./media/quickstart-linux/deploy-module.png
[7]: ./media/quickstart-linux/iotedged-running.png
[8]: ./media/tutorial-simulate-device-linux/running-modules.png
[9]: ./media/tutorial-simulate-device-linux/sensor-data.png

<!-- Links -->
[lnk-docker-ubuntu]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/ 
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
