---
title: Azure IOT Hub cihaz Yönetimi (Java) ile çalışmaya başlama | Microsoft Docs
description: Uzak aygıt yeniden başlatma işlemi başlatmak için Azure IOT Hub cihaz yönetimini kullanma Doğrudan bir yöntem ve Azure IOT hizmeti doğrudan yöntemini çağıran bir hizmet uygulaması uygulamak için Java SDK'sını içeren bir sanal cihaz uygulamasının uygulamak için Azure IOT cihaz SDK'sı Java için kullanın.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 6a4ba8c2b88520dff028610cf64aa9b3a6e3fefd
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34636197"
---
# <a name="get-started-with-device-management-java"></a>Aygıt Yönetimi (Java) ile çalışmaya başlama

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* IOT hub'ı oluşturun ve IOT hub'ınızda bir cihaz kimliği oluşturma için Azure Portalı'nı kullanın.
* Cihaz yeniden başlatmak için doğrudan bir yöntem uygulayan bir sanal cihaz uygulaması oluşturursunuz. Doğrudan yöntemleri buluttan çağrılır.
* IOT hub'ınız aracılığıyla sanal cihaz uygulama yeniden başlatma doğrudan yöntemini çağıran bir uygulama oluşturun. Bu uygulama daha sonra yeniden başlatma işlemi tamamlandığında görmek için aygıt bildirilen özelliklerinden izler.

Bu öğreticinin sonunda iki Java konsol uygulamaları vardır:

**simulated-device**. Bu uygulama:

* Daha önce oluşturulan cihaz kimliğiyle IOT hub'ınıza bağlanır.
* Yeniden başlatma doğrudan yöntem çağrısı alır.
* Fiziksel bir yeniden başlatma benzetimini yapar.
* Bildirilen bir özellik aracılığıyla son yeniden başlatma zamanı raporlar.

**Tetikleyici yeniden başlatma**. Bu uygulama:

* Sanal cihaz uygulamasının doğrudan bir yöntem çağırır.
* Sanal aygıt tarafından gönderilen doğrudan yöntem çağrısının yanıtı görüntüler
* Güncelleştirilmiş görüntüler özellikleri bildirdi.

