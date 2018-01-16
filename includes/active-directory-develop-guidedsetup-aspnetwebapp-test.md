## <a name="test-your-code"></a>Kodunuzu test

Visual Studio'da uygulamanızı test etmek için basın **F5** projeyi çalıştırın. Http:// tarayıcı açar<span></span>localhost: {port} konumu ve bkz **oturum oturum Microsoft** düğmesi. Oturum açma işlemini başlatmak için düğmesini seçin.

Testinizi çalıştırmak hazır olduğunuzda, bir Microsoft Azure Active Directory (Azure AD) hesabı (iş veya Okul hesabı) veya kişisel Microsoft hesabı kullanın (<span>Canlı.</span> com veya <span>outlook.</span> com) oturum açın.

![Microsoft ile oturum açın](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin.png)
<br/><br/>
![Microsoft hesabınızda oturum açın](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin2.png)

#### <a name="view-application-results"></a>Uygulama sonuçları görüntüleme
Kullanıcı oturum açtıktan sonra Web sitenizin giriş sayfasına yönlendirilir. Uygulama kayıt bilgilerinizi Microsoft uygulama kayıt Portalı'nda belirtilen HTTPS URL'sini giriş sayfasıdır. Giriş sayfasına Hoş Geldiniz iletisi içerir "Merhaba \<kullanıcı >," bağlantısı oturumu kapatın ve kullanıcının talepleri görüntülemek için bir bağlantı. Kullanıcının talepleri için bağlantı gözatar için **Authorize** daha önce oluşturduğunuz denetleyicisi.

### <a name="browse-to-see-the-users-claims"></a>Kullanıcının talepleri görmek için Gözat
Kullanıcının talepleri görmek için yalnızca kimliği doğrulanmış kullanıcılar için kullanılabilir denetleyicisi görünümüne gitmek için bağlantıyı seçin.

#### <a name="view-the-claims-results"></a>Talep sonuçları görüntüleme
Denetleyici görünümüne göz sonra kullanıcı için temel özelliklerini içeren bir tablo görmeniz gerekir:

|Özellik |Değer |Açıklama |
|---|---|---|
|**Ad** |Kullanıcının tam adı | Kullanıcı adı ve soyadı.
|**Kullanıcı Adı** |Kullanıcı<span>@domain.com</span> | Kullanıcıyı tanımlamak için kullanılan kullanıcı adı.
|**Konu** |Konu |Kullanıcı web üzerinden benzersiz olarak tanımlayan bir dize.|
|**Kiracı kimliği** |Guid | A **GUID** benzersiz olarak temsil eden kullanıcının Azure AD kuruluş.|

Ayrıca, kimlik doğrulama isteğine olan tüm talepler tablosu görmeniz gerekir. Daha fazla bilgi için bkz: [bir Azure AD kimlik belirtecinde bulunan talepleri listesi](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims).


### <a name="test-access-to-a-method-that-has-an-authorize-attribute-optional"></a>(İsteğe bağlı) bir Authorize özniteliğine sahip bir yöntem test erişimi
Erişimi test etmek için **Authorize** bir anonim kullanıcı olarak denetleyici kullanıcının talepleri için şu adımları izleyin:
1. Out kullanıcı oturum açabilir ve oturum kapatma işlemini tamamlamak için bağlantıyı seçin.
2. Tarayıcınızda http:// yazın<span></span>localhost: {port} / ile korunan denetleyicinizi erişmek için kimliği doğrulanmış **Authorize** özniteliği.

#### <a name="expected-results-after-access-to-a-protected-controller"></a>Korumalı bir denetleyicinin erişimini sonra beklenen sonuçları
Korumalı denetleyicisi görünümü kullanmak için kimlik doğrulaması yapmak istenir.

## <a name="additional-information"></a>Ek bilgiler

<!--start-collapse-->
### <a name="protect-your-entire-website"></a>Web sitenizin tamamını koruma
İçindeki tüm Web sitenizin, korunacak **Global.asax** dosya, ekleme **AuthorizeAttribute** özniteliğini **GlobalFilters** filtrelemek **uygulama _Sihirbaz Başlat** yöntemi:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

### <a name="restrict-sign-in-access-to-your-application"></a>Uygulamanıza oturum açma erişimi kısıtlama
Varsayılan olarak, outlook.com, live.com ve diğerleri gibi kişisel hesaplar uygulamanıza oturum açabilirsiniz. İş veya Okul hesapları Azure AD ile tümleşik kuruluşlardaki de varsayılan olarak oturum açabilir.

Uygulamanız için kullanıcı oturum açma erişimini kısıtlamak için birkaç seçenek bulunur.

#### <a name="restrict-access-to-a-single-organization"></a>Tek bir kurumun erişimi kısıtlama
Tek bir olan kullanıcı hesapları için uygulamanız için oturum açma erişimi kısıtlayabilirsiniz Azure AD kuruluş:
1. İçinde **web.config** dosya, değerini değiştirin **Kiracı** parametresi. Değeri değiştirme **ortak** Kiracı adına kuruluşun gibi **contoso.onmicrosoft.com**.
2. İçinde **OWIN başlangıç** sınıfı, Ayarla **Validateıssuer** bağımsız değişkeni **doğru**.

#### <a name="restrict-access-to-a-list-of-organizations"></a>Kuruluşlar listesine erişimi kısıtlama
İzin verilen kuruluşlar listesinde bir Azure AD kuruluşta olan tek kullanıcı hesaplarının oturum açma erişimi kısıtlayabilirsiniz:
1. İçinde **web.config** dosyasında, **OWIN başlangıç** sınıfı, Ayarla **Validateıssuer** bağımsız değişkeni **doğru**.
2. Değerini **ValidIssuers** parametresi izin verilen kuruluşların listesi.

#### <a name="use-a-custom-method-to-validate-issuers"></a>Verenler doğrulamak için özel bir yöntem kullanın
Özel bir yöntem kullanarak verenler doğrulamak için uygulayabileceğiniz **IssuerValidator** parametresi. Bu parametre kullanma hakkında daha fazla bilgi için bilgiyi [tokenvalidationparameters değerini sınıfı](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx) konusuna bakın.

[!INCLUDE  [Help and support](./active-directory-develop-help-support-include.md)]
