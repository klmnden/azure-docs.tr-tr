---
title: "Azure IOT Hub cihaz çifti özellikleri (düğüm) kullanın | Microsoft Docs"
description: "Azure IOT Hub cihaz çiftlerini cihazları yapılandırmak için nasıl kullanılacağını. Sanal cihaz uygulaması ve cihaz çifti kullanarak bir aygıt yapılandırmasını değiştirir bir hizmet uygulaması uygulamak için Node.js için Azure IOT SDK'ları kullanın."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: d0bcec50-26e6-40f0-8096-733b2f3071ec
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/07/2017
ms.author: elioda
ms.openlocfilehash: 6ff6f1c331d5a77e7ac0a47af6806f5d90fb0fdc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-desired-properties-to-configure-devices-node"></a>İstenen özelliklerde cihazları (düğüm) yapılandırmak için kullanın
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Bu öğreticinin sonunda iki Node.js konsol uygulamaları olacaktır:

* **SimulateDeviceConfiguration.js**, istenen yapılandırma güncelleştirmesi bekler ve benzetimli yapılandırmasını güncelleştirme işleminin durumunu raporlar bir sanal cihaz uygulaması.
* **SetDesiredConfigurationAndQuery.js**, istenen yapılandırma bir cihazda ayarlar ve yapılandırmayı güncelleştirme işlemini sorgular bir Node.js arka uç uygulaması.

> [!NOTE]
> Makaleyi [Azure IOT SDK'ları] [ lnk-hub-sdks] hem cihaz hem de arka uç uygulamalar oluşturmak için kullanabileceğiniz Azure IOT SDK'ları hakkında bilgi sağlar.
> 
> 

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Node.js 4.0.x sürümü veya sonraki bir sürüm.
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

İzlediyseniz [cihaz çiftlerini ile çalışmaya başlama] [ lnk-twin-tutorial] öğretici, zaten sahip olduğunuz bir IOT hub ve adlı bir cihaz kimliği **myDeviceId**; ve atlayabilirsiniz[Sanal cihaz uygulaması oluşturma] [ lnk-how-to-configure-createapp] bölümü.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-the-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma
Bu bölümde, hub'ınıza bağlanan bir Node.js konsol uygulaması oluşturma **myDeviceId**, istenen yapılandırma güncelleştirmesi bekler ve daha sonra güncelleştirmeleri benzetimli yapılandırmasını güncelleştirme işlemini raporlar.

1. Adlı yeni bir boş klasör oluşturun **simulatedeviceconfiguration**. İçinde **simulatedeviceconfiguration** klasörü, komut isteminde aşağıdaki komutu kullanarak yeni bir package.json dosyası oluşturun. Tüm varsayılanları kabul edin:
   
    ```
    npm init
    ```
2. Komut isteminizde **simulatedeviceconfiguration** klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure IOT cihaz**, ve **azure-IOT-cihaz-mqtt** paketi:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Bir metin düzenleyicisi kullanarak yeni bir oluşturma **SimulateDeviceConfiguration.js** dosyasını **simulatedeviceconfiguration** klasör.
4. Aşağıdaki kodu ekleyin **SimulateDeviceConfiguration.js** dosya ve yerine **{cihaz bağlantı dizesi}** yer tutucu oluştururken kopyaladığınız cihaz bağlantı dizesiyle **myDeviceId** cihaz kimliği:
   
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });
   
    **İstemci** nesne cihaz çiftlerini aygıttan ile etkileşim kurmak için gereken tüm yöntemleri gösterir. Bunu başlatır sonra önceki kod **istemci** nesnesi, cihaz çiftinin alır **myDeviceId**ve istenen özellikler güncelleştirme için bir işleyici ekler. İşleyici olmadığını configIds karşılaştırarak bir gerçek yapılandırma değişikliği isteği olup, ardından yapılandırma değişikliği başlatan bir yöntemi çağırır doğrular.
   
    Basitleştirmek amacıyla, sabit kodlanmış varsayılan bir önceki kod ilk yapılandırmasını kullandığını unutmayın. Gerçek bir uygulama büyük olasılıkla bu yapılandırma yerel depolama biriminden yüklenir.
   
   > [!IMPORTANT]
   > İstenen özellik değiştirme olayları her zaman bir kez aygıt bağlantıdaki gösterilen, herhangi bir işlem gerçekleştirmeden önce İstenen özelliklerde gerçek bir değişiklik olup olmadığını denetlemek emin olun.
   > 
   > 
