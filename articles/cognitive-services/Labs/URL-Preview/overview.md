---
title: Proje URL'si Önizleme nedir? -Microsoft Bilişsel hizmetler | Microsoft Docs
description: Proje URL'si Önizleme giriş.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/16/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 6b486e0ab4092bef4fe829a5f166311a572a2900
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353987"
---
# <a name="what-is-project-url-preview"></a>Proje URL'si Önizleme nedir?
URL önizleme uç noktasını URL sorgu parametresini alır ve bir Önizleme'de görüntülenecek bir görüntü için hedef kaynak, kısa bir açıklama ve bir bağlantı adı ile bir JSON yanıtı döndürür. Yanıt ayrıca içeriyor [isFamilyFriendly](url-preview-reference.md#query-parameters) URL yetişkin, korsan veya diğer geçersiz içerik içerip içermediğini gösteren bayrak. 

URL önizleme sonuçları almak için bir GET isteği gönderin ve dahil *Apim abonelik anahtar Ocp* geçerli bir belirteci üstbilgiyle:  
```
https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search?q=https://swiftkey.com

```
Yanıtı: 
````
HTTP Headers:
BingAPIs-TraceId: 3CC74C94769440C0851D9DF0869FCE7F
BingAPIs-SessionId: 52219085A6364692958C9C83983A0DBA
X-MSEdge-ClientID: 13D44DC2DE946B4B0F25460CDF036AD6
BingAPIs-Market: en-US
X-MSEdge-Ref: Ref A: 3CC74C94769440C0851D9DF0869FCE7F Ref B: CO1EDGE0315 Ref C: 2018-04-11T18:47:40Z
{
  "_type": "WebPage",
  "name": "SwiftKey - Smart prediction technology for easier mobile typing",
  "url": "https:\/\/swiftkey.com\/en",
  "description": "Discover the best Android and iPhone and iPad apps for faster, easier typing with emoji, colorful themes and more - download SwiftKey Keyboard free today.",
  "isFamilyFriendly": true,
  "primaryImageOfPage": {
    "contentUrl": "https:\/\/swiftkey.com\/images\/og\/default.jpg"
  }
}

````
## <a name="scenarios"></a>Senaryolar 

URL önizleme API Web kaynakların kısa açıklamaları destekler. Geliştiriciler, Zengin Önizleme deneyimleri oluşturmak için kullanın.  Kullanıcılar, paylaşımı veya Web sayfaları, haber, bloglar, forumlar, vb. yer işareti. Bu API, içerik yönetimi için de kullanılabilir.    

Uygulamaları, Web istekleri uç noktasına URL'ye Önizleme atanmış bir sorgu ile göndermek için URL önizlemeyi kullanın.  JSON yanıt önizleme bilgileri içerir: ad, açıklama kaynak *familyFriendly* bayrağı ve temsili bir görüntü ve çevrimiçi tam kaynağa erişim sağlamak bağlantılar. 

## <a name="terms-of-use"></a>Kullanım koşulları
Yalnızca proje URL önizleme verilerden Önizleme parçacıkları ve resimlerin köprülü sosyal medya üzerinde sohbet bot veya benzer teklifleri paylaşımı son kullanıcı tarafından başlatılan URL'de kaynak sitelerini görüntülemek için kullanın. D değil kopyalama, saklamak veya proje URL'si Önizlemesi'nden aldığınız herhangi bir veriyi önbelleğe. Web sitesi veya içerik sahipleri alabilirsiniz önizlemeleri devre dışı bırakmak için tüm istekleri kabul.

Siz veya bir üçüncü taraf sizin adınıza kullanın, korumak, depolamaz, önbellek, paylaşım, veya test, geliştirme, eğitim, dağıtma veya herhangi bir Microsoft dışı hizmeti kullanılabilir hale getirme URL önizleme API'sinden herhangi bir veri dağıtmak veya özellik. 

## <a name="throttling-requests"></a>İstekleri azaltma

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Sonraki adımlar
- [C# hızlı başlangıç](csharp.md)
- [Java hızlı başlangıç](java-quickstart.md)
- [JavaScript hızlı başlangıç](javascript.md)
- [Düğüm hızlı başlangıç](node-quickstart.md)
- [Python hızlı başlangıç](python-quickstart.md)
