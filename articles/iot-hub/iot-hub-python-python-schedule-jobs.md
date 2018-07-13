---
title: Azure IOT hub'ı (Python) ile işleri zamanlama | Microsoft Docs
description: Birden fazla cihazda doğrudan yöntem çağırmak için bir Azure IOT hub'ı işini zamanlamak nasıl. Python için Azure IOT SDK'ları, sanal cihaz uygulamaları ve işi çalıştırmak için bir hizmet uygulaması'nı uygulamak için kullanın.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 02/16/2018
ms.author: kgremban
ms.openlocfilehash: 7cbfe289f662987d85f0f2678e4971492ed8cd80
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38666963"
---
# <a name="schedule-and-broadcast-jobs-python"></a>Zamanlama ve yayınlama işleri (Python)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Azure IOT Hub oluşturma ve zamanlama ve milyonlarca cihaza güncelleştirme işleri izlemek bir arka uç uygulaması sağlayan tam olarak yönetilen bir hizmettir.  İşleri için aşağıdaki eylemler kullanılabilir:

* İstenen özellikleri güncelleştirme
* Etiketleri güncelleştirin
* Doğrudan metotları çağırma

Kavramsal olarak, bir işi bu eylemlerden biri, sarmalar ve bir cihaz ikizi sorgu tarafından tanımlanan bir dizi cihazda karşı yürütme ilerleme durumunu izler.  Örneğin, bir arka uç uygulaması, bir işi 10.000 cihaz, cihaz ikizi sorgu tarafından belirtilen ve gelecekteki zaman için programlanmış bir yeniden başlatma yöntemini çağırmak için kullanabilirsiniz.  Bu cihazların her biri alın ve yeniden başlatma yöntemi uygulamak, uygulamanın sonra ilerleme durumunu izleyebilirsiniz.

Bu makaleler, bu özelliklerin her biri hakkında daha fazla bilgi edinin:

* Cihaz ikizi ve özellikleri: [cihaz ikizlerini kullanmaya başlama] [ lnk-get-started-twin] ve [öğretici: cihaz ikizi özelliklerini kullanma][lnk-twin-props]
* Doğrudan yöntemler: [IOT Hub Geliştirici Kılavuzu - doğrudan yöntemler] [ lnk-dev-methods] ve [Öğreticisi: doğrudan yöntemler][lnk-c2d-methods]

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* Sağlayan bir doğrudan yöntem olan Python sanal cihaz uygulaması oluşturma **lockDoor**, hangi çağrılabilir çözüm arka ucu tarafından.
* Çağıran bir Python konsol uygulaması oluşturacaksınız **lockDoor** yöntemi bir iş ve güncelleştirmeleri kullanarak bir cihaz iş istenen özellikleri kullanarak sanal cihaz uygulamasında doğrudan.

Bu öğreticinin sonunda iki Python uygulamaları vardır:

**simDevice.py**, cihaz kimliğiyle IOT hub'ınıza bağlanır ve aldığı kaydeden bir **lockDoor** doğrudan yöntemi.

**scheduleJobService.py**, sanal cihaz uygulamasında bir doğrudan yöntem çağrıları ve cihaz ikizinin güncelleştirmeleri kullanarak bir proje özelliklerini istenen.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* [Python 2.x veya 3.x][lnk-python-download]. Kurulumunuzun gereksinimine uygun olarak 32 bit veya 64 bit yüklemeyi kullanmaya dikkat edin. Yükleme sırasında istendiğinde, platforma özgü ortam değişkeninize Python’u eklediğinizden emin olun. Python 2.x kullanıyorsanız, [Python paket yönetim sistemi *pip*’yi yüklemeniz veya yükseltmeniz][lnk-install-pip] gerekebilir.
* Windows işletim sistemi kullanıyorsanız, Python’dan yerel DLL’lerin kullanımına olanak tanımak için [Visual C++ yeniden dağıtılabilir paketi][lnk-visual-c-redist].
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

> [!NOTE]
> **Python için Azure IOT SDK'sı** doğrudan desteklemez **işleri** işlevselliği. Bunun yerine Bu öğreticide, zaman uyumsuz iş parçacıkları ve zamanlayıcılar kullanan alternatif bir çözüm sunar. Daha fazla güncelleştirmeler için bkz **hizmeti istemci SDK'sı** özellik listesinde [Python için Azure IOT SDK'sı](https://github.com/Azure/azure-iot-sdk-python) sayfası. 
> 
> 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]


## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma
Bu bölümde, bir sanal tetikler bulut tarafından çağrılan doğrudan bir yönteme yanıt veren bir Python konsol uygulaması oluşturma **lockDoor** yöntemi.

1. Komut isteminizde yüklemek için aşağıdaki komutu çalıştırın **azure IOT cihaz istemci** paket:
   
    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Bir metin düzenleyicisi kullanarak yeni bir oluşturma **simDevice.py** çalışma dizininizde dosya.

1. Aşağıdaki `import` deyimlerini ve değişkenlerini başlangıcında **simDevice.py** dosya. Değiştirin `deviceConnectionString` yukarıda oluşturduğunuz cihaz bağlantı dizesiyle:
   
    ```python
    import time
    import sys

    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult
    from iothub_client import IoTHubError, DeviceMethodReturnValue

    METHOD_CONTEXT = 0
    TWIN_CONTEXT = 0
    WAIT_COUNT = 10

    PROTOCOL = IoTHubTransportProvider.MQTT
    CONNECTION_STRING = "{deviceConnectionString}"
    ```

1. İşlemek için aşağıdaki işlev geri çağırma ekleme **lockDoor** yöntemi:
   
    ```python
    def device_method_callback(method_name, payload, user_context):
        if method_name == "lockDoor":
            print ( "Locking Door!" )

            device_method_return_value = DeviceMethodReturnValue()
            device_method_return_value.response = "{ \"Response\": \"lockDoor called successfully\" }"
            device_method_return_value.status = 200
            return device_method_return_value
    ```

1. Cihaz ikizlerini güncelleştirmeleri işlemek için başka bir işlev geri çağırma ekleyin:

    ```python
    def device_twin_callback(update_state, payload, user_context):
        print ( "")
        print ( "Twin callback called with:")
        print ( "payload: %s" % payload )
    ```

1. İşleyicisini kaydetmek için aşağıdaki kodu ekleyin **lockDoor** yöntemi. De `main` yordam:
   
    ```python
    def iothub_jobs_sample_run():
        try:
            client = IoTHubClient(CONNECTION_STRING, PROTOCOL)
            client.set_device_method_callback(device_method_callback, METHOD_CONTEXT)
            client.set_device_twin_callback(device_twin_callback, TWIN_CONTEXT)

            print ( "Direct method initialized." )
            print ( "Device twin callback initialized." )
            print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )
        
            while True:
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
        print ( "Starting the IoT Hub Python jobs sample..." )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_jobs_sample_run()
    ```

1. Kaydet ve Kapat **simDevice.py** dosya.

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. [Geçici Hata İşleme][lnk-transient-faults] adlı MSDN makalesinde önerildiği üzere, üretim kodunda yeniden deneme ilkelerini (üstel geri alma gibi) uygulamanız gerekir.
> 
> 


## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a>Bir doğrudan yöntem çağırma ve bir cihaz ikizinin özelliklerini güncelleştirmek için işleri zamanlama
Bu bölümde, bir uzak başlatan bir Python konsol uygulaması oluşturma **lockDoor** bir cihazda doğrudan yöntem kullanarak ve cihaz ikizinin özelliklerini güncelleştirir.

1. Komut isteminizde yüklemek için aşağıdaki komutu çalıştırın **azure-IOT-service-client** paket:
   
    ```cmd/sh
    pip install azure-iothub-service-client
    ```

1. Bir metin düzenleyicisi kullanarak yeni bir oluşturma **scheduleJobService.py** çalışma dizininizde dosya.

1. Aşağıdaki `import` deyimlerini ve değişkenlerini başlangıcında **scheduleJobService.py** dosyası:
   
    ```python
    import sys
    import time
    import threading
    import uuid

    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod
    from iothub_service_client import IoTHubDeviceTwin, IoTHubDeviceMethod, IoTHubError

    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"

    METHOD_NAME = "lockDoor"
    METHOD_PAYLOAD = "{\"lockTime\":\"10m\"}"
    UPDATE_JSON = "{\"properties\":{\"desired\":{\"building\":43,\"floor\":3}}}"
    TIMEOUT = 60
    WAIT_COUNT = 5
    ```

1. Kullanılan aşağıdaki işlevi ekleyin sorgulamak cihazlar için:
   
    ```python
    def query_condition(device_id):
        iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)
    
        number_of_devices = 10
        dev_list = iothub_registry_manager.get_device_list(number_of_devices)
    
        for device in range(0, number_of_devices):
            if dev_list[device].deviceId == device_id:
                return 1

        print ( "Device not found" )
        return 0
    ```

