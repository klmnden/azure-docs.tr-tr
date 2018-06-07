---
title: Java kullanarak Azure Event Hubs için olayları göndermek | Microsoft Docs
description: Java kullanarak Event Hubs'a gönderme kullanmaya başlama
services: event-hubs
author: sethmanheim
manager: timlt
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 05/30/2018
ms.author: sethm
ms.openlocfilehash: 6d3bf0b8ac5c5bdc7bf3deda21e800fe3cc6be2e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34626420"
---
# <a name="send-events-to-azure-event-hubs-using-java"></a>Java kullanarak Azure Event Hubs için olayları Gönder

Olay hub'ları saniye başına milyonlarca olayı alma, bir uygulamaya etkinleştirme işlemek ve veri bağlı cihazlarınız ve uygulamalarınız tarafından üretilen oldukça büyük miktardaki analiz bir yüksek düzeyde ölçeklenebilir bir alım sistemine olur. Bir olay hub'ına toplandığında, dönüştürme ve herhangi bir gerçek zamanlı analiz sağlayıcısı veya depolama kümesi kullanarak verileri depolar.

Daha fazla bilgi için bkz: [Event Hubs'a genel bakış][Event Hubs overview].

Bu öğretici bir Java konsol uygulaması kullanarak bir event hub'ına olayları göndermek nasıl gösterir. Java olay işleyicisi konağı kitaplığını kullanarak olayları almak için bkz: [bu makalede](event-hubs-java-get-started-receive-eph.md), veya sol İçindekiler uygun alıcı dilde'ı tıklatın.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşullar gerekir:

* Java geliştirme ortamı. Bu öğretici kullanır [Eclipse](https://www.eclipse.org/).
* Etkin bir Azure hesabı. Bir Azure aboneliğiniz yoksa oluşturma bir [Ücretsiz hesap][] başlamadan önce.

Bu öğreticideki kod dayanır [SimpleSend GitHub örnek](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SimpleSend), hangi tam görmek için inceleyebilirsiniz çalışan bir uygulama.

## <a name="send-events-to-event-hubs"></a>Olayları Event Hubs'a gönderme

Olay hub'ları için Java istemci kitaplığı Maven projelerden kullanmak için kullanılabilir [Maven merkezi bir depoya](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22). Maven projesi dosyanızı içinde aşağıdaki bağımlılık bildirimi kullanarak bu kitaplık başvurusu yapabilir. Geçerli sürümü 1.0.0 şöyledir:    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>1.0.0</version>
</dependency>
```

Derleme ortamları farklı türleri için en son yayımlanan JAR dosyalarını açıkça edinebilirsiniz [Maven merkezi bir depoya](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).  

Basit olay yayımcısı alma *com.microsoft.azure.eventhubs* olay hub'ları istemci sınıfları için paketi ve *com.microsoft.azure.servicebus* Azure Service Bus Mesajlaşma istemcisiyle paylaşılan ortak özel durumlar gibi yardımcı sınıflar için paketi. 

### <a name="declare-the-send-class"></a>Gönderme sınıf bildirme

Aşağıdaki örnek için önce en sevdiğiniz Java geliştirme ortamında bir konsol/kabuk uygulaması için yeni bir Maven projesi oluşturun. Sınıf adını `Send`:     

```java
package com.microsoft.azure.eventhubs.samples.send;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.microsoft.azure.eventhubs.ConnectionStringBuilder;
import com.microsoft.azure.eventhubs.EventData;
import com.microsoft.azure.eventhubs.EventHubClient;
import com.microsoft.azure.eventhubs.PartitionSender;
import com.microsoft.azure.eventhubs.EventHubException;

import java.io.IOException;
import java.nio.charset.Charset;
import java.time.Instant;
import java.util.Random;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.Executors;
import java.util.concurrent.ExecutorService;

public class Send {

    public static void main(String[] args)
            throws EventHubException, ExecutionException, InterruptedException, IOException {
```

### <a name="construct-connection-string"></a>Bağlantı dizesi oluşturun

ConnectionStringBuilder sınıfı Event Hubs istemcisi örneğine geçirmek için bir bağlantı dizesi değeri oluşturmak için kullanın. Yer tutucuları ad alanı ve olay hub'ı oluşturduğunuzda elde ettiğiniz değerlerle değiştirin:

```java
   final ConnectionStringBuilder connStr = new ConnectionStringBuilder()
      .setNamespaceName("----NamespaceName-----")
      .setEventHubName("----EventHubName-----")
      .setSasKeyName("-----SharedAccessSignatureKeyName-----")
      .setSasKey("---SharedAccessSignatureKey----");
```

### <a name="send-events"></a>Olayları gönderme

Bir dize, UTF-8 bayt kodlamaya dönüştürerek tekil bir olay oluşturun. Ardından, yeni bir olay hub'ları istemci örnek bağlantı dizesinden oluşturun ve ileti gönderin:   

```java 
byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
EventData sendEvent = new EventData(payloadBytes);

final EventHubClient ehClient = EventHubClient.createSync(connStr.toString(), executorService);
ehClient.sendSync(sendEvent);
    
// close the client at the end of your program
ehClient.closeSync();

``` 

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [EventProcessorHost kullanarak olayları alma](event-hubs-java-get-started-receive-eph.md)
* [Event Hubs'a genel bakış][Event Hubs overview]
* [Olay Hub’ı oluşturma](event-hubs-create.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md
[Ücretsiz hesap]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio

