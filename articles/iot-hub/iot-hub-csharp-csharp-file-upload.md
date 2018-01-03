---
title: ".NET ile Azure IOT Hub'ına aygıtlardan dosyaları karşıya yükleme | Microsoft Docs"
description: ".NET için Azure IOT cihaz SDK'sını kullanarak buluta bir aygıttan dosyaları karşıya yükleme yapma. Karşıya yüklenen dosyaların bir Azure depolama blob kapsayıcısında depolanır."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 4759d229-f856-4526-abda-414f8b00a56d
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/04/2017
ms.author: elioda
ms.openlocfilehash: 4362512121ca426fcae6716c74e1f8effa0986f1
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-using-net"></a>Cihazınızı dosyalarından buluta IOT Hub .NET kullanarak karşıya yükle

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Bu öğretici kodda inşa edilmiştir [IOT Hub ile bulut cihaza ileti gönderme](iot-hub-csharp-csharp-c2d.md) IOT hub'ı dosya karşıya yükleme özelliklerini kullanmayı gösteren öğretici. Size bir nasıl gösterir için:

- Güvenli bir şekilde bir aygıt ile Azure sağlayan bir dosya yüklemek için URI blob.
- IOT hub'ı dosya karşıya yükleme bildirimleri, uygulama arka uç dosya işleme tetiklemek için kullanın.

[IOT Hub ile çalışmaya başlama](iot-hub-csharp-csharp-getstarted.md) ve [IOT Hub ile bulut cihaza ileti gönderme](iot-hub-csharp-csharp-c2d.md) öğreticiler IOT Hub'ın temel cihaz Bulut ve bulut-cihaz Mesajlaşma işlevlerini gösterir. [İşlem cihaz-bulut iletileri](iot-hub-csharp-csharp-process-d2c.md) öğretici Azure blob depolama alanına cihaz bulut iletilerini güvenilir bir şekilde depolamak için bir yol açıklar. Ancak, bazı senaryolarda aygıtlarınızı IOT hub'ı kabul görece küçük bir cihaz bulut iletilerini göndermek verileri kolayca eşlenemiyor. Örneğin:

* Görüntüleri içeren büyük dosyaları
* Videolar
* Yüksek sıklıkta örneklenen titreşimi veri
* Önceden işlenmiş veri çeşit

Bu dosyalar genellikle gibi araçları kullanılarak bulutta işlenen toplu olan [Azure Data Factory](../data-factory/introduction.md) veya [Hadoop](../hdinsight/index.md) yığını. Bir CİHAZDAN dosyaları karşıya yükleme gerektiğinde, güvenlik ve güvenilirlik IOT Hub'ın kullanmaya devam edebilirsiniz.

Bu öğreticinin sonunda iki .NET konsol uygulamaları çalıştırın:

* **SimulatedDevice**, oluşturduğunuz uygulama değiştirilmiş bir sürümünü [IOT Hub ile bulut cihaza ileti gönderme](iot-hub-csharp-csharp-c2d.md) Öğreticisi. Bu uygulama, IOT hub tarafından sağlanan bir SAS URI'sini kullanarak depolama için bir dosya yükler.
* **ReadFileUploadNotification**, IOT hub'ından dosya karşıya yükleme bildirimlerini alır.

> [!NOTE]
> IOT hub'ı Azure IOT cihaz SDK'ları çok sayıda cihaz platformları ve (C, Java ve Javascript gibi) dilleri destekler. Başvurmak [Azure IOT Geliştirme Merkezi] Cihazınızı Azure IOT Hub'ına bağlamak adım adım yönergeler için.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2015 veya Visual Studio 2017
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Bir aygıt uygulamasından bir dosyayı karşıya yüklemek

Bu bölümde, oluşturduğunuz cihaz uygulamayı değiştirmek [IOT Hub ile bulut cihaza ileti gönderme](iot-hub-csharp-csharp-c2d.md) IOT hub'ından bulut cihaz iletileri almak için.

1. Visual Studio'da sağ **SimulatedDevice** proje, tıklatın **Ekle**ve ardından **varolan öğeyi**. Bir görüntü dosyasına gidin ve projenize ekleyin. Bu öğretici görüntü adlandırılan varsayar `image.jpg`.

1. Görüntüde sağ tıklayın ve ardından **özellikleri**. Olduğundan emin olun **çıktı dizinine Kopyala** ayarlanır **her zaman Kopyala**.

    ![][1]

1. İçinde **Program.cs** dosyasında, dosyanın üst kısmında aşağıdaki deyimleri ekleyin:

    ```csharp
    using System.IO;
    ```