1. Doğrudan yöntem ve cihaz ikizi çağrı işlerini çalıştırmak için aşağıdaki yöntemleri ekleyin:
   
    ```python
    def device_method_job(job_id, device_id, wait_time, execution_time):
        print ( "" )
        print ( "Scheduling job: " + str(job_id) )
        time.sleep(wait_time)
    
        if query_condition(device_id):
            iothub_device_method = IoTHubDeviceMethod(CONNECTION_STRING)
    
            response = iothub_device_method.invoke(device_id, METHOD_NAME, METHOD_PAYLOAD, TIMEOUT)
        
            print ( "" )
            print ( "Direct method " + METHOD_NAME + " called." )
        
    def device_twin_job(job_id, device_id, wait_time, execution_time):
        print ( "" )
        print ( "Scheduling job " + str(job_id) )
        time.sleep(wait_time)
    
        if query_condition(device_id):
            iothub_twin_method = IoTHubDeviceTwin(CONNECTION_STRING)
    
            twin_info = iothub_twin_method.update_twin(DEVICE_ID, UPDATE_JSON)
        
            print ( "" )
            print ( "Device twin updated." )
    ```

1. İşleri zamanlama ve iş durumu güncelleştirmek için aşağıdaki kodu ekleyin. De `main` yordam:
   
    ```python
    def iothub_jobs_sample_run():
        try:
            method_thr_id = uuid.uuid4()
            method_thr = threading.Thread(target=device_method_job, args=(method_thr_id, DEVICE_ID, 20, TIMEOUT), kwargs={})
            method_thr.start()
        
            print ( "" )
            print ( "Direct method called with Job Id: " + str(method_thr_id) )
        
            twin_thr_id = uuid.uuid4()
            twin_thr = threading.Thread(target=device_twin_job, args=(twin_thr_id, DEVICE_ID, 10, TIMEOUT), kwargs={})
            twin_thr.start()
        
            print ( "" )
            print ( "Device twin called with Job Id: " + str(twin_thr_id) )
        
            while True:
                print ( "" )
            
                if method_thr.is_alive():
                    print ( "...job " + str(method_thr_id) + " still running." )
                else:
                    print ( "...job " + str(method_thr_id) + " complete." )
            
                if twin_thr.is_alive():
                    print ( "...job " + str(twin_thr_id) + " still running." )
                else:
                    print ( "...job " + str(twin_thr_id) + " complete." )
                
                print ( "Job status posted, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
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
        print ( "Starting the IoT Hub jobs Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_jobs_sample_run()
    ```

1. Kaydet ve Kapat **scheduleJobService.py** dosya.


## <a name="run-the-applications"></a>Uygulamaları çalıştırma
Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Çalışma dizininizdeki komut isteminde, yeniden başlatma doğrudan yöntem için dinleme başlamak için aşağıdaki komutu çalıştırın:
   
    ```cmd/sh
    python simDevice.py
    ```

1. Çalışma dizininizdeki başka bir komut isteminde işlerinin kapısını kilitleme ve ikiz güncelleştirmesi tetiklemek için aşağıdaki komutu çalıştırın:
   
    ```cmd/sh
    python scheduleJobService.py
    ```

1. Doğrudan yöntem cihaz yanıtlarını görebilir ve cihaz ikizleri konsolda güncelleştirilmesi.

    ![cihaz çıktısı][1]

    ![Hizmet çıkış][2]


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir işi bir doğrudan yöntem bir cihaz ve cihaz ikizinin özelliklerini güncelleştirme için zamanlamak için kullanılır.

IOT Hub ve cihaz yönetim modellerini uzaktan gibi hava üretici yazılımı güncelleştirme kullanmaya başlama devam etmek için bkz:

[Öğretici: nasıl üretici yazılımlarını güncelleştirme][lnk-fwupdate]

IOT Hub kullanmaya başlamaya devam etmek için bkz: [Azure IOT Edge'i kullanmaya başlama][lnk-iot-edge].

[lnk-get-started-twin]: iot-hub-python-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-python-python-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[lnk-transient-faults]: https://docs.microsoft.com/azure/architecture/best-practices/transient-faults

[1]: ./media/iot-hub-python-python-schedule-jobs/1.png
[2]: ./media/iot-hub-python-python-schedule-jobs/2.png
