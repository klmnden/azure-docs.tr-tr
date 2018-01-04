## <a name="create-the-webapi-project"></a>Webapı projesi oluşturma
Sonraki bölümlerde yeni bir ASP.NET Webapı arka uç oluşturulması açıklanmaktadır. Bu işlem, üç ana amacı vardır:

* **İstemcilerin kimliğini**: daha sonra istemci isteklerinin kimliğini doğrulamak ve kullanıcı isteği ile ilişkilendirmek için ileti işleyicisi ekleyin.

* **Webapı kullanarak bildirim arka uç için kaydetme**: bildirimleri almak bir istemci aygıt için yeni kayıtlar işlemek için bir denetleyici ekleyin. Kimliği doğrulanmış kullanıcı kayıt otomatik olarak eklenen bir [etiketi](https://msdn.microsoft.com/library/azure/dn530749.aspx).

* **İstemciler için bildirimler Gönder**: Ayrıca cihazlara ve etiketiyle ilişkili istemcileri güvenli bir anında iletme tetiklemek bir yol sağlamak için bir denetleyici ekleyin. 

Yeni ASP.NET Webapı arka uç, aşağıdakileri yaparak oluşturun: 

> [!IMPORTANT]
> Visual Studio 2015 kullanıyorsanız veya önceki sürümleri, bu öğreticiye başlamadan önce emin olun, Visual Studio için NuGet Paket Yöneticisi'nin en son sürümü yüklü. 
>
>Denetlemek için Visual Studio’yu başlatın. Üzerinde **Araçları** menüsünde, select **Uzantılar ve güncelleştirmeler**. Arama **NuGet Paket Yöneticisi** Visual Studio emin olun ve sürümünüzde en son sürüme sahip. Sürümünüz en son sürümü değilse, bunu kaldırın ve NuGet Paket Yöneticisi'ni yeniden yükleyin.
 
![][B4]

> [!NOTE]
> Web sitesi dağıtımı için Visual Studio [Azure SDK](https://azure.microsoft.com/downloads/)’sını yüklediğinizden emin olun.
> 
> 

1. Visual Studio veya Visual Studio Express’i başlatın. 

2. Seçin **Sunucu Gezgini**ve Azure hesabınızda oturum açın. Hesabınızla ilgili web sitesini kaynak oluşturmak için oturum açmanız gerekir.

3. Visual Studio'da seçin **dosya** > **yeni** > **proje**, genişletin **şablonları**, genişletin**Visual C#**ve ardından **Web** ve **ASP.NET Web uygulaması**.

4. İçinde **adı** kutusuna **AppBackend**ve ardından **Tamam**. 
   
    ![Yeni Proje penceresinin][B1]

5. İçinde **yeni ASP.NET projesi** penceresinde, seçin **Web API** onay kutusunu işaretleyin ve ardından **Tamam**.
   
    ![Yeni ASP.NET projesi penceresi][B2]

6. İçinde **yapılandırma Microsoft Azure Web uygulaması** penceresinde, bir abonelik seçin ve ardından **uygulama hizmeti planı** listesinde, aşağıdakilerden birini yapın:

    * Önceden oluşturduğunuz bir uygulama hizmeti planı seçin. 
    * Seçin **yeni bir uygulama hizmeti planı oluştur**, ardından oluşturun. 
    
  Bu öğretici için bir veritabanı gerekmez. Uygulama hizmet planınız seçtikten sonra Seç **Tamam** projesi oluşturmak için.
   
    ![Microsoft Azure Web uygulaması yapılandırma penceresi][B5]

## <a name="authenticate-clients-to-the-webapi-back-end"></a>İstemciler için Webapı arka uç kimlik doğrulaması
Bu bölümde, oluşturduğunuz adlı yeni bir ileti işleyicisi sınıf **AuthenticationTestHandler** yeni arka uç için. Bu sınıfın türetildiği [DelegatingHandler](https://msdn.microsoft.com/library/system.net.http.delegatinghandler.aspx) ve böylece tüm işleyebilir ileti işleyicisi bu arka uç içine istekleri olarak eklendi. 

1. Çözüm Gezgini'nde sağ **AppBackend** proje, select **Ekle**ve ardından **sınıfı**. 
 
2. Yeni sınıf **AuthenticationTestHandler.cs**ve ardından **Ekle** sınıfı oluşturmak için. Bu sınıf, kullanarak kullanıcıların kimlik doğrulaması *temel kimlik doğrulaması* basitleştirmek için. Uygulamanız herhangi bir kimlik doğrulama düzeni kullanabilirsiniz.

3. AuthenticationTestHandler.cs sınıfına aşağıdaki `using` deyimlerini ekleyin:
   
        using System.Net.Http;
        using System.Threading;
        using System.Security.Principal;
        using System.Net;
        using System.Text;
        using System.Threading.Tasks;

4. AuthenticationTestHandler.cs içinde Değiştir `AuthenticationTestHandler` sınıf tanımını aşağıdaki kod ile: 
   
    Aşağıdaki üç koşullar doğru olduğunda işleyicisi isteği yetkilendirin.
   
   * İsteği içeren bir *yetkilendirme* üstbilgi. 
   * İstek *temel* kimlik doğrulaması kullanır. 
   * Kullanıcı adı dizesi ve parola dizesi aynı dizelerdir.
     
  Aksi takdirde istek reddedilir. Bu geçerli bir kimlik doğrulama ve yetkilendirme yaklaşımı değildir. Bu öğretici için yalnızca çok basit bir örnek olmasından.
     
  İstek iletisi ise ve kimlik doğrulaması tarafından yetkili `AuthenticationTestHandler`, temel kimlik doğrulaması kullanıcı üzerinde geçerli isteğe bağlı [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.current.aspx). Kullanıcı bilgilerini HttpContext başka bir denetleyici (RegisterController) tarafından daha sonra eklemek için kullanılacak bir [etiketi](https://msdn.microsoft.com/library/azure/dn530749.aspx) bildirimi kayıt isteği için.
     
       public class AuthenticationTestHandler : DelegatingHandler
       {
           protected override Task<HttpResponseMessage> SendAsync(
           HttpRequestMessage request, CancellationToken cancellationToken)
           {
               var authorizationHeader = request.Headers.GetValues("Authorization").First();
     
               if (authorizationHeader != null && authorizationHeader
                   .StartsWith("Basic ", StringComparison.InvariantCultureIgnoreCase))
               {
                   string authorizationUserAndPwdBase64 =
                       authorizationHeader.Substring("Basic ".Length);
                   string authorizationUserAndPwd = Encoding.Default
                       .GetString(Convert.FromBase64String(authorizationUserAndPwdBase64));
                   string user = authorizationUserAndPwd.Split(':')[0];
                   string password = authorizationUserAndPwd.Split(':')[1];
     
                   if (verifyUserAndPwd(user, password))
                   {
                       // Attach the new principal object to the current HttpContext object
                       HttpContext.Current.User =
                           new GenericPrincipal(new GenericIdentity(user), new string[0]);
                       System.Threading.Thread.CurrentPrincipal =
                           System.Web.HttpContext.Current.User;
                   }
                   else return Unauthorized();
               }
               else return Unauthorized();
     
               return base.SendAsync(request, cancellationToken);
           }
     
           private bool verifyUserAndPwd(string user, string password)
           {
               // This is not a real authentication scheme.
               return user == password;
           }
     
           private Task<HttpResponseMessage> Unauthorized()
           {
               var response = new HttpResponseMessage(HttpStatusCode.Forbidden);
               var tsc = new TaskCompletionSource<HttpResponseMessage>();
               tsc.SetResult(response);
               return tsc.Task;
           }
       }
     
     > [!NOTE]
     > Güvenlik Notu: `AuthenticationTestHandler` sınıfı doğru kimlik doğrulaması sağlamaz. Yalnızca temel kimlik doğrulamasını taklit etmek için kullanılır ve güvenli değildir. Üretim uygulamalarınızda ve hizmetlerinizde güvenli bir kimlik doğrulama mekanizması uygulamanız gerekir.                
     > 
     > 
5. İleti işleyicisi kaydetmek için sonuna aşağıdaki kodu ekleyin `Register` yönteminde **App_Start/WebApiConfig.cs** sınıfı:
   
        config.MessageHandlers.Add(new AuthenticationTestHandler());

6. Yaptığınız değişiklikleri kaydedin.

## <a name="register-for-notifications-by-using-the-webapi-back-end"></a>Webapı arka ucu kullanarak bildirim kaydolun
Bu bölümde, yeni bir denetleyicisi bildirim hub'ları için istemci kitaplığını kullanarak bir kullanıcı ve cihaz bildirimlerinin kaydettirmek için istekleri işlemek üzere Webapı arka uç ekleyin. Kimlik doğrulaması ve HttpContext tarafından bağlı kullanıcı için bir kullanıcı etiket denetleyicisi ekler `AuthenticationTestHandler`. Etiket `"username:<actual username>"` dize biçiminde olacaktır.

1. Çözüm Gezgini'nde sağ **AppBackend** proje ve ardından **NuGet paketlerini Yönet**.

2. Sol bölmede seçin **çevrimiçi** , daha sonra **arama** kutusuna **Microsoft.Azure.NotificationHubs**.

3. Sonuçlar listesinde **Microsoft Azure bildirim hub'ları**ve ardından **yükleme**. Yüklemeyi tamamlamak ve sonra da NuGet Paket Yöneticisi penceresini kapatın.
   
    Bu eylem <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet paketini</a> kullanarak Azure Notification Hubs SDK'sına bir başvuru ekler.

4. Bildirimleri göndermek için kullanılan bildirim hub'ı ile bağlantıyı temsil eden yeni bir sınıf dosyası oluşturun. Çözüm Gezgini'nde sağ **modelleri** klasöründe seçin **Ekle**ve ardından **sınıfı**. Yeni sınıf **Notifications.cs**ve ardından **Ekle** sınıfı oluşturmak için. 
   
    ![Yeni Öğe Ekle penceresi][B6]

5. Notifications.cs sınıfında aşağıdaki `using` deyimini dosyanın üst kısmına ekleyin:
   
        using Microsoft.Azure.NotificationHubs;

6. Değiştir `Notifications` sınıf tanımını aşağıdaki kodla ve iki yer tutucuları adlı bağlantı dizesi (tam erişimi) için bildirim hub'ınızı ve hub adı ile değiştirin (adresinde [Azure portal](http://portal.azure.com)):
   
        public class Notifications
        {
            public static Notifications Instance = new Notifications();
   
            public NotificationHubClient Hub { get; set; }
   
            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("<your hub's DefaultFullSharedAccessSignature>", 
                                                                             "<hub name>");
            }
        }
7. Ardından, adlı yeni bir denetleyici oluşturma **RegisterController**. Çözüm Gezgini'nde sağ **denetleyicileri** klasöründe seçin **Ekle**ve ardından **denetleyicisi**. 

8. Seçin **Web API 2 denetleyici - boş**ve ardından **Ekle**.
   
    ![İskele Ekle penceresi][B7]
   
9. İçinde **Denetleyici adı** kutusuna **RegisterController** yeni sınıf adını ve ardından **Ekle**.

    ![Denetleyici Ekle penceresi][B8]

10. RegisterController.cs sınıfına aşağıdaki `using` deyimlerini ekleyin:
   
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.NotificationHubs.Messaging;
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;

11. Aşağıdaki kodu `RegisterController` sınıf tanımına ekleyin. Bu kod, biz HttpContext için bağlı bir kullanıcı için bir kullanıcı etiketi eklemek unutmayın. Kullanıcı kimlik doğrulaması ve HttpContext için eklediğimiz, İleti Filtresi tarafından bağlı `AuthenticationTestHandler`. Ayrıca, kullanıcının istenen etiketlere kaydolma haklarının olduğunu doğrulamak için isteğe bağlı denetimler ekleyebilirsiniz.
   
        private NotificationHubClient hub;
   
        public RegisterController()
        {
            hub = Notifications.Instance.Hub;
        }
   
        public class DeviceRegistration
        {
            public string Platform { get; set; }
            public string Handle { get; set; }
            public string[] Tags { get; set; }
        }
   
        // POST api/register
        // This creates a registration id
        public async Task<string> Post(string handle = null)
        {
            string newRegistrationId = null;
   
            // make sure there are no existing registrations for this push handle (used for iOS and Android)
            if (handle != null)
            {
                var registrations = await hub.GetRegistrationsByChannelAsync(handle, 100);
   
                foreach (RegistrationDescription registration in registrations)
                {
                    if (newRegistrationId == null)
                    {
                        newRegistrationId = registration.RegistrationId;
                    }
                    else
                    {
                        await hub.DeleteRegistrationAsync(registration);
                    }
                }
            }
   
            if (newRegistrationId == null) 
                newRegistrationId = await hub.CreateRegistrationIdAsync();
   
            return newRegistrationId;
        }
   
        // PUT api/register/5
        // This creates or updates a registration (with provided channelURI) at the specified id
        public async Task<HttpResponseMessage> Put(string id, DeviceRegistration deviceUpdate)
        {
            RegistrationDescription registration = null;
            switch (deviceUpdate.Platform)
            {
                case "mpns":
                    registration = new MpnsRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "wns":
                    registration = new WindowsRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "apns":
                    registration = new AppleRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "gcm":
                    registration = new GcmRegistrationDescription(deviceUpdate.Handle);
                    break;
                default:
                    throw new HttpResponseException(HttpStatusCode.BadRequest);
            }
   
            registration.RegistrationId = id;
            var username = HttpContext.Current.User.Identity.Name;
   
            // add check if user is allowed to add these tags
            registration.Tags = new HashSet<string>(deviceUpdate.Tags);
            registration.Tags.Add("username:" + username);
   
            try
            {
                await hub.CreateOrUpdateRegistrationAsync(registration);
            }
            catch (MessagingException e)
            {
                ReturnGoneIfHubResponseIsGone(e);
            }
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
        // DELETE api/register/5
        public async Task<HttpResponseMessage> Delete(string id)
        {
            await hub.DeleteRegistrationAsync(id);
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
        private static void ReturnGoneIfHubResponseIsGone(MessagingException e)
        {
            var webex = e.InnerException as WebException;
            if (webex.Status == WebExceptionStatus.ProtocolError)
            {
                var response = (HttpWebResponse)webex.Response;
                if (response.StatusCode == HttpStatusCode.Gone)
                    throw new HttpRequestException(HttpStatusCode.Gone.ToString());
            }
        }
12. Yaptığınız değişiklikleri kaydedin.

## <a name="send-notifications-from-the-webapi-back-end"></a>Webapı arka ucunuzdan bildirim gönderme
Bu bölümde, bildirim göndermek istemci cihazları için bir yol sunan yeni bir denetleyici ekleyin. Bildirim Azure bildirim hub'ları Hizmet Yönetimi Kitaplığı ASP.NET Webapı arka uç kullanan kullanıcı adı etiketi temel alır.

1. Adlı başka bir yeni denetleyicisi oluşturmak **NotificationsController** oluşturduğunuz aynı şekilde **RegisterController** önceki bölümdeki.

2. NotificationsController.cs sınıfına aşağıdaki `using` deyimlerini ekleyin:
   
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;

3. Aşağıdaki yöntemi ekleyin **NotificationsController** sınıfı:
   
    Bu kod Platform bildirim hizmeti (PNS) dayalı bir bildirim türü gönderir `pns` parametresi. `to_tag` değeri, iletideki *kullanıcı adı* etiketini ayarlamak için kullanılır. Bu etiket, etkin bir bildirim hub'ı kaydının kullanıcı etiketi ile eşleşmelidir. Bildirim iletisi, POST isteğinin gövdesinden çekilir ve hedef PNS biçimlendirilir. 
   
    Bildirimleri almak için desteklenen aygıtların kullanmak PNS bağlı olarak, bildirimler, çeşitli biçimlerde tarafından desteklenir. Örneğin, Windows cihazlarda kullanabileceğinize bir [kutlayın bildirim WNS ile](https://msdn.microsoft.com/library/windows/apps/br230849.aspx) başka bir PNS tarafından doğrudan desteklenmeyen. Bu tür bir örnekte, arka uç desteklemeyi planladığınız bildirim desteklenen bildirim içine için PNS'ye aygıtların biçimlendirmek gerekir. Ardından uygun gönderme API kullanımı [NotificationHubClient sınıfı](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.notificationhubclient_methods.aspx).
   
        public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag)
        {
            var user = HttpContext.Current.User.Identity.Name;
            string[] userTag = new string[2];
            userTag[0] = "username:" + to_tag;
            userTag[1] = "from:" + user;
   
            Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;
            HttpStatusCode ret = HttpStatusCode.InternalServerError;
   
            switch (pns.ToLower())
            {
                case "wns":
                    // Windows 8.1 / Windows Phone 8.1
                    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" + 
                                "From " + user + ": " + message + "</text></binding></visual></toast>";
                    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
                    break;
                case "apns":
                    // iOS
                    var alert = "{\"aps\":{\"alert\":\"" + "From " + user + ": " + message + "\"}}";
                    outcome = await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(alert, userTag);
                    break;
                case "gcm":
                    // Android
                    var notif = "{ \"data\" : {\"message\":\"" + "From " + user + ": " + message + "\"}}";
                    outcome = await Notifications.Instance.Hub.SendGcmNativeNotificationAsync(notif, userTag);
                    break;
            }
   
            if (outcome != null)
            {
                if (!((outcome.State == Microsoft.Azure.NotificationHubs.NotificationOutcomeState.Abandoned) ||
                    (outcome.State == Microsoft.Azure.NotificationHubs.NotificationOutcomeState.Unknown)))
                {
                    ret = HttpStatusCode.OK;
                }
            }
   
            return Request.CreateResponse(ret);
        }

4. Uygulamayı çalıştırın ve o ana kadarki doğruluğunu sağlamak için seçin **F5** anahtarı. Uygulama bir web tarayıcısı açar ve ASP.NET giriş sayfasında görüntülenir. 

## <a name="publish-the-new-webapi-back-end"></a>Yeni Webapı arka uç yayımlama
Ardından, tüm cihazlar üzerinden erişilebilir olması için bir Azure Web sitesine uygulama dağıtın. 

1. Sağ **AppBackend** proje ve ardından **Yayımla**.

2. Seçin **Microsoft Azure App Service** yayımlama hedefi ve ardından olarak **Yayımla**.  
    App Service Oluştur penceresi açılır. Burada, Azure üzerinde ASP.NET web uygulamasını çalıştırmak için gereken tüm Azure kaynakların oluşturabilirsiniz.

    ![Microsoft Azure App Service döşeme][B15]

3. İçinde **App Service Oluştur** penceresinde, Azure hesabınızı seçin. Seçin **türünü** > **Web uygulaması**. Varsayılan tutmak **Web uygulaması adı**ve ardından **abonelik**, **kaynak grubu**, ve **App Service planı**. 

4. **Oluştur**’u seçin.

5. **Özet** bölümündeki **Site URL** özelliğini not edin. Bu URL, *arka uç endpoint* öğreticide daha sonra. 

6. Seçin **yayımlama**.

Sihirbazı tamamladıktan sonra ASP.NET web uygulaması için Azure yayımlar ve ardından varsayılan tarayıcıda uygulama açar.  Uygulamanızı Azure App Services görülebilir.

URL daha önce belirttiğiniz web uygulaması adını http://<uygulama_adı>.azurewebsites.net biçimiyle kullanır.

[B1]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push1.png
[B2]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push2.png
[B3]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push3.png
[B4]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push4.png
[B5]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push5.png
[B6]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push6.png
[B7]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push7.png
[B8]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push8.png
[B14]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push14.png
[B15]: ./media/notification-hubs-aspnet-backend-notifyusers/publish-to-app-service.png
[B16]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-notify-users16.PNG
[B18]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-notify-users18.PNG
