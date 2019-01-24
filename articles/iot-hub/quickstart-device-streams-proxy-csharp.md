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
ms.date: 01/15/2019
ms.author: rezas
ms.openlocfilehash: 7a6a9a96bbde24fa8340714a8078e015a83d8cfb
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54830239"
---
# <a name="quickstart-sshrdp-over-iot-hub-device-streams-using-c-proxy-applications-preview"></a>Hızlı Başlangıç: IOT Hub cihaz üzerinde SSH/RDP kullanarak akışları C# proxy uygulamaları (Önizleme)

[!INCLUDE [iot-hub-quickstarts-4-selector](../../includes/iot-hub-quickstarts-4-selector.md)]

[IOT Hub cihaz akışları](./iot-hub-device-streams-overview.md) güvenli ve güvenlik duvarı uyumlu bir şekilde iletişim kurmak hizmet ve cihaz uygulamalarınıza izin verin. Bu hızlı başlangıçta iki içerir C# SSH ve RDP trafiğin, IOT hub'ı aracılığıyla kurulan bir cihaz akış üzerinden gönderilmesini sağlayan programlar. Bkz: [bu sayfayı](./iot-hub-device-streams-overview.md#local-proxy-sample-for-ssh-or-rdp) Kurulum genel bakış.

İlk kurulum için SSH (22 numaralı bağlantı noktasını kullanarak) açıklanmaktadır. Biz sonra RDP için Kurulum'ın yapılandırılmış bağlantı noktalarını değiştirmek nasıl açıklar. Cihaz akışlar, uygulama ve protokolü belirsiz olduğundan, aynı örnek (genellikle iletişim bağlantı noktalarını değiştirerek) değiştirilebilir uygulama trafiği diğer türleri uyum sağlamak için.

## <a name="how-it-works"></a>Nasıl çalışır?

Bu örnek cihaz ve hizmet yerel proxy programlarında SSH istemcisi SSH arka plan programı arasında uçtan uca bağlantısı nasıl etkinleştirir Kurulumu aşağıdaki şekilde gösterilmektedir. Burada, arka plan programı cihaz yerel ara sunucu ile aynı cihazda çalıştığını varsayar.

![Alternatif metin](./media/quickstart-device-streams-proxy-csharp/device-stream-proxy-diagram.svg "yerel ara Kurulumu")

1. Hizmet yerel proxy, IOT hub'ına bağlanır ve bir cihaz akışını hedef cihaza başlatır.

2. Cihaz yerel proxy akış başlatma el sıkışma işlemi tamamlandıktan ve IOT Hub'ın hizmet tarafına akış uç noktası aracılığıyla uçtan uca bir akış tüneli oluşturur.

