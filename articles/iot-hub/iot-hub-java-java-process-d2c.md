---
title: "Azure IOT hub'ı (Java) yönlendirme iletilerle | Microsoft Docs"
description: "Diğer arka uç hizmetlerine iletilerinin gönderilmesi için yönlendirme kurallarını ve özel uç noktaları kullanarak Azure IOT Hub cihaz bulut iletilerini işlemek nasıl."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: bd9af5f9-a740-4780-a2a6-8c0e2752cf48
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: dobett
ms.openlocfilehash: 81f846e1fd8cca586613e6fc57737ec27e43a639
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="routing-messages-with-iot-hub-java"></a>IOT hub'ı (Java) ile ileti yönlendirme

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

Azure IOT Hub güvenilir sağlayan tam olarak yönetilen bir hizmettir ve arka uç milyonlarca cihaza ve çözüm arasında güvenli çift yönlü iletişim. Diğer öğreticiler ([IOT Hub ile çalışmaya başlama] ve [IOT Hub ile bulut cihaza ileti gönderme][lnk-c2d]) cihaz Bulut ve bulut-cihaz temel kullanmayı Göster IOT hub'ı işlevselliğini Mesajlaşma.

Bu öğreticide gösterilen kodu derlemeler [IOT Hub ile çalışmaya başlama] öğretici ve ileti yönlendirme ölçeklenebilir bir şekilde cihaz bulut iletilerini işlemek için nasıl kullanılacağını gösterir. Öğretici, çözüm arka uçtan Acil eylem gerekli iletileri işlemek göstermektedir. Örneğin, bir aygıt bir bilet bir CRM sistemine ekleme tetikleyen bir uyarı iletisi gönderebilir. Bunun aksine, veri noktası iletileri yalnızca bir analytics motoruna akış. Örneğin, sonraki analize depolanacağı bir aygıttan sıcaklık telemetri, bir veri noktası iletisidir.

Bu öğreticinin sonunda üç Java konsol uygulamaları çalıştırın:

* **simulated-device**, oluşturduğunuz uygulama değiştirilmiş bir sürümünü [IOT Hub ile çalışmaya başlama] öğretici, saniyede veri noktası cihaz bulut iletilerini gönderir ve etkileşimli cihaz bulut iletilerini her 10 saniye . Bu uygulama, IOT Hub ile iletişim kurmak için AMQP protokolünü kullanır.
* **Read-d2c-messages** cihaz uygulamanız tarafından gönderilen telemetriyi görüntüler.
* **Okuma kritik-sıra** IOT hub'ına bağlı Service Bus kuyruğundan kritik iletileri çıkarır.

