---
title: Azure IOT Hub cihaz akışları C# SSH ve RDP (Önizleme) için hızlı başlangıç | Microsoft Docs
description: Bu hızlı başlangıçta, iki örnek çalıştırma C# SSH ve RDP senaryolarından, bir IOT Hub cihaz akış üzerinden uygulamaları.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: rezas
ms.openlocfilehash: 1d5fbb410a61419f6f6d2e80cdb1a16c07672fe9
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66733334"
---
# <a name="quickstart-enable-ssh-and-rdp-over-an-iot-hub-device-stream-by-using-a-c-proxy-application-preview"></a>Hızlı Başlangıç: SSH ve RDP kullanarak bir IOT Hub cihaz akış üzerinden etkinleştirme bir C# Ara sunucu uygulamasını (Önizleme)

[!INCLUDE [iot-hub-quickstarts-4-selector](../../includes/iot-hub-quickstarts-4-selector.md)]

Microsoft Azure IOT Hub cihaz akışları olarak şu anda destekleyen bir [önizleme özelliği](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[IOT Hub cihaz akışları](iot-hub-device-streams-overview.md) güvenli ve güvenlik duvarı uyumlu bir şekilde iletişim kurmak hizmet ve cihaz uygulamalarınıza izin verin. Bu Hızlı Başlangıç Kılavuzu, iki içerir C# (örneğin, bir IOT hub'ı aracılığıyla kurulan bir cihaz akış üzerinden gönderilmesi için Secure Shell [SSH] ve Uzak Masaüstü Protokolü [RDP]. istemci-sunucu uygulama trafiğini etkinleştiren uygulamalar Kurulum genel bakış için bkz. [yerel proxy uygulama örneği için SSH veya RDP](iot-hub-device-streams-overview.md#local-proxy-sample-for-ssh-or-rdp).

Bu makalede, ilk kurulumu için (22 numaralı bağlantı noktasını kullanarak) SSH açıklar ve ardından Kurulum'ın bağlantı noktası için RDP değiştirme işlemi açıklanmaktadır. Cihaz akışları uygulama ve protokolü-depolamadan bağımsız, çünkü aynı örnek uygulama trafiği diğer türleri uyum sağlayacak şekilde değiştirilebilir. Bu değişikliği, genelde yalnızca hedeflenen uygulama tarafından kullanılan bir iletişim bağlantı noktası değiştirilmesini kapsar.

## <a name="how-it-works"></a>Nasıl çalışır?

Aşağıdaki şekilde, bu örnek yerel cihaz ve hizmet yerel ara sunucu uygulamalarında SSH istemcisi ve SSH arka plan işlemleri arasındaki uçtan uca bağlantısını etkinleştirin gösterilmektedir. Burada, arka plan programı cihaz yerel ara sunucu uygulamasını aynı cihaz üzerinde çalıştığını varsayar.

![Yerel proxy Uygulama Kurulumu](./media/quickstart-device-streams-proxy-csharp/device-stream-proxy-diagram.svg)

1. Hizmet yerel ara sunucu uygulamasını, IOT hub'ına bağlanır ve bir cihaz akışını hedef cihaza başlatır.

1. Cihaz yerel ara sunucu uygulamasını akış başlatma el sıkışma işlemi tamamlandıktan ve IOT hub'ın hizmet tarafına akış uç noktası aracılığıyla uçtan uca bir akış tüneli oluşturur.

1. Cihaz yerel ara sunucu uygulamasını cihaza 22 numaralı bağlantı noktasında dinleme SSH arka plan programı bağlanır. Bu ayar, "cihaz yerel ara sunucu uygulamasını Çalıştır" bölümünde açıklanan şekilde yapılandırılabilir, desteklenir.

1. 2222 numaralı bağlantı noktasına bu durumda olan belirtilen bir bağlantı noktasında dinleme tarafından bir kullanıcı yeni SSH bağlantıları için hizmet yerel ara sunucu uygulamasını bekler. Bu ayar, "service-yerel ara sunucu uygulamasını Çalıştır" bölümünde açıklanan şekilde yapılandırılabilir, desteklenir. Kullanıcı SSH istemcisi bağlandığında, SSH istemcisi ve sunucusu uygulama arasında aktarılmak SSH uygulama trafiği tüneli etkinleştirir.

> [!NOTE]
> Bir cihaz akış üzerinden gönderilen SSH trafiği IOT hub'ının akış uç noktası aracılığıyla tünel yerine doğrudan hizmet ve cihaz arasında gönderilen. Daha fazla bilgi için [IOT Hub cihaz akışları kullanmanın avantajları](iot-hub-device-streams-overview.md#benefits).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* Cihaz akışları önizlemesi, şu anda şu bölgelerde oluşturulan yalnızca IOT hub'ları için desteklenir:

  * Orta ABD
  * Orta ABD EUAP

* Bu hızlı başlangıçta çalışan iki örnek uygulamaları kullanılarak yazılan C#. Geliştirme makinenize .NET Core SDK'sını 2.1.0 veya sonraki bir sürümü gerekir.

  İndirebileceğiniz [net'ten birden çok platform için .NET Core SDK](https://www.microsoft.com/net/download/all).

* Geçerli sürümünü doğrulama C# geliştirme makinenizde aşağıdaki komutu kullanarak:

    ```
    dotnet --version
    ```

* Azure IOT uzantısı için Azure CLI Cloud Shell Örneğinize eklemek için aşağıdaki komutu çalıştırın. IOT Hub, IOT Edge ve IOT cihaz sağlama hizmeti (DPS) IOT uzantısını ekler-Azure CLI için özel komutları.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

* [Örneği indirmek C# proje](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip)ve ZIP arşivini ayıklayın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub-device-streams](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta, bir simülasyon cihazı kaydetmek için Azure Cloud Shell kullanın.

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

1. IOT hub'ınıza bağlanmak ve bir cihaz akışını oluşturmak için ayrıca gerekir *hizmet bağlantı dizesini* Hizmet tarafı uygulamasını etkinleştirmek için IOT hub'ından. Aşağıdaki komut, IOT hub'ınız için bu değeri alır:

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

Git *cihaza akışları cihaz proxy* sıkıştırması proje klasörünüzde dizin. Aşağıdaki bilgiler yararlı bulundurun:

| Bağımsız değişken adı | Bağımsız değişken değeri |
|----------------|-----------------|
| `deviceConnectionString` | Daha önce oluşturduğunuz cihaz bağlantı dizesi. |
| `targetServiceHostName` | Burada SSH sunucunun dinlediği IP adresi. Adresi olacaktır `localhost` cihaz yerel ara sunucu uygulamasını çalıştığı aynı IP olsaydı. |
| `targetServicePort` | Uygulama protokolü tarafından kullanılan bağlantı noktası (SSH için varsayılan olarak, bu bağlantı noktası 22 olacaktır).  |

Derleyin ve kodun aşağıdaki gibi çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/device/

# Build the application
dotnet build

# Run the application
# In Linux or macOS
dotnet run $deviceConnectionString localhost 22

# In Windows
dotnet run %deviceConnectionString% localhost 22
```

### <a name="run-the-service-local-proxy-application"></a>Hizmet yerel ara sunucu uygulamasını çalıştırın

Gidin `device-streams-proxy/service` sıkıştırması proje klasörünüzde. Aşağıdaki bilgiler yararlı gerekir:

| Parametre adı | Parametre değeri |
|----------------|-----------------|
| `iotHubConnectionString` | IOT Hub'ınıza hizmet bağlantı dizesi. |
| `deviceId` | Daha önce oluşturduğunuz cihaz tanımlayıcısı. |
| `localPortNumber` | SSH istemciniz bağlanacağı yerel bağlantı noktası. 2222 numaralı bağlantı noktasına bu kullanıyoruz, ancak diğer rastgele sayıları kullanabilirsiniz. |

Derleyin ve kodun aşağıdaki gibi çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/service/

# Build the application
dotnet build

# Run the application
# In Linux or macOS
dotnet run $serviceConnectionString MyDevice 2222

# In Windows
dotnet run %serviceConnectionString% MyDevice 2222
```

### <a name="run-the-ssh-client"></a>SSH istemcisi çalıştırın

Artık SSH istemci uygulamanızı kullanın ve hizmet yerel ara sunucu uygulamasını (yerine SSH arka plan programı doğrudan) 2222 numaralı bağlantı noktasında bağlanın.

```
ssh <username>@localhost -p 2222
```

Bu noktada, SSH penceresi, oturum açma kimlik bilgilerinizi girmenizi ister.

(Hizmeti-yerel ara sunucu uygulamasını 2222 numaralı bağlantı noktasında dinler) hizmet tarafında konsol çıkışı:

![Proxy hizmeti-yerel uygulama çıktısı](./media/quickstart-device-streams-proxy-csharp/service-console-output.png)

Konsol çıktısı SSH arka plan programı bağlanan cihazın yerel proxy aplikaci *IP_address:22*:

![Cihaz yerel proxy uygulama çıktısı](./media/quickstart-device-streams-proxy-csharp/device-console-output.png)

SSH istemci uygulamasının konsol çıkışı. SSH istemcisi, hangi hizmet yerel ara sunucu uygulamasını dinleme yaptığı bağlantı noktası 22'yi bağlanarak SSH arka plan programı iletişim kurar:

![SSH istemcisi uygulama çıktısı](./media/quickstart-device-streams-proxy-csharp/ssh-console-output.png)

## <a name="rdp-to-a-device-via-device-streams"></a>Bir cihazın cihaz akışları yoluyla RDP

Kurulum için RDP, SSH (yukarıda açıklanmıştır) için Kurulum çok benzer. Bunun yerine 3389 numaralı bağlantı noktası RDP hedef IP kullanın ve RDP istemcisi (yerine, SSH istemcisi) kullanın.

### <a name="run-the-device-local-proxy-application-rdp"></a>Cihaz yerel ara sunucu uygulamasını (RDP) çalıştırın

Git *cihaza akışları cihaz proxy* sıkıştırması proje klasörünüzde dizin. Aşağıdaki bilgiler yararlı bulundurun:

| Bağımsız değişken adı | Bağımsız değişken değeri |
|----------------|-----------------|
| `DeviceConnectionString` | Daha önce oluşturduğunuz cihaz bağlantı dizesi. |
| `targetServiceHostName` | Konak adı veya IP adresi RDP sunucusuna çalıştığı. Adresi olacaktır `localhost` cihaz yerel ara sunucu uygulamasını çalıştığı aynı IP olsaydı. |
| `targetServicePort` | Uygulama protokolü tarafından kullanılan bağlantı noktası (RDP için varsayılan olarak, bu bağlantı noktası 3389 olacaktır).  |

Derleyin ve kodun aşağıdaki gibi çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/device

# Run the application
# In Linux or macOS
dotnet run $DeviceConnectionString localhost 3389

# In Windows
dotnet run %DeviceConnectionString% localhost 3389
```

### <a name="run-the-service-local-proxy-application-rdp"></a>Hizmet yerel ara sunucu uygulamasını (RDP) çalıştırın

Gidin `device-streams-proxy/service` sıkıştırması proje klasörünüzde. Aşağıdaki bilgiler yararlı gerekir:

| Parametre adı | Parametre değeri |
|----------------|-----------------|
| `iotHubConnectionString` | IOT Hub'ınıza hizmet bağlantı dizesi. |
| `deviceId` | Daha önce oluşturduğunuz cihaz tanımlayıcısı. |
| `localPortNumber` | SSH istemciniz bağlanacağı yerel bağlantı noktası. 2222 numaralı bağlantı noktasına bu kullanıyoruz, ancak bu diğer rastgele sayılar için değiştirebilir. |

Derleyin ve kodun aşağıdaki gibi çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/service/

# Build the application
dotnet build

# Run the application
# In Linux or macOS
dotnet run $serviceConnectionString MyDevice 2222

# In Windows
dotnet run %serviceConnectionString% MyDevice 2222
```

### <a name="run-rdp-client"></a>RDP istemcisi çalıştırın

Şimdi RDP istemci uygulamanızı kullanın ve hizmet yerel proxy uygulamayı (daha önce seçtiğiniz rastgele kullanılabilir bir bağlantı olduğu) 2222 numaralı bağlantı noktasına bağlanın.

![RDP proxy hizmeti-yerel uygulamaya bağlanır](./media/quickstart-device-streams-proxy-csharp/rdp-screen-capture.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IOT hub'ı ayarladınız, kayıtlı bir cihaz, dağıtılan bir IOT hub cihaz akışı oluşturmak için yerel cihaz ve hizmet yerel proxy uygulamalar ve proxy uygulamaları, SSH veya RDP trafiğine tünel oluşturmak için kullanılan. Diğer istemci-sunucu, sunucuyu (örneğin, SSH arka plan programı) cihazda çalıştığı protokoller, aynı paradigma barındırabilir.

Cihaz akışları hakkında daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
> [Cihaz akışları genel bakış](./iot-hub-device-streams-overview.md)
