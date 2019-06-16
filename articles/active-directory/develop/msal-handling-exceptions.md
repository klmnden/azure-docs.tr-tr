---
title: Hatalar ve özel durumlar (MSAL) | Azure
description: Hatalar ve özel durumları işlemek nasıl koşullu erişim ve talepleri MSAL uygulamalarda meydan öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 30ab8a3fec459bef1a85c44e9a7cdb91b541fa2d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67111388"
---
# <a name="handling-exceptions-and-errors-using-msal"></a>Özel durum ve MSAL kullanarak hataları işleme
Microsoft Authentication Library (MSAL) özel durum sorunlarını gidermek uygulama geliştiricileri için ve son kullanıcılara görüntülenemiyor içindir. Özel durum iletileri yerelleştirilmiş değil.

Özel durumlar ve hataları işleme sırasında özel durumlar arasında ayrım yapmak için özel durum türü ve hata kodu kullanabilirsiniz.  Hata kodlarının listesi için bkz. [kimlik doğrulama ve yetkilendirme hata kodları](reference-aadsts-error-codes.md).

## <a name="net-exceptions"></a>.NET özel durumları
Özel durum işlenirken özel durum türü kullanın ve `ErrorCode` özel durumları arasında ayrım yapmak için üye. Değerlerini `ErrorCode` türünde sabitler [MsalError](/dotnet/api/microsoft.identity.client.msalerror?view=azure-dotnet).

Alanlarını göz bulundurabilirsiniz [MsalClientException](/dotnet/api/microsoft.identity.client.msalexception?view=azure-dotnet), [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet), [MsalUIRequiredException](/dotnet/api/microsoft.identity.client.msaluirequiredexception?view=azure-dotnet).

Varsa [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) oluşturulur, hata kodu bulabileceğiniz bir kod içerebilir [kimlik doğrulama ve yetkilendirme hata kodları](reference-aadsts-error-codes.md).

### <a name="common-exceptions"></a>Sık karşılaşılan özel durumlar
Harekete geçirilebilirse ortak özel durumları ve bazı olası risk azaltmaları aşağıda verilmiştir.

| Özel durum | Hata kodu | Risk azaltma|
| --- | --- | --- |
| [MsalUiRequiredException](/dotnet/api/microsoft.identity.client.msaluirequiredexception?view=azure-dotnet) | AADSTS65001: Kullanıcı veya yönetici Kimliğine sahip '{AppID}', '{appName}' adlı uygulamayı kullanmak için izin verilmez. Bu kullanıcı ve kaynak için bir etkileşimli yetkilendirme isteği gönderin.| Kullanıcı onayı'nın ilk almanız gerekir. (Bu, herhangi bir Web UI olmayan) bir .NET Core kullanmıyorsanız (yalnızca bir kez) çağrı `AcquireTokeninteractive`. .NET core kullanarak veya yapmak istemediğiniz bir `AcquireTokenInteractive`, kullanıcı izni vermek için bir URL'ye gidebilir: https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id={clientId}&response_type=code&scope=user.read . Çağrılacak `AcquireTokenInteractive`: `app.AcquireTokenInteractive(scopes).WithAccount(account).WithClaims(ex.Claims).ExecuteAsync();`|
| [MsalUiRequiredException](/dotnet/api/microsoft.identity.client.msaluirequiredexception?view=azure-dotnet) | AADSTS50079: Kullanıcı, çok faktörlü kimlik doğrulaması kullanmak için gereklidir.| MFA kiracınız için yapılandırılmışsa, risk azaltma - olduğundan ve AAD karar bunu zorlamak, geri dönüş için etkileşimli bir akış aşağıdaki gibi ihtiyacınız `AcquireTokenInteractive` veya `AcquireTokenByDeviceCode`.|
| [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) |AADSTS90010: İzin verme türü üzerinden desteklenmeyen */ortak* veya */consumers* uç noktaları. Kullanım */organizations* veya kiracıya özgü uç nokta. Kullanılan */ortak*.| Yetkilisi Azure AD'den ileti içinde açıklandığı şekilde, bir kiracı olması gerekir veya başka türlü */organizations*.|
| [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) | AADSTS70002: İstek gövdesi şu parametre içermelidir: ' client_secret veya client_assertion'.| Uygulamanızı Azure AD'de bir genel istemci uygulaması olarak kayıtlı değil, bu durum oluşabilir. Azure portalında uygulama ve küme için bildirimi düzenleyin `allowPublicClient` için `true`. |
| [MsalClientException](/dotnet/api/microsoft.identity.client.msalclientexception?view=azure-dotnet)| unknown_user ileti: Oturum açmış olan kullanıcının tanımlanamıyor| Kitaplık geçerli Windows oturum açmış kullanıcının sorgulayamadı veya bu kullanıcı AD değil veya AAD birleştirilmiş (çalışma alanına katılmış kullanıcılar desteklenmez). Azaltma 1: UWP üzerinde uygulama aşağıdaki özelliklere sahip olduğunu denetleyin: Kurumsal kimlik, özel ağlar (istemci ve sunucu), kullanıcı hesabı bilgileri. Azaltma 2: Kullanıcı adını almak için kendi mantığını (örneğin, john@contoso.com) ve `AcquireTokenByIntegratedWindowsAuth` kullanıcı adında gereken form.|
| [MsalClientException](/dotnet/api/microsoft.identity.client.msalclientexception?view=azure-dotnet)|integrated_windows_auth_not_supported_managed_user| Bu yöntem Active Directory (AD tarafından) kullanıma sunulan bir protokolü kullanır. Bir kullanıcı Azure Active Directory'de AD ("yönetilen" kullanıcı), yedekleme oluşturulmuş olsa bile, bu yöntem başarısız olur. Kullanıcıların AD'de oluşturulmuş ve AAD ("birleştirilmiş" kullanıcılar) tarafından desteklenen bu etkileşimli olmayan kimlik doğrulama yönteminden yararlı olabilir. Azaltma: Etkileşimli kimlik doğrulaması kullanın.|

