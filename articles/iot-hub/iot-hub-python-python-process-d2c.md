---
title: "Azure IOT hub'ı (Python) yönlendirme iletilerle | Microsoft Docs"
description: "Diğer arka uç hizmetlerine iletilerinin gönderilmesi için yönlendirme kurallarını ve özel uç noktaları kullanarak Azure IOT Hub cihaz bulut iletilerini işlemek nasıl."
services: iot-hub
documentationcenter: python
author: msebolt
manager: timlt
editor: 
ms.assetid: bd9af5f9-a740-4780-a2a6-8c0e2752cf48
ms.service: iot-hub
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/22/2018
ms.author: v-masebo
ms.openlocfilehash: f467437afb4bf89e77668cfd3e8a824bfbde9e10
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="routing-messages-with-iot-hub-python"></a>IOT hub'ı (Python) ile ileti yönlendirme

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

Bu öğretici derlemeler [IOT Hub ile çalışmaya başlama] Öğreticisi.  Öğretici:

* Cihaz bulut iletilerini kolay, yapılandırma tabanlı bir şekilde gönderilmesi için yönlendirme kurallarını kullanmayı gösterir.
* Daha ayrıntılı işleme için çözüm arka uçtan Acil eylem gerekli etkileşimli iletilerin yalıtmak nasıl gösterilmektedir.  Örneğin, bir aygıt bir bilet bir CRM sistemine ekleme tetikleyen bir uyarı iletisi gönderebilir.  Buna karşılık, sıcaklık telemetri gibi veri noktası iletileri analytics motoruna akış.

Bu öğreticinin sonunda üç Python konsol uygulamaları çalıştırın:

* **SimulatedDevice.py**, oluşturduğunuz uygulama değiştirilmiş bir sürümünü [IOT Hub ile çalışmaya başlama] öğretici, her ikinci ve etkileşimli cihaz bulut iletilerini rastgele başına veri noktası cihaz bulut iletilerini gönderir Aralık. Bu uygulama, IOT Hub ile iletişim kurmak için MQTT protokolünü kullanır.
* **ReadCriticalQueue.py** IOT hub'ına bağlı Service Bus kuyruğundan kritik iletileri çıkarır.

> [!NOTE]
> IOT hub'ı birçok cihaz platformları ve C, Java ve JavaScript gibi diller için SDK desteğe sahiptir. Cihaz Bu öğreticide bir fiziksel aygıt ile değiştirme ve aygıtları bir IOT Hub'ına bağlanmak hakkında yönergeler için bkz: [Azure IOT Geliştirme Merkezi].

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Eksiksiz bir çalışma sürümü [IOT Hub ile çalışmaya başlama] Öğreticisi.
* [Python 2.x veya 3.x][lnk-python-download]. Kurulumunuzun gereksinimine uygun olarak 32 bit veya 64 bit yüklemeyi kullanmaya dikkat edin. Yükleme sırasında istendiğinde, platforma özgü ortam değişkeninize Python’u eklediğinizden emin olun. Python 2.x kullanıyorsanız, [Python paket yönetim sistemi *pip*’yi yüklemeniz veya yükseltmeniz][lnk-install-pip] gerekebilir.
* Windows işletim sistemi kullanıyorsanız, Python’dan yerel DLL’lerin kullanımına olanak tanımak için [Visual C++ yeniden dağıtılabilir paketi][lnk-visual-c-redist].
* [Node.js 4.0 veya üstü][lnk-node-download]. Kurulumunuzun gereksinimine uygun olarak 32 bit veya 64 bit yüklemeyi kullanmaya dikkat edin. Bu, [IoT Hub Gezgini aracını][lnk-iot-hub-explorer] yüklemek için gereklidir.
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

Ayrıca okumayı öneririz [Azure Storage] ve [Azure Service Bus].


## <a name="send-interactive-messages-from-a-device-app"></a>Bir aygıt uygulamadan etkileşimli ileti gönderme
Bu bölümde, oluşturduğunuz cihaz uygulamayı değiştirmek [IOT Hub ile çalışmaya başlama] bazen hemen işlem iletileri göndermek için öğretici.

1. Açmak için bir metin düzenleyicisi kullanın **SimulatedDevice.py** dosya. Bu dosya için kod içeren **SimulatedDevice** oluşturduğunuz uygulama [IOT Hub ile çalışmaya başlama] Öğreticisi.

