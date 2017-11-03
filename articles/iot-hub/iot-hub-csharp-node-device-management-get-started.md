---
title: "Azure IOT Hub cihaz Yönetimi (.NET/düğümü) ile çalışmaya başlama | Microsoft Docs"
description: "Uzak aygıt yeniden başlatma işlemi başlatmak için Azure IOT Hub cihaz yönetimini kullanma Doğrudan bir yöntem ve Azure IOT hizmeti SDK'sını doğrudan yöntemini çağıran bir hizmet uygulaması uygulamak .NET için içeren bir sanal cihaz uygulamasının uygulamak için Azure IOT cihaz SDK'sı Node.js için kullanın."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: e044006d-ffd6-469b-bc63-c182ad066e31
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: juanpere
ms.openlocfilehash: 5d0b7b1ab5893e55a6e2aa16451b6a9fc1481966
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2017
---
# <a name="get-started-with-device-management-netnode"></a>Aygıt Yönetimi (.NET/düğümü) ile çalışmaya başlama

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* IOT hub'ı oluşturun ve IOT hub'ınızda bir cihaz kimliği oluşturma için Azure Portalı'nı kullanın.
* Bu cihazı yeniden başlatır doğrudan bir yöntem içeren bir sanal cihaz uygulaması oluşturursunuz. Doğrudan yöntemleri buluttan çağrılır.
* IOT hub'ınız aracılığıyla sanal cihaz uygulama yeniden başlatma doğrudan yöntemini çağıran bir .NET konsol uygulaması oluşturun.

