---
title: "Azure IOT Hub cihaz çifti özellikleri (.NET/.NET) kullanın | Microsoft Docs"
description: "Azure IOT Hub cihaz çiftlerini cihazları yapılandırmak için nasıl kullanılacağını. Sanal cihaz uygulaması ve Azure IOT hizmeti SDK'sını cihaz çifti kullanarak bir aygıt yapılandırmasını değiştirir bir hizmet uygulaması uygulamak .NET için uygulamak için Azure IOT cihaz SDK'sı .NET için kullanın."
services: iot-hub
documentationcenter: .net
author: dsk-2015
manager: timlt
editor: 
ms.assetid: 3c627476-f982-43c9-bd17-e0698c5d236d
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2017
ms.author: dkshir
ms.openlocfilehash: 0bb18f1b289dd01866842a5193437dabdb5fd20b
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2017
---
# <a name="use-desired-properties-to-configure-devices"></a>İstenen özelliklerde cihazları yapılandırmak için kullanın
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Bu öğreticinin sonunda iki .NET konsol uygulamaları olacaktır:

* **SimulateDeviceConfiguration**, istenen yapılandırma güncelleştirmesi bekler ve benzetimli yapılandırmasını güncelleştirme işleminin durumunu raporlar bir sanal cihaz uygulaması.
* **SetDesiredConfigurationAndQuery**, istenen yapılandırma bir cihazda ayarlar ve yapılandırmayı güncelleştirme işlemini sorgular bir arka uç uygulaması.

