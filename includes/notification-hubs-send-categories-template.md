
Bu bölümde, son dakika haberleri bir .NET konsol uygulamasından etiketli olarak şablon bildirimleri gönderin.

Microsoft Azure App Service Mobile Apps özelliğini kullanıyorsanız, başvurmak [Mobile Apps için anında iletme bildirimleri ekleme] öğretici, üst platformunuzu seçin.

Java veya PHP kullanmak istiyorsanız, başvurmak [Notification Hubs Java veya PHP kullanma]. Kullanarak bir arka uçtan bildirimleri gönderebilir [bildirim hub'ları REST arabirimini].

Tamamlandığında, bildirim göndermek için konsol uygulamasını oluşturduysanız [Notification Hubs ile çalışmaya başlama], 1-3 adımları atlayın.

1. Visual Studio'da yeni bir Visual C# konsol uygulaması oluşturun:
   
      ![Konsol uygulaması bağlantı][13]

2. Visual Studio ana menüde seçin **Araçları** > **kitaplık Paket Yöneticisi** > **Paket Yöneticisi Konsolu** ve ardından konsol penceresinde, aşağıdaki dizeyi girin:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
3. Seçin **girin**.  
    Bu eylem [Microsoft.Azure.Notification Hubs NuGet paketini] kullanarak Azure Notification Hubs SDK'sına bir başvuru ekler.

4. Program.cs dosyasını açın ve aşağıdakileri ekleyin `using` deyimi:
   
        using Microsoft.Azure.NotificationHubs;

5. İçinde `Program` sınıfı, aşağıdaki yöntemi ekleyin veya zaten varsa değiştirin:
   
        private static async void SendTemplateNotificationAsync()
        {
            // Define the notification hub.
            NotificationHubClient hub =
                NotificationHubClient.CreateClientFromConnectionString(
                    "<connection string with full access>", "<hub name>");
   
            // Create an array of breaking news categories.
            var categories = new string[] { "World", "Politics", "Business",
                                            "Technology", "Science", "Sports"};
   
            // Send the notification as a template notification. All template registrations that contain
            // "messageParam" and the proper tags will receive the notifications.
            // This includes APNS, GCM, WNS, and MPNS template registrations.
   
            Dictionary<string, string> templateParams = new Dictionary<string, string>();
   
            foreach (var category in categories)
            {
                templateParams["messageParam"] = "Breaking " + category + " News!";
                await hub.SendTemplateNotificationAsync(templateParams, category);
            }
         }
   
    Bu kod, her bir dize dizisi altı etiketleri için bir şablon bildirim gönderir. Aygıtları bildirimlerin yalnızca kayıtlı kategorileri için etiketler kullanımını sağlar.

5. Önceki kodla `<hub name>` ve `<connection string with full access>` , bildirim hub'ı adı ve bağlantı dizesinde yer tutucularını *DefaultFullSharedAccessSignature* bildirim hub'ınızın panosundan.

6. **Ana** yöntemine aşağıdaki satırları ekleyin:
   
         SendTemplateNotificationAsync();
         Console.ReadLine();

7. Konsol uygulaması oluşturun.

<!-- Images. -->
[13]: ./media/notification-hubs-back-end/notification-hub-create-console-app.png

<!-- URLs. -->
[Notification Hubs ile çalışmaya başlama]: ../articles/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[bildirim hub'ları REST arabirimini]: http://msdn.microsoft.com/library/windowsazure/dn223264.aspx
[Mobile Apps için anında iletme bildirimleri ekleme]: ../articles/app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md
[Notification Hubs Java veya PHP kullanma]: ../articles/notification-hubs/notification-hubs-java-push-notification-tutorial.md
[Microsoft.Azure.Notification Hubs NuGet paketini]: http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/
