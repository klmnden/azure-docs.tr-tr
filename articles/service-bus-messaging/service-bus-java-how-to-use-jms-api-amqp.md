---
title: AMQP 1.0 ile Java JMS Service Bus API'sini kullanma | Microsoft Docs
description: Advanced Message Queuing Protocol (AMQP) 1.0 ile Azure Service Bus ile Java mesaj hizmeti (JMS) kullanma
services: service-bus-messaging
documentationcenter: java
author: axisc
manager: timlt
editor: spelluru
ms.assetid: be766f42-6fd1-410c-b275-8c400c811519
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 03/05/2019
ms.author: aschhab
ms.openlocfilehash: a7e4282a176794fe885049173ba56ce2461cd6fa
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58885562"
---
# <a name="how-to-use-the-java-message-service-jms-api-with-service-bus-and-amqp-10"></a>Hizmet veri yolu AMQP 1.0 ile Java mesaj hizmeti (JMS) API kullanma
Advanced Message Queuing Protocol (AMQP) 1.0 sağlam, platformlar arası Mesajlaşma uygulamaları oluşturmak için kullanabileceğiniz bir verimli, güvenilir, hat düzeyinde bir Mesajlaşma protokolüdür.

Destek için hizmet veri yolu AMQP 1.0 sıraya alma kullanın ve yayımlama/aracılı Mesajlaşma özelliklerinin verimli bir ikili protokolü kullanılarak platformlar aralığından abonelik anlamına gelir. Ayrıca, dillerin, çerçevelerin ve işletim sistemlerinin bir karışımını kullanılarak oluşturulan bileşenlerden oluşan bir uygulama oluşturabilirsiniz.

Bu makalede, Java uygulamalarından popüler Java mesaj hizmeti (JMS) API standart kullanarak Service Bus Mesajlaşma özelliklerinin (sıralar ve yayımlama/abone olma konuları) kullanmayı açıklar. Var olan bir [Yardımcısı makale](service-bus-amqp-dotnet.md) , hizmet veri yolu .NET API kullanarak aynı şeyi nasıl açıklar. AMQP 1.0 kullanarak platformlar arası Mesajlaşma hakkında bilgi edinmek için bu iki Kılavuzlar birlikte kullanabilirsiniz.

