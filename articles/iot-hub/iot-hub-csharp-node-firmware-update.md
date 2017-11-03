---
title: "Cihaz üretici yazılımı güncelleştirme ile Azure IOT hub'ı (.NET/düğümü) | Microsoft Docs"
description: "Cihaz Yönetimi Azure IOT hub'ına aygıt üretici yazılımı güncelleştirmesi başlatmak için nasıl kullanılacağını. Sanal cihaz uygulaması ve Azure IOT hizmeti SDK'sını bellenim güncelleştirme tetikleyen bir hizmet uygulaması uygulamak .NET için uygulamak için Azure IOT cihaz SDK'sı Node.js için kullanın."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: 70b84258-bc9f-43b1-b7cf-de1bb715f2cf
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/30/2017
ms.author: juanpere
ms.openlocfilehash: 157f112869f0042e330e6b281367632ca015e890
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-device-management-to-initiate-a-device-firmware-update-netnode"></a>Bir cihaz üretici yazılımı güncelleştirmesi (.NET/düğümü) başlatmak için cihaz Yönetimi'ni kullanın
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

İçinde [aygıt Management'i kullanmaya başlama] [ lnk-dm-getstarted] Öğreticisi, nasıl kullanılacağını gördüğünüz [cihaz çifti] [ lnk-devtwin] ve [doğrudan yöntemleri ] [ lnk-c2dmethod] temelleri uzaktan bir aygıt yeniden başlatma. Bu öğretici aynı IOT hub'ı temelleri kullanır ve bir uçtan uca sanal üretici yazılımı güncelleştirme yapmak nasıl gösterir.  Bu desen bellenim güncelleştirme uygulaması için kullanılan [Raspberry Pi'yi cihaz uygulaması örnek][lnk-rpi-implementation].

Bu öğretici şunların nasıl yapıldığını gösterir:

* IOT hub'ınız aracılığıyla sanal cihaz uygulamasında firmwareUpdate doğrudan yöntemini çağıran bir .NET konsol uygulaması oluşturun.
* Arabirimini uygulayan bir sanal cihaz uygulaması oluşturma bir **firmwareUpdate** doğrudan yöntemi. Bu yöntem bellenim görüntüsünü karşıdan yüklemek için bekleyeceği, bellenim görüntüyü indirir ve son bellenim görüntü geçerli bir çok aşama işlemi başlatır. Güncelleştirme her aşaması sırasında aygıt ilerlemesini bildirmek için bildirilen özelliklerini kullanır.

Bu öğreticinin sonunda bir Node.js konsol cihaz uygulaması ve bir .NET (C#) konsol arka uç uygulaması sahiptir:

**dmpatterns_fwupdate_service.js**yanıt görüntüler, sanal cihaz uygulamada, doğrudan bir yöntemi çağırır ve düzenli aralıklarla (her 500ms) görüntüler güncelleştirilmiş bildirilen özellikleri.

**TriggerFWUpdate**, daha önce oluşturulan cihaz kimliğiyle IOT hub'ınızı bağlayan firmwareUpdate doğrudan bir yöntem alır, bellenim güncelleştirme dahil benzetimini yapmak için çok durumlu bir işlem çalışır: görüntü yükleme bekleniyor Yeni görüntüyü indirme ve son görüntü uygulama.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2015 veya Visual Studio 2017.
* Node.js sürümünü 4.0.x veya sonraki bir sürümü <br/>  [Geliştirme ortamınızı hazırlama] [ lnk-dev-setup] Node.js Bu öğretici için Windows veya Linux'ta nasıl yükleneceğini açıklar.
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

İzleyin [aygıt Management'i kullanmaya başlama](iot-hub-csharp-node-device-management-get-started.md) IOT hub'ınızı oluşturun ve IOT Hub bağlantı dizenizi almak için makale.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Doğrudan bir yöntem kullanarak aygıt bir uzak bellenim güncelleştirme Tetikle
Bu bölümde, bir cihazda uzaktan yazılımı güncelleştirmesi başlatır (C# kullanarak) bir .NET konsol uygulaması oluşturun. Uygulama güncelleştirme başlatmak için doğrudan bir yöntem kullanır ve etkin bellenim güncelleştirme durumunu düzenli aralıklarla almak için cihaz çifti sorgularını kullanır.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Proje adı **TriggerFWUpdate**.

    ![Yeni Visual C# Windows Klasik Masaüstü projesi][img-createapp]

1. Çözüm Gezgini'nde sağ **TriggerFWUpdate** proje ve ardından **NuGet paketlerini Yönet...** .
1. **NuGet Paket Yöneticisi** penceresinde **Gözat**'ı seçin, **microsoft.azure.devices**'ı aratın, **Microsoft.Azure.Devices** paketini yüklemek için **Yükle**'yi seçin ve kullanım koşullarını kabul edin. Bu yordam ile [Azure IoT hizmet SDK'sı][lnk-nuget-service-sdk] NuGet paketi ve bağımlılıkları indirilir, yüklenir ve bu pakete bir başvuru eklenir.

    ![NuGet Paket Yöneticisi penceresi][img-servicenuget]
1. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Shared;
        
1. **Program** sınıfına aşağıdaki alanları ekleyin. Birden çok yer tutucu değerlerini önceki bölümde ve Cihazınızı kimliğini oluşturulan hub'ın IOT Hub bağlantı dizesiyle değiştirin.
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "{deviceIdForTargetDevice}";
        
1. **Program** sınıfına aşağıdaki yöntemi ekleyin:
   
        public static async Task QueryTwinFWUpdateReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
1. **Program** sınıfına aşağıdaki yöntemi ekleyin:

        public static async Task StartFirmwareUpdate()
        {
            client = ServiceClient.CreateFromConnectionString(connString);
            CloudToDeviceMethod method = new CloudToDeviceMethod("firmwareUpdate");
            method.ResponseTimeout = TimeSpan.FromSeconds(30);
            method.SetPayloadJson(
                @"{
                    fwPackageUri : 'https://someurl'
                }");

            CloudToDeviceMethodResult result = await client.InvokeDeviceMethodAsync(targetDevice, method);

            Console.WriteLine("Invoked firmware update on device.");
        }

1. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartFirmwareUpdate().Wait();
        QueryTwinFWUpdateReported().Wait();
        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
        
1. Çözüm Gezgini'nde açık **başlangıç projelerini Ayarla...**  ve emin olun **eylem** için **TriggerFWUpdate** projedir **Başlat**.

1. Çözümü oluşturun.

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-the-apps"></a>Uygulamaları çalıştırma
Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Komut isteminde **manageddevice** klasörü, yeniden başlatma doğrudan yöntemi için dinleme başlamak için aşağıdaki komutu çalıştırın.
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. Visual Studio'da sağ tıklayın **TriggerFWUpdate** proje, select **hata ayıklama** ve **başlangıç yeni örnek**.

3. Konsolunda doğrudan yöntemi aygıt yanıta bakın.

    ![Bellenim başarıyla güncelleştirildi][img-fwupdate]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, doğrudan bir yöntem bir cihazda uzaktan Bellenim güncelleştirmesini tetiklemek için bildirilen özellikleri bellenim güncelleştirme durumunu izlemek için kullanılır ve.

Çözüm ve zamanlama yöntemini çağıran birden fazla cihazda, IOT genişletmek öğrenmek için bkz: [zamanlama ve yayın işleri] [ lnk-tutorial-jobs] Öğreticisi.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-firmware-update/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-firmware-update/createnetapp.png
[img-fwupdate]: media/iot-hub-csharp-node-firmware-update/fwupdated.png

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-rpi-implementation]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt_dm/pi_device
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/