## <a name="create-the-webapi-project"></a>WebAPI Projesi Oluşturma
Aşağıdaki bölümlerde yeni bir ASP.NET WebAPI arka ucu oluşturulur ve bu işlemi üç temel amacı vardır:

1. **İstemcilerin Kimliğini Doğrulama**: İstemci isteklerinin kimliğini doğrulamak ve kullanıcıyı istekle ilişkilendirmek üzere daha sonra bir ileti işleyicisi eklenir.
2. **İstemci Bildirimi Kayıtları**: Daha sonra, bir istemci cihazının bildirimleri alması için yeni kayıtları işlemek üzere bir denetleyici ekleyeceksiniz. Kimliği doğrulanmış kullanıcı adı, kayda bir [etiket](https://msdn.microsoft.com/library/azure/dn530749.aspx) olarak eklenir.
3. **İstemcilere Bildirimleri Gönderme**: Daha sonra, kullanıcının etiketle ilişkili cihazlara ve istemcilere güvenli bir iletmeyi tetiklemesi için yeni bir yol sağlamak üzere bir denetleyici de ekleyeceksiniz. 

Aşağıdaki adımlar yeni ASP.NET WebAPI arka ucunun nasıl oluşturulacağını gösterir: 

> [!NOTE]
> **Önemli**: Visual Studio 2015 veya önceki bir sürümü kullanıyorsanız, bu öğreticiye başlamadan önce lütfen NuGet Paket Yöneticisi’nin en son sürümünü yüklediğinizden emin olun. Denetlemek için Visual Studio’yu başlatın. **Araçlar** menüsünde **Uzantılar ve Güncelleştirmeler**’e tıklayın. Visual Studio sürümünüz için **NuGet Paket Yöneticisi** araması yapın ve en son sürümü kullandığınızdan emin olun. En son sürümü kullanmıyorsanız, NuGet Paket Yöneticisi'ni yeniden yükleyin.
> 
> ![][B4]
> 
> [!NOTE]
> Web sitesi dağıtımı için Visual Studio [Azure SDK](https://azure.microsoft.com/downloads/)’sını yüklediğinizden emin olun.
> 
> 

1. Visual Studio veya Visual Studio Express’i başlatın. **Sunucu Gezgini**’ne tıklayıp Azure hesabınızda oturum açın. Visual Studio, hesabınızda web sitesi kaynakları oluşturmak için oturum açmanızı gerektirir.
2. Visual Studio’da sırasıyla **Dosya**, **Yeni** ve **Proje**’ye tıklayın, **Şablonlar**’ı ve **Visual C#**’yi genişletip **Web** ve **ASP.NET Web Uygulaması**’na tıklayın, **AppBackend** adını yazın ve ardından **Tamam**’a tıklayın. 
   
    ![][B1]
3. **Yeni ASP.NET Projesi** iletişim kutusunda **Web API**’sine ve ardından **Tamam**’a tıklayın.
   
    ![][B2]
4. **Microsoft Azure Web Uygulaması** iletişim kutusunda bir aboneliği ve daha önce oluşturduğunuz **App Service planını** seçin. Ayrıca **Yeni bir app service planı oluştur**’u seçip iletişim kutusundan bir plan oluşturabilirsiniz. Bu öğretici için bir veritabanı gerekmez. App service planınızı seçtikten sonra projeyi oluşturmak için **Tamam**’a tıklayın.
   
    ![][B5]

## <a name="authenticating-clients-to-the-webapi-backend"></a>WebAPI Arka Ucunda İstemcilerin Kimliğini Doğrulama
Bu bölümde, yeni arka uç için **AuthenticationTestHandler** adlı yeni bir ileti işleyicisi oluşturacaksınız. Bu sınıf [DelegatingHandler](https://msdn.microsoft.com/library/system.net.http.delegatinghandler.aspx)’dan türetilip bir ileti işleyicisi olarak eklenir, böylece arka uca gelen istekleri işleyebilir. 

1. Çözüm Gezgini'nde **AppBackend** projesine sağ tıklayın, **Ekle**'ye ve ardından **Sınıf**'a tıklayın. Yeni **AuthenticationTestHandler.cs** sınıfını adlandırıp **Ekle**’ye tıklayarak sınıfı oluşturun. Bu sınıf, kolaylık için *Temel Kimlik Doğrulaması* kullanan kullanıcıların kimliğini doğrulamak amacıyla kullanılır. Uygulamanız herhangi bir kimlik doğrulama şemasını kullanabilir.
2. AuthenticationTestHandler.cs sınıfına aşağıdaki `using` deyimlerini ekleyin:
   
        using System.Net.Http;
        using System.Threading;
        using System.Security.Principal;
        using System.Net;
        using System.Text;
        using System.Threading.Tasks;

3. AuthenticationTestHandler.cs sınıfında `AuthenticationTestHandler` sınıf tanımını aşağıdaki kod ile değiştirin. 
   
    Bu işleyici aşağıdaki üç koşulun tümü geçerli olduğunda bu isteği yetkilendirir:
   
   * İstek bir *Yetkilendirme* üst bilgisi içerir. 
   * İstek *temel* kimlik doğrulaması kullanır. 
   * Kullanıcı adı dizesi ve parola dizesi aynı dizelerdir.
     
     Aksi takdirde istek reddedilir. Bu geçerli bir kimlik doğrulama ve yetkilendirme yaklaşımı değildir. Yalnızca bu öğreticiye yönelik çok basit bir örnektir.
     
     İstek iletisi `AuthenticationTestHandler` tarafından yetkilendirilir ve kimlik doğrulaması yapılırsa, temel kimlik doğrulaması kullanıcısı [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.current.aspx) üzerindeki geçerli isteğe eklenir. HttpContext içindeki kullanıcı bilgileri, bildirim kaydı isteğine bir [etiket](https://msdn.microsoft.com/library/azure/dn530749.aspx) eklemek amacıyla daha sonra başka bir denetleyici (RegisterController) tarafından kullanılır.
     
       public class AuthenticationTestHandler : DelegatingHandler   {       protected override Task<HttpResponseMessage> SendAsync(       HttpRequestMessage request, CancellationToken cancellationToken)       {           var authorizationHeader = request.Headers.GetValues("Authorization").First();
     
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
     > **Güvenlik Notu**: `AuthenticationTestHandler` sınıfı gerçek kimlik doğrulaması sağlamaz. Yalnızca temel kimlik doğrulamasını taklit etmek için kullanılır ve güvenli değildir. Üretim uygulamalarınızda ve hizmetlerinizde güvenli bir kimlik doğrulama mekanizması uygulamanız gerekir.                
     > 
     > 
4. İleti işleyicisini kaydetmek için **App_Start/WebApiConfig.cs** sınıfındaki `Register` yönteminin sonuna aşağıdaki kodu ekleyin:
   
        config.MessageHandlers.Add(new AuthenticationTestHandler());
5. Yaptığınız değişiklikleri kaydedin.

## <a name="registering-for-notifications-using-the-webapi-backend"></a>WebAPI Arka Ucunu Kullanarak Bildirimlere Kaydolma
Bu bölümde, bildirim hub’ları için istemci kitaplığını kullanarak WebAPI arka ucuna bir kullanıcı ve cihazı bildirimlere kaydetme isteklerini işlemek üzere yeni bir denetleyici ekleyeceğiz. Denetleyici, kimliği doğrulanmış ve `AuthenticationTestHandler` tarafından HttpContext’e eklenmiş kullanıcı için bir kullanıcı etiketi ekler. Etiket `"username:<actual username>"` dize biçiminde olacaktır.

1. Çözüm Gezgini'nde, **AppBackend** projesine sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'e tıklayın.
2. Sol taraftaki **Çevrimiçi** öğesine tıklayıp, **Ara** kutusunda **Microsoft.Azure.NotificationHubs** araması yapın.
3. Sonuç listesinde **Microsoft Azure Notification Hubs**’a ve ardından **Yükle**’ye tıklayın. Yüklemeyi tamamlayın ve sonra NuGet paket yöneticisi penceresini kapatın.
   
    Bu, <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet paketini</a> kullanarak Azure Notification Hubs SDK'sına bir başvuru ekler.
4. Bildirimleri göndermek için bildirim hub'ı kullanarak bağlantıyı temsil eden yeni bir sınıf dosyası oluşturacağız. Çözüm Gezgini'nde **Modeller** klasörüne sağ tıklayın, **Ekle**'ye ve ardından **Sınıf**'a tıklayın. Yeni sınıfı **Notifications.cs** olarak adlandırın, ardından **Ekle**’ye tıklayarak sınıfı oluşturun. 
   
    ![][B6]
5. Notifications.cs sınıfında aşağıdaki `using` deyimini dosyanın üst kısmına ekleyin:
   
        using Microsoft.Azure.NotificationHubs;
6. `Notifications` sınıf tanımını aşağıdakiyle değiştirin ve iki yer tutucuyu bildirim hub’ınızın bağlantı dizesi ile (tam erişimli) değiştirdiğinizden emin olun ([Klasik Azure Portalında bulunabilir](http://manage.windowsazure.com)):
   
        public class Notifications
        {
            public static Notifications Instance = new Notifications();
   
            public NotificationHubClient Hub { get; set; }
   
            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("<your hub's DefaultFullSharedAccessSignature>", 
                                                                             "<hub name>");
            }
        }
7. Bundan sonra **RegisterController** adlı yeni bir denetleyici oluşturacağız. Çözüm Gezgini'nde **Denetleyiciler** klasörüne sağ tıklayın, **Ekle**'ye ve ardından **Denetleyici**'ye tıklayın. **Web API 2 Denetleyicisi -- Boş** öğesine ve ardından **Ekle**’ye tıklayın. Yeni sınıfı **RegisterController** olarak adlandırın ve ardından **Ekle**’ye tekrar tıklayarak denetleyiciyi oluşturun.
   
    ![][B7]
   
    ![][B8]
8. RegisterController.cs sınıfına aşağıdaki `using` deyimlerini ekleyin:
   
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.NotificationHubs.Messaging;
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;
9. Aşağıdaki kodu `RegisterController` sınıf tanımına ekleyin. Bu kodda, kodun HttpContext’e eklendiği kullanıcı için bir kullanıcı etiketi ekleriz. Kullanıcının kimliği doğrulanmış ve eklediğimiz `AuthenticationTestHandler` mesaj filtresi ile HttpContext’e eklenmiştir. Ayrıca, kullanıcının istenen etiketlere kaydolma haklarının olduğunu doğrulamak için isteğe bağlı denetimler ekleyebilirsiniz.
   
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
10. Yaptığınız değişiklikleri kaydedin.

## <a name="sending-notifications-from-the-webapi-backend"></a>WebAPI Arka Ucundan Bildirim Gönderme
Bu bölümde, istemci cihazlarının, ASP.NET WebAPI arka ucundaki Azure Notification Hubs Hizmet Yönetimi Kitaplığı’nı kullanarak kullanıcı adı etiketini temel alan bir bildirim göndermesini sağlayan yeni bir denetleyici ekleyeceksiniz.

1. **NotificationsController** adlı başka bir yeni denetleyici oluşturun. Bu denetleyici de önceki bölümde oluşturduğunuz **RegisterController** ile aynı şekilde oluşturun.
2. NotificationsController.cs sınıfına aşağıdaki `using` deyimlerini ekleyin:
   
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;
3. Aşağıdaki yöntemi **NotificationsController** sınıfına ekleyin.
   
    Bu kod, Platform Bildirim Sistemi (PNS) `pns` parametresini temel alan bir bildirim türü gönderir. `to_tag` değeri, iletideki *kullanıcı adı* etiketini ayarlamak için kullanılır. Bu etiket, etkin bir bildirim hub'ı kaydının kullanıcı etiketi ile eşleşmelidir. Bildirim iletisi, POST isteğinin gövdesinden çekilir ve hedef PNS biçimlendirilir. 
   
    Desteklenen cihazlarınızın bildirimleri almak için kullandığı Platform Bildirim Hizmeti’ne (PNS) bağlı olarak, farklı biçimleri kullanan farklı bildirimler desteklenmektedir. Örneğin, Windows cihazlarında başka bir PNS tarafından doğrudan desteklenmeyen bir [WNS ile bildirim](https://msdn.microsoft.com/library/windows/apps/br230849.aspx) kullanabilirsiniz. Bu nedenle arka ucunuzun, desteklemeyi planladığınız cihazların PNS’si için bildirimi desteklenen bir bildirim biçimine dönüştürmesi gerekir. Bundan sonra [NotificationHubClient sınıfında](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.notificationhubclient_methods.aspx) uygun gönderme API’sini kullanın
   
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
4. Uygulamayı çalıştırmak için **F5** tuşuna basın ve o ana kadar gerçekleştirdiğiniz işin doğruluğundan emin olun. Uygulama bir web tarayıcısı başlatmalı ve ASP.NET giriş sayfasını göstermelidir. 

## <a name="publish-the-new-webapi-backend"></a>Yeni WebAPI Arka Ucunu yayımlama
1. Şimdi bu uygulamayı tüm cihazlardan erişilebilir olması için bir Azure Web Sitesinde yayımlayacağız. **AppBackend** projesine sağ tıklayıp **Yayımla**’yı seçin.
2. Yayımlama hedefi olarak **Microsoft Azure Uygulama Hizmeti**’ni seçip **Yayımla**’ya tıklayın. Bu işlem, ASP.NET web uygulamasını Azure’da çalıştırmak için gereken tüm Azure kaynaklarını oluşturmanıza yardımcı olan App Service Oluştur iletişim kutusunu açar.

    ![][B15]
3. **App Service Oluştur** iletişim kutusunda Azure hesabınızı seçin. **Tür Değiştir**’e tıklayıp **Web Uygulaması**’nı seçin. Mevcut **Web Uygulaması Adı**’nı değiştirmeyin ve **Abonelik**, **Kaynak Grubu** ve **App Service Planı**’nı seçin.  **Oluştur**’a tıklayın.

4. **Özet** bölümündeki **Site URL** özelliğini not edin. Bu URL'ye bu öğreticinin sonraki bölümlerinde *arka uca ait uç nokta* olarak başvuracağız. **Yayımla**’ta tıklayın.

5. Sihirbaz tamamlandıktan sonra ASP.NET web uygulamasını Azure’da yayımlar ve ardından uygulamayı varsayılan tarayıcıda başlatır.  Uygulamanız Azure Uygulama Hizmetileri’nde görüntülenebilir.

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
