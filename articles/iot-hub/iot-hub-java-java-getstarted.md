---
title: "Azure IoT Hub'ı (Java) kullanmaya başlama | Microsoft Belgeleri"
description: "Java için Azure IoT SDK'larını kullanarak bir cihazdan bir Azure IoT hub'ına cihazdan buluta iletiler gönderme. İleti göndermek için bir sanal cihaz uygulaması, cihazınızı kimlik kayıt defterine kaydetmek için bir hizmet uygulaması ve cihazdan buluta gönderilen iletileri IoT hub'ından okumak için bir hizmet uygulaması oluşturmanız gerekir."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 70dae4a8-0e98-4c53-b5a5-9d6963abb245
ms.service: iot-hub
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 7b44762ffea876d628886192376b6275bbc0b83b
ms.contentlocale: tr-tr
ms.lasthandoff: 07/04/2017


---
<a id="connect-your-simulated-device-to-your-iot-hub-using-java" class="xliff"></a>

# Java kullanarak sanal cihazınızı IoT hub’ınıza bağlama
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Bu öğreticinin sonunda üç Java konsol uygulamanız olur:

* Bir cihaz kimliği ve sanal cihaz uygulamanızı bağlamak için ilişkili güvenlik anahtarı oluşturan **create-device-identity**.
* Sanal cihaz uygulamanız tarafından gönderilen telemetriyi görüntüleyen **read-d2c-messages**.
* Daha önce oluşturulan cihaz kimliğiyle IoT hub'ınıza bağlanan ve MQTT protokolünü kullanarak her saniye bir telemetri iletisi gönderen **simulated-device**.

