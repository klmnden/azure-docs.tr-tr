---
title: Azure AD v2.0 uç noktasında farklı nedir? | Microsoft Docs
description: Özgün Azure AD arasında bir karşılaştırma ve v2.0 uç noktaları.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 5060da46-b091-4e25-9fa8-af4ae4359b6c
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: celested
ms.reviewer: elisol, jmprieur, hirsin
ms.custom: aaddev
ms.openlocfilehash: 0e344f6e9dfee3793320dc9cb79e3231c2eeda87
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39581921"
---
# <a name="whats-different-about-the-v20-endpoint"></a>V2.0 uç nokta hakkında farklı nedir?

Azure Active Directory (Azure AD) ile ilgili bilgi sahibi ya da daha önce Azure AD ile tümleştirilmiş uygulamalar, beklenmedik v2.0 uç noktası bazı farklılıklar vardır. Bu makalede farklılıkların anlamanız için çağırır.

> [!NOTE]
> Tüm Azure AD senaryolar ve Özellikler v2.0 uç noktası tarafından desteklenir. V2.0 uç noktası kullanıyorsanız belirlemek için aşağıdaki hakkında bilgi edinin: [v2.0 sınırlamaları](active-directory-v2-limitations.md).
>

## <a name="microsoft-accounts-and-azure-ad-accounts"></a>Microsoft hesapları ve Azure AD hesapları

V2.0 uç noktası, geliştiricilere, oturum açma kullanarak tek bir kimlik doğrulama uç noktası, hem Microsoft Accounts hem de Azure AD'den hesaplardan gelen kabul eden uygulamalar yazma olanağı sağlar. Bu uygulama ile kullanıcı oturum açtığı hesabı türünün ignorant olabilir yani tamamen hesabı depolamadan bağımsız, uygulama yazma olanağı sağlar. Uygulamanızı belirli bir oturumda kullanılan hesap türüne farkında yapabilirsiniz, ancak gerek yoktur.