## <a name="javascript-errors"></a>JavaScript hataları

Soyut ve sık karşılaşılan farklı türlerini sınıflandırmak ve bunları uygun şekilde işlemek için hata iletileri gibi hataları belirli ayrıntılarını erişmek için bir arabirimi hata nesneleri MSAL.js sağlar.

**Hata nesnesi**

```javascript                                
export class AuthError extends Error {
    // This is a short code describing the error
    errorCode: string;
    // This is a descriptive string of the error,
    // and may also contain the mitigation strategy
    errorMessage: string;
    // Name of the error class
    this.name = "AuthError";
}
```                
Hata sınıfını genişleterek, aşağıdaki özelliklere erişebilirsiniz:
* **AuthError.message:** ErrorMessage aynı budur.
* **AuthError.stack:** Oluşturulan hatalar için yığın izlemesi. Hatanın kaynak noktasına izleme sağlar.

**Hata türleri**

Aşağıdaki hata türleri kullanılabilir:

* *AuthError:* Temel hata sınıfı MSAL.js kitaplığın beklenmeyen hatalar için de kullanılır.

* *ClientAuthError:* İstemci kimlik doğrulaması ile ilgili bir sorun gösterir hata sınıfı. Kitaplıktan gelen en hataları ClientAuthErrors olacaktır. Bu işlem, oturum açma kullanıcı oturum açma, vb. iptal ediliyor, devam ederken bir oturum açma yöntemini çağırarak gibi hataları olabilir.

* *ClientConfigurationError:* İstekler zaman yapılmadan önce oluşturulan ClientAuthError genişletme hatası sınıfı belirli bir kullanıcı yapılandırma parametreleri hatalı veya eksik.

* *ServerError:* Kimlik doğrulama sunucu tarafından gönderilen hata dizelerini temsil etmek için hata sınıfı. Bu, hataları gibi geçersiz istek biçimlerinin veya parametreler veya kimlik doğrulaması veya kullanıcı yetkilendirme sunucusu önleyen diğer hataları olabilir.

