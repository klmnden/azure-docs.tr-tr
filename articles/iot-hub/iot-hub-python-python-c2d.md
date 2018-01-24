---
title: "Azure IOT hub'ı (Python) sahip bulut-cihaz iletilerini | Microsoft Docs"
description: "Python için Azure IOT SDK'ları kullanarak bir Azure IOT hub'dan bir aygıta bulut-cihaz iletilerini göndermek nasıl. Bulut-cihaz iletilerini ve bulut-cihaz iletilerini göndermek için bir arka uç uygulaması değiştirmek için bir sanal cihaz uygulamasının değiştirin."
services: iot-hub
documentationcenter: python
author: msebolt
manager: timlt
editor: 
ms.assetid: 3ca8a78f-ade2-46e8-8a49-d5d599cdf1f1
ms.service: iot-hub
ms.devlang: javascript
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/22/2018
ms.author: v-masebo
ms.openlocfilehash: 592e0cd858d16715f95955194eca4217d9914b05
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="send-cloud-to-device-messages-with-iot-hub-python"></a>IOT hub'ı (Python) sahip bulut-cihaz iletilerini gönder
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]


## <a name="introduction"></a>Giriş
Azure IOT Hub, milyonlarca cihaza arasında güvenilir ve güvenli çift yönlü iletişimler sağlayan tam olarak yönetilen hizmet etkinleştirin ve bir çözüm arka ucu ' dir. [IOT Hub ile çalışmaya başlama] öğretici, IOT hub'ı oluşturma, bir cihaz kimliği, sağlama ve cihaz-bulut iletileri gönderen bir sanal cihaz uygulamasının kodu gösterir.

Bu öğretici derlemeler [IOT Hub ile çalışmaya başlama]. Size bir nasıl gösterir için:

* Çözüm arka ucunuz, tek bir cihaz IOT hub'ı aracılığıyla bulut cihaz ileti gönderebilir.
* Bir cihazda bulut-cihaz iletilerini alır.
* Çözüm arka ucunuz, teslimat alındısı iste (*geri bildirim*) için bir cihaz IOT Hub'ından gönderilen iletiler için.

Bulut cihaz iletileri hakkında daha fazla bilgi bulabilirsiniz [IOT Hub Geliştirici Kılavuzu][IoT Hub developer guide - C2D].

Bu öğreticinin sonunda iki Python konsol uygulamaları çalıştırın:

* **SimulatedDevice.py**, oluşturduğunuz uygulama değiştirilmiş bir sürümünü [IOT Hub ile çalışmaya başlama], IOT hub'ınıza bağlanır ve bulut-cihaz iletilerini alır.
* **SendCloudToDeviceMessage.py**, IOT hub'ı aracılığıyla sanal cihaz uygulamasının bir bulut cihaz ileti gönderir ve teslimat alındısı alır.

> [!NOTE]
> IOT Hub SDK desteği birçok cihaz platformları ve Azure IOT cihaz SDK'ları aracılığıyla dilleri (C, Java ve Javascript dahil) sahiptir. Bu öğreticinin kod ve genellikle Azure IOT Hub Cihazınızı bağlamak hakkında adım adım yönergeler için bkz: [Azure IOT Geliştirme Merkezi].
> 

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* [Python 2.x veya 3.x][lnk-python-download]. Kurulumunuzun gereksinimine uygun olarak 32 bit veya 64 bit yüklemeyi kullanmaya dikkat edin. Yükleme sırasında istendiğinde, platforma özgü ortam değişkeninize Python’u eklediğinizden emin olun. Python 2.x kullanıyorsanız, [Python paket yönetim sistemi *pip*’yi yüklemeniz veya yükseltmeniz][lnk-install-pip] gerekebilir.
* Windows işletim sistemi kullanıyorsanız, Python’dan yerel DLL’lerin kullanımına olanak tanımak için [Visual C++ yeniden dağıtılabilir paketi][lnk-visual-c-redist].
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

> [!NOTE]
> `azure-iothub-service-client` ve `azure-iothub-device-client` için *PIP* paketleri şu anda yalnızca Windows İşletim Sistemi için mevcuttur. Linux/Mac OS için lütfen [Python için geliştirme ortamınızı hazırlama] [lnk-python-devbox] post Linux ve Mac OS özgü bölümlere bakın.
> 


## <a name="receive-messages-in-the-simulated-device-app"></a>Sanal cihaz uygulamasının ileti alma
Bu bölümde IOT hub'ından bulut cihaz ileti alıp cihazın benzetimini yapmak için bir Python konsol uygulaması oluşturun.