Örneğin, uygulamanızın çağırırsa [Microsoft Graph](https://graph.microsoft.io), bazı ek işlevler ve veri SharePoint sitelerinde veya dizin verilerini gibi işletme kullanıcıları için kullanılabilir duruma gelir. Ancak birçok eylemi için gibi [bir kullanıcının posta okuma](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), tıpkı Microsoft Accounts hem Azure AD hesapları için kod yazılabilir. 

Uygulamanızı Microsoft Accounts ve Azure AD hesapları ile tümleştirme artık bir basit bir işlemdir. Uç noktaları, tek bir kitaplık ve tek uygulama kaydı tek bir kümesi tüketici ve kurumsal dünyaları erişim elde etmek için kullanabilirsiniz. V2.0 uç noktası hakkında daha fazla bilgi edinmek için kullanıma [genel](active-directory-appmodel-v2-overview.md).

## <a name="new-app-registration-portal"></a>Yeni uygulama kayıt portalı

V2.0 uç noktası ile çalışan bir uygulamayı kaydetmek için Microsoft uygulama kayıt portalı kullanmanız gerekir: [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). Burada elde edebilirsiniz bir uygulama kimliği, portal budur ve uygulamanızın oturum açma sayfasını özelleştirme. Portala erişmek için ihtiyacınız olan bir desteklenen Microsoft hesabıdır - kişisel veya iş/Okul hesabı.

## <a name="one-app-id-for-all-platforms"></a>Tüm platformlar için bir uygulama kimliği

Azure AD kullandıysanız, birkaç farklı uygulamaları tek bir proje için büyük olasılıkla kaydınız. Bir Web sitesi hem de iOS uygulama oluşturulduysa, örneğin, ayrı olarak kaydetmek iki farklı uygulama kimliği kullanarak vardı. Azure AD uygulama kayıt portalı kayıt sırasında Bu ayrım yapmak için zorunlu:

![Eski uygulama kaydı kullanıcı Arabirimi](./media/azure-ad-endpoint-comparison/old_app_registration.PNG)

Bir Web sitesi ve web API arka ucu varsa, benzer şekilde, her ayrı bir uygulama olarak Azure AD'de kayıtlı. Veya bir iOS uygulaması ve bir Android uygulaması olsaydı, ayrıca iki farklı uygulamaları kayıtlı. Uygulama'nın her bileşeninin kaydetme, geliştiriciler ve müşterileri için bazı beklenmeyen davranışlara neden:

* Her müşteri Azure AD kiracısında ayrı bir uygulama olarak her bileşenin görüntülenmektedir.
* Bir kiracı yönetici erişimini yönetin veya bir uygulamayı silmek için ilkeyi uygulamak çalışırken, her bir uygulama bileşeni için yapmanız gerekir.
* Müşteriler tarafından onaylanan bir uygulamaya, her bir bileşeni ayrı bir uygulama olarak onay ekranında görüntülenir.

V2.0 uç noktası ile artık bir tek bir uygulama kaydı projenizin tüm bileşenleri kaydetmek ve projeniz için tek bir uygulama kimliği kullanın. Birkaç "platforms" her projeye ekleme ve eklediğiniz her platform için uygun veri sağlar. Elbette, gereksinimlerinize bağlı olarak istediğiniz, ancak çoğu için yalnızca bir uygulama kimliği gerekli olmamalıdır gerektiği kadar uygulama oluşturabilirsiniz.

Bizim amacı, bu daha Basitleştirilmiş uygulama yönetimi ve geliştirme deneyimi için sağlama ve tek bir proje üzerinde çalışıyor, daha fazla birleştirilmiş bir görünümünü oluşturmak, ' dir.

## <a name="scopes-not-resources"></a>Kapsamları, kaynakları değil

Azure AD'de bir uygulama olarak davranabilir bir **kaynak**, veya bir alıcı belirteçleri. Bir kaynak bir dizi tanımlayabilirsiniz **kapsamları** veya **oAuth2Permissions** anladığı, istemci bu kaynağa kapsamları belirli bir dizi için belirteçler istemek uygulamalar sağlar. Örnek bir kaynak olarak Azure AD Graph API'sine göz önünde bulundurun:

* Kaynak tanımlayıcısı veya `AppID URI`: `https://graph.windows.net/`
* Kapsamları veya `OAuth2Permissions`: `Directory.Read`, `Directory.Write`vb. 

Tüm bu tutar v2.0 uç noktası için true. Bir uygulama yine de davranabilir kaynak olarak kapsamları tanımlamak ve bir URI tarafından tanımlanan. İstemci uygulamaları yine de bu kapsamlara erişim isteğinde bulunabilirsiniz. Ancak, bu izinleri bir istemci isteklerinin şekilde değişti. Geçmişte, OAuth 2.0 yetkilendirme isteği Azure AD'ye baktığı gibi:

```
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https%3A%2F%2Fgraph.windows.net%2F
...
```

Burada **kaynak** parametresi belirtilen hangi kaynak yetkilendirme için istemci uygulaması istiyor. Azure AD uygulaması Azure portalında statik yapılandırmasına göre ve buna uygun olarak verilen belirteçler için gereken izinleri hesaplanan. Artık, aynı OAuth 2.0 yetkilendirme isteği aşağıdaki gibi görünür:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

Burada **kapsam** parametresi hangi kaynak gösterir ve izinleri uygulama istediği için yetkilendirme. İstenen kaynak istekte hala mevcut - bunu yalnızca her kapsam parametresinin değeri kapsadığı. Bu şekilde kapsam parametresi kullanarak v2.0 uç noktası ile OAuth 2.0 belirtimini daha uyumlu olmasını sağlar ve ortak sektör uygulamalarına sahip daha yakından hizalar. Ayrıca gerçekleştirmek uygulamalar sağlar [artımlı onay](#incremental-and-dynamic-consent), sonraki bölümde açıklanmıştır.

## <a name="incremental-and-dynamic-consent"></a>Artımlı ve dinamik onayı

Uygulamalar, daha önce gerekli OAuth 2.0 izinlerini Azure portalında uygulama oluşturma zamanında belirtmek için gerekli Azure AD'de kayıtlı:

![İzinleri kayıt kullanıcı Arabirimi](./media/azure-ad-endpoint-comparison/app_reg_permissions.PNG)

Bir uygulamayı gerekli yapılandırılan izinler **statik olarak**. Bu yapılandırma Azure portalında mevcut uygulamanın izin verilen ve güzel ve basit kod tutulur, ancak geliştiriciler için bazı sorunları gösterir:

* Uygulama, tüm uygulama oluşturma sırasında hiç olmadığı kadar gerekir izinleri bilmeniz gerekiyordu. Zaman içinde izinler eklemek zor bir işlemi oluştu.
* Bir uygulamayı tüm önceden hiç olmadığı kadar eriştiğiniz kaynaklar bilmeniz gerekiyordu. Kaynakları rastgele bir sayıdan erişebilir uygulamalar oluşturmak zordu.
* Kullanıcının ilk oturum açma sırasında hiç olmadığı kadar gerekir tüm izinleri istemek bir uygulama vardı. Bu izinleri uzun bir listesi için yol açan bazı durumlarda, son kullanıcıların, ilk oturum açma uygulamanın erişimi onaylama önerilmez.

V2.0 uç noktası ile izinleri belirtebilir uygulama ihtiyaçlarınızı **dinamik olarak**, uygulamanızın normal kullanım sırasında zamanında. Bunu yapmak için uygulamanızı gereken belirli bir anda süre de dahil ederek kapsamları belirtebilirsiniz `scope` bir yetkilendirme isteği parametresinin:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

Bir Azure AD kullanıcının dizin verilerini yanı sıra veri kendi dizinine yazma okumak uygulamayı yukarıdaki istekleri izni. Kullanıcı bu belirli bir uygulama için daha önce bu izinleri onaylamasını, kimlik bilgilerini girer ve uygulamada oturum açan. Kullanıcının tüm bu izinleri olmayan etmişse, v2.0 uç noktası kullanıcı bu izinler için izin ister. Daha fazla bilgi için okuyabilirsiniz [izinler ve onay kapsamları](v2-permissions-and-consent.md).

İzin istemek için bir uygulama ile dinamik olarak izin vererek `scope` parametresi kullanıcı deneyimi üzerinde tam denetim sağlar. İsterseniz, izninizi deneyimi ve tüm izinleri bir ilk yetkilendirme isteği için sorun frontload seçebilirsiniz. Veya uygulamanız çok sayıda izinleri gerektiriyorsa, bunlar zaman içinde uygulamanızın belirli özelliklerini kullanma girişimi gibi bu izinleri kullanıcıdan kademeli olarak toplamak seçebilirsiniz.

## <a name="well-known-scopes"></a>İyi bilinen kapsamları

### <a name="offline-access"></a>Çevrimdışı erişim

V2.0 uç noktası kullanan uygulamalar - uygulamalar için yeni bir bilinen izin kullanımını gerektirebilir `offline_access` kapsam. Tüm uygulamalar, bir uzun süre, kullanıcının uygulamayı etkin bir şekilde kullanırken değil, hatta bir kullanıcı adına kaynaklara gerekiyorsa bu izin istemesi gerekir. `offline_access` Kapsamı görünür Onayı iletişim kutularının kullanıcıya "Verilerinizi çevrimdışı erişim", kullanıcının kabul etmesi gerekir. İsteyen `offline_access` izni OAuth 2.0 refresh_tokens v2.0 uç noktasından almak üzere web uygulamanızı etkinleştirir. Refresh_tokens uzun süreli ve uzun süre erişim için yeni bir OAuth 2.0 access_tokens değiştirilebilir. 

Uygulamanızı değil istemiyorsa `offline_access` kapsam refresh_tokens almaz. OAuth 2.0 yetkilendirme kod akışı, bir authorization_code şifrenizi kullandığınızda, yalnızca geri gelen bir access_token alırsınız, yani `/token` uç noktası. Bu access_token süreyi (genellikle bir saat) kısa bir süre için geçerli kalır, ancak sonunda sona erer. Uygulamanızı, zaman içinde nokta kullanıcıyı yeniden yönlendirmek için gereken en başa `/authorize` yeni authorization_code almak için uç nokta. Bu yeniden yönlendirme sırasında kullanıcı veya kimlik bilgilerini yeniden girin veya izinler, uygulamanın türüne bağlı olarak yeniden onay gerekmez.

OAuth 2.0 ve refresh_tokens access_tokens hakkında daha fazla bilgi edinmek için kullanıma [v2.0 protokol başvurusu](active-directory-v2-protocols.md).

### <a name="openid-profile-and-email"></a>Openıd, profil ve e-posta

Tarihsel olarak, en temel Openıd Connect oturum açma akışını Azure AD ile çok fazla sonuç id_token kullanıcı hakkında bilgi sağlar. Bir id_token talep kullanıcının adı, tercih edilen kullanıcı adı, e-posta adresi, nesne kimliği ve daha fazlasını içerebilir.

Bilgi, `openid` kapsam ortaya koymaktadır, uygulamanıza erişimi, artık sınırlı. `openid` Kapsamı yalnızca kullanıcının oturum açmasını ve kullanıcı için uygulamaya özgü tanımlayıcı almak uygulamanızı sağlayacaktır. Uygulamanızdaki kullanıcı hakkındaki kişisel verileri almak istiyorsanız, uygulamanızı kullanıcıdan ilave izin istemek gerekir. İki yeni kapsamlar – `email` ve `profile` kapsamları – ilave izin istemek için sağlayacaktır.

`email` Kapsam aracılığıyla kullanıcının birincil e-posta adresi erişim sağlayan `email` id_token içinde talep. 

`profile` Nesne kimliği kapsam tüm diğer ilgili temel bilgileri – kullanıcıların adı, tercih edilen kullanıcı adı, kullanıcı erişim verir ve benzeri.

Bu sayede uygulamanızı açığa en az bir şekilde kod – kullanılabilmesi için uygulamanızın gerektirdiği bilgiler kümesi için yalnızca kullanıcı isteyebilir. Bu kapsamları hakkında daha fazla bilgi için bkz. [v2.0 kapsam başvurusu](v2-permissions-and-consent.md).

## <a name="token-claims"></a>Belirteç Talepleri

V2.0 uç noktası tarafından verilen belirteçlere talep sunuldu tarafından verilen belirteçlere aynı olmayacaktır Azure AD'ye uç noktaları. Yeni hizmete geçiş uygulamalar belirli bir talep id_tokens veya access_tokens mevcut varsayımında bulunmamalıdır. Kullanıcının belirli taleplerini v2.0 belirteçlerinde yayılan hakkında bilgi edinmek için bkz. [v2.0 belirteç başvurusu](v2-id-and-access-tokens.md).

## <a name="limitations"></a>Sınırlamalar

V2.0 noktası kullanırken dikkat edilmesi gereken bazı kısıtlamalar vardır. Bu kısıtlamaların hiçbirini özel senaryonuz için geçerli olup olmadığına bilgi edinmek için [v2.0 sınırlamaları doc](active-directory-v2-limitations.md).
