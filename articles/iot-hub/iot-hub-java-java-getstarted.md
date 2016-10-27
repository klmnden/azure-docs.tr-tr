<properties
    pageTitle="Java için Azure IoT Hub ile çalışmaya başlama | Microsoft Azure"
    description="Java ile Azure IoT Hub'ı kullanmaya başlama öğreticisi. Bir Nesnelerin İnterneti çözümü uygulamak için Microsoft Azure IoT SDK'ları ile Azure IoT Hub ve Java kullanın."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/11/2016"
     ms.author="dobett"/>


# Java için Azure IoT Hub ile çalışmaya başlama

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Bu öğreticinin sonunda üç Java konsol uygulamanız olur:

* Bir cihaz kimliği ve sanal cihazınızı bağlamak için ilişkili güvenlik anahtarı oluşturan **create-device-identity**.
* Sanal cihazınız tarafından gönderilen telemetriyi görüntüleyen **read-d2c-messages**.
* Daha önce oluşturulan cihaz kimliğiyle IoT hub'ınızı bağlayan ve AMQPS protokolünü kullanarak her saniye bir telemetri iletisi gönderen **simulated-device**.

> [AZURE.NOTE] [IoT Hub SDK'ları][lnk-hub-sdks] makalesi, cihazlarda çalıştırmak için her iki uygulamayı da derlemek amacıyla kullanabileceğiniz çeşitli SDK'lar ve çözüm arka ucunuz hakkında bilgi sağlar.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

+ Java SE 8. <br/> [Geliştirme ortamınızı hazırlama][lnk-dev-setup], bu öğretici için Java'nın Windows veya Linux'ta nasıl yükleneceğini açıklar.

+ Maven 3.  <br/> [Geliştirme ortamınızı hazırlama][lnk-dev-setup], bu öğretici için Maven'ın Windows veya Linux'ta nasıl yükleneceğini açıklar.

+ Etkin bir Azure hesabı. (Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü][lnk-free-trial].)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Son adım olarak **Birincil anahtar** değerini not edin ve ardından **Mesajlaşma**’ya tıklayın. **Mesajlaşma** dikey penceresinde **Event Hub ile uyumlu adı** ve **Event Hub ile uyumlu uç noktasını** not edin. **read-d2c-messages** uygulamanızı oluştururken bu üç değere sahip olmanız gerekir.

![Azure portalı IoT Hub Mesajlaşma dikey penceresi][6]

Artık IoT hub'ınızı oluşturdunuz ve bu öğreticiyi tamamlamak için ihtiyacınız olan IoT Hub konak adına, IoT Hub bağlantı dizesine, IoT Hub Birincil Anahtarına, Event Hubs ile uyumlu ada ve Event Hubs ile uyumlu uç noktasına sahipsiniz.

## Cihaz kimliği oluşturma

Bu bölümde, IoT hub'ınızdaki kimlik kayıt defterinde yeni bir cihaz kimliği oluşturan bir Java konsol uygulaması oluşturursunuz. Cihaz kimlik kayıt defterinde girişi olmayan bir cihaz IoT hub'ına bağlanamaz. Daha fazla bilgi için [IoT Hub Geliştirici Kılavuzu][lnk-devguide-identity] konumundaki **Cihaz Kimlik Kayıt Defteri** bölümüne bakın. Bu konsol uygulamasını çalıştırdığınızda, cihazınızın IoT Hub'a cihaz-bulut iletileri gönderdiğinde kendisini tanımlamak için kullanabileceği benzersiz bir cihaz kimliği ve anahtarı oluşturulur.

1. iot-java-get-started adlı yeni bir boş klasör oluşturun. Komut isteminizde aşağıdaki komutu kullanarak iot-java-get-started klasöründe **create-device-identity** adlı yeni bir Maven projesi oluşturun. Bunun tek ve uzun bir komut olduğunu unutmayın:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Komut isteminizde yeni create-device-identity klasörüne gidin.

