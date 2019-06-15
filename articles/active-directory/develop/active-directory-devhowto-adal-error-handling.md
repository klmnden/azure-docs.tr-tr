---
title: Azure Active Directory kimlik doğrulama kitaplığı (ADAL) istemciler için en iyi işleme hatası
description: Hata işleme yönergeleri ve ADAL istemci uygulamaları için en iyi yöntemler sağlar.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
ms.author: ryanwi
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2017
ms.custom: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 652175e99c800b8e4aa69c639f0bdb9aba838987
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65544635"
---
# <a name="error-handling-best-practices-for-azure-active-directory-authentication-library-adal-clients"></a>Azure Active Directory kimlik doğrulama kitaplığı (ADAL) istemciler için en iyi işleme hatası

Bu makalede, geliştiricilerin, kullanıcıların kimlik doğrulaması için ADAL kullanarak karşılaşabilirsiniz, tür hatalarda yönergeleri sağlanır. ADAL kullanarak, bir geliştirici içeri adım ve hataları işlemek için burada gerekebilir gerçekleşebileceği birkaç durum vardır. Uygun hata işleme harika son kullanıcı deneyimi sağlar ve son kullanıcının oturum açması gerekiyor sayısını sınırlar.

Bu makalede, özel durumlarda ADAL ve nasıl uygulamanız her durumda düzgün işleyebilmesi tarafından desteklenen her platform için inceleyeceğiz. Hata Kılavuzu ADAL API'leri tarafından sağlanan belirteç edinme desenleri göre iki geniş kategoriye ayrılır:

- **AcquireTokenSilent**: İstemci çalışır sessiz bir belirteç almak (kullanıcı Arabirimi) ve ADAL başarısız olması durumunda başarısız olabilir. 
- **AcquireToken**: İstemci sessiz edinme çalışabilir, ancak oturum açma gerektiren etkileşimli istekleri de gerçekleştirebilirsiniz.

