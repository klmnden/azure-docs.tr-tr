---
title: Azure IOT hub'ı Hızlı Başlangıç (Android) bir CİHAZDAN kontrol | Microsoft Docs
description: Bu hızlı başlangıçta iki örnek Java uygulaması çalıştıracaksınız. Hub'ınıza bağlı cihazları uzaktan denetleyebilir bir hizmet uygulaması bir uygulamadır. Diğer uygulama Uzaktan denetlenen hub'ınıza bağlı bir fiziksel veya sanal cihazda çalışır.
author: wesmc7777
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/15/2019
ms.author: wesmc
ms.openlocfilehash: e3b0c0703cb46087db38121055117b50f97ad03f
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59006582"
---
# <a name="quickstart-control-a-device-connected-to-an-iot-hub-android"></a>Hızlı Başlangıç: Bir IOT hub'ına (Android) bağlı cihazı denetleme

[!INCLUDE [iot-hub-quickstarts-2-selector](../../includes/iot-hub-quickstarts-2-selector.md)]

IoT Hub, IoT cihazlarınızdan buluta yüksek hacimlerde telemetri almanızı ve buluttan cihazlarınızı yönetmenizi sağlayan bir Azure hizmetidir. Bu hızlı başlangıçta, IoT hub’ınıza bağlı bir simülasyon cihazını denetlemek için *doğrudan yöntem* kullanırsınız. IoT hub’ınıza bağlı bir cihazın davranışını uzaktan değiştirmek için doğrudan yöntemler kullanabilirsiniz.

Hızlı başlangıçta, önceden yazılmış iki Java uygulaması kullanılır:

* Doğrudan yöntemler yanıt veren bir sanal cihaz uygulaması, bir arka uç hizmet uygulamasından çağrılır. Doğrudan yöntem çağrıları almak için bu uygulama, IoT hub’ınızda aygıta özgü bir uç noktaya bağlanır.

* Android cihazda doğrudan yöntem çağrıları bir hizmet uygulaması. Bir cihazda doğrudan yöntem çağırmak için bu uygulama, IoT hub’ınızda sunucu tarafı uç noktasına bağlanır.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* Android Studio'da https://developer.android.com/studio/. Android Studio yükleme hakkında daha fazla bilgi için bkz. [android yükleme](https://developer.android.com/studio/install).

* Android SDK 27 bu makaledeki örnek tarafından kullanılır.

* Microsoft Azure IOT uzantısı için Azure CLI Cloud Shell Örneğinize eklemek için aşağıdaki komutu çalıştırın. IOT uzantısı, Azure CLI için IOT Hub, IOT Edge ve IOT cihaz sağlama hizmeti (DPS) belirli komutları ekler.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

