---
title: Bing Yerel İşletme Arama API'si nedir? | Microsoft Docs
titleSuffix: Azure Cognitive Services
description: Bing Yerel İşletme Arama API'si, uygulamalarınızın arama sorguları temelinde yerel konumlar ve işletmeler hakkında bilgi bulmasına olanak tanıyan bir RESTful hizmetidir.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.topic: article
ms.date: 11/01/2018
ms.author: rosh
ms.openlocfilehash: fb3b375a66ac9b05ad26287b7c39ce8e748ad699
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60336720"
---
# <a name="what-is-bing-local-business-search"></a>Bing arama yerel iş nedir?
Bing yerel iş arama API'si arama sorgularına dayalı yerel işletmeler hakkında bilgi, uygulamaların bir RESTful hizmeti olduğu. Örneğin, `q=<business-name> in Redmond, Washington`, veya `q=Italian restaurants near me`. 

## <a name="features"></a>Özellikler
| Özellik | Açıklama |  
| -- | -- | 
| [Yerel işletmeler ve konumları bulma](quickstarts/local-quickstart.md) | Bing yerel iş arama API'si, bir sorgudan yerelleştirilmiş sonuçlarını alır. Sonuçları işletmenin Web sitesi için bir URL içerir ve metin, telefon numarası ve coğrafi konum, görüntüleme dahil olmak üzere: GPS koordinatlarını, şehir, posta adresi |  
| [Coğrafi sınırlar ile yerel sonuçlarını filtreleme](specify-geographic-search.md) | Dairesel bir alanı veya sınırlayıcı kutu kare tarafından belirtilen belirli bir coğrafi alana sonuçlarını sınırlamak için arama parametrelerini olarak koordinatları ekleyin. | 
| [Yerel iş sonuçları kategoriye göre filtreleme](local-categories.md) | Yerel iş sonuçları kategoriye göre arayın. Bu seçenek, ters IP konumu veya çağıranın GPS koordinatlarını iş çeşitli kategorilerde yerelleştirilmiş sonuçları döndürmek için kullanır.|

## <a name="workflow"></a>İş akışı
Bing yerel iş arama API'si, HTTP istekleri ve JSON yanıtlarını ayrıştırabilen herhangi programlama dilinden çağırın. Bu hizmet, REST API kullanılarak erişilebilir.
 
1. Oluşturma bir [Bilişsel hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) Bing arama API'lerine erişim. Azure aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) oluşturabilirsiniz.   
2. URL kodlaması için arama terimlerinizi `q=""` sorgu parametresi. Örneğin, `q=nearby+restaurant` veya `q=nearby%20restaurant`. Sayfalandırma da gerekirse ayarlayın. 
3. Gönderme bir [Bing yerel iş arama API'si isteği](quickstarts/local-quickstart.md) 
4. JSON yanıtı ayrıştırılamadı 

> [!NOTE]
> Şu anda yalnızca yerel iş arama destekler `en-US` Pazar. 
> [!NOTE]
> Şu anda yerel iş arama otomatik öneri desteklemez. 

## <a name="next-steps"></a>Sonraki adımlar
- [Sorgulama ve yanıtlama](local-search-query-response.md)
- [Yerel iş arama hızlı başlangıç](quickstarts/local-quickstart.md)
- [Yerel iş arama API'si başvurusu](local-search-reference.md)
- [Kullanım ve görüntüleme gereksinimleri](use-display-requirements.md)
