---
title: "Azure Active Directory v2.0 uç noktası sınırlamaları ve kısıtlamaları | Microsoft Docs"
description: "Sınırlamalar ve kısıtlamalarla Azure AD v2.0 uç noktası için listesi."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mtillman
editor: 
ms.assetid: a99289c0-e6ce-410c-94f6-c279387b4f66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
<<<<<<< HEAD
ms.openlocfilehash: 5a9d455203e50da47208ef1494d38a950161bee1
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: HT
=======
ms.openlocfilehash: a81f505c189da31edb91d1b522d9f3140f821cb4
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="should-i-use-the-v20-endpoint"></a>V2.0 uç kullanmalıyım?
Azure Active Directory ile tümleştirme uygulamalar oluştururken, v2.0 uç noktası ve kimlik doğrulama protokolleri gereksinimlerinizi karşılayıp karar vermeniz gerekir. Azure Active Directory'nin özgün uç nokta hala tam olarak desteklenir ve bazı bakımlardan v2.0'den daha fazla özellik zengin değil. Ancak, v2.0 uç [önemli avantajlar sunar](active-directory-v2-compare.md) geliştiriciler için.

Geliştiriciler için bu anda Basitleştirilmiş Bizim önerimiz şöyledir:

* Uygulamanızda kişisel Microsoft hesapları desteklemesi gerekiyorsa v2.0 uç kullanın. Ancak bunu yapmadan önce bu makalede aşağıdakiler ele kısıtlamaları anladığınızdan emin olun.
* Uygulamanızı yalnızca desteklenmesi için Microsoft iş ve Okul hesapları gerekirse, v2.0 uç nokta kullanmayın. Bunun yerine, başvurmak bizim [Azure AD Geliştirici Kılavuzu](active-directory-developers-guide.md).

Zaman içinde her zaman sadece v2.0 uç noktası kullanmanız gerekir böylece burada listelenen kısıtlamalarla ortadan kaldırmak için v2.0 uç büyüyecektir. Bu arada, bu makale, v2.0 uç sizin için uygun olup olmadığını belirlemek için tasarlanmıştır. Bu makalede v2.0 uç noktası geçerli durumunu yansıtacak şekilde güncelleştirmeye devam eder. Geri gereksinimlerinizi v2.0 yetenekleri karşı yeniden değerlendirmeye denetleyin.

V2.0 uç noktası kullanmaz var olan bir Azure AD uygulaması varsa, baştan başlatmaya gerek yoktur. Gelecekte, biz var olan Azure AD uygulamalarınız v2.0 uç noktası ile kullanmak bir yol sağlayacaktır.

## <a name="restrictions-on-app-types"></a>Uygulama türleri kısıtlamalar
Şu anda, aşağıdaki uygulama türlerini v2.0 uç noktası tarafından desteklenmez. Desteklenen uygulama türleri açıklaması için bkz: [uygulama türleri için Azure Active Directory v2.0 uç](active-directory-v2-flows.md).