3. Bir metin düzenleyicisi kullanarak create-device-identity klasöründeki pom.xml dosyasını açın ve aşağıdaki bağımlılığı **bağımlılıklar** düğümüne ekleyin. Böylece uygulamanızda iothub-service-sdk paketini kullanmanıza olanak sağlanmış olur.

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.9</version>
    </dependency>
    ```
    
4. pom.xml dosyasını kaydedin ve kapatın.

5. Bir metin düzenleyicisi kullanarak create-device-identity\src\main\java\com\mycompany\app\App.java dosyasını açın.

6. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```
    import com.microsoft.azure.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.iot.service.sdk.Device;
    import com.microsoft.azure.iot.service.sdk.RegistryManager;

    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. **App** sınıfına aşağıdaki sınıf düzeyi değişkenleri ekleyip daha önce not ettiğiniz değerlerle **{yourhubconnectionstring}** değerlerini değiştirin:

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    
    ```
    
8. **main** yönteminin imzasını, aşağıda gösterilen özel durumları içerecek şekilde değiştirin:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```
    
9. Aşağıdaki kodu **main** yönteminin gövdesi olarak ekleyin. Bu kod, IoT Hub kimlik kayıt defterinizde zaten yoksa *javadevice* adlı bir cihaz oluşturur. Ardından, daha sonra ihtiyacınız olacak cihaz kimliğini ve anahtarını görüntüler:

    ```
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }
    System.out.println("Device id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    ```

10. App.java dosyasını kaydedin ve kapatın.

11. Maven kullanarak **create-device-identity** uygulamasını oluşturmak için, create-device-identity klasöründeki komut isteminde aşağıdaki komutu yürütün:

    ```
    mvn clean package -DskipTests
    ```

12. Maven kullanarak **create-device-identity** uygulamasını çalıştırmak için, create-device-identity klasöründeki komut isteminde aşağıdaki komutu yürütün:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. **Cihaz kimliği** ve **Cihaz anahtarını** not edin. IoT Hub'a bir cihaz olarak bağlanan bir uygulama oluşturduğunuzda bunlara ihtiyacınız olacak.

> [AZURE.NOTE] IoT Hub kimlik kayıt defteri, yalnızca hub'a güvenli erişim sağlamak amacıyla cihaz kimliklerini depolar. Güvenlik kimlik bilgileri olarak kullanılmak üzere cihaz kimliklerini ve anahtarlarını ve tek bir cihaza erişimi devre dışı bırakmak için kullanabileceğiniz etkin/devre dışı bayrağını depolar. Uygulamanızın cihaza özgü diğer meta verileri depolaması gerekiyorsa uygulamaya özgü bir depo kullanması gerekir. Daha fazla bilgi için bkz. [IoT Hub Geliştirici Kılavuzu][lnk-devguide-identity].

## Cihazdan buluta iletileri alma

Bu bölümde IoT Hub'dan cihaz-bulut iletilerini okuyan bir Java konsol uygulaması oluşturursunuz. IoT hub'ı, cihaz bulut iletilerini okumanızı sağlamak için [Event Hub][lnk-event-hubs-overview] ile uyumlu bir uç noktasını kullanıma sunar. Sade ve basit bir anlatım gözetildiği için bu öğretici yüksek işleme dağıtımına uygun olmayan temel bir okuyucu oluşturur. [Cihazdan buluta iletileri işleme][lnk-process-d2c-tutorial] öğreticisi, cihazdan buluta iletilerin ölçekli olarak nasıl işleneceğini gösterir. [Event Hubs ile Çalışmaya Başlama][lnk-eventhubs-tutorial] öğreticisi, Event Hubs'dan alınan iletilerin nasıl işleneceği hakkında daha fazla bilgi sağlar; IoT Hub Event Hubs ile uyumlu uç noktalar için geçerlidir.

> [AZURE.NOTE] Cihazdan buluta iletileri okumak için Event Hubs ile uyumlu uç nokta her zaman AMQPS protokolünü kullanır.

