---
title: "Azure IOT Hub cihaz çiftlerini (düğüm) ile çalışmaya başlama | Microsoft Docs"
description: "Azure IOT Hub cihaz çiftlerini etiket ekleyebilir ve IOT Hub sorgusuyla kullanmak için nasıl kullanılacağını. Sanal cihaz uygulamasının ve etiketleri ekler ve IOT hub'ı sorgu çalışan bir hizmet uygulaması uygulamak için Node.js için Azure IOT SDK'ları kullanın."
services: iot-hub
documentationcenter: node
author: fsautomata
manager: timlt
editor: 
ms.assetid: 314c88e4-cce1-441c-b75a-d2e08e39ae7d
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: elioda
ms.openlocfilehash: df49f054b5eb26c3d68f088bc05f5209cf2ebccf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-device-twins-node"></a>Cihaz çiftlerini (düğüm) ile çalışmaya başlama
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Bu öğreticinin sonunda iki Node.js konsol uygulamaları olacaktır:

* **AddTagsAndQuery.js**, etiketleri ekler ve cihaz çiftlerini sorgular bir Node.js arka uç uygulaması.
* **TwinSimulatedDevice.js**, bir cihaza benzetim yapan daha önce oluşturulan cihaz kimliğiyle IOT hub'ınıza bağlanır ve kendi bağlantı koşulu raporları bir Node.js uygulaması.

