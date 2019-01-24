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
ms.date: 01/15/2019
ms.author: rezas
ms.openlocfilehash: 012fdfa4faf10cacaf85819517f358c1af1ab39d
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54830215"
---
# <a name="quickstart-sshrdp-over-iot-hub-device-streams-using-nodejs-proxy-application-preview"></a>Hızlı Başlangıç: SSH/RDP üzerinden Node.js Ara sunucu uygulamasını (Önizleme) kullanarak IOT Hub cihaz akışları

[!INCLUDE [iot-hub-quickstarts-4-selector](../../includes/iot-hub-quickstarts-4-selector.md)]

[IOT Hub cihaz akışları](./iot-hub-device-streams-overview.md) güvenli ve güvenlik duvarı uyumlu bir şekilde iletişim kurmak hizmet ve cihaz uygulamalarınıza izin verin. Bu hızlı başlangıçta, bir cihaz akış üzerinden cihaza gönderilecek SSH ve RDP trafiğini etkinleştirmek için hizmet tarafında çalışan bir Node.js proxy uygulamanın yürütülmesini açıklar. Bkz: [bu sayfayı](./iot-hub-device-streams-overview.md#local-proxy-sample-for-ssh-or-rdp) Kurulum genel bakış. Genel Önizleme süresince Node.js SDK'sı yalnızca hizmet tarafında cihaz akışlarını destekler. Sonuç olarak, bu hızlı başlangıçta, yalnızca hizmet tarafı proxy çalıştırma yönergeleri kapsar. Kullanılabilir eşlik eden bir aygıt tarafı proxy çalıştırmalısınız [C hızlı](./quickstart-device-streams-proxy-c.md) veya [ C# hızlı](./quickstart-device-streams-proxy-csharp.md) Kılavuzlar.

SSH için öncelikle Kurulumu açıklanmaktadır (bağlantı noktası kullanarak `22`). Biz sonra nasıl Kurulum RDP için (Bu bağlantı noktası 3389'ı kullanır) değiştirileceğini açıklar. Cihaz akışlar, uygulama ve protokolü belirsiz olduğundan, aynı örnek (genellikle iletişim bağlantı noktalarını değiştirerek) değiştirilebilir uygulama trafiği diğer türleri uyum sağlamak için.

Kod başlatma ve cihaz akışını kullanımını gösteren ve diğer uygulama trafiği de (dışında RDP ve SSH) güvenilemez.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta Hizmet tarafı uygulamayı çalıştırmak için geliştirme makinenizi Node.js v4.x.x veya sonraki bir sürümü gerekir.

[nodejs.org](https://nodejs.org) adresinden birden fazla platform için Node.js’yi indirebilirsiniz.

Aşağıdaki komutu kullanarak geliştirme makinenizde geçerli Node.js sürümünü doğrulayabilirsiniz:

```
node --version
```

Örnek Node.js projesini önceden indirmediyseniz https://github.com/Azure-Samples/azure-iot-samples-node/archive/streams-preview.zip adresinden indirip ZIP arşivini ayıklayın.


## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Önceki tamamladıysanız [hızlı başlangıç: Bir IOT hub'ına bir CİHAZDAN telemetri gönderme](quickstart-send-telemetry-node.md), bu adımı atlayabilirsiniz.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub-device-streams.md)]


## <a name="register-a-device"></a>Cihaz kaydetme

Önceki tamamladıysanız [hızlı başlangıç: Bir IOT hub'ına bir CİHAZDAN telemetri gönderme](quickstart-send-telemetry-node.md), bu adımı atlayabilirsiniz.

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Aşağıdaki komutları Azure Cloud Shell'de çalıştırarak IoT Hub CLI uzantısını ekleyin ve cihaz kimliğini oluşturun. 

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adı ile değiştirin.

   **Cihazım**: Bu, kayıtlı bir cihaz için verilen addır. Cihazım gösterildiği gibi kullanın. Cihazınız için farklı bir ad seçerseniz bu makalenin geri kalan bölümünde aynı adı kullanmanız ve örnek uygulamaları çalıştırmadan önce bunlarda da cihaz adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

2. Arka uç uygulamasının IoT hub’ınıza bağlanmasına ve iletileri almasına olanak sağlamak için bir _hizmet bağlantı dizesi_ de gerekir. Aşağıdaki komut, IoT hub'ınız için hizmeti bağlantı dizesini alır:

    **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adı ile değiştirin.

    ```azurecli-interactive
    az iot hub show-connection-string --policy-name service --hub-name YourIoTHubName
    ```

    Şuna benzer döndürülen değeri not edin:

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`


## <a name="ssh-to-a-device-via-device-streams"></a>Bir cihazın cihaz akışları yoluyla SSH

### <a name="run-the-device-side-proxy"></a>Aygıt tarafı proxy çalıştırma

Daha önce belirtildiği gibi IOT Hub Node.js SDK'sı hizmet tarafında yalnızca cihaz akışlarını destekler. Aygıt tarafı uygulaması için eşlik eden cihazın proxy programlar kullanılabilir kullanın [C hızlı](./quickstart-device-streams-proxy-c.md) veya [ C# hızlı](./quickstart-device-streams-proxy-csharp.md) Kılavuzlar. Aygıt tarafı proxy, sonraki adıma devam etmeden önce çalıştığından emin olun.


### <a name="run-the-service-side-proxy"></a>Hizmet tarafı proxy çalıştırma

Aygıt tarafı proxy çalıştıran varsayıldığında, node.js'de yazılmış Hizmet tarafı proxy çalıştırmak için aşağıdaki adımları izleyin:

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
Değişiklik `MyDevice` cihazınız için seçtiğiniz cihaz kimliği.

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

Konsol SSH oturum kurulduktan sonra üzerinde hizmet tarafı çıktısı (2222 numaralı bağlantı noktasında service-yerel proxy dinlediği): ![Alternatif metin](./media/quickstart-device-streams-proxy-nodejs/service-console-output.PNG "SSH terminal çıkış")


Konsol çıktısı SSH istemcisi programının (SSH istemcisi bağlantı noktasına bağlanarak SSH arka plan programı için iletişim kuran <code>22</code> nerede hizmet yerel proxy üzerinde dinleme): ![Alternatif metin](./media/quickstart-device-streams-proxy-nodejs/ssh-console-output.PNG "SSH istemcisi çıkış")


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
