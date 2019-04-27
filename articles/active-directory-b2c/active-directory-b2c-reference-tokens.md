---
title: Belirteçleri - Azure Active Directory B2C genel bakış | Microsoft Docs
description: Azure Active Directory B2C'de kullanılan belirteçleri hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 11361bc6ab75e873e1b4081dcfc6492abc093b54
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60316944"
---
# <a name="overview-of-tokens-in-azure-active-directory-b2c"></a>Azure Active Directory B2C belirteçlerinde genel bakış

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD) B2C, her işleme gibi çeşitli güvenlik belirteçleri yayar [kimlik doğrulama akışı](active-directory-b2c-apps.md). Bu belge, biçimi, güvenlik özellikleri ve her belirteç türünün içeriğini açıklar.

## <a name="token-types"></a>Belirteç türleri

Azure AD B2C'yi destekleyen [OAuth 2.0 ve Openıd Connect protokolleri](active-directory-b2c-reference-protocols.md), hale getirir, kimlik doğrulaması ve kaynaklarına güvenli erişim belirteçleri kullanın. Azure AD B2C'de kullanılan tüm belirteçlerin [JSON web belirteçleri (Jwt'ler)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) onaylar taşıyıcı hakkında bilgilerin ve belirtecin konu içerir.

Aşağıdaki belirteçler, Azure AD B2C ile iletişim kullanılır:

- *Kimlik belirteci* -içeren bir JWT talep uygulamanızda kullanıcıları tanımlamak için kullanabilirsiniz. Bu belirteç, güvenli bir şekilde aynı uygulama veya hizmetin iki bileşenleri arasındaki iletişim için HTTP istek gönderilir. Gördüğünüz gibi bir kimlik belirtecinde talep kullanabilirsiniz. Hesap bilgilerini görüntülemek veya erişim denetimi kararları uygulamada için yaygın olarak kullanılır. Kimlik belirteçlerini imzalanmış, ancak şifreli değil. Uygulamanızı veya API kimlik belirteci aldığında, belirteç gerçek olduğunu kanıtlamak için imza doğrulamanız gerekir. Uygulamanızı veya API de geçerli olduğunu kanıtlamak için birkaç talep belirteci doğrulamanız gerekir. Senaryonun gereksinimlerine bağlı olarak bir uygulama tarafından doğrulanmış talep farklılık gösterebilir, ancak uygulamanızı her senaryoda bazı ortak talep doğrulamaları gerçekleştirmeniz gerekir.
- *Erişim belirteci* -içeren bir JWT talep apı'lerinize verilen izinleri belirlemek için kullanabilirsiniz. Erişim belirteçleri imzalanmış, ancak şifreli değildir. Erişim belirteçleri, API'leri ve kaynak sunuculara erişim sağlamak için kullanılır.  API'nizi bir erişim belirteci aldığında, belirteç gerçek olduğunu kanıtlamak için imza doğrulamanız gerekir. API'nizi de geçerli olduğunu kanıtlamak için birkaç talep belirteci doğrulamanız gerekir. Senaryonun gereksinimlerine bağlı olarak bir uygulama tarafından doğrulanmış talep farklılık gösterebilir, ancak uygulamanızı her senaryoda bazı ortak talep doğrulamaları gerçekleştirmeniz gerekir.
- *Belirteci Yenile* -yenileme belirteçleri, yeni kimlik belirteçlerini almak ve bir OAuth 2.0 akışı belirteçlerinde erişmek için kullanılır. Söz konusu kullanıcılarla etkileşime gerek kalmadan uygulamanızla uzun süreli erişim kaynaklara kullanıcılar adına sağlarlar. Yenileme belirteçleri, uygulamanız için donuk. Bunlar Azure AD B2C tarafından verilen inceledi ve yalnızca Azure AD B2C tarafından yorumlanır. Uzun süreli olduklarından, ancak uygulama, bir yenileme belirteci belirli bir süre için en son beklentisi ile yazılmış olmamalıdır. Çeşitli nedenlerle için herhangi bir anda yenileme belirteçleri geçersiz olabilir. Azure AD B2C'ye bir belirteç isteğini yaparak kullanmak uygulamanızı bir yenileme belirteci geçerli olup olmadığını öğrenmek için tek yolu denemektir. Yeni bir belirteç için bir yenileme belirteci şifrenizi kullandığınızda, belirteç yanıt olarak yeni bir yenileme belirteci alırsınız. Yeni bir yenileme belirteci kaydedin. Daha önce istekte kullanılan yenileme belirteci değiştirir. Bu eylem, yenileme belirteçleri için mümkün olan en kısa sürece geçerli kalır garanti yardımcı olur. 

## <a name="endpoints"></a>Uç Noktalar

A [kayıtlı uygulama](tutorial-register-applications.md) belirteçlerini alır ve bu uç noktaları için istek göndererek Azure AD B2C ile iletişim kurar:

- `https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/oauth2/v2.0/authorize`
- `https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/oauth2/v2.0/token`

Uygulamanızı Azure AD B2C'den aldığı güvenlik belirteçleri gelen gelebilir `/authorize` veya `/token` uç noktaları. Ne zaman kimlik belirteçlerini alınan gelen `/authorize` uç nokta, bunların yapılır kullanarak [örtük akış](active-directory-b2c-reference-spa.md), sık kullanılan javascript tabanlı web uygulamaları için oturum kullanıcılar için. Ne zaman kimlik belirteçlerini alınan gelen `/token` uç nokta, bunların yapılır kullanarak [gizli kod akışını](active-directory-b2c-reference-oidc.md), belirteç tarayıcıdan gizli tutar.

## <a name="claims"></a>Talepler

Azure AD B2C'yi kullandığınızda, içerik belirteçlerinizden birinin üzerinde ayrıntılı denetime sahiptir. Yapılandırabileceğiniz [kullanıcı akışları](active-directory-b2c-reference-policies.md) ve [özel ilkeler](active-directory-b2c-overview-custom.md) belirli kullanıcı veri kümelerini uygulamanız için gerekli olan talepleri göndermek için. Bu talep gibi standart özellikleri içerebilir **displayName** ve **emailAddress**. Uygulamalarınız, bu talepler, güvenli bir şekilde kullanıcılar ve isteklerinin kimliğini doğrulamak için kullanabilirsiniz. 

Belirli bir sırayla kimliği belirteçlere talep döndürmedi. Yeni Talep Kimliği belirteçlerinde herhangi bir zamanda tanıtılabilir. Yeni Talep sunulan gibi uygulamanızın çalışmamasına neden olmamalıdır. Ayrıca ekleyebilirsiniz [özel kullanıcı öznitelikleri](active-directory-b2c-reference-custom-attr.md) , taleplerinde.

Aşağıdaki tabloda, kimliği belirteçlerinde beklediğiniz ve Azure AD B2C tarafından verilen belirteçlere erişim talepleri listeler.

| Ad | İste | Örnek değer | Açıklama |
| ---- | ----- | ------------- | ----------- |
| Hedef kitle | `aud` | `90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` | Amaçladığınız alıcının belirtecin tanımlar. Azure AD B2C için İzleyici uygulama kimliğini olduğu. Uygulamanız, bu değeri doğrulamak ve bu eşleşmiyorsa belirteci reddetme. Hedef kitle kaynak ile eşanlamlıdır. |
| Veren | `iss` |`https://{tenant}.b2clogin.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` | Oluşturur ve belirteci döndürür güvenlik belirteci hizmeti (STS) tanımlar. Ayrıca, kullanıcı kimlik doğrulamasının yapıldığı dizini tanımlar. Uygulama belirteci uygun uç noktasından gelen emin olmak için verenin talep doğrulamalıdır. |
| Çıkışı | `iat` | `1438535543` | Belirteç düzenlendiği zaman dönem saatle gösterilir. |
| Sona erme zamanı | `exp` | `1438539443` | Başlangıçtan belirteci geçersiz hale geldiği tarih dönem saatle gösterilir. Uygulamanızın bu talep belirteci ömrü geçerliliğini doğrulamak için kullanması gerekir. |
| Öncesine değil | `nbf` | `1438535543` | Başlangıçtan belirtecin geçerli hale geldiği tarih dönem saatle gösterilir. Bu süre genellikle belirtecin verilmiş saat ile aynıdır. Uygulamanızın bu talep belirteci ömrü geçerliliğini doğrulamak için kullanması gerekir. |
| Version | `ver` | `1.0` | Azure AD B2C tarafından tanımlanan kimlik belirteci sürümü. |
| Kod karması | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Kod karma belirteci ile birlikte bir OAuth 2.0 yetkilendirme kodu verildiğinde bir kimliği belirtece dahildir. Kod karma bir yetkilendirme kodu özgünlüğünü doğrulamak için kullanılabilir. Bu doğrulama gerçekleştirme hakkında daha fazla bilgi için bkz. [Openıd Connect belirtimi](https://openid.net/specs/openid-connect-core-1_0.html).  |
| Erişim belirteci karması | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Bir kimliği belirtece dahildir, yalnızca OAuth 2.0 erişim belirteci ile birlikte belirtecin verildiğinde bir erişim belirteci karması. Bir erişim belirteci karma bir erişim belirteci özgünlüğünü doğrulamak için kullanılabilir. Bu doğrulama gerçekleştirme hakkında daha fazla bilgi için bkz. [Openıd Connect belirtimi](https://openid.net/specs/openid-connect-core-1_0.html)  |
| nonce | `nonce` | `12345` | Nonce belirteç yeniden yürütme saldırıları azaltmak için kullanılan bir stratejidir. Uygulamanızı bir geçici öğe içinde bir yetkilendirme isteği kullanarak belirtebilirsiniz `nonce` sorgu parametresi. İstekte sağladığınız değeri içinde değiştirilmemiş yayıldığını `nonce` yalnızca bir kimlik belirteci talep. Bu talep, istekte belirtilen değerle değerini doğrulamak uygulamanızın sağlar. Uygulamanız kimlik belirteci doğrulama işlemi sırasında bu doğrulaması gerçekleştirmeniz gerekir. |
| Subject | `sub` | `884408e1-2918-4cz0-b12d-3aa027d7563b` | Sorumlu olduğu hakkında bir uygulamanın kullanıcı gibi bilgileri belirteci onaylar. Bu değer sabittir ve yeniden atandı yeniden veya değiştirilemez. Belirteç bir kaynağa erişmek için kullanıldığında gibi güvenli bir şekilde, yetkilendirme denetimleri gerçekleştirmek için kullanılabilir. Varsayılan olarak, konu talep, dizinde kullanıcının nesne kimliği ile doldurulur. |
| Kimlik doğrulaması bağlamı sınıf başvurusu | `acr` | Uygulanamaz | Yalnızca eski ilkeleriyle kullanılır. |
| Güven Framework İlkesi | `tfp` | `b2c_1_signupsignin1` | Kimlik belirteci almak için kullanılan ilke adı. |
| Kimlik doğrulama süresi | `auth_time` | `1438535543` | Hangi kullanıcı kimlik bilgileri, en son girilen saati dönem saatle gösterilir. |
| Kapsam | `scp` | `Read`| Kaynak bir erişim belirteci için verilen izinler. Birden çok verilen izinler, boşlukla ayrılır. |
| Yetkili taraf | `azp` | `975251ed-e4f5-4efd-abcb-5f1a8f566ab7` | **Uygulama kimliği** istemci uygulamasının isteği başlatıldı. |

## <a name="configuration"></a>Yapılandırma

Aşağıdaki özellikler için kullanılan [güvenlik belirteçlerinin ömrü yönetme](configure-tokens.md) Azure AD B2C tarafından yayılan:

- **Erişim ve kimlik belirteci ömrü (dakika)** - korunan bir kaynağa erişmek için kullanılan OAuth 2.0 taşıyıcı belirtecinin ömrü. Varsayılan değer 60 dakikadır. (Sınırlar dahil) en az 5 dakikadır. (Sınırlar dahil) en fazla 1440 dakika olmalıdır.

- **Yenileme belirteci ömrü (gün)** - önüne bir yenileme belirteci yeni erişim veya kimlik belirteci almak için kullanılabilir en uzun süre. Uygulamanızı verilmişse, yeni bir yenileme belirteci alınırken zaman aralığını da kapsar `offline_access` kapsam. Varsayılan değer 14 gündür. (Sınırlar dahil) en az bir gündür. (Sınırlar dahil) en fazla 90 gündür.

- **Yenileme belirteci kayan pencere ömrü (gün)** - kullanıcının kimliğinin yeniden kimlik doğrulaması yapın, bağımsız olarak geçerlilik süresi en son yenileme belirtecinin uygulama tarafından bu zaman süre geçtikten sonra. Anahtar ayarlanırsa yalnızca sağlanabilir **sınırlanmış**. Büyük veya buna eşit olması gereken **yenileme belirteci ömrü (gün)** değeri. Anahtar ayarlanırsa **Unbounded**, belirli bir değerin sağlayamaz. Varsayılan değer 90 gündür. (Sınırlar dahil) en az bir gündür. (Sınırlar dahil) en fazla 365 gündür.

Aşağıdaki kullanım örnekleri, bu özellikleri kullanarak etkinleştirilir:

- Kullanıcı uygulamayı sürekli olarak etkin olduğu sürece, bir mobil uygulamaya oturum açık kalsın açmasına izin verin. Ayarlayabileceğiniz **yenileme belirteci kayan pencere ömrü (gün)** için **Unbounded** oturum açma kullanıcı akışınıza.
- Uygun erişim belirteç ömrünü ayarlayarak, sektörün güvenlik ve uyumluluk gereksinimlerinizi karşılayın.

Kullanıcı akışları parola sıfırlama için bu ayarlar kullanılamaz. 

## <a name="compatibility"></a>Uyumluluk

Aşağıdaki özellikler için kullanılan [belirteç uyumluluk yönetme](configure-tokens.md):

- **Verici (iss) talebi** -bu özellik, belirteci veren Azure AD B2C kiracısı tanımlar. Varsayılan değer `https://<domain>/{B2C tenant GUID}/v2.0/` şeklindedir. Değerini `https://<domain>/tfp/{B2C tenant GUID}/{Policy ID}/v2.0/` kimlikleri hem Azure AD B2C kiracısı hem de belirteci istekte kullanılan kullanıcı akışı içerir. Uygulamanızın veya kitaplık ile uyumlu olması için Azure AD B2C gerekip gerekmediğini [Openıd Connect bulma 1.0 belirtimi](https://openid.net/specs/openid-connect-discovery-1_0.html), bu değeri kullanın.

- **Konu (sub) talebi** -belirteç için bilgilerini onayladığı varlık bu özelliği tanımlar. Varsayılan değer **objectID**, hangi doldurur `sub` kullanıcının nesne kimliği ile belirtecinde talep. Değerini **desteklenmiyor** yalnızca geriye dönük uyumluluk için sağlanır. İçin geçmeniz özellikle önerilir **objectID** yapabilecekleriniz hemen sonra.

- **İlke Kimliğini temsil eden talep** -bu özellik, belirteç istekte kullanılan ilke adı doldurulur talep türü tanımlar. Varsayılan değer `tfp` şeklindedir. Değerini `acr` yalnızca geriye dönük uyumluluk için sağlanır.

## <a name="pass-through"></a>Geçiş

Kullanıcı yolculuğu başladığında, Azure AD B2C'yi bir kimlik sağlayıcısından gelen bir erişim belirteci alır. Azure AD B2C kullanıcı hakkında bilgi almak için bu belirteci kullanır. [Kullanıcı akışınızı bir talebi etkinleştirme](idp-pass-through-user-flow.md) veya [talep özel ilkenizi tanımlayın](idp-pass-through-custom.md) belirteci aracılığıyla Azure AD B2C'de kayıt uygulamaları geçirilecek. Uygulamanızı kullanarak bir [v2 kullanıcı akışı](user-flow-versions.md) talep olarak belirtece geçirerek avantajlarından yararlanmak için.

Azure AD B2C şu anda yalnızca Facebook ve Google OAuth 2.0 kimlik sağlayıcıları erişim belirtecini geçirme destekler. Diğer tüm kimlik sağlayıcıları için talep boş olarak döndürülür. 

## <a name="validation"></a>Doğrulama

Bir belirteci doğrulamak için uygulamanızın imza ve talep belirtecinin denetlemelisiniz. Birçok açık kaynak kitaplıkları Jwt'ler, tercih ettiğiniz dili bağlı olarak doğrulamak için kullanılabilir. Bu, bu seçenekleri keşfedin yerine kendi doğrulama mantığı uygulama önerilir.

### <a name="validate-signature"></a>İmza doğrulama

JWT'nin üç segmentleri içeren bir *üstbilgi*, *gövdesi*ve *imza*. İmza segment, böylece uygulamanız tarafından güvenilen belirteç özgünlüğünü doğrulamak için kullanılabilir. Azure AD B2C belirteçleri, RSA 256 gibi endüstri standardı asimetrik şifreleme algoritmaları kullanarak imzalanmıştır. 

Belirteç üstbilgisi, belirteç imzalamak için kullanılan anahtar ve şifreleme yöntemi hakkında bilgi içerir:

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

Değerini **algoritma** talep olduğundan, belirteç imzalamak için kullanılan algoritma. Değerini **çocuk** talep olduğunu belirteç imzalamak için kullanılan ortak anahtar. Herhangi bir zamanda Azure AD B2C genel-özel anahtar çiftleri kümesini herhangi birini kullanarak bir belirteç oturum açabilirsiniz. Azure AD B2C, düzenli aralıklarla anahtarları olası kümesini döndürür. Bu anahtar değişiklikleri otomatik olarak işlemek için uygulamanızı yeniden yazılması gerekir. 24 saatte bir Azure AD B2C tarafından kullanılan ortak anahtarlar için güncelleştirmeleri denetlemek için makul bir sıklığıdır.

Azure AD B2C'yi bir Openıd Connect meta veri uç noktası vardır. Bu uç noktayı kullanarak, uygulamaları Azure AD B2C hakkında bilgi çalışma zamanında isteyebilir. Bu bilgiler, uç noktaları, belirteç içeriği ve belirteç imzalama anahtarı içerir. Azure AD B2C kiracınızı her ilke için bir JSON meta verileri belgesi içerir. Meta veri belgesi birçok yararlı bilgi parçalarını içeren bir JSON nesnesidir. Meta veriler içeren **jwks_uri**, ortak anahtar kümesini konumunu, sağlayan Belirteçleri imzalamak için kullanılır. Konum, ama verilmiştir, konum meta veri belgesi kullanarak ve ayrıştırma dinamik olarak almak en iyi **jwks_uri**:

```
https://contoso.b2clogin.com/contoso.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_signupsignin1
```
JSON belgesini bu URL'de bulunan tüm ortak anahtar bilgilerini belirli bir anda kullanımda içerir. Uygulamanız kullanabilir `kid` belirli bir belirteç imzalamak için kullanılan JSON belgesinde ortak anahtarı seçmek için JWT üst bilgisindeki talep. Doğru ortak anahtarı ve belirtilen algoritması kullanarak onu imza doğrulaması daha sonra gerçekleştirebilirsiniz.

Meta veri belgesi için `B2C_1_signupsignin1` ilkesinde `contoso.onmicrosoft.com` Kiracı konumu:

```
https://contoso.b2clogin.com/contoso.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_signupsignin1
```

İlkeyi bir belirteç imzalamak için kullanılan (ve meta veri isteği için yapılması gerekenler) belirlemek için iki seçeneğiniz vardır. İlk olarak, ilke adı dahil `acr` belirtecinde talep. Gövde çözme ve sonuçları bir JSON dizesini seri durumdan çıkarılırken 64 tabanlı tarafından talep JWT gövdesinin dışında ayrıştırabilirsiniz. `acr` Talep, belirteci vermek için kullanılan ilke adıdır. Diğer seçenek değerini ilkesinde şifrelemektir `state` isteği ve hangi ilke kullanılan belirlemek için kod çözme parametresi. Her iki yöntem geçerli değil.

Bu belgenin kapsamı dışında imza doğrulaması gerçekleştirme açıklamasıdır. Birçok açık kaynak kitaplıkları, bir belirteç doğrulamanıza yardımcı olmak kullanılabilir.

### <a name="validate-claims"></a>Talep doğrula

Uygulamalar veya API kimlik belirteci aldığında, talepleri karşı çeşitli denetimleri kimliği belirteçteki gerçekleştirmelisiniz. Aşağıdaki talep iade edilmelidir:

- **Hedef kitle** -kimlik belirteci uygulamanıza verilmesi amaçlanmamıştır doğrular.
- **öncesine** ve **süre sonu** -kimlik belirteci dolmadığından doğrular.
- **veren** -belirteç uygulamanızı Azure AD B2C tarafından verildiğini onaylar.
- **nonce** -bir belirteç yeniden yürütme saldırısı riskini azaltma stratejisi.

Uygulamanızı gerçekleştirmesi gereken doğrulamaları tam bir listesi için başvurmak [Openıd Connect belirtimi](https://openid.net).  

## <a name="next-steps"></a>Sonraki adımlar

Kullanma hakkında daha fazla bilgi edinin [erişim belirteçleri kullanma](active-directory-b2c-access-tokens.md).

