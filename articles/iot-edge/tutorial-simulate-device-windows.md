---
title: Windows'da Azure IoT Edge benzetimi | Microsoft Docs
description: Azure IoT Edge çalışma zamanını Windows'da benzetimli bir cihaza yükleme ve ilk modülünüzü dağıtma
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 11/16/2017
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 7ad99a49a578de4997a2d76d48da33aba6847f3c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631199"
---
# <a name="deploy-azure-iot-edge-on-a-simulated-device-in-windows----preview"></a>Azure IoT Edge'i Windows'da benzetimli bir cihazda dağıtma -  önizleme

Azure IoT Edge, tüm verilerinizi buluta göndermek zorunda kalmak yerine analiz ve veri işlemeyi cihazlarınızda gerçekleştirmenizi sağlar. IoT Edge öğreticileri, Azure hizmetlerinden veya özel koddan geliştirilmiş farklı tür modülleri dağıtmayı gösterir, ancak önce test etmek için bir cihaz gerekir. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

1. IoT Hub'ı oluşturma
2. IoT Edge cihazı kaydetme
3. IoT Edge çalışma zamanını başlatma
4. Modül dağıtma

![Öğretici mimarisi][2]

Bu öğreticide oluşturduğunuz benzetimli cihaz, bir rüzgar türbininde sıcaklık, nem ve basınç verileri üreten bir monitördür. Bu verilerle ilgilenmenizin nedeni, türbinlerinizin hava koşullarına bağlı olarak farklı verimlilik düzeylerinde çalışmasıdır. Diğer Azure IoT Edge öğreticileri, burada iş içgörüsü için verileri analiz eden modüller dağıtarak burada yaptığınız çalışmayı temel almaktadır. 

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici, bir Nesnelerin İnterneti cihazının benzetimi için Windows çalıştıran bir bilgisayar veya bir sanal makine kullanmakta olduğunuzu varsayar. 

>[!TIP]
>Windows'u sanal bir makinede çalıştırıyorsanız, [iç içe sanallaştırmayı][lnk-nested] etkinleştirin ve en az 2 GB bellek ayırın. 

1. Desteklenen bir Windows sürümü kullandığınızdan emin olun:
   * Windows 10 
   * Windows Server
