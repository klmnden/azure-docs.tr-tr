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
ms.openlocfilehash: 40d5a02f83188330facc82701abdfb950585781c
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2018
ms.locfileid: "49310407"
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

## <a name="usage"> </a> Üçüncü taraf hizmetlerle tümleştirme
Azure SignalR hizmeti, üçüncü taraf hizmetlerin sistemle tümleştirilmesini sağlar.
### <a name="usage"> </a> Teknik özelliklerin tanımı
Aşağıdaki tabloda, desteklenen tüm REST API sürümleri gösterilmektedir. Belirli bir sürüme ait tanım dosyasını da bulabilirsiniz
Sürüm | API Durumu | Kapı | Özel
--- | --- | --- | ---
`1.0-preview` | Kullanılabilir | 5002 | [Swagger] (https://github.com/Azure/azure-signalr/tree/dev/docs/swagger/v1-preview.json)
`1.0` | Kullanılabilir | Standart | [Swagger] (https://github.com/Azure/azure-signalr/tree/dev/docs/swagger/v1.json)
Her sürüm için kullanılabilir API'lerin listesi aşağıda verilmiştir.
API | `1.0-preview` | `1.0`
--- | --- | ---
[Tümüne yayın] (# broadcast) | : heavy_check_mark: | : Heavy_check_mark:
[Gruba yayın] (# broadcast-group) | : heavy_check_mark: | : Heavy_check_mark:
Bazı gruplara yayın | : heavy_check_mark: (Kullanım dışı) | `N / A`
[Belirli kullanıcılara gönder] (# send-user) | : heavy_check_mark: | : Heavy_check_mark:
Bazı kullanıcılara gönder | : heavy_check_mark: (Kullanım dışı) | `N / A`
[Gruba kullanıcı ekleme] (# add-user-to-group) | `N / A` | : Heavy_check_mark:
[Gruptan kullanıcı kaldırma] (# remove-user-from-group) | `N / A` | : Heavy_check_mark:
<a name="broadcast"> </a>
### <a name="broadcast-to-everyone"></a>Herkese yayınlama
Sürüm | API HTTP Yöntemi | İstek URL'si | İstek gövdesi
--- | --- | --- | ---
`1.0-preview` | `POST` | `https: // <instance-name> .service.signalr.net: 5002 / api / v1-preview / hub / <hub-name>` | `{" target ":" <method-name> "," arguments ": [...]}`
`1.0` | `POST` | `https: // <instance-name> .service.signalr.net / api / v1 / hubs / <hub-name>` | Yukarıdaki gibi
<a name="broadcast-group"> </a>
### <a name="broadcast-to-a-group"></a>Gruba yayınla
Sürüm | API HTTP Yöntemi | İstek URL'si | İstek gövdesi
--- | --- | --- | ---
`1.0-preview` | `POST` | `https: // <instance-name> .service.signalr.net: 5002 / api / v1-preview / hub / <hub-name> / group / <group-name>` | `{" target ":" <method-name> "," arguments ": [...]}`
`1.0` | `POST` | `https: // <instance-name> .service.signalr.net / api / v1 / hubs / <hub-name> / groups / <group-name>` | Yukarıdakiyle aynı
<a name="send-user"> </a>
### <a name="sending-to-specific-users"></a>Belirli kullanıcılara gönderme
Sürüm | API HTTP Yöntemi | İstek URL'si | İstek gövdesi
--- | --- | --- | ---
`1.0-preview` | `POST` | `https: // <instance-name> .service.signalr.net: 5002 / api / v1-preview / hub / <hub-name> / user / <user-id>` | `{" target ":" <method-name> "," arguments ": [...]}`
`1.0` | `POST` | `https: // <instance-name> .service.signalr.net / api / v1 / hubs / <hub-name> / users / <user-id>` | Yukarıdakiyle aynı
<a name="add-user-to-group"> </a>
### <a name="adding-a-user-to-a-group"></a>Gruba kullanıcı ekleme
Sürüm | API HTTP Yöntemi | İstek URL'si
--- | --- | ---
`1.0` | `PUT` | `Https: // <instance-name> .service.signalr.net / api / v1 / hubs / <hub-name> / groups / <group-name> / users / <userid>`
<a name="remove-user-from-group"> </a>
### <a name="removing-a-user-from-a-group"></a>Gruptan kullanıcı kaldırma
Sürüm | API HTTP Yöntemi | İstek URL'si
--- | --- | ---
`1.0` | `DELETE` | `Https: // <instance-name> .service.signalr.net / api / v1 / hubs / <hub-name> / groups / <group-name> / users / <userid>`

[!INCLUDE [Cleanup](includes/signalr-quickstart-cleanup.md)]
