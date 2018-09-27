---
title: Azure IOT hub'ı (Java) ile bulut-cihaz iletilerini | Microsoft Docs
description: Java için Azure IOT SDK'larını kullanarak bir Azure IOT hub'ından bir cihaza bulut-cihaz iletilerini göndermek nasıl. Bir sanal cihaz uygulamasının bulut-cihaz iletilerini ve bulut-cihaz iletilerini göndermek için bir arka uç uygulaması değiştirme değiştirin.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 06/28/2017
ms.author: dobett
ms.openlocfilehash: e4d0df28449a2e50e72b192f0118a8eae3325d15
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47220226"
---
# <a name="send-cloud-to-device-messages-with-iot-hub-java"></a>IOT hub'ı (Java) ile bulut buluttan cihaza iletileri gönderme
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

Azure IOT hub'ı yardımcı olan tam olarak yönetilen bir hizmet, milyonlarca cihaz arasında güvenilir ve güvenli çift yönlü iletişimi etkinleştirmek ve bir çözüm arka ucu ' dir. [IOT Hub ile çalışmaya başlama] öğretici, IOT hub oluşturma, bir cihaz kimliği da sağlamak ve CİHAZDAN buluta iletiler gönderen bir sanal cihaz uygulamasının kodu nasıl gösterir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğreticide yapılar [IOT Hub ile çalışmaya başlama]. Bunun nasıl yapılacağı anlatılmaktadır için:

* Çözüm arka ucunuz, tek bir cihaz IOT hub'ı aracılığıyla bulut-cihaz iletilerini gönderin.
* Bir cihazda bulut-cihaz iletilerini alır.
* Çözüm arka ucunuz, teslimat alındısı istek (*geri bildirim*) için bir cihaz IOT Hub'ından gönderilen iletileri.

Bulut-cihaz iletileri hakkında daha fazla bilgi bulabilirsiniz [IOT Hub Geliştirici kılavuzunun][IoT Hub developer guide - C2D].

Bu öğreticinin sonunda iki Java konsol uygulaması çalıştırın:

* **simulated-device**, oluşturulan uygulamayı değiştirilmiş bir sürümünü [IOT Hub ile çalışmaya başlama], IOT hub'ınıza bağlanır ve bulut-cihaz iletilerini alır.
* **Send-c2d-messages**, IOT hub'ı aracılığıyla sanal cihaz uygulaması için bir bulut buluttan cihaza ileti gönderir ve ardından, teslimat alındısı.

