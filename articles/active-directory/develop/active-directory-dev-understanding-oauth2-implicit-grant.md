---
title: Azure AD içinde izin akışı örtük OAuth2 anlama | Microsoft Docs
description: Akışı vermek örtük OAuth2 Azure Active Directory'nin uygulaması hakkında daha fazla bilgi edinin ve uygulamanız için uygun olup.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 90e42ff9-43b0-4b4f-a222-51df847b2a8d
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/15/2016
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 263a093d5cf4b48ed1dadd4a288e548065ddf282
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34155788"
---
# <a name="understanding-the-oauth2-implicit-grant-flow-in-azure-active-directory-ad"></a>OAuth2 örtük grant akış içinde Azure Active Directory (AD) Anlama
OAuth2 örtük verme OAuth2 belirtimi Güvenlik sorunlarının en uzun listesiyle grant olması için notorious olur. Ve henüz, ADAL JS ve SPA uygulamaları yazarken öneririz biri tarafından uygulanan bir yaklaşımdır. Neler sağlar? Tüm sağlasa da, bileşim olduğu: ve ortaya çıkmıştır gibi örtük grant sizin bir işin peşine düşmek tarayıcıdan JavaScript aracılığıyla bir Web API kullanan uygulamalar için en iyi yaklaşımdır.

