---
title: Azure IOT Hub cihaz Yönetimi (.NET/.NET) ile çalışmaya başlama | Microsoft Docs
description: Uzak aygıt yeniden başlatma işlemi başlatmak için Azure IOT Hub cihaz yönetimini kullanma Doğrudan bir yöntem ve Azure IOT hizmeti SDK'sını doğrudan yöntemini çağıran bir hizmet uygulaması uygulamak .NET için içeren bir sanal cihaz uygulamasının uygulamak için Azure IOT cihaz SDK'sı .NET için kullanın.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: ''
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/15/2017
ms.author: v-jamebr;dobett
ms.openlocfilehash: 44160eeb90f0f65c974b7188dd7c70cce382bf21
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="get-started-with-device-management-netnet"></a>Aygıt Yönetimi (.NET/.NET) ile çalışmaya başlama

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* IOT hub'ı oluşturun ve IOT hub'ınızda bir cihaz kimliği oluşturma için Azure Portalı'nı kullanın.
* Bu cihazı yeniden başlatır doğrudan bir yöntem içeren bir sanal cihaz uygulaması oluşturursunuz. Doğrudan yöntemleri buluttan çağrılır.
* IOT hub'ınız aracılığıyla sanal cihaz uygulama yeniden başlatma doğrudan yöntemini çağıran bir .NET konsol uygulaması oluşturun.

Bu öğreticinin sonunda iki .NET konsol uygulamaları vardır:

* **SimulateManagedDevice**, daha önce oluşturulan cihaz kimliğiyle IOT hub'ınızı bağlayan bir yeniden başlatma doğrudan yöntemi alır fiziksel yeniden başlatma taklit eder ve son yeniden başlatma zamanı raporlar.
* **TriggerReboot**, yanıt görüntüler, sanal cihaz uygulamada, doğrudan bir yöntemi çağırır ve görüntüler güncelleştirilmiş rapor özellikleri.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2015 veya Visual Studio 2017.
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

