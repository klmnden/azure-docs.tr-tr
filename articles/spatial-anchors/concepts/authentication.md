---
title: Kimlik doğrulama ve yetkilendirme Azure uzamsal yer işaretlerine giden | Microsoft Docs
description: Bir uygulama veya hizmet kimlik doğrulaması Azure uzamsal yer işaretleri ve Azure uzamsal bağlayıcılarını kapı erişimi olan denetim düzeyleri için çeşitli yollar hakkında bilgi edinin.
author: julianparismorgan
manager: vriveras
services: azure-spatial-anchors
ms.author: pmorgan
ms.date: 05/28/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: c7ffa432c9311ba9d4ecf4ba82c375e2dad988d0
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67478539"
---
# <a name="authentication-and-authorization-to-azure-spatial-anchors"></a>Kimlik doğrulama ve yetkilendirme Azure uzamsal yer işaretlerine giden

Bu bölümde, uzamsal yer işaretleri hesaplara erişimi denetlemek için biz çeşitli yollarla Azure uygulamanızı veya web hizmetinden uzamsal yer işaretlerine giden kimlik doğrulaması yapabilir ve rol tabanlı erişim denetimi Azure Directory (Azure AD) olarak kullanma yolları kapsar.  

## <a name="overview"></a>Genel Bakış

![Azure uzamsal yer işaretlerine giden kimlik doğrulamasına genel bakış](./media/spatial-anchors-authentication-overview.png)

Belirli bir Azure uzamsal bağlayıcılarını hesabına erişmek için öncelikle Azure karma gerçeklik güvenlik belirteci hizmeti (STS) bir erişim belirteci almak istemcileri gerekir. STS'den elde edilen belirteçleri için 24 canlı ve hesapta yetkilendirme kararları ve bu hesaba yalnızca yetkili sorumluları erişebildiğinden emin olmak uzamsal bağlayıcılarını hizmetler için bilgileri içerir. 

Erişim belirteçleri exchange ya da hesabı anahtarları veya Azure AD tarafından verilen belirteçleri elde edilebilir. 

Hesap anahtarlarını uzamsal bağlayıcılarını Azure hizmeti kullanarak hızla çalışmaya başlamanızı etkinleştirme; Uygulamanızı üretim ortamında dağıtmadan önce ancak bu, Azure AD tabanlı kimlik doğrulaması kullanmak için uygulamanızı güncelleştirmeniz önerilir. 

Azure AD kimlik doğrulama belirteçlerinizi iki yolla elde edilebilir:

- Kuruluş uygulaması oluşturuyorsunuz ve şirketiniz kendi kimlik sistemi Azure AD'yi kullanarak, kullanıcı tabanlı Azure kullanabilir AD kimlik doğrulamasını uygulama ve uzamsal bağlayıcılarını hesaplarınızı, mevcut Azure AD güvenlik gruplarını kullanma izni verin veya Kuruluşunuzdaki doğrudan kullanıcılar için. 
- Aksi takdirde, uygulamanızı destekleyen bir web hizmetinden Azure AD belirteçleri elde etmek önerilir. İstemci uygulamanızı Azure uzamsal bağlayıcılarını erişim için kimlik bilgilerini katıştırma önler destekleyen bir web hizmetini kullanarak kimlik doğrulaması önerilen yöntem, üretim uygulamaları için aynıdır. 

## <a name="account-keys"></a>Hesabı anahtarları

Uzamsal bağlayıcılarını Azure hesabınıza erişim için hesap anahtarlarınızı kullanarak başlamak için en basit yoludur. Hesap anahtarlarınızı Azure portalında bulabilirsiniz. Hesabınıza gidin ve "Anahtarlar" sekmesini seçin.

![Azure uzamsal yer işaretlerine giden kimlik doğrulamasına genel bakış](../../../includes/media/spatial-anchors-get-started-create-resource/view-account-key.png)


