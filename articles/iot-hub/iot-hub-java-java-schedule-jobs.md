---
title: Zamanlama işlerini ile Azure IOT hub'ı (Java) | Microsoft Docs
description: Doğrudan bir yöntem çağırma ve birden fazla cihazda istenen özelliği ayarlamak için bir Azure IOT Hub işini zamanlamak nasıl. Sanal cihaz uygulamaları ve Azure IOT hizmeti işi çalıştırmak için bir hizmet uygulaması uygulamak için Java SDK'sını uygulamak için Azure IOT cihaz SDK'sı Java için kullanın.
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: ''
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2017
ms.author: dobett
ms.openlocfilehash: b8b0742054b0348ded39b6357d00f6eac3449f99
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="schedule-and-broadcast-jobs-java"></a>Zamanlama ve yayın işleri (Java)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Zamanlama ve milyonlarca cihaza güncelleştirme işleri izlemek için Azure IOT hub'ı kullanın. İşlerini kullanın:

* İstenen özellikleri güncelleştirme
* Güncelleştirme etiketleri
* Doğrudan yöntemleri çağırma

Bir işi şu eylemlerden birini sarmalar ve bir cihaz kümesi karşı yürütme izler. Cihaz çifti sorgu karşı iş yürütür cihaz kümesini tanımlar. Örneğin, bir arka uç uygulaması, aygıtların yeniden 10.000 cihazlarda doğrudan yöntemini çağırmak için bir iş kullanabilirsiniz. Cihaz kümesini içeren bir cihaz çifti sorgu belirtin ve gelecek bir zamanda çalıştırmak için iş zamanlama. Her aygıt olarak ilerleyişini izler almak ve yeniden başlatma doğrudan yöntemi yürütün.

Bu özelliklerin her biri hakkında daha fazla bilgi için bkz:

* Cihaz çifti ve özellikleri: [cihaz çiftlerini ile çalışmaya başlama](iot-hub-java-java-twin-getstarted.md)
* Doğrudan yöntemleri: [IOT Hub Geliştirici Kılavuzu - doğrudan yöntemleri](iot-hub-devguide-direct-methods.md) ve [Öğreticisi: doğrudan yöntemleri kullanın](iot-hub-java-java-direct-methods.md)

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* Adlı doğrudan bir yöntem uygulayan bir cihaz uygulaması oluşturma **lockDoor**. Cihaz uygulaması Ayrıca, arka uç uygulama istenen özellik değişiklikleri alır.
* Aranacak bir işi oluşturan bir arka uç uygulaması oluşturma **lockDoor** birden fazla cihazda yöntemi doğrudan. Başka bir iş birden çok aygıta istenen özelliği güncelleştirmeleri gönderir.

Bu öğreticinin sonunda bir java konsol cihaz uygulaması ve bir java konsol arka uç uygulaması sahiptir:

**simulated-device** uygular, IOT hub'ına bağlanan **lockDoor** yöntemi ve tanıtıcıları istenen özellik değişikliklerini doğrudan.

**İşleri Zamanla** çağırmak için işleri kullanan **lockDoor** doğrudan yöntemi ve birden çok aygıta cihaz çifti istenen özelliklerini güncelleştirir.

