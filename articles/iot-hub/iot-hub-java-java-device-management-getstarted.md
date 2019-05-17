---
title: Azure IOT Hub cihaz yönetimini (Java) kullanmaya başlama | Microsoft Docs
description: Uzak cihazı yeniden başlatmak için Azure IOT Hub cihaz Yönetimi'ni kullanma Bir doğrudan yöntem ve Azure IOT hizmeti doğrudan yöntemini çağıran bir hizmet uygulaması uygulamak için Java SDK'sını içeren bir sanal cihaz uygulaması uygulamak için Azure IOT cihaz SDK'sı için Java kullanın.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 08/08/2017
ms.openlocfilehash: e9100a764ba3922e0254b7fa5cd03b18e204925f
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65596010"
---
# <a name="get-started-with-device-management-java"></a>Cihaz yönetimini (Java) kullanmaya başlama

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* IOT Hub oluşturma ve IOT hub'ınızda bir cihaz kimliği oluşturmak için Azure portalını kullanın.

* Cihazı yeniden başlatmak için bir doğrudan yöntem uygulayan bir sanal cihaz uygulaması oluşturma. Doğrudan yöntemler buluttan çağrılır.

* Sanal cihaz uygulamasının, IOT hub'ı üzerinden doğrudan yeniden başlatma yöntemi çağıran bir uygulama oluşturun. Bu uygulama CİHAZDAN yeniden başlatma işlemi tamamlandığında görmek için bildirilen özellikleri sonra izler.

Bu öğreticinin sonunda, iki Java konsol uygulamanız olacak:

**simulated-device**. Bu uygulama:

* Daha önce oluşturulan cihaz kimliğiyle IOT hub'ınıza bağlanır.

* Yeniden başlatma doğrudan yöntem çağırma alır.

* Fiziksel sistemin yeniden başlatılması benzetimini yapar.

* Bildirilen özellik aracılığıyla son yeniden başlatma süresini bildiriyor.

**tetikleyiciyi yeniden başlatma**. Bu uygulama:

* Sanal cihaz uygulamasında bir doğrudan yöntem çağırır.

* Sanal cihaz tarafından gönderilen bir doğrudan yöntem çağrısına yanıt görüntüler.

* Bildirilen özellikler güncelleştirilmiş görüntüler.

> [!NOTE]
> Cihazlar ve çözüm arka ucunuz üzerinde çalıştırılacak uygulamalar oluşturmak için kullanabileceğiniz SDK'lar hakkında daha fazla bilgi için bkz. [Azure IOT SDK'ları](iot-hub-devguide-sdks.md).

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Java SE 8. <br/> [Geliştirme ortamınızı hazırlama](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md) Java Bu öğretici için Windows veya Linux'ta nasıl yükleneceğini açıklar.

