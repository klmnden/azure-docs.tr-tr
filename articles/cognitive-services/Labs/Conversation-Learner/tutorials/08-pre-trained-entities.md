---
title: Bir konuşma Öğrenici modeline - Microsoft Bilişsel hizmetler Pre-Trained varlıklar ekleme | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici modele Pre-trained varlıklar ekleme konusunda bilgi edinin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: dade442cb04f79ef75e65d6eb29128b105e72b40
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60707758"
---
# <a name="how-to-add-pre-trained-entities"></a>Pre-trained varlıklar ekleme
Bu öğreticide, konuşma Öğrenici modelinizi Pre-Trained varlık eklemek gösterilir.

## <a name="video"></a>Video

[![Önceden eğitilmiş varlıkları öğretici Önizleme](https://aka.ms/cl_Tutorial_v3_PreTrainedEntities_Preview)](https://aka.ms/cl_Tutorial_v3_PreTrainedEntities)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, genel öğretici Bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar

Önceden eğitilmiş varlıklar, sayılar, tarihler, parasal tutarların ve diğerleri gibi varlıklar, genel türleri tanır.  Çalıştıkları "out-of--box," tüm eğitim gerektirmez ve bunların davranışlarını özel varlıklar farklı olarak değiştirilemez.  Varsayılan olarak, Pre-Trained tanımlanan her varlık örneği biriktirme birden çok değerli varlıklardır.

## <a name="steps"></a>Adımlar

Giriş sayfasında Web kullanıcı arabiriminde başlatın.

### <a name="create-the-model"></a>Model oluşturma

1. Seçin **yeni modeli**.
2. Girin **PretrainedEntities** için **adı**.
3. **Oluştur**’u seçin.

### <a name="entity-creation"></a>Varlık oluşturma

1. Seçin **varlıkları** sol bölmesinde, ardından **yeni varlık**.
2. Seçin **öncesi Trained/datetimeV2** için **varlık türü**.
3. Denetleme **birden çok değerli** varlık etkinleştirmek için bir veya daha fazla değer accumulate. Not, Pre-Trained varlıkları değişkeni yoksayılamaz olamaz.
4. **Oluştur**’u seçin.

![](../media/T08_entity_create.png)

1. Seçin **eylemleri** sol bölmesinde, ardından **yeni eylem**.
2. Girin **$builtin tarihtir-datetimev2** için **Botun yanıt...** .
3. **Oluştur**’u seçin.

![](../media/T08_action_create_1.png)

### <a name="create-the-second-action"></a>İkinci Eylem oluştur

1. Seçin **eylemleri** sol bölmesinde, ardından **yeni eylem**.
2. Girin **tarih nedir?** için **Botun yanıt...** . Önceden eğitilmiş varlıkları olamaz **gerekli varlıkları** gibi tüm konuşma için varsayılan olarak tanınır.
3. Girin **yerleşik datetimev2** için **eleyerek varlıkları**.
4. **Oluştur**’u seçin.

![](../media/T08_action_create_2.png)

### <a name="train-the-model"></a>Modeli eğitme

1. Seçin **eğitme iletişim kutuları** sol bölmesinde, ardından **yeni Train iletişim**.
2. Girin **hello** kullanıcının utterance sol sohbet panelinde için.
3. Seçin **puan Eylemler**.
4. Seçin **tarih nedir?** eylemler listesinden
5. Girin **Bugün** kullanıcının utterance sol sohbet panelinde için.
    - **Bugün** utterance LUIS önceden eğitilmiş modeller tarafından tanınan otomatik olarak.
    - Değer Pre-Trained varlıkların üzerinde bekleyerek LUIS tarafından sağlanan ek verileri gösterir.

![](../media/T08_training.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Varlık çözümleyiciler](./09-entity-resolvers.md)
