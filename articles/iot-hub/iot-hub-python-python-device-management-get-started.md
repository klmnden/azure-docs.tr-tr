---
title: Azure IOT Hub cihaz Yönetimi (Python) ile çalışmaya başlama | Microsoft Docs
description: IOT Hub cihaz yönetimine uzak aygıt yeniden başlatma işlemi başlatmak için nasıl kullanılacağını. Python için Azure IOT SDK'sı, doğrudan bir yöntem içeren bir sanal cihaz uygulamasının ve doğrudan yöntemini çağıran bir hizmet uygulaması uygulamak için kullanın.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 01/02/2018
ms.author: kgremban
ms.openlocfilehash: fa966ee2ea26cccc7d841a0e969d8329ac5bc0de
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34635296"
---
# <a name="get-started-with-device-management-python"></a>Aygıt Yönetimi (Python) ile çalışmaya başlama

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* IOT hub'ı oluşturun ve IOT hub'ınızda bir cihaz kimliği oluşturma için Azure Portalı'nı kullanın.
* Bu cihazı yeniden başlatır doğrudan bir yöntem içeren bir sanal cihaz uygulaması oluşturursunuz. Doğrudan yöntemleri buluttan çağrılır.
* IOT hub'ınız aracılığıyla sanal cihaz uygulama yeniden başlatma doğrudan yöntemini çağıran bir Python konsol uygulaması oluşturun.

Bu öğreticinin sonunda, iki Python konsol uygulamaları vardır:

**dmpatterns_getstarted_device.PY**, daha önce oluşturulan cihaz kimliğiyle IOT hub'ınızı bağlayan bir yeniden başlatma doğrudan yöntemi alır fiziksel yeniden başlatma taklit eder ve son yeniden başlatma zamanı raporlar.

**dmpatterns_getstarted_service.PY**, yanıt görüntüler, sanal cihaz uygulamada, doğrudan bir yöntemi çağırır ve görüntüler güncelleştirilmiş rapor özellikleri.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* [Python 2.x veya 3.x][lnk-python-download]. Kurulumunuzun gereksinimine uygun olarak 32 bit veya 64 bit yüklemeyi kullanmaya dikkat edin. Yükleme sırasında istendiğinde, platforma özgü ortam değişkeninize Python’u eklediğinizden emin olun. Python 2.x kullanıyorsanız, [Python paket yönetim sistemi *pip*’yi yüklemeniz veya yükseltmeniz][lnk-install-pip] gerekebilir.
    * Yükleme [azure-iothub-aygıt-client](https://pypi.org/project/azure-iothub-device-client/) komutunu kullanarak, paketi   `pip install azure-iothub-device-client`
    * Yükleme [azure-iothub-service-client](https://pypi.org/project/azure-iothub-service-client/) komutunu kullanarak, paketi   `pip install azure-iothub-service-client`
* Windows işletim sistemi kullanıyorsanız, Python’dan yerel DLL’lerin kullanımına olanak tanımak için [Visual C++ yeniden dağıtılabilir paketi][lnk-visual-c-redist].
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma
Bu bölümde şunları yapacaksınız:

* Bulut tarafından adlı doğrudan bir yöntem yanıtlaması bir Python konsol uygulaması oluşturma
* Aygıt yeniden başlatma benzetimi
* Aygıtları ve bunların en son ne zaman yeniden tanımlamak için cihaz çifti sorgular etkinleştirmek için bildirilen özelliklerini kullanın

1. Bir metin düzenleyicisi kullanarak oluşturduğunuz bir **dmpatterns_getstarted_device.py** dosya.

1. Aşağıdakileri ekleyin `import` deyimleri başlangıcında **dmpatterns_getstarted_device.py** dosya.
   
    ```python
    import random
    import time, datetime
    import sys

    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError, DeviceMethodReturnValue
    ```

1. Dahil olmak üzere değişken ekleyebilirsiniz bir **CONNECTION_STRING** değişkeni ve istemci başlatma.  Bağlantı dizesi, cihaz bağlantı dizesi ile değiştirin.  
   
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

1. Cihazda doğrudan yöntemi uygulamak için aşağıdaki işlevi geri aramalar ekleyin.
   
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

1. Doğrudan yöntemi dinleyicisini başlatmak ve bekleyin.
   
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

1. Kaydet ve Kapat **dmpatterns_getstarted_device.py** dosya.

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. [Geçici Hata İşleme][lnk-transient-faults] adlı MSDN makalesinde önerildiği üzere, üretim kodunda yeniden deneme ilkelerini (üstel geri alma gibi) uygulamanız gerekir.


## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Tetikleyici doğrudan bir yöntem kullanarak cihaz üzerinde Uzaktan yeniden başlatma
Bu bölümde, doğrudan bir yöntem kullanarak bir cihazda Uzaktan yeniden başlatma başlatan bir Python konsol uygulaması oluşturun. Uygulama cihaz çifti sorguları bu aygıtın son yeniden başlatma zamanını bulmak için kullanır.

1. Bir metin düzenleyicisi kullanarak oluşturduğunuz bir **dmpatterns_getstarted_service.py** dosya.

1. Aşağıdakileri ekleyin `import` deyimleri başlangıcında **dmpatterns_getstarted_service.py** dosya.
   
    ```python
    import sys, time
    import iothub_service_client

    from iothub_service_client import IoTHubDeviceMethod, IoTHubError, IoTHubDeviceTwin
    ```

1. Aşağıdaki değişken bildirimlerini ekleyin. Yalnızca yer tutucu değerlerini değiştirmek _IoTHubConnectionString_ ve _DeviceID_.
   
    ```python
    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"

    METHOD_NAME = "rebootDevice"
    METHOD_PAYLOAD = "{\"method_number\":\"42\"}"
    TIMEOUT = 60
    WAIT_COUNT = 10
    ```

1. Hedef aygıt yeniden başlatma, ardından cihaz çiftlerini sorgulamak için aygıtın yöntemini çağırmak için aşağıdaki işlevi ekleyin ve son yeniden başlatma zamanı alın.
   
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

1. Kaydet ve Kapat **dmpatterns_getstarted_service.py** dosya.


## <a name="run-the-apps"></a>Uygulamaları çalıştırma
Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Komut isteminde yeniden başlatma doğrudan yöntemi için dinleme başlamak için aşağıdaki komutu çalıştırın.
   
    ```
    python dmpatterns_getstarted_device.py
    ```

1. Başka bir komut isteminde aşağıdaki komutu tetikleyici Uzaktan yeniden başlatma ve son bulmak cihaz çifti için sorgu zaman yeniden çalıştırın.
   
    ```
    python dmpatterns_getstarted_service.py
    ```

1. Konsolunda doğrudan yöntemi aygıt yanıta bakın.

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/

[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
