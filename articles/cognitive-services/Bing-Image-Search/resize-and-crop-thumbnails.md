---
title: Yeniden boyutlandırma ve kırpma masaüstünün - Bing resim arama API'si
titleSuffix: Azure Cognitive Services
description: Yeniden boyutlandırma ve Bing resim arama API'si alınan yanıtları dahil masaüstünün kırpar.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: F4FFAE91-A003-4F7C-8E60-83A142485E28
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: scottwhi
ms.custom: seodec2018
ms.openlocfilehash: c53b84f31100ab72458ab6fb79b2009450eda15c
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58917456"
---
# <a name="resize-and-crop-thumbnail-images"></a>Yeniden boyutlandırma ve küçük resim görüntüleri kırpma

Bir arama sorgusu işleme sırasında Bing tüm görüntüleri küçük resim bilgileri oluşturan kendi [yanıt](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-get-images#bing-image-search-response-format). Bu bilgiler, tüm görüntü veya döndürülen küçük bir alt kümesi için kullanılabilir. Bir alt görüntülerseniz, kalan resimleri görüntülemek için bir seçenek sağlar.


<!-- Removing image until we can replace it with a sanatized version.
![Expanded view of thumbnail image](../bing-web-search/media/cognitive-services-bing-web-api/bing-web-image-thumbnail-expansion.PNG)
-->

Kullanıcı küçük resme tıklarsa [contentUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-contenturl) alanını kullanarak tam boyutlu görüntüyü kullanıcıya gösterebilirsiniz. Görüntüye bağlam sağlamayı unutmayın.

`shoppingSourcesCount` veya `recipeSourcesCount` sıfırdan büyükse görüntüdeki öğe için alışveriş veya tarif seçeneklerinin mevcut olduğunu belirtmek için küçük resme alışveriş sepeti gibi simgeler ekleyin.

<!-- this is a sanitized version but we're removing all images for now until everything is sanitized.
![Shopping sources badge](./media/cognitive-services-bing-images-api/bing-images-shopping-source.PNG)
-->

Görüntüyü içeren web sayfaları veya görüntüde tanınan kişiler gibi görüntü hakkında içgörüler için [imageInsightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-imageinsightstoken) belirtecini kullanın. Ayrıntılar için bkz. [Görüntü İçgörüleri](image-insights.md).

## <a name="resizing-and-cropping-thumbnails"></a>Yeniden boyutlandırma ve küçük resimleri kırpma

Ayrıca, yeniden boyutlandırabilir ve bir kullanıcının imleç üzerinde geldiğinde gibi küçük resimler, genişletin.
> [!NOTE]
> Büyütmeniz halinde görüntüye bağlam sağlamayı unutmayın. Örneğin [hostPageDisplayUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-hostpagedisplayurl) parametresindeki ana bilgisayarı ayıklayıp görüntünün altına ekleyebilirsiniz.

[!INCLUDE [cognitive-services-bing-resize-crop-thumbnails](../../../includes/cognitive-services-bing-resize-crop-thumbnails.md)]
