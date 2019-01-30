---
title: Konuşma Öğrenici sahip bir Model - Microsoft Bilişsel hizmetler birden çok değerli varlıklar kullanma | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici modeliyle birden çok değerli varlıkların kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 8544d63f38f88a0e623dff343bf8b5133931b70b
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228313"
---
# <a name="how-to-use-multi-value-entities-with-a-conversation-learner-model"></a>Konuşma Öğrenici modeli ile birden çok değerli varlıklar kullanma
Bu öğreticide, varlıkların birden çok değerli özellik gösterilir.

## <a name="video"></a>Video

[![Birden çok değer varlıkları öğretici Önizleme](https://aka.ms/cl_Tutorial_v3_MultiValued_Preview)](https://aka.ms/cl_Tutorial_v3_MultiValued)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar
Birden çok değerli varlıkları tek bir değer depolamak yerine bir liste değerleri toplar.  Bu varlıklar, kullanıcıların birden fazla değer belirtebilirsiniz olduğunda yararlıdır. Örneğin bir pizza üzerinde toppings.

Varlıklar birden çok değerli Botun bellekte bir listeye eklenen varlığın tanınan her örneği olarak işaretlenmiş. Sonraki tanıma varlığın değeri yerine üzerine ekler.

## <a name="steps"></a>Adımlar

### <a name="create-the-model"></a>Model oluşturma

1. Web kullanıcı Arabiriminde "Yeni modeli."'a tıklayın.
2. "MultiValueEntities" "Name" alanına yazın ve ENTER tuşuna basın.
3. "Oluştur" düğmesine tıklayın.

### <a name="entity-creation"></a>Varlık oluşturma

1. Sol panelde, "Varlık" sonra "Yeni varlık" düğmesine tıklayın.
2. "Özel olarak eğitilen" seçin "Varlık türü."
3. "Varlık adı." "toppings" yazın
4. "Birden çok değerli" onay kutusunu işaretleyin.
    - Birden çok değerli varlıklar varlıktaki bir veya daha fazla değerleri toplar.
5. "Negatable" onay kutusunu işaretleyin.
    - "Negatable" özelliği, başka bir öğreticinin çalışmasındaki.
6. "Oluştur" düğmesine tıklayın.

![](../media/tutorial6_entities.PNG)

### <a name="create-the-first-action"></a>İlk Eylem oluştur

1. Sol panelde, "Eylemler" sonra "Yeni Eylem" düğmesine tıklayın.
2. "Botun yanıtta..." alanına ", toppings şunlardır: $toppings"
    - Bir varlık başvurusu önde gelen dolar işareti gösterir
3. "Oluştur" düğmesine tıklayın.

### <a name="create-the-second-action"></a>İkinci Eylem oluştur

1. Sol panelde, "Eylemler" sonra "Yeni Eylem" düğmesine tıklayın.
2. "Botun yanıt..." alanı, "hangi toppings ister misiniz?" yazın
3. "Toppings." "Eleyerek sağlar" alanına yazın
4. "Oluştur" düğmesine tıklayın.

Artık iki eylem var.

![](../media/tutorial6_actions.PNG)

### <a name="train-the-model"></a>Modeli eğitme

1. Sol panelde, "İletişim kutuları eğitme" ve ardından "Yeni Train iletişim kutusu" düğmesine tıklayın.
2. Burada yazacaktır "Yazın, iletinizi...", sohbet panelinde türü "hi."
3. "Puan Eylemler" düğmesine tıklayın.
4. "Hangi toppings istiyor musunuz?" yanıt seçin
    - Kısıtlamalar yalnızca geçerli işlem tabanlı olarak yüzdelik dilim % 100 ' dir.
5. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "peynirlerine ayırıyor ve mushrooms" yazın
6. 'Peynirlerine ayırıyor' tıklayın ve etiketi seçin "+ toppings"
7. 'Mushrooms' tıklayın ve etiketi seçin "+ toppings"
8. "Puan Eylemler" düğmesine tıklayın.
9. Yanıtı seçin ", toppings şunlardır: $toppings"
10. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "Biber Ekle" türü
11. 'Biber' tıklayın ve etiketi seçin "+ toppings"
12. "Puan Eylemler" düğmesine tıklayın.
13. Yanıtı seçin ", toppings şunlardır: $toppings"
14. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "peynirlerine ayırıyor Kaldır" yazın
15. "Peynirlerine ayırıyor' tıklayın ve etiketi seçin"-toppings "
16. "Puan Eylemler" düğmesine tıklayın.
17. Yanıtı seçin ", toppings şunlardır: $toppings"

![](../media/tutorial5_dialogs.PNG)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Önceden eğitilmiş varlıklar](./08-pre-trained-entities.md)
