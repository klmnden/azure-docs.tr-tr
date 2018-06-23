---
title: Birden çok değerli varlıklar bir konuşma öğrenen uygulaması - Microsoft Bilişsel hizmetler ile kullanma | Microsoft Docs
titleSuffix: Azure
description: Birden çok değerli varlıklar bir konuşma öğrenen uygulaması ile kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 865b50747b2c9574b5f88d4902bea9e4c8e0e032
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354010"
---
# <a name="how-to-use-multi-value-entities-with-a-conversation-learner-application"></a>Birden çok değerli varlıklar bir konuşma öğrenen uygulaması ile kullanma
Bu öğretici varlıkların "birden çok değer" özelliğini gösterir.

##<a name="requirements"></a>Gereksinimler
Bu öğretici genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar
"Çoklu değer" olan bir varlığın tek bir değer depolamak yerine bir liste değerleri birikir.  Bu kullanıcı bir pizza üzerinde toppings gibi birden fazla değer belirtebileceğiniz varlıklar için kullanışlıdır.

Bir varlık "birden çok değer" olarak işaretlenmişse, concretely, ardından her varlığın örneği bot'ın bellek (yerine tek bir varlık değeri üzerine) bir listesine eklenir tanınmıyor.

## <a name="steps"></a>Adımlar

### <a name="create-the-application"></a>Uygulama oluşturma

1. Yeni uygulama Web kullanıcı Arabiriminde tıklatın
2. MultiValueEntities adı girin. Ardından, Oluştur'u tıklatın.

### <a name="create-an-entity"></a>Bir varlık oluşturun

1. Varlıklar, yeni varlık tıklatın.
2. Varlık adı Toppings girin.
3. Birden çok değerli denetleyin.
    - Birden çok değerli varlıklar varlıktaki bir veya daha fazla değerleri toplar.
2. Onay değişkeni yoksayılamaz.  
    - Bu toppings kendi birikmiş pizza toppings listesinden kaldırmak için kullanıcı izin verir.
3. Oluştur’a tıklayın.

![](../media/tutorial6_entities.PNG)

### <a name="create-two-actions"></a>İki eylem oluşturma

1. Eylemler tıklatın, yeni eylem
2. Yanıtta 'ne toppings, istiyor musunuz?' yazın.
3. Adayını varlıklarda Toppings girin.
3. Oluştur’a tıklayın

Daha sonra ikinci bir eylem oluşturun.

1. Eylemler, sonra yeni eylem ikinci bir eylem oluşturmak için tıklayın.
3. Yanıtta, türü ', toppings şunlardır: $Toppings'.
4. Oluştur’a tıklayın

Şimdi iki eylemlere sahiptir.

![](../media/tutorial6_actions.PNG)

### <a name="train-the-bot"></a>Bot eğitme

1. Tren iletişim kutuları, ardından yeni tren iletişim'ı tıklatın.
2. 'Hello' yazın.
3. Puan Eylemler'i tıklatın ve 'ne toppings, istiyor musunuz?' seçin
2. 'Mushrooms ve Peynir' girin. 
    - Sıfır, bir veya birden fazla varlık etiketleyebilirsiniz.
3. 'Mushrooms' tıklayın ve Toppings seçin.
4. 'Peynir' tıklayın ve Toppings seçin.
5. Puan Eylemler
    - İki değer Toppings varlıkta mevcut olduğunu unutmayın. 
6. Seç ', toppings şunlardır: $Toppings'.

Daha fazla için ekleyebiliriz:

7. 'Peppers Ekle' girin.
    - Varlık algılama altında 'peppers' tıklayın ve Toppings seçin.
3. Puan Eylemler'i tıklatın.
    - Şimdi peppers Not Toppings ek bir değer olarak görüntülenir.
6. Seç ', toppings şunlardır: $Toppings'.

Şimdi bir belirtti kaldırın ve bir tane ekleyin:

2. 'Peppers kaldırın ve sausage Ekle' yazın.
1. 'Peppers üzerinde' tıklayın ve onu kaldırmak için kırmızı x simgesini tıklatın.
2. 'Peppers üzerinde' tıklayın ve '-Toppings'.
3. Puan Eylemler'i tıklatın.
    - 'Peppers' silindi ve 'sausage' eklenip eklenmediğini unutmayın.
6. Seç ', toppings şunlardır: $Toppings'.

Artık her şey kaldırma deneyelim:

6. 'Mushrooms kaldırmak, peynir kaldırın ve sausage Kaldır' girin.
7. Her üç tıklayıp '-Toppings'.
7. Puan Eylemler'i tıklatın.
    - Tüm toppings temizlenir unutmayın.
2. 'Ne toppings, istiyor musunuz?' seçin
3. Öğretme Bitti'yi tıklatın

![](../media/tutorial6_dialogs.PNG)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yerleşik varlıklar](./7-built-in-entities.md)