## <a name="get-started-with-service-bus"></a>Service Bus’ı kullanmaya başlama
Bu kılavuz, adlandırılan bir kuyruğun içeren bir Service Bus ad alanı zaten sahip olduğunuzu varsayar **basicqueue**. Bunu yapmanız durumunda yapabilecekleriniz [ad alanı ve Kuyruk oluşturma](service-bus-create-namespace-portal.md) kullanarak [Azure portalında](https://portal.azure.com). Service Bus ad alanları ve Kuyruk oluşturma hakkında daha fazla bilgi için bkz. [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md).

> [!NOTE]
> Bölümlenmiş kuyruklar ve konular da AMQP destekler. Daha fazla bilgi için [bölümlenmiş Mesajlaşma varlıkları](service-bus-partitioning.md) ve [hizmet veri yolu AMQP 1.0 desteği bölümlenmiş kuyruklar ve konular](service-bus-partitioned-queues-and-topics-amqp-overview.md).
> 
> 

## <a name="downloading-the-amqp-10-jms-client-library"></a>AMQP 1.0 JMS istemci Kitaplığı'nı indirme
Apache Qpid JMS AMQP 1.0 istemci Kitaplığı'nın en son sürümü karşıdan yükleme konumu hakkında daha fazla bilgi için ziyaret [ https://qpid.apache.org/download.html ](https://qpid.apache.org/download.html).

Aşağıdaki dört JAR dosyaları Apache Qpid JMS AMQP 1.0 dağıtım arşivden için Java sınıf yolu oluşturma ve Service Bus ile JMS uygulamaları çalıştırırken eklemelisiniz:

* geronimo jms\_1.1\_1.0.jar belirtimi
* qpid-jms - client-[sürüm] .jar

> [!NOTE]
> JMS JAR adlar ve sürümler değişmiş olabilir. Ayrıntılar için bkz [Qpid JMS - AMQP 1.0](https://qpid.apache.org/maven.html#qpid-jms-amqp-10).

## <a name="coding-java-applications"></a>Java uygulamalarını kodlama
### <a name="java-naming-and-directory-interface-jndi"></a>Java adlandırma ve dizini arabirimi (JNDI)
JMS mantıksal ve fiziksel adlarını arasında bir ayrım oluşturmak için Java adlandırma ve dizin arabirimi (JNDI) kullanır. İki tür JMS nesnelerin JNDI kullanarak çözümlenir: ConnectionFactory ve hedef. JNDI farklı dizin hizmetleri, ad çözümlemesi görevlerini işlemek için takılabilir bir sağlayıcı modeli kullanır. Dosya tabanlı JNDI aşağıdaki özellikler dosyası kullanılarak yapılandırılan sağlayıcısı kitaplığı basit bir özellik ile gelir Apache Qpid JMS AMQP 1.0 biçimlendirin:

```
# servicebus.properties - sample JNDI configuration

# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCF = amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net

# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
queue.QUEUE = queue1
```

#### <a name="setup-jndi-context-and-configure-the-connectionfactory"></a>JNDI bağlam kurulumu ve ConnectionFactory yapılandırma

**ConnectionString** 'paylaşılan erişim ilkeleri' kullanılabilir bir başvuru [Azure portalı](https://portal.azure.com) altında **birincil bağlantı dizesi**
```java
// The connection string builder is the only part of the azure-servicebus SDK library
// we use in this JMS sample and for the purpose of robustly parsing the Service Bus 
// connection string. 
ConnectionStringBuilder csb = new ConnectionStringBuilder(connectionString);
        
// set up JNDI context
Hashtable<String, String> hashtable = new Hashtable<>();
hashtable.put("connectionfactory.SBCF", "amqps://" + csb.getEndpoint().getHost() + "?amqp.idleTimeout=120000&amqp.traceFrames=true");
hashtable.put("queue.QUEUE", "BasicQueue");
hashtable.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.jms.jndi.JmsInitialContextFactory");
Context context = new InitialContext(hashtable);

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");

// Look up queue
Destination queue = (Destination) context.lookup("QUEUE");
```

#### <a name="configure-producer-and-consumer-destination-queues"></a>Üretici ve tüketici hedef sıra yapılandırın
Bir hedef Qpid özellikleri dosya JNDI sağlayıcısında tanımlamak için kullanılan giriş aşağıdaki biçimi şöyledir:

Üretici için hedef sıra oluşturmak için- 
```java
String queueName = "queueName";
Destination queue = (Destination) queueName;

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
Connection connection - cf.createConnection(csb.getSasKeyName(), csb.getSasKey());

Session session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);

// Create Producer
MessageProducer producer = session.createProducer(queue);
```

Tüketici için bir hedef sıra oluşturmak için- 
```java
String queueName = "queueName";
Destination queue = (Destination) queueName;

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
Connection connection - cf.createConnection(csb.getSasKeyName(), csb.getSasKey());

Session session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);

// Create Consumer
MessageConsumer consumer = session.createConsumer(queue);
```

### <a name="write-the-jms-application"></a>JMS uygulaması yazma
Özel API'ler veya JMS ile Service Bus kullanırken gerekli seçenekleri yoktur. Ancak, daha sonra ele alınacak bazı kısıtlamalar vardır. Herhangi bir JMS uygulama ile çözümleyebilmesi için JNDI ortamın yapılandırması ilk şey gerekli olduğu gibi bir **ConnectionFactory** ve hedefleri.

#### <a name="configure-the-jndi-initialcontext"></a>JNDI InitialContext yapılandırın
Yapılandırma bilgilerinin bir hashtable javax.naming.InitialContext sınıf oluşturucusuna geçirerek JNDI ortam yapılandırılır. İki hashtable öğeleri ilk bağlam Üreteç sağlayıcısı URL'si ve sınıf adı gereklidir. Aşağıdaki kod, özellikler dosyası adlı bir özellik dosyası ile JNDI sağlayıcı tabanlı Qpid kullanacak şekilde JNDI ortamı yapılandırma işlemi gösterilmektedir **servicebus.properties**.

```java
// set up JNDI context
Hashtable<String, String> hashtable = new Hashtable<>();
hashtable.put("connectionfactory.SBCF", "amqps://" + csb.getEndpoint().getHost() + \
"?amqp.idleTimeout=120000&amqp.traceFrames=true");
hashtable.put("queue.QUEUE", "BasicQueue");
hashtable.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.jms.jndi.JmsInitialContextFactory");
Context context = new InitialContext(hashtable);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a>Service Bus kuyruğu kullanarak basit bir JMS uygulaması
Aşağıdaki örnek program JMS TextMessages kuyruğun JNDI mantıksal adı ile bir Service Bus kuyruğuna gönderir ve iletileri geri alır.

Tüm kaynak kod ve yapılandırma gelen tüm bilgileri erişebileceğiniz [Azure Service Bus örnekleri JMS kuyruk hızlı başlangıç](https://github.com/Azure/azure-service-bus/tree/master/samples/Java/qpid-jms-client/JmsQueueQuickstart)

```java
// Copyright (c) Microsoft. All rights reserved.
// Licensed under the MIT license. See LICENSE file in the project root for full license information.

package com.microsoft.azure.servicebus.samples.jmsqueuequickstart;

import com.microsoft.azure.servicebus.primitives.ConnectionStringBuilder;
import org.apache.commons.cli.*;
import org.apache.log4j.*;

import javax.jms.*;
import javax.naming.Context;
import javax.naming.InitialContext;
import java.util.Hashtable;
import java.util.concurrent.atomic.AtomicInteger;
import java.util.function.Function;

/**
 * This sample demonstrates how to send messages from a JMS Queue producer into
 * an Azure Service Bus Queue, and receive them with a JMS message consumer.
 * JMS Queue. 
 */
public class JmsQueueQuickstart {

    // Number of messages to send
    private static int totalSend = 10;
    //Tracking counter for how many messages have been received; used as termination condition
    private static AtomicInteger totalReceived = new AtomicInteger(0);
    // log4j logger 
    private static Logger logger = Logger.getRootLogger();

    public void run(String connectionString) throws Exception {

        // The connection string builder is the only part of the azure-servicebus SDK library
        // we use in this JMS sample and for the purpose of robustly parsing the Service Bus 
        // connection string. 
        ConnectionStringBuilder csb = new ConnectionStringBuilder(connectionString);
        
        // set up JNDI context
        Hashtable<String, String> hashtable = new Hashtable<>();
        hashtable.put("connectionfactory.SBCF", "amqps://" + csb.getEndpoint().getHost() + "?amqp.idleTimeout=120000&amqp.traceFrames=true");
        hashtable.put("queue.QUEUE", "BasicQueue");
        hashtable.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.jms.jndi.JmsInitialContextFactory");
        Context context = new InitialContext(hashtable);
        ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
        
        // Look up queue
        Destination queue = (Destination) context.lookup("QUEUE");

        // we create a scope here so we can use the same set of local variables cleanly 
        // again to show the receive side separately with minimal clutter
        {
            // Create Connection
            Connection connection = cf.createConnection(csb.getSasKeyName(), csb.getSasKey());
            // Create Session, no transaction, client ack
            Session session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);

            // Create producer
            MessageProducer producer = session.createProducer(queue);

            // Send messages
            for (int i = 0; i < totalSend; i++) {
                BytesMessage message = session.createBytesMessage();
                message.writeBytes(String.valueOf(i).getBytes());
                producer.send(message);
                System.out.printf("Sent message %d.\n", i + 1);
            }

            producer.close();
            session.close();
            connection.stop();
            connection.close();
        }

        {
            // Create Connection
            Connection connection = cf.createConnection(csb.getSasKeyName(), csb.getSasKey());
            connection.start();
            // Create Session, no transaction, client ack
            Session session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
            // Create consumer
            MessageConsumer consumer = session.createConsumer(queue);
            // create a listener callback to receive the messages
            consumer.setMessageListener(message -> {
                try {
                    // receives message is passed to callback
                    System.out.printf("Received message %d with sq#: %s\n",
                            totalReceived.incrementAndGet(), // increments the tracking counter
                            message.getJMSMessageID());
                    message.acknowledge();
                } catch (Exception e) {
                    logger.error(e);
                }
            });

            // wait on the main thread until all sent messages have been received
            while (totalReceived.get() < totalSend) {
                Thread.sleep(1000);
            }
            consumer.close();
            session.close();
            connection.stop();
            connection.close();
        }

        System.out.printf("Received all messages, exiting the sample.\n");
        System.out.printf("Closing queue client.\n");
    }

    public static void main(String[] args) {

        System.exit(runApp(args, (connectionString) -> {
            JmsQueueQuickstart app = new JmsQueueQuickstart();
            try {
                app.run(connectionString);
                return 0;
            } catch (Exception e) {
                System.out.printf("%s", e.toString());
                return 1;
            }
        }));
    }

    static final String SB_SAMPLES_CONNECTIONSTRING = "SB_SAMPLES_CONNECTIONSTRING";

    public static int runApp(String[] args, Function<String, Integer> run) {
        try {

            String connectionString = null;

            // parse connection string from command line
            Options options = new Options();
            options.addOption(new Option("c", true, "Connection string"));
            CommandLineParser clp = new DefaultParser();
            CommandLine cl = clp.parse(options, args);
            if (cl.getOptionValue("c") != null) {
                connectionString = cl.getOptionValue("c");
            }

            // get overrides from the environment
            String env = System.getenv(SB_SAMPLES_CONNECTIONSTRING);
            if (env != null) {
                connectionString = env;
            }

            if (connectionString == null) {
                HelpFormatter formatter = new HelpFormatter();
                formatter.printHelp("run jar with", "", options, "", true);
                return 2;
            }
            return run.apply(connectionString);
        } catch (Exception e) {
            System.out.printf("%s", e.toString());
            return 3;
        }
    }
}
```

### <a name="run-the-application"></a>Uygulamayı çalıştırma
Geçirmek **bağlantı dizesi** uygulamayı çalıştırmak için paylaşılan erişim ilkeleri'nden.
Çıkış biçiminde uygulamayı çalıştırarak aşağıdadır:

```
> mvn clean package
>java -jar ./target/jmsqueuequickstart-1.0.0-jar-with-dependencies.jar -c "<CONNECTION_STRING>"

Sent message 1.
Sent message 2.
Sent message 3.
Sent message 4.
Sent message 5.
Sent message 6.
Sent message 7.
Sent message 8.
Sent message 9.
Sent message 10.
Received message 1 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-1
Received message 2 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-2
Received message 3 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-3
Received message 4 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-4
Received message 5 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-5
Received message 6 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-6
Received message 7 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-7
Received message 8 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-8
Received message 9 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-9
Received message 10 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-10
Received all messages, exiting the sample.
Closing queue client.

```

## <a name="amqp-disposition-and-service-bus-operation-mapping"></a>AMQP değerlendirme ve Service Bus işlemi eşleme
Nasıl bir Service Bus işlemi için bir AMQP değerlendirme çevirir aşağıda verilmiştir:

```
ACCEPTED = 1; -> Complete()
REJECTED = 2; -> DeadLetter()
RELEASED = 3; (just unlock the message in service bus, will then get redelivered)
MODIFIED_FAILED = 4; -> Abandon() which increases delivery count
MODIFIED_FAILED_UNDELIVERABLE = 5; -> Defer()
```

## <a name="jms-topics-vs-service-bus-topics"></a>JMS konuları vs. Service Bus Konuları
Azure Service Bus konuları ve abonelikleri aracılığıyla Java mesaj hizmeti (JMS) API kullanarak temel gönderme sağlar ve özellikleri alır. Service Bus konuları JMS konularından farklıdır ve bazı ayarlamalar gerekli olsa bile diğer ileti aracıları JMS uyumlu API'leri ile uygulamalar taşırken uygun bir seçenek olan. 

Azure Service Bus konuları, Azure portalında veya Azure komut satırı araçları, Azure kaynak yönetimi arabirimi aracılığıyla yönetilen adlandırılmış, paylaşılan, dayanıklı abonelikleri içine iletiler yönlendirebilirsiniz. Her abonelik en fazla 2000 seçimi kurallar, her bir filtre koşulu sahip olabilir ve SQL filtreleri, ayrıca bir meta veri dönüştürme eylemi sağlar. Her bir filtre koşulu eşleşme tehj aboneliğe kopyalanacak giriş iletisi seçer.  

Aboneliklerden iletiler almaya kuyruklardaki iletilere aynıdır. Her aboneliğin, ilişkili bir eski ileti sırası yanı sıra otomatik olarak başka bir kuyruk veya konulara ileti iletme yeteneği vardır. 

JMS konuları istemcileri filtreleme iletilerinin ileti Seçici ile isteğe bağlı olarak izin belirlediyseniz ve dayanıklı aboneleri dinamik olarak oluşturmak izin verin. Service Bus tarafından paylaşılmayan varlıkları desteklenmez. Service Bus SQL filtresi kuralı sözdizimi olduğu, ancak JMS tarafından desteklenen ileti Seçici söz dizimi çok benzer. 

Bu örnekte gösterildiği gibi JMS konu Yayımcı tarafında Service Bus ile uyumlu olan ancak dinamik aboneleri değildir. Aşağıdaki topoloji ilgili JMS API'leri, Service Bus ile desteklenmez. 

## <a name="unsupported-features-and-restrictions"></a>Desteklenmeyen özellikler ve kısıtlamalar
Yani JMS Service Bus ile AMQP 1.0 üzerinden kullanırken, aşağıdaki kısıtlamaları mevcuttur:

* Yalnızca bir **MessageProducer** veya **MessageConsumer** başına izin verilen **oturumu**. Birden çok oluşturmanız gerekiyorsa **MessageProducers** veya **MessageConsumers** bir uygulamada, ayrılmış bir oluşturma **oturumu** her biri için.
* Volatile konu abonelikleri şu anda desteklenmemektedir.
* **MessageSelectors** şu anda desteklenmiyor.
* İşlenen oturumları ve dağıtılmış işlemler desteklenmez.

Ek olarak, Azure Service Bus denetim düzlemi veri düzlemine öğesinden ayırır ve bu nedenle birkaç JMS'ın dinamik topoloji işlevleri desteklemiyor olabilir:

| Desteklenmeyen yöntemi          | Şununla değiştir                                                                             |
|-----------------------------|------------------------------------------------------------------------------------------|
| createDurableSubscriber     | ileti Seçici taşıma konu aboneliği oluştur                                 |
| createDurableConsumer       | ileti Seçici taşıma konu aboneliği oluştur                                 |
| createSharedConsumer        | Service Bus konu başlıklarını her zaman paylaşılabilir, yukarıya bakın                                       |
| createSharedDurableConsumer | Service Bus konu başlıklarını her zaman paylaşılabilir, yukarıya bakın                                       |
| createTemporaryTopic        | Yönetim aracılığıyla konu oluşturma araçları/API/portal ile *AutoDeleteOnIdle* için süre ayarlama |
| createTopic                 | Yönetim Araçları/API/portal aracılığıyla bir konu oluşturun                                           |
| aboneliği iptal et                 | Konu Yönetim Araçları/API/portal Sil                                             |
| createBrowser               | Desteklenmeyen. Service Bus API'sini Peek() işlevlerini kullanma                         |
| createQueue                 | Yönetim Araçları/API/portal aracılığıyla kuyruk oluşturma                                           | 
| createTemporaryQueue        | Yönetim aracılığıyla kuyruk oluşturma araçları/API/portal ile *AutoDeleteOnIdle* için süre ayarlama |

## <a name="summary"></a>Özet
Bu nasıl yapılır kılavuzunda AMQP 1.0 ve popüler JMS API kullanarak Service Bus aracılı Mesajlaşma özelliklerinin (sıralar ve yayımlama/abone olma konuları) Java kullanma gösterdi.

Hizmet veri yolu AMQP 1.0 .NET, C, Python ve PHP dahil olmak üzere diğer dillerden de kullanabilirsiniz. Bu farklı diller kullanılarak oluşturulan bileşenleri, iletileri güvenilir bir şekilde gönderip alır ve hizmet veri yolunda AMQP 1.0 kullanarak tam uygunlukta destekler.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure hizmet veri yolu AMQP 1.0 desteği](service-bus-amqp-overview.md)
* [Hizmet veri yolu .NET API ile AMQP 1.0 kullanma](service-bus-dotnet-advanced-message-queuing.md)
* [Hizmet veri yolu AMQP 1.0 Geliştirici Kılavuzu](service-bus-amqp-dotnet.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Java Geliştirici Merkezi](https://azure.microsoft.com/develop/java/)

