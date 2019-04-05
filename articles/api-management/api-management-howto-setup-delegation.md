---
title: Nasıl kullanıcı kaydı ve ürün aboneliği temsilcisi seçin
description: Azure API Yönetimi'nde bir üçüncü tarafa kullanıcı kaydı ve ürün aboneliği temsilcisi öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.assetid: 8b7ad5ee-a873-4966-a400-7e508bbbe158
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2019
ms.author: apimpm
ms.openlocfilehash: 796bea3c64ef7fc03367707461d13e0ea2514b8b
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59051766"
---
# <a name="how-to-delegate-user-registration-and-product-subscription"></a>Nasıl kullanıcı kaydı ve ürün aboneliği temsilcisi seçin

Temsilci seçme, kaydolun / oturum açma Geliştirici işleme için mevcut Web sitenizi kullanın ve Geliştirici Portalı'nda yerleşik işlevleri kullanarak farklı ürünlere, abonelik sağlar. Bu kullanıcı verileri ve doğrulama adımları, özel bir şekilde gerçekleştirmek, Web sitesi sağlar.

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="delegate-signin-up"> </a>Geliştirici için temsilci seçme, oturum açın ve kaydolun

Geliştirici oturum açma temsilci ve mevcut Web sitenize kaydolmak için sitenizde bir özel temsilci seçme uç nokta oluşturmanız gerekir. API Management Geliştirici portalından başlatılan bu tür bir istek için giriş noktası olarak hareket edecek gerekir.

Son iş akışı şu şekilde olacaktır:

1. Geliştirici tıklamayla oturum açma veya kaydolma bağlantısını API Management Geliştirici Portalı
2. Tarayıcı temsilci seçme uç noktaya yönlendirilir
3. Temsilci seçme uç nokta iade yönlendirir veya oturum açma veya kaydolma için anın kullanıcı Arabirimi sunar.
4. Başarı durumunda, kullanıcı bunlar başlangıcı API Management Geliştirici Portalı sayfasına geri yönlendirilir

Başlamak için şimdi ilk kurulum API yönlendirmek için yönetim, temsilci seçme uç nokta ister. API Management yayımcı Portalı'nda tıklayarak **güvenlik** ve ardından **temsilci** sekmesi. 'Temsilci oturum açma ve kaydolma' etkinleştirmek için onay kutusuna tıklayın.

![Temsilci seçme sayfası][api-management-delegation-signin-up]

* Hangi özel temsilci seçme uç noktanızın URL'sini olması ve girin karar **temsilci seçme uç nokta URL'si** alan. 
* Temsilci kimlik doğrulaması anahtar alanı içinde doğrulama isteği gerçekten Azure API Management'ı geldiğinden emin olmak için size sağlanan imza hesaplamak için kullanılan bir gizli dizi girin. Tıklayabilirsiniz **oluşturmak** API Management'ın sizin için rastgele bir anahtar oluşturmak için düğme.

Oluşturmak için ihtiyacınız artık **temsilci seçme uç nokta**. Bir dizi eylem gerçekleştirmek şunları içerir:

1. Bir isteği şu biçimde alırsınız:
   
   > *http:\//www.yourwebsite.com/apimdelegation?operation=SignIn & returnUrl = {kaynak sayfasının URL'sini} & salt {dize} = & sig = {string}*
   > 
   > 
   
    Sorgu parametreleri için oturum açma / servis talebini oturum:
   
   * **işlem**: ne tür bir temsilci istek olduğu - olabilir tanımlayan **Signın** bu durumda
   * **returnUrl**: kullanıcısının, bir oturum açma tıklanan veya bağlantıyı oluşturan oturum sayfasının URL'si
   * **Salt**: güvenlik karma hesaplama için kullanılan özel bir anahtar güvenlik değerinin dize
   * **Sig**: karma kendi karşılaştırma için kullanılacak bir hesaplanan güvenlik karma hesaplanır
