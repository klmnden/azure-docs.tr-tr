---
title: URL önizleme uç noktasını - Microsoft Bilişsel hizmetler proje | Microsoft Docs
description: URL önizleme uç noktasını özeti.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/29/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: ddd53aa49db01d7a6db397eb285d0854edc59388
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353998"
---
# <a name="project-url-preview-endpoint"></a>Proje URL'si Önizleme uç noktası

URL önizleme API'sı bir uç nokta içerir.

## <a name="endpoint"></a>Uç Nokta
Bir URL önizleme almak için aşağıdaki uç noktası için bir istek gönderin. Üstbilgiler ve URL parametreleri diğer belirtimleri için kullanın.

AL:
````
https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search?q=https://swiftkey.com

````

### <a name="query-parameters"></a>Sorgu parametreleri
|Ad|Değer|Tür|Gerekli|  
|----------|-----------|----------|--------------|  
|q|Önizleme için URL|Dize |Evet|
|güvenli arama|Geçersiz yetişkinlere yönelik içeriğe veya korsan içeriği, 400, hata kodu ile engellenir ve *isFamilyFriendly* bayrağı alınmadı. <p>Davranış yasal yetişkinlere yönelik içeriğe için aşağıdadır. Durum kodu 200, döndürür ve *isFamilyFriendly* bayrağı false olarak ayarlanır.<ul><li>güvenli arama strict =: Başlık, açıklama, URL ve görüntü değil döndürülecek.</li><li>güvenli arama Orta; = Başlık, URL ve açıklama ancak açıklayıcı görüntüsü alın.</li><li>güvenli arama = off; Tüm yanıt nesneleri/öğelerini – başlık, URL, açıklama ve görüntü alma.</li></ul> |Dize|Gerekli değildir. </br> Varsayılan olarak güvenli arama için katı =.| 

## <a name="response-object"></a>Yanıt nesnesi

Aşağıdaki örnekte gösterildiği gibi yanıt HTTP üst bilgilerine ve Web sayfası nesne öznitelikleri olan içeriyor: `name`, `url`, `description`, `isFamilyFriendly`, ve `primaryImageOfPage`.

````
BingAPIs-TraceId: 15AFE52A97AA422F960433A94803F6CE
BingAPIs-SessionId: 40587764F42142D3A8BA99F66B2B3BB6
X-MSEdge-ClientID: 0389E3EDED106B5E1424E82FEC436A56
BingAPIs-Market: en-US
X-MSEdge-Ref: Ref A: 15AFE52A97AA422F960433A94803F6CE Ref B: PAOEDGE0418 Ref C: 2018-03-30T16:36:27Z
{
  "_type": "WebPage",
  "name": "SwiftKey - Smart prediction technology for easier mobile ...",
  "url": "https://swiftkey.com/",
   "description": "Discover the best Android and iPhone and iPad apps for faster, easier typing with emoji, colorful themes and more - download SwiftKey Keyboard free today.",
  "isFamilyFriendly": true,
  "primaryImageOfPage": {
    "contentUrl": "https://swiftkey.com/images/og/default.jpg"
  }
}

````

## <a name="next-steps"></a>Sonraki adımlar
- [C# hızlı başlangıç](csharp.md)
- [Java hızlı başlangıç](java-quickstart.md)
- [JavaScript hızlı başlangıç](javascript.md)
- [Düğüm hızlı başlangıç](node-quickstart.md)
- [Python hızlı başlangıç](python-quickstart.md)
