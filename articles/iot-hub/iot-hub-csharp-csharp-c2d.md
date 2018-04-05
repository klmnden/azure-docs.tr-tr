---
title: Bulut-cihaz iletilerini ile Azure IOT hub'ı (.NET) | Microsoft Docs
description: .NET için Azure IOT SDK'ları kullanarak bir Azure IOT hub'dan bir aygıta bulut-cihaz iletilerini göndermek nasıl. Bulut-cihaz iletilerini ve bulut-cihaz iletilerini göndermek için bir arka uç uygulaması değiştirmek için bir cihaz uygulaması değiştirin.
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: ''
ms.assetid: a31c05ed-6ec0-40f3-99ab-8fdd28b1a89a
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: elioda
ms.openlocfilehash: f3110e81a7229f8f279609a64949c7f0ce78d338
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="send-messages-from-the-cloud-to-your-device-with-iot-hub-net"></a>Cihazınızı IOT hub'ı (.NET) ile buluttan iletileri gönder
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Giriş
Azure IOT Hub, milyonlarca cihaza arasında güvenilir ve güvenli çift yönlü iletişimler sağlayan tam olarak yönetilen hizmet etkinleştirin ve bir çözüm arka ucu ' dir. [IOT Hub ile çalışmaya başlama] öğretici, IOT hub'ı oluşturma, bir cihaz kimliği, sağlama ve cihaz-bulut iletileri gönderen bir aygıt uygulama kodu gösterir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici derlemeler [IOT Hub ile çalışmaya başlama]. Size bir nasıl gösterir için:

* Çözüm arka ucunuz, tek bir cihaz IOT hub'ı aracılığıyla bulut cihaz ileti gönderebilir.
* Bir cihazda bulut-cihaz iletilerini alır.
* Çözüm arka ucunuz, teslimat alındısı iste (*geri bildirim*) için bir cihaz IOT Hub'ından gönderilen iletiler için.

Bulut cihaz iletileri hakkında daha fazla bilgi bulabilirsiniz [IOT Hub Geliştirici Kılavuzu][IoT Hub developer guide - C2D].

Bu öğreticinin sonunda iki .NET konsol uygulamaları çalıştırın:

* **SimulatedDevice**, oluşturduğunuz uygulama değiştirilmiş bir sürümünü [IOT Hub ile çalışmaya başlama], IOT hub'ınıza bağlanır ve bulut-cihaz iletilerini alır.
* **SendCloudToDevice**, hangi cihaz uygulaması IOT hub'ı aracılığıyla bir bulut cihaz ileti gönderir ve teslimat alındısı alır.

> [!NOTE]
> IOT hub'ı aracılığıyla birçok cihaz platformları ve (C, Java ve Javascript dahil) dillerin SDK desteği olan [Azure IOT cihaz SDK'ları]. Bu öğreticinin kod ve genellikle Azure IOT Hub Cihazınızı bağlamak hakkında adım adım yönergeler için bkz: [IOT Hub Geliştirici Kılavuzu].
> 
> 

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2015 veya Visual Studio 2017
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

## <a name="receive-messages-in-the-device-app"></a>Cihaz uygulama ileti alma
Bu bölümde, oluşturduğunuz cihaz uygulaması değiştireceksiniz [IOT Hub ile çalışmaya başlama] IOT hub'ından bulut cihaz iletileri almak için.

