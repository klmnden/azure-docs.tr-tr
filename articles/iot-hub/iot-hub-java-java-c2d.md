---
title: "Azure IOT hub'ı (Java) sahip bulut-cihaz iletilerini | Microsoft Docs"
description: "Java için Azure IOT SDK'ları kullanarak bir Azure IOT hub'dan bir aygıta bulut-cihaz iletilerini göndermek nasıl. Bulut-cihaz iletilerini ve bulut-cihaz iletilerini göndermek için bir arka uç uygulaması değiştirmek için bir sanal cihaz uygulamasının değiştirin."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 7f785ea8-e7c2-40c5-87ef-96525e9b9e1e
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: dobett
ms.openlocfilehash: 6a5f14f411c2ec82478fef6d20d22f8b8dc8d7bf
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="send-cloud-to-device-messages-with-iot-hub-java"></a>IOT hub'ı (Java) sahip bulut-cihaz iletilerini gönder
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

Azure IOT Hub, milyonlarca cihaza arasında güvenilir ve güvenli çift yönlü iletişimler sağlayan tam olarak yönetilen hizmet etkinleştirin ve bir çözüm arka ucu ' dir. [IOT Hub ile çalışmaya başlama] öğretici, IOT hub'ı oluşturma, bir cihaz kimliği, sağlama ve cihaz-bulut iletileri gönderen bir sanal cihaz uygulamasının kodu gösterir.

Bu öğretici derlemeler [IOT Hub ile çalışmaya başlama]. Size bir nasıl gösterir için:

* Çözüm arka ucunuz, tek bir cihaz IOT hub'ı aracılığıyla bulut cihaz ileti gönderebilir.
* Bir cihazda bulut-cihaz iletilerini alır.
* Çözüm arka ucunuz, teslimat alındısı iste (*geri bildirim*) için bir cihaz IOT Hub'ından gönderilen iletiler için.

Bulut cihaz iletileri hakkında daha fazla bilgi bulabilirsiniz [IOT Hub Geliştirici Kılavuzu][IoT Hub developer guide - C2D].

Bu öğreticinin sonunda iki Java konsol uygulamaları çalıştırın:

* **simulated-device**, oluşturduğunuz uygulama değiştirilmiş bir sürümünü [IOT Hub ile çalışmaya başlama], IOT hub'ınıza bağlanır ve bulut-cihaz iletilerini alır.
* **Send-c2d-messages**, IOT hub'ı aracılığıyla sanal cihaz uygulamasının bir bulut cihaz ileti gönderir ve teslimat alındısı alır.

