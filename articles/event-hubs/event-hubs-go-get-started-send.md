---
title: Gönder ve Git - Azure Event Hubs kullanarak olay alma | Microsoft Docs
description: Bu makalede, Azure Event Hubs'dan olayları gönderen bir Go uygulaması oluşturmak için bir kılavuz sağlar.
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 04/15/2019
ms.author: shvija
ms.openlocfilehash: 823ebc985c77785f8b48d12d5919dbbd1b2b1459
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60821683"
---
# <a name="send-events-to-or-receive-events-from-event-hubs-using-go"></a>Olayları gönderme veya Git kullanarak Event Hubs'dan olayları alma
Azure Event Hubs saniyede milyonlarca olay alıp işleme kapasitesine sahip olan bir Büyük Veri akış platformu ve olay alma hizmetidir. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Olay Hub’larının ayrıntılı genel bakışı için bkz. [Olay Hub’larına genel bakış](event-hubs-about.md) ve [Olay Hub’ları özellikleri](event-hubs-features.md).

Bu öğretici, olayları bir event hub'ından gönderemeyecek ya olaylara Git uygulamaları yazmak açıklar. 

> [!NOTE]
> Bu hızlı başlangıcı [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/eventhubs)’dan örnek olarak indirebilir, `EventHubConnectionString` ve `EventHubName` dizelerini olay hub’ınızdaki değerlerle değiştirebilir ve çalıştırabilirsiniz. Alternatif olarak bu öğreticideki adımları izleyerek kendi çözümünüzü de oluşturabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

- Yerel olarak yüklü gidin. İzleyin [bu yönergeleri](https://golang.org/doc/install) gerekirse.
- Etkin bir Azure hesabı. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap][] oluşturun.
- **Bir Event Hubs ad alanı ve bir olay hub'ı oluşturma**. Kullanım [Azure portalında](https://portal.azure.com) Event Hubs türünde bir ad alanı oluşturma, ardından uygulamanızın olay hub'ı ile iletişim kurmak için gereken yönetim kimlik bilgilerini edinin. Bir ad alanı ve olay hub'ı oluşturmak için verilen yordamı izleyin [bu makalede](event-hubs-create.md).

## <a name="send-events"></a>Olayları gönderme
Bu bölümde, olayları olay hub'ına göndermek için bir Go uygulaması oluşturma işlemini gösterir. 

### <a name="install-go-package"></a>Go paketini yükle

Go için Event Hubs ile paketi `go get` veya `dep`. Örneğin:

```bash
go get -u github.com/Azure/azure-event-hubs-go
go get -u github.com/Azure/azure-amqp-common-go/...

# or

dep ensure -add github.com/Azure/azure-event-hubs-go
dep ensure -add github.com/Azure/azure-amqp-common-go
```

### <a name="import-packages-in-your-code-file"></a>Kod dosyanızın içinde paketleri içeri aktarma

Git paketleri içeri aktarmak için aşağıdaki kod örneği kullanın:

```go
import (
    aad "github.com/Azure/azure-amqp-common-go/aad"
    eventhubs "github.com/Azure/azure-event-hubs-go"
)
```

### <a name="create-service-principal"></a>Hizmet sorumlusu oluşturma

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

### <a name="create-event-hubs-client"></a>Event Hubs istemcisi oluşturma

Aşağıdaki kod, Event Hubs istemcisi oluşturur:

```go
hub, err := eventhubs.NewHub("namespaceName", "hubName", tokenProvider)
ctx := context.WithTimeout(context.Background(), 10 * time.Second)
defer hub.Close(ctx)
if err != nil {
    log.Fatalf("failed to get hub %s\n", err)
}
```

### <a name="write-code-to-send-messages"></a>İleti göndermek için kod yazma

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

### <a name="extras"></a>Ek Özellikler

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

## <a name="receive-events"></a>Olayları alma

### <a name="create-a-storage-account-and-container"></a>Bir depolama hesabı ve kapsayıcı oluşturma

Durum kiraları bölümleri ve kontrol noktaları gibi olay akışı paylaşılır kullanarak bir Azure depolama kapsayıcısı alıcılar arasında. Go SDK'sı ile bir depolama hesabı ve kapsayıcı oluşturabilirsiniz, ancak yönergeleri izleyerek bir tane de oluşturabilirsiniz [Azure depolama hesapları hakkında](../storage/common/storage-create-storage-account.md).

Yapıtları depolama ile Go SDK oluşturmak için örnekleri kullanılabilir [Git deposu örnekleri](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage) ve Bu öğretici için karşılık gelen örnek.

### <a name="go-packages"></a>Paketleri gidin

İletileri almak için Event hubs ile Git paketleri al `go get` veya `dep`:

```bash
go get -u github.com/Azure/azure-event-hubs-go/...
go get -u github.com/Azure/azure-amqp-common-go/...
go get -u github.com/Azure/go-autorest/...

# or

dep ensure -add github.com/Azure/azure-event-hubs-go
dep ensure -add github.com/Azure/azure-amqp-common-go
dep ensure -add github.com/Azure/go-autorest
```

### <a name="import-packages-in-your-code-file"></a>Kod dosyanızın içinde paketleri içeri aktarma

Git paketleri içeri aktarmak için aşağıdaki kod örneği kullanın:

```go
import (
    aad "github.com/Azure/azure-amqp-common-go/aad"
    eventhubs "github.com/Azure/azure-event-hubs-go"
    eph "github.com/Azure/azure-event-hubs-go/eph"
    storageLeaser "github.com/Azure/azure-event-hubs-go/storage"
    azure "github.com/Azure/go-autorest/autorest/azure"
)
```

