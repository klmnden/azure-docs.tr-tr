---
title: "Azure Active Directory koşullu erişim için Geliştirici Kılavuzu | Microsoft Docs"
description: "Geliştirici Kılavuzu ve Azure AD koşullu erişim senaryoları"
services: active-directory
keywords: 
author: danieldobalian
manager: mbaldwin
editor: PatAltimore
ms.author: dadobali
ms.date: 07/19/2017
ms.assetid: 115bdab2-e1fd-4403-ac15-d4195e24ac95
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.openlocfilehash: eddc1988e094a50ba7e41331a576846aa26f77a4
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="developer-guidance-for-azure-active-directory-conditional-access"></a>Azure Active Directory koşullu erişim için Geliştirici Kılavuzu

Azure Active Directory (AD), uygulamanızın güvenli ve bir hizmet korumak için çeşitli yollar sunar.  Bu benzersiz özellikleri koşullu erişim biridir.  Geliştiricilerin ve kurumsal müşteriler dahil olmak üzere çok sayıda hizmetlerini korumak koşullu erişim sağlar:

* Multi-Factor Authentication
* Kayıtlı cihazlar belirli hizmetlere erişmek için yalnızca Intune izin verme
* Kullanıcı konumları ve IP kısıtlama aralıkları

Koşullu erişim özelliklerinin hakkında daha fazla bilgi için bkz: [Klasik Azure portalındaki koşullu erişim](../active-directory-conditional-access-azure-portal.md). 

Bu makalede, geliştiriciler için Azure AD uygulamaları oluşturmak için hangi koşullu erişim gelir odaklanın.  Bilgisi varsayar [tek](active-directory-integrating-applications.md) ve [çok kiracılı](active-directory-devhowto-multi-tenant-overview.md) uygulamaları ve [ortak kimlik doğrulama desenler](active-directory-authentication-scenarios.md).

Biz, koşullu erişim ilkelerinin uygulandığı olabilir üzerinde denetime sahip değilseniz kaynaklara erişme etkisini dalın.  Ayrıca, biz web uygulamaları etkileri üzerinde-temsili akış, koşullu erişim ve Microsoft Graph erişme ve API'larını çağırma keşfedin.

## <a name="how-does-conditional-access-impact-an-app"></a>Koşullu erişim uygulama nasıl etkiler?

### <a name="app-types-impacted"></a>Etkilenen uygulama türleri

En yaygın durumlarda, koşullu erişim bir uygulamanın davranışı değiştirmez veya geliştiriciden herhangi bir değişiklik gerektirmez.  Bir uygulama bir hizmet için bir belirteç dolaylı ya da sessizce istediğinde yalnızca belirli durumlarda bir uygulamayı koşullu erişim "zorluklar" işlemek için kod değişiklikleri gerektirir.  Etkileşimli bir oturum açma isteği gerçekleştirme olarak kadar basit olabilir. 

Özellikle, aşağıdaki senaryolarda koşullu erişim "zorluklar" işlemek için kod gerektirir: 

* Microsoft Graph erişen uygulamaları
* Uygulamalar üzerinde-temsili akış gerçekleştirme
* Uygulamaları birden çok Hizmetleri/kaynaklara erişme
* Tek sayfa uygulamaları ADAL.js kullanma
* Web uygulamaları bir kaynak çağırma

Koşullu erişim ilkeleri uygulama için geçerli, ancak aynı zamanda bir web API uygulanabilir uygulamanızı erişir. Koşullu erişim ilkesini yapılandırma hakkında daha fazla bilgi için lütfen bkz [Azure Active Directory koşullu erişimi kullanmaya başlama](../active-directory-conditional-access-azure-portal-get-started.md).

Bağlı olarak senaryo, bir kurumsal müşteri uygulanır ve herhangi bir zamanda koşullu erişim ilkelerini kaldırın.  Yeni bir ilke uygulandığında çalışmaya devam edebilmesi, uygulamanızın "sınama" işleme uygulamak gerekir. Aşağıdaki örnekler sınama işleme gösterir. 

### <a name="conditional-access-examples"></a>Koşullu erişim örnekleri

Bazı senaryolar başkalarının olduğu gibi çalışır ancak koşullu erişim işlemek için kod değişikliklerini gerektirir.  Bazı bir anlayış farkı verir çok faktörlü kimlik doğrulaması yapmak için koşullu erişim kullanarak bazı senaryolar verilmiştir.