* *InteractionRequiredAuthError:* Etkileşimli bir arama gerektiren sunucu hataları temsil etmek için ServerError genişletme hatası sınıfı. Tarafından oluşturulan bu `acquireTokenSilent` kullanıcının kimlik bilgilerini belirtin veya kimlik doğrulama/yetkilendirme için onay sunucusu ile etkileşim kurmak için gerekiyorsa. Hata kodları "interaction_required", "login_required", "consent_required" içerir.

Hata ile kimlik doğrulama akışları işleme yöntemlerini yeniden yönlendirmek için (`loginRedirect`, `acquireTokenRedirect`), yeniden yönlendirme kullandıktan sonra çağrılan geri arama başarı veya başarısızlık ile kaydetmeniz gerekir `handleRedirectCallback()` yöntemini aşağıdaki şekilde:

```javascript
function authCallback(error, response) {
    //handle redirect response
}


var myMSALObj = new Msal.UserAgentApplication(msalConfig);

// Register Callbacks for redirect flow
myMSALObj.handleRedirectCallback(authCallback);

myMSALObj.acquireTokenRedirect(request);
```

Açılır deneyimi için yöntemleri (`loginPopup`, `acquireTokenPopup`) iade gösterir, promise desenini (.then ve .catch) gösterildiği gibi işlemek için kullanabilirsiniz:

```javascript
myMSALObj.acquireTokenPopup(request).then(
    function (response) {
        // success response
    }).catch(function (error) {
        console.log(error);
    });
```

### <a name="interaction-required-errors"></a>Gerekli etkileşim hataları

Bir kullanıcı Arabirimi etkileşimi gerekli olduğunda bir hata döndürülür. Bu belirteç alınırken bir etkileşimli olmayan bir yöntem kullanılacak çalıştı anlamına gelir (örneğin, `acquireTokenSilent`), ancak MSAL bunu, sessizce. Olası nedenler şunlardır:

* oturum açmanız gerekir
* onay gerekiyor
* bir çok faktörlü kimlik doğrulaması deneyimi gitmeniz gerekir.

Etkileşimli bir yöntemi çağırmak için düzeltmedir `acquireTokenPopup` veya `acquireTokenRedirect`:

```javascript
// Request for Access Token
myMSALObj.acquireTokenSilent(request).then(function (response) {
    // call API
}).catch( function (error) {
    // call acquireTokenPopup in case of acquireTokenSilent failure
    // due to consent or interaction required
    if (error.errorCode === "consent_required"
    || error.errorCode === "interaction_required"
    || error.errorCode === "login_required") {
        myMSALObj.acquireTokenPopup(request).then(
            function (response) {
                // call API
            }).catch(function (error) {
                console.log(error);
            });
    }
});
```

## <a name="conditional-access-and-claims-challenges"></a>Koşullu erişim ve talepleri zorlukları
Uygulama belirteçleri sessizce alınırken hatalar alabilirsiniz, bir [koşullu erişim talep sınama](conditional-access-dev-guide.md) MFA İlkesi tarafından API gerektiği gibi erişmeye çalıştığınız.

Bu hatayı işlemek için bir desen, etkileşimli olarak MSAL kullanarak bir belirteç elde etmektir. Etkileşimli bir belirteç alınırken kullanıcıdan ve bunları gerekli koşullu erişim ilkesini karşılayan fırsatı sunar.

Koşullu erişim gerektiren bir API'nin çağrılması durumunda bazı durumlarda, bir talep sınaması hatalı API'den alabilir. Koşullu erişim ilkesi, yönetilen bir cihazda (Intune) hata ise örneği, aşağıdakine benzer olacaktır [AADSTS53000: Cihazınızın bu kaynağa erişebilmek için yönetilmesi için gerekli olduğundan](reference-aadsts-error-codes.md) veya benzer bir şey. Bu durumda, böylece uygun ilkeyi karşılamak için kullanıcıdan talep belirteci alma çağrısında geçirebilirsiniz.

### <a name="net"></a>.NET
MSAL.NET koşullu erişim gerektiren bir API'nin çağrılması durumunda talep karşılıklı özel durumları işlemek uygulamanız gerekir. Bu olarak görünür bir [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) burada [talep](/dotnet/api/microsoft.identity.client.msalserviceexception.claims?view=azure-dotnet) özelliği boş olmayacak.

