---
title: Microsoft Çeviricisi metin API V3.0 başvurusu | Microsoft Docs
description: V3.0 Microsoft Çeviricisi metin API başvuru belgeleri.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: cfaa9584e833b137b417d9074fbfcf606eb21388
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352702"
---
#<a name="translator-text-api-v30"></a>Çevirici metin API v3.0

## <a name="whats-new"></a>Yenilikler

Sürüm 3 Microsoft Çeviricisi metin API modern bir JSON tabanlı Web API sağlar. Kullanılabilirliğini artırır ve daha az işlemleri ve var olan özellikleri birleştirerek performansı yeni özellikleri sağlar.

 * Bir dilde bir komut dosyasından başka bir komut dosyasına dönüştürmek için harf çevirisi.
 * Bir istek birden fazla dilde çeviri.
 * Dil algılama, çeviri ve harf çevirisi bir isteği.
 * Terimin geri çevirileri ve bağlamında kullanılan terimler gösteren örnekler bulmak için arama alternatif Çeviriler sözlüğe.
 * Daha bilgilendirici dil algılama sonuçları.

## <a name="base-urls"></a>Temel URL'leri

Metin API v3.0 aşağıdaki bulutta kullanılabilir:

| Açıklama | Bölge | Temel URL                                        |
|-------------|--------|-------------------------------------------------|
| Azure       | Genel | api.cognitive.microsofttranslator.com           |


## <a name="authentication"></a>Kimlik Doğrulaması

Microsoft Bilişsel hizmetler Çeviricisi metin API abone olma ve kimlik doğrulaması için abonelik anahtarınızı (Azure Portalı'nda bulunur) kullanın. 

En basit yolu istek üstbilgisi kullanarak Çeviricisi hizmetini Azure gizli anahtarınızı geçirmektir `Ocp-Apim-Subscription-Key`.

Gizli anahtarınızı belirteci Hizmeti'nden bir yetki belirteci almak için kullanılacak bir alternatiftir. Daha sonra çevirici kullanarak hizmet yetkilendirme belirtecini geçirin `Authorization` istek üstbilgisi. Bir yetkilendirme belirteci edinmek için olun bir `POST` aşağıdaki URL'si isteğine:

| Ortam     | Kimlik doğrulama hizmeti URL'si                                |
|-----------------|-----------------------------------------------------------|
| Azure           | `https://api.cognitive.microsoft.com/sts/v1.0/issueToken` |

Gizli bir anahtar verilen bir belirteç elde etmek için örnek isteklerini şunlardır:

```
// Pass secret key using header
curl --header 'Ocp-Apim-Subscription-Key: <your-key>' --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken'
// Pass secret key using query string parameter
curl --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken?Subscription-Key=<your-key>'
```

Başarılı bir istek, yanıt gövdesi düz metin olarak kodlanmış erişim belirteci döndürür. Geçerli bir belirteci Çeviricisi hizmete yetkilendirme taşıyıcı belirteçte olarak geçirilir.

```
Authorization: Bearer <Base64-access_token>
```

Kimlik Doğrulama belirtecini 10 dakika için geçerlidir. Belirteç Çeviricisi API'lerine birden fazla çağrı yapılırken yeniden kullanılan olması gerekir. Programınızı uzun bir süre Çeviricisi API istekleri yaparsa, ancak ardından programınızı yeni bir erişim belirteci düzenli aralıklarla (örneğin 8 dakikada bir) istemeniz gerekir.

Özetlemek için bir istemci isteği Çeviricisi API aşağıdaki tablodan gerçekleştirilecek bir authorization üstbilgisi içerir:

<table width="100%">
  <th width="30%">Üst bilgiler</th>
  <th>Açıklama</th>
  <tr>
    <td>Ocp Apim abonelik anahtarı</td>
    <td>*Gizli anahtarınızı geçirilirse Bilişsel hizmetler abonelikle kullanmak*.<br/>Aboneliğinize Çeviricisi metin API için Azure gizli anahtar değeridir.</td>
  </tr>
  <tr>
    <td>Yetkilendirme</td>
    <td>*Bilişsel hizmetler abonelikle bir kimlik doğrulama belirteci geçirilirse kullanın.*<br/>Taşıyıcı belirteci değer: ' taşıyıcı <token>'.</td>
  </tr>
</table> 

## <a name="errors"></a>Hatalar

Standart hata yanıtı adlı ad/değer çifti ile bir JSON nesnesidir `error`. Ayrıca bir JSON nesnesi özellikleriyle değeridir:

  * `code`: Bir sunucu tarafından tanımlanan hata kodu.

  * `message`: Hata okunabilir bir gösterimini vermiş bir dize.

Örneğin, ücretsiz kota tükendi sonra müşteri ücretsiz bir deneme aboneliği ile şu hatayı alıyorsunuz:

```
{
  "error": {
    "code":403000,
    "message":"The subscription has exceeded its free quota."
    }
}
```
