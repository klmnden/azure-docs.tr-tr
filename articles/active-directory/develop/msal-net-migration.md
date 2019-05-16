---
title: Geçiş için MSAL.NET | Azure
description: Microsoft kimlik doğrulama kitaplığı için .NET (MSAL.NET) ve Azure AD kimlik doğrulama kitaplığı için .NET (ADAL.NET) ve MSAL.NET için geçirme arasındaki farklar hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 028c7f5d42587a6b2129bba07831b0e799d607f4
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544228"
---
# <a name="migrating-applications-to-msalnet"></a>MSAL.NET uygulamalarını geçirme

Hem Microsoft kimlik doğrulama kitaplığı için .NET (MSAL.NET) ve Azure AD kimlik doğrulama kitaplığı için .NET (ADAL.NET), varlıklar Azure AD kimlik doğrulaması ve Azure AD'den belirteçler istemek için kullanılır. Şimdiye kadar Çoğu geliştirici, geliştiricilerin platformlar (v1.0) Azure AD Authentication Library (ADAL) kullanarak belirteçleri isteğinde bulunarak Azure AD kimlikleri (iş ve Okul hesapları) kimlik doğrulaması için Azure AD ile çalıştı. Şimdi, MSAL.NET kullanarak, Microsoft kimlik platformu uç noktası aracılığıyla Microsoft kimlikleri (Azure AD kimlikleri ve Microsoft hesapları ve sosyal ve yerel hesapları Azure AD B2C ile) daha geniş bir dizi doğrulayabilir. 

Bu makalede, Microsoft kimlik doğrulama kitaplığı için .NET (MSAL.NET) ve Azure AD kimlik doğrulama kitaplığı için .NET (ADAL.NET) arasında seçim açıklar ve iki kitaplıkları karşılaştırır.  

## <a name="differences-between-adal-and-msal-apps"></a>ADAL ve MSAL uygulamaları arasındaki farklar
Çoğu durumda, MSAL.NET ve Microsoft kimlik doğrulama kitaplıkları en yeni nesil Microsoft kimlik platformu endpoint kullanmak istiyorsunuz. İmzalama için uygulamanızı Azure AD'ye (iş ve Okul hesaplarında) (Kişisel) Microsoft hesapları (MSA) bileşenini kullanıcılar için belirteçlerini almak MSAL.NET kullanarak veya Azure AD B2C. 

Zaten Azure AD geliştiricilerin (v1.0) uç noktası (ve ADAL.NET) ile ilgili bilgi sahibi olduğunuz, okumak isteyebilirsiniz [Microsoft kimlik Platformu (v2.0) uç noktası hakkında farklı nedir?](active-directory-v2-compare.md).