2. Değiştir **iothub_client_telemetry_sample_run** işlevi aşağıdaki kod ile:

    ```python
    def iothub_client_telemetry_sample_run():

    try:
        client = iothub_client_init()
        print ( "IoT Hub device sending periodic messages, press Ctrl-C to exit" )
        message_counter = 0

        while True:
            random_seed = random.random()
            msg_txt_formatted = MSG_TXT % (AVG_WIND_SPEED + (random_seed * 4 + 2))
            # messages can be encoded as string or bytearray
            if (message_counter & 1) == 1:
                message = IoTHubMessage(bytearray(msg_txt_formatted, 'utf8'))
            else:
                message = IoTHubMessage(msg_txt_formatted)
            # optional: assign ids
            message.message_id = "message_%d" % message_counter
            message.correlation_id = "correlation_%d" % message_counter
            # optional: assign properties
            prop_map = message.properties()
            if random_seed > .5:
                if random_seed > .8:
                    prop_map.add("level", 'critical')
                else:
                    prop_map.add("level", 'storage')

            client.send_event_async(message, send_confirmation_callback, message_counter)
            print ( "IoTHubClient.send_event_async accepted message [%d] for transmission to IoT Hub." % message_counter )

            status = client.get_send_status()
            print ( "Send status: %s" % status )
            time.sleep(30)

            status = client.get_send_status()
            print ( "Send status: %s" % status )

            message_counter += 1

    except IoTHubError as iothub_error:
        print ( "Unexpected error %s from IoTHub" % iothub_error )
        return
    except KeyboardInterrupt:
        print ( "IoTHubClient sample stopped" )
    ```
   
    Bu yöntem rastgele özelliği ekler `"level": "critical"` ve `"level": "storage"` aygıt tarafından gönderilen iletiler için hangi benzetimini yapar, uygulama arka uç ya da kalıcı olarak depolanması gereken bir Acil eylem gerektiren bir ileti. Uygulama ileti gövdesinde temelinde yönlendirme iletileri destekler.
   
   > [!NOTE]
   > Burada gösterilen etkin yolunuzda örnek yanı sıra soğuk yolu işleme dahil olmak üzere çeşitli senaryoları için ileti özellikleri iletileri yönlendirmek için kullanabilirsiniz.

1. **SimulatedDevice.py** dosyasını kaydedin ve kapatın.

    > [!NOTE]
    > Basitleştirmek amacıyla, Bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda MSDN makalesinde önerilen üstel geri alma gibi bir yeniden deneme ilkesi uygulamalıdır [geçici hata işleme].


## <a name="add-queues-to-your-iot-hub-and-route-messages-to-them"></a>IOT hub ve rota iletileri bunlara sıraları ekleyin

Bu bölümde, Service Bus kuyruğuna ve bir depolama hesabı oluşturmak, IOT hub'ınıza bağlanın ve ileti ve tüm iletileri depolama hesabı, bir özellik varlığına dayalı kuyruğa ileti göndermek için IOT hub'ınızı yapılandırma. Hizmet veri yolu sıralardan işlem iletileri hakkında daha fazla bilgi için bkz: [kuyruklarla çalışmaya başlama] [ lnk-sb-queues-node] ve depolama yönetmek için bkz: nasıl [Azure Storage ile çalışmaya başlama] [Azure depolama].

1. Service Bus kuyruğuna açıklandığı gibi oluşturmak [kuyruklarla çalışmaya başlama][lnk-sb-queues-node]. Ad alanı ve sıra adını not edin.

    > [!NOTE]
    > Service Bus kuyrukları ve konularından IOT Hub uç noktaları değil olarak kullanılan **oturumları** veya **yinelenen saptama** etkin. Bu seçeneklerden birini etkinleştirilmişse, uç nokta olarak görünür **ulaşılamıyor** Azure portalında.

1. Azure portalında, IOT hub'ınızı açın ve **uç noktaları**.

    ![IOT hub uç noktaları][30]

1. İçinde **uç noktaları** dikey penceresinde tıklatın **Ekle** sıranız IOT hub'ınıza eklemek için üst. Uç nokta adı **CriticalQueue** ve seçmek için açılır listeleri kullanın **Service Bus kuyruğuna**, sıranız bulunduğu hizmet veri yolu ad alanı ve sıranız adı. İşiniz bittiğinde tıklatın **Tamam** altındaki.  

    ![Bir uç nokta ekleme][31]

