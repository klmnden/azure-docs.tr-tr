---
title: Cihaz üretici yazılımını güncelleştirme ile Azure IOT hub'ı (.NET/.NET) | Microsoft Docs
description: Cihaz üretici yazılımı güncelleştirme başlatmak için Azure IOT Hub cihaz Yönetimi kullanma Bir sanal cihaz uygulaması ve Azure IOT hizmeti SDK'sını üretici yazılımı güncelleştirmesini tetikler bir hizmet uygulaması uygulamak .NET için Azure IOT cihaz SDK'sı .NET için kullanın.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 10/19/2017
ms.author: dobett
ms.openlocfilehash: 1bf7a647ab2fdc175231133b0cfd8abdd51b6d43
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38723934"
---
# <a name="use-device-management-to-initiate-a-device-firmware-update-netnet"></a>Cihaz üretici yazılımını güncelleştirme (.NET/.NET) başlatmak için cihaz Yönetimi'ni kullanın
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

## <a name="introduction"></a>Giriş
İçinde [cihaz yönetimini kullanmaya başlama] [ lnk-dm-getstarted] eğitmen, nasıl kullanılacağını gördüğünüz [cihaz ikizi] [ lnk-devtwin] ve [doğrudan yöntemler ] [ lnk-c2dmethod] ilkel bir cihazı Uzaktan yeniden başlatmak için. Bu öğretici aynı IOT hub'ı temelleri kullanır ve uçtan uca sanal üretici yazılımı güncelleştirme işlemini gösterir.  Bu düzen üretici yazılımı güncelleştirme uygulaması için kullanılan [Raspberry Pi cihaz uygulama örnek][lnk-rpi-implementation].

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* Çağıran bir .NET konsol uygulaması oluşturacaksınız **firmwareUpdate** yöntemi doğrudan IOT hub'ınız aracılığıyla sanal cihaz uygulamasında.
* Uygulayan bir sanal cihaz uygulaması oluşturma bir **firmwareUpdate** doğrudan yöntemi. Bu yöntem, üretici yazılımı görüntüsünü indirmeye bekler, üretici yazılımı görüntüsünü indirir ve son olarak üretici yazılımı görüntüsünü geçerlidir çok aşamalı bir işlem başlatır. Güncelleştirmenin her aşamasında cihaz ilerlemesini bildirmek üzere bildirilen özellikleri kullanır.

