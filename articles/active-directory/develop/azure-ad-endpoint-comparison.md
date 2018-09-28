---
title: Azure AD v2.0 uç noktası v1.0 uç noktası ile karşılaştırma | Microsoft Docs
description: Azure AD v2.0 uç noktası v1.0 uç nokta arasındaki farkları bilmeniz
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
ms.date: 09/27/2018
ms.author: andret
ms.reviewer: hirsin, andret
ms.custom: aaddev
ms.openlocfilehash: b75b31ddfc77be5ed651e7b8484e41a4ae73d8d8
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47406541"
---
# <a name="comparing-the-azure-ad-v20-endpoint-with-the-v10-endpoint"></a>Azure AD v2.0 uç noktası v1.0 uç noktası ile karşılaştırma

Yeni bir uygulama geliştirirken, v1.0 ve v2.0 uç noktaları arasındaki farkları bilmeniz önemlidir. Aşağıda, v2.0 uç noktası için var olan bazı sınırlamalar yanı sıra temel farklılıklar verilmiştir.

> [!NOTE]
> Tüm Azure Active Directory (Azure AD) senaryolar ve Özellikler v2.0 uç noktası tarafından desteklenir. V2.0 uç noktası kullanıyorsanız belirlemek için aşağıdaki hakkında bilgi edinin: [v2.0 sınırlamaları](#limitations).

## <a name="who-can-sign-in"></a>Kim oturum

![Kim oturum v1.0 ve v2.0 uç imzalayabilir mi](media/azure-ad-endpoint-comparison/who-can-sign-in.png)

* Uygulamanıza (Azure AD) oturum açmak yalnızca iş ve Okul hesaplarında v1.0 uç nokta sağlar

* V2.0 uç noktası, iş ve Okul hesapları Azure AD'den ve kişisel hesapları (MSA) (hotmail.com, outlook.com, msn.com) oturum açmak için sağlar.

* V1.0 hem v2.0 uç noktaları ayrıca oturum açma işlemlerini kabul *[Konuk kullanıcılar](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b)* olarak yapılandırılan uygulamalar için bir Azure AD Directory *[tek kiracılı](single-and-multi-tenant-apps.md)* veya *çok kiracılı* kiracıya özgü uç noktaya işaret edecek şekilde yapılandırılmış uygulamaların (`https://login.microsoftonline.com/{TenantId_or_Name}`).

V2.0 uç noktası, oturum açma hesaplarından, kişisel ve iş ve Okul kabul uygulamaları uygulamanızı tamamen hesabı belirsiz yazma olanağı veren yazmanızı sağlar. Örneğin, uygulamanızın çağırırsa [Microsoft Graph](https://graph.microsoft.io), bazı ek işlevler ve veriler iş hesaplarını, SharePoint sitelerinde veya dizin verilerini gibi kullanılabilir olacaktır. Ancak birçok eylemi için gibi [bir kullanıcının posta okuma](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), aynı kod hem kişisel ve iş ve Okul hesapları için e-posta erişebilirsiniz.

V2.0 uç noktası için tüketici, eğitim ve kurumsal dünyaları erişmek için tek bir kitaplığı (MSAL) kullanabilirsiniz.

 Azure AD v1.0 uç noktası, yalnızca iş ve Okul hesaplarında oturum açma işlemleri kabul eder.

## <a name="incremental-and-dynamic-consent"></a>Artımlı ve dinamik onayı

Azure AD v1.0 uç noktası kullanan uygulamalar gibi gerekli OAuth 2.0 izinlerini önceden belirtmeniz gerekir:

![İzinleri kayıt kullanıcı Arabirimi](./media/azure-ad-endpoint-comparison/app_reg_permissions.png)

Doğrudan uygulama kaydı üzerinde izinler **statik**. Uygulamanın statik izinleri Azure Portalı'nda tanımlandığı ve güzel ve basit kod tutulur, ancak geliştiriciler için bazı sorunları sunabilir:

* Uygulamayı tüm uygulama oluşturma sırasında hiç olmadığı kadar gerekir izinleri bilmeniz. Zaman içinde izinler eklemek zor bir işlemi oluştu.

* Uygulama, tüm önceden hiç olmadığı kadar eriştiğiniz kaynaklar bilmeniz gerekir. Kaynakları rastgele bir sayıdan erişebilir uygulamalar oluşturmak zordu.

* Uygulama, kullanıcının ilk oturum açma sırasında hiç olmadığı kadar gerekir tüm izinlere ilişkin istek gerekir. Bu izinleri uzun bir listesi için yol açan bazı durumlarda, son kullanıcıların, ilk oturum açma uygulamanın erişimi onaylama önerilmez.

V2.0 uç noktası ile uygulama kayıt bilgilerini Azure Portalı'nda tanımlanan statik olarak tanımlanan izinleri yoksay ve izinleri belirtmek uygulama ihtiyaçlarınızı **dinamik olarak** normal kullanımı sırasında çalışma zamanında, uygulama kayıt bilgilerini statik olarak tanımlı izinler bakılmaksızın uygulaması.

Bunu yapmak için uygulamanız gereken belirli bir anda uygulama saatinizde yeni kapsamlar dahil ederek kapsamları belirtebilirsiniz `scope` - önceden uygulama kayıt defteri bilgilerini tanımlamak için gerek kalmadan, bir erişim belirteci isteğinde bulunurken parametresi .

Kullanıcı henüz isteğine eklenen yeni kapsamlar seçtiği değil, yalnızca yeni izinler için izin istenir. Daha fazla bilgi için okuyabilirsiniz [izinler ve onay kapsamları](v2-permissions-and-consent.md).

İzin istemek için bir uygulama ile dinamik olarak izin vererek `scope` parametresi, geliştiricilerin kullanıcı deneyimi üzerinde tam denetim sağlar. İsterseniz, izninizi deneyimi ve tüm izinleri bir ilk yetkilendirme isteği için sorun ön yük seçebilirsiniz. Veya uygulamanız çok sayıda izinleri gerektiriyorsa, bunlar zaman içinde uygulamanızın belirli özelliklerini kullanma girişimi gibi bu izinleri kullanıcıdan kademeli olarak toplamak seçebilirsiniz.

Bir kuruluş adına gerçekleştirilen yönetici onayı yine de önerilen tüm adına izin vermek için bir yönetici gerekiyorsa v2.0 uç noktası kullanan uygulamalar için bu izinleri ayarlamak için bir uygulama için kayıtlı statik izinleri kullandığına dikkat edin Kuruluş. Bu uygulamanın kurulumunu kuruluş yönetici tarafından gerekli döngüleri azaltır

## <a name="scopes-not-resources"></a>Kapsamları, kaynakları değil

V1.0 uç noktası kullanan uygulamalar için bir uygulama olarak davranabilir bir **kaynak**, veya bir alıcı belirteçleri. Bir kaynak bir dizi tanımlayabilirsiniz **kapsamları** veya **oAuth2Permissions** anladığı, istemci bu kaynağa kapsamları belirli bir dizi için belirteçler istemek uygulamalar sağlar. Örnek bir kaynak olarak Azure AD Graph API'sine göz önünde bulundurun:

* Kaynak tanımlayıcısı veya `AppID URI`: `https://graph.windows.net/`

* Kapsamları veya `OAuth2Permissions`: `Directory.Read`, `Directory.Write`ve benzeri.

Tüm bu tutar v2.0 uç noktası için true. Bir uygulama yine de davranabilir kaynak olarak kapsamları tanımlamak ve bir URI tarafından tanımlanan. İstemci uygulamaları yine de bu kapsamlara erişim isteğinde bulunabilirsiniz. Ancak, bu izinleri bir istemci isteklerinin şekilde değişti. Bir OAuth 2.0 v1.0 uç noktayı yetkilendirmek için Azure AD'ye istek gibi attıktan:

```text
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https://graph.windows.net/
...
```

Burada **kaynak** parametresi belirtilen hangi kaynak yetkilendirme için istemci uygulaması istiyor. Azure AD uygulaması Azure portalında statik yapılandırmasına göre ve buna uygun olarak verilen belirteçler için gereken izinleri hesaplanan. V2.0 uç noktası kullanan uygulamalar için aynı OAuth 2.0 yetkilendirme isteği aşağıdaki gibi görünür:

```text
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https://graph.windows.net/directory.read%20https://graph.windows.net/directory.write
...
```

Burada **kapsam** parametresi hangi kaynak gösterir ve izinleri uygulama istediği için yetkilendirme. İstenen kaynak istekte hala mevcut - bunu yalnızca her kapsam parametresinin değeri kapsadığı. Bu şekilde kapsam parametresi kullanarak v2.0 uç noktası ile OAuth 2.0 belirtimini daha uyumlu olmasını sağlar ve ortak sektör uygulamalarına sahip daha yakından hizalar. Ayrıca gerçekleştirmek uygulamaları tanır [artımlı onay](#incremental-and-dynamic-consent), daha önce açıklanmıştır.

## <a name="well-known-scopes"></a>İyi bilinen kapsamları

### <a name="offline-access"></a>Çevrimdışı erişim

V2.0 uç noktası kullanan uygulamalar - uygulamalar için yeni bir bilinen izin kullanımını gerektirebilir `offline_access` kapsam. Tüm uygulamalar, bir uzun süre, kullanıcının uygulamayı etkin bir şekilde kullanırken değil, hatta bir kullanıcı adına kaynaklara gerekiyorsa bu izin istemesi gerekir. `offline_access` Kapsamı Onayı iletişim kutularının kullanıcıya görünür **verilerinizi dilediğiniz zaman erişin**, hangi kullanıcının kabul etmesi gerekir. İsteyen `offline_access` izni OAuth 2.0 refresh_tokens v2.0 uç noktasından almak üzere web uygulamanızı etkinleştirir. Yenileme belirteçleri uzun süreli ve uzun süreler erişim için yeni OAuth 2.0 erişim belirteçleri değiştirilebilir.

Uygulamanızı değil istemiyorsa `offline_access` kapsamı, yenileme belirteçleri almaz. OAuth 2.0 yetkilendirme kod akışı bir yetkilendirme kodunda şifrenizi kullandığınızda, yalnızca geri bir erişim belirteci alırsınız, yani `/token` uç noktası. Erişim belirtecinizi süreyi (genellikle bir saat) kısa bir süre için geçerli kalır, ancak sonunda sona erer. Uygulamanızı, zaman içinde nokta kullanıcıyı yeniden yönlendirmek için gereken en başa `/authorize` yeni bir yetkilendirme kodu almak için uç nokta. Bu yeniden yönlendirme sırasında kullanıcı veya kimlik bilgilerini yeniden girin veya izinler, uygulamanın türüne bağlı olarak yeniden onay gerekmez.

OAuth 2.0 hakkında daha fazla bilgi edinmek için `refresh_tokens`, ve `access_tokens`, kullanıma [v2.0 protokol başvurusu](active-directory-v2-protocols.md).

### <a name="openid-profile-and-email"></a>Openıd, profil ve e-posta

Tarihsel olarak, en temel Openıd Connect oturum açma akışını Azure AD ile çok fazla sonuç kullanıcı hakkındaki bilgilerin sağlayacağını *id_token*. Taleplerde bir *id_token* kullanıcının adı, tercih edilen kullanıcı adı, e-posta adresi, nesne kimliği ve daha fazlasını içerebilir.

Bilgi, `openid` kapsam ortaya koymaktadır, uygulamanıza erişimi, artık sınırlı. `openid` Kapsamı yalnızca kullanıcının oturum açmasını ve kullanıcı için uygulamaya özgü tanımlayıcı almak uygulamanızı sağlayacaktır. Uygulamanızdaki kullanıcı hakkındaki kişisel verileri almak istiyorsanız, uygulamanızı kullanıcıdan ilave izin istemek gerekir. İki yeni kapsamlar – `email` ve `profile` kapsamları – ilave izin istemek için sağlayacaktır.

`email` Kapsam aracılığıyla kullanıcının birincil e-posta adresi erişim sağlayan `email` id_token içinde talep. `profile` Nesne kimliği kapsam tüm diğer ilgili temel bilgileri – kullanıcıların adı, tercih edilen kullanıcı adı, kullanıcı erişim verir ve benzeri.

Bu sayede uygulamanızı açığa en az bir şekilde kod – kullanılabilmesi için uygulamanızın gerektirdiği bilgiler kümesi için yalnızca kullanıcı isteyebilir. Bu kapsamları hakkında daha fazla bilgi için bkz. [v2.0 kapsam başvurusu](v2-permissions-and-consent.md).

## <a name="token-claims"></a>Belirteç taleplerinden

V2.0 uç noktası tarafından verilen belirteçlere talep sunuldu tarafından verilen belirteçlere aynı olmayacaktır Azure AD'ye uç noktaları. Yeni hizmete geçiş uygulamalar belirli bir talep id_tokens veya access_tokens mevcut varsayımında bulunmamalıdır. Daha ayrıntılı bilgi v2.0 uç noktası kullanılan belirteçlerin farklı türdeki kullanılabilir [erişim belirteci](access-tokens.md) başvuru ve [ `id_token` başvurusu](id-tokens.md)

## <a name="limitations"></a>Sınırlamalar

V2.0 kullanırken dikkat edilmesi gereken bazı kısıtlamalar vardır.

Microsoft kimlik platformu ile tümleştirilen uygulamalar oluştururken, v2.0 uç noktasını ve kimlik doğrulama protokolleri gereksinimlerinizi karşılayıp karşılamadığını karar vermeniz gerekir. Platform ve v1.0 uç nokta hala tam olarak desteklenir ve bazı açılardan, v2.0 değerinden daha fazla özellik zengin olduğu. Ancak, v2.0 [önemli avantajlar sunuyor](azure-ad-endpoint-comparison.md) geliştiriciler için.

Geliştiriciler için bu anda basitleştirilmiş bir öneri şu şekildedir:

* Kişisel Microsoft hesapları, uygulamanızda desteklemelidir, v2.0 kullanın. Ancak bunu yapmadan önce bu makalede ele alınan sınırlamaları anladığınızdan emin olun.

* Uygulamanız yalnızca destekleyen Microsoft iş ve Okul hesapları gerekiyorsa, v2.0 kullanmayın. Bunun yerine, başvurmak [v1.0 Kılavuzu](azure-ad-developers-guide.md).

V2.0 uç noktası, böylece hiç olmadığı kadar yalnızca v2.0 uç noktası kullanmanız gerekir, burada listelenen kısıtlamalarla ortadan kaldırmak için evrilir. Bu arada, v2.0 uç noktası sizin için uygun olup olmadığını belirlemek için bu makaleyi kullanın. Bu makalede, v2.0 uç noktasının geçerli durumu yansıtacak şekilde güncelleştirmeye devam eder. Geri gereksinimlerinize göre v2.0 özellikleri yeniden değerlendirmek için denetleyin.

### <a name="restrictions-on-app-types"></a>Uygulama türleri kısıtlamaları

Şu anda aşağıdaki uygulama türlerini v2.0 uç noktası tarafından desteklenmez. Desteklenen uygulama türleri ile ilgili açıklama için bkz: [v2.0 uygulama türlerinde](v2-app-types.md).

#### <a name="standalone-web-apis"></a>Tek başına Web API'leri

V2.0 uç noktası için kullanabileceğiniz [güvenli bir Web API'si OAuth 2.0 ile oluşturma](v2-app-types.md#web-apis). Ancak, Web API'SİNİN belirteçleri aynı uygulama kimliği olan bir uygulamadan alabilirsiniz Farklı uygulama kimliği olan bir istemciden bir Web API'si erişemiyor İstemci isteği veya Web API'niz için izinleri almak mümkün olmayacaktır.

V2.0 uç noktası Web API örnekleri aynı uygulama kimliği olan bir istemciden gelen belirteçleri kabul eden bir Web API'si oluşturma hakkında bilgi için bkz [Başlarken v2.0](v2-overview.md#getting-started) bölümü.

### <a name="restrictions-on-app-registrations"></a>Uygulama kayıtları kısıtlamaları

Şu anda, v2.0 uç noktası ile tümleştirmek istediğiniz her uygulama için bir uygulama kaydı yeni oluşturmalısınız [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). Mevcut Azure AD veya Microsoft hesabı uygulamaları v2.0 uç noktası ile uyumlu değildir. Uygulama kayıt portalı dışında herhangi bir portal kayıtlı uygulamalar v2.0 uç noktası ile uyumlu değildir.

Buna ek olarak, oluşturduğunuz uygulama kayıtları [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) aşağıdaki uyarılar vardır:

* Uygulama Kimliği yalnızca iki uygulama sırrı izin verilir

* Kişisel bir Microsoft hesabı olan bir kullanıcı tarafından kaydedilen bir uygulama kaydı görüntülenebilir ve yalnızca tek bir geliştirici hesabı tarafından yönetilir. Birden fazla Geliştirici arasında paylaşılamaz. Uygulama kaydınızı birden fazla Geliştirici arasında paylaşmak istiyorsanız, uygulama kayıt portalı bir Azure AD hesabı ile oturum açarak oluşturabilirsiniz.

* İzin verilen yeniden yönlendirme URL biçimiyle ilgili çeşitli kısıtlamalar vardır. URL yeniden yönlendirme hakkında daha fazla bilgi için sonraki bölüme bakın.

### <a name="restrictions-on-redirect-urls"></a>Kısıtlamalar yeniden yönlendirme URL'leri

Uygulama kayıt Portalı'nda kayıtlı uygulamalar sınırlı bir yeniden yönlendirme URL'si değerler kümesi kısıtlanır. Yeniden yönlendirme URL'sini web uygulamaları ve Hizmetleri ile şemasıyla başlamalı `https`, ve tüm yeniden yönlendirme URL'si değerleri tek bir DNS etki alanını paylaşmalıdır. Örneğin, bunlardan birine sahip bir web uygulamasını kaydedemezsiniz yeniden yönlendirme URL'leri:

* `https://login-east.contoso.com`  
* `https://login-west.contoso.com`

Kayıt sistemi mevcut yeniden yönlendirme URL'sini yeniden yönlendirme URL'sini eklemekte olduğunuz DNS adını tam DNS adı ile karşılaştırır. Aşağıdaki koşullardan biri geçerli olduğunda DNS adı ekleme isteği başarısız olur:  

* Yeni yeniden yönlendirme URL'sinin tam DNS adı mevcut bir yeniden yönlendirme URL'sinin DNS adı ile eşleşmiyor.

* Yeni yeniden yönlendirme URL'sinin tam DNS adı mevcut bir yeniden yönlendirme URL'sinin alt etki alanı değil.

Örneğin, uygulama bu yeniden yönlendirme URL'si şu ise:

`https://login.contoso.com`

Aşağıdaki gibi ekleme yapabilirsiniz:

`https://login.contoso.com/new`

Bu durumda, DNS adı tam olarak eşleşir. Ya da şunu yapabilirsiniz:

`https://new.login.contoso.com`

Bu durumda, login.contoso.com DNS alt etki alanına başvurursunuz. Sahip olan bir uygulama istiyorsanız `login-east.contoso.com` ve `login-west.contoso.com` URL yeniden yönlendirme olarak, bu yeniden yönlendirme URL'lerini şu sırayla eklemeniz gerekir:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

İlk yönlendirme URL'sinin alt etki alanlarını olduklarından, sonraki iki ekleyebilirsiniz contoso.com. Bu sınırlama, gelecek bir sürümde kaldırılacak.

Ayrıca unutmayın, belirli bir uygulama için yalnızca 20 yanıt URL'leri olabilir.

Uygulama kayıt Portalı'nda bir uygulamayı kaydetme hakkında bilgi için bkz: [v2.0 uç noktası ile bir uygulamayı kaydetme](quickstart-v2-register-an-app.md).

### <a name="restrictions-on-libraries-and-sdks"></a>Kitaplıklarına ve Sdk'lara genel kısıtlamalar

Şu anda, v2.0 uç noktası için kitaplık desteği sınırlıdır. V2.0 uç noktası, bir üretim uygulamasında kullanmak istiyorsanız, bu seçenekler vardır:

* Bir web uygulaması derliyorsanız, oturum açma ve belirteç doğrulama gerçekleştirmek için genel kullanıma açık Microsoft sunucu tarafı ara yazılımı güvenli bir şekilde kullanabilirsiniz. Bunlar, ASP.NET ve Node.js Passport eklentisi için OWIN Open ID Connect ara yazılımını içerir. Microsoft Ara yazılımında kullanan kod örnekleri için bkz. [Başlarken v2.0](v2-overview.md#getting-started) bölümü.

* Masaüstü veya mobil bir uygulama oluşturuyorsanız Microsoft kimlik doğrulama kitaplığı (MSAL) önizleme birini kullanabilirsiniz. Bu kitaplıklar üretim desteklenen bir önizleme aşamasında olduğundan, bunları üretim uygulamalarında kullanmak daha güvenlidir. Daha fazla Önizleme kullanılabilir kitaplıkları ve terimler hakkında [kimlik doğrulama kitaplıkları başvuru](reference-v2-libraries.md).

* Microsoft kitaplıkları tarafından kapsamında olmayan platformlar için doğrudan uygulama kodunuzda protokolü ileti alma ve gönderme tarafından v2.0 uç noktası ile tümleştirebilirsiniz. V2.0 Openıd Connect ve OAuth protokolleri [açıkça belirtilmiştir](active-directory-v2-protocols.md) bu tür bir tümleştirme gerçekleştirmenizi sağlayacak.

* Son olarak, v2.0 uç noktası ile tümleştirmek için açık kaynak açın ID Connect ve OAuth kitaplıklarını kullanabilirsiniz. V2.0 protokol önemli değişiklikler olmadan birçok açık kaynak Protokolü kitaplıkları ile uyumlu olmalıdır. Bu tür kitaplıkları kullanılabilirliğini, dil ve platforma göre değişir. [Open ID Connect](http://openid.net/connect/) ve [OAuth 2.0](http://oauth.net/2/) Web siteleri korumak popüler uygulamaları listesi. Daha fazla bilgi için [Azure Active Directory v2.0 ve kimlik doğrulama kitaplıkları](reference-v2-libraries.md), açık kaynak istemci kitaplıkları ve v2.0 uç noktası ile test edilmiştir örnekleri listesi.

* Başvuru için `.well-known` v2.0 ortak uç nokta için uç nokta `https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration` .  Değiştirin `common` kiracınıza belirli verileri almak için Kiracı kimliği.  

### <a name="restrictions-on-protocols"></a>Protokolleri kısıtlamaları

V2.0 uç noktası, SAML veya WS-Federation desteklemez; yalnızca, Open ID Connect ve OAuth 2.0 de destekler. Tüm özellikler ve yetenekler OAuth kurallarının v2.0 uç noktasında eklenmiştir.

Şu anda aşağıdaki protokolü özellikleri ve yetenekleri olan *kullanılamıyor* veya *desteklenmiyor* v2.0 uç noktasını:

* `email` Talep yalnızca döndürülen isteğe bağlı bir talep yapılandırılır ve kapsam kapsam = e-posta, istekte belirtildi. Bununla birlikte, daha da Open ID Connect ve OAuth2.0 standartlarına uymak için v2.0 uç noktası güncelleştirildikçe değiştirmek için bu davranışı unutmayın.

* V2.0 uç noktası kimliği belirteçler veren bir rol veya grup talepleri desteklemez.

* V2.0 uç noktası desteklemediği [OAuth 2.0 kaynak sahibi parola kimlik bilgileri verme](https://tools.ietf.org/html/rfc6749#section-4.3).

V2.0 uç noktasına desteklenen protokol işlevselliği daha iyi anlamak için okuyun bizim [Openıd Connect ve OAuth 2.0 protokolü başvurusu](active-directory-v2-protocols.md).

#### <a name="saml-restrictions"></a>SAML kısıtlamaları

Windows uygulamalarında Active Directory Authentication Library (ADAL) kullandıysanız, güvenlik onaylama işlemi biçimlendirme dili (SAML) onay verme kullanan Windows tümleşik kimlik doğrulaması avantajlarından geçen. Bu izinle, kullanıcıları Azure AD'ye Federasyon kiracıları sessizce kimlik doğrulaması gerçekleştirmenin kendi şirket içi Active Directory örneği ile kimlik bilgilerini girmeden. Şu anda, SAML onay verme v2.0 uç noktada desteklenmiyor.