1. Şimdi **yollar** IOT hub'ınızdaki. Tıklatın **Ekle** iletileri kuyruğa yönlendiren bir yönlendirme kuralı oluşturmak için dikey pencerenin üstündeki eklediğiniz. 

    ![Bir yol ekleme][34]

    Bir ad girin ve **cihaz iletilerini** veri kaynağı olarak. Seçin **CriticalQueue** yönlendirme olarak özel bir uç nokta olarak kural endpoint ve girin `level="critical"` sorgu dizesi olarak. Tıklatın **kaydetmek** altındaki.

    ![Rota Ayrıntıları][32]

    Emin olun geri dönüş rota ayarlanmış **ON**. Bu ayar, bir IOT hub'ın varsayılan yapılandırmadır.

    ![Geri dönüş yolu][33]

## <a name="optional-read-from-the-queue-endpoint"></a>(İsteğe bağlı) Sıra uç noktasından okumak

Bu bölümde IOT hizmeti yolundan kritik iletileri okuyan bir Python konsol uygulaması oluşturun. Daha fazla bilgi [kuyruklarla çalışmaya başlama][lnk-sb-queues-node]. 

1. Yeni bir komut istemi açın ve Python için Azure IoT Hub Cihazı SDK’sını aşağıda gösterildiği gibi yükleyin. Yükleme bittikten sonra komut istemini kapatın.

    ```cmd/sh
    pip install azure-servicebus
    ```

    > [!NOTE]
    > Yükleme sorunları için **azure servicebus** paketini veya daha fazla yükleme seçenekleri için bkz. [Python azure servicebus paket][lnk-python-service-bus].

1. Adlı bir dosya oluşturun **ReadCriticalQueue.py**. Bu dosyayı, kendi seçiminize bağlı olarak Python düzenleyicisinde/IDE’de açın (örneğin, IDLE).

1. Gerekli modüllerini aygıttan SDK almak için aşağıdaki kodu ekleyin:

    ```python
    from azure.servicebus import ServiceBusService, Message, Queue
    ```

1. Aşağıdaki kodu ekleyin ve bağlantı verileri için hizmet veri yolu ile yer tutucuları değiştirin:

    ```python
    SERVICE_BUS_NAME = {serviceBusName}
    SHARED_ACCESS_POLICY_NAME = {sharedAccessPolicyName}
    SHARED_ACCESS_POLICY_KEY_VALUE = {sharedAccessPolicyKeyValue}
    QUEUE_NAME = {queueName}    
    ```

5. Bağlanmak ve hizmet veri yolu okumak için aşağıdaki kodu ekleyin: 

    ```python
    def setup_client():
        bus_service = ServiceBusService(
        service_namespace=SERVICE_BUS_NAME,
        shared_access_key_name=SHARED_ACCESS_POLICY_NAME,
        shared_access_key_value=SHARED_ACCESS_POLICY_KEY_VALUE)

        while True:
            msg = bus_service.receive_queue_message(QUEUE_NAME, peek_lock=False)
            print(msg.body)
    ```

1. Son olarak, ana işlevi ekleyin. 

    ```python
    if __name__ == '__main__':
        setup_client()
    ```

1. Kaydet ve Kapat **ReadCriticalQueue.py** dosya. Şimdi uygulamaları çalıştırmaya hazırsınız.


## <a name="run-the-applications"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Komut istemini açın ve IoT Hub Gezgini’ni yükleyin. 

    ```cmd/sh
    npm install -g iothub-explorer
    ```

1. Cihazdan buluta gelen iletileri cihazınızdan izlemeye başlamak için, komut isteminde aşağıdaki komutu çalıştırın. Yer tutucuda `--login` öğesinden sonra IoT hub’ınızın bağlantı dizesini kullanın.

    ```cmd/sh
    iothub-explorer monitor-events [deviceId] --login "[IoTHub connection string]"
    ```

1. Yeni bir komut istemi açın ve **SimulatedDevice.py** dosyasını içeren dizine gidin.

1. IoT hub’ınıza düzenli aralıklarla telemetri verileri gönderen **SimulatedDevice.py** dosyasını çalıştırın. 
   
    ```cmd/sh
    python SimulatedDevice.py
    ```

1. Çalıştırmak için **ReadCriticalQueue** uygulamasında, bir komut istemi veya kabuk gidin **ReadCriticalQueue.py** dosya ve aşağıdaki komutu yürütün:

   ```cmd/sh
   python ReadCriticalQueue.py
   ```

1. Önceki bölümde IoT Hub Gezgini’ni çalıştırarak komut isteminde cihaz iletilerini gözlemleyin. Gözlemlemek `critical` içinde iletileri **ReadCriticalQueue** uygulama.

    ![Python cihazdan buluta iletiler][2]


## <a name="optional-add-storage-container-to-your-iot-hub-and-route-messages-to-it"></a>(İsteğe bağlı) Depolama kapsayıcısı IOT hub ve rota iletileri ona ekleyin

