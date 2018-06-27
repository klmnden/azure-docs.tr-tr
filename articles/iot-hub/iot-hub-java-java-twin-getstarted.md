---
title: Azure IOT Hub cihaz çiftlerini (Java) ile çalışmaya başlama | Microsoft Docs
description: Azure IOT Hub cihaz çiftlerini etiket ekleyebilir ve IOT Hub sorgusuyla kullanmak için nasıl kullanılacağını. Cihaz uygulaması ve Azure IOT hizmeti etiketleri ekler ve IOT hub'ı sorgu çalışan bir hizmet uygulaması uygulamak için Java SDK'sını uygulamak için Azure IOT cihaz SDK'sı Java için kullanın.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 07/04/2017
ms.author: dobett
ms.openlocfilehash: 96cad0fc7f387c5f0cb14996ae6ac015c104b81d
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37016708"
---
# <a name="get-started-with-device-twins-java"></a>Cihaz çiftlerini (Java) ile çalışmaya başlama

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Bu öğreticide, iki Java konsol uygulamaları oluşturun:

* **ekleme-etiketleri-query**, etiketleri ekler ve cihaz çiftlerini sorgular bir Java arka uç uygulaması.
* **simulated-device**, IOT hub'ınıza bağlanır ve bildirilen bir özellik kullanarak kendi bağlantı koşulunun raporları bir Java cihaz uygulaması.

