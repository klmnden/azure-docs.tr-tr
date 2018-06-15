---
title: Azure Active Directory Authentication Library (ADAL) istemciler için en iyi yöntemler işleme hatası
description: Hata işleme yönerge ve ADAL istemci uygulamaları için en iyi yöntemler sağlar.
services: active-directory
documentationcenter: ''
author: danieldobalian
manager: mtillman
ms.author: celested
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2017
ms.custom: ''
ms.openlocfilehash: 27315262ff64b640acc3af16a26fc3887d852a00
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34157652"
---
# <a name="error-handling-best-practices-for-azure-active-directory-authentication-library-adal-clients"></a>Azure Active Directory Authentication Library (ADAL) istemciler için en iyi yöntemler işleme hatası

Bu makale, geliştiriciler, ADAL kullanıcıların kimliklerini doğrulamak için kullanırken karşılaşabileceğiniz olduğunu, hataları türüne yönergeler sağlar. ADAL kullanırken, geliştirici adımını ve hataları işlemek için burada gerekebilir bazı durumlar vardır. Uygun hata işleme harika son kullanıcı deneyimi sağlar ve son kullanıcının oturum açması gerekiyor sayısını sınırlar.

Bu makalede, ADAL ve nasıl uygulamanızın her örneği düzgün bir şekilde işleyebilir tarafından desteklenen her platform için özel durumları inceleyin. Hata Kılavuzu ADAL API'leri tarafından sağlanan belirteç edinme desenleri dayalı iki geniş kategoriler halinde ayrılır:

- **AcquireTokenSilent**: istemci çalışır bir belirteç sessizce almak (kullanıcı Arabirimi) ve ADAL başarısız olması durumunda başarısız olabilir. 
- **AcquireToken**: istemci sessiz edinme deneyebilirsiniz, ancak oturum açma gerektiren etkileşimli istekleri de gerçekleştirebilirsiniz.

