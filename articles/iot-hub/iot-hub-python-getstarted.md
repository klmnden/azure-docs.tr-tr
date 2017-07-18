---
title: "Azure IoT Hub'ı (Python) kullanmaya başlama | Microsoft Belgeleri"
description: "Python için IoT SDK’larını kullanarak Azure IoT Hub’a cihazdan buluta ileti göndermeyi öğrenin. IoT hub’a cihazınızı kaydetmek, ileti göndermek ve ileti okumak için sanal cihaz ve hizmet uygulamaları oluşturun."
services: iot-hub
author: dsk-2015
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: python
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/22/2017
ms.author: dkshir
ms.custom: na
ms.translationtype: HT
ms.sourcegitcommit: 54454e98a2c37736407bdac953fdfe74e9e24d37
ms.openlocfilehash: 05268924a182575b3df66fb6dad6bcac2700ec0c
ms.contentlocale: tr-tr
ms.lasthandoff: 07/13/2017

---
# <a name="connect-your-simulated-device-to-your-iot-hub-using-python"></a>Python kullanarak sanal cihazınızı IoT hub’ınıza bağlama
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Bu öğreticinin sonunda iki Python uygulamanız olacaktır:

* Bir cihaz kimliği ve sanal cihaz uygulamanızı bağlamak için ilişkili güvenlik anahtarı oluşturan **CreateDeviceIdentity.py**.
* Daha önce oluşturulan cihaz kimliğiyle IoT hub'ınızı bağlayan ve MQTT protokolünü kullanarak düzenli aralıklarla telemetri iletisi gönderen **SimulatedDevice.py**.

