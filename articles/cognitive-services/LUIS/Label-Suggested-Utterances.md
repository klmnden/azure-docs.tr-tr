---
title: Önerilen utterances HALUK ile etiket | Microsoft Docs
description: Önerilen utterances etiket ve artırma etkin makine öğrenme yardımcı olmak için dil anlama (HALUK) kullanın.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/08/2017
ms.author: v-geberr
ms.openlocfilehash: 91a2fe738743d359677392bfb79aac885702b440
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35356429"
---
# <a name="review-endpoint-utterances"></a>Uç nokta utterances gözden geçirin

HALUK devrim niteliğinde özelliğidir [kavram](luis-concept-review-endpoint-utterances.md) etkin öğrenme. Uç nokta sorguları, HALUK sonra HALUK etkin öğrenme sonuçların kalitesini artırmak için kullanır. Etkin öğrenme işleminde HALUK tüm uç nokta utterances inceler ve emin değilseniz, utterances seçer. Bu utterances etiket, eğitmek ve sonra da HALUK utterances daha doğru bir şekilde tanımlayan yayımlayın. 

## <a name="filter-utterances"></a>Filtre utterances
1. Uygulamanız (örneğin, TravelAgent) üzerinde adını seçerek açın **My uygulamaları** sayfasında sonra seçin **yapı** üst çubuğunda.

2. Altında **uygulama performansı**seçin **gözden geçirin, uç nokta utterances**.

    ![Utterances gözden geçirin](./media/label-suggested-utterances/review.png)

3. Üzerinde **gözden geçirin, uç nokta utterances** sayfasında, içinde **amacını veya varlık göre filtre listesi** metin kutusu. Bu açılır listesi altında tüm hedefleri içerir **HEDEFLERİ** ve altındaki tüm varlıkları **varlıklar**.

    ![Utterances filtresi](./media/label-suggested-utterances/filter.png)

4. Aşağı açılan listesinde (hedefleri veya varlıklar) bir kategori seçin ve utterances gözden geçirin.

    ![Hedefi utterances](./media/label-suggested-utterances/intent-utterances.png)

## <a name="label-entities"></a>Etiket varlıklar
HALUK varlık belirteçleri (sözcük) mavi renkte vurgulanan varlık adları ile değiştirir. Bir utterance varlıklar etiketlenmemiş değilse, bunları utterance etiketleyin. 

1. Utterance sözcükler seçin. 

2. Bir varlık listeden seçin.

    ![Etiket varlık](./media/label-suggested-utterances/label-entity.png)

## <a name="align-single-utterance"></a>Tek utterance hizalayın

Her utterance görüntülenen önerilen bir hedefi olan **hizalı hedefi** sütun. 

1. Bu öneri kabul ediyorsanız, onay işaretini seçin.

    ![Hizalanmış hedefi tutun](./media/label-suggested-utterances/align-intent-check.png)

2. Öneri katılmıyorum hizalanmış hedefi aşağı açılan listeden doğru hedefini seçin, sonra sağa hizalı hedefinin onay işaretini seçin. 

    ![Hedefi hizalayın](./media/label-suggested-utterances/align-intent.png)

3. Onay işareti seçtikten sonra utterance listesinden kaldırılır. 

## <a name="align-several-utterances"></a>Birkaç utterances hizalayın

Birkaç utterances hizalamak için utterances solundaki kutuyu işaretleyin, sonra seçin **seçili Ekle** düğmesi. 

![Birkaç hizalayın](./media/label-suggested-utterances/add-selected.png)

## <a name="verify-aligned-intent"></a>Hizalanmış hedefi doğrulayın
Utterance hizalı doğru hedefle giderek doğrulayabilirsiniz **hedefleri** sayfasında, hedefi adını seçin ve utterances gözden geçirme. Gelen utterance **gözden geçirin, uç nokta utterances** listesinde.

## <a name="delete-utterance"></a>Utterance Sil
Her utterance gözden geçirme listesinden silinebilir. Silindikten sonra listede yeniden görünmez. Uç noktasından aynı utterance kullanıcının girdiği olsa bile bu geçerlidir. 

Utterance silmelisiniz konusunda emin değilseniz, ya da hiçbiri hedefi, taşımak veya "sair" gibi yeni bir hedefi oluşturun ve utterance bu amaç için taşıyın. 

## <a name="delete-several-utterances"></a>Birkaç utterances Sil
Birkaç utterances silmek için her bir öğe seçin ve sağ tarafındaki çöp kutusu seçin **seçili Ekle** düğmesi.

![Birkaç Sil](./media/label-suggested-utterances/delete-several.png)

## <a name="next-steps"></a>Sonraki adımlar

Önerilen utterances etiket sonra nasıl performansı artırır test etmek için test konsol seçerek erişebilirsiniz **Test** üst panelinde. Test konsolunu kullanarak uygulamanızı test etme hakkında daha fazla yönerge için bkz: [eğitimi ve uygulamanızı test](Train-Test.md).
