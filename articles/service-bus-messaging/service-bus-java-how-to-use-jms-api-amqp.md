---
title: AMQP 1.0 ile Java Service Bus API'sini kullanma | Microsoft Docs
description: Advanced Message Queuing Protodol (AMQP) 1.0 ile Azure Service Bus ile Java mesaj hizmeti (JMS) kullanma
services: service-bus-messaging
documentationcenter: java
author: spelluru
manager: timlt
editor: ''
ms.assetid: be766f42-6fd1-410c-b275-8c400c811519
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 08/10/2017
ms.author: spelluru
ms.openlocfilehash: bfab0c374e4b20b09167f37363fe0681144426ac
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43699354"
---
# <a name="how-to-use-the-java-message-service-jms-api-with-service-bus-and-amqp-10"></a>Hizmet veri yolu AMQP 1.0 ile Java mesaj hizmeti (JMS) API kullanma
Advanced Message Queuing Protocol (AMQP) 1.0 sağlam, platformlar arası Mesajlaşma uygulamaları oluşturmak için kullanabileceğiniz bir verimli, güvenilir, hat düzeyinde bir Mesajlaşma protokolüdür.

Destek için hizmet veri yolu AMQP 1.0 sıraya alma kullanın ve yayımlama/aracılı Mesajlaşma özelliklerinin verimli bir ikili protokolü kullanılarak platformlar aralığından abonelik anlamına gelir. Ayrıca, dillerin, çerçevelerin ve işletim sistemlerinin bir karışımını kullanılarak oluşturulan bileşenlerden oluşan bir uygulama oluşturabilirsiniz.

Bu makalede, Java uygulamalarından popüler Java mesaj hizmeti (JMS) API standart kullanarak Service Bus Mesajlaşma özelliklerinin (sıralar ve yayımlama/abone olma konuları) kullanmayı açıklar. Var olan bir [Yardımcısı makale](service-bus-amqp-dotnet.md) , hizmet veri yolu .NET API kullanarak aynı şeyi nasıl açıklar. AMQP 1.0 kullanarak platformlar arası Mesajlaşma hakkında bilgi edinmek için bu iki Kılavuzlar birlikte kullanabilirsiniz.

