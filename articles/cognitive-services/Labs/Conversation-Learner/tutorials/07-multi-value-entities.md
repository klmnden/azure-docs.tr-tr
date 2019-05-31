---
title: Konuşma Öğrenici sahip bir Model - Microsoft Bilişsel hizmetler birden çok değerli varlıklar kullanma | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici modeliyle birden çok değerli varlıkların kullanmayı öğrenin.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ms.openlocfilehash: 1f62def5e498f3f744beaed0cda207e1a75bfdf2
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66387954"
---
# <a name="how-to-use-multi-value-entities-with-a-conversation-learner-model"></a>Konuşma Öğrenici modeli ile birden çok değerli varlıklar kullanma
Bu öğreticide, varlıkların birden çok değerli özellik gösterilir.

## <a name="video"></a>Video

[![Birden çok değerli varlıkların öğretici Önizleme](https://aka.ms/cl_Tutorial_v3_MultiValued_Preview)](https://aka.ms/cl_Tutorial_v3_MultiValued)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, genel öğretici Bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar
Birden çok değerli varlıkları tek bir değer depolamak yerine bir liste değerleri toplar.  Bu varlıklar, kullanıcıların birden fazla değer belirtebilirsiniz olduğunda yararlıdır. Örneğin bir pizza üzerinde toppings.

Varlıklar birden çok değerli Botun bellekte bir listeye eklenen varlığın tanınan her örneği olarak işaretlenmiş. Sonraki tanıma varlığın değeri yerine üzerine ekler.

## <a name="steps"></a>Adımlar

Giriş sayfasında Web kullanıcı arabiriminde başlatın.

### <a name="create-the-model"></a>Model oluşturma

1. Seçin **yeni modeli**.
2. Girin **MultiValueEntities** için **adı**.
3. **Oluştur**’u seçin.

### <a name="entity-creation"></a>Varlık oluşturma

1. Seçin **varlıkları** sol bölmesinde, ardından **yeni varlık**.
2. Seçin **eğitilmiş özel** için **varlık türü**.
3. Girin **toppings** için **varlık adı**.
4. Denetleme **birden çok değerli** varlık etkinleştirmek için bir veya daha fazla değer accumulate.
5. Denetleme **değişkeni yoksayılamaz**.
6. **Oluştur**’u seçin.

![](../media/T07_entity_create.png)

### <a name="create-the-first-action"></a>İlk Eylem oluştur

1. Seçin **eylemleri** sol bölmesinde, ardından **yeni eylem**.
2. Girin **, toppings şunlardır: $toppings** için **Botun yanıt...** . Önde gelen dolar işareti bir varlık başvurusu gösterir.
3. **Oluştur**’u seçin.

![](../media/T07_action_create_1.png)

### <a name="create-the-second-action"></a>İkinci Eylem oluştur

1. Seçin **eylemleri** sol bölmesinde, ardından **yeni eylem**.
2. Girin **hangi toppings ister misiniz?** için **Botun yanıt...** .
3. Girin **toppings** için **eleyerek sağlar**.
4. **Oluştur**’u seçin.

Artık iki eylem var.

![](../media/T07_action_create_2.png)

### <a name="train-the-model"></a>Modeli eğitme

1. Seçin **eğitme iletişim kutuları** sol bölmesinde, ardından **yeni Train iletişim**.
2. Girin **Merhaba** kullanıcının utterance sol sohbet panelinde için.
3. Seçin **puan Eylemler**.
4. Seçin **hangi toppings ister misiniz?** eylemler listesinden. % 100 olarak temel kısıtlamalar yalnızca geçerli eylem yüzdebirliktir.
5. Girin **peynirlerine ayırıyor ve mushrooms** kullanıcının utterance sol sohbet panelinde için.
6. Vurgulama **peynirlerine ayırıyor** seçip **+ toppings**.
7. Vurgulama **mushrooms** seçip **+ toppings**.
8. Seçin **puan Eylemler**.
9. Seçin **, toppings şunlardır: $toppings** eylemler listesinden.
10. Girin **Biber ekleme** kullanıcının sonraki utterance sol sohbet panelinde için.
11. Vurgulama **Biber** seçip **+ toppings**.
12. Seçin **puan Eylemler**.
13. Seçin **, toppings şunlardır: $toppings** eylemler listesinden.
14. Girin **peynirlerine ayırıyor kaldırmak** kullanıcının üçüncü utterance sol sohbet panelinde için.
15. Vurgulama **peynirlerine ayırıyor** seçip **-toppings**.
16. Seçin **puan Eylemler**.
17. Seçin **, toppings şunlardır: $toppings** eylemler listesinden.

![](../media/T07_training.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Önceden eğitilmiş varlıklar](./08-pre-trained-entities.md)
