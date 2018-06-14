---
title: Oturum yönetimi - Microsoft tehdit modelleme aracı - Azure | Microsoft Docs
description: Azaltıcı Etkenler tehdit modelleme Aracı kullanıma sunulan tehditleri
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 24bd0e8eff616920dba0eb5353f983444e3161cd
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28019968"
---
# <a name="security-frame-session-management--articles"></a>Güvenlik çerçevesi: Oturum yönetimi | Makaleler 
| Ürün/hizmet | Makale |
| --------------- | ------- |
| **Azure AD**    | <ul><li>[Azure AD kullanırken ADAL yöntemleri kullanarak uygulama uygun oturum kapatma](#logout-adal)</li></ul> |
| IOT cihaz | <ul><li>[Sonlu yaşam süreleri için oluşturulan SaS belirteci kullanın](#finite-tokens)</li></ul> |
| **Azure belge DB** | <ul><li>[Minimum belirteci yaşam süreleri için oluşturulan kaynak belirteçleri kullanın](#resource-tokens)</li></ul> |
| **ADFS** | <ul><li>[ADFS kullanırken WsFederation yöntemleri kullanarak uygulama uygun oturum kapatma](#wsfederation-logout)</li></ul> |
| **Identity Server** | <ul><li>[Kimlik sunucusu kullanılırken uygulama uygun oturum kapatma](#proper-logout)</li></ul> |
| **Web uygulaması** | <ul><li>[HTTPS üzerinden kullanılabilir uygulamaları güvenli tanımlama bilgileri kullanmalıdır](#https-secure-cookies)</li><li>[Tüm http tabanlı uygulama http tanımlama bilgisi tanımı için yalnızca belirtmeniz gerekir](#cookie-definition)</li><li>[ASP.NET web sayfaları siteler arası istek sahteciliği (CSRF) saldırılarını karşı azaltmak](#csrf-asp)</li><li>[Oturum etkin olmama ömrü için ayarlama](#inactivity-lifetime)</li><li>[Uygulama uygulamadan uygun oturum kapatma](#proper-app-logout)</li></ul> |
| **Web API** | <ul><li>[ASP.NET Web API siteler arası istek sahteciliği (CSRF) saldırılarını karşı azaltmak](#csrf-api)</li></ul> |

## <a id="logout-adal"></a>Azure AD kullanırken ADAL yöntemleri kullanarak uygulama uygun oturum kapatma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure AD | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Uygulama Azure AD tarafından verilen erişim belirtecini kullanır, oturum kapatma olay işleyicisi çağırmalıdır |

### <a name="example"></a>Örnek
```csharp
HttpContext.GetOwinContext().Authentication.SignOut(OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType)
```

### <a name="example"></a>Örnek
Session.Abandon() yöntemini çağırarak, kullanıcının oturumunu destroy. Yöntemini aşağıdaki kullanıcı oturum kapatma güvenli uygulamasını gösterir:
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

## <a id="finite-tokens"></a>Sonlu yaşam süreleri için oluşturulan SaS belirteci kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT cihaz | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Azure IOT Hub'ına kimlik doğrulaması için oluşturulan SaS belirteçleri sınırlı süre sonu dönemi olması gerekir. SaS belirteci yaşam süreleri belirteçleri güvenliğinin ihlal edilmesi durumunda bunlar çalınabilir süre miktarını sınırlamak için en düşük tutun.|

## <a id="resource-tokens"></a>Minimum belirteci yaşam süreleri için oluşturulan kaynak belirteçleri kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure belge DB | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Kaynak belirteci timespan gerekli en düşük değer azaltın. Kaynak belirteçleri 1 saatlik varsayılan geçerli timespan vardır.|

## <a id="wsfederation-logout"></a>ADFS kullanırken WsFederation yöntemleri kullanarak uygulama uygun oturum kapatma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | ADFS | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Uygulama tarafından ADFS STS belirteç dayalıysa, oturum kapatma olay işleyicisi için kullanıcının oturum açması için WSFederationAuthenticationModule.FederatedSignOut() yöntemini çağırmalıdır. Geçerli oturumu de yok edilmesi ve oturum belirteç değeri sıfırlamak ve nullified.|

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

## <a id="proper-logout"></a>Kimlik sunucusu kullanılırken uygulama uygun oturum kapatma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Kimlik sunucusu | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [IdentityServer3 federe oturum kapatma](https://identityserver.github.io/Documentation/docsv2/advanced/federated-signout.html) |
| **Adımları** | IdentityServer Dış kimlik sağlayıcıları ile birleştirmek özelliğini destekler. Bir Yukarı Akış kimlik sağlayıcısı dışında bir kullanıcı oturum açtığında kullanılan, protokol bağlı olarak kullanıcı oturumu kapattığında bir bildirim almak mümkün olabilir. Bunlar daha da kullanıcı oturumu şekilde istemcilerine bildirmek IdentityServer sağlar. Uygulama ayrıntıları için başvurular bölümdeki belgelere bakın.|

## <a id="https-secure-cookies"></a>HTTPS üzerinden kullanılabilir uygulamaları güvenli tanımlama bilgileri kullanmalıdır

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | EnvironmentType - OnPrem |
| **Başvuruları**              | [httpCookies Ögesi (ASP.NET Ayarlar Şeması)](http://msdn.microsoft.com/library/ms228262(v=vs.100).aspx), [HttpCookie.Secure özelliği](http://msdn.microsoft.com/library/system.web.httpcookie.secure.aspx) |
| **Adımları** | Tanımlama bilgileri normal olarak yalnızca, bunlar kapsamlı etki alanı için erişilebilir. Ne yazık ki, HTTPS üzerinden oluşturulan tanımlama bilgilerini HTTP üzerinden erişilebilir olması için "etki alanı" tanımını Protokolü içermez. "Güvenli" özniteliği, tarayıcıda tanımlama bilgisinin yalnızca HTTPS üzerinden kullanılabilir olması gerektiğini belirtir. Tüm tanımlama bilgilerini üzerinden HTTPS kullanımı ayarlandığından emin olun **güvenli** özniteliği. Gereksinim requireSSL özniteliği true olarak ayarlayarak web.config dosyasında uygulanabilir. Zorunlu kılacak tercih edilen yaklaşım demektir **güvenli** özniteliği için ek kod değişiklikleri yapmak zorunda kalmadan tüm geçerli ve gelecekteki olan tanımlama bilgileri.|

### <a name="example"></a>Örnek
```csharp
<configuration>
  <system.web>
    <httpCookies requireSSL="true"/>
  </system.web>
</configuration>
```
HTTP uygulamaya erişmek için kullanılsa bile ayarı zorunlu kılınır. Uygulamaya erişmek için HTTP kullandıysanız, çünkü tanımlama bilgileri güvenli özniteliği ile ayarlanır ve tarayıcı bunları geri uygulamaya göndermez ayarı uygulama keser.

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Web Forms, MVC5 |
| **Öznitelikleri**              | EnvironmentType - OnPrem |
| **Başvuruları**              | Yok  |
| **Adımları** | Bağlı olan taraf web uygulamasıdır ve IDP ADFS sunucusu olduğunda FedAuth belirtecin güvenli özniteliği requireSSL True olarak ayarlanarak yapılandırılabilir `system.identityModel.services` web.config bölümünü:|

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

## <a id="cookie-definition"></a>Tüm http tabanlı uygulama http tanımlama bilgisi tanımı için yalnızca belirtmeniz gerekir

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Güvenli tanımlama bilgisi özniteliği](https://en.wikipedia.org/wiki/HTTP_cookie#Secure_cookie) |
| **Adımları** | Siteler arası (XSS) saldırısı ile bilgi ifşaatı riskini azaltmak için yeni bir öznitelik - httpOnly - tanımlama bilgileri için sunulmuştur ve önde gelen tüm tarayıcılar tarafından desteklenir. Öznitelik, bir tanımlama bilgisi komut dosyası aracılığıyla erişilebilir değil belirtir. HttpOnly tanımlama bilgilerini kullanarak bir web uygulaması tanımlama bilgisine dahil hassas bilgileri komut dosyası çalınması ve bir saldırganın Web sitesine gönderilen olduğunu olasılığını azaltır. |

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

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Web formları |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [FormsAuthentication.RequireSSL özelliği](https://msdn.microsoft.com/library/system.web.security.formsauthentication.requiressl.aspx) |
| **Adımları** | RequireSSL özellik değeri, yapılandırma öğesinin requireSSL özniteliğini kullanarak bir ASP.NET uygulaması için yapılandırma dosyasında ayarlanır. ASP.NET uygulamanız için SSL (Güvenli Yuva Katmanı) requireSSL özniteliğini ayarlayarak form kimlik doğrulaması tanımlama bilgisinin sunucuya döndürülmesi gerekip gerekmediğini, Web.config dosyasında belirtebilirsiniz.|

### <a name="example"></a>Örnek 
Aşağıdaki kod örneğinde requireSSL özniteliği Web.config dosyasında ayarlar.
```XML
<authentication mode="Forms">
  <forms loginUrl="member_login.aspx" cookieless="UseCookies" requireSSL="true"/>
</authentication>
```

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | MVC5 |
| **Öznitelikleri**              | EnvironmentType - OnPrem |
| **Başvuruları**              | [Windows Identity Foundation (WIF) yapılandırması – Bölüm II](https://blogs.msdn.microsoft.com/alikl/2011/02/01/windows-identity-foundation-wif-configuration-part-ii-cookiehandler-chunkedcookiehandler-customcookiehandler/) |
| **Adımları** | FedAuth tanımlama bilgilerini httpOnly özniteliğini ayarlamak için hideFromCsript öznitelik değeri True olarak ayarlanması gerekir. |

### <a name="example"></a>Örnek
Aşağıdaki yapılandırma doğru yapılandırması gösterilmektedir:
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

## <a id="csrf-asp"></a>ASP.NET web sayfaları siteler arası istek sahteciliği (CSRF) saldırılarını karşı azaltmak

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Siteler arası istek sahtekarlığı (CSRF veya XSRF), bir saldırganın bir web sitesi kurulan oturum farklı bir kullanıcının güvenlik bağlamında eylemleri gerçekleştirebilirsiniz saldırı türüdür. Hedeflenen web sitesi alınan istek kimliğini doğrulamak için özel olarak oturum tanımlama dayalıysa değiştirmek veya içerik silmek için belirtilir. Bir saldırgan, üzerinde kullanıcı zaten oturum açık bir siteden bir komutla bir URL yüklemek için farklı bir kullanıcının tarayıcı alarak bu güvenlik açığından yararlanabilir. Bir bağlantıyı, gibi bir kaynak savunmasız sunucusundan yükler farklı bir web sitesi barındırma veya kullanıcı alma Bunu yapmak bir saldırganın birçok yolu vardır. Sunucu istemciye bir ek belirteç, belirtecini de gelecekteki tüm istekler dahil etmek istemcinin gerektiriyorsa ve tüm gelecekteki isteklerin ASP.NET kullanılarak gibi geçerli oturum için ilgili bir belirteç dahil olduğunu doğrular, saldırı önlenebilir AntiForgeryToken veya Görünüm durumu. |

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | MVC5, MVC6 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [ASP.NET MVC ve Web sayfaları XSRF/CSRF önleme](http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) |
| **Adımları** | Anti-CSRF ve ASP.NET MVC formları - kullanma `AntiForgeryToken` yardımcı yöntemi görünümleri; put bir `Html.AntiForgeryToken()` forma, örneğin,|

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
Aynı anda Html.AntiForgeryToken() ziyaretçi yukarıda gösterilen rastgele gizli değer ile aynı değere sahip __RequestVerificationToken adlı bir tanımlama bilgisi sağlar. Ardından, gelen bir form post doğrulamak için hedef eylem yöntemine [ValidateAntiForgeryToken] filtresini ekleyin. Örneğin:
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
Denetleyen yetkilendirme Filtresi:
* Gelen istek __RequestVerificationToken adlı bir tanımlama bilgisi içeriyor
* Gelen istek sahip bir `Request.Form` __RequestVerificationToken adlı giriş
* Bu tanımlama bilgisi ve `Request.Form` varsayılarak tüm değerleri Eşleştir iyi, istek geçtiği normal olarak. Ancak değilse, ardından bir Yetkilendirme hatası iletisi "gerekli sahteciliğe karşı koruma belirteci belirtilmedi veya geçersiz". 

### <a name="example"></a>Örnek
Anti-CSRF ve AJAX: JSON verilerini, HTML form verilerini bir AJAX İsteği Gönder çünkü form simgesi AJAX istekleri için bir sorun olabilir. Bir çözüm, özel bir HTTP üstbilgisi belirteçleri göndermektir. Aşağıdaki kod belirteçleri oluşturmak için Razor sözdizimini kullanır ve ardından bir AJAX isteği belirteçleri ekler. 
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
İsteği işlerken, istek üstbilgisi belirteçleri ayıklayın. Ardından belirteçleri doğrulamak için AntiForgery.Validate yöntemini çağırın. Belirteçleri geçerli değilse doğrulama yöntemi bir özel durum oluşturur.
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

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Web formları |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Web saldırıları Fend için ASP.NET yerleşik özelliklerden yararlanabilir](https://msdn.microsoft.com/library/ms972969.aspx#securitybarriers_topic2) |
| **Adımları** | CSRF saldırılarını WebForm tabanlı uygulamalarda - her kullanıcı için kullanıcı kimliği değişen rastgele bir dize ViewStateUserKey ayarlayarak azaltılabilir veya, henüz, oturum kimliği daha iyi Kimliktir öngörülemeyen, oturum zaman aşımına uğradı ve bir kullanıcı başına temelinde değişir olduğundan bir teknik ve sosyal nedeniyle için oturum kimliği daha iyi bir uyum sayısıdır.|

### <a name="example"></a>Örnek
Sayfalarınızın tümünü gerek kod aşağıdaki gibidir:
```csharp
void Page_Init (object sender, EventArgs e) {
   ViewStateUserKey = Session.SessionID;
   :
}
```

## <a id="inactivity-lifetime"></a>Oturum etkin olmama ömrü için ayarlama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [HttpSessionState.Timeout özelliği](https://msdn.microsoft.com/library/system.web.sessionstate.httpsessionstate.timeout(v=vs.110).aspx) |
| **Adımları** | Oturum zaman aşımı, kullanıcı herhangi bir eylem bir web sitesinde (web sunucusu tarafından tanımlanan) bir aralık boyunca gerçekleştirmez zaman gerçekleşen olayını temsil eder. Sunucu tarafında olay (örneğin "artık kullanılmıyor") kullanıcı oturumunun durumu 'için geçersiz' değiştirebilir ve bunu (içine bulunan tüm verileri silme) yok etmek için web sunucusu isteyin. Aşağıdaki kod örneğinde zaman aşımı oturum özniteliği Web.config dosyasında 15 dakika olarak ayarlar.|

### <a name="example"></a>Örnek
'''XML kodunu <configuration> < system.web > <sessionState mode="InProc" cookieless="true" timeout="15" /> < /system.web ></configuration>
```

## <a id="threat-detection"></a>Enable Threat detection on Azure SQL
```

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Web formları |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Öğe forms kimlik doğrulaması için (ASP.NET Ayarlar Şeması)](https://msdn.microsoft.com/library/1d3t3c61(v=vs.100).aspx) |
| **Adımları** | Forms kimlik doğrulaması bileti tanımlama bilgisi zaman aşımı 15 dakika olarak ayarlayın|

### <a name="example"></a>Örnek
'''XML kodu<forms  name=".ASPXAUTH" loginUrl="login.aspx"  defaultUrl="default.aspx" protection="All" timeout="15" path="/" requireSSL="true" slidingExpiration="true"/>
</forms>
```

| Title                   | Details      |
| ----------------------- | ------------ |
| **Component**               | Web Application | 
| **SDL Phase**               | Build |  
| **Applicable Technologies** | Web Forms, MVC5 |
| **Attributes**              | EnvironmentType - OnPrem |
| **References**              | [asdeqa](https://skf.azurewebsites.net/Mitigations/Details/wefr) |
| **Steps** | When the web application is Relying Party and ADFS is the STS, the lifetime of the authentication cookies - FedAuth tokens - can be set by the following configuration in web.config:|

### Example
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
Ayrıca belirtecin yaşam süresi 15 dakika, ADFS sunucusunda aşağıdaki powershell komutunu yürüterek ayarlamanız gerekir SAML verilen ADFS talep:
```csharp
Set-ADFSRelyingPartyTrust -TargetName “<RelyingPartyWebApp>” -ClaimsProviderName @(“Active Directory”) -TokenLifetime 15 -AlwaysRequireAuthentication $true
```

## <a id="proper-app-logout"></a>Uygulama uygulamadan uygun oturum kapatma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Doğru oturum kapatma düğmesi kullanıcı basarsa oturum açtığınızda uygulamadan gerçekleştirin. Oturum kapatma sırasında uygulama kullanıcının oturumunu destroy de sıfırlama ve sıfırlama ve kimlik doğrulama tanımlama bilgisi değeri nullifying yanı sıra oturum tanımlama bilgisi değerini iptal edilmez. Birden çok oturumu tek bir kullanıcı kimliğine bağlıdır, ayrıca, bunlar topluca sunucu tarafındaki zaman aşımı veya oturum kapatma ile bitmelidir. Son olarak, her sayfada oturum kapatma işlevselliği kullanılabilir olduğundan emin olun. |

## <a id="csrf-api"></a>ASP.NET Web API siteler arası istek sahteciliği (CSRF) saldırılarını karşı azaltmak

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Siteler arası istek sahtekarlığı (CSRF veya XSRF), bir saldırganın bir web sitesi kurulan oturum farklı bir kullanıcının güvenlik bağlamında eylemleri gerçekleştirebilirsiniz saldırı türüdür. Hedeflenen web sitesi alınan istek kimliğini doğrulamak için özel olarak oturum tanımlama dayalıysa değiştirmek veya içerik silmek için belirtilir. Bir saldırgan, üzerinde kullanıcı zaten oturum açık bir siteden bir komutla bir URL yüklemek için farklı bir kullanıcının tarayıcı alarak bu güvenlik açığından yararlanabilir. Bir bağlantıyı, gibi bir kaynak savunmasız sunucusundan yükler farklı bir web sitesi barındırma veya kullanıcı alma Bunu yapmak bir saldırganın birçok yolu vardır. Sunucu istemciye bir ek belirteç, belirtecini de gelecekteki tüm istekler dahil etmek istemcinin gerektiriyorsa ve tüm gelecekteki isteklerin ASP.NET kullanılarak gibi geçerli oturum için ilgili bir belirteç dahil olduğunu doğrular, saldırı önlenebilir AntiForgeryToken veya Görünüm durumu. |

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | MVC5, MVC6 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [ASP.NET Web API'de siteler arası istek sahtekarlığı (CSRF) saldırılarını önleme](http://www.asp.net/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks) |
| **Adımları** | Anti-CSRF ve AJAX: JSON verilerini, HTML form verilerini bir AJAX İsteği Gönder çünkü form simgesi AJAX istekleri için bir sorun olabilir. Bir çözüm, özel bir HTTP üstbilgisi belirteçleri göndermektir. Aşağıdaki kod belirteçleri oluşturmak için Razor sözdizimini kullanır ve ardından bir AJAX isteği belirteçleri ekler. |

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
İsteği işlerken, istek üstbilgisi belirteçleri ayıklayın. Ardından belirteçleri doğrulamak için AntiForgery.Validate yöntemini çağırın. Belirteçleri geçerli değilse doğrulama yöntemi bir özel durum oluşturur.
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
Anti-CSRF ve ASP.NET MVC formları - AntiForgeryToken yardımcı yöntemi görünümleri kullanma; Örneğin, bir Html.AntiForgeryToken() forma, put,
```csharp
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
}
```

### <a name="example"></a>Örnek
Yukarıdaki örnekte, aşağıdakine benzer çıktı:
```csharp
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a>Örnek
Aynı anda Html.AntiForgeryToken() ziyaretçi yukarıda gösterilen rastgele gizli değer ile aynı değere sahip __RequestVerificationToken adlı bir tanımlama bilgisi sağlar. Ardından, gelen bir form post doğrulamak için hedef eylem yöntemine [ValidateAntiForgeryToken] filtresini ekleyin. Örneğin:
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
Denetleyen yetkilendirme Filtresi:
* Gelen istek __RequestVerificationToken adlı bir tanımlama bilgisi içeriyor
* Gelen istek sahip bir `Request.Form` __RequestVerificationToken adlı giriş
* Bu tanımlama bilgisi ve `Request.Form` varsayılarak tüm değerleri Eşleştir iyi, istek geçtiği normal olarak. Ancak değilse, ardından bir Yetkilendirme hatası iletisi "gerekli sahteciliğe karşı koruma belirteci belirtilmedi veya geçersiz".

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | MVC5, MVC6 |
| **Öznitelikleri**              | Kimlik sağlayıcısı - ADFS, kimlik sağlayıcısı - Azure AD |
| **Başvuruları**              | [Bireysel hesaplar ve ASP.NET Web API 2.2 yerel oturum açma ile Web API güvenliğini sağlama](http://www.asp.net/web-api/overview/security/individual-accounts-in-web-api) |
| **Adımları** | Web API ise OAuth 2.0 kullanan güvenli sonra bir taşıyıcı belirteci yetkilendirme istek üstbilgisinde bekler ve yalnızca belirteç geçerliyse istek erişim verir. Tanımlama bilgisi tabanlı kimlik doğrulaması, tarayıcılar isteklerine taşıyıcı belirteçlerini eklemeyin. İstek üstbilgisinde taşıyıcı belirteci açıkça eklemek istekte bulunan istemci gerekir. Bu nedenle, OAuth 2.0 kullanan korumalı ASP.NET Web API için taşıyıcı belirteçlerini CSRF saldırılarına karşı savunma hattı olarak değerlendirilir. Lütfen uygulama MVC kısmı form kimlik doğrulaması (yani, tanımlama bilgileri kullanır) kullanıyorsa, sahteciliğe karşı koruma belirteçleri MVC web uygulaması tarafından kullanılması gerektiğini unutmayın. |

### <a name="example"></a>Örnek
Yalnızca üzerinde taşıyıcı belirteçlerini ve çalıştırılmadı tanımlama bilgilerini yararlanmayı bilgi sahibi olmak Web API vardır. Aşağıdaki yapılandırmada tarafından yapılabilir `WebApiConfig.Register` yöntemi: '''C-Sharp kod yapılandırma. SuppressDefaultHostAuthentication(); Config. Filters.Add (yeni HostAuthenticationFilter(OAuthDefaults.AuthenticationType));
```
The SuppressDefaultHostAuthentication method tells Web API to ignore any authentication that happens before the request reaches the Web API pipeline, either by IIS or by OWIN middleware. That way, we can restrict Web API to authenticate only using bearer tokens.