1. Komut isteminizde aşağıdaki komutu kullanarak *Bir cihaz kimliği oluşturma* bölümünde oluşturduğunuz iot-java-get-started klasöründe **read-d2c-messages** adlı yeni bir Maven projesi oluşturun. Bunun tek ve uzun bir komut olduğunu unutmayın:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Komut isteminizde yeni read-d2c-messages klasörüne gidin.

3. Bir metin düzenleyicisi kullanarak read-d2c-messages klasöründeki pom.xml dosyasını açın ve aşağıdaki bağımlılığı **bağımlılıklar** düğümüne ekleyin. Böylece Event Hubs ile uyumlu uç noktadan okumak için uygulamanızda eventhubs-client paketini kullanmanıza olanak sağlanır:

    ```
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.8.2</version> 
    </dependency>
    ```

4. pom.xml dosyasını kaydedin ve kapatın.

5. Bir metin düzenleyicisi kullanarak read-d2c-messages\src\main\java\com\mycompany\app\App.java dosyasını açın.

6. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;
    
    import java.io.IOException;
    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.Collection;
    import java.util.concurrent.ExecutionException;
    import java.util.function.*;
    import java.util.logging.*;
    ```

7. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. **{youriothubkey}**, **{youreventhubcompatibleendpoint}** ve **{youreventhubcompatiblename}** değerlerini daha önce not ettiğiniz değerlerle değiştirin:

    ```
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. Aşağıdaki **receiveMessages** yöntemini **App** sınıfına ekleyin. Bu yöntem, Event Hubs ile uyumlu uç noktasını bağlamak için bir **EventHubClient** örneği oluşturur ve ardından Event Hub bölümünden okumak için uyumsuz şekilde bir **PartitionReceiver** oluşturur. Uygulama sonlanana kadar sürekli döngüye girer ve ileti ayrıntılarını yazdırır.

    ```
    private static EventHubClient receiveMessages(final String partitionId)
    {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      }
      catch(Exception e) {
        System.out.println("Failed to create client: " + e.getMessage());
        System.exit(1);
      }
      try {
        client.createReceiver( 
          EventHubClient.DEFAULT_CONSUMER_GROUP_NAME,  
          partitionId,  
          Instant.now()).thenAccept(new Consumer<PartitionReceiver>()
        {
          public void accept(PartitionReceiver receiver)
          {
            System.out.println("** Created receiver on partition " + partitionId);
            try {
              while (true) {
                Iterable<EventData> receivedEvents = receiver.receive(100).get();
                int batchSize = 0;
                if (receivedEvents != null)
                {
                  for(EventData receivedEvent: receivedEvents)
                  {
                    System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s", 
                      receivedEvent.getSystemProperties().getOffset(), 
                      receivedEvent.getSystemProperties().getSequenceNumber(), 
                      receivedEvent.getSystemProperties().getEnqueuedTime()));
                    System.out.println(String.format("| Device ID: %s", receivedEvent.getProperties().get("iothub-connection-device-id")));
                    System.out.println(String.format("| Message Payload: %s", new String(receivedEvent.getBody(),
                      Charset.defaultCharset())));
                    batchSize++;
                  }
                }
                System.out.println(String.format("Partition: %s, ReceivedBatch Size: %s", partitionId,batchSize));
              }
            }
            catch (Exception e)
            {
              System.out.println("Failed to receive messages: " + e.getMessage());
            }
          }
        });
      }
      catch (Exception e)
      {
        System.out.println("Failed to create receiver: " + e.getMessage());
      }
      return client;
    }
    ```

    > [AZURE.NOTE] Bu yöntem alıcı oluştururken bir filtre kullanır, böylece alıcı çalışmaya başladıktan sonra IoT Hub'a gönderilen iletileri yalnızca okur. Bu, geçerli ileti kümesini görebilmeniz açısından bir test ortamında kullanışlıdır. Bir üretim ortamında kodunuzun tüm iletileri işlediğinden emin olmanız gerekir. Daha fazla bilgi için [IoT Hub cihazdan buluta iletileri işleme][lnk-process-d2c-tutorial] öğreticisine bakın.

