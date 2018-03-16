---
title: "Azure AD v2.0 uç farklı nedir? | Microsoft Docs"
description: "Özgün Azure AD arasında bir karşılaştırma ve v2.0 uç noktaları."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mtillman
editor: 
ms.assetid: 5060da46-b091-4e25-9fa8-af4ae4359b6c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 502bfa128422a029878513d6aa4533718bdddbb5
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="whats-different-about-the-v20-endpoint"></a>V2.0 uç noktası hakkında farklı nedir?
Azure Active Directory ile tanıdık veya uygulamaları geçmişte Azure AD ile tümleşik değil beklediğiniz v2.0 uç noktası bazı farklılıklar olabilir.  Bu belge, anlamak için bu farkları çıkışı çağırır.

> [!NOTE]
> Tüm Azure Active Directory senaryolarını ve özelliklerini v2.0 uç noktası tarafından desteklenir.  V2.0 uç kullanmanızın gerekli olup olmadığını belirlemek için okuyun [v2.0 sınırlamaları](active-directory-v2-limitations.md).
>

## <a name="microsoft-accounts-and-azure-ad-accounts"></a>Microsoft ve Azure AD hesapları
V2.0 uç geliştiriciler, oturum açma tek kimlik doğrulama uç noktası kullanarak, Microsoft Accounts ve Azure AD hesaplarının kabul eden uygulamalar yazmayı izin verir.  Bu, uygulamanızın tümüyle hesap belirsiz yazma yeteneği verir; Bu tür bir hesap ile oturum açtığında kullanmayan olabilir.  Elbette, *yapabilirsiniz* uygulamanızı belirli bir oturumda kullanılan hesap türünü farkında olun, ancak zorunda değilsiniz.

