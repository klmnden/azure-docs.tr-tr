---
title: Bing yerel iş arama API'si sonuçlarını filtrelemek için coğrafi sınırları kullanmanız | Microsoft Docs
titleSuffix: Azure Cognitive Services
description: Bing yerel iş arama API'si arama sonuçlarını filtrelemek hakkında bilgi edinmek için bu makaleyi kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: article
ms.date: 11/01/2018
ms.author: rosh
ms.openlocfilehash: 6da8e9e08f84fa16f22d2a061be28398d064dc8c
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67592691"
---
# <a name="use-geographic-boundaries-to-filter-results-from-the-bing-local-business-search-api"></a>Coğrafi sınırları Bing yerel iş arama API'si sonuçlarını filtrelemek için kullanılır

Bing yerel iş arama API'sini kullanarak aramak istediğiniz belirli coğrafi alanda sınırlar ayarlamanıza imkan sağlar `localCircularView` veya `localMapView` sorgu parametreleri. Yalnızca bir parametresi sorgularınızdaki kullandığınızdan emin olun. 

Bir arama terimi açık bir coğrafi konum içeriyorsa, Bing yerel iş API'si otomatik olarak, arama sonuçlarını sınırlarını ayarlamak için kullanır. Örneğin, bir arama terimi yoksa `sailing in San Diego`, ardından `San Diego` konumu ve diğer sorgu parametrelerinde belirtilen konumlar veya kullanıcı üst bilgiler yok sayılacak kullanılır. 

Coğrafi konum arama terimini algılanan değil ve sorgu parametreleri kullanarak belirtilen coğrafi konum, Bing yerel iş arama API'si isteğinin konumdan belirlemeye çalışacaktır `X-Search-ClientIP` veya `X-Search-Location` üstbilgileri. Hiçbiri üstbilgisi belirtilirse, API istek istemci IP'sini gelen konumunu belirler ve GPS koordinatlarını mobil cihazlar için.

## <a name="localcircularview"></a>localCircularView

`localCircularView` Parametresi döngüsel bir coğrafi bölge etrafında bir RADIUS tarafından tanımlanan, enlem/boylam koordinatları kümesini oluşturur. Bu parametreyi kullanırken, Bing yerel iş arama API'si alınan yanıtları yalnızca bu daire konumlara aksine içerecektir `localMapView` konumları arama alanının dışına biraz içerebilecek parametresi.

Bir döngüsel coğrafi arama alanını belirlemek için bir enlem ve boylam daire ve bir RADIUS metre olarak merkezi olarak hizmet vermek için seçin. Bu parametre sonra bir sorgu dizesi için örneğin eklenerek: `q=Restaurants&localCircularView=47.6421,-122.13715,5000`.

Tam sorgu:

```
https://api.cognitive.microsoft.com/bing/v7.0/localbusinesses/search?q=restaurant&localCircularView=47.6421,-122.13715,5000&appid=0123456789ABCDEF&mkt=en-us&form=monitr
```

## <a name="localmapview"></a>localMapView

`localMapView` Parametresi Kuzeybatı köşeler ve Güneydoğu belirtmek için iki koordinat kullanarak aramak için coğrafi bir dikdörtgen alan belirtir. Bu parametreyi kullanırken, Bing yerel iş arama API'si alınan yanıtları içinde ve yalnızca belirtilen alan dışındaki konumlara aksine içerebilir `localCircularView` yalnızca konumlara arama alanı içeren bir parametre.

Bir dikdörtgen arama alanını belirlemek için iki enlem/boylam koordinatları Güneydoğu yapacak ve sınır Kuzeybatı köşelerini'ı seçin. Güneydoğu koordinatları ilk olarak, aşağıdaki örnekte olduğu gibi tanımlar emin olun: `localMapView=47.619987,-122.181671,47.6421,-122.13715`.

Tam sorgu:

```
https://api.cognitive.microsoft.com/bing/v7.0/localbusinesses/search?q=restaurant&localMapView=47.619987,-122.181671,47.6421,-122.13715&appid=0123456789ABCDEF&mkt=en-us&form=monitr
```

## <a name="next-steps"></a>Sonraki adımlar
- [Yerel işletme arama Java hızlı başlangıç](quickstarts/local-search-java-quickstart.md)
- [Yerel işletme arama C# hızlı başlangıç](quickstarts/local-quickstart.md)
- [Yerel işletme arama düğüm hızlı başlangıç](quickstarts/local-search-node-quickstart.md)
- [Yerel iş arama Python hızlı başlangıç](quickstarts/local-search-python-quickstart.md)
