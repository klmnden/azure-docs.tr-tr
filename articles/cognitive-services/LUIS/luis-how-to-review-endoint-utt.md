---
title: Kullanıcı konuşma gözden geçirin
titleSuffix: Language Understanding - Azure Cognitive Services
description: LUIS, çığır açan etkin öğrenim kavramının özelliğidir. LUIS, uç nokta sorguları olduğunda, etkin olarak öğrenmeye emin seçer konuşma tarafından sonuçların kalitesini artırır. Bu konuşma etiket, eğitin ve yayımlayın sonra LUIS konuşma daha doğru bir şekilde tanımlar.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 7175463001eac118d2939d3479e61a01b80ba86b
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53106558"
---
# <a name="how-to-review-endpoint-utterances-in-luis-portal"></a>LUIS portalında konuşma uç noktası İnceleme

LUIS, çığır açan özelliğidir [kavramı](luis-concept-review-endpoint-utterances.md) etkin öğrenme. LUIS, uç nokta sorguları sonra LUIS etkin öğrenme sonuçların kalitesini geliştirmek için kullanır. Etkin öğrenme sürecinde LUIS tüm uç nokta sesleri inceler ve emin olan konuşma seçer. Bu konuşma etiket, eğitin ve yayımlayın sonra LUIS konuşma daha doğru bir şekilde tanımlar. 

## <a name="filter-utterances"></a>Konuşma Filtrele
1. Uygulamanız (örneğin, TravelAgent) adını seçerek açın **uygulamalarım** sayfasında ve ardından **derleme** üst çubuktaki.

2. Altında **uygulama performansını**seçin **gözden geçirin, konuşma uç noktası**.

3. Üzerinde **gözden geçirin, konuşma uç noktası** sayfasında, seçme içinde **filtrenin amacını veya varlık tarafından** metin kutusu. Bu açılan liste altındaki tüm hedefleri içerir **HEDEFLERİ** ve altındaki tüm varlıkları **varlıkları**.

    ![Konuşma Filtrele](./media/label-suggested-utterances/filter.png)

4. Bir kategori (hedefleri veya varlıklar) aşağı açılan listeden seçin ve konuşma gözden geçirin.

    ![Hedefi konuşma](./media/label-suggested-utterances/intent-utterances.png)

## <a name="label-entities"></a>Varlık etiketi
LUIS varlık adları mavi renkle vurgulandığı varlık belirteçleri (kelimeler) değiştirir. Bir utterance varlıkları etiketlenmemiş değilse, bunları utterance etiketleyin. 

1. Utterance içinde sözcükleri'ı seçin. 

2. Bir varlık listeden seçin.

    ![Varlık etiketi](./media/label-suggested-utterances/label-entity.png)

## <a name="align-single-utterance"></a>Tek utterance Hizala

Her utterance görüntülenen önerilen bir amacı güdülür; **hedefi hizalı** sütun. 

1. İle bu öneriyi kabul ederseniz üzerinde onay işaretini seçin.

    ![Hizalanmış hedefi tut](./media/label-suggested-utterances/align-intent-check.png)

2. Öneri katılmıyorum doğru hedefi hizalı hedefi aşağı açılan listeden seçin, sonra hizalanmış amaç sağındaki onay işaretini seçin. 

    ![Hedefi Hizala](./media/label-suggested-utterances/align-intent.png)

3. Utterance üzerinde onay işareti seçtikten sonra listeden kaldırıldı. 

## <a name="align-several-utterances"></a>Çeşitli konuşma Hizala

Çeşitli konuşma hizalamak için konuşma solundaki onay kutusunu işaretleyin ve ardından seçin **seçili** düğmesi. 

![Birkaç align](./media/label-suggested-utterances/add-selected.png)

## <a name="verify-aligned-intent"></a>Hizalanmış hedefi doğrulayın
Utterance hizalı doğru hedefle giderek doğrulayabilirsiniz **hedefleri** sayfasında hedefi adı seçin ve konuşma gözden geçirme. Gelen utterance **gözden geçirin, konuşma uç noktası** listesinde.

## <a name="delete-utterance"></a>Utterance Sil
Her utterance gözden geçirme listesinden silinebilir. Silindikten sonra listeden yeniden görünmez. Uç noktasından aynı utterance kullanıcının girdiği olsa bile bu geçerlidir. 

Utterance silmelisiniz konusunda emin değilseniz, ya da hiçbiri hedefi, taşımak veya "diğer" gibi yeni bir hedefi oluşturma ve utterance bu amaç için taşıyın. 

## <a name="delete-several-utterances"></a>Çeşitli konuşma Sil
Çeşitli konuşma silmek için her bir öğe seçin ve sağ tarafındaki çöp kutusu seçin **seçili** düğmesi.

![Birkaç Sil](./media/label-suggested-utterances/delete-several.png)

## <a name="next-steps"></a>Sonraki adımlar

Önerilen konuşma etiket sonra performansı nasıl iyileştirir test etmek için test konsolunda seçerek erişebilirsiniz **Test** üst panelinde. Test Konsolu kullanarak uygulamanızı test etme hakkında yönergeler için bkz: [eğitme ve uygulamanızı test](luis-interactive-test.md).
