---
title: Bing Özel Arama nedir?
titlesuffix: Azure Cognitive Services
description: Bing Özel Arama hizmetine üst düzey bir genel bakış sağlar.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: overview
ms.date: 09/29/2017
ms.author: v-brapel
ms.openlocfilehash: 2483bf36bb18af21bc454e08f3321b33094c43c8
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48814313"
---
# <a name="what-is-bing-custom-search"></a>Bing Özel Arama nedir?

Bing Özel Arama API’si, önemsediğiniz konulara özel olarak uyarlanmış arama deneyimleri oluşturmanızı sağlar. Örneğin, arama deneyimi sunan bir web sitesine sahipseniz Bing’in aradığı etki alanlarını, web sitelerini ve web sayfalarını belirtebilirsiniz. Kullanıcılarınız, arama sonucu sayfalarındaki alakasız içeriği ayıklamak zorunda kalmadan içeriğe göre oluşturulan arama sonuçlarını görebilir.

Size özel web görünümünü oluşturmak için Bing Özel Arama [portalını](https://customsearch.ai) kullanın. Portal Bing'in aramasını ve aramamasını istediğiniz etki alanlarını, web sitelerini ve web sayfalarını belirten bir özel arama örneği oluşturmanızı sağlar. Bildiğiniz içerik URL'lerini belirtmeye ek olarak portalı kullanarak eklemek isteyebileceğiniz ilgili içerikleri de keşfedebilirsiniz.

Portal ayrıca kullanıcı belirli bir arama terimini girdiğinde belirli bir web sayfasının arama sonuçlarının en üstünde sabitlemenizi de sağlar. 

Özel arama örneğinizi tanımladıktan sonra Özel Arama API'sini çağırarak web sitenize, masaüstü uygulamanıza veya mobil uygulamanıza ekleyebilirsiniz. Web tabanlı bir siteniz veya uygulamanız varsa barındırılan arabirimin arama arabirimini sizin yerinize oluşturmasını sağlayabilirsiniz.

Aşağıdaki görüntüde özel arama tümleştirmesinin ne kadar kolay olduğu gösterilmektedir.

![picture alt](./media/bcs-overview.png "Bing Özel Arama hizmetinin çalışması.")

## <a name="adding-custom-search-box-suggestions"></a>Özel arama kutusu önerilerini ekleme

Özel arama kutusu önerileriyle özel arama deneyiminizi geliştirebilirsiniz. Bu özellik, arama deneyiminizle ilgili özel arama önerileri sunmanızı sağlar. Kullanıcı arama kutusuna metin yazdığında açılan listede kullanıcı tarafından girilen kısmi dizeye göre sorgu dizeleri önerilir. Yalnızca özel önerileri döndürebilir veya Bing önerilerini dahil edebilirsiniz. [Daha fazla bilgi edinin](define-custom-suggestions.md).

## <a name="adding-custom-image-search-experience"></a>Özel görüntü arama deneyimini ekleme

Görüntülerle özel arama deneyiminizi geliştirebilirsiniz. Özel arama, web sonuçlarına benzer şekilde örneğinizin web sitesi listesinde görüntü aramayı destekler. [Daha fazla bilgi edinin](get-images-from-instance.md).

## <a name="adding-custom-video-search-experience"></a>Özel video arama deneyimini ekleme

Videolarla özel arama deneyiminizi geliştirebilirsiniz. Özel arama, web sonuçlarına benzer şekilde örneğinizin web sitesi listesinde video aramayı destekler. [Daha fazla bilgi edinin](get-videos-from-instance.md).

## <a name="sharing-your-custom-search-instance-with-others"></a>Özel arama örneğinizi başkalarıyla paylaşma

Örneğinizi ekibinizin diğer üyeleriyle paylaşarak ortak düzenleme ve test gerçekleştirilmesini sağlayabilirsiniz. [Daha fazla bilgi edinin](share-your-custom-search.md).

## <a name="next-steps"></a>Sonraki adımlar

Kullanmaya hızlıca başlamak için bkz. [İlk Bing Özel Arama örneğinizi oluşturma](quick-start.md).

Arama örneğinizi özelleştirmeyle ilgili ayrıntılar için bkz. [Özel arama örneği tanımlama](define-your-custom-view.md).

Özel arama uç noktalarının başvuru bilgilerini incelemeniz önerilir. Başvuruda arama sonuçlarını istemek için kullanabileceğiniz uç noktaları, üst bilgiler ve sorgu parametreleri yer alır. Ayrıca yanıt nesnelerinin tanımları da bulunur.

- [Özel Arama API’si](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference)
- [Özel Görüntü API'si](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-images-api-v7-reference)
- [Özel Video API'si](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-videos-api-v7-reference)
- [Özel Otomatik Öneri API'si](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-autosuggest-api-v7-reference)


Arama sonuçlarını kullanma kurallarına uygun hareket ettiğinizden emin olmak için [Bing Kullanım ve Görüntüleme Gereksinimleri](./use-and-display-requirements.md)'ni okumayı unutmayın.