> [!NOTE]
> Makaleyi [Azure IOT SDK'ları](iot-hub-devguide-sdks.md) hem cihaz hem de arka uç uygulamalar oluşturmak için kullanabileceğiniz Azure IOT SDK'ları hakkında bilgi sağlar.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* En güncel [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Etkin bir Azure hesabı. (Bir hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](http://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="create-the-service-app"></a>Hizmet Uygulaması Oluştur

Bu bölümde IOT Hub cihaz çiftine etiket ilişkili olarak, konum meta veri ekleyen bir Java uygulaması oluşturma **myDeviceId**. Uygulamayı ilk IOT hub'ın ABD'de bulunan aygıtları ve ardından bir cep telefonu ağ bağlantısı rapor cihazlar için sorgular.

1. Geliştirme makinenizde adlı boş bir klasör oluşturun `iot-java-twin-getstarted`.

1. İçinde `iot-java-twin-getstarted` klasörünü adlı bir Maven projesi oluşturun **ekleme-etiketleri-query** , komut isteminde aşağıdaki komutu kullanarak. Bunun tek ve uzun bir komut olduğunu unutmayın:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=add-tags-query -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Komut isteminizde gidin `add-tags-query` klasör.

1. Bir metin düzenleyicisi kullanarak açın `pom.xml` dosyasını `add-tags-query` klasörü ve aşağıdaki bağımlılığı eklemek **bağımlılıkları** düğümü. Bu bağımlılık kullanmanıza olanak tanır **IOT hizmeti istemcisi** IOT hub ile iletişim kurmak için uygulamanızda paketi:

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

1. Bir metin düzenleyicisi kullanarak açın `add-tags-query\src\main\java\com\mycompany\app\App.java` dosya.

1. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.*;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.HashSet;
    import java.util.Set;
    ```

1. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. Değiştir `{youriothubconnectionstring}` , not ettiğiniz, IOT hub bağlantı dizesine sahip *IOT Hub oluşturma* bölümü:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String region = "US";
    public static final String plant = "Redmond43";
    ```

1. Güncelleştirme **ana** şunlar için yöntem imzası `throws` yan tümcesi:

    ```java
    public static void main( String[] args ) throws IOException
    ```

1. Aşağıdaki kodu ekleyin **ana** oluşturmak için yöntemi **DeviceTwin** ve **DeviceTwinDevice** nesneleri. **DeviceTwin** nesnesini işleme IOT hub ile iletişim. **DeviceTwinDevice** nesne özelliklerini ve etiketler ile cihaz çifti temsil eder:

    ```java
    // Get the DeviceTwin and DeviceTwinDevice objects
    DeviceTwin twinClient = DeviceTwin.createFromConnectionString(iotHubConnectionString);
    DeviceTwinDevice device = new DeviceTwinDevice(deviceId);
    ```

1. Aşağıdakileri ekleyin `try/catch` için engelleyin **ana** yöntemi:

    ```java
    try {
      // Code goes here
    } catch (IotHubException e) {
      System.out.println(e.getMessage());
    } catch (IOException e) {
      System.out.println(e.getMessage());
    }
    ```

1. Güncelleştirilecek **bölge** ve **tesis** cihaz çifti etiketler, cihaz çiftine aşağıdaki kodu ekleyin `try` engelle:

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

1. IOT hub cihaz çiftlerini sorgulamak için aşağıdaki kodu ekleyin `try` önceki adımda eklediğiniz koddan sonra bloğu. Kod iki sorguları çalıştırır. Her sorgu en fazla 100 cihazları döndürür:

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

1. Kaydet ve Kapat `add-tags-query\src\main\java\com\mycompany\app\App.java` dosyası

1. Yapı **ekleme-etiketleri-query** uygulama ve olan hataları düzeltin. Komut isteminizde gidin `add-tags-query` klasörü ve aşağıdaki komutu çalıştırın:

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a>Cihaz uygulaması oluşturma

Bu bölümde IOT hub'a gönderilen bir bildirilen özelliği değerini ayarlayan bir Java konsol uygulaması oluşturun.

1. İçinde `iot-java-twin-getstarted` klasörünü adlı bir Maven projesi oluşturun **simulated-device** , komut isteminde aşağıdaki komutu kullanarak. Bunun tek ve uzun bir komut olduğunu unutmayın:

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
    private static String deviceId = "myDeviceId";
    ```

    Bu örnek uygulama, bir **DeviceClient** nesnesi örneğini oluşturduğunda **prokotol** değişkenini kullanır. 

1. Aşağıdaki kodu ekleyin **ana** yöntemi için:
    * IOT Hub ile iletişim kurmak için bir cihaz istemcisi oluşturun.
    * Oluşturma bir **aygıt** cihaz çifti özelliklerini depolamak için nesne.

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

1. Aşağıdaki kodu ekleyin **ana** yöntemi oluşturmak için bir **connectivityType** özelliği bildirilen ve IOT Hub'ına gönderebilirsiniz:

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
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. Sonuna aşağıdaki kodu ekleyin **ana** yöntemi. Bekleyen **Enter** anahtar IOT Hub'ın cihaz çifti işlemlerinin durumunu raporlamak için zaman sağlar:

    ```java
    System.out.println("Press any key to exit...");

    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();

    dataCollector.clean();
    client.close();
    ```

1. Kaydet ve Kapat `simulated-device\src\main\java\com\mycompany\app\App.java` dosya.

1. Yapı **simulated-device** uygulama ve olan hataları düzeltin. Komut isteminizde gidin `simulated-device` klasörü ve aşağıdaki komutu çalıştırın:

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Şimdi konsol uygulamaları çalıştırmaya hazırsınız.

1. Bir komut isteminde `add-tags-query` klasörü çalıştırmak için aşağıdaki komutu çalıştırın, **ekleme-etiketleri-query** service uygulaması:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Etiket değerleri güncelleştirmek ve cihaz sorguları çalıştırmak için Java IOT Hub hizmeti uygulaması](media/iot-hub-java-java-twin-getstarted/service-app-1.png)

    Gördüğünüz **tesis** ve **bölge** cihaz çifti eklenen etiketler. İlk sorguyu Cihazınızı döndürür, ancak ikinci desteklemez.

1. Bir komut isteminde `simulated-device` klasörü eklemek için aşağıdaki komutu çalıştırın, **connectivityType** özelliği cihaz çifti bildirdi:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Aygıt istemcisi ekler ** connectivityType ** bildirilen özelliği](media/iot-hub-java-java-twin-getstarted/device-app-1.png)

1. Bir komut isteminde `add-tags-query` klasörü çalıştırmak için aşağıdaki komutu çalıştırın, **ekleme-etiketleri-query** service uygulaması ikinci kez:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Etiket değerleri güncelleştirmek ve cihaz sorguları çalıştırmak için Java IOT Hub hizmeti uygulaması](media/iot-hub-java-java-twin-getstarted/service-app-2.png)

    Cihazınızı gönderdi artık **connectivityType** özelliği IOT Hub'ına ikinci sorguyu Cihazınızı döndürür.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portalında yeni bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Cihaz meta verilerini etiketi gibi bir arka uç uygulamadan eklenen ve cihaz uygulaması rapor cihaz bağlantı bilgilerini cihaz çiftine yazıldı. Ayrıca SQL benzeri IOT hub'ı sorgu dili kullanarak cihaz çifti bilgileri sorgulamak öğrendiniz.

Bilgi edinmek için aşağıdaki kaynakları kullanın nasıl yapılır:

* Aygıtlarla telemetri gönderen [IOT Hub ile çalışmaya başlama](iot-hub-java-java-getstarted.md) Öğreticisi.
* Aygıtlarını etkileşimli olarak (örneğin, kullanıcı tarafından denetlenen bir uygulamadan fan etkinleştirdikten) ile [doğrudan yöntemleri kullanın](iot-hub-java-java-direct-methods.md) Öğreticisi.

<!-- Images. -->
[7]: ./media/iot-hub-java-java-twin-getstarted/invoke-method.png
[8]: ./media/iot-hub-java-java-twin-getstarted/device-listen.png
[9]: ./media/iot-hub-java-java-twin-getstarted/device-respond.png

<!-- Links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
