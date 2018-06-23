---
title: Bing özel arama nedir? | Microsoft Docs
description: Bing özel arama üst düzey bir genel bakış sağlar
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/29/2017
ms.author: v-brapel
ms.openlocfilehash: 7cd61fc63d0d7734b842ed222c67c6753da9a418
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352882"
---
# <a name="what-is-bing-custom-search"></a>Bing özel arama nedir?

Bing özel arama uyarlanmış arama deneyimleri hakkında önemli konular için oluşturmanıza olanak sağlar. Örneğin, bir arama deneyimi sağlayan bir Web sitesi sahibiyseniz, etki alanı, Web siteleri ve Bing arar Web sayfaları belirtebilirsiniz. Bunlar çok önem verdiğiniz yerine içerik uyarlanmış arama sonuçları, kullanıcılarınızın görmek ilgisiz içeriğe sahip arama sonuçları aracılığıyla sayfasına.

Web özel görünümünü oluşturmak için Bing özel arama kullanın [portal](https://customsearch.ai). Portal, etki alanları, Web siteleri ve Bing arama yapmak istediğiniz Web sayfalarını ve onu aramak için istemediğiniz Web siteleri belirten bir özel arama örneği oluşturmanızı sağlar. URL'ler hakkında bilmeniz içeriğin belirtmeye ek olarak portal eklemek isteyebilirsiniz ilgili içeriği bulmak için de kullanabilirsiniz.

Portal Ayrıca, kullanıcı belirli bir arama terimi girerse belirli bir Web sayfası arama sonucu üstüne Sabitle olanak tanır. 

Örneğinizi tanımladıktan sonra özel arama Web sitesine, masaüstü uygulaması veya mobil uygulama özel arama API'sini çağırarak tümleştirebilirsiniz. Bir web tabanlı site veya uygulama varsa, arama arabirimi işlenmesini barındırılan UI izin verebilirsiniz.

Aşağıdaki resimde, özel bir arama tümleştirme basitliği gösterir.

![Resim alt](./media/bcs-overview.png "nasıl Bing özel arama çalışır.")

## <a name="customize-search-suggestions"></a>Arama önerilerini özelleştirme

Uygun düzeyde özel aranacak abone (bkz [sayfaları fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), özel arama deneyiminiz yapılan arama önerileri özelleştirebilirsiniz. Otomatik öneri özel API kullanıcı sağlayan bir kısmi sorgu dizesine dayalı önerilen sorguların listesini döndürür. Özel otomatik öneri ile arama deneyiminizi ilgili özel arama önerileri sağlar. Yalnızca özel öneriler dönmek mi, yoksa Bing önerileri eklemek için belirtin. Bing önerileri eklenirse özel öneriler Bing sağlar önerileri önce görünür. Bing önerileri özel arama örneğinizi bağlamına kısıtlanır.

## <a name="next-steps"></a>Sonraki adımlar

Hızlıca başlamak için bkz: [ilk Bing özel arama örneğinizi oluşturmak](quick-start.md).

Arama örneğinizi özelleştirmek için kullanılabilir seçenekler hakkında daha fazla ayrıntı için bkz: [özel arama örneği tanımlama](define-your-custom-view.md).

İle öğrenmeniz [özel arama API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference) başvuru. Başvuru uç noktaları, üstbilgi ve arama sonuçlarını istemek için kullanacağınız sorgu parametreleri listesini içerir. Ayrıca yanıt nesnelerin tanımları içerir.

Öneriler özelleştirmek öğrenmek için bkz: [özel arama önerileri tanımlamak](define-custom-suggestions.md).

Okuduğunuzdan emin olun [Bing kullanın ve görüntü gereksinimleri](./use-and-display-requirements.md) arama sonuçlarını kullanma hakkında kurallardan herhangi birinin kesmeyin şekilde.