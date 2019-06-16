---
title: Azure Active Directory koşullu erişim için Geliştirici Kılavuzu
description: Geliştirici Kılavuzu ve Azure AD koşullu erişim senaryoları
services: active-directory
keywords: ''
author: rwike77
manager: CelesteDG
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda
ms.date: 02/28/2019
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9e4e0eb830d5ede910e72ec3193cfd613561811b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67111531"
---
# <a name="developer-guidance-for-azure-active-directory-conditional-access"></a>Azure Active Directory koşullu erişim için Geliştirici Kılavuzu

Azure Active Directory'de (Azure AD) koşullu erişim özelliğini, uygulamanızı güvenli hale getirme ve bir hizmet korumak için kullanabileceğiniz çeşitli yollardan sunar. Geliştiriciler ve enterprise müşterileri de dahil olmak üzere çok çeşitli hizmetlerini korumak koşullu erişim sağlar:

* Multi-factor authentication
* Kayıtlı cihazlar belirli hizmetlere erişmek için yalnızca Intune izin verme
* Kullanıcı konumları ve IP kısıtlama aralıkları

Koşullu erişim tam özellikleri hakkında daha fazla bilgi için bkz. [Azure Active Directory'de koşullu erişim](../active-directory-conditional-access-azure-portal.md).

Azure AD için uygulama oluşturmaya, geliştiriciler için nasıl koşullu erişimle kullanabilirsiniz ve üzerinde koşullu erişim ilkelerinin uygulandığı denetiminiz sahip olmayan kaynaklara erişim etkisini hakkında öğreneceksiniz bu makalede açıklanır. Makale ayrıca koşullu erişim uygulamaları Microsoft Graph erişme ve API'leri çağırma web uygulamaları, on-behalf-of akışı içinde keşfediyor.

Bilgi [tek](quickstart-v1-integrate-apps-with-azure-ad.md) ve [çok kiracılı](howto-convert-app-to-be-multi-tenant.md) uygulamalar ve [ortak kimlik doğrulaması desenleri](authentication-scenarios.md) varsayılır.

## <a name="how-does-conditional-access-impact-an-app"></a>Uygulama koşullu erişimi nasıl etkiler?

### <a name="app-types-impacted"></a>Etkilenen uygulama türleri

Koşullu erişim, en yaygın durumlarda, bir uygulamanın davranışını değiştirmez veya geliştiriciden herhangi bir değişiklik gerektirmez. Bir uygulamayı bir hizmet için bir belirteç dolaylı olarak ya da sessizce istediğinde yalnızca belirli durumlarda koşullu erişim "zorluklarını" işlemek üzere kod değişikliklerini uygulama gerektirir. Etkileşimli bir oturum açma isteği gerçekleştirmek kadar basit olabilir.

Özellikle aşağıdaki senaryolarda, koşullu erişim "zorluklarını" işlemek için kod gerektirir:

* Uygulamaları üzerinde-behalf-of akışı gerçekleştirme
* Birden çok Hizmetleri/kaynaklarına erişen uygulamaları
* ADAL.js kullanarak tek sayfa uygulamaları
* Web Apps kaynak çağırma

İlkeleri uygulamaya uygulanabilir, ancak bir web API'sine de uygulanabilir koşullu erişim, uygulamanızı erişir. Koşullu erişim ilkesini yapılandırma hakkında daha fazla bilgi için bkz: [hızlı başlangıç: Azure Active Directory koşullu erişimiyle birlikte belirli uygulamalar için mfa'yı gerekli](../conditional-access/app-based-mfa.md).

Kurumsal bir müşteri senaryosuna bağlı olarak, uygulamak ve herhangi bir zamanda koşullu erişim ilkelerini kaldırın. Uygulamanızı yeni bir ilke uygulandığında çalışmaya devam edebilmesi "özel" işleme uygulamak gerekir. Aşağıdaki örnekler, sınama işleme gösterir.

### <a name="conditional-access-examples"></a>Koşullu erişim örnekleri

Bazı senaryolar olduğu gibi diğer iş ise, koşullu erişimi işlemek için kod değişiklikleri gerektirir. Fark bazı Öngörüler sunan çok faktörlü kimlik doğrulaması yapmak için koşullu erişim kullanarak bazı senaryolar aşağıda verilmiştir.

