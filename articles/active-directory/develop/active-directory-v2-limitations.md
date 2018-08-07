---
title: Azure Active Directory v2.0 uç noktası sınırlamaları ve kısıtlamaları | Microsoft Docs
description: Sınırlamalar ve kısıtlamalar Azure AD v2.0 uç noktası için listesi.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: a99289c0-e6ce-410c-94f6-c279387b4f66
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: celested
ms.reviewer: hirsin, dastrock
ms.custom: aaddev
ms.openlocfilehash: aa931702975c2c6bdcc65853c3865dbeff570bf4
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39578458"
---
# <a name="should-i-use-the-v20-endpoint"></a>V2.0 uç noktası kullanmalıyım?

Azure Active Directory (Azure AD) ile tümleştirilen uygulamalar oluştururken, v2.0 uç noktasını ve kimlik doğrulama protokolleri gereksinimlerinizi karşılayıp karşılamadığını karar vermeniz gerekir. Azure AD'nin özgün uç nokta hala tam olarak desteklenir ve bazı açılardan, v2.0 değerinden daha fazla özellik zengin olduğu. Ancak, v2.0 uç noktası [önemli avantajlar sunuyor](azure-ad-endpoint-comparison.md) geliştiriciler için.

Geliştiriciler için bu anda basitleştirilmiş bir öneri şu şekildedir:

* Kişisel Microsoft hesapları, uygulamanızda desteklemelidir, v2.0 uç noktası kullanın. Ancak bunu yapmadan önce bu makalede ele alınan sınırlamaları anladığınızdan emin olun.
* Uygulamanız yalnızca destekleyen Microsoft iş ve Okul hesapları gerekiyorsa, v2.0 uç nokta kullanmayın. Bunun yerine, başvurmak [Azure AD Geliştirici Kılavuzu](azure-ad-developers-guide.md).

V2.0 uç noktası, böylece hiç olmadığı kadar yalnızca v2.0 uç noktası kullanmanız gerekir, burada listelenen kısıtlamalarla ortadan kaldırmak için evrilir. Bu arada, v2.0 uç noktası sizin için uygun olup olmadığını belirlemek için bu makaleyi kullanın. Bu makalede, v2.0 uç noktasının geçerli durumu yansıtacak şekilde güncelleştirmeye devam eder. Geri gereksinimlerinize göre v2.0 özellikleri yeniden değerlendirmek için denetleyin.

V2.0 uç noktası kullanmaz var olan bir Azure AD uygulaması varsa, baştan başlatmaya gerek yoktur. Gelecekte, var olan Azure AD uygulamalarınız v2.0 uç noktası ile kullanmak bir yol sağlayacağız.

## <a name="restrictions-on-app-types"></a>Uygulama türleri kısıtlamaları

Şu anda aşağıdaki uygulama türlerini v2.0 uç noktası tarafından desteklenmez. Desteklenen uygulama türleri ile ilgili açıklama için bkz: [uygulama türleri için Azure Active Directory v2.0 uç noktası](active-directory-v2-flows.md).

### <a name="standalone-web-apis"></a>Tek başına Web API'leri