5. Önce aşağıdaki yöntemleri ekleyin `client.open()` çağırma:
   
        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";
   
            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }
   
        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
   
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;
   
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };
   
    **İnitConfigChange** yöntemi yerel cihaz çifti nesnesindeki bildirilen özellikleri yapılandırma güncelleştirme isteği ile güncelleştirir ve durumunu ayarlar **bekleyen**, cihaz çifti sonra güncelleştirir hizmet. Cihaz çifti başarıyla güncelleştirdikten sonra yürütme işlemi sona erer uzun süre çalışan bir işlem benzetim **completeConfigChange**. Bu yöntem yerel cihaz çifti'nın bildirilen özellikleri durumu ayarını güncelleştirmeleri **başarı** ve kaldırma **pendingConfig** nesnesi. Ardından, cihaz çifti hizmette güncelleştirir.
   
    Bant genişliğini korumak için bildirilen özellikler, yalnızca değiştirilecek özelliklerini belirterek güncelleştirilir unutmayın (adlı **düzeltme eki** Yukarıdaki koddaki), tüm belgeyi değiştirme yerine.
   
   > [!NOTE]
   > Bu öğretici herhangi davranışı eşzamanlı yapılandırma güncelleştirmelerini benzetimini değil. Bazı yapılandırma güncelleştirme işlemleri hedef yapılandırma değişiklikleri güncelleştirme çalışan, diğerleri bunları kuyruğuna olabilir ve diğerleri ile bir hata koşulu reddedebilirsiniz sırada karşılamak mümkün olabilir. Belirli bir yapılandırma işlemi için istenen davranışı dikkate aldığınızdan emin olun ve yapılandırma değişikliğini başlatmadan önce uygun mantığı ekleyin.
   > 
   > 
6. Cihaz uygulaması çalıştırın:
   
        node SimulateDeviceConfiguration.js
   
    Şu iletiyi görürsünüz `retrieved device twin`. Uygulamayı çalıştıran tutun.

## <a name="create-the-service-app"></a>Hizmet Uygulaması Oluştur
Bu bölümde, güncelleştirmeleri bir Node.js konsol uygulaması oluşturacak *özelliklerini istenen* ilişkili cihaz çifti üzerinde **myDeviceId** ile yeni bir telemetri yapılandırma nesnesi. IOT hub'ı depolanan cihaz çiftlerini sorgular ve cihazın istenen ve bildirilen yapılandırmaları arasındaki farkı gösterir.

1. Adlı yeni bir boş klasör oluşturun **setdesiredandqueryapp**. İçinde **setdesiredandqueryapp** klasörü, komut isteminde aşağıdaki komutu kullanarak yeni bir package.json dosyası oluşturun. Tüm varsayılanları kabul edin:
   
    ```
    npm init
    ```
2. Komut isteminizde **setdesiredandqueryapp** klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure-iothub** paketi:
   
    ```
    npm install azure-iothub node-uuid --save
    ```
