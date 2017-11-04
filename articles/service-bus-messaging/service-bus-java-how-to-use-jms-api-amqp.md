---
title: AMQP 1.0 Java hizmet veri yolu API'si ile kullanma | Microsoft Docs
description: "Java ileti hizmeti (JMS) Azure Service Bus ve Gelişmiş Message Queuing Protodol (AMQP) 1.0 ile nasıl kullanılır."
services: service-bus-messaging
documentationcenter: java
author: sethmanheim
manager: timlt
editor: 
ms.assetid: be766f42-6fd1-410c-b275-8c400c811519
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 0848facd764c4fb0d7f95c1ae89ecb02a32257e1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-the-java-message-service-jms-api-with-service-bus-and-amqp-10"></a>Hizmet veri yolu AMQP 1.0 ile Java ileti hizmeti (JMS) API kullanma
Gelişmiş Message Queuing Protokolü (AMQP) 1.0, güçlü, platformlar arası ileti uygulamaları oluşturmak için kullanabileceğiniz bir verimli, güvenilir ve hat düzeyinde Mesajlaşma protokolüdür.

Destek için hizmet veri yolu AMQP 1.0 queuing kullanın ve yayımlama/aracılı Mesajlaşma özellikleri verimli bir ikili protokolünü kullanarak platformları aralığından abonelik anlamına gelir. Ayrıca, diller, çerçevelerinin ve işletim sistemlerinin bir karışımını kullanılarak oluşturulan bileşenlerden oluşan uygulamalar oluşturabilir.

Bu makalede, popüler Java ileti hizmeti (JMS) API standart kullanarak Java uygulamalardan nasıl Service Bus (kuyruklar ve konu başlıkları Yayınla/Abone ol) özellikleri Mesajlaşma kullanılacağı açıklanmaktadır. Var olan bir [yardımcı makale](service-bus-amqp-dotnet.md) , hizmet veri yolu .NET API kullanarak aynı şeyi açıklanmaktadır. AMQP 1.0 kullanarak platformlar arası ileti hakkında bilgi edinmek için bu iki kılavuzları birlikte kullanabilirsiniz.

