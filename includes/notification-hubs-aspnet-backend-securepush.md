## <a name="webapi-project"></a>Webapı proje
1. Visual Studio'da açın **AppBackend** oluşturduğunuz proje **kullanıcılara bildirme** Öğreticisi.
2. Tüm Notifications.cs içinde Değiştir **bildirimleri** aşağıdaki kodla sınıfı. Yer tutucuları bağlantı dizenizi (tam erişimli) için bildirim hub'ınızı ve hub adı ile değiştirdiğinizden emin olun. Bu değerleri elde edebilirsiniz [Azure portal](http://portal.azure.com). Bu modül şimdi gönderilecek farklı güvenli bildirimleri temsil eder. Tam bir uygulama bildirimleri bir veritabanında depolanır; Kolaylık olması için bu durumda bunları bellekte depolarız.
   
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

1. Kod içinde NotificationsController.cs içinde değiştirin **NotificationsController** sınıf tanımını aşağıdaki kod ile. Bu bileşen, cihazın güvenli bir şekilde bildirim almak bir yol uygular ve ayrıca cihazlarınıza güvenli push tetiklemek için (Bu öğreticinin amaçları doğrultusunda) bir yol sağlar. Bildirim hub'ına bildirimi gönderirken, biz yalnızca ham bildirim kimliği bildirim (ve gerçek ileti yok) göndermek olduğunu unutmayın:
   
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


Unutmayın `Post` yöntemi şimdi göndermez bildirim. Yalnızca bildirim kimliği ve hassas olmayan tüm içeriği içeren bir ham bildirim gönderir. Ayrıca, gönderme işlemi hatalarına neden şekilde, bildirim hub'ına, yapılandırılmış kimlik bilgilerine sahip olduğunuz değil platformlar için açıklama emin olun.

1. Şimdi Biz bu uygulamayı bir Azure Web sitesine tüm cihazlar üzerinden erişilebilir olması için yeniden dağıtır. **AppBackend** projesine sağ tıklayıp **Yayımla**’yı seçin.
2. Azure Web sitesi yayımlama hedefi olarak seçin. Azure hesabınızla oturum açın ve mevcut veya yeni bir Web sitesini seçin ve Not **hedef URL** özelliğinde **bağlantı** sekmesi. Bu URL'ye bu öğreticinin sonraki bölümlerinde *arka uca ait uç nokta* olarak başvuracağız. **Yayımla**’ta tıklayın.

