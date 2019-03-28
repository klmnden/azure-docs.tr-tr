---
title: Telemetri gönderme (Android) için Azure IOT hub'ı hızlı başlangıç | Microsoft Docs
description: Bu hızlı başlangıçta bir IOT hub'a sanal telemetri göndermek ve bulutta işlemek için IOT hub'dan telemetri okumak için örnek Android uygulaması çalıştırın.
author: wesmc7777
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/15/2019
ms.author: wesmc
ms.openlocfilehash: 63e1f6a6779cf2689a7cbffad06447d272a413d7
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58516935"
---
# <a name="quickstart-send-iot-telemetry-from-an-android-device"></a>Hızlı Başlangıç: Bir Android CİHAZDAN IOT telemetri gönderme

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

IoT Hub, IoT cihazlarınızdan buluta depolama veya işleme amacıyla yüksek hacimlerde telemetri almanızı sağlayan bir Azure hizmetidir. Bu hızlı başlangıçta, telemetri bir IOT Hub'ına fiziksel veya sanal bir cihaz üzerinde çalışan bir Android uygulamasından gönderirsiniz.

Hızlı Başlangıç, telemetri göndermek için önceden yazılmış bir Android uygulaması kullanır. Telemetriyi Azure Cloud Shell'i kullanarak IOT Hub'ından okur. Uygulamayı çalıştırmadan önce IOT hub oluşturma ve hub'ı ile cihaz kaydetme.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* Android Studio'da https://developer.android.com/studio/. Android Studio yükleme hakkında daha fazla bilgi için bkz. [android yükleme](https://developer.android.com/studio/install). 

* Android SDK 27 bu makaledeki örnek tarafından kullanılır. 

* [Örnek Android uygulaması](https://github.com/Azure-Samples/azure-iot-samples-java/tree/master/iot-hub/Samples/device/AndroidSample) bu çalıştırma hızlı başlangıcı GitHub azure-IOT-samples-java havuzda bir parçasıdır. İndirin veya kopyalayın [azure-IOT-samples-java](https://github.com/Azure-Samples/azure-iot-samples-java) depo.



## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Aşağıdaki komutları Azure Cloud Shell'de çalıştırarak IoT Hub CLI uzantısını ekleyin ve cihaz kimliğini oluşturun. 

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

   **MyAndroidDevice**: MyAndroidDevice için kayıtlı cihaza verilen addır. MyAndroidDevice gösterildiği gibi kullanın. Cihazınız için farklı bir ad seçerseniz bu makalenin geri kalan bölümünde aynı adı kullanmanız ve örnek uygulamaları çalıştırmadan önce bunlarda da cihaz adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyAndroidDevice
    ```

2. Yeni kaydettiğiniz cihazın _cihaz bağlantı dizesini_ almak için aşağıdaki komutları Azure Cloud Shell'de çalıştırın:

    **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyAndroidDevice --output table
    ```

    Şu ifadeye benzer şekilde görünen cihaz bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyAndroidDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu hızlı başlangıcın ilerleyen telemetri göndermek için bu değeri kullanın.

## <a name="send-telemetry"></a>Telemetri gönderme

1. Android Studio'da GitHub örnek Android projesini açın. Proje kopyalanmış veya indirilen kopyanızı şu dizinde bulunur [azure IOT örnek java](https://github.com/Azure-Samples/azure-iot-samples-java) depo.

        \azure-iot-samples-java\iot-hub\Samples\device\AndroidSample

2. Android Studio'da açın *gradle.properties* değiştirin ve örnek proje için **Device_Connection_String** yer tutucusunu önceki ettiğiniz cihaz bağlantısı dizeniz ile.

    ```
    DeviceConnectionString=HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}
    ```

3. Android Studio'da **dosya** > **projeyi Gradle dosyalarıyla Eşitle**. Derleme tamamlandığında doğrulayın.

   > [!NOTE]
   > Proje eşitleme başarısız olursa aşağıdakilerden biri olabilir:
   >
   > * Eski Android Studio sürümünüz için Android Gradle eklentisi ve projede başvurulmuş Gradle sürümleridir. İzleyin [bu yönergeleri](https://developer.android.com/studio/releases/gradle-plugin) başvuru ve doğru sürümlerini eklentisi ve Gradle yüklemenizin yükleyin.
   > * Android SDK için lisans anlaşması imzalı değil. Derleme çıkışını Lisans Sözleşmesi'ni imzalamak ve SDK'yı indirmek için yönergeleri izleyin.

4. Derleme tamamlandıktan sonra tıklayın **çalıştırma** > **'uygulamayı' Çalıştır**. Fiziksel bir Android cihazı veya Android öykünücüsünde çalıştırmak üzere uygulamayı yapılandırır. Bir Android uygulaması bir fiziksel cihaz veya öykünücü üzerinde çalışan daha fazla bilgi için bkz: [uygulamanızı çalıştırma](https://developer.android.com/training/basics/firstapp/running-app).

5. Uygulama yüklendikten sonra tıklayın **Başlat** IOT Hub'ınıza telemetri göndermeye başlaması düğmesi:

    ![Uygulama](media/quickstart-send-telemetry-android/sample-screenshot.png)


## <a name="read-the-telemetry-from-your-hub"></a>Hub’ınızdan telemetri okuma

Bu bölümde, Azure Cloud Shell ile kullanacağınız [IOT uzantısı](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) Android cihaz tarafından gönderilen cihaz iletileri izlemeye yönelik.

1. Azure Cloud Shell'i kullanarak, IoT hub’ınızdan gelen iletilere bağlanmak ve bu iletileri okumak için aşağıdaki komutu çalıştırın:

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

    ```azurecli-interactive
    az iot hub monitor-events --hub-name YourIoTHubName --output table
    ```
    Aşağıdaki ekran görüntüsünde, IOT hub'ı Android cihaz tarafından gönderilen telemetriyi aldığında olarak çıktı gösterir:

      ![Azure CLI kullanarak cihaz iletilerini okuma](media/quickstart-send-telemetry-android/read-data.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir IOT Kurulum olduğunuz hub'ı kullanarak bir Android uygulaması hub'a sanal telemetri gönderilen bir cihaz kaydettiniz ve Azure Cloud Shell'i kullanarak hub'dan telemetri okumak.

Bir arka uç uygulamasından simülasyon cihazınızı denetlemeyi öğrenmek için sonraki hızlı başlangıçla devam edin.

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Bir IOT hub'ına bağlı cihazı denetleme](quickstart-control-device-android.md)

