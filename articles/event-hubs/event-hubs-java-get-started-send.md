---
title: Olayları Java kullanarak Azure Event Hubs'a gönderme | Microsoft Docs
description: Java kullanarak Event Hubs için göndermeye başlama
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 11/12/2018
ms.author: shvija
ms.openlocfilehash: 8fd0b1923bd1e89b16a1e33be908dc9ef73b398b
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52955403"
---
# <a name="send-events-to-azure-event-hubs-using-java"></a>Java kullanarak Azure Event Hubs için olayları gönderme

Azure Event Hubs saniyede milyonlarca olay alıp işleme kapasitesine sahip olan bir Büyük Veri akış platformu ve olay alma hizmetidir. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Olay Hub’larının ayrıntılı genel bakışı için bkz. [Olay Hub’larına genel bakış](event-hubs-about.md) ve [Olay Hub’ları özellikleri](event-hubs-features.md).

Bu öğreticide, Java dilinde yazılmış bir konsol uygulaması kullanarak olayları bir event hub'ına gönderme işlemi gösterilmektedir. 

> [!NOTE]
> Bu hızlı başlangıcı [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SimpleSend)’dan örnek olarak indirebilir, `EventHubConnectionString` ve `EventHubName` dizelerini olay hub’ınızdaki değerlerle değiştirebilir ve çalıştırabilirsiniz. Alternatif olarak bu öğreticideki adımları izleyerek kendi çözümünüzü de oluşturabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

* Java geliştirme ortamı. Bu öğreticide [Eclipse](https://www.eclipse.org/).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Event Hubs ad alanı ve bir olay hub’ı oluşturma
İlk adımda [Azure portalını](https://portal.azure.com) kullanarak Event Hubs türünde bir ad alanı oluşturun, ardından uygulamanızın olay hub’ı ile iletişim kurması için gereken yönetim kimlik bilgilerini edinin. Bir ad alanı ve olay hub'ı oluşturmak için verilen yordamı izleyin [bu makalede](event-hubs-create.md).

Olay hub'ı için erişim anahtarı değeri makalesindeki yönergeleri izleyerek almak: [bağlantı dizesi alma](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Bu öğreticinin ilerleyen bölümlerinde yazdığınız kod erişim anahtarını kullanın. Varsayılan anahtar adı: **RootManageSharedAccessKey**.

Şimdi, bu öğreticide aşağıdaki adımlarla devam edin.

## <a name="add-reference-to-azure-event-hubs-library"></a>Azure Event Hubs kitaplığına bir başvuru ekleyin

Event Hubs için Java istemci kitaplığı içindeki Maven projelerinde kullanılabilir [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22). Bu kitaplık içinde Maven proje dosyanızda aşağıdaki bağımlılık bildirimi kullanarak başvurabilirsiniz. Geçerli 1.0.2 sürümüdür:    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>1.0.2</version>
</dependency>
```

Derleme ortamlarının farklı türleri için en son JAR dosyalarının açıkça edinebilirsiniz [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).  

Basit olay yayımcısı, içeri aktarma *com.microsoft.azure.eventhubs* Event Hubs istemci sınıfları için paket ve *com.microsoft.azure.servicebus* için yardımcı sınıflar gibi paket Azure Service Bus Mesajlaşma istemcisi ile paylaşılan sık karşılaşılan özel durumlar. 

## <a name="write-code-to-send-messages-to-the-event-hub"></a>Olay hub'ına ileti göndermek için kod yazma

Aşağıdaki örnek için önce en sevdiğiniz Java geliştirme ortamında bir konsol/kabuk uygulaması için yeni bir Maven projesi oluşturun. Sınıf adını `SimpleSend`:     

```java
package com.microsoft.azure.eventhubs.samples.send;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.microsoft.azure.eventhubs.ConnectionStringBuilder;
import com.microsoft.azure.eventhubs.EventData;
import com.microsoft.azure.eventhubs.EventHubClient;
import com.microsoft.azure.eventhubs.EventHubException;

import java.io.IOException;
import java.nio.charset.Charset;
import java.time.Instant;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.Executors;
import java.util.concurrent.ExecutorService;

public class SimpleSend {

    public static void main(String[] args)
            throws EventHubException, ExecutionException, InterruptedException, IOException {
            
            
    }
 }
```

### <a name="construct-connection-string"></a>Bağlantı dizesi oluşturun

ConnectionStringBuilder sınıfı Event Hubs istemci örneğine geçirmek için bir bağlantı dizesi değerini oluşturmak için kullanın. Yer tutucuları, ad alanı ve olay hub'ı oluştururken aldığınız değerlerle değiştirin:

```java
final ConnectionStringBuilder connStr = new ConnectionStringBuilder()
        .setNamespaceName("Your Event Hubs namespace name")
        .setEventHubName("Your event hub")
        .setSasKeyName("Your policy name")
        .setSasKey("Your primary SAS key");
```

### <a name="send-events"></a>Olayları gönderme

Bir dizeyi UTF-8 kodlamasını bayt içine dönüştürerek tekil bir olay oluşturun. Ardından, bağlantı dizesinden yeni bir olay hub'ları istemci örneği oluşturun ve ileti gönder:   

```java 
String payload = "Message " + Integer.toString(i);
byte[] payloadBytes = gson.toJson(payload).getBytes(Charset.defaultCharset());
EventData sendEvent = EventData.create(payloadBytes);

final EventHubClient ehClient = EventHubClient.createSync(connStr.toString(), executorService);
ehClient.sendSync(sendEvent);
    
// close the client at the end of your program
ehClient.closeSync();

``` 

Derleme programı çalıştırın ve hiçbir hata olmadığından emin olun.

Tebrikler! Bir olay hub'ına ileti gönderdiniz.

### <a name="appendix-how-messages-are-routed-to-eventhub-partitions"></a>Ek: EventHub bölümleri nasıl iletiler yönlendirilir

İleti tüketiciler tarafından alınır önce bunlar bölümlere ilk yayımcılar tarafından yayımlanan gerekir. Olay hub'ına zaman uyumlu olarak com.microsoft.azure.eventhubs.EventHubClient nesnede sendSync() yöntemi kullanarak ileti yayımlandığında, ileti belirli bir bölüme gönderilebilecek veya hepsini bir kez deneme biçimde için kullanılabilir tüm bölümleri dağıtılmış Bölüm anahtarı veya belirtilen üzerinde bağlı olarak.

Bölüm anahtarını temsil eden bir dize belirtildiğinde, anahtarı için bir olay göndermek için hangi bölümünün belirlemek için karma.

Bölüm anahtarı olarak ayarlanmadığında, iletileri ardından gidiş-robined kullanılabilen tüm bölümler için olacaktır

```java
// Serialize the event into bytes
byte[] payloadBytes = gson.toJson(messagePayload).getBytes(Charset.defaultCharset());

// Use the bytes to construct an {@link EventData} object
EventData sendEvent = EventData.create(payloadBytes);

// Transmits the event to event hub without a partition key
// If a partition key is not set, then we will round-robin to all topic partitions
eventHubClient.sendSync(sendEvent);

//  the partitionKey will be hash'ed to determine the partitionId to send the eventData to.
eventHubClient.sendSync(sendEvent, partitionKey);

// close the client at the end of your program
eventHubClient.closeSync();

```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Java kullanarak bir olay hub'ına ileti gönderdiniz. Java kullanarak bir olay hub'ından olay alma konusunda bilgi almak için bkz: [olayları event hub'dan - Java alma](event-hubs-java-get-started-receive-eph.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio

