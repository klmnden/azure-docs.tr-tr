---
title: Olaylar farklı protokollere - Azure Event Hubs kullanan uygulamalar arasındaki değişimi | Microsoft Docs
description: Bu makalede, Azure Event Hubs kullanarak olay tüketicileri ve farklı protokoller (AMQP, Apache Kafka ve HTTPS) kullanan üreticileri nasıl değiştirebilir gösterilmektedir.
services: event-hubs
documentationcenter: ''
author: basilhariri
manager: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/06/2018
ms.author: bahariri
ms.openlocfilehash: e704a2595130a2a815388447ac482ab96789d64a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60821779"
---
# <a name="exchange-events-between-consumers-and-producers-that-use-different-protocols-amqp-kafka-and-https"></a>Tüketiciler ve farklı protokoller kullanan üreticileri arasındaki Exchange olayları: AMQP, Kafka ve HTTPS
Azure Event Hubs Tüketicileri ve üreticileri için üç protokolden destekler: AMQP, Kafka ve HTTPS. Her biri bu protokolleri, bir ileti, bu nedenle doğal olarak aşağıdaki soruyu ortaya temsil eden kendi yolu vardır: bir uygulama bir protokol olan olay Hub'ına olayları gönderir ve bunları farklı bir protokol kullanır, çeşitli bölümlerini ve değerlerini ne yapması Olay aramak gibi tüketici ulaştığında? Bu makalede, üretici ve tüketici olaya içindeki değerleri kullanan uygulama tarafından doğru şekilde yorumlandığından emin olmak için en iyi uygulamalar açıklanmaktadır.

Bu makalede öneriler bu istemciler, kod parçacıkları geliştirmede kullanılan listelenen sürümler ile özellikle kapsar:

