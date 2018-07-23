---
title: Java ile Azure IOT Hub'ına cihazlardan dosya yükleme | Microsoft Docs
description: Java için Azure IOT cihaz SDK'sını kullanarak bulutta bir CİHAZDAN dosyaları karşıya yükleme. Karşıya yüklenen dosyaları bir Azure depolama blob kapsayıcısında depolanır.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 06/28/2017
ms.author: dobett
ms.openlocfilehash: 165c6ed53b611ba6b4f56260f20069661cf22da7
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39186568"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub"></a>Cihazınızı IOT Hub ile buluta dosyaları karşıya yükleme

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Bu öğreticide kodda geliştirir [IOT Hub ile bulut buluttan cihaza ileti gönderme](iot-hub-java-java-c2d.md) nasıl kullanılacağını göstermek için öğretici [dosya karşıya yükleme özellikleri IOT Hub'ın](iot-hub-devguide-file-upload.md) bir dosyayı karşıya yüklemek için [Azure blob Depolama](../storage/index.yml). Öğretici şunların nasıl yapıldığını göstermektedir:

- Bir cihaz Azure ile güvenli bir şekilde sağlayan bir dosya karşıya yükleme için URI blob.
- IOT hub'ı dosya karşıya yükleme bildirimlerini, uygulama arka ucu dosyasında bir işlem tetiklemek için kullanın.

[IOT Hub ile çalışmaya başlama](quickstart-send-telemetry-java.md) ve [IOT Hub ile bulut buluttan cihaza ileti gönderme](iot-hub-java-java-c2d.md) öğreticiler, IOT Hub'ın temel CİHAZDAN buluta ve bulut-cihaz Mesajlaşma işlevlerini gösterir. [İşlem CİHAZDAN buluta iletileri](tutorial-routing.md) Öğreticisi, CİHAZDAN buluta iletileri Azure blob depolama alanında güvenilir bir şekilde depolamak için bir yol açıklar. Ancak, bazı senaryolarda cihazlarınızı IOT hub'ı kabul görece küçük bir CİHAZDAN buluta ileti gönderme verileri kolayca eşlenemiyor. Örneğin:

* Görüntüleri içeren büyük dosyaları
* Videolar
* Titreşim veri yüksek sıklıkta örneklenir
* Önceden işlenmiş verilerin bazı formlarıyla.

Bu dosyalar genellikle toplu işleme gibi araçları kullanarak bulutta olduğu [Azure Data Factory](../data-factory/introduction.md) veya [Hadoop](../hdinsight/index.yml) yığını. Bir CİHAZDAN upland dosyalara ihtiyacınız olduğunda, güvenlik ve güvenilirlik IOT hub'ı kullanmaya devam edebilirsiniz.

Bu öğreticinin sonunda iki Java konsol uygulaması çalıştırın:

* **simulated-device**, [IOT Hub ile gönderme bulut-cihaz iletilerini] öğreticisinde oluşturulan uygulamanın değiştirilmiş bir sürümüdür. Bu uygulama bir dosya depolama, IOT hub tarafından sağlanan bir SAS URI'sini kullanarak yükler.
* **dosya karşıya yükleme bildirimini oku**, IOT hub'ından dosya karşıya yükleme bildirimleri alır.

> [!NOTE]
> IOT Hub, Azure IOT cihaz SDK'ları birçok cihaz platformlarını ve dilini (C, .NET ve Javascript gibi) destekler. Başvurmak [Azure IOT Geliştirici Merkezi] Cihazınızı Azure IOT Hub'ına bağlanmak adım adım yönergeler için.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* En güncel [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Etkin bir Azure hesabı. (Hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](http://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.)

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Bir cihaz uygulamasından bir dosyayı karşıya yükleyin

Bu bölümde oluşturduğunuz cihaz uygulamasını değiştirmek [IOT Hub ile bulut buluttan cihaza ileti gönderme](iot-hub-java-java-c2d.md) IOT hub'ına bir dosyayı karşıya yüklemek için.

1. Bir görüntü dosyasına kopyalama `simulated-device` klasörü ve yeniden adlandırmak `myimage.png`.

1. Bir metin düzenleyicisi kullanarak açın `simulated-device\src\main\java\com\mycompany\app\App.java` dosya.

1. Değişken bildirimi olarak ekleme **uygulama** sınıfı:

    ```java
    private static String fileName = "myimage.png";
    ```

1. Dosya karşıya yükleme durumu geri çağırma iletileri işlemek için aşağıdaki iç içe geçmiş sınıf için ekleme **uygulama** sınıfı:

    ```java
    // Define a callback method to print status codes from IoT Hub.
    protected static class FileUploadStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to file upload for " + fileName
            + " operation with status " + status.name());
      }
    }
    ```

1. IOT Hub'ına görüntüleri karşıya yüklemek için aşağıdaki yöntemi ekleyin. **uygulama** sınıfı IOT Hub'ına görüntüleri karşıya yüklemek için:

    ```java
    // Use IoT Hub to upload a file asynchronously to Azure blob storage.
    private static void uploadFile(String fullFileName) throws FileNotFoundException, IOException
    {
      File file = new File(fullFileName);
      InputStream inputStream = new FileInputStream(file);
      long streamLength = file.length();

      client.uploadToBlobAsync(fileName, inputStream, streamLength, new FileUploadStatusCallBack(), null);
    }
    ```

1. Değiştirme **ana** çağrılacak yöntem **uploadFile** aşağıdaki kod parçacığında gösterildiği gibi yöntemi:

    ```java
    client.open();

    try
    {
      // Get the filename and start the upload.
      String fullFileName = System.getProperty("user.dir") + File.separator + fileName;
      uploadFile(fullFileName);
      System.out.println("File upload started with success");
    }
    catch (Exception e)
    {
      System.out.println("Exception uploading file: " + e.getCause() + " \nERROR: " + e.getMessage());
    }

    MessageSender sender = new MessageSender();
    ```

1. Oluşturmak için aşağıdaki komutu kullanın **simulated-device** uygulama ve hatalar için denetleyin:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="receive-a-file-upload-notification"></a>Dosya karşıya yükleme bildirim alma

Bu bölümde, IOT Hub'ından dosya karşıya yükleme bildirim iletileri alan bir Java konsol uygulaması oluşturun.

Gereksinim duyduğunuz **iothubowner** bu bölümünü tamamlamak IOT Hub'ınız için bağlantı dizesi. Bağlantı dizesinde bulabilirsiniz [Azure portalında](https://portal.azure.com/) üzerinde **paylaşılan erişim ilkesi** dikey penceresi.

1. Adlı bir Maven projesi oluşturun **dosya karşıya yükleme bildirimini oku** komut isteminizde aşağıdaki komutu kullanarak. Bu komut, tek ve uzun bir komut olduğunu unutmayın:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-file-upload-notification -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

1. Komut isteminizde yeni gidin `read-file-upload-notification` klasör.

1. Bir metin düzenleyicisi kullanarak açın `pom.xml` dosyası `read-file-upload-notification` klasörü ve aşağıdaki bağımlılığı ekleyin **bağımlılıkları** düğümü. Bağımlılık ekleme kullanmanıza olanak sağlar **iothub-java-service-client** , IOT hub hizmetiyle iletişim kurmak için uygulama paketi:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > En son sürümünü kontrol **IOT hizmeti istemcisi** kullanarak [Maven arama](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

1. Kaydet ve Kapat `pom.xml` dosya.

1. Bir metin düzenleyicisi kullanarak açın `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` dosya.

1. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    ```

1. Aşağıdaki sınıf düzeyi değişkenleri ekleyip **uygulama** sınıfı:

    ```java
    private static final String connectionString = "{Your IoT Hub connection string}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    private static FileUploadNotificationReceiver fileUploadNotificationReceiver = null;
    ```

1. Konsola dosya karşıya yükleme hakkında bilgi yazdırmak için aşağıdaki iç içe geçmiş sınıf için ekleme **uygulama** sınıfı:

    ```java
    // Create a thread to receive file upload notifications.
    private static class ShowFileUploadNotifications implements Runnable {
      public void run() {
        try {
          while (true) {
            System.out.println("Recieve file upload notifications...");
            FileUploadNotification fileUploadNotification = fileUploadNotificationReceiver.receive();
            if (fileUploadNotification != null) {
              System.out.println("File Upload notification received");
              System.out.println("Device Id : " + fileUploadNotification.getDeviceId());
              System.out.println("Blob Uri: " + fileUploadNotification.getBlobUri());
              System.out.println("Blob Name: " + fileUploadNotification.getBlobName());
              System.out.println("Last Updated : " + fileUploadNotification.getLastUpdatedTimeDate());
              System.out.println("Blob Size (Bytes): " + fileUploadNotification.getBlobSizeInBytes());
              System.out.println("Enqueued Time: " + fileUploadNotification.getEnqueuedTimeUtcDate());
            }
          }
        } catch (Exception ex) {
          System.out.println("Exception reading reported properties: " + ex.getMessage());
        }
      }
    }
    ```

1. Dosya karşıya yükleme bildirimleri için bekleyen iş parçacığı başlatmak için aşağıdaki kodu ekleyin. **ana** yöntemi:

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(connectionString, protocol);

      if (serviceClient != null) {
        serviceClient.open();

        // Get a file upload notification receiver from the ServiceClient.
        fileUploadNotificationReceiver = serviceClient.getFileUploadNotificationReceiver();
        fileUploadNotificationReceiver.open();

        // Start the thread to receive file upload notifications.
        ShowFileUploadNotifications showFileUploadNotifications = new ShowFileUploadNotifications();
        ExecutorService executor = Executors.newFixedThreadPool(1);
        executor.execute(showFileUploadNotifications);

        System.out.println("Press ENTER to exit.");
        System.in.read();
        executor.shutdownNow();
        System.out.println("Shutting down sample...");
        fileUploadNotificationReceiver.close();
        serviceClient.close();
      }
    }
    ```

1. Kaydet ve Kapat `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` dosya.

1. Oluşturmak için aşağıdaki komutu kullanın **dosya karşıya yükleme bildirimini oku** uygulama ve hatalar için denetleyin:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

Bir komut isteminde `read-file-upload-notification` klasörü, aşağıdaki komutu çalıştırın:

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

Bir komut isteminde `simulated-device` klasörü, aşağıdaki komutu çalıştırın:

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

Aşağıdaki ekran görüntüsünde çıktısında **simulated-device** uygulama:

![Simulated-device uygulama çıktısı](media/iot-hub-java-java-upload/simulated-device.png)

Aşağıdaki ekran görüntüsünde çıktısında **dosya karşıya yükleme bildirimini oku** uygulama:

![Dosya karşıya yükleme bildirimini okuma uygulama çıktısı](media/iot-hub-java-java-upload/read-file-upload-notification.png)

Karşıya yüklenen dosya yapılandırdığınız depolama kapsayıcısında görüntülemek için portalı kullanabilirsiniz:

![Karşıya yüklenen dosya](media/iot-hub-java-java-upload/uploaded-file.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, cihazlardan karşıya dosya yükleme işlemleri basitleştirmek için dosya karşıya yükleme özellikleri IOT hub'ı kullanmayı öğrendiniz. IOT hub özelliklerini ve aşağıdaki makalelerde senaryolarını keşfetmeye devam edebilirsiniz:

* [Programlamalı IOT hub oluşturma][lnk-create-hub]
* [C SDK'ya giriş][lnk-c-sdk]
* [Azure IOT SDK'ları][lnk-sdks]

Daha fazla IOT Hub'ın özelliklerini keşfetmek için bkz:

* [IOT Edge ile cihaz benzetimi][lnk-iotedge]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/file-upload-project-csharp1.png
[3]: ./media/iot-hub-csharp-csharp-file-upload/enable-file-notifications.png

<!-- Links -->



[Azure IoT Geliştirici Merkezi]: http://azure.microsoft.com/develop/iot

[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure Storage]:../storage/common/storage-create-storage-account.md#create-a-storage-account
[lnk-configure-upload]: iot-hub-configure-file-upload.md
[Azure IoT service SDK NuGet package]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md


