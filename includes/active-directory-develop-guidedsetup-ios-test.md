## <a name="test-querying-the-microsoft-graph-api-from-your-ios-application"></a>İOS uygulamanızdan Microsoft grafik API'si sorgulanırken test

Benzetici kodu çalıştırmak için basın **komutu** + **R**.

![Benzetici uygulamanızı test edin](media/active-directory-develop-guidedsetup-ios-test/iostestscreenshot.png)

Test etmek hazır olduğunuzda, seçin **Microsoft Graph API çağrısı**. İstendiğinde, kullanıcı adı ve parola girin.

### <a name="provide-consent-for-application-access"></a>Uygulama erişimi için izin sağlayın
Uygulamanız için oturum ilk kez profilinizi erişmek için ve oturum açmak için uygulama izin vermek için onay vermeniz istenir:

![Uygulama erişimi için onayınızı sağlayın](media/active-directory-develop-guidedsetup-ios-test/iosconsentscreen.png)

### <a name="view-application-results"></a>Uygulama sonuçları görüntüleme
Oturum açtıktan sonra Microsoft Graph API çağrısı tarafından döndürülen kullanıcı profil bilgilerinizi görmelisiniz **günlüğü** bölümü. 

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi

Microsoft Graph API gerektiriyor **user.read** kullanıcı profilini okuma için kapsamı. Bu kapsam, varsayılan olarak kayıt portalı üzerinde kayıtlı her uygulama otomatik olarak eklenir. Diğer Microsoft Graph için API'leri yanı sıra, arka uç sunucusu için özel API'leri ek kapsamlar gerektirebilir. Microsoft Graph API gerektiriyor **Calendars.Read** kullanıcının takvimleri listelemek için kapsam.

Bir uygulama bağlamında kullanıcının takvimleri erişmek için eklemeniz **Calendars.Read** izin uygulama kayıt bilgileri için temsilci. Ardından, ekleyin **Calendars.Read** için kapsam **acquireTokenSilent** çağırın. 

>[!NOTE]
>Kapsam sayısı arttıkça, kullanıcı için ek onayları istenebilir.

<!--end-collapse-->

[!INCLUDE  [Help and support](./active-directory-develop-help-support-include.md)]
