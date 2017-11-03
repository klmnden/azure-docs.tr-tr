---
title: "Azure IOT hub'ı doğrudan yöntemleri (.NET/.NET) | Microsoft Docs"
description: "Azure IOT hub'ı doğrudan yöntemlerinin nasıl kullanılacağını. Doğrudan bir yöntem ve Azure IOT hizmeti SDK'sını doğrudan yöntemini çağıran bir hizmet uygulaması uygulamak .NET için içeren bir sanal cihaz uygulamasının uygulamak için Azure IOT cihaz SDK'sı .NET için kullanın."
services: iot-hub
documentationcenter: 
author: dsk-2015
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: dkshir
ms.openlocfilehash: 9ce1fbebb6417c10618aa182e3c1d9ddf8132fb6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-direct-methods-netnet"></a>Doğrudan yöntemleri (.NET/.NET) kullanın
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

Bu öğreticide, şu iki .NET konsol uygulamaları geliştirmek için yapacaksınız:

* **CallMethodOnDevice**, sanal cihaz uygulamasının bir yöntemi çağırır ve yanıt görüntüler bir arka uç uygulaması.
* **SimulateDeviceMethods**, daha önce oluşturulan cihaz kimliğiyle IOT hub'ınıza bağlanan bir cihaza benzetim bir konsol uygulaması ve yöntemi yanıtlar bulut tarafından çağrılır.