2. [Docker for Windows] [ lnk-docker] yükleyin ve çalıştığından emin olun.
3. [Windows'da Python] [ lnk-python] yükleyin ve PIP komutunu kullanabildiğinizden emin olun. Bu öğretici Python'ın 2.7.9 ve 3.5.4 veya üzeri sürümleri ile test edilmiştir.  
4. IoT Edge kontrol betiğini indirmek için aşağıdaki komutu çalıştırın.

   ```cmd
   pip install -U azure-iot-edge-runtime-ctl
   ```

> [!NOTE]
> Azure IoT Edge, Windows veya Linux kapsayıcılarını çalıştırabilir. Aşağıdaki Windows sürümlerinden birini kullanıyorsanız, Windows kapsayıcılarını kullanabilirsiniz:
>    * Windows 10 Fall Creators Update
>    * Windows Server 1709 (Derleme 16299)
>    * x64 tabanlı bir cihazda Windows IoT Core (Derleme 16299)
>
> Windows IoT Core'da, [Windows IoT Core'a IoT Edge çalışma zamanını yükleme][lnk-install-iotcore] yönergelerini izleyin. Diğer durumlarda [Docker'ı Windows kapsayıcılarını kullanmak üzere yapılandırmanız][lnk-docker-containers] yeterlidir. Önkoşullarınızı doğrulamak için aşağıdaki komutu kullanın:
>    ```powershell
>    Invoke-Expression (Invoke-WebRequest -useb https://aka.ms/iotedgewin)
>    ```


## <a name="create-an-iot-hub"></a>IoT hub oluşturma

IoT Hub'ınızı oluşturarak öğreticiyi başlatın.
![IoT Hub'ı oluşturma][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>IoT Edge cihazı kaydetme

Yeni oluşturulan IoT Hub'a bir IoT Edge cihazı kaydetme.
![Cihaz kaydetme][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="configure-the-iot-edge-runtime"></a>IoT Edge çalışma zamanını yapılandırma

Azure IoT Edge çalışma zamanını cihazınıza yükleyin ve başlatın. 
![Cihaz kaydetme][5]

IoT Edge çalışma zamanı tüm IoT Edge cihazlarına dağıtılır. İki modülden oluşur. **IoT Edge aracısı**, IoT Edge cihazındaki modüllerin dağıtımını ve izlenmesini kolaylaştırır. **IoT Edge hub'ı** IoT Edge cihazındaki modüller ve cihaz ile IoT Hub'ı arasındaki iletişimi yönetir. Çalışma zamanını yeni cihazınızda yapılandırdığınızda, ilk önce yalnızca IoT Edge aracısı başlayacaktır. Bir modül dağıttığınızda IoT Edge hub'ı daha sonra gelir. 


Çalışma zamanını önceki bölümdeki IoT Edge cihaz bağlantı dizesi ile yapılandırın.

```cmd
iotedgectl setup --connection-string "{device connection string}" --nopass
```

Çalışma zamanını başlatın.

```cmd
iotedgectl start
```

IoT Edge arasının bir modül olarak çalıştığından emin olmak için Docker'ı denetleyin.

```cmd
docker ps
```

![Docker'da edgeAgent'a bakın](./media/tutorial-simulate-device-windows/docker-ps.png)

## <a name="deploy-a-module"></a>Modül dağıtma

Azure IoT Edge cihazınızı, IoT Hub'ına telemetri verileri gönderecek bir modül dağıtmak için buluttan yönetin.
![Cihaz kaydetme][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]


## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Bu öğreticide yeni bir IoT Edge cihazı oluşturdunuz ve üzerine IoT Edge çalışma zamanını yüklediniz. Ardından, cihazda bir değişiklik yapmak zorunda kalmadan çalışacak bir IoT Edge modülünü göndermek için Azure portalı kullandınız. Bu örnekte gönderdiğiniz modül öğreticiler için kullanabileceğiniz ortam verileri oluşturmaktadır. 

Benzetimli cihazınızı çalıştıran bilgisayarda yeniden komut istemini açın. Buluttan dağıtılan modülün IoT Edge cihazınızda çalıştığından emin olun. 

```cmd
docker ps
```

![Cihazınızda üç modül görüntüleme](./media/tutorial-simulate-device-windows/docker-ps2.png)

tempSensor modülünden buluta gönderilen iletileri görüntüleyin. 

```cmd
docker logs -f tempSensor
```

![Verileri modülünüzden görüntüleme](./media/tutorial-simulate-device-windows/docker-logs.png)

Ayrıca, [IoT Hub Gezgini aracını][lnk-iothub-explorer] kullanarak cihazın gönderdiği telemetriyi görüntüleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide yeni bir IoT Edge cihazı oluşturdunuz ve cihaza kod dağıtmak için Azure IoT Edge bulut arabirimini kullandınız. Artık ortamı hakkında ham veri üreten benzetimli bir cihazınız var. 

Bu öğretici diğer tüm IoT Edge öğreticilerinin önkoşuludur. Azure IoT Edge'in bu verileri Edge'de iş içgörüsüne dönüştürmenize nasıl yardımcı olabileceğini öğrenmek için diğer öğreticilere devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Modül olarak C# kodu geliştirme ve dağıtma](tutorial-csharp-module.md)

<!-- Images -->
[2]: ./media/tutorial-install-iot-edge/install-edge-full.png
[3]: ./media/tutorial-install-iot-edge/create-iot-hub.png
[4]: ./media/tutorial-install-iot-edge/register-device.png
[5]: ./media/tutorial-install-iot-edge/start-runtime.png
[6]: ./media/tutorial-install-iot-edge/deploy-module.png

<!-- Links -->
[lnk-nested]: https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization
[lnk-docker]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-python]: https://www.python.org/downloads/
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-install-iotcore]: how-to-install-iot-core.md