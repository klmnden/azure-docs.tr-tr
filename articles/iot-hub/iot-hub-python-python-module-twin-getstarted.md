---
title: Azure IOT hub'ı modül kimlik ve modül ikizi ile (Python) kullanmaya başlayın | Microsoft Docs
description: Modül kimliği oluşturma ve Python için IOT SDK'ları kullanarak modül ikizi güncelleştirme hakkında bilgi edinin.
author: chrissie926
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 04/26/2018
ms.author: menchi
ms.openlocfilehash: f887fbd4f82e59c02d6a5b69d0d5b43b426a39bc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61441222"
---
# <a name="get-started-with-iot-hub-module-identity-and-module-twin-using-python-back-end-and-python-device"></a>Python arka ucu ile Python cihaz IOT hub'ı modül kimlik ve modül ikizi ile çalışmaya başlama

> [!NOTE]
> [Modül kimlikleri ve modül ikizleri](iot-hub-devguide-module-twins.md), Azure IoT Hub cihaz kimliğine ve cihaz ikizine benzer, ancak daha hassas ayrıntı düzeyi sağlar. Azure IoT Hub cihaz kimliği ve cihaz ikizi, arka uç uygulamasının bir cihaz yapılandırmasına imkan tanıyıp cihazın koşullarına yönelik görünürlük sağlarken, modül kimliği ve modül ikizi de bir cihazın tek tek bileşenleri için bu özellikleri sağlar. İşletim sistemi tabanlı cihazlar veya üretici yazılımı cihazları gibi, birden fazla bileşen içeren ve bu özelliklere sahip cihazlarda her bir bileşen için yalıtılmış yapılandırma ve koşullara olanak sağlar.
>

Bu öğreticinin sonunda iki Python uygulamaları vardır:

* Cihaz ve modül istemcilerinizi bağlamak için bir cihaz kimliği, bir modül kimliği ve ilişkili güvenlik anahtarı oluşturan **CreateIdentities**.

* Güncelleştirilmiş modül ikizi tarafından raporlanan özelliklerini IoT Hub’ınıza gönderen **UpdateModuleTwinReportedProperties**.

> [!NOTE]
> Cihazlar ve çözüm arka ucunuz çalıştırılacak hem uygulamalar oluşturmak için kullanabileceğiniz Azure IOT SDK'ları hakkında daha fazla bilgi için bkz. [Azure IOT SDK'ları](iot-hub-devguide-sdks.md).
>

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. (Hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.)

* IOT hub'ı.

