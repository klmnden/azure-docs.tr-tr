---
title: Azure IOT Hub cihazı akışları Node.js hızlı başlangıç SSH/RDP (Önizleme) | Microsoft Docs
description: Bu hızlı başlangıçta, IOT Hub cihaz akışlar üzerinde SSH/RDP senaryoları etkinleştirmek için bir proxy görevi gören örnek Node.js uygulamasını çalıştırın.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: rezas
ms.openlocfilehash: 8c323475bd5d1633feee45191a16d5af438ebc82
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65595234"
---
# <a name="quickstart-sshrdp-over-iot-hub-device-streams-using-nodejs-proxy-application-preview"></a>Hızlı Başlangıç: SSH/RDP üzerinden Node.js Ara sunucu uygulamasını (Önizleme) kullanarak IOT Hub cihaz akışları

[!INCLUDE [iot-hub-quickstarts-4-selector](../../includes/iot-hub-quickstarts-4-selector.md)]

Microsoft Azure IOT Hub cihaz akışları olarak şu anda destekleyen bir [önizleme özelliği](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[IOT Hub cihaz akışları](./iot-hub-device-streams-overview.md) güvenli ve güvenlik duvarı uyumlu bir şekilde iletişim kurmak hizmet ve cihaz uygulamalarınıza izin verin. Bu Hızlı Başlangıç Kılavuzu, bir cihaz akış üzerinden cihaza gönderilecek SSH ve RDP trafiğini etkinleştirmek için hizmet tarafında çalışan bir Node.js proxy uygulamanın yürütülmesini açıklar. Bkz: [burada](./iot-hub-device-streams-overview.md#local-proxy-sample-for-ssh-or-rdp) Kurulum genel bakış. Genel Önizleme süresince Node.js SDK'sı yalnızca hizmet tarafında cihaz akışlarını destekler. Sonuç olarak, bu Hızlı Başlangıç Kılavuzu, yalnızca hizmeti-yerel proxy çalıştırma yönergeleri kapsar. Kullanılabilir eşlik eden bir cihaz yerel proxy çalıştırmalısınız [C hızlı](./quickstart-device-streams-proxy-c.md) veya [ C# hızlı](./quickstart-device-streams-proxy-csharp.md) Kılavuzlar.

İlk kurulum için SSH (22 numaralı bağlantı noktasını kullanarak) açıklanmaktadır. Biz sonra nasıl Kurulum RDP için (Bu bağlantı noktası 3389'ı kullanır) değiştirileceğini açıklar. Cihaz akışlar, uygulama ve protokolü belirsiz olduğundan, aynı örnek istemci/sunucu uygulama trafiği diğer türleri (genellikle iletişim bağlantı noktasını değiştirerek) uyum sağlayacak şekilde değiştirilebilir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Cihaz akışları şu anda önizlemesidir yalnızca IOT hub'ları aşağıdaki bölgelerde oluşturulan için desteklenir:

  - **Orta ABD**
  - **Orta ABD EUAP**

Bu hızlı başlangıçta hizmet yerel uygulamayı çalıştırmak için geliştirme makinenizi Node.js v10.x.x veya sonraki bir sürümü gerekir.

[nodejs.org](https://nodejs.org) adresinden birden fazla platform için Node.js’yi indirebilirsiniz.

Aşağıdaki komutu kullanarak geliştirme makinenizde geçerli Node.js sürümünü doğrulayabilirsiniz:

```
node --version
```

Microsoft Azure IOT uzantısı için Azure CLI Cloud Shell Örneğinize eklemek için aşağıdaki komutu çalıştırın. IOT uzantısı, Azure CLI için IOT Hub, IOT Edge ve IOT cihaz sağlama hizmeti (DPS) belirli komutları ekler.

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Örnek Node.js projesini önceden indirmediyseniz https://github.com/Azure-Samples/azure-iot-samples-node/archive/streams-preview.zip adresinden indirip ZIP arşivini ayıklayın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Önceki tamamladıysanız [hızlı başlangıç: Bir IOT hub'ına bir CİHAZDAN telemetri gönderme](quickstart-send-telemetry-node.md), bu adımı atlayabilirsiniz.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Önceki tamamladıysanız [hızlı başlangıç: Bir IOT hub'ına bir CİHAZDAN telemetri gönderme](quickstart-send-telemetry-node.md), bu adımı atlayabilirsiniz.

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Cihaz kimliği oluşturmak için Azure Cloud Shell'de aşağıdaki komutu çalıştırın.

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adı ile değiştirin.

   **Cihazım**: Bu, kayıtlı bir cihaz için verilen addır. Cihazım gösterildiği gibi kullanın. Cihazınız için farklı bir ad seçerseniz bu makalenin geri kalan bölümünde aynı adı kullanmanız ve örnek uygulamaları çalıştırmadan önce bunlarda da cihaz adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

2. Arka uç uygulamasının IoT hub’ınıza bağlanmasına ve iletileri almasına olanak sağlamak için bir _hizmet bağlantı dizesi_ de gerekir. Aşağıdaki komut, IoT hub'ınız için hizmeti bağlantı dizesini alır:

    **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adı ile değiştirin.

    ```azurecli-interactive
    az iot hub show-connection-string --policy-name service --name YourIoTHubName
    ```

    Şuna benzer döndürülen değeri not edin:

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`

## <a name="ssh-to-a-device-via-device-streams"></a>Bir cihazın cihaz akışları yoluyla SSH

### <a name="run-the-device-local-proxy"></a>Cihaz yerel ara sunucu çalıştırın

Daha önce belirtildiği gibi IOT Hub Node.js SDK'sı hizmet tarafında yalnızca cihaz akışlarını destekler. Cihaz yerel uygulama için bulunan eşlik eden cihazın proxy programları kullanın [C hızlı](./quickstart-device-streams-proxy-c.md) veya [ C# hızlı](./quickstart-device-streams-proxy-csharp.md) Kılavuzlar. Cihaz yerel proxy, sonraki adıma devam etmeden önce çalıştığından emin olun.

### <a name="run-the-service-local-proxy"></a>Hizmet yerel ara sunucu çalıştırın

Varsayarak [cihaz yerel proxy](#run-the-device-local-proxy) olan çalışan, node.js'de yazılmış hizmeti-yerel proxy çalıştırmak için aşağıdaki adımları izleyin.

- Ortam değişkenleri olarak cihaz üzerinde çalışan proxy için hizmet kimlik bilgilerinizi, SSH arka plan programı çalıştığı hedef cihaz kimliği ve bağlantı noktası numarasını sağlayın.
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
  Cihaz kimliği ve bağlantı dizenizle eşleşen yukarıdaki değerleri değiştirin.

- Gidin `Quickstarts/device-streams-service` sıkıştırması proje klasörü ve Çalıştır hizmeti-yerel proxy.
  ```
  cd azure-iot-samples-node-streams-preview/iot-hub/Quickstarts/device-streams-service

  # Install the preview service SDK, and other dependencies
  npm install azure-iothub@streams-preview
  npm install

  # Run the service-local proxy application
  node proxy.js
  ```

### <a name="ssh-to-your-device-via-device-streams"></a>Cihazınıza cihaz akışları aracılığıyla SSH

SSH kullanarak Linux içinde çalıştırma `ssh $USER@localhost -p 2222` bir terminal üzerinde. Windows sık kullanılan SSH istemciniz kullanın (örneğin, PuTTY).

Konsol SSH oturum kurulduktan sonra hizmeti-yerel çıktısı (2222 numaralı bağlantı noktasında service-yerel proxy dinlediği): ![Alternatif metin](./media/quickstart-device-streams-proxy-nodejs/service-console-output.PNG "SSH terminal çıkış")

Konsol çıktısı SSH istemcisi programının (SSH istemcisi iletişim kuran SSH arka plan programı için burada hizmeti-yerel proxy üzerinde dinleme bağlantı noktası 22 bağlanarak): ![Alternatif metin](./media/quickstart-device-streams-proxy-nodejs/ssh-console-output.PNG "SSH istemcisi çıkış")

### <a name="rdp-to-your-device-via-device-streams"></a>Cihazınıza cihaz akışları üzerinden RDP

Şimdi RDP istemci programınız kullanın ve hizmeti proxy (daha önce seçtiğiniz rasgele kullanılabilir bağlantı noktası olduğu) 2222 numaralı bağlantı noktasına bağlanın.

> [!NOTE]
> Cihaz Ara sunucunuz RDP için yapılandırıldığını ve RDP bağlantı noktası 3389'ile yapılandırılmış emin olun.

![Alternatif metin](./media/quickstart-device-streams-proxy-nodejs/rdp-screen-capture.PNG "RDP istemci hizmeti-yerel ara sunucuya bağlanır.")

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IOT hub'ı ayarladınız, kayıtlı bir cihaz ve dağıtılan RDP ve SSH IOT cihazına etkinleştirmek için hizmeti proxy programı. RDP ve SSH trafiği, IOT hub'ı aracılığıyla cihaz akış aracılığıyla tünel. Bu cihaza doğrudan bağlantı gereksinimini ortadan kaldırır.

Cihaz akışları hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın:

> [!div class="nextstepaction"]
> [Cihaz akışları genel bakış](./iot-hub-device-streams-overview.md)
