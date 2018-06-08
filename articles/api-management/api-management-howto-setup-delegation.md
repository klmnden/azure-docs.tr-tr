---
title: Kullanıcı kaydı ve ürün aboneliği nasıl
description: Azure API Management'te üçüncü bir tarafa kullanıcı kayıt ve ürün aboneliği temsilci öğrenin.
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
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 02c3a3d996fa253cf56e551a37e098639bf73533
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "32151952"
---
# <a name="how-to-delegate-user-registration-and-product-subscription"></a>Kullanıcı kaydı ve ürün aboneliği nasıl
Temsilci seçme, geliştirici oturum-açma/kaydolma ve abonelik Geliştirici Portalı'nda yerleşik işlevini kullanarak aksine ürünlere işlemek için varolan Web sitenizi kullanmanıza olanak sağlar. Bu kullanıcı verileri kendi ve özel bir biçimde adımları doğrulama gerçekleştirmek Web sitenizi sağlar.

## <a name="delegate-signin-up"> </a>Geliştirici oturum açma ve kaydolma için temsilci seçme
Geliştirici oturum açma ve kaydolma, varolan Web sitesi için temsilci seçmek için sitenizdeki API Management Geliştirici Portalı'ndan başlatılan böyle bir istek için giriş noktası görevi gören özel temsilci uç noktası oluşturmanız gerekir.

Son iş akışı şu şekilde olacaktır:

1. API Management Geliştirici Portalı oturum açma veya kaydolma bağlantıda Geliştirici tıklar
2. Tarayıcı temsilci uç noktasına yönlendirilir
3. Temsilci uç noktası iade yönlendirir veya ya da kayıt oturum için kullanıcı isteyen kullanıcı Arabirimi gösterir
4. Başarı, kullanıcı bunlar başlattığınız API Management Geliştirici Portalı sayfasına geri yönlendirilir

Başlamak için şimdi ilk kurulum API yönlendirmek için yönetim temsilci uç noktanızı ister. API Management yayımcı Portalı'nda tıklatın **güvenlik** ve ardından **temsilci** sekmesi. 'Temsilci oturum açma ve kaydolma' etkinleştirmek için onay kutusuna tıklayın.

![Temsilci seçme sayfası][api-management-delegation-signin-up]

* Hangi özel temsilci uç nokta URL'sini ve olması içinde girin karar **temsilci uç nokta URL'si** alan. 
* Temsilci kimlik doğrulama anahtar alanı içinde doğrulama isteği Azure API Yönetimi'nden gerçekten geldiğinden emin olun, sağlanan imza hesaplamak için kullanılan bir gizli anahtarı girin. Tıklayabilirsiniz **oluşturmak** API Management sizin için rastgele bir anahtar oluşturmak için düğmesi.

Oluşturmanıza gerek artık **temsilci endpoint**. Çeşitli eylemleri gerçekleştirmek şunları içerir:

1. Bir isteği şu biçimde alırsınız:
   
   > *http://www.yourwebsite.com/apimdelegation?operation=SignIn&returnUrl={URL Kaynak sayfasının} & salt = {dize} & SIG = {dize}*
   > 
   > 
   
    Sorgu parametreleri için oturum açma / oturum durumu:
   
   * **işlem**: olduğu - yalnızca olabilir temsilci istek türünü tanımlayan **Signın** bu durumda
   * **returnUrl**: kullanıcı bir oturum açma veya kaydolma bağlantısına tıklattığınız sayfasının URL'si
   * **Salt**: güvenlik karma hesaplama için kullanılan özel bir salt dizesi
   * **SIG**: kendi karşılaştırma için kullanılacak bir hesaplanmış güvenlik karma karma hesaplanır
2. İstek Azure API Yönetimi'nden (isteğe bağlı, ancak güvenlik için önerilen yüksek oranda) geldiğini doğrulamak
   
   * Temel bir dize HMAC SHA512 karmasını işlem **returnUrl** ve **salt** sorgu parametreleri ([aşağıda sağlanan örnek kod]):
     
     > HMAC (**salt** + '\n' + **returnUrl**)
     > 
     > 
   * Yukarıdaki hesaplanan karma değerini karşılaştırmak **SIG** sorgu parametresi. İki karmalar eşleşiyorsa, sonraki adıma geçin, aksi takdirde isteği reddeder.
3. Sign-ın/oturum-yukarı için bir istek aldığını onaylayın: **işlemi** sorgu parametresi ayarlanacak "**Signın**".
4. Kullanıcının kullanıcı Arabirimi olmadan oturum içinde veya kaydolma sunması
5. Kullanıcı kaydolduğunuz ise karşılık gelen bir hesap için API Management'te oluşturmanız gerekir. [Bir kullanıcı oluşturun] API Management REST API ile. Bunu yaparken, kullanıcı kimliği, kullanıcı deposunda olan aynı veya, izlemek bir kimlik olarak ayarlayın emin olun.
6. Ne zaman kullanıcı başarıyla kimlik doğrulaması:
   
   * [bir çoklu oturum açma (SSO) belirteci iste] API Management REST API aracılığıyla
   * API çağrısından alınan SSO URL'ye returnUrl sorgu parametresi ekleyin:
     
     > Örneğin, https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url 
     > 
     > 
   * kullanıcı için yukarıdaki üretilen URL'sini yeniden yönlendirme

