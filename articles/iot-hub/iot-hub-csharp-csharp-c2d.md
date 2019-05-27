---
title: Bulut-cihaz iletilerini Azure IOT hub'ı (.NET) | Microsoft Docs
description: .NET için Azure IOT SDK'larını kullanarak bir Azure IOT hub'ından bir cihaza bulut-cihaz iletilerini göndermek nasıl. Bulut-cihaz iletilerini ve bulut-cihaz iletilerini göndermek için bir arka uç uygulaması değiştirmek için bir cihaz uygulaması değiştirin.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: robinsh
ms.openlocfilehash: 0d83bdc3fd3f644013a2d2b80128839658524db9
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65864441"
---
# <a name="send-messages-from-the-cloud-to-your-device-with-iot-hub-net"></a>Cihazınızı IOT hub'ı (.NET) ile buluttan iletiler gönderme

[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Tanıtım

Azure IOT hub'ı yardımcı olan tam olarak yönetilen bir hizmet, milyonlarca cihaz arasında güvenilir ve güvenli çift yönlü iletişimi etkinleştirmek ve bir çözüm arka ucu ' dir. [Telemetri, bir CİHAZDAN bir IOT hub'ına gönderme... ](quickstart-send-telemetry-dotnet.md) IOT hub oluşturma, bir cihaz kimliği da sağlamak ve CİHAZDAN buluta iletiler gönderen bir cihaza kod gösterilmektedir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici hızlı derlemeler [telemetri gönderir bir CİHAZDAN bir IOT hub'ına... ](quickstart-send-telemetry-dotnet.md). Bunu, aşağıdaki adımları uygulayın işlemini göstermektedir:

* Çözüm arka ucunuz, tek bir cihaz IOT hub'ı aracılığıyla bulut-cihaz iletilerini gönderin.

* Bir cihazda bulut-cihaz iletilerini alır.

* Çözüm arka ucunuz, teslimat alındısı istek (*geri bildirim*) için bir cihaz IOT Hub'ından gönderilen iletileri.

Bulut-cihaz iletileri hakkında daha fazla bilgi bulabilirsiniz [D2C ve IOT Hub ile C2D Mesajlaşma](iot-hub-devguide-messaging.md).

Bu öğreticinin sonunda iki .NET konsol uygulaması çalıştırın.

* **SimulatedDevice**, oluşturulan uygulamayı değiştirilmiş bir sürümünü [telemetri gönderir bir CİHAZDAN bir IOT hub'ına... ](quickstart-send-telemetry-dotnet.md), IOT hub'ınıza bağlanır ve bulut-cihaz iletilerini alır.

* **SendCloudToDevice**, IOT hub'ı aracılığıyla cihaz uygulamasına bulut-cihaz ileti gönderir ve ardından, teslimat alındısı.

> [!NOTE]
> IOT Hub aracılığıyla SDK desteği birçok cihaz platformlarını ve dilini (C, Java ve Javascript gibi) sahip [Azure IOT cihaz SDK'ları](iot-hub-devguide-sdks.md). Bu öğreticinin koda ve genellikle Azure IOT hub'a Cihazınızı bağlamak hakkında adım adım yönergeler için bkz. [IOT Hub Geliştirici kılavuzunun](iot-hub-devguide.md).
> 

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio

* Etkin bir Azure hesabı. (Hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.)

## <a name="receive-messages-in-the-device-app"></a>Cihaz uygulamasında ileti alma

Bu bölümde oluşturduğunuz cihaz uygulamasını değiştireceksiniz [telemetri gönderir bir CİHAZDAN bir IOT hub'ına... ](quickstart-send-telemetry-dotnet.md) IOT hub'ından bulut-cihaz iletilerini almak için.

1. Visual Studio içinde **SimulatedDevice** projesinde, aşağıdaki yöntemi ekleyin **Program** sınıfı.

   ```csharp
    private static async void ReceiveC2dAsync()
    {
        Console.WriteLine("\nReceiving cloud to device messages from service");
        while (true)
        {
            Message receivedMessage = await s_deviceClient.ReceiveAsync();
            if (receivedMessage == null) continue;

            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("Received message: {0}", 
            Encoding.ASCII.GetString(receivedMessage.GetBytes()));
            Console.ResetColor();

            await s_deviceClient.CompleteAsync(receivedMessage);
        }
    }
   ```

   `ReceiveAsync` Yöntemi zaman uyumsuz olarak döndürür alınan ileti, cihaz tarafından alınan zaman. Döndürür *null* specifiable zaman aşımı süresinden sonra (Bu durumda, varsayılan bir dakika kullanılır). Uygulama aldığında bir *null*, yeni iletileri için beklemeye devam etmelidir. Bu gereksinim sebebi `if (receivedMessage == null) continue` satır.

    Çağrı `CompleteAsync()` IOT Hub ileti başarıyla işlendi bildirir. İleti, cihaz kuyruktan güvenli bir şekilde kaldırılabilir. IOT Hub cihaz uygulaması iletisinin işlenmesi tamamlamasını engelleyen bir sorun oluştu, yeniden sağlar. Ardından işleme mantığı cihaz uygulamasında bu ileti önemli olduğu *ıdempotent*, böylece birden çok kez aynı iletiyi almak için aynı sonucu üretir. 

    Bir uygulamanın da geçici olarak IOT hub'ı gelecekteki kullanım için bir Kuyruktaki iletinin koruma sonuçlanır bir ileti birleştirileceğine. Veya uygulamayı kalıcı olarak iletiyi kuyruktan kaldırır. bir ileti reddedebilirsiniz. Bulut buluttan cihaza iletinin yaşam döngüsü hakkında daha fazla bilgi için bkz. [D2C ve C2D IOT Hub ile ileti](iot-hub-devguide-messaging.md).

   > [!NOTE]
   > Aktarım olarak, MQTT veya AMQP yerine HTTPS kullanarak `ReceiveAsync` yöntemi hemen döndürür. Bulut-cihaz iletilerini HTTPS ile desteklenen desenini denetleyen aralıklı olarak bağlanan seyrek iletileri (küçüktür 25 dakikada bir) cihazlar içindir. Daha fazla HTTPS verme istekleri azaltma IOT hub'da sonuçlarını alır. MQTT, AMQP ve HTTPS desteği ve IOT hub'ı azaltma arasındaki farklar hakkında daha fazla bilgi için bkz. [D2C ve C2D IOT Hub ile ileti](iot-hub-devguide-messaging.md).
   >

2. Aşağıdaki yöntemi ekleyin **ana** yöntemi, hemen önce `Console.ReadLine()` satırı:

   ```csharp
   ReceiveC2dAsync();
   ```

## <a name="get-the-iot-hub-connection-string"></a>IOT Hub bağlantı dizesini alın

İlk olarak, IOT Hub bağlantı dizesini portaldan alma.

1. Oturum [Azure portalında](https://portal.azure.com)seçin **kaynak grupları**.

2. Bu yöntem için kullandığınız kaynak grubunu seçin.

3. Kullanmakta olduğunuz IOT Hub'ı seçin.

4. Hub için bölmesinde seçin **paylaşılan erişim ilkeleri**.

5. Seçin **iothubowner**. Bağlantı dizelerini gösterir **iothubowner** paneli. Kopyala simgesini seçin **bağlantı dizesi – birincil anahtar**. Daha sonra kullanmak için bağlantı dizesini kaydedin.

   ![IOT Hub bağlantı dizesini alma](./media/iot-hub-csharp-csharp-c2d/get-iot-hub-connection-string.png)

## <a name="send-a-cloud-to-device-message"></a>Bulut buluttan cihaza ileti gönderme

Şimdi cihaz uygulaması için bulut-cihaz iletilerini gönderen bir .NET konsol uygulaması yazın.

1. Geçerli Visual Studio çözümünde, çözüme sağ tıklayın ve Ekle > Yeni proje. Seçin **Windows Masaüstü** ardından **konsol uygulaması (.NET Framework)**. Projeyi adlandırın **SendCloudToDevice** ve .NET Framework'ün en son sürümü seçin ve ardından **Tamam** projeyi oluşturmak için.

   ![Visual Studio'da yeni proje](./media/iot-hub-csharp-csharp-c2d/create-identity-csharp1.png)

2. Çözüm Gezgini'nde çözüme sağ tıklayın ve ardından **çözüm için NuGet paketlerini Yönet...** .

   Bu eylem açar **NuGet paketlerini Yönet** penceresi.

3. Arama **Microsoft.Azure.Devices**, Gözat sekmesini seçin. Paket bulduğunuzda tıklayın **yükleme**ve kullanım koşullarını kabul edin.

   Bu indirir, yükler ve bir başvuru ekler [Azure IOT hizmeti SDK'sı NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.Devices/).

4. Aşağıdaki `using` en üstündeki deyimi **Program.cs** dosya.

   ``` csharp
   using Microsoft.Azure.Devices;
   ```

5. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini, bu bölümde daha önce kaydettiğiniz IOT hub bağlantı dizesiyle değiştirin. 

   ``` csharp
   static ServiceClient serviceClient;
   static string connectionString = "{iot hub connection string}";
   ```

6. **Program** sınıfına aşağıdaki yöntemi ekleyin. Cihazı tanımlarken kullandığınız için cihaz adını ayarlayın [telemetri gönderir bir CİHAZDAN bir IOT hub'ına... ](quickstart-send-telemetry-dotnet.md).

   ``` csharp
   private async static Task SendCloudToDeviceMessageAsync()
   {
        var commandMessage = new
         Message(Encoding.ASCII.GetBytes("Cloud to device message."));
        await serviceClient.SendAsync("myDevice", commandMessage);
   }
   ```

   Bu yöntem cihazı kimliği ile yeni bir bulut-cihaz ileti gönderir `myFirstDevice`. Yalnızca kullanılan bir değişiklik varsa, bu parametreyi değiştirmek [telemetri gönderir bir CİHAZDAN bir IOT hub'ına... ](quickstart-send-telemetry-dotnet.md).

7. Son olarak, aşağıdaki satırları ekleyin **ana** yöntemi.

   ``` csharp
   Console.WriteLine("Send Cloud-to-Device message\n");
   serviceClient = ServiceClient.CreateFromConnectionString(connectionString);

   Console.WriteLine("Press any key to send a C2D message.");
   Console.ReadLine();
   SendCloudToDeviceMessageAsync().Wait();
   Console.ReadLine();
   ```

8. Visual Studio içinde çözümü sağ tıklatın ve seçin **başlangıç projelerini Ayarla...** . Seçin **birden fazla başlangıç projesi**, ardından **Başlat** için eylem **ReadDeviceToCloudMessages**, **SimulatedDevice**, ve **SendCloudToDevice**.

9. Tuşuna **F5**. Üç uygulama başlamanız gerekir. Seçin **SendCloudToDevice** windows ve ENTER tuşuna **Enter**. Cihaza uygulama tarafından alınan iletiyi görmeniz gerekir.

   ![Uygulama alma iletisi](./media/iot-hub-csharp-csharp-c2d/sendc2d1.png)

## <a name="receive-delivery-feedback"></a>Teslim geri bildirim alın

İstek teslim (veya zaman aşımı) bildirimleri için IOT Hub'ından her bulut-cihaz ileti için mümkündür. Bu seçenek, kolayca yeniden deneyin ya da tazminat ödemeden mantıksal bildirmek çözüm arka ucu sağlar. Bulut-cihaz geri bildirim hakkında daha fazla bilgi için bkz. [D2C ve IOT Hub ile C2D Mesajlaşma](iot-hub-devguide-messaging.md).

Bu bölümde, değişiklik **SendCloudToDevice** geri bildirim isteme ve IOT hub'ından almak için uygulama.

1. Visual Studio içinde **SendCloudToDevice** projesinde, aşağıdaki yöntemi ekleyin **Program** sınıfı.

   ```csharp
   private async static void ReceiveFeedbackAsync()
   {
        var feedbackReceiver = serviceClient.GetFeedbackReceiver();

        Console.WriteLine("\nReceiving c2d feedback from service");
        while (true)
        {
            var feedbackBatch = await feedbackReceiver.ReceiveAsync();
            if (feedbackBatch == null) continue;

            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("Received feedback: {0}",
              string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
            Console.ResetColor();

            await feedbackReceiver.CompleteAsync(feedbackBatch);
        }
    }
    ```

    Bu alma Düzen cihaz uygulamasından bulut-cihaz iletilerini almak için kullanılan aynı olduğuna dikkat edin.

2. Aşağıdaki yöntemi ekleyin **ana** yöntemi, sonra sağ `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` satır.

   ``` csharp
   ReceiveFeedbackAsync();
   ```

3. Bir özelliğin belirtmek zorunda bulut buluttan cihaza iletinin teslim için geri bildirim istemek için **SendCloudToDeviceMessageAsync** yöntemi. Hemen sonra aşağıdaki satırı ekleyin `var commandMessage = new Message(...);` satır.

   ``` csharp
   commandMessage.Ack = DeliveryAcknowledgement.Full;
   ```

4. Tuşlarına basarak uygulamaları çalıştırma **F5**. Başlangıç üç uygulama görmeniz gerekir. Seçin **SendCloudToDevice** windows ve ENTER tuşuna **Enter**. Birkaç saniye sonra ve cihaz uygulaması tarafından alınan geri bildirim iletisi alınan iletiyi görmeniz gerekir, **SendCloudToDevice** uygulama.

   ![Uygulama alma iletisi](./media/iot-hub-csharp-csharp-c2d/sendc2d2.png)

> [!NOTE]
> Basitlik'ın çok için bu öğreticiyi herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda yeniden deneme ilkelerini (üstel geri alma), örneğin makalesinde önerildiği uygulamalıdır [geçici hata işleme](/azure/architecture/best-practices/transient-faults).
>

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır makalesinde bulut-cihaz iletilerini gönderip öğrendiniz.

IOT hub'ı kullanan tam uçtan uca çözümler örneklerini görmek için bkz: [Azure IOT Uzaktan izleme çözüm Hızlandırıcısını](https://docs.microsoft.com/azure/iot-suite/).

IOT Hub ile çözümleri geliştirme hakkında daha fazla bilgi için bkz. [IOT Hub Geliştirici kılavuzunun](iot-hub-devguide.md).