---
title: Birden çok değerli varlıkların bir konuşma Öğrenici modeli - Microsoft Bilişsel hizmetler ile kullanma | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici modeli ile birden çok değerli varlıkların kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 6193a515f0d8136e0d420b7554cf26fee8f50953
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39173110"
---
# <a name="how-to-use-multi-value-entities-with-a-conversation-learner-model"></a>Konuşma Öğrenici modeli ile birden çok değerli varlıklar kullanma
Bu öğretici, varlıkların "birden çok değerli" özelliğini gösterir.

## <a name="video"></a>Video

[![Öğreticinin 6 Preview](http://aka.ms/cl-tutorial-06-preview)](http://aka.ms/blis-tutorial-06)

##<a name="requirements"></a>Gereksinimler
Bu öğreticide, genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar
"Birden çok değerli" olan bir varlığın tek bir değer depolamak yerine bir liste değerleri toplanır.  Bu kullanıcı bir pizza üzerinde toppings gibi birden fazla değer belirleyebileceğiniz varlıklar için kullanışlıdır.

Varlık "çok value" işaretlenmişse concretely, ardından her varlık örneği botun bellek (yerine tek bir varlık değeri üzerine) listesine eklenir tanınır.

## <a name="steps"></a>Adımlar

### <a name="create-the-model"></a>Modeli oluşturma

1. Web kullanıcı Arabiriminde, yeni bir modele tıklayın
2. MultiValueEntities adı girin. Oluştur'a tıklayın.

### <a name="create-an-entity"></a>Bir varlık oluşturma

1. Varlıklar ve ardından yeni bir varlık tıklayın.
2. Varlık adı, Toppings girin.
3. Birden çok değerli denetim.
    - Birden çok değerli varlıklar varlıktaki bir veya daha fazla değerleri toplar.
2. Denetim değişkeninin tersi.  
    - Bu, toppings kendi birikmiş pizza toppings listesinden kaldırmak için kullanıcıya izin verir.
3. Oluştur’a tıklayın.

![](../media/tutorial6_entities.PNG)

### <a name="create-two-actions"></a>İki eylem oluşturma

1. Eylemler ve ardından yeni bir eylem tıklayın
2. Yanıtta 'hangi toppings istiyorsunuz?' yazın.
3. Eleyerek varlıklarda Toppings girin.
3. Oluştur’a tıklayın

Ardından ikinci bir eylem oluşturun.

1. Eylemler ve ardından yeni bir eylem ikinci bir eylem oluşturmak için tıklayın.
3. Yanıt olarak, türü ', toppings şunlardır: $Toppings'.
4. Oluştur’a tıklayın

Artık iki eylem var.

![](../media/tutorial6_actions.PNG)

### <a name="train-the-bot"></a>Botunuzu

1. Train iletişim kutuları, ardından yeni Train iletişim tıklayın.
2. 'Hello' yazın.
3. Puan Eylemler ve 'hangi toppings istiyorsunuz?' seçin
2. 'Mushrooms ve peynirlerine ayırıyor' girin. 
    - Sıfır, bir veya daha fazla varlık birden etiketleyebilirsiniz.
3. 'Mushrooms' tıklayın ve Toppings seçin.
4. 'Peynirlerine ayırıyor' tıklayın ve Toppings seçin.
5. Puan Eylemler
    - İki değer Toppings varlık içinde artık yok. 
6. Seç ', toppings şunlardır: $Toppings'.

Daha fazla için ekleyebiliriz:

7. 'Add ekmeye' girin.
    - Varlık algılama altında 'ekmeye' tıklayın ve Toppings seçin.
3. Puan eylemleri tıklayın.
    - 'ekmeye' şimdi Toppings ek bir değer olarak gösterilir.
6. Seç ', toppings şunlardır: $Toppings'.

Şimdi bir belirtti kaldırın ve bir tane ekleyin:

2. 'Ekmeye kaldırın ve sausage Ekle' yazın.
1. 'Ekmeye' ve kırmızı x kaldırmak için tıklayın.
2. 'Ekmeye' üzerinde'ı tıklatın ve seçin '-Toppings'.
3. Puan eylemleri tıklayın.
    - Silinmiş olan 'ekmeye' ve 'sausage' eklendi.
6. Seç ', toppings şunlardır: $Toppings'.

Artık her şeyi kaldırma deneyelim:

6. 'Mushrooms kaldırmak, peynirlerine ayırıyor kaldırın ve sausage Kaldır' girin.
7. Her üç tıklayın ve '-Toppings'.
7. Puan eylemleri tıklayın.
    - Tüm toppings temizlenir.
2. 'Hangi toppings istiyorsunuz?' seçin
3. Öğretim bitti seçeneğine tıklayın

![](../media/tutorial6_dialogs.PNG)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yerleşik varlıklar](./7-built-in-entities.md)