> [!TIP]
> ADAL ve Azure AD kullanarak verdiğinde tüm hataları ve özel durumları günlüğe kaydetmek için iyi bir fikirdir. Günlükleri, yalnızca uygulamanız genel durumunu anlamak için yararlı değildir, ancak daha geniş sorunlarda hata ayıklaması da önemlidir. Uygulamanızın belirli hataları kurtarmak, ancak aynı çözmek için kod değişikliği gerektiren daha geniş tasarım sorunların ipucu. 
> 
> Bu belgede ele alınan hata koşulları uygulama başladığında, daha önce ele alınan nedenlerle hata kodu ve açıklama oturum. Bkz: [hata ve günlük başvuru](#error-and-logging-reference) günlük kod örnekleri için. 
>

## <a name="acquiretokensilent"></a>AcquireTokenSilent

Son kullanıcının bir kullanıcı arabirimi (UI) görmez Garantisi ile bir belirteç almak üzere AcquireTokenSilent çalışır. Çeşitli durumlarda burada sessiz alımı başarısız olabilir ve etkileşimli istekleri aracılığıyla veya varsayılan işleyici tarafından işlenecek ihtiyaçları vardır. Biz, her durumda aşağıdaki bölümlerde uygulamaları nasıl ve ne zaman ayrıntılarını derinlerine.

Bir dizi uygulamaya özgü hata gerektirebilir işletim sistemi tarafından oluşturulan hatalar var. Daha fazla bilgi için bkz: "İşletim sistemi" hataları bölümünde [hata ve günlük başvuru](#error-and-logging-reference). 

### <a name="application-scenarios"></a>Uygulama senaryoları

- [Yerel istemci](developer-glossary.md#native-client) uygulamalar (iOS, Android, .NET Masaüstü veya Xamarin)
- [Web istemcisi](developer-glossary.md#web-client) çağıran uygulamalara bir [kaynak](developer-glossary.md#resource-server) (.NET)

### <a name="error-cases-and-actionable-steps"></a>Hata durumları ve eyleme dönüştürülebilir adımlar

Temelde, AcquireTokenSilent hataların iki durum vardır:

| Servis talebi | Açıklama |
|------|-------------|
| **Case 1**: Hata bir etkileşimli oturum açma ile çözümlenebilir. | Etkileşimli bir istek tarafından geçerli belirteçleri eksikliği nedeniyle hataları için gereklidir. Özellikle, önbellek araması ve bir geçersiz/süresi dolmuş bir yenileme belirteci çözmek için bir AcquireToken çağrısı gerektirir.<br><br>Bu durumlarda, son kullanıcı oturum açmak için sorulması gerekir. Uygulama, etkileşimli bir isteği hemen sonra son kullanıcı etkileşimi (örneğin, bir oturum açma düğmesine basarak) veya daha sonra yapmak seçebilirsiniz. Seçim uygulamasının istenen davranışı üzerinde bağlıdır.<br><br>Kod bu özel durum ve hataları tanılamak için aşağıdaki bölüme bakın.|
| **Durum 2**: Hata, bir etkileşimli oturum açma ile çözümlenebilir değil. | Ağ ve geçici/geçici hataları veya diğer hatalar için etkileşimli bir AcquireToken isteği gerçekleştiren sorunu çözmez. Gereksiz etkileşimli oturum açma yönergeleri son kullanıcılar da rahatsız edebilir. ADAL, tek bir yeniden deneme AcquireTokenSilent hatalarda en hataları için otomatik olarak çalışır.<br><br>İstemci uygulaması da sonraki bir noktada yeniden deneyebilirsiniz ancak bunu nasıl ve ne zaman istenen son kullanıcı deneyimi ve uygulama davranışı üzerinde bağımlı değildir. Örneğin, uygulamayı birkaç dakika sonra veya bazı son kullanıcı eylemine yanıt olarak bir AcquireTokenSilent yeniden deneme yapabilirsiniz. Hemen yeniden aşarak uygulamada neden olur ve denenmedi.<br><br>Aynı hata ile başarısız olan bir sonraki yeniden deneme hata çözümlenmiyor gibi istemci AcquireToken, kullanarak etkileşimli bir istek yapmanız gerektiğini anlamına gelmez.<br><br>Kod bu özel durum ve hataları tanılamak için aşağıdaki bölüme bakın. |

### <a name="net"></a>.NET

Aşağıdaki yönergeler, hata ADAL yöntemleriyle birlikte işleme için örnekler verilmektedir: 

- acquireTokenSilentAsync(...)
- acquireTokenSilentSync(…) 
- [kullanım dışı] acquireTokenSilent(...)
- [kullanım dışı] acquireTokenByRefreshToken(...) 

Kodunuz şu şekilde uygulanması:

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

Aşağıdaki yönergeler, hata ADAL yöntemleriyle birlikte işleme için örnekler verilmektedir: 

- acquireTokenSilentSync(…)
- acquireTokenSilentAsync(...)
- [kullanım dışı] acquireTokenSilent(...)

Kodunuz şu şekilde uygulanması:

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

Aşağıdaki yönergeler, hata ADAL yöntemleriyle birlikte işleme için örnekler verilmektedir: 

- acquireTokenSilentWithResource(...)

Kodunuz şu şekilde uygulanması:

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

AcquireToken belirteçlerini almak için kullanılan varsayılan ADAL yöntemidir. Kullanıcı kimliği gerekli olduğu durumlarda AcquireToken sessizce ilk belirteç almaya çalışır ve ardından (PromptBehavior.Never geçirilmiyorsa) gerekiyorsa, kullanıcı arabirimini görüntüler. Uygulama kimliği gerekli olduğu durumlarda AcquireToken bir belirteç almaya çalışır, ancak hiçbir son kullanıcı olarak kullanıcı Arabirimi göstermez. 

Senaryo uygulama elde etmek çalışıyor ve AcquireToken hataları işleme, hata işleme seçtiğiniz platforma bağlıdır. 

İşletim sistemi hata belirli uygulamasına bağımlı işleme gerektiren hata, bir dizi da oluşturabilirsiniz. Daha fazla bilgi için bkz: "İşletim sistemi hataları" [hata ve günlük başvuru](#error-and-logging-reference). 

### <a name="application-scenarios"></a>Uygulama senaryoları

- Yerel istemci uygulamalar (iOS, Android, .NET Masaüstü veya Xamarin)
- Bir kaynak (.NET) API çağrısı web uygulamaları
- Tek sayfa uygulamaları (JavaScript)
- Hizmetten hizmete uygulamaları (.NET, Java)
  - On-behalf-of dahil olmak üzere tüm senaryolar
  - On-Behalf-of belirli senaryoları

### <a name="error-cases-and-actionable-steps-native-client-applications"></a>Hata durumları ve eyleme dönüştürülebilir adımlar: Yerel istemci uygulamaları

Yerel istemci uygulaması oluşturuyorsanız, hangi ağ sorunları, geçici hataları ve diğer platforma özgü hataları ile ilgili dikkate alınması gereken birkaç hata işleme durumlar vardır. Çoğu durumda, bir uygulamayı hemen yeniden deneme gerçekleştirir ancak bunun yerine bir oturum açma isteyen son kullanıcı etkileşimi için bekleyin olmamalıdır. 

Tek bir yeniden deneme sorunu giderebilir bazı özel durumlar vardır. Örneğin, ne zaman bir kullanıcı bir cihazda veri etkinleştirmesi gerekir veya Azure AD Aracısı tamamlandı ilk hatadan sonra indirin. 

Hata durumlarında, uygulamanın son kullanıcı, bir yeniden deneme ister bazı etkileşim gerçekleştirmek kullanıcı Arabirimi sunabilir. Örneği için cihaz için çevrimdışı bir hatayla başarısız olursa bir AcquireToken isteyen bir "oturum açmayı yeniden deneyin" düğmesine yeniden hata hemen yeniden denenmesi yerine. 

Hata işleme yerel uygulamalarda iki çalışmaları tarafından tanımlanabilir:

|  |  |
|------|-------------|
| **Case 1**:<br>Yeniden denenemeyen hata (çoğu zaman) | 1. Hemen yeniden deneme çalışmayın. Kullanıcı Arabirimi ("oturum açmak için yeniden deneyin", "Azure AD Yükleme Aracısı uygulama", vb.) bir yeniden deneme çağıran belirli hataya göre son kullanıcı yok. |
| **Durum 2**:<br>Yeniden denenebilir hata | 1. Son kullanıcı başarılı bir şekilde sonuçları bir duruma girdiyse gibi tek bir yeniden deneme gerçekleştirin.<br><br>2. Yeniden deneme başarısız olursa, kullanıcı Arabirimi ("oturum açmak için yeniden deneyin", "Azure AD indirme aracı uygulama", vb.) bir yeniden deneme çağıran belirli hataya göre son kullanıcı yok. |

> [!IMPORTANT]
> Bir kullanıcı hesabı için ADAL sessiz bir çağrı ve başarısız iletilmezse, farklı bir hesap kullanarak oturum açın son kullanıcının etkileşimli Taleplerde sağlar. Bir kullanıcı hesabıyla başarılı AcquireToken sonra uygulama, uygulamalar'ın yerel kullanıcı nesnesinin oturum açmış kullanıcı eşleşen doğrulamanız gerekir. Uyuşmazlık özel durum oluşturmaz (Objective C'deki hariç), ancak burada bir kullanıcı bilinen yerel olarak (gibi başarısız sessiz çağrısı) kimlik doğrulama isteklerini önce durumlarda düşünülmelidir.
>

#### <a name="net"></a>.NET

Aşağıdaki yönergeler, hata tüm sessiz olmayan AcquireToken(...) birlikte işleme için örnekler sağlar. ADAL yöntemleri *dışında*: 

- AcquireTokenAsync (..., IClientAssertionCertification,...)
- AcquireTokenAsync (..., ClientCredential,...)
- AcquireTokenAsync (..., ClientAssertion,...)
- AcquireTokenAsync(...,UserAssertion,...)   

Kodunuz şu şekilde uygulanması:

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
> ADAL .NET AcquireTokenSilent gibi davranış olduğu PromptBehavior.Never destekler gibi ek bir husus vardır.
>

Aşağıdaki yönergeler, hata ADAL yöntemleriyle birlikte işleme için örnekler verilmektedir: 

- acquireToken(..., PromptBehavior.Never)

Kodunuz şu şekilde uygulanması:

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

Aşağıdaki yönergeler, hata tüm sessiz olmayan AcquireToken(...) birlikte işleme için örnekler sağlar. ADAL yöntemleri. 

Kodunuz şu şekilde uygulanması:

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

Aşağıdaki yönergeler, hata tüm sessiz olmayan AcquireToken(...) birlikte işleme için örnekler sağlar. ADAL yöntemleri. 

Kodunuz şu şekilde uygulanması:

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

### <a name="error-cases-and-actionable-steps-web-applications-that-call-a-resource-api-net"></a>Hata durumları ve eyleme dönüştürülebilir adımlar: Bir kaynak (.NET) API çağrısı web uygulamaları

Derliyorsanız çağıran bir .NET web uygulaması için bir kaynak bir yetkilendirme kodunu kullanarak bir belirteç alır, gereken tek kod genel çalışması için bir varsayılan işleyici. 

Aşağıdaki yönergeler, hata ADAL yöntemleriyle birlikte işleme için örnekler verilmektedir: 

- AcquireTokenByAuthorizationCodeAsync(...)

Kodunuz şu şekilde uygulanması:

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

### <a name="error-cases-and-actionable-steps-single-page-applications-adaljs"></a>Hata durumları ve eyleme dönüştürülebilir adımlar: Tek sayfa uygulamaları (adal.js)

Adal.js AcquireToken ile kullanarak tek sayfalı uygulama oluşturuyorsanız, hata işleme kodunu tipik bir sessiz çağrısının benzer. Özellikle adal.js içinde AcquireToken hiçbir zaman bir kullanıcı Arabirimi gösterilir. 

Başarısız bir AcquireToken aşağıdaki durumlarda oluşur:

|  |  |
|------|-------------|
| **Case 1**:<br>Etkileşimli bir istekle çözülebilir | 1. Tanımlar: Login() başarısız olursa, hemen yeniden deneme işlemleri yapma. Yalnızca bir kullanıcı eylemi bir yeniden deneme ister sonra yeniden deneyin.|
| **Durum 2**:<br>Değil Resolvable etkileşimli bir isteği ile. Yeniden denenebilir hata var. | 1. Son kullanıcının ana girmiş gibi başarılı bir şekilde sonuçları bir durum tek bir yeniden deneme gerçekleştirin.<br><br>2. Yeniden deneme başarısız olursa, son kullanıcı bir yeniden deneme çağırabileceği belirli hataya göre bir eylemle sunar ("oturum açmayı yeniden deneyin"). |
| **Case 3**:<br>Değil Resolvable etkileşimli bir isteği ile. Yeniden denenebilir hata değil. | 1. Hemen yeniden deneme çalışmayın. Bir yeniden deneme çağırabileceği gelen belirli hataya göre bir eylem ile son kullanıcı sunar ("oturum açmayı yeniden deneyin"). |

Kodunuz şu şekilde uygulanması:

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

### <a name="error-cases-and-actionable-steps-service-to-service-applications-net-only"></a>Hata durumları ve eyleme dönüştürülebilir adımlar: hizmet uygulamalar (yalnızca .NET)

AcquireToken kullanan bir hizmet uygulaması oluşturuyorsanız, kodunuzu işlemesi birkaç anahtar hatalarını yok. Geri çağırma uygulaması (on-behalf-of örnekler) için hata döndürür veya bir yeniden deneme stratejisi uygulamak için başarısızlık yalnızca sunduğumuz var. 

#### <a name="all-scenarios"></a>Tüm senaryolar

İçin *tüm* on-behalf-of dahil olmak üzere, hizmetten hizmete uygulama senaryoları:

- Hemen yeniden denemeyin. Tek bir yeniden deneme belirli ADAL sayısı, istek başarısız oldu. 
- Yalnızca bir kullanıcı veya uygulama eylemi istemleri olduktan sonra yeniden deneniyor. yeniden devam edin. Örneğin, bazı aralığı kümesinde çalışır bir daemon uygulamasının yeniden denemek için İleri aralığına kadar beklemeniz gerekir.

Aşağıdaki yönergeler, hata ADAL yöntemleriyle birlikte işleme için örnekler verilmektedir: 

- AcquireTokenAsync (..., IClientAssertionCertification,...)
- AcquireTokenAsync (..., ClientCredential,...)
- AcquireTokenAsync (..., ClientAssertion,...)
- AcquireTokenAsync (..., UserAssertion,...)

Kodunuz şu şekilde uygulanması:

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

#### <a name="on-behalf-of-scenarios"></a>On-behalf-of senaryoları

İçin *on-behalf-of* hizmetten hizmete uygulama senaryoları.

Aşağıdaki yönergeler, hata ADAL yöntemleriyle birlikte işleme için örnekler verilmektedir: 

- AcquireTokenAsync (..., UserAssertion,...)

Kodunuz şu şekilde uygulanması:

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

Oluşturduğumuz bir [tam örnek](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca) , bu senaryo gösterir.

## <a name="error-and-logging-reference"></a>Hata ve günlük başvurusu

### <a name="logging-personal-identifiable-information-pii--organizational-identifiable-information-oii"></a>Kişisel olarak tanımlanabilir bilgileri (PII) ve kuruluş olarak tanımlanabilir bilgi (OII) günlüğe kaydetme
Varsayılan olarak, ADAL günlüğü bırakmaz yakalamak veya tüm PII veya OII oturum açın. Kitaplık uygulama geliştiricileri bu Günlükçünün sınıfındaki bir ayarlayıcı aracılığıyla açmak izin verir. Uygulama, PII veya OII etkinleştirerek, güvenli bir şekilde yüksek oranda duyarlı veri işleme ve yasal gereksinimlere uymak için sorumluluğunu üstlenir.

### <a name="net"></a>.NET

#### <a name="adal-library-errors"></a>ADAL kitaplığını hataları

Kendi ADAL hatalarıyla, kaynak kodunda keşfetmek için [azure-activedirectory-kitaplığı-için-dotnet depo](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/blob/8f6d560fbede2247ec0e217a21f6929d4375dcaa/src/ADAL.PCL/Utilities/Constants.cs#L58) en iyi hata başvurudur.

#### <a name="guidance-for-error-logging-code"></a>Yönergeler için hata günlüğünü kodu

Üzerinde çalıştığınız platforma bağlı olarak ADAL .NET günlük değişiklikleri. Başvurmak [günlük wiki](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Logging-in-ADAL.Net) için günlük kaydını etkinleştirme kodu.

### <a name="android"></a>Android

#### <a name="adal-library-errors"></a>ADAL kitaplığını hataları

Kendi ADAL hatalarıyla, kaynak kodunda keşfetmek için [azure-activedirectory-kitaplığı-için-android depo](https://github.com/AzureAD/azure-activedirectory-library-for-android/blob/dev/adal/src/main/java/com/microsoft/aad/adal/ADALError.java#L33) en iyi hata başvurudur.

#### <a name="operating-system-errors"></a>İşletim sistemi hataları

Android işletim sistemi hataları AuthenticationException ADAL içinde aracılığıyla sunulur, "SERVER_INVALID_REQUEST" tanımlanabilir ve daha ayrıntılı hata açıklamaları olabilir. 

Sık karşılaşılan hataların ve uygulamanızı veya son kullanıcıların bunları karşılaştığınızda gerçekleştirilecek adımlar tam listesi için başvurmak [ADAL Android Wiki](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki). 

#### <a name="guidance-for-error-logging-code"></a>Yönergeler için hata günlüğünü kodu

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

Kendi ADAL hatalarıyla, kaynak kodunda keşfetmek için [azure-activedirectory-kitaplığı-için-objc depo](https://github.com/AzureAD/azure-activedirectory-library-for-objc/blob/dev/ADAL/src/ADAuthenticationError.m#L295) en iyi hata başvurudur.

#### <a name="operating-system-errors"></a>İşletim sistemi hataları

Kullanıcılar web görünümleri ve kimlik doğrulama yapısını kullandığınızda oturum açma sırasında iOS hataları oluşabilir. Bu SSL hataları, zaman aşımı veya ağ hataları gibi koşullar neden olabilir:

- Yetkilendirme paylaşımı, oturumları kalıcı değildir ve önbellek boş görünür. Aşağıdaki kod satırını Anahtarlığa ekleyerek çözebilirsiniz: `[[ADAuthenticationSettings sharedInstance] setSharedCacheKeychainGroup:nil];`
- Hataları NsUrlDomain kümesi için eylemi uygulama mantığı bağlı olarak değişir. Bkz: [NSURLErrorDomain başvuru belgeleri](https://developer.apple.com/documentation/foundation/nsurlerrordomain#declarations) işlenebilir belirli örnekler için.
- Bkz: [ADAL Obj C sık karşılaşılan sorunları](https://github.com/AzureAD/azure-activedirectory-library-for-objc#adauthenticationerror) ADAL Objective-C ekibi tarafından korunan sık karşılaşılan hataların listesi.

#### <a name="guidance-for-error-logging-code"></a>Yönergeler için hata günlüğünü kodu

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

### <a name="guidance-for-error-logging-code---javascript"></a>Yönergeler için hata günlüğünü kod - JavaScript 

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

Geri bildirim sağlamak ve geliştirmek ve içeriklerimizde şekil yardımcı aşağıdaki Açıklamalar bölümüne kullanın.

[![Oturum açın düğmesi][AAD-Sign-In]][AAD-Sign-In]
<!--Reference style links -->

[AAD-Auth-Libraries]: ./active-directory-authentication-libraries.md
[AAD-Auth-Scenarios]:authentication-scenarios.md
[AAD-Dev-Guide]:azure-ad-developers-guide.md
[AAD-Integrating-Apps]:quickstart-v1-integrate-apps-with-azure-ad.md
[AZURE-portal]: https://portal.azure.com

<!--Image references-->
[AAD-Sign-In]:./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png

