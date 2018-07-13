---
title: Azure IOT hub'ı (Java/Java) ile cihaz üretici yazılımını güncelleştirme | Microsoft Docs
description: Cihaz üretici yazılımı güncelleştirme başlatmak için Azure IOT Hub cihaz Yönetimi kullanma Bir sanal cihaz uygulaması uygulayın ve üretici yazılımı güncelleştirmesini tetikler bir hizmet uygulaması uygulamak için Azure IOT cihaz SDK'sı için Java kullanın.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 09/11/2017
ms.author: dobett
ms.openlocfilehash: 5991615bca26749e1f138b561260108f8bcf2646
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38611336"
---
# <a name="use-device-management-to-initiate-a-device-firmware-update-javajava"></a>Cihaz üretici yazılımını güncelleştirme (Java/Java) başlatmak için cihaz Yönetimi'ni kullanın
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

İçinde [cihaz yönetimini kullanmaya başlama] [ lnk-dm-getstarted] eğitmen, nasıl kullanılacağını gördüğünüz [cihaz ikizi] [ lnk-devtwin] ve [doğrudan yöntemler ] [ lnk-c2dmethod] ilkel bir cihazı Uzaktan yeniden başlatmak için. Bu öğretici aynı IOT hub'ı temelleri kullanır ve uçtan uca sanal üretici yazılımı güncelleştirme işlemini gösterir.  Bu düzen üretici yazılımı güncelleştirme uygulaması için kullanılan [Raspberry Pi cihaz uygulama örnek][lnk-rpi-implementation].

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* Çağıran bir Java konsol uygulaması oluşturacaksınız **firmwareUpdate** üzerinde sanal cihaz uygulamasının, IOT hub'ı aracılığıyla doğrudan yöntemi.
* Uygular ve cihaza benzetim yapan bir Java konsol uygulaması oluşturma **firmwareUpdate** doğrudan yöntemi. Bu yöntem, üretici yazılımı görüntüsünü indirmeye bekler, üretici yazılımı görüntüsünü indirir ve son olarak üretici yazılımı görüntüsünü geçerlidir çok aşamalı bir işlem başlatır. Güncelleştirmenin her aşamasında cihaz ilerlemesini bildirmek üzere bildirilen özellikleri kullanır.

Bu öğreticinin sonunda, iki Java konsol uygulamanız olacak:

**üretici yazılımı güncelleştirmesi**, sanal cihaz üzerinde doğrudan bir yöntem çağırır, yanıt ve düzenli aralıklarla görüntüler bildirilen özellik güncelleştirmeleri