> [!NOTE]
> Aygıtları ve çözüm arka ucunuz çalıştırmak için uygulamalar oluşturmak için kullanabileceğiniz SDK'ları hakkında daha fazla bilgi için bkz: [Azure IOT SDK'ları][lnk-hub-sdks].

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Java SE 8. <br/> [Geliştirme ortamınızı hazırlama][lnk-dev-setup], bu öğretici için Java'nın Windows veya Linux'ta nasıl yükleneceğini açıklar.
* Maven 3.  <br/> [Geliştirme ortamınızı hazırlama][lnk-dev-setup], bu öğretici için [Maven][lnk-maven]'ın Windows veya Linux'ta nasıl yükleneceğini açıklar.
* [Node.js sürümünü 0.10.0 veya üzeri](http://nodejs.org).

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Tetikleyici doğrudan bir yöntem kullanarak cihaz üzerinde Uzaktan yeniden başlatma

Bu bölümde, bir Java konsol uygulaması, oluşturun:

1. Sanal cihaz uygulamasının yeniden başlatma doğrudan yöntemi çağırır.
1. Yanıt görüntüler.
1. Anketler yeniden başlatmanın ne zaman tamamlandığını belirlemek için cihazın gönderilen bildirilen özellikleri.

Bu konsol uygulamasını IOT doğrudan yöntemini çağırmak ve bildirilen özellikleri okumak için Hub'ınıza bağlanır.

1. DM-get-started adlı boş bir klasör oluşturun.

1. Dm-get-started klasöründe adlı bir Maven projesi oluşturun **tetikleyici yeniden başlatma** , komut isteminde aşağıdaki komutu kullanarak. Tek ve uzun bir komut gösterir:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=trigger-reboot -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Komut isteminizde tetikleyici yeniden başlatma klasöre gidin.

1. Bir metin düzenleyicisi kullanarak, tetikleyici yeniden başlatma klasöründeki pom.xml dosyasını açın ve aşağıdaki bağımlılığı eklemek **bağımlılıkları** düğümü. Bu bağımlılık, IOT hub ile iletişim kurmak için uygulamanızda IOT-service-client paketini kullanmanıza olanak sağlar:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > [Maven arama][lnk-maven-service-search] kullanarak en yeni **iot-service-client** sürümünü kontrol edebilirsiniz.

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

1. pom.xml dosyasını kaydedin ve kapatın.

1. Bir metin düzenleyicisi kullanarak trigger-reboot\src\main\java\com\mycompany\app\App.java kaynak dosyasını açın.

1. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwin;
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwinDevice;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

1. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. Değiştir `{youriothubconnectionstring}` , not ettiğiniz, IOT hub bağlantı dizesine sahip *IOT Hub oluşturma* bölümü:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    private static final String methodName = "reboot";
    private static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    private static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    ```

1. Bildirilen özellikleri cihaz çifti 10 saniyede okuyan bir iş parçacığı uygulamak için aşağıdaki iç içe geçmiş sınıf ekleme **uygulama** sınıfı:

    ```java
    private static class ShowReportedProperties implements Runnable {
      public void run() {
        try {
          DeviceTwin deviceTwins = DeviceTwin.createFromConnectionString(iotHubConnectionString);
          DeviceTwinDevice twinDevice = new DeviceTwinDevice(deviceId);
          while (true) {
            System.out.println("Get reported properties from device twin");
            deviceTwins.getTwin(twinDevice);
            System.out.println(twinDevice.reportedPropertiesToString());
            Thread.sleep(10000);
          }
        } catch (Exception ex) {
          System.out.println("Exception reading reported properties: " + ex.getMessage());
        }
      }
    }
    ```

1. İmzası değiştirme **ana** yöntemi aşağıdaki özel durum:

    ```java
    public static void main(String[] args) throws IOException
    ```

1. Sanal cihaz için yeniden başlatma doğrudan yöntemini çağırmak için aşağıdaki kodu ekleyin **ana** yöntemi:

    ```java
    System.out.println("Starting sample...");
    DeviceMethod methodClient = DeviceMethod.createFromConnectionString(iotHubConnectionString);

    try
    {
      System.out.println("Invoke reboot direct method");
      MethodResult result = methodClient.invoke(deviceId, methodName, responseTimeout, connectTimeout, null);

      if(result == null)
      {
        throw new IOException("Invoke direct method reboot returns null");
      }
      System.out.println("Invoked reboot on device");
      System.out.println("Status for device:   " + result.getStatus());
      System.out.println("Message from device: " + result.getPayload());
    }
    catch (IotHubException e)
    {
        System.out.println(e.getMessage());
    }
    ```

1. Sanal cihaz bildirilen özelliklerinden yoklamak için iş parçacığı başlatmak için aşağıdaki kodu ekleyin **ana** yöntemi:

    ```java
    ShowReportedProperties showReportedProperties = new ShowReportedProperties();
    ExecutorService executor = Executors.newFixedThreadPool(1);
    executor.execute(showReportedProperties);
    ```

1. Uygulamayı durdurun sağlamak için aşağıdaki kodu ekleyin **ana** yöntemi:

    ```java
    System.out.println("Press ENTER to exit.");
    System.in.read();
    executor.shutdownNow();
    System.out.println("Shutting down sample...");
    ```

1. Trigger-reboot\src\main\java\com\mycompany\app\App.java dosyasını kaydedip kapatın.

1. Yapı **tetikleyici yeniden başlatma** arka uç uygulama ve olan hataları düzeltin. Komut isteminde, tetikleyici yeniden başlatma klasörüne gidin ve aşağıdaki komutu çalıştırın:

    `mvn clean package -DskipTests`

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma

Bu bölümde, bir cihaza benzetim bir Java konsol uygulaması oluşturun. Yeniden başlatma uygulama dinler yöntem çağrısı IOT hub'ınızı ve hemen o aramayı yanıtlar doğrudan. Uygulama sonra uykuya geçme bildirmek için bildirilen bir özellik kullanmadan önce yeniden başlatma işleminin benzetimini yapmak bir süre için **tetikleyici yeniden başlatma** yeniden başlatma tamamlandıktan arka uç uygulama.

1. Dm-get-started klasöründe adlı bir Maven projesi oluşturun **simulated-device** , komut isteminde aşağıdaki komutu kullanarak. Tek ve uzun bir komut verilmiştir:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Komut isteminizde simulated-device klasörüne gidin.

1. Bir metin düzenleyicisi kullanarak simulated-device klasöründeki pom.xml dosyasını açın ve aşağıdaki bağımlılığı eklemek **bağımlılıkları** düğümü. Bu bağımlılık, IOT hub ile iletişim kurmak için uygulamanızda IOT-service-client paketini kullanmanıza olanak sağlar:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > [Maven arama][lnk-maven-device-search] kullanarak en yeni **iot-device-client** sürümünü kontrol edebilirsiniz.

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

1. pom.xml dosyasını kaydedin ve kapatın.

1. Bir metin düzenleyicisi kullanarak simulated-device\src\main\java\com\mycompany\app\App.java kaynak dosyasını açın.

1. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.time.LocalDateTime;
    import java.util.Scanner;
    import java.util.Set;
    import java.util.HashSet;
    ```

1. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. Değiştir `{yourdeviceconnectionstring}` , not ettiğiniz aygıt bağlantı dizesiyle *bir cihaz kimliği oluşturma* bölümü:

    ```java
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;

    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String connString = "{yourdeviceconnectionstring}";
    private static DeviceClient client;
    ```

1. Doğrudan yöntem durum olayları için bir geri çağırma işleyici uygulamak için aşağıdaki iç içe geçmiş sınıf ekleme **uygulama** sınıfı:

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded to device method operation with status " + status.name());
      }
    }
    ```

1. Cihaz çifti durum olayları için bir geri çağırma işleyici uygulamak için aşağıdaki iç içe geçmiş sınıf ekleme **uygulama** sınıfı:

    ```java
    protected static class DeviceTwinStatusCallback implements IotHubEventCallback
    {
        public void execute(IotHubStatusCode status, Object context)
        {
            System.out.println("IoT Hub responded to device twin operation with status " + status.name());
        }
    }
    ```

1. Özellik olayları için bir geri çağırma işleyici uygulamak için aşağıdaki iç içe geçmiş sınıf ekleme **uygulama** sınıfı:

    ```java
    protected static class PropertyCallback implements PropertyCallBack<String, String>
    {
      public void PropertyCall(String propertyKey, String propertyValue, Object context)
      {
        System.out.println("PropertyKey:     " + propertyKey);
        System.out.println("PropertyKvalue:  " + propertyKey);
      }
    }
    ```

1. Aygıt yeniden başlatma benzetimini yapmak için bir iş parçacığı uygulamak için aşağıdaki iç içe geçmiş sınıf ekleme **uygulama** sınıfı. İş parçacığı beş saniye için uyku moduna geçer ve ardından ayarlar **lastReboot** özelliği bildirdi:

    ```java
    protected static class RebootDeviceThread implements Runnable {
      public void run() {
        try {
          System.out.println("Rebooting...");
          Thread.sleep(5000);
          Property property = new Property("lastReboot", LocalDateTime.now());
          Set<Property> properties = new HashSet<Property>();
          properties.add(property);
          client.sendReportedProperties(properties);
          System.out.println("Rebooted");
        }
        catch (Exception ex) {
          System.out.println("Exception in reboot thread: " + ex.getMessage());
        }
      }
    }
    ```

1. Cihazda doğrudan yöntemi uygulamak için aşağıdaki iç içe geçmiş sınıf ekleme **uygulama** sınıfı. Ne zaman sanal uygulama alan bir çağrı **yeniden** doğrudan yöntemi, bir bildirim çağırana döndürür ve ardından yeniden işlemek için bir iş parçacığı başlatır:

    ```java
    protected static class DirectMethodCallback implements com.microsoft.azure.sdk.iot.device.DeviceTwin.DeviceMethodCallback
    {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context)
      {
        DeviceMethodData deviceMethodData;
        switch (methodName)
        {
          case "reboot" :
          {
            int status = METHOD_SUCCESS;
            System.out.println("Received reboot request");
            deviceMethodData = new DeviceMethodData(status, "Started reboot");
            RebootDeviceThread rebootThread = new RebootDeviceThread();
            Thread t = new Thread(rebootThread);
            t.start();
            break;
          }
          default:
          {
            int status = METHOD_NOT_DEFINED;
            deviceMethodData = new DeviceMethodData(status, "Not defined direct method " + methodName);
          }
        }
        return deviceMethodData;
      }
    }
    ```

1. İmzası değiştirme **ana** aşağıdaki özel durumlar oluşturma yöntemi:

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException
    ```

