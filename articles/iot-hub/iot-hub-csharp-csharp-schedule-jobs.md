---
title: Azure IOT hub'ı (.NET/.NET) ile işleri zamanlama | Microsoft Docs
description: Birden fazla cihazda doğrudan yöntem çağırmak için bir Azure IOT hub'ı işini zamanlamak nasıl. Sanal cihaz uygulamaları ve işi çalıştırmak için bir hizmet uygulaması'nı uygulamak için Azure IOT cihaz SDK'sı .NET için kullanın.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/06/2018
ms.author: dobett
ms.openlocfilehash: beb1e1e166325cb41a5d4e4fa07565b1f3d4b3bb
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38666925"
---
# <a name="schedule-and-broadcast-jobs-netnet"></a>İşleri (.NET/.NET) zamanlama ve yayınlama

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Zamanlama ve milyonlarca cihaza güncelleştirme işleri izlemek için Azure IOT Hub'ı kullanın. İşleri kullanın:

* İstenen özellikleri güncelleştirme
* Etiketleri güncelleştirin
* Doğrudan metotları çağırma

Bir iş, aşağıdaki eylemlerden birini sarmalar ve bir cihaz ikizi sorgu tarafından tanımlanan bir dizi cihazda yürütmeyi izler. Örneğin, bir arka uç uygulaması, bir iş cihazları yeniden bir doğrudan yöntem 10.000 cihaz üzerinde çağırmak için kullanabilirsiniz. Bir dizi cihazda cihaz ikizi sorgusu ile belirtin ve gelecekteki bir tarihte çalışmaya planlayın. CİHAZDAN her biri olarak ilerleyişini izler, alma ve yeniden başlatma doğrudan yöntem yürütün.

Bu özelliklerin her biri hakkında daha fazla bilgi için bkz:

* Cihaz ikizi ve özellikleri: [cihaz ikizlerini kullanmaya başlama] [ lnk-get-started-twin] ve [öğretici: cihaz ikizi özelliklerini kullanma][lnk-twin-props]
* Doğrudan yöntemler: [IOT Hub Geliştirici Kılavuzu - doğrudan yöntemler] [ lnk-dev-methods] ve [Öğreticisi: doğrudan yöntemler kullanma][lnk-c2d-methods]

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* Çağrılan doğrudan bir yönteme uygulayan bir cihaz uygulaması oluşturma **LockDoor** arka uç uygulama tarafından çağrılabilir.
* Çağırmak için bir iş oluşturur bir arka uç uygulaması oluşturma **LockDoor** birden fazla cihazda doğrudan yöntem. Başka bir iş birden çok cihaz için istenen özellik güncelleştirmeleri gönderir.

Bu öğreticinin sonunda, iki .NET (C#) konsol uygulamanız olacak:

**SimulateDeviceMethods** uygular ve IOT hub'a bağlanan **LockDoor** doğrudan yöntemi.

**ScheduleJob** çağırmak için işleri kullanan **LockDoor** doğrudan yöntemini ve birden fazla cihazda cihaz çiftinin istenen özelliklerini güncelleştirin.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2015 veya Visual Studio 2017.
* Etkin bir Azure hesabı. Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]


## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma
Bu bölümde, çözüm tarafından arka uç olarak adlandırılan doğrudan bir yönteme yanıt veren bir .NET konsol uygulaması oluşturun.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Projeyi adlandırın **SimulateDeviceMethods**.
   
    ![Yeni Windows Visual C# Klasik cihaz uygulaması][img-createdeviceapp]
    
1. Çözüm Gezgini'nde sağ **SimulateDeviceMethods** proje ve ardından **NuGet paketlerini Yönet...** .

1. İçinde **NuGet Paket Yöneticisi** penceresinde **Gözat** araması **microsoft.azure.devices.client**. Seçin **yükleme** yüklemek için **Microsoft.Azure.Devices.Client** paketini ve kullanım koşullarını kabul edin. Bu yordam indirir, yükler ve bir başvuru ekler [Azure IOT cihaz SDK'sını] [ lnk-nuget-client-sdk] NuGet paketi ve bağımlılıkları.
   
    ![NuGet Paket Yöneticisi penceresi istemci uygulaması][img-clientnuget]

1. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
   
    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;

    using Newtonsoft.Json;
    ```

1. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini önceki bölümde not ettiğiniz cihaz bağlantı dizesiyle değiştirin:

    ```csharp
    static string DeviceConnectionString = "<yourDeviceConnectionString>";
    static DeviceClient Client = null;
    ```

1. Bir cihazda doğrudan yöntem uygulamak için aşağıdakileri ekleyin:

    ```csharp
    static Task<MethodResponse> LockDoor(MethodRequest methodRequest, object userContext)
    {
        Console.WriteLine();
        Console.WriteLine("Locking Door!");
        Console.WriteLine("\nReturning response for method {0}", methodRequest.Name);
            
        string result = "'Door was locked.'";
        return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
    }
    ```

1. Cihaz ikizlerini dinleyici cihazda uygulamak için aşağıdakileri ekleyin:

    ```csharp
    private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, object userContext)
    {
        Console.WriteLine("Desired property change:");
        Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));
    }
    ```

1. Son olarak, aşağıdaki kodu ekleyin **ana** yöntemi, IOT hub'ınıza bağlantıyı açın ve yöntemi dinleyicisi başlatılamıyor:
   
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
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda yeniden deneme ilkelerini (örneğin, bağlantı yeniden deneme), MSDN makalesinde önerildiği uygulamalıdır [geçici hata işleme][lnk-transient-faults].
> 


## <a name="schedule-jobs-for-calling-a-direct-method-and-sending-device-twin-updates"></a>Bir doğrudan yöntem çağırma ve cihaz ikizi güncelleştirmeleri göndermek için işleri zamanlama

Bu bölümde, çağırmak için işleri kullanır (C# kullanarak) bir .NET konsol uygulaması oluşturma **LockDoor** doğrudan yöntemini ve birden çok cihaz için istenen özellik güncelleştirmeleri gönderir.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Projeyi adlandırın **ScheduleJob**.

    ![Yeni Visual C# Windows Klasik Masaüstü projesi][img-createapp]

1. Çözüm Gezgini'nde sağ **ScheduleJob** proje ve ardından **NuGet paketlerini Yönet...** .

1. **NuGet Paket Yöneticisi** penceresinde **Gözat**'ı seçin, **microsoft.azure.devices**'ı aratın, **Microsoft.Azure.Devices** paketini yüklemek için **Yükle**'yi seçin ve kullanım koşullarını kabul edin. Bu adım indirir, yükler ve bir başvuru ekler [Azure IOT hizmeti SDK'sını] [ lnk-nuget-service-sdk] NuGet paketi ve bağımlılıkları.

    ![NuGet Paket Yöneticisi penceresi][img-servicenuget]

1. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
    
    ```csharp
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

1. Aşağıdaki `using` deyimi yoksa varsayılan deyimlerinde.

    ```csharp
    using System.Threading;
    using System.Threading.Tasks;
    ```

1. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucuları, cihaz adını ve önceki bölümde oluşturduğunuz hub'ın IOT Hub bağlantı dizesiyle değiştirin.

    ```csharp
    static JobClient jobClient;
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

1. Başka bir yöntemi ekleyin **Program** sınıfı:

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
    > Sorgu söz dizimi hakkında daha fazla bilgi için bkz: [IOT Hub sorgu dili][lnk-query].
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

1. Visual Studio Çözüm Gezgini'nde Çözümünüze sağ tıklayın ve ardından **yapı**. **Birden fazla başlangıç projesi**. Emin `SimulateDeviceMethods` ardından listenin en `ScheduleJob`. Her iki eylemlerini kümesine **Başlat** tıklatıp **Tamam**.

1. Projeleri tıklayarak çalıştırın **Başlat** veya Git **hata ayıklama** menüsüne ve ardından **hata ayıklamayı Başlat**.

1. Hem cihaz hem de arka uç uygulamaları çıktısını görürsünüz.

    ![İşleri zamanlamak için uygulamaları çalıştırma][img-schedulejobs]


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir işi bir doğrudan yöntem bir cihaz ve cihaz ikizinin özelliklerini güncelleştirme için zamanlamak için kullanılır.

IOT Hub ve cihaz yönetim modellerini uzaktan gibi hava üretici yazılımı güncelleştirme kullanmaya başlama devam etmek için okuma [öğretici: bir üretici yazılımı güncelleştirme yapmak nasıl][lnk-fwupdate].

Yapay ZEKA ile Azure IOT Edge uç cihazlarına dağıtma hakkında bilgi edinmek için [IOT Edge'i kullanmaya başlama][lnk-iot-edge].

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
[lnk-query]: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-query-language