9. **main** yönteminin imzasını, aşağıda gösterilen özel durumu içerecek şekilde değiştirin:

    ```
    public static void main( String[] args ) throws IOException
    ```

10. Aşağıdaki kodu **App** sınıfındaki **main** yöntemine ekleyin. Bu kod, iki **EventHubClient** ve **PartitionReceiver** örneği oluşturur ve iletileri işlemeyi bitirdiğinizde uygulamayı kapatmanıza olanak sağlar.

    ```
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER to exit.");
    System.in.read();
    try
    {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    }
    catch (ServiceBusException sbe)
    {
      System.exit(1);
    }
    ```

    > [AZURE.NOTE] Bu kod, IoT hub'ınızı F1 (ücretsiz) katmanında oluşturduğunuzu varsayar. Ücretsiz IoT hub'ının "0" ve "1" adlı iki bölümü vardır.

11. App.java dosyasını kaydedin ve kapatın.

12. Maven kullanarak **read-d2c-messages** uygulamasını oluşturmak için, read-d2c-messages klasöründeki komut isteminde aşağıdaki komutu yürütün:

    ```
    mvn clean package -DskipTests
    ```

## Sanal cihaz uygulaması oluşturma

Bu bölümde, IoT Hub'a cihazdan buluta iletiler gönderen bir cihaza benzetim yapan bir Java konsol uygulaması oluşturacaksınız.

1. Komut isteminizde aşağıdaki komutu kullanarak *Bir cihaz kimliği oluşturma* bölümünde oluşturduğunuz iot-java-get-started klasöründe **simulated-device** adlı yeni bir Maven projesi oluşturun. Bunun tek ve uzun bir komut olduğunu unutmayın:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Komut isteminizde yeni simulated-device klasörüne gidin.

3. Bir metin düzenleyicisi kullanarak simulated-device klasöründeki pom.xml dosyasını açın ve aşağıdaki bağımlılıkları **bağımlılıklar** düğümüne ekleyin. Böylece IoT hub'ınızla iletişim kurmak ve Java nesnelerini JSON'a seri hale getirmek için uygulamanızda iothub-java-client paketini kullanmanıza olanak sağlanmış olur.

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-device-client</artifactId>
      <version>1.0.14</version>
    </dependency>
    <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.3.1</version>
    </dependency>
    ```

4. pom.xml dosyasını kaydedin ve kapatın.

5. Bir metin düzenleyicisi kullanarak simulated-device\src\main\java\com\mycompany\app\App.java dosyasını açın.

6. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```
    import com.microsoft.azure.iothub.DeviceClient;
    import com.microsoft.azure.iothub.IotHubClientProtocol;
    import com.microsoft.azure.iothub.Message;
    import com.microsoft.azure.iothub.IotHubStatusCode;
    import com.microsoft.azure.iothub.IotHubEventCallback;
    import com.microsoft.azure.iothub.IotHubMessageResult;
    import com.google.gson.Gson;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyip **{youriothubname}** değerini IoT hub'ınızın adıyla ve **{yourdevicekey}** değerlerini de *Bir cihaz kimliği oluşturma* bölümünde oluşturduğunuz cihaz anahtarı değerleriyle değiştirin:

    ```
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.AMQPS;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```

    Bu örnek uygulama, bir **DeviceClient** nesnesi örneğini oluşturduğunda **prokotol** değişkenini kullanır. IoT Hub ile iletişim kurmak için HTTPS veya AMQPS protokolünü kullanabilirsiniz.

8. Cihazınızın IoT hub'ınıza gönderdiği telemetri verilerini belirtmek için aşağıdaki iç içe geçmiş **TelemetryDataPoint** sınıfını **App** sınıfının içine ekleyin:

    ```
    private static class TelemetryDataPoint {
      public String deviceId;
      public double windSpeed;
        
      public String serialize() {
        Gson gson = new Gson();
        return gson.toJson(this);
      }
    }
    ```