Talep kimlik işlemek için kullanmanız gerekecektir `.WithClaim()` yöntemi `PublicClientApplicationBuilder` sınıfı.

### <a name="javascript"></a>JavaScript
Belirteçleri sessizce alırken (kullanarak `acquireTokenSilent`) MSAL.js kullanılarak, uygulamanızın hatalar alabilirsiniz, bir [koşullu erişim talep sınama](conditional-access-dev-guide.md) MFA İlkesi tarafından API gerektiği gibi erişmeye çalıştığınız.

Bu hatayı işlemek için bir desen gibi MSAL.js belirteç almak için etkileşimli bir arama yapmak için olan `acquireTokenPopup` veya `acquireTokenRedirect` aşağıdaki örnekteki gibi:

```javascript
myMSALObj.acquireTokenSilent(accessTokenRequest).then(function (accessTokenResponse) {
    // call API
}).catch( function (error) {
    // call acquireTokenPopup in case of acquireTokenSilent failure
    myMSALObj.acquireTokenPopup(accessTokenRequest).then(
        function (accessTokenResponse) {
            // call API
        }).catch(function (error) {
            console.log(error);
        });
});
```

Etkileşimli olarak belirtecini alma kullanıcıdan ve bunları gerekli koşullu erişim ilkesini karşılayan fırsatı sunar.

Koşullu erişim gerektiren bir API'nin çağrılması durumunda hata API'sinden bir talep challenge alabilir. Bu durumda, hata olarak iade talepleri geçirebilirsiniz `extraQueryParameters` çağrısında uygun ilkeyi karşılamak için kullanıcıdan böylece belirteçlerini almak için:

```javascript
var request = {
    scopes: ["user.read"],
    extraQueryParameters: {claims: claims}
}

myMSALObj.acquireTokenPopup(request);
```

## <a name="retrying-after-errors-and-exceptions"></a>Hatalar ve özel durumlar sonra yeniden deneniyor

### <a name="http-error-codes-500-600"></a>500-600 HTTP hata kodları
MSAL.NET basit bir yeniden deneme uygulayan-500-600 HTTP hatası hatalarla mekanizması kodları sonra.

### <a name="http-429"></a>HTTP 429
Hizmet belirteci sunucu (STS) "çok fazla istek nedeniyle" çok meşgulse, 429 Yapabildiğinizde hakkında bir ipucuyla birlikte bir HTTP hatası döndürür. yeniden deneyin (Retry-After yanıt alan) bir gecikme olarak saniye ya da bir tarih.

#### <a name="net"></a>.NET
[MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) özel durum yüzeyleri `System.Net.Http.Headers.HttpResponseHeaders` bir özellik olarak `namedHeaders`. Bu nedenle, hata kodu güvenilirliği artırmak için ek bilgi de yararlanabilirsiniz. Açıkladığımız durumda kullanabilirsiniz `RetryAfterproperty` (tür `RetryConditionHeaderValue`) ve işlem ne zaman yeniden deneyin.

İşte bir örnek için bir arka plan programı uygulama (istemci kimlik bilgileri akışını kullanarak), ancak herhangi bir yöntemi için bir belirteç alınırken uyarlayabilirsiniz.

```csharp
do
{
 retry = false;
 TimeSpan? delay;
 try
 {
  result = await publicClientApplication.AcquireTokenForClient(scopes, account)
                                        .ExecuteAsync();
 }
 catch (MsalServiceException serviceException)
 {
  if (ex.ErrorCode == "temporarily_unavailable")
  {
   RetryConditionHeaderValue retryAfter = serviceException.Headers.RetryAfter;
   if (retryAfter.Delta.HasValue)
   {
    delay = retryAfter.Delta;
   }
   else if (retryAfter.Date.HasValue)
   {
    delay = retryAfter.Date.Value.Offset;
   }
  }
 }

    …
 if (delay.HasValue)
 {
  Thread.Sleep((int)delay.Value.TotalMilliseconds); // sleep or other
  retry = true;
 }
} while (retry);
```