> [!NOTE]
> IOT hub'ı birçok cihaz platformları ve C, Java ve JavaScript gibi diller için SDK desteğe sahiptir. Cihaz Bu öğreticide bir fiziksel aygıt ile değiştirme ve aygıtları bir IOT Hub'ına bağlanmak hakkında yönergeler için bkz: [Azure IOT Geliştirme Merkezi].

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Eksiksiz bir çalışma sürümü [IOT Hub ile çalışmaya başlama] Öğreticisi.
* En güncel [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

Ayrıca okumayı öneririz [Azure Storage] ve [Azure Service Bus].

## <a name="send-interactive-messages-from-a-device-app"></a>Bir aygıt uygulamadan etkileşimli ileti gönderme
Bu bölümde, oluşturduğunuz cihaz uygulamayı değiştirmek [IOT Hub ile çalışmaya başlama] bazen hemen işlem iletileri göndermek için öğretici.

1. Simulated-device\src\main\java\com\mycompany\app\App.java dosyasını açmak için bir metin düzenleyicisi kullanın. Bu dosya için kod içeren **simulated-device** oluşturduğunuz uygulama [IOT Hub ile çalışmaya başlama] Öğreticisi.

2. Değiştir **MessageSender** aşağıdaki kodla sınıfı:

    ```java
    private static class MessageSender implements Runnable {

        public void run()  {
            try {
                double minTemperature = 20;
                double minHumidity = 60;
                Random rand = new Random();

                while (true) {
                    String msgStr;
                    Message msg;
                    if (new Random().nextDouble() > 0.7) {
                        if (new Random().nextDouble() > 0.5) {
                            msgStr = "This is a critical message.";
                            msg = new Message(msgStr);
                            msg.setProperty("level", "critical");
                        } else {
                            msgStr = "This is a storage message.";
                            msg = new Message(msgStr);
                            msg.setProperty("level", "storage");
                        }
                    } else {
                        double currentTemperature = minTemperature + rand.nextDouble() * 15;
                        double currentHumidity = minHumidity + rand.nextDouble() * 20; 
                        TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
                        telemetryDataPoint.deviceId = deviceId;
                        telemetryDataPoint.temperature = currentTemperature;
                        telemetryDataPoint.humidity = currentHumidity;

                        msgStr = telemetryDataPoint.serialize();
                        msg = new Message(msgStr);
                    }
                    
                    System.out.println("Sending: " + msgStr);

                    Object lockobj = new Object();
                    EventCallback callback = new EventCallback();
                    client.sendEventAsync(msg, callback, lockobj);

                    synchronized (lockobj) {
                        lockobj.wait();
                    }
                    Thread.sleep(1000);
                }
            } catch (InterruptedException e) {
                System.out.println("Finished.");
            }
        }
    }
    ```
   
    Bu yöntem rastgele özelliği ekler `"level": "critical"` ve `"level": "storage"` aygıt tarafından gönderilen iletiler için hangi benzetimini yapar, uygulama arka uç ya da kalıcı olarak depolanması gereken bir Acil eylem gerektiren bir ileti. Bu IOT hub'ın uygun mesajı hedefe ileti yönlendirmek için uygulama bu bilgileri ileti özelliklerinde yerine ileti gövdesinde geçirir.
   
   > [!NOTE]
   > Burada gösterilen etkin yolunuzda örnek yanı sıra soğuk yolu işleme dahil olmak üzere çeşitli senaryoları için ileti özellikleri iletileri yönlendirmek için kullanabilirsiniz.

2. Simulated-device\src\main\java\com\mycompany\app\App.java dosyasını kaydedip kapatın.

    > [!NOTE]
    > MSDN makalesinde önerilen üstel geri alma gibi bir yeniden deneme ilkesi uygulamak önerilir [geçici hata işleme].

3. Maven kullanarak **simulated-device** uygulamasını oluşturmak için, simulated-device klasöründeki komut isteminde aşağıdaki komutu yürütün:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="add-a-queue-to-your-iot-hub-and-route-messages-to-it"></a>IOT hub ve rota iletileri ona bir kuyruk ekleyin

Bu bölümde, hizmet veri yolu kuyruğu oluşturma, IOT hub'ınıza bağlanın ve ileti özellikte varlığına dayalı kuyruğa ileti göndermek için IOT hub'ınızı yapılandırma. Hizmet veri yolu sıralardan işlem iletileri hakkında daha fazla bilgi için bkz: [kuyruklarla çalışmaya başlama][lnk-sb-queues-java].

1. Service Bus kuyruğuna açıklandığı gibi oluşturmak [kuyruklarla çalışmaya başlama][lnk-sb-queues-java]. Ad alanı ve sıra adını not edin.

2. Azure portalında, IOT hub'ınızı açın ve **uç noktaları**.

    ![IOT hub uç noktaları][30]

3. İçinde **uç noktaları** dikey penceresinde tıklatın **Ekle** sıranız IOT hub'ınıza eklemek için üst. Uç nokta adı **CriticalQueue** ve seçmek için açılır listeleri kullanın **Service Bus kuyruğuna**, sıranız bulunduğu hizmet veri yolu ad alanı ve sıranız adı. İşiniz bittiğinde tıklatın **kaydetmek** altındaki.

    ![Bir uç nokta ekleme][31]

4. Şimdi **yollar** IOT hub'ınızdaki. Tıklatın **Ekle** iletileri kuyruğa yönlendiren bir yönlendirme kuralı oluşturmak için dikey pencerenin üstündeki eklediğiniz. Seçin **DeviceTelemetry** veri kaynağı olarak. Girin `level="critical"` koşulu olarak ve yeni eklediğiniz yönlendirme kuralı uç noktası olarak özel bir uç noktası olarak sıra'yı seçin. İşiniz bittiğinde tıklatın **kaydetmek** altındaki.

    ![Bir yol ekleme][32]

    Emin olun geri dönüş rota ayarlanmış **ON**. Bu ayar, bir IOT hub'ın varsayılan yapılandırmadır.

    ![Geri dönüş yolu][33]

## <a name="optional-read-from-the-queue-endpoint"></a>(İsteğe bağlı) Sıra uç noktasından okumak

Yönergeleri izleyerek iletileri sıra uç noktasından isteğe bağlı olarak okuyabilir [kuyruklarla çalışmaya başlama][lnk-sb-queues-java]. Uygulama adı **okuma kritik-sıra**.

## <a name="run-the-applications"></a>Uygulamaları çalıştırma

Artık üç uygulamaları çalıştırmak hazırsınız.

1. Çalıştırmak için **read-d2c-messages** uygulamasında, bir komut istemi veya kabuk read-d2c klasörüne gidin ve aşağıdaki komutu yürütün:

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```

   ![Read-d2c-messages çalıştırın][readd2c]

2. Çalıştırmak için **okuma kritik-sıra** uygulamasında, bir komut istemi veya kabuk okuma kritik-sıra klasörüne gidin ve aşağıdaki komutu yürütün:

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```
   
   ![Read-kritik-messages çalıştırın][readqueue]

3. Çalıştırmak için **simulated-device** uygulamasında, bir komut istemi veya kabuk simulated-device klasörüne gidin ve aşağıdaki komutu yürütün:

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```
   
   ![Simulated-device çalıştırın][simulateddevice]

## <a name="optional-add-storage-container-to-your-iot-hub-and-route-messages-to-it"></a>(İsteğe bağlı) Depolama kapsayıcısı IOT hub ve rota iletileri ona ekleyin

Bu bölümde, depolama hesabı oluşturma, IOT hub'ınıza bağlanın ve iletiyi bir özellik varlığına dayalı hesabına iletileri göndermek için IOT hub'ınızı yapılandırma. Depolama yönetme hakkında daha fazla bilgi için bkz: [Azure Storage ile çalışmaya başlama][Azure Storage].

 > [!NOTE]
   > Birle sınırlı değilse **Endpoint**, Kurulum **StorageContainer** ek olarak **CriticalQueue** ve her iki simulatneously çalıştırın.

1. [Azure depolama belgelerinde] [lnk-depolama] açıklandığı gibi bir depolama hesabı oluşturun. Hesap adını not edin.

2. Azure portalında, IOT hub'ınızı açın ve **uç noktaları**.

3. İçinde **uç noktaları** dikey penceresinde, select **CriticalQueue** uç noktası ve tıklatın **silmek**. Tıklatın **Evet**ve ardından **Ekle**. Uç nokta adı **StorageContainer** ve seçmek için açılır listeleri kullanın **Azure depolama kapsayıcısının**ve oluşturma bir **depolama hesabı** ve **depolama kapsayıcı**.  Adlarını not edin.  İşiniz bittiğinde tıklatın **Tamam** altındaki. 

 > [!NOTE]
   > Birle sınırlı değilse **Endpoint**, silme gerekmez **CriticalQueue**.

4. Tıklatın **yollar** IOT hub'ınızdaki. Tıklatın **Ekle** iletileri kuyruğa yönlendiren bir yönlendirme kuralı oluşturmak için dikey pencerenin üstündeki eklediğiniz. Seçin **cihaz iletilerini** veri kaynağı olarak. Girin `level="storage"` koşul olarak seçin **StorageContainer** yönlendirme kuralı uç noktası olarak özel bir uç noktası olarak. Tıklatın **kaydetmek** altındaki.  

    Emin olun geri dönüş rota ayarlanmış **ON**. Bu ayar, bir IOT hub'ın varsayılan yapılandırmadır.

1. Önceki uygulamalarınızı hala çalıştığından emin olun. 

1. Azure Portalı'nda altında depolama hesabınıza gidin **Blob hizmeti**, tıklatın **BLOB'lar Gözat...** .  Kapsayıcıyı seçin, gidin ve JSON dosyasına tıklayın ve tıklayın **karşıdan** verileri görüntülemek için.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, IOT Hub'ının ileti yönlendirme işlevini kullanarak cihaz bulut iletilerini güvenilir bir şekilde gönderme öğrendiniz.

[IOT Hub ile bulut-cihaz iletilerini göndermek nasıl] [ lnk-c2d] aygıtlarınıza çözüm arka ucu iletileri göndermek nasıl gösterir.

IOT hub'ı kullanan tam uçtan uca çözümler örnekleri görmek için bkz: [Azure IOT paketi][lnk-suite].

IOT Hub ile çözümleri geliştirme hakkında daha fazla bilgi için bkz: [IOT Hub Geliştirici Kılavuzu].

IOT Hub içinde ileti yönlendirme hakkında daha fazla bilgi için bkz: [IOT Hub ile iletileri almasına ve göndermesine][lnk-devguide-messaging].

<!-- Images. -->
<!-- TODO: UPDATE PICTURES -->
[simulateddevice]: ./media/iot-hub-java-java-process-d2c/runsimulateddevice.png
[readd2c]: ./media/iot-hub-java-java-process-d2c/runprocessinteractive.png
[readqueue]: ./media/iot-hub-java-java-process-d2c/runprocessd2c.png

[30]: ./media/iot-hub-java-java-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-java-java-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-java-java-process-d2c/route-creation.png
[33]: ./media/iot-hub-java-java-process-d2c/fallback-route.png

<!-- Links -->

[lnk-sb-queues-java]: ../service-bus-messaging/service-bus-java-how-to-use-queues.md

[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[IOT Hub Geliştirici Kılavuzu]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[IOT Hub ile çalışmaya başlama]: iot-hub-java-java-getstarted.md
[Azure IOT Geliştirme Merkezi]: https://azure.microsoft.com/develop/iot
[geçici hata işleme]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Geçici hata işleme]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-c2d]: iot-hub-java-java-c2d.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-free-trial]: https://azure.microsoft.com/free/