**simulated-device**, daha önce oluşturulan cihaz kimliğiyle IOT hub'ınıza bağlanır, firmwareUpdate doğrudan yöntem çağrısı alır ve bir üretici yazılımı güncelleştirme simülasyonu çalıştırır

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* En güncel [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 
* [Maven 3](https://maven.apache.org/install.html) 
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Bir doğrudan yöntem kullanarak cihaz üzerinde bir uzak üretici yazılımı güncelleştirmesini tetikleme
Bu bölümde, bir cihazda uzak üretici yazılımı güncelleştirme başlatan bir Java konsol uygulaması oluşturun. Uygulamayı bir doğrudan yöntem güncelleştirmeyi başlatmak için ve cihaz çifti sorguları etkin üretici yazılımı güncelleştirme durumunu düzenli aralıklarla almak için kullanır.

1. FW-get-started adlı boş bir klasör oluşturun.

1. Fw-get-started klasöründe adlı bir Maven projesi oluşturun **üretici yazılımı güncelleştirmesi** komut isteminizde aşağıdaki komutu kullanarak. Bunun tek ve uzun bir komut olduğunu unutmayın:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=firmware-update -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Komut isteminizde üretici yazılımı güncelleştirmesi klasöre gidin.

1. Bir metin düzenleyicisi kullanarak, üretici yazılımı güncelleştirmesi klasöründeki pom.xml dosyasını açın ve aşağıdaki bağımlılığı ekleyin **bağımlılıkları** düğümü. Bu bağımlılık, IOT hub ile iletişim kurmak için uygulamanızda IOT-service-client paketini kullanmanıza olanak sağlar:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.5.22</version>
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

1. pom.xml dosyasını kaydedin ve kapatın.

1. Bir metin düzenleyicisi kullanarak firmware-update\src\main\java\com\mycompany\app\App.java kaynak dosyasını açın.

1. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwin;
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwinDevice;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    ```

1. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. Değiştirin **{youriothubconnectionstring}** not ettiğiniz IOT hub bağlantı dizenizle *IOT Hub oluşturma* bölümü:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    private static final String methodName = "firmwareUpdate";
    private static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    private static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    ```

1. Bildirilen özellikler cihaz ikizinden okuyan bir yöntem uygulamak için aşağıdaki ekleyin **uygulama** sınıfı:

    ```java
    public static void ShowReportedProperties() 
    {
        try 
        {
          DeviceTwin deviceTwins = DeviceTwin.createFromConnectionString(iotHubConnectionString);
          DeviceTwinDevice twinDevice = new DeviceTwinDevice(deviceId);
          
          Boolean firmwareUpdated = false;
          Integer timeoutCycle = 0;
    
          while (!firmwareUpdated)
          {
            if (timeoutCycle > 5)
            {
              System.out.println("Operation timed out");
              break;
            }
    
            Thread.sleep(1000);
              
            deviceTwins.getTwin(twinDevice);
    
            String reportedProperties = twinDevice.reportedPropertiesToString();
              
            if(reportedProperties.contains("status=waiting"))
            {
              System.out.println("Waiting on device...");
            }
            else if(reportedProperties.contains("status=downloadComplete"))
            {
              System.out.println("Download complete, applying firmware...");
            }
            else if (reportedProperties.contains("status=applyComplete"))
            {
              System.out.println("Firmware applied");
              System.out.println("Get reported properties from device twin");
              System.out.println(twinDevice.reportedPropertiesToString());
              firmwareUpdated = true;
            }
            else
            {
              timeoutCycle++;
            }
          }
        } catch (Exception ex) {
            System.out.println("Exception reading reported properties: " + ex.getMessage());
        }
    }
    ```

1. Değişiklik imzası **ana** aşağıdaki özel durumlar oluşturan yöntemi:

    ```java
    public static void main( String[] args ) throws IOException
    ```

1. Sanal cihaz üzerinde firmwareUpdate doğrudan yöntem çağırma için aşağıdaki kodu ekleyin. **ana** yöntemi:

    ```java
    DeviceMethod methodClient = DeviceMethod.createFromConnectionString(iotHubConnectionString);

    try
    {
      String payload = "https://someurl";
      
      System.out.println("Invoked firmware update on device.");
      System.out.println("Sent URL: " + payload);
      
      MethodResult result = methodClient.invoke(deviceId, methodName, responseTimeout, connectTimeout, payload);

      if(result == null)
      {
        throw new IOException("Invoke direct method reboot returns null");
      }

      System.out.println("Status for device:   " + result.getStatus());
      System.out.println("Message from device: " + result.getPayload());
    }
    catch (IotHubException e)
    {
      System.out.println(e.getMessage());
    }
    ```

1. Bildirilen özellikler sanal CİHAZDAN yoklamak için aşağıdaki kodu ekleyin. **ana** yöntemi:

    ```java
    ShowReportedProperties();
    ```

1. Uygulamayı durdurun sağlamak için aşağıdaki kodu ekleyin. **ana** yöntemi:

    ```java
    System.out.println("Press ENTER to exit.");
    System.in.read();
    System.out.println("Shutting down sample...");
    ```

1. Firmware-update\src\main\java\com\mycompany\app\App.java dosyasını kaydedip kapatın.

1. Derleme **üretici yazılımı güncelleştirmesi** arka uç uygulaması ve olan hataları düzeltin. Komut isteminizde üretici yazılımı güncelleştirmesi klasöre gidin ve aşağıdaki komutu çalıştırın:

    `mvn clean package -DskipTests`

## <a name="simulate-a-device-to-handle-direct-method-calls"></a>Doğrudan yöntem çağrıları işlemek için bir cihazın benzetimini gerçekleştirme
Bu bölümde, firmwareUpdate doğrudan yöntem alabileceği bir Java konsol sanal cihaz uygulaması oluşturun. Uygulama durumu iletişim kurmak için reportedProperties kullanarak üretici yazılımı güncelleştirmesi benzetimini yapmak için çok durumlu bir işlem yapması sonra çalışır.

1. Fw-get-started klasöründe adlı bir Maven projesi oluşturun **simulated-device** komut isteminizde aşağıdaki komutu kullanarak. Bunun tek ve uzun bir komut olduğunu unutmayın:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Komut isteminizde simulated-device klasörüne gidin.

1. Bir metin düzenleyicisi kullanarak, üretici yazılımı güncelleştirmesi klasöründeki pom.xml dosyasını açın ve aşağıdaki bağımlılığı ekleyin **bağımlılıkları** düğümü. Bu bağımlılık, IOT hub ile iletişim kurmak için uygulamanızda IOT-service-client paketini kullanmanıza olanak sağlar:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
      <type>jar</type>
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
    import java.util.HashMap;
    ```

1. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. Değiştirin **{yourdeviceconnectionstring}** not ettiğiniz cihaz bağlantısı dizeniz ile *bir cihaz kimliği oluşturma* bölümü:

    ```java
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;

    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String connString = "{yourdeviceconnectionstring";
    private static DeviceClient client;

    private static String downloadURL = "unknown";
    ```

1. Doğrudan yöntem işlevselliği uygulamak için geri aramaları için aşağıdaki iç içe geçmiş sınıflar ekleyerek sağlamak **uygulama** sınıfı:

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded to device method operation with status " + status.name());
      }
    }
    protected static class DirectMethodCallback implements com.microsoft.azure.sdk.iot.device.DeviceTwin.DeviceMethodCallback
    {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context)
      {
        DeviceMethodData deviceMethodData;
        switch (methodName)
        {
          case "firmwareUpdate" :
          {
            System.out.println("Response to method '" + methodName + "' sent successfully");
    
            downloadURL = new String((byte[])methodData);
    
            int status = METHOD_SUCCESS;
            System.out.println("Starting firmware update");
            deviceMethodData = new DeviceMethodData(status, "Started firmware update");
            FirmwareUpdateThread firmwareUpdateThread = new FirmwareUpdateThread();
            Thread t = new Thread(firmwareUpdateThread);
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

1. Cihaz ikizi işlevselliği uygulamak için geri aramaları için aşağıdaki iç içe geçmiş sınıflar ekleyerek sağlamak **uygulama** sınıfı:

    ```java
    protected static class DeviceTwinStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded to device twin operation with status " + status.name());
      }
    }
    protected static class PropertyCallback implements PropertyCallBack<String, String>
    {
      public void PropertyCall(String propertyKey, String propertyValue, Object context)
      {
        System.out.println("PropertyKey:     " + propertyKey);
        System.out.println("PropertyKvalue:  " + propertyKey);
      }
    }
    ```

1. Üretici yazılımı güncelleştirme uygulamak için aşağıdaki iç içe geçmiş Sınıf Ekle **uygulama** sınıfı:

    ```java
    protected static class FirmwareUpdateThread implements Runnable {
      public void run() {
        try {      
          HashMap initialUpdate = new HashMap();
          Property sentProperty = new Property("firmwareUpdate", initialUpdate);
          Set<Property> sentPackage = new HashSet<Property>();
          
          System.out.println("Downloading from " + downloadURL);
    
          initialUpdate.put("status", "waiting");
          initialUpdate.put("fwPackageUri", downloadURL);
          initialUpdate.put("startedWaitingTime", LocalDateTime.now().toString());   
          sentPackage.add(sentProperty);
    
          client.sendReportedProperties(sentPackage);
    
          Thread.sleep(5000);
    
          System.out.println("Download complete");
    
          HashMap downloadUpdate = new HashMap();
          downloadUpdate.put("status","downloadComplete");
          downloadUpdate.put("downloadCompleteTime", LocalDateTime.now().toString());
          downloadUpdate.put("startedApplyingImage", LocalDateTime.now().toString());
          sentProperty.setValue(downloadUpdate);
    
          client.sendReportedProperties(sentPackage);
    
          Thread.sleep(5000);
    
          System.out.println("Apply complete");
    
          HashMap applyUpdate = new HashMap();
          applyUpdate.put("status","applyComplete");
          applyUpdate.put("lastFirmwareUpdate", LocalDateTime.now().toString());
          sentProperty.setValue(applyUpdate);
    
          client.sendReportedProperties(sentPackage);
    
          Thread.sleep(5000);
    
          HashMap resetUpdate = new HashMap();
          applyUpdate.put("status","reset");
          sentProperty.setValue(resetUpdate);
    
          client.sendReportedProperties(sentPackage);
        }
        catch (Exception ex) {
          System.out.println("Exception in reboot thread: " + ex.getMessage());
        }
      }
    }
    ```

1. Değişiklik imzası **ana** aşağıdaki özel durumlar oluşturan yöntemi:

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException
    ```

