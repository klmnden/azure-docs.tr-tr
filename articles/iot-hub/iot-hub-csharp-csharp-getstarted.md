<properties
    pageTitle="C# için Azure IoT Hub ile çalışmaya başlama | Microsoft Azure"
    description="C# ile Azure IoT Hub ile çalışmaya başlama öğreticisi. Nesnelerin İnterneti çözümü uygulamak için Microsoft Azure IoT SDK'ları ile Azure IoT Hub ve C# kullanın."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>


# .NET için Azure IoT Hub ile çalışmaya başlama

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Bu öğreticinin sonunda üç Windows konsol uygulamanız olacak:

* Bir cihaz kimliği ve sanal cihazınızı bağlamak için ilişkili güvenlik anahtarı oluşturan **CreateDeviceIdentity**.
* Sanal cihazınız tarafından gönderilen telemetriyi görüntüleyen **ReadDeviceToCloudMessages**.
* Daha önce oluşturulan cihaz kimliğiyle IoT hub'ınızı bağlayan ve AMQPS protokolünü kullanarak her saniye bir telemetri iletisi gönderen **SimulatedDevice**.

> [AZURE.NOTE] Cihazlarda çalıştırmak için her iki uygulamayı da oluşturmak üzere kullanabileceğiniz çeşitli SDK'lar ve çözüm arka ucunuz hakkında bilgi almak için bkz. [IoT Hub SDK'ları][lnk-hub-sdks].

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

+ Microsoft Visual Studio 2015.

+ Etkin bir Azure hesabı. (Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü][lnk-free-trial].)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

IoT hub'ınızı oluşturdunuz ve bu öğreticinin geri kalanını tamamlamak için gereken ana bilgisayar adı ve bağlantı dizesine sahipsiniz.

## Cihaz kimliği oluşturma

Bu bölümde, IoT hub'ınızdaki kimlik kayıt defterinde bir cihaz kimliği oluşturan bir Windows konsol uygulaması oluşturacaksınız. Cihaz kimlik kayıt defterinde girişi olmayan bir cihaz IoT hub'ına bağlanamaz. Daha fazla bilgi için [IoT Hub Geliştirici Kılavuzu][lnk-devguide-identity]'nun "Cihaz kimlik kayıt defteri" bölümüne bakın. Bu konsol uygulamasını çalıştırdığınızda, cihazınızın IoT Hub'a cihaz-bulut iletileri gönderdiğinde kendisini tanımlamak için kullanabileceği benzersiz bir cihaz kimliği ve anahtarı oluşturulur.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. .NET Framework sürümünün 4.5.1 veya sonraki bir sürüm olduğundan emin olun. Projeye **CreateDeviceIdentity** adını verin.

    ![Yeni Visual C# Windows Klasik Masaüstü projesi][10]

2. Çözüm Gezgini'nde **CreateDeviceIdentity** projesine sağ tıklayın ve ardından **Nuget Paketlerini Yönet**'e tıklayın.

3. **Nuget Paket Yöneticisi** penceresinde **Gözat**'ı seçin, **microsoft.azure.devices**'ı aratın, **Microsoft.Azure.Devices** paketini yüklemek için **Yükle**'yi seçin ve kullanım koşullarını kabul edin. Bu yordam ile [Microsoft Azure IoT Hizmeti SDK'sı][lnk-nuget-service-sdk] Nuget paketi ve bağımlılıkları indirilir, yüklenir ve bu pakete bir başvuru eklenir.

    ![Nuget Paket Yöneticisi penceresi][11]

4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;

5. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini, önceki bölümde IoT hub'ı için oluşturduğunuz bağlantı dizesiyle değiştirin.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. **Program** sınıfına aşağıdaki yöntemi ekleyin:

        private static async Task AddDeviceAsync()
        {
            string deviceId = "myFirstDevice";
            Device device;
            try
            {
                device = await registryManager.AddDeviceAsync(new Device(deviceId));
            }
            catch (DeviceAlreadyExistsException)
            {
                device = await registryManager.GetDeviceAsync(deviceId);
            }
            Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
        }

    Bu yöntem, **myFirstDevice** kimliği ile bir cihaz kimliği oluşturur. (Bu cihaz kimliği kayıt defterinde zaten varsa kod yalnızca var olan cihaz bilgilerini alır.) Bu durumda uygulama, bu kimliğin birincil anahtarını görüntüler. IoT hub'ınıza bağlanmak için sanal cihazda bu anahtarı kullanırsınız.

7. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();

8. Bu uygulamayı çalıştırın ve cihaz anahtarını not edin.

    ![Uygulama tarafından üretilen cihaz anahtarı][12]