> [!TIP]
> ADAL ve Azure AD kullanırken, tüm hataları ve özel durumları günlüğe kaydetmek için iyi bir fikirdir. Günlükleri yalnızca uygulamanız genel durumunu anlamak için yararlı değildir, ancak daha geniş sorunlarda hata ayıklaması da önemlidir. Uygulamanızı belirli hataları kurtarmak, ancak bunlar çözümlemek amacıyla kod değişiklikleri gerektiren geniş tasarım sorunları ipucu. 
> 
> Bu belgede ele alınan hata koşulları uygulama başladığında, daha önce ele alınan nedenlerle hata kodu ve açıklama oturum. Bkz: [hata ve günlük başvuru](#error-and-logging-reference) günlük kod örnekleri için. 
>

## <a name="acquiretokensilent"></a>AcquireTokenSilent

Son kullanıcı bir kullanıcı arabirimi (UI) görmez Garantisi ile bir belirteç almak üzere AcquireTokenSilent çalışır. Bazı durumları sessiz edinme nerede başarısız olabilir ve etkileşimli istekleri aracılığıyla veya varsayılan işleyici tarafından işlenen gereksinimleri vardır. Aşağıdaki bölümlerde her durumda kullanımlar nasıl ve ne zaman ayrıntılarını içine öğrenebilirsiniz.

Uygulamaya özgü işleme hatası gerektirebilir işletim sistemi tarafından oluşturulan hata kümesi yok. "İşletim sistemi" hataları bölümünde daha fazla bilgi için bkz: [hata ve günlük başvuru](#error-and-logging-reference). 

### <a name="application-scenarios"></a>Uygulama senaryoları

- [Yerel istemci](active-directory-dev-glossary.md#native-client) uygulamalar (iOS, Android, .NET Masaüstü veya Xamarin)
- [Web istemcisi](active-directory-dev-glossary.md#web-client) çağıran uygulamalara bir [kaynak](active-directory-dev-glossary.md#resource-server) (.NET)

### <a name="error-cases-and-actionable-steps"></a>Hata durumları ve tıklatılabilir adımlar

Temelde, AcquireTokenSilent hataları iki durum vardır:

| Durum | Açıklama |
|------|-------------|
| **Durum 1**: hata bir etkileşimli oturum açma ile çözülebilir. | Geçerli belirteçlerini eksikliği nedeniyle sebep olunan hataları için etkileşimli bir isteği gereklidir. Özellikle, önbellek araması ve geçersiz/süresi dolmuş yenileme belirtecini çözümlemek için bir AcquireToken çağrı gerektirir.<br><br>Bu durumda, son kullanıcı, oturum açmak için sizden gerekiyor. Uygulama, etkileşimli bir isteği hemen sonra son kullanıcı etkileşiminin (örneğin, bir oturum açma düğmesine basarsa) veya daha sonra yapmak seçebilirsiniz. Seçim uygulama istenen davranışı üzerinde bağlıdır.<br><br>Bu belirli durumda ve bu tanılamak hataları için aşağıdaki bölümdeki koduna bakın.|
| **Durum 2**: hata bir etkileşimli oturum açma ile çözülebilir değildir | Ağ ve geçici/geçici hataları veya diğer hataları için etkileşimli bir AcquireToken isteği gerçekleştirme sorunu çözmezse. Gereksiz etkileşimli oturum açma komut istemlerini Ayrıca son kullanıcıları rahatsız edebilir. ADAL AcquireTokenSilent hatalarda hataların çoğu için tek bir yeniden deneme otomatik olarak çalışır.<br><br>İstemci uygulamasının daha sonraki bir noktada bir yeniden deneme de deneyebilirsiniz, ancak ne zaman ve nasıl yapılacağını istenen son kullanıcı deneyimi ve uygulama davranışı üzerinde bağımlı. Örneğin, uygulama bir AcquireTokenSilent yeniden deneme birkaç dakika sonra veya bazı son kullanıcı eylemine yanıt olarak yapabilirsiniz. Hemen bir yeniden deneme karşılaşıldığı uygulamada neden olur ve değil denenmesi gerekir.<br><br>Aynı hatası ile başarısız olan bir sonraki yeniden deneme hata çözümlenmiyor gibi istemci AcquireToken, kullanarak etkileşimli bir isteği yapmalısınız anlamına gelmez.<br><br>Bu belirli durumda ve bu tanılamak hataları için aşağıdaki bölümdeki koduna bakın. |

### <a name="net"></a>.NET

Aşağıdaki kılavuzlar, hata ADAL yöntemleriyle birlikte işleme için örnekler verilmektedir: 

- acquireTokenSilentAsync(...)
- acquireTokenSilentSync(…) 
- [kullanım dışı] acquireTokenSilent(...)
- [kullanım dışı] acquireTokenByRefreshToken(...) 

Kodunuzun şu şekilde uygulanması:

```csharp
try{
    AcquireTokenSilentAsync(…);
} 

catch (AdalSilentTokenAcquisitionException e) {
    // Exception: AdalSilentTokenAcquisitionException
    // Caused when there are no tokens in the cache or a required refresh failed. 

    // Action: Case 1, resolvable with an interactive request. 
} 

catch(AdalServiceException e) {
    // Exception: AdalServiceException 
    // Represents an error produced by the STS. 
    // e.ErrorCode contains the error code and description, which can be used for debugging. 
    // NOTE: Do not code a dependency on the contents of the error description, as it can change over time.

    // Action: Case 2, not resolvable with an interactive request.
    // Attempt retry after a timed interval or user action.
} 
    
catch (AdalException e) {
    // Exception: AdalException 
    // Represents a library exception generated by ADAL .NET. 
    // e.ErrorCode contains the error code. 

    // Action: Case 2, not resolvable with an interactive request.
    // Attempt retry after a timed interval or user action.
    // Example Error: network_not_available, default case.
}
```

### <a name="android"></a>Android

Aşağıdaki kılavuzlar, hata ADAL yöntemleriyle birlikte işleme için örnekler verilmektedir: 

- acquireTokenSilentSync(…)
- acquireTokenSilentAsync(...)
- [kullanım dışı] acquireTokenSilent(...)

Kodunuzun şu şekilde uygulanması:

```java
// *Inside callback*
public void onError(Exception e) {

    if (e instanceof AuthenticationException) {
        // Exception: AdalException
        // Represents a library exception generated by ADAL Android.
        // Error Code: e.getCode().

        // Errors: ADALError.ERROR_SILENT_REQUEST,
        // ADALError.AUTH_REFRESH_FAILED_PROMPT_NOT_ALLOWED,
        // ADALError.INVALID_TOKEN_CACHE_ITEM
        // Description: Request failed due to no tokens in
        // cache or failed a required refresh. 

        // Action: Case 1, resolvable with an interactive request. 

        // Action: Case 2, not resolvable with an interactive request.
        // Attempt retry after a timed interval or user action.
        // Example Errors: default case,
        // DEVICE_CONNECTION_IS_NOT_AVAILABLE, 
        // BROKER_AUTHENTICATOR_ERROR_GETAUTHTOKEN,
    }
}
```

### <a name="ios"></a>iOS

Aşağıdaki kılavuzlar, hata ADAL yöntemleriyle birlikte işleme için örnekler verilmektedir: 

- acquireTokenSilentWithResource(...)

Kodunuzun şu şekilde uygulanması:

```objc
[context acquireTokenSilentWithResource:[ARGS], completionBlock:^(ADAuthenticationResult *result) {
    if (result.status == AD_FAILED) {
        if ([error.domain isEqualToString:ADAuthenticationErrorDomain]){
            // Exception: AD_FAILED 
            // Represents a library error generated by ADAL Objective-C.
            // Error Code: result.error.code

            // Errors: AD_ERROR_SERVER_REFRESH_TOKEN_REJECTED, AD_ERROR_CACHE_NO_REFRESH_TOKEN
            // Description: No tokens in cache or failed a required token refresh failed. 
            // Action: Case 1, resolvable with an interactive request. 

            // Error: AD_ERROR_CACHE_MULTIPLE_USERS
            // Description: There was ambiguity in the silent request resulting in multiple cache items.
            // Action: Special Case, application should perform another silent request and specify the user using ADUserIdentifier. 
            // Can be caused in cases of a multi-user application. 

            // Action: Case 2, not resolvable with an interactive request.
            // Attempt retry after some time or user action.
            // Example Errors: default case,
            // AD_ERROR_CACHE_BAD_FORMAT
        }
    }
}]
```

## <a name="acquiretoken"></a>AcquireToken

AcquireToken belirteçleri almak için kullanılan varsayılan ADAL yöntemidir. Kullanıcı kimliği gerekli olduğu durumlarda AcquireToken bir belirteç sessizce ilk almaya çalışır, sonra (PromptBehavior.Never geçirilen sürece) gerekiyorsa, kullanıcı Arabirimi görüntüler. Uygulama kimliği gerekli olduğu durumlarda AcquireToken bir belirteç almak üzere çalışır, ancak hiçbir son kullanıcı olarak UI göstermez. 

Senaryo uygulama elde etmek çalışıyor ve AcquireToken hatalarını işlerken hata işleme platformuna bağlıdır. 

İşletim sistemi hata belirli uygulamanın bağımlı işleme gerektiren hataları kümesi de oluşturabilirsiniz. Daha fazla bilgi için bkz: "İşletim sistemi hataları" [hata ve günlük başvuru](#error-and-logging-reference). 

### <a name="application-scenarios"></a>Uygulama senaryoları

- Yerel istemci uygulamalar (iOS, Android, .NET Masaüstü veya Xamarin)
- Bir kaynak (.NET) API çağrısı web uygulamaları
- Tek sayfa uygulamaları (JavaScript)
- Hizmet hizmet uygulamaları (.NET, Java)
  - Üzerinde-adına-of dahil olmak üzere tüm senaryoları
  - Üzerinde-adına-belirli senaryolar

### <a name="error-cases-and-actionable-steps-native-client-applications"></a>Hata durumları ve tıklatılabilir adımlar: yerel istemci uygulamaları

Yerel istemci uygulaması oluşturuyorsanız, hangi ağ sorunları, geçici hataları ve diğer platforma özgü hataları ile ilgili dikkate alınması gereken birkaç hata işleme durumlar vardır. Çoğu durumda, bir uygulama döndürmemelidir hemen yeniden deneme gerçekleştirir, ancak bunun yerine bir oturum açma ister son kullanıcı etkileşiminin için bekleyin. 

Tek bir yeniden deneme sorunu çözebilir bazı özel durumlar vardır. Örneğin, ne zaman bir kullanıcı bir cihazda etkinleştirmek gereken veya Azure AD Aracısı tamamlandı ilk hatasından sonra indirin. 

Başarısız durumda bir yeniden deneme ister bazı etkileşim gerçekleştirmek son kullanıcı izin vermek için kullanıcı Arabirimi bir uygulama oluşturabilir. Örneği için cihaz için çevrimdışı bir hata başarısız olursa bir AcquireToken isteyen bir "yeniden oturum açmayı deneyin" düğmesini yeniden deneyin yerine hemen hata yeniden deneniyor. 

Hata işleme yerel uygulamalar tarafından iki durumda tanımlanabilir:

|  |  |
|------|-------------|
| **Durum 1**:<br>Olmayan yeniden denenebilir hata (çoğu zaman) | 1. Hemen yeniden deneme çalışmayın. Kullanıcı Arabirimi ("oturum açma ile yeniden deneyin", "Azure AD Yükleme Aracısı uygulama", vb.) bir yeniden deneme çağıran belirli hataya göre son kullanıcı sunar. |
| **Durum 2**:<br>Yeniden denenebilir hata | 1. Son kullanıcı başarılı bir şekilde sonuçları bir duruma girdiyse gibi tek bir yeniden deneme gerçekleştirir.<br><br>2. Yeniden deneme başarısız olursa, kullanıcı Arabirimi ("oturum açma ile yeniden deneyin", "İndirme Azure AD aracı uygulaması", vb.) bir yeniden deneme çağıran belirli hataya göre son kullanıcı sunar. |

> [!IMPORTANT]
> Bir kullanıcı hesabı için ADAL sessiz bir çağrı ve başarısız geçirilirse etkileşimli Taleplerde son kullanıcının farklı bir hesap kullanarak oturum olanak sağlar. Bir kullanıcı hesabı kullanarak bir başarılı AcquireToken sonra uygulamanın yerel kullanıcı nesnesinin oturum açmış kullanıcı eşleştiğinden uygulama doğrulamanız gerekir. Uyuşmazlık özel durum oluşturmaz (Objective C'de hariç), ancak burada kullanıcı bilinen yerel olarak (örneğin, başarısız bir sessiz çağrı) kimlik doğrulama isteklerini önce durumlarda düşünülmelidir.
>

#### <a name="net"></a>.NET

Aşağıdaki kılavuzlar, hata tüm sessiz olmayan AcquireToken(...) birlikte işleme için örnekler verilmektedir. ADAL yöntemleri *dışında*: 

- AcquireTokenAsync (..., IClientAssertionCertification,...)
- AcquireTokenAsync (..., ClientCredential,...)
- AcquireTokenAsync (..., ClientAssertion,...)
- AcquireTokenAsync(...,UserAssertion,...)   

Kodunuzun şu şekilde uygulanması:

```csharp
try {
    AcquireTokenAsync(…);
} 
    
catch(AdalServiceException e) {
    // Exception: AdalServiceException 
    // Represents an error produced by the STS. 
    // e.ErrorCode contains the error code and description, which can be used for debugging. 
    // NOTE: Do not code a dependency on the contents of the error description, as it can change over time.
    
    // Design time consideration: Certain errors may be caused at development and exposed through this exception. 
    // Looking inside the description will give more guidance on resolving the specific issue. 

    // Action: Case 1: Non-Retryable 
    // Do not perform an immediate retry. Only retry after user action. 
    // Example Errors: default case

    } 

catch (AdalException e) {
    // Exception: AdalException 
    // Represents a library exception generated by ADAL .NET.
    // e.ErrorCode contains the error code

    // Action: Case 1, Non-Retryable 
    // Do not perform an immediate retry. Only retry after user action. 
    // Example Errors: network_not_available, default case
}
```

> [!NOTE]
> ADAL .NET davranışı AcquireTokenSilent gibi PromptBehavior.Never destekler gibi ek bir konu vardır.
>

Aşağıdaki kılavuzlar, hata ADAL yöntemleriyle birlikte işleme için örnekler verilmektedir: 

- acquireToken(..., PromptBehavior.Never)

Kodunuzun şu şekilde uygulanması:

```csharp
    try {acquireToken(…, PromptBehavior.Never);
    } 

catch(AdalServiceException e) {
    // Exception: AdalServiceException represents 
    // Represents an error produced by the STS. 
    // e.ErrorCode contains the error code and description, which can be used for debugging. 
    // NOTE: Do not code a dependency on the contents of the error description, as it can change over time.

    // Action: Case 1: Non-Retryable 
    // Do not perform an immediate retry. Only retry after user action. 
    // Example Errors: default case

} catch (AdalException e) {
    // Error Code: e.ErrorCode == "user_interaction_required"
    // Description: user_interaction_required indicates the silent request failed 
    // in a way that's resolvable with an interactive request.
    // Action: Resolvable with an interactive request. 

    // Action: Case 1, Non-Retryable 
    // Do not perform an immediate retry. Only retry after user action. 
    // Example Errors: network_not_available, default case
}
```

#### <a name="android"></a>Android

Aşağıdaki kılavuzlar, hata tüm sessiz olmayan AcquireToken(...) birlikte işleme için örnekler verilmektedir. ADAL yöntemleri. 

Kodunuzun şu şekilde uygulanması:

```java
AcquireTokenAsync(…);

// *Inside callback*
public void onError(Exception e) {
    if (e instanceof AuthenticationException) {
        // Exception: AdalException 
        // Represents a library exception generated by ADAL Android.
        // Error Code: e.getCode();

        // Error: ADALError.BROKER_APP_INSTALLATION_STARTED
        // Description: Broker app not installed, user will be prompted to download the app. 

        // Action: Case 2, Retriable Error 
        // Perform a single retry. If that fails, only try again after user action. 

        // Action: Case 1, Non-Retriable 
        // Do not perform an immediate retry. Only retry after user action. 
        // Example Errors: default case, DEVICE_CONNECTION_IS_NOT_AVAILABLE
    }
}
```

#### <a name="ios"></a>iOS

Aşağıdaki kılavuzlar, hata tüm sessiz olmayan AcquireToken(...) birlikte işleme için örnekler verilmektedir. ADAL yöntemleri. 

Kodunuzun şu şekilde uygulanması:

```objc
[context acquireTokenWithResource:[ARGS], completionBlock:^(ADAuthenticationResult *result) {
    if (result.status == AD_FAILED) {
        if ([error.domain isEqualToString:ADAuthenticationErrorDomain]){
            // Exception: AD_FAILED 
            // Represents a library error generated by ADAL ObjC.
            // Error Code: result.error.code 

            // Error: AD_ERROR_SERVER_WRONG_USER
            // Description: App passed a user into ADAL and the end user signed in with a different account. 
            // Action: Case 1, Non-retriable (as is) and up to the application on how to handle this case. 
            // It can attempt a new request without specifying the user, or use UI to clarify the user account to sign in. 

            // Action: Case 1, Non-Retriable 
            // Do not perform an immediate retry. Only retry after user action. 
            // Example Errors: default case
        }
    }
}]
```

### <a name="error-cases-and-actionable-steps-web-applications-that-call-a-resource-api-net"></a>Hata durumları ve tıklatılabilir adımlar: Web API (.NET) kaynak çağıran uygulamalar

Oluşturmakta olduğunuz çağıran .NET web uygulaması için bir kaynağı bir yetkilendirme kodu kullanarak bir belirteç alır, gerekli yalnızca genel çalışması için bir varsayılan işleyici kodudur. 

Aşağıdaki kılavuzlar, hata ADAL yöntemleriyle birlikte işleme için örnekler verilmektedir: 

- AcquireTokenByAuthorizationCodeAsync(...)

Kodunuzun şu şekilde uygulanması:

```csharp
try {
    AcquireTokenByAuthorizationCodeAsync(…);
} 

catch (AdalException e) {
    // Exception: AdalException
    // Represents a library exception generated by ADAL .NET.
    // Error Code: e.ErrorCode

    // Action: Do not perform an immediate retry. Only try again after user action or wait until much later. 
    // Example Errors: default case
}
```

### <a name="error-cases-and-actionable-steps-single-page-applications-adaljs"></a>Hata durumları ve tıklatılabilir adımlar: tek sayfa uygulamaları (adal.js)

Adal.js AcquireToken ile kullanarak bir tek sayfa uygulaması oluşturuyorsanız, hata kodu işleme tipik sessiz çağrısının benzer. Özellikle adal.js içinde AcquireToken hiçbir zaman bir kullanıcı Arabirimi gösterir. 

Aşağıdaki durumlarda başarısız AcquireToken sahiptir:

|  |  |
|------|-------------|
| **Durum 1**:<br>Etkileşimli bir istekle çözümlenebilir | 1. Tanımlar: Login() başarısız olursa, hemen yeniden deneme gerçekleştirmeyin. Yalnızca bir kullanıcı eylemi tekrar denemek ister sonra yeniden deneyin.|
| **Durum 2**:<br>Değil Resolvable etkileşimli isteğiyle. Hata yeniden denenebilir. | 1. Son Kullanıcı ana girmiş olarak başarılı bir şekilde sonuçları bir durumda tek bir yeniden deneme gerçekleştirir.<br><br>2. Yeniden deneme başarısız olursa, son kullanıcı bir yeniden deneme çağırabileceği belirli hataya göre eylemi sunar ("oturum açmayı yeniden deneyin"). |
| **Case 3**:<br>Değil Resolvable etkileşimli isteğiyle. Hata yeniden denenebilir değil. | 1. Hemen yeniden deneme çalışmayın. Son kullanıcı bir yeniden deneme çağırabileceği belirli hataya göre eylemi sunar ("oturum açmayı yeniden deneyin"). |

Kodunuzun şu şekilde uygulanması:

```javascript
AuthContext.acquireToken(…, function(error, errorDesc, token) {
    if (error || errorDesc) {
        // Represents any token acquisition failure that occurred. 
        // Error Code: error.indexOf("<ERROR_STRING>")

        // Errors: if (error.indexOf("interaction_required"))
        //         if (error.indexOf("login required"))
        // Description: ADAL wasn't able to silently acquire a token because of expire or fresh session. 
        // Action: Case 1, Resolvable with an interactive login() request. 

        // Error: if (error.indexOf("Token Renewal Failed")) 
        // Description: Timeout when refreshing the token.
        // Action: Case 2, Not resolvable interactively, error is retriable.
        // Perform a single retry. Only try again after user action.

        // Action: Case 3, Not resolvable interactively, error is not retriable. 
        // Do not perform an immediate retry. Only retry after user action.
        // Example Errors: default case
    }
}
```

### <a name="error-cases-and-actionable-steps-service-to-service-applications-net-only"></a>Hata durumları ve tıklatılabilir adımlar: hizmet hizmet uygulamaları (yalnızca .NET)

AcquireToken kullanan bir hizmet uygulaması oluşturuyorsanız, kodunuzu işlemelidir birkaç anahtar hatalarını yok. Hata geri çağırma uygulaması (için-adına-of durumlar) dönün veya bir yeniden deneme stratejisini uygulamak için yalnızca başvuru mercii başarısızlık var. 

#### <a name="all-scenarios"></a>Tüm senaryoları

İçin *tüm* üzerinde-adına-of dahil olmak üzere hizmet uygulama senaryoları:

- Hemen bir yeniden deneme çalışmayın. Tek bir yeniden deneme belirli ADAL sayısı isteği başarısız oldu. 
- Yalnızca bir kullanıcı veya uygulama eylemi istemleri sonra yeniden deneniyor, yeniden deneme devam edin. Örneğin, bazı kümesi aralıkta çalışır bir arka plan programı uygulama yeniden denemek için İleri aralığı kadar beklemeniz gerekir.

Aşağıdaki kılavuzlar, hata ADAL yöntemleriyle birlikte işleme için örnekler verilmektedir: 

- AcquireTokenAsync (..., IClientAssertionCertification,...)
- AcquireTokenAsync (..., ClientCredential,...)
- AcquireTokenAsync (..., ClientAssertion,...)
- AcquireTokenAsync (..., UserAssertion,...)

Kodunuzun şu şekilde uygulanması:

```csharp
try {
    AcquireTokenAsync(…);
} 

catch (AdalException e) {
    // Exception: AdalException
    // Represents a library exception generated by ADAL .NET.
    // Error Code: e.ErrorCode

    // Action: Do not perform an immediate retry. Only try again after user action (if applicable) or wait until much later. 
    // Example Errors: default case
}  
```

#### <a name="on-behalf-of-scenarios"></a>Üzerinde-adına-senaryolar

İçin *üzerinde-adına-of* hizmet uygulama senaryoları.

Aşağıdaki kılavuzlar, hata ADAL yöntemleriyle birlikte işleme için örnekler verilmektedir: 

- AcquireTokenAsync (..., UserAssertion,...)

Kodunuzun şu şekilde uygulanması:

```csharp
try {
AcquireTokenAsync(…);
} 

catch (AdalServiceException e) {
    // Exception: AdalServiceException 
    // Represents an error produced by the STS. 
    // e.ErrorCode contains the error code and description, which can be used for debugging. 
    // NOTE: Do not code a dependency on the contents of the error description, as it can change over time.

    // Error: On-Behalf-Of Error Handler
    if (e.ErrorCode == "interaction_required") {
        // Description: The client needs to perform some action due to a config from admin. 
        // Action: Capture `claims` parameter inside ex.InnerException.InnerException.
        // Generate HTTP error 403 with claims, throw back HTTP error to client.
        // Wait for client to retry. 
    }
} 
        
catch (AdalException e) {
    // Exception: AdalException 
    // Represents a library exception generated by ADAL .NET.
    // Error Code: e.ErrorCode

    // Action: Do not perform an immediate retry. Only try again after user action (if applicable) or wait until much later. 
    // Example Error: default case
}
```

Biz oluşturuncaya bir [tam örnek](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca) bu senaryo gösterir.

## <a name="error-and-logging-reference"></a>Hata ve günlük başvurusu

### <a name="logging-personal-identifiable-information-pii--organizational-identifiable-information-oii"></a>Günlüğe kaydetme kişisel olarak tanımlanabilir bilgileri (PII) & kuruluş olarak tanımlanabilir bilgileri (OII)
Varsayılan olarak, ADAL günlüğü yakalama ya da tüm PII veya OII oturum. Kitaplık bu Günlükçü sınıfında ayarlayıcı aracılığıyla açmak uygulama geliştiricilerin sağlar. PII veya OII açarak, uygulama güvenle çok hassas veri işleme ve tüm yasal düzenleme gereksinimlerine uymak için sorumluluk alır.

### <a name="net"></a>.NET

#### <a name="adal-library-errors"></a>ADAL kitaplığını hataları

Belirli ADAL hataları, kaynak kodunda keşfetmek için [azure-Active Directory-library-için-dotnet depo](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/blob/8f6d560fbede2247ec0e217a21f6929d4375dcaa/src/ADAL.PCL/Utilities/Constants.cs#L58) en iyi hata başvurudur.

#### <a name="guidance-for-error-logging-code"></a>Hata günlüğü kodu için yönergeler

Üzerinde çalıştığınız platforma bağlı olarak ADAL .NET günlük değişiklikleri. Başvurmak [günlüğü wiki](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Logging-in-ADAL.Net) için günlük kaydını etkinleştirme kodu.

### <a name="android"></a>Android

#### <a name="adal-library-errors"></a>ADAL kitaplığını hataları

Belirli ADAL hataları, kaynak kodunda keşfetmek için [azure-Active Directory-library-for-android depo](https://github.com/AzureAD/azure-activedirectory-library-for-android/blob/dev/adal/src/main/java/com/microsoft/aad/adal/ADALError.java#L33) en iyi hata başvurudur.

#### <a name="operating-system-errors"></a>İşletim sistemi hataları

Android işletim sistemi hataları ADAL AuthenticationException aracılığıyla sunulan, "SERVER_INVALID_REQUEST" tanımlanabilir ve daha fazla hata açıklamalarını ayrıntılı olabilir. 

Yaygın hatalar ve hangi uygulama ya da son kullanıcılar bunları karşılaştığınızda yapılacak adımların tam bir listesi için bkz [ADAL Android Wiki](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki). 

#### <a name="guidance-for-error-logging-code"></a>Hata günlüğü kodu için yönergeler

```java
// 1. Configure Logger
Logger.getInstance().setExternalLogger(new ILogger() {    
    @Override   
    public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) { 
    // …
    // You can write this to logfile depending on level or errorcode. 
    writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);    
    }
}

// 2. Set the log level
Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);

// By default, the `Logger` does not capture any PII or OII

// PII or OII will be logged
Logger.getInstance().setEnablePII(true);

// To STOP logging PII or OII, use the following setter
Logger.getInstance().setEnablePII(false);


// 3. Send logs to logcat.
adb logcat > "C:\logmsg\logfile.txt";
```

### <a name="ios"></a>iOS

#### <a name="adal-library-errors"></a>ADAL kitaplığını hataları

Belirli ADAL hataları, kaynak kodunda keşfetmek için [azure-Active Directory-library-için-objc depo](https://github.com/AzureAD/azure-activedirectory-library-for-objc/blob/dev/ADAL/src/ADAuthenticationError.m#L295) en iyi hata başvurudur.

#### <a name="operating-system-errors"></a>İşletim sistemi hataları

Kullanıcılar web görünümleri ve kimlik doğrulama yapısını kullandığınızda oturum açma sırasında iOS hataları oluşabilir. Bu, SSL hataları, zaman aşımı veya ağ hataları gibi koşullar nedeni olabilir:

- Yetkilendirme paylaşım, oturumları kalıcı değildir ve önbellek boş görünür. Aşağıdaki kod satırını Anahtarlığa ekleyerek çözülebilir: `[[ADAuthenticationSettings sharedInstance] setSharedCacheKeychainGroup:nil];`
- NsUrlDomain kümesi için eylem uygulama mantığını bağlı olarak değişir. Bkz: [NSURLErrorDomain başvuru belgeleri](https://developer.apple.com/documentation/foundation/nsurlerrordomain#declarations) işlenebilir belirli örnekleri için.
- Bkz: [ADAL Obj-C sık karşılaşılan sorunları](https://github.com/AzureAD/azure-activedirectory-library-for-objc#adauthenticationerror) ADAL Objective-C ekibi tarafından korunan sık karşılaşılan hataların listesi.

#### <a name="guidance-for-error-logging-code"></a>Hata günlüğü kodu için yönergeler

```objc
// 1. Enable NSLogging
[ADLogger setNSLogging:YES];

// 2. Set the log level (if you want verbose)
[ADLogger setLevel:ADAL_LOG_LEVEL_VERBOSE];

// 3. Set up a callback block to simply print out
[ADLogger setLogCallBack:^(ADAL_LOG_LEVEL logLevel, NSString *message, NSString *additionalInformation, NSInteger errorCode, NSDictionary *userInfo) {
     NSString* log = [NSString stringWithFormat:@"%@ %@", message, additionalInformation];    
     NSLog(@"%@", log);
}];
```

### <a name="guidance-for-error-logging-code---javascript"></a>Hata günlüğü kodu - JavaScript için yönergeler 

```javascript
0: Error1: Warning2: Info3: Verbose
window.Logging = {
    level: 3,
    log: function (message) {
        console.log(message);
    }
};
```
## <a name="related-content"></a>İlgili içerik

* [Azure AD Geliştirici Kılavuzu][AAD-Dev-Guide]
* [Azure AD kimlik doğrulama kitaplıkları][AAD-Auth-Libraries]
* [Azure AD kimlik doğrulama senaryoları][AAD-Auth-Scenarios]
* [Uygulamaları Azure Active Directory ile tümleştirme][AAD-Integrating-Apps]

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.

[![Düğmesini oturum][AAD-Sign-In]][AAD-Sign-In]
<!--Reference style links -->
[AAD-Auth-Libraries]: ./active-directory-authentication-libraries.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AZURE-portal]: https://portal.azure.com

<!--Image references-->
[AAD-Sign-In]:./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png