Ancak, önceki sürümleriyle kullanıcılar oturum açmak uygulamanızın ihtiyaç duyduğu ADAL.NET kullanırsanız yine [Active Directory Federasyon Hizmetleri (ADFS)](/windows-server/identity/active-directory-federation-services). Daha fazla ayrıntı için [ADFS Destek](https://aka.ms/msal-net-adfs-support).

Aşağıdaki resimde ADAL.NET MSAL.NET arasındaki farklılıklar özetlenmiştir ![yan yana kod](./media/msal-compare-msaldotnet-and-adaldotnet/differences.png)

### <a name="nuget-packages-and-namespaces"></a>NuGet paketleri ve ad alanları

ADAL.NET tüketilen [Microsoft.IdentityModel.Clients.activedirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) NuGet paketi. ad alanı kullanılacak `Microsoft.IdentityModel.Clients.ActiveDirectory`.

MSAL.NET ihtiyaç duyacağınız eklemek için kullanılacak [Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client) NuGet paketini bulun ve kullanın `Microsoft.Identity.Client` ad alanı

### <a name="scopes-not-resources"></a>Kapsamları kaynakları değil

ADAL.NET için belirteç edinme *kaynakları*, ancak MSAL.NET için belirteç edinme *kapsamları*. Bir dizi MSAL.NET AcquireToken geçersiz kılmaları kapsamları adlı bir parametreyi gerektirir (`IEnumerable<string> scopes`). Bu parametre istenen izinlere ve istenen kaynaklara bildirmek dizeleri basit bir listesidir. İyi bilinen kapsamları misiniz [Microsoft Graph'ın kapsamları](/graph/permissions-reference).

Ayrıca v1.0 kaynaklarına erişmek için MSAL.NET mümkündür. Ayrıntılara bakın [v1.0 uygulama için kapsamları](#scopes-for-a-web-api-accepting-v10-tokens). 

### <a name="core-classes"></a>Temel sınıflar

- ADAL.NET kullanan [Authenticationcontext'i](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/AuthenticationContext:-the-connection-to-Azure-AD) bağlantınız üzerinden bir yetkili güvenlik belirteci hizmeti (STS) veya yetkilendirme sunucusu temsili olarak. Geçici bir çözüm tam, MSAL.NET tasarlanmıştır [istemci uygulamaları](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Client-Applications). İki ayrı sınıf sağlar: `PublicClientApplication` ve `ConfidentialClientApplication`

- Belirteçlerini almak: ADAL.NET ve MSAL.NET sahip aynı kimlik doğrulama çağrıları (`AcquireTokenAsync` ve `AcquireTokenSilentAsync` ADAL.NET için ve `AqquireTokenInteractive` ve `AcquireTokenSilent` MSAL.NET içinde) ancak farklı parametreler gereklidir. Bir farktır MSAL.NET içinde artık geçirin gerekmez, olgu `ClientID` uygulamanızın her AcquireTokenXX çağrı. Aslında, `ClientID` oluştururken yalnızca bir kez ayarlanır (`IPublicClientApplication` veya `IConfidentialClientApplication`).

### <a name="iaccount-not-iuser"></a>IAccount Iuser değil

Kullanıcılar ADAL.NET yönetilebilir. Ancak, bir kullanıcı bir insan veya yazılım aracı, ancak Microsoft kimlik sistemi (birkaç Azure AD hesapları, Azure AD B2C'de, kişisel Microsoft hesapları) için sahip/sahibi/olması bir veya daha fazla hesap sorumlu. 

MSAL.NET 2.x artık hesap kavramını (IAccount arabiriminden) tanımlar. Bu değişiklik doğru semantiği sağlar: aynı kullanıcı farklı Azure'da çeşitli hesapların olabilir olgu AD dizinleri. Ayrıca ana hesap bilgileri koşuluyla MSAL.NET Konuk senaryolarında, daha iyi bilgiler sağlar.

Iuser IAccount arasındaki farklar hakkında daha fazla bilgi için bkz. [MSAL.NET 2.x](https://aka.ms/msal-net-2-released).

### <a name="exceptions"></a>Özel Durumlar

#### <a name="interaction-required-exceptions"></a>Etkileşim gerekli özel durumları

MSAL.NET daha açık özel durumlar vardır. ADAL sessiz kimlik doğrulaması başarısız olduğunda, özel durumları yakalamalı ve aramak için oluşan bir yordamdır `user_interaction_required` hata kodu:

```csharp
catch(AdalException exception)
{
 if (exception.ErrorCode == “user_interaction_required”)
 {
  try
  {“try to authenticate interactively”}}
 }
}
```

Ayrıntılara bakın [bir belirteç almak için önerilen Düzen](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/AcquireTokenSilentAsync-using-a-cached-token#recommended-pattern-to-acquire-a-token) ADAL.NET ile

Catch, MSAL.NET kullanarak `MsalUiRequiredException` açıklandığı [AcquireTokenSilent](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AcquireTokenSilentAsync-using-a-cached-token).

```csharp
catch(MsalUiRequiredException exception)
{
 try {“try to authenticate interactively”}
}
```

#### <a name="handling-claim-challenge-exceptions"></a>Talep karşılıklı özel durumları işleme

Aşağıdaki şekilde özel durumların işlenme sınama ADAL.NET içinde talep:

- `AdalClaimChallengeException` bir özel durum (öğesinden türetme `AdalServiceException`) durumda (örneğin iki faktör kimlik doğrulaması) kullanıcıdan daha fazla talepleri kaynak gerektiren hizmet tarafından oluşturulur. `Claims` Üyeyi içeren bazı JSON parçası önermelerle beklenir.
- Hala ADAL.NET içinde çağırmak bu özel durumuyla genel istemci uygulaması gereken `AcquireTokenInteractive` talep parametreye sahip geçersiz kılar. Bu geçersiz kılma `AcquireTokenInteractive` gerekli olmadığından isabetli önbellek okuması bile denemez. Belirteç önbelleğinde doğru talepleri yok nedeni (Aksi halde bir `AdalClaimChallengeException` değil atılmış olup). Bu nedenle, önbellek aramak için gerek yoktur. Unutmayın `ClaimChallengeException` OBO, bulunurken bir Webapı içinde ise alınan `AcquireTokenInteractive` bu Web API'ye çağrı yapma genel istemci uygulamasında çağrılması gerekir.
- Ayrıntılar için işleme örnekleri dahil olmak üzere bkz [AdalClaimChallengeException](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Exceptions-in-ADAL.NET#handling-adalclaimchallengeexception)

Aşağıdaki şekilde özel durumların işlenme sınama MSAL.NET içinde talep:

- `Claims` Olarak ortaya çıkmış `MsalServiceException`.
- Var olan bir `.WithClaim(claims)` uygulayabilirsiniz yöntemi `AcquireTokenInteractive` Oluşturucusu. 

### <a name="supported-grants"></a>Desteklenen verir

Tüm verir, MSAL.NET ve v2.0 uç noktası henüz desteklenmemektedir. MSAL ADAL.NET karşılaştıran bir özeti verilmiştir. NET verir desteklenen.

#### <a name="public-client-applications"></a>Genel istemci uygulamaları

İşte ADAL.NET ve MSAL.NET Masaüstü ve mobil uygulamalar için desteklenen verir

Erişim İzni Verme | ADAL.NET | MSAL.NET
----- |----- | -----
Etkileşimli | [Etkileşimli kimlik doğrulaması](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Acquiring-tokens-interactively---Public-client-application-flows) | [Etkileşimli olarak MSAL.NET belirteçleri alınıyor](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Acquiring-tokens-interactively)
Tümleşik Windows Kimlik Doğrulaması | [(Kerberos) Windows tümleşik kimlik doğrulaması](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/AcquireTokenSilentAsync-using-Integrated-authentication-on-Windows-(Kerberos)) | [Tümleşik Windows kimlik doğrulaması](msal-authentication-flows.md#integrated-windows-authentication)
Kullanıcı adı / parola | [Kullanıcı adı ve parola ile belirteçlerini almak](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Acquiring-tokens-with-username-and-password)| [Kullanıcı adı parola kimlik doğrulaması](msal-authentication-flows.md#usernamepassword)
Cihaz kod akışı | [Web tarayıcıları olmayan cihazlar için cihaz profili](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Device-profile-for-devices-without-web-browsers) | [Cihaz kod akışı](msal-authentication-flows.md#device-code)

#### <a name="confidential-client-applications"></a>Gizli istemci uygulamaları

Web uygulamaları, Web API'leri ve arka plan programı uygulamaları için ADAL.NET ve MSAL.NET desteklenen izinler şunlardır:

Uygulama türü | Erişim İzni Verme | ADAL.NET | MSAL.NET
----- | ----- | ----- | -----
Web uygulaması, Web API'si, arka plan programı | İstemci kimlik bilgileri | [İstemci kimlik bilgisi ADAL.NET akışlarında](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Client-credential-flows) | [İstemci kimlik bilgisi akan MSAL.NET](msal-authentication-flows.md#client-credentials))
Web API'si | Şu kişi adına: | [Hizmetten hizmete çağrılar kullanıcı adına ADAL.NET ile](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Service-to-service-calls-on-behalf-of-the-user) | [Adına, MSAL.NET](msal-authentication-flows.md#on-behalf-of)
Web Uygulaması | Kimlik doğrulama kodu | [Yetkilendirme kodları ADAL.NET ile web apps üzerinde belirteçleriyle alınıyor](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Acquiring-tokens-with-authorization-codes-on-web-apps) | [Yetkilendirme kodları A MSAL.NET ile web apps üzerinde belirteçleriyle alınıyor](msal-authentication-flows.md#authorization-code)

### <a name="cache-persistence"></a>Önbellek kalıcılığı

ADAL.NET genişletmenizi sağlayan `TokenCache` kullanarak (.NET Framework ve .NET core) bir güvenli depolama olmadan platformlarda istenen Kalıcılık işlevselliği uygulamak için sınıfı `BeforeAccess`, ve `BeforeWrite` yöntemleri. Ayrıntılar için bkz [ADAL.NET belirteç önbelleği serileştirme](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Token-cache-serialization).

MSAL.NET kaldırma yeteneği genişletmek için bir korumalı sınıf belirteç önbelleği sağlar. Bu nedenle, uygulamanıza bir belirteç önbelleği Kalıcılık korumalı belirteç önbelleği ile etkileşime giren bir yardımcı sınıfı biçiminde olması gerekir. Bu etkileşim açıklanmıştır [MSAL.NET belirteç önbelleği serileştirme](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/token-cache-serialization).

## <a name="signification-of-the-common-authority"></a>Ortak yetkilisinin signification

V1.0 kullanırsanız, içindeki https://login.microsoftonline.com/common yetkilisine (tüm kuruluşlar için) herhangi bir AAD hesabıyla oturum açmasına izin. Bkz: [ADAL.NET yetkilisi doğrulama](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/AuthenticationContext:-the-connection-to-Azure-AD#authority-validation)

Kullanırsanız https://login.microsoftonline.com/common v2.0 yetkilisindeki AAD kuruluşlarla veya bir Microsoft Kişisel hesabı (MSA) kullanarak oturum açma olanağı sağlayacaktır. Bir AAD hesabı (aynı davranışı ADAL.NET ile), oturum açma kısıtlamak istiyorsanız, MSAL.NET içinde kullanmanız gerekir https://login.microsoftonline.com/organizations. Ayrıntılar için bkz `authority` parametresinde [genel istemci uygulaması](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Client-Applications#publicclientapplication).

## <a name="v10-and-v20-tokens"></a>V1.0 ve v2.0 belirteçleri

Belirteçlerin iki sürümü vardır:
- V1.0 belirteçleri
- V2.0 belirteçleri 

(ADAL tarafından kullanılır) v1.0 uç noktası yalnızca v1.0 belirteçleri yayar.

Ancak, Web API'si kabul eden belirteç sürümünü v2.0 uç noktası (MSAL tarafından kullanılır) yayar. Bir özellik uygulama bildiriminin Web API, geliştiricilerin belirtecinin hangi sürümü kabul seçin olanak tanır. Bkz: `accessTokenAcceptedVersion` içinde [uygulama bildirimini](reference-app-manifest.md) başvuru belgeleri.

V1.0 ve v2.0 belirteçleri hakkında daha fazla bilgi için bkz: [Azure Active Directory erişim belirteçleri](access-tokens.md)

## <a name="scopes-for-a-web-api-accepting-v10-tokens"></a>V1.0 belirteçleri kabul eden bir Web API'si için kapsamları

OAuth2, v1.0 web API (kaynak) uygulamasının istemci uygulamalarına sunan izin kapsamları izinlerdir. Bu izin kapsamları, onay işlemi sırasında istemci uygulamalara verilebilir. Oauth2Permissions içinde hakkındaki bölüme bakın [Azure Active Directory Uygulama bildirimini](active-directory-application-manifest.md).

### <a name="scopes-to-request-access-to-specific-oauth2-permissions-of-a-v10-application"></a>Belirli OAuth2 izinleri v1.0 uygulamanın erişim istemek için kapsamları

V1.0 uygulamasının belirli kapsamlarının belirteçlerini almak istiyorsanız (örneğin olduğundan AAD graph https://graph.windows.net), oluşturmanız gerekir `scopes` bir istenen kaynak tanımlayıcısı bu kaynak için istenen bir OAuth2 izinle birleştirerek tarafından.

Örneğin, bir uygulama kimliği URI'si, Web API'si v1.0 kullanıcı adını erişmek için `ResourceId`, kullanmak istediğiniz:

```csharp
var scopes = new [] {  ResourceId+"/user_impersonation"};
```

Okuma ve yazma MSAL.NET Azure AAD graph API'yi kullanarak Active Directory ile istiyorsanız (https://graph.windows.net/), aşağıdaki kod parçacığında kapsamların gibi bir liste oluşturun:

```csharp
ResourceId = "https://graph.windows.net/";
var scopes = new [] { ResourceId + “Directory.Read”, ResourceID + “Directory.Write”}
```

#### <a name="warning-should-you-have-one-or-two-slashes-in-the-scope-corresponding-to-a-v10-web-api"></a>Uyarı: Bir veya iki eğik çizgi, karşılık gelen bir v1.0 Web API'si kapsamında olmalıdır

Azure Resource Manager API'si için karşılık gelen kapsam yazmak istiyorsanız (https://management.core.windows.net/), aşağıdaki kapsamın (Not iki eğik çizgi) istemeniz gerekir 

```csharp
var scopes = new[] {"https://management.core.windows.net//user_impersonation"};
var result = await app.AcquireTokenInteractive(scopes).ExecuteAsync();

// then call the API: https://management.azure.com/subscriptions?api-version=2016-09-01
```

Resource Manager API'si bir eğik çizgi, İzleyici talebi bekliyor olmasıdır (`aud`), ve ardından kapsamı API addan ayırmak için bir eğik çizgi yoktur.

Azure AD tarafından kullanılan mantıksal aşağıda verilmiştir:
- Bir v1.0 erişim belirteci (tek olası) aud ile ADAL (v1.0) uç noktası için kaynak =
- MSAL aud v2.0 belirteç kabul eden bir kaynak için bir erişim belirteci isteyen (v2.0 uç noktası) için kaynak =. Uygulama Kimliği
- Azure AD, bir erişim belirteci (yukarıdaki söz konusu olduğu) v1.0 bir erişim belirteci kabul eden bir kaynak için soran MSAL için (v2.0 uç noktası), son eğik çizgiden önce her şeyi alma ve kaynak tanımlayıcısı kullanılarak istenen hedef istenen kapsamından ayrıştırır. Bu nedenle, https://database.windows.net hedef kitlesine bekliyor "https://database.windows.net/", istek bir kapsam gerekir https://database.windows.net//.default. Bkz: #'de sorun[747 ABD Doları](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/747): Sql kimlik doğrulaması hatası #747 durdurulmasına kaynak URL'nin sonunda eğik çizgi atlanır


### <a name="scopes-to-request-access-to-all-the-permissions-of-a-v10-application"></a>V1.0 uygulamasının tüm izinleri erişim istemek için kapsamları

V1.0 uygulamanın statik tüm kapsamlar için bir belirteç almak istiyorsanız, örneğin, birini kullanması gerekir

```csharp
ResourceId = "someAppIDURI";
var scopes = new [] {  ResourceId+"/.default"};
```

### <a name="scopes-to-request-in-the-case-of-client-credential-flow--daemon-app"></a>Kapsamların söz konusu olduğunda, istemci istek için kimlik bilgisi akış / daemon uygulama

Söz konusu olduğunda istemci kimlik bilgileri akışı, geçirilecek kapsamını da olacaktır `/.default`. Bu Azure AD'ye söyler: "Yönetici uygulama kaydında seçtiği tüm uygulama düzeyinde izinleri.

## <a name="adal-to-msal-migration"></a>MSAL geçiş için ADAL

ADAL.NET v2'de. X, yenileme belirteçleri, böylece bunları önbelleğe alma ve kullanarak bu belirteçleri kullanımını geçici çözümleri geliştirmek üzere ifşa `AcquireTokenByRefreshToken` ADAL 2.x tarafından sağlanan yöntemleri. Bu çözümleri bazı senaryolarda gibi kullanılıyordu:
* Kullanıcılar artık bağlı ise kullanıcılar adına panolar yenileme dahil olmak üzere eylemleri yapın, uzun süre çalışan hizmetler. 
* WebFarm senaryoları RT web Service'in için istemciyi etkinleştirme (önbelleğe alma gerçekleştirilir istemci tarafı, şifrelenmiş bir tanımlama bilgisi ve sunucu tarafı)

Ancak, bu şekilde güvenlik nedenleriyle yenileme kullanan hiçbir uzun öneri olarak belirteçleri, MSAL.NET, durum değil. Bu geçiş için MSAL zorlaştıran 3.x API olarak daha önceden alınan yenileme belirteçleri geçirmek için bir yol sağlamaz. 

Neyse ki, MSAL.NET artık önceki yenileme belirteçlerinizi geçirmenizi sağlayan bir API'ye sahip olmalıdır `IConfidentialClientApplication` 

```CSharp
/// <summary>
/// Acquires an access token from an existing refresh token and stores it and the refresh token into 
/// the application user token cache, where it will be available for further AcquireTokenSilent calls.
/// This method can be used in migration to MSAL from ADAL v2 and in various integration 
/// scenarios where you have a RefreshToken available. 
/// (see https://aka.ms/msal-net-migration-adal2-msal2)
/// </summary>
/// <param name="scopes">Scope to request from the token endpoint. 
/// Setting this to null or empty will request an access token, refresh token and ID token with default scopes</param>
/// <param name="refreshToken">The refresh token from ADAL 2.x</param>
IByRefreshToken.AcquireTokenByRefreshToken(IEnumerable<string> scopes, string refreshToken);
```
 
Bu yöntemle, istediğiniz herhangi bir kapsam (kaynaklar) yanı sıra daha önce kullanılmış bir yenileme belirteci sağlayabilirsiniz. Yenileme belirteci değiş tokuş için yeni bir tane ve uygulamanıza önbelleğe alınmış.  

Bu yöntem, tipik olmayan senaryoları içindir gibi kolayca erişilebilir değil `IConfidentialClientApplication` ilk için atama `IByRefreshToken`.

Bu kod parçacığı bir gizli istemci uygulamasında bazı geçiş kodunu gösterir. `GetCachedRefreshTokenForSignedInUser` Bazı depolama alanında depolanan yararlanın 2.x ADAL için kullanılan uygulama, önceki bir sürümü ile yenileme belirtecini alır. `GetTokenCacheForSignedInUser` (kullanıcı başına bir önbellek gizli istemci uygulamaları olmalıdır gibi) oturum açmış kullanıcı için bir önbelleği seri durumdan çıkarır.

```csharp
TokenCache userCache = GetTokenCacheForSignedInUser();
string rt = GetCachedRefreshTokenForSignedInUser();

IConfidentialClientApplication app;
app = ConfidentialClientApplicationBuilder.Create(clientId)
 .WithAuthority(Authority)
 .WithRedirectUri(RedirectUri)
 .WithClientSecret(ClientSecret)
 .Build();
IByRefreshToken appRt = app as IByRefreshToken;
         
AuthenticationResult result = await appRt.AcquireTokenByRefreshToken(null, rt)
                                         .ExecuteAsync()
                                         .ConfigureAwait(false);
```

Bir erişim belirteci ve yeni, yenileme belirteci önbellekte depolanır, AuthenticationResult içinde döndürülen kimlik belirteci görürsünüz.

Bu yöntem ayrıca çeşitli tümleştirme senaryoları için kullanılabilen bir yenileme belirteci bulunduğu de kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Kapsamlar hakkında daha fazla bilgi bulabilirsiniz [kapsamlar, izinler ve onay Microsoft kimlik platformu uç noktasını](v2-permissions-and-consent.md)