---
title: "Azure IOT Hub cihaz çiftlerini (.NET/.NET) ile çalışmaya başlama | Microsoft Docs"
description: "Azure IOT Hub cihaz çiftlerini etiket ekleyebilir ve IOT Hub sorgusuyla kullanmak için nasıl kullanılacağını. Sanal cihaz uygulamasının ve Azure IOT hizmeti SDK'sını etiketleri ekler ve IOT hub'ı sorgu çalışan bir hizmet uygulaması uygulamak .NET için uygulamak için Azure IOT cihaz SDK'sı .NET için kullanın."
services: iot-hub
documentationcenter: node
author: dsk-2015
manager: timlt
editor: 
ms.assetid: f7e23b6e-bfde-4fba-a6ec-dbb0f0e005f4
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: dkshir
ms.openlocfilehash: 6073d594117e69676b753a1e3af25fffa3583a2b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-device-twins-netnet"></a>Cihaz çiftlerini (.NET/.NET) ile çalışmaya başlama
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Bu öğreticinin sonunda bu .NET konsol uygulamaları olacaktır:

* **CreateDeviceIdentity**, bir cihaz kimliği ve sanal cihaz uygulamanızı bağlamak için ilişkili güvenlik anahtarı oluşturan bir .NET uygulaması.
* **AddTagsAndQuery**, etiketleri ekler ve cihaz çiftlerini sorgular .NET arka uç uygulaması.
* **ReportConnectivity**, bir cihaza benzetim yapan daha önce oluşturulan cihaz kimliğiyle IOT hub'ınıza bağlanır ve kendi bağlantı koşulu raporları .NET cihaz uygulaması.

> [!NOTE]
> Makaleyi [Azure IOT SDK'ları] [ lnk-hub-sdks] hem cihaz hem de arka uç uygulamalar oluşturmak için kullanabileceğiniz Azure IOT SDK'ları hakkında bilgi sağlar.
> 
> 

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2015 veya Visual Studio 2017.
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

Bunun yerine cihaz kimliğini program aracılığıyla oluşturmak istiyorsanız, ilgili bölümünü okuyun [.NET kullanarak IOT hub'ınıza sanal Cihazınızı bağlamak] [ lnk-device-identity-csharp] makalesi.

## <a name="create-the-service-app"></a>Hizmet Uygulaması Oluştur
Bu bölümde, konum meta verileri ile ilişkili cihaz çifti ekler (C# kullanarak) bir .NET konsol uygulaması oluşturma **myDeviceId**. Ardından, IOT hub'ı ABD, ardından bir cep telefonu bağlantı bildirilen olanları içinde bulunan aygıtları seçerek depolanan cihaz çiftlerini sorgular.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Proje adı **AddTagsAndQuery**.
   
    ![Yeni Visual C# Windows Klasik Masaüstü projesi][img-createapp]
1. Çözüm Gezgini'nde sağ **AddTagsAndQuery** proje ve ardından **NuGet paketlerini Yönet...** .
1. İçinde **NuGet Paket Yöneticisi** penceresinde, seçin **Gözat** arayın ve **microsoft.azure.devices**. Seçin **yükleme** yüklemek için **Microsoft.Azure.Devices** paketini ve kullanım koşullarını kabul edin. Bu yordam ile [Azure IoT hizmet SDK'sı][lnk-nuget-service-sdk] NuGet paketi ve bağımlılıkları indirilir, yüklenir ve bu pakete bir başvuru eklenir.
   
    ![NuGet Paket Yöneticisi penceresi][img-servicenuget]
1. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
   
        using Microsoft.Azure.Devices;
1. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini, önceki bölümde hub için oluşturduğunuz IoT Hub bağlantı dizesiyle değiştirin.
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. **Program** sınıfına aşağıdaki yöntemi ekleyin:
   
        public static async Task AddTagsAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch =
                @"{
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                        }
                    }
                }";
            await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);
   
            var query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            var twinsInRedmond43 = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43: {0}", string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));
   
            query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43 using cellular network: {0}", string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
        }
   
    **RegistryManager** sınıfı cihaz çiftlerini hizmet ile etkileşim kurmak için gereken tüm yöntemleri sunar. Önceki kod ilk başlatır **registryManager** nesnesi, ardından cihaz çiftinin alır **myDeviceId**ve son olarak, etiketleri istenen konumu bilgilerle güncelleştirir.
   
    Güncelleştirdikten sonra iki sorguları yürüten: yalnızca cihaz çiftlerini bulunan aygıtların ilk seçer **Redmond43** tesis ve ikinci da cep telefonu şebekesi bağlı aygıtlar seçmek için sorgu iyileştirir.
   
    Unutmayın, oluşturduğunda, önceki kod **sorgu** nesne, döndürülen belgelerin en fazla sayısını belirtir. **Sorgu** nesnesini içeren bir **HasMoreResults** çağırmak için kullanabileceğiniz boolean özelliği **GetNextAsTwinAsync** birden çok kez tüm sonuçları almak için yöntemleri. Bir yöntem olarak adlandırılan **GetNextAsJson** değil cihaz çiftlerini, örneğin, sonuçları için toplama sorguların sonuçlarını kullanılabilir.
1. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

1. Çözüm Gezgini'nde açık **başlangıç projelerini Ayarla...**  ve emin olun **eylem** için **AddTagsAndQuery** projedir **Başlat**. Çözümü oluşturun.
1. Üzerinde sağ tıklayarak bu uygulamayı çalıştırmak **AddTagsAndQuery** proje ve seçerek **hata ayıklama**, ardından **başlangıç yeni örnek**. Bulunan tüm cihazlar için sorgu soran bir cihazda sonuçlarında görmelisiniz **Redmond43** ve sonuçları bir cep telefonu şebekesi kullanan cihazlar için sınırlar sorgu için yok.
   
    ![Sorgu Sonuçları penceresinde][img-addtagapp]

Sonraki bölümde, önceki bölümde sorgusunun sonucu değiştirir ve bağlantı bilgilerini raporlar bir cihaz uygulaması oluşturursunuz.

## <a name="create-the-device-app"></a>Cihaz uygulaması oluşturma
Bu bölümde, hub'ınıza bağlanan bir .NET konsol uygulaması oluşturma **myDeviceId**ve ardından bir cep telefonu şebekesi kullanarak bağlı bilgileri içerecek şekilde bildirilen özelliklerini güncelleştirir.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Proje adı **ReportConnectivity**.
   
    ![Yeni Visual C# Klasik Windows cihaz uygulaması][img-createdeviceapp]
    
1. Çözüm Gezgini'nde sağ **ReportConnectivity** proje ve ardından **NuGet paketlerini Yönet...** .
1. İçinde **NuGet Paket Yöneticisi** penceresinde, seçin **Gözat** arayın ve **microsoft.azure.devices.client**. Seçin **yükleme** yüklemek için **Microsoft.Azure.Devices.Client** paketini ve kullanım koşullarını kabul edin. Bu yordam indirir, yükler ve bir başvuru ekler [Azure IOT cihaz SDK'sı] [ lnk-nuget-client-sdk] NuGet paketi ve bağımlılıklarını.
   
    ![NuGet Paket Yöneticisi penceresi istemci uygulaması][img-clientnuget]
1. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using Newtonsoft.Json;

1. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini önceki bölümünde belirtildiği cihaz bağlantı dizesini değiştirin.
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;

1. **Program** sınıfına aşağıdaki yöntemi ekleyin:

       public static async void InitClient()
        {
            try
            {
                Console.WriteLine("Connecting to hub");
                Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);
                Console.WriteLine("Retrieving twin");
                await Client.GetTwinAsync();
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

    **İstemci** nesne ihtiyaç duyduğunuz etkileşim kurmak için cihaz çiftlerini aygıttan ile tüm yöntemleri gösterir. Yukarıda gösterilen koddan başlatır **istemci** nesne ve cihaz çiftinin alır **myDeviceId**.

1. **Program** sınıfına aşağıdaki yöntemi ekleyin:
   
        public static async void ReportConnectivity()
        {
            try
            {
                Console.WriteLine("Sending connectivity data as reported property");
                
                TwinCollection reportedProperties, connectivity;
                reportedProperties = new TwinCollection();
                connectivity = new TwinCollection();
                connectivity["type"] = "cellular";
                reportedProperties["connectivity"] = connectivity;
                await Client.UpdateReportedPropertiesAsync(reportedProperties);
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

   Güncelleştirmeleri yukarıdaki kodu **myDeviceId**bağlantı bilgilerini özelliğiyle bildirilen.

1. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:
   
       try
       {
            InitClient();
            ReportConnectivity();
       }
       catch (Exception ex)
       {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
       }
       Console.WriteLine("Press Enter to exit.");
       Console.ReadLine();

1. Çözüm Gezgini'nde açık **başlangıç projelerini Ayarla...**  ve emin olun **eylem** için **ReportConnectivity** projedir **Başlat**. Çözümü oluşturun.
1. Üzerinde sağ tıklayarak bu uygulamayı çalıştırmak **ReportConnectivity** proje ve seçerek **hata ayıklama**, ardından **başlangıç yeni örnek**. Twin bilgi alma ve sonra bağlantı olarak gönderme görmelisiniz bir *özelliği bildirilen*.
   
    ![Rapor bağlantısı cihaz uygulamayı çalıştırma][img-rundeviceapp]
    
    
1. Aygıt bağlantısı bilgilerini bildirdi, her iki sorgularda görüntülenmelidir. .NET çalıştırmak **AddTagsAndQuery** uygulama sorguları yeniden çalıştırın. Bu süre **myDeviceId** hem sorgu sonuçlarında görüntülenmesi gerekir.
   
    ![Başarıyla bildirilen cihaz bağlantısı][img-tagappsuccess]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure portalında yeni bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Cihaz meta verilerini bir arka uç uygulamadan etiketler eklendi ve sanal cihaz uygulaması rapor cihaz bağlantı bilgilerini cihaz çiftine yazıldı. Ayrıca SQL benzeri IOT hub'ı sorgu dili kullanarak bu bilgileri sorgulamak öğrendiniz.

Bilgi edinmek için aşağıdaki kaynakları kullanın nasıl yapılır:

* aygıtlarla telemetri gönderen [IOT Hub ile çalışmaya başlama] [ lnk-iothub-getstarted] öğretici
* cihaz çifti'nın istenen özelliklere sahip kullanarak cihazları yapılandırma [kullanmak istediğiniz cihazları yapılandırmak için Özellikler] [ lnk-twin-how-to-configure] öğretici
* aygıtlarını etkileşimli olarak (örneğin, kullanıcı tarafından denetlenen bir uygulamadan fan etkinleştirdikten) ile [doğrudan yöntemleri kullanın] [ lnk-methods-tutorial] Öğreticisi.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-csharp-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-csharp-twin-getstarted/createnetapp.png
[img-addtagapp]: media/iot-hub-csharp-csharp-twin-getstarted/addtagapp.png
[img-createdeviceapp]: media/iot-hub-csharp-csharp-twin-getstarted/createdeviceapp.png
[img-clientnuget]: media/iot-hub-csharp-csharp-twin-getstarted/clientsdknuget.png
[img-rundeviceapp]: media/iot-hub-csharp-csharp-twin-getstarted/rundeviceapp.png
[img-tagappsuccess]: media/iot-hub-csharp-csharp-twin-getstarted/tagappsuccess.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/

[lnk-device-identity-csharp]: iot-hub-csharp-csharp-getstarted.md#DeviceIdentity_csharp
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-twin-how-to-configure]: iot-hub-csharp-node-twin-how-to-configure.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