* Bir tek Kiracı iOS uygulaması oluşturmak ve bir koşullu erişim ilkesi uygula.  Uygulama bir kullanıcı oturum açtığında ve bir API erişim isteği değil.  Kullanıcı oturum açtığında ilke otomatik olarak çağrılır ve kullanıcının çok faktörlü kimlik doğrulaması (MFA) yapmak gerekmez. 
* Microsoft Graph diğer hizmetler arasında Exchange'e erişmek için kullandığı bir çok kiracılı web uygulaması oluşturuyorsanız.  Bu uygulamayı uyarlar bir enterprise müşterileri Exchange'de bir ilke ayarlar.  Web uygulaması için MS Graph bir belirteç istediğinde, uygulama ilkeye uymak için sınanır değil.  Son kullanıcı oturum geçerli belirteçlerini imzalanır. Uygulama Exchange verilerine erişmek için Microsoft Graph karşı bu belirteç kullanmaya çalıştığında, bir talep "challenge" web uygulamasına döndürülen ```WWW-Authenticate``` üstbilgi.  Uygulama ardından kullanabilirsiniz ```claims``` yeni bir istek ve son kullanıcı ile koşullara uyması istenir. 
* Orta katman hizmet bir aşağı akış API erişmek için kullandığı yerel bir uygulama oluşturmakta olduğunuz.  Bu uygulamayı kullanarak şirket Kurumsal müşteride Aşağı Akış API için bir ilke uygulanır.  Bir son kullanıcı oturum açtığında yerel uygulama orta katman erişim isteğinde ve belirteci gönderir.  Orta katman aşağı akış API'sine erişim istemek için üzerinde-temsili akış gerçekleştirir.  Bu noktada, bir talep "challenge" Orta katman sunulur. Orta katman koşullu erişim ilkesi ile uyum sağlamak için gereken geri yerel uygulama, sınama gönderir.

### <a name="complying-with-a-conditional-access-policy"></a>Koşullu erişim ilkesi ile uymak

Oturum kurulduktan sonra birçok farklı uygulama Topolojileri için bir koşullu erişim ilkesi değerlendirilir.  Bir koşullu erişim ilkesi uygulama ve hizmetlere kesinliği çalıştığı gibi çalışır, hangi çağrılır noktası yoğun bir şekilde gerçekleştirmeye çalıştığınız senaryoya bağlıdır.

Uygulamanızı bir koşullu erişim ilkesi ile bir hizmeti erişmeyi denediğinde, bir koşullu erişim challenge karşılaşabilirsiniz.  Bu sorunu kodlandığını `claims` Azure AD'den karşılık gelen bir parametre veya Microsoft Graph.  Bu sınama parametre bir örneği burada verilmiştir: 

```
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

Geliştiriciler, bu sorunu ele ve Azure ad ile yeni bir istek üzerine ekleyin.  Bu durum geçirme koşullu erişim ilkesi ile uyum sağlamak gerekli herhangi bir eylemi gerçekleştirmek için son kullanıcı ister. Aşağıdaki senaryolarda, hata ve parametre ayıklama özellikleri açıklanmıştır. 

## <a name="scenarios"></a>Senaryolar

### <a name="prerequisites"></a>Önkoşullar

Azure AD koşullu erişim, bulunan bir özelliktir [Azure AD Premium](../active-directory-whatis.md#choose-an-edition).  Lisans gereksinimleri hakkında daha fazla bilgiyi [lisanssız kullanım raporu](../active-directory-conditional-access-unlicensed-usage-report.md).  Geliştiriciler birleştirme [Microsoft Developer Network](https://msdn.microsoft.com/dn308572.aspx), Azure AD Premium içeren Enterprise Mobility Suite için ücretsiz bir abonelik içerir.

### <a name="considerations-for-specific-scenarios"></a>Belirli senaryolar için ilgili önemli noktalar

Aşağıdaki bilgiler, yalnızca bu koşullu erişim senaryolarda geçerlidir:

* Microsoft Graph erişen uygulamaları
* Uygulamalar üzerinde-temsili akış gerçekleştirme
* Uygulamaları birden çok Hizmetleri/kaynaklara erişme
* Tek sayfa uygulamaları ADAL.js kullanma

Aşağıdaki bölümlerde, senaryoları daha karmaşık biz ortak elde edersiniz.  İlkeye işletim çekirdeği koşullu erişim ilkelerinin belirteç istenen Microsoft Graph erişilen sürece uygulanan bir koşullu erişim ilkesine sahip hizmeti zamanında değerlendirildiği olur.

## <a name="scenario-app-accessing-microsoft-graph"></a>Senaryo: Microsoft Graph erişme uygulama

Bu senaryoda, biz Microsoft Graph için durum bir web uygulama isteklerini erişimi olduğunda yol. Koşullu erişim ilkesi, SharePoint, Exchange veya Microsoft Graph aracılığıyla bir iş yükü olarak erişilen başka bir hizmet için bu durumda atanabilir.  Bu örnekte, Sharepoint Online'da koşullu erişim ilkesi bulunmamaktadır varsayalım.

![Uygulama Microsoft Graph Akış Diyagramı erişme](media/active-directory-conditional-access-developer/app-accessing-microsoft-graph-scenario.png)

Uygulama yetkilendirme koşullu erişimi olmadan bir aşağı akış iş yükü erişim gerektiren Microsoft Graph için ilk kez istediğinde.  Herhangi bir ilke çağırmadan istek başarılı olur ve uygulama için Microsoft Graph belirteçlerini alır.  Bu noktada, uygulama erişim belirtecini bir taşıyıcı isteğinde istenen uç noktası için kullanabilir. Şimdi, Microsoft Graph, Sharepoint Online bir uç noktasını örneğin erişmek uygulamanın gerekir:`https://graph.microsoft.com/v1.0/me/mySite`