Bu öğreticinin sonunda bir Node.js konsol cihaz uygulaması ve bir .NET (C#) konsol arka uç uygulaması sahiptir:

**dmpatterns_getstarted_device.js**, daha önce oluşturulan cihaz kimliğiyle IOT hub'ınızı bağlayan bir yeniden başlatma doğrudan yöntemi alır fiziksel yeniden başlatma taklit eder ve son yeniden başlatma zamanı raporlar.

**TriggerReboot**, yanıt görüntüler, sanal cihaz uygulamada, doğrudan bir yöntemi çağırır ve görüntüler güncelleştirilmiş rapor özellikleri.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2015 veya Visual Studio 2017.
* Node.js sürümünü 4.0.x veya sonraki bir sürümü <br/>  [Geliştirme ortamınızı hazırlama] [ lnk-dev-setup] Node.js Bu öğretici için Windows veya Linux'ta nasıl yükleneceğini açıklar.
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Tetikleyici doğrudan bir yöntem kullanarak cihaz üzerinde Uzaktan yeniden başlatma
Bu bölümde, doğrudan bir yöntem kullanarak bir cihazda Uzaktan yeniden başlatma başlatır (C# kullanarak) bir .NET konsol uygulaması oluşturun. Uygulama cihaz çifti sorguları bu aygıtın son yeniden başlatma zamanını bulmak için kullanır.

1. Visual Studio’da **Konsol Uygulaması (.NET Framework)** proje şablonunu kullanarak yeni bir çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. .NET Framework sürümünün 4.5.1 veya sonraki bir sürüm olduğundan emin olun. Proje adı **TriggerReboot**.

    ![Yeni Visual C# Windows Klasik Masaüstü projesi][img-createapp]

2. Çözüm Gezgini'nde sağ **TriggerReboot** proje ve ardından **NuGet paketlerini Yönet**.
3. **NuGet Paket Yöneticisi** penceresinde **Gözat**'ı seçin, **microsoft.azure.devices**'ı aratın, **Microsoft.Azure.Devices** paketini yüklemek için **Yükle**'yi seçin ve kullanım koşullarını kabul edin. Bu yordam ile [Azure IoT hizmet SDK'sı][lnk-nuget-service-sdk] NuGet paketi ve bağımlılıkları indirilir, yüklenir ve bu pakete bir başvuru eklenir.

    ![NuGet Paket Yöneticisi penceresi][img-servicenuget]
4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Shared;
        
5. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini "IOT hub oluşturma." bölümünde oluşturduğunuz hub'ın IOT Hub bağlantı dizesiyle değiştirin 
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "myDeviceId";
        
6. Aşağıdaki yöntemi ekleyin **Program** sınıfı.  Bu kod yeniden başlatılan cihaz için cihaz çifti alır ve bildirilen özellikleri çıkarır.
   
        public static async Task QueryTwinRebootReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
7. Aşağıdaki yöntemi ekleyin **Program** sınıfı.  Bu kod, doğrudan bir yöntem kullanarak cihazı önyüklemede başlatır.

        public static async Task StartReboot()
        {
            client = ServiceClient.CreateFromConnectionString(connString);
            CloudToDeviceMethod method = new CloudToDeviceMethod("reboot");
            method.ResponseTimeout = TimeSpan.FromSeconds(30);

            CloudToDeviceMethodResult result = await client.InvokeDeviceMethodAsync(targetDevice, method);

            Console.WriteLine("Invoked firmware update on device.");
        }

7. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartReboot().Wait();
        QueryTwinRebootReported().Wait();
        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
        
8. Çözümü oluşturun.

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma
Bu bölümde şunları yapacaksınız:

* Bulut tarafından çağrılan doğrudan bir yönteme yanıt veren bir Node.js konsol uygulaması oluşturma
* Tetikleyici sanal cihaz yeniden başlatma
* Aygıtları ve bunların en son ne zaman yeniden tanımlamak için cihaz çifti sorgular etkinleştirmek için bildirilen özelliklerini kullanın

1. Adlı yeni bir boş klasör oluşturun **manageddevice**.  Komut isteminizde aşağıdaki komutu kullanarak **manageddevice** klasöründe bir package.json dosyası oluşturun.  Tüm varsayılanları kabul edin:
   
    ```
    npm init
    ```
2. Komut isteminizde **manageddevice** klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure IOT cihaz** cihaz SDK'sı paketinin ve **azure-IOT-cihaz-mqtt** paketi:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Bir metin düzenleyicisi kullanarak yeni bir oluşturma **dmpatterns_getstarted_device.js** dosyasını **manageddevice** klasör.
4. Aşağıdaki 'İste' deyimleri başlangıcında eklemek **dmpatterns_getstarted_device.js** dosyası:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. Bir **connectionString** değişkeni ekleyin ve bir **İstemci** örneği oluşturmak için bunu kullanın.  Bağlantı dizesi, cihaz bağlantı dizesi ile değiştirin.  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. Cihazda doğrudan yöntemi uygulamak için aşağıdaki işlevi ekleyin
   
    ```
    var onReboot = function(request, response) {
   
        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        // Report the reboot before the physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };
   
        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
   
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```
7. IOT hub'ınıza bağlantıyı açın ve doğrudan yöntemi dinleyicisini başlatmak için aşağıdaki kodu ekleyin:
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
8. Kaydet ve Kapat **dmpatterns_getstarted_device.js** dosya.
   
> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. [Geçici Hata İşleme][lnk-transient-faults] adlı MSDN makalesinde önerildiği üzere, üretim kodunda yeniden deneme ilkelerini (üstel geri alma gibi) uygulamanız gerekir.


## <a name="run-the-apps"></a>Uygulamaları çalıştırma
Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Komut isteminde **manageddevice** klasörü, yeniden başlatma doğrudan yöntemi için dinleme başlamak için aşağıdaki komutu çalıştırın.
   
    ```
    node dmpatterns_getstarted_device.js
    ```
2. C# konsol uygulaması çalıştırmak **TriggerReboot**. Sağ **TriggerReboot** proje, select **hata ayıklama**ve ardından **başlangıç yeni örnek**.

3. Konsolunda doğrudan yöntemi aygıt yanıta bakın.

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png
[img-servicenuget]: media/iot-hub-csharp-node-device-management-get-started/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-device-management-get-started/createnetapp.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
