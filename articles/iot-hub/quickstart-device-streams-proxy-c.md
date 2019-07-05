---
title: Azure IOT Hub cihaz akışları C Hızlı Başlangıç için SSH ve RDP (Önizleme) | Microsoft Docs
description: Bu hızlı başlangıçta, IOT Hub üzerinde SSH ve RDP senaryoları etkinleştirmek için bir proxy görevi gören bir örnek C uygulama cihaz akışları çalıştırın.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: robinsh
ms.openlocfilehash: b958711c498f0826f2a48d92d4892eb43ec8d18a
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446072"
---
# <a name="quickstart-enable-ssh-and-rdp-over-an-iot-hub-device-stream-by-using-a-c-proxy-application-preview"></a>Hızlı Başlangıç: SSH ve RDP C Ara sunucu uygulamasını (Önizleme) kullanarak bir IOT Hub cihaz akış üzerinden etkinleştirme

[!INCLUDE [iot-hub-quickstarts-4-selector](../../includes/iot-hub-quickstarts-4-selector.md)]

Azure IOT Hub cihaz akışları olarak şu anda destekleyen bir [önizleme özelliği](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[IOT Hub cihaz akışları](./iot-hub-device-streams-overview.md) güvenli ve güvenlik duvarı uyumlu bir şekilde iletişim kurmak hizmet ve cihaz uygulamalarınıza izin verin. Kurulum genel bakış için bkz. [yerel Proxy örnek sayfasına](./iot-hub-device-streams-overview.md#local-proxy-sample-for-ssh-or-rdp).

Bu hızlı başlangıçta, cihaz akışları aracılığıyla güvenli Kabuk (SSH) trafiği (22 numaralı bağlantı noktasını kullanarak) tünel Kurulumu açıklanır. Uzak Masaüstü Protokolü (RDP) trafiği için Kurulum benzer ve basit bir yapılandırma değişikliği gerektiriyor. Cihaz akışları uygulama ve protokolü-depolamadan bağımsız, çünkü uygulama trafiği diğer türleri uyum sağlamak için bu hızlı başlangıçta değiştirebilirsiniz.

## <a name="how-it-works"></a>Nasıl çalışır?

Aşağıdaki şekilde, cihaz ve hizmet yerel proxy programları SSH istemcisi ve SSH arka plan işlemleri arasında uçtan uca bağlantı nasıl etkinleştirin gösterilmektedir. Genel Önizleme süresince C SDK'sı cihaz tarafında yalnızca cihaz akışlarını destekler. Sonuç olarak, bu hızlı başlangıçta yalnızca yerel cihaz Ara sunucu uygulamasını çalıştırmak için yönergeler içerir. Aşağıdaki hizmet tarafına quickstarts çalıştırmalısınız:

* [IOT Hub cihaz üzerinde SSH/RDP kullanarak akışları C# proxy](./quickstart-device-streams-proxy-csharp.md)
* [SSH/RDP üzerinden NodeJS proxy kullanarak IOT Hub cihaz akışları](./quickstart-device-streams-proxy-nodejs.md).

![Kurulum yerel Ara](./media/quickstart-device-streams-proxy-csharp/device-stream-proxy-diagram.svg)

1. Hizmet yerel proxy, IOT hub'ına bağlanır ve bir cihaz akışını hedef cihaza başlatır.

2. Cihaz yerel proxy akış başlatma el sıkışma işlemi tamamlandıktan ve IOT hub'ın hizmet tarafına akış uç noktası aracılığıyla uçtan uca bir akış tüneli oluşturur.

3. Cihaz yerel proxy cihazda 22 numaralı bağlantı noktasında dinleme SSH arka plan programı bağlanır. Bu ayar, "cihaz yerel ara sunucu uygulamasını Çalıştır" bölümünde açıklanan şekilde yapılandırılabilir, desteklenir.

4. 2222 numaralı bağlantı noktasına bu durumda olan belirtilen bir bağlantı noktasında dinleme tarafından bir kullanıcı yeni SSH bağlantıları için hizmet yerel proxy bekler. Bu ayar, "cihaz yerel ara sunucu uygulamasını Çalıştır" bölümünde açıklanan şekilde yapılandırılabilir, desteklenir. Kullanıcı SSH istemcisi bağlandığında, SSH istemcisi ve sunucusu programlar arasında aktarılmak SSH uygulama trafiği tüneli etkinleştirir.

> [!NOTE]
> Bir cihaz akış üzerinden gönderilen SSH trafiği IOT hub'ının akış uç noktası aracılığıyla tünel yerine doğrudan hizmet ve cihaz arasında gönderilen. Daha fazla bilgi için [IOT Hub cihaz akışları kullanmanın avantajları](iot-hub-device-streams-overview.md#benefits). Ayrıca, aynı cihaza (veya makinede) çalıştırılan SSH arka plan programı şekilde cihaz yerel proxy olarak gösterilmektedir. Bu hızlı başlangıçta, SSH arka plan programı IP adresini sağlayan cihaz yerel proxy ve farklı makinelerde de çalıştırmak için arka plan programı izin verir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* Cihaz akışları önizlemesi, şu anda şu bölgelerde oluşturulan yalnızca IOT hub'ları için desteklenir:

  * Orta ABD
  * Orta ABD EUAP

* Yükleme [Visual Studio 2019](https://www.visualstudio.com/vs/) ile [ile masaüstü geliştirme C++ ](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) iş yükünün etkinleştirilmiş.
* En son [Git](https://git-scm.com/download/) sürümünü yükleyin.

* Azure IOT uzantısı için Azure CLI Cloud Shell Örneğinize eklemek için aşağıdaki komutu çalıştırın. IOT Hub, IOT Edge ve IOT cihaz sağlama hizmeti (DPS) IOT uzantısını ekler-Azure CLI için özel komutları.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Bu Hızlı Başlangıç için kullandığınız [C için Azure IOT cihaz SDK'sını](iot-hub-device-sdk-c-intro.md). Kopyalama ve oluşturmak için kullanılan bir geliştirme ortamı hazırlama [Azure IOT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c) github'dan. GitHub üzerinde SDK'sı, bu hızlı başlangıçta kullanılan örnek kodu içerir.

1. İndirme [CMake derleme sistemini](https://cmake.org/download/).

    Önemli olduğu, Visual Studio önkoşulları (Visual Studio ve *ile masaüstü geliştirme C++*  iş yükü), makinenizde yüklü *önce* CMake yüklemesi Başlat. Önkoşulların yerinde olduğundan ve yüklemeyi doğruladıktan sonra CMake derleme sistemini yükleyebilirsiniz.

1. Komut istemini veya Git Bash kabuğunu açın. Aşağıdaki komutu yürüterek [Azure IoT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c) GitHub deposunu kopyalayın:

    ```
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive -b public-preview
    ```

    Bu işlem birkaç dakika beklemelisiniz.

1. Oluşturma bir *cmake* aşağıdaki komutta gösterildiği gibi Git deposunun kök dizininde alt ve bu klasöre gidin.

    ```
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

1. Aşağıdaki komutları çalıştırın *cmake* geliştirme istemci Platformunuza özgü SDK'sı sürümünü oluşturmak için dizin.

   * Linux'ta:

      ```bash
      cmake ..
      make -j
      ```

   * Windows Visual Studio 2015 veya 2017 için geliştirici Komut İstemi'nde aşağıdaki komutları çalıştırın. Sanal cihaz için bir Visual Studio çözümü içinde oluşturulacağı *cmake* dizin.

      ```cmd
      rem For VS2015
      cmake .. -G "Visual Studio 14 2015"

      rem Or for VS2017
      cmake .. -G "Visual Studio 15 2017"

      rem Then build the project
      cmake --build . -- /m /p:Configuration=Release
      ```

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub-device-streams](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu bölümde, Azure Cloud Shell ile kullandığınız [IOT uzantısı](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) bir simülasyon cihazı kaydedeceksiniz.

1. Cihaz kimliği oluşturma için Cloud Shell'de aşağıdaki komutu çalıştırın:

   > [!NOTE]
   > * Değiştirin *YourIoTHubName* yer tutucu IOT hub'ınız için seçtiğiniz ada sahip.
   > * Kullanım *Cihazım*gösterildiği gibi. Kayıtlı cihaz için verilen addır. Cihazınız için farklı bir ad seçerseniz, bu makalenin tamamında bu adı kullanın ve bunları çalıştırmadan önce örnek uygulamalar, cihaz adını güncelleştirin.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

1. Alınacak *cihaz bağlantı dizesini* yalnızca kayıtlı bir cihaz için Cloud Shell'de aşağıdaki komutları çalıştırın:

   > [!NOTE]
   > Değiştirin *YourIoTHubName* yer tutucu IOT hub'ınız için seçtiğiniz ada sahip.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyDevice --output table
    ```

    Bu hızlı başlangıçta daha sonra kullanmak için cihaz bağlantı dizesini not edin. Aşağıdaki örneğe benzer şekilde görünür:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

## <a name="ssh-to-a-device-via-device-streams"></a>Bir cihazın cihaz akışları yoluyla SSH

Bu bölümde, SSH trafiği tünel oluşturmak için bir uçtan uca stream oluşturun.

### <a name="run-the-device-local-proxy-application"></a>Cihaz yerel ara sunucu uygulamasını çalıştırın

1. Kaynak dosyayı düzenlemek *iothub_client_c2d_streaming_sample.c* klasöründe *iothub_client/samples/iothub_client_c2d_streaming_sample*ve cihaz bağlantısı dizeniz sağlamak için hedef cihaz IP/ana bilgisayar adı ve SSH bağlantı noktası 22'de:

   ```C
   /* Paste in your iothub connection string  */
   static const char* connectionString = "[Connection string of IoT Hub]";
   static const char* localHost = "[IP/Host of your target machine]"; // Address of the local server to connect to.
   static const size_t localPort = 22; // Port of the local server to connect to.
   ```

1. Örnek derleme:

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_proxy_sample
   make -j
   ```

   ```cmd
   rem In Windows
   rem Go to cmake at root of repository
   cmake --build . -- /m /p:Configuration=Release
   ```

1. Derlenmiş programın cihazda çalıştırın:

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_proxy_sample
   ./iothub_client_c2d_streaming_proxy_sample
   ```

   ```cmd
   rem In Windows
   rem Go to the sample's release folder cmake\iothub_client\samples\iothub_client_c2d_streaming_proxy_sample\Release
   iothub_client_c2d_streaming_proxy_sample.exe
   ```

### <a name="run-the-service-local-proxy-application"></a>Hizmet yerel ara sunucu uygulamasını çalıştırın

"Nasıl çalışır?" bölümünde açıklandığı gibi SSH trafiği tünel oluşturmak için bir uçtan uca stream oluşturma (üzerinde hizmet ve cihaz kenarlar için) her iki ucunda yerel bir ara sunucu gerektirir. Genel Önizleme sırasında IOT Hub C SDK'sı cihaz tarafında yalnızca cihaz akışlarını destekler. Derleme ve hizmet yerel proxy çalıştırmak için şu hızlı başlangıçlarda birindeki yönergeleri izleyin:

   * [IOT Hub cihaz üzerinde SSH/RDP kullanarak akışları C# proxy'si uygulamaları](./quickstart-device-streams-proxy-csharp.md)
   * [SSH/proxy'si uygulamaları Node.js kullanarak IOT Hub cihaz akışları üzerinden RDP](./quickstart-device-streams-proxy-nodejs.md)

### <a name="establish-an-ssh-session"></a>Bir SSH oturumu oluşturur

Cihaz ve hizmet yerel proxy'leri çalıştırıldıktan sonra SSH istemcisi programınız kullanın ve (yerine SSH arka plan programı doğrudan) 2222 numaralı bağlantı noktasında service-yerel ara sunucuya bağlanın.

```cmd/sh
ssh <username>@localhost -p 2222
```

Bu noktada, SSH penceresi, oturum açma kimlik bilgilerinizi girmenizi ister.

SSH arka plan programı bağlanan cihazın yerel ara sunucu üzerinde aşağıdaki görüntüde konsol çıktısı gösterilmektedir `IP_address:22`:

![Cihaz yerel proxy çıkış](./media/quickstart-device-streams-proxy-c/device-console-output.png)

Aşağıdaki görüntüde SSH istemcisi programının konsol çıktısı gösterilmektedir. SSH istemcisi, yerel hizmet proxy dinlediği bağlantı noktası 22'yi bağlanarak SSH arka plan programı iletişim kurar:

![SSH istemcisi çıkış](./media/quickstart-device-streams-proxy-csharp/ssh-console-output.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IOT hub'ı ayarladınız, kayıtlı bir cihaz, cihaz - ve bir IOT Hub cihaz akışı oluşturmak için bir proxy hizmeti-yerel program dağıtılan ve proxy'ler SSH trafiği tünel oluşturmak için kullanılan.

Cihaz akışları hakkında daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
> [Cihaz akışları genel bakış](./iot-hub-device-streams-overview.md)