* Tek kiracılı iOS uygulaması oluşturuyorsunuz ve koşullu erişim ilkesi uygula. Uygulama, bir kullanıcı oturum açtığında ve istek API erişimi yoktur. Kullanıcı oturum açtığında ilke otomatik olarak çağrılır ve kullanıcı çok faktörlü kimlik doğrulaması (MFA) gerçekleştirmesine gerek. 
* Bir aşağı akış API'ye erişmek için bir orta katman hizmet kullanan yerel bir uygulamayı oluşturuyorsunuz. Bu uygulamayı kullanarak şirket kurumsal bir müşterinin aşağı akış API için bir ilke uygulanır. Son kullanıcı oturum açtığında, yerel uygulama Orta katmanda ister ve belirteci gönderir. Orta katman aşağı akış API erişimi istemek için on-behalf-of akışı gerçekleştirir. Bu noktada, "zor" talepler için Orta katmanda sunulur. Orta katman sınama koşullu erişim ilkesi ile uyum sağlaması gerektiğinde geri yerel uygulamaya gönderir.

#### <a name="microsoft-graph"></a>Microsoft Graph

Koşullu erişim ortamlarda uygulamalar oluştururken, Microsoft Graph özel durumlar vardır. Genel olarak, koşullu erişim mekanikleri aynı şekilde davranır, ancak kullanıcıların görmesi ilkeleri uygulamanızı grafikten isteyen temel alınan verileri temel alır. 

Özellikle, tüm Microsoft Graph kapsamları, ayrı ayrı uygulanan ilkelere sahip bazı veri kümesini temsil eder. Koşullu erişim ilkeleri, belirli veri kümeleri atanmış olduğundan, Azure AD Grafı - arkasında verileri temel alan koşullu erişim ilkelerini zorlamak yerine kendi grafik.

Örneğin, bir uygulama aşağıdaki Microsoft Graph kapsamları isterse,

```
scopes="Bookings.Read.All Mail.Read"
```

Uygulama kullanıcılarının Bookings ve Exchange tüm ilkeleri karşılamak üzere bekleyebilirsiniz. Erişim verirse birden fazla veri kümesi için bazı kapsamlar eşlenebilir. 

### <a name="complying-with-a-conditional-access-policy"></a>Koşullu erişim ilkesi ile uyumlu

Oturum kurulduktan sonra birçok farklı uygulama Topolojileri için koşullu erişim ilkesi değerlendirilir. Koşullu erişim ilkesi ayrıntı düzeyi, uygulamaları ve hizmetleri üzerinde çalıştığı gibi çalışır, hangi çağrılan noktası yoğun bir şekilde gerçekleştirmeye çalıştığınız senaryoya bağlıdır.

Koşullu erişim ilkesi ile ilgili bir hizmete erişmek uygulamanızı çalışır bir koşullu Erişim İtirazı karşılaşabilirsiniz. Bu zorluğu kodlandığını `claims` Azure AD'den bir karşılık gelen parametre. Bu sınama parametrenin bir örnek aşağıda verilmiştir: 

```
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

Geliştiriciler bu Yarışması'na katılın ve Azure AD'ye yeni bir istek üzerine ekleyin. Bu durum geçirme, koşullu erişim ilkesi ile uyum sağlamak gerekli olan herhangi bir eylemi gerçekleştirmek için son kullanıcıya sorar. Aşağıdaki senaryolarda, hata ve parametre ayıklama özellikleri açıklanmıştır.

## <a name="scenarios"></a>Senaryolar

### <a name="prerequisites"></a>Önkoşullar

Azure AD koşullu erişim, eklenen bir özelliktir [Azure AD Premium](https://docs.microsoft.com/azure/active-directory/active-directory-whatis). Lisanslama gereksinimleri hakkında daha fazla bilgi [lisanssız kullanım raporu](../active-directory-conditional-access-unlicensed-usage-report.md). Geliştiriciler birleştirme [Microsoft Developer Network](https://msdn.microsoft.com/dn308572.aspx), Azure AD Premium içeren Enterprise Mobility Suite için ücretsiz bir abonelik içerir.

### <a name="considerations-for-specific-scenarios"></a>Belirli senaryolar için dikkat edilmesi gerekenler

Aşağıdaki bilgiler, yalnızca bu koşullu erişim senaryolarda geçerlidir:

* Uygulamaları üzerinde-behalf-of akışı gerçekleştirme
* Birden çok Hizmetleri/kaynaklarına erişen uygulamaları
* ADAL.js kullanarak tek sayfa uygulamaları

Aşağıdaki bölümlerde, daha karmaşık yaygın senaryolar açıklanmaktadır. Koşullu erişim ilkeleri, belirteç istediği zaman uygulanan bir koşullu erişim ilkesi olan hizmetinde değerlendirilir İlkesi işletim çekirdeği olur.

## <a name="scenario-app-performing-the-on-behalf-of-flow"></a>Senaryo: Kullanıcı adına akışını gerçekleştiren uygulama

Bu senaryoda, yerel bir uygulama bir web hizmetini/API'sini çağıran vakası inceleyeceğiz. Sırayla bu hizmet bir aşağı akış hizmeti çağırmak amacıyla "on-behalf-of" akışını yapar. Bizim durumumuzda, bizim koşullu erişim ilkesi aşağı akış hizmetine (Web API 2) uyguladığınız ve bir sunucu/daemon uygulamasının yerine yerel bir uygulama kullanma. 

![Uygulama üzerinde temsili Akış Diyagramı gerçekleştirme](./media/conditional-access-dev-guide/app-performing-on-behalf-of-scenario.png)

Web API 1 her zaman aşağı akış API karşılaşabilirsiniz değil olarak çok faktörlü kimlik doğrulaması için son kullanıcı Web API 1 için'ilk belirteç isteği istemez. Web API 1 bir belirteç on-behalf-of Web API 2 için Kullanıcı isteği dener sonra kullanıcı çok faktörlü kimlik doğrulaması ile oturum açmadı olduğundan istek başarısız olur.

Azure AD, bazı ilginç veriler ile bir HTTP yanıtı döndürür:

> [!NOTE]
> Bu örnekte, çok faktörlü kimlik doğrulaması hata açıklamasını, yoktur ancak geniş kapsamlı `interaction_required` olası koşullu erişim için tıklarsınız.

```
HTTP 400; Bad Request
error=interaction_required
error_description=AADSTS50076: Due to a configuration change made by your administrator, or because you moved to a new location, you must use multi-factor authentication to access '<Web API 2 App/Client ID>'.
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