1. Bir metin düzenleyicisi kullanarak oluşturduğunuz bir **SimulatedDevice.py** dosya.

1. Aşağıdakileri ekleyin `import` deyimleri ve değişkenleri başlangıcında **SimulatedDevice.py** dosyası:
   
    ```python
    import time
    import sys
    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult
    from iothub_client import IoTHubMessage, IoTHubMessageDispositionResult, IoTHubError

    RECEIVE_CONTEXT = 0
    WAIT_COUNT = 10
    RECEIVED_COUNT = 0
    RECEIVE_CALLBACKS = 0
    ```

1. Aşağıdaki kodu ekleyin **SimulatedDevice.py** dosya. "{DeviceConnectionString}" yer tutucu değerini, oluşturduğunuz cihaz için cihaz bağlantı dizesiyle değiştirin [IOT Hub ile çalışmaya başlama] Öğreticisi:
   
    ```python
    # choose AMQP or AMQP_WS as transport protocol
    PROTOCOL = IoTHubTransportProvider.AMQP
    CONNECTION_STRING = "{deviceConnectionString}"
    ```

1. Konsola alınan ileti yazdırmak için aşağıdaki işlevi ekleyin:
   
    ```python
    def receive_message_callback(message, counter):
        global RECEIVE_CALLBACKS
        message_buffer = message.get_bytearray()
        size = len(message_buffer)
        print ( "Received Message [%d]:" % counter )
        print ( "    Data: <<<%s>>> & Size=%d" % (message_buffer[:size].decode('utf-8'), size) )
        map_properties = message.properties()
        key_value_pair = map_properties.get_internals()
        print ( "    Properties: %s" % key_value_pair )
        counter += 1
        RECEIVE_CALLBACKS += 1
        print ( "    Total calls received: %d" % RECEIVE_CALLBACKS )
        return IoTHubMessageDispositionResult.ACCEPTED

    def iothub_client_init():
        client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

        client.set_message_callback(receive_message_callback, RECEIVE_CONTEXT)

        return client

    def print_last_message_time(client):
        try:
            last_message = client.get_last_message_receive_time()
            print ( "Last Message: %s" % time.asctime(time.localtime(last_message)) )
            print ( "Actual time : %s" % time.asctime() )
        except IoTHubClientError as iothub_client_error:
            if iothub_client_error.args[0].result == IoTHubClientResult.INDEFINITE_TIME:
                print ( "No message received" )
            else:
                print ( iothub_client_error )
    ```

1. İstemci başlatmak için aşağıdaki kodu ekleyin ve bulut-cihaz ileti almak için bekleyin:
   
    ```python
    def iothub_client_init():
        client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

        client.set_message_callback(receive_message_callback, RECEIVE_CONTEXT)

        return client

    def iothub_client_sample_run():
        try:
            client = iothub_client_init()

            while True:
                print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    status = client.get_send_status()
                    print ( "Send status: %s" % status )
                    time.sleep(10)
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )

        print_last_message_time(client)
    ```

1. Aşağıdaki ana işlevi ekleyin:
   
    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Python sample..." )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_client_sample_run()
    ```

1. Kaydet ve Kapat **SimulatedDevice.py** dosya.


## <a name="send-a-cloud-to-device-message"></a>Bulut cihaza ileti gönderme
Bu bölümde, sanal cihaz uygulamasının bulut cihaz iletileri gönderen bir Python konsol uygulaması oluşturun. Eklediğiniz aygıt cihaz Kimliğini gereksinim [IOT Hub ile çalışmaya başlama] Öğreticisi. Ayrıca bulabilirsiniz, hub'ın IOT Hub bağlantı dizesine gerekir [Azure portal].

1. Bir metin düzenleyicisi kullanarak oluşturduğunuz bir **SendCloudToDeviceMessage.py** dosya.

1. Aşağıdakileri ekleyin `import` deyimleri ve değişkenleri başlangıcında **SendCloudToDeviceMessage.py** dosyası:
   
    ```python
    import random
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubMessaging, IoTHubMessage, IoTHubError

    OPEN_CONTEXT = 0
    FEEDBACK_CONTEXT = 1
    MESSAGE_COUNT = 1
    AVG_WIND_SPEED = 10.0
    MSG_TXT = "{\"service client sent a message\": %.2f}"
    ```

1. Aşağıdaki kodu ekleyin **SendCloudToDeviceMessage.py** dosya. "{IoTHubConnectionString}" yer tutucu değerini, oluşturduğunuz hub'ın IOT Hub bağlantı dizesiyle değiştirin [IOT Hub ile çalışmaya başlama] Öğreticisi. "{DeviceID}" yer tutucusunu, eklediğiniz aygıt cihaz Kimliğini değiştirin [IOT Hub ile çalışmaya başlama] Öğreticisi:
   
    ```python
    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"
    ```

1. İşlem sonuçları konsoluna yazdırmak için aşağıdaki işlevi ekleyin:
   
    ```python
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

