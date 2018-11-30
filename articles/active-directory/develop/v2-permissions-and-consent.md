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
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: hirsin, jesakowi, justhu
ms.custom: aaddev
ms.openlocfilehash: 5283782188eaebe3997b6de31b087da74cf10486
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52620141"
---
# <a name="permissions-and-consent-in-the-azure-active-directory-v20-endpoint"></a>İzinler ve onay Azure Active Directory v2.0 uç noktası

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Microsoft kimlik platformu ile tümleşen uygulamaları, verileri nasıl erişilebileceğini kullanıcıların ve yöneticilerin denetim sağlayan bir yetkilendirme modelini izler. Yetkilendirme modelini uygulamasını v2.0 uç noktada güncelleştirildi ve uygulama Microsoft kimlik platformu ile nasıl etkileşimde olması değiştirir. Bu makale, kapsamlar, izinler ve onay dahil olmak üzere bu yetkilendirme modeli temel kavramları kapsar.

> [!NOTE]
> V2.0 uç noktası, tüm senaryolar ve Özellikler desteklemez. V2.0 uç noktası kullanması gerekip gerekmediğini belirlemek için aşağıdaki hakkında bilgi edinin: [v2.0 sınırlamaları](active-directory-v2-limitations.md).

## <a name="scopes-and-permissions"></a>Kapsamlar ve izinleri

Microsoft kimlik platformu uygular [OAuth 2.0](active-directory-v2-protocols.md) Yetkilendirme Protokolü. OAuth 2.0, üçüncü taraf bir uygulama bir kullanıcı adına web barındırılan kaynaklara erişebilir bir yöntemdir. Microsoft kimlik platformu ile tümleşen bir web barındırılan kaynak bir kaynak tanımlayıcısı olup veya *uygulama kimliği URI'si*. Örneğin, Microsoft'un web bulunan kaynakların bazıları şunlardır:

* Microsoft Graph: `https://graph.microsoft.com`
* Office 365 posta API: `https://outlook.office.com`
* Azure AD Graph: `https://graph.windows.net`

> [!NOTE]
> Microsoft Graph kullanmanız Azure AD Graph, Office 365 posta API, vb. yerine öneririz.