Örneğin, uygulamanızın çağırırsa [Microsoft Graph](https://graph.microsoft.io), bazı ek işlevler ve veri SharePoint sitelerinde veya dizin verilerini gibi kurumsal kullanıcılar için kullanılabilir.  Ancak birçok eylemler gibi [bir kullanıcının posta okuma](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), kodu tam olarak aynı Microsoft Accounts ve Azure AD hesapları için yazılabilir.  

Uygulamanızı Microsoft Accounts ve Azure AD hesapları ile tümleştirme şimdi bir basit bir işlemdir.  Tüketici ve enterprise dünyaları erişmek için tek bir uç noktaları, tek bir kitaplık ve tek uygulama kayıt kümesi kullanabilirsiniz.  V2.0 uç noktası hakkında daha fazla bilgi için kullanıma [genel bakış](active-directory-appmodel-v2-overview.md).

## <a name="new-app-registration-portal"></a>Yeni uygulama kayıt portalı
V2.0 uç noktası ile çalışan bir uygulama kaydetmek için yeni bir uygulama kayıt portalı kullanın: [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Bu burada edinebilirsiniz bir uygulama kimliği portalıdır uygulamanızın oturum açma sayfası ve daha fazlasını görünümünü özelleştirin.  Tüm portalına erişmek için gereken desteklenen Microsoft hesabı - kişisel veya iş/Okul hesap budur.

## <a name="one-app-id-for-all-platforms"></a>Tüm platformlar için bir uygulama kimliği
Azure Active Directory kullandıysanız, büyük olasılıkla birkaç farklı uygulamaları tek bir proje için kaydınız.  Bir Web sitesi ve bir iOS uygulaması oluşturulduysa, örneğin, ayrı olarak kaydetmek iki farklı uygulama kimliği kullanarak içeriyor. Azure AD uygulama kayıt Portalı'nı kayıt sırasında Bu ayrım yapmanıza zorla:

![Eski uygulama kaydı kullanıcı Arabirimi](../../media/active-directory-v2-flows/old_app_registration.PNG)

Bir Web sitesi ve web API'si arka uç olsaydı, benzer şekilde, her ayrı bir uygulama olarak Azure AD içinde kaydettiğiniz.  Veya bir iOS uygulaması ve bir Android uygulaması varsa, aynı zamanda iki farklı uygulamalar kaydettiğiniz.  Her bir uygulama bileşeninin kaydetme bazı beklenmeyen davranışları, geliştiriciler ve müşterileri için neden:

* Her müşterinin Azure Active Directory kiracısı içinde ayrı bir uygulama olarak her bileşenin görüldü.
* Erişimi yönetmek veya bir uygulamayı silmek için ilkeyi uygulamak bir kiracı Yöneticisi çalışırken her uygulama bileşeni için yapmanız gerekir.
* Bir uygulamaya müşteriler rıza, her bileşen farklı bir uygulama olarak onay ekranında görüntülenir.

V2.0 uç noktası ile artık projenizin tüm bileşenleri tek uygulama kaydı kaydetmek ve tek bir uygulama kimliği projeniz için kullanın.  Her bir proje için birkaç "Platform" ekleyin ve eklediğiniz her platform için uygun veri sağlar.  Elbette, gereksinimlerinize bağlı olarak ister, ancak yalnızca bir uygulama kimliği çoğu durumda gerekli olması gereken şekilde sayıda uygulamaları oluşturabilirsiniz.

Bizim AIM bu bir daha Basitleştirilmiş uygulama yönetimi ve geliştirme deneyimi sağlama, üzerinde çalışabilir tek bir proje, fazla birleştirilmiş bir görünümünü oluşturmak olmasıdır.

## <a name="scopes-not-resources"></a>Kapsamları, kaynakları değil
Azure Active Directory'de bir uygulama olarak davranabilir bir **kaynak**, veya bir alıcının belirteçleri.  Bir kaynak bir dizi tanımlayabilirsiniz **kapsamları** veya **oAuth2Permissions** anladığı, istemci bu kaynağa kapsamları belirli bir dizi için belirteçler istemek uygulamaları sağlar.  Örnek bir kaynak olarak Azure AD Graph API göz önünde bulundurun:

* Kaynak tanımlayıcısı veya `AppID URI`: `https://graph.windows.net/`
* Kapsamları veya `OAuth2Permissions`: `Directory.Read`, `Directory.Write`vb.  

Tüm bu tutar v2.0 uç noktası için true.  Bir uygulamayı hala davranabilir kaynak olarak kapsamları tanımlamak ve bir URI tarafından tanımlanan.  İstemci uygulamaları hala bu kapsamlara erişim isteğinde bulunabilirsiniz.  Ancak, bir istemci bu izinleri ister şekilde değiştirdi.  Geçmişte, OAuth 2.0 yetkilendirme isteği Azure ad Aranan gibi:

```
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https%3A%2F%2Fgraph.windows.net%2F
...
```

Burada **kaynak** parametresi belirtilen hangi kaynak yetkilendirme için istemci uygulaması istiyor.  Azure AD, Azure Portal'da statik yapılandırmasını temel alarak ve verilen belirteçler buna göre uygulama tarafından gerekli izinleri hesaplanır.  Şimdi, aynı OAuth 2.0 isteği görülüyor yetkilendirin.

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

Burada **kapsam** parametresi hangi kaynak gösterir ve izinleri uygulamanın istediği yetkilendirme için. İstenen kaynak hala çok istekte - bunu yalnızca her kapsam parametresinin değeri ileride olan.  Bu şekilde kapsam parametresi kullanarak v2.0 uç noktası ile OAuth 2.0 belirtimi daha uyumlu olmasını sağlar ve daha yakından ortak endüstri yöntemlerle hizalar.  Ayrıca gerçekleştirmek uygulamaları sağlar [artımlı izin](#incremental-and-dynamic-consent), sonraki bölümde açıklanmaktadır.

## <a name="incremental-and-dynamic-consent"></a>Artımlı ve dinamik onayı
Uygulama oluşturma sırasında Azure Portalı'nda gerekli OAuth 2.0 izinlerini belirlemek için önceden gereken Azure AD'de uygulamalar kayıtlı:

![İzinleri kayıt kullanıcı Arabirimi](../../media/active-directory-v2-flows/app_reg_permissions.PNG)

Gerekli bir uygulama yapılandırıldı izinleri **statik olarak**.  Bu yapılandırma Azure Portalı'nda mevcut uygulamanın izin verilen ve iyi ve basit kod tutulan olsa da, geliştiriciler için bazı sorunları gösterir:

* Bir uygulama, uygulama oluşturma sırasında herhangi bir zamanda gerekir tüm izinleri bilmeniz gerekiyordu.  Zaman içinde izinlerini eklemek zor bir işlem.
* Bir uygulama herhangi bir zamanda önceden erişir kaynakların tümünü biliyorsanız gerekiyordu.  Rastgele bir kaynak sayısı erişebilir uygulamaları oluşturmak zordu.
* Şimdiye kadar kullanıcının ilk oturum açma sırasında gerekir tüm izinleri istemek bir uygulama içeriyor.  Bazı durumlarda bu ilk oturum açma uygulamanın erişimi onaylama gelen son kullanıcılar önermez izinleri çok uzun bir listesi kılavuzluk sağlar.

V2.0 uç noktası ile izinleri belirtebilirsiniz uygulama gereksinimlerinizi **dinamik olarak**, uygulamanızın normal kullanım sırasında çalışma zamanında.  Bunu yapmak için uygulamanız gereken belirli bir anda zamanında bunları dahil ederek kapsamları belirtebilirsiniz `scope` bir yetkilendirme isteği parametresinin:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

Bir Azure AD kullanıcının dizin verilerini yanı sıra, dizinlerini veri yazma okumak uygulama için yukarıdaki istekleri izni.  Kullanıcı bu belirli uygulama için geçmiş bu izni verdiği, yalnızca kendi kimlik bilgilerini girer ve uygulamada oturum açmanız.  Kullanıcı bu izinleri hiçbirine seçtiği değil, v2.0 uç noktası kullanıcı bu izinler için izin sorar.  Daha fazla bilgi için okuyabilirsiniz [izinleri, onay ve kapsamları](active-directory-v2-scopes.md).

Bir uygulamanın dinamik olarak aracılığıyla izinleri istemek için verme `scope` parametresi, kullanıcı deneyimi üzerinde tam denetim verir.  İsterseniz, onayınızı deneyimi ve bir başlangıç yetkilendirme isteği tüm izinleri isteyin frontload seçebilirsiniz.  Veya, uygulamanız çok sayıda izinleri gerektiriyorsa, bunlar zaman içinde uygulamanızın belirli özellikleri kullanma girişimi gibi bu izinleri kullanıcıdan artımlı olarak, toplamak seçebilirsiniz.

## <a name="well-known-scopes"></a>İyi bilinen kapsamları
#### <a name="offline-access"></a>Çevrimdışı erişim
V2.0 uç noktası kullanan uygulamalar için uygulama - yeni bir iyi bilinen izin kullanımını gerektirebilir `offline_access` kapsam.  Tüm uygulamalar, bir uzun süren süre, kullanıcının uygulamayı etkin olarak kullanan olmayan zamanlarda bile bir kullanıcı adına kaynaklara erişmek gerekiyorsa bu izni istemek gerekir.  `offline_access` Kapsam görünür kullanıcı onay iletişim kutuları için "Verilerinizi çevrimdışı erişim", hangi kullanıcı kabul etmeniz gerekir.  İsteyen `offline_access` izni OAuth 2.0 refresh_tokens v2.0 uç noktasından almak için web uygulamanızı etkinleştirir.  Refresh_tokens uzun süreli ve uzun süreler erişim için yeni OAuth 2.0 access_tokens değiştirilebilir.  

Uygulamanızı değil istemiyorsa `offline_access` kapsam refresh_tokens almaz.  Bu bir authorization_code OAuth 2.0 yetkilendirme kodu akışı içinde kullanmak, yalnızca geri gelen bir access_token alacağınız anlamına gelir `/token` uç noktası.  Bu access_token saati (genellikle bir saat) kısa bir süre için geçerli kalır, ancak sonunda dolacaktır.  Kullanıcıyı yeniden yönlendirmek için bir noktaya, uygulamanız gereken at başa `/authorize` yeni authorization_code almak için uç nokta.  Bu yeniden yönlendirme sırasında kullanıcı veya kullanıcının kimlik bilgilerini yeniden girmesi veya izinler için yeniden onayı bağlı olarak gerek kalmaz uygulamanın türü.

OAuth 2.0, refresh_tokens ve access_tokens hakkında daha fazla bilgi için kullanıma [v2.0 protokol başvurusu](active-directory-v2-protocols.md).

#### <a name="openid-profile-and-email"></a>Openıd, profili ve e-posta
Tarihsel olarak, en temel Openıd Connect oturum açma akışını Azure Active Directory ile çok sayıda sonuç id_token kullanıcı hakkında bilgi sağlar.  Bir id_token talepleri, kullanıcının adı, tercih edilen kullanıcı adı, e-posta adresi, nesne kimliği ve daha fazlasını içerebilir.

Biz şimdi bilgileri kısıtlayarak, `openid` kapsam uygulama erişiminizi ortaya koymaktadır.  'Openıd' kapsamı yalnızca kullanıcı oturum sağlamak ve kullanıcı için uygulamaya özgü tanımlayıcıyı alır.  Uygulamanızda kullanıcıyla ilgili kişisel bilgileri (PII) elde etmek istiyorsanız, uygulamanızı kullanıcıdan ek izinler istemeniz gerekir.  İki yeni kapsamlar – sunuyoruz `email` ve `profile` , bunu yapmak izin kapsamları –.

`email` Kapsamı çok basit – kullanıcının birincil e-posta adresi uygulama erişiminizi sağlayan `email` id_token talep.  `profile` Kapsam ortaya koymaktadır tüm diğer ilgili temel bilgileri kendi adı, tercih edilen kullanıcı adı, bir kullanıcının, uygulama erişimini nesne kimliği ve benzeri.

Bu sayede uygulamanız açığa en az bir şekilde kod – uygulamanızı işini yapmak için gerektirdiği bilgiler kümesi için yalnızca kullanıcı sorabilirsiniz.  Bu kapsamları hakkında daha fazla bilgi için başvurmak [v2.0 kapsam başvurusu](active-directory-v2-scopes.md).

## <a name="token-claims"></a>Belirteç talep
V2.0 uç noktası tarafından yayınlanan belirteçleri Taleplerde genel olarak kullanılabilir tarafından yayınlanan belirteçleri aynı olmayacak Azure AD uç noktaları - uygulamaları yeni Service'e geçirilmesi değil varsayın belirli bir talep id_tokens veya access_tokens var. V2.0 belirteçleri yayılan belirli talepler hakkında bilgi edinmek için [v2.0 belirteç başvurusu](active-directory-v2-tokens.md).

## <a name="limitations"></a>Sınırlamalar
V2.0 noktası kullanırken dikkat edilmesi gereken bazı kısıtlamalar vardır.  Lütfen [v2.0 sınırlamaları belge](active-directory-v2-limitations.md) kısıtlamalarının hiçbirini özel senaryonuz için geçerli değilse görmek için.
