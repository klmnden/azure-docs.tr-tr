---
title: Microsoft kimlik platformu kimlik doğrulaması | Azure
description: Microsoft kimlik platformu, uygulama kimlik doğrulaması hakkında bilgi edinin, sağlama, API, model ve söz konusu Microsoft kimlik platformu en yaygın kimlik doğrulama senaryoları destekler.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 0c84e7d0-16aa-4897-82f2-f53c6c990fd9
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/05/2019
ms.author: ryanwi
ms.reviewer: saeeda, sureshja, hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: b35d2e21de3da184496da53fdf46d865fdfdf5c7
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734478"
---
# <a name="what-is-authentication"></a>Kimlik doğrulaması nedir?

*Kimlik doğrulaması*, taraflardan geçerli kimlik bilgileri isteyerek kimlik ve erişim denetimi için kullanılacak bir güvenlik sorumlusu oluşturmaktır. Daha basit bir deyişle söylediğiniz kişi olduğunuzu kanıtlama işlemidir. Kimlik doğrulaması bazen AuthN şeklinde kısaltılabilir.

*Yetkilendirme*, kimliği doğrulanmış bir güvenlik sorumlusuna bir şeyi yapma iznini vermektir. Erişme iznine sahip olduğunuz verileri ve bu verilerle gerçekleştirebileceğiniz işlemleri belirtir. Yetkilendirme bazen AuthZ şeklinde kısaltılabilir.

Microsoft kimlik platformu için farklı platformlar için açık kaynak kitaplıkları yanı sıra OAuth 2.0 ve Openıd Connect gibi endüstri standardı protokoller için desteği ile bir hizmet olarak kimlik sağlayarak uygulama geliştiriciler için kimlik doğrulaması basitleştirir. hızlı bir şekilde kodlamaya başlamanıza yardımcı olur.

Microsoft kimlik platformu programlama modeli içinde iki ana kullanım örneği vardır:

* OAuth 2.0 yetki verme akışı: Burada kaynak sahibi istemci uygulamayı yetkilendirerek istemcinin kaynak sahibinin kaynaklarına erişmesini sağlar.
* İstemci tarafından kaynak erişimi sırasında: Kaynak sunucusu erişim belirtecindeki talep değerlerini kullanarak erişim denetimi kararları alır.

## <a name="authentication-basics-in-microsoft-identity-platform"></a>Microsoft kimlik platformu kimlik doğrulaması temel bilgileri

Kimliğin gerekli olduğu en basit senaryoyu ele alalım: Bir kullanıcının web tarayıcısından bir web uygulaması için kimlik doğrulamasından geçmesi gerekiyor. Aşağıdaki diyagramda bu senaryo gösterilmektedir:

![Web uygulamasında oturum açmaya genel bakış](./media/authentication-scenarios/auth-basics-microsoft-identity-platform.svg)

Diyagramda gösterilen bileşenlerle ilgili bilmeniz gerekenler burada v erilmiştir:

* Microsoft kimlik platformu kimlik sağlayıcıdır. Kimlik sağlayıcısı, kuruluşun dizinindeki kullanıcıların ve uygulamaların kimliğinin doğrulanmasından sorumludur ve bu kullanıcılarla uygulamalar kimlik doğrulamasından başarıyla geçtiğinde güvenlik belirteçleri oluşturur.
* Dış kimlik doğrulama için Microsoft kimlik platformu istediği bir uygulamayı Azure Active Directory (Azure AD) kayıtlı olması gerekir. Azure AD, dizindeki her uygulamayı kaydeder ve benzersiz bir şekilde tanımlar.
* Geliştiriciler, kimlik doğrulama protokolü ayrıntıları işleyerek kolaylaştıran açık kaynak Microsoft kimlik platformu kimlik doğrulama kitaplıklarını kullanabilir. Daha fazla bilgi için bkz. Microsoft kimlik platformu [v2.0 kimlik doğrulama kitaplıkları](reference-v2-libraries.md) ve [v1.0 kimlik doğrulama kitaplıkları](active-directory-authentication-libraries.md).
* Kullanıcının kimliği doğrulandıktan sonra uygulamanın kimlik doğrulamasının başarılı olduğundan emin olmak için kullanıcının güvenlik belirtecini doğrulaması gerekir. Farklı diller ve çerçeveler için uygulamanın yapması gereken işlemleri anlatan birçok hızlı başlangıç, öğretici ve kod örneği mevcuttur.
  * Hızlıca uygulama derlemek ve belirteç alma, belirteçleri yenileme, kullanıcı oturumu açma ve kullanıcı bilgilerini görüntüleme gibi işlevler eklemek için belgelerin **Hızlı başlangıçlar** bölümüne bakın.
  * Erişim belirteçleri alma ve bunları Microsoft Graph API ve diğer API'lere yapılan çağrılarda kullanma, OpenID Connect kullanarak geleneksel bir web tarayıcısı tabanlı uygulamaya Microsoft ile oturum açma özelliğini ekleme ve daha fazla geliştirici görevi için ayrıntılı ve senaryo tabanlı yordamlar için belgelerin **Öğreticiler** bölümüne bakın.
  * Kod örneklerini indirmek için [GitHub](https://github.com/Azure-Samples?q=active-directory)'a gidin.
* Kimlik doğrulaması işlemi için istek ve yanıt akışı OAuth 2.0, OpenID Connect, WS-Federation veya SAML 2.0 gibi kullandığınız kimlik doğrulaması protokolü tarafından belirlenir. Protokoller hakkında daha fazla bilgi için belgelerin **Kavramlar > Protokoller** bölümüne bakın.

Yukarıdaki örnek senaryoda uygulamaları bu iki role göre sınıflandırabilirsiniz:

* Kaynaklara güvenli bir şekilde erişmesi gereken uygulamalar
* Kaynağın rolünü oynaması gereken uygulamalar

API ve kimlik uygulama modeli anlamak için okumaya devam temel bir bakış gördüğünüze nasıl sağlama Microsoft kimlik platformu çalışır ve yaygın senaryolar hakkında ayrıntılı bilgi için Microsoft kimlik platformu bağlantıları destekler.

## <a name="application-model"></a>Uygulama modeli

Microsoft kimlik platformu uygulamaları aşağıdaki iki ana işlev karşılamak üzere tasarlanmış belirli bir modeli temsil eder:

* **Uygulamayı desteklediği kimlik doğrulaması protokollerine göre tanımlama**: Bu model tüm tanımlayıcıların, URL'lerin, gizli dizilerin ve kimlik doğrulaması sırasında gerekli olan ilgili bilgilerin listelenmesini kapsar. Burada, Microsoft kimlik platformu:

    * Çalışma zamanında kimlik doğrulamasını gerçekleştirmek için gerekli tüm verileri barındırır.
    * Bir uygulamanın erişmesi gerekebilecek kaynakları ve belirli bir isteğin gerçekleştirilip gerçekleştirilmeyeceğini ve hangi koşullarda gerçekleştirileceğini belirlemek için gerekli tüm verileri barındırır.
    * Uygulama geliştiricisinin kiracısında ve diğer Azure AD kiracısında uygulama sağlamak için gerekli altyapıyı sağlar.

* **Belirteç isteği süre boyunca kullanıcı onayı işlemek ve dinamik uygulamalar kiracılar genelinde sağlama kolaylaştırmak** - Burada, Microsoft kimlik platformu:

    * Kullanıcıların ve yöneticilerin uygulamanın kendileri adına kaynaklara erişmesine dinamik olarak onay vermesini veya reddetmesini sağlar.
    * Yöneticilerin uygulamaların gerçekleştirebilecekleri işlemler, belirli uygulamalara erişebilecek kullanıcılar ve erişilen dizin kaynakları hakkında son kararı vermesini sağlar.

Microsoft kimlik platformu içinde bir **uygulama nesnesi** soyut bir varlık olarak bir uygulamayı açıklar. Geliştiriciler uygulamalarla çalışır. Dağıtım sırasında Microsoft kimlik platformu belirli uygulama nesnesi bir şema oluşturmak için kullandığı bir **hizmet sorumlusu**, dizin veya Kiracı içinde bir uygulama somut bir örneğini temsil eder. Uygulamanın belirli bir hedef dizinde yapabileceklerini, onu kullanabilecek kişileri, erişim sağlayabileceği kaynakları ve diğer bilgileri tanımlayan hizmet sorumlusudur. Microsoft kimlik platformu uygulama nesneden bir hizmet sorumlusu oluşturur **onay**.

Akış tarafından onay temelli sağlama basitleştirilmiş bir Microsoft kimlik platformu Aşağıdaki diyagramda gösterilmiştir.  İçinde iki Kiracı yok (A ve B), burada Kiracı A sahip uygulamayı ve B kiracısındaki bir hizmet sorumlusu aracılığıyla uygulamaya örnekleme.  

![Onay temelli basitleştirilmiş sağlama akışı](./media/authentication-scenarios/simplified-provisioning-flow-consent-driven.svg)

Bu sağlama akışında:

1. Oturum uygulamayı imzalamak için B çalışır kiracıda bir kullanıcı, yetkilendirme uç noktası, uygulama için bir belirteç ister.
1. Kullanıcı kimlik bilgilerini alınan ve kimlik doğrulaması için doğrulandı
1. Kullanıcının B kiracısındaki erişim elde etmek uygulama için onay vermeniz istenir
1. Microsoft kimlik platformu uygulama nesnesi bir kiracıda bir şema bir hizmet sorumlusu B kiracısındaki oluşturmak için kullanır
1. Kullanıcı istenen belirteci alır

Bu işlemi diğer kiracılar için (C, D vb.) istediğiniz kadar tekrarlayabilirsiniz. Kiracı bir uygulama (uygulama nesnesi) için şema korur. Uygulama için onay verilen diğer kiracıların kullanıcıları ve yöneticileri kiracıdaki hizmet sorumlusu nesnesini kullanarak uygulamanın gerçekleştirmesine izin verilen işlemleri belirler. Daha fazla bilgi için [uygulama ve hizmet sorumlusu nesneleri Microsoft Identity platformuna](app-objects-and-service-principals.md).

## <a name="claims-in-microsoft-identity-platform-security-tokens"></a>Microsoft kimlik platformu belirteç içindeki talep

Microsoft kimlik platformu tarafından verilen güvenlik belirteçleri (erişim ve kimlik belirteçlerini) talep veya onayları doğrulandıktan sonra konu hakkında bilgi içerir. Uygulamalar talepleri birçok görev için kullanabilir; bazıları:

* Belirteci doğrulama
* Öznenin dizin kiracısını tanımlama
* Kullanıcı bilgilerini görüntüleme
* Öznenin yetkilendirme durumunu belirleme

Herhangi bir güvenlik belirtecindeki talepler belirteç türüne, kullanıcının kimliğini doğrulamak için kullanılan kimlik bilgisi türüne ve uygulama yapılandırmasına göre değişiklik gösterir.

Aşağıdaki tabloda her Microsoft kimlik platformu tarafından yayılan talep türünü kısa bir açıklaması sağlanmaktadır. Daha ayrıntılı bilgi için bkz. [erişim belirteçlerini](access-tokens.md) ve [kimlik belirteçlerini](id-tokens.md) Microsoft kimlik platformu tarafından verilmiş.

| İste | Açıklama |
| --- | --- |
| Uygulama Kimliği | Belirteci kullanan uygulamayı tanımlar. |
| Hedef kitle | Belirtecin gönderileceği alıcı kaynağını tanımlar. |
| Uygulama Kimlik Doğrulaması Bağlamı Sınıf Başvurusu | İstemcinin kimliğinin nasıl doğrulandığını belirtir (genel istemci veya gizli istemci). |
| Kimlik Doğrulaması Anı | Kimlik doğrulamasının gerçekleştirildiği tarihi ve saati kaydeder. |
| Kimlik Doğrulama Yöntemi | Belirtecin kimlik doğrulama yöntemini (parola, sertifika vb.) belirtir. |
| Ad | Kullanıcının Azure AD'deki adını sağlar. |
| Gruplar | Kullanıcının üyesi olduğu Azure AD gruplarının nesne kimliklerini içerir. |
| Kimlik Sağlayıcı | Belirtecin öznesinin kimliğini doğrulayan kimlik sağlayıcısını kaydeder. |
| Verilme Zamanı | Belirtecin verilme zamanını kaydeder ve bu değer genellikle belirtecin ne kadar güncel olduğunu anlamak için kullanılır. |
| Veren | Belirteci oluşturan STS ve Azure AD kiracısını tanımlar. |
| Soyadı | Kullanıcının Azure AD'deki soyadını sağlar. |
| Ad | Belirtecin konusunu tanımlayan ve okunabilir bir değer sunar. |
| Nesne Kimliği | Öznenin Azure AD'deki değişmez ve benzersiz tanıtıcısını içerir. |
| Roller | Kullanıcıya verilmiş olan Azure AD Uygulama Rollerinin kolay adlarını içerir. |
| `Scope` | İstemci uygulamasına verilmiş olan izinleri belirtir. |
| Özne | Belirtecin bilgi verdiği sorumluyu belirtir. |
| Kiracı Kimliği | Belirteci düzenleyen dizin kiracısının değişmez ve benzersiz tanıtıcısını içerir. |
| Belirteç Ömrü | Belirtecin geçerli olduğu zaman aralığını tanımlar. |
| Kullanıcı Asıl Adı | Öznenin kullanıcı asıl adını içerir. |
| Version | Belirtecin sürüm numarasını içerir. |

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [uygulama türleri ve Microsoft kimlik platformu desteklenen senaryolar](app-types.md)