2. İstek Azure API Management'ı (isteğe bağlı, ancak yüksek düzeyde güvenlik için önerilen) geldiğini doğrulayın
   
   * Temel bir dizenin bir HMAC SHA512 karması hesaplanamadı **returnUrl** ve **salt** sorgu parametreleri ([aşağıda sağlanan kod örneği]):
     
     > HMAC (**salt** + '\n' + **returnUrl**)
     > 
     > 
   * Yukarıda hesaplanan karma değerini karşılaştırmak **sig** sorgu parametresi. İki karmalar eşleşiyorsa, sonraki adıma geçmek, aksi takdirde isteği reddeder.
3. Oturum açma isteği alma / kaydolma doğrulayın: **işlemi** sorgu parametresi ayarlanacak "**Signın**".
4. Kullanıcı oturum açma veya kaydolma için kullanıcı Arabirimi ile var
5. Kullanıcı imza, karşılık gelen bir hesap için API Yönetimi'nde oluşturmanız gerekir. [Bir kullanıcı oluşturma] API Management REST API ile. Bunun yapılması, kullanıcı kimliği, kullanıcı deposu ile aynı değer veya takip bir kimlik olarak ayarlandığından emin olun.
6. Ne zaman kullanıcının başarıyla kimliği:
   
   * [bir çoklu oturum açma (SSO) belirteci istemek] API Management REST API aracılığıyla
   * API çağrısından alınan SSO URL'sine returnUrl sorgu parametresi ekleyin:
     
     > Örneğin, https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url 
     > 
     > 
   * kullanıcı için yukarıdaki üretilen URL'ye yeniden yönlendirme

Ek olarak **Signın** işlemi da gerçekleştirebilirsiniz hesap yönetimi önceki adımları izleyerek ve aşağıdaki işlemlerden birini kullanarak:

* **ChangePassword**
* **ChangeProfile**
* **CloseAccount**

Hesap yönetimi işlemleri için aşağıdaki sorgu parametreleri geçmesi gerekir.

* **işlem**: ne tür bir temsilci isteği (ChangePassword, ChangeProfile veya CloseAccount) olduğu tanımlar
* **UserId**: yönetilecek hesabının kullanıcı kimliği
* **Salt**: güvenlik karma hesaplama için kullanılan özel bir anahtar güvenlik değerinin dize
* **Sig**: karma kendi karşılaştırma için kullanılacak bir hesaplanan güvenlik karma hesaplanır

## <a name="delegate-product-subscription"> </a>Ürün aboneliği temsilcisi seçme
Ürün aboneliği temsilcisi seçme, benzer şekilde kullanıcı oturumu açma/artırma için temsilci seçme için çalışır. Son iş akışı şu şekilde olacaktır:

1. Geliştirici, bir ürün API Management Geliştirici Portalı'nda seçer ve abone ol düğmesine tıklar.
2. Tarayıcı, temsilci seçme uç noktaya yönlendirilir.
3. Temsilci seçme uç nokta gerekli ürün aboneliği adımları gerçekleştirir. Bu adımları tasarlamak için size aittir. Bunlar, başka bir sayfaya ek sorular sormak veya basitçe bilgileri depolamak ve herhangi bir kullanıcı eylemi gerektirmeyen fatura bilgi istemek için yeniden yönlendirme içerebilir.

İşlevselliğini etkinleştirmek için **temsilci** sayfasında **ürün aboneliği temsilcisi**.

Ardından, temsilci seçme uç nokta aşağıdaki eylemleri yapar emin olun:

1. Bir isteği şu biçimde alırsınız:
   
   > *http:\/{operation} /www.yourwebsite.com/apimdelegation?operation= & ProductID {abone olmak için product} = & UserID {isteği yapan kullanıcı} = & karmalaştırılır {dize} = & sig = {string}*
   >
   
    Ürün aboneliği çalışması için sorgu parametreleri:
   
   * **işlem**: ne tür bir temsilci istek olduğu tanımlar. Ürün aboneliği için istekleri geçerli seçenekler şunlardır:
     * "Abone": kullanıcı ile belirli bir ürüne abone olmak için bir istek sağlanan kimliği (aşağıya bakın)
     * "Aboneliği iptal et": bir kullanıcının bir ürünü kaldırma isteği
     * "Yenile": isteği aboneliğini (örneğin, süresi dolan)
   * **ProductID**: kullanıcının istenen abone olmak için ürün kimliği
   * **Subscriptionıd**: üzerinde *Unsubscribe* ve *yenileme* -ürün aboneliği kimliği
   * **UserId**: istek için yapılan kullanıcının kimliği.
   * **Salt**: güvenlik karma hesaplama için kullanılan özel bir anahtar güvenlik değerinin dize
   * **Sig**: karma kendi karşılaştırma için kullanılacak bir hesaplanan güvenlik karma hesaplanır

2. İstek Azure API Management'ı (isteğe bağlı, ancak yüksek düzeyde güvenlik için önerilen) geldiğini doğrulayın
   
   * Temel bir dizenin bir HMAC SHA512 işlem **ProductID**, **UserID**, ve **salt** sorgu parametreleri:
     
     > HMAC (**salt** + '\n' + **ProductID** + '\n' + **UserID**)
     > 
     > 
   * Yukarıda hesaplanan karma değerini karşılaştırmak **sig** sorgu parametresi. İki karmalar eşleşiyorsa, sonraki adıma geçmek, aksi takdirde isteği reddeder.
3. Ürün aboneliği içinde istenen işlem türüne göre işlem **işlemi** - Örneğin, faturalandırma, başka sorularınız varsa, vs.
4. Kullanıcı tarafından API Management ürününe başarıyla abone olmadan, tarafındaki ürün kullanıcıya üzerinde abone [REST API'sini çağırmak için ürün aboneliği].

## <a name="delegate-example-code"> </a> Örnek kod

Bu kod örnekleri Göster nasıl için:

* Ele *temsilci seçme doğrulama anahtarı*, yayımcı portalında temsilci ekranın ayarlayın
* Geçirilen returnUrl geçerliliğini kanıtlama ardından imzayı doğrulamak için kullanılan bir HMAC, oluşturun.

ProductID ve düğümündedir kullanıcı kimliği için aynı kodu çalışır.

**C#kod returnUrl karmasını oluşturmak için**

```csharp
using System.Security.Cryptography;

string key = "delegation validation key";
string returnUrl = "returnUrl query parameter";
string salt = "salt query parameter";
string signature;
using (var encoder = new HMACSHA512(Convert.FromBase64String(key)))
{
    signature = Convert.ToBase64String(encoder.ComputeHash(Encoding.UTF8.GetBytes(salt + "\n" + returnUrl)));
    // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
    // compare signature to sig query parameter
}
```

**NodeJS kod returnUrl karmasını oluşturmak için**

```
var crypto = require('crypto');

var key = 'delegation validation key'; 
var returnUrl = 'returnUrl query parameter';
var salt = 'salt query parameter';

var hmac = crypto.createHmac('sha512', new Buffer(key, 'base64'));
var digest = hmac.update(salt + '\n' + returnUrl).digest();
// change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
// compare signature to sig query parameter

var signature = digest.toString('base64');
```

## <a name="next-steps"></a>Sonraki adımlar
Temsilci seçme hakkında daha fazla bilgi için aşağıdaki videoya bakın:

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Delegating-User-Authentication-and-Product-Subscription-to-a-3rd-Party-Site/player]
> 
> 

[Delegating developer sign in and sign up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[bir çoklu oturum açma (SSO) belirteci iste]: https://docs.microsoft.com/rest/api/apimanagement/User/GenerateSsoUrl
[Bir kullanıcı oluşturun]: https://docs.microsoft.com/rest/api/apimanagement/user/createorupdate
[Ürün aboneliği için REST API çağırma]: https://docs.microsoft.com/rest/api/apimanagement/productsubscriptions
[Next steps]: #next-steps
[Aşağıda sağlanan kod örneği]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 