* Son yükleme [Python SDK'sı](https://github.com/Azure/azure-iot-sdk-python).

IoT hub'ınızı oluşturdunuz ve bu öğreticinin geri kalanını tamamlamak için gereken ana bilgisayar adı ve IoT Hub bağlantı dizesine sahipsiniz.

## <a name="create-a-device-identity-and-a-module-identity-in-iot-hub"></a>Bir cihaz kimliği ve bir modül kimliği, IOT hub'ı oluşturma

Bu bölümde, IOT hub'ınızdaki kimlik kayıt defterinde bir cihaz kimliği ve bir modül kimliği oluşturan bir Python uygulaması oluşturun. Kimlik kayıt defterinde girişi olmayan bir cihaz veya modül, IoT hub'ına bağlanamaz. Daha fazla bilgi için "Kimlik kayıt defteri" bölümüne bakın. [IOT Hub Geliştirici kılavuzunun](iot-hub-devguide-identity-registry.md). Bu konsol uygulamasını çalıştırdığınızda, hem cihaz hem de modül için benzersiz bir kimlik ve anahtar oluşturur. Cihazınız ve modülünüz, IoT Hub’ına cihazdan buluta iletileri gönderdiğinde kendisini tanımlamak için bu değerleri kullanır. Kimlikler büyük/küçük harfe duyarlıdır.

Python dosyanıza aşağıdaki kodu ekleyin:

```python
import sys
import iothub_service_client
from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod, IoTHubError

CONNECTION_STRING = "YourConnString"
DEVICE_ID = "myFirstDevice"
MODULE_ID = "myFirstModule"

try:
    # RegistryManager
    iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)

    # CreateDevice
    primary_key = ""
    secondary_key = ""
    auth_method = IoTHubRegistryManagerAuthMethod.SHARED_PRIVATE_KEY
    new_device = iothub_registry_manager.create_device(DEVICE_ID, primary_key, secondary_key, auth_method)
    print("new_device <" + DEVICE_ID + "> has primary key = " + new_device.primaryKey)

    # CreateModule
    new_module = iothub_registry_manager.create_module(DEVICE_ID, primary_key, secondary_key, MODULE_ID, auth_method)
    print("device/new_module <" + DEVICE_ID + "/" + MODULE_ID + "> has primary key = " + new_module.primaryKey)

except IoTHubError as iothub_error:
    print ( "Unexpected error {0}".format(iothub_error) )
except KeyboardInterrupt:
    print ( "IoTHubRegistryManager sample stopped" )
```

Bu uygulama Kimliğine sahip bir cihaz kimliği oluşturan **myFirstDevice** ve Kimliğe sahip bir modül kimliği **myFirstModule** cihaz altında **myFirstDevice**. (Bu modül kimliği, kimlik kayıt defterinde zaten varsa, kod yalnızca mevcut modül bilgilerini alır.) Bu durumda uygulama, bu kimliğin birincil anahtarını görüntüler. IoT hub'ınıza bağlanmak için sanal modül uygulamasında bu anahtarı kullanırsınız.

> [!NOTE]
> IoT Hub kimlik kayıt defteri yalnızca IoT hub'ına güvenli erişim sağlamak amacıyla cihaz ve modül kimliklerini depolar. Kimlik kayıt defteri, cihaz kimliklerini ve anahtarlarını güvenlik kimlik bilgileri olarak kullanmak için depolar. Kimlik kayıt defterinin her cihaz için depoladığı etkin/devre dışı bayrağını kullanarak, ilgili cihaza erişimi devre dışı bırakabilirsiniz. Uygulamanızın cihaza özgü diğer meta verileri depolaması gerekiyorsa uygulamaya özgü bir depo kullanması gerekir. Modül kimlikleri için etkin/devre dışı bayrağı yoktur. Daha fazla bilgi için [IOT Hub Geliştirici kılavuzunun](iot-hub-devguide-identity-registry.md).
>

## <a name="update-the-module-twin-using-python-device-sdk"></a>Python cihaz SDK'sını kullanarak modül ikizi güncelleştir

Bu bölümde, oluşturduğunuz bir Python uygulamasını sanal Cihazınızda modül ikizi güncelleştirmeleri bildirilen özellikler.

1. **Modülü bağlantı dizenizi alma** --oturum açmanız durumunda şimdi [Azure portalında](https://portal.azure.com/). IoT Hub’ınıza gidin ve IoT Cihazları’na tıklayın. Bul myFirstDevice, açık myFirstModule göreceksiniz başarıyla oluşturuldu. Modül bağlantı dizesini kopyalayın. Sonraki adımda gerekecektir.

   ![Azure portalı modül ayrıntısı](./media/iot-hub-python-python-module-twin-getstarted/module-detail.png)

2. **UpdateModuleTwinReportedProperties uygulaması oluşturma**

   Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

    ```python
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod, IoTHubDeviceTwin, IoTHubError

    CONNECTION_STRING = "FILL IN CONNECTION STRING"
    DEVICE_ID = "MyFirstDevice"
    MODULE_ID = "MyFirstModule"

    UPDATE_JSON = "{\"properties\":{\"desired\":{\"telemetryInterval\":122}}}"

    try:
        iothub_twin = IoTHubDeviceTwin(CONNECTION_STRING)

        twin_info = iothub_twin.get_twin(DEVICE_ID, MODULE_ID)
        print ( "" )
        print ( "Twin before update    :" )
        print ( "{0}".format(twin_info) )

        twin_info_updated = iothub_twin.update_twin(DEVICE_ID, MODULE_ID, UPDATE_JSON)
        print ( "" )
        print ( "Twin after update     :" )
        print ( "{0}".format(twin_info_updated) )

    except IoTHubError as iothub_error:
        print ( "Unexpected error {0}".format(iothub_error) )
    except KeyboardInterrupt:
        print ( "IoTHubRegistryManager sample stopped" )
    ```

Bu kod örneği, AMQP protokolüyle raporlanan özellikleri güncelleştirme ve modül ikizini alma işlemini nasıl yapacağınızı gösterir.

## <a name="get-updates-on-the-device-side"></a>Güncelleştirmeleri cihaz tarafında Al

İkiz güncelleştirmesi almak için kod bloğu ekleyebileceğiniz ek olarak Yukarıdaki kod, Cihazınızda ileti.

```python
import random
import time
import sys
import iothub_client
from iothub_client import IoTHubModuleClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult

PROTOCOL = IoTHubTransportProvider.AMQP
CONNECTION_STRING = ""

def module_twin_callback(update_state, payload, user_context):
    print ("")
    print ("Twin callback called with:")
    print ("updateStatus: %s" % update_state )
    print ("context: %s" % user_context )
    print ("payload: %s" % payload )

try:
    module_client = IoTHubModuleClient(CONNECTION_STRING, PROTOCOL)
    module_client.set_module_twin_callback(module_twin_callback, 1234)

    print ("Waiting for incoming twin messages.  Hit Control-C to exit.")
    while True:

        time.sleep(1000000)

except IoTHubError as iothub_error:
    print ( "Unexpected error {0}".format(iothub_error) )
except KeyboardInterrupt:
    print ( "module client sample stopped" )
```

## <a name="next-steps"></a>Sonraki adımlar

IoT Hub’ı kullanmaya başlamak ve diğer IoT senaryolarını keşfetmek için bkz:

* [Cihaz yönetimi ile çalışmaya başlama](iot-hub-node-node-device-management-get-started.md)

* [IOT Edge'i kullanmaya başlama](../iot-edge/tutorial-simulate-device-linux.md)