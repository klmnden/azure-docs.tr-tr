---
title: Azure IOT hub'ı (Python) ile cihaz üretici yazılımını güncelleştirme | Microsoft Docs
description: Cihaz üretici yazılımı güncelleştirme başlatmak için Azure IOT Hub cihaz Yönetimi kullanma Python için Azure IOT SDK'ları, bir sanal cihaz uygulaması ve üretici yazılımı güncelleştirmesi tetikleyen bir hizmet uygulaması'nı uygulamak için kullanın.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 02/16/2018
ms.author: kgremban
ms.openlocfilehash: d2ebdf54e595c2f02464c0c2446a6e5f5feefb9c
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38482029"
---
# <a name="use-device-management-to-initiate-a-device-firmware-update-pythonpython"></a>Cihaz üretici yazılımını başlatmak için cihaz yönetimini kullanma güncelleştirin (Python/Python)
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

İçinde [cihaz yönetimini kullanmaya başlama] [ lnk-dm-getstarted] eğitmen, nasıl kullanılacağını gördüğünüz [cihaz ikizi] [ lnk-devtwin] ve [doğrudan yöntemler ] [ lnk-c2dmethod] ilkel bir cihazı Uzaktan yeniden başlatmak için. Bu öğretici aynı IOT hub'ı temelleri kullanır ve rehberlik sağlar ve bir uçtan uca sanal üretici yazılımı güncelleştirme işlemini gösterir.  Bu düzen, bellenim güncelleştirme uygulamasında Intel Edison cihaz örnek için kullanılır.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* IOT hub'ınız aracılığıyla sanal cihaz uygulamasında firmwareUpdate doğrudan yöntemi çağıran bir Python konsol uygulaması oluşturacaksınız.
* Uygulayan bir sanal cihaz uygulaması oluşturma bir **firmwareUpdate** doğrudan yöntemi. Bu yöntem, üretici yazılımı görüntüsünü indirmeye bekler, üretici yazılımı görüntüsünü indirir ve son olarak üretici yazılımı görüntüsünü geçerlidir çok aşamalı bir işlem başlatır. Güncelleştirmenin her aşamasında cihaz ilerlemesini bildirmek üzere bildirilen özellikleri kullanır.

Bu öğreticinin sonunda iki Python konsol uygulaması vardır:

**dmpatterns_fwupdate_service.PY**bir doğrudan yöntem sanal cihaz uygulamasında çağıran yanıt görüntüler ve düzenli aralıklarla (her 500ms) görüntüler güncelleştirilmiş bildirilen özellikler.

**dmpatterns_fwupdate_device.PY**, daha önce oluşturulan cihaz kimliğiyle IOT hub'ınıza bağlanan firmwareUpdate bir doğrudan yöntem alır, üretici yazılımı güncelleştirme dahil benzetimini yapmak için çok durumlu bir işlem yapması çalıştırır: görüntüsü için bekleniyor İndirme, yeni görüntüyü indirme ve son olarak görüntüsü uygulanıyor.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* [Python 2.x veya 3.x][lnk-python-download]. Kurulumunuzun gereksinimine uygun olarak 32 bit veya 64 bit yüklemeyi kullanmaya dikkat edin. Yükleme sırasında istendiğinde, platforma özgü ortam değişkeninize Python’u eklediğinizden emin olun. Python 2.x kullanıyorsanız, [Python paket yönetim sistemi *pip*’yi yüklemeniz veya yükseltmeniz][lnk-install-pip] gerekebilir.
* Windows işletim sistemi kullanıyorsanız, Python’dan yerel DLL’lerin kullanımına olanak tanımak için [Visual C++ yeniden dağıtılabilir paketi][lnk-visual-c-redist].
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Bir doğrudan yöntem kullanarak cihaz üzerinde bir uzak üretici yazılımı güncelleştirmesini tetikleme
Bu bölümde, bir cihazda uzak üretici yazılımı güncelleştirme başlatan bir Python konsol uygulaması oluşturun. Uygulamayı bir doğrudan yöntem güncelleştirmeyi başlatmak için ve cihaz çifti sorguları etkin üretici yazılımı güncelleştirme durumunu düzenli aralıklarla almak için kullanır.

1. Komut isteminizde yüklemek için aşağıdaki komutu çalıştırın **azure-iothub-service-client** paket:
   
    ```cmd/sh
    pip install azure-iothub-service-client
    ```