* Kafka Java istemci (sürümünden 1.1.1 https://www.mvnrepository.com/artifact/org.apache.kafka/kafka-clients)
* Microsoft Azure olay hub'ları istemci için Java (sürümünden 1.1.0 https://github.com/Azure/azure-event-hubs-java)
* .NET (sürümü 2.1.0 için Microsoft Azure Event Hubs istemcisi https://github.com/Azure/azure-event-hubs-dotnet)
* Microsoft Azure Service Bus (sürümünden 5.0.0 https://www.nuget.org/packages/WindowsAzure.ServiceBus)
* HTTPS (yalnızca üreticileri destekler)

Diğer AMQP istemciler biraz daha farklı davranabilir. AMQP iyi tanımlanmış tür sistemi vardır, ancak dile özel türleri serileştirmek için ve bu tür sisteminden ayrıntılarını, istemci bir AMQP iletisi bölümlerine erişimi nasıl sağladığını gibi istemcide bağlıdır.

## <a name="event-body"></a>Olay gövdesi
Tüm Microsoft AMQP istemciler olay gövdesinde bayt Yorumlanmamış bir paketi temsil eder. Bir üretim uygulaması bayt dizisi istemciye geçirir ve kullanıcı bir uygulama, istemciden o aynı dizi alır. Bayt dizisi yorumunu uygulama kodu içinde gerçekleşir.

HTTPS üzerinden bir olayı gönderirken, olay aynı zamanda Yorumlanmamış bayt olarak kabul edilir deftere nakledilen içeriği gövdesidir. Kafka üretici veya tüketici aynı durumda sağlanan ByteArraySerializer ve aşağıdaki kodda gösterildiği gibi ByteArrayDeserializer kullanarak elde etmek daha kolaydır:

### <a name="kafka-byte-producer"></a>Kafka bayt [] üretici

```java
final Properties properties = new Properties();
// add other properties
properties.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, ByteArraySerializer.class.getName());

final KafkaProducer<Long, byte[]> producer = new KafkaProducer<Long, byte[]>(properties);

final byte[] eventBody = new byte[] { 0x01, 0x02, 0x03, 0x04 };
ProducerRecord<Long, byte[]> pr =
    new ProducerRecord<Long, byte[]>(myTopic, myPartitionId, myTimeStamp, eventBody);
```

### <a name="kafka-byte-consumer"></a>Kafka bayt [] tüketici
```java
final Properties properties = new Properties();
// add other properties
properties.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, ByteArrayDeserializer.class.getName());

final KafkaConsumer<Long, byte[]> consumer = new KafkaConsumer<Long, byte[]>(properties);

ConsumerRecord<Long, byte[]> cr = /* receive event */
// cr.value() is a byte[] with values { 0x01, 0x02, 0x03, 0x04 }
```

Bu kod uygulamanın iki yarısı arasında saydam bayt işlem hattı oluşturur ve el ile seri hale getrime ve seri durumundan çıkarma kararların çalışma zamanında dahil olmak üzere istenen, herhangi bir şekilde uygulama geliştiricinin sağlar örneğin türüne göre veya olay Gönderen bilgisi kullanıcı kümesi özellikleri.

Tek bir uygulama, sabit olay gövde türü şeffaf bir şekilde verileri dönüştürmek için diğer Kafka seri hale getiricileri genişletme ve deserializers kullanmanız mümkün olabilir. Örneğin, JSON kullanan bir uygulama düşünün. Yapım ve JSON dizesi yorumu, uygulama düzeyinde gerçekleşir. Event hubs'ı düzeyinde olay gövdesi her zaman bir UTF-8 kodlaması karakterleri temsil eden bir bayt dizisi dizesidir. Bu durumda, Kafka üretici ve tüketici sağlanan StringSerializer veya StringDeserializer aşağıdaki kodda gösterildiği gibi yararlanabilirsiniz:

### <a name="kafka-utf-8-string-producer"></a>Kafka UTF-8 dize üretici
```java
final Properties properties = new Properties();
// add other properties
properties.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());

final KafkaProducer<Long, String> producer = new KafkaProducer<Long, String>(properties);

final String exampleJson = "{\"name\":\"John\", \"number\":9001}";
ProducerRecord<Long, String> pr =
    new ProducerRecord<Long, String>(myTopic, myPartitionId, myTimeStamp, exampleJson);
```

### <a name="kafka-utf-8-string-consumer"></a>Kafka UTF-8 dize tüketici
```java
final Properties properties = new Properties();
// add other properties
properties.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());

final KafkaConsumer<Long, String> consumer = new KafkaConsumer<Long, String>(properties);

ConsumerRecord<Long, Bytes> cr = /* receive event */
final String receivedJson = cr.value();
```

AMQP tarafı için hem Java hem de .NET gelen UTF-8 bayt dizileri ve dizeleri dönüştürmek için yerleşik yollar sağlar. Microsoft AMQP istemcileri olayları EventData adlı bir sınıf olarak temsil eder. Aşağıdaki örnekler bir UTF-8 dize bir AMQP üretici EventData olay gövdesine seri hale getirmek nasıl ve bir AMQP tüketici UTF-8 dizeye bir EventData olay gövdesi seri durumdan çıkarılacak nasıl gösterir.

### <a name="java-amqp-utf-8-string-producer"></a>Java AMQP UTF-8 dize üretici
```java
final String exampleJson = "{\"name\":\"John\", \"number\":9001}";
final EventData ed = EventData.create(exampleJson.getBytes(StandardCharsets.UTF_8));
```

### <a name="java-amqp-utf-8-string-consumer"></a>Java AMQP UTF-8 dize tüketici
```java
EventData ed = /* receive event */
String receivedJson = new String(ed.getBytes(), StandardCharsets.UTF_8);
```

### <a name="c-net-utf-8-string-producer"></a>C#.NET UTF-8 dize üretici
```csharp
string exampleJson = "{\"name\":\"John\", \"number\":9001}";
EventData working = new EventData(Encoding.UTF8.GetBytes(exampleJson));
```

### <a name="c-net-utf-8-string-consumer"></a>C#.NET UTF-8 dize tüketici
```csharp
EventData ed = /* receive event */

// getting the event body bytes depends on which .NET client is used
byte[] bodyBytes = ed.Body.Array;  // Microsoft Azure Event Hubs Client for .NET
// byte[] bodyBytes = ed.GetBytes(); // Microsoft Azure Service Bus

string receivedJson = Encoding.UTF8.GetString(bodyBytes);
```

Kafka, açık kaynaklı olduğundan, uygulama geliştiricisi herhangi bir serileştirici uygulamasını inceleyin veya deserializer ve oluşturur veya uyumlu bir AMQP tarafında bayt dizisini tüketir, kodu.

## <a name="event-user-properties"></a>Olay kullanıcı özellikleri

Kullanıcı tarafından ayarlanmış özellikleri ayarlayın ve her iki AMQP istemcilerden alınan (Microsoft AMQP istemcileri oldukları özellik olarak adlandırılır) ve Kafka (nerede bunlar çağrılır üst bilgiler). HTTPS göndericiler, olaya HTTP üstbilgileri POST işlemi olarak sağlayarak kullanıcı özellikleri ayarlayabilirsiniz. Ancak, Kafka olay gövdeleri hem olay üstbilgi değerlerini bayt dizileri değerlendirir. AMQP istemcileri iletildiği AMQP tür sistemine göre özellik değerlerini kodlayarak türleri, özellik değerlerine sahip ise.

HTTPS özel bir durumdur. Tüm özellik değerlerini gönderme noktasında UTF-8 metin olan. Event Hubs hizmeti yorumu AMQP kodlu 32-bit ve 64-bit imzalı tamsayı, 64-bit kayan noktalı sayılar ve Boole değerlerini için uygun özellik değerlerini dönüştürmek için sınırlı sayıda yapar. Bu türlerinden birini uymayan bir özellik değeri bir dize olarak kabul edilir.

Özellik yazmak için bu yaklaşımları karıştırma Kafka tüketicisi'nin AMQP tür bilgileri de dahil olmak üzere ham AMQP kodlamalı bayt dizisi dilediği zaman isteyebilmesi anlamına gelir. AMQP tüketici uygulama yorumlama gerekir Kafka üretici tarafından gönderilen yazılmamış bayt dizisini görür ancak.

AMQP veya HTTPS üreticilerden özellikleri alma Kafka tüketicileri için Kafka ekosistemindeki diğer deserializers modellenmiştir AmqpDeserializer sınıfı kullanın. Bu tür bilgiler, veri baytı bir Java türü seri durumdan çıkarılacak AMQP kodlamalı bayt dizileri yorumlar.

En iyi uygulama, AMQP veya HTTPS üzerinden gönderilen iletileri bir özelliği dahil öneririz. Kafka tüketicisi üstbilgi değerlerini AMQP seri durumundan çıkarma gerekip gerekmediğini belirlemek için bunu kullanabilirsiniz. Özelliğinin değeri önemli değildir. Yalnızca, bilinen bir ad Kafka tüketicisi üstbilgileri listesinde bulun ve uygulamanın davranışını uygun şekilde ayarlamanız gerekir.

### <a name="amqp-to-kafka-part-1-create-and-send-an-event-in-c-net-with-properties"></a>AMQP Kafka bölümü 1: oluşturma ve bir olay gönderebilir C# (.NET) özelliklere sahip
```csharp
// Create an event with properties "MyStringProperty" and "MyIntegerProperty"
EventData working = new EventData(Encoding.UTF8.GetBytes("an event body"));
working.Properties.Add("MyStringProperty", "hello");
working.Properties.Add("MyIntegerProperty", 1234);

// BEST PRACTICE: include a property which indicates that properties will need AMQP deserialization
working.Properties.Add("AMQPheaders", 0);
```

### <a name="amqp-to-kafka-part-2-use-amqpdeserializer-to-deserialize-those-properties-in-a-kafka-consumer"></a>AMQP Kafka bölümü 2: Bu bir Kafka tüketicisi özelliklerinde seri durumdan çıkarılacak AmqpDeserializer kullanın
```java
final AmqpDeserializer amqpDeser = new AmqpDeserializer();

ConsumerRecord<Long, Bytes> cr = /* receive event */
final Header[] headers = cr.headers().toArray();

final Header headerNamedMyStringProperty = /* find header with key "MyStringProperty" */
final Header headerNamedMyIntegerProperty = /* find header with key "MyIntegerProperty" */
final Header headerNamedAMQPheaders = /* find header with key "AMQPheaders", or null if not found */

// BEST PRACTICE: detect whether AMQP deserialization is needed
if (headerNamedAMQPheaders != null) {
    // The deserialize() method requires no prior knowledge of a property's type.
    // It returns Object and the application can check the type and perform a cast later.
    Object propertyOfUnknownType = amqpDeser.deserialize("topicname", headerNamedMyStringProperty.value());
    if (propertyOfUnknownType instanceof String) {
        final String propertyString = (String)propertyOfUnknownType;
        // do work here
    }
    propertyOfUnknownType = amqpDeser.deserialize("topicname", headerNamedMyIntegerProperty.value());
    if (propertyOfUnknownType instanceof Integer) {
        final Integer propertyInt = (Integer)propertyOfUnknownType;
        // do work here
    }
} else {
    /* event sent via Kafka, interpret header values the Kafka way */
}
```

Uygulama özelliği için beklenen tür bilir, daha sonra bir tür dönüştürme gerektirmeyen seri halden çıkarma yöntemleri vardır, ancak özelliği beklenen türde değilse bir hata oluştururlar.

### <a name="amqp-to-kafka-part-3-a-different-way-of-using-amqpdeserializer-in-a-kafka-consumer"></a>AMQP için Kafka bölüm 3: bir Kafka tüketicisi AmqpDeserializer kullanarak farklı bir yol
```java
// BEST PRACTICE: detect whether AMQP deserialization is needed
if (headerNamedAMQPheaders != null) {
    // Property "MyStringProperty" is expected to be of type string.
    try {
        final String propertyString = amqpDeser.deserializeString(headerNamedMyStringProperty.value());
        // do work here
    }
    catch (IllegalArgumentException e) {
        // property was not a string
    }

    // Property "MyIntegerProperty" is expected to be a signed integer type.
    // The method returns long because long can represent the value range of all AMQP signed integer types.
    try {
        final long propertyLong = amqpDeser.deserializeSignedInteger(headerNamedMyIntegerProperty.value());
        // do work here
    }
    catch (IllegalArgumentException e) {
        // property was not a signed integer
    }
} else {
    /* event sent via Kafka, interpret header values the Kafka way */
}
```

Ve diğer yöndeki giderek daha eklendiyse, üst bilgilerini ayarla bir Kafka üretici tarafından her zaman bir AMQP tüketici tarafından ham bayt olarak gördükleri (Java için Microsoft Azure Event Hubs istemcisi için org.apache.qpid.proton.amqp.Binary yazın veya `System.Byte[]` Microsoft'un için .NET AMQP istemciler). Kolay Kafka tarafından sağlanan serileştiricileri birini Kafka üretici tarafında üstbilgi değerleri için bayt oluşturun ve ardından AMQP tüketici tarafında uyumlu seri durumundan çıkarma kod yazmak için kullanılacak yoludur.

AMQP-için-Kafka ile gibi Kafka ile gönderilen iletileri bir özellik eklemek için önerdiğimiz en iyi yöntem olacaktır. AMQP tüketici özelliği üstbilgi değerlerini seri durumundan çıkarma gerekip gerekmediğini belirlemek için kullanabilirsiniz. Özelliğinin değeri önemli değildir. Yalnızca, bilinen bir ad AMQP tüketici üstbilgileri listesinde bulun ve uygulamanın davranışını uygun şekilde ayarlamanız gerekir. Kafka üretici değiştirilemez, ayrıca seri durumundan çıkarma türüne göre özellik değeri ikili veya bayt türü olup olmadığını denetleyin ve kullanıcı uygulamanın mümkündür.

### <a name="kafka-to-amqp-part-1-create-and-send-an-event-from-kafka-with-properties"></a>Kafka için AMQP 1. Bölüm: oluşturma ve bir olay Kafka'dan özellikleri ile gönderme
```java
final String topicName = /* topic name */
final ProducerRecord<Long, String> pr = new ProducerRecord<Long, String>(topicName, /* other arguments */);
final Headers h = pr.headers();

// Set headers using Kafka serializers
IntegerSerializer intSer = new IntegerSerializer();
h.add("MyIntegerProperty", intSer.serialize(topicName, 1234));

LongSerializer longSer = new LongSerializer();
h.add("MyLongProperty", longSer.serialize(topicName, 5555555555L));

ShortSerializer shortSer = new ShortSerializer();
h.add("MyShortProperty", shortSer.serialize(topicName, (short)22222));

FloatSerializer floatSer = new FloatSerializer();
h.add("MyFloatProperty", floatSer.serialize(topicName, 1.125F));

DoubleSerializer doubleSer = new DoubleSerializer();
h.add("MyDoubleProperty", doubleSer.serialize(topicName, Double.MAX_VALUE));

StringSerializer stringSer = new StringSerializer();
h.add("MyStringProperty", stringSer.serialize(topicName, "hello world"));

// BEST PRACTICE: include a property which indicates that properties will need deserialization
h.add("RawHeaders", intSer.serialize(0));
```

### <a name="kafka-to-amqp-part-2-manually-deserialize-those-properties-in-c-net"></a>Kafka için AMQP bölüm 2: el ile bu özellikleri seri durumdan C# (.NET)
```csharp
EventData ed = /* receive event */

// BEST PRACTICE: detect whether manual deserialization is needed
if (ed.Properties.ContainsKey("RawHeaders"))
{
    // Kafka serializers send bytes in big-endian order, whereas .NET on x86/x64 is little-endian.
    // Therefore it is frequently necessary to reverse the bytes before further deserialization.

    byte[] rawbytes = ed.Properties["MyIntegerProperty"] as System.Byte[];
    if (BitConverter.IsLittleEndian)
    {
            Array.Reverse(rawbytes);
    }
    int myIntegerProperty = BitConverter.ToInt32(rawbytes, 0);

    rawbytes = ed.Properties["MyLongProperty"] as System.Byte[];
    if (BitConverter.IsLittleEndian)
    {
            Array.Reverse(rawbytes);
    }
    long myLongProperty = BitConverter.ToInt64(rawbytes, 0);

    rawbytes = ed.Properties["MyShortProperty"] as System.Byte[];
    if (BitConverter.IsLittleEndian)
    {
            Array.Reverse(rawbytes);
    }
    short myShortProperty = BitConverter.ToInt16(rawbytes, 0);

    rawbytes = ed.Properties["MyFloatProperty"] as System.Byte[];
    if (BitConverter.IsLittleEndian)
    {
            Array.Reverse(rawbytes);
    }
    float myFloatProperty = BitConverter.ToSingle(rawbytes, 0);

    rawbytes = ed.Properties["MyDoubleProperty"] as System.Byte[];
    if (BitConverter.IsLittleEndian)
    {
            Array.Reverse(rawbytes);
    }
    double myDoubleProperty = BitConverter.ToDouble(rawbytes, 0);

    rawbytes = ed.Properties["MyStringProperty"] as System.Byte[];
string myStringProperty = Encoding.UTF8.GetString(rawbytes);
}
```

### <a name="kafka-to-amqp-part-3-manually-deserialize-those-properties-in-java"></a>Kafka için AMQP bölüm 3: el ile Java'da özelliklere seri durumdan
```java
final EventData ed = /* receive event */

// BEST PRACTICE: detect whether manual deserialization is needed
if (ed.getProperties().containsKey("RawHeaders")) {
    byte[] rawbytes =
        ((org.apache.qpid.proton.amqp.Binary)ed.getProperties().get("MyIntegerProperty")).getArray();
    int myIntegerProperty = 0;
    for (byte b : rawbytes) {
        myIntegerProperty <<= 8;
        myIntegerProperty |= ((int)b & 0x00FF);
    }

    rawbytes = ((org.apache.qpid.proton.amqp.Binary)ed.getProperties().get("MyLongProperty")).getArray();
    long myLongProperty = 0;
    for (byte b : rawbytes) {
        myLongProperty <<= 8;
        myLongProperty |= ((long)b & 0x00FF);
    }

    rawbytes = ((org.apache.qpid.proton.amqp.Binary)ed.getProperties().get("MyShortProperty")).getArray();
    short myShortProperty = (short)rawbytes[0];
    myShortProperty <<= 8;
    myShortProperty |= ((short)rawbytes[1] & 0x00FF);

    rawbytes = ((org.apache.qpid.proton.amqp.Binary)ed.getProperties().get("MyFloatProperty")).getArray();
    int intbits = 0;
    for (byte b : rawbytes) {
        intbits <<= 8;
        intbits |= ((int)b & 0x00FF);
    }
    float myFloatProperty = Float.intBitsToFloat(intbits);

    rawbytes = ((org.apache.qpid.proton.amqp.Binary)ed.getProperties().get("MyDoubleProperty")).getArray();
    long longbits = 0;
    for (byte b : rawbytes) {
        longbits <<= 8;
        longbits |= ((long)b & 0x00FF);
    }
    double myDoubleProperty = Double.longBitsToDouble(longbits);

    rawbytes = ((org.apache.qpid.proton.amqp.Binary)ed.getProperties().get("MyStringProperty")).getArray();
String myStringProperty = new String(rawbytes, StandardCharsets.UTF_8);
}
```

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede protokol istemcilerinizi değiştirmenize veya kendi kümelerinizi çalıştırmanıza gerek kalmadan Kafka etkin Event Hubs’a nasıl akış oluşturacağınızı öğrendiniz. Kafka için Event Hubs ile Event Hubs hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:  

* [Event Hubs hakkında bilgi edinin](event-hubs-what-is-event-hubs.md)
* [Kafka için Event Hubs hakkında bilgi edinin](event-hubs-for-kafka-ecosystem-overview.md)
* [Kafka için Event Hubs GitHub'ındaki diğer örnekleri keşfedin](https://github.com/Azure/azure-event-hubs-for-kafka)
* Kullanım [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) için [kafka'ya şirket içi kafka'dan akış olayları Event Hubs bulut üzerinde etkin.](event-hubs-kafka-mirror-maker-tutorial.md)
* Kafka akışı yapmayı öğrenin etkin Event Hubs kullanarak [yerel Kafka uygulamalar](event-hubs-quickstart-kafka-enabled-event-hubs.md), [Apache Flink](event-hubs-kafka-flink-tutorial.md), veya [Akka akışları](event-hubs-kafka-akka-streams-tutorial.md)
