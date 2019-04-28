---
title: Azure IOT Hub cihaz Yönetimi (.NET/.NET) ile çalışmaya başlama | Microsoft Docs
description: Uzak cihazı yeniden başlatmak için Azure IOT Hub cihaz Yönetimi'ni kullanma Bir doğrudan yöntem ve Azure IOT hizmeti SDK'sını doğrudan yöntemini çağıran bir hizmet uygulaması uygulamak .NET için içeren bir sanal cihaz uygulaması uygulamak için Azure IOT cihaz SDK'sı .NET için kullanın.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 09/15/2017
ms.author: robinsh
ms.openlocfilehash: 556c10cc5ec5e528857a120dadb16c2a10202ed3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61250992"
---
# <a name="get-started-with-device-management-netnet"></a>Cihaz Yönetimi (.NET/.NET) ile çalışmaya başlama

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* IOT Hub oluşturma ve IOT hub'ınızda bir cihaz kimliği oluşturmak için Azure portalını kullanın.

* Bu cihazı yeniden başlatır bir doğrudan yöntem içeren bir sanal cihaz uygulaması oluşturma. Doğrudan yöntemler buluttan çağrılır.

* IOT hub'ınız aracılığıyla sanal cihaz uygulaması, yeniden başlatma doğrudan yöntemi çağıran bir .NET konsol uygulaması oluşturacaksınız.

Bu öğreticinin sonunda iki .NET konsol uygulamanız olacak:

* **SimulateManagedDevice**, daha önce oluşturulan cihaz kimliğiyle IOT hub'ınızı bağlayan bir yeniden başlatma doğrudan yöntem alıp fiziksel sistemin yeniden başlatılması benzetimini yapar ve zamanı son yeniden başlatma için raporlar.

* **TriggerReboot**bir doğrudan yöntem sanal cihaz uygulamasında çağıran yanıt görüntüler ve görüntüler güncelleştirilmiş bildirilen özellikler.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2017.

* Etkin bir Azure hesabı. (Hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.)

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>IOT hub için bağlantı dizesi alma