1. **Program** sınıfına aşağıdaki yöntemi ekleyin:

    ```csharp
    private static async void SendToBlobAsync()
    {
        string fileName = "image.jpg";
        Console.WriteLine("Uploading file: {0}", fileName);
        var watch = System.Diagnostics.Stopwatch.StartNew();

        using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
        {
            await deviceClient.UploadToBlobAsync(fileName, sourceData);
        }

        watch.Stop();
        Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
    }
    ```

    `UploadToBlobAsync` Yöntemi dosya adı ve akış kaynak dosyası karşıya yüklenecek alır ve depolama için karşıya yükleme işler. Konsol uygulama dosyasını karşıya yüklemek için gereken süreyi görüntüler.

1. Aşağıdaki yöntemi ekleyin **ana** yöntemi, hemen önce `Console.ReadLine()` satır:

    ```csharp
    SendToBlobAsync();
    ```

> [!NOTE]
> Basitlik'ın artırmak amacıyla için bu öğreticiyi herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda yeniden deneme ilkelerini (üstel geri alma), önerilen MSDN makalesinde uygulamalıdır [geçici hata işleme].

## <a name="receive-a-file-upload-notification"></a>Dosya karşıya yükleme bildirimi

Bu bölümde IOT hub'dan dosya karşıya yükleme bildirim iletileri alan bir .NET konsol uygulaması yazma.

1. Geçerli Visual Studio çözümünde kullanarak bir Visual C# Windows projesi oluşturma **konsol uygulaması** proje şablonu. Proje adı **ReadFileUploadNotification**.

    ![Visual Studio'da yeni proje][2]

1. Çözüm Gezgini'nde sağ **ReadFileUploadNotification** proje ve ardından **NuGet paketlerini Yönet...** .

1. İçinde **NuGet Paket Yöneticisi** penceresinde, arama **Microsoft.Azure.Devices**, tıklatın **yükleme**ve kullanım koşullarını kabul edin.

    Bu eylem indirir, yükler ve bir başvuru ekler [Azure IOT hizmeti SDK'sı NuGet paketi] içinde **ReadFileUploadNotification** projesi.

1. İçinde **Program.cs** dosyasında, dosyanın üst kısmında aşağıdaki deyimleri ekleyin:

    ```csharp
    using Microsoft.Azure.Devices;
    ```

1. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini IOT hub bağlantı dizesi ile değiştirin [IOT Hub ile çalışmaya başlama]:

    ```csharp
    static ServiceClient serviceClient;
    static string connectionString = "{iot hub connection string}";
    ```

1. **Program** sınıfına aşağıdaki yöntemi ekleyin:

    ```csharp
    private async static void ReceiveFileUploadNotificationAsync()
    {
        var notificationReceiver = serviceClient.GetFileNotificationReceiver();

        Console.WriteLine("\nReceiving file upload notification from service");
        while (true)
        {
            var fileUploadNotification = await notificationReceiver.ReceiveAsync();
            if (fileUploadNotification == null) continue;

            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("Received file upload noticiation: {0}", string.Join(", ", fileUploadNotification.BlobName));
            Console.ResetColor();

            await notificationReceiver.CompleteAsync(fileUploadNotification);
        }
    }
    ```

    Bu alma düzeni aygıt uygulamadan bulut-cihaz iletilerini almak için kullanılan aynı olmasına dikkat edin.

1. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:

    ```csharp
    Console.WriteLine("Receive file upload notifications\n");
    serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
    ReceiveFileUploadNotificationAsync();
    Console.WriteLine("Press Enter to exit\n");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Visual Studio'da Çözümünüze sağ tıklayın ve seçin **başlangıç projelerini Ayarla**. Seçin **birden fazla başlangıç projesi**seçeneğini belirleyip **Başlat** eylemi için **ReadFileUploadNotification** ve **SimulatedDevice**.

1. Tuşuna **F5**. Her iki uygulamayı başlamanız gerekir. Bir konsol uygulamasında tamamlandı karşıya yükleme ve bir konsol uygulaması tarafından alınan karşıya yükleme bildirim iletisini görmeniz gerekir. Kullanabileceğiniz [Azure portal] veya Visual Studio Sunucu Gezgini aygıtını karşıya yüklenen dosyanın Azure depolama hesabınızdaki varlığını denetle.

    ![][50]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, dosya yüklemelerini basitleştirmek için IOT hub'ı dosya karşıya yükleme becerilerini kullanacak öğrendiniz. IOT hub özelliklerini ve aşağıdaki makalelerde senaryolarını keşfetmeye devam edebilirsiniz:

* [IOT hub'ı program aracılığıyla oluşturma][lnk-create-hub]
* [C SDK Giriş][lnk-c-sdk]
* [Azure IOT SDK'ları][lnk-sdks]

Daha fazla IOT hub'ı özelliklerini keşfetmek için bkz:

* [Azure IOT Edge ile sınır cihazlarına Al dağıtma][lnk-iotedge]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/file-upload-project-csharp1.png

<!-- Links -->

[Azure portal]: https://portal.azure.com/

[Azure IOT Geliştirme Merkezi]: http://azure.microsoft.com/develop/iot

[geçici hata işleme]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure IOT hizmeti SDK'sı NuGet paketi]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