1. Çalışma dizininizde bir metin düzenleyicisi kullanarak oluşturduğunuz bir **dmpatterns_getstarted_service.py** dosya.

1. Aşağıdaki 'import' deyimlerini ve değişkenlerini başlangıcına ekleyin **dmpatterns_getstarted_service.py** dosya. Değiştirin `IoTHubConnectionString` ve `deviceId` daha önce not aldığınız değerlerinizi içeren:
   
    ```python
    import sys
    import time

    import iothub_service_client
    from iothub_service_client import IoTHubDeviceTwin, IoTHubDeviceMethod, IoTHubError

    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"

    METHOD_NAME = "firmwareUpdate"
    METHOD_PAYLOAD = "{\"fwPackageUri\":\"test.com\"}"
    TIMEOUT = 60
    MESSAGE_COUNT = 5
    ```

1. Aşağıdaki işlev doğrudan yöntemini çağırın ve firmwareUpdate değerini görüntülemek için bildirilen özellik. Ayrıca `main` yordam:
   
    ```python
    def iothub_firmware_sample_run():
        try:
            iothub_twin_method = IoTHubDeviceTwin(CONNECTION_STRING)

            print ( "" )
            print ( "Direct Method called." )
            iothub_device_method = IoTHubDeviceMethod(CONNECTION_STRING)
        
            response = iothub_device_method.invoke(DEVICE_ID, METHOD_NAME, METHOD_PAYLOAD, TIMEOUT)
            print ( response.payload )
        
            print ( "" )
            print ( "Device Twin queried, press Ctrl-C to exit" )
            while True:
                twin_info = iothub_twin_method.get_twin(DEVICE_ID)

                if "\"firmwareStatus\":\"standBy\"" in twin_info:
                    print ( "Waiting on device..." )
                elif "\"firmwareStatus\":\"downloading\"" in twin_info:
                    print ( "Downloading firmware..." )
                elif "\"firmwareStatus\":\"applying\"" in twin_info:
                    print ( "Download complete, applying firmware..." )
                elif "\"firmwareStatus\":\"completed\"" in twin_info:
                    print ( "Firmware applied" )
                    print ( "" )
                    print ( "Get reported properties from device twin:" )
                    print ( twin_info )
                    break
                else:
                    print ( "Unknown status" )

                status_counter = 0
                while status_counter <= MESSAGE_COUNT:
                    time.sleep(1)
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "" )
            print ( "Unexpected error {0}" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "" )
            print ( "IoTHubService sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub firmware update Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_firmware_sample_run()
    ```

1. Kaydet ve Kapat **dmpatterns_fwupdate_service.py** dosya.


## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma
Bu bölümde şunları yapacaksınız:

* Bulut tarafından çağrılan doğrudan bir yönteme yanıt veren bir Python konsol uygulaması oluşturma
* Sanal bir üretici yazılımı güncelleştirmesini tetikleme
* Cihaz ikizi sorgularının cihazları ve bir üretici yazılımı güncelleştirmesini en son ne zaman tamamladığını belirlemesi için rapor edilen özellikleri kullanın

1. Komut isteminizde yüklemek için aşağıdaki komutu çalıştırın **azure-iothub-cihaz-client** paket:
   
    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Bir metin düzenleyicisi kullanarak oluşturduğunuz bir **dmpatterns_fwupdate_device.py** dosya.

1. Aşağıdaki 'import' deyimlerini ve değişkenlerini başlangıcına ekleyin **dmpatterns_fwupdate_device.py** dosya. Değiştirin `deviceConnectionString` , IOT hub'dan cihaz bağlantı dizesiyle:
   
    ```python
    import time, datetime
    import sys
    import threading

    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult
    from iothub_client import IoTHubError, DeviceMethodReturnValue

    SEND_REPORTED_STATE_CONTEXT = 0
    METHOD_CONTEXT = 0

    MESSAGE_COUNT = 10

    PROTOCOL = IoTHubTransportProvider.MQTT
    CONNECTION_STRING = "{deviceConnectionString}"
    CLIENT = IoTHubClient(CONNECTION_STRING, PROTOCOL)
    ```

