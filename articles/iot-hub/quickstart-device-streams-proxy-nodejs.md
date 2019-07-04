---
title: Azure IOT Hub cihazı akışları Node.js Hızlı Başlangıç için SSH ve RDP (Önizleme) | Microsoft Docs
description: Bu hızlı başlangıçta, cihaz akışları IOT Hub üzerinde SSH ve RDP senaryoları etkinleştirmek için bir proxy görevi gören örnek Node.js uygulamasını çalıştırın.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: robinsh
ms.openlocfilehash: 83339273d9161c3947df191d10e788980db39b28
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446002"
---
# <a name="quickstart-enable-ssh-and-rdp-over-an-iot-hub-device-stream-by-using-a-nodejs-proxy-application-preview"></a>Hızlı Başlangıç: SSH ve RDP Node.js Ara sunucu uygulamasını (Önizleme) kullanarak bir IOT Hub cihaz akış üzerinden etkinleştirme

[!INCLUDE [iot-hub-quickstarts-4-selector](../../includes/iot-hub-quickstarts-4-selector.md)]

Microsoft Azure IOT Hub cihaz akışları olarak şu anda destekleyen bir [önizleme özelliği](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[IOT Hub cihaz akışları](./iot-hub-device-streams-overview.md) güvenli ve güvenlik duvarı uyumlu bir şekilde iletişim kurmak hizmet ve cihaz uygulamalarınıza izin verin. 

Bu hızlı başlangıçta, Secure Shell (SSH) ve Uzak Masaüstü Protokolü (RDP) trafiğine cihaza bir cihaz akış üzerinden gönderilmek üzere etkinleştirmek için hizmet tarafında çalıştırılan bir Node.js proxy uygulamanın yürütülmesini açıklanır. Kurulum genel bakış için bkz. [yerel Proxy örnek](./iot-hub-device-streams-overview.md#local-proxy-sample-for-ssh-or-rdp). 

Genel Önizleme süresince Node.js SDK'sı yalnızca hizmet tarafında cihaz akışlarını destekler. Sonuç olarak, bu hızlı başlangıçta, yalnızca hizmeti-yerel ara sunucu uygulamasını çalıştırmak için yönergeler kapsar. Cihaz yerel ara sunucu uygulamasını çalıştırmak için bkz:  

   * [SSH ve RDP IOT Hub cihaz akışları C Ara sunucu uygulamasını kullanarak etkinleştirin](./quickstart-device-streams-proxy-c.md)
   * [SSH ve RDP kullanarak IOT Hub cihaz akışları etkinleştirme bir C# proxy uygulama](./quickstart-device-streams-proxy-csharp.md)

Bu makalede, (bir bağlantı noktası 22'ı kullanarak), SSH için Kurulum açıklar ve sonra nasıl Kurulum RDP için (Bu bağlantı noktası 3389'ı kullanır) değiştirileceğini açıklar. Cihaz akışları uygulama ve protokolü-depolamadan bağımsız, çünkü iletişimi bağlantı noktasını değiştirerek istemci-sunucu uygulama trafiği, diğer türleri genellikle uyum sağlayacak şekilde aynı örnek değiştirebilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* Cihaz akışları önizlemesi, şu anda şu bölgelerde oluşturulan yalnızca IOT hub'ları için desteklenir:

  * Orta ABD
  * Orta ABD EUAP

* Bu hızlı başlangıçta hizmet yerel uygulamayı çalıştırmak için geliştirme makinenizi Node.js v10.x.x veya sonraki bir sürümü gerekir.
  * İndirme [Node.js](https://nodejs.org) birden çok platform için.
  * Geçerli sürümü Node.js geliştirme makinenizde aşağıdaki komutu kullanarak doğrulayın:

   ```
   node --version
   ```

* Azure IOT uzantısı, Azure CLI için aşağıdaki komutu çalıştırarak Cloud Shell Örneğinize ekleyin. IOT Hub, IOT Edge ve IOT cihaz sağlama hizmeti (DPS) IOT uzantısını ekler-Azure CLI için özel komutları.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    ```

* Bunu zaten bunu yapmadıysanız [örnek Node.js projesini indirin](https://github.com/Azure-Samples/azure-iot-samples-node/archive/streams-preview.zip) ve ZIP arşivini ayıklayın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Önceki tamamladıysanız [hızlı başlangıç: Bir IOT hub'ına bir CİHAZDAN telemetri gönderme](quickstart-send-telemetry-node.md), bu adımı atlayabilirsiniz.

[!INCLUDE [iot-hub-include-create-hub-device-streams](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Tamamlanmışsa [hızlı başlangıç: Bir IOT hub'ına bir CİHAZDAN telemetri gönderme](quickstart-send-telemetry-node.md), bu adımı atlayabilirsiniz.

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu bölümde, bir simülasyon cihazı kaydetmek için Azure Cloud Shell kullanın.

1. Cihaz kimliği oluşturma için Cloud Shell'de aşağıdaki komutu çalıştırın:

   > [!NOTE]
   > * Değiştirin *YourIoTHubName* yer tutucu IOT hub'ınız için seçtiğiniz ada sahip.
   > * Kullanım *Cihazım*gösterildiği gibi. Kayıtlı cihaz için verilen addır. Cihazınız için farklı bir ad seçerseniz, bu makalenin tamamında bu adı kullanın ve bunları çalıştırmadan önce örnek uygulamalar, cihaz adını güncelleştirin.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

1. IOT hub'ınıza bağlanmak ve iletileri almak arka uç uygulaması etkinleştirmek için bir *hizmet bağlantı dizesini*. Aşağıdaki komut, IOT hub'ınız için bir dize alır:

   > [!NOTE]
   > Değiştirin *YourIoTHubName* yer tutucu IOT hub'ınız için seçtiğiniz ada sahip.

    ```azurecli-interactive
    az iot hub show-connection-string --policy-name service --name YourIoTHubName
    ```

    Bu hızlı başlangıçta daha sonra kullanmak için döndürülen değer unutmayın. Aşağıdaki örneğe benzer şekilde görünür:

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`

## <a name="ssh-to-a-device-via-device-streams"></a>Bir cihazın cihaz akışları yoluyla SSH

Bu bölümde, SSH trafiği tünel oluşturmak için bir uçtan uca stream oluşturun.

### <a name="run-the-device-local-proxy-application"></a>Cihaz yerel ara sunucu uygulamasını çalıştırın

Daha önce belirtildiği gibi IOT Hub Node.js SDK'sı yalnızca hizmet tarafında cihaz akışlarını destekler. Cihaz yerel uygulama için şu hızlı başlangıçlarda birinde kullanılabilir olan bir cihaz Ara sunucu uygulamasını kullanın:

   * [SSH ve RDP IOT Hub cihaz akışları C Ara sunucu uygulamasını kullanarak etkinleştirin](./quickstart-device-streams-proxy-c.md)
   * [SSH ve RDP kullanarak IOT Hub cihaz akışları etkinleştirme bir C# proxy uygulama](./quickstart-device-streams-proxy-csharp.md) 

Sonraki adıma devam etmeden önce cihaz yerel ara sunucu uygulamasını çalıştığından emin olun.

### <a name="run-the-service-local-proxy-application"></a>Hizmet yerel ara sunucu uygulamasını çalıştırın

Çalıştıran yerel cihaz proxy uygulama ile birlikte, aşağıdakileri yaparak node.js'de yazılmış hizmeti-yerel ara sunucu uygulamasını çalıştırın:

1. Ortam değişkenleri için cihaz üzerinde çalışan proxy için hizmet kimlik bilgilerinizi, SSH arka plan programı çalıştığı hedef cihaz kimliği ve bağlantı noktası numarasını sağlayın.

   ```
   # In Linux
   export IOTHUB_CONNECTION_STRING="<provide_your_service_connection_string>"
   export STREAMING_TARGET_DEVICE="MyDevice"
   export PROXY_PORT=2222

   # In Windows
   SET IOTHUB_CONNECTION_STRING=<provide_your_service_connection_string>
   SET STREAMING_TARGET_DEVICE=MyDevice
   SET PROXY_PORT=2222
   ```

   Cihaz kimliği ve bağlantı dizesini eşleştirmek için yukarıdaki değerleri değiştirin.

1. Git *hızlı Başlangıçlar/cihaz akışları hizmet* dizin sıkıştırması proje klasörü ve hizmet yerel ara sunucu uygulamasını çalıştırın.

   ```
   cd azure-iot-samples-node-streams-preview/iot-hub/Quickstarts/device-streams-service

   # Install the preview service SDK, and other dependencies
   npm install azure-iothub@streams-preview
   npm install

   # Run the service-local proxy application
   node proxy.js
   ```

### <a name="ssh-to-your-device-via-device-streams"></a>Cihazınıza cihaz akışları aracılığıyla SSH

Linux'ta, SSH kullanarak çalıştırın `ssh $USER@localhost -p 2222` bir terminal üzerinde. Windows sık kullanılan SSH istemciniz (örneğin, PuTTY) kullanın.

Konsol SSH oturum kurulduktan sonra hizmeti-yerel çıktısı (proxy hizmeti-yerel uygulama dinlediği 2222 numaralı bağlantı noktasında):

![SSH terminal çıkış](./media/quickstart-device-streams-proxy-nodejs/service-console-output.png)

Konsol çıktısı SSH istemci uygulamasının (SSH istemcisi iletişim kurar, hizmeti-yerel ara sunucu uygulamasını dinleme yaptığı bağlantı noktası 22'yi bağlanarak SSH arka plan programı için):

![SSH istemcisi çıkış](./media/quickstart-device-streams-proxy-nodejs/ssh-console-output.png)

### <a name="rdp-to-your-device-via-device-streams"></a>Cihazınıza cihaz akışları üzerinden RDP

Şimdi RDP istemci uygulamanızı kullanın ve hizmet proxy'si, 2222 numaralı bağlantı noktasında bağlanmak daha önce seçtiğiniz ise işleminizdeki rastgele bağlantı.

> [!NOTE]
> Cihaz Ara sunucunuz RDP için yapılandırıldığını ve RDP bağlantı noktası 3389'ile yapılandırılmış emin olun.

![RDP istemcisinin proxy hizmeti-yerel uygulamaya bağlanır](./media/quickstart-device-streams-proxy-nodejs/rdp-screen-capture.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IOT hub'ı ayarladınız, kayıtlı bir cihaz ve bir IOT cihazına RDP ve SSH sağlamak için bir hizmet proxy'si uygulaması dağıtılmış. RDP ve SSH trafiği, IOT hub'ı aracılığıyla cihaz akış aracılığıyla tünel. Bu işlem cihazı doğrudan bağlantı ihtiyacını ortadan kaldırır.

Cihaz akışları hakkında daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
> [Cihaz akışları genel bakış](./iot-hub-device-streams-overview.md)