## <a name="get-started-with-service-bus"></a>Service Bus’ı kullanmaya başlama
Bu kılavuz, adlandırılan bir kuyruğun içeren bir hizmet veri yolu ad alanı zaten sahip olduğunuzu varsayar **sıra 1**. Bunu yapmanız sonra şunları yapabilirsiniz [ad alanı oluşturup sıra](service-bus-create-namespace-portal.md) kullanarak [Azure portal](https://portal.azure.com). Hizmet veri yolu ad alanları ve Kuyruklar oluşturma hakkında daha fazla bilgi için bkz: [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md).

> [!NOTE]
> Bölümlenmiş kuyruklar ve konu başlıkları da AMQP destekler. Daha fazla bilgi için bkz: [bölümlenmiş Mesajlaşma varlıkları](service-bus-partitioning.md) ve [hizmet veri yolu AMQP 1.0 desteği bölümlenmiş kuyruklar ve konu başlıkları](service-bus-partitioned-queues-and-topics-amqp-overview.md).
> 
> 

## <a name="downloading-the-amqp-10-jms-client-library"></a>AMQP 1.0 JMS istemci kitaplığı indirme
Apache Qpid JMS AMQP 1.0 istemci kitaplığının en son sürümü karşıdan yükleme konumu hakkında daha fazla bilgi için ziyaret [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html).

Aşağıdaki dört JAR dosyalarını Apache Qpid JMS AMQP 1.0 dağıtım arşivinden Java sınıf oluşturma ve Service Bus ile JMS uygulamaları çalıştırırken eklemeniz gerekir:

* geronimo jms\_1.1\_spec 1.0.jar
* qpid-amqp-1-0-client-[Version].jar
* qpid-amqp-1-0-Client-jms-[Version].jar
* qpid-amqp-1-0-Common-[Version].jar

## <a name="coding-java-applications"></a>Java uygulamalarını kodlama
### <a name="java-naming-and-directory-interface-jndi"></a>Java adlandırma ve dizini arabirimi (JNDI)
JMS, mantıksal ve fiziksel adlarını arasında ayrım oluşturmak için Java adlandırma ve dizin arabirimi (JNDI) kullanır. İki tür JMS nesneleri JNDI kullanarak çözümlenir: ConnectionFactory ve hedef. JNDI ad çözümleme görevlerini işlemek için farklı dizin hizmetleri ekleyebilirsiniz sağlayıcı modeli kullanır. Dosya tabanlı JNDI aşağıdaki özellikleri dosyası kullanılarak yapılandırılmış sağlayıcısı kitaplığı basit özellikleri ile birlikte gelen Apache Qpid JMS AMQP 1.0 Biçimlendir:

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
Girişi tanımlamak için kullanılan bir **ConnectionFactory** Qpid özellikleri JNDI sağlayıcısı şu biçimde dosyasıdır:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Burada **[jndi_name]** ve **[ConnectionURL]** şu anlama gelir:

* **[jndi_name]** : ConnectionFactory mantıksal adı. Bu, JNDI IntialContext.lookup() yöntemini kullanarak Java uygulamasında çözümlenir addır.
* **[ConnectionURL]** : JMS kitaplığı AMQP aracısı için gereken bilgileri sağlayan bir URL.

Biçimi **ConnectionURL** aşağıdaki gibidir:

```
amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
```
Burada **[ad]**, **[SASPolicyName]** ve **[SASPolicyKey]** şu anlama gelir:

* **[ad]** : Hizmet veri yolu ad alanı.
* **[SASPolicyName]** : Sıra paylaşılan erişim imzası ilke adı.
* **[SASPolicyKey]** : Sıra paylaşılan erişim imzası ilke anahtarı.

> [!NOTE]
> URL kodlama parola kullanmanız gerekir. URL kodlaması yararlı bir yardımcı şu adresten edinilebilir [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).
> 
> 

#### <a name="configure-destinations"></a>Hedeflerini yapılandırma
Bir hedef Qpid özellikleri dosya JNDI sağlayıcısında tanımlamak için kullanılan girişinin aşağıdaki biçimi şöyledir:

```
queue.[jndi_name] = [physical_name]
```

or

```
topic.[jndi_name] = [physical_name]
```

Burada **[JNDI\_adı]** ve **[fiziksel\_adı]** şu anlama gelir:

* **[jndi_name]** : Hedef mantıksal adı. Bu, JNDI IntialContext.lookup() yöntemini kullanarak Java uygulamasında çözümlenir addır.
* **[physical_name]** : Uygulama gönderir veya iletileri alan Service Bus varlık adı.

> [!NOTE]
> Bir hizmet veri yolu konusu abonelikten alırken JNDI içinde belirtilen fiziksel adı konu adı olmalıdır. Dayanıklı abonelik JMS uygulama kodunda oluşturulduğunda, abonelik adını sağlanır. [Hizmet veri yolu AMQP 1.0 Geliştirici Kılavuzu](service-bus-amqp-dotnet.md) JMS Service Bus konularından ile çalışma hakkında daha fazla ayrıntı sağlar.
> 
> 

### <a name="write-the-jms-application"></a>JMS uygulama yazma
Özel API'leri veya JMS Service Bus ile kullanırken gereken seçenekler yok. Ancak, daha sonra ele alınacak bazı kısıtlamalar vardır. Herhangi bir JMS uygulama ile çözümleyebilmesi için JNDI ortamının yapılandırması ilk şey gerekli olduğu gibi bir **ConnectionFactory** ve hedefler.

#### <a name="configure-the-jndi-initialcontext"></a>JNDI InitialContext yapılandırın
JNDI ortam yapılandırma bilgilerini bir hashtable javax.naming.InitialContext sınıfı oluşturucusuna geçirerek yapılandırılır. İki hashtable öğelerindeki ilk bağlam fabrikası ve sağlayıcı URL sınıf adı gereklidir. Aşağıdaki kod özellikleri dosya adlı bir özellik dosyası JNDI sağlayıcı tabanlı Qpid kullanmak için JNDI ortamını yapılandırma gösterir **servicebus.properties**.

```java
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
InitialContext context = new InitialContext(env);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a>Service Bus kuyruğu kullanarak basit bir JMS uygulama
Aşağıdaki örnek program sırasının JNDI mantıksal adı ile Service Bus kuyruğuna JMS TextMessages gönderir ve iletilerini geri alır.

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
Uygulamayı çalıştıran biçiminde bir çıktı üretir:

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

## <a name="cross-platform-messaging-between-jms-and-net"></a>Platformlar arası ileti JMS ve .NET arasında
Bu kılavuz, JMS kullanarak Service Bus gelen ve giden ileti gönderme ve alma nasıl oluşturulacağını gösterir. Ancak, AMQP 1.0 kilit yararları güvenilir bir şekilde ve tam bir güvenilirlik, alınıp iletilerle farklı dillerde yazılmış bileşenlerinden oluşturulacak uygulamalar sağlayan biridir.

Yukarıda açıklanan örnek JMS uygulama ve yardımcı makaleden gerçekleştirilecek benzer bir .NET uygulaması kullanarak [kullanarak Service Bus .NET AMQP 1.0 ile birlikte gelen](service-bus-amqp-dotnet.md), .NET ve Java arasında iletileri değiştirebilir. Platformlar arası Service Bus ve AMQP 1.0 kullanarak Mesajlaşma, ayrıntıları hakkında daha fazla bilgi için bu makaleyi okuyun.

### <a name="jms-to-net"></a>.NET için JMS
.NET ileti JMS göstermek için:

* Tüm komut satırı bağımsız değişkenleri olmadan .NET örnek uygulaması başlatın.
* Java örnek uygulaması "sendonly" komut satırı bağımsız değişkeniyle başlatın. Bu modda, uygulama iletileri almaz sıradan, yalnızca gönderir.
* Tuşuna **Enter** birkaç kez Java uygulama konsolunda neden olacak gönderilecek iletilerin.
* Bu iletiler .NET uygulama tarafından alınır.

#### <a name="output-from-jms-application"></a>JMS uygulama çıktısı
```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

#### <a name="output-from-net-application"></a>.NET uygulaması çıktısı
```
> SimpleSenderReceiver.exe    
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

### <a name="net-to-jms"></a>JMS için .NET
İleti gönderme ve alma JMS .NET göstermek için:

* .NET örnek uygulaması "sendonly" komut satırı bağımsız değişkeniyle başlatın. Bu modda, uygulama iletileri almaz sıradan, yalnızca gönderir.
* Tüm komut satırı bağımsız değişkenleri olmadan Java örnek uygulamayı başlatın.
* Tuşuna **Enter** birkaç kez .NET uygulama konsolunda neden olacak gönderilecek iletilerin.
* Bu iletiler Java uygulama tarafından alınır.

#### <a name="output-from-net-application"></a>.NET uygulaması çıktısı
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
Yani JMS Service Bus ile AMQP 1.0 üzerinden kullanırken, aşağıdaki kısıtlamalar mevcuttur:

* Yalnızca bir **MessageProducer** veya **MessageConsumer** başına izin verilen **oturum**. Birden çok oluşturmanız gerekiyorsa **MessageProducers** veya **MessageConsumers** bir uygulamada, ayrılmış bir oluşturma **oturum** her biri için.
* Volatile konu abonelikleri şu anda desteklenmemektedir.
* **MessageSelectors** şu anda desteklenmiyor.
* Geçici hedefleri; Örneğin, **TemporaryQueue**, **TemporaryTopic** şu anda, bunların ile desteklenmemektedir **QueueRequestor** ve **TopicRequestor**Kullanılacakları API'leri.
* Uygulaması yapılan oturumlar ve dağıtılmış işlemler desteklenmiyor.

## <a name="summary"></a>Özet
Nasıl yapılır bu kılavuz, popüler JMS API ve AMQP 1.0 kullanarak Service Bus aracılı Mesajlaşma özellikleri (kuyruklar ve konu başlıkları Yayınla/Abone ol) Java'dan kullanmak nasıl oluşturulacağını gösterir.

Hizmet veri yolu AMQP 1.0 .NET, C, Python ve PHP gibi diğer dillerden de kullanabilirsiniz. Bu farklı diller kullanılarak oluşturulan bileşenleri ileti güvenilir bir şekilde gönderip ve AMQP 1.0 kullanarak tam bir güvenilirlik hizmet veri yolundaki destekler.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure hizmet veri yolu AMQP 1.0 desteği](service-bus-amqp-overview.md)
* [AMQP 1.0 ile Service Bus .NET API kullanma](service-bus-dotnet-advanced-message-queuing.md)
* [Hizmet veri yolu AMQP 1.0 Geliştirici Kılavuzu](service-bus-amqp-dotnet.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Java Geliştirici Merkezi](https://azure.microsoft.com/develop/java/)

