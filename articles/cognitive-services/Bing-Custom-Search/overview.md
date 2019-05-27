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
ms.date: 03/4/2019
ms.author: aahi
ms.openlocfilehash: e788da047cb0567fc00f27130621a2f21e575dc4
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "61335537"
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
## <a name="workflow"></a>İş Akışı

Özel arama örneği kullanarak oluşturabileceğiniz [Bing özel arama portalı](https://customsearch.ai). Portal, etki alanı, Web siteleri ve Bing arama yapmak istemediğiniz olanları birlikte aranacak istediğiniz Web sayfalarını belirten bir özel arama örneği oluşturmanızı sağlar. Portal için de kullanabilirsiniz: arama deneyimini Önizleme, API'nin sağladığı arama sonuçlarımızda ayarlama ve isteğe bağlı olarak, Web sitelerinin ve uygulamaların oluşturulması için aranabilir kullanıcı arabirimi yapılandırın.

Arama örneğinizin oluşturduktan sonra (ve isteğe bağlı olarak, bir kullanıcı arabirimi), Web sitenize veya uygulamanıza Bing özel arama API'si çağırarak tümleştirebilirsiniz:

![Bing özel arama API'si üzerinden bağlanabilirsiniz gösteren resim](media/BCS-Overview.png "nasıl Bing özel arama çalışır.")


## <a name="next-steps"></a>Sonraki adımlar

Kullanmaya hızlıca başlamak için bkz. [İlk Bing Özel Arama örneğinizi oluşturma](quick-start.md).

Arama örneğinizi özelleştirmeyle ilgili ayrıntılar için bkz. [Özel arama örneği tanımlama](define-your-custom-view.md).

Mutlaka okuyun [Bing kullanım ve görüntü gereksinimleri](./use-and-display-requirements.md) arama sonuçlarında hizmet ve uygulamalarınızı kullanma.

Özel arama uç noktalarının başvuru bilgilerini incelemeniz önerilir. Başvuruda arama sonuçlarını istemek için kullanabileceğiniz uç noktaları, üst bilgiler ve sorgu parametreleri yer alır. Ayrıca yanıt nesnelerinin tanımları da bulunur.

- [Özel Arama API’si](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference)
- [Özel Görüntü API'si](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-images-api-v7-reference)
- [Özel Video API'si](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-videos-api-v7-reference)
- [Özel Otomatik Öneri API'si](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-autosuggest-api-v7-reference)

