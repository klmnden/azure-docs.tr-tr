---
title: Azure IoT Hub’a telemetri gönderme hızlı başlangıcı (Python) | Microsoft Docs
description: Bu hızlı başlangıçta, bir IoT hub’a sanal telemetri göndermek için örnek bir Python uygulaması çalıştırır ve IoT hub’dan telemetri okumak için bir yardımcı program kullanırsınız.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: quickstart
ms.custom: mvc
ms.date: 02/28/2019
ms.openlocfilehash: a8abd71609d3e063c92541485007a3bde44be954
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67051246"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-and-read-it-with-a-back-end-application-python"></a>Hızlı Başlangıç: Bir IOT hub'ına bir CİHAZDAN telemetri gönderme ve arka uç uygulaması ile (Python) okuyun

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

IoT Hub, IoT cihazlarınızdan buluta depolama veya işleme amacıyla yüksek hacimlerde telemetri almanızı sağlayan bir Azure hizmetidir. Bu hızlı başlangıçta, bir simülasyon cihazı uygulamasından bir arka uç uygulamasına işlenmek üzere IoT Hub aracılığıyla telemetri gönderirsiniz.

Bu hızlı başlangıçta, telemetri göndermek için önceden yazılmış bir Python uygulaması ve hub’dan telemetri okumak için bir CLI yardımcı programı kullanılır. Bu iki uygulamayı çalıştırmadan önce bir IoT hub oluşturur ve hub’a bir cihaz kaydedersiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta çalıştırma örnek uygulaması, Python kullanarak yazılır. Şu anda Python için Microsoft Azure IOT SDK, her platform için Python yalnızca belirli sürümlerini destekler. Daha fazla bilgi için bkz. [Python SDK'sı Benioku](https://github.com/Azure/azure-iot-sdk-python#important-installation-notes---dealing-with-importerror-issues).

Bu hızlı başlangıçta, Windows geliştirme makinesi kullandığınızı varsayar. Yalnızca Windows sistemleri için [Python 3.6.x](https://www.python.org/downloads/release/python-368/) desteklenir. Çalıştığınız sistemin mimarisine uygun Python yükleyicisini seçmeniz gerekir. Sisteminiz CPU mimarisi, 32 bit ve ardından indirme x86 yükleyici ise; 64 bit mimari için x86 64 yükleyiciyi indirin. Ayrıca, emin olun [Microsoft Visual C++ yeniden dağıtılabilir için Visual Studio 2019](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) Mimarinizi (x86 veya x64) için yüklenir.

Python için diğer platformlardan indirebileceğiniz [Python.org](https://www.python.org/downloads/).

Aşağıdaki komutlardan birini kullanarak geliştirme makinenizde geçerli Python sürümünü doğrulayabilirsiniz:

```python
python --version
```

```python
python3 --version
```

Microsoft Azure IOT uzantısı için Azure CLI Cloud Shell Örneğinize eklemek için aşağıdaki komutu çalıştırın. IOT uzantısı, Azure CLI için IOT Hub, IOT Edge ve IOT cihaz sağlama hizmeti (DPS) belirli komutları ekler.

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

[https://github.com/Azure-Samples/azure-iot-samples-python/archive/master.zip](https://github.com/Azure-Samples/azure-iot-samples-python/archive/master.zip ) adresinden örnek Python projesini indirin ve ZIP arşivini ayıklayın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Cihaz kimliği oluşturmak için Azure Cloud Shell'de aşağıdaki komutu çalıştırın.

    **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

    **MyPythonDevice**: Bu, kayıtlı bir cihaz için verilen addır. Gösterilen MyPythonDevice değerini kullanın. Cihazınız için farklı bir ad seçerseniz bu makalenin geri kalan bölümünde aynı adı kullanmanız ve örnek uygulamaları çalıştırmadan önce bunlarda da cihaz adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyPythonDevice
    ```

1. Azure Cloud Shell içinde almak için aşağıdaki komutları çalıştırın _cihaz bağlantı dizesini_ kaydettiğiniz cihazın:

    **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyPythonDevice --output table
    ```

    Şu ifadeye benzer şekilde görünen cihaz bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız.

## <a name="send-simulated-telemetry"></a>Sanal telemetri gönderme

Simülasyon cihazı uygulaması, IoT hub’ınız üzerindeki cihaza özgü bir uç noktaya bağlanır ve sanal sıcaklık ve nem telemetrisi gönderir.

1. Yerel terminal penceresinde, örnek Python projesinin kök klasörüne gidin. Daha sonra **iot-hub\Quickstarts\simulated-device** klasörüne gidin.

1. **SimulatedDevice.py** dosyasını, istediğiniz bir metin düzenleyicide açın.

    `CONNECTION_STRING` değişkeninin değerini, önceden not ettiğiniz cihaz bağlantı dizesiyle değiştirin. Daha sonra **SimulatedDevice.py** dosyasına değişikliklerinizi kaydedin.

1. Yerel terminal penceresinde, aşağıdaki komutları çalıştırarak simülasyon cihazı uygulaması için gerekli kitaplıkları yükleyin:

    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Yerel terminal penceresinde, aşağıdaki komutları çalıştırarak simülasyon cihazı uygulamasını çalıştırın:

    ```cmd/sh
    python SimulatedDevice.py
    ```

    Aşağıdaki ekran görüntüsünde, simülasyon cihazı uygulaması, IoT hub’ınıza telemetri gönderdiğinde oluşan çıktı gösterilmektedir:

    ![Simülasyon cihazını çalıştırma](media/quickstart-send-telemetry-python/SimulatedDevice.png)
    
### <a name="to-avoid-the-import-iothubclient-error"></a>İçeri aktarma iothub_client hatayı önlemek için
Python için Azure IOT SDK'ın geçerli sürümü üzerindeki bir sarmalayıcıdır [C SDK'mız](https://github.com/azure/azure-iot-sdk-c). Kullanılarak oluşturulan [Boost](https://www.boost.org/) kitaplığı. Bu nedenle bazı önemli kısıtlamaları ile birlikte gelir. Daha fazla ayrıntı görmek [burada](https://github.com/Azure/azure-iot-sdk-python#important-installation-notes---dealing-with-importerror-issues)

1. Doğru sürüme sahip olup olmadığını denetleyin [Python](https://github.com/Azure/azure-iot-sdk-python#important-installation-notes---dealing-with-importerror-issues). Yalnızca belirli sürümler, düzgün çalıştığını bu örneğe ilişkin dikkat edin. 
2. Doğru sürüme sahip olup olmadığını denetleyin C++ çalışma zamanı [Microsoft Visual C++ yeniden dağıtılabilir için Visual Studio 2019](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads). (En son öneririz).
3. Iothub istemci yüklü olduğunu doğrulayın: `pip install azure-iothub-device-client`.

## <a name="read-the-telemetry-from-your-hub"></a>Hub’ınızdan telemetri okuma

IoT Hub uzantısı IoT Hub’ınızdaki bir hizmet tarafı **Olaylar** uç noktasına bağlanabilir. Uzantı, simülasyon cihazınızdan gönderilen cihazdan buluta iletileri alır. IoT Hub arka uç uygulaması genellikle cihazdan buluta iletileri alıp işlemek için bulutta çalışır.

Aşağıdaki komutları Azure Cloud Shell'de çalıştırın, `YourIoTHubName` yerine IoT hub'ınızın adını yazın:

```azurecli-interactive
az iot hub monitor-events --hub-name YourIoTHubName --device-id MyPythonDevice 
```

Aşağıdaki ekran görüntüsünde uzantı, simülasyon cihazı tarafından hub’a gönderilen telemetriyi aldığında oluşan çıktı gösterilmektedir:

![Arka uç uygulamasını çalıştırma](media/quickstart-send-telemetry-python/ReadDeviceToCloud.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IoT hub’ını ayarladınız, bir cihazı kaydettiniz, Python uygulamasını kullanarak hub’a sanal telemetri gönderdiniz ve basit bir arka uç uygulamasını kullanarak hub’dan telemetriyi okudunuz.

Bir arka uç uygulamasından simülasyon cihazınızı denetlemeyi öğrenmek için sonraki hızlı başlangıçla devam edin.

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Bir IOT hub'ına bağlı cihazı denetleme](quickstart-control-device-python.md)
