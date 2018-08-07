---
title: Azure Active Directory v2.0 kapsamlar, izinler ve onay | Microsoft Docs
description: Kapsamlar, izinler ve onay dahil olmak üzere Azure AD v2.0 uç noktası, yetkilendirme açıklaması.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 8f98cbf0-a71d-4e34-babf-e644ad9ff423
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: celested
ms.reviewer: hirsin, dastrock
ms.custom: aaddev
ms.openlocfilehash: b38d90251ab59e537e7d637f45f04c4db87a94ae
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39581999"
---
# <a name="scopes-permissions-and-consent-in-the-azure-active-directory-v20-endpoint"></a>Kapsamlar, izinler ve onay Azure Active Directory v2.0 uç noktası
Azure Active Directory (Azure AD) ile tümleştiren uygulamalar, uygulama verilerini nasıl erişebileceğiniz bir denetime kullanıcılara bir yetkilendirme modelini izler. Yetkilendirme modeli v2.0 uygulamasını güncelleştirildi ve bir uygulamayı Azure AD ile nasıl etkileşimde olması değiştirir. Bu makale, kapsamlar, izinler ve onay dahil olmak üzere bu yetkilendirme modeli temel kavramları kapsar.

> [!NOTE]
> V2.0 uç noktası, tüm Azure Active Directory senaryolarını ve özelliklerini desteklemez. V2.0 uç noktası kullanması gerekip gerekmediğini belirlemek için aşağıdaki hakkında bilgi edinin: [v2.0 sınırlamaları](active-directory-v2-limitations.md).
>
>

## <a name="scopes-and-permissions"></a>Kapsamlar ve izinleri
Azure AD uygular [OAuth 2.0](active-directory-v2-protocols.md) Yetkilendirme Protokolü. OAuth 2.0, üçüncü taraf bir uygulama bir kullanıcı adına web barındırılan kaynaklara erişebilir bir yöntemdir. Azure AD ile tümleşen bir web barındırılan kaynak bir kaynak tanımlayıcısı olup veya *uygulama kimliği URI'si*. Örneğin, Microsoft'un web bulunan kaynakların bazıları şunlardır:

* Office 365 posta API birleşik: `https://outlook.office.com`
* Azure AD Graph API'si: `https://graph.windows.net`
* Microsoft Graph: `https://graph.microsoft.com`

