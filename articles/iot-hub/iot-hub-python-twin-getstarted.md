---
title: Azure IOT Hub cihaz ikizlerini (Python) kullanmaya başlama | Microsoft Docs
description: Azure IOT Hub cihaz ikizlerini etiketler ekleyin ve ardından IOT Hub sorgu kullanma Python için Azure IOT SDK'ları, sanal cihaz uygulaması ve etiketleri ekler ve IOT Hub sorgu çalışan bir hizmet uygulaması'nı uygulamak için kullanın.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 12/04/2017
ms.author: kgremban
ms.openlocfilehash: 08e457febaa7522cac86e63c0c187d1e8e49daff
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38619374"
---
# <a name="get-started-with-device-twins-python"></a>Cihaz ikizlerini (Python) kullanmaya başlama
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Bu öğreticinin sonunda iki Python konsol uygulamanız olacaktır:

* **AddTagsAndQuery.py**, etiketleri ekler ve cihaz ikizlerini sorgular bir Python arka uç uygulaması.
* **ReportConnectivity.py**, bir cihaza benzetim yapan daha önce oluşturulan cihaz kimliğiyle IOT hub'ınıza bağlanır ve kendi bağlantı koşulu raporları bir Python uygulaması.

> [!NOTE]
> Makaleyi [Azure IOT SDK'ları] [ lnk-hub-sdks] hem cihaz hem de arka uç uygulamaları oluşturmak için kullanabileceğiniz Azure IOT SDK'ları hakkında bilgi sağlar.
> 
> 

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* [Python 2.x veya 3.x][lnk-python-download]. Kurulumunuzun gereksinimine uygun olarak 32 bit veya 64 bit yüklemeyi kullanmaya dikkat edin. Yükleme sırasında istendiğinde, platforma özgü ortam değişkeninize Python’u eklediğinizden emin olun. Python 2.x kullanıyorsanız, [Python paket yönetim sistemi *pip*’yi yüklemeniz veya yükseltmeniz][lnk-install-pip] gerekebilir.
* Windows işletim sistemi kullanıyorsanız, Python’dan yerel DLL’lerin kullanımına olanak tanımak için [Visual C++ yeniden dağıtılabilir paketi][lnk-visual-c-redist].
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

> [!NOTE]
> `azure-iothub-service-client` ve `azure-iothub-device-client` için *PIP* paketleri şu anda yalnızca Windows İşletim Sistemi için mevcuttur. Linux/macOS için lütfen [Python için geliştirme ortamınızı hazırlayın][lnk-python-devbox] makalesindeki Linux ve macOS ile ilgili bölümlere bakın.
> 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="create-the-service-app"></a>Hizmet uygulaması oluşturma
Bu bölümde, konum meta verileri ile ilişkili cihaz ikizi ekleyen bir Python konsol uygulaması oluşturun, **{cihaz kimliği}**. Ardından, IOT hub'ı Redmond sonra bir hücresel bağlantı raporlama olanları bulunan aygıtları seçme içinde depolanan cihaz ikizlerini sorgular.

1. Komut istemini açın ve **Python için Azure IoT Hub Hizmeti SDK’sını** aşağıda gösterildiği gibi yükleyin. SDK’yı yükledikten sonra komut istemini kapatın.

    ```
    pip install azure-iothub-service-client
    ```

1. Bir metin düzenleyicisi kullanarak yeni bir oluşturma **AddTagsAndQuery.py** dosya.

3. Hizmet SDK’sından gerekli modülleri içeri aktarmak için aşağıdaki kodu ekleyin:

    ```python
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod
    from iothub_service_client import IoTHubDeviceTwin, IoTHubError
    ```
2. Yer tutucusunu değiştirerek aşağıdaki kodu ekleyin `[IoTHub Connection String]` ve `[Device Id]` bağlantı dizesine IOT hub ve önceki bölümde oluşturduğunuz cihaz kimliği için sahip.
   
    ```python
    CONNECTION_STRING = "[IoTHub Connection String]"
    DEVICE_ID = "[Device Id]"

    UPDATE_JSON = "{\"properties\":{\"desired\":{\"location\":\"Redmond\"}}}"

    UPDATE_JSON_SEARCH = "\"location\":\"Redmond\""
    UPDATE_JSON_CLIENT_SEARCH = "\"connectivity\":\"cellular\""
    ```

1. Aşağıdaki kodu ekleyin **AddTagsAndQuery.py** dosyası:
   
     ```python
    def iothub_service_sample_run():
        try:
            iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)
        
            iothub_registry_statistics = iothub_registry_manager.get_statistics()
            print ( "Total device count                       : {0}".format(iothub_registry_statistics.totalDeviceCount) )
            print ( "Enabled device count                     : {0}".format(iothub_registry_statistics.enabledDeviceCount) )
            print ( "Disabled device count                    : {0}".format(iothub_registry_statistics.disabledDeviceCount) )
            print ( "" )

            number_of_devices = iothub_registry_statistics.totalDeviceCount
            dev_list = iothub_registry_manager.get_device_list(number_of_devices)
        
            iothub_twin_method = IoTHubDeviceTwin(CONNECTION_STRING)

            for device in range(0, number_of_devices):
                if dev_list[device].deviceId == DEVICE_ID:
                    twin_info = iothub_twin_method.update_twin(dev_list[device].deviceId, UPDATE_JSON)
        
            print ( "Devices in Redmond: " )        
            for device in range(0, number_of_devices):
                twin_info = iothub_twin_method.get_twin(dev_list[device].deviceId)
         
                if twin_info.find(UPDATE_JSON_SEARCH) > -1:
                    print ( dev_list[device].deviceId )
        
            print ( "" )
        
            print ( "Devices in Redmond using cellular network: " )
            for device in range(0, number_of_devices):
                twin_info = iothub_twin_method.get_twin(dev_list[device].deviceId)
                
                if twin_info.find(UPDATE_JSON_SEARCH) > -1:
                    if twin_info.find(UPDATE_JSON_CLIENT_SEARCH) > -1:
                        print ( dev_list[device].deviceId )

        except IoTHubError as iothub_error:
            print ( "Unexpected error {0}".format(iothub_error) )
            return
        except KeyboardInterrupt:
            print ( "IoTHub sample stopped" )
    ```
   
    **Kayıt defteri** nesne hizmetinden cihaz ikizlerini ile etkileşim kurmak için gereken tüm yöntemleri sunar. Kod ilk başlatır **kayıt defteri** nesnesi ve ardından cihaz ikizi güncelleştirmeleri **DeviceID**, ve son olarak iki sorguları çalıştırır. Yalnızca cihaz ikizlerini bulunan cihazların ilk seçer **Redmond43** tesis ve ikincisi de hücresel ağ üzerinden bağlı cihazları seçmek için sorguyu iyileştirir.
   
1. Sonuna aşağıdaki kodu ekleyin **AddTagsAndQuery.py** uygulamak için **iothub_service_sample_run** işlevi:
   
    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Device Twins Python service sample..." )

        iothub_service_sample_run()
    ```

1. Uygulamayı çalıştırın:
   
    ```cmd/sh
    python AddTagsAndQuery.py
    ```
   
    Bulunan tüm cihazlar için bir cihaz sormaya sorgu sonuçları görmeniz gerekir **Redmond43** ve sonuçları bir hücresel ağ kullanan cihazlar için sınırlar sorgu için yok.
   
    ![ilk sorgu][1]

Sonraki bölümde, bağlantı bilgilerini raporlar ve önceki bölümde sorgusunun sonucunu değiştiren bir cihaz uygulaması oluşturun.

## <a name="create-the-device-app"></a>Cihaz uygulaması oluşturma
Bu bölümde, hub'ınıza bağlanan bir Python konsol uygulaması oluşturun, **{cihaz kimliği}** ve ardından, cihaz çiftinin bildirilen özelliklerini hücresel ağ üzerinden bağlı bilgiler içermesini kimler güncelleştirmeler.

1. Komut istemini açın ve **Python için Azure IoT Hub Hizmeti SDK’sını** aşağıda gösterildiği gibi yükleyin. SDK’yı yükledikten sonra komut istemini kapatın.

    ```
    pip install azure-iothub-device-client
    ```

1. Bir metin düzenleyicisi kullanarak yeni bir oluşturma **ReportConnectivity.py** dosya.

3. Hizmet SDK’sından gerekli modülleri içeri aktarmak için aşağıdaki kodu ekleyin:

    ```python
    import time
    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError
    ```

2. Yer tutucusunu değiştirerek aşağıdaki kodu ekleyin `[IoTHub Device Connection String]` önceki bölümde oluşturduğunuz IOT hub cihaz bağlantı dizesiyle.
   
    ```python
    CONNECTION_STRING = "[IoTHub Device Connection String]"

    # choose HTTP, AMQP, AMQP_WS or MQTT as transport protocol
    PROTOCOL = IoTHubTransportProvider.MQTT

    TIMER_COUNT = 5
    TWIN_CONTEXT = 0
    SEND_REPORTED_STATE_CONTEXT = 0
    ```

1. Aşağıdaki kodu ekleyin **ReportConnectivity.py** cihaz uygulamak için dosya ikizlerini işlevselliği:

    ```python
    def device_twin_callback(update_state, payload, user_context):
        print ( "" )
        print ( "Twin callback called with:" )
        print ( "    updateStatus: %s" % update_state )
        print ( "    payload: %s" % payload )

    def send_reported_state_callback(status_code, user_context):
        print ( "" )
        print ( "Confirmation for reported state called with:" )
        print ( "    status_code: %d" % status_code )

    def iothub_client_init():
        client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

        if client.protocol == IoTHubTransportProvider.MQTT or client.protocol == IoTHubTransportProvider.MQTT_WS:
            client.set_device_twin_callback(
                device_twin_callback, TWIN_CONTEXT)

        return client

    def iothub_client_sample_run():
        try:
            client = iothub_client_init()

            if client.protocol == IoTHubTransportProvider.MQTT:
                print ( "Sending data as reported property..." )

                reported_state = "{\"connectivity\":\"cellular\"}"

                client.send_reported_state(reported_state, len(reported_state), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)

            while True:
                print ( "Press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= TIMER_COUNT:
                    status = client.get_send_status()
                    time.sleep(10)
                    status_counter += 1 
        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )
    ```   

    **İstemci** nesne ihtiyaç duyduğunuz etkileşim kurmak için cihaz ikizlerini CİHAZDAN ile tüm yöntemleri sunar. Sonra onu başlatır önceki kodu **istemci** nesnesi, cihaz için cihaz ikizi alır ve kendi bildirilen özellik ile bağlantı bilgilerini güncelleştirir.

1. Sonuna aşağıdaki kodu ekleyin **ReportConnectivity.py** uygulamak için **iothub_client_sample_run** işlevi:
   
    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Device Twins Python client sample..." )

        iothub_client_sample_run()
    ```

1. Cihaz uygulamasını çalıştırın
   
    ```cmd/sh
    python ReportConnectivity.py
    ```
   
    Cihaz ikizlerini güncelleştirildi onay görmeniz gerekir.

    ![Güncelleştirme çiftleri][2]

6. Cihaz bağlantı bilgilerini bildirilen, her iki sorgularda görüntülenmesi gerekir. Geri dönün ve sorguları yeniden çalıştırın:
   
    ```cmd/sh
    python AddTagsAndQuery.py
    ```
   
    Bu süre, **{cihaz kimliği}** iki sorgu sonuçlarında görüntülenmesi gerekir.
   
    ![İkinci sorgu][3]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure portalında yeni bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Cihaz meta verilerini etiketler bir arka uç uygulamasından eklenen ve bir sanal cihaz uygulaması rapor cihaz bağlantı bilgileri cihaz ikizinde söyleyebiliriz. Ayrıca bu bilgileri, kayıt defterini kullanarak sorgulamayı öğrendiniz.

Bilgi edinmek için aşağıdaki kaynakları kullanın. nasıl yapılır:

* ile cihazlardan telemetri gönderme [IOT Hub ile çalışmaya başlama] [ lnk-iothub-getstarted] öğretici
* ile cihaz ikizinin istenen özellikleri kullanarak cihazları yapılandırma [kullanmak istediğiniz cihazları yapılandırmak için Özellikler] [ lnk-twin-how-to-configure] öğretici
* İle etkileşimli olarak (örneğin, bir kullanıcı tarafından denetlenen uygulamasından fan üzerinde kapatma), cihazları denetleme [doğrudan yöntemler kullanma] [ lnk-methods-tutorial] öğretici.

<!-- images -->
[1]: media/iot-hub-python-twin-getstarted/1.png
[2]: media/iot-hub-python-twin-getstarted/2.png
[3]: media/iot-hub-python-twin-getstarted/3.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[lnk-python-devbox]: https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-python-getstarted.md
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

[lnk-twin-how-to-configure]: iot-hub-node-node-twin-how-to-configure.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
