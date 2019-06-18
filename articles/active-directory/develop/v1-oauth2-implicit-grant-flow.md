---
title: Azure AD'de izin akışı örtük OAuth2 anlama | Microsoft Docs
description: OAuth2 örtük Azure Active Directory uygulaması hakkında daha fazla izin akışı, öğrenin ve uygulamanız için doğru olup olmadığını.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 90e42ff9-43b0-4b4f-a222-51df847b2a8d
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8fe0ee8021ae7e70654a161e37d072195bbc035f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65545253"
---
# <a name="understanding-the-oauth2-implicit-grant-flow-in-azure-active-directory-ad"></a>Azure Active Directory (AD) OAuth2 örtük verme akışı anlama

[!INCLUDE [active-directory-develop-applies-v1](../../../includes/active-directory-develop-applies-v1.md)]

OAuth2 örtük verme, güvenlik kaygıları OAuth2 belirtimi uzun listesi izinle olan için ticaret. Ve ADAL JS ve SPA uygulamaları yazarken öneririz biri tarafından uygulanan yaklaşıma henüz olmasıdır. Neler sağlar? Tüm birkaç ödünler olduğu: ve ortaya çıkmıştır gibi örtük vermeyi sizin daha sonra amacınızın bir tarayıcıdan JavaScript aracılığıyla bir Web API'sini kullanan uygulamalar için en iyi yaklaşımdır.

## <a name="what-is-the-oauth2-implicit-grant"></a>OAuth2 örtük verme nedir?