> [!NOTE]
> IOT Hub SDK desteği birçok cihaz platformlarını ve Azure IOT cihaz SDK'ları aracılığıyla diller (C, Java ve Javascript dahil) sahiptir. Bu öğreticinin koda ve genellikle Azure IOT hub'a Cihazınızı bağlamak hakkında adım adım yönergeler için bkz. [Azure IOT Geliştirici Merkezi].

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Çalışan bir bir tam sürümü [IOT Hub ile çalışmaya başlama](quickstart-send-telemetry-java.md) veya [işlem IOT Hub CİHAZDAN buluta iletileri](tutorial-routing.md) öğretici.
* En güncel [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

## <a name="receive-messages-in-the-simulated-device-app"></a>Sanal cihaz uygulamasında ileti alma

Bu bölümde, oluşturduğunuz sanal cihaz uygulaması değiştirme [IOT Hub ile çalışmaya başlama] IOT hub'ından bulut-cihaz iletilerini almak için.

1. Bir metin düzenleyicisi kullanarak simulated-device\src\main\java\com\mycompany\app\App.java dosyasını açın.

2. Aşağıdaki **MessageCallback** sınıf içinde iç içe geçmiş bir sınıf olarak **uygulama** sınıfı. **Yürütme** yöntemi, cihaz IOT Hub'ından ileti aldığında çağrılır. Bu örnekte, cihazın her zaman IOT hub ileti tamamlandığını bildirir:

    ```java
    private static class AppMessageCallback implements MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));
    
        return IotHubMessageResult.COMPLETE;
      }
    }
    ```
3. Değiştirme **ana** yöntemi oluşturmak için bir **AppMessageCallback** örneği ve çağrı **setMessageCallback** gibi istemci açılmadan önce yöntemi:

    ```java
    client = new DeviceClient(connString, protocol);
   
    MessageCallback callback = new AppMessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [!NOTE]
    > HTTPS MQTT veya AMQP yerine aktarım olarak kullanırsanız **DeviceClient** örneği iletileri IOT hub'dan seyrek (küçüktür 25 dakikada bir) denetler. MQTT, AMQP ve HTTPS desteği ve IOT hub'ı azaltma arasındaki farklar hakkında daha fazla bilgi için bkz. [IOT Hub Geliştirici kılavuzunun][IoT Hub developer guide - C2D].

4. Maven kullanarak **simulated-device** uygulamasını oluşturmak için, simulated-device klasöründeki komut isteminde aşağıdaki komutu yürütün:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="send-a-cloud-to-device-message"></a>Bulut buluttan cihaza ileti gönderme

Bu bölümde, sanal cihaz uygulaması için bulut-cihaz iletilerini gönderen bir Java konsol uygulaması oluşturun. Cihazın kimliği, eklediğiniz ihtiyacınız [IOT Hub ile çalışmaya başlama] öğretici. IOT Hub bağlantı dizesine, bulabileceğiniz hub'ınız için etmeniz [Azure portal].

1. Adlı bir Maven projesi oluşturun **c2d iletileri gönderme** komut isteminizde aşağıdaki komutu kullanarak. Bu komut, tek ve uzun bir komut olduğunu unutmayın:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Komut isteminizde yeni Gönder-c2d-messages klasörüne gidin.

3. Bir metin düzenleyicisi kullanarak Gönder-c2d-messages klasöründeki pom.xml dosyasını açın ve aşağıdaki bağımlılığı ekleyin **bağımlılıkları** düğümü. Bağımlılık ekleme kullanmanıza olanak sağlar **iothub-java-service-client** , IOT hub hizmetiyle iletişim kurmak için uygulama paketi:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > [Maven arama][lnk-maven-service-search] kullanarak en yeni **iot-service-client** sürümünü kontrol edebilirsiniz.

4. pom.xml dosyasını kaydedin ve kapatın.

5. Bir metin düzenleyicisi kullanarak send-c2d-messages\src\main\java\com\mycompany\app\App.java dosyasını açın.

6. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Aşağıdaki sınıf düzeyi değişkenleri ekleyip **uygulama** sınıfına **{yourhubconnectionstring}** ve **{yourdeviceid}** değerlerle daha önce not ettiğiniz:

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    ```

8. Değiştirin **ana** yöntemini aşağıdaki kod ile. Bu kod, IOT hub'ınıza bağlanır, cihazınıza bir ileti gönderir ve sonra cihaz aldı ve iletiyi işlenen bir bildirim bekler:
   
    ```java
    public static void main(String[] args) throws IOException,
        URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(
        connectionString, protocol);
   
      if (serviceClient != null) {
        serviceClient.open();
        FeedbackReceiver feedbackReceiver = serviceClient
          .getFeedbackReceiver();
        if (feedbackReceiver != null) feedbackReceiver.open();
   
        Message messageToSend = new Message("Cloud to device message.");
        messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);
   
        serviceClient.send(deviceId, messageToSend);
        System.out.println("Message sent to device");
   
        FeedbackBatch feedbackBatch = feedbackReceiver.receive(10000);
        if (feedbackBatch != null) {
          System.out.println("Message feedback received, feedback time: "
            + feedbackBatch.getEnqueuedTimeUtc().toString());
        }
   
        if (feedbackReceiver != null) feedbackReceiver.close();
        serviceClient.close();
      }
    }
    ```

    > [!NOTE]
    > Basitlik'ın çok için bu öğreticiyi herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda yeniden deneme ilkelerini (üstel geri alma), örneğin makalesinde önerildiği uygulamalıdır [geçici hata işleme](/azure/architecture/best-practices/transient-faults).


9. Maven kullanarak **simulated-device** uygulamasını oluşturmak için, simulated-device klasöründeki komut isteminde aşağıdaki komutu yürütün:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Simulated-device klasöründeki komut isteminde IOT hub'ınıza telemetri göndermeye başlamak ve bulut-cihaz iletilerini hub'ından gönderilen dinlemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Sanal cihaz uygulaması çalıştırma][img-simulated-device]

2. Send-c2d-messages klasöründeki komut isteminde, bir geri bildirim için bekleyin ve bulut buluttan cihaza ileti göndermek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Bulut buluttan cihaza ileti göndermek için komutu çalıştırın][img-send-command]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bulut-cihaz iletilerini gönderip öğrendiniz. 

IOT hub'ı kullanan tam uçtan uca çözümler örneklerini görmek için bkz: [Azure IOT Uzaktan izleme çözüm Hızlandırıcısını].

IOT Hub ile çözümleri geliştirme hakkında daha fazla bilgi için bkz. [IOT Hub Geliştirici Kılavuzu].

<!-- Images -->
[img-simulated-device]: media/iot-hub-java-java-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-java-java-c2d/sendc2d.png
<!-- Links -->

[IOT Hub ile çalışmaya başlama]: quickstart-send-telemetry-java.md
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
[IOT Hub Geliştirici Kılavuzu]: iot-hub-devguide.md
[Azure IoT Geliştirici Merkezi]: http://azure.microsoft.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java
[Azure portal]: https://portal.azure.com
[Azure IOT Uzaktan izleme çözüm Hızlandırıcısını]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22