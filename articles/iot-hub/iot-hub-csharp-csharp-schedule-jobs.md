---
title: "Zamanlama işlerini ile Azure IOT hub'ı (.NET/.NET) | Microsoft Docs"
description: "Birden çok aygıta doğrudan bir yöntemi çağırmak için bir Azure IOT Hub işini zamanlamak nasıl. Sanal cihaz uygulamaları ve işi çalıştırmak için bir hizmet uygulaması uygulamak için Azure IOT cihaz SDK'sı .NET için kullanın."
services: iot-hub
documentationcenter: .net
author: msebolt
manager: timlt
editor: 
ms.assetid: 2233356e-b005-4765-ae41-3a4872bda943
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 012/16/2018
ms.author: v-masebo
ms.openlocfilehash: 0cdc23683489e103b061044856d68a8b014d3535
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="schedule-and-broadcast-jobs-netnet"></a>Zamanlama ve yayın işleri (.NET/.NET)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Zamanlama ve milyonlarca cihaza güncelleştirme işleri izlemek için Azure IOT hub'ı kullanın. İşlerini kullanın:

* İstenen özellikleri güncelleştirme
* Güncelleştirme etiketleri
* Doğrudan yöntemleri çağırma

Bir işi şu eylemlerden birini sarmalar ve yürütme cihaz çifti sorgu tarafından tanımlanan bir dizi aygıtları izler. Örneğin, bir arka uç uygulaması, aygıtların yeniden 10.000 cihazlarda doğrudan yöntemini çağırmak için bir iş kullanabilirsiniz. Cihaz kümesini içeren bir cihaz çifti sorgu belirtin ve gelecek bir zamanda çalıştırmak için iş zamanlama. Her aygıt olarak ilerleyişini izler almak ve yeniden başlatma doğrudan yöntemi yürütün.

Bu özelliklerin her biri hakkında daha fazla bilgi için bkz:

* Cihaz çifti ve özellikleri: [cihaz çiftlerini ile çalışmaya başlama] [ lnk-get-started-twin] ve [öğretici: cihaz çifti özellikleri kullanma][lnk-twin-props]
* Doğrudan yöntemleri: [IOT Hub Geliştirici Kılavuzu - doğrudan yöntemleri] [ lnk-dev-methods] ve [Öğreticisi: doğrudan yöntemleri kullanın][lnk-c2d-methods]

Bu öğretici şunların nasıl yapıldığını gösterir:

* Adlı doğrudan bir yöntem uygulayan bir cihaz uygulaması oluşturma **LockDoor** arka uç uygulama tarafından çağrılabilir.
* Aranacak bir işi oluşturan bir arka uç uygulaması oluşturma **LockDoor** birden fazla cihazda yöntemi doğrudan. Başka bir iş birden çok aygıta istenen özelliği güncelleştirmeleri gönderir.

Bu öğreticinin sonunda iki .NET (C#) konsol uygulamaları vardır:

**SimulateDeviceMethods** uygular ve IOT hub'a bağlanan **LockDoor** doğrudan yöntemi.

**ScheduleJob** çağırmak için işleri kullanan **LockDoor** doğrudan yöntemi ve birden çok aygıta cihaz çifti istenen özelliklerini güncelleştirir.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2015 veya Visual Studio 2017.
* Etkin bir Azure hesabı. Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]


## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma
Bu bölümde, çözüm tarafından arka uç adlı doğrudan bir yönteme yanıt veren bir .NET konsol uygulaması oluşturun.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Proje adı **SimulateDeviceMethods**.
   
    ![Yeni Visual C# Klasik Windows cihaz uygulaması][img-createdeviceapp]
    
1. Çözüm Gezgini'nde sağ **SimulateDeviceMethods** proje ve ardından **NuGet paketlerini Yönet...** .

1. İçinde **NuGet Paket Yöneticisi** penceresinde, seçin **Gözat** arayın ve **microsoft.azure.devices.client**. Seçin **yükleme** yüklemek için **Microsoft.Azure.Devices.Client** paketini ve kullanım koşullarını kabul edin. Bu yordam indirir, yükler ve bir başvuru ekler [Azure IOT cihaz SDK'sı] [ lnk-nuget-client-sdk] NuGet paketi ve bağımlılıklarını.
   
    ![NuGet Paket Yöneticisi penceresi istemci uygulaması][img-clientnuget]

1. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
   
    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;

    using Newtonsoft.Json;
    ```

1. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini önceki bölümünde belirtildiği cihaz bağlantı dizesiyle değiştirin:

    ```csharp
    static string DeviceConnectionString = "<yourDeviceConnectionString>";
    static DeviceClient Client = null;

1. Add the following to implement the direct method on the device:

    ```csharp
    static Task<MethodResponse> LockDoor(MethodRequest methodRequest, object userContext)
    {
        Console.WriteLine();
        Console.WriteLine("Locking Door!");
        Console.WriteLine("\nReturning response for method {0}", methodRequest.Name);
            
        string result = "'Door was locked.'";
        return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
    }

1. Add the following to implement the device twins listener on the device:

    ```csharp
    private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, object userContext)
    {
        Console.WriteLine("Desired property change:");
        Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));
    }
    ```

1. Son olarak, aşağıdaki kodu ekleyin **ana** IOT hub'ınıza bağlantıyı açın ve yöntemi dinleyicisini başlatmak için yöntem:
   
    ```csharp
    try
    {
        Console.WriteLine("Connecting to hub");
        Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);

        Client.SetMethodHandlerAsync("LockDoor", LockDoor, null);
        Client.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertyChanged, null);

        Console.WriteLine("Waiting for direct method call and device twin update\n Press enter to exit.");
        Console.ReadLine();

        Console.WriteLine("Exiting...");

        Client.SetMethodHandlerAsync("LockDoor", null, null);
        Client.CloseAsync().Wait();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```
        
1. Çalışmanızı kaydedin ve çözümünüzü oluşturun.         

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda yeniden deneme ilkelerini (bağlantı yeniden deneme), önerilen MSDN makalesinde uygulamalıdır [geçici hata işleme][lnk-transient-faults].
> 


## <a name="schedule-jobs-for-calling-a-direct-method-and-sending-device-twin-updates"></a>Doğrudan bir yöntem çağırma ve cihaz çifti güncelleştirmeleri göndermek için zamanlama işleri

Bu bölümde, işleri çağırmak için kullanır (C# kullanarak) bir .NET konsol uygulaması oluşturma **LockDoor** doğrudan yöntemi ve istenen özelliği güncelleştirmeleri birden fazla cihaza Gönder.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Proje adı **ScheduleJob**.

    ![Yeni Visual C# Windows Klasik Masaüstü projesi][img-createapp]

1. Çözüm Gezgini'nde sağ **ScheduleJob** proje ve ardından **NuGet paketlerini Yönet...** .

1. **NuGet Paket Yöneticisi** penceresinde **Gözat**'ı seçin, **microsoft.azure.devices**'ı aratın, **Microsoft.Azure.Devices** paketini yüklemek için **Yükle**'yi seçin ve kullanım koşullarını kabul edin. Bu adım indirir, yükler ve bir başvuru ekler [Azure IOT hizmeti SDK'sını] [ lnk-nuget-service-sdk] NuGet paketi ve bağımlılıklarını.

    ![NuGet Paket Yöneticisi penceresi][img-servicenuget]

1. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
    
    ```csharp
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

1. Aşağıdakileri ekleyin `using` deyimi yoksa varsayılan deyimlerinde.

    ```csharp
    using System.Threading;
    using System.Threading.Tasks;
    ```

1. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucuları, önceki bölümde ve Cihazınızı adını oluşturulan hub'ın IOT Hub bağlantı dizesiyle değiştirin.

    ```csharp
    static string connString = "<yourIotHubConnectionString>";
    static string deviceId = "<yourDeviceId>";
    ```

1. **Program** sınıfına aşağıdaki yöntemi ekleyin:

    ```csharp
    public static async Task MonitorJob(string jobId)
    {
        JobResponse result;
        do
        {
            result = await jobClient.GetJobAsync(jobId);
            Console.WriteLine("Job Status : " + result.Status.ToString());
            Thread.Sleep(2000);
        } while ((result.Status != JobStatus.Completed) && (result.Status != JobStatus.Failed));
    }
    ```