Aynı Azure AD ile tümleşik herhangi bir üçüncü taraf kaynaklar için geçerlidir. Şu kaynaklara, bu kaynağın işlevleri daha küçük öbeklere ayırmak için kullanılan bir izin kümesi de tanımlayabilirsiniz. Örneğin, [Microsoft Graph](https://graph.microsoft.io) Diğerlerinin yanında aşağıdaki görevleri gerçekleştirmek için izinler tanımlanır:

* Kullanıcının Takvim okuyun
* Kullanıcının takvim için yazma
* Bir kullanıcı olarak posta gönderme

Bu tür izinler tanımlayarak, kaynak verileri ve verilerin nasıl sunulduğunu üzerinde ayrıntılı denetime sahiptir. Bir üçüncü taraf uygulama bu izinleri bir uygulama kullanıcıdan isteyebilir. Uygulamanın kullanıcı adına hareket edebilecekleri önce izinleri uygulama kullanıcı tarafından onaylanması gerekir. Daha küçük izin kümeleri kaynağın işlevsellik Öbekleme tarafından üçüncü taraf uygulamaları işlevleri gerçekleştirmek için ihtiyaç duydukları belirli izinleri istemek için oluşturulabilir. Uygulama kullanıcıları tam olarak bir uygulama verilerini nasıl kullanacağını bilmeniz ve uygulama ile kötü amaçlı davranmıyorsa doğrulayabilirse olabilir.

Azure AD'de ve OAuth, bu tür izinler çağrılır *kapsamları*. Bunlar bazen denir *oAuth2Permissions*. Bir kapsam Azure AD'de bir dize değeri temsil edilir. Microsoft Graph örneğiyle devam, her izin için kapsam değerdir:

* Kullanıcının takvim kullanarak okuma `Calendars.Read`
* Kullanarak bir kullanıcının takvime yazma `Calendars.ReadWrite`
* Kullanarak bir kullanıcı tarafından olarak posta gönderme `Mail.Send`

Uygulama, v2.0 uç noktasına istek kapsamları belirterek bu izinler isteyebilir.

## <a name="openid-connect-scopes"></a>Openıd Connect kapsamları
Openıd Connect v2.0 uygulaması belirli bir kaynak için geçerli olmayan birkaç iyi tanımlanmış kapsamına sahiptir: `openid`, `email`, `profile`, ve `offline_access`.

### <a name="openid"></a>openıd
Uygulama oturum açma kullanarak gerçekleştiriyorsa [Openıd Connect](active-directory-v2-protocols.md), isteği göndermelidir `openid` kapsam. `openid` Kapsamı, iş hesabı onay sayfası "oturum açtığınızda" iznini ve kişisel Microsoft hesabı onay sayfası "Profilinizi görüntüleyin ve uygulamaları ve Microsoft hesabınızı kullanarak hizmetlere bağlanma" iznini gösterir. Bu izne sahip bir kullanıcı için benzersiz bir tanımlayıcı biçiminde alabilir `sub` talep. Ayrıca uygulama erişimi ve UserInfo uç noktasına sağlar. `openid` Kapsamı farklı bir uygulama bileşenleri arasında HTTP çağrıları güvenliğini sağlamak için kullanılan kimlik belirteçlerini almak için v2.0 belirteç uç noktası kullanılabilir.

### <a name="email"></a>e-posta
`email` Kapsamı ile kullanılabilir `openid` kapsamı ve diğerleri. Uygulama erişimi için kullanıcının birincil e-posta adresi biçiminde sağlar `email` talep. `email` Yalnızca bir e-posta adresi her zaman çalışması değil kullanıcı hesabıyla ilişkiliyse talep bir belirteç içine eklenir. Kullanılıyorsa `email` kapsamı, uygulamanız hazırlanmış bir durumu işlemek için `email` talep belirteci yok.

### <a name="profile"></a>Profili
`profile` Kapsamı ile kullanılabilir `openid` kapsamı ve diğerleri. Kullanıcı hakkındaki bilgileri önemli miktarda uygulama erişim sağlar. Uygulamaya erişebildiğinizden bilgiler içerir, ancak kullanıcının verilen adı, Soyadı, tercih edilen kullanıcı adı ve nesne kimliği için sınırlı değildir Belirli bir kullanıcı için id_tokens parametresinde kullanılabilir profili talepleri tam bir listesi için bkz. [v2.0 belirteç başvurusu](v2-id-and-access-tokens.md).

### <a name="offlineaccess"></a>offline_access
[ `offline_access` Kapsam](http://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) erişim kaynaklara kullanıcı adına uzun bir süre sağlar. İş hesabı onay sayfasında, bu kapsamı "verilerinizi dilediğiniz zaman erişim" izni görünür. Kişisel Microsoft hesabı onay sayfasında, "bilgilerinize dilediğiniz zaman erişim" izni görünür. Bir kullanıcının ne zaman onaylar `offline_access` kapsamı, uygulama v2.0 belirteç uç noktasından yenileme belirteçleri alabilir. Uzun süreli yenileme belirteçleri. Uygulamanızı, eski görüntülerin süresi dolduğundan yeni erişim belirteçleri elde edebilirsiniz.

Uygulamanızı değil istemiyorsa `offline_access` kapsamı, yenileme belirteçleri almazsınız. Bunun anlamı bir yetkilendirme kodunda şifrenizi kullandığınızda [OAuth 2.0 yetkilendirme kod akışı](active-directory-v2-protocols.md), yalnızca bir erişim belirteci alırsınız `/token` uç noktası. Erişim belirteci, kısa bir süre için geçerlidir. Erişim belirteci, genellikle bir saat içinde süresi dolar. Noktası, kullanıcı yeniden yönlendirmek uygulamanız gereken en başa `/authorize` yeni bir yetkilendirme kodunu almak için uç nokta. Uygulama türünü bağlı olarak bu yeniden yönlendirme sırasında kullanıcı kimlik bilgilerini yeniden girin veya izinleri yeniden onay gerekebilir.

Alma ve yenileme belirteçleri kullanma hakkında daha fazla bilgi için bkz. [v2.0 protokol başvurusu](active-directory-v2-protocols.md).

## <a name="requesting-individual-user-consent"></a>Bireysel kullanıcı onay isteme
İçinde bir [Openıd Connect veya OAuth 2.0](active-directory-v2-protocols.md) yetkilendirme isteği, bir uygulamayı, ihtiyaç duyduğu kullanarak izinler isteyebilir `scope` sorgu parametresi. Örneğin, bir kullanıcı bir uygulama, uygulama gönderen oturum açtığında bir istek aşağıdaki örnekteki gibi (ile Okunaklılık için eklenen satır sonları):

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=
https%3A%2F%2Fgraph.microsoft.com%2Fcalendars.read%20
https%3A%2F%2Fgraph.microsoft.com%2Fmail.send
&state=12345
```

`scope` Parametredir uygulamayı isteyen kapsam boşlukla ayrılmış bir listesi. Her kapsam, kaynak tanımlayıcısı (uygulama kimliği URI'si) için kapsam değeri ekleyerek gösterilir. İstek örnekte, uygulamanın kullanıcının Takvim okuyun ve kullanıcı olarak posta gönderme izni gerekir.

Kullanıcı kimlik bilgilerini girdikten sonra v2.0 uç noktası eşleşen bir kayıt için denetler *kullanıcı onayı*. Kullanıcının istenen izinleri birine geçmişte değil etmişse, v2.0 uç noktası istenen izinleri vermek için kullanıcıya sorar.

![İş hesabı onayı](./media/v2-permissions-and-consent/work_account_consent.png)

Kullanıcının izni onayladığında, böylece kullanıcının yeniden sonraki hesap oturum açma işlemleri üzerinde onay gerekmez onay kaydedilir.

## <a name="requesting-consent-for-an-entire-tenant"></a>Tüm bir kiracı için onay isteme
Genellikle, bir kuruluşun bir lisans ya da bir uygulama için bir abonelik satın aldığında kuruluş çalışanlarının uygulamayı tam olarak sağlamak istiyor. Bu işlemin bir parçası, yönetici, uygulamanın adına bir çalışan işlem izin verebilirsiniz. Yönetici, Kiracı genelinde izin verir, kuruluş çalışanları uygulama için bir onay sayfası görmeyeceksiniz.

Bir kiracıdaki tüm kullanıcılar için izin istemek için yönetici onayı uç noktası uygulamanızı kullanabilirsiniz.

## <a name="admin-restricted-scopes"></a>Admin-kısıtlı kapsamları
Bazı Microsoft ekosisteminde yüksek ayrıcalıklı izinlere ayarlanabilir *admin-kısıtlı*. Bu tür kapsamları örnekleri aşağıdaki izinler şunlardır:

* Kullanarak, bir kuruluşun dizin verilerini okuma `Directory.Read`
* Kullanarak, bir kuruluşun dizine veri yazma `Directory.ReadWrite`
* Bir kuruluşun Directory'de güvenlik gruplarını kullanarak okuma `Groups.Read.All`

Bir tüketici kullanıcı verileri bu tür bir uygulama erişimi verebilir olsa da, kurumsal kullanıcılar kümesine hassas şirket verilerinin erişim kısıtlanır. Uygulamanızı erişim bu izinleri birine bir kuruluş kullanıcıdan istemesi durumunda kullanıcı, uygulamanın izinlerini onay yetkiniz yok bildiren bir hata iletisi alır.

Uygulamanızı kuruluşlar için kapsamları admin-kısıtlı erişim gerektiriyorsa bir şirket yöneticisinden doğrudan yönetici onayı sonraki bölümde açıklandığı uç noktası, kullanarak da istemelidir.

Yönetici yönetici onay uç noktası üzerinden bu izin veriyorsa kiracıdaki tüm kullanıcılar için izin verilir.

## <a name="using-the-admin-consent-endpoint"></a>Yönetici onay uç noktası kullanma
Bu adımları izlerseniz, uygulamanızı admin-kısıtlı kapsamlar dahil olmak üzere, bir kiracıdaki tüm kullanıcılar için izinleri toplayabilirsiniz. Adımları uygulayan bir kod örnek görmek için [admin-kısıtlı kapsamları örnek](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2).

### <a name="request-the-permissions-in-the-app-registration-portal"></a>Uygulama kayıt portalında izinlere ilişkin istek
1. Uygulamanıza gidin [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), veya [uygulama oluşturma](quickstart-v2-register-an-app.md) henüz yapmadıysanız.
2. Bulun **Microsoft Graph izinleri** bölümüne ve ardından uygulamanız için gerekli izinleri ekleyin.
3. Emin olun **Kaydet** uygulama kaydı.

### <a name="recommended-sign-the-user-in-to-your-app"></a>Önerilir: kullanıcı uygulamanızda oturum
Genellikle, yönetici onayı uç noktası kullanan bir uygulama oluşturduğunuzda, uygulamanın bir sayfa ya da yönetici uygulamanın izinlerini onaylayabilir görünüm gerekir. Adanmış bir "Bağlan" akış olabilir veya bu sayfada uygulamanın kaydolma akışın, uygulamanın ayarlarının parçası parçası olabilir. Çoğu durumda, "yalnızca bir kullanıcı bir iş veya Okul hesabı Microsoft ile imzaladığı sonra bu göstermek uygulama için Görünüm Bağlan" mantıklıdır.

Kullanıcı uygulamanızda oturum zaman yönetici gerekli izinleri onaylamasını isteyen önce ait olduğu kuruluş tanımlayabilirsiniz. Kesinlikle gerekli olmasa da, daha sezgisel bir deneyim kullanıcılarınızın kuruluş oluşturmanıza yardımcı olur. Kullanıcının oturum açmasını için izleyin bizim [v2.0 protokol öğreticiler](active-directory-v2-protocols.md).

### <a name="request-the-permissions-from-a-directory-admin"></a>Bir dizin yönetici izinleri iste
Kuruluşunuzun yönetici izinleri istemek hazır olduğunuzda, kullanıcıyı v2.0 için yönlendirebilirsiniz *yönetici onay uç noktası*.

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro tip: Try pasting the below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parametre | Koşul | Açıklama |
| --- | --- | --- |
| kiracı |Gerekli |İzni istemek için istediğiniz dizinin Kiracı. GUID veya kolay adı biçiminde sağlanan veya "Genel" ile örnekte görüldüğü gibi genel olarak başvurulan. |
| client_id |Gerekli |Uygulama Kimliği [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) uygulamanıza atanan. |
| redirect_uri |Gerekli |Yeniden yönlendirme URI'si, uygulamanızı işlemek gönderilecek yanıt istediğiniz. Yeniden yönlendirme uygulama kayıt Portalı'nda kayıtlı bir URI'leri biri tam olarak eşleşmesi gerekir. |
| durum |Önerilen |Belirteç yanıtta döndürülecek isteğinde bulunan bir değer. Bu, istediğiniz herhangi bir içerik dizesi olabilir. Durum, uygulama kullanıcının durumu hakkındaki bilgileri sayfasında ya da görünümü üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce kodlamak için kullanın. |

Bu noktada, Azure AD isteği tamamlamak oturum açmak bir kiracı Yöneticisi gerektirir. Yönetici uygulama kayıt portalında uygulamanıza için istenen tüm izinleri de onaylaması istenir.

#### <a name="successful-response"></a>Başarılı yanıt
Yönetici izinleri uygulamanızın onaylarsa, başarılı yanıt şöyle görünür:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parametre | Açıklama |
| --- | --- | --- |
| kiracı |Uygulamanız, GUID biçiminde istenen izinler directory kiracısı. |
| durum |Belirteç yanıtta döndürülecek isteğinde bulunan bir değer. Bu, istediğiniz herhangi bir içerik dizesi olabilir. Durumu, uygulama kullanıcının durumu hakkındaki bilgileri sayfasında ya da görünümü üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce kodlamak için kullanılır. |
| admin_consent |Ayarlanacak **true**. |

#### <a name="error-response"></a>Hata yanıtı
Yönetici izinleri uygulamanızın onaylamaz, başarısız bir yanıt şöyle görünür:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parametre | Açıklama |
| --- | --- | --- |
| error |Oluşan hataları türlerini sınıflandırmak için kullanılabilir ve hatalara tepki vermek için kullanılan bir hata kodu dizesi. |
| error_description |Bir hata nedenini Geliştirici yardımcı olabilecek belirli bir hata iletisi. |

Yönetici onayı uç noktasından başarılı bir yanıt alındı sonra uygulamanızı, istenen izinleri kazanmıştır. Ardından, kullanmak istediğiniz kaynak için bir belirteç isteğinde bulunabilirsiniz.

## <a name="using-permissions"></a>İzinleri kullanma
Uygulamanızı, uygulamanız için izinler için kullanıcı onay sonra uygulamanızın bazı kapasite bir kaynağa erişim izni temsil eden erişim belirteçleri elde edebilirsiniz. Bir erişim belirteci, yalnızca tek bir kaynak için kullanılabilir ancak içinde erişim belirteci, uygulamanızı bu kaynak için verilmiş her izin kodlanır. Bir erişim belirteci almak için uygulamanızı istekte böyle v2.0 belirteç uç bulunabilirsiniz:

```
POST common/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/json

{
    "grant_type": "authorization_code",
    "client_id": "6731de76-14a6-49ae-97bc-6eba6914391e",
    "scope": "https://outlook.office.com/mail.read https://outlook.office.com/mail.send",
    "code": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq..."
    "redirect_uri": "https://localhost/myapp",
    "client_secret": "zc53fwe80980293klaj9823"  // NOTE: Only required for web apps
}
```

HTTP isteklerinde kaynağa erişim belirtecinin kullanabilirsiniz. Güvenilir bir şekilde, kaynağa'nin uygulamanızı belirli bir görevi gerçekleştirmek için uygun izinlere sahip olduğunu gösterir. 

OAuth 2.0 protokolünü ve erişim belirteçlerini almak nasıl hakkında daha fazla bilgi için bkz. [v2.0 uç noktası Protokolü başvurusu](active-directory-v2-protocols.md).