> [!NOTE]
> [IoT Hub SDK'ları][lnk-hub-sdks] makalesi, hem cihazlarınızda hem de çözüm arka ucunuzda çalıştırılacak uygulamalar oluşturmak için kullanabileceğiniz Azure IoT SDK’ları hakkında bilgi içerir.
> 
> 

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2015 veya Visual Studio 2017.
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

Bunun yerine cihaz kimliğini program aracılığıyla oluşturmak istiyorsanız, ilgili bölümünü okuyun [.NET kullanarak IOT hub'ınıza sanal Cihazınızı bağlamak] [ lnk-device-identity-csharp] makalesi.


## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma
Bu bölümde, çözüm tarafından arka uç olarak adlandırılan bir yönteme yanıt veren bir .NET konsol uygulaması oluşturun.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Proje adı **SimulateDeviceMethods**.
   
    ![Yeni Visual C# Klasik Windows cihaz uygulaması][img-createdeviceapp]
    
1. Çözüm Gezgini'nde sağ **SimulateDeviceMethods** proje ve ardından **NuGet paketlerini Yönet...** .
1. İçinde **NuGet Paket Yöneticisi** penceresinde, seçin **Gözat** arayın ve **microsoft.azure.devices.client**. Seçin **yükleme** yüklemek için **Microsoft.Azure.Devices.Client** paketini ve kullanım koşullarını kabul edin. Bu yordam indirir, yükler ve bir başvuru ekler [Azure IOT cihaz SDK'sı] [ lnk-nuget-client-sdk] NuGet paketi ve bağımlılıklarını.
   
    ![NuGet Paket Yöneticisi penceresi istemci uygulaması][img-clientnuget]
1. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;

1. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini önceki bölümünde belirtildiği cihaz bağlantı dizesini değiştirin.
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;

1. Cihazda doğrudan yöntemi uygulamak için aşağıdakileri ekleyin:

        static Task<MethodResponse> WriteLineToConsole(MethodRequest methodRequest, object userContext)
        {
            Console.WriteLine();
            Console.WriteLine("\t{0}", methodRequest.DataAsJson);
            Console.WriteLine("\nReturning response for method {0}", methodRequest.Name);

            string result = "'Input was written to log.'";
            return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
        }

1. Son olarak, aşağıdaki kodu ekleyin **ana** IOT hub'ınıza bağlantıyı açın ve yöntemi dinleyicisini başlatmak için yöntem:
   
        try
        {
            Console.WriteLine("Connecting to hub");
            Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);

            // setup callback for "writeLine" method
            Client.SetMethodHandlerAsync("writeLine", WriteLineToConsole, null).Wait();
            Console.WriteLine("Waiting for direct method call\n Press enter to exit.");
            Console.ReadLine();

            Console.WriteLine("Exiting...");

            // as a good practice, remove the "writeLine" handler
            Client.SetMethodHandlerAsync("writeLine", null, null).Wait();
            Client.CloseAsync().Wait();
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }
        
1. Visual Studio Çözüm Gezgini'nde Çözümünüze sağ tıklayın ve ardından **başlangıç projelerini Ayarla...** . Seçin **tek başlangıç projesi**ve ardından **SimulateDeviceMethods** açılır menüsünde projesinde.        

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda yeniden deneme ilkelerini (bağlantı yeniden deneme), önerilen MSDN makalesinde uygulamalıdır [geçici hata işleme][lnk-transient-faults].
> 
> 

## <a name="call-a-direct-method-on-a-device"></a>Bir cihazda doğrudan bir yöntem çağrısı
Bu bölümde, sanal cihaz uygulamasının bir yöntemi çağırır ve yanıt görüntüleyen bir .NET konsol uygulaması oluşturun.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. .NET Framework sürümünün 4.5.1 veya sonraki bir sürüm olduğundan emin olun. Proje adı **CallMethodOnDevice**.
   
    ![Yeni Visual C# Windows Klasik Masaüstü projesi][img-createserviceapp]
2. Çözüm Gezgini'nde sağ **CallMethodOnDevice** proje ve ardından **NuGet paketlerini Yönet...** .
3. **NuGet Paket Yöneticisi** penceresinde **Gözat**'ı seçin, **microsoft.azure.devices**'ı aratın, **Microsoft.Azure.Devices** paketini yüklemek için **Yükle**'yi seçin ve kullanım koşullarını kabul edin. Bu yordam ile [Azure IoT hizmet SDK'sı][lnk-nuget-service-sdk] NuGet paketi ve bağımlılıkları indirilir, yüklenir ve bu pakete bir başvuru eklenir.
   
    ![NuGet Paket Yöneticisi penceresi][img-servicenuget]

4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
   
        using System.Threading.Tasks;
        using Microsoft.Azure.Devices;
5. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini, önceki bölümde hub için oluşturduğunuz IoT Hub bağlantı dizesiyle değiştirin.
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. **Program** sınıfına aşağıdaki yöntemi ekleyin:
   
        private static async Task InvokeMethod()
        {
            var methodInvocation = new CloudToDeviceMethod("writeLine") { ResponseTimeout = TimeSpan.FromSeconds(30) };
            methodInvocation.SetPayloadJson("'a line to be written'");

            var response = await serviceClient.InvokeDeviceMethodAsync("myDeviceId", methodInvocation);

            Console.WriteLine("Response status: {0}, payload:", response.Status);
            Console.WriteLine(response.GetPayloadAsJson());
        }
   
    Bu yöntem adı ile doğrudan bir yöntem çağırır `writeLine` üzerinde `myDeviceId` aygıt. Ardından, konsol aygıtta tarafından sağlanan yanıt yazar. Nasıl yanıt aygıta için zaman aşımı değeri belirlemek mümkün olduğunu unutmayın.
7. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:
   
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        InvokeMethod().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

1. Visual Studio Çözüm Gezgini'nde Çözümünüze sağ tıklayın ve ardından **başlangıç projelerini Ayarla...** . Seçin **tek başlangıç projesi**ve ardından **CallMethodOnDevice** açılır menüsünde projesinde.

## <a name="run-the-applications"></a>Uygulamaları çalıştırma
Şimdi uygulamaları çalıştırmaya hazırsınız.

1. .NET cihaz uygulama çalıştırma **SimulateDeviceMethods**. IOT Hub'ınızı gelen yöntem çağrıları için dinleme başlamanız gerekir: 

    ![Cihaz uygulaması çalıştırın][img-deviceapprun]
1. Artık, cihazın bağlı ve .NET çalıştırmak için yöntem çağrılarına, bekleyen **CallMethodOnDevice** sanal cihaz uygulamasının yöntemi çağırmak için uygulama. Konsolda yazılmış aygıt yanıtı görmeniz gerekir.
   
    ![Hizmet uygulaması çalıştırın][img-serviceapprun]
1. Cihaz daha sonra bu iletiyi yazdırma tarafından yönteme tepki verdiğini:
   
    ![Cihazda çağrılan doğrudan yöntemi][img-directmethodinvoked]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure portalında yeni bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Bu cihaz kimliğini bulut tarafından çağrılan yöntemler tepki vermek sanal cihaz uygulamasının sağlamak için kullanılır. Ayrıca aygıtta yöntemleri çağırır ve aygıttan yanıt görüntüleyen bir uygulama oluşturduğunuz. 

IoT Hub’ı kullanmaya başlamak ve diğer IoT senaryolarını keşfetmek için bkz:

* [IOT Hub ile çalışmaya başlama]
* [Birden çok aygıta işleri zamanla][lnk-devguide-jobs]

Çözüm ve zamanlama yöntemini çağıran birden fazla cihazda, IOT genişletmek öğrenmek için bkz: [zamanlama ve yayın işleri] [ lnk-tutorial-jobs] Öğreticisi.

<!-- Images. -->
[img-createdeviceapp]: ./media/iot-hub-csharp-csharp-direct-methods/create-device-app.png
[img-clientnuget]: ./media/iot-hub-csharp-csharp-direct-methods/device-app-nuget.png
[img-createserviceapp]: ./media/iot-hub-csharp-csharp-direct-methods/create-service-app.png
[img-servicenuget]: ./media/iot-hub-csharp-csharp-direct-methods/service-app-nuget.png
[img-deviceapprun]: ./media/iot-hub-csharp-csharp-direct-methods/run-device-app.png
[img-serviceapprun]: ./media/iot-hub-csharp-csharp-direct-methods/run-service-app.png
[img-directmethodinvoked]: ./media/iot-hub-csharp-csharp-direct-methods/direct-method-invoked.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-device-identity-csharp]: iot-hub-csharp-csharp-getstarted.md#DeviceIdentity_csharp

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[IOT Hub ile çalışmaya başlama]: iot-hub-node-node-getstarted.md