* Maven 3.  <br/> [Geliştirme ortamınızı hazırlama](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md) nasıl yüklendiğini açıklar [Maven](https://maven.apache.org/what-is-maven.html) Windows veya Linux üzerinde Bu öğretici için.

* Etkin bir Azure hesabı. (Hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.)

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>IOT hub için bağlantı dizesi alma

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Bir doğrudan yöntem kullanarak cihaz üzerinde Uzaktan yeniden başlatma tetikleyin

Bu bölümde, bir Java konsol uygulaması oluşturun:

1. Sanal cihaz uygulaması yeniden başlatma doğrudan yöntemini çağırır.

2. Yanıt görüntüler.

3. Polls – yeniden başlatma ne zaman tamamlandığını belirlemek için CİHAZDAN gönderilen bildirilen özellikleri.

Bu konsol uygulamasını IOT doğrudan yöntem çağırma ve bildirilen özellikler okumak için Hub'ınıza bağlanır.

1. DM-get-started adlı boş bir klasör oluşturun.

2. Dm-get-started klasöründe adlı bir Maven projesi oluşturun **tetikleyici-reboot** komut isteminizde aşağıdaki komutu kullanarak. Tek ve uzun bir komut aşağıda gösterilmiştir:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=trigger-reboot -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

3. Komut isteminizde tetikleyici yeniden klasöre gidin.

4. Bir metin düzenleyicisi kullanarak, tetikleyici yeniden başlatma klasöründeki pom.xml dosyasını açın ve aşağıdaki bağımlılığı ekleyin **bağımlılıkları** düğümü. Bu bağımlılık, IOT hub ile iletişim kurmak için uygulamanızda IOT-service-client paketini kullanmanıza olanak sağlar:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > En son sürümünü kontrol **IOT hizmeti istemcisi** kullanarak [Maven arama](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

5. Aşağıdaki **derleme** düğümünün sonra **bağımlılıkları** düğümü. Bu yapılandırma, uygulama oluşturmak için Java 1.8 kullanmak için Maven bildirir:

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

6. pom.xml dosyasını kaydedin ve kapatın.

7. Bir metin düzenleyicisi kullanarak trigger-reboot\src\main\java\com\mycompany\app\App.java kaynak dosyasını açın.

8. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

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

9. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. Değiştirin `{youriothubconnectionstring}` not ettiğiniz IOT hub bağlantı dizenizle *IOT Hub oluşturma* bölümü:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    private static final String methodName = "reboot";
    private static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    private static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    ```

10. Bildirilen özellikler cihaz ikizinden 10 saniyede okuyan bir iş parçacığı uygulamak için aşağıdaki iç içe geçmiş Sınıf Ekle **uygulama** sınıfı:

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

11. Değişiklik imzası **ana** yönteminin şu özel durum:

    ```java
    public static void main(String[] args) throws IOException
    ```

12. Sanal cihazı yeniden başlatma doğrudan yöntemini çağırmak için aşağıdaki kodu ekleyin. **ana** yöntemi:

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

13. Bildirilen özellikler sanal CİHAZDAN yoklamak için iş parçacığı başlatmak için aşağıdaki kodu ekleyin. **ana** yöntemi:

    ```java
    ShowReportedProperties showReportedProperties = new ShowReportedProperties();
    ExecutorService executor = Executors.newFixedThreadPool(1);
    executor.execute(showReportedProperties);
    ```

14. Uygulamayı durdurun sağlamak için aşağıdaki kodu ekleyin. **ana** yöntemi:

    ```java
    System.out.println("Press ENTER to exit.");
    System.in.read();
    executor.shutdownNow();
    System.out.println("Shutting down sample...");
    ```

15. Trigger-reboot\src\main\java\com\mycompany\app\App.java dosyasını kaydedip kapatın.

16. Derleme **tetikleyici-reboot** arka uç uygulaması ve olan hataları düzeltin. Komut isteminizde tetikleyici yeniden klasöre gidin ve aşağıdaki komutu çalıştırın:

    `mvn clean package -DskipTests`

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma

Bu bölümde, bir cihaza benzetim yapan bir Java konsol uygulaması oluşturun. Yeniden başlatma uygulama dinler, bu çağrı için metot çağrısı IOT hub'ınıza ve hemen yanıt doğrudan. Uygulama sonra uyku bildirmek üzere bildirilen özellik kullanmadan önce yeniden başlatma işleminin benzetimini yapmak, bir süredir **tetikleyici yeniden başlatma** yeniden başlatma tamamlandıktan arka uç uygulaması.

1. Dm-get-started klasöründe adlı bir Maven projesi oluşturun **simulated-device** komut isteminizde aşağıdaki komutu kullanarak. Tek ve uzun bir komut aşağıda verilmiştir:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

2. Komut isteminizde simulated-device klasörüne gidin.

3. Bir metin düzenleyicisi kullanarak simulated-device klasöründeki pom.xml dosyasını açın ve aşağıdaki bağımlılığı ekleyin **bağımlılıkları** düğümü. Bu bağımlılık, IOT hub ile iletişim kurmak için uygulamanızda IOT-service-client paketini kullanmanıza olanak sağlar:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > En son sürümünü kontrol **IOT cihaz istemcisi** kullanarak [Maven arama](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

4. Aşağıdaki **derleme** düğümünün sonra **bağımlılıkları** düğümü. Bu yapılandırma, uygulama oluşturmak için Java 1.8 kullanmak için Maven bildirir:

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

5. pom.xml dosyasını kaydedin ve kapatın.

6. Bir metin düzenleyicisi kullanarak simulated-device\src\main\java\com\mycompany\app\App.java kaynak dosyasını açın.

7. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

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

7. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. Değiştirin `{yourdeviceconnectionstring}` not ettiğiniz cihaz bağlantı dizesiyle *bir cihaz kimliği oluşturma* bölümü:

    ```java
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;

    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String connString = "{yourdeviceconnectionstring}";
    private static DeviceClient client;
    ```

8. Doğrudan yöntem durum olayları için bir geri çağırma işleyici uygulamak için aşağıdaki iç içe geçmiş Sınıf Ekle **uygulama** sınıfı:

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded to device method operation with status " + status.name());
      }
    }
    ```

9. Cihaz ikizi durumu olayları için bir geri çağırma işleyici uygulamak için aşağıdaki iç içe geçmiş Sınıf Ekle **uygulama** sınıfı:

    ```java
    protected static class DeviceTwinStatusCallback implements IotHubEventCallback
    {
        public void execute(IotHubStatusCode status, Object context)
        {
            System.out.println("IoT Hub responded to device twin operation with status " + status.name());
        }
    }
    ```

10. Özellik olayları için bir geri çağırma işleyici uygulamak için aşağıdaki iç içe geçmiş Sınıf Ekle **uygulama** sınıfı:

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

11. Cihaz önyüklemesi benzetmek için bir iş parçacığı uygulamak için aşağıdaki iç içe geçmiş Sınıf Ekle **uygulama** sınıfı. İş parçacığı beş saniye için uyku moduna geçer ve ardından ayarlar **lastReboot** bildirilen özellik:

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

12. Doğrudan yöntemi cihazda uygulamak için aşağıdaki iç içe geçmiş Sınıf Ekle **uygulama** sınıfı. Sanal uygulama için bir çağrı aldığında **yeniden** doğrudan yöntem, bir bildirim çağırana döner ve daha sonra yeniden başlatma işlemi için bir iş parçacığı başlatır:

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

13. Değişiklik imzası **ana** aşağıdaki özel durumlar oluşturan yöntemi:

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException
    ```

14. Örneği oluşturmak için bir **DeviceClient**, aşağıdaki kodu ekleyin **ana** yöntemi:

    ```java
    System.out.println("Starting device client sample...");
    client = new DeviceClient(connString, protocol);
    ```

15. Doğrudan yöntem çağrıları için dinleme başlatmak için aşağıdaki kodu ekleyin. **ana** yöntemi:

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

16. Cihaz simülatörü kapatmak için aşağıdaki kodu ekleyin. **ana** yöntemi:

    ```java
    System.out.println("Press any key to exit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    scanner.close();
    client.close();
    System.out.println("Shutting down...");
    ```

17. Simulated-device\src\main\java\com\mycompany\app\App.java dosyasını kaydedip kapatın.

18. Derleme **simulated-device** arka uç uygulaması ve olan hataları düzeltin. Komut isteminizde simulated-device klasörüne gidin ve aşağıdaki komutu çalıştırın:

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Simulated-device klasöründeki komut isteminde IOT hub'ınıza yeniden başlatma yöntemine yapılan çağrılar için dinleme başlamak için aşağıdaki komutu çalıştırın:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IOT Hub sanal cihaz uygulaması dinlemek için doğrudan yöntem çağrılarını yeniden başlatma](./media/iot-hub-java-java-device-management-getstarted/launchsimulator.png)

2. Tetikleyici yeniden başlatma klasöründeki bir komut isteminde, yeniden başlatma yöntemi sanal Cihazınızı IOT hub'ından çağırmak için aşağıdaki komutu çalıştırın:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Yeniden başlatma doğrudan yöntem çağırma yönelik Java IOT Hub hizmet uygulaması](./media/iot-hub-java-java-device-management-getstarted/triggerreboot.png)

3. Sanal cihazı yeniden başlatma doğrudan yöntem çağrısına yanıt verir:

    ![Java IOT Hub sanal cihaz uygulaması doğrudan yöntem çağrısına yanıt verir.](./media/iot-hub-java-java-device-management-getstarted/respondtoreboot.png)

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]