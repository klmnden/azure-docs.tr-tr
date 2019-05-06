---
title: Azure AD B2C'yi (Microsoft kimlik doğrulama kitaplığı .NET için) | Azure
description: Azure AD B2C ile Microsoft kimlik doğrulama kitaplığı .NET (MSAL.NET) kullanırken hakkında belirli değerlendirmeler öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: celested
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/24/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5c608518a9eb80d807297f010778ae452c0f61f5
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075783"
---
# <a name="use-msalnet-to-sign-in-users-with-social-identities"></a>Sosyal medya kimliklerinden kullanıcıları oturum açma için MSAL.NET kullanın

MSAL.NET kullanarak sosyal medya kimliklerinden kullanıcıları oturum için kullanabileceğiniz [Azure Active Directory B2C'yi (Azure AD B2C)](https://aka.ms/aadb2c). Azure AD B2C ilkeleri kavramı üretilmiştir. MSAL.NET içinde bir yetkilisi sağlamak için bir ilke belirtmek çevirir.

- Genel istemci uygulamasını başlattığınızda, ilke yetkilisi belirtmeniz gerekir.
- Bir ilkeyi uygulamak istediğinizde, geçersiz kılma çağırmanız gerekir `AcquireTokenInteractive` içeren bir `authority` parametresi.

Bu sayfa için MSAL, 3.x. MSAL ilgileniyorsanız 2.x, lütfen [MSAL, Azure AD B2C özellikleri 2.x](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-Specifics-MSAL-2.x).

## <a name="authority-for-a-azure-ad-b2c-tenant-and-policy"></a>Bir Azure AD B2C kiracısı ve ilke yetkilisi

Kullanma yetkisi olan `https://login.microsoftonline.com/tfp/{tenant}/{policyName}` burada:

- `tenant` Azure AD B2C kiracısı adıdır, 
- `policyName` (örneğin, oturum-içinde açma/kaydolma için "b2c_1_susi") uygulamak için bir ilke adı.

Azure AD B2C geçerli kılavuzdan kullanmaktır `b2clogin.com` yetkilisi olarak. Örneğin, `$"https://{your-tenant-name}.b2clogin.com/tfp/{your-tenant-ID}/{policyname}"`. Daha fazla bilgi için bkz. Bu [belgeleri](/azure/active-directory-b2c/b2clogin).

## <a name="instantiating-the-application"></a>Uygulama örnekleme

Uygulama oluştururken, yetki vermeniz gerekir.

```csharp
// Azure AD B2C Coordinates
public static string Tenant = "fabrikamb2c.onmicrosoft.com";
public static string ClientID = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
public static string PolicySignUpSignIn = "b2c_1_susi";
public static string PolicyEditProfile = "b2c_1_edit_profile";
public static string PolicyResetPassword = "b2c_1_reset";

public static string AuthorityBase = $"https://fabrikamb2c.b2clogin.com/tfp/{Tenant}/";
public static string Authority = $"{AuthorityBase}{PolicySignUpSignIn}";
public static string AuthorityEditProfile = $"{AuthorityBase}{PolicyEditProfile}";
public static string AuthorityPasswordReset = $"{AuthorityBase}{PolicyResetPassword}";

application = PublicClientApplicationBuilder.Create(ClientID)
               .WithB2CAuthority(Authority)
               .Build();
```

## <a name="acquire-a-token-to-apply-a-policy"></a>Bir ilkeyi uygulamak için bir belirteç Al

Azure AD B2C için bir belirteç alınırken bir ortak istemci uygulamasındaki korumalı API geçersiz kılmaları bir yetkilisiyle kullanmanızı gerektirir:

```csharp
IEnumerable<IAccount> accounts = await application.GetAccountsAsync();
AuthenticationResult ar = await application .AcquireToken(scopes, parentWindow)
                                            .WithAccount(GetAccountByPolicy(accounts, policy))
                                            .ExecuteAsync();
```

Yeni değer:

- `policy` Önceki dizelerinden biri olan (örneğin `PolicySignUpSignIn`).
- `GetAccountByPolicy(IEnumerable<IAccount>, string)` belirli bir ilke için bir hesap bulur bir yöntemdir. Örneğin:

  ```csharp
  private IAccount GetAccountByPolicy(IEnumerable<IAccount> accounts, string policy)
  {
   foreach (var account in accounts)
   {
    string userIdentifier = account.HomeAccountId.ObjectId.Split('.')[0];
    if (userIdentifier.EndsWith(policy.ToLower()))
     return account;
   }
   return null;
  }
  ```

(Örneğin izin vermesini profillerini düzenleyebilir veya parolasını sıfırlama son kullanıcı) bir ilke uygulama şu anda yapılır çağırarak `AcquireTokenInteractive`. Bu iki ilke söz konusu olduğunda, döndürülen belirteç kullanma / kimlik doğrulaması sonucu.

## <a name="special-case-of-editprofile-and-resetpassword-policies"></a>Özel durum EditProfile ve ResetPassword ilkeleri

Burada, son kullanıcılarınızın bir sosyal kimlik bilgilerinizle oturum bir deneyim sağlamak ve ardından profillerini düzenleyebilir istediğinizde Azure AD B2C EditProfile ilkesi uygulamak ister. Bunu yapmak için bir yoldur çağırarak `AcquireTokenInteractive` belirli yetkilisini bu ilkeyi ve ayarlamak bir komut istemi ile `Prompt.NoPrompt` (kullanıcı zaten oturum açmış olduğu gibi) görüntülenecek hesap seçimi iletişim kutusunu önlemek için

```csharp
private async void EditProfileButton_Click(object sender, RoutedEventArgs e)
{
 IEnumerable<IAccount> accounts = await app.GetAccountsAsync();
 try
 {
  var authResult = await app.AcquireToken(scopes:App.ApiScopes)
                               .WithAccount(GetUserByPolicy(accounts, App.PolicyEditProfile)),
                               .WithPrompt(Prompt.NoPrompt),
                               .WithB2CAuthority(App.AuthorityEditProfile)
                               .ExecuteAsync();
  DisplayBasicTokenInfo(authResult);
 }
 catch
 {
  . . .
 }
}
```
## <a name="resource-owner-password-credentials-ropc-with-azure-ad-b2c"></a>Azure AD B2C ile kaynak sahibi parola kimlik (ROPC)
ROPC akışı hakkında daha fazla ayrıntı için lütfen bu makaleye [belgeleri](v2-oauth-ropc.md).

Bu akış **önerilmez** uygulamanızı kendi parolasını anın güvenli olmadığı için. Bu sorun hakkında daha fazla bilgi için bkz. [bu makalede](https://news.microsoft.com/features/whats-solution-growing-problem-passwords-says-microsoft/). 

Kullanıcı adı/parola kullanarak, veren etmenizi Yukarı:
- modern kimlik kiracılar Çekirdek: parola fished, yeniden yürütülmesi. Çünkü bu kavramı, geçirilebilir bir paylaşım gizli dizi sahip olabiliyoruz. Bu, parolasız ile uyumlu değil.
- MFA yapmak için gereken kullanıcılar etkileşimi (olduğu gibi) oturum açmanız mümkün olmayacaktır.
- Kullanıcılara çoklu oturum açmayı mümkün olmayacaktır.

### <a name="configure-the-ropc-flow-in-azure-ad-b2c"></a>Azure AD B2C'de ROPC akışı Yapılandır
Azure AD B2C kiracınıza yeni kullanıcı akışı oluşturun ve seçin **ROPC kullanarak oturum**. Bu ROPC ilke kiracınız için etkinleştirir. Bkz: [kaynak sahibi parola kimlik bilgileri akışı yapılandırma](/azure/active-directory-b2c/configure-ropc) daha fazla ayrıntı için.

`IPublicClientApplication` bir yöntem içerir:
```csharp
AcquireTokenByUsernamePassword(
            IEnumerable<string> scopes,
            string username,
            SecureString password)
```

Bu yöntem, parametre alır:
- *Kapsamları* için bir erişim belirteci istemek için.
- A *username*.
- Bir SecureString *parola* kullanıcı.

ROPC ilke içerdiği yetkilisi kullanmayı unutmayın.

### <a name="limitations-of-the-ropc-flow"></a>ROPC akışı sınırlamaları
 - ROPC akışı **yalnızca yerel hesaplar için çalışır** (Azure AD B2C ile kaydetmek burada bir e-posta veya kullanıcı adı kullanarak). Bu akış, herhangi bir Azure AD B2C tarafından desteklenen kimlik sağlayıcıları Federasyon varsa çalışmaz (Facebook, Google, vs.).
 - Şu anda **hiçbir id_token döndürülen Azure AD B2C'den** MSAL ROPC akıştan uygularken. Önbellekte olacak şekilde hesap ve kullanıcı bu hesabı nesnesi oluşturulamıyor, anlamına gelir. Bu senaryoda AcquireTokenSilent akış çalışmaz. Ancak, ROPC bir kullanıcı Arabirimi göstermez, bu nedenle kullanıcı deneyimini herhangi bir etkisi olacaktır.

## <a name="google-auth-and-embedded-webview"></a>Google kimlik doğrulama ve katıştırılmış Webview

Azure AD B2C geliştiriciyseniz Google izin vermiyor gibi Google kimlik sağlayıcısı olarak kullanarak biz sistem tarayıcı, kullandığınız recommand [ekli Web görünümleri kimlik doğrulamasını](https://developers.googleblog.com/2016/08/modernizing-oauth-interactions-in-native-apps.html). Şu anda `login.microsoftonline.com` Google ile güvenilir bir yetkili olan. Bu yetki kullanarak katıştırılmış webview ile çalışır. Ancak `b2clogin.com` kullanıcıların kimlik doğrulaması için bu nedenle, Google ile güvenilir bir yetkili değil.

Wiki ve bunun için bir güncelleştirme sağlayacağız [sorunu](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/688) şeyler değiştirirseniz.

## <a name="caching-with-azure-ad-b2c-in-msalnet"></a>Azure AD B2C'de MSAL.Net ile önbelleğe alma 

### <a name="known-issue-with-azure-ad-b2c"></a>Azure AD B2C ile ilgili bilinen sorun

MSAL.Net destekleyen bir [belirteç önbelleği](/dotnet/api/microsoft.identity.client.tokencache?view=azure-dotnet). Anahtar önbelleğe alma belirteci kimlik sağlayıcısı tarafından döndürülen talepleri temel alır. Şu anda bir belirteç önbelleği anahtarı oluşturmak için iki talep MSAL.Net gerekir:  
- `tid` Azure AD Kiracı kimliği olduğu ve 
- `preferred_username` 

Hem bu talep Azure AD B2C senaryoların çoğunun eksik. 

Müşteri etkisi kullanıcı adı alanı görüntülemek çalışırken "Belirteci yanıtından eksik" alıyorsanız, değer mi? Azure AD B2C'yi bir değer preferred_username için ilgili Idtoken de sosyal hesaplar ve Dış kimlik sağlayıcı (IDP) sınırlamaları nedeniyle döndürmeyen Öyleyse olmasıdır. Azure AD kullanıcısının kim olduğunu bildiğinden preferred_username için bir değer döndürür, ancak kullanıcı bir yerel hesapla oturum, Facebook, Google, GitHub'ı oturum açamaz çünkü Azure AD B2C, vb. vardır değil preferred_username için kullanılacak Azure AD B2C için tutarlı bir değer. MSAL, ADAL önbellek uyumluluğu sunmaya gelen engelini kaldırmak için biz "Belirteci yanıtından eksik" bizim tarafımızda Idtoken preferred_username için hiçbir şey döndürüldüğünde, Azure AD B2C hesaplarla ilgilenirken kullanılmasına karar verildi. MSAL preferred_username kitaplıkları önbellek uyumluluğu korumak bir değer döndürmesi gerekir.

### <a name="workarounds"></a>Geçici Çözümler

#### <a name="mitigation-for-the-missing-tenant-id"></a>Risk azaltma için Kiracı kimliği eksik

Önerilen çözüm kullanmaktır [İlkesi tarafından önbelleğe alma](#acquire-a-token-to-apply-a-policy)

Alternatif olarak, `tid` kullanıyorsanız, talep [B2C özel ilkeler](https://aka.ms/ief), uygulamaya ek talep döndürülecek yeteneği sağlar. Hakkında daha fazla bilgi edinmek için [talep dönüştürme](/azure/active-directory-b2c/claims-transformation-technical-profile)

#### <a name="mitigation-for-missing-from-the-token-response"></a>Risk azaltma için "Belirteci yanıtından eksik"
Tercih edilen kullanıcı adı olarak "name" talep kullanan bir seçenektir. İşlem, bu konuda açıklanan [B2C doc](/azure/active-directory-b2c/active-directory-b2c-reference-policies#frequently-asked-questions) -> "dönüş talep sütununda, başarılı bir profil düzenleme deneyimi sonra uygulamanıza geri gönderilen yetkilendirme belirteçlerinde döndürülmesini istediğiniz talepleri seçin. Örneğin, görünen ad, posta kodu seçin."

## <a name="next-steps"></a>Sonraki adımlar 

Aşağıdaki örnekte Azure AD B2C uygulamaları için MSAL.NET ile etkileşimli olarak belirteçlerini almak hakkında daha fazla ayrıntı sağlanır.

| Örnek | Platform | Açıklama|
|------ | -------- | -----------|
|[Active-directory-b2c-xamarin-yerel](https://github.com/Azure-Samples/active-directory-b2c-xamarin-native) | Xamarin iOS, Xamarin Android, UWP | MSAL.NET Azure AD B2C ile kullanıcıların kimliğini doğrulama ve sonuç belirteçleriyle bir Web API'sine erişmek için nasıl kullanılacağını gösteren basit Xamarin.Forms uygulaması.|
