---
title: "Azure geçiş .NET standart API'lerini genel bakış | Microsoft Docs"
description: "Azure geçiş .NET standart API genel bakış"
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: b1da9ac1-811b-4df7-a22c-ccd013405c40
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: sethm
ms.openlocfilehash: 58451bae409c74c319f41c38a1cec5f051619e0c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-relay-hybrid-connections-net-standard-api-overview"></a>Azure geçişi karma bağlantıları .NET standart API genel bakış

Bu makalede Azure geçişi karma bağlantıları .NET standart anahtar özetlenmektedir [istemci API](/dotnet/api/microsoft.azure.relay).
  
## <a name="relay-connection-string-builder"></a>Geçiş bağlantı dizesi Oluşturucusu

[RelayConnectionStringBuilder] [ RelayConnectionStringBuilder] sınıfı geçişi karma bağlantılar için belirli bağlantı dizeleri biçimlendirir. Bir bağlantı dizesi biçimi doğrulamak için ya da sıfırdan bir bağlantı dizesi oluşturmak için kullanabilirsiniz. Aşağıdaki kod örneği için bkz:

```csharp
var endpoint = "{Relay namespace}";
var entityPath = "{Name of the Hybrid Connection}";
var sharedAccessKeyName = "{SAS key name}";
var sharedAccessKey = "{SAS key value}";

var connectionStringBuilder = new RelayConnectionStringBuilder()
{
    Endpoint = endpoint,
    EntityPath = entityPath,
    SharedAccessKeyName = sasKeyName,
    SharedAccessKey = sasKeyValue
};
```

Doğrudan bir bağlantı dizesi geçirebilirsiniz `RelayConnectionStringBuilder` yöntemi. Bu işlem, bağlantı dizesi geçerli bir biçimde olduğunu doğrulamak sağlar. Parametrelerden biri geçersiz olan Oluşturucusu oluşturur bir `ArgumentException`.

```csharp
var myConnectionString = "{RelayConnectionString}";
// Declare the connectionStringBuilder so that it can be used outside of the loop if needed
RelayConnectionStringBuilder connectionStringBuilder;
try
{
    // Create the connectionStringBuilder using the supplied connection string
    connectionStringBuilder = new RelayConnectionStringBuilder(myConnectionString);
}
catch (ArgumentException ae)
{
    // Perform some error handling
}
```

## <a name="hybrid-connection-stream"></a>Karma bağlantı akışı
[HybridConnectionStream] [ HCStream] sınıftır çalıştığınız olup olmadığını veri göndermek ve bir Azure geçiş uç noktasından almak için kullanılan birincil nesne bir [HybridConnectionClient] [ HCClient], veya bir [HybridConnectionListener][HCListener].

### <a name="getting-a-hybrid-connection-stream"></a>Karma bağlantı akışı alma

#### <a name="listener"></a>Dinleyici
Kullanarak bir [HybridConnectionListener][HCListener], elde edebilirsiniz bir `HybridConnectionStream` gibi nesnesi:

```csharp
// Use the RelayConnectionStringBuilder to get a valid connection string
var listener = new HybridConnectionListener(csb.ToString());
// Open a connection to the Relay endpoint
await listener.OpenAsync();
// Get a `HybridConnectionStream`
var hybridConnectionStream = await listener.AcceptConnectionAsync();
```

#### <a name="client"></a>İstemci
Kullanarak bir [HybridConnectionClient][HCClient], elde edebilirsiniz bir `HybridConnectionStream` gibi nesnesi:

```csharp
// Use the RelayConnectionStringBuilder to get a valid connection string
var client = new HybridConnectionClient(csb.ToString());
// Open a connection to the Relay endpoint and get a `HybridConnectionStream`
var hybridConnectionStream = await client.CreateConnectionAsync();
```

### <a name="receiving-data"></a>Veri alma
[HybridConnectionStream] [ HCStream] sınıfı iki yönlü iletişim sağlar. Çoğu durumda, sürekli olarak akıştan alırsınız. Akıştan metin okuma, ayrıca kullanmak istediğiniz bir [StreamReader](https://msdn.microsoft.com/library/system.io.streamreader(v=vs.110).aspx) daha kolay verilerini ayrıştırma sağlayan nesne. Örneğin, metin olarak yerine olarak verileri okuyabilir `byte[]`.

İptal istendi kadar aşağıdaki kodu tek tek satırlık metin akıştan okur:

```csharp
// Create a CancellationToken, so that we can cancel the while loop
var cancellationToken = new CancellationToken();
// Create a StreamReader from the 'hybridConnectionStream`
var streamReader = new StreamReader(hybridConnectionStream);

while (!cancellationToken.IsCancellationRequested)
{
    // Read a line of input until a newline is encountered
    var line = await streamReader.ReadLineAsync();
    if (string.IsNullOrEmpty(line))
    {
        // If there's no input data, we will signal that 
        // we will no longer send data on this connection
        // and then break out of the processing loop.
        await hybridConnectionStream.ShutdownAsync(cancellationToken);
        break;
    }
}
```

### <a name="sending-data"></a>Verileri gönderme
Bir bağlantı kuruldu olduktan sonra geçiş uç noktasına bir ileti gönderebilir. Bağlantı nesnesi devralındığından [akış](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx), verilerinizi olarak gönderme bir `byte[]`. Aşağıdaki örnek, bunun nasıl yapılacağı gösterilmektedir:

```csharp
var data = Encoding.UTF8.GetBytes("hello");
await clientConnection.WriteAsync(data, 0, data.Length);
```

Metin her zaman dizesini kodlayın gerek kalmadan doğrudan göndermek istiyorsanız, ancak kayabilir `hybridConnectionStream` nesnesi ile bir [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) nesnesi.

```csharp
// The StreamWriter object only needs to be created once
var textWriter = new StreamWriter(hybridConnectionStream);
await textWriter.WriteLineAsync("hello");
```

## <a name="next-steps"></a>Sonraki adımlar
Azure geçişi hakkında daha fazla bilgi için bu bağlantıları ziyaret edin:

* [Microsoft.Azure.Relay başvurusu](/dotnet/api/microsoft.azure.relay)
* [Azure Geçiş nedir?](relay-what-is-it.md)
* [Kullanılabilir geçiş API'leri](relay-api-overview.md)

[RelayConnectionStringBuilder]: /dotnet/api/microsoft.azure.relay.relayconnectionstringbuilder
[HCStream]: /dotnet/api/microsoft.azure.relay.hybridconnectionstream
[HCClient]: /dotnet/api/microsoft.azure.relay.hybridconnectionclient
[HCListener]: /dotnet/api/microsoft.azure.relay.hybridconnectionlistener