Quintessential [OAuth2 yetkilendirme kodu verme](https://tools.ietf.org/html/rfc6749#section-1.3.1) kullanan iki ayrı uç noktası yetkilendirme verme olur. Yetkilendirme uç noktası, sonuçları bir yetkilendirme kodu ile kullanıcı etkileşimi aşaması için kullanılır. Belirteç uç noktası, ardından bir erişim belirteci ve genellikle de bir yenileme belirteci için kod değişimi için istemci tarafından kullanılır. Yetkilendirme sunucusu istemcinin kimliğini doğrulamak için web uygulamaları ve belirteç uç noktasına, kendi uygulama kimlik sunmak için gereklidir.

[OAuth2 örtük verme](https://tools.ietf.org/html/rfc6749#section-1.3.2) diğer yetkilendirme vermeleri bir çeşididir. Bir erişim belirteci almak bir istemci sağlar (ve kullanırken id_token [Openıd Connect](https://openid.net/specs/openid-connect-core-1_0.html)) ya da istemci kimliğini doğrulama ve belirteç uç noktasına bağlanılıyor olmadan doğrudan yetkilendirme uç noktasından,. Bu değişken, JavaScript tabanlı bir Web tarayıcısında çalışan uygulamalar için tasarlanmıştır: orijinal OAuth2 belirtimi belirteçleri bir URI parçası döndürülür. Belirteci BITS JavaScript kodunu istemciye kullanılabilmesini sağlar, ancak yeniden yönlendirmeleri sunucunun doğru olarak eklenmeyecek garanti eder. Tarayıcı yoluyla belirteçleri döndüren doğrudan yetkilendirme uç noktasından yönlendirilir. Ayrıca, çapraz JavaScript uygulama ve belirteç uç noktasına başvurun gerekiyorsa, gerekli olan kaynak çağrıları için herhangi bir gereksinimi ortadan avantajına sahiptir.

OAuth2 örtük verme önemli bir özelliğidir, gibi istemciye hiçbir zaman dönüş yenileme belirteçleri akışlar da dağıtılmasıdır. Sonraki bölümde, nasıl bunun gerekli olmadığını gösterir ve aslında bir güvenlik sorunu olabilir.

## <a name="suitable-scenarios-for-the-oauth2-implicit-grant"></a>OAuth2 örtük verme için uygun senaryoları

OAuth2 belirtimi, örtük vermeyi Kullanıcı aracısını uygulamaları – yani, bir tarayıcı içinde yürütülen JavaScript uygulamaları etkinleştirmek için çıkabilecek bildirir. Edici bu tür uygulamalar JavaScript kodu (genellikle bir Web API'si) sunucu kaynaklarına erişim ve uygulama kullanıcı deneyiminde uygun şekilde güncelleştirmek için kullanılan özelliğidir. Gmail veya Outlook Web Access gibi uygulamalar düşünün: bir ileti gelen kutunuzdaki seçtiğinizde, sayfanın geri kalanını değiştirilmemiş devam ederken yeni seçim, görüntülenecek yalnızca ileti görselleştirme paneli değiştirir. Bu özellik geleneksel yeniden yönlendirme tabanlı Web apps farklı olarak, burada her kullanıcı etkileşimi tam sayfa geri gönderme ve yeni sunucu yanıtı tam sayfada çizmeye sonuçları oluyor.

JavaScript tabanlı yaklaşımı, üst düzey için uygulamaları tek sayfa uygulamaları veya Spa'lar çağrılır. Bu uygulamalar yalnızca bir ilk HTML sayfası ve ilişkili JavaScript ile Web API çağrıları tarafından yönlendirilen JavaScript aracılığıyla gerçekleştirilen tüm etkileşiminde hizmet emin olur. Ancak, karma yaklaşımları burada uygulama çoğunlukla geri gönderme temelli ancak bazen JS çağrıları gerçekleştirir, seyrek olmayan – örtük akış kullanımıyla ilgili tartışma de olanlar için geçerlidir.

Yeniden yönlendirme tabanlı uygulamalar genellikle yaklaşım JavaScript uygulamaları için de çalışmıyor isteklerinde tanımlama bilgileri, ancak aracılığıyla güvenli hale getirin. Tanımlama bilgileri, yalnızca diğer etki alanlarında doğru JavaScript çağrılarını yönlendirilebilir ancak bunlar için oluşturulmuş bir etki alanına göre çalışır. Aslında, sık olacak durum: Microsoft Graph API, Office API'si, Azure API uygulama Burada sunulan gelen etki alanı dışındaki tüm bulunan – çağıran uygulamalar düşünün. Artan bir eğilim JavaScript uygulamaları için arka uç, bağlı olan üçüncü taraf Web API'leri, kendi iş işlevi uygulamak için %100 sağlamaktır.

Şu anda, tercih edilen yöntem Web API'sine yapılan çağrıları korumak için OAuth2 taşıyıcı belirteci yaklaşım burada her çağrı bir OAuth2 erişim belirteciyle birlikte sunulduğu kullanmaktır. Web API'si gelen erişim belirteci inceler ve içinde gerekli kapsamları bulursa, istenen işlem için erişim verir. Örtük akış, tanımlama bilgileri açısından çok sayıda avantaj sunan bir Web API'si için erişim belirteçleri almak JavaScript uygulamaları için kullanışlı bir mekanizma sağlar:

* Gerek kaynak çağrıları – çapraz zorunlu kayıt yeniden yönlendirme URI'si belirteçleri dönüş belirteçleri değil gördüğümüz şey de, garanti eder. belirteçlerin güvenilir bir şekilde elde edilebilir
* JavaScript uygulamaları, hedefledikleri – çok Web API'leri hiçbir kısıtlama olmaksızın etki alanları için ihtiyaç duydukları kadar erişim belirteçleri elde edebilirsiniz
* Oturum gibi HTML5 özellikler veya yerel depolama, tanımlama bilgileri yönetim uygulaması için donuktur ise belirteç önbelleğe almayı ve kullanım ömrü yönetimi üzerinde Tam Denetim verin
* Erişim belirteçleri siteler arası istek sahteciliği (CSRF) saldırılarına açık değil

Örtük verme akışı çoğunlukla güvenlik nedenleriyle, yenileme belirteçleri kesmez. Bir yenileme belirteci olarak, sayısı azalacağından, sızmasına durumunda bu nedenle çok fazla zarara inflicting çok daha fazla güç vermek, erişim belirteçleri olarak kapsamlı değildir. Örtük akış, belirteçleri URL'de teslim edilir, bu nedenle riskinin içinde yetkilendirme kodu verme göre olandan yüksektir.

Ancak, bir JavaScript uygulama kullanıcıdan kimlik bilgilerini tekrar tekrar sormadan erişim belirteçlerini yenileme kendi elden başka bir mekanizma vardır. Uygulamanın Azure ad yetkilendirme uç noktasına yönelik yeni belirteç isteklerini gerçekleştirmek için gizli bir iframe kullanın: tarayıcı hala etkin bir oturuma sahip olduğu sürece (okuma: oturum tanımlama bilgisi varsa) Azure AD etki alanına göre kimlik doğrulama isteği olabilir Kullanıcı etkileşimi gerek kalmadan başarıyla oluşur.

Bu model, JavaScript uygulama bağımsız olarak erişim belirteçlerini yenileme ve yenilerini yeni bir API için (kullanıcı daha önce bunlar için onaylı şartıyla) bile alma olanağı verir. Bu, alma, sürdürme ve bir yenileme belirteci gibi bir yüksek değerli yapıt korumaya ek yükünü ortadan kaldırır. Sessiz yenileme gelebilecek yapıt Azure AD oturum tanımlama bilgisinin uygulamanın dışında yönetilir. Bu yaklaşımın başka bir avantajı, bir kullanıcının Azure AD'den Azure AD'ye tarayıcı sekmeleri birini çalıştıran, oturum açmış uygulamalarından herhangi birini kullanarak oturum açabilirsiniz olmasıdır. Bu Azure AD oturum tanımlama bilgisinin silinmesine neden olur ve JavaScript uygulama otomatik olarak kullanıma oturum açmış kullanıcının belirteçleri yenileme özelliği kaybedilmesine neden olur.

## <a name="is-the-implicit-grant-suitable-for-my-app"></a>Uygulamam için örtük vermeyi uygundur?

Örtük vermeyi diğer verir değerinden daha fazla riskine neden olur ve ihtiyacınız dikkat edilmesi gereken alanları iyi belgelenmiştir (örneğin, [kötüye, erişim belirtecini örtük akış, kaynak sahibi taklit] [ OAuth2-Spec-Implicit-Misuse]ve [OAuth 2.0 tehdit modeli ve güvenlik konuları][OAuth2-Threat-Model-And-Security-Implications]). Ancak, büyük ölçüde etkin kod, bir tarayıcıya uzak bir kaynak tarafından sunulan yürütülen uygulamalarda etkinleştirmek için tasarlanmıştır alınmamasından ötürü olabilir, daha yüksek risk profili olur. Planlıyorsanız, bir SPA mimari hiçbir arka uç bileşenlerine sahip veya JavaScript aracılığıyla bir Web API'sini çağırmak mı istiyordunuz, belirteç edinme için örtük akış kullanılması önerilir.

Örtük akış, uygulamanızı bir yerel istemciyse, harika uygun değildir. Azure AD oturum tanımlama bilgisinin yerel bir istemci bağlamında olmaması, uzun süreli oturumunun bakımını yapma toplanabilmesini uygulamanızdan deprives. Yani uygulamanızı sürekli olarak yeni kaynaklar için erişim belirteci alınırken girmesini ister.

Bir arka uç ve arka uç kodunu API'den tüketen içeren bir Web uygulaması geliştiriyorsanız örtük akış ayrıca uygun değildir. Diğer verir çok daha fazla güç sağlar. Örneğin, OAuth2 istemci kimlik bilgileri verme kullanıcı temsilcileri aksine uygulamanın kendisi için atanan izinler yansıtan belirteçleri elde etme yeteneği sağlar. Bu durum, istemci bir kullanıcı etkin bir oturum ve benzeri ilgilendiğini değil, kaynaklarına programlı erişim bile koruma özelliği olduğu anlamına gelir. Sıra, ancak böyle bir onayları daha yüksek güvenlik Güvenceleri sağlar. Örneğin, erişim belirteçleri şu kullanıcı tarayıcıdan hiçbir zaman geçiş, bunlar kullanmayın tarayıcı geçmişini kaydedilmesini risk ve benzeri. İstemci uygulaması, ayrıca bir belirteç isterken güçlü kimlik doğrulaması gerçekleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* OAuth2 yetkilendirme ve protokolleri Azure AD tarafından akışlar destek vermek için başvuru bilgileri de dahil olmak üzere Geliştirici Kaynakları tam listesi için bkz [Azure AD Geliştirici Kılavuzu][AAD-Developers-Guide]
* Bkz: [uygulamanın Azure AD ile tümleştirmek nasıl] [ ACOM-How-To-Integrate] uygulama tümleştirme işlemi hakkında ek ayrıntılı için.

<!--Image references-->

<!--Reference style links in use-->
[AAD-Developers-Guide]:azure-ad-developers-guide.md
[ACOM-How-And-Why-Apps-Added-To-AAD]: active-directory-how-applications-are-added.md
[ACOM-How-To-Integrate]: active-directory-how-to-integrate.md
[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819