Aynı durum Microsoft kimlik platformu ile tümleştirdik herhangi bir üçüncü taraf kaynaklar için geçerlidir. Şu kaynaklara, bu kaynağın işlevleri daha küçük öbeklere ayırmak için kullanılan bir izin kümesi de tanımlayabilirsiniz. Örneğin, [Microsoft Graph](https://graph.microsoft.com) Diğerlerinin yanında aşağıdaki görevleri gerçekleştirmek için izinler tanımlanır:

* Kullanıcının Takvim okuyun
* Kullanıcının takvim için yazma
* Kullanıcı olarak posta gönderme

Bu tür izinler tanımlayarak kaynak verilerini ve API işlevselliğini nasıl sunulduğunu üzerinde ayrıntılı denetime sahiptir. Bir üçüncü taraf uygulaması kullanıcılar ve Yöneticiler, bu izinler isteyebilir kimin önce uygulama isteğini onaylaması veri erişebilir veya bir kullanıcı adına hareket. Daha küçük izin kümeleri kaynağın işlevsellik Öbekleme tarafından üçüncü taraf uygulamaları işlevleri gerçekleştirmek için ihtiyaç duydukları belirli izinleri istemek için oluşturulabilir. Tam olarak hangi verilerin uygulama erişimi olan kullanıcıların ve yöneticilerin bilebilirsiniz ve kötü amaçlı bir eyleme çalışmıyorsa doğrulayabilirse olabilir. Geliştiriciler için yalnızca işlev uygulamalarına ihtiyaç duydukları izinleri isteyen en az ayrıcalık kavramı tarafından her zaman uymanız.

OAuth bu tür izinler adlandırılır *kapsamları*. Bunlar çoğunlukla kısaca olarak adlandırılan *izinleri*. Bir izni Microsoft kimlik platformu, dize değeri olarak temsil edilir. Microsoft Graph örneğiyle devam, her izin için dize değeridir:

* Kullanıcının takvim kullanarak okuma `Calendars.Read`
* Kullanarak bir kullanıcının takvime yazma `Calendars.ReadWrite`
* Kullanarak bir kullanıcı tarafından olarak posta gönderme `Mail.Send`

Uygulama, v2.0 isteklerinde kapsamları belirterek bu izinleri son noktanın yetkilendirilmesi en yaygın olarak ister. Ancak, bazı yüksek ayrıcalıklı izinlere yalnızca yönetici onayı ve genellikle istenen/verilen kullanılarak verilebilir [yönetici onay uç noktası](v2-permissions-and-consent.md#admin-restricted-scopes). Daha fazla bilgi için okumaya devam edin.

## <a name="permission-types"></a>İzin türleri

Microsoft kimlik platformu destekleyen iki tür izinler: **temsilci izinleri** ve **uygulama izinleri**.

* **Temsilci izinleri** oturum açmış kullanıcı var olan uygulamaları tarafından kullanılır. Bu uygulamalar için kullanıcı veya yönetici izinleri uygulama istekleri ve uygulama olduğunu temsilci atanmış izin, hedef kaynağın çağrı yaparken oturum açmış kullanıcı olarak davranacak şekilde toplanmasına onay verir. Bazı izinleri devralmış yönetici olmayan kullanıcılar tarafından onay, ancak bazı daha yüksek ayrıcalıklı izinlere gerektiren [yönetici onayı](v2-permissions-and-consent.md#admin-restricted-scopes). Hangi yönetici rolleri için temsilci izinleri izin alma bilgi edinmek için [Azure AD'de Yönetici rolü izinleri](../users-groups-roles/directory-assign-admin-roles.md).

* **Uygulama izinleri** uygulamaları tarafından kullanılan bir oturum açmış kullanıcı mevcut çalıştırın; Örneğin, uygulamaları çalıştıran arka plan Hizmetleri veya daemon'ları olarak.  Uygulama izinleri yalnızca olabilir [bir yönetici tarafından onaylı](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant).

_Geçerli İzinler_ uygulamanızı istekleri hedef kaynağı yaparken sahip olacağı izinler. Hedef kaynak çağrı yaparken yetkilendirilen ve uygulamanızı verilen uygulama izinleri ve etkili izinleri arasındaki farkı anlamak önemlidir.

- Temsilci izinleri için _etkili izinleri_ uygulamanızı app (izni) verilmiş temsilci izinleri en az ayrıcalıklı kesişimi ve şu anda oturum açmış kullanıcının ayrıcalıkları olacaktır. Uygulamanızın ayrıcalıkları hiçbir zaman oturum açmış kullanıcının ayrıcalıklarından fazla olamaz. Kuruluşların içinde, oturum açmış kullanıcının ayrıcalıkları ilkeyle ya da bir veya birden çok yönetici rolü üyeliğiyle belirlenebilir. Hangi yönetici rolleri için temsilci izinleri izin alma bilgi edinmek için [Azure AD'de Yönetici rolü izinleri](../users-groups-roles/directory-assign-admin-roles.md).
  Örneğin, uygulamanızın verilen varsayın _User.ReadWrite.All_ izin temsilci. Adından da anlaşıldığı gibi bu izin uygulamanıza kuruluştaki her kullanıcının profilini okuma ve güncelleştirme izni verir. Oturum açmış kullanıcının bir genel yönetici olması durumunda, uygulamanız kuruluştaki her kullanıcının profilini güncelleştirebilir. Öte yandan oturum açmış kullanıcı bir yönetici rolünde değilse, uygulamanız yalnızca oturum açmış kullanıcının profilini güncelleştirebilir. Kuruluştaki diğer kullanıcıların profillerini güncelleştiremez çünkü adına çalışma iznine sahip olduğu kullanıcı söz konusu ayrıcalıklara sahip değildir.
  
- Uygulama izinleri için _etkili izinleri_ uygulamanızı izinle kapsanan ayrıcalıkları tam düzeyini olacaktır. Örneğin, sahip bir uygulama _User.ReadWrite.All_ uygulama izni, kuruluşunuzdaki her kullanıcının profilini güncelleştirebilirsiniz. 

## <a name="openid-connect-scopes"></a>Openıd Connect kapsamları

Openıd Connect v2.0 uygulaması belirli bir kaynak için geçerli olmayan birkaç iyi tanımlanmış kapsamına sahiptir: `openid`, `email`, `profile`, ve `offline_access`.

### <a name="openid"></a>openıd

Uygulama oturum açma kullanarak gerçekleştiriyorsa [Openıd Connect](active-directory-v2-protocols.md), isteği göndermelidir `openid` kapsam. `openid` Kapsamı, iş hesabı onay sayfası "oturum açtığınızda" iznini ve kişisel Microsoft hesabı onay sayfası "Profilinizi görüntüleyin ve uygulamaları ve Microsoft hesabınızı kullanarak hizmetlere bağlanma" iznini gösterir. Bu izne sahip bir kullanıcı için benzersiz bir tanımlayıcı biçiminde alabilir `sub` talep. Ayrıca uygulama erişimi ve UserInfo uç noktasına sağlar. `openid` Kapsamı farklı bir uygulama bileşenleri arasında HTTP çağrıları güvenliğini sağlamak için kullanılan kimlik belirteçlerini almak için v2.0 belirteç uç noktası kullanılabilir.

### <a name="email"></a>e-posta

`email` Kapsamı ile kullanılabilir `openid` kapsamı ve diğerleri. Uygulama erişimi için kullanıcının birincil e-posta adresi biçiminde sağlar `email` talep. `email` Yalnızca bir e-posta adresi her zaman çalışması değil kullanıcı hesabıyla ilişkiliyse talep bir belirteç içine eklenir. Kullanılıyorsa `email` kapsamı, uygulamanız hazırlanmış bir durumu işlemek için `email` talep belirteci yok.

### <a name="profile"></a>profil

`profile` Kapsamı ile kullanılabilir `openid` kapsamı ve diğerleri. Kullanıcı hakkındaki bilgileri önemli miktarda uygulama erişim sağlar. Uygulamaya erişebildiğinizden bilgiler içerir, ancak kullanıcının verilen adı, Soyadı, tercih edilen kullanıcı adı ve nesne kimliği için sınırlı değildir Belirli bir kullanıcı için id_tokens parametresinde kullanılabilir profili talepleri tam bir listesi için bkz. [ `id_tokens` başvuru](id-tokens.md).

### <a name="offlineaccess"></a>offline_access

[ `offline_access` Kapsam](https://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) erişim kaynaklara kullanıcı adına uzun bir süre sağlar. İş hesabı onay sayfasında, bu kapsamı "verilerinizi dilediğiniz zaman erişim" izni görünür. Kişisel Microsoft hesabı onay sayfasında, "bilgilerinize dilediğiniz zaman erişim" izni görünür. Bir kullanıcının ne zaman onaylar `offline_access` kapsamı, uygulama v2.0 belirteç uç noktasından yenileme belirteçleri alabilir. Uzun süreli yenileme belirteçleri. Uygulamanızı, eski görüntülerin süresi dolduğundan yeni erişim belirteçleri elde edebilirsiniz.

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

`scope` Parametre uygulamanın istediği izinleri devralmış boşlukla ayrılmış bir listesi verilmiştir. Her izin, kaynak tanımlayıcısı (uygulama kimliği URI'si) için izin değeri ekleyerek gösterilir. İstek örnekte, uygulamanın kullanıcının Takvim okuyun ve kullanıcı olarak posta gönderme izni gerekir.

Kullanıcı kimlik bilgilerini girdikten sonra v2.0 uç noktası eşleşen bir kayıt için denetler *kullanıcı onayı*. Kullanıcının istenen izinleri birine geçmişte onay verdi değil ya da yöneticinin tüm kuruluş adına bu izinleri seçtiği, v2.0 uç noktası istenen izinleri vermek için kullanıcıya sorar.

![İş hesabı onayı](./media/v2-permissions-and-consent/work_account_consent.png)

Kullanıcı bir izin isteği onayladığında, onay kaydedilir ve kullanıcı yeniden uygulamaya sonraki oturum açma işlemleri üzerinde onay gerekmez.

## <a name="requesting-consent-for-an-entire-tenant"></a>Tüm bir kiracı için onay isteme

Genellikle, bir kuruluşun bir lisans ya da bir uygulama için bir abonelik satın aldığında, kuruluşun tüm üyeleri tarafından kullanılmak üzere uygulama proaktif bir şekilde ayarlamak kuruluş istiyor. Bu işlemin bir parçası, bir yönetici kiracıdaki tüm kullanıcılar adına hareket uygulamaya izin verebilirsiniz. Yönetici, Kiracı genelinde izin verir, kuruluşun kullanıcılar, uygulama için bir onay sayfası görmez.

Bir kiracıdaki tüm kullanıcılar için temsilci izinleri izin istemek için yönetici onayı uç noktası uygulamanızı kullanabilirsiniz.

Buna ek olarak, uygulamaların uygulama izinleri istemek için yönetici onayı uç noktası kullanmanız gerekir.

## <a name="admin-restricted-permissions"></a>Admin-kısıtlı izinler

Bazı Microsoft ekosisteminde yüksek ayrıcalıklı izinlere ayarlanabilir *admin-kısıtlı*. Bu tür izinler örnekleri şunları içerir:

* Kullanarak tüm kullanıcıların tüm profilini okuma `User.Read.All`
* Kullanarak, bir kuruluşun dizine veri yazma `Directory.ReadWrite.All`
* Kullanarak, bir kuruluşun dizinindeki tüm grupları okuma `Groups.Read.All`

Bir tüketici kullanıcı verileri bu tür bir uygulama erişimi verebilir olsa da, kurumsal kullanıcılar kümesine hassas şirket verilerinin erişim kısıtlanır. Uygulamanızı erişim bu izinleri birine bir kuruluş kullanıcıdan istemesi durumunda kullanıcı, uygulamanın izinlerini onay yetkiniz yok bildiren bir hata iletisi alır.

Uygulamanızı kuruluşlar için kapsamları admin-kısıtlı erişim gerektiriyorsa bir şirket yöneticisinden doğrudan yönetici onayı sonraki bölümde açıklandığı uç noktası, kullanarak da istemelidir.

Yüksek ayrıcalıklı izinlere temsilci ve yönetici yönetici onay uç noktası aracılığıyla bu izinleri verir. Uygulamanın istediği, kiracıdaki tüm kullanıcılar için izin verilir.

Uygulamanın uygulama izinleri isteyen bir yönetici uç noktası aracılığıyla yönetici bu izinleri kabul veriyorsa, belirli bir kullanıcı adına bu izni yapılmaz. Bunun yerine, istemci uygulama izinleri verilir *doğrudan*. Bu tür İzinler genellikle arka plan programı Services'ı ve arka planda çalışan etkileşimli olmayan diğer uygulamalar tarafından yalnızca kullanılır.

## <a name="using-the-admin-consent-endpoint"></a>Yönetici onay uç noktası kullanma

Şirket Yöneticisi, uygulamanızın kullandığı ve uç noktayı yetkilendirmek için yönlendirilir, Microsoft kimlik platformu kullanıcı rolü algılar ve bunlar için istediğiniz izinleri Kiracı genelinde adına onay isteyip istemediğini isteyin. Ancak, var. Ayrıca proaktif olarak yöneticinin tüm Kiracı adına izin veren istek istiyorsanız kullanabileceğiniz bir adanmış yönetici onay uç noktası Bu uç noktayı kullanarak da (Bu Authorize son noktası kullanarak istenemez) uygulama izinleri istemek için gereklidir.

Bu adımları izlerseniz, uygulamanızı admin-kısıtlı kapsamlar dahil olmak üzere, bir kiracıdaki tüm kullanıcılar için izinler isteyebilir. Bu yüksek ayrıcalıklı bir işlemdir ve yalnızca senaryonuz için gerekirse yapılmalıdır.

Adımları uygulayan bir kod örnek görmek için [admin-kısıtlı kapsamları örnek](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2).

### <a name="request-the-permissions-in-the-app-registration-portal"></a>Uygulama kayıt portalında izinlere ilişkin istek

Yönetici onayı bir kapsam parametresi kabul etmiyor, uygulama kaydında izinlerin istenen şekilde statik olarak tanımlanması gerekir. Genel izinler statik olarak belirli bir uygulama için tanımlı bir alt kümesi, dinamik olarak/artımlı olarak isteyen, izinleri olduğundan emin olun en iyi bir uygulamadır.

Bir uygulama için statik olarak istenen izinler listesini yapılandırmak için: 
1. Uygulamanıza gidin [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), veya [uygulama oluşturma](quickstart-v2-register-an-app.md) henüz yapmadıysanız.
2. Bulun **Microsoft Graph izinleri** bölümüne ve ardından uygulamanız için gerekli izinleri ekleyin.
3. **Kaydet** uygulama kaydı.

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
| `tenant` | Gerekli | İzni istemek için istediğiniz dizinin Kiracı. GUID veya kolay adı biçiminde sağlanan veya "Genel" ile örnekte görüldüğü gibi genel olarak başvurulan. |
| `client_id` | Gerekli | Uygulama Kimliği [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) uygulamanıza atanan. |
| `redirect_uri` | Gerekli |Yeniden yönlendirme URI'si, uygulamanızı işlemek gönderilecek yanıt istediğiniz. Yeniden yönlendirme uygulama kayıt Portalı'nda kayıtlı bir URI'leri biri tam olarak eşleşmesi gerekir. |
| `state` | Önerilen | Belirteç yanıtta döndürülecek isteğinde bulunan bir değer. Bu, istediğiniz herhangi bir içerik dizesi olabilir. Durum, uygulama kullanıcının durumu hakkındaki bilgileri sayfasında ya da görünümü üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce kodlamak için kullanın. |

Bu noktada, Azure AD isteği tamamlamak oturum açmak bir kiracı Yöneticisi gerektirir. Yönetici uygulama kayıt portalında uygulamanıza için istenen tüm izinleri de onaylaması istenir.

#### <a name="successful-response"></a>Başarılı yanıt

Yönetici izinleri uygulamanızın onaylarsa, başarılı yanıt şöyle görünür:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parametre | Açıklama |
| --- | --- | --- |
| `tenant` | Uygulamanız, GUID biçiminde istenen izinler directory kiracısı. |
| `state` | Belirteç yanıtta döndürülecek isteğinde bulunan bir değer. Bu, istediğiniz herhangi bir içerik dizesi olabilir. Durumu, uygulama kullanıcının durumu hakkındaki bilgileri sayfasında ya da görünümü üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce kodlamak için kullanılır. |
| `admin_consent` | Ayarlanacak **true**. |

#### <a name="error-response"></a>Hata yanıtı

Yönetici izinleri uygulamanızın onaylamaz, başarısız bir yanıt şöyle görünür:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parametre | Açıklama |
| --- | --- | --- |
| `error` |Oluşan hataları türlerini sınıflandırmak için kullanılabilir ve hatalara tepki vermek için kullanılan bir hata kodu dizesi. |
| `error_description` |Bir hata nedenini Geliştirici yardımcı olabilecek belirli bir hata iletisi. |

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

## <a name="troubleshooting"></a>Sorun giderme

Siz veya uygulamanızın kullanıcıları onay işlemi sırasında beklenmeyen hatalar görüyorsanız, lütfen sorun giderme adımları için bu makalede başvuru: [bir uygulama için onay gerçekleştirirken beklenmeyen bir hata](../manage-apps/application-sign-in-unexpected-user-consent-error.md).
