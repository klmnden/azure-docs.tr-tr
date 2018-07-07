---
title: Anlamak yinelemeli LUIS uygulaması tasarlama - Azure | Microsoft Docs
description: LUIS uygulamaları en iyi veri ayıklama almak için LUIS eğitmek için tasarım yinelemeleri gerektirir.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/12/2018
ms.author: v-geberr
ms.openlocfilehash: e0467e4c41209c937f548edc0c40c05cae588f4c
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888257"
---
# <a name="authoring-cycle"></a>Yazma döngüsü
LUIS, en iyi modeli değişiklikleri, utterance örnekler, yayımlama ve veri toplamayı yinelemeli bir döngüyle uç nokta sorgularından öğrenir. 

![Yazma döngüsü](./media/luis-concept-app-iteration/iteration.png)

## <a name="building-a-luis-model"></a>LUIS modeline oluşturma
Modelinin amacı, hangi (amaç veya hedefi için) kullanıcıdan isteyen ve sorunun hangi bölümlerinin yanıt belirlemeye yardımcı olmak (varlık) ayrıntıları sağlama out şekil sağlamaktır. 

Model sözcükleri belirlemek için uygulama etki alanına özel olması ve ilgili yanı sıra tipik sözcük sıralama, tümceleri. 

Model amacı, varlıkları içerir. 

## <a name="add-training-examples"></a>Eğitim örnekleri Ekle
LUIS, örnek konuşma amacı de gerekir. Örnekler, sözcük seçimi ve sözcük sırasını utterance yöneliktir hangi hedefini belirlemek için yeterli sayıda çeşitlemesi gerekir. Her örnek utterance varlıklar olarak etiketlenen tüm gerekli veri olmalıdır. 

LUIS için utterance atayarak uygulamanızın etki ilgili olmayan konuşma yok saymak için toplamasını **hiçbiri** hedefi. Herhangi bir sözcük veya gerekmeyen tümcecikleri dışında bir utterance çekilen etiketlenmiş gerekmez. Sözcük ve tümcecikleri yok saymak için hiçbir etiket yok. 
<!--
## Not just yet
Do not add features such as a [phrase list](luis-concept-feature.md) feature in your first cycle. Phrase lists are phrases that would be specific to your app's subject area.  
-->
## <a name="train-and-publish-the-app"></a>Eğitim ve uygulama yayımlama
10-15 farklı konuşma etiketli gerekli varlıklarla her hedefini oluşturduktan sonra LUIS eğitin ve ardından uç noktalarınızı almak için yayımlama. Uygulamanızı oluşturmak ve kullanılabilir, böylece uygulamanızı yayımlamak emin [uç nokta bölgeleri](luis-reference-regions.md) ihtiyacınız. 

## <a name="https-endpoint-testing"></a>HTTPS uç noktasını sınama
LUIS uygulamanızı listelenen HTTPS uç noktasından test edebilirsiniz **[Yayımla](luis-how-to-publish-app.md)** sayfası. Uç noktasından test LUIS gözden geçirme için düşük güvenle herhangi bir konuşma seçmenizi sağlar.  

## <a name="recycle"></a>Geri dönüştür
Geliştirme döngüsünü işiniz bittiğinde, yeniden başlayabilirsiniz. Uç nokta konuşma LUIS düşük güvenle işaretlenmiş inceleyerek başlayın. Bu konuşma amacı hem de varlık için denetleyin. Konuşma gözden sonra İnceleme listesi boş olmalıdır.  

## <a name="batch-testing"></a>Toplu işe testi
Toplu test kaç tane örnek konuşma LUIS göre puanlanır görmek için bir yoldur. Örnekler için LUIS yeni olmalıdır ve amacı ve LUIS bulmak için istediğiniz varlıkları ile doğru etiketli. LUIS, konuşma kümesinde ne kadar iyi gerçekleştirecek test sonuçlarını gösterir. 

## <a name="next-steps"></a>Sonraki adımlar

Kavramları hakkında bilgi edinin [işbirliği](luis-concept-collaborator.md).