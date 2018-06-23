---
title: GÜVENİRLİK Score - Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: GÜVENİRLİK puan açıklayan
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: c97bdb7e57275ebd1893bc28248c4ecc6b35c153
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354059"
---
# <a name="confidence-score"></a>Güvenilirlik Puanı

GÜVENİRLİK puanı kullanıcı soru ve döndürülen yanıt arasında eşleşme derecesi gösterir.

Kullanıcı sorgusu Bilgi Bankası içeriğe karşı eşleştiğinde döndürülen birden fazla yanıt olabilir. Yanıtları güvenirlik puan azalan dereceli bir sırada döndürülür.

0 ile 100 arasında bir güven puan olduğu.

|Puan değeri|Güven|
|--|--|
|100|Kullanıcı sorgusu KB soru ve tam bir eşleşme|
|90|Sözcüklerin en yüksek güvenilirlik - eşleşmesi|
|40-60|Orta güven - önemli sözcükleri eşleştirmeyi|
|10|Düşük güvenilirlik - önemli sözcükler eşleşmiyor|
|0|Word eşleşmesi yok|


## <a name="similar-confidence-scores"></a>Benzer güvenirlik puanları
Birden çok yanıta benzer bir güvenilirlik puan varsa, sorgu çok genel ve bu nedenle eşleşen birden çok yanıtlarla eşit olasılığını ile olasıdır. Her QnA varlık ayrı bir amacı vardır, QnAs daha iyi yapılandırılır deneyin.


## <a name="improving-confidence-scores"></a>GÜVENİRLİK puanları artırma
Kullanıcı sorgusu için belirli bir yanıt güvenirlik puanı artırmak için bu yanıt alternatif bir soru olarak Bilgi Bankası'na kullanıcı sorgu ekleyebilirsiniz.
   
## <a name="confidence-score-differences"></a>GÜVENİRLİK puan farklar
İçeriği aynı olsa bile, bir yanıt güvenirlik puanı negligibly test ve Bilgi Bankası yayımlanmış sürümü arasında değişebilir. Test ve yayımlanan Bilgi Bankası içeriğini bulunur farklı Azure Search dizinlerini olmasıdır.

Burada gördüğünüz nasıl [yayımlama](../How-To/publish-knowledge-base.md) işlemi çalışır.


## <a name="no-match-found"></a>Eşleşme bulunamadı
Tarafından ranker iyi eşleşme bulunduğunda, güvenirlik puanı 0,0 veya "Hiçbiri" döndürülür ve varsayılan yanıt "iyi eşleşme KB bulunamadı". Bu varsayılan yanıt uç noktası çağrılmadan bot veya uygulama kodundaki geçersiz kılabilirsiniz. Alternatif olarak, Azure içinde geçersiz kılma yanıt de ayarlayabilirsiniz ve bu belirli bir QnA Maker hizmet dağıtılan tüm Bilgi Bankası için varsayılan değiştirir.

1. Git [Azure portal](http://portal.azure.com) ve oluşturduğunuz QnA Maker hizmetini temsil eder kaynak grubuna gidin.

2. Açmak için tıklatın **uygulama hizmeti**.

    ![Uygulama hizmeti erişim](../media/qnamaker-concepts-confidencescore/set-default-response.png)

3. Tıklayın **uygulama ayarları** ve düzenleme **DefaultAnswer** istenen varsayılan yanıt alanı. **Kaydet**’e tıklayın.

    ![Varsayılan yanıt değiştirme](../media/qnamaker-concepts-confidencescore/change-response.png)

4. Uygulama hizmeti yeniden başlatın

    ![QnA Maker appservice yeniden başlatma](../media/qnamaker-faq/qnamaker-appservice-restart.png)


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Desteklenen veri kaynakları](./data-sources-supported.md)

## <a name="see-also"></a>Ayrıca bkz. 

[QnA Maker genel bakış](../Overview/overview.md)
