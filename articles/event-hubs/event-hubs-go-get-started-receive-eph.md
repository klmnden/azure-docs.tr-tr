---
title: Git - Azure Event Hubs kullanarak olay alma | Microsoft Docs
description: Bu makalede, Azure Event Hubs'tan gelen olayları alır bir Go uygulaması oluşturmak için bir kılavuz sağlar.
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: db952b82172928e42e951563d98bb32b275e8af7
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53085000"
---
# <a name="receive-events-from-event-hubs-using-go"></a>Git kullanarak Event Hubs'dan olayları alma

Azure Event Hubs, milyonlarca işleyebilen ileri düzeyde ölçeklenebilir bir olay yönetim sistemi saniyede olayları, işlemek ve çok büyük miktardaki verileri çözümlemek uygulamaları etkinleştirme bağlı cihazlarınız ve diğer sistemleri tarafından üretilen. Bir olay hub'ına toplandıktan sonra almak ve işlem içi işleyicileri kullanarak olayları işleme veya diğer analiz sistemleri ileterek.

Event Hubs hakkında daha fazla bilgi için bkz: [Event Hubs'a genel bakış][Event Hubs overview].

Bu öğretici, olayları bir event hub'ında bir Go uygulaması alacak şekilde açıklar. Olayları gönderme hakkında bilgi edinmek için [karşılık gelen gönderme makale](event-hubs-go-get-started-send.md).

Bu öğreticideki kod öğesinden alınır [bu GitHub örneklerine](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/eventhubs), hangi tam görmek için inceleyebilirsiniz uygulama da dahil olmak üzere çalışma deyimleri ve değişken bildirimleri alın.

Diğer örnekler kullanılabilir [olay hub'ları depo paketini](https://github.com/Azure/azure-event-hubs-go/tree/master/_examples).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşullar gerekir:

* Yerel olarak yüklü gidin. İzleyin [bu yönergeleri](https://golang.org/doc/install) gerekirse.
* Etkin bir Azure hesabı. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap][] oluşturun.
* İleti almak için olmalıdır iletileri hedef olay hub'ında. İletileri gönder öğrenin [gönderme Öğreticisi](event-hubs-go-get-started-send.md).
* Mevcut bir olay hub'ı (aşağıdaki bölüme bakın).
* Bir mevcut depolama hesabı ve kapsayıcı (sonraki bölümde sonra bölümüne bakın).

### <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

Bu öğreticide, mevcut bir Event Hubs ad alanı ve olay hub'ile başlar. Yönergeleri izleyerek bu varlıkları oluşturabilirsiniz [bu makalede](event-hubs-create.md).

### <a name="create-a-storage-account-and-container"></a>Bir depolama hesabı ve kapsayıcı oluşturma

Durum kiraları bölümleri ve kontrol noktaları gibi olay akışı paylaşılır kullanarak bir Azure depolama kapsayıcısı alıcılar arasında. Go SDK'sı ile bir depolama hesabı ve kapsayıcı oluşturabilirsiniz, ancak yönergeleri izleyerek bir tane de oluşturabilirsiniz [Azure depolama hesapları hakkında](../storage/common/storage-create-storage-account.md).

Yapıtları depolama ile Go SDK oluşturmak için örnekleri kullanılabilir [Git deposu örnekleri](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage) ve Bu öğretici için karşılık gelen örnek.

## <a name="receive-messages"></a>İleti alma

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

## <a name="import-packages-in-your-code-file"></a>Kod dosyanızın içinde paketleri içeri aktarma

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

## <a name="create-service-principal"></a>Hizmet sorumlusu oluşturma

Yönergeleri izleyerek yeni bir hizmet sorumlusu oluşturma [Azure, Azure CLI 2.0 ile hizmet sorumlusu oluşturma](/cli/azure/create-an-azure-service-principal-azure-cli). Sağlanan kimlik bilgilerini aşağıdaki adlara sahip ortamınızdaki Kaydet: hem Azure SDK'sı için Git ve Event Hubs paketini aramak için bu değişken adları için önceden yapılandırılmış.

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

## <a name="get-metadata-struct"></a>Yapı meta verilerini al

Bir yapı ile Azure Go SDK'sı kullanarak Azure ortamınızda hakkındaki meta verileri alın. Sonraki işlemlerin bu yapı doğru Uç noktalara bulmak için kullanın.

```go
azureEnv, err := azure.EnvironmentFromName("AzurePublicCloud")
if err != nil {
    log.Fatalf("could not get azure.Environment struct: %s\n", err)
}
```

## <a name="create-credential-helper"></a>Kimlik bilgisi Yardımcısını oluşturma 

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

## <a name="create-checkpointer-and-leaser"></a>Checkpointer ve Leaser oluşturma 

Oluşturma bir **Leaser**, belirli bir alıcı bir bölüm kiralama sorumlu ve **Checkpointer**, diğer alıcılar başlayabilmesi için bir ileti akışı denetim noktaları yazmak için sorumlu doğru uzaklığı okunuyor.

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

## <a name="construct-event-processor-host"></a>Olay işleyicisi ana bilgisayarı oluşturun

Artık bir EventProcessorHost şu şekilde oluşturmak için gerekli parça var. Aynı **StorageLeaserCheckpointer** daha önce açıklandığı gibi bir Leaser ve Checkpointer, olarak kullanılır:

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

## <a name="create-handler"></a>İşleyici oluşturun 

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

## <a name="receive-messages"></a>İleti alma

Her şeyi, olay işlemcisi konağı ile başlayabilirsiniz `Start(context)` kalıcı olarak çalışan veya ile tutmak `StartNonBlocking(context)` yalnızca iletileri kullanılabilir olduğu sürece çalıştırılacak.

Bu öğreticide başlar ve şu şekilde çalışır; bir örnek için GitHub örnek görmek `StartNonBlocking`:

```go
ctx := context.Background()
err = p.Start()
if err != nil {
    log.Fatalf("failed to start EPH: %s\n", err)
}
```

## <a name="notes"></a>Notlar

Bu öğretici, **EventProcessorHost**’un tek bir örneğini kullanır. Aktarım hızını ve güvenilirliğini artırmak için birden çok örneğini çalıştırmalısınız **EventProcessorHost** farklı sistemlerde. Bu yalnızca bir alıcı sistemi sağlar Leaser ilişkili olduğu ve belirli bir zamanda, belirtilen bir bölüm iletileri alır.

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, bir olay hub'ından iletiler alan bir Go uygulaması oluşturdunuz. Go kullanarak olay hub'ına olay gönderme hakkında bilgi edinmek için bkz: [olayları event hub'dan - Git gönderme](event-hubs-go-get-started-send.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[ücretsiz bir hesap]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
