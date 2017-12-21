## <a name="test-your-code"></a>Kodunuzu test

Visual Studio, projenizin çalıştırmak için seçin **F5**. Uygulamanızı **MainWindow** , aşağıda gösterildiği gibi görüntülenir:

![Uygulamanızı test edin](./media/active-directory-develop-guidedsetup-windesktop-test/samplescreenshot.png)

Uygulamayı çalıştırmak ilk kez seçip **Microsoft Graph API çağrısı** istenir oturum açmak için düğmesi,. Bir Azure Active Directory hesabı (iş veya Okul hesabı) veya bir Microsoft hesabı (live.com, outlook.com) test etmek için kullanın.

![Uygulama için oturum açın](./media/active-directory-develop-guidedsetup-windesktop-test/signinscreenshot.png)

### <a name="provide-consent-for-application-access"></a>Uygulama erişimi için izin sağlayın
Uygulamanız için de istenir sağlamak üzere oturum ilk kez uygulamanın profilinizi erişmek ve buna, aşağıda gösterildiği gibi oturum izin vermiş olursunuz: 

![Uygulama erişimi için onayınızı sağlayın](./media/active-directory-develop-guidedsetup-windesktop-test/consentscreen.png)

### <a name="view-application-results"></a>Uygulama sonuçları görüntüleme
Oturum açtıktan sonra Microsoft Graph API çağrısı tarafından döndürülen kullanıcı profili bilgilerini görmeniz gerekir. Sonuçları görüntülenir **API çağrısı sonuçları** kutusu. Çağrı aracılığıyla edinilen belirteci ile ilgili temel bilgileri `AcquireTokenAsync` veya `AcquireTokenSilentAsync` görünür olmalıdır **belirteci bilgisi** kutusu. Sonuçları aşağıdaki özellikleri içerir:

|Özellik  |Biçim  |Açıklama |
|---------|---------|---------|
|**Ad** |Kullanıcının tam adı |Kullanıcı adı ve soyadı.|
|**Kullanıcı Adı** |<span>user@domain.com</span> |Kullanıcıyı tanımlamak için kullanılan kullanıcı adı.|
|**Belirtecinin süresi** |Tarih Saat |Belirtecin süresinin dolma zamanı. MSAL belirteci gerekli olarak yenileyerek sona erme tarihini genişletir.|
|**Erişim belirteci** |Dize |HTTP gönderilen belirteç dizesini gerektiren ister bir *Authorization Üstbilgisi*.|

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi

Microsoft Graph API gerektiriyor *user.read* kullanıcı profilini okuma için kapsamı. Bu kapsam, varsayılan olarak uygulama kayıt Portalı'nda kayıtlı her uygulama otomatik olarak eklenir. Diğer Microsoft Graph için API'leri yanı sıra, arka uç sunucusu için özel API'leri ek kapsamlar gerektirebilir. Microsoft Graph API gerektiriyor *Calendars.Read* kullanıcının takvimleri listelemek için kapsam.

Bir uygulama bağlamında kullanıcının takvimleri erişmek için eklemeniz *Calendars.Read* izin uygulama kayıt bilgileri için temsilci. Ardından, ekleyin *Calendars.Read* için kapsam `acquireTokenSilent` çağırın. 

>[!NOTE]
>Kapsam sayısı arttıkça, kullanıcı için ek onayları istenebilir.

<!--end-collapse-->

[!INCLUDE  [Help and support](./active-directory-develop-help-support-include.md)]
