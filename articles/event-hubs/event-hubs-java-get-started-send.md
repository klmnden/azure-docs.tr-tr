---
title: "Java kullanarak Azure Event Hubs için olayları göndermek | Microsoft Docs"
description: "Java kullanarak Event Hubs'a gönderme kullanmaya başlama"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: core
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: 28585a1b6a6c7c642c8bccf4615382d15f89d11c
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="send-events-to-azure-event-hubs-using-java"></a>Java kullanarak Azure Event Hubs için olayları Gönder

## <a name="introduction"></a>Giriş
Olay hub'ları saniye başına milyonlarca olayı alma, bir uygulamaya etkinleştirme işlemek ve veri bağlı cihazlarınız ve uygulamalarınız tarafından üretilen oldukça büyük miktardaki analiz bir yüksek düzeyde ölçeklenebilir bir alım sistemine olur. Bir olay hub'ına toplandığında, dönüştürme ve herhangi bir gerçek zamanlı analiz sağlayıcısı veya depolama kümesi kullanarak verileri depolar.

Daha fazla bilgi için bkz: [Event Hubs'a genel bakış][Event Hubs overview].

Bu öğretici bir Java konsol uygulaması kullanarak bir event hub'ına olayları göndermek nasıl gösterir. Java olay işleyicisi konağı kitaplığını kullanarak olayları almak için bkz: [bu makalede](event-hubs-java-get-started-receive-eph.md), veya sol İçindekiler uygun alıcı dilde'ı tıklatın.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Java geliştirme ortamı. Bu öğretici için varsayıyoruz [Eclipse](https://www.eclipse.org/).
* Etkin bir Azure hesabı. <br/>Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure Ücretsiz Deneme Sürümü</a>.

## <a name="send-messages-to-event-hubs"></a>Event Hubs’a ileti gönderme
Olay hub'ları için Java istemci kitaplığı Maven projelerden kullanmak için kullanılabilir [Maven merkezi bir depoya](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22). Maven projesi dosyanızı içinde aşağıdaki bağımlılık bildirimi kullanarak bu kitaplık başvurusu yapabilir:    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```

Derleme ortamları farklı türleri için en son yayımlanan JAR dosyalarını açıkça edinebilirsiniz [Maven merkezi bir depoya](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).  

Basit olay yayımcısı alma *com.microsoft.azure.eventhubs* olay hub'ları istemci sınıfları için paketi ve *com.microsoft.azure.servicebus* Azure Service Bus Mesajlaşma istemcisiyle paylaşılan ortak özel durumlar gibi yardımcı sınıflar için paketi. 

Aşağıdaki örnek için önce en sevdiğiniz Java geliştirme ortamında bir konsol/kabuk uygulaması için yeni bir Maven projesi oluşturun. Sınıf adını `Send`.     

```java
import java.io.IOException;
import java.nio.charset.*;
import java.util.*;
import java.util.concurrent.ExecutionException;

import com.microsoft.azure.eventhubs.*;
import com.microsoft.azure.servicebus.*;

public class Send
{
    public static void main(String[] args) 
            throws ServiceBusException, ExecutionException, InterruptedException, IOException
    {
```

Ad alanı ve olay hub'ı adları olay hub'ı oluşturduğunuzda kullanılan değerlerle değiştirin.

```java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

Ardından, bir dize, UTF-8 bayt kodlamaya dönüştürerek tekil bir olay oluşturun. Ardından, yeni bir olay hub'ları istemci örnek bağlantı dizesinden oluşturun ve ileti gönderin.   

```java 

    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);

    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    
    // close the client at the end of your program
    ehClient.closeSync();
    }
}

``` 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [EventProcessorHost kullanarak olayları alma](event-hubs-java-get-started-receive-eph.md)
* [Event Hubs'a genel bakış][Event Hubs overview]
* [Olay Hub’ı oluşturma](event-hubs-create.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md