## <a name="get-started-with-service-bus"></a>Service Bus’ı kullanmaya başlama
Bu kılavuz, adlandırılan bir kuyruğun içeren bir Service Bus ad alanı zaten sahip olduğunuzu varsayar **sıra 1**. Bunu yapmanız durumunda yapabilecekleriniz [ad alanı ve Kuyruk oluşturma](service-bus-create-namespace-portal.md) kullanarak [Azure portalında](https://portal.azure.com). Service Bus ad alanları ve Kuyruk oluşturma hakkında daha fazla bilgi için bkz. [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md).

> [!NOTE]
> Bölümlenmiş kuyruklar ve konular da AMQP destekler. Daha fazla bilgi için [bölümlenmiş Mesajlaşma varlıkları](service-bus-partitioning.md) ve [hizmet veri yolu AMQP 1.0 desteği bölümlenmiş kuyruklar ve konular](service-bus-partitioned-queues-and-topics-amqp-overview.md).
> 
> 

## <a name="downloading-the-amqp-10-jms-client-library"></a>AMQP 1.0 JMS istemci Kitaplığı'nı indirme
Apache Qpid JMS AMQP 1.0 istemci Kitaplığı'nın en son sürümü karşıdan yükleme konumu hakkında daha fazla bilgi için ziyaret [ https://qpid.apache.org/download.html ](https://qpid.apache.org/download.html).

Aşağıdaki dört JAR dosyaları Apache Qpid JMS AMQP 1.0 dağıtım arşivden için Java sınıf yolu oluşturma ve Service Bus ile JMS uygulamaları çalıştırırken eklemelisiniz:

* geronimo jms\_1.1\_1.0.jar belirtimi
* qpid-amqp-1-0-client-[Version].jar
* qpid-amqp-1-0-Client-jms-[Version].jar
* qpid-amqp-1-0-Common-[Version].jar

## <a name="coding-java-applications"></a>Java uygulamalarını kodlama
### <a name="java-naming-and-directory-interface-jndi"></a>Java adlandırma ve dizini arabirimi (JNDI)
JMS mantıksal ve fiziksel adlarını arasında bir ayrım oluşturmak için Java adlandırma ve dizin arabirimi (JNDI) kullanır. İki tür JMS nesnelerin JNDI kullanarak çözülmüş: ConnectionFactory ve hedef. JNDI farklı dizin hizmetleri, ad çözümlemesi görevlerini işlemek için takılabilir bir sağlayıcı modeli kullanır. Dosya tabanlı JNDI aşağıdaki özellikler dosyası kullanılarak yapılandırılan sağlayıcısı kitaplığı basit bir özellik ile gelir Apache Qpid JMS AMQP 1.0 biçimlendirin:

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

#### <a name="configure-the-connectionfactory"></a>ConnectionFactory yapılandırın
Girişi tanımlamak için kullanılan bir **ConnectionFactory** Qpid özellikleri dosyasında JNDI sağlayıcısı aşağıdaki biçimi şöyledir:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Burada **[jndi_name]** ve **[ConnectionURL]** aşağıdaki anlamlara sahiptir:

* **[jndi_name]** : ConnectionFactory mantıksal adı. Bu, JNDI IntialContext.lookup() yöntemini kullanarak bir Java uygulamasında çözümlenir addır.
* **[ConnectionURL]** : JMS kitaplığı AMQP aracısı için gereken bilgileri sağlayan bir URL.

Biçimi **ConnectionURL** aşağıdaki gibidir:

```
amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
```
Burada **[ad]**, **[SASPolicyName]** ve **[SASPolicyKey]** aşağıdaki anlamlara sahiptir:

* **[ad]** : Service Bus ad alanı.
* **[SASPolicyName]** : Sıra paylaşılan erişim imzası ilke adı.
* **[SASPolicyKey]** : Sıra paylaşılan erişim imzası ilke anahtarı.

> [!NOTE]
> URL kodlaması parola kendiniz bağlanmalısınız. Yararlı bir URL kodlaması yardımcı programı kullanılabilir [ http://www.w3schools.com/tags/ref_urlencode.asp ](http://www.w3schools.com/tags/ref_urlencode.asp).
> 
> 

#### <a name="configure-destinations"></a>Hedefleri yapılandırma
Bir hedef Qpid özellikleri dosya JNDI sağlayıcısında tanımlamak için kullanılan giriş aşağıdaki biçimi şöyledir:

```
queue.[jndi_name] = [physical_name]
```

or

```
topic.[jndi_name] = [physical_name]
```

Burada **[JNDI\_adı]** ve **[fiziksel\_adı]** aşağıdaki anlamlara sahiptir:

* **[jndi_name]** : Hedef mantıksal adı. Bu, JNDI IntialContext.lookup() yöntemini kullanarak bir Java uygulamasında çözümlenir addır.
* **[physical_name]** : Uygulama gönderir veya iletileri alan Service Bus varlık adı.

> [!NOTE]
> Bir Service Bus konu aboneliğinden alındığında, JNDI belirtilen fiziksel adı konunun adı olmalıdır. Dayanıklı abonelik JMS uygulama kodunda oluşturulduğunda abonelik adı sağlanır. [Hizmet veri yolu AMQP 1.0 Geliştirici Kılavuzu](service-bus-amqp-dotnet.md) JMS hizmet veri yolu konuları ile çalışma hakkında daha fazla ayrıntı sağlar.
> 
> 

### <a name="write-the-jms-application"></a>JMS uygulaması yazma
Özel API'ler veya JMS ile Service Bus kullanırken gerekli seçenekleri yoktur. Ancak, daha sonra ele alınacak bazı kısıtlamalar vardır. Herhangi bir JMS uygulama ile çözümleyebilmesi için JNDI ortamın yapılandırması ilk şey gerekli olduğu gibi bir **ConnectionFactory** ve hedefleri.

#### <a name="configure-the-jndi-initialcontext"></a>JNDI InitialContext yapılandırın
Yapılandırma bilgilerinin bir hashtable javax.naming.InitialContext sınıf oluşturucusuna geçirerek JNDI ortam yapılandırılır. İki hashtable öğeleri ilk bağlam Üreteç sağlayıcısı URL'si ve sınıf adı gereklidir. Aşağıdaki kod, özellikler dosyası adlı bir özellik dosyası ile JNDI sağlayıcı tabanlı Qpid kullanacak şekilde JNDI ortamı yapılandırma işlemi gösterilmektedir **servicebus.properties**.

```java
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
InitialContext context = new InitialContext(env);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a>Service Bus kuyruğu kullanarak basit bir JMS uygulaması
Aşağıdaki örnek program JMS TextMessages kuyruğun JNDI mantıksal adı ile bir Service Bus kuyruğuna gönderir ve iletileri geri alır.

```java
// SimpleSenderReceiver.java

import javax.jms.*;
import javax.naming.Context;
import javax.naming.InitialContext;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Hashtable;
import java.util.Random;

public class SimpleSenderReceiver implements MessageListener {
    private static boolean runReceiver = true;
    private Connection connection;
    private Session sendSession;
    private Session receiveSession;
    private MessageProducer sender;
    private MessageConsumer receiver;
    private static Random randomGenerator = new Random();

    public SimpleSenderReceiver() throws Exception {
        // Configure JNDI environment
        Hashtable<String, String> env = new Hashtable<String, String>();
        env.put(Context.INITIAL_CONTEXT_FACTORY, 
                   "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory");
        env.put(Context.PROVIDER_URL, "servicebus.properties");
        Context context = new InitialContext(env);

        // Look up ConnectionFactory and Queue
        ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
        Destination queue = (Destination) context.lookup("QUEUE");

        // Create Connection
        connection = cf.createConnection();

        // Create sender-side Session and MessageProducer
        sendSession = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        sender = sendSession.createProducer(queue);

        if (runReceiver) {
            // Create receiver-side Session, MessageConsumer,and MessageListener
            receiveSession = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
            receiver = receiveSession.createConsumer(queue);
            receiver.setMessageListener(this);
            connection.start();
        }
    }

    public static void main(String[] args) {
        try {

            if ((args.length > 0) && args[0].equalsIgnoreCase("sendonly")) {
                runReceiver = false;
            }

            SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
            System.out.println("Press [enter] to send a message. Type 'exit' + [enter] to quit.");
            BufferedReader commandLine = new java.io.BufferedReader(new InputStreamReader(System.in));

            while (true) {
                String s = commandLine.readLine();
                if (s.equalsIgnoreCase("exit")) {
                    simpleSenderReceiver.close();
                    System.exit(0);
                } else {
                    simpleSenderReceiver.sendMessage();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private void sendMessage() throws JMSException {
        TextMessage message = sendSession.createTextMessage();
        message.setText("Test AMQP message from JMS");
        long randomMessageID = randomGenerator.nextLong() >>>1;
        message.setJMSMessageID("ID:" + randomMessageID);
        sender.send(message);
        System.out.println("Sent message with JMSMessageID = " + message.getJMSMessageID());
    }

    public void close() throws JMSException {
        connection.close();
    }

    public void onMessage(Message message) {
        try {
            System.out.println("Received message with JMSMessageID = " + message.getJMSMessageID());
            message.acknowledge();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}    
```

### <a name="run-the-application"></a>Uygulamayı çalıştırma
Uygulamayı çalıştıran biçiminde çıktı üretir:

```
> java SimpleSenderReceiver
Press [enter] to send a message. Type 'exit' + [enter] to quit.

Sent message with JMSMessageID = ID:2867600614942270318
Received message with JMSMessageID = ID:2867600614942270318

Sent message with JMSMessageID = ID:7578408152750301483
Received message with JMSMessageID = ID:7578408152750301483

Sent message with JMSMessageID = ID:956102171969368961
Received message with JMSMessageID = ID:956102171969368961
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Platformlar arası .NET JMS arasında ileti
Bu kılavuz, JMS kullanarak Service Bus gelen ve giden iletileri gönderip nasıl oluşturulacağını gösterir. Ancak, iletileri güvenilir bir şekilde ve tam uygunlukta değişimi ile farklı dillerde yazılmış olan bileşenlerden oluşturulacak uygulamalar sağlar AMQP 1.0 başlıca avantajlarından biri olur.

Yukarıda açıklanan örnek JMS uygulama ve bir yardımcı makalesinden gerçekleştirilecek benzer bir .NET uygulaması kullanarak [.NET ile AMQP 1.0 kullanarak Service Bus](service-bus-amqp-dotnet.md), .NET ve Java'dan arasında iletileri gönderip alabilir. Platformlar arası Mesajlaşma Service Bus ve AMQP 1.0 kullanarak, ayrıntıları hakkında daha fazla bilgi için bu makaleyi okuyun.

### <a name="jms-to-net"></a>.NET için JMS
.NET ileti JMS göstermek için:

* Herhangi bir komut satırı bağımsız değişkeni olmadan .NET örnek uygulaması başlatın.
* Java örnek uygulaması "sendonly" komut satırı bağımsız değişkeniyle başlatın. Bu modda uygulama iletileri almaz sıradan, yalnızca gönderir.
* Tuşuna **Enter** birkaç kez Java uygulama konsolunda neden olacak gönderilecek iletilerin.
* Bu iletiler, .NET uygulama tarafından alınır.

#### <a name="output-from-jms-application"></a>JMS uygulama çıktısı
```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

#### <a name="output-from-net-application"></a>.NET uygulama çıktısı
```
> SimpleSenderReceiver.exe    
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

### <a name="net-to-jms"></a>JMS için .NET
.NET JMS ileti göstermek için:

* .NET örnek uygulaması "sendonly" komut satırı bağımsız değişkeniyle başlatın. Bu modda uygulama iletileri almaz sıradan, yalnızca gönderir.
* Herhangi bir komut satırı bağımsız değişkeni olmadan Java örnek uygulamayı başlatın.
* Tuşuna **Enter** birkaç kez .NET uygulama konsolunda neden olacak gönderilecek iletilerin.
* Bu iletiler, Java uygulama tarafından alınır.

#### <a name="output-from-net-application"></a>.NET uygulama çıktısı
```
> SimpleSenderReceiver.exe sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with MessageID = d64e681a310a48a1ae0ce7b017bf1cf3    
Sent message with MessageID = 98a39664995b4f74b32e2a0ecccc46bb
Sent message with MessageID = acbca67f03c346de9b7893026f97ddeb
exit
```

#### <a name="output-from-jms-application"></a>JMS uygulama çıktısı
```
> java SimpleSenderReceiver    
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with JMSMessageID = ID:d64e681a310a48a1ae0ce7b017bf1cf3
Received message with JMSMessageID = ID:98a39664995b4f74b32e2a0ecccc46bb
Received message with JMSMessageID = ID:acbca67f03c346de9b7893026f97ddeb
exit
```

## <a name="unsupported-features-and-restrictions"></a>Desteklenmeyen özellikler ve kısıtlamalar
Yani JMS Service Bus ile AMQP 1.0 üzerinden kullanırken, aşağıdaki kısıtlamaları mevcuttur:

* Yalnızca bir **MessageProducer** veya **MessageConsumer** başına izin verilen **oturumu**. Birden çok oluşturmanız gerekiyorsa **MessageProducers** veya **MessageConsumers** bir uygulamada, ayrılmış bir oluşturma **oturumu** her biri için.
* Volatile konu abonelikleri şu anda desteklenmemektedir.
* **MessageSelectors** şu anda desteklenmiyor.
* Geçici hedefleri; Örneğin, **TemporaryQueue**, **TemporaryTopic** şu anda, bunların ile desteklenmemektedir **QueueRequestor** ve **TopicRequestor**Bunları API'leri.
* İşlenen oturumları ve dağıtılmış işlemler desteklenmez.

## <a name="summary"></a>Özet
Bu nasıl yapılır kılavuzunda AMQP 1.0 ve popüler JMS API kullanarak Service Bus aracılı Mesajlaşma özelliklerinin (sıralar ve yayımlama/abone olma konuları) Java kullanma gösterdi.

Hizmet veri yolu AMQP 1.0 .NET, C, Python ve PHP dahil olmak üzere diğer dillerden de kullanabilirsiniz. Bu farklı diller kullanılarak oluşturulan bileşenleri, iletileri güvenilir bir şekilde gönderip alır ve hizmet veri yolunda AMQP 1.0 kullanarak tam uygunlukta destekler.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure hizmet veri yolu AMQP 1.0 desteği](service-bus-amqp-overview.md)
* [Hizmet veri yolu .NET API ile AMQP 1.0 kullanma](service-bus-dotnet-advanced-message-queuing.md)
* [Hizmet veri yolu AMQP 1.0 Geliştirici Kılavuzu](service-bus-amqp-dotnet.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Java Geliştirici Merkezi](https://azure.microsoft.com/develop/java/)

