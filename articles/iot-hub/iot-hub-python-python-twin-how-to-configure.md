---
title: "Azure IOT Hub cihaz çifti özellikleri (Python) kullanın | Microsoft Docs"
description: "Azure IOT Hub cihaz çiftlerini cihazları yapılandırmak için nasıl kullanılacağını. Sanal cihaz uygulaması ve cihaz çifti kullanarak bir aygıt yapılandırmasını değiştirir bir hizmet uygulaması uygulamak için Python için Azure IOT SDK'ları kullanın."
services: iot-hub
documentationcenter: .net
author: msebolt
manager: timlt
editor: 
ms.assetid: d0bcec50-26e6-40f0-8096-733b2f3071ec
ms.service: iot-hub
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/12/2018
ms.author: v-masebo
ms.openlocfilehash: d0d5a30a76068eb3212124fd14e7ea1616b75708
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="use-desired-properties-to-configure-devices-python"></a>İstenen özelliklerde cihazları (Python) yapılandırmak için kullanın
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Bu öğreticinin sonunda iki Python konsol uygulamaları olacaktır:

* **SimulateDeviceConfiguration.py**, istenen yapılandırma güncelleştirmesi bekler ve benzetimli yapılandırmasını güncelleştirme işleminin durumunu raporlar bir sanal cihaz uygulaması.
* **SetDesiredConfigurationAndQuery.py**, istenen yapılandırma bir cihazda ayarlar ve yapılandırmayı güncelleştirme işlemini sorgular Python arka uç uygulaması.