> [!NOTE]
> Makaleyi [Azure IOT SDK'ları] [ lnk-hub-sdks] hem cihaz hem de arka uç uygulamalar oluşturmak için kullanabileceğiniz Azure IOT SDK'ları hakkında bilgi sağlar.
> 
> 

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Node.js 4.0.x sürümü veya sonraki bir sürüm.
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-the-service-app"></a>Hizmet Uygulaması Oluştur
Bu bölümde, ilişkili cihaz çifti konumu meta veri ekleyen bir Node.js konsol uygulaması oluşturma **myDeviceId**. Ardından, IOT hub'ı ABD, ardından bir cep telefonu bağlantı raporlama olanları içinde bulunan aygıtları seçerek depolanan cihaz çiftlerini sorgular.

1. Adlı yeni bir boş klasör oluşturun **addtagsandqueryapp**. İçinde **addtagsandqueryapp** klasörü, komut isteminde aşağıdaki komutu kullanarak yeni bir package.json dosyası oluşturun. Tüm varsayılanları kabul edin:
   
    ```
    npm init
    ```
2. Komut isteminizde **addtagsandqueryapp** klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure-iothub** paketi:
   
    ```
    npm install azure-iothub --save
    ```
3. Bir metin düzenleyicisi kullanarak yeni bir oluşturma **AddTagsAndQuery.js** dosyasını **addtagsandqueryapp** klasör.
4. Aşağıdaki kodu ekleyin **AddTagsAndQuery.js** dosya ve yerine **{IOT hub bağlantı dizesine}** yer tutucu hub'ınızı oluşturduğunuzda kopyaladığınız IOT Hub bağlantı dizesine sahip:
   
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
   
    **Kayıt defteri** nesne cihaz çiftlerini hizmet ile etkileşim kurmak için gereken tüm yöntemleri gösterir. Önceki kod ilk başlatır **kayıt defteri** nesnesi, ardından cihaz çiftinin alır **myDeviceId**ve son olarak, etiketleri istenen konumu bilgilerle güncelleştirir.
   
    Etiketler güncelleştirdikten sonra çağırır **queryTwins** işlevi.
5. Sonuna aşağıdaki kodu ekleyin **AddTagsAndQuery.js** uygulamak için **queryTwins** işlevi:
   
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
   
    Önceki kod iki sorguları yürüten: yalnızca cihaz çiftlerini bulunan aygıtların ilk seçer **Redmond43** tesis ve ikinci da cep telefonu şebekesi bağlı aygıtlar seçmek için sorgu iyileştirir.
   
    Önceki kod oluştururken, **sorgu** nesne, döndürülen belgelerin en fazla sayısını belirtir. **Sorgu** nesnesini içeren bir **hasMoreResults** çağırmak için kullanabileceğiniz boolean özelliği **nextAsTwin** birden çok kez tüm sonuçları almak için yöntemleri. Bir yöntem olarak adlandırılan **sonraki** değil cihaz çiftlerini, örneğin, sonuçları için toplama sorguların sonuçlarını kullanılabilir.
6. Uygulama ile çalıştırın:
   
        node AddTagsAndQuery.js
   
    Bulunan tüm cihazlar için sorgu soran bir cihazda sonuçlarında görmelisiniz **Redmond43** ve sonuçları bir cep telefonu şebekesi kullanan cihazlar için sınırlar sorgu için yok.
   
    ![][1]

Sonraki bölümde, önceki bölümde sorgusunun sonucu değiştirir ve bağlantı bilgilerini raporlar bir cihaz uygulaması oluşturursunuz.

## <a name="create-the-device-app"></a>Cihaz uygulaması oluşturma
Bu bölümde, hub'ınıza bağlanan bir Node.js konsol uygulaması oluşturma **myDeviceId**ve ardından, cihaz çifti bir cep telefonu şebekesi kullanarak bağlı bilgileri içerecek şekilde özellikleri bildirilen güncelleştirmeler.


1. Adlı yeni bir boş klasör oluşturun **reportconnectivity**. İçinde **reportconnectivity** klasörü, komut isteminde aşağıdaki komutu kullanarak yeni bir package.json dosyası oluşturun. Tüm varsayılanları kabul edin:
   
    ```
    npm init
    ```
2. Komut isteminizde **reportconnectivity** klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure IOT cihaz**, ve **azure-IOT-cihaz-mqtt** paketi:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Bir metin düzenleyicisi kullanarak yeni bir oluşturma **ReportConnectivity.js** dosyasını **reportconnectivity** klasör.
4. Aşağıdaki kodu ekleyin **ReportConnectivity.js** dosya ve yerine **{cihaz bağlantı dizesi}** yer tutucu oluşturduğunuzdakopyaladığınızcihazbağlantıdizesiyle**myDeviceId** cihaz kimliği:
   
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
   
    **İstemci** nesne ihtiyaç duyduğunuz etkileşim kurmak için cihaz çiftlerini aygıttan ile tüm yöntemleri gösterir. Bunu başlatır sonra önceki kod **istemci** nesnesi, cihaz çiftinin alır **myDeviceId** ve kendi bildirilen özelliği ile bağlantı bilgilerini güncelleştirir.
5. Cihaz uygulama çalıştırma
   
        node ReportConnectivity.js
   
    Şu iletiyi görürsünüz `twin state reported`.
6. Aygıt bağlantısı bilgilerini bildirdi, her iki sorgularda görüntülenmelidir. Geri gidin **addtagsandqueryapp** klasörü ve sorguları yeniden çalıştırın:
   
        node AddTagsAndQuery.js
   
    Bu süre **myDeviceId** hem sorgu sonuçlarında görüntülenmesi gerekir.
   
    ![][3]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure portalında yeni bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Cihaz meta verilerini bir arka uç uygulamadan etiketler eklendi ve sanal cihaz uygulaması rapor cihaz bağlantı bilgilerini cihaz çiftine yazıldı. Ayrıca SQL benzeri IOT hub'ı sorgu dili kullanarak bu bilgileri sorgulamak öğrendiniz.

Bilgi edinmek için aşağıdaki kaynakları kullanın nasıl yapılır:

* aygıtlarla telemetri gönderen [IOT Hub ile çalışmaya başlama] [ lnk-iothub-getstarted] öğretici
* cihaz çifti'nın istenen özelliklere sahip kullanarak cihazları yapılandırma [kullanmak istediğiniz cihazları yapılandırmak için Özellikler] [ lnk-twin-how-to-configure] öğretici
* ile etkileşimli olarak (örneğin, kullanıcı tarafından denetlenen bir uygulamadan fan etkinleştirdikten), cihazları denetleme [doğrudan yöntemleri kullanın] [ lnk-methods-tutorial] Öğreticisi.

<!-- images -->
[1]: media/iot-hub-node-node-twin-getstarted/service1.png
[3]: media/iot-hub-node-node-twin-getstarted/service2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

[lnk-twin-how-to-configure]: iot-hub-node-node-twin-how-to-configure.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