1. Geri bildirim iletilerini konsola yazdırmak için aşağıdaki işlevi ekleyin:
   
    ```python
    def open_complete_callback(context):
        print ( 'open_complete_callback called with context: {0}'.format(context) )

    def send_complete_callback(context, messaging_result):
        context = 0
        print ( 'send_complete_callback called with context : {0}'.format(context) )
        print ( 'messagingResult : {0}'.format(messaging_result) )
    ```

1. Aygıtınıza ileti gönderme ve cihaz bulut cihaz iletiyi onayladıktan zaman geri bildirim iletisi işlemek için aşağıdaki kodu ekleyin:
   
    ```python
    def iothub_messaging_sample_run():
        try:
            iothub_messaging = IoTHubMessaging(CONNECTION_STRING)

            iothub_messaging.open(open_complete_callback, OPEN_CONTEXT)

            for i in range(0, MESSAGE_COUNT):
                print ( 'Sending message: {0}'.format(i) )
                msg_txt_formatted = MSG_TXT % (AVG_WIND_SPEED + (random.random() * 4 + 2))
                message = IoTHubMessage(bytearray(msg_txt_formatted, 'utf8'))

                # optional: assign ids
                message.message_id = "message_%d" % i
                message.correlation_id = "correlation_%d" % i
                # optional: assign properties
                prop_map = message.properties()
                prop_text = "PropMsg_%d" % i
                prop_map.add("Property", prop_text)

                iothub_messaging.send_async(DEVICE_ID, message, send_complete_callback, i)

            try:
                # Try Python 2.xx first
                raw_input("Press Enter to continue...\n")
            except:
                pass
                # Use Python 3.xx in the case of exception
                input("Press Enter to continue...\n")

            iothub_messaging.close()

        except IoTHubError as iothub_error:
            print ( "Unexpected error {0}" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubMessaging sample stopped" )
    ```

1. Aşağıdaki ana işlevi ekleyin:
   
    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Service Client Messaging Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_messaging_sample_run()
    ```

1. Kaydet ve Kapat **SendCloudToDeviceMessage.py** dosya.


## <a name="run-the-applications"></a>Uygulamaları çalıştırma
Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Bir komut istemi açın ve yükleme **Python için Azure IOT Hub cihaz SDK**.

    ```
    pip install azure-iothub-device-client
    ```

1. Komut isteminde bulut-cihaz iletilerini dinlemek için aşağıdaki komutu çalıştırın:
   
    ```shell
    python SimulatedDevice.py 
    ```
   
    ![Sanal cihaz uygulamasının çalıştırın][img-simulated-device]

1. Yeni bir komut istemi açın ve yükleme **Python için Azure IOT Hub hizmeti SDK**.

    ```
    pip install azure-iothub-service-client
    ```

1. Bir komut isteminde, bulut cihaza ileti gönderme ve ileti geri bildirim için beklemek için aşağıdaki komutu çalıştırın:
   
    ```shell
    python SendCloudToDeviceMessage.py 
    ```
   
    ![Bulut cihaz komut göndermek için uygulamayı çalıştırın][img-send-command]
   
1. Aygıt tarafından alınan ileti unutmayın.

    ![İleti alındı][img-message-recieved]


## <a name="next-steps"></a>Sonraki adımlar
Bu öğretici kapsamında, bulut cihaza ileti gönderme ve alma öğrendiniz. 

IOT hub'ı kullanan tam uçtan uca çözümler örnekleri görmek için bkz: [Azure IOT paketi].

IOT Hub ile çözümleri geliştirme hakkında daha fazla bilgi için bkz: [IOT Hub Geliştirici Kılavuzu].

<!-- Images -->
[img-simulated-device]: media/iot-hub-python-python-c2d/simulated-device.png
[img-send-command]:  media/iot-hub-python-python-c2d/send-command.png
[img-message-recieved]: media/iot-hub-python-python-c2d/message-recieved.png

<!-- Links -->
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-node-download]: https://nodejs.org/en/download/
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[IOT Hub ile çalışmaya başlama]: iot-hub-node-node-getstarted.md
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
[IOT Hub Geliştirici Kılavuzu]: iot-hub-devguide.md
[Azure IOT Geliştirme Merkezi]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure portal]: https://portal.azure.com
[Azure IOT paketi]: https://azure.microsoft.com/documentation/suites/iot-suite/