Yeni istek yeni bir belirteç verilen olmadan gerçekleştirebilmek için uygulamanın zaten Microsoft Graph için geçerli bir belirteci yok. Bu istek başarısız olur ve bir talep challenge, Microsoft Graph biçiminde bir HTTP 403 Yasak ile verildiği bir ```WWW-Authenticate``` sınaması.
Yanıtı bir örneği burada verilmiştir: 

```
HTTP 403; Forbidden 
error=insufficient_claims
www-authenticate="Bearer realm="", authorization_uri="https://login.windows.net/common/oauth2/authorize", client_id="<GUID>", error=insufficient_claims, claims={"access_token":{"polids":{"essential":true,"values":["<GUID>"]}}}"
```

Talep sınama içindedir ```WWW-Authenticate``` sonraki istek için talep parametre ayıklamak için Ayrıştırılan üstbilgi.  Yeni istek eklenir sonra Azure AD koşullu erişim ilkesi kullanıcı oturum açarken değerlendirmek için bilir ve uygulama artık koşullu erişim ilkesiyle uyumlu olur.  Sharepoint Online uç nokta isteği yinelenen başarılı olur.

```WWW-Authenticate``` Üstbilgi benzersiz yapısına sahip ve değerleri ayıklamak için ayrıştırmak için Önemsiz değil.  Yardımcı olmak için kısa bir yöntem aşağıda verilmiştir.

    ```C#
        /// <summary>
        /// This method extracts the claims value from the 403 error response from MS Graph. 
        /// </summary>
        /// <param name="wwwAuthHeader"></param>
        /// <returns>Value of the claims entry. This should be considered an opaque string. 
        /// Returns null if the wwwAuthheader does not contain the claims value. </returns>
        private String extractClaims(String wwwAuthHeader)
        {
            String ClaimsKey = "claims=";
            String ClaimsSubstring = "";
            if (wwwAuthHeader.Contains(ClaimsKey))
            {
                int Index = wwwAuthHeader.IndexOf(ClaimsKey);
                ClaimsSubstring = wwwAuthHeader.Substring(Index, wwwAuthHeader.Length - Index);
                string ClaimsChallenge;
                if (Regex.Match(ClaimsSubstring, @"}$").Success)
                {
                    ClaimsChallenge = ClaimsSubstring.Split('=')[1];
                }
                else
                {
                    ClaimsChallenge = ClaimsSubstring.Substring(0, ClaimsSubstring.IndexOf("},") + 1);
                }
                return ClaimsChallenge;
            }
            return null; 
        }
    ```

Talep sınama nasıl ele alınacağını gösteren kod örnekleri için başvurmak [üzerinde-adına-kodunu örnek](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca) ADAL .NET için.

## <a name="scenario-app-performing-the-on-behalf-of-flow"></a>Senaryo: uygulama üzerinde-temsili akış gerçekleştirme

