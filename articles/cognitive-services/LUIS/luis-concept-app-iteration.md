---
title: HALUK yinelemeli uygulama anlamak tasarım - Azure | Microsoft Docs
description: En iyi veri ayıklama almak için HALUK eğitmek için tasarım yineleme HALUK uygulamaları gerekir.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/12/2018
ms.author: v-geberr
ms.openlocfilehash: b7f8dd46dc8289322726934f330761b0f1ab94bd
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265946"
---
# <a name="authoring-cycle"></a>Döngüsü yazma
HALUK en iyi modeli değişiklikleri, utterance örnekler, yayımlama ve veri toplamayı yinelemeli bir döngüyle uç nokta sorgularından öğrenir. 

![Döngüsü yazma](./media/luis-concept-app-iteration/iteration.png)

## <a name="building-a-luis-model"></a>HALUK modelini oluşturma
Modelin amacı, hangi kullanıcı (amaç veya hedefi için) istemesi ve sorunun hangi kısımlarının yanıt belirlemeye yardımcı olmak (varlık) ayrıntıları sağlayın çıkışı şekil sağlamaktır. 

Model sözcükler belirlemek için uygulama etki alanına belirli olması ve ilgili yanı sıra tipik word sıralama olan tümceleri. 

Model, amacı, varlıkları içerir. 

## <a name="add-training-examples"></a>Eğitim örnekleri Ekle
HALUK örnek utterances hedefleri de gerekir. Örnekler word seçeneği ve word order utterance için tasarlanmıştır hangi hedefini belirlemek için yeterli değişim gerekir. Her örnek utterance varlıklar etiketli tüm gerekli veri olmalıdır. 

Utterance atayarak uygulamanızın etki alanına ilgili olmayan utterances yoksaymak için HALUK toplamasını **hiçbiri** hedefi. Herhangi bir sözcük veya ihtiyacınız olmayan tümcecikleri dışında bir utterance çekilen etiketli gerekmez. Kelimeler ve ifadeler yoksaymak için hiçbir etiket yok. 
<!--
## Not just yet
Do not add features such as a [phrase list](luis-concept-feature.md) feature in your first cycle. Phrase lists are phrases that would be specific to your app's subject area.  
-->
## <a name="train-and-publish-the-app"></a>Eğitmek ve uygulama yayımlama
Etiketli gerekli varlıklarıyla her amacı, 10-15 farklı utterances sahip olduğunda, HALUK eğitmek ve sonra noktalarınızı almak için yayımlama. Uygulamanızı oluşturun ve böylece kullanılabilir uygulamanızı yayımlamak emin olun [uç nokta bölgeler](luis-reference-regions.md) , gerekir. 

## <a name="https-endpoint-testing"></a>HTTPS uç noktası test etme
Listelenen HTTPS uç noktasından HALUK uygulamanızı test **[Yayımla](publishapp.md)** sayfası. Uç noktasından sınama HALUK herhangi utterances gözden geçirme için düşük güvenle seçmenizi sağlar.  

## <a name="recycle"></a>Geri dönüştür
Geliştirme ile ilgili bir döngü bittiğinde, yeniden başlayabilirsiniz. Uç nokta utterances düşük güvenle HALUK işaretlenmiş inceleyerek başlayın. Bu utterances amacı ve varlık için denetleyin. Utterances gözden sonra gözden geçirme listesi boş olmalıdır.  

## <a name="batch-testing"></a>Toplu test etme
Toplu sınama, kaç tane örnek utterances HALUK göre puanlanır görmek için bir yoldur. Örnekler için HALUK yeni olmalı ve amacı ve varlıkları bulmak için HALUK istediğiniz doğru etiketli. Test sonuçları, HALUK utterances kümesini üzerinde ne kadar iyi gerçekleştireceği gösterir. 

## <a name="next-steps"></a>Sonraki adımlar

Kavramları hakkında bilgi edinin [işbirliği](luis-concept-collaborator.md).

[luis-reference-prebuilt-domains]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-prebuilt-domains