Web API 1'de, biz hata catch `error=interaction_required`ve geri gönderme `claims` masaüstü uygulaması sınaması. Bu noktada, Masaüstü uygulamasını yeni bir yapabilirsiniz `acquireToken()` arama ve ekleme `claims`ek sorgu dizesi parametresi olarak sınama. Bu yeni istek kullanıcının çok faktörlü kimlik doğrulaması yapın ve ardından bu yeni belirteç geri Web API 1'e gönderin ve on-behalf-of akışı tamamlama olmasını gerektirir.

Bu senaryo denemek için bkz. bizim [.NET kodu örneği](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca). Bu talep sınaması geri Web API 1'den yerel uygulamaya geçmek ve istemci uygulamanın içinde yeni bir isteği oluşturmak nasıl gösterir.

## <a name="scenario-app-accessing-multiple-services"></a>Senaryo: Uygulama birden çok hizmetlerine erişme

Bu senaryoda, biri bir koşullu erişim ilkesi atanmış olan iki hizmet içinde erişen bir web uygulaması vakası inceleyeceğiz. Uygulama mantığınız bağlı olarak, uygulamanızı her iki web hizmetlerine erişim için gerektirmez yol bulunabilir. Bu senaryoda, bir belirteç isteği sırada son kullanıcı deneyimi de önemli bir rol oynar.

Web hizmeti A ve B sunuyoruz ve web hizmeti B uygulanan bizim koşullu erişim ilkesi varsayalım. Koşullu erişim ilkesini ilk etkileşimli yetkilendirme isteği için her iki hizmet onayı gerektirir, ancak her durumda gerekli değildir. Uygulama web hizmeti B için bir belirteç isteğinde bulunursa ilke çağrılır ve sonraki istekleri için bir web hizmeti Ayrıca başarılı şekilde.

![Uygulama Hizmetleri birden çok akış diyagramı erişme](./media/conditional-access-dev-guide/app-accessing-multiple-services-scenario.png)

Alternatif olarak, uygulama web hizmeti bir için başlangıçta bir belirteç isterse, son kullanıcı koşullu erişim ilkesi çağrılmaz. Bu, son kullanıcı deneyimi ve her durumda çağrılacak koşullu erişim ilkesini zorlama değil denetlemek için uygulama geliştiricisinin sağlar. Zor durumda uygulama daha sonra b web hizmeti için bir belirteç isteklerini Bu noktada, son kullanıcı koşullu erişim ilkesi ile uyumlu olması gerekir. Uygulama çalıştığında `acquireToken`, (Aşağıdaki diyagramda gösterildiği) şu hatayı verebilir:

