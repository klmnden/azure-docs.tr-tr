---
title: Zamanlama işlerini ile Azure IOT hub'ı (Python) | Microsoft Docs
description: Birden çok aygıta doğrudan bir yöntemi çağırmak için bir Azure IOT Hub işini zamanlamak nasıl. Sanal cihaz uygulamaları ve işi çalıştırmak için bir hizmet uygulaması uygulamak için Python için Azure IOT SDK'ları kullanın.
services: iot-hub
documentationcenter: .net
author: kgremban
manager: timlt
editor: ''
ms.assetid: 2233356e-b005-4765-ae41-3a4872bda943
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/16/2018
ms.author: v-masebo;kgremban
ms.openlocfilehash: bb087d3fce8d663995a3878951477df8343dca00
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="schedule-and-broadcast-jobs-python"></a>Zamanlama ve yayın işleri (Python)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Azure IOT Hub oluşturma ve zamanlama ve milyonlarca cihaza güncelleştirme işleri izlemek için bir arka uç uygulaması sağlayan tam olarak yönetilen bir hizmettir.  İşlerini için aşağıdaki eylemler kullanılabilir:

* İstenen özellikleri güncelleştirme
* Güncelleştirme etiketleri
* Doğrudan yöntemleri çağırma

Kavramsal olarak, bir işi şu eylemlerden birini sarmalar ve bir cihaz çifti sorgu tarafından tanımlanan bir dizi aygıtlar, yürütme ilerlemesini izler.  Örneğin, bir arka uç uygulaması, bir iş 10.000 cihaz, cihaz çifti sorgu tarafından belirtilen ve gelecek bir zamanda zamanlanmış bir yeniden başlatma yöntemini çağırmak için kullanabilirsiniz.  Bu cihazların her biri almak ve yeniden başlatma yöntemini çalıştırmak gibi uygulama sonra ilerleme durumunu izleyebilirsiniz.

Bu makaleler, bu özelliklerin her biri hakkında daha fazla bilgi edinin:

* Cihaz çifti ve özellikleri: [cihaz çiftlerini ile çalışmaya başlama] [ lnk-get-started-twin] ve [öğretici: cihaz çifti özellikleri kullanma][lnk-twin-props]
* Doğrudan yöntemleri: [IOT Hub Geliştirici Kılavuzu - doğrudan yöntemleri] [ lnk-dev-methods] ve [Öğreticisi: doğrudan yöntemleri][lnk-c2d-methods]

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* Etkinleştirir doğrudan bir yönteme sahip bir Python sanal cihaz uygulaması oluşturma **lockDoor**, hangi çağrılabilir çözüm arka ucu tarafından.
* Çağıran bir Python konsol uygulaması oluşturma **lockDoor** yöntemi bir işi ve güncelleştirmeleri cihaz işi kullanarak istenen özelliklerini kullanan sanal cihaz uygulamasında doğrudan.

Bu öğreticinin sonunda, iki Python uygulamaları vardır:

**simDevice.py**, hangi cihaz kimliğiyle IOT hub'ınıza bağlanır ve alan bir **lockDoor** doğrudan yöntemi.

**scheduleJobService.py**, sanal cihaz uygulamasının doğrudan bir yöntemi çağırır ve cihaz çifti 's güncelleştirmeleri bir işi kullanarak özelliklerini istenen.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* [Python 2.x veya 3.x][lnk-python-download]. Kurulumunuzun gereksinimine uygun olarak 32 bit veya 64 bit yüklemeyi kullanmaya dikkat edin. Yükleme sırasında istendiğinde, platforma özgü ortam değişkeninize Python’u eklediğinizden emin olun. Python 2.x kullanıyorsanız, [Python paket yönetim sistemi *pip*’yi yüklemeniz veya yükseltmeniz][lnk-install-pip] gerekebilir.
* Windows işletim sistemi kullanıyorsanız, Python’dan yerel DLL’lerin kullanımına olanak tanımak için [Visual C++ yeniden dağıtılabilir paketi][lnk-visual-c-redist].
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

> [!NOTE]
> **Python için Azure IOT SDK'sı** doğrudan desteklemediği **işleri** işlevselliği. Bunun yerine Bu öğretici zaman uyumsuz iş parçacığı ve zamanlayıcılar kullanan alternatif bir çözüm sunar. Daha fazla güncelleştirmeler için bkz: **hizmeti istemci SDK'sı** özellik listesi [Python için Azure IOT SDK'sı](https://github.com/Azure/azure-iot-sdk-python) sayfası. 
> 
> 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]


## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma
Bu bölümde, sanal tetikler bulut tarafından adlı doğrudan bir yöntem yanıtlar bir Python konsol uygulaması oluşturma **lockDoor** yöntemi.

1. Komut isteminizde yüklemek için aşağıdaki komutu çalıştırın **azure IOT cihaz istemci** paketi:
   
    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Bir metin düzenleyicisi kullanarak yeni bir oluşturma **simDevice.py** çalışma dizininizi dosyasında.

1. Aşağıdakileri ekleyin `import` deyimleri ve değişkenleri başlangıcında **simDevice.py** dosya. Değiştir `deviceConnectionString` yukarıda oluşturduğunuz cihaz bağlantı dizesiyle:
   
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

1. İşlemek için aşağıdaki işlevi geri çağırma eklemek **lockDoor** yöntemi:
   
    ```python
    def device_method_callback(method_name, payload, user_context):
        if method_name == "lockDoor":
            print ( "Locking Door!" )

            device_method_return_value = DeviceMethodReturnValue()
            device_method_return_value.response = "{ \"Response\": \"lockDoor called successfully\" }"
            device_method_return_value.status = 200
            return device_method_return_value
    ```

1. Cihaz çiftlerini güncelleştirmeleri işlemek için başka bir işlev geri çağırma ekleyin:

    ```python
    def device_twin_callback(update_state, payload, user_context):
        print ( "")
        print ( "Twin callback called with:")
        print ( "payload: %s" % payload )
    ```

1. İşleyicisi kaydetmek için aşağıdaki kodu ekleyin **lockDoor** yöntemi. De dahil `main` yordamı:
   
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


## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a>İşlerini doğrudan bir yöntemi çağırmak ve bir cihaz çifti'nın özelliklerini güncelleştirmek için zamanla
Bu bölümde, bir uzak başlatan bir Python konsol uygulaması oluşturma **lockDoor** bir aygıtta doğrudan bir yöntem kullanarak ve cihaz çifti'nın özelliklerini güncelleştirir.

1. Komut isteminizde yüklemek için aşağıdaki komutu çalıştırın **azure-IOT-service-client** paketi:
   
    ```cmd/sh
    pip install azure-iothub-service-client
    ```

1. Bir metin düzenleyicisi kullanarak yeni bir oluşturma **scheduleJobService.py** çalışma dizininizi dosyasında.

1. Aşağıdakileri ekleyin `import` deyimleri ve değişkenleri başlangıcında **scheduleJobService.py** dosyası:
   
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

1. Kullanılan aşağıdaki işlevi ekleyin sorgulamak aygıtları için:
   
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

1. Doğrudan yöntemi ve cihaz çifti çağrı işlerini çalıştırmak için aşağıdaki yöntemleri ekleyin:
   
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

1. İşlerini zamanla ve iş durumunu güncelleştirmek için aşağıdaki kodu ekleyin. De dahil `main` yordamı:
   
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

1. Çalışma dizini içinde komut isteminde yeniden başlatma doğrudan yöntemi için dinleme başlamak için aşağıdaki komutu çalıştırın:
   
    ```cmd/sh
    python simDevice.py
    ```

1. Çalışma dizini içinde başka bir komut isteminde kapısını kilitleme ve twin güncelleştirmek için işleri tetiklemek için aşağıdaki komutu çalıştırın:
   
    ```cmd/sh
    python scheduleJobService.py
    ```

1. Doğrudan yöntemi aygıt yanıtlarını bakın ve cihaz çiftlerini konsolda güncelleştirin.

    ![cihaz çıktısı][1]

    ![Hizmet çıktı][2]


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir cihaz ve cihaz çifti'nın özelliklerini güncelleştirmek için doğrudan bir yöntem zamanlamak için bir iş kullanılır.

IOT Hub ve Uzaktan gibi cihaz yönetim düzenleri hava bellenim güncelleştirme kullanmaya Başlarken devam etmek için bkz:

[Öğretici: bir üretici yazılımı güncelleştirmesi yapma][lnk-fwupdate]

IOT Hub ile çalışmaya başlama devam etmek için bkz: [Azure IOT Edge ile çalışmaya başlama][lnk-iot-edge].

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