> [!NOTE]
> Makaleyi [Azure IOT SDK'ları] [ lnk-hub-sdks] hem cihaz hem de arka uç uygulamalar oluşturmak için kullanabileceğiniz Azure IOT SDK'ları hakkında bilgi sağlar.
> 
> 

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* [Python 2.x veya 3.x][lnk-python-download]. Kurulumunuzun gereksinimine uygun olarak 32 bit veya 64 bit yüklemeyi kullanmaya dikkat edin. Yükleme sırasında istendiğinde, platforma özgü ortam değişkeninize Python’u eklediğinizden emin olun. Python 2.x kullanıyorsanız, [Python paket yönetim sistemi *pip*’yi yüklemeniz veya yükseltmeniz][lnk-install-pip] gerekebilir.
* Windows işletim sistemi kullanıyorsanız, Python’dan yerel DLL’lerin kullanımına olanak tanımak için [Visual C++ yeniden dağıtılabilir paketi][lnk-visual-c-redist].
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

İzlediyseniz [cihaz çiftlerini ile çalışmaya başlama] [ lnk-twin-tutorial] öğretici, zaten sahip olduğunuz bir IOT hub ve adlı bir cihaz kimliği **myDeviceId**; ve atlayabilirsiniz[Sanal cihaz uygulaması oluşturma] [ lnk-how-to-configure-createapp] bölümü.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="create-the-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma
Bu bölümde, hub'ınıza bağlanan bir Python konsol uygulaması oluşturma **myDeviceId**, istenen yapılandırma güncelleştirmesi bekler ve daha sonra güncelleştirmeleri benzetimli yapılandırmasını güncelleştirme işlemini raporlar.

1. Yükleme **Azure IOT Python cihaz SDK'sı** , komut isteminde aşağıdaki komutu kullanarak:
   
    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Bir metin düzenleyicisi kullanarak yeni bir oluşturma **SimulateDeviceConfiguration.py** dosya.

1. Aşağıdaki kodu ekleyin **SimulateDeviceConfiguration.py** dosya ve yerine **{cihaz bağlantı dizesi}** oluşturduğunuzdakopyaladığınızcihazbağlantıdizesiyleyertutucusu**myDeviceId** cihaz kimliği:

    ```python   
    import time
    import sys

    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError

    CONNECTION_STRING = "{deviceConnectionString}"
    PROTOCOL = IoTHubTransportProvider.MQTT

    CLIENT = IoTHubClient(CONNECTION_STRING, PROTOCOL)

    WAIT_COUNT = 5

    TWIN_CONTEXT = 0
    SEND_REPORTED_STATE_CONTEXT = 0

    TWIN_CALLBACKS = 0
    SEND_REPORTED_STATE_CALLBACKS = 0

    CONFIG_ID = "1"
    TWIN_PAYLOAD = "{\"configId\":" + CONFIG_ID + ",\"sendFrequency\":\"24h\"}"
    ```

    **İstemci** nesne cihaz çiftlerini aygıttan ile etkileşim kurmak için gereken tüm yöntemleri gösterir. Daha fazla kod içinde istenen özellikler, güncelleştirme için bir işleyici ekleyecek sonra ekleme olduğunu gerçek yapılandırması değişiklik isteğinin yapılandırmasında değişiklik başlatan bir yöntemi çağırır configIds karşılaştırarak doğrulamak için bir ek işleyicisi . Basitleştirmek amacıyla, önceki kod sabit kodlanmış bir varsayılan ilk yapılandırmasını kullanır. Gerçek bir uygulama büyük olasılıkla bu yapılandırma yerel depolama biriminden yüklenir.
   
   > [!IMPORTANT]
   > İstenen özellik değiştirme olayları her zaman bir kez aygıt bağlantıdaki gösterilen, herhangi bir işlem gerçekleştirmeden önce İstenen özelliklerde gerçek bir değişiklik olup olmadığını denetlemek emin olun.
   > 

1. Sonuna aşağıdaki kodu ekleyin **SimulateDeviceConfiguration.py** dosyası:

    ```python
    def device_twin_callback(update_state, payload, user_context):
        global TWIN_CALLBACKS
        global CONFIG_ID

        desired_configId = payload[payload.find("configId")+11:payload.find("configId")+12]
        if CONFIG_ID != desired_configId:
            print ( "" )
            print ( "Reported pending config change: %s" % payload)
        
            desired_sendFrequency = payload[payload.find("sendFrequency")+17:payload.find("sendFrequency")+19]
            print ( "...desired configId: " + desired_configId)
            print ( "...desired sendFrequency: " + desired_sendFrequency)
            new_payload = "{\"configId\":" + desired_configId + ",\"sendFrequency\":\"" + desired_sendFrequency + "\"}"
            CLIENT.send_reported_state(new_payload, len(new_payload), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)
        
        CONFIG_ID = desired_configId
    
    def send_reported_state_callback(status_code, user_context):
        global SEND_REPORTED_STATE_CALLBACKS
    
        print ( "" )
        print ( "Device twins updated." )
    ```
   
    **Device_twin_callback** yöntemi güncelleştirmeleri bildirilen yapılandırmasıyla yerel cihaz çifti nesne üzerindeki özellikleri güncelleştirme isteği ve ayarlar _configId_. Cihaz çifti başarıyla güncelleştirdikten sonra bir güncelleştirme iletisi yazdırır. Bant genişliğinden tasarruf için bildirilen özellikler yalnızca değiştirilecek özelliklerini belirterek güncelleştirilir (adlı **TWIN_PAYLOAD** Yukarıdaki koddaki), tüm belgeyi değiştirme yerine.
   
   > [!NOTE]
   > Bu öğretici herhangi davranışı eşzamanlı yapılandırma güncelleştirmelerini benzetimini değil. Bazı yapılandırma güncelleştirme işlemleri hedef yapılandırma değişiklikleri güncelleştirme çalışan, diğerleri bunları kuyruğuna olabilir ve diğerleri ile bir hata koşulu reddedebilirsiniz sırada karşılamak mümkün olabilir. Belirli bir yapılandırma işlemi için istenen davranışı dikkate aldığınızdan emin olun ve yapılandırma değişikliğini başlatmadan önce uygun mantığı ekleyin.
   > 

1. Sonuna aşağıdaki kodu ekleyin **SimulateDeviceConfiguration.py** dosyası: 

    ```python
    def iothub_client_init():
        if CLIENT.protocol == IoTHubTransportProvider.MQTT or client.protocol == IoTHubTransportProvider.MQTT_WS:
            CLIENT.set_device_twin_callback(device_twin_callback, TWIN_CONTEXT)

    def iothub_client_sample_run():
        try:
            iothub_client_init()
        
            CLIENT.send_reported_state(TWIN_PAYLOAD, len(TWIN_PAYLOAD), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)

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

1. Cihaz uygulaması çalıştırın:
   
    ```cmd/sh
    node SimulateDeviceConfiguration.js
    ```
   
    Şu iletiyi görürsünüz `Device twins updated.`. Uygulamayı çalıştıran tutun.


## <a name="create-the-service-app"></a>Hizmet Uygulaması Oluştur
Bu bölümde, güncelleştirmeleri bir Python konsol uygulaması oluşturacak *özelliklerini istenen* ilişkili cihaz çifti üzerinde **myDeviceId** ile yeni bir telemetri yapılandırma nesnesi. IOT hub'ı depolanan cihaz çiftlerini sorgular ve cihazın istenen ve bildirilen yapılandırmaları arasındaki farkı gösterir.

1. Yükleme **Azure IOT Python hizmet SDK'sı** , komut isteminde aşağıdaki komutu kullanarak:
   
    ```cmd/sh
    pip install azure-iothub-service-client
    ```

1. Bir metin düzenleyicisi kullanarak yeni bir oluşturma **SetDesiredAndQuery.py** dosya.

1. Aşağıdaki kodu ekleyin **SetDesiredAndQuery.py** dosya ve yerine **{IoTHubConnectionString}** yer tutucu IOT Hub bağlantı dizesiyle hub'ınızı oluşturduğunuzda kopyaladığınız ve **{DeviceID}** yer tutucusunu, cihaz adı ile:

    ```python
    import sys, time
    import iothub_service_client

    from iothub_service_client import IoTHubError, IoTHubDeviceTwin

    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"

    UPDATE_JSON = "{\"properties\":{\"desired\":{\"configId\":3, \"sendFrequency\":\"" + sys.argv[1:][0] + "\"}}}"
    TIMEOUT = 60
    WAIT_COUNT = 10
    ```

1. Sonuna aşağıdaki kodu ekleyin **SetDesiredAndQuery.py** dosyası:

    ```python
    def iothub_devicetwin_sample_run():
        try:
            iothub_twin_method = IoTHubDeviceTwin(CONNECTION_STRING)
        
            twin_info = iothub_twin_method.get_twin(DEVICE_ID)
            print ( "" )
            print ( "Device Twin before update    :" )
            print ( "{0}".format(twin_info) )

            twin_info = iothub_twin_method.update_twin(DEVICE_ID, UPDATE_JSON)
            print ( "" )
            print ( "Device Twin after update     :" )
            print ( "{0}".format(twin_info) )
        
            while True:
                time.sleep(10)

                twin_info = iothub_twin_method.get_twin(DEVICE_ID)
                print ( "" )
                print ( "Device Twin after client change initiated    :" )
                print ( "{0}".format(twin_info) )
        
                print ( "" )
                print ( "IoT Hub service sample complete, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    time.sleep(30)
                
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "" )
            print ( "Unexpected error {0}".format(iothub_error) )
            return
        except KeyboardInterrupt:
            print ( "" )
            print ( "IoTHubDeviceMethod sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Service Client DeviceTwins Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_devicetwin_sample_run()
    ```

1. İle **SimulateDeviceConfiguration.py** , uygulama ile yeni bir komut istemindeki çalıştırma:

    ```cmd/sh
    python SetDesiredAndQuery.py 5m
    ```
   
    Çıkarılıp bildirilen yapılandırması kimliği görmelisiniz **1** için **2** yeni etkin 24 saat yerine beş dakikalık sıklığı gönderin.


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, istenen yapılandırma olarak ayarlamak *özelliklerini istenen* bir arka uç uygulamasından ve bu değişikliği algılar ve bir güncelleştirme işleminin durumunu olarak raporlama benzetimini yapmak için bir sanal cihaz uygulaması yazdı *bildirdi Özellikler* cihaz çifti için.

Bilgi edinmek için aşağıdaki kaynakları kullanın nasıl yapılır:

* aygıtlarla telemetri gönderen [IOT Hub ile çalışmaya başlama] [ lnk-iothub-getstarted] öğretici
* zamanlama veya aygıtları bakın büyük kümeleri işlemleri [zamanlama ve yayın işleri] [ lnk-schedule-jobs] Öğreticisi.
* ile etkileşimli olarak (örneğin, kullanıcı tarafından denetlenen bir uygulamadan fan etkinleştirdikten), cihazları denetleme [doğrudan yöntemleri kullanın] [ lnk-methods-tutorial] Öğreticisi.

<!-- links -->
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-python-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-iothub-getstarted]: iot-hub-python-getstarted.md
[lnk-methods-tutorial]: iot-hub-python-python-direct-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app
