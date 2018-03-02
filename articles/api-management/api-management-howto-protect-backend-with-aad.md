--Başlık: Web API'si arka uç Azure Active Directory ve API Management ile koruma | Microsoft Docs Açıklama: Azure Active Directory ve API Management ile Web API arka uç korumayı öğrenin.
services: api-management documentationcenter: '' author: juliako manager: cfowler editor: ''

MS.Service: API management ms.workload: Mobil ms.tgt_pltfrm: na ms.devlang: na ms.topic: ms.date makale: 30/10/2017 ms.author: apimpm
---

# <a name="how-to-protect-a-web-api-backend-with-azure-active-directory-and-api-management"></a>Bir Web API arka ucu Azure Active Directory ve API Management ile korumak nasıl

Bu konuda, bir Web API arka ucu oluşturmak ve Azure Active Directory ve API Management ile OAuth 2.0 protokolünü kullanarak korumak gösterilmektedir.  

## <a name="create-an-azure-ad-directory"></a>Azure AD dizini oluşturma
Azure Active Directory'yi kullanarak Web API'si arka güvenli hale getirmek için öncelikle bir AAD kiracısı olması gerekir. Bir AAD kiracısı oluşturmak için oturum için açma [Klasik Azure portalı](https://manage.windowsazure.com) tıklatıp **yeni**->**uygulama hizmetleri**->**Active Directory**->**Directory**->**özel Oluştur**. 

![Azure Active Directory][api-management-create-aad-menu]

Bu örnekte, adında bir dizin **APIMDemo** adlı varsayılan etki alanı ile oluşturulan **DemoAPIM.onmicrosoft.com**. 

![Azure Active Directory][api-management-create-aad]

## <a name="create-a-web-api-service-secured-by-azure-active-directory"></a>Azure Active Directory tarafından güvenliği sağlanan bir Web API hizmet oluşturma
Bu adımda, Visual Studio 2013 kullanarak bir Web API arka uç oluşturulur. Visual Studio tıklatın Web API'si arka uç projesi oluşturmak için **dosya**->**yeni**->**proje**ve seçin **ASP.NET Web uygulaması** gelen **Web** şablonları listesi. 

![Visual Studio][api-management-new-web-app]

Tıklatın **Web API** gelen **şablon listesini seçin** bir Web API projesi oluşturmak için. Azure Directory kimlik doğrulamasını yapılandırmak için tıklatın **kimlik doğrulamayı Değiştir**.

![Yeni proje][api-management-new-project]

Tıklatın **Kurumsal hesaplar**ve belirtin **etki alanı** AAD kiracınızın. Bu örnekte, etki alanıdır **DemoAPIM.onmicrosoft.com**. Dizininizin etki alanından elde edilebilir **etki alanları** dizininizin sekmesi.

![Etki Alanları][api-management-aad-domains]

İstenen ayarları yapılandırın **kimlik doğrulamayı Değiştir** iletişim kutusu ve tıklatın **Tamam**.

![Kimlik doğrulamayı Değiştir][api-management-change-authentication]

Tıkladığınızda **Tamam** Visual Studio, Azure AD dizini ile uygulamanızı kaydetme deneyecek ve Visual Studio tarafından oturum istenebilir. Dizininiz için bir yönetici hesabı kullanarak oturum açın.

![Visual Studio'da oturum açın][api-management-sign-in-vidual-studio]

Bu proje bir Azure Web API olarak onay kutusu için yapılandırmak için **bulutta Barındır** ve ardından **Tamam**.

![Yeni proje][api-management-new-project-cloud]

Azure'da oturum açın istenebilir ve daha sonra Web uygulaması yapılandırabilirsiniz.

![Yapılandırma][api-management-configure-web-app]

Bu örnekte, yeni bir **uygulama hizmeti planı** adlı **APIMAADDemo** belirtilir.

Tıklatın **Tamam** Web uygulamasını yapılandırma ve projeyi oluşturmak için.

## <a name="add-the-code-to-the-web-api-project"></a>Web API projesi için kod ekleme

Bu örnekte, Web API bir model ve bir denetleyici kullanarak bir temel hesaplayıcı hizmet uygular. Hizmeti için modeli eklemek için sağ tıklatın **modelleri** içinde **Çözüm Gezgini** ve **Ekle**, **sınıfı**. Sınıf adını `CalcInput` tıklatıp **Ekle**.

Aşağıdakileri ekleyin `using` deyimi üstüne `CalcInput.cs` dosya.

```csharp
using Newtonsoft.Json;
```

Oluşturulan sınıf aşağıdaki kodla değiştirin.

```csharp
public class CalcInput
{
    [JsonProperty(PropertyName = "a")]
    public int a;

    [JsonProperty(PropertyName = "b")]
    public int b;
}
```

Sağ **denetleyicileri** içinde **Çözüm Gezgini** ve **Ekle**->**denetleyicisi**. Seçin **Web API 2 denetleyici - boş** tıklatıp **Ekle**. Tür **CalcController** denetleyici için adı ve'ı tıklatın **Ekle**.

![Denetleyici ekleme][api-management-add-controller]

Aşağıdakileri ekleyin `using` deyimi üstüne `CalcController.cs` dosya.

```csharp
using System.IO;
using System.Web;
using APIMAADDemo.Models;
```

Oluşturulan denetleyici sınıfını aşağıdaki kodla değiştirin. Bu kod uygulayan `Add`, `Subtract`, `Multiply`, ve `Divide` temel hesaplayıcı API'si işlemleri.

```csharp
[Authorize]
public class CalcController : ApiController
{
    [Route("api/add")]
    [HttpGet]
    public HttpResponseMessage GetSum([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a + b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }

    [Route("api/sub")]
    [HttpGet]
    public HttpResponseMessage GetDiff([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a - b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }

    [Route("api/mul")]
    [HttpGet]
    public HttpResponseMessage GetProduct([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a * b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }

    [Route("api/div")]
    [HttpGet]
    public HttpResponseMessage GetDiv([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a / b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }
}
```

Tuşuna **F6** oluşturun ve çözümü doğrulayın.

## <a name="publish-the-project-to-azure"></a>Projeyi Azure'da yayımlama

Projeyi Azure'a yayımlamak için sağ **APIMAADDemo** Visual Studio'da ve seçin **Yayımla**. Varsayılan ayarları korumak **Web'i Yayımla** iletişim kutusu ve tıklatın **Yayımla**.

![Web yayımlama][api-management-web-publish]

## <a name="grant-permissions-to-the-azure-ad-backend-service-application"></a>Azure AD arka uç hizmeti uygulama izinleri
Arka uç hizmeti için yeni bir uygulama, Web API projesi yapılandırma ve yayımlama işleminin bir parçası olarak, Azure AD dizini oluşturulur.

![Uygulama][api-management-aad-backend-app]

Gerekli izinleri yapılandırma uygulama adına tıklayın. Gidin **yapılandırma** sekmesinde ve ekranı aşağı kaydırarak **diğer uygulamalara izinler** bölümü. Tıklatın **uygulama izinleri** yanında aşağı açılan **Windows** **Azure Active Directory**, için kutuyu **dizin verilerini okuma**, tıklatıp **kaydetmek**.

![İzin ekle][api-management-aad-add-permissions]

> [!NOTE]
> Varsa **Windows** **Azure Active Directory** olan diğer uygulamalara izinler altında listelenen değil, tıklatın **uygulama ekleme** ve listeden ekleyin.
> 
> 

Not **uygulama kimliği URI'si** Azure AD uygulaması API Management Geliştirici Portalı için yapılandırıldığında, bir sonraki adımda kullanmak için.

![Uygulama Kimliği URI][api-management-aad-sso-uri]

## <a name="import-the-web-api-into-api-management"></a>Web API API yönetim altına alın
API'leri, Azure Portalı aracılığıyla erişilen API yayımcı portalında yapılandırılır. Bunu ulaşmak için tıklatın **yayımcı portalına** API Management hizmetiniz araç çubuğundan. Henüz bir API Management hizmeti örneği oluşturmadıysanız, bkz: [bir API Management hizmet örneği oluşturma] [ Create an API Management service instance] içinde [ilk API'nizi yönetme] [ Manage your first API] Öğreticisi.

![Yayımcı portalı][api-management-management-console]

İşlemleri olabilir [API'leri için el ile eklenen](api-management-howto-add-operations.md), veya içeri aktarılabilir. Bu videoda, Swagger biçiminde 6:40 başlangıç işlemleri alınır.

Adlı bir dosya oluşturun `calcapi.json` aşağıdaki içeriğe sahip ve bilgisayarınıza kaydedin. Emin `host` , Web API uç noktaları özniteliği. Bu örnekte, `"host": "apimaaddemo.azurewebsites.net"` kullanılır.

```json
{
  "swagger": "2.0",
  "info": {
    "title": "Calculator",
    "description": "Arithmetics over HTTP!",
    "version": "1.0"
  },
  "host": "apimaaddemo.azurewebsites.net",
  "basePath": "/api",
  "schemes": [
    "http"
  ],
  "paths": {
    "/add?a={a}&b={b}": {
      "get": {
        "description": "Responds with a sum of two numbers.",
        "operationId": "Add two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>51</code>.",
            "required": true,
            "type": "string",
            "default": "51",
            "enum": [
              "51"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>49</code>.",
            "required": true,
            "type": "string",
            "default": "49",
            "enum": [
              "49"
            ]
          }
        ],
        "responses": { }
      }
    },
    "/sub?a={a}&b={b}": {
      "get": {
        "description": "Responds with a difference between two numbers.",
        "operationId": "Subtract two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>100</code>.",
            "required": true,
            "type": "string",
            "default": "100",
            "enum": [
              "100"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>50</code>.",
            "required": true,
            "type": "string",
            "default": "50",
            "enum": [
              "50"
            ]
          }
        ],
        "responses": { }
      }
    },
    "/div?a={a}&b={b}": {
      "get": {
        "description": "Responds with a quotient of two numbers.",
        "operationId": "Divide two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>100</code>.",
            "required": true,
            "type": "string",
            "default": "100",
            "enum": [
              "100"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>20</code>.",
            "required": true,
            "type": "string",
            "default": "20",
            "enum": [
              "20"
            ]
          }
        ],
        "responses": { }
      }
    },
    "/mul?a={a}&b={b}": {
      "get": {
        "description": "Responds with a product of two numbers.",
        "operationId": "Multiply two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>20</code>.",
            "required": true,
            "type": "string",
            "default": "20",
            "enum": [
              "20"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>5</code>.",
            "required": true,
            "type": "string",
            "default": "5",
            "enum": [
              "5"
            ]
          }
        ],
        "responses": { }
      }
    }
  }
}
```

Hesaplayıcı API’sini içeri aktarmak için, soldaki **API Management** menüsünde **API'ler**’e tıklayın ve ardından **API’yi İçeri Aktar**’a tıklayın.

![API’yi İçeri Aktar düğmesi][api-management-import-api]

Hesaplayıcı API'sini yapılandırmak için aşağıdaki adımları gerçekleştirin.

1. ' I tıklatın **dosyasından**, göz atın `calculator.json` kaydettiniz dosyasını bulun ve tıklatın **Swagger** radyo düğmesi.
2. Tür **calc** içine **Web API'si URL soneki** metin kutusu.
3. **Ürünler (isteğe bağlı)** öğesine tıklayın ve **Starter**’ı seçin.
4. API’yi içeri aktarmak için **Kaydet**’e tıklayın.

![Yeni API ekle][api-management-import-new-api]

API içeri aktarıldığında, yayımcı portalında API’nin özet sayfası gösterilir.

## <a name="call-the-api-unsuccessfully-from-the-developer-portal"></a>Geliştirici Portalı'ndan başarısız API çağrısı
Bu noktada, API API yönetime içeri aktarıldı ancak arka uç hizmetine Azure AD kimlik doğrulaması ile korunduğu için henüz başarıyla Geliştirici portalından çağrılamaz. 

Tıklatın **Geliştirici Portalı** yayımcı portalının sağ üst tarafındaki.

![Geliştirici portalı][api-management-developer-portal-menu]

Tıklatın **API'leri** tıklatıp **hesaplayıcı** API.

![Geliştirici portalı][api-management-dev-portal-apis]

Tıklatın **deneyin**.

![Deneyin][api-management-dev-portal-try-it]

Tıklatın **Gönder** ve yanıt durumu Not **401 Yetkisiz**.

![Gönder][api-management-dev-portal-send-401]

Arka uç API'si Azure Active Directory tarafından korunduğu için yetkisiz bir istektir. Geliştirici başarıyla API çağırmadan önce portal geliştiricilerinin OAuth 2.0 kullanan yetkilendirmek için yapılandırılmış olması gerekir. Bu işlem aşağıdaki bölümlerde açıklanmıştır.

## <a name="register-the-developer-portal-as-an-aad-application"></a>Geliştirici Portalı bir AAD uygulaması Kaydet
Geliştirici Portalı bir AAD uygulaması kaydetmek için OAuth 2.0 kullanan geliştiriciler yetkilendirmek için Geliştirici Portalı yapılandırmada ilk adım olacaktır. 

Azure AD kiracısı gidin. Bu örnekte seçin **APIMDemo** gidin **uygulamaları** sekmesi.

![Yeni uygulama][api-management-aad-new-application-devportal]

Tıklatın **Ekle** yeni bir Azure Active Directory uygulaması oluşturmak için düğmesine tıklayın ve seçin **kuruluşumun geliştirmekte olduğu bir uygulama Ekle**.

![Yeni uygulama][api-management-new-aad-application-menu]

Seçin **Web uygulaması ve/veya Web API**, bir ad girin ve İleri okuna tıklayın. Bu örnekte, **APIMDeveloperPortal** kullanılır.

![Yeni uygulama][api-management-aad-new-application-devportal-1]

İçin **oturum açma URL'si** API Management hizmetiniz URL'sini girin ve ilave `/signin`. Bu örnekte, `https://contoso5.portal.azure-api.net/signin` kullanılır.

İçin **uygulama kimliği URL'si** API Management hizmetiniz URL'sini girin ve bazı benzersiz karakterler ekleyin. Bunlar istenen herhangi bir karakter olabilir ve bu örnekte, `https://contoso5.portal.azure-api.net/dp` kullanılır. Zaman istenen **uygulama özellikleri** olan yapılandırılmış, uygulama oluşturmak için onay işaretine tıklayın.

![Yeni uygulama][api-management-aad-new-application-devportal-2]

## <a name="configure-an-api-management-oauth-20-authorization-server"></a>Bir API Management OAuth 2.0 yetkilendirme sunucusu yapılandırın
Sonraki adım, bir OAuth 2.0 yetkilendirme Sunucusu API Management'te yapılandırmaktır. 

Tıklatın **güvenlik** soldaki API Management menüden **OAuth 2.0**ve ardından **yetkilendirme eklemek** sunucu.

![Yetkilendirme Sunucusu Ekle][api-management-add-authorization-server]

Bir ad ve isteğe bağlı bir açıklama girin **adı** ve **açıklama** alanları. Bu alanlar, API Management hizmet örneği içinde OAuth 2.0 yetkilendirme sunucusu tanımlamak için kullanılır. Bu örnekte, **yetkilendirme sunucusu demo** kullanılır. Daha sonra bir API için kimlik doğrulaması için kullanılacak bir OAuth 2.0 sunucu belirttiğinizde, bu ad seçer.

İçin **istemci kayıt sayfası URL'si** bir yer tutucu değerini girin `http://localhost`.  **İstemci kayıt sayfası URL'si** kullanıcılar oluşturmak ve kullanıcı hesaplarının yönetimini desteklemek için OAuth 2.0 sağlayıcıları kendi hesaplarını yapılandırmak için kullanabileceğiniz sayfa işaret eder. Bu örnekte, kullanıcıların değil oluşturun ve yer tutucu kullanılmak üzere kendi hesaplarını yapılandırın.

![Yetkilendirme Sunucusu Ekle][api-management-add-authorization-server-1]

Ardından, belirtin **yetkilendirme uç noktası URL'si** ve **belirteç uç nokta URL'si**.

![Yetkilendirme sunucusu][api-management-add-authorization-server-1a]

Bu değerleri penceresinden alınabilir **uygulama uç noktaları** Geliştirici Portalı için oluşturulmuş AAD uygulama sayfası. Uç noktaları erişmek için gidin **yapılandırma** sekmesinde AAD uygulaması için **uç noktaları görüntülemek**.

![Uygulama][api-management-aad-devportal-application]

![Uç noktalarını görüntüle][api-management-aad-view-endpoints]

Kopya **OAuth 2.0 yetkilendirme uç noktası** ve yapıştırın **yetkilendirme uç noktası URL'si** metin kutusu.

![Yetkilendirme Sunucusu Ekle][api-management-add-authorization-server-2]

Kopya **OAuth 2.0 belirteç uç noktası** ve yapıştırın **belirteç uç nokta URL'si** metin kutusu.

![Yetkilendirme Sunucusu Ekle][api-management-add-authorization-server-2a]

Belirteç uç noktası yapıştırarak yanı sıra, adlandırılmış bir ek gövde parametresini ekleyin **kaynak** ve değerini kullanmak için **uygulama kimliği URI'si** Visual Studio projesi yayımlandığında, oluşturulan arka uç hizmeti için AAD uygulamasından.

![Uygulama Kimliği URI][api-management-aad-sso-uri]

Ardından, istemci kimlik bilgilerini belirtin. Bu erişim, bu durumda Geliştirici Portalı istediğiniz kaynak için kimlik bilgileridir.

![İstemci kimlik bilgileri][api-management-client-credentials]

Alınacak **istemci kimliği**, gidin **yapılandırma** sekmesi Geliştirici Portalı ve kopyalama için AAD uygulaması **istemci kimliği**.

Almak için **gizli** tıklatın **seçin süresi** açılan **anahtarları** bölüm ve bir aralık belirtin. Bu örnekte, 1 yıl kullanılır.

![İstemci Kimliği][api-management-aad-client-id]

Tıklatın **kaydetmek** yapılandırmayı kaydedin ve anahtarı görüntülemek için. 

> [!IMPORTANT]
> Bu anahtarı not edin. Azure Active Directory yapılandırması penceresini kapattığınızda, anahtarı yeniden görüntülenemiyor.
> 
> 

Anahtarı panoya kopyalayın, geçiş yayımcı portalına dönün, içine anahtarını yapıştırın **gizli** metin kutusuna, tıklatıp **kaydetmek**.

![Yetkilendirme Sunucusu Ekle][api-management-add-authorization-server-3]

Hemen istemci kimlik bilgileri aşağıdaki yetkilendirme kodu verme olur. Bu yetkilendirme kodu ve anahtarı yedekleme Azure AD Geliştirici Portalı uygulamanıza kopyalama sayfasını yapılandırmak ve yetkilendirme verme içine yapıştırma **yanıt URL'si** alan öğesini tıklatıp **kaydetmek** yeniden.

![Yanıt URL'si][api-management-aad-reply-url]

Geliştirici Portalı AAD uygulama izinlerini yapılandırmak için sonraki adımdır bakın. Tıklatın **uygulama izinleri** ve için kutuyu **dizin verilerini okuma**. Tıklatın **kaydetmek** bu değişikliği kaydetmek ve ardından **uygulama eklemek**.

![İzin ekle][api-management-add-devportal-permissions]

Arama simgesine tıklayın türü **APIM** kutusuyla başlangıç içine seçin **APIMAADDemo**ve kaydetmek için onay işaretine tıklayın.

![İzin ekle][api-management-aad-add-app-permissions]

Tıklatın **izinlere temsilci** için **APIMAADDemo** ve için kutuyu **erişim APIMAADDemo**, tıklatıp **kaydetmek**. Geliştirici böylece arka uç hizmetine erişmek için portal uygulaması.

![İzin ekle][api-management-aad-add-delegated-permissions]

## <a name="enable-oauth-20-user-authorization-for-the-calculator-api"></a>OAuth 2.0 kullanıcı yetkilendirme hesaplayıcı API'si etkinleştir
OAuth 2.0 sunucu yapılandırılır, API'nizi için güvenlik ayarlarını belirtebilirsiniz. 

Tıklatın **API'leri** sol menüsüne ve ardından içinde **hesaplayıcı** görüntülemek ve ayarlarını yapılandırmak için.

![Hesaplayıcı API'sini][api-management-calc-api]

Gidin **güvenlik** sekmesi, onay **OAuth 2.0** onay kutusunu istenen yetkilendirme sunucusundan seçip **yetkilendirme sunucusu** aşağı açılan tıklatıp **kaydetmek**.

![Hesaplayıcı API'sini][api-management-enable-aad-calculator]

## <a name="successfully-call-the-calculator-api-from-the-developer-portal"></a>Hesaplayıcı API'sini Geliştirici Portalı'ndan başarıyla çağırın
OAuth 2.0 yetkilendirme API yapılandırılır, işlemlerini başarıyla Geliştirici Merkezi'nden çağrılabilir. 

Geri gidin **iki tamsayı Ekle** işlemi tıklatın ve Geliştirici Portalı hesap makinesi hizmetinin **deneyin**. Yeni öğesini Not **yetkilendirme** yeni eklenen yetkilendirme sunucusu karşılık gelen bölüm.

![Hesaplayıcı API'sini][api-management-calc-authorization-server]

Seçin **yetkilendirme kodu** açılan yetkilendirme dan listesinde ve kullanılacak hesap kimlik bilgilerini girin. Hesapla oturum açtıysanız, istenebilir değil.

![Hesaplayıcı API'sini][api-management-devportal-authorization-code]

Tıklatın **Gönder** ve not edin **yanıt durumu** , **200 Tamam** ve yanıt içeriği işlemde sonuçları.

![Hesaplayıcı API'sini][api-management-devportal-response]

## <a name="configure-a-desktop-application-to-call-the-api"></a>API'yi çağırmak için bir masaüstü uygulamasını yapılandırma

API'yi çağırmak için basit bir masaüstü uygulaması yapılandırın. Azure AD'de masaüstü uygulaması kaydetmek ve dizine ve arka uç hizmetine erişim sağlamak için ilk adımdır bakın. 

## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>İstekleri önceden yetkilendirmek için JWT doğrulama ilkesini yapılandırma

Kullanım [doğrulamak JWT](api-management-access-restriction-policies.md#ValidateJWT) isteklerini her gelen istek erişim belirteçleri doğrulayarak önceden yetkilendirmek için ilke. İstek doğrulamak JWT İlkesi tarafından doğrulanmaz, istek API Management tarafından engellenir ve boyunca arka ucuna aktarılmaz.

```xml
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
    <openid-config url="https://login.microsoftonline.com/DemoAPIM.onmicrosoft.com/.well-known/openid-configuration" />
    <required-claims>
        <claim name="aud">
            <value>https://DemoAPIM.NOTonmicrosoft.com/APIMAADDemo</value>
        </claim>
    </required-claims>
</validate-jwt>
```

Daha fazla bilgi için bkz: [bulut kapak bölüm 177: daha fazla API yönetimi özellikleri](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) ve ileri sarma 13:50. İlke Düzenleyicisi'nde yapılandırılmış ilkeler görmek için 15:00 ve 18:50 hem ile hem de gerekli yetkilendirme belirteci olmadan Geliştirici portalından bir işlem arama tanıtımı için ileri sarma.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi denetleyin [videolar](https://azure.microsoft.com/documentation/videos/index/?services=api-management) API Management hakkında.
* Arka uç hizmetinizin güvenliğini sağlamak diğer yolları için bkz: [karşılıklı sertifika kimlik doğrulaması](api-management-howto-mutual-certificates.md).

[api-management-management-console]: ./media/api-management-howto-protect-backend-with-aad/api-management-management-console.png

[api-management-import-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-new-api.png
[api-management-create-aad-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad-menu.png
[api-management-create-aad]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad.png
[api-management-new-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-web-app.png
[api-management-new-project]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project.png
[api-management-new-project-cloud]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project-cloud.png
[api-management-change-authentication]: ./media/api-management-howto-protect-backend-with-aad/api-management-change-authentication.png
[api-management-sign-in-vidual-studio]: ./media/api-management-howto-protect-backend-with-aad/api-management-sign-in-vidual-studio.png
[api-management-configure-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-configure-web-app.png
[api-management-aad-domains]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-domains.png
[api-management-add-controller]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-controller.png
[api-management-web-publish]: ./media/api-management-howto-protect-backend-with-aad/api-management-web-publish.png
[api-management-aad-backend-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-backend-app.png
[api-management-aad-add-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-permissions.png
[api-management-developer-portal-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-developer-portal-menu.png
[api-management-dev-portal-apis]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-apis.png
[api-management-dev-portal-try-it]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-try-it.png
[api-management-dev-portal-send-401]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-send-401.png
[api-management-aad-new-application-devportal]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal.png
[api-management-aad-new-application-devportal-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-1.png
[api-management-aad-new-application-devportal-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-2.png
[api-management-aad-devportal-application]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-devportal-application.png
[api-management-add-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server.png
[api-management-aad-sso-uri]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-sso-uri.png
[api-management-aad-view-endpoints]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-view-endpoints.png
[api-management-aad-client-id]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-client-id.png
[api-management-add-authorization-server-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1.png
[api-management-add-authorization-server-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2.png
[api-management-add-authorization-server-2a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2a.png
[api-management-add-authorization-server-3]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-3.png
[api-management-aad-reply-url]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-reply-url.png
[api-management-add-devportal-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-devportal-permissions.png
[api-management-aad-add-app-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-app-permissions.png
[api-management-aad-add-delegated-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-delegated-permissions.png
[api-management-calc-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-api.png
[api-management-enable-aad-calculator]: ./media/api-management-howto-protect-backend-with-aad/api-management-enable-aad-calculator.png
[api-management-devportal-authorization-code]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-authorization-code.png
[api-management-devportal-response]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-response.png
[api-management-calc-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-authorization-server.png
[api-management-add-authorization-server-1a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1a.png
[api-management-client-credentials]: ./media/api-management-howto-protect-backend-with-aad/api-management-client-credentials.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-aad-application-menu.png

[Create an API Management service instance]: get-started-create-service-instance.md
[Manage your first API]: import-and-publish.md