1. Örneği oluşturmak için bir **DeviceClient**, aşağıdaki kodu ekleyin **ana** yöntemi:

    ```java
    System.out.println("Starting device client sample...");
    client = new DeviceClient(connString, protocol);
    ```

1. Doğrudan yöntem çağrıları için dinleme başlatmak için aşağıdaki kodu ekleyin **ana** yöntemi:

    ```java
    try
    {
      client.open();
      client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
      client.startDeviceTwin(new DeviceTwinStatusCallback(), null, new PropertyCallback(), null);
      System.out.println("Subscribed to direct methods and polling for reported properties. Waiting...");
    }
    catch (Exception e)
    {
      System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" +  e.getMessage());
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. Aygıt benzeticisi kapatmak için aşağıdaki kodu ekleyin **ana** yöntemi:

    ```java
    System.out.println("Press any key to exit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    scanner.close();
    client.close();
    System.out.println("Shutting down...");
    ```

1. Simulated-device\src\main\java\com\mycompany\app\App.java dosyasını kaydedip kapatın.

1. Yapı **simulated-device** arka uç uygulama ve olan hataları düzeltin. Komut isteminde, simulated-device klasörüne gidin ve aşağıdaki komutu çalıştırın:

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Simulated-device klasöründeki bir komut isteminde IOT hub'ınızı gelen yeniden başlatma yöntemi çağrıları dinleme yapmaya başlamak için aşağıdaki komutu çalıştırın:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Dinlemek için Java IOT hub'ı sanal cihaz uygulamasının yeniden doğrudan yöntem çağrıları][1]

1. Tetikleyici yeniden başlatma klasöründeki bir komut isteminde IOT hub'ından sanal cihazınız üzerinde yeniden başlatma yöntemini çağırmak için aşağıdaki komutu çalıştırın:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Yeniden başlatma doğrudan yöntemini çağırmak için Java IOT Hub hizmeti uygulaması][2]

1. Sanal cihaz yeniden başlatma doğrudan yöntem çağrısı yanıt verir:

    ![Java IOT hub'ı sanal cihaz uygulamasının doğrudan yöntem çağrısı yanıt verir.][3]

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[1]: ./media/iot-hub-java-java-device-management-getstarted/launchsimulator.png
[2]: ./media/iot-hub-java-java-device-management-getstarted/triggerreboot.png
[3]: ./media/iot-hub-java-java-device-management-getstarted/respondtoreboot.png
<!-- Links -->

[lnk-maven]: https://maven.apache.org/what-is-maven.html

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md

[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-device-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22