## <a name="what-is-the-oauth2-implicit-grant"></a>OAuth2 örtük grant nedir?
Quintessential [OAuth2 yetkilendirme kodu verme](https://tools.ietf.org/html/rfc6749#section-1.3.1) iki ayrı uç noktaları kullanan yetkilendirme verme olur. Yetkilendirme uç noktası, bir kimlik doğrulama kodu sonuçları kullanıcı etkileşimi aşaması için kullanılır. Belirteç uç noktası, ardından bir erişim belirteci ve genellikle de bir yenileme belirteci kodunu değişimi için istemci tarafından kullanılır. Yetkilendirme sunucusu istemci doğrulanabilmesi web uygulamaları, kendi uygulama kimlik bilgilerini belirteç uç noktası için sunmak için gereklidir.

[OAuth2 örtük grant](https://tools.ietf.org/html/rfc6749#section-1.3.2) bir diğer yetkilendirme verir çeşididir. Bir erişim belirteci almak bir istemcinin sağlar (ve kullanırken id_token [Openıd Connect](http://openid.net/specs/openid-connect-core-1_0.html)) belirteç uç noktası arayarak veya istemci kimlik doğrulaması olmadan doğrudan yetkilendirme uç noktasından,. Bu değişken JavaScript tabanlı bir Web tarayıcısında çalışan uygulamaları için özel olarak tasarlanmıştır: özgün OAuth2 belirtimi belirteçleri URI parçadaki döndürülür. Bu belirteç BITS JavaScript kodu istemcisinde kullanılabilmesini sağlar, ancak yeniden yönlendirmeleri sunucunun doğru olarak eklenmeyecek güvence altına alır. Tarayıcı yoluyla belirteçleri döndürme doğrudan yetkilendirme uç noktasından yönlendirir. Ayrıca, JavaScript uygulama belirteç uç noktası başvurmak için gerekliyse, gerekli olan kaynak çağrıları çapraz için herhangi bir gereksinimi ortadan avantajı vardır.

Bir önemli OAuth2 örtük grant gibi istemciye hiçbir zaman dönüş yenileme belirteçleri akar olgu özelliğidir. Biz sonraki bölümde göreceğiniz gibi gerçekten gerekli değildir ve hatta bir güvenlik sorunu olabilir.

## <a name="suitable-scenarios-for-the-oauth2-implicit-grant"></a>OAuth2 örtük vermek için uygun senaryoları
OAuth2 belirtimi bildirir gibi kullanıcı aracısı uygulamaları – yani, bir tarayıcı içinde yürütülen JavaScript uygulamaları etkinleştirmek için örtük grant olup geliştirilmiş. Tanımlama gibi uygulamalar JavaScript kodu sunucu kaynaklarının (genellikle bir Web API) erişmek için ve uygulama UX uygun şekilde güncelleştirmek için kullanılır özelliğidir. Gmail veya Outlook Web Access gibi uygulamalar düşünün: gelen kutusundan bir ileti seçtiğinizde yalnızca ileti görselleştirme paneli sayfaya geri kalanı değiştirilmemiş devam ederken yeni seçim görüntülenecek olarak değişir. Bu, geleneksel yeniden yönlendirme tabanlı Web uygulamaları tersine, burada her kullanıcı etkileşimi tam sayfa geri gönderme ve yeni sunucu yanıtı tam sayfa işlenmesi ile sonuçlanır olur.

Tek sayfa uygulamaları ya da SPAs kendi extreme JavaScript tabanlı yaklaşımı uygulamalar olarak adlandırılan: fikirdir bu uygulamaların yalnızca bir başlangıç HTML sayfası ve ilişkili JavaScript hizmet, tüm sonraki etkileşim Web API çağrıları tarafından yönlendirilen JavaScript aracılığıyla gerçekleştirilir. Ancak, burada uygulama çoğunlukla geri gönderme temelli ancak bazen JS çağrıları gerçekleştirir, karma yaklaşım, seyrek olmayan – örtük Akış kullanımı hakkında tartışma de olanlar için geçerlidir.

Yeniden yönlendirme tabanlı uygulamalar genellikle yaklaşım JavaScript uygulamaları için de çalışmıyor kendi isteklerini tanımlama bilgileri, ancak yoluyla güvenli hale getirin. Tanımlama bilgileri, yalnızca diğer etki alanlarında doğru JavaScript çağrılarını yönlendirilebilir sırasında bunlar için oluşturulmuş etki alanına göre çalışır. Aslında, sık olacaktır durum: Microsoft Graph API, Office API, Azure API – tüm bulunan uygulama Burada sunulan etki dışında çağırma uygulamaları düşünün. JavaScript uygulamaları için büyüyen bir eğilim hiçbir arka uç, 3. taraf Web API'ları kendi iş işlevi uygulamak için % 100 bağlı olmalıdır.

Şu anda, bir Web API çağrıları korumak için tercih edilen yöntem OAuth2 taşıyıcı belirteci yaklaşım yapılan her çağrı tarafından bir OAuth2 erişim belirteci burada eşlik kullanmaktır. Web API gelen erişim belirteci inceler ve içinde gerekli kapsamları bulursa, istenen işlem erişim verir. Örtük akış tanımlama bilgileri açısından çok sayıda avantaj teklifi için bir Web API, erişim belirteçleri almak JavaScript uygulamaları için kullanışlı bir mekanizma sağlar:

* Gerek kaynak çağrıları – çapraz belirteçleri dönüş URI belirteçleri değil hatalı yerleştirilen, garanti yeniden yönlendirmesi zorunlu kayıt belirteçleri güvenilir bir şekilde elde edilebilir
* JavaScript uygulamaları, bunlar hedef – sayıda Web API'leri hiçbir kısıtlama olmaksızın etki alanları için ihtiyaç duydukları kadar erişim belirteçleri alabilir
* Yerel depolama tanımlama bilgisi yönetim uygulamaya donuk iken belirteç önbelleğe alma ve yaşam süresi management üzerinde Tam Denetim verin veya HTML5 özellikleri oturum gibi
* Erişim belirteçleri siteler arası istek sahtekarlığı (CSRF) saldırılara açık değil

Örtük grant akış güvenlik nedenleriyle çoğunlukla yenileme belirteçleri kesmez. Bir yenileme belirteci olarak dar bir şekilde erişim belirteçleri, onu sızmasını durumunda bu nedenle çok daha fazla zarar inflicting çok daha fazla güç verme olarak kapsamlı değildir. Örtük akış belirteçleri URL'de teslim edilir, bu nedenle riskinin yetkilendirme kodu verme içinde daha yüksektir.

Ancak, bir JavaScript uygulaması tüm olanaklarına erişim belirteçleri tekrar tekrar kimlik bilgileri kullanıcıya sormadan yenileme için başka bir mekanizma olduğuna dikkat edin. Uygulama gizli IFRAME yetkilendirme uç noktası Azure ad yeni belirteç isteklerini gerçekleştirmek için kullanabilirsiniz: tarayıcı hala etkin bir oturumu var olduğu sürece (okuyun: bir oturum tanımlama bilgisi varsa) Azure AD etki alanına göre kimlik doğrulama isteği başarıyla kullanıcı etkileşimi gerek kalmadan oluşabilir.

Bu model JavaScript uygulama erişim belirteçleri bağımsız olarak yenileyin ve (kullanıcı daha önce bunları rıza koşuluyla. yenilerini yeni bir API için bile alma olanağı verir. Bu, alma, koruma ve bir yenileme belirteci gibi yüksek değerli yapı koruma eklenen yükünü önler. Sessiz yenileme mümkün kılan yapı, Azure AD oturum tanımlama bilgisi dışında uygulama yönetiliyor. Bu yaklaşımın başka bir avantajı, bir kullanıcı Azure AD'den Azure AD'ye, tarayıcı sekmeleri birini çalıştıran imzalı uygulamalardan herhangi biri kullanılarak oturumu ' dir. Bu Azure AD oturum tanımlama bilgisi silinmesine neden olur ve JavaScript uygulama otomatik olarak imzalanan çıkışı kullanıcı için belirteç yenileme özelliği kaybedersiniz.

## <a name="is-the-implicit-grant-suitable-for-my-app"></a>Örtük grant Uygulamam için uygun mu?
Örtük grant diğer verir'den daha fazla riskine neden olur ve dikkat edilmesi gereken alanları belgelendiğinden. For example, [kötüye kullanılması, erişim belirteci taklit kaynak sahibine örtük akış içinde] [ OAuth2-Spec-Implicit-Misuse] ve [OAuth 2.0 tehdit modeli ve güvenlik konuları][OAuth2-Threat-Model-And-Security-Implications]). Büyük ölçüde bir tarayıcıya uzak bir kaynak tarafından sunulan etkin kod yürütmek uygulamalara olanak tanımak için tasarlanmıştır due için ancak, daha yüksek risk profili olabilir. Uygulamasını planlıyorsanız SPA mimarisi arka uç bileşenlerine sahip veya JavaScript aracılığıyla bir Web API'sini çağırmak düşündüğünüz, belirteç edinme için örtük akış kullanılması önerilir.

Uygulamanızı yerel istemci ise, örtük akış harika uygun değil. Azure AD oturum tanımlama bilgisi yerel istemci bağlamında yokluğu uzun süreli bir oturum sürdürme anlamına gelir uygulamanızdan deprives. Uygulamanızın başka bir deyişle, sürekli olarak yeni kaynaklar için erişim belirteçleri edinirken ister.

Bir arka uç ve arka uç kodu API'den tüketen içeren bir Web uygulaması geliştiriyorsanız örtük akış de iyi bir tercihtir değil. Diğer verir çok daha fazla güç sağlar. Örneğin, OAuth2 istemci kimlik bilgileri sağlama kullanıcı temsilcilerini aksine uygulama kendisi için atanan izinler yansıtacak belirteçleri elde etme yeteneği sağlar. Bu, istemci bir kullanıcının etkin bir oturum vb. ilgilendiğini değil, kaynaklarına programlı erişim bile koruma özelliği olduğu anlamına gelir. Yalnızca, ancak bu tür verir daha yüksek güvenlik garantileri sağlar. Örneğin, kullanıcı tarayıcı üzerinden erişim belirteçleri hiçbir zaman geçiş, şirketlerin tarayıcı geçmişi kaydedilmesini risk ve benzeri. İstemci uygulaması, ayrıca bir belirteç isterken, güçlü kimlik doğrulaması gerçekleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Geliştirici Kaynakları tam bir listesi için Azure AD tarafından akışları desteği protokolleri ve OAuth2 yetkilendirme vermek için başvuru bilgileri de dahil olmak üzere başvurduğu [Azure AD Geliştirici Kılavuzu][AAD-Developers-Guide]
* Bkz: [bir uygulamayı Azure AD ile tümleştirmek nasıl] [ ACOM-How-To-Integrate] ek derinliğe uygulama tümleştirme işlemi için.

<!--Image references-->

<!--Reference style links in use-->
[AAD-Developers-Guide]: active-directory-developers-guide.md
[ACOM-How-And-Why-Apps-Added-To-AAD]: active-directory-how-applications-are-added.md
[ACOM-How-To-Integrate]: active-directory-how-to-integrate.md
[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819