### <a name="standalone-web-apis"></a>Tek başına Web API'leri
V2.0 uç noktasına kullanabilirsiniz [güvenli bir Web API OAuth 2.0 ile derleme](active-directory-v2-flows.md#web-apis). Ancak, bu Web API belirteçleri aynı uygulama kimliği olan bir uygulamadan alabilirsiniz Farklı bir uygulama kimliği sahip bir istemciden bir Web API erişemiyor İstemci isteği veya Web apı'nize izinleri almak mümkün olmayacaktır.

Aynı uygulama Kimliğine sahip bir istemciden gelen belirteçleri kabul eder bir Web API'sini nasıl oluşturulacağını görmek için v2.0 uç noktası Web API örneklerinde bakın bizim [Başlarken](active-directory-appmodel-v2-overview.md#getting-started) bölümü.

## <a name="restrictions-on-app-registrations"></a>Uygulama kayıtlar kısıtlamalar
Şu anda v2.0 uç noktası ile tümleştirmek istediğiniz her uygulama için bir uygulama kaydı yeni oluşturmalısınız [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). Var olan Azure AD veya Microsoft hesabı uygulamaları v2.0 uç noktası ile uyumlu değil. Uygulama kayıt Portalı'nı dışındaki herhangi bir portal kaydedilmiş uygulamaları v2.0 uç noktası ile uyumlu değildir. Gelecekte, v2.0 uygulama olarak mevcut bir uygulamayı kullanmak için bir yol sağlamak planlıyoruz. Şu anda, yine de yoktur v2.0 uç noktası ile çalışmak var olan bir uygulama için geçiş yol yok.

Buna ek olarak, oluşturduğunuz uygulama kayıtlar [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) aşağıdaki uyarılarla vardır:

* Yalnızca iki uygulama sırrı uygulama kimliği izin verilen
* Kişisel bir Microsoft hesabı olan bir kullanıcı tarafından kaydedilen bir uygulama kaydı görüntülenebilir ve yalnızca bir tek Geliştirici hesabı tarafından yönetilebilir. Birden çok geliştiriciler arasında paylaşılamaz.  Uygulama kaydınızı birden çok geliştiriciler arasında paylaşmak istiyorsanız, bir Azure AD hesabı ile kayıt portalı uygulamasına oturum açarak uygulamayı oluşturabilirsiniz.
* Yeniden yönlendirme izin URI biçimi birkaç kısıtlamalar vardır. Yeniden yönlendirme URI'ler hakkında daha fazla bilgi için sonraki bölüme bakın.

## <a name="restrictions-on-redirect-uris"></a>Yeniden yönlendirme URI'ler kısıtlamalar
Şu anda, uygulama kayıt Portalı'nda kayıtlı apps sınırlı sayıda yeniden yönlendirme URI'si değerleri için kısıtlanır. Web uygulamaları ve Hizmetleri için URI şeması ile başlamalıdır yeniden yönlendirme `https`, ve tüm yeniden yönlendirme URI'si değerler tek bir DNS etki alanı paylaşması gerekir. Örneğin, bu yeniden yönlendirme URI'ler biri sahip bir web uygulaması kaydedilemiyor:

`https://login-east.contoso.com`  
`https://login-west.contoso.com`

Kayıt sistem yeniden yönlendirme eklemekte olduğunuz URI'si DNS adını mevcut yeniden yönlendirme URI'si tam DNS adını karşılaştırır. Aşağıdaki koşullardan biri geçerli olduğunda DNS adı ekleme isteği başarısız olur:  

* Yeni yeniden yönlendirme URI'si tam DNS adı, varolan yeniden yönlendirme URI'si DNS adı eşleşmiyor.
* Yeni yeniden yönlendirme URI'si tam DNS adı bir alt etki alanı mevcut yeniden yönlendirme URI'si değil.

Örneğin, uygulamanın bu yeniden yönlendirme URI'si varsa:

`https://login.contoso.com`

Aşağıdaki gibi ekleme yapabilirsiniz:

`https://login.contoso.com/new`

Bu durumda, DNS adı tam olarak eşleşir. Ya da şunu yapabilirsiniz:

`https://new.login.contoso.com`

Bu durumda, login.contoso.com DNS alt etki alanına başvurursunuz. Oturum açma east.contoso.com ve oturum açma-west.contoso.com URI'ler yeniden yönlendirme olarak bir uygulama sahip olmak istiyorsanız, bu URI şu sırayla yeniden yönlendirme eklemeniz gerekir:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

İlk yeniden yönlendirme URI'si alt etki alanlarına olduklarından ikinci iki ekleyebilirsiniz contoso.com. Bu sınırlama gelecek sürümde kaldırılacak.

Ayrıca unutmayın, belirli bir uygulama için yalnızca 20 yanıt URL'leri olabilir.

Uygulama kayıt Portalı'nda uygulama kaydetmeyi öğrenmek için bkz: [v2.0 uç noktası ile bir uygulama nasıl](active-directory-v2-app-registration.md).

## <a name="restrictions-on-services-and-apis"></a>Hizmetleri ve API üzerindeki kısıtlamaları
Şu anda v2.0 uç oturum açma için uygulama kayıt Portalı'nda kayıtlı ve listesinde iner herhangi bir uygulama destekler [kimlik doğrulaması akışı desteklenen](active-directory-v2-flows.md). Ancak, bu uygulamaları çok sınırlı bir kaynak kümesi için OAuth 2.0 erişim belirteçleri elde edebilirsiniz. Belirteçleri yalnızca erişim v2.0 uç noktası sorunlarını:

* Belirteç istenen uygulama. Mantıksal uygulama birkaç farklı bileşenler veya katmanları oluşuyorsa uygulama kendisi için bir erişim belirteci alması. Bu senaryoyu çalışırken görmek için kullanıma bizim [Başlarken](active-directory-appmodel-v2-overview.md#getting-started) öğreticileri.
* Outlook posta, Takvim ve kişiler REST API'leri, her biri https://outlook.office.com bulunur. Bu API'leri erişen bir uygulama yazmak öğrenmek için bkz: [Office Başlarken](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) öğreticileri.
* Microsoft Graph API. Hakkında daha fazla bilgiyi [Microsoft Graph](https://graph.microsoft.io) ve kullanabileceğiniz verileri.

Şu anda başka bir hizmetler desteklenir. Daha fazla Microsoft Online Services, kendi özel olarak geliştirilmiş Web API'leri ve hizmetler için destek yanı sıra gelecekte eklenir.

## <a name="restrictions-on-libraries-and-sdks"></a>Kitaplıklar ve SDK'ları üzerindeki kısıtlamaları
Şu anda v2.0 uç noktası için kitaplık desteği sınırlıdır. Bir üretim uygulamasında v2.0 uç kullanmak istiyorsanız, bu seçenekler vardır:

* Bir web uygulaması oluşturuyorsanız, oturum açma ve belirteci doğrulamayı gerçekleştirmek için Microsoft Genel olarak kullanılabilir sunucu tarafı Ara güvenli bir şekilde kullanabilirsiniz. Bunlar, ASP.NET ve Node.js Passport eklenti için Open ID Connect OWIN ara yazılımı içerir. Microsoft Ara kullanmak kod örnekleri için bkz: bizim [Başlarken](active-directory-appmodel-v2-overview.md#getting-started) bölümü.
* Masaüstü ve mobil uygulama geliştiriyorsanız, önizlememiz Microsoft kimlik doğrulama kitaplıkları (MSAL) birini kullanabilirsiniz.  Bu kitaplıklar üretim desteklenen Önizleme'de, olduğundan, üretim uygulamaları kullanmak güvenlidir. Daha fazla bilgiyi koşullarını Önizleme ve kullanılabilir kitaplıklarında hakkında bizim [kimlik doğrulama kitaplıkları başvuru](active-directory-v2-libraries.md).
* Microsoft kitaplıkları tarafından kapsanmayan platformları için doğrudan uygulama kodunuzda protokolü ileti alma ve gönderme tarafından v2.0 uç noktası ile tümleştirebilirsiniz. V2.0 Openıd Connect ve OAuth protokollerini [açıkça belgelenmiş](active-directory-v2-protocols.md) bu tür bir tümleştirme gerçekleştirmenize yardımcı olmak için.
* Son olarak, v2.0 uç noktası ile tümleştirmek için açık kaynak açmak ID Connect ve OAuth kitaplıkları kullanabilirsiniz. V2.0 protokol önemli bir değişiklik yapılmadan birçok açık kaynak Protokolü kitaplıkları ile uyumlu olmalıdır. Bu tür kitaplıklarının kullanılabilirliğini dile ve platforma göre değişir. [Open ID Connect](http://openid.net/connect/) ve [OAuth 2.0](http://oauth.net/2/) Web siteleri korumak popüler uygulamaları listesi. Daha fazla bilgi için bkz: [Azure Active Directory v2.0 ve kimlik doğrulama kitaplıkları](active-directory-v2-libraries.md)ve açık kaynak istemci kitaplıkları ve v2.0 uç noktası ile test örnekleri listesi.

## <a name="restrictions-on-protocols"></a>Protokolleri kısıtlamalar
V2.0 uç SAML veya WS-Federasyon desteklemiyor; yalnızca, Open ID Connect ve OAuth 2.0 de destekler.  Tüm özellikleri ve yetenekleri OAuth kurallarının v2.0 uç noktası eklenmiştir. Şu anda bu protokolü özellikleri ve yetenekleri olan *kullanılamaz* v2.0 uç noktada:

* V2.0 uç noktası tarafından verilen kimlik belirteçlerini içermez bir `email` e-postalarına görüntülemek için kullanıcıdan izin alma olsa bile, kullanıcı için talep.
* Openıd Connect kullanıcı bilgisi endpoint v2.0 uç noktada uygulanmadı. Ancak, büyük olasılıkla bu uç noktada alacağı tüm kullanıcı profil verileri kullanılabilir Microsoft Graph'tan `/me` uç noktası.
* V2.0 uç veren rol veya Grup Talep Kimliği belirteçleri desteklemez.
* [OAuth 2.0 kaynak sahibi parolası kimlik bilgilerini verme](https://tools.ietf.org/html/rfc6749#section-4.3) v2.0 uç noktası tarafından desteklenmiyor.

Ayrıca, v2.0 uç SAML veya WS-Federasyon kurallarının herhangi bir biçimde desteklemez.

V2.0 uç desteklenen protokol işlevselliği kapsamını daha iyi anlamak için okuyun bizim [Openıd Connect ve OAuth 2.0 protokolü başvurusu](active-directory-v2-protocols.md).

## <a name="restrictions-for-work-and-school-accounts"></a>İş ve Okul hesapları için kısıtlamalar
Windows uygulamalarında Active Directory Authentication Library (ADAL) kullandıysanız, güvenlik onaylama işlemi biçimlendirme dili (SAML) onaylama grant kullanan Windows tümleşik kimlik doğrulaması, avantajlarından gerçekleştirilecek. Bu verme ile Federasyon kullanıcıları, Azure AD kiracılar sessizce kimliğini kendi şirket içi Active Directory örneği ile kimlik bilgileri girmeden. Şu anda, SAML onaylama grant v2.0 uç noktada desteklenmiyor.
