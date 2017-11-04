---
title: "Cihaz üretici yazılımı güncelleştirmesi ile Azure IOT hub'ı (.NET/.NET) | Microsoft Docs"
description: "Cihaz Yönetimi Azure IOT hub'ına aygıt üretici yazılımı güncelleştirmesi başlatmak için nasıl kullanılacağını. Sanal cihaz uygulaması ve Azure IOT hizmeti SDK'sını bellenim güncelleştirme tetikleyen bir hizmet uygulaması uygulamak .NET için uygulamak için Azure IOT cihaz SDK'sı .NET için kullanın."
services: iot-hub
documentationcenter: .net
author: JimacoMS2
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/19/2017
ms.author: v-jamebr
ms.openlocfilehash: bd0a227861d75dc66af8fb4865a17a3b6d0f70ba
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2017
---
# <a name="use-device-management-to-initiate-a-device-firmware-update-netnet"></a>Bir cihaz üretici yazılımı güncelleştirmesi (.NET/.NET) başlatmak için cihaz Yönetimi'ni kullanın
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

## <a name="introduction"></a>Giriş
İçinde [aygıt Management'i kullanmaya başlama] [ lnk-dm-getstarted] Öğreticisi, nasıl kullanılacağını gördüğünüz [cihaz çifti] [ lnk-devtwin] ve [doğrudan yöntemleri ] [ lnk-c2dmethod] temelleri uzaktan bir aygıt yeniden başlatma. Bu öğretici aynı IOT hub'ı temelleri kullanır ve bir uçtan uca sanal üretici yazılımı güncelleştirme yapmak nasıl gösterir.  Bu desen bellenim güncelleştirme uygulaması için kullanılan [Raspberry Pi'yi cihaz uygulaması örnek][lnk-rpi-implementation].

Bu öğretici şunların nasıl yapıldığını gösterir:

* Çağıran bir .NET konsol uygulaması oluşturma **firmwareUpdate** yöntemi, IOT hub'ı aracılığıyla sanal cihaz uygulamasında doğrudan.
* Arabirimini uygulayan bir sanal cihaz uygulaması oluşturma bir **firmwareUpdate** doğrudan yöntemi. Bu yöntem bellenim görüntüsünü karşıdan yüklemek için bekleyeceği, bellenim görüntüyü indirir ve son bellenim görüntü geçerli bir çok aşama işlemi başlatır. Güncelleştirme her aşaması sırasında aygıt ilerlemesini bildirmek için bildirilen özelliklerini kullanır.

Bu öğreticinin sonunda bir .NET (C#) konsol cihaz uygulaması ve bir .NET (C#) konsol arka uç uygulaması sahiptir:

* **SimulatedDeviceFwUpdate**, daha önce oluşturulan cihaz kimliğiyle IOT hub'ınızı bağlayan alan **firmwareUpdate** doğrudan yöntemi, bir ürün yazılımı güncelleştirmesi benzetimini yapmak için çok durumlu bir işlem aracılığıyla çalıştırır dahil: görüntü yükleme bekleniyor, yeni görüntüyü indirme ve son olarak görüntüyü uygulamadan.

* **TriggerFWUpdate**, uzaktan çağrılacak hizmeti SDK'sını kullanan **firmwareUpdate** doğrudan sanal cihaz yöntemi yanıtı görüntüler ve (500ms her) görüntüler güncelleştirilmiş rapor düzenli aralıklarla Özellikler.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2015 veya Visual Studio 2017.
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

İzleyin [aygıt Management'i kullanmaya başlama](iot-hub-csharp-csharp-device-management-get-started.md) IOT hub'ınızı oluşturun ve IOT Hub bağlantı dizenizi almak için makale.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Doğrudan bir yöntem kullanarak aygıt bir uzak bellenim güncelleştirme Tetikle
Bu bölümde, bir cihazda uzaktan yazılımı güncelleştirmesi başlatır (C# kullanarak) bir .NET konsol uygulaması oluşturun. Uygulama güncelleştirme başlatmak için doğrudan bir yöntem kullanır ve etkin bellenim güncelleştirme durumunu düzenli aralıklarla almak için cihaz çifti sorgularını kullanır.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Proje adı **TriggerFWUpdate**.

    ![Yeni Visual C# Windows Klasik Masaüstü projesi][img-createserviceapp]

2. Çözüm Gezgini'nde sağ **TriggerFWUpdate** proje ve ardından **NuGet paketlerini Yönet**.
3. **NuGet Paket Yöneticisi** penceresinde **Gözat**'ı seçin, **microsoft.azure.devices**'ı aratın, **Microsoft.Azure.Devices** paketini yüklemek için **Yükle**'yi seçin ve kullanım koşullarını kabul edin. Bu yordam ile [Azure IoT hizmet SDK'sı][lnk-nuget-service-sdk] NuGet paketi ve bağımlılıkları indirilir, yüklenir ve bu pakete bir başvuru eklenir.

    ![NuGet Paket Yöneticisi penceresi][img-servicenuget]
4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

    ```csharp   
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

5. **Program** sınıfına aşağıdaki alanları ekleyin. Birden çok yer tutucu değerlerini önceki bölümde ve Cihazınızı Kimliğini oluşturulan hub'ın IOT Hub bağlantı dizesiyle değiştirin.
   
    ```csharp   
    static RegistryManager registryManager;
    static string connString = "{iot hub connection string}";
    static ServiceClient client;
    static string targetDevice = "{deviceIdForTargetDevice}";
    ```
        
6. Aşağıdaki yöntemi ekleyin **Program** sınıfı. Bu yöntem, her 500 milisaniye güncelleştirilmiş durumu için cihaz çifti yoklar. Yalnızca durum gerçekte değiştiğinde konsola yazar. Aboneliğinize ek IOT hub'ı iletilerinde tüketen önlemek için bu örnek için yoklama durakları cihaz durumu bildirdiğinde **applyComplete** veya bir hata oluştu.  
   
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

8. Son olarak aşağıdaki satırları ekleyin **ana** yöntemi. Bu Yöneticisi bir çalışan iş parçacığı yoklama görevde cihaz çifti ile başlar okumak için bir kayıt defteri oluşturur ve bellenim güncelleştirmesini tetikler.
   
    ```csharp   
    registryManager = RegistryManager.CreateFromConnectionString(connString);

    Task queryTask = Task.Run(() => (QueryTwinFWUpdateReported(DateTime.Now)));

    StartFirmwareUpdate().Wait();
    Console.WriteLine("Press ENTER to exit.");
    Console.ReadLine();
    ```
        
9. Çözümü oluşturun.

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma
Bu bölümde şunları yapacaksınız:

* Bulut tarafından adlı doğrudan bir yöntem yanıtlaması bir .NET konsol uygulaması oluşturma
* Doğrudan bir yöntem bir arka uç hizmeti tarafından tetiklenen bir üretici yazılımı güncelleştirmesi benzetimi
* Cihaz ikizi sorgularının cihazları ve bir üretici yazılımı güncelleştirmesini en son ne zaman tamamladığını belirlemesi için rapor edilen özellikleri kullanın

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Proje adı **SimulateDeviceFWUpdate**.
   
    ![Yeni Visual C# Klasik Windows cihaz uygulaması][img-createdeviceapp]
    
2. Çözüm Gezgini'nde sağ **SimulateDeviceFWUpdate** proje ve ardından **NuGet paketlerini Yönet**.
3. İçinde **NuGet Paket Yöneticisi** penceresinde, seçin **Gözat** arayın ve **microsoft.azure.devices.client**. Seçin **yükleme** yüklemek için **Microsoft.Azure.Devices.Client** paketini ve kullanım koşullarını kabul edin. Bu yordam indirir, yükler ve bir başvuru ekler [Azure IOT cihaz SDK'sı] [ lnk-nuget-client-sdk] NuGet paketi ve bağımlılıklarını.
   
    ![NuGet Paket Yöneticisi penceresi istemci uygulaması][img-clientnuget]
4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
   
    ```csharp   
    using Newtonsoft.Json.Linq;
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    ```

5. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini de not ettiğiniz aygıt bağlantı dizesiyle değiştirin **bir cihaz kimliği oluşturma** bölümü.
   
    ```csharp   
    static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
    static DeviceClient Client = null;
    ```

6. Cihaz çifti aracılığıyla bulut dön durumu raporlamak için aşağıdaki yöntemi ekleyin: 

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

7. Bellenim görüntü indirme benzetimini yapmak için aşağıdaki yöntemi ekleyin:
        
    ```csharp   
    static async Task<byte[]> simulateDownloadImage(string imageUrl)
    {
        var image = "[fake image data]";

        Console.WriteLine("Downloading image from " + imageUrl);

        await Task.Delay(4000);

        return Encoding.ASCII.GetBytes(image);
            
    }
    ```

8. Bellenim görüntü cihaza uygulama benzetimini yapmak için aşağıdaki yöntemi ekleyin:
        
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
 
9.  Bellenim görüntüsünü karşıdan yüklemek için bekleyen benzetimini yapmak için aşağıdaki yöntemi ekleyin. Durumunu güncelleştirmek **bekleyen** ve diğer bellenim güncelleştirme özellikleri twin temizleyin. Bu özellikler, var olan tüm değerleri önceki Bellenim güncelleştirmeleri kaldırmak için temizlenir. Düzeltme eki işlemi (delta) ve değil PUT işlemi (tüm önceki değerleri değiştiren eksiksiz bir kümesini özellikleri) bildirilen özellikleri gönderildiğinden, bu gereklidir. Genellikle, cihazlar kullanılabilir güncelleştirmeden haberdar edilir ve yönetici tarafından tanımlanan bir ilke, cihazın güncelleştirmeyi indirip uygulamaya başlamasına neden olur. Bu işlev, ilgili ilkeyi etkinleştirme mantığının çalışması gereken yerdir. 
        
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

10. Karşıdan yükleme gerçekleştirmek için aşağıdaki yöntemi ekleyin. Durum güncelleştirmeleri **indirme** cihaz çifti benzetim indirme yöntemini çağırır ve durumunu rapor **downloadComplete** veya **downloadFailed** yükleme işleminin sonuçlarını bağlı olarak twin. 
        
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

11. Yansıma uygulamak için aşağıdaki yöntemi ekleyin. Durum güncelleştirmeleri **uygulama** cihaz çifti benzetim uygulamak çağrıları görüntü yöntemi ve güncelleştirme durumunu **applyComplete** veya **applyFailed** aracılığıyla uygulama işleminin sonuçları bağlı olarak çifti. 
        
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

12. Görüntü cihaza uygulama aracılığıyla görüntüsünü karşıdan yüklemek için beklemesini bellenim güncelleştirme işlemi sıralamak için aşağıdaki yöntemi ekleyin:
        
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

13. İşlemek için aşağıdaki yöntemi ekleyin **updateFirmware** doğrudan buluttan yöntemi. URL için bellenim güncelleştirme ileti yükü ayıklar ve buna ileten **doUpdate** başka bir iş parçacığı havuzu iş parçacığı üzerinde başlatır görev. Daha sonra hemen bulut yöntemi yanıtı döndürür.
        
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
> Bu yöntem olarak çalıştırmak için benzetimli güncelleştirme tetikleyen bir **görev** ve hizmet bellenim güncelleştirme başlatılmış olduğunu bildiren yöntemi çağrısına hemen yanıt verir. Güncelleştirme durumu ve tamamlama cihaz çifti bildirilen özelliklerini aracılığıyla hizmetine gönderilir. Güncelleştirme başlatırken yöntemi çağrısına yerine kendi tamamlandıktan sonra yanıt vermemiz nedeni:
> * Gerçek güncelleştirme işlemi yöntem çağrısı zaman aşımından daha uzun sürer olasılığı yüksektir.
> * Bu uygulama yapma yeniden başlatma bir yeniden başlatma gerektiren büyük olasılıkla gerçek güncelleştirme işlemidir **MetodRequest** nesnesi kullanılamıyor. (Bildirilen özelliklerini güncelleştirme ancak yeniden başlatmadan sonra bile mümkündür.) 

14. Son olarak, aşağıdaki kodu ekleyin **ana** IOT hub'ınıza bağlantıyı açın ve yöntemi dinleyicisini başlatmak için yöntem:
   
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
        
15. Çözümü oluşturun.       

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. [Geçici Hata İşleme][lnk-transient-faults] adlı MSDN makalesinde önerildiği üzere, üretim kodunda yeniden deneme ilkelerini (üstel geri alma gibi) uygulamanız gerekir.


## <a name="run-the-apps"></a>Uygulamaları çalıştırma
Şimdi uygulamaları çalıştırmaya hazırsınız.
1. .NET cihaz uygulama çalıştırma **SimulatedDeviceFWUpdate**.  Sağ **SimulatedDeviceFWUpdate** proje, select **hata ayıklama**ve ardından **başlangıç yeni örnek**. IOT Hub'ınızı gelen yöntem çağrıları için dinleme başlamanız gerekir: 

2. Cihaz bağlı ve .NET çalıştırmak için yöntem çağrılarına, bekleyen bir kez **TriggerFWUpdate** sanal cihaz uygulamasının bellenim update yöntemini çağırmak için uygulama. Sağ **TriggerFWUpdate** proje, select **hata ayıklama**ve ardından **başlangıç yeni örnek**. Görmeniz gerekir güncelleştirme yazılan dizisi **SimulatedDeviceFWUpdate** konsol ve ayrıca dizisi bildirilen aygıtı bildirilen özellikleri aracılığıyla **TriggerFWUpdate** konsol. İşlemin tamamlanması birkaç saniye sürer. 
   
    ![Çalıştırma hizmet ve cihaz uygulaması][img-combinedrun]


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, doğrudan bir yöntem bir cihazda uzaktan Bellenim güncelleştirmesini tetiklemek için bildirilen özellikleri bellenim güncelleştirme durumunu izlemek için kullanılır ve.

Çözüm ve zamanlama yöntemini çağıran birden fazla cihazda, IOT genişletmek öğrenmek için bkz: [zamanlama ve yayın işleri] [ lnk-tutorial-jobs] Öğreticisi.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-csharp-firmware-update/servicesdknuget.png
[img-clientnuget]: media/iot-hub-csharp-csharp-firmware-update/clientsdknuget.png
[img-createserviceapp]: media/iot-hub-csharp-csharp-firmware-update/createserviceapp.png
[img-createdeviceapp]: media/iot-hub-csharp-csharp-firmware-update/createdeviceapp.png
[img-combinedrun]: media/iot-hub-csharp-csharp-firmware-update/combinedrun.png

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-csharp-csharp-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-csharp-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-rpi-implementation]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt_dm/pi_device
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/