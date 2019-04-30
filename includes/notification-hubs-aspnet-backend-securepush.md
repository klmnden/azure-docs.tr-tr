---
author: spelluru
ms.service: service-bus
ms.topic: include
ms.date: 11/09/2018
ms.author: spelluru
ms.openlocfilehash: b8cf4217ca6c80be998b92e71c3ba29c4f68bce2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60874570"
---
## <a name="webapi-project"></a>Webapı projesi
1. Visual Studio'da açın **AppBackend** oluşturduğunuz proje **kullanıcılara bildirme** öğretici.
2. Notifications.cs sınıfında bütün değiştirin **bildirimleri** aşağıdaki kodla sınıfı. Yer tutucuları bildirim hub'ınıza ve hub adını, bağlantı dizesi (tam erişimli) değiştirdiğinizden emin olun. Bu değerleri edinebilirsiniz [Azure portalında](http://portal.azure.com). Bu modül, gönderilecek farklı güvenli bildirimler artık temsil eder. Tam bir uygulama bildirimleri bir veritabanında depolanır; Kolaylık olması için bu durumda bunları bellekte depolarız.
   
        public class Notification
        {
            public int Id { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }

        public class Notifications
        {
            public static Notifications Instance = new Notifications();

            private List<Notification> notifications = new List<Notification>();

            public NotificationHubClient Hub { get; set; }

            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",     "{hub name}");
            }

            public Notification CreateNotification(string payload)
            {
                var notification = new Notification() {
                Id = notifications.Count,
                Payload = payload,
                Read = false
                };

                notifications.Add(notification);

                return notification;
            }

            public Notification ReadNotification(int id)
            {
                return notifications.ElementAt(id);
            }
        }

1. NotificationsController.cs içindeki kodu değiştirin **NotificationsController** sınıf tanımını aşağıdaki kodla. Bu bileşen, cihazın güvenli bir şekilde bildirim almak bir yol uygular ve ayrıca cihazlarınıza güvenli bir anında iletme tetiklemek için (Bu öğreticinin amaçları doğrultusunda) bir yol sağlar. Bildirim bildirim hub'ına gönderirken, yalnızca bildirim (ve gerçek ileti yok) Kimliğine sahip bir ham bildirim göndereceğiz olduğunu unutmayın:
   
       public NotificationsController()
       {
           Notifications.Instance.CreateNotification("This is a secure notification!");
       }
   
       // GET api/notifications/id
       public Notification Get(int id)
       {
           return Notifications.Instance.ReadNotification(id);
       }
   
       public async Task<HttpResponseMessage> Post()
       {
           var secureNotificationInTheBackend = Notifications.Instance.CreateNotification("Secure confirmation.");
           var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;
   
           // windows
           var rawNotificationToBeSent = new Microsoft.Azure.NotificationHubs.WindowsNotification(secureNotificationInTheBackend.Id.ToString(),
                           new Dictionary<string, string> {
                               {"X-WNS-Type", "wns/raw"}
                           });
           await Notifications.Instance.Hub.SendNotificationAsync(rawNotificationToBeSent, usernameTag);
   
           // apns
           await Notifications.Instance.Hub.SendAppleNativeNotificationAsync("{\"aps\": {\"content-available\": 1}, \"secureId\": \"" + secureNotificationInTheBackend.Id.ToString() + "\"}", usernameTag);
   
           // gcm
           await Notifications.Instance.Hub.SendGcmNativeNotificationAsync("{\"data\": {\"secureId\": \"" + secureNotificationInTheBackend.Id.ToString() + "\"}}", usernameTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }


Unutmayın `Post` yöntemi artık göndermez bir bildirim. Bu, yalnızca bildirim kimliği ve hassas olmayan tüm içeriği içeren bir ham bildirim gönderir. Ayrıca, gönderme işleminin hatalara neden olabilecek gibi kimlik bilgileri, bildirim hub'ındaki yapılandırılmış olan olmayan platformlar için açıklama emin olun.

1. Artık bir Azure Web sitesi için bu uygulamayı tüm cihazlardan erişilebilir olması için yeniden dağıtacağız. **AppBackend** projesine sağ tıklayıp **Yayımla**’yı seçin.
2. Azure Web sitesi, yayımlama hedefi seçin. Azure hesabınızla oturum açın ve mevcut veya yeni bir Web sitesini seçin ve Not **hedef URL** özelliğinde **bağlantı** sekmesi. Bu URL'ye bu öğreticinin sonraki bölümlerinde *arka uca ait uç nokta* olarak başvuracağız. **Yayımla**’ta tıklayın.

