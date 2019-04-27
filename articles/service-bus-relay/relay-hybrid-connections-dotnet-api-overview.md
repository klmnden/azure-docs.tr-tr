---
title: Standart .NET API'ları Azure geçişine genel bakış | Microsoft Docs
description: Azure geçişi .NET standart API'sine genel bakış
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: b1da9ac1-811b-4df7-a22c-ccd013405c40
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: spelluru
ms.openlocfilehash: 78ad3ab49db162af060b4273deea717cd3472668
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60749028"
---
# <a name="azure-relay-hybrid-connections-net-standard-api-overview"></a>Azure geçiş karma bağlantılar .NET standart API'sine genel bakış

Bu makalede Azure geçiş karma bağlantılar .NET standart anahtar bazıları özetlenmektedir [istemci API'leri](/dotnet/api/microsoft.azure.relay).
  
## <a name="relay-connection-string-builder-class"></a>Geçiş bağlantı dizesi Oluşturucusu sınıfı

[RelayConnectionStringBuilder] [ RelayConnectionStringBuilder] sınıfı belirli geçişi karma bağlantıları için bağlantı dizelerini biçimlendirir. Bir bağlantı dizesinin biçimini doğrulayın veya sıfırdan bir bağlantı dizesi oluşturmak için kullanabilirsiniz. Aşağıdaki kod örneği için bkz:

```csharp
var endpoint = "[Relay namespace]";
var entityPath = "[Name of the Hybrid Connection]";
var sharedAccessKeyName = "[SAS key name]";
var sharedAccessKey = "[SAS key value]";

var connectionStringBuilder = new RelayConnectionStringBuilder()
{
    Endpoint = endpoint,
    EntityPath = entityPath,
    SharedAccessKeyName = sasKeyName,
    SharedAccessKey = sasKeyValue
};
```

Doğrudan bir bağlantı dizesi geçirebilirsiniz `RelayConnectionStringBuilder` yöntemi. Bu işlem, bağlantı dizesi geçerli bir biçimde olduğunu doğrulamak sağlar. Herhangi bir parametre geçersiz, oluşturucu oluşturur. bir `ArgumentException`.

```csharp
var myConnectionString = "[RelayConnectionString]";
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

## <a name="hybrid-connection-stream"></a>Karma bağlantı akış

[HybridConnectionStream] [ HCStream] ile çalışırken, bir Azure geçişi uç noktasından veri alıp göndermek için kullanılan birincil nesne sınıfı, bir [HybridConnectionClient] [ HCClient], veya bir [HybridConnectionListener][HCListener].

### <a name="getting-a-hybrid-connection-stream"></a>Karma bağlantı akış alma

#### <a name="listener"></a>Dinleyici

Kullanarak bir [HybridConnectionListener] [ HCListener] nesnesi elde edebilirsiniz bir `HybridConnectionStream` gibi nesnesi:

```csharp
// Use the RelayConnectionStringBuilder to get a valid connection string
var listener = new HybridConnectionListener(csb.ToString());
// Open a connection to the Relay endpoint
await listener.OpenAsync();
// Get a `HybridConnectionStream`
var hybridConnectionStream = await listener.AcceptConnectionAsync();
```

#### <a name="client"></a>İstemci

Kullanarak bir [HybridConnectionClient] [ HCClient] nesnesi elde edebilirsiniz bir `HybridConnectionStream` gibi nesnesi:

```csharp
// Use the RelayConnectionStringBuilder to get a valid connection string
var client = new HybridConnectionClient(csb.ToString());
// Open a connection to the Relay endpoint and get a `HybridConnectionStream`
var hybridConnectionStream = await client.CreateConnectionAsync();
```

### <a name="receiving-data"></a>Veri alma

[HybridConnectionStream] [ HCStream] sınıfı iki yönlü iletişim sağlar. Çoğu durumda, sürekli olarak akıştan alırsınız. Metin akışından okuma yapıyorsanız de kullanmak isteyebileceğiniz bir [StreamReader](https://msdn.microsoft.com/library/system.io.streamreader(v=vs.110).aspx) verilerini daha kolay ayrıştırma sağlayan nesne. Örneğin, metin yerine olarak verileri okuyabilir `byte[]`.

Aşağıdaki kod, bir iptal isteğinde kadar tek tek satırlık metin akıştan okur:

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

### <a name="sending-data"></a>Veri gönderme

Bir bağlantı kuruldu aldıktan sonra geçiş uç noktaya bir ileti gönderebilir. Bağlantı nesnesi e devralındığından [Stream](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx), verilerinizi olarak göndermek bir `byte[]`. Aşağıdaki örnek bunun nasıl yapılacağı gösterilmektedir:

```csharp
var data = Encoding.UTF8.GetBytes("hello");
await clientConnection.WriteAsync(data, 0, data.Length);
```

Metin dizesi her zaman kodlamak zorunda kalmadan doğrudan göndermeyi istiyorsanız, ancak kayabilir `hybridConnectionStream` nesnesi ile bir [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) nesne.

```csharp
// The StreamWriter object only needs to be created once
var textWriter = new StreamWriter(hybridConnectionStream);
await textWriter.WriteLineAsync("hello");
```

## <a name="next-steps"></a>Sonraki adımlar

Azure geçişi hakkında daha fazla bilgi edinmek için şu bağlantıları ziyaret edin:

* [Microsoft.Azure.Relay başvurusu](/dotnet/api/microsoft.azure.relay)
* [Azure Geçiş nedir?](relay-what-is-it.md)
* [Kullanılabilir geçiş API'leri](relay-api-overview.md)

[RelayConnectionStringBuilder]: /dotnet/api/microsoft.azure.relay.relayconnectionstringbuilder
[HCStream]: /dotnet/api/microsoft.azure.relay.hybridconnectionstream
[HCClient]: /dotnet/api/microsoft.azure.relay.hybridconnectionclient
[HCListener]: /dotnet/api/microsoft.azure.relay.hybridconnectionlistener