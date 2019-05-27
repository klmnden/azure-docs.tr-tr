---
title: Azure IOT hub'ı (.NET/.NET) ile işleri zamanlama | Microsoft Docs
description: Birden fazla cihazda doğrudan yöntem çağırmak için bir Azure IOT hub'ı işini zamanlamak nasıl. Sanal cihaz uygulamaları ve işi çalıştırmak için bir hizmet uygulaması'nı uygulamak için Azure IOT cihaz SDK'sı .NET için kullanın.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/06/2018
ms.author: robinsh
ms.openlocfilehash: f21f1eed6babee52f30c6eccc79f88dc7bee5d58
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65864474"
---
# <a name="schedule-and-broadcast-jobs-netnet"></a>İşleri (.NET/.NET) zamanlama ve yayınlama

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Zamanlama ve milyonlarca cihaza güncelleştirme işleri izlemek için Azure IOT Hub'ı kullanın. İşleri kullanın:

* İstenen özellikleri güncelleştirme
* Etiketleri güncelleştirin
* Doğrudan metotları çağırma

Bir iş, aşağıdaki eylemlerden birini sarmalar ve bir cihaz ikizi sorgu tarafından tanımlanan bir dizi cihazda yürütmeyi izler. Örneğin, bir arka uç uygulaması, bir iş cihazları yeniden bir doğrudan yöntem 10.000 cihaz üzerinde çağırmak için kullanabilirsiniz. Bir dizi cihazda cihaz ikizi sorgusu ile belirtin ve gelecekteki bir tarihte çalışmaya planlayın. CİHAZDAN her biri olarak ilerleyişini izler, alma ve yeniden başlatma doğrudan yöntem yürütün.

Bu özelliklerin her biri hakkında daha fazla bilgi için bkz:

* Cihaz ikizi ve özellikleri: [Cihaz ikizlerini kullanmaya başlama](iot-hub-csharp-csharp-twin-getstarted.md) ve [Öğreticisi: Cihaz ikizi özelliklerini kullanma](tutorial-device-twins.md)

* Doğrudan yöntemler: [IOT Hub Geliştirici Kılavuzu - doğrudan yöntemler](iot-hub-devguide-direct-methods.md) ve [Öğreticisi: Doğrudan yöntemler kullanma](quickstart-control-device-dotnet.md)

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* Çağrılan doğrudan bir yönteme uygulayan bir cihaz uygulaması oluşturma **LockDoor** arka uç uygulama tarafından çağrılabilir.

* Çağırmak için bir iş oluşturur bir arka uç uygulaması oluşturma **LockDoor** birden fazla cihazda doğrudan yöntem. Başka bir iş birden çok cihaz için istenen özellik güncelleştirmeleri gönderir.

Bu öğreticinin sonunda, iki .NET (C#) konsol uygulamanız olacak:

**SimulateDeviceMethods** uygular ve IOT hub'a bağlanan **LockDoor** doğrudan yöntemi.

**ScheduleJob** çağırmak için işleri kullanan **LockDoor** doğrudan yöntemini ve birden fazla cihazda cihaz çiftinin istenen özelliklerini güncelleştirin.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio.
* Etkin bir Azure hesabı. Bir hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>IOT hub için bağlantı dizesi alma

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Yeni bir cihaz IOT hub'ı Kaydet

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma

Bu bölümde, çözüm tarafından arka uç olarak adlandırılan doğrudan bir yönteme yanıt veren bir .NET konsol uygulaması oluşturun.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Projeyi adlandırın **SimulateDeviceMethods**.
   
    ![Yeni Windows Visual C# Klasik cihaz uygulaması](./media/iot-hub-csharp-csharp-schedule-jobs/create-device-app.png)
    
2. Çözüm Gezgini'nde sağ **SimulateDeviceMethods** proje ve ardından **NuGet paketlerini Yönet...** .

3. İçinde **NuGet Paket Yöneticisi** penceresinde **Gözat** araması **Microsoft.Azure.Devices.Client**. Seçin **yükleme** yüklemek için **Microsoft.Azure.Devices.Client** paketini ve kullanım koşullarını kabul edin. Bu yordam indirir, yükler ve bir başvuru ekler [Azure IOT cihaz SDK'sını](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/) NuGet paketi ve bağımlılıkları.
   
    ![NuGet Paket Yöneticisi penceresi istemci uygulaması](./media/iot-hub-csharp-csharp-schedule-jobs/device-app-nuget.png)

4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
   
    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using Newtonsoft.Json;
    ```

5. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini önceki bölümde not ettiğiniz cihaz bağlantı dizesiyle değiştirin:

    ```csharp
    static string DeviceConnectionString = "<yourDeviceConnectionString>";
    static DeviceClient Client = null;
    ```

6. Bir cihazda doğrudan yöntem uygulamak için aşağıdakileri ekleyin:

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

7. Cihaz ikizlerini dinleyici cihazda uygulamak için aşağıdakileri ekleyin:

    ```csharp
    private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, 
      object userContext)
    {
        Console.WriteLine("Desired property change:");
        Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));
    }
    ```

8. Son olarak, aşağıdaki kodu ekleyin **ana** yöntemi, IOT hub'ınıza bağlantıyı açın ve yöntemi dinleyicisi başlatılamıyor:
   
    ```csharp
    try
    {
        Console.WriteLine("Connecting to hub");
        Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, 
          TransportType.Mqtt);

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
        
