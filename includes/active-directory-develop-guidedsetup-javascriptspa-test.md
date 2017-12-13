## <a name="test-your-code"></a>Kodunuzu test

### <a name="test-with-visual-studio"></a>Visual Studio ile test
Visual Studio kullanıyorsanız, basın **F5** projeyi çalıştırın. Http:// tarayıcı açar<span></span>localhost: {port} konumu ve bkz **Microsoft Graph API çağrısı** düğmesi.

<p/><!-- --> 

### <a name="test-with-python-or-other-web-server"></a>Python veya başka bir web sunucusu ile test
Visual Studio kullanmıyorsanız, web sunucunuz başlatıldığından emin olun. Konumuna dayalı bir TCP bağlantı noktasını dinlemek üzere yapılandırmak, **index.html** dosya. Uygulama klasöründen terminal komut istemi çalıştırarak bağlantı noktasını dinlemek üzere Python için başlatın:
 
```bash
python -m http.server 8080
```
Tarayıcıyı açın ve http:// yazın<span></span>localhost:8080 veya http://<span></span>localhost: {port} nerede **bağlantı noktası** , web sunucunuz dinlediği bağlantı noktasıdır. İndex.html dosyasının içeriğini görmeniz gerekir ve **Microsoft Graph API çağrısı** düğmesi.

## <a name="test-your-application"></a>Uygulamanızı test edin

Tarayıcı index.html dosyanızı yüklendikten sonra seçin **Microsoft Graph API çağrısı**. Uygulamanızın, çalıştırdığınız ilk kez tarayıcı Microsoft Azure Active Directory (Azure AD) v2.0 uç noktasına yönlendirir ve oturum açmak için istenir:
 
![JavaScript SPA hesabınızda oturum açın](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="provide-consent-for-application-access"></a>Uygulama erişimi için izin sağlayın

Uygulamanız için oturum ilk kez profilinizi erişmek için ve oturum açmak için uygulama izin vermek için onay vermeniz istenir:

![Uygulama erişimi için onayınızı sağlayın](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

### <a name="view-application-results"></a>Uygulama sonuçları görüntüleme
Oturum açtıktan sonra kullanıcı profili bilgilerini görmelisiniz **Graph API çağrısı yanıt** kutusu.
 
![Microsoft Graph API çağrısı beklenen sonuçları](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptsparesults.png)

İçinde edinilen belirteci hakkında temel bilgiler görmeniz gerekir **erişim belirteci** ve **belirteç talep kimliği** kutuları.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi

Microsoft Graph API gerektiriyor **user.read** kullanıcı profilini okuma için kapsamı. Bu kapsam, varsayılan olarak kayıt portalı üzerinde kayıtlı her uygulama otomatik olarak eklenir. Diğer Microsoft Graph için API'leri yanı sıra, arka uç sunucusu için özel API'leri ek kapsamlar gerektirebilir. Microsoft Graph API gerektiriyor **Calendars.Read** kullanıcının takvimleri listelemek için kapsam.

Bir uygulama bağlamında kullanıcının takvimleri erişmek için eklemeniz **Calendars.Read** izin uygulama kayıt bilgileri için temsilci. Ardından, ekleyin **Calendars.Read** için kapsam **acquireTokenSilent** çağırın. 

>[!NOTE]
>Kapsam sayısı arttıkça, kullanıcı için ek onayları istenebilir.

Bir arka uç API'si (önerilmez) bir kapsam gerek yoksa, kullanabileceğiniz **ClientID** kapsamda olarak **acquireTokenSilent** ve **acquireTokenRedirect** çağrıları.

<!--end-collapse-->

[!INCLUDE  [Help and support](./active-directory-develop-help-support-include.md)]
