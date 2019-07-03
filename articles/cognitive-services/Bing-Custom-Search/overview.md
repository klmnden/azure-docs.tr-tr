---
title: Bing Özel Arama API'si nedir?
titlesuffix: Azure Cognitive Services
description: Bing özel arama API'si, önem verdiğiniz konulara özel olarak uyarlanmış arama deneyimleri oluşturmanıza olanak sağlar.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: overview
ms.date: 06/27/2019
ms.author: aahi
ms.openlocfilehash: 067693c3c02d19f3bdab77f315c920b25078e7f5
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542701"
---
# <a name="what-is-the-bing-custom-search-api"></a>Bing Özel Arama API'si nedir?

Bing özel arama API'si, önem verdiğiniz konulara Reklamsız özel arama deneyimleri oluşturmanıza olanak sağlar. Etki alanı belirtebilirsiniz ve PIN, yanı sıra arama, Bing Web sayfalarında artırmak veya özel bir web görünümünü oluşturmak ve alakalı arama sonuçlarını hızla bulmak yardımcı olmak için belirli bir içeriğe indirgeyin. 

## <a name="features"></a>Özellikler

|Özellik  |Açıklama  |
|---------|---------|
|[Özel gerçek zamanlı arama önerileri](define-custom-suggestions.md)     | Bir açılan listedeki kullanıcılar türünüz olarak olarak görüntülenen arama önerileri sağlar.       | 
|[Özel resim arama deneyimleri](get-images-from-instance.md)     | Etki alanları ve Web siteleri özel arama örneğinizin belirtilen görüntülerden aramak etkinleştirin.        |        
|[Özel video arama deneyimleri](get-videos-from-instance.md)     | Etki alanları ve özel arama örneğinizin belirtilen siteleri videolardan aramak etkinleştirin.        |    
|[Özel arama örneği kullanımınızın paylaşın](share-your-custom-search.md)     | İşbirliğine dayalı bir şekilde düzenleyin ve takım üyeleri ile paylaşarak arama örneğinizin test edin.        | 
|[Bir kullanıcı Arabirimi uygulamalarınız ve Web siteleri için yapılandırma](hosted-ui.md)     | İşbirliğine dayalı bir şekilde düzenleyin ve takım üyeleri ile paylaşarak arama örneğinizin test edin.        | 
## <a name="workflow"></a>İş akışı

Özel arama örneği kullanarak oluşturabileceğiniz [Bing özel arama portalı](https://customsearch.ai). Portal, etki alanı, Web siteleri ve Bing arama yapmak istemediğiniz olanları birlikte aranacak istediğiniz Web sayfalarını belirten bir özel arama örneği oluşturmanızı sağlar. Portal için de kullanabilirsiniz: arama deneyimini Önizleme, API'nin sağladığı arama sonuçlarımızda ayarlama ve isteğe bağlı olarak, Web sitelerinin ve uygulamaların oluşturulması için aranabilir kullanıcı arabirimi yapılandırın.

Arama örneğinizin oluşturduktan sonra (ve isteğe bağlı olarak, bir kullanıcı arabirimi), Web sitenize veya uygulamanıza Bing özel arama API'si çağırarak tümleştirebilirsiniz:

![Bing özel arama API'si üzerinden bağlanabilirsiniz gösteren resim](media/BCS-Overview.png "nasıl Bing özel arama çalışır.")


## <a name="next-steps"></a>Sonraki adımlar

Kullanmaya hızlıca başlamak için bkz. [İlk Bing Özel Arama örneğinizi oluşturma](quick-start.md).

Arama örneğinizi özelleştirmeyle ilgili ayrıntılar için bkz. [Özel arama örneği tanımlama](define-your-custom-view.md).

Mutlaka okuyun [Bing kullanım ve görüntü gereksinimleri](./use-and-display-requirements.md) arama sonuçlarında hizmet ve uygulamalarınızı kullanma.

Özel arama uç noktalarının başvuru bilgilerini incelemeniz önerilir. Başvuruda arama sonuçlarını istemek için kullanabileceğiniz uç noktaları, üst bilgiler ve sorgu parametreleri yer alır. Ayrıca yanıt nesnelerinin tanımları da bulunur.

[!INCLUDE [cognitive-services-bing-url-note](../../../includes/cognitive-services-bing-url-note.md)]

- [Özel Arama API’si](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference)
- [Özel Görüntü API'si](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-images-api-v7-reference)
- [Özel Video API'si](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-videos-api-v7-reference)
- [Özel Otomatik Öneri API'si](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-autosuggest-api-v7-reference)

