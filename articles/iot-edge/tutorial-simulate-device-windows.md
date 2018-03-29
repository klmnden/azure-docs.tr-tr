---
title: Windows Azure IOT kenarına benzetimini | Microsoft Docs
description: Sanal cihazda Windows Azure IOT kenar çalışma zamanı yükleme ve ilk modülünüzün dağıtma
services: iot-edge
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 11/16/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: ae974162a460289a34443879a9e78224684d94ed
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-azure-iot-edge-on-a-simulated-device-in-windows----preview"></a>Bir sanal cihaz Windows Azure IOT kenar dağıtın - Önizleme

Azure IOT kenar, tüm verileri buluta itme zorunda kalmak yerine cihazlarınızda analizi ve veri işleme gerçekleştirmenizi sağlar. IOT kenar öğreticileri Azure hizmetlerine veya özel kod yerleşik modülleri, farklı türlerde dağıtmak nasıl gerçekleştirileceğini gösterir ancak önce test etmek için bir aygıt gerekir. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

1. IOT Hub oluşturma
2. Bir IOT sınır cihazı kaydetme
3. IOT kenar çalışma zamanı Başlat
4. Bir modül dağıtma

![Öğretici mimarisi][2]

Bu öğreticide oluşturduğunuz benzetimli sıcaklık ve nem Basıncı verileri oluşturan Rüzgar Türbin monitörde aygıttır. Turbines verimlilik hava durumu koşullara bağlı olarak farklı düzeylerde gerçekleştirdiğinden bu veriyi ilginizi. İşletme öngörüleri için verileri analiz modülleri dağıtarak yapılacağını burada iş bağlı diğer Azure IOT kenar öğreticileri oluşturun. 

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici, bir bilgisayarı veya Windows çalıştıran sanal makine bir nesnelerin interneti aygıt benzetimini yapmak için kullanmakta olduğunuz olduğunu varsayar. 

>[!TIP]
>Bir sanal makinede Windows çalıştırıyorsanız, etkinleştirme [iç içe geçmiş sanallaştırma] [ lnk-nested] ve en az 2 GB bellek ayıramadı. 

1. Desteklenen bir Windows sürümünü kullandığınızdan emin olun:
   * Windows 10 
   * Windows Server
2. Yükleme [Windows için Docker] [ lnk-docker] ve emin olun çalıştığından.
3. Yükleme [Python 2.7 Windows] [ lnk-python] ve PIP komutunu kullanabilirsiniz emin olun.
4. IOT kenar denetim komut dosyasını karşıdan yüklemek için aşağıdaki komutu çalıştırın.

   ```cmd
   pip install -U azure-iot-edge-runtime-ctl
   ```

> [!NOTE]
> Azure IOT kenar Windows kapsayıcıları veya Linux kapsayıcıları çalıştırabilirsiniz. Aşağıdaki Windows sürümlerinden birini kullanıyorsanız, Windows kapsayıcıları kullanabilirsiniz:
>    * Windows 10 sonbaharda oluşturucuları güncelleştir
>    * Windows Server 1709 (16299 derleme)
>    * X64 tabanlı bir cihazda Windows IOT Core (yapı 16299)
>
> Windows IOT Core için yönergeleri izleyin [Windows IOT Core üzerinde IOT kenar çalışma zamanı yükleme][lnk-install-iotcore]. Aksi takdirde, sadece [Windows kapsayıcılar kullanmak için Docker yapılandırma][lnk-docker-containers]. Önkoşulların doğrulamak için aşağıdaki komutu kullanın:
>    ```powershell
>    Invoke-Expression (Invoke-WebRequest -useb https://aka.ms/iotedgewin)
>    ```


## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Öğretici, IOT Hub'ınızı oluşturarak başlayın.
![IOT Hub oluşturma][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>Bir IOT sınır cihazı kaydetme

Bir IOT sınır cihazı, yeni oluşturulan IOT Hub ile kaydedin.
![Bir cihaz kaydetme][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="configure-the-iot-edge-runtime"></a>IOT kenar çalışma zamanı yapılandırma

Yükleyin ve Azure IOT kenar çalışma zamanı aygıtınızda başlatın. 
![Bir cihaz kaydetme][5]

IOT kenar çalışma zamanı, tüm IOT kenar aygıtlarda dağıtılır. İki modülden oluşur. **IOT kenar Aracısı** dağıtım ve IOT sınır cihazı modülleri izlenmesini kolaylaştırır. **IOT kenar hub** IOT sınır cihazı modülleri arasında ve cihaz IOT hub'ı arasındaki iletişim yönetir. Çalışma zamanı yeni aygıtınızda yapılandırdığınızda, yalnızca IOT kenar aracı ilk başta başlayacaktır. Bir modül dağıttığınızda kenar IOT hub'ı daha sonra gelir. 


Çalışma zamanı IOT kenar cihaz bağlantı dizenizi önceki bölümdeki yapılandırın.

```cmd
iotedgectl setup --connection-string "{device connection string}" --nopass
```

Çalışma zamanı başlatın.

```cmd
iotedgectl start
```

Docker IOT kenar Aracısı'nı bir modül olarak çalışıp çalışmadığını denetleyin.

```cmd
docker ps
```

![Docker edgeAgent bakın](./media/tutorial-simulate-device-windows/docker-ps.png)

## <a name="deploy-a-module"></a>Bir modül dağıtma

Azure IOT kenar Cihazınızı IOT Hub'ına telemetri verileri gönderecek bir modül dağıtmak için buluttan yönetin.
![Bir cihaz kaydetme][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]


## <a name="view-generated-data"></a>Oluşturulan görünüm verileri

Bu öğreticide, yeni bir IOT sınır cihazı oluşturulur ve IOT kenar çalışma zamanı yüklü. Ardından, cihaz için değişiklik yapmak zorunda kalmadan cihazda çalıştırmak için bir IOT kenar modülü göndermek için Azure portal kullanılır. Bu durumda, gönderilen modülü öğreticileri için kullanabileceğiniz çevresel veri oluşturur. 

Sanal cihazınız yeniden çalıştıran bilgisayarda komut istemi açın. Buluttan dağıtılan modülü IOT kenar aygıtınızda çalışır durumda olduğunu doğrulayın. 

```cmd
docker ps
```

![Cihazınızda üç modüller görünümü](./media/tutorial-simulate-device-windows/docker-ps2.png)

TempSensor modülünden buluta gönderilen iletiler görüntüleyin. 

```cmd
docker logs -f tempSensor
```

![Modülünüzün verileri görüntüleme](./media/tutorial-simulate-device-windows/docker-logs.png)

Cihaz kullanarak göndermeyi telemetriyi de görüntüleyebilirsiniz [IOT hub'ı explorer aracı][lnk-iothub-explorer]. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, yeni bir IOT sınır cihazı oluşturulur ve Azure IOT kenar bulut arabirimi kodu cihaza dağıtmak için kullanılır. Artık, kendi ortamı hakkında ham verileri oluşturma bir sanal cihaz var. 

Bu öğretici, tüm diğer IOT kenar öğreticileri için önkoşuldur. Azure IOT kenar işletme öngörüleri kenarına bu verileri dönüştürmek nasıl yardımcı olabileceğini öğrenmek için diğer öğreticileri birini açın devam edebilirsiniz.

> [!div class="nextstepaction"]
> [C# kod bir modül olarak geliştirmek ve dağıtmak](tutorial-csharp-module.md)

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