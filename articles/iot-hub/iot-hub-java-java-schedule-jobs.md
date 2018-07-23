---
title: Azure IOT hub'ı (Java) ile işleri zamanlama | Microsoft Docs
description: Bir doğrudan yöntem çağırma ve birden çok cihazda istenen bir özelliği ayarlamak için bir Azure IOT hub'ı işini zamanlamak nasıl. Sanal cihaz uygulamalarını hem de Azure IOT hizmeti işi çalıştırmak için bir hizmet uygulaması uygulamak için Java SDK'sını uygulamak için Azure IOT cihaz SDK'sı için Java kullanın.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 07/10/2017
ms.author: dobett
ms.openlocfilehash: 3161715ac2ff212e2de8a27ff8f8eb53fb858b92
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39186814"
---
# <a name="schedule-and-broadcast-jobs-java"></a>Zamanlama ve yayınlama işleri (Java)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Zamanlama ve milyonlarca cihaza güncelleştirme işleri izlemek için Azure IOT Hub'ı kullanın. İşleri kullanın:

* İstenen özellikleri güncelleştirme
* Etiketleri güncelleştirin
* Doğrudan metotları çağırma

Bir iş, aşağıdaki eylemlerden birini sarmalar ve yürütmeyi bir dizi cihazda izler. Cihaz ikizi sorgusu iş karşı yürütür cihaz kümesini tanımlar. Örneğin, bir arka uç uygulaması, bir iş cihazları yeniden bir doğrudan yöntem 10.000 cihaz üzerinde çağırmak için kullanabilirsiniz. Bir dizi cihazda cihaz ikizi sorgusu ile belirtin ve gelecekteki bir tarihte çalışmaya planlayın. CİHAZDAN her biri olarak ilerleyişini izler, alma ve yeniden başlatma doğrudan yöntem yürütün.

Bu özelliklerin her biri hakkında daha fazla bilgi için bkz:

* Cihaz ikizi ve özellikleri: [cihaz ikizlerini kullanmaya başlama](iot-hub-java-java-twin-getstarted.md)
* Doğrudan yöntemler: [IOT Hub Geliştirici Kılavuzu - doğrudan yöntemler](iot-hub-devguide-direct-methods.md) ve [Öğreticisi: doğrudan yöntemler kullanma](quickstart-control-device-java.md)

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* Çağrılan doğrudan bir yönteme uygulayan bir cihaz uygulaması oluşturma **lockDoor**. Cihaz uygulaması, arka uç uygulamasından da istenen özellik değişiklikleri alır.
* Çağırmak için bir iş oluşturur bir arka uç uygulaması oluşturma **lockDoor** birden fazla cihazda doğrudan yöntem. Başka bir iş birden çok cihaz için istenen özellik güncelleştirmeleri gönderir.

Bu öğreticinin sonunda, bir java konsol cihaz uygulaması ve bir java konsol arka uç uygulaması vardır:

**simulated-device** uygular, IOT hub'ına bağlanan **lockDoor** yöntemi ve işleyicilerini istenen özellik değişiklikleri doğrudan.

**İşleri zamanlama** çağırmak için işleri kullanan **lockDoor** doğrudan yöntemini ve birden fazla cihazda cihaz çiftinin istenen özelliklerini güncelleştirin.

