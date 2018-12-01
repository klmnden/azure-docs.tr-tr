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
ms.openlocfilehash: 8302a444f28e4fb330a1eedbac9a5da762979d6c
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52681968"
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

Microsoft Translator, birden çok veri merkezi konumlarını dışında sunulur. 6'da şu anda bulunan [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions):

* **Americas:** Batı ABD 2 ve Batı Orta ABD 
* **Asya Pasifik:** Güneydoğu Asya ve Kore Güney
* **Avrupa:** Kuzey Avrupa ve Batı Avrupa

Microsoft Translator metin çevirisi API'si için isteğin geldiği için en yakın veri merkezi tarafından işlenen çoğu durumda isteklerdir. Veri merkezinde hata oluşması halinde, istek bölgenin dışında yönlendirilebilir.

Belirli bir veri merkezi tarafından işlenmek üzere istek zorlamak için istenen bölge uç noktası için API isteğinde genel uç noktası değiştirin:

|Açıklama|Bölge|Temel URL|
|:--|:--|:--|
|Azure|Genel|  api.cognitive.microsofttranslator.com|
|Azure|Kuzey Amerika|   API nam.cognitive.microsofttranslator.com|
|Azure|Avrupa|  API eur.cognitive.microsofttranslator.com|
|Azure|Asya Pasifik|    API apc.cognitive.microsofttranslator.com|


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
    "code":403001,
    "message":"The operation is not allowed because the subscription has exceeded its free quota."
    }
}
```
3 haneli HTTP durum kodu için 3 basamaklı bir sayı daha da ardından 6 basamaklı bir sayı birleştirme kategorilere ayırma hatası hata kodudur. Genel hata kodları şunlardır:

| Kod | Açıklama |
|:----|:-----|
| 400000| İstek girdilerden biri geçerli değil.|
| 400001| "Scope" parametresi geçersiz.|
| 400002| "Kategori" parametresi geçersiz.|
| 400003| Bir dil tanımlayıcısı eksik veya geçersiz.|
| 400004| ("İçin"betik") bir hedef komut tanımlayıcısı eksik veya geçersiz.|
| 400005| Eksik veya geçersiz bir giriş metni.|
| 400006| Dili ve betiği birleşimi geçerli değil.|
| 400018| ("Kimden"betik") bir kaynak betik tanımlayıcısı eksik veya geçersiz.|
| 400019| Belirtilen dil birini desteklenmiyor.|
| 400020| Giriş metin dizideki öğelerin biri geçerli değil.|
| 400021| API Sürüm parametresi eksik veya geçersiz.|
| 400023| Belirtilen dil çifti biri geçerli değil.|
| 400035| Kaynak dil ("Kimden" alanı) geçerli değil.|
| 400036| Hedef Dil ("Kime" alanı) eksik veya geçersiz.|
| 400042| Bir alanın seçenekleri belirtilen ("Seçenekler") geçerli değil.|
| 400043| İstemci izleme kimliği (ClientTraceId alan veya X-ClientTranceId başlığı) eksik veya geçersiz.|
| 400050| Giriş metni çok uzun.|
| 400064| "Çeviri" parametresi eksik veya geçersiz.|
| 400070| Hedef komut dosyaları (ToScript parametresi) sayısı, hedef dil ('parametresi) sayısı eşleşmiyor.|
| 400071| Değer TextType için geçerli değil.|
| 400072| Giriş metni dizisi çok fazla öğe var.|
| 400073| Betik parametresi geçerli değil.|
| 400074| İstek gövdesi JSON geçerli değil.|
| 400075| Dil eşleştirme ve kategori birleşimi geçerli değil.|
| 400077| En fazla istek boyutu aşıldı.|
| 400079| Gelen ve dil arasında çeviri için istenen özel sistem yok.|
| 401000| Kimlik bilgileri eksik veya geçersiz olduğu için istek yetkili değil.|
| 401015| "Konuşma tanıma API'si için sağlanan kimlik bilgileri değildir. Bu istek metin API'si için kimlik bilgilerini gerektirir. "Lütfen Translator Text API aboneliği kullanın."|
| 403000| İşleme izin verilmiyor.|
| 403001| Abonelik, ücretsiz kotasını aştığı için işleme izin verilmiyor.|
| 405000| İstenen kaynak için istek yöntemi desteklenmiyor.|
| 408001| İstenen özel çeviri sistemi henüz kullanılamıyor. Lütfen birkaç dakika sonra yeniden deneyin.|
| 415000| Content-Type üst bilgisi eksik veya geçersiz.|
| 429000, 429001 429002| İstemci çok fazla istek gönderdiği için sunucu isteği reddetti. Azaltmayı önlemek için istekleri sıklığını azaltın.|
| 500000| Beklenmeyen bir hata oluştu. Hata devam ederse, hatanın tarih/saat ile rapor istek tanımlayıcısı yanıt üst bilgisi X-RequestId ve istek üst bilgisi X-ClientTraceId alınan istemci tanımlayıcısı.|
| 503000| Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin. Hata devam ederse, hatanın tarih/saat ile rapor istek tanımlayıcısı yanıt üst bilgisi X-RequestId ve istek üst bilgisi X-ClientTraceId alınan istemci tanımlayıcısı.|