> [!NOTE]
> [IoT Hub SDK'ları][lnk-hub-sdks] makalesi, hem cihazlarınızda hem de çözüm arka ucunuzda çalıştırılacak uygulamalar oluşturmak için kullanabileceğiniz Azure IoT SDK’ları hakkında bilgi içerir.
> 
> 

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* [Python 2.x veya 3.x][lnk-python-download]. Kurulumunuzun gereksinimine uygun olarak 32 bit veya 64 bit yüklemeyi kullanmaya dikkat edin. Yükleme sırasında istendiğinde, platforma özgü ortam değişkeninize Python’u eklediğinizden emin olun. Python 2.x kullanıyorsanız, [Python paket yönetim sistemi *pip*’yi yüklemeniz veya yükseltmeniz][lnk-install-pip] gerekebilir.
* Windows işletim sistemi kullanıyorsanız, Python’dan yerel DLL’lerin kullanımına olanak tanımak için [Visual C++ yeniden dağıtılabilir paketi][lnk-visual-c-redist].
* [Node.js 4.0 veya üstü][lnk-node-download]. Kurulumunuzun gereksinimine uygun olarak 32 bit veya 64 bit yüklemeyi kullanmaya dikkat edin. Bu, [IoT Hub Gezgini aracını][lnk-iot-hub-explorer] yüklemek için gereklidir.
* Etkin bir Azure hesabı. Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.

> [!NOTE]
> `azure-iothub-service-client` ve `azure-iothub-device-client` için *PIP* paketleri şu anda yalnızca Windows İşletim Sistemi için mevcuttur. Linux/macOS için lütfen [Python için geliştirme ortamınızı hazırlayın][lnk-python-devbox] makalesindeki Linux ve macOS ile ilgili bölümlere bakın.
> 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

IoT Hub’ınızı oluşturdunuz. Bu öğreticinin kalan bölümünde IoT Hub konak adını ve IoT Hub bağlantı dizesini kullanın.

> [!NOTE]
> Ayrıca, komut satırında Python veya Node.js tabanlı Azure CLI’yi kullanarak kolayca IoT hub’ınızı oluşturabilirsiniz. [Azure CLI 2.0 kullanarak IoT hub’ı oluşturma][lnk-azure-cli-hub] makalesinde bunu yapmanın hızlı adımları gösterilir. 
> 

## <a name="create-a-device-identity"></a>Cihaz kimliği oluşturma
Bu bölümde, IoT hub'ınızdaki kimlik kayıt defterinde cihaz kimliği oluşturan bir Python konsol uygulaması oluşturma adımları listelenir. Yalnızca kimlik kayıt defterinde girişi olan cihazlar IoT Hub'ına bağlanabilir. Daha fazla bilgi için [IoT Hub Geliştirici Kılavuzu][lnk-devguide-identity]'nun **Kimlik Kayıt Defteri** bölümüne bakın. Bu konsol uygulamasını çalıştırdığınızda, cihazınızın IoT Hub'a cihaz-bulut iletileri gönderdiğinde kendisini tanımlamak için kullanabileceği benzersiz bir cihaz kimliği ve anahtarı oluşturulur.

1. Komut istemini açın ve **Python için Azure IoT Hub Hizmeti SDK’sını** aşağıda gösterildiği gibi yükleyin. SDK’yı yükledikten sonra komut istemini kapatın.

    ```
    pip install azure-iothub-service-client
    ```

2. **CreateDeviceIdentity.py** adlı bir Python dosyası oluşturun. Bu dosyayı, [kendi seçiminize bağlı olarak Python düzenleyicisinde/IDE’de][lnk-python-ide-list], (örneğin, varsayılan [IDLE][lnk-idle]) açın.

3. Hizmet SDK’sından gerekli modülleri içeri aktarmak için aşağıdaki kodu ekleyin:

    ```python
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod
    from iothub_service_client import IoTHubDeviceStatus, IoTHubError
    ```
2. Aşağıdaki kodu ekleyerek `[IoTHub Connection String]` yer tutucusunu önceki bölümde oluşturduğunuz IoT hub'ının bağlantı dizesiyle değiştirin. `DEVICE_ID` olarak herhangi bir ad kullanabilirsiniz.
   
    ```python
    CONNECTION_STRING = "[IoTHub Connection String]"
    DEVICE_ID = "MyFirstPythonDevice"
    ```
3. Cihaz bilgilerinden bir bölümünü yazdırmak için aşağıdaki işlevi ekleyin.

    ```python
    def print_device_info(title, iothub_device):
        print ( title + ":" )
        print ( "iothubDevice.deviceId                    = {0}".format(iothub_device.deviceId) )
        print ( "iothubDevice.primaryKey                  = {0}".format(iothub_device.primaryKey) )
        print ( "iothubDevice.secondaryKey                = {0}".format(iothub_device.secondaryKey) )
        print ( "iothubDevice.connectionState             = {0}".format(iothub_device.connectionState) )
        print ( "iothubDevice.status                      = {0}".format(iothub_device.status) )
        print ( "iothubDevice.lastActivityTime            = {0}".format(iothub_device.lastActivityTime) )
        print ( "iothubDevice.cloudToDeviceMessageCount   = {0}".format(iothub_device.cloudToDeviceMessageCount) )
        print ( "iothubDevice.isManaged                   = {0}".format(iothub_device.isManaged) )
        print ( "iothubDevice.authMethod                  = {0}".format(iothub_device.authMethod) )
        print ( "" )
    ```
3. Kayıt Defteri Yöneticisi’ni kullanarak cihaz kimliği oluşturmak için aşağıdaki işlevi ekleyin. 

    ```python
    def iothub_createdevice():
        try:
            iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)
            auth_method = IoTHubRegistryManagerAuthMethod.SHARED_PRIVATE_KEY
            new_device = iothub_registry_manager.create_device(DEVICE_ID, "", "", auth_method)
            print_device_info("CreateDevice", new_device)

        except IoTHubError as iothub_error:
            print ( "Unexpected error {0}".format(iothub_error) )
            return
        except KeyboardInterrupt:
            print ( "iothub_createdevice stopped" )
    ```
4. Son olarak, aşağıda gösterildiği gibi ana işlevi ekleyin ve dosyayı kaydedin.

    ```python
    if __name__ == '__main__':
        print ( "" )
        print ( "Python {0}".format(sys.version) )
        print ( "Creating device using the Azure IoT Hub Service SDK for Python" )
        print ( "" )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_createdevice()
    ```
5. Komut isteminde, aşağıda gösterildiği gibi **CreateDeviceIdentity.py** komutunu çalıştırın:

    ```python
    python CreateDeviceIdentity.py
    ```
6. Oluşturulmakta olan sanal cihazı görmeniz gerekir. Cihazın **deviceId** ve **primaryKey** değerlerini not alın. İleride IoT Hub'a bir cihaz olarak bağlanan bir uygulama oluşturduğunuzda bu değerlere ihtiyacınız olur.

    ![Cihaz başarısı oluşturma][1]

> [!NOTE]
> IoT Hub kimlik kayıt defteri, yalnızca IoT hub'ına güvenli erişim sağlamak amacıyla cihaz kimliklerini depolar. Güvenlik kimlik bilgileri olarak kullanılmak üzere cihaz kimliklerini ve anahtarlarını ve tek bir cihaza erişimi devre dışı bırakmak için kullanabileceğiniz etkin/devre dışı bayrağını depolar. Uygulamanızın cihaza özgü diğer meta verileri depolaması gerekiyorsa uygulamaya özgü bir depo kullanması gerekir. Daha fazla bilgi için bkz. [IoT Hub geliştirici kılavuzu][lnk-devguide-identity].
> 
> 


## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma
Bu bölümde, IoT hub'ınızda cihazdan buluta iletiler gönderen ve cihaz benzetimi yapan bir Python konsol uygulaması oluşturma adımları listelenir.

1. Yeni bir komut istemi açın ve Python için Azure IoT Hub Cihazı SDK’sını aşağıda gösterildiği gibi yükleyin. Yükleme bittikten sonra komut istemini kapatın.

    ```
    pip install azure-iothub-device-client
    ```
2. **SimulatedDevice.py** adlı bir dosya oluşturun. Bu dosyayı, kendi seçiminize bağlı olarak Python düzenleyicisinde/IDE’de açın (örneğin, IDLE).

3. Cihaz SDK’sından gerekli modülleri içeri aktarmak için aşağıdaki kodu ekleyin.

    ```python
    import random
    import time
    import sys
    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult
    from iothub_client import IoTHubMessage, IoTHubMessageDispositionResult, IoTHubError, DeviceMethodReturnValue
    ```
4. Aşağıdaki kodu ekleyin `[IoTHub Device Connection String]` yer tutucusunu cihazınızın bağlantı dizesiyle değiştirin. Cihaz bağlantı dizesi çoğunlukla `HostName=<hostName>;DeviceId=<deviceId>;SharedAccessKey=<primaryKey>` biçimindedir. `<deviceId>` ve `<primaryKey>` değerlerini sırasıyla önceki bölümde oluşturduğunuz cihazın **deviceId** ve **primaryKey** değerleriyle değiştirin. `<hostName>` değerini IoT hub'ınızın konak adıyla (çoğunlukla `<IoT hub name>.azure-devices.net` gibi) değiştirin.

    ```python
    # String containing Hostname, Device Id & Device Key in the format
    CONNECTION_STRING = "[IoTHub Device Connection String]"
    # choose HTTP, AMQP or MQTT as transport protocol
    PROTOCOL = IoTHubTransportProvider.MQTT
    MESSAGE_TIMEOUT = 10000
    AVG_WIND_SPEED = 10.0
    SEND_CALLBACKS = 0
    MSG_TXT = "{\"deviceId\": \"MyFirstPythonDevice\",\"windSpeed\": %.2f}"    
    ```
5. Gönderme onayı geri çevirmeyi tanımlamak için aşağıdaki kodu ekleyin. 

    ```python
    def send_confirmation_callback(message, result, user_context):
        global SEND_CALLBACKS
        print ( "Confirmation[%d] received for message with result = %s" % (user_context, result) )
        map_properties = message.properties()
        print ( "    message_id: %s" % message.message_id )
        print ( "    correlation_id: %s" % message.correlation_id )
        key_value_pair = map_properties.get_internals()
        print ( "    Properties: %s" % key_value_pair )
        SEND_CALLBACKS += 1
        print ( "    Total calls confirmed: %d" % SEND_CALLBACKS )
    ```
6. Cihaz istemcisini başlatmak için aşağıdaki kodu ekleyin.

    ```python
    def iothub_client_init():
        # prepare iothub client
        client = IoTHubClient(CONNECTION_STRING, PROTOCOL)
        # set the time until a message times out
        client.set_option("messageTimeout", MESSAGE_TIMEOUT)
        client.set_option("logtrace", 0)
        return client
    ```
7. Sanal cihazınızdan IoT hub’ınıza bir ileti biçimlendirmek ve göndermek için aşağıdaki işlevi ekleyin.

    ```python
    def iothub_client_telemetry_sample_run():

        try:
            client = iothub_client_init()
            print ( "IoT Hub device sending periodic messages, press Ctrl-C to exit" )
            message_counter = 0

            while True:
                msg_txt_formatted = MSG_TXT % (AVG_WIND_SPEED + (random.random() * 4 + 2))
                # messages can be encoded as string or bytearray
                if (message_counter & 1) == 1:
                    message = IoTHubMessage(bytearray(msg_txt_formatted, 'utf8'))
                else:
                    message = IoTHubMessage(msg_txt_formatted)
                # optional: assign ids
                message.message_id = "message_%d" % message_counter
                message.correlation_id = "correlation_%d" % message_counter
                # optional: assign properties
                prop_map = message.properties()
                prop_text = "PropMsg_%d" % message_counter
                prop_map.add("Property", prop_text)

                client.send_event_async(message, send_confirmation_callback, message_counter)
                print ( "IoTHubClient.send_event_async accepted message [%d] for transmission to IoT Hub." % message_counter )

                status = client.get_send_status()
                print ( "Send status: %s" % status )
                time.sleep(30)

                status = client.get_send_status()
                print ( "Send status: %s" % status )

                message_counter += 1

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )
    ```
8. Son olarak, ana işlevi ekleyin. 

    ```python
    if __name__ == '__main__':
        print ( "Simulating a device using the Azure IoT Hub Device SDK for Python" )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_client_telemetry_sample_run()
    ```
9. **SimulatedDevice.py** dosyasını kaydedin ve kapatın. Şimdi bu uygulamayı çalıştırmaya hazırsınız.

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. [Geçici Hata İşleme][lnk-transient-faults] adlı MSDN makalesinde önerildiği üzere, üretim kodunda yeniden deneme ilkelerini (üstel geri alma gibi) uygulamanız gerekir.
> 
> 

## <a name="receive-messages-from-your-simulated-device"></a>Sanal cihazınızdan ileti alma
Cihazınızdan telemetri iletilerini almak için, cihazdan buluta giden iletileri okuyan IoT Hub tarafından ortaya konan [Event Hubs][lnk-event-hubs-overview] ile uyumlu bir uç nokta kullanmanız gerekir. Event Hubs’tan IoT hub’ınızın Event Hub uyumlu uç noktasına gelen iletilerin nasıl işleneceği hakkında bilgi edinmek için, [Event Hubs ile Çalışmaya Başlama][lnk-eventhubs-tutorial] öğreticisini okuyun. Event Hubs henüz Python’da telemetriyi desteklememektedir, dolayısıyla IoT Hub’dan cihazdan buluta gelen iletileri okumak için [Node.js](iot-hub-node-node-getstarted.md#D2C_node) veya [.NET](iot-hub-csharp-csharp-getstarted.md#D2C_csharp) Event Hubs tabanlı bir konsol uygulaması oluşturabilirsiniz. Bu öğretici, bu cihaz iletilerini okumak için [IoT Hub Gezgini aracını][lnk-iot-hub-explorer] nasıl kullanabileceğinizi gösterir.

1. Komut istemini açın ve IoT Hub Gezgini’ni yükleyin. 

    ```
    npm install -g iothub-explorer
    ```

2. Cihazdan buluta gelen iletileri cihazınızdan izlemeye başlamak için, komut isteminde aşağıdaki komutu çalıştırın. Yer tutucuda `--login` öğesinden sonra IoT hub’ınızın bağlantı dizesini kullanın.

    ```
    iothub-explorer monitor-events MyFirstPythonDevice --login "[IoTHub connection string]"
    ```

3. Yeni bir komut istemi açın ve **SimulatedDevice.py** dosyasını içeren dizine gidin.

4. IoT hub’ınıza düzenli aralıklarla telemetri verileri gönderen **SimulatedDevice.py** dosyasını çalıştırın. 
   
    ```
    python SimulatedDevice.py
    ```
5. Önceki bölümde IoT Hub Gezgini’ni çalıştırarak komut isteminde cihaz iletilerini gözlemleyin. 

    ![Python cihazdan buluta iletiler][2]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure portalında yeni bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Bu cihaz kimliğini, sanal cihaz uygulamasının, IoT hub'ına cihazdan buluta iletileri göndermesini sağlamak için kullandınız. IoT Hub Gezgini aracının yardımıyla IoT hub’ı tarafından alınan iletileri gözlemlediniz. 

Azure IoT Hub için Python SDK’sının kullanımını derinlemesine incelemek için [bu Git Hub deposunu][lnk-python-github] ziyaret edin. Azure IoT Hub Hizmeti SDK’sının ileti özelliklerini gözden geçirmek için, [iothub_messaging_sample.py][lnk-messaging-sample] dosyasını indirebilir ve çalıştırabilirsiniz. Python için Azure IoT Hub Cihazı SDK’sını kullanan cihaz tarafı benzetimi için, [iothub_client_sample.py][lnk-client-sample] dosyasını indirebilir ve çalıştırabilirsiniz.

IoT Hub’ı kullanmaya başlamak ve diğer IoT senaryolarını keşfetmek için bkz:

* [Cihazınızı bağlama][lnk-connect-device]
* [Cihaz yönetimini kullanmaya başlama][lnk-device-management]
* [Azure IoT Edge’i kullanmaya başlama][lnk-iot-edge]

IoT çözümünüzün nasıl genişletileceğini ve cihazdan buluta iletilerin doğru ölçekte nasıl işleneceğini öğrenmek için [Cihazdan buluta iletileri işleme][lnk-process-d2c-tutorial] öğreticisine bakın.
[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[1]: ./media/iot-hub-python-getstarted/createdevice.png
[2]: ./media/iot-hub-python-getstarted/sendd2cmessage.png

<!-- Links -->
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-node-download]: https://nodejs.org/en/download/
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[lnk-azure-cli-hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-create-using-cli
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-idle]: https://docs.python.org/3/library/idle.html
[lnk-python-ide-list]: https://wiki.python.org/moin/IntegratedDevelopmentEnvironments
[lnk-iot-hub-explorer]: https://github.com/Azure/iothub-explorer
[lnk-python-github]: https://github.com/Azure/azure-iot-sdk-python
[lnk-messaging-sample]: https://github.com/Azure/azure-iot-sdk-python/blob/master/service/samples/iothub_messaging_sample.py
[lnk-client-sample]: https://github.com/Azure/azure-iot-sdk-python/blob/master/device/samples/iothub_client_sample.py

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md
[lnk-python-devbox]: https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md

[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

