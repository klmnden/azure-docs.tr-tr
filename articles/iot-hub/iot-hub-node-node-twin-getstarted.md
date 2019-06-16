---
title: Azure IOT Hub cihaz ikizlerini (Node) kullanmaya başlama | Microsoft Docs
description: Azure IOT Hub cihaz ikizlerini etiketler ekleyin ve ardından IOT Hub sorgu kullanma Node.js için Azure IOT SDK'ları, sanal cihaz uygulaması ve etiketleri ekler ve IOT Hub sorgu çalışan bir hizmet uygulaması'nı uygulamak için kullanın.
author: fsautomata
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 08/25/2017
ms.author: elioda
ms.openlocfilehash: 20b804f3d15543d0cf415d00dc81a6f55a348260
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65597426"
---
# <a name="get-started-with-device-twins-node"></a>Cihaz ikizlerini (Node) kullanmaya başlama

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Bu öğreticinin sonunda iki Node.js konsol uygulamanız olacaktır:

* **AddTagsAndQuery.js**, etiketleri ekler ve cihaz ikizlerini sorgular bir Node.js arka uç uygulaması.

* **TwinSimulatedDevice.js**, bir cihaza benzetim yapan daha önce oluşturulan cihaz kimliğiyle IOT hub'ınıza bağlanır ve kendi bağlantı koşulu raporları bir Node.js uygulaması.

