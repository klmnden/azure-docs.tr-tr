---
title: ".NET Framework kullanarak olayları Azure Event Hubs’a gönderme | Microsoft Belgeleri"
description: ".NET Framework kullanarak Event Hubs&quot;a olay göndermeye başlama"
services: event-hubs
documentationcenter: 
author: jtaubensee
manager: timlt
editor: 
ms.assetid: c4974bd3-2a79-48a1-aa3b-8ee2d6655b28
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/08/2017
ms.author: jotaub;sethm
translationtype: Human Translation
ms.sourcegitcommit: cfe4957191ad5716f1086a1a332faf6a52406770
ms.openlocfilehash: 5c326a025a4276ae9b1a777439ed6c728d3e7103
ms.lasthandoff: 03/09/2017


---
# <a name="send-events-to-azure-event-hubs-using-the-net-framework"></a>.NET Framework kullanarak olayları Azure Event Hubs’a gönderme

## <a name="introduction"></a>Giriş
Event Hubs bağlı cihaz ve uygulamalardan büyük miktarlarda olay verileri (telemetri) işleyen bir hizmettir. Verileri Event Hubs’a topladıktan sonra bir depolama kümesi kullanarak depolayabilir veya gerçek zamanlı bir analiz sağlayıcısı kullanarak dönüştürebilirsiniz. Bu büyük ölçekli olay toplama ve işleme özelliği, Nesnelerin İnterneti (IoT) gibi modern uygulama mimarilerinin temel bir bileşenidir.

Bu öğretici, [Azure portalının](https://portal.azure.com) bir Olay Hub'ı oluşturmak için nasıl kullanılacağını gösterir. Ayrıca .NET Framework kullanılarak C# dilinde yazılmış bir konsol uygulaması ile bir Olay Hub’ına olay gönderme işlemini gösterir. .NET Framework kullanarak olayları almak için [.NET Framework kullanarak olay alma](event-hubs-dotnet-framework-getstarted-receive-eph.md) makalesine bakın veya soldaki içindekiler bölümünden uygun alma diline tıklayın.

Bu öğreticiyi tamamlamak için şunlar gerekir:

* [Microsoft Visual Studio 2015 veya üzeri](http://visualstudio.com). Bu öğreticideki ekran görüntülerinde Visual Studio 2017 kullanılır.
* Etkin bir Azure hesabı. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir hesap oluşturabilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme](https://azure.microsoft.com/free/).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Event Hubs ad alanı ve bir Olay Hub’ı oluşturma

İlk adımda [Azure portalını](https://portal.azure.com) kullanarak Event Hubs türünde bir ad alanı oluşturun, ardından uygulamanızın Olay Hub’ı ile iletişim kurması için gereken yönetim kimlik bilgilerini edinin. Bir ad alanı ve Olay Hub'ı oluşturmak için [bu makalede](event-hubs-create.md) verilen yordamı izleyin, ardından aşağıdaki adımlarla devam edin.

## <a name="create-a-console-application"></a>Konsol uygulaması oluşturma
Bu bölümde, Olay Hub'ınıza olayları gönderen Windows konsol uygulamasını yazacaksınız.

1. Visual Studio'da, **Konsol Uygulaması** proje şablonunu kullanarak yeni bir Visual C# Masaüstü Uygulaması projesi oluşturun. Projeyi **Gönderen** için bir ad verin.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp1.png)
2. Çözüm Gezgini'nde **Gönderen** projesine sağ tıklayın ve ardından **Çözüm için NuGet Paketlerini Yönet**'e tıklayın. 
3. **Gözat** sekmesine tıklayıp `Microsoft Azure Service Bus` için arama yapın. **Yükle**'ye tıklayın ve kullanım koşullarını kabul edin. 
   
    ![](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp2.png)
   
    Visual Studio, [Azure Service Bus kitaplığı NuGet paketini](https://www.nuget.org/packages/WindowsAzure.ServiceBus) indirir, yükler ve ona bir başvuru ekler.
4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
   
    ```csharp
    using System.Threading;
    using Microsoft.ServiceBus.Messaging;
    ```
5. Aşağıdaki alanları **Program** sınıfına ekleyin; bu işlemi yaparken yer tutucu değerlerini önceki bölümde oluşturduğunuz Olay Hub’ı adıyla ve daha önce kaydettiğiniz ad alanı düzeyinde bağlantı dizesiyle değiştirin.
   
    ```csharp
    static string eventHubName = "{Event Hub name}";
    static string connectionString = "{send connection string}";
    ```
6. **Program** sınıfına aşağıdaki yöntemi ekleyin:
   
    ```csharp
    static void SendingRandomMessages()
    {
        var eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, eventHubName);
        while (true)
        {
            try
            {
                var message = Guid.NewGuid().ToString();
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, message);
                eventHubClient.Send(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("{0} > Exception: {1}", DateTime.Now, exception.Message);
                Console.ResetColor();
            }
   
            Thread.Sleep(200);
        }
    }
    ```
   
    Bu yöntem, olayları 200 ms'lik bir gecikmeyle sürekli olarak Olay Hub'ınıza gönderir.
7. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:
   
    ```csharp
    Console.WriteLine("Press Ctrl-C to stop the sender process");
    Console.WriteLine("Press Enter to start now");
    Console.ReadLine();
    SendingRandomMessages();
    ```
8. Programı çalıştırın ve herhangi bir hata olmadığından emin olun.
  
Tebrikler! Bir Olay Hub'ına ileti gönderdiniz.

## <a name="next-steps"></a>Sonraki adımlar
Event Hub'ı oluşturan ve veri gönderen bir çalışan uygulama oluşturduğunuza göre aşağıdaki senaryolara geçebilirsiniz:

* [Olay İşlemcisi Konağı kullanarak olay alma](event-hubs-dotnet-framework-getstarted-receive-eph.md)
* [Olay İşlemcisi Konağı başvurusu](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)
* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png


