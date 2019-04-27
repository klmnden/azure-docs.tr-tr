---
title: Konuşma Öğrenici sahip bir Model - Microsoft Bilişsel hizmetler değişkeni yoksayılamaz varlıklar kullanma | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici modeliyle değişkeni yoksayılamaz varlıkları kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 78dc759632c4fc3116a59ea1e5bc0b93200bca45
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60708030"
---
# <a name="how-to-use-negatable-entities-with-a-conversation-learner-model"></a>Konuşma Öğrenici modeliyle değişkeni yoksayılamaz varlıklar kullanma

Bu öğreticide "Negatable" özelliğini varlıkları gösterir.

## <a name="video"></a>Video

[![Değişkeni yoksayılamaz varlıkları öğretici Önizleme](https://aka.ms/cl_Tutorial_v3_NegatableEntities_Preview)](https://aka.ms/cl_Tutorial_v3_NegatableEntities)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, genel öğretici Bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar
Bir varlığın "Negatable" özelliği, hem normal (pozitif) etiket sağlar ve negatif varlık örneklerini, pozitif ve negatif modellerde tabanlı öğretmek ve var olan bir varlığa değerini Temizle. Kendi "Negatable" özellik kümesine sahip varlıklar konuşma daha yalın değişkeni yoksayılamaz varlık adı verilir.

## <a name="steps"></a>Adımlar

Giriş sayfasında Web kullanıcı arabiriminde başlatın.

### <a name="create-the-model"></a>Model oluşturma

1. Seçin **yeni modeli**.
2. Girin **NegatableEntity** için **adı**.
3. **Oluştur**’u seçin.

### <a name="entity-creation"></a>Varlık oluşturma

1. Seçin **varlıkları** sol bölmesinde, ardından **yeni varlık**.
2. Seçin **eğitilmiş özel** için **varlık türü**.
3. Girin **adı** için **varlık adı**.
4. Denetleme **Negatable** kullanıcıların bir varlık değeri belirtin veya bir şey olmadığını söylüyor sağlamak için *değil* varlık değeri böylece eşleşen varlık değeri siliniyor.
5. **Oluştur**’u seçin.

![](../media/T06_entity_create.png)

### <a name="create-the-first-action"></a>İlk Eylem oluştur

1. Seçin **eylemleri** sol bölmesinde, ardından **yeni eylem**.
2. Girin **adınızı bilmiyorum.** için **Botun yanıt...** .
3. Girin **adı** için **eleyerek sağlar**.
4. **Oluştur**’u seçin.

![](../media/T06_action_create_1.png)

### <a name="create-the-second-action"></a>İkinci Eylem oluştur

1. Seçin **eylemleri** sol bölmesinde, ardından **yeni eylem**.
2. Girin **adınızı biliyorum. Bu, $name olur.** için **Botun yanıt...** .
3. **Oluştur**’u seçin.

> [!NOTE]
> **Adı** varlık olarak otomatik olarak eklenen bir **gerekli varlıkları** yanıt utterance başvuruya göre.

Artık iki eylem var.

![](../media/T06_action_create_2.png)

### <a name="train-the-model"></a>Modeli eğitme

1. Seçin **eğitme iletişim kutuları** sol bölmesinde, ardından **yeni Train iletişim**.
2. Girin **hello** kullanıcının utterance sol sohbet panelinde için.
3. Seçin **puan Eylemler**.
4. Seçin **adınızı bilmiyorum.** Eylem listesinden. % 100 olarak temel kısıtlamalar yalnızca geçerli eylem yüzdebirliktir.
5. Girin **adımın Frank olduğu** kullanıcının utterance sol sohbet panelinde için.
6. Vurgulama **Frank** seçip **+ ad**. Değişkeni yoksayılamaz varlıkların iki örneği: (+) artı ekler veya değer;'üzerine yazar (-) eksi değer kaldırır.
7. Seçin **puan Eylemler**. **Adı** varlık artık olarak tanımlanmıştır **Frank** modelin bellekte, böylece **adınızı biliyorum. $Name olan** eylemi, kullanılabilir.
8. Seçin **adınızı biliyorum. Bu, $name olur.** Eylem listesinden.
9. Girin **adımın Frank değil.** Sol sohbet panelinde kullanıcının utterance için.
10. Vurgulama **Frank** seçip **-adı** değerini temizlemek için **adı** varlık.
11. Seçin **puan Eylemler**.
12. Seçin **adınızı bilmiyorum.** Eylem listesinden.
13. Girin **Susan My adıdır.** kullanıcının üçüncü utterance için sol sohbet panelinde.
14. Vurgulama **Susan** ardından **+ adı** 

![](../media/T06_training.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Birden çok değerli varlıklar](./07-multi-value-entities.md)
