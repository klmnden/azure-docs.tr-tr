---
title: Translator metin çevirisi API'si V3.0 başvurusu
titlesuffix: Azure Cognitive Services
description: Translator metin API'si V3.0 için başvuru belgeleri.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: reference
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 9282d8af30cbfb3346394bcd71510faf8d8c8a21
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46129395"
---
# <a name="translator-text-api-v30"></a>Translator metin çevirisi API'si v3.0

## <a name="whats-new"></a>Yenilikler

Translator metin çevirisi API'si 3 sürümünü modern bir JSON tabanlı Web API'si sağlar. Kullanılabilirliğini artırır ve performansı daha az işlemleri ve var olan özellikler birleştirerek yeni özellikler sağlar.

 * Bir dilde bir komut dosyasından başka bir komut dosyasına dönüştürmek için alfabeye.
 * Bir istekte birden çok dil için çeviri.
 * Dil algılama, çeviri ve bir istekteki alfabeye.
 * Sözlük döneminin geri çevirileri ve bağlam içinde kullanılan terimleri gösteren örnekleri bulmak için arama alternatif çevirileri için.
 * Dil algılama sonuçları daha bilgilendirici.

## <a name="base-urls"></a>Temel URL

Metin API'si v3.0 aşağıdaki bulutta kullanılabilir:

| Açıklama | Bölge | Temel URL                                        |
|-------------|--------|-------------------------------------------------|
| Azure       | Genel | api.cognitive.microsofttranslator.com           |


## <a name="authentication"></a>Kimlik Doğrulaması

Translator metin çevirisi API'si Microsoft Bilişsel hizmetler abone olma ve kimlik doğrulaması için abonelik anahtarınızı (Azure portalında kullanılabilir) kullanın. 

En basit yolu Translator hizmetine istek üst bilgisini kullanarak Azure gizli anahtarınızı geçirmektir `Ocp-Apim-Subscription-Key`.

Belirteci Hizmeti'nden bir yetkilendirme belirteci almak için gizli anahtarınızı kullanmaya alternatiftir. Ardından, Translator kullanarak hizmet yetkilendirme belirtecini geçirin `Authorization` isteği üstbilgisi. Bir yetkilendirme belirteci almak için olun bir `POST` isteği şu URL'ye:

| Ortam     | Kimlik doğrulama hizmeti URL'si                                |
|-----------------|-----------------------------------------------------------|
| Azure           | `https://api.cognitive.microsoft.com/sts/v1.0/issueToken` |

Gizli anahtar verilen bir belirteç elde etmek için örnek isteklerini şunlardır:

```
// Pass secret key using header
curl --header 'Ocp-Apim-Subscription-Key: <your-key>' --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken'
// Pass secret key using query string parameter
curl --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken?Subscription-Key=<your-key>'
```

Başarılı bir istek, yanıt gövdesi içinde düz metin olarak kodlanmış bir erişim belirteci döndürür. Geçerli belirteç için Translator hizmeti, yetkilendirme taşıyıcı belirteç olarak geçirilir.

```
Authorization: Bearer <Base64-access_token>
```

Bir kimlik doğrulama belirteci 10 dakika için geçerlidir. Translator API'ları için birden çok çağrı yapılırken belirtecin yeniden kullanılan olmalıdır. Programınızı uzun bir süre Translator API için istek yaparsa, ancak ardından, programınızın yeni bir erişim belirteci düzenli aralıklarla (örneğin 8 dakikada bir) istemeniz gerekir.

Özetlemek gerekirse, bir istemci isteği Translator API'si aşağıdaki tablodan alınan bir yetkilendirme üst bilgisi içerir:

<table width="100%">
  <th width="30%">Üst bilgiler</th>
  <th>Açıklama</th>
  <tr>
    <td>Ocp-Apim-Subscription-Key</td>
    <td>*Gizli anahtarınızı geçirilirse Bilişsel hizmetler abonelikle kullanın*.<br/>Aboneliğinize Translator metin çevirisi API'si için Azure gizli anahtar değeridir.</td>
  </tr>
  <tr>
    <td>Yetkilendirme</td>
    <td>*Bir kimlik doğrulama belirteci geçirilirse, Bilişsel hizmetler abonelikle kullanın.*<br/>Taşıyıcı belirteç değerdir: ' taşıyıcı <token>'.</td>
  </tr>
</table> 

## <a name="errors"></a>Hatalar

Standart hata yanıtı ile ad/değer çifti adlı bir JSON nesnesidir `error`. Ayrıca özelliklere sahip bir JSON nesnesi değerdir:

  * `code`Server tanımlı hata kodu.

  * `message`: Hata kullanıcı tarafından okunabilen bir gösterimini sağlar bir dize.

Örneğin, ücretsiz kotayı bitti sonra ücretsiz deneme aboneliği olan bir müşteri aşağıdaki hatayı alırsınız:

```
{
  "error": {
    "code":403000,
    "message":"The subscription has exceeded its free quota."
    }
}
```
