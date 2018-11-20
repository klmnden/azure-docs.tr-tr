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
ms.date: 11/17/2018
ms.author: celested
ms.reviewer: hirsin, andret, jmprieur, sureshja, jesakowi, lenalepa, kkrishna, dadobali, negoe
ms.custom: aaddev
ms.openlocfilehash: 3e9765bf2c6b746b892f7fbc97ea3124f80d772e
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2018
ms.locfileid: "51976019"
---
# <a name="comparing-the-azure-ad-v20-endpoint-with-the-v10-endpoint"></a>Azure AD v2.0 uç noktası v1.0 uç noktası ile karşılaştırma

Yeni bir uygulama geliştirirken, Azure Active Directory (Azure AD) v1.0 ve v2.0 uç noktaları arasındaki farkları bilmeniz önemlidir. Bu makalede uç noktaları ve v2.0 uç noktası için var olan bazı sınırlamalar arasındaki temel farklar ele alınmaktadır.

> [!NOTE]
> V2.0 uç noktası, tüm Azure AD senaryoları ve özellikleri desteklemiyor. V2.0 uç noktası kullanıyorsanız belirlemek için aşağıdaki hakkında bilgi edinin: [v2.0 sınırlamaları](#limitations).

## <a name="who-can-sign-in"></a>Kim oturum

![Kim oturum v1.0 ve v2.0 uç imzalayabilir mi](media/azure-ad-endpoint-comparison/who-can-sign-in.png)

* Uygulamanıza (Azure AD) oturum açmak yalnızca iş ve Okul hesaplarında v1.0 uç nokta sağlar
* V2.0 uç noktası, hotmail.com, outlook.com ve oturum açmak için msn.com gibi iş ve Okul hesapları Azure AD'den ve kişisel Microsoft hesapları (MSA) sağlar.
* V1.0 hem v2.0 uç noktaları ayrıca oturum açma işlemlerini kabul *[Konuk kullanıcılar](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b)* olarak yapılandırılan uygulamalar için bir Azure AD Directory *[tek kiracılı](single-and-multi-tenant-apps.md)* veya *çok kiracılı* kiracıya özgü uç noktaya işaret edecek şekilde yapılandırılmış uygulamaların (`https://login.microsoftonline.com/{TenantId_or_Name}`).

V2.0 uç noktası, kişisel Microsoft hesapları ve iş ve Okul hesaplarında oturum açma işlemleri kabul uygulamaları yazmanıza olanak sağlar. Bu, uygulamanızı tamamen hesabı belirsiz yazma olanağı sağlar. Örneğin, uygulamanızın çağırırsa [Microsoft Graph](https://graph.microsoft.io), bazı ek işlevler ve veriler iş hesaplarını, SharePoint sitelerinde veya dizin verilerini gibi kullanılabilir olacaktır. Ancak birçok eylemi için gibi [bir kullanıcının posta okuma](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/user_list_messages), aynı kod hem kişisel ve iş ve Okul hesapları için e-posta erişebilirsiniz.

V2.0 uç noktası için Kurumsal dünyaları ve tüketici, eğitim, erişim sağlamak için Microsoft kimlik doğrulama kitaplığı (MSAL) kullanabilirsiniz. Azure AD v1.0 uç noktası, yalnızca iş ve Okul hesaplarında oturum açma işlemleri kabul eder.

## <a name="incremental-and-dynamic-consent"></a>Artımlı ve dinamik onayı

Azure AD v1.0 uç noktası kullanan uygulamalar gibi gerekli OAuth 2.0 izinlerini önceden belirtmeniz gerekir:

![İzinleri kayıt kullanıcı Arabirimi](./media/azure-ad-endpoint-comparison/app_reg_permissions.png)

Doğrudan uygulama kaydı üzerinde izinler **statik**. Azure Portalı'nda tanımlanan uygulamanın statik izinleri güzel ve basit kod toplarken, geliştiriciler için olası bazı sorunlar sunar:

* Uygulamanın, kullanıcının ilk oturum açma sırasında hiç olmadığı kadar gerekir tüm izinleri istemek gerekir. Bu, son kullanıcıların, ilk oturum açma uygulamanın erişimi onaylama gerçekleştirilmesini önler uzun izinleri listesi neden olabilir.

* Uygulamanın tüm önceden hiç olmadığı kadar eriştiğiniz kaynaklar bilmeniz gerekir. Kaynakları rastgele bir sayıdan erişebilir uygulamalar oluşturmak zordu.

V2.0 uç noktası ile bir çıplak en düşük izinler kümesini önceden için soran ve zaman içinde daha tahakkuk anlamına gelir Azure portalı ve istek izinlerini uygulama kayıt bilgileri artımlı olarak bunun yerine, tanımlanan statik izinleri yoksayabilirsiniz müşterisi olarak, ek uygulama özelliklerini kullanır. Bunu yapmak için uygulamanız gereken herhangi bir zamanda yeni kapsamlar dahil ederek kapsamları belirtebilirsiniz `scope` - önceden uygulama kayıt defteri bilgilerini tanımlamak için gerek kalmadan, bir erişim belirteci isteğinde bulunurken parametresi. Kullanıcı henüz isteğine eklenen yeni kapsamlar için onaylı taşınmadığından, yalnızca yeni izinler için izin istenir. Daha fazla bilgi için bkz. [izinler ve onay kapsamları](v2-permissions-and-consent.md).

İzin istemek için bir uygulama ile dinamik olarak izin vererek `scope` parametresi, geliştiricilerin kullanıcı deneyimi üzerinde tam denetim sağlar. Ayrıca, izninizi deneyimi ve tüm izinleri bir ilk yetkilendirme isteği için sorun yük ön. Uygulamanız çok sayıda izinleri gerektiriyorsa, zaman içinde belirli bir uygulamanın özelliklerini kullanmak çalışırken, bu izinleri kullanıcıdan artımlı olarak toplayabilirsiniz.

Bir kuruluş adına gerçekleştirilen yönetici onayı yine yönetici tüm kuruluş adına izin vermeniz gerekiyorsa, bu izinleri uygulamalar için uygulama kayıt Portalı'nda ayarlamanız bir uygulama için kayıtlı statik izinleri gerektirir. Bu uygulamayı kurmak için kuruluş yönetici tarafından gerekli döngüleri azaltır.

## <a name="scopes-not-resources"></a>Kapsamları, kaynakları değil

V1.0 uç noktası kullanan uygulamalar için bir uygulama olarak davranabilir bir **kaynak**, veya bir alıcı belirteçleri. Bir kaynak bir dizi tanımlayabilirsiniz **kapsamları** veya **oAuth2Permissions** anladığı, istemci bu kaynaktan belirli bir dizi kapsamları belirteçler istemek uygulamalar sağlar. Örnek bir kaynak olarak Azure AD Graph API'sine göz önünde bulundurun:

* Kaynak tanımlayıcısı veya `AppID URI`: `https://graph.windows.net/`
* Kapsamları veya `oAuth2Permissions`: `Directory.Read`, `Directory.Write`ve benzeri.

Bu, v2.0 uç noktası için geçerlidir. Bir uygulama yine de davranabilir bir kaynak olarak kapsamları tanımlamak ve bir URI tarafından tanımlanan. İstemci uygulamaları yine de bu kapsamlara erişim isteğinde bulunabilirsiniz. Ancak, bu izinleri bir istemci isteklerinin şekilde değişti. 

Bir OAuth 2.0 v1.0 uç noktayı yetkilendirmek için Azure AD'ye istek gibi attıktan:

```text
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https://graph.windows.net/
...
```

Burada, **kaynak** parametresi hangi kaynağın belirtilen istemci uygulamasının yetkilendirme istiyor. Azure AD uygulaması Azure portalında statik yapılandırmasına göre ve buna uygun olarak verilen belirteçler için gereken izinleri hesaplanan. 

V2.0 uç noktası kullanan uygulamalar için aynı OAuth 2.0 yetkilendirme isteği aşağıdaki gibi görünür:

```text
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https://graph.windows.net/directory.read%20https://graph.windows.net/directory.write
...
```

Burada, **kapsam** parametresi gösterir uygulamanın hangi kaynak ve izinleri isteyen yetkilendirme. İstenen kaynak istekte hala mevcut - her kapsam parametresinin değeri ileride olan. Bu şekilde kapsam parametresi kullanarak v2.0 uç noktası ile OAuth 2.0 belirtimini daha uyumlu olmasını sağlar ve ortak sektör uygulamalarına sahip daha yakından hizalar. Ayrıca gerçekleştirmek uygulamaları tanır [artımlı onay](#incremental-and-dynamic-consent) - yalnızca uygulama bunları başlangıcı yerine sonundan Önden gerektirdiğinde izinleri isteyen.

## <a name="well-known-scopes"></a>İyi bilinen kapsamları

### <a name="offline-access"></a>Çevrimdışı erişim

V2.0 uç noktası kullanan uygulamalar - uygulamalar için yeni bir bilinen izin kullanımını gerektirebilir `offline_access` kapsam. Tüm uygulamalar, bir uzun süre, kullanıcının uygulamayı etkin bir şekilde kullanırken değil, hatta bir kullanıcı adına kaynaklara gerekiyorsa bu izin istemesi gerekir. `offline_access` Kapsamı Onayı iletişim kutularının kullanıcıya görünür **verilerinizi dilediğiniz zaman erişin**, hangi kullanıcının kabul etmesi gerekir. İsteyen `offline_access` izni OAuth 2.0 refresh_tokens v2.0 uç noktasından almak üzere web uygulamanızı etkinleştirir. Yenileme belirteçleri uzun süreli ve uzun süreler erişim için yeni OAuth 2.0 erişim belirteçleri değiştirilebilir.

Uygulamanızı istemeyen varsa `offline_access` kapsamı, yenileme belirteçleri almaz. OAuth 2.0 yetkilendirme kod akışı bir yetkilendirme kodunda şifrenizi kullandığınızda, yalnızca geri bir erişim belirteci alırsınız, yani `/token` uç noktası. Bu belirteç kalan süre (genellikle bir saat) kısa bir süre için geçerli erişim, ancak sonunda sona erer. Uygulamanızı, zaman içinde nokta kullanıcıyı yeniden yönlendirmek için gereken en başa `/authorize` yeni bir yetkilendirme kodu almak için uç nokta. Bu yeniden yönlendirme sırasında kullanıcı veya kimlik bilgilerini yeniden girin veya uygulamanın türüne bağlı olarak, izinlerde reconsent gerekmeyebilir.

OAuth 2.0 hakkında daha fazla bilgi edinmek için `refresh_tokens`, ve `access_tokens`, kullanıma [v2.0 protokol başvurusu](active-directory-v2-protocols.md).

### <a name="openid-profile-and-email"></a>Openıd, profil ve e-posta

Tarihsel olarak, en temel Openıd Connect oturum açma akışını Azure AD ile çok fazla sonuç kullanıcı hakkındaki bilgilerin sağlayacağını *id_token*. Bir id_token talep kullanıcının adı, tercih edilen kullanıcı adı, e-posta adresi, nesne kimliği ve daha fazlasını içerebilir.

Bilgi, `openid` kapsam ortaya koymaktadır, uygulamanıza erişimi, artık sınırlı. `openid` Kapsamı yalnızca kullanıcının oturum açmasını ve kullanıcı için uygulamaya özgü tanımlayıcı almak uygulamanızı sağlayacaktır. Uygulamanızda kullanıcı hakkındaki kişisel verileri almak istiyorsanız, kullanıcıdan ilave izin istemek uygulamanız gerekir. İki yeni kapsamlar `email` ve `profile`, ilave izin istemek izin verir.

* `email` Kapsam aracılığıyla kullanıcının birincil e-posta adresi erişim sağlayan `email` adreslenebilir e-posta adresi kullanıcının sahip olduğu varsayılırsa, id_token içinde talep. 
* `profile` Kapsamı ortaya koymaktadır, diğer tüm kullanıcıların adı, tercih edilen kullanıcı adı gibi kullanıcı hakkında temel bilgileri erişim nesne kimliği, vb. id_token içinde.

Bu kapsam, uygulamanızı kendi işini yapması için gereken bilgileri kümesi için yalnızca kullanıcı sorabilirsiniz uygulamanızı açığa en az bir şekilde kod olanak sağlar. Bu kapsamları hakkında daha fazla bilgi için bkz. [v2.0 kapsam başvurusu](v2-permissions-and-consent.md).

## <a name="token-claims"></a>Belirteç taleplerinden

V2.0 uç noktası yükleri küçük tutmak için varsayılan olarak kendi belirteçlere talep daha küçük bir dizi yayınlar. Uygulamalar ve belirli bir talep artık varsayılan olarak bir v2.0 belirteç sağlanan v1.0 belirtecindeki üzerinde bağımlılığa sahip hizmetler varsa, kullanmayı [isteğe bağlı bir talep](active-directory-optional-claims.md) bu talebi eklenecek özellik.

## <a name="limitations"></a>Sınırlamalar

V2.0 kullanırken dikkat edilmesi gereken bazı kısıtlamalar vardır.

Microsoft kimlik platformu ile tümleştirilen uygulamalar oluştururken, v2.0 uç noktasını ve kimlik doğrulama protokolleri gereksinimlerinizi karşılayıp karşılamadığını karar vermeniz gerekir. Platform ve v1.0 uç nokta hala tam olarak desteklenir ve bazı açılardan, v2.0 değerinden daha fazla özellik zengin olduğu. Ancak, v2.0 [önemli avantajlar sunuyor](azure-ad-endpoint-comparison.md) geliştiriciler için.

Geliştiriciler için bu anda basitleştirilmiş bir öneri şu şekildedir:

* İstediğiniz veya kişisel Microsoft hesapları, uygulamanızda desteklemesi gerekir ya da yeni bir uygulama yazıyorsanız, v2.0 kullanın. Ancak bunu yapmadan önce bu makalede ele alınan sınırlamaları anladığınızdan emin olun.
* Geçiş ya da SAML üzerinde dayalı bir uygulama güncelleştirme, v2.0 kullanamazsınız. Bunun yerine, başvurmak [v1.0 Kılavuzu](v1-overview.md).

V2.0 uç noktası, böylece hiç olmadığı kadar yalnızca v2.0 uç noktası kullanmanız gerekir, burada listelenen kısıtlamalarla ortadan kaldırmak için evrilir. Bu arada, v2.0 uç noktası sizin için uygun olup olmadığını belirlemek için bu makaleyi kullanın. Bu makalede, v2.0 uç noktasının geçerli durumu yansıtacak şekilde güncelleştirmeye devam edeceğiz. Geri gereksinimlerinize göre v2.0 özellikleri yeniden değerlendirmek için denetleyin.

### <a name="restrictions-on-app-registrations"></a>Uygulama kayıtları kısıtlamaları

V2.0 uç noktası ile tümleştirmek istediğiniz her uygulama için bir uygulama kaydı içinde oluşturabileceğiniz [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). Alternatif olarak, bir uygulama kullanarak kaydedebilirsiniz [ **uygulama kayıtları (Önizleme)** deneyimi](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview) Azure portalında. Mevcut Microsoft hesabı uygulamaları Önizleme portalı ile uyumlu değildir, ancak tüm AAD uygulamaları mı, nerede ve ne zaman kayıtlı bağımsız olarak. 

İş ve Okul hesapları ve kişisel hesapları destekleyen bir uygulama kayıtları aşağıdaki uyarılar vardır:

* Uygulama Kimliği yalnızca iki uygulama sırrı izin verilir
* Uygulama kayıt portalında kişisel bir Microsoft hesabı olan bir kullanıcı tarafından kaydedilen bir uygulama kaydı görüntülenebilir ve yalnızca tek bir geliştirici hesabı tarafından yönetilir. Birden fazla Geliştirici arasında paylaşılamaz. Birden fazla Geliştirici ile uygulama kaydınızı paylaşmak istiyorsanız, Azure portalında uygulama kayıtları (Önizleme) bölümünü kullanarak uygulama oluşturabilirsiniz.
* İzin verilen yeniden yönlendirme URL biçimiyle ilgili çeşitli kısıtlamalar vardır. URL yeniden yönlendirme hakkında daha fazla bilgi için sonraki bölüme bakın.

### <a name="restrictions-on-redirect-urls"></a>Kısıtlamalar yeniden yönlendirme URL'leri

V2.0 için kayıtlı uygulamalar sınırlı bir yeniden yönlendirme URL'si değerler kümesi kısıtlanır. Yeniden yönlendirme URL'sini web uygulamaları ve Hizmetleri ile şemasıyla başlamalı `https`, ve tüm yeniden yönlendirme URL'si değerleri tek bir DNS etki alanını paylaşmalıdır.  Kayıt sistemi mevcut yeniden yönlendirme URL'sini ekliyorsunuz yeniden yönlendirme URL'sini DNS adı tam DNS adı ile karşılaştırır. `http://localhost` yeniden yönlendirme URL'si olarak da desteklenir.  

Aşağıdaki koşullardan biri geçerli olduğunda DNS adı ekleme isteği başarısız olur:  

* Yeni yeniden yönlendirme URL'sinin tam DNS adı mevcut bir yeniden yönlendirme URL'sinin DNS adı eşleşmiyor.
* Yeni yeniden yönlendirme URL'sinin tam DNS adı mevcut bir yeniden yönlendirme URL'sinin alt etki alanı değil.

#### <a name="example-1"></a>Örnek 1

Uygulamanın yeniden yönlendirme URL'sini varsa `https://login.contoso.com`, burada DNS adı eşleşmeleri aşağıdaki örnekte gösterildiği gibi tam olarak bir yeniden yönlendirme URL'sini ekleyebilirsiniz:

`https://login.contoso.com/new`

Veya aşağıdaki örnekte gösterildiği gibi login.contoso.com DNS alt etki için başvurabilirsiniz:

`https://new.login.contoso.com`

#### <a name="example-2"></a>Örnek 2

Sahip olan bir uygulama istiyorsanız `login-east.contoso.com` ve `login-west.contoso.com` URL yeniden yönlendirme olarak, bu yeniden yönlendirme URL'lerini şu sırayla eklemeniz gerekir:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

İlk yönlendirme URL'sinin alt etki alanlarını olduklarından, sonraki iki ekleyebilirsiniz contoso.com. Bu sınırlama, gelecek bir sürümde kaldırılacak.

Ayrıca unutmayın, belirli bir uygulama için yalnızca 20 yanıt URL'leri olabilir - kayıt (SPA, yerel istemci, web uygulaması ve hizmet) destekler, bu sınır tüm uygulama türlerinde geçerlidir.  

V2.0 ile kullanmak için bir uygulamayı kaydetme hakkında bilgi almak için şu hızlı başlangıçlara bakın:

* [Uygulama kayıt Portalı'nı kullanarak bir uygulamayı kaydetme](quickstart-v2-register-an-app.md)
* [Uygulama kayıtları (Önizleme) deneyimi kullanarak bir uygulamayı kaydetme](quickstart-register-app.md)

### <a name="restrictions-on-libraries-and-sdks"></a>Kitaplıklarına ve Sdk'lara genel kısıtlamalar

Şu anda, v2.0 uç noktası için kitaplık desteği sınırlıdır. V2.0 uç noktası, bir üretim uygulamasında kullanmak istiyorsanız, bu seçenekler vardır:

* Bir web uygulaması derliyorsanız, oturum açma ve belirteç doğrulama gerçekleştirmek için genel kullanıma sunulan sunucu tarafı ara yazılımı güvenli bir şekilde kullanabilirsiniz. Bunlar, ASP.NET ve Node.js Passport eklentisi için OWIN Openıd Connect ara yazılımını içerir. Microsoft Ara yazılımında kullanan kod örnekleri için bkz. [Başlarken v2.0](v2-overview.md#getting-started) bölümü.
* Masaüstü veya mobil bir uygulama oluşturuyorsanız, Microsoft kimlik doğrulama kitaplığı (MSAL) önizleme birini kullanabilirsiniz. Bu kitaplıklar üretim desteklenen bir önizleme aşamasında olduğundan, bunları üretim uygulamalarında kullanmak daha güvenlidir. Daha fazla Önizleme kullanılabilir kitaplıkları ve terimler hakkında [kimlik doğrulama kitaplıkları başvuru](reference-v2-libraries.md).
* Microsoft kitaplıkları tarafından kapsamında olmayan platformlar için doğrudan uygulama kodunuzda protokolü ileti alma ve gönderme tarafından v2.0 uç noktası ile tümleştirebilirsiniz. V2.0 Openıd Connect ve OAuth protokolleri [açıkça belirtilmiştir](active-directory-v2-protocols.md) bu tür bir tümleştirme gerçekleştirmenizi sağlayacak.
* Son olarak, v2.0 uç noktası ile tümleştirmek için açık kaynaklı Openıd Connect ve OAuth kitaplıklarını kullanabilirsiniz. V2.0 uç noktası, değişiklik yapmak zorunda kalmadan birçok açık kaynak Protokolü kitaplıkları ile uyumlu olmalıdır. Bu tür kitaplıkları kullanılabilirliğini, dil ve platforma göre değişir. [Openıd Connect](http://openid.net/connect/) ve [OAuth 2.0](http://oauth.net/2/) Web siteleri korumak popüler uygulamaları listesi. Daha fazla bilgi için [Azure Active Directory v2.0 ve kimlik doğrulama kitaplıkları](reference-v2-libraries.md), açık kaynak istemci kitaplıkları ve v2.0 uç noktası ile test edilmiştir örnekleri listesi.
* Başvuru için `.well-known` v2.0 ortak uç nokta için uç nokta `https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration`. Değiştirin `common` kiracınıza belirli verileri almak için Kiracı kimliği.  

### <a name="protocol-changes"></a>Protokol değişiklikleri

V2.0 uç noktası, SAML veya WS-Federation desteklemez; yalnızca, Openıd Connect ve OAuth 2.0 de destekler.  OAuth 2.0 protokolleri v1.0 uç noktasından önemli değişiklikler şunlardır: 

* `email` Talep, isteğe bağlı bir talep yapılandırılmışsa döndürülür **veya** kapsam = e-posta, istekte belirtildi. 
* `scope` Parametresi yerine artık desteklenmektedir `resource` parametresi.  
* Birçok yanıtları gibi OAuth 2.0 belirtimini ile daha uyumlu hale getirmek için doğru döndüren değiştirilmiş `expires_in` bir dize yerine tamsayı olarak.  

V2.0 uç noktasına desteklenen protokol işlevselliği daha iyi anlamak için bkz: [Openıd Connect ve OAuth 2.0 protokolü başvurusu](active-directory-v2-protocols.md).

#### <a name="saml-restrictions"></a>SAML kısıtlamaları

Windows uygulamalarında Active Directory Authentication Library (ADAL) kullandıysanız, güvenlik onaylama işlemi biçimlendirme dili (SAML) onay verme kullanan Windows tümleşik kimlik doğrulaması avantajlarından geçen. Bu izinle, kullanıcıları Azure AD'ye Federasyon kiracıları sessizce kimlik doğrulaması gerçekleştirmenin kendi şirket içi Active Directory örneği ile kimlik bilgilerini girmeden. SAML onay verme v2.0 uç nokta üzerinde desteklenmiyor.