3. Bir metin düzenleyicisi kullanarak yeni bir oluşturma **SetDesiredAndQuery.js** dosyasını **setdesiredandqueryapp** klasör.
4. Aşağıdaki kodu ekleyin **SetDesiredAndQuery.js** dosya ve yerine **{IOT hub bağlantı dizesine}** yer tutucu hub'ınızı oluşturduğunuzda kopyaladığınız IOT Hub bağlantı dizesine sahip:
   
        'use strict';
        var iothub = require('azure-iothub');
        var uuid = require('node-uuid');
        var connectionString = '{iot hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
   
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var newConfigId = uuid.v4();
                var newFrequency = process.argv[2] || "5m";
                var patch = {
                    properties: {
                        desired: {
                            telemetryConfig: {
                                configId: newConfigId,
                                sendFrequency: newFrequency
                            }
                        }
                    }
                }
                twin.update(patch, function(err) {
                    if (err) {
                        console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                    } else {
                        console.log(twin.deviceId + ' twin updated successfully');
                    }
                });
                setInterval(queryTwins, 10000);
            }
        });

    **Kayıt defteri** nesne cihaz çiftlerini hizmet ile etkileşim kurmak için gereken tüm yöntemleri gösterir. Bunu başlatır sonra önceki kod **kayıt defteri** nesnesi, cihaz çiftinin alır **myDeviceId**ve yeni bir telemetri yapılandırma nesnesi ile istenen özelliklerini güncelleştirir. Bundan sonra çağırır **queryTwins** 10 saniye olay işlev.

    > [!IMPORTANT]
    > Bu uygulama yalnızca tanım amaçlıdır her 10 saniye IOT hub'ı sorgular. Sorguları birçok cihaz arasında kullanıcı dönük raporları oluşturmak ve değil değişikliklerini algılamak için kullanın. Çözümünüz cihaz olayların gerçek zamanlı bildirimler gerektiriyorsa, kullanın [twin bildirimleri][lnk-twin-notifications].
    > 
    >.

1. Aşağıdaki kod hemen önce eklemek `registry.getDeviceTwin()` uygulamak için çağırma **queryTwins** işlevi:
   
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log();
                    results.forEach(function(twin) {
                        var desiredConfig = twin.properties.desired.telemetryConfig;
                        var reportedConfig = twin.properties.reported.telemetryConfig;
                        console.log("Config report for: " + twin.deviceId);
                        console.log("Desired: ");
                        console.log(JSON.stringify(desiredConfig, null, 2));
                        console.log("Reported: ");
                        console.log(JSON.stringify(reportedConfig, null, 2));
                    });
                }
            });
        };
   
    Önceki kod sorguları cihaz çiftlerini IOT hub'ı depolanan ve istenen ve bildirilen telemetri yapılandırmaları yazdırır. Başvurmak [IOT hub'ı sorgu dili] [ lnk-query] tüm cihazlarınızda zengin raporlar üretmek hakkında bilgi edinmek için.
2. İle **SimulateDeviceConfiguration.js** , uygulama ile çalıştırma:
   
        node SetDesiredAndQuery.js 5m
   
    Bildirilen yapılandırma öğesinden değişikliği görmelisiniz **başarı** için **bekleyen** için **başarı** yeni etkin 24 saat yerine beş dakikalık sıklığı yeniden gönderin.
   
   > [!IMPORTANT]
   > Cihaz rapor işlemi ve sorgu sonucu arasında bir dakika kadar bir gecikme yoktur. Bu, çok yüksek ölçekli olarak çalışmak sorgu altyapısı olanak sağlamasıdır. Bir tek cihaz çifti kullanımı tutarlı görünümleri almak için **getDeviceTwin** yönteminde **kayıt defteri** sınıfı.
   > 
   > 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, istenen yapılandırma olarak ayarlamak *özelliklerini istenen* bir arka uç uygulamasından ve bu değişikliği algılar ve durumunu olarak raporlama çok adımlı güncelleştirme işleminin benzetimini yapmak için bir sanal cihaz uygulaması yazdı  *Özellikler bildirilen* cihaz çifti için.

Bilgi edinmek için aşağıdaki kaynakları kullanın nasıl yapılır:

* aygıtlarla telemetri gönderen [IOT Hub ile çalışmaya başlama] [ lnk-iothub-getstarted] öğretici
* zamanlama veya aygıtları bakın büyük kümeleri işlemleri [zamanlama ve yayın işleri] [ lnk-schedule-jobs] Öğreticisi.
* ile etkileşimli olarak (örneğin, kullanıcı tarafından denetlenen bir uygulamadan fan etkinleştirdikten), cihazları denetleme [doğrudan yöntemleri kullanın] [ lnk-methods-tutorial] Öğreticisi.

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app