> [!NOTE]
> Makaleyi [Azure IOT SDK'ları](iot-hub-devguide-sdks.md) hem cihaz hem de arka uç uygulamaları oluşturmak için kullanabileceğiniz Azure IOT SDK'ları hakkında bilgi sağlar.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* En güncel [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Etkin bir Azure hesabı. (Hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](http://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

Ayrıca [Azure CLI 2.0 için IOT uzantısı](https://github.com/Azure/azure-iot-cli-extension) IOT hub'ınıza bir cihaz eklemek için araç.

## <a name="create-the-service-app"></a>Hizmet uygulaması oluşturma

Bu bölümde, işleri kullanan bir Java konsol uygulaması oluşturun:

* Çağrı **lockDoor** birden fazla cihazda doğrudan yöntem.
* İstenen özellikler birden çok cihaza gönderin.

Uygulama oluşturmak için:

1. Geliştirme makinenizde adlı boş bir klasör oluşturma `iot-java-schedule-jobs`.

1. İçinde `iot-java-schedule-jobs` klasöründe adlı bir Maven projesi oluşturun **işleri zamanlama** komut isteminizde aşağıdaki komutu kullanarak. Bunun tek ve uzun bir komut olduğunu unutmayın:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=schedule-jobs -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Komut isteminizde gidin `schedule-jobs` klasör.

1. Bir metin düzenleyicisi kullanarak açın `pom.xml` dosyası `schedule-jobs` klasörü ve aşağıdaki bağımlılığı ekleyin **bağımlılıkları** düğümü. Bu bağımlılık kullanmanızı sağlayan **IOT hizmeti istemcisi** , IOT hub ile iletişim kurmak için uygulamanızda paket:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > En son sürümünü kontrol **IOT hizmeti istemcisi** kullanarak [Maven arama](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

1. Aşağıdaki **derleme** düğümünün sonra **bağımlılıkları** düğümü. Bu yapılandırma, uygulama oluşturmak için Java 1.8 kullanmak için Maven bildirir:

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

1. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. Değiştirin `{youriothubconnectionstring}` not ettiğiniz IOT hub bağlantı dizenizle *IOT Hub oluşturma* bölümü:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    // How long the job is permitted to run without
    // completing its work on the set of devices
    private static final long maxExecutionTimeInSeconds = 30;
    ```

1. Aşağıdaki yöntemi ekleyin **uygulama** güncelleştirmek için bir iş zamanlama sınıfı **yapı** ve **kat** istenen cihaz ikizinde özellikleri:

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

1. Çağırmak için bir iş zamanlama **lockDoor** yöntemi, aşağıdaki yöntemi ekleyin **uygulama** sınıfı:

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

1. Bir işi izlemek için aşağıdaki yöntemi ekleyin. **uygulama** sınıfı:

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

1. Çalıştırdığınız işlerinin ayrıntılarını sorgulamak için aşağıdaki yöntemi ekleyin:

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

1. Çalıştırın ve iki işler sırayla izlemek için aşağıdaki kodu ekleyin. **ana** yöntemi:

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

1. Derleme **işleri zamanlama** uygulama ve olan hataları düzeltin. Komut isteminizde gidin `schedule-jobs` klasörü ve aşağıdaki komutu çalıştırın:

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a>Cihaz uygulaması oluşturma

Bu bölümde, IOT Hub'ından gönderilen istenen özellikleri işleyen ve doğrudan yöntem çağrısında uygulayan bir Java konsol uygulaması oluşturun.

1. İçinde `iot-java-schedule-jobs` klasöründe adlı bir Maven projesi oluşturun **simulated-device** komut isteminizde aşağıdaki komutu kullanarak. Bunun tek ve uzun bir komut olduğunu unutmayın:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Komut isteminizde gidin `simulated-device` klasör.

1. Bir metin düzenleyicisi kullanarak açın `pom.xml` dosyası `simulated-device` klasörü ve aşağıdaki bağımlılıkları ekleyin **bağımlılıkları** düğümü. Bu bağımlılık kullanmanızı sağlayan **IOT cihaz istemcisi** , IOT hub ile iletişim kurmak için uygulamanızda paket:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > En son sürümünü kontrol **IOT cihaz istemcisi** kullanarak [Maven arama](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

1. Aşağıdaki **derleme** düğümünün sonra **bağımlılıkları** düğümü. Bu yapılandırma, uygulama oluşturmak için Java 1.8 kullanmak için Maven bildirir:

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

1. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. Değiştirme `{youriothubname}` IOT hub'ı adınızla ve `{yourdevicekey}` cihaz anahtarı değerini *bir cihaz kimliği oluşturma* bölümü:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    Bu örnek uygulama, bir **DeviceClient** nesnesi örneğini oluşturduğunda **prokotol** değişkenini kullanır.

1. Cihaz ikizi bildirimleri konsola yazdırmak için aşağıdaki iç içe geçmiş sınıf için ekleme **uygulama** sınıfı:

    ```java
    // Handler for device twin operation notifications from IoT Hub
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to device twin operation with status " + status.name());
      }
    }
    ```

1. Doğrudan yöntem bildirimleri konsola yazdırmak için aşağıdaki iç içe geçmiş sınıf için ekleme **uygulama** sınıfı:

    ```java
    // Handler for direct method notifications from IoT Hub
    protected static class DirectMethodStatusCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to direct method operation with status " + status.name());
      }
    }
    ```

1. IOT Hub'ından doğrudan yöntem çağrıları işlemek için aşağıdaki iç içe geçmiş sınıf için ekleme **uygulama** sınıfı:

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

1. Aşağıdaki kodu ekleyin **ana** yöntemi:
    * IOT Hub ile iletişim kurmak için bir cihaz istemcisi oluşturun.
    * Oluşturma bir **cihaz** cihaz ikizi özelliklerini depolamak için nesne.

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

1. Cihaz istemci hizmetlerini başlatmak için aşağıdaki kodu ekleyin. **ana** yöntemi:

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

1. Kullanıcı basmak beklenecek **Enter** kapatmadan önce anahtarı, sonuna aşağıdaki kodu ekleyin **ana** yöntemi:

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

1. Derleme **simulated-device** uygulama ve olan hataları düzeltin. Komut isteminizde gidin `simulated-device` klasörü ve aşağıdaki komutu çalıştırın:

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Artık konsol uygulamaları çalıştırmaya hazırsınız.

1. Bir komut isteminde `simulated-device` klasörü, istenen özellik değişiklikleri ve doğrudan yöntem çağrıları için dinleme cihaz uygulamasını başlatmak için aşağıdaki komutu çalıştırın:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Cihaz istemcisi başlar](media/iot-hub-java-java-schedule-jobs/device-app-1.png)

1. Bir komut isteminde `schedule-jobs` çalıştırmak için aşağıdaki komutu çalıştırın, klasör **işleri zamanlama** iki işlerini çalıştırmak için app service. İstenen özellik değerleri ilk ayarlar, ikinci bir doğrudan yöntem çağırır:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![İki iş Java IOT Hub hizmet uygulaması oluşturur](media/iot-hub-java-java-schedule-jobs/service-app-1.png)

1. Cihaz uygulaması, istenen özellik değişikliğinin hem de doğrudan yöntem çağrısında işler:

    ![Cihaz istemcisi değişikliklere yanıt verdiği.](media/iot-hub-java-java-schedule-jobs/device-app-2.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portalında yeni bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. İki işlerini çalıştırmak için bir arka uç uygulaması oluşturdunuz. İlk işiniz ayarlanan istenen özellik değerleri ve ikinci iş bir doğrudan yöntem çağrılır.

Bilgi edinmek için aşağıdaki kaynakları kullanın. nasıl yapılır:

* İle cihazlardan telemetri gönderme [IOT Hub ile çalışmaya başlama](quickstart-send-telemetry-java.md) öğretici.
* İle etkileşimli olarak (örneğin, bir kullanıcı tarafından denetlenen uygulamasından fan özelliğini) cihazları denetleme [doğrudan yöntemler kullanma](quickstart-control-device-java.md) öğretici.
