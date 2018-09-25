---
title: Bing Özel Arama nedir? | Microsoft Docs
description: Bing özel arama üst düzey bir genel bakış sağlar
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/29/2017
ms.author: v-brapel
ms.openlocfilehash: b6f50844d6571cca6d63c1db7a85863e3d22d411
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46948086"
---
# <a name="what-is-bing-custom-search"></a>Bing Özel Arama nedir?

Bing özel arama, önem verdiğiniz konulara özel olarak uyarlanmış arama deneyimleri oluşturmanıza olanak sağlar. Örneğin, bir arama deneyimi sağlayan bir Web sitesi sahipseniz, etki alanı, Web siteleri ve Bing arama, Web sayfaları belirtebilirsiniz. Arama sonuçları içerik, önem verdiğiniz yerine uyarlanmış kullanıcıların görmesi aracılığıyla ilgisiz içerik arama sonuçları sayfası.

Özel web görünümünü oluşturmak için Bing özel arama kullanın [portalı](https://customsearch.ai). Portal, etki alanları, Web siteleri ve Bing arama yapmak istediğiniz Web sayfalarını ve arayacak şekilde istemediğiniz Web sitelerini belirten bir özel arama örneği oluşturmanızı sağlar. URL'leri hakkında bilmeniz içeriğin belirtmeye ek olarak, portal eklemek isteyebilirsiniz ilgili içeriği bulmak için de kullanabilirsiniz.

Portal Ayrıca, kullanıcı belirli bir arama terimi girerse belirli bir Web sayfası arama sonucu en üstüne Sabitle sağlar. 

Örneğinizin tanımladıktan sonra özel arama, Web, masaüstü uygulaması veya mobil uygulamanıza özel arama API'si çağırarak tümleştirebilirsiniz. Bir web tabanlı bir site veya uygulama varsa, arama arabirimi işlenmesini barındırılan UI izin verebilirsiniz.

Aşağıdaki görüntüde, özel arama tümleştirme basitliğinin gösterilmektedir.

![Resim alt](./media/bcs-overview.png "nasıl Bing özel arama çalışır.")

## <a name="adding-custom-search-box-suggestions"></a>Özel arama kutusunu önerileri ekleme

Özel arama deneyiminizi özel arama kutusunun öneriler zenginleştirebilirsiniz. Bu özellik, arama deneyiminizle ilgili özel arama önerilerini sağlamanıza olanak tanır. Arama kutusuna kullanıcı türleri olarak açılan listede kullanıcının kısmi sorgu dizesine göre önerilen sorgu dizelerini içerir. Yalnızca özel önerilerinizi döndürür veya Bing önerileri de belirtebilirsiniz. [Daha fazla bilgi edinin](define-custom-suggestions.md).

## <a name="adding-custom-image-search-experience"></a>Özel resim arama deneyimi ekleme

Özel arama deneyiminizi görüntülerle zenginleştirebilirsiniz. Benzer şekilde web sonuçları, Web siteleri listesinin örneğinizin görüntüleri arama özel arama destekler. [Daha fazla bilgi edinin](get-images-from-instance.md).

## <a name="adding-custom-video-search-experience"></a>Özel video arama deneyimi ekleme

Özel arama deneyiminizi videoları zenginleştirebilirsiniz. Benzer şekilde web sonuçları, Web siteleri listesinin örneğinizin videoları arama özel arama destekler. [Daha fazla bilgi edinin](get-videos-from-instance.md).

## <a name="sharing-your-custom-search-instance-with-others"></a>Özel arama örneği kullanımınızın başkalarıyla paylaşma

Kolayca işbirliğine dayalı düzenleme ve takım üyeleri ile paylaşarak örneğiniz test izin verebilirsiniz. [Daha fazla bilgi edinin](share-your-custom-search.md).

## <a name="next-steps"></a>Sonraki adımlar

Hızlıca başlamak için bkz: [ilk Bing özel arama örneğinizin oluşturma](quick-start.md).

Arama örneğinizin özelleştirme hakkında daha fazla ayrıntı için bkz. [özel arama örneği tanımlama](define-your-custom-view.md).

Başvuru içeriği ile her özel arama uç noktaları için edinin. Uç noktaları, üst bilgiler ve arama sonuçlarını istemek için kullanacağınız sorgu parametreleri bir başvuru içerir. Ayrıca yanıt nesnelerinin tanımları da bulunur.

- [Özel arama API'si](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference)
- [Özel görüntü API'si](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-images-api-v7-reference)
- [Özel video API'si](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-videos-api-v7-reference)
- [Özel otomatik öneri API'si](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-autosuggest-api-v7-reference)


Arama sonuçlarını kullanma kurallarına uygun hareket ettiğinizden emin olmak için [Bing Kullanım ve Görüntüleme Gereksinimleri](./use-and-display-requirements.md)'ni okumayı unutmayın.