1. Visual Studio içinde **SimulatedDevice** projesi, aşağıdaki yöntemi ekleyin **Program** sınıfı.
   
        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud to device messages from service");
            while (true)
            {
                Message receivedMessage = await deviceClient.ReceiveAsync();
                if (receivedMessage == null) continue;
   
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
                Console.ResetColor();
   
                await deviceClient.CompleteAsync(receivedMessage);
            }
        }
   
    `ReceiveAsync` Yöntemi zaman uyumsuz olarak döndürür alınan ileti aygıt tarafından alınan zaman. Döndürdüğü *null* specifiable zaman aşımı süresinden sonra (Bu durumda, varsayılan bir dakika kullanılır). Uygulamanın ne zaman alan bir *null*, yeni iletiler için beklemeye devam etmelidir. Bu gereksinim sebebi `if (receivedMessage == null) continue` satır.
   
    Çağrı `CompleteAsync()` iletiyi başarıyla işlendi IOT hub'ı bildirir. İleti, cihaz sıradan güvenle kaldırılabilir. Bir şey, cihaz uygulama iletiyi işlemeyi tamamlanmasını engelledi oluştuysa, IOT Hub tarafından tekrar teslim edilir. Sonra cihaz uygulama mantığını işleme o iletisi önemli *ıdempotent*, böylece aynı iletiyi birden çok kez alma aynı sonucu verir. Bir uygulama geçici olarak IOT hub'ı gelecekteki tüketimi için sırasındaki ileti korunuyor sonuçları bir ileti iptal. Veya uygulama kalıcı olarak iletiyi kuyruktan kaldırır bir ileti reddedebilirsiniz. Bulut cihaz ileti yaşam döngüsü hakkında daha fazla bilgi için bkz: [IOT Hub Geliştirici Kılavuzu][IoT Hub developer guide - C2D].
   
   > [!NOTE]
   > HTTPS MQTT veya AMQP yerine bir taşıma olarak kullanırken, `ReceiveAsync` yöntemi hemen döndürür. HTTPS ile bulut cihaz iletileri için desteklenen desen denetleyen aralıklı bağlı seyrek iletileri (değerinden 25 dakikada bir) cihazlar içindir. Daha fazla HTTPS veren sonuçları IOT hub'da istekleri azaltma alır. MQTT, AMQP ve HTTPS desteği ve IOT hub'ı azaltma arasındaki farklar hakkında daha fazla bilgi için bkz: [IOT Hub Geliştirici Kılavuzu][IoT Hub developer guide - C2D].
   > 
   > 
2. Aşağıdaki yöntemi ekleyin **ana** yöntemi, hemen önce `Console.ReadLine()` satır:
   
        ReceiveC2dAsync();

> [!NOTE]
> Basitlik'ın artırmak amacıyla için bu öğreticiyi herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda yeniden deneme ilkelerini (üstel geri alma), önerilen MSDN makalesinde uygulamalıdır [geçici hata işleme].
> 
> 

## <a name="send-a-cloud-to-device-message"></a>Bulut cihaza ileti gönderme
Bu bölümde, bulut-cihaz iletilerini aygıt uygulamaya gönderir bir .NET konsol uygulaması yazma.