İki anahtar kullanılabilir, her ikisi de olduğu uzamsal bağlayıcılarını hesabına erişim için aynı anda geçerli hale getirilir. Hesaba erişmek için kullandığınız anahtarı düzenli olarak güncelleştirmeniz önerilir; iki gibi kapalı kalma süresi olmadan güncelleştirmeleri geçerli anahtarlarının etkinleştir ayırmak zorunda; Alternatif olarak, birincil anahtar ve ikincil anahtar güncelleştirmek yeterlidir. 

SDK'LARINI hesabı anahtarları ile kimlik doğrulaması için yerleşik destek içerir; yalnızca cloudSession nesnenize AccountKey özelliği ayarlamanız gerekir. 

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```csharp
this.cloudSession.Configuration.AccountKey = @"MyAccountKey";
```

# <a name="objctabobjc"></a>[ObjC](#tab/objc)

```objc
_cloudSession.configuration.accountKey = @"MyAccountKey";
```

# <a name="swifttabswift"></a>[Swift](#tab/swift)

```swift
_cloudSession!.configuration.accountKey = "MyAccountKey"
```

# <a name="javatabjava"></a>[Java](#tab/java)

```java
mCloudSession.getConfiguration().setAccountKey("MyAccountKey");
```

# <a name="c-ndktabcpp"></a>[C++ NDK](#tab/cpp)

```cpp
auto configuration = cloudSession_->Configuration();
configuration->AccountKey(R"(MyAccountKey)");
```

# <a name="c-winrttabcppwinrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
auto configuration = m_cloudSession.Configuration();
configuration.AccountKey(LR"(MyAccountKey)");
```

***

Bu yapıldıktan sonra SDK'sı alışverişi hesap anahtarı için bir erişim belirteci ve gerekli uygulama belirteçlerini önbelleğe almayı işler. 

> [!WARNING] 
> Hesap anahtarlarını kolaylaşmasına hızlı, ancak geliştirme/prototipleme yalnızca sırasında önerilir. Önemle tavsiye edilir uygulamanızı üretim ortamında bir katıştırılmış hesap anahtarı kullanmadan sevk değil ve bunun yerine kullanıcı veya hizmet tabanlı Azure'ı kullanmak için AD kimlik doğrulaması listelenen sonraki yaklaşıyor.

## <a name="azure-ad-user-authentication"></a>Azure AD kullanıcı kimlik doğrulaması

Azure Active Directory kullanıcılarını hedefleyen uygulamalar için önerilen Azure AD belirteçlerini ADAL kitaplığını aşağıdaki belgelerinde açıklanan şekilde kullanarak elde edebileceğiniz kullanıcının yaklaşımdır: [ https://docs.microsoft.com/azure/active-directory/develop/v1-overview ](../../active-directory/develop/v1-overview.md); içeren "Hızlı başlıyor altında", listelenen adımları izlemelidir:

1. Azure portalında yapılandırma
    1.  Uygulamanızı Azure AD'ye kaydetmeniz **yerel uygulama**. Kayıt bir parçası olarak uygulamanız veya çok kiracılı ve URL'leri, uygulamanız için izin verilen yeniden yönlendirme sağlamak olup olmadığını belirlemek gerekir.  
    2.  Uygulama veya kullanıcıların erişimi kaynağınıza verin: 
        1.  Azure portalında uzamsal bağlayıcılarını kaynağınıza gidin
        2.  Geçiş **erişim denetimi (IAM)** sekmesi
        3.  İsabet **rol ataması Ekle**
            1.  [Bir rol seçin](#role-based-access-control)
            2.  İçinde **seçin** kullanıcıları, grupları ve/veya erişim atamak istediğiniz uygulamaları adını girin. 
            3.  İsabet **Kaydet**.
2. Kodunuz:
    1.  Kullandığınızdan emin olun **uygulama kimliği** ve **yeniden yönlendirme URI'si** kendi Azure AD uygulamasının **istemci kimliği** ve **RedirectUri** ADAL parametreleri
    2.  Kiracı bilgileri ayarlayın:
        1.  Uygulamanız destekliyorsa **Kuruluşum yalnızca**, bu değeri ile değiştirin, **Kiracı kimliği** veya **Kiracı adı** (örneğin, contoso.microsoft.com)
        2.  Uygulamanız destekliyorsa **herhangi bir kuruluş dizini hesaplarında**, bu değeri ile değiştirin **kuruluşlar**
        3.  Uygulamanız destekliyorsa **tüm Microsoft hesabı kullanıcılarını**, bu değeri ile değiştirin **ortak**
    3.  Belirteç, istek üzerinde ayarlanan **kaynak** için "https://sts.mixedreality.azure.com". Bu "kaynak" Azure AD'ye uygulamanızı uzamsal bağlayıcılarını Azure hizmeti için bir belirteç isteyen olduğunu gösterir.  

Uygulamanızı Azure AD belirteçlerini İOS'tan elde olmalıdır; Azure AD belirtecini olarak ayarlayabilirsiniz **authenticationToken** bulut oturum yapılandırma nesneniz üzerinde. 

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```csharp
this.cloudSession.Configuration.AuthenticationToken = @"MyAuthenticationToken";
```

# <a name="objctabobjc"></a>[ObjC](#tab/objc)

```objc
_cloudSession.configuration.authenticationToken = @"MyAuthenticationToken";
```

# <a name="swifttabswift"></a>[Swift](#tab/swift)

```swift
_cloudSession!.configuration.authenticationToken = "MyAuthenticationToken"
```

# <a name="javatabjava"></a>[Java](#tab/java)

```java
mCloudSession.getConfiguration().setAuthenticationToken("MyAuthenticationToken");
```

# <a name="c-ndktabcpp"></a>[C++ NDK](#tab/cpp)

```cpp
auto configuration = cloudSession_->Configuration();
configuration->AuthenticationToken(R"(MyAuthenticationToken)");
```

# <a name="c-winrttabcppwinrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
auto configuration = m_cloudSession.Configuration();
configuration.AuthenticationToken(LR"(MyAuthenticationToken)");
```

