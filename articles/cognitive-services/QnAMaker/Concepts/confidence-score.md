---
title: Güvenilirlik puanı - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Derecesi eşleşme kullanıcı soru ve döndürülen yanıtı arasında bir güven puanı gösterir.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: 33da5cf5724b8314ce813f12eb077d9a15ec1b2a
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47041532"
---
# <a name="confidence-score"></a>Güvenilirlik Puanı

Derecesi eşleşme kullanıcı soru ve döndürülen yanıtı arasında bir güven puanı gösterir.

Kullanıcı sorgusu karşı Bilgi Bankası içerikleri eşleştiğinde, döndürülen birden fazla yanıt olabilir. Yanıtları azalan güven puanı kişilerinin sıralı bir düzende döndürülür.

0 ile 100 arasında bir güven puanı var.

|Puanı değeri|Güven|
|--|--|
|100|Kullanıcı sorgusu KB soru ve tam bir eşleşme|
|90|Bir kelimelerin en yüksek güvenilirliğe - eşleşmesi|
|40-60|Orta güven - önemli sözcüklerin eşleştir|
|10|Düşük güven - önemli sözcükleri eşleşmiyor|
|0|Word eşleşme yok|


## <a name="similar-confidence-scores"></a>Benzer güven puanları
Birden çok yanıtı benzer bir güvenilirlik puanı varsa, sorgu çok geneldir ve bu nedenle, birden çok yanıtlarla eşit olasılığını ile eşleşen olasıdır. Soru-cevap her varlık, farklı bir hedefi sahip olacak şekilde, Bankalarıyla daha iyi yapısı deneyin.


## <a name="improving-confidence-scores"></a>Güven puanları geliştirme
Belirli bir kullanıcı sorgu yanıt güvenilirlik puanı geliştirmek için alternatif bir soru, yanıt olarak Bilgi Bankası'na kullanıcı sorgusu ekleyebilirsiniz.
   
## <a name="confidence-score-differences"></a>Güvenilirlik puanı farkları
İçeriği aynı olsa bile bir yanıt güvenilirlik puanı negligibly test ve Bilgi Bankası yayımlanmış sürümü arasında değişebilir. Test ve yayımlanan Bilgi Bankası içeriğini bulunur farklı Azure Search dizinlerini olmasıdır.

Burada gördüğünüz nasıl [yayımlama](../How-To/publish-knowledge-base.md) işlemi çalışır.


## <a name="no-match-found"></a>Eşleşme bulunamadı
Derecelendiricisini tarafından iyi bir eşleşme bulunduğunda 0.0 ya da "None" güven puanı döndürülür ve "iyi eşleşme KB bulunamadı" varsayılan yanıttır. Bu varsayılan yanıt uç noktasını çağırma bot veya uygulama kodunda geçersiz kılabilirsiniz. Alternatif olarak, geçersiz kılma yanıt Azure'da ayarlayabilirsiniz ve bu belirli bir soru-cevap Oluşturucu hizmeti dağıtılan tüm bilgi bankaları için varsayılan değiştirir.

1. Git [Azure portalında](http://portal.azure.com) ve oluşturduğunuz soru-cevap Oluşturucu hizmetini temsil eder kaynak grubuna gidin.

2. Açmak için tıklayın **App Service**.

    ![Uygulama hizmetine erişim](../media/qnamaker-concepts-confidencescore/set-default-response.png)

3. Tıklayarak **uygulama ayarları** ve düzenleme **DefaultAnswer** istenen varsayılan yanıt alanı. **Kaydet**’e tıklayın.

    ![Varsayılan yanıt değiştirme](../media/qnamaker-concepts-confidencescore/change-response.png)

4. App service'ı yeniden başlatın

    ![Soru-cevap Oluşturucu appservice yeniden başlatma](../media/qnamaker-faq/qnamaker-appservice-restart.png)


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Desteklenen veri kaynakları](./data-sources-supported.md)

## <a name="see-also"></a>Ayrıca bkz. 

[Soru-Cevap Oluşturma’ya genel bakış](../Overview/overview.md)
