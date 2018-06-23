---
title: Bing Öngörüler örnekleri | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Görüntü Öngörüler aratıp üzerinde gösterilen örnekleri gösterilir.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 04/17/2018
ms.author: scottwhi
ms.openlocfilehash: 102bd0e916491738d74956c209829008d779bcbf
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354130"
---
# <a name="examples-of-bing-insights-usage"></a>Bing Öngörüler kullanım örnekleri

Bu bölümde Bing üzerinde aratıp Öngörüler nasıl görüntülenebilir örnekler yer almaktadır.

## <a name="pagesincluding-insight-example"></a>PagesIncluding Insight örneği

Aşağıdaki Bing görüntüyü içeren Web sayfalarının nasıl görüntülenebilir gösterir. Örnek ilk Web sayfasına bağlantı görüntüler ve kullanıcının genişletme ve daraltma görüntüyü içeren diğer Web sayfalarının listesini sağlar.

![Genişletilmiş sayfalar dahil olmak üzere](./media/pages-including.PNG)


## <a name="shoppingsources-insight-example"></a>ShoppingSources Insight örneği

Aşağıdaki görüntüde görülen ürünleri için alışveriş kaynakları Bing nasıl görüntülenebilir gösterir.

![Alışveriş kaynakları](./media/shopping-sources.PNG)


## <a name="visualsearch-insight-example"></a>VisualSearch Insight örneği

Bing görsel olarak benzer görüntüleri nasıl görüntülenebilir aşağıdaki gösterilir (bkz **ilgili görüntüleri** örnekte).

![Görsel olarak benzer görüntüleri](./media/similar-images.PNG)

## <a name="recipes-insight-example"></a>Tarif Insight örneği

Aşağıdaki görüntüde gösterilen yemek tarif Bing nasıl görüntülenebilir gösterir. Örnek tarif kullanılabilir olduğunu biliyor olanak tanır.

![Tarif ve sayfalar dahil olmak üzere](./media/recipes-pages-including.PNG)

 Kullanıcı listesi genişletirken tarif bağlantısını sağlar.

![Genişletilmiş tarif sayfalar dahil olmak üzere](./media/expanded-recipes-pages-including.PNG)


## <a name="relatedsearches-insight-example"></a>RelatedSearches Insight örneği

Aşağıdaki Bing ilişkili aramaları başkaları tarafından yapılan görüntülerinin nasıl görüntülenebilir gösterir. Kullanıcı görüntü tıklarsa, kullanıcı için ilgili sorgulayan Bing.com/images arama sonuçları sayfasına alınır.

![Görüntüleri için ilgili aramalar](./media/bordered-related-searches.PNG)


## <a name="entity-insight-example"></a>Varlık Insight örneği

Aşağıdaki görüntüde gösterilen varlık (kişi, yer veya şey) hakkında bilgi Bing nasıl görüntülenebilir gösterir. Kullanıcının varlık bağlantıya tıklar, kullanıcının varlık aratıp arama sonuçları sayfasına alınır.

![Görüntüde gösterilen varlık](./media/entity.PNG)


## <a name="displaying-other-insights-that-the-user-might-explore"></a>Kullanıcı keşfedebilirsiniz diğer Öngörüler görüntüleme

Aşağıdaki kullanıcı keşfedebilirsiniz görüntü ile ilgili diğer bilgileri Bing nasıl görüntülenebilir gösterir.

![Görüntü ile ilgili diğer Öngörüler keşfedin](./media/apple-pie-more-tags.PNG)


## <a name="bounding-boxes-and-hot-spots"></a>Sınırlayıcı kutu ve etkin noktalar

Varsayılan dışı etiketleri etiket uygulandığı görüntü ilgi tanımlayan sınırlama kutusu içerir. Sınırlama kutusu görüntünün tamamını tanımlamıyorsa, görüntüde etkin nokta oluşturmak için sınırlayıcı kutu kullanın. Kullanıcı etkin nokta (veya dikdörtgen) altında bulunan içeriğin ilgili bilgi almak için etkin nokta tıklatabilirsiniz. Görüntü yüksek şekilde görüntü ise, örneğin, sonuçları görüntüde gösterilen Donatılar etiketleri (ve sınırlayıcı kutuları) içerebilir bir çantanızda, mücevher, scarfs, vb. gibi. Aşağıdaki örnekte görüldüğü Gözlüklü için etkin nokta dikdörtgen gösterir.

![Sınırlayıcı kutu ve etkin nokta](./media/click-to-search.PNG)



## <a name="next-steps"></a>Sonraki adımlar

Bu örnekler arkasında JSON kullanıma için bkz: [varsayılan Öngörüler](default-insights-tag.md) ve [JSON yanıt](overview.md#the-response).

Quickstarts ilk isteklerinize hızlı bir şekilde başlamak için bkz: [C#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [node.js](quickstarts/nodejs.md)  |  [Python](quickstarts/python.md)