1. Cihaz ikizlerini yordamı ve doğrudan yöntemler başlatmak için aşağıdaki kodu ekleyin. **ana** yöntemi:

    ```java
    client = new DeviceClient(connString, protocol);

    try
    {
      client.open();
      client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
      client.startDeviceTwin(new DeviceTwinStatusCallback(), null, new PropertyCallback(), null);
      System.out.println("Client connected to IoT Hub.  Waiting for firmwareUpdate direct method.");
    }
    catch (Exception e)
    {
      System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" +  e.getMessage());
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. Uygulamayı durdurun sağlamak için sonuna aşağıdaki kodu ekleyin **ana** yöntemi:

    ```java
    System.out.println("Press any key to exit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    scanner.close();
    client.close();
    System.out.println("Shutting down...");
    ```

1. Simulated-device\src\main\java\com\mycompany\app\App.java dosyasını kaydedip kapatın.

1. Derleme **simulated-device** uygulama ve olan hataları düzeltin. Komut isteminizde simulated-device klasörüne gidin ve aşağıdaki komutu çalıştırın:

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a>Uygulamaları çalıştırma
Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Bir komut isteminde **simulated-device** klasörü, üretici yazılımı güncelleştirme doğrudan yöntemi için dinleme başlamak için aşağıdaki komutu çalıştırın.
   
    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

1. Bir komut isteminde **üretici yazılımı güncelleştirmesi** klasörü, üretici yazılımı güncelleştirmesi çağırmak ve sanal cihazınızdan IOT hub'ınız üzerinde cihaz çiftlerini sorgulamak için aşağıdaki komutu çalıştırın:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

3. Sanal cihazı bir doğrudan yöntem konsolunda yanıt verme görebilirsiniz.

    ![Üretici yazılımı başarıyla güncelleştirildi][img-fwupdate]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir cihazda bir uzak üretici yazılımı güncelleştirmesini tetikleme için kullanılan bir doğrudan yöntem ve bildirilen özellikleri üretici yazılımı güncelleştirme durumunu izlemek için kullanılır.

IOT çözümü ve zamanlama yöntemi çağıran birden çok cihazda genişletmek öğrenmek için bkz [işleri zamanlama ve yayınlama] [ lnk-tutorial-jobs] öğretici.

<!-- images -->
[img-fwupdate]: media/iot-hub-java-java-firmware-update/firmwareUpdated.png

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-java-java-device-management-getstarted.md
[lnk-tutorial-jobs]: iot-hub-java-java-schedule-jobs.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-rpi-implementation]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt_dm/pi_device