<a id="DeviceIdentity_csharp"></a>
[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Tetikleyici doğrudan bir yöntem kullanarak cihaz üzerinde Uzaktan yeniden başlatma
Bu bölümde, doğrudan bir yöntem kullanarak bir cihazda Uzaktan yeniden başlatma başlatır (C# kullanarak) bir .NET konsol uygulaması oluşturun. Uygulama cihaz çifti sorguları bu aygıtın son yeniden başlatma zamanını bulmak için kullanır.

1. Visual Studio’da **Konsol Uygulaması (.NET Framework)** proje şablonunu kullanarak yeni bir çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. .NET Framework sürümünün 4.5.1 veya sonraki bir sürüm olduğundan emin olun. Proje adı **TriggerReboot**.

    ![Yeni Visual C# Windows Klasik Masaüstü projesi][img-createserviceapp]

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
        static string targetDevice = "myDeviceId";
        
6. **Program** sınıfına aşağıdaki yöntemi ekleyin.  Bu kod yeniden başlatılan cihaz için cihaz çifti alır ve bildirilen özellikleri çıkarır.
   
        public static async Task QueryTwinRebootReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
7. **Program** sınıfına aşağıdaki yöntemi ekleyin.  Bu kod, doğrudan bir yöntem kullanarak cihazı önyüklemede başlatır.

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
        
8. Çözümü derleyin.

> [!NOTE]
> Bu öğretici için bildirilen özellikler yalnızca tek bir sorgu gerçekleştirir. Üretim kodunda bildirilen özelliklerinde değişikliklerini algılamak için yoklama öneririz.

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma
Bu bölümde şunları yapacaksınız:

* Bulut tarafından adlı doğrudan bir yöntem yanıtlaması bir .NET konsol uygulaması oluşturma
* Tetikleyici sanal cihaz yeniden başlatma
* Aygıtları ve bunların en son ne zaman yeniden tanımlamak için cihaz çifti sorgular etkinleştirmek için bildirilen özelliklerini kullanın

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Proje adı **SimulateManagedDevice**.
   
    ![Yeni Visual C# Klasik Windows cihaz uygulaması][img-createdeviceapp]
    
2. Çözüm Gezgini'nde sağ **SimulateManagedDevice** proje ve ardından **NuGet paketlerini Yönet...** .
3. İçinde **NuGet Paket Yöneticisi** penceresinde, seçin **Gözat** arayın ve **microsoft.azure.devices.client**. Seçin **yükleme** yüklemek için **Microsoft.Azure.Devices.Client** paketini ve kullanım koşullarını kabul edin. Bu yordam indirir, yükler ve bir başvuru ekler [Azure IOT cihaz SDK'sı] [ lnk-nuget-client-sdk] NuGet paketi ve bağımlılıklarını.
   
    ![NuGet Paket Yöneticisi penceresi istemci uygulaması][img-clientnuget]
4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;

5. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini önceki bölümünde belirtildiği cihaz bağlantı dizesini değiştirin.
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;

6. Cihazda doğrudan yöntemi uygulamak için aşağıdakileri ekleyin:

        static Task<MethodResponse> onReboot(MethodRequest methodRequest, object userContext)
        {
            // In a production device, you would trigger a reboot scheduled to start after this method returns
            // For this sample, we simulate the reboot by writing to the console and updating the reported properties 
            try
            {
                Console.WriteLine("Rebooting!");

                // Update device twin with reboot time. 
                TwinCollection reportedProperties, reboot, lastReboot;
                lastReboot = new TwinCollection();
                reboot = new TwinCollection();
                reportedProperties = new TwinCollection();
                lastReboot["lastReboot"] = DateTime.Now;
                reboot["reboot"] = lastReboot;
                reportedProperties["iothubDM"] = reboot;
                Client.UpdateReportedPropertiesAsync(reportedProperties).Wait();
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }

            string result = "'Reboot started.'";
            return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
        }

7. Son olarak, aşağıdaki kodu ekleyin **ana** IOT hub'ınıza bağlantıyı açın ve yöntemi dinleyicisini başlatmak için yöntem:
   
        try
        {
            Console.WriteLine("Connecting to hub");
            Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);

            // setup callback for "reboot" method
            Client.SetMethodHandlerAsync("reboot", onReboot, null).Wait();
            Console.WriteLine("Waiting for reboot method\n Press enter to exit.");
            Console.ReadLine();

            Console.WriteLine("Exiting...");

            // as a good practice, remove the "reboot" handler
            Client.SetMethodHandlerAsync("reboot", null, null).Wait();
            Client.CloseAsync().Wait();
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }
        
8. Visual Studio Çözüm Gezgini'nde Çözümünüze sağ tıklayın ve ardından **başlangıç projelerini Ayarla...** . Seçin **tek başlangıç projesi**ve ardından **SimulateManagedDevice** açılır menüsünde projesinde. Çözümü derleyin.       

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. [Geçici Hata İşleme][lnk-transient-faults] adlı MSDN makalesinde önerildiği üzere, üretim kodunda yeniden deneme ilkelerini (üstel geri alma gibi) uygulamanız gerekir.


## <a name="run-the-apps"></a>Uygulamaları çalıştırma
Şimdi uygulamaları çalıştırmaya hazırsınız.
1. .NET cihaz uygulama çalıştırma **SimulateManagedDevice**.  Sağ **SimulateManagedDevice** proje, select **hata ayıklama**ve ardından **başlangıç yeni örnek**. IOT Hub'ınızı gelen yöntem çağrıları için dinleme başlamanız gerekir: 

2. Artık, cihazın bağlı ve .NET çalıştırmak için yöntem çağrılarına, bekleyen **TriggerReboot** sanal cihaz uygulama yeniden başlatma yöntemini çağırmak için uygulama. Sağ **TriggerReboot** proje, select **hata ayıklama**ve ardından **başlangıç yeni örnek**. "Rebooting!" görmeniz gerekir yazılan **SimulatedManagedDevice** konsolu ve son dahil bildirilen cihaz özelliklerini yeniden yazılan zaman **TriggerReboot** konsol.
   
    ![Çalıştırma hizmet ve cihaz uygulaması][img-combinedrun]

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png
[img-servicenuget]: media/iot-hub-csharp-csharp-device-management-get-started/servicesdknuget.png
[img-createserviceapp]: media/iot-hub-csharp-csharp-device-management-get-started/createserviceapp.png
[img-clientnuget]: media/iot-hub-csharp-csharp-device-management-get-started/clientsdknuget.png
[img-createdeviceapp]: media/iot-hub-csharp-csharp-device-management-get-started/createdeviceapp.png
[img-combinedrun]: media/iot-hub-csharp-csharp-device-management-get-started/combinedrun.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
