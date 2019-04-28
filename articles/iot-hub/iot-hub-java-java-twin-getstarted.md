---
title: Azure IOT Hub cihaz ikizlerini (Java) kullanmaya başlama | Microsoft Docs
description: Azure IOT Hub cihaz ikizlerini etiketler ekleyin ve ardından IOT Hub sorgu kullanma Cihaz uygulaması ve Azure IOT hizmeti etiketleri ekler ve IOT Hub sorgu çalışan bir hizmet uygulaması uygulamak için Java SDK'sını uygulamak için Azure IOT cihaz SDK'sı için Java kullanın.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 07/04/2017
ms.openlocfilehash: bfb111b07db105190fc59f21b3255c2ea2b1471c
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62129915"
---
# <a name="get-started-with-device-twins-java"></a>Cihaz ikizlerini (Java) kullanmaya başlama

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Bu öğreticide, iki Java konsol uygulaması oluşturun:

* **Ekle-etiketleri-query**, etiketleri ekler ve cihaz ikizlerini sorgular bir Java arka uç uygulaması.
* **simulated-device**, IOT hub'ınıza bağlanır ve bildirilen özellik kullanarak, bağlantı koşulunun raporları bir Java cihaz uygulaması.

> [!NOTE]
> Makaleyi [Azure IOT SDK'ları](iot-hub-devguide-sdks.md) hem cihaz hem de arka uç uygulamaları oluşturmak için kullanabileceğiniz Azure IOT SDK'ları hakkında bilgi sağlar.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* En güncel [Java SE Development Kit 8](https://aka.ms/azure-jdks)

* [Maven 3](https://maven.apache.org/install.html)

* Etkin bir Azure hesabı. (Hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.)

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>IOT hub için bağlantı dizesi alma

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Yeni bir cihaz IOT hub'ı Kaydet

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="create-the-service-app"></a>Hizmet uygulaması oluşturma

Bu bölümde, IOT hub'daki cihaz ikizine etiket ile ilişkili olarak, konum meta veri ekleyen bir Java uygulaması oluşturma **myDeviceId**. Uygulamayı ilk IOT hub'ın ABD'de bulunan cihazlara ve ardından bir hücresel ağ bağlantısı rapor cihazlar için sorgular.

1. Geliştirme makinenizde adlı boş bir klasör oluşturma `iot-java-twin-getstarted`.

2. İçinde `iot-java-twin-getstarted` klasöründe adlı bir Maven projesi oluşturun **Ekle-etiketleri-query** komut isteminizde aşağıdaki komutu kullanarak. Bunun tek ve uzun bir komut olduğunu unutmayın:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=add-tags-query -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

3. Komut isteminizde gidin `add-tags-query` klasör.

4. Bir metin düzenleyicisi kullanarak açın `pom.xml` dosyası `add-tags-query` klasörü ve aşağıdaki bağımlılığı ekleyin **bağımlılıkları** düğümü. Bu bağımlılık kullanmanızı sağlayan **IOT hizmeti istemcisi** , IOT hub ile iletişim kurmak için uygulamanızda paket:

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

6. Kaydet ve Kapat `pom.xml` dosya.

7. Bir metin düzenleyicisi kullanarak açın `add-tags-query\src\main\java\com\mycompany\app\App.java` dosya.

8. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.*;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.HashSet;
    import java.util.Set;
    ```

9. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. Değiştirin `{youriothubconnectionstring}` not ettiğiniz IOT hub bağlantı dizenizle *IOT Hub oluşturma* bölümü:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String region = "US";
    public static final String plant = "Redmond43";
    ```

10. Güncelleştirme **ana** şunlar için yöntem imzası `throws` yan tümcesi:

    ```java
    public static void main( String[] args ) throws IOException
    ```

11. Aşağıdaki kodu ekleyin **ana** yöntemini **DeviceTwin** ve **DeviceTwinDevice** nesneleri. **DeviceTwin** nesne, IOT hub'ınızla iletişim işler. **DeviceTwinDevice** nesnesi, cihaz ikizi özelliklerini ve etiketleri ile temsil eder:

    ```java
    // Get the DeviceTwin and DeviceTwinDevice objects
    DeviceTwin twinClient = DeviceTwin.createFromConnectionString(iotHubConnectionString);
    DeviceTwinDevice device = new DeviceTwinDevice(deviceId);
    ```

12. Aşağıdaki `try/catch` bloğunu **ana** yöntemi:

    ```java
    try {
      // Code goes here
    } catch (IotHubException e) {
      System.out.println(e.getMessage());
    } catch (IOException e) {
      System.out.println(e.getMessage());
    }
    ```

13. Güncelleştirilecek **bölge** ve **tesis** , cihaz ikizindeki cihaz ikizi etiketler ekleyin aşağıdaki kodda `try` bloğu:

    ```java
    // Get the device twin from IoT Hub
    System.out.println("Device twin before update:");
    twinClient.getTwin(device);
    System.out.println(device);

    // Update device twin tags if they are different
    // from the existing values
    String currentTags = device.tagsToString();
    if ((!currentTags.contains("region=" + region) && !currentTags.contains("plant=" + plant))) {
      // Create the tags and attach them to the DeviceTwinDevice object
      Set<Pair> tags = new HashSet<Pair>();
      tags.add(new Pair("region", region));
      tags.add(new Pair("plant", plant));
      device.setTags(tags);

      // Update the device twin in IoT Hub
      System.out.println("Updating device twin");
      twinClient.updateTwin(device);
    }

    // Retrieve the device twin with the tag values from IoT Hub
    System.out.println("Device twin after update:");
    twinClient.getTwin(device);
    System.out.println(device);
    ```

14. IOT hub'ındaki cihaz ikizlerini sorgulamak için aşağıdaki kodu ekleyin. `try` önceki adımda eklediğiniz koddan sonra blok. Kod, iki sorguları çalıştırır. Her sorgu, en fazla 100 cihaz döndürür:

    ```java
    // Query the device twins in IoT Hub
    System.out.println("Devices in Redmond:");

    // Construct the query
    SqlQuery sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "tags.plant='Redmond43'", null);

    // Run the query, returning a maximum of 100 devices
    Query twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 100);
    while (twinClient.hasNextDeviceTwin(twinQuery)) {
      DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
      System.out.println(d.getDeviceId());
    }

    System.out.println("Devices in Redmond using a cellular network:");

    // Construct the query
    sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "tags.plant='Redmond43' AND properties.reported.connectivityType = 'cellular'", null);

    // Run the query, returning a maximum of 100 devices
    twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 3);
    while (twinClient.hasNextDeviceTwin(twinQuery)) {
      DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
      System.out.println(d.getDeviceId());
    }
    ```

15. Kaydet ve Kapat `add-tags-query\src\main\java\com\mycompany\app\App.java` dosyası

16. Derleme **Ekle-etiketleri-query** uygulama ve olan hataları düzeltin. Komut isteminizde gidin `add-tags-query` klasörü ve aşağıdaki komutu çalıştırın:

    ```
    mvn clean package -DskipTests
    ```

## <a name="create-a-device-app"></a>Cihaz uygulaması oluşturma

Bu bölümde, IOT Hub'ına gönderilen bildirilen özellik değerini ayarlayan bir Java konsol uygulaması oluşturun.

1. İçinde `iot-java-twin-getstarted` klasöründe adlı bir Maven projesi oluşturun **simulated-device** komut isteminizde aşağıdaki komutu kullanarak. Bunun tek ve uzun bir komut olduğunu unutmayın:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Komut isteminizde gidin `simulated-device` klasör.

3. Bir metin düzenleyicisi kullanarak açın `pom.xml` dosyası `simulated-device` klasörü ve aşağıdaki bağımlılıkları ekleyin **bağımlılıkları** düğümü. Bu bağımlılık kullanmanızı sağlayan **IOT cihaz istemcisi** , IOT hub ile iletişim kurmak için uygulamanızda paket:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.14.2</version>
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

5. Kaydet ve Kapat `pom.xml` dosya.

6. Bir metin düzenleyicisi kullanarak açın `simulated-device\src\main\java\com\mycompany\app\App.java` dosya.

7. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

8. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. Değiştirme `{youriothubname}` IOT hub'ı adınızla ve `{yourdevicekey}` cihaz anahtarı değerini *bir cihaz kimliği oluşturma* bölümü:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myDeviceId";
    ```

    Bu örnek uygulama, bir **DeviceClient** nesnesi örneğini oluşturduğunda **prokotol** değişkenini kullanır. 

1. Aşağıdaki yöntemi ekleyin **uygulama** ikizi güncelleştirmeleri hakkında bilgi yazdırma sınıfı:

    ```java
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback {
        @Override
        public void execute(IotHubStatusCode status, Object context) {
          System.out.println("IoT Hub responded to device twin operation with status " + status.name());
        }
      }
    ```

9. Aşağıdaki kodu ekleyin **ana** yöntemi:
    * IOT Hub ile iletişim kurmak için bir cihaz istemcisi oluşturun.
    * Oluşturma bir **cihaz** cihaz ikizi özelliklerini depolamak için nesne.

      ```java
      DeviceClient client = new DeviceClient(connString, protocol);

      // Create a Device object to store the device twin properties
      Device dataCollector = new Device() {
      // Print details when a property value changes
      @Override
      public void PropertyCall(String propertyKey, Object propertyValue, Object context) {
        System.out.println(propertyKey + " changed to " + propertyValue);
      }
      };
      ```

10. Aşağıdaki kodu ekleyin **ana** yöntemi oluşturmak için bir **connectivityType** bildirilen özellik ve IOT Hub'ına gönderebilirsiniz:

    ```java
    try {
      // Open the DeviceClient and start the device twin services.
      client.open();
      client.startDeviceTwin(new DeviceTwinStatusCallBack(), null, dataCollector, null);

      // Create a reported property and send it to your IoT hub.
      dataCollector.setReportedProp(new Property("connectivityType", "cellular"));
      client.sendReportedProperties(dataCollector.getReportedProp());
    }
    catch (Exception e) {
      System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" + e.getMessage());
      dataCollector.clean();
      client.closeNow();
      System.out.println("Shutting down...");
    }
    ```

11. Sonuna aşağıdaki kodu ekleyin **ana** yöntemi. Bekleyen **Enter** zaman IOT hub'ının cihaz ikizi işlemleri durumunu bildirmek anahtar sağlar:

    ```java
    System.out.println("Press any key to exit...");

    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();

    dataCollector.clean();
    client.close();
    ```

1. **main** yönteminin imzasını, aşağıda gösterilen özel durumları içerecek şekilde değiştirin:

     ```java
     public static void main(String[] args) throws URISyntaxException, IOException
     ```

1. Kaydet ve Kapat `simulated-device\src\main\java\com\mycompany\app\App.java` dosya.

13. Derleme **simulated-device** uygulama ve olan hataları düzeltin. Komut isteminizde gidin `simulated-device` klasörü ve aşağıdaki komutu çalıştırın:

    ```
    mvn clean package -DskipTests
    ```

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Artık konsol uygulamaları çalıştırmaya hazırsınız.

1. Bir komut isteminde `add-tags-query` çalıştırmak için aşağıdaki komutu çalıştırın, klasör **Ekle-etiketleri-query** service uygulaması:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Etiket değerlerini güncelleştirin ve cihaz sorguları çalıştırmak için Java IOT Hub hizmet uygulaması](./media/iot-hub-java-java-twin-getstarted/service-app-1.png)

    Gördüğünüz **tesis** ve **bölge** cihaz ikizine eklenen etiketler. Cihazınız ilk sorgu döndürür, ancak ikinci desteklemez.

2. Bir komut isteminde `simulated-device` klasör eklemek için aşağıdaki komutu çalıştırın, **connectivityType** özelliği için cihaz ikizi bildirdi:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Cihaz istemcisi ekler ** connectivityType ** bildirilen özellik](./media/iot-hub-java-java-twin-getstarted/device-app-1.png)

3. Bir komut isteminde `add-tags-query` çalıştırmak için aşağıdaki komutu çalıştırın, klasör **Ekle-etiketleri-query** service uygulaması ikinci kez:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Etiket değerlerini güncelleştirin ve cihaz sorguları çalıştırmak için Java IOT Hub hizmet uygulaması](./media/iot-hub-java-java-twin-getstarted/service-app-2.png)

    Cihazınızı gönderdiği artık **connectivityType** özelliği IOT hub'ına ikinci sorgu, Cihazınızı döndürür.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portalında yeni bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Cihaz meta verilerini etiketler bir arka uç uygulamasından eklenen ve rapor cihaz bağlantı bilgileri cihaz ikizindeki cihaz uygulaması söyleyebiliriz. Ayrıca cihaz ikizi bilgileri SQL benzeri IOT Hub sorgu dili kullanarak sorgulamayı öğrendiniz.

Bilgi edinmek için aşağıdaki kaynakları kullanın. nasıl yapılır:

* İle cihazlardan telemetri gönderme [IOT Hub ile çalışmaya başlama](quickstart-send-telemetry-java.md) öğretici.

* İle etkileşimli olarak (örneğin, bir kullanıcı tarafından denetlenen uygulamasından fan özelliğini) cihazları denetleme [doğrudan yöntemler kullanma](quickstart-control-device-java.md) öğretici.