1. Geçerli Visual Studio çözümünde kullanarak bir Visual C# masaüstü uygulaması projesi oluşturma **konsol uygulaması** proje şablonu. Proje adı **SendCloudToDevice**.
   
    ![Visual Studio'da yeni proje][20]
2. Çözüm Gezgini'nde çözüme sağ tıklayın ve ardından **çözüm için NuGet paketlerini Yönet...** . 
   
    Bu eylem açılır **NuGet paketlerini Yönet** penceresi.
3. Arama **Microsoft.Azure.Devices**, tıklatın **yükleme**ve kullanım koşullarını kabul edin. 
   
    Bu indirir, yükler ve bir başvuru ekler [Azure IOT hizmeti SDK'sı NuGet paketi].

4. Aşağıdaki `using` deyimini **Program.cs** dosyasının üst kısmına ekleyin:
   
        using Microsoft.Azure.Devices;
5. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini IOT hub bağlantı dizesi ile değiştirin [IOT Hub ile çalışmaya başlama]:
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. **Program** sınıfına aşağıdaki yöntemi ekleyin:
   
        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud to device message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }
   
    Bu yöntem, yeni bir bulut cihaz iletisi Kimliğine sahip cihazı gönderir `myFirstDevice`. Yalnızca kullanılan bir değişiklik varsa bu parametre değiştirme [IOT Hub ile çalışmaya başlama].
7. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:
   
        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
   
        Console.WriteLine("Press any key to send a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();
8. Visual Studio'dan Çözümünüze sağ tıklayın ve seçin **başlangıç projelerini Ayarla...** . Seçin **birden fazla başlangıç projesi**seçeneğini belirleyip **Başlat** eylemi için **ReadDeviceToCloudMessages**, **SimulatedDevice**, ve **SendCloudToDevice**.
9. Tuşuna **F5**. Üç uygulama başlamanız gerekir. Seçin **SendCloudToDevice** windows ve tuşuna **Enter**. Cihaz uygulama tarafından alınan ileti görmeniz gerekir.
   
   ![Uygulama alma iletisi][21]

## <a name="receive-delivery-feedback"></a>Teslim geri alma
İstek teslim (veya sona erme) bildirimleri için IOT Hub'ından her bulut cihaz ileti için mümkündür. Bu seçenek kolayca Yeniden Dene'yi tıklatın veya maaş mantığı bildirmek çözüm arka ucu sağlar. Bulut cihaz geri bildirim hakkında daha fazla bilgi için bkz: [IOT Hub Geliştirici Kılavuzu][IoT Hub developer guide - C2D].

Bu bölümde, değişiklik **SendCloudToDevice** geri bildirim istemek ve IOT Hub'ından almak için uygulama.

1. Visual Studio içinde **SendCloudToDevice** projesi, aşağıdaki yöntemi ekleyin **Program** sınıfı.
   
        private async static void ReceiveFeedbackAsync()
        {
            var feedbackReceiver = serviceClient.GetFeedbackReceiver();
   
            Console.WriteLine("\nReceiving c2d feedback from service");
            while (true)
            {
                var feedbackBatch = await feedbackReceiver.ReceiveAsync();
                if (feedbackBatch == null) continue;
   
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received feedback: {0}", string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
                Console.ResetColor();
   
                await feedbackReceiver.CompleteAsync(feedbackBatch);
            }
        }
   
    Bu alma düzeni aygıt uygulamadan bulut-cihaz iletilerini almak için kullanılan aynı olmasına dikkat edin.
2. Aşağıdaki yöntemi ekleyin **ana** yöntemi, sonra sağ `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` satır:
   
        ReceiveFeedbackAsync();
3. Bir özelliğin belirtmek zorunda bulut cihaz ileti teslimi için geri bildirim istemek için **SendCloudToDeviceMessageAsync** yöntemi. Hemen sonra aşağıdaki satırı ekleyin `var commandMessage = new Message(...);` satır:
   
        commandMessage.Ack = DeliveryAcknowledgement.Full;
4. Basarak uygulamaları çalıştırmak **F5**. Başlangıç üç uygulama görmeniz gerekir. Seçin **SendCloudToDevice** windows ve tuşuna **Enter**. Cihaz uygulaması tarafından ve birkaç saniye sonra tarafından alınan geri bildirim iletisi alınan ileti görürsünüz, **SendCloudToDevice** uygulama.
   
   ![Uygulama alma iletisi][22]

> [!NOTE]
> Basitlik'ın artırmak amacıyla için bu öğreticiyi herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda yeniden deneme ilkelerini (üstel geri alma), önerilen MSDN makalesinde uygulamalıdır [geçici hata işleme].
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğretici kapsamında, bulut cihaza ileti gönderme ve alma öğrendiniz. 

IOT hub'ı kullanan tam uçtan uca çözümler örnekleri görmek için bkz: [Azure IOT paketi].

IOT Hub ile çözümleri geliştirme hakkında daha fazla bilgi için bkz: [IOT Hub Geliştirici Kılavuzu].

<!-- Images -->
[20]: ./media/iot-hub-csharp-csharp-c2d/create-identity-csharp1.png
[21]: ./media/iot-hub-csharp-csharp-c2d/sendc2d1.png
[22]: ./media/iot-hub-csharp-csharp-c2d/sendc2d2.png

<!-- Links -->

[Azure IOT hizmeti SDK'sı NuGet paketi]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[geçici hata işleme]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md

[IOT Hub Geliştirici Kılavuzu]: iot-hub-devguide.md
[IOT Hub ile çalışmaya başlama]: iot-hub-csharp-csharp-getstarted.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure IOT paketi]: https://docs.microsoft.com/azure/iot-suite/
[Azure IOT cihaz SDK'ları]: iot-hub-devguide-sdks.md