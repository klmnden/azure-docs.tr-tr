---
title: Azure IOT Hub cihaz yönetimini (Python) kullanmaya başlama | Microsoft Docs
description: IOT Hub cihaz Yönetimi uzak cihazı yeniden başlatmak için kullanma Python için Azure IOT SDK'sı, bir doğrudan yöntem içeren bir sanal cihaz uygulaması ve doğrudan yöntemini çağıran bir hizmet uygulaması'nı uygulamak için kullanın.
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: kgremban
ms.openlocfilehash: 04fc1da04d9da715acfed8ca9d26e9c325afb403
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59279933"
---
# <a name="get-started-with-device-management-python"></a>Cihaz yönetimini (Python) kullanmaya başlama

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* IOT Hub oluşturma ve IOT hub'ınızda bir cihaz kimliği oluşturmak için Azure portalını kullanın.

* Bu cihazı yeniden başlatır bir doğrudan yöntem içeren bir sanal cihaz uygulaması oluşturma. Doğrudan yöntemler buluttan çağrılır.

* IOT hub'ınız aracılığıyla sanal cihaz uygulaması, yeniden başlatma doğrudan yöntemi çağıran bir Python konsol uygulaması oluşturacaksınız.

Bu öğreticinin sonunda iki Python konsol uygulaması vardır:

* **dmpatterns_getstarted_device.PY**, daha önce oluşturulan cihaz kimliğiyle IOT hub'ınızı bağlayan bir yeniden başlatma doğrudan yöntem alıp fiziksel sistemin yeniden başlatılması benzetimini yapar ve zamanı son yeniden başlatma için raporlar.