> [!NOTE]
> [IoT Hub SDK'ları][lnk-hub-sdks] makalesi, hem cihazlarınızda hem de çözüm arka ucunuzda çalıştırılacak uygulamalar oluşturmak için kullanabileceğiniz Azure IoT SDK’ları hakkında bilgi içerir.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* En güncel [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 
* [Maven 3](https://maven.apache.org/install.html) 
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Son adım olarak **Birincil anahtar** değerini not edin. Ardından **Uç noktaları**'na ve **Olaylar** yerleşik uç noktasına tıklayın. **Özellikler** dikey penceresinde **Event Hub ile uyumlu adı** ve **Event Hub ile uyumlu uç noktası** adresini not edin. **read-d2c-messages** uygulamanızı oluştururken bu üç değere sahip olmanız gerekir.

![Azure portalı IoT Hub Mesajlaşma dikey penceresi][6]

IoT Hub’ınızı oluşturdunuz. Bu öğreticiyi tamamlamak için ihtiyacınız olan IoT Hub konak adına, IoT Hub bağlantı dizesine, IoT Hub Birincil Anahtarına, Olay Hub'ı ile uyumlu ada ve Olay Hub'ı ile uyumlu uç noktasına sahipsiniz.

<a id="create-a-device-identity" class="xliff"></a>

## Cihaz kimliği oluşturma
Bu bölümde, IoT hub'ınızdaki kimlik kayıt defterinde cihaz kimliği oluşturan bir Java konsol uygulaması oluşturursunuz. Kimlik kayıt defterinde girişi olmayan bir cihaz IoT hub'ına bağlanamaz. Daha fazla bilgi için [IoT Hub Geliştirici Kılavuzu][lnk-devguide-identity]'nun **Kimlik Kayıt Defteri** bölümüne bakın. Bu konsol uygulamasını çalıştırdığınızda, cihazınızın IoT Hub'a cihaz-bulut iletileri gönderdiğinde kendisini tanımlamak için kullanabileceği benzersiz bir cihaz kimliği ve anahtarı oluşturulur.

1. iot-java-get-started adlı bir boş klasör oluşturun. Komut isteminizde aşağıdaki komutu kullanarak iot-java-get-started klasöründe **create-device-identity** adlı bir Maven projesi oluşturun. Bunun tek ve uzun bir komut olduğunu unutmayın:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Komut isteminizde create-device-identity klasörüne gidin.

3. Bir metin düzenleyicisi kullanarak create-device-identity klasöründeki pom.xml dosyasını açın ve aşağıdaki bağımlılığı **bağımlılıklar** düğümüne ekleyin. Bu bağımlılık, uygulamanızda iot-service-client paketini kullanmanıza olanak sağlar:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.5.22</version>
    </dependency>
    ```

    > [!NOTE]
    > [Maven arama][lnk-maven-service-search] kullanarak en yeni **iot-service-client** sürümünü kontrol edebilirsiniz.

4. pom.xml dosyasını kaydedin ve kapatın.

5. Bir metin düzenleyicisi kullanarak create-device-identity\src\main\java\com\mycompany\app\App.java dosyasını açın.

6. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```java
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.Device;
    import com.microsoft.azure.sdk.iot.service.RegistryManager;
   
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. **App** sınıfına aşağıdaki sınıf düzeyi değişkenleri ekleyip daha önce not ettiğiniz değerlerle **{yourhubconnectionstring}** değerlerini değiştirin:

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    ```

8. **main** yönteminin imzasını, aşağıda gösterilen özel durumları içerecek şekilde değiştirin:

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```

9. Aşağıdaki kodu **main** yönteminin gövdesi olarak ekleyin. Bu kod, IoT Hub kimlik kayıt defterinizde zaten yoksa *javadevice* adlı bir cihaz oluşturur. Ardından, daha sonra ihtiyaç duyacağınız cihaz kimliğini ve anahtarını görüntüler:

    ```java
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);
RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    // Create a device that's enabled by default, 
    // with an autogenerated key.
    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      // If the device already exists.
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }

    // Display information about the
    // device you created.
    System.out.println("Device Id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    // Create a device that's enabled by default, 
    // with an autogenerated key.
    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      // If the device already exists.
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }

    // Display information about the
    // device you created.
    System.out.println("Device Id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    ```

10. App.java dosyasını kaydedin ve kapatın.

11. Maven kullanarak **create-device-identity** uygulamasını oluşturmak için, create-device-identity klasöründeki komut isteminde aşağıdaki komutu yürütün:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

12. Maven kullanarak **create-device-identity** uygulamasını çalıştırmak için, create-device-identity klasöründeki komut isteminde aşağıdaki komutu yürütün:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. **Cihaz kimliği** ve **Cihaz anahtarını** not edin. İleride IoT Hub'a bir cihaz olarak bağlanan bir uygulama oluşturduğunuzda bu değerlere ihtiyacınız olur.

> [!NOTE]
> IoT Hub kimlik kayıt defteri, yalnızca IoT hub'ına güvenli erişim sağlamak amacıyla cihaz kimliklerini depolar. Güvenlik kimlik bilgileri olarak kullanılmak üzere cihaz kimliklerini ve anahtarlarını ve tek bir cihaza erişimi devre dışı bırakmak için kullanabileceğiniz etkin/devre dışı bayrağını depolar. Uygulamanızın cihaza özgü diğer meta verileri depolaması gerekiyorsa uygulamaya özgü bir depo kullanmalıdır. Daha fazla bilgi için bkz. [IoT Hub geliştirici kılavuzu][lnk-devguide-identity].

<a id="receive-device-to-cloud-messages" class="xliff"></a>

## Cihazdan buluta iletileri alma

Bu bölümde IoT Hub'dan cihaz-bulut iletilerini okuyan bir Java konsol uygulaması oluşturursunuz. IoT hub'ı, cihazdan buluta iletileri okumanızı sağlamak için [Event Hub][lnk-event-hubs-overview] ile uyumlu bir uç noktasını kullanıma sunar. Sade ve basit bir anlatım gözetildiği için bu öğretici yüksek işleme dağıtımına uygun olmayan temel bir okuyucu oluşturur. [Cihazdan buluta iletileri işleme][lnk-process-d2c-tutorial] öğreticisi, cihazdan buluta iletilerin ölçekli olarak nasıl işleneceğini gösterir. [Event Hubs ile Çalışmaya Başlama][lnk-eventhubs-tutorial] öğreticisi, Event Hubs'dan alınan iletilerin nasıl işleneceği hakkında daha fazla bilgi sağlar; IoT Hub ve Event Hub ile uyumlu uç noktalar için geçerlidir.

> [!NOTE]
> Cihazdan buluta iletileri okumak için Event Hub ile uyumlu uç nokta her zaman AMQP protokolünü kullanır.

1. Komut isteminizde aşağıdaki komutu kullanarak *Bir cihaz kimliği oluşturma* bölümünde oluşturduğunuz iot-java-get-started klasöründe **read-d2c-messages** adlı bir Maven projesi oluşturun. Bunun tek ve uzun bir komut olduğunu unutmayın:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Komut isteminizde read-d2c-messages klasörüne gidin.

3. Bir metin düzenleyicisi kullanarak read-d2c-messages klasöründeki pom.xml dosyasını açın ve aşağıdaki bağımlılığı **bağımlılıklar** düğümüne ekleyin. Bu bağımlılık, Event Hub ile uyumlu uç noktasından okumak için uygulamanızda eventhubs-client paketini kullanmanıza olanak sağlar:

    ```java
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.13.0</version> 
    </dependency>
    ```

    > [!NOTE]
    > [Maven arama][lnk-maven-eventhubs-search] kullanarak en yeni **azure-eventhubs** sürümünü kontrol edebilirsiniz.

4. pom.xml dosyasını kaydedin ve kapatın.

5. Bir metin düzenleyicisi kullanarak read-d2c-messages\src\main\java\com\mycompany\app\App.java dosyasını açın.

6. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```java
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;

    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.function.*;
    ```

7. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. **{youriothubkey}**, **{youreventhubcompatibleendpoint}** ve **{youreventhubcompatiblename}** değerlerini daha önce not ettiğiniz değerlerle değiştirin:

    ```java
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. Aşağıdaki **receiveMessages** yöntemini **App** sınıfına ekleyin. Bu yöntem, Event Hub ile uyumlu uç noktasını bağlamak için bir **EventHubClient** örneği oluşturur ve ardından bir Event Hub bölümünden okumak için uyumsuz şekilde bir **PartitionReceiver** oluşturur. Uygulama sonlanana kadar sürekli döngüye girer ve ileti ayrıntılarını yazdırır.

    ```java
    // Create a receiver on a partition.
    private static EventHubClient receiveMessages(final String partitionId) {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      } catch (Exception e) {
        System.out.println("Failed to create client: " + e.getMessage());
        System.exit(1);
      }
      try {
        // Create a receiver using the
        // default Event Hubs consumer group
        // that listens for messages from now on.
        client.createReceiver(EventHubClient.DEFAULT_CONSUMER_GROUP_NAME, partitionId, Instant.now())
          .thenAccept(new Consumer<PartitionReceiver>() {
            public void accept(PartitionReceiver receiver) {
              System.out.println("** Created receiver on partition " + partitionId);
              try {
                while (true) {
                  Iterable<EventData> receivedEvents = receiver.receive(100).get();
                  int batchSize = 0;
                  if (receivedEvents != null) {
                    System.out.println("Got some evenst");
                    for (EventData receivedEvent : receivedEvents) {
                      System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s",
                        receivedEvent.getSystemProperties().getOffset(),
                        receivedEvent.getSystemProperties().getSequenceNumber(),
                        receivedEvent.getSystemProperties().getEnqueuedTime()));
                      System.out.println(String.format("| Device ID: %s",
                        receivedEvent.getSystemProperties().get("iothub-connection-device-id")));
                      System.out.println(String.format("| Message Payload: %s",
                        new String(receivedEvent.getBytes(), Charset.defaultCharset())));
                      batchSize++;
                    }
                  }
                  System.out.println(String.format("Partition: %s, ReceivedBatch Size: %s", partitionId, batchSize));
                }
              } catch (Exception e) {
                System.out.println("Failed to receive messages: " + e.getMessage());
              }
            }
          });
        } catch (Exception e) {
          System.out.println("Failed to create receiver: " + e.getMessage());
      }
      return client;
    }
    ```

   > [!NOTE]
   > Bu yöntem alıcı oluştururken bir filtre kullanır, böylece alıcı çalışmaya başladıktan sonra IoT Hub'a gönderilen iletileri yalnızca okur. Bu teknik, geçerli ileti kümesini görebilmeniz açısından bir test ortamında kullanışlıdır. Bir üretim ortamında kodunuzun tüm iletileri işlediğinden emin olmanız gerekir. Daha fazla bilgi için [IoT Hub cihazdan buluta iletileri işleme][lnk-process-d2c-tutorial] öğreticisine bakın.

9. **main** yönteminin imzasını, aşağıda gösterilen özel durumu içerecek şekilde değiştirin:

    ```java
    public static void main( String[] args ) throws IOException
    ```

10. Aşağıdaki kodu **App** sınıfındaki **main** yöntemine ekleyin. Bu kod, iki **EventHubClient** ve **PartitionReceiver** örneği oluşturur ve iletileri işlemeyi bitirdiğinizde uygulamayı kapatmanıza olanak sağlar:

    ```java
    // Create receivers for partitions 0 and 1.
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER to exit.");
    System.in.read();
    try {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    } catch (ServiceBusException sbe) {
      System.exit(1);
    }
    ```

    > [!NOTE]
    > Bu kod, IoT hub'ınızı F1 (ücretsiz) katmanında oluşturduğunuzu varsayar. Ücretsiz IoT hub'ının "0" ve "1" adlı iki bölümü vardır.

11. App.java dosyasını kaydedin ve kapatın.

12. Maven kullanarak **read-d2c-messages** uygulamasını oluşturmak için, read-d2c-messages klasöründeki komut isteminde aşağıdaki komutu yürütün:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

<a id="create-a-simulated-device-app" class="xliff"></a>

## Sanal cihaz uygulaması oluşturma

Bu bölümde, IoT Hub'a cihazdan buluta iletiler gönderen bir cihaza benzetim yapan bir Java konsol uygulaması oluşturacaksınız.

1. Komut isteminizde aşağıdaki komutu kullanarak *Bir cihaz kimliği oluşturma* bölümünde oluşturduğunuz iot-java-get-started klasöründe **simulated-device** adlı bir Maven projesi oluşturun. Bunun tek ve uzun bir komut olduğunu unutmayın:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Komut isteminizde simulated-device klasörüne gidin.

3. Bir metin düzenleyicisi kullanarak simulated-device klasöründeki pom.xml dosyasını açın ve aşağıdaki bağımlılıkları **bağımlılıklar** düğümüne ekleyin. Bu bağımlılık, IoT hub'ınızla iletişim kurmak ve Java nesnelerini JSON'a seri hale getirmek için uygulamanızda iothub-java-client paketini kullanmanıza olanak sağlar:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.30</version>
    </dependency>
    <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.3.1</version>
    </dependency>
    ```

    > [!NOTE]
    > [Maven arama][lnk-maven-device-search] kullanarak en yeni **iot-device-client** sürümünü kontrol edebilirsiniz.