1. **Program** sınıfına aşağıdaki yöntemi ekleyin:

    ```csharp
    public static async Task StartMethodJob(string jobId)
    {
        CloudToDeviceMethod directMethod = new CloudToDeviceMethod("LockDoor", TimeSpan.FromSeconds(5), TimeSpan.FromSeconds(5));
       
        JobResponse result = await jobClient.ScheduleDeviceMethodAsync(jobId,
            $"DeviceId IN ['{deviceId}']",
            directMethod,
            DateTime.UtcNow,
            (long)TimeSpan.FromMinutes(2).TotalSeconds);

        Console.WriteLine("Started Method Job");
    }
    ```

1. Başka bir yöntem ekleyin **Program** sınıfı:

    ```csharp
    public static async Task StartTwinUpdateJob(string jobId)
    {
        Twin twin = new Twin(deviceId);
        twin.Tags = new TwinCollection();
        twin.Tags["Building"] = "43";
        twin.Tags["Floor"] = "3";
        twin.ETag = "*";

        twin.Properties.Desired["LocationUpdate"] = DateTime.UtcNow;

        JobResponse createJobResponse = jobClient.ScheduleTwinUpdateAsync(
            jobId,
            $"DeviceId IN ['{deviceId}']", 
            twin, 
            DateTime.UtcNow, 
            (long)TimeSpan.FromMinutes(2).TotalSeconds).Result;

        Console.WriteLine("Started Twin Update Job");
    }
    ```

    > [!NOTE]
    > Sorgu sözdizimi hakkında daha fazla bilgi için bkz: [IOT hub'ı sorgu dili][lnk-query].
    > 

1. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:

    ```csharp
    Console.WriteLine("Press ENTER to start running jobs.");
    Console.ReadLine();

    jobClient = JobClient.CreateFromConnectionString(connString);

    string methodJobId = Guid.NewGuid().ToString();

    StartMethodJob(methodJobId);
    MonitorJob(methodJobId).Wait();
    Console.WriteLine("Press ENTER to run the next job.");
    Console.ReadLine();

    string twinUpdateJobId = Guid.NewGuid().ToString();

    StartTwinUpdateJob(twinUpdateJobId);
    MonitorJob(twinUpdateJobId).Wait();
    Console.WriteLine("Press ENTER to exit.");
    Console.ReadLine();
    ```

1. Çalışmanızı kaydedin ve çözümünüzü oluşturun. 


## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Visual Studio Çözüm Gezgini'nde Çözümünüze sağ tıklayın ve ardından **yapı**. **Birden fazla başlangıç projesi**. Emin olun `SimulateDeviceMethods` arkasından listenin en `ScheduleJob`. Her iki eylemlerini kümesine **Başlat** tıklatıp **Tamam**.

1. Tıklayarak projelerini çalıştırmak **Başlat** veya gitmek **hata ayıklama** menüsüne ve ardından **hata ayıklamayı Başlat**.

1. Cihaz ve arka uç uygulamaları çıktısını görürsünüz.

    ![İşlerini zamanlamak için uygulamaları çalıştırma][img-schedulejobs]


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir cihaz ve cihaz çifti'nın özelliklerini güncelleştirmek için doğrudan bir yöntem zamanlamak için bir iş kullanılır.

IOT Hub ve Uzaktan gibi cihaz yönetim düzenleri hava bellenim güncelleştirme kullanmaya Başlarken devam etmek için okuma [öğretici: bir üretici yazılımı güncelleştirmesi nasıl][lnk-fwupdate].

AI sınır cihazları için Azure IOT Edge dağıtma hakkında bilgi edinmek için bkz: [IOT Edge ile çalışmaya başlama][lnk-iot-edge].

<!-- images -->
[img-createdeviceapp]: ./media/iot-hub-csharp-csharp-schedule-jobs/create-device-app.png
[img-clientnuget]: ./media/iot-hub-csharp-csharp-schedule-jobs/device-app-nuget.png
[img-servicenuget]: media/iot-hub-csharp-csharp-schedule-jobs/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-csharp-schedule-jobs/createnetapp.png
[img-schedulejobs]: media/iot-hub-csharp-csharp-schedule-jobs/schedulejobs.png

[lnk-get-started-twin]: iot-hub-csharp-csharp-twin-getstarted.md
[lnk-twin-props]: iot-hub-csharp-csharp-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-csharp-csharp-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-csharp-csharp-firmware-update.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://docs.microsoft.com/azure/architecture/best-practices/transient-faults
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-query]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-query-language
