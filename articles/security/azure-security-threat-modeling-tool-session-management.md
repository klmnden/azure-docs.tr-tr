---
title: Oturum yönetimi - Microsoft tehdit modelleme aracı - Azure | Microsoft Docs
description: Tehdit modelleme Aracı kullanıma sunulan tehdit azaltma
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: jegeib
ms.openlocfilehash: e8f3cf3889b3f79e930630ff0e768a0c4875eec6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60611700"
---
# <a name="security-frame-session-management"></a>Güvenlik çerçevesi: Oturum yönetimi
| Ürün/hizmet | Makale |
| --------------- | ------- |
| **Azure AD**    | <ul><li>[Azure AD kullanarak ADAL yöntemleri kullanarak uygulama uygun oturum kapatma](#logout-adal)</li></ul> |
| IOT cihaz | <ul><li>[Sınırlı yaşam süreleri için oluşturulan SaS belirteçlerini kullanma](#finite-tokens)</li></ul> |
| **Azure belge veritabanı** | <ul><li>[En düşük belirteç ömrünü için oluşturulan kaynak belirteçleri kullanma](#resource-tokens)</li></ul> |
| **ADFS** | <ul><li>[ADFS kullanırken WsFederation yöntemlerle uygun uygulama oturum kapatma](#wsfederation-logout)</li></ul> |
| **Kimlik sunucusu** | <ul><li>[Kimlik sunucusu kullanılırken uygun kapatma gerçekleştir](#proper-logout)</li></ul> |
| **Web uygulaması** | <ul><li>[HTTPS üzerinden kullanılabilir uygulamaları güvenli tanımlama bilgileri kullanmalıdır](#https-secure-cookies)</li><li>[Tüm http tabanlı uygulama yalnızca tanımlama bilgisi tanımı için http belirtmeniz gerekir](#cookie-definition)</li><li>[ASP.NET web sayfaları siteler arası istek sahteciliği (CSRF) saldırılarını karşı azaltın](#csrf-asp)</li><li>[Etkinlik yaşam süresi için oturumu ayarlama](#inactivity-lifetime)</li><li>[Uygulamadan uygun oturum kapatma gerçekleştir](#proper-app-logout)</li></ul> |
| **Web API** | <ul><li>[ASP.NET Web API siteler arası istek sahteciliği (CSRF) saldırılarını karşı azaltın](#csrf-api)</li></ul> |

## <a id="logout-adal"></a>Azure AD kullanarak ADAL yöntemleri kullanarak uygulama uygun oturum kapatma

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure AD | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Uygulamanın Azure AD tarafından verilen erişim belirteci dayanıyorsa, oturum kapatma olay işleyicisi çağırmalıdır |

### <a name="example"></a>Örnek
```csharp
HttpContext.GetOwinContext().Authentication.SignOut(OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType)
```

### <a name="example"></a>Örnek
Session.Abandon() yöntemini çağırarak, kullanıcının oturumunu yok. Yöntem, kullanıcı oturum kapatma güvenli uygulanışı gösterilmektedir:
```csharp
    [HttpPost]
        [ValidateAntiForgeryToken]
        public void LogOff()
        {
            string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            AuthenticationContext authContext = new AuthenticationContext(Authority + TenantId, new NaiveSessionCache(userObjectID));
            authContext.TokenCache.Clear();
            Session.Clear();
            Session.Abandon();
            Response.SetCookie(new HttpCookie("ASP.NET_SessionId", string.Empty));
            HttpContext.GetOwinContext().Authentication.SignOut(
                OpenIdConnectAuthenticationDefaults.AuthenticationType,
                CookieAuthenticationDefaults.AuthenticationType);
        } 
```

## <a id="finite-tokens"></a>Sınırlı yaşam süreleri için oluşturulan SaS belirteçlerini kullanma

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT cihaz | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Azure IOT Hub'ına kimlik doğrulaması için oluşturulan SaS belirteçleri sınırlı bir süre sonu dönemi olması gerekir. SaS belirteci ömrü belirteçleri tehlikeye durumunda, çalınabilir süre miktarını sınırlamak için bir en az tutun.|

## <a id="resource-tokens"></a>En düşük belirteç ömrünü için oluşturulan kaynak belirteçleri kullanma

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure belge veritabanı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Gerekli en düşük bir değere kaynak belirtecini süreyi azaltın. Kaynak belirteçleri 1 saatlik varsayılan geçerli zaman vardır.|

## <a id="wsfederation-logout"></a>ADFS kullanırken WsFederation yöntemlerle uygun uygulama oturum kapatma

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | ADFS | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Oturum kapatma olay işleyicisi, STS belirteci ADFS'si tarafından verilen uygulama dayanıyorsa, kullanıcının oturumunu açmak için WSFederationAuthenticationModule.FederatedSignOut() yöntemini çağırmalıdır. Geçerli oturumda ayrıca yok ve oturum belirteci değeri sıfırlamak ve nullified.|

### <a name="example"></a>Örnek
```csharp
        [HttpPost, ValidateAntiForgeryToken]
        [Authorization]
        public ActionResult SignOut(string redirectUrl)
        {
            if (!this.User.Identity.IsAuthenticated)
            {
                return this.View("LogOff", null);
            }

            // Removes the user profile.
            this.Session.Clear();
            this.Session.Abandon();
            HttpContext.Current.Response.Cookies.Add(new System.Web.HttpCookie("ASP.NET_SessionId", string.Empty)
                {
                    Expires = DateTime.Now.AddDays(-1D),
                    Secure = true,
                    HttpOnly = true
                });

            // Signs out at the specified security token service (STS) by using the WS-Federation protocol.
            Uri signOutUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Issuer);
            Uri replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm);
            if (!string.IsNullOrEmpty(redirectUrl))
            {
                replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm + redirectUrl);
            }
           //     Signs out of the current session and raises the appropriate events.
            var authModule = FederatedAuthentication.WSFederationAuthenticationModule;
            authModule.SignOut(false);
        //     Signs out at the specified security token service (STS) by using the WS-Federation
        //     protocol.            
            WSFederationAuthenticationModule.FederatedSignOut(signOutUrl, replyUrl);
            return new RedirectResult(redirectUrl);
        }
```

## <a id="proper-logout"></a>Kimlik sunucusu kullanılırken uygun kapatma gerçekleştir

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Kimlik sunucusu | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [IdentityServer3 Federasyon Oturumu Kapat](https://identityserver.github.io/Documentation/docsv2/advanced/federated-signout.html) |
| **Adımları** | IdentityServer Dış kimlik sağlayıcıları ile federasyona eklemek için özelliğini destekler. Bir Yukarı Akış kimlik sağlayıcısı dışında bir kullanıcı oturum açtığında, kullanılan protokolü bağlı olarak, kullanıcı oturumu kapattığında bir bildirim almak mümkün olabilir. Bunlar aynı zamanda kullanıcı oturum açamaz dolayısıyla istemcilerine bildirmek IdentityServer sağlar. Uygulama ayrıntıları için başvurular bölümdeki belgelere bakın.|

## <a id="https-secure-cookies"></a>HTTPS üzerinden kullanılabilir uygulamaları güvenli tanımlama bilgileri kullanmalıdır

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | EnvironmentType - OnPrem |
| **Başvuruları**              | [Öğesi (ASP.NET Settings Schema) httpCookies](https://msdn.microsoft.com/library/ms228262(v=vs.100).aspx), [HttpCookie.Secure özelliği](https://msdn.microsoft.com/library/system.web.httpcookie.secure.aspx) |
| **Adımları** | Tanımlama bilgileri normal yalnızca kendisi için bunlar kapsamlı etki alanı için erişilebilir. Ne yazık ki, HTTPS üzerinden oluşturulan tanımlama bilgileri, HTTP üzerinden erişilebilir olacak şekilde "etki alanı" tanımını Protokolü içermez. "Güvenli" özniteliği için tarayıcı tanımlama bilgisinin yalnızca HTTPS üzerinden kullanılabilir olması gerektiğini gösterir. Tüm tanımlama bilgilerini üzerinden HTTPS kullanımı ayarlandığından emin olun **güvenli** özniteliği. Gereksinim requireSSL özniteliği true olarak ayarlayarak web.config dosyasında zorunlu tutulabilir. Zorunlu kılacak tercih edilen yaklaşım olmasıdır **güvenli** öznitelik herhangi bir ek kod değişikliği yapmaya gerek kalmadan tüm mevcut ve gelecekteki tanımlama.|

### <a name="example"></a>Örnek
```csharp
<configuration>
  <system.web>
    <httpCookies requireSSL="true"/>
  </system.web>
</configuration>
```
Uygulamaya erişmek için kullanılan HTTP bile ayarı zorunlu kılınır. Uygulamaya erişmek için kullanılan HTTP ayarı uygulama çünkü tanımlama bilgileri güvenli özniteliğiyle ayarlanır ve tarayıcı bunları uygulamaya göndermezler keser.

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Web Forms, MVC5 |
| **Öznitelikler**              | EnvironmentType - OnPrem |
| **Başvuruları**              | Yok  |
| **Adımları** | Bağlı olan taraf web uygulamasıdır ve IDP ADFS sunucusu olduğunda FedAuth belirtecinin güvenli özniteliği içindeki requireSSL öğesini true olarak ayarlanarak yapılandırılabilir `system.identityModel.services` web.config bölümünü:|

### <a name="example"></a>Örnek
```csharp
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:20:0" />
    ....  
    </federationConfiguration>
  </system.identityModel.services>
```

## <a id="cookie-definition"></a>Tüm http tabanlı uygulama yalnızca tanımlama bilgisi tanımı için http belirtmeniz gerekir

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Güvenli bir tanımlama bilgisi özniteliği](https://en.wikipedia.org/wiki/HTTP_cookie#Secure_cookie) |
| **Adımları** | Siteler arası betik (XSS) bir saldırı ile bilgi ifşaatı riskini azaltmak için yeni bir öznitelik - httpOnly - tanımlama bilgilerini kullanıma sunulmuştur ve tüm bilinen tarayıcılar tarafından desteklenir. Öznitelik, bir tanımlama bilgisi betiği aracılığıyla erişilebilir olmadığını belirtir. Bir web uygulaması HttpOnly tanımlama bilgilerini kullanarak tanımlama bilgisine dahil hassas bilgileri betiği aracılığıyla çalınması ve bir saldırganın Web sitesine gönderilen, olasılığını azaltır. |

### <a name="example"></a>Örnek
Tanımlama bilgileri kullanan tüm HTTP tabanlı uygulamalar HttpOnly yapılandırma web.config dosyasında aşağıdaki uygulayarak tanımlama bilgisi tanımında belirtmeniz gerekir:
```XML
<system.web>
.
.
   <httpCookies requireSSL="false" httpOnlyCookies="true"/>
.
.
</system.web>
```

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Web formları |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [FormsAuthentication.RequireSSL özelliği](https://msdn.microsoft.com/library/system.web.security.formsauthentication.requiressl.aspx) |
| **Adımları** | İçindeki requireSSL öğesini özellik değeri, yapılandırma öğesi içindeki requireSSL öğesini özniteliğini kullanarak bir ASP.NET uygulaması için yapılandırma dosyasında ayarlanır. ASP.NET uygulamanız için SSL (Güvenli Yuva Katmanı) içindeki requireSSL öğesini özniteliğini ayarlayarak form kimlik doğrulaması tanımlama bilgisinin sunucuya döndürülmesi gerekip gerekmediğini, Web.config dosyasında belirtebilirsiniz.|

### <a name="example"></a>Örnek 
Aşağıdaki kod örneği, Web.config dosyasında requireSSL özniteliğini ayarlar.
```XML
<authentication mode="Forms">
  <forms loginUrl="member_login.aspx" cookieless="UseCookies" requireSSL="true"/>
</authentication>
```

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | MVC5 |
| **Öznitelikler**              | EnvironmentType - OnPrem |
| **Başvuruları**              | [Windows Identity Foundation (WIF) yapılandırma – Bölüm II](https://blogs.msdn.microsoft.com/alikl/2011/02/01/windows-identity-foundation-wif-configuration-part-ii-cookiehandler-chunkedcookiehandler-customcookiehandler/) |
| **Adımları** | HttpOnly özniteliğini FedAuth tanımlama bilgilerini ayarlamak için hideFromCsript öznitelik değeri True olarak ayarlanması gerekir. |

### <a name="example"></a>Örnek
Aşağıdaki yapılandırmayı doğru yapılandırması gösterilmektedir:
```XML
<federatedAuthentication>
<cookieHandler mode="Custom"
                       hideFromScript="true"
                       name="FedAuth"
                       path="/"
                       requireSsl="true"
                       persistentSessionLifetime="25">
</cookieHandler>
</federatedAuthentication>
```

## <a id="csrf-asp"></a>ASP.NET web sayfaları siteler arası istek sahteciliği (CSRF) saldırılarını karşı azaltın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Siteler arası istek sahteciliği (CSRF veya XSRF), bir saldırganın güvenlik bağlamında farklı bir kullanıcının bir web sitesi kurulan oturum eylemlerini gerçekleştirebilirsiniz saldırı türüdür. Hedeflenen web sitesi alınan istek kimliğini doğrulamak için yalnızca oturum tanımlama dayanıyorsa değiştirin veya içeriği silme olmaktır. Bir saldırgan, üzerinde kullanıcı zaten oturum savunmasız bir siteden bir URL ile bir komut yüklemek için farklı bir kullanıcının tarayıcı alarak bu güvenlik açığından yararlanabilir. Bir bağlantıya tıklayabilecekleri gibi güvenlik açığı sunucudan kaynak yükler farklı bir web sitesi barındırma veya kullanıcı alınıyor bunun, bir saldırganın birçok yolu vardır. Sunucu istemciye bir ek belirteç gönderir, bu belirteci içindeki tüm gelecek istekleri dahil etmek istemcinin ihtiyaç duyduğu ve gelecekteki tüm istekleri ASP.NET'i kullanarak gibi geçerli oturum için ilgili bir belirteç içerdiğini doğrular, saldırı engellenebilir AntiForgeryToken veya Görünüm durumu. |

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | MVC5, MVC6 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [ASP.NET MVC ve Web sayfalarında XSRF/CSRF önleme](https://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) |
| **Adımları** | Anti-CSRF ve ASP.NET MVC formları - kullanma `AntiForgeryToken` yardımcı yöntem görünümleri; put bir `Html.AntiForgeryToken()` forma, örneğin,|

### <a name="example"></a>Örnek
```csharp
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
```

### <a name="example"></a>Örnek
```csharp
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a>Örnek
Aynı anda Html.AntiForgeryToken() ziyaretçi __RequestVerificationToken, yukarıda gösterilen rastgele bir gizli değer olarak aynı değere sahip olarak adlandırılan bir tanımlama bilgisi sağlar. Ardından, gelen bir form post doğrulamak için hedef eylem yöntemine [ValidateAntiForgeryToken] filtresi ekleyin. Örneğin:
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
Denetleyen yetkilendirme Filtresi:
* Gelen istek __RequestVerificationToken adlı bir tanımlama bilgisi içeriyor
* Gelen istekte bir `Request.Form` __RequestVerificationToken adlı giriş
* Bu tanımlama bilgisi ve `Request.Form` varsayarak tüm değerleri Eşleştir iyi, istek geçtiği normal olarak. Ancak Aksi takdirde, ardından bir Yetkilendirme hatası iletisi "gerekli bir sahteciliğe karşı koruma belirteci sağlanmadı ya da geçersiz". 

### <a name="example"></a>Örnek
Ve AJAX CSRF önleme: Form simgesi HTML form verileri JSON verilerini bir AJAX isteği gönderebilir AJAX istekleri için bir sorun olabilir. Tek bir çözüm, özel bir HTTP üst bilgisinde belirteçleri göndermektir. Aşağıdaki kod, belirteçleri oluşturmak için Razor sözdizimini kullanır ve ardından belirteçleri için bir AJAX isteği ekler. 
```csharp
<script>
    @functions{
        public string TokenHeaderValue()
        {
            string cookieToken, formToken;
            AntiForgery.GetTokens(null, out cookieToken, out formToken);
            return cookieToken + ":" + formToken;                
        }
    }

    $.ajax("api/values", {
        type: "post",
        contentType: "application/json",
        data: {  }, // JSON data goes here
        dataType: "json",
        headers: {
            'RequestVerificationToken': '@TokenHeaderValue()'
        }
    });
</script>
```

### <a name="example"></a>Örnek
İsteği işlerken, istek üstbilgisi belirteçleri ayıklayın. Ardından belirteçlerini doğrulamaya yönelik AntiForgery.Validate yöntemi çağırın. Validate yöntemi belirteçleri geçerli olmayan bir özel durum oluşturur.
```csharp
void ValidateRequestHeader(HttpRequestMessage request)
{
    string cookieToken = "";
    string formToken = "";

    IEnumerable<string> tokenHeaders;
    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
    {
        string[] tokens = tokenHeaders.First().Split(':');
        if (tokens.Length == 2)
        {
            cookieToken = tokens[0].Trim();
            formToken = tokens[1].Trim();
        }
    }
    AntiForgery.Validate(cookieToken, formToken);
}
```

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Web formları |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Web saldırılarına Fend için ASP.NET yerleşik özelliklerinden yararlanmak](https://msdn.microsoft.com/library/ms972969.aspx#securitybarriers_topic2) |
| **Adımları** | WebForm tabanlı uygulamaları CSRF saldırılarında ViewStateUserKey değişen rastgele bir dize için - her kullanıcı için kullanıcı kimliği ayarlayarak azaltılabilir veya henüz, daha iyi bir oturum kimliği ID öngörülemeyen, oturum zaman aşımına uğrar ve kullanıcı başına esasına göre değişir olduğundan bir dizi teknik ve sosyal nedenden ötürü oturum çok daha iyi uyum kimliğidir.|

### <a name="example"></a>Örnek
Sayfalarınızın tümünü ihtiyacınız kod aşağıdaki gibidir:
```csharp
void Page_Init (object sender, EventArgs e) {
   ViewStateUserKey = Session.SessionID;
   :
}
```

## <a id="inactivity-lifetime"></a>Etkinlik yaşam süresi için oturumu ayarlama

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [HttpSessionState.Timeout özelliği](https://msdn.microsoft.com/library/system.web.sessionstate.httpsessionstate.timeout(v=vs.110).aspx) |
| **Adımları** | Oturum zaman aşımı, kullanıcı herhangi bir işlem bir web sitesinde (web sunucusu tarafından tanımlanan) bir aralık boyunca gerçekleştirmez sırasında oluşan olayı temsil eder. Sunucu tarafında olay kullanıcı oturumunun durumunu (örneğin "artık kullanılmıyor") 'için geçersiz' değiştirmek ve web sunucusu (içine yer alan tüm verileri silme) edilecek isteyin. Aşağıdaki kod örneği, Web.config dosyasında 15 dakika ile oturum zaman aşımı özniteliğini ayarlar.|

### <a name="example"></a>Örnek
```XML 
<configuration>
  <system.web>
    <sessionState mode="InProc" cookieless="true" timeout="15" />
  </system.web>
</configuration>
```

## <a id="threat-detection"></a>Azure SQL tehdit algılamayı etkinleştirme

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Web formları |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Kimlik doğrulama (ASP.NET Settings Schema) için form öğesi](https://msdn.microsoft.com/library/1d3t3c61(v=vs.100).aspx) |
| **Adımları** | Forms kimlik doğrulaması bileti tanımlama bilgisi zaman aşımı 15 dakika olarak ayarlayın.|

### <a name="example"></a>Örnek
```XML
<forms  name=".ASPXAUTH" loginUrl="login.aspx"  defaultUrl="default.aspx" protection="All" timeout="15" path="/" requireSSL="true" slidingExpiration="true"/>
</forms>
```

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Web Forms, MVC5 |
| **Öznitelikler**              | EnvironmentType - OnPrem |
| **Başvuruları**              | [asdeqa](https://skf.azurewebsites.net/Mitigations/Details/wefr) |
| **Adımları** | Bağlı olan taraf web uygulamasıdır ve AD FS STS olduğunda web.config dosyasında aşağıdaki yapılandırma ile kimlik doğrulama tanımlama bilgisi - FedAuth belirteçleri - ömrünü ayarlayabilirsiniz:|

### <a name="example"></a>Örnek
```XML
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:15:0" />
      <!-- Set requireHttps=true; -->
      <wsFederation passiveRedirectEnabled="true" issuer="http://localhost:39529/" realm="https://localhost:44302/" reply="https://localhost:44302/" requireHttps="true"/>
      <!--
      Use the code below to enable encryption-decryption of claims received from ADFS. Thumbprint value varies based on the certificate being used.
      <serviceCertificate>
        <certificateReference findValue="4FBBBA33A1D11A9022A5BF3492FF83320007686A" storeLocation="LocalMachine" storeName="My" x509FindType="FindByThumbprint" />
      </serviceCertificate>
      -->
    </federationConfiguration>
  </system.identityModel.services>
```

### <a name="example"></a>Örnek
Ayrıca ADFS verilen SAML belirtecinin ömrü 15 dakika, ADFS sunucusunda aşağıdaki powershell komutunu yürüterek ayarlanmalıdır talep:
```csharp
Set-ADFSRelyingPartyTrust -TargetName “<RelyingPartyWebApp>” -ClaimsProviderName @(“Active Directory”) -TokenLifetime 15 -AlwaysRequireAuthentication $true
```

## <a id="proper-app-logout"></a>Uygulamadan uygun oturum kapatma gerçekleştir

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Doğru oturum kapatma düğmesi basarsa kullanıcı oturum açtığında uygulamadan gerçekleştirin. Oturum kapatma uygulama kullanıcının oturumu yok ve ayrıca sıfırlamak ve oturum tanımlama bilgisi değeri sıfırlamak ve kimlik doğrulama tanımlama bilgisi değeri nullifying birlikte silinmez. Birden çok oturumu tek kullanıcı kimliğine bağlıdır, ayrıca, bunlar topluca sunucu tarafında zaman aşımı veya oturum kapatma ile bitmelidir. Son olarak, oturum kapatma işlevi her sayfada kullanılabilir olduğundan emin olun. |

## <a id="csrf-api"></a>ASP.NET Web API siteler arası istek sahteciliği (CSRF) saldırılarını karşı azaltın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Siteler arası istek sahteciliği (CSRF veya XSRF), bir saldırganın güvenlik bağlamında farklı bir kullanıcının bir web sitesi kurulan oturum eylemlerini gerçekleştirebilirsiniz saldırı türüdür. Hedeflenen web sitesi alınan istek kimliğini doğrulamak için yalnızca oturum tanımlama dayanıyorsa değiştirin veya içeriği silme olmaktır. Bir saldırgan, üzerinde kullanıcı zaten oturum savunmasız bir siteden bir URL ile bir komut yüklemek için farklı bir kullanıcının tarayıcı alarak bu güvenlik açığından yararlanabilir. Bir bağlantıya tıklayabilecekleri gibi güvenlik açığı sunucudan kaynak yükler farklı bir web sitesi barındırma veya kullanıcı alınıyor bunun, bir saldırganın birçok yolu vardır. Sunucu istemciye bir ek belirteç gönderir, bu belirteci içindeki tüm gelecek istekleri dahil etmek istemcinin ihtiyaç duyduğu ve gelecekteki tüm istekleri ASP.NET'i kullanarak gibi geçerli oturum için ilgili bir belirteç içerdiğini doğrular, saldırı engellenebilir AntiForgeryToken veya Görünüm durumu. |

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | MVC5, MVC6 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [ASP.NET Web API'de siteler arası istek sahteciliği (CSRF) saldırılarını önleme](https://www.asp.net/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks) |
| **Adımları** | Ve AJAX CSRF önleme: Form simgesi HTML form verileri JSON verilerini bir AJAX isteği gönderebilir AJAX istekleri için bir sorun olabilir. Tek bir çözüm, özel bir HTTP üst bilgisinde belirteçleri göndermektir. Aşağıdaki kod, belirteçleri oluşturmak için Razor sözdizimini kullanır ve ardından belirteçleri için bir AJAX isteği ekler. |

### <a name="example"></a>Örnek
```Javascript
<script>
    @functions{
        public string TokenHeaderValue()
        {
            string cookieToken, formToken;
            AntiForgery.GetTokens(null, out cookieToken, out formToken);
            return cookieToken + ":" + formToken;                
        }
    }
    $.ajax("api/values", {
        type: "post",
        contentType: "application/json",
        data: {  }, // JSON data goes here
        dataType: "json",
        headers: {
            'RequestVerificationToken': '@TokenHeaderValue()'
        }
    });
</script>
```

### <a name="example"></a>Örnek
İsteği işlerken, istek üstbilgisi belirteçleri ayıklayın. Ardından belirteçlerini doğrulamaya yönelik AntiForgery.Validate yöntemi çağırın. Validate yöntemi belirteçleri geçerli olmayan bir özel durum oluşturur.
```csharp
void ValidateRequestHeader(HttpRequestMessage request)
{
    string cookieToken = "";
    string formToken = "";

    IEnumerable<string> tokenHeaders;
    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
    {
        string[] tokens = tokenHeaders.First().Split(':');
        if (tokens.Length == 2)
        {
            cookieToken = tokens[0].Trim();
            formToken = tokens[1].Trim();
        }
    }
    AntiForgery.Validate(cookieToken, formToken);
}
```

### <a name="example"></a>Örnek
Anti-CSRF ve ASP.NET MVC forms - AntiForgeryToken yardımcı yöntemi görünümleri kullanın. Örneğin, bir Html.AntiForgeryToken() forma koyun,
```csharp
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
}
```

### <a name="example"></a>Örnek
Yukarıdaki örnekte, aşağıdaki gibi çıktı:
```csharp
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a>Örnek
Aynı anda Html.AntiForgeryToken() ziyaretçi __RequestVerificationToken, yukarıda gösterilen rastgele bir gizli değer olarak aynı değere sahip olarak adlandırılan bir tanımlama bilgisi sağlar. Ardından, gelen bir form post doğrulamak için hedef eylem yöntemine [ValidateAntiForgeryToken] filtresi ekleyin. Örneğin:
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
Denetleyen yetkilendirme Filtresi:
* Gelen istek __RequestVerificationToken adlı bir tanımlama bilgisi içeriyor
* Gelen istekte bir `Request.Form` __RequestVerificationToken adlı giriş
* Bu tanımlama bilgisi ve `Request.Form` varsayarak tüm değerleri Eşleştir iyi, istek geçtiği normal olarak. Ancak Aksi takdirde, ardından bir Yetkilendirme hatası iletisi "gerekli bir sahteciliğe karşı koruma belirteci sağlanmadı ya da geçersiz".

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | MVC5, MVC6 |
| **Öznitelikler**              | Kimlik sağlayıcısı - AD FS, kimlik sağlayıcısı - Azure AD |
| **Başvuruları**              | [Bireysel hesaplar ve ASP.NET Web API 2.2 sürümünde yerel oturum açma ile bir Web API'SİNİN güvenliğini sağlama](https://www.asp.net/web-api/overview/security/individual-accounts-in-web-api) |
| **Adımları** | Web API'si ise OAuth 2.0 kullanarak güvenli sonra yetkilendirme istek üst bilgisinde taşıyıcı belirteç bekliyor ve yalnızca belirteç geçerliyse, istek erişim verir. Tanımlama bilgisi tabanlı kimlik doğrulamasının aksine, tarayıcılar isteklerine taşıyıcı belirteçleri eklemeyin. İstekte bulunan istemciye istek üstbilgisinde taşıyıcı belirteç açıkça eklemeniz gerekir. Bu nedenle, OAuth 2.0 kullanarak korumalı ASP.NET Web API'leri için savunma CSRF saldırılarına karşı olarak taşıyıcı belirteçleri kabul edilir. Lütfen uygulamanın MVC kısmı, form kimlik doğrulaması (yani, tanımlama bilgileri kullanır) kullanıyorsa, sahteciliğe karşı koruma belirteçleri MVC web uygulaması tarafından kullanılması gerektiğini unutmayın. |

### <a name="example"></a>Örnek
Web API'si, yalnızca üzerinde taşıyıcı belirteçleri ve çalıştırılmadı tanımlama bilgilerini yararlanmayı bilgilendirilmesi sahiptir. Aşağıdaki yapılandırmayı, yapılabilir `WebApiConfig.Register` yöntemi:

```csharp
config.SuppressDefaultHostAuthentication();
config.Filters.Add(new HostAuthenticationFilter(OAuthDefaults.AuthenticationType));
```

SuppressDefaultHostAuthentication yöntemi Web API'si, Web API ardışık düzeni, IIS veya OWIN ara yazılımı tarafından istek ulaşmadan önce gerçekleşen herhangi bir kimlik doğrulaması yok saymasını söyler. Bu şekilde, biz yalnızca taşıyıcı belirteçlerini kullanarak kimlik doğrulaması yapmak için Web API'si kısıtlayabilirsiniz.