* İki örnek uygulama, bu hızlı başlangıç ile gereklidir: [Cihaz SDK'sı örnek Android uygulama](https://github.com/Azure-Samples/azure-iot-samples-java/tree/master/iot-hub/Samples/device/AndroidSample) ve [hizmeti SDK'sı örnek Android uygulama](https://github.com/Azure-Samples/azure-iot-samples-java/tree/master/iot-hub/Samples/service/AndroidSample). Bu örneklerin her ikisi de GitHub azure-IOT-samples-java havuzda bir parçasıdır. İndirin veya kopyalayın [azure-IOT-samples-java](https://github.com/Azure-Samples/azure-iot-samples-java) depo.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Önceki tamamladıysanız [hızlı başlangıç: Bir IOT hub'ına bir CİHAZDAN telemetri gönderme](quickstart-send-telemetry-android.md), bu adımı atlayın ve önceden oluşturduğunuz IOT hub'ı kullanın.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Önceki tamamladıysanız [hızlı başlangıç: Bir IOT hub'ına bir CİHAZDAN telemetri gönderme](quickstart-send-telemetry-android.md), bu adımı atlayabilir ve önceki hızlı başlangıçta kayıtlı aynı cihaz kullanın.

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Cihaz kimliği oluşturmak için Azure Cloud Shell'de aşağıdaki komutu çalıştırın.

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adı ile değiştirin.

   **MyAndroidDevice**: Bu değer için kayıtlı cihaza verilen addır. MyAndroidDevice gösterildiği gibi kullanın. Cihazınız için farklı bir ad seçerseniz, bu makalenin tamamında bu adı kullanın ve bunları çalıştırmadan önce örnek uygulamalar, cihaz adını güncelleştirmek gerekebilir.

    ```azurecli-interactive
    az iot hub device-identity create \
      --hub-name YourIoTHubName --device-id MyAndroidDevice
    ```

2. Yeni kaydettiğiniz cihazın _cihaz bağlantı dizesini_ almak için aşağıdaki komutları Azure Cloud Shell'de çalıştırın:

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string \
      --hub-name YourIoTHubName \
      --device-id MyAndroidDevice \
      --output table
    ```

    Şu ifadeye benzer şekilde görünen cihaz bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyAndroidDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız.

## <a name="retrieve-the-service-connection-string"></a>Hizmet bağlantı dizesini alma

Ayrıca gereksinim duyduğunuz bir _hizmet bağlantı dizesini_ yöntemleri çalıştırma ve iletileri almak için IOT hub'ınıza bağlanmak arka uç hizmet uygulamalarını etkinleştirmek için. Aşağıdaki komut, IoT hub'ınız için hizmeti bağlantı dizesini alır:

**YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adı ile değiştirin.

```azurecli-interactive
az iot hub show-connection-string --name YourIoTHubName --output table
```

Şu ifadeye benzer şekilde görünen hizmet bağlantı dizesini not edin:

`HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={YourSharedAccessKey}`

Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız. Hizmet bağlantı dizesi, cihaz bağlantı dizesinden farklıdır.

## <a name="listen-for-direct-method-calls"></a>Doğrudan yöntem çağrılarını dinleme

Cihaz SDK'sı örnek uygulama, fiziksel bir Android cihazı veya Android öykünücüsü üzerinde çalıştırılabilir. Örnek, IOT hub'ınızdaki bir cihaza özel uç noktasına bağlanır, sanal telemetri gönderir ve hub'ınıza doğrudan yöntem çağrılarından dinler. Bu hızlı başlangıçta, hub’dan gelen doğrudan yöntem çağrısı, telemetri gönderme aralığını değiştirmesini cihaza bildirir. Simülasyon cihazı, doğrudan yöntemi yürüttükten sonra hub’ınıza geri bir onay gönderir.

1. Android Studio'da GitHub örnek Android projesini açın. Proje kopyalanmış veya indirilen kopyanızı şu dizinde bulunur [azure IOT örnek java](https://github.com/Azure-Samples/azure-iot-samples-java) depo.

        \azure-iot-samples-java\iot-hub\Samples\device\AndroidSample

2. Android Studio'da açın *gradle.properties* değiştirin ve örnek proje için **Device_Connection_String** yer tutucusunu önceki ettiğiniz cihaz bağlantısı dizeniz ile.

    ```
    DeviceConnectionString=HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyAndroidDevice;SharedAccessKey={YourSharedAccessKey}
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

Bu uygulama bırakılması gereken çalışma zamanı sırasında telemetri aralığını güncelleştirmek için hizmet SDK'sı örneği çalıştırılırken phycial cihaz veya öykünücü üzerinde çalışıyor.


## <a name="read-the-telemetry-from-your-hub"></a>Hub’ınızdan telemetri okuma

Bu bölümde, Azure Cloud Shell ile kullanacağınız [IOT uzantısı](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) Android cihaz tarafından gönderilen cihaz iletileri izlemeye yönelik.

1. Azure Cloud Shell'i kullanarak, IoT hub’ınızdan gelen iletilere bağlanmak ve bu iletileri okumak için aşağıdaki komutu çalıştırın:

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

    ```azurecli-interactive
    az iot hub monitor-events --hub-name YourIoTHubName --output table
    ```
    Aşağıdaki ekran görüntüsünde, IOT hub'ı Android cihaz tarafından gönderilen telemetriyi aldığında olarak çıktı gösterir:

      ![Azure CLI kullanarak cihaz iletilerini okuma](media/quickstart-send-telemetry-android/read-data.png)

Varsayılan olarak telemetri uygulama telemetri Android cihazından 5 saniyede gönderiyor. Sonraki bölümde, bir doğrudan yöntem çağrısının Android IOT cihaz telemetrisi aralığını güncelleştirmek için kullanın.


## <a name="call-the-direct-method"></a>Doğrudan yöntem çağırma

Hizmet uygulamasının, IOT hub'ınızdaki bir hizmet tarafı uç noktasına bağlanır. Uygulama, IoT hub’ınız üzerinden bir cihaza doğrudan yöntem çağrıları yapar ve onayları dinler.

Bu uygulama bir ayrı fiziksel Android cihaz veya Android öykünücüsünde çalıştırın.

Bir IOT Hub arka uç hizmeti uygulaması genellikle bir IOT Hub'ındaki tüm cihazlar denetleyen hassas bağlantı dizesiyle ilgili riskleri azaltmak daha kolay olduğu bulutta çalışır. Bu örnekte biz bunu yalnızca tanıtım amacıyla bir Android uygulaması olarak çalışır. Bu hızlı başlangıçta diğer dil sürümlerini daha yakından bir arka uç hizmet uygulaması ile hizalanan diğer örnekler sunar.

1. GitHub hizmeti örnek Android projesi Android Studio'da açın. Proje kopyalanmış veya indirilen kopyanızı şu dizinde bulunur [azure IOT örnek java](https://github.com/Azure-Samples/azure-iot-samples-java) depo.

        \azure-iot-samples-java\iot-hub\Samples\service\AndroidSample

2. Android Studio'da açın *gradle.properties* örneğinin değerini güncelleştirin ve proje **ConnectionString** ve **DeviceID** , hizmet bağlantı özellikleriyle daha önce not ettiğiniz dize ve kaydettiğiniz Android cihaz kimliği.

    ```
    ConnectionString=HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={YourSharedAccessKey}
    DeviceId=MyAndroidDevice
    ```

3. Android Studio'da **dosya** > **projeyi Gradle dosyalarıyla Eşitle**. Derleme tamamlandığında doğrulayın.

   > [!NOTE]
   > Proje eşitleme başarısız olursa aşağıdakilerden biri olabilir:
   >
   > * Eski Android Studio sürümünüz için Android Gradle eklentisi ve projede başvurulmuş Gradle sürümleridir. İzleyin [bu yönergeleri](https://developer.android.com/studio/releases/gradle-plugin) başvuru ve doğru sürümlerini eklentisi ve Gradle yüklemenizin yükleyin.
   > * Android SDK için lisans anlaşması imzalı değil. Derleme çıkışını Lisans Sözleşmesi'ni imzalamak ve SDK'yı indirmek için yönergeleri izleyin.


4. Derleme tamamlandıktan sonra tıklayın **çalıştırma** > **'uygulamayı' Çalıştır**. Ayrı bir fiziksel Android cihaz veya Android öykünücüsünde çalıştırmak üzere uygulamayı yapılandırır. Bir Android uygulaması bir fiziksel cihaz veya öykünücü üzerinde çalışan daha fazla bilgi için bkz: [uygulamanızı çalıştırma](https://developer.android.com/training/basics/firstapp/running-app).

5. Uygulama yüklendikten sonra güncelleştirme **ayarlanan Mesajlaşma aralığı** değerini **1000** tıklatıp **Invoke**.

    TH telemetri Mesajlaşma aralığını milisaniye cinsindendir. Cihaz örnek telemetri aralığını varsayılan 5 saniye olarak ayarlanır. Bu değişiklik, her saniyede telemetri gönderilmesi Android IOT cihaz güncelleştirir.

    ![Telemetri aralığını girin](media/quickstart-control-device-android/enter-telemetry-interval.png)

6. Uygulamayı yöntemi başarıyla yürütülüp olup olmadığını belirten bir bildirim alırsınız.

    ![Doğrudan yöntem onayı](media/quickstart-control-device-android/direct-method-ack.png)



## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir doğrudan yöntem bir cihazda bir arka uç uygulamasından çağrılır ve bir sanal cihaz uygulaması doğrudan yöntem çağrısında yanıt verdi.

Cihazdan buluta iletileri, buluttaki farklı hedeflere yönlendirmeyi öğrenmek için sonraki öğreticiyle devam edin.

> [!div class="nextstepaction"]
> [Öğretici: Rota telemetri işleme için farklı uç](tutorial-routing.md)
