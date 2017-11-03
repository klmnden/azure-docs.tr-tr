---
title: "Azure IOT Hub cihaz çifti özellikleri (.NET/düğüm) kullanın | Microsoft Docs"
description: "Azure IOT Hub cihaz çiftlerini cihazları yapılandırmak için nasıl kullanılacağını. Sanal cihaz uygulaması ve Azure IOT hizmeti SDK'sını cihaz çifti kullanarak bir aygıt yapılandırmasını değiştirir bir hizmet uygulaması uygulamak .NET için uygulamak için Azure IOT cihaz SDK'sı Node.js için kullanın."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 3c627476-f982-43c9-bd17-e0698c5d236d
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: elioda
ms.openlocfilehash: 8f9626fc47e7d32cb104b960bea9b7de9efa6f3e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-desired-properties-to-configure-devices"></a>İstenen özelliklerde cihazları yapılandırmak için kullanın
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Bu öğreticinin sonunda iki konsol uygulamaları olacaktır:

* **SimulateDeviceConfiguration.js**, istenen yapılandırma güncelleştirmesi bekler ve benzetimli yapılandırmasını güncelleştirme işleminin durumunu raporlar bir sanal cihaz uygulaması.
* **SetDesiredConfigurationAndQuery**, istenen yapılandırma bir cihazda ayarlar ve yapılandırmayı güncelleştirme işlemini sorgular .NET arka uç uygulaması.