* **dmpatterns_getstarted_service.PY**bir doğrudan yöntem sanal cihaz uygulamasında çağıran yanıt görüntüler ve görüntüler güncelleştirilmiş bildirilen özellikler.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* [Python 2.x veya 3.x](https://www.python.org/downloads/). Kurulumunuzun gereksinimine uygun olarak 32 bit veya 64 bit yüklemeyi kullanmaya dikkat edin. Yükleme sırasında istendiğinde, platforma özgü ortam değişkeninize Python’u eklediğinizden emin olun. Python 2.x kullanıyorsanız, [Python paket yönetim sistemi *pip*'yi yüklemeniz veya yükseltmeniz](https://pip.pypa.io/en/stable/installing/) gerekebilir.

* Yükleme [azure-iothub-cihaz-client](https://pypi.org/project/azure-iothub-device-client/) komutunu kullanarak paketi       `pip install azure-iothub-device-client`

* Yükleme [azure-iothub-service-client](https://pypi.org/project/azure-iothub-service-client/) komutunu kullanarak paketi       `pip install azure-iothub-service-client`

* Windows işletim sistemi kullanıyorsanız, Python’dan yerel DLL’lerin kullanımına olanak tanımak için [Visual C++ yeniden dağıtılabilir paketi](https://www.microsoft.com/download/confirmation.aspx?id=48145).

* Etkin bir Azure hesabı. (Hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.)

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>IOT hub için bağlantı dizesi alma

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma

Bu bölümde şunları yapacaksınız:

* Bulut tarafından çağrılan doğrudan bir yönteme yanıt veren bir Python konsol uygulaması oluşturma

* Cihaz önyüklemesi benzetmek

* Cihaz ikizi sorgularının cihazları ve bunların en son ne zaman yeniden tanımlamak üzere bildirilen özellikleri kullanın

1. Bir metin düzenleyicisi kullanarak oluşturduğunuz bir **dmpatterns_getstarted_device.py** dosya.

2. Aşağıdaki `import` başlangıcında deyimleri **dmpatterns_getstarted_device.py** dosya.

    ```python
    import random
    import time, datetime
    import sys

    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError, DeviceMethodReturnValue
    ```

3. Değişkenleri de dahil olmak üzere ekleme bir **CONNECTION_STRING** değişkeni ve istemci başlatma.  Bağlantı dizesi, cihaz bağlantı dizesiyle değiştirin.  

    ```python
    CONNECTION_STRING = "{deviceConnectionString}"
    PROTOCOL = IoTHubTransportProvider.MQTT

    CLIENT = IoTHubClient(CONNECTION_STRING, PROTOCOL)

    WAIT_COUNT = 5

    SEND_REPORTED_STATE_CONTEXT = 0
    METHOD_CONTEXT = 0

    SEND_REPORTED_STATE_CALLBACKS = 0
    METHOD_CALLBACKS = 0
    ```

4. Doğrudan yöntemi cihazda uygulamak için aşağıdaki işlevi geri çağırmaları ekleyin.

    ```python
    def send_reported_state_callback(status_code, user_context):
        global SEND_REPORTED_STATE_CALLBACKS

        print ( "Device twins updated." )

    def device_method_callback(method_name, payload, user_context):
        global METHOD_CALLBACKS

        if method_name == "rebootDevice":
            print ( "Rebooting device..." )
            time.sleep(20)

            print ( "Device rebooted." )

            current_time = str(datetime.datetime.now())
            reported_state = "{\"rebootTime\":\"" + current_time + "\"}"
            CLIENT.send_reported_state(reported_state, len(reported_state), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)

            print ( "Updating device twins: rebootTime" )

        device_method_return_value = DeviceMethodReturnValue()
        device_method_return_value.response = "{ \"Response\": \"This is the response from the device\" }"
        device_method_return_value.status = 200

        return device_method_return_value
    ```

5. Doğrudan yöntem dinleyicisini başlatmak ve bekleyin.

    ```python
    def iothub_client_init():
        if CLIENT.protocol == IoTHubTransportProvider.MQTT or client.protocol == IoTHubTransportProvider.MQTT_WS:
            CLIENT.set_device_method_callback(device_method_callback, METHOD_CONTEXT)

    def iothub_client_sample_run():
        try:
            iothub_client_init()

            while True:
                print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    time.sleep(10)
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Python sample..." )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_client_sample_run()
    ```

6. Kaydet ve Kapat **dmpatterns_getstarted_device.py** dosya.

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda yeniden deneme ilkelerini (örneğin, bir üstel geri alma), makalesinde önerildiği uygulamalıdır [geçici hata işleme](/azure/architecture/best-practices/transient-faults).


## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Bir doğrudan yöntem kullanarak cihaz üzerinde Uzaktan yeniden başlatma tetikleyin

Bu bölümde, bir cihazda doğrudan yöntem kullanarak uzaktan yeniden başlatma başlatan bir Python konsol uygulaması oluşturun. Uygulama, son yeniden başlatma zamanı bu cihaz için keşfetmek için cihaz ikizi sorgularını kullanır.

1. Bir metin düzenleyicisi kullanarak oluşturduğunuz bir **dmpatterns_getstarted_service.py** dosya.

2. Aşağıdaki `import` başlangıcında deyimleri **dmpatterns_getstarted_service.py** dosya.

    ```python
    import sys, time
    import iothub_service_client

    from iothub_service_client import IoTHubDeviceMethod, IoTHubError, IoTHubDeviceTwin
    ```

3. Aşağıdaki değişken bildirimlerini ekleyin. Yalnızca için yer tutucu değerlerini değiştirin _IoTHubConnectionString_ ve _DeviceID_.

    ```python
    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"

    METHOD_NAME = "rebootDevice"
    METHOD_PAYLOAD = "{\"method_number\":\"42\"}"
    TIMEOUT = 60
    WAIT_COUNT = 10
    ```

4. Hedef cihazı yeniden başlatın ve ardından cihaz çiftlerini sorgulamak için cihaz yöntemini çağırmak için aşağıdaki işlevi ekleyin ve son yeniden başlatma zamanı alın.

    ```python
    def iothub_devicemethod_sample_run():
        try:
            iothub_twin_method = IoTHubDeviceTwin(CONNECTION_STRING)
            iothub_device_method = IoTHubDeviceMethod(CONNECTION_STRING)

            print ( "" )
            print ( "Invoking device to reboot..." )

            response = iothub_device_method.invoke(DEVICE_ID, METHOD_NAME, METHOD_PAYLOAD, TIMEOUT)

            print ( "" )
            print ( "Successfully invoked the device to reboot." )

            print ( "" )
            print ( response.payload )

            while True:
                print ( "" )
                print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    twin_info = iothub_twin_method.get_twin(DEVICE_ID)

                    if twin_info.find("rebootTime") != -1:
                        print ( "Last reboot time: " + twin_info[twin_info.find("rebootTime")+11:twin_info.find("rebootTime")+37])
                    else:
                        print ("Waiting for device to report last reboot time...")

                    time.sleep(5)
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "" )
            print ( "Unexpected error {0}".format(iothub_error) )
            return
        except KeyboardInterrupt:
            print ( "" )
            print ( "IoTHubDeviceMethod sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Service Client DeviceManagement Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_devicemethod_sample_run()
    ```

5. Kaydet ve Kapat **dmpatterns_getstarted_service.py** dosya.

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Komut isteminde, yeniden başlatma doğrudan yöntem için dinleme başlamak için aşağıdaki komutu çalıştırın.

    ```
    python dmpatterns_getstarted_device.py
    ```

2. Başka bir komut isteminde Uzaktan yeniden başlatma ve son bulmak cihaz ikizi sorgusu zaman yeniden tetikleyici için aşağıdaki komutu çalıştırın.

    ```
    python dmpatterns_getstarted_service.py
    ```

3. Doğrudan yöntem konsolunda cihaz yanıtı görürsünüz.

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]