1. Bildirilen özellikler güncelleştirmeleri sağlamak ve doğrudan yöntemi uygulamak için kullanılan aşağıdaki işlevleri ekleyin:
   
    ```python
    def send_reported_state_callback(status_code, user_context):
        print ( "Reported state updated." )

    def device_method_callback(method_name, payload, user_context):
        if method_name == "firmwareUpdate":
            print ( "Starting firmware update." )
            image_url = payload
            thr = threading.Thread(target=simulate_download_image, args=([payload]), kwargs={})
            thr.start()
    
            device_method_return_value = DeviceMethodReturnValue()
            device_method_return_value.response = "{ \"Response\": \"Firmware update started\" }"
            device_method_return_value.status = 200
            return device_method_return_value
    ```

1. Üretici görüntüsünü indirip karşıya yüklemeyi taklit eden aşağıdaki işlevleri ekleyin:
   
    ```python
    def simulate_download_image(image_url):
        time.sleep(15)
        print ( "Downloading image from: " + image_url )

        reported_state = "{\"firmwareStatus\":\"downloading\", \"downloadComplete\":\"" + str(datetime.datetime.now()) + "\"}"
        CLIENT.send_reported_state(reported_state, len(reported_state), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)
        time.sleep(15)
    
        simulate_apply_image(image_url)
    
    def simulate_apply_image(image_url):
        print ( "Applying image from: " + image_url )

        reported_state = "{\"firmwareStatus\":\"applying\", \"startedApplyingImage\":\"" + str(datetime.datetime.now()) + "\"}"
        CLIENT.send_reported_state(reported_state, len(reported_state), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)
        time.sleep(15)
    
        simulate_complete_image()

    def simulate_complete_image():
        print ( "Image applied." )

        reported_state = "{\"firmwareStatus\":\"completed\", \"lastFirmwareUpdate\":\"" + str(datetime.datetime.now()) + "\"}"
        CLIENT.send_reported_state(reported_state, len(reported_state), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)
    ```

8. Bildirilen özellikler cihaz ikizinin başlatan aşağıdaki işlevi ekleyin ve çağrılacak doğrudan yöntemin için bekleyin. Ayrıca `main` yordam:
   
    ```python
    def iothub_firmware_sample_run():
        try:
            CLIENT.set_device_method_callback(device_method_callback, METHOD_CONTEXT)

            reported_state = "{\"firmwareStatus\":\"standBy\", \"logTime\":\"" + str(datetime.datetime.now()) + "\"}"
            CLIENT.send_reported_state(reported_state, len(reported_state), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)
            print ( "Device twins initialized." )
            print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )
        
            while True:
                status_counter = 0
                while status_counter <= MESSAGE_COUNT:
                    time.sleep(10)
                    status_counter += 1
            
        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Python firmware update sample..." )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_firmware_sample_run()
    ```

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda yeniden deneme ilkelerini (örneğin, bir üstel geri alma), MSDN makalesinde önerildiği uygulamalıdır [geçici hata işleme](https://msdn.microsoft.com/library/hh675232.aspx).
> 


## <a name="run-the-apps"></a>Uygulamaları çalıştırma
Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Komut isteminde, yeniden başlatma doğrudan yöntem için dinleme başlamak için aşağıdaki komutu çalıştırın.
   
    ```cmd/sh
    python dmpatterns_fwupdate_device.py
    ```

1. Başka bir komut isteminde Uzaktan yeniden başlatma ve son bulmak cihaz ikizi sorgusu zaman yeniden tetikleyici için aşağıdaki komutu çalıştırın.
   
    ```cmd/sh
    python dmpatterns_fwupdate_service.py
    ```

1. Doğrudan yöntem konsolunda cihaz yanıtı görürsünüz. Ardından, bildirilen özellikleri üretici yazılımı güncelleştirme boyunca değişiklik unutmayın.

    ![program çıktısı][1]


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir cihazda bir uzak üretici yazılımı güncelleştirmesini tetikleme için kullanılan bir doğrudan yöntem ve bildirilen özellikleri üretici yazılımı güncelleştirme durumunu izlemek için kullanılır.

IOT çözümü ve zamanlama yöntemi çağıran birden çok cihazda genişletmek öğrenmek için bkz [işleri zamanlama ve yayınlama] [ lnk-tutorial-jobs] öğretici.

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[1]: ./media/iot-hub-python-python-firmware-update/1.png