> [!NOTE]
> Makaleyi [Azure IOT SDK'ları] [ lnk-hub-sdks] hem cihaz hem de arka uç uygulamalar oluşturmak için kullanabileceğiniz Azure IOT SDK'ları hakkında bilgi sağlar.
> 
> 

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2015 veya Visual Studio 2017.
* Node.js 4.0.x sürümü veya sonraki bir sürüm.
* Etkin bir Azure hesabı. Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.

İzlediyseniz [cihaz çiftlerini ile çalışmaya başlama] [ lnk-twin-tutorial] öğretici, zaten sahip olduğunuz bir IOT hub ve adlı bir cihaz kimliği **myDeviceId**. Bu durumda, atlayabilirsiniz [sanal cihaz uygulaması oluşturma] [ lnk-how-to-configure-createapp] bölümü.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-the-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma
Bu bölümde, hub'ınıza bağlanan bir Node.js konsol uygulaması oluşturma **myDeviceId**, istenen yapılandırma güncelleştirmesi bekler ve daha sonra güncelleştirmeleri benzetimli yapılandırmasını güncelleştirme işlemini raporlar.

1. Adlı yeni bir boş klasör oluşturun **simulatedeviceconfiguration**. İçinde **simulatedeviceconfiguration** klasörü, komut isteminde aşağıdaki komutu kullanarak yeni bir package.json dosyası oluşturun. Tüm Varsayılanları kabul edin.
   
    ```
    npm init
    ```
1. Komut isteminizde **simulatedeviceconfiguration** klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure IOT cihaz** ve **azure-IOT-cihaz-mqtt** paketler:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
1. Bir metin düzenleyicisi kullanarak yeni bir oluşturma **SimulateDeviceConfiguration.js** dosyasını **simulatedeviceconfiguration** klasör.
1. Aşağıdaki kodu ekleyin **SimulateDeviceConfiguration.js** dosya ve yerine **{cihaz bağlantı dizesi}** yer tutucu oluştururken kopyaladığınız cihaz bağlantı dizesiyle **myDeviceId** cihaz kimliği:
   
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
   
    **İstemci** nesne cihaz çiftlerini aygıttan ile etkileşim kurmak için gereken tüm yöntemleri gösterir. Bu kod başlatır **istemci** nesnesi, cihaz çiftinin alır **myDeviceId**ve üzerinde güncelleştirme için bir işleyici ekler *özelliklerini istenen*. İşleyici olmadığını configIds karşılaştırarak bir gerçek yapılandırma değişikliği isteği olup, ardından yapılandırma değişikliği başlatan bir yöntemi çağırır doğrular.
   
    Basitleştirmek amacıyla, bu kod sabit kodlanmış bir varsayılan ilk yapılandırmasını kullandığına dikkat edin. Gerçek bir uygulama büyük olasılıkla bu yapılandırma yerel depolama biriminden yüklenir.
   
   > [!IMPORTANT]
   > İstenen özellik değiştirme olayları her zaman bir kez aygıt bağlantıdaki gösterilen. Herhangi bir işlem gerçekleştirmeden önce İstenen özelliklerde gerçek bir değişiklik olup olmadığını denetlemek emin olun.
   > 
   > 
1. Önce aşağıdaki yöntemleri ekleyin `client.open()` çağırma:
   
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
   
    **İnitConfigChange** yöntemi yerel cihaz çifti nesnesindeki bildirilen özellikleri yapılandırma güncelleştirme isteği ile güncelleştirir ve durumunu ayarlar **bekleyen**, hizmette cihaz çifti güncelleştirir. Cihaz çifti başarıyla güncelleştirdikten sonra yürütme işlemi sona erer uzun süre çalışan bir işlem benzetim **completeConfigChange**. Bu yöntem durumu ayarını yerel bildirilen özellikleri güncelleştirmeleri **başarı** ve kaldırma **pendingConfig** nesnesi. Ardından, cihaz çifti hizmette güncelleştirir.
   
    Bant genişliğini korumak için bildirilen özellikler, yalnızca değiştirilecek özelliklerini belirterek güncelleştirilir unutmayın (adlı **düzeltme eki** Yukarıdaki koddaki), tüm belgeyi değiştirme yerine.
   
   > [!NOTE]
   > Bu öğretici herhangi davranışı eşzamanlı yapılandırma güncelleştirmelerini benzetimini değil. Bazı yapılandırma güncelleştirme işlemleri hedef yapılandırma değişiklikleri güncelleştirme çalışan bazı bunları kuyruğuna olabilir ve bazı bir hata durumu reddedebilirsiniz sırada karşılamak mümkün olabilir. Belirli bir yapılandırma işlemi için istenen davranışı dikkate aldığınızdan emin olun ve yapılandırma değişikliğini başlatmadan önce uygun mantığı ekleyin.
   > 
   > 
1. Cihaz uygulaması çalıştırın:
   
        node SimulateDeviceConfiguration.js
   
    Şu iletiyi görürsünüz `retrieved device twin`. Uygulamayı çalıştıran tutun.

## <a name="create-the-service-app"></a>Hizmet Uygulaması Oluştur
Bu bölümde, güncelleştirmeleri bir .NET konsol uygulaması oluşturacak *özelliklerini istenen* ilişkili cihaz çifti üzerinde **myDeviceId** ile yeni bir telemetri yapılandırma nesnesi. IOT hub'ı depolanan cihaz çiftlerini sorgular ve cihazın istenen ve bildirilen yapılandırmaları arasındaki farkı gösterir.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Proje adı **SetDesiredConfigurationAndQuery**.
   
    ![Yeni Visual C# Windows Klasik Masaüstü projesi][img-createapp]
1. Çözüm Gezgini'nde sağ **SetDesiredConfigurationAndQuery** proje ve ardından **NuGet paketlerini Yönet...** .
1. **NuGet Paket Yöneticisi** penceresinde **Gözat**'ı seçin, **microsoft.azure.devices**'ı aratın, **Microsoft.Azure.Devices** paketini yüklemek için **Yükle**'yi seçin ve kullanım koşullarını kabul edin. Bu yordam ile [Azure IoT hizmet SDK'sı][lnk-nuget-service-sdk] NuGet paketi ve bağımlılıkları indirilir, yüklenir ve bu pakete bir başvuru eklenir.
   
    ![NuGet Paket Yöneticisi penceresi][img-servicenuget]
1. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
   
        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;
1. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini, önceki bölümde hub için oluşturduğunuz IoT Hub bağlantı dizesiyle değiştirin.
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. **Program** sınıfına aşağıdaki yöntemi ekleyin:
   
        static private async Task SetDesiredConfigurationAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch = new {
                    properties = new {
                        desired = new {
                            telemetryConfig = new {
                                configId = Guid.NewGuid().ToString(),
                                sendFrequency = "5m"
                            }
                        }
                    }
                };
   
            await registryManager.UpdateTwinAsync(twin.DeviceId, JsonConvert.SerializeObject(patch), twin.ETag);
            Console.WriteLine("Updated desired configuration");
   
            try
            {
                while (true)
                {
                    var query = registryManager.CreateQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'");
                    var results = await query.GetNextAsTwinAsync();
                    foreach (var result in results)
                    {
                        Console.WriteLine("Config report for: {0}", result.DeviceId);
                        Console.WriteLine("Desired telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Desired["telemetryConfig"], Formatting.Indented));
                        Console.WriteLine("Reported telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Reported["telemetryConfig"], Formatting.Indented));
                        Console.WriteLine();
                    }
                    Thread.Sleep(10000);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Exception: {ex.Message}");
            }
        }
   
    **Kayıt defteri** nesne cihaz çiftlerini hizmet ile etkileşim kurmak için gereken tüm yöntemleri gösterir. Bu kod başlatır **kayıt defteri** nesnesi, cihaz çiftinin alır **myDeviceId**ve ardından yeni bir telemetri yapılandırma nesnesi ile istenen özelliklerini güncelleştirir.
    Bundan sonra IOT hub'ı 10 saniyede depolanan cihaz çiftlerini sorgular ve istenen ve bildirilen telemetri yapılandırmaları yazdırır. Başvurmak [IOT hub'ı sorgu dili] [ lnk-query] tüm cihazlarınızda zengin raporlar üretmek hakkında bilgi edinmek için.
   
   > [!IMPORTANT]
   > Bu uygulama yalnızca tanım amaçlıdır her 10 saniye IOT hub'ı sorgular. Sorguları birçok cihaz arasında kullanıcı dönük raporları oluşturmak ve değil değişikliklerini algılamak için kullanın. Çözümünüz cihaz olayların gerçek zamanlı bildirimler gerektiriyorsa, kullanın [twin bildirimleri][lnk-twin-notifications].
   > 
   > 
1. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery().Wait();
        Console.WriteLine("Press any key to quit.");
        Console.ReadLine();
1. Çözüm Gezgini'nde açık **başlangıç projelerini Ayarla...**  ve emin olun **eylem** için **SetDesiredConfigurationAndQuery** projedir **Başlat**. Çözümü oluşturun.
1. İle **SimulateDeviceConfiguration.js** , .NET uygulaması Visual Studio kullanarak çalıştırma **F5** ve çıkarılıp bildirilen yapılandırma görmelisiniz. **başarı** için **bekleyen** için **başarı** yeni etkin 24 saat yerine beş dakikalık sıklığı yeniden gönderin.

 ![Cihaz başarıyla yapılandırıldı][img-deviceconfigured]
   
   > [!IMPORTANT]
   > Cihaz rapor işlemi ve sorgu sonucu arasında bir dakika kadar bir gecikme yoktur. Bu, çok yüksek ölçekli olarak çalışmak sorgu altyapısı olanak sağlamasıdır. Bir tek cihaz çifti kullanımı tutarlı görünümleri almak için **getDeviceTwin** yönteminde **kayıt defteri** sınıfı.
   > 
   > 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, istenen yapılandırma olarak kümesi *özellikleri istenen* çözümden arka uç ve bu değişikliği algılar ve durumunu bildirilen özellikleri aracılığıyla raporlama çok adımlı güncelleştirme işleminin benzetimini yapmak için bir cihaz uygulaması yazıldı.

Bilgi edinmek için aşağıdaki kaynakları kullanın nasıl yapılır:

* aygıtlarla telemetri gönderen [IOT Hub ile çalışmaya başlama] [ lnk-iothub-getstarted] öğretici
* zamanlama veya aygıtları bakın büyük kümeleri işlemleri [zamanlama ve yayın işleri] [ lnk-schedule-jobs] Öğreticisi.
* ile etkileşimli olarak (örneğin, kullanıcı tarafından denetlenen bir uygulamadan fan etkinleştirdikten), cihazları denetleme [doğrudan yöntemleri kullanın] [ lnk-methods-tutorial] Öğreticisi.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-how-to-configure/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-how-to-configure/createnetapp.png
[img-deviceconfigured]: media/iot-hub-csharp-node-twin-how-to-configure/deviceconfigured.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0/

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
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-csharp-node-twin-how-to-configure.md#create-the-simulated-device-app