V2.0 uç noktası için kullanabileceğiniz [güvenli bir Web API'si OAuth 2.0 ile oluşturma](active-directory-v2-flows.md#web-apis). Ancak, Web API'SİNİN belirteçleri aynı uygulama kimliği olan bir uygulamadan alabilirsiniz Farklı uygulama kimliği olan bir istemciden bir Web API'si erişemiyor İstemci isteği veya Web API'niz için izinleri almak mümkün olmayacaktır.

V2.0 uç noktası Web API örnekleri aynı uygulama kimliği olan bir istemciden gelen belirteçleri kabul eden bir Web API'si oluşturma hakkında bilgi için bkz [Başlarken](active-directory-appmodel-v2-overview.md#getting-started) bölümü.

## <a name="restrictions-on-app-registrations"></a>Uygulama kayıtları kısıtlamaları

Şu anda, v2.0 uç noktası ile tümleştirmek istediğiniz her uygulama için bir uygulama kaydı yeni oluşturmalısınız [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). Mevcut Azure AD veya Microsoft hesabı uygulamaları v2.0 uç noktası ile uyumlu değildir. Uygulama kayıt portalı dışında herhangi bir portal kayıtlı uygulamalar v2.0 uç noktası ile uyumlu değildir. Gelecekte, v2.0 uygulaması olarak var olan bir uygulamayı kullanmak için bir yol sağlamak üzere planlıyoruz. Şu anda, v2.0 uç noktası ile çalışmak var olan bir uygulama için geçiş yolu yoktur.

Buna ek olarak, oluşturduğunuz uygulama kayıtları [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) aşağıdaki uyarılar vardır:

* Uygulama Kimliği yalnızca iki uygulama sırrı izin verilir
* Kişisel bir Microsoft hesabı olan bir kullanıcı tarafından kaydedilen bir uygulama kaydı görüntülenebilir ve yalnızca tek bir geliştirici hesabı tarafından yönetilir. Birden fazla Geliştirici arasında paylaşılamaz. Uygulama kaydınızı birden fazla Geliştirici arasında paylaşmak istiyorsanız, uygulama kayıt portalı bir Azure AD hesabı ile oturum açarak oluşturabilirsiniz.
* Yeniden yönlendirme URI'si izin verilen biçime birkaç kısıtlama vardır. Yeniden yönlendirme URI'leri hakkında daha fazla bilgi için sonraki bölüme bakın.

## <a name="restrictions-on-redirect-uris"></a>Yeniden yönlendirme URI'leri kısıtlamaları

Uygulama kayıt Portalı'nda kayıtlı uygulamalar sınırlı bir yeniden yönlendirme URI'si değerler kümesi kısıtlanır. Web uygulamaları ve hizmetler için URI şemasıyla başlamalı yeniden yönlendirme `https`, ve tüm yeniden yönlendirme URI'si değerlerini tek bir DNS etki alanını paylaşmalıdır. Örneğin, bu yeniden yönlendirme URI'leri birine sahip bir web uygulamasını kaydedemezsiniz:

`https://login-east.contoso.com`  
`https://login-west.contoso.com`

Kayıt sistemi tam DNS adı DNS adıyla yeniden yönlendirme URI'si eklemekte olduğunuz mevcut yeniden yönlendirme URI'si karşılaştırır. Aşağıdaki koşullardan biri geçerli olduğunda DNS adı ekleme isteği başarısız olur:  

* Tam DNS adı yeni yeniden yönlendirme URI'si mevcut yeniden yönlendirme URI'si DNS adı ile eşleşmiyor.
* Tam DNS adı yeni yeniden yönlendirme URI'si bir alt etki alanı mevcut yeniden yönlendirme URI'si değil.

Örneğin, uygulamanın bu yeniden yönlendirme URI'si varsa:

`https://login.contoso.com`

Aşağıdaki gibi ekleme yapabilirsiniz:

`https://login.contoso.com/new`

Bu durumda, DNS adı tam olarak eşleşir. Ya da şunu yapabilirsiniz:

`https://new.login.contoso.com`

Bu durumda, login.contoso.com DNS alt etki alanına başvurursunuz. Login-east.contoso.com ve login-west.contoso.com yeniden yönlendirme URI'leri gibi olan bir uygulama istiyorsanız, bunları şu sırayla yeniden yönlendirme URI'leri eklemeniz gerekir:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

İlk yeniden yönlendirme URI'si, alt etki alanlarını olduklarından, sonraki iki ekleyebilirsiniz contoso.com. Bu sınırlama, gelecek bir sürümde kaldırılacak.

Ayrıca unutmayın, belirli bir uygulama için yalnızca 20 yanıt URL'leri olabilir.

Uygulama kayıt Portalı'nda bir uygulamayı kaydetme hakkında bilgi için bkz: [v2.0 uç noktası ile bir uygulamayı kaydetme](quickstart-v2-register-an-app.md).

## <a name="restrictions-on-libraries-and-sdks"></a>Kitaplıklarına ve Sdk'lara genel kısıtlamalar

Şu anda, v2.0 uç noktası için kitaplık desteği sınırlıdır. V2.0 uç noktası, bir üretim uygulamasında kullanmak istiyorsanız, bu seçenekler vardır:

* Bir web uygulaması derliyorsanız, oturum açma ve belirteç doğrulama gerçekleştirmek için genel kullanıma açık Microsoft sunucu tarafı ara yazılımı güvenli bir şekilde kullanabilirsiniz. Bunlar, ASP.NET ve Node.js Passport eklentisi için OWIN Open ID Connect ara yazılımını içerir. Microsoft Ara yazılımında kullanan kod örnekleri için bkz. [Başlarken](active-directory-appmodel-v2-overview.md#getting-started) bölümü.
* Masaüstü veya mobil bir uygulama oluşturuyorsanız Microsoft kimlik doğrulama kitaplığı (MSAL) önizleme birini kullanabilirsiniz. Bu kitaplıklar üretim desteklenen bir önizleme aşamasında olduğundan, bunları üretim uygulamalarında kullanmak daha güvenlidir. Daha fazla Önizleme kullanılabilir kitaplıkları ve terimler hakkında [kimlik doğrulama kitaplıkları başvuru](active-directory-v2-libraries.md).
* Microsoft kitaplıkları tarafından kapsamında olmayan platformlar için doğrudan uygulama kodunuzda protokolü ileti alma ve gönderme tarafından v2.0 uç noktası ile tümleştirebilirsiniz. V2.0 Openıd Connect ve OAuth protokolleri [açıkça belirtilmiştir](active-directory-v2-protocols.md) bu tür bir tümleştirme gerçekleştirmenizi sağlayacak.
* Son olarak, v2.0 uç noktası ile tümleştirmek için açık kaynak açın ID Connect ve OAuth kitaplıklarını kullanabilirsiniz. V2.0 protokol önemli değişiklikler olmadan birçok açık kaynak Protokolü kitaplıkları ile uyumlu olmalıdır. Bu tür kitaplıkları kullanılabilirliğini, dil ve platforma göre değişir. [Open ID Connect](http://openid.net/connect/) ve [OAuth 2.0](http://oauth.net/2/) Web siteleri korumak popüler uygulamaları listesi. Daha fazla bilgi için [Azure Active Directory v2.0 ve kimlik doğrulama kitaplıkları](active-directory-v2-libraries.md), açık kaynak istemci kitaplıkları ve v2.0 uç noktası ile test edilmiştir örnekleri listesi.

## <a name="restrictions-on-protocols"></a>Protokolleri kısıtlamaları

V2.0 uç noktası, SAML veya WS-Federation desteklemez; yalnızca, Open ID Connect ve OAuth 2.0 de destekler. Tüm özellikler ve yetenekler OAuth kurallarının v2.0 uç noktasında eklenmiştir.

Şu anda aşağıdaki protokolü özellikleri ve yetenekleri olan *kullanılamıyor* v2.0 uç noktasını:

* Şu anda `email` talep yalnızca döndürülen isteğe bağlı bir talep yapılandırılır ve kapsam kapsam = e-posta, istekte belirtildi. Ancak, bu davranışı, v2.0 uç noktası daha da Open ID Connect ve OAuth2.0 standartlarına uymak için güncelleştirilmiş şekilde değiştirir.
* Openıd Connect UserInfo uç nokta v2.0 uç noktada uygulanmadı. Ancak, büyük olasılıkla bu uç noktada alacak tüm kullanıcı profil verileri kullanılabilir Microsoft Graph'teki `/me` uç noktası.
* V2.0 uç noktası kimliği belirteçler veren bir rol veya grup talepleri desteklemez.
* [OAuth 2.0 kaynak sahibi parola kimlik bilgileri verme](https://tools.ietf.org/html/rfc6749#section-4.3) v2.0 uç noktası tarafından desteklenmiyor.

Ayrıca, v2.0 uç noktası, herhangi bir biçimde SAML veya WS-Federasyon protokolünü desteklemiyor.

V2.0 uç noktasına desteklenen protokol işlevselliği daha iyi anlamak için okuyun bizim [Openıd Connect ve OAuth 2.0 protokolü başvurusu](active-directory-v2-protocols.md).

## <a name="restrictions-for-work-and-school-accounts"></a>İş ve Okul hesapları için kısıtlamalar

Windows uygulamalarında Active Directory Authentication Library (ADAL) kullandıysanız, güvenlik onaylama işlemi biçimlendirme dili (SAML) onay verme kullanan Windows tümleşik kimlik doğrulaması avantajlarından geçen. Bu izinle, kullanıcıları Azure AD'ye Federasyon kiracıları sessizce kimlik doğrulaması gerçekleştirmenin kendi şirket içi Active Directory örneği ile kimlik bilgilerini girmeden. Şu anda, SAML onay verme v2.0 uç noktada desteklenmiyor.
