---
title: "Azure Active Directory v2.0 kapsamları, izinler ve onay | Microsoft Docs"
description: "Kapsam, izinleri ve onay dahil olmak üzere Azure AD v2.0 uç yetkilendirme açıklaması."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 8f98cbf0-a71d-4e34-babf-e644ad9ff423
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 1488e8d2a70f7317c97275b83db3b9f05e9deb4b
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="scopes-permissions-and-consent-in-the-azure-active-directory-v20-endpoint"></a>Kapsam, izinleri ve Azure Active Directory v2.0 uç onay
Azure Active Directory (Azure AD) ile tümleştirme uygulamalarını kullanıcılara bir uygulama verilerini nasıl erişebileceğinizi üzerinde denetim sağlar bir yetkilendirme modelini izler. Yetkilendirme modelini v2.0 uyarlamasını güncelleştirilen ve bir uygulamayı Azure AD ile nasıl etkileşim kurmalıdır değiştirir. Bu makalede, kapsamları, izinler ve onay dahil olmak üzere bu yetkilendirme modelini temel kavramları kapsar.

> [!NOTE]
> V2.0 uç noktası, tüm Azure Active Directory senaryolarını ve özelliklerini desteklemez. V2.0 uç kullanması gerekip gerekmediğini belirlemek için okuyun [v2.0 sınırlamaları](active-directory-v2-limitations.md).
>
>

## <a name="scopes-and-permissions"></a>Kapsamlar ve izinleri
Azure AD uygulayan [OAuth 2.0](active-directory-v2-protocols.md) Yetkilendirme Protokolü. OAuth 2.0 üzerinden bir üçüncü taraf uygulama bir kullanıcı adına web barındırılan kaynaklara erişebilir bir yöntemdir. Azure AD ile tümleşir web barındırılan kaynak bir kaynak tanımlayıcısı vardır veya *uygulama kimliği URI'si*. Örneğin, Microsoft'un web barındırılan kaynaklar bazıları şunlardır:

* Office 365 posta API birleşik:`https://outlook.office.com`
* Azure AD grafik API'si:`https://graph.windows.net`
* Microsoft Graph:`https://graph.microsoft.com`

