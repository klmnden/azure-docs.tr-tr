---
title: URI/yanıt URL'si kısıtlamalar ve sınırlamalar - Microsoft kimlik platformu yeniden yönlendirme
description: Yanıt URL'leri/yeniden yönlendirme URL'leri kısıtlamalar ve sınırlamalar
author: SureshJa
ms.author: sureshja
manager: CelesteDG
ms.date: 06/29/2019
ms.topic: article
ms.subservice: develop
ms.service: active-directory
ms.reviewer: lenalepa, manrath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 07be7d0c70193fec88782fea681e33d6b4cf4b40
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67486239"
---
# <a name="redirect-urireply-url-restrictions-and-limitations"></a>Yeniden yönlendirme URI’si/yanıt URL’si kısıtlamaları ve sınırlamaları

Bir yeniden yönlendirme URI'si veya yanıt URL'si, yetkilendirme sunucusu uygulaması için bir kez kullanıcı gönderir konumu başarıyla yetkilendirildi olduğu ve bir yetki kodu veya erişim verilen belirteç. Kod ya da belirtecinde yer alan yeniden yönlendirme URI'si veya yanıt belirteç uygulama kayıt işleminin bir parçası doğru konumunu kaydetmeniz önemlidir.

## <a name="maximum-number-of-redirect-uris"></a>Yeniden yönlendirme URI'leri sayısı

Aşağıdaki tabloda, en fazla yeniden yönlendirme uygulamanızı kaydettiğinizde ekleyebileceğiniz bir URI'leri sayısını gösterir. 

| İmzalanmış hesaplar | Yeniden yönlendirme URI'leri sayısı | Açıklama |
|--------------------------|---------------------------------|-------------|
| Microsoft iş veya Okul hesapları herhangi bir kuruluşun Azure Active Directory (Azure AD) kiracısı içinde | 256 | `signInAudience` uygulama bildiriminde ayarlanmış olarak *AzureADMyOrg* veya *AzureADMultipleOrgs* |
| Kişisel Microsoft hesapları ve iş ve Okul hesapları | 100 | `signInAudience` uygulama bildiriminde alanı *AzureADandPersonalMicrosoftAccount* |

## <a name="maximum-uri-length"></a>URI uzunluğu en fazla

Her yeniden yönlendirme URI'si için bir uygulama kaydı eklediğiniz için en fazla 256 karakter kullanabilirsiniz.

## <a name="restrictions-using-a-wildcard-in-uris"></a>İçinde bir URI'leri bir joker karakter kullanılması kısıtlamaları

Joker karakter URI gibi `https://*.contoso.com`, kullanışlıdır ancak kaçınılmalıdır. URI yönlendir joker karakterler kullanarak güvenlikle ilgili etkileri vardır. OAuth 2.0 belirtimini göre ([3.1.2, RFC 6749 bölümünde](https://tools.ietf.org/html/rfc6749#section-3.1.2)), bir yeniden yönlendirme uç nokta URI'si mutlak bir URI olmalıdır. 

Azure AD uygulama modeli, kişisel Microsoft hesapları oturum ve çalışmak için yapılandırılan uygulamalar için joker karakter URI'ler desteklemiyor ya da Okul hesapları. Ancak, joker karakter URI'ler iş veya Okul hesaplarını bir kuruluşun Azure AD kiracısında bugün üzere yapılandırılan uygulamalar için verilir. 
 
> [!NOTE]
> Yeni [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyimi, geliştiricilerin kullanıcı Arabirimi üzerindeki joker URI'ler eklemelerini izin değil. İş veya Okul hesapları uygulamalar için URI joker karakteri ifadenin ekleme, yalnızca uygulama bildirimi düzenleyicisindeki desteklenir. Bundan sonra yeni uygulamalar joker karakterler yeniden yönlendirme URI'sini kullanmanız mümkün olmayacaktır. Ancak, yeniden yönlendirme joker karakter içeren eski uygulamaları çalışmak URI devam eder.

Daha fazla bir joker karakter yeniden yönlendirme URI'si, ekleme yerine izin verilen üst sınırı'dan yeniden yönlendirme URI'leri senaryonuz gerektiriyorsa, aşağıdaki yaklaşımlardan birini göz önünde bulundurun.

### <a name="use-a-state-parameter"></a>Bir durum parametresi kullanma

Bir alt etki alanı sayısı ve senaryonuz başarılı kimlik doğrulamadan sonra kullanıcılar, başlatıldığı sayfasına yeniden yönlendirmek gerekiyorsa, bir durum parametresi kullanarak faydalı olabilir. 

Bu yaklaşımda:

1. Yetkilendirme uç noktasından aldığınız güvenlik belirteçleri işlemek için uygulama başına "paylaşılan" bir yeniden yönlendirme URI'si oluşturun.
1. Uygulamanız, state parametresinde uygulamaya özgü parametreler (örneğin, alt etki alanı URL'si burada kaynaklanan kullanıcı ya da herhangi bir şey gibi marka bilgilerini) gönderebilirsiniz. State parametresi kullanırken, belirtilen CSRF koruması karşı koruma sağlamak [, RFC 6749 10.12 bölümünde](https://tools.ietf.org/html/rfc6749#section-10.12)). 
1. Uygulamaya özgü parametreleri doğru işlemek için uygulamaya yönelik kullanıcı için diğer bir deyişle deneyimi, uygun uygulama durumu oluşturmak gerekli tüm bilgileri içerir. State parametresi HTML bu nedenle emin olun, Azure AD yetkilendirme uç noktası şeritler HTML bu parametrede içerik geçirmediğimiz.
1. Azure AD yeniden yönlendirme URI'si "paylaşılan" yanıt gönderir, state parametresi uygulama geri gönderir.
1. Uygulama ardından değeri state parametresinde daha fazla kullanıcıya gönderilecek hangi URL'sini belirlemek için kullanabilirsiniz. CSRF koruması için doğrulama emin olun.

> [!NOTE]
> Bu yaklaşım sağlar, böylece kullanıcı olan farklı bir URL için yeniden yönlendirme durum parametresi gönderilen ek parametreleri değiştirmek güvenliği aşılmış bir istemci [açın yeniden yönlendirici tehdit](https://tools.ietf.org/html/rfc6819#section-4.2.4) RFC 6819 içinde açıklanan. Bu nedenle, istemci durumu şifrelenirken veya belirteci yeniden yönlendirme URI'si etki alanı adını doğrulama gibi diğer yollarla bazı doğrulama tarafından bu parametreleri korumanız gerekir.

### <a name="add-redirect-uris-to-service-principals"></a>Eklemek için hizmet sorumluları yeniden yönlendirme URI'leri

Başka bir yaklaşım ise yeniden yönlendirme URI'leri için [hizmet sorumluları](app-objects-and-service-principals.md#application-and-service-principal-relationship) temsil eden bir Azure AD kiracısında uygulama kaydınız. State parametresi kullanamazsınız veya senaryonuz yeni yeniden yönlendirme URI'leri desteklediğiniz her yeni Kiracı için uygulama kaydınızı eklemenizi gerektirir, bu yaklaşımı kullanabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [uygulama bildirimi](reference-app-manifest.md)