***

## <a name="azure-ad-service-authentication"></a>Azure AD hizmet kimlik doğrulaması

Üretim için Azure uzamsal bağlayıcılarını yararlanarak uygulamaları dağıtmak için kimlik doğrulama isteklerini aracı bir arka uç hizmeti yararlanmak için önerilir. Genel bir düzeni, bu diyagramda açıklandığı gibi olmalıdır:

![Azure uzamsal yer işaretlerine giden kimlik doğrulamasına genel bakış](./media/spatial-anchors-aad-authentication.png)

Burada, uygulamanızı kendi mekanizması kullandığı varsayılır (örneğin: Microsoft hesabı, PlayFab, Facebook, Google kimliği, özel kullanıcı adı/parola, vs.) kendi arka uç hizmetinde kimlik doğrulaması için. Kullanıcılarınızın hizmet alabileceğiniz arka uç hizmetinize doğrulandıktan sonra Azure AD belirteçlerini, Azure için uzamsal yer işaretleri için bir erişim belirteci exchange ve istemci uygulamanıza geri dönün.

Aşağıdaki belgelerinde açıklanan şekilde ADAL kitaplığını kullanarak Azure AD erişim belirteci alınır: [ https://docs.microsoft.com/azure/active-directory/develop/v1-overview ](../../active-directory/develop/v1-overview.md); içeren "Hızlı başlıyor altında", listelenen adımları izlemelidir:

1.  Azure portalında yapılandırma:
    1.  Uygulamanızı Azure AD'ye kaydetme:
        1.  Azure portalında gidin **Azure Active Directory**seçip **uygulama kayıtları**
        2.  Seçin **yeni uygulama kaydı**
        3.  Seçim, uygulamanızın adını girin **Web uygulaması / API** uygulama türü olarak hizmetiniz için kimlik doğrulama URL'sini girin. Ardından isabet **Oluştur**.
        4.  Bu uygulama üzerinde isabet **ayarları**, ardından **anahtarları** sekmesi. Anahtarınızı adını girin, bir süre seçin ve isabet **Kaydet**. Web hizmetinizin kodda içerecek şekilde gerekeceğinden, o anda görüntülenen anahtar değeri kaydettiğinizden emin olun.
    2.  Uygulama ve/veya kullanıcıların erişimi kaynağınıza verin:
        1.  Azure portalında uzamsal bağlayıcılarını kaynağınıza gidin
        2.  Geçiş **erişim denetimi (IAM)** sekmesi
        3.  İsabet **rol ataması Ekle**
        1.  [Bir rol seçin](#role-based-access-control)
        2.  İçinde **seçin** alan, oluşturduğunuz uygulama adını girin ve erişim atamak istediğiniz. Uygulamanızın uzamsal bağlayıcılarını hesabına karşı farklı rollere sahip kullanıcılara istiyorsanız, birden çok uygulama Azure AD'ye kaydetme ve her birine ayrı rol atama gerekir. Ardından, kullanıcılarınız için uygun rolü kullanmak için yetkilendirme mantığının uygulayın.  
    3.  İsabet **Kaydet**.
2.  Kodunuzda (Not: Github'da bulunan hizmet örneği kullanabilirsiniz):
    1.  Uygulama gizli anahtarı, uygulama kimliği kullanmaya dikkat edin ve yeniden yönlendirme URI'si kendi Azure AD uygulamasının istemci kimliği olarak gizli ve ADAL RedirectUri parametreleri
    2.  Kendi ADAL yetkilisi parametresinde AAAzure ekleme Kiracı kimliği için Kiracı kimliği ayarlayın
    3.  Belirteç, istek üzerinde ayarlanan **kaynak** için "https://sts.mixedreality.azure.com" 

Arka uç hizmetinize bir Azure AD belirteç alabilir. Bunun ardından, istemciye döndürür bir MR belirteci için değiştirebilir. MR belirteci almak için bir Azure AD belirtecini kullanarak yapılır REST çağrısı aracılığıyla. Bir örnek çağrısı şu şekildedir:

```
GET https://mrc-auth-prod.trafficmanager.net/Accounts/35d830cb-f062-4062-9792-d6316039df56/token HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni<truncated>FL8Hq5aaOqZQnJr1koaQ
Host: mrc-auth-prod.trafficmanager.net
Connection: Keep-Alive

HTTP/1.1 200 OK
Date: Sun, 24 Feb 2019 08:00:00 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 1153
Accept: application/json
MS-CV: 05JLqWeKFkWpbdY944yl7A.0
{"AccessToken":"eyJhbGciOiJSUzI1NiIsImtpZCI6IjI2MzYyMTk5ZTI2NjQxOGU4ZjE3MThlM2IyMThjZTIxIiwidHlwIjoiSldUIn0.eyJqdGkiOiJmMGFiNWIyMy0wMmUxLTQ1MTQtOWEzNC0xNzkzMTA1NTc4NzAiLCJjYWkiOiIzNWQ4MzBjYi1mMDYyLTQwNjItOTc5Mi1kNjMxNjAzOWRmNTYiLCJ0aWQiOiIwMDAwMDAwMC0wMDAwLTAwMDAtMDAwMC0wMDAwMDAwMDAwMDAiLCJhaWQiOiIzNWQ4MzBjYi1mMDYyLTQwNjItOTc5Mi1kNjMxNjAzOWRmNTYiLCJhYW8iOi0xLCJhcHIiOiJlYXN0dXMyIiwicmlkIjoiL3N1YnNjcmlwdGlvbnMvNzIzOTdlN2EtNzA4NC00ODJhLTg3MzktNjM5Y2RmNTMxNTI0L3Jlc291cmNlR3JvdXBzL3NhbXBsZV9yZXNvdXJjZV9ncm91cC9wcm92aWRlcnMvTWljcm9zb2Z0Lk1peGVkUmVhbGl0eS9TcGF0aWFsQW5jaG9yc0FjY291bnRzL2RlbW9fYWNjb3VudCIsIm5iZiI6MTU0NDU0NzkwMywiZXhwIjoxNTQ0NjM0MzAzLCJpYXQiOjE1NDQ1NDc5MDMsImlzcyI6Imh0dHBzOi8vbXJjLWF1dGgtcHJvZC50cmFmZmljbWFuYWdlci5uZXQvIiwiYXVkIjoiaHR0cHM6Ly9tcmMtYW5jaG9yLXByb2QudHJhZmZpY21hbmFnZXIubmV0LyJ9.BFdyCX9UJj0i4W3OudmNUiuaGgVrlPasNM-5VqXdNAExD8acFJnHdvSf6uLiVvPiQwY1atYyPbOnLYhEbIcxNX-YAfZ-xyxCKYb3g_dbxU2w8nX3zDz_X3XqLL8Uha-rkapKbnNgxq4GjM-EBMCill2Svluf9crDmO-SmJbxqIaWzLmlUufQMWg_r8JG7RLseK6ntUDRyDgkF4ex515l2RWqQx7cw874raKgUO4qlx0cpBAB8cRtGHC-3fA7rZPM7UQQpm-BC3suXqRgROTzrKqfn_g-qTW4jAKBIXYG7iDefV2rGMRgem06YH_bDnpkgUa1UgJRRTckkBuLkO2FvA"}
```

Burada yetkilendirme üst bilgisi gibi biçimlendirilir: `Bearer <accoundId>:<accountKey>`

Ve düz metin MR belirteç yanıtı içerir.
 
MR belirtecini ardından istemciye döndürülür. Ardından istemci uygulamanızı bulut oturumu yapılandırması, erişim belirtecinde olarak ayarlayabilirsiniz.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```csharp
this.cloudSession.Configuration.AccessToken = @"MyAccessToken";
```

# <a name="objctabobjc"></a>[ObjC](#tab/objc)

```objc
_cloudSession.configuration.accessToken = @"MyAccessToken";
```

# <a name="swifttabswift"></a>[Swift](#tab/swift)

```swift
_cloudSession!.configuration.accessToken = "MyAccessToken"
```

# <a name="javatabjava"></a>[Java](#tab/java)

```java
mCloudSession.getConfiguration().setAccessToken("MyAccessToken");
```

# <a name="c-ndktabcpp"></a>[C++ NDK](#tab/cpp)

```cpp
auto configuration = cloudSession_->Configuration();
configuration->AccessToken(R"(MyAccessToken)");
```

# <a name="c-winrttabcppwinrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
auto configuration = m_cloudSession.Configuration();
configuration.AccessToken(LR"(MyAccessToken)");
```

***

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Denetlemenize yardımcı olmak için erişim düzeyini verilen uygulamaları, hizmetleri veya hizmetinizin, Azure AD kullanıcılarının aşağıdaki rolleri Azure uzamsal bağlayıcılarını hesaplarınızı karşı gerektiği şekilde atamanızı oluşturulmuş:

- **Uzamsal bağlayıcılarını hesap sahibi**: uygulamalar veya bu role sahip kullanıcılar uzamsal yer işaretleri oluşturmanız, bunlar için sorgulama ve bunları silin. Hesabınıza hesap anahtarlarınızı kullanarak kimlik doğrulaması sırasında **uzamsal bağlayıcılarını hesap sahibi** rolü için kimliği doğrulanmış sorumluyu atanır. 
- **Uzamsal bağlayıcılarını hesabı Katılımcısı**: uygulamalar veya bu role sahip kullanıcılar, uzamsal yer işaretleri, onlar için sorgu oluşturabilir ancak bunları silinemiyor. 
- **Uzamsal bağlayıcılarını hesabı okuyucusu**: uygulamalar veya bu role sahip kullanıcılar tarafından yalnızca uzamsal yer işaretleri için sorgu ancak yenilerini oluşturamaz, var olanları Sil veya uzamsal yer işaretleri meta verilerini güncelleştirme. Diğerleri yalnızca daha önce o ortamda yerleştirilen bağlantıları geri çağırma sırasında bazı kullanıcılar ortamı burada seçki bu uygulamalar için genellikle kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

Azure uzamsal Çıpasıyla ilk uygulamanızı oluşturun.

> [!div class="nextstepaction"]
> [Unity](../unity-overview.yml)

> [!div class="nextstepaction"]
> [iOS](../quickstarts/get-started-ios.md)

> [!div class="nextstepaction"]
> [Android](../quickstarts/get-started-android.md)

> [!div class="nextstepaction"]
> [HoloLens](../quickstarts/get-started-hololens.md)