Bu öğreticinin sonunda, bir .NET (C#) konsol cihaz uygulaması ve bir .NET (C#) konsol arka uç uygulaması vardır:

* **SimulatedDeviceFwUpdate**, daha önce oluşturulan cihaz kimliğiyle IOT hub'ınıza bağlanan alır **firmwareUpdate** doğrudan yöntemini, üretici yazılımı güncelleştirme benzetimini yapmak için çok durumlu bir işlem çalıştırır dahil olmak üzere: görüntü yükleme bekleniyor, yeni görüntüyü indirme ve son olarak görüntüsü uygulanıyor.

* **TriggerFWUpdate**, uzaktan çağrılacak hizmeti SDK'sını kullanan **firmwareUpdate** doğrudan sanal cihaz üzerinde yöntem yanıt görüntüler ve (her 500ms) görüntüler güncelleştirilmiş rapor düzenli aralıklarla özellikleri.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2015 veya Visual Studio 2017.
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

İzleyin [cihaz yönetimini kullanmaya başlama](iot-hub-csharp-csharp-device-management-get-started.md) IOT hub'ınızı oluşturması ve IOT Hub bağlantı dizesini almak için makale.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Bir doğrudan yöntem kullanarak cihaz üzerinde bir uzak üretici yazılımı güncelleştirmesini tetikleme
Bu bölümde, bir cihazda uzak üretici yazılımı güncelleştirme işlemini başlatır (C# kullanarak) bir .NET konsol uygulaması oluşturun. Uygulamayı bir doğrudan yöntem güncelleştirmeyi başlatmak için ve cihaz çifti sorguları etkin üretici yazılımı güncelleştirme durumunu düzenli aralıklarla almak için kullanır.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Projeyi adlandırın **TriggerFWUpdate**.

    ![Yeni Visual C# Windows Klasik Masaüstü projesi][img-createserviceapp]

2. Çözüm Gezgini'nde sağ **TriggerFWUpdate** proje ve ardından **NuGet paketlerini Yönet**.
3. **NuGet Paket Yöneticisi** penceresinde **Gözat**'ı seçin, **microsoft.azure.devices**'ı aratın, **Microsoft.Azure.Devices** paketini yüklemek için **Yükle**'yi seçin ve kullanım koşullarını kabul edin. Bu yordam ile [Azure IoT hizmet SDK'sı][lnk-nuget-service-sdk] NuGet paketi ve bağımlılıkları indirilir, yüklenir ve bu pakete bir başvuru eklenir.

    ![NuGet Paket Yöneticisi penceresi][img-servicenuget]
4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

    ```csharp   
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

5. **Program** sınıfına aşağıdaki alanları ekleyin. Birden fazla yer tutucu değerlerini, cihaz Kimliğini ve önceki bölümde oluşturduğunuz hub'ın IOT Hub bağlantı dizesiyle değiştirin.
   
    ```csharp   
    static RegistryManager registryManager;
    static string connString = "{iot hub connection string}";
    static ServiceClient client;
    static string targetDevice = "{deviceIdForTargetDevice}";
    ```
        
6. **Program** sınıfına aşağıdaki yöntemi ekleyin. Bu yöntem, güncelleştirilmiş durumu için cihaz ikizi her aralığını 500 milisaniyenin yoklar. Yalnızca durum gerçekten değiştiğinde konsola yazar. Ek IOT hub'ı iletileri, aboneliğinizdeki kullanmaya önlemek için bu örnek için yoklama durakları cihaz durumunu bildirdiğinde **applyComplete** veya bir hata oluştu.  
   
    ```csharp   
    public static async Task QueryTwinFWUpdateReported(DateTime startTime)
    {
        DateTime lastUpdated = startTime;

        while (true)
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);

            if (twin.Properties.Reported.GetLastUpdated().ToUniversalTime() > lastUpdated.ToUniversalTime())
            {
                lastUpdated = twin.Properties.Reported.GetLastUpdated().ToUniversalTime();
                Console.WriteLine("\n" + twin.Properties.Reported["iothubDM"].ToJson());

                var status = twin.Properties.Reported["iothubDM"]["firmwareUpdate"]["status"].Value;
                if ((status == "downloadFailed") || (status == "applyFailed") || (status == "applyComplete"))
                {
                    Console.WriteLine("\nStop polling.");
                    return;
                }
            }
            await Task.Delay(500);
        }
    }
    ```
        
7. **Program** sınıfına aşağıdaki yöntemi ekleyin:

    ```csharp   
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
    ```

8. Son olarak, aşağıdaki satırları ekleyin **ana** yöntemi. Bu cihaz ikizi ile başlar, yoklama görev bir çalışan iş parçacığı üzerinde okumak için Yöneticisi bir kayıt defteri oluşturur ve ardından üretici yazılımı güncelleştirmesini tetikler.
   
    ```csharp   
    registryManager = RegistryManager.CreateFromConnectionString(connString);

    Task queryTask = Task.Run(() => (QueryTwinFWUpdateReported(DateTime.Now)));

    StartFirmwareUpdate().Wait();
    Console.WriteLine("Press ENTER to exit.");
    Console.ReadLine();
    ```
        
9. Çözümü derleyin.

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma
Bu bölümde şunları yapacaksınız:

* Bulut tarafından çağrılan doğrudan bir yönteme yanıt veren bir .NET konsol uygulaması oluşturma
* Bir doğrudan yöntem aracılığıyla arka uç hizmeti tarafından tetiklenen bir üretici yazılımı güncelleştirmesini benzetimi
* Cihaz ikizi sorgularının cihazları ve bir üretici yazılımı güncelleştirmesini en son ne zaman tamamladığını belirlemesi için rapor edilen özellikleri kullanın

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Projeyi adlandırın **SimulateDeviceFWUpdate**.
   
    ![Yeni Windows Visual C# Klasik cihaz uygulaması][img-createdeviceapp]
    
2. Çözüm Gezgini'nde sağ **SimulateDeviceFWUpdate** proje ve ardından **NuGet paketlerini Yönet**.
3. İçinde **NuGet Paket Yöneticisi** penceresinde **Gözat** araması **microsoft.azure.devices.client**. Seçin **yükleme** yüklemek için **Microsoft.Azure.Devices.Client** paketini ve kullanım koşullarını kabul edin. Bu yordam indirir, yükler ve bir başvuru ekler [Azure IOT cihaz SDK'sını] [ lnk-nuget-client-sdk] NuGet paketi ve bağımlılıkları.
   
    ![NuGet Paket Yöneticisi penceresi istemci uygulaması][img-clientnuget]
4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
   
    ```csharp   
    using Newtonsoft.Json.Linq;
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    ```

5. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini de not ettiğiniz cihaz bağlantı dizesiyle değiştirin **bir cihaz kimliği oluşturma** bölümü.
   
    ```csharp   
    static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
    static DeviceClient Client = null;
    ```

6. Cihaz ikizi aracılığıyla buluta rapor durumu aşağıdaki yöntemi ekleyin: 

    ```csharp   
    static async Task reportFwUpdateThroughTwin(Twin twin, TwinCollection fwUpdateValue)
    {
        try
        {
            TwinCollection patch = new TwinCollection();
            TwinCollection iothubDM = new TwinCollection();

            iothubDM["firmwareUpdate"] = fwUpdateValue;
            patch["iothubDM"] = iothubDM;

            await Client.UpdateReportedPropertiesAsync(patch);
            Console.WriteLine("Twin state reported: {0}", fwUpdateValue["status"]);
        }
        catch 
        {
            Console.WriteLine("Error updating device twin");
            throw;
        }
    }
    ```

7. Bellenim görüntüsü indiriliyor benzetimini yapmak için aşağıdaki yöntemi ekleyin:
        
    ```csharp   
    static async Task<byte[]> simulateDownloadImage(string imageUrl)
    {
        var image = "[fake image data]";

        Console.WriteLine("Downloading image from " + imageUrl);

        await Task.Delay(4000);

        return Encoding.ASCII.GetBytes(image);
            
    }
    ```

8. Cihaza üretici yazılımı görüntüsünü uygulama benzetimini yapmak için aşağıdaki yöntemi ekleyin:
        
    ```csharp   
    static async Task simulateApplyImage(byte[] imageData)
    {
        if (imageData == null)
        {
            throw new ArgumentNullException();
        }

        await Task.Delay(4000);

    }
    ```
 
9.  Üretici yazılımı görüntüsünü indirmeye bekleyen benzetimini yapmak için aşağıdaki yöntemi ekleyin. Güncelleştirme durumu için **bekleyen** ikizi diğer üretici yazılımı güncelleştirme özellikleri temizleyin. Bu özellikler, mevcut değerlerle önceki üretici yazılımı güncelleştirmelerini kaldırmak için temizlenir. Bunun gerekli olmasının nedeni, bildirilen özellikleri düzeltme eki işlemi (delta) ve (tüm önceki değerleri yerini alan eksiksiz bir kümesini özellikleri) değil PUT işlemi gönderilir. Genellikle, cihazlar kullanılabilir güncelleştirmeden haberdar edilir ve yönetici tarafından tanımlanan bir ilke, cihazın güncelleştirmeyi indirip uygulamaya başlamasına neden olur. Bu işlev, ilgili ilkeyi etkinleştirme mantığının çalışması gereken yerdir. 
        
    ```csharp   
    static async Task waitToDownload(Twin twin, string fwUpdateUri)
    {
        var now = DateTime.Now;
        TwinCollection status = new TwinCollection();
        status["fwPackageUri"] = fwUpdateUri;
        status["status"] = "waiting";
        status["error"] = null;
        status["startedWaitingTime"] = DateTime.Now;
        status["downloadCompleteTime"] = null;
        status["startedApplyingImage"] = null;
        status["lastFirmwareUpdate"] = null;

        await reportFwUpdateThroughTwin(twin, status);

        await Task.Delay(2000);
    }
    ```

10. Yüklemeyi gerçekleştirmek için aşağıdaki yöntemi ekleyin. Durum güncelleştirmeleri **indirme** cihaz ikizi, benzetim indirme yöntemini çağırır ve bir durumunu raporlar **downloadComplete** veya **downloadFailed** İndirme işlemi sonuçlarına bağlı olarak ikizi. 
        
    ```csharp   
    static async Task<byte[]> downloadImage(Twin twin, string fwUpdateUri)
    {
        try
        {
            TwinCollection statusUpdate = new TwinCollection();
            statusUpdate["status"] = "downloading";
            await reportFwUpdateThroughTwin(twin, statusUpdate);

            byte[] imageData = await simulateDownloadImage(fwUpdateUri);

            statusUpdate = new TwinCollection();
            statusUpdate["status"] = "downloadComplete";
            statusUpdate["downloadCompleteTime"] = DateTime.Now;
            await reportFwUpdateThroughTwin(twin, statusUpdate);
            return imageData;
        }
        catch (Exception ex)
        {
            TwinCollection statusUpdate = new TwinCollection();
            statusUpdate["status"] = "downloadFailed";
            statusUpdate["error"] = new TwinCollection();
            statusUpdate["error"]["code"] = ex.GetType().ToString();
            statusUpdate["error"]["message"] = ex.Message;
            await reportFwUpdateThroughTwin(twin, statusUpdate);
            throw;
        }
    }
    ```

11. Görüntüsünü uygulamak için aşağıdaki yöntemi ekleyin. Durum güncelleştirmeleri **uygulama** cihaz ikizi, benzetim uygulama çağrıları görüntü yöntemi ve güncelleştirme durumu **applyComplete** veya **applyFailed** aracılığıyla ikiz uygulama işleminin sonuçlarına bağlı olarak. 
        
    ```csharp   
    static async Task applyImage(Twin twin, byte[] imageData)
    {
        try
        {
            TwinCollection statusUpdate = new TwinCollection();
            statusUpdate["status"] = "applying";
            statusUpdate["startedApplyingImage"] = DateTime.Now;
            await reportFwUpdateThroughTwin(twin, statusUpdate);

            await simulateApplyImage(imageData);

            statusUpdate = new TwinCollection();
            statusUpdate["status"] = "applyComplete";
            statusUpdate["lastFirmwareUpdate"] = DateTime.Now;
            await reportFwUpdateThroughTwin(twin, statusUpdate);
        }
        catch (Exception ex)
        {
            TwinCollection statusUpdate = new TwinCollection();
            statusUpdate["status"] = "applyFailed";
            statusUpdate["error"] = new TwinCollection();
            statusUpdate["error"]["code"] = ex.GetType().ToString();
            statusUpdate["error"]["message"] = ex.Message;
            await reportFwUpdateThroughTwin(twin, statusUpdate);
            throw;
        }
    }
    ```

12. Görüntüyü uygulamadan cihaza aracılığıyla üzere görüntüyü indirmek için bekleyen üretici yazılımı güncelleştirme işleminin sıralamak için aşağıdaki yöntemi ekleyin:
        
    ```csharp   
    static async Task doUpdate(string fwUpdateUrl)
    {
        try
        {
            Twin twin = await Client.GetTwinAsync();
            await waitToDownload(twin, fwUpdateUrl);
            byte[] imageData = await downloadImage(twin, fwUpdateUrl);
            await applyImage(twin, imageData);
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error during update: {0}", ex.Message);
        }
    }
    ```

13. İşlemek için aşağıdaki yöntemi ekleyin **updateFirmware** doğrudan buluttan yöntemi. URL üretici yazılımı güncelleştirme iletisi yükten ayıklar ve buna ileten **doUpdate** başka bir iş parçacığı havuzu iş parçacığı üzerinde başlayan bir görev. Daha sonra hemen buluta yöntemi yanıtı döndürür.
        
    ```csharp   
    static Task<MethodResponse> onFirmwareUpdate(MethodRequest methodRequest, object userContext)
    {
        string fwUpdateUrl = (string)JObject.Parse(methodRequest.DataAsJson)["fwPackageUri"];
        Console.WriteLine("\nMethod: {0} triggered by service, URI is: {1}", methodRequest.Name, fwUpdateUrl);

        Task updateTask = Task.Run(() => (doUpdate(fwUpdateUrl)));

        string result = "'FirmwareUpdate started.'";
        return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
    }
    ```
> [!NOTE]
> Sanal bir güncelleştirme olarak çalıştırmak için bu yöntem tetikleyen bir **görev** ve üretici yazılımı güncelleştirmesi başlatıldı hizmet bildiren yöntem çağrısına hemen yanıt verir. Güncelleştirme durumu ve tamamlanmayı cihaz çiftinin bildirilen özelliklerini üzerinden hizmetine gönderilir. Güncelleştirmeden yöntemi çağrısını yerine kendi tamamlandıktan sonra yanıtlıyoruz. nedeni:
> * Gerçek güncelleştirme işlemi yöntemi çağrısı zaman aşımı süresinden daha uzun sürmesine olasılıktır.
> * Bu uygulama oluşturma, yeniden başlatma bir yeniden başlatma gerektiren büyük olasılıkla gerçek güncelleştirme işlemidir **MethodRequest** nesnesi kullanılabilir değil. (Bildirilen özellikleri güncelleştirmek ancak yeniden başlatmanın ardından bile mümkündür.) 

14. Son olarak, aşağıdaki kodu ekleyin **ana** yöntemi, IOT hub'ınıza bağlantıyı açın ve yöntemi dinleyicisi başlatılamıyor:
   
    ```csharp   
    try
    {
        Console.WriteLine("Connecting to hub");
        Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);
        
        // setup callback for "firmware update" method
        Client.SetMethodHandlerAsync("firmwareUpdate", onFirmwareUpdate, null).Wait();
        Console.WriteLine("Waiting for firmware update direct method call\n Press enter to exit.");
        Console.ReadLine();

        Console.WriteLine("Exiting...");

        // as a good practice, remove the firmware update handler
        Client.SetMethodHandlerAsync("firmwareUpdate", null, null).Wait();
        Client.CloseAsync().Wait();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```
        
15. Çözümü derleyin.       

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. [Geçici Hata İşleme][lnk-transient-faults] adlı MSDN makalesinde önerildiği üzere, üretim kodunda yeniden deneme ilkelerini (üstel geri alma gibi) uygulamanız gerekir.


## <a name="run-the-apps"></a>Uygulamaları çalıştırma
Şimdi uygulamaları çalıştırmaya hazırsınız.
1. .NET cihaz uygulamasını çalıştırın **SimulatedDeviceFWUpdate**.  Sağ **SimulatedDeviceFWUpdate** proje, select **hata ayıklama**ve ardından **yeni örnek Başlat**. IOT Hub'ınıza yöntemine yapılan çağrılar için dinleme başlamanız gerekir: 

2. Cihaz bağlı ve .NET çalıştırmak için yöntem çağrılarına, bekleyen sonra **TriggerFWUpdate** sanal cihaz uygulaması, üretici yazılımı güncelleştirme yöntemini çağırmak için uygulama. Sağ **TriggerFWUpdate** proje, select **hata ayıklama**ve ardından **yeni örnek Başlat**. Görmelisiniz güncelleştirme dizisi yazılan **SimulatedDeviceFWUpdate** konsolu ve ayrıca dizisi bildirilen cihazın rapor edilen özelliklerle **TriggerFWUpdate** Konsolu. İşlemin tamamlanması birkaç saniye sürer. 
   
    ![Hizmet ve cihaz uygulama çalıştırma][img-combinedrun]


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir cihazda bir uzak üretici yazılımı güncelleştirmesini tetikleme için kullanılan bir doğrudan yöntem ve bildirilen özellikleri üretici yazılımı güncelleştirme durumunu izlemek için kullanılır.

IOT çözümü ve zamanlama yöntemi çağıran birden çok cihazda genişletmek öğrenmek için bkz [işleri zamanlama ve yayınlama] [ lnk-tutorial-jobs] öğretici.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-csharp-firmware-update/servicesdknuget.png
[img-clientnuget]: media/iot-hub-csharp-csharp-firmware-update/clientsdknuget.png
[img-createserviceapp]: media/iot-hub-csharp-csharp-firmware-update/createserviceapp.png
[img-createdeviceapp]: media/iot-hub-csharp-csharp-firmware-update/createdeviceapp.png
[img-combinedrun]: media/iot-hub-csharp-csharp-firmware-update/combinedrun.png

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-csharp-csharp-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-csharp-csharp-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-rpi-implementation]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt_dm/pi_device
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
