---
title: Konuşma Öğrenici modeliyle - Microsoft Bilişsel hizmetler varlık kullanma | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici modeliyle varlıkları kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 4b1c2d9390890618a9aa61eb425fbd132917f414
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51229057"
---
# <a name="introduction-to-entities"></a>Varlıklara giriş

Bu öğretici, varlıkları tanıtır ve "Disqualifying varlıkları" ve "varlıkları gerekli" alanları eylemleri nasıl kullanılacağını gösterir.

## <a name="video"></a>Video

[![Öğretici 3 Preview](https://aka.ms/cl-tutorial-03-preview)](https://aka.ms/blis-tutorial-03)

## <a name="requirements"></a>Gereksinimler

Bu öğreticide, genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar

Bu öğreticide varlıklar için iki ortak kullanımı da gösterir.  İlk olarak, varlıklar tanımlayan "Seattle hava durumu nedir?", bir şehir gibi kullanıcı iletisi alt dizeleri ayıklama.  İkinci olarak, Eylemler kullanılabilir olduğunda varlıkları kısıtlayabilirsiniz.  Özellikle, bir eylem bir varlık "gerekli" veya "nitelemesini" olarak listeleyebilirsiniz:
- Bir eylem gerekli varlıkları kullanılabilir olması eylem için sırayla botun bellekte mevcut olması gerekir
- Varlıkları eleyerek gerekir *değil* botun bellek kullanılabilir olması eylem için sırada bulunması

Diğer öğreticiler önceden derlenmiş varlıklar, birden çok değerli gibi varlıkları ve değişkeni yoksayılamaz varlıklar, programlı varlıkları ve kod çağırmanın varlıklarda diğer yönlerini kapsar.

## <a name="steps"></a>Adımlar

### <a name="create-the-model"></a>Modeli oluşturma

1. Web kullanıcı Arabiriminde, yeni bir modele tıklayın
2. IntroToEntities adı girin. Oluştur'a tıklayın.

### <a name="create-entity"></a>Varlık oluşturma

1. Varlıklar ve ardından yeni bir varlık tıklayın.
2. Varlık adı, Şehir girin.
3. Oluştur’a tıklayın

> [!NOTE]
> Varlık türü 'custom'--Bu varlığı eğitilmiş anlamına gelir.  Önceden derlenmiş varlıklar, yani davranışlarını değiştirilemez; bu başka bir öğreticide ele alınmaktadır vardır.

### <a name="create-two-actions"></a>İki eylem oluşturma

1. Eylemler ve ardından yeni bir eylem tıklayın
2. Yanıtta 'İstediğiniz hangi şehirde bilmiyorum' yazın.
3. Eleyerek varlıklarda $city girin. Kaydet’e tıklayın.
    - Bu varlık botun bellekte tanımlanmazsa, ardından bu eylem, yani *değil* kullanılabilir.
2. Eylemler ve ardından yeni bir eylem ikinci bir eylem oluşturmak için tıklayın.
3. Yanıtta 'hava durumu $city, büyük olasılıkla güneşli ' yazın.
4. Gerekli varlıkları için bahsetmiştik beri Şehir varlık otomatik olarak eklendi.
5. Kaydet’e tıklayın.

Artık iki eylem var.

![](../media/tutorial3_actions.PNG)

### <a name="train-the-bot"></a>Botunuzu

1. Train iletişim kutuları, ardından yeni Train iletişim tıklayın.
2. 'Hello' yazın.
3. Puan Eylemler ve 'İstediğiniz hangi şehirde bilmiyorum?' seçin
    - Şehir varlık botun bellekte tanımlanmadığından Şehir varlık gerekli olduğu yanıt seçilemez.
2. 'İstediğiniz hangi şehirde bilmiyorum' seçin.
4. 'Seattle' girin. Seattle vurgulayın ve şehir'i tıklatın.
5. Puan Eylemler
    - Şehir değeri botun bellekte sunulmuştur.
    - 'Hava durumu $city, büyük olasılıkla güneşli ' yanıt olarak kullanıma sunulmuştur. 
6. 'Hava durumu $city, büyük olasılıkla güneşli ' seçin.

'Repeat,' kullanıcının girdiği varsayalım. 
1. Türü ve girin. Şehir varlık ve değerini, bellek ve kullanılabilir.
2. 'Hava durumu $city, büyük olasılıkla güneşli ' seçin.

![](../media/tutorial3_entities.PNG)

Artık bir varlık oluşturduğunuza ve kullanıcı iletileri içerdiği durumlarda etiketli.  Eylemin eleyerek ve gerekli varlıkları alanları eylemler de kullanılabilir olduğunda denetim botun bellekte varlık varlığı/olmaması da kullandınız.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Beklenen varlık](./4-expected-entity.md)