[!INCLUDE [iot-hub-find-include-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Yeni bir cihaz IOT hub'ı Kaydet

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Bir doğrudan yöntem kullanarak cihaz üzerinde Uzaktan yeniden başlatma tetikleyin

Bu bölümde, bir cihazda doğrudan yöntem kullanarak uzaktan yeniden başlatma başlatır (C# kullanarak) bir .NET konsol uygulaması oluşturun. Uygulama, son yeniden başlatma zamanı bu cihaz için keşfetmek için cihaz ikizi sorgularını kullanır.

1. Visual Studio’da **Konsol Uygulaması (.NET Framework)** proje şablonunu kullanarak yeni bir çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. .NET Framework sürümünün 4.5.1 veya sonraki bir sürüm olduğundan emin olun. Projeyi adlandırın **TriggerReboot**.

    ![Yeni Visual C# Windows Klasik Masaüstü projesi](./media/iot-hub-csharp-csharp-device-management-get-started/createserviceapp.png)

2. Çözüm Gezgini'nde sağ **TriggerReboot** proje ve ardından **NuGet paketlerini Yönet**.

3. İçinde **NuGet Paket Yöneticisi** penceresinde **Gözat**, arama **Microsoft.Azure.Devices**seçin **yükleme** yüklemek için **Microsoft.Azure.Devices** paketini ve kullanım koşullarını kabul edin. Bu yordam indirir, yükler ve bir başvuru ekler [Azure IOT hizmeti SDK'sını](https://www.nuget.org/packages/Microsoft.Azure.Devices/) NuGet paketi ve bağımlılıkları.

    ![NuGet Paket Yöneticisi penceresi](./media/iot-hub-csharp-csharp-device-management-get-started/servicesdknuget.png)

4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
   
   ```csharp
   using Microsoft.Azure.Devices;
   using Microsoft.Azure.Devices.Shared;
   ```
        
5. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini "IOT hub oluşturma." bölümünde oluşturduğunuz hub'ın IOT Hub bağlantı dizesiyle değiştirin 
   
   ```csharp
   static RegistryManager registryManager;
   static string connString = "{iot hub connection string}";
   static ServiceClient client;
   static string targetDevice = "myDeviceId";
   ```

6. **Program** sınıfına aşağıdaki yöntemi ekleyin.  Bu kod, cihaz yeniden başlatıldığı için cihaz ikizi alır ve bildirilen özellikleri çıkarır.
   
   ```csharp
   public static async Task QueryTwinRebootReported()
   {
       Twin twin = await registryManager.GetTwinAsync(targetDevice);
       Console.WriteLine(twin.Properties.Reported.ToJson());
   }
   ```
        
7. **Program** sınıfına aşağıdaki yöntemi ekleyin.  Bu kod, bir doğrudan yöntem kullanarak cihazın yeniden başlatır.

   ```csharp
   public static async Task StartReboot()
   {
       client = ServiceClient.CreateFromConnectionString(connString);
       CloudToDeviceMethod method = new CloudToDeviceMethod("reboot");
       method.ResponseTimeout = TimeSpan.FromSeconds(30);

       CloudToDeviceMethodResult result = await 
         client.InvokeDeviceMethodAsync(targetDevice, method);

       Console.WriteLine("Invoked firmware update on device.");
   }
   ```

7. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:
   
   ```csharp
   registryManager = RegistryManager.CreateFromConnectionString(connString);
   StartReboot().Wait();
   QueryTwinRebootReported().Wait();
   Console.WriteLine("Press ENTER to exit.");
   Console.ReadLine();
   ```

8. Çözümü derleyin.

> [!NOTE]
> Bu öğretici için bildirilen özellikler yalnızca tek bir sorgu gerçekleştirir. Üretim kodunda bildirilen özellikler değişikliklerini algılamak için yoklama öneririz.

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma

Bu bölümde şunları gerçekleştireceksiniz:

* Bulut tarafından çağrılan doğrudan bir yönteme yanıt veren bir .NET konsol uygulaması oluşturacaksınız.

* Sanal cihazı yeniden başlatma tetikleyin.

* Cihaz ikizi sorgularının cihazları ve ne zaman, en son yeniden tanımlamak bildirilen özellikleri kullanın.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Projeyi adlandırın **SimulateManagedDevice**.
   
    ![Yeni Windows Visual C# Klasik cihaz uygulaması](./media/iot-hub-csharp-csharp-device-management-get-started/createdeviceapp.png)
    
2. Çözüm Gezgini'nde sağ **SimulateManagedDevice** proje ve ardından **NuGet paketlerini Yönet...** .

3. İçinde **NuGet Paket Yöneticisi** penceresinde **Gözat** araması **Microsoft.Azure.Devices.Client**. Seçin **yükleme** yüklemek için **Microsoft.Azure.Devices.Client** paketini ve kullanım koşullarını kabul edin. Bu yordam indirir, yükler ve bir başvuru ekler [Azure IOT cihaz SDK'sını](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/) NuGet paketi ve bağımlılıkları.
   
    ![NuGet Paket Yöneticisi penceresi istemci uygulaması](./media/iot-hub-csharp-csharp-device-management-get-started/clientsdknuget.png)
    
4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
   
    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    ```

5. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini önceki bölümde not ettiğiniz cihaz bağlantı dizesiyle değiştirin.

    ```csharp
    static string DeviceConnectionString = 
      "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
    static DeviceClient Client = null;
    ```
6. Bir cihazda doğrudan yöntem uygulamak için aşağıdakileri ekleyin:

   ```csharp
   static Task<MethodResponse> onReboot(MethodRequest methodRequest, object userContext)
   {
       // In a production device, you would trigger a reboot 
       //   scheduled to start after this method returns.
       // For this sample, we simulate the reboot by writing to the console
       //   and updating the reported properties.
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
   ```

7. Son olarak, aşağıdaki kodu ekleyin **ana** yöntemi, IOT hub'ınıza bağlantıyı açın ve yöntemi dinleyicisi başlatılamıyor:

   ```csharp
   try
   {
       Console.WriteLine("Connecting to hub");
       Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, 
         TransportType.Mqtt);

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
   ```
        
8. Visual Studio Çözüm Gezgini'nde çözümünüze sağ tıklayın ve ardından **Başlangıç Projelerini Ayarla...** öğesine tıklayın. Seçin **tek başlangıç projesi**ve ardından **SimulateManagedDevice** açılır menüdeki proje. Çözümü derleyin.       

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda yeniden deneme ilkelerini (örneğin, bir üstel geri alma), makalesinde önerildiği uygulamalıdır [geçici hata işleme](/azure/architecture/best-practices/transient-faults).

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. .NET cihaz uygulamasını çalıştırmak için **SimulateManagedDevice**. Sağ **SimulateManagedDevice** proje, select **hata ayıklama**ve ardından **yeni örnek Başlat**. IOT hub'ınıza yöntemine yapılan çağrılar için dinleme başlamanız gerekir. 

2. Cihaz bağlı ve .NET çalıştırmak için yöntem çağrılarına, bekleyen **TriggerReboot** sanal cihaz uygulaması yeniden başlatma yöntemini çağırmak için uygulama. Bunu yapmak için sağ **TriggerReboot** proje, select **hata ayıklama**ve ardından **yeni örnek Başlat**. "Rebooting!" görmeniz gerekir yazılan **SimulatedManagedDevice** konsolu ve son içeren bildirilen özellikleri cihazın yeniden yazılan zaman **TriggerReboot** Konsolu.
   
    ![Hizmet ve cihaz uygulama çalıştırma](./media/iot-hub-csharp-csharp-device-management-get-started/combinedrun.png)

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]
