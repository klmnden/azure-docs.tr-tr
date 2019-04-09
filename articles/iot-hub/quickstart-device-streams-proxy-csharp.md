---
title: Azure IOT Hub cihaz akışları C Hızlı Başlangıç için SSH/RDP (Önizleme) | Microsoft Docs
description: Bu hızlı başlangıçta, iki örnek çalışacak C# bir IOT Hub cihaz akış üzerinden SSH/RDP senaryolarından uygulamalar.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: rezas
ms.openlocfilehash: d36737e6007f247777689e2afa9f47b3ad5bf107
ms.sourcegitcommit: 045406e0aa1beb7537c12c0ea1fbf736062708e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "59006665"
---
# <a name="quickstart-sshrdp-over-iot-hub-device-streams-using-c-proxy-applications-preview"></a>Hızlı Başlangıç: IOT Hub cihaz üzerinde SSH/RDP kullanarak akışları C# proxy uygulamaları (Önizleme)

[!INCLUDE [iot-hub-quickstarts-4-selector](../../includes/iot-hub-quickstarts-4-selector.md)]

Microsoft Azure IOT Hub cihaz akışları olarak şu anda destekleyen bir [önizleme özelliği](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[IOT Hub cihaz akışları](./iot-hub-device-streams-overview.md) güvenli ve güvenlik duvarı uyumlu bir şekilde iletişim kurmak hizmet ve cihaz uygulamalarınıza izin verin. Bu Hızlı Başlangıç Kılavuzu, iki içerir C# istemci/sunucu uygulama trafiği (örneğin, SSH ve RDP), IOT hub'ı aracılığıyla kurulan bir cihaz akış üzerinden gönderilen olanak sağlayan program. Bkz: [burada](./iot-hub-device-streams-overview.md#local-proxy-sample-for-ssh-or-rdp) Kurulum genel bakış.

İlk kurulum için SSH (22 numaralı bağlantı noktasını kullanarak) açıklanmaktadır. Biz, ardından Kurulum'ın bağlantı noktası için RDP değiştirme açıklanmaktadır. Cihaz akışlar, uygulama ve protokolü belirsiz olduğundan, aynı örnek uygulama trafiği diğer türleri uyum sağlayacak şekilde değiştirilebilir. Bu genellikle yalnızca hedeflenen uygulama tarafından kullanılan iletişim bağlantı noktasını değiştirerek içerir.

## <a name="how-it-works"></a>Nasıl çalışır?

Bu örnek cihaz ve hizmet yerel proxy programlarında SSH istemcisi SSH arka plan programı arasında uçtan uca bağlantısı nasıl etkinleştirir Kurulumu aşağıdaki şekilde gösterilmektedir. Burada, arka plan programı cihaz yerel ara sunucu ile aynı cihazda çalıştığını varsayar.

![Alternatif metin](./media/quickstart-device-streams-proxy-csharp/device-stream-proxy-diagram.svg "yerel ara Kurulumu")

1. Hizmet yerel proxy IOT hub'ına bağlanır ve cihaz kimliğini kullanarak hedef cihaza bir cihaz akışını başlatır

2. Cihaz yerel proxy akış başlatma el sıkışma işlemi tamamlandıktan ve IOT Hub'ın hizmet tarafına akış uç noktası aracılığıyla uçtan uca bir akış tüneli oluşturur.

3. Cihazda 22 numaralı bağlantı noktasında dinleme SSH arka plan programı (SSHD) cihaz yerel proxy bağlandığında (Bu bağlantı noktası açıklandığı şekilde yapılandırılabilir, [aşağıda](#run-the-device-local-proxy)).

4. Kullanıcıdan yeni SSH bağlantıları için bekler, bu durumda 2222 numaralı bağlantı noktasına atanan bir bağlantı noktasında dinleme tarafından hizmet yerel proxy (Bu da açıklandığı gibi yapılandırılabilirdir [aşağıda](#run-the-service-local-proxy)). Kullanıcı SSH istemcisi bağlandığında, SSH istemcisi ve sunucusu programlar arasında değiş tokuş uygulama trafiği tüneli etkinleştirir.

> [!NOTE]
> Akış üzerinden gönderilen SSH trafiği doğrudan hizmet ve cihaz arasında gönderilen yerine IOT Hub'ın akış uç noktası aracılığıyla tünel. Bu sağlar [yararlar](./iot-hub-device-streams-overview.md#benefits).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Cihaz akışları şu anda önizlemesidir yalnızca IOT hub'ları aşağıdaki bölgelerde oluşturulan için desteklenir:

  - **Orta ABD**
  - **Orta ABD EUAP**

Bu hızlı başlangıçta çalıştırdığınız iki örnek uygulama, C# kullanılarak yazılır. Geliştirme makinenizde .NET Core SDK 2.1.0 veya üzeri bir sürüm olması gerekir.

[.NET](https://www.microsoft.com/net/download/all)’ten birden fazla platform için .NET Core SDK’sını indirebilirsiniz.

Aşağıdaki komutu kullanarak geliştirme makinenizde geçerli C# sürümünü doğrulayabilirsiniz:

```
dotnet --version
```

Microsoft Azure IOT uzantısı için Azure CLI Cloud Shell Örneğinize eklemek için aşağıdaki komutu çalıştırın. IOT uzantısı, Azure CLI için IOT Hub, IOT Edge ve IOT cihaz sağlama hizmeti (DPS) belirli komutları ekler.

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip adresinden örnek C# projesini indirin ve ZIP arşivini ayıklayın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Cihaz kimliği oluşturmak için Azure Cloud Shell'de aşağıdaki komutu çalıştırın.

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

   **Cihazım**: Bu, kayıtlı bir cihaz için verilen addır. Cihazım gösterildiği gibi kullanın. Cihazınız için farklı bir ad seçerseniz bu makalenin geri kalan bölümünde aynı adı kullanmanız ve örnek uygulamaları çalıştırmadan önce bunlarda da cihaz adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

2. Yeni kaydettiğiniz cihazın _cihaz bağlantı dizesini_ almak için aşağıdaki komutları Azure Cloud Shell'de çalıştırın:

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyDevice --output table
    ```

    Aşağıdaki gibi görünen cihaz bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız.

3. Ayrıca gerekir _hizmet bağlantı dizesini_ IOT hub'ınıza bağlanmak ve bir cihaz akışını kurmak Hizmet tarafı uygulamasını etkinleştirmek için IOT hub'ından. Aşağıdaki komut, IOT hub'ınız için bu değeri alır:

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

    ```azurecli-interactive
    az iot hub show-connection-string --policy-name service --name YourIoTHubName
    ```

    Şuna benzer döndürülen değeri not edin:

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`
    

## <a name="ssh-to-a-device-via-device-streams"></a>Bir cihazın cihaz akışları yoluyla SSH

### <a name="run-the-device-local-proxy"></a>Cihaz yerel ara sunucu çalıştırın

Gidin `device-streams-proxy/device` sıkıştırması proje klasörünüzde. Aşağıdaki bilgiler yararlı gerekir:

| Bağımsız değişken adı | Bağımsız değişken değeri |
|----------------|-----------------|
| `deviceConnectionString` | Daha önce oluşturduğunuz cihaz bağlantı dizesi. |
| `targetServiceHostName` | SSH sunucusu burada dinlediği IP adresi (Bu `localhost` varsa cihaz yerel proxy çalıştığı aynı IP). |
| `targetServicePort` | Uygulama protokolü tarafından kullanılan bağlantı noktasını (varsayılan olarak, bu SSH için 22 numaralı bağlantı olacaktır).  |

Derleyin ve kodun aşağıdaki gibi çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/device/

# Build the application
dotnet build

# Run the application
# In Linux/MacOS
dotnet run $deviceConnectionString localhost 22

# In Windows
dotnet run %deviceConnectionString% localhost 22
```

### <a name="run-the-service-local-proxy"></a>Hizmet yerel ara sunucu çalıştırın

Gidin `device-streams-proxy/service` sıkıştırması proje klasörünüzde. Aşağıdaki bilgiler yararlı gerekir:

| Parametre adı | Parametre değeri |
|----------------|-----------------|
| `iotHubConnectionString` | IOT Hub'ınıza hizmet bağlantı dizesi. |
| `deviceId` | Daha önce oluşturduğunuz cihaz tanımlayıcısı. |
| `localPortNumber` | SSH istemciniz burada bağlanacak bir yerel bağlantı noktası. 2222 numaralı bağlantı noktasına bu kullanıyoruz, ancak bu diğer rastgele sayılar için değiştirebilir. |

Derleyin ve kodun aşağıdaki gibi çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/service/

# Build the application
dotnet build

# Run the application
# In Linux/MacOS
dotnet run $serviceConnectionString MyDevice 2222

# In Windows
dotnet run %serviceConnectionString% MyDevice 2222
```

### <a name="run-ssh-client"></a>SSH istemcisi çalıştırın

Artık SSH istemcisi programınız kullanın ve yerel hizmet proxy'si (yerine SSH arka plan programı doğrudan) 2222 numaralı bağlantı noktasında bağlanın. 

```
ssh <username>@localhost -p 2222
```

Bu noktada kimlik bilgilerinizi girmeniz için SSH oturum açma istemi ile sunulur.

Konsol (hizmet yerel proxy 2222 numaralı bağlantı noktasında dinler) hizmeti tarafında çıktısı:

![Alternatif metin](./media/quickstart-device-streams-proxy-csharp/service-console-output.png "hizmeti-yerel proxy çıkış")

Konsol çıktısı SSH arka plan programı bağlanan cihazın yerel proxy'de `IP_address:22`:

![Alternatif metin](./media/quickstart-device-streams-proxy-csharp/device-console-output.png "cihaz yerel proxy çıkış")

Konsol çıktısı SSH istemcisi programının (SSH istemcisi iletişim kuran SSH arka plan programı için burada hizmeti-yerel proxy üzerinde dinleme bağlantı noktası 22 bağlanarak):

![Alternatif metin](./media/quickstart-device-streams-proxy-csharp/ssh-console-output.png "SSH istemcisi program çıktısı")


## <a name="rdp-to-a-device-via-device-streams"></a>Bir cihazın cihaz akışları yoluyla RDP

Kurulum için RDP SSH (yukarıda açıklanmıştır) için benzer. Temel olarak bunun yerine RDP hedef IP ve bağlantı noktası 3389'u kullanın ve RDP istemcisi (yerine, SSH istemcisi) kullanmak ihtiyacımız var.

### <a name="run-the-device-local-proxy-rdp"></a>Cihaz yerel proxy (RDP) çalıştırın

Gidin `device-streams-proxy/device` sıkıştırması proje klasörünüzde. Aşağıdaki bilgiler yararlı gerekir:

| Bağımsız değişken adı | Bağımsız değişken değeri |
|----------------|-----------------|
| `DeviceConnectionString` | Daha önce oluşturduğunuz cihaz bağlantı dizesi. |
| `targetServiceHostName` | Ana bilgisayar adı veya IP adresi RDP sunucusuna çalıştığı (Bu `localhost` varsa cihaz yerel proxy çalıştığı aynı IP). |
| `targetServicePort` | Uygulama protokolü tarafından kullanılan bağlantı noktasını (varsayılan olarak, bu bağlantı noktası 3389 için RDP olacaktır).  |

Derleyin ve kodun aşağıdaki gibi çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/device

# Run the application
# In Linux/MacOS
dotnet run $DeviceConnectionString localhost 3389

# In Windows
dotnet run %DeviceConnectionString% localhost 3389
```

### <a name="run-the-service-local-proxy-rdp"></a>Hizmet yerel proxy (RDP) çalıştırın

Gidin `device-streams-proxy/service` sıkıştırması proje klasörünüzde. Aşağıdaki bilgiler yararlı gerekir:

| Parametre adı | Parametre değeri |
|----------------|-----------------|
| `iotHubConnectionString` | IOT Hub'ınıza hizmet bağlantı dizesi. |
| `deviceId` | Daha önce oluşturduğunuz cihaz tanımlayıcısı. |
| `localPortNumber` | SSH istemciniz burada bağlanacak bir yerel bağlantı noktası. 2222 numaralı bağlantı noktasına bu kullanıyoruz, ancak bu diğer rastgele sayılar için değiştirebilir. |

Derleyin ve kodun aşağıdaki gibi çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/service/

# Build the application
dotnet build

# Run the application
# In Linux/MacOS
dotnet run $serviceConnectionString MyDevice 2222

# In Windows
dotnet run %serviceConnectionString% MyDevice 2222
```

### <a name="run-rdp-client"></a>RDP istemcisi çalıştırın

Şimdi RDP istemci programınız kullanın ve hizmet yerel proxy (daha önce seçtiğiniz rasgele kullanılabilir bağlantı noktası olduğu) 2222 numaralı bağlantı noktasına bağlanın.

![Alternatif metin](./media/quickstart-device-streams-proxy-csharp/rdp-screen-capture.PNG "RDP hizmeti-yerel ara sunucuya bağlanır")

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir IOT hub'ı ayarladınız, kayıtlı bir cihaz, cihaz - ve bir IOT Hub cihaz akışı oluşturmak için bir proxy hizmeti-yerel program dağıttıktan ve proxy'ler tüneli SSH veya RDP trafiği için kullanılır. Aynı paradigma (sunucu cihazda, örneğin, SSH arka plan programı çalıştırıldığı) diğer istemci/sunucu protokollerine barındırabilir.

Cihaz akışları hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın:

> [!div class="nextstepaction"]
> [Cihaz akışları genel bakış](./iot-hub-device-streams-overview.md)
