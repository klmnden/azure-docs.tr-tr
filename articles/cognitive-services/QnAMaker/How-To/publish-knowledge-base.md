---
title: Bilgi bankası yayımlama
titleSuffix: QnA Maker API - Azure Cognitive Services
description: Soru-cevap Oluşturucu API'si hizmeti, Bilgi Bankası'nda yayımlama bilgi bankanızı bir soruyu yanıtlarken uç noktası olarak kullanılabilir hale getirmek için son adımdır. Bilgi Bankası yayımladığınızda, soru-cevap bilgi bankanızı içeriğini taşır test dizini Azure Search'te bir üretim dizini.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 03/11/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: c65654e7d6b2e66a07116ea9555ed316ace88912
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58091962"
---
# <a name="publish-a-knowledge-base-using-the-qna-maker-api-service-portal"></a>Soru-cevap Oluşturucu API'si hizmeti portalını kullanarak Bilgi Bankası yayımlama

Soru-cevap Oluşturucu API'si hizmeti, Bilgi Bankası'nda yayımlama bilgi bankanızı bir soruyu yanıtlarken uç noktası olarak kullanılabilir hale getirmek için son adımdır. 

Bilgi Bankası yayımladığınızda, bilgi bankanızı soru ve yanıt içeriğini taşır test dizini Azure Search'te bir üretim dizini.

![Ürün test dizini yayımlama](../media/qnamaker-how-to-publish-kb/publish-prod-test.png)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="publish-a-knowledge-base"></a>Bilgi bankası yayımlama

1. KB değişiklikleri yaptıktan sonra Seç **Yayımla** üst gezinti çubuğunda. Azure arama için bilgi bankalarından ayrılan sayıda en fazla yayımlayabilirsiniz. 

    ![Bilgi bankası yayımlama](../media/qnamaker-how-to-publish-kb/publish.png)

2. Seçin **Yayımla** yeniden uygulama veya bot kodunuzda kullanılabilen uç noktası ayrıntıları görmek için.

    ![Bilgi Bankası başarıyla yayımlandı](../media/qnamaker-how-to-publish-kb/publish-success.png)
    
## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilgi Bankası ile işiniz bittiğinde soru-cevap Oluşturucu Portalı'nda kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Analytics hakkında bilgi bankanızı Al](./get-analytics-knowledge-base.md)
