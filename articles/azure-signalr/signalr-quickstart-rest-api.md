---
title: Hızlı Başlangıç - Azure SignalR Hizmeti REST API’si | Microsoft Belgeleri
description: Azure SignalR hizmeti REST API'sini kullanmak için bir hızlı başlangıç öğreticisi.
services: signalr
documentationcenter: ''
author: sffamily
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: signalr
ms.devlang: dotnet
ms.topic: quickstart
ms.tgt_pltfrm: ASP.NET
ms.workload: tbd
ms.date: 06/13/2018
ms.author: zhshang
ms.openlocfilehash: 93c1198ecfba6db809228ed6dcd99c705f53926c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46972768"
---
# <a name="quickstart-broadcast-real-time-messages-from-console-app"></a>Hızlı Başlangıç: Konsol uygulamasından gerçek zamanlı iletiler yayımlama

Azure SignalR hizmeti, yayıncılık gibi sunucudan istemciye doğrudan iletişim senaryolarını desteklemek için [REST API](https://github.com/Azure/azure-signalr/blob/dev/docs/rest-api.md)’sini sağlar. REST API çağrısı yapabilen herhangi bir programlama dilini seçebilirsiniz. Bağlı tüm istemcilere, adına göre belirli bir istemciye veya bir istemci grubuna iletiler gönderebilirsiniz.

Bu hızlı başlangıçta C# dilinde bir komut satırı uygulamasından bağlı istemci uygulamalarına nasıl ileti gönderebileceğinizi öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç; macOS, Windows veya Linux üzerinde çalıştırılabilir.
* [.NET Core SDK](https://www.microsoft.com/net/download/core)
* Tercih ettiğiniz bir metin veya kod düzenleyicisi.


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure hesabınızla Azure portalında <https://portal.azure.com/> sayfasında oturum açın.


[!INCLUDE [Create instance](includes/signalr-quickstart-create-instance.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Hizmet dağıtılırken kodu hazırlamaya geçiş yapalım. [GitHub'dan örnek uygulamayı](https://github.com/aspnet/AzureSignalR-samples.git) kopyalayın, SignalR Hizmetinin bağlantı dizesini ayarlayın ve uygulamayı yerel olarak çalıştırın.

1. Bir git terminal penceresi açın. Örnek projeyi kopyalamak istediğiniz klasöre gidin.

1. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulamanın bir kopyasını oluşturur.

    ```bash
    git clone https://github.com/aspnet/AzureSignalR-samples.git
    ```

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

Bu örnek, Azure SignalR Hizmetinin kullanımını gösteren bir konsol uygulamasıdır. İki mod sunar:

- Sunucu modu: Azure SignalR Hizmetinin REST API'sini çağırmak için basit komutlar kullanın.
- İstemci modu: Azure SignalR Hizmetine bağlanarak sunucudan ileti alın.

Ayrıca Azure SignalR Hizmeti ile kimlik doğrulaması için nasıl bir erişim belirteci oluşturacağınızı da öğrenebilirsiniz.

### <a name="build-the-executable-file"></a>Yürütülebilir dosyayı derleme

Örnek olarak macOS osx.10.13-x64 kullanıyoruz. Nasıl diğer platformlar için derleyeceğiniz hakkında [başvuru](https://docs.microsoft.com/dotnet/core/rid-catalog) belgeleri bulabilirsiniz.
```bash
cd AzureSignalR-samples/samples/Serverless/

dotnet publish -c Release -r osx.10.13-x64
```

### <a name="start-a-client"></a>İstemci başlatma

```bash
cd bin/Release/netcoreapp2.1/osx.10.13-x64/

Serverless client <ClientName> -c "<ConnectionString>" -h <HubName>
```

### <a name="start-a-server"></a>Sunucu başlatma

```bash
cd bin/Release/netcoreapp2.1/osx.10.13-x64/

Serverless server -c "<ConnectionString>" -h <HubName>
```

## <a name="run-the-sample-without-publishing"></a>Örneği yayımlamadan çalıştırma

Bir sunucu veya istemci başlatmak için aşağıdaki komutu da çalıştırabilirsiniz

```bash
# Start a server
dotnet run -- server -c "<ConnectionString>" -h <HubName>

# Start a client
dotnet run -- client <ClientName> -c "<ConnectionString>" -h <HubName>
```

### <a name="use-user-secrets-to-specify-connection-string"></a>Bağlantı dizesini belirtmek için kullanıcı gizli dizileri kullanma

`dotnet user-secrets set Azure:SignalR:ConnectionString "<ConnectionString>"` öğesini örneğin kök dizininde çalıştırabilirsiniz. Bundan sonra `-c "<ConnectionString>"` seçeneğine ihtiyacınız kalmaz.

## <a name="usage"></a>Kullanım

Sunucu başlatıldıktan sonra ileti göndermek için komutu kullanın

```
send user <User Id>
send users <User List>
send group <Group Name>
send groups <Group List>
broadcast
```

Farklı istemci adları ile birden çok istemci başlatabilirsiniz.


[!INCLUDE [Cleanup](includes/signalr-quickstart-cleanup.md)]