```
HTTP 400; Bad Request
error=interaction_required
error_description=AADSTS50076: Due to a configuration change made by your administrator, or because you moved to a new location, you must use multi-factor authentication to access '<Web API App/Client ID>'.
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

![Uygulama yeni bir belirteç isteğinde birden çok hizmetlerine erişme](./media/conditional-access-dev-guide/app-accessing-multiple-services-new-token.png)

Uygulama ADAL kitaplığı kullanıyorsa, belirteci almak için bir hata her zaman etkileşimli olarak denenir. Bu etkileşimli istek ortaya çıktığında, son kullanıcı koşullu erişim ile uyumlu fırsatına sahiptir. İstek olmadığı sürece bu doğrudur bir `AcquireTokenSilentAsync` veya `PromptBehavior.Never` etkileşimli gerçekleştirmek için bu durumda uygulamanın gereksinim duyduğu ```AcquireToken``` kullanıcıdan ilkeye uymak fırsatı sunmak için istek.

## <a name="scenario-single-page-app-spa-using-adaljs"></a>Senaryo: ADAL.js kullanarak tek sayfalı uygulama (SPA)

Bu senaryoda, biz bir koşullu erişim korumalı web API'sini çağırmak için ADAL.js kullanarak tek sayfalı uygulama (SPA) sahip olduğunuzda vakası inceleyeceğiz. Bu basit bir mimari ancak koşullu erişim geçici bir çözüm geliştirirken dikkate alınması gereken bazı küçük farklar vardır.

ADAL.js içinde belirteçleri elde etmek birkaç işlev vardır: `login()`, `acquireToken(...)`, `acquireTokenPopup(…)`, ve `acquireTokenRedirect(…)`.

* `login()` etkileşimli bir oturum açma isteği aracılığıyla bir kimlik belirteci alır, ancak herhangi bir hizmete (bir koşullu erişim korumalı web API'sini dahil) için erişim belirteçleri elde değil.
* `acquireToken(…)` ardından sessizce herhangi bir durumda kullanıcı Arabirimi göstermez anlamına gelen bir erişim belirteci almak için kullanılabilir.
* `acquireTokenPopup(…)` ve `acquireTokenRedirect(…)` olduğu etkileşimli bir kaynak için bir belirteç istemek için kullanılan her ikisi de anlamına gelir her zaman gösterdikleri oturum açma kullanıcı Arabirimi.

Bir uygulama bir Web API'sini çağırmak için bir erişim belirteci gerektiğinde, deneme bir `acquireToken(…)`. Belirteç oturumun süresi doldu veya bir koşullu erişim ilkesi ile uyum sağlamak ihtiyacımız sonra *acquireToken* işlev başarısız olur ve uygulamanın kullandığı `acquireTokenPopup()` veya `acquireTokenRedirect()`.

![ADAL akış diyagramı kullanarak tek sayfalı uygulama](./media/conditional-access-dev-guide/spa-using-adal-scenario.png)

Github'dan bir örnek ile koşullu erişim senaryomuz yol. Son kullanıcı, yalnızca sitede geldiğimizi ve bir oturumu yok. Gerçekleştiririz bir `login()` çağrısı, bir kimliği multi-Factor authentication olmadan belirteci alın. Ardından kullanıcı uygulama istek verilerini bir web API'sini gerekiyor. bir düğme denk gelir. Uygulamayı dener bir `acquireToken()` çağrısı ancak henüz çok faktörlü kimlik doğrulaması ve koşullu erişim ilkesi ile uyum sağlamak için gereksinimleri yapmamış bu yana başarısız.

Azure AD, şu HTTP yanıtı geri gönderir:

```
HTTP 400; Bad Request
error=interaction_required
error_description=AADSTS50076: Due to a configuration change made by your administrator, or because you moved to a new location, you must use multi-factor authentication to access '<Web API App/Client ID>'.
```

Yakalamak uygulamamızı ihtiyacı `error=interaction_required`. Uygulama ya da sonra kullanabilir `acquireTokenPopup()` veya `acquireTokenRedirect()` aynı kaynağı. Kullanıcı çok faktörlü kimlik doğrulaması yapmak için zorlanır. Kullanıcı çok faktörlü kimlik doğrulaması tamamlandıktan sonra uygulamayı istenen kaynak için yeni bir belirteç verilir.

Bu senaryo denemek için bkz. bizim [JS SPA'ya On-behalf-of kod örneği](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca). Bu kod örneği, web API'si daha önce bu senaryoyu göstermek amacıyla JS SPA'ya ile kayıtlı ve koşullu erişim ilkesi kullanır. Bu, Web API'niz için kullanılabilecek bir erişim belirteci alma ve düzgün bir şekilde talep sınama işlemek nasıl gösterir. Alternatif olarak, genel kullanıma alma [Angular.js kod örneği](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) Angular bir SPA hakkında yönergeler için

## <a name="see-also"></a>Ayrıca bkz.

* Özellikleri hakkında daha fazla bilgi edinmek için [Azure Active Directory'de koşullu erişim](../active-directory-conditional-access-azure-portal.md).
* Daha fazla Azure AD kod örnekleri için bkz. [kod örnekleri GitHub deposunda](https://github.com/azure-samples?utf8=%E2%9C%93&q=active-directory).
* ADAL SDK'ın ve başvuru belgelerine erişim hakkında daha fazla bilgi için bkz. [kitaplık Kılavuzu](active-directory-authentication-libraries.md).
* Çok kiracılı senaryolarda hakkında daha fazla bilgi için bkz: [çok kiracılı desenini kullanarak, kullanıcıların oturum açma](howto-convert-app-to-be-multi-tenant.md).
