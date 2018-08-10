---
title: Olayları, Go kullanarak Azure Event Hubs'a gönderme | Microsoft Docs
description: Go kullanarak Event Hubs'a olay göndermeye başlama
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 07/23/2018
ms.author: shvija
ms.openlocfilehash: 40b3aa82c3e9e8ab9a30362c0a41998877655725
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005606"
---
# <a name="send-events-to-event-hubs-using-go"></a>Git kullanarak Event Hubs için olayları gönderme

Azure Event Hubs, milyonlarca işleyebilen ileri düzeyde ölçeklenebilir bir olay yönetim sistemi saniyede olayları, işlemek ve çok büyük miktardaki verileri çözümlemek uygulamaları etkinleştirme bağlı cihazlarınız ve diğer sistemleri tarafından üretilen. Bir olay hub'ına toplandıktan sonra almak ve işlem içi işleyicileri kullanarak olayları işleme veya diğer analiz sistemleri ileterek.

Event Hubs hakkında daha fazla bilgi için bkz: [Event Hubs'a genel bakış][Event Hubs overview].

Bu öğreticide, bir seferde yazılmış bir uygulamadan bir olay hub'ına olayları göndermek açıklar. Olayları almak için kullandığınız **Git eph** açıklandığı (olay işleyicisi ana bilgisayarı) paket [karşılık gelen alma makale](event-hubs-go-get-started-receive-eph.md).

Bu öğreticideki kod öğesinden alınır [bu GitHub örneklerine](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/eventhubs), hangi tam görmek için inceleyebilirsiniz içeri aktarma deyimlerini ve değişken bildirimleri dahil olmak üzere çalışan bir uygulama.

Diğer örnekler kullanılabilir [olay hub'ları depo paketini](https://github.com/Azure/azure-event-hubs-go/tree/master/_examples).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

* Yerel olarak yüklü gidin. İzleyin [bu yönergeleri](https://golang.org/doc/install) gerekirse.
* Etkin bir Azure hesabı. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap][] oluşturun.
* Mevcut bir Event Hubs ad alanı ve olay hub '. Yönergeleri izleyerek bu varlıkları oluşturabilirsiniz [bu makalede](event-hubs-create.md).

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
hub.Send(ctx, eventhubs.NewEventFromString("hello Azure!")
```

## <a name="extras"></a>Ek Özellikler

Bölüm kimlikleri, olay hub'ında alın:

```go
info, err := hub.GetRuntimeInformation(ctx)
if err != nil {
    log.Fatalf("failed to get runtime info: %s\n", err)
}
log.Printf("got partition IDs: %s\n, info.PartitionIDs)
```

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs hakkında daha fazla bilgi için şu sayfaları ziyaret edin:

* [EventProcessorHost kullanarak olay alma](event-hubs-go-get-started-receive-eph.md)
* [Event Hubs'a genel bakış][Event Hubs overview]
* [Olay Hub’ı oluşturma](event-hubs-create.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[ücretsiz bir hesap]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