Aynı Azure AD ile tümleşik tüm üçüncü taraf kaynaklar için geçerlidir. Bu kaynaklardan herhangi birini de bu kaynak işlevselliğini daha küçük parçalara bölmek için kullanılan izinler kümesini tanımlayabilirsiniz. Örneğin, [Microsoft Graph](https://graph.microsoft.io) diğerlerinin yanı sıra aşağıdaki görevleri gerçekleştirmek için izinler tanımlanır:

* Kullanıcının Takvim okuma
* Kullanıcının Takvim yazma
* Kullanıcı olarak posta gönderme

Bu tür izin tanımlayarak, kaynak verilerini ve verilerin açığa hassas denetime sahiptir. Bir üçüncü taraf uygulaması bu izinleri bir uygulama kullanıcıdan isteyebilir. Uygulama kullanıcı, uygulamayı kullanıcının adına işlem yapabileceği önce izinleri onaylamanız gerekir. Daha küçük izin kümeleri kaynağın işlevsellik Öbekleme tarafından işlevleri gerçekleştirmek için ihtiyaç duydukları belirli izinleri istemek için üçüncü taraf uygulamaları derlenebilir. Uygulama kullanıcıların tam olarak bir uygulama verilerini nasıl kullanılacağını bilmeniz ve uygulama ile kötü amaçlı çalışmıyorsa doğrulayabilirse olabilir.

Azure AD'de ve OAuth, izinleri bu tür çağrılır *kapsamları*. Bunlar da olarak da adlandırılır *oAuth2Permissions*. Bir kapsam Azure AD'de bir dize değeri olarak temsil edilir. Microsoft Graph örnekle devam edersek, her izin için kapsam değeridir:

* Kullanıcının takvim kullanarak okuyun`Calendars.Read`
* Kullanıcının takvim kullanarak yazma`Calendars.ReadWrite`
* Kullanarak bir kullanıcı tarafından olarak posta gönderme`Mail.Send`

Bir uygulama, v2.0 uç noktasına istek kapsamlar belirterek bu izinleri isteyebilir.

## <a name="openid-connect-scopes"></a>Openıd Connect kapsamları
Openıd Connect v2.0 uyarlamasını belirli bir kaynak için geçerli olmayan birkaç iyi tanımlanmış kapsamına sahiptir: `openid`, `email`, `profile`, ve `offline_access`.

### <a name="openid"></a>openıd
Bir uygulama oturum açma kullanarak gerçekleştirirse [Openıd Connect](active-directory-v2-protocols.md), isteği göndermelidir `openid` kapsam. `openid` Kapsam, iş hesabı onay sayfasında "oturum" iznini ve kişisel Microsoft hesabı onay sayfasında "Profilinizi görüntüleyin ve uygulamaları ve Microsoft hesabınızı kullanarak hizmetlere bağlanma" iznini gösterir. Bu izne sahip bir uygulamanın kullanıcı için benzersiz bir tanımlayıcı biçiminde alabileceği `sub` talep. Ayrıca kullanıcı bilgisi uç noktasına uygulama erişimi sağlar. `openid` Kapsamı, bir uygulamanın farklı bileşenler arasındaki HTTP çağrıları güvenliğini sağlamak için kullanılan kimlik belirteçlerini edinmeye v2.0 belirteç uç noktada kullanılabilir.

### <a name="email"></a>e-posta
`email` Kapsam ile kullanılabilir `openid` kapsam ve herhangi diğer. Kullanıcının birincil e-posta adresine biçiminde uygulama erişim sağlayan `email` talep. `email` Yalnızca bir e-posta adresi her zaman bir durum yoksa kullanıcı hesabı ile ilişkili ise talep bir belirteç içine eklenmiştir. Bunu kullanıyorsa `email` kapsamı, uygulamanız hazırlanmış bir durumda işlemek için `email` talep belirteci yok.

### <a name="profile"></a>Profili
`profile` Kapsam ile kullanılabilir `openid` kapsam ve herhangi diğer. Kullanıcı hakkındaki bilgileri önemli miktarda uygulama erişim sağlar. Erişebilmesi için bilgi içerir, ancak kullanıcının verilen ad, Soyadı, tercih edilen kullanıcı adı ve nesne kimliği için sınırlı değildir Belirli bir kullanıcı için id_tokens parametresinde kullanılabilir profil talepleri tam bir listesi için bkz: [v2.0 belirteçler başvuru](active-directory-v2-tokens.md).

### <a name="offlineaccess"></a>offline_access
[ `offline_access` Kapsam](http://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) uygulama erişiminizi kaynaklara kullanıcı adına uzun bir süre sağlar. İş hesabı onay sayfasında, bu kapsamı "dilediğiniz zaman, veri erişim" izin olarak görünür. Kişisel Microsoft hesabı onay sayfasında, "bilgilerinize dilediğiniz zaman erişin" izin olarak görünür. Bir kullanıcının ne zaman onaylar `offline_access` kapsamı, uygulamanızı v2.0 belirteç uç noktasından yenileme belirteçleri alabilir. Yenileme belirteçleri uzun süreli. Eskiler süresi dolduğundan, uygulamanızın yeni erişim belirteçleri elde edebilirsiniz.

Uygulamanızı değil istemiyorsa `offline_access` kapsamı, yenileme belirteçleri almazsınız. Bir yetkilendirme kodu kullanmak olduğunda bunun anlamı [OAuth 2.0 yetkilendirme kodu akışını](active-directory-v2-protocols.md), yalnızca bir erişim belirtecinden alırsınız `/token` uç noktası. Erişim belirteci kısa bir süre için geçerli değil. Erişim belirteci, genellikle bir saat içinde süresi dolar. At noktası, kullanıcı yeniden yönlendirmek uygulamanız gereken başa `/authorize` yeni bir yetkilendirme kodu almak için uç nokta. Uygulama, türünü bağlı olarak bu yeniden yönlendirme sırasında kullanıcı kimlik bilgilerini yeniden girin veya yeniden izinleri onayı gerekebilir.

Alma ve yenileme belirteçleri kullanma hakkında daha fazla bilgi için bkz: [v2.0 protokol başvurusu](active-directory-v2-protocols.md).

## <a name="requesting-individual-user-consent"></a>Tek tek kullanıcı izni isteniyor
İçinde bir [Openıd Connect veya OAuth 2.0](active-directory-v2-protocols.md) yetkilendirme isteği, bir uygulama gerekiyor kullanarak izinleri isteyebileceği `scope` sorgu parametresi. Örneğin, bir kullanıcı bir uygulama, uygulama gönderir oturum açtığında bir istek aşağıdaki örnekteki gibi (okunabilirliği için eklenen satır sonuyla):

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

`scope` Parametresi uygulama isteyen kapsamları boşlukla ayrılmış bir listesi verilmiştir. Her kapsam, kaynak tanımlayıcısına (uygulama kimliği URI) kapsam değeri ekleyerek belirtilir. İstek örnekte, uygulama kullanıcının Takvim okumak ve kullanıcı olarak posta göndermek için izniniz gerekiyor.

Kullanıcı kimlik bilgilerini girdikten sonra v2.0 uç noktası için eşleşen bir kayıt denetler *kullanıcı izni*. Kullanıcının istenen izinleri hiçbirine geçmişte seçtiği değil, v2.0 uç istenen izinleri vermek için kullanıcıya sorar.

![İş hesabı onayı](../../media/active-directory-v2-flows/work_account_consent.png)

Kullanıcının izni onayladığında, böylece kullanıcının yeniden sonraki hesap oturum açma işlemlerini üzerinde kabul yoksa izin kaydedilir.

## <a name="requesting-consent-for-an-entire-tenant"></a>Tüm Kiracı onay isteme
Genellikle, bir kuruluş lisans ya da bir uygulama için abonelik satın aldığında kuruluş uygulama çalışanlarının için tam olarak sağlayacak istemektedir. Bu işlemin bir parçası olarak, bir yönetici uygulamanın adına bir çalışan işlem izin verebilirsiniz. Yönetici tüm Kiracı izin verirse, kuruluşun çalışanlar uygulama için bir onay sayfasına görmezsiniz.

Bir kiracıdaki tüm kullanıcılar için izni istemek için uygulamanızı yönetici onayı uç noktası kullanabilirsiniz.

## <a name="admin-restricted-scopes"></a>Yönetici kısıtlanmış kapsamları
Bazı yüksek ayrıcalıklı izinlere Microsoft ekosistemindeki ayarlanabilir *yönetici kısıtlanmış*. Bu tür kapsamları örnekleri aşağıdaki izinleri şunlardır:

* Kullanarak bir kuruluşun dizin verilerini okuma`Directory.Read`
* Kullanarak bir kuruluşun dizinine veri yazma`Directory.ReadWrite`
* Bir kuruluşun Directory'de güvenlik gruplarını kullanarak okuyun`Groups.Read.All`

Bir tüketici kullanıcı bir uygulama bu tür verilerin erişim ancak Kurumsal kullanıcılara hassas şirket verilerini aynı dizi için erişim verme kısıtlanır. Uygulamanızı erişim bu izinleri birine bir kuruluş kullanıcıdan isterse, kullanıcı, uygulamanızın izinlerini onayı yetkiniz yok bildiren bir hata iletisi alır.

Uygulamanızı kuruluşlar için yönetici kısıtlanmış kapsamlara erişim gerekiyorsa, bunları bir şirket yöneticisinden doğrudan da yönetici onayı sonraki bölümde açıklandığı uç noktası, kullanarak isteğinde.

Bir yönetici bu izinleri yönetici onayı uç noktası aracılığıyla veriyorsa kiracıdaki tüm kullanıcılar için izin verilir.

## <a name="using-the-admin-consent-endpoint"></a>Yönetici onayı uç noktası kullanma
Bu adımları izlerseniz, uygulamanızı yönetici kısıtlanmış kapsamları dahil olmak üzere, bir kiracıdaki tüm kullanıcılar için izinler toplayabilirsiniz. Adımları uygulayan bir kod örnek görmek için [yönetici kısıtlanmış kapsamları örnek](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2).

### <a name="request-the-permissions-in-the-app-registration-portal"></a>Uygulama kayıt Portalı'nda izinleri iste
1. Uygulamanızda gidin [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), veya [bir uygulama oluşturmak](active-directory-v2-app-registration.md) yapmadıysanız.
2. Bulun **Microsoft Graph izinleri** bölümünde ve uygulamanızın gerektirdiği izinleri ekleyin.
3. Emin olun **kaydetmek** uygulama kaydı.

### <a name="recommended-sign-the-user-in-to-your-app"></a>Önerilir: kullanıcı uygulamanızda oturum
Genellikle, yönetici onayı uç noktası kullanan bir uygulama oluşturduğunuzda, uygulamanın bir sayfa veya yönetici uygulamanın izinleri onaylayabilirsiniz görünüm gerekir. Bu sayfa uygulamanın kaydolma akış, uygulamanın ayarlarının bir parçası parçası olabilir veya özel bir "Bağlan" akış olabilir. Çoğu durumda, "yalnızca bir kullanıcı bir iş veya Okul Microsoft hesabı oturum imzaladığı sonra uygulamanın bunu göstermek için bağlantı görünümü" mantıklıdır.

Kullanıcının uygulamanıza oturum açtığınızda, yönetici gerekli izinleri onaylanacak yönergelerinin önce ait olduğu kuruluş tanımlayabilirsiniz. Kesinlikle gerekli olmasa da, kuruluş, kullanıcılarınızın daha sezgisel bir deneyim oluşturmanıza yardımcı olur. Kullanıcıyla oturum açmak için izleyin bizim [v2.0 protokol öğreticileri](active-directory-v2-protocols.md).

### <a name="request-the-permissions-from-a-directory-admin"></a>Bir dizin yönetici olarak izinleri iste
Kuruluşunuzun yönetici olarak izinleri istemek hazır olduğunuzda, kullanıcıyı v2.0 yönlendirebilirsiniz *yönetici onayı uç noktası*.

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
| Kiracı |Gerekli |İzni istemek için istediğiniz dizin Kiracı. GUID veya kolay ad biçiminde sağlanabilir. |
| client_id |Gerekli |Uygulama Kimliği [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) uygulamanıza atanmış. |
| redirect_uri |Gerekli |Yeniden yönlendirme işlemek uygulamanız için gönderilecek yanıt istediğiniz URI'si. Bu tam olarak yeniden yönlendirme uygulama kayıt Portalı'nda kayıtlı URI'ler biriyle eşleşmelidir. |
| durum |Önerilen |Belirteç yanıtta döndürülen de istekte bulunan bir değer. İstediğiniz herhangi bir içerik dizesi olabilir. Sayfa veya görünüm üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce uygulama kullanıcının durumu hakkında bilgi kodlanacak durumunu kullanın. |

Bu noktada, Azure AD isteği tamamlamak oturum açmak bir kiracı Yöneticisi gerektirir. Yönetici uygulama kayıt Portalı'nda, uygulamanız için istenen tüm izinleri onaylamanız istenir.

#### <a name="successful-response"></a>Başarılı yanıt
Yönetici, uygulamanızın izinlerini onaylarsa, başarılı yanıtı şuna benzer:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parametre | Açıklama |
| --- | --- | --- |
| Kiracı |Uygulamanız bu GUID biçiminde istenen izinlerin dizin Kiracı. |
| durum |Aynı zamanda isteğinde bir değer belirteci yanıt olarak döndürülür. İstediğiniz herhangi bir içerik dizesi olabilir. Durum, uygulama kullanıcının durumu hakkında bilgi sayfa veya görünüm üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce kodlamak için kullanılır. |
| admin_consent |Ayarlanacak **doğru**. |

#### <a name="error-response"></a>Hata yanıtı
Yönetici, uygulamanızın izinlerini onaylamaz, başarısız yanıtı şuna benzer:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parametre | Açıklama |
| --- | --- | --- |
| error |Oluşan hataları türlerini sınıflandırmak için kullanılan ve hataları tepki vermek için kullanılan bir hata kodu dizesi. |
| error_description |Bir geliştirici hata kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |

Yönetici onayı uç noktasından başarılı bir yanıt aldık sonra uygulamanızı istendiğinde izinleri kazanmıştır. Ardından, istediğiniz kaynağı için bir belirteç talep edebilirsiniz.

## <a name="using-permissions"></a>İzinlerini kullanma
Uygulamanız için izinleri için kullanıcı izin sonra uygulamanızı uygulamanızın bazı kapasite bir kaynağa erişim izni temsil erişim belirteçleri elde edebilirsiniz. Bir erişim belirteci, yalnızca tek bir kaynak için kullanılabilir, ancak erişim belirteci içine kodlanmış uygulamanız bu kaynak için verilen her izni yok. Bir erişim belirteci edinmek için uygulamanızı bir istek şöyle v2.0 belirteç uç yapabilirsiniz:

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

Sonuçta elde edilen erişim belirteci kaynak HTTP isteklerine kullanabilirsiniz. Güvenilir bir şekilde, kaynağa'nin uygulamanızı belirli bir görevi gerçekleştirmek için uygun izne sahip olduğunu gösterir.  

OAuth 2.0 protokolünü ve erişim belirteçleri alma hakkında daha fazla bilgi için bkz: [v2.0 uç noktası Protokolü başvurusu](active-directory-v2-protocols.md).