3. Cihazda 22 numaralı bağlantı noktasında dinleme SSH arka plan programı (SSHD) cihaz yerel proxy bağlandığında (Bu açıklandığı gibi yapılandırılabilirdir [aşağıda](#run-the-device-side-application)).

4. Kullanıcıdan yeni SSH bağlantıları için bekler bağlantı noktası bu durumda olan belirtilen bağlantı noktasında dinleme tarafından hizmet yerel proxy `2222` (Bu da açıklandığı gibi yapılandırılabilirdir [aşağıda](#run-the-service-side-application)). Kullanıcı SSH istemcisi bağlandığında, SSH istemcisi ve sunucusu programlar arasında değiş tokuş uygulama trafiği tüneli etkinleştirir.

> [!NOTE]
> Akış üzerinden gönderilen SSH trafiği doğrudan hizmet ve cihaz arasında gönderilen yerine IOT Hub'ın akış uç noktası aracılığıyla tünel. Bu sağlar [yararlar](./iot-hub-device-streams-overview.md#benefits).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta çalıştırdığınız iki örnek uygulama, C# kullanılarak yazılır. Geliştirme makinenizde .NET Core SDK 2.1.0 veya üzeri bir sürüm olması gerekir.

[.NET](https://www.microsoft.com/net/download/all)’ten birden fazla platform için .NET Core SDK’sını indirebilirsiniz.

Aşağıdaki komutu kullanarak geliştirme makinenizde geçerli C# sürümünü doğrulayabilirsiniz:

```
dotnet --version
```

https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip adresinden örnek C# projesini indirin ve ZIP arşivini ayıklayın.


## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Aşağıdaki komutları Azure Cloud Shell'de çalıştırarak IoT Hub CLI uzantısını ekleyin ve cihaz kimliğini oluşturun. 

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

   **Cihazım**: Bu, kayıtlı bir cihaz için verilen addır. Cihazım gösterildiği gibi kullanın. Cihazınız için farklı bir ad seçerseniz bu makalenin geri kalan bölümünde aynı adı kullanmanız ve örnek uygulamaları çalıştırmadan önce bunlarda da cihaz adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
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
    az iot hub show-connection-string --policy-name service --hub-name YourIoTHubName
    ```

    Şuna benzer döndürülen değeri not edin:

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`
    

## <a name="ssh-to-a-device-via-device-streams"></a>Bir cihazın cihaz akışları yoluyla SSH

### <a name="run-the-service-side-proxy"></a>Hizmet tarafı proxy çalıştırma

Gidin `device-streams-proxy/service` sıkıştırması proje klasörünüzde. Aşağıdaki bilgiler yararlı gerekir:

| Parametre adı | Parametre değeri |
|----------------|-----------------|
| `IotHubConnectionString` | IOT Hub'ınıza hizmet bağlantı dizesi. |
| `DeviceId` | Daha önce oluşturduğunuz cihaz tanımlayıcısı. |
| `Port` | SSH istemciniz burada bağlanacak bir yerel bağlantı noktası. Bağlantı noktası kullandığımız `2222` Bu örnek, ancak bu diğer rastgele sayılar için değiştirebilir. |

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

### <a name="run-the-device-local-proxy"></a>Cihaz yerel ara sunucu çalıştırın

Gidin `device-streams-proxy/device` sıkıştırması proje klasörünüzde. Aşağıdaki bilgiler yararlı gerekir:

| Parametre adı | Parametre değeri |
|----------------|-----------------|
| `DeviceConnectionString` | Daha önce oluşturduğunuz cihaz bağlantı dizesi. |
| `RemoteHostName` | IP adresi SSH burada sunucu (Bu `localhost` varsa cihaz yerel proxy çalıştığı aynı IP). |
| `RemotePort` | Uygulama protokolü tarafından kullanılan bağlantı noktasını (varsayılan olarak, bu SSH için 22 numaralı bağlantı olacaktır).  |

Derleyin ve kodun aşağıdaki gibi çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/device/

# Build the application
dotnet build

# Run the application
# In Linux/MacOS
dotnet run $DeviceConnectionString localhost 22

# In Windows
dotnet run %DeviceConnectionString% localhost 22
```

Artık SSH istemcisi programınız kullanın ve bağlantı noktası proxy hizmeti-yerel bağlanmak `2222` (yerine SSH arka plan programı doğrudan). 

```
ssh <username>@localhost -p 2222
```

Bu noktada kimlik bilgilerinizi girmeniz için SSH oturum açma istemi ile sunulur.

Konsol (hizmet yerel proxy 2222 numaralı bağlantı noktasında dinler) hizmeti tarafında çıktısı: ![Alternatif metin](./media/quickstart-device-streams-proxy-csharp/service-console-output.png "hizmeti-yerel proxy çıkış")

Konsol çıktısı SSH arka plan programı bağlanan cihazın yerel proxy'de <code>IP_address:22</code>: ![Alternatif metin](./media/quickstart-device-streams-proxy-csharp/device-console-output.png "cihaz yerel proxy çıkış")

Konsol çıktısı SSH istemcisi programının (SSH istemcisi iletişim kuran SSH arka plan programı için burada hizmeti-yerel proxy üzerinde dinleme bağlantı noktası 22 bağlanarak): ![Alternatif metin](./media/quickstart-device-streams-proxy-csharp/ssh-console-output.png "SSH istemcisi program çıktısı")

## <a name="rdp-to-a-device-via-device-streams"></a>Bir cihazın cihaz akışları yoluyla RDP

Kurulum için RDP SSH (yukarıda açıklanmıştır) için benzer. Temel olarak bunun yerine RDP hedef IP ve bağlantı noktası 3389'u kullanın ve RDP istemcisi (yerine, SSH istemcisi) kullanmak ihtiyacımız var.

### <a name="run-the-service-side-application"></a>Hizmet tarafı uygulamayı çalıştırın

Gidin `device-streams-proxy/service` sıkıştırması proje klasörünüzde. Aşağıdaki bilgiler yararlı gerekir:

| Parametre adı | Parametre değeri |
|----------------|-----------------|
| `IotHubConnectionString` | IOT Hub'ınıza hizmet bağlantı dizesi. |
| `DeviceId` | Daha önce oluşturduğunuz cihaz tanımlayıcısı. |
| `Port` | SSH istemciniz burada bağlanacak bir yerel bağlantı noktası. Bağlantı noktası kullandığımız `2222` Bu örnek, ancak bu diğer rastgele sayılar için değiştirebilir. |

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

### <a name="run-the-device-side-application"></a>Aygıt tarafı uygulamayı çalıştırın

Gidin `device-streams-proxy/device` sıkıştırması proje klasörünüzde. Aşağıdaki bilgiler yararlı gerekir:

| Parametre adı | Parametre değeri |
|----------------|-----------------|
| `DeviceConnectionString` | Daha önce oluşturduğunuz cihaz bağlantı dizesi. |
| `RemoteHostName` | IP adresi nerede RDP sunucusuna (Bu `localhost` varsa cihaz yerel proxy çalıştığı aynı IP). |
| `RemotePort` | Uygulama protokolü tarafından kullanılan bağlantı noktasını (varsayılan olarak, bu bağlantı noktası 3389 için RDP olacaktır).  |

Derleyin ve kodun aşağıdaki gibi çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/device

# Run the application
# In Linux/MacOS
dotnet run $DeviceConnectionString localhost 3389

# In Windows
dotnet run %DeviceConnectionString% localhost 3389
```

Şimdi RDP istemci programınız kullanın ve hizmeti-yerel proxy bağlantı noktası üzerinde bağlama `2222` (önceden seçtiğiniz bir rasgele kullanılabilir bağlantı noktası önceden).

![Alternatif metin](./media/quickstart-device-streams-proxy-csharp/rdp-screen-capture.PNG "RDP hizmeti-yerel ara sunucuya bağlanır")

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir IOT hub'ı ayarladınız, kayıtlı bir cihaz, cihaz - ve bir IOT Hub cihaz akışı oluşturmak için bir proxy hizmeti-yerel program dağıttıktan ve proxy'ler tüneli SSH veya RDP trafiği için kullanılır. Aynı paradigma (sunucu cihazda, örneğin, SSH arka plan programı çalıştırıldığı) diğer istemci/sunucu protokollerine barındırabilir.

Cihaz akışları hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın:

> [!div class="nextstepaction"]
> [Cihaz akışları genel bakış](./iot-hub-device-streams-overview.md)