> [!NOTE]
> Makaleyi [Azure IOT SDK'ları] [ lnk-hub-sdks] hem cihaz hem de arka uç uygulamalar oluşturmak için kullanabileceğiniz Azure IOT SDK'ları hakkında bilgi sağlar.
> 
> 

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2015 veya Visual Studio 2017.
* Etkin bir Azure hesabı. Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.

İzlediyseniz [cihaz çiftlerini ile çalışmaya başlama] [ lnk-twin-tutorial] öğretici, zaten sahip olduğunuz bir IOT hub ve adlı bir cihaz kimliği **myDeviceId**. Bu durumda, atlayabilirsiniz [sanal cihaz uygulaması oluşturma] [ lnk-how-to-configure-createapp] bölümü.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-the-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma
Bu bölümde, hub'ınıza bağlanan bir .NET konsol uygulaması oluşturma **myDeviceId**, istenen yapılandırma güncelleştirmesi bekler ve daha sonra güncelleştirmeleri benzetimli yapılandırmasını güncelleştirme işlemini raporlar.

1. Visual Studio kullanarak yeni bir Visual C# Windows Klasik Masaüstü projesi oluşturma **konsol uygulaması** proje şablonu. Proje adı **SimulateDeviceConfiguration**.
   
    ![Yeni Visual C# Klasik Windows cihaz uygulaması][img-createdeviceapp]

1. Çözüm Gezgini'nde sağ **SimulateDeviceConfiguration** proje ve ardından **NuGet paketlerini Yönet...** .
1. İçinde **NuGet Paket Yöneticisi** penceresinde, seçin **Gözat** arayın ve **microsoft.azure.devices.client**. Seçin **yükleme** yüklemek için **Microsoft.Azure.Devices.Client** paketini ve kullanım koşullarını kabul edin. Bu yordam indirir, yükler ve bir başvuru ekler [Azure IOT cihaz SDK'sı] [ lnk-nuget-client-sdk] NuGet paketi ve bağımlılıklarını.
   
    ![NuGet Paket Yöneticisi penceresi istemci uygulaması][img-clientnuget]
1. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using Newtonsoft.Json;

1. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini önceki bölümünde belirtildiği cihaz bağlantı dizesini değiştirin.
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;
        static TwinCollection reportedProperties = new TwinCollection();

1. **Program** sınıfına aşağıdaki yöntemi ekleyin:
 
        public static void InitClient()
        {
            try
            {
                Console.WriteLine("Connecting to hub");
                Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }
    **İstemci** nesne ihtiyaç duyduğunuz etkileşim kurmak için cihaz çiftlerini aygıttan ile tüm yöntemleri gösterir. Yukarıda gösterilen koddan başlatır **istemci** nesne ve cihaz çiftinin alır **myDeviceId**.

1. Aşağıdaki yöntemi ekleyin **Program** sınıfı. Bu yöntem yerel cihazda telemetri ilk değerlerini ayarlar ve cihaz çifti güncelleştirir.

        public static async void InitTelemetry()
        {
            try
            {
                Console.WriteLine("Report initial telemetry config:");
                TwinCollection telemetryConfig = new TwinCollection();
                
                telemetryConfig["configId"] = "0";
                telemetryConfig["sendFrequency"] = "24h";
                reportedProperties["telemetryConfig"] = telemetryConfig;
                Console.WriteLine(JsonConvert.SerializeObject(reportedProperties));

                await Client.UpdateReportedPropertiesAsync(reportedProperties);
            }
            catch (AggregateException ex)
            {
                foreach (Exception exception in ex.InnerExceptions)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", exception);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

1. Aşağıdaki yöntemi ekleyin **Program** sınıfı. Bu değişikliği algılar bir geri çağırma olduğu *özelliklerini istenen* cihaz çiftine.

        private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, object userContext)
        {
            try
            {
                Console.WriteLine("Desired property change:");
                Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

                var currentTelemetryConfig = reportedProperties["telemetryConfig"];
                var desiredTelemetryConfig = desiredProperties["telemetryConfig"];

                if ((desiredTelemetryConfig != null) && (desiredTelemetryConfig["configId"] != currentTelemetryConfig["configId"]))
                {
                    Console.WriteLine("\nInitiating config change");
                    currentTelemetryConfig["status"] = "Pending";
                    currentTelemetryConfig["pendingConfig"] = desiredTelemetryConfig;

                    await Client.UpdateReportedPropertiesAsync(reportedProperties);

                    CompleteConfigChange();
                }
            }
            catch (AggregateException ex)
            {
                foreach (Exception exception in ex.InnerExceptions)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", exception);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

    Bu yöntem yerel cihaz çifti nesnesindeki bildirilen özellikleri yapılandırma güncelleştirme isteği ile güncelleştirir ve durumunu ayarlar **bekleyen**, hizmette cihaz çifti güncelleştirir. Cihaz çifti başarıyla güncelleştirdikten sonra yapılandırma değişikliği yöntemini çağırarak tamamlanmadan `CompleteConfigChange` sonraki noktasında açıklanmaktadır.

1. Aşağıdaki yöntemi ekleyin **Program** sınıfı. Bu yöntem bir cihaz sıfırlama benzetimini yapar, sonra güncelleştirmeleri yerel bildirilen durum ayarını özellikleri **başarı** ve kaldırır **pendingConfig** öğesi. Ardından, cihaz çifti hizmette güncelleştirir. 

        public static async void CompleteConfigChange()
        {
            try
            {
                var currentTelemetryConfig = reportedProperties["telemetryConfig"];

                Console.WriteLine("\nSimulating device reset");
                await Task.Delay(30000); 

                Console.WriteLine("\nCompleting config change");
                currentTelemetryConfig["configId"] = currentTelemetryConfig["pendingConfig"]["configId"];
                currentTelemetryConfig["sendFrequency"] = currentTelemetryConfig["pendingConfig"]["sendFrequency"];
                currentTelemetryConfig["status"] = "Success";
                currentTelemetryConfig["pendingConfig"] = null;

                await Client.UpdateReportedPropertiesAsync(reportedProperties);
                Console.WriteLine("Config change complete \nPress any key to exit.");
            }
            catch (AggregateException ex)
            {
                foreach (Exception exception in ex.InnerExceptions)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", exception);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

1. Son olarak aşağıdaki satırları ekleyin **ana** yöntemi:

        try
        {
            InitClient();
            InitTelemetry();

            Console.WriteLine("Wait for desired telemetry...");
            Client.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertyChanged, null).Wait();
            Console.ReadKey();
        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", exception);
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }

   > [!NOTE]
   > Bu öğretici herhangi davranışı eşzamanlı yapılandırma güncelleştirmelerini benzetimini değil. Bazı yapılandırma güncelleştirme işlemleri hedef yapılandırma değişiklikleri güncelleştirme çalışan bazı bunları kuyruğuna olabilir ve bazı bir hata durumu reddedebilirsiniz sırada karşılamak mümkün olabilir. Belirli bir yapılandırma işlemi için istenen davranışı dikkate aldığınızdan emin olun ve yapılandırma değişikliğini başlatmadan önce uygun mantığı ekleyin.
   > 
   > 
1. Çözümü derlemek ve tıklayarak bu cihaz uygulaması Visual Studio'dan çalıştırma **F5**. Çıktı konsolda sanal cihazınız cihaz çifti alıyor belirten, telemetri ayarlama ve istenen özellik değişikliği için bekleyen iletileri görmeniz gerekir. Uygulamayı çalıştıran tutun.

## <a name="create-the-service-app"></a>Hizmet Uygulaması Oluştur
Bu bölümde, güncelleştirmeleri bir .NET konsol uygulaması oluşturacak *özelliklerini istenen* ilişkili cihaz çifti üzerinde **myDeviceId** ile yeni bir telemetri yapılandırma nesnesi. IOT hub'ı depolanan cihaz çiftlerini sorgular ve cihazın istenen ve bildirilen yapılandırmaları arasındaki farkı gösterir.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Proje adı **SetDesiredConfigurationAndQuery**.
   
    ![Yeni Visual C# Windows Klasik Masaüstü projesi][img-createapp]
1. Çözüm Gezgini'nde sağ **SetDesiredConfigurationAndQuery** proje ve ardından **NuGet paketlerini Yönet...** .
1. **NuGet Paket Yöneticisi** penceresinde **Gözat**'ı seçin, **microsoft.azure.devices**'ı aratın, **Microsoft.Azure.Devices** paketini yüklemek için **Yükle**'yi seçin ve kullanım koşullarını kabul edin. Bu yordam ile [Azure IoT hizmet SDK'sı][lnk-nuget-service-sdk] NuGet paketi ve bağımlılıkları indirilir, yüklenir ve bu pakete bir başvuru eklenir.
   
    ![NuGet Paket Yöneticisi penceresi][img-servicenuget]
1. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
   
        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;
1. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini, önceki bölümde hub için oluşturduğunuz IoT Hub bağlantı dizesiyle değiştirin.
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. **Program** sınıfına aşağıdaki yöntemi ekleyin:
   
        static private async Task SetDesiredConfigurationAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch = new {
                    properties = new {
                        desired = new {
                            telemetryConfig = new {
                                configId = Guid.NewGuid().ToString(),
                                sendFrequency = "5m"
                            }
                        }
                    }
                };
   
            await registryManager.UpdateTwinAsync(twin.DeviceId, JsonConvert.SerializeObject(patch), twin.ETag);
            Console.WriteLine("Updated desired configuration");
   
            while (true)
            {
                var query = registryManager.CreateQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'");
                var results = await query.GetNextAsTwinAsync();
                foreach (var result in results)
                {
                    Console.WriteLine("Config report for: {0}", result.DeviceId);
                    Console.WriteLine("Desired telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Desired["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine("Reported telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Reported["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine();
                }
                Thread.Sleep(10000);
            }
        }
   
    **Kayıt defteri** nesne cihaz çiftlerini hizmet ile etkileşim kurmak için gereken tüm yöntemleri gösterir. Bu kod başlatır **kayıt defteri** nesnesi, cihaz çiftinin alır **myDeviceId**ve ardından yeni bir telemetri yapılandırma nesnesi ile istenen özelliklerini güncelleştirir.
    Bundan sonra IOT hub'ı 10 saniyede depolanan cihaz çiftlerini sorgular ve istenen ve bildirilen telemetri yapılandırmaları yazdırır. Başvurmak [IOT hub'ı sorgu dili] [ lnk-query] tüm cihazlarınızda zengin raporlar üretmek hakkında bilgi edinmek için.
   
   > [!IMPORTANT]
   > Bu uygulama yalnızca tanım amaçlıdır her 10 saniye IOT hub'ı sorgular. Sorguları birçok cihaz arasında kullanıcı dönük raporları oluşturmak ve değil değişikliklerini algılamak için kullanın. Çözümünüz cihaz olayların gerçek zamanlı bildirimler gerektiriyorsa, kullanın [twin bildirimleri][lnk-twin-notifications].
   > 
   > 
1. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery();
        Console.WriteLine("Press any key to quit.");
        Console.ReadLine();
1. Çözüm Gezgini'nde açık **başlangıç projelerini Ayarla...**  ve emin olun **eylem** için **SetDesiredConfigurationAndQuery** projedir **Başlat**. Çözümü oluşturun.
1. İle **SimulateDeviceConfiguration** cihaz uygulamasının çalışıyor, Visual Studio kullanarak hizmet uygulama çalıştırma **F5**. Çıkarılıp bildirilen yapılandırma görmelisiniz **bekleyen** için **başarı** 24 saat yerine beş dakikalık sıklığı yeni etkin gönderin.

 ![Cihaz başarıyla yapılandırıldı][img-deviceconfigured]
   
   > [!IMPORTANT]
   > Cihaz rapor işlemi ve sorgu sonucu arasında bir dakika kadar bir gecikme yoktur. Bu, çok yüksek ölçekli olarak çalışmak sorgu altyapısı olanak sağlamasıdır. Bir tek cihaz çifti kullanımı tutarlı görünümleri almak için **getDeviceTwin** yönteminde **kayıt defteri** sınıfı.
   > 
   > 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, istenen yapılandırma olarak kümesi *özellikleri istenen* çözümden arka uç ve bu değişikliği algılar ve durumunu bildirilen özellikleri aracılığıyla raporlama çok adımlı güncelleştirme işleminin benzetimini yapmak için bir cihaz uygulaması yazıldı.

Bilgi edinmek için aşağıdaki kaynakları kullanın nasıl yapılır:

* aygıtlarla telemetri gönderen [IOT Hub ile çalışmaya başlama] [ lnk-iothub-getstarted] öğretici
* zamanlama veya aygıtları bakın büyük kümeleri işlemleri [zamanlama ve yayın işleri] [ lnk-schedule-jobs] Öğreticisi.
* ile etkileşimli olarak (örneğin, kullanıcı tarafından denetlenen bir uygulamadan fan etkinleştirdikten), cihazları denetleme [doğrudan yöntemleri kullanın] [ lnk-methods-tutorial] Öğreticisi.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-csharp-twin-how-to-configure/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-csharp-twin-how-to-configure/createnetapp.png
[img-deviceconfigured]: media/iot-hub-csharp-csharp-twin-how-to-configure/deviceconfigured.png
[img-createdeviceapp]: media/iot-hub-csharp-csharp-twin-how-to-configure/createdeviceapp.png
[img-clientnuget]: media/iot-hub-csharp-csharp-twin-how-to-configure/devicesdknuget.png
[img-deviceconfigured]: media/iot-hub-csharp-csharp-twin-how-to-configure/deviceconfigured.png


<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0/

[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-twin-tutorial]: iot-hub-csharp-csharp-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-iothub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-how-to-configure-createapp]: iot-hub-csharp-csharp-twin-how-to-configure.md#create-the-simulated-device-app