> [!NOTE]
> IOT Hub SDK desteği birçok cihaz platformları ve Azure IOT cihaz SDK'ları aracılığıyla dilleri (C, Java ve Javascript dahil) sahiptir. Bu öğreticinin kod ve genellikle Azure IOT Hub Cihazınızı bağlamak hakkında adım adım yönergeler için bkz: [Azure IOT Geliştirme Merkezi].

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Eksiksiz bir çalışma sürümü [IOT Hub ile çalışmaya başlama](iot-hub-java-java-getstarted.md) veya [işlem IOT Hub cihaz bulut iletilerini](iot-hub-java-java-process-d2c.md) Öğreticisi.
* En güncel [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

## <a name="receive-messages-in-the-simulated-device-app"></a>Sanal cihaz uygulamasının ileti alma

Bu bölümde, oluşturduğunuz sanal cihaz uygulamasının değiştirme [IOT Hub ile çalışmaya başlama] IOT hub'ından bulut cihaz iletileri almak için.

1. Bir metin düzenleyicisi kullanarak simulated-device\src\main\java\com\mycompany\app\App.java dosyasını açın.

2. Aşağıdakileri ekleyin **MessageCallback** sınıfı içinde iç içe bir sınıf olarak **uygulama** sınıfı. **Yürütme** yöntemi cihaz IOT Hub'ından bir ileti aldığında çağrılır. Bu örnekte, cihazın her zaman IOT hub'ı ileti tamamlandığını bildirir:

    ```java
    private static class AppMessageCallback implements MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));
    
        return IotHubMessageResult.COMPLETE;
      }
    }
    ```
3. Değiştirme **ana** yöntemi oluşturmak için bir **AppMessageCallback** örneği ve çağrısı **setMessageCallback** istemci gibi açılmadan önce yöntemi:

    ```java
    client = new DeviceClient(connString, protocol);
   
    MessageCallback callback = new AppMessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [!NOTE]
    > HTTPS MQTT veya AMQP yerine taşıma olarak kullanırsanız **DeviceClient** seyrek IOT Hub (değerinden 25 dakikada bir) gelen iletileri örneği denetler. MQTT, AMQP ve HTTPS desteği ve IOT hub'ı azaltma arasındaki farklar hakkında daha fazla bilgi için bkz: [IOT Hub Geliştirici Kılavuzu][IoT Hub developer guide - C2D].

4. Maven kullanarak **simulated-device** uygulamasını oluşturmak için, simulated-device klasöründeki komut isteminde aşağıdaki komutu yürütün:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="send-a-cloud-to-device-message"></a>Bulut cihaza ileti gönderme

Bu bölümde, sanal cihaz uygulamasının bulut cihaz iletileri gönderen bir Java konsol uygulaması oluşturun. Eklediğiniz aygıt cihaz Kimliğini gereksinim [IOT Hub ile çalışmaya başlama] Öğreticisi. Ayrıca bulabilirsiniz, hub'ın IOT Hub bağlantı dizesine gerekir [Azure portal].

1. Adlı bir Maven projesi oluşturun **c2d iletileri gönderme** , komut isteminde aşağıdaki komutu kullanarak. Bu komut tek ve uzun bir komut olduğuna dikkat edin:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Komut isteminizde yeni c2d iletileri gönderme klasöre gidin.

3. Bir metin düzenleyicisi kullanarak, gönderme-c2d-messages klasöründeki pom.xml dosyasını açın ve aşağıdaki bağımlılığı eklemek **bağımlılıkları** düğümü. Bağımlılık ekleme kullanabilmenizi sağlar **iothub-java-service-client** IOT hub hizmeti ile iletişim kurmak için uygulamanızda paketi:

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

7. Aşağıdaki sınıf düzeyi değişkenleri ekleyip **uygulama** sınıfına **{yourhubconnectionstring}** ve **{yourdeviceid}** değerlerle, daha önce not ettiğiniz:

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    ```

8. Değiştir **ana** aşağıdaki kod ile yöntemi. Bu kod, IOT hub'ınıza bağlanır, cihazınıza bir ileti gönderir ve cihaz aldı ve iletiyi işlenen bir bildirim bekler:
   
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
    > Basitlik'ın artırmak amacıyla için bu öğreticiyi herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda yeniden deneme ilkelerini (üstel geri alma), önerilen MSDN makalesinde uygulamalıdır [geçici hata işleme].


9. Maven kullanarak **simulated-device** uygulamasını oluşturmak için, simulated-device klasöründeki komut isteminde aşağıdaki komutu yürütün:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Simulated-device klasöründeki bir komut isteminde IOT hub'ınıza telemetri göndermeye başlamak ve hub'ından gönderilen bulut-cihaz iletilerini dinlemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Sanal cihaz uygulamasının çalıştırın][img-simulated-device]

2. Send-c2d-messages klasöründeki komut isteminde, bulut cihaza ileti gönderme ve geri bildirim için beklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Bulut cihaza ileti göndermek için kullanılan komut çalıştırın][img-send-command]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici kapsamında, bulut cihaza ileti gönderme ve alma öğrendiniz. 

IOT hub'ı kullanan tam uçtan uca çözümler örnekleri görmek için bkz: [Azure IOT paketi].

IOT Hub ile çözümleri geliştirme hakkında daha fazla bilgi için bkz: [IOT Hub Geliştirici Kılavuzu].

<!-- Images -->
[img-simulated-device]: media/iot-hub-java-java-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-java-java-c2d/sendc2d.png
<!-- Links -->

[IOT Hub ile çalışmaya başlama]: iot-hub-java-java-getstarted.md
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
[IOT Hub Geliştirici Kılavuzu]: iot-hub-devguide.md
[Azure IOT Geliştirme Merkezi]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java
[geçici hata işleme]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure portal]: https://portal.azure.com
[Azure IOT paketi]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22