> [!NOTE]
> Makaleyi [Azure IOT SDK'ları](iot-hub-devguide-sdks.md) hem cihaz hem de arka uç uygulamalar oluşturmak için kullanabileceğiniz Azure IOT SDK'ları hakkında bilgi sağlar.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* En güncel [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Etkin bir Azure hesabı. (Bir hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](http://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

Aynı zamanda [IOT uzantısı için Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension) bir cihaz IOT hub'ınıza eklemek için aracı.

## <a name="create-the-service-app"></a>Hizmet Uygulaması Oluştur

Bu bölümde, işleri kullanan bir Java konsol uygulaması oluşturun:

* Çağrı **lockDoor** birden fazla cihazda yöntemi doğrudan.
* İstenen özelliklerde birden fazla cihaza Gönder.

Uygulama oluşturmak için:

1. Geliştirme makinenizde adlı boş bir klasör oluşturun `iot-java-schedule-jobs`.

1. İçinde `iot-java-schedule-jobs` klasörünü adlı bir Maven projesi oluşturun **işleri zamanla** , komut isteminde aşağıdaki komutu kullanarak. Bunun tek ve uzun bir komut olduğunu unutmayın:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=schedule-jobs -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Komut isteminizde gidin `schedule-jobs` klasör.

1. Bir metin düzenleyicisi kullanarak açın `pom.xml` dosyasını `schedule-jobs` klasörü ve aşağıdaki bağımlılığı eklemek **bağımlılıkları** düğümü. Bu bağımlılık kullanmanıza olanak tanır **IOT hizmeti istemcisi** IOT hub ile iletişim kurmak için uygulamanızda paketi:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > En son sürümü için kontrol edebilirsiniz **IOT hizmeti istemcisi** kullanarak [Maven arama](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

1. Aşağıdakileri ekleyin **yapı** düğümünden sonraki **bağımlılıkları** düğümü. Bu yapılandırma, uygulamanızı oluşturmak için Java 1.8 kullanmak için Maven bildirir:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

1. Kaydet ve Kapat `pom.xml` dosya.

1. Bir metin düzenleyicisi kullanarak açın `schedule-jobs\src\main\java\com\mycompany\app\App.java` dosya.

1. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwinDevice;
    import com.microsoft.azure.sdk.iot.service.devicetwin.Pair;
    import com.microsoft.azure.sdk.iot.service.devicetwin.Query;
    import com.microsoft.azure.sdk.iot.service.devicetwin.SqlQuery;
    import com.microsoft.azure.sdk.iot.service.jobs.JobClient;
    import com.microsoft.azure.sdk.iot.service.jobs.JobResult;
    import com.microsoft.azure.sdk.iot.service.jobs.JobStatus;

    import java.util.Date;
    import java.time.Instant;
    import java.util.HashSet;
    import java.util.Set;
    import java.util.UUID;
    ```

1. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. Değiştir `{youriothubconnectionstring}` , not ettiğiniz, IOT hub bağlantı dizesine sahip *IOT Hub oluşturma* bölümü:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    // How long the job is permitted to run without
    // completing its work on the set of devices
    private static final long maxExecutionTimeInSeconds = 30;
    ```

1. Aşağıdaki yöntemi ekleyin **uygulama** güncelleştirmek için bir iş zamanlamak için sınıf **yapı** ve **kat** özellikler cihaz çiftine istenen:

    ```java
    private static JobResult scheduleJobSetDesiredProperties(JobClient jobClient, String jobId) {
      DeviceTwinDevice twin = new DeviceTwinDevice(deviceId);
      Set<Pair> desiredProperties = new HashSet<Pair>();
      desiredProperties.add(new Pair("Building", 43));
      desiredProperties.add(new Pair("Floor", 3));
      twin.setDesiredProperties(desiredProperties);
      // Optimistic concurrency control
      twin.setETag("*");

      // Schedule the update twin job to run now
      // against a single device
      System.out.println("Schedule job " + jobId + " for device " + deviceId);
      try {
        JobResult jobResult = jobClient.scheduleUpdateTwin(jobId, 
          "deviceId='" + deviceId + "'",
          twin,
          new Date(),
          maxExecutionTimeInSeconds);
        return jobResult;
      } catch (Exception e) {
        System.out.println("Exception scheduling desired properties job: " + jobId);
        System.out.println(e.getMessage());
        return null;
      }
    }
    ```

1. Çağrılacak işini zamanlamak için **lockDoor** yöntemi, aşağıdaki yöntemi ekleyin **uygulama** sınıfı:

    ```java
    private static JobResult scheduleJobCallDirectMethod(JobClient jobClient, String jobId) {
      // Schedule a job now to call the lockDoor direct method
      // against a single device. Response and connection
      // timeouts are set to 5 seconds.
      System.out.println("Schedule job " + jobId + " for device " + deviceId);
      try {
        JobResult jobResult = jobClient.scheduleDeviceMethod(jobId,
          "deviceId='" + deviceId + "'",
          "lockDoor",
          5L, 5L, null,
          new Date(),
          maxExecutionTimeInSeconds);
        return jobResult;
      } catch (Exception e) {
        System.out.println("Exception scheduling direct method job: " + jobId);
        System.out.println(e.getMessage());
        return null;
      }
    };
    ```

1. Bir işi izlemek için aşağıdaki yöntemi ekleyin **uygulama** sınıfı:

    ```java
    private static void monitorJob(JobClient jobClient, String jobId) {
      try {
        JobResult jobResult = jobClient.getJob(jobId);
        if(jobResult == null)
        {
          System.out.println("No JobResult for: " + jobId);
          return;
        }
        // Check the job result until it's completed
        while(jobResult.getJobStatus() != JobStatus.completed)
        {
          Thread.sleep(100);
          jobResult = jobClient.getJob(jobId);
          System.out.println("Status " + jobResult.getJobStatus() + " for job " + jobId);
        }
        System.out.println("Final status " + jobResult.getJobStatus() + " for job " + jobId);
      } catch (Exception e) {
        System.out.println("Exception monitoring job: " + jobId);
        System.out.println(e.getMessage());
        return;
      }
    }
    ```

1. Çalıştırdığınız iş ayrıntılarını sorgulamak için aşağıdaki yöntemi ekleyin:

    ```java
    private static void queryDeviceJobs(JobClient jobClient, String start) throws Exception {
      System.out.println("\nQuery device jobs since " + start);

      // Create a jobs query using the time the jobs started
      Query deviceJobQuery = jobClient
          .queryDeviceJob(SqlQuery.createSqlQuery("*", SqlQuery.FromType.JOBS, "devices.jobs.startTimeUtc > '" + start + "'", null).getQuery());

      // Iterate over the list of jobs and print the details
      while (jobClient.hasNextJob(deviceJobQuery)) {
        System.out.println(jobClient.getNextJob(deviceJobQuery));
      }
    }
    ```

1. Güncelleştirme **ana** şunlar için yöntem imzası `throws` yan tümcesi:

    ```java
    public static void main( String[] args ) throws Exception
    ```

1. Çalıştırmak ve iki işler sırayla izlemek için aşağıdaki kodu ekleyin **ana** yöntemi:

    ```java
    // Record the start time
    String start = Instant.now().toString();

    // Create JobClient
    JobClient jobClient = JobClient.createFromConnectionString(iotHubConnectionString);
    System.out.println("JobClient created with success");

    // Schedule twin job desired properties
    // Maximum concurrent jobs is 1 for Free and S1 tiers
    String desiredPropertiesJobId = "DPCMD" + UUID.randomUUID();
    scheduleJobSetDesiredProperties(jobClient, desiredPropertiesJobId);
    monitorJob(jobClient, desiredPropertiesJobId);

    // Schedule twin job direct method
    String directMethodJobId = "DMCMD" + UUID.randomUUID();
    scheduleJobCallDirectMethod(jobClient, directMethodJobId);
    monitorJob(jobClient, directMethodJobId);

    // Run a query to show the job detail
    queryDeviceJobs(jobClient, start);

    System.out.println("Shutting down schedule-jobs app");
    ```

1. Kaydet ve Kapat `schedule-jobs\src\main\java\com\mycompany\app\App.java` dosyası

1. Yapı **işleri zamanla** uygulama ve olan hataları düzeltin. Komut isteminizde gidin `schedule-jobs` klasörü ve aşağıdaki komutu çalıştırın:

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a>Cihaz uygulaması oluşturma

Bu bölümde IOT Hub'ından gönderilen istenen özellikleri işler ve doğrudan yöntem çağrısının uygulayan bir Java konsol uygulaması oluşturun.

1. İçinde `iot-java-schedule-jobs` klasörünü adlı bir Maven projesi oluşturun **simulated-device** , komut isteminde aşağıdaki komutu kullanarak. Bunun tek ve uzun bir komut olduğunu unutmayın:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Komut isteminizde gidin `simulated-device` klasör.

1. Bir metin düzenleyicisi kullanarak açın `pom.xml` dosyasını `simulated-device` klasörü ve aşağıdaki bağımlılıkları ekleyin **bağımlılıkları** düğümü. Bu bağımlılık kullanmanıza olanak tanır **IOT cihaz istemci** IOT hub ile iletişim kurmak için uygulamanızda paketi:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > En son sürümü için kontrol edebilirsiniz **IOT cihaz istemci** kullanarak [Maven arama](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

1. Aşağıdakileri ekleyin **yapı** düğümünden sonraki **bağımlılıkları** düğümü. Bu yapılandırma, uygulamanızı oluşturmak için Java 1.8 kullanmak için Maven bildirir:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

1. Kaydet ve Kapat `pom.xml` dosya.

1. Bir metin düzenleyicisi kullanarak açın `simulated-device\src\main\java\com\mycompany\app\App.java` dosya.

1. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. Değiştirme `{youriothubname}` , IOT hub'ı adıyla ve `{yourdevicekey}` içinde oluşturulan cihaz anahtarla değeri *bir cihaz kimliği oluşturma* bölümü:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    Bu örnek uygulama, bir **DeviceClient** nesnesi örneğini oluşturduğunda **prokotol** değişkenini kullanır.

1. Cihaz çifti bildirimleri konsoluna yazdırmak için aşağıdaki iç içe geçmiş sınıf ekleme **uygulama** sınıfı:

    ```java
    // Handler for device twin operation notifications from IoT Hub
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to device twin operation with status " + status.name());
      }
    }
    ```

1. Konsola doğrudan yöntem bildirimleri yazdırmak için aşağıdaki iç içe geçmiş sınıf ekleme **uygulama** sınıfı:

    ```java
    // Handler for direct method notifications from IoT Hub
    protected static class DirectMethodStatusCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to direct method operation with status " + status.name());
      }
    }
    ```

1. IOT hub'dan doğrudan yöntem çağrıları işlemek için aşağıdaki iç içe geçmiş sınıf ekleme **uygulama** sınıfı:

    ```java
    // Handler for direct method calls from IoT Hub
    protected static class DirectMethodCallback
        implements DeviceMethodCallback {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context) {
        DeviceMethodData deviceMethodData;
        switch (methodName) {
          case "lockDoor": {
            System.out.println("Executing direct method: " + methodName);
            deviceMethodData = new DeviceMethodData(METHOD_SUCCESS, "Executed direct method " + methodName);
            break;
          }
          default: {
            deviceMethodData = new DeviceMethodData(METHOD_NOT_DEFINED, "Not defined direct method " + methodName);
          }
        }
        // Notify IoT Hub of result
        return deviceMethodData;
      }
    }
    ```

1. Güncelleştirme **ana** şunlar için yöntem imzası `throws` yan tümcesi:

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException
    ```

1. Aşağıdaki kodu ekleyin **ana** yöntemi için:
    * IOT Hub ile iletişim kurmak için bir cihaz istemcisi oluşturun.
    * Oluşturma bir **aygıt** cihaz çifti özelliklerini depolamak için nesne.

    ```java
    // Create a device client
    DeviceClient client = new DeviceClient(connString, protocol);

    // An object to manage device twin desired and reported properties
    Device dataCollector = new Device() {
      @Override
      public void PropertyCall(String propertyKey, Object propertyValue, Object context)
      {
        System.out.println("Received desired property change: " + propertyKey + " " + propertyValue);
      }
    };
    ```

1. Aygıt istemci hizmetlerini başlatmak için aşağıdaki kodu ekleyin **ana** yöntemi:

    ```java
    try {
      // Open the DeviceClient
      // Start the device twin services
      // Subscribe to direct method calls
      client.open();
      client.startDeviceTwin(new DeviceTwinStatusCallBack(), null, dataCollector, null);
      client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
    } catch (Exception e) {
      System.out.println("Exception, shutting down \n" + " Cause: " + e.getCause() + " \n" + e.getMessage());
      dataCollector.clean();
      client.closeNow();
      System.out.println("Shutting down...");
    }
    ```

1. Kullanıcı basmak beklenecek **Enter** anahtar kapatmadan önce sonuna aşağıdaki kodu ekleyin **ana** yöntemi:

    ```java
    // Close the app
    System.out.println("Press any key to exit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    dataCollector.clean();
    client.closeNow();
    scanner.close();
    ```

1. Kaydet ve Kapat `simulated-device\src\main\java\com\mycompany\app\App.java` dosya.

1. Yapı **simulated-device** uygulama ve olan hataları düzeltin. Komut isteminizde gidin `simulated-device` klasörü ve aşağıdaki komutu çalıştırın:

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Şimdi konsol uygulamaları çalıştırmaya hazırsınız.

1. Bir komut isteminde `simulated-device` klasörü, istenen özellik değişikliklerini ve doğrudan yöntem çağrıları için dinleme aygıt uygulamasını başlatmak için aşağıdaki komutu çalıştırın:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Aygıt istemcisi başlatır](media/iot-hub-java-java-schedule-jobs/device-app-1.png)

1. Bir komut isteminde `schedule-jobs` klasörü çalıştırmak için aşağıdaki komutu çalıştırın, **işleri zamanla** iki işlerini çalıştırmak için hizmeti uygulaması. İlk istenen özellik değerlerini ayarlar, ikinci doğrudan yöntemini çağırır:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IOT Hub hizmeti uygulaması iki işleri oluşturur](media/iot-hub-java-java-schedule-jobs/service-app-1.png)

1. Cihaz uygulaması istenen özellik değişikliği ve doğrudan yöntem çağrısının işler:

    ![Aygıt istemcisi değişiklikleri yanıt veriyor](media/iot-hub-java-java-schedule-jobs/device-app-2.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portalında yeni bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. İki işlerini çalıştırmak için bir arka uç uygulaması oluşturuldu. İlk iş istenen özellik değerlerini ayarlayın ve ikinci işi doğrudan bir yöntem çağrılır.

Bilgi edinmek için aşağıdaki kaynakları kullanın nasıl yapılır:

* Aygıtlarla telemetri gönderen [IOT Hub ile çalışmaya başlama](iot-hub-java-java-getstarted.md) Öğreticisi.
* Aygıtlarını etkileşimli olarak (örneğin, kullanıcı tarafından denetlenen bir uygulamadan fan etkinleştirdikten) ile [doğrudan yöntemleri kullanın](iot-hub-java-java-direct-methods.md) Öğreticisi.