### <a name="create-service-principal"></a>Hizmet sorumlusu oluşturma

Yönergeleri izleyerek yeni bir hizmet sorumlusu oluşturma [Azure, Azure CLI 2.0 ile hizmet sorumlusu oluşturma](/cli/azure/create-an-azure-service-principal-azure-cli). Sağlanan kimlik bilgilerini aşağıdaki adlara sahip ortamınızdaki kaydedin: Hem Azure SDK'sı için Git ve Event Hubs paketini önceden yapılandırılmış bu değişken adları için aranacak.

```bash
export AZURE_CLIENT_ID=
export AZURE_CLIENT_SECRET=
export AZURE_TENANT_ID=
export AZURE_SUBSCRIPTION_ID= 
```

Ardından, bu kimlik bilgilerini kullanır, Event Hubs istemcisi için bir yetkilendirme sağlayıcısı oluşturun:

```go
tokenProvider, err := aad.NewJWTProvider(aad.JWTProviderWithEnvironmentVars())
if err != nil {
    log.Fatalf("failed to configure AAD JWT provider: %s\n", err)
}
```

### <a name="get-metadata-struct"></a>Yapı meta verilerini al

Bir yapı ile Azure Go SDK'sı kullanarak Azure ortamınızda hakkındaki meta verileri alın. Sonraki işlemlerin bu yapı doğru Uç noktalara bulmak için kullanın.

```go
azureEnv, err := azure.EnvironmentFromName("AzurePublicCloud")
if err != nil {
    log.Fatalf("could not get azure.Environment struct: %s\n", err)
}
```

### <a name="create-credential-helper"></a>Kimlik bilgisi Yardımcısını oluşturma 

Depolama için bir paylaşılan erişim imzası (SAS) kimlik bilgisi oluşturmak için önceki Azure Active Directory (AAD) kimlik bilgilerini kullanan bir kimlik bilgisi Yardımcısını oluşturun. Son parametre olarak daha önce kullanılan aynı ortam değişkenlerini kullanmak için bu oluşturucu bildirir:

```go
cred, err := storageLeaser.NewAADSASCredential(
    subscriptionID,
    resourceGroupName,
    storageAccountName,
    storageContainerName,
    storageLeaser.AADSASCredentialWithEnvironmentVars())
if err != nil {
    log.Fatalf("could not prepare a storage credential: %s\n", err)
}
```

### <a name="create-a-check-pointer-and-a-leaser"></a>Bir onay işaretçi ve bir leaser oluşturma 

Oluşturma bir **leaser**, belirli bir alıcı bir bölüm kiralama sorumlu ve **işaretçi denetleyin**, diğer alıcılar başlayabilmesi için bir ileti akışı denetim noktaları yazmak için sorumlu doğru uzaklığı okunuyor.

Şu anda bir tek **StorageLeaserCheckpointer** olan kullanılabilir, aynı depolama kapsayıcısını hem kiraları ve kontrol noktaları yönetmek için kullanır. Depolama hesabı ve kapsayıcı adları yanı sıra **StorageLeaserCheckpointer** önceki adımı ve doğru şekilde kapsayıcısına erişmek için Azure ortamı struct oluşturulan kimlik bilgileri gerekiyor.

```go
leaserCheckpointer, err := storageLeaser.NewStorageLeaserCheckpointer(
    cred,
    storageAccountName,
    storageContainerName,
    azureEnv)
if err != nil {
    log.Fatalf("could not prepare a storage leaserCheckpointer: %s\n", err)
}
```

### <a name="construct-event-processor-host"></a>Olay işleyicisi ana bilgisayarı oluşturun

Artık bir EventProcessorHost şu şekilde oluşturmak için gerekli parça var. Aynı **StorageLeaserCheckpointer** daha önce açıklandığı gibi bir leaser ve onay işaretçi olarak kullanılır:

```go
ctx := context.Background()
p, err := eph.New(
    ctx,
    nsName,
    hubName,
    tokenProvider,
    leaserCheckpointer,
    leaserCheckpointer)
if err != nil {
    log.Fatalf("failed to create EPH: %s\n", err)
}
defer p.Close(context.Background())
```

### <a name="create-handler"></a>İşleyici oluşturun 

Artık bir işleyici oluşturun ve Event Processor Host ile kaydedin. Ana bilgisayar başlatıldığında, bu ve diğer belirtilen işleyicileri gelen iletiler için geçerlidir:

```go
handler := func(ctx context.Context, event *eventhubs.Event) error {
    fmt.Printf("received: %s\n", string(event.Data))
    return nil
}

// register the handler with the EPH
_, err := p.RegisterHandler(ctx, handler)
if err != nil {
    log.Fatalf("failed to register handler: %s\n", err)
}
```

### <a name="write-code-to-receive-messages"></a>İleti almak için kod yazma

Her şeyi, olay işlemcisi konağı ile başlayabilirsiniz `Start(context)` kalıcı olarak çalışan veya ile tutmak `StartNonBlocking(context)` yalnızca iletileri kullanılabilir olduğu sürece çalıştırılacak.

Bu öğreticide başlar ve şu şekilde çalışır; bir örnek için GitHub örnek görmek `StartNonBlocking`:

```go
ctx := context.Background()
err = p.Start()
if err != nil {
    log.Fatalf("failed to start EPH: %s\n", err)
}
```

## <a name="next-steps"></a>Sonraki adımlar
Bu makaleleri okuyun:

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Özellikler ve Azure Event Hubs terminolojisinde](event-hubs-features.md)
- [Event Hubs ile ilgili SSS](event-hubs-faq.md)


<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[ücretsiz bir hesap]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