Bu senaryoda, yerel bir uygulama bir web hizmeti/API çağrılarının çalışması yol.  Buna karşılık, bu hizmeti yapar ["üzerinde-temsili" akış](active-directory-authentication-scenarios.md#application-types-and-scenarios) bir aşağı akış hizmetini çağırmak için.  Bu örnekte, koşullu erişim ilkemizi aşağı akış hizmeti (Web API 2) uyguladıysanız ve sunucu/arka plan programı uygulama yerine yerel bir uygulama kullanarak. 

![Uygulama üzerinde-adına-Akış Diyagramı gerçekleştirme](media/active-directory-conditional-access-developer/app-performing-on-behalf-of-scenario.png)

Web API 1 her zaman aşağı akış API isabet değil olarak ilk belirteç isteği Web API 1 için çok faktörlü kimlik doğrulaması için son kullanıcı istemez.  Web API 1 üzerinde-adına-of bir belirteç kullanıcı için Web API 2 isteği dener sonra kullanıcı çok faktörlü kimlik doğrulaması ile oturum imzalamamış olduğundan istek başarısız olur.

Azure AD bazı ilginç veriler sahip bir HTTP yanıtı döndürür: 

> [!NOTE]
> Bu örnekte, çok faktörlü kimlik doğrulaması hata açıklaması, ancak çeşitli `interaction_required` olası koşullu erişim ilgili.  

```
HTTP 400; Bad Request 
error=interaction_required
error_description=AADSTS50076: Due to a configuration change made by your administrator, or because you moved to a new location, you must use multi-factor authentication to access '<Web API 2 App/Client ID>'.
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

Bizim Web API 1'de, biz hata catch `error=interaction_required`ve geri göndermek `claims` masaüstü uygulaması sınaması.  Bu noktada, Masaüstü uygulama yeni bir yapabilir `acquireToken()` çağırın ve ilave `claims`ek sorgu dizesi parametresi olarak sınama.  Bu yeni istek, çok faktörlü kimlik doğrulaması yapın ve ardından bu yeni belirteç Web API 1 geri göndermek ve üzerinde-temsili akış tamamlamak kullanıcının gerektirir.

Bu senaryo denemek için bkz: bizim [.NET kod örneği](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca).  Talep sınama geri Web API 1'den yerel uygulamaya geçirin ve istemci app içinde yeni bir istek oluşturun görüntülemeyi gösterir. 

## <a name="scenario-app-accessing-multiple-services"></a>Senaryo: uygulama birden çok hizmetlere erişme

Bu senaryoda, bunlardan biri atanan bir koşullu erişim ilkesine sahip bir web uygulaması iki hizmet erişen durum yol.  Uygulama mantığınızı bağlı olarak, uygulamanızı her iki web hizmetlerine erişim gerektirmez bir yolu bulunabilir.  Bu senaryoda, bir belirteç istemek sipariş son kullanıcı deneyimini önemli bir rol oynar.

Web hizmeti A ve B sahibiz ve web hizmeti B uygulanan bizim koşullu erişim ilkesine sahip varsayalım.  İlk etkileşimli kimlik doğrulama isteği her iki hizmet için onay gerektirir, ancak koşullu erişim ilkesi her durumda gerekli değildir.  Uygulama web hizmeti B için bir belirteç isterse, ilke çağrılır ve web hizmeti bir sonraki istekleri de başarılı şekilde.

![Uygulama Hizmetleri birden çok akış diyagramı erişme](media/active-directory-conditional-access-developer/app-accessing-multiple-services-scenario.png)

Alternatif olarak, uygulama için web hizmeti A başlangıçta bir belirteç isterse, son kullanıcı koşullu erişim ilkesi çağrılmaz.  Bu, son kullanıcı deneyimi denetlemek ve her durumda çağrılacak koşullu erişim ilkesi değil zorlamak üzere uygulama geliştiricisi sağlar. Hassas durumda uygulama daha sonra b web hizmeti için bir belirteç isteklerini varsa Bu noktada, son kullanıcı koşullu erişim ilkesi ile uyumlu olması gerekir.  Uygulama çalıştığında `acquireToken`, (Aşağıdaki diyagramda gösterilmiştir) şu hatayı verebilir: 

```
HTTP 400; Bad Request
error=interaction_required
error_description=AADSTS50076: Due to a configuration change made by your administrator, or because you moved to a new location, you must use multi-factor authentication to access '<Web API App/Client ID>'.
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
``` 

![Uygulama yeni bir belirteç isteme birden çok hizmetlere erişme](media/active-directory-conditional-access-developer/app-accessing-multiple-services-new-token.png)

Uygulama ADAL kitaplığını kullanıyorsanız, belirtecini almak üzere bir hata her zaman etkileşimli olarak yeniden denenir.  Bu etkileşimli istek olduğunda, son kullanıcı koşullu erişim ile uyumlu fırsatına sahip.  İstek sürece bu doğrudur bir `AcquireTokenSilentAsync` veya `PromptBehavior.Never` ; bu durumda uygulama etkileşimli gerçekleştirmek için gereksinim duyduğu ```AcquireToken``` son kullanım ilkeye uymak fırsatı sunmak için istek. 

## <a name="scenario-single-page-app-spa-using-adaljs"></a>Senaryo: Tek sayfa uygulaması (ADAL.js kullanarak SPA)

Biz korumalı koşullu erişim web API'sini çağırmak için ADAL.js kullanarak bir tek sayfalı uygulama (SPA) varsa, bu senaryoda, biz çalışması yol.  Bu basit bir mimaridir ancak koşullu erişim geçici bir çözüm geliştirirken dikkate alınması gereken bazı küçük farklar vardır.

ADAL.js içinde belirteçleri elde birkaç işlevleri vardır: `login()`, `acquireToken(...)`, `acquireTokenPopup(…)`, ve `acquireTokenRedirect(…)`. 

* `login()`etkileşimli bir oturum açma isteği aracılığıyla bir kimliği belirteci alır ancak (korumalı koşullu erişim web API dahil) herhangi bir hizmeti için erişim belirteçleri elde değil.  
* `acquireToken(…)`ardından sessizce hiçbir durumda UI göstermez yani bir erişim belirteci almak için kullanılabilir.  
* `acquireTokenPopup(…)`ve `acquireTokenRedirect(…)` olduğu etkileşimli bir kaynak için bir belirteç istemek için kullanılan her ikisi de anlamına gelir, her zaman göster oturum açma kullanıcı Arabirimi.

Bir uygulamanın Web API'sini çağırmak için bir erişim belirteci ihtiyacı olduğunda çalışır bir `acquireToken(…)`.  Belirteç oturum süresi doldu veya koşullu erişim ilkesi ile uyum sağlamak ihtiyacımız sonra *acquireToken* işlevi başarısız olur ve uygulama kullandığı `acquireTokenPopup()` veya `acquireTokenRedirect()`.

![ADAL akış diyagramı kullanarak tek sayfa uygulaması](media/active-directory-conditional-access-developer/spa-using-adal-scenario.png)

Şimdi bir koşullu erişim Senaryomuzda örnekle yol.  Son kullanıcı, yalnızca sitede landed ve bir oturum yok.  Biz gerçekleştirmek bir `login()` çağrı, bir kimliği olmadan çok faktörlü kimlik doğrulaması belirteci alma.  Ardından kullanıcı uygulama istek verileri bir web API gerektiren bir düğme kadardır.  Uygulamayı dener bir `acquireToken()` araması ancak kullanıcı henüz çok faktörlü kimlik doğrulama ve koşullu erişim ilkesi ile uyum sağlamak için gereksinimlerini gerçekleştirmediği bu yana başarısız.

Azure AD aşağıdaki HTTP yanıtı geri gönderir: 

```
HTTP 400; Bad Request 
error=interaction_required
error_description=AADSTS50076: Due to a configuration change made by your administrator, or because you moved to a new location, you must use multi-factor authentication to access '<Web API App/Client ID>'.
```

Catch uygulamamıza gerekiyor `error=interaction_required`.  Uygulama daha sonra ya da kullanabilirsiniz `acquireTokenPopup()` veya `acquireTokenRedirect()` aynı kaynağı.  Kullanıcı çok faktörlü kimlik doğrulaması yapmak için zorlanır. Kullanıcı çok faktörlü kimlik doğrulaması tamamlandıktan sonra uygulamayı istenen kaynak için yeni erişim belirtecini verilir.

Bu senaryo denemek için bkz: bizim [JS SPA üzerinde-adına-kodunu örnek](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca).  Bu kod örneği koşullu erişim ilkesini ve web daha önce bu senaryoyu göstermek amacıyla JS SPA ile kayıtlı API kullanır. Düzgün talep sınama işlemek ve Web API için kullanılan bir erişim belirteci almak nasıl gösterir. Alternatif olarak, genel kullanıma alma [Angular.js kod örneği](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) Açısal SPA hakkında yönergeler için


## <a name="see-also"></a>Ayrıca bkz.

* Özellikleri hakkında daha fazla bilgi edinmek için [Azure AD'de koşullu erişim](../active-directory-conditional-access-azure-portal.md).
* Daha fazla Azure AD kod örnekleri için bkz: [Github deposuna kod örnekleri](https://github.com/azure-samples?utf8=%E2%9C%93&q=active-directory). 
* ADAL SDK'ın ve erişim başvuru belgeleri hakkında daha fazla bilgi için bkz: [kitaplığı Kılavuzu](active-directory-authentication-libraries.md).
* Çok kiracılı senaryoları hakkında daha fazla bilgi için bkz: [çok kiracılı desenini kullanarak kullanıcılar oturum nasıl](active-directory-devhowto-multi-tenant-overview.md).