4. pom.xml dosyasını kaydedin ve kapatın.

5. Bir metin düzenleyicisi kullanarak simulated-device\src\main\java\com\mycompany\app\App.java dosyasını açın.

6. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.google.gson.Gson;

    import java.io.*;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. **{youriothubname}** değerini IoT hub'ınızın adıyla, **{yourdevicekey}** değerini de *Bir cihaz kimliği oluşturma* bölümünde oluşturduğunuz cihaz anahtar değeriyle değiştirin:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```
   
    Bu örnek uygulama, bir **DeviceClient** nesnesi örneğini oluşturduğunda **prokotol** değişkenini kullanır. IoT Hub ile iletişim kurmak için MQTT, AMQP veya HTTP protokolünü kullanabilirsiniz.

8. Cihazınızın IoT hub'ınıza gönderdiği telemetri verilerini belirtmek için aşağıdaki iç içe geçmiş **TelemetryDataPoint** sınıfını **App** sınıfının içine ekleyin:

    ```java
    private static class TelemetryDataPoint {
      public String deviceId;
      public double temperature;
      public double humidity;
   
      public String serialize() {
        Gson gson = new Gson();
        return gson.toJson(this);
      }
    }
    ```

9. Sanal cihaz uygulamasından bir ileti işlediğinde IoT hub'ının döndürdüğü onay durumunu görüntülemek için aşağıdaki iç içe geçmiş **EventCallback** sınıfını **App** sınıfına ekleyin. Bu metot, ileti işlendiğinde uygulamadaki ana iş parçacığına da bildirimde bulunur:

    ```java
    private static class EventCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to message with status: " + status.name());
   
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. Aşağıdaki iç içe geçmiş **MessageSender** sınıfını **App** sınıfına ekleyin. Bu sınıftaki **run** yöntemi, IoT hub'ınıza gönderilecek örnek telemetri verilerini oluşturur ve bir sonraki iletiyi göndermeden önce onay bekler:

    ```java
    private static class MessageSender implements Runnable {
      public void run()  {
        try {
          double minTemperature = 20;
          double minHumidity = 60;
          Random rand = new Random();
    
          while (true) {
            double currentTemperature = minTemperature + rand.nextDouble() * 15;
            double currentHumidity = minHumidity + rand.nextDouble() * 20;
            TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
            telemetryDataPoint.deviceId = deviceId;
            telemetryDataPoint.temperature = currentTemperature;
            telemetryDataPoint.humidity = currentHumidity;
    
            String msgStr = telemetryDataPoint.serialize();
            Message msg = new Message(msgStr);
            msg.setProperty("temperatureAlert", (currentTemperature > 30) ? "true" : "false");
            msg.setMessageId(java.util.UUID.randomUUID().toString()); 
            System.out.println("Sending: " + msgStr);
    
            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);
    
            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(1000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        }
      }
    }
    ```

    Bu yöntem, IoT hub'ı bir önceki iletiyi onayladıktan bir saniye sonra yeni bir cihazdan buluta ileti gönderir. İleti, cihaz kimliğine ve bir sıcaklık sensörüyle nem sensörünün benzetimini gerçekleştirmeye yönelik rastgele oluşturulmuş sayılara sahip bir JSON ile seri hale getirilmiş bir nesneyi içerir.

11. **Main** yöntemi, IoT hub'ınıza cihazdan buluta iletiler göndermek için bir iş parçacığı oluşturan aşağıdaki kodla değiştirin:

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();
    
      MessageSender sender = new MessageSender();
    
      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);
    
      System.out.println("Press ENTER to exit.");
      System.in.read();
      executor.shutdownNow();
      client.closeNow();
    }
    ```

12. App.java dosyasını kaydedin ve kapatın.

13. Maven kullanarak **simulated-device** uygulamasını oluşturmak için, simulated-device klasöründeki komut isteminde aşağıdaki komutu yürütün:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. [Geçici Hata İşleme][lnk-transient-faults] adlı MSDN makalesinde önerildiği üzere, üretim kodunda yeniden deneme ilkelerini (üstel geri alma gibi) uygulamanız gerekir.

<a id="run-the-apps" class="xliff"></a>

## Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. read-d2c klasöründeki bir komut isteminde IoT hub'ınızdaki ilk bölümü izlemeye başlamak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Cihazdan buluta iletileri izlemeye yönelik Java IoT Hub hizmet uygulaması][7]

2. simulated-device klasöründeki bir komut isteminde IoT hub'ınıza telemetri verileri göndermeye başlamak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Cihazdan buluta iletileri göndermeye yönelik Java IoT Hub cihaz uygulaması][8]

3. [Azure portalındaki][lnk-portal] **Kullanım** kutucuğu, IoT hub'ına gönderilen ileti sayısını gösterir:

    ![IoT Hub’a gönderilen ileti sayısını gösteren Azure portalı Kullanım kutucuğu][43]

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar

Bu öğreticide, Azure portalında yeni bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Bu cihaz kimliğini, sanal cihaz uygulamasının, IoT hub'ına cihazdan buluta iletileri göndermesini sağlamak için kullandınız. Ayrıca, IoT hub’ı tarafından alınan iletileri görüntüleyen bir uygulama da oluşturdunuz.

IoT Hub’ı kullanmaya başlamak ve diğer IoT senaryolarını keşfetmek için bkz:

* [Cihazınızı bağlama][lnk-connect-device]
* [Cihaz yönetimini kullanmaya başlama][lnk-device-management]
* [Azure IoT Edge’i kullanmaya başlama][lnk-iot-edge]

IoT çözümünüzün nasıl genişletileceğini ve cihazdan buluta iletilerin doğru ölçekte nasıl işleneceğini öğrenmek için [Cihazdan buluta iletileri işleme][lnk-process-d2c-tutorial] öğreticisine bakın.
[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[6]: ./media/iot-hub-java-java-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-java-java-getstarted/runapp1.png
[8]: ./media/iot-hub-java-java-getstarted/runapp2.png
[43]: ./media/iot-hub-java-java-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-device-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-eventhubs-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22