9. Çalışmanızı kaydedin ve çözümünüzü oluşturun.         

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda yeniden deneme ilkelerini (örneğin, bağlantı yeniden deneme), makalesinde önerildiği uygulamalıdır [geçici hata işleme](/azure/architecture/best-practices/transient-faults).
> 

## <a name="schedule-jobs-for-calling-a-direct-method-and-sending-device-twin-updates"></a>Bir doğrudan yöntem çağırma ve cihaz ikizi güncelleştirmeleri göndermek için işleri zamanlama

Bu bölümde, çağırmak için işleri kullanır (C# kullanarak) bir .NET konsol uygulaması oluşturma **LockDoor** doğrudan yöntemini ve birden çok cihaz için istenen özellik güncelleştirmeleri gönderir.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Projeyi adlandırın **ScheduleJob**.

    ![Yeni Visual C# Windows Klasik Masaüstü projesi](./media/iot-hub-csharp-csharp-schedule-jobs/createnetapp.png)

2. Çözüm Gezgini'nde sağ **ScheduleJob** proje ve ardından **NuGet paketlerini Yönet...** .

3. İçinde **NuGet Paket Yöneticisi** penceresinde **Gözat**, arama **Microsoft.Azure.Devices**seçin **yükleme** yüklemek için **Microsoft.Azure.Devices** paketini ve kullanım koşullarını kabul edin. Bu adım indirir, yükler ve bir başvuru ekler [Azure IOT hizmeti SDK'sını](https://www.nuget.org/packages/Microsoft.Azure.Devices/) NuGet paketi ve bağımlılıkları.

    ![NuGet Paket Yöneticisi penceresi](./media/iot-hub-csharp-csharp-schedule-jobs/servicesdknuget.png)

4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
    
    ```csharp
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

5. Aşağıdaki `using` deyimi yoksa varsayılan deyimlerinde.

    ```csharp
    using System.Threading;
    using System.Threading.Tasks;
    ```

6. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucuları, cihaz adını ve önceki bölümde oluşturduğunuz hub'ın IOT Hub bağlantı dizesiyle değiştirin.

    ```csharp
    static JobClient jobClient;
    static string connString = "<yourIotHubConnectionString>";
    static string deviceId = "<yourDeviceId>";
    ```

7. **Program** sınıfına aşağıdaki yöntemi ekleyin:

    ```csharp
    public static async Task MonitorJob(string jobId)
    {
        JobResponse result;
        do
        {
            result = await jobClient.GetJobAsync(jobId);
            Console.WriteLine("Job Status : " + result.Status.ToString());
            Thread.Sleep(2000);
        } while ((result.Status != JobStatus.Completed) && 
          (result.Status != JobStatus.Failed));
    }
    ```

8. **Program** sınıfına aşağıdaki yöntemi ekleyin:

    ```csharp
    public static async Task StartMethodJob(string jobId)
    {
        CloudToDeviceMethod directMethod = 
          new CloudToDeviceMethod("LockDoor", TimeSpan.FromSeconds(5), 
          TimeSpan.FromSeconds(5));
       
        JobResponse result = await jobClient.ScheduleDeviceMethodAsync(jobId,
            $"DeviceId IN ['{deviceId}']",
            directMethod,
            DateTime.UtcNow,
            (long)TimeSpan.FromMinutes(2).TotalSeconds);

        Console.WriteLine("Started Method Job");
    }
    ```

9. Başka bir yöntemi ekleyin **Program** sınıfı:

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
    > Sorgu söz dizimi hakkında daha fazla bilgi için bkz: [IOT Hub sorgu dili](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-query-language).
    > 

10. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:

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

11. Çalışmanızı kaydedin ve çözümünüzü oluşturun. 

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Visual Studio Çözüm Gezgini'nde Çözümünüze sağ tıklayın ve ardından **yapı**. **Birden fazla başlangıç projesi**. Emin `SimulateDeviceMethods` ardından listenin en `ScheduleJob`. Her iki eylemlerini kümesine **Başlat** tıklatıp **Tamam**.

2. Projeleri tıklayarak çalıştırın **Başlat** veya Git **hata ayıklama** menüsüne ve ardından **hata ayıklamayı Başlat**.

3. Hem cihaz hem de arka uç uygulamaları çıktısını görürsünüz.

    ![İşleri zamanlamak için uygulamaları çalıştırma](./media/iot-hub-csharp-csharp-schedule-jobs/schedulejobs.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir işi bir doğrudan yöntem bir cihaz ve cihaz ikizinin özelliklerini güncelleştirme için zamanlamak için kullanılır.

IOT Hub ve cihaz yönetim modellerini uzaktan gibi hava üretici yazılımı güncelleştirme kullanmaya başlama devam etmek için okuma [Öğreticisi: Üretici yazılımlarını güncelleştirme nasıl](tutorial-firmware-update.md).

Yapay ZEKA ile Azure IOT Edge uç cihazlarına dağıtma hakkında bilgi edinmek için [IOT Edge'i kullanmaya başlama](../iot-edge/tutorial-simulate-device-linux.md).