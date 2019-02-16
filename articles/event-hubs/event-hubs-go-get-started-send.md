---
title: Git - Azure Event Hubs'ı kullanarak olayları gönderme | Microsoft Docs
description: Bu makalede, Azure Event Hubs'dan olayları gönderen bir Go uygulaması oluşturmak için bir kılavuz sağlar.
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: c0583f67d2351d05f877d0ebc1f29cea9e52c5eb
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56311996"
---
# <a name="send-events-to-event-hubs-using-go"></a>Git kullanarak Event Hubs için olayları gönderme

Azure Event Hubs saniyede milyonlarca olay alıp işleme kapasitesine sahip olan bir Büyük Veri akış platformu ve olay alma hizmetidir. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Olay Hub’larının ayrıntılı genel bakışı için bkz. [Olay Hub’larına genel bakış](event-hubs-about.md) ve [Olay Hub’ları özellikleri](event-hubs-features.md).

Bu öğreticide, bir seferde yazılmış bir uygulamadan bir olay hub'ına olayları göndermek açıklar. 

> [!NOTE]
> Bu hızlı başlangıcı [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/eventhubs)’dan örnek olarak indirebilir, `EventHubConnectionString` ve `EventHubName` dizelerini olay hub’ınızdaki değerlerle değiştirebilir ve çalıştırabilirsiniz. Alternatif olarak bu öğreticideki adımları izleyerek kendi çözümünüzü de oluşturabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

* Yerel olarak yüklü gidin. İzleyin [bu yönergeleri](https://golang.org/doc/install) gerekirse.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Event Hubs ad alanı ve bir olay hub’ı oluşturma
İlk adımda [Azure portalını](https://portal.azure.com) kullanarak Event Hubs türünde bir ad alanı oluşturun, ardından uygulamanızın olay hub’ı ile iletişim kurması için gereken yönetim kimlik bilgilerini edinin. Bir ad alanı ve olay hub'ı oluşturmak için verilen yordamı izleyin [bu makalede](event-hubs-create.md).

Şimdi, bu öğreticide aşağıdaki adımlarla devam edin.

## <a name="install-go-package"></a>Go paketini yükle

Go için Event Hubs ile paketi `go get` veya `dep`. Örneğin:

```bash
go get -u github.com/Azure/azure-event-hubs-go
go get -u github.com/Azure/azure-amqp-common-go/...

# or

dep ensure -add github.com/Azure/azure-event-hubs-go
dep ensure -add github.com/Azure/azure-amqp-common-go
```

## <a name="import-packages-in-your-code-file"></a>Kod dosyanızın içinde paketleri içeri aktarma

Git paketleri içeri aktarmak için aşağıdaki kod örneği kullanın:

```go
import (
    aad "github.com/Azure/azure-amqp-common-go/aad"
    eventhubs "github.com/Azure/azure-event-hubs-go"
)
```

## <a name="create-service-principal"></a>Hizmet sorumlusu oluşturma

Yönergeleri izleyerek yeni bir hizmet sorumlusu oluşturma [Azure, Azure CLI 2.0 ile hizmet sorumlusu oluşturma](/cli/azure/create-an-azure-service-principal-azure-cli). Sağlanan kimlik bilgilerini aşağıdaki adlara sahip ortamınızdaki kaydedin. Go için Azure SDK'sı hem de Event Hubs paketleri, bu değişken adlarını aramak için yapılandırılmış:

```bash
export AZURE_CLIENT_ID=
export AZURE_CLIENT_SECRET=
export AZURE_TENANT_ID=
export AZURE_SUBSCRIPTION_ID= 
```

Şimdi bu kimlik bilgilerini kullanır, Event Hubs istemcisi için bir yetkilendirme sağlayıcısı oluşturun:

```go
tokenProvider, err := aad.NewJWTProvider(aad.JWTProviderWithEnvironmentVars())
if err != nil {
    log.Fatalf("failed to configure AAD JWT provider: %s\n", err)
}
```

## <a name="create-event-hubs-client"></a>Event Hubs istemcisi oluşturma

Aşağıdaki kod, Event Hubs istemcisi oluşturur:

```go
hub, err := eventhubs.NewHub("namespaceName", "hubName", tokenProvider)
ctx := context.WithTimeout(context.Background(), 10 * time.Second)
defer hub.Close(ctx)
if err != nil {
    log.Fatalf("failed to get hub %s\n", err)
}
```

## <a name="send-messages"></a>İleti gönderme

Aşağıdaki kod parçacığında, etkileşimli bir terminalden göndermek (1) için ya da programınızdan iletileri gönderme (2) için kullanın:

```go
// 1. send messages at the terminal
ctx = context.Background()
reader := bufio.NewReader(os.Stdin)
for {
    fmt.Printf("Input a message to send: ")
    text, _ := reader.ReadString('\n')
    hub.Send(ctx, eventhubs.NewEventFromString(text))
}

// 2. send messages within program
ctx = context.Background()
hub.Send(ctx, eventhubs.NewEventFromString("hello Azure!"))
```

## <a name="extras"></a>Ek Özellikler

Bölüm kimlikleri, olay hub'ında alın:

```go
info, err := hub.GetRuntimeInformation(ctx)
if err != nil {
    log.Fatalf("failed to get runtime info: %s\n", err)
}
log.Printf("got partition IDs: %s\n", info.PartitionIDs)
```

Olay hub'ına olayları göndermek için uygulamayı çalıştırın. 

Tebrikler! Bir olay hub'ına ileti gönderdiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta Go kullanarak olay hub'ına ileti gönderdiniz. Git kullanarak bir olay hub'ından olay alma konusunda bilgi almak için bkz: [olayları event hub'dan - Git alma](event-hubs-go-get-started-receive-eph.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