> [AZURE.NOTE] IoT Hub kimlik kayıt defteri, yalnızca hub'a güvenli erişim sağlamak amacıyla cihaz kimliklerini depolar. Güvenlik kimlik bilgileri olarak kullanılmak üzere cihaz kimliklerini ve anahtarlarını ve tek bir cihaza erişimi devre dışı bırakmak için kullanabileceğiniz etkin/devre dışı bayrağını depolar. Uygulamanızın cihaza özgü diğer meta verileri depolaması gerekiyorsa uygulamaya özgü bir depo kullanması gerekir. Daha fazla bilgi için bkz. [IoT Hub Geliştirici Kılavuzu][lnk-devguide-identity].

## Cihazdan buluta iletileri alma

Bu bölümde, IoT Hub'dan cihaz-bulut iletilerini okuyan bir Windows konsol uygulaması oluşturacaksınız. IoT hub'ı, cihazdan buluta iletileri okumanızı sağlamak için [Azure Event Hubs][lnk-event-hubs-overview] ile uyumlu bir uç noktasını kullanıma sunar. Sade ve basit bir anlatım gözetildiği için bu öğretici yüksek işleme dağıtımına uygun olmayan temel bir okuyucu oluşturur. Cihazdan buluta iletilerin ölçekli olarak nasıl işleneceğini öğrenmek için [Cihazdan buluta iletileri işleme][lnk-process-d2c-tutorial] öğreticisine bakın. Event Hubs'dan iletilerin nasıl işleneceği hakkında daha fazla bilgi için [Event Hubs ile Çalışmaya Başlama][lnk-eventhubs-tutorial] öğreticisine bakın. (Bu öğretici, IoT Hub ve Event Hubs ile uyumlu uç noktalar için geçerlidir.)

> [AZURE.NOTE] Cihazdan buluta iletileri okumak için Event Hubs ile uyumlu uç nokta her zaman AMQPS protokolünü kullanır.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. .NET Framework sürümünün 4.5.1 veya sonraki bir sürüm olduğundan emin olun. Projeye **ReadDeviceToCloudMessages** adını verin.

    ![Yeni Visual C# Windows Klasik Masaüstü projesi][10]

2. Çözüm Gezgini'nde **ReadDeviceToCloudMessages** projesine sağ tıklayın ve ardından **Nuget Paketlerini Yönet**'e tıklayın.

3. **Nuget Paket Yöneticisi** penceresinde **WindowsAzure.ServiceBus**'ı aratın, **Yükle**'yi seçin ve kullanım koşullarını kabul edin. Bu yordam ile [Azure Service Bus][lnk-servicebus-nuget] ile tüm bağımlılıkları indirilir, yüklenir ve bu pakete bir başvuru eklenir. Bu paket, uygulamanın IoT hub'ınızdaki Event Hubs ile uyumlu uç noktaya bağlanmasını sağlar.

4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

        using Microsoft.ServiceBus.Messaging;
        using System.Threading;

5. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini, "IoT hub oluşturma" bölümünde IoT hub için oluşturduğunuz bağlantı dizesiyle değiştirin.

        static string connectionString = "{iothub connection string}";
        static string iotHubD2cEndpoint = "messages/events";
        static EventHubClient eventHubClient;

6. **Program** sınıfına aşağıdaki yöntemi ekleyin:

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

    Bu yöntem, tüm IoT hub'ı cihazdan buluta alma bölümlerinden iletileri almak için bir **EventHubReceiver** örneği kullanır. **EventHubReceiver** nesnesini oluştururken, yalnızca başladıktan sonra gönderilen iletileri alması için bir `DateTime.Now` parametresini nasıl geçirdiğinize dikkat edin. Bu filtre, geçerli ileti kümesini görebilmeniz açısından bir test ortamında kullanışlıdır. Bir üretim ortamında, kodunuzun tüm iletileri işlediğinden emin olmanız gerekir. Daha fazla bilgi için [IoT Hub cihazdan buluta iletiler nasıl işlenir?][lnk-process-d2c-tutorial] öğreticisine bakın.

7. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:

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

## Sanal cihaz uygulaması oluşturma

Bu bölümde, IoT Hub'a cihaz-bulut iletileri gönderen bir cihaza benzetim yapan bir Windows konsol uygulaması oluşturacaksınız.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. .NET Framework sürümünün 4.5.1 veya sonraki bir sürüm olduğundan emin olun. Projeye **SimulatedDevice** adını verin.

    ![Yeni Visual C# Windows Klasik Masaüstü projesi][10]

2. Çözüm Gezgini'nde **SimulatedDevice** projesine sağ tıklayın ve ardından **Nuget Paketlerini Yönet**'e tıklayın.

3. **Nuget Paket Yöneticisi** penceresinde **Gözat**'ı seçin, **Microsoft.Azure.Devices.Client**'ı aratın, **Microsoft.Azure.Devices.Client** paketini yüklemek için **Yükle**'yi seçin ve kullanım koşullarını kabul edin. Bu yordam ile [Azure IoT - Cihaz SDK'sı Nuget paketi][lnk-device-nuget] paketi ve bağımlılıkları indirilir, yüklenir ve bu pakete bir başvuru eklenir.

4. Aşağıdaki `using` deyimini **Program.cs** dosyasının üst kısmına ekleyin:

        using Microsoft.Azure.Devices.Client;
        using Newtonsoft.Json;

5. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucuyu, "IoT hub'ı oluşturma" bölümünde aldığınız IoT hub'ı ana bilgisayar adı ve "Bir cihaz kimliği oluşturma" bölümünde aldığınız cihaz anahtarı ile değiştirin.

        static DeviceClient deviceClient;
        static string iotHubUri = "{iot hub hostname}";
        static string deviceKey = "{device key}";

6. **Program** sınıfına aşağıdaki yöntemi ekleyin:

        private static async void SendDeviceToCloudMessagesAsync()
        {
            double avgWindSpeed = 10; // m/s
            Random rand = new Random();

            while (true)
            {
                double currentWindSpeed = avgWindSpeed + rand.NextDouble() * 4 - 2;

                var telemetryDataPoint = new
                {
                    deviceId = "myFirstDevice",
                    windSpeed = currentWindSpeed
                };
                var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
                var message = new Message(Encoding.ASCII.GetBytes(messageString));

                await deviceClient.SendEventAsync(message);
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

                Task.Delay(1000).Wait();
            }
        }

    Bu yöntem, her saniye yeni bir cihazdan buluta iletisi gönderir. İleti, cihaz kimliğine ve bir rüzgar hızı sensörü benzetimi gerçekleştirmeye yönelik rastgele oluşturulmuş bir sayıya sahip bir JSON ile seri hale getirilmiş bir nesneyi içerir.

7. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:

        Console.WriteLine("Simulated device\n");
        deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));

        SendDeviceToCloudMessagesAsync();
        Console.ReadLine();

  Varsayılan olarak, **Create** yöntemiyle IoT hub'ıyla iletişim kurmak için AMQP protokolünü kullanan bir **DeviceClient** örneği oluşturulur. HTTPS protokolünü kullanmak için, **Create** yönteminin protokolü belirtmenize olanak tanıyan geçersiz kılmasını kullanın. HTTPS protokolünü kullanıyorsanız **System.Net.Http.Formatting** ad alanını dahil etmek için projenize **Microsoft.AspNet.WebApi.Client** Nuget paketini de eklemeniz gerekir.