Bu bölümde, depolama hesabı oluşturma, IOT hub'ınıza bağlanın ve iletiyi bir özellik varlığına dayalı hesabına iletileri göndermek için IOT hub'ınızı yapılandırma. Depolama yönetme hakkında daha fazla bilgi için bkz: [Azure Storage ile çalışmaya başlama][Azure Storage].

 > [!NOTE]
   > IOT hub'ı hesapları altında oluşturulan _F1 ücretsiz_ katmanı birle sınırlı **Endpoint**. Birle sınırlı değilse **Endpoint**, Kurulum **StorageContainer** ek olarak **CriticalQueue** ve her iki simulatneously çalıştırın.

1. Bölümünde açıklandığı gibi bir depolama hesabı oluşturma [Azure Storage belgeleri][lnk-storage]. Hesap adını not edin.

2. Azure portalında, IOT hub'ınızı açın ve **uç noktaları**.

3. İçinde **uç noktaları** dikey penceresinde, select **CriticalQueue** uç noktası ve tıklatın **silmek**. Tıklatın **Evet**ve ardından **Ekle**. Uç nokta adı **StorageContainer** ve seçmek için açılır listeleri kullanın **Azure depolama kapsayıcısının**ve oluşturma bir **depolama hesabı** ve **depolama kapsayıcı**.  Adlarını not edin.  İşiniz bittiğinde tıklatın **Tamam** altındaki. 

 > [!NOTE]
   > IOT hub'ı hesapları altında oluşturulan _F1 ücretsiz_ katmanı birle sınırlı **Endpoint**. Birle sınırlı değilse **Endpoint**, silme gerekmez **CriticalQueue**.

4. Tıklatın **yollar** IOT hub'ınızdaki. Tıklatın **Ekle** iletileri kuyruğa yönlendiren bir yönlendirme kuralı oluşturmak için dikey pencerenin üstündeki eklediğiniz. Seçin **cihaz iletilerini** veri kaynağı olarak. Girin `level="storage"` koşul olarak seçin **StorageContainer** yönlendirme kuralı uç noktası olarak özel bir uç noktası olarak. Tıklatın **kaydetmek** altındaki.  

    Emin olun geri dönüş rota ayarlanmış **ON**. Bu ayar, bir IOT hub'ın varsayılan yapılandırmadır.

1. Önceki uygulamanızı emin olun **SimulatedDevice.py** hala çalışıyor. 

1. Azure Portalı'nda altında depolama hesabınıza gidin **Blob hizmeti**, tıklatın **BLOB'lar Gözat...** .  Kapsayıcıyı seçin, gidin ve JSON dosyasına tıklayın ve tıklayın **karşıdan** verileri görüntülemek için.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, IOT Hub'ının ileti yönlendirme işlevini kullanarak cihaz bulut iletilerini güvenilir bir şekilde gönderme öğrendiniz.

IOT hub'ı kullanan tam uçtan uca çözümler örnekleri görmek için bkz: [Azure IOT paketi][lnk-suite].

IOT Hub ile çözümleri geliştirme hakkında daha fazla bilgi için bkz: [IOT Hub Geliştirici Kılavuzu].

IOT Hub içinde ileti yönlendirme hakkında daha fazla bilgi için bkz: [IOT Hub ile iletileri almasına ve göndermesine][lnk-devguide-messaging].

<!-- Images. -->
[2]: ./media/iot-hub-python-python-process-d2c/output.png

[30]: ./media/iot-hub-python-python-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-python-python-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-python-python-process-d2c/route-creation.png
[33]: ./media/iot-hub-python-python-process-d2c/fallback-route.png
[34]: ./media/iot-hub-python-python-process-d2c/click-routes.png

<!-- Links -->
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-node-download]: https://nodejs.org/en/download/
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[lnk-iot-hub-explorer]: https://github.com/Azure/iothub-explorer
[lnk-sb-queues-node]: ../service-bus-messaging/service-bus-python-how-to-use-queues.md

[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[IOT Hub Geliştirici Kılavuzu]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[IOT Hub ile çalışmaya başlama]: iot-hub-python-getstarted.md
[Azure IOT Geliştirme Merkezi]: https://azure.microsoft.com/develop/iot

[geçici hata işleme]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-free-trial]: https://azure.microsoft.com/free/
[lnk-storage]: https://docs.microsoft.com/en-us/azure/storage/
[lnk-python-service-bus]: https://pypi.python.org/pypi/azure-servicebus