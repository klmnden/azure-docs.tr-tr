---
title: Bir konuşma Öğrenici modeline - Microsoft Bilişsel hizmetler Pre-Trained varlıklar ekleme | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici modele Pre-trained varlıklar ekleme konusunda bilgi edinin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: c198dfc19a350188f500af86c531be9a9ac424ce
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2018
ms.locfileid: "53796842"
---
# <a name="how-to-add-pre-trained-entities"></a>Pre-trained varlıklar ekleme
Bu öğreticide, konuşma Öğrenici modelinizi Pre-Trained varlık eklemek gösterilir.

## <a name="video"></a>Video

[![Önceden eğitilmiş varlıkları öğretici Önizleme](https://aka.ms/cl_Tutorial_v3_PreTrainedEntities_Preview)](https://aka.ms/cl_Tutorial_v3_PreTrainedEntities)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar

Önceden eğitilmiş varlıklar, sayılar, tarihler, parasal tutarların ve diğerleri gibi varlıklar, genel türleri tanır.  Çalıştıkları "out-of--box," tüm eğitim gerektirmez ve bunların davranışlarını özel varlıklar farklı olarak değiştirilemez.  Varsayılan olarak, Pre-Trained tanımlanan her varlık örneği biriktirme birden çok değerli varlıklardır.

## <a name="steps"></a>Adımlar

### <a name="create-the-model"></a>Model oluşturma

1. Web kullanıcı Arabiriminde "Yeni modeli."'a tıklayın.
2. "PretrainedEntities" "Name" alanına yazın ve ENTER tuşuna basın.
3. "Oluştur" düğmesine tıklayın.

### <a name="entity-creation"></a>Varlık oluşturma

1. Sol panelde, "Varlık" sonra "Yeni varlık" düğmesine tıklayın.
2. "Öncesi-Trained/datetimeV2" seçin "Varlık türü."
3. "Birden çok değerli" onay kutusunu işaretleyin.
    - Birden çok değerli varlıklar varlıktaki bir veya daha fazla değerleri toplar.
    - Değişkeni yoksayılamaz özellikleri Pre-Trained varlıklar için devre dışı bırakıldı.
4. "Oluştur" düğmesine tıklayın.

![](../media/tutorial7_entities_a.PNG)

### <a name="create-the-first-action"></a>İlk Eylem oluştur

1. Sol panelde, "Eylemler" sonra "Yeni Eylem" düğmesine tıklayın.
2. "Botun yanıtta..." alanına "$builtin tarihtir-datetimev2"
3. "Oluştur" düğmesine tıklayın.

![](../media/tutorial7_actions_a.PNG)

### <a name="create-the-second-action"></a>İkinci Eylem oluştur

1. Sol panelde, "Eylemler" sonra "Yeni Eylem" düğmesine tıklayın.
2. "Botun yanıt..." alanı, "tarih nedir?" yazın
    - Varsayılan olarak tüm kullanıcı konuşma için algılandıkça önceden eğitilmiş varlıkları gerekli varlıkları olamaz.
3. "Yerleşik-datetimev2." "Eleyerek sağlar" alanına yazın
4. "Oluştur" düğmesine tıklayın.

![](../media/tutorial7_actions2_a.PNG)

### <a name="train-the-model"></a>Modeli eğitme

1. Sol panelde, "İletişim kutuları eğitme" ve ardından "Yeni Train iletişim kutusu" düğmesine tıklayın.
2. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "hello" yazın
3. "Puan Eylemler" düğmesine tıklayın.
4. "Tarih nedir?" yanıt seçin
5. Burada, "Tür iletinizi..." türünde "Bugün" diyor sohbet panelinde
    - Bugün utterance LUIS önceden eğitilmiş modeller tarafından otomatik olarak kabul edilir.
    - Değer Pre-Trained varlıkların üzerinde bekleyerek LUIS tarafından sağlanan ek verileri gösterir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Varlık çözümleyiciler](./09-entity-resolvers.md)
