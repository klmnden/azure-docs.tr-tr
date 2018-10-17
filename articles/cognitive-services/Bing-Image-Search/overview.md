---
title: Bing Resim Arama nedir?
titleSuffix: Azure Cognitive Services
description: Bing Resim Arama API'si uygulamanızda Bing'in bilişsel resim arama özellikleri kullanmanıza olanak tanır. API ile kullanıcı arama sorguları göndererek Bing Resimler’e benzeyen ilgili ve yüksek kaliteli resimler alıp görüntüleyebilirsiniz.
services: cognitive-services
author: aahill
manager: cgronlun
ms.assetid: 1446AD8B-A685-4F5F-B4AA-74C8E9A40BE9
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: overview
ms.date: 10/11/2017
ms.author: aahi
ms.openlocfilehash: 5d5d69eea3a064679cbc5ddc41891a73e77e55ea
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46295368"
---
# <a name="what-is-bing-image-search"></a>Bing Resim Arama nedir?

Bing Resim Arama API'si uygulamanızda Bing'in bilişsel resim arama özellikleri kullanmanıza olanak tanır. API ile kullanıcı arama sorguları göndererek [Bing Resimler](https://www.bing.com/images)'e benzeyen ilgili ve yüksek kaliteli resimler alıp görüntüleyebilirsiniz.

Bing Resim Arama API'sinin yalnızca resim sonuçları sağladığını unutmayın. Diğer türdeki web içerikleri için [Bing Web Araması API](../bing-web-search/search-the-web.md)'sini, [Video Arama API](https://docs.microsoft.com/azure/cognitive-services/Bing-Video-Search)'sini ve [Haber Arama API](https://review.docs.microsoft.com/azure/cognitive-services/bing-news-search)'sini kullanın.

## <a name="bing-image-search-features"></a>Bing Resim Arama özellikleri

Bing Resim Arama öncelikle bir arama sorgusundaki ilgili resimleri bulur ve gösterir, hizmet ayrıca web'de akıllı ve odaklı resim alma için çeşitli ek özellikler de sağlar.


| Özellik                                                                                                                                                                                 | Açıklama                                                                                                                                                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Gerçek zamanlı arama terimleri önerme](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries#using-and-suggesting-search-terms) | Yazılmaya başladıkları anda önerilen arama terimleri görüntülemek için [Bing Otomatik Öneri API](../bing-autosuggest/get-suggested-search-terms.md)'sini kullanarak uygulama deneyimini iyileştirin. |
| [Resim sonuçlarını filtreleme ve kısıtlama](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-get-images#filtering-images)                       | Sorgu parametrelerini düzenleyerek Bing'in getirdiği resimleri filtreleyin.                                                                                                       |
| [Küçük resimleri kırpma, yeniden boyutlandırma ve görüntüleme](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/resize-and-crop-thumbnails)                                                | Bing Resim Arama tarafından getirilen resimlerin küçük resim önizlemelerini düzenleyin ve görüntüleyin.                                                                                      |
| [Kullanıcı arama sorgularını özetleme ve genişletme](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries#pivoting-the-query)               | Sorgulara Bing'in önerdiği arama terimlerini dahil ederek ve görüntüleyerek arama özelliklerinizi genişletin.                                                                    |
| [Popüler resimler alma](https://review.docs.microsoft.com/azure/cognitive-services/bing-image-search/trending-images)                                                                     | Dünyanın her yanından popüler resimler için bir aramayı özelleştirin.                                                                                                          |

## <a name="workflow"></a>İş akışı

Bing Resim Arama API'si HTTP istekleri yapabilen ve JSON ayrıştırabilen tüm programlama dillerinden kolayca çağrılan bir RESTful web hizmetidir. Hizmeti [REST API](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/quickstarts/csharp?) veya [SDK](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart) kullanarak kullanabilirsiniz.

1. Bing Arama API'lerine erişimi olan bir [Bilişsel Hizmetler API'si hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oluşturun. Azure aboneliğiniz yoksa ücretsiz olarak [hesap oluşturabilirsiniz](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).
2. İstekleri geçerli bir [arama sorgusu](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries) ile API'ye gönderin.
3. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.

## <a name="next-steps"></a>Sonraki adımlar

İlk olarak, Bing Resim Arama API'sinin [etkileşimli tanıtımını](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/) deneyin.
Bu tanırım arama sorgusunu hızlı bir şekilde özelleştirmeyi ve web'de resim aramayı göstermektedir.

API'yi çağırmaya hazır olduğunuzda, bir [Bilişsel hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oluşturun. Azure aboneliğiniz yoksa ücretsiz olarak [hesap oluşturabilirsiniz](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).

İlk API isteğinize hızlı bir şekilde başlamak için aşağıdakileri öğrenebilirsiniz:

* REST API'sini kullanarak [Bing'e arama sorgusu gönderme](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/quickstarts/csharp) veya
* Bing'in getirdiği resimleri SDK kullanarak [isteme ve filtreleme](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart).

## <a name="see-also"></a>Ayrıca bkz.

* [Bing Resim Arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) başvuru bölümü tanımları ve uç noktalar, üst bilgiler, API yanıtları ve görüntü tabanlı arama sonuçları istemek için kullanabileceğiniz sorgu parametreleri hakkında bilgi içerir.

* Bing Arama API'leri ile edinilen içeriğin ve bilgilerin kabul edilebilir kullanımları [Bing Kullanımı ve Görüntü Gereksinimleri](./useanddisplayrequirements.md) konusunda belirtilmektedir.

* [Getting images from the web with the Bing Image Search API](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-get-images) (Bing Resim Arama API'si ile web'den resim alma) konusu resimlerin web'den nasıl aranacağını ve alınacağını açıklamaktadır.

* [Arama sorguları gönderme ve bunlarla çalışma](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries) konusu arama sorgusu yapmayı, özelleştirmeyi ve özetlemeyi açıklamaktadır.