9. Sanal cihazdan bir ileti işlediğinde IoT hub'ının döndürdüğü onay durumunu görüntülemek için aşağıdaki iç içe geçmiş **EventCallback** sınıfını **App** sınıfına ekleyin. Bu yöntem, ileti işlendiğinde uygulamadaki ana iş parçacığına da bildirimde bulunur.

    ```
    private static class EventCallback implements IotHubEventCallback
    {
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

    ```
    private static class MessageSender implements Runnable {
      public volatile boolean stopThread = false;
      
      public void run()  {
        try {
          double avgWindSpeed = 10; // m/s
          Random rand = new Random();
          
          while (!stopThread) {
            double currentWindSpeed = avgWindSpeed + rand.nextDouble() * 4 - 2;
            TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
            telemetryDataPoint.deviceId = deviceId;
            telemetryDataPoint.windSpeed = currentWindSpeed;
            
            String msgStr = telemetryDataPoint.serialize();
            Message msg = new Message(msgStr);
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

    Bu yöntem, IoT hub'ı bir önceki iletiyi onayladıktan bir saniye sonra yeni bir cihazdan buluta ileti gönderir. İleti; cihaz kimliğine ve bir rüzgar hızı sensörü benzetimi gerçekleştirmeye yönelik rastgele oluşturulmuş bir sayıya sahip bir JSON ile seri hale getirilmiş bir nesneyi içerir.

11. **Main** yöntemi, IoT hub'ınıza cihazdan buluta iletiler göndermek için bir iş parçacığı oluşturan aşağıdaki kodla değiştirin:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();

      MessageSender sender = new MessageSender();

      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);

      System.out.println("Press ENTER to exit.");
      System.in.read();
      executor.shutdownNow();
      client.close();
    }
    ```

12. App.java dosyasını kaydedin ve kapatın.

13. Maven kullanarak **simulated-device** uygulamasını oluşturmak için, simulated-device klasöründeki komut isteminde aşağıdaki komutu yürütün:

    ```
    mvn clean package -DskipTests
    ```

> [AZURE.NOTE] Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. [Geçici Hata İşleme][lnk-transient-faults] adlı MSDN makalesinde önerildiği üzere, üretim kodunda yeniden deneme ilkelerini (üstel geri alma gibi) uygulamanız gerekir.

## Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. read-d2c klasöründeki bir komut isteminde IoT hub'ınızdaki ilk bölümü izlemeye başlamak için aşağıdaki komutu çalıştırın:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Cihazdan buluta iletileri izlemeye yönelik Java IoT Hub hizmeti istemci uygulaması][7]

2. simulated-device klasöründeki bir komut isteminde IoT hub'ınıza telemetri verileri göndermeye başlamak için aşağıdaki komutu çalıştırın:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Cihazdan buluta iletileri göndermeye yönelik Java IoT Hub cihazı istemci uygulaması][8]

3. [Azure portalındaki][lnk-portal] **Kullanım** kutucuğu, hub'a gönderilen ileti sayısını gösterir:

    ![IoT Hub’a gönderilen ileti sayısını gösteren Azure portalı Kullanım kutucuğu][43]

## Sonraki adımlar

Bu öğreticide, portalda yeni bir IoT hub'ı yapılandırdınız ve ardından hub'ın kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Bu cihaz kimliğini, sanal cihaz uygulamasının hub'a cihaz-bulut iletileri göndermesini sağlamak için kullandınız. Hub tarafından alınan iletileri görüntüleyen bir uygulama da oluşturdunuz. 

IoT Hub’ı kullanmaya başlamak ve diğer IoT senaryolarını keşfetmek için bkz:

- [Cihazınızı bağlama][lnk-connect-device]
- [Cihaz yönetimi ile çalışmaya başlama][lnk-device-management]
- [Ağ Geçidi SDK’sı ile çalışmaya başlama][lnk-gateway-SDK]

IoT çözümünüzün nasıl genişletileceğini ve cihazdan buluta iletilerin doğru ölçekte nasıl işleneceğini öğrenmek için [Cihazdan buluta iletileri işleme][lnk-process-d2c-tutorial] öğreticisine bakın.

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

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/


<!--HONumber=Oct16_HO3-->