> [!NOTE]
> Makaleyi [Azure IOT SDK'ları](iot-hub-devguide-sdks.md) hem cihaz hem de arka uç uygulamaları oluşturmak için kullanabileceğiniz Azure IOT SDK'ları hakkında bilgi sağlar.
>

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Node.js sürümü 10.0.x veya üzeri.

* Etkin bir Azure hesabı. (Hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.)

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>IOT hub için bağlantı dizesi alma

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-the-service-app"></a>Hizmet uygulaması oluşturma

Bu bölümde, konum meta verileri ile ilişkili cihaz ikizi ekleyen bir Node.js konsol uygulaması oluşturma **myDeviceId**. Ardından, IOT hub'ı ABD, ardından bir hücresel bağlantı raporlama olanları içinde bulunan aygıtları seçme içinde depolanan cihaz ikizlerini sorgular.

1. Adlı yeni bir boş klasör oluşturun **addtagsandqueryapp**. İçinde **addtagsandqueryapp** klasöründe komut isteminizde aşağıdaki komutu kullanarak yeni bir package.json dosyası oluşturun. Tüm varsayılanları kabul edin:

    ```
    npm init
    ```

2. Komut isteminizde **addtagsandqueryapp** klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure-iothub** paket:
   
    ```
    npm install azure-iothub --save
    ```

3. Bir metin düzenleyicisi kullanarak yeni bir oluşturma **AddTagsAndQuery.js** dosyası **addtagsandqueryapp** klasör.

4. Aşağıdaki kodu ekleyin **AddTagsAndQuery.js** dosya ve substitute **{IOT hub bağlantı dizesine}** IOT Hub bağlantı dizesiyle hub'ınızı oluşturduğunuz sırada kopyaladığınız yer tutucu:

   ``` javascript
        'use strict';
        var iothub = require('azure-iothub');
        var connectionString = '{iot hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
   
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var patch = {
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                      }
                    }
                };
   
                twin.update(patch, function(err) {
                  if (err) {
                    console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                  } else {
                    console.log(twin.deviceId + ' twin updated successfully');
                    queryTwins();
                  }
                });
            }
        });
   ```

    **Kayıt defteri** nesne hizmetinden cihaz ikizlerini ile etkileşim kurmak için gereken tüm yöntemleri sunar. Önceki kodun ilk başlatır **kayıt defteri** nesnesi ve ardından cihaz ikizi alır **myDeviceId**ve son olarak, etiketler ile istenen konuma bilgileri güncelleştirir.

    Etiketleri güncelleştirdikten sonra onu çağıran **queryTwins** işlevi.

5. Sonuna aşağıdaki kodu ekleyin **AddTagsAndQuery.js** uygulamak için **queryTwins** işlevi:

   ```javascript
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
   
            query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43 using cellular network: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
        };
   ```

    Önceki kod iki sorguları yürüten: yalnızca cihaz ikizlerini bulunan cihazların ilk seçer **Redmond43** tesis ve ikincisi de hücresel ağ üzerinden bağlı cihazları seçmek için sorguyu iyileştirir.

    Önceki kodda oluştururken **sorgu** nesne, döndürülen belgeler sayısı üst sınırını belirtir. **Sorgu** nesne içeren bir **hasMoreResults** çağırmak için kullanabileceğiniz bir boolean özelliği **nextAsTwin** birden çok kez tüm sonuçları almak için yöntemleri. Bir yöntemi çağıran **sonraki** değil cihaz çiftleri, örneğin, sonuçları için toplama sorguların sonuçlarını kullanılabilir.

6. Uygulamayı çalıştırın:

    ```
        node AddTagsAndQuery.js
    ```

   Bulunan tüm cihazlar için bir cihaz sormaya sorgu sonuçları görmeniz gerekir **Redmond43** ve sonuçları bir hücresel ağ kullanan cihazlar için sınırlar sorgu için yok.
   
    ![Bir cihaz sorgu sonuçlarında bakın](media/iot-hub-node-node-twin-getstarted/service1.png)

Sonraki bölümde, bağlantı bilgilerini raporlar ve önceki bölümde sorgusunun sonucunu değiştiren bir cihaz uygulaması oluşturun.

## <a name="create-the-device-app"></a>Cihaz uygulaması oluşturma

Bu bölümde oluşturduğunuz hub'ınıza bağlanan bir Node.js konsol uygulaması **myDeviceId**ve ardından, cihaz çiftinin bildirilen özelliklerini hücresel ağ üzerinden bağlı bilgiler içermesini kimler güncelleştirmeler.

1. Adlı yeni bir boş klasör oluşturun **reportconnectivity**. İçinde **reportconnectivity** klasöründe komut isteminizde aşağıdaki komutu kullanarak yeni bir package.json dosyası oluşturun. Tüm varsayılanları kabul edin:
   
    ```
    npm init
    ```

2. Komut isteminizde **reportconnectivity** klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure IOT cihaz**, ve **azure-iot-device-mqtt** paket:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

3. Bir metin düzenleyicisi kullanarak yeni bir oluşturma **ReportConnectivity.js** dosyası **reportconnectivity** klasör.

4. Aşağıdaki kodu ekleyin **ReportConnectivity.js** dosya ve substitute **{cihaz bağlantı dizesini}** yer tutucu oluşturduğunuzsıradakopyaladığınızcihazbağlantıdizesiyle**myDeviceId** cihaz kimliği:

    ```
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
   
            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };
   
                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });
    ```

    **İstemci** nesne ihtiyaç duyduğunuz etkileşim kurmak için cihaz ikizlerini CİHAZDAN ile tüm yöntemleri sunar. Sonra onu başlatır önceki kodu **istemci** nesne, cihaz çiftinin alır **myDeviceId** ve kendi bildirilen özellik ile bağlantı bilgilerini güncelleştirir.

5. Cihaz uygulamasını çalıştırın

    ```   
        node ReportConnectivity.js
    ```

    Şu iletiyi görürsünüz `twin state reported`.

6. Cihaz bağlantı bilgilerini bildirilen, her iki sorgularda görüntülenmesi gerekir. Geri gidin **addtagsandqueryapp** klasörü ve sorguları yeniden çalıştırın:

    ```   
        node AddTagsAndQuery.js
    ```

    Bu süre **myDeviceId** iki sorgu sonuçlarında görüntülenmesi gerekir.

    ![Her iki sorgu sonuçlarında myDeviceId Göster](media/iot-hub-node-node-twin-getstarted/service2.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portalında yeni bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Cihaz meta verilerini etiketler bir arka uç uygulamasından eklenen ve bir sanal cihaz uygulaması rapor cihaz bağlantı bilgileri cihaz ikizinde söyleyebiliriz. Ayrıca bu bilgiler IOT Hub'ı SQL benzeri sorgu dili kullanarak sorgulamayı öğrendiniz.

Bilgi edinmek için aşağıdaki kaynakları kullanın. nasıl yapılır:

* İle cihazlardan telemetri gönderme [IOT Hub ile çalışmaya başlama](quickstart-send-telemetry-node.md) öğretici

* ile cihaz ikizinin istenen özellikleri kullanarak cihazları yapılandırma [kullanmak istediğiniz cihazları yapılandırmak için Özellikler](tutorial-device-twins.md) öğretici

* İle etkileşimli olarak (örneğin, bir kullanıcı tarafından denetlenen uygulamasından fan üzerinde kapatma), cihazları denetleme [doğrudan yöntemler kullanma](quickstart-control-device-node.md) öğretici.