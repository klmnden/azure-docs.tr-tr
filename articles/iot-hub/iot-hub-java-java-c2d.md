---
title: Azure IOT hub'ı (Java) ile bulut-cihaz iletilerini | Microsoft Docs
description: Java için Azure IOT SDK'larını kullanarak bir Azure IOT hub'ından bir cihaza bulut-cihaz iletilerini göndermek nasıl. Bir sanal cihaz uygulamasının bulut-cihaz iletilerini ve bulut-cihaz iletilerini göndermek için bir arka uç uygulaması değiştirme değiştirin.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 06/28/2017
ms.openlocfilehash: 9032a6903833ba819e09fd1ca11cd6b43d5485cb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60399496"
---
# <a name="send-cloud-to-device-messages-with-iot-hub-java"></a>IOT hub'ı (Java) ile bulut buluttan cihaza iletileri gönderme

[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

Azure IOT hub'ı yardımcı olan tam olarak yönetilen bir hizmet, milyonlarca cihaz arasında güvenilir ve güvenli çift yönlü iletişimi etkinleştirmek ve bir çözüm arka ucu ' dir. [Telemetri bir CİHAZDAN bir merkeze gönderme (Java)](quickstart-send-telemetry-java.md) öğretici, IOT hub oluşturma, bir cihaz kimliği da sağlamak ve CİHAZDAN buluta iletiler gönderen bir sanal cihaz uygulamasının kodu nasıl gösterir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğreticide yapılar [telemetri gönderir bir CİHAZDAN IOT hub'a (Java)](quickstart-send-telemetry-java.md). Aşağıdakilerin nasıl gerçekleştirileceği gösterilmektedir:

* Çözüm arka ucunuz, tek bir cihaz IOT hub'ı aracılığıyla bulut-cihaz iletilerini gönderin.

* Bir cihazda bulut-cihaz iletilerini alır.

* Çözüm arka ucunuz, teslimat alındısı istek (*geri bildirim*) için bir cihaz IOT Hub'ından gönderilen iletileri.

Daha fazla bilgi bulabilirsiniz [IOT Hub Geliştirici Kılavuzu'nda bulut-cihaz iletilerini](iot-hub-devguide-messaging.md).

Bu öğreticinin sonunda iki Java konsol uygulaması çalıştırın:

* **simulated-device**, oluşturulan uygulamayı değiştirilmiş bir sürümünü [telemetri bir CİHAZDAN bir merkeze gönderme (Java)](quickstart-send-telemetry-java.md), IOT hub'ınıza bağlanır ve bulut-cihaz iletilerini alır.

* **Send-c2d-messages**, IOT hub'ı aracılığıyla sanal cihaz uygulaması için bir bulut buluttan cihaza ileti gönderir ve ardından, teslimat alındısı.

> [!NOTE]
> IOT Hub SDK desteği birçok cihaz platformlarını ve Azure IOT cihaz SDK'ları aracılığıyla diller (C, Java ve Javascript dahil) sahiptir. Bu öğreticinin koda ve genellikle Azure IOT hub'a Cihazınızı bağlamak hakkında adım adım yönergeler için bkz. [Azure IOT Geliştirici Merkezi](https://azure.microsoft.com/develop/iot).

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Çalışan bir bir tam sürümü [telemetri bir CİHAZDAN bir merkeze gönderme (Java)](quickstart-send-telemetry-java.md) veya [IOT Hub ile ileti yönlendirmeyi yapılandırma](tutorial-routing.md) öğretici.

* En güncel [Java SE Development Kit 8](https://aka.ms/azure-jdks)

* [Maven 3](https://maven.apache.org/install.html)

* Etkin bir Azure hesabı. Bir hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.

## <a name="receive-messages-in-the-simulated-device-app"></a>Sanal cihaz uygulamasında ileti alma

Bu bölümde, oluşturduğunuz sanal cihaz uygulaması değiştirme [telemetri bir CİHAZDAN bir merkeze gönderme (Java)](quickstart-send-telemetry-java.md) IOT hub'ından bulut-cihaz iletilerini almak için.

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
    > HTTPS MQTT veya AMQP yerine aktarım olarak kullanırsanız **DeviceClient** örneği iletileri IOT hub'dan seyrek (küçüktür 25 dakikada bir) denetler. MQTT, AMQP ve HTTPS desteği ve IOT hub'ı azaltma arasındaki farklar hakkında daha fazla bilgi için bkz. [bölümde IOT Hub Geliştirici kılavuzunun Mesajlaşma](iot-hub-devguide-messaging.md).

4. Maven kullanarak **simulated-device** uygulamasını oluşturmak için, simulated-device klasöründeki komut isteminde aşağıdaki komutu yürütün:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="send-a-cloud-to-device-message"></a>Bulut buluttan cihaza ileti gönderme

Bu bölümde, sanal cihaz uygulaması için bulut-cihaz iletilerini gönderen bir Java konsol uygulaması oluşturun. Cihazın kimliği, eklediğiniz ihtiyacınız [telemetri bir CİHAZDAN bir merkeze gönderme (Java)](quickstart-send-telemetry-java.md) hızlı başlangıç. IOT Hub bağlantı dizesine, bulabileceğiniz hub'ınız için etmeniz [Azure portalında](https://portal.azure.com).

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
    > En son sürümünü kontrol **IOT hizmeti istemcisi** kullanarak [Maven arama](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

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
    private static final IotHubServiceClientProtocol protocol =    
        IotHubServiceClientProtocol.AMQPS;
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

    ![Sanal cihaz uygulaması çalıştırma](./media/iot-hub-java-java-c2d/receivec2d.png)

2. Send-c2d-messages klasöründeki komut isteminde, bir geri bildirim için bekleyin ve bulut buluttan cihaza ileti göndermek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Bulut buluttan cihaza ileti göndermek için komutu çalıştırın](media/iot-hub-java-java-c2d/sendc2d.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bulut-cihaz iletilerini gönderip öğrendiniz. 

IOT hub'ı kullanan tam uçtan uca çözümler örneklerini görmek için bkz: [Azure IOT Çözüm Hızlandırıcıları](https://azure.microsoft.com/documentation/suites/iot-suite/).

IOT Hub ile çözümleri geliştirme hakkında daha fazla bilgi için bkz. [IOT Hub Geliştirici kılavuzunun](iot-hub-devguide.md).