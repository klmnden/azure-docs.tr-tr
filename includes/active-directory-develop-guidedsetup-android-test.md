## <a name="test-your-code"></a>Kodunuzu test

1. Kodunuzu Aygıt/Öykünücüsü için dağıtın.

2. Uygulamanızı test etmek hazır olduğunuzda, bir Azure Active Directory hesabı (iş veya Okul hesabı) veya bir Microsoft hesabı (live.com, outlook.com) oturum açmak için kullanın. 

    ![Uygulamanızı test edin](media/active-directory-develop-guidedsetup-android-test/mainwindow.png)
    <br/><br/>
    ![Kullanıcı adı ve parola girin](media/active-directory-develop-guidedsetup-android-test/usernameandpassword.png)

### <a name="provide-consent-for-application-access"></a>Uygulama erişimi için izin sağlayın
Uygulamanız için de istenir sağlamak üzere oturum ilk kez uygulamanın profilinizi erişmek ve buna, aşağıda gösterildiği gibi oturum izin vermiş olursunuz: 

![Uygulama erişimi için onayınızı sağlayın](media/active-directory-develop-guidedsetup-android-test/androidconsent.png)


### <a name="view-application-results"></a>Uygulama sonuçları görüntüleme
Oturum açtıktan sonra Microsoft Graph API çağrısı tarafından döndürülen sonuçları görmeniz gerekir. Microsoft Graph API çağrısı **bana** endpoint döndürür [kullanıcı profili](https://graph.microsoft.com/v1.0/me). Genel Microsoft Graph uç noktaları listesi için bkz: [Microsoft Graph API Geliştirici belgeleri](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi

Microsoft Graph API gerektiriyor *user.read* kullanıcı profilini okuma için kapsamı. Bu kapsam, varsayılan olarak uygulama kayıt Portalı'nda kayıtlı her uygulama otomatik olarak eklenir. Diğer Microsoft Graph için API'leri yanı sıra, arka uç sunucusu için özel API'leri ek kapsamlar gerektirebilir. Microsoft Graph API gerektiriyor *Calendars.Read* kullanıcının takvimleri listelemek için kapsam. 

Bir uygulama bağlamında kullanıcının takvimleri erişmek için eklemeniz *Calendars.Read* izin uygulama kayıt bilgileri için temsilci. Ardından, ekleyin *Calendars.Read* için kapsam `acquireTokenSilent` çağırın. 

>[!NOTE]
>Kapsam sayısı arttıkça, kullanıcı için ek onayları istenebilir.

<!--end-collapse-->

[!INCLUDE [Help and support](active-directory-develop-help-support-include.md)]
