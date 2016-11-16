## <a name="send-messages-to-event-hubs"></a>Event Hubs’a ileti gönderme
Bu bölümde, Olay Hub'ınıza olayları gönderen Windows konsol uygulamasını yazacaksınız.

1. Visual Studio'da, **Konsol Uygulaması** proje şablonunu kullanarak yeni Visual C# Masaüstü Uygulaması projesi oluşturun. Projeyi **Gönderen** için bir ad verin.
   
    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp1.png)
2. Çözüm Gezgini'nde çözüme sağ tıklayın ve ardından **Çözüm için NuGet Paketlerini Yönet**'e tıklayın. 
3. **Gözat** sekmesine tıklayıp `Microsoft Azure Service Bus` aramasını gerçekleştirin. Proje adının (**Gönderen**), **Sürüm(ler)** kutusunda belirtildiğinden emin olun. **Yükle**'ye tıklayın ve kullanım koşullarını kabul edin. 
   
    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp2.png)
   
    Visual Studio, [Azure Service Bus kitaplığı NuGet paketini](https://www.nuget.org/packages/WindowsAzure.ServiceBus) indirir, yükler ve ona bir başvuru ekler.
4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
   
    ```
    using System.Threading;
    using Microsoft.ServiceBus.Messaging;
    ```
5. Aşağıdaki alanları **Program** sınıfına ekleyin; bu işlemi yaparken yer tutucu değerlerini önceki bölümde oluşturduğunuz Olay Hub’ı adıyla ve daha önce kaydettiğiniz ad alanı düzeyinde bağlantı dizesiyle değiştirin.
   
    ```
    static string eventHubName = "{Event Hub name}";
    static string connectionString = "{send connection string}";
    ```
6. **Program** sınıfına aşağıdaki yöntemi ekleyin:
   
    ```
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
   
    ```
    Console.WriteLine("Press Ctrl-C to stop the sender process");
    Console.WriteLine("Press Enter to start now");
    Console.ReadLine();
    SendingRandomMessages();
    ```



<!--HONumber=Nov16_HO2-->


