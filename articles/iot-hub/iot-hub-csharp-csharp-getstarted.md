---
title: "Azure IoT Hub'ı (.NET) kullanmaya başlama | Microsoft Belgeleri"
description: ".NET için IoT SDK’larını kullanarak Azure IoT Hub’a cihazdan buluta ileti göndermeyi öğrenin. IoT hub’a cihazınızı kaydetmek, ileti göndermek ve ileti okumak için sanal cihaz ve hizmet uygulamaları oluşturun."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: f40604ff-8fd6-4969-9e99-8574fbcf036c
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: f9003c65d1818952c6a019f81080d595791f63bf
ms.openlocfilehash: 69296eb9ac2a74a97b632d27733a6a06500b4abd
ms.contentlocale: tr-tr
ms.lasthandoff: 08/09/2017

---
# <a name="connect-your-device-to-your-iot-hub-using-net"></a>.NET kullanarak cihazınızı IoT hub’ınıza bağlama

[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Bu öğreticinin sonunda üç .NET konsol uygulamanız olacak:

* Bir cihaz kimliği ve cihaz uygulamanızı bağlamak için ilişkili güvenlik anahtarı oluşturan **CreateDeviceIdentity**.
* Cihaz uygulamanız tarafından gönderilen telemetriyi görüntüleyen **ReadDeviceToCloudMessages**.
* Daha önce oluşturulan cihaz kimliğiyle IoT hub'ınızı bağlayan ve MQTT protokolünü kullanarak her saniye bir telemetri iletisi gönderen **SimulatedDevice**.

Bu üç GitHub uygulamasını içeren Visual Studio çözümünü indirebilir veya kopyalayabilirsiniz.

```bash
git clone https://github.com/Azure-Samples/iot-hub-dotnet-simulated-device-client-app.git
```

> [!NOTE]
> Hem cihazlarınızda hem de çözüm arka ucunuzda çalıştırılacak uygulamalar oluşturmak için kullanabileceğiniz Azure IoT SDK'ları hakkında bilgi için bkz. [Azure IoT SDK'ları][lnk-hub-sdks].

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2015 veya Visual Studio 2017.
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

IoT hub'ınızı oluşturdunuz ve bu öğreticinin geri kalanını tamamlamak için gereken ana bilgisayar adı ve IoT Hub bağlantı dizesine sahipsiniz.

<a id="DeviceIdentity_csharp"></a>
[!INCLUDE [iot-hub-get-started-create-device-identity-csharp](../../includes/iot-hub-get-started-create-device-identity-csharp.md)]

<a id="D2C_csharp"></a>
## <a name="receive-device-to-cloud-messages"></a>Cihazdan buluta iletileri alma
Bu bölümde IoT Hub'dan cihaz-bulut iletilerini okuyan bir .NET konsol uygulaması oluşturacaksınız. IoT hub'ı, cihazdan buluta iletileri okumanızı sağlamak için [Azure Event Hubs][lnk-event-hubs-overview] ile uyumlu bir uç noktasını kullanıma sunar. Sade ve basit bir anlatım gözetildiği için bu öğretici yüksek işleme dağıtımına uygun olmayan temel bir okuyucu oluşturur. Cihazdan buluta iletilerin ölçekli olarak nasıl işleneceğini öğrenmek için [Cihazdan buluta iletileri işleme][lnk-process-d2c-tutorial] öğreticisine bakın. Event Hubs'dan iletilerin nasıl işleneceği hakkında daha fazla bilgi için [Event Hubs ile Çalışmaya Başlama][lnk-eventhubs-tutorial] öğreticisine bakın. (Bu öğretici, IoT Hub ve Event Hub ile uyumlu uç noktalar için geçerlidir.)

> [!NOTE]
> Cihazdan buluta iletileri okumak için Event Hub ile uyumlu uç nokta her zaman AMQP protokolünü kullanır.

