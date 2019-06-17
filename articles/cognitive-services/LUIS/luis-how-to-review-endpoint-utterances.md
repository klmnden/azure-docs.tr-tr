---
title: Kullanıcı konuşma gözden geçirin
titleSuffix: Language Understanding - Azure Cognitive Services
description: Etkin öğrenme, uç nokta sorguları yakalar ve emin olan kullanıcının uç nokta konuşma seçer. Bu konuşma hedefini seçin ve bu okuma dünya konuşma için varlıklar işaretlemek için gözden geçirin. Bu değişiklikleri, örnek konuşma kabul, sonra eğitin ve yayımlayın. LUIS ardından konuşma daha doğru bir şekilde tanımlar.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/25/2019
ms.author: diberry
ms.openlocfilehash: 8fac360682ef11c438cdec333fac21d6f8cfc117
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60195169"
---
# <a name="how-to-review-endpoint-utterances-in-luis-portal-for-active-learning"></a>Etkin öğrenme LUIS Portalı'nda konuşma uç noktası İnceleme

[Etkin öğrenme](luis-concept-review-endpoint-utterances.md) uç nokta sorguları yakalar ve emin olan kullanıcının uç nokta konuşma seçer. Bu konuşma hedefini seçin ve bu okuma dünya konuşma için varlıklar işaretlemek için gözden geçirin. Bu değişiklikleri, örnek konuşma kabul, sonra eğitin ve yayımlayın. LUIS ardından konuşma daha doğru bir şekilde tanımlar.


## <a name="enable-active-learning"></a>Etkin öğrenme etkinleştir

Etkin öğrenme etkinleştirmek için kullanıcı sorgularına oturum açın. Bu ayarı gerçekleştirilir [uç nokta sorgu](luis-get-started-create-app.md#query-the-endpoint-with-a-different-utterance) ile `log=true` querystring parametresi ve değeri.

## <a name="disable-active-learning"></a>Etkin öğrenme devre dışı bırak

Etkin öğrenme devre dışı bırakmak için kullanıcı sorgularına oturum yok. Bu ayarı gerçekleştirilir [uç nokta sorgu](luis-get-started-create-app.md#query-the-endpoint-with-a-different-utterance) ile `log=false` querystring parametresi ve değeri.

## <a name="filter-utterances"></a>Konuşma Filtrele

1. Uygulamanız (örneğin, TravelAgent) adını seçerek açın **uygulamalarım** sayfasında ve ardından **derleme** üst çubuktaki.

1. Altında **uygulama performansını**seçin **gözden geçirin, konuşma uç noktası**.

1. Üzerinde **gözden geçirin, konuşma uç noktası** sayfasında, seçme içinde **filtrenin amacını veya varlık tarafından** metin kutusu. Bu açılan liste altındaki tüm hedefleri içerir **HEDEFLERİ** ve altındaki tüm varlıkları **varlıkları**.

    ![Konuşma Filtrele](./media/label-suggested-utterances/filter.png)

1. Bir kategori (hedefleri veya varlıklar) aşağı açılan listeden seçin ve konuşma gözden geçirin.

    ![Hedefi konuşma](./media/label-suggested-utterances/intent-utterances.png)

## <a name="label-entities"></a>Varlık etiketi
LUIS varlık adları mavi renkle vurgulandığı varlık belirteçleri (kelimeler) değiştirir. Bir utterance varlıkları etiketlenmemiş değilse, bunları utterance etiketleyin. 

1. Utterance içinde sözcükleri'ı seçin. 

1. Bir varlık listeden seçin.

    ![Varlık etiketi](./media/label-suggested-utterances/label-entity.png)

## <a name="align-single-utterance"></a>Tek utterance Hizala

Her utterance görüntülenen önerilen bir amacı güdülür; **hedefi hizalı** sütun. 

1. İle bu öneriyi kabul ederseniz üzerinde onay işaretini seçin.

    ![Hizalanmış hedefi tut](./media/label-suggested-utterances/align-intent-check.png)

1. Öneri katılmıyorum doğru hedefi hizalı hedefi aşağı açılan listeden seçin, sonra hizalanmış amaç sağındaki onay işaretini seçin. 

    ![Hedefi Hizala](./media/label-suggested-utterances/align-intent.png)

1. Utterance üzerinde onay işareti seçtikten sonra listeden kaldırıldı. 

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
