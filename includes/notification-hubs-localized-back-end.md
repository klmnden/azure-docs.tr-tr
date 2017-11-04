



Yalnızca bir özellikler kümesi sağlamanız gereken Şablon Bildirimleri gönderirken örneğimizde örneği için güncel haberleri yerelleştirilmiş sürümünü içeren özellikler kümesini göndereceğiz:

    {
        "News_English": "World News in English!",
        "News_French": "World News in French!",
        "News_Mandarin": "World News in Mandarin!"
    }


Bu bölümde bir konsol uygulaması kullanarak bildirim göndermek nasıl gösterir

Arka uç'ın herhangi bir desteklenen aygıtlar için yayın beri Windows mağazası ve iOS aygıtları için eklenen kod yayınlar.

### <a name="to-send-notifications-using-a-c-console-app"></a>Bir C# konsol uygulaması kullanarak bildirim göndermek için
Değiştirme `SendTemplateNotificationAsync` aşağıdaki kod ile daha önce oluşturduğunuz konsol uygulaması yöntemi. Nasıl bu durumda farklı yerel ayarlara ve platformlar için birden fazla bildirim göndermek için gerek yoktur dikkat edin.

        private static async void SendTemplateNotificationAsync()
        {
            // Define the notification hub.
            NotificationHubClient hub = 
                NotificationHubClient.CreateClientFromConnectionString(
                    "<connection string with full access>", "<hub name>");

            // Sending the notification as a template notification. All template registrations that contain 
            // "messageParam" or "News_<local selected>" and the proper tags will receive the notifications. 
            // This includes APNS, GCM, WNS, and MPNS template registrations.
            Dictionary<string, string> templateParams = new Dictionary<string, string>();

            // Create an array of breaking news categories.
            var categories = new string[] { "World", "Politics", "Business", "Technology", "Science", "Sports"};
            var locales = new string[] { "English", "French", "Mandarin" };

            foreach (var category in categories)
            {
                templateParams["messageParam"] = "Breaking " + category + " News!";

                // Sending localized News for each tag too...
                foreach( var locale in locales)
                {
                    string key = "News_" + locale;

                    // Your real localized news content would go here.
                    templateParams[key] = "Breaking " + category + " News in " + locale + "!";
                }

                await hub.SendTemplateNotificationAsync(templateParams, category);
            }
        }


Bu basit aramayı haberleri ile yerelleştirilmiş parçası dağıtılacak Not **tüm** aygıtlarınızın platformunun, bildirim Hub'ınızı oluşturur ve tüm aygıtlar için belirli bir abone doğru yerel yükü teslim gibi belirtilmediğine etiketi.

### <a name="sending-the-notification-with-mobile-services"></a>Mobile Services bildirim gönderme
Mobil hizmeti Zamanlayıcısı'nda aşağıdaki betiği kullanabilirsiniz:

    var azure = require('azure');
    var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string with full access>');
    var notification = {
            "News_English": "World News in English!",
            "News_French": "World News in French!",
            "News_Mandarin", "World News in Mandarin!"
    }
    notificationHubService.send('World', notification, function(error) {
        if (!error) {
            console.warn("Notification successful");
        }
    });


