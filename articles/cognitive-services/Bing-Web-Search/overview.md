---
title: Bing Web araması API'si nedir?
titleSuffix: Azure Cognitive Services
description: Bing Web araması API'si, kullanıcı sorgularına anında yanıt sağlayan bir RESTful hizmetidir. Arama sonuçları; web sayfalarını, görüntüleri, videoları, haberleri, çevirileri ve daha fazlasını içerecek şekilde yapılandırılabilir. Sonuçlar, JSON biçiminde sunulur ve aramayla ilgi düzeyini ve Bing Web Araması aboneliklerinizi temel alır.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: overview
ms.date: 03/12/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: b6e546130efd52312fca62ab4e6a22626778a19e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60648167"
---
# <a name="what-is-the-bing-web-search-api"></a>Bing Web araması API'si nedir?

Bing Web araması API'si, kullanıcı sorgularına anında yanıt sağlayan bir RESTful hizmetidir. Arama sonuçları; web sayfalarını, görüntüleri, videoları, haberleri, çevirileri ve daha fazlasını içerecek şekilde yapılandırılabilir. Bing Web araması, aramanın ilgi düzeyini ve Bing Web araması aboneliklerinizin göre JSON olarak sonuçlar sağlar.

Bu API, bir kullanıcının arama sorgusuyla ilişkili olan tüm içeriğe erişmesi gereken uygulamalar için idealdir. Yalnızca belirli bir sonuç türünü gerektiren bir uygulama oluşturuyorsanız [Bing Resim Arama API'si](../Bing-Image-Search/overview.md), [Bing Video Arama API'si](../Bing-Video-Search/search-the-web.md) veya [Bing Haber Arama API'sini](../Bing-News-Search/search-the-web.md) kullanabilirsiniz. Bing Arama API'lerinin tam listesi için bkz. [Bilişsel Hizmetler API'leri](https://docs.microsoft.com/azure/cognitive-services).

Nasıl çalıştığını görmek ister misiniz? [Bing Web Araması API'si tanıtımımızı](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/) deneyin.

## <a name="features"></a>Özellikler  

Bing Web araması yalnızca anında cevap erişmenizi değil. Ayrıca, ek özellikleri ve arama sonuçlarını kullanıcılarınız için özelleştirmenize olanak tanıyan işlevselliği de sağlar.

| Özellik | Açıklama |
|---------|-------------|
| [Gerçek zamanlı olarak arama terimleri önerme](../bing-autosuggest/get-suggested-search-terms.md) | Yazılmaya başladıkları anda önerilen arama terimleri görüntülemek için Bing Otomatik Öneri API'sini kullanarak uygulama deneyimini iyileştirin. |
| [Sonuçları içerik türüne göre filtreleme ve kısıtlama](filter-answers.md) | Arama sonuçlarını web sayfalarına, görüntülere, videolara, güvenli aramaya ve daha fazlasına yönelik filtreler ve sorgu parametreleri ile özelleştirin ve geliştirin. |
| [Unicode karakterler için isabet vurgulama gerçekleştirme](hit-highlighting.md) | İsabet vurgulama sayesinde arama sonuçlarını kullanıcılara görüntülemeden önce içerdikleri istenmeyen Unicode karakterleri belirleyin ve kaldırın. |
| [Arama sonuçlarını ülkeye, bölgeye ve/veya pazara göre yerelleştirme](supported-countries-markets.md) | Bing Web Araması, otuz altıyı aşkın ülkeyi ve bölgeyi destekler. Arama sonuçlarını belirli bir ülke/bölge veya pazarla kısıtlamak için bu özelliği kullanın. |
| [Bing İstatistikleri ile arama ölçümlerini analiz etme](bing-web-stats.md) | Bing İstatistikleri, çağrı hacmi, en çok kullanılan sorgu dizeleri, coğrafi dağılım ve daha fazlası ile ilgili analizler sunan ücretli bir aboneliktir. |

## <a name="workflow"></a>İş akışı

Bing Web Araması API'si, HTTP isteklerinin yapılabildiği ve JSON yanıtlarının ayrıştırabildiği herhangi bir programlama dili ile kolayca çağrılabilir. [REST API](quickstarts/python.md) veya [Bing Web Araması SDK'ları](web-sdk-python-quickstart.md) kullanılarak hizmete erişilebilir.

1. Bing Arama API'lerine erişimi olan bir [Bilişsel Hizmetler API'si hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oluşturun. Azure aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) oluşturabilirsiniz.  
2. [Bing Web Araması API'sine yönelik bir istek](quickstarts/python.md) gönderin.
3. JSON yanıtını ayrıştırın.

## <a name="next-steps"></a>Sonraki adımlar

* Bing Web Araması API'sine ilk çağrınızı yapmak için [Python hızlı başlangıç](quickstarts/python.md) makalemizden yararlanın.  
* [Tek sayfalı bir web uygulaması oluşturun](tutorial-bing-web-search-single-page-app.md).
* [Web Araması API'si v7 başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) belgelerini gözden geçirin.  
* Bing Web Araması için [kullanım ve görüntüleme gereksinimleri](UseAndDisplayRequirements.md) hakkında daha fazla bilgi edinin.  
