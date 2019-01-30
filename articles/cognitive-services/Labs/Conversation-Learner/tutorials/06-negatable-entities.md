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
ms.openlocfilehash: fea950e2c13d9b5ce0c3619990961e611edd6626
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55207386"
---
# <a name="how-to-use-negatable-entities-with-a-conversation-learner-model"></a>Konuşma Öğrenici modeliyle değişkeni yoksayılamaz varlıklar kullanma

Bu öğreticide "Negatable" özelliğini varlıkları gösterir.

## <a name="video"></a>Video

[![Değişkeni yoksayılamaz varlıkları öğretici Önizleme](https://aka.ms/cl_Tutorial_v3_NegatableEntities_Preview)](https://aka.ms/cl_Tutorial_v3_NegatableEntities)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar
Bir varlığın "Negatable" özelliği, hem normal (pozitif) etiket sağlar ve negatif varlık örneklerini, pozitif ve negatif modellerde tabanlı öğretmek ve var olan bir varlığa değerini Temizle. Kendi "Negatable" özellik kümesine sahip varlıklar konuşma daha yalın değişkeni yoksayılamaz varlık adı verilir.

## <a name="steps"></a>Adımlar

### <a name="create-the-model"></a>Model oluşturma

1. Web kullanıcı Arabiriminde "Yeni modeli."'a tıklayın.
2. "NegatableEntity" "Name" alanına yazın ve ENTER tuşuna basın.
3. "Oluştur" düğmesine tıklayın.

### <a name="entity-creation"></a>Varlık oluşturma

1. Sol panelde, "Varlık" sonra "Yeni varlık" düğmesine tıklayın.
2. "Özel" seçin "Varlık türü."
3. "Varlık adı." "name" yazın
4. "Negatable" onay kutusunu işaretleyin.
    - Bu özelliğin denetimi, bir varlık değeri sağlamak kullanıcı etkinleştirir veya örneğin bir şeydir *değil* varlık değeri. İkinci durumda, eşleşen varlık değeri silinmesini sonucudur.
5. "Oluştur" düğmesine tıklayın.

![](../media/tutorial5_entities.PNG)

### <a name="create-the-first-action"></a>İlk Eylem oluştur

1. Sol panelde, "Eylemler" sonra "Yeni Eylem" düğmesine tıklayın.
2. "Botun yanıt..." alanı, "Adınızı bilmiyorum." yazın
3. "Name" "Eleyerek sağlar" alanına yazın
4. "Oluştur" düğmesine tıklayın.

### <a name="create-the-second-action"></a>İkinci Eylem oluştur

1. Sol panelde, "Eylemler" sonra "Yeni Eylem" düğmesine tıklayın.
2. "Botun yanıt..." alanı, "adınızı biliyorum. yazın Buna $name var."
3. "Oluştur" düğmesine tıklayın.

> [!NOTE]
> "name" varlık, bir "gerekli varlıkları" başvuruya göre yanıt olarak otomatik olarak eklendi.

Artık iki eylem var.

![](../media/tutorial5_actions.PNG)

### <a name="train-the-model"></a>Modeli eğitme

1. Sol panelde, "İletişim kutuları eğitme" ve ardından "Yeni Train iletişim kutusu" düğmesine tıklayın.
2. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "hello" yazın
3. "Puan Eylemler" düğmesine tıklayın.
4. Yanıtı seçin, "Adınızı bilmiyorum."
    - Kısıtlamalar yalnızca geçerli işlem tabanlı olarak yüzdelik dilim % 100 ' dir.
5. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "Ferdi adımın olduğu" türü
6. "Frank','ı seçin ve etiket seçin" + ad "
    - "Name" varlığı için iki durum vardır: "+ name" ve "-name".  (+) Artı ekler veya değerin üzerine yazar. (-) Eksi değer kaldırır.
7. "Puan Eylemler" düğmesine tıklayın.
    - "name" varlık "modelin bellek, bu nedenle"biliyorum adınızı. Ferdi gibi"şimdi tanımlanır $Name olduğu"eylem kullanılabilir.
8. Yanıtı seçin, "adınızı biliyorum. Buna $name var."
9. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "adım Frank değil." türü
10. "Ferdi" seçin ve etiket "-name"
    - Seçiyoruz "-name" varlığın geçerli değerini temizleyin.
11. "Puan Eylemler" düğmesine tıklayın.
12. Yanıtı seçin, "Adınızı bilmiyorum."
13. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "adım Susan sağlıyor." türü
14. "Susan','ı seçin ve etiket seçin" + ad "

![](../media/tutorial5_dialogs.PNG)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Birden çok değerli varlıklar](./07-multi-value-entities.md)
