---
title: Azure IOT Hub cihaz çiftlerini (Python) ile çalışmaya başlama | Microsoft Docs
description: Azure IOT Hub cihaz çiftlerini etiket ekleyebilir ve IOT Hub sorgusuyla kullanmak için nasıl kullanılacağını. Sanal cihaz uygulamasının ve etiketleri ekler ve IOT hub'ı sorgu çalışan bir hizmet uygulaması uygulamak için Python için Azure IOT SDK'ları kullanın.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 12/04/2017
ms.author: kgremban
ms.openlocfilehash: 08e457febaa7522cac86e63c0c187d1e8e49daff
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "34634990"
---
# <a name="get-started-with-device-twins-python"></a>Cihaz çiftlerini (Python) ile çalışmaya başlama
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Bu öğreticinin sonunda iki Python konsol uygulamaları olacaktır:

* **AddTagsAndQuery.py**, etiketleri ekler ve cihaz çiftlerini sorgular Python arka uç uygulaması.
* **ReportConnectivity.py**, bir cihaza benzetim yapan daha önce oluşturulan cihaz kimliğiyle IOT hub'ınıza bağlanır ve kendi bağlantı koşulu raporlar Python uygulaması.

> [!NOTE]
> Makaleyi [Azure IOT SDK'ları] [ lnk-hub-sdks] hem cihaz hem de arka uç uygulamalar oluşturmak için kullanabileceğiniz Azure IOT SDK'ları hakkında bilgi sağlar.
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

## <a name="create-the-service-app"></a>Hizmet Uygulaması Oluştur
Bu bölümde, ilişkili cihaz çifti konumu meta veri ekleyen bir Python konsol uygulaması oluşturun, **{cihaz kimliği}**. Ardından, IOT hub'ı Redmond sonra bir cep telefonu bağlantı raporlama olanları bulunan aygıtları seçerek depolanan cihaz çiftlerini sorgular.

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
2. İçin yer tutucu değiştirerek aşağıdaki kodu ekleyin `[IoTHub Connection String]` ve `[Device Id]` IOT hub'ı ve önceki kısımlarında oluşturduğunuz cihaz kimliği için bağlantı dizesiyle.
   
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
   
    **Kayıt defteri** nesne cihaz çiftlerini hizmet ile etkileşim kurmak için gereken tüm yöntemleri gösterir. Kod ilk başlatır **kayıt defteri** nesne sonra cihaz çifti için güncelleştirmeleri **DeviceID**, ve son olarak iki sorguları çalıştırır. İlk yalnızca cihaz çiftlerini bulunan aygıtların seçer **Redmond43** tesis ve ikinci da cep telefonu şebekesi bağlı aygıtlar seçmek için sorgu iyileştirir.
   
1. Sonuna aşağıdaki kodu ekleyin **AddTagsAndQuery.py** uygulamak için **iothub_service_sample_run** işlevi:
   
    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Device Twins Python service sample..." )

        iothub_service_sample_run()
    ```

1. Uygulama ile çalıştırın:
   
    ```cmd/sh
    python AddTagsAndQuery.py
    ```
   
    Bulunan tüm cihazlar için sorgu soran bir cihazda sonuçlarında görmelisiniz **Redmond43** ve sonuçları bir cep telefonu şebekesi kullanan cihazlar için sınırlar sorgu için yok.
   
    ![ilk sorgu][1]

Sonraki bölümde, önceki bölümde sorgusunun sonucu değiştirir ve bağlantı bilgilerini raporlar bir cihaz uygulaması oluşturursunuz.

## <a name="create-the-device-app"></a>Cihaz uygulaması oluşturma
Bu bölümde, hub'ınıza bağlanan bir Python konsol uygulaması oluşturun, **{cihaz kimliği}** ve ardından, cihaz çifti bir cep telefonu şebekesi kullanarak bağlı bilgileri içerecek şekilde özellikleri bildirilen güncelleştirmeler.

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

2. İçin yer tutucu değiştirerek aşağıdaki kodu ekleyin `[IoTHub Device Connection String]` önceki kısımlarında oluşturduğunuz IOT hub cihaz bağlantı dizesine sahip.
   
    ```python
    CONNECTION_STRING = "[IoTHub Device Connection String]"

    # choose HTTP, AMQP, AMQP_WS or MQTT as transport protocol
    PROTOCOL = IoTHubTransportProvider.MQTT

    TIMER_COUNT = 5
    TWIN_CONTEXT = 0
    SEND_REPORTED_STATE_CONTEXT = 0
    ```

1. Aşağıdaki kodu ekleyin **ReportConnectivity.py** cihaz uygulamak için dosya çiftlerini işlevselliği:

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

    **İstemci** nesne ihtiyaç duyduğunuz etkileşim kurmak için cihaz çiftlerini aygıttan ile tüm yöntemleri gösterir. Bunu başlatır sonra önceki kod **istemci** nesnesi, cihazınız için cihaz çifti alır ve kendi bildirilen özelliği ile bağlantı bilgilerini güncelleştirir.

1. Sonuna aşağıdaki kodu ekleyin **ReportConnectivity.py** uygulamak için **iothub_client_sample_run** işlevi:
   
    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Device Twins Python client sample..." )

        iothub_client_sample_run()
    ```

1. Cihaz uygulama çalıştırma
   
    ```cmd/sh
    python ReportConnectivity.py
    ```
   
    Cihaz çiftlerini güncelleştirildi onay görmeniz gerekir.

    ![Güncelleştirme çiftlerini][2]

6. Aygıt bağlantısı bilgilerini bildirdi, her iki sorgularda görüntülenmelidir. Geri dönün ve yeniden sorgular çalıştırın:
   
    ```cmd/sh
    python AddTagsAndQuery.py
    ```
   
    Bu süre, **{cihaz kimliği}** hem sorgu sonuçlarında görüntülenmesi gerekir.
   
    ![İkinci sorgu][3]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure portalında yeni bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Cihaz meta verilerini bir arka uç uygulamadan etiketler eklendi ve sanal cihaz uygulaması rapor cihaz bağlantı bilgilerini cihaz çiftine yazıldı. Ayrıca, kayıt defterini kullanarak bu bilgileri sorgulamak nasıl de öğrendiniz.

Bilgi edinmek için aşağıdaki kaynakları kullanın nasıl yapılır:

* Aygıtlarla telemetri gönderen [IOT Hub ile çalışmaya başlama] [ lnk-iothub-getstarted] öğretici
* Cihaz çifti'nın istenen özelliklere sahip kullanarak cihazları yapılandırma [kullanmak istediğiniz cihazları yapılandırmak için Özellikler] [ lnk-twin-how-to-configure] öğretici
* İle etkileşimli olarak (örneğin, kullanıcı tarafından denetlenen bir uygulamadan fan etkinleştirdikten), cihazları denetleme [doğrudan yöntemleri kullanın] [ lnk-methods-tutorial] Öğreticisi.

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