Bu öğretici, IoT Hub cihaz istemcisi oluşturma adımlarında size rehberlik eder. Cihaz istemcisi uygulamanıza gerekli kodu eklemek için [Azure IoT Hub için Bağlı Hizmet][lnk-connected-service] Visual Studio uzantısını da kullanabilirsiniz.


> [AZURE.NOTE] Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. [Geçici Hata İşleme][lnk-transient-faults] adlı MSDN makalesinde önerildiği üzere, üretim kodunda yeniden deneme ilkelerini (üstel geri alma gibi) uygulamanız gerekir.

## Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1.  Visual Studio'daki Çözüm Gezgini'nde çözümünüze sağ tıklayın ve ardından **Başlangıç projelerini ayarla**'ya tıklayın. **Birden fazla başlangıç projesi**'ni seçin ve ardından hem **ReadDeviceToCloudMessages** hem de **SimulatedDevice** projeleri için eylem olarak **Başla**'yı seçin.

    ![Başlangıç projesinin özellikleri][41]

2.  Her iki uygulamanın da çalışmasını başlatmak için **F5**'e basın. **SimulatedDevice** uygulamasından konsol çıktısı, sanal cihazınızın IoT hub'ınıza gönderdiği iletileri gösterir. **ReadDeviceToCloudMessages** uygulamasından konsol çıktısı, IoT hub'ınızın aldığı iletileri gösterir.

    ![Uygulamalardan konsol çıktısı][42]

3. [Azure portalındaki][lnk-portal] **Kullanım** kutucuğu, hub'a gönderilen ileti sayısını gösterir:

    ![Azure portalı Kullanım kutucuğu][43]


## Sonraki adımlar

Bu öğreticide, portalda bir IoT hub'ı yapılandırdınız ve ardından hub'ın kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Bu cihaz kimliğini, sanal cihaz uygulamasının hub'a cihaz-bulut iletileri göndermesini sağlamak için kullandınız. Hub tarafından alınan iletileri görüntüleyen bir uygulama da oluşturdunuz. 

IoT Hub’ı kullanmaya başlamak ve diğer IoT senaryolarını keşfetmek için bkz:

- [Cihazınızı bağlama][lnk-connect-device]
- [Cihaz yönetimi ile çalışmaya başlama][lnk-device-management]
- [Ağ Geçidi SDK’sı ile çalışmaya başlama][lnk-gateway-SDK]

IoT çözümünüzün nasıl genişletileceğini ve cihazdan buluta iletilerin doğru ölçekte nasıl işleneceğini öğrenmek için [Cihazdan buluta iletileri işleme][lnk-process-d2c-tutorial] öğreticisine bakın.

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp1.png
[11]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp2.png
[12]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp3.png

<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/



<!--HONumber=Oct16_HO1-->


