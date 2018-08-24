---
title: Olayları Java kullanarak Azure Event Hubs'a gönderme | Microsoft Docs
description: Java kullanarak Event Hubs için göndermeye başlama
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 08/20/2018
ms.author: shvija
ms.openlocfilehash: 2120fedc83b1dcad5462f4d5fb5118d19c3ce91c
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2018
ms.locfileid: "42747036"
---
# <a name="send-events-to-azure-event-hubs-using-java"></a>Java kullanarak Azure Event Hubs için olayları gönderme

Event Hubs, saniye başına milyonlarca olayı içe almak, işlemek ve çok büyük miktarda veriyi uygulamanızın bağlı cihazlarınız ve uygulamalarınız tarafından üretilen uygulama etkinleştirme bir yüksek düzeyde ölçeklenebilir bir alım sistemidir. Bir olay hub'ına toplandıktan sonra dönüştürme ve herhangi bir gerçek zamanlı analiz sağlayıcısı veya depolama kümesi kullanarak verileri depolama.

Daha fazla bilgi için [Event Hubs'a genel bakış][Event Hubs overview].

Bu öğreticide, bir Java konsol uygulaması kullanarak olayları bir event hub'ına gönderme işlemi gösterilmektedir. Java olay işleyicisi konağı kitaplığı kullanarak olayları almak için bkz. [bu makalede](event-hubs-java-get-started-receive-eph.md), veya soldaki içindekiler bölümünden uygun alma diline tıklayın.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

* Java geliştirme ortamı. Bu öğreticide [Eclipse](https://www.eclipse.org/).
* Etkin bir Azure hesabı. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap][] oluşturun.

Bu öğreticideki kod dayanır [SimpleSend GitHub örnek](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SimpleSend), hangi tam görmek için inceleyebilirsiniz çalışan uygulama.

## <a name="send-events-to-event-hubs"></a>Event Hubs’a olaylar gönderme

Event Hubs için Java istemci kitaplığı içindeki Maven projelerinde kullanılabilir [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22). Bu kitaplık içinde Maven proje dosyanızda aşağıdaki bağımlılık bildirimi kullanarak başvurabilirsiniz. Geçerli sürüm 1.0.1.:    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>1.0.1</version>
</dependency>
```

Derleme ortamlarının farklı türleri için en son JAR dosyalarının açıkça edinebilirsiniz [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).  

Basit olay yayımcısı, içeri aktarma *com.microsoft.azure.eventhubs* Event Hubs istemci sınıfları için paket ve *com.microsoft.azure.servicebus* için yardımcı sınıflar gibi paket Azure Service Bus Mesajlaşma istemcisi ile paylaşılan sık karşılaşılan özel durumlar. 

### <a name="declare-the-send-class"></a>Gönderme sınıf bildirme

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

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [EventProcessorHost kullanarak olay alma](event-hubs-java-get-started-receive-eph.md)
* [Event Hubs'a genel bakış][Event Hubs overview]
* [Olay Hub’ı oluşturma](event-hubs-create.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md
[ücretsiz bir hesap]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio

