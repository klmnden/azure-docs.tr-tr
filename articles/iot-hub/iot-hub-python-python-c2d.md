---
title: Azure IOT hub'ı (Python) ile bulut-cihaz iletilerini | Microsoft Docs
description: Python için Azure IOT SDK'larını kullanarak bir Azure IOT hub'ından bir cihaza bulut-cihaz iletilerini göndermek nasıl. Bir sanal cihaz uygulamasının bulut-cihaz iletilerini ve bulut-cihaz iletilerini göndermek için bir arka uç uygulaması değiştirme değiştirin.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: kgremban
ms.openlocfilehash: 316a8cd9ebf58e06ba39ba18fa19ede4b6a62229
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39187307"
---
# <a name="send-cloud-to-device-messages-with-iot-hub-python"></a>IOT hub'ı (Python) ile bulut buluttan cihaza iletileri gönderme
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]


## <a name="introduction"></a>Giriş
Azure IOT hub'ı yardımcı olan tam olarak yönetilen bir hizmet, milyonlarca cihaz arasında güvenilir ve güvenli çift yönlü iletişimi etkinleştirmek ve bir çözüm arka ucu ' dir. [IOT Hub ile çalışmaya başlama] öğretici, IOT hub oluşturma, bir cihaz kimliği da sağlamak ve CİHAZDAN buluta iletiler gönderen bir sanal cihaz uygulamasının kodu nasıl gösterir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğreticide yapılar [IOT Hub ile çalışmaya başlama]. Bunun nasıl yapılacağı anlatılmaktadır için:

* Çözüm arka ucunuz, tek bir cihaz IOT hub'ı aracılığıyla bulut-cihaz iletilerini gönderin.
* Bir cihazda bulut-cihaz iletilerini alır.
* Çözüm arka ucunuz, teslimat alındısı istek (*geri bildirim*) için bir cihaz IOT Hub'ından gönderilen iletileri.

Bulut-cihaz iletileri hakkında daha fazla bilgi bulabilirsiniz [IOT Hub Geliştirici kılavuzunun][IoT Hub developer guide - C2D].

Bu öğreticinin sonunda iki Python konsol uygulaması çalıştırın:

* **SimulatedDevice.py**, oluşturulan uygulamayı değiştirilmiş bir sürümünü [IOT Hub ile çalışmaya başlama], IOT hub'ınıza bağlanır ve bulut-cihaz iletilerini alır.
* **SendCloudToDeviceMessage.py**, IOT hub'ı aracılığıyla sanal cihaz uygulaması için bir bulut buluttan cihaza ileti gönderir ve ardından, teslimat alındısı.

> [!NOTE]
> IOT Hub SDK desteği birçok cihaz platformlarını ve Azure IOT cihaz SDK'ları aracılığıyla diller (C, Java ve Javascript dahil) sahiptir. Bu öğreticinin koda ve genellikle Azure IOT hub'a Cihazınızı bağlamak hakkında adım adım yönergeler için bkz. [Azure IOT Geliştirici Merkezi].
> 

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* [Python 2.x veya 3.x][lnk-python-download]. Kurulumunuzun gereksinimine uygun olarak 32 bit veya 64 bit yüklemeyi kullanmaya dikkat edin. Yükleme sırasında istendiğinde, platforma özgü ortam değişkeninize Python’u eklediğinizden emin olun. Python 2.x kullanıyorsanız, [Python paket yönetim sistemi *pip*’yi yüklemeniz veya yükseltmeniz][lnk-install-pip] gerekebilir.
* Windows işletim sistemi kullanıyorsanız, Python’dan yerel DLL’lerin kullanımına olanak tanımak için [Visual C++ yeniden dağıtılabilir paketi][lnk-visual-c-redist].
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

> [!NOTE]
> `azure-iothub-service-client` ve `azure-iothub-device-client` için *PIP* paketleri şu anda yalnızca Windows İşletim Sistemi için mevcuttur. Linux/Mac OS için lütfen [Python için geliştirme ortamınızı hazırlama] [lnk-python-devbox] post Linux ve MacOS ile ilgili bölümlere bakın.
> 


## <a name="receive-messages-in-the-simulated-device-app"></a>Sanal cihaz uygulamasında ileti alma
Bu bölümde, bir cihazın benzetimini gerçekleştirme ve IOT hub'ından bulut-cihaz iletilerini almak için bir Python konsol uygulaması oluşturun.

1. Bir metin düzenleyicisi kullanarak oluşturduğunuz bir **SimulatedDevice.py** dosya.

1. Aşağıdaki `import` deyimlerini ve değişkenlerini başlangıcında **SimulatedDevice.py** dosyası:
   
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

1. Alınan iletileri konsola yazdırmak için aşağıdaki işlevi ekleyin:
   
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

1. İstemci başlatmak için aşağıdaki kodu ekleyin ve bulut-cihaz ileti almak için:
   
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


## <a name="send-a-cloud-to-device-message"></a>Bulut buluttan cihaza ileti gönderme
Bu bölümde, sanal cihaz uygulaması için bulut-cihaz iletilerini gönderen bir Python konsol uygulaması oluşturun. Cihazın kimliği, eklediğiniz ihtiyacınız [IOT Hub ile çalışmaya başlama] öğretici. IOT Hub bağlantı dizesine, bulabileceğiniz hub'ınız için etmeniz [Azure portal].

1. Bir metin düzenleyicisi kullanarak oluşturduğunuz bir **SendCloudToDeviceMessage.py** dosya.

1. Aşağıdaki `import` deyimlerini ve değişkenlerini başlangıcında **SendCloudToDeviceMessage.py** dosyası:
   
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

1. Aşağıdaki kodu ekleyin **SendCloudToDeviceMessage.py** dosya. "{IoTHubConnectionString}" yer tutucu değerini hub için oluşturduğunuz, IOT Hub bağlantı dizesiyle değiştirin [IOT Hub ile çalışmaya başlama] öğretici. Eklediğiniz cihazın cihaz kimliği "{DeviceID}" yer tutucusunu değiştirin [IOT Hub ile çalışmaya başlama] Öğreticisi:
   
    ```python
    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"
    ```

1. Geri bildirim iletileri konsola yazdırmak için aşağıdaki işlevi ekleyin:
   
    ```python
    def open_complete_callback(context):
        print ( 'open_complete_callback called with context: {0}'.format(context) )

    def send_complete_callback(context, messaging_result):
        context = 0
        print ( 'send_complete_callback called with context : {0}'.format(context) )
        print ( 'messagingResult : {0}'.format(messaging_result) )
    ```

1. Cihazınız için bir ileti gönderin ve cihaz bulut-cihaz iletiyi onayladıktan, geri bildirim iletisini işlemek için aşağıdaki kodu ekleyin:
   
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

1. Bir komut istemi açın ve yükleme **Python için Azure IOT Hub cihazı SDK**.

    ```
    pip install azure-iothub-device-client
    ```

1. Komut isteminde, bulut-cihaz iletileri dinlemek için aşağıdaki komutu çalıştırın:
   
    ```shell
    python SimulatedDevice.py 
    ```
   
    ![Sanal cihaz uygulaması çalıştırma][img-simulated-device]

1. Yeni bir komut istemi açın ve yükleme **Python için Azure IOT Hub hizmeti SDK**.

    ```
    pip install azure-iothub-service-client
    ```

1. Komut isteminde, bulut buluttan cihaza ileti göndermek ve ileti hakkındaki geri bildirimleri beklemek için aşağıdaki komutu çalıştırın:
   
    ```shell
    python SendCloudToDeviceMessage.py 
    ```
   
    ![Bulut-cihaz komut göndermek için uygulamayı çalıştırın][img-send-command]
   
1. Cihaz tarafından alınan ileti unutmayın.

    ![İleti alındı][img-message-recieved]


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bulut-cihaz iletilerini gönderip öğrendiniz. 

IOT hub'ı kullanan tam uçtan uca çözümler örneklerini görmek için bkz: [Azure IOT Uzaktan izleme çözüm Hızlandırıcısını].

IOT Hub ile çözümleri geliştirme hakkında daha fazla bilgi için bkz. [IOT Hub Geliştirici Kılavuzu].

<!-- Images -->
[img-simulated-device]: media/iot-hub-python-python-c2d/simulated-device.png
[img-send-command]:  media/iot-hub-python-python-c2d/send-command.png
[img-message-recieved]: media/iot-hub-python-python-c2d/message-recieved.png

<!-- Links -->
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-node-download]: https://nodejs.org/en/download/
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[IOT Hub ile çalışmaya başlama]: quickstart-send-telemetry-node.md
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
[IOT Hub Geliştirici Kılavuzu]: iot-hub-devguide.md
[Azure IoT Geliştirici Merkezi]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure portal]: https://portal.azure.com
[Azure IOT Uzaktan izleme çözüm Hızlandırıcısını]: https://azure.microsoft.com/documentation/suites/iot-suite/
