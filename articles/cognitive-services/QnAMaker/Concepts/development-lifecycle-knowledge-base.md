---
title: Bilgi Bankası - soru-cevap Oluşturucu yaşam döngüsü
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu, en iyi modeli değişiklikleri, utterance örnekler, yayımlama ve veri toplamayı yinelemeli bir döngüyle uç nokta sorgularından öğrenir.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 04/05/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 4acecb9d15f820ba092f36d8fa3ea204658d2dba
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59276788"
---
# <a name="knowledge-base-lifecycle-in-qna-maker"></a>Soru-cevap Oluşturucu, Bilgi Bankası yaşam döngüsü
Soru-cevap Oluşturucu, en iyi modeli değişiklikleri, utterance örnekler, yayımlama ve veri toplamayı yinelemeli bir döngüyle uç nokta sorgularından öğrenir. 

![Yazma döngüsü](../media/qnamaker-concepts-lifecycle/kb-lifecycle.png)

## <a name="creating-a-qna-maker-knowledge-base"></a>Soru-cevap Oluşturucu Bilgi Bankası oluşturma
Soru-cevap Oluşturucu Bilgi Bankası (KB) uç noktası, bir kullanıcı sorgu KB içeriğine göre en iyi eşleşme Yanıtla sağlar. Bilgi Bankası oluşturma, bir içerik deposu soru, yanıt ve ilişkili meta verileri ayarlama için tek seferlik bir işlemdir. Bilgi Bankası, SSS sayfaları, ürün kılavuzlarını veya yapılandırılmış Q A çiftleri gibi önceden varolan içerikte tarafından oluşturulabilir. Bilgi edinmek için nasıl [Bilgi Bankası oluşturma](../How-To/create-knowledge-base.md).

## <a name="testing-and-updating-the-knowledge-base"></a>Test ve Bilgi Bankası güncelleştirme

Bilgi Bankası bilgi bankanızı düzenleyerek veya otomatik ayıklama aracılığıyla içerikle doldurulur sonra test etmek için hazırdır. Soru-cevap Oluşturucu portalı üzerinden içinde etkileşimli testi yapılabilir **Test** paneli ortak kullanıcı sorguları girerek ve yanıtları doğru yanıtı ve yeterli güvenilirlik puanı ile döndürülen doğrulanıyor. 

* **Düşük güven puanları düzeltmek için**: diğer sorular ekleyin. 
* **Bir sorgu yanlış döndürdüğünde [varsayılan yanıt](confidence-score.md#change-default-answer)**: doğru soruyu yeni yanıtları ekleyin. 

Sonuçlardan memnun olana kadar test güncelleştirme sıkı Bu döngü devam eder. Bilgi edinmek için nasıl [bilgi bankanızı test](../How-To/test-knowledge-base.md).

Büyük KB'leri için kullanım otomatik testi [generateAnswer API](../how-to/metadata-generateanswer-usage.md#get-answer-predictions-with-the-generateanswer-api) ve `isTest=true` sorgu dizesi parametresi hangi sorguların `test` Bankası yayımlanan Bilgi Bankası yerine. 

## <a name="publish-the-knowledge-base"></a>Bilgi bankasını yayımlama
İşiniz bittiğinde Bilgi Bankası sınanması, bunu yayımlayabilirsiniz. En son sürümünü test edilmiş Bilgi Bankası'na ayrılmış bir Azure Search dizinini temsil eden bildirim yayımlama **yayımlanan** Bilgi Bankası. Ayrıca uygulamanızda veya sohbet botunuzda çağrılabilecek bir uç nokta da oluşturulur.

Bu şekilde, bir üretim uygulamasında Canlı olabilecek yayımlanmış sürümüne Bilgi Bankası test sürümüne yapılan her değişikliği etkilemez.

Bu bilgi bankalarından herbiri ayrı olarak test etmek için hedeflenebilir. API'leri kullanarak Bilgi Bankası ile test sürümünü hedefleyebilirsiniz `isTest=true` generateAnswer çağrısında bayrağı.

Bilgi edinmek için nasıl [, Bilgi Bankası yayımlama](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base).

## <a name="monitor-usage"></a>Kullanımı izleme
Sohbet günlükleri hizmetinizin oturum açabilmesi için Application Insights'ı etkinleştirmek ihtiyacınız zaman, [soru-cevap Oluşturucu hizmetinizi oluşturun](../How-To/set-up-qnamaker-service-azure.md).

Hizmet kullanımınızın çeşitli analiz elde edebilirsiniz. Almak için application ınsights'ı kullanma hakkında daha fazla bilgi edinin [soru-cevap Oluşturucu hizmetiniz için analytics](../How-To/get-analytics-knowledge-base.md).

Bağlı olarak, analytics'ten öğrenin, uygun hale [bilgi bankanızı güncelleştirmeleri](../How-To/edit-knowledge-base.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Güvenilirlik puanı](./confidence-score.md)

## <a name="see-also"></a>Ayrıca bkz. 

[Bilgi Bankası](./knowledge-base.md)
[soru-cevap Oluşturucu genel bakış](../Overview/overview.md)