1. Visual Studio’da **Konsol Uygulaması (.NET Framework)** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. .NET Framework sürümünün 4.5.1 veya sonraki bir sürüm olduğundan emin olun. Projeye **ReadDeviceToCloudMessages** adını verin.

    ![Yeni Visual C# Windows Klasik Masaüstü projesi][10a]

2. Çözüm Gezgini'nde **ReadDeviceToCloudMessages** projesine sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'e tıklayın.

3. **NuGet Paket Yöneticisi** penceresinde **WindowsAzure.ServiceBus**'ı aratın, **Yükle**'yi seçin ve kullanım koşullarını kabul edin. Bu yordam ile [Azure Service Bus][lnk-servicebus-nuget] ile tüm bağımlılıkları indirilir, yüklenir ve bu pakete bir başvuru eklenir. Bu paket, uygulamanın IoT hub'ınızdaki Event Hub ile uyumlu uç noktaya bağlanmasını sağlar.

4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

    ```csharp
    using Microsoft.ServiceBus.Messaging;
    using System.Threading;
    ```

5. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini, "IoT hub oluşturma" bölümünde hub için oluşturduğunuz IoT Hub bağlantı dizesiyle değiştirin.

    ```csharp
    static string connectionString = "{iothub connection string}";
    static string iotHubD2cEndpoint = "messages/events";
    static EventHubClient eventHubClient;
    ```

6. **Program** sınıfına aşağıdaki yöntemi ekleyin:

    ```csharp
    private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
    {
        var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
        while (true)
        {
            if (ct.IsCancellationRequested) break;
            EventData eventData = await eventHubReceiver.ReceiveAsync();
            if (eventData == null) continue;

            string data = Encoding.UTF8.GetString(eventData.GetBytes());
            Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
        }
    }
    ```

    Bu yöntem, tüm IoT hub'ı cihazdan buluta alma bölümlerinden iletileri almak için bir **EventHubReceiver** örneği kullanır. **EventHubReceiver** nesnesini oluştururken, yalnızca başladıktan sonra gönderilen iletileri alması için bir `DateTime.Now` parametresini nasıl geçirdiğinize dikkat edin. Bu filtre, geçerli ileti kümesini görebilmeniz açısından bir test ortamında kullanışlıdır. Bir üretim ortamında, kodunuzun tüm iletileri işlediğinden emin olmanız gerekir. Daha fazla bilgi için [IoT Hub cihazdan buluta iletiler işleme][lnk-process-d2c-tutorial] öğreticisine bakın.

7. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:

    ```csharp
    Console.WriteLine("Receive messages. Ctrl-C to exit.\n");
    eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, iotHubD2cEndpoint);

    var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;

    CancellationTokenSource cts = new CancellationTokenSource();

    System.Console.CancelKeyPress += (s, e) =>
    {
        e.Cancel = true;
        cts.Cancel();
        Console.WriteLine("Exiting...");
    };

    var tasks = new List<Task>();
    foreach (string partition in d2cPartitions)
    {
        tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
    }  
    Task.WaitAll(tasks.ToArray());
    ```

## <a name="create-a-device-app"></a>Cihaz uygulaması oluşturma

Bu bölümde, IoT Hub'a cihazdan buluta iletiler gönderen bir cihaza benzetim yapan bir .NET konsol uygulaması oluşturacaksınız.

1. Visual Studio’da **Konsol Uygulaması (.NET Framework)** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. .NET Framework sürümünün 4.5.1 veya sonraki bir sürüm olduğundan emin olun. Projeye **SimulatedDevice** adını verin.

    ![Yeni Visual C# Windows Klasik Masaüstü projesi][10b]

2. Çözüm Gezgini'nde **SimulatedDevice** projesine sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'e tıklayın.

3. **NuGet Paket Yöneticisi** penceresinde **Gözat**'ı seçin, **Microsoft.Azure.Devices.Client**'ı aratın, **Microsoft.Azure.Devices.Client** paketini yüklemek için **Yükle**'yi seçin ve kullanım koşullarını kabul edin. Bu yordam ile [Azure IoT cihaz SDK'sı NuGet paketi][lnk-device-nuget] ve bağımlılıkları indirilir, yüklenir ve bu pakete bir başvuru eklenir.

4. Aşağıdaki `using` deyimini **Program.cs** dosyasının üst kısmına ekleyin:

    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Newtonsoft.Json;
    ```

5. **Program** sınıfına aşağıdaki alanları ekleyin. `{iot hub hostname}` değerini "Bir IoT hub’ı oluşturma" bölümünde aldığınız IoT hub ana bilgisayar adı ile değiştirin. `{device key}` değerini "Bir cihaz kimliği oluşturma" bölümünde aldığınız cihaz anahtarıyla değiştirin.

    ```csharp
    static DeviceClient deviceClient;
    static string iotHubUri = "{iot hub hostname}";
    static string deviceKey = "{device key}";
    ```

6. **Program** sınıfına aşağıdaki yöntemi ekleyin:

    ```csharp
    private static async void SendDeviceToCloudMessagesAsync()
    {
        double minTemperature = 20;
        double minHumidity = 60;
        int messageId = 1;
        Random rand = new Random();

        while (true)
        {
            double currentTemperature = minTemperature + rand.NextDouble() * 15;
            double currentHumidity = minHumidity + rand.NextDouble() * 20;

            var telemetryDataPoint = new
            {
                messageId = messageId++,
                deviceId = "myFirstDevice",
                temperature = currentTemperature,
                humidity = currentHumidity
            };
            var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
            var message = new Message(Encoding.ASCII.GetBytes(messageString));
            message.Properties.Add("temperatureAlert", (currentTemperature > 30) ? "true" : "false");

            await deviceClient.SendEventAsync(message);
            Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

            await Task.Delay(1000);
        }
    }
    ```

    Bu yöntem, her saniye yeni bir cihazdan buluta iletisi gönderir. İleti, cihaz kimliğine ve bir sıcaklık sensörüyle nem sensörünün benzetimini gerçekleştirmeye yönelik rastgele oluşturulmuş sayılara sahip bir JSON ile seri hale getirilmiş bir nesneyi içerir.

7. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:

    ```csharp
    Console.WriteLine("Simulated device\n");
    deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey ("myFirstDevice", deviceKey), TransportType.Mqtt);

    SendDeviceToCloudMessagesAsync();
    Console.ReadLine();
    ```

    Varsayılan olarak, .NET Framework uygulamalarında **Create** yöntemiyle IoT hub'ıyla iletişim kurmak için AMQP protokolünü kullanan bir **DeviceClient** örneği oluşturulur. MQTT veya HTTP protokolünü kullanmak için **Create** yönteminin protokolü belirtmenize olanak tanıyan geçersiz kılmasını kullanın. UWP ve PCL istemcileri varsayılan olarak HTTP protokolünü kullanır. HTTP protokolünü kullanıyorsanız **System.Net.Http.Formatting** ad alanını dahil etmek için projenize **Microsoft.AspNet.WebApi.Client** NuGet paketini de eklemeniz gerekir.

Bu öğretici, IoT Hub cihaz uygulaması oluşturma adımlarında size rehberlik eder. Cihaz uygulamanıza gerekli kodu eklemek için [Azure IoT Hub için Bağlı Hizmet][lnk-connected-service] Visual Studio uzantısını da kullanabilirsiniz.

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. [Geçici Hata İşleme][lnk-transient-faults] adlı MSDN makalesinde önerildiği üzere, üretim kodunda yeniden deneme ilkelerini (üstel geri alma gibi) uygulamanız gerekir.

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Visual Studio'daki Çözüm Gezgini'nde çözümünüze sağ tıklayın ve ardından **Başlangıç projelerini ayarla**'ya tıklayın. **Birden fazla başlangıç projesi**'ni seçin ve ardından hem **ReadDeviceToCloudMessages** hem de **SimulatedDevice** projeleri için eylem olarak **Başla**'yı seçin.

    ![Başlangıç projesinin özellikleri][41]

2. Her iki uygulamanın da çalışmasını başlatmak için **F5**'e basın. **SimulatedDevice** uygulamasının konsol çıkışı, cihaz uygulamanızın IoT hub'ınıza gönderdiği iletileri gösterir. **ReadDeviceToCloudMessages** uygulamasından konsol çıktısı, IoT hub'ınızın aldığı iletileri gösterir.

    ![Uygulamalardan konsol çıktısı][42]

3. [Azure portalındaki][lnk-portal] **Kullanım** kutucuğu, IoT hub'ına gönderilen ileti sayısını gösterir:

    ![Azure portalı Kullanım kutucuğu][43]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portalında bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Bu cihaz kimliğini, cihaz uygulamasının, IoT hub'ına cihazdan buluta iletileri göndermesini sağlamak için kullandınız. Ayrıca, IoT hub’ı tarafından alınan iletileri görüntüleyen bir uygulama da oluşturdunuz.

IoT Hub’ı kullanmaya başlamak ve diğer IoT senaryolarını keşfetmek için bkz:

* [Cihazınızı bağlama][lnk-connect-device]
* [Cihaz yönetimini kullanmaya başlama][lnk-device-management]
* [IoT Edge ile çalışmaya başlama][lnk-iot-edge]

IoT çözümünüzün nasıl genişletileceğini ve cihazdan buluta iletilerin doğru ölçekte nasıl işleneceğini öğrenmek için [Cihazdan buluta iletileri işleme][lnk-process-d2c-tutorial] öğreticisine bakın.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10a]: ./media/iot-hub-csharp-csharp-getstarted/create-receive-csharp1.png
[10b]: ./media/iot-hub-csharp-csharp-getstarted/create-device-csharp1.png


<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

