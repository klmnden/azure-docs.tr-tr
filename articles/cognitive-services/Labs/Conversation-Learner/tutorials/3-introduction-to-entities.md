---
title: Varlıklar bir konuşma öğrenen uygulaması - Microsoft Bilişsel hizmetler ile kullanma | Microsoft Docs
titleSuffix: Azure
description: Varlıklar bir konuşma öğrenen uygulaması ile kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 85df31c2e2ff3ca81698921a1f17f415daefb6c5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354004"
---
# <a name="introduction-to-entities"></a>Varlıkları giriş

Bu öğretici varlıklar tanıtır ve "Disqualifying varlıklar" ve "varlıklar gerekli" alanları eylemleri nasıl kullanılacağını gösterir.

## <a name="requirements"></a>Gereksinimler

Bu öğretici genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar

Bu öğretici varlıklar için iki yaygın kullanımları gösterilmektedir.  İlk olarak, varlıklar, alt dizeler "Seattle hava durumu nedir?", bir şehrin tanımlama gibi bir kullanıcı iletiden ayıklayabilirsiniz.  İkinci olarak, Eylemler kullanılabilir olduğunda varlıklar kısıtlayabilirsiniz.  Özellikle, bir eylem bir varlığı "gerektiği" veya "adayını" olarak listeleyebilirsiniz:
- Bir eylem gerekli varlıkları eylemin kullanılabilir olması sırayla bot's bellekte mevcut olması gerekir
- Varlıkları adayını gerekir *değil* bot'ın bellek eylemin kullanılabilir olması sırada bulunması

Diğer öğreticiler önceden derlenmiş varlıklar, birden çok değerli gibi varlıkları ve değişkeni yoksayılamaz varlıkları, programlı varlıkları ve kodda değişiklik yapma varlıklar diğer yönlerini kapsar.

## <a name="steps"></a>Adımlar

### <a name="create-the-application"></a>Uygulama oluşturma

1. Yeni uygulama Web kullanıcı Arabiriminde tıklatın
2. IntroToEntities adı girin. Ardından, Oluştur'u tıklatın.

### <a name="create-entity"></a>Varlık oluştur

1. Varlıklar, yeni varlık tıklatın.
2. Varlık adı Şehir girin.
3. Oluştur’a tıklayın

Not varlık türü 'custom'--Bu varlık eğitilmiş anlamına gelir.  Davranışlarını düzeltilemez--bu başka bir öğreticide kapsanan önceden derlenmiş varlıkları vardır.

### <a name="create-two-actions"></a>İki eylem oluşturma

1. Eylemler tıklatın, yeni eylem
2. Yanıtta 'I istediğiniz hangi Şehir bilmiyorsanız' yazın.
3. Adayını varlıklarda $city girin. Kaydet'i tıklatın.
    - Bu varlık bot'ın bellek tanımlanırsa, ardından bu eylem olacağı anlamına gelir *değil* kullanılabilir.
2. Eylemler, sonra yeni eylem ikinci bir eylem oluşturmak için tıklayın.
3. Yanıtta ' $city hava büyük olasılıkla güneşli ' yazın.
4. Gerekli varlıklarda ifade edilir beri Şehir varlık otomatik olarak eklendiğini unutmayın.
5. Kaydet’e tıklayın.

Şimdi iki eylemlere sahiptir.

![](../media/tutorial3_actions.PNG)

### <a name="train-the-bot"></a>Bot eğitme

1. Tren iletişim kutuları, ardından yeni tren iletişim'ı tıklatın.
2. 'Hello' yazın.
3. Puan Eylemler'i tıklatın ve 'İstediğiniz hangi Şehir biliyor mu?' seçin
    - Şehir varlık bot'ın bellek tanımlanmadığı Şehir varlık gerekli olduğu yanıt seçilemez unutmayın.
2. 'I istediğiniz hangi Şehir bilmiyorsanız' seçin.
4. 'Seattle' girin. Seattle vurgulayın ve şehir'i tıklatın.
5. Puan Eylemler
    - Not Şehir değeri bot's bellekte sunulmuştur.
    - '$City hava büyük olasılıkla güneşli ' yanıt olarak kullanıma sunulmuştur. 
6. ' $City hava büyük olasılıkla güneşli ' seçin.

', Repeat' kullanıcının girdiği varsayalım. 
1. Türü ve girin. Şehir varlık ve değerini, bellek ve kullanılabilir olduğunu unutmayın.
2. ' $City hava büyük olasılıkla güneşli ' seçin.

![](../media/tutorial3_entities.PNG)

Şimdi bir varlık oluşturduğunuza ve kullanıcı iletileri bunu durumlarda etiketli.  Eylemler eylemin adayını ve gerekli varlıkları alanları aracılığıyla kullanılabilir olduğunda bot'ın bellek denetlemek için de varlık varlığı/yokluğu kullandığınız.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Beklenen varlık](./4-expected-entity.md)