Ek olarak **Signın** işlemi de gerçekleştirebilirsiniz hesap yönetimi önceki adımları izleyerek ve aşağıdaki işlemlerden birini kullanarak:

* **Parola değiştirme**
* **ChangeProfile**
* **CloseAccount**

Hesap yönetimi işlemleri için aşağıdaki sorgu parametreleri geçmesi gerekir.

* **işlem**: (parola değiştirme, ChangeProfile veya CloseAccount) için temsilci seçme isteği türünü tanımlar
* **UserId**: yönetmek için hesabının kullanıcı kimliği
* **Salt**: güvenlik karma hesaplama için kullanılan özel bir salt dizesi
* **SIG**: kendi karşılaştırma için kullanılacak bir hesaplanmış güvenlik karma karma hesaplanır

## <a name="delegate-product-subscription"> </a>Ürün aboneliği için temsilci seçme
Ürün aboneliği için temsilci seçme, benzer şekilde kullanıcı oturum açma/yukarı için temsilci seçme için çalışır. Son iş akışını aşağıdaki gibi olur:

1. Geliştirici API Management Geliştirici Portalı'nda bir ürün seçer ve abone ol düğmesini tıklattığında
2. Tarayıcı temsilci uç noktasına yönlendirilir
3. Temsilci endpoint gerekli ürün abonelik adımları gerçekleştirir - Bu size kalmıştır ve ek sorular sormak veya yalnızca bilgilerini depolamak ve herhangi bir kullanıcı eylemi gerektirmeyen fatura bilgi istemek için başka bir sayfasına yeniden yönlendirmeye gerektirdiği

İşlevselliğini etkinleştirmek için **temsilci** sayfasında **temsilci ürün aboneliği**.

Ardından temsilci uç noktası aşağıdaki eylemleri gerçekleştirir emin olun:

1. Bir isteği şu biçimde alırsınız:
   
   > *http://www.yourwebsite.com/apimdelegation?operation={operation}&productId={product Abone olunacak} & UserID = {isteği yapan kullanıcı} & salt = {dize} & SIG = {dize}*
   > 
   > 
   
    Sorgu parametreleri ürün abonelik örneği için:
   
   * **işlem**: olmasından temsilci istek türünü tanımlar. Ürün abonelik için istekleri geçerli seçenekler şunlardır:
     * "Abone": kullanıcı ile belirli bir ürüne abone olmak için bir istek sağlanan kimliği (aşağıya bakın)
     * "Aboneliği": bir kullanıcı bir üründen aboneliğinizi iptal etmek için bir istek
     * "Yenile": (örneğin, süresi doluyorsa) bir abonelik yenileme isteği
   * **ProductID**: kullanıcının istenen abone olmak için ürün kimliği
   * **UserId**: kendisi için istek yapıldığında kullanıcının kimliği
   * **Salt**: güvenlik karma hesaplama için kullanılan özel bir salt dizesi
   * **SIG**: kendi karşılaştırma için kullanılacak bir hesaplanmış güvenlik karma karma hesaplanır
2. İstek Azure API Yönetimi'nden (isteğe bağlı, ancak güvenlik için önerilen yüksek oranda) geldiğini doğrulamak
   
   * HMAC SHA512 dayalı bir dizenin işlem **ProductID**, ** USERID, ve **salt** sorgu parametreleri:
     
     > HMAC (**salt** + '\n' + **ProductID** + '\n' + **UserID**)
     > 
     > 
   * Yukarıdaki hesaplanan karma değerini karşılaştırmak **SIG** sorgu parametresi. İki karmalar eşleşiyorsa, sonraki adıma geçin, aksi takdirde isteği reddeder.
3. İçinde istenen işlem türüne göre herhangi bir ürün Abonelik işlem gerçekleştirme **işlemi** - Örneğin, fatura, başka sorularınız varsa, vs.
4. Başarıyla abone olma, tarafındaki ürün kullanıcıya üzerinde kullanıcı tarafından API Management ürüne abone [Ürün abonelik için REST API çağırma].

## <a name="delegate-example-code"> </a> Örnek kod
Bu kod örnekleri nasıl alınacağını Göster *temsilci doğrulama anahtarı*, ayarlanmış yayımcı portalına temsilci ekranda geçirilen geçerliliğini kanıtlayan sonra imzayı doğrulamak için kullanılan bir HMAC, oluşturmak için returnUrl. Küçük değişiklik kullanıcı kimliği ve ProductID için aynı kodu çalışır.

**ReturnUrl karmasını oluşturmak için C# kodu**

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

[Delegating developer sign-in and sign-up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[bir çoklu oturum açma (SSO) belirteci iste]: https://docs.microsoft.com/rest/api/apimanagement/User/GenerateSsoUrl
[Bir kullanıcı oluşturun]: https://docs.microsoft.com/rest/api/apimanagement/user/createorupdate
[Ürün abonelik için REST API çağırma]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO
[Next steps]: #next-steps
[aşağıda sağlanan örnek kod]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 
